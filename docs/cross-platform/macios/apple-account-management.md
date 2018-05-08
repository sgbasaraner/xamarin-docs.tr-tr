---
title: Apple hesap yönetimi
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: a4ca803085f31ff0db5dd4f194b705d765447c9d
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="apple-account-management"></a>Apple hesap yönetimi

Apple hesabı yönetim arabirimi Apple kimliği ile ilişkili tüm geliştirme ekiplerinin görüntülemek için bir yol sağlar Listesini görüntüleyerek her takım hakkında daha fazla ayrıntı görüntülemek de tanır _imzalama kimlikleri_ ve _sağlama profilleri_ makinenize yüklü.

Apple Kimliğinizi kimlik doğrulaması ile komut satırında gerçekleştirilir [fastlane](https://fastlane.tools/). fastlane başarıyla doğrulanması, makinenizde yüklenmelidir. İçinde ayrıntılı fastlane ve nasıl yükleneceği hakkında daha fazla bilgi [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) kılavuzları.

Apple hesabı iletişim aşağıdakileri sağlar:

* **Sertifikaları oluşturmak ve yönetmek** 
* **Oluşturma ve sağlama profilleri yönetme** 

Bunun nasıl yapılacağı hakkında bilgi, bu kılavuzda açıklanan.

Otomatik olarak oluşturup imzalama kimlikleri, uygulama kimlikleri ve sağlama profillerini yönetmek için iOS otomatik sağlama araçları da kullanabilirsiniz.

Bu özellikler kullanma hakkında daha fazla bilgi için başvurmak [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.
️
## <a name="requirements"></a>Gereksinimler

Apple hesap yönetimi, Mac ve Visual Studio 2017 (sürüm 15.7 ve üzeri) için Visual Studio edinilebilir

Bu özelliği kullanmak için bir Apple Geliştirici hesabınızın olması gerekir. Apple Geliştirici hesapları hakkında daha fazla bilgi kullanılabilir [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

- İnternet'e bağlı olduğundan emin olun. Fastlane Apple Geliştirici Portalı ile doğrudan iletişim kurar olmasıdır.
- Olduğundan emin olun [fastlane araçlarının yüklü](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- En son fastlane Araçları'ndan olduğundan emin olun [ https://download.fastlane.tools ](https://download.fastlane.tools).
- Başlamadan önce tüm kullanıcı lisans sözleşmelerini kabul etmek emin olun [Geliştirici Portalı](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Apple Geliştirici hesabı ekleme

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Hesap yönetimi iletişim kutusu açmak için Git **Visual Studio > Tercihler > Apple Developer hesabı**:

    ![Apple Geliştirici hesabı seçenekleri](apple-account-management-images/image1.png)

2. Tuşuna **+** düğmesini aşağıda gösterildiği gibi oturum açma iletişim kutusunda, görüntülemek için: 

    ![fastlane iletişim kutusu.](apple-account-management-images/image2.png)

4. Apple Kimliğinizi ve parolanızı girin ve tıklayın **oturum** düğmesi. Bu işlem, bu makinede güvenli Anahtarlıkta kimlik bilgilerinizi kaydeder. [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) kimlik bilgileriniz güvenli bir şekilde işlemek ve Apple Geliştirici Portalı'na iletmek için kullanılır.
 
5. Seçin **her zaman izin ver** kimlik bilgilerinizi kullanmak Visual Studio izin vermek için uyarı iletişim kutusunda:

    ![Uyarı iletişim her zaman izin ver](apple-account-management-images/image4.png)

6. Hesabınız başarıyla eklendikten sonra Apple Kimliğinizi ve Apple Kimliğinizi parçası olan tüm takımlar görürsünüz.

    ![Apple Geliştirici hesabı iletişim eklenen hesaplarıyla](apple-account-management-images/image5.png)

7. Tüm ekip seçip tuşuna **ayrıntıları görüntüle...** düğme. Bu, tüm oturum kimlikleri ve makinenize yüklü sağlama profilleri listesi görüntülenir:

    ![Kimlikleri imzalama ve sağlama profilleri makinenizde görünümü ayrıntıları ekran gösterme](apple-account-management-images/image6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio 2017 Apple Kimliğinizi eklemeden önce geliştirme ortamınızı olduğundan emin olun [bir Mac yapı konağa eşleştirilmiş](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Hesap Yönetimi penceresini açmak için Git **Araçlar > Seçenekler > Xamarin > Apple hesapları**:

    ![Apple hesapları seçenekleri ekranı](apple-account-management-images/prov1.png)

1. Seçin **Ekle** düğmesine tıklayın ve Apple kimliği ve parola girin:

    ![Kullanıcı adı ve parola iletişim kutusu](apple-account-management-images/prov1a.png)

1. Hesabınız başarıyla eklendikten sonra Apple Kimliğinizi ve Apple Kimliğinizi parçası olan tüm takımlar görürsünüz.
 
1. Tüm ekip seçip tuşuna **ayrıntıları görüntüle...** düğme. Bu, tüm oturum kimlikleri ve makinenize yüklü sağlama profilleri listesi görüntülenir:

    ![Kullanıcı adı ve parola iletişim kutusu](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>İmzalama kimlikleri yönetmek ve sağlama profilleri

Takım Ayrıntıları iletişim imzalama türüne göre düzenlenmiş kimlikleri listesini görüntüler. **Durum** sütun öneren, sertifikanın olup olmadığını: 

* **Geçerli** – (sertifika ve özel anahtarı) imzalama kimliğinize makinenize yüklü ve süresi geçmemiş.

* **Anahtarlık içinde değil** – Apple'nın sunucuda geçerli bir imza kimliği yok. Bunu, makinenizde yüklemek için başka bir makineden verilmelidir. Özel anahtarı içermeyecek şekilde imzalama kimliğinize Apple Geliştirici portalından karşıdan yükleyemiyor.

* **Özel anahtarı eksik** – bir sertifika özel anahtarı ile Anahtarlıkta yüklenir.

* **Süresi dolmuş** – sertifikanın süresi doldu. Bu, Anahtarlık kaldırmanız gerekir.

  ![Takım Ayrıntıları iletişim bilgileri](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>İmzalama kimlik oluşturun

Yeni bir imza kimliği oluşturmak için seçin **oluşturduğunuz sertifika** açılan düğmesine ve ihtiyaç duyduğunuz türünü seçin. Yeni imzalama doğru izinlere sahip kimlik birkaç saniye sonra görünür.

Aşağı açılır bir seçeneği devre dışı ve seçili, bu tür bir sertifika oluşturmak için doğru takım izinlere sahip değil demektir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Sertifika seçeneklerini oluşturma](apple-account-management-images/image8.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Sertifika seçeneklerini oluşturma](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>Sağlama profilleri indirin

Takım Ayrıntıları iletişim ayrıca Geliştirici hesabınızı bağlı tüm sağlama profilleri listesini görüntüler. Tuşlarına basarak tüm sağlama profilleri yerel makinenize indirebilirsiniz **tüm profiller karşıdan** düğmesi

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Sağlama profilleri bölüm indirin](apple-account-management-images/image9.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Sağlama profilleri bölüm indirin](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>iOS paket imzalama

Uygulamanızı bir cihaza dağıtma hakkında daha fazla bilgi için bkz [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="view-details-dialog-is-empty"></a>Ayrıntıları iletişim kutusunu görüntüle boştur

Bu şu anda hata ile ilgili bilinen bir sorun olup [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Mac için Visual Studio en son kararlı sürümü kullandığınızdan emin olun

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Hesabınızda oturum sorunlarıyla karşılaşıyorsanız, lütfen aşağıdakileri deneyin:

* Anahtarlık uygulamayı açın ve altından kategoriyi seçin *parolaları*. Arama `deliver.`ve tüm girişleri silin.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"Hesabı eklenirken hata oluştu. Lütfen bir uygulamaya özgü parolayla oturum"

Hesabınızdaki 2 faktörlü kimlik doğrulamasının etkin olmasıdır. Mac için Visual Studio en son kararlı sürümü kullandığınızdan emin olun

### <a name="failed-to-create-new-certificate"></a>Yeni bir sertifika oluşturulamadı
"Bu tür sertifikalar için üst sınıra ulaştınız"

![Sertifika sınırı iletişim kutusu](apple-account-management-images/image10.png)

En fazla izin verilen sertifika sayısına oluşturulmuş. Bu sorunu gidermek için Gözat [Apple Geliştirici Merkezi](https://developer.apple.com/account/ios/certificate/distribution) ve bir üretim sertifikaları iptal etme.

## <a name="known-issues"></a>Bilinen Sorunlar

* Dağıtım sağlama profilleri varsayılan olarak App Store’u hedefleyecektir. Şirket İçi veya Geçici profiller el ile oluşturulmalıdır.
