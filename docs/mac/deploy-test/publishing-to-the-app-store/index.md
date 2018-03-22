---
title: "Uygulama mağazası yayımlama"
description: "Bu kılavuzda, Mac için Visual Studio kullanarak bir Xamarin.Mac uygulaması dağıtma aracılığıyla anlatılmaktadır. Bir Mac Geliştirici hesabı ayarlamanız açıklanmaktadır, kod imzalama sertifikaları oluşturma işleminde size yol göstermektedir ve bunları doğrudan veya Mac uygulama mağazası üzerinden dağıtılabilir Mac uygulamalar oluşturmak için nasıl kullanılacağını gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: D26C5E54-EAD2-5487-264D-4263AEA1EBF2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: e4c7b0913d43d9db3b5414c831864dae8d0b4d61
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="publishing-to-the-app-store"></a>Uygulama mağazası yayımlama

_Bu kılavuzda, Mac için Visual Studio kullanarak bir Xamarin.Mac uygulaması dağıtma aracılığıyla anlatılmaktadır. Bir Mac Geliştirici hesabı ayarlamanız açıklanmaktadır, kod imzalama sertifikaları oluşturma işleminde size yol göstermektedir ve bunları doğrudan veya Mac uygulama mağazası üzerinden dağıtılabilir Mac uygulamalar oluşturmak için nasıl kullanılacağını gösterir._

## <a name="overview"></a>Genel Bakış

Xamarin.Mac uygulamaları iki farklı yolla dağıtılabilir:

- **Geliştirici kimliği** – Geliştirici kimliği can ile imzalanmış uygulamaları App Store dışında dağıtılmasını ancak ağ geçidi tarafından tanınmıyor ve yüklemesine izin.
- **Mac App Store** – uygulamaları bir yükleyici paketi olmalıdır ve uygulama ve yükleyici, Mac uygulama mağazası göndermelerini imzalanması gerekir.

Bu belgede, Visual Studio Mac ve Xcode için bir Apple Developer hesabı kurmak ve her bir dağıtım türü için bir Xamarin.Mac projesi yapılandırmak için nasıl kullanılacağı açıklanmaktadır.


## <a name="mac-developer-program"></a>Mac Geliştirici programı

Eklediğinizde [Mac Developer Program](https://developer.apple.com/devcenter/mac/) aşağıdaki ekran görüntüsünde gösterildiği gibi bir kişi veya bir şirket katılmak için bir seçenek Geliştirici sunulur:

[![Apple Geliştirici Portalı](images/image1.png "Apple Geliştirici Portalı")](images/image1-large.png#lightbox)

Durumunuz için doğru kayıt türü seçin.

> [!NOTE]
> Burada yaptığınız seçimleri bir geliştirici hesabını yapılandırırken bazı ekranlar görünecek şekilde etkiler. Bu belgede ekran görüntüleri ve açıklamaları perspektifinden yapılır bir **tek tek** Geliştirici hesabı. İçinde bir **şirket**, bazı seçenekler yalnızca kullanılabilir **takım Yönetim** kullanıcılar.


### <a name="certificates-and-identifiersmacdeploy-testpublishing-to-the-app-storecertificates-identifiersmd"></a>[Sertifikalar ve Tanımlayıcılar](~/mac/deploy-test/publishing-to-the-app-store/certificates-identifiers.md)

Bu kılavuz gerekli sertifikaları ve Xamarin.Mac uygulama yayımlamak için gereken tanımlayıcıları oluşturmada size yol gösterir.


### <a name="create-provisioning-profilemacdeploy-testpublishing-to-the-app-storeprofilesmd"></a>[Sağlama profili oluşturun](~/mac/deploy-test/publishing-to-the-app-store/profiles.md)

Bu kılavuz gerekli sağlama Xamarin.Mac uygulamayı yayımlamak için gerekli profilleri oluşturmada size yol gösterir.


### <a name="mac-app-configurationmacdeploy-testpublishing-to-the-app-storeapp-configurationmd"></a>[Mac Uygulama Yapılandırması](~/mac/deploy-test/publishing-to-the-app-store/app-configuration.md)

Bu kılavuzda, yayın için Xamarin.Mac uygulamasını nasıl yapılandıracağınız anlatılmaktadır.


### <a name="sign-with-developer-idmacdeploy-testpublishing-to-the-app-storesigningmd"></a>[Geliştirici Kimliği ile İmzala](~/mac/deploy-test/publishing-to-the-app-store/signing.md)

Bu kılavuzda, yayım için bir geliştirici kimliği Xamarin.Mac uygulamayla imzalama aracılığıyla anlatılmaktadır.


### <a name="bundle-for-mac-app-storemacdeploy-testpublishing-to-the-app-storebundlingmd"></a>[Mac App Store için Paket Grubu](~/mac/deploy-test/publishing-to-the-app-store/bundling.md)

Bu kılavuzda, Mac App Store yayını için bir Xamarin.Mac Uygulama paketleme aracılığıyla anlatılmaktadır.


### <a name="upload-to-mac-app-storemacdeploy-testpublishing-to-the-app-storeuploadingmd"></a>[Mac App Store’a Yükle](~/mac/deploy-test/publishing-to-the-app-store/uploading.md)

Bu kılavuzda, Xamarin.Mac uygulama yayının Mac App Store karşıya aracılığıyla anlatılmaktadır.


## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](/visualstudio/mac/installation/)
- [Merhaba, Mac örnek](~/mac/get-started/hello-mac.md)
- [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/)
