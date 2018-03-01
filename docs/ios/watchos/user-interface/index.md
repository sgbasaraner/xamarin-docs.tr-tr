---
title: "watchOS kullanıcı arabirimi"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 34063be728f8a13bbe5d68440d9aceba417c5627
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="watchos-user-interface"></a>watchOS kullanıcı arabirimi

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) örneği, çeşitli watchOS denetimleri gösterir. Uygulamanın film şeridi (yakınlaştırma için tıklatın) aşağıda gösterilmiştir:

[ ![](images/storyboard-sml.png "Örnek watchOS düzeni")](images/storyboard.png)

Tüm denetimler programlı adlarını önekiyle `WKInterface` (ör.) `WKInterfaceLabel`, `WKInterfaceButton`).


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
        <strong>denetimi</strong>
      </th>
      <th>
        <strong>Açıklama</strong>
      </th>
      <th>
        <strong>ekran görüntüsü</strong>
      </th>
    </thead>
    <tbody>
    <tr>
      <td valign="top">
Etiketle </td>
      <td valign="top">
Kullanım <code>SetText</code> ve etiket denetimi içindeki metnin görünümünü denetlemek için diğer özellikleri. <code>NSAttributedString</code> Ayrıca desteklenir.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/label.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Düğme </td>
      <td valign="top">
Oluşturun ve film şeridi özellikleri ayarlayın. <kbd>CTRL + Sürükle</kbd> eklemek için bir <code>Action</code> tıklandığında işleyicisi uygulamak için.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/button.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Anahtar </td>
      <td valign="top">
Kullanım <code>SetOn</code> anahtar durumunu denetlemek için.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/switch.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Kaydırıcı </td>
      <td valign="top">
Birçok farklı stillerde mümkündür.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/slider.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Görüntü </td>
      <td valign="top">
Kullanmak <code>myImage.SetImage("MyWatchImage")</code> izleme görüntülerinde yüklemeye veya <code>WKInterfaceDevice.CurrentDevice.AddCachedImage</code> bunları saatin üzerinde yinelenen kullanılmak üzere önbelleğe almak için.
        <br />
        <a href="~/ios/watchos/user-interface/image.md">Görüntü Denetim belgeleri</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/image.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Ayırıcı </td>
      <td valign="top">
Ayırıcı çekici izleme Uı'lar oluşturmaya yardımcı olmak için kullanın.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/separator.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
eşleme </td>
      <td valign="top">
Harita resminin saatin statik olarak görüntülenir, ancak birçok yönünü PIN'ler ekleme gibi görünümünü denetleyebilirsiniz.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/map.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Film & InlineMove </td>
      <td valign="top">
Film olabilir ya da kendi başlarına açık veya satır içi <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/movie.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Grup </td>
      <td valign="top">
Çekici izleme Uı'lar oluşturmaya yardımcı olmak için grupları kullanın.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/group.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Tablo </td>
      <td valign="top">
İOS tablolarda basitleştirilmiş bir sürümünü.
Uygulama <code>DidSelectRow</code> değiştirmek üzere kullanıcı seçimi yanıt (veya bir segue kullanın).
        <br />
        <a href="~/ios/watchos/user-interface/table.md">Tablo denetim belgeleri</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TableDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/table.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Cihaz </td>
      <td valign="top">
        <code>WKInterfaceDevice.CurrentDevice</code> gibi özellikler içeren <code>ScreenBounds</code>, <code>ScreenScale</code>, ve <code>PreferredContentSizeCategory</code>.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/device.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="~/ios/watchos/user-interface/menu.md">Menü</a>
      </td>
      <td valign="top">
Film şeridi zorla tuşuna menü tanımlayın ve her düğme için eylemleri koda uygulanması.
        <br />
        <a href="~/ios/watchos/user-interface/menu.md">Menü denetim (zorla Touch) belgeleri</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/controller.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Metin girişi </td>
      <td valign="top">
Kullanım <code>PresentTextInputController</code> ve <code>WKTextInputMode</code> numaralandırması.
        <br />
        <a href="~/ios/watchos/user-interface/text-input.md">Metin girişi belgeleri</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/textinput.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Dijital Dama yapma </td>
      <td valign="top">
Bir seçici sürücü için dijital Dama yapma kullanılabilir veya kendi dönüş kodu izlenebilir.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/digital-crown.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Hareketleri </td>
      <td valign="top">
Bir Sahne eklenebilir hareketi tanıma dört tür vardır: dokunun, geçirme, Pan ve LongPress.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs">Katalog kodu</a>
      </td>
      <td>
        <img src="Images/gestures.png" class="tableimg">
      </td>
    </tr>
    </tbody>
</table>



## <a name="related-links"></a>İlgili bağlantılar

- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Gözcü Seti API Başvurusu](https://developer.xamarin.com/api/namespace/WatchKit/)
