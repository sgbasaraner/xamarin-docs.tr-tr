---
title: LinearLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: fc6bc9e1d4625f8f45887b0a144a31383046b296
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) olan bir [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) alt görüntüleyen [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) doğrusal bir yönde öğeleri dikey veya yatay olarak.

Aşırı kullanma konusunda dikkatli olmalıdır [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).
İç içe geçmiş birden çok başlarsa [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s, kullanarak düşünmek isteyebilirsiniz bir [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) yerine.

Adlı yeni bir proje başlatın **HelloLinearLayout**.

Açık **Resources/Layout/Main.axml** ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "fill_parent"
      android:layout_height=    "fill_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

Bu XML dikkatle inceleyin. Bir kök [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) yönünü dikey olarak tanımlayan &ndash; tüm alt [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)(hangi iki var) s olacaktır Yığılmış dikey olarak. Başka bir öğenin ilk alt olan [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) yatay yönlendirme kullanan ve ikinci alt olduğu bir [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) dikey yönlendirme kullanır. Bunların her biri iç içe geçmiş [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s içeren birkaç [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) birbirleri ile kendi üst tarafından tanımlanan şekilde yönlendirilmiş öğeleri [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).

Şimdi açmak **HelloLinearLayout.cs** ve yüklerken emin **Resources/Layout/Main.axml** düzende [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) yöntemi:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) Yöntemi yükler için Düzen dosyasını [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), belirtilen kaynak Koduna göre &ndash; `Resources.Layout.Main` başvurduğu **kaynakları/Düzen / Main.AXML** Düzen dosyası.

Uygulamayı çalıştırın. Şunları görmeniz gerekir:

[![Ekran ilk LinearLayout yatay olarak düzenlenmiş uygulamasının, dikey olarak ikinci](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

XML öznitelikleri her görünümün davranışı nasıl tanımlamak dikkat edin. Farklı değerlere sahip gözlemleyin `android:layout_weight` ekran Gayrimenkul nasıl dağıtıldığını görmek için her öğe ağırlığını tabanlı. Bkz: [ortak yerleşim nesneleri](http://developer.android.com/guide/topics/ui/declaring-layout.html) belge hakkında daha fazla bilgi için [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) tanıtıcıları `android:layout_weight` özniteliği.


## <a name="references"></a>Referanslar

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/).

