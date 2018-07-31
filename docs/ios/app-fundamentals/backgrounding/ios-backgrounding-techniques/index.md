---
title: iOS arka planda işleme teknikleri
description: "Bu belge bağlantıları ios'ta çeşitli backgrounding teknikleri açıklar kılavuzlarına: arka plan görevleri, arka plan aktarım hizmetini, arka planda getirme ve uzak bildirimler."
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 33c4613e3698556b41a0d01acf2a4ac359ca5dd8
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350975"
---
# <a name="ios-backgrounding-techniques"></a>iOS arka planda işleme teknikleri

Aşağıdaki bölümlerde, biz varolan backgrounding seçenekleri yanı sıra aşağıdaki iOS özellikleri keşfedin:

-  [Fırsatçı arka plan görevleri](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -cihaz için başka bir işleme uyanık olduğunda fırsatçı öbekler halinde arka plan görevleri çalıştırarak pil ömrü korumak.
-  [Arka plan Aktarım Hizmeti](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - güvenilir bir şekilde karşıya yükleyin ve ağ durumunu veya dosya boyutundan bağımsız olarak dosyalarını indirin.
-  [Arka planda getirme](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -arka planda bir uygulamadan sistem tarafından belirlenen aralıklarla yenileyin.
-  [Uzak bildirimler](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -kullanıcı uygulama kullanıcıya bildirim ya da sessizce güncelleştirmek için bir seçenek açılmadan önce arka planda içerik güncelleştirmeleri tetiklemeye yönelik anında iletme bildirimleri kullanın.
-  **Arka plan, kullanıcı Arabirimi güncelleştirmeleri** - kullanıcı için uygulama UI hazırlamak ve uygulamanın anlık görüntü, arka plan gelen tüm güncelleştirme.
