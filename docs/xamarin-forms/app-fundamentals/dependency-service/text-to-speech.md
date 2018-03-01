---
title: Metin okuma uygulama
description: "Her platformun yerel metin okuma API çağırmak için DependencyService kullanın"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: c5bd753aaa3a9e807a03a9fb05b233cfa39d0dc3
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="implementing-text-to-speech"></a>Metin okuma uygulama

Kullanan bir platformlar arası uygulamayı oluştururken bu makale size yol gösterecektir [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) yerel metin okuma API'leri erişmek için:

- **[Arabirimi oluşturma](#Creating_the_Interface)**  &ndash; arabirimi paylaşılan kodda nasıl oluşturulduğunu anlayın.
- **[iOS uygulaması](#iOS_Implementation)**  &ndash; iOS için yerel kodda arabirimini uygulayan öğrenin.
- **[Android uygulaması](#Android_Implementation)**  &ndash; arabirimini yerel kodda Android için uygulama öğrenin.
- **[Windows uygulaması](#WindowsImplementation)**  &ndash; Windows Phone ve evrensel Windows Platformu (UWP) için yerel kodda arabirimini uygulayan öğrenin.
- **[Paylaşılan kod içinde uygulama](#Implementing_in_Shared_Code)**  &ndash; nasıl kullanacağınızı öğrenin `DependencyService` paylaşılan koddan yerel uygulama çağırmak için.

Uygulamayı kullanarak `DependencyService` aşağıdaki yapı ayarlanmıştır:

![](text-to-speech-images/tts-diagram.png "DependencyService uygulama yapısı")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Arabirimi oluşturma

İlk olarak, bir arabirim uygulamak için planlama işlevselliği ifade paylaşılan kod içinde oluşturun. Bu örnekte, tek bir yöntem arabirimi içeren `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

Bu arabirim paylaşılan kodda karşı kodlama Xamarin.Forms uygulaması her platformda API'leri konuşma erişmesine izin verir.

> [!NOTE]
> **Not**: arabirimini uygulayan sınıflar çalışmak için parametresiz bir oluşturucusu olmalıdır `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS uygulaması

Arabirim her platforma özgü uygulama projesinde uygulanmalıdır. Sınıf parametresiz bir oluşturucuya sahip Not böylece `DependencyService` yeni örnekleri oluşturabilirsiniz.

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

Android kod iOS sürümden daha karmaşıktır: Android özgü devralacak şekilde uygulayan sınıfa gerektirir `Java.Lang.Object` ve uygulamak için `IOnInitListener` de arabirimi.

Xamarin.Forms de sağlar `Forms.Context` örneği geçerli Android bağlam nesnesi. Bu, çok sayıda Android SDK yöntemleri çağırma yeteneği olan iyi bir örnek gerektirir `StartActivity()`.

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
                speaker = new TextToSpeech(Forms.Context, this);
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

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone ve evrensel Windows Platform uygulaması

Windows Phone ve evrensel Windows platformu, konuşma API sahip `Windows.Media.SpeechSynthesis` ad alanı. Yalnızca uyarısıyla belirliyoruz anımsaması olan **mikrofon** yetenek bildiriminde aksi API'leri engellendiğini konuşma erişim.

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

Şimdi biz yazma ve metin okuma arabirimi erişen paylaşılan kod sınayın. Bu basit sayfa konuşma işlevi tetikleyen bir düğme içerir. Kullandığı `DependencyService` örneğini almak için `ITextToSpeech` arabirimi &ndash; çalışma zamanında bu örneği yerel SDK tam erişime sahip platforma özgü uygulaması olacaktır.

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

İOS, Android veya Windows platformları ve düğmesi, her platformda yerel konuşma SDK kullanarak konuşma uygulamanın neden olacak tuşlarına basarak bu uygulamayı çalıştırıyor.

 ![iOS ve Android metin okuma düğmesi](text-to-speech-images/running.png "metin okuma örnek")


## <a name="related-links"></a>İlgili bağlantılar

- [DependencyService (örnek) kullanma](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Metin okuma çalışma kitabı](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
