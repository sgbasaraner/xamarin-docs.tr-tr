---
title: SiriKit uygulama
description: "Bu makalede bir Xamarin.iOS uygulamaları SiriKit destek uygulamak için gerekli adımlar kapsanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a891e5bf797742ceb1bb45bb8144fa77dec99b2c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="implementing-sirikit"></a>SiriKit uygulama

_Bu makalede bir Xamarin.iOS uygulamaları SiriKit destek uygulamak için gerekli adımlar kapsanmaktadır._



Yeni iOS 10, SiriKit Siri ve haritalar uygulama iOS cihazında kullanarak kullanıcı tarafından erişilebilir hizmetleri sağlamak bir Xamarin.iOS uygulaması sağlar. Bu makalede gerekli hedefleri uzantılar, hedefleri UI uzantıları ve sözlük ekleyerek Xamarin.iOS uygulamalar SiriKit destek uygulamak için gerekli adımlar kapsanmaktadır.

Siri çalışır kavramıyla **etki alanları**, grupları bilmeniz ilgili görevleri için Eylemler. Uygulama ile Siri sahip her etkileşim, bilinen bir hizmet etki alanlarından biri aşağıdaki gibi olmalıdır:

- Ses veya video çağırma.
- Bir kılma kayıt.
- Egzersizleriniz yönetme.
- İleti.
- Fotoğraf aranıyor.
- Gönderme veya ödemeler alma.

Kullanıcı, uygulama uzantının hizmetlerinden birini içeren Siri isteği yaptığında, SiriKit uzantısı gönderir bir **hedefi** kullanıcının isteği destekleyici verilerin tanımlayan nesne. Uygulama Uzantısı sonra uygun oluşturur **yanıt** için nesne verilen **hedefi**, uzantısı isteği nasıl işleyebilir ayrıntılı.

Bu kılavuz, varolan bir uygulamaya SiriKit desteği dahil olmak üzere hızlı bir örnek sunacaktır. Bu örnek amacıyla, biz sahte MonkeyChat uygulamasını kullanarak:

[ ![](implementing-sirikit-images/monkeychat01.png "MonkeyChat simgesi")](implementing-sirikit-images/monkeychat01.png)

Kullanıcının arkadaş kişi kendi rehberini MonkeyChat tutar, her bir ekran adı (ör, örneğin Bobo) ile ilişkili ve metin sohbetleri ekran adına göre her arkadaşınıza gönderin olanak tanır.

## <a name="extending-the-app-with-sirikit"></a>SiriKit uygulamayla genişletme

Gösterildiği gibi [anlama SiriKit kavramları](~/ios/platform/sirikit/understanding-sirikit.md) Kılavuzu, bir uygulamayla SiriKit genişletme ile ilgili üç ana bölümü vardır:

[ ![](implementing-sirikit-images/elements01.png "SiriKit diyagramı uygulamayla genişletme")](implementing-sirikit-images/elements01.png)

Bu güncelleştirmeler şunlardır:

1. **Hedefleri uzantısı** -kullanıcıların yanıtları doğrular, uygulama istek işleyebilir ve gerçekte kullanıcının isteği yerine getirmek için bir görev gerçekleştiren onaylar.
2. **Hedefleri UI uzantısı** - *isteğe bağlı*, yanıtları Siri ortamında özel bir kullanıcı Arabirimi sağlar ve kullanıcı Arabirimi uygulamaları kullanıma sunabilirsiniz ve kullanıcı deneyimini zenginleştirmek için Siri markalama.
3. **Uygulama** -uygulama ile çalışma Siri yardımcı olmak için kullanılan kullanıcı belirli sözcük dağarcıklarını sağlar. 

Tüm uygulama eklemek için adımları ve bu öğeleri aşağıdaki bölümlerde ayrıntılı olarak ele alınacaktır.


## <a name="preparing-the-app"></a>Uygulama hazırlama

SiriKit uzantılarını yerleşik olarak bulunur, ancak, uygulama için tüm uzantılar eklemeden önce vardır Geliştirici SiriKit benimsenmesi yardımcı olmak için yapması gereken birkaç şey.

### <a name="moving-common-shared-code"></a>Ortak paylaşılan kod taşıma 

İlk olarak, geliştirici paylaşılacak ortak kodun bazı uygulama ve uzantıları arasında ya da taşıyabilirsiniz _paylaşılan projeleri_, _taşınabilir sınıf kitaplıkları (PCLs)_ veya _yerel Kitaplıkları_.

Uzantılar, tüm uygulama mu şeyleri yapabilmek gerekir. Örnek MonkeyChat uygulama, kişileri, yeni kişiler ekleme bulma gibi şeyler koşullarını iletilerini göndermek ve ileti geçmişi alın.

Bu ortak kodun bir paylaşılan projesi, PCL veya yerel kitaplığa taşıyarak, ortak bir yerde kodda bakımını kolay hale getirir ve Tekdüzen deneyimleri ve işlevsellik uzantısı ve üst uygulama kullanıcıdan sağlamak sağlar.

Örnek uygulama MonkeyChat söz konusu olduğunda veri modelleri ve ağ ve veritabanı erişim gibi işlem kodu yerel bir kitaplık içine taşınır.

Aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'yu başlatın ve MonkeyChat uygulamasını açın.
2. Çözüm adına sağ tıklayın **çözüm paneli** seçip **Ekle** > **yeni proje...** : 

    [ ![](implementing-sirikit-images/prep01.png "Yeni bir proje ekleyin")](implementing-sirikit-images/prep01.png)
3. Seçin **iOS** > **Kitaplığı** > **sınıf kitaplığı** tıklatıp **sonraki** düğmesi: 

    [ ![](implementing-sirikit-images/prep02.png "Sınıf kitaplığı seçin")](implementing-sirikit-images/prep02.png)
4. Girin `MonkeyChatCommon` için **adı** tıklatıp **oluşturma** düğmesi: 

    [ ![](implementing-sirikit-images/prep03.png "MonkeyChatCommon için bir ad girin")](implementing-sirikit-images/prep03.png)
5. Sağ tıklayın **başvuruları** ana uygulamada klasörünü **Çözüm Gezgini** seçip **başvuruları Düzenle...** . Denetleme **MonkeyChatCommon** proje ve tıklatın **Tamam** düğmesi: 

    [ ![](implementing-sirikit-images/prep05.png "Onay MonkeyChatCommon proje")](implementing-sirikit-images/prep05.png)
6. İçinde **Çözüm Gezgini**, ortak paylaşılan kod ana uygulamadan yerel kitaplığına sürükleyin.
7. MonkeyChat söz konusu olduğunda, sürükleyin **DataModels** ve **işlemciler** ana uygulama yerel kitaplığa klasörlerinden: 

    [ ![](implementing-sirikit-images/prep06.png "Çözüm Gezgini'nde DataModels ve işlemci klasörleri")](implementing-sirikit-images/prep06.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio'yu başlatın ve MonkeyChat uygulamasını açın.
2. Çözüm adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle** > **yeni proje...** .
3. Seçin **Visual C#** > **paylaşılan proje** tıklatıp **sonraki** düğmesi: 

    [ ![](implementing-sirikit-images/prep02w.png "Sınıf kitaplığı seçin")](implementing-sirikit-images/prep02w.png)
4. Girin `MonkeyChatCommon` için **adı** tıklatıp **oluşturma** düğmesi.
5. Sağ tıklayın **başvuruları** ana uygulamada klasörünü **Çözüm Gezgini** seçip **başvuruları Düzenle...** . Denetleme **MonkeyChatCommon** proje ve tıklatın **Tamam** düğmesi: 

    [ ![](implementing-sirikit-images/prep05w.png "Onay MonkeyChatCommon proje")](implementing-sirikit-images/prep05w.png)
6. İçinde **Çözüm Gezgini**, ortak paylaşılan kodu paylaşılan bir proje için ana uygulama sürükleyin.
7. MonkeyChat söz konusu olduğunda, sürükleyin **DataModels** ve **işlemciler** ana uygulama yerel kitaplığa klasörlerinden.

-----

Yerel kitaplığa taşındı dosyalardan birini düzenleyin ve Kitaplığı'nın adıyla eşleşmesi için ad alanı değiştirin. Örneğin, değiştirme `MonkeyChat` için `MonkeyChatCommon`:

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

Ardından, ana uygulamaya geri dönün ve ekleme bir `using` deyimi yerel kitaplığın ad alanı için herhangi bir yere uygulamanın kullandığı taşınmış sınıflarından biri:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>Uygulama için Uzantılar mimarisi oluşturma

Genellikle, bir uygulama için birden çok hedefleri imzalar ve uygulama amacı uzantıları uygun sayısı için geliştirilmiştir emin olmak Geliştirici gerekiyor.

Burada bir uygulama birden çok hedefi gerektirir durumda Geliştirici hedefi işleme tümünün tek amacı uzantısı'nda yerleştirme veya ayrı bir hedefi uzantı her hedefini oluşturma seçeneği vardır.

Her amaç için ayrı bir hedefi uzantı oluşturmayı seçerek, geliştirici Demirbaş kod her uzantısı'nda, büyük bir miktarını çoğaltma yukarı sonlandırmak ve işlemci ve bellek ek yüküne büyük miktarda oluşturun.

İki seçenek arasından seçim yardımcı olmak için herhangi bir amacı doğal olarak birlikte ait olmadığını bakın. Örneğin, ses ve video çağrılarını yapılan bir uygulama benzer görevleri işleme gibi bu hedefleri her ikisi de tek bir hedefi uzantısına dahil etme isteyebilirsiniz ve çoğu kodu yeniden kullanma sağlayabilir.

Herhangi bir amaç veya varolan bir gruba uymayan hedefleri grubunu için uygulamanın çözümde bunları içeren yeni bir hedefi uzantısı oluşturun.


### <a name="setting-the-required-entitlements"></a>Gerekli yetkilendirmeleri ayarlama

SiriKit tümleştirme içeren herhangi bir Xamarin.iOS uygulaması doğru yetkilendirmeler ayarlamanız gerekir. Geliştirici bu gerekli yetkilendirmeler doğru ayarlı değil, yüklemek veya donanımda 10 iOS gereksinimi olan gerçek iOS 10 (veya daha fazla), uygulamayı test etme mümkün olmaz Simulator SiriKit desteklemiyor.

Aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Entitlements.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. Geçiş **kaynak** sekmesi.
3. Ekleme `com.apple.developer.siri` **özelliği**ayarlayın **türü** için `Boolean` ve **değeri** için `Yes`: 

    [ ![](implementing-sirikit-images/setup01.png "Com.apple.developer.siri özellik ekleme")](implementing-sirikit-images/setup01.png)
4. Değişiklikleri dosyaya kaydedin.
5. Çift **proje dosyası** içinde **Çözüm Gezgini** düzenlemek için açın.
6. Seçin **iOS paket imzalama** ve emin `Entitlements.plist` dosya seçildiğinde, **özel yetkilendirmeler** alan: 

    [ ![](implementing-sirikit-images/setup02.png "Özel yetkilendirmeler alanında Entitlements.plist dosyası seçin")](implementing-sirikit-images/setup02.png)
7. Tıklatın **Tamam** değişiklikleri kaydetmek için düğmesi.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çift `Entitlements.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
3. Ekleme `com.apple.developer.siri` **özelliği**ayarlayın **türü** için `Boolean` ve **değeri** için `Yes`: 

    [ ![](implementing-sirikit-images/setup01w.png "Com.apple.developer.siri özellik ekleme")](implementing-sirikit-images/setup01w.png)
4. Değişiklikleri dosyaya kaydedin.
5. Çift **proje dosyası** içinde **Çözüm Gezgini** düzenlemek için açın.
6. Seçin **iOS paket imzalama** ve emin `Entitlements.plist` dosya seçildiğinde, **özel yetkilendirmeler** alan.

-----

Tamamlandığında, uygulamanın `Entitlements.plist` dosya, aşağıdaki gibi görünmelidir (harici bir düzenleyicide açılır):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>Doğru uygulama sağlama

Apple SiriKit framework SiriKit uygulayan herhangi bir Xamarin.iOS uygulaması yerleştirdiğini katı güvenlik nedeniyle _gerekir_ doğru uygulama kimliği ve yetkilendirmeler (yukarıdaki bölümüne bakın) ve uygun ile imzalanmalıdır Sağlama profili.

Mac aşağıdakileri yapın:

1. Bir web tarayıcısında gidin [http://developer.apple.com](http://developer.apple.com) ve hesabınızda oturum.
2. Tıklayın **sertifikaları**, **tanımlayıcıları** ve **profilleri**.
3. Seçin **sağlama profilleri** seçip **uygulama kimlikleri**, ardından  **+**  düğmesi.
4. Girin bir **adı** yeni profil için.
5. Girin bir **paket kimliği** Apple aşağıdaki öneri adlandırma kullanıcının.
6. Ekranı aşağı kaydırarak **uygulama hizmetleri** bölümünde, select **SiriKit** tıklatıp **devam** düğmesi: 

    [ ![](implementing-sirikit-images/setup03.png "SiriKit seçin")](implementing-sirikit-images/setup03.png)
7. Tüm ayarlar ardından doğrulayın **gönderme** uygulama kimliği.
8. Seçin **sağlama profilleri** > **geliştirme**, tıklatın  **+**  düğmesi, select **Apple kimliği**, ardından **devam**.
9. Seç'i tıklatın **tüm**, ardından **devam**.
10. Tıklatın **Tümünü Seç** yeniden, ardından **devam**.
11. Girin bir **profil adı** Apple kullanarak önerileri adlandırma kullanıcının, ardından **devam**.
12. Xcode başlatın.
13. Xcode menüsünden seçin **tercihleri...**
14. Seçin **hesapları**, ardından **ayrıntıları görüntüle...** düğmesi: 

    [ ![](implementing-sirikit-images/setup04.png "Hesabı seçin.")](implementing-sirikit-images/setup04.png)
15. Tıklatın **karşıdan tüm profiller** sol alt köşesindeki düğmesi: 

    [ ![](implementing-sirikit-images/setup05.png "Tüm profillerin indirin")](implementing-sirikit-images/setup05.png)
16. Emin **sağlama profili** oluşturulan Xcode'da yukarıdaki yükledi.
17. Mac için Visual Studio'da SiriKit desteği eklemek için projeyi açın
18. Çift `Info.plist` dosyasını **Çözüm Gezgini**.
18. Emin **paket tanımlayıcı** yukarıdaki Apple Developer portalında oluşturulan bir eşleşen: 

    [ ![](implementing-sirikit-images/setup06.png "Paket tanımlayıcısı")](implementing-sirikit-images/setup06.png)
18. İçinde **Çözüm Gezgini**seçin **proje**.
19. Projeye sağ tıklayın ve seçin **seçenekleri**.
21. Seçin **iOS paket imzalama**seçin **imzalama kimlik** ve **sağlama profili** yukarıda oluşturduğunuz: 

    [ ![](implementing-sirikit-images/setup07.png "İmzalama kimlik ve sağlama profili seçin")](implementing-sirikit-images/setup07.png)
22. Tıklatın **Tamam** değişiklikleri kaydetmek için düğmesi.

> [!IMPORTANT]
> **Not:** test SiriKit 10 donanım aygıtı yalnızca gerçek bir iOS üzerinde çalışır ve 10 iOS simülatörü. Bir SiriKit yükleme konusunda sorun Xamarin.iOS uygulaması gerçek donanımda etkinleştirilirse, gerekli yetkilendirmeler, uygulama kimliği, imzalama tanımlayıcısı ve sağlama profili doğru Apple Geliştirici Portalı hem Visual Studio içinde Mac için yapılandırıldığından emin olun

### <a name="requesting-siri-authorization"></a>Siri yetkilendirme isteme

Uygulama herhangi bir kullanıcı özel sözlük ekler veya hedefleri uzantıları Siri bağlayan önce gerekir istediği yetkilendirme Siri erişmek için kullanıcıdan.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Uygulamanın Düzenle `Info.plist` dosya, geçiş **kaynak** görüntülemek ve eklemek `NSSiriUsageDescription` anahtar Siri ve hangi uygulamanın nasıl kullanacağını açıklayan bir dize değeri ile veri türleri gönderilir. Örneğin, "MonkeyChat kişiler Siri gönderilecek" MonkeyChat uygulama diyebilirsiniz:

[ ![](implementing-sirikit-images/request01.png "Info.plist düzenleyicisinde NSSiriUsageDescription")](implementing-sirikit-images/request01.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Uygulamanın Düzenle `Info.plist` dosya ve ekleme `NSSiriUsageDescription` anahtar Siri ve hangi uygulamanın nasıl kullanacağını açıklayan bir dize değeri ile veri türleri gönderilir. Örneğin, "MonkeyChat kişiler Siri gönderilecek" MonkeyChat uygulama diyebilirsiniz:

[ ![](implementing-sirikit-images/request01w.png "Info.plist düzenleyicisinde NSSiriUsageDescription")](implementing-sirikit-images/request01w.png)

-----

Çağrı `RequestSiriAuthorization` yöntemi `INPreferences` uygulama ilk kez başlatıldığında sınıfı. Düzen `AppDelegate.cs` sınıfı ve olun `FinishedLaunching` yöntemi görünüm aşağıdaki gibi:


```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });


    return true;
}
```

Bu yöntem çağrılır, ilk kez Siri erişmek uygulama izin vermek için kullanıcıdan bir uyarı görüntülenir. Geliştirici eklenen iletinin `NSSiriUsageDescription` yukarıdaki bu uyarı görüntülenir. Kullanıcı başlangıçta erişimini engellediği, kullanabileceklerini **ayarları** uygulama erişim vermek için uygulama.

Herhangi bir zamanda uygulama uygulamanın çağırarak Siri erişme olanağını kontrol edebilirsiniz `SiriAuthorizationStatus` yöntemi `INPreferences` sınıfı.

### <a name="localization-and-siri"></a>Yerelleştirme ve Siri

Kullanıcı bir iOS cihazında farklı Siri sonra sistem varsayılan için bir dil seçmek mümkün değil. Yerelleştirilmiş verilerle çalışırken, uygulamayı kullanmanız gerekecektir `SiriLanguageCode` yöntemi `INPreferences` Siri dil kodu almak için sınıf. Örneğin:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>Kullanıcı belirli sözlük ekleme

Sözcükleri veya uygulama bireysel kullanıcılara benzersiz tümceleri sağlamak için kullanıcı belirli sözlük geçiyor. Bu ana uygulamadaki (uygulama uzantıları değil) çalışma zamanında koşulları, listenin başındaki en önemli terimler sahip kullanıcılar için en önemli kullanım önceliği sıralı sıralı bir dizi olarak sağlanacaktır.

Kullanıcı belirli sözlük Aşağıdaki kategorilerde birine ait olması gerekir:

- (Kişiler çerçevesi tarafından yönetilmeyen) adları başvurun.
- Fotoğraf etiketler.
- Fotoğraf albümü adları.
- Etkinlik adları.

Özel sözlük kaydetmek için terminolojisi seçerken, yalnızca Koşulları'nı seçin, yanlış uygulamayla değil tanıdık bir kişi tarafından. Hiç kayıt genel koşulları "My etkinlik" veya "My albümü" gibi. Örneğin, MonkeyChat uygulama kullanıcının adres defterinde her kişiyle ilişkili takma adlar kaydeder.

Uygulama çağırarak kullanıcı belirli sözlüğünü sağlar `SetVocabularyStrings` yöntemi `INVocabulary` sınıfı ve tümleştirilmesidir bir `NSOrderedSet` ana uygulama koleksiyonundan. Uygulama her zaman çağırmalıdır `RemoveAllVocabularyStrings` yöntemi yeni bir tane eklemeden önce mevcut tüm koşullar kaldırmak için ilk. Örneğin:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

Yerinde bu kodu ile şu şekilde adlandırılmış:

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> **Not:** Siri özel sözlük ipuçları değerlendirir ve terminolojiyi mümkün olduğunca çok dahil. Ancak, özel sözlük kaydetmek önemli hale getirme sınırlıdır boşluk _yalnızca_ kafa, bu nedenle kayıtlı koşulları toplam sayısı en düşük tutma olabilir terminolojisi.

Daha fazla bilgi için lütfen bkz bizim [kullanıcı belirli sözlük başvurusu](~/ios/platform/sirikit/understanding-sirikit.md) ve Apple'nın [belirtme özel sözlük başvuru](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

### <a name="adding-app-specific-vocabulary"></a>Uygulama özel sözlük ekleme

Uygulama özel sözlük tüm uygulamanın kullanıcılar, araç türleri veya Etkinlik adları gibi belirli sözcük ve bilinecek deyimleri tanımlar. Bu uygulamanın bir parçası olduğundan, bunlar içinde tanımlanan bir `AppIntentVocabulary.plist` dosyası ana uygulama paketi bir parçası olarak. Ayrıca, bu sözcükleri ve tümcecikleri yerelleştirilmiş.

Uygulama özel sözlük terimleri ve aşağıdaki kategorilerin birine ait olması gerekir:

- Seçenekler ride.
- Etkinlik adları.

Uygulama özel sözlük dosyası iki kök düzeyinde anahtarlarını içerir:

- `ParameterVocabularies` **Gerekli** -uygulamanın özel hüküm ve amacı uygulamak için parametreleri tanımlar.
- `IntentPhrases` **İsteğe bağlı** -tanımlı özel terimleri kullanarak örnek tümceleri içeren `ParameterVocabularies`.

Her giriş `ParameterVocabularies` bir kimlik dizesi, terim ve terimi uygulandığı amaca belirtmeniz gerekir. Ayrıca, tek bir terim için birden çok hedefleri uygulanabilir.

Apple'nın kabul edilebilir değerler ve gerekli dosya yapısı tam listesi için lütfen bkz [uygulama sözlük dosya biçimi başvurusu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

Eklemek için bir `AppIntentVocabulary.plist` dosya uygulama projesi için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Proje adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle** > **yeni dosya...**   >  **iOS**:

    [ ![](implementing-sirikit-images/plist01.png "Bir özellik listesi ekleme")](implementing-sirikit-images/plist01.png) 
2. Çift `AppIntentVocabulary.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
3. Tıklatın  **+**  anahtar eklemek için ayarlanmış **adı** için `ParameterVocabularies` ve **türü** için `Array`:

    [ ![](implementing-sirikit-images/plist02.png "ParameterVocabularies ve dizi türü adı ayarlayın")](implementing-sirikit-images/plist02.png)
4. Genişletme `ParameterVocabularies` tıklatıp  **+**  düğmesine tıklayın ve ayarlama **türü** için `Dictionary`:

    [ ![](implementing-sirikit-images/plist03.png "Sözlüğe türünü ayarlayın")](implementing-sirikit-images/plist03.png)
5. Tıklatın  **+**  yeni bir anahtar eklemek için ayarlayın **adı** için `ParameterNames` ve **türü** için `Array`:

    [ ![](implementing-sirikit-images/plist04.png "ParameterNames ve dizi türü adı ayarlayın")](implementing-sirikit-images/plist04.png)
6. Tıklatın  **+**  ile yeni bir anahtar eklemek için **türü** , `String` ve değeri olarak kullanılabilir parametre adlarından biri. Örneğin, `INStartWorkoutIntent.workoutName`:

    [ ![](implementing-sirikit-images/plist05.png "The INStartWorkoutIntent.workoutName key")](implementing-sirikit-images/plist05.png)
7. Ekleme `ParameterVocabulary` anahtarını `ParameterVocabularies` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist06.png "Bu tür dizi ParameterVocabularies anahtarla ParameterVocabulary anahtarını ekleyin")](implementing-sirikit-images/plist06.png)
8. Yeni bir anahtarla eklemek **türü** , `Dictionary`:

    [ ![](implementing-sirikit-images/plist07.png "Bu tür sözlüğü ile yeni bir anahtar ekleyin")](implementing-sirikit-images/plist07.png)
9. Ekleme `VocabularyItemIdentifier` ile anahtar **türü** , `String` ve dönem için benzersiz bir kimlik belirtin:

    [ ![](implementing-sirikit-images/plist08.png "Dize türünde VocabularyItemIdentifier anahtarla ekleyin ve benzersiz bir kimlik belirtin")](implementing-sirikit-images/plist08.png)
10. Ekleme `VocabularyItemSynonyms` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist09.png "Bu tür dizi VocabularyItemSynonyms anahtarla ekleme")](implementing-sirikit-images/plist09.png)
11. Yeni bir anahtarla eklemek **türü** , `Dictionary`:

    [ ![](implementing-sirikit-images/plist10.png "Bu tür sözlüğü ile yeni bir anahtar ekleyin")](implementing-sirikit-images/plist10.png)
12. Ekleme `VocabularyItemPhrase` ile anahtar **türü** , `String` ve olduğunu uygulamayı terimi tanımlama:

    [ ![](implementing-sirikit-images/plist11.png "Dize türünde ve uygulama olduğunu tanımlama terim VocabularyItemPhrase anahtarla ekleme")](implementing-sirikit-images/plist11.png)
13. Ekleme `VocabularyItemPronunciation` ile anahtar **türü** , `String` ve ses telaffuz koşulunun:

    [ ![](implementing-sirikit-images/plist12.png "Dize türünde ve ses telaffuz koşulunun VocabularyItemPronunciation anahtarla ekleme")](implementing-sirikit-images/plist12.png)
14. Ekleme `VocabularyItemExamples` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist13.png "Bu tür dizi VocabularyItemExamples anahtarla ekleme")](implementing-sirikit-images/plist13.png)
15. Birkaç eklemek `String` terimi örnek kullanımları anahtarlarla:

    [ ![](implementing-sirikit-images/plist14.png "Terim örnek kullanımları birkaç dize anahtarlarıyla ekleme")](implementing-sirikit-images/plist14.png)
16. Uygulamanın gereksinim tanımlamak için tüm diğer özel terimler için yukarıdaki adımları yineleyin.
17. Daraltma `ParameterVocabularies` anahtarı.
18. Ekleme `IntentPhrases` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist15.png "Bu tür dizi IntentPhrases anahtarla ekleme")](implementing-sirikit-images/plist15.png)
19. Yeni bir anahtarla eklemek **türü** , `Dictionary`:

    [ ![](implementing-sirikit-images/plist16.png "Bu tür sözlüğü ile yeni bir anahtar ekleyin")](implementing-sirikit-images/plist16.png)
20. Ekleme `IntentName` ile anahtar **türü** , `String` ve hedefi örneğin:

    [ ![](implementing-sirikit-images/plist17.png "Örneğin dize türünde ve amacı ile IntentName anahtarı Ekle")](implementing-sirikit-images/plist17.png)
21. Ekleme `IntentExamples` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist18.png "Bu tür dizi IntentExamples anahtarla ekleme")](implementing-sirikit-images/plist18.png)
22. Birkaç eklemek `String` terimi örnek kullanımları anahtarlarla:

    [ ![](implementing-sirikit-images/plist19.png "Terim örnek kullanımları birkaç dize anahtarlarıyla ekleme")](implementing-sirikit-images/plist19.png)
23. Uygulama kullanım örneği sağlamanız gereken tüm hedefleri için yukarıdaki adımları yineleyin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Proje adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle** > **yeni dosya...**   >  **iOS**:

    [ ![](implementing-sirikit-images/plist01w.png "Yeni Info.plist ekleme")](implementing-sirikit-images/plist01w.png) 
2. Çift `AppIntentVocabulary.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
3. Tıklatın  **+**  anahtar eklemek için ayarlanmış **adı** için `ParameterVocabularies` ve **türü** için `Array`:

    [ ![](implementing-sirikit-images/plist02w.png "ParameterVocabularies ve dizi türü adı ayarlayın")](implementing-sirikit-images/plist02w.png)
4. Genişletme `ParameterVocabularies` tıklatıp  **+**  düğmesine tıklayın ve ayarlama **türü** için `Dictionary`:

    [ ![](implementing-sirikit-images/plist03w.png "Sözlüğe türünü ayarlayın")](implementing-sirikit-images/plist03w.png)
5. Tıklatın  **+**  yeni bir anahtar eklemek için ayarlayın **adı** için `ParameterNames` ve **türü** için `Array`:

    [ ![](implementing-sirikit-images/plist04w.png "ParameterNames ve dizi türü adı ayarlayın")](implementing-sirikit-images/plist04w.png)
6. Tıklatın  **+**  ile yeni bir anahtar eklemek için **türü** , `String` ve değeri olarak kullanılabilir parametre adlarından biri. Örneğin, `INStartWorkoutIntent.workoutName`:

    [ ![](implementing-sirikit-images/plist05w.png "The INStartWorkoutIntent.workoutName key")](implementing-sirikit-images/plist05w.png)
7. Ekleme `ParameterVocabulary` anahtarını `ParameterVocabularies` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist06w.png "Bu tür dizi ParameterVocabularies anahtarla ParameterVocabulary anahtarını ekleyin")](implementing-sirikit-images/plist06w.png)
8. Yeni bir anahtarla eklemek **türü** , `Dictionary`:

    [ ![](implementing-sirikit-images/plist07w.png "Bu tür sözlüğü ile yeni bir anahtar ekleyin")](implementing-sirikit-images/plist07w.png)
9. Ekleme `VocabularyItemIdentifier` ile anahtar **türü** , `String` ve dönem için benzersiz bir kimlik belirtin:

    [ ![](implementing-sirikit-images/plist08w.png "Dize türünde VocabularyItemIdentifier anahtarla ekleyin ve dönem için benzersiz bir kimlik belirtin")](implementing-sirikit-images/plist08w.png)
10. Ekleme `VocabularyItemSynonyms` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist09w.png "Bu tür dizi VocabularyItemSynonyms anahtarla ekleme")](implementing-sirikit-images/plist09w.png)
11. Yeni bir anahtarla eklemek **türü** , `Dictionary`:

    [ ![](implementing-sirikit-images/plist10w.png "Bu tür sözlüğü ile yeni bir anahtar ekleyin")](implementing-sirikit-images/plist10w.png)
12. Ekleme `VocabularyItemPhrase` ile anahtar **türü** , `String` ve olduğunu uygulamayı terimi tanımlama:

    [ ![](implementing-sirikit-images/plist11w.png "Dize türünde ve uygulama olduğunu tanımlama terim VocabularyItemPhrase anahtarla ekleme")](implementing-sirikit-images/plist11w.png)
13. Ekleme `VocabularyItemPronunciation` ile anahtar **türü** , `String` ve ses telaffuz koşulunun:

    [ ![](implementing-sirikit-images/plist12w.png "Dize türünde ve ses telaffuz koşulunun VocabularyItemPronunciation anahtarla ekleme")](implementing-sirikit-images/plist12w.png)
14. Ekleme `VocabularyItemExamples` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist13w.png "Bu tür dizi VocabularyItemExamples anahtarla ekleme")](implementing-sirikit-images/plist13w.png)
15. Birkaç eklemek `String` terimi örnek kullanımları anahtarlarla:

    [ ![](implementing-sirikit-images/plist14w.png "Terim örnek kullanımları birkaç dize anahtarlarıyla ekleme")](implementing-sirikit-images/plist14w.png)
16. Uygulamanın gereksinim tanımlamak için tüm diğer özel terimler için yukarıdaki adımları yineleyin.
17. Daraltma `ParameterVocabularies` anahtarı.
18. Ekleme `IntentPhrases` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist15w.png "Bu tür dizi IntentPhrases anahtarla ekleme")](implementing-sirikit-images/plist15w.png)
19. Yeni bir anahtarla eklemek **türü** , `Dictionary`:

    [ ![](implementing-sirikit-images/plist16w.png "Bu tür sözlüğü ile yeni bir anahtar ekleyin")](implementing-sirikit-images/plist16w.png)
20. Ekleme `IntentName` ile anahtar **türü** , `String` ve hedefi örneğin:

    [ ![](implementing-sirikit-images/plist17w.png "Örneğin dize türünde ve amacı ile IntentName anahtarı Ekle")](implementing-sirikit-images/plist17w.png)
21. Ekleme `IntentExamples` ile anahtar **türü** , `Array`:

    [ ![](implementing-sirikit-images/plist18w.png "Bu tür dizi IntentExamples anahtarla ekleme")](implementing-sirikit-images/plist18w.png)
22. Birkaç eklemek `String` terimi örnek kullanımları anahtarlarla:

    [ ![](implementing-sirikit-images/plist19w.png "Terim örnek kullanımları birkaç dize anahtarlarıyla ekleme")](implementing-sirikit-images/plist19w.png)
23. Uygulama kullanım örneği sağlamanız gereken tüm hedefleri için yukarıdaki adımları yineleyin.

-----

> [!IMPORTANT]
> **Not:** `AppIntentVocabulary.plist` kaydedilecek Siri test ile geliştirme ve sırasında cihazlar özel sözlük içerecek şekilde Siri için biraz zaman alabilir. Sonuç olarak, tester, güncelleştirildiğinde uygulama belirli sözlük test denemeden önce birkaç dakika beklemeniz gerekecektir.

Daha fazla bilgi için lütfen bkz bizim [uygulama özel sözlük başvuru](~/ios/platform/sirikit/understanding-sirikit.md) ve Apple'nın [belirtme özel sözlük başvuru](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

## <a name="adding-an-intents-extension"></a>Hedefleri uzantı ekleme

Uygulama SiriKit benimsemek için hazırlanan, geliştirici Siri tümleştirme için gereken hedefleri işlemek için çözüme bir (veya daha fazla) hedefleri uzantıları eklemeniz gerekir.

Her hedefleri gerekli bir uzantı için aşağıdakileri yapın:

- Hedefleri uzantı projesi Xamarin.iOS uygulaması çözüme ekleyin.
- Hedefleri uzantısını yapılandırmak `Info.plist` dosya.
- Hedefleri uzantısı ana sınıfı değiştirin.

Daha fazla bilgi için lütfen bkz bizim [hedefleri uzantı başvurusu](~/ios/platform/sirikit/understanding-sirikit.md) ve Apple'nın [hedefleri uzantı başvurusu oluşturma](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1).

### <a name="creating-the-extension"></a>Uzantısı oluşturma

Hedefleri uzantı çözüme eklemek için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Sağ tıklayın **çözüm adı** içinde **çözüm paneli** seçip **Ekle** > **Yeni Proje Ekle...** .
2. İletişim kutusundan **iOS** > **uzantıları** > **hedefi uzantısı** tıklatıp **sonraki** düğmesi: 

    [ ![](implementing-sirikit-images/intents05.png "Hedefi uzantı Seç")](implementing-sirikit-images/intents05.png)
3. Sonraki girin bir **adı** tıklatın ve hedefi uzantısı için **sonraki** düğmesi: 

    [ ![](implementing-sirikit-images/intents06.png "Hedefi uzantısı için bir ad girin")](implementing-sirikit-images/intents06.png)
4. Son olarak, tıklatın **oluşturma** düğmesi hedefi uzantısı uygulamaları çözüme eklemek için: 

    [ ![](implementing-sirikit-images/intents07.png "Hedefi uzantısı uygulamaları ekleyin")](implementing-sirikit-images/intents07.png)
5. İçinde **Çözüm Gezgini**, sağ tıklayın **başvuruları** klasörü yeni oluşturulan hedefi uzantısı. (Uygulama yukarıda oluşturduğunuz) ortak paylaşılan kod kitaplığı projesi adını denetleyin ve tıklayın **Tamam** düğmesi: 

    [ ![](implementing-sirikit-images/intents08.png "Ortak paylaşılan kod kitaplık projesinin adı seçin")](implementing-sirikit-images/intents08.png)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Sağ **çözüm adı** içinde **Çözüm Gezgini** seçip **Ekle** > **Yeni Proje Ekle...** .
2. İletişim kutusundan **iOS** > **uzantıları** > **hedefi uzantısı** tıklatıp **sonraki** düğmesi: 

    [ ![](implementing-sirikit-images/intents05w.png "Hedefi uzantı Seç")](implementing-sirikit-images/intents05w.png)
3. Sonraki girin bir **adı** tıklatın ve hedefi uzantısı için **Tamam** düğmesi.
5. İçinde **Çözüm Gezgini**, sağ tıklayın **başvuruları** klasörü yeni oluşturulan hedefi uzantısı. (Uygulama yukarıda oluşturduğunuz) ortak paylaşılan kod kitaplığı projesi adını denetleyin ve tıklayın **Tamam** düğmesi: 

    [ ![](implementing-sirikit-images/intents08w.png "Ortak paylaşılan kod kitaplık projesinin adı seçin")](implementing-sirikit-images/intents08w.png)
    
-----

Hedefi uzantıları sayısı için bu adımları yineleyin (temel [uzantıları için uygulama mimariden](#Architecting-the-App-for-Extensions) yukarıdaki bölümde) uygulama gerektirecek.

### <a name="configuring-the-infoplist"></a>Info.plist yapılandırma

Her uygulamanın çözüme eklemiş olduğunuz hedefleri uzantılar yapılandırılmalıdır `Info.plist` dosyaları uygulama ile birlikte çalışır.

Yalnızca normal uygulama uzantıyı gibi uygulamaya varolan anahtarlara sahip `NSExtension` ve `NSExtensionAttributes`. Hedefleri uzantı için yapılandırılması gereken iki yeni özniteliği vardır:

[ ![](implementing-sirikit-images/intents01.png "Yapılandırılması gereken iki yeni öznitelikler")](implementing-sirikit-images/intents01.png)

- **IntentsSupported** - gereklidir ve bir dizi uygulama amacı uzantı desteklemek istediği hedefi sınıf adını oluşur.
- **IntentsRestrictedWhileLocked** -uzantının kilit ekranı davranışını belirtmek uygulama için isteğe bağlı bir anahtardır. Uygulama hedefi uzantısını kullanmak için oturum açmanız kullanıcının gerektiren istediği hedefi sınıf adlarının dizisini oluşur.

Hedefi uzantının yapılandırmak için `Info.plist` dosya, çift tıklatın **Çözüm Gezgini** düzenlemek için açın. Ardından, geçiş **kaynak** görüntüleyin. ardından genişletin `NSExtension` ve `NSExtensionAttributes` anahtarları Düzenleyicisi'nde:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![](implementing-sirikit-images/intents02.png "Düzenleyicideki NSExtension ve NSExtensionAttributes anahtarları")](implementing-sirikit-images/intents02.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](implementing-sirikit-images/intents02w.png "Düzenleyicideki NSExtension ve NSExtensionAttributes anahtarları")](implementing-sirikit-images/intents02w.png)

-----

Genişletme `IntentsSupported` anahtarı ve bu uzantı destekleneceğini herhangi bir amaç sınıf adını ekleyin. Örnek MonkeyChat uygulaması için onu destekleyen `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![](implementing-sirikit-images/intents09.png "INSendMessageIntent anahtarı")](implementing-sirikit-images/intents09.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](implementing-sirikit-images/intents09w.png "INSendMessageIntent anahtarı")](implementing-sirikit-images/intents09w.png)

-----

Uygulama, isteğe bağlı olarak kullanıcının belirli bir amaç kullanmak için Genişlet cihaza oturum açmasını gerektiriyorsa, `IntentRestrictedWhileLocked` anahtarı ve sınırlı erişimi olan hedefleri sınıf adını ekleyin. Örnek MonkeyChat uygulama için kullanıcı ekledik Sohbet iletisi göndermek için oturum açmanız gerekir `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![](implementing-sirikit-images/intents10.png "Eklenen INSendMessageIntent anahtarı")](implementing-sirikit-images/intents10.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](implementing-sirikit-images/intents10w.png "Eklenen INSendMessageIntent anahtarı")](implementing-sirikit-images/intents10w.png)

-----


Apple'nın kullanılabilir amacı etki alanlarını tam bir listesi için lütfen bkz [hedefi etki alanları başvuru](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Ana sınıf yapılandırma

Ardından, geliştirici Siri hedefi uzantısı ana giriş noktası olarak davranır ana sınıfı yapılandırmanız gerekecektir. Öğesinin bir alt kümesi olmalıdır `INExtension` için uygun `IINIntentHandler` temsilci. Örneğin:

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

Uygulama hedefi uzantısı ana sınıf üzerinde uygulamalıdır bir insanı yalnızlaştırıcı yöntemi yoktur `GetHandler` yöntemi. Bu yöntem tarafından SiriKit amacına geçirilir ve uygulama döndürmelidir bir **hedefi işleyici** verilen hedefinin türüyle eşleşen.

Örnek MonkeyChat uygulama yalnızca bir hedefi işleme olduğundan, kendisini döndürmektir `GetHandler` yöntemi. Uzantı birden fazla hedefi işleniyorsa Geliştirici hedefi her tür için bir sınıf ekleyin ve burada dayalı bir örnek `Intent` yönteme geçirilen.

### <a name="handling-the-resolve-stage"></a>Resolve aşama işleme

Çözmek aşama hedefi uzantısı burada ve açıklamak parametreleri doğrulayın Siri geçirilen ve kullanıcının konuşma ayarlayın.

Siri gönderilir her parametre için yoktur bir `Resolve` yöntemi. Uygulama, uygulama kullanıcıdan doğru yanıt almak için Siri'nın yardım gerekebilecek her parametre için bu yöntemi uygulaması gerekir.

Örnek MonkeyChat uygulama durumunda hedefi uzantısı ileti göndermek bir veya daha fazla alıcı gerektirir. Listedeki her alıcı için uzantı aşağıdaki sonucu olan bir kişi arama yapmanız gerekir:

- Tam olarak bir eşleşen kişi bulunamadı.
- İki veya daha fazla eşleşen kişiler bulunur.
- Eşleşen hiçbir kişiler bulunur.

Ayrıca, MonkeyChat ileti gövdesi için içerik gerektirir. Kullanıcı bu sağlamamış, Siri içeriği kullanıcıdan gerekir.

Bu durumların her birinde işleyebilmesini hedefi uzantısı gerekir.


```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

Daha fazla bilgi için lütfen bkz bizim [gidermek aşama Reference](~/ios/platform/sirikit/understanding-sirikit.md) ve Apple'nın [çözümleme ve işleme hedefleri başvuru](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1).

### <a name="handling-the-confirm-stage"></a>Onayla aşama işleme

Onayla burada kullanıcının isteği yerine getirmek için bilgilerin tümünü olduğunu görmek için amacı uzantısını denetler aşamasıdır. Uygulama onayı boyunca tüm destekleyici ayrıntılarını ne olduğu hakkında için durum Siri kullanıcıyla, onaylanabilir bu hedeflenen bir eylemdir şekilde gönderecek istiyor.

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

Daha fazla bilgi için lütfen bkz bizim [onaylayın aşama Reference](~/ios/platform/sirikit/understanding-sirikit.md).

### <a name="processing-the-intent"></a>İşleme için amacını

Burada amaç uzantısı kullanıcının isteği yerine getirmek ve kullanıcı haberdar olmak için sonuçları geri Siri geçirmek için görev gerçekte gerçekleştirir noktasıdır.


```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

Daha fazla bilgi için lütfen bkz bizim [işlemek aşama Reference](~/ios/platform/sirikit/understanding-sirikit.md).

## <a name="adding-an-intents-ui-extension"></a>Hedefleri UI uzantı ekleme

İsteğe bağlı hedefleri UI uzantısı uygulamanın kullanıcı Arabirimi ve Siri deneyim halinde markalama getirmek için bir fırsat sunar ve uygulamaya bağlı eşitleyerek kullanıcılar yapın. Bu uzantıya sahip uygulama marka yanı sıra dökümü görsel ve diğer bilgileri kullanıma sunabilirsiniz.

[ ![](implementing-sirikit-images/intentsui01.png "Hedefleri UI uzantısı örnek çıkış")](implementing-sirikit-images/intentsui01.png)

Yalnızca hedefleri uzantısını gibi amaçlarını UI uzantısı için aşağıdaki adımı Geliştirici yapın:

- Hedefleri UI uzantı projesi Xamarin.iOS uygulaması çözüme ekleyin.
- Hedefleri UI uzantısı yapılandırma `Info.plist` dosya.
- Hedefleri UI uzantısı ana sınıfı değiştirin.

Daha fazla bilgi için lütfen bkz bizim [hedefleri UI uzantı başvurusu](~/ios/platform/sirikit/understanding-sirikit.md) ve Apple'nın [özel arabirimi başvuru sağlamanın](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1).

### <a name="creating-the-extension"></a>Uzantısı oluşturma

Hedefleri UI uzantı çözüme eklemek için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Sağ tıklayın **çözüm adı** içinde **çözüm paneli** seçip **Ekle** > **Yeni Proje Ekle...** .
2. İletişim kutusundan **iOS** > **uzantıları** > **hedefi UI uzantısı** tıklatıp **sonraki** düğmesi: 

    [ ![](implementing-sirikit-images/intents11.png "Hedefi UI uzantı Seç")](implementing-sirikit-images/intents11.png)
3. Sonraki girin bir **adı** tıklatın ve hedefi uzantısı için **sonraki** düğmesi: 

    [ ![](implementing-sirikit-images/intents12.png "Hedefi uzantısı için bir ad girin")](implementing-sirikit-images/intents12.png)
4. Son olarak, tıklatın **oluşturma** düğmesi hedefi uzantısı uygulamaları çözüme eklemek için: 

    [ ![](implementing-sirikit-images/intents13.png "Hedefi uzantısı uygulamaları ekleyin")](implementing-sirikit-images/intents13.png)
5. İçinde **Çözüm Gezgini**, sağ tıklayın **başvuruları** klasörü yeni oluşturulan hedefi uzantısı. (Uygulama yukarıda oluşturduğunuz) ortak paylaşılan kod kitaplığı projesi adını denetleyin ve tıklayın **Tamam** düğmesi: 

    [ ![](implementing-sirikit-images/intents14.png "Ortak paylaşılan kod kitaplık projesinin adı seçin")](implementing-sirikit-images/intents14.png)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Sağ **çözüm adı** içinde **Çözüm Gezgini** seçip **Ekle** > **Yeni Proje Ekle...**
2. İletişim kutusundan **iOS** > **uzantıları** > **hedefi UI uzantısı** tıklatıp **sonraki** düğme.
3. Sonraki girin bir **adı** tıklatın ve hedefi uzantısı için **Tamam** düğmesi.
5. İçinde **Çözüm Gezgini**, sağ tıklayın **başvuruları** klasörü yeni oluşturulan hedefi uzantısı. (Uygulama yukarıda oluşturduğunuz) ortak paylaşılan kod kitaplığı projesi adını denetleyin ve tıklayın **Tamam** düğmesi.
    
-----

### <a name="configuring-the-infoplist"></a>Info.plist yapılandırma

Hedefleri UI uzantının yapılandırma `Info.plist` dosya uygulama ile birlikte çalışır.

Yalnızca normal uygulama uzantıyı gibi uygulamaya varolan anahtarlara sahip `NSExtension` ve `NSExtensionAttributes`. Hedefleri uzantı için yapılandırılması gereken bir yeni öznitelik vardır:

[ ![](implementing-sirikit-images/intents03.png "Yapılandırılması gereken bir yeni özniteliği")](implementing-sirikit-images/intents03.png)

**IntentsSupported** gereklidir ve bir dizi uygulama amacı uzantı desteklemek istediğiniz hedefi sınıf adını oluşur.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Hedefi UI uzantının yapılandırmak için `Info.plist` dosya, çift tıklatın **Çözüm Gezgini** düzenlemek için açın. Ardından, geçiş **kaynak** görüntüleyin. ardından genişletin `NSExtension` ve `NSExtensionAttributes` anahtarları Düzenleyicisi'nde:

[ ![](implementing-sirikit-images/intents04.png "Düzenleyicideki NSExtension ve NSExtensionAttributes anahtarları")](implementing-sirikit-images/intents04.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Hedefi UI uzantının yapılandırmak için `Info.plist` dosya, çift tıklatın **Çözüm Gezgini** düzenlemek için açın. Genişletme `NSExtension` ve `NSExtensionAttributes` anahtarları Düzenleyicisi'nde:

[ ![](implementing-sirikit-images/intents04w.png "Düzenleyicideki kurulacağı NSExtension ve NSExtensionAttributes anahtarları")](implementing-sirikit-images/intents04w.png)

-----

Genişletme `IntentsSupported` anahtarı ve bu uzantı destekleneceğini herhangi bir amaç sınıf adını ekleyin. Örnek MonkeyChat uygulaması için onu destekleyen `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![](implementing-sirikit-images/intents15.png "INSendMessageIntent anahtarı")](implementing-sirikit-images/intents15.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](implementing-sirikit-images/intents15w.png "INSendMessageIntent anahtarı")](implementing-sirikit-images/intents15w.png)

-----

Apple'nın kullanılabilir amacı etki alanlarını tam bir listesi için lütfen bkz [hedefi etki alanları başvuru](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Ana sınıf yapılandırma

Siri hedefi UI uzantısı ana giriş noktası olarak davranır ana sınıfı yapılandırın. Öğesinin bir alt kümesi olmalıdır `UIViewController` için uygun `IINUIHostedViewController` arabirimi. Örneğin:

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Siri geçecek bir `INInteraction` sınıf örneğine `Configure` yöntemi `UIViewController` hedefi UI uzantısı içinde örneği.

`INInteraction` Nesnesi üç temel bilgiler uzantısına sağlar:

1. İşlenmekte olan hedefi nesnesi.
2. Hedefi yanıt nesnesinden `Confirm` ve `Handle` hedefi uzantı yöntemleri.
3. Siri ve uygulama arasındaki etkileşimi durumunu tanımlar etkileşim durumu.

`UIViewController` Örneğidir Siri etkileşim ilkesine sınıfı ve sınıfından devraldığı için `UIViewController`, tüm Uıkit özelliklerini erişimi vardır.

Siri çağırdığında `Configure` yöntemi `UIViewController` görünüm denetleyicisini ya da bir Siri Snippit veya eşlemeleri kartı barındırılmasına belirten bir görünümü bağlamında geçirir.

Siri Ayrıca uygulama gereken görünümün istenen boyuta uygulama işlemi tamamladıktan sonra bu yapılandırma döndürmek için tamamlama işleyici geçer.

### <a name="design-the-ui-in-ios-designer"></a>Tasarım kullanıcı Arabiriminde iOS Tasarımcısı

Düzen iOS Tasarımcısı hedefleri UI uzantının kullanıcı arabirimi. Uzantının çift `MainInterface.storyboard` dosyasını **Çözüm Gezgini** düzenlemek için açın. Tüm kullanıcı arabirimi oluşturma ve değişiklikleri kaydetmek için gerekli kullanıcı Arabirimi öğeleri sürükleyin.

> [!IMPORTANT]
> **Not:** etkileşimli öğeler eklemek mümkün olmakla birlikte `UIButtons` veya `UITextFields` hedefi UI uzantının için `UIViewController`, bu amacı kullanıcı Arabiriminde etkileşimli olmayan olarak kesinlikle yasaktır ve kullanıcı etkileşim mümkün olmayacaktır. bunlarla.

### <a name="wire-up-the-user-interface"></a>Kablo yukarı kullanıcı arabirimi

Hedefleri UI uzantının kullanıcı arabirimiyle iOS Tasarımcısı oluşturulan Düzenle `UIViewController` alt sınıf ve geçersiz kılma `Configure` yöntemini aşağıdaki şekilde:

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>Varsayılan Siri UI geçersiz kılma

Hedefleri UI uzantısı her zaman diğer Siri gibi içerik uygulama simgesine ve kullanıcı arabirimini başındaki ad ile birlikte görüntülenir ve yüklenecek, amacına, (örneğin, Gönder veya iptal) düğmeleri en altında görüntülenebilir.

Uygulama Siri Mesajlaşma gibi varsayılan olarak, kullanıcı için görüntüleme bilgileri veya uygulamayı bir uygulamaya özel olarak hazırlanmış varsayılan deneyimi yeri değiştirebilirsiniz eşlemeleri yeri değiştirebilirsiniz birkaç örneği vardır.

Hedefleri UI uzantısı varsayılan Siri UI öğelerini geçersiz kılması gerekiyorsa `UIViewController` bir alt kümesi uygulayan gerekecektir `IINUIHostedViewSiriProviding` arabirimi ve belirli arabirimi öğesi görüntülenmesini katılımı.

Aşağıdaki kodu ekleyin `UIViewController` hedefi UI uzantısı zaten ileti içeriği görüntüleme Siri bildirmek için bir alt kümesi:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>Dikkat Edilecekler

Apple Geliştirici tasarlarken ve amacı UI uzantıları uygulama hesaba aşağıdaki konular ele önerir:

- **Bilinçli, bellek kullanımı olması** - çünkü hedefi UI uzantıları geçici ve kısa bir süre gösterilen yalnızca sistem tam bir uygulamayla halinden daha sıkı bellek kısıtlamaları getirir.
- **Minimum ve maksimum görünüm boyutlarını dikkate** -hedefi UI uzantıları her iOS cihaz türünün, boyutuna ve yönüne iyi görünür emin olun. Ayrıca, uygulama Siri geri gönderir istenen boyuta verilmesi mümkün olmayabilir.
- **Esnek ve Uyarlamalı düzeni desenleri kullanmak** - yeniden UI her cihazda harika görünüyor emin olmak için.

## <a name="summary"></a>Özet

Bu makalede, SiriKit kapsamdaki ve gösterilen nasıl Siri ve haritalar uygulama iOS cihazında kullanarak kullanıcı tarafından erişilebilir hizmetleri sağlamak için Xamarin.iOS uygulamaları için eklenebilir.




## <a name="related-links"></a>İlgili bağlantılar

- [ElizaChat Sample](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Hedefleri Framework başvurusu](https://developer.apple.com/reference/intents)
- [Hedefleri UI Framework başvurusu](https://developer.apple.com/reference/intentsui)
- [Hedefi etki alanları başvurusu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
