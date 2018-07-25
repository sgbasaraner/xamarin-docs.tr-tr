---
title: Xamarin.Mac'te AVAudioPlayer ile ses çalma
description: Bu belge, bir Xamarin.Mac uygulamasını AVAudioPlayer ile ses çal açıklar. Bu bir yüksek düzeyde ve daha tam olarak ele diğer belgelere bağlantılar AVAudioPlayer açıklanır.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 03a0207ce8c742f0ca98ab6c75ed3e7b2f37d09e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241364"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Xamarin.Mac'te AVAudioPlayer ile ses çalma

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer hakkında

`AVAudioPlayer` Sınıfı, kayıttan yürütme ses veri kullanılır, bellek veya dosyası. Apple, ağ akışını yapıyor ya da düşük gecikme süresi ses g/ç gerektiren uygulamanızda ses yürütmek için bu sınıfın kullanılmasını önerir.

Kullanabileceğiniz `AVAudioPlayer` aşağıdakileri yapmak için sınıfı:

- İsteğe bağlı döngü ile herhangi bir süre sesler yürütün.
- İsteğe bağlı eşitleme ile aynı anda birden çok ses yürütün.
- Birim, kayıttan yürütme hızını ve her çalmasını stereo konumlandırma denetler.
- İleri veya Geri Sar gibi özellikleri destekler.
- Kayıttan yürütme düzeyi ölçümü verilerini alın.

`AVAudioPlayer` iOS, tvOS ve macOS .aif, .wav veya .mp3 gibi tarafından sağlanan herhangi bir ses biçimi sesleri destekler.

## <a name="playing-sounds-in-macos"></a>MacOS ses çalma

İOS, macOS, iOS olarak aynı ses araç sınıfları desteklediğinden, lütfen bakın [AVAudioPlayer ile ses oyun](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) bir Xamarin.Mac uygulamasını yürütmeyi ses tam Ayrıntılar için.

## <a name="related-links"></a>İlgili bağlantılar

- [AVAudioPlayer başvurusu](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
