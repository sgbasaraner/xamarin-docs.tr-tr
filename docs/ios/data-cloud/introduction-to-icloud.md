---
title: İCloud Xamarin.iOS ile kullanma
description: Bu belge, iCloud ve Xamarin.iOS uygulamalarının kullanımını açıklar. Anahtar-değer deposu, belge depolaması ve İcloud'a yedekleme ele alınmaktadır.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: b72ecc40994d9336c4941f3db700796edd80e81f
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353223"
---
# <a name="using-icloud-with-xamarinios"></a>İCloud Xamarin.iOS ile kullanma

İOS 5, iCloud depolama API'si, uygulamaların kullanıcı belgeleri ve uygulamaya özgü verileri merkezi bir konuma kaydedin ve bu öğeleri kullanıcının tüm cihazlardan erişmesine izin verir.

Dört tür depolama vardır:

- **Anahtar-değer deposu** - küçük miktarlarda veri uygulamanız ile paylaşım için bir kullanıcının diğer cihazlarına.

- **UIDocument depolama** - kullanıcının iCloud hesabıyla UIDocument öğesinin kullanarak belgeleri ve diğer verileri depolamak için.

- **CoreData** -SQLite veritabanı depolama alanı.

- **Tek tek dosyalar ve dizinler** - çok sayıda farklı dosya dosya sistemindeki doğrudan yönetmek için.

Bu belge, ilk iki tür - UIDocument görünümünüzde ve anahtar-değer çiftleri - ve Xamarin.iOS içinde bu özellikleri kullanmak nasıl ele alır.

> [!IMPORTANT]
> Apple [araçlar sağlar](https://developer.apple.com/support/allowing-users-to-manage-data/) Avrupa Birliği'nin genel veri koruma yönetmeliği (GDPR) düzgün bir şekilde işlemek geliştiricilerin yardımcı olmak için.

## <a name="requirements"></a>Gereksinimler

- Xamarin.iOS en son kararlı sürümü
- Xcode 8 veya daha yeni
- Visual Studio Mac veya Visual Studio 2015 veya daha yeni.

## <a name="preparing-for-icloud-development"></a>İCloud geliştirme için hazırlama

Uygulamalar, iCloud hem de kullanmak için yapılandırılmalıdır [Apple sağlama portalı](https://developer.apple.com/account/ios/overview.action) ve proje. İCloud için geliştirme (veya örneklerini gözden çalışırken) aşağıdaki adımları izlemeden önce.

İCloud erişmek için bir uygulama düzgün şekilde yapılandırmak için:

-   **Teamıd değeri Bul** -oturum açma [developer.apple.com](http://developer.apple.com) ziyaret edin **üye Merkezi'nde > hesabınızı > Geliştirici hesap özeti** , takım kimliği (veya tek geliştiriciler için tek tek Kimliği almak için ). 10 karakter olacak ( **A93A5CM278** örneğin)-Bu, "kapsayıcı tanımlayıcı" bölümünü oluşturur.

-   **Yeni bir uygulama kimliği oluşturma** - uygulama kimliği oluşturma, konusunda özetlenen adımları [Store teknolojileri bölümü için cihaz sağlama kılavuzuna sağlama](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)ve kontrol ettiğinizden emin olun **iCloud** olarak bir hizmet izin verilen:

 [![](introduction-to-icloud-images/icloud-sml.png "İCloud izin verilen bir hizmet olarak işaretleyin")](introduction-to-icloud-images/icloud.png#lightbox)

- **Yeni bir sağlama profili oluşturma** - bir sağlama profili oluşturmak için özetlenen adımları izleyin [cihaz sağlama Kılavuzu](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) .

- **Entitlements.plist dosyanıza Kapsayıcı tanımlayıcı Ekle** -kapsayıcı tanımlayıcı biçimi `TeamID.BundleID`. Daha fazla bilgi için [Yetkilendirmelerle çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

- **Proje özelliklerini yapılandırma** - dosya olun Info.plist dosyasında **paket grubu tanımlayıcısı** eşleşen **paket kimliği** ayarlanır [uygulama kimliği oluşturma ](~/ios/deploy-test/provisioning/capabilities/index.md); İOS paket grubu imzalama kullanan bir **sağlama profili** iCloud App Service ile bir uygulama kimliği içeren ve **özel yetkilendirmeler** dosya seçildi. Bu tüm Visual Studio'da proje Özellikler bölmesi altında yapılabilir.

- **Cihazınızda İcloud'u etkinleştir** - Git **Ayarları > iCloud** ve cihaz açtığınızdan emin olun.
Seçin ve açın **belgeler ve veriler** seçeneği.

- **İCloud test etmek için bir cihaz kullanmanız gerekir** -Simulator'da çalışmaz.
Aslında, gerçekten iCloud iş başında görmek için aynı Apple ID ile oturum açmış iki veya daha fazla aygıtlar gerekir.


## <a name="key-value-storage"></a>Anahtar-değer deposu

Anahtar-değer deposu küçük miktarlarda arasında kalıcı cihazlar - bir kitap ya da dergi görüntülenen son sayfa gibi bir kullanıcı gibi veri yöneliktir. Anahtar-değer deposu yedeklenirken veri için kullanılmamalıdır.

Anahtar-değer depolama kullanırken dikkat edilmesi gereken bazı sınırlamalar vardır:

- **En fazla anahtar boyutu** -anahtar adları 64 bayttan daha uzun olamaz.

- **Maksimum değer boyutu** -tek bir değer olarak birden fazla 64 KB depolanamıyor.

- **Bir uygulama için en fazla anahtar-değer deposu boyutu** -uygulamalar yalnızca olarak toplam en fazla 64 KB anahtar-değer veri depolayabilir. Bu sınırı aşan anahtarlarını ayarlamak için denemeleri başarısız olur ve önceki değeri korunur.

- **Veri türleri** - yalnızca temel türler ister dizeler, sayılar ve Boole değerlerini depolanabilir.

**İCloudKeyValue** örnek, nasıl çalıştığını gösterir. Örnek kod, her cihaz için adlı bir anahtar oluşturur: bir cihazda bu anahtarı ayarlayıp başkalarına yayılan değeri izleyin. Ayrıca, tek seferde birçok cihazda düzenlerseniz, "herhangi bir cihazda - düzenlenebilen paylaşılan" adlı bir anahtar oluşturur, iCloud yayılan ve "(bir zaman damgası değişiklik kullanarak) WINS" değeri karar verir.

Bu ekran örnek kullanımını göstermektedir. İCloud değişiklik bildirimi alındığında bunlar kayan ekranın metin görünümünde yazdırılır ve giriş alanlarına güncelleştirildi.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Cihazlar arasında ileti akışı")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Veri alma ve ayarlama

Bu kod, bir dize değeri ayarlama işlemi gösterilmektedir.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Eşitleme çağırma değeri yalnızca yerel disk depolama için kalıcı sağlar. İcloud eşitleme arka planda gerçekleşir ve "uygulama kodu tarafından zorlanamıyor". Düşük (veya bağlantısı kesilmiş) ağ ise, bir güncelleştirme çok daha uzun sürebilir ancak iyi bir ağ bağlantısına sahip eşitleme genellikle 5 saniye içinde gerçekleşir.

Bu kod bir değerle alabilirsiniz:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Değeri, yerel veri deposundan alınır: Bu yöntem, "son" değerini almak için iCloud sunucularla iletişim kurmayı değil. iCloud yerel veri deposundaki kendi zamanlamaya göre güncelleştirir.

### <a name="deleting-data"></a>Verileri silme

Bir anahtar-değer çifti tamamen kaldırmak için Remove yöntemi bu gibi kullanın:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Değişiklikleri gözleme

Değerleri bir gözlemci ekleyerek iCloud tarafından değiştirildiğinde bir uygulamanın bildirim de alabilir `NSNotificationCenter.DefaultCenter`.
Aşağıdaki kodu **KeyValueViewController.cs** `ViewWillAppear` yöntemi için bu bildirimleri dinleyip hangi anahtarları değiştirilmiş bir liste oluşturmak nasıl gösterir:

```csharp
keyValueNotification =
NSNotificationCenter.DefaultCenter.AddObserver (
    NSUbiquitousKeyValueStore.DidChangeExternallyNotification, notification => {
    Console.WriteLine ("Cloud notification received");
    NSDictionary userInfo = notification.UserInfo;

    var reasonNumber = (NSNumber)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangeReasonKey);
    nint reason = reasonNumber.NIntValue;

    var changedKeys = (NSArray)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangedKeysKey);
    var changedKeysList = new List<string> ();
    for (uint i = 0; i < changedKeys.Count; i++) {
        var key = changedKeys.GetItem<NSString> (i); // resolve key to a string
        changedKeysList.Add (key);
    }
    // now do something with the list...
});
```

Kodunuz sonra yerel bir kopyasını veya kullanıcı Arabirimi yeni değerlerle güncelleme gibi değiştirilen anahtarların listesi ile bazı eylemler gerçekleştirebilir.

Olası bir değişiklik nedenler: (0) HtmlRadioButton, InitialSyncChange (1) veya QuotaViolationChange (2). Nedeni erişim ve gerekirse farklı bir işlem gerçekleştirme (örneğin, sonucu olarak bazı anahtarları kaldırmanız gerekebilir bir *QuotaViolationChange*).

## <a name="document-storage"></a>Belge depolama

İcloud'a belge depolama uygulamanıza (ve kullanıcı) önemli olan verileri yönetmek için tasarlanmıştır. Dosyaları ve uygulamanızı sağlayan iCloud tabanlı yedekleme ve kullanıcının tüm cihazlarda işlevselliği paylaşımı aynı anda sırada çalıştırmak için gereken diğer verileri yönetmek için kullanılabilir.

Bu diyagramda, tüm birbirine nasıl uyduğunu gösterilmektedir. Her cihazın yerel depolama (UbiquityContainer) ve arka plan programı gönderme ve bulutta veri alma üstlenir işletim sisteminin iCloud üzerinde kaydedilen veriler var. Tüm dosya erişimi UbiquityContainer FilePresenter/eş zamanlı erişimi engellemek için FileCoordinator yapılmalıdır. `UIDocument` Sınıfı bu sizin için gerçekleştirir; Bu örnekte UIDocument kullanma işlemi gösterilmektedir.

 [![](introduction-to-icloud-images/icloud-overview.png "Belge depolamaya genel bakış")](introduction-to-icloud-images/icloud-overview.png#lightbox)

Basit bir iCloudUIDoc örnek uygulayan `UIDocument` tek bir metin alanı içeren bir alt sınıfı. Metin olarak işlenen bir `UITextView` ve düzenlemeleri yayılır İcloud'a diğer cihazlar tarafından bir bildirim iletisi kırmızı olarak gösterilir. Örnek kod çakışma çözümü gibi daha gelişmiş iCloud özellikleri ile ilgilenir değil.

Metin değiştirme ve tuşuna basarak örnek uygulaması - bu ekran görüntüsünde gösterilmiştir **UpdateChangeCount** belge diğer cihazlara iCloud ile eşitlenir.

 [![](introduction-to-icloud-images/iclouduidoc.png "Bu ekran metin değiştirme ve UpdateChangeCount tuşuna basarak sonra örnek uygulamayı gösterir.")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Beş iCloudUIDoc örnek bölümü vardır:

1. **UbiquityContainer erişme** -iCloud etkin olup olmadığını ve bu durumda belirlemek, uygulamanızın iCloud depolama alanı yolu.

1. **UIDocument alt sınıf oluşturarak** -Orta iCloud depolama ve model nesnelerinizi arasında bir sınıf oluşturun.

1. **Bulma ve açma iCloud belgeleri** -kullanın `NSFileManager` ve `NSPredicate` iCloud belgeleri bulun ve bunları açmak için.

1. **İCloud belgeleri görüntüleme** -özelliklerinden kullanıma sunma, `UIDocument` böylece kullanıcı Arabirimi denetimleri ile etkileşim kurabilir.

1. **İCloud belgeleri kaydetme** -kullanıcı Arabiriminde yapılan değişiklikler iCloud ve disk ile kalıcı emin olun.

Tüm iCloud işlemleri çalıştırın (veya çalışması gereken) ve böylece bunlar birşeyler olmasını beklerken zaman uyumsuz olarak engellemez. Bunu gerçekleştirmenin örnekte üç farklı şekilde görürsünüz:

 **İş parçacığı** - `AppDelegate.FinishedLaunching` ilk çağrı `GetUrlForUbiquityContainer` ana iş parçacığı engellenmesini önlemek için başka bir iş parçacığı üzerinde gerçekleştirilir.

 **NotificationCenter** - bildirimler zaman uyumsuz olduğunda kayıt gibi işlemler `NSMetadataQuery.StartQuery` tamamlandı.

 **Tamamlama işleyicileri** - yöntemleriyle gibi zaman uyumsuz işlemleri tamamlama çalıştırmak için geçen `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>UbiquityContainer erişme

İcloud'a belge depolama kullanmanın ilk adımı iCloud etkin olup olmadığını ve bu durumda belirlemektir "yaygınlığıyla kapsayıcı" konumunu (cihazda iCloud etkin dosyaların depolandığı dizin).

Bu kodu `AppDelegate.FinishedLaunching` örnek yöntemi.

```csharp
// GetUrlForUbiquityContainer is blocking, Apple recommends background thread or your UI will freeze
ThreadPool.QueueUserWorkItem (_ => {
    CheckingForiCloud = true;
    Console.WriteLine ("Checking for iCloud");
    var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer (null);
    // OR instead of null you can specify "TEAMID.com.your-company.ApplicationName"

    if (uburl == null) {
        HasiCloud = false;
        Console.WriteLine ("Can't find iCloud container, check your provisioning profile and entitlements");

        InvokeOnMainThread (() => {
            var alertController = UIAlertController.Create ("No \uE049 available",
            "Check your Entitlements.plist, BundleId, TeamId and Provisioning Profile!", UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Destructive, null));
            viewController.PresentViewController (alertController, false, null);
        });
    } else { // iCloud enabled, store the NSURL for later use
        HasiCloud = true;
        iCloudUrl = uburl;
        Console.WriteLine ("yyy Yes iCloud! {0}", uburl.AbsoluteUrl);
    }
    CheckingForiCloud = false;
});
```

Örnek Bunu yapmak olsa da, Apple, uygulama ön plana gelen her GetUrlForUbiquityContainer çağırma önerir.

### <a name="creating-a-uidocument-subclass"></a>UIDocument alt sınıf oluşturma

Tüm iCloud dosyaları ve dizinleri (IE. herhangi bir şey UbiquityContainer dizinde depolanan) NSFilePresenter protokolünü uygulamaya ve bir NSFileCoordinator yazma NSFileManager yöntemler kullanılarak yönetilmesi gerekir.
En basit yolu, tüm gerçekleştirmek için bunu kendiniz fakat bunu yapan UIDocument alt sınıfı değil yazmaktır tümünü sizin için.

UIDocument alt iCloud ile çalışmak için uygulamanız gereken yalnızca iki yöntem vardır:

- **LoadFromContents** -NSData, listelenen model sınıfı/es açmak, dosyanın içeriğinin geçirir.

- **ContentsForType** -disk (ve buluta) kaydetmek için istek modeli sınıfı/es NSData gösterimini temin etmeniz amacıyla.

Bu örnek koddan **iCloudUIDoc\MonkeyDocument.cs** nasıl UIDocument uygulanacağı gösterilmektedir.

```csharp
public class MonkeyDocument : UIDocument
{
    // the 'model', just a chunk of text in this case; must easily convert to NSData
    NSString dataModel;
    // model is wrapped in a nice .NET-friendly property
    public string DocumentString {
        get {
            return dataModel.ToString ();
        }
        set {
            dataModel = new NSString (value);
        }
    }
    public MonkeyDocument (NSUrl url) : base (url)
    {
        DocumentString = "(default text)";
    }
    // contents supplied by iCloud to display, update local model and display (via notification)
    public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("LoadFromContents({0})", typeName);

        if (contents != null)
            dataModel = NSString.FromData ((NSData)contents, NSStringEncoding.UTF8);

        // LoadFromContents called when an update occurs
        NSNotificationCenter.DefaultCenter.PostNotificationName ("monkeyDocumentModified", this);
        return true;
    }
    // return contents for iCloud to save (from the local model)
    public override NSObject ContentsForType (string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("ContentsForType({0})", typeName);
        Console.WriteLine ("DocumentText:{0}",dataModel);

        NSData docData = dataModel.Encode (NSStringEncoding.UTF8);
        return docData;
    }
}
```

Veri modeli Bu örnekte çok basit - tek bir metin alanı. Veri modelinizi gibi karmaşık bir Xml belgesi veya ikili veri gibi gerekli olabilir. Birincil UIDocument uygulamasının model sınıflarınızı ve diske kaydedilmiş/yüklenen bir NSData gösterimi arasında çeviri yapmak için rolüdür.

### <a name="finding-and-opening-icloud-documents"></a>Bulma ve açma iCloud belgeleri

Örnek uygulamayı tek bir dosyayı - test.txt - yalnızca ilgilenir. böylece kodda **AppDelegate.cs** oluşturur bir `NSPredicate` ve `NSMetadataQuery` özellikle bu dosya için aranacak. `NSMetadataQuery` Zaman uyumsuz olarak çalışır ve tamamlandığında bir bildirim gönderir. `DidFinishGathering` bildirim gözlemci tarafından çağrıldığında, sorgu durdurur ve kullandığı LoadDocument çağırır `UIDocument.Open` dosyasını yükleyin ve görüntüleyin denemek için bir tamamlama işleyicisi yöntemiyle bir `MonkeyDocumentViewController`.

```csharp
string monkeyDocFilename = "test.txt";
void FindDocument ()
{
    Console.WriteLine ("FindDocument");
    query = new NSMetadataQuery {
        SearchScopes = new NSObject [] { NSMetadataQuery.UbiquitousDocumentsScope }
    };

    var pred = NSPredicate.FromFormat ("%K == %@", new NSObject[] {
        NSMetadataQuery.ItemFSNameKey, new NSString (MonkeyDocFilename)
    });

    Console.WriteLine ("Predicate:{0}", pred.PredicateFormat);
    query.Predicate = pred;

    NSNotificationCenter.DefaultCenter.AddObserver (
        this,
        new Selector ("queryDidFinishGathering:"),
        NSMetadataQuery.DidFinishGatheringNotification,
        query
    );

    query.StartQuery ();
}

[Export ("queryDidFinishGathering:")]
void DidFinishGathering (NSNotification notification)
{
    Console.WriteLine ("DidFinishGathering");
    var metadataQuery = (NSMetadataQuery)notification.Object;
    metadataQuery.DisableUpdates ();
    metadataQuery.StopQuery ();

    NSNotificationCenter.DefaultCenter.RemoveObserver (this, NSMetadataQuery.DidFinishGatheringNotification, metadataQuery);
    LoadDocument (metadataQuery);
}

void LoadDocument (NSMetadataQuery metadataQuery)
{
    Console.WriteLine ("LoadDocument");

    if (metadataQuery.ResultCount == 1) {
        var item = (NSMetadataItem)metadataQuery.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);
        doc = new MonkeyDocument (url);

        doc.Open (success => {
            if (success) {
                Console.WriteLine ("iCloud document opened");
                Console.WriteLine (" -- {0}", doc.DocumentString);
                viewController.DisplayDocument (doc);
            } else {
                Console.WriteLine ("failed to open iCloud document");
            }
        });
    } // TODO: if no document, we need to create one
}
```

### <a name="displaying-icloud-documents"></a>İCloud belgeleri görüntüleme

Bir UIDocument görüntülemek için başka bir model sınıfı farklı olmamalıdır
- UI denetimleri, büyük olasılıkla kullanıcı tarafından düzenlenebilir ve modele geri yazılan özellikleri görüntülenir.

Örnekte **iCloudUIDoc\MonkeyDocumentViewController.cs** MonkeyDocument metni görüntüler bir `UITextView`. `ViewDidLoad` gönderilen bildirim dinler `MonkeyDocument.LoadFromContents` yöntemi. `LoadFromContents` Yeni veri dosyası için iCloud sahip olduğunda bildirim belge güncelleştirilip güncelleştirilmediğini gösterir. böylece çağrılır.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Örnek kod bildirim işleyicisi - kullanıcı Arabirimi, bu durumda herhangi bir çakışma algılaması veya çözüm olmadan güncelleştirmek için bir yöntem çağırır.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>İCloud belgeleri kaydetme

Bir UIDocument çağırabilirsiniz İcloud'a eklemek için `UIDocument.Save` doğrudan (yeni belgeler için yalnızca) veya kullanarak mevcut bir dosyayı taşıma `NSFileManager.DefaultManager.SetUbiquitious`. Örnek kod, doğrudan bu kodla yaygınlığıyla kapsayıcıda yeni bir belge oluşturur. (iki tamamlama işleyicisi vardır, burada bir `Save` işlemi, diğeri için Aç):

```csharp
var docsFolder = Path.Combine (iCloudUrl.Path, "Documents"); // NOTE: Documents folder is user-accessible in Settings
var docPath = Path.Combine (docsFolder, MonkeyDocFilename);
var ubiq = new NSUrl (docPath, false);
var monkeyDoc = new MonkeyDocument (ubiq);
monkeyDoc.Save (monkeyDoc.FileUrl, UIDocumentSaveOperation.ForCreating, saveSuccess => {
Console.WriteLine ("Save completion:" + saveSuccess);
if (saveSuccess) {
    monkeyDoc.Open (openSuccess => {
        Console.WriteLine ("Open completion:" + openSuccess);
        if (openSuccess) {
            Console.WriteLine ("new document for iCloud");
            Console.WriteLine (" == " + monkeyDoc.DocumentString);
            viewController.DisplayDocument (monkeyDoc);
        } else {
            Console.WriteLine ("couldn't open");
        }
    });
} else {
    Console.WriteLine ("couldn't save");
}
```

Belgedeki sonraki değişiklikler "kaydedilmez" doğrudan, bunun yerine size bildirmek `UIDocument` ile değişen `UpdateChangeCount`, ve bir disk işlemi Kaydet otomatik olarak zamanlar:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>İCloud belgeleri yönetme

Kullanıcıların iCloud belgeleri yönetebilir **belgeleri** uygulamanızı; ayarlarından dışında "yaygınlığıyla kapsayıcısının" dizin dosya listesi ve silmek için kullanılan görüntüleyebilirsiniz. Uygulama kodu, belgeler kullanıcı tarafından burada silinir durumu işleyebilen olmalıdır. İç uygulama verileri depolamaz **belgeleri** dizin.

 [![](introduction-to-icloud-images/icloudstorage.png "İcloud'a belge iş akışını yönetme")](introduction-to-icloud-images/icloudstorage.png#lightbox)



İCloud etkinleştirilmiş bir uygulama bu uygulamayla ilgili iCloud belgeleri durumunu bildiren cihazını kaldırmayı denediğinizde kullanıcılar farklı uyarılar da alırsınız.

 [![](introduction-to-icloud-images/icloud-delete1.png "Kullanıcı cihazlarından iCloud etkinleştirilmiş bir uygulama kaldırmaya çalıştığında örnek iletişim")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Kullanıcı cihazlarından iCloud etkinleştirilmiş bir uygulama kaldırmaya çalıştığında örnek iletişim")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>İcloud'a yedekleme

İCloud yedeklemeyi, geliştiriciler tarafından doğrudan erişilebilen bir özelliği olmasa da, uygulamanızı tasarlarken, kullanıcı deneyimini etkileyebilir.
Apple sağlar [iOS veri depolama yönergeleri](http://developer.apple.com/icloud/documentation/data-storage/) geliştiricilerin iOS uygulamalarını izleyin.

Uygulamanızı olmayan kullanıcı tarafından oluşturulan (örneğin, sorun başına içeriğinin hundred-plus megabayt depolayan bir dergi okuyucu uygulama) büyük dosyalar olup olmadığını depolar en önemli husustur. Apple, bu tür, İcloud'a yedekleme ve gereksiz yere kullanıcının iCloud kota dolgu verileri depolamaz tercih eder.

Büyük miktarlarda benzer veri depolayan uygulamalar ya da bunu yedeklenen (örn. değil kullanıcı dizinleri birinde depolamanız gerekir Önbellekler veya tmp) veya `NSFileManager.SetSkipBackupAttribute` İcloud'a yedekleme işlemleri sırasında bunları yoksayar, böylece bu dosyalara bir bayrak uygulamak için.

## <a name="summary"></a>Özet

Bu makalede, iOS 5 dahil yeni iCloud özelliğini kullanıma sunmuştur. Bu, projenizi iCloud kullanmak üzere yapılandırmak için gereken ve ardından iCloud özellikleri uygulamak örnekler sağlanan adımları incelenir.

İCloud az miktarda bir benzer şekilde NSUserPreferences depolanan verileri depolamak için nasıl kullanılabileceğini anahtar-değer deposu örneği gösterilmektedir. Nasıl daha fazla karmaşık veri depolanabilir ve birden çok cihazda iCloud ile eşitlenmiş UIDocument örneği gösterilmiştir.

Son olarak nasıl İcloud'a yedekleme eklenmesini uygulama tasarımınızı etkilemelidir hakkında kısa bir açıklama dahil.



## <a name="related-links"></a>İlgili bağlantılar

- [Giriş için iCloud (örnek)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [iCloud seminer örnek kod](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud seminer slayt](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud depolama](http://support.apple.com/kb/HT4847)
