---
title: Tablo görünümleri
description: Bu makalede Xamarin.Mac uygulamasında tablo görünümlerle çalışma kapsar. Xcode arabirimi oluşturucu ve bunlarla kodda etkileşim tablo görünüm oluşturmayı açıklar.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: c274405613f079cb61ad9c96497a9effdc7173f5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="table-views"></a>Tablo görünümleri

_Bu makalede Xamarin.Mac uygulamasında tablo görünümlerle çalışma kapsar. Xcode arabirimi oluşturucu ve bunlarla kodda etkileşim tablo görünüm oluşturmayı açıklar._

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı erişiminiz tablo görünümleri, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve tablo görünümlerinizi korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

Bir tablo görünümü verileri birden çok satır bilgilerinin bir veya daha fazla sütun içeren bir tablo biçiminde görüntüler. Tablo oluşturulan görünüm türüne bağlı olarak, kullanıcı sütuna göre sıralamak, sütunları yeniden düzenlemek, sütun ekleme, sütunları kaldırmak veya tablo içinde bulunan verileri düzenleyin.

[![](table-view-images/intro01.png "Bir örnek tablo")](table-view-images/intro01.png#lightbox)

Bu makalede, sizi bir Xamarin.Mac uygulamasında tablosu görünümleri ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Giriş tablosu görünümleri

Bir tablo görünümü verileri birden çok satır bilgilerinin bir veya daha fazla sütun içeren bir tablo biçiminde görüntüler. Tablo görünümleri kaydırma görünümler içinde görüntülenir (`NSScrollView`) ve 10.7 macOS ile başlayarak, kullanabilirsiniz `NSView` hücreleri yerine (`NSCell`) satırları ve sütunları görüntülemek için. Bununla kullanmaya devam edebilirsiniz `NSCell` alt genellikle gerekir ancak `NSTableCellView` ve özel satırları ve sütunları oluşturun.

Bir tablo görünümü kendi veri depolamaz, bunun yerine bir veri kaynağında kullanır (`NSTableViewDataSource`) satır ve sütunları, gerektiği ölçüde temeline üzerinde gerekli sağlamak için.

Tablo görünümü temsilci öğesinin bir alt sağlayarak bir tablo görünümün davranışı özelleştirilebilir (`NSTableViewDelegate`) işlevselliği, satır seçimi ve düzenleme, özel izleme ve bireysel sütunlar için özel görünümler seçmek için tablo sütun Yönetimi desteklemek için yazın ve Satır.

Tablo görünümleri oluştururken, Apple aşağıdaki önerir:

* Kullanıcının tablo üzerinde sütun üstbilgilerini tıklatarak sıralama izin verin.
* İsimleri veya sütunda gösterildikten verileri tanımlayan kısa bir isim tümcecikleri sütun başlıklarını oluşturun.

Daha fazla bilgi için lütfen bkz [içerik görünümleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Oluşturma ve tablo görünümleri xcode'da koruma

Yeni bir Xamarin.Mac Cocoa uygulaması oluşturduğunuzda, varsayılan olarak standart boş, bir pencere alın. Bu windows tanımlanmış bir `.storyboard` otomatik olarak projeye dahil dosyası. Windows tasarımınızı düzenlemek için **Çözüm Gezgini**, çift tıklayarak `Main.storyboard` dosyası:

[![](table-view-images/edit01.png "Ana film şeridi seçme")](table-view-images/edit01.png#lightbox)

Bu pencere tasarım Xcode'nın arabirimi Oluşturucusu'nda açın:

[![](table-view-images/edit02.png "Xcode kullanıcı Arabiriminde düzenleme")](table-view-images/edit02.png#lightbox)

Tür `table` içine **kitaplığı Denetçisi'nin** Tablo görünümü denetimleri bulmayı kolaylaştırmak için arama kutusunu:

[![](table-view-images/edit03.png "Kitaplıktan bir tablo görünümü seçme")](table-view-images/edit03.png#lightbox)

Bir tablo görünümü Görünüm denetleyiciye sürükleyin **arabirimi Düzenleyicisi**, görünüm denetleyicisini içerik alanını doldurun ve burada küçültür ve penceresinde ile büyür ayarlanan **kısıtlaması Düzenleyicisi**:

[![](table-view-images/edit04.png "Kısıtlamaları düzenleme")](table-view-images/edit04.png#lightbox)

Tablo görünümünde seçin **arabirimi hiyerarşi** ve aşağıdaki özellikler mevcuttur **özniteliği denetçisi**:

[![](table-view-images/edit05.png "Öznitelik denetçisi")](table-view-images/edit05.png#lightbox)

- **İçerik modu** -ya da görünümleri kullanmanıza olanak tanır (`NSView`) veya hücreleri (`NSCell`) satır ve sütunları verileri görüntülemek için. MacOS 10.7 ile başlayarak, görünümleri kullanmanız gerekir.
- **Gezinen grup satırları** - `true`, kayan gibi Tablo görünümünde gruplandırılmış hücreleri çizin.
- **Sütunları** -görüntülenen sütunların sayısını tanımlar.
- **Üstbilgileri** - `true`, sütunları üst bilgileri içerir.
- **Yeniden sıralama** - `true`, kullanıcı sürükleyin açabilecektir tablodaki sütunlar yeniden Sırala.
- **Yeniden boyutlandırma** - `true`, kullanıcı sütunları yeniden boyutlandırmak için sütun üst bilgileri sürükleyin mümkün olacaktır.
- **Sütun boyutlandırma** -tablonun boyutu sütunları otomatik nasıl denetler.
- **Vurgula** -tablo vurgulama türünü kullanan bir hücre seçildiğinde kontrol eder.
- **Alternatif satır** - `true`, herhangi bir zamanda diğer satır farklı arka plan rengi olur.
- **Yatay Kılavuz** -arasındaki hücrelere yatay olarak çizileceğini kenarlık türünü seçer.
- **Dikey kılavuz** -arasındaki hücrelere dikey olarak çizilen kenarlık türünü seçer.
- **Kılavuz rengi** -hücre kenarlık rengini belirler.
- **Arka plan** -hücre arka plan rengini belirler.
- **Seçim** -kullanıcı tabloda hücrelerin nasıl seçebilir denetlemenize izin:
    - **Birden çok** - `true`, kullanıcı birden çok satır ve sütunları seçebilir.
    - **Sütun** - `true`, kullanıcı sütunları seçebilirsiniz.
    - **Türü seç** - `true`, kullanıcı bir sırayı seçmek için bir karakter yazabilirsiniz.
    - **Boş** - `true`, kullanıcının gerekli değildir bir satır veya sütun seçmek için tablonun herhangi bir seçim hiç izin verir.
- **Otomatik kaydetme** -tabloları biçim adını otomatik olarak kaydetmek altında.
- **Sütun bilgileri** - `true`, sıra ve sütunların genişliğini otomatik olarak kaydedilir.
- **Satır sonları** - hücre, satır sonları işleme yöntemini seçin.
- **Görünen son satırın kesen** - `true`, hücre uygulamasındaki kendi sınırları içinde uygun değildir verileri kesilecek.

> [!IMPORTANT]
> Eski bir Xamarin.Mac Uygulama Bakımı sürece `NSView` üzerinden tabanlı tablosu görünümleri kullanılmalıdır `NSCell` tablosu görünümleri tabanlı. `NSCell` eski olarak kabul edilir ve ileride desteklenmiyor olabilir.

Bir tablo sütununda seçin **arabirimi hiyerarşi** ve aşağıdaki özellikler mevcuttur **özniteliği denetçisi**:

[![](table-view-images/edit06.png "Öznitelik denetçisi")](table-view-images/edit06.png#lightbox)

- **Başlık** -sütun başlığını ayarlar.
- **Hizalama** -hücrelerde metin hizalamasını ayarlama.
- **Başlık yazı tipi** -hücre üst bilgi metninin yazı tipini seçer.
- **Sıralama anahtarı** -sütunundaki verileri sıralamak için kullanılan anahtar. Kullanıcı Bu sütun sıralayamazsınız boş bırakın.
- **Seçici** -olan **eylem** sıralamayı gerçekleştirmek için kullanılır. Kullanıcı Bu sütun sıralayamazsınız boş bırakın.
- **Sipariş** -sütunları veriler için sıralama düzeni.
- **Yeniden boyutlandırma** -sütunu için yeniden boyutlandırma türünü seçer.
- **Düzenlenebilir** - `true`, kullanıcı tabanlı hücre tablodaki hücreleri düzenleyebilirsiniz.
- **Gizli** - `true`, sütun gizlenir.

Buna ait (sütunun sağ tarafta dikey ortalanmış) tanıtıcısı sola veya sağa sürükleyerek de sütunu yeniden boyutlandırabilirsiniz.

Şimdi, Tablo görünümünde her sütunu seçin ve ilk sütun verin bir **başlık** , `Product` ve ikincisi `Details`.

Tablo Hücre görünümünü seçin (`NSTableViewCell`) içinde **arabirimi hiyerarşi** ve aşağıdaki özellikler mevcuttur **özniteliği denetçisi**:

[![](table-view-images/edit07.png "Öznitelik denetçisi")](table-view-images/edit07.png#lightbox)

Bu standart bir görünüm özelliklerini tümü. Burada bu sütun için satırları yeniden boyutlandırma seçeneğiniz de vardır.

Bir tablo görünümü hücre seçin (varsayılan olarak, bir `NSTextField`) içinde **arabirimi hiyerarşi** ve aşağıdaki özellikler mevcuttur **özniteliği denetçisi**:

[![](table-view-images/edit08.png "Öznitelik denetçisi")](table-view-images/edit08.png#lightbox)

Burada ayarlamak için standart bir metin alanı tüm özelliklerine sahip olacaksınız. Varsayılan olarak, standart bir metin alanı, bir sütundaki bir hücrenin verilerini görüntülemek için kullanılır.

Tablo Hücre görünümünü seçin (`NSTableFieldCell`) içinde **arabirimi hiyerarşi** ve aşağıdaki özellikler mevcuttur **özniteliği denetçisi**:

[![](table-view-images/edit09.png "Öznitelik denetçisi")](table-view-images/edit09.png#lightbox)

Burada en önemli ayarlar şunlardır:

- **Düzen** - bu sütundaki hücrelerin düzenlendiği yöntemini seçin.
- **Tek satırlı moda kullanan** - `true`, tek bir satır hücre sınırlıdır.
- **İlk çalışma zamanı düzen genişliği** - `true`, hücre için (el ile veya otomatik olarak) olarak ayarlandığında genişliği tercih eder uygulama çalıştırıldığında ilk kez görüntülenir.
- **Eylem** -ne zaman denetimleri düzenleme **eylem** hücre için gönderilir.
- **Davranış** -bir hücre seçilebilir veya düzenlenebilir ise tanımlar.
- **Zengin metin** - `true`, hücre biçimlendirilmiş ve stilde metin görüntüleyebilirsiniz.
- **Geri** - `true`, bunun için hücre sorumluluğu üstlenir davranışı geri al.

Tablo hücre görünümü seçin (`NSTableFieldCell`) bir tablo sütununda sonundaki **arabirimi hiyerarşi**:

[![](table-view-images/edit10.png "Tablo Hücre görünümünü seçme")](table-view-images/edit10.png#lightbox)

Bu sayede temel olarak kullanılan tablo hücre görünümünü düzenlemek _düzeni_ belirtilen sütun için oluşturulan tüm hücreler için.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Eylemler ve çıkışlar ekleme

Yalnızca bizim Tablo görünümü kullanıma sunmak ihtiyacımız diğer Cocoa UI denetimi gibi ve sütunları ve hücreleri C# kod kullanarak **Eylemler** ve **çıkışlar** (gerekli işlevselliğine bağlı).

Biz kullanıma sunmak istediğiniz herhangi bir tablo görünümü öğesi işlemi aynıdır:

1. Geçiş **Yardımcısı Düzenleyicisi** ve emin `ViewController.h` dosya seçili: 

    [![](table-view-images/edit11.png "Yardımcısı Düzenleyicisi")](table-view-images/edit11.png#lightbox)
2. Tablo görünümünden seçin **arabirimi hiyerarşi**denetim tıklatın ve sürükleyin `ViewController.h` dosya.
3. Oluşturma bir **çıkışı** Tablo görünümünde adlı için `ProductTable`: 

    [![](table-view-images/edit13.png "Prizine yapılandırma")](table-view-images/edit13.png#lightbox)
4. Oluşturma **çıkışlar** de tablo sütunlarını adlı `ProductColumn` ve `DetailsColumn`: 

    [![](table-view-images/edit14.png "Prizine yapılandırma")](table-view-images/edit14.png#lightbox)
5. Değişiklikleri kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Ardından, uygulamayı çalıştırdığınızda şu kodu görüntüleme tablo bazı verileri yazacaksınız.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Tablo görünümünde doldurma

Bizim Tablo görünümü ile tasarlanmış arabirim Oluşturucusu'nda ve aracılığıyla kullanıma sunulan bir **çıkışı**, ardından biz bunu doldurmak için C# kodu oluşturmanız gerekir.

İlk olarak, yeni bir oluşturalım `Product` tek tek satırların bilgiyi tutmak için sınıf. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `Product` için **adı** tıklatıp **yeni** düğmesi:

[![](table-view-images/populate01.png "Boş bir sınıf oluşturma")](table-view-images/populate01.png#lightbox)

Olun `Product.cs` aşağıdaki gibi dosya bakın:

```csharp
using System;

namespace MacTables
{
    public class Product
    {
        #region Computed Propoperties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}

```

Ardından, bir alt sınıfı oluşturmak ihtiyacımız `NSTableDataSource` , istendiği bizim tablosu için veri sağlamak için. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `ProductTableDataSource` için **adı** tıklatıp **yeni** düğmesi.

Düzen `ProductTableDataSource.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDataSource : NSTableViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Products.Count;
        }
        #endregion
    }
}

```

Bu sınıf bizim tablo görünümün öğeleri için depolama alanına sahip ve geçersiz kılmalar `GetRowCount` tablodaki satır sayısını dönün.

Son olarak, bir alt sınıfı oluşturmak ihtiyacımız `NSTableDelegate` bizim tablosu için davranışı sağlamak için. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `ProductTableDelegate` için **adı** tıklatıp **yeni** düğmesi.

Düzen `ProductTableDelegate.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDelegate: NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductTableDataSource DataSource;
        #endregion

        #region Constructors
        public ProductTableDelegate (ProductTableDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = DataSource.Products [(int)row].Title;
                break;
            case "Details":
                view.StringValue = DataSource.Products [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Bir örneğini oluştururken biz `ProductTableDelegate`, biz de bir örnekte geçirmek `ProductTableDataSource` tablo için veriler sağlar. `GetViewForItem` Yöntemdir hücre verin sütun ve satır için görüntülenecek bir görünüm (veri) döndürmek için sorumlu. Mümkünse, var olan bir görünümü hücre görüntülemek için yeniden kullanılabilir değilse yeni bir görünüm oluşturulması gerekir.

Tabloyu doldurmak için düzenleyelim `ViewController.cs` dosya ve olun `AwakeFromNib` yöntemi görünüm aşağıdaki gibi:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

Biz uygulama çalıştırıyorsanız, aşağıdaki görüntülenir:

[![](table-view-images/populate02.png "Çalıştıran bir örnek uygulama")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sütuna göre sıralama

Şimdi tablodaki verileri bir sütun başlığına tıklayarak sıralayın izin verin. İlk olarak, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Seçin `Product` sütun girin `Title` için **sıralama anahtarı**, `compare:` için **Seçici** seçip `Ascending` için **sipariş**:

[![](table-view-images/sort01.png "Sıralama anahtarı ayarlama")](table-view-images/sort01.png#lightbox)

Seçin `Details` sütun girin `Description` için **sıralama anahtarı**, `compare:` için **Seçici** seçip `Ascending` için **sipariş**:

[![](table-view-images/sort02.png "Sıralama anahtarı ayarlama")](table-view-images/sort02.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Şimdi düzenleyelim `ProductTableDataSource.cs` dosya ve aşağıdaki yöntemleri ekleyin:

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    case "Description":
        if (ascending) {
            Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
        } else {
            Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
        }
        break;
    }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    if (oldDescriptors.Length > 0) {
        // Update sort
        Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    } else {
        // Grab current descriptors and update sort
        NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
        Sort (tbSort[0].Key, tbSort[0].Ascending); 
    }
            
    // Refresh table
    tableView.ReloadData ();
}
```

`Sort` Yöntemi izin bize temel veri kaynağındaki verileri sıralamak bir verilen `Product` artan veya azalan sınıfı alanı. Geçersiz kılınan `SortDescriptorsChanged` bir sütun başlığı kullanımı tıklar her zaman yöntemi çağrılır. Kendisine geçirilen **anahtar** arabirimi oluşturucu ve bu sütun için sıralama düzenini ayarlarız değeri.

Uygulamayı çalıştırın ve sütun başlıklarının'ı tıklatın, satır sütuna göre sıralanır:

[![](table-view-images/sort03.png "Bir örnek uygulamayı çalıştırma")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Satır seçimi

Tek bir satır seçin, çift kullanıcıya izin vermek isteyip istemediğinizi `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Tablo görünümünde seçin **arabirimi hiyerarşi** ve işaretini **birden çok** onay kutusu **özniteliği denetçisi**:

[![](table-view-images/select01.png "Öznitelik denetçisi")](table-view-images/select01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.


Ardından, düzenleme `ProductTableDelegate.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Bu, kullanıcının tek bir satır Tablo görünümünde seçmesine izin verir. Dönüş `false` için `ShouldSelectRow` herhangi bir satır için seçmek kullanıcı istemediğiniz veya `false` tüm satırları seçmek kullanıcı istemiyorsanız, her satır için.

Tablo görünümünde (`NSTableView`) satır seçimi ile çalışmak için aşağıdaki yöntemleri içerir:

- `DeselectRow(nint)` -Tabloda verilen satır seçimini kaldırır.
- `SelectRow(nint,bool)` -Belirtilen satır seçer. Geçirmek `false` aynı anda yalnızca bir satır seçmek ikinci parametre için.
- `SelectedRow` -Tabloda seçilen geçerli satır döndürür.
- `IsRowSelected(nint)` -Döndürür `true` verilen satır seçtiyseniz.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Birden çok satır seçimi

Birden çok satır seçmek, çift kullanıcıya izin vermek isteyip istemediğinizi `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Tablo görünümünde seçin **arabirimi hiyerarşi** ve denetleme **birden çok** onay kutusu **özniteliği denetçisi**:

[![](table-view-images/select02.png "Öznitelik denetçisi")](table-view-images/select02.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.


Ardından, düzenleme `ProductTableDelegate.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Bu, kullanıcının tek bir satır Tablo görünümünde seçmesine izin verir. Dönüş `false` için `ShouldSelectRow` herhangi bir satır için seçmek kullanıcı istemediğiniz veya `false` tüm satırları seçmek kullanıcı istemiyorsanız, her satır için.

Tablo görünümünde (`NSTableView`) satır seçimi ile çalışmak için aşağıdaki yöntemleri içerir:

- `DeselectAll(NSObject)` -Tablodaki tüm satırları seçimini kaldırır. Kullanım `this` seçerek yapılması nesnesinde göndermek ilk parametresi için. 
- `DeselectRow(nint)` -Tabloda verilen satır seçimini kaldırır.
- `SelectAll(NSobject)` -Tablodaki tüm satırları seçer. Kullanım `this` seçerek yapılması nesnesinde göndermek ilk parametresi için.
- `SelectRow(nint,bool)` -Belirtilen satır seçer. Geçirmek `false` ikinci parametre için seçimi temizleyin ve yalnızca tek bir satır seçin, geçirin `true` seçimi genişletir ve bu satır eklemek için.
- `SelectRows(NSIndexSet,bool)` -Belirtilen satır kümesini seçer. Geçirmek `false` ikinci parametre için seçimi temizleyin ve yalnızca bir bunlar seçin satır geçirme `true` seçimi genişletir ve bu satırları ekler.
- `SelectedRow` -Tabloda seçilen geçerli satır döndürür.
- `SelectedRows` -Döndüren bir `NSIndexSet` seçili satırları dizinlerini içeren.
- `SelectedRowCount` -Seçilen satır sayısını döndürür.
- `IsRowSelected(nint)` -Döndürür `true` verilen satır seçtiyseniz.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Sırayı seçmek için türü

İlk satırı seçin ve bir karakter ile tablo seçili görünüm türünü kullanıcıya izin vermek istiyorsanız, bu karakter olan, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Tablo görünümünde seçin **arabirimi hiyerarşi** ve denetleme **türünü seçin** onay kutusu **özniteliği denetçisi**:

[![](table-view-images/type01.png "Seçim türünü ayarlama")](table-view-images/type01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Şimdi düzenleyelim `ProductTableDelegate.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
    nint row = 0;
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains(searchString)) return row;

        // Increment row counter
        ++row;
    }

    // If not found select the first row
    return 0;
}
```

`GetNextTypeSelectMatch` Yöntemi alır verilen `searchString` ve ilk satırının döndürür `Product` , o dizeyi 's içerdiği `Title`.

Uygulamayı çalıştırın ve bir karakter yazın, bir satır seçilir:

[![](table-view-images/type02.png "Çalıştıran bir örnek uygulama")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Sütunları yeniden sıralama

Sürükleme yapmalarına izin vermek istiyorsanız, Tablo görünümünde sütunları yeniden sıralama, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Tablo görünümünde seçin **arabirimi hiyerarşi** ve denetleme **Reordering** onay kutusu **özniteliği denetçisi**:

[![](table-view-images/reorder01.png "Öznitelik denetçisi")](table-view-images/reorder01.png#lightbox)

Biz için bir değer verirseniz **otomatik kaydetme** özelliği ve onay **sütun bilgileri** alan tablonun düzene vermiyoruz değişiklikleri bize otomatik olarak kaydedilir ve uygulama başlatıldığında geri çalıştırılır.

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Şimdi düzenleyelim `ProductTableDelegate.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Yöntemi döndürmelidir `true` istediğiniz olmasını sağlamak için herhangi bir sütun için içine sürükleyin kaldırılmasında `newColumnIndex`, aksi takdirde dönüş `false`;

Biz uygulama çalıştırırsanız, biz bizim sütunları yeniden sıralamak için sütun başlıkları geçici sürükleyebilirsiniz:

[![](table-view-images/reorder02.png "Yeniden düzenlenen sütunları örneği")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Hücre düzenleme

Belli bir hücre değerlerini Düzenle, Düzenle kullanıcıya izin vermek istiyorsanız `ProductTableDelegate.cs` dosya ve değişiklik `GetViewForItem` yöntemini aşağıdaki şekilde:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = true;

        view.EditingEnded += (sender, e) => {
                    
            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.Tag].Title = view.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.Tag].Description = view.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Şimdi biz uygulama çalıştırırsanız, kullanıcı Tablo görünümünde hücreleri düzenleyebilirsiniz:

[![](table-view-images/editing01.png "Bir hücre düzenleme örneği")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Tablo görünümlerde görüntüleri kullanma

Hücrenin bir parçası olarak bir resim eklemek için bir `NSTableView`, tablo görünümün tarafından döndürülen veriler nasıl değiştirmeniz gerekir `NSTableViewDelegate's` `GetViewForItem` yöntemini kullanmak üzere bir `NSTableCellView` tipik yerine `NSTextField`. Örneğin:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Daha fazla bilgi için lütfen bkz [kullanarak görüntüleri tablosu görünümleri ile](~/mac/app-fundamentals/image.md) bölümünü bizim [görüntüsüyle çalışma](~/mac/app-fundamentals/image.md) belgeleri.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Satır Sil düğme ekleme

Uygulamanızın gereksinimlerine bağlı olarak, olabilir durumlar burada tablodaki her satır için eylem düğmesi sağlamanız gerekir. Bu örnek olarak, şimdi eklemek için yukarıda oluşturduğunuz Tablo görünümü örnek genişletin bir **silmek** her satırda düğmesi.

İlk olarak, düzenleme `Main.storyboard` Xcode'nın arabirimi Oluşturucu'da, Tablo görünümünde seçin ve sütun sayısını artırmak üç (3). Ardından, değiştirme **başlık** yeni sütunun `Action`:

[![](table-view-images/delete01.png "Sütun adı düzenleme")](table-view-images/delete01.png#lightbox)

Film şeridi için değişiklikleri kaydetmek ve değişiklikleri eşitlemek Mac için Visual Studio geri dönün.

Ardından, düzenleme `ViewController.cs` dosya ve ortak aşağıdaki yöntemi ekleyin:

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

Yeni Tablo görünümü temsilci içinde oluşturulmasını aynı dosyada değişiklik `ViewDidLoad` yöntemini aşağıdaki şekilde:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Şimdi, düzenlemek `ProductTableDelegate.cs` dosya görünüm denetleyiciye özel bir bağlantı içerir ve denetleyici temsilci yeni bir örneğini oluştururken, bir parametre olarak almak için:

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
    this.Controller = controller;
    this.DataSource = datasource;
}
#endregion
```

Ardından, aşağıdaki yeni özel yöntem sınıfına ekleyin:

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
    // Add to view
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);

    // Configure
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    // Wireup events
    view.TextField.EditingEnded += (sender, e) => {

        // Take action based on type
        switch (view.Identifier) {
        case "Product":
            DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
            break;
        case "Details":
            DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
            break;
        }
    };

    // Tag view
    view.TextField.Tag = row;
}
```

Bu işlem tüm önceden gerçekleştirilen metin görünümü yapılandırmalarını sürer `GetViewForItem` yöntemi ve (tablonun son sütunu metin görünümü, ancak bir düğme içermediğinden) bunları aranabilir, tek bir konumda yerleştirir.

Son olarak, düzenleme `GetViewForItem` yöntemi ve şu şekilde görünür yapın:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();

        // Configure the view
        view.Identifier = tableColumn.Title;

        // Take action based on title
        switch (tableColumn.Title) {
        case "Product":
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Details":
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Action":
            // Create new button
            var button = new NSButton (new CGRect (0, 0, 81, 16));
            button.SetButtonType (NSButtonType.MomentaryPushIn);
            button.Title = "Delete";
            button.Tag = row;

            // Wireup events
            button.Activated += (sender, e) => {
                // Get button and product
                var btw = sender as NSButton;
                var product = DataSource.Products [(int)btn.Tag];

                // Configure alert
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
                    MessageText = $"Delete {product.Title}?",
                };
                alert.AddButton ("Cancel");
                alert.AddButton ("Delete");
                alert.BeginSheetForResponse (Controller.View.Window, (result) => {
                    // Should we delete the requested row?
                    if (result == 1001) {
                        // Remove the given row from the dataset
                        DataSource.Products.RemoveAt((int)btn.Tag);
                        Controller.ReloadTable ();
                    }
                });
            };

            // Add to view
            view.AddSubview (button);
            break;
        }

    }

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tag.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        view.TextField.Tag = row;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        view.TextField.Tag = row;
        break;
    case "Action":
        foreach (NSView subview in view.Subviews) {
            var btw = subview as NSButton;
            if (btw != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

Bu kodu daha ayrıntılı çeşitli bölümlerini bakalım. İlk olarak, yeni `NSTableViewCell` olan eylem oluşturulan alınır tabanlı sütunun adını. İlk iki sütun için (**ürün** ve **ayrıntıları**), yeni `ConfigureTextField` yöntemi çağrılır.

İçin **eylem** sütun, yeni bir `NSButton` oluşturulur ve bir alt görünüm olarak hücreye eklenir:

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

Düğmenin `Tag` özelliği şu anda işleniyor satır sayısını tutmak için kullanılır. Kullanıcı bir satır düğmenin içinde silinecek istediğinde bu numarayı daha sonra kullanılacak `Activated` olay:

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btw = sender as NSButton;
    var product = DataSource.Products [(int)btn.Tag];

    // Configure alert
    var alert = new NSAlert () {
        AlertStyle = NSAlertStyle.Informational,
        InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
        MessageText = $"Delete {product.Title}?",
    };
    alert.AddButton ("Cancel");
    alert.AddButton ("Delete");
    alert.BeginSheetForResponse (Controller.View.Window, (result) => {
        // Should we delete the requested row?
        if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
        }
    });
};
```

Olay işleyicisi başlangıcında düğmesi ve verilen tablo satırı olduğu ürünün alın. Daha sonra bir uyarı satır silme onayı kullanıcıya sunulur. Kullanıcının satırı silmek seçerse, verilen satır veri kaynağından kaldırılır ve tablonun geri yüklenir:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Son olarak, yeni oluşturulan yerine tablo görünümü hücresi yeniden kullanılıyor, aşağıdaki kodu işlenmekte olan sütuna göre yapılandırır:

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
case "Action":
    foreach (NSView subview in view.Subviews) {
        var btw = subview as NSButton;
        if (btw != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

İçin **eylem** tüm alt görünümleri sütun, taranan kadar `NSButton` bulunur, bu ise `Tag` özelliği geçerli satırındaki işaret edecek şekilde güncelleştirilir.

Yerinde bu değişikliklerle uygulama çalıştırıldığında her satır olacaktır bir **silmek** düğmesi:

[![](table-view-images/delete02.png "Silme düğmeleri olan tablo görünümü")](table-view-images/delete02.png#lightbox)

Kullanıcı tıkladığında bir **silmek** düğmesi, kendilerine verilen satır silme soran bir uyarı görüntülenir:

[![](table-view-images/delete03.png "Bir silme satır Uyarısı")](table-view-images/delete03.png#lightbox)

Kullanıcı silme seçerse, satır kaldırılır ve tablo çizilir:

[![](table-view-images/delete04.png "Tablonun satır silindikten sonra")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Veri bağlama tablosu görünümleri

Xamarin.Mac uygulamanızda anahtar-değer kodlama ve veri bağlama teknikleri kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız için sahip kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma faydası vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), başında bakımı kolay, daha esnek uygulama için Tasarım.

Anahtar-değer kodlama (KVC) olan bir nesnenin özelliklerine dolaylı olarak erişme, örnek değişkenleri erişim yerine özelliklerini tanımlamak için (özel olarak biçimlendirilmiş dizeler) anahtarları veya erişimci yöntemlerini kullanarak için bir mekanizma (`get/set`). Anahtar-değer kodlama uyumlu erişimcilerini Xamarin.Mac uygulamanızda uygulayarak, anahtar-değer Gözlemleme (KVO), veri bağlama, çekirdek verileri, Cocoa bağlamalar ve scriptability gibi diğer macOS özelliklerine erişim sahibi olursunuz.

Daha fazla bilgi için lütfen bkz [Tablo görünümü verileri bağlama](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) bölümünü bizim [verileri bağlama ve anahtar-değer kodlama](~/mac/app-fundamentals/databinding.md) belgeleri.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede bir Xamarin.Mac uygulamasında tablo görünümleriyle çalışma ayrıntılı bir bakış sürdü. Biz farklı türleri gördünüz ve tablo görünümleri, oluşturma ve Xcode'nın arabirimi Oluşturucu tablosu görünümleri korumak ve C# kodunda tablosu görünümleri ile çalışmak nasıl kullanır.

## <a name="related-links"></a>İlgili bağlantılar

- [MacTables (örnek)](https://developer.xamarin.com/samples/mac/MacTables/)
- [MacImages (örnek)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Anahat Görünümleri](~/mac/user-interface/outline-view.md)
- [Kaynak Listeleri](~/mac/user-interface/source-list.md)
- [Veri Bağlama ve Anahtar-Değer Kodlaması](~/mac/app-fundamentals/databinding.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
