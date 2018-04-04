---
title: Uygulama dağıtım genel bakış
description: Bu belge Xamarin.tvOS uygulaması için kullanılabilen dağıtım teknikleri genel bir bakış sağlar ve konusunda daha ayrıntılı belgeler için bir işaretçi olarak görev yapar.
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: e5be0bef158c87fe06516d9a58e34c741e6e14b1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="app-distribution-overview"></a>Uygulama dağıtım genel bakış

_Bu belge Xamarin.tvOS uygulaması için kullanılabilen dağıtım teknikleri genel bir bakış sağlar ve konusunda daha ayrıntılı belgeler için bir işaretçi olarak görev yapar._


Xamarin.tvOS uygulamanızı geliştirilen sonra yazılım geliştirme yaşam döngüsü sonraki adımda uygulamanızı, kullanıcılara dağıtmak için aşağıdaki çizimde vurgulanan bölümünde gösterilen aynıdır:


[![Yazılım geliştirme yaşam döngüsü genel bakış](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)


Apple Xamarin.tvOS tarafından desteklenmeyen bir tvOS uygulama dağıtmak için aşağıdaki yöntemleri sağlar:

1. [**Uygulama Mağazası**](#Apple-TV-App-Store-Distribution)
2. [**Şirket içi (Kurumsal)**](#In-House-Distribution) 
2. [**Ad Hoc**](#Ad_Hoc_Distribution) 

Bu senaryolar uygulamaları olmasını gerektirir uygun kullanılarak sağlanan *sağlama profili*. Sağlama profilleri kod imzalama uygulama ve hedeflenen dağıtım mekanizması kimliğini yanı sıra bilgiler içeren dosyalardır. Uygulama mağazası dağıtım için bunlar Ayrıca uygulama dağıtılabilir hangi aygıtlar hakkında bilgiler içerir.

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Apple TV App Store dağıtım

Bu, tvOS uygulamaları Apple TV cihazlarda tüketicileri dağıtılan ana yoludur. Apple TV App Store gönderilen tüm uygulamalar, Apple tarafından onay gerektirir.

Uygulamaları, uygulama mağazası adlı bir portal üzerinden gönderildiğinde *iTunes Bağlan*. Lütfen bakın bizim [uygulamanızı iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) kılavuzda aşağıdaki konular hakkında bilgi için:

- Yönetme anlaşmaları, vergi ve bankacılık.
- Bir iTunes Connect kaydı oluşturma.
- Uygulama videolar ve ekran görüntüleri yönetme.
- Uygulama adı, açıklaması, yenilikler, yönetme anahtar sözcükleri ve URL'ler.
- Genel Uygulama bilgileri sağlama.
- Oyun merkezi bilgi koruma.
- Uygulama gözden geçirme bilgi koruma.
- Fiyatlandırma bilgileri sağlama.
- Uygulama içi satın alma bilgileri sağlama.
- Uygulama incelemeler görüntüleme.

Özellikle tvOS için yazılmaz olsa da, bu belgede ayarlamak ve uygulamanızı yayımlama Apple TV uygulama mağazasında hazırlamak için bu portalı kullanmak nasıl hakkında bilgiler sağlar. Olup bir iOS veya tvOS uygulaması kuruluyor birçok aynı adımları gereklidir.

Yukarıda listelenen tüm adımları tamamladıktan sonra bkz bizim [, tvOS uygulama iTunes Connect yapılandırma](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) gerekli olacak tvOS uygulama belirli bilgileri eklemek için.

Yalnızca dikkate almak önemlidir ait geliştiriciler **Apple Developer Program** iTunes Bağlan erişimi. Üyeleri **Apple Geliştirici Kurumsal programı** erişim izniniz yok.

Apple TV App Store Xamarin.tvOS uygulamanıza gönderme sorunlarınız varsa lütfen bkz bizim [sorun giderme](~/ios/tvos/troubleshooting.md) Kılavuzu. Karşılaşabileceğiniz bazı bilinen sorunlar ve Xamarin.tvOS çözmek nasıl içerir.

Daha fazla bilgi için lütfen ziyaret [Apple TV App Store yayımlama](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) Kılavuzu.

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>Şirket içi dağıtım

Adlandırılan *kurumsal dağıtım*, şirket içi dağıtım verir üyeleri **Apple Geliştirici Kurumsal programı** dahili olarak diğer üyeleriyle aynı kuruluş uygulamaları dağıtmak için. Şirket içi dağıtım değil App Store gözden geçirme gerektiren ve sınır uygulamanın yüklenebileceği cihaz sayısına sahip avantajları vardır. Ancak, dikkat edilecek önemli **Apple Geliştirici Kurumsal programı** musunuz **değil** iTunes Bağlan erişimi vardır ve bu nedenle edinmediyseniz uygulama dağıtmaktan sorumludur.

Kurulum ve uygulamanız şirket içinde dağıtmak nasıl alma ile ilgili daha fazla bilgi için lütfen başvurmak [şirket içi Dağıtım Kılavuzu'na](~/ios/deploy-test/app-distribution/in-house-distribution.md). Bu belgede iOS özeldir, ancak aynı teknikleri tvOS uygulamalar için kullanılır.

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>Geçici dağıtım

Xamarin.tvOS uygulamalar hem de kullanılabilir geçici dağıtım aracılığıyla kullanıcı olarak test olabilir **Apple Developer Program**ve **Apple Geliştirici Kurumsal programı**ve en fazla 100 Apple TV cihazların sağlar sınanacak. İTunes Bağlan bir seçenek olmadığı durumlarda en iyi kullanımı geçici dağıtım için dağıtım şirketinizdeki bir durumdur.

Kurulum ve şirket içinde uygulamanızı dağıtmak nasıl alma ile ilgili daha fazla bilgi için lütfen başvurmak [geçici Dağıtım Kılavuzu'na](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). Yeniden, bu belgede iOS belirli ancak aynı teknikleri tvOS uygulamalar için kullanılır.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, Xamarin.tvOS uygulamaları için kullanılabilir dağıtım mekanizmaları kısa bir genel bakış verdi. Apple TV App Store dağıtım geçici ve şirket içinde sunulmuştur ve daha ayrıntılı bilgi için bağlantılar sağlanmaktadır.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
