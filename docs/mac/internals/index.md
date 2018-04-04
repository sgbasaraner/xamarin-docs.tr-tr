---
title: Başlık altında
description: Bir bilgi çalışmalar Xamarin.Mac
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 74721e880bb0d3ada3f3940a4074d06f55601c0e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="under-the-hood"></a>Başlık altında

_Bir bilgi çalışmalar Xamarin.Mac_

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Şimdi zaman derlenmesini (Uygulama Nesne AĞACI)](aot.md)

Şimdi saati (Uygulama Nesne AĞACI) derleme başlangıç performansı artırmak için bir güçlü iyileştirme tekniğidir. Nasıl çalıştığını anlamadan faydalı olacak şekilde ancak, aynı zamanda derleme zamanında, uygulama boyutu ve program yürütme çok büyük şekillerde etkiler.

## <a name="mac-architecturearchitecturemd"></a>[Mac mimarisi](architecture.md)

Objective-C, derleme, seçiciler, kaydedicilerin, uygulama başlatma ve oluşturucunun gibi kavramları dahil olmak üzere Xamarin.Mac'in ilişki.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac registrar](registrar.md)

Xamarin.Mac arasında köprü yönetilen world ve Cocoa'nın çalışma zamanı, yönetilmeyen Objective-C sınıflar çağırın ve olaylar gerçekleştiğinde geri çağrılması yönetilen sınıflar izin vererek arasındaki boşluk. Bu "Sihirli" işlemini gerçekleştirmek için gereken iş kaydedici tarafından ele alınır, ancak "başlık altında" neler olduğunu anlama bazı durumlarda yararlı olabilir.
