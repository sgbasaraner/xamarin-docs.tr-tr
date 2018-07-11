---
ms.topic: include
ms.openlocfilehash: 5c11c94956f8d56c66c50a9a480177c5b77c2643
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947423"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Algılayıcı hızı](xref:Xamarin.Essentials.SensorSpeed)

- **Hızlı** – algılayıcı verileri (UI iş parçacığı üzerinde dönmesi garanti değil) mümkün olduğunca hızlı alın.
- **Oyun** – oyunlar (UI iş parçacığı üzerinde dönmesi garanti değil) için uygun oranı.
- **Normal** – ekran yönünü değişiklikler için uygun olan varsayılan oranı.
- **UI** – genel kullanıcı arabirimi için uygun oranı.

Olay işleyicisi kullanıcı Arabirimi iş parçacığı üzerinde çalıştırın ve kullanıcı arabirimi öğeleri, olay işleyicisi erişmesi gerekiyorsa garanti edilmez, [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) UI iş parçacığında bu kodu çalıştırmak için yöntemi.