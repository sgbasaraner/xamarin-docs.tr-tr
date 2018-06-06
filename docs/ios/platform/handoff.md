---
title: Xamarin.iOS iletimi
description: Bu makalede aktarmak için bir Xamarin.iOS uygulaması ile iletim çalışma kapsar kullanıcı üzerinde çalışan uygulamalar arasında kullanıcı etkinlikleri kullanıcının diğer aygıtlar.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ec324e8fb8327b622424311b89567608311a6a19
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787526"
---
# <a name="handoff-in-xamarinios"></a>Xamarin.iOS iletimi

_Bu makalede aktarmak için bir Xamarin.iOS uygulaması ile iletim çalışma kapsar kullanıcı üzerinde çalışan uygulamalar arasında kullanıcı etkinlikleri kullanıcının diğer aygıtlar._

Apple aynı uygulama veya aynı etkinlik destekleyen başka bir uygulama çalıştıran başka bir cihaza iOS 8 ve OS X cihazlarını birinde başlatılan etkinliklerin aktarmak kullanıcı için ortak bir mekanizma sağlamak için Yosemite (10.10) iletimi kullanıma sunuldu.

[![](handoff-images/handoff02.png "İletim işlemi gerçekleştirmeye dair bir örnek")](handoff-images/handoff02.png#lightbox)

Bu makalede, bir Xamarin.iOS uygulaması paylaşımı etkinlik etkinleştirme hızlı bir göz atalım ve ayrıntılı iletimi framework kapsar:

## <a name="about-handoff"></a>İletim hakkında

İletim (sürekliliği olarak da bilinir) Apple, iOS 8 ve OS X Yosemite (10.10) tarafından kullanıcının cihazlarını (iOS veya Mac) birinde bir etkinlik başlangıç ve aynı etkinliğin başka cihazlarını (kullanıcının iClou tarafından tanımlandığı gibi çalışmaya devam etmek için bir yol olarak sunulmuştur d hesap için).

İletim Gelişmiş arama özellikleri iOS 9 ayrıca yeni, desteklemek için genişletilmiştir. Daha fazla bilgi için lütfen bkz bizim [arama geliştirmeleri](~/ios/platform/search/index.md) belgeleri.

Örneğin, kullanıcı bir e-posta kendi iPhone üzerinde başlatmak ve sorunsuz bir şekilde e-posta ile tüm doldurulmuş aynı ileti bilgileri ve bunların iOS sol aynı konumda imleç kendi Mac üzerinde çalışmaya devam etmek.

Aynı paylaşmak uygulamalarınızı hiçbirini _Takım Kimliği_ (Mac, kuruluş için bu uygulama sürece ya da kullanıcı etkinlikleri uygulama arasında devam etmek için Handoff'a kullanarak iTunes App Store teslim için uygun veya kayıtlı bir geliştirici tarafından imzalanmış olan veya geçici uygulamaları).

Tüm `NSDocument` veya `UIDocument` tabanlı uygulamalar otomatik olarak yerleşik destek ve iletim desteklemek üzere küçük değişiklikler gerektiren iletimi sahip.

### <a name="continuing-user-activities"></a>Devam ediliyor kullanıcı etkinlikleri

`NSUserActivity` Sınıfı (bazı küçük değişiklikler birlikte `UIKit` ve `AppKit`) büyük olasılıkla devam bir kullanıcının etkinliğini başka kullanıcının aygıtları tanımlamak için destek sağlar.

Bir etkinliği başka kullanıcının aygıtları iletilecek onu bir örneğinde kapsüllenmiş gerekir `NSUserActivity`olarak işaretli _geçerli etkinliği_, buna ait (devamlılık gerçekleştirmek için kullanılan veri) yükü kümesine sahip ve Etkinlik sonra bu cihaza iletilmelidir.

İletim, iCloud eşitlenen büyük veri paketleri ile devam etkinlik tanımlama bilgilerinin tam minimum geçirir.

Alıcı cihazda kullanıcı bir etkinlik için devamlılık kullanılabilir olduğuna dair bir bildirim alırsınız. Kullanıcı etkinliği yeni cihazda devam etmek seçerse (henüz çalıştırılıyorsa) belirtilen uygulama başlatıldıktan ve yükü `NSUserActivity` etkinlik yeniden başlatmak için kullanılır.

[![](handoff-images/handoffinteractions.png "Devam ediliyor kullanıcı etkinlikleri genel bakış")](handoff-images/handoffinteractions.png#lightbox)

Takım Kimliği aynı Geliştirici paylaşan ve yanıt uygulamaları bir verilen _etkinlik türü_ devamlılık için uygundur. Bir uygulama altında destekliyorsa etkinlik türlerini tanımlar `NSUserActivityTypes` , anahtar kendi **Info.plist** dosya. Bu verildiğinde, devam ediliyor cihaz etkinlik türü takım kimliği temel alarak devamlılık gerçekleştirmek için app seçer ve isteğe bağlı olarak _etkinlik başlığı_.

Bilgileri alıcı uygulamanın kullandığı `NSUserActivity`'s `UserInfo` kendi kullanıcı arabirimini yapılandırmak ve geçiş son kullanıcı için sorunsuz görünmesi belirli etkinlik durumunu geri yüklemek için Sözlük.

Devamlılık verimli bir şekilde üzerinden gönderilebilir daha fazla bilgi gerektiriyorsa bir `NSUserActivity`, uygulama sürdürme kaynak uygulama için bir çağrı gönderebilir ve gerekli verileri aktarmak için bir veya daha fazla akışlar oluşturmak. Etkinlik birden fazla görüntü ile büyük metin belgesi düzenleme, örneğin, akış etkinliği alıcı cihazda devam etmek için gerekli bilgileri aktarmak için gerekli olacak. Daha fazla bilgi için bkz: [destekleyen devamlılık akışları](#Supporting-Continuation-Streams) bölümüne bakın.

Yukarıda belirtildiği gibi `NSDocument` veya `UIDocument` tabanlı uygulamalar otomatik olarak iletimi yerleşik destek bulunur. Daha fazla bilgi için bkz: [destekleyen iletimi belge tabanlı uygulamalarda](#Supporting-Handoff-in-Document-Based-Apps) bölümüne bakın.

### <a name="the-nsuseractivity-class"></a>NSUserActivity sınıfı

`NSUserActivity` Sınıfı iletimi exchange birincil nesne ve devamlılık için kullanılabilir olan bir kullanıcı etkinliği durumunu kapsüllemek için kullanılır. Bir uygulama bir kopyasını örneği oluşturulmaz `NSUserActivity` destekler ve devam etmek başka bir cihazda istediği herhangi bir etkinlik için. Örneğin, belge Düzenleyicisi her belge için bir etkinlik açık oluşturursunuz. Ancak, (öndeki penceresi veya sekmesinde görüntülenir) yalnızca öndeki belgedir _geçerli etkinliği_ ve devamlılık therefor kullanılabilir.

Örneği `NSUserActivity` her ikisi için de tanımlanan kendi `ActivityType` ve `Title` özellikleri. `UserInfo` Sözlük özelliği, etkinlik durumu hakkında bilgi taşımak için kullanılır. Ayarlama `NeedsSave` özelliğine `true` çok yavaş istiyorsanız, durumu bilgisini yük `NSUserActivity`kullanıcının temsilci. Kullanım `AddUserInfoEntries` diğer istemcilerden gelen yeni verileri birleştirmek için yöntemi `UserInfo` etkinliğin durumunu korumak için gerektiği şekilde sözlük.

### <a name="the-nsuseractivitydelegate-class"></a>NSUserActivityDelegate sınıfı

`NSUserActivityDelegate` Bilgilerini tutmak için kullanılan bir `NSUserActivity`'s `UserInfo` sözlük güncel ve etkinliğin geçerli durumu ile eşitlenmiş. Sistem bilgileri güncelleştirilmesi etkinliğinde gerektiği zaman (gibi başka bir aygıtta devamlılık önce), çağırır `UserActivityWillSave` temsilci yöntemi.

Uygulamanız gereken `UserActivityWillSave` yöntemi ve tüm değişiklikleri yapma `NSUserActivity` (gibi `UserInfo`, `Title`, vb.) geçerli etkinlik durumunu gösterdiğine emin olmak için. Sistem çağırdığında `UserActivityWillSave` yöntemi, `NeedsSave` bayrağı temizlendi. Etkinliğin veri özelliklerinden herhangi birini değiştirirseniz, ayarlamanız gerekir `NeedsSave` için `true` yeniden.

Yerine `UserActivityWillSave` yöntemi, yukarıda sunulan isteğe bağlı olarak sağlayabilirsiniz `UIKit` veya `AppKit` kullanıcı etkinliği otomatik olarak yönetir. Bunu yapmak için Yanıtlayıcı nesnenin ayarlayın `UserActivity` özelliği ve uygulama `UpdateUserActivityState` yöntemi. Bkz: [destekleyen iletimi yanıtlayıcılarını içinde](#Supporting-Handoff-in-Responders) daha fazla bilgi için bölüm aşağıda.

### <a name="app-framework-support"></a>Uygulama Framework desteği

Her ikisi de `UIKit` (iOS) ve `AppKit` (OS X) içinde iletimi için yerleşik destek sağlar `NSDocument`, Yanıtlayıcı (`UIResponder`/`NSResponder`), ve `AppDelegate` sınıfları. Her işletim sistemi iletimi biraz daha farklı uygulayan karşın, temel mekanizmasını ve API'leri aynıdır.

#### <a name="user-activities-in-document-based-apps"></a>Belge tabanlı uygulamalarda kullanıcı etkinlikleri

Belge tabanlı iOS ve OS X uygulamalarını otomatik olarak yerleşik iletimi desteğine sahip. Bu destek etkinleştirmek için eklemeniz gerekir bir `NSUbiquitousDocumentUserActivityType` anahtar ve değer her `CFBundleDocumentTypes` giriş uygulamasının **Info.plist** dosya.

Bu anahtarı varsa, her ikisi de `NSDocument` ve `UIDocument` otomatik olarak oluştur `NSUserActivity` örnekleri iCloud tabanlı belgeler için belirtilen türde. Her uygulamanın destekleyip belge türü için bir etkinlik türü sağlamanız gerekir ve birden çok belge türü aynı etkinlik türü kullanabilirsiniz. Her ikisi de `NSDocument` ve `UIDocument` otomatik olarak doldurulması `UserInfo` özelliği `NSUserActivity` ile bunların `FileURL` özelliğin değeri.

OS x `NSUserActivity` tarafından yönetilen `AppKit` ve otomatik olarak yanıtlayıcılarını ile ilişkili geçerli etkinlik belgenin penceresi ana penceresinde olduğunda olur. İOS, için `NSUserActivity` tarafından yönetilen nesneleri `UIKit`, ya da çağrı gerekir `BecomeCurrent` yöntemi açıkça veya belgenin `UserActivity` özelliği üzerinde ayarlanmış bir `UIViewController` uygulama ön plana ne zaman birlikte gelir.

`AppKit` herhangi bir otomatik olarak geri `UserActivity` OS x bu yolla oluşturulan özelliği. Bu durum ortaya çıkar `ContinueUserActivity` yöntemi döndürür `false` veya uygulanmayan ise. Bu durumda, belge açılmış `OpenDocument` yöntemi `NSDocumentController` ve ardından alacak bir `RestoreUserActivityState` yöntem çağrısı.

Bkz: [destekleyen iletimi belge tabanlı uygulamalarda](#Supporting-Handoff-in-Document-Based-Apps) daha fazla bilgi için bölüm aşağıda.

#### <a name="user-activities-and-responders"></a>Kullanıcı etkinlikleri ve yanıtlayıcılarını

Her ikisi de `UIKit` ve `AppKit` bir Yanıtlayıcı nesnesinin ayarlarsanız, bir kullanıcı etkinliği otomatik olarak yönetebilirsiniz `UserActivity` özelliği. Durum değiştirilirse ayarlamanız gerekir `NeedsSave` Yanıtlayıcı'nın özelliğinin `UserActivity` için `true`. Sistem otomatik olarak kaydedecek `UserActivity` çağırarak durumunu güncelleştirmek için Yanıtlayıcı zaman verdikten sonra gerekli olduğunda, `UpdateUserActivityState` yöntemi.

Birden çok yanıtlayıcılarını tek bir paylaşıyorsanız `NSUserActivity` aldıkları örneği, bir `UpdateUserActivityState` sistem kullanıcı etkinliği nesnesi güncelleştirildiğinde geri çağırma. Yanıtlayıcı çağırmayı gerektiren `AddUserInfoEntries` güncelleştirmek için yöntemi `NSUserActivity`'s `UserInfo` geçerli etkinlik durumu bu noktada yansıtacak şekilde sözlük. `UserInfo` Sözlük her önce temizlendiğinde `UpdateUserActivityState` çağırın.

Kendisini bir etkinlikten ilişkisini kaldırmak için bir Yanıtlayıcı ayarlayabilirsiniz kendi `UserActivity` özelliğine `null`. Ne zaman bir uygulama framework yönetilen `NSUserActivity` örneği sahipse daha ilişkili yanıtlayıcılarını ya da belgeler, otomatik olarak geçersiz kılınan olur.

Bkz: [destekleyen iletimi yanıtlayıcılarını içinde](#Supporting-Handoff-in-Responders) daha fazla bilgi için bölüm aşağıda.

#### <a name="user-activities-and-the-appdelegate"></a>Kullanıcı etkinlikleri ve AppDelegate

Uygulamanızın `AppDelegate` iletimi devamlılık işlerken kendi birincil giriş noktasıdır. Ne zaman kullanıcı yanıt iletimi bildirim için uygun uygulama başlatıldığında (zaten çalışmıyorsa) ve `WillContinueUserActivityWithType` yöntemi `AppDelegate` çağrılır. Bu noktada, uygulama devamlılık başlangıç kullanıcı bildirin.

`NSUserActivity` Örneği teslim `AppDelegate`'s `ContinueUserActivity` yöntemi çağrılır. Bu noktada, uygulamanın kullanıcı arabirimini yapılandırmak ve belirli etkinlik devam gerekir.

Bkz: [uygulama iletimi](#Implementing-Handoff) daha fazla bilgi için bölüm aşağıda.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Xamarin uygulaması iletimi etkinleştirme

İletim tarafından uygulanan güvenlik gereksinimleri olduğundan, iletim framework kullanan bir Xamarin.iOS uygulaması düzgün hem Apple Geliştirici Portalı ve Xamarin.iOS proje dosyası yapılandırılması gerekir.

Aşağıdakileri yapın:

1. İçine oturum [Apple Geliştirici Portalı](http://developer.apple.com).
2. Tıklayın **tanımlayıcıları & profilleri, sertifikaları**.
3. Zaten yapmadıysanız, tıklayın **tanımlayıcıları** ve uygulamanız için bir kimlik oluşturun (örneğin `com.company.appname`), mevcut kimliğinizi başka Düzenle
4. Emin **iCloud** hizmet verilen kimlik için kontrol edildiyse: 

    [![](handoff-images/provision01.png "Verilen kimlik için iCloud hizmetini etkinleştirme")](handoff-images/provision01.png#lightbox)
5. Değişikliklerinizi kaydedin.
4. Tıklayın **sağlama profilleri** > **geliştirme** ve sağlama profili sizin için yeni bir yazılım geliştirme uygulama oluşturun: 

    [![](handoff-images/provision02.png "Sağlama profili uygulama için yeni bir yazılım geliştirme oluşturma")](handoff-images/provision02.png#lightbox)
5. Karşıdan yükle ve yeni sağlama profili yüklemek veya Xcode profili karşıdan yükleyip kullanabilirsiniz.
6. Xamarin.iOS projesi seçeneklerinizi düzenleyin ve yeni oluşturduğunuz sağlama profili kullandığınızdan emin olun: 

    [![](handoff-images/provision03.png "Yeni oluşturduğunuz sağlama profili seçin")](handoff-images/provision03.png#lightbox)
7. Ardından, düzenleme, **Info.plist** dosya ve sağlama profili oluşturmak için kullanılan uygulama kimliği kullandığından emin olun: 

    [![](handoff-images/provision04.png "Uygulama Kimliği ayarlayın")](handoff-images/provision04.png#lightbox)
8. Kaydırma **arka plan modları** bölümünde ve aşağıdakileri denetleyin: 

    [![](handoff-images/provision05.png "Gerekli arka plan modlarını etkinleştir")](handoff-images/provision05.png#lightbox)
9. Tüm dosyalara değişiklikleri kaydedin.

Yerinde bu ayarlarla uygulama Handoff'a Framework API'lerine erişmek hazır. Hazırlama hakkında ayrıntılı bilgi için lütfen bkz bizim [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) ve [uygulamanızı sağlama](~/ios/get-started/installation/device-provisioning/index.md) kılavuzları.

## <a name="implementing-handoff"></a>İletim uygulama

Kullanıcı etkinlikleri aynı Geliştirici takım kimliği ile imzalanmış ve aynı etkinlik türü destekleyen uygulamalar arasında devam. Bir Xamarin.iOS uygulaması iletimi uygulama, gerektirir, bir kullanıcı etkinliği nesnesi oluşturmak (ya da `UIKit` veya `AppKit`) etkinliğini izlemek için nesnenin durumunu güncelleştirin ve etkinlik bir alıcı cihazda devam etmeden.

### <a name="identifying-user-activities"></a>Kullanıcı etkinlikleri tanımlama

İletim uygulama ilk adımı uygulamanız destekleyen kullanıcı etkinlik türlerini tanımlamaktır ve bu etkinlikler hangisinin görme başka bir aygıtta devamlılık için iyi bir aday değildir. Örneğin: bir Yapılacaklar uygulamasının düzenleme öğeleri biri olarak destekleyebilir _kullanıcı etkinlik türü_ve kullanılabilir öğeler listesi olarak başka bir tarama destekler.

Bir uygulama sayıda kullanıcı etkinliği, gerektiğinde türleri uygulama sağlayan herhangi bir işlev için bir tane oluşturabilirsiniz. Her kullanıcı etkinlik türü için bir etkinlik türünün başlar ve biter ve görev başka bir cihazda devam etmek için güncel durum bilgilerini saklamak üzere gerektiğinde izlemek uygulama gerekir.

Kullanıcı etkinlikleri gönderme ve alma uygulamaları arasında bire bir eşleme olmadan aynı takım kimlikli imzalı uygulamayı devam edebilir. Örneğin, belirli bir uygulamanın başka bir aygıtta farklı, tek tek uygulamalar tarafından tüketilen etkinlikleri, dört farklı türde oluşturabilirsiniz. Her uygulama daha küçük ve belirli bir görevi odaklanmıştır olduğu (birçok özellik ve işlevleri olabilir) uygulamayı Mac sürümü ve iOS uygulamaları arasında ortak bir oluşumu budur.

### <a name="creating-activity-type-identifiers"></a>Etkinlik türü tanımlayıcıları oluşturma

_Etkinlik türü tanımlayıcısı_ kısa bir dize eklenir `NSUserActivityTypes` uygulamanın dizisi **Info.plist** belirli bir kullanıcı etkinliği türde benzersiz şekilde tanımlamak için kullanılan dosya. Dizi uygulama destekleyen her etkinlik için bir giriş olacaktır. Apple çakışmaları önlemek için geriye doğru DNS stili gösterimi etkinlik türü tanımlayıcısı için kullanılmasını önerir. Örneğin: `com.company-name.appname.activity` için belirli uygulama tabanlı etkinlikler veya `com.company-name.activity` birden çok uygulama arasında çalıştırabilirsiniz etkinlikler için.

Etkinlik türü tanımlayıcısı oluşturulurken kullanılan bir `NSUserActivity` etkinlik türünü tanımlamak için örnek. Bir etkinliği başka bir cihazda devam ettirildiğinde, etkinlik türü (birlikte uygulamanın takım kimliği) etkinlik devam etmek için başlatmak için hangi uygulama belirler.

Örnek olarak, biz adlı örnek bir uygulama oluşturmak için kalacaklarını **MonkeyBrowser** ([buradan indirin](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)). Bu uygulama, her bir web tarayıcısı görünümünde farklı bir URL açıkken dört sekme sunacaktır. Kullanıcılar uygulamayı çalıştıran farklı iOS cihazında herhangi bir sekme devam edin.

Bu davranış desteklemek için gerekli etkinlik türü tanımlayıcıları oluşturmak için düzenleyin **Info.plist** dosya ve geçiş **kaynak** görünümü. Ekleme bir `NSUserActivityTypes` anahtar ve aşağıdaki tanımlayıcılar oluşturun:

[![](handoff-images/type01.png "NSUserActivityTypes anahtarı ve plist düzenleyicisinde gerekli tanımlayıcıları")](handoff-images/type01.png#lightbox)

Dört yeni etkinlik türü tanımlayıcıları, her örnekte sekmelerinin oluşturduğumuz **MonkeyBrowser** uygulama. Kendi uygulamaları oluştururken Değiştir `NSUserActivityTypes` etkinlikleri belirli etkinlik türü tanımlayıcıları ile uygulamanızı dizi destekler.

### <a name="tracking-user-activity-changes"></a>Kullanıcı etkinliği değişiklikleri izleme

Yeni bir örneğini oluştururken biz `NSUserActivity` sınıfı, biz belirtin bir `NSUserActivityDelegate` etkinliğin durumuna değişiklikleri izlemek için örneği. Örneğin, izleme kodu durum değişiklikleri izlemek için kullanılabilir:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;

namespace MonkeyBrowse
{
    public class UserActivityDelegate : NSUserActivityDelegate
    {
        #region Constructors
        public UserActivityDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void UserActivityReceivedData (NSUserActivity userActivity, NSInputStream inputStream, NSOutputStream outputStream)
        {
            // Log
            Console.WriteLine ("User Activity Received Data: {0}", userActivity.Title);
        }

        public override void UserActivityWasContinued (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity Was Continued: {0}", userActivity.Title);
        }

        public override void UserActivityWillSave (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity will be Saved: {0}", userActivity.Title);
        }
        #endregion
    }
}
```

`UserActivityReceivedData` Yöntemi, bir devamlılık akış veri gönderen bir aygıttan aldığında çağrılır. Daha fazla bilgi için bkz: [destekleyen devamlılık akışları](#Supporting-Continuation-Streams) bölümüne bakın.

`UserActivityWasContinued` Yöntemi çağrılır başka bir aygıt, geçerli bir CİHAZDAN bir etkinlik üzerinden sürdü. Yapılacaklar listesi için yeni bir öğe ekleme gibi etkinliğinin türüne bağlı olarak uygulama gönderen aygıtın faaliyete durdurabilir.

`UserActivityWillSave` Yöntemi etkinlik değişiklikler kaydedilir ve yerel olarak kullanılabilir cihaz arasında eşitlenen önce çağrılır. Son dakika değişiklikleri yapmak için bu yöntemi kullanabilirsiniz `UserInfo` özelliği `NSUserActivity` gönderilmeden önce örneği.

### <a name="creating-a-nsuseractivity-instance"></a>NSUserActivity örneği oluşturma

İçinde başka bir cihazda etmeden olasılığını sağlamak için uygulamanızın istediği her etkinlik saklanmasını bir `NSUserActivity` örneği. Uygulama gerektiği gibi birçok etkinlik oluşturabilir ve bu etkinlikler yapısını işlevselliği ve söz konusu uygulamanın özelliklerini bağlıdır. Örneğin, bir e-posta uygulaması oluşturmayı yeni bir ileti ve başka bir ileti okumak için bir etkinlik oluşturabilirsiniz.

Bizim örnek uygulama için yeni bir `NSUserActivity` sekmeli web tarayıcı görünümü birinde yeni bir URL kullanıcının girdiği her zaman oluşturulur. Aşağıdaki kod, belirli bir sekme durumunu depolar:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSUserActivity UserActivity { get; set; }
...

UserActivity = new NSUserActivity (UserActivityTab1);
UserActivity.Title = "Weather Tab";
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Yeni bir oluşturur `NSUserActivity` kullanıcı etkinlik türü birini kullanarak yukarıda oluşturduğunuz ve etkinlik için okunabilir bir başlık sağlar. Örneğine ekler `NSUserActivityDelegate` durumu değiştirir ve iOS bu kullanıcı etkinliğini geçerli etkinlik olduğunu bildirir izlemek için yukarıda oluşturulan.

### <a name="populating-the-userinfo-dictionary"></a>Kullanıcı bilgisi sözlük doldurma

Yukarıda anlatıldığı gibi `UserInfo` özelliği `NSUserActivity` sınıfı bir `NSDictionary` belirli bir etkinlik durumu tanımlamak için kullanılan anahtar-değer çiftleri. İçinde depolanan değerleri `UserInfo` şu türlerden biri olmalıdır: `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`, veya `NSURL`. `NSURL` böylece bir alıcı aygıtta aynı belgeleri üzerine iCloud belgeler veri değerleri otomatik olarak ayarlanır.

Yukarıdaki örnekte, oluşturduğumuz bir `NSMutableDictionary` nesne ve verilen sekmesinde kullanıcı görüntülemekte URL sağlayan tek bir anahtar ile doldurulur. `AddUserInfoEntries` Etkinlik alıcı aygıt faaliyete geri yüklemek için kullanılan verileri güncelleştirmek için kullanılan kullanıcı etkinliği yöntemi:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple önermek etkinlik alıcı aygıta zamanında gönderilir emin olmak için barest minimum gönderilen bilgiler tutma. Daha büyük bilgi belgeye iliştirilmiş bir görüntü düzenlenmesi gibi gerekirse gönderilmesi kullanmanız gereken devamlılık akışları gerekiyor. Bkz: [destekleyen devamlılık akışları](#Supporting-Continuation-Streams) daha fazla ayrıntı için bölüm aşağıda.

### <a name="continuing-an-activity"></a>Bir etkinlik etmeden

İletim, yerel iOS ve kaynak cihaza fiziksel olarak yakın olan ve aynı iCloud hesabıyla devam ettirilebilir kullanıcı etkinlikleri kullanılabilirlik imzalanan OS X cihazları otomatik olarak size bildirir. Kullanıcı bir etkinlik üzerinde yeni bir aygıt devam etmek seçerse, sistem bilgi ve (Takım Kimliği ve etkinlik türü göre) uygun uygulama başlatacak kendi `AppDelegate` devamlılık gerçekleşmesi gerekir.

İlk olarak, `WillContinueUserActivityWithType` uygulama devamlılık hakkında başlatmak için kullanıcıya bilgilendirebilir şekilde yöntemi çağrılır. Aşağıdaki kodda kullanırız **AppDelegate.cs** devamlılık başlangıç işlemek için bizim örnek uygulamasının dosyası:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSString UserActivityTab2 = new NSString ("com.xamarin.monkeybrowser.tab2");
public NSString UserActivityTab3 = new NSString ("com.xamarin.monkeybrowser.tab3");
public NSString UserActivityTab4 = new NSString ("com.xamarin.monkeybrowser.tab4");
...

public FirstViewController Tab1 { get; set; }
public SecondViewController Tab2 { get; set;}
public ThirdViewController Tab3 { get; set; }
public FourthViewController Tab4 { get; set; }
...

public override bool WillContinueUserActivity (UIApplication application, string userActivityType)
{
    // Report Activity
    Console.WriteLine ("Will Continue Activity: {0}", userActivityType);

    // Take action based on the user activity type
    switch (userActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Inform view that it's going to be modified
        Tab1.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Inform view that it's going to be modified
        Tab2.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Inform view that it's going to be modified
        Tab3.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Inform view that it's going to be modified
        Tab4.PreparingToHandoff ();
        break;
    }

    // Inform system we handled this
    return true;
}
```

Yukarıdaki örnekte, her görünüm denetleyicisi ile kaydeder. `AppDelegate` ve ortak bir `PreparingToHandoff` bir etkinliği göstergesini ve etkinliği hakkında kapalı geçerli aygıta verilmesini olduğunu biliyor kullanıcı bildiren bir ileti görüntüler yöntemi. Örnek:

```csharp
private void ShowBusy(string reason) {

    // Display reason
    BusyText.Text = reason;

    //Define Animation
    UIView.BeginAnimations("Show");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0.5f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PreparingToHandoff() {
    // Inform caller
    ShowBusy ("Continuing Activity...");
}
```

`ContinueUserActivity` , `AppDelegate` Gerçekten belirli etkinlik devam etmek için çağrılır. Yeniden bizim örnek uygulamadan:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Ortak `PerformHandoff` yöntemi her görünüm denetleyicisinin gerçekten iletimi preforms ve geçerli aygıtı faaliyete geri yükler. Örnek söz konusu olduğunda, bu kullanıcı farklı bir aygıtta gözatma belirli bir sekmesinde aynı URL'yi görüntüler. Örnek:

```csharp
private void HideBusy() {

    //Define Animation
    UIView.BeginAnimations("Hide");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PerformHandoff(NSUserActivity activity) {

    // Hide busy indicator
    HideBusy ();

    // Extract URL from dictionary
    var url = activity.UserInfo ["Url"].ToString ();

    // Display value
    URL.Text = url;

    // Display the give webpage
    WebView.LoadRequest(new NSUrlRequest(NSUrl.FromString(url)));

    // Save activity
    UserActivity = activity;
    UserActivity.BecomeCurrent ();

}
```

`ContinueUserActivity` Yöntemi içeren bir `UIApplicationRestorationHandler` belge veya Yanıtlayıcı için çağırabilirsiniz göre etkinlik sürdürülüyor. Geçirmeniz gereken bir `NSArray` veya geri yükleme çağrıldığında işleyicisine geri yüklenebilen nesneleri. Örneğin:

```csharp
completionHandler (new NSObject[]{Tab4});
```

Geçirilen, her nesne için kendi `RestoreUserActivityState` yöntemi çağrılır. Her nesne sonra verileri kullanabilirsiniz `UserInfo` kendi durumunu geri yüklemek için Sözlük. Örneğin:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Uygulamaz, belge tabanlı uygulamalar için `ContinueUserActivity` yöntemi veya verir `false`, `UIKit` veya `AppKit` etkinlik otomatik olarak devam edebilirsiniz. Bkz: [destekleyen iletimi belge tabanlı uygulamalarda](#Supporting-Handoff-in-Document-Based-Apps) daha fazla bilgi için bölüm aşağıda.

### <a name="failing-handoff-gracefully"></a>İletim düzgün biçimde başarısız

Aktarım işlemi geniş bağlı koleksiyonuna iOS ve OS X cihazlar arasında bilgi iletimi iletimi dayanan olduğundan, bazen başarısız olabilir. Bu hatalar işleyebilmesini ve ortaya çıkan durumlara kullanıcıyı bilgilendirmek için uygulamanızı tasarlamanız gerekir.

Bir arıza olması durumunda `DidFailToContinueUserActivitiy` yöntemi `AppDelegate` çağrılır. Örneğin:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Sağlanan kullanması gereken `NSError` kullanıcı hata hakkında bilgi sağlamak üzere.

## <a name="native-app-to-web-browser-handoff"></a>Web tarayıcısı iletimi için yerel uygulama

Bir kullanıcı, istediğiniz cihazda yüklü uygun bir yerel uygulama gerek kalmadan bir etkinlik devam etmek isteyebilirsiniz. Bazı durumlarda, bir web tabanlı arabirim gerekli işlevselliği sağlayabilir ve etkinlik hala devam eder. Örneğin, kullanıcının e-posta hesabı oluşturma ve iletileri okumak için bir web tabanlı kullanıcı Arabirimi sağlayabilir.

Kaynak, yerel uygulama URL'sini biliyorsa web Arabirimi'ni (ve devam verilen öğe tanımlamak için gerekli söz dizimi) için bu bilgileri kodlayabilir `WebpageURL` özelliği `NSUserActivity` örneği. Alıcı aygıt devamlılık işlemek için yüklü bir uygun yerel uygulama yoksa, sağlanan web arabirimi çağrılabilir.

## <a name="web-browser-to-native-app-handoff"></a>Yerel uygulama iletimi Web tarayıcısı

Kullanıcı web tabanlı bir arabirim kaynak aygıtı tarafından kullanılan ve etki alanı kısmı alıcı cihazda yerel bir uygulama talep `WebpageURL` özelliği sonra sistem bu uygulama tanıtıcısı devamlılık kullanır. Yeni cihaz alacak bir `NSUserActivity` etkinlik türü olarak işaretler örneği `BrowsingWeb` ve `WebpageURL` ziyaret eden kullanıcı, URL içerecek `UserInfo` sözlüğü boş olacaktır.

Bir uygulamayı bu iletimi türünde katılmak için etki alanında talep gerekir bir `com.apple.developer.associated-domains` biçiminde yetkilendirme `<service>:<fully qualified domain name>` (örneğin: `activity continuation:company.com`).

Belirtilen etki alanı eşleşmesi durumunda bir `WebpageURL` özelliğin değeri, iletim o etki alanındaki Web sitesinden onaylanan uygulama kimlikleri listesini indirir. Web sitesi adlı imzalı bir JSON dosyası onaylanan kimlikleri listesi sağlamalısınız **apple app site ilişkilendirmesi** (örneğin, `https://company.com/apple-app-site-association`).

Bu JSON dosyası biçiminde uygulama kimlikleri listesini belirtir bir sözlük içeren `<team identifier>.<bundle identifier>`. Örneğin:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

JSON dosyasını imzalamak için (doğru sahip olması `Content-Type` , `application/pkcs7-mime`), kullanın **Terminal** uygulama ve `openssl` bir sertifika ve anahtar iOS tarafından güvenilen bir sertifika yetkilisi tarafından verilen komutunu (bkz: [ http://support.apple.com/kb/ht5012 ](http://support.apple.com/kb/ht5012) bir listesi için). Örneğin:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

`openssl` Komutu çıkarır, Web sitesine Yerleştir imzalı bir JSON dosyası **apple app site ilişkilendirmesi** URL. Örneğin:

```csharp
https://example.com/apple-app-site-association.
```

Uygulama herhangi bir etkinlik alacak olan `WebpageURL` etki alanı kendi `com.apple.developer.associated-domains` yetkilendirme. Yalnızca `http` ve `https` kurallarıdır desteği, herhangi bir protokol bir özel durum oluşturacak.

## <a name="supporting-handoff-in-document-based-apps"></a>Belge tabanlı uygulamalarda iletimi destekleme

İOS ve OS X, yukarıda belirtildiği gibi belge tabanlı uygulamaları otomatik olarak iCloud bağlı olarak belgelerin iletimi varsa destekleyecek uygulamanın **Info.plist** dosyasını içeren bir `CFBundleDocumentTypes` , anahtar `NSUbiquitousDocumentUserActivityType`. Örneğin:

```xml
<key>CFBundleDocumentTypes</key>
<array>
    <dict>
        <key>CFBundleTypeName</key>
        <string>NSRTFDPboardType</string>
        . . .
        <key>LSItemContentTypes</key>
        <array>
        <string>com.myCompany.rtfd</string>
        </array>
        . . .
        <key>NSUbiquitousDocumentUserActivityType</key>
        <string>com.myCompany.myEditor.editing</string>
    </dict>
</array>
```

Bu örnekte, dize eklenmiş etkinlik adı bir ters DNS uygulama göstergesidir. Bu şekilde girdiyseniz, etkinlik türü girişleri, yinelenen gerekmez `NSUserActivityTypes` dizisi **Info.plist** dosya.

Otomatik olarak oluşturulan kullanıcı etkinliği nesnesi (belgenin aracılığıyla kullanılabilen `UserActivity` özelliği) uygulamasında diğer nesneler tarafından başvurulan ve devamlılık durumunu geri yüklemek için kullanılır. Örneğin, izlemek için öğe seçimi ve belge yerleştirin. Bu etkinlikler ayarlamak gereken `NeedsSave` özelliğine `true` her durum değiştirir ve güncelleştirme `UserInfo` sözlükte `UpdateUserActivityState` yöntemi.

`UserActivity` Özelliği iCloud içeri ve dışarı taşır gibi bir belge eşitlenmiş tutmak için kullanılabilmesi için anahtar-değer gözlemci (KVO) protokolü için uyumlu ve herhangi bir iş parçacığından kullanılabilir. `UserActivity` Özelliği geçersiz kılınan belge kapalı olduğunda.

Daha fazla bilgi için lütfen Apple'nın bkz [kullanıcı etkinliği desteği belge tabanlı uygulamalarda](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) belgeleri.

## <a name="supporting-handoff-in-responders"></a>İçinde yanıtlayıcılarını iletimi destekleme

Yanıtlayıcı ilişkilendirebilirsiniz (araçtan devralınan `UIResponder` iOS veya `NSResponder` OS x) ayarlayarak etkinlikler için kendi `UserActivity` özellikleri. Sistem otomatik olarak kaydeder `UserActivity` özelliğine uygun zamanları Yanıtlayıcı'nın çağırma `UpdateUserActivityState` kullanıcı etkinliğini kullanarak nesne geçerli veri ekleme yöntemi `AddUserInfoEntriesFromDictionary` yöntemi.

## <a name="supporting-continuation-streams"></a>Devamlılık akışları destekleme

Burada bir etkinlik devam etmek için gerekli bilgi miktarını verimli bir şekilde aktarılamaz tarafından ilk iletim yükü durumlar olabilir. Bu durumlarda, alıcı uygulamanın kendisi verileri aktarmak için kaynak uygulama arasında bir veya daha fazla akış kurabilirsiniz.

Kaynak uygulama ayarlayacaktır `SupportsContinuationStreams` özelliği `NSUserActivity` için örnek `true`. Örneğin:

```csharp
// Create a new user Activity to support this tab
UserActivity = new NSUserActivity (ThisApp.UserActivityTab1){
    Title = "Weather Tab",
    SupportsContinuationStreams = true
};
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Alıcı uygulamanın ardından çağırabilirsiniz `GetContinuationStreams` yöntemi `NSUserActivity` içinde kendi `AppDelegate` akış kurmak için. Örneğin:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Kaynak aygıtta çağırarak kullanıcı etkinliği temsilci akışları alır, `DidReceiveInputStream` veri sağlamak için yöntemini istenen kullanıcı etkinliği devam ettirme cihazda devam etmek için.

Kullanacağınız bir `NSInputStream` veri akışı, salt okunur erişim sağlamak için ve bir `NSOutputStream` yalnızca yazma erişimi sağlar. Akışlar burada alıcı uygulamanın daha fazla veri istekleri ve kaynak uygulamanın sağladığı bir istek ve yanıt şekilde, kullanılmalıdır. Kaynak cihazda çıkış akışına yazılan veriler devam ediliyor aygıttaki giriş akışından okuma böylece tersi.

Devamlılık akış nerede gerekli durumlarda bile olmamalıdır en az bir geri ve İleri iki uygulama arasındaki iletişim.

Daha fazla bilgi için Apple'nın bkz [kullanma devamlılık akışları](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) belgeleri.

## <a name="handoff-best-practices"></a>İletim en iyi uygulamalar

Başarılı, bir kullanıcı etkinliği iletimi aracılığıyla sorunsuz devamlılık dikkatli tasarım tüm çeşitli bileşenleri nedeniyle söz konusu gerektirir. Apple iletimi etkinleştirilmiş uygulamalarınız için izleme en iyi yöntemlerini kullanma önerir:

- Kullanıcı etkinliklerinizi ettirilecek etkinlik durumunu ilişkili olası en küçük yükü gerektirecek şekilde tasarlayın. Daha büyük yük, o kadar uzun başlatmak için devamlılık alır.
- Büyük miktarlarda veri için başarılı devamlılık transfer ederseniz, yapılandırma ve Ağ Yükü maliyetleri dahil dikkate alın.
- Birkaç tarafından daha küçük, görev özel uygulamalar iOS cihazlarda işlenen kullanıcı etkinlikleri oluşturmak büyük bir Mac uygulaması için yaygın bir durumdur. Farklı uygulama ve işletim sistemi sürümleri de birlikte çalışır veya düzgün biçimde başarısız tasarlanmalıdır.
- Etkinlik türleri belirtilirken, çakışmaları önlemek için ters DNS gösterimini kullanın. Bir etkinlik için belirli bir uygulamanın özgüyse adını tür tanımında eklenmesi gereken (örneğin `com.myCompany.myEditor.editing`). Etkinlik birden çok uygulama arasında çalışıyorsanız tanımından uygulama adı bırakın (örneğin `com.myCompany.editing`).
- Uygulamanızı bir kullanıcı etkinliği durumunu güncelleştirmek gerekip gerekmediğini (`NSUserActivity`) ayarlamak `NeedsSave` özelliğine `true`. Uygun zamanlarda iletimi temsilcinin çağıracak `UserActivityWillSave` güncelleştirebilmek yöntemi `UserInfo` gerektiği gibi sözlük.
- İletim işlemi hemen alıcı cihazda değil çünkü başlatamadı, uygulamalıdır `AppDelegate`'s `WillContinueUserActivity` ve bir devamlılık hakkında başlatmak için kullanıcıya bildirin.

## <a name="example-handoff-app"></a>Örnek iletimi uygulama

İletim bir Xamarin.iOS uygulaması kullanarak örnek olarak, biz dahil ettiğiniz [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/) bu kılavuzu ile örnek uygulama. Uygulama kullanıcı web her belirli etkinlik türü ile göz atmak için kullanabileceğiniz dört sekme bulunur: hava durumu, sık kullanılan, kahve sonu ve iş.

Kullanıcı yeni bir URL ve dokunma girdiğinde bir sekmede **Git** düğmesi, yeni bir `NSUserActivity` kullanıcı şu anda gözatma URL'si içeren sekmenin için oluşturulur:

[![](handoff-images/handoff01.png "Örnek iletimi uygulama")](handoff-images/handoff01.png#lightbox)

Başka bir kullanıcının aygıtları varsa **MonkeyBrowser** uygulamasının yüklü, aynı kullanıcı hesabı kullanarak iCloud imzalı, aynı ağ ve yakın bir yerde konumlandırıldığında içinde yukarıdaki cihaz, giriş animasyonuna etkinlik görüntülenecek ekranında (sol alt köşesindeki):

[![](handoff-images/handoff02.png "Sol alt köşesindeki giriş ekranında görüntülenen iletimi etkinliği")](handoff-images/handoff02.png#lightbox)

Kullanıcının yukarı iletimi simgesine sürüklediği, uygulama başlatılır ve kullanıcı etkinliğini belirtilen `NSUserActivity` yeni cihazda devam:

[![](handoff-images/handoff03.png "Yeni cihaz üzerinde kullanıcı etkinliği devam")](handoff-images/handoff03.png#lightbox)

Ne zaman bir kullanıcı etkinliği başarıyla gönderildikten başka bir Apple aygıtı için gönderen kişinin `NSUserActivity` yapılan bir çağrı alacaksınız `UserActivityWasContinued` yöntemi kendi `NSUserActivityDelegate` , kullanıcı etkinliği başarıyla diğerine aktarıldı olduğunu bilmeniz izin vermek için aygıt.

## <a name="summary"></a>Özet

Bu makalede, bir kullanıcı etkinliği birden çok kullanıcının Apple aygıt arasında devam etmek için kullanılan iletimi framework giriş verdiği. Ardından, etkinleştirmek ve bir Xamarin.iOS uygulaması iletimi uygulamak nasıl oluşturulacağını gösterir. Son olarak, farklı türdeki iletimi devamlılıklar kullanılabilir ve iletim en iyi yöntemler açıklanmıştır.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [MonkeyBrowser örnek](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [Yeni iOS 9.0 nedir](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper Kılavuzu](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit kullanıcı arabirimi yönergeleri](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Framework başvurusu](https://developer.apple.com/library/ios/home_kit_framework_ref)
