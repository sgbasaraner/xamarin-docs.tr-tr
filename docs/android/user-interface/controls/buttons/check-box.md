---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85c505d03e7a763b24fb176b6a94c0fe43009b79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765759"
---
# <a name="checkbox"></a>CheckBox

Bu bölümde, kullanarak öğelerini seçmek için bir onay kutusu oluşturacak [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox) pencere öğesi. Onay kutusu basıldığında bir bildirim iletisi onay kutusunu geçerli durumunu gösterir.

Açık **Resources/layout/Main.axml** dosya ve ekleme [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) öğesi (içinde [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

Durumu değiştiğinde bir şeyler için sonuna aşağıdaki kodu ekleyin [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) yöntemi:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

Bu yakalar [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) Düzeni öğesinden sonra onay kutusunu tıklatıldığında yapılacak eylemi tanımlar Click olayını işler. Tıklatıldığında [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) özelliği, onay kutusunu yeni durumunu denetlemek için çağrılır. İade edildi, sonra bir [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) görüntülediği "Seçili" iletisi "Seçili", aksi takdirde görüntüler. [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) Kendi durum değişiklikleri yalnızca geçerli durumunu sorgulamak gereken şekilde işler.

Çalıştırın.

**İpucu:** durumu kendiniz değiştirmeniz gerekiyorsa (ne zaman gibi kaydedilmiş yüklenirken [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference), kullanın [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked) özellik ayarlayıcısı veya [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle) yöntemi.

*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/).
