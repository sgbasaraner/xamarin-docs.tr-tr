---
title: Metni Düzenle
description: Kullanıcı girişi kabul edecek şekilde EDITTEXT pencere kullanma
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/09/2018
ms.openlocfilehash: bb2cb13472e7e17eb1b0438ed67033f2b04defe2
ms.sourcegitcommit: b6f3e55d4f3dcdc505abc8dc9241cff0bb5bd154
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780671"
---
# <a name="edit-text"></a>Metni Düzenle

Bu bölümde, kullanacağınız [EDITTEXT](https://developer.xamarin.com/api/type/Android.Widget.EditText/) pencere öğesini kullanarak kullanıcı girişi için bir metin alanı oluşturun. Metin alanına girilen sonra **Enter** anahtarı bir bildirim iletisi metni görüntülenir.

Açık **Resources/layout/activity_main.axml** ve ekleme [EDITTEXT](https://developer.xamarin.com/api/type/Android.Widget.EditText/) içeren düzen öğesi. Aşağıdaki örnek **activity_main.axml** sahip bir `EditText` için eklenmiş bir `LinearLayout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <EditText
        android:id="@+id/edittext"
        android:layout_width="match_parent"
        android:imeOptions="actionGo"
        android:inputType="text"
        android:layout_height="wrap_content" />
</LinearLayout>
```

Bu kod örneğinde `EditText` özniteliği `android:imeOptions` ayarlanır `actionGo`. Bu ayar varsayılan değişiklikleri [Bitti](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE) eyleme [Git](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO) eylem dokunarak böylece **Enter** anahtar Tetikleyicileri `KeyPress` giriş işleyicisi.
(Genellikle `actionGo` kullanılan böylece **Enter** anahtar alan kullanıcı tarafından yazılmış bir URL hedefi.)

Kullanıcı metin girişini işlemek için sonuna aşağıdaki kodu ekleyin [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) yönteminde **MainActivity.cs**:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
    e.Handled = false;
    if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) 
    {
        Toast.MakeText(this, edittext.Text, ToastLength.Short).Show();
        e.Handled = true;
    }
};
```

Ayrıca, aşağıdaki ekleyin `using` üstüne deyimi **MainActivity.cs** zaten mevcut değilse:

```csharp
using Android.Views;
```

Bu kod örneği Şişir [EDITTEXT](https://developer.xamarin.com/api/type/Android.Widget.EditText/) öğesi düzeninden ve ekler bir [tuş basışı](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) Pencere odağı alırken bir tuşa basıldığında yapılacak eylemi tanımlayan bir işleyici. Bu durumda, yöntem dinlemek için tanımlanan **Enter** anahtar (dokunduğunuzda) ve ardından açılır bir [bildirim](https://developer.xamarin.com/api/type/Android.Widget.Toast/) girilmiş metinle ileti. Unutmayın [işlenmiş](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) özelliği her zaman olmalıdır `true` olay işlenmişse. Bu gelen tırmanma olay önlemek gereklidir (Bu bir satır başı metin alanına neden olur) yukarı.

Uygulamayı çalıştırmak ve metin alanına metin girin. Bastığınızda **Enter** anahtar, bildirim sağ tarafta gösterildiği gibi görüntülenir:

[![Metin EDITTEXT girerek örnekleri](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*Bu sayfanın bölümleri üzerinde oluşturulan iş tabanlı değişiklikleri olan ve* [ *Android açık kaynak projesi tarafından paylaşılan* ](http://code.google.com/policies.html) *ve içindeşartlargörekullanılan* [ *2.5 creative Commons Attribution lisans* ](http://creativecommons.org/licenses/by/2.5/) *. Bu öğreticide dayanır* [ *Android Form öğe öğretici* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*


## <a name="related-links"></a>İlgili bağlantılar

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
