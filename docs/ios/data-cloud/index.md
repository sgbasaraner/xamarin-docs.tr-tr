---
title: Veri ve bulut Hizmetleri
description: "Sabitlemeyi ve dağıtım kılavuzları"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 5814936289164f14a07b33c219ad1fd01b00473b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="data-and-cloud-services"></a>Veri ve bulut Hizmetleri


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Veri Erişimi](~/ios/data-cloud/data/index.md)

Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinim vardır. Veri miktarını trivially küçük olmadığı sürece, bu genellikle bir veritabanı ve veri katmanı veritabanı erişimi yönetmek üzere uygulamada gerektirir. iOS "yerleşik" Sqlite veritabanı altyapısı vardır ve verilere erişim SQLite veri sağlayıcısı ile birlikte gelen Xamarin'ın platform tarafından basitleştirilmiştir.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

İOS 5 iCloud depoda API uygulamalarının kullanıcı belgeleri ve uygulamaya özgü verileri merkezi bir konuma kaydedebilir ve kullanıcının tüm cihazlardan bu öğelere erişmesine olanak sağlar.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit framework bu erişim iCloud uygulamaların geliştirilmesini kolaylaştırır. Bu, uygulama verilerini ve varlık hakları yanı sıra uygulama bilgilerini güvenli bir şekilde erişebildiklerinden alınmasını içerir. Bu paketi kişisel bilgi paylaşımı olmadan kullanıcıların iCloud kimlikleri ile uygulamalara erişim sağlayarak kullanıcılara anonim bir katman sağlar.