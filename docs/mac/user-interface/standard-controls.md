---
title: Standart denetimler
description: Bu makalede düğmeleri, etiketler, metin alanlarına, onay kutuları gibi standart AppKit denetimlerle çalışma kapsayan ve Xamarin.Mac uygulama denetimlerinde bölümlenmiş. Arabirim Oluşturucu arabirimiyle ekleme ve bunlarla kodda etkileşim açıklar.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3fe155508b60cbe502c3beca58426528d6f49c9d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="standard-controls"></a>Standart denetimler

_Bu makalede düğmeleri, etiketler, metin alanlarına, onay kutuları gibi standart AppKit denetimlerle çalışma kapsayan ve Xamarin.Mac uygulama denetimlerinde bölümlenmiş. Arabirim Oluşturucu arabirimiyle ekleme ve bunlarla kodda etkileşim açıklar._

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı erişiminiz AppKit denetimleri, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve Appkit denetimlerinizi korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

AppKit denetimleri Xamarin.Mac uygulamanızın kullanıcı arabirimi oluşturmak için kullanılan kullanıcı Arabirimi öğeleri bulunur. Bunlar düğmeleri, etiketler, metin alanlarına, onay kutularını ve bölümlenmiş denetimleri gibi oluşur ve bir kullanıcının onları yönettiğinde anlık eylemler veya görünür sonuçlara neden olabilir.

[![](standard-controls-images/intro01.png "Örnek Uygulama Ana Ekran")](standard-controls-images/intro01.png#lightbox)

Bu makalede, sizi bir Xamarin.Mac uygulamasında AppKit denetimleri ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Denetimleri ve görünümler giriş

macOS (eski adıyla Mac OS X) standart bir kullanıcı arabirimi denetimlerini AppKit Framework aracılığıyla kümesi sağlar. Bunlar düğmeleri, etiketler, metin alanlarına, onay kutularını ve bölümlenmiş denetimleri gibi oluşur ve bir kullanıcının onları yönettiğinde anlık eylemler veya görünür sonuçlara neden olabilir.

Tüm AppKit denetimlerinin birçok kullanım için uygun olacak standart, yerleşik bir görünüme sahip, bazı bir pencere çerçevesi alanı veya kullanım için alternatif bir görünümünü belirtmek bir _Titreşim etkisi_ bağlamı, böyle bir kenar çubuğu alanı olduğu gibi veya içinde bir Bildirim merkezi pencere öğesi.

Apple AppKit denetimleri ile çalışırken, aşağıdaki yönergeleri öneririz:

- Denetim boyutları aynı görünümünde karıştırma kaçının.
- Genel olarak, denetimleri dikey yeniden boyutlandırma kaçının.
- Sistem yazı tipi ve bir denetim içindeki uygun metin boyutu kullanın.
- Denetimler arasındaki uygun aralığı kullanın.

Daha fazla bilgi için pleas bakın [hakkında denetimleri ve görünümler](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Bir pencere çerçevesi denetimlerini kullanma

Pencerenin çerçeve alanında dahil edilecek öğeleri olanak veren bir görüntüle stil içeren AppKit denetimleri kümesini vardır. Örneğin, posta uygulamanın araç bakın:

[![](standard-controls-images/mailapp.png "Mac pencere çerçevesi")](standard-controls-images/mailapp.png#lightbox)

- **Yuvarlamak doku düğmesi** - A `NSButton` stili ile `NSTexturedRoundedBezelStyle`.
- **Yuvarlanmış bölümlenmiş denetim doku** - A `NSSegmentedControl` stili ile `NSSegmentStyleTexturedRounded`.
- **Yuvarlanmış bölümlenmiş denetim doku** - A `NSSegmentedControl` stili ile `NSSegmentStyleSeparated`.
- **Yuvarlamak Doku açılır menüsünden** - A `NSPopUpButton` stili ile `NSTexturedRoundedBezelStyle`.
- **Yuvarlamak Doku açılır menü** - A `NSPopUpButton` stili ile `NSTexturedRoundedBezelStyle`.
- **Arama çubuğunu** - A `NSSearchField`.

Apple pencere çerçevesi AppKit denetimleri ile çalışırken, aşağıdaki yönergeleri öneririz:

- Pencere çerçevesi belirli denetim stilleri penceresi gövdesinde kullanmayın.
- Pencere gövde denetimleri veya stilleri penceresi çerçevede kullanmayın.

Daha fazla bilgi için pleas bakın [hakkında denetimleri ve görünümler](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Arabirim Oluşturucusu'nda bir kullanıcı arabirimi oluşturma

Yeni bir Xamarin.Mac Cocoa uygulaması oluşturduğunuzda, varsayılan olarak standart boş, bir pencere alın. Bu windows tanımlanmış bir `.storyboard` otomatik olarak projeye dahil dosyası. Windows tasarımınızı düzenlemek için **Çözüm Gezgini**, çift tıklayarak `Main.storyboard` dosyası:

[![](standard-controls-images/edit01.png "Çözüm Gezgini'nde ana film şeridi seçme")](standard-controls-images/edit01.png#lightbox)

Bu pencere tasarım Xcode'nın arabirimi Oluşturucusu'nda açın:

[![](standard-controls-images/edit02.png "Xcode'da film şeridi düzenleme")](standard-controls-images/edit02.png#lightbox)

Kullanıcı arabirimi oluşturmak için kullanıcı Arabirimi öğeleri (AppKit denetimleri) sürüklediğiniz **kitaplığı denetçisi** için **arabirimi Düzenleyicisi** arabirimi Oluşturucu. Aşağıdaki örnekte bir **Dikey bölme Görünüm** denetim gelen Uyuşturucu süredir **kitaplığı denetçisi** ve penceresinde getirilen **arabirimi Düzenleyicisi**:

[![](standard-controls-images/edit03.png "Kitaplıktan bir bölünmüş görünümlü seçme")](standard-controls-images/edit03.png#lightbox)

Arabirim Oluşturucusu'nda bir kullanıcı arabirimi oluşturma hakkında daha fazla bilgi için lütfen bkz bizim [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) belgeleri.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Boyutlandırma ve konumlandırma

Kullanıcı arabiriminde bir denetim eklenmiştir sonra kullanarak **kısıtlaması Düzenleyicisi** değerlerini el ile girerek konumunu ve boyutunu ayarlama ve nasıl denetimi otomatik olarak yerleştirilir ve ne zaman boyutta denetlemek için ana penceresi veya Görünüm boyutlandırılır:

[![](standard-controls-images/edit04.png "Kısıtlamalar ayarlama")](standard-controls-images/edit04.png#lightbox)

Kullanım **kırmızı t-kirişleri** dışına geçici **Autoresizing** kutusunu için _çubuğu_ (x, y) belirli bir konuma bir denetim. Örneğin: 

[![](standard-controls-images/edit05.png "Bir kısıtlama düzenleme")](standard-controls-images/edit05.png#lightbox)

Belirleyen Seçili denetimi (içinde **hiyerarşisi görünümü** & **arabirimi Düzenleyicisi**) yeniden boyutlandırılmış veya taşındığında gibi penceresinin veya Görünüm üst ve sağ konuma takılmış. 

Diğer öğeleri yükseklik ve genişlik gibi Düzenleyicisi denetim özellikleri:

[![](standard-controls-images/edit06.png "Yükseklik ayarlama")](standard-controls-images/edit06.png#lightbox)

Öğelerin hizalamasını kullanarak kısıtlamalarıyla denetleyebilirsiniz **hizalama Düzenleyicisi**:

[![](standard-controls-images/edit07.png "Hizalama Düzenleyicisi")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> İOS aksine nerede (0,0) üst olduğu macOS (0,0) içinde ekranın sol alt köşesindeki sol alt köşesindeki olduğu. MacOS yukarı ve sağa değerini artırmayı sayı değerleri içeren bir matematik koordinat sistemi kullandığından budur. Bu kullanıcı arabiriminde AppKit denetimleri yerleştirirken göz önünde bulundurmanız gerekir.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Özel bir sınıf ayarlama

Ne zaman AppKit denetimleri ile çalışmakta subclass ve var olan denetim gerekir ve bu sınıf, kendi özel sürümünü oluşturmak zamanları vardır. Örneğin, özel bir kaynak listesi sürümünü tanımlama:

```csharp
using System;
using AppKit;
using Foundation;

namespace AppKit
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

Burada `[Register("SourceListView")]` yönerge çıkarır `SourceListView` Objective-C, böylece sınıfına arabirimi Oluşturucu'da kullanılabilir. Daha fazla bilgi için lütfen bkz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) belge, açıklar `Register` ve `Export` kullanılan komutlar kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için.

Yerinde yukarıdaki kodu ile genişletme, temel türün bir AppKit denetimi tasarım yüzeyine sürükleyebilirsiniz (aşağıdaki örnekte bir **kaynağı listesi**), geçiş **kimlik denetçisi** ve ayarlayın **özel sınıf** Objective-C için kullanıma sunulan adına (örneğin `SourceListView`):

[![](standard-controls-images/edit10.png "Özel bir sınıf Xcode'da ayarlama")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Çıkışlar ve eylemleri gösterme

C# kodunda AppKit denetim erişilmeden önce olarak açığa çıkarılması gereken bir **çıkışı** veya ve **eylem**. Bunu yapmak için seçin belirli bir denetim ya da **arabirimi hiyerarşi** veya **arabirimi Düzenleyicisi** ve geçiş **Yardımcısı görünümü** ( sahipolduğundaneminolun`.h`düzenleme için seçili pencerenizin):

[![](standard-controls-images/edit11.png "Düzenlemek için doğru dosya seçme")](standard-controls-images/edit11.png#lightbox)

Control-sürükleyin verin üzerine AppKit denetiminden `.h` oluşturmaya başlamak için dosyayı bir **çıkışı** veya **eylem**:

[![](standard-controls-images/edit12.png "Bir çıkış veya eylem oluşturmak için sürükleme")](standard-controls-images/edit12.png#lightbox)

Etkilenme oluşturmak ve vermek için seçin **çıkışı** veya **eylem** bir **adı**: 

[![](standard-controls-images/edit13.png "Eylem ve çıkış yapılandırma")](standard-controls-images/edit13.png#lightbox)


İle çalışma hakkında daha fazla bilgi için **çıkışlar** ve **Eylemler**, lütfen bkz [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) bölümünü bizim [Xcode ve arabirim giriş Oluşturucu](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) belgeleri.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Xcode ile değişiklikler eşitleniyor

Xcode Mac için Visual Studio'ya geri döndüğünüzde, Xcode'da yaptığınız tüm değişiklikler Xamarin.Mac projenizi ile otomatik olarak eşitlenir.

Seçerseniz `SplitViewController.designer.cs` içinde **Çözüm Gezgini** görmeye devam nasıl, **çıkışı** ve **eylem** yukarı bizim C# kodunda Kablolu:

[![](standard-controls-images/sync01.png "Xcode ile değişiklikler eşitleniyor")](standard-controls-images/sync01.png#lightbox)

Bildirim nasıl tanımında `SplitViewController.designer.cs` dosyası:

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

İçinde tanımıyla birlikte satır yukarı `MainWindow.h` Xcode dosyasında:

```csharp
@interface SplitViewController : NSSplitViewController {
    NSSplitViewItem *_LeftController;
    NSSplitViewItem *_RightController;
    NSSplitView *_SplitView;
}

@property (nonatomic, retain) IBOutlet NSSplitViewItem *LeftController;

@property (nonatomic, retain) IBOutlet NSSplitViewItem *RightController;

@property (nonatomic, retain) IBOutlet NSSplitView *SplitView;
```

Gördüğünüz gibi Mac için Visual Studio değişiklikleri dinler `.h` dosya ve ilgili değişiklikleri otomatik olarak eşitleyen `.designer.cs` uygulamanıza göstermek için dosya. Ayrıca, fark edebilirsiniz `SplitViewController.designer.cs` bir parçalı sınıf, böylelikle Mac için Visual Studio değiştirmek yok `SplitViewController.cs ` hangi sınıfa yapmış olduğunuz değişiklikleri üzerine.

Normalde hiçbir zaman açmanız gerekecek `SplitViewController.designer.cs` yalnızca eğitim amacıyla kendiniz burada verildi.

> [!IMPORTANT]
> Çoğu durumda, Mac için Visual Studio otomatik olarak Xcode'da yapılan değişiklikleri görmek ve bunları Xamarin.Mac projenizi eşitleyin. Eşitleme otomatik olarak gerçekleşmez kapalı örneği içinde geri Xcode ve bunları Visual Studio'ya geri Mac için yeniden geçin. Bu, normalde bir eşitleme döngüsü tetiklersiniz.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Düğmeleri ile çalışma

Kullanıcı arabirimi tasarımınızda kullanılabilir düğmesi çeşitli türlerde AppKit sağlar. Daha fazla bilgi için lütfen bkz [düğmeleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons01.png "Farklı bir düğmeyi türleri örneği")](standard-controls-images/buttons01.png#lightbox)

Bir düğme üzerinden kullanıma varsa bir **çıkışı**, aşağıdaki kodu basılan kendisine yanıt verir:

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

Aracılığıyla sunulan düğmelerinin **Eylemler**, `public partial` yöntemi otomatik olarak oluşturulacak sizin için Xcode'da seçtiğiniz ada sahip. Yanıt verecek şekilde **eylem**, kısmi yöntemi sınıfında tamamlamak, **eylem** üzerinde tanımlandı. Örneğin:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

Bir duruma sahip düğmelerinin (gibi **üzerinde** ve **kapalı**), durum kullanıma veya kümesiyle `State` karşı özelliği `NSCellStateValue` enum. Örneğin:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Burada `NSCellStateValue` olabilir:

- **Üzerinde** - düğmesi gönderilen veya denetimi (örneğin, bir onay kutusuna onay) seçilidir.
- **Devre dışı** - düğmesi değil gönderilir veya denetim seçilmedi.
- **Karma** -bir karışımını **üzerinde** ve **kapalı** durumları.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Düğme varsayılan ve anahtar eşdeğer bir gruba ayarlamak gibi işaretle

Bir kullanıcı arabirimi tasarımı eklediğiniz herhangi bir düğmesini için bu düğme olarak işaretleyebilirsiniz _varsayılan_ kullanıcı bastığında etkinleştirilir düğmesi **Return/Enter** klavyedeki anahtar. MacOS içinde bu düğme mavi arka plan rengi varsayılan olarak alır.

Varsayılan olarak bir düğme ayarlamak için Xcode'nın arabirimi Oluşturucusu'nda seçin. İleri ' **özniteliği denetçisi**seçin **anahtar eşdeğer** alan ve tuşuna **Return/Enter** anahtarı:

[![](standard-controls-images/buttons03.png "Anahtar eşdeğer düzenleme")](standard-controls-images/buttons03.png#lightbox)

Eşit oranda, fare yerine klavyeyi kullanarak düğmesi etkinleştirmek için kullanılan anahtar sırası atayabilirsiniz. Örneğin, göre komutu-C yukarıdaki resimde tuşlarına.

Ne zaman uygulamayı çalıştırın ve pencerenin düğmesiyle anahtarıdır ve kullanıcı komutu-C basarsa odaklanmış, eylem düğmesi için etkinleştirilecek (olarak-kullanıcı düğmesine tıkladıysanız).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Radyo düğmeleri ve onay kutuları ile çalışma

AppKit onay kutularını ve kullanıcı arabirimi tasarımınızda kullanılabilmesi için bir radyo düğmesi gruplarına çeşitli türleri sağlar. Daha fazla bilgi için lütfen bkz [düğmeleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons02.png "Kullanılabilir onay kutusu türleri örneği")](standard-controls-images/buttons02.png#lightbox)


Onay kutuları ve radyo düğmeleri (aracılığıyla kullanıma sunulan **çıkışlar**) bir duruma sahip (gibi **üzerinde** ve **devre dışı**), durum kullanıma veya kümesiyle `State` karşı özelliği `NSCellStateValue` enum. Örneğin:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Burada `NSCellStateValue` olabilir:

- **Üzerinde** - düğmesi gönderilen veya denetimi (örneğin, bir onay kutusuna onay) seçilidir.
- **Devre dışı** - düğmesi değil gönderilir veya denetim seçilmedi.
- **Karma** -bir karışımını **üzerinde** ve **kapalı** durumları.

Radyo düğmesi grubunda bir düğme seçmek için olarak seçilecek radyo düğmesi kullanıma bir **çıkışı** ve kendi `State` özelliği. Örneğin:

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Bir grup olarak davranır ve otomatik olarak seçilen durumu işlemek için radyo düğmeleri koleksiyonu almak için yeni bir oluşturma **eylem** ve gruptaki her düğmesi eklemektir:

![](standard-controls-images/buttons04.png "Yeni bir eylem oluşturma")

Ardından, benzersiz bir Ata `Tag` her radyo düğmesine **özniteliği denetçisi**:

![](standard-controls-images/buttons05.png "Radyo düğmesi etiketi düzenleme")

Değişikliklerinizi kaydetmek ve Mac için Visual Studio'ya geri dönün, işlemek üzere kod eklemek **eylem** tüm radyo düğmeleri için eklenen:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Kullanabileceğiniz `Tag` hangi radyo düğmesinin seçili görmek için özellik.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Menü denetimleri ile çalışma

AppKit çeşitli kullanıcı arabirimi tasarımınızda kullanılabilir menü denetimleri sağlar. Daha fazla bilgi için lütfen bkz [menü denetimleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/menu01.png "Örnek menü denetimleri")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Menü denetim verileri sağlayan

MacOS için kullanılabilir menü denetimleri listeden (arabirimi Oluşturucusu'nda önceden tanımlanamıyor veya kodu aracılığıyla doldurulmuş) bir iç ya da aşağı açılan listeyi doldurmak için ayarlayabilirsiniz veya kendi özel, dış veri kaynağı sağlayarak.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>İç verileri ile çalışma

Menü denetimleri arabirimi Oluşturucusu'nda, öğeleri tanımlama yanı sıra (gibi `NSComboBox`), eklemek, düzenlemek veya bunların bakımını iç listeden öğeleri silmek olanak sağlayan yöntemleri eksiksiz bir kümesini sağlar:

- `Add` -Yeni bir öğe listesinin sonuna ekler.
- `GetItem` -Belirtilen dizinindeki öğeyi döndürür.
- `Insert` -Listesinde belirtilen konumda yeni bir öğe ekler.
- `IndexOf` -Belirtilen öğenin dizinini döndürür.
- `Remove` -Belirtilen öğeyi listeden kaldırır.
- `RemoveAll` -Tüm öğeleri listeden kaldırır.
- `RemoveAt` -Belirtilen dizinindeki öğeyi kaldırır.
- `Count` -Listedeki öğe sayısını döndürür.

> [!IMPORTANT]
> Bir dış veri kaynağına kullanıyorsanız (`UsesDataSource = true`), yukarıdaki yöntemlerden herhangi birini çağıran bir özel durum oluşturur.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Dış veri kaynağı ile çalışma

Menü denetimi için satırları sağlamak için yerleşik iç veri kullanmak yerine, isteğe bağlı olarak bir dış veri kaynağına kullanın ve öğeleri (örneğin, bir SQLite veritabanı) için kendi yedekleme deposu sağlayın.

Bir dış veri kaynağı ile çalışmak için menü denetimin veri kaynağı örneği oluşturacaksınız (`NSComboBoxDataSource` gibi) ve gerekli verileri sağlamak için çeşitli yöntemler geçersiz kılın:

- `ItemCount` -Listedeki öğe sayısını döndürür.
- `ObjectValueForItem` -Belirli bir dizine öğenin değerini döndürür.
- `IndexOfItem` -Dizini verin öğe değerini döndürür.
- `CompletedString` -Kısmen yazılan öğeyi değerin ilk eşleşen öğe değerini döndürür. Otomatik Tamamlama etkinse bu yöntem yalnızca çağrılır (`Completes = true`).

Lütfen bakın [veritabanları ve ComboBoxes](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) bölümünü [veritabanlarıyla çalışma](~/mac/app-fundamentals/databases.md) daha fazla ayrıntı için belge.

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>List öğesinin görünümünü ayarlama

Menü denetimin görünümünü ayarlamak aşağıdaki yöntemleri kullanılabilir:

- `HasVerticalScroller` -İf `true`, denetimi dikey kaydırma çubuğu görüntülenir. 
- `VisibleItems` -Denetim açıldığında görüntülenen öğelerin sayısını ayarlayın. Varsayılan değer beş (5).
- `IntercellSpacing` -Belirli bir öğe boşluk miktarını sağlayarak ayarlamak bir `NSSize` nerede `Width` sol ve sağ kenar boşluklarını belirtir ve `Height` önce ve sonra bir öğe alanını belirtir.
- `ItemHeight` -Listedeki her öğenin yüksekliğini belirtir.

Aşağı açılan türleri için `NSPopupButtons`, ilk menü öğesi denetim için bir başlık sağlar. Örneğin: 

[![](standard-controls-images/menu02.png "Bir örnek menü denetimi")](standard-controls-images/menu02.png#lightbox)

Başlık değiştirmek için bu öğeyi olarak kullanıma bir **çıkışı** ve kod aşağıdaki gibi kullanın:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Seçili öğeleri düzenleme

Aşağıdaki yöntemleri ve özellikleri menü denetimin listesinde seçili öğeleri değiştirmenize izin ver:

- `SelectItem` -Belirtilen dizinindeki öğeyi seçer.
- `Select` -Belirtilen öğe değeri seçin.
- `DeselectItem` -Belirtilen dizinindeki öğeyi seçimini kaldırır.
- `SelectedIndex` -Seçili öğenin dizinini döndürür.
- `SelectedValue` -Seçili öğenin değerini döndürür.

Kullanım `ScrollItemAtIndexToTop` listenin başındaki verilen dizindeki öğeyi sunmak için ve `ScrollItemAtIndexToVisible` verilen dizindeki öğesi görünene kadar listesine gidin.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Olaylara yanıt verme

Menü denetimleri için kullanıcı etkileşimi yanıt aşağıdaki olaylar sağlar:

- `SelectionChanged` -Kullanıcı listesinden bir değer seçtiğinde adı verilir.
- `SelectionIsChanging` -Yeni kullanıcı seçili öğesi etkin seçim haline gelir önce adı verilir.
- `WillPopup` -Açılır liste öğelerinin görüntülenmeden önce adı verilir.
- `WillDismiss` -Açılan liste öğelerinin kapatılmadan önce adı verilir.

İçin `NSComboBox` denetimleri içerirler tüm aynı olaylar `NSTextField`, gibi `Changed` birleşik giriş kutusu metinde değerini kullanıcı düzenlemeleri olduğunda çağrılan olay.

İsteğe bağlı olarak, yanıt arabirimi öğesine ekleyerek seçilmesini Oluşturucusu'nda bir iç veri menü öğeleri tanımlı bir **eylem** ve yanıt verecek şekilde aşağıdaki gibi bir kod kullanın **eylem** bırakılıyor kullanıcı tarafından tetiklenen:

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Menüler ve menü denetimleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [menüleri](~/mac/user-interface/menu.md) ve [açılır düğmesi ve aşağı açılır listeler](~/mac/user-interface/menu.md) belgeleri.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Seçim denetimleri ile çalışma

AppKit çeşitli kullanıcı arabirimi tasarımınızda kullanılabilir seçim denetimleri sağlar. Daha fazla bilgi için lütfen bkz [seçim denetimleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/select01.png "Örnek seçim denetimleri")](standard-controls-images/select01.png#lightbox)

Seçim denetimi olarak göstererek kullanıcı etkileşimi olduğunda izlemek için iki yolla bir **eylem**. Örneğin:

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

Ya da ekleyerek bir **temsilci** için `Activated` olay. Örneğin:

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

Ayarlayın ya da seçim denetiminin değerini okumak için kullanın `IntValue` özelliği. Örneğin:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

Özel denetimler (örneğin, iyi rengi ve resim iyi) kendi değer türleri için belirli özelliklere sahiptir. Örneğin:

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

`NSDatePicker` Doğrudan tarih ve saat ile çalışmak için aşağıdaki özelliklere sahiptir:

- **TARİHSAYISI** -geçerli tarih ve saat değeri olarak bir `NSDate`.
- **Yerel** -kullanıcının konumu olarak bir `NSLocal`.
- **TimeInterval** -saat değeri olarak bir `Double`.
- **Saat dilimi** -kullanıcının saat dilimi olarak bir `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Gösterge denetimleriyle çalışma

AppKit çeşitli kullanıcı arabirimi tasarımınızda kullanılabilir göstergesi denetimleri sağlar. Daha fazla bilgi için lütfen bkz [göstergesi denetimleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/level01.png "Örnek göstergesi denetimleri")](standard-controls-images/level01.png#lightbox)

Bir gösterge denetimi olarak gösterme ya da kullanıcı etkileşimi olduğunda izlemek için iki yolla bir **eylem** veya bir **çıkışı** ve ekleyerek bir **temsilci** için`Activated`olay. Örneğin:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Okumak veya gösterge denetim değerini ayarlamak için kullanın `DoubleValue` özelliği. Örneğin:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Belirsiz ve zaman uyumsuz İlerleme göstergesi görüntülendiğinde animasyon. Kullanım `StartAnimation` görüntülendikleri animasyonun başlattığınızda yöntemi. Örneğin:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Çağırma `StopAnimation` yöntemi animasyonun durdurur.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Metin denetimleri ile çalışma

AppKit çeşitli kullanıcı arabirimi tasarımınızda kullanılabilir metin denetimleri sağlar. Daha fazla bilgi için lütfen bkz [metin denetimleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/text01.png "Örnek metin denetimleri")](standard-controls-images/text01.png#lightbox)

Metin alanları için (`NSTextField`), aşağıdaki olaylar, kullanıcı etkileşimi izlemek için kullanılabilir:

- **Değiştirilen** -kullanıcı değişiklikleri alanının değeri her zaman tetiklenir. Örneğin, her karakter belirtilmiş.
- **EditingBegan** -kullanıcı düzenleme için alan seçtiğinde tetiklenir.
- **EditingEnded** - kullanıcı alanında Enter tuşuna bastığında ya da alan bırakır.

Kullanım `StringValue` okuma veya alanın değerini ayarlamak için özellik. Örneğin:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

Görüntülemek veya sayısal değerleri düzenlemek alanlar için kullandığınız `IntValue` özelliği. Örneğin:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

Bir `NSTextView` tam özellikli bir metin sağlar düzenleyin ve yerleşik biçimlendirmeye sahip alanı görüntüler. Gibi bir `NSTextField`, kullanın `StringValue` okumak veya alanın değerini ayarlamak için özellik.

Örneği Xamarin.Mac uygulama metin görünümlerle çalışma karmaşık bir örnek için lütfen bkz [SourceWriter örnek uygulaması](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter kod tamamlama ve basit sözdizimi vurgulama için destek sağlayan bir basit kaynak kod düzenleyicisidir.

SourceWriter kodu tam olarak geçersiz kılınan ve kullanılabilir olduğunda, bağlantılar sahip sağlanmalı temel teknolojileri veya yöntemlerini Xamarin.Mac kılavuzları belgelerinde ilgili bilgilere.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>İçerik görünümlerle çalışma

AppKit birkaç kullanıcı arabirimi tasarımınızda kullanılabilir içerik görünüm türlerini sağlar. Daha fazla bilgi için lütfen bkz [içerik görünümleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[![](standard-controls-images/content01.png "Bir örnek içerik görünümü")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

Bir popover doğrudan belirli bir denetim bir ya da ekranda alanla ilgili işlevselliği sunan geçici bir UI öğedir. Denetim veya ilgili olduğu alanı içeren pencerenin üstündeki bir popover gezinen ve kenarlığını kendisinden ortaya çıktı noktasını belirtmek için bir ok içerir.

Bir popover oluşturmak için aşağıdakileri yapın:

1. Açık `.storyboard` dosyasını çift tıklatarak bir popover eklemek istediğiniz penceresinin **Çözüm Gezgini**
2. Sürükleme bir **görüntülemek denetleyicisi** gelen **kitaplığı denetçisi** üzerine **Arayüzü Düzenleyicisi**: 

    [![](standard-controls-images/content02.png "Kitaplıktan bir görünüm denetleyicisini seçme")](standard-controls-images/content02.png#lightbox)
4. Boyut ve yerleşimini tanımlamak **Özel Görünüm**: 

    [![](standard-controls-images/content04.png "Düzen düzenleme")](standard-controls-images/content04.png#lightbox)
5. Denetim tıklayın ve açılan kaynağından sürükleyin **View Controller**: 

    [![](standard-controls-images/content05.png "Bir segue oluşturmak için sürükleme")](standard-controls-images/content05.png#lightbox)
6. Seçin **Popover** açılan menüsünden: 

    [![](standard-controls-images/content06.png "Ayar segue türü")](standard-controls-images/content06.png#lightbox)
7. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

<a name="Tab_Views" />

### <a name="tab-views"></a>Sekme görünümleri

Sekme görünümleri oluşur (görünen bölümlenmiş denetime benzer) olarak adlandırılan görünümleri kümesiyle birlikte bir sekme listesinin _bölmeleri_. Kullanıcı yeni bir sekme seçtiğinde ekli olduğu bölmesinde görüntülenir. Her bölmesi kendi denetimleri kümesini içerir.

Xcode'nın arabirimi Oluşturucu sekmesi görünümü ile çalışırken kullanın **özniteliği denetçisi** sekmeleri sayısını ayarlamak için:

[![](standard-controls-images/content08.png "Sekme sayısı düzenleme")](standard-controls-images/content08.png#lightbox)

Her bir sekmede seçin **arabirimi hiyerarşi** ayarlamak için kendi **başlık** ve kullanıcı Arabirimi öğeleri ekleyin, **bölmesinde**:

[![](standard-controls-images/content09.png "Xcode sekmeleri düzenleme")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>Veri bağlama AppKit denetimleri

Xamarin.Mac uygulamanızda anahtar-değer kodlama ve veri bağlama teknikleri kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız için sahip kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma faydası vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), başında bakımı kolay, daha esnek uygulama için Tasarım.

Anahtar-değer kodlama (KVC) olan bir nesnenin özelliklerine dolaylı olarak erişme, örnek değişkenleri erişim yerine özelliklerini tanımlamak için (özel olarak biçimlendirilmiş dizeler) anahtarları veya erişimci yöntemlerini kullanarak için bir mekanizma (`get/set`). Anahtar-değer kodlama uyumlu erişimcilerini Xamarin.Mac uygulamanızda uygulayarak, anahtar-değer Gözlemleme (KVO), veri bağlama, çekirdek verileri, Cocoa bağlamalar ve scriptability gibi diğer macOS özelliklerine erişim sahibi olursunuz.

Daha fazla bilgi için lütfen bkz [basit veri bağlama](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) bölümünü bizim [verileri bağlama ve anahtar-değer kodlama](~/mac/app-fundamentals/databinding.md) belgeleri.


<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede düğmeleri, etiketler, metin alanlarına, onay kutularını ve bölümlenmiş denetimleri gibi standart AppKit denetimleri Xamarin.Mac uygulamasında ile çalışan bir ayrıntılı bakış sürdü. Bir kullanıcı arabirimi tasarımı Xcode'nın arabirimi Oluşturucu ekleme, bunları çıkışlar ve eylemleri aracılığıyla koda gösterme ve C# kodunda AppKit denetimleriyle çalışma ele.

## <a name="related-links"></a>İlgili bağlantılar

- [MacControls (örnek)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Veri Bağlama ve Anahtar-Değer Kodlaması](~/mac/app-fundamentals/databinding.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Denetimleri ve görünümleri hakkında](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
