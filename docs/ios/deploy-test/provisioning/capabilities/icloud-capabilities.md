---
title: iCloud Xamarin.iOS özellikleri
description: Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz iCloud özellikleri için gereken kurulum açıklar.
ms.prod: xamarin
ms.assetid: 3CBAC982-D8DE-48DD-97CD-32B551D9DB85
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: a1d2197a4cc7c7bf8fc9b69463a4679389e45ee8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785147"
---
# <a name="icloud-capabilities-in-xamarinios"></a>iCloud Xamarin.iOS özellikleri

_Bir uygulama genelde yetenekleri ekleme sağlama ek kurulum gerektirir. Bu kılavuz iCloud özellikleri için gereken kurulum açıklar._

iCloud içeriklerini depolamak ve cihazlar arasında paylaşmak için iOS kullanıcılarla kullanışlı ve basit bir yol sağlar. Geliştiriciler kendi kullanıcılar için depolama sağlamak için iCloud kullanabilir dört yolu vardır: anahtar-değer depolama, UIDocument depolama, CoreData ve CloudKit doğrudan tek dosyalar ve dizinler için depolama sağlamak için kullanma. Bunlar üzerinde daha fazla bilgi için bkz [icloud giriş](~/ios/data-cloud/introduction-to-icloud.md) Kılavuzu.

İCloud ekleme yeteneği uygulamaya diğer uygulama hizmetleri nedeniyle daha biraz daha zordur _kapsayıcıları_. Kapsayıcıları iCloud içinde bir uygulama için bilgileri depolamak ve – kullanıcının iOS cihazında korumalı alan gibi yinelenmeli için bir tek iCloud hesabıyla bulunan tüm bilgileri sağlamak için kullanılır. Kapsayıcıları hakkında daha fazla bilgi için başvurmak [CloudKit giriş](~/ios/data-cloud/intro-to-cloudkit.md) Kılavuzu.

> [!IMPORTANT]
> Apple [araçlar sağlar](https://developer.apple.com/support/allowing-users-to-manage-data/) Avrupa Birliği'nın genel veri koruma düzenleme (GDPR) düzgün bir şekilde işlemek geliştiricilere yardımcı olmak için.

<a name="icloud-developer-center" />

## <a name="developer-center"></a>Geliştirici Merkezi

Geliştirici Merkezi aracılığıyla yeni bir uygulama sağlamada alınması gereken iki adımı vardır:

1.  Bir kapsayıcı oluşturun.
2.  İCloud özelliğine sahip bir uygulama kimliği oluşturmanız ve kapsayıcı ekleyin.
3. Bu uygulama kimliği içeren bir sağlama profili oluşturun

Aşağıdaki adımlar bu adımları uygularken size kılavuzluk eder:

1.  Gözat [Apple Geliştirici Merkezi](https://developer.apple.com/account/) ve sertifikalar, tanımlayıcı ve profilleri bölümüne gidin: 
    
     ![Apple Geliştirici Merkezi ana sayfası](icloud-capabilities-images/image22.png)

2.  Altında **tanımlayıcıları** seçin **iCloud kapsayıcıları**ve ardından **+** yeni bir kapsayıcı oluşturmak için:  
    
    ![iCloud kapsayıcı ekranı](icloud-capabilities-images/image23.png)

3.  Girin bir **açıklama** ve benzersiz bir **tanımlayıcısı** iCloud kapsayıcısı için: 
    
    ![iCloud kapsayıcı kayıt ekranı](icloud-capabilities-images/image24.png)

4.  Basın **devam**, bilgilerin doğru olduğundan emin olun ve tuşuna **kaydetmek** iCloud kapsayıcı oluşturmak için:  
    
    ![iCloud kapsayıcı kayıt ekranı](icloud-capabilities-images/image25.png)

Yeni bir uygulama kimliği oluşturmak ve bir kapsayıcı eklemek için aşağıdakileri yapın:

1.  İçinde [Geliştirici Merkezi](https://developer.apple.com/account/), tıklayın **uygulama kimlikleri** altında **tanımlayıcıları**: 
    
    ![Tanımlayıcı bölümünde Geliştirici Merkezi](icloud-capabilities-images/image26.png)

2.  Seçin **+** düğmesi yeni bir uygulama kimliği eklemek için: 
    
    ![Yeni uygulama kimliği düğme ekleme](icloud-capabilities-images/image27.png)

3.  Girin bir **adı** uygulama kimliği için ve şablona bir **açık uygulama kimliği**:
    
    ![Yeni uygulama kimliği ayrıntılarını girin](icloud-capabilities-images/image28.png)

4.  Altında **uygulama hizmetleri** seçin **iCloud** ve **dahil CloudKit Destek**:
    
    ![İCloud uygulama hizmetleri seçin](icloud-capabilities-images/image29.png)

5.  Seçin **devam** ve ardından **kaydetmek**. Not: onay ekranında iCloud, sarı bir simgeyle seçili yapılandırılabilir görüntülenir   
    
    ![Onay ekranı](icloud-capabilities-images/image30.png)

6.  Uygulama kimlikleri listesine dönmek ve az önce oluşturduğunuz birini seçin: 
    
    ![Uygulama Kimliği ekran'ı seçin](icloud-capabilities-images/image31.png)

7.  Bu genişletilmiş bölüm en altına kadar kaydırın ve tıklayın **Düzenle**:
    
    ![Uygulama Kimliği Düzenle](icloud-capabilities-images/image32.png)

8.  Listeye tıklayın ve iCloud aşağı **Düzenle** düğmesi:  
    
    ![İCloud uygulama kimliği Düzenle](icloud-capabilities-images/image33.png)

9.  Bu uygulama kimliği ile kullanmak için kapsayıcıyı seçin:  
    
    ![Kapsayıcı ekran'ı seçin](icloud-capabilities-images/image34.png)

10. Kapsayıcı onaylayın atamaları ve tuşuna **atamak**.
 
Bu uygulama kimliği artık oluşturmak ya da, yeni bir sağlama profili yeniden oluşturmak için açıklandığı gibi kullanılabilir [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu. 

İCloud kullanma hakkında daha fazla bilgi için aşağıdaki kılavuzlara bakın:

*   [İcloud giriş](~/ios/data-cloud/introduction-to-icloud.md)
*   [CloudKit giriş](~/ios/data-cloud/intro-to-cloudkit.md)
*   [Seçici belge giriş](~/ios/platform/document-picker.md)

## <a name="next-steps"></a>Sonraki Adımlar
 
Aşağıdaki liste yapılması gereken ek adımları açıklar:

* Uygulamanızda framework ad kullanın.
* Gerekli yetkilendirmeler uygulamanıza ekleyin. Gerekli yetkilendirmeler ve bunları ekleme hakkında bilgi ayrıntılı olarak [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.
* Uygulamasının **iOS paket imzalama**, emin **özel yetkilendirmeler** ayarlanır **Entitlements.plist**. Bu _değil_ hata ayıklama ve iOS simülatörü varsayılan ayarı oluşturur.

Uygulama Hizmetleri ile ilgili sorunlarla karşılaşırsanız, başvurmak [sorun giderme](~/ios/deploy-test/provisioning/capabilities/index.md) ana Kılavuzu'nun bölümünde.