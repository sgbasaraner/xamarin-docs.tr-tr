---
title: "watchOS kullanıcı arabirimi"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: f7a7b565372970cc487b664ed7415cf876e290b6
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="watchos-user-interface"></a>watchOS kullanıcı arabirimi

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) örneği, çeşitli watchOS denetimleri gösterir. Uygulamanın film şeridi (yakınlaştırma için tıklatın) aşağıda gösterilmiştir:

[![](images/storyboard-sml.png "Örnek watchOS düzeni")](images/storyboard.png#lightbox)

Tüm denetimler programlı adlarını önekiyle `WKInterface` (ör.) `WKInterfaceLabel`, `WKInterfaceButton`).

|Denetim|Açıklama|ekran görüntüsü|
|---|---|---|
|Etiketle|Kullanım `SetText` ve etiket denetimi içindeki metnin görünümünü denetlemek için diğer özellikleri. `NSAttributedString` Ayrıca desteklenir.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Düğme|Oluşturun ve film şeridi özellikleri ayarlayın. CTRL + Sürükle eklemek için bir `Action` tıklandığında işleyicisi uygulamak için.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Anahtar|Kullanım `SetOn` anahtar durumunu denetlemek için.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Kaydırıcı|Birçok farklı stillerde mümkündür.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Görüntü|Kullanmak `myImage.SetImage("MyWatchImage")` izleme görüntülerinde yüklemeye veya `WKInterfaceDevice.CurrentDevice.AddCachedImage` bunları saatin üzerinde yinelenen kullanılmak üzere önbelleğe almak için.<br />[Görüntü Denetim belgeleri](~/ios/watchos/user-interface/image.md)<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Ayırıcı|Ayırıcı çekici izleme Uı'lar oluşturmaya yardımcı olmak için kullanın.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|eşleme|Harita resminin saatin statik olarak görüntülenir, ancak birçok yönünü PIN'ler ekleme gibi görünümünü denetleyebilirsiniz.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Film & InlineMove|Film olabilir ya da kendi başlarına açık veya satır içi<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Grup|Çekici izleme Uı'lar oluşturmaya yardımcı olmak için grupları kullanın.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Tablo|İOS tablolarda basitleştirilmiş bir sürümünü. Uygulama `DidSelectRow` değiştirmek üzere kullanıcı seçimi yanıt (veya bir segue kullanın).<br />[Tablo denetim belgeleri](~/ios/watchos/user-interface/table.md)<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Cihaz|`WKInterfaceDevice.CurrentDevice` gibi özellikler içeren `ScreenBounds`, `ScreenScale`, ve `PreferredContentSizeCategory`.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menü](~/ios/watchos/user-interface/menu.md)|Film şeridi zorla tuşuna menü tanımlayın ve her düğme için eylemleri koda uygulanması.<br />[Menü denetim (zorla Touch) belgeleri](~/ios/watchos/user-interface/menu.md)<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Metin girişi|Kullanım `PresentTextInputController` ve `WKTextInputMode` numaralandırması.<br />[Metin girişi belgeleri](~/ios/watchos/user-interface/text-input.md)<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Dijital Dama yapma|Bir seçici sürücü için dijital Dama yapma kullanılabilir veya kendi dönüş kodu izlenebilir.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Hareketleri|Bir Sahne eklenebilir hareketi tanıma dört tür vardır: dokunun, geçirme, Pan ve LongPress.<br />[Katalog kodu](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>İlgili bağlantılar

- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Gözcü Seti API Başvurusu](https://developer.xamarin.com/api/namespace/WatchKit/)
