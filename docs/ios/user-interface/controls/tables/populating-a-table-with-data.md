---
title: Bir tablo verilerle doldurma
ms.topic: article
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: fe62b917946dda4cf669f5b15c91a5e3b596a0fc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="populating-a-table-with-data"></a>Bir tablo verilerle doldurma

Satır eklemek için bir `UITableView` uygulamak gereken bir `UITableViewSource` bir alt kümesi ve Tablo görünümünde yöntemlerini çağıran kendisini doldurmak için geçersiz kılma.

Bu kılavuz kapsar:

- Bir UITableViewSource alt sınıf yapma
- Hücre yeniden kullanma
- Bir dizin ekleme
- Üstbilgiler ve altbilgiler ekleme


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>UITableViewSource alt sınıf yapma

A `UITableViewSource` alt atandığı her `UITableView`. Tablo görünümünde kendisi (örneğin, satır sayısını gereklidir ve için varsayılandan farklı olması durumunda her satırın yüksekliğini) nasıl oluşturulacağını belirlemek için kaynak sınıf sorgular. En önemlisi, kaynak verilerle doldurmuş her hücre görünüm sağlar.

Verileri görüntüleyen bir tablo oluşturmak için gereken yalnızca iki zorunlu yöntemi vardır:

-   **RowsInSection** – dönüş bir [ `nint` ](http://developer.xamarin.com/guides/cross-platform/macios/nativetypes/) tablo görüntülemesi gereken veri satırı sayısı toplam sayısı.
-   **GetCell** – dönüş bir `UITableCellView` yönteme geçirilen karşılık gelen satır dizini için verilerle doldurulur.


BasicTable örnek dosya **TableSource.cs** basit olası uygulanması sahip `UITableViewSource`. Aşağıdaki kod parçacığında tabloda görüntülemek için bir dizeler dizisi kabul eder ve her dizeyi içeren bir varsayılan hücre stili döndürür görebilirsiniz:

```csharp
public class TableSource : UITableViewSource {

        string[] TableItems;
        string CellIdentifier = "TableCell";

        public TableSource (string[] items)
        {
            TableItems = items;
        }

        public override nint RowsInSection (UITableView tableview, nint section)
        {
            return TableItems.Length;
        }

        public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewCell cell = tableView.DequeueReusableCell (CellIdentifier);
            string item = TableItems[indexPath.Row];

            //---- if there are no cells to reuse, create a new one
            if (cell == null)
            { cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

A `UITableViewSource` herhangi bir veri yapısını basit bir dize diziden (Bu örnekte gösterildiği gibi) bir listesi <> veya diğer koleksiyon için kullanabilirsiniz. Uygulaması `UITableViewSource` yöntemleri veri yapılarını tablosundan yalıtır.

Bu alt kullanmak için kaynağınıza sonra örneğine atamak için bir dize dizisi oluşturmanız `UITableView`:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    table = new UITableView(View.Bounds); // defaults to Plain style
    string[] tableItems = new string[] {"Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers"};
    table.Source = new TableSource(tableItems);
    Add (table);
}
```

Ortaya çıkan tabloda şöyle görünür:

 [![](populating-a-table-with-data-images/image3.png "Çalışan örnek tablo")](populating-a-table-with-data-images/image3.png#lightbox)

Çoğu tabloları seçin ve (örneğin, bir tıklatın ya da bir kişi arama veya başka bir ekranını gösteren) başka bir eylemi gerçekleştirmek için bir satır touch izin verin. Bunu başarmak için yapmamız gereken birkaç nokta vardır. İlk olarak, kullanıcı tıkladığınızda bir satırda aşağıdakileri ekleyerek bir ileti görüntülemek için bir AlertController oluşturalım `RowSelected` yöntemi:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

Ardından, bizim View Controller örneğini oluşturun:

```csharp
HomeScreen owner;
```
Bir görünüm denetleyicisi bir parametre olarak alır ve bir alanı kaydeder UITableViewSource sınıfınıza bir oluşturucu ekleyin:

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
Burada UITableViewSource sınıfı oluşturulur geçirmek için ViewDidLoad yöntemi Değiştir `this` başvurusu:

```csharp
table.Source = new TableSource(tableItems, this);
```
Son olarak, geri, `RowSelected` yöntemi, çağrı `PresentViewController` önbelleğe alınmış alanı:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


Kullanıcı bir satır şimdi touch ve bir uyarı görünür:



 [![](populating-a-table-with-data-images/image4.png "Satır Seçili uyarıyı")](populating-a-table-with-data-images/image4.png#lightbox)


## <a name="cell-reuse"></a>Hücre yeniden kullanma

Bu örnekte yalnızca altı öğe yok, bu yüzden gerekli hiçbir hücre yeniden. Ancak, yüzlerce veya binlerce satır görüntülerken, yüzlerce veya binlerce oluşturmak için bellek kaybı olacaktır `UITableViewCell` nesneleri yalnızca birkaç ekranda aynı anda uygun olduğunda.

Bir hücrenin görünümünü yeniden kullanım için bir sıra yerleştirilir ekranından kaybolduğunda bu durumdan kaçınmak için. Kullanıcı kayarken tablosu çağırır `GetCell` görüntülenecek – (görüntülenmeyen şu anda) var olan bir hücre yeniden kullanmak için yeni görünümler istemek için basitçe çağrı `DequeueReusableCell` yöntemi. Bir hücre, döndürülecek kullanılabilmeleri için ise, aksi takdirde null döndürdü ve kodunuzu yeni bir hücre örneği oluşturmanız gerekir.

Bu örnekteki kod parçacığı desen gösterir:

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

`cellIdentifier` Etkin hücre farklı türde ayrı kuyruklar oluşturur. Aynı şekilde yalnızca bir sabit kodlanmış tüm hücreleri Ara Bu örnekte tanımlayıcı kullanılır. Hücre farklı türlerde varsa bunlar her farklı tanımlayıcı dizesinde hem örneği oluşturulmadan olduğunda ve yeniden sıradan istendiğinde olması gerekir.

### <a name="cell-reuse-in-ios-6"></a>İOS 6 + yeniden hücre

iOS 6 koleksiyon görünümleri ile bir giriş için benzer bir hücre yeniden deseni eklendi. Yukarıda gösterilen varolan yeniden düzeni hücreyi null denetimi gereksinimini ortadan kaldırır gibi geriye dönük uyumluluk, bu yeni düzeni tercih için hala desteklenmektedir ancak.

Hücre sınıfı veya ya da çağırarak kullanılacak xib uygulamanın yeni desenle kaydeder `RegisterClassForCellReuse` veya `RegisterNibForCellReuse` denetleyicinin oluşturucuda. Sonra dequeueing hücre içinde `GetCell` yöntemi, yalnızca çağrısı `DequeueReusableCell` hücre sınıfı veya xib ve dizin yolu için kayıtlı tanımlayıcı geçirme.

Örneğin, aşağıdaki kod bir özel hücre sınıfı bir UITableViewController kaydeder:

```csharp
public class MyTableViewController : UITableViewController
{
  static NSString MyCellId = new NSString ("MyCellId");

  public MyTableViewController ()
  {
    TableView.RegisterClassForCellReuse (typeof(MyCell), MyCellId);
  }
  ...
}
```

MyCell Sınıf kayıtlı hücre içinde kuyruktan çıkarıldı `GetCell` yöntemi `UITableViewSource` gerek kalmadan gösterildiği gibi ek null denetimi aşağıdaki:

```csharp
class MyTableSource : UITableViewSource
{
  public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
  {
    // if cell is not available in reuse pool, iOS will create one automatically
    // no need to do null check and create cell manually
    var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

    // do whatever you need to with cell, such as assigning properties, etc.

    return cell;
  }
}
```

Unutmayın, yeni yeniden düzeni özel hücre sınıfı ile kullanırken, alan oluşturucu uygulamak gereken bir `IntPtr`, parçacığında gösterildiği gibi Aksi takdirde Objective-C hücre sınıfının bir örneğini oluşturmak mümkün değildir:

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

Yukarıda açıklandığı konuları örnekleri görebilirsiniz **BasicTable** örnek bu makaleyle bağlantılı.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>Bir dizin ekleme

Bir dizin dizin oluşturabilirsiniz ancak genellikle alfabetik sıraya göre uzun listeleriyle kaydırma yapmasına yardımcı istediğiniz herhangi bir ölçüte göre. **BasicTableIndex** örnek kadar uzun bir liste öğeleri dizini göstermek için bir dosyadan yükler. Dizindeki her öğe bir 'bölümüne' tablosunun karşılık gelir.

 [![](populating-a-table-with-data-images/image5.png "Dizin görüntüleme")](populating-a-table-with-data-images/image5.png#lightbox)

'Tablo ardındaki verileri gereken Gruplanacak, BasicTableIndex örnek oluşturur bölümler' desteklemek için bir `Dictionary<>` sözlük anahtar olarak her öğenin ilk harfini kullanarak dizeleri dizisinden:

```csharp
indexedTableItems = new Dictionary<string, List<string>>();
foreach (var t in items) {
    if (indexedTableItems.ContainsKey (t[0].ToString ())) {
        indexedTableItems[t[0].ToString ()].Add(t);
    } else {
        indexedTableItems.Add (t[0].ToString (), new List<string>() {t});
    }
}
keys = indexedTableItems.Keys.ToArray ();
```

`UITableViewSource` Alt ardından eklenen veya değiştirilen kullanmak için aşağıdaki yöntemlerden gerekiyor `Dictionary<>` :

-   **NumberOfSections** – bu yöntem isteğe bağlıdır, varsayılan olarak bir bölüm tablo varsayar. Dizin görüntülerken, bu yöntem öğe sayısı (tüm alfabedeki İngilizce dizini içeriyorsa, örneğin, 26) dizininde döndürmelidir.
-   **RowsInSection** – belirli bir bölümde satır sayısını döndürür.
-   **SectionIndexTitles** – dizini görüntülemek için kullanılan bir dize dizisi döndürür. Örnek kod bir dizi harf döndürür.


Örnek dosyayı güncelleştirilmiş yöntemlere **BasicTableIndex/TableSource.cs** şöyle görünür:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return keys.Length;
}
public override nint RowsInSection (UITableView tableview, nint section)
{
    return indexedTableItems[keys[section]].Count;
}
public override string[] SectionIndexTitles (UITableView tableView)
{
    return keys;
}
```

Dizinleri genellikle yalnızca düz tablo stiliyle kullanılır.


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>Üstbilgiler ve altbilgiler ekleme

Üstbilgiler ve altbilgiler tablodaki satırları görsel olarak gruplandırmak için kullanılabilir. Gerekli veri yapısı bir dizin ekleme ile çok benzer – bir `Dictionary<>` gerçekten iyi çalışır. Hücreleri grubuna alfabe kullanmak yerine, bu örnek et botanical türüne göre gruplandırır.
Çıktı şu şekildedir:

 [![](populating-a-table-with-data-images/image6.png "Örnek üstbilgiler ve altbilgiler")](populating-a-table-with-data-images/image6.png#lightbox)

Üstbilgiler ve altbilgiler görüntülenecek `UITableViewSource` alt bu ek yöntemleri gerektirir:

-   **TitleForHeader** – üstbilgisi olarak kullanılacak metin döndürür
-   **TitleForFooter** – altbilgi kullanılacak metin döndürür.


Örnek dosyayı güncelleştirilmiş yöntemlere **BasicTableHeaderFooter/Code/TableSource.cs** şöyle görünür:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    return keys[section];
}
public override string TitleForFooter (UITableView tableView, nint section)
{
    return indexedTableItems[keys[section]].Count + " items";
}
```

Daha fazla üstbilgi ve altbilgi bir görünümle görünümünü özelleştirebilirsiniz nesnesi, kullanarak `GetViewForHeader` ve `GetViewForFooter` yöntemini geçersiz kılar üzerinde `UITableViewSource`.


## <a name="related-links"></a>İlgili bağlantılar

- [WorkingWithTables (örnek)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
