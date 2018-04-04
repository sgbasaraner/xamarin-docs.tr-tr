---
title: HttpClient yığını ve iOS/macOS için SSL/TLS uygulama Seçici
description: SSL/TLS uygulama Seçici ve HttpClient yığını, Xamarin iOS, tvOS veya macOS uygulamanız tarafından kullanılan HttpClient ve SSL/TLS uygulaması belirler.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 06/12/2017
ms.openlocfilehash: ba9eb6a062ce91db5f1597de6f9a2b01ad18a367
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient yığını ve iOS/macOS için SSL/TLS uygulama Seçici

## <a name="httpclient-stack-selector"></a>HttpClient Stack Selector

Xamarin.iOS, Xamarin.tvOS ve Xamarin.Mac için kullanılabilir: Bu denetleyen `HttpClient` uygulaması kullanın. Varsayılan olarak açık bir HttpClient olmaya devam `HttpWebRequest`, artık iOS, tvOS veya macOS yerel taşımaları kullanan bir uygulama için isteğe bağlı olarak geçebilirsiniz (`NSUrlSession` veya `CFNetwork` işletim sistemi bağlı olarak). Baş küçük ikili dosyaları ve daha hızlı indirmeler, dezavantajı olay döngüsünü yürütülecek zaman uyumsuz işlemleri için çalışıyor olmasını gerektirir.

Projeleri başvurmalıdır **System.Net.Http** derleme.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>HttpClient yığın seçme

Uygulamanız tarafından kullanılan HttpClient ayarlamak için:

1. Çift **proje adı** içinde **Çözüm Gezgini** proje Seçenekleri'ni açmak için.
2. Geçiş **yapı** projeniz için ayarları (örneğin, **iOS yapı** bir Xamarin.iOS uygulaması için).
3. Gelen **HttpClient uygulama** açılır, select HttpClient aşağıdakilerden birini yazın: **yönetilen**, **CFNetwork** veya **NSUrlSession**.

[![Yönetilen, CFNetwork veya NSUrlSession HttpClient uygulama seçin](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

<a name="Managed" />

### <a name="managed-default"></a>Yönetilen (varsayılan)

Yönetilen işleyici Xamarin önceki sürümü ile birlikte gelen tam olarak yönetilen HttpClient işleyicidir.

#### <a name="pros"></a>Uzmanları:

 - Microsoft .NET ve Xamarin bir eski sürümü ile uyumlu en özelliğini içeriyor.

#### <a name="cons"></a>Cons:

 - Apple işletim sistemleri ile tamamen tümleşik değildir ve TLS 1.0 sınırlıdır.
 - Onu yerel API'leri genellikle çok daha yavaş şifreleme gibi şeyleri adresindeki.
 - Bu nedenle daha büyük bir uygulama dağıtılabilir oluşturma daha yönetilen kodu gerektirir.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

CFNetwork tabanlı işleyici üzerinde yerel tabanlı `CFNetwork` framework bulunan iOS 6 ve daha yeni.

#### <a name="pros"></a>Uzmanları:

 - Daha iyi performans ve daha küçük yürütülebilir boyutu için yerel API'lerini kullanır.
 - TLS 1.2 gibi daha yeni standartları destekler.

#### <a name="cons"></a>Cons:

 - İOS 6 veya üstünü gerektirir.
 - WatchOS kullanılamaz.
 - Bazı HttpClient özellikleri/seçenekler kullanılamaz.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

NSURLSession tabanlı işleyici üzerinde yerel tabanlı `NSURLSession` framework bulunan iOS 7 ve daha yeni.

#### <a name="pros"></a>Uzmanları:

 - Daha iyi performans ve daha küçük yürütülebilir boyutu için yerel API'lerini kullanır.
 - TLS 1.2 gibi en son standartları destekler.

#### <a name="cons"></a>Cons:

 - İOS 7 veya üzeri gerekir.
 - Bazı HttpClient özellikleri/seçenekler kullanılamaz.

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

## <a name="ssltls-implementation-build"></a>SSL/TLS uygulaması derleme

SSL (Güvenli Yuva Katmanı) ve onun ardıl TLS (Aktarım Katmanı Güvenliği), HTTP ve diğer ağ bağlantıları üzerinden için destek sağlayan `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS veya Xamarin.Mac'ın `System.Net.Security.SslStream` uygulama Mono tarafından sağlanan yönetilen uygulama kullanmak yerine Apple'nın yerel SSL/TLS uygulama çağıracaktır. Apple'nın yerel uygulama, TLS 1.2 destekler.

<a name="Mono" />

> [!WARNING]
> **Mono/yönetilen** TLS sağlayıcısıdır SSL v3 ve TLS v1 sınırlıdır. Bu TLS sağlayıcısı kullanım dışı bırakıldı ve artık Xamarin.iOS uygulamaları için kullanılabilir. 

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
