---
title: Kayan nokta işlemleri içinde Xamarin.iOS
description: Bu belge Xamarin.iOS kayan nokta işlemleri 32-bit ve 64-bit duyarlık nasıl işlediğini açıklar ve ilişkili performans etkileri açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ea5d69b52cbd4c76abb236bd1a272633dde440b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786167"
---
# <a name="floating-point-operations-in-xamarinios"></a>Kayan nokta işlemleri içinde Xamarin.iOS

Xamarin.iOS varsayılan olarak, 64-bit duyarlık ARM üzerinde kullanarak 32-bit ve 64-bit kayan nokta işlemleri gerçekleştirir.  

Bu daha yüksek duyarlılık ne geliştiriciler kayan nokta işlemleri C# ' ta masaüstünde, mobil, gelen beklersiniz daha yakın olan performans etkisi önemli olabilir.

32 bit kayan nokta işlemleri kullanmak için 32-bit kayan nokta kodunuzu derleyin mümkündür.  Bunu yapmak için en az kullanmanız gerekir Xamarin.iOS 8.10 ve iOS kümesinde Seçenekleri'nin Masası derleme "mtouch ek bağımsız değişkenler" giriş satır aşağıdaki değeri:

     --aot-options=-O=float32

Bu statik derleyicileri bilgilendirecektir (Mono'nın yerleşik statik derleyici veya LLVM destekli bir) 32-bit float kullanarak kayan nokta işlemleri gerçekleştirmek için.
