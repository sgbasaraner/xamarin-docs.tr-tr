---
title: Düzenleme
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 161de0209217dde671b976afad90eaad18d8c7b0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="editing"></a>Düzenleme

Tablo düzenleme özellikleri yöntemleri geçersiz kılarak etkin bir `UITableViewSource` bir alt kümesi. En basit düzenleme tek yöntemi geçersiz kılma ile uygulanabilir manyetik Silme hareketi davranıştır.
Daha karmaşık (dahil olmak üzere taşıma satırlar) düzenleme düzenleme modunda tabloyla yapılabilir.

## <a name="swipe-to-delete"></a>Manyetik silme

Özellik silmek için manyetik doğal hareketi kullanıcıların beklediğiniz iOS içinde ' dir. 

 [![](editing-images/image10.png "Manyetik örneği silme")](editing-images/image10.png#lightbox)

Göstermek için manyetik hareketi etkileyen üç yöntemi geçersiz kılmalar vardır bir **silme** bir hücreye düğmesi:

-   **CommitEditingStyle** – bu yöntem geçersiz kılınır ve manyetik Silme hareketi otomatik olarak etkinleştirir, tablo kaynak algılar. Yöntemin uygulamayı çağırması gerekir `DeleteRows` üzerinde `UITableView` kaybolur ve de temel alınan veri, modelinden (örneğin, bir dizi, sözlük veya veritabanı) kaldırmak için hücreleri neden olacak. 
-   **CanEditRow** – geçersiz durumunda CommitEditingStyle kılınmıştır, tüm satırları düzenlenebilir olduğu varsayılır. Bu yöntem uygulanmadı ve (bazı belirli satırlar için veya tüm satırların) false değerini döndürür, ardından geçirme Silme hareketi bu hücrede kullanılabilir olmaz. 
-   **TitleForDeleteConfirmation** – isterseniz metnini belirtir **silmek** düğmesi. Bu yöntem uygulanmadı, "Delete" düğmesi metni olacaktır. 


Bu yöntemler uygulanır `TableSource` sınıf aşağıdadır:

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

Bu örneğin `UITableViewSource` kullanmak için güncelleştirilmiş bir `List<TableItem>` (bir dize dizisi yerine) desteklediğinden, veri kaynağı olarak ekleme ve silme öğeleri koleksiyondan.


## <a name="edit-mode"></a>Düzenleme modu

Bir tablo düzenleme modunda olduğunda kullanıcı işlemdeki zaman Sil düğmesini gösterir her satırda bir kırmızı 'stop' pencere görür. Tablonun satır sırasını değiştirmek için sürüklenebilir belirtmek için bir 'işleyicisi' simgesi de görüntüler.
**TableEditMode** örnek gösterildiği gibi bu özellikler uygular.

 [![](editing-images/image11.png "Gösterildiği gibi bu özellikleri TableEditMode örnek uygular")](editing-images/image11.png#lightbox)

Üzerinde bir dizi farklı yöntemler vardır `UITableViewSource` bir tablonun düzenleme modu davranışını etkiler:

-   **CanEditRow** – her satır düzenlenebilir olup olmadığını. Manyetik silme ve düzenleme modundayken silinmesini önlemek için false döndürür. 
-   **CanMoveRow** – dönün girintili taşıma 'işleyicisi' veya taşıma önlemek için etkinleştirin. 
-   **EditingStyleForRow** – tablo düzenleme modunda olduğunda hücre kırmızı silme simgesini görüntüler veya yeşil Ekle simgesi Bu yöntemden dönüş değeri belirler. Dönüş `UITableViewCellEditingStyle.None` satır düzenlenebilir değil gerekiyorsa. 
-   **MoveRow** – temel alınan veri yapısı tabloda gösterildiği verilerle eşleşecek şekilde değiştirilebilir için bir satır taşındı olduğunda çağrılır. 


Kullanmak istediğiniz sürece ilk üç yöntemleri için göreceli olarak doğrudan bir ileri – uygulamasıdır `indexPath` yalnızca stillerinizin belirli satırlar davranışını değiştirmek için tüm tablo için dönüş değerleri.

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

`MoveRow` Yeni sıraya uyacak şekilde temel alınan veri yapısı alter gerektiğinden uygulaması biraz daha karmaşıktır. Veri olarak uygulandığı için bir `List` aşağıdaki kodu, eski konumda veri öğesi siler ve yeni konuma ekler. Verileri bir 'sırası' sütunuyla bir SQLite veritabanı tablosundaki bir (örneğin) depolanmışsa, bu yöntem bunun yerine bu sütundaki sayıları yeniden sıralamak için bazı SQL işlemlerini gerçekleştirmek gerekir.

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

Son olarak, almak için tabloya düzenleme modunda, **Düzenle** düğmesi gereken çağırmak `SetEditing` şöyle

```csharp
table.SetEditing (true, true);
```

ve kullanıcı tamamlandığında, düzenleme **Bitti** düğmesi kapatmak düzenleme modu:

```csharp
table.SetEditing (false, true);
```


## <a name="row-insertion-editing-style"></a>Satır ekleme düzenleme stili

Tablo içindeki satır ekleme bir seyrek kullanıcı arabirimi – standart iOS uygulamalarını ana örnekte **Düzenle kişi** ekran. Bu ekran satır ekleme işlevselliğini nasıl çalıştığı gösterilmektedir – Düzen modu yoktur ek bir satır, (tıklatıldığında) ek satırlar verilerini ekler. Düzenleme tamamlandığında, geçici **(Yeni Ekle)** satır kaldırılır.

 [![](editing-images/image12.png "Düzenleme tamamlandıktan sonra geçici ekleme yeni satır kaldırılır")](editing-images/image12.png#lightbox)

Üzerinde bir dizi farklı yöntemler vardır `UITableViewSource` bir tablonun düzenleme modu davranışını etkiler. Bu yöntemler aşağıdaki gibi örnek kodda uygulanmıştır:

-   **EditingStyleForRow** – döndürür `UITableViewCellEditingStyle.Delete` veri ve döndürür içeren satırları için `UITableViewCellEditingStyle.Insert` (özellikle Ekle düğmesi olarak davranacak şekilde eklenir) son satır için. 
-   **CustomizeMoveTarget** – kullanıcı hücrenin isteğe bağlı Bu yöntemden dönüş değeri kendi seçtikleri konumunun geçersiz kılma taşıma sırasında. Bu, engelleyebilir bunları 'den bırakma' anlamına gelir – herhangi bir satır sonra taşınmış engeller Bu örnek gibi belirli konumlarda hücre **(Yeni Ekle)** satır. 
-   **CanMoveRow** – dönün girintili taşıma 'işleyicisi' veya taşıma önlemek için etkinleştirin. Örnekte 'sunucusuna yalnızca bir ekleme düğmesi olarak tasarlandığından gizli taşıma tutamacını' son satır var. 


Biz de 'INSERT' satır ekleyin ve ardından yeniden artık gerekli olduğunda kaldırmak için iki özel yöntemlerini ekleyin. Gelen adlı **Düzenle** ve **Bitti** düğmeleri:

-   **WillBeginTableEditing** – **Düzenle** düğme çağrıları işlemdeki `SetEditing` tablo düzenleme moduna yerleştirilecek. Bu, biz görüntülemek WillBeginTableEditing yöntemi tetikler **(Yeni Ekle)** 'ekleme düğmesi' davranacak şekilde tablonun sonuna satır. 
-   **DidFinishTableEditing** – Bitti düğmesini işlemdeki `SetEditing` yeniden düzenleme modunu devre dışı bırakmak için çağrılır. Kod örneği kaldırır **(Yeni Ekle)** düzenlerken tablosundan satır gereklidir artık. 


Bu yöntemi geçersiz kılmalar örnek dosyasında uygulanan **TableEditModeAdd/Code/TableSource.cs**:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

Bu iki özel yöntem eklemek ve kaldırmak için kullanılan **(Yeni Ekle)** tablo modu düzenlerken kullanıcının satır etkin veya devre dışı:

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

Son olarak, bu kod başlatır **Düzenle** ve **Bitti** düğmeleriyle etkinleştiren veya touched düzenleme modunu devre dışı Lambda'lar:

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

De kullanabilirsiniz ancak bu satır ekleme UI deseni çok sık kullanılmaz `UITableView.BeginUpdates` ve `EndUpdates` ekleme veya kaldırma herhangi bir tablodaki hücre animasyon yöntemleri. Bu yöntemleri kullanarak için değer fark tarafından döndürülen kuralıdır `RowsInSection` arasında `BeginUpdates` ve `EndUpdates` çağrıları net eklenen/ile silinen hücre sayısını eşleşmelidir `InsertRows` ve `DeleteRows` yöntemleri. Temel alınan veri kaynağı eklemeleri/silme tablosundaki görünümündeki eşleşecek şekilde değişirse olmayan bir hata meydana gelir.


## <a name="related-links"></a>İlgili bağlantılar

- [WorkingWithTables (örnek)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
