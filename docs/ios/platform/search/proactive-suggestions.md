---
title: "Öngörülü önerileri giriş"
description: "Bu makalede öngörülü önerileri Xamarin.iOS uygulaması sürücü katılım için proaktif olarak yararlı bilgiler kullanıcıya otomatik olarak sunmak üzere sistem vererek nasıl kullanılacağı gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: e7252aa89e2514653fc730c7221d22cc053d2e24
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-proactive-suggestions"></a>Öngörülü önerileri giriş

_Bu makalede öngörülü önerileri Xamarin.iOS uygulaması sürücü katılım için proaktif olarak yararlı bilgiler kullanıcıya otomatik olarak sunmak üzere sistem vererek nasıl kullanılacağı gösterilmektedir._

Yeni iOS 10, öngörülü önerileri mevcut haber yolları otomatik olarak kullanıcı için önceden mevcut yararlı bilgiler uygun zamanlarda tarafından bir Xamarin.iOS uygulaması ile etkileşim kullanıcılar için.

iOS 10 proaktif olarak yararlı bilgiler otomatik olarak kullanıcı için uygun zamanlarda sunmak sistem izin vererek uygulama yönlendirmeli katılım yeni yolları gösterir. Yalnızca, iOS 9 derin arama Spotlight, iletim ve Siri önerileri kullanarak uygulama ekleme yeteneği sağlanan (bkz [yeni arama API'leri](~/ios/platform/search/index.md)), iOS 10 ile bir uygulama kullanıcıya sistem tarafından içinden sunulabilir işlevselliğini hale getirebilir Aşağıdaki konumlardan:

- Uygulama değiştirici
- Kilit ekranı
- CarPlay
- Eşlemeleri
- Siri etkileşimleri
- QuickType önerileri

Uygulama teknolojileri koleksiyonu gibi kullanarak sisteme bu işlevselliği kullanıma sunan `NSUserActivity`, web biçimlendirme, çekirdek Spotlight, MapKit, Media Player ve Uıkit. Ayrıca, uygulama için öngörülü öneri desteği sağlayarak, daha derin Siri tümleştirme ücretsiz alır.

## <a name="location-based-suggestions"></a>Konum tabanlı önerileri

10, iOS için yeni `NSUserActivity` sınıfı içeren bir `MapItem` diğer bağlamlarda kullanılabilir konum bilgileri sağlamak Geliştirici imkan tanıyan özellik. Örneğin, uygulama Restoran incelemeler gösterirse, geliştirici ayarlayabilirsiniz `MapItem` kullanıcı uygulamada görüntüleme Restoran konumunu özelliği. Kullanıcı Maps uygulamaya geçerse Restoran'ın konumu otomatik olarak kullanılabilir.

Uygulama uygulama arama destekliyorsa, yeni adres bileşenlerinin kullanabilirsiniz `CSSearchableItemAttributesSet` sınıfı kullanıcı ziyaret isteyebilir konumlarını belirtin. Ayarlayarak `MapItem` özelliği, diğer özellikleri otomatik olarak doldurulmuş içinde.

Ayar yanı sıra `Latitude` ve `Longitude` adresi bileşen özelliklerinden, uygulama sağlamanız önerilir `NamedLocation` ve `PhoneNumbers` özellikleri çok, bu nedenle Siri başlatabilir konumu için bir çağrı.

## <a name="web-markup-based-suggestions"></a>Web biçimlendirme tabanlı önerileri

iOS 9 Spotlight ve Safari arama sonuçlarında kullanıcıların gördüğü içerik aşağıdakilere zenginleştirir Web sitesinde yapılandırılmış verileri biçimlendirme dahil etme yeteneği eklendi (bkz [Web biçimlendirme arama](~/ios/platform/search/web-markup.md)). iOS 10 konum temelli biçimlendirme ekleme yeteneği ekler (gibi [PostalAddress](http://schema.org/PostalAddress) tarafından tanımlanan [Schema.org](http://schema.org/)) daha fazla kullanıcı deneyimini geliştirmek için. Örneğin, bir konum kullanıcılar görünümleri Web sayfasında işaretliyse eşlemeleri açtıklarında sistem aynı konuma önerebilir.

## <a name="text-based-suggestions"></a>Metin tabanlı önerileri

Uıkit iOS 10 içerecek şekilde genişletilmiştir [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) özelliği [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) metin alanında içeriği anlamsal anlamını belirlemek için sınıf. Yerinde bu bilgilerle sistem genellikle otomatik olarak uygun klavye türünü seçebilir, otomatik düzeltme önerileri geliştirmek ve önceden diğer uygulamalar ve Web siteleri bilgilerinden tümleştirin.

Örneğin, kullanıcı olarak işaretlenmiş bir metin alanına metin girerek `UITextContentType.FullStreetAddress`, sistem alan kullanıcı son görüntüleme konumla otomatik doldurma önerebilir.

## <a name="media-based-suggestions"></a>Medya tabanlı önerileri

Uygulama medya kullanarak yürütülürse [MPPlayableContentManager](https://developer.xamarin.com/api/type/MediaPlayer.MPPlayableContentManager/) API, iOS 10 kullanıcıların albüm resim görüntüleyin ve kilit ekranı üzerinde uygulama üzerinden medya yürütün.

## <a name="contextual-siri-reminders"></a>Bağlamsal Siri anımsatıcıları

Şu anda uygulamada daha sonraki bir tarihte görüntüledikleri içeriği görüntülemek için bir anımsatıcı hızlı bir şekilde sağlamak için Siri kullanın olanak tanır. Örneğin, bir restoran incelemesi uygulamada görüntülemekte olduğunuz varsa, bunlar Siri çağırma ve söyleyin *"home almak, bu konuda hatırlat."* Siri gözden bağlantı anımsatıcı uygulamada oluşturur.

## <a name="contact-based-suggestions"></a>Kişi tabanlı önerileri

Uygulamanın verir görünmesi kişiler (ve ilgili kişi bilgilerini) **başvurun** uygulamanız tüm kullanıcıların kişiler yanı sıra iOS cihazında depolanan sistemde.

## <a name="ride-sharing-based-suggestions"></a>Paylaşımı kılma tabanlı önerileri

Bir kılma paylaşımı uygulaması kullanıyorsa [MKDirectionsRequest](https://developer.xamarin.com/api/type/MapKit.MKDirectionsRequest/) API, iOS 10 düzenleyecek, uygulama Değiştirici bir seçenek olarak zamanlarda kullanıcı bir kılma istediğiniz olasılığı olduğunda. Uygulama aynı zamanda kılma paylaşımı uygulaması belirterek kaydedilmelidir `MKDirectionsModeRideShare` için [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) anahtarını kendi `Info.plist` dosya.

Uygulamanın yalnızca kılma paylaşımı destekliyorsa, sistem öneri ile başlayan *"bir kılma Get..."*, diğer türleri (örneğin, Walking veya bisiklet) yönlendirme yönü destekleniyorsa, sistemin kullanacağı *"yönlendirmeler Get..."*

> [!IMPORTANT]
> [MKMapItem](https://developer.xamarin.com/api/type/MapKit.MKMapItem/) uygulama alan nesnesi boylam ve enlem bilgi içermeyebilir ve coğrafi kodlama gerektirir.

## <a name="implementing-proactive-suggestions"></a>Öngörülü önerileri uygulama

Öngörülü öneri ekleme desteği bir Xamarin.iOS uygulaması için birkaç API'leri uygulamadan veya uygulaması zaten uygulama birkaç API genişletme genellikle kadar kolaydır.

Öngörülü önerileri uygulamalarla üç temel şekilde çalışır:

- **`NSUserActivity` ve Schema.org**  -  `NSUserActivity` sistemi kullanıcı şu anda çalıştığını ile ekranda hangi bilgilerin anlamanıza yardımcı olur. Schema.org benzer yeteneklerini web sayfalarına ekler.
- **Konum önerileri** - uygulama sunar veya bu bilgileri uygulama arasında paylaşmak için temel konum bilgileri, bu API uzantısı teklif yeni yolları kullanır.
- **Medya uygulama önerileri** - sistem uygulama yükseltebilirsiniz ve medya içeriği temel alarak kullanıcı etkileşimi iOS aygıtıyla bağlamı.

Ve uygulamada aşağıdakileri uygulayarak desteklenir:

- **İletim**  -  `NSUserActivity` bir aygıtta bir etkinlik başlayın, ardından başka devam etmek Geliştirici veren iletimi desteklemek için iOS 8 eklenmiştir (bkz [iletimi giriş](~/ios/platform/handoff.md)).
- **Sahne Işığı arama** -iOS 9 özelliği uygulama içeriğini kullanarak Spotlight arama sonuçlarını içinden yükseltmek için eklenen `NSUserActivity` (bkz [çekirdek Spotlight arama](~/ios/platform/search/corespotlight.md)).
- **Bağlamsal Siri anımsatıcıları** - iOS 10 `NSUserActivity` hızlı bir şekilde kullanıcı şu anda görüntüleme uygulamada daha sonraki bir tarihte içeriği görüntülemek için bir anımsatıcı yapmak Siri'ye izin verecek şekilde genişletilmiştir.
- **Konum önerileri** -iOS 10 geliştirir `NSUserActivity` uygulama içinde görüntülenen konumları yakalamak ve bunları Sistem genelindeki birçok yerde yükseltmek için.
- **Bağlamsal Siri istekleri**  -  `NSUserActivity` böylece kullanıcı uygulama içinde bildirilecekse Siri gelen yönergeleri veya bir çağrı olmalıdır yer alabilir içinde uygulama Siri tarafından sunulan bilgiler için bir bağlam sağlar.
- **İlgili kişi etkileşimleri** - iOS 10, yeni `NSUserActivity` iletişimleri uygulamalarının (kişiler uygulamasında) kişi kartından bir alternatif iletişim yöntemi olarak yükseltilmesi sağlar.

Bu özelliklerin tümü, ortak bir şey vardır, tüm kullandıkları `NSUserActivity` bir form veya başka işlevleri sağlamak için. 

## <a name="nsuseractivity"></a>NSUserActivity

Yukarıda belirtildiği gibi `NSUserActivity` sistemi kullanıcı şu anda çalıştığını ile ekranda hangi bilgilerin anlamanıza yardımcı olur. `NSUserActivity` Hafif durumu, kullanıcının etkinliğini üzerinden uygulama gittikleri olarak yakalamak için bir mekanizma önbelleğe alma. Örneğin, bir restoran uygulamaya aranıyor:

[![](proactive-suggestions-images/activity02.png "Önbelleğe alma mekanizması NSUserActivity hafif durumu")](proactive-suggestions-images/activity02.png#lightbox)

İle aşağıdaki etkileşimler:

1. Kullanıcının uygulamayla çalışırken bir `NSUserActivity` daha sonra uygulama durumunu yeniden oluşturmak için oluşturulur.
2. Kullanıcı için bir restoran arar, etkinlikleri oluşturma aynı düzeni izler.
3. Ve yeniden, kullanıcı bir sonuç zaman görüntüler. Son bu durumda, kullanıcı bir konum görüntüleme ve iOS 10'da, sistem daha belirli kavramları (örneğin, konum veya iletişim etkileşimleri) bilmektedir.

Son ekran daha yakın bir göz atın:

[![](proactive-suggestions-images/activity03.png "NSUserActivity ayrıntıları")](proactive-suggestions-images/activity03.png#lightbox)

Burada uygulama oluşturma bir `NSUserActivity` ve durumunu daha sonra yeniden bilgilerle doldurulur. Uygulama konumun ad ve adres gibi bazı meta veriler ayrıca eklemiştir. Oluşturulan bu etkinlikle uygulamanın, kullanıcının geçerli durumunu temsil eden bilmeniz iOS sağlar.

Uygulama sonra etkinlik iletimi için havadan tanıtılan, konum önerileri için geçici bir değer olarak kaydedilmiş veya kaldırılacak arama sonuçlarında görüntülemek için cihaz üzerindeki Spotlight dizine eklenen varsa karar verir.

İletim ve Spotlight arama ile ilgili daha fazla bilgi için lütfen bkz bizim [iletimi giriş](~/ios/platform/handoff.md) ve [iOS 9 yeni arama API'leri](~/ios/platform/search/index.md) kılavuzları.

### <a name="creating-an-activity"></a>Bir etkinlik oluşturma

Bir etkinlik oluşturmadan önce bir etkinlik türü tanımlayıcısı gerekli tanımlamak için oluşturulacak. Etkinlik türü tanımlayıcısıdır eklenen kısa bir dize `NSUserActivityTypes` uygulamanın dizisi `Info.plist` belirli bir kullanıcı etkinliği türde benzersiz şekilde tanımlamak için kullanılan dosya. Dizi, uygulama destekler ve uygulama ara ortaya çıkaran her etkinlik için bir giriş olacaktır. Bkz: bizim [oluşturma etkinlik türü tanımlayıcıları başvurusu](~/ios/platform/search/nsuseractivity.md) daha fazla ayrıntı için.

Bir etkinlik bir örneğe bakın:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

Yeni bir etkinlik, bir etkinlik türü tanımlayıcısı kullanılarak oluşturulur. Ardından, bu durum daha sonraki bir tarihte geri yüklenebilmesi için etkinlik tanımlama bazı meta verileri oluşturulur. Ardından, etkinliği anlamlı bir başlık verilen ve kullanıcı bilgileri eklenir. Son olarak, bazı özellikleri etkinleştirilir ve etkinlik sisteme gönderilir.

Yukarıdaki kod aşağıdaki değişiklikler yaparak etkinliği için bağlam sağlar meta verileri dahil etmek için daha gelişmiş:

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

Geliştirici uygulamayı olarak aynı bilgileri görüntüleme yeteneğine sahip bir Web sitesi varsa, uygulama URL'sini içerebilir ve içerik uygulamasını (iletim) yüklemediyseniz diğer cihazlarda görüntülenebilir:

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Bir etkinlik geri yükleme

Arama sonucu dokunarak kullanıcı yanıt verecek şekilde (`NSUserActivity`) uygulama için düzenleme **AppDelegate.cs** dosya ve geçersiz kılma `ContinueUserActivity` yöntemi. Örneğin:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Bu etkinlik türü tanımlayıcısının olduğundan emin olmak Geliştirici gerekir (`com.xamarin.platform`) yukarıda oluşturduğunuz etkinlik olarak. Depolanan bilgileri uygulamanın kullandığı `NSUserActivity` durumu geri kullanıcı burada bıraktığınız geri yüklemek için.

### <a name="benefits-of-creating-an-activity"></a>Bir etkinlik oluşturma avantajları

Yukarıda gösterilen kodu en düşük düzeyde ile uygulama üç yeni iOS 10 özelliklerden yararlanmak için sunulmuştur:

- **İletim**
- **Spotlight aramasının**
- **Bağlamsal Siri anımsatıcıları**

Aşağıdaki bölümde, diğer iki yeni iOS 10 özelliklerini etkinleştirme bir göz atalım:

- **Konum önerileri**
- **Bağlamsal Siri istekleri**

### <a name="location-based-suggestions"></a>Konum tabanlı önerileri 

Yukarıdaki Restoran arama uygulama örneği alın. Bunu uyguladı varsa `NSUserActivity` ve doğru şekilde doldurulmuş tüm meta veriler ve öznitelikler, kullanıcı aşağıdakileri gerçekleştirebilir:

1. Bir restoran arkadaş adresindeki karşılamak için istediğiniz uygulamayı bulun.
2. Kullanıcı çoklu uygulama değiştirici kullanarak uygulamayı uzağa doğru hareket ederken, sistem otomatik olarak bir öneri (ekranın en altında), sık kullanılan Gezinti uygulama kullanarak Restoran yönergeleri almak için görüntüler.
3. Kullanıcı iletileri uygulamaya geçer ve yazarak başlatırsa *"Konumundaki şimdi karşılayan"*, QuickType klavye yapıştırma Restoran adresini otomatik olarak önerir.
4. Kullanıcı Maps uygulamaya geçerse Restoran 's adresi otomatik olarak hedef olarak önerilir.
5. Bu bile 3 taraf uygulamalar için çalışır (destekleyen `NSUserActivity`), böylece kullanıcı uygulamasını kılma paylaşımı geçebilir ve Restoran 's adresi otomatik olarak var. hedef olarak da önerilir.
6. Böylece kullanıcının Siri içinde Restoran uygulamayı çağırmak ve sorun ayrıca bağlam Siri için sağlar *"Get yönergeleri..."* ve Siri kullanıcı görüntüleme Restoran yönlendirmeler sağlayacaktır.

Yukarıdaki işlevlerin olan tek şey ortak, tüm bunlar burada öneriyi başlangıçta geldiği gösterir. Yukarıdaki örnekte söz konusu olduğunda, kurgusal Restoran gözden geçirme uygulamasıdır.

iOS 10 birkaç küçük değişiklikler ve eklemeler varolan çerçeveleri aracılığıyla bir uygulama için bu işlevselliği etkinleştirmek için geliştirilmiştir:

- `NSUserActivity` uygulama içinde görüntülenen konum bilgileri yakalamak için ek alanlar vardır.
- Birkaç eklemeleri konumu yakalamak için MapKit ve CoreSpotlight yapıldı.
- Konum kullanan işlevselliği Siri, haritalar, klavyeler, çoklu ve diğer uygulamalar sistemi içinde eklenmiştir.

Konum tabanlı önerileri uygulamak için yukarıda gösterilen aynı etkinlik kodla başlatın:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

Uygulama MapKit kullanıyorsanız, geçerli eşlemesi kadar basittir `MKMapItem` etkinlik için:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Uygulama MapKit kullanıyorsa, uygulama arama benimsemeyi ve aşağıdaki yeni öznitelikler için konum belirtin:

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

Yukarıdaki kod ayrıntılı olarak bakalım. İlk olarak, konum adı her örneği gereklidir:

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Ardından, konuma göre metin açıklaması (örneğin, QuickType klavye) göre metin örnekleri için gereklidir:

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Enlem ve boylam isteğe bağlıdır, ancak kullanıcı dahil edilecek şekilde uygulama için göndermeden isteyen tam konumu yönlendirildiğinden emin olmak:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Kullanıcının Siri uygulamadan aşağıdakine benzer, söyleyerek çağırabileceği için telefon numaralarını ayarlayarak, uygulama Siri erişmesini *"burası çağrısı"*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Son olarak, uygulama örneği gezinti ve telefon aramaları için uygun olup olmadığını belirtir:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>İlgili kişi etkileşimlerini uygulama

Yeni iOS 10'da, iletişimin uygulamaları derine kişi kartından kişiler uygulamada tümleşiktir. Kişi etkileşimleri uyguladık uygulamaları için kullanıcının kişilerinin belirli kişilere belirtilen uygulamanın bilgisi ekleyin. Ve ileti göndermek için bir seçenek olarak Örneğin, bunlar bir ileti göndermek üzere bir kart üstündeki hızlı eylem düğmesine basın, ekli uygulama listelenir.

Bir 3. taraf uygulama seçtiyseniz, hatırlanan ve bir sonraki sefer kullanıcı isteyin istediğinde belirli kişi iletisi için varsayılan yol olarak sunulur.

İlgili kişi etkileşimlerini uygulamasını kullanarak uygulanır `NSUserActivity` ve iOS 10 sunulan yeni hedefleri çerçevesi. Hedefleri ile çalışma hakkında daha fazla ayrıntı için lütfen bkz. bizim [anlama SiriKit kavramları](~/ios/platform/sirikit/understanding-sirikit.md) ve [uygulama SiriKit](~/ios/platform/sirikit/implementing-sirikit.md) kılavuzları.

#### <a name="donating-interactions"></a>Hibe etkileşimleri

Uygulama etkileşimleri nasıl hibe bir göz atalım:

[![](proactive-suggestions-images/activity04.png "Hibe etkileşimleri genel bakış")](proactive-suggestions-images/activity04.png#lightbox)

Uygulamasını oluşturur bir `INInteraction` içeren nesne bir **hedefi** (`INIntent`), **katılımcıları** ve **meta verileri**. **Hedefi** video arama yapmak veya kısa mesaj göndermek gibi bir kullanıcı eylemi temsil eder. **Katılımcıları** iletişimi alma kişileri içerir. **Meta veri** başarıyla gönderme iletisi, vb. gibi ek bilgileri tanımlar.

Geliştirici hiçbir zaman doğrudan bir örneğini oluşturur `INIntent` veya `INIntentResponse`, (uygulama kullanıcı adına gerçekleştirmeye görev bağlı olarak) belirli alt sınıflarından kullanacakları bu üst sınıflardan devralır. Örneğin, `INSendMessageIntent` ve `INSendMessageIntentResponse` bir kısa mesaj göndermek için. 

Etkileşim tam olarak doldurulan sonra çağrı `DonateInteraction` etkileşim kullanmak için kullanılabilir olan sistem bildirmek için yöntem.

Uygulamayla ilgili kişi kartı kullanıcı etkileşim kurduğunda etkileşim birlikte bir `NSUserActivity`, ardından uygulamayı başlatmak için kullanılan:

[![](proactive-suggestions-images/activity05.png "Etkileşim uygulamayı başlatmak için kullanılan bir NSUserActivity ile birlikte")](proactive-suggestions-images/activity05.png#lightbox)

Aşağıdaki örnekte bir Gönder ileti hedefinin göz atın:

```csharp
using System;
using Foundation;
using UIKit;
using Intents;

namespace MonkeyNotification
{
    public class DonateInteraction
    {
        #region Constructors
        public DonateInteraction ()
        {
        }
        #endregion

        #region Public Methods
        public void SendMessageIntent (string text, INPerson from, INPerson[] to)
        {

            // Create App Activity
            var activity = new NSUserActivity ("com.xamarin.message");

            // Define details
            var info = new NSMutableDictionary ();
            info.Add (new NSString ("message"), new NSString (text));

            // Populate Activity
            activity.Title = "Sent MonkeyChat Message";
            activity.UserInfo = info;

            // Enable capabilities
            activity.EligibleForSearch = true;
            activity.EligibleForHandoff = true;
            activity.EligibleForPublicIndexing = true;

            // Inform system of Activity
            activity.BecomeCurrent ();

            // Create message Intent
            var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);

            // Create Intent Reaction
            var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);

            // Create interaction
            var interaction = new INInteraction (intent, response);

            // Donate interaction to the system
            interaction.DonateInteraction ((err) => {
                // Handle donation error
                ...
            });
        }
        #endregion
    }
}
```

Bu kod ayrıntılı bakarak, oluşturur ve bir örneğini doldurur `NSUserActivity` (gösterildiği gibi [bir etkinlik oluşturma](#Creating-an-Activity) yukarıdaki bölümde). Ardından, bir örneğini oluşturur `INSendMessageIntent` (devralan `INIntent`) ve gönderilen ileti ayrıntılarını doldurur:

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

Bir `INSendMessageIntentResponse` oluşturulan ve geçirilen `NSUserActivity` yukarıda oluşturduğunuz:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

Bir `INInteraction` Gönder ileti amaçtan yerleşik (`INSendMessageIntent`) ve yanıt (`INSendMessageIntentResponse`) yeni oluşturduğunuz:

```csharp
var interaction = new INInteraction (intent, response);
```

Son olarak, bildirim etkileşim sistemidir:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

Tamamlama işleyici uygulama nerede başarılı veya başarısız Bağış yanıt olarak geçirilir.

### <a name="activities-best-practices"></a>Etkinlikler en iyi uygulamalar

Apple etkinlikleri ile çalışırken aşağıdaki en iyi yöntemler önerir:

- Kullanım `NeedsSave` yavaş yükü güncelleştirmeler için.
- Geçerli etkinliği için güçlü bir başvuru tutmak için emin olun.
- Yalnızca durumunu geri yüklemek için yeterli bilgi içeren küçük yüklerini aktarın.
- Etkinlik türü tanımlayıcıları bunları belirtmek için ters DNS gösterimini kullanarak benzersiz ve açıklayıcı olduğundan emin olun. 

## <a name="schemaorg"></a>Schema.org

Yukarıda gösterildiği gibi `NSUserActivity` sistemi kullanıcı şu anda çalıştığını ile ekranda hangi bilgilerin anlamanıza yardımcı olur. Schema.org benzer yeteneklerini web sayfalarına ekler.

Schema.org aynı tür Web sitesine göre konumuna etkileşimler sağlayabilir. Apple Safari ile yerel bir uygulamada olduğu gibi görüntülendiğinde da çalışmak için yeni konum önerileri tasarlanmıştır.

Bazı Schema.org arka planı:

- Açık web biçimlendirme sözlük standart sağlar.
- Web sayfalarındaki yapısal meta veriler ekleyerek çalışır.
- Çeşitli kavramlar kullanılabilir temsil eden 500'den fazla şemaları vardır.
- Web sitesinde uygulama tarafından Geliştirici bazı kullanmanın faydaları elde edebilirsiniz `NSUserActivity` yerel bir uygulama.

Bir ağaç yapısı, özel türleri gibi olduğu gibi şemaları düzenlenmiş *Restoran*, daha genel türlerinden gibi devral *yerel iş*. Daha fazla bilgi için lütfen bkz [Schema.org](http://schema.org).

Örneğin, web sayfası aşağıdaki veriler içeriyorsa:

```xml
<script type="application/ld+json>
{
    "@context":"http://schema.org",
    "@type":"Restaurant",
    "telephone":"(415) 781-1111",
    "url":"https://www.yanksing.com",
    "address":{
        "@type":"PostalAddress",
        "streetAddress":"101 Spear St",
        "addressLocality":"San Francisco",
        "postalCode":"94105",
        "addressRegion":"CA"
    },
    "aggregateRating":{
        "@type":"AggregateRating",
        "ratingValue":"3.5",
        "reviewCount":"2022"
    }
}
</script>
```

Kullanıcı Safari'de bu sayfasını ziyaret ve başka bir uygulamaya anahtarlı, sayfadaki konum bilgileri yakalanan ve (yukarıdaki aktivitelerde görüldüğü gibi) konumunu öneri olarak sisteminin diğer bölümlerinde sunulan.

Safari web sayfasında herhangi bir şema aşağıdakiler için aynılarını herhangi bir şey ayıklayın:

- **PostalAddress**
- **GeoCoordinates**
- Telefon özelliği.

Daha fazla bilgi için lütfen bkz bizim [Web biçimlendirme arama](~/ios/platform/search/web-markup.md) Kılavuzu.

## <a name="consuming-location-suggestions"></a>Konum önerileri kullanma

Sonraki bölümde, diğer bölümlerini sisteminde (örneğin, Maps uygulama) veya 3 diğer taraf uygulamalar gelmiş konumu öneri tüketen ele alınacaktır.

Uygulama konumu önerileri tüketebileceği başlıca iki yolu vardır:

- QuickType klavye.
- Konum bilgileri yönlendirme uygulamalarda doğrudan tüketen tarafından.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Konum önerileri ve QuickType klavye

Temel metin biçiminde adresleriyle uygulama ilgilenen, uygulama konumu önerileri QuickType kullanıcı Arabirimi aracılığıyla avantajından yararlanabilirsiniz. iOS 10 aşağıdaki özelliklerle QuickType UI genişletir:

- Uygulamanın kullanıcı Arabiriminde metin alanları için anlamsal amacı hakkında ipuçları ekleyebilirsiniz.
- Uygulama öngörülü önerileri uygulamada elde edebilirsiniz.
- Uygulama, Gelişmiş otomatik düzeltme yararlı olabilir.

Yeni `TextContentType` iOS 10 metin alanı denetimlerinde özellik, kullanıcı belirli bir alana gittiği değeri anlamsal hedefini tanımlamak Geliştirici sağlar. Örneğin:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

Sistem uygulama tam sokak adresi verilen alanında girmesini beklediğini bildirir. Bu, kullanıcı bu alanda bir değer girerken konumu önerileri klavyede otomatik olarak sağlamak QuickType klavye olanak tanır.

Aşağıdaki Developer'da kullanılabilir daha yaygın türleri birkaçıdır `UITextContentType` statik sınıf:

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Yönlendirme uygulamaları ve konumları önerileri

Bu bölümde yönlendirme bir uygulamanın içinden konumu önerileri tüketen bir göz atalım. Bu işlevsellik eklemek yönlendirme uygulama için geliştirici varolan özelliğinden yararlanır `MKDirectionsRequest` framework şekilde:

- Çoklu uygulamada yükseltmek için.
- Uygulama yönlendirme bir uygulama olarak kaydetmek için kullanılır.
- Uygulamasını bir MapKit ile başlatarak işlemek için `MKDirectionsRequest` nesnesi.
- İOS uygulama kullanıcı için uygun zamanlarda önermek bilgi vermek için kullanıcı etkileşimini temel.

Uygulama ile bir MapKit başlatıldığında `MKDirectionsRequest` nesnesi, otomatik olarak kullanıcı yönergeleri istenen konuma vermiş başlatmak veya yönergeleri alma başlatmak kullanıcı kolaylaştırır bir kullanıcı Arabirimi sunar. Örneğin:


```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
    {
        ...
        
        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
                var request = new MKDirectionsRequest (url);
                var coordinate = request.Destination?.Placemark.Location?.Coordinate;
                var address = request.Destination.Placemark.AddressDictionary;
                if (coordinate.IsValid()) {
                    var geocoder = new CLGeocoder ();
                    geocoder.GeocodeAddress (address, (place, err) => {
                        // Handle the display of the address

                    });
                }
            }

            return true;
        }
    }       
}
```

Bu kod ayrıntılı olarak bakalım. Geçerli bir hedef isteği olup olmadığını görmek için sınar:

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Bunun ardından oluşturduğu bir `MKDirectionsRequest` URL'den:

```csharp
var request = new MKDirectionsRequest(url);
```

Yeni iOS 10'da, Uygulama geliştirici adresi kodlama gerektiğinde bu nedeni coğrafi koordinatlarına sahip olmayan bir adresi gönderilebilir:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>Medya uygulama önerileri

Uygulama medya podcast uygulama ya da ses veya video iOS 10 gibi bir akış medya içeriği gibi herhangi bir türde işliyorsa, medya önerileri sistemde yararlanabilir.

Medya işleyen uygulamalar için iOS aşağıdaki davranışları destekler:

- iOS önceki davranışlarını temel kullanıcı kullanmak büyük olasılıkla uygulamalar yükseltir.
- Spotlight ve bugün görünümü uygulamaya ilgili öneriler sunulur.
- Uygulama aşağıdaki Tetikleyicileri karşılıyorsa, kilit ekranı öneride yükseltilmiş:
    - Kulaklıklar veya Bluetooth cihaz takma bir bağlantı yaptıktan sonra.
    - Bir araba edindikten sonra.
    - Evde ulaşan veya iş sonra. 

İOS 10 basit bir API çağrısı ekleyerek, geliştirici medya uygulaması kullanıcıları için daha ilgi çekici bir kilit ekranı deneyimi oluşturabilirsiniz. Kullanarak `MPPlayableContentManager` medya kayıttan yürütme, tam medya denetimlerini (gelenler müzik uygulama tarafından sunulan) yönetmek için sınıf uygulaması için kilit ekranında sunulabilir.


```csharp
using System;
using MediaPlayer;
using UIKit;

namespace MonkeyPlayer
{
    public class PlayableContentDelegate : MPPlayableContentDelegate
    {
        #region Constructors
        public PlayableContentDelegate ()
        {
        }
        #endregion

        #region Override methods
        public override void InitiatePlaybackOfContentItem (MPPlayableContentManager contentManager, Foundation.NSIndexPath indexPath, Action<Foundation.NSError> completionHandler)
        {
            // Access the media item to play
            var item = LoadMediaItem (indexPath);

            // Populate the lock screen
            PopulateNowPlayingItem (item);

            // Prep item to be played
            var status = PreparePlayback (item);

            // Call completion handler
            completionHandler (null);
        }
        #endregion

        #region Public Methods
        public MPMediaItem LoadMediaItem (Foundation.NSIndexPath indexPath)
        {
            var item = new MPMediaItem ();

            // Load item from media store
            ...

            return item;
        }

        public void PopulateNowPlayingItem (MPMediaItem item)
        {
            // Get Info Center and album art
            var infoCenter = MPNowPlayingInfoCenter.DefaultCenter;
            var albumArt = (item.Artwork == null) ? new MPMediaItemArtwork (UIImage.FromFile ("MissingAlbumArt.png")) : item.Artwork;

            // Populate Info Center
            infoCenter.NowPlaying.Title = item.Title;
            infoCenter.NowPlaying.Artist = item.Artist;
            infoCenter.NowPlaying.AlbumTitle = item.AlbumTitle;
            infoCenter.NowPlaying.PlaybackDuration = item.PlaybackDuration;
            infoCenter.NowPlaying.Artwork = albumArt;
        }

        public bool PreparePlayback (MPMediaItem item)
        {
            // Prepare media item for playback
            ...

            // Return results
            return true;
        }
        #endregion
    }
}
```

## <a name="summary"></a>Özet

Bu makalede, öngörülü öneriler ele ve nasıl Geliştirici Xamarin.iOS uygulaması sürücü trafiğini kullanabilmek için gösterdi. Öngörülü önerileri uygulamak için adım ele ve kullanım yönergeleri sunulur.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
- [SiriKit Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
