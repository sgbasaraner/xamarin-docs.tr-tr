---
title: RelativeLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c7de56038d5f59c6cff89b87f48535c3e757bb91
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) olan bir [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) alt görüntüleyen [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) göreli konumları öğeler. Konumunu bir [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) Eşdüzey öğeleri olarak (örneğin sol biri için veya belirli bir öğenin altında) göre belirtilen veya içinde için göreli konumlandırır [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) alanı (örn Orta Sol alta hizalı:).

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) ortadan kaldırabilirsiniz çünkü bir kullanıcı arabirimi tasarlama iç içe geçmiş için çok güçlü bir yardımcı programdır [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Kendinizi bulursanız birkaç kullanarak iç içe geçmiş [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) grupları olabilir tek bir tıklatmayla değiştirebilir [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Adlı yeni bir proje başlatın **HelloRelativeLayout**.

Açık **Resources/Layout/Main.axml** dosya ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="fill_parent"
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
Kullanırken bir [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), bu öznitelikler her yerleştirmek istediğiniz nasıl tanımlamak için kullanabileceğiniz [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Bu özniteliklerin her biri farklı türde bir göreli konumunu tanımlayın. Bazı öznitelikler bir eşdüzeyi kaynak Kimliğini kullanan [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) göreli konumunu tanımlamak için. Örneğin, son [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) sol- ve hizalı-ile--üst-için kalan için tanımlanan [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) Kimliğiyle tanımlanan `ok` (önceki olduğu[`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Tüm kullanılabilir düzeni özniteliklerini tanımlanan [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Bu düzende yük emin olun [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) yöntemi:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) Yöntemi yükler için Düzen dosyasını [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), belirtilen kaynak Koduna göre &mdash; `Resource.Layout.Main` başvurduğu **kaynakları/Düzen / Main.AXML** Düzen dosyası.

Uygulamayı çalıştırın. Aşağıdaki Düzen görmeniz gerekir:

[![Bir kutusu TextView, EDITTEXT ve iki düğme göreli bir düzen ekran görüntüsü](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Kaynaklar

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/).
