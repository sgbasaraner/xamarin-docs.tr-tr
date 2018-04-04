---
title: Ek watchOS 3 çerçeveleri değişiklikleri
description: Bu makalede, ek, küçük değişiklikler veya watchOS 3 varolan çerçeveyi geliştirmeler kapsar.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 572aaff9d010ec1ec1f48db2878859e445e2fb20
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="additional-watchos-3-frameworks-changes"></a>Ek watchOS 3 çerçeveleri değişiklikleri

_Bu makalede, ek, küçük değişiklikler veya watchOS 3 varolan çerçeveyi geliştirmeler kapsar._

İOS için önemli değişiklikler yanı sıra Apple değişikliklerini ve birkaç varolan çerçeveleri geliştirmeleri watchOS 3 yaptı.


## <a name="core-data"></a>Çekirdek veri

Aşağıdaki geliştirmeleri olması için yapmış çekirdek veri çerçevesi izleme OS 3 için:

- Kök [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) eşzamanlı hatalı ve seri hale getirme getirme nesnelerini destekler.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) sınıfı SQLite veri depolarına havuzu korur.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) NİLEME günlük modu desteği yeni sorgu oluşturma SQLite veri depolarında nesneleriyle özellik burada Yönetilen Nesne bağlamları (MOC) gelecekteki getirme için belirli veritabanı sürümleriyle sabitlenmelidir ve hataya neden olan işlemleri.
- Üst düzey kullanarak `NSPersistenceContainer` başvuru `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) ve diğer temel verilere yapılandırma kaynakları.
- Birkaç yeni kullanışlı yöntemler için eklenene `NSManagedObject` öğesinden gerçekleştirmek ve alt sınıflar oluşturmak kolaylaştırır.

Daha fazla bilgi için lütfen Apple'nın bkz [çekirdek veri Framework başvurusu](https://developer.apple.com/reference/coredata).


## <a name="core-motion"></a>Çekirdek hareket

Aşağıdaki geliştirmeleri olması için yapmış çekirdek hareket framework izleme OS 3 için:

- Yeni cihaz hareket olay hareket ve yönlendirmesini güncelleştirmeleri sağlamak üzere jiroskop ve accelerometer kullanır. uygulama bu (en fazla 100 Hz oranlarda) güncelleştirilmiş kaydedebilirsiniz.
- Yeni Pedometer olay hızlı, kullanıcının durduğu zaman gerçek zamanlı bildirimler ve çalışan sürdürür sağlar. Kullanım [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) ön plan veya arka plan pedometer olaylarını kaydetmek için.


## <a name="foundation"></a>Foundation

Aşağıdaki geliştirmeleri olması için yapmış Foundation çerçevesi izleme OS 3 için:

- Yeni [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) aralıkları karşılaştırma ve aralığı kesişimlerini için test etme için süreleri gibi tarih ve saat aralığı hesaplamaları yapmak için sınıf.
- Çeşitli yeni özellikler eklendi [NSLocal](https://developer.apple.com/reference/foundation/nslocale) yerel bilgileri ve kullanılabilir görüntü biçimlerini edinmeye sınıfı.
- Yeni [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) farklı birimleri, ölçü (ölçü birimi arasında) Dönüştür veya farklı UOMs değerleri hesaplamalar için sınıf.
- Yeni [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) son kullanıcıya görüntüleme için yerelleştirilmiş ölçümleri biçimlendirmek için sınıf.
- Yeni [NSUnit](https://developer.apple.com/reference/foundation/nsunit) ve [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) belirli UOMs temsil eden sınıf.


## <a name="healthkit"></a>HealthKit

Aşağıdaki geliştirmeleri olması için yapmış HealthKit framework izleme OS 3 için:

- Yeni [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) belirlemek için sınıf `ActivityType` ve `LocationType` bir etkinlik olarak.
- Yeni [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) ve `WheelchairUse` yöntemi [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) sınıfı eklendi Tekerlekli ile çalışmak için ilişkili sistem durumu verileri.
- Yeni meta veri anahtarları hava durumu türleri için eklenmiştir (gibi `HKWeatherConditionClear` ve `HKWeatherConditionCloudy`) ve etkinlik türleri (gibi `HKWorkoutActivityTypeFlexibility` ve `HKWorkoutActivityTypeWheelchairRunPace`) eklenmiştir.


## <a name="homekit"></a>HomeKit

Aşağıdaki geliştirmeleri olması için yapmış HomeKit framework izleme OS 3 için:

- Görüntülemek ve HomeKit ile etkileşim özelliği bağlı IP eklenen kameralar.
- Birkaç yeni hizmetler ve özelliklere eklendi.
- Daha fazla içerik ve birincil ve Bağlantı Hizmetleri, Donatılar yapılandırmasını eklendi.


## <a name="passkit"></a>PassKit

Aşağıdaki geliştirmeleri olması için yapmış PassKit framework izleme OS 3 için:

- Güvenli, uygulama ödemeler üzerinde Apple Watch fiziksel mal ve hizmetlerin desteklemek için framework genişletir.
- Aşağıdaki sınıflar artık kullanılabilir: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) ve [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>Uıkit

Aşağıdaki geliştirmeleri olması için yapmış Uıkit framework izleme OS 3 için:

- Dinamik tür etiketler desteklemek için metin alanları ve metin kutuları yeni kullanma `PreferredFontForTextStyle` yöntemi `UIFont` sınıfı.
- `ColorWithDisplayP3` Yöntemi geniş renk desteklemek için eklenmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [Başlarken (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [WatchOS 3 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
