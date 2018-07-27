---
title: WatchOS simgeleri Xamarin ile çalışma
description: Bu belgede, çeşitli simgeler watchOS uygulama ve nasıl bir çözümü bu simgeler dahil etmek için ayarlama açıklanmaktadır.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/26/2018
ms.openlocfilehash: e46ecc9d78ccc5dcfbe571c9ec5350fe6c391b7e
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275930"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>WatchOS simgeleri Xamarin ile çalışma

Apple Watch çözümler iki simgeden birini gerektirir:

* İphone'da görünür iOS uygulama simgeleri.
* Apple Watch simgeleri watch menüsünde bir daire ve bildirim ekranlarda oluşturulur. Watch uygulaması simgesinde ayrıca şurada görünür [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS uygulaması.

## <a name="apple-watch-icons"></a>Apple Watch simgeleri

| | | |
|-|-|-|
|iOS uygulama simgesi|İphone'da görünür ve üst uygulama başlar|![iOS uygulama simgesi](icons-images/icon-ios.png)|
|Uygulama simgesi izleyin|Apple Watch giriş ekranında görüntülenir.|![watchOS uygulama simgesi](icons-images/icon-home.png)|
||İzleme bildirimlerinde görünür|![watchOS bildirim simgesi](icons-images/notification-icon.png)|
||Görünür [iOS Apple Watch uygulaması](~/ios/watchos/app-fundamentals/settings.md)|![iOS Watch uygulaması simgesi](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Çözümünüzü yapılandırma

İOS uygulamanızı hem de watch uygulaması doğru adını ve simgesini göster emin olmak için her proje için bu yönergeleri izleyin:

### <a name="ios-app"></a>iOS uygulaması

Başvurmak [iOS uygulama simgeleri kılavuz](~/ios/app-fundamentals/images-icons/app-icons.md) simgeler, iOS uygulamanızın doğru yapılandırıldığından emin olmak için.

#### <a name="infoplist"></a>Info.plist

Watch uygulaması yanında dize [Apple Watch ayarlar uygulamasında](~/ios/watchos/app-fundamentals/settings.md) yapılandırılan **iOS uygulamanın Info.plist**.

Doğrulayın, **Info.plist** sahip bir `CFBundleName` anahtar ve değer (Not: Bu farklı `CFBundleDisplayName`, her ikisi de olabilir):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch uygulaması

Bir kez, [üst uygulama](~/ios/watchos/app-fundamentals/parent-app.md) watch uygulaması için bir uygulama simgesi varlık Kataloğu eklemenize gerek sahip alt simgeler yapılandırılmış,.

1. Watch uygulaması projeye sağ tıklayıp **Dosya > Ekle > yeni dosya... > iOS > varlık Kataloğu** bir varlık Kataloğu projeye eklenecek.

 ![](icons-images/newasset.png "Projeye bir varlık Kataloğu ekleyin")

2. Çift **AppIcon.appiconset/Contents.json** dosyası

  ![](icons-images/xcassets-iconset-sml.png "AppIcon içeriği")

3. WatchOS görüntüleri, bu ekran görüntüsünde gösterildiği gibi ekleyin:

  [![](icons-images/appicons-sml.png "Bu ekran görüntüsünde gösterildiği gibi watchOS görüntüleri ekleme")](icons-images/appicons.png#lightbox)

  Başvurmak [Apple'nın simgesi yönergeleri](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/menu-icons/) (boyutları ekranında da de gösterilir) gerekli boyutları için. Bu simgeler bir daire içinde işlemek için otomatik olarak kırpılır unutmayın.

  Simge listenize şunun gibi görünmelidir:

  ![](icons-images/xcassets-complete-sml.png "Çözüm Gezgini'nde simge listesi")

4. Varlık Kataloğu uygulamada dahil olmak üzere, şu anahtarı ekleyin ve değerini **Watch uygulamanın Info.plist**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcon.appiconset</string>
```

Simgeleri yapılandırılmış olan doğru denetleyerek doğrulayabilirsiniz [Apple Watch ayarlar uygulamasında](~/ios/watchos/app-fundamentals/settings.md) iPhone simülatörü, içinde veya oluşturma bir [bildirim](~/ios/watchos/platform/notifications.md) ve bildirim görünür simge onaylanıyor ekranı.

> [!NOTE]
> Simgeler (bir alfa kanalı mevcutsa uygulama sırasında App Store gönderim reddedilir) alfa kanalı sahip olamaz. Alfa kanalı var ve kaldırmak denetleyebilirsiniz [önizleme uygulamayı kullanarak Mac OS X üzerinde](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>İlgili bağlantılar

- [Apple'nın watchOS simgesi ve görüntü Kılavuzu](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/)
