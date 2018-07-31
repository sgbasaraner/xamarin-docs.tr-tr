---
title: 'İOS 9 uygulamam neden başarısız: System.Exception: Objective-C nesnesi hazırlanamadı?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 93666fcb39f0cd717c14eb07e6407801e9f0642e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350605"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>İOS 9 uygulamam neden başarısız: System.Exception: Objective-C nesnesi hazırlanamadı?

Bu formu, bir hata görebilirsiniz:

> System.Exception: Objective-C nesnesi hazırlanamadı başarısız... Bu nesne için mevcut bir yönetilen örnek bulunamadı...

İOS 9 API değişiklikler artık temel alınan API olarak yönetilmeyen kod çağırmak beklerken bir geri çağırma Oluşturucu kullanılmasını gerektirir. Geri çağırma Oluşturucusu sınıfa eklemek için aşağıdaki satırı kullanın: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Sonraki Adımlar

Bizimle iletişim kurun ya da Yukarıdaki bilgilerin kullanan daha sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [Xamarin için hangi destek seçenekleri mevcuttur?](~/cross-platform/troubleshooting/support-options.md) önerileri olan iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Gerekirse yeni hata bildirin. 
