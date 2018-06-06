---
title: iOS Backgrounding teknikleri
description: 'Bu belgede iOS çeşitli backgrounding teknikleri açıklar kılavuzları için bağlantıları: arka plan görevleri, arka plan Aktarım Hizmeti, arka planda getirmeye ve uzak bildirimler.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ebf3c07a319a79994093f89f8e54f4cba7402533
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783761"
---
# <a name="ios-backgrounding-techniques"></a>iOS Backgrounding teknikleri

Aşağıdaki bölümlerde, biz varolan backgrounding seçenekleri yanında aşağıdaki iOS özellikleri inceleyeceksiniz:

-  [Fırsatçılıktan arka plan görevleri](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -cihaz için başka bir işlem uyanık olduğunda fırsatçılıktan yığınlar halinde arka plan görevleri çalıştırarak pil ömrünün korunmasını.
-  [Arka plan Aktarım Hizmeti](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - güvenilir bir şekilde karşıya yükleme ve ağ durum ya da dosya boyutu bakılmaksızın dosyalarını yükleyin.
-  [Arka plan Fetch](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -arka planda bir uygulamadan sistem tarafından belirlenen aralıklarla yenileyin.
-  [Uzak bildirimler](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -kullanıcı kullanıcıya bildir ya da sessizce güncelleştirmek için bir seçenek ile uygulama açılmadan önce içerik güncelleştirmeleri arka planda tetiklemek için anında iletme bildirimleri kullanın.
-  **Arka plan UI güncelleştirmeleri** - uygulama kullanıcı Arabirimi kullanıcı için hazırlamak ve uygulamanın anlık görüntü, arka plan gelen tüm güncelleştirme.
