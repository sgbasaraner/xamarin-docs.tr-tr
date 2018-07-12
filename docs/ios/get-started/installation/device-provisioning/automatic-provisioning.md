---
title: Xamarin.iOS için sağlama otomatik
description: Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlama sağlamaktır. Bu kılavuz, geliştirme sertifika ve profilleri otomatik imzalaması kullanarak keşfediyor.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/22/2018
ms.openlocfilehash: a0c3179dc8e349c23d5521230e0957d1be9384ec
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986193"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Xamarin.iOS için sağlama otomatik

_Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlama sağlamaktır. Bu kılavuz, geliştirme sertifika ve profilleri otomatik imzalaması kullanarak keşfediyor._

## <a name="requirements"></a>Gereksinimler

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

- Visual Studio Mac 7.3 veya büyük
- Xcode 9 veya üzeri

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 sürüm 15.7 (veya üstü)

Ayrıca aşağıdakileri içeren bir Mac derleme konağı eşleştirilmelidir:

- Xcode 9 veya üzeri

-----

## <a name="enabling-automatic-signing"></a>Otomatik imzalama etkinleştirme

Otomatik imzalama işlemi başlamadan önce açıklandığı gibi Visual Studio'da eklenen bir Apple kimliği olduğundan emin olun [Apple hesap yönetimi](~/cross-platform/macios/apple-account-management.md) Kılavuzu. Bir Apple kimliği ekledikten sonra varsa ilişkili kullanabilirsiniz _takım_. Bu sertifikalar, profilleri ve diğer kimlikleri team karşı yapılacak sağlar. Takım Kimliği oluşturmak için kullanılan aynı zamanda bir sağlama profiliyle dahil edilecek uygulama kimliği öneki. Bu, kim olduğunuzu doğrulamak Apple sağlar.

> [!IMPORTANT]
> Başlamadan önce ya da oturum açtığınızdan emin olun [iTunes Connect](https://itunesconnect.apple.com/) veya [appleid.apple.com](https://appleid.apple.com) son Apple hesap ilkeleri kabul denetlemek için. İstenirse, yeni hesap anlaşmaları Apple kabul etmek için adımları tamamlayın. Mayıs 2018'den Gizlilik Sözleşmesi kabul etmediğiniz takdirde Cihazınızı sağlamak çalışırken aşağıdaki uyarıyı alırsınız:
> ```
> Unexpected authentication failure. Reason: {
> "authType" : "sa"
>}
>```

Uygulamanız bir iOS cihazında dağıtım için otomatik olarak oturum açmak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Bir iOS projesi Mac için Visual Studio'da açın.

2. Açık **Info.plist** dosya.

3. İçinde **imzalama** bölümünde, select seçin **otomatik sağlama**:

    ![Takım Seçici açılan](automatic-provisioning-images/image2.png)

4. Takımınız seçin **takım** açılır.

6. Birkaç saniye sonra bir imzalama sertifikası ve sağlama profili oluşturulur:

    ![Sertifika ve profili başarıyla oluşturuldu](automatic-provisioning-images/image5.png)

    Otomatik imzalama başarısız olursa **otomatik imzalama paneli** hatanın nedenini gösterir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Bölümünde anlatıldığı gibi bir Mac için Visual Studio 2017 pair [Mac ile eşleştirme](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Kılavuzu.

2. İçinde **Çözüm Gezgini**, proje adını sağ tıklatın ve seçin **özellikleri**. Ardından, gidin **iOS paket grubu imzalama** sekmesi.

3. Seçin **otomatik sağlama** düzeni:

    ![Otomatik düzen seçimi](automatic-provisioning-images/prov4.png)

4. Takımınız seçin **takım** otomatik imzalama işlemini başlatmak için birleşik giriş kutusu.

    ![Ekibin seçimi](automatic-provisioning-images/prov3.png)

4. Bu, otomatik imzalama işlemi başlatır. Visual Studio bu yapıtları imzalama için kullanılacak bir uygulama kimliği, sağlama profili ve imzalama kimliği oluşturmak daha sonra çalışır. Derleme çıktısında oluşturma işleminde görebilirsiniz:

    ![Derleme yapıtları çıkış gösteren oluşturma](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>Otomatik sağlama tetikleme

Otomatik imzalama etkinleştirildiğinde, herhangi aşağıdaki şeylerden biri meydana geldiğinde Mac için Visual Studio bu yapıtları gerekirse güncelleştireceği konum:

* Bir iOS cihazı Mac'inizde prize takılı
    - Bu otomatik olarak Apple Geliştirici Portalı'nda cihazın kayıtlı olup olmadığını denetler. Aksi takdirde, eklemek ve içerdiği yeni bir sağlama profili oluşturun.
* Uygulama paket kimliği değiştirildiğinde
    - Bu uygulama kimliğini güncelleştirir Bu uygulama Kimliğini içeren yeni bir sağlama profili oluşturulur.
* Desteklenen bir özellik Entitlements.plist dosyasında etkinleştirilir.
    - Bu özellik, uygulama kimliği ve güncelleştirilmiş uygulama kimliği oluşturulan yeni bir sağlama profiliyle eklenir.
    - Tüm özellikleri şu anda desteklenmiyor. Desteklenen sürücüler hakkında daha fazla bilgi için kullanıma [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu.


## <a name="related-links"></a>İlgili bağlantılar

- [Ücretsiz Sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Uygulama Dağıtımı](~/ios/deploy-test/app-distribution/index.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
- [Apple - uygulama dağıtım kılavuzu](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
