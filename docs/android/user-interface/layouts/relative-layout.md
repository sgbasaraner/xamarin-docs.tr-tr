---
title: Xamarin.Android RelativeLayout kullanma
description: Bir Xamarin.Android uygulamasına RelativeLayout kullanma
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/29/2018
ms.openlocfilehash: af8d37775a798fc6019106a66df75843a951c108
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403422"
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) olan bir [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) alt görüntüleyen [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) göreli konumlarını öğeleri. Konumu bir [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) göre Eşdüzey öğeleri olarak (örneğin seçeceğine sol, ya da belirli bir öğeyi altında) belirtilen veya içinde göre konumlandırır [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) alanı (örn Orta sol altına aligned).

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) ortadan kaldırabilir çünkü bir kullanıcı arabirimi tasarlama iç içe geçmiş çok güçlü bir aracıdır [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Kendiniz görürseniz, birkaç kullanarak iç içe [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) grupları olabilir bunları tek bir değiştirebilirsiniz [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Adlı yeni bir proje başlatın **HelloRelativeLayout**.

Açık **Resources/Layout/Main.axml** dosya ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background"
        android:layout_below="@id/label"/>
    <Button
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/entry"
        android:layout_alignParentRight="true"
        android:layout_marginLeft="10dip"
        android:text="OK" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toLeftOf="@id/ok"
        android:layout_alignTop="@id/ok"
        android:text="Cancel" />
</RelativeLayout>
```

Her bir fark `android:layout_*` gibi öznitelikleri `layout_below`, `layout_alignParentRight`, ve `layout_toLeftOf`.
Kullanırken bir [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), bu öznitelikleri her yerleştirmek istediğiniz nasıl tanımlamak için kullanabileceğiniz [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Bu özniteliklerin her biri farklı türde bir göreli konumunu tanımlayın. Bazı öznitelikler bir eş kaynak Kimliğini kullanın [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) göreli konumunu tanımlamak için. Örneğin, son [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) sol-of ve hizalanmış-ile--top-of yattığı tanımlanan [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) kimliğiyle `ok` (önceki olduğu[`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Tüm kullanılabilir Düzen özniteliklerini tanımlanan [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Bu düzende yük emin [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) yöntemi:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) Yöntemi için yerleşim dosyası yükler [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), kaynak kimliği tarafından belirtilen &mdash; `Resource.Layout.Main` başvurduğu **kaynakları/Düzen / Main.AXML** Düzen dosyası.

Uygulamayı çalıştırın. Aşağıdaki Düzen görmeniz gerekir:

[![Bir TextView EDITTEXT ve iki düğme ile göreli bir düzeni görüntüsü](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Kaynaklar

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Bu sayfanın bölümleri olan oluşturulan Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş tabanlı değişiklikleri*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).
