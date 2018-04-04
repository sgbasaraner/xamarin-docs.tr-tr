---
title: Sürükleme ve Bırakma
description: Sürükle ve bırak 11 iOS için uygulama
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2016
ms.openlocfilehash: fa6fcb2c4f5f17011307b31e4644889d066b71a9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="drag-and-drop"></a>Sürükleme ve Bırakma

_Sürükle ve bırak 11 iOS için uygulama_

iOS 11 içerir sürükle ve bırak iPad uygulamalar arasında veri kopyalamak için destek. Kullanıcıları seçin ve tüm içerik türlerini konumlandırılmış uygulamalar yan yana veya açmak ve veri kesilmesine olanak tanımak için uygulama tetikleyecek bir uygulama simgesi sürükleyerek sürükleyin:

![Özel uygulama notları uygulamada sürükle ve bırak örnekten](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Sürükle ve bırak yalnızca mevcut değil aynı uygulama iPhone içinde.

Sürükleme destekleme göz önünde bulundurun ve bırakma işlemleri herhangi bir yerden içerik oluşturduğunuz veya düzenlediğiniz:

- Metin denetimleri, iOS 11, herhangi bir ek iş olmadan karşı oluşturulan tüm uygulamaların için sürükle ve bırak desteği.
- Tablo görünümleri ve koleksiyon görünümlerini ekleme Sürükle basitleştirmek ve bırak davranışı iOS 11 geliştirmeler içerir.
- Diğer bir görünümü ile ek özelleştirme bırakma ve sürükleme desteklemek için yapılabilir.

Uygulamalarınıza ekleme sürükle ve bırak desteği, içerik uygunluğunu farklı düzeylerde sağlayabilir; Örneğin, alıcı uygulamanın hangi Sürükle hedef en iyi uyan seçebilmeniz biçimlendirilmiş metin ve verileri düz metin sürümü sağlayabilir. Ayrıca Sürükle görselleştirme özelleştirmek için ve ayrıca aynı anda birden çok öğe Sürüklemeyi etkinleştirmek için mümkündür.

## <a name="drag-and-drop-with-text-controls"></a>Sürükle ve bırak metin denetimleri ile

`UITextView` ve `UITextField` seçili metnin, sürükleyip metin içeriği otomatik olarak destekler.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Sürükle ve bırak UITableView ile

`UITableView` varsayılan davranışı etkinleştirmek için yalnızca birkaç yöntem gerektiren tablo satırları ile etkileşim sürükleyip yerleşik olarak sahiptir.

Söz konusu iki arabirimi vardır:

- `IUITableViewDragDelegate` – Tablo görünümünde bir Sürükle başlatıldığında paketleri bilgileri.
- `IUITableViewDropDelegate` – Bir açılan yüklenirken bilgileri işler çalıştı ve tamamlandı.

İçinde [DragAndDropTableView örnek](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) bu iki arabirim hem üzerinde uygulanan `UITableViewController` temsilci ve veri kaynağının birlikte sınıfı. İçinde atanmış oldukları `ViewDidLoad` yöntemi:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Bu iki arabirimleri için en az gerekli kod aşağıda açıklanmıştır.

### <a name="table-view-drag-delegate"></a>Tablo görünümü Sürükle temsilci

Tek yöntem _gerekli_ Tablo görünümünden bir satır sürükleyerek desteklemektir `GetItemsForBeginningDragSession`. Kullanıcı bir satır sürükleyin başlarsa, bu yöntem çağrılır.

Bir uygulama, aşağıda gösterilmiştir. Sürüklenen satırla ilişkili verileri alır, bu kodlar ve yapılandırır bir `NSItemProvider` uygulamaları işlemi "açılan" parçası nasıl işleyeceğini belirler (örneğin, olup bunların veri türü işleyebilir `PlainText`, örnekteki):

```csharp
public UIDragItem[] GetItemsForBeginningDragSession (UITableView tableView,
  IUIDragSession session, NSIndexPath indexPath)
{
  // gets the 'information' to be dragged
  var placeName = model.PlaceNames[indexPath.Row];
  // convert to NSData representation
  var data = NSData.FromString(placeName, NSStringEncoding.UTF8);
  // create an NSItemProvider to describe the data
  var itemProvider = new NSItemProvider();
  itemProvider.RegisterDataRepresentation(UTType.PlainText,
                                NSItemProviderRepresentationVisibility.All,
                                (completion) =>
  {
    completion(data, null);
    return null;
  });
  // wrap in a UIDragItem
  return new UIDragItem[] { new UIDragItem(itemProvider) };
}
```

Hedef uygulamalarında avantajlarından alınabilir birden çok veri Beyanları sağlama gibi Sürükle davranışını özelleştirmek için uygulanabilir Sürükle temsilci üzerinde birçok isteğe bağlı yöntemler vardır (düz metin yanı ya da bir vektör olarak biçimlendirilmiş metin gibi ve bit eşlem sürümleri çizim). Ayrıca aynı uygulama içinde sürükleyip yapılırken kullanılacak özel veri Beyanları sağlayabilir.

### <a name="table-view-drop-delegate"></a>Tablo görünümü açılan temsilci

Sürükleme işlemi bir tablo görünümü oluşur ya da tamamlanmadan açılan temsilci yöntemlerde denir. Gerekli yöntemleri veri bırakılmasına izin verilip verilmeyeceğini ve açılan tamamlanırsa hangi gerçekleştirilecek eylemleri belirler:

- `CanHandleDropSession` – İlerleme ve potansiyel olarak uygulamanın bırakılmakta Sürükle olmakla birlikte, bu yöntem sürüklenen veri bırakılmasına izin verilip verilmediğini belirler.
- `DropSessionDidUpdate` – Sürükleme işlemi sürerken ne yöneliktir belirlemek için bu yöntem çağrılır. Üzerinden sürüklenmekte olan tablo görünümü, sürükle oturum ve olası bir dizin yolu bilgilerinden tüm davranışını ve kullanıcı tarafından sağlanan görsel geribildirim belirlemek için kullanılabilir.
- `PerformDrop` – Kullanıcının açılan (kendi parmak kaldırma tarafından) tamamlandığında, bu yöntem sürüklenen verileri ayıklar ve yeni bir satır (veya satır) veri eklemek için Tablo görünümü değiştirir.

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Tablo görünümünde sürüklenen verileri kabul etmek olup olmadığını gösterir. Bu kod parçacığında bulunan `CanLoadObjects` Bu tablo görünüm dize verilerini kabul ettiğini onaylamak için kullanılır.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` Yöntemi sürükleme işlemi sürüyor, kullanıcıya görsel Yardım sağlamak için durumdayken sürekli olarak çağrılır.

Aşağıdaki kodda `HasActiveDrag` işlemi geçerli Tablo görünümünde kaynaklanan olup olmadığını belirlemek için kullanılır. Bu durumda, yalnızca tek satırları taşınmasına izin verilmez.
Sürükle başka bir kaynaktan ise, bir kopyalama işlemi gösterilir:

```csharp
public UITableViewDropProposal DropSessionDidUpdate(UITableView tableView, IUIDropSession session, NSIndexPath destinationIndexPath)
{
  // The UIDropOperation.Move operation is available only for dragging within a single app.
  if (tableView.HasActiveDrag)
  {
    if (session.Items.Length > 1)
    {
        return new UITableViewDropProposal(UIDropOperation.Cancel);
    } else {
        return new UITableViewDropProposal(UIDropOperation.Move, UITableViewDropIntent.InsertAtDestinationIndexPath);
    }
  } else {
    return new UITableViewDropProposal(UIDropOperation.Copy, UITableViewDropIntent.InsertAtDestinationIndexPath);
  }
}
```

Bırakma işlemi şunlardan biri olabilir `Cancel`, `Move`, veya `Copy`.

Bırakma hedefi, yeni bir satır eklemek veya var olan bir satır veri ekleyin/eklemek için olabilir.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` Kullanıcı işlemi tamamlandığında ve devre dışı bırakılan veri yansıtacak şekilde tablo görüntüle ve veri kaynağı değiştirir yöntemi çağrılır.

```csharp
public void PerformDrop(UITableView tableView, IUITableViewDropCoordinator coordinator)
{
  NSIndexPath indexPath, destinationIndexPath;
  if (coordinator.DestinationIndexPath != null)
  {
    indexPath = coordinator.DestinationIndexPath;
    destinationIndexPath = indexPath;
  }
  else
  {
    // Get last index path of table view
    var section = tableView.NumberOfSections() - 1;
    var row = tableView.NumberOfRowsInSection(section);
    destinationIndexPath = NSIndexPath.FromRowSection(row, section);
  }
  coordinator.Session.LoadObjects(typeof(NSString), (items) =>
  {
    // Consume drag items
    List<string> stringItems = new List<string>();
    foreach (var i in items)
    {
      var q = NSString.FromHandle(i.Handle);
      stringItems.Add(q.ToString());
    }
    var indexPaths = new List<NSIndexPath>();
    for (var j = 0; j < stringItems.Count; j++)
    {
      var indexPath1 = NSIndexPath.FromRowSection(destinationIndexPath.Row + j, destinationIndexPath.Section);
      model.AddItem(stringItems[j], indexPath1.Row);
      indexPaths.Add(indexPath1);
    }
    tableView.InsertRows(indexPaths.ToArray(), UITableViewRowAnimation.Automatic);
  });
}
```

Büyük veri nesneleri zaman uyumsuz olarak yüklemek için ek kod eklenebilir.

### <a name="testing-drag-and-drop"></a>Sınama sürükleme ve bırakma

Test etmek için iPad kullanmalısınız [örnek](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Başka bir uygulama (örneğin, Notlar) yanında örnek açın ve satır ve metin arasında sürükleyin:

![ekran görüntüsü sürükleme işlemi devam ediyor](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>İlgili bağlantılar

- [Sürükle ve bırak İnsan Arabirimi yönergelerine (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Sürükle ve bırak Tablo görünümü örneği](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Sürükleme ve bırakma koleksiyon görünümü örneği](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Sürükleme ve bırakma (WWDC) (video) Tanıtımı](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Sürükleme ve bırakma koleksiyon ve Tablo görünümü (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/223/)
