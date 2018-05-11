---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Ortak Görevler karşılaştırma
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b4d45ae61c5d7a7093d51c4440d11dd883c157a4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="common-tasks-comparison"></a>Ortak Görevler karşılaştırma

| Görev | WPF | Xamarin.Forms |
|--- |--- |--- |
|Düğmeleri ile ekranında bir ileti görüntüler|`MessageBox`|`Page.DisplayAlert`|
|Bir zamanlayıcı oluşturma|`DispatcherTimer` sınıfı|`Device.StartTimer` statik yöntemi|
|Varsayılan yazı tipi boyutunu Al|`SystemFonts` statik sınıf|`Device.GetNamedSize` statik yöntemi|
|Bir URI/URL'yi açın|`Process.Start`|`Device.OpenUri`|
|Görüntü eylem sayfası (düğmeleri listesi)|yok|`Page.DisplayActionSheet`|
