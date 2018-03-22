---
title: "Mac App Store için paketleme"
description: "Bu kılavuzda, Mac App Store yayını için bir Xamarin.Mac Uygulama paketleme aracılığıyla anlatılmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: c3f94b448539b2e4073c7d8a1092df066e484dfc
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="bundle-for-mac-app-store"></a>Mac App Store için paket

Bu bölümde Mac uygulama Mağazası'nin Mac için Visual Studio kullanarak sürümde için uygulama oluşturma temelleri açıklanır Ek özellikleri (örneğin, iCloud erişim ve anında iletme bildirimleri) bağlı olarak, daha fazla Kurulum gerekli olabilir, bu makalenin kapsamı beyon gider.

> [!NOTE]
> Bu bölüme başlamadan önce Geliştirici sağlama profili için Mac uygulama mağazası oluşturmak için bir üretim oluşturmuş olmanız gerekir. Gerekli sağlama profilleri oluşturma hakkında daha önce bu belgedeki yönergelere bakın.

## <a name="code-signing-options"></a>Kod imzalama seçenekleri

Değişiklik **yapılandırma** için **sürüm** kod imzalama ve paketleme seçenekleri güncelleştirmeden önce. Geliştirici şirketlerinin kullandığınızdan emin olun gerekiyor **kimlik** ve zaman oluşturduğumuz sağlama profili uygulama mağazasında yayımı için uygulamanızı imzalama.

 [![Kod imzalama seçenekleri düzenleme](bundling-images/config02.png "imzalama seçenekleri kod düzenleme")](bundling-images/config02-large.png#lightbox)

Bir yükleyici paketi oluşturma seçeneğini işaretlenen emin olun **Mac yapı** ayarları:

[![Derleme seçenekleri düzenleme](bundling-images/config03.png "düzenleme derleme seçenekleri")](bundling-images/config03-large.png#lightbox)

## <a name="build"></a>Derleme

Yapılandırmadan önce emin **sürüm** yapılandırması seçtiniz. Geliştirici uygulama oluşturduğunda, iki sertifika kullanacak şekilde istenir:

 ![Sertifikayı kullanmak üzere uygulamanın verme](bundling-images/image62.png "sertifikayı kullanmak üzere uygulama izin verme")

 ![Sertifikayı kullanmak üzere uygulamanın verme](bundling-images/image63.png "sertifikayı kullanmak üzere uygulama izin verme")

Uygulama oluşturulduktan sonra Geliştirici projeye sağ tıklayın ve seçin **içeren klasör Aç** paket dosyasını bulmak için (içinde `bin/x86/AppStore` aşağıda gösterilen örnekte dizin).  Bu paket dosyası için Apple Mac App Store eklenmek üzere gönderilebilen uygulama için bir yükleyici içerir.

 ![Yapı paket Finder seçme](bundling-images/image64.png "Finder yapı paketi seçme")


## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](/visualstudio/mac/installation/)
- [Merhaba, Mac örnek](~/mac/get-started/hello-mac.md)
- [Mac App Store, uygulamaları dağıtma](https://developer.apple.com/devcenter/mac/checklist/)
- [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/)
