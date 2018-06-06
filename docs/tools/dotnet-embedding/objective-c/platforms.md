---
title: Objective-C platformları
description: Bu belgede .NET katıştırma Objective-C kodunu ile çalışırken hedefleyebilirsiniz çeşitli platformlar açıklanmaktadır. MacOS, iOS, tvOS ve watchOS anlatılmaktadır.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 6eeb776959d1a2a37d67bfae6603971d0e5a22b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793897"
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
