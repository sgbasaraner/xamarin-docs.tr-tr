---
title: "Üst uygulama ile çalışma"
description: "WatchOS 1 iOS ve izleme uygulamada arasında veri paylaşımı"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: dab1f5d571473fbbdb12ef78eed3788165c3f0a6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-the-parent-application"></a>Üst uygulama ile çalışma

_WatchOS 1 iOS ve izleme uygulamada arasında veri paylaşımı_

> [!IMPORTANT]
> **Not:** örnekler yalnızca kullanarak üst uygulamaya erişmeyi watchOS 1 izleme uygulamalar üzerinde çalışır.


İzleme uygulaması ve birlikte paketlenebilir iOS uygulaması arasında iletişim kurmak için farklı yolu vardır:

- Gözcü uzantıları can [bir yöntem çağrısı](#code) arka planda iPhone üzerinde çalışan bir üst uygulamanın karşı.

- Gözcü uzantıları can [paylaşımı bir depolama konumu](#storage) üst iPhone uygulama.

- Bir bakışta veya bildirim bir uygulamadaki belirli arabirim denetleyicisi kullanıcı gönderme izleme uygulama veri iletmek için iletimi kullanıyor.

Üst uygulama, bazen bir kapsayıcı uygulama olarak da adlandırılır.


<a name="code" />

## <a name="run-code"></a>Kodu çalıştırma

Bir izleme uzantısı ve üst iPhone uygulama iletişim de gösterilmiştir [GpsWatch örnek](https://developer.xamarin.com/samples/GpsWatch).
Gözcü uzantınızı kendi adına kullanarak bir işlem yapmak için üst iOS uygulaması isteyebilir `OpenParentApplication` yöntemi.

Bu, özellikle uzun çalışan görevler (dahil olmak üzere ağ isteklerini) - yalnızca üst iOS uygulaması bu görevleri tamamlamak ve izleme uzantısı erişilebilir bir konumda alınan verileri kaydetmek için arka plan işleme avantajından yararlanmak için kullanışlıdır.



### <a name="watch-kit-app-extension"></a>Gözcü Seti uygulama uzantısı

Çağrı `WKInterfaceController.OpenParentApplication` izleme Uygulama Uzantısı'nda. Döndürdüğü bir `bool` yöntemi isteğinin başarılı bir şekilde gönderilip gönderilmediğini gösterir. Denetleyin `error` yanıt parametresinde `Action` iPhone uygulamada çalıştıran yöntemi sırasında varsa belirlemek için oluştu.

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
    if(error != null) {
        Console.WriteLine (error);
        return;
    }
    Console.WriteLine ("parent app responded");
    // do something with replyInfo[] dictionary
});
```


### <a name="ios-app"></a>iOS App

İzleme uygulaması uzantısının gelen tüm çağrıları iPhone uygulamanın yönlendirilir `HandleWatchKitExtensionRequest` yöntemi.
Gözcü uygulamada farklı istekleri yapan sonra bu yöntem sorgu gerekecek `userInfo` isteğini işlemek nasıl belirlemek için Sözlük.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    // ... other AppDelegate methods
    public override void HandleWatchKitExtensionRequest
        (UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
    {
        var status = 2;
        // do something in the background, and respond
        reply (new NSDictionary (
            "count", NSNumber.FromInt32 ((int)status),
            "value2", new NSString("some-info")
            ));
    }
}
```


<a name="storage" />

## <a name="shared-storage"></a>Paylaşılan depolama alanı

Yapılandırırsanız, bir [uygulama grubu](~/ios/watchos/app-fundamentals/app-groups.md) sonra (izleme uzantıları dahil olmak üzere) iOS 8 uzantıları üst uygulamayla verileri paylaşabilir.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

Ortak bir dizi başvurabilmek aşağıdaki kodu hem izleme uygulaması uzantısı hem de üst iPhone uygulama yazılabilir `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>Dosyalar

İOS uygulama ve izleme uzantısı de ortak bir dosya yolu kullanarak dosyaları paylaşabilir.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Not: yol ise `null` ardından denetleyin [uygulama grubu yapılandırmasını](~/ios/watchos/app-fundamentals/app-groups.md) sağlama profilleri doğru şekilde yapılandırılmış ve indirilen/geliştirme bilgisayarınızda emin olmak için.

Daha fazla bilgi için lütfen bkz [uygulama grubu özellikleri](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) belgeleri.

## <a name="wormholesharp"></a>WormHoleSharp

Popüler açık kaynak mekanizması watchOS 1 (temel [MMWormHole](https://github.com/mutualmobile/MMWormhole)) veri veya komutları üst uygulaması ve izleme uygulaması arasında geçirmek için.

İOS uygulamanızı böyle bir uygulama grubu kullanılarak deliği yapılandırın ve uzantı izleyin:

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

C# sürümü [WormHoleSharp](https://github.com/Clancey/WormHoleSharp).



## <a name="related-links"></a>İlgili bağlantılar

- [GpsWatch (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WormHoleSharp (örnek)](https://github.com/Clancey/WormHoleSharp)
- [Apple'nın WKInterfaceController başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple'nın içeren uygulamanız ile veri paylaşımı](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
