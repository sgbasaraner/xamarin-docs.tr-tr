---
title: Bileşenler makinemizin depolandığı?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 4152c8ef7eeba3748d9244e27e48f3f9a2c0019b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350725"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Bileşenler makinemizin depolandığı?

Bir uygulama projesine Xamarin bileşeni'ni yüklediğinizde iki yerde yer:

1. Çözüm klasörünüzün kök düzeyinde bileşenleri klasöründe. Bileşenin iş Çözümdeki tüm projeleri kaldırırsanız, bu klasörden kaldırılacak.

2. Kopya ayrıca aşağıdaki konumlarda depolanır:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Bu nedenle bir bileşen sisteminizden tamamen kaldırmak için proje/çözüm ve yukarıdaki önbellek klasörünü silin.
