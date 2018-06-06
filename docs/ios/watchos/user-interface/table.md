---
title: watchOS Xamarin tablo denetimleri
description: Bu belge, Xamarin içinde watchOS tablo denetimleri kullanmayı açıklar. Bir tablo ekleme, satır denetleyicisi ekleme, oluşturma ve dokunma ve daha fazla bilgi için yanıt satırları doldurmak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: afb8f9a96fa14877cbd0352869e23972719a4480
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791364"
---
# <a name="watchos-table-controls-in-xamarin"></a>watchOS Xamarin tablo denetimleri

WatchOS `WKInterfaceTable` denetimi iOS karşılığı çok daha kolaydır, ancak benzer bir rolü gerçekleştirir. Kaydırma, özel düzenler olabilir ve dokunma olayları yanıt satır listesini oluşturur.

![](table-images/table-list-sml.png "Gözcü Tablo listesi") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Bir tablo ekleme

Sürükleme **tablo** bir Sahne denetime. Varsayılan olarak bu (tek belirtilmeyen satır düzeni gösteren gibi) görünür:

[![](table-images/add-table-sml.png "Bir tablo ekleme")](table-images/add-table.png#lightbox)

Tablo bir ad verin **özellikleri** paneli'nın **adı** kutusunda, böylece bu kodda başvurulabilen.

## <a name="add-a-row-controller"></a>Satır denetleyici ekleme

Tablonun otomatik olarak içeren bir satır denetleyicisi tarafından temsil edilen tek bir satır içeren bir **grup** varsayılan denetim.

Ayarlamak için **sınıfı** satır denetleyici için satır seçtikten **belge anahattı** ve bir sınıf adı yazın **özellikleri** paneli:

[![](table-images/add-row-controller-sml.png "Bir sınıf adı özellikleri defterinde girme")](table-images/add-row-controller.png#lightbox)

Sıranın denetleyicisi için sınıf ayarladıktan sonra IDE karşılık gelen bir C# dosyasına projesi oluşturun. Denetimleri (örneğin, etiketleri) satırın sürükleyin ve bunlar kodda başvurulabilen şekilde adlar verin.




## <a name="create-and-populate-rows"></a>Oluşturma ve satır doldurma

`SetNumberOfRows` Satır denetleyicisi sınıflar her biri için oluşturur satır, kullanarak `Identifier` doğru olanı seçin. Özel bir satırını denetleyicinizi verdiyse `Identifier`, değiştirme **varsayılan** kullandığınız tanımlayıcıyla kod parçacığında bulunan. `RowController` *Her satır için* ne zaman oluşturulur `SetNumberOfRows` olarak adlandırılır ve görüntülenen tablo.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> İOS gibi tablo satırları sanallaştırılmış değil. (Apple 20'den az önerir) satır sayısını sınırlamak deneyin.

Satırları oluşturduktan sonra her bir hücre doldurmanız gerekir (gibi `GetCell` iOS yapın). Bu kod parçacığını gelen [WatchTables örnek](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/) her satır etiketinde güncelleştirir

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> Kullanarak `SetNumberOfRows` ve kullanarak, döngü `GetRowController` saate gönderilmek üzere tüm tablo neden olur. Tablonun sonraki görünümlerinde belirli satırlar eklemek veya kaldırmak gerekiyorsa kullanın `InsertRowsAt` ve `RemoveRowsAt` daha iyi performans için.


## <a name="respond-to-taps"></a>Dokunma için yanıt

Satır seçimi için iki farklı şekilde yanıt verebilir:

- Uygulama `DidSelectRow` arabirimi denetleyicisinde yöntemi veya
- bir segue şeridinde oluşturmak ve uygulamak `GetContextForSegue` başka bir görünüm açmak için satır seçimi istiyorsanız.

### <a name="didselectrow"></a>DidSelectRow

Program aracılığıyla satır seçimi işlemek için uygulama `DidSelectRow` yöntemi. Yeni bir görünüm açmak için kullanmak `PushController` ve Sahne 's tanımlayıcısı ve kullanmak için veri bağlamı geçirin:

```csharp
public override void DidSelectRow (WKInterfaceTable table, nint rowIndex)
{
    var rowData = rows [(int)rowIndex];
    Console.WriteLine ("Row selected:" + rowData);
    // if selection should open a new scene
    PushController ("secondInterface", rows[(int)rowIndex]);
}
```

### <a name="getcontextforsegue"></a>GetContextForSegue

Bir segue şeridinde tablo satırdan başka bir Sahne sürükleme (basılı **denetim** sürükleme sırasında anahtar).
Mutlaka segue seçin ve tanımlayıcısını belirtin **özellikleri** paneli (gibi `secondLevel` aşağıdaki örnekte).

Arabirim Denetleyicisi uygulamak `GetContextForSegue` yöntemi ve segue tarafından sunulan Sahne sağlanmalıdır veri bağlamı dönün.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Bu veriler hedef film şeridi Sahne geçirilir kendi `Awake` yöntemi.

## <a name="multiple-row-types"></a>Birden çok satır türleri

Varsayılan olarak, tasarlayabilirsiniz tek satır türü tablo denetim sahiptir. Daha fazla satır 'şablon' kullanmak eklemek için **satırları** kutusunda **özellikleri** paneli daha fazla satır denetleyicileri oluşturmak için:

![](table-images/prototype-rows1.png "Prototip satır sayısını ayarlama")

Ayarı **satırları** özelliğine **3** denetimlere sürükleme için ek bir satır yer tutucuları oluşturur. Her satır için ayarlanmış **sınıfı** ad bulunan **özellikleri** satır denetleyici sınıfı oluşturulur emin olmak için paneli.

![](table-images/prototype-rows2.png "Tasarımcıda prototip satırları")

Farklı satır türleri kullanımıyla bir tabloyu doldurmak için `SetRowTypes` yöntemi tablodaki her satır için kullanılacak satır denetleyici türü belirtin. Hangi satır denetleyicisi her satır için kullanılması gerektiğini belirtmek için satırın tanımlayıcılar kullanın.

Bu dizide öğe sayısı tabloda beklerler satır sayısını eşleşmesi gerekir:

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

Birden çok satır denetleyicileri içeren bir tablo doldurulurken UI doldurmak gibi beklediğiniz hangi tür izlemek gerekir:

```csharp
for (var i = 0; i < rows.Count; i++) {
    if (i == 0) {
        var elementRow = (Type1RowController)myTable.GetRowController (i);
        // populate UI controls
    } else if (i == 3) {
        var elementRow = (Type2RowController)myTable.GetRowController (i);
        // populate UI controls
    } else {
        var elementRow = (DefaultRowController)myTable.GetRowController (i);
        // populate UI controls
    }
}
```


## <a name="vertical-detail-paging"></a>Dikey ayrıntı disk belleği

watchOS 3 tablolar için yeni bir özellik sunulmuştur: ayrıntı sayfaları arasında kaydırma yeteneği tabloya geri dönün ve başka bir satır seçin gerek kalmadan her satır için ilgili. Ayrıntı ekranlar yukarı ve aşağı geçirmeyi veya dijital Dama yapma kullanarak kaydırılabileceğini.

![](table-images/table-scroll-sml.png "Dikey ayrıntı disk belleği örneği") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> Bu özellik şu anda yalnızca Xcode arabirimi Oluşturucu film şeridi düzenleyerek kullanılabilir.

Bu özelliği etkinleştirmek için seçin `WKInterfaceTable` tasarım yüzeyi ve onay **dikey ayrıntı disk belleği** seçeneği:

![](table-images/vertical-detail-paging-sml.png "Dikey ayrıntı disk belleği seçeneği")

Olarak [Apple tarafından açıklanan](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) tablo gezinmeyi kullanmalısınız disk belleği özelliğinin çalışması için segues. Kullanan var olan kodu yeniden yazma `PushController` kullanmak için bunun yerine segues.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>Ek: Satır denetleyicisi kod örneği

Bir satır denetleyicisi Tasarımcısı'nda oluşturulduğunda IDE iki kod dosyaları otomatik olarak oluşturur. Bu oluşturulan dosyaları kodda başvurusunu aşağıda gösterilmiştir.

İlk sınıf için örneğin adlandırılacağını **RowController.cs**, şöyle:

```csharp
using System;
using Foundation;

namespace WatchTablesExtension
{
    public partial class RowController : NSObject
    {
        public RowController ()
        {
        }
    }
}
```

Diğer **. designer.cs** dosyadır çıkışlar ve bu örnek biriyle gibi tasarımcı yüzey üzerinde oluşturulan eylemleri içeren bir parçalı sınıf tanımı `WKInterfaceLabel` denetimi:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using UIKit;

namespace WatchTables.OnWatchExtension
{
    [Register ("RowController")]
    partial class RowController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        public WatchKit.WKInterfaceLabel MyLabel { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (MyLabel != null) {
                MyLabel.Dispose ();
                MyLabel = null;
            }
        }
    }
}
```

Çıkışlar ve Eylemler burada bildirilen kodda - sonra başvurulabilir ancak **. designer.cs** dosyası değil düzenlenmesi gerekir doğrudan.



## <a name="related-links"></a>İlgili bağlantılar

- [WatchTables (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple'nın tablo belge](https://developer.apple.com/reference/watchkit/wkinterfacetable)
