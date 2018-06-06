---
title: Ücretsiz Xamarin.iOS uygulamaları için sağlama
description: Bu belgede, Apple'nın Ücretli Geliştirici programına kaydolun gerek kalmadan Xamarin.iOS geliştiriciler fiziksel cihazda kendi uygulama nasıl sınayabilirsiniz açıklanmaktadır.
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 03/19/2017
ms.openlocfilehash: 623f79f482170c6b1d8ecdb642afb2fc7acf061d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786028"
---
# <a name="free-provisioning-for-xamarinios-apps"></a>Ücretsiz Xamarin.iOS uygulamaları için sağlama

_Tüm iOS ve Mac geliştiricileri için önemli bir değişiklik Xcode 7 Apple'nın sürümü ile gelen: sağlama boş._

Ücretsiz sağlanmasına olanak tanır geliştiriciler için iOS cihazına Xamarin.iOS uygulamasına dağıtmak **olmadan** herhangi bir parçası olan **Apple Developer Program**. Bu geliştiriciler için oldukça avantajlıdır bir aygıtta test olarak birçok faydaları sağlar, ancak bunlarla sınırlı olmamak bellek, depolama, ağ bağlantısı diğerlerinin yanı sıra dahil olmak üzere simulator sınama üzerinden.

Oluşturan bir Apple Developer hesabı Xcode gerçekleştirilmelidir olmadan sağlama, bir *imzalama kimlik* (bir geliştirici sertifikası ve özel anahtarı içeren) ve bir *sağlama profili* () bir açık uygulama kimliği ve bağlı iOS Cihazınızı UDID içeren).

## <a name="requirements"></a>Gereksinimler

Xamarin.iOS dağıtma avantajlarından yararlanmak için ücretsiz sağlama ile bir cihaza uygulamaları Xcode 7'yi kullanarak veya üstü.

**Kullanılan Apple kimliği herhangi Apple Developer Program bağlı olmamalıdır.**

Uygulamanızda kullanılan paket kimliği benzersiz olmalıdır ve başka bir uygulamada daha önce kullanılmış olamaz. Ücretsiz sağlama olabilir ile kullanılmayan tüm paket kimliği yeniden yeniden kullanılabilir. Bir uygulama zaten dağıtılmış, ücretsiz sağlama ile bu uygulama sağlayamazsınız. 

Başvurmak [uygulama dağıtım kılavuzları](~/ios/deploy-test/app-distribution/index.md) daha fazla bilgi için.

Uygulama Hizmetleri, uygulamanızın kullandığı sonra ayrıntılı biçimde açıklandığı gibi bir sağlama profili oluşturmanız gerekir [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md#appservices) Kılavuzu. Daha fazla sınırlamalarını görebilirsiniz [ilgili bölümü](#limitations) aşağıda.


## <a name="a-namelaunching--launching-your-app"></a><a name="launching" /> Uygulamanızı başlatma

Mac veya Visual Studio için Visual Studio Xcode imzalama kimlik ve sağlama profilleri oluşturmak için kullanacağı ve ardından kullanacaktır cihazına bir uygulamayı dağıtmak için ücretsiz sağlama kullanmak için uygulamamıza ile imzalamak için doğru profili seçin. Bunu yapmak için aşağıdaki adım adım izleyin:

1. Bir Apple kimliği yoksa, tek tek oluşturun [appleid.apple.com](https://appleid.apple.com/account).
2. Xcode açın ve **Xcode > Tercihler**.
3. Altında **hesapları**, kullanın **+** var olan Apple kimliğinizi Ekle düğmesi Aşağıdaki ekran görüntüsüne benzer görünmelidir:

  [![](free-provisioning-images/launchapp1.png "Xcode Tercihler hesapları")](free-provisioning-images/launchapp1.png#lightbox)

4. Xcode'da yeni bir boş tek görünüm iOS projesi oluşturun ve dağıtmak istediğinizden iOS cihazını takın. Ayarlama **takım** eklemiş olduğunuz Apple kimliği açılır. Benzer bir biçimde olmalıdır `your name (Personal Team - your Apple ID)`:

  [![](free-provisioning-images/launchapp2.png "İmzalama kimlik oluşturma")](free-provisioning-images/launchapp2.png#lightbox)

5. Altında **genel > kimlik** bölümünde, paket tanımlayıcısı eşleştiğinden emin olun _tam olarak_ Xamarin.iOS uygulamanızı paket tanımlayıcısını ve dağıtım hedef eşleşen veya değerinden daha düşük emin olun bağlı iOS Cihazınızı. Xcode yalnızca bir açık uygulama kimliği ile bir sağlama profili oluşturacak şekilde bu adım çok önemlidir:

  [![](free-provisioning-images/launchapp5.png "Bir açık uygulama kimliği ile bir sağlama profili oluşturun")](free-provisioning-images/launchapp5.png#lightbox)

6. İmzalama bölümünde **otomatik olarak yönetme imzalama** ve ekibinizin aşağı açılan listeden seçin:

  [![](free-provisioning-images/launchapp6.png "Otomatik olarak yönetme imzalama ve açılır listeden ekibinizin seçin")](free-provisioning-images/launchapp6.png#lightbox)

7. Önceki adımda otomatik olarak bir sağlama profili ve imzalama kimliğinize sizin için oluşturur. Bu, sağlama profili yanındaki bilgi simgesine tıklayarak görüntüleyebilirsiniz:

  [![](free-provisioning-images/launchapp7.png "Sağlama profili görüntüleyin")](free-provisioning-images/launchapp7.png#lightbox)

8. Xcode'da test etmek için çalışma düğmesine tıklayarak Cihazınızı boş uygulamayı dağıtın.

9. Prize takılı aynı aygıt, IDE, dönün ve açmak için Xamarin.iOS proje adına sağ tıklayın **proje seçenekleri** iletişim. İOS paket imzalama bölümüne göz atın ve açıkça kimlik imzalama ve sağlama profili ayarlayın:

  [![](free-provisioning-images/launchapp8.png "Kimlik imzalama ve sağlama profili Ayarla")](free-provisioning-images/launchapp8.png#lightbox)

IDE içinde imzalama kimliğinizi veya doğru sağlama profili göremiyorsanız, onu yeniden başlatmanız gerekebilir.


## <a name="a-namelimitations-limitations"></a><a name="limitations" />Sınırlamaları

Apple bir dizi sınırlama nasıl ve ne zaman, ücretsiz sağlama yalnızca dağıtabileceğiniz emin olduktan uygulamanız bir iOS cihazında çalıştırmak için kullanabileceğiniz uygulanan *,* aygıt. Bunlar bu bölümünde listelenmiştir.

Connect de sınırlıdır ve bu nedenle uygulama depolama ve TestFlight yayımlama gibi hizmetleri iTunes erişimi uygulamalarını serbestçe sağlama geliştiricileri için kullanılamaz. Geçici ve şirket içi bir yol dağıtmak için bir Apple Developer hesabı (Kurumsal veya kişisel) gereklidir.

Bu yolla oluşturulan profiller sağlama imzalama kimlikleri bir yıl sonra bir hafta sonra süresi dolar. Ayrıca, sağlama profilleri yalnızca oluşturulacak açık uygulama kimlikleri ve bu nedenle, gerekecek yönergeleri izleyin [yukarıda](#launching) yüklemek istediğiniz her uygulama için.

Çoğu uygulama hizmetleri için sağlama de serbest sağlama ile mümkün değildir. Şunları içerir:

- Apple Pay
- Oyun Merkezi
- iCloud
- Uygulama içi satın alma
- Anında iletme bildirimleri
- Cüzdan (Passbook idi)

Apple ' tam bir liste sağladığı kendi [desteklenen özelliklerine](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/SupportedCapabilities/SupportedCapabilities.html#//apple_ref/doc/uid/TP40012582-CH38-SW1) Kılavuzu. Uygulama Hizmetleri ile kullanmak için uygulamanızı sağlamak için ziyaret [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) kılavuzları.


## <a name="summary"></a>Özet

Bu kılavuz, avantajları ve bir iOS cihazında uygulamaları yüklemek için ücretsiz sağlama kullanma sınırlamaları incelediniz. Bu ayrıca, bir Xamarin.iOS uygulaması yüklemek için ücretsiz sağlama kullanarak adım adım geçti.

## <a name="related-links"></a>İlgili bağlantılar

- [Cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md)
- [Uygulama hizmetleri için sağlama](~/ios/get-started/installation/device-provisioning/index.md#appservices)
