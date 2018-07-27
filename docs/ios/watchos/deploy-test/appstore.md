---
title: WatchOS uygulamaları App Store için dağıtma
description: Bu belge, App Store için Xamarin ile oluşturulan watchOS uygulamaları dağıtmayı açıklar. Dağıtım sağlama profili ve iTunes Connect göz alır ve ayrıca bazı sorun giderme ipuçları sağlar.
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 90058f5074759fdded5d259004cb40c0cb7ea212
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276073"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>WatchOS uygulamaları App Store için dağıtma

> [!IMPORTANT]
> Gözden geçirmeyi unutmayın [Apple'nın izleme paketi gönderimi Kılavuzu](https://developer.apple.com/app-store/watch/)görüp [sorun giderme](#Troubleshooting) bölümde sahip olabileceğiniz herhangi bir sorunu.

- Olduğundan emin olun:
  - [**Dağıtım sağlama profilleri** ](#provisioning) projeleriniz için oluşturulmuş.
  - **Dağıtım hedefi** (`MinimumOSVersion`) için iOS için uygulama kümesi üst **8.2** veya önceki sürümleri (8.3 desteklenmez).

- İçinde [ **iTunes Connect**](#iTunes_Connect):

  - İOS uygulama girişi oluşturma (veya bir **yeni sürüm** var olan bir uygulama için).
  - İzleme simgesi ve ekran görüntüleri ekleyin.

- Ardından [Mac için Visual Studio](#xamarin_studio) (Visual Studio şu anda desteklenmiyor):

  - İOS uygulaması'nı sağ tıklatıp seçin **başlangıç projesi olarak ayarla**.
  - Değiştirin **App Store** yapılandırma.
  - Kullanım **arşiv** özelliği, bir uygulama arşivi oluşturun.

- Son olarak, geçiş [Xcode 6.2 +](#xcode)

  - Git **penceresi > düzenleyici** ve **arşivleri**.
  - Arşiv ve uygulamayı listeden seçin.
  - (İsteğe bağlı) **Doğrula...**  arşiv.
  - **Gönder...**  Arşiv ve iTunes yüklemek için aşağıdaki adımları gözden geçirin ve onay için bağlama izleyin.

Bu öğe için ilgili belirli ipuçlarını okuyun. Bkz: [sorun giderme](#Troubleshooting) sorunlarla karşılaşırsanız bölümü.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Dağıtım sağlama profilleri

App Store dağıtımı için oluşturmanız gereken oluşturmak için bir **dağıtım sağlama profili** çözümünüzdeki her uygulama kimliği.

Uygulama kimliği, joker karakter varsa *yalnızca bir sağlama profili, gerekli olacak*; ancak her proje için ayrı bir uygulama kimliği olan sonra her uygulama kimliği için bir sağlama profili gerekir:

![](appstore-images/provisioningprofile-distribution-sml.png "App Store dağıtım profili")

Üç profil oluşturduktan sonra bunlar listede görüntülenir. Her birini (çift) yükleyip unutmayın:

![](appstore-images/provisioningprofiles-sml.png "Kullanılabilir profiller listesi")

Sağlama profilinde doğrulayabilirsiniz **proje seçenekleri** seçerek **Yapı > iOS paket grubu imzalama** ekran ve seçerek **AppStore | iPhone** yapılandırma.

**Sağlama profili** listesini tüm eşleşen Profilleri Göster - Bu aşağı açılan listesinde oluşturduğunuz eşleşen profilleri görmeniz gerekir.

![](appstore-images/options-selectprofile-sml.png "İOS paket grubu imzalama iletişim kutusu")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

İzleyin [uygulama dağıtımına genel bakış](~/ios/deploy-test/app-distribution/index.md), özellikle:

- [Uygulama iTunes CONNECT'te yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store’da Yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

Uygulama iTunes CONNECT'te yapılandırırken, ekran görüntüleri ve izleme simgesi eklemek unutmayın:

![](appstore-images/itunesconnect-watch-sml.png "İTunes Connect ekran görüntüleri ve izleme simgesi")

Simge dosyası 1024 x 1024 piksel olmalıdır ve bir döngüsel maskesi görüntülendiğinde uygulanmış olur. Simge bir alfa kanalı sahip olmamalıdır.

En az bir ekran görüntüsü, gerekli, en fazla beş gönderilebilir.
312 x 390 piksel olması ve uygulamada Watch uygulaması gösterir.
Bu boyutta ekran görüntüleri alma 42 mm watch simülatör kullanabilirsiniz.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Mac için Visual Studio

1. İOS uygulaması başlangıç projesi olduğundan emin olun. Değilse, bunu ayarlamak için sağ tıklayın:

  ![](appstore-images/xs-startup.png "Başlangıç projesi ayarlama")

2. Seçin **AppStore** derleme yapılandırması:

  ![](appstore-images/xs-appstore.png "AppStore derleme yapılandırması")

3. Seçin **Yapı > Arşiv** arşivleme işlemini başlatmak için menü öğesi:

  ![](appstore-images/xs-archive.png "Derleme menüsü")

Ayrıca seçebilirsiniz **Görüntüle > arşivleri...**  daha önce oluşturduğunuz arşivlerini görmek için menü öğesi.

  ![](appstore-images/xs-archives-sml.png "Arşivleri görüntüle")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode otomatik olarak Mac için Visual Studio'da oluşturulan arşivleri gösterir

1. Xcode başlatın ve seçin **penceresi > düzenleyici**:

  ![](appstore-images/xc-organizer.png "Pencere menüsü")

2. Geçiş **arşivleri** sekme ve Mac için Visual Studio ile oluşturulmuş bir arşiv seçin:

  ![](appstore-images/xc-archives.png "Arşivleri sekmesi")

3. İsteğe bağlı olarak **doğrula...**  arşiv seçin **Gönder...**  iTunes CONNECT'te uygulamayı yüklemek için.

4. Geliştirme ekibi seçin (birden fazla ait olduğunuz) ve sonra gönderim onaylayın:

  ![](appstore-images/xc-submit1.png "Geliştirme ekibi bölümü")

5. İTunes Connect yeniden karşıya yüklenen ikili görmek için ziyaret edin. Uygulamanızın yapılandırma sayfasına gidip seçin **yayın öncesi** görmek için üstteki menüden **yapılar** listesi:

  [![](appstore-images/itc-prerelease-sml.png "İTunes Connect uygulama yapılandırma sayfası")](appstore-images/itc-prerelease.png#lightbox)

Ardından uygulama onay üzerinde gönderebilirsiniz **sürümleri** sayfası. Başvurmak [iOS uygulama dağıtımına genel bakış](~/ios/deploy-test/app-distribution/index.md) daha fazla bilgi için.


## <a name="troubleshooting"></a>Sorun giderme

App Store ve bunları gidermek için atabileceğiniz adımları gönderilirken karşılaşabileceğiniz bazı hataları aşağıda verilmiştir.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Arşiv menü seçeneği Mac için Visual Studio'da görünür değil

İzleyin [yukarıdaki adımları](#xamarin_studio) arşivleme çözümü yapılandırmak için. Başlangıç projesi doğru ayarlayamazsınız, derleme yapılandırmasını varsayılan hata ayıklama veya başlangıç projesi değiştirmeye çalışmadan önce yayın için ayarlandığından emin olun. Ardından derleme yapılandırmasını döner **AppStore**.

### <a name="invalid-icon"></a>Geçersiz Simgesi

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

İzleyin [alfa kanalı kaldırma yönergeleri](~/ios/watchos/troubleshooting.md) simgelerinizi öğesinden.

### <a name="cfbundleversion-mismatch"></a>CFBundleVersion uyuşmazlığı

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Çözümünüzdeki - iOS uygulaması, Watch uzantısı ve izleme uygulaması - tüm projeleri aynı sürüm numarasını kullanmalıdır. Her Düzen **Info.plist** sürüm numarasını tam olarak eşleşmesi dosya.

### <a name="missing-icons"></a>Simgeleri eksik

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Bölümündeki yönergeleri [simgelerle çalışma](~/ios/watchos/app-fundamentals/icons.md) tüm gerekli görüntüleri Watch uygulaması projesine eklemek için.

### <a name="missing-icon"></a>Eksik simgesi

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Visual Studio'nun en son sürümü Mac ve bu olduğundan emin olun, **AppIcon.appiconset** görüntüleri eksiksiz bir kümesini içerir. Yine de bu hatayı görüyorsanız, kaynağını görüntüleyerek **Contents.json** tüm gerekli görüntüleri için bir giriş içerdiği onaylamak için. Alternatif olarak, en yeni Xamarin sürümünü kullandığınızı doğruladıktan sonra silin ve yeniden oluşturun **AppIcon.appiconset**.

> [!IMPORTANT]
> Mac İzle simgesini desteği için Visual Studio'da bilinen bir hata yoktur: 88 x 88 piksel görüntü için bekliyor **29x29@3x** (87 x 87 piksel olmalıdır) görüntüsü.


Bu Visual Studio Mac - düzenleme ya da görüntü varlık xcode'da düzeltme veya el ile düzenlemeniz **Contents.json** dosyası (eşleştirilecek [Bu örnek](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>Geçersiz WatchKit desteği

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Bu ileti doğrulama ve gönderim sırasında veya otomatik bir e-posta iTunes Connect'ten görünüşe göre başarılı bir karşıya yükledikten sonra görünebilir.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Yapmanız gerekenler **arşiv** için Visual Studio Mac ve Xcode 6.2 + ENTER anahtara iTunes CONNECT'te yüklemek ve doğrulamak uygulamanıza.


Xamarin kararlı kanal ve Xcode 6.2 + kullanın.



### <a name="invalid-provisioning-profile"></a>Geçersiz sağlama profili

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Dağıtım sağlama profilleri** için bir watch uygulaması çözümü üç projenin tamamına sağlanmalıdır: - Watch uygulaması iOS uygulaması ve Watch uzantısı ya da açıkça (üç profil) veya bir tek bir joker profili aracılığıyla. Sağlama profilleri iOS Geliştirici Merkezi mevcut indirilir ve bunları Mac'iniz için eklenir ve denetleyin

### <a name="invalid-code-signing-entitlements"></a>Geçersiz kod imzalama yetkilendirmeleri

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Sağlama profilleriniz Kurulum Apple Dev Center üzerinde doğru olduğundan ve indirdiğiniz ve bunları yüklü emin olun. Ayrıca, her proje için Özellikler penceresini Mac için Visual Studio'da ayarlanır denetleyin.

### <a name="invalid-architecture"></a>Geçersiz mimarisi

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Watch uygulamalarına yalnızca ekleyebilirsiniz [birleşik API (64-bit)](~/cross-platform/macios/unified/index.md) Xamarin.iOS uygulamaları.
İOS uygulama projesine sağ tıklayın, ardından Git **Seçenekleri > derleme > iOS Derlemesi'ne > Gelişmiş sekmesinde** olduğundan emin olun **desteklenen mimariler** AppStore iPhone için yapılandırma içerir.**ARM64** (örn.) **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Bu paket geçersiz.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Üst iOS uygulamanızı MinimumOSVersion '8.2' veya daha önce ayarlanmış olması gerekir.

### <a name="non-public-api-usage"></a>Genel olmayan API kullanımı

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Xcode ve Xamarin Araçları'nın en son sürümünü kullandığınızdan emin olun.
Kodunuzu herhangi bir genel olmayan API'sini erişmeli değil.

### <a name="build-error-mt5309"></a>Hata MT5309 oluşturun

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Bu hata büyük olasılıkla, Xcode yüklemenizi yeniden adlandırılmış sonucu olan **Xcode.app**. Yüklemenize yeniden adlandırırsanız, bu hata meydana gelir **XCode 6.2.app**.



## <a name="related-links"></a>İlgili bağlantılar

- [Apple WatchKit gönderimi Kılavuzu](https://developer.apple.com/app-store/watch/)
