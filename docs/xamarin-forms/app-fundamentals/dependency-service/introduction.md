---
title: DependencyService giriş
description: Bu makalede, Xamarin.Forms DependencyService sınıfı yerel platform özelliklere erişim için nasıl çalıştığı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/15/2018
ms.openlocfilehash: 28c6daa361b7de09a0d9332b21f1b6f75e035850
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "38995420"
---
# <a name="introduction-to-dependencyservice"></a>DependencyService giriş

## <a name="overview"></a>Genel Bakış

[`DependencyService`](xref:Xamarin.Forms.DependencyService) Paylaşılan kod platforma özgü işlevlerini çağırmak uygulamalar sağlar. Xamarin.Forms uygulamaları, yerel bir uygulama yapabileceği bir şey yapmak bu işlevi etkinleştirir.

`DependencyService` hizmet bulucu olur. Uygulamada, bir arabirim tanımlanır ve `DependencyService` doğru uygulama çeşitli platform projelerindeki o arabirimin bulur.

> [!NOTE]
> Varsayılan olarak, [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) parametresiz oluşturucular sahip platform uygulamalarını tek bir Çözümle olur. Bununla birlikte, bağımlılığı çözümleme yöntemi platform uygulamalarını çözümlemek için bir bağımlılık ekleme kapsayıcısını veya Fabrika yöntemleri kullanan Xamarin.Forms yerleştirilebilir. Bu yaklaşım, parametreli oluşturuculara sahip platform uygulamalarını gidermek için kullanılabilir. Daha fazla bilgi için [bağımlılık çözümlemesi xamarin.Forms'taki](~/xamarin-forms/internals/dependency-resolution.md).

## <a name="how-dependencyservice-works"></a>DependencyService nasıl çalışır?

Xamarin.Forms uygulamalarına ihtiyacı kullanmak için dört bileşen `DependencyService`:

- **Arabirimi** &ndash; gerekli işlevselliği bir arabirimde bir paylaşılan kod tarafından tanımlanır.
- **Uygulama başına Platform** &ndash; arabirimi uygulayan sınıflar her platform projeye eklenmesi gerekiyor.
- **Kayıt** &ndash; her uygulayan bir sınıfa ile kaydedilmelidir `DependencyService` aracılığıyla meta veri özniteliği. Kayıt etkinleştirir `DependencyService` uygulayan sınıfa bulup çalışma zamanında arabirim yerine sağlayın.
- **Çağırmak için DependencyService** &ndash; açıkça çağırmak için Kod gereksinimlerini paylaşılan `DependencyService` arabirim uygulamaları için isteyebilir.

Uygulamaları her platform projesi çözümünüzdeki sağlanmalıdır unutmayın. Platformu projelerinde uygulamaları olmadan, çalışma zamanında başarısız olur.

Aşağıdaki diyagram tarafından uygulama yapısı açıklanmıştır:

![](introduction-images/overview-diagram.png "DependencyService uygulama yapısı")

### <a name="interface"></a>Arabirim

Tasarım arabirimi platforma özgü işlevselliği ile etkileşimini tanımlar. Bir bileşen veya Nuget paketi olarak paylaşılan bir bileşen geliştiriyorsanız dikkatli olun. API tasarımı değişiklik yapabilir veya bir paket bölün. Aşağıdaki örnekte söylenir sözcükler belirterek esneklik sağlar, ancak her platform için özelleştirilmiş uygulama bırakır metni konuşma için basit bir arabirim belirtir:

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>Platform başına uygulama

Uygun arabirimi tasarlanmış sonra bu arabirim, hedeflediğiniz her platform için projedeki uygulanmalıdır. Örneğin, aşağıdaki uygulayan sınıf `ITextToSpeech` iOS arabiriminde:

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

Hepsi bir araya getirildiğinde, platforma özgü uygulama şöyle görünür:

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

.NET yerel derleme seçeneğini kullanan UWP projeleri izlemelidir bir [biraz farklı yapılandırma](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) Xamarin.Forms başlatılırken. .NET yerel derleme, aynı zamanda bağımlılık Hizmetleri için biraz daha farklı bir kayıt gerektirir.

İçinde **App.xaml.cs** dosya, UWP projesini kullanarak tanımlanan her bağımlılık hizmeti elle kaydetmeniz `Register<T>` aşağıda gösterildiği gibi yöntemi:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

Not: el ile kayıt kullanarak `Register<T>` yalnızca geçerli sürümdeki .NET Native derlemesi kullanarak yer almaktadır. Bu satırı atlarsanız, hata ayıklama yapıları çalışmaya devam eder, ancak sürüm derlemeleri bağımlılık hizmetini yükleme başarısız olur.

### <a name="call-to-dependencyservice"></a>İçin DependencyService çağırın

Ortak bir arabirim ve her platform için uygulamalar ile proje kurulduktan sonra kullanmak `DependencyService` doğru uygulama çalışma zamanında almak için:

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` doğru uygulama arabiriminin bulabilirsiniz `T`.

### <a name="solution-structure"></a>Çözüm yapısı

[UsingDependencyService çözüm örnek](https://developer.xamarin.com/samples/UsingDependencyService/) iOS ve Android için aşağıda gösterilen, yukarıda özetlenen kod değişiklikleriyle vurgulanır.

 [![iOS ve Android çözüm](introduction-images/solution-sml.png "DependencyService örnek Çözüm yapısı")](introduction-images/solution.png#lightbox "DependencyService örnek Çözüm yapısı")

> [!NOTE]
> **Gerekir** her platform projesinde bir uygulanmasını sağlar. Arabirim uygulaması kayıtlıysa, ardından `DependencyService` çözmek mümkün olmayacaktır `Get<T>()` zamanında yöntemi.

## <a name="related-links"></a>İlgili bağlantılar

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
