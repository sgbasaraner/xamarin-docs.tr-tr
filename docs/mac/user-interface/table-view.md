---
title: Xamarin.Mac tablo görünümleri
description: Bu makale, bir Xamarin.Mac uygulamasını tablo görünümlerde çalışmak kapsar. Bu, Xcode ve arabirim oluşturucu ve kodda normalde tabloda görünüm oluşturmayı açıklar.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 68b52fb4b7a3a65b45fcbecdc865bc64d9865fd9
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780613"
---
# <a name="table-views-in-xamarinmac"></a>Xamarin.Mac tablo görünümleri

_Bu makale, bir Xamarin.Mac uygulamasını tablo görünümlerde çalışmak kapsar. Bu, Xcode ve arabirim oluşturucu ve kodda normalde tabloda görünüm oluşturmayı açıklar._

Bir Xamarin.Mac uygulamasında çalışırken, C# ve .NET ile aynı erişiminiz tablo görünümleri, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Xcode'un kullanabileceğiniz _arabirim Oluşturucu_ oluşturmak ve korumak, tablo görünümleri (veya isteğe bağlı olarak bunları doğrudan C# kod içinde oluşturma için).

Tablo görünümü verileri birden çok satır bilgilerinin bir veya daha fazla sütun içeren bir tablo biçiminde görüntüler. Tablo oluşturulması görünüm türüne bağlı olarak, kullanıcı sütuna göre sırala, sütunları yeniden düzenlemek, sütun ekleme, sütunları kaldırın veya tabloda yer alan verileri düzenleyin.

[![](table-view-images/intro01.png "Bir örnek tablo")](table-view-images/intro01.png#lightbox)

Bu makalede, biz bir Xamarin.Mac uygulamasında tablo görünümleri ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Tablo görünümleri giriş

Tablo görünümü verileri birden çok satır bilgilerinin bir veya daha fazla sütun içeren bir tablo biçiminde görüntüler. Tablo görünümleri kaydırma görünümler içinde görüntülenir (`NSScrollView`) ve macOS 10.7 ile başlayarak, tüm kullanabilirsiniz `NSView` hücreler yerine (`NSCell`) hem satırları ve sütunları görüntülemek için. Bununla birlikte, yine de kullanabilir `NSCell` alt genellikle gerekir ancak `NSTableCellView` özel satırları ve sütunları oluşturabilirsiniz.

Tablo görünümü kendi verilerini depolamaz, bunun yerine, bir veri kaynağında kullanır (`NSTableViewDataSource`) satır ve sütunları, gerektiğinde bir temelinde gerekli sağlamak.

Tablo görünümü temsilci öğesinin sağlayarak bir tablo görünümün davranışı özelleştirilebilir (`NSTableViewDelegate`) işlevselliği, satır seçimi ve düzenleme, özel izleme ve özel görünümler için tek tek sütun seçmek için tablo sütunu yönetimini desteklemek için şunu yazın ve Satır.

Tablo görünümleri oluştururken Apple aşağıdaki önerir:

* Tabloda bir sütun başlıklarını tıklatarak sıralama izin verin.
* Sütun başlıkları, isimleri veya sütunda gösterilen verileri tanımlayan kısa bir isim tümcecikleri oluşturun.

Daha fazla bilgi için lütfen bkz [içerik görünümleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Oluşturulması ve bakımının yapılması xcode'da tablo görünümleri

Yeni bir Xamarin.Mac Cocoa uygulaması oluşturduğunuzda, varsayılan olarak standart boş bir pencere alın. Bu windows tanımlanmış bir `.storyboard` dosya otomatik olarak projeye dahil. Windows tasarımınızı düzenlemeye **Çözüm Gezgini**, çift tıklayarak `Main.storyboard` dosyası:

[![](table-view-images/edit01.png "Ana görsel taslak seçme")](table-view-images/edit01.png#lightbox)

Bu pencere tasarım Xcode'un arabirimi Oluşturucu'da açın:

[![](table-view-images/edit02.png "Xcode kullanıcı Arabiriminde düzenleme")](table-view-images/edit02.png#lightbox)

Tür `table` içine **kitaplığı Denetçisi'nin** Tablo görünümü denetimleri bulmayı kolaylaştırmak için arama kutusuna:

[![](table-view-images/edit03.png "Tablo görünümü kitaplıktan seçme")](table-view-images/edit03.png#lightbox)

Tablo görünümü Görünüm denetleyicisi sürükleyin **Arayüzü Düzenleyicisi**, görünüm denetleyicisi pencerenin içerik alanı doldurun ve burada küçüldükçe ve penceresinde ile büyüdükçe ayarlayın **kısıtlaması Düzenleyicisi**:

[![](table-view-images/edit04.png "Kısıtlama düzenleme")](table-view-images/edit04.png#lightbox)

Tablo Görünümü'nde seçin **arabirimi hiyerarşi** aşağıdaki özellikler kullanılabilir **özniteliği denetçisi**:

[![](table-view-images/edit05.png "Öznitelik denetçisi")](table-view-images/edit05.png#lightbox)

- **İçerik modu** -ya da görünümleri kullanmanıza olanak tanır (`NSView`) veya daha fazla hücreyi (`NSCell`) veri satırları ve sütunları görüntülemek için. MacOS 10.7 ile başlayarak, görünümleri kullanmanız gerekir.
- **Satırları gruplandırma gezinen** - `true`, kayan gibi Tablo görünümünde gruplandırılmış hücreleri çizer.
- **Sütunları** -görüntülenen sütunların sayısını tanımlar.
- **Üstbilgileri** - `true`, sütunları üst bilgileri içerir.
- **Yeniden sıralama** - `true`, kullanıcı sürükleyerek tablodaki sütunları yeniden Sırala.
- **Yeniden boyutlandırma** - `true`, kullanıcının sütun üst bilgilerini, sütunları yeniden boyutlandırmak için sürükleyin mümkün olacaktır.
- **Sütun boyutlandırma** -tablo boyutu sütunları otomatik nasıl kontrol eder.
- **Vurgulama** -tablo vurgulama türünü kullanan bir hücre seçildiğinde denetimleri.
- **Alternatif satırlar** - `true`, her zamankinden farklı bir arka plan rengi diğer satırında yer alacaktır.
- **Yatay Kılavuz** -yatay arasında hücreler çizilmiş kenarlık türünü seçer.
- **Dikey kılavuz** -dikey arasında hücreler çizilmiş kenarlık türünü seçer.
- **Kılavuz rengi** -hücre kenarlık rengini belirler.
- **Arka plan** -hücre arka plan rengini ayarlar.
- **Seçimi** -kullanıcı haliyle tablodaki hücre nasıl seçebilir denetlemenize izin veren:
    - **Birden çok** - `true`, kullanıcının birden çok satır ve sütunları seçebilirsiniz.
    - **Sütun** - `true`, kullanıcı sütunları seçebilirsiniz.
    - **Türünü seçin** - `true`, kullanıcı bir sırayı seçmek için bir karakter yazabilirsiniz.
    - **Boş** - `true`, kullanıcının gerekli değildir bir satır veya sütun seçmek için hiç herhangi bir seçim tablosu sağlar.
- **Otomatik kaydetme** -altında tablolar biçim adını otomatik olarak kaydedin.
- **Sütun bilgisi** - `true`, sıra ve sütunların genişliğini otomatik olarak kaydedilir.
- **Satır sonlarını** - hücre satır sonlarını nasıl seçin.
- **Görünen son satırın keser** - `true`, hücre uygulamasındaki onun sınırları içinde uygun olmayan verileri kesilecek.

> [!IMPORTANT]
> Eski bir Xamarin.Mac uygulamasını muhafaza sürece `NSView` temel tablo görünümleri, üzerinde kullanılmalıdır `NSCell` tablo görünümleri bağlı. `NSCell` eski olarak kabul edilir ve gelecekte desteklenmeyebilir.

Bir tablo sütununda seçin **arabirimi hiyerarşi** aşağıdaki özellikler kullanılabilir **özniteliği denetçisi**:

[![](table-view-images/edit06.png "Öznitelik denetçisi")](table-view-images/edit06.png#lightbox)

- **Başlık** -sütun başlığını ayarlar.
- **Hizalama** -hücrelerde metin hizalamasını ayarlayın.
- **Başlık yazı tipi** -hücre üst bilgi metni için yazı tipi seçer.
- **Sıralama anahtarı** -sütunundaki verileri sıralamak için kullanılan anahtar. Bu sütun kullanıcı sıralayamazsınız yoksa boş bırakın.
- **Seçici** -olan **eylem** sıralama gerçekleştirmek için kullanılır. Bu sütun kullanıcı sıralayamazsınız yoksa boş bırakın.
- **Sipariş** -Sütun verisi için sıralama düzeni.
- **Yeniden boyutlandırma** -sütun için boyutlandırma türünü belirler.
- **Düzenlenebilir** - `true`, kullanıcı bir hücre temel tablodaki hücre düzenleyebilirsiniz.
- **Gizli** - `true`, sütun gizlenir.

Sütunu onun (dikey sütunun sağ tarafta ortalanmış) tutamacı sola veya sağa sürükleyerek de yeniden boyutlandırabilirsiniz.

Şimdi, Tablo görünümünde her sütunu seçin ve ilk sütun vermek bir **başlık** , `Product` ve ikincisi `Details`.

Bir tablo hücre görünümü seçin (`NSTableViewCell`) içinde **arabirimi hiyerarşi** aşağıdaki özellikler kullanılabilir **özniteliği denetçisi**:

[![](table-view-images/edit07.png "Öznitelik denetçisi")](table-view-images/edit07.png#lightbox)

Tüm Standart Görünüm özellikler şunlardır. Burada bu sütun için satırları yeniden boyutlandırma seçeneğiniz de vardır.

Tablo görünümü hücresi seçin (varsayılan olarak, bir `NSTextField`) içinde **arabirimi hiyerarşi** aşağıdaki özellikler kullanılabilir **özniteliği denetçisi**:

[![](table-view-images/edit08.png "Öznitelik denetçisi")](table-view-images/edit08.png#lightbox)

Burada ayarlanan için standart bir metin alanı tüm özelliklerine sahip olacaksınız. Varsayılan olarak, standart bir metin alanı bir sütunda bir hücreyi bilgileri görüntülemek için kullanılır.

Bir tablo hücre görünümü seçin (`NSTableFieldCell`) içinde **arabirimi hiyerarşi** aşağıdaki özellikler kullanılabilir **özniteliği denetçisi**:

[![](table-view-images/edit09.png "Öznitelik denetçisi")](table-view-images/edit09.png#lightbox)

Burada en önemli ayarlar şunlardır:

- **Düzen** - bu sütundaki hücrelerin düzenlendiği nasıl seçin.
- **Tek satır modunu kullanan** - `true`, hücre tek bir satıra sınırlıdır.
- **İlk çalışma zamanı düzen genişliği** - `true`, hücre için (el ile veya otomatik olarak) olarak ayarlandığında genişliğini tercih eder uygulamasını çalıştırdığınızda ilk kez görüntülenir.
- **Eylem** -ne zaman denetimleri Düzen **eylem** hücre için gönderilir.
- **Davranış** -bir hücre seçilebilir düzenlenebilir olup olmadığını tanımlar.
- **Zengin metin** - `true`, hücre biçimlendirilmiş ve stil uygulanmış metni görüntüleyebilirsiniz.
- **Geri** - `true`, bunun için hücre sorumluluk üstlenen davranışı geri al.

Tablo hücre görünümü seçin (`NSTableFieldCell`) altındaki bir tablo sütununda **arabirimi hiyerarşi**:

[![](table-view-images/edit10.png "Tablo hücre görünümü seçme")](table-view-images/edit10.png#lightbox)

Bu sayede temel olarak kullanılan tablo hücre görünümü düzenlemek _deseni_ belirtilen sütun için oluşturulan tüm hücreler için.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Eylemler ve çıkışlar ekleme

Yalnızca bizim Tablo görünümü kullanıma sunmak ihtiyacımız diğer Cocoa UI denetimi gibi ve sütunları ve C# kodu kullanarak için hücre **eylemleri** ve **çıkışlar** (gerekli işlevselliğine bağlı olarak).

Biz kullanıma sunmak istiyorsanız herhangi bir tablo görünümü öğe işlem aynıdır:

1. Geçiş **Yardımcısı Düzenleyicisi** olduğundan emin olun `ViewController.h` dosyası seçili: 

    [![](table-view-images/edit11.png "Yardımcısı Düzenleyicisi")](table-view-images/edit11.png#lightbox)
2. Tablo görünümünden seçim **arabirimi hiyerarşi**denetim tıklayın ve sürükleyin `ViewController.h` dosya.
3. Oluşturma bir **çıkışı** adlı tablo görünümü için `ProductTable`: 

    [![](table-view-images/edit13.png "Bir çıkış yapılandırma")](table-view-images/edit13.png#lightbox)
4. Oluşturma **çıkışlar** de tablo sütunlarını adlı `ProductColumn` ve `DetailsColumn`: 

    [![](table-view-images/edit14.png "Bir çıkış yapılandırma")](table-view-images/edit14.png#lightbox)
5. Değişiklikleri kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Ardından, uygulamayı çalıştırdığınızda biz bazı veri tablosu için kod görünen yazacaksınız.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Tablo görünümü doldurma

Bizim Tablo görünümü ile tasarlanmış arabirim Oluşturucu'da ve aracılığıyla kullanıma sunulan bir **çıkışı**, ardından bunu doldurmak için C# kodu için oluşturmamız gerekir.

İlk olarak, yeni bir oluşturalım `Product` ayrı satırlara ait bilgileri için sınıf. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `Product` için **adı** tıklatıp **yeni** düğmesi:

[![](table-view-images/populate01.png "Boş bir sınıf oluşturma")](table-view-images/populate01.png#lightbox)

Olun `Product.cs` dosya görünüm aşağıdaki gibi:

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

Ardından, bir alt sınıfı oluşturmak ihtiyacımız `NSTableDataSource` istendiğinde tablomuza için veri sağlamak için. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `ProductTableDataSource` için **adı** tıklatıp **yeni** düğmesi.

Düzen `ProductTableDataSource.cs` dosyasını açıp aşağıdaki gibi görünmesi:

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

Bu sınıf, bizim tablo görünümün öğeleri için depolama alanına sahip ve geçersiz kılmalar `GetRowCount` tablodaki satır sayısını döndürmek için.

Son olarak, bir alt sınıfı oluşturmak ihtiyacımız `NSTableDelegate` tablomuza için davranışı sağlamak için. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `ProductTableDelegate` için **adı** tıklatıp **yeni** düğmesi.

Düzen `ProductTableDelegate.cs` dosyasını açıp aşağıdaki gibi görünmesi:

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

Biz örneğini oluşturduğunuzda `ProductTableDelegate`, biz de bir örneğini geçirin `ProductTableDataSource` tablosu için veri sağlar. `GetViewForItem` Yöntemdir verin sütun ve satır için hücre görüntülemek için bir görünüm (veriler) döndürmekten sorumludur. Mümkünse, varolan bir görünümü hücresi görüntülemek için yeniden kullanılabilir değilse yeni bir görünüm oluşturmanız gerekir.

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

Uygulama çalıştırıyoruz, aşağıdakiler gösterilir:

[![](table-view-images/populate02.png "Bir örnek uygulama çalıştırma")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sütuna göre sıralama

Şimdi bir sütun başlığına tıklayarak tablosundaki verileri sıralamak kullanıcının verin. İlk olarak, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Seçin `Product` sütun girin `Title` için **sıralama anahtarı**, `compare:` için **Seçici** seçip `Ascending` için **sipariş**:

[![](table-view-images/sort01.png "Sıralama anahtarı ayarlama")](table-view-images/sort01.png#lightbox)

Seçin `Details` sütun girin `Description` için **sıralama anahtarı**, `compare:` için **Seçici** seçip `Ascending` için **sipariş**:

[![](table-view-images/sort02.png "Sıralama anahtarı ayarlama")](table-view-images/sort02.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Artık düzenleyelim `ProductTableDataSource.cs` dosyasını açıp aşağıdaki yöntemleri ekleyin:

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

`Sort` Yöntemi bize temel veri kaynağındaki verileri sıralamak izin bir verilen `Product` artan veya azalan sınıf alanı. Geçersiz kılınan `SortDescriptorsChanged` kullanımı bir sütun başlığı her tıklayışında yöntemi çağrılır. Geçirilecek olan **anahtar** arabirim oluşturucu ve söz konusu sütun için sıralama düzenini ayarladığımız değeri.

Uygulamayı çalıştırmak ve sütun başlıklarını tıklayın, satır sütuna göre sıralanır:

[![](table-view-images/sort03.png "Bir örnek uygulama çalıştırma")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Satır seçimi

Tek bir satırı seçin, çift kullanıcıya izin vermek isteyip istemediğinizi `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Tablo Görünümü'nde seçin **arabirimi hiyerarşi** kaldırın **birden çok** onay kutusu **özniteliği denetçisi**:

[![](table-view-images/select01.png "Öznitelik denetçisi")](table-view-images/select01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.


Ardından, Düzenle `ProductTableDelegate.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Bu, tek bir satır Tablo görünümünde seçmesini olanak tanır. Dönüş `false` için `ShouldSelectRow` herhangi satır için seçilecek kullanıcı istemediğiniz veya `false` tüm satırları seçmek kullanıcı istemiyorsanız, her satır için.

Tablo görünümü (`NSTableView`) satır seçimi ile çalışmak için aşağıdaki yöntemleri içerir:

- `DeselectRow(nint)` -Tablosunda belirtilen satırı öğenin seçimi kaldırılır.
- `SelectRow(nint,bool)` -Belirli satırı seçer. Geçirmek `false` için ikinci parametresinin bir kerede yalnızca bir satır seçin.
- `SelectedRow` -Tabloda seçilen satırın geçerli döndürür.
- `IsRowSelected(nint)` -Döndürür `true` sağlanan satırda seçili değilse.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Birden çok satır seçimi

Birden çok satır seçin ve çift kullanıcıya izin vermek isteyip istemediğinizi `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Tablo görünümünde seçin **arabirimi hiyerarşi** ve **birden çok** onay kutusu **özniteliği denetçisi**:

[![](table-view-images/select02.png "Öznitelik denetçisi")](table-view-images/select02.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.


Ardından, Düzenle `ProductTableDelegate.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Bu, tek bir satır Tablo görünümünde seçmesini olanak tanır. Dönüş `false` için `ShouldSelectRow` herhangi satır için seçilecek kullanıcı istemediğiniz veya `false` tüm satırları seçmek kullanıcı istemiyorsanız, her satır için.

Tablo görünümü (`NSTableView`) satır seçimi ile çalışmak için aşağıdaki yöntemleri içerir:

- `DeselectAll(NSObject)` -Tablodaki tüm satırları öğenin seçimi kaldırılır. Kullanım `this` seçerek yapılması nesnesinde göndermek ilk parametre için. 
- `DeselectRow(nint)` -Tablosunda belirtilen satırı öğenin seçimi kaldırılır.
- `SelectAll(NSobject)` -Tablodaki tüm satırları seçer. Kullanım `this` seçerek yapılması nesnesinde göndermek ilk parametre için.
- `SelectRow(nint,bool)` -Belirli satırı seçer. Geçirmek `false` ikinci parametresi için seçimi kaldırın ve yalnızca tek bir satırı seçin, aktarmak `true` Seçimi Genişlet ve bu satırı içerecek.
- `SelectRows(NSIndexSet,bool)` -Belirtilen satır kümesini seçer. Geçirmek `false` ikinci parametresi için seçimi kaldırın ve yalnızca bir bunları satır geçirmek `true` Seçimi Genişlet ve bu satırlar eklemek için.
- `SelectedRow` -Tabloda seçilen satırın geçerli döndürür.
- `SelectedRows` -Döndürür bir `NSIndexSet` içeren dizinler seçilen satır.
- `SelectedRowCount` -Seçilen satır sayısını döndürür.
- `IsRowSelected(nint)` -Döndürür `true` sağlanan satırda seçili değilse.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Sırayı seçmek için türü

Seçilen Tablo görünümünde bir karakter yazın izin verin ve ilk satırı seçmek istiyorsanız, o karakteri olan, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Tablo görünümünde seçin **arabirimi hiyerarşi** ve **türünü seçin** onay kutusu **özniteliği denetçisi**:

[![](table-view-images/type01.png "Seçim türünü ayarlama")](table-view-images/type01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Artık düzenleyelim `ProductTableDelegate.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

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

`GetNextTypeSelectMatch` Yöntemi belirtilen `searchString` ve ilk satırını döndürür `Product` , o dizeyi ait olduğu `Title`.

Uygulamayı çalıştırmak ve bir karakter yazın, bir satır seçildi:

[![](table-view-images/type02.png "Bir örnek uygulama çalıştırma")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Sütunları yeniden sıralama

Sürükleyin kullanıcıya izin vermek istiyorsanız, Tablo görünümünde sütunları yeniden Sırala, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Tablo görünümünde seçin **arabirimi hiyerarşi** ve **yeniden sıralama** onay kutusu **özniteliği denetçisi**:

[![](table-view-images/reorder01.png "Öznitelik denetçisi")](table-view-images/reorder01.png#lightbox)

İçin bir değer sunuyoruz, **otomatik kaydetme** özelliği ve onay **sütun bilgileri** biz Tablo düzeni yaptığınız tüm değişiklikler bizim için otomatik olarak kaydedilecek ve uygulamayı açtığında geri alan çalıştırılır.

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Artık düzenleyelim `ProductTableDelegate.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Yöntemi döndürmelidir `true` içine sürükleyin istediğiniz olmasını sağlamak için herhangi bir sütun için yeniden `newColumnIndex`, aksi takdirde dönüş `false`;

Uygulama çalıştırıyoruz, bizim sütunları yeniden sıralamak için sütun üst bilgilerini etrafında sürükleyebilirsiniz:

[![](table-view-images/reorder02.png "Yeniden sıralanan sütunlar örneği")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Hücre düzenleme

Belirtilen hücre değerlerini düzenlemek Düzenle kullanıcı izin vermek istiyorsanız `ProductTableDelegate.cs` dosya ve değiştirme `GetViewForItem` yöntemini aşağıdaki şekilde:

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

Artık kullanıcı uygulama çalıştırıyoruz, Tablo görünümünde hücreleri düzenleyebilirsiniz:

[![](table-view-images/editing01.png "Bir hücre düzenleme örneği")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Tablo görünümleri görüntülerini kullanma

Hücresinde bir parçası olarak bir resim eklemek için bir `NSTableView`, tablo görünümün tarafından döndürülen veriler nasıl değiştirileceği ihtiyacınız olacak `NSTableViewDelegate's` `GetViewForItem` yönteminin kullanılacağını bir `NSTableCellView` tipik yerine `NSTextField`. Örneğin:

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

Daha fazla bilgi için lütfen bkz [kullanarak görüntüleri tablo görünümleri ile](~/mac/app-fundamentals/image.md) bölümünü bizim [görüntüyle](~/mac/app-fundamentals/image.md) belgeleri.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Satır Sil düğmesini ekleme

Uygulama gereksinimlerinize bağlı olarak, olabilir durumlarda burada tablodaki her satır için bir eylem düğmesi sağlamanız gerekir. Bu örnek olarak, şimdi eklemek için yukarıda oluşturulan Tablo görünümü örnek genişletme bir **Sil** her satırında düğmesi.

İlk olarak, Düzen `Main.storyboard` Xcode'un arabirim Oluşturucu Tablo görünümü seçin ve sütun sayısını artırmak üç (3). Ardından, değiştirme **başlık** için yeni sütunun `Action`:

[![](table-view-images/delete01.png "Sütun adı düzenleme")](table-view-images/delete01.png#lightbox)

Film şeridini değişiklikleri kaydetmek ve değişiklikleri eşitlemek Mac için Visual Studio geri dönün.

Ardından, Düzenle `ViewController.cs` dosyası ve ortak aşağıdaki yöntemi ekleyin:

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

Aynı dosyada içinde yeni tablo görünümü temsilci oluşturulmasını değiştirme `ViewDidLoad` yöntemini aşağıdaki şekilde:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Şimdi Düzenle `ProductTableDelegate.cs` dosya görünüm denetleyicisi özel bir bağlantı içerir ve parametre olarak, yeni bir temsilci örneğini oluştururken denetleyici almak için:

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

Ardından, aşağıdaki yeni özel yöntem sınıfa ekleyin:

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

Bu tüm daha önce gerçekleştirilen metin görünümünü yapılandırmalarını sürer `GetViewForItem` yöntemi ve (tablosunun son sütununda bir metin görünümünü ancak bir düğme içermediğinden) çağrılabilir, tek bir konuma yerleştirir.

Son olarak, Düzen `GetViewForItem` yöntemi ve aşağıdaki gibi görünmesi:

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

Bu kodu daha ayrıntılı birkaç bölümlerini bakalım. İlk olarak, yeni `NSTableViewCell` olan eylem oluşturulan alınır tabanlı sütun adına. İlk iki sütun (**ürün** ve **ayrıntıları**), yeni `ConfigureTextField` yöntemi çağrılır.

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

Düğmenin `Tag` özelliği şu anda işlenmekte olan satır sayısını tutmak için kullanılır. Bu sayı daha sonra kullanıcı düğmenin içinde silinecek bir satır istediğinde kullanılacak `Activated` olay:

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

Olay işleyicisi başlangıcında, düğme ve verilen tablo satırı olan ürünü alın. Ardından bir uyarı satır silme işlemini onaylayan kullanıcıyı sunulur. Kullanıcının satırı silmek seçerse, sağlanan satırda veri kaynağından kaldırılır ve tablo yüklenir:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Son olarak, yeni oluşturulan yerine tablo görünümü hücresi yeniden kullanılıyor, aşağıdaki kod, işlenmekte olan sütuna göre yapılandırır:

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

İçin **eylem** tüm alt Görünüm sütunu, taranan kadar `NSButton` bulunur, bu ise `Tag` özelliği, geçerli satır sonunda işaret edecek şekilde güncelleştirilir.

Bu değişikliklerle yerinde uygulama çalıştırıldığında her satır olacaktır bir **Sil** düğmesi:

[![](table-view-images/delete02.png "Düğmeleri silme ile Tablo görünümü")](table-view-images/delete02.png#lightbox)

Kullanıcı tıkladığında bir **Sil** düğmesi belirtilen satırı silmek için bunları isteyen bir uyarı görüntülenir:

[![](table-view-images/delete03.png "Bir silme satır Uyarısı")](table-view-images/delete03.png#lightbox)

Kullanıcı silme seçerse, satır kaldırılacak ve tabloyu yeniden:

[![](table-view-images/delete04.png "Tablonun satır silindikten sonra")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Veri bağlama tablo görünümleri

Xamarin.Mac uygulamanızda veri bağlama ve anahtar-değer kodlaması teknikleri kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız gereken kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma avantajı vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), bakımı kolay, daha esnek bir uygulama için önde gelen Tasarım.

Anahtar-değer kodlaması (KVC) olan bir mekanizma örnek değişkenleri erişmesini yerine özellikleri tanımlamak için anahtarları (özel olarak biçimlendirilmiş dizeler) veya erişimci yöntemlerini kullanarak, dolaylı olarak bir nesnenin özelliklerine erişme (`get/set`). Xamarin.Mac uygulamanızda anahtar-değer kodlaması uyumlu erişimcilerini uygulayarak, anahtar-değer gözleme (KVO), veri bağlama, temel veri, Cocoa bağlamaları ve scriptability gibi diğer macOS özelliklere erişiminiz olur.

Daha fazla bilgi için lütfen bkz [tablo, görünüm veri bağlama](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) bölümünü bizim [veri bağlama ve anahtar-değer kodlaması](~/mac/app-fundamentals/databinding.md) belgeleri.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede ayrıntılı bir Xamarin.Mac uygulamasında tablo görünümlerle çalışma göz duruma getirdi. Biz farklı türleri gördünüz ve tablo görünümleri, oluşturma ve tablo görünümlerde Xcode'un arabirim Oluşturucu korumak ve C# kodunda tablo görünümleri ile çalışma konusunda kullanır.

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
