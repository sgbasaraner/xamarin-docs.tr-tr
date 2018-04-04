---
title: Aktarım Katmanı Güvenliği (TLS)
description: Android, iOS ve Mac Xamarin projeleri için TLS 1.2 etkinleştirme
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/10/2017
ms.openlocfilehash: 8b2d0288248f2468e6976ad4f7c46255690116c0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="transport-layer-security-tls"></a>Aktarım Katmanı Güvenliği (TLS)

_Android, iOS ve Mac Xamarin projeleri için TLS 1.2 etkinleştirme_

En son sürümünü kullanarak [ _Aktarım Katmanı Güvenliği_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) uygulama ağ iletişimleri güvenli olduğundan emin olmak önemlidir.

> [!NOTE]
> Xamarin serbest itibaren [Şubat 2017](https://releases.xamarin.com/stable-release-cycle-9/) TLS 1.2 yeni projelerinde varsayılan olarak kullanın.

TLS 1.2 desteği artık kullanılabilir:

* Mono 4.8 (içeren [TLS 1.2 Destek](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support))
* Xamarin.iOS
* Xamarin.Mac
* Xamarin.Android (Android 5.0 veya daha yeni gerektirir)

Projeleri başvurmalıdır **System.Net.Http** derleme. 

## <a name="updating-to-tls-12"></a>TLS 1.2 güncelleştiriliyor

Bu bölümde, ağ Xamarin projelerinde yapılandırma seçeneklerinin bazılarını güncelleştirebilmek açıklanmaktadır, _varolan_ daha güvenli bir protokol yararlanmak için uygulamalar.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu ayarları bulunabilir **proje Seçenekleri > Android seçenekleri** ve ardından tıklayarak **Gelişmiş** düğmesi: 

[![Visual Studio'da HttpClient ve TLS yapılandırma](transport-layer-security-images/properties-vs-sml.png)](transport-layer-security-images/properties-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
Bu ayarları bulunabilir **proje özellikleri > derleme seçenekleri > Gelişmiş** sekmesi:

[![HttpClient ve TLS Mac için Xamarin Studio ve Visual Studio'yu yapılandırma](transport-layer-security-images/properties-xs-sml.png)](transport-layer-security-images/properties-xs.png#lightbox)

-----


### <a name="httpclient-implementation"></a>HttpClient uygulama

Xamarin geliştiriciler her zaman kendi kodda yerel ağ iletişimi sınıfları kullanmaya devam edebilir, ancak Ayrıca hangi ağ yığınını belirleyen bir seçeneği kullanıldığında tarafından `HttpClient` sınıfları. Bu yerel platformun hızı ve güvenlik avantajları vardır tanıdık bir .NET API sağlar.

Seçenekler şunlardır:

- **Yönetilen yığın** – Mono tarafından sağlanan ağ işlevselliğini veya
- **Yerel yığın** – çeşitli API'ler temel platformlar (Android, iOS veya macOS) tarafından sağlanan ağ.

Ancak daha yavaş ve yürütülebilir daha büyük boyutta neden Yönetilen yığın en üst düzey varolan .NET kodu ile uyumluluk sağlar.

Yerel seçenekleri daha hızlı ve daha iyi güvenlik (TLS 1.2 dahil) sahip, ancak tüm işlevleri ve seçenekleri sağlamayabilir `HttpClient` sınıfı.


### <a name="ssltls-implementation"></a>SSL/TLS uygulama

Proje seçenekleri de desteklemek için hangi SSL/TLS uygulama seçmenize olanak tanır:

- **Mono/yönetilen** – Android TLS 1.1, TLS 1.0 iOS ve macOS.
- **Yerel** – hem Android, iOS ve macOS TLS 1.2.

TLS (tüm projeleri için önerilir) 1.2 destekleyen yerel uygulama için yeni Xamarin projeleri varsayılan olarak, uyumluluk açısından gerekli ancak, yönetilen koda geçebilirsiniz.

> [!IMPORTANT]
> **Mono/yönetilen** seçeneği kaldırılacak bir [gelecekteki sürüm](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/).
>
> Yerel seçeneği önerilir.

## <a name="platform-specific-details"></a>Platforma özgü ayrıntıları

Yukarıdaki Özet Xamarin projelerinde HttpClient ve SSL/TLS uygulaması proje düzeyi ayarları açıklanmaktadır. İos'ta aralarından seçim yapabileceğiniz yerel iki seçenek vardır ve HttpClient uygulama kodunda da bir dinamik olarak ayarlanabilir.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS ve Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>Özet

Uygulamalar, mümkün olduğunda Aktarım Katmanı Güvenliği (TLS) 1.2 kullanmalıdır.
Yeni uygulamalar artık bu yapılandırma için varsayılan olarak ancak, bu makaledeki yönergeleri göre varolan uygulamalarında ayarları güncelleştirmeniz gerekebilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Uygulama Aktarım Güvenliği](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin döngüsü 9 (Şubat 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8 sürüm notları - TLS 1.2 desteği](http://www.mono-project.com/docs/about-monohttps://developer.xamarin.com/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler ve açıklandığı WebRequestHandler](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP istemcisi (örnek)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
