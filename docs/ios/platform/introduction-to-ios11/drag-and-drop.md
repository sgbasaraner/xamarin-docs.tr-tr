---
title: Sürükle ve bırak Xamarin.ios'ta
description: Bu belge, sürükle uygulamak ve iOS 11 ' sunulan API'ler kullanarak Xamarin.iOS uygulamalarında açılan açıklar. Özellikle, etkinleştirme anlatılmaktadır sürükle ve bırak UITableView içinde.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2017
ms.openlocfilehash: bc58c866a4a754bccea8d851f79e73fe5a415eed
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351141"
---
# <a name="drag-and-drop-in-xamarinios"></a>Sürükle ve bırak Xamarin.ios'ta

_Sürükle ve bırak iOS 11 uygulama_

iOS 11 içerir sürükle ve bırak iPad uygulamaları arasında veri kopyalamak için destek. Kullanıcılar, seçin ve tüm içerik türlerini konumlandırılmış uygulamalar yan yana veya uygulamayı açın ve verilerinin kesilmesini izin tetikleyecek bir uygulama simgesi üzerinde sürükleyerek sürükleyin:

![Sürükle ve bırak örnek notları uygulamasına özel bir uygulamadan](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Sürükle ve bırak, yalnızca kullanılabilir iPhone aynı uygulama içinde.

Sürükleme desteklemeyi düşünün ve bırakma işlemleri herhangi bir içerik oluşturuldu veya düzenlendi:

- Metin denetimlerine sürükle ve bırak iOS 11, herhangi bir ek çalışma yapmadan karşı oluşturulan tüm uygulamaları destekler.
- Tablo görünümleri ve koleksiyon görünümlerini ekleme Sürükle kolaylaştıran ve bırak davranışı iOS 11'geliştirmeler içerir.
- Herhangi bir görünüm Sürükle destekler ve ek özelleştirme ile yapılabilir.

Uygulamalarınıza ekleme sürükle ve bırak desteği, içerik uygunluk farklı düzeylerde sağlayabilir; Örneğin, alıcı uygulamanın sürüklemenin hedefi olan en iyi uyan seçebilmeniz biçimlendirilmiş metin ve verilerin düz metin sürümünü sağlayabilir. Ayrıca Sürükle görselleştirmeyi özelleştirmek için ve aynı anda birden çok öğe sürükleyerek sağlamak mümkündür.

## <a name="drag-and-drop-with-text-controls"></a>Sürükle ve bırak metin denetimleri

`UITextView` ve `UITextField` seçili metni, sürükleyip metin içeriği otomatik olarak destekler.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Sürükle ve bırak ile UITableView

`UITableView` Sürükle ve bırak tablo satırları, varsayılan davranışı etkinleştirmek için yalnızca birkaç yöntem gerektiren etkileşim yerleşik olarak sahiptir.

Kullanılan iki arabirim vardır:

- `IUITableViewDragDelegate` – Bir Sürükle Tablo görünümünde başlatıldığında paketleri bilgileri.
- `IUITableViewDropDelegate` – Bir bırakma edilirken bilgileri işler çalıştı ve tamamlandı.

İçinde [DragAndDropTableView örnek](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) bu iki arabirim hem de şirket uygulanan `UITableViewController` temsilci ve veri kaynağı ile birlikte bir sınıf. İçinde atanmış oldukları `ViewDidLoad` yöntemi:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Bu iki arabirim için en düşük gerekli kod, aşağıda açıklanmıştır.

### <a name="table-view-drag-delegate"></a>Tablo görünümü Sürükle temsilci

Tek yöntem _gerekli_ Tablo görünümünden bir satır sürükleyerek desteklemektir `GetItemsForBeginningDragSession`. Kullanıcı bir satıra sürükleyin başlarsa, bu yöntem çağrılır.

Bir uygulama, aşağıda gösterilmiştir. Sürüklenen satırla ilişkili verileri alır, bu kodlar ve yapılandırır bir `NSItemProvider` uygulamaları işlemi "bırakma" bölümünü nasıl işleyeceğini belirler (örneğin, yoksa bunlar veri türü işleyebilir `PlainText`, örnekte):

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

Hedef uygulamalarında avantajlarından alınabilir birden çok veri gösterimlerini sağlamak gibi Sürükle davranışını özelleştirmek için uygulanabilir Sürükle temsilci üzerinde birçok isteğe bağlı yöntemler vardır (düz metin kutusu veya bir vektörü olarak biçimlendirilmiş metin gibi ve bit eşlem sürümleri çizim). Sürükleme ve bırakma aynı uygulama içinde kullanılacak özel veri ifadeleri de sağlayabilirsiniz.

### <a name="table-view-drop-delegate"></a>Tablo görünümü bırakma temsilci

Bir sürükleme işlemi tablo görünüm üzerinden gerçekleşir ya da tamamlanmadan bırakma temsilci yöntemleri çağrılır. Gerekli yöntemleri, verileri bırakılmasına izin verilip ve açılan tamamlanırsa hangi eylemler gerçekleştirildikçe belirler:

- `CanHandleDropSession` – Bir sürükleme ilerleme ve potansiyel olarak uygulamayı bırakılmakta olsa da, bu yöntem sürüklenen veri bırakılmasına izin verilip verilmediğini belirler.
- `DropSessionDidUpdate` – Sürükleme işlemi devam ederken, hangi eylemin hedeflenen belirlemek için bu yöntem çağrılır. Bilgiler üzerinden sürüklenen Tablo görünümü, sürükle oturumu ve olası dizin yolu tüm görsel geri bildirim kullanıcı tarafından sağlanan ve davranışını belirlemek için kullanılabilir.
- `PerformDrop` – Kullanıcının açılan (kendi parmak kaldırarak tarafından) tamamlandığında, bu yöntem sürüklenen verileri ayıklayan ve yeni satır (veya satır) veri eklemek için Tablo görünümü değiştirir.

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Tablo görünümü sürüklenen veri kabul edip edemeyeceğini gösterir. Bu kod parçacığında `CanLoadObjects` Bu tablo görünümü dize verileri kabul ettiğini doğrulamak için kullanılır.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` Yöntemi sürükleme işlemi sürüyor kullanıcıya görsel ipuçlarını sağlama ederken art arda çağrılır.

Aşağıdaki kodda `HasActiveDrag` işlemi geçerli bir Tablo görünümünde kaynağı olup olmadığını belirlemek için kullanılır. Bu durumda, yalnızca tek satır taşınmasına izin verilir.
Sürükleme başka bir kaynaktan ise, bir kopyalama işlemi gösterilir:

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

Bırakma işlemi olabilir `Cancel`, `Move`, veya `Copy`.

Bırakma hedefi yeni bir satır ekleyin veya mevcut bir satırı olan verilere ekleme/eklemeyi olabilir.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` Yöntemi çağrıldığında kullanıcı işlemi tamamlandığında ve devre dışı bırakılan verileri yansıtacak şekilde tablo görünümü ve veri kaynağı değiştirir.

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

Büyük veri nesnelerini zaman uyumsuz olarak yüklemek için ek kod eklenebilir.

### <a name="testing-drag-and-drop"></a>Test sürükle ve bırak

Test etmek için bir iPad kullanmalısınız [örnek](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Başka bir uygulama (örneğin, Notlar) yanı sıra örnek açın ve satır ve metin bunlar arasında sürükleyin:

![ekran görüntüsü sürükleme işlemi sürüyor](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>İlgili bağlantılar

- [Sürükle ve bırak İnsan Arabirimi yönergelerine (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Sürükle ve bırak Tablo görünümü örneği](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Sürükleme ve bırakma koleksiyon görünümü örneği](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Sürükle ve bırak (WWDC) (video) ile tanışın](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Koleksiyon ve Tablo görünümü (WWDC) (video) ile sürükleyip bırakın](https://developer.apple.com/videos/play/wwdc2017/223/)
