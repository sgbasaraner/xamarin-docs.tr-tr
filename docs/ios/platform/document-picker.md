---
title: Xamarin.iOS, belge Seçici
description: Bu belge, belge Seçici iOS açıklar ve nasıl Xamarin.ios'ta kullanılacağı açıklanır. İCloud, belgeler, ortak kurulum kodu, belge sağlayıcısı uzantıları ve diğer göz sürer.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: ca0c7a6e655fdc44aa673a59be71bc83044d3085
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353340"
---
# <a name="document-picker-in-xamarinios"></a>Xamarin.iOS, belge Seçici

Belge Seçici belgelerinin uygulamalar arasında paylaşılmasına olanak tanır. Bu belgeler, iCloud veya farklı bir uygulamanın dizinine depolanabilir. Belgeleri kümesi paylaşılan [belge sağlayıcısı uzantıları](~/ios/platform/extensions.md) kullanıcı cihazlarında yüklü. 

Uygulamaları ve bulut arasında eşitlenen belgeleri tutmak zorluğu nedeniyle, belirli bir miktarda gerekli karmaşıklığı tanıtır.

## <a name="requirements"></a>Gereksinimler

Aşağıda sunulan bu makaledeki adımları tamamlamak için gereklidir:

-  **Xcode 7 ve iOS 8 veya daha yenisini** – Apple'nın Xcode 7 ve iOS 8 veya daha yeni API'lere gereken yüklenmeli ve geliştiricinin bilgisayarında yapılandırılmış.
-  **Visual Studio veya Mac için Visual Studio** – Mac için Visual Studio'nun en son sürümü yüklü olması gerekir.
-  **iOS cihazını** – iOS 8 çalıştıran bir iOS cihazını veya üzeri.

## <a name="changes-to-icloud"></a>Değişiklikler icloud

Belge Seçici'nün yeni özelliklerini uygulamak için Apple'nın İcloud'a hizmeti aşağıdaki değişiklikler yapılmıştır:

-  Arka plan programı iCloud tamamen CloudKit kullanılarak yazılmıştır.
-  Özellik değerinin mevcut iCloud iCloud sürücü olarak yeniden adlandırıldı.
-  İcloud'a Microsoft Windows işletim sistemi için destek eklendi.
-  Mac OS Finder'da iCloud klasörü eklendi.
-  iOS cihazlar Mac OS iCloud klasörünün içeriğini erişebilir.

> [!IMPORTANT]
> Apple [araçlar sağlar](https://developer.apple.com/support/allowing-users-to-manage-data/) Avrupa Birliği'nin genel veri koruma yönetmeliği (GDPR) düzgün bir şekilde işlemek geliştiricilerin yardımcı olmak için.

## <a name="what-is-a-document"></a>Bir belge nedir?

İCloud belgede söz konusu olduğunda, tek başına tek bir varlık ve kullanıcı tarafından algılandığı şekilde. Bir kullanıcının belgeyi değiştirmesine veya (örneğin, e-posta kullanarak) diğer kullanıcılarla paylaşmak isteyebilirsiniz.

Birden fazla kullanıcı hemen sayfaları gibi bir belge olarak açılış veya sayı dosyaları algılar dosyaları vardır. Ancak, iCloud bu kavramı için sınırlı değildir. Örneğin, (örneğin, bir Chess eşleşme) oyun durumunu bir belge olarak kabul edilir ve iCloud içinde depolanır. Bu dosya, bir kullanıcının cihazlar arasında geçirilebilir ve farklı bir cihaza bıraktığı yerden bir oyun çekme izin vermek.

## <a name="dealing-with-documents"></a>Belgelerle ilgilenme

Belge Seçici Xamarin ile kullanmak için gerekli koda atlama, bu makalede iCloud belgeleri ile çalışmak için en iyi karşılamak için gittiği ve mevcut API'lere yapılan değişiklikler birkaç belge Seçici desteklemek için gereken önce.

### <a name="using-file-coordination"></a>Dosya düzenleme kullanma

Bir dosya, farklı konumlardan değiştirilebildiğinden, veri kaybını önlemek için koordinasyon kullanılması gerekir.

 [![](document-picker-images/image1.png "Dosya düzenleme kullanma")](document-picker-images/image1.png#lightbox)

Yukarıdaki resimde bir göz atalım:

1.  Dosya düzenleme kullanarak bir iOS cihazı yeni bir belge oluşturur ve İcloud'a klasörüne kaydeder.
2.  iCloud bulut her cihaza dağıtım için değiştirilmiş dosya kaydeder.
3.  Bağlı bir Mac, değiştirilen dosya klasörü icloud'da görür ve dosya koordinasyon değişikliklerini dosyasına kopyalamak için kullanır.
4.  Dosya düzenleme kullanmayan bir cihaz dosyasında bir değişiklik yapar ve İcloud'a klasörüne kaydeder. Bu değişiklikler, diğer cihazlara anında çoğaltılır.

Özgün varsayar iOS cihaz veya Mac dosya düzenleme yaptıkları değişiklikleri kaybolur ve eşgüdümlü olmayan bir CİHAZDAN dosyasının sürümü üzerine artık. Veri kaybını önlemek için dosyayı düzenleme bulut tabanlı belgelerle çalışırken zorunluluktur.

### <a name="using-uidocument"></a>UIDocument kullanma

 `UIDocument` şeyler basit hale getirir (veya `NSDocument` macos'ta) için geliştirici yaparak tüm ağır işlerin çoğunu. Sağladığı dosya düzenleme uygulamanın kullanıcı Arabiriminde engellemelerini önlemek için arka plan kuyrukları'yla oluşturulmuş.

 `UIDocument` birden çok kullanıma sunan herhangi bir geliştirici amaç için bir Xamarin uygulaması geliştirme sürecine kolaylaştıran üst düzey API'ler gerektirir.

Aşağıdaki kod öğesinin oluşturur `UIDocument` iCloud metnini almak ve depolamak için kullanılan bir genel metin tabanlı belge uygulamak için:

```csharp
using System;
using Foundation;
using UIKit;

namespace DocPicker
{
    public class GenericTextDocument : UIDocument
    {
        #region Private Variable Storage
        private NSString _dataModel;
        #endregion

        #region Computed Properties
        public string Contents {
            get { return _dataModel.ToString (); }
            set { _dataModel = new NSString(value); }
        }
        #endregion

        #region Constructors
        public GenericTextDocument (NSUrl url) : base (url)
        {
            // Set the default document text
            this.Contents = "";
        }

        public GenericTextDocument (NSUrl url, string contents) : base (url)
        {
            // Set the default document text
            this.Contents = contents;
        }
        #endregion

        #region Override Methods
        public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Were any contents passed to the document?
            if (contents != null) {
                _dataModel = NSString.FromData( (NSData)contents, NSStringEncoding.UTF8 );
            }

            // Inform caller that the document has been modified
            RaiseDocumentModified (this);

            // Return success
            return true;
        }

        public override NSObject ContentsForType (string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Convert the contents to a NSData object and return it
            NSData docData = _dataModel.Encode(NSStringEncoding.UTF8);
            return docData;
        }
        #endregion

        #region Events
        public delegate void DocumentModifiedDelegate(GenericTextDocument document);
        public event DocumentModifiedDelegate DocumentModified;

        internal void RaiseDocumentModified(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentModified != null) {
                this.DocumentModified (document);
            }
        }
        #endregion
    }
}
```

`GenericTextDocument` Sınıfı üzerinde sunulan bir Xamarin.iOS 8 uygulaması dış belgeleri ve belge Seçici ile çalışırken bu makale boyunca kullanılır.

## <a name="asynchronous-file-coordination"></a>Zaman uyumsuz dosya düzenleme

iOS 8 yeni dosya koordinasyon API'leri aracılığıyla birkaç yeni zaman uyumsuz dosya düzenleme özellikleri sağlar. İOS 8 önce var olan tüm dosya koordinasyon API'lerini tamamen zaman uyumlu. Bu, geliştirici, kendi arka plan dosya koordinasyon uygulamanın kullanıcı Arabiriminde engellemesini önleyecek queuing uygulamak için sorumlu geliyordu.

Yeni `NSFileAccessIntent` sınıfı, dosya ve gerekli koordinasyon türünü kontrol etmek için çeşitli seçenekler gösteren bir URL içerir. Aşağıdaki kod, bir dosya bir konumdan diğerine taşınmasını gösterir kullanarak hedefleri:

```csharp
// Get source options
var srcURL = NSUrl.FromFilename ("FromFile.txt");
var srcIntent = NSFileAccessIntent.CreateReadingIntent (srcURL, NSFileCoordinatorReadingOptions.ForUploading);

// Get destination options
var dstURL = NSUrl.FromFilename ("ToFile.txt");
var dstIntent = NSFileAccessIntent.CreateReadingIntent (dstURL, NSFileCoordinatorReadingOptions.ForUploading);

// Create an array
var intents = new NSFileAccessIntent[] {
    srcIntent,
    dstIntent
};

// Initialize a file coordination with intents
var queue = new NSOperationQueue ();
var fileCoordinator = new NSFileCoordinator ();
fileCoordinator.CoordinateAccess (intents, queue, (err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}",err.LocalizedDescription);
    }
});
```

## <a name="discovering-and-listing-documents"></a>Bulma ve belgeleri listeleme

Bulmak ve belgeler listesinde mevcut kullanarak yoludur `NSMetadataQuery` API'leri. Bu bölüm için eklenen yeni özellikler ele alınacaktır `NSMetadataQuery` oluşturan belgelerle çalışma bile artık daha kolay.

### <a name="existing-behavior"></a>Varolan davranışı

İOS 8, önce `NSMetadataQuery` gibi toplama yerel dosya değişiklikleri yavaşlama: siler, oluşturur ve yeniden adlandırır.

 [![](document-picker-images/image2.png "NSMetadataQuery yerel dosya değişiklikleri genel bakış")](document-picker-images/image2.png#lightbox)

Yukarıdaki diyagramda:

1.  Uygulama kapsayıcısında mevcut dosyaları için `NSMetadataQuery` sahip `NSMetadata` kayıtları önceden oluşturulmuş ve bir uygulama için anında kullanılabilir olduklarından biriktirilir.
1.  Uygulama uygulama kapsayıcısında yeni bir dosya oluşturur.
1.  Önce bir gecikme `NSMetadataQuery` değişiklik uygulama kapsayıcısı'nda görür ve gerekli oluşturur `NSMetadata` kaydı.


Oluşturulmasını gecikme nedeniyle `NSMetadata` iki veri kaynakları açmak kayıt ve uygulama vardı: yerel dosya değişiklikleri için bir tane ve bulut için bir temel değişiklikler.

### <a name="stitching"></a>Birleştirme

İOS 8 ' deki `NSMetadataQuery` birleştirme adlı doğrudan yeni bir özellik ile kullanmak daha kolaydır:

 [![](document-picker-images/image3.png "Birleştirme ile yeni bir özellik NSMetadataQuery çağırılır")](document-picker-images/image3.png#lightbox)

Yukarıdaki diyagramda birleştirme kullanma:

1.  Önceki örneklerde olduğu gibi uygulama kapsayıcısında mevcut dosyaları için `NSMetadataQuery` sahip `NSMetadata` kayıtları önceden oluşturulmuş ve biriktirilir.
1.  Uygulama uygulama kapsayıcısında dosya koordinasyon kullanarak yeni bir dosya oluşturur.
1.  Uygulama kapsayıcısında bir kancası çağrıları ve değişikliği görür `NSMetadataQuery` gerekli oluşturmak için `NSMetadata` kaydı.
1.  `NSMetadata` Kayıt doğrudan sonra dosya oluşturulur ve uygulama için kullanılabilir hale getirilir.


Birleştirme kullanarak uygulaması artık yerel izlemek için bir veri kaynağı açmak ve bulut tabanlı dosya değişiklikleri. Uygulama üzerinde güvenebilirsiniz artık `NSMetadataQuery` doğrudan.

> [!IMPORTANT]
> Yukarıdaki bölümde sunulan dosya koordinasyon uygulama kullanıyorsa birleştirme yalnızca çalışır. Dosya koordinasyon kullanılmayan, API'leri varsayılan olarak öncesi iOS 8'deki mevcut davranışı.




### <a name="new-ios-8-metadata-features"></a>Yeni iOS 8 meta veri özellikleri

Aşağıdaki yeni özellikler eklenmiş `NSMetadataQuery` iOS 8'de:

-   `NSMetatadataQuery` Şimdi yerel olmayan belgeler bulutta depolanan listeleyebilirsiniz.
-  Meta veri bilgilerini bulut tabanlı belgelere erişmek için yeni API'ler eklenmiştir. 
-  Yeni bir `NSUrl_PromisedItems` kullanılabilir içeriği yerel olarak olmayabilir veya dosyaların dosya özniteliklerine erişmek için olacak API.
-  Kullanma `GetPromisedItemResourceValue` belirli bir dosya hakkında bilgi almak veya kullanmak için bir yöntem `GetPromisedItemResourceValues` aynı anda birden fazla dosya bilgi almak için yöntemi.


Meta veri başa çıkmak için iki yeni dosya koordinasyon bayrakları eklenmiştir:

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Yukarıdaki bayraklarıyla dosyanın içeriğini yerel olarak bunlar için kullanılmak üzere kullanılabilir olması gerekmez.

Aşağıdaki kod kesimi nasıl kullanılacağını gösterir `NSMetadataQuery` belirli bir dosyanın varlığını sorgulamak ve yoksa dosyasını oluşturmak için:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;


#region Static Properties
public const string TestFilename = "test.txt"; 
#endregion

#region Computed Properties
public bool HasiCloud { get; set; }
public bool CheckingForiCloud { get; set; }
public NSUrl iCloudUrl { get; set; }

public GenericTextDocument Document { get; set; }
public NSMetadataQuery Query { get; set; }
#endregion

#region Private Methods
private void FindDocument () {
    Console.WriteLine ("Finding Document...");

    // Create a new query and set it's scope
    Query = new NSMetadataQuery();
    Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

    // Build a predicate to locate the file by name and attach it to the query
    var pred = NSPredicate.FromFormat ("%K == %@"
        , new NSObject[] {
            NSMetadataQuery.ItemFSNameKey
            , new NSString(TestFilename)});
    Query.Predicate = pred;

    // Register a notification for when the query returns
    NSNotificationCenter.DefaultCenter.AddObserver (this,
            new Selector("queryDidFinishGathering:"),           NSMetadataQuery.DidFinishGatheringNotification,
            Query);

    // Start looking for the file
    Query.StartQuery ();
    Console.WriteLine ("Querying: {0}", Query.IsGathering);
}

[Export("queryDidFinishGathering:")]
public void DidFinishGathering (NSNotification notification) {
    Console.WriteLine ("Finish Gathering Documents.");

    // Access the query and stop it from running
    var query = (NSMetadataQuery)notification.Object;
    query.DisableUpdates();
    query.StopQuery();

    // Release the notification
    NSNotificationCenter.DefaultCenter.RemoveObserver (this
        , NSMetadataQuery.DidFinishGatheringNotification
        , query);

    // Load the document that the query returned
    LoadDocument(query);
}

private void LoadDocument (NSMetadataQuery query) {
    Console.WriteLine ("Loading Document...");    

    // Take action based on the returned record count
    switch (query.ResultCount) {
    case 0:
        // Create a new document
        CreateNewDocument ();
        break;
    case 1:
        // Gain access to the url and create a new document from
        // that instance
        NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

        // Load the document
        OpenDocument (url);
        break;
    default:
        // There has been an issue
        Console.WriteLine ("Issue: More than one document found...");
        break;
    }
}
#endregion

#region Public Methods
public void OpenDocument(NSUrl url) {

    Console.WriteLine ("Attempting to open: {0}", url);
    Document = new GenericTextDocument (url);

    // Open the document
    Document.Open ( (success) => {
        if (success) {
            Console.WriteLine ("Document Opened");
        } else
            Console.WriteLine ("Failed to Open Document");
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public void CreateNewDocument() {
    // Create path to new file
    // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
    var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
    var docPath = Path.Combine (docsFolder, TestFilename);
    var ubiq = new NSUrl (docPath, false);

    // Create new document at path 
    Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
    Document = new GenericTextDocument (ubiq);

    // Set the default value
    Document.Contents = "(default value)";

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
        Console.WriteLine ("Save completion:" + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
        } else {
            Console.WriteLine ("Unable to Save Document");
        }
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public bool SaveDocument() {
    bool successful = false;

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
        Console.WriteLine ("Save completion: " + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
            successful = true;
        } else {
            Console.WriteLine ("Unable to Save Document");
            successful=false;
        }
    });

    // Return results
    return successful;
}
#endregion

#region Events
public delegate void DocumentLoadedDelegate(GenericTextDocument document);
public event DocumentLoadedDelegate DocumentLoaded;

internal void RaiseDocumentLoaded(GenericTextDocument document) {
    // Inform caller
    if (this.DocumentLoaded != null) {
        this.DocumentLoaded (document);
    }
}
#endregion
```

### <a name="document-thumbnails"></a>Belge küçük resimleri

Apple belgeleri uygulamanın listelerken en iyi kullanıcı deneyimini'nin Önizleme kullanmaktır hissettirir. Birlikte çalışmak istediğiniz belge hızlıca tanımlayabilirsiniz böylece bu son kullanıcıların bağlamı sağlar.

İOS 8 önce belge önizlemeleri gösteren bir özel uygulaması gerekir. Yeni iOS 8 Geliştirici belge küçük resimleri ile hızlıca çalışmaya izin dosya sistem öznitelikleri olan.

#### <a name="retrieving-document-thumbnails"></a>Belge küçük resimleri alınıyor 

Çağırarak `GetPromisedItemResourceValue` veya `GetPromisedItemResourceValues` yöntemleri `NSUrl_PromisedItems` API, bir `NSUrlThumbnailDictionary`, döndürülür. Bu sözlükteki şu anda yalnızca anahtar `NSThumbnial1024X1024SizeKey` ve kendi eşleşen `UIImage`.

#### <a name="saving-document-thumbnails"></a>Kaydetme belge küçük resimleri

Bir küçük resim kaydetmek için en kolay yolu kullanmaktır `UIDocument`. Çağırarak `GetFileAttributesToWrite` yöntemi `UIDocument` ve dosyanın olduğunda küçük ayarı, bu otomatik olarak kaydedilir. Arka plan programı iCloud bu değişikliği görmek ve İcloud'a yayar. Mac OS X üzerinde küçük resimleri için Geliştirici Hızlı Ara eklenti tarafından otomatik olarak oluşturulur.

Mevcut API değişikliklerin yanı sıra yerinde tabanlı iCloud belgeleri ile çalışmanın temel bilgileri ile bir Xamarin iOS 8 belge Seçici görünüm denetleyicisi uygulamak hazırız mobil uygulaması.


## <a name="enabling-icloud-in-xamarin"></a>Xamarin, iCloud etkinleştirme

Belge Seçici içinde bir Xamarin.iOS uygulaması kullanılmadan önce iCloud destek hem uygulamanızı hem Apple aracılığıyla etkinleştirilmesi gerekir. 

Aşağıdaki adımları gözden geçirme için iCloud sağlama işlemi.

1. İCloud bir kapsayıcı oluşturun.
2. App Service iCloud içeren bir uygulama kimliği oluşturun.
3. Bu uygulama kimliğini içeren bir sağlama profili oluşturma

[Özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) Kılavuzu, ilk iki adımları boyunca size yol gösterir. Bir sağlama profili oluşturmak için adımları izleyin. [sağlama profili](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) Kılavuzu.



Aşağıdaki adımları gözden geçirme iCloud için uygulamanızı yapılandırma işlemi:

Aşağıdakileri yapın:

1.  Proje, Visual Studio veya Mac için Visual Studio'da açın.
2.  İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçenekleri belirleyin.
3.  Seçenekler iletişim kutusu seçme içinde **iOS uygulama**, emin **paket grubu tanımlayıcısı** içinde tanımlanan bir eşleşen **uygulama kimliği** yukarıda uygulama için oluşturulan. 
4.  Seçin **iOS paket grubu imzalama**seçin **Geliştirici kimliği** ve **sağlama profili** yukarıda oluşturduğunuz.
5.  Tıklayın **Tamam** değişiklikleri kaydetmek ve iletişim kutusunu kapatmak için düğme.
6.  Sağ `Entitlements.plist` içinde **Çözüm Gezgini** Düzenleyicisi'nde açın.

    > [!IMPORTANT]
    > Visual Studio'da, yetkilendirmeleri Düzenleyicisi'ni açın, üzerine sağ tıklayarak seçerek gerekebilir **birlikte Aç...** ve özellik listesi Düzenleyicisi seçme

7.  Denetleme **etkinleştirme iCloud** , **iCloud belgeleri** , **anahtar-değer deposu** ve **CloudKit** .
8.  Olun **kapsayıcı** (yukarıda oluşturulan gibi) için uygulama yok. Örnek: `iCloud.com.your-company.AppName`
9.  Değişiklikleri dosyaya kaydedin.

Yetkilendirmeler hakkında daha fazla bilgi için bkz [Yetkilendirmelerle çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

Yerinde yukarıdaki kurulum ile uygulama artık bulutta yer alan belgeler ve yeni belge Seçici görünüm denetleyicisi kullanabilirsiniz.

## <a name="common-setup-code"></a>Genel Kurulum kodu

Belge Seçici görünüm denetleyicisi ile başlamadan önce gerekli bazı standart kurulum kodu yoktur. Başlangıç uygulamanın değiştirerek `AppDelegate.cs` dosyasını açıp aşağıdaki gibi görünmesi:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

namespace DocPicker
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Static Properties
        public const string TestFilename = "test.txt"; 
        #endregion

        #region Computed Properties
        public override UIWindow Window { get; set; }
        public bool HasiCloud { get; set; }
        public bool CheckingForiCloud { get; set; }
        public NSUrl iCloudUrl { get; set; }

        public GenericTextDocument Document { get; set; }
        public NSMetadataQuery Query { get; set; }
        public NSData Bookmark { get; set; }
        #endregion

        #region Private Methods
        private void FindDocument () {
            Console.WriteLine ("Finding Document...");

            // Create a new query and set it's scope
            Query = new NSMetadataQuery();
            Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

            // Build a predicate to locate the file by name and attach it to the query
            var pred = NSPredicate.FromFormat ("%K == %@",
                new NSObject[] {NSMetadataQuery.ItemFSNameKey
                , new NSString(TestFilename)});
            Query.Predicate = pred;

            // Register a notification for when the query returns
            NSNotificationCenter.DefaultCenter.AddObserver (this
                , new Selector("queryDidFinishGathering:")
                , NSMetadataQuery.DidFinishGatheringNotification
                , Query);

            // Start looking for the file
            Query.StartQuery ();
            Console.WriteLine ("Querying: {0}", Query.IsGathering);
        }


        [Export("queryDidFinishGathering:")]
        public void DidFinishGathering (NSNotification notification) {
            Console.WriteLine ("Finish Gathering Documents.");

            // Access the query and stop it from running
            var query = (NSMetadataQuery)notification.Object;
            query.DisableUpdates();
            query.StopQuery();

            // Release the notification
            NSNotificationCenter.DefaultCenter.RemoveObserver (this
                , NSMetadataQuery.DidFinishGatheringNotification
                , query);

            // Load the document that the query returned
            LoadDocument(query);
        }

        private void LoadDocument (NSMetadataQuery query) {
            Console.WriteLine ("Loading Document...");  

            // Take action based on the returned record count
            switch (query.ResultCount) {
            case 0:
                // Create a new document
                CreateNewDocument ();
                break;
            case 1:
                // Gain access to the url and create a new document from
                // that instance
                NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
                var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

                // Load the document
                OpenDocument (url);
                break;
            default:
                // There has been an issue
                Console.WriteLine ("Issue: More than one document found...");
                break;
            }
        }
        #endregion

        #region Public Methods

        public void OpenDocument(NSUrl url) {

            Console.WriteLine ("Attempting to open: {0}", url);
            Document = new GenericTextDocument (url);

            // Open the document
            Document.Open ( (success) => {
                if (success) {
                    Console.WriteLine ("Document Opened");
                } else
                    Console.WriteLine ("Failed to Open Document");
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        public void CreateNewDocument() {
            // Create path to new file
            // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
            var docPath = Path.Combine (docsFolder, TestFilename);
            var ubiq = new NSUrl (docPath, false);

            // Create new document at path 
            Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
            Document = new GenericTextDocument (ubiq);

            // Set the default value
            Document.Contents = "(default value)";

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
                Console.WriteLine ("Save completion:" + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                } else {
                    Console.WriteLine ("Unable to Save Document");
                }
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        /// <summary>
        /// Saves the document.
        /// </summary>
        /// <returns><c>true</c>, if document was saved, <c>false</c> otherwise.</returns>
        public bool SaveDocument() {
            bool successful = false;

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
                Console.WriteLine ("Save completion: " + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                    successful = true;
                } else {
                    Console.WriteLine ("Unable to Save Document");
                    successful=false;
                }
            });

            // Return results
            return successful;
        }
        #endregion

        #region Override Methods
        public override void FinishedLaunching (UIApplication application)
        {

            // Start a new thread to check and see if the user has iCloud
            // enabled.
            new Thread(new ThreadStart(() => {
                // Inform caller that we are checking for iCloud
                CheckingForiCloud = true;

                // Checks to see if the user of this device has iCloud
                // enabled
                var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer(null);

                // Connected to iCloud?
                if (uburl == null)
                {
                    // No, inform caller
                    HasiCloud = false;
                    iCloudUrl =null;
                    Console.WriteLine("Unable to connect to iCloud");
                    InvokeOnMainThread(()=>{
                        var okAlertController = UIAlertController.Create ("iCloud Not Available", "Developer, please check your Entitlements.plist, Bundle ID and Provisioning Profiles.", UIAlertControllerStyle.Alert);
                        okAlertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        Window.RootViewController.PresentViewController (okAlertController, true, null);
                    });
                }
                else
                {   
                    // Yes, inform caller and save location the the Application Container
                    HasiCloud = true;
                    iCloudUrl = uburl;
                    Console.WriteLine("Connected to iCloud");

                    // If we have made the connection with iCloud, start looking for documents
                    InvokeOnMainThread(()=>{
                        // Search for the default document
                        FindDocument ();
                    });
                }

                // Inform caller that we are no longer looking for iCloud
                CheckingForiCloud = false;

            })).Start();
                
        }
        
        // This method is invoked when the application is about to move from active to inactive state.
        // OpenGL applications should use this method to pause.
        public override void OnResignActivation (UIApplication application)
        {
        }
        
        // This method should be used to release shared resources and it should store the application state.
        // If your application supports background execution this method is called instead of WillTerminate
        // when the user quits.
        public override void DidEnterBackground (UIApplication application)
        {
            // Trap all errors
            try {
                // Values to include in the bookmark packet
                var resources = new string[] {
                    NSUrl.FileSecurityKey,
                    NSUrl.ContentModificationDateKey,
                    NSUrl.FileResourceIdentifierKey,
                    NSUrl.FileResourceTypeKey,
                    NSUrl.LocalizedNameKey
                };

                // Create the bookmark
                NSError err;
                Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

                // Was there an error?
                if (err != null) {
                    // Yes, report it
                    Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
                }
            }
            catch (Exception e) {
                // Report error
                Console.WriteLine ("Error: {0}", e.Message);
            }
        }
        
        // This method is called as part of the transition from background to active state.
        public override void WillEnterForeground (UIApplication application)
        {
            // Is there any bookmark data?
            if (Bookmark != null) {
                // Trap all errors
                try {
                    // Yes, attempt to restore it
                    bool isBookmarkStale;
                    NSError err;
                    var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

                    // Was there an error?
                    if (err != null) {
                        // Yes, report it
                        Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
                    } else {
                        // Load document from bookmark
                        OpenDocument (srcUrl);
                    }
                }
                catch (Exception e) {
                    // Report error
                    Console.WriteLine ("Error: {0}", e.Message);
                }
            }

        }
        
        // This method is called when the application is about to terminate. Save data, if needed.
        public override void WillTerminate (UIApplication application)
        {
        }
        #endregion

        #region Events
        public delegate void DocumentLoadedDelegate(GenericTextDocument document);
        public event DocumentLoadedDelegate DocumentLoaded;

        internal void RaiseDocumentLoaded(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentLoaded != null) {
                this.DocumentLoaded (document);
            }
        }
        #endregion
    }
}

```

> [!IMPORTANT]
> Yukarıdaki kod, yukarıdaki keşfedin ve listeleme belgeler bölümünden kodu içerir. Gerçek bir uygulamada görüneceği şekilde sunabilen, burada gösterilir. Kolaylık olması için bu örnek, sabit kodlanmış, tek bir dosya ile çalışır (`test.txt`) yalnızca.

Yukarıdaki kod, uygulamanın geri kalanına çalışmak daha kolay hale getirmek için bazı iCloud sürücü kısayollar kullanıma sunar.

Ardından, herhangi bir görünüm veya belge Seçici'yi kullanarak veya bulut tabanlı belgeleriyle çalışma görünüm kapsayıcısını aşağıdaki kodu ekleyin:

```csharp
using CloudKit;
...

#region Computed Properties
/// <summary>
/// Returns the delegate of the current running application
/// </summary>
/// <value>The this app.</value>
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Bu almak için bir kısayol ekler `AppDelegate` ve yukarıda oluşturulan iCloud kısayolları erişebilirsiniz.

Yerinde şu kodla belge Seçici görünüm denetleyicisi bir Xamarin iOS 8 uygulamasında uygulama göz atalım.

## <a name="using-the-document-picker-view-controller"></a>Belge Seçici görünüm denetleyicisi kullanma

İOS 8 önce uygulamanın içinden uygulamada dışında belgeleri bulmak için hiçbir yolu olmadığından, başka bir uygulamaya ait belgelere erişmek çok zor oluyordu.

### <a name="existing-behavior"></a>Varolan davranışı

 [![](document-picker-images/image31.png "Varolan davranışı genel bakış")](document-picker-images/image31.png#lightbox)

İOS 8 önce dış bir belgeye erişme göz atalım:

1.  İlk kullanıcı, belge orijinal olarak oluşturduğunuz uygulamayı açmak gerekir.
1.  Seçili bir belge ve `UIDocumentInteractionController` yeni uygulamaya belge göndermek için kullanılır.
1.  Son olarak, özgün belgenin bir kopyasını yeni uygulama kapsayıcısında yerleştirilir.


Buradan belgeyi açın ve düzenlemek ikinci uygulama için kullanılabilir.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Bir uygulamanın kapsayıcısı dışındaki belge bulma

İOS 8'de, uygulamanın kendi uygulama kapsayıcısı dışında belgeleri kolayca erişebilir:

 [![](document-picker-images/image32.png "Bir uygulamanın kapsayıcısı dışındaki belge bulma")](document-picker-images/image32.png#lightbox)

Yeni İcloud'a belge Seçici kullanarak ( `UIDocumentPickerViewController`), bir iOS uygulamasına doğrudan bulabilir ve erişim uygulama kapsayıcısı dışında. `UIDocumentPickerViewController` Kullanıcının erişim izni vermek ve bunları düzenlemek için bir mekanizma bulunan belgeleri aracılığıyla izinleri sağlar.

Uygulamanın kendi belgeleri İcloud'a belge Seçici gösterilir ve diğer uygulamaları keşfetmek ve bunlarla çalışmak için kullanılabilir olmasını katılımı gerekir. Uygulama kapsayıcısı paylaşın, düzenleyin bir Xamarin iOS 8 uygulamanın `Info.plist` standart bir metin Düzenleyicisi'nde dosya ve sözlük altına aşağıdaki iki satırı ekleyin (arasında `<dict>...</dict>` etiketleri):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController` Belgeleri seçmesine izin veren bir harika yeni kullanıcı Arabirimi sağlar. Belge Seçici görünüm denetleyicisi bir Xamarin iOS 8 uygulamada görüntülemek için aşağıdakileri yapın:

```csharp
using MobileCoreServices;
...

// Allow the Document picker to select a range of document types
        var allowedUTIs = new string[] {
            UTType.UTF8PlainText,
            UTType.PlainText,
            UTType.RTF,
            UTType.PNG,
            UTType.Text,
            UTType.PDF,
            UTType.Image
        };

        // Display the picker
        //var picker = new UIDocumentPickerViewController (allowedUTIs, UIDocumentPickerMode.Open);
        var pickerMenu = new UIDocumentMenuViewController(allowedUTIs, UIDocumentPickerMode.Open);
        pickerMenu.DidPickDocumentPicker += (sender, args) => {

            // Wireup Document Picker
            args.DocumentPicker.DidPickDocument += (sndr, pArgs) => {

                // IMPORTANT! You must lock the security scope before you can
                // access this file
                var securityEnabled = pArgs.Url.StartAccessingSecurityScopedResource();

                // Open the document
                ThisApp.OpenDocument(pArgs.Url);

                // IMPORTANT! You must release the security lock established
                // above.
                pArgs.Url.StopAccessingSecurityScopedResource();
            };

            // Display the document picker
            PresentViewController(args.DocumentPicker,true,null);
        };

pickerMenu.ModalPresentationStyle = UIModalPresentationStyle.Popover;
PresentViewController(pickerMenu,true,null);
UIPopoverPresentationController presentationPopover = pickerMenu.PopoverPresentationController;
if (presentationPopover!=null) {
    presentationPopover.SourceView = this.View;
    presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Down;
    presentationPopover.SourceRect = ((UIButton)s).Frame;
}
```

> [!IMPORTANT]
> Geliştirici çağırmalıdır `StartAccessingSecurityScopedResource` yöntemi `NSUrl` bir harici belge erişilmeden önce. `StopAccessingSecurityScopedResource` Belge yüklendikten hemen sonra güvenlik kilidi metodu çağrılmalıdır.

### <a name="sample-output"></a>Örnek Çıktı

Yukarıdaki kod, bir belge bir iPhone cihaza çalıştırdığınızda Seçici nasıl görüntüleyebilir, bir örnek aşağıda verilmiştir:

1.  Kullanıcı uygulamayı başlatır ve ana arabirimi görüntülenir:   
 
    [![](document-picker-images/image33.png "Temel arabirim görüntülenir")](document-picker-images/image33.png#lightbox)
1.  Kullanıcı Tap'ları **eylem** ekranın üstünde düğme ve seçmek için kullanıcıdan bir **belge sağlayıcısı** kullanılabilir sağlayıcılar listesinden:   
 
    [![](document-picker-images/image34.png "Kullanılabilir sağlayıcılar listesinden bir belge sağlayıcısı seçin")](document-picker-images/image34.png#lightbox)
1.  **Belge Seçici görünüm denetleyicisi** görüntülenen seçili **belge sağlayıcısı**:   
 
    [![](document-picker-images/image35.png "Belge Seçici görünüm denetleyicisi görüntülenir")](document-picker-images/image35.png#lightbox)
1.  Kullanıcı dokunduğunda üzerinde bir **belge klasörü** içeriğini görüntülemek için:   
 
    [![](document-picker-images/image36.png "Belge klasör içeriği")](document-picker-images/image36.png#lightbox)
1.  Kullanıcının seçtiği bir **belge** ve **belge Seçici** kapatılır.
1.  Temel arabirim görünürler, **belge** dış kapsayıcı ve görüntülenen içeriği yüklenir.


Belge Seçici görünüm denetleyicisi gerçek görüntülenmesini belge kullanıcı cihazda yüklü ve hangi belge Seçici modu uygulama sonlandırıldı sağlayıcılarında bağlıdır. Yukarıdaki örnekte, Aç modunu kullanıyor, diğer modu türleri aşağıda ayrıntılı olarak açıklanmıştır.

## <a name="managing-external-documents"></a>Dış belgeleri yönetme

İOS 8, önce yukarıda açıklandığı gibi bir uygulama yalnızca uygulama kapsayıcısı parçası olan belgelere erişebilir. İOS 8'de bir uygulama, dış kaynaklardan gelen belgeleri erişebilirsiniz:

 [![](document-picker-images/image37.png "Yönetmeye harici belgelere genel bakış")](document-picker-images/image37.png#lightbox)

Kullanıcı bir belgeyi bir dış kaynaktan seçtiğinde, bir başvuru belgesini özgün belgede işaret eden bir uygulama kapsayıcısı yazılır.

Bu yeni özellik mevcut uygulamalara ekleme yardımcı olmak için çeşitli yeni özellikler eklenmiş `NSMetadataQuery` API. Genellikle, bir uygulama, uygulama kapsayıcısı içinde Canlı listesi belgelere bulunabilen belge kapsamı kullanır. Bu kapsamı kullanarak, yalnızca uygulama kapsayıcısı içindeki belgeler görüntülenecek devam eder.

Yeni bulunabilen dış belge kapsamı kullanarak uygulama kapsayıcısı dışında canlı ve meta verileri döndürülmeleri belgeleri döndürür. `NSMetadataItemUrlKey` URL'sine belge gerçekten bulunduğu yeri gösterir.

Bazen bir uygulama th başvuru tarafından işaret edilen belgelerle çalışmak üzere istememektedir. Bunun yerine, uygulama başvuru belgesini ile doğrudan çalışmak ister. Örneğin, uygulamanın kullanıcı arabiriminde uygulamanın klasöründeki belgeyi görüntülemek veya bir klasörün içine başvuruları değiştirmemiz izin vermek için isteyebilirsiniz.

İOS 8 ' deki yeni `NSMetadataItemUrlInLocalContainerKey` başvuru belgesini doğrudan erişmek için sağlanan. Bu anahtar, gerçek bir uygulama kapsayıcısında harici belge başvuru işaret eder.

`NSMetadataUbiquitousItemIsExternalDocumentKey` Belgeye bir uygulamanın kapsayıcıya dış olup olmadığını test etmek için kullanılır. `NSMetadataUbiquitousItemContainerDisplayNameKey` Bir harici belge özgün kopyasını barındıran bir kapsayıcı adı erişmek için kullanılır.

### <a name="why-document-references-are-required"></a>Belge başvuruları neden gereklidir

Dış belgelere erişmek için başvurular, iOS 8 kullanan temel nedeni, güvenlik sağlıyor. Uygulama, tüm diğer uygulamanın kapsayıcıya erişim verilir. Belge Seçici, çalışan giden işlem olduğundan ve sistem genelinde erişim sahip yapabilirsiniz.

Belge Seçici'yi kullanarak uygulama kapsayıcısı dışında belgeye almanın tek yolu olduğundan ve güvenlik kapsamı Seçici URL tarafından döndürülen, yapılır. Kapsamlı güvenlik URL'si seçili belgeye belgeye bir uygulama erişimi vermek için gerekli olan kapsamlı hakların birlikte yeterli bilgi içerir.

Güvenlik kapsamı URL'si bir dize olarak serileştirilmiş ve json'dan seri, güvenlik bilgileri kaybolur ve dosya URL'den erişilemez unutulmaması önemlidir. Belge başvuru özelliği, bu URL'ler tarafından işaret edilen dosyaları geri almak için bir mekanizma sağlar.

Uygulama edinirse bunu bir `NSUrl` başvuru belgeleri birinden zaten eklenmiş güvenlik kapsamına sahiptir ve dosyaya erişmek için kullanılabilir. Bu nedenle, son derece Geliştirici kullanmanız önerilir `UIDocument` olduğundan, tüm bu bilgileri ve işlemler için işleme.

### <a name="using-bookmarks"></a>Yer İşaretlerini Kullanma

Her zaman belirli bir belge için örneğin durumu geri yüklemesinin yaparken geri dönmek için bir uygulamanın belgeleri numaralandırmak için uygun değildir. iOS 8 doğrudan belirli bir belge hedef yer işaretleri oluşturmak için bir mekanizma sağlar.

Aşağıdaki kod, bir yer işaretinden oluşturacak bir `UIDocument`'s `FileUrl` özelliği:

```csharp
// Trap all errors
try {
    // Values to include in the bookmark packet
    var resources = new string[] {
        NSUrl.FileSecurityKey,
        NSUrl.ContentModificationDateKey,
        NSUrl.FileResourceIdentifierKey,
        NSUrl.FileResourceTypeKey,
        NSUrl.LocalizedNameKey
    };

    // Create the bookmark
    NSError err;
    Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

    // Was there an error?
    if (err != null) {
        // Yes, report it
        Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
    }
}
catch (Exception e) {
    // Report error
    Console.WriteLine ("Error: {0}", e.Message);
}
```

Mevcut yer işareti API varolan karşı bir yer işareti oluşturmak için kullanılan `NSUrl` , kaydedilebilir ve dış dosya doğrudan erişim sağlamak için yüklenir. Aşağıdaki kod, yukarıda oluşturulan bir yer işareti geri yükler:

```csharp
if (Bookmark != null) {
    // Trap all errors
    try {
        // Yes, attempt to restore it
        bool isBookmarkStale;
        NSError err;
        var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

        // Was there an error?
        if (err != null) {
            // Yes, report it
            Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
        } else {
            // Load document from bookmark
            OpenDocument (srcUrl);
        }
    }
    catch (Exception e) {
        // Report error
        Console.WriteLine ("Error: {0}", e.Message);
    }
}
```

## <a name="open-vs-import-mode-and-the-document-picker"></a>VS açın. İçeri aktarma moduna ve belge Seçici

Belge Seçici görünüm denetleyicisi işleminin iki farklı mod sunar:

1.  **Açık modu** – Bu mod, kullanıcının seçtiği ve dış belge belge Seçici kapsamlı bir güvenlik yer işareti uygulama kapsayıcısında oluşturduğunuzda.   
 
    [![](document-picker-images/image37.png "Yer işareti uygulama kapsayıcısında bir güvenlik kapsamı")](document-picker-images/image37.png#lightbox)
1.  **İçeri aktarma moduna** – Bu mod, kullanıcının seçtiği ve dış belge, belge Seçici değil bir yer işareti oluşturur ancak bunun yerine, geçici bir konuma dosyaya kopyalayın ve belge bu konuma erişim sağlar:   
 
    [![](document-picker-images/image38.png "Belge Seçici dosya geçici bir konuma kopyalayın ve belge bu konumdaki uygulama erişim sağlamak")](document-picker-images/image38.png#lightbox)   
 Uygulama herhangi bir nedenle sonlandırıldığında geçici konuma boşaltılıp boşaltılmaması ve dosya kaldırılır. Uygulama dosyasına erişimi tutması gerekiyorsa, bunu bir kopyasını oluşturun ve uygulama kapsayıcısı yerleştirin.


Modunu açın, uygulama başka bir uygulama ile işbirliği yapabilir ve belge bu uygulama ile yapılan tüm değişiklikler istediğinde yararlıdır. İçeri aktarma moduna, uygulamanın kendi değişiklikleri belgeye başka uygulamalarla paylaşma istemediği gerektiğinde kullanılır.

## <a name="making-a-document-external"></a>Bir belgeyi harici hale getirme

Yukarıda belirtildiği gibi bir iOS 8 uygulama kapsayıcıları, kendi uygulama kapsayıcısı dışında erişimi yok. Uygulama yerel olarak veya geçici bir konuma kendi kapsayıcıya yazma ardından seçilen konum bir kullanıcıya uygulama kapsayıcısı dışında sonuç belgesi taşımak için bir özel belge modunu kullanın.

Bir belge bir dış konuma taşımak için aşağıdakileri yapın:

1.  İlk yerel ya da geçici bir konumda yeni bir belge oluşturun.
1.  Oluşturma bir `NSUrl` yeni belgesine işaret eder.
1.  Yeni belge Seçici görünüm denetleyicisi açın ve geçirin `NSUrl` modu ile `MoveToService` . 
1.  Kullanıcı yeni bir konum seçtikten sonra belge geçerli konumundan yeni konumuna taşınır.
1.  Böylece dosyayı yine de oluşturma uygulama tarafından erişilebilen bir başvuru belgesini uygulamanın uygulama kapsayıcısı için yazılır.


Aşağıdaki kod, bir belge bir dış konuma taşımak için kullanılabilir: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

Yukarıdaki işlem tarafından döndürülen başvuru belgesini tam olarak bir belge Seçici açık modu tarafından oluşturulan aynıdır. Ancak, uygulama buna bir başvuru korumadan belge taşımak isteyebilirsiniz zamanlar vardır.

Bir başvuru oluşturmadan bir belge taşımak için kullanın `ExportToService` modu. Örnek: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Kullanırken `ExportToService` modu, belge dış kapsayıcıya kopyalanır ve kopyası, özgün konumunda bırakılır.

## <a name="document-provider-extensions"></a>Belge sağlayıcısı uzantıları

İOS 8 Apple son kullanıcının aslında mevcut oldukları fark etmeksizin, bulut tabanlı belgelerine erişmek için izin istiyor. Bu hedefe ulaşmak için iOS 8 yeni bir belge sağlayıcısı uzantı mekanizması sağlar.

### <a name="what-is-a-document-provider-extension"></a>Bir belge sağlayıcısı uzantısı nedir?

Kısacası, bir belge sağlayıcısı mevcut iCloud depolama konumu olarak tam olarak aynı şekilde erişilebilir bir uygulama alternatif belge depolama sağlamak için bir yol için bir geliştirici ya da üçüncü bir tarafa, uzantısıdır.

Kullanıcı bu alternatif depolama konumlarından birine belge seçiciden seçebilir ve bunlar tam aynı erişim modları (açık, içeri aktarma, taşıma veya dışarı aktarma) bu konumda bulunan dosyalarla çalışmak için kullanabilirsiniz.

Bu işlem, iki farklı uzantıları kullanılarak uygulanır:

-  **Belge Seçici uzantısı** – sağlar bir `UIViewController` öğesinin alt sınıfı bir belge bir alternatif bir depolama konumundan seçim yapması için bir grafik arabirim sağlar. Bu alt belge Seçici görünüm denetleyicisi bir parçası olarak görüntülenir.
-  **Dosya uzantısını girin** – bu dosyaların içeriğini sağlamayla ilgilenen bir UI olmayan uzantısıdır. Bu uzantıları dosya koordinasyon sağlanır ( `NSFileCoordinator` ). Bu dosya düzenleme gerekli olduğu başka bir önemli bir durumdur.


Belge sağlayıcısı uzantıları ile çalışırken, aşağıdaki diyagramda tipik veri akışı gösterilmektedir:

 [![](document-picker-images/image39.png "Bu diyagramda tipik veri akışı, belge sağlayıcısı uzantıları ile çalışırken gösterilmektedir.")](document-picker-images/image39.png#lightbox)

Aşağıdaki süreç gerçekleşir:

1.  Uygulama ile çalışmak için bir dosya seçmek izin vermek için bir belge Seçici denetleyicisi sunar.
1.  Bir alternatif dosya konumu ve özel kullanıcının seçtiği `UIViewController` uzantısı kullanıcı arabirimini görüntülemek için çağrılır.
1.  Kullanıcı bu konumdan bir dosya seçer ve belge seçiciye geçirilen URL.
1.  Belge Seçici dosya URL'si seçer ve üzerinde çalışmak kullanıcının uygulamaya döndürür.
1.  URL dosya içeriğini bir uygulamaya geri dönmek için bu dosya Düzenleyici geçirilir.
1.  Dosya Düzenleyicisi dosyasını almak için özel dosya sağlayıcı uzantısı çağırır.
1.  Dosyanın içeriğini dosya Düzenleyicisi ile döndürülür.
1.  Dosyasının içeriğini bir uygulamaya döndürülür.


### <a name="security-and-bookmarks"></a>Güvenlik ve yer işaretleri

Bu bölümde, yer işaretleri çalışır belge sağlayıcısı uzantıları aracılığıyla güvenlik ve sürekli dosya nasıl erişeceğini Hızlı Bakış sürer. Belge başvuru sisteminin bir parçası olmadıklarından İcloud'a belge sağlayıcısı, uygulama kapsayıcısı için güvenlik ve yer işaretlerini otomatik olarak kaydeder, farklı olarak belge sağlayıcısı uzantıları yok.

Örneğin: kendi şirket çapındaki güvenli veri deposu sağlayan bir kurumsal ayarında Yöneticiler erişilen veya genel iCloud sunucuları tarafından işlenen gizli Kurumsal bilgilerin istemezsiniz. Bu nedenle, yerleşik belge başvuru sistemi kullanılamaz.

Yer işareti sistem hala kullanılabilir ve işaretli bir URL doğru şekilde işlemek ve tarafından işaret edilen belgesinin içeriğini döndürmek için dosya sağlayıcı uzantısı sorumluluğundadır.

Güvenlik nedenleriyle, iOS 8 hakkında uygulama erişimi için hangi dosya sağlayıcısı içinde hangi tanımlayıcısı olan bilgileri kalıcı bir yalıtım katmanı vardır. Tüm dosya erişimi bu yalıtım katmanı tarafından denetlenir unutulmamalıdır.

Aşağıdaki diyagramda, yer işaretleri ve belge sağlayıcısı uzantısı ile çalışırken, veri akışı gösterilmektedir:

 [![](document-picker-images/image40.png "Bu diyagramda veri akışı, yer işaretleri ve belge sağlayıcısı uzantısı ile çalışırken gösterilmektedir.")](document-picker-images/image40.png#lightbox)

Aşağıdaki süreç gerçekleşir:

1.  Uygulama arka planda yaklaşık girmektir ve durumu kalıcı hale getirilmesi. Çağrı `NSUrl` alternatif depolama alanında bir dosya için bir yer işareti oluşturmak için.
1.  `NSUrl` belgede kalıcı bir URL almak için dosya sağlayıcı uzantısı çağırır. 
1.  Dosya sağlayıcı uzantısı URL'yi bir dize olarak döndürür. `NSUrl` .
1.  `NSUrl` URL bir yer işaretine oluşturur ve uygulamaya döndürür.
1.  Uygulama arka planda yüklenmesini uyanır ve durumunu geri yüklemek gereken zaman ve yer işaretine geçirir `NSUrl` .
1.  `NSUrl` dosyanın URL'si ile dosya sağlayıcı uzantısı çağırır.
1.  Dosya uzantısı sağlayıcısı dosyaya erişir ve dosyaya konumunu döndürür `NSUrl` .
1.  Dosya konumu güvenlik bilgileri ile birlikte ve uygulamaya döndürdü.


Buradan uygulama dosyaya erişim ve birlikte normal şekilde çalışır.

### <a name="writing-files"></a>Dosyalara yazma

Bu bölümde, nasıl alternatif bir konuma yazma ile belge sağlayıcısı uzantısı çalışır dosyaları Hızlı Bakış sürer. İOS uygulama dosya koordinasyon bilgileri uygulama kapsayıcı içinde diske kaydetmek için kullanır. Kısa bir süre içinde dosya başarıyla yazıldıktan sonra dosya sağlayıcı uzantısı değişikliği bildirim alırsınız.

Bu noktada, dosya sağlayıcı uzantısı alternatif bir konuma dosyasının karşıya yüklenmesi başlatabilir (veya dosya kirli ve gerektiren karşıya yükleme olarak işaretle).

### <a name="creating-new-document-provider-extensions"></a>Yeni belge sağlayıcısı uzantıları oluşturma

Yeni belge sağlayıcısı uzantıları oluşturmak, giriş niteliğindeki bu makalenin kapsamı dışında olur. Bu bilgiler, burada, bir kullanıcı, iOS cihazlarının yükledi uzantıları temel alınarak göstermek için uygulama erişimi belge iCloud konum Apple dışındaki depolama konumlarına olabilir sağlanır.

Geliştirici belge Seçici kullanırken bu duruma dikkat edin olmalıdır ve dış belgeleriyle çalışma. Bunlar bu belge iCloud barındırılan varsayımında bulunmamalıdır.

Bir depolama sağlayıcısı veya belge Seçici uzantısı oluşturma hakkında daha fazla bilgi için lütfen bkz [uygulama uzantıları giriş](~/ios/platform/extensions.md) belge.

## <a name="migrating-to-icloud-drive"></a>Sürücü İcloud'a geçirme

İOS 8, kullanıcılar, mevcut iCloud kullanarak belgeleri sistemi iOS 7 (ve önceki sistemleri) kullanılan veya varolan belgeleri yeni iCloud sürücü mekanizmasına geçirilecek seçebilirsiniz devam etmek seçebilirsiniz.

Mac OS X Yosemite Apple sağlamaz geriye dönük uyumluluk tüm belgeleri sürücü İcloud'a geçirilmelidir ya da bunlar artık güncelleştirilmesi cihazlar arasında.

Bir kullanıcı hesabının sürücü İcloud'a geçirildikten sonra yalnızca cihazlar iCloud sürücü kullanarak bu cihazları arasında belgeleri değişiklikleri yaymak erişemeyecek.

> [!IMPORTANT]
> Geliştiriciler bu makalede ele alınan yeni özellikler yalnızca kullanıcının hesabı sürücü İcloud'a geçirdiyseniz kullanılabilir olduğunu bilmeniz gerekir. 

## <a name="summary"></a>Özet

Bu makalede mevcut icloud API'leri iCloud sürücü ve yeni belge Seçici görünüm denetleyicisi desteklemek için gerekli değişiklikleri kapsamına. Dosya düzenleme ve bulut tabanlı belgelerle çalışırken önemi kapsamına. Bulut tabanlı bir Xamarin.iOS uygulaması belgelerde etkinleştirmek için gereken kurulumu kapsamında ve belgeleri belge Seçici görünüm denetleyicisi kullanarak bir uygulamanın uygulama kapsayıcısı dışında çalışan bir tanıtım bakış verilen.

Ayrıca, belge sağlayıcısı uzantıları ve neden Geliştirici belgeleri bulut tabanlı işleyebileceği uygulamaları yazarken bunları bilmeniz gerekir bu makalede kısaca ele.

## <a name="related-links"></a>İlgili bağlantılar

- [DocPicker (örnek)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [iOS 8’e Giriş](~/ios/platform/introduction-to-ios8.md)
- [Giriş uygulaması uzantıları](~/ios/platform/extensions.md)
