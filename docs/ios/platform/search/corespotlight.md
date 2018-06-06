---
title: Xamarin.iOS çekirdek Spotlight arama
description: Bu belge, uygulama içeriği bağlantı sağlamak için bir Xamarin.iOS uygulaması çekirdek Spotlight kullanmayı açıklar. Oluşturma, geri yükleme, güncelleştirme ve aranabilir öğeleri silme açıklanır.
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: a8bc3aaa43d7830b0a3baa0768d495458b1ecfad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788045"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Xamarin.iOS çekirdek Spotlight arama

Çekirdek Spotlight eklemek, düzenlemek veya silmek için uygulamanızın içinde içerik bağlantıları için bir veritabanı gibi API sunar iOS 9 için yeni bir çerçevedir. Çekirdek Spotlight kullanılarak eklenen öğeler Spotlight arama iOS cihazında kullanılabilir.

Örneği çekirdek Spotlight kullanarak dizine içerik türleri için Apple'nın iletileri, posta, Takvim ve notlar uygulamaları bakın. Tüm bunlar şu anda çekirdek Spotlight arama sonuçları sağlamak için kullanın.

## <a name="creating-an-item"></a>Bir öğe oluşturma

Bir öğe oluşturma ve çekirdek Spotlight kullanarak dizin oluşturma bir örnek verilmiştir:

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "App Center Test";
attributes.ContentDescription = "Automatically test your app on 1,000 devices in the cloud.";

// Create item
var item = new CSSearchableItem ("1", "products", attributes);

// Index item
CSSearchableIndex.DefaultSearchableIndex.Index (new CSSearchableItem[]{ item }, (error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Bu bilgiler, arama sonucu olarak aşağıdaki gibi görünür:

[![](corespotlight-images/corespotlight01.png "Çekirdek Spotlight arama sonucu genel bakış")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>Bir öğeyi geri yükleme

Çekirdek Spotlight aracılığıyla arama sonucu, uygulamanız için eklenen bir öğe üzerinde kullanıcı dokunur zaman `AppDelegate` yöntemi `ContinueUserActivity` olarak adlandırılır (Bu yöntem için de kullanılabilir `NSUserActivity`). Örneğin:

```csharp
public override bool ContinueUserActivity (UIApplication application,
   NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchableItem.ActionType.ToString ()) {
            // Display content for searchable item...
        }
        break;
    }

    return true;
}
```

Bu süre biz etkinlik sahip olmak için onay olduğuna dikkat edin bir `ActivityType` , `CSSearchableItem.ActionType`.

## <a name="updating-an-item"></a>Bir öğe güncelleştiriliyor

Bir dizin ile çekirdek Spotlight oluşturduğumuz öğe gerektiğinde değiştirilmesine izin gibi bir değişiklik başlık veya küçük resim gereklidir zamanlar olabilir. Bu değişikliği yapmak için aynı yöntemi başlangıçta dizini oluşturmak için kullanılan kullanırız.
Yeni oluşturduğumuz `CSSearchableItem` öğesi oluşturun ve yeni bir eklemek için kullanılan aynı Kimliğini kullanarak `CSSearchableItemAttributeSet` değiştirilmiş özelliklere sahip:

[![](corespotlight-images/corespotlight02.png "Güncelleştirmeye öğesi genel bakış")](corespotlight-images/corespotlight02.png#lightbox)

Bu öğe aranabilir dizine yazıldığında varolan öğeyi yeni bilgilerle güncelleştirilir.

## <a name="deleting-an-item"></a>Öğe silme

Çekirdek Spotlight artık gerekli olmadığında bir dizin öğeyi silmek için birden çok yol sağlar.

İlk olarak, bir öğe tanımlayıcısıyla, örneğin silebilirsiniz:

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Ardından, etki alanı adına göre dizin öğelerinin bir grubu silebilirsiniz. Örneğin:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Son olarak, aşağıdaki kod ile tüm dizin öğeleri silebilirsiniz:

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>Ek çekirdek Spotlight özellikleri

Çekirdek Spotlight dizin doğru ve güncel tutmaya yardımcı aşağıdaki özelliklere sahiptir:

- **Toplu güncelleştirme desteği** – uygulamanızı oluşturmak veya aynı anda çok sayıda dizinleri değiştirmek gerekirse, toplu işin tamamını gönderilebilir `Index` yöntemi `CSSearchableIndex` bir çağrısında sınıfı.
- **Dizin değişikliklere** – kullanarak `CSSearchableIndexDelegate` aranabilir dizinden değişiklikleri ve bildirimleri için uygulamanızı yanıt verebilir.
- **Veri koruma uygulama** – veri koruma sınıflarını kullanarak, çekirdek Spotlight kullanarak aranabilir dizini eklediğiniz öğeler üzerinde güvenlik uygulayabilirsiniz.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Uygulama arama Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
