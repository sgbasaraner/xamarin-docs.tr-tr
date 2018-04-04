---
title: Uygulama mağazası dağıtım
description: Bu belge için Apple App Store dağıtım için gereksinimleri kapsar.
ms.prod: xamarin
ms.assetid: B07E2C1F-A6DF-43CB-BFB0-0252A5558467
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: 7a38c77dde7a66f2db194cd8888a2c32a3529a9a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="app-store-distribution"></a>Uygulama mağazası dağıtım

_Bu belge için Apple App Store dağıtım için gereksinimleri kapsar._

Bir Xamarin.iOS uygulaması geliştirmiştir sonra yazılım geliştirme yaşam döngüsü sonraki adımda iTunes App Store'da kullanarak kullanıcıların uygulamayı dağıtmaktır. Uygulamaları dağıtma en yaygın yolu budur. Apple App Store'dan bir uygulama yayımlayarak, tüketicilere dünya çapında kullanılabilir.

> [!IMPORTANT]
> Bu **önemli** iTunes Bağlan kullanın ve bu nedenle bir uygulamanın uygulama Mağazası'na yayımlamak için not etmek için **gerekir** bireysel veya kurumsal bir Apple Developer Program ya da bir parçası olarak. Apple Geliştirici üye olması durumunda bu sayfada adımları mümkün olmaz **Kurumsal** Program.

Uygulama – yalnızca ile bir uygulama gibi geliştirme – dağıtma uygulamaları olmasını gerektirir uygun kullanılarak sağlanan *sağlama profili*. Sağlama profilleri kod imzalama uygulama ve hedeflenen dağıtım mekanizması kimliğini yanı sıra bilgiler içeren dosyalardır. Ayrıca uygulama dağıtılabilir hangi cihazların uygulama mağazası dağıtım hakkında bilgi içerir.

<a name="provisioning" />

## <a name="provisioning-an-app-for-app-store-distribution"></a>Uygulama mağazası dağıtım için bir uygulama sağlama

Nasıl bir Xamarin.iOS uygulaması yayımlamayı planladığınız bağımsız olarak, yapı gerekecektir bir *dağıtım sağlama profili* onu özgüdür. Bu profili iOS cihazında yüklenebilir böylece sürüm için dijital olarak imzalanması uygulama izin verir. Benzer şekilde bir geliştirme sağlama profili, bir dağıtım profili aşağıdakileri içerir:

- Bir uygulama kimliği
- Bir dağıtım sertifikası

Aynı seçebileceğiniz **uygulama kimliği** ve **aygıtları** sağlama profili geliştirme için kullanılır, ancak zaten yoksa, tanımlamak için bir dağıtım sertifikası oluşturmanız gerekir, Uygulama mağazası uygulaması gönderirken kuruluş. Aşağıdaki bölümde dağıtım sertifikası oluşturma adımları açıklanmaktadır.

> [!NOTE]
> Yalnızca Team aracıları ve yöneticilerin dağıtım sertifikalarını ve sağlama profilleri oluşturabilirsiniz.

<a name="creatingcertificate" />

## <a name="creating-a-distribution-certificate"></a>Dağıtım sertifika oluşturma

1. Gözat *tanımlayıcıları & profilleri, sertifikaları* Apple Developer Member Center'da bölümü.
2. Altında *sertifikaları*seçin **üretim**.
3. Tıklatın **+** yeni bir sertifika oluşturmak için düğmeye.
4. Altında *üretim* başlığını seçin **App Store ve geçici**:

    [![](images/createcertmanually01.png "Uygulama mağazası ve geçici seçin")](images/createcertmanually01.png#lightbox)
5. Tıklatın **devam**ve bir sertifika imzalama isteği Anahtarlık erişim aracılığıyla oluşturmak için yönergeleri izleyin:

    [![](images/createcertmanually02.png "Sertifika imzalama isteği Anahtarlık erişim aracılığıyla oluşturma")](images/createcertmanually02.png#lightbox)
6. CSR belirtildiği şekilde oluşturduktan sonra tıklatın **devam**ve üye Merkezi'nde CSR dosyasını karşıya yükleyin:

    [![](images/createcertmanually03.png "Üye Merkezi CSR dosyasını karşıya yükleyin")](images/createcertmanually03.png#lightbox)

7. Tıklatın **Generate** bir sertifika oluşturmak için.
8. Son olarak, **karşıdan** tamamlanan sertifikayı ve yüklemek için dosyayı çift tıklayın.
9. Bu noktada, sertifika makinede yüklü olmalıdır, ancak gerekebilir [profillerinizi yenileme](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download), Xcode içinde görünür olduğundan emin olun.

Alternatif olarak, xcode'da Tercihler iletişim kutusu üzerinden sertifika istemek mümkündür. Bunu yapmak için aşağıdaki adımları izleyin:

1.   Ekibinizin seçin ve tıklayın **sertifikaları Yönet...** : [ ![ ] (images/selectteam.png "Takım seçip ayrıntıları görüntüle")](images/selectteam.png#lightbox)

2.   Bundan sonra öğesini **oluşturma** düğmesine **iOS dağıtım sertifikası**: [ ![ ] (images/selectcert.png "iOS dağıtım sertifikası oluşturma")](images/selectcert.png#lightbox)

3.   Takım ayrıcalıklarınız bağlı olarak, imzalama kimliğinize, aşağıda gösterildiği gibi oluşturulur veya bir takım Aracısı kadar beklemeniz gerekebilir veya yönetici onaylar: [ ![ ] (images/generated.png "imzalama kimliğinize oluşturulur ve bir Görüntülenen iletişim")](images/generated.png#lightbox)


<a name="creatingprofile" />

## <a name="creating-a-distribution-profile"></a>Bir dağıtım profili oluşturma

<a name="creatingappid" />

### <a name="creating-an-app-id"></a>Bir uygulama kimliği oluşturma

Olarak herhangi diğer sağlama oluşturduğunuz profil ile bir uygulama kimliği kullanıcının cihazına dağıtırken uygulama tanımlamak için gereklidir. Zaten bu oluşturmadıysanız, oluşturmak için aşağıdaki adımları izleyin:


1. İçinde [Apple Geliştirici Merkezi](https://developer.apple.com/account/overview.action) göz *sertifika, tanımlayıcılarını ve profiller* bölümü. Seçin **uygulama kimlikleri** altında **tanımlayıcıları**.
2. Tıklatın **+** düğmesine tıklayın ve sağlayan bir **adı** hangi tanımlar ve bu Portalı'nda.
3. Uygulama önek takım Kimliğiniz ayarlanması gerekir ve değiştirilemez. Explicit veya joker uygulama kimliği seçin ve bir paket kimliği gibi geriye doğru DNS biçimde girin:
    - **Açık**: com [etkialanıadı]. [ AppName]
    - **Joker karakter**: com [etkialanıadı]. *
4. Seçin [uygulama hizmetleri](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) , uygulama gerektirir.
5. Tıklatın **devam** düğmesi ve yeni bir uygulama kimliği oluşturmak için açık ekran yönergeleri izleyerek


### <a name="creating-a-provisioning-profile"></a>Bir sağlama profili oluşturma

Dağıtım profili oluşturmak için gerekli gerekli bileşenleri olduktan sonra onu oluşturmak için aşağıdaki adımları izleyin:

1. Apple sağlama Portalı'na dönün ve seçin **sağlama** > **dağıtım**:

    [![](images/distribute01.png "RSelect sağlama > Dağıtım")](images/distribute01.png#lightbox)

2. Tıklatın **+** düğmesini tıklatın ve olarak oluşturmak istediğiniz dağıtım profili türünü seçin **App Store**:

    [![](images/distribute02.png "Bir uygulama mağazası dağıtım profili oluştur")](images/distribute02.png#lightbox)

3. Tıklatın **devam** düğmesini tıklatın ve uygulama kimliği için bir dağıtım profili oluşturmak istediğiniz aşağı açılan listeden seçin:

    [![](images/distribute03.png "Uygulama Kimliği açılır listeden seçin")](images/distribute03.png#lightbox)

4. Tıklatın **devam** düğmesine tıklayın ve uygulamayı imzalamak için gerekli sertifikayı seçin:

    [![](images/distribute04.png "Uygulamayı imzalamak için gerekli sertifikayı seçin")](images/distribute04.png#lightbox)

5. Tıklatın **devam** düğmesine tıklayın ve Xamarin.iOS uygulaması çalışmasına izin iOS aygıtları seçin:

    [![](images/distribute05.png "İOS seçin aygıtlar bu uygulama izin çalıştırmak için")](images/distribute05.png#lightbox)

6. Tıklatın **devam** düğmesini tıklatın ve girin bir **adı** yeni dağıtım profili için:

    [![](images/distribute06.png "Yeni dağıtım profili için bir ad girin")](images/distribute06.png#lightbox)

7. Tıklatın **Generate** düğmesine yeni profil oluşturmak ve işlemini sonlandırabilir.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 Mac için Visual Studio çıkın ve kullanılabilir oturum kimlikleri ve sağlama profilleri listesini yenile Xcode olması gerekebilir ('ndaki yönergeleri izleyerek [imzalama kimlikleri isteyen](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) bölüm) yeni bir önce Mac için Visual Studio'da dağıtım profili kullanılabilir

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 Visual Studio çıkın ve onun kullanılabilir imzalama kimlikleri ve sağlama profilleri listelerini Xcode (üzerinde yapı ana bilgisayarın Mac) sahip olabilir ('ndaki yönergeleri izleyerek [imzalama kimlikleri isteyen](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) bölüm) Visual Studio'da yeni bir dağıtım profili kullanılabilir olmadan önce.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Bir Xamarin.iOS projesi dağıtım profil seçme

Son yapı iTunes App Store'da satış için bir Xamarin.iOS uygulaması yapmak hazır olduğunuzda, yukarıda oluşturduğunuz dağıtım profili seçin.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 Mac için Visual Studio'da, aşağıdakileri yapın:

1. Proje adına çift **Çözüm Gezgini** düzenleme için açın.
2. Seçin **iOS paket imzalama** ve **yayın | iPhone** gelen **yapılandırma** açılır:

    ![](images/releasexs01.png "Yayın Seç | iPhone yapılandırma açılır gelen")
3. Çoğu durumda, **imzalama kimlik** ve **sağlama profili** varsayılan değerleri bırakılabilir **otomatik** ve Mac için Visual Studio seçin doğru Info.plist paket tanımlayıcısını temel profil:

    ![](images/releasexs02.png "İmzalama kimlik ve sağlama profili otomatik varsayılan değerlere ayarlayın")
4. Gerekli olursa, dağıtım profili (yukarıda oluşturduğunuz bir) ve imzalama kimlik aşağı açılan listeler seçin:

    ![](images/releasexs03.png "İmzalama kimlik ve dağıtım profillerini seçin")
5. Tıklatın **Tamam** değişiklikleri kaydetmek için düğmesi.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 Visual Studio'da, aşağıdakileri yapın:

1. Proje adına sağ tıklayın **Çözüm Gezgini** seçip **özellikleri** düzenleme için açın.
2. Seçin **iOS paket imzalama** ve **yayın | iPhone** gelen **yapılandırma** açılır:

    ![](images/releasevs01.png "Yayın Seç | iPhone yapılandırma açılır gelen")
3. Çoğu durumda, **imzalama kimlik** ve **sağlama profili** varsayılan değerleri bırakılabilir **otomatik** ve Visual Studio doğru profili seçin Info.plist paket tanımlayıcısını dayalı

    ![](images/releasevs02.png "İmzalama kimlik ve sağlama profili otomatik varsayılan değerlere ayarlayın")
4. Gerekli olursa, dağıtım profili (yukarıda oluşturduğunuz bir) ve imzalama kimlik aşağı açılan listeler seçin:

    ![](images/releasevs03.png "İmzalama kimlik ve dağıtım profili seçin")
5. Projenin özelliklerine değişiklikleri kaydedin.

-----

<a name="itunesconnect" />

## <a name="configuring-your-application-in-itunes-connect"></a>İTunes Bağlan uygulamanızı yapılandırma

Uygulama başarıyla sağlandıktan sonra sonraki adıma uygulamalar yapılandırmaktır [iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa), başka şeylerin uygulama mağazasında bulunan iOS uygulamaları yönetmek için Araçlar dayalı olduğu web paketidir.

Xamarin.iOS uygulamanızı düzgün Kurulum olması gerekir ve Apple için gözden geçirilmesi için gönderilebilir ve sonuç olarak, satış veya uygulama mağazasında bulunan ücretsiz bir uygulama olarak serbest bırakılacak önce iTunes Bağlan yapılandırılır.

Daha fazla ayrıntı için lütfen bkz. bizim [uygulama iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) belgeleri.

<a name="submitting" />

## <a name="submitting-an-app-to-itunes-connect"></a>Bir uygulamaya Bağlan iTunes gönderiliyor

Uygulama dağıtım sağlama profili kullanılarak imzalandığını ve uygulama iTunes Bağlan oluşturulduktan sonra uygulamaya gözden geçirilmek üzere Apple için yüklenir. Apple tarafından başarılı gözden geçirme sırasında kullanılabilir uygulama Mağazası'nda hale getirilir.

Uygulama mağazası uygulama yayımlama hakkında daha fazla bilgi için bkz: [uygulama mağazasında yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

<a name="windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Windows geri .app paketleri otomatik olarak kopyalama

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.iOS uygulaması uygulama mağazasında dağıtım için hazırlama anahtar bileşenlerinin ele.

## <a name="related-links"></a>İlgili bağlantılar

- [Uygulama iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store’da Yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Şirket içi dağıtım](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Geçici Dağıtım](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist Dosyası](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA Desteği](~/ios/deploy-test/app-distribution/ipa-support.md)
