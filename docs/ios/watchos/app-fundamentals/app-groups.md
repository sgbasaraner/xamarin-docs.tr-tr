---
title: "Uygulama grupları ile çalışma"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 27ce6c48c5bca69605773eb5ef5637201b9ce6c5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-app-groups"></a>Uygulama grupları ile çalışma


Bir uygulama grubu farklı uygulamaları (veya bir uygulama ve uzantılarını) paylaşılan dosya depolama konumuna erişim sağlar. Uygulama grupları gibi verileri için kullanılabilir:

- Apple Watch [ayarları](~/ios/watchos/app-fundamentals/settings.md).
- Paylaşılan [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Paylaşılan [dosyaları](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Bir uygulama grubu yapılandırma

Paylaşılan konumda kullanılarak yapılandırılmış bir [uygulama grubu](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), içinde yapılandırılmış **tanımlayıcıları & profilleri, sertifikaları** bölümünde [iOS Geliştirme Merkezi](https://developer.apple.com/devcenter/ios/). Bu değer, her projesinin başvurulmalıdır **Entitlements.plist**.

### <a name="provisioning"></a>Sağlama

Uygulama grubu, genellikle, paket kimliği olan bir tanımlayıcı sahip olur ile bir `group.` öneki. Örneğin, biz paket kimliği kullanabilirsiniz `com.xamarin.WatchSettings` ve uygulama grubu `group.com.xamarin.WatchSettings`.

[ ![](app-groups-images/app-group-sml.png "Paket kimliği com.xamarin.WatchSettings ve uygulama grubu group.com.xamarin.WatchSettings kullanın")](app-groups-images/app-group.png)

### <a name="entitlementsplist"></a>Entitlements.plist

Sağlama profili yapılandırma yanı sıra **uygulama gruplarını etkinleştirmek** içinde **Entitlements.plist** ve seçtiğiniz Kimliğini girin:

[ ![](app-groups-images/entitlements-sml.png "Plist yapılandırmak ve kimliği girin")](app-groups-images/entitlements.png)


### <a name="deployment"></a>Dağıtım

Uygulama grubu doğru bir şekilde yapılandırdığınız emin olun, [dağıtım](~/ios/watchos/deploy-test/index.md#app-groups) sağlama.


Daha fazla bilgi için lütfen bkz [uygulama grubu özellikleri](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) belgeleri.


## <a name="related-links"></a>İlgili bağlantılar

- [Apple'nın içeren uygulamanız ile veri paylaşımı](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple'nın uygulama grubu belge](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
