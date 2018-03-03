---
title: "Geçici dağıtım"
description: "Bu belge, öncelikle kişilerin geniş bir grupla bir Xamarin.iOS uygulamaları test etmek için kullanılan geçici dağıtım teknikleri genel bir bakış sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3B621CAD-103C-478A-97C3-829015F48D1A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 423240949daf45d8d179a3ca9f89677f490cc24d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="ad-hoc-distribution"></a>Geçici dağıtım

_Bu belge, öncelikle kişilerin geniş bir grupla bir Xamarin.iOS uygulamaları test etmek için kullanılan geçici dağıtım teknikleri genel bir bakış sağlar._

Bir Xamarin.iOS uygulaması geliştirmiştir sonra yazılım geliştirme yaşam döngüsü sonraki adımda kullanıcılara uygulama test etmek için dağıtmaktır.

iTunes Bağlan uygulama test yönetmeye yönelik bir seçenektir ve daha açıklanan [TestFlight](~/ios/deploy-test/testflight.md) Kılavuzu. Ancak, Apple Geliştirici Kurumsal programı üyeleri erişiminiz yok iTunes bağlanılamadı, bunu *geçici* dağıtım, bu uygulamaları test etme için en iyi yöntem.

Xamarin.iOS uygulamaları aracılığıyla kullanıcı olarak test olabilir *geçici* Apple Developer Program ve Apple Geliştirici Kurumsal programı üzerinde kullanılabilir ve 100'e kadar iOS cihazlarını sınanacak sağlayan dağıtım.

Geçici dağıtım App Store onayı gerektirmeyen avantajı vardır ve bir web sunucusundan ya da iTunes aracılığıyla havadan yüklenebilir. Ancak, sınırlıdır **100** hem geliştirme ve dağıtım ve bunlar için üyelik yıllık cihazları el ile eklenmelidir üye Merkezi'nde tarafından kendi UDID. Cihaz ekleme hakkında daha fazla bilgi için [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#adddevice) Kılavuzu.

Geçici dağıtım uygulamaları olmasını gerektirir bir geçici kullanılarak sağlanan *sağlama profili* kod imzalama uygulama ve uygulama yükleyebilirsiniz aygıtları kimliğini yanı sıra, bilgi içeren.

Bu kılavuz bir Xamarin.iOS uygulaması dağıtmak nasıl geçici dağıtım için hazırlama hakkında bilgi ve bilgi sağlar.

<a name="setup" />

## <a name="setting-up-for-distribution"></a>Dağıtım için ayarlama

Test amacıyla şirket içi dağıtım için bir Xamarin.iOS uygulaması yayımlamayı planlama olsa bile bir geçici dağıtım sağlama profili için belirli oluşturulacağı gerekir. Bu profili iOS cihazında yüklenebilir böylece sürüm için dijital olarak imzalanması için bir uygulama sağlar.

Sonraki bölümde dağıtım sertifikası ve sağlama dağıtım profili nasıl oluşturulacağı açıklanmıştır.

> [!NOTE]
>  Not: Yalnızca Team aracıları ve yöneticilerin dağıtım sertifikalarını ve sağlama profilleri oluşturabilirsiniz.

<a name="createcertificate" />

## <a name="create-a-distribution-certificate"></a>Bir dağıtım sertifika oluşturun


1. Gözat *tanımlayıcıları & profilleri, sertifikaları* Apple Developer Member Center'da bölümü.
2. Altında *sertifikaları*seçin **üretim**.
3. Tıklatın  **+**  yeni bir sertifika oluşturmak için düğmeye.
4. Altında *üretim* başlığını seçin **şirket içi ve geçici**, veya **App Store ve geçici**program üyeliğinizi bağlı olarak:

  [ ![](ad-hoc-distribution-images/cert-first-small.png "Şirket içi ve Ad Hoc seçin veya uygulama depolama ve geçici")](ad-hoc-distribution-images/cert-first-large.png)

5. Devam'ı tıklatın ve bir sertifika imzalama isteği Anahtarlık erişim aracılığıyla oluşturmak için yönergeleri izleyin:

  [ ![](ad-hoc-distribution-images/createcertmanually02.png "Sertifika imzalama isteği Anahtarlık erişim aracılığıyla oluşturma")](ad-hoc-distribution-images/createcertmanually02.png)

6. CSR belirtildiği şekilde oluşturduktan sonra devam'ı tıklatın ve üye Merkezi'nde CSR dosyasını karşıya yükleyin:

  [ ![](ad-hoc-distribution-images/createcertmanually03.png "Üye Merkezi CSR dosyasını karşıya yükleyin")](ad-hoc-distribution-images/createcertmanually03.png)

7. Bir sertifika oluşturmak için Oluştur'u tıklatın.
8. Son olarak, tamamlanan sertifika indirmek ve yüklemek için dosyaya çift tıklayın.
9. Bu noktada, sertifika makinede yüklü olmalıdır, ancak gerekebilir [profillerinizi yenileme](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) Xcode'da görünür olduğundan emin olmak için.

Alternatif olarak, xcode'da Tercihler iletişim kutusu üzerinden sertifika istemek mümkündür. Bunu yapmak için aşağıdaki adımları izleyin:

1.   Ekibinizin seçin ve tıklayın **sertifikaları Yönet...** : [ ![ ] (ad-hoc-distribution-images/selectteam.png "Takım seçme")](ad-hoc-distribution-images/selectteam.png)

2.   Bundan sonra öğesini **artı (+)** düğmesine tıklayın ve ardından **iOS App Store'da**: [ ![ ] (ad-hoc-distribution-images/selectcert.png "iOS App Store'da seçme")](ad-hoc-distribution-images/selectcert.png)

<a name="createprofile" />

## <a name="create-a-distribution-provisioning-profile"></a>Sağlama profili bir dağıtım oluşturun

<a name="createappid" />

### <a name="create-an-app-id"></a>Bir uygulama kimliği oluşturma
Olarak herhangi diğer sağlama oluşturduğunuz profil ile bir uygulama kimliği kullanıcının cihaza dağıtılmış uygulamanın tanımlanması için gerekir. Zaten bu oluşturmadıysanız, oluşturmak için aşağıdaki adımları izleyin:


1. İçinde [Apple Geliştirici Merkezi](https://developer.apple.com/account/overview.action) göz *sertifika, tanımlayıcılarını ve profiller* bölümü. Seçin **uygulama kimlikleri** altında **tanımlayıcıları**.
2. Tıklatın  **+**  düğmesine tıklayın ve sağlayan bir **adı** hangi tanımlar ve bu Portalı'nda.
3. Uygulama önek takım Kimliğiniz ayarlanması gerekir ve değiştirilemez. Explicit veya joker uygulama kimliği seçin ve bir paket kimliği gibi geriye doğru DNS biçimde girin:
    - **Explicit**: `com.[DomainName].[AppName]`
    - **Joker karakter**: `com.[DomainName].*`
4. Seçin [uygulama hizmetleri](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) , uygulamanızı gerektirir.
5. Tıklatın **devam** düğmesine tıklayın ve yeni bir uygulama kimliği oluşturmak için açık ekran yönergelerini izleyin

Dağıtım profili oluşturmak için gerekli gerekli bileşenleri olduktan sonra onu oluşturmak için aşağıdaki adımları izleyin:

1. Apple sağlama Portalı'na dönün ve seçin **sağlama > Dağıtım**: [ ![ ] (ad-hoc-distribution-images/distribute01.png "seçin sağlama > Dağıtım")](ad-hoc-distribution-images/distribute01.png)

2. Tıklatın  **+**  düğmesini tıklatın ve olarak oluşturmak istediğiniz dağıtım profili türünü seçin **geçici**:

    [ ![](ad-hoc-distribution-images/distribute02.png "Bir geçici dağıtım türü oluşturma")](ad-hoc-distribution-images/distribute02.png)

3. Tıklatın **devam** düğmesini tıklatın ve uygulama kimliği için bir dağıtım profili oluşturmak istediğiniz aşağı açılan listeden seçin:

    [ ![](ad-hoc-distribution-images/distribute03.png "Uygulama Kimliği açılır listeden seçin")](ad-hoc-distribution-images/distribute03.png)

4. Tıklatın **devam** düğmesi ve select dağıtım sertifikası gerekli uygulamayı imzalamak için:

    [ ![](ad-hoc-distribution-images/distribute04.png "Uygulamayı imzalamak için gereken select dağıtım sertifikası")](ad-hoc-distribution-images/distribute04.png)

6. Tıklatın **devam** düğmesini tıklatın ve girin bir **adı** yeni dağıtım profili için:

    [ ![](ad-hoc-distribution-images/distribute06.png "Yeni dağıtım profili için bir ad girin")](ad-hoc-distribution-images/distribute06.png)

7. Tıklatın **Generate** düğmesine yeni profil oluşturmak ve işlemini sonlandırabilir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio çıkın ve kullanılabilir oturum kimlikleri ve sağlama profilleri listesini yenile Xcode olması gerekebilir ('ndaki yönergeleri izleyerek [indirme profilleri ve sertifikaları xcode'da](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) bölüm) Mac için Visual Studio'da yeni bir dağıtım profili kullanılabilir olmadan önce

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio çıkın ve kullanılabilir oturum kimlikleri ve sağlama profilleri listesini yenile Xcode (üzerinde yapı ana bilgisayarın Mac) sahip olabilir ('ndaki yönergeleri izleyerek [indirme profilleri ve sertifikalarıxcode'da](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)bölüm) yeni bir dağıtım profili Visual Studio'da kullanılabilir olmadan önce.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Bir Xamarin.iOS projesi dağıtım profil seçme

Bir Xamarin.iOS uygulaması son derlemesinde yapmak hazır olduğunuzda, yukarıda oluşturduğunuz dağıtım profili seçin.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 Mac için Visual Studio'da, aşağıdakileri yapın:

1. Proje adına çift **Çözüm Gezgini** düzenleme için açın.
2. Seçin **iOS paket imzalama** ve derlemeden tür **yapılandırma** açılır:

    ![](ad-hoc-distribution-images/releasexs01.png "Derleme yapılandırması açılan listeden seçin")
3. Çoğu durumda, **imzalama kimlik** ve **sağlama profili** varsayılan değerler olarak bırakılabilir **otomatik** ve Mac için Visual Studio seçin doğru Info.plist paket tanımlayıcısını temel profil:

    ![](ad-hoc-distribution-images/releasexs02.png "İmzalama kimlik ve sağlama profili otomatik varsayılan değerlere ayarlayın")
4. Gerekli olursa, dağıtım profili (yukarıda oluşturduğunuz bir) ve imzalama kimlik aşağı açılan listeler seçin:

    ![](ad-hoc-distribution-images/releasexs03.png "İmzalama kimlik ve dağıtım profili seçin")
5. Tıklatın **Tamam** değişiklikleri kaydetmek için düğmesi.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
 Visual Studio'da, aşağıdakileri yapın:

1. Proje adına sağ tıklayın **Çözüm Gezgini** seçip **özellikleri** düzenleme için açın.
2. Seçin **iOS paket imzalama** ve derlemeden tür **yapılandırma** açılır:

    ![](ad-hoc-distribution-images/releasevs01.png "Derleme yapılandırması açılan listeden seçin")
3. Çoğu durumda, **imzalama kimlik** ve **sağlama profili** varsayılan değerler olarak bırakılabilir **otomatik** ve Visual Studio doğru profili seçin Info.plist paket tanımlayıcısını temel:

    ![](ad-hoc-distribution-images/releasevs02.png "İmzalama kimlik ve sağlama profili otomatik varsayılan değerlere ayarlayın")
4. Gerekli olursa, dağıtım profili (yukarıda oluşturduğunuz bir) ve imzalama kimlik aşağı açılan listeler seçin:

    ![](ad-hoc-distribution-images/releasevs03.png "İmzalama kimlik ve dağıtım profili seçin")
5. Projenin özelliklerine değişiklikleri kaydedin.

-----

<a name="adhoc" />

## <a name="ad-hoc-distribution"></a>Geçici dağıtım

Sırada [TestFlight](~/ios/deploy-test/testflight.md) beta popüler bir çeşit sınamak ve dağıtım, iTunes Bağlan bir parçasıdır ve bu nedenle, üyelerine kullanılamıyor **Apple Geliştirici Kurumsal programı**.

Geçici dağıtım geliştiricileri çok çeşitli iTunes bağlandığınızda aygıtları beta test uygulamaları için bir seçenek değil sağlar. Geçici şirket içi dağıtım benzer şekilde çalışır ve hangi sonra havadan, ya da el ile iTunes üzerinden dağıtılabilir oluşturulacak bir IPA gerektirir.

<a name="IPA_Creation" />

### <a name="ipa-support-for-ad-hoc-deployment"></a>Geçici dağıtım IPA desteği

Sağlanan sonra uygulamaları olarak bilinen bir dosyaya paketlenmesi bir *IPA*. Ek meta veri ve simgeleri birlikte uygulamayı içeren bir zip dosyası budur. IPA, böylece doğrudan sağlama profilinde bir cihaza eşitlenebilen iTunes yerel bir uygulamaya eklemek için kullanılır.

Bir IPA oluşturma hakkında daha fazla bilgi için bkz: [IPA Destek](~/ios/deploy-test/app-distribution/ipa-support.md) Kılavuzu.

## <a name="summary"></a>Özet

Bu makalede Xamarin.iOS uygulamaları test etmek için gerekli olan geçici dağıtım mekanizmaları açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Uygulama mağazası dağıtım](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Şirket içi dağıtım](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [İTunesMetadata.plist dosyası](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA desteği](~/ios/deploy-test/app-distribution/ipa-support.md)
