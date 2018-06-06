---
title: Bir geliştirici kimliği Xamarin.Mac uygulamalarla imzalama
description: Bu belge, böylece Mac App Store dışında dağıtılabilir Xamarin.Mac Uygulama geliştirici ID ile oturum açıklar. Kod imzalama seçenekleri ve oluşturmayı açıklar.
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 130766ef7f9ab8e311db97a7209f4ec62a2ceee4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792310"
---
# <a name="signing-xamarinmac-apps-with-a-developer-id"></a>Bir geliştirici kimliği Xamarin.Mac uygulamalarla imzalama

Geliştirici doğrudan macOS kullanıcılara bir uygulama dağıtmak planlıyorsa, Apple önerir bunlar kod-oturum geliştirici, BT macOS sistemlerde yüklenebilmesi için kimliği ile **ağ geçidi** etkin. Uygulama imzalanmamış, **ağ geçidi** kullanıcıların (bunlar atlayabilir bu başlatılırken CTRL tuşunu basılı tutarak sınırlama) bir uyarı iletisi içeren yüklemenize engel olmaz.

Daha fazla bilgi edinin [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/) ve [dağıtma dışında Mac App Store](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html) Apple'nın Web sitesinde.

## <a name="code-signing-options"></a>Kod imzalama seçenekleri

Kullanıcılar (aracılığıyla değil Mac App Store) kümesine doğrudan dağıtım için bir uygulama oluşturmak için **imzalama ayarlarını** kullanmak için **Geliştirici kimliği**. Düzenlenecek olun **sürüm** yapılandırma.

 [![](signing-images/config02.png "Mac imzalama seçenekleri")](signing-images/config02.png#lightbox)


## <a name="build"></a>Derleme

Seçili doğru yapılandırma için yapılandırmadan önce olun ve bir yükleme paketi oluşturmak için seçin **Mac yapı** ayarları:

[![](signing-images/config03.png "Derleme seçenekleri")](signing-images/config03.png#lightbox)

Uygulama oluştururken, her iki sertifikalar kullanmak üzere Geliştirici istenir:

 [![](signing-images/image57.png "Anahtarlık erişimi izin verme")](signing-images/image57.png#lightbox)

 [![](signing-images/image58.png "Anahtarlık erişimi izin verme")](signing-images/image58.png#lightbox)

Uygulama oluşturulduktan sonra Geliştirici projeye sağ tıklayın ve seçin **içeren klasör Aç** paket dosyasını bulmak için (içinde `bin/Release` directory). Yüklemesi için herhangi bir macOS kullanıcı dağıtılabilir şekilde bu paket dosyası uygulaması için bir yükleyici içerir.

 [![](signing-images/image59.png "Uygulama paketi Finder seçme")](signing-images/image59.png#lightbox)

## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](~//mac/get-started/installation.md)
- [Merhaba, Mac örnek](~//mac/get-started/hello-mac.md)
- [Mac App Store, uygulamaları dağıtma](https://developer.apple.com/devcenter/mac/checklist/)
- [Araçlar Kılavuzu: Kod uygulamanızı imzalama](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/)
