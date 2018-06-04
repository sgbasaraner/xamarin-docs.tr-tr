---
title: DependencyService giriş
description: Yerel platform özelliklere erişim DependencyService nasıl çalıştığını anlamak
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 6a0544a58245efed696b92c543fbaf5e7e1b8365
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732833"
---
# <a name="introduction-to-dependencyservice"></a>DependencyService giriş

## <a name="overview"></a>Genel Bakış

[`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) platforma özgü işlevsellik paylaşılan kodundan çağıran uygulamalarının sağlar. Bu işlev Xamarin.Forms uygulamaları yerel uygulama yapmak için herhangi bir şey yapmanızı sağlar.

`DependencyService` bağımlılık çözümleyici olur. Uygulamada, bir arabirim tanımlanır ve `DependencyService` çeşitli platform projeleri arabirimden doğru uyarlamasını bulur.

## <a name="how-dependencyservice-works"></a>DependencyService nasıl çalışır?

Xamarin.Forms uygulamalarına ihtiyacı kullanmak için dört bileşenleri `DependencyService`:

- **Arabirim** &ndash; gerekli işlevselliği paylaşılan kodda bir arabirim tarafından tanımlanır.
- **Uygulama başına Platform** &ndash; arabirimini uygulayan sınıflar her platform projeye eklenmelidir.
- **Kayıt** &ndash; her uygulayan sınıfa ile kaydedilmelidir `DependencyService` aracılığıyla bir meta veri özniteliği. Kayıt etkinleştirir `DependencyService` uygulayan sınıfa bulmak ve çalışma zamanında yerine arabirimi sağlayın.
- **Çağrı için DependencyService** &ndash; açıkça çağırmak için kodu gereksinimlerini paylaşılan `DependencyService` arabirim uygulamaları için isteyebilir.

Uygulamaları her bir platform projesinin çözümünüzdeki sağlanmalıdır unutmayın. Uygulamaları olmadan Platform projeleri çalışma zamanında başarısız olur.

Aşağıdaki diyagram tarafından uygulama yapısını açıklanmıştır:

![](introduction-images/overview-diagram.png "DependencyService uygulama yapısı")

### <a name="interface"></a>Arabirim

Tasarım arabirimi platforma özgü işlevselliği ile nasıl etkileşim tanımlayacaksınız. Bir bileşeni ya da Nuget paketi paylaşılan bileşeni geliştiriyorsanız dikkatli olun. API tasarımı yapın veya bir paket bölün. Aşağıdaki örnek söylenir sözcükleri belirtme esneklik sağlar, ancak her platform için özelleştirilmek üzere uygulama bırakır metni söylemeyi için basit bir arabirim belirtir:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Uygulama platformu başına

Uygun bir arabirim tasarlanmış sonra bu arabirim, hedeflediğiniz her platform için projedeki uygulanmalıdır. Örneğin, aşağıdaki uygulayan sınıf `ITextToSpeech` iOS arabiriminde:

```csharp
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

### <a name="registration"></a>Kayıt

Her uygulama arabirimi ile kayıtlı olması gerekiyor `DependencyService` meta verileri özniteliğine sahip. Aşağıdaki kod, iOS için uygulama kaydeder:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

Tüm bir araya getirilmesi, platforma özgü uygulama şöyle görünür:

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

Not:, kayıt sınıf düzeyinde değil ad alanı düzeyinde gerçekleştirilir.

#### <a name="universal-windows-platform-net-native-compilation"></a>Evrensel Windows platformu .NET yerel derleme

.NET yerel derleme seçeneğini kullanan UWP projeleri izlemelidir bir [biraz farklı yapılandırma](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) Xamarin.Forms başlatırken. .NET yerel derleme biraz farklı kayıt bağımlılık Hizmetleri için de gerektirir.

İçinde **App.xaml.cs** dosya, UWP projesini kullanarak tanımlanan her bağımlılık hizmeti el ile kaydetmeniz `Register<T>` aşağıda gösterildiği gibi yöntemi:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Not: el ile kayıt kullanarak `Register<T>` yalnızca geçerli sürümdeki .NET yerel derleme kullanarak yer almaktadır. Bu satırı atlarsanız, hata ayıklama yapıları çalışmaya devam eder, ancak sürüm derlemeler bağımlılık hizmetini yükleme başarısız olur.

### <a name="call-to-dependencyservice"></a>İçin DependencyService çağırın

Ortak bir arabirim ve her platform için uygulamaları ile proje ayarlandığına sonra kullanarak `DependencyService` doğru uygulama çalışma zamanında almak için:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` arabiriminin doğru uygulamasını bulacaksınız `T`.

### <a name="solution-structure"></a>Çözüm yapısı

[UsingDependencyService çözüm örnek](https://developer.xamarin.com/samples/UsingDependencyService/) iOS ve Android için aşağıda gösterilen, yukarıda özetlenen kodda değişiklik vurgulanır.

 [![iOS ve Android çözümü](introduction-images/solution-sml.png "DependencyService örnek çözümü yapısı")](introduction-images/solution.png#lightbox "DependencyService örnek çözümü yapısı")

> [!NOTE]
> **Gerekir** bir platform projelerdeki belirtin. Arabirim uygulaması kayıtlı değilse, sonra `DependencyService` çözümleyemez olacaktır `Get<T>()` çalışma zamanında yöntemi.


## <a name="related-links"></a>İlgili bağlantılar

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
