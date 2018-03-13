---
title: "Koleksiyon görünümleri"
description: "Bu makalede Xamarin.Mac uygulamasında koleksiyon görünümleri ile çalışmayı açıklar. Oluşturma ve koleksiyon görünümlerini Xcode ve arabirim Oluşturucu koruma ve bunlarla program aracılığıyla çalışma kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/24/2017
ms.openlocfilehash: 9aa66a531b723f176b940ba35ee4e86eae711f7d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="collection-views"></a>Koleksiyon görünümleri

_Bu makalede Xamarin.Mac uygulamasında koleksiyon görünümleri ile çalışmayı açıklar. Oluşturma ve koleksiyon görünümlerini Xcode ve arabirim Oluşturucu koruma ve bunlarla program aracılığıyla çalışma kapsar._

AppKit koleksiyon görünümü Xamarin.Mac uygulama, geliştiriciler C# ve .NET ile çalışma erişimi aynı olduğunda, çalışan bir geliştirici denetleyen *Objective-C* ve *Xcode* yapar. Geliştirici, Xcode'nın kullanır, Xamarin.Mac Xcode ile doğrudan tümleşir çünkü _arabirimi Oluşturucu_ oluşturmak ve koleksiyon görünümlerini güncelleştirmek için.

A `NSCollectionView` kullanılarak organize subviews oluşan bir kılavuz görüntüler bir `NSCollectionViewLayout`. Her alt görünüm kılavuzunda tarafından temsil edilen bir `NSCollectionViewItem` görünümün içerikten yüklenmesini yöneten bir `.xib` dosyası.

[![Bir örnek uygulamayı çalıştırma](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

Bu makalede, bir Xamarin.Mac uygulamasında koleksiyon görünümleri ile çalışmanın temelleri kapsar. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve bu makale kullanılan teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>Koleksiyon görünümleri hakkında

Asıl amacı, bir koleksiyon görünümü (`NSCollectionView`) bir koleksiyon görünümü düzen kullanarak düzenli bir şekilde bir grup görsel olarak ayarlamaktır (`NSCollectionViewLayout`), tek tek her nesnesiyle (`NSCollectionViewItem`) daha büyük koleksiyonunda kendi görünüm alınıyor. Koleksiyon görünümlerini çalışma ile verileri bağlama ve anahtar-değer kodlama teknikleri ve bu nedenle, okumanız gereken [verileri bağlama ve anahtar-değer kodlama](~/mac/app-fundamentals/databinding.md) Bu makale ile devam etmeden önce belgeleri.

Geliştirici tasarlama ve uygulama sorumludur koleksiyon görünümü standart, yerleşik koleksiyon görünümü (anahat veya Tablo görünümü yaptığı gibi) öğe, sahiptir, bu nedenle bir _prototip Görünüm_ görüntü alanları gibi diğer AppKit denetimlerini kullanma , Metin alanları, etiketler, vs. Bu prototip görünüm depolanır ve görüntülemek ve koleksiyon görünümü tarafından yönetilen her bir öğe ile çalışmak için kullanılacak bir `.xib` dosyası.

Geliştirici görünüm için bir koleksiyon görünümü öğesinin sorumlu olduğu için koleksiyon görünümü kılavuz içinde seçilen bir öğeyi vurgulama hiçbir yerleşik desteğe sahiptir. Bu özellik uygulama bu makalede ele alınacaktır.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Veri modeli tanımlama

Veri Arabirimi Oluşturucu'da, bir anahtar-değer kodlama (KVC) bir koleksiyon görünümü bağlama önce / anahtar-değer Gözlemleme (KVO) uyumlu sınıfı tanımlanmış, olarak davranacak şekilde Xamarin.Mac uygulama _veri modeli_ bağlama için. Veri modeli tüm koleksiyonunda görüntülenir ve kullanıcı Arabiriminde uygulaması çalıştırılırken yapar verilere herhangi bir değişiklik alan verileri sağlar.

Bir çalışan grubu yöneten bir uygulama örneği alın, aşağıdaki sınıf veri modeli tanımlamak için kullanılabilir:

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

`PersonModel` Veri modeli bu makalenin geri kalanında kullanılır.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>Bir koleksiyon görünümü ile çalışma

Bir koleksiyon görünümü ile veri bağlaması olarak bir tablo görünümü gibi çok bağlamayla olan `NSCollectionViewDataSource` için koleksiyon veri sağlamak için kullanılır. Koleksiyon görünümü hazır görüntü biçimi sahip olmadığından, daha fazla iş kullanıcı etkileşimi geribildirim sağlamak ve kullanıcının seçimi izlemek için gereklidir.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Hücre prototip oluşturma

Koleksiyon görünümü varsayılan hücre prototip içermediğinden geliştirici bir veya daha fazla eklemeniz gerekir `.xib` düzenini ve tek tek hücrelere içeriğini tanımlamak için Xamarin.Mac uygulama dosyaları.

Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve seçin **Ekle** > **yeni dosya...**
2. Seçin **Mac** > **View Controller**, bir ad verin (gibi `EmployeeItem` Bu örnekte) tıklatıp **yeni** düğmesi oluşturmak için: 

    ![Yeni bir görünüm denetleyicisi ekleme](collection-view-images/proto01.png)

    Bu ekler bir `EmployeeItem.cs`, `EmployeeItemController.cs` ve `EmployeeItemController.xib` projenin çözüm dosyasına.
3. Çift `EmployeeItemController.xib` dosyayı Xcode'nın arabirimi Oluşturucusu'nda düzenlemek için açın.
4. Ekleme bir `NSBox`, `NSImageView` ve iki `NSLabel` görünümüne denetimler ve aşağıdaki gibi düzenlenmesine:

    ![Hücre prototip düzeni tasarlama](collection-view-images/proto02.png)
5. Açık **Yardımcısı Düzenleyicisi** ve oluşturma bir **çıkışı** için `NSBox` böylece bir hücre seçimi durumunu göstermek için kullanılabilir:

    ![Bir çıkış olarak NSBox gösterme](collection-view-images/proto03.png)
6. Geri dönüp **Standart Düzenleyici** ve görüntü görünümü seçin.
7. İçinde **bağlama denetçisi**seçin **bağlamak için** > **dosyanın sahibi** ve girin bir **modeli anahtar yolu** , `self.Person.Icon`:

    ![Simge bağlama](collection-view-images/proto04.png)
8. İlk etiketi seçin ve **bağlama denetçisi**seçin **bağlamak için** > **dosyanın sahibi** ve girin bir **Model anahtar yolu**, `self.Person.Name`:

    ![Bağlama adı](collection-view-images/proto05.png)
9. İkinci etiketi seçin ve **bağlama denetçisi**seçin **bağlamak için** > **dosyanın sahibi** ve girin bir **Model anahtar yolu**, `self.Person.Occupation`:

    ![Mesleği bağlama](collection-view-images/proto06.png)
10. Değişiklikleri kaydetmek `.xib` dosya ve değişiklikleri eşitlemek için Visual Studio'ya geri dönün.

Düzen `EmployeeItemController.cs` dosya ve şu şekilde görünür yapın:

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

Bu kod ayrıntılı bakarak, sınıfının devraldığı yer `NSCollectionViewItem` koleksiyon görünümü hücre için prototip olarak hareket etmesini sağlamak. `Person` Özelliği veri bağlamak xcode'da etiketleri ve resim görünümü için kullanılan sınıfı gösterir. Bu örneği olan `PersonModel` yukarıda oluşturduğunuz.

`BackgroundColor` Özelliği olan bir kısayol `NSBox` denetimin `FillColor` bir hücre seçimi durumunu göstermek için kullanılır. Geçersiz kılma tarafından `Selected` özelliği `NSCollectionViewItem`, aşağıdaki kod bu seçim durumu temizler veya ayarlar:

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

Bir koleksiyon görünümü veri kaynağı (`NSCollectionViewDataSource`) tüm veriler için bir koleksiyon görünümü sağlar ve oluşturur ve bir koleksiyon görünümü hücresi doldurur (kullanarak `.xib` prototip) koleksiyondaki her öğe için gereklidir.

Yeni bir sınıf ekleyin proje çağrısından `CollectionViewDataSource` ve şu şekilde görünür yapın:

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

Bu kod ayrıntılı bakarak, sınıfının devraldığı yer `NSCollectionViewDataSource` ve bir listesini sunar `PersonModel` aracılığıyla örnekleri kendi `Data` özelliği.

Bu koleksiyon yalnızca bir bölüm olduğundan, kodu geçersiz kılmaları `GetNumberOfSections` yöntemi ve her zaman döndürür `1`. Ayrıca, `GetNumberofItems` yöntemi geçersiz kılınmıştır adresindeki öğelerin sayısını döndürür `Data` özellik listesi.

`GetItem` Yöntemi çağrıldığında yeni hücreye gereklidir ve aşağıdaki gibi görünür:

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

`MakeItem` Oluşturmak veya yeniden kullanılabilir bir örneğini döndürmek için koleksiyon görünümünün yöntemi çağrıldığında `EmployeeItemController` ve kendi `Person` özelliği ayarlanmış istenen hücrede öğe görüntüleniyor. 

`EmployeeItemController` Koleksiyon görünümü önceden aşağıdaki kodu kullanarak denetleyiciyle kayıtlı olması gerekir:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

**Tanımlayıcısı** (`EmployeeCell`) kullanılan `MakeItem` çağrısı _gerekir_ ile koleksiyon görünümü kaydedildi görünüm denetleyicisini adıyla aynı. Bu adım aşağıda ayrıntılı olarak ele alınacaktır.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>İşleme öğe seçimi

Seçim ve deselection koleksiyondaki öğelerin işlemek için bir `NSCollectionViewDelegate` gerekli olacaktır. Bu örnek yerleşik olarak kullanacağınız beri `NSCollectionViewFlowLayout` Düzen türü, bir `NSCollectionViewDelegateFlowLayout` Bu temsilci belirli sürümünü gerekli olacaktır.

Projeye yeni bir sınıf ekleyin çağrı `CollectionViewDelegate` ve şu şekilde görünür yapın:

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

`ItemsSelected` Ve `ItemsDeselected` yöntemleri geçersiz kılındı ve ayarlama veya kaldırma için kullanılan `PersonSelected` kullanıcı seçer ya da bir öğe seçimini kaldırır, koleksiyon görünümü işleyen görünüm denetleyicisini özelliği. Bu aşağıda ayrıntılı olarak gösterilir.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Arabirim Oluşturucusu'nda koleksiyon görünümü oluşturma

Tüm gerekli destek parçaları yerinde, ana film şeridi düzenlenemez ve bir koleksiyon görünümü eklenir.

Aşağıdakileri yapın:

1. Çift `Main.Storyboard` dosyasını **Çözüm Gezgini** Xcode'da düzenlemek üzere açmak için kullanıcının arabirimi Oluşturucu.
2. Bir koleksiyon görünümü ana görünüme sürükleyin ve görünüm dolduracak şekilde yeniden boyutlandırın:

    ![Bir koleksiyon görünümü düzenine ekleme](collection-view-images/collection01.png)
3. Seçili koleksiyon görünümü ile sabitlemek için Görünüm için onu yeniden boyutlandırıldığında kısıtlaması Düzenleyicisi'ni kullanın:

    ![Kısıtlamaları ekleme](collection-view-images/collection02.png)
4. Koleksiyon görünümü içinde seçili olduğundan emin olun **tasarım yüzeyi** (ve **katýna kaydırma Görünüm** veya **küçük resim görünümü** onu içeren), geçiş  **Yardımcısı Düzenleyicisi** ve oluşturma bir **çıkışı** koleksiyon görünümü için:

    ![Kısıtlamaları ekleme](collection-view-images/collection03.png)
5. Değişiklikleri kaydetmek ve eşitlemeye Visual Studio'ya geri dönün.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>Tüm birlikte hale getirme

Tüm destekleyici parçaları artık veri modeli olarak hareket edecek bir sınıf yer içine konmuş (`PersonModel`), bir `NSCollectionViewDataSource` veri sağlamak için eklenen bir `NSCollectionViewDelegateFlowLayout` öğe seçimi işlemek için oluşturulan ve `NSCollectionView` için ana film şeridi eklendi ve prizine kullanıma (`EmployeeCollection`).

Koleksiyon görünümü içeren görünüm denetleyicisini düzenleyin ve birlikte koleksiyonu doldurmak ve öğe seçimi işlemek için şey hale getirmek için son adımdır bakın.

Düzen `ViewController.cs` dosya ve şu şekilde görünür yapın:

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

Bu kodu göz ayrıntılı olarak ayırdığınız bir `Datasource` özelliği tanımlı bir örneğini tutmak için `CollectionViewDataSource` için koleksiyon görünümü verileri sağlayan. A `PersonSelected` özelliği tanımlı tutmak için `PersonModel` koleksiyon görünümü seçili öğenin temsil eden. Bu özellik ayrıca başlatır `SelectionChanged` seçim değiştiğinde olay.

`ConfigureCollectionView` Sınıfı, aşağıdaki satırı kullanarak koleksiyon görünümü ile hücre prototip olarak davranan görünüm denetleyicisini kaydetmek için kullanılır:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Dikkat **tanımlayıcısı** (`EmployeeCell`) adlı bir prototip eşleşmeleri kaydetmek için kullanılan `GetItem` yöntemi `CollectionViewDataSource` yukarıda tanımlanan:

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

Ayrıca, görünüm denetleyicisi türü **gerekir** adıyla eşleşen `.xib` prototip tanımlayan dosyası **tam olarak**. Bu örnekte, söz konusu olduğunda `EmployeeItemController` ve `EmployeeItemController.xib`.

Koleksiyon görünümü'ndeki öğelerin kullanımını gerçek düzeni koleksiyon görünümü düzeni sınıfı tarafından denetlenir ve yeni bir örneğine atayarak çalışma zamanında dinamik olarak değiştirilebilir `CollectionViewLayout` özelliği. Bu özelliğin değiştirilmesi, koleksiyon görünümü Görünüm değişikliği animasyon olmadan güncelleştirir.

Apple iki yerleşik yerleşim türleri en tipik kullanır işleyecek koleksiyon görünümü ile birlikte gelir: `NSCollectionViewFlowLayout` ve `NSCollectionViewGridLayout`. Geliştirici bir daire öğelerde out yerleştirme gibi özel bir biçim gerekiyorsa özel bir örneğini oluşturabilirsiniz `NSCollectionViewLayout` ve istenilen efekti elde etmek için gereken yöntemleri geçersiz kılın.

Bir örneğini oluşturur Bu örnek varsayılan akış düzeni kullanır, bu nedenle `NSCollectionViewFlowLayout` sınıfı ve aşağıdaki gibi yapılandırılır:

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

`ItemSize` Özelliği, koleksiyondaki her hücre boyutunu tanımlar. `SectionInset` Özelliği de hücreleri düzenleneceğini koleksiyonu kenarından iç metinleri tanımlar. `MinimumInteritemSpacing` öğeleri arasında en az aralık tanımlar ve `MinimumLineSpacing` koleksiyondaki satırları arasındaki en az aralık tanımlar.

Düzen koleksiyon görünümü ve bir örneğini atanan `CollectionViewDelegate` öğe seçimi işlemek için bağlı:

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

`PopulateWithData` Yöntemi, yeni bir örneğini oluşturur `CollectionViewDataSource`, verilerle doldurur, koleksiyon görünümü ve aramaları iliştirir `ReloadData` yöntemi öğeleri görüntülemek için:

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

`ViewDidLoad` Yöntemi geçersiz kılınır ve çağırır `ConfigureCollectionView` ve `PopulateWithData` yöntemleri kullanıcıya son koleksiyon görüntülemek için:

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

Bu makalede bir Xamarin.Mac uygulamasında koleksiyonu görünümlerle çalışma ayrıntılı bir bakış sürdü. İlk olarak, anahtar-değer kodlama (KVC) ve anahtar-değer Gözlemleme (KVO) kullanarak bir C# sınıfına Objective-C gösterme Aranan. Ardından, KVO uyumlu sınıfının nasıl kullanılacağı gösterilmiştir ve veri koleksiyonu görünümlere Xcode'nın arabirimi Oluşturucu bağlayın. Son olarak, C# kodunda koleksiyon görünümleri ile etkileşim kurmak nasıl oluşturulacağını gösterir.

## <a name="related-links"></a>İlgili bağlantılar

- [MacCollectionNew (örnek)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Veri Bağlama ve Anahtar-Değer Kodlaması](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
