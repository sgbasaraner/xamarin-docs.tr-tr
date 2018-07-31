---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353275"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Algılayıcı hızı](xref:Xamarin.Essentials.SensorSpeed)

- **Hızlı** – algılayıcı verileri (UI iş parçacığı üzerinde dönmesi garanti değil) mümkün olduğunca hızlı alın.
- **Oyun** – oyunlar (UI iş parçacığı üzerinde dönmesi garanti değil) için uygun oranı.
- **Normal** – ekran yönünü değişiklikler için uygun olan varsayılan oranı.
- **UI** – genel kullanıcı arabirimi için uygun oranı.

Olay işleyicisi kullanıcı Arabirimi iş parçacığı üzerinde çalıştırın ve kullanıcı arabirimi öğeleri, olay işleyicisi erişmesi gerekiyorsa garanti edilmez, [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) UI iş parçacığında bu kodu çalıştırmak için yöntemi.