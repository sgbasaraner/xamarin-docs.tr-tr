---
title: Xamarin.iOS uygulaması dağıtımı genel bakış
description: Bu belge Xamarin.iOS uygulamaları için kullanılabilir dağıtım teknikleri genel bir bakış sağlar ve konusunda daha ayrıntılı belgeler için bir işaretçi olarak görev yapar.
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 83c58fa430a0cf21e8b86e56b9dea04b3cad6d82
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784774"
---
# <a name="xamarinios-app-distribution-overview"></a>Xamarin.iOS uygulaması dağıtımı genel bakış

_Bu belge Xamarin.iOS uygulamaları için kullanılabilir dağıtım teknikleri genel bir bakış sağlar ve konusunda daha ayrıntılı belgeler için bir işaretçi olarak görev yapar._

Bir Xamarin.iOS uygulaması geliştirmiştir sonra yazılım geliştirme yaşam döngüsü sonraki adımda, kullanıcılara uygulama dağıtmak için aşağıdaki çizimde vurgulanan bölümünde gösterilen aynıdır:


[![](images/publishingdiagram.png "İOS uygulaması geliştirmiştir sonra sonraki adım, kullanıcılara uygulama dağıtmak için bu diyagrama vurgulanan bölümünde gösterildiği gibi olur")](images/publishingdiagram.png#lightbox)


Apple Xamarin.iOS tarafından desteklenmeyen bir iOS uygulamasını dağıtmak için aşağıdaki yöntemleri sağlar:

1. [**Uygulama Mağazası**](#App_Store_Distribution)
2. [**Şirket içi (Kurumsal)**](#In-House_Distribution)
2. [**Geçici**](#Ad_Hoc_Distribution)

Bu senaryolar uygulamaları olmasını gerektirir uygun kullanılarak sağlanan *sağlama profili*. Sağlama profilleri kod imzalama uygulama ve hedeflenen dağıtım mekanizması kimliğini yanı sıra bilgiler içeren dosyalardır. Uygulama mağazası dağıtım için bunlar Ayrıca uygulama dağıtılabilir hangi aygıtlar hakkında bilgiler içerir.

<a name="App_Store_Distribution"/>

## <a name="app-store-distribution"></a>Uygulama mağazası dağıtım

Bu, iOS uygulamaları iOS cihazlarda tüketicileri dağıtılan ana yoludur. Uygulama mağazasında gönderilen tüm uygulamalar, Apple tarafından onay gerektirir.

Uygulamaları, uygulama mağazası adlı bir portal üzerinden gönderildiğinde *iTunes Bağlan*. [Uygulamanızı iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) kılavuzu ayarlamak ve bir Xamarin.iOS uygulaması yayımlama uygulama mağazasında hazırlamak için bu portalı kullanmak hakkında daha fazla bilgi sağlar.

Yalnızca dikkate almak önemlidir ait geliştiriciler **Apple Developer Program** iTunes Bağlan erişimi. Üyeleri **Apple Geliştirici Kurumsal programı** erişim izniniz yok.

Daha fazla bilgi için lütfen ziyaret [App Store dağıtım](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) Kılavuzu.

<a name="In-House_Distribution"/>

## <a name="in-house-distribution"></a>Şirket içi dağıtım

Adlandırılan *kurumsal dağıtım*, şirket içi dağıtım verir üyeleri **Apple Geliştirici Kurumsal programı** dahili olarak diğer üyeleriyle aynı kuruluş uygulamaları dağıtmak için. Şirket içi dağıtım değil App Store gözden geçirme gerektiren ve sınır uygulamanın yüklenebileceği cihaz sayısına sahip avantajları vardır. Ancak, dikkat edilecek önemli **Apple Geliştirici Kurumsal programı** musunuz **değil** iTunes Bağlan erişimi vardır ve bu nedenle edinmediyseniz uygulama dağıtmaktan sorumludur.

Kurulum ve şirket içinde bir uygulama dağıtmak nasıl alma ile ilgili daha fazla bilgi için lütfen başvurmak [şirket içi Dağıtım Kılavuzu'na](~/ios/deploy-test/app-distribution/in-house-distribution.md).

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>Geçici dağıtım

Xamarin.iOS uygulamaları hem de kullanılabilir geçici dağıtım aracılığıyla kullanıcı olarak test **Apple Developer Program**ve **Apple Geliştirici Kurumsal programı**ve en fazla 100 iOS sağlar sınanacak cihazlar. İTunes Bağlan bir seçenek olmadığı durumlarda en iyi kullanımı geçici dağıtım için dağıtım şirket içindeki bir durumdur.

Kurulum ve şirket içinde bir uygulama dağıtmak nasıl alma ile ilgili daha fazla bilgi için lütfen başvurmak [geçici Dağıtım Kılavuzu'na](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md).

## <a name="summary"></a>Özet

Bu makalede, Xamarin.iOS uygulamaları için kullanılabilir dağıtım mekanizmaları kısa bir genel bakış verdi. İTunes App Store'da dağıtım geçici ve şirket içinde sunulmuştur ve daha ayrıntılı bilgi için bağlantılar sağlanmaktadır.

## <a name="related-links"></a>İlgili bağlantılar

- [App Store Dağıtımı](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Uygulama iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store’da Yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Şirket içi dağıtım](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Geçici Dağıtım](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist Dosyası](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA Desteği](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
