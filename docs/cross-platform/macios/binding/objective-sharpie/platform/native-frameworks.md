---
title: "Yerel çerçeveler bağlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4588a879452b4ee49535ac8882977be2c064a43f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-native-frameworks"></a>Yerel çerçeveler bağlama

Bazen yerel kitaplığı olarak dağıtılmış bir [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Amaç Sharpie düzgün bağlama üzerinden çerçeveler tanımlanan kullanışlı bir özellik sağlar `-framework` seçeneği.

Örneğin, bağlama [Adobe Creative SDK Framework](https://creativesdk.adobe.com/downloads.html) iOS basittir için:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

Bazı durumlarda, bir çerçeve belirleyeceksiniz bir **Info.plist** framework hangi SDK karşı belirten derlenmiş. Bu bilgiler varsa ve ACE'si `-sdk` seçeneği geçirilir, hedefi Sharpie Infer onu çerçevesinden 's **Info.plist** (ya da `DTSDKName` anahtar veya bir birleşimini `DTPlatformName` ve `DTPlatformVersion`anahtarları).

`-framework` Seçeneği açık üstbilgi dosyaları geçirilecek izin vermez. Şemsiyesi üstbilgi dosyası framework adına göre kural tarafından seçilir. Bir şemsiyesi üstbilgisi bulunamıyor, hedefi Sharpie framework bağlamak vermeyeceğinden ve clang için herhangi bir framework bağımsız değişkeni ile birlikte ayrıştırmak için doğru şemsiyesi üstbilgi dosyaları sağlayarak bağlama el ile gerçekleştirmelisiniz ( gibi`-F`framework arama yolu seçeneği).

Başlık altında belirtme `-framework` yalnızca bir kısayol. Aşağıdaki bağlama bağımsız değişkenleri aynı `-framework` yukarıdaki toplu özelliktir.
Özel önem derecesi olan `-F .` clang için sağlanan framework arama yolu (komutun bir parçası olarak gerekli alanı ve süresi, unutmayın).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

