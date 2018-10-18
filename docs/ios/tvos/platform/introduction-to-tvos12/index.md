---
title: TvOS 12 giriş
description: Bu belge, yüksek düzeyde genel bakış tvOS 12 hangi Xamarin'in Önizleme sürümü için yeni ve güncelleştirilmiş özelliklerin birlikte şu anda C# bağlamaları sağlar sağlar.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: f7fb8cc379a070b848c5154c9c1d4fbfc8186266
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615210"
---
# <a name="introduction-to-tvos-12"></a>TvOS 12 giriş

Bu belge, yeni ve güncelleştirilmiş tvOS 12 üst düzey bir genel bakış sağlar.

Xamarin ile tvOS 12 uygulamalar oluşturmaya başlamak için göz atın [Başlangıç Kılavuzu](~/ios/platform/introduction-to-ios12/get-started.md).

## <a name="tvuikit"></a>TVUIKit

tvOS 12 TVUIKit, tvOS geliştiricilerin posteri görünümleri, resim yazılı düğmelerini, kart görünümleri ve monogram görünümleri gibi ortak tvOS denetimleri kullanmak mümkün kılan bir API kümesini içerir. tvOS 12 etiketlerini tamamen görünür olmasını çok uzun metin kaydırma izin veren bir özelliği de tanıtır.

## <a name="password-autofill"></a>Parola otomatik doldurma

TvOS 12 kullanıcıların iOS cihazlarını bir tvOS uygulaması için tek bir dokunmayla oturum açmak için kullanabilirsiniz. Bu bir birleşimi etkin `UITextContentType` kullanıcı adı ve parola belirtmek için kullanım alanları, bir iOS uygulaması ve bir tvOS uygulaması ve sonra bir kullanıcı odağının alınacağı öğe seçmek için tercih edilen odak ortamlar arasında bir ilişki kurmak için ilişkili etki alanları bir kullanıcı adı ve parola sağlar.

## <a name="focus-engine-enhancements"></a>Odağa altyapısı iyileştirmeleri

tvOS 12 nasıl bunlar, odağa altyapısı ile etkileşim kurmak için işlendiğini ne olursa olsun tüm uygulamalar sağlar. Siri Remote ile bir kullanıcının etkileşimler odağa altyapısı bir öğe seçin, olası odak değişikliklerini ipucu ve odak doğal olarak güncelleştirmek için herhangi bir uygulama ile kullanılabilir. Bu özel uygulamalar aracılığıyla Uıkit'ın etkin `IUIFocusItemContainer` arabirimi `UIFocusMovementHint` sınıfı `IUIFocusItemScrollableContainer` arabirimi ve diğer ilgili sınıflar ve yöntemler.

## <a name="vision-framework"></a>Vision çerçevesi

Vision çerçevesi, yüzleri çeşitli yönleriyle algılayabilir bir geliştirilmiş yüz algılayıcısı içerir. Ayrıca, belirli bir işleme çerçevesi algoritması düzeltmesini seçmek için istek düzeltmeler artık kullanılabilir.

## <a name="natural-language-framework"></a>Doğal dil çerçevesi

Doğal dil çerçevesi, çeşitli dil analizi gerçekleştirmek uygulamaları etkinleştirir. Örneğin, konuşma bölümü tanımlamak ve bir metin bloğu tarafından temsil edilen dil belirlemek için kullanılabilir.

## <a name="deprecations"></a>Bırakılanların

TvOS 12 Apple OpenGL ES kullanımdan kaldırmıştır [geliştiriciler teşvik](https://developer.apple.com/tvos/whats-new/) Metal benimsemek için.

## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [TvOS 12 (Apple) (video) yenilikler](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)