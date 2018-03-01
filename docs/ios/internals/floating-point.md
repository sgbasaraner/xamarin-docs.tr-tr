---
title: Kayan nokta
ms.topic: article
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea29132ad4ac55f6fb151ac2125ab1add82c8518
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="floating-point"></a>Kayan nokta

Xamarin.iOS varsayılan olarak, 64-bit duyarlık ARM üzerinde kullanarak 32-bit ve 64-bit kayan nokta işlemleri gerçekleştirir.  

Bu daha yüksek duyarlılık ne geliştiriciler kayan nokta işlemleri C# ' ta masaüstünde, mobil, gelen beklersiniz daha yakın olan performans etkisi önemli olabilir.

32 bit kayan nokta işlemleri kullanmak için 32-bit kayan nokta kodunuzu derleyin mümkündür.  Bunu yapmak için en az kullanmanız gerekir Xamarin.iOS 8.10 ve iOS kümesinde Seçenekleri'nin Masası derleme "mtouch ek bağımsız değişkenler" giriş satır aşağıdaki değeri:

     --aot-options=-O=float32

Bu statik derleyicileri bilgilendirecektir (Mono'nın yerleşik statik derleyici veya LLVM destekli bir) 32-bit float kullanarak kayan nokta işlemleri gerçekleştirmek için.
