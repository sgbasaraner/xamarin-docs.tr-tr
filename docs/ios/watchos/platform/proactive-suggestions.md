---
title: "Öngörülü önerileri"
description: "Bu makalede öngörülü önerileri watchOS 3 uygulama sürücü katılım için proaktif olarak yararlı bilgiler kullanıcıya otomatik olarak sunmak üzere sistem vererek nasıl kullanılacağı gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: f9711cc39662a7e77d926551a0d2b49363d8ec4d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="proactive-suggestions"></a>Öngörülü önerileri

_Bu makalede öngörülü önerileri watchOS 3 uygulama sürücü katılım için proaktif olarak yararlı bilgiler kullanıcıya otomatik olarak sunmak üzere sistem vererek nasıl kullanılacağı gösterilmektedir._


WatchOS 3, öngörülü önerileri mevcut haber yolları otomatik olarak kullanıcı için önceden mevcut yararlı bilgiler uygun zamanlarda tarafından Xamarin.iOS uygulaması ile etkileşim kullanıcılar için yeni.


## <a name="about-proactive-suggestions"></a>Öngörülü öneriler hakkında

WatchOS 3, yeni `NSUserActivity` içeren bir `MapItem` diğer bağlamlarda kullanılabilir konum bilgileri sağlayan yazmasına izin verir. özellik. Görüntülenen uygulama otel gözden geçirir ve sağlar, örneğin, bir `MapItem` konumu, kullanıcı Maps uygulamaya geçtiyseniz, bunlar yalnızca görüntülemekte olduğunuz otel konumunu olacaktır kullanılabilir.

Uygulama teknolojileri koleksiyonu gibi kullanarak sisteme bu işlevselliği kullanıma sunan `NSUserActivity`, MapKit, Media Player ve Uıkit. Ayrıca, uygulama için öngörülü öneri desteği sağlayarak, daha derin Siri tümleştirme ücretsiz alır.

## <a name="location-based-suggestions"></a>Konum tabanlı önerileri

WatchOS 3, yeni `NSUserActivity` sınıfı içeren bir `MapItem` diğer bağlamlarda kullanılabilir konum bilgileri sağlamak Geliştirici imkan tanıyan özellik. Örneğin, uygulama Restoran incelemeler gösterirse, geliştirici ayarlayabilirsiniz `MapItem` kullanıcı uygulamada görüntüleme Restoran konumunu özelliği. Kullanıcı Maps uygulamaya geçerse Restoran'ın konumu otomatik olarak kullanılabilir.

Uygulama uygulama arama destekliyorsa, yeni adres bileşenlerinin kullanabilirsiniz `CSSearchableItemAttributesSet` sınıfı kullanıcı ziyaret isteyebilir konumlarını belirtin. Ayarlayarak `MapItem` özelliği, diğer özellikleri otomatik olarak doldurulmuş içinde.

Ayar yanı sıra `Latitude` ve `Longitude` adresi bileşen özelliklerinden, uygulama sağlamanız önerilir `NamedLocation` ve `PhoneNumbers` özellikleri çok, bu nedenle Siri başlatabilir konumu için bir çağrı.

## <a name="contextual-siri-reminders"></a>Bağlamsal Siri anımsatıcıları

Şu anda uygulamada daha sonraki bir tarihte görüntüledikleri içeriği görüntülemek için bir anımsatıcı hızlı bir şekilde sağlamak için Siri kullanın olanak tanır. Örneğin, bir restoran incelemesi uygulamada görüntülemekte olduğunuz varsa, bunlar Siri çağırma ve söyleyin *"home almak, bu konuda hatırlat."* Siri gözden bağlantı anımsatıcı uygulamada oluşturur.

## <a name="implementing-proactive-suggestions"></a>Öngörülü önerileri uygulama

Öngörülü öneri ekleme desteği Xamarin.iOS uygulaması için birkaç API'leri uygulamadan veya uygulaması zaten uygulama birkaç API genişletme genellikle kadar kolaydır.

Öngörülü önerileri uygulamalarla üç temel şekilde çalışır:

- **`NSUserActivity`** -Kullanıcı şu anda ekranda'nın çalıştığı hangi bilgilerin anlamak sistemi yardımcı olur.
- **Konum önerileri** - uygulama sunar veya bu bilgileri uygulama arasında paylaşmak için temel konum bilgileri, bu API uzantısı teklif yeni yolları kullanır.

Ve uygulamada aşağıdakileri uygulayarak desteklenir:

- **Bağlamsal Siri anımsatıcıları** - iOS 10 `NSUserActivity` hızla bunlar şu anda görüntülemekte olduğunuz uygulamada daha sonraki bir tarihte içeriği görüntülemek için bir anımsatıcı yapmak Siri'ye izin verecek şekilde genişletilmiştir.
- **Konum önerileri** -iOS 10 geliştirir `NSUserActivity` uygulama içinde görüntülenen konumları yakalamak ve bunları Sistem genelindeki birçok yerde yükseltmek için.
- **Bağlamsal Siri istekleri**  -  `NSUserActivity` böylece kullanıcı uygulama içinde bildirilecekse Siri gelen yönergeleri veya bir çağrı olmalıdır yer alabilir içinde uygulama Siri tarafından sunulan bilgiler için bir bağlam sağlar.

Bu özelliklerin tümü, ortak bir şey vardır, tüm kullandıkları `NSUserActivity` bir form veya başka işlevleri sağlamak için. 

## <a name="nsuseractivity"></a>NSUserActivity

Yukarıda belirtildiği gibi `NSUserActivity` sistemi kullanıcı şu anda çalıştığını ile ekranda hangi bilgilerin anlamanıza yardımcı olur. `NSUserActivity` Hafif durumu, kullanıcının etkinliğini üzerinden uygulama gittikleri olarak yakalamak için bir mekanizma önbelleğe alma. Örneğin, Restoran uygulamaya aranıyor:

[![](proactive-suggestions-images/activity02.png "Restoran uygulama")](proactive-suggestions-images/activity02.png#lightbox)

İle aşağıdaki etkileşimler:

1. Kullanıcının uygulamayla çalışırken bir `NSUserActivity` daha sonra uygulama durumunu yeniden oluşturmak için oluşturulur.
2. Kullanıcı için bir restoran arar, etkinlikleri oluşturma aynı düzeni izler.
3. Ve yeniden, kullanıcı bir sonuç zaman görüntüler. Son bu durumda, kullanıcı bir konum görüntüleme ve iOS 10'da, sistem daha belirli kavramları (örneğin, konum veya iletişim etkileşimleri) bilmektedir.

Son ekran daha yakın bir göz atın:

[![](proactive-suggestions-images/activity03.png "NSUserActivity yükü")](proactive-suggestions-images/activity03.png#lightbox)

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

Bu etkinlik türü tanımlayıcısının olduğundan emin olun (`com.xamarin.platform`) yukarıda oluşturduğunuz etkinlik olarak. Depolanan bilgileri uygulamanın kullandığı `NSUserActivity` durumu geri kullanıcı burada bıraktığınız geri yüklemek için.

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
4. Kullanıcı Maps uygulamaya geçerse Restoran 's adresi otomatik olarak hedef olarak önerilir.
5. Bu bile 3 taraf uygulamalar için çalışır (destekleyen `NSUserActivity`), böylece kullanıcı uygulamasını kılma paylaşımı geçebilir ve Restoran 's adresi otomatik olarak var. hedef olarak da önerilir.
6. Böylece kullanıcının Siri içinde Restoran uygulamayı çağırmak ve sorun ayrıca bağlam Siri için sağlar *"Get yönergeleri..."* ve Siri kullanıcı görüntüleme Restoran yönlendirmeler sağlayacaktır.

Yukarıdaki işlevlerin olan tek şey ortak, tüm bunlar burada öneriyi başlangıçta geldiği gösterir. Yukarıdaki örnekte söz konusu olduğunda, kurgusal Restoran gözden geçirme uygulamasıdır.

watchOS 3 birkaç küçük değişiklikler ve eklemeler varolan çerçeveleri aracılığıyla bir uygulama için bu işlevselliği etkinleştirmek için geliştirilmiştir:

- `NSUserActivity` uygulama içinde görüntülenen konum bilgileri yakalamak için ek alanlar vardır.
- Birkaç eklemeleri konumu yakalamak için MapKit ve CoreSpotlight yapıldı.
- Konum kullanan işlevselliği Siri, haritalar, çoklu ve diğer uygulamalar sistemi içinde eklenmiştir.

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

Ardından, bağlı bir açıklama metin için gereken metin (örneğin, QuickType klavye) çıkarak:

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Enlem ve boylam isteğe bağlıdır, ancak kullanıcı uygulamayı göndermenize isteyen tam konumuna yönlendirildiğinden emin olmak:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Kullanıcının Siri uygulamadan aşağıdakine benzer, söyleyerek çağırabileceği için telefon numaralarını ayarlayarak, uygulama Siri erişmesini * "burası çağrısı":

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Son olarak, uygulama örneği gezinti ve telefon aramaları için uygun olup olmadığını belirtir:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>Etkinlikler en iyi uygulamalar

Apple etkinlikleri ile çalışırken aşağıdaki en iyi yöntemler önerir:

- Kullanım `NeedsSave` yavaş yükü güncelleştirmeler için.
- Geçerli etkinliği için güçlü bir başvuru tutmak için emin olun.
- Yalnızca durumunu geri yüklemek için yeterli bilgi içeren küçük yüklerini aktarın.
- Etkinlik türü tanımlayıcıları bunları belirtmek için ters DNS gösterimini kullanarak benzersiz ve açıklayıcı olduğundan emin olun. 

## <a name="consuming-location-suggestions"></a>Konum önerileri kullanma

Sonraki bölümde, diğer bölümlerini sisteminde (örneğin, Maps uygulama) veya 3 diğer taraf uygulamalar gelmiş konumu öneri tüketen ele alınacaktır.

## <a name="routing-apps-and-locations-suggestions"></a>Yönlendirme uygulamaları ve konumları önerileri

Bu bölümde yönlendirme bir uygulamanın içinden konumu önerileri tüketen bir göz atalım. Bu işlevsellik eklemek yönlendirme uygulama için geliştirici varolan özelliğinden yararlanır `MKDirectionsRequest` framework şekilde:

- Çoklu uygulamada yükseltmek için.
- Uygulama yönlendirme bir uygulama olarak kaydetmek için kullanılır.
- Uygulamasını bir MapKit ile başlatarak işlemek için `MKDirectionsRequest` nesnesi.
- WatchOS kullanıcı etkileşimini tabanlı uygulama önermek üzere öğrenin olanağı verir.

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

Yeni watchOS 3'de, Uygulama geliştirici adresi kodlama gerektiğinde bu nedeni coğrafi koordinatlarına sahip olmayan bir adresi gönderilebilir:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>Özet

Bu makalede, öngörülü öneriler ele ve nasıl geliştirici bir Xamarin.iOS uygulaması sürücü trafiğini watchOS için kullanabilmek için gösterdi. Öngörülü önerileri uygulamak için adım ele ve kullanım yönergeleri sunulur.


## <a name="related-links"></a>İlgili bağlantılar

- [watchOS Örnekleri](https://developer.xamarin.com/samples/watchos/all/)
- [SiriKit Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
