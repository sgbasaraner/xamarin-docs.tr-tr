---
title: El ile Xamarin.iOS için sağlama
description: Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlama sağlamaktır. Bu kılavuz, geliştirme sertifika ve profilleri ayarlamak için kullanarak el ile sağlama keşfediyor.
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/15/2017
ms.openlocfilehash: dd0afe03adbd021717a88cd4409e3e1351ba9b50
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111192"
---
# <a name="manual-provisioning-for-xamarinios"></a>El ile Xamarin.iOS için sağlama

_Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlama sağlamaktır. Bu kılavuz, geliştirme sertifika ve profilleri ayarlamak için kullanarak el ile sağlama keşfediyor._

> [!NOTE]
> Bu sayfadaki yönergeleri, erişim için Apple Developer Program ödemesi geliştiriciler için uygundur. Bir ücretsiz hesabınız varsa, lütfen göz atın [ücretsiz sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md) cihazda test etme hakkında daha fazla bilgi için Kılavuzu.

## <a name="creating-a-signing-identity"></a>İmzalama kimliği oluşturma

Geliştirme cihazı kurarken ilk adımı, imzalama kimliği oluşturmaktır. İmzalama kimliği iki şey oluşur:

- Bir geliştirme sertifikası
- Özel anahtar

Geliştirme sertifikası ve ilişkili [anahtarları](#understanding-certificate-key-pairs) bir iOS geliştirici için kritik öneme sahiptir: Apple kimliğinizi oluşturmak ve belirli bir cihaz ve dijital imza koyarak yakındır geliştirme profili ile ilişkilendirme uygulamalarınızı. Apple sertifikaları cihazlara erişimi denetlemesine izin dağıtmak için izin verilen denetler.

Geliştirme ekipleri, sertifika ve profilleri erişerek yönetilebilir [tanımlayıcıları & profilleri, Sertifikalar](https://developer.apple.com/account/overview.action) Apple'nın üye Merkezi'nde bölümünü (oturum açma gereklidir). Apple cihaz veya simülatör için kodunuzu derlemek için imzalama kimliği olmasını gerektirir.  

> [!IMPORTANT]
> Herhangi bir anda yalnızca iki iOS geliştirme sertifikaları olabilir dikkat edin önemlidir. Daha fazla oluşturmanız gerekiyorsa, mevcut bir iptal gerekecektir. İptal edilen sertifikalar kullanarak herhangi bir makine, uygulamayı imzalamak mümkün olmayacaktır.

İmzalama kimliği oluşturmak için aşağıdakileri yapın:

1. Oturum açma [sertifikalar ve tanımlayıcılar profilleri bölümünde Geliştirici portalının](https://developer.apple.com/account/overview.action) seçip **sertifikaları** bölümü **iOS uygulamaları** sütun. Ardından, isabet **+** yeni bir sertifika oluşturmak için:

    [![](manual-provisioning-images/cert-plus.png "' A tıklayın ve yeni bir sertifika oluşturmak için")](manual-provisioning-images/cert-plus.png#lightbox)

2. Seçin **iOS uygulama geliştirmeyi** tıklayın ve sertifika türüne ilişkin seçenek **devam**. Bu ekran, hesap ayrıcalığı bağlı olarak farklı görünebilir:

    [![](manual-provisioning-images/cert-first.png "İOS sertifika türü için uygulama geliştirme seçeneği seçin")](manual-provisioning-images/cert-first.png#lightbox)

3. Bir sertifika imzalama, karşıya yüklenecek bir sertifikayı el ile oluşturmak için isteği, istek. Bunu yapmak için başlatma **Anahtarlık erişimi** bir Mac Ana menüye gidin ve **sertifika Yardımcısı** ve **bir sertifika yetkilisinden bir sertifika isteği...** aşağıda gösterildiği gibi:

      [![](manual-provisioning-images/key-first.png "Sertifika imzalama isteği iste")](manual-provisioning-images/key-first.png#lightbox)

4. Bilgilerinizi doldurun ve seçeneğini **diske Kaydet**:

    [![](manual-provisioning-images/key-second.png "Bilgilerinizi doldurun")](manual-provisioning-images/key-second.png#lightbox)

5. CSR bunu kolayca bulunabileceği bir konuma kaydedin:

    [![](manual-provisioning-images/cert-third.png "CSR Kaydet")](manual-provisioning-images/cert-third.png#lightbox)

6. Sağlama portala geri dönün, portala sertifikayı karşıya yüklemek ve gönderin:

    [![](manual-provisioning-images/cert-second.png "Portala sertifikasını karşıya yükle")](manual-provisioning-images/cert-second.png#lightbox)

    Yönetici ayrıcalıklarına sahip değil, Sertifika Yöneticisi veya ekip aracısı tarafından onaylanması gerekir.

7. Sertifika onaylandıktan sonra sağlama Portalı'ndan indirin:

    [![](manual-provisioning-images/status-dev.png "Sertifika sağlama Portalı'ndan indirin")](manual-provisioning-images/status-dev.png#lightbox)

8. İndirilen sertifika anahtar zinciri erişimi'ni başlatın ve açmak için çift tıklayın **My sertifikaları** panelinde yeni sertifikaları ve ilişkili özel anahtarı gösteriliyor:

    [![](manual-provisioning-images/keychain.png "Anahtarlık erişimi'nde sertifika")](manual-provisioning-images/keychain.png#lightbox)

### <a name="understanding-certificate-key-pairs"></a>Sertifika için anahtar çiftleri anlama

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Geliştirici profili, sertifikaları, bunların ilişkili anahtarları ve hesapla ilişkili hiç sağlama profili içerir. Geliştirici profili aslında iki sürümü vardır: bir geliştirici portalında ve diğer yerel bir Mac üzerinde yaşadığı İkisi arasındaki fark içerdikleri anahtarları türüdür: _kopyalama yerel mac'inizde tüm özel anahtarları içerirken, sertifika ile ilişkili tüm ortak anahtarları portalında profili barındırıldığı_. Geçerli olması sertifikaları için anahtar çiftleri eşleşmesi gerekir. Özel anahtarlarının kaybolması durumunda, sertifikaları ve sağlama profillerinin oluşturulması gerekir çünkü Geliştirici profilinin bir yedekleme yerel Mac üzerinde tutun.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Geliştirici profili, sertifikaları, bunların ilişkili anahtarları ve hesapla ilişkili hiç sağlama profili içerir. Geliştirici profili aslında iki sürümü vardır: bir geliştirici portalında ve başka bir Mac üzerinde yaşadığı İkisi arasındaki fark içerdikleri anahtarları türüdür: _tüm ortak anahtarları, kopya Mac iken sertifikalarınızı ile ilişkili portalı evler profilindeki tüm özel anahtarları içeren_. Geçerli olması sertifikaları için anahtar çiftleri eşleşmesi gerekir. Özel anahtarlarının kaybolması durumunda, sertifikaları ve sağlama profillerinin oluşturulması gerekir çünkü Geliştirici profili yedeğini Xamarin derleme konağın Mac'ten takip edin.

-----

> [!WARNING]
> Sertifikayı ve ilişkili anahtarları kaybetme mevcut sertifikaların iptal edilmesini gerektirecek ve ilişkili tüm cihazları yeniden sağlama dahil olmak üzere kayıtlı geçici dağıtım için son derece karışıklığa neden olabilir. Başarılı bir şekilde geliştirme sertifikaları ayarlayarak sonra yedekleme kopyasını dışarı aktarın ve bunları güvenli bir yerde saklayın. Bunun nasıl yapılacağı hakkında daha fazla bilgi için aktarma, sertifikaları içeri aktarma ve Profiller bölümüne bakın [koruma sertifikaları](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html) içinde Apple'nın docs Kılavuzu.

<a name="provisioning" />

## <a name="provisioning-an-ios-device-for-development"></a>Bir iOS geliştirme için cihazı sağlama

Apple kimliğinizi belirlediğinize ve geliştirme sertifikası sahip olduğunuzdan, uygulama için bir Apple cihazı dağıtma olanağı, bu nedenle bir sağlama profili ve gerekli varlıkları ayarlamanız gerekir. Cihazın iOS Xcode tarafından desteklenen bir sürümü çalıştırmalıdır: cihaz, Xcode veya her ikisi de güncelleştirmeniz gerekebilir.

<a name="adddevice" />

## <a name="add-a-device"></a>Cihaz ekleme

Geliştirme için bir sağlama profili oluştururken, biz hangi cihazların uygulama çalıştırılabilir belirtmelidir. Bunu etkinleştirmek için Takvim yılı başına 100 cihaz varan geliştirme Portalımızı eklenebilir ve buradan için belirli bir sağlama profili eklenecek cihazları seçeneğini belirleyebiliriz. Mac için geliştirici portalına bir cihaz eklemek için aşağıdaki adımları izleyin.

1. Xcode başlatın.
2. Sağlanan, USB kablosu ile Mac sağlanacak cihazı bağlayın.
2. Gelen **Windows** menüsünü seçin **cihazları**:

  [![](manual-provisioning-images/add01.png "Windows Menüsü'nden cihazları seçin.")](manual-provisioning-images/add01.png#lightbox)

3. İstenen bir iOS cihazından seçin **CİHAZLARI** cihazları pencerenin sol tarafındaki listesi.
4. Vurgulama **tanımlayıcı** dize ve Pano'ya kopyalayın:

  [![](manual-provisioning-images/add02.png "Kimlik dizesi vurgulayın")](manual-provisioning-images/add02.png#lightbox)

5. Safari'de yer gidin [Apple Developer Center'da](https://developer.apple.com/membercenter/index.action) ve oturum açın.
6. Tıklayın **tanımlayıcıları & profilleri, Sertifikalar** bağlantı:

  [![](manual-provisioning-images/add03.png "Sertifikalar'ı tıklatın, tanımlayıcılar profilleri bağlantı")](manual-provisioning-images/add03.png#lightbox)

7. Tıklayarak **cihazları** bağlantı:

  [![](manual-provisioning-images/add04.png "Cihazları bağlantıya tıklayın")](manual-provisioning-images/add04.png#lightbox)

8. Tıklayın **+** düğmesi:

  [![](manual-provisioning-images/add05.png "Tıklayın + düğmesi")](manual-provisioning-images/add05.png#lightbox)

9. Yeni cihaz için bir ad sağlayın ve cihazın yapıştırın **tanımlayıcı** , biz üstüne kopyalanmasını **UUID** alan:

  [![](manual-provisioning-images/add06.png "Yeni cihaz ve cihaz tanımlayıcısı için bir ad sağlayın")](manual-provisioning-images/add06.png#lightbox)

10. Tıklayın **devam** düğmesi.
11. Son olarak, bilgileri gözden geçirin ve tıklayın **kaydetme** düğmesi:

  [![](manual-provisioning-images/add07.png "Bilgileri gözden geçirin")](manual-provisioning-images/add07.png#lightbox)

Bir Xamarin.iOS uygulaması hata ayıklama veya test için kullanılan iOS cihazlar için yukarıdaki adımları yineleyin.

Cihazın Geliştirici portalına ekledikten sonra bir sağlama profili oluşturun ve cihaz eklemek gereklidir.

<a name="provisioningprofile" />

## <a name="creating-a-development-provisioning-profile"></a>Bir geliştirme sağlama profili oluşturma

Geliştirme sertifikası ile sağlama profilleri aracılığıyla el ile oluşturulabilir gibi [tanımlayıcıları & profilleri, Sertifikalar](https://developer.apple.com/account/overview.action) Apple'nın üyeleri Merkezi bölümü.

Bir sağlama profili oluşturmadan önce bir *uygulama kimliği* yapılması gerekir. Uygulama Kimliği uygulamanın benzersiz olarak tanımlayan bir geriye doğru DNS stili dizedir. Aşağıdaki adımların nasıl oluşturulacağını gösteren bir **joker uygulama kimliği**, derleme ve çoğu uygulamaları yüklemek için kullanılabilir. **Açık uygulama kimlikleri** yalnızca (eşleşen paket kimliği ile) bir uygulama yüklenmesine izin ver ve Apple Pay ve HealthKit gibi belirli iOS özellikler için genel olarak kullanılır. Açık uygulama kimlikleri oluşturma hakkında daha fazla bilgi için bkz [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu.

### <a name="app-id"></a>Uygulama Kimliği

1. İçinde [Geliştirici Portalı](https://developer.apple.com/account/overview.action) göz atın *sertifika, tanımlayıcılarını ve profillerini* Apple Geliştirici Merkezi'ndeki bölümü. Seçin **uygulama kimlikleri** altında **tanımlayıcıları**.
2. Tıklayın **+** düğmesine tıklayın ve sağlayan bir **adı**:

    [![](manual-provisioning-images/appid05a.png "Bir ad sağlayın")](manual-provisioning-images/appid05a.png#lightbox)
3. Uygulama ön eki önceden. Seçin **joker uygulama kimliği** uygulama sonek için. Bir paket kimliği şu biçimde girin `com.[DomainName].*`:

  [![](manual-provisioning-images/appid05b.png "Paket Kimliğini girin")](manual-provisioning-images/appid05b.png#lightbox)

3. Tıklayın **devam** düğmesi ve yeni uygulama kimliğini oluşturmak için açık ekran yönergeleri

### <a name="provisioning-profile"></a>Sağlama profili

Uygulama Kimliği oluşturulduktan sonra sağlama profilinin üretilebilir. Bu sağlama profili, ilgili bilgiler içerir. *ne* uygulama (veya joker uygulama kimliği ise uygulamaları) Bu profil ilişkili *kimin* (bağlı olarak hangi geliştirici sertifikaları eklenir), profil kullanabilirsiniz ve *ne* cihazlar, uygulamayı yükleyebilir.

El ile geliştirme için bir sağlama profili oluşturmak için şunu yapın:

1. Gözatmak için Safari kullanmanız [Apple Geliştirici üye Merkezi](https://developer.apple.com/membercenter/index.action)ve bölümünde *tanımlayıcıları & profilleri, Sertifikalar* sağlama profillerini seçin.
2. Tıklayın **+** düğmesi, yeni bir profil oluşturmak için sağ üst köşedeki.
3. Gelen **geliştirme** bölümünde, radyo düğmesini işaretleyin **iOS uygulama geliştirmeyi**basın **devam**:

    [![](manual-provisioning-images/provisioning-profile01.png "Oluşturmak için profili türünü seçin")](manual-provisioning-images/provisioning-profile01.png#lightbox)
4. Uygulamayı seçin açılan menüden kullanacak şekilde kimliği:

    [![](manual-provisioning-images/provisioning-profile02.png "Uygulamayı seçin, kullanılacak kimliği")](manual-provisioning-images/provisioning-profile02.png#lightbox)
5. Sağlama profili ve ENTER tuşuna dahil sertifikaları seçin **devam**:

    [![](manual-provisioning-images/provisioning-profile03.png "Sağlama profilinde içerecek şekilde sertifikaları seçin")](manual-provisioning-images/provisioning-profile03.png#lightbox)
6. Uygulamanın üzerinde yüklü olan tüm cihazları seçin.

    [![](manual-provisioning-images/provisioning-profile04.png "Uygulamanın üzerinde yüklü tüm cihazları seçin")](manual-provisioning-images/provisioning-profile04.png#lightbox)
7. Sağlama profili tanımlanabilen bir ad ve ENTER tuşuna sağlamak **devam** profili oluşturmak için:

    [![](manual-provisioning-images/provisioning-profile05.png "Sağlama profili ile bir tanımlanabilen bir ad sağlayın")](manual-provisioning-images/provisioning-profile05.png#lightbox)
8. Tuşuna **indirme** sağlama profilinin Mac üzerine indirmek için:

    [![](manual-provisioning-images/provisioning-profile06.png "Sağlama profili indirin")](manual-provisioning-images/provisioning-profile06.png#lightbox)
9. Xcode'da sağlama profili yüklemek için dosyaya çift tıklayın. Xcode herhangi bir görsel açma dışında profili yüklendiğini clues gösterilmeyebilir unutmayın. Bunu göz atarak doğrulanabilir **Xcode > Tercihler > hesapları**. Apple Kimliği'ni seçip tıklayın **ayrıntıları görüntüle...** . Yeni sağlama profili, aşağıda gösterildiği gibi listelenmesi gerekir:

      [![](manual-provisioning-images/provisioning-profile07.png "Xcode'da profili görüntüleme")](manual-provisioning-images/provisioning-profile07.png#lightbox)

Sağlama profili başarıyla oluşturulduktan sonra tüm geliştirme sertifikaları, Mac ve Visual Studio için Visual Studio için kullanılabilir olacak şekilde Xcode yenilemek gerekli olabilir.

<a name="download" />

## <a name="downloading-profiles-and-certificates-in-xcode"></a>Profilleri ve sertifikaları xcode'da indiriliyor

Sertifikalar ve Apple Geliştirici Portalı'nda oluşturulan sağlama profilleri otomatik olarak görünmeyebilir Xcode'da. Bu nedenle, bu nedenle indirmek gerekebilir, bunların Visual Studio tarafından Mac ve Visual Studio için erişilebilir olduğunu. Apple Geliştirici Portalı'nda oluşturulan herhangi bir sertifika indirin ve güncelleştirmek için aşağıdakileri yapın:

1.   Visual Studio veya Mac için Visual Studio çıkın.
2.   Xcode başlatın.
3.   Seçin **Xcode menüsü > Tercihler...**
4.   Tıklayın **hesapları** sekmesi.
5.   Bir takım seçin ve tıklayın **el ile profil indirme** düğmesi: [ ![ ] (manual-provisioning-images/selectteam1.png "el ile yükleme profilleri")](manual-provisioning-images/selectteam1.png#lightbox)

6.   Xcode çıkın.
7.  Visual Studio veya Mac için Visual Studio'yu başlatın.

Sağlama profili ve yeni sertifikalar, Visual Studio veya Mac için Visual Studio içinde kullanılabilir ve kullanılmaya hazır olacaktır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

> [!IMPORTANT]
> Durdur ve herhangi bir yeni veya değiştirilmiş sertifikalar veya profilleri Xcode tarafından güncelleştirilmiş görürsünüz önce Mac için Visual Studio'yu yeniden başlatmanız gerekebilir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> Durdur ve herhangi bir yeni veya değiştirilmiş sertifikalar veya profilleri Xcode tarafından güncelleştirilmiş görürsünüz önce Visual Studio'yu yeniden başlatmanız gerekebilir.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Uygulama hizmetleri için hazırlama

Apple, özel uygulama hizmetleri için bir Xamarin.iOS uygulaması etkinleştirilebilen özellikleri olarak da bilinir, seçimi sağlar. Bu uygulama hizmetleri, hem iOS sağlama portalı yapılandırılmalıdır olduğunda **uygulama kimliği** oluşturulur ve **Entitlements.plist** dosya Xamarin.iOS uygulama projesinin bir parçası. Uygulama Hizmetleri uygulamanıza ekleme hakkında daha fazla bilgi için bkz [özellikleri giriş](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu ve [Yetkilendirmelerle çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

* Gerekli uygulama hizmetleri ile bir uygulama kimliği oluşturun.
* Yeni bir [sağlama profili](#provisioningprofile) , bu uygulama kimliğini içerir.
* Yetkilendirmeler Xamarin.iOS projesi Ayarla

## <a name="deploying-to-a-device"></a>Bir cihaza dağıtma

Bu noktada sağlama tamamlanmış olmalıdır ve uygulama cihaza dağıtılmaya hazırdır. Bunu yapmak için aşağıdaki adımları izleyin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

> [!IMPORTANT]
> Başlamadan önce seçtiğinizden emin olun **el ile sağlama** içinde **Info.plist**.

1. Cihazı bir mac'e takılır
2. Projenin **Info.plist**, paket grubu tanımlayıcısı, uygulama kimliği (uygulama kimliği bir joker karakter değilse) eşleştiğinden emin olun:

  ![](manual-provisioning-images/deploydevice01xs.png "Bir tanımlayıcı girme")

3. Göz atın ve proje Seçenekleri iletişim kutusunu görüntülemek için projeyi sağ **Yapı > iOS paket grubu imzalama**. Her ikisi de yanındaki aşağı açılan listeden **imzalama kimliği** ve **sağlama profili**, Mac için Visual Studio doğru profilleri bakın ve bir belirli kimlik & profili seçin doğrulayın:

  ![](manual-provisioning-images/deploydevice02xs.png "Bir belirli kimlik & profili seçin")

Bu ayarlanırsa **otomatik**, Visual Studio Mac profili ayarlanmış paket Kimliğini ve kimlik seçin. adımda #2.

4. Derleme yapılandırması ayarlayın sağlayın **iPhone** / **iPad**, simülatör değil.
5. Tıklayın **çalıştırma** Mac için Visual Studio'da ve cihaz üzerinde çalışan uygulamayı görüntüleyin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> Başlamadan önce seçtiğinizden emin olun **el ile sağlama** içinde **Proje > sağlama özellikleri...** .

1. Mac derleme konağı için bir cihaz takın.
2. Projenin **Info.plist**, paket grubu tanımlayıcısı, uygulama kimliği eşleştiğinden emin olun:

  ![](manual-provisioning-images/servicevs01.png "Bir tanımlayıcı girme")

3. Proje Seçenekleri iletişim kutusunu görüntülemek için proje üzerinde sağ tıklayın ve göz atın **Yapı > iOS paket grubu imzalama**. Her ikisi de yanındaki aşağı açılan listeden **imzalama kimliği** ve **sağlama profili** Visual Studio doğru profilleri bakın ve bir belirli kimlik & profili seçin doğrulayın.

    Bu ayarlanırsa **otomatik**, Visual Studio, kimlik ve profil #2. adımda ayarlanan paket Kimliğini göre seçer.

4. Derleme yapılandırması ayarlayın sağlayın **iPhone** veya **iPad**, simülatör değil.
5. Tıklayın **çalıştırma** Visual Studio'daki ve cihaz üzerinde çalışan uygulamayı görüntüleyin.


-----

## <a name="summary"></a>Özet

Bu kılavuz, Xamarin.iOS için geliştirme ortamı kurulum için gerekli adımları ele. Bu, nasıl bir geliştirici, kendi takım, bir uygulamayı çalıştıran cihazlara ve tek tek uygulama kimliği hakkında bilgi imzalandığı kod uygulamadır incelediniz.

## <a name="related-links"></a>İlgili bağlantılar

- [Ücretsiz Sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Uygulama Dağıtımı](~/ios/deploy-test/app-distribution/index.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
- [Apple - uygulama dağıtım kılavuzu](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
