---
title: Objective-C platformları
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8c48a14e5d58f95bf93363e0b354fcf3afd94b44
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="objective-c-platforms"></a>Objective-C platformları

.NET katıştırma çeşitli platformlar Objective-C kodunu oluştururken hedef alabilirsiniz:

* MacOS
* iOS
* tvOS
* [henüz uygulanmadı] watchOS

Platform geçirerek seçili `--platform=<platform>` .NET katıştırmak için komut satırı bağımsız değişkeni.

İOS için oluştururken, tvOS ve watchOS platformları, .NET katıştırma her zaman Xamarin.iOS bu platformlarda gerekli olan çalıştırma desteği kodu çok içerdiğinden Xamarin.iOS, katıştırır framework oluşturun.

Ancak, macOS platformu oluştururken veya oluşturulan framework Xamarin.Mac katıştırmak olup olmadığını seçmek mümkündür. Xamarin.Mac ilişkili derleme Xamarin.Mac.dll (doğrudan veya dolaylı olarak) başvurmuyor ve bu geçirerek seçiliyse katıştırmak değil mümkündür `--platform=macOS` .NET katıştırma aracı.

İlişkili derleme Xamarin.Mac.dll başvuru içeriyorsa, Xamarin.Mac katıştırmak gereklidir ve ayrıca embeddinator kullanmak için hangi hedef framework bilmeniz gerekir.

Üç olası Xamarin.Mac hedef çerçeve vardır: `modern` (daha önce adı `mobile`), `full` ve `system` (her arasındaki farkı Xamarin.Mac içinde 's açıklanan [hedef framework] [ 1] belge), ve her geçirerek seçildiğinde `--platform=macOS-modern`, `--platform=macOS-full` veya `--platform=macOS-system` .NET katıştırma aracı.

[1]: ~/mac/platform/target-framework.md
