---
title: "Gezinti çubuğu"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 396ed31cba336976342a8dfb26f31eeda20cf494
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="navigation-bar"></a>Gezinti çubuğu

Android 4 adlı yeni bir sistem kullanıcı arabirimi özellik tanıtılan bir *gezinti çubuğu*, Gezinti denetimleri için donanım düğmeleri içerme cihazlarda sağlayan **giriş**, **geri** , ve **menü**.
Aşağıdaki ekran görüntüsü Nexus asal aygıttan gezinti çubuğu gösterir:

 [ ![Bir Android gezinti çubuğu örneği](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png)

Birkaç yeni bayrakları kullanılabilir gezinti çubuğu ve denetimlerinin görünürlüğünü yanı sıra, sistem Android 3'te tanıtılan çubuğunun görünürlüğünü denetleme. Flags içinde tanımlanan `Android.View.View` sınıfı ve aşağıda listelenmiştir:

-   `SystemUiFlagVisible` &ndash; Gezinti çubuğunda görünür hale getirir. 
-   `SystemUiFlagLowProfile` &ndash; DIMS Gezinti çubuğundaki denetimleri. 
-   `SystemUiFlagHideNavigation` &ndash; Gezinti çubuğu gizler. 


Bu bayrakların görünüm hiyerarşideki herhangi bir görünüm ayarlayarak uygulanabilir `SystemUiVisibility` özelliği. Birden çok görünüm bu özelliği ayarlanmış varsa, sistem veya işlemi ile birleştirir ve bayrakları ayarlanmış olan penceresi odağını korur sürece uygular. Bir görünüm kaldırdığınızda, bu ayarlanmış herhangi bir bayrağı da kaldırılacak.

Aşağıdaki örnek, burada herhangi bir düğmeyi tıklatarak değişiklikleri basit bir uygulama gösterir `SystemUiVisibility`:

 [ ![Görünür, düşük profili ve gizli SystemUiVisibility gösteren ekran görüntüsü](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png)

Değiştirmek için kodu `SystemUiVisibility` özelliği ayarlar bir `TextView` her düğmenin olay işleyicisi aşağıda gösterildiği gibi tıklayın:

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);
           
lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};
           
hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};
           
visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

Ayrıca, bir `SystemUiVisibility` başlatır değiştirme bir `SystemUiVisibilityChange` olay. Ayar'olduğu gibi `SystemUiVisibility` özelliği, bir işleyici `SystemUiVisibilityChange` olay, hiyerarşideki herhangi bir görünüm için kaydedilebilir. Örneğin, aşağıdaki kodu kullandığı `TextView` olayı için kaydetmek için örnek:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>İlgili bağlantılar

- [SystemUIVisibilityDemo (örnek)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
