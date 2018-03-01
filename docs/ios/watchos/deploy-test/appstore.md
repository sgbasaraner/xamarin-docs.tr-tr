---
title: "Uygulama deposuna dağıtma"
description: "Uygulama mağazası uygulamalarının dağıtımını izleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: eed12c84b9952ef5c3dd27847071f05392bc16c4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="deploying-to-the-app-store"></a>Uygulama deposuna dağıtma

> [!IMPORTANT]
>  Gözden geçirmeyi unutmayın [Apple'nın izleme Seti gönderme Kılavuzu](https://developer.apple.com/app-store/watch/)ve [sorun giderme](#Troubleshooting) olabilecek sorunlar için bölüm.

- Olduğundan emin olun:
  - [**Dağıtım sağlama profilleri** ](#provisioning) projeleri için oluşturulan.
  - **Dağıtım hedef** (`MinimumOSVersion`) iOS app kümesine üst için **8.2** veya önceki (8.3 desteklenmez).

- İçinde [ **iTunes Bağlan**](#iTunes_Connect):

  - İOS uygulama girişi oluşturun (veya ekleme bir **yeni sürümü** varolan bir uygulamaya).
  - İzleme simgesi ve ekran görüntüleri ekleyin.

- Ardından [Mac için Visual Studio](#xamarin_studio) (Visual Studio şu anda desteklenmiyor):

  - İOS uygulaması sağ tıklatın ve seçin **başlangıç projesi olarak ayarla**.
  - Dönüştür **App Store** yapılandırma.
  - Kullanım **arşiv** özelliği bir uygulama arşiv oluşturun.

- Son olarak, geçiş [Xcode 6.2 +](#xcode)

  - Git **penceresi > düzenleyici** ve **arşivler**.
  - Uygulama ve Arşiv listeden seçin.
  - (İsteğe bağlı) **Doğrula...**  arşiv.
  - **Gönder...**  Arşiv ve iTunes yüklemek için adımları gözden geçirin ve onay için bağlanmak izleyin.

Bu öğeler için ilgili belirli ipuçları konusunu okuyun. Bkz: [sorun giderme](#Troubleshooting) sorunlar yaşıyorsanız, bölüm.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Dağıtım sağlama profilleri

Uygulama mağazası dağıtım oluşturmanız için oluşturmak için bir **dağıtım sağlama profili** çözümünüzdeki her uygulama kimliği.

Joker karakter uygulama kimliği varsa *yalnızca bir sağlama profili gerekli olacaktır*; ancak her proje için ayrı bir uygulama Kimliğine sahip sonra her uygulama kimliği için bir sağlama profili gerekir:

![](appstore-images/provisioningprofile-distribution-sml.png "Uygulama mağazası dağıtım profili")

Üç profil oluşturduktan sonra bunlar listede görüntülenir. Her birini (çift) yükleyip unutmayın:

![](appstore-images/provisioningprofiles-sml.png "Kullanılabilir profiller listesi")

Sağlama profilinde doğrulayabilirsiniz **proje seçenekleri** seçerek **Yapı > iOS paket imzalama** ekran ve seçerek **AppStore | iPhone** yapılandırma.

**Sağlama profili** listesini tüm eşleşen Profilleri Göster - Bu aşağı açılan listesinde oluşturduğunuz eşleşen profili görmeniz gerekir.

![](appstore-images/options-selectprofile-sml.png "Paket imzalama iOS iletişim")

## <a name="itunes-connect"></a>iTunes Connect

İzleyin [uygulama dağıtım genel bakış](~/ios/deploy-test/app-distribution/index.md), özellikle:

- [Uygulama iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Uygulama mağazası yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

Uygulama iTunes Bağlan yapılandırırken, ekran görüntüleri ve izleme simgesi eklemek unutmayın:

![](appstore-images/itunesconnect-watch-sml.png "Ekran görüntülerinde iTunes Bağlan ve izleme simgesi")

Simge dosyası 1024 x 1024 piksel olmalıdır ve görüntülendiğinde uygulanan bir döngüsel maskesi gerekir. Simge alfa kanalı yok.

En az bir ekran görüntüsü gerekli, en fazla beş gönderilebilir.
312 x 390 piksel olması ve izleme uygulamanızda eylem gösterme.
Bu boyutta ekran görüntüleri alma 42 mm izleme simulator kullanabilirsiniz.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Mac için Visual Studio

1. İOS uygulaması başlangıç projesi olduğundan emin olun. Değilse, bunu ayarlamak için sağ tıklatın:

  ![](appstore-images/xs-startup.png "Başlangıç projesi ayarlama")

2. Seçin **AppStore** yapı yapılandırması:

  ![](appstore-images/xs-appstore.png "AppStore yapı yapılandırması")

3. Seçin **Yapı > Arşiv** menü öğesi arşiv işlemini başlatmak için:

  ![](appstore-images/xs-archive.png "Yapı menüsü")

Ayrıca seçebilirsiniz **Görünüm > arşivler...**  daha önce oluşturduğunuz arşivler görmek için menü öğesi.

  ![](appstore-images/xs-archives-sml.png "Arşivler görünümü")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode otomatik olarak Mac için Visual Studio'da oluşturulan arşivler gösterir

1. Xcode başlatın ve seçin **penceresi > düzenleyici**:

  ![](appstore-images/xc-organizer.png "Pencere menüsü")

2. Geçiş **arşivler** sekmesinde ve Visual Studio ile Mac için oluşturulan Arşivi'ni seçin:

  ![](appstore-images/xc-archives.png "Arşivler sekmesi")

3. İsteğe bağlı olarak **doğrula...**  arşiv ardından **Gönder...**  uygulamayı iTunes Bağlan karşıya yüklemek için.

4. (Birden fazla ait olduğunuz varsa) geliştirme ekibi seçin ve ardından gönderim onaylayın:

  ![](appstore-images/xc-submit1.png "Geliştirme ekibi bölümü")

5. İTunes yeniden karşıya yüklenen ikili görmek için Bağlan'ı ziyaret edin. Uygulamanızın yapılandırma sayfasına gidin ve seçin **yayın öncesi** görmek için üstteki menüden **derlemeler** listesi:

  [ ![](appstore-images/itc-prerelease-sml.png "Uygulamaları yapılandırma sayfasında iTunes Bağlan")](appstore-images/itc-prerelease.png)

Üzerinde uygulama onay sonra gönderebilirsiniz **sürümleri** sayfası. Başvurmak [iOS uygulama dağıtım genel bakış](~/ios/deploy-test/app-distribution/index.md) daha fazla bilgi için.


## <a name="troubleshooting"></a>Sorun giderme

Burada, App Store ve bunları gidermek için atabileceğiniz adımları gönderme sırasında karşılaşabileceğiniz bazı hatalar bulunmaktadır.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Arşiv menü seçeneği Mac için Visual Studio'da görünür değil

İzleyin [yukarıdaki adımları](#xamarin_studio) arşivlemek için çözümü yapılandırmak için. Başlangıç projesi doğru şekilde ayarlanamıyor, derleme yapılandırması hata ayıklama veya başlangıç projesi değiştirmeye çalışmadan önce serbest ilk ayarlandığından emin olun. Ardından yapı yapılandırması başa kümesi **AppStore**.

### <a name="invalid-icon"></a>Geçersiz Simgesi

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcons27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

İzleyin [alfa kanal kaldırma yönergeleri](~/ios/watchos/troubleshooting.md) simgelerini gelen.

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion Mismatch

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Çözümünüzdeki - iOS uygulaması, izleme uzantısı ve izleme uygulaması - tüm projeleri aynı sürüm numarasına kullanıyor olması gerekir. Her Düzenle **Info.plist** sürüm numarasını tam olarak eşleşen dosya.

### <a name="missing-icons"></a>Simgeleri eksik

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

' Ndaki yönergeleri izleyin [simgeleriyle çalışma](~/ios/watchos/app-fundamentals/icons.md) izleme uygulama projesi gerekli tüm görüntüleri eklemek için.

### <a name="missing-icon"></a>Eksik simgesi

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Visual Studio en son sürümünü Mac ve, için olduğundan emin olun, **AppIcons.appiconset** görüntüleri eksiksiz bir kümesini içerir. Yine de bu hatayı görüyorsanız, kaynağını görüntülemek **Contents.json** gerekli tüm görüntüleri için bir giriş içerdiği onaylamak için. Alternatif olarak, Xamarin en son sürümünü kullandığınızı doğruladıktan sonra silip yeniden oluşturmanız **AppIcons.appiconset**.

> [!IMPORTANT]
> Not: Bir bilinen hata Visual Studio'da Mac'ın izleme simgesi için desteği yoktur: 88 x 88 piksel resmi bekliyor  **29x29@3x**  (87 x 87 piksel olmalıdır) görüntüsü.


Bu Mac - ya da Düzenle xcode'da görüntü varlığı için Visual Studio'da düzeltin veya el ile düzenlediğinizde **Contents.json** dosyası (eşleşecek şekilde [Bu örnek](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>Geçersiz WatchKit desteği

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Bu ileti doğrulama ve gönderme sırasında veya otomatik bir e-posta iTunes Bağlan ' sonra görünüşe göre başarılı bir karşıya yükleme görünebilir.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Not: Gerekir **arşiv** uygulamanızı Mac ve Xcode 6.2 + ENTER anahtara doğrulamak ve iTunes Bağlan karşıya yüklemek için Visual Studio.


Kararlı Xamarin kanalı ve Xcode 6.2 + kullanın.



### <a name="invalid-provisioning-profile"></a>Geçersiz sağlama profili

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Dağıtım sağlama profilleri** izleme uygulama çözümde tüm üç proje için sağlanmalıdır: iOS uygulaması, izleme uzantısı ve izleme uygulaması - ya da açıkça (üç profilleri) veya bir tek joker profili aracılığıyla. Sağlama profili iOS Geliştirme Merkezi yok ve indirilir ve Mac için eklenen denetleyin

### <a name="invalid-code-signing-entitlements"></a>Geçersiz kod yetkilendirmeler imzalama

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Sağlama profilleri Kurulum Apple Dev Center üzerinde doğru olduğundan ve indirilir ve bunları yüklü emin olun. Ayrıca her proje için Mac'in Özellikler penceresi için Visual Studio'da ayarlandıktan denetleyin.

### <a name="invalid-architecture"></a>Geçersiz mimarisi

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Gözcü uygulamalar yalnızca ekleyebilirsiniz [Unified API (64 bit)](~/cross-platform/macios/unified/index.md) Xamarin.iOS uygulamaları.
İOS uygulama projeye sağ tıklayın ardından Git **Seçenekleri > Yapı > iOS Yapı > Gelişmiş sekmesinde** ve emin olun **desteklenen mimariler** için AppStore iPhone yapılandırma içerir**ARM64** (ör.) **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Bu paket geçersiz.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Üst iOS uygulamanızı '8.2' veya daha önce MinimumOSVersion olması gerekir.

### <a name="non-public-api-usage"></a>Ortak olmayan API kullanımı

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Xcode ve Xamarin'ın araçları en son sürümünü kullandığınızdan emin olun.
Kodunuzu tüm genel olmayan API'leri erişemezler.

### <a name="build-error-mt5309"></a>Hata MT5309 derleme

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Bu hata muhtemelen, Xcode yüklemenizi yeniden adlandırılmış sonucudur **Xcode.app**. Yüklemenize yeniden adlandırırsanız, örneğin, bu hata oluşur **XCode 6.2.app**.



## <a name="related-links"></a>İlgili bağlantılar

- [Apple WatchKit gönderme Kılavuzu](https://developer.apple.com/app-store/watch/)
