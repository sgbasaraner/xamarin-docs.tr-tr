---
title: Özel düğme
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: e6fc3fe4c3cb89d74188557615f58cc8e34f5991
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766575"
---
# <a name="custom-button"></a>Özel düğme

Bu bölümde, bir metin yerine özel bir görüntü ile bir düğme oluşturacak kullanarak [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) pencere öğesi ve farklı düğme durumları için kullanılacak üç farklı görüntüleri tanımlayan bir XML dosyası. Düğmeye basıldığında kısa bir ileti görüntülenir.

Sağ tıklatın ve aşağıdaki üç görüntüleri indirin, sonra kopyalamak için **kaynakları/drawable** projenizin dizin. Bunlar farklı düğme durumları için kullanılır.

 [![Normal durum yeşil Android simgesi](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![odaklanmış durum turuncu Android simgesi](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [ ![sarı Android basılı durum simgesi](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

Yeni bir dosyada oluşturmak **kaynakları/drawable** adlı dizin **android_button.xml**. Aşağıdaki XML Ekle:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

Bu düğme geçerli durumuna göre görüntüsünü değiştirir tek bir drawable kaynak tanımlar. İlk `<item>` tanımlar **android_pressed.png** düğmesine basıldığında görüntü olarak (etkinleştirilmiş); ikinci `<item>` tanımlar **android_focused.png** görüntüsü olarak olduğunda (düğme topu veya tek yönlü panelini kullanarak vurgulanır olduğunda) düğmesini odaklanmıştır; ve üçüncü `<item>` tanımlar **android_normal.png** (basılı ne odaklanmış olduğunda) normal durum görüntüsü olarak. Bu XML dosyası artık tek bir drawable kaynak temsil eder ve başvurduğu bir [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) , arka plan için görüntülenen resmi tabanlı üzerinde üç bu durumu değişir.


> [!NOTE]
> Sırasını `<item>` öğeleri önemlidir. Bu drawable başvurulduğunda `<item>`s hangisinin geçerli düğmesi durumu için uygun olduğunu belirlemek için sıralı geçiş.
> "Normal" resmi son olduğundan, yalnızca uygulanan olduğunda olan koşullar `android:state_pressed` ve `android:state_focused` her ikisi de false değerlendirilir.

Açık **Resources/layout/Main.axml** dosya ve ekleme [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) öğe:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

`android:background` Özniteliği düğmesi arka planı için kullanılacak drawable kaynak belirtir (hangi konumunda kaydedildiğinde **Resources/drawable/android.xml**, olarak başvurulan `@drawable/android`). Bu, Sistem genelindeki düğmeleri için kullanılan normal arka plan görüntüsü yerini alır. Drawable düğmesi durumuna bağlı görüntüsünü değiştirmek için sırayla görüntü arka planı uygulanmış olması gerekir.

Düğmeye basıldığında şeyler yapmak için sonuna aşağıdaki kodu ekleyin [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/) yöntemi:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Bu yakalar [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) düzeninden sonra ekler bir [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) zaman görüntülenecek ileti [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) tıklandığında.

Şimdi uygulamayı çalıştırın.


*Bu sayfayı bölümlerini olan oluşturulan ve Android açık kaynak projesi tarafından paylaşılan ve açıklanan terimleri göre kullanılan iş göre değişiklikler*
[*Creative Commons 2.5 Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/).
