---
title: "Otomatik boyutlandırma satır yüksekliği"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c8d66ff8199d451ce7469fa893b7673589c9e320
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="auto-sizing-row-height"></a>Otomatik boyutlandırma satır yüksekliği

İOS 8 ile başlayarak, Apple bir tablo görünümü oluşturma yeteneği eklendi (`UITableView`), otomatik olarak Büyüt ve otomatik düzeni, boyutu sınıfları ve kısıtlamaları kullanarak içeriği boyutuna göre belirli bir satırın yüksekliğini daraltma.

iOS 11 otomatik olarak genişletmek satır yeteneği eklemiştir. Üstbilgiler, altbilgiler ve hücreler artık otomatik olarak içeriğine göre boyutlandırılabilir. Ancak, iOS Tasarımcısı arabirimi Oluşturucu tablonuz oluşturduysanız veya satır yükseklikte sabit sahip, el ile kendini hücreleri boyutlandırma bu kılavuzda açıklandığı etkinleştirmeniz gerekir.

## <a name="cell-layout-in-the-ios-designer"></a>İOS Tasarımcısı düzeni hücre

Açık Tablo Tasarımcısı, iOS için sıranın otomatik boyutlandırma olmasını istediğiniz görünümü film şeridi seçin hücrenin *prototip* ve hücre düzenini tasarım. Örneğin:

[![](autosizing-row-height-images/table01.png "Hücrenin prototip tasarım")](autosizing-row-height-images/table01.png#lightbox)

Her bir öğe için prototip, Tablo görünümünde döndürme veya farklı iOS cihazı ekran boyutlarına boyutlandırıldığında doğru konumda öğelerinizi kısıtlamaları ekleyin. Örneğin, sabitleme `Title` , sol üst ve sağ hücrenin *içerik görünümü*:

[![](autosizing-row-height-images/table02.png "Üst ve sol hücreleri içerik görünümü sağ başlık sabitleme")](autosizing-row-height-images/table02.png#lightbox)

Bizim örnek tablo küçük olması durumunda `Label` (altında `Title`) küçültmek ve artırmak veya azaltmak satır yüksekliğini büyüyebileceği alanıdır. Bu etkiyi elde etmek için sol, sağ, alt ve üst etiketinin sabitlemek için aşağıdaki kısıtlamalar ekleyin:

[![](autosizing-row-height-images/table03.png "Bu kısıtlamaların sol, sağ, alt ve üst etiketinin sabitlemek için")](autosizing-row-height-images/table03.png#lightbox)

Biz hücre öğeleri tam olarak kısıtlı, hangi öğesi uzatılmış açıklamak gerekir. Bunu yapmak için ayarlayın **içerik Hugging öncelik** ve **içerik sıkıştırma Direnci öncelik** gerektiği şekilde **düzeni** özellikleri paneli bölümünü:

[![](autosizing-row-height-images/table03a.png "Özellikler paneli düzeni bölümü")](autosizing-row-height-images/table03a.png#lightbox)

Olmasını genişletmek istediğiniz öğe bir **alt** Hugging öncelik değeri ve **alt** sıkıştırma Direnci öncelik değeri.

Ardından, hücre prototip seçin ve benzersiz bir vermek ihtiyacımız **tanımlayıcısı**:

[![](autosizing-row-height-images/table04.png "Benzersiz bir tanımlayıcı hücre prototip vermiş")](autosizing-row-height-images/table04.png#lightbox)

Örneğimizde, söz konusu olduğunda `GrowCell`. Biz tabloyu doldurmak olduğunda bu değer daha sonra kullanacağız.

> [!IMPORTANT]
> **Not:** tablonuz birden fazla hücre türü içeriyorsa (**prototip**), her tür kendi benzersiz olduğundan emin olun gerek `Identifier` çalışmak satır otomatik yeniden boyutlandırma.

Bizim hücre prototip her öğe için Ata bir **adı** C# kodu kullanıma sunmak için. Örneğin:

[![](autosizing-row-height-images/table05.png "C# kodu kullanıma sunmak için bir ad atayın")](autosizing-row-height-images/table05.png#lightbox)

Ardından, için özel bir sınıf ekleyin `UITableViewController`, `UITableView` ve `UITableCell` (prototip). Örneğin: 

[![](autosizing-row-height-images/table06.png "UITableViewController, UITableView ve UITableCell için özel bir sınıf ekleme")](autosizing-row-height-images/table06.png#lightbox)

Son olarak, tüm içerik bizim etiketinde görüntülenen beklenen emin olmak için ayarlanmış **satırları** özelliğine `0`:

[![](autosizing-row-height-images/table06.png "Satırları özelliği 0 olarak ayarlayın")](autosizing-row-height-images/table06a.png#lightbox)

Tanımlanan kullanıcı Arabirimi, otomatik satır yüksekliği yeniden boyutlandırma olanak sağlamak için kod ekleyelim.

## <a name="enabling-auto-resizing-height"></a>Otomatik yeniden boyutlandırma yükseklik etkinleştirme

Her ikisinde bizim tablo görünümün veri kaynağı (`UITableViewDatasource`) ya da kaynak (`UITableViewSource`), biz ihtiyacımız kullanmak üzere bir hücre dequeue `Identifier` biz Tasarımcısı'nda tanımlanan. Örneğin:

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

Varsayılan olarak, Tablo görünümünde otomatik yeniden boyutlandırma satır yüksekliği için ayarlanacak. Bu, emin olmak için `RowHeight` özelliği ayarlanmalıdır `UITableView.AutomaticDimension`. Ayrıca ayarlamak ihtiyacımız `EstimatedRowHeight` özelliğinde bizim `UITableViewController`. Örneğin:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

Bu tahmini değere tam olmak zorunda değildir, Tablo görünümünde her satırın ortalama yükseklik yalnızca kabaca bir tahmin.

Yerinde bu kodu ile uygulama çalıştırıldığında, her satır küçültmek ve hücre prototip son etiketinde yüksekliğinde bağlı olarak büyümek. Örneğin:

[![](autosizing-row-height-images/table07.png "Çalıştıran bir örnek tablo")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>İlgili bağlantılar

- [GrowRowTable (örnek)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
