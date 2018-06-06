---
title: İçinde Xamarin.Mac ile AVAudioPlayer ses çalma
description: Bu belge, bir Xamarin.Mac uygulaması ile AVAudioPlayer ses çalınmaya açıklar. Bir üst düzey ve daha tam olarak ele diğer belgelere yönelik bağlantılar AVAudioPlayer açıklanır.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 9e5b9ec43189999f8a0aee29eb50221b494e2133
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791861"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>İçinde Xamarin.Mac ile AVAudioPlayer ses çalma

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
