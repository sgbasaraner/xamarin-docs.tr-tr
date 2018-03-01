---
title: "Uygulama deposuna gönderirken hata: \"Geçersiz paket - seçenekleri bitcode'u katıştırılacak izin verilmiyor gönderisine algılanır\""
ms.topic: article
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b8ce643d392945e7e746c28b13063a2b0b7ebb48
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Uygulama deposuna gönderirken hata: "Geçersiz paket - seçenekleri bitcode'u katıştırılacak izin verilmiyor gönderisine algılanır"

watchOS uygulamalar ve tvOS uygulamalar _gerektiren_ uygulama mağazasında gönderildiğinde bitcode'u. Derleme Xcode 8.3 veya önceki bir sürümünü kullanarak watchOS ve tvOS uygulamalarını gönderme, aşağıdaki hata (üzerinden e-posta bildirimi) ortaya çıkabilir ve uygulama mağazasında yüklemeye çalışırken:

>Geçersiz paket - seçenekleri bitcode'u katıştırılacak izin verilmiyor gönderisine algıladığından uygulama işlenemiyor. Xcode'da sağlanan zincirinin ile uygulama oluşturmuyorsanız olasıdır.

Bu sorun için çözüm Xcode 9 ve Xamarin.iOS en son sürümünü uygulamalarla oluşturmaktır.
