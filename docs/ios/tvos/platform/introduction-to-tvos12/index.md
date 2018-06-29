---
title: TvOS 12 giriş
description: Bu belge üst düzey genel bakış tvOS 12 hangi Xamarin'ın Önizleme sürümü için yeni ve güncelleştirilmiş özelliklerin bir şu anda C# bağlamaları sağlar sağlar.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067190"
---
# <a name="introduction-to-tvos-12"></a>TvOS 12 giriş

![Önizleme](~/media/shared/preview.png)

> [!WARNING]
> Xamarin'ın tvOS 12 desteği hatalar içerebilir anlamına değil özelliği tamamlamak, şu anda Önizleme aşamasındadır ve değişebilir. Yalnızca deneme için kullanın.

> [!NOTE]
> - Gözden geçirme [Başlarken](~/ios/platform/introduction-to-ios12/get-started.md) iOS 12 ve xamarin'le tvOS 12 uygulamalar oluşturmaya başlamak konusunda yönergeler için Kılavuzu.
> - Daha fazla bilgi için okuma [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/) için Xamarin Önizleme sürümü.

Bu belge için hangi Xamarin'ın Önizleme sürümü şu anda C# bağlamalarını sunar 12 özelliklerini yeni ve güncelleştirilmiş tvOS üst düzey bir genel bakış sağlar.

## <a name="password-autofill"></a>Parola otomatik doldurma

TvOS 12 kullanıcıların iOS cihazlarını tek bir dokunuşla bir tvOS uygulamasına oturum açmak için kullanabilirsiniz. Bu bir birleşimi etkin `UITextContentType` kullanıcı adı ve parola belirtmek için kullanım alanları, bir iOS uygulaması ve tvOS uygulama ve odaklanması sonra bir kullanıcı için bir öğe seçmek için tercih edilen odak ortamlar arasında bir ilişki kurmak için ilişkili etki alanları bir kullanıcı adı ve parola sağlar.

## <a name="focus-engine-enhancements"></a>Odağı altyapısı geliştirmeleri

tvOS 12 nasıl, odak altyapısıyla etkileşim kurmak için işlendikleri olursa olsun tüm uygulamalar sağlar. Siri uzaktan kullanıcının etkileşimleri odak altyapısı bir öğe seçin, olası odak değişikliklerini ipucu ve odak doğal olarak güncelleştirmek için herhangi bir uygulama ile kullanılabilir. Bu özel uygulamalar Uıkit'ın aracılığıyla etkinleştirilir `IUIFocusItemContainer` arabirimi, `UIFocusMovementHint` sınıfı, `IUIFocusItemScrollableContainer` arabirimi ve diğer ilgili sınıflar ve yöntemler.

## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [TvOS 12 (Apple) (video) yenilikler](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin Önizleme [sürüm notları](https://releases.xamarin.com/preview-release-xcode-10-beta/)
