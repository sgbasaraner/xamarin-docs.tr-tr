---
title: "Cihaz sağlama"
description: "Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlamaya yöneliktir. Bu kılavuz, geliştirme sertifikalar istemek ve profilleri, uygulama hizmetleri ile çalışma ve cihaza bir uygulama dağıtmak inceleyeceksiniz."
ms.topic: article
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/15/2017
ms.openlocfilehash: 3ff77e909c86e627a6a1ea4875d8bef7db3f8d63
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="device-provisioning"></a>Cihaz sağlama

_Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlamaya yöneliktir. Bu kılavuz, geliştirme sertifikalar istemek ve profilleri, uygulama hizmetleri ile çalışma ve cihaza bir uygulama dağıtmak inceleyeceksiniz._

Bir Xamarin.iOS uygulaması geliştirme sırasında fiziksel bir aygıtı uygulamaya ayrıca simulator dağıtarak test etmek için gereklidir. Yalnızca aygıt hataları ve performans sorunlarını bellek ya da ağ bağlantısı gibi donanım sınırları nedeniyle bir cihazda çalıştırıldığında, gerçekleşen. Fiziksel cihaz üzerindeki test etmek için cihaz olmalıdır *sağlanan*, ve Apple gerekir haberdar cihaz test etmek için kullanılacaktır.

Aşağıdaki görüntü vurgulanan bölümlerde iOS sağlama için ayarlanmış almak için gerekli adımları gösterir:

[![](images/provisioningdiagram.png "Bu görüntü vurgulanan bölümlerde iOS sağlama için ayarlanmış almak için gerekli adımları gösterir")](images/provisioningdiagram.png#lightbox)

Bundan sonra sonraki adım uygulamayı dağıtmaktır. Dağıtım hakkında daha fazla bilgi için ziyaret [uygulama dağıtım](~/ios/deploy-test/app-distribution/index.md) kılavuzları.

Uygulama bir cihaza dağıtmadan önce Apple Developer Program, etkin bir aboneliğiniz gerek *veya* kullanmak [ücretsiz sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md). Apple iki program seçenekleri sunar:

- **Apple Developer Program** – bir kişi veya kuruluş, Apple Developer Program geliştirmenize olanak veren test ve uygulamaları dağıtmak temsil olmanıza bakılmaksızın.
- **Apple Geliştirici Kurumsal programı** – Kurumsal programdır geliştirmek ve şirket içi uygulamalar yalnızca dağıtmak istediğiniz kuruluşlar için en uygun. Kurumsal programı'nın üyeleri iTunes Bağlan erişiminiz yoksa ve oluşturulan uygulamalarını uygulama mağazasında yayımlanamaz.


Ya da bu programlar kaydetmek için ziyaret [Apple Geliştirici Portalı](https://developer.apple.com/programs/enroll/) kaydetmek için. Apple geliştirici olarak kaydetmek için bunun gerekli olduğunu unutmayın bir [Apple kimliği](https://appleid.apple.com/). Bu kılavuz varsayımıyla oluşturuldu, **olan** bir Apple Developer Program üyesi.

Alternatif olarak, Apple sunulan [ücretsiz sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md) tek bir cihaz üzerinde çalıştırmak tek bir uygulama sağlayan Xcode 7 *olmadan* Apple Developer Program üyesi olma. Bu şekilde ayrıntılı ayrılırken bir dizi sınırlama olan [burada](~/ios/get-started/installation/device-provisioning/free-provisioning.md#limitations).

Bir cihazda çalışan herhangi bir uygulama meta verileri kümesi içermesi gerekir (veya *parmak izi*), uygulama ve geliştirici hakkında bilgiler içerir. Apple uygulama için dağıtımı sırasında değiştirilmiş değil olduğundan emin olmak için bu parmak izine veya üzerinde çalışan, bir kullanıcının aygıtının kullanır. Uygulama geliştiriciler geliştirici olarak kendi Apple Kimliği'ni kaydetmeniz ve bir uygulama kimliği Kurulum gerektirerek elde edilen bir sertifika istemek ve uygulama dağıtılacak cihazı kaydedin.

Bir cihaza bir uygulama dağıtımı sırasında bir sağlama profili iOS cihazında da yüklenir. Uygulama ile derleme zamanında imzalandığı ve şifreleme açısından Apple tarafından imzalanmış bilgileri doğrulamak için sağlama profil yok. Bir uygulama bir cihaza denetleyerek dağıtılabilir durumunda birlikte, sağlama profili ve 'parmak izi' denetimleri belirleyin:

- **Kimin** (Sertifikalar – uygulama edilmiş açtığı sağlama profilinde karşılık gelen bir ortak anahtara sahip bir özel anahtara sahip mi? Sertifika, ayrıca Geliştirici geliştirme ekibi ile ilişkilendirir)
- **Ne** (tek tek uygulama kimliği – Info.plist eşleşmeyi uygulama kimliği ve sağlama profilinde paket tanımlayıcısı kümesinde mu?)
- **Burada** (aygıtları – sağlama profilinde bulunan cihaz?)

Bu adımları oluşturulan veya uygulamalar ve cihazlar dahil olmak üzere geliştirme işlemi sırasında kullanılan her şeyi geri bir Apple Geliştirici hesabını izlenebilir olduğundan emin olun.

<a name="Provisioning_Profile" />

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="provisioning-your-device"></a>Cihazınızı sağlama

Mac için Visual Studio ile iOS Cihazınızı sağlamak için iki yolu vardır:

* **Otomatik olarak (önerilen)** – Select **imzalama otomatik olarak yönetir** Mac otomatik olarak oluşturup imzalama kimlikleri, uygulama kimlikleri ve sağlama yönetmek için Visual Studio için Info.plist dosyasında seçeneği Profiller.  Otomatik sağlama yönetme hakkında daha fazla bilgi için bkz: [otomatik sağlamayı](automatic-provisioning.md) Kılavuzu. Bu, bir iOS aygıtı sağlama önerilen yoludur.

* **El ile** – imzalama kimlikleri, uygulama kimlikleri ve sağlama profilleri oluşturulabilir ve Apple Geliştirici Portalı açıklandığı gibi yönetilen [el ile sağlama](manual-provisioning.md) Kılavuzu. Bu yapıtların sonra açıklandığı gibi yönetilebilir [Apple hesap yönetimi](~/cross-platform/macios/apple-account-management.md) Kılavuzu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="provisioning-your-device"></a>Cihazınızı sağlama

Dağıtım için bir Apple aygıt ayarlama ve Visual Studio Windows üzerindeki uygulamayla dağıtma adımları için ayrıntılı adımları izlemeniz önerilir [el ile sağlama](manual-provisioning.md) Kılavuzu.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Uygulama hizmetleri için sağlama

Apple özel uygulama hizmetleri, bir Xamarin.iOS uygulaması için etkin hale getirilebilir özellikleri, olarak da bilinir seçimi sağlar. Bu uygulama hizmetleri hem iOS sağlama portalı yapılandırılmalıdır zaman **uygulama kimliği** oluşturulur ve **Entitlements.plist** dosya Xamarin.iOS uygulamanın projesinin bir parçası. Uygulama Hizmetleri uygulamanıza ekleme hakkında daha fazla bilgi için bkz [yetenekleri giriş](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu ve [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

* Bir uygulama kimliği ile gerekli uygulama hizmetleri oluşturun.
* Yeni bir [sağlama profili](#Provisioning_Profile) , bu uygulama kimliğini içerir.
* Xamarin.iOS projesinde yetkilendirmeler ayarlayın

> [!NOTE]
> Şu anda, Mac için Visual Studio'da oluşturulan profiller sağlama projeler (Entitlements.plist) seçili hesap yetkilendirmeler içine olmayacaktır. Bu işlev gelecekteki IDE sürümlerinde eklenecektir. Uygulama Hizmetleri kullanmanız gerekiyorsa, yönergeleri izlemeniz önerilir [el ile sağlama](manual-provisioning.md) Kılavuzu.

## <a name="related-links"></a>İlgili bağlantılar

- [Ücretsiz Sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Uygulama Dağıtımı](~/ios/deploy-test/app-distribution/index.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
- [Apple - uygulama dağıtım kılavuzu](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)