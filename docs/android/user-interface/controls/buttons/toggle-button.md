---
title: ToggleButton
ms.topic: article
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 9e1e9711d218f4f4be825ff223b650ae932ad041
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="togglebutton"></a>ToggleButton

Bu bölümde, özellikle kullanan iki durumlu arasında geçiş için kullanılan bir düğme oluşturacaksınız [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) pencere öğesi. Birbirini dışlayan iki basit durumlar varsa, bu pencere öğesi radyo düğmeleri için mükemmel bir alternatif olan ("açık" ve "kapalı" gibi). Android 4.0 (API düzeyi 14) olarak bilinen iki durumlu düğme alternatif sunulan bir [ `Switch` ](https://developer.xamarin.com/api/type/Android.Widget.Switch/).

Örnek olarak bir **ToggleButton** görüntüleri sağ taraftaki çiftinin bir örneğini gösterir ancak görüntüleri, sol taraftaki çiftinin görülebilir bir **anahtar**:

![Anahtarlar ve ToggleButtons örnekleri hem de açma ve kapatma durumları](toggle-button-images/togglebutton-switch.png)  

Uygulama kullanan denetleyen stili bir konudur. Her iki pencere öğeleri işlevsel olarak eşdeğerdir.

Açık **Resources/layout/Main.axml** dosya ve ekleme [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) öğesi (içinde [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

Durumu değiştiğinde bir şeyler için sonuna aşağıdaki kodu ekleyin [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) yöntemi:

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

Bu yakalar [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) Düzeni öğesinden ve düğmesine tıklandığında gerçekleştirilecek eylemi tanımlar Click olayını işler. Bu örnekte, yöntem düğmesi yeni durumunu denetler ve ardından gösterir bir [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) ileti geçerli durumunu gösterir.

Dikkat [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) tanıtıcıları kendi durumunu, checked ve unchecked, arasında etkindir yalnızca isteyin şekilde değiştirin.

Uygulamayı çalıştırın.


**İpucu:** durumu kendiniz değiştirmeniz gerekiyorsa (ne zaman gibi kaydedilmiş yüklenirken [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), kullanın [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) özellik ayarlayıcısı veya [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) yöntemi.


## <a name="related-links"></a>İlgili bağlantılar

- [ToggleButton](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [geçiş](http://developer.android.com/reference/android/widget/Switch.html)
