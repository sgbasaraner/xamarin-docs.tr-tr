---
title: "Uyarıları görüntüleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 09f4178d5d6e12388771fa63875fbe1f489c959a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="displaying-alerts"></a>Uyarıları görüntüleme

İOS 8 ile başlayarak, UIAlertController tamamlanmış değiştirilen UIActionSheet sahiptir ve UIAlertView hem de artık kullanım.

UIView alt sınıflarının olan yerine, sınıfları UIAlertController UIViewController sınıfıdır.

Kullanım `UIAlertControllerStyle` görüntülemek için uyarı türünü belirtmek için. Bu uyarı türleri şunlardır:

- **UIAlertControllerStyleActionSheet**
    * Ön iOS 8 Bu bir UIActionSheet olabilirdi
- **UIAlertControllerStyleAlert**
    * Ön iOS 8 Bu UIAlertView olabilirdi 

Bir uyarı denetleyicisi oluştururken almak için gereken üç adım vardır:

- Oluşturma ve uyarı şununla yapılandırma
    * title
    * iletisi
    * preferredStyle
    
- (İsteğe bağlı) Bir metin alanı Ekle
- Gerekli eylemleri ekleme
- Görünüm denetleyicisi sunar

En basit uyarı bu ekran görüntüsünde gösterildiği gibi tek bir düğme içerir:

 ![Bir düğme uyar](alerts-images/alert1.png)

Basit bir uyarı görüntülemek üzere kod aşağıdaki gibidir:

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

Birden fazla seçeneği olan bir uyarı görüntüleme benzer bir şekilde yapılır, ancak iki eylemler ekleyebilirsiniz. Örneğin, aşağıdaki ekran görüntüsünde iki düğme olan bir uyarı gösterir:

 ![ İki düğmeleriyle uyar](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

Uyarılar ayrıca aşağıdaki ekran görüntüsüne benzer bir eylem sayfası görüntüleyebilirsiniz:

 ![Eylem Sayfası Uyarısı](alerts-images/alert3.png)

Düğmeleri, uyarı ile eklenir `AddAction` yöntemi:

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Three pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>İlgili bağlantılar

- [Denetimleri (örnek)](https://developer.xamarin.com/samples/Controls/)
- [Uyarı denetleyicisi](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
