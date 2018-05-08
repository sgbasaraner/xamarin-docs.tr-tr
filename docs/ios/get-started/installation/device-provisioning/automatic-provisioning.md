---
title: Otomatik sağlama
description: Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlamaya yöneliktir. Otomatik imzalama geliştirme sertifika istemek ve profilleri kullanarak bu kılavuzda araştırır.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 0e2ce758da2951efa0508e76cdf4eaac5384fa6b
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="automatic-provisioning"></a>Otomatik sağlama

_Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlamaya yöneliktir. Otomatik imzalama geliştirme sertifika istemek ve profilleri kullanarak bu kılavuzda araştırır._

## <a name="requirements"></a>Gereksinimler

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

- Visual Studio Mac 7.3 veya büyük
- Xcode 9 veya üzeri

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 sürüm 15.7 (veya üzeri)

Ayrıca aşağıdaki olduğu bir Mac yapı konak eşleştirilmelidir:

- Xcode 9 veya üzeri

-----

## <a name="enabling-automatic-signing"></a>Otomatik imzalama etkinleştirme

Otomatik imzalama işlemi başlamadan önce Visual Studio'da eklenen bir Apple kimliği açıklandığı gibi sahip olduğundan emin olun [Apple hesap yönetimi](~/cross-platform/macios/apple-account-management.md) Kılavuzu. Bir Apple kimliği ekledikten sonra varsa ilişkili kullanabilirsiniz _takım_. Bu sertifikalar, profilleri ve diğer kimlikleri karşı takım yapılacak sağlar. Kimliği oluşturmak için kullanılan aynı zamanda takım bir sağlama profiliyle eklenecek bir uygulama kimliği öneki. Bu kim olduğunuzu olduğunu doğrulamak Apple sağlar.

Uygulamanız bir iOS cihazında dağıtım için otomatik olarak imzalamak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Bir iOS projesi Mac için Visual Studio'da açın

2. Açık **Info.plist** dosya.

3. İçinde **imzalama** bölümünde, seçin seçin **otomatik sağlamayı**:

    ![Takım Seçici açılan kutusu](automatic-provisioning-images/image2.png)

4. Ekibinden seçin **takım** açılır.

6. Birkaç saniye sonra bir imzalama sertifikası ve sağlama profili oluşturulur:

    ![Sertifika ve profili başarıyla oluşturuldu](automatic-provisioning-images/image5.png)

    Otomatik imzalama başarısız olursa **otomatik imzalama paneli** hatanın nedenini görüntüler.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Bölümünde açıklandığı gibi bir Mac için Visual Studio 2017 eşleştirin [Mac çiftine](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Kılavuzu.

2. Hazırlama Seçenekleri'ni seçerek açmak **Proje > sağlama özellikleri...**

3. Seçin **otomatik olarak sağlama** şeması:

    ![Otomatik düzeninin seçimi](automatic-provisioning-images/prov4.png)

4. Ekibinden seçin **takım** otomatik imzalama işlemini başlatmak için birleşik giriş kutusu.

    ![Takım seçimi](automatic-provisioning-images/prov3.png)

4. Bu otomatik imzalama işlemini başlatır. Bu yapıtların imzalama için kullanılacak bir uygulama kimliği, sağlama profili ve bir imza kimliği oluşturmak Visual Studio sonra çalışır. Derleme çıktı oluşturma işleminde görebilirsiniz:

    ![Çıktı gösteren nesil yapılarının derleme](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>Otomatik sağlama tetikleme

Otomatik oturum etkin olduğunda, Mac için Visual Studio gerekiyorsa, aşağıdakilerden birini gerçekleştiğinde güncelleştirecek bu yapıtların:

* Bir iOS aygıtı Mac takılı
    - Bu Apple Developer portalında cihazın kayıtlı olup olmadığını görmek için otomatik olarak denetler. Değilse, onu ekleyin ve onu içeren yeni bir sağlama profili oluşturun.
* Paket kimliği, uygulamanızın değiştirilir
    - Bu uygulama kimliği güncelleştirir Bu uygulama Kimliğini içeren yeni bir sağlama profili oluşturulur.
* Desteklenen bir yetenek Entitlements.plist dosyasında etkindir.
    - Bu özellik, uygulama kimliği ve güncelleştirilmiş uygulama kimliği oluşturulan yeni bir sağlama profiliyle eklenir.
    - Tüm özellikleri şu anda desteklenmiyor. Desteklenen sürücüler hakkında daha fazla bilgi için kullanıma [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu.


## <a name="related-links"></a>İlgili bağlantılar

- [Ücretsiz Sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Uygulama Dağıtımı](~/ios/deploy-test/app-distribution/index.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
- [Apple - uygulama dağıtım kılavuzu](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
