---
title: "Mimari değişiklikleri"
description: "İOS 11'ın yeni özellikleri keşfetmek"
ms.topic: article
ms.prod: xamarin
ms.assetid: 55F62F3F-8570-402B-B7D9-2875F76CB946
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 6e233d83eb9c5cb360add36da100963b95e54514
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="architecture-changes"></a>Mimari değişiklikleri

_İOS 11'ın yeni özellikleri keşfetmek_

İOS 11 bilincinde olmanız gereken büyük değişiklikleri biri ayrıntılı biçimde açıklandığı gibi uygulamalar için 32-bit desteğini kullanımdan [Apple'nın](https://developer.apple.com/news/?id=06282017b) basın açıklaması. Tüm yeni uygulamaları ve güncelleştirmeleri olan mevcut uygulamalar için 64-bit desteklemesi gerekir. 32-bit uygulamaları **başlatılmaz** iOS 11 içinde.

Mac için Visual Studio uygulamanızda güncelleştirmek için aşağıdaki adımları kullanın:

1. Açmak için proje adına sağ tıklayın **proje seçenekleri**.
2. Gözat **iOS yapı** sekmesi.
3. Ayarlama **desteklenen mimariler** aşağı açılan **x86_64** için **hata ayıklama | iPhoneSimulator** ve **yayın | iPhoneSimulator**:

    ![Simulator mimarileri açılan değiştirme](architecture-changes-images/image1.png)

4. İOS cihazları için yapılandırma değiştirme **hata ayıklama | iPhone** ve **desteklenen mimariler** aşağı açılan **ARM64**:

    ![Cihaz mimarileri açılan değiştirme](architecture-changes-images/image2.png)

5. Yapılandırmasını değiştirmek **yayın | iPhone** ve **desteklenen mimariler** aşağı açılan **ARM64**.

32 bit ve 64-bit mimarileri hakkında daha fazla bilgi için bkz: [32/64 bit Platform konuları](~/cross-platform/macios/32-and-64/index.md#ios) Kılavuzu.

## <a name="related-links"></a>İlgili bağlantılar

- [Yeni iOS 11 (Apple) nedir](https://developer.apple.com/ios/)
- [Güncelleştirilmiş uygulama mağazası ürün sayfası (Apple)](https://developer.apple.com/app-store/product-page/)
- [Güncelleştirme uygulamanızı iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
