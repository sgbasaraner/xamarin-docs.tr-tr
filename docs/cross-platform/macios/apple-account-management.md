---
title: Apple hesap yönetimi
description: Bu belge, Mac ve Visual Studio 2017 için Visual Studio için Apple hesap yönetimi özelliklerini kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 4557d3b055e5c49842b9fdcff1dac9ee996e8bab
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986024"
---
# <a name="apple-account-management"></a>Apple hesap yönetimi

Apple hesap yönetimi arabirimi Apple kimliği ile ilişkili tüm geliştirme ekipleri görüntülemek için bir yol sağlar. Listesini görüntüleyerek her takım hakkında daha fazla ayrıntı görüntülemek de tanır _imzalama kimlikleri_ ve _sağlama profilleri_ makinenizde yüklü.

Apple Kimliğinizi kimlik doğrulaması ile komut satırında gerçekleştirilir [fastlane](https://fastlane.tools/). kimlik doğrulamasını başarılı makinenizde fastlane yüklenmesi gerekir. Fastlane'i edinin ve nasıl yükleneceği hakkında daha fazla bilgi ayrıntılı [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) Kılavuzlar.

Apple hesabı iletişim aşağıdakileri sağlar:

* **Sertifikaları oluşturmak ve yönetmek**
* **Oluşturma ve sağlama profillerini yönetme**

Bunun nasıl yapılacağı hakkında bilgi, bu kılavuzda açıklanmıştır.

> [!NOTE]
> Apple hesap yönetimi için Xamarin'in Araçlar, yalnızca ücretli Apple Geliştirici hesapları hakkında bilgi görüntüler. Ücretli Apple Geliştirici hesabı olmayan bir cihazdaki bir uygulamayı test etme konusunda bilgi almak için lütfen bkz [ücretsiz xamarin iOS uygulamaları için sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md) Kılavuzu.

Ayrıca, otomatik olarak oluşturup, imzalama kimlikleri, uygulama kimlikleri ve sağlama profillerini yönetmek için iOS otomatik sağlama araçları da kullanabilirsiniz. Bu özellikleri kullanmaya ilişkin daha fazla bilgi için [cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

## <a name="requirements"></a>Gereksinimler

Apple hesap yönetimi için Visual Studio Mac ve Visual Studio 2017 (sürüm 15.7 ve üzeri) üzerinde kullanılabilir

Bu özelliği kullanmak için bir Apple Developer hesabınız olmalıdır. Apple Geliştirici hesapları hakkında daha fazla bilgi kullanılabilir [cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

- İnternet'e bağlı olduğunuzdan emin olun. Fastlane doğrudan Apple Developer portal ile iletişim kuran olmasıdır.
- Olduğundan emin olun [fastlane araçları yüklü](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- En son fastlane araçlarından sahip olduğunuzdan emin olun [ https://download.fastlane.tools ](https://download.fastlane.tools).
- Başlamadan önce içinde herhangi bir kullanıcı lisans sözleşmelerini kabul ettiğinizden emin olun [Geliştirici Portalı](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Bir Apple Geliştirici hesabı ekleme

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Hesap yönetimi iletişim kutusunu açmak için Git **Visual Studio > Tercihler > Apple Geliştirici hesabı**:

    ![Apple Geliştirici hesabı seçenekleri](apple-account-management-images/image1.png)

2. Tuşuna **+** düğmesini oturum açma iletişim kutusunda, aşağıda gösterildiği şekilde görüntülemek için: 

    ![fastlane iletişim.](apple-account-management-images/image2.png)

4. Apple kimliği ve parola girin ve tıklayın **oturum** düğmesi. Bu işlem, bu makinede güvenli Anahtarlıkta kimlik bilgilerinizi kaydeder. [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) kimlik bilgilerinizi güvenli bir şekilde işlemek ve Apple'nın Geliştirici portalında geçirileceğini için kullanılır.
 
5. Seçin **her zaman izin ver** kimlik bilgilerinizi kullanmanız Visual Studio izin vermek için uyarı iletişim kutusunda:

    ![Uyarı iletişim kutusu her zaman izin ver](apple-account-management-images/image4.png)

6. Hesabınızı başarıyla eklendikten sonra Apple Kimliğinizi ve Apple Kimliğinizi parçası olan herhangi bir ekibin görürsünüz.

    ![Apple Geliştirici hesabı iletişim hesaplarıyla eklendi](apple-account-management-images/image5.png)

7. Tüm takım seçip ENTER tuşuna **ayrıntıları görüntüle...** düğmesi. Bu, tüm imzalama kimlikleri ve makinenizde yüklü sağlama profilleri listesi görüntülenir:

    ![İmzalama kimlikleri ve sağlama profilleri makinenizde görünümü ayrıntıları ekran gösterme](apple-account-management-images/image6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio 2017'ye Apple KİMLİĞİNİZİ'i eklemeye başlamadan önce geliştirme ortamınızı olduğundan emin olun [eşleştirilmiş bir Mac derleme konağı](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Hesap Yönetimi penceresini açmak için Git **Araçlar > Seçenekler > Xamarin > Apple hesapları**:

    ![Apple hesapları seçenekleri ekranı](apple-account-management-images/prov1.png)

1. Seçin **Ekle** düğmesine tıklayın ve Apple kimliği ve parola girin:

    ![Kullanıcı adı ve parola iletişim kutusu](apple-account-management-images/prov1a.png)

1. Hesabınızı başarıyla eklendikten sonra Apple Kimliğinizi ve Apple Kimliğinizi parçası olan herhangi bir ekibin görürsünüz.
 
1. Tüm takım seçip ENTER tuşuna **ayrıntıları görüntüle...** düğmesi. Bu, tüm imzalama kimlikleri ve makinenizde yüklü sağlama profilleri listesi görüntülenir:

    ![Kullanıcı adı ve parola iletişim kutusu](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>Sağlama profillerini ve imzalama kimlikleri yönetme

Takım Ayrıntıları iletişim imzalama türüne göre düzenlenmiş kimlikleri listesini görüntüler. **Durumu** sütun öneren, sertifikanın olup olmadığını: 

* **Geçerli** – imzalama kimliği (hem sertifika hem de özel anahtar), makinenizde yüklü ve süresi geçmemiş.

* **Anahtarlıktaki değil** – Apple'nın sunucuda geçerli bir imzalama kimliği yok. Bu, makinenize yüklemek için başka bir makineden verilmelidir. Özel anahtarı içermeyecek şekilde Apple Geliştirici Portalı'ndan imzalama kimliği karşıdan yükleyemiyor.

* **Özel anahtar eksik** – hiçbir özel anahtara sahip bir sertifika anahtar zinciri'ne yüklenir.

* **Süresi dolan** – sertifikanın süresi doldu. Bu, anahtarlığınızdan kaldırmanız gerekir.

  ![Takım Ayrıntıları iletişim bilgileri](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Bir imzalama kimlikleri oluşturma

Yeni bir imza kimliği oluşturmak için Seç **oluşturduğunuz sertifika** açılan düğmeyi ve gereksinim türünü seçin. Yeni imzalama doğru izinlere sahip kimlik birkaç saniye sonra görünür.

Aşağı açılan bir seçeneği gri ve seçili, bu tür bir sertifika oluşturmak için doğru takım izinlere sahip değilsiniz demektir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Sertifika seçenekleri oluşturma](apple-account-management-images/image8.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Sertifika seçenekleri oluşturma](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>Sağlama profilleri indir

Takım Ayrıntıları iletişim de Geliştirici hesabınıza bağlı tüm sağlama profillerinin bir listesini görüntüler. Yerel makinenize tuşlarına basarak tüm sağlama profilleri indirebilirsiniz **tüm profilleri indir** düğmesi

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Sağlama profilleri bölümünü indirin](apple-account-management-images/image9.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Sağlama profilleri bölümünü indirin](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>iOS paket grubu imzalama

Bir cihaz için uygulamanızı dağıtma hakkında daha fazla bilgi için başvurmak [cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="view-details-dialog-is-empty"></a>Görünümü Ayrıntılar iletişim kutusu boştur

Şu anda hata ile ilgili bilinen bir sorun budur [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Mac için Visual Studio'nun en son kararlı sürümü kullandığınızdan emin olun

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Hesabınızda oturum sorunları yaşıyorsanız, lütfen aşağıdakileri deneyin:

* Anahtarlık uygulamasını açın ve altından kategoriyi seçin *parolaları*. Arama `deliver.`ve tüm girişleri silin.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"Hesabı eklenirken hata oluştu. Lütfen bir uygulamaya özgü parolayla oturum"

Hesabınızdaki 2 faktörlü kimlik doğrulaması etkin olmasıdır. Mac için Visual Studio'nun en son kararlı sürümü kullandığınızdan emin olun

### <a name="failed-to-create-new-certificate"></a>Yeni sertifika oluşturulamadı
"Bu tür sertifikaların sınırına"

![Sertifika sınırı iletişim kutusu](apple-account-management-images/image10.png)

Oluşturulan sertifika izin verilen maksimum sayısı. Bu sorunu gidermek için Gözat [Apple Developer Center'da](https://developer.apple.com/account/ios/certificate/distribution) ve bir üretim sertifika iptal etme.

## <a name="known-issues"></a>Bilinen Sorunlar

* Dağıtım sağlama profilleri varsayılan olarak App Store’u hedefleyecektir. Şirket İçi veya Geçici profiller el ile oluşturulmalıdır.
