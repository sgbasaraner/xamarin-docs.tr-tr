---
title: watchOS menü denetiminde (zorla Touch) Xamarin
description: Bu belge, Xamarin içinde watchOS zorla dokunma hareketi kullanmayı açıklar. Zorla dokunma için yanıt nasıl ele menü ve menü öğelerini değiştirme ekleme.
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4b973b925b99189416087224644c376864c56871
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791351"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>watchOS menü denetiminde (zorla Touch) Xamarin

Gözcü Seti izleme uygulama ekranda uygulandığında menü tetikler zorla Touch bir hareketi sağlar.

![](menu-images/menu.png "Apple Watch menü gösterme")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Zorla dokunma yanıt

Varsa bir `Menu` bir arabirim denetleyicisi için bir kullanıcı bir menüsü görüntülenir, zorla Touch gerçekleştirdiğinde uygulanmıştır. Hiçbir menüsünü uygulanırsa, ekran kısa bir süre animasyonlu hiçbir diğer eylem oluşur.

Zorla rötuşları ekranında belirli bir öğesiyle ilişkili olmayan; yalnızca bir menü bir arabirim denetleyicisi eklenebilecek ve zorla Touch tuşuna ekranda nerede oluştuğunu bağımsız olarak görüntülenir.

Arasında bir ve dört menü seçenekleri sunulabilir.


## <a name="adding-a-menu"></a>Menü ekleme

A `Menu` eklenmeli bir `InterfaceController` tasarım zamanında şeridinde. Menü denetimi bir arabirim denetleyicide sürüklendiğinde film şeridi preview'daki görsel bir gösterge yoktur ancak **menü** görünür **belge anahattı** paneli:

![](menu-images/menu-action.png "Bir menüyü tasarım zamanında düzenleme")

En fazla dört menü öğeleri menü denetimine eklenebilir. İçinde yapılandırılabilir **özellikleri** paneli. Aşağıdaki öznitelikler ayarlanabilir:

- Başlık, ve
- Özel görüntü veya
- Bir sistem görüntüsü: kabul etme, Ekle, blok, reddet, bilgi, belki de, daha sessiz, duraklatma, yürütmek, yineleyin, Sürdür, paylaşım, karışık, konuşmacı, çöp kutusu.

Oluşturma bir `Action` seçerek **olayları** bölümünü **özellikleri** paneli ve eylem yöntemi adını yazarak. Kısmi bir yöntem arabirimi denetleyicisi sınıfında şöyle uygulanabilir kod oluşturulacak:

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Özel resimler

Benzer şekilde iOS sekmesini görüntülerde, menü öğesi görüntüleri üzerinden göstermek arka plan veren alfa kanalı donuk bir desenle gerektirir.

En iyi performans için izleme uygulama projesi (izleme uygulama uzantısı projeye değil) menüsüne için kullanılan görüntüleri eklemeniz gerekir.


## <a name="changing-the-menu-items"></a>Menü öğelerini değiştirme

<!--
### Design Time Items

Menu items added the the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Çalışma zamanında ekleme

Sebep olamaz bir `Menu` çalışma zamanında bir arabirim denetleyicisi rağmen eklenecek koleksiyonunu `MenuItem`s *yapabilirsiniz* değiştirilebilir programlı olarak.
Kullanım `AddMenuItem` gösterildiği gibi yöntemi:

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

Xamarin.iOS izleme Seti API gerektirir şu anda bir `selector` için `AdMenuItem` şöyle bildirilmelidir yöntemi:

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Çalışma zamanında kaldırma

`ClearAllMenuItems` Yöntemi, tüm kaldırmak için çağrılabilir *program aracılığıyla eklenen* menü öğeleri.

Menü öğeleri film şeridi yapılandırılmış temizlenemez.



## <a name="related-links"></a>İlgili bağlantılar

- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple'nın menü belge](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
