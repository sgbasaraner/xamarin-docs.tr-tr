---
title: AVAudioPlayer, Xamarin ile tvOS içinde ses çalma
description: Bu makalede, ses bir AVAudioPlayer bir Xamarin.iOS uygulamasına kullanmanın kayıttan yürütmeyi denetlemek için bir yardımcı sınıfını kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 2ce1d4b8564ef9599581aabd6a72ba3af12ec251
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241357"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>AVAudioPlayer, Xamarin ile tvOS içinde ses çalma

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer hakkında

`AVAudioPlayer` Kayıttan ses veriler için bellek veya dosyası kullanılır. Apple, ağ akışını yapıyor ya da düşük gecikme süresi ses g/ç gerektiren uygulamanızda ses yürütmek için bu sınıfın kullanılmasını önerir.

Kullanabileceğiniz `AVAudioPlayer` aşağıdakileri yapmak için:

- İsteğe bağlı döngü ile herhangi bir süre sesler yürütün.
- İsteğe bağlı eşitleme ile aynı anda birden çok ses yürütün.
- Birim, kayıttan yürütme hızını ve her çalmasını stereo konumlandırma denetler.
- İleri veya Geri Sar gibi özellikleri destekler.
- Kayıttan yürütme düzeyi ölçümü verilerini alın.

`AVAudioPlayer` ses gibi iOS, tvOS ve OS X tarafından sağlanan ses biçimi destekler `.aif`, `.wav` veya `.mp3`.

## <a name="playing-sounds-in-tvos"></a>TvOS içinde ses çalma

İOS, tvOS aynı ses araç sınıfları iOS olarak desteklediğinden, lütfen bakın [AVAudioPlayer ile ses oyun](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) yürütmeyi ses bir Xamarin.tvOS uygulamasında tam Ayrıntılar için.



## <a name="related-links"></a>İlgili bağlantılar

- [AVAudioPlayer başvurusu](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
