---
title: Aktarım Katmanı Güvenliği (TLS) 1.2
description: Bu belge nasıl Xamarin.iOS ve Xamarin.Android Xamarin.Mac projeleri için TLS 1.2 etkinleştirmek için. Mac için Visual Studio 2017 ve Visual Studio ile bunu nasıl gösterir
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 88cbce6dbfee4e7aa1a0711d6da74f6f12abd4b7
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780591"
---
# <a name="transport-layer-security-tls-12"></a>Aktarım Katmanı Güvenliği (TLS) 1.2

En son sürümünü kullanarak [ _Aktarım Katmanı Güvenliği_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) uygulama ağ iletişimlerini güvenli olduğundan emin olmak önemlidir.

> [!WARNING]
> **Nisan 2018** : Artırılmış Güvenlik nedeniyle PCI uyumluluğu dahil olmak üzere gereksinimleri, birincil bulut sağlayıcıları ve web sunucuları, TLS sürüm 1.2 eski desteğini durduracak şekilde beklenir.  Visual Studio varsayılan olarak TLS eski sürümlerini kullanan önceki sürümlerinde oluşturulan Xamarin projeleri.
>
> Uygulamalarınızı bu sunucuları ve Hizmetleri ile çalışmaya devam sağlamak amacıyla **aşağıdaki ayarları kullanın, ardından yeniden oluşturun ve uygulamalarınızı yeniden dağıtmak için Xamarin projeleriniz güncelleştirmelisiniz** kullanıcılarınıza.

Projeleri başvurmalıdır **System.Net.Http** derleme ve aşağıda gösterildiği gibi yapılandırılması.

## <a name="update-xamarinandroid-to-tls-12"></a>TLS 1.2 için Xamarin.Android güncelleştirmesi

Güncelleştirme **HttpClient uygulaması** ve **SSL/TLS uygulaması** TLS 1.2 güvenlik seçeneğini etkinleştirmek için Seçenekler.

> [!NOTE]
> Android 5.0 ve üzeri sürümlerde gerektirir.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Bu ayarlar bulunabilir **proje özellikleri > Android seçenekleri** ve ardından tıklayarak **Gelişmiş** düğmesi:

[![Visual Studio'da HttpClient ve TLS yapılandırma](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

Bu ayarlar bulunabilir **proje Seçenekleri > derleme > Android derleme** sekmesinde:

[![HttpClient ve TLS Mac için Visual Studio'da yapılandırma](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Xamarin.iOS için TLS 1.2 güncelleştir

Güncelleştirme **HttpClient uygulaması** TLS 1.2 güvenlik seçeneğini etkinleştirmek için seçenek.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Bu ayar bulunabilir **proje özellikleri > iOS Derlemesi'ne**:

[![Visual Studio'da HttpClient ve TLS yapılandırma](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

Bu ayar bulunabilir **proje Seçenekleri > derleme > iOS Derlemesi'ne** sekmesinde:

[![Mac için Visual Studio'da HttpClient yapılandırma](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>TLS 1.2 Xamarin.mac'e güncelleştirme

TLS 1.2 bir Xamarin.Mac uygulamasını etkinleştirmek için Mac için Visual Studio'daki güncelleştirme **HttpClient uygulaması** seçeneğini **proje Seçenekleri > derleme > Mac derleme**:

[![Mac için Visual Studio'da HttpClient yapılandırma](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> Yaklaşan Xamarin.Mac 4.8 sürümü yalnızca macOS 10.9 veya sonraki sürümleri destekleyecektir.
> Xamarin.Mac’in önceki sürümleri macOS 10.7 veya sonraki sürümleri destekler ancak bu eski macOS sürümleri TLS 1.2’yi destekleyecek yeterli TLS altyapısını barındırmaz. macOS 10.7 veya macOS 10.8’i hedeflemek için Xamarin.Mac 4.6 veya önceki sürümleri kullanın.

## <a name="alternative-configuration-options"></a>Alternatif yapılandırma seçenekleri

Bu bölümde alternatifleri yukarıda gösterilen TLS 1.2 desteklenen yapılandırmalar açıklanmaktadır.
Uygulama geliştiricileri bu alternatifleri TLS destek düzeyleri farklı kullanmanın risklerini anladıkları yalnızca düşünmelisiniz.

### <a name="httpclient-implementation"></a>HttpClient uygulaması

Xamarin geliştiricilerine her zaman kodlarını yerel ağ iletişimi sınıfları kullanmanız mümkün olmuştur, ancak Ayrıca hangi ağ yığınını belirleyen bir seçeneği kullanıldığında tarafından `HttpClient` sınıfları. Bu, yerel platform hız ve güvenlik avantajlarından sahip tanıdık bir .NET API sağlar.

Seçenekler şunlardır:

- **Yönetilen yığın** – Mono tarafından sağlanan ağ işlevleri veya
- **Yerel yığın** – çeşitli ağ bağlantılı platformların (Android, iOS veya macOS) tarafından sağlanan API'leri.

Ancak daha yavaş ve daha büyük yürütülebilir boyutta neden Yönetilen yığın ile mevcut .NET kodu uyumluluğu en üst düzey sağlar.

Yerel seçenekleri daha hızlı ve daha iyi güvenliği (TLS 1.2 dahil), ancak tüm işlevselliği ve seçenekleri sağlamayabilir `HttpClient` sınıfı.

### <a name="ssltls-implementation-android"></a>SSL/TLS uygulaması (Android)

Android projesi seçenekleri desteklemek için hangi SSL/TLS uygulaması seçmenizi sağlar:

- **Mono ve yönetilen** – android'de TLS 1.1
- **Yerel** – android'de TLS 1.2.

Yeni Xamarin projeleri (Bu, tüm projeleri için tavsiye edilir) TLS 1.2 destekleyen yerel uygulama varsayılan olarak, uyumluluk açısından gerekli ancak, yönetilen koda geri dönebilirsiniz.

> [!IMPORTANT]
> **Mono/yönetilen** seçeneği olmuştur [iOS ve Mac kaldırıldı](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) proje seçenekleri.
>
> Yerel seçenek, iOS ve Mac platformları üzerinde her zaman kullanılır.

## <a name="platform-specific-details"></a>Platforma özgü ayrıntıları

Yukarıdaki özeti Xamarin projelerinde HttpClient ve SSL/TLS uygulaması için proje düzeyi ayarlarını açıklar. HttpClient uygulaması kodunu da bir dinamik olarak ayarlanabilir. Daha fazla bilgi için bu platforma özgü kılavuzlara bakın:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS ve Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>Özet

Uygulama Mümkünse, Aktarım Katmanı Güvenliği (TLS) 1.2 kullanmanız gerekir.
Bu makaledeki yönergelere göre mevcut uygulamalarda ayarları güncelleştirmek daha sonra yeniden oluşturun ve müşterilerinize yeniden dağıtma gerekir.

## <a name="related-links"></a>İlgili bağlantılar

- [Uygulama Aktarım Güvenliği](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android ortamı](~/android/deploy-test/environment.md)
- [Xamarin döngüsü 9 (Şubat 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8 sürüm notları - TLS 1.2 desteği](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient HttpClientHandler ve WebRequestHandler açıklaması](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.NET.WebClient'a](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP istemcisi (örnek)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
