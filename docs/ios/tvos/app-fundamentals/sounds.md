---
title: Xamarin AVAudioPlayer ile tvOS içinde ses çalma
description: Bu makalede, bir yardımcı sınıfı ses bir AVAudioPlayer bir Xamarin.iOS uygulaması kullanarak kayıttan yürütmeyi denetlemek için nasıl kullanılacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7d95a8ea6c22c0d897d8ccfe0c2ca401f6523783
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788640"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Xamarin AVAudioPlayer ile tvOS içinde ses çalma

## <a name="about-the-avaudioplayer"></a>AVAudioPlayer hakkında

`AVAudioPlayer` Kayıttan yürütme ses veriler için bellek veya dosyası kullanılır. Apple Bu sınıf akış ağ yapmakta olduğunuz veya ses g/ç düşük gecikme gerektiren sürece uygulamanızda ses çalınmaya kullanılmasını önerir.

Kullanabileceğiniz `AVAudioPlayer` aşağıdakileri yapmak için:

- İsteğe bağlı döngü ile tüm Duration ses yürütün.
- İsteğe bağlı eşitleme ile aynı anda birden çok ses yürütün.
- Birim, oynatma hızını ve her çalmasını stereo konumlandırma denetler.
- İleri sarma veya geri sarma gibi özellikleri destekler.
- Kayıttan yürütme düzeyi ölçüm verileri alın.

`AVAudioPlayer` iOS, tvOS ve OS X gibi sağladığı tüm ses biçiminde ses destekler `.aif`, `.wav` veya `.mp3`.

## <a name="playing-sounds-in-tvos"></a>İçinde tvOS ses çalma

Bizim iOS tvOS aynı ses araç sınıfları iOS olarak desteklediğinden, lütfen bkz [AVAudioPlayer oyun sesle](http://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) Xamarin.tvOS uygulama yürütmeyi ses tam ayrıntıları için belgeleri.



## <a name="related-links"></a>İlgili bağlantılar

- [AVAudioPlayer başvurusu](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
