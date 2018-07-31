---
title: Cihaz Xamarin.iOS için sağlama
description: Bu belgede, böylece bir uygulamayı test etmek için kullanılabilir bir cihaz sağlama işlemi açıklanır. Ayrıca, anında iletme bildirimleri gibi özellikleri kullanabilmesi için bir uygulama yapılandırma anlatılmaktadır.
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: f0d6d2343350455a101033aced7cec0c31695503
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353236"
---
# <a name="device-provisioning-for-xamarinios"></a>Cihaz Xamarin.iOS için sağlama

Bir Xamarin.iOS uygulaması geliştirmeye devam ederken, uygulamayı fiziksel bir cihaz için ayrıca simülatöre dağıtılıyor tarafından test etmek için gereklidir. Yalnızca cihaz hataları ve performans sorunlarını, bellek veya ağ bağlantısı gibi donanım sınırları nedeniyle bir cihazda çalıştırıldığında, dizin. Fiziksel bir cihazda test etmek için cihaz olmalıdır *sağlanan*, ve Apple gerekir hakkında bilgi sahibi olmak cihaz test için kullanılacak.

Aşağıdaki görüntüde vurgulanan bölümler iOS sağlama için kurulum için gerekli olan adımları göster:

[![](images/provisioningdiagram.png "Bu görüntüde vurgulanan bölümler iOS sağlama için kurulum için gerekli olan adımları Göster")](images/provisioningdiagram.png#lightbox)

Bundan sonra sonraki adım uygulamayı dağıtmaktır. Dağıtım hakkında daha fazla bilgi için ziyaret [uygulama dağıtımı](~/ios/deploy-test/app-distribution/index.md) Kılavuzlar.

Uygulamayı bir cihaza dağıtmadan önce Apple'nın Geliştirici programına etkin bir aboneliğiniz olması gerekir *veya* kullanın [ücretsiz sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md). Apple, programı iki seçenek sunar:

- **Apple Developer Program** – bir kişi veya bir kuruluş, Apple Developer Program geliştirmek, test ve uygulamaları dağıtmak temsil olmanıza bakılmaksızın.
- **Apple Geliştirici Kurumsal programı** – Enterprise program geliştirin ve yalnızca şirket içi uygulamaları dağıtmak istediğiniz kuruluşlar için en uygun. Kurumsal programı üyeleri iTunes Connect için erişimi yoktur ve oluşturulan uygulamaları App Store için yayımlanamaz.

Bu programlardan birini kaydetmek için ziyaret [Apple Developer Portal'a](https://developer.apple.com/programs/enroll/) kaydedilecek. Bir Apple geliştirici olarak kaydetmek için bunun gerekli olduğunu unutmayın bir [Apple kimliği](https://appleid.apple.com/). Bu kılavuzda varsayımıyla oluşturuldu, **olan** bir Apple Geliştirici programının üyesi.

Alternatif olarak, Apple sunulan [ücretsiz sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md) sağlayan tek bir uygulama, tek bir cihaz üzerinde çalıştırmak Xcode 7'de *olmadan* Apple'nın Geliştirici programının üyesi olmak. Bu şekilde, ayrıntılı olarak sağlanırken bir dizi sınırlandırması olan [burada](~/ios/get-started/installation/device-provisioning/free-provisioning.md#limitations).

Bir cihazda çalışan herhangi bir uygulama kümesi meta verileri içermesi gerekir (veya *parmak izi*), uygulama ve geliştirici ile ilgili bilgiler içerir. Apple, uygulama ile dağıtım yapılırken müdahaleye uğramadığından emin olmak için bu parmak izi veya üzerinde çalışan, kullanıcının cihazına kullanır. Sertifika isteme kullanıcıdan Apple kimliği bir geliştirici olarak kaydolun ve bir uygulama Kimliğini kurmak için uygulama geliştiricileri gerektirerek elde edilir ve uygulamanın dağıtılacağı cihazı kaydedin.

Bir uygulama bir cihaza dağıtım yaparken, bir sağlama profili iOS cihazına de yüklenir. Uygulama oluşturma zamanında birlikte imzalandığı ve şifreli olarak Apple tarafından imzalanır bilgileri doğrulamak için sağlama profili yok. Bir uygulama bir cihaza denetleyerek yerleştirilip yerleştirilemediğini birlikte, sağlama profili ve 'parmak izi' denetimleri belirleyin:

- **Kimin** (Sertifikalar – uygulama edilmiş açtığı sağlama profilinde bir ilgili ortak anahtara sahip bir özel anahtara sahip? Sertifika, bir geliştirme ekibi ile de Geliştirici ilişkilendirir)
- **Ne** (tek tek uygulama kimliği – uygulama kimliği ve sağlama profilinde Info.plist eşleşme paket grubu tanımlayıcısı kümesinde mu?)
- **Burada** (cihaz-cihaz sağlama profilinde yer alan?)

Bu adımları oluşturulan veya kullanılan cihazlar ve uygulamalar dahil geliştirme sürecinde her şeyi geri Apple Developer hesabınız izlenebilir emin olun.

## <a name="provisioning-your-device"></a>Cihazınızı sağlama

İOS Cihazınızı sağlamak için iki yolu vardır:

* **Otomatik olarak (önerilen)** – Select **otomatik sağlama** Visual Studio otomatik olarak oluşturup, imzalama kimlikleri, uygulama kimlikleri ve sağlama profillerini yönetmek için projenizdeki düzeni. Otomatik sağlamayı yönetme hakkında daha fazla bilgi için bkz: [otomatik sağlama](automatic-provisioning.md) Kılavuzu. Bir iOS cihazı sağlama, önerilen yöntem budur.

* **El ile** – imzalama kimlikleri, uygulama kimlikleri ve sağlama profilleri oluşturulabilir ve açıklandığı gibi Apple Geliştirici Portalı yönetilen [el ile sağlama](manual-provisioning.md) Kılavuzu. Bu yapılar, daha sonra açıklandığı gibi yönetilebilir [Apple hesap yönetimi](~/cross-platform/macios/apple-account-management.md) Kılavuzu.

## <a name="provisioning-for-application-services"></a>Uygulama hizmetleri için hazırlama

Apple, özel uygulama hizmetleri için bir Xamarin.iOS uygulaması etkinleştirilebilen özellikleri olarak da bilinir, seçimi sağlar. Bu uygulama hizmetleri, hem iOS sağlama portalı yapılandırılmalıdır olduğunda **uygulama kimliği** oluşturulur ve **Entitlements.plist** dosya Xamarin.iOS uygulama projesinin bir parçası. Uygulama Hizmetleri uygulamanıza ekleme hakkında daha fazla bilgi için bkz [özellikleri giriş](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu ve [Yetkilendirmelerle çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

* Gerekli uygulama hizmetleri ile bir uygulama kimliği oluşturun.
* Yeni bir [sağlama profili](#provisioning-your-device) , bu uygulama kimliğini içerir.
* Yetkilendirmeler Xamarin.iOS projesi Ayarla

## <a name="related-links"></a>İlgili bağlantılar

- [Ücretsiz Sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Uygulama Dağıtımı](~/ios/deploy-test/app-distribution/index.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
- [Apple - uygulama dağıtım kılavuzu](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
