---
title: İle AVAudioPlayer ses çalma
description: Bu makalede, bir yardımcı sınıfı bir AVAudioPlayer ses kullanımının kayıttan yürütmeyi denetlemek için nasıl kullanılacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 25e3285a1da1b6a112629001d5412fdd0788c705
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>İle AVAudioPlayer ses çalma

_Bu makalede, bir yardımcı sınıfı bir AVAudioPlayer ses kullanımının kayıttan yürütmeyi denetlemek için nasıl kullanılacağı gösterilmektedir._

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer hakkında

`AVAudioPlayer` Bellek veya dosyası sınıfı kayıttan yürütme ses verileri için kullanılır. Apple Bu sınıf akış ağ yapmakta olduğunuz veya ses g/ç düşük gecikme gerektiren sürece uygulamanızda ses çalınmaya kullanılmasını önerir.

Kullanabileceğiniz `AVAudioPlayer` sınıfı aşağıdakileri yapın:

- İsteğe bağlı döngü ile tüm Duration ses yürütün.
- İsteğe bağlı eşitleme ile aynı anda birden çok ses yürütün.
- Birim, oynatma hızını ve her çalmasını stereo konumlandırma denetler.
- İleri sarma veya geri sarma gibi özellikleri destekler.
- Kayıttan yürütme düzeyi ölçüm verileri alın.

`AVAudioPlayer` iOS, tvOS ve macOS .aif, .wav veya .mp3 gibi tarafından sağlanan herhangi bir ses biçiminde ses destekler.

## <a name="playing-sounds-in-macos"></a>MacOS içinde ses çalma

MacOS aynı ses araç sınıfları iOS olarak desteklediğinden, bizim iOS bakın [AVAudioPlayer oyun sesle](https://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) Xamarin.Mac uygulama yürütmeyi ses tam ayrıntıları için belgeleri.



## <a name="related-links"></a>İlgili bağlantılar

- [AVAudioPlayer başvurusu](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
