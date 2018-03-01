---
title: "Geliştirici ID ile oturum"
description: "Bu kılavuzda, yayın için geliştirici kimliği Xamarin.Mac uygulama imzalama aracılığıyla anlatılmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d9e0bb41360185ffbe476ec5eed3a5c8c2ebf8f9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="sign-with-developer-id"></a>Geliştirici ID ile oturum

Geliştirici doğrudan macOS kullanıcılara bir uygulama dağıtmak planlıyorsa, Apple önerir bunlar kod-oturum geliştirici, BT macOS sistemlerde yüklenebilmesi için kimliği ile **ağ geçidi** etkin. Uygulama imzalanmamış, **ağ geçidi** kullanıcıların (bunlar atlayabilir bu başlatılırken CTRL tuşunu basılı tutarak sınırlama) bir uyarı iletisi içeren yüklemenize engel olmaz.

Daha fazla bilgi edinin [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/) ve [dağıtma dışında Mac App Store](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html) Apple'nın Web sitesinde.

## <a name="code-signing-options"></a>Kod imzalama seçenekleri

Kullanıcılar (aracılığıyla değil Mac App Store) kümesine doğrudan dağıtım için bir uygulama oluşturmak için **imzalama ayarlarını** kullanmak için **Geliştirici kimliği**. Düzenlenecek olun **sürüm** yapılandırma.

 [ ![](signing-images/config02.png "Mac imzalama seçenekleri")](signing-images/config02.png)


## <a name="build"></a>Derleme

Seçili doğru yapılandırma için yapılandırmadan önce olun ve bir yükleme paketi oluşturmak için seçin **Mac yapı** ayarları:

[ ![](signing-images/config03.png "Derleme seçenekleri")](signing-images/config03.png)

Uygulama oluştururken, her iki sertifikalar kullanmak üzere Geliştirici istenir:

 [ ![](signing-images/image57.png "Anahtarlık erişimi izin verme")](signing-images/image57.png)

 [ ![](signing-images/image58.png "Anahtarlık erişimi izin verme")](signing-images/image58.png)

Uygulama oluşturulduktan sonra Geliştirici projeye sağ tıklayın ve seçin **içeren klasör Aç** paket dosyasını bulmak için (içinde `bin/Release` directory). Yüklemesi için herhangi bir macOS kullanıcı dağıtılabilir şekilde bu paket dosyası uygulaması için bir yükleyici içerir.

 [ ![](signing-images/image59.png "Uygulama paketi Finder seçme")](signing-images/image59.png)

## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](~//mac/get-started/installation.md)
- [Merhaba, Mac örnek](~//mac/get-started/hello-mac.md)
- [Mac App Store, uygulamaları dağıtma](https://developer.apple.com/devcenter/mac/checklist/)
- [Araçlar Kılavuzu: Kod uygulamanızı imzalama](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/)
