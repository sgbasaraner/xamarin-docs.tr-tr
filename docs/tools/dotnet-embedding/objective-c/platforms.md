---
title: Objective-C platformları
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: a783dc554f54922f4fa4f99a43d83c4c864ef681
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="objective-c-platforms"></a>Objective-C platformları


.NET katıştırma çeşitli platformlar Objective-C kodunu oluştururken hedef alabilirsiniz:

* macOS
* iOS
* tvOS
* [henüz uygulanmadı] watchOS

Platform geçirerek seçili `--platform=<platform>` embeddinator komut satırı bağımsız değişkeni.

İOS, tvOS ve watchOS platformlar için oluştururken, embeddinator her zaman Xamarin.iOS bu platformlarda gerekli olan çalıştırma desteği kodu çok içerdiğinden Xamarin.iOS, katıştırır framework oluşturun.

Ancak, macOS platformu oluştururken veya oluşturulan framework Xamarin.Mac katıştırmak olup olmadığını seçmek mümkündür. Xamarin.Mac ilişkili derleme Xamarin.Mac.dll (doğrudan veya dolaylı olarak) başvurmuyor ve bu geçirerek seçiliyse katıştırmak değil mümkündür `--platform=macOS` embeddinator için.

İlişkili derleme Xamarin.Mac.dll başvuru içeriyorsa, Xamarin.Mac katıştırmak gereklidir ve ayrıca embeddinator kullanmak için hangi hedef framework bilmeniz gerekir.

Üç olası Xamarin.Mac hedef çerçeve vardır: `modern` (daha önce adı `mobile`), `full` ve `system` (her arasındaki farkı Xamarin.Mac içinde 's açıklanan [hedef framework] [ 1] belge), ve her geçirerek seçildiğinde `--platform=macOS-modern`, `--platform=macOS-full` veya `--platform=macOS-system` embeddinator için.

[1]: ~/mac/platform/target-framework.md
