---
title: Veri ve bulut Hizmetleri
description: "Sabitlemeyi ve dağıtım kılavuzları"
ms.topic: article
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: b5e250c7c505958c8293970321b6173e6086e7b1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="data-and-cloud-services"></a>Veri ve bulut Hizmetleri


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Veri Erişimi](~/ios/data-cloud/data/index.md)

Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinim vardır. Veri miktarını trivially küçük olmadığı sürece, bu genellikle bir veritabanı ve veri katmanı veritabanı erişimi yönetmek üzere uygulamada gerektirir. iOS "yerleşik" Sqlite veritabanı altyapısı vardır ve verilere erişim SQLite veri sağlayıcısı ile birlikte gelen Xamarin'ın platform tarafından basitleştirilmiştir.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

İOS 5 iCloud depoda API uygulamalarının kullanıcı belgeleri ve uygulamaya özgü verileri merkezi bir konuma kaydedebilir ve kullanıcının tüm cihazlardan bu öğelere erişmesine olanak sağlar.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit framework bu erişim iCloud uygulamaların geliştirilmesini kolaylaştırır. Bu, uygulama verilerini ve varlık hakları yanı sıra uygulama bilgilerini güvenli bir şekilde erişebildiklerinden alınmasını içerir. Bu paketi kişisel bilgi paylaşımı olmadan kullanıcıların iCloud kimlikleri ile uygulamalara erişim sağlayarak kullanıcılara anonim bir katman sağlar.