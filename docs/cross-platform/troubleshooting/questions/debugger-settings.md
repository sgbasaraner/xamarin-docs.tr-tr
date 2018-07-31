---
title: Hata ayıklayıcı için hangi proje ayarları gereklidir?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 646ef7f708be2de6a851ace25d69a7c2f0b18a83
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350813"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Hata ayıklayıcı için hangi proje ayarları gereklidir?

Hata ayıklayıcının (isabet kesme noktaları, görüntü hata ayıklama günlükleri, vb.) beklendiği şekilde çalışması sırayla geliştirici araçları ve hata ayıklama bilgileri görüntüleme her ikisi de etkinleştirilmelidir.

Lütfen ortam ayarlarınızı denetlemek için şu adımları izleyin:

## <a name="visual-studio"></a>Visual Studio
1. Proje seçenekleri
2. Git **Yapı > Gelişmiş...** Hata ayıklama bilgileri ayarlayın **tam**
3. Her platform için ayarları:
   - Git **Android Seçenekler > hata ayıklama seçenekleri**. Değer çizgisi **Geliştirici izlemesini etkinleştir** kutusu.
   - Git **iOS derleme > hata ayıklama seçenekleri**. Değer çizgisi **Etkinleştir'hata ayıklama** kutusu.

## <a name="visual-studio-for-mac"></a>Mac için Visual Studio
1. Proje seçenekleri
2. Git **Yapı > derleyici > Genel Seçenekler**. Hata ayıklama bilgileri ayarlayın **tam**
3. Her platform için ayarları:
  - Git **Yapı > Android derleme > hata ayıklama seçenekleri**. Değer çizgisi **Geliştirici izlemesini etkinleştir** kutusu.
  - Git **Yapı > iOS hata ayıklama**. Değer çizgisi **Etkinleştir'hata ayıklama** kutusu.

