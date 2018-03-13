---
title: "Anahat görünümleri"
description: "Bu makalede Xamarin.Mac uygulamasında anahat görünümlerle çalışma kapsar. Oluşturma ve Xcode ve arabirim Oluşturucu anahat görünümleri Bakımı ve bunlarla program aracılığıyla çalışma açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: dbbd10af046c0a8421e06e675364f92405b2317f
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="outline-views"></a>Anahat görünümleri

_Bu makalede Xamarin.Mac uygulamasında anahat görünümlerle çalışma kapsar. Oluşturma ve Xcode ve arabirim Oluşturucu anahat görünümleri Bakımı ve bunlarla program aracılığıyla çalışma açıklar._

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı erişiminiz anahat görünümleri, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve anahat görünümlerinizi korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

Anahat görünümü verir tablo türü Genişlet veya hiyerarşik veri satırı Daralt ' dir. Bir tablo görünümü gibi bir anahat görünümü ayrı öğeler ve bu öğeleri özniteliklerini temsil eden sütun temsil eden satırlar ile ilgili öğeler, bir dizi verilerini görüntüler. Bir tablo görünümü aksine bir anahat görünümünde öğeler, düz bir liste değil, dosyaları ve klasörleri sabit sürücü gibi bir hiyerarşide düzenlenir.

[![](outline-view-images/populate03.png "Bir örnek uygulamayı çalıştırma")](outline-view-images/populate03.png#lightbox)

Bu makalede, sizi bir Xamarin.Mac uygulamasında anahat görünümleri ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Anahat görünümlerinde giriş

Anahat görünümü verir tablo türü Genişlet veya hiyerarşik veri satırı Daralt ' dir. Bir tablo görünümü gibi bir anahat görünümü ayrı öğeler ve bu öğeleri özniteliklerini temsil eden sütun temsil eden satırlar ile ilgili öğeler, bir dizi verilerini görüntüler. Bir tablo görünümü aksine bir anahat görünümünde öğeler, düz bir liste değil, dosyaları ve klasörleri sabit sürücü gibi bir hiyerarşide düzenlenir.

Anahat görünümde bir öğe başka öğeler varsa, genişletilmiş veya kullanıcı tarafından daraltılmış. Genişletilebilir bir öğe öğe daraltılmış ve öğesi genişletilmediğinde aşağı işaret eden bağlandığınızda sağı gösteren bir açığa üçgen görüntüler. Üzerinde açığa üçgen tıklatarak genişletmek veya daraltmak madde neden olur.

Anahat görünümü (`NSOutlineView`) bir tablo görünümü sınıfıdır (`NSTableView`) ve bu nedenle, davranışını çoğunu kendi üst sınıfından devralıyor. Sonuç olarak, bir tablo satır veya sütun seçme gibi sütun üst bilgileri, vb. sürükleyerek sütunları yeniden konumlandırma görünümü tarafından desteklenen birçok işlemler de anahat görünümü tarafından desteklenir. Xamarin.Mac uygulama bu özelliklerin denetleyebilir ve anahat görünümün parametreleri (ya da kod veya arabirim Oluşturucu) izin vermek veya belirli işlemlerin engellemek için yapılandırabilirsiniz.

Anahat görünümü kendi veri depolamaz, bunun yerine bir veri kaynağında kullanır (`NSOutlineViewDataSource`) satır ve sütunları, gerektiği ölçüde temeline üzerinde gerekli sağlamak için.

Anahat görünümü temsilci öğesinin bir alt sağlayarak bir anahat görünümün davranışı özelleştirilebilir (`NSOutlineViewDelegate`) işlevselliği, satır seçimi ve düzenleme, özel izleme ve kişi için özel görünümler seçmek için anahat sütun yönetimini desteklemek için yazın satırları ve sütunları.

Anahat görünümü olan bir tablo görünümü, davranış ve işlevsellik çoğunu paylaşır olduğundan, üzerinden geçmek isteyebilirsiniz bizim [tablosu görünümleri](~/mac/user-interface/table-view.md) Bu makale ile devam etmeden önce belgeleri.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Oluşturma ve Xcode anahat görünümlerinde koruma

Yeni bir Xamarin.Mac Cocoa uygulaması oluşturduğunuzda, varsayılan olarak standart boş, bir pencere alın. Bu windows tanımlanmış bir `.storyboard` otomatik olarak projeye dahil dosyası. Windows tasarımınızı düzenlemek için **Çözüm Gezgini**, çift tıklayarak `Main.storyboard` dosyası:

[![](outline-view-images/edit01.png "Ana film şeridi seçme")](outline-view-images/edit01.png#lightbox)

Bu pencere tasarım Xcode'nın arabirimi Oluşturucusu'nda açın:

[![](outline-view-images/edit02.png "Xcode kullanıcı Arabiriminde düzenleme")](outline-view-images/edit02.png#lightbox)

Tür `outline` içine **kitaplığı Denetçisi'nin** anahat görünümü denetimleri bulmayı kolaylaştırmak için arama kutusunu:

[![](outline-view-images/edit03.png "Kitaplıktan bir anahat görünümü seçme")](outline-view-images/edit03.png#lightbox)

Anahat görünümü Görünüm denetleyiciye sürükleyin **arabirimi Düzenleyicisi**, görünüm denetleyicisini içerik alanını doldurun ve burada küçültür ve penceresinde ile büyür ayarlanan **kısıtlaması Düzenleyicisi**:

[![](outline-view-images/edit04.png "Kısıtlamalar düzenleme")](outline-view-images/edit04.png#lightbox)

Anahat görünümünde seçme **arabirimi hiyerarşi** ve aşağıdaki özellikler mevcuttur **özniteliği denetçisi**:

[![](outline-view-images/edit05.png "Öznitelik denetçisi")](outline-view-images/edit05.png#lightbox)

- **Anahat sütun** -sütununu hiyerarşik veri görüntülenir.
- **Otomatik kaydetme anahat sütununun** - `true`, anahat sütununun otomatik olarak kaydedilir ve uygulama çalışmaları arasında geri.
- **Girinti** -sütunları bir genişletilmiş öğesinin altında girinti tutar.
- **Girinti izleyen hücreleri** - `true`, girinti işareti birlikte hücreleri girintili.
- **Otomatik kaydetme genişletilmiş öğeleri** - `true`, öğeleri genişletilmiş/daraltılmış durumunu otomatik olarak kaydedilir ve uygulama çalışmaları arasında geri.
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
> Eski bir Xamarin.Mac Uygulama Bakımı sürece `NSView` tabanlı anahat görünümler üzerinde kullanılmalıdır `NSCell` tablosu görünümleri tabanlı. `NSCell` eski olarak kabul edilir ve ileride desteklenmiyor olabilir.

Bir tablo sütununda seçin **arabirimi hiyerarşi** ve aşağıdaki özellikler mevcuttur **özniteliği denetçisi**:

[![](outline-view-images/edit06.png "Öznitelik denetçisi")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "Öznitelik denetçisi")](outline-view-images/edit07.png#lightbox)

Bu standart bir görünüm özelliklerini tümü. Burada bu sütun için satırları yeniden boyutlandırma seçeneğiniz de vardır.

Bir tablo görünümü hücre seçin (varsayılan olarak, bir `NSTextField`) içinde **arabirimi hiyerarşi** ve aşağıdaki özellikler mevcuttur **özniteliği denetçisi**:

[![](outline-view-images/edit08.png "Öznitelik denetçisi")](outline-view-images/edit08.png#lightbox)

Burada ayarlamak için standart bir metin alanı tüm özelliklerine sahip olacaksınız. Varsayılan olarak, standart bir metin alanı, bir sütundaki bir hücrenin verilerini görüntülemek için kullanılır.

Tablo Hücre görünümünü seçin (`NSTableFieldCell`) içinde **arabirimi hiyerarşi** ve aşağıdaki özellikler mevcuttur **özniteliği denetçisi**:

[![](outline-view-images/edit09.png "Öznitelik denetçisi")](outline-view-images/edit09.png#lightbox)

Burada en önemli ayarlar şunlardır:

- **Düzen** - bu sütundaki hücrelerin düzenlendiği yöntemini seçin.
- **Tek satırlı moda kullanan** - `true`, tek bir satır hücre sınırlıdır.
- **İlk çalışma zamanı düzen genişliği** - `true`, hücre için (el ile veya otomatik olarak) olarak ayarlandığında genişliği tercih eder uygulama çalıştırıldığında ilk kez görüntülenir.
- **Eylem** -ne zaman denetimleri düzenleme **eylem** hücre için gönderilir.
- **Davranış** -bir hücre seçilebilir veya düzenlenebilir ise tanımlar.
- **Zengin metin** - `true`, hücre biçimlendirilmiş ve stilde metin görüntüleyebilirsiniz.
- **Geri** - `true`, bunun için hücre sorumluluğu üstlenir davranışı geri al.

Tablo hücre görünümü seçin (`NSTableFieldCell`) bir tablo sütununda sonundaki **arabirimi hiyerarşi**:

[![](outline-view-images/edit11.png "Tablo Hücre görünümünü seçme")](outline-view-images/edit10.png#lightbox)

Bu sayede temel olarak kullanılan tablo hücre görünümünü düzenlemek _düzeni_ belirtilen sütun için oluşturulan tüm hücreler için.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Eylemler ve çıkışlar ekleme

Yalnızca bizim anahat görünümü kullanıma sunmak ihtiyacımız diğer Cocoa UI denetimi gibi ve sütunları ve hücreleri C# kod kullanarak **Eylemler** ve **çıkışlar** (gerekli işlevselliğine bağlı).

Biz kullanıma sunmak istediğiniz herhangi bir anahat görünümü öğesi işlemi aynıdır:

1. Geçiş **Yardımcısı Düzenleyicisi** ve emin `ViewController.h` dosya seçili: 

    [![](outline-view-images/edit11.png "Doğru .h dosyası seçme")](outline-view-images/edit11.png#lightbox)
2. Anahat görünümünden seçin **arabirimi hiyerarşi**denetim tıklatın ve sürükleyin `ViewController.h` dosya.
3. Oluşturma bir **çıkışı** anahat görünümü adlı için `ProductOutline`: 

    [![](outline-view-images/edit13.png "Prizine yapılandırma")](outline-view-images/edit13.png#lightbox)
4. Oluşturma **çıkışlar** de tablo sütunlarını adlı `ProductColumn` ve `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Prizine yapılandırma")](outline-view-images/edit14.png#lightbox)
5. Değişiklikleri kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Ardından, uygulamayı çalıştırdığınızda şu kodu görüntüleme anahattı için bazı veriler yazacaksınız.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Anahat görünümü doldurma

Bizim anahat görünümüyle tasarlanmış arabirim Oluşturucusu'nda ve aracılığıyla kullanıma sunulan bir **çıkışı**, ardından biz bunu doldurmak için C# kodu oluşturmanız gerekir.

İlk olarak, yeni bir oluşturalım `Product` tek tek satırların ve alt ürünleri grupları için bilgiyi tutmak için sınıf. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `Product` için **adı** tıklatıp **yeni** düğmesi:

[![](outline-view-images/populate01.png "Boş bir sınıf oluşturma")](outline-view-images/populate01.png#lightbox)

Olun `Product.cs` aşağıdaki gibi dosya bakın:

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
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

Ardından, bir alt sınıfı oluşturmak ihtiyacımız `NSOutlineDataSource` , istendiği gibi verileri için bizim anahattı sağlamak için. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `ProductOutlineDataSource` için **adı** tıklatıp **yeni** düğmesi.

Düzen `ProductTableDataSource.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }
                
        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }
        
        }
        #endregion
    }
}
```

Bu sınıf bizim anahat görünümün öğeleri için depolama alanına sahip ve geçersiz kılmalar `GetChildrenCount` tablodaki satır sayısını dönün. `GetChild` (Anahat görünüm tarafından istendiği gibi) belirli bir üst veya alt öğeye döndürür ve `ItemExpandable` belirtilen öğe bir üst veya alt öğesi olarak tanımlar.

Son olarak, bir alt sınıfı oluşturmak ihtiyacımız `NSOutlineDelegate` için bizim anahattı davranışı sağlamak için. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `ProductOutlineDelegate` için **adı** tıklatıp **yeni** düğmesi.

Düzen `ProductOutlineDelegate.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Bir örneğini oluştururken biz `ProductOutlineDelegate`, biz de bir örnekte geçirmek `ProductOutlineDataSource` anahat için veriler sağlar. `GetView` Yöntemdir hücre verin sütun ve satır için görüntülenecek bir görünüm (veri) döndürmek için sorumlu. Mümkünse, var olan bir görünümü hücre görüntülemek için yeniden kullanılabilir değilse yeni bir görünüm oluşturulması gerekir.

Anahat doldurmak için düzenleyelim `MainWindow.cs` dosya ve olun `AwakeFromNib` yöntemi görünüm aşağıdaki gibi:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

Biz uygulama çalıştırıyorsanız, aşağıdaki görüntülenir:

[![](outline-view-images/populate02.png "Daraltılmış görünümü")](outline-view-images/populate02.png#lightbox)

Biz anahat görünümünde bir düğüm genişletirseniz, aşağıdaki gibi görünür:

[![](outline-view-images/populate03.png "Genişletilmiş Görünüm")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sütuna göre sıralama

Şimdi anahattı verileri bir sütun başlığına tıklayarak sıralayın izin verin. İlk olarak, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Seçin `Product` sütun girin `Title` için **sıralama anahtarı**, `compare:` için **Seçici** seçip `Ascending` için **sipariş**:

[![](outline-view-images/sort01.png "Sıralama anahtarı ayarlama")](outline-view-images/sort01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Şimdi düzenleyelim `ProductOutlineDataSource.cs` dosya ve aşağıdaki yöntemleri ekleyin:

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
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

`Sort` Yöntemi izin bize temel veri kaynağındaki verileri sıralamak bir verilen `Product` artan veya azalan sınıfı alanı. Geçersiz kılınan `SortDescriptorsChanged` bir sütun başlığı kullanımı tıklar her zaman yöntemi çağrılır. Kendisine geçirilen **anahtar** arabirimi oluşturucu ve bu sütun için sıralama düzenini ayarlarız değeri.

Uygulamayı çalıştırın ve sütun başlıklarının'ı tıklatın, satır sütuna göre sıralanır:

[![](outline-view-images/sort02.png "Sıralanmış bir çıkış")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Satır seçimi

Tek bir satır seçin, çift kullanıcıya izin vermek isteyip istemediğinizi `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Anahat görünümünde seçme **arabirimi hiyerarşi** ve onay kutusunu temizleyin **birden çok** onay kutusu **özniteliği denetçisi**:

[![](outline-view-images/select01.png "Öznitelik denetçisi")](outline-view-images/select01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.


Ardından, düzenleme `ProductOutlineDelegate.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Bu, tek bir satır anahat görünümünde seçme kullanıcıya izin verir. Dönüş `false` için `ShouldSelectItem` herhangi öğesi için seçmek kullanıcı istemediğiniz veya `false` herhangi bir öğeyi seçmek kullanıcı istemiyorsanız, her öğe için.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Birden çok satır seçimi

Birden çok satır seçmek, çift kullanıcıya izin vermek isteyip istemediğinizi `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Anahat görünümünde seçme **arabirimi hiyerarşi** ve denetleyin **birden çok** onay kutusu **özniteliği denetçisi**:

[![](outline-view-images/select02.png "Öznitelik denetçisi")](outline-view-images/select02.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.


Ardından, düzenleme `ProductOutlineDelegate.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Bu, tek bir satır anahat görünümünde seçme kullanıcıya izin verir. Dönüş `false` için `ShouldSelectRow` herhangi öğesi için seçmek kullanıcı istemediğiniz veya `false` herhangi bir öğeyi seçmek kullanıcı istemiyorsanız, her öğe için.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Sırayı seçmek için türü

İlk satırı seçin ve bir karakter ile anahat seçili görünümü yazın kullanıcıya izin vermek istiyorsanız, bu karakter olan, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Anahat görünümünde seçme **arabirimi hiyerarşi** ve denetleyin **türünü seçin** onay kutusu **özniteliği denetçisi**:

[![](outline-view-images/type01.png "Satır türü düzenleme")](outline-view-images/type01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Şimdi düzenleyelim `ProductOutlineDelegate.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

`GetNextTypeSelectMatch` Yöntemi alır verilen `searchString` ve ilk öğeyi döndürür `Product` , o dizeyi 's içerdiği `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Sütunları yeniden sıralama

Sürükleme kullanıcıya izin vermek istiyorsanız anahat görünümünde sütunları yeniden sıralama, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın. Anahat görünümünde seçme **arabirimi hiyerarşi** ve denetleme **Reordering** onay kutusu **özniteliği denetçisi**:

[![](outline-view-images/reorder01.png "Öznitelik denetçisi")](outline-view-images/reorder01.png#lightbox)

Biz için bir değer verirseniz **otomatik kaydetme** özelliği ve onay **sütun bilgileri** alan tablonun düzene vermiyoruz değişiklikleri bize otomatik olarak kaydedilir ve uygulama başlatıldığında geri çalıştırılır.

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Şimdi düzenleyelim `ProductOutlineDelegate.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Yöntemi döndürmelidir `true` istediğiniz olmasını sağlamak için herhangi bir sütun için içine sürükleyin kaldırılmasında `newColumnIndex`, aksi takdirde dönüş `false`;

Biz uygulama çalıştırırsanız, biz bizim sütunları yeniden sıralamak için sütun başlıkları geçici sürükleyebilirsiniz:

[![](outline-view-images/reorder02.png "Sipariş sütunları örneği")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Hücre düzenleme

Belli bir hücre değerlerini Düzenle, Düzenle kullanıcıya izin vermek istiyorsanız `ProductOutlineDelegate.cs` dosya ve değişiklik `GetViewForItem` yöntemini aşağıdaki şekilde:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

Şimdi biz uygulama çalıştırırsanız, kullanıcı Tablo görünümünde hücreleri düzenleyebilirsiniz:

[![](outline-view-images/editing01.png "Hücre düzenleme örneği")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Anahat görünümlerinde görüntüleri kullanma

Hücrenin bir parçası olarak bir resim eklemek için bir `NSOutlineView`, anahat görünümün tarafından döndürülen veriler nasıl değiştirmeniz gerekir `NSTableViewDelegate's` `GetView` yöntemini kullanmak üzere bir `NSTableCellView` tipik yerine `NSTextField`. Örneğin:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
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
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Daha fazla bilgi için lütfen bkz [kullanarak görüntüleri anahat görünümlerle](~/mac/app-fundamentals/image.md) bölümünü bizim [görüntüsüyle çalışma](~/mac/app-fundamentals/image.md) belgeleri.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Veri bağlama anahat görünümleri

Xamarin.Mac uygulamanızda anahtar-değer kodlama ve veri bağlama teknikleri kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız için sahip kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma faydası vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), başında bakımı kolay, daha esnek uygulama için Tasarım.

Anahtar-değer kodlama (KVC) olan bir nesnenin özelliklerine dolaylı olarak erişme, örnek değişkenleri erişim yerine özelliklerini tanımlamak için (özel olarak biçimlendirilmiş dizeler) anahtarları veya erişimci yöntemlerini kullanarak için bir mekanizma (`get/set`). Anahtar-değer kodlama uyumlu erişimcilerini Xamarin.Mac uygulamanızda uygulayarak, anahtar-değer Gözlemleme (KVO), veri bağlama, çekirdek verileri, Cocoa bağlamalar ve scriptability gibi diğer macOS özelliklerine erişim sahibi olursunuz.

Daha fazla bilgi için lütfen bkz [anahat görünüm verileri bağlama](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) bölümünü bizim [verileri bağlama ve anahtar-değer kodlama](~/mac/app-fundamentals/databinding.md) belgeleri.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede bir Xamarin.Mac uygulamasında anahat görünümlerle çalışma ayrıntılı bir bakış sürdü. Biz farklı türleri gördünüz ve anahat görünümleri, oluşturma ve Xcode'nın arabirimi Oluşturucu anahat Görünümleri Koru ve C# kodunda anahat görünümlerle nasıl kullanır.

## <a name="related-links"></a>İlgili bağlantılar

- [MacOutlines (örnek)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (örnek)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Tablo Görünümleri](~/mac/user-interface/table-view.md)
- [Kaynak Listeleri](~/mac/user-interface/source-list.md)
- [Veri Bağlama ve Anahtar-Değer Kodlaması](~/mac/app-fundamentals/databinding.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Anahat görünümlerinde giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
