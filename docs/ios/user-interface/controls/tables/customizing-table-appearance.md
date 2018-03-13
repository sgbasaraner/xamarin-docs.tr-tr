---
title: "Bir tablonun görünümünü özelleştirme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 4a3f8ca8f4502b9585536815aef81f66cacd214f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-a-tables-appearance"></a>Bir tablonun görünümünü özelleştirme

Bir tablonun görünümünü değiştirmek için en basit yolu, farklı hücre stili kullanmaktır. Hangi hücre stili her hücre oluşturulurken kullanılan değiştirebilirsiniz `UITableViewSource`'s `GetCell` yöntemi.

## <a name="cell-styles"></a>Hücre stilleri

Dört yerleşik stilleri vardır:

-  **Varsayılan** – destekleyen bir `UIImageView`.
-  **Alt başlık** – destekleyen bir `UIImageView` ve alt başlığı.
-  **Değer1** – sağa hizalı altyazısı destekleyen bir `UIImageView`.
-  **Value2** – başlık sağa hizalı ve altyazısı sola hizalı (görüntü).


Bu ekran görüntüleri her stili nasıl göründüğünü gösterir:

 [![](customizing-table-appearance-images/image7.png "Bu ekran görüntüleri her stili nasıl göründüğünü göster")](customizing-table-appearance-images/image7.png#lightbox)

Örnek **CellDefaultTable** Bu ekranlar üretmek için kodunu içerir. Hücre stili kümesinde `UITableViewCell` Oluşturucusu şuna benzer:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[Desteklenen Özellikler](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/) hücrenin stili sonra ayarlanabilir:

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Donatılar

Hücreleri görünümü sağa eklenen aşağıdaki Donatılar sahip olabilir:

-   **Onay işareti** – çoklu seçim bir tabloda göstermek için kullanılır.
-   **DetailButton** – hücre dokunmak için farklı bir işlevi gerçekleştirmeye izin hücrenin kalan bağımsız olarak touch yanıtlar (açılan veya olmayan yeni bir pencere açarak gibi parçası bir `UINavigationController` yığın).
-   **DisclosureIndicator** – hücre temas başka bir görünüm açmak belirtmek için normal olarak kullanılır.
-   **DetailDisclosureButton** – bir birleşimini `DetailButton` ve `DisclosureIndicator`.


Bu, nasıl göründüğünü oluşur:

 [![](customizing-table-appearance-images/image8.png "Örnek Donatılar")](customizing-table-appearance-images/image8.png#lightbox)

Ayarlayabileceğiniz bu Donatılar birini görüntülemek için `Accessory` özelliğinde `GetCell` yöntemi:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Zaman `DetailButton` veya `DetailDisclosureButton` gösterilir, aynı zamanda geçersiz kılma `AccessoryButtonTapped` , işlemdeki olduğunda bazı eylemleri gerçekleştirmek için.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

Örnek **CellAccessoryTable** Donatılar kullanan bir örnek gösterilmektedir.

## <a name="cell-separators"></a>Hücre ayırıcılar

Hücre ayırıcılar tablo ayırmak için kullanılan tablo hücreleri ' dir. Özellikler tablo üzerinde ayarlanır.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

Ayırıcı bulandırma veya canlılık verir etkisi eklemek mümkündür:

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

Ayırıcı bir iç metin de sahip olabilir:

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>Özel hücre düzenleri oluşturma

Bir tablonun görsel stilini değiştirmek için görüntülemek için özel hücresini sağlamanız gerekir. Özel hücre farklı renkleri ve denetim düzenleri olabilir.

CellCustomTable örnek uygulayan bir `UITableViewCell` özel yerleşimini tanımlayan bir alt `UILabel`s ve `UIImage` farklı yazı tipleri ve renkler ile. Sonuçta elde edilen hücreleri şöyle görünür:

 [![](customizing-table-appearance-images/image9.png "Özel hücre düzenleri")](customizing-table-appearance-images/image9.png#lightbox)

Özel hücre sınıfı yalnızca üç yöntem oluşur:

-   **Oluşturucu** – kullanıcı Arabirimi denetimlerini oluşturur ve (ör. özel bir stil özelliklerini ayarlar yazı tipi, boyut ve renk).
-   **UpdateCell** – için bir yöntem `UITableView.GetCell` hücrenin özelliklerini ayarlamak için kullanılacak.
-   **LayoutSubviews** – kullanıcı Arabirimi denetimlerini konumunu ayarlayın. Örnekte her hücre aynı düzeni olsa da, daha karmaşık bir hücre (özellikle olanlar farklı boyutlarda) görüntülenmekte olan içeriğin bağlı olarak farklı düzen konumlar gerekebilir.


Tam örnek kodda **CellCustomTable > CustomVegeCell.cs** izler:

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

`GetCell` Yöntemi `UITableViewSource` özel hücre oluşturmak için değiştirilmesi gerekir:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```



## <a name="related-links"></a>İlgili bağlantılar

- [WorkingWithTables (örnek)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
