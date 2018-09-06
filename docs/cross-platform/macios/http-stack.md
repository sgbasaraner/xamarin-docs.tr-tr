---
title: HttpClient ve iOS/macOS için SSL/TLS uygulama Seçicisi
description: HttpClient yığını ve SSL/TLS uygulama Seçicisi, Xamarin iOS, tvOS ve macOS uygulamanız tarafından kullanılan HttpClient ve SSL/TLS uygulaması belirler.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: fd48c7148aadd8d156544113e2d719295294bf40
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780666"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>İOS/macOS için HttpClient ve SSL/TLS uygulama Seçicisi

**HttpClient uygulama Seçicisi** Xamarin.iOS için Xamarin.tvOS ve Xamarin.Mac denetimleri, `HttpClient` kullanılacak uygulama. İOS, tvOS ve macOS için yerel taşımalar kullanan bir uygulamaya geçebilirsiniz (`NSUrlSession` veya `CFNetwork`işletim sistemine bağlı olarak). Baş, TLS 1.2 desteği, daha küçük ikili dosyaları olan ve daha hızlı yükler; dezavantajı olay döngüsünü yürütülecek zaman uyumsuz işlemler için çalışıyor olması gerekiyor.

Projeleri başvurmalıdır **System.Net.Http** derleme.

> [!WARNING]
> **Nisan 2018** : Artırılmış Güvenlik nedeniyle PCI uyumluluğu dahil olmak üzere gereksinimleri, birincil bulut sağlayıcıları ve web sunucuları, TLS sürüm 1.2 eski desteğini durduracak şekilde beklenir.  Visual Studio varsayılan olarak TLS eski sürümlerini kullanan önceki sürümlerinde oluşturulan Xamarin projeleri.
>
> Uygulamalarınızı bu sunucuları ve Hizmetleri ile çalışmaya devam sağlamak amacıyla **Xamarin projeleriniz ile güncelleştirmeniz gerekir `NSUrlSession` aşağıda gösterilen, ardından yeniden oluşturun ve uygulamalarınızı yeniden dağıtmak** kullanıcılarınıza.

### <a name="selecting-an-httpclient-stack"></a>HttpClient yığını seçme

Ayarlanacak `HttpClient` uygulamanız tarafından kullanılan:

1. Çift **proje adı** içinde **Çözüm Gezgini** proje Seçenekleri'ni açın.
2. Geçiş **derleme** projeniz için ayarları (örneğin, **iOS derleme** bir Xamarin.iOS uygulaması için).
3. Gelen **HttpClient uygulaması** açılır menüsünde, select `HttpClient` aşağıdakilerden birini yazın: **nsurlsession'ı** (önerilen) **CFNetwork**, veya  **Yönetilen**.

[![HttpClient uygulaması yönetilen, CFNetwork veya nsurlsession'ı seçin.](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> TLS 1.2 desteği `NSUrlSession` seçeneği önerilir.

### <a name="nsurlsession"></a>Nsurlsession'ı

`NSURLSession`-Tabanlı işleyicisi yerel temel `NSURLSession` framework bulunan iOS 7 ve daha yeni. 
**Önerilen ayar budur.**

#### <a name="pros"></a>Uzmanları

- Daha iyi performans ve daha küçük yürütülebilir boyutu için yerel API kullanır.
- TLS 1.2 gibi en son standartları desteği.

#### <a name="cons"></a>Simgeler

- İOS 7 veya üzerini gerektirir.
- Bazı `HttpClient` özellikleri/seçenekleri kullanılamaz.

### <a name="cfnetwork"></a>CFNetwork

`CFNetwork`-Tabanlı işleyicisi yerel temel `CFNetwork` framework bulunan iOS 6 ve daha yeni.

#### <a name="pros"></a>Uzmanları

- Daha iyi performans ve daha küçük yürütülebilir boyutu için yerel API kullanır.
- TLS 1.2 gibi yeni standartları desteği.

#### <a name="cons"></a>Simgeler

- İOS 6 veya sonraki sürümünü gerektirir.
- WatchOS kullanılamaz.
- Bazı HttpClient özellik/seçenekleri kullanılabilir değil.

### <a name="managed"></a>Yönetilen

Yönetilen işleyici, Xamarin önceki bir sürümü ile sunulan tam olarak yönetilen bir HttpClient işleyicisidir.

#### <a name="pros"></a>Uzmanları

- Microsoft .NET ve eski Xamarin sürümleri ile en uyumlu özelliği var.

#### <a name="cons"></a>Simgeler

- Bu Apple işletim sistemleri ile tamamen tümleşik değildir ve TLS 1.0 sınırlıdır. Web sunucularının güvenliğini sağlamak veya gelecekteki bulut hizmetlerine bağlanmak mümkün olmayabilir.
- Bu yerel API genellikle daha yavaş, şifreleme gibi şeyler.
- Bu nedenle daha büyük bir uygulamanın dağıtılabilir oluşturma daha yönetilen kod gerektirir.

### <a name="programmatically-setting-the-httpmessagehandler"></a>HttpMessageHandler programlı olarak ayarlama

Proje genelinde yapılandırmaya ek olarak, yukarıda gösterilen başlatabilir bir `HttpClient` ve istenen ekleme `HttpMessageHandler` Bu kod parçacıklarında gösterildiği gibi Oluşturucusu aracılığıyla:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Bu farklı bir kullanmayı mümkün kılar `HttpMessageHandler` bölümünde bildirilen gelen **proje seçenekleri** iletişim.

## <a name="ssltls-implementation"></a>SSL/TLS uygulaması

SSL (Güvenli Yuva Katmanı) ve onun ardılı olan TLS (Aktarım Katmanı Güvenliği), HTTP ve diğer ağ bağlantıları üzerinden için destek sağlayan `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS veya Xamarin.Mac'ın `System.Net.Security.SslStream` uygulama Mono tarafından sağlanan yönetilen bir uygulamasını kullanmak yerine Apple'nın yerel SSL/TLS uygulaması çağıracaktır. Apple'nın yerel uygulama, TLS 1.2 destekler.

> [!WARNING]
> Yaklaşan Xamarin.Mac 4.8 sürümü yalnızca macOS 10.9 veya sonraki sürümleri destekleyecektir.
> Xamarin.Mac’in önceki sürümleri macOS 10.7 veya sonraki sürümleri destekler ancak bu eski macOS sürümleri TLS 1.2’yi destekleyecek yeterli TLS altyapısını barındırmaz. macOS 10.7 veya macOS 10.8’i hedeflemek için Xamarin.Mac 4.6 veya önceki sürümleri kullanın.

## <a name="app-transport-security"></a>Uygulama aktarım güvenliği

Apple'nın _uygulama taşıma güvenliği_ (ATS) uygulamanız ile internet kaynakları (örneğin, uygulamanın arka uç sunucu) arasında güvenli bağlantılar zorlar. ATS böylece uygulamanız ya da onu kullanan bir kitaplık aracılığıyla doğrudan hassas bilgilerin yanlışlıkla açığa engelleyen tüm internet iletişimi güvenli bağlantı, en iyi uyan sağlar.

ATS iOS 9, tvOS 9 ve OS X 10.11 (El Capitan) için oluşturulan uygulamalarda varsayılan olarak etkin olduğundan ve daha yeni sürümü, tüm bağlantıları kullanarak `NSUrlConnection`, `CFUrl` veya `NSUrlSession` ATS güvenlik gereksinimlerine tabi olacaktır. Bağlantılarınızı bu gereksinimleri karşılamıyorsa, bir özel durum ile başarısız olur.

HttpClient yığını ve SSL/TLS uygulaması seçimlerinize bağlı olarak, uygulamanızı ATS ile düzgün çalışması için değişiklik yapmanız gerekebilir.

ATS hakkında daha fazla bilgi için bkz. bizim [uygulama taşıma Güvenliği Kılavuzu](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Bilinen sorunlar

Bu bölümde, Xamarin.iOS TLS desteği ile ilgili bilinen sorunlar ele alınacaktır.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Proje yükleme "istenen değer AppleTLS bulunamadı" hatası ile başarısız oldu.

Xamarin.iOS 9.8 sunulan yer alan bazı yeni ayarları **.csproj** dosyası için bir Xamarin.iOS uygulaması. Bu değişiklikler, projeyi Xamarin.iOS eski sürümleriyle açıldığında sorunlara neden olabilir. Aşağıdaki ekran görüntüsünde, bu senaryoda görüntülenen hata iletisini örneğidir:

![Ekran görüntüsü projesi yüklenmeye çalışılırken bir hatayla istenen değer eski bulunamadı](http-stack-images/tlserror-xs.png)

Giriş bu hataya `MtouchTlsProvider` Xamarin.iOS 9.8 proje dosyasında ayarı. Xamarin.iOS 9.8 için güncelleştirilecek olası (veya üzeri) değilse, yaklaşık el ile düzenlemek için çalışmadır **.csproj** dosya uygulama, kaldırma `MtouchTlsprovider` öğesi ve değiştirilen proje dosyasını kaydedin.

Aşağıdaki kod parçacığını bir örnek olduğundan `MtouchTlsProvider` ayarı görünebileceğini içinde ister bir **.csproj** dosyası:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>İlgili bağlantılar

- [Aktarım Katmanı Güvenliği (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [Uygulama Aktarım Güvenliği](~/ios/app-fundamentals/ats.md)
