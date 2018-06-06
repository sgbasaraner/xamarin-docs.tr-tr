---
title: İOS 11 uygulamanızı güncelleştirme
description: Bu belge bağlantılar çeşitli kılavuzlara Xamarin.iOS geliştiricilere iOS 11 sürümü ile kullanılabilen yeni özellikleri açıklar. Örneğin, uygulama mağazası görsel tasarım güncelleştirmeleri değiştirir ve uygulama simgesini güncelleştirir.
ms.prod: xamarin
ms.assetid: EC809504-9CF6-4949-B6EE-36384297E744
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: ea57acbd6165f7b1abd8b9bd69873670c179f411
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787578"
---
# <a name="updating-your-app-to-ios-11"></a>İOS 11 uygulamanızı güncelleştirme

İOS 11, Apple mimarisi güncelleştirmeleri, yeni görsel değişiklikler ve güncelleştirilmiş iTunes bağlanma işlemi anlatılmıştır. Bu kılavuzda bu değişiklikler, her 11 iOS için güncelleştirilmiş Xamarin.iOS uygulamanızı alma yardımcı araştırır.

## <a name="architecture-changesarchitecture-changesmd"></a>[Mimari Değişiklikler](architecture-changes.md)

İOS 11 bilincinde olmanız gereken büyük değişiklikleri biri ayrıntılı biçimde açıklandığı gibi uygulamalar için 32-bit desteğini kullanımdan [Apple'nın](https://developer.apple.com/news/?id=06282017b) basın açıklaması.

Bu kılavuz, uygulamanız için 64-bit güncelleştirme ile size yol gösterir.

## <a name="visual-design-updatesvisual-designmd"></a>[Görsel Tasarım Güncelleştirmeleri](visual-design.md)

İOS 11'da, gezinti çubuğunda, arama çubuğunda ve tablo görünümleri güncelleştirmeleri de dahil olmak üzere yeni görsel değişiklikler Apple kullanıma sunuldu. Ayrıca kenar boşluklarını ve tam ekran içerik üzerinde daha fazla esneklik sağlamak amacıyla iyileştirmeler yapılmıştır. Bu değişiklikler, bu kılavuzda ele alınmıştır.

## <a name="app-store-changesapp-store-changesmd"></a>[App Store Değişiklikleri](app-store-changes.md)

İOS App Store'da yalnızca kullanıcıların verimli bir şekilde mağazaya gidin izin verir, ancak Ayrıca, uygulamanızı kullanıcılara yükseltmek için bir geliştirici olarak tanır tam bir tasarlanması oluşturdu. Bu promosyon uygulama içi satın almalara güncelleştirmeleri ve ürün sayfası için güncelleştirmeleri içerir. iOS 11 kullanıcılarla iletişim kurma, uygulama simge ekleme ve ortak uygulamanıza serbest bırakma ilgili güncelleştirmeleri de ekler.

## <a name="app-icon-updates"></a>Uygulama simgesi güncelleştirir

> [!NOTE]
> Uygulama simgeleri şimdi teslim edilemiyor tarafından bir _varlık Kataloğu_. 

Varlık kataloglar kullanma hakkında bilgi için bkz [uygulama mağazası simgesini](~/ios/app-fundamentals/images-icons/app-store-icon.md) Kılavuzu. Geçirme konusunda yardım için bir Info.plist simgelerden bir varlık Kataloğu'na bakın [varlık Kataloğu Info.plist geçiş](~/ios/app-fundamentals/images-icons/app-icons.md) Kılavuzu.

Varlık kataloğunda gerekli simgesi adlı **App Store** 1024 x 1024 boyutunda olması gerekir. Apple varlık kataloğunda uygulama mağazası simgesini saydam olamaz veya bir alfa kanal içeren belirtilmiş.

![Uygulama varlık kataloğunda simgesi konuma depolayın.](images/image1.png)

## <a name="related-links"></a>İlgili bağlantılar

- [Yeni iOS 11 (Apple) nedir](https://developer.apple.com/ios/)
- [Güncelleştirilmiş uygulama mağazası ürün sayfası (Apple)](https://developer.apple.com/app-store/product-page/)
- [Güncelleştirme uygulamanızı iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
