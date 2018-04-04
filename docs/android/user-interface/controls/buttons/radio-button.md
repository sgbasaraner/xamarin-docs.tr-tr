---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 1267491f2d9b7519f76651df059722420fa8e1eb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="radiobutton"></a>RadioButton

Bu bölümde, (etkinleştirme bir devre dışı bırakır diğer) iki birbirini dışlayan radyo düğmelerini kullanarak oluşturacağınız [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) ve [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) pencere öğeleri. Her iki radyo düğmesine basıldığında bir bildirim iletisi görüntülenir.


Açık **Resources/layout/Main.axml** dosya ve iki ekleyin [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s, iç içe geçmiş bir [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (içinde [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<RadioGroup
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  <RadioButton android:id="@+id/radio_red"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Red" />
  <RadioButton android:id="@+id/radio_blue"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Blue" />
</RadioGroup>
```

Önemlidir, [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s gruplandırılır birlikte tarafından [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) öğesi böylece aynı anda birden fazla seçilebilir. Bu mantık, Android sistem tarafından otomatik olarak gerçekleştirilir. Bir zaman [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) içinde seçili bir grubu, diğerlerini otomatik olarak seçili olmayan.

Bir şey yapmak için her [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) olduğunu belirlenirse, bir olay işleyicisi yazma için ihtiyacımız:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

İlk olarak, geçirilen gönderen RadioButton atamalısınız.
Ardından bir [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) seçili radyo düğmesinin metin iletisi görüntüler.

Şimdi, ekranın alt kısmındaki [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) yöntemi, aşağıdakileri ekleyin:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

Bu her biri yakalar [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)düzeninden s ve her yeni oluşturulan olay handlerto ekler.

Uygulamayı çalıştırın.

**İpucu:** durumu kendiniz değiştirmeniz gerekiyorsa (ne zaman gibi kaydedilmiş yüklenirken [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)), kullanın [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) özellik ayarlayıcısı veya [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) yöntemi.

*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/). 
