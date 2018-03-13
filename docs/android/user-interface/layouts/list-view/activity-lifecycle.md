---
title: "ListView ve etkinlik yaşam döngüsü"
ms.topic: article
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: b8ee113a321dbc84cf12a7ef4bb5084c5307115b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView ve etkinlik yaşam döngüsü

Etkinlikleri belirli durumlarında, başlangıç gibi uygulama çalışmalarınız olarak çalıştığından, duraklatıldıktan ve durdurulmasını gidin. Daha fazla bilgi ve durumu geçişleri işleme hakkında belirli yönergeler için bkz: [etkinlik yaşam döngüsü Öğreticisi](~/android/app-fundamentals/activity-lifecycle/index.md).
Etkinlik yaşam döngüsü ve Yerleştir anlamak önemlidir, `ListView` doğru konumda kodu.

Tüm bu belgedeki örneklerde 'kurulum görevlerini' etkinliğin içinde yerine `OnCreate` yöntemi ve (gerektiğinde) 'erdirme' gerçekleştirmek `OnDestroy`. Genellikle örnekler değiştirmeyin, küçük veri kümeleri kullandığından, verilerin daha sık yeniden yüklenmesi gerekli değildir.

Verilerinizi sık değişen veya çok miktarda bellek kullanıyorsa, ancak bunu doldurmak ve yenilemek için farklı yaşam döngüsü yöntemleri kullanmak uygun olabilir, `ListView`. Örneğin, temel alınan veri sürekli değişen (veya diğer etkinlikler güncelleştirmeleri etkilenebilir varsa)'deki bağdaştırıcı oluşturma sonra `OnStart` veya `OnResume` en son verileri etkinlik gösterilir her zaman görüntülenir güvence altına alır.

Bağdaştırıcı bellek veya yönetilen bir imleç gibi kaynakları kullanıyorsa, bu kaynakları burada bunlar (ör. örneği tamamlayıcı yöntemi de serbest unutmayın oluşturulan nesnelerini `OnStart` , içinde dönüşüme gönderilebilir `OnStop`).


## <a name="configuration-changes"></a>Yapılandırma Değişiklikleri

Bu yapılandırma değişiklikleri unutulmaması önemlidir; &ndash; özellikle, döndürme ve klavye görünürlük ekran &ndash; yok ve yeniden oluşturulması için geçerli etkinliği neden olabilir (kullanarak aksi belirtilmedikçe `ConfigurationChanges` özniteliği). Normal koşullar altında bir cihaz döndürme oluşturacağını yani bir `ListView` ve `Adapter` yeniden oluşturulması ve (kod yazdığınız sürece `OnPause` ve `OnResume`) kaydırma konumunu ve satır seçimi durumları kaybolacak.

Aşağıdaki öznitelik yok ve yapılandırma değişikliklerini sonucunda yeniden aktivite engeller:

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

Etkinlik sonra kılmalıdır `OnConfigurationChanged` bu değişiklikleri uygun şekilde yanıt vermesi. Yapılandırma değişiklikleri nasıl ele alınacağını hakkında daha fazla ayrıntı için belgelere bakın.

