---
title: Mimarisine genel bakış
description: Bu belge Xamarin etkileşimli aracısı ve etkileşimli istemci birlikte nasıl çalıştığını inceleniyor çalışma kitapları, mimarisini açıklar.
ms.prod: xamarin
ms.assetid: 6C0226BE-A0C4-4108-B482-0A903696AB04
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 3cd0d3e8cc47dd7fa43dfa0b910c9f09740fcc9a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794013"
---
# <a name="architecture-overview"></a>Mimarisine genel bakış

Xamarin çalışma kitaplarını birbirleri ile birlikte çalışması gereken iki ana bileşeni özellikleri: _Aracısı_ ve _istemci_.

## <a name="interactive-agent"></a>Etkileşimli Aracısı

.NET uygulaması bağlamında çalışan küçük bir platforma özgü derleme aracısı bileşendir.

Xamarin çalışma kitaplarını platformlar, iOS, Android, Mac ve WPF gibi bir dizi için önceden derlenmiş "boş" uygulamaları sağlar. Bu uygulamalar açıkça Aracısı barındırır.

(Xamarin denetçisi) Canlı İnceleme sırasında normal geliştirme ve hata ayıklama iş akışının parçası olarak var olan bir uygulama IDE ayıklayıcıya aracılığıyla aracı eklenmiş.

## <a name="interactive-client"></a>Etkileşimli istemci

Çalışma kitabı/REPL arabirim sunmak için bir web tarayıcısı yüzey barındıran bir yerel Kabuğu (Cocoa Mac üzerinde Windows'da WPF) istemcidir. SDK açısından bakıldığında, tüm istemci tümleştirmeler CSS ve JavaScript ile uygulanır.

İstemci (Roslyn) sorumlu olduğu küçük derlemelerine kaynak kodu derleme ve bunları üzerine yürütülmek bağlı aracı gönderme. Yürütme sonuçlarını istemciye işleme için gönderilir. Çalışma kitabındaki her hücreye önceki hücrenin derleme başvuruda bulunan bir derleme verir.

Bir aracı .NET platformu herhangi bir türde çalıştıran herhangi bir şey için erişimi çalışan uygulamada sahip olduğundan, sonuçları bir platform belirsiz şekilde serileştirmek için dikkatli olunması gerekir.
