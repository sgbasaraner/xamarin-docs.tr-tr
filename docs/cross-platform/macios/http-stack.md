---
title: HttpClient yığını ve iOS/macOS için SSL/TLS uygulama Seçici
description: SSL/TLS uygulama Seçici ve HttpClient yığını, Xamarin iOS, tvOS veya macOS uygulamanız tarafından kullanılan HttpClient ve SSL/TLS uygulaması belirler.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: dcbdb4d20bca9764731b08e551a4d3b8a26a2ab4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient yığını ve iOS/macOS için SSL/TLS uygulama Seçici

**HttpClient uygulama Seçici** Xamarin.iOS için Xamarin.tvOS ve Xamarin.Mac denetimleri, `HttpClient` uygulaması kullanın. İOS, tvOS veya macOS yerel taşımaları kullanan bir uygulama için geçiş yapabilirsiniz (`NSUrlSession` veya `CFNetwork`, işletim sistemine bağlı olarak). Baş TLS 1.2 desteği, daha küçük ikili olduğu ve daha hızlı indirmeler; dezavantajı yürütülecek zaman uyumsuz işlemleri için çalışıyor olması için olay döngüsünü gerektirmesidir.

Projeleri başvurmalıdır **System.Net.Http** derleme.

> [!WARNING]
> **Nisan, 2018** – Artırılmış Güvenlik nedeniyle PCI uyumluluğunu içeren gereksinimlerini birincil Bulutu sağlayıcıları ve web sunucuları, TLS 1.2 eski sürümleri desteklenmesini durdurmak için beklenir.  TLS eski sürümleri kullanmak için Visual Studio varsayılan önceki sürümlerinde oluşturulan Xamarin projeleri.
>
> Uygulamalarınızı bu sunucuları ve Hizmetleri ile çalışmaya devam emin olmak için **Xamarin projelerinizi güncelleştirmeniz gerekir `NSUrlSession` aşağıda gösterilen, ardından yeniden derleme ayarlayıp uygulamalarınızı yeniden dağıtma** kullanıcılarınıza.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>HttpClient yığın seçme

Ayarlamak için `HttpClient` uygulamanız tarafından kullanılan:

1. Çift **proje adı** içinde **Çözüm Gezgini** proje Seçenekleri'ni açmak için.
2. Geçiş **yapı** projeniz için ayarları (örneğin, **iOS yapı** bir Xamarin.iOS uygulaması için).
3. Gelen **HttpClient uygulama** açılan listesinde, select `HttpClient` aşağıdakilerden birini yazın: **NSUrlSession** (önerilen), **CFNetwork**, veya  **Yönetilen**.

[![Yönetilen, CFNetwork veya NSUrlSession HttpClient uygulama seçin](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> TLS 1.2 desteği için `NSUrlSession` seçeneği önerilir.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

`NSURLSession`-Tabanlı işleyici üzerinde yerel temel `NSURLSession` framework bulunan iOS 7 ve daha yeni. 
**Önerilen ayar budur.**

#### <a name="pros"></a>Uzmanları

- Daha iyi performans ve daha küçük yürütülebilir boyutu için yerel API'lerini kullanır.
- TLS 1.2 gibi en son standartları desteği.

#### <a name="cons"></a>Simgeler

- İOS 7 veya üzeri gerekir.
- Bazı `HttpClient` özellikleri/seçenekleri kullanılamaz.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

`CFNetwork`-Tabanlı işleyici üzerinde yerel temel `CFNetwork` framework bulunan iOS 6 ve daha yeni.

#### <a name="pros"></a>Uzmanları

- Daha iyi performans ve daha küçük yürütülebilir boyutu için yerel API'lerini kullanır.
- TLS 1.2 gibi daha yeni standartları desteği.

#### <a name="cons"></a>Simgeler

- İOS 6 veya üstünü gerektirir.
- WatchOS kullanılamaz.
- Bazı HttpClient özellikleri/seçenekler kullanılamaz.

<a name="Managed" />

### <a name="managed"></a>Yönetilen

Yönetilen işleyici Xamarin önceki sürümü ile birlikte gelen tam olarak yönetilen HttpClient işleyicidir.

#### <a name="pros"></a>Uzmanları

- Microsoft .NET ve Xamarin bir eski sürümü ile uyumlu en özelliğini içeriyor.

#### <a name="cons"></a>Simgeler

- Apple işletim sistemleri ile tamamen tümleşik değildir ve TLS 1.0 sınırlıdır. Güvenli web sunucularına veya gelecekte bulut hizmetlerine bağlanmak mümkün olmayabilir.
- Onu yerel API'leri genellikle çok daha yavaş şifreleme gibi şeyleri adresindeki.
- Bu nedenle daha büyük bir uygulama dağıtılabilir oluşturma daha yönetilen kodu gerektirir.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Program aracılığıyla HttpMessageHandler ayarlama

Yukarıda gösterilen proje çapındaki yapılandırmanın yanı sıra, aynı zamanda oluşturabileceğiniz bir `HttpClient` ve istenen Ekle `HttpMessageHandler` Bu kod parçacıkları gösterildiği gibi Oluşturucusu aracılığıyla:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Bu farklı bir kullanmayı mümkün kılar `HttpMessageHandler` içinde bildirilen gelen **proje seçenekleri** iletişim.

<a name="New-SSL-TLS-implementation-build-option" />
<a name="Selecting-a-SSL-TLS-implementation" />
<a name="Apple-TLS" />

## <a name="ssltls-implementation"></a>SSL/TLS uygulama

SSL (Güvenli Yuva Katmanı) ve onun ardıl TLS (Aktarım Katmanı Güvenliği), HTTP ve diğer ağ bağlantıları üzerinden için destek sağlayan `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS veya Xamarin.Mac'ın `System.Net.Security.SslStream` uygulama Mono tarafından sağlanan yönetilen uygulama kullanmak yerine Apple'nın yerel SSL/TLS uygulama çağıracaktır. Apple'nın yerel uygulama, TLS 1.2 destekler.

<a name="App-Transport-Security" />

## <a name="app-transport-security"></a>Uygulama taşıma güvenliği

Apple'nın _uygulama taşıma güvenliği_ (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulamanız arasındaki güvenli bağlantılar zorlar. ATS tüm Internet iletişimlerini güvenli bağlantı en iyi uygulamalar için uygun böylece uygulamanız veya bunu kullanan bir kitaplığı ile doğrudan hassas bilgilerin yanlışlıkla açığa önleme sağlar.

Varsayılan olarak iOS 9, tvOS 9 ve OS X 10.11 sürümünü (El Capitan) için oluşturulan uygulamaların ATS etkin olduğundan ve daha yeni kullanan tüm bağlantıların `NSUrlConnection`, `CFUrl` veya `NSUrlSession` ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantılarınızı bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.

HttpClient yığını ve SSL/TLS uygulama seçimlerinize bağlı olarak, uygulamanızı ATS ile düzgün çalışması için yapılan değişiklikler yapmanız gerekebilir.

ATS hakkında daha fazla bilgi için lütfen bkz bizim [uygulama aktarım Güvenlik Kılavuzu](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Bilinen Sorunlar

Bu bölümde Xamarin.iOS TLS desteği ile ilgili bilinen sorunlar ele alınacaktır.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Proje yükleme "istenen değer AppleTLS bulunamadı" hatası ile başarısız oldu.

Xamarin.iOS 9.8 sunulan yer alan bazı yeni ayarları **.csproj** bir Xamarin.iOS uygulaması için dosya. Bu değişiklikler, eski sürümleri Xamarin.iOS projesi açıldığında sorunlara neden olabilir. Aşağıdaki ekran görüntüsünde, bu senaryoda görüntülenen hata iletisini örneğidir:

![Ekran görüntüsü projesi yüklemeye çalışırken hata oluştu. İstenen değer eski bulunamadı](http-stack-images/tlserror-xs.png)

Giriş tarafından bu hataya `MtouchTlsProvider` Xamarin.iOS 9.8 proje dosyasında ayarlama. Xamarin.iOS 9.8 güncelleştirmek olası (veya üstü) değilse, yaklaşık el ile düzenlemek için bir iştir **.csproj** dosya uygulama, kaldırma `MtouchTlsprovider` öğesi ve değiştirilen proje dosyasını kaydedin.

Aşağıdaki kod parçacığını bir örnek olduğundan `MtouchTlsProvider` ayarı konum içinde ister bir **.csproj** dosyası:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>İlgili bağlantılar

- [Aktarım Katmanı Güvenliği (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [Uygulama Aktarım Güvenliği](~/ios/app-fundamentals/ats.md)
