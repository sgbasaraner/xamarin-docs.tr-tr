---
title: Xamarin.Mac kaynak listeleri
description: Bu makale, bir Xamarin.Mac uygulamasını kaynak listeleriyle çalışma kapsar. Bu, Xcode ve arabirim Oluşturucu kaynak listelerini koruma oluşturma ve C# kodunda normalde açıklar.
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: de51395192ab009f5c7fa338a4414ba553610b8c
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780578"
---
# <a name="source-lists-in-xamarinmac"></a>Xamarin.Mac kaynak listeleri

_Bu makale, bir Xamarin.Mac uygulamasını kaynak listeleriyle çalışma kapsar. Bu, Xcode ve arabirim Oluşturucu kaynak listelerini koruma oluşturma ve C# kodunda normalde açıklar._

Bir Xamarin.Mac uygulamasında çalışırken, C# ve .NET ile aynı erişiminiz kaynak listeler, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Xcode'un kullanabileceğiniz _arabirim Oluşturucu_ oluşturun ve kaynak listelerinin bakımını (veya isteğe bağlı olarak bunları doğrudan C# kodu oluşturmak için).

Kaynak listesini anahat görünümünün kenar çubuğu Bulucu veya iTunes gibi bir eylem kaynağını göstermek için kullanılan özel bir türdür.

[![](source-list-images/source05.png "Bir örnek kaynağı listesi")](source-list-images/source05.png#lightbox)

Bu makalede, biz bir Xamarin.Mac uygulamasında kaynak listeleri ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>Kaynak listeleri giriş

Yukarıda belirtildiği gibi özel bir ana görünüm türü Bulucu veya iTunes kenar çubuğu gibi bir eylem kaynağını göstermek için kullanılan kaynak listesi alır. Kullanıcıya izin veren bir tablo türü genişletin veya hiyerarşik veri satırlarını Daralt kaynak listesi alır. Bir tablo görünümü farklı öğeler kaynak listesinde, düz bir listede değil, bir hiyerarşideki bir sabit sürücü üzerindeki dosya ve klasörleri gibi düzenlenir. Bir kaynak listesindeki bir öğeyi diğer bir öğe içeriyorsa, genişletilmiş veya daraltılmış kullanıcı tarafından.

Kaynak listesi özel stil uygulanmış anahat görülmektedir (`NSOutlineView`), kendisi bir tablo görünümü sınıfıdır (`NSTableView`) ve bu nedenle, çok davranışını kendi üst sınıfından devralır. Sonuç olarak, bir ana görünüm tarafından desteklenen çok sayıda işlemi de desteklenir göre bir kaynak listesi. Bir Xamarin.Mac uygulamasını bu özellikleri denetime sahiptir ve izin verebilir veya yasaklayabilir belirli işlemleri kaynak listenin parametreleri (ya da kod veya arabirim Oluşturucu) yapılandırabilirsiniz.

Kendi veri kaynağı listesinden depolamaz, bunun yerine, bir veri kaynağında kullanır (`NSOutlineViewDataSource`) satır ve sütunları, gerektiğinde bir temelinde gerekli sağlamak.

Bir kaynak listenin davranışı anahat görünümü temsilci öğesinin sağlayarak özelleştirilebilir (`NSOutlineViewDelegate`) işlevselliği seçmek için ana hat türü desteklemek için seçim ve düzenleme, özel izleme ve özel görünüm tek tek öğelerin öğesi.

Tablo görünümü ve bir ana hat görünümü ile kaynak listesini, davranış ve işlevsellik çoğunu paylaşır olduğundan, üzerinden geçmek üzere isteyebilirsiniz bizim [tablo görünümleri](~/mac/user-interface/table-view.md) ve [anahat görünümleri](~/mac/user-interface/outline-view.md) devam etmeden önce belgeleri Bu makalede ile.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>Kaynak listeleri ile çalışma

Kaynak listesini anahat görünümünün kenar çubuğu Bulucu veya iTunes gibi bir eylem kaynağını göstermek için kullanılan özel bir türdür. Arabirimi Oluşturucu'da kaynak listemize tanımlarız önce ana hat görünümleri destekleyen sınıfları Xamarin.Mac'te oluşturalım.

İlk olarak, yeni bir oluşturalım `SourceListItem` kaynak listemize verileri tutmak için sınıf. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `SourceListItem` için **adı** tıklatıp **yeni** düğmesi:

[![](source-list-images/source01.png "Boş bir sınıf ekleme")](source-list-images/source01.png#lightbox)

Olun `SourceListItem.cs` dosya görünüm aşağıdaki gibi: 

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListItem: NSObject, IEnumerator, IEnumerable
    {
        #region Private Properties
        private string _title;
        private NSImage _icon;
        private string _tag;
        private List<SourceListItem> _items = new List<SourceListItem> ();
        #endregion

        #region Computed Properties
        public string Title {
            get { return _title; }
            set { _title = value; }
        }

        public NSImage Icon {
            get { return _icon; }
            set { _icon = value; }
        }

        public string Tag {
            get { return _tag; }
            set { _tag=value; }
        }
        #endregion

        #region Indexer
        public SourceListItem this[int index]
        {
            get
            {
                return _items[index];
            }

            set
            {
                _items[index] = value;
            }
        }

        public int Count {
            get { return _items.Count; }
        }

        public bool HasChildren {
            get { return (Count > 0); }
        }
        #endregion

        #region Enumerable Routines
        private int _position = -1;

        public IEnumerator GetEnumerator()
        {
            _position = -1;
            return (IEnumerator)this;
        }

        public bool MoveNext()
        {
            _position++;
            return (_position < _items.Count);
        }

        public void Reset()
        {_position = -1;}

        public object Current
        {
            get 
            { 
                try 
                {
                    return _items[_position];
                }

                catch (IndexOutOfRangeException)
                {
                    throw new InvalidOperationException();
                }
            }
        }
        #endregion

        #region Constructors
        public SourceListItem ()
        {
        }

        public SourceListItem (string title)
        {
            // Initialize
            this._title = title;
        }

        public SourceListItem (string title, string icon)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
        }

        public SourceListItem (string title, string icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
        }

        public SourceListItem (string title, NSImage icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon, string tag)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
        }

        public SourceListItem (string title, NSImage icon, string tag, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
            this.Clicked = clicked;
        }
        #endregion

        #region Public Methods
        public void AddItem(SourceListItem item) {
            _items.Add (item);
        }

        public void AddItem(string title) {
            _items.Add (new SourceListItem (title));
        }

        public void AddItem(string title, string icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, string icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, NSImage icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon, string tag) {
            _items.Add (new SourceListItem (title, icon, tag));
        }

        public void AddItem(string title, NSImage icon, string tag, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, tag, clicked));
        }

        public void Insert(int n, SourceListItem item) {
            _items.Insert (n, item);
        }

        public void RemoveItem(SourceListItem item) {
            _items.Remove (item);
        }

        public void RemoveItem(int n) {
            _items.RemoveAt (n);
        }

        public void Clear() {
            _items.Clear ();
        }
        #endregion

        #region Events
        public delegate void ClickedDelegate();
        public event ClickedDelegate Clicked;

        internal void RaiseClickedEvent() {
            // Inform caller
            if (this.Clicked != null)
                this.Clicked ();
        }
        #endregion
    }
}
```

İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `SourceListDataSource` için **adı** tıklatıp **yeni** düğmesi. Olun `SourceListDataSource.cs` dosya görünüm aşağıdaki gibi:

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDataSource : NSOutlineViewDataSource
    {
        #region Private Variables
        private SourceListView _controller;
        #endregion

        #region Public Variables
        public List<SourceListItem> Items = new List<SourceListItem>();
        #endregion

        #region Constructors
        public SourceListDataSource (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Properties
        public override nint GetChildrenCount (NSOutlineView outlineView, Foundation.NSObject item)
        {
            if (item == null) {
                return Items.Count;
            } else {
                return ((SourceListItem)item).Count;
            }
        }

        public override bool ItemExpandable (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, Foundation.NSObject item)
        {
            if (item == null) {
                return Items [(int)childIndex];
            } else {
                return ((SourceListItem)item) [(int)childIndex]; 
            }
        }

        public override NSObject GetObjectValue (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            return new NSString (((SourceListItem)item).Title);
        }
        #endregion

        #region Internal Methods
        internal SourceListItem ItemForRow(int row) {
            int index = 0;

            // Look at each group
            foreach (SourceListItem item in Items) {
                // Is the row inside this group?
                if (row >= index && row <= (index + item.Count)) {
                    return item [row - index - 1];
                }

                // Move index
                index += item.Count + 1;
            }

            // Not found 
            return null;
        }
        #endregion
    }
}
```

Bu veri kaynağı listemize sağlar.

İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `SourceListDelegate` için **adı** tıklatıp **yeni** düğmesi. Olun `SourceListDelegate.cs` dosya görünüm aşağıdaki gibi:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDelegate : NSOutlineViewDelegate
    {
        #region Private variables
        private SourceListView _controller;
        #endregion

        #region Constructors
        public SourceListDelegate (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Methods
        public override bool ShouldEditTableColumn (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            return false;
        }

        public override NSCell GetCell (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            nint row = outlineView.RowForItem (item);
            return tableColumn.DataCellForRow (row);
        }

        public override bool IsGroupItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            NSTableCellView view = null;

            // Is this a group item?
            if (((SourceListItem)item).HasChildren) {
                view = (NSTableCellView)outlineView.MakeView ("HeaderCell", this);
            } else {
                view = (NSTableCellView)outlineView.MakeView ("DataCell", this);
                view.ImageView.Image = ((SourceListItem)item).Icon;
            }

            // Initialize view
            view.TextField.StringValue = ((SourceListItem)item).Title;

            // Return new view
            return view;
        }

        public override bool ShouldSelectItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return (outlineView.GetParent (item) != null);
        }

        public override void SelectionDidChange (NSNotification notification)
        {
            NSIndexSet selectedIndexes = _controller.SelectedRows;

            // More than one item selected?
            if (selectedIndexes.Count > 1) {
                // Not handling this case
            } else {
                // Grab the item
                var item = _controller.Data.ItemForRow ((int)selectedIndexes.FirstIndex);

                // Was an item found?
                if (item != null) {
                    // Fire the clicked event for the item
                    item.RaiseClickedEvent ();

                    // Inform caller of selection
                    _controller.RaiseItemSelected (item);
                }
            }
        }
        #endregion
    }
}
```

Bu kaynak listemize davranışını sağlar.

Son olarak **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıf**, girin `SourceListView` için **adı** tıklatıp **yeni** düğmesi. Olun `SourceListView.cs` dosya görünüm aşağıdaki gibi:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }
        
        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

Bu özel, yeniden kullanılabilir öğesinin oluşturur `NSOutlineView` (`SourceListView`), kaynak listesi vermiyoruz herhangi bir Xamarin.Mac uygulamada sürücü için kullanabiliriz.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Oluşturulması ve bakımının yapılması xcode'da kaynak listeleri

Şimdi, şimdi kaynak listemize arabirim Oluşturucu tasarlayın. Çift `Main.storyboard` arabirimi Oluşturucusu'nda düzenlemek üzere açın ve bir bölünmüş görünümünden sürükleyin dosyaya **kitaplığı denetçisi**, görünüm denetleyiciye ekleyin ve görünüm ile yeniden boyutlandırmak için ayarlayın **Kısıtlamaları Düzenleyicisi** :

[![](source-list-images/source00.png "Kısıtlama düzenleme")](source-list-images/source00.png#lightbox)

Ardından, bir kaynak listesinden alan sürükleyin **kitaplığı denetçisi**Bölünmüş Görünüm sol tarafına ekleyin ve görünüm ile yeniden boyutlandırmak için ayarlanmış **Kısıtlamaları Düzenleyicisi**:

[![](source-list-images/source02.png "Kısıtlama düzenleme")](source-list-images/source02.png#lightbox)

Ardından, geçiş **kimlik görünümü**ve kaynak listesinden seçin ve bunu değiştirmeniz 's **sınıfı** için `SourceListView`:

[![](source-list-images/source03.png "Sınıf adı ayarlama")](source-list-images/source03.png#lightbox)

Son olarak, oluşturun bir **çıkışı** kaynak listemize adlı için `SourceList` içinde `ViewController.h` dosyası:

[![](source-list-images/source04.png "Bir çıkış yapılandırma")](source-list-images/source04.png#lightbox)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>Kaynak listesi dolduruluyor

Düzenleyelim `RotationWindow.cs` değerinizi ve Mac için Visual Studio'da dosya 's `AwakeFromNib` yöntemi görünüm aşağıdaki gibi:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Populate source list
    SourceList.Initialize ();

    var library = new SourceListItem ("Library");
    library.AddItem ("Venues", "house.png", () => {
        Console.WriteLine("Venue Selected");
    });
    library.AddItem ("Singers", "group.png");
    library.AddItem ("Genre", "cards.png");
    library.AddItem ("Publishers", "box.png");
    library.AddItem ("Artist", "person.png");
    library.AddItem ("Music", "album.png");
    SourceList.AddItem (library);

    // Add Rotation 
    var rotation = new SourceListItem ("Rotation"); 
    rotation.AddItem ("View Rotation", "redo.png");
    SourceList.AddItem (rotation);

    // Add Kiosks
    var kiosks = new SourceListItem ("Kiosks");
    kiosks.AddItem ("Sign-in Station 1", "imac");
    kiosks.AddItem ("Sign-in Station 2", "ipad");
    SourceList.AddItem (kiosks);

    // Display side list
    SourceList.ReloadData ();
    SourceList.ExpandItem (null, true);

}
```

`Initialize ()` Yöntemi bizim kaynak listenin karşı çağrılması gereken **çıkışı** _önce_ öğeler buna eklenir. Her Grup öğeleri için bir üst öğesi oluşturun ve ardından alt öğeleri bu Grup öğesine ekleyin. Her grup, ardından kaynak listenin koleksiyona eklenir `SourceList.AddItem (...)`. Son iki satırı için kaynak listesi verileri yüklemek ve tüm gruplar genişletilir:

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Son olarak, Düzen `AppDelegate.cs` dosya ve olun `DidFinishLaunching` yöntemi görünüm aşağıdaki gibi:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

Uygulamamızı çalıştırıyoruz, aşağıdaki görüntülenir:

[![](source-list-images/source05.png "Bir örnek uygulama çalıştırma")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede ayrıntılı bir Xamarin.Mac uygulamasında kaynak listeleriyle çalışmak göz duruma getirdi. Oluşturma ve kaynak listesi Xcode'un arabirim oluşturucu içinde korumak ve C# kodunda kaynak listeleri ile çalışma konusunda gördük.

## <a name="related-links"></a>İlgili bağlantılar

- [MacOutlines (örnek)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Tablo Görünümleri](~/mac/user-interface/table-view.md)
- [Anahat Görünümleri](~/mac/user-interface/outline-view.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Anahat görünümleri giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
