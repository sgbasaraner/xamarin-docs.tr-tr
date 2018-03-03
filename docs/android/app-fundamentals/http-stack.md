---
title: "HttpClient yığını ve Android için SSL/TLS uygulama Seçici"
description: "HttpClient yığını ve SSL/TLS uygulama seçiciler, Xamarin.Android uygulamaları tarafından kullanılan HttpClient ve SSL/TLS uygulaması belirler."
ms.topic: article
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: bcb6f033c7fad76a17a7a5aa82f48a76b1ae501d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>HttpClient yığını ve Android için SSL/TLS uygulama Seçici

_HttpClient yığını ve SSL/TLS uygulama seçiciler, Xamarin.Android uygulamaları tarafından kullanılan HttpClient ve SSL/TLS uygulaması belirler._

## <a name="overview"></a>Genel Bakış

Xamarin.Android Android uygulaması TLS ayarlarını denetleyen iki birleşik giriş kutuları sağlar. Tek bir birleşik giriş kutusu tanımlar ve hangi `HttpMessageHandler` başlatılırken kullanacağı bir `HttpClient` hangi TLS uygulaması web istekleri tarafından kullanılacak diğer tanımlayan sırasında nesne.

> [!NOTE]
> **Not:** projeleri başvurmalıdır **System.Net.Http** derleme.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

HttpClient yığın ayarlarını bir Xamarin.Android projesi için Proje seçenekleri bulunur. Tıklayın **Android seçenekleri** sekmesini ve ardından tıklatın **Gelişmiş Seçenekler** düğmesi. Bu görüntüler **Gelişmiş Android seçenekleri** biri HttpClient uygulaması diğeri SSL/TLS uygulaması için iki birleşik giriş kutuları olan iletişim:


[ ![Visual Studio Android Options](http-stack-images/tls07-vs-sml.png)](http-stack-images/tls07-vs.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

HttpClient yığın ayarlarını bir Xamarin.Android projesi için Proje seçenekleri bulunur. Tıklayın **Yapı > Android derleme** ayarları ve tıklayarak **genel** sekmesi:

[ ![Visual Studio için Mac Android seçenekleri](http-stack-images/tls07-xs-sml.png)](http-stack-images/tls07-xs.png)


-----

## <a name="httpclient-stack-selector"></a>HttpClient Stack Selector

Bu proje seçeneği denetleyen `HttpMessageHandler` uygulama her zaman kullanılacak bir `HttpClient` nesnesi. Varsayılan olarak, bu yönetilen olduğu `HttpClientHandler`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Visual Studio'da Android HttpClient uygulama birleşik giriş kutusu](http-stack-images/tls04-vs-sml.png)](http-stack-images/tls04-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Mac için Visual Studio'da Android HttpClient uygulama birleşik giriş kutusu](http-stack-images/tls04-xs.png )

-----

### <a name="managed-httpclienthandler"></a>Yönetilen (HttpClientHandler)

Yönetilen işleyici önceki Xamarin.Android sürümleriyle birlikte tam olarak yönetilen HttpClient işleyicidir.

#### <a name="pros"></a>Uzmanları:

- Bu en (Özellikler) ile uyumlu MS .NET ve Xamarin'ın eski sürümlerini olur.

#### <a name="cons"></a>Cons:

- Tam işletim sistemiyle (ör.) ile tümleştirildikten değil sınırlı TLS 1.0).
- (Ör. genellikle çok daha yavaş şifreleme) yerel API daha.
- Büyük uygulamalar oluşturma daha yönetilen kodu gerektirir.

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler her şeyi yönetilen kodda uygulama yerine yerel Java/OS kod temsilciler yeni işleyicidir.

#### <a name="pros"></a>Uzmanları:

- Yerel API daha iyi performans ve daha küçük yürütülebilir boyutu için kullanın.
- En son standartları, örneğin destekler. TLS 1.2.

#### <a name="cons"></a>Cons:

- Android 5.0 veya sonrasını gerektirir.
- Bazı HttpClient özellikleri/seçenekler kullanılamaz.


### <a name="programatically-using-androidclienthandler"></a>Programlı şekilde kullanma `AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler` Olan bir `HttpMessageHandler` Xamarin.Android için özellikle uygulamasıdır. Bu sınıfın örnekleri, yerel kullanacağınız `java.net.URLConnection` tüm HTTP bağlantıları için uygulamasıdır. Bu, teorik olarak HTTP performansı ve daha küçük APK boyutları artış sağlayacaktır.

Bu kod parçacığını nasıl tek bir örneği için açıkça bir örnektir `HttpClient` sınıfı:

```csharp
// Android 5.0 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
>  **Not**: TLS 1.2 (örn. temel alınan Android cihaz desteklemesi gerekir Android 5.0 ve üzeri)


## <a name="ssltls-implementation-build-option"></a>SSL/TLS uygulaması derleme seçeneği

Bu proje seçeneği hangi temel TLS kitaplık tüm web isteği tarafından kullanılacak denetimleri her ikisi de `HttpClient` ve `WebRequest`. Varsayılan olarak, TLS 1.2 seçilir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Visual Studio'da TLS/SSL uygulaması birleşik giriş kutusu](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![Mac için Visual Studio'da TLS/SSL uygulaması birleşik giriş kutusu](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png)

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

Üç seçimleri, önerilen varsayılan bildirmek için Xamarin.Android projesi seçeneklerini kullanmayı yaklaşımdır `HttpMessageHandler` ve TLS tüm uygulama için. Ardından, gerekirse, program aracılığıyla örneği `Xamarin.Android.Net.AndroidClientHandler` nesneleri.
Bu seçenekler yukarıda açıklanan.

Üçüncü seçenek &ndash; ortam değişkenlerini kullanma &ndash; aşağıda açıklanmıştır.

### <a name="declare-environment-variables"></a>Ortam değişkenleri bildirme

Xamarin.Android TLS kullanımı ile ilgili iki ortam değişkenleri şunlardır:

-   `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; Bu ortam değişkenini varsayılan bildirir `HttpMessageHandler` uygulama kullanacak. Örneğin:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

-   `XA_TLS_PROVIDER` &ndash; Bu ortam değişkenini hangi TLS kitaplığı, ya da kullanılacak tanımlayacağınız `btls`, `legacy`, veya `default` (olduğu bu değişken atlanması ile aynı):

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Bu ortam değişkeni ekleyerek ayarlanır bir _ortam dosya_ projeye. Bir yapı eylemi zaman aşımına UNIX olarak biçimlendirilmiş bir düz metin dosyasıyla bir ortam dosyasıdır **AndroidEnvironment**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Visual Studio'da AndroidEnvironment yapı eyleminin ekran görüntüsü.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Ekran görüntüsü AndroidEnvironment yapı Mac için Visual Studio eylemi](http-stack-images/tls03-xs.png)

-----

Lütfen bakın [Xamarin.Android ortam](~/android/deploy-test/environment.md) ortam değişkenleri ve Xamarin.Android hakkında daha fazla ayrıntı için Kılavuzu.


## <a name="related-links"></a>İlgili bağlantılar

- [Aktarım Katmanı Güvenliği (TLS)](~/cross-platform/app-fundamentals/transport-layer-security.md)
