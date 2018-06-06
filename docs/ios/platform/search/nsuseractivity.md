---
title: Xamarin.iOS NSUserActivity ile arama
description: Bu belge, aranabilir Spotlight ve Safari kolaylaştırarak bir NSUserActivity dizin açıklar. Arama sonuçlarında bir NSUserActivity seçimini yanıt verecek şekilde nasıl açıklanır.
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4b053f66e9b6b7715cbe52c4e43d9db32db48f4c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788214"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Xamarin.iOS NSUserActivity ile arama

`NSUserActivity` iOS 8 sunulmuştur ve iletimi için veri sağlamak için kullanılır.
Ardından uygulamanızı farklı iOS cihaz üzerinde çalışan başka bir örneğine geçirilebilir, uygulamanızın belirli bölümlerinde etkinlikleri oluşturmanızı sağlar. Alıcı aygıt önceki cihazda kullanıcı burada bıraktığınız yukarı sola çekme başlatılan etkinliği sonra devam edebilirsiniz. İletim kullanma hakkında daha fazla bilgi için lütfen bkz bizim [iletimi giriş](~/ios/platform/handoff.md) belgeleri.

İOS 9, yeni `NSUserActivity` (genel ve özel olarak) dizine ve Spotlight arama ve Safari aranır. İşaretleyerek bir `NSUserActivity` aranabilir ve ekleme dizine olarak meta veriler, iOS cihazında arama sonuçlarında etkinlik listelenebilir.

[![](nsuseractivity-images/apphistory01.png "Uygulama geçmişi genel bakış")](nsuseractivity-images/apphistory01.png#lightbox)

Kullanıcı, uygulamanızdan bir etkinliğe ait arama sonucu seçerse, uygulama başlatılır ve etkinlik açıklanan tarafından `NSUserActivity` yeniden ve kullanıcıya sunulmaz.

Aşağıdaki özellikleri `NSUserActivity` uygulama arama desteklemek için kullanılır:

 - `EligibleForHandoff` – IF `true`, bu etkinlik bir iletim işlemde kullanılabilir.
 - `EligibleForSearch` – IF `true`, bu etkinlik aygıt dizine eklenecek ve arama sonuçlarında sunulmaz.
 - `EligibleForPublicIndexing` – IF `true`, bu etkinlik için Apple'nın bulut tabanlı dizin eklenir ve uygulamanızı iOS cihazlarında zaten yüklü değildir (Ara) aracılığıyla kullanıcılara sunulur. Bkz: [ortak arama dizini oluşturma](#Public-Search-Indexing) daha fazla ayrıntı için bölüm aşağıda.
 - `Title` – Etkinliği için bir başlık sağlar ve arama sonuçlarında görüntülenir. Kullanıcılar ayrıca başlık metni için arama yapabilirsiniz.
 - `Keywords` – Dizine ve son kullanıcı tarafından aranabilir yapılan etkinliklerinizi tanımlamak için kullanılan bir dize dizisi değil.
 - `ContentAttributeSet` – Olan bir `CSSearchableItemAttributeSet` daha fazla ayrıntı etkinlik tanımlamak ve arama sonuçlarında zengin içerik sağlamak için kullanılır.
 - `ExpirationDate` – Belirli bir tarih yalnızca gösterilecek için bir etkinlik istiyorsanız, o tarih sağlayabilir.
 - `WebpageURL` – Etkinlik Web'de görüntülenebilir veya uygulamanızın Safari'nin ayrıntılı bağlantılar destekliyorsa, burada ziyaret etmek için bağlantıyı ayarlayabilirsiniz.

## <a name="nsuseractivity-quickstart"></a>NSUserActivity hızlı başlangıç

Bir aranabilir uygulamak için bu yönergeleri izleyin `NSUserActivity` uygulamanızda:

- [Etkinlik türü tanımlayıcıları oluşturma](#creatingtypeid)
- [Bir etkinlik oluşturma](#createactivity)
- [Bir etkinliğe yanıt](#respondactivity)
- [Genel arama dizini oluşturma](#indexing)

Bazı vardır [başka avantajları](#benefits) kullanmaya `NSUserActivity` içeriğinizi aranabilir yapmak için.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>Etkinlik türü tanımlayıcıları oluşturma

Bir arama etkinlik oluşturabilmeniz için önce oluşturmanız gerekir bir _etkinlik türü tanımlayıcısı_ tanımlamak için. Etkinlik türü tanımlayıcısıdır eklenen kısa bir dize `NSUserActivityTypes` uygulamanın dizisi **Info.plist** belirli bir kullanıcı etkinliği türde benzersiz şekilde tanımlamak için kullanılan dosya. Dizi, uygulama destekler ve uygulama ara ortaya çıkaran her etkinlik için bir giriş olacaktır. 

Apple çakışmaları önlemek için geriye doğru DNS stili gösterimi etkinlik türü tanımlayıcısı için kullanılmasını önerir. Örneğin: `com.company-name.appname.activity` için belirli uygulama tabanlı etkinlikler veya `com.company-name.activity` birden çok uygulama arasında çalıştırabilirsiniz etkinlikler için.

Etkinlik türü tanımlayıcısı oluşturulurken kullanılan bir `NSUserActivity` etkinlik türünü tanımlamak için örnek. Bir etkinlik arama sonucu dokunarak kullanıcı sonucu olarak devam ettirildiğinde, etkinlik türü (birlikte uygulamanın takım kimliği) etkinlik devam etmek için başlatmak için hangi uygulama belirler.

Bu davranış desteklemek için gerekli etkinlik türü tanımlayıcıları oluşturmak için düzenleyin **Info.plist** dosya ve geçiş **kaynak** görünümü. Ekleme bir `NSUserActivityTypes` anahtarı ve tanımlayıcıları aşağıdaki biçimde oluşturun:

[![](nsuseractivity-images/type01.png "NSUserActivityTypes anahtarı ve plist düzenleyicisinde gerekli tanımlayıcıları")](nsuseractivity-images/type01.png#lightbox)

Yukarıdaki örnekte oluşturduğumuz arama etkinliği için yeni bir etkinlik türü tanımlayıcısı (`com.xamarin.platform`). Kendi uygulamaları oluştururken Değiştir `NSUserActivityTypes` etkinlikleri belirli etkinlik türü tanımlayıcıları ile uygulamanızı dizi destekler.

<a name="createactivity" />

## <a name="creating-an-activity"></a>Bir etkinlik oluşturma

Barındırılan cihaz üzerinde dizin araması için bir etkinlik oluşturma bir örnek verilmiştir:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.BecomeCurrent();
```

Daha fazla ayrıntı ayarlayarak eklediğimiz `ContentAttributeSet` özelliği bizim `NSUserActivity` gibi:

[![](nsuseractivity-images/apphistory02.png "Ek arama ayrıntıları genel bakış")](nsuseractivity-images/apphistory02.png#lightbox)

Kullanarak bir `ContentAttributeSet` etkileşim için son kullanıcı ikna zengin arama sonuçları oluşturabilirsiniz.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>Bir etkinliğe yanıt

Arama sonucu dokunarak kullanıcı yanıt verecek şekilde (`NSUserActivity`) uygulamamıza için düzenleme **AppDelegate.cs** dosya ve geçersiz kılma `ContinueUserActivity` yöntemi. Örneğin:

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

Bu iletimi isteklerine yanıt vermek için kullanılan aynı yöntemi geçersiz kılma olduğuna dikkat edin. Artık kullanıcı Spotlight arama sonuçlarında bizim uygulamasından bir bağlantıyı tıklattığında, uygulamamıza ön plana duruma (veya kaldırılacak zaten çalışıyorsa başlatıldı) ve içerik, gezinti veya bu bağlantı tarafından temsil edilen özelliği görüntülenir:

[![](nsuseractivity-images/apphistory03.png "Önceki duruma geri araması")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>Genel arama dizini oluşturma

Yukarıdaki gördüğümüz gibi iOS 9, uygulama içeriğini ve kullanıcı daha önce bulunan ve belirtilen iOS aygıtında kullanılan özellikler arama erişim sağlamak kolaylaştırır. Ortak dizin ile iOS 9 bir kimin henüz bulunan içerik veya özellikler henüz (veya olmasanız bile yüklemiş uygulama) kullanıcılar için yoldur kendi aramalarda çok bu sonuçlar almak için.

Ortak dizin oluşturma, aşağıdaki şekilde çalışır:

1. Uygulamanız için bir etkinlik oluşturduğunuzda, genel olarak işaretleyebilirsiniz.
2. Ortak etkinlikler için Apple gönderilir ve bulutta dizini.
3. Daha fazla kullanıcı cihazda, uygulama ile etkileşim ve belirli özelliğini veya etkinlik tarafından temsil edilen içeriği gibi derece artar.
4. Uygulamasının yüklü olmasa bile popüler ortak sonuçlar diğer kullanıcılar için kullanılabilir olacaktır.
5. (Etkinlik bir URL içeriyorsa) bu ortak sonuçları Spotlight arama ve Safari görünecektir.

Biz yukarıda oluşturduğumuz özel arama etkinlik alabilir ve genel olarak genişletin:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.EligibleForPublicIndexing = true;
activity.BecomeCurrent();
```

Yalnızca bir etkinlik ortak ayarlayarak dizin oluşturma işlemi için sahip ayarlanmadığından `EligibleForPublicIndexing = true`, Apple'nın genel bulut dizini otomatik olarak eklenen anlamına gelmez. Önce aşağıdaki koşulların karşılanması gerekir:

1. Arama sonuçlarında görüntülenmesi gerekir ve birden çok kullanıcı tarafından seçili olmalıdır. Bir etkinlik katılım eşik geçildikten kadar sonuçları özel kalır.
2. Uygulama sağlama dizine ve ortak yapılan herhangi bir kullanıcıya özgü veri engeller.

<a name="benefits" />

## <a name="additional-benefits"></a>Ek avantajları

Uygulama arama aracılığıyla benimsenmesi tarafından `NSUserActivity` uygulamanızda, aynı zamanda aşağıdaki özellikleri alırsınız:

- **İletim** - uygulama arama içerik, gezinti gösterme ve/veya iletimi aynı mekanizmayı kullanarak özellikleri bu yana (`NSUserActivity`), bir aygıtta bir etkinlik başlangıç ve başka devam etmek, uygulamanızın kullanıcıların kolayca izin verebilirsiniz.
- **Siri önerileri** -normal olarak Siri önerilerde standart öneriler yanı sıra, uygulamanızdan Etkinleri otomatik olarak önerilebilir.
- **Siri akıllı anımsatıcıları** -kullanıcıların bunları uygulamanızdan etkinlikleriyle ilgili anımsatmak için Siri isteyin mümkün olacaktır.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Uygulama arama Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [Blog gönderisi & örnek](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
