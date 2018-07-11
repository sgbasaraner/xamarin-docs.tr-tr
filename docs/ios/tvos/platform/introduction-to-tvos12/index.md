---
title: TvOS 12 giriş
description: Bu belge, yüksek düzeyde genel bakış tvOS 12 hangi Xamarin'in Önizleme sürümü için yeni ve güncelleştirilmiş özelliklerin birlikte şu anda C# bağlamaları sağlar sağlar.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38847567"
---
# <a name="introduction-to-tvos-12"></a>TvOS 12 giriş

![Önizleme](~/media/shared/preview.png)

> [!WARNING]
> Xamarin'in tvOS 12 desteği, hatalar, içerdiği anlamına gelen değil özelliği tamamlamak, şu anda Önizleme aşamasında olan ve değişebilir. Yalnızca deneme kullanın.

> [!NOTE]
> - Gözden geçirme [Başlarken](~/ios/platform/introduction-to-ios12/get-started.md) iOS 12 ve Xamarin ile tvOS 12 uygulamaları oluşturmaya başlayın hakkında yönergeler için kılavuz.
> - Daha fazla bilgi için okuma [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/) için Xamarin Önizleme sürümü.

Bu belge, hangi Xamarin'in preview için sürüm şu anda C# bağlamaları sağlar. 12 özellikleri yeni ve güncelleştirilmiş tvOS üst düzey bir genel bakış sağlar.

## <a name="password-autofill"></a>Parola otomatik doldurma

TvOS 12 kullanıcıların iOS cihazlarını bir tvOS uygulaması için tek bir dokunmayla oturum açmak için kullanabilirsiniz. Bu bir birleşimi etkin `UITextContentType` kullanıcı adı ve parola belirtmek için kullanım alanları, bir iOS uygulaması ve bir tvOS uygulaması ve sonra bir kullanıcı odağının alınacağı öğe seçmek için tercih edilen odak ortamlar arasında bir ilişki kurmak için ilişkili etki alanları bir kullanıcı adı ve parola sağlar.

## <a name="focus-engine-enhancements"></a>Odağa altyapısı iyileştirmeleri

tvOS 12 nasıl bunlar, odağa altyapısı ile etkileşim kurmak için işlendiğini ne olursa olsun tüm uygulamalar sağlar. Siri Remote ile bir kullanıcının etkileşimler odağa altyapısı bir öğe seçin, olası odak değişikliklerini ipucu ve odak doğal olarak güncelleştirmek için herhangi bir uygulama ile kullanılabilir. Bu özel uygulamalar aracılığıyla Uıkit'ın etkin `IUIFocusItemContainer` arabirimi `UIFocusMovementHint` sınıfı `IUIFocusItemScrollableContainer` arabirimi ve diğer ilgili sınıflar ve yöntemler.

## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [TvOS 12 (Apple) (video) yenilikler](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin Önizleme [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/)
