---
title: Açılır menüsü
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/18/2017
ms.openlocfilehash: e7fad84133ca712c531ab0d12a67db78103c7cdd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763156"
---
# <a name="popup-menu"></a>Açılır menüsü

`PopupMenu` Sınıfı, belirli bir görünüme bağlı açılır menüler görüntüleme desteği ekler. Aşağıdaki çizimde bir düğmesinden yalnızca seçili vurgulu ikinci öğe ile sunulan bir açılır menü gösterir:

 [![Üç adet PopopMenu örneği üç öğeleri](popup-menu-images/20-popupmenu.png)](popup-menu-images/20-popupmenu.png#lightbox)

Android 4 eklenen yeni özellikler için birkaç `PopupMenu` kolaylaştırmak biraz, yani çalışmak:

-   **Inflate** &ndash; Şişir yöntemi artık doğrudan PopupMenu sınıfı üzerinde kullanılabilir durumdadır.
-   **DismissEvent** &ndash; PopupMenu sınıfı şimdi bir DismissEvent sahiptir.

Bu geliştirmeler bir göz atalım. Bu örnekte, bir düğmeyi içeren tek bir etkinlik vardır. Kullanıcı düğmesine tıkladığında, aşağıda gösterildiği gibi açılır menüsü görüntülenir:

 [![Bir öykünücü düğmesi ve 3-item açılır menü ile çalışan uygulama örneği](popup-menu-images/06-popupmenu.png)](popup-menu-images/06-popupmenu.png#lightbox)


## <a name="creating-a-popup-menu"></a>Açılan menü oluşturma

Bir örneğini oluştururken biz `PopupMenu`, başvuru kurucusu geçirmek ihtiyacımız `Context`, menü bağlı görüntülemenin yanı sıra. Bu durumda, oluşturduğumuz `PopupMenu` bizim düğmesi için click olay işleyicisi olarak adlandırılmış `showPopupMenu`.
Bu düğme ayrıca biz ekleme görünümdür `PopupMenu`aşağıdaki kodda gösterildiği gibi:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
}
```

İlk olarak bir başvuru almak Android 3'te, bir XML kaynak menüsünden Şişir için kodu gerekli bir `MenuInflator`ve ardından arama kendi `Inflate` yöntemi menü ve menü örneğe içine Şişir bulunan XML kaynak kimliği. Bu tür bir yaklaşım hala Android 4 ve daha sonra aşağıdaki kodu gösterir olarak çalışır:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.MenuInflater.Inflate (Resource.Menu.popup_menu, menu.Menu);
};
```

İtibariyle Android 4 ancak şimdi çağırabilirsiniz `Inflate` örneği üzerinde doğrudan `PopupMenu`. Bu kod aşağıda gösterildiği gibi daha kısa yapar:

```csharp
showPopupMenu.Click += (s, arg) => {
    PopupMenu menu = new PopupMenu (this, showPopupMenu);
    menu.Inflate (Resource.Menu.popup_menu);
    menu.Show ();
};
```

Yukarıdaki kod menü inflating sonra yalnızca diyoruz `menu.Show` ekranda görüntülemek için.


## <a name="handling-menu-events"></a>Menü olaylarını işleme

Kullanıcı bir menü öğesi seçtiğinde `MenuItemClick` olay harekete geçirilen ve menü kapatılır. Menü dışında herhangi bir yere dokunarak, yalnızca kapatmak. Android menü kapatıldığında, 4'ten itibaren her iki durumda da, `DismissEvent` gerçekleştirilecektir. Aşağıdaki kod olay işleyicileri için her ikisini de ekler `MenuItemClick` ve `DismissEvent` olayları:

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
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
