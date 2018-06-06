---
title: Cihaz Xamarin.iOS için hazırlama
description: Bu belge, böylece bir uygulamayı test etmek için kullanılabilir bir cihaz hazırlayın açıklar. Ayrıca, anında iletme bildirimleri gibi özelliklerini kullanabilmesi için bir uygulama yapılandırma anlatılmaktadır.
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 9721cc40319f0b4d6f0869eabccb84256122fb02
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785794"
---
# <a name="device-provisioning-for-xamarinios"></a>Cihaz Xamarin.iOS için hazırlama

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

## <a name="provisioning-your-device"></a>Cihazınızı sağlama

İOS Cihazınızı sağlamak için iki yolu vardır:

* **Otomatik olarak (önerilen)** – Select **otomatik sağlamayı** projeniz Visual Studio otomatik olarak oluşturup imzalama kimlikleri, uygulama kimlikleri ve sağlama profillerini yönetmek için düzeni. Otomatik sağlama yönetme hakkında daha fazla bilgi için bkz: [otomatik sağlamayı](automatic-provisioning.md) Kılavuzu. Bu, bir iOS aygıtı sağlama önerilen yoludur.

* **El ile** – imzalama kimlikleri, uygulama kimlikleri ve sağlama profilleri oluşturulabilir ve Apple Geliştirici Portalı açıklandığı gibi yönetilen [el ile sağlama](manual-provisioning.md) Kılavuzu. Bu yapıtların sonra açıklandığı gibi yönetilebilir [Apple hesap yönetimi](~/cross-platform/macios/apple-account-management.md) Kılavuzu.


<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Uygulama hizmetleri için sağlama

Apple özel uygulama hizmetleri, bir Xamarin.iOS uygulaması için etkin hale getirilebilir özellikleri, olarak da bilinir seçimi sağlar. Bu uygulama hizmetleri hem iOS sağlama portalı yapılandırılmalıdır zaman **uygulama kimliği** oluşturulur ve **Entitlements.plist** dosya Xamarin.iOS uygulamanın projesinin bir parçası. Uygulama Hizmetleri uygulamanıza ekleme hakkında daha fazla bilgi için bkz [yetenekleri giriş](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu ve [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

* Bir uygulama kimliği ile gerekli uygulama hizmetleri oluşturun.
* Yeni bir [sağlama profili](#Provisioning_Profile) , bu uygulama kimliğini içerir.
* Xamarin.iOS projesinde yetkilendirmeler ayarlayın

## <a name="related-links"></a>İlgili bağlantılar

- [Ücretsiz Sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Uygulama Dağıtımı](~/ios/deploy-test/app-distribution/index.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
- [Apple - uygulama dağıtım kılavuzu](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
