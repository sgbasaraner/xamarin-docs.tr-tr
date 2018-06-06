---
title: WatchOS Xamarin ayarları ile çalışma
description: Bu belge Xamarin watchOS ayarlarında ile nasıl çalışılacağını açıklar. İPhone üzerinde uygulama ve Apple Watch uygulama bu ayarları kullanarak ekleme ayarlarını izleme uygulama çözümü açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 6cfbcf3b4383588819490838c2a54cdb4faf9403
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790883"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>WatchOS Xamarin ayarları ile çalışma

Apple Watch uygulamaları iOS uygulamaları olarak aynı ayarları işlevselliği kullanabileceğini - ayarları kullanıcı arabirimi görüntülenir **Apple Watch** iPhone uygulama ancak değerleri hem iPhone uygulamanızı ve izleme uzantısı da erişilebilir.

![](settings-images/intro.png "İOS uygulamaları olarak kullanabileceğiniz Apple Watch uygulamaları aynı ayarları işlevi")

Ayarlar hem iOS uygulaması hem de tarafından tanımlanan izleme uygulaması uzantısının erişilebilen bir paylaşılan dosya konumunda depolanan bir **uygulama grubu**. Yapmanız gerekenler [bir uygulama grubu yapılandırmak](~/ios/watchos/app-fundamentals/app-groups.md) ayarları aşağıdaki yönergeleri kullanarak eklemeden önce.

## <a name="add-settings-in-a-watch-solution"></a>Bir izleme çözümü ayarları ekleme

İçinde **iPhone uygulama** çözümünüzdeki (*değil* izleme uygulama veya uzantı):

1. Sağ **Ekle > yeni dosya...**  ve **Settings.bundle** (adını düzenleyemezsiniz **yeni dosya** iletişim):

   [![](settings-images/settings-add-sml.png "Yeni ayarlar paketi ekleme")](settings-images/settings-add.png#lightbox)

2. Adına değiştirme **ayarları Watch.bundle** (seçin ve yazın **komutu + R** yeniden adlandırmak için):

   ![](settings-images/settings-rename.png "Paket yeniden adlandırma")

3. Yeni bir anahtar ekleyin `ApplicationGroupContainerIdentifier` için **Root.plist** yapılandırdığınız, (örn. uygulama grubu için ayarlanmış değere sahip `group.com.xamarin.WatchSettings` Aşağıdaki örnekte):

   [ ![](settings-images/settings-appgroup-sml.png "Root.plist ApplicationGroupContainerIdentifier anahtar ekleme")](settings-images/settings-appgroup.png#lightbox)

4. Düzen **Settings-Watch.bundle/Root.plist** kullanılacak - istediğiniz seçenekleri içeren bir grup şablon dosyası içerir.
  TextField, iki durumlu anahtar ve kaydırıcısını varsayılan (silin ve kendi ayarlarınızla değiştirin):

  [![](settings-images/rootplist-sml.png "Settings-Watch.bundle/Root.plist Düzenle")](settings-images/rootplist.png#lightbox)


## <a name="use-settings-in-the-watch-app"></a>İzleme uygulama ayarlarını kullanma

Kullanıcı tarafından seçilen değerlerine erişmek için oluşturma bir `NSUserDefaults` uygulama grubu kullanarak ve belirterek örnek `NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch uygulama

[![](settings-images/settings-app-sml.png "İPhone yeni Apple Watch uygulaması")](settings-images/settings-app.png#lightbox)

Kullanıcılar yeni aracılığıyla ayarlarıyla etkileşim **Apple Watch** kendi iPhone uygulama. Bu uygulama uygulamalar izleme ve ayrıca ayarları kullanıma sunulan kullanarak düzenleme Göster/Gizle olanak tanır. **ayarları Watch.bundle**.

![](settings-images/applewatch-1.png "Örnek uygulama ayarları") ![ ] (settings-images/applewatch-2.png "örnek uygulama ayarları")



## <a name="related-links"></a>İlgili bağlantılar

- [Apple'nın ayarları belge](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
