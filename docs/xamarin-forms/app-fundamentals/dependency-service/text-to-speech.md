---
title: Metinden konuşmaya işlevini uygulama
description: Bu makalede, her platformun yerel metin okuma API'si, çağırmak için Xamarin.Forms DependencyService sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: d39902f2269d3eb241b48831b8eb1b181ff11e7a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997549"
---
# <a name="implementing-text-to-speech"></a>Metinden konuşmaya işlevini uygulama

Kullanan bir platformlar arası uygulama oluştururken bu makalede size yol gösterecek [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) metin okuma yerel API'lere erişmek için:

- **[Arabirimi oluşturma](#Creating_the_Interface)**  &ndash; arabirimi paylaşılan kodda nasıl oluşturulduğunu anlamanız.
- **[iOS uygulaması](#iOS_Implementation)**  &ndash; iOS için yerel kod içinde arabirim uygulamak hakkında bilgi edinin.
- **[Android uygulaması](#Android_Implementation)**  &ndash; arabirimi, Android için yerel koda uygulanması hakkında bilgi edinin.
- **[UWP uygulaması](#WindowsImplementation)**  &ndash; arabirimi, Evrensel Windows Platformu (UWP) için yerel koda uygulanması hakkında bilgi edinin.
- **[Paylaşılan kod içinde uygulama](#Implementing_in_Shared_Code)**  &ndash; nasıl kullanacağınızı öğrenin `DependencyService` paylaşılan kod yerel uygulamasından çağırmak için.

Uygulamayı kullanarak `DependencyService` aşağıdaki yapıya sahip:

![](text-to-speech-images/tts-diagram.png "DependencyService uygulama yapısı")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Arabirimi oluşturma

İlk olarak bir arabirim uygulamak için planlama işlevselliğini ifade paylaşılan kodda oluşturun. Bu örnekte, tek bir yöntem arabirimi içeren `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Paylaşılan kodun bu arabirimde karşı kodlama her platformda konuşma API'leri erişmek Xamarin.Forms uygulaması izin verir.

> [!NOTE]
> Arabirimini uygulayan sınıflar, çalışmak için parametresiz bir oluşturucusu olmalıdır `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS uygulaması

Her platforma özgü uygulama projesinde arabirimi uygulanmalıdır. Sınıfı, parametresiz bir oluşturucu olduğuna dikkat edin böylece `DependencyService` yeni kopyalarını oluşturabilirsiniz.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.iOS
{

    public class TextToSpeechImplementation : ITextToSpeech
    {
        public TextToSpeechImplementation() { }

        public void Speak(string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer();
            var speechUtterance = new AVSpeechUtterance(text)
            {
                Rate = AVSpeechUtterance.MaximumSpeechRate / 4,
                Voice = AVSpeechSynthesisVoice.FromLanguage("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance(speechUtterance);
        }
    }
}
```

`[assembly]` Öznitelik sınıfı uygulaması kaydeder `ITextToSpeech` anlamına arabirimi `DependencyService.Get<ITextToSpeech>()` paylaşılan kodda bir örneğini oluşturmak için kullanılabilir.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android uygulaması

Android kodu iOS sürümünden daha karmaşıktır: Android özel devralacak şekilde uygulayan sınıfa gerektirir `Java.Lang.Object` ve uygulamak için `IOnInitListener` de arabirimi. Tarafından sunulan geçerli Android içeriğine erişim de gerektirir `MainActivity.Instance` özelliği.

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.Droid
{
    public class TextToSpeechImplementation : Java.Lang.Object, ITextToSpeech, TextToSpeech.IOnInitListener
    {
        TextToSpeech speaker;
        string toSpeak;

        public void Speak(string text)
        {
            toSpeak = text;
            if (speaker == null)
            {
                speaker = new TextToSpeech(MainActivity.Instance, this);
            }
            else
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }

        public void OnInit(OperationResult status)
        {
            if (status.Equals(OperationResult.Success))
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }
    }
}
```

`[assembly]` Öznitelik sınıfı uygulaması kaydeder `ITextToSpeech` anlamına arabirimi `DependencyService.Get<ITextToSpeech>()` paylaşılan kodda bir örneğini oluşturmak için kullanılabilir.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Evrensel Windows platformu uygulaması

Konuşma tanıma API'si Evrensel Windows platformu olan `Windows.Media.SpeechSynthesis` ad alanı. Yalnızca uyarı belirliyoruz unutmayın olmaktır **mikrofon** özellik bildiriminde aksi erişim konuşma API'leri engellenir.

```csharp
[assembly:Dependency(typeof(TextToSpeechImplementation))]
public class TextToSpeechImplementation : ITextToSpeech
{
    public async void Speak(string text)
    {
        var mediaElement = new MediaElement();
        var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
        var stream = await synth.SynthesizeTextToStreamAsync(text);

        mediaElement.SetSource(stream, stream.ContentType);
        mediaElement.Play();
    }
}
```

`[assembly]` Öznitelik sınıfı uygulaması kaydeder `ITextToSpeech` anlamına arabirimi `DependencyService.Get<ITextToSpeech>()` paylaşılan kodda bir örneğini oluşturmak için kullanılabilir.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Paylaşılan kod içinde uygulama

Şimdi biz yazın ve metin okuma arabirimi erişen paylaşılan kodu test edin. Bu basit sayfa konuşma işlevi tetikleyen bir düğme içerir. Kullandığı `DependencyService` örneğini almak için `ITextToSpeech` arabirimi &ndash; çalışma zamanında bu örneği yerel SDK tam erişimi olan platforma özgü uygulama olacak.

```csharp
public MainPage ()
{
    var speak = new Button {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    speak.Clicked += (sender, e) => {
        DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
    };
    Content = speak;
}
```

Bu uygulama, iOS, Android ve UWP üzerinde çalışan ve düğmeye basıldığında, her platformda yerel konuşma SDK'sını kullanarak konuşma uygulama sonuçlanır.

 ![iOS ve Android metin okuma düğmesi](text-to-speech-images/running.png "metin okuma örneği")


## <a name="related-links"></a>İlgili bağlantılar

- [DependencyService (örnek) kullanma](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Metin okuma çalışma kitabı](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
