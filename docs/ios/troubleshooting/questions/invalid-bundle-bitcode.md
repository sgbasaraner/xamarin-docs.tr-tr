---
title: "Uygulama deposuna gönderirken hata: \"Geçersiz paket - seçenekleri bitcode'u katıştırılacak izin verilmiyor gönderisine algılanır\""
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: cacb9040ddc8582490c68bcfd24e80c4c4679eb4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777710"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Uygulama deposuna gönderirken hata: "Geçersiz paket - seçenekleri bitcode'u katıştırılacak izin verilmiyor gönderisine algılanır"

watchOS uygulamalar ve tvOS uygulamalar _gerektiren_ uygulama mağazasında gönderildiğinde bitcode'u. Derleme Xcode 8.3 veya önceki bir sürümünü kullanarak watchOS ve tvOS uygulamalarını gönderme, aşağıdaki hata (üzerinden e-posta bildirimi) ortaya çıkabilir ve uygulama mağazasında yüklemeye çalışırken:

>Geçersiz paket - seçenekleri bitcode'u katıştırılacak izin verilmiyor gönderisine algıladığından uygulama işlenemiyor. Xcode'da sağlanan zincirinin ile uygulama oluşturmuyorsanız olasıdır.

Bu sorun için çözüm Xcode 9 ve Xamarin.iOS en son sürümünü uygulamalarla oluşturmaktır.
