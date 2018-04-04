---
title: Uyarılarla çalışma
description: Bu makalede kullanıcı Xamarin.tvOS için bir uyarı iletisi görüntülenecek UIAlertController çalışmak kapsar.
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: da4d2d952687c7e39276ca76af413b83c4519eea
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-alerts"></a>Uyarılarla çalışma

_Bu makalede kullanıcı Xamarin.tvOS için bir uyarı iletisi görüntülenecek UIAlertController çalışmak kapsar._


TvOS kullanıcı dikkatini veya (örneğin, bir dosyayı silmek) zararlı bir eylemi gerçekleştirmek için izni isteyin gerekiyorsa, kullanarak bir uyarı iletisi sunabilir `UIAlertViewController`:

[![](alerts-images/alert01.png "Bir örnek UIAlertViewController")](alerts-images/alert01.png#lightbox)

İleti görüntüleme yanı sıra, düğmeler ve metin alanları eylemlerine yanıt ve geri bildirim sağlamak kullanıcının izin vermek için bir uyarı ekleyebileceğiniz durumunda.

<a name="About-Alerts" />

## <a name="about-alerts"></a>Uyarılar hakkında

Yukarıda belirtildiği gibi uyarıları kullanıcının dikkatini ve bunları app veya istek görüşlerinizi durumunu bildirmek için kullanılır. Uyarıları bir başlık sunması gerekir, bunlar isteğe bağlı olarak bir ileti ve bir veya daha fazla düğme veya metin alanları olabilir.

[![](alerts-images/alert04.png "Bir örnek uyarı")](alerts-images/alert04.png#lightbox)

Apple uyarıları ile çalışmak için aşağıdaki önerileri vardır:

- **Uyarıları tutumlu kullanmak** -uyarıları uygulama ile kullanıcının akışını kesintiye ve kullanıcı deneyimini kesme ve bu nedenle, yalnızca hata bildirimleri ve uygulama içi satın almalara bozucu eylemler gibi önemli durumlar için kullanılmalıdır. 
- **Yararlı seçimler sağlar** - uyarı kullanıcıya seçenekleri sunuyorsa, her seçenek kritik bilgiler sunar ve kullanıcının almak faydalı Eylemler sağlar emin olmalısınız.

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>Uyarı başlıkları ve iletileri

Apple bir uyarının başlık ve isteğe bağlı bir ileti sunmak için aşağıdaki önerileri vardır:

- **Multiword başlıklarını kullanın** -bir uyarı'nın başlığı noktası durumunun hala basit kaldığı sürece açıkça almak. Tek sözcüklük başlık nadiren yeterli bilgi sağlar.
- **Bir ileti gerektirmeyen açıklayıcı başlıklarını kullanın** - mümkün olduğunda, uyarının başlık açıklayıcı isteğe bağlı ileti metni olmayan yeterli gerekli yapmayı düşünün. 
- **İletinin kısa, tam bir cümle sağlamak** - isteğe bağlı bir ileti üzerinde uyarı puan alırsınız, olabildiğince basit tutmak ve doğru büyük/küçük harf ve noktalama işaretleri içeren tam bir cümle yapmak için gereklidir.

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>Uyarı düğmeleri

Apple düğmeleri için bir uyarı eklemek için aşağıdaki önerisi vardır:

- **İki düğme sınırla** - mümkün olduğunda, en fazla iki düğme uyarıya sınırlayın. Bilgiler, ancak hiçbir eylem tek düğmesi uyarılar sağlar. İki düğmesi uyarıları sağlayan basit bir seçenek Evet/Hayır eylem.
- **Succinct, mantıksal düğmesi başlıkları kullanmak** -açıkça düğmenin eylemi açıklayan bir veya iki word düğmesi başlıkları iş en iyi basit. Daha fazla bilgi için bizim [düğmelerle çalışma](~/ios/tvos/user-interface/buttons.md) belgeleri.
- **Açıkça işareti bozucu düğmeleri** - (örneğin, bir dosyayı silmek) zararlı bir eylemi gerçekleştirmek düğmeleri açıkça bunlarla işaretlemek için `UIAlertActionStyle.Destructive` stili.

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>Bir uyarı görüntüleme

Bir uyarı görüntülemek için bir örneğini oluşturun. `UIAlertViewController` ve eylemleri (düğme) ekleme ve uyarı stilini seçerek yapılandırın. Örneğin, aşağıdaki kodu Tamam/iptal uyarıyı görüntüler:

```csharp
const string title = "A Short Title is Best";
const string message = "A message should be a short, complete sentence.";
const string acceptButtonTitle = "OK";
const string cancelButtonTitle = "Cancel";
const string deleteButtonTitle = "Delete";
...
        
var alertController = UIAlertController.Create (title, message, UIAlertControllerStyle.Alert);

// Create the action.
var acceptAction = UIAlertAction.Create (acceptButtonTitle, UIAlertActionStyle.Default, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

var cancelAction = UIAlertAction.Create (cancelButtonTitle, UIAlertActionStyle.Cancel, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

// Add the actions.
alertController.AddAction (acceptAction);
alertController.AddAction (cancelAction);
PresentViewController (alertController, true, null);
```

Bir bu kodu ayrıntılı olarak bakalım. İlk olarak, yeni bir uyarı verilen başlık ve ileti oluşturuyoruz:

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

Ardından, size uyarıda görüntülemek istediğiniz her düğme için biz düğmesi, kendi stil ve düğmesine basıldığında varsa yapılacak istiyoruz eylem başlığı tanımlayan bir eylem oluşturun:

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ => 
    // Do something when the button is pressed
    ...
);
```

`UIAlertActionStyle` Enum düğmesinin stili aşağıdakilerden biri olarak ayarlamanızı izin ver:

- **Varsayılan** -düğme uyarı gösterildiğinde seçilen varsayılan düğme olacaktır.
- **İptal** -uyarı iptal düğmesi düğme vardır.
- **Bozucu** -dosya silme gibi zararlı bir eylem olarak düğmesi vurgular. Şu anda tvOS bozucu düğmesi kırmızı bir arka plan ile işler.

`AddAction` Yöntemi, belirtilen eylem ekler `UIAlertViewController` ve son olarak `PresentViewController (alertController, true, null)` yöntemi kullanıcıya verilen uyarısını gösterir.

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>Metin alanları ekleme

Eylemler (düğme) için uyarı ekleme ek olarak, kullanıcı kimliklerini ve parolaları gibi bilgileri doldurmak izin vermek için uyarı metin alanları ekleyebilirsiniz:

[![](alerts-images/alert02.png "Bir uyarı metin alanı")](alerts-images/alert02.png#lightbox)

Kullanıcı metin alanı seçerse, standart tvOS klavye alan için bir değer girmesini vermeden görüntülenir:

[![](alerts-images/alert03.png "Metin girme")](alerts-images/alert03.png#lightbox)

Aşağıdaki kod bir Tamam/iptal uyarı tek bir metin alanı ile bir değer girmek için görüntüler:

```csharp
UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
UITextField field = null;

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;

    // Initialize field
    field.Placeholder = placeholder;
    field.Text = text;
    field.AutocorrectionType = UITextAutocorrectionType.No;
    field.KeyboardType = UIKeyboardType.Default;
    field.ReturnKeyType = UIReturnKeyType.Done;
    field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

});

// Add cancel button
alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
    // User canceled, do something
    ...
}));

// Add ok button
alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
    // User selected ok, do something
    ...
}));

// Display the alert
controller.PresentViewController(alert,true,null);
```

`AddTextField` Yöntemi yeni bir metin alanı metin (alanın boş olduğunda görüntülenen metin), varsayılan metin değeri ve klavye türünü yer tutucu gibi özellikleri ayarlayarak ardından yapılandırabilirsiniz uyarıya ekler. Örneğin:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

Daha sonra metin alanının değeri çalışabilmek için böylece biz de bir kopyasını aşağıdaki kodu kullanarak kaydediyorsanız:

```csharp
UITextField field = null;
...

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;
    ...
});
```

Kullanıcı bir değer metin alanına girdikten sonra biz kullanabilirsiniz `field` bu değeri erişmek için değişkeni.

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>Uyarı görünümü Denetleyicisi yardımcı sınıfı

Basit, ortak kullanarak uyarı türlerini görüntülemek için `UIAlertViewController` yinelenen kod oldukça bit neden olabilir, yinelenen kod miktarını azaltmak için bir yardımcı sınıfı kullanabilirsiniz. Örneğin:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;

namespace UIKit
{
    /// <summary>
    /// Alert view controller is a reusable helper class that makes working with <c>UIAlertViewController</c> alerts
    /// easier in a tvOS app.
    /// </summary>
    public class AlertViewController
    {
        #region Static Methods
        public static UIAlertController PresentOKAlert(string title, string description, UIViewController controller) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Configure the alert
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(action) => {}));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentOKCancelAlert(string title, string description, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentDestructiveAlert(string title, string description, string destructiveAction, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create(destructiveAction,UIAlertActionStyle.Destructive,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentTextInputAlert(string title, string description, string placeholder, string text, UIViewController controller, AlertTextInputDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
            UITextField field = null;

            // Add and configure text field
            alert.AddTextField ((textField) => {
                // Save the field
                field = textField;

                // Initialize field
                field.Placeholder = placeholder;
                field.Text = text;
                field.AutocorrectionType = UITextAutocorrectionType.No;
                field.KeyboardType = UIKeyboardType.Default;
                field.ReturnKeyType = UIReturnKeyType.Done;
                field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

            });

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false,"");
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null && field !=null) {
                    action(true, field.Text);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }
        #endregion

        #region Delegates
        public delegate void AlertOKCancelDelegate(bool OK);
        public delegate void AlertTextInputDelegate(bool OK, string text);
        #endregion
    }
}
```

Bu sınıf kullanarak, görüntüleme ve basit uyarılara yanıt verme gibi yapılabilir:

```csharp
#region Custom Actions
partial void DisplayDestructiveAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentDestructiveAlert("A Short Title is Best","The message should be a short, complete sentence.","Delete",this, (ok) => {
        Console.WriteLine("Destructive Alert: The user selected {0}",ok);
    });
}

partial void DisplayOkCancelAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKCancelAlert("A Short Title is Best","The message should be a short, complete sentence.",this, (ok) => {
        Console.WriteLine("OK/Cancel Alert: The user selected {0}",ok);
    });
}

partial void DisplaySimpleAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKAlert("A Short Title is Best","The message should be a short, complete sentence.",this);
}

partial void DisplayTextInputAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentTextInputAlert("A Short Title is Best","The message should be a short, complete sentence.","placeholder", "", this, (ok, text) => {
        Console.WriteLine("Text Input Alert: The user selected {0} and entered `{1}`",ok,text);
    });
}
#endregion
```


<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede ele alınan çalışmak `UIAlertController` Xamarin.tvOS kullanıcıya bir uyarı iletisi görüntülenecek. İlk olarak, basit bir uyarı görüntüler ve düğmeleri eklemek nasıl oluşturulacağını gösterir. Ardından, bir uyarı metin alanları eklemek nasıl oluşturulacağını gösterir. Son olarak, bir yardımcı sınıfı bir uyarı görüntülemesi gereken yinelenen kod miktarını azaltmak için nasıl kullanılacağı gösterilmiştir.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
