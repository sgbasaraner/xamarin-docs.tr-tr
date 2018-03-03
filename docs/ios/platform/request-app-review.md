---
title: "İstek uygulama gözden geçirme"
description: "Bu makalede RequestReview yöntemi, Apple iOS 10 ve Xamarin.iOS içinde uygulamak nasıl ekleneceği kapsanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 469a63a990b1adb108284cfb88ee54e05218a8a9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="request-app-review"></a>İstek uygulama gözden geçirme

_Bu makalede RequestReview yöntemi, Apple iOS 10 ve Xamarin.iOS içinde uygulamak nasıl ekleneceği kapsanmaktadır._

10.3, iOS için yeni `RequestReview()` yöntemi oran veya incelemek için kullanıcıya sor bir iOS uygulamasını sağlar. Kullanıcı uygulama mağazasından yüklü bir nakliye uygulamasında bu yöntem çağrıldığında, iOS 10 tüm derecelendirme işler ve işlem için geliştirici gözden geçirin. Bu işlem uygulama mağazası İlkesi tarafından yönetilir olduğundan, bir uyarı olabilir veya görüntülenmeyebilir.

![](request-app-review-images/review01.png "Gözden geçirme isteği bir örnek Uyarısı")

## <a name="requesting-a-rating-or-review"></a>Derecelendirme veya gözden geçirme

Sırada `RequestReview()` statik yöntemi `SKStoreReviewController` sınıfı çağrılabilir burada anlamlı kullanıcı deneyimi, herhangi bir noktada gözden geçirme işlemi yönetilen ve uygulama mağazası İlkesi tarafından işlenir. Sonuç olarak, bu yöntem olabilir veya bir uyarı görüntülenmeyebilir ve hiçbir zaman yanıt olarak bir düğmesine gibi bir kullanıcı eylemi çağrılmalıdır.

Örneğin, bir uygulama edildikten sonra bir gözden geçirme verilen birkaç kez başlatıldığında veya bir düzeye player bittikten sonra oyun İnceleme isteyebilir isteyebilir.

İsteklerine başlatılıyor, bir Xamarin.iOS uygulaması tamamlanır tamamlanmaz bir gözden geçirme aşağıdaki değişiklikleri yapın `AppDelegate.cs` dosyası:

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }
        
        ...
        
    }
}
```

> [!NOTE]
> **Not:** çağırma `RequestReview()` eksik bir geliştirme uygulama her zaman derecelendirme görüntüler ve test edilebilir şekilde iletişim gözden geçirin. Bu yöntem çağrısının burada göz ardı edilir TestFlight dağıtılmış uygulamalar için geçerli değil.

Zaman `RequestReview()` yöntemi, kullanıcı uygulama mağazasından yüklü bir nakliye uygulamasında çağrılır, iOS 10 geliştiricisi tüm derecelendirme ve gözden geçirme işlemi işleyecek. Yeniden, bu işlem uygulama mağazası İlkesi tarafından yönetilir olduğundan, bir uyarı olabilir veya görüntülenmeyebilir.

## <a name="linking-to-an-app-store-product-page"></a>Bir uygulama mağazası ürün sayfasına bağlantılandırma 

Ek olarak yeni `RequestReview` yöntemi, geliştirici hala sağlayabilir uygulamanın ürün sayfası uygulama Mağazası'ndan bir uygulama içinde ayrıntılı bağlantısı. Ekleyerek `action=write-review` ürün sayfası URL'si sonuna bir sayfa olduğu kullanıcı uygulamayı gözden otomatik olarak yazabilirsiniz açılır. 

## <a name="summary"></a>Özet

Bu makalede, Apple iOS 10 ve Xamarin.iOS içinde uygulamak nasıl ekleneceği RequestReview yöntemi ele.



## <a name="related-links"></a>İlgili bağlantılar

- [iOSTenThree örnek](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
