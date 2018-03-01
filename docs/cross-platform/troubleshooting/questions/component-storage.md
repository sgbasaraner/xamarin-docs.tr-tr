---
title: "Bileşenleri my makinede saklandığı?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 9549363d7aeb5ff7a5cfa79eb38443aaa80b7019
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Bileşenleri my makinede saklandığı?

Bir uygulama projesine bir Xamarin bileşeni'ni yüklediğinizde iki yerde de yer:

1. Çözüm klasörünüzün kök düzeyinde bileşenleri klasöründe. Çözümdeki tüm projeleri bileşen kaldırmak, bu klasörden kaldırılacağına.

2. Bir kopyası ayrıca aşağıdaki konumlarda depolanır:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Bu nedenle bir bileşen sisteminizden tamamen kaldırmak için bu proje/çözüm ve yukarıdaki Önbellek klasörü silin.
