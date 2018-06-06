---
title: İCloud Xamarin.iOS ile kullanma
description: Bu belge iCloud ve Xamarin.iOS uygulamaları kullanımını açıklar. Anahtar-değer depolama, belge depolama ve İcloud'a yedekleme açıklanır.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: 032d5f01ae63e5aececa14390300c28623c4f371
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785550"
---
# <a name="using-icloud-with-xamarinios"></a>İCloud Xamarin.iOS ile kullanma

İOS 5 iCloud depoda API uygulamalarının kullanıcı belgeleri ve uygulamaya özgü verileri merkezi bir konuma kaydedebilir ve kullanıcının tüm cihazlardan bu öğelere erişmesine olanak sağlar.

Dört depolama türleri kullanılabilir:

- **Anahtar-değer depolama** - üzerinde küçük miktarda veri uygulamanız ile paylaşmak için bir kullanıcının diğer aygıtlar.

- **UIDocument depolama** - kullanıcının iCloud hesabıyla UIDocument öğesinin bir alt kümesi kullanarak belgeleri ve diğer verileri depolamak için.

- **CoreData** -SQLite veritabanı depolama.

- **Tek tek dosyalar ve dizinler** - çok sayıda farklı dosya doğrudan dosya sistemini de yönetmek için.

Bu belge - anahtar-değer çiftleri ve UIDocument sınıfları - ilk iki türleri ve Xamarin.iOS içinde bu özellikleri kullanmak nasıl anlatılmaktadır.

> [!IMPORTANT]
> Apple [araçlar sağlar](https://developer.apple.com/support/allowing-users-to-manage-data/) Avrupa Birliği'nın genel veri koruma düzenleme (GDPR) düzgün bir şekilde işlemek geliştiricilere yardımcı olmak için.

## <a name="requirements"></a>Gereksinimler

- Xamarin.iOS en son kararlı sürümü
- Xcode 8 veya daha yeni
- Visual Studio Mac veya Visual Studio 2015 ve sonraki sürümleri için.

## <a name="preparing-for-icloud-development"></a>İCloud geliştirme için hazırlama

Uygulamaların iCloud hem de kullanacak şekilde yapılandırılması gerekir [Apple sağlama portalı](https://developer.apple.com/account/ios/overview.action) ve proje. İCloud için geliştirme (veya örnekleri çalışırken) aşağıdaki adımları izlemeden önce.

İCloud erişmek için bir uygulamayı doğru şekilde yapılandırmak için:

-   **Sistem kullanıcısı kimliği Bul** -oturum açma [developer.apple.com](http://developer.apple.com) ve ziyaret **üye merkezi > hesabınız > Geliştirici hesabı Özet** , takım kimliği (veya tek geliştiriciler için tek tek Kimliği almak için ). 10 karakter dizesi olacaktır ( **A93A5CM278** örneğin)-bu "kapsayıcı tanımlayıcının" bölümü oluşturur.

-   **Yeni bir uygulama kimliği oluşturma** - bir uygulama kimliği oluşturma, özetlenen adımları izleyin [cihaz sağlamayı Kılavuzu depolama teknolojileri bölümü için sağlama](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)ve kontrol ettiğinizden emin olun **iCloud** olarak bir izin verilen hizmeti:

 [![](introduction-to-icloud-images/icloud-sml.png "İCloud izin verilen bir hizmet olarak denetleyin")](introduction-to-icloud-images/icloud.png#lightbox)

- **Yeni bir sağlama profili oluşturun** - sağlama profili oluşturun, özetlenen adımları izleyin [cihaz sağlamayı Kılavuzu](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) .

- **Kapsayıcı tanımlayıcı için Entitlements.plist Ekle** -kapsayıcı tanımlayıcı biçimi `TeamID.BundleID`. Daha fazla bilgi için bkz [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

- **Proje özelliklerini yapılandırma** - dosya olduğundan emin olun Info.plist **paket tanımlayıcısı** eşleşen **paket kimliği** ne zaman ayarlamak [bir uygulama kimliği oluşturma ](~/ios/deploy-test/provisioning/capabilities/index.md); Paket imzalama iOS kullanan bir **sağlama profili** iCloud uygulama hizmeti ile bir uygulama kimliği içeren ve **özel yetkilendirmeler** dosyası seçili. Bu tüm Visual Studio Proje Özellikler bölmesi altında yapılabilir.

- **İCloud Cihazınızda etkinleştirmek** - gidin **ayarlar > iCloud** ve cihaz oturum emin olun.
Seçin ve Aç **belgeleri & veri** seçeneği.

- **İCloud test etmek için bir aygıt kullanmalıdır** -Simulator'da çalışmaz.
Aslında, gerçekten iCloud eylem görmek için aynı Apple Kimliğiyle oturum imzalı iki veya daha fazla aygıtlar gerekir.


## <a name="key-value-storage"></a>Anahtar-değer depolama

Anahtar-değer depolama küçük miktarda bir kullanıcı arasında kalıcı cihazları - kitap veya dergi görüntülenen son sayfa gibi gibi veri yöneliktir. Anahtar-değer depolama için yedekleme yukarı verileri kullanılmamalıdır.

Anahtar-değer depolama kullanırken dikkat edilmesi gereken bazı sınırlamalar vardır:

- **Maksimum anahtar boyutu** -anahtar adları 64 bayttan daha uzun olamaz.

- **En büyük değer boyutu** -tek bir değer birden fazla 64 KB depolanamıyor.

- **Bir uygulama için en fazla anahtar değeri deposu boyutu** -uygulamalar yalnızca olarak toplamda en fazla 64 KB anahtar-değer veri depolayabilir. Bu sınırı aşan anahtarlarını ayarlamak için denemeleri başarısız olur ve önceki değeri korunur.

- **Veri türleri** - yalnızca temel türleri gibi dizeler, sayılar ve Boole değerlerini depolanabilir.

**İCloudKeyValue** örneğinde, nasıl çalıştığı gösterilmektedir. Örnek kod, her cihaz için adlı bir anahtar oluşturur: bir cihazda bu anahtarı ayarlayın ve başkalarına yayılan değerini izleyin. Ayrıca, aynı anda birçok cihazlarda düzenlerseniz, "herhangi bir cihazda - düzenlenebilen paylaşılan" adlı bir anahtar oluşturur, iCloud "(bir zaman damgası değişiklik kullanarak) WINS" değeri ve yayılan karar verir.

Bu ekran örnek kullanımını göstermektedir. Değişiklik bildirimleri iCloud alındığında bunlar ekranın altındaki kaydırma metin görünümünde yazdırılan ve giriş alanları güncelleştirildi.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Cihazlar arasında ileti akışı")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Veri alma ve ayarlama

Bu kod bir dize değeri ayarlamak nasıl gösterir.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Eşitle çağırma değeri yalnızca yerel disk depolama için kalıcı sağlar. İcloud eşitleme arka planda gerçekleşir ve "uygulama kodu tarafından zorlanamaz". Ağ zayıf (ya da bağlantısı kesilmiş) ise bir güncelleştirmeyi daha uzun sürebilir ancak iyi bir ağ bağlantısına sahip eşitleme genellikle 5 saniye içinde gerçekleşir.

Bu kod bir değerle alabilirsiniz:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Değer yerel veri deposundan alınır - bu yöntem, "en son" değerini almak için iCloud sunucularla iletişim kurmayı değil. iCloud yerel veri deposundaki kendi zamanlamaya göre güncelleştirilir.

### <a name="deleting-data"></a>Verileri silme

Bir anahtar-değer çifti tamamen kaldırmak için bu gibi Kaldır yöntemi kullanın:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Değişiklikleri Gözlemleme

Değerleri bir gözlemci ekleyerek iCloud tarafından değiştirildiğinde uygulama bildirimleri de alabilirsiniz `NSNotificationCenter.DefaultCenter`.
Aşağıdaki kod **KeyValueViewController.cs** `ViewWillAppear` yöntemi için bu bildirimleri dinlemek ve hangisinin anahtarları değiştirilmiş listesini oluşturmak nasıl gösterir:

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

Kodunuzu bazı listesiyle bunları yerel bir kopyasını güncelleştirmek veya yeni değerlerle UI güncelleştirme gibi değişen anahtarların önlem alabilirsiniz.

Değişiklik olası nedenleri şunlardır: HtmlRadioButton (0), InitialSyncChange (1) veya QuotaViolationChange (2). Nedeni erişebilir ve gerekirse farklı işlem gerçekleştirme (örneğin, bazı anahtarları sonucu olarak kaldırmanız gerekebilir bir *QuotaViolationChange*).

## <a name="document-storage"></a>Belge depolama

İcloud'a belge depolama uygulamanıza (ve kullanıcıya) önemli olan verileri yönetmek için tasarlanmıştır. Dosyaları ve iCloud tabanlı Yedekleme sağlama ve kullanıcının tüm cihazlarda işlevselliği paylaşımı aynı anda sırada çalıştırmak için uygulamanız gereken diğer verileri yönetmek için kullanılabilir.

Bu diyagramda, nasıl tüm birlikte uyduğunu gösterilmektedir. Her cihazın yerel depolama (UbiquityContainer) ve arka plan programı veri bulutta gönderip ilgilenir işletim sisteminin iCloud kaydedilen verilerin gerekir. Tüm dosya erişimi UbiquityContainer FilePresenter/eşzamanlı erişimi engellemek için FileCoordinator yapılmalıdır. `UIDocument` Sınıfı bu sizin için gerçekleştirir; Bu örnek UIDocument kullanmayı gösterir.

 [![](introduction-to-icloud-images/icloud-overview.png "Belge depolama genel bakış")](introduction-to-icloud-images/icloud-overview.png#lightbox)

Basit bir iCloudUIDoc örnek uygulayan `UIDocument` tek bir metin alanı içeren bir alt kümesi. Metin olarak işlenir bir `UITextView` ve düzenlemeleri yayılır diğer cihazları İcloud'a tarafından bir bildirim iletisi kırmızı olarak gösterilir. Örnek kod çakışma çözümü gibi daha gelişmiş iCloud özellikleri ile ilgilenir değil.

Bu ekran metni değiştirme ve tuşuna basarak sonra örnek uygulaması - gösterir **UpdateChangeCount** belge diğer cihazları İcloud'a yoluyla eşitlenir.

 [![](introduction-to-icloud-images/iclouduidoc.png "Bu ekran metni değiştirme ve UpdateChangeCount tuşuna basarak sonra örnek uygulaması gösterir")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

İCloudUIDoc örnek beş bölümü vardır:

1. **UbiquityContainer erişme** -iCloud etkin olup olmadığını ve bu durumda belirlemek, uygulamanızın iCloud depolama alanı yolu.

1. **UIDocument alt sınıf oluşturma** -iCloud depolama model nesneleri arasındaki ara için bir sınıf oluşturun.

1. **Bulma ve açma iCloud belgeleri** -kullanmak `NSFileManager` ve `NSPredicate` iCloud belgeleri bulmak ve bunları açmak için.

1. **İcloud'a belge görüntüleme** -özelliklerinden kullanıma, `UIDocument` böylece UI denetimleri ile etkileşim kurabilirsiniz.

1. **İCloud belgeleri kaydetme** -kullanıcı Arabiriminde yapılan değişiklikleri disk ve iCloud kalıcı emin olun.

Tüm iCloud işlemleri çalıştırma (veya çalışması gerektiğini) böylece bunlar yapacağı olmasını beklerken zaman uyumsuz olarak engelleme. Bu örnekte gerçekleştirmeye üç farklı yolla görürsünüz:

 **İş parçacığı** - `AppDelegate.FinishedLaunching` ilk çağrısı `GetUrlForUbiquityContainer` ana iş parçacığı engellenmesini önlemek için başka bir iş parçacığı üzerinde gerçekleştirilir.

 **NotificationCenter** - zaman uyumsuz olduğunda bildirimleri için kaydediliyor gibi işlemleri `NSMetadataQuery.StartQuery` tamamlandı.

 **Tamamlama işleyicileri** - yöntemler gibi zaman uyumsuz işlemleri tamamlama çalıştırmak için geçen `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>UbiquityContainer erişme

İcloud'a belge depolama kullanmanın ilk adımı iCloud etkin olup olmadığını ve bu durumda belirlemektir "yaygınlığıyla kapsayıcısının" konumu (cihazda iCloud etkin dosyaların depolandığı dizin).

Bu kod `AppDelegate.FinishedLaunching` örnek yöntemi.

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

Örnek Bunu yapmak karşın, bir uygulama için ön gelen her GetUrlForUbiquityContainer çağırma Apple önerir.

### <a name="creating-a-uidocument-subclass"></a>UIDocument alt sınıf oluşturma

Tüm iCloud dosyaları ve dizinleri (IE. herhangi bir şey UbiquityContainer dizinde saklanan) NSFilePresenter protokolünü uygulamaya yönelik ve bir NSFileCoordinator yazma NSFileManager yöntemlerini kullanarak yönetiliyor olması gerekir.
En basit yolu, tümünü bunu kendiniz ancak bunu yapan UIDocument bir alt kümesi değil yazmaktır tüm için.

İCloud ile çalışmak için bir UIDocument alt uygulamalıdır yalnızca iki yöntemi vardır:

- **LoadFromContents** -, listelenen model sınıfı/es açmak, dosyanın içeriğini NSData geçirir.

- **ContentsForType** -disk (ve bulut) kaydetmek için model sınıfı/es NSData gösterimini sağlamak istek.

Bu örnek koddan **iCloudUIDoc\MonkeyDocument.cs** nasıl UIDocument uygulandığını gösterir.

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

Veri modeli Bu örnekte çok basit - tek bir metin alanı. Veri modelinizi gerektiği bir Xml belgesi veya ikili veriler gibi karmaşık olabilir. UIDocument uygulama birincil rolü, model sınıflarınızı ve kaydedilen/yüklendi diskte bir NSData gösterimi arasında ifadelere çevrilmesidir.

### <a name="finding-and-opening-icloud-documents"></a>Bulma ve iCloud belgeleri açma

Örnek uygulamayı yalnızca tek bir dosyayı - sınama.txt - ilgilenir böylece kodu **AppDelegate.cs** oluşturur bir `NSPredicate` ve `NSMetadataQuery` özellikle bu dosya için aranacak. `NSMetadataQuery` Zaman uyumsuz olarak çalışır ve tamamlandığında bir bildirim gönderir. `DidFinishGathering` bildirim gözlemci tarafından çağrılır, sorgu durdurur ve kullandığı LoadDocument çağırır `UIDocument.Open` dosya yükleme ve içinde görüntüleme girişiminde tamamlama işleyici yöntemiyle bir `MonkeyDocumentViewController`.

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

Bir UIDocument görüntüleme için başka bir model sınıfı fark olmamalıdır
- Özellikler kullanıcı Arabirimi denetimlerini, büyük olasılıkla kullanıcı tarafından düzenlenmiş ve modele geri yazılan görüntülenir.

Örnekte **iCloudUIDoc\MonkeyDocumentViewController.cs** MonkeyDocument metin olarak görüntüler bir `UITextView`. `ViewDidLoad` gönderilen bildirim dinler `MonkeyDocument.LoadFromContents` yöntemi. `LoadFromContents` iCloud dosyanın yeni verileri olduğunda bildirim belgenin güncelleştirildiğini gösterir böylece denir.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Örnek kod bildirim işleyicisi UI - bu durumda herhangi bir çakışma algılaması veya çözümleme güncelleştirmek için bir yöntem çağırır.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>Kaydetme iCloud belgeleri

Bir UIDocument çağırabilirsiniz İcloud'a eklemek için `UIDocument.Save` doğrudan (yeni belgeler için yalnızca) veya kullanarak varolan bir dosya taşıma `NSFileManager.DefaultManager.SetUbiquitious`. Kod örneği, doğrudan bu kodla yaygınlığıyla kapsayıcıda yeni bir belge oluşturur (iki tamamlama işleyicisi vardır burada için bir tane `Save` işlemi ve açık için başka bir):

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

Sonraki değişiklikler belgeye "kaydedilmez" doğrudan, bunun yerine biz söyleyin `UIDocument` ile değişen `UpdateChangeCount`, ve otomatik olarak kaydetme disk işlemi için zamanlama:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>İCloud belgeleri yönetme

Kullanıcıların iCloud belgelerde yönetebilir **belgeleri** ayarları; aracılığıyla uygulamanızı dışında "yaygınlığıyla kapsayıcısının" dizin silmek için geçirme ve dosya listesini görüntüleyebilirsiniz. Uygulama kodu belgeler kullanıcı tarafından nerede silinir durum kaldırabilir olmalıdır. İç uygulama verileri depolamaz **belgeleri** dizin.

 [![](introduction-to-icloud-images/icloudstorage.png "İcloud'a belge iş akışını yönetme")](introduction-to-icloud-images/icloudstorage.png#lightbox)



Bu uygulamayla ilgili iCloud belgelerinin durumunu bildiren cihazlarını iCloud etkinleştirilmiş bir uygulama kaldırmayı denediğinizde kullanıcılar ayrıca farklı uyarılar alırsınız.

 [![](introduction-to-icloud-images/icloud-delete1.png "Kullanıcının iCloud etkinleştirilmiş bir uygulama kullanıcıların cihazlarından kaldırma girişiminde bulunduğunda örnek iletişim")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Kullanıcının iCloud etkinleştirilmiş bir uygulama kullanıcıların cihazlarından kaldırma girişiminde bulunduğunda örnek iletişim")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>İcloud'a yedekleme

İcloud'a yedekleme doğrudan geliştiriciler tarafından erişilen özelliği olmasa da, uygulamanızın tasarlarken kullanıcı deneyimini etkileyebilir.
Apple sağlar [iOS veri depolama yönergeleri](http://developer.apple.com/icloud/documentation/data-storage/) geliştiricilerin kendi iOS uygulamalarını izleyin.

En önemli konu olup uygulamanızı, kullanıcı tarafından oluşturulan (örneğin, içeriği sorunu başına hundred-plus megabayt depolar dergi okuyucu uygulama) olmayan büyük dosyaların depolandığı yerdir. Apple bu tür, İcloud'a yedeklenen ve gereksiz yere kullanıcının iCloud kota doldurun veri depolamayın tercih eder.

Büyük miktarlarda veri şöyle depolamak uygulamaları ya da onu yedeklenen (ör. değil kullanıcı dizinleri birinde depolamanız gerekir Önbellekleri veya tmp) veya `NSFileManager.SetSkipBackupAttribute` İcloud'a yedekleme işlemleri sırasında saymasıdır böylece bu dosyaları bir bayrak uygulanacak.

## <a name="summary"></a>Özet

Bu makalede iOS 5 dahil yeni iCloud özellik sunulmuştur. Projenizi iCloud kullanacak şekilde yapılandırmak için gereken ve örnekleri iCloud özellikleri uygulamak sağlanan adımları incelendi.

Anahtar-değer depolama örneğine benzer şekilde NSUserPreferences depolanan verilerin kısa süreli depolamak için iCloud'ın nasıl kullanılabileceğini gösterilmektedir. Nasıl daha fazla karmaşık veri depolanır ve birden çok cihazı iCloud aracılığıyla arasında eşitlenen UIDocument örnek gösterilmiştir.

Son olarak nasıl İcloud'a yedekleme eklenmesi uygulama tasarımınızı etkilemelidir hakkında kısa bir tartışma dahil.



## <a name="related-links"></a>İlgili bağlantılar

- [Giriş için iCloud (örnek)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [iCloud Semineri örnek kod](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud Semineri slayt](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud depolama](http://support.apple.com/kb/HT4847)
