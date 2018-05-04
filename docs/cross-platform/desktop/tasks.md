---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Ortak Görevler karşılaştırma
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 84b3edc1a8cbc9ad642d94b457321786fae5fe70
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="common-tasks-comparison"></a>Ortak Görevler karşılaştırma

| Görev | WPF | Xamarin.Forms |
|--- |--- |--- |
|Düğmeleri ile ekranında bir ileti görüntüler|`MessageBox`|`Page.DisplayAlert`|
|Bir zamanlayıcı oluşturma|`DispatcherTimer` sınıfı|`Device.StartTimer` statik yöntemi|
|Varsayılan yazı tipi boyutunu Al|`SystemFonts` statik sınıf|`Device.GetNamedSize` statik yöntemi|
|Bir URI/URL'yi açın|`Process.Start`|`Device.OpenUri`|
|Görüntü eylem sayfası (düğmeleri listesi)|yok|`Page.DisplayActionSheet`|
