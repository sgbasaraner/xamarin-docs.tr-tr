---
title: Xamarin.Mac anahat görünümleri
description: Bu makale, bir Xamarin.Mac uygulamasını anahat görünümleri ile çalışmayı kapsar. Bu, oluşturmak, Xcode ve arabirim Oluşturucu anahat görünümleri bakımını yapma ve program aracılığıyla çalışma açıklar.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7228a22eeae3dfdb354ba3693c277251fd2cd821
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780675"
---
# <a name="outline-views-in-xamarinmac"></a>Xamarin.Mac anahat görünümleri

_Bu makale, bir Xamarin.Mac uygulamasını anahat görünümleri ile çalışmayı kapsar. Bu, oluşturmak, Xcode ve arabirim Oluşturucu anahat görünümleri bakımını yapma ve program aracılığıyla çalışma açıklar._

Bir Xamarin.Mac uygulamasında çalışırken, C# ve .NET ile aynı erişiminiz anahat görünümleri, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Xcode'un kullanabileceğiniz _arabirim Oluşturucu_ oluşturmak ve korumak, anahat görünümleri (veya isteğe bağlı olarak bunları doğrudan C# kodu oluşturmak için).

Kullanıcıya izin veren bir tablo türü genişletin veya hiyerarşik veri satırlarını Daralt bir anahat bir görünümdür. Bir tablo görünümü gibi bir anahat görünümü tek tek öğeler ve öğelerin özniteliklerini temsil eden sütunları temsil eden satırlar ile ilgili öğeler, bir dizi verilerini görüntüler. Bir tablo görünümü aksine bir ana görünümünde öğeler, düz bir listede değil, bir hiyerarşideki bir sabit sürücü üzerindeki dosya ve klasörleri gibi düzenlenir.

[![](outline-view-images/populate03.png "Bir örnek uygulama çalıştırma")](outline-view-images/populate03.png#lightbox)

Bu makalede, biz bir Xamarin.Mac uygulamasında anahat görünümleri ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Anahat görünümleri giriş

Kullanıcıya izin veren bir tablo türü genişletin veya hiyerarşik veri satırlarını Daralt bir anahat bir görünümdür. Bir tablo görünümü gibi bir anahat görünümü tek tek öğeler ve öğelerin özniteliklerini temsil eden sütunları temsil eden satırlar ile ilgili öğeler, bir dizi verilerini görüntüler. Bir tablo görünümü aksine bir ana görünümünde öğeler, düz bir listede değil, bir hiyerarşideki bir sabit sürücü üzerindeki dosya ve klasörleri gibi düzenlenir.

Anahat görünümde bir öğeyi diğer bir öğe içeriyorsa, genişletilmiş veya daraltılmış kullanıcı tarafından. Genişletilebilir bir öğe, öğe daraltılmış ve aşağı öğesi genişletildiğinde işaret sağı gösteren bir açıklama üçgenini görüntüler. Açığa üçgeni tıklayarak öğeyi genişletmek veya daraltmak neden olur.

Anahat görünümü (`NSOutlineView`) tablo görünümü sınıfıdır (`NSTableView`) ve bu nedenle, çok davranışını kendi üst sınıfından devralır. Sonuç olarak, bir tablo satır veya sütun seçme gibi sütun başlıkları, vs. sürükleyerek sütunları yeniden konumlandırma görünümü tarafından desteklenen birçok işlem bir anahat görünüm tarafından da desteklenir. Bir Xamarin.Mac uygulamasını bu özellikleri denetime sahiptir ve izin verebilir veya yasaklayabilir belirli işlemleri ana hat görünümün parametreleri (ya da kod veya arabirim Oluşturucu) yapılandırabilirsiniz.

Anahat görünümü kendi verilerini depolamaz, bunun yerine, bir veri kaynağında kullanır (`NSOutlineViewDataSource`) satır ve sütunları, gerektiğinde bir temelinde gerekli sağlamak.

Anahat görünümü temsilci öğesinin sağlayarak bir anahat görünümün davranışı özelleştirilebilir (`NSOutlineViewDelegate`) işlevselliği, satır seçimi ve düzenleme, özel izleme ve özel görünümler için tek tek seçin, anahat sütun yönetimini desteklemek için yazın satırları ve sütunları.

Bu yana bir anahat görünümü olan bir tablo görünümü onun davranışı ve işlevselliği çoğunu paylaşır, üzerinden geçmek üzere isteyebilirsiniz bizim [tablo görünümleri](~/mac/user-interface/table-view.md) bu makalede ile devam etmeden önce belgeleri.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Oluşturulması ve bakımının yapılması xcode'da anahat görünümleri

Yeni bir Xamarin.Mac Cocoa uygulaması oluşturduğunuzda, varsayılan olarak standart boş bir pencere alın. Bu windows tanımlanmış bir `.storyboard` dosya otomatik olarak projeye dahil. Windows tasarımınızı düzenlemeye **Çözüm Gezgini**, çift tıklayarak `Main.storyboard` dosyası:

[![](outline-view-images/edit01.png "Ana görsel taslak seçme")](outline-view-images/edit01.png#lightbox)

Bu pencere tasarım Xcode'un arabirimi Oluşturucu'da açın:

[![](outline-view-images/edit02.png "Xcode kullanıcı Arabiriminde düzenleme")](outline-view-images/edit02.png#lightbox)

Tür `outline` içine **kitaplığı Denetçisi'nin** anahat görünüm denetimlerini bulmayı kolaylaştırmak için arama kutusuna:

[![](outline-view-images/edit03.png "Kitaplıktan bir anahat görünümü seçme")](outline-view-images/edit03.png#lightbox)

Bir ana görünüm görünüm denetleyicisi sürükleyin **Arayüzü Düzenleyicisi**, görünüm denetleyicisi pencerenin içerik alanı doldurun ve burada küçüldükçe ve penceresinde ile büyüdükçe ayarlayın **kısıtlaması Düzenleyicisi**:

[![](outline-view-images/edit04.png "Kısıtlama düzenleme")](outline-view-images/edit04.png#lightbox)

Ana görünümünde seçin **arabirimi hiyerarşi** aşağıdaki özellikler kullanılabilir **özniteliği denetçisi**:

[![](outline-view-images/edit05.png "Öznitelik denetçisi")](outline-view-images/edit05.png#lightbox)

- **Anahat sütun** -tablo sütunu hiyerarşik veriler görüntülenir.
- **Otomatik kaydetme anahat sütun** - `true`, anahat sütununun otomatik olarak kaydedilir ve uygulama çalışmaları arasında geri.
- **Girinti** -sütunları genişletilmiş bir öğeyi Girintile tutar.
- **Girinti izleyen hücreleri** - `true`, girinti işareti birlikte hücreleri girintilenecek.
- **Otomatik kaydetme genişletilmiş öğeleri** - `true`, öğeleri genişlet/daralt durumunu otomatik olarak kaydedilir ve uygulama çalışmaları arasında geri.
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
> Eski bir Xamarin.Mac uygulamasını muhafaza sürece `NSView` tabanlı anahat görünümleri, üzerinde kullanılmalıdır `NSCell` tablo görünümleri bağlı. `NSCell` eski olarak kabul edilir ve gelecekte desteklenmeyebilir.

Bir tablo sütununda seçin **arabirimi hiyerarşi** aşağıdaki özellikler kullanılabilir **özniteliği denetçisi**:

[![](outline-view-images/edit06.png "Öznitelik denetçisi")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "Öznitelik denetçisi")](outline-view-images/edit07.png#lightbox)

Tüm Standart Görünüm özellikler şunlardır. Burada bu sütun için satırları yeniden boyutlandırma seçeneğiniz de vardır.

Tablo görünümü hücresi seçin (varsayılan olarak, bir `NSTextField`) içinde **arabirimi hiyerarşi** aşağıdaki özellikler kullanılabilir **özniteliği denetçisi**:

[![](outline-view-images/edit08.png "Öznitelik denetçisi")](outline-view-images/edit08.png#lightbox)

Burada ayarlanan için standart bir metin alanı tüm özelliklerine sahip olacaksınız. Varsayılan olarak, standart bir metin alanı bir sütunda bir hücreyi bilgileri görüntülemek için kullanılır.

Bir tablo hücre görünümü seçin (`NSTableFieldCell`) içinde **arabirimi hiyerarşi** aşağıdaki özellikler kullanılabilir **özniteliği denetçisi**:

[![](outline-view-images/edit09.png "Öznitelik denetçisi")](outline-view-images/edit09.png#lightbox)

Burada en önemli ayarlar şunlardır:

- **Düzen** - bu sütundaki hücrelerin düzenlendiği nasıl seçin.
- **Tek satır modunu kullanan** - `true`, hücre tek bir satıra sınırlıdır.
- **İlk çalışma zamanı düzen genişliği** - `true`, hücre için (el ile veya otomatik olarak) olarak ayarlandığında genişliğini tercih eder uygulamasını çalıştırdığınızda ilk kez görüntülenir.
- **Eylem** -ne zaman denetimleri Düzen **eylem** hücre için gönderilir.
- **Davranış** -bir hücre seçilebilir düzenlenebilir olup olmadığını tanımlar.
- **Zengin metin** - `true`, hücre biçimlendirilmiş ve stil uygulanmış metni görüntüleyebilirsiniz.
- **Geri** - `true`, bunun için hücre sorumluluk üstlenen davranışı geri al.

Tablo hücre görünümü seçin (`NSTableFieldCell`) altındaki bir tablo sütununda **arabirimi hiyerarşi**:

[![](outline-view-images/edit11.png "Tablo hücre görünümü seçme")](outline-view-images/edit10.png#lightbox)

Bu sayede temel olarak kullanılan tablo hücre görünümü düzenlemek _deseni_ belirtilen sütun için oluşturulan tüm hücreler için.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Eylemler ve çıkışlar ekleme

Yalnızca bizim anahat görünümü kullanıma sunmak ihtiyacımız diğer Cocoa UI denetimi gibi ve sütunları ve C# kodu kullanarak için hücre **eylemleri** ve **çıkışlar** (gerekli işlevselliğine bağlı olarak).

Biz kullanıma sunmak istiyorsanız herhangi bir Özet Görünüm öğe işlem aynıdır:

1. Geçiş **Yardımcısı Düzenleyicisi** olduğundan emin olun `ViewController.h` dosyası seçili: 

    [![](outline-view-images/edit11.png "Doğru .h dosyası seçme")](outline-view-images/edit11.png#lightbox)
2. Ana hat görünümünden seçim **arabirimi hiyerarşi**denetim tıklayın ve sürükleyin `ViewController.h` dosya.
3. Oluşturma bir **çıkışı** anahat görünümü adlı için `ProductOutline`: 

    [![](outline-view-images/edit13.png "Bir çıkış yapılandırma")](outline-view-images/edit13.png#lightbox)
4. Oluşturma **çıkışlar** de tablo sütunlarını adlı `ProductColumn` ve `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Bir çıkış yapılandırma")](outline-view-images/edit14.png#lightbox)
5. Değişiklikleri kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Ardından, uygulamayı çalıştırdığınızda şu kod görünen anahat için bazı veriler yazacaksınız.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Anahat görünümü doldurma

Bizim anahat görünümü ile tasarlanmış arabirim Oluşturucu'da ve aracılığıyla kullanıma sunulan bir **çıkışı**, ardından bunu doldurmak için C# kodu için oluşturmamız gerekir.

İlk olarak, yeni bir oluşturalım `Product` ayrı satırlara ve Grup alt ürün bilgileri için sınıf. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `Product` için **adı** tıklatıp **yeni** düğmesi:

[![](outline-view-images/populate01.png "Boş bir sınıf oluşturma")](outline-view-images/populate01.png#lightbox)

Olun `Product.cs` dosya görünüm aşağıdaki gibi:

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

Ardından, bir alt sınıfı oluşturmak ihtiyacımız `NSOutlineDataSource` istendiğinde bizim anahat için veri sağlamak için. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `ProductOutlineDataSource` için **adı** tıklatıp **yeni** düğmesi.

Düzen `ProductTableDataSource.cs` dosyasını açıp aşağıdaki gibi görünmesi:

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

Bu sınıf, bizim anahat görünümün öğeleri için depolama alanına sahip ve geçersiz kılmalar `GetChildrenCount` tablodaki satır sayısını döndürmek için. `GetChild` Belirli bir üst veya alt öğe (ana hat görünüm tarafından istendiği gibi) döndürür ve `ItemExpandable` belirtilen öğenin bir üst veya alt tanımlar.

Son olarak, bir alt sınıfı oluşturmak ihtiyacımız `NSOutlineDelegate` bizim için anahat davranışı sağlamak için. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `ProductOutlineDelegate` için **adı** tıklatıp **yeni** düğmesi.

Düzen `ProductOutlineDelegate.cs` dosyasını açıp aşağıdaki gibi görünmesi:

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

Biz örneğini oluşturduğunuzda `ProductOutlineDelegate`, biz de bir örneğini geçirin `ProductOutlineDataSource` Anahat verileri sağlar. `GetView` Yöntemdir verin sütun ve satır için hücre görüntülemek için bir görünüm (veriler) döndürmekten sorumludur. Mümkünse, varolan bir görünümü hücresi görüntülemek için yeniden kullanılabilir değilse yeni bir görünüm oluşturmanız gerekir.

Ana hat doldurmak için düzenleyelim `MainWindow.cs` dosya ve olun `AwakeFromNib` yöntemi görünüm aşağıdaki gibi:

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

Uygulama çalıştırıyoruz, aşağıdakiler gösterilir:

[![](outline-view-images/populate02.png "Daraltılmış görüntüle")](outline-view-images/populate02.png#lightbox)

Biz ana görünümünde bir düğümünü genişletirseniz, aşağıdaki gibi görünür:

[![](outline-view-images/populate03.png "Genişletilmiş Görünüm")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sütuna göre sıralama

Şimdi bir sütun başlığına tıklayarak Anahat verileri sıralamak kullanıcının verin. İlk olarak, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Seçin `Product` sütun girin `Title` için **sıralama anahtarı**, `compare:` için **Seçici** seçip `Ascending` için **sipariş**:

[![](outline-view-images/sort01.png "Sıralama anahtarı ayarlama")](outline-view-images/sort01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Artık düzenleyelim `ProductOutlineDataSource.cs` dosyasını açıp aşağıdaki yöntemleri ekleyin:

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

`Sort` Yöntemi bize temel veri kaynağındaki verileri sıralamak izin bir verilen `Product` artan veya azalan sınıf alanı. Geçersiz kılınan `SortDescriptorsChanged` kullanımı bir sütun başlığı her tıklayışında yöntemi çağrılır. Geçirilecek olan **anahtar** arabirim oluşturucu ve söz konusu sütun için sıralama düzenini ayarladığımız değeri.

Uygulamayı çalıştırmak ve sütun başlıklarını tıklayın, satır sütuna göre sıralanır:

[![](outline-view-images/sort02.png "Sıralanmış bir çıkış örneğidir")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Satır seçimi

Tek bir satırı seçin, çift kullanıcıya izin vermek isteyip istemediğinizi `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Ana görünümünde seçin **arabirimi hiyerarşi** kaldırın **birden çok** onay kutusu **özniteliği denetçisi**:

[![](outline-view-images/select01.png "Öznitelik denetçisi")](outline-view-images/select01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.


Ardından, Düzenle `ProductOutlineDelegate.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Bu, kullanıcının ana görünümünde tek bir satır seçmesine olanak tanır. Dönüş `false` için `ShouldSelectItem` herhangi öğesi için seçilecek kullanıcı istemediğiniz veya `false` tüm öğeleri seçmek kullanıcı istemiyorsanız, her öğe için.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Birden çok satır seçimi

Birden çok satır seçin ve çift kullanıcıya izin vermek isteyip istemediğinizi `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Ana görünümünde seçin **arabirimi hiyerarşi** ve **birden çok** onay kutusu **özniteliği denetçisi**:

[![](outline-view-images/select02.png "Öznitelik denetçisi")](outline-view-images/select02.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.


Ardından, Düzenle `ProductOutlineDelegate.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Bu, kullanıcının ana görünümünde tek bir satır seçmesine olanak tanır. Dönüş `false` için `ShouldSelectRow` herhangi öğesi için seçilecek kullanıcı istemediğiniz veya `false` tüm öğeleri seçmek kullanıcı istemiyorsanız, her öğe için.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Sırayı seçmek için türü

Anahatlandırma seçili görünümü ile bir karakter yazın izin verin ve ilk satırı seçmek istiyorsanız, o karakteri olan, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Ana görünümünde seçin **arabirimi hiyerarşi** ve **türünü seçin** onay kutusu **özniteliği denetçisi**:

[![](outline-view-images/type01.png "Satır türü düzenleme")](outline-view-images/type01.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Artık düzenleyelim `ProductOutlineDelegate.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

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

`GetNextTypeSelectMatch` Yöntemi belirtilen `searchString` ve ilk öğeyi döndürür `Product` , o dizeyi ait olduğu `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Sütunları yeniden sıralama

Sürükleyin kullanıcıya izin vermek istiyorsanız ana görünümünde sütunları yeniden Sırala, çift `Main.storyboard` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın. Ana görünümünde seçin **arabirimi hiyerarşi** ve **yeniden sıralama** onay kutusu **özniteliği denetçisi**:

[![](outline-view-images/reorder01.png "Öznitelik denetçisi")](outline-view-images/reorder01.png#lightbox)

İçin bir değer sunuyoruz, **otomatik kaydetme** özelliği ve onay **sütun bilgileri** biz Tablo düzeni yaptığınız tüm değişiklikler bizim için otomatik olarak kaydedilecek ve uygulamayı açtığında geri alan çalıştırılır.

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Artık düzenleyelim `ProductOutlineDelegate.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Yöntemi döndürmelidir `true` içine sürükleyin istediğiniz olmasını sağlamak için herhangi bir sütun için yeniden `newColumnIndex`, aksi takdirde dönüş `false`;

Uygulama çalıştırıyoruz, bizim sütunları yeniden sıralamak için sütun üst bilgilerini etrafında sürükleyebilirsiniz:

[![](outline-view-images/reorder02.png "Sipariş sütunları örneği")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Hücre düzenleme

Belirtilen hücre değerlerini düzenlemek Düzenle kullanıcı izin vermek istiyorsanız `ProductOutlineDelegate.cs` dosya ve değiştirme `GetViewForItem` yöntemini aşağıdaki şekilde:

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

Artık kullanıcı uygulama çalıştırıyoruz, Tablo görünümünde hücreleri düzenleyebilirsiniz:

[![](outline-view-images/editing01.png "Hücre düzenleme örneği")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Anahat görünümleri görüntülerini kullanma

Hücresinde bir parçası olarak bir resim eklemek için bir `NSOutlineView`, anahat görünümün tarafından döndürülen veriler nasıl değiştirileceği ihtiyacınız olacak `NSTableViewDelegate's` `GetView` yönteminin kullanılacağını bir `NSTableCellView` tipik yerine `NSTextField`. Örneğin:

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

Daha fazla bilgi için lütfen bkz [kullanarak görüntüleri anahat görünümleri ile](~/mac/app-fundamentals/image.md) bölümünü bizim [görüntüyle](~/mac/app-fundamentals/image.md) belgeleri.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Veri bağlama anahat görünümleri

Xamarin.Mac uygulamanızda veri bağlama ve anahtar-değer kodlaması teknikleri kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız gereken kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma avantajı vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), bakımı kolay, daha esnek bir uygulama için önde gelen Tasarım.

Anahtar-değer kodlaması (KVC) olan bir mekanizma örnek değişkenleri erişmesini yerine özellikleri tanımlamak için anahtarları (özel olarak biçimlendirilmiş dizeler) veya erişimci yöntemlerini kullanarak, dolaylı olarak bir nesnenin özelliklerine erişme (`get/set`). Xamarin.Mac uygulamanızda anahtar-değer kodlaması uyumlu erişimcilerini uygulayarak, anahtar-değer gözleme (KVO), veri bağlama, temel veri, Cocoa bağlamaları ve scriptability gibi diğer macOS özelliklere erişiminiz olur.

Daha fazla bilgi için lütfen bkz [anahat görünüm veri bağlama](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) bölümünü bizim [veri bağlama ve anahtar-değer kodlaması](~/mac/app-fundamentals/databinding.md) belgeleri.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede ayrıntılı bir Xamarin.Mac uygulamasında anahat görünümlerle çalışma göz duruma getirdi. Biz farklı türleri gördünüz ve ana hat görünümleri, oluşturma ve ana hat görünümleri Xcode'un arabirimi Oluşturucu Koru ve C# kodunda anahat görünümleri ile çalışmaya nasıl kullanır.

## <a name="related-links"></a>İlgili bağlantılar

- [MacOutlines (örnek)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (örnek)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Tablo Görünümleri](~/mac/user-interface/table-view.md)
- [Kaynak Listeleri](~/mac/user-interface/source-list.md)
- [Veri Bağlama ve Anahtar-Değer Kodlaması](~/mac/app-fundamentals/databinding.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Anahat görünümleri giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
