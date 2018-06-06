---
title: Adım 1. Bir uygulamayı Azure Active Directory kullanmak için kaydetme
description: Bu belge, böylece güvenli bir şekilde mobil istemcileri tarafından erişilebilen bir Azure uygulamayı Azure Active Directory ile kaydetme açıklar.
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 7f1e743eea81edc0aa45b49f6acb6a9fd461bc80
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780675"
---
# <a name="step-1-register-an-app-to-use-azure-active-directory"></a>Adım 1. Bir uygulamayı Azure Active Directory kullanmak için kaydetme

1. Gidin [windowsazure.com](https://manage.windowsazure.com) ve Microsoft Account veya kuruluş hesabı Azure Portalı'nda oturum. Bir Azure aboneliğiniz yoksa, bir deneme sürümünden alabilirsiniz [azure.com](http://www.azure.com)

2. Oturum açtıktan sonra Git **Active Directory** (1) bölümü ve (2) uygulamayı kaydetmek istediğiniz dizini seçin

  [ ![](register-images/01.-active-directory-in-azure-portal-sml.jpg "bölümünde ve uygulamayı kaydetmek istediğiniz dizini seçin")](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. Tıklatın **Ekle** yeni uygulama oluşturmak için ardından **kuruluşumun geliştirmekte olduğu bir uygulama Ekle**

  [ ![](register-images/02.-add-new-application-sml.jpg "Kuruluşumun geliştirmekte olduğu bir uygulama ekleme")](register-images/02.-add-new-application.jpg#lightbox)

4. Sonraki ekranda, uygulamanız (ör. bir ad verin. XAM-DEMO).
  Seçtiğinizden emin olun **yerel istemci uygulaması** uygulama türü olarak.

  ![](register-images/03.-app-name.jpg "Uygulama türü olarak yerel istemci uygulaması seçtiğinizden emin olun")

5. Son ekranındaki bir **yeniden yönlendirme URI'si* , uygulamanız için benzersiz olarak kimlik doğrulaması tamamlandığında bu URI döndürür.

  ![](register-images/04.-app-redirect.jpg "Son ekranda bir yeniden yönlendirme kimlik doğrulaması tamamlandığında bu URI döndürülecek gibi uygulamanız için benzersiz olan URI sağlayın")

6. Uygulama oluşturulduktan sonra gidin **yapılandırma** sekmesi. Yazma **istemci kimliği** hangi bizim uygulamada daha sonra kullanacağız. Ayrıca, bu ekranda Active Directory'ye, mobil uygulama erişmesini veya Web API veya kimlik doğrulama tamamlandıktan sonra mobil uygulama tarafından kullanılabilecek Office365 gibi başka bir uygulama ekleyin.

    ![](register-images/05.-configure.jpg "Ayrıca, bu ekranda, mobil uygulama erişmenizi Active Directory'ye veya Web API veya Office365 gibi başka bir uygulama Ekle")



## <a name="related-links"></a>İlgili bağlantılar

- [Microsoft NativeClient örnek](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
