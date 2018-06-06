---
title: Xamarin.Mac menüleri
description: Bu makalede, bir Xamarin.Mac uygulamasında Menülerle çalışma yer almaktadır. Oluşturma ve menüleri ve menü öğeleri Xcode ve arabirim Oluşturucu koruyarak ve bunlarla program aracılığıyla çalışma açıklar.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: cb89d1df60bafe14dcc989666f0eeb5d757e4017
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792927"
---
# <a name="menus-in-xamarinmac"></a>Xamarin.Mac menüleri

_Bu makalede, bir Xamarin.Mac uygulamasında Menülerle çalışma yer almaktadır. Oluşturma ve menüleri ve menü öğeleri Xcode ve arabirim Oluşturucu koruyarak ve bunlarla program aracılığıyla çalışma açıklar._

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, Objective-C ve Xcode çalışmak Geliştirici mu aynı Cocoa menüleri erişiminiz vardır. Xamarin.Mac doğrudan Xcode ile tümleşir nedeniyle, Xcode'nın arabirimi oluşturucu ve menü çubukları, menüler ve menü öğeleri korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için) kullanabilirsiniz.

Menüleri Mac uygulamanın kullanıcı deneyiminin ayrılmaz bir parçası olan ve yaygın olarak kullanıcı arabirimi çeşitli bölümlerini görünür:

- **Uygulamanın menü çubuğu** -her Mac uygulaması için ekranın üstünde görünür ana menü budur.
- **Bağlam menüleri** -kullanıcı sağ tıklatır veya denetim tıklama öğeyi penceresinde bu görünür.
- **Durum çubuğu** -Bu, ('sol menü çubuğunu saatinin) ekranın üstünde görünür ve sola öğeleri için eklendikçe büyür uygulama menü çubuğu uzak sağ tarafındaki alandır.
- **Menü yerleştirme** -menü yuva her bir uygulama için belirir zaman kullanıcı sağ tıklatır veya denetim-uygulamanın simgesine tıklama ya da kullanıcı simgesini tıklattığı ve fare düğmesini tutar.
- **Açılır düğmesi ve açılır listeleri** -açılır düğmesi seçilen bir öğeyi görüntüler ve kullanıcı tarafından tıklatıldığında seçmek üzere seçeneklerin bir listesini gösterir. Aşağı açılır listesi, genellikle geçerli görev bağlamına özgü komutları seçmek için kullanılır. açılan düğmesine türüdür. Her ikisi de bir penceresinde herhangi bir yerde görünebilir.

[![Bir örnek menü](menu-images/intro01.png "bir örnek menüsü")](menu-images/intro01-large.png#lightbox)

Bu makalede, biz Cocoa menü çubukları, menüler ve menü öğeleri Xamarin.Mac uygulama ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

## <a name="the-applications-menu-bar"></a>Uygulamanın menü çubuğu 

Her penceresinde bağlı kendi menü çubuğu sahip olduğu Windows işletim sistemi üzerinde çalışan uygulamalardan farklı olarak, bu uygulamadaki her penceresi için kullanılan ekranın üstünde çalışan bir tek menü çubuğu macOS üzerinde çalışan her uygulama vardır:

[![Menü çubuğu](menu-images/appmenu01.png "menü çubuğu")](menu-images/appmenu01-large.png#lightbox)

Bu menü çubuğundaki öğeleri etkin veya devre dışı geçerli bağlam veya uygulama ve kullanıcı arabirimi durumu belirli bir anda göre. Örneğin: üzerinde kullanıcı bir metin alanı seçerse, öğeleri **Düzenle** menü gelen gibi etkin **kopya** ve **Kes**.

Apple göre ve varsayılan olarak, tüm macOS uygulamaları menüleri ve uygulamanın menü çubuğunda görünmesini menü öğeleri standart kümesine sahiptir:

- **Apple menü** -bu menü hangi uygulama çalıştıran bağımsız olarak her zaman, kullanıcılar için uygun geniş öğeleri sisteme erişim sağlar. Bu öğeler geliştirici tarafından değiştirilemez.
- **Uygulama menüsünden** -bu menü uygulamanın adını kalın olarak görüntüler ve hangi uygulama şu anda çalışan tanımlamak kullanıcı yardımcı olur. Uygulamanızı bir bütün ve verilen belgedeki veya değil uygulama başlatılabileceği gibi işlem olarak uygulamak öğeleri içerir.
- **Dosya menüsü** - oluşturmak için kullanılan öğeleri veya kaydetme belgeleri ile uygulamanızı çalışır. Uygulamanızı belge tabanlı değilse, bu menü yeniden adlandırılmış veya kaldırılabilir.
- **Düzen menüsü** -komutları gibi tutan **Kes**, **kopya**, ve **Yapıştır** , düzenlemek veya uygulamanın kullanıcı arabiriminde öğeleri değiştirmek için kullanılır.
- **Biçim menüsü** - uygulama, metin biçimlendirmesini ayarlamak için ayrı tutma komutları bu menü metni ile çalışır.
- **Görünüm menüsü** -tutan içeriği nasıl görüntüleneceğini (uygulamanın kullanıcı arabiriminde görüntülenemez) etkileyen komutları.
- **Uygulamaya özgü menüler** -(örneğin, bir web tarayıcısı için bir yer işaretleri menü) uygulamanıza özgü menüler bunlar. Bunlar arasında görünmelidir **Görünüm** ve **penceresi** menü çubuğundaki.
- **Pencere menüsü** -geçerli pencereler listesi yanı sıra, uygulamanızı Windows'ta ile çalışmak için komutlar içerir.
- **Yardım menüsü** -uygulamanızı ekranda Yardım sağlıyorsa, Yardım menüsünden en sağdaki menü çubuğundaki olmalıdır. 

Apple'nın uygulama menü çubuğu ve Standart menüler ve menü öğeleri hakkında daha fazla bilgi için lütfen bkz [İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/).

### <a name="the-default-application-menu-bar"></a>Varsayılan uygulama menü çubuğu

Yeni bir Xamarin.Mac projesi oluşturduğunuzda, otomatik olarak macOS uygulama normalde (olarak yukarıdaki bölümde ele) olurdu tipik öğeleri içeren standart, varsayılan uygulama menü çubuğu alın. Uygulamanızın varsayılan menü çubuğu tanımlanan **Main.storyboard** (yanı sıra, uygulamanızın UI rest) dosyası projeye altında **çözüm paneli**:  

![Ana film şeridi seçin](menu-images/appmenu02.png "ana film şeridi seçin")

Çift **Main.storyboard** dosyayı Xcode'nın arabirimi oluşturucusu ve, düzenleme menü Düzenleyicisi arabirimiyle sunulur için açın:

[![Xcode kullanıcı Arabiriminde düzenleme](menu-images/defaultbar01.png "Xcode Arabiriminde düzenleme")](menu-images/defaultbar01-large.png#lightbox)

Buradan biz öğeleri gibi tıklatabilirsiniz **açık** menü öğesine **dosya** menü düzenleyin ve özelliklerini ayarla **öznitelikleri denetçisi**:

[![Bir menünün öznitelikleri düzenleme](menu-images/defaultbar02.png "bir menünün öznitelikleri düzenleme")](menu-images/defaultbar02-large.png#lightbox)

Ekleme, düzenleme ve menüleri ve bu makalenin sonraki bölümlerinde öğeleri silme içine elde edersiniz. Artık yalnızca hangi menüleri ve menü öğeleri varsayılan olarak kullanılabilir ve nasıl bunlar otomatik olarak bir dizi önceden tanımlanmış çıkışlar ve Eylemler aracılığıyla kodu maruz görmeyi istiyoruz için (daha fazla bilgi için bizim [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) belgeler).

Örneğin, biz tıklayın **bağlantı denetçisi** için **açık** biz bunu otomatik olarak kablolu kadar görebilir menü öğesi `openDocument:` eylem: 

[![Ekli eylem görüntüleme](menu-images/defaultbar03.png "ekli eylem görüntüleme")](menu-images/defaultbar03-large.png#lightbox)

Seçerseniz **ilk Yanıtlayıcı** içinde **arabirimi hiyerarşi** ve aşağı gidin **bağlantı denetçisi**, tanımını görürsünüz `openDocument:` Eylem, **açık** menü öğesi eklendiği (birlikte kadar denetimleri otomatik olarak kablolu değil ve çeşitli diğer varsayılan eylemleri uygulama için):

[![Tüm bağlı eylemler görüntüleme](menu-images/defaultbar04.png "tüm bağlı eylemler görüntüleme")](menu-images/defaultbar04-large.png#lightbox) 

Bu neden önemlidir? Bu otomatik olarak tanımlanan eylemleri otomatik olarak etkinleştir ve menü öğelerini devre dışı bırakın, yanı sıra için öğeler için yerleşik işlevsellik sağlamak diğer Cocoa kullanıcı arabirimi öğeleri ile nasıl çalıştığıyla sonraki bölümüne bakın.

Daha sonra Biz bu yerleşik Eylemler etkinleştirmek ve kodundan öğelerini devre dışı bırakın ve seçildiklerinde kendi işlevselliği sağlamak için kullanırsınız.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>Yerleşik menü işlevi

Herhangi bir kullanıcı Arabirimi öğeleri veya kodu eklemeden önce yeni oluşturulan bir Xamarin.Mac uygulama Çalıştır olsaydı, bazı öğeler otomatik olarak kablolu yukarı ve sizin için (tam olarak işlev ile otomatik olarak yerleşik) gibi etkin olduğunu fark edeceksiniz **Çık** öğesi **uygulama** menüsü:

![Etkin menü öğesi](menu-images/appmenu03.png "etkin menü öğesi")

Gibi diğer menü öğelerinin while **Kes**, **kopya**, ve **Yapıştır** değil:

![Menü öğelerini devre dışı](menu-images/appmenu04.png "menü öğelerini devre dışı")

Şimdi uygulamayı durdurun ve çift **Main.storyboard** dosyasını **çözüm paneli** Xcode'da düzenlemek üzere açmak için kullanıcının arabirimi Oluşturucu. Ardından, sürükleyin bir **metin görünümü** gelen **Kitaplığı** pencerenin görünümü denetleyicisine **arabirimi Düzenleyicisi**:

[![Kitaplıktan bir metin görünümü seçerek](menu-images/appmenu05.png "kitaplıktan bir metin görünümü seçme")](menu-images/appmenu05-large.png#lightbox)

İçinde **kısıtlaması Düzenleyicisi** şimdi metin görünümü pencerenin kenarlarına sabitleme ve burada büyür ve tüm dört kırmızı t-kirişleri Düzenleyicisi üstündeki ve'ı tıklatarak penceresiyle küçültür ayarlayın **4 kısıtlamalarıEkle** düğmesi:

[![Contraints düzenleme](menu-images/appmenu06.png "contraints düzenleme")](menu-images/appmenu06-large.png#lightbox)

Değişikliklerinizi kaydetmek için kullanıcı arabirimi tasarımı ve değişiklikleri Xamarin.Mac projenizi ile eşitlemek Mac için Visual Studio geri dönebilirsiniz. Şimdi uygulamayı başlatmak, bazı metni metin görünüme yazın, seçin ve açmak **Düzenle** menüsü:

![Menü öğelerini otomatik olarak etkin/devre dışı](menu-images/appmenu07.png "menü öğelerini otomatik olarak etkin/devre dışı")

Bildirim nasıl **Kes**, **kopya**, ve **Yapıştır** öğeler otomatik olarak etkinleştirilmiş ve tam olarak işlevsel tek satırlık bir kod yazmadan. 

Burada neler olup bittiğini? Yerleşik önceden (yukarıda sunulan gibi) kadar varsayılan menü öğeleri kablolu gelen eylemleri, macOS parçası olan Cocoa kullanıcı arabirimi öğelerinin çoğunu kancaları belirli eylemler için yerleşik unutmayın (gibi `copy:`). Bu nedenle bir penceresine etkin, eklenen ve seçtiğiniz, ilgili menü öğesine ya da bağlı öğeleri zaman bu eylemi otomatik olarak etkinleştirilir. Kullanıcı bu menü öğesini seçerse, kullanıcı Arabirimi öğesine yerleşik işlevi çağrılır ve, yürütülen tüm geliştirici müdahalesi olmadan.

### <a name="enabling-and-disabling-menus-and-items"></a>Menüler ve öğeleri devre dışı bırakma ve etkinleştirme

Varsayılan olarak kullanıcı olayı her gerçekleştiğinde, `NSMenu` otomatik olarak etkinleştirir ve her görünen menüyü ve menü öğesi uygulamanın bağlamına dayalı devre dışı bırakır. Etkinleştir/devre dışı bir öğe için üç yol vardır:

- **Otomatik menü etkinleştirme** -menü öğesi etkinleştirilip `NSMenu` öğesi kablolu için yukarı eylemi yanıtlaması uygun bir nesne bulabilirsiniz. Bir yerleşik kancası olan Örneğin, metin görünümü yukarıdaki `copy:` eylem.
- **Özel Eylemler ve validateMenuItem:** - bağlı herhangi bir menü öğesi için bir [penceresi veya Görünüm özel eylem denetleyicisi](#Working-with-Custom-Window-Actions), ekleyebileceğiniz `validateMenuItem:` eylem ve el ile etkinleştirmeniz veya menü öğelerini devre dışı bırakın.
- **El ile menü etkinleştirme** -el ile ayarlamanız `Enabled` her özellik `NSMenuItem` etkinleştirmek veya ayrı ayrı her bir menü öğesi devre dışı bırakmak için.

Bir sistem seçmek için ayarlanmış `AutoEnablesItems` özelliği bir `NSMenu`. `true` Otomatik (varsayılan davranış) ve `false` el ile gerçekleştirilir. 

> [!IMPORTANT]
> El ile menü etkinleştirme kullanmayı seçerseniz, menüsünü hiçbiri öğe olanlar gibi AppKit sınıfları tarafından denetlenen `NSTextView`, otomatik olarak güncelleştirilir. Etkinleştirme ve el ile kod tüm öğelerin devre dışı bırakma sorumlu olacaktır.

#### <a name="using-validatemenuitem"></a>ValidateMenuItem kullanma

Bağlı herhangi bir menü öğesi için yukarıda belirtildiği gibi bir [penceresi veya Görünüm denetleyicisini özel eylem](#Working-with-Custom-Window-Actions), ekleyebileceğiniz `validateMenuItem:` eylem ve el ile etkinleştirmeniz veya menü öğelerini devre dışı bırakın.

Aşağıdaki örnekte, `Tag` özelliği, etkin/tarafından devre dışı menü öğesi türünü karar vermek için kullanılacak `validateMenuItem:` eylem, seçili metinde durumunu temel bir `NSTextView`. `Tag` Özelliği ayarlanmış arabirimi Oluşturucusu'nda her menü öğesi için:

![Tag özelliği ayarlama](menu-images/validate01.png "Tag özelliği ayarlama")

Ve görünüm denetleyiciye eklenen aşağıdaki kodu:

```csharp
[Action("validateMenuItem:")]
public bool ValidateMenuItem (NSMenuItem item) {

    // Take action based on the menu item type
    // (As specified in its Tag)
    switch (item.Tag) {
    case 1:
        // Wrap menu items should only be available if
        // a range of text is selected
        return (TextEditor.SelectedRange.Length > 0);
    case 2:
        // Quote menu items should only be available if
        // a range is NOT selected.
        return (TextEditor.SelectedRange.Length == 0);
    }

    return true;
}
```

Ne zaman bu kodu çalıştırmak ve metin seçili `NSTextView`, (bunlar görünümü denetleyici eylemleri için kablolu arabirimlerdir olsa bile) iki kaydırma menü öğelerini devre dışı bırakılır:

![Devre dışı gösteren öğeleri](menu-images/validate02.png "gösteren devre dışı öğeler")

Metnin bir bölümünü seçtiyseniz ve menüsünün yeniden açılacak varsa, iki kaydırma menü öğeleri kullanılabilir:

![Etkin gösteren öğeleri](menu-images/validate03.png "gösteren etkin öğeleri")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>Etkinleştirme ve kod menü öğeleri yanıtlama

Yukarıdaki bizim UI tasarım (örneğin, bir metin alanı), yalnızca belirli Cocoa kullanıcı arabirimi öğeleri ekleyerek anlatıldığı gibi birkaç varsayılan menü öğelerinin etkinleştirilir ve otomatik olarak, herhangi bir kod yazmak zorunda kalmadan işlev. Sonraki kendi C# kod menü öğesi etkinleştirmek ve kullanıcı seçtiğinde işlevselliği sağlamak için Xamarin.Mac Projemizin ekleme konumundaki bakalım.

Örneğin, kullanılacak kullanıcı istiyoruz let say **açık** öğesi **dosya** menüsünde bir klasör seçin. Bu uygulama çapında işlevi istediğiniz ve bir verme penceresi veya kullanıcı Arabirimi öğesi bunlarla sınırlı olmamak olduğundan, bu bizim uygulama temsilciye işlemek üzere kod eklemek için yapacağız.

İçinde **çözüm paneli**, çift `AppDelegate.CS` dosyayı düzenlemek için açın:

![Uygulama temsilci seçme](menu-images/appmenu08.png "uygulama temsilci seçme")

Aşağıdaki kodu ekleyin `DidFinishLaunching` yöntemi:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

Şimdi uygulamayı şimdi çalıştırmak ve açmak **dosya** menüsü: 

![Dosya menüsü](menu-images/appmenu09.png "Dosya menüsü")

Dikkat **açık** menü öğesini şu anda etkin. Biz bu seçeneği belirlerseniz Aç iletişim kutusu görüntülenir:

![Açık bir iletişim kutusu](menu-images/appmenu10.png "açık bir iletişim kutusu")

Biz tıklatırsanız **açık** düğmesi, bizim uyarı iletisi görüntülenir:

![Bir örnek iletişim iletisini](menu-images/appmenu11.png "bir örnek iletişim iletisini")

Burada anahtar satırı `[Export ("openDocument:")]`, bunu belirten `NSMenu` , bizim **AppDelegate** bir yönteme sahip `void OpenDialog (NSObject sender)` , yanıt verir `openDocument:` eylem. Yukarıda, unutmayın, **açık** menü öğesi otomatik olarak kablolu yukarı Bu eylem için varsayılan arabirimi Oluşturucu:

[![Görüntüleme ekli eylemleri](menu-images/defaultbar03.png "bağlı eylemler görüntüleme")](menu-images/defaultbar03-large.png#lightbox)

Sonraki kendi menüsü, menü öğeleri ve eylemleri oluşturma ve yanıt kodu onlara bakalım.

### <a name="working-with-the-open-recent-menu"></a>Yeni menü Aç ile çalışma

Varsayılan olarak, **dosya** menü içeren bir **açık son** kullanıcı uygulamanızı açtı son birkaç dosyaları, öğesi. Oluşturuyorsanız, bir `NSDocument` Xamarin.Mac uygulama dayalı bu menü sizin için otomatik olarak ele alınacaktır. Tüm diğer Xamarin.Mac uygulama türü için yönetme ve bu menü öğesine el ile yanıt sorumlu olacaktır.

El ile işlemek için **açık son** menüsünde ilk gerekir yeni bir dosya açılan veya aşağıdaki kullanılarak kaydedilen olduğunu bildirmek:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

Uygulamanızı değil kullanıyor olsa da `NSDocuments`, kullanmaya devam `NSDocumentController` korumak için **açık son** göndererek menü bir `NSUrl` dosyasına konumu ile `NoteNewRecentDocumentURL` yöntemi `SharedDocumentController`.

Ardından, geçersiz kılmanız gerekir `OpenFile` yöntemi kullanıcı seçer herhangi bir dosyayı açmak için uygulama temsilcisi **açmak son** menüsü. Örneğin:

```csharp
public override bool OpenFile (NSApplication sender, string filename)
{
    // Trap all errors
    try {
        filename = filename.Replace (" ", "%20");
        var url = new NSUrl ("file://"+filename);
        return OpenFile(url);
    } catch {
        return false;
    }
}
```

Dönüş `true` dosya açılabiliyorsa başka dönmek `false` ve dosya açılamadı kullanıcıya yerleşik bir uyarı görüntülenir.

Dosya adı ve yolu döndürülen çünkü **açık son** menüsünde bir boşluk içerebilir, düzgün oluşturmadan önce bu karakteri kaçış ihtiyacımız bir `NSUrl` veya bir hata iletisi alır. Aşağıdaki kod ile bunu:

```csharp
filename = filename.Replace (" ", "%20");
```

Son olarak, oluşturduğumuz bir `NSUrl` işaret dosya ve uygulama yardımcı bir yöntem temsilci yeni penceresi açın ve dosyanın içine yüklemek için kullanabilirsiniz:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Her şeyi birlikte çıkarmak için bir örnek uygulama bakalım bir **AppDelegate.cs** dosyası:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace MacHyperlink
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }

        public override bool OpenFile (NSApplication sender, string filename)
        {
            // Trap all errors
            try {
                filename = filename.Replace (" ", "%20");
                var url = new NSUrl ("file://"+filename);
                return OpenFile(url);
            } catch {
                return false;
            }
        }
        #endregion

        #region Private Methods
        private bool OpenFile(NSUrl url) {
            var good = false;

            // Trap all errors
            try {
                var path = url.Path;

                // Is the file already open?
                for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
                    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
                    if (content != null && path == content.FilePath) {
                        // Bring window to front
                        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
                        return true;
                    }
                }

                // Get new window
                var storyboard = NSStoryboard.FromName ("Main", null);
                var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

                // Display
                controller.ShowWindow(this);

                // Load the text into the window
                var viewController = controller.Window.ContentViewController as ViewController;
                viewController.Text = File.ReadAllText(path);
                viewController.SetLanguageFromPath(path);
                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                viewController.View.Window.RepresentedUrl = url;

                // Add document to the Open Recent menu
                NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);

                // Make as successful
                good = true;
            } catch {
                // Mark as bad file on error
                good = false;
            }

            // Return results
            return good;
        }
        #endregion

        #region actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = true;
            dlg.CanChooseDirectories = false;

            if (dlg.RunModal () == 1) {
                // Nab the first file
                var url = dlg.Urls [0];

                if (url != null) {
                    // Open the document in a new window
                    OpenFile (url);
                }
            }
        }
        #endregion
    }
}
```

Uygulamanızın gereksinimlerine bağlı olarak, size aynı dosyayı aynı anda birden fazla penceresinde açmak için kullanıcının istemeyebilirsiniz. Kullanıcı zaten açık olan bir dosya seçerse bizim örnek uygulamasında (araçtan **açmak son** veya **açın...** menü öğeleri), dosyayı içeren penceresi öne getirilir.

Bunu gerçekleştirmek için aşağıdaki kodu bizim yardımcı yönteminin kullandık:

```csharp
var path = url.Path;

// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

Biz tasarlanmış bizim `ViewController` dosyasında yolu tutmak için sınıf kendi `Path` özelliği. Ardından, biz uygulamadaki tüm açık windows döngü. Dosyanın zaten bir windows açıksa, kullanarak tüm windows öne getirilene:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Eşleşme bulunamazsa, yüklenen dosya ile yeni bir pencerede açılır ve dosyanın Not edilir **açık son** menüsü:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);

// Load the text into the window
var viewController = controller.Window.ContentViewController as ViewController;
viewController.Text = File.ReadAllText(path);
viewController.SetLanguageFromPath(path);
viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
viewController.View.Window.RepresentedUrl = url;

// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

<a name="Working-with-Custom-Window-Actions" />

### <a name="working-with-custom-window-actions"></a>Özel pencere Eylemler ile çalışma

Yerleşik'olduğu gibi **ilk Yanıtlayıcı** standart menü öğeleri için önceden kablolu gelen Eylemler, yeni, özel eylemler oluşturabilir ve bunları arabirimi Oluşturucusu menü öğelerine wire.

İlk olarak, özel bir eylem, uygulamanızın penceresi denetleyicilerinden birinde tanımlayın. Örneğin:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Ardından, uygulamanın film şeridi dosyasına çift tıklayarak **çözüm paneli** Xcode'da düzenlemek üzere açmak için kullanıcının arabirimi Oluşturucu. Seçin **ilk Yanıtlayıcı** altında **uygulama Sahne**, o **öznitelikleri denetçisi**:

![Öznitelikleri denetçisi](menu-images/action01.png "öznitelikleri denetçisi")

Tıklatın **+** alt kısmındaki düğmesi **öznitelikleri denetçisi** yeni bir özel eylem eklemek için:

![Yeni bir eylem ekleme](menu-images/action02.png "yeni bir eylem ekleme")

Bu pencere denetleyicinizde oluşturduğunuz özel eylem aynı adı verin:

![Eylem adı düzenleme](menu-images/action03.png "eylem adı düzenleme")

Denetim tıklatın ve bir menü öğesine sürükleyin **ilk Yanıtlayıcı** altında **uygulama Sahne**. Açılan listeden, az önce oluşturduğunuz yeni bir eylem seçin (`defineKeyword:` Bu örnekte):

![Bir eylem ekleme](menu-images/action04.png "bir eylem ekleme")

Film şeridi için değişiklikleri kaydetmek ve değişiklikleri eşitlemek Mac için Visual Studio geri dönün. Uygulama çalıştırırsanız, özel eylem bağlı menü öğesi otomatik olarak etkin / (üzerinde penceresi açık olan eylemiyle göre) devre dışı bırakılmış ve menü öğesi seçilerek eylem ateşlenir:

[![Yeni Eylem sınama](menu-images/action05.png "yeni eylemi test etme")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>Ekleme, düzenleme ve menüleri silme

Önceki bölümlerde anlatıldığı gibi bir Xamarin.Mac uygulaması varsayılan menüleri ve Özel UI denetimlerinin otomatik olarak etkinleştirin ve yanıt menü öğelerini önceden belirlenmiş bir dizi birlikte gelir. Biz de ayrıca etkinleştir ve bu varsayılan öğeler yanıt uygulamamız için kod ekleme gördünüz.

Bu bölümde biz gerekmez menü öğeleri kaldırma, menüler yeniden düzenleme ve yeni menüler ve menü öğeleri eylemler ekleme arar.

Çift **Main.storyboard** dosyasını **çözüm paneli** düzenlemek üzere açmak için:

[![Xcode kullanıcı Arabiriminde düzenleme](menu-images/maint01.png "Xcode Arabiriminde düzenleme")](menu-images/maint01-large.png#lightbox)

Belirli Xamarin.Mac uygulamamız için biz varsayılan kullanılmasını yapmayacağınız **Görünüm** biz kaldırmak olacak şekilde menüsü. İçinde **arabirimi hiyerarşi** seçin **Görünüm** ana menü çubuğunda bir parçası olan menü öğesi:

![Görünüm menü öğesi seçilerek](menu-images/maint02.png "görünüm menü öğesi seçilerek")

DELETE tuşlarına basın veya menü silmek için Geri Al. Ardından, biz tüm öğeler kullanılmasını giderek olmayan **biçimi** menü ve biz alt menüler menüsündeki üzerinden kullanıma kullanılacak olduğumuz öğeleri taşımak istediğiniz. İçinde **arabirimi hiyerarşi** aşağıdaki menü öğeleri seçin:

![Birden çok öğe vurgulama](menu-images/maint03.png "birden çok öğe vurgulama")

Öğe üst öğe altına sürükleyin **menü** şu anda oldukları alt menüsünde:

[![Menü öğeleri ana menüye sürükleme](menu-images/maint04.png "ana menüye menü öğeleri sürükleme")](menu-images/maint04-large.png#lightbox)

Menünüze gibi görünmelidir:

[![Yeni konumu öğelerde](menu-images/maint05.png "yeni konumu öğeleri")](menu-images/maint05-large.png#lightbox)

Sonraki şimdi sürükleyin **metin** gelen altında alt menüyü **biçimi** menü ve arasında ana menü çubuğundaki yerleştirin **biçimi** ve **penceresi** menüler:

[![Metin menüsü](menu-images/maint06.png "metin menüsü")](menu-images/maint06-large.png#lightbox)

Edelim geri altında **biçimi** menü ve delete **yazı tipi** alt menü öğesi. Ardından, **biçimi** menü ve "Yazı tipi" yeniden adlandırın:

[![Yazı tipi menü](menu-images/maint07.png "yazı tipi menüsü")](menu-images/maint07-large.png#lightbox)

Ardından, özel bir menüyü seçildiklerinde, metin görünümündeki metni için otomatik olarak eklenen predefine tümce oluşturalım. Arama kutusuna üzerinde altındaki **kitaplığı denetçisi** türü menüde"." Bu, bulmak ve tüm menü kullanıcı Arabirimi öğeleri ile çalışmak kolaylaştırır:

![Kitaplık denetçisi](menu-images/maint08.png "kitaplığı denetçisi")

Şimdi şimdi bizim menü oluşturmak için aşağıdakileri yapın:

1. Sürükleme bir **menü öğesi** gelen **kitaplığı denetçisi** arasında menü çubuğu üzerine **metin** ve **penceresi** menüleri: 

    ![Kitaplığı'nda yeni bir menü öğesi seçilerek](menu-images/maint10.png "Kitaplığı'nda yeni menü öğesi seçme")
2. "Tümcecikleri" öğeyi yeniden adlandırın: 

    [![Ayar menüsü adı](menu-images/maint09.png "ayar menüsü adı")](menu-images/maint09-large.png#lightbox)
3. Sonraki sürükleyin bir **menü** gelen **kitaplığı denetçisi**: 

    ![Kitaplıktan bir menüsünü seçerek](menu-images/maint11.png "kitaplıktan bir menü seçme")
4. Ardından bırakma **menü** yeni **menü öğesi** biz yalnızca oluşturulan ve "Tümcecikleri" adını değiştirin: 

    [![Menü adı düzenleme](menu-images/maint12.png "menüsü adı düzenleme")](menu-images/maint12-large.png#lightbox)
5. Artık üç varsayılan şimdi yeniden adlandırmak **menü öğeleri** "Adres", "Tarih" ve "Selamlama": 

    [![Tümcecikleri menü](menu-images/maint13.png "tümcecikleri menüsü")](menu-images/maint13-large.png#lightbox)
6. Dördüncü ekleyelim **menü öğesi** sürükleyerek bir **menü öğesi** gelen **kitaplığı denetçisi** ve "İmza" çağırma: 

    [![Menü öğesi adı düzenleme](menu-images/maint14.png "menü öğesi adı düzenleme")](menu-images/maint14-large.png#lightbox)
7. Menü çubuğu değişiklikleri kaydedin.

Şimdi yeni bizim menü öğeleri için C# kodu gösterilen böylece özel eylemler kümesi oluşturalım. Xcode'da şimdi geçiş **Yardımcısı** görünümü:

[![Gerekli eylemleri oluşturma](menu-images/maint15.png "gerekli eylemleri oluşturma")](menu-images/maint15-large.png#lightbox)

Şimdi aşağıdakileri yapın:

1. Gelen Control-sürükleyin **adresi** menü öğesine **AppDelegate.h** dosya.
2. Anahtar **bağlantı** için yazın **eylem**: 

    [![Eylem türü seçme](menu-images/maint17.png "eylem türü seçme")](menu-images/maint17-large.png#lightbox)
3. Girin bir **adı** "phraseAddress" tuşuna basın ve **Bağlan** düğmesi yeni bir eylem oluşturun: 

    [![Eylem yapılandırma](menu-images/maint18.png "eylemi yapılandırma")](menu-images/maint18-large.png#lightbox)
4. Yukarıdaki adımları yineleyin **tarih**, **selamlama**, ve **imza** menü öğeleri: 

    [![Tamamlanmış Eylemler](menu-images/maint19.png "tamamlanmış Eylemler")](menu-images/maint19-large.png#lightbox)
5. Menü çubuğu değişiklikleri kaydedin.

Sonraki biz biz içeriğini kodundan ayarlayabilmesi prizine bizim metin görünümü için oluşturmanız gerekir. Seçin **ViewController.h** dosyasını **Yardımcısı Düzenleyicisi** ve adlı yeni bir çıkış oluşturmak `documentText`:

[![Prizine oluşturma](menu-images/maint20.png "prizine oluşturma")](menu-images/maint20-large.png#lightbox)

Visual Studio Xcode değişikliklerden eşitlemek için Mac için geri dönün. Sonraki düzenleme **ViewController.cs** dosya ve şu şekilde görünür yapın:

```csharp
using System;

using AppKit;
using Foundation;

namespace MacMenus
{
    public partial class ViewController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }

        public string Text {
            get { return documentText.Value; }
            set { documentText.Value = value; }
        } 
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            App.textEditor = this;
        }

        public override void ViewWillDisappear ()
        {
            base.ViewDidDisappear ();

            App.textEditor = null;
        }
        #endregion
    }
}
```

Bu bizim metin görünümü dışında metnin sunan `ViewController` sınıfı ve ne zaman penceresi kazanır veya odağı kaybettiğinde uygulama temsilci bildirir. Şimdi Düzenle **AppDelegate.cs** dosya ve şu şekilde görünür yapın:

```csharp
using AppKit;
using Foundation;
using System;

namespace MacMenus
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public ViewController textEditor { get; set;} = null;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = false;
            dlg.CanChooseDirectories = true;

            if (dlg.RunModal () == 1) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
                    MessageText = "Folder Selected"
                };
                alert.RunModal ();
            }
        }

        partial void phrasesAddress (Foundation.NSObject sender) {

            textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
        }

        partial void phrasesDate (Foundation.NSObject sender) {

            textEditor.Text += DateTime.Now.ToString("D");
        }

        partial void phrasesGreeting (Foundation.NSObject sender) {

            textEditor.Text += "Dear Sirs,\n\n";
        }

        partial void phrasesSignature (Foundation.NSObject sender) {

            textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
        }
        #endregion
    }
}
```

Burada yaptık `AppDelegate` kısmi bir sınıf böylece biz Eylemler ve biz arabirimi Oluşturucusu'nda tanımlanan çıkışlar kullanabilirsiniz. Biz de kullanıma bir `textEditor` hangi şu anda odakta penceredir izlemek için.

Aşağıdaki yöntemlerden bizim Özel menü ve menü öğelerini işlemek için kullanılır:

```csharp
partial void phrasesAddress (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
}

partial void phrasesDate (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += DateTime.Now.ToString("D");
}

partial void phrasesGreeting (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Dear Sirs,\n\n";
}

partial void phrasesSignature (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
}
```

Biz uygulamamız, tüm öğeler çalıştırırsanız şimdi **tümcecik** menü etkin olur ve verme deyimi seçildiğinde metin görünümüne ekler:

![Çalışan uygulama örneği](menu-images/maint21.png "çalışan uygulama örneği")

Uygulama menü çubuğu'nu ile çalışmanın temelleri sahibiz, özel bağlamsal menü oluşturma sırasında bakalım.

### <a name="creating-menus-from-code"></a>Koddan menüleri oluşturma

Menüler ve menü öğeleri Xcode'nın arabirimi Oluşturucu ile oluşturmaya ek olarak, bir Xamarin.Mac uygulaması oluşturma, değiştirme veya menü, alt menü veya menü öğesi koddan kaldırma gerektiği zaman zamanlar olabilir.

Aşağıdaki örnekte, üzerinde-çalışma sırasında dinamik olarak oluşturulacak alt menüler ve menü öğeleri hakkındaki bilgileri tutmak için bir sınıf oluşturulur:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace AppKit.TextKit.Formatter
{
    public class LanguageFormatCommand : NSObject
    {
        #region Computed Properties
        public string Title { get; set; } = "";
        public string Prefix { get; set; } = "";
        public string Postfix { get; set; } = "";
        public List<LanguageFormatCommand> SubCommands { get; set; } = new List<LanguageFormatCommand>();
        #endregion

        #region Constructors
        public LanguageFormatCommand () {

        }

        public LanguageFormatCommand (string title)
        {
            // Initialize
            this.Title = title;
        }

        public LanguageFormatCommand (string title, string prefix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
        }

        public LanguageFormatCommand (string title, string prefix, string postfix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
            this.Postfix = postfix;
        }
        #endregion
    }
}
```

#### <a name="adding-menus-and-items"></a>Menüler ve öğeler ekleme

Bu sınıf tanımlanan, aşağıdaki yordamı koleksiyonu ayrıştırır `LanguageFormatCommand`nesneleri yinelemeli olarak oluşturun ve yeni menüler ve menü öğeleri, geçirilen (arabirimi Oluşturucusu'nda oluşturulan) mevcut menü altına ekleyerek:

```csharp
private void AssembleMenu(NSMenu menu, List<LanguageFormatCommand> commands) {
    NSMenuItem menuItem;

    // Add any formatting commands to the Formatting menu
    foreach (LanguageFormatCommand command in commands) {
        // Add separator or item?
        if (command.Title == "") {
            menuItem = NSMenuItem.SeparatorItem;
        } else {
            menuItem = new NSMenuItem (command.Title);

            // Submenu?
            if (command.SubCommands.Count > 0) {
                // Yes, populate submenu
                menuItem.Submenu = new NSMenu (command.Title);
                AssembleMenu (menuItem.Submenu, command.SubCommands);
            } else {
                // No, add normal menu item
                menuItem.Activated += (sender, e) => {
                    // Apply the command on the selected text
                    TextEditor.PerformFormattingCommand (command);
                };
            }
        }
        menu.AddItem (menuItem);
    }
}
``` 

Herhangi bir için `LanguageFormatCommand` boş olan nesneyi `Title` özelliği, bu yordamı oluşturur bir **ayırıcı menü öğesi** (ince gri çizgi) menü bölümleri arasında:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Başlık sağlanırsa, bu başlıkla yeni menü öğesi oluşturulur:

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Varsa `LanguageFormatCommand` nesnesini içeren alt `LanguageFormatCommand` nesneleri, bir alt menüsü oluşturulur ve `AssembleMenu` yinelemeli olarak menüye oluşturmak için çağrılan bir yöntemdir:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Alt menüler sahip olmayan tüm yeni menü öğesi için kullanıcı tarafından seçilmesini menü öğesini işlemek için kod eklenir:

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Menü oluşturma test etme

Yukarıdaki kod, yerinde hiçbiriyle varsa aşağıdaki koleksiyonu `LanguageFormatCommand` nesneleri oluşturulmuş:

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Stong","**","**"));
FormattingCommands.Add(new LanguageFormatCommand("Emphasize","_","_"));
FormattingCommands.Add(new LanguageFormatCommand("Inline Code","`","`"));
FormattingCommands.Add(new LanguageFormatCommand("Code Block","```\n","\n```"));
FormattingCommands.Add(new LanguageFormatCommand("Comment","<!--","-->"));
FormattingCommands.Add (new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Unordered List","* "));
FormattingCommands.Add(new LanguageFormatCommand("Ordered List","1. "));
FormattingCommands.Add(new LanguageFormatCommand("Block Quote","> "));
FormattingCommands.Add (new LanguageFormatCommand ());

var Headings = new LanguageFormatCommand ("Headings");
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 1","# "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 2","## "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 3","### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 4","#### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 5","##### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 6","###### "));
FormattingCommands.Add (Headings);

FormattingCommands.Add(new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Link","[","]()"));
FormattingCommands.Add(new LanguageFormatCommand("Image","![]("-")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[ ![]("-")](linkimagehere/index.md)"));
```

Ve toplama için geçirilen `AssembleMenu` işlevi (ile **biçimi** menüsü Ayarla temel olarak), aşağıdaki dinamik menüler ve menü öğeleri oluşturulacak:

![Yeni menü öğeleri çalışan uygulama](menu-images/dynamic01.png "çalışan uygulamanın yeni menü öğeleri")

#### <a name="removing-menus-and-items"></a>Menüler ve öğeleri kaldırma

Herhangi bir menü veya menü öğesi uygulamanın kullanıcı arabiriminden kaldırmanız gerekirse, kullanabileceğiniz `RemoveItemAt` yöntemi `NSMenu` sınıfı yalnızca kaldırılacak öğenin dizini dayalı sıfır vererek.

Örneğin, menüler ve menü öğeleri yukarıdaki yordamı tarafından oluşturulan kaldırmak için aşağıdaki kodu kullanabilirsiniz:

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

Dinamik olarak kaldırılmaz için yukarıdaki kod söz konusu olduğunda, Xcode'nın arabirimi oluşturucusu ve işlemlerine uygulamada, kullanılabilir ilk dört menü öğeleri oluşturulur.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>Bağlam menüleri

Bağlam menüleri kullanıcı sağ tıklatır veya denetim tıklama öğeyi penceresinde görünür. Varsayılan olarak, birkaç macOS daha önce oluşturulan kullanıcı Arabirimi öğeleri (metin görünümü gibi) iliştirilmiş bağlamsal menüleri sahiptir. Ancak, biz pencere ekledik bir kullanıcı Arabirimi öğesi için kendi özel bağlamsal menüler oluşturmak istediğinizde kez olabilir.

Düzenleyelim bizim **Main.storyboard** dosya Xcode'da ve eklemek bir **penceresi** bizim tasarım penceresini kendi **sınıfı** "NSPanel" için **kimlik denetçisi**, yeni bir ekleme **Yardımcısı** öğesinin **penceresi** menüsünde ve penceresini yeni kullanarak eklemektir bir **Göster ü**:

[![Ayar segue türü](menu-images/context01.png "ayar segue türü")](menu-images/context01-large.png#lightbox)

Şimdi aşağıdakileri yapın:

1. Sürükleme bir **etiket** gelen **kitaplığı denetçisi** üzerine **Masası** penceresi ve kendi metin "Özellik" olarak ayarlanmış: 

    [![Etiketin değerini düzenleme](menu-images/context03.png "etiketin değerini düzenleme")](menu-images/context03-large.png#lightbox)
2. Sonraki sürükleyin bir **menü** gelen **kitaplığı denetçisi** hiyerarşisini görüntüleme ve yeniden adlandırma üç varsayılan menü öğeleri görünüm denetleyicisine **belge**, **metin**  ve **yazı tipi**:

    [![Gerekli menü öğelerini](menu-images/context02.png "gerekli menü öğeleri")](menu-images/context02-large.png#lightbox)
3. Şimdi control-gelen sürükleyin **özelliği etiket** üzerine **menü**:

    [![Bir segue oluşturmak için sürükleme](menu-images/context04.png "bir segue oluşturmak için sürükleme")](menu-images/context04-large.png#lightbox)
4. Açılan iletişim kutusundan seçin **menü**: 

    ![Ayar segue türü](menu-images/context05.png "ayar segue türü")
5. Gelen **kimlik denetçisi**, "PanelViewController" görünümü denetleyicinin sınıfa ayarlayın: 

    [![Ayar segue sınıfı](menu-images/context10.png "ayar segue sınıfı")](menu-images/context10-large.png#lightbox)
6. Eşitlemek Mac için Visual Studio için dönün sonra arabirimi oluşturucuya döndürür.
7. Geçiş **Yardımcısı Düzenleyicisi** seçip **PanelViewController.h** dosya.
8. İçin bir eylem oluşturun **belge** menü öğesi adlı `propertyDocument`: 

    [![Eylem yapılandırma](menu-images/context06.png "eylemi yapılandırma")](menu-images/context06-large.png#lightbox)
9. Oluşturma işlemleri için kalan menü öğeleri Yinele: 

    [![Gerekli eylemleri](menu-images/context07.png "gerekli eylemleri")](menu-images/context07-large.png#lightbox)
10. Son olarak prizine için oluşturun **özelliği etiket** adlı `propertyLabel`: 

    [![Çıkış yapılandırma](menu-images/context08.png "çıkış yapılandırma")](menu-images/context08-large.png#lightbox)
11. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Düzen **PanelViewController.cs** dosya ve aşağıdaki kodu ekleyin:

```csharp
partial void propertyDocument (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Document";
}

partial void propertyFont (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Font";
}

partial void propertyText (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Text";
}
```

Şimdi uygulamayı çalıştırın ve özellik etiketi panelinde sağ tıklatın, özel bizim bağlamsal menü göreceğiz. Seçerseniz ve menüsünden öğesi, etiketin değerini değiştirir:

![Çalışan bağlamsal menü](menu-images/context09.png "çalıştıran bağlam menüsü")

Sonraki durum çubuğu menüler oluşturma sırasında bakalım.

## <a name="status-bar-menus"></a>Durum çubuğu menüler

Durum çubuğu menüler bir menüsü veya bir uygulamanın durumunu yansıtacak şekilde görüntü gibi kullanıcı için bir koleksiyonu ile etkileşimi sağlayan durum menü öğeleri veya geri bildirim görüntüleyin. Uygulama arka planda çalışıyor olsa bile bir uygulamanın durum çubuğu menü etkin ve etkin kalır. Sistem genelinde durum çubuğu uygulama menü çubuğu sağ tarafında bulunan ve yalnızca durum çubuğunun macOS içinde şu anda kullanılabilir.

Düzenleyelim bizim **AppDelegate.cs** dosya ve olun `DidFinishLaunching` yöntemi görünüm aşağıdaki gibi:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Create a status bar menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Text";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Text");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        PhraseAddress(address);
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        PhraseDate(date);
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        PhraseGreeting(greeting);
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        PhraseSignature(signature);
    };
    item.Menu.AddItem (signature);
}
```

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` Bize sistem genelinde durum çubuğu erişmenizi sağlar. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` Yeni bir durum çubuğu öğesi oluşturur. Buradan menü ve bir dizi menü öğeleri oluşturun ve yeni oluşturduğumuz durum çubuğu öğesi menüsü ekleme. 

Biz uygulama çalıştırıyorsanız, yeni durum çubuğu öğesi görüntülenir. Bir öğe menüden seçerek metin görünümünde metin değişir: 

![Çalışan durum çubuğu menüsünü](menu-images/statusbar01.png "çalıştıran durum çubuğu menüsü")

Ardından, özel yerleştirme menü öğeleri oluşturma sırasında bakalım.

## <a name="custom-dock-menus"></a>Özel yerleştirme menüleri

Kullanıcı sağ tıklatır ya da Denetim-yuva uygulamanın simgesine tıklama yerleştirme menü Mac uygulaması görüntülenir:

![Özel yerleştirme menü](menu-images/dock01.png "özel yerleştirme menüsü")

Aşağıdakileri yaparak uygulamamız için bir özel yerleştirme menü oluşturalım:

1. Mac için Visual Studio'da Uygulama projesine sağ tıklatın ve **Ekle** > **yeni dosya...** Yeni dosya iletişim kutusundan seçin **Xamarin.Mac** > **boş arabirim tanımı**, "DockMenu" kullanılmaya **adı** tıklatıp **yeni**  yeni oluşturmak için düğmesini **DockMenu.xib** dosyası:

    ![Boş bir arabirim tanımı ekleme](menu-images/dock02.png "boş bir arabirim tanımı ekleme")
2. İçinde **çözüm paneli**, çift **DockMenu.xib** dosyayı Xcode'da düzenlemek için açın. Yeni bir **menü** aşağıdaki öğeleri içeren: **adresi**, **tarih**, **selamlama**, ve **imza** 

    [![Kullanıcı arabirimini yerleştirmede](menu-images/dock03.png "kullanıcı arabirimini düzenleme")](menu-images/dock03-large.png#lightbox)
3. Ardından, özel bizim menüde için oluşturduğumuz bizim varolan eylemler şimdi bizim yeni menü öğeleri bağlanmak [ekleme, düzenleme ve silme menüleri](#Adding,_Editing_and_Deleting_Menus) yukarıdaki bölümde. Geçiş **bağlantı denetçisi** seçip **ilk Yanıtlayıcı** içinde **arabirimi hiyerarşi**. Aşağı kaydırın ve Bul `phraseAddress:` eylem. Bu eylemi daire bir satır sürükleyin **adresi** menü öğesi:

    [![Hat üzeri bir eylem yukarı sürükleyerek](menu-images/dock04.png "kablo bir eylem yukarı sürükleyerek")](menu-images/dock04-large.png#lightbox)
4. Karşılık gelen eylemlerini ekleme tüm menü öğelerinin için yineleyin: 

    [![Gerekli eylemleri](menu-images/dock05.png "gerekli eylemleri")](menu-images/dock05-large.png#lightbox)
5. Ardından, **uygulama** içinde **arabirimi hiyerarşi**. İçinde **bağlantı denetçisi**, bir satırı daire sürükleyin `dockMenu` yeni oluşturduğumuz menüsüne çıkışı:

    [![Kablo çıkış yukarı sürükleyerek](menu-images/dock06.png "kablo çıkış yukarı sürükleyerek")](menu-images/dock06-large.png#lightbox)
6. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.
7. Çift **Info.plist** dosyayı düzenlemek için açın: 

    [![Info.plist dosyasını düzenleyerek](menu-images/dock07.png "Info.plist dosyasını düzenleme")](menu-images/dock07-large.png#lightbox)
8. Tıklatın **kaynak** ekranın altındaki sekmesi: 

    [![Kaynak Görünümü seçerek](menu-images/dock08.png "kaynak görünümü seçme")](menu-images/dock08-large.png#lightbox)
9. Tıklatın **yeni giriş Ekle**, yeşil artı düğmesini tıklatın, özellik adı "AppleDockMenu" ve "DockMenu" (uzantısı olmadan yeni bizim .xib dosya adı) değerine ayarlayın: 

    [![DockMenu öğesi ekleme](menu-images/dock09.png "DockMenu öğesi ekleme")](menu-images/dock09-large.png#lightbox)

Şimdi uygulamamızı çalıştırmak ve yuva simgesini sağ tıklatın, yeni menü öğeleri görüntülenir:

![Çalışan yerleştirme menü örneği](menu-images/dock10.png "çalıştıran yerleştirme menü örneği")

Size özel öğeler menüsünden birini seçerseniz, bizim metin görünümündeki metni değiştirilecek.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>Açılır düğmesi ve aşağı açılır listeler

Açılır düğmesi seçilen bir öğeyi görüntüler ve kullanıcı tarafından tıklatıldığında seçmek üzere seçeneklerin bir listesini sunar. Aşağı açılır listesi, genellikle geçerli görev bağlamına özgü komutları seçmek için kullanılır. açılan düğmesine türüdür. Her ikisi de bir penceresinde herhangi bir yerde görünebilir.

Aşağıdakileri yaparak uygulamamız için özel bir açılır düğmesi oluşturalım:

1. Düzenleme **Main.storyboard** Xcode ve sürükleyin dosyasında bir **açılan düğmesine** gelen **kitaplığı denetçisi** üzerine **Masası** oluşturduğumuz içinde penceresi [bağlamsal menüleri](#Contextual_Menus) bölümü: 

    [![Açılan düğme ekleme](menu-images/popup01.png "açılan düğme ekleme")](menu-images/popup01-large.png#lightbox)
2. Yeni bir menü öğesi ekleme ve açılan öğeleri başlıklarını kümesindeki: **adresi**, **tarih**, **selamlama**, ve **imza** 

    [![Menü öğelerini yapılandırma](menu-images/popup02.png "menü öğelerini yapılandırma")](menu-images/popup02-large.png#lightbox)
3. Ardından, özel bizim menüde için oluşturduğumuz varolan eylemlerin şimdi bizim yeni menü öğeleri bağlanmak [ekleme, düzenleme ve silme menüleri](#Adding,_Editing_and_Deleting_Menus) yukarıdaki bölümde. Geçiş **bağlantı denetçisi** seçip **ilk Yanıtlayıcı** içinde **arabirimi hiyerarşi**. Aşağı kaydırın ve Bul `phraseAddress:` eylem. Bu eylemi daire bir satır sürükleyin **adresi** menü öğesi: 

    [![Hat üzeri bir eylem yukarı sürükleyerek](menu-images/popup03.png "kablo bir eylem yukarı sürükleyerek")](menu-images/popup03-large.png#lightbox)
4. Karşılık gelen eylemlerini ekleme tüm menü öğelerinin için yineleyin: 

    [![Tüm gerekli eylemleri](menu-images/popup04.png "tüm gerekli eylemleri")](menu-images/popup04-large.png#lightbox)
5. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.

Şimdi uygulamamızı çalıştırmak ve açılır penceresinden bir öğe seçin, bizim metin görünümünde metin değişir:

![Çalışan açılan örneği](menu-images/popup05.png "çalıştıran açılan örneği")

Oluşturun ve içeren aşağı açılır listeleri açılır düğmeler tam aynı şekilde çalışır. Yalnızca bizim bağlamsal menüde için yaptığımız gibi varolan eylem eklemek yerine, kendi özel eylemler oluşturabilirsiniz [bağlamsal menüleri](#Contextual_Menus) bölümü.

## <a name="summary"></a>Özet

Bu makalede, menüler ve menü öğeleri Xamarin.Mac uygulamasında çalışan bir ayrıntılı bakış sürdü. İlk biz uygulamanın menü çubuğunda, biz durum çubuğu menüler incelenmesi ve özel yerleştirme menüleri sonraki bağlamsal menüleri oluşturma sırasında aranan incelendi. Son olarak, açılır menüler ve açılır listeleri ele.


## <a name="related-links"></a>İlgili bağlantılar

- [MacMenus (örnek)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [İnsan Arabirimi yönergelerine - menüleri](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Uygulama menüleri ve açılır listeleri giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
