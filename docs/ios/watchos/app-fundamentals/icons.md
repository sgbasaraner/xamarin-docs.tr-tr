---
title: WatchOS Xamarin simgeleri ile çalışma
description: Bu belgede watchOS uygulama ve bu simgeleri eklemek için bir çözümü ayarlama nasıl için gereken çeşitli simgeleri açıklanmaktadır.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 150cca754de26edffcf97bb5d39b26166662c75b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790672"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>WatchOS Xamarin simgeleri ile çalışma

Apple Watch çözümleri iki simgeleri gerektirir:

* İPhone üzerinde görünür iOS uygulama simgeleri.
* Gözcü menüsünde bir daire ve bildirim ekranlar işlenir Apple Watch simgeler. İzleme uygulama simgesi de görünür [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS uygulaması.

## <a name="apple-watch-icons"></a>Apple Watch simgeler

| | | |
|-|-|-|
|iOS uygulama simgesi|İPhone üzerinde görünür ve üst uygulama başlatır|![](icons-images/icon-ios.png)|
|İzleme uygulama simgesi|Apple Watch giriş ekranında görüntülenir|![](icons-images/icon-home.png)|
||Gözcü bildirimlerini görüntülenir|![](icons-images/notification-icon.png)|
||Görünür [iOS Apple Watch uygulaması](~/ios/watchos/app-fundamentals/settings.md)|![](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Çözümünüzü yapılandırma

İOS uygulamanızı ve izleme uygulama adının doğru ve simgesini göster emin olmak için her proje için bu yönergeleri izleyin:

### <a name="ios-app"></a>iOS uygulaması

Başvurmak [iOS uygulama simgeleri Kılavuzu](~/ios/app-fundamentals/images-icons/app-icons.md) , iOS uygulamanızın simgeleri doğru şekilde yapılandırıldığından emin olmak için.

#### <a name="infoplist"></a>Info.plist

İzleme uygulamanızda yanındaki görünen dize [Apple Watch ayarları uygulaması](~/ios/watchos/app-fundamentals/settings.md) yapılandırılan **iOS uygulamanın Info.plist**.

Onaylayın, **Info.plist** sahip bir `CFBundleName` anahtarı ve değeri (Not: Bu farklı `CFBundleDisplayName`, her ikisi de olabilir):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch uygulama

Bir kez, [üst uygulama](~/ios/watchos/app-fundamentals/parent-app.md) kendi simgeler yapılandırmamış, bir uygulama simgesi varlık Kataloğu izleme uygulamaya eklemeniz gerekir.

1. İzleme uygulaması projesine sağ tıklayıp seçin **Dosya > Ekle > Yeni Dosya > iOS > varlık Kataloğu** bir varlık katalog projeye eklemek için.

 ![](icons-images/newasset.png "Bir varlık katalog projeye ekleyin")

2. Çift **AppIcons.appiconset/Contents.json** dosyası

  ![](icons-images/xcassets-iconset-sml.png "AppIcons içeriği")

3. Tüm watchOS görüntüleri bu ekran görüntüsünde gösterildiği gibi ekleyin:

  [![](icons-images/appicons-sml.png "Bu ekran görüntüsünde gösterildiği gibi tüm watchOS görüntüleri ekleme")](icons-images/appicons.png#lightbox)

  Başvurmak [Apple'nın simgesi yönergeleri](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html) (boyutları ekranda da de gösterilir) gerekli boyutları için. Bu simgeleri bir daire içinde işlemek için otomatik olarak kırpılacak unutmayın.

  Simge listesi aşağıdakine benzer görünmelidir:

  ![](icons-images/xcassets-complete-sml.png "Çözüm Gezgini'nde simge listesi")

4. Varlık Kataloğu uygulamada dahil emin olmak için aşağıdaki anahtarı ekleyin ve değerini **izleme uygulamanın Info.plist**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcons.appiconset</string>
```

Simgeler yapılandırılmış olan doğru denetleyerek doğrulayabilirsiniz [Apple Watch ayarları uygulaması](~/ios/watchos/app-fundamentals/settings.md) iPhone benzeticisi, içinde veya oluşturma bir [bildirim](~/ios/watchos/platform/notifications.md) ve simge onaylayan bildirimi görüntülenir Ekran.

> [!NOTE]
> Simgeler (alfa kanal varsa, uygulamanın uygulama mağazası gönderme sırasında reddedilecek) alfa kanalı sahip olamaz. Alfa kanalı var ve kaldırmak, denetleyebilirsiniz [Mac OS x'te önizleme uygulamasını kullanarak](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>İlgili bağlantılar

- [Apple'nın simgesi & resimleri Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)
