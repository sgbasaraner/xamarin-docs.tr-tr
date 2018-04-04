---
title: Kayan nokta
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 67fcf459747152346d32eb5836fa22b99719af12
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="floating-point"></a>Kayan nokta

Xamarin.iOS varsayılan olarak, 64-bit duyarlık ARM üzerinde kullanarak 32-bit ve 64-bit kayan nokta işlemleri gerçekleştirir.  

Bu daha yüksek duyarlılık ne geliştiriciler kayan nokta işlemleri C# ' ta masaüstünde, mobil, gelen beklersiniz daha yakın olan performans etkisi önemli olabilir.

32 bit kayan nokta işlemleri kullanmak için 32-bit kayan nokta kodunuzu derleyin mümkündür.  Bunu yapmak için en az kullanmanız gerekir Xamarin.iOS 8.10 ve iOS kümesinde Seçenekleri'nin Masası derleme "mtouch ek bağımsız değişkenler" giriş satır aşağıdaki değeri:

     --aot-options=-O=float32

Bu statik derleyicileri bilgilendirecektir (Mono'nın yerleşik statik derleyici veya LLVM destekli bir) 32-bit float kullanarak kayan nokta işlemleri gerçekleştirmek için.
