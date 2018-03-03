---
title: Kaynak listeleri
description: "Bu makalede Xamarin.Mac uygulamasında kaynak listeleriyle çalışma kapsar. Oluşturma ve kaynak listelerinde Xcode arabirimi Oluşturucu Bakımı ve bunlarla C# kodunda etkileşim açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1cc74fb30e59ecd5f6be3cf3e1c84f60cd5ca0a6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="source-lists"></a>Kaynak listeleri

_Bu makalede Xamarin.Mac uygulamasında kaynak listeleriyle çalışma kapsar. Oluşturma ve kaynak listelerinde Xcode arabirimi Oluşturucu Bakımı ve bunlarla C# kodunda etkileşim açıklar._

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı erişiminiz kaynak listeler, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve kaynak listelerinin bakımını (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

Bir kaynak listesi anahat Bulucu ya da iTunes yan çubuğunda gibi bir eylem, kaynak göstermek için kullanılan görünüm özel bir türde değil.

[ ![](source-list-images/source05.png "Bir örnek kaynağı listesi")](source-list-images/source05.png)

Bu makalede, sizi kaynak listeleriyle Xamarin.Mac uygulamada çalışma temellerini ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>Kaynak listeleri giriş

Yukarıda belirtildiği gibi bir kaynak listesi anahat görünümü özel bir tür Bulucu ya da iTunes yan çubuğunda gibi bir eylem, kaynak göstermek için kullanılır. Kaynak verir tablo türü genişletme veya hiyerarşik veri satırı daraltma listesidir. Bir tablo görünümü aksine bir kaynak listedeki öğeleri düz bir liste, dosyaları ve klasörleri sabit sürücü gibi bir hiyerarşide düzenlenir. Bir kaynak listesindeki bir öğeyi başka öğeler varsa, genişletilmiş veya kullanıcı tarafından daraltılmış.

Kaynak listesi bir özel stilde anahat görünümdür (`NSOutlineView`), kendisi bir tablo görünümü sınıfıdır (`NSTableView`) ve bu nedenle, davranışını çoğunu kendi üst sınıfından devralıyor. Sonuç olarak, anahat görünümü tarafından desteklenen birçok işlemler de desteklenir göre bir kaynak listesi. Xamarin.Mac uygulama bu özelliklerin denetleyebilir ve kaynak listenin parametreleri (ya da kod veya arabirim Oluşturucu) izin vermek veya belirli işlemlerin engellemek için yapılandırabilirsiniz.

Bir kaynak listesi kendi veri depolamaz, bunun yerine bir veri kaynağında kullanır (`NSOutlineViewDataSource`) satır ve sütunları, gerektiği ölçüde temeline üzerinde gerekli sağlamak için.

Anahat görünümü temsilci öğesinin bir alt sağlayarak kaynak listenin davranışı özelleştirilebilir (`NSOutlineViewDelegate`) işlevselliği seçmek için anahat türünü desteklemek üzere Seçim ve düzenleme, özel izleme ve tek tek öğelerin özel görünümler öğesi.

Bir tablo görünümü ve bir anahat görünümü kaynak listesini, davranış ve işlevsellik çoğunu paylaşır olduğundan, üzerinden geçmek isteyebilirsiniz bizim [tablosu görünümleri](~/mac/user-interface/table-view.md) ve [anahat görünümleri](~/mac/user-interface/outline-view.md) devam etmeden önce belgeleri Bu makale ile.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>Kaynak listeleri ile çalışma

Bir kaynak listesi anahat Bulucu ya da iTunes yan çubuğunda gibi bir eylem, kaynak göstermek için kullanılan görünüm özel bir türde değil. Bizim kaynağı listesi arabirimi Oluşturucusu'nda tanımlarız önce Anahat görünümlerin tersine destekleyen sınıfları içinde Xamarin.Mac oluşturalım.

İlk olarak, yeni bir oluşturalım `SourceListItem` verileri için bizim kaynak listesini tutmak için sınıf. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `SourceListItem` için **adı** tıklatıp **yeni** düğmesi:

[ ![](source-list-images/source01.png "Boş bir sınıf ekleme")](source-list-images/source01.png)

Olun `SourceListItem.cs` aşağıdaki gibi dosya bakın: 

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

İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `SourceListDataSource` için **adı** tıklatıp **yeni** düğmesi. Olun `SourceListDataSource.cs` aşağıdaki gibi dosya bakın:

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

Bu veriler için bizim kaynak listesini sağlar.

İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `SourceListDelegate` için **adı** tıklatıp **yeni** düğmesi. Olun `SourceListDelegate.cs` aşağıdaki gibi dosya bakın:

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

Bu bizim kaynağı listesi davranışını sağlayacaktır.

Son olarak, içinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** Seçin **genel** > **boş sınıfı**, girin `SourceListView` için **adı** tıklatıp **yeni** düğmesi. Olun `SourceListView.cs` aşağıdaki gibi dosya bakın:

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

Bu, yeniden kullanılabilir, özel bir alt oluşturur `NSOutlineView` (`SourceListView`), kaynak listesi vermiyoruz herhangi bir Xamarin.Mac uygulamada sürücü kullanırız.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Oluşturma ve kaynak listeleri xcode'da koruma

Şimdi, şimdi bizim arabirimi Oluşturucu kaynak listesinde tasarlayın. Çift `Main.storyboard` arabirimi Oluşturucusu'nda düzenlemek için açın ve bir bölme görünümden sürükleyin dosyaya **kitaplığı denetçisi**, görünüm denetleyiciye ekleyin ve görünümünde yeniden boyutlandırmak için ayarlanmış **Kısıtlamaları Düzenleyicisi** :

[ ![](source-list-images/source00.png "Kısıtlamaları düzenleme")](source-list-images/source00.png)

Ardından, bir kaynak listesinden sürükleyin **kitaplığı denetçisi**Bölünmüş Görünüm sol tarafına ekleyin ve görünümünde yeniden boyutlandırmak için ayarlanmış **Kısıtlamaları Düzenleyicisi**:

[ ![](source-list-images/source02.png "Kısıtlamaları düzenleme")](source-list-images/source02.png)

Ardından, geçiş **kimlik Görünüm**kaynak listesi seçin ve değiştirin 's **sınıfı** için `SourceListView`:

[ ![](source-list-images/source03.png "Sınıf adı ayarlama")](source-list-images/source03.png)

Son olarak, oluşturun bir **çıkışı** bizim kaynağı listesi adlı için `SourceList` içinde `ViewController.h` dosyası:

[ ![](source-list-images/source04.png "Prizine yapılandırma")](source-list-images/source04.png)

Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>Kaynak listesi doldurma

Düzenleyelim `RotationWindow.cs` Mac için Visual Studio içindeki dosya ve hale 's `AwakeFromNib` yöntemi görünüm aşağıdaki gibi:

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

`Initialize ()` Yöntemi bizim kaynak listenin karşı çağrılması gereken **çıkışı** _önce_ tüm öğeler için eklenir. Her grup için öğesi, bir üst öğesi oluşturun ve ardından alt öğeleri bu Grup öğesine ekleyin. Her Grup sonra kaynak listenin koleksiyonuna eklenen `SourceList.AddItem (...)`. Son iki satır için kaynak listesini verileri yüklemek ve tüm grupları genişletir:

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

Son olarak, düzenleme `AppDelegate.cs` dosya ve olun `DidFinishLaunching` yöntemi görünüm aşağıdaki gibi:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

Biz uygulamamızı çalıştırmak, aşağıdaki görüntülenir:

[ ![](source-list-images/source05.png "Bir örnek uygulamayı çalıştırma")](source-list-images/source05.png)

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, kaynak listeleriyle çalışma Xamarin.Mac uygulamada ayrıntılı bir bakış sürdü. Oluşturma ve kaynak listelerinin Xcode'nın arabirimi Oluşturucu bakımını ve C# kodunda kaynak listeleri ile çalışmak nasıl gördük.

## <a name="related-links"></a>İlgili bağlantılar

- [MacOutlines (örnek)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Merhaba, Mac](~/mac/get-started/hello-mac.md)
- [Tablo görünümleri](~/mac/user-interface/table-view.md)
- [Anahat görünümleri](~/mac/user-interface/outline-view.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Anahat görünümlerinde giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
