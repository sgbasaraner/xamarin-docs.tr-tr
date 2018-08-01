---
title: Açılır menü
description: Sabitlenmiş bir açılan menü için belirli bir görünüm ekleme.
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: d7cadde88e9ae7ee30815ee9323785038dbb1a39
ms.sourcegitcommit: ecdc031e9e26bbbf9572885531ee1f2e623203f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393665"
---
# <a name="popup-menu"></a>Açılır menü

[PopupMenu](https://developer.xamarin.com/api/type/Android.Widget.PopupMenu/) (olarak da adlandırılan bir _kısayol menüsünü_) için belirli bir görünüm bağlantılı bir menü. Aşağıdaki örnekte, bir düğme tek bir etkinlik içerir. Kullanıcı düğmeye bastığında üç öğe açılan menü görüntülenir:

[![Bir düğme ve üç öğe açılır menü içeren bir uygulama örneği](popup-menu-images/01-app-example-sml.png)](popup-menu-images/01-app-example.png#lightbox)


## <a name="creating-a-popup-menu"></a>Bir açılan menü oluşturma

İlk adım menüsü için menü kaynak dosyası oluşturma ve yerleştirebilir olarak **kaynakları/menüsü**. Örneğin, aşağıdaki XML önceki ekran görüntüsünde görüntülenen üç öğesi menüsü kodudur **Resources/menu/popup_menu.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/item1"
          android:title="item 1" />
    <item android:id="@+id/item1"
          android:title="item 2" />
    <item android:id="@+id/item1"
          android:title="item 3" />
</menu>
```

Ardından, bir örneğini oluşturmak `PopupMenu` ve kendi görünüm bağlantı. Bir örneğini oluştururken `PopupMenu`, başvuru oluşturucusuna geçirmeniz `Context` , menü bağlı görüntülemenin yanı sıra. Sonuç olarak, açılan menü, yapım sırasında bu görünüme sabitlenmiştir.

Aşağıdaki örnekte, `PopupMenu` düğme için tıklama olay işleyicisine oluşturulur (olarak adlandırılmış `showPopupMenu`). Bu düğme Ayrıca hangi görünümdür `PopupMenu` aşağıdaki kod örneğinde gösterildiği gibi bağlanmıştır:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
};
```

Son olarak, açılan menü olmalıdır *inflated* daha önce oluşturulan menü kaynağı ile. Aşağıdaki örnekte, menünün çağrısı [Inflate](https://developer.xamarin.com/api/member/Android.Views.LayoutInflater.Inflate/p/System.Int32/Android.Views.ViewGroup/) yöntemi eklenir ve kendi [Göster](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Show%28%29/) yöntemi çağrıldığında görüntülemek için:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```


## <a name="handling-menu-events"></a>Menü olaylarını işleme

Kullanıcı bir menü öğesi seçtiğinde [MenuItemClick](https://developer.xamarin.com/api/event/Android.Widget.PopupMenu.MenuItemClick/) tıklayın olay yükseltilir ve menü kapatıldı. Menü dışında herhangi bir yere dokunarak onu yalnızca kapat. Menü kapatıldı, her iki durumda da, [DismissEvent](https://developer.xamarin.com/api/member/Android.Widget.PopupMenu.Dismiss%28%29/) gerçekleştirilecektir. Aşağıdaki kod her ikisi için olay işleyicileri ekler `MenuItemClick` ve `DismissEvent` olayları:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);

    menu.MenuItemClick += (s1, arg1) => {
        Console.WriteLine ("{0} selected", arg1.Item.TitleFormatted);
    };

    menu.DismissEvent += (s2, arg2) => {
        Console.WriteLine ("menu dismissed");
    };
    menu.Show ();
};
```



## <a name="related-links"></a>İlgili bağlantılar

- [PopupMenuDemo (örnek)](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/)
