---
title: Neden Xamarin.Forms.Maps Android proje COMPILETODALVIK beklenmeyen en üst düzey bir hata ile başarısız oluyor?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 9df9e348440b9dd4b18b3859d64cbe47bd05b24c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Neden Xamarin.Forms.Maps Android proje COMPILETODALVIK beklenmeyen en üst düzey bir hata ile başarısız oluyor?

Bu hata, Visual Studio derleme çıktı penceresinde veya Mac için Visual Studio hata defterinde görülebilir; Xamarin.Forms.Maps kullanarak Android projelerinde.

Bu Xamarin.Android projenizi Java öbek boyutunu artırarak en yaygın olarak çözümlenir. Yığın boyutunu artırmak için aşağıdaki adımları izleyin:

## <a name="visual-studio"></a>Visual Studio

1. Android projesine sağ tıklayın ve proje Seçenekleri'ni açın.
2. Git **Android Seçenekler -> Gelişmiş**
3. 1 G Java yığın boyutu metin kutusuna girin.
4. Projeyi yeniden oluşturun.

![Visual Studio Proje seçenekleri ekran](maps-compiletodalvik-error-images/vsjavaheap.png "Android derleme Visual Studio seçenekleri")

## <a name="visual-studio-for-mac"></a>Mac için Visual Studio

1.  Android projesine sağ tıklayın ve proje Seçenekleri'ni açın.
2.  Git **Yapı -> Android derleme -> Gelişmiş**
3.  1 G Java yığın boyutu metin kutusuna girin.
4.  Projeyi yeniden oluşturun.  

![Visual Studio Mac proje seçenekleri için ekran](maps-compiletodalvik-error-images/xsjavaheap.png "Android derleme Mac için Visual Studio seçenekleri")

