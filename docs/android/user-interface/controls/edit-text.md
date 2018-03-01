---
title: "Metin düzenleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 280bb1ad8f717476ec425462a43a32c1f3eac381
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="edit-text"></a>Metin düzenleme

Bu bölümde, bir metin alanı kullanarak kullanıcı girişi için oluşturacağınız [EDITTEXT](https://developer.xamarin.com/api/type/Android.Widget.EditText/) pencere öğesi. Metin alanına girilen sonra "Enter" tuşuna metni bir bildirim iletisi görüntüler.

Açık <code>Resources\layout\main.xml</code> dosya ve ekleme [EDITTEXT](https://developer.xamarin.com/api/type/Android.Widget.EditText/) öğesi (içinde [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<pre><code class=" syntax brush-C#">&lt;EditText
    android:id="@+id/edittext"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"/&gt;</code></pre>
```

Kullanıcı türleri sonuna aşağıdaki kodu ekleyin metinle birlikte bir şeyler için [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) yöntemi:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
 e.Handled = false;
 if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) {
  Toast.MakeText (this, edittext.Text, ToastLength.Short).Show ();
  e.Handled = true;
         }
};
```

Bu yakalar [ `EditText` ](https://developer.xamarin.com/api/type/Android.Widget.EditText/) kümeleri ve Düzen öğesinden bir [ `KeyPress` ](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) pencere öğesi odağa sahipken bir tuşuna basıldığında yapılacak eylemi tanımlar işleyici. Bu durumda, yöntemi (aşağı basıldığında) Enter tuşuna dinler ve ardından açılır için tanımlanmış bir bir [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) girilen metin iletisi. [ `Handled` ](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) Özelliği uymanız gereken `true` değil böylece olay Kabarcık (hangi satır başı metin alanına neden olacağından) li olay, işlenene durumunda.

Uygulamayı çalıştırın.

*Bu sayfayı bölümlerini oluşturulan çalışmaları temel değişiklikler olduğundan ve* [ *Android açık kaynak projesi tarafından paylaşılan* ](http://code.google.com/policies.html) *veaçıklananterimlerigörekullanılan* [ *2.5 creative Commons Attribution lisansı* ](http://creativecommons.org/licenses/by/2.5/) *. Bu öğretici dayanır* [ *Android Form şeyler öğretici* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*



## <a name="related-links"></a>İlgili bağlantılar

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
