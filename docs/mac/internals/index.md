---
title: Xamarin.Mac başlık altında
description: Bu belge bağlantılar çeşitli kılavuzlara çalışmalar Xamarin.Mac açıklanmaktadır. Bağlantılı belgeleri zamanında derleme, Xamarin.Mac mimarisi ve Xamarin.Mac kayıt öncesinde tartışın.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: c940252a675c38247d2c5bb374b9c30237222bda
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792495"
---
# <a name="under-the-hood-in-xamarinmac"></a>Xamarin.Mac başlık altında

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Şimdi zaman derlenmesini (Uygulama Nesne AĞACI)](aot.md)

Şimdi saati (Uygulama Nesne AĞACI) derleme başlangıç performansı artırmak için bir güçlü iyileştirme tekniğidir. Nasıl çalıştığını anlamadan faydalı olacak şekilde ancak, aynı zamanda derleme zamanında, uygulama boyutu ve program yürütme çok büyük şekillerde etkiler.

## <a name="mac-architecturearchitecturemd"></a>[Mac mimarisi](architecture.md)

Objective-C, derleme, seçiciler, kaydedicilerin, uygulama başlatma ve oluşturucunun gibi kavramları dahil olmak üzere Xamarin.Mac'in ilişki.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin.Mac kayıt](registrar.md)

Xamarin.Mac arasında köprü yönetilen world ve Cocoa'nın çalışma zamanı, yönetilmeyen Objective-C sınıflar çağırın ve olaylar gerçekleştiğinde geri çağrılması yönetilen sınıflar izin vererek arasındaki boşluk. Bu "Sihirli" işlemini gerçekleştirmek için gereken iş kaydedici tarafından ele alınır, ancak "başlık altında" neler olduğunu anlama bazı durumlarda yararlı olabilir.
