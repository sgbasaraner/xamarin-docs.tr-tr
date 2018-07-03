---
title: Anahtar
description: Bir Xamarin.Android uygulamasına anahtar pencere kullanma
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/29/2018
ms.openlocfilehash: e3bcce48a675a9ba3d1d41f93babc7fcb26448c8
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403282"
---
# <a name="switch"></a>Anahtar

`Switch` (Aşağıda gösterilen) pencere öğesi iki durum arasında böyle bir şekilde geçiş veya kapalı olanak tanır. `Switch` Varsayılan değer: kapalı. Pencere öğesi içinde her iki bir, açık ve kapalı durumlarını aşağıda gösterilmiştir:

[![Bir anahtar pencere öğesinde KAPATIP durumları ekran görüntüleri](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>Bir anahtar oluşturma

Bir anahtar oluşturmak için yalnızca bildirin bir `Switch` XML öğesinde aşağıdaki gibi:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

Bu, aşağıda gösterildiği gibi temel bir anahtar oluşturur:

[![Bir anahtarı OFF durumda görüntüleme tanıtım uygulamasının ekran görüntüsü](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>Varsayılan değerleri değiştirme

AÇIK ve kapalı durumlarını için Denetim görüntülenen metni hem varsayılan değer yapılandırılabilir özelliktedir. Örneğin, varsayılan şirket için ve Hayır/Evet kapalı/ON yerine okuma geçiş yapmak için biz ayarlayabilirsiniz `checked`, `textOn`, ve `textOff` aşağıdaki XML öznitelikleri.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>Bir başlık sağlama

`Switch` Pencere öğesi de destekler ayarlayarak bir metin etiketi de dahil olmak üzere `text` özniteliğini aşağıdaki gibi:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

Aşağıdaki ekran görüntüsünde, çalışma zamanında bu biçimlendirme oluşturur:

[![Yatay olarak anahtar pencere önceki metinle tanıtım uygulamasının ekran görüntüsü](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

Olduğunda bir `Switch`'s bilmemektedir değeri değiştiğinde bir `CheckedChange` olay.
Örneğin, aşağıdaki kodda Biz bu olayı yakalamak ve mevcut bir `Toast` pencere öğesi ile bir ileti dayalı `isChecked` değerini `Switch`, hangi geçirilen olay işleyicisine bir parçası olarak `CompoundButton.CheckedChangeEventArg` bağımsız değişken.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>İlgili bağlantılar

- [SwitchDemo (örnek)](https://developer.xamarin.com/samples/monodroid/SwitchDemo/)
- [Sekme düzeni Öğreticisi](~/android/user-interface/layouts/tab-layout/index.md)
