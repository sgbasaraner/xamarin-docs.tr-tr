---
title: TableLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 80147de0f639b4b597e11b41a6854f550edd61a9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="tablelayout"></a>TableLayout

[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) olan bir [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) alt görüntüleyen [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) satırları ve sütunları öğe.

Adlı yeni bir proje başlatın **HelloTableLayout**.

Açık **Resources/Layout/Main.axml** dosya ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

Bu bir HTML tablosunun yapısı nasıl benzer dikkat edin. [ `TableLayout` ](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) Öğesidir gibi HTML `<table>` öğesi; [ `TableRow` ](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) benzer bir `<tr>` öğesi; ancak hücreler için herhangi bir tür kullanabilirsiniz [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) öğesi. Bu örnekte, bir [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) her bir hücre için kullanılır. Bazı satırlar Between bulunmaktadır temel [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/), yatay çizgi çizmek için kullanılan.

Emin olun, **HelloTableLayout** etkinlik yükler Bu düzende [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) yöntemi:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) Yöntemi yükler için Düzen dosyasını [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), belirtilen kaynak Koduna göre &mdash; `Resource.Layout.Main` başvurduğu **kaynakları/Düzen / Main.AXML** Düzen dosyası.

Uygulamayı çalıştırın. Şunları görmeniz gerekir:

[![Örnek uygulamasının ekran görüntüsü birden çok tablo satırı görüntüleme TableLayout](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png)


<a name="References" />

## <a name="references"></a>Referanslar

-   [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 

-   [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 

-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/).
