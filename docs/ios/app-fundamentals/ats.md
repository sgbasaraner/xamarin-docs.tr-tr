---
title: Uygulama taşıma güvenliği
description: Uygulama taşıma güvenliği (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulamanız arasındaki güvenli bağlantılar zorlar.
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 7e3a191def7e0c06365f334b4a7708e5927eadf8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="app-transport-security"></a>Uygulama taşıma güvenliği

_Uygulama taşıma güvenliği (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulamanız arasındaki güvenli bağlantılar zorlar._

Bu makalede, bir iOS 9 uygulama üzerinde uygulama taşıma güvenliği zorlar güvenlik değişiklikleri getirir ve [Xamarin.iOS projeleriniz için bu gelir](#xamarinsupport), ele alınacaktır [ATS yapılandırma seçenekleri](#config) ve ele alınacaktır nasıl [çevirin ATS,](#optout) gerekirse ATS. ATS varsayılan olarak etkinleştirilmiş olduğundan, güvenli olmayan herhangi bir internet bağlantısı (siz açıkça, izin verilen sürece) bir özel durum iOS 9 uygulamalarda ortaya koyar.


## <a name="about-app-transport-security"></a>Uygulama taşıma güvenliği hakkında

Yukarıda belirtildiği gibi ATS iOS 9 ve OS X El Capitan tüm internet iletişimlerini güvenli bağlantı en iyi uygulamalar için uygun böylece uygulamanız veya bunun bir kitaplık aracılığıyla doğrudan ya da hassas bilgilerin yanlışlıkla açığa önleme sağlar kullanma.

Var olan uygulamalar için uygulama `HTTPS` mümkün olduğunca protokol. Yeni Xamarin.iOS uygulamaları için kullanmanız gereken `HTTPS` özel olarak Internet kaynakların ile iletişim kurarken. Ayrıca, üst düzey API iletişimini TLS sürüm 1.2 iletme gizliliği ile kullanılarak şifrelenmesi gerekir.

İle yapılan herhangi bir bağlantı [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) veya [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) ATS varsayılan uygulamaları iOS 9 ve OS X 10.11 sürümünü (El Capitan) için yerleşik olarak kullanır.

## <a name="default-ats-behavior"></a>Varsayılan ATS davranışı

Varsayılan olarak tüm bağlantılar kullanarak iOS 9 ve OS X 10.11 sürümünü (El Capitan) için oluşturulan uygulamaların ATS etkin olduğundan [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) veya [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) tabi olacaktır ATS güvenlik gereksinimleri. Bağlantılarınızı bu gereksinimi karşılamıyorsa, bir özel durum ile başarısız olur.

### <a name="ats-connection-requirements"></a>ATS bağlantı gereksinimleri

ATS tüm internet bağlantıları için aşağıdaki gereksinimleri zorunlu kılacak:

- Tüm bağlantı şifrelemeleri iletme gizliliği kullanıyor olmanız gerekir. Kabul edilen şifrelemeleri aşağıdaki listeye bakın.
- Aktarım Katmanı Güvenliği (TLS) Protokolü sürüm 1.2 veya daha büyük olmalıdır.
- En az bir SHA256 parmak izi 2048 bit veya daha büyük bir RSA anahtarı veya 256 bit veya büyük Elliptic eğri (ECC) anahtar ile tüm sertifikalar için kullanılması gerekir.

Yeniden ATS iOS 9 varsayılan olarak etkin olduğundan, bu gereksinimleri karşılamıyor bir bağlantı kurmayı her türlü girişim oluşturulan bir özel durum neden olur. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>ATS uyumlu şifre

Aşağıdaki iletme gizliliği şifre türü Internet iletişimlerini güvenli ATS tarafından kabul edilir:

- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

Apple iOS internet iletişimi sınıfları ile çalışma hakkında daha fazla bilgi için lütfen bkz [NSURLConnection sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [CFURL başvuru](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) veya [NSURLSession sınıf başvurusu ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Xamarin.iOS içinde ATS destekleme

Xamarin.iOS uygulamanızı veya kitaplığı veya onu kullanarak hizmet bağlantı Internet'e yaparsa ATS iOS 9 ve OS X El Capitan, varsayılan olarak etkin olduğundan, bazı işlemler yapması gerekir veya bağlantılarınızı oluşturulan bir özel durum neden olur.

Var olan bir uygulama için Apple desteklediğiniz öneren `HTTPS` mümkün olan en kısa sürede protokol. Desteği olmayan web hizmeti bir 3. taraf bağlandığınız olmadığından ya da yapılamıyor, `HTTPS` veya destekleniyorsa `HTTPS` pratik olacaktır, size ATS çevirin. Bkz: [Opting-ATS dışı](#optout) daha fazla ayrıntı için bölüm aşağıda.

Yeni bir Xamarin.iOS uygulaması için kullanmanız gereken `HTTPS` özel olarak Internet kaynakların ile iletişim kurarken. Yeniden, (3 taraf web hizmetini kullanarak gibi) durumlar olabilir burada bu mümkün değildir ve ATS çevirin sağlamanız gerekir.

Ayrıca, ATS TLS sürüm 1.2 ile iletme gizliliği kullanılarak şifrelenmesi için üst düzey API iletişimi zorlar. Bkz: [ATS bağlantı gereksinimleri](#ATS-Connection-Requirements) ve [ATS uyumlu şifre](#ATS-Compatible-Ciphers) daha fazla ayrıntı için bölümler yukarıda.

TLS ile tanıdık olmayabilir ancak ([Aktarım Katmanı Güvenliği](https://en.wikipedia.org/wiki/Transport_Layer_Security)), SSL devamıdır ([Güvenli Yuva Katmanı](https://en.wikipedia.org/wiki/Transport_Layer_Security)) ve şifreleme protokolleri üzerinden güvenlik zorlamak için oluşan bir koleksiyon sağlar ağ bağlantıları.

TLS düzeyi, kullanan web hizmeti tarafından kontrol edilir ve bu nedenle uygulamanın denetimi dışında. Hem `HttpClient` ve `ModernHttpClient` TLS şifrelemesi sunucu tarafından desteklenen en üst düzey otomatik olarak kullanmanız gerekir.

Sunucuda bağlı olarak (özellikle 3 taraf hizmet ise) için varsayılır, devre dışı bırakmanız gerekebilir gizliliği iletmek veya daha düşük bir TLS düzeyini seçin. Bkz: [ATS seçenekleri yapılandırma](#Configuring-ATS-Options) daha fazla ayrıntı için bölüm aşağıda.

> [!IMPORTANT]
> Uygulama taşıma güvenliği kullanarak Xamarin uygulamaları için geçerli olmayan **yönetilen HTTPClient uygulamalarına**. CFNetwork kullanarak bağlantılara uygulanır **HTTPClient uygulamalarına** veya **NSURLSession HTTPClient uygulamalarına** yalnızca.

### <a name="setting-the-httpclient-implementation"></a>HTTPClient uygulama ayarlama

Bir iOS uygulamanız tarafından kullanılan HTTPClient uygulama ayarlamak için **proje** içinde **Çözüm Gezgini** açmak için **proje seçenekleri**. Gidin **iOS yapı** ve istenen istemci türü'nün altında seçin **HttpClient uygulama** açılır:

![](ats-images/client01.png "İOS derleme seçenekleri ayarlama")


#### <a name="managed-handler"></a>Yönetilen işleyici

Yönetilen işleyici, varsayılan işleyici Xamarin.iOS önceki sürümleri ile birlikte gelen ve tam olarak yönetilen HttpClient işleyicidir.

Uzmanları:

- En eski sürümü Xamarin ve Microsoft .NET ile uyumludur.

Cons:

- Bu tam olarak iOS (örneğin, TLS 1.0 ile sınırlıdır) tümleşiktir değil.
- Yerel API genellikle çok daha yavaşlatabilir.
- Daha fazla yönetilen kod gerektirir ve büyük uygulamalar oluşturur.

#### <a name="cfnetwork-handler"></a>CFNetwork işleyicisi

Temel CFNetwork işleyici üzerinde yerel tabanlı `CFNetwork` framework.

Uzmanları:

- Yerel API daha iyi performans ve daha küçük yürütülebilir boyutları için kullanır.
- TLS 1.2 gibi daha yeni standartları desteği ekler.

Cons:

- İOS 6 veya üstünü gerektirir.
- WatchOS, mevcut değil.
- Bazı HttpClient özellikleri ve seçenekleri kullanılamaz.

#### <a name="nsurlsession-handler"></a>NSUrlSession işleyicisi

Temel NSUrlSession işleyici üzerinde yerel tabanlı `NSUrlSession` API.

Uzmanları:

- Yerel API daha iyi performans ve daha küçük yürütülebilir boyutları için kullanır.
- TLS 1.2 gibi daha yeni standartları desteği ekler.

Cons:

- İOS 7 veya üzeri gerekir.
- Bazı HttpClient özellikleri ve seçenekleri kullanılamaz. 

## <a name="diagnosing-ats-issues"></a>ATS sorunlarını tanılama

Doğrudan veya bir web görünümü iOS 9'internet ' e bağlanmaya çalışılırken bir hata biçiminde alabilirsiniz:

> Uygulama taşıma güvenliği doğrulamaya HTTP engelledi (http://www.-the-blocked-domain.com) , güvenli olduğundan kaynak yükü. Geçici özel durumlar, uygulamanızın Info.plist dosyası aracılığıyla yapılandırılabilir.

İOS9 içinde uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulamanız arasındaki güvenli bağlantılar zorlar. Ayrıca, ATS iletişimi kullanılması `HTTPS` protokolü ve TLS sürüm 1.2 ile iletme gizliliği kullanılarak şifrelenmesi için üst düzey API iletişimi.

Varsayılan olarak tüm bağlantılar kullanarak iOS 9 ve OS X 10.11 sürümünü (El Capitan) için oluşturulan uygulamaların ATS etkin olduğundan `NSURLConnection`, `CFURL` veya `NSURLSession` ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantılarınızı bu gereksinimi karşılamıyorsa, bir özel durum ile başarısız olur.

Apple de sağlar [TLSTool örnek uygulaması](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) , derlenmesi (veya isteğe bağlı olarak kod çevrimi Xamarin ve C#) ve ATS/TLS sorunları tanılamak için kullanılır. Lütfen bakın [Opting-ATS dışı](#optout) bölümünde aşağıda bu sorunu çözmek hakkında bilgi için.


<a name="config" />

## <a name="configuring-ats-options"></a>ATS seçeneklerini yapılandırma

Uygulamanızın içinde özel anahtarları için değerlerini ayarlayarak birkaç ATS özelliklerini yapılandırabilirsiniz **Info.plist** dosya. Aşağıdaki anahtarları ATS denetlemek için kullanılabilir (_nasıl içe göstermek için girintili_):

```csharp
NSAppTransportSecurity
    NSAllowsArbitraryLoads
    NSAllowsArbitraryLoadsInWebContent
    NSExceptionDomains
    <domain-name-for-exception-as-string>
        NSExceptionMinimumTLSVersion
        NSExceptionRequiresForwardSecrecy
        NSExceptionAllowsInsecureHTTPLoads
        NSRequiresCertificateTransparency
        NSIncludesSubdomains
        NSThirdPartyExceptionMinimumTLSVersion
        NSThirdPartyExceptionRequiresForwardSecrecy
        NSThirdPartyExceptionAllowsInsecureHTTPLoads
```

Her anahtar aşağıdaki türü ve anlamı vardır:

- **NSAppTransportSecurity** (`Dictionary`) - tüm ayar anahtarlarını içerir ve değerleri için ATS.
- **NSAllowsArbitraryLoads** (`Boolean`) - `YES` ATS devre dışı bırakılacak için herhangi bir etki alanı **değil** listelenen `NSExceptionDomains`. Listelenen etki alanları için belirlediğiniz güvenlik ayarları kullanılır.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) - If `YES` will allow web pages to load correctly while Apple Transport Security (ATS) protection is still enabled for the rest of the app.
- **NSExceptionDomains** (`Dictionary`)-etki alanı topluluğu ve ATS belirli bir etki alanı için kullanması gereken güvenlik ayarları.
- **< Domain-name-for-exception-as-string >** (`Dictionary`)-belirli bir etki alanının (ör özel durum koleksiyonu. `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`)-en az TLS sürümü olarak ya da `TLSv1.0`, `TLSv1.1` veya `TLSv1.2` (varsayılan olmayan).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) - `NO` etki alanı, bir şifre ile ileri güvenliği kullanmak zorunda kalmazsınız. Varsayılan değer `YES` şeklindedir.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) - `NO` bu etki alanı ile tüm iletişimler olmalıdır (varsayılan) `HTTPS` protokolü.
- **NSRequiresCertificateTransparency** (`Boolean`) - `YES` geçerli saydamlık veri etki alanının Güvenli Yuva Katmanı (SSL) içermelidir. Varsayılan değer `NO` şeklindedir.
- **NSIncludesSubdomains** (`Boolean`) - `YES` bu etki alanının tüm alt etki alanları bu ayarları geçersiz kılar. Varsayılan değer `NO` şeklindedir.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-TLS sürümü etki alanı 3 taraf hizmeti Geliştirici denetimi dışında olduğunda kullanılır.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) - If `YES` a 3rd party domain requires forward secrecy.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) - If `YES` the ATS will allow non-secure communication with 3rd party domains.

<a name="optout" />

### <a name="opting-out-of-ats"></a>ATS kullanmama-genişletme

Apple, yüksek oranda kullanılmasını önerir sırada `HTTPS` protokolü ve güvenli iletişim için Internet tabanlı bilgileri, bu her zaman mümkün olmayan zamanlar olabilir. Örneğin, bir 3 taraf web hizmetiyle iletişim kurulurken veya reklam uygulamanızda teslim Internet kullanıyorsanız.

Xamarin.iOS uygulamanızı güvensiz bir etki alanı için bir istek yapmanız gerekiyorsa, uygulamanızın aşağıdaki değişiklikler **Info.plist** dosya belirli bir etki alanı için ATS zorlar güvenlik Varsayılanları devre dışı bırakır:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>www.the-domain-name.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.0</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <false/>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

Mac için Visual Studio içinde çift `Info.plist` dosyasını **Çözüm Gezgini**, geçiş **kaynak** görüntülemek ve yukarıdaki anahtarları ekleyin:

[![](ats-images/ats01.png "Info.plist dosyasının kaynağı görünümü")](ats-images/ats01.png#lightbox)


Uygulamanızı yüklemek ve güvenli olmayan sitelerinden web içeriğini görüntülemek gerekirse, aşağıdaki, uygulamanızın ekleyin **Info.plist** dosya web sayfalarının Apple Aktarım güvenlik (ATS) koruması için rest etkinken doğru bir şekilde yüklemeye izin ver Uygulama:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

İsteğe bağlı olarak, uygulamanızın aşağıdaki değişiklikleri yapabilirsiniz **Info.plist** dosya tamamen ATS tüm etki alanları ve internet iletişimi için devre dışı bırakmak için:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

Mac için Visual Studio içinde çift `Info.plist` dosyasını **Çözüm Gezgini**, geçiş **kaynak** görüntülemek ve yukarıdaki anahtarları ekleyin:

[![](ats-images/ats02.png "Info.plist dosyasının kaynağı görünümü")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> Uygulamanızı bir bağlantı güvenli olmayan bir Web sitesine gerektiriyorsa, aşağıdakileri yapmalısınız **her zaman** kullanarak özel durum olarak etki alanını girin `NSExceptionDomains` tamamen kullanarak devre dışı ATS kapatma yerine `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` yalnızca aşırı acil durumlarda kullanılmalıdır.




Yeniden ATS devre dışı bırakılması gereken _yalnızca_ kullanılabilir son çare olarak, güvenli bağlantı geçiş kullanılamıyor veya pratik ise.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makale, uygulama Aktarım güvenlik (ATS) sunulan ve internet ile güvenli iletişim zorlar şekilde açıklanmıştır. İlk olarak, iOS 9 çalıştıran bir Xamarin.iOS uygulaması için ATS gerektiren değişiklikler ele. Ardından denetleme ATS özellikleri ve seçenekleri ele. Son olarak, ATS dışında Xamarin.iOS uygulamanızı kullanmama ele.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
