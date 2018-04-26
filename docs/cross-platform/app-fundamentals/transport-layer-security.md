---
title: Aktarım Katmanı Güvenliği (TLS) 1.2
description: Android, iOS ve Mac Xamarin projeleri için TLS 1.2 etkinleştirme
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 6205e8633ccdd2c1e568e7de8103c38eb9edbc2f
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="transport-layer-security-tls-12"></a>Aktarım Katmanı Güvenliği (TLS) 1.2

En son sürümünü kullanarak [ _Aktarım Katmanı Güvenliği_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) uygulama ağ iletişimleri güvenli olduğundan emin olmak önemlidir.

> [!WARNING]
> **Nisan, 2018** – Artırılmış Güvenlik nedeniyle PCI uyumluluğunu içeren gereksinimlerini birincil Bulutu sağlayıcıları ve web sunucuları, TLS 1.2 eski sürümleri desteklenmesini durdurmak için beklenir.  TLS eski sürümleri kullanmak için Visual Studio varsayılan önceki sürümlerinde oluşturulan Xamarin projeleri.
>
> Uygulamalarınızı bu sunucuları ve Hizmetleri ile çalışmaya devam emin olmak için **aşağıdaki ayarları kullanın sonra yeniden oluşturun ve uygulamalarınızı yeniden dağıtmak için Xamarin projelerinizi güncelleştirmeniz gerekir** kullanıcılarınıza.

Projeleri başvurmalıdır **System.Net.Http** derleme ve aşağıda gösterildiği gibi yapılandırılması.

## <a name="update-android-to-tls-12"></a>TLS 1.2 Android güncelleştir

Güncelleştirme **HttpClient uygulama** ve **SSL/TLS uygulama** TLS 1.2 güvenliği etkinleştirmek için Seçenekler.

> [!NOTE]
> Android 5.0 veya daha yeni gerektirir.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Bu ayarları bulunabilir **proje özellikleri > Android seçenekleri** ve ardından tıklayarak **Gelişmiş** düğmesi:

[![Visual Studio'da HttpClient ve TLS yapılandırma](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

Bu ayarları bulunabilir **proje Seçenekleri > Yapı > Android derleme** sekmesi:

[![Visual Studio'da HttpClient ve TLS Mac için yapılandırın](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-ios-to-tls-12"></a>İOS için TLS 1.2 güncelleştir

Güncelleştirme **HttpClient uygulama** TLS 1.2 güvenlik etkinleştirmek için seçeneği.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Bu ayar bulunabilir **proje özellikleri > iOS yapı**:

[![Visual Studio'da HttpClient ve TLS yapılandırma](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

Bu ayar bulunabilir **proje Seçenekleri > Yapı > iOS yapı** sekmesi:

[![Mac için Visual Studio'da HttpClient yapılandırın](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-macos-to-tls-12-in-visual-studio-for-mac"></a>Mac için TLS 1.2 Visual Studio için macOS güncelleştirin

Güncelleştirme **HttpClient uygulama** seçeneğini **proje Seçenekleri > Yapı > Mac yapı** sekmesini TLS 1.2 güvenliği etkinleştirmek için:

[![Mac için Visual Studio'da HttpClient yapılandırın](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

## <a name="alternative-configuration-options"></a>Alternatif yapılandırma seçenekleri

Bu bölümde alternatifleri yukarıda gösterilen TLS 1.2 desteklenen yapılandırmalar açıklanmaktadır.
TLS destek farklı düzeylerde kullanarak riskleri anlarsanız, uygulama geliştiricilerinin yalnızca bu alternatifleri göz önünde bulundurmalısınız.

### <a name="httpclient-implementation"></a>HttpClient uygulama

Xamarin geliştiriciler her zaman kendi kodda yerel ağ iletişimi sınıfları kullanmaya devam edebilir, ancak Ayrıca hangi ağ yığınını belirleyen bir seçeneği kullanıldığında tarafından `HttpClient` sınıfları. Bu yerel platformun hızı ve güvenlik avantajları vardır tanıdık bir .NET API sağlar.

Seçenekler şunlardır:

- **Yönetilen yığın** – Mono tarafından sağlanan ağ işlevselliğini veya
- **Yerel yığın** – çeşitli API'ler temel platformlar (Android, iOS veya macOS) tarafından sağlanan ağ.

Ancak daha yavaş ve yürütülebilir daha büyük boyutta neden Yönetilen yığın en üst düzey varolan .NET kodu ile uyumluluk sağlar.

Yerel seçenekleri daha hızlı ve daha iyi güvenlik (TLS 1.2 dahil) sahip, ancak tüm işlevleri ve seçenekleri sağlamayabilir `HttpClient` sınıfı.

### <a name="ssltls-implementation-android"></a>SSL/TLS uygulaması (Android)

Android proje seçenekleri de desteklemek için hangi SSL/TLS uygulama seçmenize olanak tanır:

- **Mono ve yönetilen** – android'de TLS 1.1
- **Yerel** – TLS 1.2 android'de.

TLS (tüm projeleri için önerilir) 1.2 destekleyen yerel uygulama için yeni Xamarin projeleri varsayılan olarak, uyumluluk açısından gerekli ancak, yönetilen koda geçebilirsiniz.

> [!IMPORTANT]
> **Mono/yönetilen** seçeneği süredir [iOS ve Mac kaldırılan](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) proje seçenekleri.
>
> Yerel seçeneği, iOS ve Mac platformları üzerinde her zaman kullanılır.

## <a name="platform-specific-details"></a>Platforma özgü ayrıntıları

Yukarıdaki Özet Xamarin projelerinde HttpClient ve SSL/TLS uygulaması proje düzeyi ayarları açıklanmaktadır. HttpClient uygulama kodunda da bir dinamik olarak ayarlanabilir. Daha fazla bilgi için bu platforma özgü kılavuzlara bakın:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS ve Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>Özet

Uygulamalar, mümkün olduğunda Aktarım Katmanı Güvenliği (TLS) 1.2 kullanmalıdır.
Bu makaledeki yönergeleri göre varolan uygulamalarında ayarları güncelleştirme daha sonra yeniden oluşturun ve müşterilerinize yeniden dağıtma gerekir.

## <a name="related-links"></a>İlgili bağlantılar

- [Uygulama Aktarım Güvenliği](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android ortamı](~/android/deploy-test/environment.md)
- [Xamarin döngüsü 9 (Şubat 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8 sürüm notları - TLS 1.2 desteği](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler ve açıklandığı WebRequestHandler](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP istemcisi (örnek)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
