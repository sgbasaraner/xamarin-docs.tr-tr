---
title: Belge Seçici
description: Belge Seçici görünüm denetleyicisini uygulamanın sandbox dışındaki dosyalara kullanıcıların erişim verir. Uygulamalar arasında belgeleri paylaşmak için basit bir mekanizmadır. Kullanıcılar, birden çok uygulama ile tek bir belgenin düzenleyebilir çünkü ayrıca daha karmaşık iş akışları sağlar. Bu makalede bir Xamarin.iOS uygulaması belge seçicisini kullanarak bir giriş sağlar ve iCloud belgelerdeki değişiklikleri desteklemek için gereklidir.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: d9b98611c7d269e590ce6fe2ce0270ef71dacf1e
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="document-picker"></a>Belge Seçici

_Belge Seçici görünüm denetleyicisini uygulamanın sandbox dışındaki dosyalara kullanıcıların erişim verir. Uygulamalar arasında belgeleri paylaşmak için basit bir mekanizmadır. Kullanıcılar, birden çok uygulama ile tek bir belgenin düzenleyebilir çünkü ayrıca daha karmaşık iş akışları sağlar. Bu makalede bir Xamarin.iOS uygulaması belge seçicisini kullanarak bir giriş sağlar ve iCloud belgelerdeki değişiklikleri desteklemek için gereklidir._

Belge Seçici belgelerinin uygulamalar arasında paylaşılmasına olanak tanır. Bu belgeler, iCloud veya farklı bir uygulamanın dizinde depolanabilir. Belgeleri kümesi paylaşılan [belge sağlayıcısı uzantıları](~/ios/platform/extensions.md) kullanıcı cihazlarında yükledi. 

Uygulamaları ve bulut arasında eşitlenen belgeleri tutma zorluk nedeniyle, bunlar belirli bir miktar gerekli karmaşıklık tanıtır.

## <a name="requirements"></a>Gereksinimler

Aşağıda sunulan bu makaledeki adımları tamamlamak için gereklidir:

-  **Xcode 7 ve iOS 8 veya daha yeni** – Apple'nın Xcode 7 ve iOS 8 veya daha yeni API'ler gereken yüklenmeli ve geliştirici bilgisayarda yapılandırılmış.
-  **Visual Studio veya Mac için Visual Studio** – Mac için Visual Studio en son sürümü yüklü olmalıdır.
-  **iOS aygıtı** – iOS 8 çalıştıran bir iOS cihazını veya üstü.

## <a name="changes-to-icloud"></a>İcloud değişiklikleri

Belge Seçici yeni özelliklerini uygulamak için Apple'nın İcloud'a hizmeti aşağıdaki değişiklikler yapılmıştır:

-  Arka plan programı iCloud CloudKit kullanarak, tamamen yeniden yazılmıştır.
-  Özellikleri olan mevcut iCloud iCloud sürücü yeniden adlandırıldı.
-  İcloud'a Microsoft Windows işletim sistemi için destek eklendi.
-  İCloud klasörü Mac OS Finder eklendi.
-  iOS aygıtları Mac OS iCloud klasörünün içeriğini erişebilir.

> [!IMPORTANT]
> Apple [araçlar sağlar](https://developer.apple.com/support/allowing-users-to-manage-data/) Avrupa Birliği'nın genel veri koruma düzenleme (GDPR) düzgün bir şekilde işlemek geliştiricilere yardımcı olmak için.

## <a name="what-is-a-document"></a>Bir belge nedir?

İCloud belgede söz konusu olduğunda, tek başına tek bir varlık ve kullanıcı tarafından algılandığı şekilde. Bir kullanıcının belgeyi değiştirmek veya (örneğin, e-posta, kullanarak) diğer kullanıcılarla paylaşmak isteyebilirsiniz.

Birkaç kullanıcı hemen sayfaları gibi belgeleri olarak açılış konuşması veya numaraları dosyaları algılar, dosya türlerini vardır. Ancak, iCloud bu kavramı sınırlı değildir. Örneğin, oyun (örneğin, bir Chess eşleşme) durumunu bir belge olarak kabul edilir ve değiştirebilirsiniz iCloud içinde depolanır. Bu dosyayı bir kullanıcının cihazlar arasında geçirilen ve bunları farklı bir aygıtta bıraktığı yerden oyun seçmek sağlar.

## <a name="dealing-with-documents"></a>Belgelerle ele alma

Belge Seçici Xamarin ile kullanmak için gerekli koda Dalma, bu makalede iCloud belgeleriyle çalışma yönelik en iyi uygulamaları kapak edecek ve birkaç mevcut API'lerini yapılan değişiklikleri belge Seçici desteklemek için gereken önce.

### <a name="using-file-coordination"></a>Dosya düzenleme kullanma

Bir dosya birkaç farklı konumlardan değiştirilebildiğinden koordinasyon veri kaybını önlemek için kullanılması gerekir.

 [![](document-picker-images/image1.png "Dosya düzenleme kullanma")](document-picker-images/image1.png#lightbox)

Yukarıdaki resimde bir göz atalım:

1.  Dosya düzenleme kullanılarak bir iOS cihazı yeni bir belge oluşturur ve klasör İcloud'a kaydeder.
2.  iCloud dağıtım her aygıt için bulut değiştirilen dosya kaydeder.
3.  Bağlı bir Mac iCloud klasör değiştirilmiş dosyasında görür ve değişiklikleri dosyaya kopyalamak için dosya düzenleme kullanır.
4.  Dosya düzenleme kullanmayan bir aygıtı dosyasına bir değişiklik yapar ve klasör İcloud'a kaydeder. Bu değişiklikler diğer cihazlara anında çoğaltılır.

Özgün varsayın iOS cihazı veya Mac dosya düzenleme yaptıkları değişiklikleri şimdi kaybolur ve eşgüdümlü olmayan aygıt dosyasından sürümüyle üzerine. Veri kaybını önlemek için dosyayı düzenleme bir bulut tabanlı belgelerle çalışırken gerekliliktir.

### <a name="using-uidocument"></a>UIDocument kullanma

 `UIDocument` işleri basit hale getirir (veya `NSDocument` macOS üzerinde) geliştiricisi yaparak tüm ağır lifting. Sağlar uygulamanın kullanıcı Arabirimi engellemelerini önlemek için arka plan kuyruklarla dosya düzenleme yerleşik.

 `UIDocument` birden çok sunan herhangi bir geliştirici amaç için bir Xamarin uygulaması geliştirme efor kolaylaştırır üst düzey API'leri gerektirir.

Aşağıdaki kod öğesinin bir alt kümesi oluşturur `UIDocument` depolamak ve metin iCloud almak için kullanılan genel bir metin tabanlı belgede uygulamak için:

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

`GenericTextDocument` Yukarıda sunulan sınıfı bir Xamarin.iOS 8 uygulaması dış belgeleri ve belge Seçici ile çalışırken, bu makale kullanılır.

## <a name="asynchronous-file-coordination"></a>Zaman uyumsuz dosya düzenleme

iOS 8 yeni dosya düzenleme API'leri aracılığıyla birkaç yeni zaman uyumsuz dosya düzenleme özellikleri sağlar. İOS 8 önce var olan tüm dosya düzenleme API'lerini tamamen zaman uyumlu. Bu, geliştirici uygulamanın UI Engellemesi dosya düzenleme önlemek için queuing kendi arka plan uygulamak için sorumlu anlamına gelir.

Yeni `NSFileAccessIntent` sınıfı, dosya ve gerekli koordinasyon türünü denetlemek için birkaç seçenek işaret eden bir URL içerir. Aşağıdaki kod, bir dosyayı bir konumdan diğerine taşıma gösterir amaçlarını kullanma:

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

## <a name="discovering-and-listing-documents"></a>Keşfetmek ve belgeleri listeleme

Bul ve belgeleri listelemek için varolan kullanarak yoludur `NSMetadataQuery` API'leri. Bu bölümde için eklenen yeni özellikler ele alınacaktır `NSMetadataQuery` olduğundan olun belgelerle çalışma bile daha önce.

### <a name="existing-behavior"></a>Var olan davranışı

İOS 8, önce `NSMetadataQuery` gibi toplama yerel dosya değişiklikleri yavaştı: siler, oluşturur ve yeniden adlandırır.

 [![](document-picker-images/image2.png "NSMetadataQuery yerel dosya değişiklikleri genel bakış")](document-picker-images/image2.png#lightbox)

Yukarıdaki diyagramda:

1.  Uygulama kapsayıcısında zaten mevcut dosyaların için `NSMetadataQuery` var olan `NSMetadata` kayıtları önceden oluşturulması ve uygulamaya anında kullanılabilir olmaları Biriktiricideki.
1.  Uygulama uygulama kapsayıcıda yeni bir dosya oluşturur.
1.  Önce bir gecikme olur `NSMetadataQuery` uygulama kapsayıcısı değişikliği görür ve gerekli oluşturur `NSMetadata` kaydı.


Oluşturulmasına gecikme nedeniyle `NSMetadata` iki veri kaynakları açmak kayıt, uygulama vardı: yerel dosya değişiklikleri için bir tane ve bulut için bir temel değişiklikler.

### <a name="stitching"></a>Dikiş

İOS 8 ' de `NSMetadataQuery` Stitching doğrudan yeni bir özellik ile kullanmak daha kolay olur:

 [![](document-picker-images/image3.png "Yeni bir özellik ile NSMetadataQuery Stitching çağrılır")](document-picker-images/image3.png#lightbox)

Yukarıdaki diyagramda Stitching kullanma:

1.  Önceki gibi uygulama kapsayıcısında zaten mevcut dosyaların `NSMetadataQuery` var olan `NSMetadata` kayıtları önceden oluşturulması ve belleğe.
1.  Uygulamanın dosya düzenleme kullanarak uygulama kapsayıcıda yeni bir dosya oluşturur.
1.  Kanca uygulama kapsayıcısında değiştirilmesini ve çağrıları görür `NSMetadataQuery` gerekli oluşturmak için `NSMetadata` kaydı.
1.  `NSMetadata` Kaydı doğrudan sonra dosya oluşturulur ve uygulama için kullanılabilir hale getirilir.


Stitching kullanarak uygulama artık yerel izlemek için bir veri kaynağı açmak sahip ve dosya değişiklikleri bulut tabanlı. Uygulama üzerinde güvenebilirsiniz artık `NSMetadataQuery` doğrudan.

> [!IMPORTANT]
> Yukarıdaki bölümde sunulan gibi dosya düzenleme uygulama kullanıyorsa Dikiş yalnızca çalışır. Dosya düzenleme kullanılmadığından, API için var olan öncesi iOS 8 davranışı varsayılan.




### <a name="new-ios-8-metadata-features"></a>Yeni iOS 8 meta veri özellikleri

Aşağıdaki yeni özellikleri için eklenene `NSMetadataQuery` iOS 8'de:

-   `NSMetatadataQuery` artık bulutta depolanan yerel olmayan belgeleri listeleyebilirsiniz.
-  Yeni API meta veri bilgileri bulut tabanlı belgelere erişmek için eklenmiştir. 
-  Yeni bir `NSUrl_PromisedItems` içeriklerini kullanılabilir yerel olarak olmayabilir veya dosyaların dosya özniteliklerini erişmek için olacak API.
-  Kullanmak `GetPromisedItemResourceValue` belirli bir dosya hakkında bilgi almak veya kullanmak için bir yöntem `GetPromisedItemResourceValues` aynı anda birden fazla dosyada bilgi almak için yöntemi.


Meta veri ilgilenmek için iki yeni dosya düzenleme bayrakları eklenmiştir:

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Yukarıdaki bayraklarıyla belge dosyasının içeriği yerel olarak kendileri için kullanılacak kullanılabilir olması gerekmez.

Aşağıdaki kod kesimi nasıl kullanılacağını gösterir `NSMetadataQuery` belirli bir dosya varlığını sorgulamak ve yoksa dosyasını oluşturmak için:

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

Apple belgeleri bir uygulama için listelerken en iyi kullanıcı deneyimini'nin önizlemeleri kullanmaktır hissettirir. Bunlar birlikte çalışmak istediğiniz belge hızlı bir şekilde tanımlayabilir bu son kullanıcılar bağlamı sağlar.

İOS 8 önce belge önizlemeleri gösteren özel bir uygulaması gerekir. İOS için yeni 8 olan hızlı bir şekilde belge küçük resimleri ile çalışmak Geliştirici izin dosya sistemi öznitelikleri.

#### <a name="retrieving-document-thumbnails"></a>Belge küçük resimleri alma 

Çağırarak `GetPromisedItemResourceValue` veya `GetPromisedItemResourceValues` yöntemleri `NSUrl_PromisedItems` API, bir `NSUrlThumbnailDictionary`, döndürülür. Bu sözlükteki şu anda yalnızca bir anahtardır `NSThumbnial1024X1024SizeKey` ve kendi eşleştirme `UIImage`.

#### <a name="saving-document-thumbnails"></a>Kaydetme belge küçük resimleri

Küçük resim kaydetmek için en kolay yolu kullanmaktır `UIDocument`. Çağırarak `GetFileAttributesToWrite` yöntemi `UIDocument` ve dosyanın olduğunda küçük resim ayarlama, onu otomatik olarak kaydedilir. Arka plan programı iCloud bu değişikliği görmek ve İcloud'a yayar. Mac OS X üzerinde küçük resimleri için Geliştirici Hızlı Ara eklenti tarafından otomatik olarak oluşturulur.

Varolan API için yapılan değişiklikleri birlikte bir yerde dayalı iCloud belgelerle çalışmanın temelleri ile bir Xamarin iOS 8 belge Seçici View Controller uygulamak hazır duyuyoruz mobil uygulama.


## <a name="enabling-icloud-in-xamarin"></a>Xamarin iCloud etkinleştirme

Belge seçici bir Xamarin.iOS uygulaması kullanılmadan önce iCloud destek uygulamanızda hem Apple aracılığıyla etkinleştirilmesi gerekir. 

Aşağıdaki adımları gözden geçirme için iCloud sağlama işlemidir.

1. İCloud kapsayıcı oluşturun.
2. Uygulama hizmeti iCloud içeren bir uygulama kimliği oluşturun.
3. Bu uygulama kimliği içeren bir sağlama profili oluşturun

[Özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) Kılavuzu ilk iki adım adım anlatılmaktadır. Bir sağlama profili oluşturmak için adımları [sağlama profili](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) Kılavuzu.



Aşağıdaki adımları gözden geçirme, uygulamanız için iCloud yapılandırma işlemi:

Aşağıdakileri yapın:

1.  Proje, Mac veya Visual Studio için Visual Studio'da açın.
2.  İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçenekleri seçin.
3.  Seçenekler iletişim kutusu seç **iOS uygulama**, emin **paket tanımlayıcısı** tanımlanan bir eşleşen **uygulama kimliği** yukarıda uygulama için oluşturulan. 
4.  Seçin **iOS paket imzalama**seçin **Geliştirici kimlik** ve **sağlama profili** yukarıda oluşturduğunuz.
5.  Tıklatın **Tamam** düğmesine değişiklikleri kaydetmek ve iletişim kutusunu kapatın.
6.  Sağ `Entitlements.plist` içinde **Çözüm Gezgini** düzenleyicisinde açın.

    > [!IMPORTANT]
    > Visual Studio'da, bunun üzerinde sağ tıklayarak yetkilendirmeler Düzenleyicisi'ni açmak seçerek gerekebilir **birlikte Aç...** ve özellik listesi Düzenleyici seçme

7.  Denetleme **etkinleştirmek iCloud** , **iCloud belgeleri** , **anahtar-değer depolama** ve **CloudKit** .
8.  Olun **kapsayıcı** (yukarıda oluşturduğunuz gibi) uygulama için bulunmaktadır. Örnek: `iCloud.com.your-company.AppName`
9.  Değişiklikleri dosyaya kaydedin.

Yetkilendirmeler hakkında daha fazla bilgi için bkz [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

Yerinde yukarıdaki kurulumla uygulama artık bulut tabanlı belgeler ve yeni belge Seçici görünüm denetleyicisini kullanabilirsiniz.

## <a name="common-setup-code"></a>Genel Kurulum kodu

Belge Seçici görünüm denetleyicisiyle başlamadan önce gereken bazı standart kurulum kodu yok. Başlangıç uygulamanın değiştirerek `AppDelegate.cs` dosya ve şu şekilde görünür yapın:

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
> Yukarıdaki kod yukarıdaki keşfedin ve listeleme belgeler bölümünden kodu içerir. Gerçek bir uygulamada görüneceği şekilde tamamının, burada sunulur. Kolaylık olması için bu örnek ile tek, sabit kodlanmış bir dosya çalışır (`test.txt`) yalnızca.

Yukarıdaki kod, uygulamanın geri kalanına çalışmak kolaylaştırmak için çeşitli iCloud sürücü kısayolları kullanıma sunar.

Ardından, herhangi bir görünüm veya belge seçicisini kullanarak veya bulut tabanlı belgeleriyle çalışma görünüm kapsayıcısını aşağıdaki kodu ekleyin:

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

Bu almak için bir kısayol ekler `AppDelegate` ve yukarıda oluşturduğunuz iCloud kısayolları erişebilirsiniz.

Bu kod yerinde belge Seçici View Controller bir Xamarin iOS 8 uygulaması'nda uygulama bir bakalım.

## <a name="using-the-document-picker-view-controller"></a>Belge Seçici görünüm denetleyicisini kullanma

İOS 8 önce uygulama içinde uygulamadan dışında belgeleri bulmak için hiçbir şekilde olduğundan başka bir uygulamaya ait belgelere erişmek oldukça zor.

### <a name="existing-behavior"></a>Var olan davranışı

 [![](document-picker-images/image31.png "Varolan davranışı genel bakış")](document-picker-images/image31.png#lightbox)

İOS 8 önce dış bir belgeye erişen bir bakalım:

1.  İlk kullanıcı, ilk olarak belgeyi oluşturan uygulamayı açmak gerekir.
1.  Seçili belge ve `UIDocumentInteractionController` yeni uygulamaya belge göndermek için kullanılır.
1.  Son olarak, özgün belgenin bir kopyasını yeni uygulamanın kapsayıcısında yerleştirilir.


Buradan belgeyi açmak ve düzenlemek ikinci uygulama için kullanılabilir.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Bir uygulamanın kapsayıcı dışında belgeleri keşfetme

İOS 8'de, uygulamanın kendi uygulama kapsayıcısı dışındaki belgelere kolayca erişebilmesi için:

 [![](document-picker-images/image32.png "Bir uygulamanın kapsayıcı dışında belgeleri keşfetme")](document-picker-images/image32.png#lightbox)

Yeni İcloud'a belge Seçici kullanarak ( `UIDocumentPickerViewController`), bir iOS uygulaması, doğrudan bulmak ve erişim uygulama kapsayıcısı dışında. `UIDocumentPickerViewController` Kullanıcıya erişim izni vermek ve bunları düzenlemek için bir mekanizma bulunan belgeleri aracılığıyla izinleri sağlar.

Uygulamanın kendi belgeleri İcloud'a belge Seçicisi gösterilir ve bulmak ve bunlarla çalışmak diğer uygulamalar için kullanılabilir olmasını katılımı gerekir. Uygulama kapsayıcısı paylaşmak, düzenlemeden bir Xamarin iOS 8 uygulaması için `Info.plist` standart bir metin Düzenleyicisi'nde dosya ve sözlük altına aşağıdaki iki satırı ekleyin (arasında `<dict>...</dict>` etiketleri):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController` Kullanıcının belgeler seçmesine izin veren bir harika yeni kullanıcı Arabirimi sağlar. Belge Seçici View Controller bir Xamarin iOS 8 uygulaması'nda görüntülemek için aşağıdakileri yapın:

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
> Geliştirici çağırmalısınız `StartAccessingSecurityScopedResource` yöntemi `NSUrl` harici bir belge erişilmeden önce. `StopAccessingSecurityScopedResource` Yöntemi çağrılır, belge yüklenirken hemen sonra güvenlik kilidi serbest bırakmak için.

### <a name="sample-output"></a>Örnek Çıktı

Yukarıdaki kod bir belge bir iPhone cihazında çalıştırdığınızda Seçici nasıl görüntüleyecektir örneği şöyledir:

1.  Kullanıcı uygulamayı başlatır ve ana arabirimi görüntülenir:   
 
    [![](document-picker-images/image33.png "Ana arabirimi görüntülenir")](document-picker-images/image33.png#lightbox)
1.  Kullanıcı Tap'ları **eylem** ekranın üstündeki düğmesi ve seçmek için sorulan bir **belge sağlayıcısı** kullanılabilir sağlayıcılar listesinden:   
 
    [![](document-picker-images/image34.png "Kullanılabilir sağlayıcılar listesinden bir belge sağlayıcısı seçin")](document-picker-images/image34.png#lightbox)
1.  **Belge Seçici View Controller** görüntülenen seçili **belge sağlayıcısı**:   
 
    [![](document-picker-images/image35.png "Belge Seçici View Controller görüntülenir")](document-picker-images/image35.png#lightbox)
1.  Kullanıcı dokunur üzerinde bir **belgenin klasör** içeriğini görüntülemek için:   
 
    [![](document-picker-images/image36.png "Belgenin klasör içeriği")](document-picker-images/image36.png#lightbox)
1.  Kullanıcının seçtiği bir **belge** ve **belge Seçici** kapalı.
1.  Ana arabirimi görünürler, **belge** dış kapsayıcı ve görüntülenen içeriği yüklenir.


Belge Seçici View Controller gerçek görüntüsünü belge kullanıcı cihazda yüklü olduğu ve hangi belge Seçici modu uygulama olmuştur sağlayıcılarının bağlıdır. Yukarıdaki örnekte açık modunu kullanıyor, diğer modu türleri aşağıda ayrıntılı olarak açıklanmıştır.

## <a name="managing-external-documents"></a>Dış belgeleri yönetme

Yukarıdaki 8, iOS önce anlatıldığı gibi bir uygulama yalnızca uygulama kapsayıcısı parçası olan belgeleri erişebilir. İOS 8 bir uygulama, dış kaynaklardan belgeleri erişebilirsiniz:

 [![](document-picker-images/image37.png "Dış belgeler genel bakış yönetme")](document-picker-images/image37.png#lightbox)

Kullanıcı bir belge bir dış kaynaktan seçtiğinde başvuru belgesini özgün belgesini işaret uygulama kapsayıcısı yazılır.

Bu yeni özelliği var olan uygulamalara ekleme yardımcı olmak için birkaç yeni özellik için eklenmiştir `NSMetadataQuery` API. Genellikle, bir uygulamanın uygulama kapsayıcısı içinde dinamik listesi belgeleri bulunabilen belge kapsama kullanır. Bu kapsamı kullanarak, yalnızca belgeleri uygulama kapsayıcıdaki gösterilmeye devam eder.

Yeni bulunabilen dış belge kapsamı kullanarak uygulama kapsayıcısı dışında canlı ve meta veriler için döndürün belgeleri döndürür. `NSMetadataItemUrlKey` URL'ye belge gerçekten bulunduğu işaret edecek.

Bazen bir uygulama tarafından th başvuru işaret belgeler birlikte çalışmak istediğiniz değil. Bunun yerine, uygulama başvuru belgeyle doğrudan çalışmak ister. Örneğin, uygulamanın kullanıcı arabiriminde uygulama klasöründe belgeyi görüntülemek için veya bir klasörün içine başvuruları dolaşmak izin vermek için isteyebilirsiniz.

İOS 8 ' de yeni bir `NSMetadataItemUrlInLocalContainerKey` başvuru belgesini doğrudan erişmek için sağlanan. Bu anahtar, bir uygulama kapsayıcısındaki harici belge gerçek referansı işaret eder.

`NSMetadataUbiquitousItemIsExternalDocumentKey` Bir belge uygulamanın kapsayıcıya dış olup olmadığını test etmek için kullanılır. `NSMetadataUbiquitousItemContainerDisplayNameKey` Harici bir belge özgün kopyasını barındıran kapsayıcı adı erişmek için kullanılır.

### <a name="why-document-references-are-required"></a>Belge başvuruları neden gereklidir

Bu iOS 8 dış belgelere erişmek için başvurular kullanan ana nedeni, güvenlik olmasıdır. Hiçbir uygulama başka bir uygulama kullanıcının kapsayıcıya erişim verilir. Yalnızca belge Seçici, çalışan giden işlem dışı olduğundan ve sistem geniş erişimi yapabilirsiniz.

Belge Seçici kullanarak uygulama kapsayıcısı dışında bir belge almanın tek yolu olduğundan ve URL tarafından döndürülen Seçici güvenlik kapsamına ise. Güvenlik kapsamı URL seçili belgenin belge bir uygulama erişim vermek için gereken kapsamlı haklar birlikte yeterli bilgiler içerir.

Güvenlik kapsamı URL bir dize olarak serileştirilmiş ve ardından seri durumundan çıkarılan, güvenlik bilgilerini kaybolur ve dosya URL'den erişilemez olacaktır dikkate almak önemlidir. Belge başvurusu özelliği tarafından bu URL'leri işaret dosyaları geri almak için bir mekanizma sağlar.

Uygulama edindiğinde bunu bir `NSUrl` başvuru belgeleri birinden zaten sahip bağlı güvenlik kapsamı ve dosyaya erişmek için kullanılabilir. Bu nedenle, yüksek oranda Geliştirici kullanmanız önerilir `UIDocument` , tüm bu bilgileri ve işlemler için bunları işleme için.

### <a name="using-bookmarks"></a>Yer İşaretlerini Kullanma

Her zaman belirli bir belgeyi için örneğin, durumu geri yüklemesinin yaparken dönmek için uygulamanın belgeleri numaralandırmak için uygun değildir. iOS 8 doğrudan belirli bir belge hedef yer işaretleri oluşturmak için bir mekanizma sağlar.

Aşağıdaki kod bir yer işareti gelen oluşturacak bir `UIDocument`'s `FileUrl` özelliği:

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

Varolan yer işareti API varolan karşı bir yer işareti oluşturmak için kullanılan `NSUrl` , kaydedilebilir ve dış dosyası doğrudan erişim sağlamak için yüklenir. Aşağıdaki kod, yukarıda oluşturduğunuz bir yer işareti geri yükler:

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

## <a name="open-vs-import-mode-and-the-document-picker"></a>VS açın. İçe aktarma moduna ve belge Seçici

Belge Seçici View Controller iki farklı mod işlem özellikleri:

1.  **Açık mod** – kullanıcı seçer ve harici belge belge Seçici oluştururken güvenlik kapsamına yer işareti uygulama kapsayıcısında bu modda.   
 
    [![](document-picker-images/image37.png "Uygulama kapsayıcısında yer işareti bir güvenlik kapsamı")](document-picker-images/image37.png#lightbox)
1.  **İçe aktarma moduna** – Bu mod, kullanıcı seçtiğinde ve dış belge, belge Seçici değil bir yer işareti oluşturur ancak bunun yerine, geçici bir konuma dosyaya kopyalayın ve bu konumda belge uygulama erişim sağlar:   
 
    [![](document-picker-images/image38.png "Belge Seçici dosyayı geçici bir konuma kopyalayın ve bu konumda belge uygulama erişim sağlamak")](document-picker-images/image38.png#lightbox)   
 Uygulama herhangi bir nedenle sonlandırıldıktan sonra geçici konuma boşaltılır ve dosya kaldırıldı. Uygulama dosyasına erişimi tutması gerekiyorsa, bir kopya yapmak ve uygulama kapsayıcısı yerleştirin.


Açma modu, uygulama başka bir uygulama ile birlikte çalışın ve bu uygulama ile belgeye yapılan değişiklikler paylaşın isteyen yararlıdır. Uygulama, bir belgeyi kendi değişiklikler başka uygulamalarla paylaşma istediğinizde değil alma modu kullanılır.

## <a name="making-a-document-external"></a>Bir belge dış yapma

Yukarıda belirtildiği gibi iOS 8 uygulamanın kendi uygulama kapsayıcısı dışında kapsayıcıları erişimi yok. Uygulamayı yerel olarak veya geçici bir konuma kendi kapsayıcıya yazma sonra sonuçta elde edilen belgenin uygulama kapsayıcısı dışında konumu seçilen kullanıcıya taşımak için özel belge modu kullanın.

Bir belge harici bir konuma taşımak için aşağıdakileri yapın:

1.  Önce yeni bir belge yerel ya da geçici bir konumda oluşturun.
1.  Oluşturma bir `NSUrl` yeni belgesine işaret eder.
1.  Yeni bir belge Seçici görünümü denetleyicisi açmak ve onu geçirin `NSUrl` modu ile `MoveToService` . 
1.  Kullanıcı yeni bir konum seçtikten sonra belgenin geçerli konumundan yeni konuma taşınır.
1.  Böylece dosyayı hala oluşturma uygulama tarafından erişilebilen bir başvuru belgesini uygulamanın uygulama kapsayıcıya yazılır.


Aşağıdaki kod, bir belge harici bir konuma taşımak için kullanılabilir: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

Yukarıdaki işlem tarafından döndürülen başvuru belgesini tam olarak bir belge Seçici açık modu tarafından oluşturulan aynı değil. Ancak, uygulama bir başvuruyu tutma olmadan bir belgeyi Taşı isteyebilir zamanlar vardır.

Bir başvuru oluşturmadan bir belge taşımak için kullanın `ExportToService` modu. Örnek: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Kullanırken `ExportToService` modu, belge dış kapsayıcıya kopyalanır ve varolan kopyayı özgün konumunda bırakılır.

## <a name="document-provider-extensions"></a>Belge sağlayıcısı uzantıları

İOS 8 Apple bunlar gerçekten var olduğu olsun, bulut tabanlı belgeleri hiçbirini erişebilmeleri için son kullanıcı istemektedir. Bu hedefe ulaşmak için iOS 8 bir yeni belge sağlayıcısı uzantısı mekanizma sağlar.

### <a name="what-is-a-document-provider-extension"></a>Bir belge sağlayıcısı uzantısı nedir?

Kısacası, bir belge sağlayıcısı tam aynı şekilde varolan iCloud depolama konumu olarak erişilebilir bir uygulama alternatif belge depolama sağlamak için bir yol bir geliştirici veya bir üçüncü taraf, uzantısıdır.

Kullanıcı bu alternatif depolama konumları belge Seçicisi'nden birini seçebilirsiniz ve tam aynı erişim modları (açık, alma, taşıma veya dışa aktarma) bu konumda bulunan dosyalarıyla çalışmak için kullanabilirsiniz.

Bu, iki farklı uzantıları kullanılarak uygulanır:

-  **Belge seçicisini uzantısı** – sağlar bir `UIViewController` bir alternatif depolama konumundan bir belge seçmek kullanıcı için bir grafik arabirim sağlayan bir alt kümesi. Bu alt belge Seçici View Controller bir parçası olarak görüntülenir.
-  **Dosya uzantısını girin** – bu dosyaları içeriği sağlamayla ilgilenen bir kullanıcı Arabirimi olmayan uzantısıdır. Bu uzantılar dosya düzenleme sağlanır ( `NSFileCoordinator` ). Bu dosya düzenleme gerekli olduğu başka bir önemli bir durumdur.


Aşağıdaki diyagramda, belge sağlayıcısı uzantıları ile çalışırken, normal veri akışı gösterilmektedir:

 [![](document-picker-images/image39.png "Belge sağlayıcısı uzantıları ile çalışırken bu diyagramda normal veri akışı gösterilmektedir")](document-picker-images/image39.png#lightbox)

Aşağıdaki süreç gerçekleşir:

1.  Uygulama ile çalışmak için bir dosya seçmek izin vermek için bir belge Seçici denetleyicisi sunar.
1.  Kullanıcının seçtiği bir alternatif dosya konumu ve özel `UIViewController` uzantısı kullanıcı arabirimini görüntülemek için çağrılır.
1.  Kullanıcı bir dosyayı bu konumdan seçer ve URL belge seçiciye geçirilir.
1.  Belge Seçici dosyanın URL seçer ve kullanıcının üzerinde çalışmak uygulamaya döndürür.
1.  URL uygulamaya dosyaların içeriğini döndürmek için dosya Düzenleyicisi'ni geçirilir.
1.  Dosya Düzenleyicisi dosyasını almak için özel dosya sağlayıcısı uzantısını çağırır.
1.  Dosya içeriğini dosya düzenleyiciye döndürülür.
1.  Dosya içeriğini uygulamaya döndürülür.


### <a name="security-and-bookmarks"></a>Güvenlik ve yer işaretleri

Bu bölümde nasıl belge sağlayıcısı uzantıları olan yer işaretleri works aracılığıyla güvenlik ve kalıcı dosya erişimini hızlı bir göz atalım. Belge başvuru sisteminin parçası olmadıklarından otomatik olarak güvenlik ve yer işaretleri uygulama kapsayıcıya kaydeder, belge sağlayıcısı iCloud belge sağlayıcısı uzantıları yok.

Örneğin: kendi şirket çapında güvenli veri deposu sağlayan bir kurumsal ayarında Yöneticiler erişilen veya ortak iCloud sunucuları tarafından işlenen gizli şirket bilgileri istemezsiniz. Bu nedenle, yerleşik belge başvuru sistemi kullanılamaz.

Yer işareti sistem hala kullanılabilir ve doğru şekilde işaretli bir URL işlemek ve tarafından işaret belgesinin içeriğini döndürmek için dosya sağlayıcısı uzantısını sorumluluğundadır.

Güvenlik nedeniyle, iOS 8 hakkında uygulama erişimi hangi dosya sağlayıcısı içinde hangi tanımlayıcısına olan bilgi devam ederse bir yalıtım katmanı vardır. Tüm dosya erişimi bu yalıtım katmanı tarafından denetlenir unutulmamalıdır.

Aşağıdaki diyagramda yer işaretleri ve bir belge sağlayıcısı uzantısı ile çalışırken, veri akışı gösterilmektedir:

 [![](document-picker-images/image40.png "Bu diyagramda yer işaretleri ve bir belge sağlayıcısı uzantısı ile çalışırken veri akışı gösterilmektedir")](document-picker-images/image40.png#lightbox)

Aşağıdaki süreç gerçekleşir:

1.  Uygulama hakkında arka plan girmek için ve durumuna kalıcı olması gerekiyor. Çağırır `NSUrl` alternatif depolama alanında bir dosya için bir yer işareti oluşturmak için.
1.  `NSUrl` belgede kalıcı bir URL almak için dosya sağlayıcısı uzantısını çağırır. 
1.  Dosya sağlayıcısı uzantısını URL'yi bir dize olarak döndürür. `NSUrl` .
1.  `NSUrl` URL bir yer işaretine sunmaktadır ve uygulamaya döndürür.
1.  Uygulama arka planda yüklenmesini uyanır ve durumunu geri yüklemek gerektiğinde işaretine geçirir `NSUrl` .
1.  `NSUrl` dosyanın URL'sini dosya sağlayıcısı uzantısıyla çağırır.
1.  Dosya uzantısı sağlayıcısı dosyası erişir ve dosya konumunu döndürür `NSUrl` .
1.  Dosya konumu güvenlik bilgileri ile birlikte ve uygulamaya döndürdü.


Buradan, uygulama dosyaya erişmek ve ile normal olarak çalışır.

### <a name="writing-files"></a>Dosyalara yazma

Bu bölümde nasıl alternatif bir konuma yazma ile belge sağlayıcısı uzantısı çalışır dosyaları hızlı bir göz atalım. İOS uygulamasının uygulama kapsayıcısı içinde disk bilgileri kaydetmek için dosyayı düzenleme kullanır. Kısa süre içinde dosya başarılı bir şekilde yazıldıktan sonra dosya sağlayıcısı uzantısını değişikliği bildirilir.

Bu noktada, dosya sağlayıcısı uzantısını dosyayı alternatif bir konuma yüklemeyi başlatabilir (veya dosyayı karşıya yükleyin kirli ve gerektiren olarak işaretle).

### <a name="creating-new-document-provider-extensions"></a>Yeni belge sağlayıcısı uzantıları oluşturma

Yeni belge sağlayıcısı uzantıları oluşturma giriş bu makalenin kapsamı dışında bağlıdır. Bu bilgiler, burada, kullanıcı iOS cihazını yükledi uzantıları dayanan göstermek için bir uygulama erişim belge depolama konumları iCloud konumu sağlanan Apple dışında gerekebilir sağlanır.

Geliştirici belge Seçici kullanırken bu olgu dikkat etmeniz gerekir ve dış belgeleriyle çalışma. Bunlar bu belgenin iCloud içinde barındırılan varsayımında bulunmamalıdır.

Bir depolama sağlayıcısı veya belge seçicisini uzantısı oluşturma hakkında daha fazla bilgi için lütfen bkz [uygulama uzantıları giriş](~/ios/platform/extensions.md) belge.

## <a name="migrating-to-icloud-drive"></a>Sürücü İcloud'a geçirme

İOS 8, kullanıcılar, varolan iCloud kullanarak belgeler sistem iOS 7 (ve önceki sistemleri) kullanılan veya var olan belgeler için yeni iCloud sürücü mekanizması geçirmek seçebilirsiniz devam etmeyi seçebilirsiniz.

Mac OS X Yosemite üzerinde Apple sağlamaz geriye dönük uyumluluk tüm belgeleri sürücü İcloud'a geçirilmesi gerekir ya da bunlar artık güncelleştirilmesi aygıtlarda.

Bir kullanıcının hesabını sürücü İcloud'a geçirildikten sonra iCloud sürücü kullanarak yalnızca cihazlarda bu cihazlar arasında belgeleri değişiklikleri yaymak yapabilirsiniz.

> [!IMPORTANT]
> Geliştiricilerin bu makalede ele alınan yeni özellikler yalnızca kullanıcı hesabının sürücü İcloud'a geçirdiyseniz kullanılabilir olduğunu bilmeniz gerekir. 

## <a name="summary"></a>Özet

Bu makalede, varolan icloud API'leri iCloud sürücü ve yeni belge Seçici View Controller desteklemek için gereken değişiklikleri ele. Dosya eşgüdümü ve neden bulut tabanlı belgelerle çalışırken önemlidir kapsamına. Bulut tabanlı bir Xamarin.iOS uygulaması belgelerde etkinleştirmek için gereken kurulum kapsamdaki ve bir uygulamanın uygulama belge Seçici görünüm denetleyicisini kullanarak kapsayıcı dışında belgelerle çalışma bir tanıtım bakış verilen.

Ayrıca, bu makalede kısaca belge sağlayıcısı uzantıları ve neden Geliştirici belgeleri bulut tabanlı işleyebilir uygulamaları yazarken bunlarla ilgili dikkat etmeniz gerekir ele.

## <a name="related-links"></a>İlgili bağlantılar

- [DocPicker (örnek)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [iOS 8’e Giriş](~/ios/platform/introduction-to-ios8.md)
- [Uygulama uzantıları giriş](~/ios/platform/extensions.md)
