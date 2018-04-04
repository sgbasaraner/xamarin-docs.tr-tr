---
title: Anahtar
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0f4bfc3646f1ccd956ee8151468b3de20f6e1e2b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="switch"></a>Anahtar

`Switch` Pencere öğesi (aşağıda gösterilen) iki durumlu arasında böyle bir birimi geçiş veya kapalı kullanıcıya izin verir. `Switch` Varsayılan değerdir kapalı. Pencere öğesi varsayılan olarak, her iki kendi açık ve kapalı durumda aşağıda gösterilmiştir:

[![Bir anahtar pencere öğesinde KAPATIP durumları ekran görüntüleri](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Bir anahtar oluşturma

Bir anahtar oluşturmak için yalnızca bildirme bir `Switch` XML öğesinde şekilde:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Aşağıda gösterildiği gibi temel bir anahtar oluşturur:

[![Bir anahtarı OFF durumda görüntüleme demo uygulamasının ekran görüntüsü](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Varsayılan değerlerini değiştirme

AÇIK ve kapalı durumları için denetimi görüntülenen metni ve varsayılan değeri yapılandırılabilir. Örneğin, varsayılan ON ve kapalı/ON yerine Hayır/Evet okuyun geçiş yapmak için biz ayarlayabilirsiniz `checked`, `textOn`, ve `textOff` aşağıdaki XML öznitelikleri.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>Başlık sağlama

`Switch` Pencere öğesi de destekler ayarlayarak bir metin etiketi de dahil olmak üzere `text` özniteliğini aşağıdaki gibi:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Bu biçimlendirme, aşağıdaki ekran görüntüsünde çalışma zamanında üretir:

[![Yatay anahtar pencere öğesi önceki metinle demo uygulamasının ekran görüntüsü](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Zaman bir `Switch`ait değer değişiklikleri yükseltir bir `CheckedChange` olay.
Örneğin, aşağıdaki kodda Biz bu olayı yakalamak ve mevcut bir `Toast` pencere öğesi bir iletiyle dayalı olarak `isChecked` değerini `Switch`, hangi geçirilen olay işleyicisine bir parçası olarak `CompoundButton.CheckedChangeEventArg` bağımsız değişkeni.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>İlgili bağlantılar

- [SwitchDemo (örnek)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/SwitchDemo/)
- [Sekme düzeni Öğreticisi](~/android/user-interface/layouts/tab-layout/index.md)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
