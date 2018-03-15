---
title: "Otomatik sağlama"
description: "Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlamaya yöneliktir. Otomatik imzalama Mac için Visual Studio geliştirme sertifikaları ve profilleri istemek için kullanarak bu kılavuzda araştırır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 11/17/2017
ms.openlocfilehash: a411c214e35f78ff9d3dd8d4e9122702d66a2156
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="automatic-provisioning"></a>Otomatik sağlama

_Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlamaya yöneliktir. Otomatik imzalama Mac için Visual Studio geliştirme sertifikaları ve profilleri istemek için kullanarak bu kılavuzda araştırır._

## <a name="requirements"></a>Gereksinimler

- Visual Studio Mac 7.3 veya büyük
- Xcode 9 veya üzeri

> [!IMPORTANT]
>  Bu kılavuz, Mac için Visual Studio dağıtımı için bir Apple aygıt ayarlamak için nasıl kullanılacağı ve bir uygulamanın nasıl dağıtılacağı gösterilmektedir. Bunu yapmak için ya da Windows Visual Studio ile bunun için nasıl el ile adımlar için ayrıntılı adımları izlemeniz önerilir [el ile sağlama](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) Kılavuzu.

## <a name="enabling-automatic-signing"></a>Otomatik imzalama etkinleştirme

Otomatik imzalama işlemi başlamadan önce Mac, Visual Studio'da eklenen bir Apple kimliği açıklandığı gibi sahip olduğundan emin olun [Apple hesap yönetimi](~/cross-platform/macios/apple-account-management.md) Kılavuzu. Bir Apple kimliği ekledikten sonra varsa ilişkili kullanabilirsiniz _takım_. Bu sertifikalar, profilleri ve diğer kimlikleri karşı takım yapılacak sağlar. Kimliği oluşturmak için kullanılan aynı zamanda takım bir sağlama profiliyle eklenecek bir uygulama kimliği öneki. Bu kim olduğunuzu olduğunu doğrulamak Apple sağlar.

Uygulamanız bir iOS cihazında dağıtım için otomatik olarak imzalamak için aşağıdakileri yapın:

1. Bir iOS projesi Mac için Visual Studio'da açın

2. Açık **Info.plist** dosya.

3. İçinde **imzalama** bölümünde, seçin seçin **otomatik sağlamayı**:

    ![Takım Seçici açılan kutusu](automatic-provisioning-images/image2.png)

4. Ekibinden seçin **takım** açılır.

6. Birkaç saniye sonra bir imzalama sertifikası ve sağlama profili oluşturulur:

    ![Sertifika ve profili başarıyla oluşturuldu](automatic-provisioning-images/image5.png)

    Otomatik imzalama başarısız olursa **otomatik imzalama paneli** hatanın nedenini görüntüler.

## <a name="triggering-automatic-provisioning"></a>Otomatik sağlama tetikleme

Otomatik oturum etkin olduğunda, Mac için Visual Studio gerekiyorsa, aşağıdakilerden birini gerçekleştiğinde güncelleştirecek bu yapıtların:

* Bir iOS aygıtı mac takılı
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
