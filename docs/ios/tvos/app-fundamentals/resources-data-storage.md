---
title: Kaynakları ve veri depolama
description: Bu makalede, kaynakları ve kalıcı veri depolamayı Xamarin.tvOS uygulamasında çalışmak yer almaktadır.
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b4d96ef50498b454da583a955169b9d51c29dd01
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="resources-and-data-storage"></a>Kaynakları ve veri depolama

_Bu makalede, kaynakları ve kalıcı veri depolamayı Xamarin.tvOS uygulamasında çalışmak yer almaktadır._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvOS kaynak sınırlamaları

İOS cihazlar farklı olarak, yeni Apple TV tvOS uygulamalar veya veriler için çok sınırlı kalıcı, yerel depolama sağlar. İçin çok küçük öğeleri (örneğin, kullanıcı tercihlerini) tvOS uygulamanızı hala erişimi `NSUserDefaults` ile bir [500 KB veri sınırının](https://forums.developer.apple.com/message/50696#50696). Ancak, Xamarin.tvOS uygulamanızı daha büyük miktarda bilgi kalıcı olması gerekiyorsa, gerekir depolamak ve bu verilerin alınacağı [iCloud](#iCloud-Data-Storage).

Ayrıca, 200 MB Apple TV uygulamayı boyutunu tvOS sınırlar. Uygulamanız bu boyut ötesindeki kaynaklara gerektiriyorsa, bunlar olması gerekecektir paketlenir ve kullanılarak yüklenen [isteğe bağlı kaynakları](#On-Demand-Resources) (kadar gerekenden 2 GB). Bu sınırlamalara verildiğinde, uygulamanızın kullanıcılara en iyi deneyimi sağlamak için ek varlıklar indirme doğru süresi kritik önem taşır. Daha fazla bilgi için lütfen Apple'nın bkz [isteğe bağlı kaynakları Kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>Kalıcı olmayan yüklemeleri

Her tvOS uygulama varlıkları ve ek kaynaklar için indirdiğiniz geçici önbellek dizini sağlanır. Uygulama hala çalışıyor sürece bu dizin korunur. Ancak, diğer uygulamalar veya sistem kullanımı için yer açmak Apple TV gereksinimleriniz değiştikçe bu önbelleği silinebilir.

Sonuç olarak, uygulamanızı daha önce indirilen içeriği başlatılır sonraki zaman kullanılabilir olmasını durduracak bağlı olamaz. Xamarin.tvOS uygulamanızı her zaman gerekli kaynakları varlığını denetlemek ve gerektiği gibi indirmeniz gerekir.

> [!IMPORTANT]
> Diğer varlıklar ve gerektiği gibi kaynakları yükleme yeteneğini sahip olsa da, öngörülemeyen sonuçlara yol açabilir gibi tüm uygulamanızın önbellek alanı tüketen karşı Apple sizi uyarır.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>Kaynakları yönetme

Yukarıdaki bilgilerin tvOS uygulamaları için kullanılabilir kalıcı olmayan depolama sınırlı nedeniyle belirtildiği gibi bu kısıtlamalar Xamarin.tvOS uygulamanız için harika kullanıcı deneyimi oluşturmak dikkatli planlama gerekir.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>iCloud veri depolama

Apple TV depolama sınırlı olduğundan, sadece değildir var. çok sınırlı kalıcı, yerel depolama uygulamanızı daha önce yüklediğiniz herhangi bir bilgi için İleri çalıştırıldığında kullanılabilecek hiçbir garanti sahiptir.

Sonuç olarak, Xamarin.tvOS uygulamanız tüm kullanıcı verilerini bir iCloud veri deposu depolamanız gerekir. Apple tvOS uygulamalarınız için iki iCloud tabanlı veri depolama seçenekleri sağlar:

- **iCloud anahtar-değer depolama (KVS)** - küçük parça bilgi (1 MB'tan az) uygulamanız gerekebilir olduğunu (kullanıcı tercihleri gibi), iCloud KVS depolama kullanabilir. iCloud KVS verileri otomatik olarak Bulut ve tüm aynı uygulamayı çalıştıran kullanıcının cihazlarını eşitlenmedi. Daha fazla bilgi için lütfen bkz [anahtar-değer depolama](~/ios/data-cloud/introduction-to-icloud.md) bölümünü bizim [icloud giriş](~/ios/data-cloud/introduction-to-icloud.md) belge veya Apple'nın [içinde iCloud için anahtar-değer verileri tasarlama](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) belgeler.
- **CloudKit** - için Apple'nın CloudKit Framework büyük parça bilgi (1 MB'tan fazla), depolama kullanın. İCloud KVS depolama CloudKit veri uygulamanızı (yanı sıra tek bir kullanıcıya özel) tüm kullanıcıları arasında paylaşılabilir. Daha fazla bilgi formunda, lütfen bakın bizim [CloudKit giriş](~/ios/data-cloud/intro-to-cloudkit.md) belgelerine veya Apple'nın [CloudKit Hızlı Başlangıç](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987).

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>İsteğe bağlı kaynaklar

Uygulama içeriğini ve App Store'da barındırılan ve uygulamanızın gerektirdiği şekilde indirilen varlıkları (ayrı), uygulama paketindeki isteğe bağlı kaynaklar sağlar. Bir ek 2 GB kadar veri isteğe bağlı kaynakları kullanarak hizmet edilebilir. Daha küçük olması için ilk uygulama yüklemeyi etkinleştir (tvOS uygulamalar için en fazla 200 MB sınırlı), gerektiği gibi zengin varlıklar hala sağlarken.

İsteğe bağlı kaynakları tvOS uygulama istediğinde, sistem otomatik olarak karşıdan yükleniyor ve bu içeriğin uygulamanın önbellek dizinine depolama yönetir. Uygulamanız bu içeriğin indirilir ve devam etmeden önce kullanılabilir hale beklemeniz gerekir.

Bu kaynaklar, Apple TV birden çok başlatır, uygulamanızın, böylece hızlandırma başlatma döngüsü boyunca önbelleğe alınacak devam edebilir. Ancak, uygulamanızı başlatılır sonraki zaman kullanılabilir olmasını durduracak önceden indirilen içerik üzerinde yeterli olmaz. Bkz: [olmayan kalıcı indirir](#Non-Persistent-Downloads) daha fazla ayrıntı için bölüm üstünde.

Xcode (örneğin, tüm varlıklar için oyun Düzey 2) ilgili içerik verin kaynak etiketi ile ilişkili paketleri oluşturmak için kullanın. Daha sonra uygulamanız, bu kaynak etiketi belirterek isteğe bağlı kaynak ister. Uygulamanızı bir kullanıcı Arabirimi o içeriği belirten kullanıcı sunmalıdır yüklenmektedir. Daha fazla bilgi için lütfen Apple'nın bkz [isteğe bağlı kaynakları Kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

> [!IMPORTANT]
> İsteğe bağlı kaynakları indirmek için uygulamanın olduğu durum sayısı ve ayrı indirme boyutu arasındaki sağ denge için dikkatli olunmalıdır. Kullanıcı oyunlar sürekli yeni içerik indirmek için kesintiye uğrarsa veya tek bir yüklemeye çok fazla sürüyorsa, uygulamanızı engellenen hale gelebilir.




<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede bir Xamarin.tvOS uygulaması tvOS sistem tarafından yerleştirilen boyutu, kaynak ve veri depolama sınırlamaları kapsamına. Bu sınırlamalar ve uygulamanız için harika kullanıcı deneyimi oluşturmak için öneriler geçici olarak çözmek için seçenekler sunulan.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
