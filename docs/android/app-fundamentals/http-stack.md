---
title: HttpClient yığını ve Android için SSL/TLS uygulama Seçici
description: HttpClient yığını ve SSL/TLS uygulama seçiciler, Xamarin.Android uygulamaları tarafından kullanılan HttpClient ve SSL/TLS uygulaması belirler.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/20/2018
ms.openlocfilehash: 765c51346ac63a00838fec52bde87b38091e2dd9
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689480"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>HttpClient yığını ve Android için SSL/TLS uygulama Seçici

HttpClient yığını ve SSL/TLS uygulama seçiciler, Xamarin.Android uygulamaları tarafından kullanılan HttpClient ve SSL/TLS uygulaması belirler.

Projeleri başvurmalıdır **System.Net.Http** derleme.

> [!WARNING]
> **Nisan, 2018** – Artırılmış Güvenlik nedeniyle PCI uyumluluğunu içeren gereksinimlerini birincil Bulutu sağlayıcıları ve web sunucuları, TLS 1.2 eski sürümleri desteklenmesini durdurmak için beklenir.  TLS eski sürümleri kullanmak için Visual Studio varsayılan önceki sürümlerinde oluşturulan Xamarin projeleri.
>
> Uygulamalarınızı bu sunucuları ve Hizmetleri ile çalışmaya devam emin olmak için **Xamarin projelerinizi güncelleştirmeniz gerekir `Android HttpClient` ve `Native TLS 1.2` aşağıda gösterilen ayarları daha sonra yeniden oluşturun ve uygulamalarınızı yeniden dağıtma** için Kullanıcılar.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin.Android HttpClient yapılandırma bulunduğu **proje Seçenekleri > Android seçenekleri**, ardından **Gelişmiş Seçenekler** düğmesi.

TLS 1.2 desteği için önerilen ayarları şunlardır:

[![Visual Studio Android seçenekleri](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)


# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

Xamarin.Android HttpClient yapılandırma bulunduğu **proje Seçenekleri > Yapı > Android derleme** ayarları ve tıklatın **genel** sekmesi.

TLS 1.2 desteği için önerilen ayarları şunlardır:

[![Visual Studio için Mac Android seçenekleri](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>Alternatif yapılandırma seçenekleri

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler her şeyi yönetilen kodda uygulama yerine yerel Java/OS kod temsilciler yeni işleyicidir.
**Önerilen seçenek budur.**

#### <a name="pros"></a>Uzmanları

- Yerel API daha iyi performans ve daha küçük yürütülebilir boyutu için kullanın.
- En son standartları, örneğin destekler. TLS 1. 2.

#### <a name="cons"></a>Simgeler

- Android 5.0 veya sonrasını gerektirir.
- Bazı HttpClient özellikleri/seçenekler kullanılamaz.

### <a name="managed-httpclienthandler"></a>Yönetilen (HttpClientHandler)

Yönetilen işleyici önceki Xamarin.Android sürümleriyle birlikte tam olarak yönetilen HttpClient işleyicidir.

#### <a name="pros"></a>Uzmanları

- Bu en (Özellikler) ile uyumlu MS .NET ve Xamarin'ın eski sürümlerini olur.

#### <a name="cons"></a>Simgeler

- Tam işletim sistemiyle (ör.) ile tümleştirildikten değil sınırlı TLS 1.0).
- (Ör. genellikle çok daha yavaş şifreleme) yerel API daha.
- Büyük uygulamalar oluşturma daha yönetilen kodu gerektirir.



### <a name="choosing-a-handler"></a>Bir işleyici seçme

Arasında seçim `AndroidClientHandler` ve `HttpClientHandler` sonra uygulamanızın gereksinimlerine bağlıdır. `AndroidClientHandler` en güncel güvenlik desteği ör önerilir.

-   TLS 1.2 + desteği gerektirir.
-   Uygulamanızı Android 5.0 (API 21) hedefleme veya sonraki bir sürümü.
-   TLS 1.2 + gereksinim duyduğunuz desteği `HttpClient`.
-   Gerekmeyen TLS 1.2 + desteği için `WebClient`.

`HttpClientHandler` TLS 1.2 + gerekiyorsa iyi bir seçimdir destekler ancak Android Android 5.0'den önceki sürümleri desteklemesi gerekir. TLS 1.2 + ihtiyacınız varsa onu da iyi bir seçimdir desteği `WebClient`.

Xamarin.Android 8.3 ile başlayan `HttpClientHandler` Boring SSL varsayılanlara (`btls`) temel alınan TLS sağlayıcısı. SSL TLS Boring sağlayıcısı aşağıdaki avantajları sunar:

-   TLS 1.2 + destekler.
-   Tüm Android sürümlerini destekler.
-   TLS 1.2 + her ikisi için destek sağlar `HttpClient` ve `WebClient`.

Alt TLS sağlayıcı Boring SSL kullanmanın olumsuz yanı (yaklaşık 1 MB desteklenen ABI başına ek APK boyut ekler) sonuçta elde edilen APK boyutunu artırabilirsiniz ' dir.

Xamarin.Android 8.3 ile başlayarak, varsayılan TLS Boring SSL sağlayıcısıdır (`btls`). Boring SSL kullanmak istemiyorsanız ayarlayarak geçmiş yönetilen SSL uygulaması döndürebilirsiniz `$(AndroidTlsProvider)` özelliğine `legacy` (derleme özelliklerini ayarlama hakkında daha fazla bilgi için bkz: [derleme işlemi](~/android/deploy-test/building-apps/build-process.md)).


### <a name="programatically-using-androidclienthandler"></a>Programlı şekilde kullanma `AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler` Olan bir `HttpMessageHandler` Xamarin.Android için özellikle uygulamasıdır.
Bu sınıfın örnekleri, yerel kullanacağınız `java.net.URLConnection` tüm HTTP bağlantıları için uygulamasıdır. Bu, teorik olarak HTTP performansı ve daha küçük APK boyutları artış sağlayacaktır.

Bu kod parçacığını nasıl tek bir örneği için açıkça bir örnektir `HttpClient` sınıfı:

```csharp
// Android 5.0 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> Temel alınan Android cihaz (örn. TLS 1.2 desteklemesi gerekir Android 5.0 ve üzeri)


## <a name="ssltls-implementation-build-option"></a>SSL/TLS uygulaması derleme seçeneği

Bu proje seçeneği hangi temel TLS kitaplık tüm web isteği tarafından kullanılacak denetimleri her ikisi de `HttpClient` ve `WebRequest`. Varsayılan olarak, TLS 1.2 seçilir:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio'da TLS/SSL uygulaması birleşik giriş kutusu](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

[![Mac için Visual Studio'da TLS/SSL uygulaması birleşik giriş kutusu](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

Örneğin:

```csharp
var client = new HttpClient();
```

HttpClient uygulaması ayarlandıysa **yönetilen** ve TLS uygulama ayarlandı **yerel TLS 1.2 +**, sonra `client` nesne otomatik olarak kullandığınız yönetilen `HttpClientHandler` ve TLS (BoringSSL kitaplığı tarafından sağlanan) 1.2, HTTP isteklerini.

Ancak, varsa **HttpClient uygulama** ayarlanır `AndroidHttpClient`, ardından tüm `HttpClient` nesneleri temel alınan Java sınıfı kullanacak `java.net.URLConnection` ve tarafından etkilenmeyecek **TLS/SSL uygulaması** değeri. `WebRequest` nesneleri BoringSSL kitaplığı kullanırsınız.

## <a name="other-ways-to-control-ssltls-configuration"></a>SSL/TLS yapılandırmasını kontrol etmenin diğer yolları

Bir Xamarin.Android uygulaması TLS ayarları denetleyebilir üç yolu vardır:

1. HttpClient uygulama ve varsayılan TLS kitaplığı proje seçenekleri seçin.
2. Programlı şekilde kullanarak `Xamarin.Android.Net.AndroidClientHandler`.
3. Ortam değişkenlerini (isteğe bağlı) bildirin.

Üç seçimleri, önerilen varsayılan bildirmek için Xamarin.Android projesi seçeneklerini kullanmayı yaklaşımdır `HttpMessageHandler` ve TLS tüm uygulama için. Ardından, gerekirse, program aracılığıyla örneği `Xamarin.Android.Net.AndroidClientHandler` nesneleri. Bu seçenekler yukarıda açıklanan.

Üçüncü seçenek &ndash; ortam değişkenlerini kullanma &ndash; aşağıda açıklanmıştır.

### <a name="declare-environment-variables"></a>Ortam değişkenleri bildirme

Xamarin.Android TLS kullanımı ile ilgili iki ortam değişkenleri şunlardır:

- `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; Bu ortam değişkenini varsayılan bildirir `HttpMessageHandler` uygulama kullanacak. Örneğin:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- `XA_TLS_PROVIDER` &ndash; Bu ortam değişkenini hangi TLS kitaplığı, ya da kullanılacak tanımlayacağınız `btls`, `legacy`, veya `default` (olduğu bu değişken atlanması ile aynı):

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Bu ortam değişkeni ekleyerek ayarlanır bir _ortam dosya_ projeye. Bir yapı eylemi zaman aşımına UNIX olarak biçimlendirilmiş bir düz metin dosyasıyla bir ortam dosyasıdır **AndroidEnvironment**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio'da AndroidEnvironment yapı eyleminin ekran görüntüsü.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

![Ekran görüntüsü AndroidEnvironment yapı Mac için Visual Studio eylemi](http-stack-images/tls03-xs.png)

-----

Lütfen bakın [Xamarin.Android ortam](~/android/deploy-test/environment.md) ortam değişkenleri ve Xamarin.Android hakkında daha fazla ayrıntı için Kılavuzu.


## <a name="related-links"></a>İlgili bağlantılar

- [Aktarım Katmanı Güvenliği (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
