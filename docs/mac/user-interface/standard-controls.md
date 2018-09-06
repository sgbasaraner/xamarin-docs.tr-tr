---
title: Xamarin.Mac standart denetimlerinde
description: Bu makalede, düğmeler, etiketler, metin alanları, onay kutuları gibi standart olan tüm AppKit denetimleri ile çalışmayı kapsar ve bölümlenmiş bir Xamarin.Mac uygulamasını denetimleri. Bu, bir arabirim Oluşturucu arabirimiyle ekleyerek ve kodu normalde açıklar.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1760e764c4918f597c95a4c811be9166e33bb1f3
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780674"
---
# <a name="standard-controls-in-xamarinmac"></a>Xamarin.Mac standart denetimlerinde

_Bu makalede, düğmeler, etiketler, metin alanları, onay kutuları gibi standart olan tüm AppKit denetimleri ile çalışmayı kapsar ve bölümlenmiş bir Xamarin.Mac uygulamasını denetimleri. Bu, bir arabirim Oluşturucu arabirimiyle ekleyerek ve kodu normalde açıklar._

Bir Xamarin.Mac uygulamasında çalışırken, C# ve .NET ile aynı erişiminiz olan tüm AppKit denetimleri, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Xcode'un kullanabileceğiniz _arabirim Oluşturucu_ oluşturmak ve, olan tüm Appkit denetimleri korumak (veya isteğe bağlı olarak bunları doğrudan C# kodu oluşturmak için).

Xamarin.Mac uygulamanızın kullanıcı arayüzü oluşturmak için kullanılan kullanıcı Arabirimi öğeleri olan tüm AppKit denetimlerdir. Bunlar, düğmeler, etiketler, metin alanları, onay kutularını ve bölümlenmiş denetimler gibi öğelerin oluşur ve bir kullanıcı bunları yönettiğinde, anlık eylemleri ya da görünür sonuçları neden.

[![](standard-controls-images/intro01.png "Örnek Uygulama Ana Ekran")](standard-controls-images/intro01.png#lightbox)

Bu makalede, biz bir Xamarin.Mac uygulamasında olan tüm AppKit denetimleri ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Denetimleri ve görünümleri giriş

macOS (Mac OS X eski adıyla), standart bir kullanıcı arabirimi denetimleri AppKit Framework aracılığıyla sağlar. Bunlar, düğmeler, etiketler, metin alanları, onay kutularını ve bölümlenmiş denetimler gibi öğelerin oluşur ve bir kullanıcı bunları yönettiğinde, anlık eylemleri ya da görünür sonuçları neden.

Tüm AppKit denetimleri birçok kullanım için uygun olacak standart, yerleşik bir görünüme sahip, bazı alternatif bir görünümü bir pencere çerçevesi alanını veya kullanmak için belirtin bir _Titreşim etkin_ bağlamı, böyle bir kenar alanı olduğu gibi veya bir Bildirim merkezi pencere öğesi.

Apple olan tüm AppKit denetimleri ile çalışırken aşağıdaki yönergeler önerilir:

- Denetim boyutları aynı görünümde karıştırmayın.
- Genel olarak, dikey denetimleri yeniden boyutlandırma kaçının.
- Sistem yazı tipi ve denetim içinde uygun metin boyutunu kullanın.
- Uygun bir aralık arasında denetimleri kullanın.

Daha fazla bilgi için bkz. pleas [ilgili denetimleri ve görünümleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Bir pencere çerçevesinde denetimlerini kullanma

Bir alt pencerenin çerçeve alanına dahil edilecek öğeleri izin veren bir görüntü stili dahil olan tüm AppKit denetimleri vardır. Örneğin, posta uygulamanın araç bakın:

[![](standard-controls-images/mailapp.png "Bir Mac pencere çerçevesi")](standard-controls-images/mailapp.png#lightbox)

- **Yuvarlak dokulu düğmesi** - `NSButton` stili ile `NSTexturedRoundedBezelStyle`.
- **Yuvarlatılmış segmentli denetim dokulu** - `NSSegmentedControl` stili ile `NSSegmentStyleTexturedRounded`.
- **Yuvarlatılmış segmentli denetim dokulu** - `NSSegmentedControl` stili ile `NSSegmentStyleSeparated`.
- **Yuvarlak dokulu açılır menü** - `NSPopUpButton` stili ile `NSTexturedRoundedBezelStyle`.
- **Yuvarlak dokulu açılan menü** - `NSPopUpButton` stili ile `NSTexturedRoundedBezelStyle`.
- **Arama çubuğunu** - `NSSearchField`.

Apple pencere çerçevesi olan tüm AppKit denetimleri ile çalışırken aşağıdaki yönergeler önerilir:

- Pencere çerçevesi belirli denetim stilleri penceresi gövdesinde kullanmayın.
- Pencere gövdesi denetimler pencere çerçevesinde kullanmayın.

Daha fazla bilgi için bkz. pleas [ilgili denetimleri ve görünümleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Arabirimi Oluşturucu'da bir kullanıcı arabirimi oluşturma

Yeni bir Xamarin.Mac Cocoa uygulaması oluşturduğunuzda, varsayılan olarak standart boş bir pencere alın. Bu windows tanımlanmış bir `.storyboard` dosya otomatik olarak projeye dahil. Windows tasarımınızı düzenlemeye **Çözüm Gezgini**, çift tıklayarak `Main.storyboard` dosyası:

[![](standard-controls-images/edit01.png "Çözüm Gezgini'nde ana görsel taslak seçme")](standard-controls-images/edit01.png#lightbox)

Bu pencere tasarım Xcode'un arabirimi Oluşturucu'da açın:

[![](standard-controls-images/edit02.png "Xcode içinde bir film şeridini düzenleme")](standard-controls-images/edit02.png#lightbox)

Kullanıcı arabirimi oluşturmak için kullanıcı Arabirimi öğeleri (olan tüm AppKit denetimleri) sürüklediğiniz **kitaplığı denetçisi** için **Arayüzü Düzenleyicisi** arabirimi Oluşturucu. Aşağıdaki örnekte bir **Dikey bölme Görünüm** denetimi, ilaç gelen oluştu **kitaplığı denetçisi** ve penceresine yerleştirilen **Arayüzü Düzenleyicisi**:

[![](standard-controls-images/edit03.png "Bölünmüş Görünüm Kitaplığı'ndan seçme")](standard-controls-images/edit03.png#lightbox)

Arabirimi Oluşturucu'da bir kullanıcı arabirimi oluşturma hakkında daha fazla bilgi için lütfen bkz. bizim [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) belgeleri.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Boyutlandırma ve konumlandırma

Kullanıcı arabiriminde bir denetim eklenmiştir sonra kullanın **kısıtlaması Düzenleyicisi** değerlerini el ile girerek konumunu ve boyutunu ayarlayın ve denetimin nasıl otomatik olarak konumlandırılmış ve ne zaman boyutu denetlemek için Görünüm ve ana penceresi yeniden boyutlandırıldıktan:

[![](standard-controls-images/edit04.png "Kısıtlamaları ayarlama")](standard-controls-images/edit04.png#lightbox)

Kullanın **kırmızı ben-kirişleri** dışına etrafında **Autoresizing** kutusunun _Sopası_ (x, y) belirli bir konuma bir denetim. Örneğin: 

[![](standard-controls-images/edit05.png "Kısıtlama düzenleme")](standard-controls-images/edit05.png#lightbox)

Belirten Seçili denetimi (içinde **hiyerarşi görünümü** & **Arayüzü Düzenleyicisi**) yeniden boyutlandırılabilir veya taşınmış penceresinin veya görünümü üst ve sağ konumuna takıldı. 

Düzenleyici denetimi özelliklerinin yükseklik ve genişlik gibi diğer öğeleri:

[![](standard-controls-images/edit06.png "Yüksekliği ayarlama")](standard-controls-images/edit06.png#lightbox)

Öğelerin hizalarını kullanarak kısıtlamalarla denetleyebilirsiniz **hizalama Düzenleyicisi**:

[![](standard-controls-images/edit07.png "Hizalama Düzenleyicisi")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> İOS aksine nerede (0,0) büyük olan macOS (0,0), ekranda sol üst köşesinde olan sol alt köşesinde. MacOS yukarı ve sağa doğru değeri artırmayı sayı değerleri matematiksel bir koordinat sistemini kullanıyor olmasıdır. Bir kullanıcı arabirimi olan tüm AppKit denetimleri yerleştirme sırasında bu hesaba katmak gerekir.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Özel bir sınıf ayarlama

Ne zaman olan tüm AppKit denetimleri ile çalışmakta alt ve mevcut denetim gerekir ve bu sınıf, kendi özel sürümünü oluşturmak zamanlar vardır. Örneğin, kaynak listenin, özel bir sürümünü tanımlama:

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

Burada `[Register("SourceListView")]` yönerge kullanıma sunan `SourceListView` sınıfı Objective-C, bu nedenle için arabirim Oluşturucu'da kullanılabilir. Daha fazla bilgi için lütfen bkz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) açıklar, belge `Register` ve `Export` kullanılan komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yukarı için.

Yerinde yukarıdaki kodla genişletilmesi, temel türün bir AppKit denetimi tasarım yüzeyine sürükleyebilirsiniz (aşağıdaki örnekte bir **kaynağı listesi**), geçiş **kimlik denetçisi** ve ayarlayın **özel sınıf** Objective-C için kullanıma sunulan adı (örnek `SourceListView`):

[![](standard-controls-images/edit10.png "Xcode içinde özel bir sınıf ayarlama")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Çıkışlar ve eylemleri gösterme

Olarak açığa ihtiyacı olan tüm AppKit denetim C# kodunda erişilmeden önce bir **çıkışı** veya ve **eylem**. Bunu yapmak için seçin belirli bir denetim ya da **arabirimi hiyerarşi** veya **Arayüzü Düzenleyicisi** geçin **Yardımcısı görünümü** ( sahipolduğunuzdaneminolun`.h`düzenleme için seçili pencerenizin):

[![](standard-controls-images/edit11.png "Düzenlemek için doğru dosya seçme")](standard-controls-images/edit11.png#lightbox)

Denetim Sürükle verin üzerine AppKit denetiminden `.h` oluşturmaya başlamak için dosya bir **çıkışı** veya **eylem**:

[![](standard-controls-images/edit12.png "Bir çıkış veya eylem oluşturmak için sürükleme")](standard-controls-images/edit12.png#lightbox)

Oluşturmak ve vermek için açığa türünü **çıkışı** veya **eylem** bir **adı**: 

[![](standard-controls-images/edit13.png "Çıkış veya eylem yapılandırma")](standard-controls-images/edit13.png#lightbox)


İle çalışma hakkında daha fazla bilgi için **çıkışlar** ve **eylemleri**, lütfen [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) bölümünü bizim [Xcode ve arabirimi giriş Oluşturucu](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) belgeleri.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Xcode ile değişiklikler eşitleniyor

Mac için Visual Studio'ya dönün Xcode'dan geçiş yaptığınızda, Xamarin.Mac projenizle Xcode'da yaptığınız tüm değişiklikler otomatik olarak eşitlenir.

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

Tanımında ile hizalamak `MainWindow.h` Xcode dosyasında:

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

Gördüğünüz gibi değişiklikler için Mac için Visual Studio dinler `.h` dosya ve ilgili değişiklikleri otomatik olarak eşitler `.designer.cs` uygulamanıza gösterilecek dosya. Bu da da bildirebilirsiniz `SplitViewController.designer.cs` Mac için Visual Studio değiştirmek zorunda değildir kısmi bir sınıf olduğunu `SplitViewController.cs ` , sınıfa yaptık tüm değişikliklerin üzerine.

Normalde hiçbir zaman açmak ihtiyacınız `SplitViewController.designer.cs` eğitim amacıyla yalnızca kendiniz burada sunuldu.

> [!IMPORTANT]
> Çoğu durumda, Mac için Visual Studio otomatik olarak Xcode'da yapılan değişiklikleri görmek ve bunları Xamarin.Mac projenize eşitleyebilirsiniz. Eşitleme otomatik olarak gerçekleşmez kapalı örneği, geri Xcode ve bunları Visual Studio'ya dönün Mac için yeniden geçin. Bu başlatır normalde bir eşitleme döngüsü devre dışı.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Düğme ile çalışma

AppKit birden fazla kullanıcı arabirimi tasarımı kullanılan düğmesi sağlar. Daha fazla bilgi için lütfen bkz [düğmeleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons01.png "Farklı bir düğme türlerini örneği")](standard-controls-images/buttons01.png#lightbox)

Bir düğme aracılığıyla sunulan, bir **çıkışı**, aşağıdaki kod basıldığında buna yanıt verir:

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

Aracılığıyla sunulan düğmelerinin **eylemleri**, `public partial` yöntemi otomatik olarak oluşturulur, Xcode'da seçtiğiniz ada sahip. Yanıt vermek için **eylem**, kısmi yöntem sınıfında tamamlamak, **eylem** üzerinde tanımlandı. Örneğin:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

Durumunda olan düğmeler (gibi **üzerinde** ve **kapalı**), durumu işaretli veya kümesi `State` özelliği karşı `NSCellStateValue` sabit listesi. Örneğin:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Burada `NSCellStateValue` olabilir:

- **Üzerinde** - düğmeyi gönderilir veya denetim (örneğin, bir onay kutusu denetiminde) seçili.
- **Kapalı** - düğme değil gönderilir veya denetim seçilmedi.
- **Karma** -bir karışımını **üzerinde** ve **kapalı** durumları.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Bir düğme anahtar eşdeğer ayarlayın ve varsayılan olarak işaretle

Bir kullanıcı arabirimi tasarımı için eklediğiniz tüm düğmesi için bu düğmeyi olarak işaretleyebilirsiniz _varsayılan_ kullanıcının bastığında etkinleştirilecek düğme **dönüş/Enter** klavyedeki anahtar. MacOS, bu düğme mavi arka plan rengini varsayılan olarak alır.

Bir düğme varsayılan olarak ayarlamak için Xcode'un arabirimi Oluşturucu'da seçin. Ardından **özniteliği denetçisi**seçin **anahtar eşdeğer** alan ve ENTER tuşuna **dönüş/Enter** anahtarı:

[![](standard-controls-images/buttons03.png "Anahtar eşdeğer düzenleme")](standard-controls-images/buttons03.png#lightbox)

Eşit şekilde, klavye, fare yerine kullanarak düğmesini etkinleştirmek için kullanılan herhangi bir anahtar serisine atayabilirsiniz. Örneğin, göre Command-C tuşlarına yukarıdaki görüntüde basarak.

Ne zaman uygulamayı çalıştırın ve pencerenin düğmesi anahtarıdır ve kullanıcı komutu C bastığında odaklanmış, düğme için eylem etkinleştirilir (olarak-kullanıcı düğmesine tıkladıysanız).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Radyo düğmeleri ve onay kutularını ile çalışma

AppKit onay kutularını ve kullanıcı arabirimi tasarımı kullanılabilmesi için bir radyo düğmesi gruplarına çeşitli türleri sağlar. Daha fazla bilgi için lütfen bkz [düğmeleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons02.png "Kullanılabilir onay kutusu türleri örneği")](standard-controls-images/buttons02.png#lightbox)


Onay kutularını ve radyo düğmeleri (aracılığıyla kullanıma sunulan **çıkışlar**) bir duruma sahip (gibi **üzerinde** ve **kapalı**), durum işaretli veya kümesi `State` karşı özelliği `NSCellStateValue` sabit listesi. Örneğin:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Burada `NSCellStateValue` olabilir:

- **Üzerinde** - düğmeyi gönderilir veya denetim (örneğin, bir onay kutusu denetiminde) seçili.
- **Kapalı** - düğme değil gönderilir veya denetim seçilmedi.
- **Karma** -bir karışımını **üzerinde** ve **kapalı** durumları.

Bir radyo düğmesi grubunda bir düğme seçmek için olarak seçilecek radyo düğmesini açığa bir **çıkışı** ve kendi `State` özelliği. Örneğin:

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Bir grup olarak davranmanıza ve otomatik olarak seçilen durumu işlemek için radyo düğmelerinden oluşan bir koleksiyonu almak için yeni bir oluşturma **eylem** ve gruptaki her bir düğme ekler:

![](standard-controls-images/buttons04.png "Yeni bir eylem oluşturma")

Ardından, benzersiz bir atama `Tag` her radyo düğmesi için **özniteliği denetçisi**:

![](standard-controls-images/buttons05.png "Bir radyo düğmesi etiketi düzenleme")

Değişikliklerinizi kaydedin ve Mac için Visual Studio'ya geri dönün, işlemek üzere kod eklemek **eylem** tüm radyo düğmeleri için eklenir:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Kullanabileceğiniz `Tag` hangi radyo düğmesini seçili görmek için özellik.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Menü denetimleri ile çalışma

AppKit birden fazla kullanıcı arabirimi tasarımı içinde kullanılabilir menü denetimleri sağlar. Daha fazla bilgi için lütfen bkz [menü denetimlerinin](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/menu01.png "Örnek menü denetimleri")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Menü denetim verileri sağlama

MacOS için kullanılabilir menü denetimleri (, arabirim oluşturucu içinde önceden tanımlanmış veya kod doldurulur) bir iç listesinden ya da açılan listeyi doldurmak için ayarlanabilir veya kendi özel, dış veri kaynağı sağlayarak.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>İç veri ile çalışma

Menü denetimleri, arabirim Oluşturucu öğelerini tanımlama yanı sıra (gibi `NSComboBox`), eksiksiz bir eklemek, düzenlemek veya bunların bakımını yapan iç listeden öğeleri silmek olanak tanıyan yöntemler sağlar:

- `Add` -Yeni bir öğe listesinin sonuna ekler.
- `GetItem` -Belirtilen dizindeki öğeyi döndürür.
- `Insert` -Belirtilen konuma listesinde yeni bir öğe ekler.
- `IndexOf` -Verilen öğenin dizinini döndürür.
- `Remove` -Belirtilen öğeyi listeden kaldırır.
- `RemoveAll` -Listesinden tüm öğeleri kaldırır.
- `RemoveAt` -Belirtilen dizinindeki öğeyi kaldırır.
- `Count` -Listedeki öğe sayısını döndürür.

> [!IMPORTANT]
> Bir dış veri kaynağı kullanıyorsanız (`UsesDataSource = true`), yukarıdaki yöntemlerden birini çağırma bir özel durum oluşturur.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Bir dış veri kaynağı ile çalışma

Yerleşik iç veri satırları için menü denetimi sağlamak için kullanmak yerine, isteğe bağlı olarak bir dış veri kaynağını kullanın ve öğeleri (örneğin, bir SQLite veritabanı) için kendi yedekleme deposu sağlar.

Bir dış veri kaynağı ile çalışmak için menü denetimin veri kaynağı örneği oluşturacaksınız (`NSComboBoxDataSource` gibi) ve gerekli veri sağlamak için çeşitli yöntemler geçersiz kılın:

- `ItemCount` -Listedeki öğe sayısını döndürür.
- `ObjectValueForItem` -Belirli bir dizin için öğenin değerini döndürür.
- `IndexOfItem` -Verin öğesi değerinin indisini döndürür.
- `CompletedString` -Kısmen yazılı öğe değeri ilk eşleşen öğe değerini döndürür. Otomatik Tamamlama etkinleştirildiyse, bu yöntem yalnızca çağrılır (`Completes = true`).

Lütfen [veritabanları ve ComboBoxes](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) bölümünü [veritabanlarıyla çalışma](~/mac/app-fundamentals/databases.md) daha fazla ayrıntı için belge.

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>Listenin görünümü ayarlama

Menü denetimin görünümü ayarlamak aşağıdaki yöntemleri kullanılabilir:

- `HasVerticalScroller` -Eğer `true`, denetimin dikey bir kaydırma çubuğu görüntülenir. 
- `VisibleItems` -Denetim açıldığında görüntülenen öğe sayısını ayarlayın. Varsayılan değer beş (5).
- `IntercellSpacing` -Belirli bir öğe çevresindeki boşluğun miktarını sağlayarak Ayarla bir `NSSize` burada `Width` sol ve sağ kenar boşluklarını belirtir ve `Height` önce ve sonra bir öğesi alanını belirtir.
- `ItemHeight` -Listesindeki her öğenin yüksekliğini belirtir.

Aşağı açılan türlerinin `NSPopupButtons`, ilk menü öğesi denetimi için başlık sağlar. Örneğin: 

[![](standard-controls-images/menu02.png "Bir örnek menü denetimi")](standard-controls-images/menu02.png#lightbox)

Başlığı değiştirmek için bu öğe olarak kullanıma sunma bir **çıkışı** ve kod aşağıdaki gibi kullanın:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Seçili öğeleri düzenleme

Menü denetim listesinde seçilen öğeleri değiştirmek için aşağıdaki yöntemleri ve özellikleri izin ver:

- `SelectItem` -Belirtilen dizindeki öğeyi seçer.
- `Select` -Belirli bir öğe değerini seçin.
- `DeselectItem` -Öğe belirtilen dizindeki öğenin seçimi kaldırılır.
- `SelectedIndex` -Seçili öğenin dizinini döndürür.
- `SelectedValue` -Seçili öğenin değerini döndürür.

Kullanım `ScrollItemAtIndexToTop` listenin üst kısmındaki belirtilen dizindeki öğeyi sunmak ve `ScrollItemAtIndexToVisible` listesine verilen dizindeki öğesi görünene kadar kaydırın.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Olaylarına yanıt verme

Kullanıcı etkileşimine yanıt vermek için aşağıdaki olaylar menü denetimleri sağlayın:

- `SelectionChanged` -Kullanıcı, listeden bir değer seçtiğinde adı verilir.
- `SelectionIsChanging` -Yeni kullanıcı seçili öğe etkin seçimin raporluyor adı verilir.
- `WillPopup` -Açılır listeden öğeleri görüntülenmeden önce adı verilir.
- `WillDismiss` -Açılır liste öğesi kapatılmadan hemen önce adı verilir.

İçin `NSComboBox` denetimleri içerirler aynı olayların tümünü `NSTextField`, gibi `Changed` birleşik giriş kutusundaki metin değerini kullanıcı düzenlemeleri her çağrılan olay.

İsteğe bağlı olarak yanıt arabirim öğeye ekleyerek seçili Oluşturucu bir iç veri menü öğelerini tanımlanan bir **eylem** ve yanıt vermek için aşağıdaki gibi bir kod kullanın **eylem** olan kullanıcı tarafından tetiklenen:

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Menüleri ve menü denetimleri ile çalışma hakkında daha fazla bilgi için lütfen bkz. bizim [menüleri](~/mac/user-interface/menu.md) ve [açılır düğme ve aşağı açılır listeler](~/mac/user-interface/menu.md) belgeleri.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Seçim denetimleri ile çalışma

AppKit birden fazla kullanıcı arabirimi tasarımı kullanılabilecek seçim denetimleri sağlar. Daha fazla bilgi için lütfen bkz [seçim denetimleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/select01.png "Örnek seçim denetimleri")](standard-controls-images/select01.png#lightbox)

Seçim denetimi olarak ifşa eden kullanıcı etkileşimi olduğunda izlemenin iki yolu vardır. bir **eylem**. Örneğin:

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

Özel denetimler (örneğin, iyi renk ve resim de), değer türleri için belirli özelliklere sahiptir. Örneğin:

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

`NSDatePicker` Doğrudan tarih ve saat ile çalışmak için aşağıdaki özelliklere sahiptir:

- **DateValue** -geçerli tarih ve saat değeri olarak bir `NSDate`.
- **Yerel** -kullanıcının konumu olarak bir `NSLocal`.
- **TimeInterval** -saat değeri olarak bir `Double`.
- **Saat dilimi** -kullanıcının saat diliminde bir `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Gösterge denetimleriyle çalışma

AppKit birden fazla kullanıcı arabirimi tasarımı kullanılabilecek bir göstergesi denetimleri sağlar. Daha fazla bilgi için lütfen bkz [göstergesi denetimleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/level01.png "Örnek göstergesi denetimleri")](standard-controls-images/level01.png#lightbox)

Bir göstergesi denetimi olarak ifşa eden ya da kullanıcı etkileşimi olduğunda izlemenin iki yolu vardır bir **eylem** veya **çıkışı** ve ekleyerek bir **temsilci** için`Activated`olay. Örneğin:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Okuma veya göstergesi denetimin değerini ayarlamak için kullanın `DoubleValue` özelliği. Örneğin:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Zaman uyumsuz bir İlerleme göstergesi ve belirsiz görüntülendiğinde animasyon. Kullanım `StartAnimation` görüntülendiklerinde animasyonu başlatmak için yöntemi. Örneğin:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Çağırma `StopAnimation` yöntemi animasyon durdurur.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Metin denetimleriyle çalışma

AppKit birkaç kullanıcı arabirimi tasarımı kullanılabilecek bir metin denetimi türleri sağlar. Daha fazla bilgi için lütfen bkz [metin denetimleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/text01.png "Örnek metin denetimleri")](standard-controls-images/text01.png#lightbox)

Metin alanları için (`NSTextField`), aşağıdaki olaylar, kullanıcı etkileşimi izlemek için kullanılabilir:

- **Değiştirilen** -kullanıcı değişiklik alanının değeri her zaman tetiklenir. Örneğin, her karakter türü belirtilmiş.
- **EditingBegan** -kullanıcı düzenleme alanı seçtiğinde tetiklenir.
- **EditingEnded** - kullanıcı alanında Enter tuşuna bastığında veya alan bırakır.

Kullanım `StringValue` özelliği okuma veya alanın değeri ayarlayın. Örneğin:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

Görüntülemek veya düzenlemek sayısal değerler alanlar için kullanabileceğiniz `IntValue` özelliği. Örneğin:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

Bir `NSTextView` tam bir öne çıkan metni sağlar düzenleyin ve yerleşik biçimlendirme ile bir alanı görüntüler. Gibi bir `NSTextField`, kullanın `StringValue` özelliği okuma veya alanın değeri ayarlayın.

Karmaşık bir Xamarin.Mac uygulamasını metin görünümlerle çalışma örnek bir örneği için lütfen bkz [SourceWriter örnek uygulaması](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter kod tamamlama ve basit söz dizimi vurgulama için destek sağlayan bir basit kaynak kod düzenleyicidir.

SourceWriter kod tamamen yorum yaptı ve mümkün olan durumlarda bağlantıları sahip olması sağlanan anahtar teknolojileri veya yöntemleri Xamarin.Mac kılavuzları belgelerinde ilgili bilgileri.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>İçerik görünümlerle çalışma

AppKit birden fazla kullanıcı arabirimi tasarımınızı kullanılabilir içerik görünümler sağlar. Daha fazla bilgi için lütfen bkz [içerik görünümleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[![](standard-controls-images/content01.png "Örnek içerik görünümü")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

Bir popover doğrudan belirli bir denetim veya ekran alanı ilgili işlevleri sağlayan geçici bir kullanıcı Arabirimi öğesidir. Denetim veya ilgili olduğu alan içeren bir pencere üzerinde bir popover gezinen ve kenarlığını içinden ortaya noktasını belirtmek için bir ok içerir.

Bir popover oluşturmak için aşağıdakileri yapın:

1. Açık `.storyboard` dosyasına çift tıklayarak bir popover için eklemek istediğiniz pencereyi **Çözüm Gezgini**
2. Sürükleme bir **görüntüleme denetleyicisi** gelen **kitaplığı denetçisi** üzerine **Arayüzü Düzenleyicisi**: 

    [![](standard-controls-images/content02.png "Kitaplıktan bir görünüm denetleyicisi seçme")](standard-controls-images/content02.png#lightbox)
4. Boyut ve düzenini tanımlamak **Özel Görünüm**: 

    [![](standard-controls-images/content04.png "Düzen düzenleme")](standard-controls-images/content04.png#lightbox)
5. Control tuşuna tıklama ve kaynağından açılan pencerenin sürükleyin **görünüm denetleyicisi**: 

    [![](standard-controls-images/content05.png "Bir segue oluşturmak için sürükleme")](standard-controls-images/content05.png#lightbox)
6. Seçin **Popover** açılan menüsünde: 

    [![](standard-controls-images/content06.png "Ayar segue türü")](standard-controls-images/content06.png#lightbox)
7. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

<a name="Tab_Views" />

### <a name="tab-views"></a>Sekme görünümleri

Sekme görünümleri oluşur (şuna benzer bir kesimli denetimine) olarak adlandırılan görünümü kümesi ile birleştirilmiş bir sekme listesi _bölmeleri_. Kullanıcı yeni bir sekme seçtiğinde, kendisine bağlı bölmesinde görüntülenir. Her bölmede kendi denetimleri kümesini içerir.

Xcode'un arabirim oluşturucu içinde sekme görünümü ile çalışırken kullanmak **özniteliği denetçisi** sekmeleri sayısını ayarlamak için:

[![](standard-controls-images/content08.png "Sekme sayısı düzenleme")](standard-controls-images/content08.png#lightbox)

Her sekmede seçin **arabirimi hiyerarşi** ayarlamak için kendi **başlık** ve kullanıcı Arabirimi öğeleri ekleyebilir, **bölmesinde**:

[![](standard-controls-images/content09.png "Xcode sekmeleri düzenleme")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>Veri bağlama olan tüm AppKit denetimleri

Xamarin.Mac uygulamanızda veri bağlama ve anahtar-değer kodlaması teknikleri kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız gereken kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma avantajı vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), bakımı kolay, daha esnek bir uygulama için önde gelen Tasarım.

Anahtar-değer kodlaması (KVC) olan bir mekanizma örnek değişkenleri erişmesini yerine özellikleri tanımlamak için anahtarları (özel olarak biçimlendirilmiş dizeler) veya erişimci yöntemlerini kullanarak, dolaylı olarak bir nesnenin özelliklerine erişme (`get/set`). Xamarin.Mac uygulamanızda anahtar-değer kodlaması uyumlu erişimcilerini uygulayarak, anahtar-değer gözleme (KVO), veri bağlama, temel veri, Cocoa bağlamaları ve scriptability gibi diğer macOS özelliklere erişiminiz olur.

Daha fazla bilgi için lütfen bkz [basit veri bağlama](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) bölümünü bizim [veri bağlama ve anahtar-değer kodlaması](~/mac/app-fundamentals/databinding.md) belgeleri.


<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede ayrıntılı bir Xamarin.Mac uygulamasındaki standart olan tüm AppKit denetimleri düğmeler, etiketler, metin alanları, onay kutularını ve bölümlenmiş denetimler gibi çalışma göz duruma getirdi. Bu, bir kullanıcı arabirimi tasarımı Xcode'un arabirimi Oluşturucu ekleme, bunları kod çıkışlar ve eylemleri aracılığıyla kullanıma sunmak ve C# kodunda olan tüm AppKit denetimleri ile çalışma kapsamında.

## <a name="related-links"></a>İlgili bağlantılar

- [MacControls (örnek)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Veri Bağlama ve Anahtar-Değer Kodlaması](~/mac/app-fundamentals/databinding.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Denetimleri ve görünümler hakkında](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
