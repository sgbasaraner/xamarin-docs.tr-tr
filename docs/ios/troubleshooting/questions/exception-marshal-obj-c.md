---
title: 'İOS 9 uygulamam neden başarısız: System.Exception: Objective-C nesnesi çağrılamadı?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f7382ac963249a3f3646a917d8700e3a12873ec9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30775981"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>İOS 9 uygulamam neden başarısız: System.Exception: Objective-C nesnesi çağrılamadı?

Bu biçimde bir hata görebilirsiniz:

> System.Exception: Objective-C nesne sıralamakta başarısız... Bu nesne için var olan bir yönetilen örneği bulunamadı...

API iOS 9 artık temel API yönetilmeyen kodu çağırma beklerken bir geri çağırma Oluşturucu kullanılmasını gerektirir. Geri çağırma Oluşturucusu sınıfa eklemek için aşağıdaki satırını kullanın: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Sonraki Adımlar

Bizimle iletişime geçin veya bile Yukarıdaki bilgilerin kullanılarak sonra bu sorun devam ederse lütfen görmek için daha fazla yardım için [için Xamarin hangi destek seçenekleri kullanılabilir?](~/cross-platform/troubleshooting/support-options.md) önerileri, iletişim seçenekleri hakkında bilgi için nasıl yanı sıra Yeni bir hatanın gerekirse dosya. 
