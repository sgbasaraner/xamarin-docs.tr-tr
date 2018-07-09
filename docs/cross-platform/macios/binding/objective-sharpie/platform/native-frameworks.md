---
title: Yerel çerçeveleri bağlama
description: Bu belgenin amacı Sharpie kullanmayı açıklar kullanıcının framework seçeneği'bağlama kitaplığı oluşturmak için Dağıtılmış bir çerçeve.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: ca103ee027597813be51e126aaa05f9aa969af35
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855103"
---
# <a name="binding-native-frameworks"></a>Yerel çerçeveleri bağlama

Bazen yerel kitaplığı olarak dağıtılmış bir [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Amaç Sharpie çerçeveleri aracılığıyla tanımlanan düzgün bir şekilde bağlama için kullanışlı bir özellik sağlar `-framework` seçeneği.

Örneğin, bağlama [Adobe Creative SDK'sı çerçevesi](https://creativesdk.adobe.com/downloads.html) için iOS basittir:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

Bazı durumlarda, bir çerçeve belirten bir **Info.plist** framework hangi SDK karşı gösteren derlenmelidir. Bu bilgiler varsa ve ACE'si `-sdk` seçeneği geçirilir, hedefi Sharpie gerçekleştirip bu çerçevenin **Info.plist** (ya da `DTSDKName` anahtar veya bir birleşimini `DTPlatformName` ve `DTPlatformVersion`anahtarları).

`-framework` Seçeneği geçirilecek açık üst bilgi dosyaları izin vermez. Genel üst bilgi dosyası framework ada göre kural tarafından seçilir. Bir terimdir üst bilgisi bulunamıyor, framework bağlamak hedefi Sharpie çalışmayacak ve bağlama clang için herhangi bir framework bağımsız değişkeni ile birlikte ayrıştırmak için doğru terimdir üst bilgi dosyaları sağlayarak el ile yapmanız gerekir ( gibi`-F`çerçeve arama yolu seçeneği).

Başlık altında belirtme `-framework` bir kısayol bulunur. Şu bağlama bağımsız değişkenler aynı olan `-framework` yukarıdaki toplu özellik.
Özel önem derecesi olan `-F .` clang sağlanan çerçeve arama yolu (boşluk ve süresi, komutun bir parçası olarak gerekli olan unutmayın).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)

