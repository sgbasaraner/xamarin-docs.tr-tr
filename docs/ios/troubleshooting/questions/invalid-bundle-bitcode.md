---
title: 'App Store için gönderirken hata: "Geçersiz paket grubu - bitcode katıştırılmış izin verilmeyen seçenekler algılandı"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 393c1ed81c68d21b610781dfe09de97969e031d1
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350930"
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>App Store için gönderirken hata: "Geçersiz paket grubu - bitcode katıştırılmış izin verilmeyen seçenekler algılandı"

watchOS ve tvOS uygulamalar _gerektiren_ App Store için gönderildiğinde bitcode. Oluşturma ve gönderme Xcode 8.3 veya önceki bir sürümünü kullanarak, watchOS ve tvOS uygulamaları (e-posta bildirimi) aşağıdaki hata oluşabilir App Store için karşıya yükleme girişiminde bulunduğunuzda:

>Geçersiz paket grubu - gönderisine bitcode'u katıştırılmış izin verilmeyen seçenekler algıladığından, uygulama işlenemiyor. Xcode'da sağlanan araç zinciri ile uygulama oluşturmuyorsanız olasıdır.

Bu sorunun çözümü, Xcode 9 ve Xamarin.iOS en son sürümünü içeren uygulamalar oluşturmaktır.
