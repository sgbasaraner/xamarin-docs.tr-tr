---
title: "Hata ayıklayıcı proje ayarları gereklidir?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 6097dc8dfdff8807137ef68be86a08e4c9e23988
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Hata ayıklayıcı proje ayarları gereklidir?

(İsabet kesme noktaları, görüntü hata ayıklama günlükleri, vb.) beklendiği şekilde çalışması hata ayıklayıcı için sırayla geliştirici araçları ve hata ayıklama bilgileri görünen her ikisi de etkinleştirilmesi gerekir.

Lütfen ortam ayarlarınızı denetlemek için aşağıdaki adımları izleyin:

## <a name="visual-studio"></a>Visual Studio
1. Proje seçenekleri
2. Git **Yapı > Gelişmiş...** Hata ayıklama bilgilerini ayarlamak **tam**
3. Her platform için ayarları:
   - Git **Android Seçenekleri > hata ayıklama seçeneklerini**. Değer çizgilerinin **Geliştirici Araçları'nı etkinleştir** kutusu.
   - Git **iOS Yapı > hata ayıklama seçenekleri**. Değer çizgilerinin **etkinleştirmek hata ayıklama** kutusu.

## <a name="visual-studio-for-mac"></a>Mac için Visual Studio
1. Proje seçenekleri
2. Git **Yapı > derleyici > Genel Seçenekler**. Hata ayıklama bilgilerini ayarlamak **tam**
3. Her platform için ayarları:
  - Git **Yapı > Android derleme > hata ayıklama seçeneklerini**. Değer çizgilerinin **Geliştirici Araçları'nı etkinleştir** kutusu.
  - Git **Yapı > iOS hata ayıklama**. Değer çizgilerinin **etkinleştirmek hata ayıklama** kutusu.

