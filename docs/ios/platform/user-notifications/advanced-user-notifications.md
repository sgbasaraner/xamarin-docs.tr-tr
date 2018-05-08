---
title: Gelişmiş Kullanıcı bildirimleri
description: Bu makalede, yeni kullanıcı bildirimleri framework daha derin göz ve bunu bir Xamarin.iOS uygulaması içinde yararlanmak nasıl alır.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: bd8a95afc5bdd5aed958913d63f9b6cfe853677e
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="advanced-user-notifications"></a>Gelişmiş Kullanıcı bildirimleri

_Bu makalede, yeni kullanıcı bildirimleri framework daha derin göz ve bunu bir Xamarin.iOS uygulaması içinde yararlanmak nasıl alır._

Yeni ios'a 10, kullanıcı bildirimi teslimi ve yerel ve uzak bildirimler işleme framework olanak sağlar. Bu çerçeve kullanarak, bir uygulama ya da uygulama uzantısı yerel bildirimler teslimini konum gibi koşulları veya günün saatini belirterek zamanlayabilirsiniz.

## <a name="about-user-notifications"></a>Kullanıcı bildirimleri hakkında

Yeni kullanıcı bildirimi framework teslim ve yerel ve uzak bildirimler işleme olanak sağlar. Bu çerçeve kullanarak, bir uygulama ya da uygulama uzantısı yerel bildirimler teslimini konum gibi koşulları veya günün saatini belirterek zamanlayabilirsiniz.

Ayrıca, uygulama veya uzantı alabilir (ve büyük olasılıkla değiştirmek) hem yerel hem de uzak bildirimler kullanıcının iOS cihazına teslim edilir.

Yeni Kullanıcı bildirim UI framework bir uygulama ya da uygulama kullanıcıya sunulduğunda hem yerel hem de uzak bildirimler görünümünü özelleştirmek için uzantı sağlar.

Bu çerçeve, bir uygulama bir kullanıcıya bildirimleri teslim aşağıdaki yöntemleri sağlar:

- **Görsel uyarılar** - burada bildirim aşağı dökümünü başlık olarak ekranın üstünde yapar.
- **Ses ve Vibrations** -bir bildirim ile ilişkili olabilir.
- **Uygulama simgesi Badging** - uygulamanın simgesi yeni içeriğin mevcut olduğunu gösteren bir gösterge görüntüler burada. Okunmamış e-posta iletilerinin sayısı gibi.

Ayrıca, kullanıcının geçerli bağlam bağlı olarak, bir bildirim sunulan farklı yolu vardır:

- Cihaz kilidi ise bildirim aşağı ekranın üst kısmından bir başlık dökümünü yapar.
- Cihaz kilitli bildirim kullanıcının kilit ekranında görüntülenir.
- Kullanıcıya bir bildirim eksik olduğunu, bildirim merkezi açın ve kullanılabilir tüm bekleyen bildirimler görüntüleyin.

Bir Xamarin.iOS uygulaması gönderebilmesi için kullanıcı bildirimleri iki tür vardır:

- **Yerel bildirimler** -bu kullanıcıların cihazda yerel olarak yüklü uygulamalar tarafından gönderilir.
- **Uzak bildirimler** - uzak bir sunucuya ve kullanıcıya ya da gönderildiği veya uygulamanın içeriğinin bir arka plan güncelleştirmesi tetikler.

Daha fazla bilgi için lütfen bkz bizim [Gelişmiş Kullanıcı bildirimleri](~/ios/platform/user-notifications/enhanced-user-notifications.md) belgeleri.

## <a name="the-new-user-notification-interface"></a>Yeni Kullanıcı bildirim arabirimi

İOS 10 kullanıcı bildirimlerini sağlayan bir başlık, alt başlık ve sunulabilir isteğe bağlı medya ekleri gibi daha fazla içerik kilit ekranında, aygıtın veya bildirim merkezinde üstünde başlık olarak yeni bir kullanıcı Arabirimi tasarımı sunulur.

Kullanıcı bildirimi iOS 10 görüntülendiği olsun, aynı görünüme ve hisse sahip ve aynı özellikler ve işlevsellik ile sunulur.

İOS 8'de, Apple tıklatılabilir Geliştirici burada ve özel eylemler için bir bildirim ekleme kullanıcı uygulamayı başlatmak gerek kalmadan bir bildirim eyleme izin ver bildirimleri kullanıma sunuldu. İOS 9'da, kullanıcının metin girişi içeren bir bildirim yanıt veren hızlı yanıt tıklatılabilir bildirimlerle Apple geliştirilmiştir.

Kullanıcı bildirimleri iOS 10 kullanıcı deneyimi daha ayrılmaz bir parçası olduğundan, Apple burada bir bildirim kullanıcı ve bir özel kullanıcı arabirimi zengin etkileşim sağlamak için görüntü 3B dokunma desteklemek üzere işlem yapılabilir bildirimleri daha fazla genişletilmiştir bildirim ile.

Özel Kullanıcı bildirim UI görüntülendiğinde, kullanıcı bildirimi bağlı herhangi bir eylem ile etkileşime giren, özel kullanıcı Arabirimi anında ne değiştirildi için geri bildirim sağlamak için güncelleştirilebilir.

Yeni iOS 10, Kullanıcı bildirim UI API kolayca, bu yeni Kullanıcı bildirim UI özelliklerden yararlanmak bir Xamarin.iOS uygulaması sağlar.

## <a name="adding-media-attachments"></a>Medya eklerini ekleme

Ortam öğesi (örneğin, bir fotoğraf) ekleme yeteneği iOS 10 eklenen kullanıcılar arasında paylaşılan daha yaygın öğelerden birini fotoğrafları, burada sunulan ve kullanıcıya bildirim'ın ğlamda geri kalanı ile birlikte kullanıma hazır olacak doğrudan bir bildirim için olduğundan NT.

Ancak, gönderilmesinde gerçekleştirilen boyutları nedeniyle uzak bir bildirim yükü ekleme bile bir küçük resim, pratik haline gelir. Bu durumu yönetmek için geliştirici yeni hizmeti uzantı iOS 10 görüntü (örneğin, bir CloudKit veri deposu) başka bir kaynaktan indirin ve kullanıcıya gösterilmeden önce bildirim 's içeriği eklemek için kullanabilirsiniz.

Bir uzak hizmet uzantısı tarafından değiştirilecek bildirimi için yükü olarak kesilebilir işaretlenmesi gerekir. Örneğin:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

Aşağıdaki işlemine genel bakış göz atın:

[![](advanced-user-notifications-images/extension02.png "Medya ekleri işlem ekleme")](advanced-user-notifications-images/extension02.png#lightbox)

Uzak bildirim (APN) üzerinden aygıta teslim sonra hizmet uzantısı sonra gerekli görüntüsü istenen herhangi bir yöntem aracılığıyla yükleyebilirsiniz (gibi bir `NSURLSession`) ve görüntünün aldıktan sonra bildirim ve görüntü içeriğini değiştirebilirsiniz Bu kullanıcı için.

Bu işlem kodda nasıl işleneceğini bir örnek verilmiştir:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyNotification
{
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Constructors
        public NotificationService ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            // Get file URL
            var attachementPath = request.Content.UserInfo.ObjectForKey (new NSString ("my-attachment"));
            var url = new NSUrl (attachementPath.ToString ());

            // Download the file
            var localURL = new NSUrl ("PathToLocalCopy");

            // Create attachment
            var attachmentID = "image";
            var options = new UNNotificationAttachmentOptions ();
            NSError err;
            var attachment = UNNotificationAttachment.FromIdentifier (attachmentID, localURL, options , out err);

            // Modify contents
            var content = request.Content.MutableCopy() as UNMutableNotificationContent;
            content.Attachments = new UNNotificationAttachment [] { attachment };

            // Display notification
            contentHandler (content);
        }

        public override void TimeWillExpire ()
        {
            // Handle service timing out
        }
        #endregion
    }
}
```

APNs bildirim alındığında, görüntünün özel adres içerikten okuyun ve dosya sunucusundan indirilir. Ardından bir `UNNotificationAttachement` benzersiz bir kimlik ve yerel bir konum görüntünün ile oluşturulur (olarak bir `NSUrl`). Bildirim içerik bir değişebilir kopyası oluşturulur ve medya ekleri eklenir. Son olarak, bildirim kullanıcıya çağrılarak görüntülenen `contentHandler`.

Ek bir bildirim eklendikten sonra Sistem taşıma ve dosya yönetimi devralır.

Uzak yukarıda sunulan bildirimler yanı sıra medya ekler yerel bildirimler, aynı zamanda desteklenen nerede `UNNotificationAttachement` oluşturulur ve içeriği ile birlikte bildirim bağlı.

İOS 10 bildiriminde görüntü Media ekleri destekler (statik ve GIF), ses veya video ve sistem otomatik olarak görüntüler doğru özel kullanıcı Arabirimi türlerinin her biri bu ekleri için bildirim kullanıcıya sunulduğunda.

> [!NOTE]
> Her iki ortam boyutu en iyi duruma getirme dikkatli olunmalıdır ve uygulamanın hizmeti uzantı çalışırken Uzak sunucudan medya indirmek için (veya yerel bildirimler için ortam derlemek için) sistem süresini hem katı sınırları uygular. Örneğin, görüntünün aşağı genişletilmiş bir sürümünü veya küçük bir küçük bildiriminde sunulacak bir video gönderme göz önünde bulundurun.

## <a name="creating-custom-user-interfaces"></a>Özel kullanıcı arabirimi oluşturma

Kendi kullanıcı bildirimleri için özel bir kullanıcı arabirimi oluşturmak için uygulamanın çözüme (IOS 10 yeni) bir bildirim içerik uzantısı eklemek Geliştirici gerekir.

Bildirim içerik uzantısı bildirim UI kendi görünümler ekleyebilir ve istedikleri herhangi bir içerik çizmek Geliştirici sağlar. Ancak, etkileşimli kullanıcı Arabirimi pencere öğeleri (örneğin, düğmeler veya metin alanları) olan **değil** bildirim UI tarafından desteklenen ve özel bir kullanıcı Arabirimi tasarım hiçbir zaman eklenmelidir.

Kullanıcı bildirimi ile kullanıcı etkileşimi desteklemek için özel eylemler oluşturulabilir, sisteme kayıtlı ve sistemiyle zamanlanmadan önce bildirime bağlı. Bildirim içerik uzantısı, bu eylemleri işlenmesini işlemek üzere çağrılır. Bkz: [bildirim eylemleriyle çalışma](~/ios/platform/user-notifications/enhanced-user-notifications.md) bölümünü [Gelişmiş Kullanıcı bildirimleri](~/ios/platform/user-notifications/enhanced-user-notifications.md) özel eylemler hakkında daha fazla ayrıntı için belge.

Özel kullanıcı Arabirimi ile bir kullanıcı bildirimi kullanıcıya sunulduğunda, aşağıdaki öğeleri olacaktır:

[![](advanced-user-notifications-images/customui01.png "Özel kullanıcı Arabirimi öğeleri içeren bir Kullanıcı bildirim")](advanced-user-notifications-images/customui01.png#lightbox)

Kullanıcı (bildirim gösterilen) özel eylemler ile etkileşime giren, belirli bir eylem çağrıldığında ne olarak kullanıcı geribildirim vermek için kullanıcı arabirimi güncelleştirilebilir.

### <a name="adding-a-notification-content-extension"></a>Bir bildirim içerik uzantısına ekleme

Özel Kullanıcı bildirim UI bir Xamarin.iOS uygulaması uygulamak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'da uygulamanın çözümü açın
2. Çözüm adına sağ tıklayın **çözüm paneli** seçip **Ekle** > **Yeni Proje Ekle**.
3. Seçin **iOS** > **uzantıları** > **bildirim içerik uzantıları** tıklatıp **sonraki** düğmesi: 

    [![](advanced-user-notifications-images/notify01.png "Bildirim içerik uzantıları seçin")](advanced-user-notifications-images/notify01.png#lightbox)
4. Girin bir **adı** tıklatın ve uzantı için **sonraki** düğmesi: 

    [![](advanced-user-notifications-images/notify02.png "Uzantı için bir ad girin")](advanced-user-notifications-images/notify02.png#lightbox)
5. Ayarlama **proje adı** ve/veya **çözüm adı** gerekli ve tıklatırsanız **oluşturma** düğmesi: 

    [![](advanced-user-notifications-images/notify03.png "Proje adı ve/veya çözüm adı")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Mac için Visual Studio'da uygulamanın çözümü açın
2. Çözüm adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle > Yeni proje...** .
3. Seçin **Visual C# > iOS uzantıları > bildirim içerik uzantısı**:

    [![](advanced-user-notifications-images/notify01.w157-sml.png "Bildirim içerik uzantıları seçin")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. Girin bir **adı** tıklatın ve uzantı için **Tamam** düğmesi.

-----

Bildirim içerik uzantısı çözüme eklendiğinde, üç dosya uzantının projesinde oluşturulacak:

1. `NotificationViewController.cs` -Bu bildirim içerik uzantısı ana görünüm denetleyicisidir.
2. `MainInterface.storyboard` -Burada Geliştirici Tasarımcısı iOS bildirim içerik uzantısında görünür kullanıcı arabirimini yerleştirir.
3. `Info.plist` -Bildirim içerik uzantı yapılandırmasını denetler.

Varsayılan `NotificationViewController.cs` dosya şu şekilde görünür:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
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
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

        }
        #endregion
    }
}
```

`DidReceiveNotification` Bildirim içerik uzantısı içeriğini Özel UI doldurabilirsiniz böylece bildirim kullanıcı tarafından genişletildiğinde yöntemi çağrıldığında `UNNotification`. Yukarıdaki örnekte, bir etiket adı koduyla maruz görünümü eklendi `label` ve bildirim gövdesini görüntülemek için kullanılır.

### <a name="setting-the-notification-content-extensions-categories"></a>Bildirim içerik uzantının kategorileri ayarlama

Sistem, uygulamanın bildirim içerik uzantısı için yanıt belirli kategorilerini temel alarak bulma konusunda bilgi sahibi olmak gerekir. Aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Uzantının çift `Info.plist` dosyasını **çözüm paneli** düzenlemek için açın.
2. Geçiş **kaynak** görünümü.
3. Genişletme `NSExtension` anahtarı.
4. Ekleme `UNNotificationExtensionCategory` anahtar türü olarak **dize** uzantısı ait olduğu kategoriyi değeriyle (Bu örnekte ' olay davet): 

    [![](advanced-user-notifications-images/customui02.png "UNNotificationExtensionCategory anahtarı Ekle")](advanced-user-notifications-images/customui02.png#lightbox)
5. Değişikliklerinizi kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uzantının çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
3. Genişletme `NSExtension` anahtarı.
4. Ekleme `UNNotificationExtensionCategory` anahtar türü olarak **dize** uzantısı ait olduğu kategoriyi değeriyle (Bu örnekte ' olay davet): 

    [![](advanced-user-notifications-images/customui02w.png "UNNotificationExtensionCategory anahtarı Ekle")](advanced-user-notifications-images/customui02w.png#lightbox)
5. Değişikliklerinizi kaydedin.

-----

Bildirim içerik uzantısı kategorileri (`UNNotificationExtensionCategory`) bildirim eylemlerini kaydetmek için kullanılan aynı kategori değerlerini kullanın. Burada uygulama kullanacağınız aynı kullanıcı arabirimini için birden çok kategori durumda geçiş `UNNotificationExtensionCategory` türüne **dizi** ve tüm gerekli kategorilerini sağlayın. Örneğin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](advanced-user-notifications-images/customui03.png "Bildirim içerik uzantısı kategorileri")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui03w.png "Bildirim içerik uzantısı kategorileri")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>Varsayılan bildirim içerik gizleme

Burada özel bildirim UI görüntüleme aynı içerik bildirim (başlık, alt başlık ve otomatik olarak bildirim UI alt kısmında görüntülenen gövde) varsayılan olarak durumda ekleyerekbuvarsayılanbilgilerigizlenebilir`UNNotificationExtensionDefaultContentHidden`anahtarını `NSExtensionAttributes` anahtar türü olarak **Boolean** değerini `YES` uzantının içinde `Info.plist` dosyası:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](advanced-user-notifications-images/customui04.png "Varsayılan bilgileri bulma")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui04w.png "Varsayılan bilgileri bulma")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>Özel kullanıcı Arabirimi tasarlama

Bildirim içerik uzantının özel kullanıcı arabirimi tasarlamak için çift `MainInterface.storyboard` iOS Tasarımcısı düzenlemek üzere açmak için dosyayı sürükleyin istenen arabirim oluşturmak için gereken öğeleri (gibi `UILabels` ve `UIImageViews`).

> [!NOTE]
> Bildirim UI mu _değil_ bildirim içerik uzantısında metin alanları veya düğmeleri gibi etkileşimli denetimleri destekler. Film şeridi için eklenebilir, ancak kullanıcı etkileşim mümkün olmayacaktır. Özel bir bildirim kullanıcı Arabirimi için kullanıcı etkileşimi eklemek için bunun yerine özel eylemleri kullanın.

Kullanıcı arabirimini yerleştirilmiş ve gerekli denetimleri kullanıma C# kodu, açın `NotificationViewController.cs` düzenleme ve değiştirme `DidReceiveNotification` Kullanıcı bildirim genişletirken UI doldurmak için yöntemi. Örneğin:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
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
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

        }
        #endregion
    }
}
```

### <a name="setting-the-content-area-size"></a>İçerik alanının boyutunu ayarlama

Kullanıcıya görüntülenen içerik alanının boyutunu ayarlamak için aşağıdaki kodu ayarı `PreferredContentSize` özelliğinde `ViewDidLoad` istenen boyuta yöntemi. Bu boyut da Tasarımcısı iOS görünümünde kısıtlamaları uygulayarak ayarlanması, bunlar için en uygun yöntemi seçmek için geliştirici açılacak.

Sistem içerik uzantısı çağrılır, bildirim önce içerik alanı zaten çalışıyor bildirim başlar çünkü tam boyuta sahip ve kullanıcıya sunulduğunda istenen boyuta aşağıya doğru animasyonlu.

Bu etkiyi ortadan kaldırmak için Düzenle `Info.plist` dosya uzantısı ve kümesi için `UNNotificationExtensionInitialContentSizeRatio` , anahtar `NSExtensionAttributes` türü için anahtar **numarası** istenen oranını temsil eden bir değere sahip. Örneğin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](advanced-user-notifications-images/customui05.png "UNNotificationExtensionInitialContentSizeRatio anahtarı")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui05w.png "UNNotificationExtensionInitialContentSizeRatio anahtarı")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>Medya ekleri özel kullanıcı Arabirimi kullanma

Çünkü medya ekler (de görüldüğü gibi [medya eklerini ekleme](#Adding-Media-Attachments) yukarıdaki bölümde) bildirim yükü parçası olan, bunlar erişilebildiğini ve varsayılan olarak yalnızca olacaktır gibi bildirim içerik uzantısı'nda görüntülenen Bildirim kullanıcı Arabirimi.

Örneğin, yukarıdaki Özel UI dahil bir `UIImageView` C# kodu için açık, aşağıdaki kod, ondan medya ekli doldurmak için kullanılan:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
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
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }
        #endregion
    }
}
```

Medya ek sistem tarafından yönetildiğinden, bu uygulamanın sandbox dışında var. Uzantı bu dosyaya erişim çağırarak istediği sistem bildirmesi gerekir. `StartAccessingSecurityScopedResource` yöntemi. Uzantı dosyası ile işiniz bittiğinde, çağırmak gereken `StopAccessingSecurityScopedResource` bağlantısını serbest bırakmak için.

### <a name="adding-custom-actions-to-a-custom-ui"></a>Özel Eylemler özel bir kullanıcı Arabirimi ekleme

Metin alanları veya düğmeleri gibi denetimleri UI tasarımında desteklenmez için bildirim UI yukarıda belirtildiği gibi kullanıcı etkileşimi desteklemez.

Bunun yerine, standart özel eylemler mekanizması, özel bir bildirim UI etkileşim eklemek için kullanılır. Bkz: [bildirim eylemleriyle çalışma](~/ios/platform/user-notifications/enhanced-user-notifications.md) bölümünü [Gelişmiş Kullanıcı bildirimleri](~/ios/platform/user-notifications/enhanced-user-notifications.md) özel eylemler hakkında daha fazla ayrıntı için belge.

Özel Eylemler ek olarak, aşağıdaki yerleşik eylemleri de bildirim içerik uzantısı yanıt vermesini sağlayabilirsiniz:

- **Varsayılan eylem** -uygulamasını açın ve verilen bildirim ayrıntılarını görüntülemek için bir bildirim kullanıcı dokunur durumunda olur.
- **Eylem yok sayın** -belirli bir bildirim kullanıcı atar Bu eylem uygulamaya gönderilir.

Bildirim içerik uzantıları kullanıcı özel eylemlerden birini çalıştırdığında, kullanıcı güncelleştirme becerisini de, ne zaman kabul gibi bir tarih gösteren gibi kullanıcı dokunur **kabul** özel eylem düğmesi. Ayrıca, bildirim içerik uzantıları bildirim kapatılmadan önce kullanıcının kendi eylem etkisini görebilmeniz için bildirim UI işten çıkarma gecikme sistem anlayabilirsiniz.

Bu ikinci sürümü uygulayarak yapılır `DidReceiveNotification` tamamlama işleyicisi içerir yöntemi. Örneğin:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;
using CoreGraphics;

namespace myApp {
    public class NotificationViewController : UIViewController, UNNotificationContentExtension {

        public override void ViewDidLoad() {
            base.ViewDidLoad();

            // Adjust the size of the content area
            var size = View.Bounds.Size
            PreferredContentSize = new CGSize(size.Width, size.Width/2);
        }

        public void DidReceiveNotification(UNNotification notification) {

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = Content.UserInfo["location"] as string;
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments[0];
                if (attachment.Url.StartAccessingSecurityScopedResource()) {
                    EventImage.Image = UIImage.FromFile(attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch(response.ActionIdentifier){
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
    }
}
```

Ekleyerek `Server.PostEventResponse` işleyicisine `DidReceiveNotification` yöntemi bildirim içerik uzantısı, uzantı *gerekir* tüm özel eylemleri tanıtıcı. Uzantı de içeren uygulama açın özel eylemler değiştirerek iletebilir `UNNotificationContentExtensionResponseOption`. Örneğin:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>Özel kullanıcı Arabirimi metin giriş eyleminde ile çalışma

Uygulama ve bildirim tasarımını bağlı olarak (örneğin, bir iletiyi yanıtlarken) bildirim metin girerek kullanıcının gerektiren zamanlar olabilir. Standart bir bildirim gibi bir bildirim içerik uzantısı yerleşik metin giriş eylem erişebilir.

Örneğin:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Computed Properties
        // Allow to take input
        public override bool CanBecomeFirstResponder {
            get { return true; }
        }

        // Return the custom created text input view with the
        // required buttons and return here
        public override UIView InputAccessoryView {
            get { return InputView; }
        }
        #endregion

        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
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
        #endregion

        #region Private Methods
        private UNNotificationCategory MakeExtensionCategory ()
        {

            // Create Accept Action
            ...

            // Create decline Action
            ...

            // Create Text Input Action
            var commentID = "comment";
            var commentTitle = "Comment";
            var textInputButtonTitle = "Send";
            var textInputPlaceholder = "Enter comment here...";
            var commentAction = UNTextInputNotificationAction.FromIdentifier (commentID, commentTitle, UNNotificationActionOptions.None, textInputButtonTitle, textInputPlaceholder);

            // Create category
            var categoryID = "event-invite";
            var actions = new UNNotificationAction [] { acceptAction, declineAction, commentAction };
            var intentIDs = new string [] { };
            var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);

            // Return new category
            return category;

        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Is text input?
            if (response is UNTextInputNotificationResponse) {
                var textResponse = response as UNTextInputNotificationResponse;
                Server.Send (textResponse.UserText, () => {
                    // Close Notification
                    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
                });
            }

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch (response.ActionIdentifier) {
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
        #endregion
    }
}
```

Bu kod yeni bir metin giriş eylem oluşturur ve uzantının kategorisine ekler (içinde `MakeExtensionCategory`) yöntemi. İçinde `DidReceive` yöntemini geçersiz kılma aşağıdaki kodla metin girerek kullanıcı işler:

```csharp
// Is text input?
if (response is UNTextInputNotificationResponse) {
    var textResponse = response as UNTextInputNotificationResponse;
    Server.Send (textResponse.UserText, () => {
        // Close Notification
        completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
    });
}
```

Özel düğmeler metin giriş alanına eklemek için tasarım çağrılar içeriyorsa, bunları eklemek için aşağıdaki kodu ekleyin:

```csharp
// Allow to take input
public override bool CanBecomeFirstResponder {
    get {return true;}
}

// Return the custom created text input view with the
// required buttons and return here
public override UIView InputAccessoryView {
    get {return InputView;}
}
```

Açıklama eylem kullanıcı tarafından tetiklendiğinde görünüm denetleyicisini ve özel metin giriş alanını etkinleştirilmesi gerekir:

```csharp
// Update UI when the user interacts with the
// Notification
Server.PostEventResponse += (response) {
    // Take action based on the response
    switch(response.ActionIdentifier){
    ...
    case "comment":
        BecomeFirstResponder();
        TextField.BecomeFirstResponder();
        break;
    }

    // Close Notification
    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);

};
```

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.iOS uygulaması yeni kullanıcı bildirimi framework kullanılarak bir Gelişmiş bakış duruma getirdi. Hem yerel hem de uzak bildirim medya eklerini ekleme kapsamdaki ve özel bildirim Uı'lar oluşturmak için Yeni Kullanıcı bildirim kullanıcı arabirimini kullanarak ele.


## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications Framework başvurusu](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Yerel ve uzak bildirim Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
