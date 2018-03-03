---
title: "Proje başvuruları"
description: "İOS uygulaması, izleme uygulama ve izleme uzantısı arasındaki ilişki açıklaması."
ms.topic: article
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 2fd39ed46a070d091f0fd5480cc782d319ec15bd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="project-references"></a>Proje başvuruları

_İOS uygulaması, izleme uygulama ve izleme uzantısı arasındaki ilişki açıklaması._

Üç projeler watchOS çözümde *otomatik olarak yapılandırılan* birbirine watchOS 3 uygulamaların oluşturulur ve doğru şekilde paketlenmiş belirli bir şekilde başvurmak için. Aşağıda bu proje başvuruları ve paket tanımlayıcı ayarlarını başvurusunu açıklanmıştır.

## <a name="project-references"></a>Proje başvuruları

Başvuruları her proje için başvurular düğümlerde çift tıklayarak görüntülersiniz:

- **iPhone uygulama** başvuruları **izleme uygulama**

![](project-references-images/catalog-reference1.png "iPhone uygulama izleme uygulama başvurur")

- **Uygulama izleme** başvuruları **izleme uygulama uzantısı**

![](project-references-images/catalog-reference2.png "iPhone uygulama izleme uygulama başvurur")


 - **İzleme uygulaması uzantısı** ya da diğer projeleri başvurmuyor

![](project-references-images/catalog-reference3.png "İzleme uygulaması uzantısının diğer projeleri başvurmuyor")



## <a name="bundle-identifiers"></a>Paket tanımlayıcıları

Emin olmanız gerekir, **paket tanımlayıcı** doğrudur.
Üç projenin olmalıdır *aynı* uzantılarını önceden tanımlanmış iki izleme projelerle tanımlayıcı öneki `watchkitextension` ve `watchkitapp`aşağıdaki gibi (için **WatchKitCatalog** Örnek):

 - Xamarin.iOS Unified project - `com.xamarin.WatchKitCatalog`

 - WatchKit uzantı projesi- `com.xamarin.WatchKitCatalog.watchkitextension`

 - İzleme uygulaması proje- `com.xamarin.WatchKitCatalog.watchkitapp`

Da emin olmanız bu **Info.plist** ayarları doğru:

 - İzleme uygulaması projenin `WKCompanionAppBundleIdentifier` üst/kapsayıcı uygulamanın paket Kimliğini eşleşen (IE. iPhone üzerinde çalışan tek);

 - Gözcü Seti uzantısı projenin **WKApp paket kimliği** izleme uygulama projenin paket kimliği ile eşleşir

Çift tıklayarak tanımlayıcıları düzenleyebilirsiniz **Info.plist** her proje dosyasında.

Bu ekran görüntüsü **izleme uzantının** gösteren Info.plist dosyası **izleme uygulamanın** de tanımlayıcısı:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
    
![](project-references-images/infoplist-extension.png "Bu ekran izleme uzantının Info.plist dosyasını değil")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
![](project-references-images/infoplist-extension-vs.png "Bu ekran izleme uzantının Info.plist dosyasını değil")

-----

Bu ekran görüntüsü **izleme uygulamanın** Info.plist dosyası.
Geçerli **izleme OS** sürümüdür 8.2, böylece **dağıtım hedef** izleme uygulama olmalıdır **8.2**. Xcode yüklü 6.3 varsa, bu değer 8.3 ayarlanabilir - değiştirmeniz gerektiğini unutmayın 8.2.

![](project-references-images/infoplist-watchapp.png "Info.plist dosya izleme")

İzleme uygulaması için dağıtım hedef izleme uzantısı ve iOS uygulaması farklı olabilir.

