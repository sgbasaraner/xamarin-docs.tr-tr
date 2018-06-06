---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Ortak Görevler karşılaştırma
description: Bu belge, WPF ve Xamarin.Forms çeşitli ortak görevleri gerçekleştirme karşılaştırır. Bu düğme, zamanlayıcılar, bir URI'yı açmak ve bir eylem sayfası görüntüleme yazı tipi boyutlarını, bakar.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780482"
---
# <a name="common-tasks-comparison"></a>Ortak Görevler karşılaştırma

| Görev | WPF | Xamarin.Forms |
|--- |--- |--- |
|Düğmeleri ile ekranında bir ileti görüntüler|`MessageBox`|`Page.DisplayAlert`|
|Bir zamanlayıcı oluşturma|`DispatcherTimer` sınıfı|`Device.StartTimer` statik yöntemi|
|Varsayılan yazı tipi boyutunu Al|`SystemFonts` statik sınıf|`Device.GetNamedSize` statik yöntemi|
|Bir URI/URL'yi açın|`Process.Start`|`Device.OpenUri`|
|Görüntü eylem sayfası (düğmeleri listesi)|yok|`Page.DisplayActionSheet`|
