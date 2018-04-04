---
title: Bileşenleri my makinede saklandığı?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f0dad6e6219d373eaa9f8410aea7d96c81eceb6b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Bileşenleri my makinede saklandığı?

Bir uygulama projesine bir Xamarin bileşeni'ni yüklediğinizde iki yerde de yer:

1. Çözüm klasörünüzün kök düzeyinde bileşenleri klasöründe. Çözümdeki tüm projeleri bileşen kaldırmak, bu klasörden kaldırılacağına.

2. Bir kopyası ayrıca aşağıdaki konumlarda depolanır:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Bu nedenle bir bileşen sisteminizden tamamen kaldırmak için bu proje/çözüm ve yukarıdaki Önbellek klasörü silin.
