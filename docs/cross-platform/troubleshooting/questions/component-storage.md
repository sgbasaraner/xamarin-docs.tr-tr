---
title: Bileşenleri my makinede saklandığı?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.openlocfilehash: a53045a6179a26b30d824976d11fd2769a84811e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Bileşenleri my makinede saklandığı?

Bir uygulama projesine bir Xamarin bileşeni'ni yüklediğinizde iki yerde de yer:

1. Çözüm klasörünüzün kök düzeyinde bileşenleri klasöründe. Çözümdeki tüm projeleri bileşen kaldırmak, bu klasörden kaldırılacağına.

2. Bir kopyası ayrıca aşağıdaki konumlarda depolanır:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Bu nedenle bir bileşen sisteminizden tamamen kaldırmak için bu proje/çözüm ve yukarıdaki Önbellek klasörü silin.
