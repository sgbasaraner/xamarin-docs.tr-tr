---
title: Xamarin.Mac menüleri
description: Bu makale, bir Xamarin.Mac uygulamasında Menülerle çalışma kapsar. Bu, menüleri ve menü öğeleri Xcode ve arabirim Oluşturucu Oluşturma ve bunları ile program aracılığıyla çalışma açıklar.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d84910cd5c2bc72a563fb04457532d544aedf571
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780618"
---
# <a name="menus-in-xamarinmac"></a>Xamarin.Mac menüleri

_Bu makale, bir Xamarin.Mac uygulamasında Menülerle çalışma kapsar. Bu, menüleri ve menü öğeleri Xcode ve arabirim Oluşturucu Oluşturma ve bunları ile program aracılığıyla çalışma açıklar._

C# ve .NET ile bir Xamarin.Mac uygulamasında çalışırken, Objective-C ve Xcode içinde çalışan bir geliştirici yaptığı aynı Cocoa menüleri erişiminiz vardır. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü oluşturup, menü çubukları, menüler ve menü öğeleri korumak (veya isteğe bağlı olarak bunları doğrudan C# kodu oluşturmak için) Xcode'un arabirim Oluşturucu kullanabilirsiniz.

Menüleri, bir Mac uygulamasının kullanıcı deneyimi ayrılmaz bir parçasıdır ve yaygın olarak kullanıcı arabiriminin çeşitli bölümlerinde görünür:

- **Uygulamanın menü çubuğu** -her bir Mac uygulaması için ekranın üst kısmında görünür ana menüye budur.
- **Bağlam menüleri** -kullanıcı tıkladığı veya denetim tıklamasıyla bir pencerede bir öğe olduğunda bu görünür.
- **Durum çubuğu** -bu kadar sağ tarafındaki (menü çubuğu saati solundaki) ekranın üst kısmında görünür ve sola öğeleri için eklendikçe büyür uygulama menü çubuğu alanıdır.
- **Dock menüsünü** -her iki uygulamada dock menüsünü görüntülenir kullanıcı tıkladığı veya Denetim-uygulama simgesine tıklama veya kullanıcı simgesini tıklattığı ve fare düğmesini tutar.
- **Açılır düğme ve aşağı açılır listeleri** -açılır düğme seçili bir öğe görüntüler ve kullanıcı tarafından tıklandığında seçmek için seçenekleri bir listesini sunar. Aşağı açılır liste, şu anki görevini bağlamına özgü komutların seçmek için genellikle kullanılan açılır düğme türüdür. Her ikisi de, herhangi bir pencere içinde görünebilir.

[![Bir örnek menü](menu-images/intro01.png "örnek menüsü")](menu-images/intro01-large.png#lightbox)

Bu makalede, biz Cocoa menü çubukları, menüler ve menü öğeleri bir Xamarin.Mac uygulamasını ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

## <a name="the-applications-menu-bar"></a>Uygulamanın menü çubuğu 

Her pencere bağlı kendi menü çubuğu sahip olduğu Windows işletim sisteminde çalışan uygulamalardan farklı olarak, bu uygulamadaki her bir pencere için kullanılan ekranın üstünde çalışan tek bir menü çubuğu macOS üzerinde çalışan her bir uygulama vardır:

[![Bir menü çubuğu](menu-images/appmenu01.png "bir menü çubuğu")](menu-images/appmenu01-large.png#lightbox)

Bu menü çubuğundaki öğeleri etkin veya devre dışı geçerli bağlamı veya uygulama ve kullanıcı arabirimi durumu belirli bir andaki göre. Örneğin: üzerinde kullanıcı bir metin alanı seçerse, öğeleri **Düzenle** menü gelir gibi etkin **kopyalama** ve **Kes**.

Apple göre ve varsayılan olarak, tüm macOS uygulamaları menüleri ve menü öğeleri, uygulamanın menü çubuğunda görünür bir standart kümesine sahiptir:

- **Apple menüsü** -bu menü, hangi uygulama çalıştığından bağımsız olarak her zaman kullanılabilir olan geniş öğeleri sisteme erişim sağlar. Bu öğeler, geliştirici tarafından değiştirilemez.
- **Uygulama menüsü** -bu menü uygulamanın adını kalın olarak görüntüler ve kullanıcının hangi uygulama şu anda çalışıyor tanımlamak yardımcı olur. Bu uygulamayı bir bütün ve değil belirli belge veya uygulamadan çıkmayı gibi işlem olarak uygulanan öğeler içeriyor.
- **Dosya menüsü** - oluşturmak için kullanılan öğeleri veya belgeleri Kaydet uygulamanız ile çalışır. Uygulamanızı belge tabanlı değilse, bu menü olarak yeniden adlandırıldı veya kaldırıldı.
- **Düzen menüsü** -komutları gibi tutar **Kes**, **kopyalama**, ve **Yapıştır** düzenleyin veya uygulamanın kullanıcı arabirimi öğeleri değiştirmek için kullanılır.
- **Biçim menüsüne** - uygulama ayrı tutma komutları, metin biçimlendirmesini ayarlamak için bu menü metni ile çalışır.
- **Görünüm menüsü** -içeriğin nasıl görüntüleneceğini (uygulamanın kullanıcı arabiriminde görüntülenen) etkileyen komutlar içerir.
- **Uygulamaya özgü menüleri** -uygulamanız (örneğin, bir web tarayıcısı için yer işaretlerini menüsü) özgü menüler şunlardır. Arasında görünmelidir **görünümü** ve **penceresi** menü çubuğu.
- **Pencere menüsü** -geçerli açık pencereleri listesini yanı sıra, uygulamanızı windows ile çalışmaya yönelik komutlar içerir.
- **Yardım menüsü** -uygulamanızı ekranda Yardım sağlıyorsa, Yardım menüsünden en sağdaki menü çubuğundaki olmalıdır. 

Apple'nın uygulama menü çubuğu ve standart menüleri ve menü öğeleri hakkında daha fazla bilgi için bkz [İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/).

### <a name="the-default-application-menu-bar"></a>Varsayılan uygulama menü çubuğu

Yeni bir Xamarin.Mac projesi oluşturduğunuzda, otomatik olarak macOS uygulama normalde (olarak yukarıdaki bölümde ele alındığı) sahip olabileceği tipik öğeleri içeren standart, varsayılan uygulama menü çubuğu alın. Uygulamanızın varsayılan menü çubuğu tanımlanan **Main.storyboard** (birlikte, uygulamanızın kullanıcı arabiriminin geri kalan) dosya projede altında **çözüm bölmesi**:  

![Ana görsel taslak seçin](menu-images/appmenu02.png "ana görsel Taslak'ı seçin")

Çift **Main.storyboard** Dosya menüsü Düzenleyici arabirimi sunulur Xcode'un arabirim oluşturucu ve, düzenleme için açın:

[![Xcode kullanıcı Arabiriminde düzenleme](menu-images/defaultbar01.png "Xcode kullanıcı Arabiriminde düzenleme")](menu-images/defaultbar01-large.png#lightbox)

Buradan size öğelerde gibi tıklayabilirsiniz **açık** menü öğesi **dosya** menü ve düzenleyebilir ya da özelliklerini ayarlamak **öznitelikleri denetçisi**:

[![Bir menünün öznitelikleri düzenleme](menu-images/defaultbar02.png "bir menünün öznitelikleri düzenleme")](menu-images/defaultbar02-large.png#lightbox)

Ekleme, düzenleme ve menüleri ve bu makalenin devamındaki öğeleri silme elde edersiniz. Şimdi biz yalnızca hangi menüleri ve menü öğeleri varsayılan olarak kullanılabilir ve nasıl bunlar otomatik olarak bir dizi önceden tanımlanmış çıkışlar ve eylemleri aracılığıyla koda karşılaştıklarını görmek istiyorsanız için (daha fazla bilgi için bkz. bizim [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) belgeleri).

Örneğin, üzerinde tıklarsanız **bağlantı denetçisi** için **açık** menü öğesi biz bunu otomatik olarak kablolu kadar görebilirsiniz `openDocument:` eylem: 

[![Ekli eylem görüntüleme](menu-images/defaultbar03.png "ekli eylem görüntüleme")](menu-images/defaultbar03-large.png#lightbox)

Seçerseniz **ilk Yanıtlayıcı** içinde **arabirimi hiyerarşi** ve aşağı kaydırın **bağlantı denetçisi**, tanımını görürsünüz `openDocument:` Eylem, **açık** menü öğesi eklendiği (birlikte kadar denetimleri otomatik olarak kablolu değil ve çeşitli diğer varsayılan eylemleri uygulama için):

[![Ekli tüm eylemleri görüntüleme](menu-images/defaultbar04.png "ekli tüm eylemleri görüntüleme")](menu-images/defaultbar04-large.png#lightbox) 

Bu neden önemlidir? Bu otomatik olarak tanımlanan Eylemler ile diğer Cocoa kullanıcı arabirimi öğeleri otomatik olarak etkinleştir ve menü öğelerini devre dışı bırakın, aynı zamanda için öğeler için yerleşik işlevsellik sağlar nasıl sonraki bölüme bakın.

Daha sonra bu yerleşik Eylemler etkinleştirmek ve kod öğelerini devre dışı bırakın ve seçildiklerinde kendi işlevselliği sağlamak için kullanacağız.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>Yerleşik bir menüsünü işlevi

Herhangi bir kullanıcı Arabirimi öğeleri veya kodu eklemeden önce yeni oluşturulan bir Xamarin.Mac uygulamasını çalıştırma olsaydı, bazı öğeler otomatik olarak kablolu yukarı ve sizin için gibi (tam işlevselliği ile otomatik olarak yerleşik), etkin olduğunu fark edeceksiniz **Çık** öğesi **uygulama** menüsü:

![Bir etkin menü öğesi](menu-images/appmenu03.png "etkin menü öğesi")

Gibi diğer menü öğeleri, while **Kes**, **kopyalama**, ve **Yapıştır** değil:

![Menü öğelerini devre dışı](menu-images/appmenu04.png "menü öğelerini devre dışı")

Şimdi uygulamayı durdurun ve çift **Main.storyboard** dosyası **çözüm bölmesi** Xcode'da düzenlemek üzere açmak için kullanıcının arabirim Oluşturucu. Sonraki adımda bir **metni görünümü** gelen **Kitaplığı** pencerenin görünüm denetleyicisi üzerine **Arayüzü Düzenleyicisi**:

[![Kitaplıktan bir metin görünümünü seçerek](menu-images/appmenu05.png "kitaplıktan bir metin görünümünü seçme")](menu-images/appmenu05-large.png#lightbox)

İçinde **kısıtlaması Düzenleyicisi** şimdi metin görünümünü pencerenin kenarlarına sabitleyebilirsiniz ve burada büyüdükçe ve tüm dört kırmızı ben-kirişleri Düzenleyicisi üst kısmındaki ve tıklatarak penceresiyle küçültür ayarlayın **4 kısıtlamalarıEkle** düğmesi:

[![Kısıtlamaları düzenleme](menu-images/appmenu06.png "kısıtlamaları düzenleme")](menu-images/appmenu06-large.png#lightbox)

Değişikliklerinizi kaydetmek için kullanıcı arabirimi tasarımı ve Xamarin.Mac projenizle değişiklikleri eşitlemek Mac için Visual Studio geri dönebilirsiniz. Şimdi uygulamayı başlatmak, metin görünüme bir metin yazın, seçmek ve açmak **Düzenle** menüsü:

![Menü öğelerini otomatik olarak etkin/devre dışı](menu-images/appmenu07.png "menü öğelerini otomatik olarak etkin/devre dışı")

Bildirim nasıl **Kes**, **kopyalama**, ve **Yapıştır** öğeleri otomatik olarak etkin ve tam işlevsel bir tek satır kod yazmadan. 

Ne anlama geliyor? Yerleşik (yukarıda gösterilen şekilde) kadar varsayılan menü öğelerini kablolu gelen eylemleri ön tanımlamasında, macOS parçası olan Cocoa kullanıcı arabirimi öğelerinin çoğunu kancaları belirli eylemler için yerleşik unutmayın (gibi `copy:`). Bu nedenle bir pencereye etkin eklenir ve karşılık gelen menü öğesi ya da bağlı öğeleri seçili olduğunda bu eylem otomatik olarak etkinleştirilir. Kullanıcı bu menü öğesi seçtiğinde, kullanıcı Arabirimi öğesine yerleşik işlevleri çağrılan ve yürütülen, tüm geliştirici müdahalesi olmadan.

### <a name="enabling-and-disabling-menus-and-items"></a>Menüler ve öğeleri devre dışı bırakma ve etkinleştirme

Varsayılan olarak bir kullanıcı olay her gerçekleştiğinde `NSMenu` otomatik olarak sağlar ve her bir menü görünür ve menü öğesi uygulamanın bağlama göre devre dışı bırakır. Bir öğe etkinleştirmek/devre dışı üç yolu vardır:

- **Otomatik menü etkinleştirme** -menü öğesi durumunda etkin `NSMenu` öğesi kablolu için yukarı eylemi yanıt veren bir uygun nesne bulabilirsiniz. Yerleşik bir kanca için olan gibi metin görünümünü yukarıdaki `copy:` eylem.
- **Özel Eylemler ve validateMenuItem:** - bağlı herhangi bir menü öğesi için bir [penceresi veya Görünüm denetleyicisi özel eylem](#Working-with-Custom-Window-Actions), ekleyebileceğiniz `validateMenuItem:` eylem el ile etkinleştirin veya menü öğelerini devre dışı bırakın.
- **El ile menü etkinleştirme** -el ile ayarlamanız `Enabled` her özellik `NSMenuItem` etkinleştirme veya ayrı ayrı her bir menü öğesi devre dışı.

Bir sistem seçilecek ayarlamak `AutoEnablesItems` özelliği bir `NSMenu`. `true` Otomatik (varsayılan davranış) ve `false` el ile gerçekleştirilir. 

> [!IMPORTANT]
> El ile menü etkinleştirme kullanmayı seçerseniz, menüsünü hiçbiri öğe olanlar gibi AppKit sınıflar tarafından denetlenen `NSTextView`, otomatik olarak güncelleştirilir. El ile kod tüm öğelerini devre dışı bırakma ve etkinleştirme sorumlu olacaktır.

#### <a name="using-validatemenuitem"></a>ValidateMenuItem kullanma

Bağlı herhangi bir menü öğesi için yukarıda belirtildiği gibi bir [penceresi ya da Görünüm denetleyicisi özel eylem](#Working-with-Custom-Window-Actions), ekleyebileceğiniz `validateMenuItem:` eylem el ile etkinleştirin veya menü öğelerini devre dışı bırakın.

Aşağıdaki örnekte, `Tag` özelliği, etkin/tarafından devre dışı menü öğesinin bir türde karar vermek için kullanılacak `validateMenuItem:` eylem, seçili metinde durumu temelinde bir `NSTextView`. `Tag` Özelliğinin ayarlandığı arabirimi Oluşturucu'da her bir menü öğesi için:

![Tag özelliği ayarı](menu-images/validate01.png "Tag özelliği ayarlama")

Ve görünüm denetleyicisi için eklenen aşağıdaki kodu:

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

Ne zaman bu kod çalışır ve hiçbir metin seçili olduğundan `NSTextView`, (bunlar görünümü denetleyici eylemleri için kablolu arabirimlerdir olsa bile) iki kaydırma menü öğelerini devre dışı bırakılır:

![Devre dışı gösteren öğeler](menu-images/validate02.png "gösteren devre dışı öğeler")

İki kaydırma menü öğeleri, metnin bir bölümünü seçilir ve menü yeniden kullanılabilir olacak:

![Etkin gösteren öğeler](menu-images/validate03.png "etkin öğeleri gösterme")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>Etkinleştirme ve kod menü öğelerinde yanıtlama

Yukarıdaki bizim kullanıcı Arabirimi tasarımı (örneğin, bir metin alanı), yalnızca belirli Cocoa kullanıcı arabirimi öğeleri ekleyerek anlatıldığı gibi birkaç varsayılan menü öğelerinin etkinleştirilecek ve kod yazmaya gerek kalmadan otomatik olarak işlev. Sonraki kendi C# kod bir menü öğesini etkinleştirmek ve kullanıcı seçtiğinde işlevselliği sağlamak için sunduğumuz Xamarin.Mac projeye ekleniyor bakalım.

Kullanıcı kullanabilmek için istediğimiz gibi varsayalım **açık** öğesi **dosya** menüsünde bir klasör seçin. Birçok farklı uygulama işlevi olmasını istediğiniz ve Ver penceresi veya kullanıcı Arabirimi öğesi için sınırlı olmayan olduğundan, bu bizim uygulama temsilciye işlemek üzere kod eklemek için ekleyeceğiz.

İçinde **çözüm bölmesi**, çift `AppDelegate.CS` dosyayı düzenlemek için açın:

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

Şimdi uygulamayı şimdi çalıştırmak ve açık **dosya** menüsü: 

![Dosya menüsünü](menu-images/appmenu09.png "Dosya menüsü")

Dikkat **açık** menü öğesini şimdi etkinleştirilir. Biz bu seçeneği belirlerseniz Aç iletişim kutusu görüntülenir:

![Açık bir iletişim kutusu](menu-images/appmenu10.png "açık bir iletişim kutusu")

Biz tıklarsanız **açık** düğmesi, bizim uyarı iletisi görüntülenir:

![Bir örnek iletişim iletisini](menu-images/appmenu11.png "bir örnek iletişim iletisini")

Burada anahtar satırı `[Export ("openDocument:")]`, onu bildiren `NSMenu` , bizim **AppDelegate** bir yönteme sahip `void OpenDialog (NSObject sender)` tepki veren `openDocument:` eylem. Yukarıda, unutmayın, **açık** menü öğesi otomatik olarak kablolu yukarı Bu eylem için varsayılan arabirim Oluşturucu olarak:

[![Ekli eylemleri görüntüleme](menu-images/defaultbar03.png "bağlı eylemleri görüntüleme")](menu-images/defaultbar03-large.png#lightbox)

Sonraki kendi menü, menü öğeleri ve Eylemler oluşturma ve yanıt kodu onlara bakalım.

### <a name="working-with-the-open-recent-menu"></a>Son menüyü Aç ile çalışma

Varsayılan olarak, **dosya** menü içeren bir **açık son** kullanıcı uygulamanızla açtıysa son birkaç dosyaları, öğe. Oluşturuyorsanız bir `NSDocument` Xamarin.Mac uygulama tabanlı bu menü sizin için otomatik olarak ele alınacaktır. Herhangi diğer Xamarin.Mac uygulama türü için yönetme ve bu menü öğesine el ile yanıt sorumlu olacaktır.

El ile işlemek için **açık son** menüsünde önce ihtiyaç duyacağınız yeni bir dosya açılır veya aşağıdaki kullanarak kaydedilmiş olduğunu bildirmek:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

Uygulamanızı kullanılmıyor olsa bile `NSDocuments`, kullanmaya devam `NSDocumentController` korumak için **açık son** göndererek menüsünde bir `NSUrl` dosyaya konumu ile `NoteNewRecentDocumentURL` yöntemi `SharedDocumentController`.

Ardından, geçersiz kılmak ihtiyacınız `OpenFile` kullanıcı seçer herhangi bir dosyayı açmak için uygulama temsilcinin yöntemi **açın son** menüsü. Örneğin:

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

Dönüş `true` dosya açılır, aksi takdirde döndürür `false` ve dosya açılamadı kullanıcıya yerleşik bir uyarı görüntülenir.

Dosya adı ve yolu döndürüldüğü çünkü **açık son** menüsünde, bir alanı içerebilir, düzgün bir şekilde oluşturmadan önce bu karakteri kaçırmak ihtiyacımız bir `NSUrl` veya bir hata iletisiyle karşılaşırsınız. Aşağıdaki kod ile bunu:

```csharp
filename = filename.Replace (" ", "%20");
```

Son olarak, oluşturduğumuz bir `NSUrl` işaret eden dosya ve uygulama bir yardımcı yöntemi temsilci yeni bir pencere açın ve dosyanın içine yüklemek için kullanın:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Her şeyi birlikte çekmek için bir örnek uygulama bakalım bir **AppDelegate.cs** dosyası:

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

Uygulama gereksinimlerinize bağlı olarak, aynı dosyayı aynı anda birden fazla pencerede açmak için kullanıcının istemeyebilirsiniz. Kullanıcı zaten açık olan bir dosya seçerse bizim örnek uygulamasında (araçtan **açın son** veya **açın...** menü öğeleri için), dosyayı içeren bir pencere öne getirilir.

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

Tasarladığımız bizim `ViewController` yolu dosyasında tutacak sınıf kendi `Path` özelliği. Ardından, uygulamadaki tüm açık pencereleri arasında döngüye gireceğiz. Dosya zaten windows birinde açık ise kullanarak tüm windows öne sağlanmıştır:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Eşleşme bulunamazsa, yüklenen dosya ile yeni bir pencere açılır ve dosyayı Not edilir **açık son** menüsü:

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

Yerleşik'olduğu gibi **ilk Yanıtlayıcı** standart menü öğeleri için önceden kablolu gelen eylemleri, yeni, özel eylemler oluşturabilir ve bunları arabirim Oluşturucu menü öğelerine bağlayabilirsiniz.

İlk olarak, özel bir eylem uygulamanızın penceresi denetleyicilerinden birinde tanımlayın. Örneğin:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Ardından, uygulamanın görsel taslak dosyasına çift tıklayarak **çözüm bölmesi** Xcode'da düzenlemek üzere açmak için kullanıcının arabirim Oluşturucu. Seçin **ilk Yanıtlayıcı** altında **uygulama Sahne**, ardından geçin **öznitelikleri denetçisi**:

![Öznitelikleri denetçisi](menu-images/action01.png "öznitelikleri denetçisi")

Tıklayın **+** düğme alttaki **öznitelikleri denetçisi** yeni bir özel eylem eklemek için:

![Yeni bir eylem ekleme](menu-images/action02.png "yeni bir eylem ekleme")

Bu pencere denetleyicinizde oluşturduğunuz özel eylem aynı adı verin:

![Eylem adı düzenleme](menu-images/action03.png "eylem adı düzenleme")

Control tuşuna tıklama ve bir menü öğesine sürükleyin **ilk Yanıtlayıcı** altında **uygulama Sahne**. Açılan listeden yeni oluşturduğunuz yeni bir eylem seçin (`defineKeyword:` Bu örnekte):

![Bir eylem ekleme](menu-images/action04.png "bir eylem ekleme")

Film şeridini değişiklikleri kaydetmek ve değişiklikleri eşitlemek Mac için Visual Studio geri dönün. Uygulamayı çalıştırırsanız, özel eylemine bağlı menü öğesini otomatik olarak etkin / (penceresinde açılırken eylemiyle göre) devre dışı ve menü öğesi seçilerek eylem ateşlenir:

[![Yeni Eylem test](menu-images/action05.png "yeni eylem test etme")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>Ekleme, düzenleme ve menüler siliniyor

Önceki bölümde anlatıldığı gibi bir Xamarin.Mac uygulamasını varsayılan menüleri ve menü öğelerini belirli kullanıcı Arabirimi denetimleri otomatik olarak etkinleştirin ve yanıt önceden belirlenmiş bir dizi ile birlikte gelir. Ayrıca, ayrıca etkinleştirir ve bu varsayılan öğeleri yanıt uygulamamız için kod ekleme gördük.

Bu bölümde biz ihtiyaç duymayacağımız menü öğelerini kaldırma, yeniden düzenleme menüleri ve yeni menüleri ve menü öğeleri eylemler ekleme görünecektir.

Çift **Main.storyboard** dosyası **çözüm bölmesi** düzenlemek üzere açın:

[![Xcode kullanıcı Arabiriminde düzenleme](menu-images/maint01.png "Xcode kullanıcı Arabiriminde düzenleme")](menu-images/maint01-large.png#lightbox)

Belirli Xamarin.Mac uygulamamız için varsayılan kullanacağız değil **görünümü** menüsü kaldırmak için kullanacağız. İçinde **arabirimi hiyerarşi** seçin **görünümü** ana menü çubuğu bir parçası olan menü öğesi:

![Görünüm menü öğesi seçilerek](menu-images/maint02.png "görüntüle menü öğesi seçilerek")

DELETE tuşuna basın veya menü silmek için Geri Al. Ardından, tüm öğelerin kullanılmasını kullanacağız olmayan **biçimi** menüsünü ve alt menülerinin altında çıkış kullanılacak olduğumuz öğeleri taşımak istediğiniz. İçinde **arabirimi hiyerarşi** aşağıdaki menü öğeleri seçin:

![Birden çok öğe vurgulama](menu-images/maint03.png "birden çok öğe vurgulama")

Üst altındaki öğeleri sürükleyin **menü** şu anda oldukları alt menüsünde:

[![Üst menüye menü öğeleri sürükleyerek](menu-images/maint04.png "üst menüye menü öğeleri sürükleme")](menu-images/maint04-large.png#lightbox)

Menünüzde gibi görünmelidir:

[![Yeni konumdaki öğeleri](menu-images/maint05.png "yeni konumdaki öğeleri")](menu-images/maint05-large.png#lightbox)

Sonraki sürükleyip **metin** altındaki alt menüyü **biçimi** menü arasında ana menü çubuğundaki yerleştirin **biçimi** ve **penceresi** menüler:

[![Metin menüsü](menu-images/maint06.png "metin menüsü")](menu-images/maint06-large.png#lightbox)

Altında geri dönelim **biçimi** menü ve delete **yazı tipi** alt menü öğesi. Ardından, **biçimi** menüsünde ve "Yazı tipi" yeniden adlandırın:

[![Yazı tipi menü](menu-images/maint07.png "yazı tipi menüsü")](menu-images/maint07-large.png#lightbox)

Ardından, özel bir menü seçildiklerinde, metin görüntüleme metni için otomatik olarak eklenen predefine tümce oluşturalım. Arama kutusuna üzerinde altındaki **kitaplığı denetçisi** türü "menüsünde." Bu, bulmak ve tüm menü UI öğeleri ile çalışmak daha kolay bulabilmesini sağlar:

![Kitaplık denetçisi](menu-images/maint08.png "kitaplığı denetçisi")

Şimdi github'dan bizim menü oluşturmak için aşağıdakileri yapın:

1. Sürükleme bir **menü öğesi** gelen **kitaplığı denetçisi** arasında menü çubuğu üzerine **metin** ve **penceresi** menüleri: 

    ![Kitaplıkta yeni bir menü öğesini seçerek](menu-images/maint10.png "Kitaplığı'nda yeni bir menü öğesini seçerek")
2. ' % S'öğesi "İfadeleri" yeniden adlandır: 

    [![Menü adı ayarlama](menu-images/maint09.png "menüsü adı ayarlama")](menu-images/maint09-large.png#lightbox)
3. Sonraki sürükleyin bir **menü** gelen **kitaplığı denetçisi**: 

    ![Kitaplıktan bir menüsünü seçerek](menu-images/maint11.png "kitaplıktan bir menü seçme")
4. Ardından drop **menü** yeni **menü öğesi** biz yalnızca oluşturulan ve "İfadeleri için" adını değiştirin: 

    [![Menü adı düzenleme](menu-images/maint12.png "menüsü adı düzenleme")](menu-images/maint12-large.png#lightbox)
5. Artık üç varsayılan Şimdi Yeniden Adlandır **menü öğeleri** "Adres", "Tarih" ve "Selamlama": 

    [![İfadeleri menü](menu-images/maint13.png "tümcecikleri menüsü")](menu-images/maint13-large.png#lightbox)
6. Dördüncü ekleyelim **menü öğesi** sürükleyerek bir **menü öğesi** gelen **kitaplığı denetçisi** ve "Signature" çağırma: 

    [![Menü öğesi adı düzenleme](menu-images/maint14.png "menü öğesi adı düzenleme")](menu-images/maint14-large.png#lightbox)
7. Menü çubuğu için değişiklikleri kaydedin.

Artık müşterilerimize yeni menü öğesi için C# kodu sunulur, böylece özel eylemler kümesi oluşturalım. Xcode'da şimdi geçin **Yardımcısı** görüntüle:

[![Gerekli eylemleri oluşturma](menu-images/maint15.png "gerekli eylemleri oluşturma")](menu-images/maint15-large.png#lightbox)

Şimdi aşağıdakileri yapın:

1. Gelen denetim Sürükle **adresi** menü öğesine **AppDelegate.h** dosya.
2. Anahtar **bağlantı** için yazın **eylem**: 

    [![Eylem türü seçme](menu-images/maint17.png "eylem türü seçme")](menu-images/maint17-large.png#lightbox)
3. Girin bir **adı** "phraseAddress" tuşuna basın ve **Connect** yeni eylem oluşturmak için: 

    [![Eylem yapılandırma](menu-images/maint18.png "eylemini yapılandırma")](menu-images/maint18-large.png#lightbox)
4. Yukarıdaki adımları yineleyin **tarih**, **Karşılama**, ve **imza** menü öğeleri: 

    [![Tamamlanan Eylemler](menu-images/maint19.png "tamamlanan Eylemler")](menu-images/maint19-large.png#lightbox)
5. Menü çubuğu için değişiklikleri kaydedin.

Sonraki prizine bizim metin görünümü için biz içeriğini koddan ayarlayabilmesi için oluşturmamız gerekir. Seçin **ViewController.h** dosyası **Yardımcısı Düzenleyicisi** adlı yeni bir çıkış oluşturup `documentText`:

[![Bir çıkış oluşturma](menu-images/maint20.png "prizine oluşturma")](menu-images/maint20-large.png#lightbox)

Değişiklikler xcode'dan eşitlemek Mac için Visual Studio dönün. Sonraki düzenleme **ViewController.cs** dosyasını açıp aşağıdaki gibi görünmesi:

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

Bu bizim metin görünümünü dışında metnini sunan `ViewController` sınıfı ve ne zaman penceresi kazanır veya odağı kaybettiğinde uygulama temsilci bildirir. Şimdi Düzenle **AppDelegate.cs** dosyasını açıp aşağıdaki gibi görünmesi:

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

Burada yaptığımız `AppDelegate` kısmi bir sınıfın eylemleri ve arabirim Oluşturucu'da tanımladığımız çıkışlar kullanabiliriz. Ayrıca kullanıma sunuyoruz bir `textEditor` hangi penceresi şu anda odağı, izlemek için.

Aşağıdaki yöntemlerden bizim Özel menü ve menü öğeleri işlemek için kullanılır:

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

Şimdi uygulamamız, tüm öğeleri içinde çalıştırıyoruz **tümcecik** menü etkin olacaktır ve verme deyimi seçildiğinde metin görünümüne ekler:

![Çalıştıran uygulama örneği](menu-images/maint21.png "çalıştıran uygulama örneği")

Uygulama menü çubuğunun ile çalışmanın temelleri sahibiz, özel bağlamsal menü oluşturma sırasında göz atalım.

### <a name="creating-menus-from-code"></a>Koddan menüleri oluşturma

Menüleri ve menü öğeleri ile Xcode'un arabirim Oluşturucu oluşturmaya ek olarak, bir Xamarin.Mac uygulamasını oluşturmak, değiştirmek veya koddan bir menü, alt menü veya menü öğesini kaldırmak için gereken zamanı zamanlar olabilir.

Aşağıdaki örnekte, bir sınıf üzerinde halindeyken dinamik olarak oluşturulacak alt menüleri ve menü öğeleri ile ilgili bilgileri tutmak için oluşturulur:

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

Bu sınıfla tanımlanmış, aşağıdaki yordam bir koleksiyonunu ayrıştırmaz `LanguageFormatCommand`nesneleri ve yinelemeli olarak yapı yeni menüleri ve menü öğeleri, geçirilen (arabirimi Oluşturucu'da oluşturulan) mevcut menüsü altına ekleyerek:

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

Herhangi `LanguageFormatCommand` boş nesnesi `Title` özelliği, bu yordamı oluşturur bir **ayırıcı menü öğesi** (ince gri çizgi) menüsünde bölümler arasında:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Bir başlık sağlanırsa, başlıklı yeni bir menü öğesi oluşturulur:

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Varsa `LanguageFormatCommand` nesnesini içeren alt `LanguageFormatCommand` nesneler, alt menüyü oluşturulur ve `AssembleMenu` yinelemeli olarak bu menüyü oluşturmak için çağrılan bir yöntemdir:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Alt menü yok. tüm yeni menü öğesi için kullanıcı tarafından seçilen menü öğesini işlemek için kod eklenir:

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Menü oluşturma testi

Tüm varken, yukarıdaki kod, aşağıdaki koleksiyonunu `LanguageFormatCommand` nesneleri oluşturulmuş:

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

Koleksiyon geçirilir, `AssembleMenu` işlevi (ile **biçimi** menüsünde temel olarak ayarlayın), aşağıdaki dinamik menüler ve menü öğeleri oluşturulacak:

![Çalışan uygulamaya yeni menü öğelerinde](menu-images/dynamic01.png "çalışan uygulamada yeni menü öğesi")

#### <a name="removing-menus-and-items"></a>Menüler ve öğeleri kaldırma

Herhangi bir menü veya menü öğesi, uygulamanın kullanıcı arabiriminden kaldırmanız gerekirse, kullanabileceğiniz `RemoveItemAt` yöntemi `NSMenu` sınıfı yalnızca kaldırılacak öğenin dizini temel sıfır vererek.

Örneğin, menüleri ve menü öğeleri yukarıdaki yordamı tarafından oluşturulan kaldırmak için aşağıdaki kodu kullanabilirsiniz:

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

Bunlar dinamik olarak kaldırılmasını sağlayın Yukarıdaki kod söz konusu olduğunda Xcode'un uygulamasındaki işlemlerine ve arabirim Oluşturucu ilk dört menü öğelerinden oluşturulur.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>Bağlam menüleri

Bağlam menüleri kullanıcı tıkladığı veya denetim tıklamasıyla bir pencere öğesinde görünür. Varsayılan olarak, bazı macOS önceden oluşturulan UI öğeleri bağlam menüleri (metin görünümü gibi) bağlı sahiptir. Ancak, ne zaman bir pencereye ekledik bir kullanıcı Arabirimi öğesi için kendi özel bağlam menüleri oluşturmak istiyoruz zamanlar olabilir.

Düzenleyelim bizim **Main.storyboard** dosyasını Xcode'da ve ekleme bir **penceresi** bizim tasarım penceresini ayarlamak kendi **sınıfı** "NSPanel" için **kimlikdenetçisi**, yeni bir **Yardımcısı** öğesinin **penceresi** menüsünde ve penceresi kullanarak yeni ekleme bir **Göster ü**:

[![Ayar segue türü](menu-images/context01.png "segue türünü ayarlama")](menu-images/context01-large.png#lightbox)

Şimdi aşağıdakileri yapın:

1. Sürükle bir **etiket** gelen **kitaplığı denetçisi** üzerine **paneli** penceresi ve "Özelliğine" metnini ayarlayın: 

    [![Etiketin değeri düzenleme](menu-images/context03.png "etiketin değeri düzenleme")](menu-images/context03-large.png#lightbox)
2. Sonraki sürükleyin bir **menü** gelen **kitaplığı denetçisi** hiyerarşisini görüntüle ve varsayılan menü öğeleri yeniden adlandırma üç görünüm denetleyicisine **belge**, **metin**  ve **yazı tipi**:

    [![Gerekli menü öğelerini](menu-images/context02.png "gerekli menü öğeleri")](menu-images/context02-large.png#lightbox)
3. Artık denetim öğesinden sürükleyin **özelliği etiketi** üzerine **menü**:

    [![Bir segue oluşturmak için sürükleme](menu-images/context04.png "bir segue oluşturmak için sürükleme")](menu-images/context04-large.png#lightbox)
4. Açılan iletişim kutusundan seçin **menü**: 

    ![Ayar segue türü](menu-images/context05.png "segue türünü ayarlama")
5. Gelen **kimlik denetçisi**, "PanelViewController" görünümü denetleyicinin sınıfa ayarlayın: 

    [![Ayar segue sınıfı](menu-images/context10.png "ayar segue sınıfı")](menu-images/context10-large.png#lightbox)
6. Eşitlemek Mac için Visual Studio geri dönmek ve arabirim Oluşturucu döndürür.
7. Geçiş **Yardımcısı Düzenleyicisi** seçip **PanelViewController.h** dosya.
8. İçin eylem oluşturma **belge** adlı menü öğesi `propertyDocument`: 

    [![Eylem yapılandırma](menu-images/context06.png "eylemini yapılandırma")](menu-images/context06-large.png#lightbox)
9. Kalan menü öğeleri için oluşturma eylemleri yineleyin: 

    [![Gerekli eylemleri](menu-images/context07.png "gerekli eylemleri")](menu-images/context07-large.png#lightbox)
10. Son olarak bir çıkış için oluşturma **özelliği etiketi** adlı `propertyLabel`: 

    [![Çıkış yapılandırma](menu-images/context08.png "çıkışı yapılandırma")](menu-images/context08-large.png#lightbox)
11. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Düzen **PanelViewController.cs** dosyasını açıp aşağıdaki kodu ekleyin:

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

Uygulamayı çalıştırmak ve özellik etiketi panelinde sağ tıklayın, bizim özel bağlam menüsü artık göreceğiz. Seçin ve menüden öğesi, etiketin değeri değiştirilir:

![Çalışan bağlamsal menüyü](menu-images/context09.png "çalıştırma bağlam menüsü")

Sonraki durum çubuğu menüleri oluşturma sırasında bakalım.

## <a name="status-bar-menus"></a>Durum çubuğu menüleri

Durum çubuğu menüleri, bir menü veya bir uygulamanın durumunu yansıtan bir görüntü gibi kullanıcının etkileşim sağlamak durum menü öğeleri veya geri bildirim koleksiyonu görüntüler. Uygulama arka planda çalışıyor olsa bile bir uygulamanın durum çubuğuna menü etkinleştirildiğinde ve etkin. Sistem genelinde durum çubuğu uygulaması menü çubuğunun sağ tarafında bulunan ve yalnızca durum çubuğunun macOS şu anda kullanılabilir.

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

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` Sistem genelinde durum çubuğuna erişim sağlıyor. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` Yeni bir durum çubuğu öğesi oluşturur. Buradan bir menü ve bir dizi menü öğelerini oluşturma ve oluşturduğumuz durum çubuğu öğesi menüsü ekleme. 

Uygulama çalıştırıyoruz, yeni durum çubuğu öğesi görüntülenir. Menüden bir öğenin metin görünümünde metin değişir: 

![Çalışan durum çubuğu menüsü](menu-images/statusbar01.png "çalıştırma durum çubuğu menüsü")

Ardından, özel yerleştirme menü öğelerini oluşturmakta bakalım.

## <a name="custom-dock-menus"></a>Özel yerleştirme menü

Dock menüsünü kullanıcı tıkladığı veya denetim-dock uygulama simgesine tıklama, Mac uygulaması görünür:

![Özel yerleştirme menü](menu-images/dock01.png "özel yerleştirme menü")

Aşağıdakileri yaparak bir özel dock menüsünü uygulamamız için oluşturalım:

1. Mac için Visual Studio'da uygulamanın projeyi sağ tıklatın ve **Ekle** > **yeni dosya...** Yeni dosya iletişim kutusundan seçin **Xamarin.Mac** > **boş arabirim tanımı**, "DockMenu" kullanmak **adı** tıklatıp **yeni**  yeni düğme **DockMenu.xib** dosyası:

    ![Boş arabirim tanımı ekleme](menu-images/dock02.png "boş arabirim tanımı ekleme")
2. İçinde **çözüm bölmesi**, çift **DockMenu.xib** dosyayı Xcode'da düzenlemek üzere açın. Yeni bir **menü** şu öğeler: **adresi**, **tarih**, **Karşılama**, ve **imzası** 

    [![Kullanıcı arabirimini yerleştirme](menu-images/dock03.png "kullanıcı arabirimini düzenleme")](menu-images/dock03-large.png#lightbox)
3. Ardından, sunduğumuz yeni menü öğesi özel bizim menüde için oluşturduğumuz mevcut eylemlerimiz için yeniliklerden [ekleme, düzenleme ve siliniyor menülerindeki](#Adding,_Editing_and_Deleting_Menus) yukarıdaki bölümde. Geçiş **bağlantı denetçisi** seçip **ilk Yanıtlayıcı** içinde **arabirimi hiyerarşi**. Ekranı aşağı kaydırın ve bulma `phraseAddress:` eylem. Bu eylem için daire bir satır sürükleyin **adresi** menü öğesi:

    [![İletilen bir eylem yukarı sürükleyerek](menu-images/dock04.png "kablo eylem'kurmak için sürükleme")](menu-images/dock04-large.png#lightbox)
4. Tüm ilgili eylemlerini eklemeden menü öğeleri için tekrarlayın: 

    [![Gerekli eylemleri](menu-images/dock05.png "gerekli eylemleri")](menu-images/dock05-large.png#lightbox)
5. Ardından, **uygulama** içinde **arabirimi hiyerarşi**. İçinde **bağlantı denetçisi**, dairenin bir satıra sürükleyin `dockMenu` oluşturduğumuz menüsüne çıkışı:

    [![Kablo çıkışı yukarı sürükleyerek](menu-images/dock06.png "çıkışı'kurmak kablo sürükleme")](menu-images/dock06-large.png#lightbox)
6. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.
7. Çift **Info.plist** dosyayı düzenlemek için açın: 

    [![Info.plist dosyasını düzenleyerek](menu-images/dock07.png "Info.plist dosyasını düzenleme")](menu-images/dock07-large.png#lightbox)
8. Tıklayın **kaynak** sekmesinde ekranın alt kısmındaki: 

    [![Kaynak Görünümü seçerek](menu-images/dock08.png "kaynak görünümü seçme")](menu-images/dock08-large.png#lightbox)
9. Tıklayın **yeni giriş Ekle**, yeşil artı düğmesine tıklayın, "AppleDockMenu" özellik adına ve "DockMenu" (uzantısı olmadan yeni bizim .xib dosya adı) değeri ayarlayın: 

    [![DockMenu öğesi ekleme](menu-images/dock09.png "DockMenu öğesi ekleme")](menu-images/dock09-large.png#lightbox)

Uygulamamızı çalıştırmak ve Dock simgesini sağ tıklatın, artık müşterilerimize yeni menü öğeleri görüntülenir:

![Bir örneğini çalıştıran dock menüsünü](menu-images/dock10.png "çalıştıran dock menüsünü örneği")

Menüden özel öğelerden birini seçtiğinizde, bizim metin görünümünü metinde değiştirilecek.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>Açılır düğme ve aşağı açılır listeler

Bir açılır düğme seçili bir öğe görüntüler ve kullanıcı tarafından tıklandığında seçmek için seçenekleri bir listesini sunar. Aşağı açılır liste, şu anki görevini bağlamına özgü komutların seçmek için genellikle kullanılan açılır düğme türüdür. Her ikisi de, herhangi bir pencere içinde görünebilir.

Aşağıdakileri yaparak uygulamamız için özel açılır düğme oluşturalım:

1. Düzenleme **Main.storyboard** Xcode ve sürükleme dosyasında bir **açılan düğmesine** gelen **kitaplığı denetçisi** üzerine **paneli** oluşturduğumuz penceresi [bağlam menüleri](#Contextual_Menus) bölümü: 

    [![Bir açılan düğmeye ekleme](menu-images/popup01.png "açılır düğme ekleme")](menu-images/popup01-large.png#lightbox)
2. Yeni bir menü öğesi ekleme ve öğeleri başlıkları açılan olarak ayarlayın: **adresi**, **tarih**, **Karşılama**, ve **imzası** 

    [![Menü öğelerini yapılandırma](menu-images/popup02.png "menü öğelerini yapılandırma")](menu-images/popup02-large.png#lightbox)
3. Ardından, sunduğumuz yeni menü öğesi özel bizim menüde oluşturduğumuz mevcut eylemleri yeniliklerden [ekleme, düzenleme ve siliniyor menülerindeki](#Adding,_Editing_and_Deleting_Menus) yukarıdaki bölümde. Geçiş **bağlantı denetçisi** seçip **ilk Yanıtlayıcı** içinde **arabirimi hiyerarşi**. Ekranı aşağı kaydırın ve bulma `phraseAddress:` eylem. Bu eylem için daire bir satır sürükleyin **adresi** menü öğesi: 

    [![İletilen bir eylem yukarı sürükleyerek](menu-images/popup03.png "kablo eylem'kurmak için sürükleme")](menu-images/popup03-large.png#lightbox)
4. Tüm ilgili eylemlerini eklemeden menü öğeleri için tekrarlayın: 

    [![Tüm gerekli eylemleri](menu-images/popup04.png "tüm gerekli eylemleri")](menu-images/popup04-large.png#lightbox)
5. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.

Artık uygulamamızı çalıştırmak ve açılır penceresinden bir öğe seçin, bizim metin görünümünde metin değişir:

![Bir örneğini çalıştıran açılan](menu-images/popup05.png "çalıştıran açılan örneği")

Oluşturun ve açılır listeleri ile açılır düğmeler tam aynı şekilde çalışır. Bizim bağlamsal menüde için yaptığımız gibi mevcut eylemini ekleme yerine, kendi özel eylemler oluşturabilirsiniz [bağlam menüleri](#Contextual_Menus) bölümü.

## <a name="summary"></a>Özet

Bu makalede, menüleri ve menü öğeleri bir Xamarin.Mac uygulamasını çalışma ayrıntılı bir bakış duruma getirdi. İlk biz uygulamanın menü çubuğunda, biz durum çubuğu menüleri incelenir ve özel yerleştirme menü yanındaki bağlam menüleri oluşturma konusunda incelemiştik incelenir. Son olarak, açılır menüler ve aşağı açılır liste kapsamdaki.


## <a name="related-links"></a>İlgili bağlantılar

- [MacMenus (örnek)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [İnsan Arabirimi yönergelerine - menüler](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Uygulama menüleri ve açılır listeleri giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
