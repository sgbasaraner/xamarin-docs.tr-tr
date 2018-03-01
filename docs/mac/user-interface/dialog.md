---
title: "İletişim kutuları"
description: "Bu makalede iletişim kutuları ve Xamarin.Mac uygulamada kalıcı windows ile çalışma kapsar. Bu, Xcode ve arabirimi Oluşturucu standart iletişim kutuları ile çalışma ve C# kodunda bu denetimleri ile etkileşim kalıcı pencere oluşturma işlemini açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9b65e870fae0074726d0bdd46d9eecbe99240e98
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="dialogs"></a>İletişim kutuları

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı iletişim kutuları ve kalıcı Windows erişiminiz, içinde çalışan bir geliştirici *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve kalıcı Windows korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

Bir iletişim kutusu yanıt olarak bir kullanıcı eylemi görüntülenir ve genelde yol kullanıcılar eylemi tamamlayabilmeniz için sağlar. Bir iletişim kutusu kapatılabilmesi için kullanıcıdan bir yanıt gerektirir.

Windows (örneğin, uygulama devam etmeden önce kapatıldığında gerekir bir dışarı aktarma iletişim kutusu) kalıcı veya geçici bir durumda (örneğin, birden çok belge aynı anda açık olan bir metin düzenleyicisi) kullanılan olabilir.

[ ![](dialog-images/dialog03.png "Açık bir iletişim kutusu")](dialog-images/dialog03.png)

Bu makalede, sizi bir Xamarin.Mac uygulamasında iletişim kutuları ve kalıcı Windows ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>İletişim kutuları giriş

Bir iletişim kutusu yanıt (dosya kaydetme gibi) bir kullanıcı eylemi olarak görüntülenir ve bu eylemi tamamlamak bir yol sağlar. Bir iletişim kutusu kapatılabilmesi için kullanıcıdan bir yanıt gerektirir.

Apple göre bir iletişim kutusunu sunmak için üç yolu vardır:

- **Belge kalıcı** -A belge kalıcı iletişim kullanıcı onu kapatılmadan belirli bir belge içinde başka bir şey yapmasını engeller.
- **Uygulama kalıcı** - bir uygulama Modal iletişim kutusu, kapatılmadan uygulama ile etkileşim kullanıcı engeller.
- **Engelleyici olmayan** A engelleyici olmayan iletişim hala belge penceresi ile etkileşim sırasında iletişim ayarlarını değiştirmek kullanıcıların sağlar.

### <a name="modal-window"></a>Kalıcı pencere

Herhangi bir standart `NSWindow` kalıcı olarak görüntüleyerek özelleştirilmiş bir iletişim kutusu olarak kullanılabilir:

[ ![](dialog-images/modal01.png "Bir örnek kalıcı pencere")](dialog-images/modal01.png)

### <a name="document-modal-dialog-sheets"></a>Belge Modal iletişim kutusu sayfaları

A _sayfası_ iletişim kutusunu kapatmak kadar penceresiyle etkileşim kullanıcıların engelleme verilen belge penceresine, bağlı olan kalıcı bir iletişim kutusu. Bir sayfa, ortaya çıkar ve herhangi bir anda yalnızca bir sayfa için bir pencere aç olabilir penceresine eklenir.

[ ![](dialog-images/sheet08.png "Bir örnek kalıcı sayfası")](dialog-images/sheet08.png)

### <a name="preferences-windows"></a>Tercihler Windows

Kullanıcı çok sık değişmeyen uygulamanın ayarlarını içeren bir kalıcı olmayan iletişim tercihleri penceredir. Tercihler Windows genellikle farklı ayar grupları arasında geçiş yapmak kullanıcının sağlayan bir araç içerir:

[ ![](dialog-images/dialog02.png "Bir örnek tercih penceresi")](dialog-images/dialog02.png)

### <a name="open-dialog"></a>Aç iletişim kutusu

Aç iletişim kullanıcıları bulmak ve bir uygulamada bir öğeyi açmak için tutarlı bir yol sunar:

[ ![](dialog-images/dialog03.png "Aç iletişim kutusu")](dialog-images/dialog03.png)


### <a name="print-and-page-setup-dialogs"></a>Yazdırma ve Sayfa Yapısı iletişim kutuları

Standart yazdırma macOS sağlar ve sayfa Kurulum uygulamanızı görüntüleyebilir ve böylece kullanıcıların tutarlı bir yazdırma sahibi iletişim kutularını kullandıkları her uygulama deneyimi.

Yazdır iletişim kutusu, hem de serbest Yüzen bir iletişim kutusu görüntülenebilir:

[ ![](dialog-images/print01.png "Yazdır iletişim kutusu")](dialog-images/print01.png)

Veya bir sayfa olarak görüntülenebilir:

[ ![](dialog-images/print02.png "Bir yazdırma sayfası")](dialog-images/print02.png)

Sayfa Yapısı iletişim hem de serbest Yüzen bir iletişim kutusu gösterilebilir:

[ ![](dialog-images/print03.png "Sayfa Yapısı iletişim")](dialog-images/print03.png)

Veya bir sayfa olarak görüntülenebilir:

[ ![](dialog-images/print04.png "Bir sayfa kurulum sayfası")](dialog-images/print04.png)

### <a name="save-dialogs"></a>İletişim kutuları Kaydet

Kaydet iletişim kullanıcılara bir uygulamada bir öğesini kaydetmek için tutarlı bir yol sağlar. İki durumlu Kaydet iletişim vardır: **en az** (daraltılmış olarak da bilinir):

[ ![](dialog-images/save01.png "İletişim kaydetme")](dialog-images/save01.png)

Ve **Genişletilmiş** durumu:

[ ![](dialog-images/save02.png "Bir genişletilmiş Kaydet iletişim kutusu")](dialog-images/save02.png)

**En az** Kaydet iletişim bir sayfa olarak da görüntülenebilir:

[ ![](dialog-images/save03.png "En az sayfası Kaydet")](dialog-images/save03.png)

Gibi **Genişletilmiş** Kaydet iletişim:

[ ![](dialog-images/save04.png "Genişletilmiş bir kaydetme sayfası")](dialog-images/save04.png)

Daha fazla bilgi için bkz: [iletişim kutularını](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Kalıcı pencere projeye ekleme

Ana belge penceresine yanı sıra Xamarin.Mac uygulama kullanıcıya Tercihler veya Inspector paneller gibi diğer windows türlerini görüntülemek gerekebilir.

Yeni bir pencere eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, açık `Main.storyboard` dosyasını Xcode'nın arabirimi Oluşturucusu'nda düzenleme için.
2. Yeni bir sürükleyin **View Controller** tasarım yüzeyine içine:

    [ ![](dialog-images/new01.png "Kitaplıktan bir görünüm denetleyicisini seçme")](dialog-images/new01.png)
3. İçinde **kimlik denetçisi**, girin `CustomDialogController` için **sınıf adı**: 

    [ ![](dialog-images/new02.png "Sınıf adı ayarlama")](dialog-images/new02.png)
4. Mac için Visual Studio'ya geri geçiş, Xcode ile eşitleme ve oluşturmak izin `CustomDialogController.h` dosya.
5. Xcode için dönün ve Arabiriminizin tasarım: 

    [ ![](dialog-images/new03.png "Xcode kullanıcı Arabiriminde tasarlama")](dialog-images/new03.png)
6. Oluşturma bir **kalıcı ü** ana penceresinde uygulamanızın denetim sürükleyerek yeni görünüm denetleyiciye UI öğeden iletişim kutusu penceresine iletişim kutusu açılır. Ata **tanımlayıcısı** `ModalSegue`: 

    [ ![](dialog-images/new06.png "Kalıcı segue")](dialog-images/new06.png)
6. Herhangi bir kablo yukarı **Eylemler** ve **çıkışlar**: 

    [ ![](dialog-images/new04.png "Bir eylem yapılandırma")](dialog-images/new04.png)
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Olun `CustomDialogController.cs` aşağıdaki gibi dosya bakın:

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

Bu kod, başlığı ve açıklamayı iletişim ayarlamak için birkaç özellikleri ve iptal edilen veya kabul iletişim tepki vermek için birkaç olayları gösterir.

Sonra düzenleme `ViewController.cs` dosya, geçersiz kılma `PrepareForSegue` yöntemi ve şu şekilde görünür yapın:

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

Bu kod size bizim iletişim Xcode'nın arabirimi Oluşturucusu'nda tanımlanan segue başlatır ve başlık ve açıklama ayarlar. Ayrıca bu iletişim kutusunda kullanıcının yaptığı seçim yürütür.

Uygulamamızı çalıştırmak ve özel iletişim kutusunu görüntüle:

[ ![](dialog-images/new05.png "Örnek bir iletişim kutusu")](dialog-images/new05.png)

Windows Xamarin.Mac uygulamasında kullanma hakkında daha fazla bilgi için lütfen bkz bizim [Windows ile birlikte çalışma](~/mac/user-interface/window.md) belgeleri.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Özel bir sayfa oluşturma

A _sayfası_ iletişim kutusunu kapatmak kadar penceresiyle etkileşim kullanıcıların engelleme verilen belge penceresine, bağlı olan kalıcı bir iletişim kutusu. Bir sayfa, ortaya çıkar ve herhangi bir anda yalnızca bir sayfa için bir pencere aç olabilir penceresine eklenir. 

İçinde Xamarin.Mac özel sayfası oluşturmak için şirketinizdeki aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, açık `Main.storyboard` dosyasını Xcode'nın arabirimi Oluşturucusu'nda düzenleme için.
2. Yeni bir sürükleyin **View Controller** tasarım yüzeyine içine:

    [ ![](dialog-images/new01.png "Kitaplıktan bir görünüm denetleyicisini seçme")](dialog-images/new01.png)
2. Kullanıcı Arabiriminizin tasarım:

    [ ![](dialog-images/sheet01.png "Kullanıcı Arabirimi tasarımı")](dialog-images/sheet01.png)
3. Oluşturma bir **sayfası ü** yeni görünüm denetleyiciye, ana pencereden: 

    [ ![](dialog-images/sheet02.png "Sayfa segue türü seçme")](dialog-images/sheet02.png)
4. İçinde **kimlik denetçisi**, görünüm denetleyicinin adı **sınıfı** `SheetViewController`: 

    [ ![](dialog-images/sheet03.png "Sınıf adı ayarlama")](dialog-images/sheet03.png)
5. Gerekli tanımlamak **çıkışlar** ve **Eylemler**: 

    [ ![](dialog-images/sheet04.png "Gerekli çıkışlar ve eylemleri tanımlama")](dialog-images/sheet04.png)
6. Değişikliklerinizi kaydetmek ve Visual Studio eşitlemek için Mac için geri dönün.

Ardından, düzenleme `SheetViewController.cs` dosya ve şu şekilde görünür yapın:

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

Ardından, düzenleme `ViewController.cs` dosya, Düzen `PrepareForSegue` yöntemi ve şu şekilde görünür yapın:

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

[ ![](dialog-images/sheet08.png "Bir örnek sayfası")](dialog-images/sheet08.png)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Tercihler iletişim kutusu oluşturma

Biz arabirimi Oluşturucu tercih görünümünde yerleştirme önce biz Tercihler geçişi işlemek için bir özel segue türü eklemeniz gerekir. Projeniz için yeni bir sınıf ekleyin ve bunu `ReplaceViewSeque `. Sınıf Düzenle ve aşağıdaki gibi yapar:

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

Oluşturulan özel segue ile bizim Tercihler işlemek için Xcode'nın arabirimi Oluşturucusu'nda yeni bir pencere ekleyebiliriz.

Yeni bir pencere eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, açık `Main.storyboard` dosyasını Xcode'nın arabirimi Oluşturucusu'nda düzenleme için.
2. Yeni bir sürükleyin **penceresi denetleyicisi** tasarım yüzeyine içine:

    [ ![](dialog-images/pref01.png "Kitaplıktan bir pencere denetleyicisi seçin")](dialog-images/pref01.png)
3. Pencere yakın Yerleştir **menü çubuğu** Tasarımcısı:

    [ ![](dialog-images/pref02.png "Yeni pencerede ekleme")](dialog-images/pref02.png)
4. Tercih görünümünüzde sekmeleri olacaktır gibi ekli View Controller kopyalarını oluşturun:

    [ ![](dialog-images/pref03.png "Gerekli görünüm denetleyicileri ekleme")](dialog-images/pref03.png)
5. Yeni bir sürükleyin **araç denetleyicisi** gelen **Kitaplığı**:

    [ ![](dialog-images/pref04.png "Kitaplıktan bir araç denetleyicisi seçin")](dialog-images/pref04.png)
6. Ve tasarım yüzeyine penceresinde bırakın:

    [ ![](dialog-images/pref05.png "Yeni bir araç denetleyicisi ekleme")](dialog-images/pref05.png)
7. Düzen araç tasarımını:

    [ ![](dialog-images/pref06.png "Araç çubuğu düzeni")](dialog-images/pref06.png)
8. Denetim tıklatın ve her birinden sürükleyin **araç çubuğu düğmesi** yukarıda oluşturduğunuz görünümlere. Seçin bir **özel** türü ü:

    [ ![](dialog-images/pref07.png "Ayar segue türü")](dialog-images/pref07.png)
9. Yeni Segue seçin ve ayarlayın **sınıfı** için `ReplaceViewSegue`:

    [ ![](dialog-images/pref08.png "Ayar segue sınıfı")](dialog-images/pref08.png)
10. İçinde **Menubar Tasarımcısı** tasarım yüzeyine uygulama menüsünden seçin **tercihleri...** denetim tıklatın ve oluşturmak için Tercihler penceresine sürükleyin bir **Göster** ü:

    [ ![](dialog-images/pref09.png "Ayar segue türü")](dialog-images/pref09.png)
11. Değişikliklerinizi kaydetmek ve Visual Studio eşitlemek için Mac için geri dönün.

Şu kodu çalıştırın ve seçerseniz **tercihleri...**  gelen **uygulama menüsü**, penceresi görüntülenir:

[ ![](dialog-images/pref10.png "Bir örnek Tercihler penceresi")](dialog-images/pref10.png)

Windows ve araç çubuklarını ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Windows](~/mac/user-interface/window.md) ve [araç çubukları](~/mac/user-interface/toolbar.md) belgeleri.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Kaydetme ve tercihleri yükleme

Kullanıcı değişiklikleri uygulamanın kullanıcı tercihleri birine yaptığında, bir tipik macOS uygulama, bu değişiklikleri otomatik olarak kaydedilir. Bu, bir Xamarin.Mac uygulamasında işlemek için en kolay yoludur sistem genelinde tüm kullanıcının tercihlerini yönetmek ve onları paylaşmak için tek bir sınıf oluşturmak için.

İlk olarak, yeni bir ekleyin `AppPreferences` sınıf projeye ve devralınmalıdır `NSObject`. Tercihler kullanmak üzere tasarlanmış [verileri bağlama ve anahtar-değer kodlama](~/mac/app-fundamentals/databinding.md) oluşturma işlemini yapacak ve tercih koruma formlar çok daha kolaydır. Tercihler basit veri türleri, az miktarda oluşacak olduğundan, yerleşik kullanmak `NSUserDefaults` depolamak ve değerleri almak için.

Düzen `AppPreferences.cs` dosya ve şu şekilde görünür yapın:

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

Bu sınıf gibi birkaç yardımcı yordamları içerir `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, vb. ile çalışma yapmaya `NSUserDefaults` daha kolay. Ayrıca, bu yana `NSUserDefaults` işlemek için yerleşik bir yol yok `NSColors`, `NSColorToHexString` ve `NSColorFromHexString` renkleri web tabanlı onaltılık dizeleri dönüştürme için kullanılan yöntemleri (`#RRGGBBAA` burada `AA` Alfa Saydamlığı olan), olabilir kolayca depolanan ve alındı.

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

Ardından, kullanıcı Arabirimi öğeleri yukarıda oluşturulan görünümleri ve tercih penceresi tercih sınıfı bağlayın. Arabirim Oluşturucu tercih görünüm denetleyicisini seçin ve geçiş **kimlik denetçisi**, denetleyici için özel bir sınıf oluşturun: 

[ ![](dialog-images/prefs12.png "Kimlik denetçisi")](dialog-images/prefs12.png)

Değişikliklerinizi eşitlemeyi ve düzenlemek için yeni oluşturulan sınıf açmak Mac için Visual Studio için dönün. Şuna benzer sınıfı olun:

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

Bu sınıf burada iki şey yaptığına dikkat edin: ilk olarak, bir yardımcı yoktur `App` erişme özelliğini **AppDelegate** daha kolay. İkinci, `Preferences` özelliği sunan genel **AppPreferences** bu görünümde herhangi bir kullanıcı Arabirimi denetim ile veri bağlaması yerleştirilen için sınıf.

Ardından, yeniden arabirimi Oluşturucusu'nda açın (ve yalnızca yukarıda yapılan değişiklikleri görmek için) film şeridi dosyasına çift tıklayın. Görünüme Tercihler arabirim oluşturmak için gerekli kullanıcı Arabirimi denetimlerini sürükleyin. Her denetim için geçiş **bağlama denetçisi** ve bağlamak için tek tek özellikleri **AppPreference** sınıfı:

[ ![](dialog-images/prefs13.png "Bağlama denetçisi")](dialog-images/prefs13.png)

Tüm panelleri (Görünüm denetleyicileri) için yukarıdaki adımları yineleyin ve tercih özellikleri gereklidir.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Tercih uygulama için tüm açık Windows değiştirir

Yukarıda belirtildiği gibi kullanıcı değişiklikleri uygulamanın kullanıcı tercihleri, bu değişiklikleri birine yaptığında, uygulama otomatik olarak kaydedilir ve tüm windows uygulanan tipik bir macOS kullanıcı uygulamada açık olabilir.

Dikkatli planlama ve tasarımı, uygulamanızın tercihlerini ve windows, bu işlem çalışma kodlama en az bir miktar son kullanıcının sorunsuz ve şeffaf bir şekilde yapılmasını izin verir.

Uygulama Tercihleri kullanan herhangi bir pencerede için aşağıdaki Yardımcısı özelliği erişme yapmak için içerik görünümü denetleyicisi ekleme bizim **AppDelegate** daha kolay:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Ardından, içeriği veya kullanıcının tercihlerini dayalı davranışı yapılandırmak için bir sınıf ekleyin:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Pencerenin kullanıcının tercihlerini uyan emin olmak için ilk kez açıldığında yapılandırma yöntemi çağırmanız gerekir:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Ardından, düzenleme `AppDelegate.cs` dosya ve tüm tercih uygulamak için aşağıdaki yöntemi değişiklikler için tüm açık windows ekleyin:

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

Ardından, eklemek bir `PreferenceWindowDelegate` sınıf projeye ve aşağıdaki gibi görünmesi:

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

Bu tercih değişiklikleri tercih penceresi kapatıldığında tüm açık pencereleri gönderilmesine neden olur.

Son olarak, tercih penceresi denetleyicisi düzenleyin ve yukarıda oluşturduğunuz temsilci ekleyin:

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

Tüm bu değişikliklerle yerinde, kullanıcı uygulamanın Tercihler düzenler ve tercih penceresi kapanır değişiklikler için tüm açık Windows uygulanır:

[ ![](dialog-images/prefs14.png "Bir örnek Tercihler penceresi")](dialog-images/prefs14.png)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Aç iletişim kutusu

Aç iletişim kullanıcıları bulmak ve bir uygulamada bir öğeyi açmak için tutarlı bir yol sağlar. Xamarin.Mac uygulamada açık bir iletişim kutusu görüntülemek için aşağıdaki kodu kullanın:

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

Yukarıdaki kod içinde biz dosyasının içeriğini görüntülemek için yeni bir belge penceresi açıyorsunuz. Bunu değiştirmeniz gerekir işlevselliği koduyla, uygulamanız tarafından gereklidir.

İle çalışırken aşağıdaki özellikler kullanılabilir bir `NSOpenPanel`:

- **CanChooseFiles** - `true` kullanıcı dosyaları seçebilirsiniz.
- **CanChooseDirectories** - `true` kullanıcı dizinleri seçebilirsiniz.
- **AllowsMultipleSelection** - `true` kullanıcı aynı anda birden çok dosya seçin.
- **ResolveAliases** - `true` seçerek ve diğer çözümler, özgün dosyanın yolu.
- **AllowedFileTypes** -bir dize dizisi dosya türleri kullanıcı ya da bir uzantısı olarak seçebilir veya _UTI_. Varsayılan değer `null`, açılması herhangi bir dosya sağlar.

`RunModal ()` Yöntemi Aç iletişim kutusu görüntüler ve kullanıcının dosya veya dizinlerin (Özellikler tarafından belirtildiği şekilde) seçmesine izin ver ve döndürür `1` kullanıcı tıklarsa **açık** düğmesi.

Aç iletişim URL'lerinde dizisi olarak kullanıcının seçili dosyaları veya dizinleri döndürür `URL` özelliği.

Programını çalıştırın ve seçeneğini belirlerseniz **Aç...**  gelen öğe **dosya** menüsü, aşağıdaki görüntülenir: 

[ ![](dialog-images/dialog03.png "Açık bir iletişim kutusu")](dialog-images/dialog03.png)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Yazdırma ve Sayfa Yapısı iletişim kutuları

Standart yazdırma macOS sağlar ve sayfa Kurulum uygulamanızı görüntüleyebilir ve böylece kullanıcıların tutarlı bir yazdırma sahibi iletişim kutularını kullandıkları her uygulama deneyimi.

Aşağıdaki kod, standart yazdırma iletişim kutusunu gösterecektir:

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

Biz ayarlarsanız `ShowPrintAsSheet` özelliğine `false`, uygulamayı çalıştırın ve yazdırma iletişim kutusu görüntüler, aşağıdaki görüntülenir:

[ ![](dialog-images/print01.png "Yazdır iletişim kutusu")](dialog-images/print01.png)

Varsa ayarlamak `ShowPrintAsSheet` özelliğine `true`, uygulamayı çalıştırın ve yazdırma iletişim kutusu görüntüler, aşağıdaki görüntülenir:

[ ![](dialog-images/print02.png "Bir yazdırma sayfası")](dialog-images/print02.png)

Aşağıdaki kod sayfası düzeni iletişim kutusu görüntülenir:

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

Biz ayarlarsanız `ShowPrintAsSheet` özelliğine `false`, uygulamayı çalıştırın ve yazdırma düzeni iletişim kutusu görüntüler, aşağıdaki görüntülenir:

[ ![](dialog-images/print03.png "Sayfa Yapısı iletişim")](dialog-images/print03.png)

Varsa ayarlamak `ShowPrintAsSheet` özelliğine `true`, uygulamayı çalıştırın ve yazdırma düzeni iletişim kutusu görüntüler, aşağıdaki görüntülenir:

[ ![](dialog-images/print04.png "Bir sayfa kurulum sayfası")](dialog-images/print04.png)

Yazdırma ve sayfa Kurulum iletişim kutuları ile çalışma hakkında daha fazla bilgi için lütfen Apple'nın bakın [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) ve [yazdırma giriş](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) belgeler.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Kaydet iletişim kutusu

Kaydet iletişim kullanıcılara bir uygulamada bir öğesini kaydetmek için tutarlı bir yol sağlar.

Aşağıdaki kod, standart Kaydet iletişim kutusunu gösterecektir:

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

`AllowedFileTypes` Özelliği olan bir dize dizisi dosya türlerinin dosyası olarak kaydetmek için kullanıcı seçebilirsiniz. Dosya türü ya da bir uzantısı olarak belirtilebilir veya _UTI_. Varsayılan değer `null`, kullanılacak herhangi bir dosya türü sağlar.

Biz ayarlarsanız `ShowSaveAsSheet` özelliğine `false`, uygulamayı çalıştırın ve seçin **Kaydet...**  gelen **dosya** menüsünde aşağıdaki görüntülenir:

[ ![](dialog-images/save01.png "İletişim kutusu kaydetme")](dialog-images/save01.png)

Kullanıcı iletişim kutusu genişletebilirsiniz:

[ ![](dialog-images/save02.png "Bir genişletilmiş Kaydet iletişim kutusu")](dialog-images/save02.png)

Biz ayarlarsanız `ShowSaveAsSheet` özelliğine `true`, uygulamayı çalıştırın ve seçin **Kaydet...**  gelen **dosya** menüsünde aşağıdaki görüntülenir:

[ ![](dialog-images/save03.png "Sayfa kaydetme")](dialog-images/save03.png)

Kullanıcı iletişim kutusu genişletebilirsiniz:

[ ![](dialog-images/save04.png "Genişletilmiş bir kaydetme sayfası")](dialog-images/save04.png)

Apple'nın Kaydet iletişim ile çalışma hakkında daha fazla bilgi için lütfen bkz [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) belgeleri.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede kalıcı Windows, sayfaları ve Xamarin.Mac uygulama standart sisteminde iletişim kutuları ile çalışan bir ayrıntılı bakış sürdü. Farklı türler ve kalıcı Windows, sayfaları ve iletişim kutuları, kullanımlarını gördüğümüz oluşturmak ve kalıcı pencere ve sayfa xcode'da korumak için arabirimi oluşturucusu ve kalıcı Windows ile çalışmak üzere nasıl kullanıcının nasıl sayfaları ve C# kodunda iletişim kutuları.

## <a name="related-links"></a>İlgili bağlantılar

- [MacWindows (örnek)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Merhaba, Mac](~/mac/get-started/hello-mac.md)
- [Menüleri](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Araç Çubukları](~/mac/user-interface/toolbar.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Giriş sayfası](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Yazdırma giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
