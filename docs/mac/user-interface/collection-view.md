---
title: Xamarin.Mac koleksiyon görünümleri
description: Bu makalede bir Xamarin.Mac uygulamasını koleksiyon görünümleri ile çalışma. Oluşturma ve koleksiyon görünümlerini Xcode ve arabirim Oluşturucu bakımını yapma ve program aracılığıyla çalışma ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/24/2017
ms.openlocfilehash: c8b4b5ff8bebf5fbbded410ae84d1aefcca2d6cc
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780606"
---
# <a name="collection-views-in-xamarinmac"></a>Xamarin.Mac koleksiyon görünümleri

_Bu makalede bir Xamarin.Mac uygulamasını koleksiyon görünümleri ile çalışma. Oluşturma ve koleksiyon görünümlerini Xcode ve arabirim Oluşturucu bakımını yapma ve program aracılığıyla çalışma ele alınmaktadır._

AppKit koleksiyon görünümü bir Xamarin.Mac uygulamasını, geliştirici çalışma C# ve .NET ile aynı erişimi olduğunda, çalışan bir geliştirici denetleyen *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Geliştirici Xcode'un kullanan _arabirim Oluşturucu_ oluşturmak ve koleksiyon görünümlerini korumak için.

A `NSCollectionView` kullanarak düzenli subviews kılavuzunun görüntüler bir `NSCollectionViewLayout`. Her alt görünüm kılavuzunda tarafından temsil edilen bir `NSCollectionViewItem` görünümün içeriğini yüklenmesini yöneten bir `.xib` dosya.

[![Bir örnek uygulama çalıştırma](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

Bu makalede bir Xamarin.Mac uygulamasını ile koleksiyon görünümlerini çalışmanın temelleri ele alınmaktadır. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makale boyunca kullanılır teknikler kapsar.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>Koleksiyon görünümleri hakkında

Asıl amacı, bir koleksiyon görünümü (`NSCollectionView`) bir grup nesne bir koleksiyon görünümü düzen kullanılarak düzenli bir şekilde görsel olarak ayarlamaktır (`NSCollectionViewLayout`), her ayrı nesneyi ile (`NSCollectionViewItem`) daha büyük koleksiyonda kendi görünüm alınıyor. Koleksiyon görünümlerini çalışma veri bağlama ve anahtar-değer kodlaması teknikleri ve bu nedenle, okumanız gereken [veri bağlama ve anahtar-değer kodlaması](~/mac/app-fundamentals/databinding.md) bu makalede ile devam etmeden önce belgeleri.

Geliştirici tasarlama ve uygulama için sorumlu standart, yerleşik koleksiyon görünümü (anahat veya Tablo görünümü yaptığı gibi) öğe yok, koleksiyon görünümü bulunduğundan bir _prototip görünümü_ görüntü alanları gibi diğer AppKit denetimleri kullanma , Metin alanları, etiketler, vs. Bu prototip görünüm depolanır ve görüntülemek ve koleksiyon görünümü tarafından yönetilen her bir öğesi ile çalışmak için kullanılacak bir `.xib` dosya.

Geliştirici görünümü için bir koleksiyon görünümü öğesinin sorumlu olduğu için koleksiyon görünümü kılavuz seçili bir öğe vurgulama için hiçbir yerleşik destek sunmaktadır. Uygulama bu özellik bu makalede ele alınmaktadır.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Veri modelini tanımlama

Önce veri bir koleksiyon görünümü arabirim Oluşturucu, bir anahtar-değer kodlaması (KVC) bağlama / anahtar-değer gözleme (KVO) uyumlu sınıflar olarak davranmak için Xamarin.Mac uygulamaya tanımlanmalıdır _veri modeli_ bağlama için. Veri modeli, tüm koleksiyonunda görüntülenir ve herhangi bir değişiklik uygulama çalışırken kullanıcı Arabiriminde kullanıcının yaptığı veri alan verileri sağlar.

Bir çalışan grubunu yöneten bir uygulama örneği almak, veri modeli tanımlamak için aşağıdaki sınıf kullanılabilir:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon
        {
            get
            {
                if (isManager)
                {
                    return NSImage.ImageNamed("IconGroup");
                }
                else
                {
                    return NSImage.ImageNamed("IconUser");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

`PersonModel` Bu makalenin geri kalan aşamalarında veri modeli kullanılır.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>Koleksiyon görünümü ile çalışırken

Koleksiyon görünümü ile veri bağlaması olan bir tablo görünümü gibi çok bağlamayla olarak `NSCollectionViewDataSource` koleksiyon için veri sağlamak için kullanılır. Koleksiyon görünümü önceden oluşturulmuş görüntü biçimi sahip olmadığından, daha fazla iş kullanıcı etkileşimi geri bildirim sağlamak ve kullanıcı seçimine izlemek için gereklidir.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Hücre prototip oluşturma

Koleksiyon görünümü varsayılan hücre prototip içermediğinden, geliştirici bir veya daha fazla eklemeniz gerekir `.xib` tek tek hücrelere içeriğini ve düzenini tanımlamak için Xamarin.Mac uygulamasını dosyaları.

Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, proje adını sağ tıklatın ve seçin **Ekle** > **yeni dosya...**
2. Seçin **Mac** > **görünüm denetleyicisi**, bir ad verin (gibi `EmployeeItem` Bu örnekte) tıklayıp **yeni** oluşturmak için: 

    ![Yeni bir görünüm denetleyicisi ekleme](collection-view-images/proto01.png)

    Bu bir `EmployeeItem.cs`, `EmployeeItemController.cs` ve `EmployeeItemController.xib` projenin çözüm dosyası.
3. Çift `EmployeeItemController.xib` dosyayı Xcode'un arabirimi Oluşturucusu'nda düzenleme için açın.
4. Ekleme bir `NSBox`, `NSImageView` ve iki `NSLabel` denetimleri görüntülemek ve şu şekilde düzenlemeniz:

    ![Hücre prototip düzenini tasarlama](collection-view-images/proto02.png)
5. Açık **Yardımcısı Düzenleyicisi** oluşturup bir **çıkışı** için `NSBox` böylece bir hücre seçimi durumunu göstermek için kullanılabilir:

    ![Bir çıkış olarak NSBox gösterme](collection-view-images/proto03.png)
6. Geri dönüp **Standart Düzenleyici** ve görüntü görünümü seçin.
7. İçinde **bağlama denetçisi**seçin **bağlamak için** > **dosya sahibi** girin bir **modeli anahtar yolunu** , `self.Person.Icon`:

    ![Simge bağlama](collection-view-images/proto04.png)
8. İlk etiketi seçin ve **bağlama denetçisi**seçin **bağlamak için** > **dosya sahibi** girin bir **modeli anahtar yolunu**, `self.Person.Name`:

    ![Bağlama adı](collection-view-images/proto05.png)
9. İkinci etiketi seçin ve **bağlama denetçisi**seçin **bağlamak için** > **dosya sahibi** girin bir **modeli anahtar yolunu**, `self.Person.Occupation`:

    ![Meslek bağlama](collection-view-images/proto06.png)
10. Değişiklikleri kaydetmek `.xib` dosya ve değişiklikleri eşitlemek için Visual Studio'ya geri dönün.

Düzen `EmployeeItemController.cs` dosyasını açıp aşağıdaki gibi görünmesi:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// The Employee item controller handles the display of the individual items that will
    /// be displayed in the collection view as defined in the associated .XIB file.
    /// </summary>
    public partial class EmployeeItemController : NSCollectionViewItem
    {
        #region Private Variables
        /// <summary>
        /// The person that will be displayed.
        /// </summary>
        private PersonModel _person;
        #endregion

        #region Computed Properties
        // strongly typed view accessor
        public new EmployeeItem View
        {
            get
            {
                return (EmployeeItem)base.View;
            }
        }

        /// <summary>
        /// Gets or sets the person.
        /// </summary>
        /// <value>The person that this item belongs to.</value>
        [Export("Person")]
        public PersonModel Person
        {
            get { return _person; }
            set
            {
                WillChangeValue("Person");
                _person = value;
                DidChangeValue("Person");
            }
        }

        /// <summary>
        /// Gets or sets the color of the background for the item.
        /// </summary>
        /// <value>The color of the background.</value>
        public NSColor BackgroundColor {
            get { return Background.FillColor; }
            set { Background.FillColor = value; }
        }

        /// <summary>
        /// Gets or sets a value indicating whether this <see cref="T:MacCollectionNew.EmployeeItemController"/> is selected.
        /// </summary>
        /// <value><c>true</c> if selected; otherwise, <c>false</c>.</value>
        /// <remarks>This also changes the background color based on the selected state
        /// of the item.</remarks>
        public override bool Selected
        {
            get
            {
                return base.Selected;
            }
            set
            {
                base.Selected = value;

                // Set background color based on the selection state
                if (value) {
                    BackgroundColor = NSColor.DarkGray;
                } else {
                    BackgroundColor = NSColor.LightGray;
                }
            }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public EmployeeItemController(IntPtr handle) : base(handle)
        {
            Initialize();
        }

        // Called when created directly from a XIB file
        [Export("initWithCoder:")]
        public EmployeeItemController(NSCoder coder) : base(coder)
        {
            Initialize();
        }

        // Call to load from the XIB/NIB file
        public EmployeeItemController() : base("EmployeeItem", NSBundle.MainBundle)
        {
            Initialize();
        }

        // Added to support loading from XIB/NIB
        public EmployeeItemController(string nibName, NSBundle nibBundle) : base(nibName, nibBundle) {

            Initialize();
        }

        // Shared initialization code
        void Initialize()
        {
        }
        #endregion
    }
}
```

Devraldığı sınıfı bu ayrıntısı koddaki bakarak, `NSCollectionViewItem` için bir koleksiyon görünümü hücresi için bir prototip olarak görev yapabilir. `Person` Xcode'da etiketleri ve görüntü görünüm veri bağlama için kullanılan sınıf özelliğini gösterir. Bunun bir örneğidir `PersonModel` yukarıda oluşturduğunuz.

`BackgroundColor` Özelliği için bir kısayol `NSBox` denetimin `FillColor` hücre seçimi durumunu göstermek için kullanılır. Geçersiz kılma tarafından `Selected` özelliği `NSCollectionViewItem`, aşağıdaki kod bu seçim durumu kaldırır veya ayarlar:

```csharp
public override bool Selected
{
    get
    {
        return base.Selected;
    }
    set
    {
        base.Selected = value;

        // Set background color based on the selection state
        if (value) {
            BackgroundColor = NSColor.DarkGray;
        } else {
            BackgroundColor = NSColor.LightGray;
        }
    }
}
```

<a name="Creating-the-Collection-View-Data-Source"/>

### <a name="creating-the-collection-view-data-source"></a>Koleksiyon görünümü veri kaynağı oluşturma

Koleksiyon görünümü veri kaynağı (`NSCollectionViewDataSource`) tüm veriler için bir koleksiyon görünümü sağlar ve bir koleksiyon görünümü hücresi doldurur oluşturur (kullanarak `.xib` prototip) koleksiyondaki her öğe için gerektiği şekilde.

Yeni bir sınıf ekleyin proje çağrısından `CollectionViewDataSource` ve aşağıdaki gibi görünmesi:

```csharp
using System;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view data source provides the data for the collection view.
    /// </summary>
    public class CollectionViewDataSource : NSCollectionViewDataSource
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent collection view.
        /// </summary>
        /// <value>The parent collection view.</value>
        public NSCollectionView ParentCollectionView { get; set; }

        /// <summary>
        /// Gets or sets the data that will be displayed in the collection.
        /// </summary>
        /// <value>A collection of PersonModel objects.</value>
        public List<PersonModel> Data { get; set; } = new List<PersonModel>();
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDataSource"/> class.
        /// </summary>
        /// <param name="parent">The parent collection that this datasource will provide data for.</param>
        public CollectionViewDataSource(NSCollectionView parent)
        {
            // Initialize
            ParentCollectionView = parent;

            // Attach to collection view
            parent.DataSource = this;

        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Gets the number of sections.
        /// </summary>
        /// <returns>The number of sections.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        public override nint GetNumberOfSections(NSCollectionView collectionView)
        {
            // There is only one section in this view
            return 1;
        }

        /// <summary>
        /// Gets the number of items in the given section.
        /// </summary>
        /// <returns>The number of items.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="section">The Section number to count items for.</param>
        public override nint GetNumberofItems(NSCollectionView collectionView, nint section)
        {
            // Return the number of items
            return Data.Count;
        }

        /// <summary>
        /// Gets the item for the give section and item index.
        /// </summary>
        /// <returns>The item.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPath">Index path specifying the section and index.</param>
        public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
        {
            var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
            item.Person = Data[(int)indexPath.Item];

            return item;
        }
        #endregion
    }
}
```

Devraldığı sınıfı bu ayrıntısı koddaki bakarak, `NSCollectionViewDataSource` listesini gösteren `PersonModel` aracılığıyla örnekleri kendi `Data` özelliği.

Bu koleksiyon yalnızca bir bölüm olduğundan, kodun geçersiz kılmalar `GetNumberOfSections` yöntemi ve her zaman döndürür `1`. Ayrıca, `GetNumberofItems` yöntemi geçersiz kılınmıştır öğe sayısını döndürür, `Data` özellik listesi.

`GetItem` Yöntemi yeni bir hücre gereklidir ve aşağıdaki gibi olduğunda çağrılır:

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

`MakeItem` Koleksiyon görünümü yöntemi oluşturma veya yeniden kullanılabilir bir örneğini döndürmek için çağrılır `EmployeeItemController` ve kendi `Person` ayarlanırsa istenen hücresinde görüntülenmesini öğesi. 

`EmployeeItemController` Koleksiyon görünümü denetleyicisi başlamadan önce aşağıdaki kodu kullanarak kayıtlı olması gerekir:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

**Tanımlayıcı** (`EmployeeCell`) kullanılan `MakeItem` çağrı _gerekir_ eşleşen koleksiyon görünümü ile kaydedilen görünümü denetleyicinin adı. Bu adım, aşağıda ayrıntılı olarak ele alınacaktır.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>İşleme öğe seçimi

Seçim ve koleksiyondaki öğelerin deselection işlemek için bir `NSCollectionViewDelegate` gerekli olacaktır. Bu örnek yerleşik olarak kullandıklarından `NSCollectionViewFlowLayout` düzen türünü bir `NSCollectionViewDelegateFlowLayout` Bu temsilci belirli bir sürümünü gerekli olacaktır.

Çağrı, projeye yeni bir sınıf ekleyin `CollectionViewDelegate` ve aşağıdaki gibi görünmesi:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view delegate handles user interaction with the elements of the 
    /// collection view for the Flow-Based layout type.
    /// </summary>
    public class CollectionViewDelegate : NSCollectionViewDelegateFlowLayout
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent view controller.
        /// </summary>
        /// <value>The parent view controller.</value>
        public ViewController ParentViewController { get; set; }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDelegate"/> class.
        /// </summary>
        /// <param name="parentViewController">Parent view controller.</param>
        public CollectionViewDelegate(ViewController parentViewController)
        {
            // Initialize
            ParentViewController = parentViewController;
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Handles one or more items being selected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being selected.</param>
        public override void ItemsSelected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = (int)paths[0].Item;

            // Save the selected item
            ParentViewController.PersonSelected = ParentViewController.Datasource.Data[index];

        }

        /// <summary>
        /// Handles one or more items being deselected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being deselected.</param>
        public override void ItemsDeselected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = paths[0].Item;

            // Clear selection
            ParentViewController.PersonSelected = null;
        }
        #endregion
    }
}
``` 

`ItemsSelected` Ve `ItemsDeselected` yöntemleri geçersiz kılındı ve ayarlama veya kaldırma için kullanılan `PersonSelected` kullanıcı seçer veya öğeyi dilimleyiciye koleksiyon görünümü işleyen görünüm denetleyicisi özelliğidir. Bu, aşağıda ayrıntılı olarak gösterilir.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Arabirimi Oluşturucu'da koleksiyon görünümü oluşturma

Tüm gerekli destekleyici parçaları yerinde, ana görsel taslak düzenlenebilir ve bir koleksiyon görünümü kendisine eklenir.

Aşağıdakileri yapın:

1. Çift `Main.Storyboard` dosyası **Çözüm Gezgini** Xcode'da düzenlemek üzere açmak için kullanıcının arabirim Oluşturucu.
2. Koleksiyon görünümü ana görünüme sürükleyin ve görünüm dolduracak şekilde yeniden boyutlandırın:

    ![Koleksiyon görünümü düzenine ekleme](collection-view-images/collection01.png)
3. Seçili koleksiyon görünümü ile sabitlemek için bu görünümü, yeniden boyutlandırıldığında kısıtlaması Düzenleyicisi'ni kullanın:

    ![Kısıtlamaları ekleme](collection-view-images/collection02.png)
4. Koleksiyon görünümü içinde seçili olduğundan emin olun **tasarım yüzeyine** (ve **katýna kaydırma görünümü** veya **küçük resim görünümü** onu içeren), geçiş  **Yardımcısı Düzenleyicisi** oluşturup bir **çıkışı** koleksiyon görünümü için:

    ![Kısıtlamaları ekleme](collection-view-images/collection03.png)
5. Değişiklikleri kaydetmek ve eşitlemek için Visual Studio'ya geri dönün.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>Tüm birlikte getirme

Tüm destekleyici parçaları artık bir sınıf veri modeli olarak davranan bir yer içine koyulmuş (`PersonModel`), `NSCollectionViewDataSource` veri sağlamak için eklenen bir `NSCollectionViewDelegateFlowLayout` öğe seçimi işlemek için oluşturuldu ve `NSCollectionView` ana film şeridi eklendi ve bir çıkış kullanıma sunulan (`EmployeeCollection`).

Koleksiyon görünümü içeren görünüm denetleyicisi düzenleyin ve tüm parçaları birlikte koleksiyonu doldurmak ve öğe seçimi işlemek için duruma getirmek için son adımdır bakın.

Düzen `ViewController.cs` dosyasını açıp aşağıdaki gibi görünmesi:

```csharp
using System;
using AppKit;
using Foundation;
using CoreGraphics;

namespace MacCollectionNew
{
    /// <summary>
    /// The View controller controls the main view that houses the Collection View.
    /// </summary>
    public partial class ViewController : NSViewController
    {
        #region Private Variables
        private PersonModel _personSelected;
        private bool shouldEdit = true;
        #endregion

        #region Computed Properties
        /// <summary>
        /// Gets or sets the datasource that provides the data to display in the 
        /// Collection View.
        /// </summary>
        /// <value>The datasource.</value>
        public CollectionViewDataSource Datasource { get; set; }

        /// <summary>
        /// Gets or sets the person currently selected in the collection view.
        /// </summary>
        /// <value>The person selected or <c>null</c> if no person is selected.</value>
        [Export("PersonSelected")]
        public PersonModel PersonSelected
        {
            get { return _personSelected; }
            set
            {
                WillChangeValue("PersonSelected");
                _personSelected = value;
                DidChangeValue("PersonSelected");
                RaiseSelectionChanged();
            }
        }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.ViewController"/> class.
        /// </summary>
        /// <param name="handle">Handle.</param>
        public ViewController(IntPtr handle) : base(handle)
        {
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Called after the view has finished loading from the Storyboard to allow it to
        /// be configured before displaying to the user.
        /// </summary>
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();

            // Initialize Collection View
            ConfigureCollectionView();
            PopulateWithData();
        }
        #endregion

        #region Private Methods
        /// <summary>
        /// Configures the collection view.
        /// </summary>
        private void ConfigureCollectionView()
        {
            EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");

            // Create a flow layout
            var flowLayout = new NSCollectionViewFlowLayout()
            {
                ItemSize = new CGSize(150, 150),
                SectionInset = new NSEdgeInsets(10, 10, 10, 20),
                MinimumInteritemSpacing = 10,
                MinimumLineSpacing = 10
            };
            EmployeeCollection.WantsLayer = true;

            // Setup collection view
            EmployeeCollection.CollectionViewLayout = flowLayout;
            EmployeeCollection.Delegate = new CollectionViewDelegate(this);

        }

        /// <summary>
        /// Populates the Datasource with data and attaches it to the collection view.
        /// </summary>
        private void PopulateWithData()
        {
            // Make datasource
            Datasource = new CollectionViewDataSource(EmployeeCollection);

            // Build list of employees
            Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
            Datasource.Data.Add(new PersonModel("Amy Burns", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Joel Martinez", "Web & Infrastructure"));
            Datasource.Data.Add(new PersonModel("Kevin Mullins", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Mark McLemore", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Tom Opgenorth", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Larry O'Brien", "API Docs Manager", true));
            Datasource.Data.Add(new PersonModel("Mike Norman", "API Documentor"));

            // Populate collection view
            EmployeeCollection.ReloadData();
        }
        #endregion

        #region Events
        /// <summary>
        /// Selection changed delegate.
        /// </summary>
        public delegate void SelectionChangedDelegate();

        /// <summary>
        /// Occurs when selection changed.
        /// </summary>
        public event SelectionChangedDelegate SelectionChanged;

        /// <summary>
        /// Raises the selection changed event.
        /// </summary>
        internal void RaiseSelectionChanged() {
            // Inform caller
            if (this.SelectionChanged != null) SelectionChanged();
        }
        #endregion
    }
}
```

Bu kodu göz ayrıntılı olarak ayırdığınız bir `Datasource` özelliği tanımlı örneğini tutacak `CollectionViewDataSource` koleksiyon görünümü için veri sağlamanın. A `PersonSelected` tutacak özelliği tanımlı `PersonModel` koleksiyon görünümü seçili öğeyi temsil eden. Bu özellik ayrıca başlatır `SelectionChanged` seçim değiştiğinde olay.

`ConfigureCollectionView` Sınıfı aşağıdaki satırı kullanarak koleksiyon görünümü ile hücre prototip olarak davranan bir görünüm denetleyicisi kaydetmek için kullanılır:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Dikkat **tanımlayıcı** (`EmployeeCell`) adlı bir prototip eşleşmeleri kaydetmek için kullanılan `GetItem` yöntemi `CollectionViewDataSource` yukarıda tanımlanan:

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

Ayrıca, görünüm türünü **gerekir** adıyla eşleşmesi `.xib` tanımlayan prototip dosyası **tam olarak**. Bu örnekte, söz konusu olduğunda `EmployeeItemController` ve `EmployeeItemController.xib`.

Koleksiyon görünümü'ndeki öğelerin kullanımını gerçek düzeni koleksiyon görünümü Düzen sınıfı tarafından denetlenir ve yeni bir örneğine atayarak çalışma zamanında dinamik olarak değiştirilebilir `CollectionViewLayout` özelliği. Bu özelliği değiştirmek, değişiklik hareketlendirme olmadan koleksiyon görünümü görünümünü güncelleştirir.

Apple, iki yerleşik Düzen türü en tipik kullanımları işleyecek koleksiyon görünümü ile birlikte gelir: `NSCollectionViewFlowLayout` ve `NSCollectionViewGridLayout`. Geliştirici bir daire öğelerinde out düzenleme gibi özel bir biçim gerekirse özel bir örneğini oluşturabilirsiniz `NSCollectionViewLayout` ve istenilen etkiyi elde etmek için gereken yöntemleri geçersiz kılın.

Bu örnek bir örneğini oluşturur, varsayılan akış düzeni kullanır `NSCollectionViewFlowLayout` sınıfı ve aşağıdaki gibi yapılandırılır:

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

`ItemSize` Özelliği, koleksiyondaki her hücre boyutunu tanımlar. `SectionInset` İç metinleri, hücre düzenleneceğini koleksiyonu kenarından özelliği tanımlar. `MinimumInteritemSpacing` öğeleri arasında en az aralık tanımlar ve `MinimumLineSpacing` koleksiyondaki satırlar arasında en az aralık tanımlar.

Koleksiyon görünümü ve bir örneğini düzenini atanan `CollectionViewDelegate` öğe seçimi işlemek için eklenir:

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

`PopulateWithData` Yöntemi yeni bir örneğini oluşturur `CollectionViewDataSource`, verilerle doldurur, koleksiyon görünümü ve aramaları ekler `ReloadData` yöntemi öğeleri görüntülemek için:

```csharp
private void PopulateWithData()
{
    // Make datasource
    Datasource = new CollectionViewDataSource(EmployeeCollection);

    // Build list of employees
    Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
    ...

    // Populate collection view
    EmployeeCollection.ReloadData();
}
```

`ViewDidLoad` Yöntemi geçersiz kılınır ve çağıran `ConfigureCollectionView` ve `PopulateWithData` yöntemleri kullanıcıya son koleksiyonu görüntülemek için:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Initialize Collection View
    ConfigureCollectionView();
    PopulateWithData();
}
```

<a name="Summary"/>

## <a name="summary"></a>Özet

Bu makalede ayrıntılı bir Xamarin.Mac uygulamasında koleksiyon görünümlerle çalışma göz duruma getirdi. İlk olarak, anahtar-değer kodlaması (KVC) ve anahtar-değer gözleme (KVO) kullanarak bir C# sınıf Objective-c gösterme görünüyordu. Ardından, KVO uyumlu sınıfının nasıl kullanılacağını gösterdi ve veri koleksiyonu görünümle Xcode'un arabirim Oluşturucu bağlar. Son olarak, C# kodunda koleksiyon görünümleri ile etkileşim nasıl oluşturulacağını gösterir.

## <a name="related-links"></a>İlgili bağlantılar

- [MacCollectionNew (örnek)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Veri Bağlama ve Anahtar-Değer Kodlaması](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
