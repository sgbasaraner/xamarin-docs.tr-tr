---
title: Xamarin.iOS uyarıları görüntüleme
description: Bu belge, iOS 8'de sunulan API'ler UIAlertController kullanarak Xamarin.ios'ta uyarı görüntüleyecek şekilde açıklar.
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 788e62b30dbf533df059b0c3805e04ecf7b857aa
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241346"
---
# <a name="displaying-alerts-in-xamarinios"></a>Xamarin.iOS uyarıları görüntüleme

İOS 8 ile başlayarak, tamamlanan değiştirilen UIActionSheet UIAlertController sahip ve UIAlertView hem biri artık kullanım dışı bırakılmıştır.

Adornerset'in alt Uıview olan değiştirildi, sınıflardan farklı olarak UIAlertController bir uıviewcontroller'ı sınıfıdır.

Kullanım `UIAlertControllerStyle` görüntülenecek uyarı türünü belirtmek için. Bu uyarı türleri şunlardır:

- **UIAlertControllerStyleActionSheet**
    * Öncesi iOS 8 bir UIActionSheet olabilirdi
- **UIAlertControllerStyleAlert**
    * Öncesi iOS 8 UIAlertView olabilirdi 

Olduğunda bir uyarı denetleyicisi oluşturmak için gereken üç adım vardır:

- Oluşturma ve şununla uyarı yapılandırma
    * Başlık
    * iletisi
    * preferredStyle
    
- (İsteğe bağlı) Bir metin alanı ekleyin
- Gerekli eylemleri ekleme
- Görünüm denetleyicisi var

En basit uyarı, bu ekran görüntüsünde gösterildiği gibi tek bir düğmeye içerir:

 ![Bir düğmeye sahip uyarı](alerts-images/alert1.png)

Basit bir uyarı görüntülemek için kod aşağıdaki gibidir:

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

Birden fazla seçenek içeren bir uyarı görüntüleme benzer şekilde yapılır, ancak iki eylemler ekleyin. Örneğin, aşağıdaki ekran görüntüsünde iki düğme olan bir uyarı gösterir:

 ![ İki düğme ile uyar](alerts-images/alert2.png)

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

Uyarıları, aşağıdaki ekran görüntüsüne benzer bir eylem sayfası da görüntüleyebilirsiniz:

 ![Eylem sayfası uyarı](alerts-images/alert3.png)

Düğmeler uyarı ile eklenir `AddAction` yöntemi:

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

- [Denetimler (örnek)](https://developer.xamarin.com/samples/Controls/)
- [Uyarı denetleyicisi](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
