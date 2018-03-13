---
title: "Adım 1. Bir uygulamayı Azure Active Directory kullanmak için kaydetme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 776a60701e01a81856b0a85e7136c57b97cff101
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
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
