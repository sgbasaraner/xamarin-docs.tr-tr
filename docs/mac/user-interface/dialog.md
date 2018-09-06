---
title: Xamarin.Mac iletişim kutularında
description: Bu makale, iletişim kutuları ve kalıcı bir Xamarin.Mac uygulamasını Windows'ta çalışmayı kapsar. Bu durum, Xcode ve arabirim Oluşturucu standart iletişim kutuları ile çalışma ve C# kodunda bu denetimler ile etkileşim kurma kalıcı pencere oluşturma işlemini açıklar.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 2f28b52b4904b73f97cd9da575e90ef583e443da
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780602"
---
# <a name="dialogs-in-xamarinmac"></a>Xamarin.Mac iletişim kutularında

Bir Xamarin.Mac uygulamasında çalışırken, C# ve .NET ile aynı iletişim kutuları ve kalıcı Windows erişiminiz olan, çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Xcode'un kullanabileceğiniz _arabirim Oluşturucu_ oluşturmak ve korumak, kalıcı Windows (veya isteğe bağlı olarak bunları doğrudan C# kodu oluşturmak için).

Bir iletişim kutusu, yanıt olarak bir kullanıcı eylemi görünür ve yolları kullanıcılar eylemi tamamlamak genellikle sağlar. Bir iletişim kutusu, kapatılabilmesi için kullanıcıdan bir yanıt gerektirir.

Windows (örneğin, uygulama devam etmeden önce kapatıldı gereken bir dışarı aktarma iletişim) kalıcı veya geçici bir durumda (örneğin, birden çok belge aynı anda açık olan bir metin düzenleyicisine) kullanılabilir olabilir.

[![](dialog-images/dialog03.png "Açık bir iletişim kutusu")](dialog-images/dialog03.png#lightbox)

Bu makalede, biz bir Xamarin.Mac uygulamasında iletişim kutuları ve kalıcı Windows ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>İletişim kutuları giriş

Bir iletişim kutusu, yanıt olarak bir kullanıcı eylemi (örneğin, dosya kaydetme) görüntülenir ve bu eylemi tamamlamak bir yol sağlar. Bir iletişim kutusu, kapatılabilmesi için kullanıcıdan bir yanıt gerektirir.

Apple'nın göre bir iletişim kutusu için üç yol vardır:

- **Belge kalıcı** -bir belge kalıcı iletişim kullanıcı onu kapatılmadan belirli bir belge içinde başka bir şey yapmasını engeller.
- **Uygulama kalıcı** - bir uygulama kalıcı iletişim kutusu, kapatılmadan uygulamayla etkileşim kullanıcı engeller.
- **Engelleyici olmayan** A modsuz iletişim kutusu kullanıcıların yine belge penceresi ile etkileşim sırasında iletişim ayarlarını değiştirmesine olanak sağlar.

### <a name="modal-window"></a>Kalıcı pencere

Herhangi bir standart `NSWindow` kalıcı olarak görüntüleyen özelleştirilmiş bir iletişim kutusu olarak kullanılabilir:

[![](dialog-images/modal01.png "Bir örnek kalıcı pencere")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>Belge kalıcı iletişim kutusu sayfaları

A _sayfası_ kullanıcılar, iletişim kutusunu kapatmak kadar pencere ile etkileşim engelleyen bir belirtilen belge penceresi iliştirildiği kalıcı bir iletişim kutusu. Bir sayfa, dolayısıyla ve herhangi bir anda yalnızca bir sayfa için bir pencere aç olabilir penceresine eklenir.

[![](dialog-images/sheet08.png "Bir örnek kalıcı sayfası")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Tercihler Windows

Kullanıcının sık değişmeyen uygulama ayarları içeren bir kalıcı olmayan iletişim tercihleri penceredir. Tercihler Windows genellikle farklı ayar grupları arasında geçiş yapmak kullanıcıya izin veren bir araç çubuğu içerir:

[![](dialog-images/dialog02.png "Bir örnek tercih penceresi")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>Aç iletişim kutusu

Aç iletişim kullanıcıları bulmak ve bir uygulamada bir öğeyi açmak için tutarlı bir yol sunar:

[![](dialog-images/dialog03.png "Bir açık iletişim kutusu")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>Yazdırma ve Sayfa Yapısı iletişim kutuları

Standart yazdırma macOS sağlar ve kullandıkları her uygulama deneyimi sayfasında Kurulum uygulamanız görüntüleyebilir ve böylece kullanıcılar, tutarlı bir yazdırma olabilir iletişim kutuları.

Yazdır iletişim, her iki kayan ücretsiz olarak iletişim kutusu görüntülenebilir:

[![](dialog-images/print01.png "Yazdırma iletişim kutusu")](dialog-images/print01.png#lightbox)

Veya bir sayfa olarak görüntülenebilir:

[![](dialog-images/print02.png "Bir yazdırma sayfası")](dialog-images/print02.png#lightbox)

Sayfa Yapısı iletişim, her iki kayan ücretsiz olarak iletişim kutusu görüntülenebilir:

[![](dialog-images/print03.png "Sayfa Yapısı iletişim")](dialog-images/print03.png#lightbox)

Veya bir sayfa olarak görüntülenebilir:

[![](dialog-images/print04.png "Bir sayfa kurulum sayfası")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>Kaydet iletişim kutuları

Kaydet iletişim kutusu, kullanıcılara bir uygulamada bir öğesini kaydetmek için tutarlı bir yol sağlar. Kaydet iletişim iki durum vardır: **Minimal** (daraltılmış olarak da bilinir):

[![](dialog-images/save01.png "İletişim kaydetme")](dialog-images/save01.png#lightbox)

Ve **Genişletilmiş** durumu:

[![](dialog-images/save02.png "Bir genişletilmiş Kaydet iletişim kutusu")](dialog-images/save02.png#lightbox)

**Minimal** Kaydet iletişim bir sayfa olarak görüntülenebilir:

[![](dialog-images/save03.png "Minimal bir sayfa Kaydet")](dialog-images/save03.png#lightbox)

Mümkün olduğunca **Genişletilmiş** Kaydet iletişim:

[![](dialog-images/save04.png "Genişletilmiş bir sayfayı Kaydet")](dialog-images/save04.png#lightbox)

Daha fazla bilgi için [iletişim kutuları](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Kalıcı pencere projeye ekleme

Ana belge penceresi yanı sıra, kullanıcı tercihlerine veya denetçisi bölmeleri gibi diğer türleri windows görüntülemek bir Xamarin.Mac uygulamasını gerekebilir.

Yeni bir pencere eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**açın `Main.storyboard` Xcode'un arabirimi Oluşturucusu'nda düzenlemek için dosya.
2. Yeni bir sürükleyin **görünüm denetleyicisi** tasarım yüzeyine içine:

    [![](dialog-images/new01.png "Kitaplıktan bir görünüm denetleyicisi seçme")](dialog-images/new01.png#lightbox)
3. İçinde **kimlik denetçisi**, girin `CustomDialogController` için **sınıf adı**: 

    [![](dialog-images/new02.png "Sınıf adı ayarlama")](dialog-images/new02.png#lightbox)
4. Mac için Visual Studio'ya geçmek, Xcode ile eşitleyin ve oluşturmak izin `CustomDialogController.h` dosya.
5. Xcode için geri dönün ve Arabiriminizin tasarlama: 

    [![](dialog-images/new03.png "Xcode kullanıcı Arabiriminde tasarlama")](dialog-images/new03.png#lightbox)
6. Oluşturma bir **kalıcı ü** iletişim kutusunun penceresi için bir iletişim kutusu açılır UI öğesinden yeni bir görünüm denetleyicisi denetimi sürükleyerek uygulamanızı ana penceresinde. Ata **tanımlayıcı** `ModalSegue`: 

    [![](dialog-images/new06.png "Kalıcı segue")](dialog-images/new06.png#lightbox)
6. Tüm kablo yukarı **eylemleri** ve **çıkışlar**: 

    [![](dialog-images/new04.png "Eylem yapılandırma")](dialog-images/new04.png#lightbox)
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Olun `CustomDialogController.cs` dosya görünüm aşağıdaki gibi:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Bu kod, başlık ve açıklama iletişim kutusunun ayarlamak için bazı özellikleri ve iletişim kutusunu iptal edildi veya kabul tepki vermek için birkaç olayları gösterir.

Ardından, Düzenle `ViewController.cs` dosya, geçersiz kılma `PrepareForSegue` yöntemi ve aşağıdaki gibi görünmesi:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

Bu kod, bizim iletişim için Xcode'un arabirimi Oluşturucusu'nda tanımladığımız segue başlatır ve başlık ve açıklama ayarlar. Ayrıca, kullanıcının yaptığı iletişim kutusunda seçim işler.

Uygulamamızı çalıştırmak ve özel iletişim kutusunu görüntüle:

[![](dialog-images/new05.png "Örnek bir iletişim kutusu")](dialog-images/new05.png#lightbox)

Bir Xamarin.Mac uygulamasını Windows'da kullanma hakkında daha fazla bilgi için lütfen bkz. bizim [Windows ile çalışan](~/mac/user-interface/window.md) belgeleri.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Özel bir sayfa oluşturma

A _sayfası_ kullanıcılar, iletişim kutusunu kapatmak kadar pencere ile etkileşim engelleyen bir belirtilen belge penceresi iliştirildiği kalıcı bir iletişim kutusu. Bir sayfa, dolayısıyla ve herhangi bir anda yalnızca bir sayfa için bir pencere aç olabilir penceresine eklenir. 

Xamarin.Mac bir özel sayfası oluşturmak için şimdi aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**açın `Main.storyboard` Xcode'un arabirimi Oluşturucusu'nda düzenlemek için dosya.
2. Yeni bir sürükleyin **görünüm denetleyicisi** tasarım yüzeyine içine:

    [![](dialog-images/new01.png "Kitaplıktan bir görünüm denetleyicisi seçme")](dialog-images/new01.png#lightbox)
2. Kullanıcı arabiriminizi tasarım:

    [![](dialog-images/sheet01.png "Kullanıcı Arabirimi tasarımı")](dialog-images/sheet01.png#lightbox)
3. Oluşturma bir **sayfa ü** yeni görünüm denetleyicisi için ana penceresinde: 

    [![](dialog-images/sheet02.png "Sayfa segue türü seçme")](dialog-images/sheet02.png#lightbox)
4. İçinde **kimlik denetçisi**, görünüm denetleyicinin adı **sınıfı** `SheetViewController`: 

    [![](dialog-images/sheet03.png "Sınıf adı ayarlama")](dialog-images/sheet03.png#lightbox)
5. Gerekli tanımlamak **çıkışlar** ve **Eylemler**: 

    [![](dialog-images/sheet04.png "Gerekli Eylemler ve çıkışlar tanımlama")](dialog-images/sheet04.png#lightbox)
6. Değişikliklerinizi kaydetmek ve eşitlemek Mac için Visual Studio geri dönün.

Ardından, Düzenle `SheetViewController.cs` dosyasını açıp aşağıdaki gibi görünmesi:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Ardından, Düzenle `ViewController.cs` dosya, Düzen `PrepareForSegue` yöntemi ve aşağıdaki gibi görünmesi:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

Uygulamamızı çalıştırmak ve sayfasını açın, penceresine eklenir:

[![](dialog-images/sheet08.png "Bir örnek sayfası")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Tercihler iletişim kutusu oluşturma

Biz arabirim Oluşturucu tercih görünümünde yerleştirme önce biz değiştirme işlemini yerine getiremiyorsa tercihleri için bir özel segue türü eklemeniz gerekir. Projenize yeni bir sınıf ekleyin ve onu çağırmak `ReplaceViewSeque `. Sınıf düzenleyin ve aşağıdaki gibi görünmesi:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

Oluşturulan özel segue ile bizim tercihleri işlemek için Xcode'un arabirim oluşturucu içinde yeni bir pencere ekleyebiliriz.

Yeni bir pencere eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**açın `Main.storyboard` Xcode'un arabirimi Oluşturucusu'nda düzenlemek için dosya.
2. Yeni bir sürükleyin **penceresi denetleyicisi** tasarım yüzeyine içine:

    [![](dialog-images/pref01.png "Pencere denetleyicisi kitaplıktan Seç")](dialog-images/pref01.png#lightbox)
3. Pencerenin yakın düzenleme **menü çubuğu** Tasarımcısı:

    [![](dialog-images/pref02.png "Yeni pencere ekleme")](dialog-images/pref02.png#lightbox)
4. Tercih görünümünüzde sekmeleri olacaktır gibi ekli görünüm denetleyicisi kopyalarını oluşturun:

    [![](dialog-images/pref03.png "Gerekli görünüm denetleyicileri ekleme")](dialog-images/pref03.png#lightbox)
5. Yeni bir sürükleyin **araç çubuğu denetleyicisi** gelen **Kitaplığı**:

    [![](dialog-images/pref04.png "Bir araç çubuğu denetleyicisi kitaplıktan seçin")](dialog-images/pref04.png#lightbox)
6. Ve penceresinde bir tasarım yüzeyine bırakın:

    [![](dialog-images/pref05.png "Yeni araç çubuğu denetleyici ekleme")](dialog-images/pref05.png#lightbox)
7. Tasarım, araç çubuğunun düzenini:

    [![](dialog-images/pref06.png "Araç çubuğu düzeni")](dialog-images/pref06.png#lightbox)
8. Control tuşuna tıklama ve her birinden sürükleyin **araç çubuğu düğmesi** yukarıda oluşturduğunuz görünümler. Seçin bir **özel** ü türü:

    [![](dialog-images/pref07.png "Ayar segue türü")](dialog-images/pref07.png#lightbox)
9. Yeni Segue seçin ve ayarlayın **sınıfı** için `ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Ayar segue sınıfı")](dialog-images/pref08.png#lightbox)
10. İçinde **Menü Tasarımcısı** tasarım yüzeyinde uygulama menüsünden seçeneğini **tercihleri...** control tuşuna tıklama ve oluşturmak için Tercihler penceresine sürükleyin, bir **Göster** ü:

    [![](dialog-images/pref09.png "Ayar segue türü")](dialog-images/pref09.png#lightbox)
11. Değişikliklerinizi kaydetmek ve eşitlemek Mac için Visual Studio geri dönün.

Kodu çalıştırmak ve seçeneğini belirlerseniz **tercihleri...**  gelen **uygulama menüsü**, penceresi görüntülenir:

[![](dialog-images/pref10.png "Bir örnek tercihleri penceresini")](dialog-images/pref10.png#lightbox)

Windows ve araç çubuklarını ile çalışma hakkında daha fazla bilgi için lütfen bkz. bizim [Windows](~/mac/user-interface/window.md) ve [araç çubukları](~/mac/user-interface/toolbar.md) belgeleri.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Kaydetme ve yükleme tercihleri

Bir normal macOS uygulama, kullanıcının herhangi bir uygulamanın kullanıcı tercihleri için değişiklik yaptığında bu değişiklikleri otomatik olarak kaydedilir. Xamarin.Mac uygulamasında, bu durumu çözmek için en kolay yolu olan sistem genelinde tüm kullanıcının tercihlerini yönetin ve paylaşın için tek bir sınıf oluşturmak için.

İlk olarak, yeni bir ekleme `AppPreferences` projeye sınıf ve devralınan `NSObject`. Tercihler kullanmak üzere tasarlanmış [veri bağlama ve anahtar-değer kodlaması](~/mac/app-fundamentals/databinding.md) oluşturma işlemini yapacak ve tercih koruma forms çok daha kolaydır. Basit veri türleri, az miktarda tercihleri oluşacak olduğundan, yerleşik kullanmak `NSUserDefaults` depolamak ve değerleri almak için.

Düzen `AppPreferences.cs` dosyasını açıp aşağıdaki gibi görünmesi:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLangauge")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLangauge", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLangauge");
                SaveInt ("DefaultLangauge", value, true);
                DidChangeValue ("DefaultLangauge");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

Bu sınıf gibi birkaç Yardımcısı yordamlarını içeren `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, vb. ile çalışma yapmak `NSUserDefaults` daha kolay. Ayrıca, bu yana `NSUserDefaults` işlemek için yerleşik bir yol yok `NSColors`, `NSColorToHexString` ve `NSColorFromHexString` yöntemleri renkleri web tabanlı onaltılık dizeye dönüştürmek için kullanılır (`#RRGGBBAA` burada `AA` alfa saydamlık) olabilir. kolayca depolanan ve alınan.

İçinde `AppDelegate.cs` dosya, bir örneğini oluşturmak **AppPreferences** uygulama genelinde kullanılan nesnesi:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>Tercih görünümleri kablolama tercihleri

Ardından, kullanıcı Arabirimi öğeleri tercih penceresi ve yukarıda oluşturulan görünümler tercih sınıfı bağlanın. Arabirim Oluşturucu tercih görünüm denetleyicisi seçin ve geçiş **kimlik denetçisi**, denetleyici için özel bir sınıf oluşturun: 

[![](dialog-images/prefs12.png "Kimlik denetçisi")](dialog-images/prefs12.png#lightbox)

Değişikliklerinizi eşitleyin ve düzenleme için yeni oluşturulan sınıfın açmak Mac için Visual Studio için dönün. Aşağıdaki gibi sınıf yapın:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Bu sınıf, burada iki şey yapmış dikkat edin: ilk olarak, bir yardımcı yoktur `App` erişme özelliğini **AppDelegate** daha kolay. İkinci olarak, `Preferences` özelliği sunan küresel **AppPreferences** bu görünümde herhangi bir kullanıcı Arabirimi denetimleri ile veri bağlama yerleştirilen için sınıf.

Ardından, yeniden arabirimi Oluşturucu'da açın (ve yalnızca yukarıda yapılan değişiklikleri görmek için) görsel taslak dosyasına çift tıklayın. Görünüm tercihleri arabirimi oluşturmak için gereken herhangi bir UI denetimine sürükleyin. Her denetim için geçiş **bağlama denetçisi** ve bağlamak için tek tek özelliklerini **AppPreference** sınıfı:

[![](dialog-images/prefs13.png "Bağlama denetçisi")](dialog-images/prefs13.png#lightbox)

Tüm Panel (Görünüm denetleyicisi) için yukarıdaki adımları yineleyin ve gerekli tercih özellikler.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Tercih uygulamak için tüm açık Windows değiştirir

Yukarıda belirtildiği gibi uygulama kullanıcının herhangi bir uygulamanın kullanıcı tercihleri bu değişiklikleri değişiklik yaptığında, otomatik olarak kaydedilir ve tüm windows uygulanan tipik bir macOS kullanıcı uygulamada açık olabilir.

Minimal bir kodlama iş miktarını ile son kullanıcı için sorunsuz ve şeffaf bir şekilde olması için bu işlemi, dikkatli planlama ve tasarım ve uygulama tercihleri windows izin verir.

Uygulama Tercihleri kullanan herhangi bir pencerede için aşağıdaki yardımcı özelliği erişmesini sağlamak için içerik görünümü denetleyicisi ekleme bizim **AppDelegate** daha kolay:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Ardından, içeriği veya kullanıcı tercihleri temelinde davranışı yapılandırmak için bir sınıf ekleyin:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Pencerenin kullanıcının tercihlerini uyduğundan emin olmak için ilk kez açıldığında yapılandırma yöntemini çağırmanız gerekir:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Ardından, Düzenle `AppDelegate.cs` dosya ve değişiklikleri tüm açık pencereleri herhangi bir tercih uygulamak için aşağıdaki yöntemi ekleyin:

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

Ardından, ekleme bir `PreferenceWindowDelegate` projeye sınıf ve aşağıdaki gibi görünmesi:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWidowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

Bu tercih penceresi kapandığında için tüm açık Windows gönderilmesini tercih değişiklikleri neden olur.

Son olarak, tercih penceresi denetleyicisi düzenleyin ve yukarıda oluşturulan temsilci ekleyin:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWidowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

Kullanıcı, uygulamanın tercihleri düzenler ve tercih pencereyi kapatır, tüm bu değişiklikler ile yerinde, tüm açık Windows için değişiklikler uygulanır:

[![](dialog-images/prefs14.png "Bir örnek tercihleri penceresini")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Aç iletişim kutusu

Aç iletişim kutusu, kullanıcılara bulmak ve bir uygulamada bir öğeyi açmak için tutarlı bir yol sağlar. Bir Xamarin.Mac uygulamasında açık bir iletişim kutusu görüntülemek için aşağıdaki kodu kullanın:

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

Yukarıdaki kodda, biz dosyasının içeriğini görüntülemek için yeni bir belge penceresi açıyoruz. Bunu değiştirmeniz gerekir işlevi koduyla uygulamanızın gerektirdiği.

İle çalışırken aşağıdaki özellikler kullanılabilir bir `NSOpenPanel`:

- **CanChooseFiles** - `true` kullanıcı dosyaları seçebilirsiniz.
- **CanChooseDirectories** - `true` kullanıcı dizinler seçebilirsiniz.
- **AllowsMultipleSelection** - `true` kullanıcı aynı anda birden fazla dosya seçebilirsiniz.
- **ResolveAliases** - `true` seçerek ve diğer çözümler, özgün dosyanın yolu.
- **AllowedFileTypes** -kullanıcının ya da bir uzantısı olarak seçip dosya türleri bir dize dizisi veya _UTI_. Varsayılan değer `null`, herhangi bir dosya açılmasını sağlar.

`RunModal ()` Yöntemi Aç iletişim kutusu görüntüler ve kullanıcının dosyaları veya dizinleri (Özellikler tarafından belirtildiği şekilde) seçmesine izin ver ve döndürür `1` kullanıcı tıklarsa **açık** düğmesi.

Aç iletişim kutusu URL'lerinde dizisi olarak kullanıcının seçili dosyaları veya dizinleri döndürür `URL` özelliği.

Programı çalıştırın ve seçeneğini belirlerseniz **Aç...**  öğesini **dosya** menüsünde, aşağıda görüntülenmektedir: 

[![](dialog-images/dialog03.png "Açık bir iletişim kutusu")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Yazdırma ve Sayfa Yapısı iletişim kutuları

Standart yazdırma macOS sağlar ve kullandıkları her uygulama deneyimi sayfasında Kurulum uygulamanız görüntüleyebilir ve böylece kullanıcılar, tutarlı bir yazdırma olabilir iletişim kutuları.

Aşağıdaki kod, standart yazdırma iletişim kutusu gösterilir:

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

Ayarlarsanız `ShowPrintAsSheet` özelliğini `false`, uygulamayı çalıştırmak ve yazdırma iletişim kutusu görüntüler, aşağıdaki görüntülenir:

[![](dialog-images/print01.png "Yazdırma iletişim kutusu")](dialog-images/print01.png#lightbox)

Varsa Ayarla `ShowPrintAsSheet` özelliğini `true`, uygulamayı çalıştırmak ve yazdırma iletişim kutusu görüntüler, aşağıdaki görüntülenir:

[![](dialog-images/print02.png "Bir yazdırma sayfası")](dialog-images/print02.png#lightbox)

Aşağıdaki kod, sayfa düzeni iletişim kutusu görüntülenir:

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

Ayarlarsanız `ShowPrintAsSheet` özelliğini `false`, uygulamayı çalıştırmak ve sayfa düzeni iletişim kutusu görüntüler, aşağıdaki görüntülenir:

[![](dialog-images/print03.png "Sayfa Yapısı iletişim")](dialog-images/print03.png#lightbox)

Varsa Ayarla `ShowPrintAsSheet` özelliğini `true`, uygulamayı çalıştırmak ve sayfa düzeni iletişim kutusu görüntüler, aşağıdaki görüntülenir:

[![](dialog-images/print04.png "Bir sayfa kurulum sayfası")](dialog-images/print04.png#lightbox)

Yazdırma ve Kurulum sayfasında iletişim kutuları ile çalışma hakkında daha fazla bilgi için lütfen Apple'nın bakın [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) ve [yazdırma giriş](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) belgeleri.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Kaydet iletişim kutusu

Kaydet iletişim kutusu, kullanıcılara bir uygulamada bir öğesini kaydetmek için tutarlı bir yol sağlar.

Aşağıdaki kod, standart Kaydet iletişim kutusu gösterilir:

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

`AllowedFileTypes` Özelliği olan bir dize dizisi dosya türlerinin kullanıcı dosyayı farklı kaydet seçeneğini belirleyebilirsiniz. Dosya türü ya da bir uzantısı olarak belirtilebilir veya _UTI_. Varsayılan değer `null`, kullanılacak herhangi bir dosya türünü sağlar.

Ayarlarsanız `ShowSaveAsSheet` özelliğini `false`, uygulamayı çalıştırmak ve seçmek **Farklı Kaydet...**  gelen **dosya** menüsünde, aşağıdaki görüntülenir:

[![](dialog-images/save01.png "İletişim kutusu kaydetme")](dialog-images/save01.png#lightbox)

Kullanıcı iletişim genişletebilirsiniz:

[![](dialog-images/save02.png "Bir genişletilmiş Kaydet iletişim kutusu")](dialog-images/save02.png#lightbox)

Ayarlarsanız `ShowSaveAsSheet` özelliğini `true`, uygulamayı çalıştırmak ve seçmek **Farklı Kaydet...**  gelen **dosya** menüsünde, aşağıdaki görüntülenir:

[![](dialog-images/save03.png "Bir kayıt sayfası")](dialog-images/save03.png#lightbox)

Kullanıcı iletişim genişletebilirsiniz:

[![](dialog-images/save04.png "Genişletilmiş bir sayfayı Kaydet")](dialog-images/save04.png#lightbox)

Kaydet iletişim ile çalışma hakkında daha fazla bilgi için lütfen Apple'nın bakın [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) belgeleri.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede ayrıntılı kalıcı Windows, sayfa ve bir Xamarin.Mac uygulamasını standart iletişim kutuları sistemi çalışma göz duruma getirdi. Farklı türler ve kalıcı Windows, sayfaları ve iletişim kutuları, kullanımları gördüğümüz oluşturmak ve kalıcı Windows ve xcode'da sayfaları korumak için arabirim oluşturucu ve kalıcı Windows ile çalışma konusunda kullanıcının nasıl sayfaları ve iletişim kutuları, C# kodu.

## <a name="related-links"></a>İlgili bağlantılar

- [MacWindows (örnek)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Menüler](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Araç Çubukları](~/mac/user-interface/toolbar.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Giriş sayfası](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Yazdırma giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
