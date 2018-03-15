---
title: "El ile sağlama"
description: "Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlamaya yöneliktir. Bu kılavuz, geliştirme sertifikalar istemek ve profilleri, uygulama hizmetleri ile çalışma ve cihaza bir uygulama dağıtmak inceleyeceksiniz."
ms.topic: article
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/15/2017
ms.openlocfilehash: e42b9d0b5eb64c17c96b66c9dbae7582551a06a0
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="manual-provisioning"></a>El ile sağlama

_Xamarin.iOS başarıyla yüklendikten sonra sonraki adımda iOS geliştirme iOS Cihazınızı sağlamaya yöneliktir. Bu kılavuz, geliştirme sertifikalar istemek ve profilleri, uygulama hizmetleri ile çalışma ve cihaza bir uygulama dağıtmak inceleyeceksiniz._

<a name="signingidentity" />

## <a name="creating-a-signing-identity"></a>İmzalama kimlik oluşturma

Geliştirme cihazını ayarlamaya ilk adımı, imzalama kimliğinize oluşturmaktır. İmzalama kimlik ikisinden oluşur:

- Geliştirme sertifikası
- Bir özel anahtar

Geliştirme sertifikası ve ilişkili [anahtarları](#keypairs) için iOS Geliştirici önemlidir: Apple kimliğinizi oluşturmak ve belirli bir aygıt ve dijital imza koyma benzer geliştirme profili ile ilişkilendirin uygulamalarınızı. Apple dağıtmak için izin verilen cihazlara erişimi denetlemek sertifikalar için denetler.

Geliştirme ekiplerinin, sertifikalar ve profilleri erişerek yönetilebilir [tanımlayıcıları & profilleri, sertifikaları](https://developer.apple.com/account/overview.action) Apple'nın üyeleri Merkezi bölümü. Apple aygıt veya simulator için kodunuzu oluşturmak için imzalama bir kimliğe sahip gerektirir.  

> [!IMPORTANT]
> Herhangi bir anda yalnızca iki iOS geliştirme sertifikaları olabilir dikkate almak önemlidir. Daha fazla oluşturmanız gerekiyorsa, mevcut bir iptal gerekecektir. İptal edilen sertifika kullanarak herhangi bir makine, uygulama oturum mümkün olmaz.

İmzalama kimlik oluşturmak için aşağıdakileri yapın:

1. Oturum açma [Geliştirici Portalı'nın sertifikaları, tanımlayıcılar ve profilleri bölümüne](https://developer.apple.com/account/overview.action) seçip **sertifikaları** konusundaki **iOS uygulamaları** sütun. Ardından, isabet  **+**  yeni bir sertifika oluşturmak için:

    [![](manual-provisioning-images/cert-plus.png "Tıklatın + yeni bir sertifika oluşturmak için")](manual-provisioning-images/cert-plus.png#lightbox)

2. Seçin **iOS uygulaması geliştirme** seçenek için sertifika türü ve'ı tıklatın **devam**. Bu ekran, hesap ayrıcalığı bağlı olarak farklı görünebilir:

    [![](manual-provisioning-images/cert-first.png "İOS sertifika türü için uygulama geliştirme seçeneği seçin")](manual-provisioning-images/cert-first.png#lightbox)

3. Bir sertifika imzalama olan bir sertifikayı el ile oluşturmak için karşıya isteği, istek. Bunu yapmak için Başlat **Anahtarlık erişimi** bir Mac bilgisayar üzerinde Ana menüye gidin ve **sertifika Yardımcısı** ve **bir sertifika yetkilisinden sertifika iste...** aşağıda gösterildiği gibi:

      [![](manual-provisioning-images/key-first.png "Bir sertifika imzalama isteği")](manual-provisioning-images/key-first.png#lightbox)

4. Bilgilerinizi doldurun ve seçeneğini **diske Kaydet**:

    [![](manual-provisioning-images/key-second.png "Bilgilerinizi doldurun")](manual-provisioning-images/key-second.png#lightbox)

5. CSR bunu kolayca bulunabileceği bir konuma kaydedin:

    [![](manual-provisioning-images/cert-third.png "CSR Kaydet")](manual-provisioning-images/cert-third.png#lightbox)

6. Sağlama portalına geri dönün, sertifika portala yükleyin ve gönder:

    [![](manual-provisioning-images/cert-second.png "Sertifika portala yükleyin")](manual-provisioning-images/cert-second.png#lightbox)

    Yönetici ayrıcalıklarına sahip değil, Sertifika Yöneticisi veya ekibi aracısı tarafından onaylanması gerekir.

7. Sertifika onaylandıktan sonra sağlama Portalı'ndan yükleyin:

    [![](manual-provisioning-images/status-dev.png "Sertifikayı sağlama Portalı'ndan indirin")](manual-provisioning-images/status-dev.png#lightbox)

8. Anahtarlık erişimi başlatın ve açmak için indirilen sertifikayı çift **My sertifikaları** paneli, yeni sertifikaları ve ilişkili özel anahtarı gösterirken:

    [![](manual-provisioning-images/keychain.png "Anahtarlık erişimi sertifikada")](manual-provisioning-images/keychain.png#lightbox)

<a name="keypairs" />

### <a name="understanding-certificate-key-pairs"></a>Anlama sertifikası anahtar çiftleri

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Geliştirici profili sertifikalar, bunların ilişkili anahtarları ve hesapla ilişkili tüm sağlama profilleri içerir. Bir geliştirici profili gerçekte iki sürümü vardır: Geliştirici portalında biridir ve diğer yaşadığı yerel Mac'te İkisi arasındaki farkı içerdikleri anahtarları türüdür: _yerel Mac kopyasında tüm özel anahtarları içerirken sertifikalarınızı ile ilişkili tüm ortak anahtarları Portal profilinde barındırıldığı_. Geçerli olması sertifikaları için anahtar çiftleri eşleşmesi gerekir. Özel anahtarlar kaybolursa, tüm sertifikaları ve sağlama profilleri yeniden oluşturulması gerekir çünkü bir yedekleme Geliştirici profilinin yerel Mac tutun.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Geliştirici profili sertifikalar, bunların ilişkili anahtarları ve hesapla ilişkili tüm sağlama profilleri içerir. Bir geliştirici profili gerçekte iki sürümü vardır: Geliştirici portalında biridir ve diğer yaşadığı Mac'te İkisi arasındaki farkı içerdikleri anahtarları türüdür: _tüm ortak anahtarlar, kopyada Mac iken sertifikalarınızı ile ilişkili Portal evleri profilindeki tüm özel anahtarları içeren_. Geçerli olması sertifikaları için anahtar çiftleri eşleşmesi gerekir. Özel anahtarlar kaybolursa, tüm sertifikaları ve sağlama profilleri yeniden oluşturulması gerekir çünkü Geliştirici profili yedeğini Xamarin derleme ana bilgisayarın Mac, takip.

-----

> [!WARNING]
> **Not:** sertifikayı ve ilişkili anahtarları kaybetme olabilir son derece kesintiye uğratan varolan sertifikaların iptal gerektirecek ve ilişkili tüm cihazları yeniden sağlama dahil olmak üzere geçici dağıtım için kayıtlı. Başarılı bir şekilde geliştirme sertifikaları ayarlayarak sonra bir yedek kopyasının dışarı aktarın ve bunları güvenli bir yerde saklayın. Bunun nasıl yapılacağı hakkında daha fazla bilgi için aktarma ve içeri aktarma sertifikaları ve profilleri bölümüne bakın [Bakımı sertifikaları](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html) Apple'nın belgeleri kılavuzda.

<a name="provisioning" />

## <a name="provisioning-an-ios-device-for-development"></a>Bir iOS geliştirme için cihazı sağlama

Apple kimliğinizi belirlediğinize göre ve geliştirme sertifikaya sahip olduğunuza Apple cihazına bir uygulamayı dağıtmak mümkün olması için bir sağlama profili ve gerekli varlıkları ayarlamanız gerekir. Aygıt Xcode tarafından desteklenen iOS sürümünü çalıştırması gerekir — cihaz, Xcode veya her ikisi de güncelleştirmeniz gerekebilir.

<a name="adddevice" />

## <a name="add-a-device"></a>Bir cihaz ekleme

Geliştirme için bir sağlama profili oluştururken, biz hangi cihazların uygulama çalıştırabilirsiniz belirtmelidir. Bunu etkinleştirmek için Takvim yılı başına 100 aygıtlara yukarı Geliştirici portalımıza eklenebilir ve buradan biz belirli bir sağlama profili eklenecek cihazları seçebilirsiniz. Mac Geliştirici portalına bir cihaz eklemek için aşağıdaki adımları izleyin

1. Xcode başlatın.
2. Sağlanan, USB kablosu ile Mac sağlanacak aygıtı bağlayın.
2. Gelen **Windows** menüsünü seçin **aygıtları**:

  [![](manual-provisioning-images/add01.png "Cihazları Windows menüsünden seçin")](manual-provisioning-images/add01.png#lightbox)

3. İstenen iOS CİHAZDAN Seç **AYGITLARI** aygıtları penceresinin sol tarafındaki listesi.
4. Vurgula **tanımlayıcısı** dize ve Pano'ya kopyalayın:

  [![](manual-provisioning-images/add02.png "Kimlik dizesi vurgulayın")](manual-provisioning-images/add02.png#lightbox)

5. Safari ile gidin [Apple Geliştirici Merkezi](https://developer.apple.com/membercenter/index.action) ve oturum açın.
6. Tıklatın **tanımlayıcıları & profilleri, sertifikaları** bağlantı:

  [![](manual-provisioning-images/add03.png "Sertifikalar'ı tıklatın, tanımlayıcılar profilleri bağlayın")](manual-provisioning-images/add03.png#lightbox)

7. Tıklayın **aygıtları** bağlantı:

  [![](manual-provisioning-images/add04.png "Aygıtları bağlantıya tıklayın")](manual-provisioning-images/add04.png#lightbox)

8. Tıklatın  **+**  düğmesi:

  [![](manual-provisioning-images/add05.png "Tıklatın + düğmesi")](manual-provisioning-images/add05.png#lightbox)

9. Yeni cihaz için bir ad ve cihaz yapıştırma **tanımlayıcısı** biz içine kopyaladığınız yukarıda **UUID** alan:

  [![](manual-provisioning-images/add06.png "Yeni cihaz ve cihaz tanımlayıcısı için bir ad sağlayın")](manual-provisioning-images/add06.png#lightbox)

10. Tıklatın **devam** düğmesi.
11. Son olarak, bilgileri gözden geçirin ve tıklatın **kaydetmek** düğmesi:

  [![](manual-provisioning-images/add07.png "Bilgileri gözden geçirin")](manual-provisioning-images/add07.png#lightbox)

Bir Xamarin.iOS uygulaması hata ayıklama veya test için kullanılan iOS cihazlar için yukarıdaki adımları yineleyin.

Cihaz Geliştirici portalına eklendikten sonra bir sağlama profili oluşturun ve cihaz eklemek gereklidir.


<a name="provisioningprofile" />

## <a name="creating-a-development-provisioning-profile"></a>Bir geliştirme sağlama profili oluşturma

Geliştirme sertifikayla sağlama profilleri aracılığıyla el ile oluşturulabilir gibi [tanımlayıcıları & profilleri, sertifikaları](https://developer.apple.com/account/overview.action) Apple'nın üyeleri Merkezi bölümü.

Bir sağlama profili oluşturmadan önce bir *uygulama kimliği* yapılması gerekir. Bir uygulama kimliği uygulamanın benzersiz olarak tanımlayan bir geriye doğru DNS stili dizesidir. Olacak aşağıdaki adımları nasıl oluşturulacağını gösteren bir **joker uygulama kimliği**, yapı ve çoğu uygulamayı yüklemek için kullanılabilir. **Açık uygulama kimlikleri** yalnızca bir uygulama (ile eşleşen paket kimliği) yüklenmesine izin ver ve genellikle Apple Pay ve HealthKit gibi belirli iOS özellikler için kullanılır. Açık uygulama kimlikleri oluşturma hakkında daha fazla bilgi için bkz [özellikleriyle çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu.

### <a name="app-id"></a>Uygulama Kimliği

1. İçinde [Geliştirici Portalı](https://developer.apple.com/account/overview.action) göz *sertifika, tanımlayıcılarını ve profiller* Apple Developer Center'da bölümünde. Seçin **uygulama kimlikleri** altında **tanımlayıcıları**.
2. Tıklatın  **+**  düğmesine tıklayın ve sağlayan bir **adı**:

    [![](manual-provisioning-images/appid05a.png "Bir ad sağlayın")](manual-provisioning-images/appid05a.png#lightbox)
3. Uygulama önek önceden yüklenmiş. Seçin **joker uygulama kimliği** uygulama sonek için. Bir paket kimliği şu biçimde girin `com.[DomainName].*`:

  [![](manual-provisioning-images/appid05b.png "Bir paket kimliği girin")](manual-provisioning-images/appid05b.png#lightbox)

3. Tıklatın **devam** düğmesi ve yeni bir uygulama kimliği oluşturmak için açık ekran yönergeleri izleyerek

### <a name="provisioning-profile"></a>Sağlama profili

Uygulama Kimliği oluşturulduktan sonra sağlama profili üretilebilir. Bu sağlama profili hakkında bilgiler içerir. *ne* uygulama (veya joker uygulama kimliği ise uygulamalar) bu profili teklifiyle *kimin* (bağlı olarak hangi geliştirici sertifikaları eklenir), profil kullanabilirsiniz ve *ne* cihazlar, uygulama yükleyebilir.

El ile geliştirme için bir sağlama profili oluşturmak için şunu yapın:

1. Gözatmak için Safari kullanmak [Apple geliştiriciler üye Merkezi](https://developer.apple.com/membercenter/index.action)ve bölümünün altında *tanımlayıcıları & profilleri, sertifikaları* sağlama profilleri seçin.
2. Tıklatın  **+**  düğmesi, yeni bir profil oluşturmak için sağ üst köşedeki.
3. Gelen **geliştirme** bölümünde, radyo düğmesini işaretleyin **iOS uygulaması geliştirme**ve basın **devam**:

    [![](manual-provisioning-images/provisioning-profile01.png "Oluşturmak için profili türünü seçin")](manual-provisioning-images/provisioning-profile01.png#lightbox)
4. Açılan menüden seçin uygulamayı kullanmak için kimlik:

    [![](manual-provisioning-images/provisioning-profile02.png "Uygulamayı seçin, kullanılacak kimliği")](manual-provisioning-images/provisioning-profile02.png#lightbox)
5. Sağlama profili ve tuşuna dahil sertifikaları seçin **devam**:

    [![](manual-provisioning-images/provisioning-profile03.png "Sağlama profilinde dahil etmek için sertifikaları seçin")](manual-provisioning-images/provisioning-profile03.png#lightbox)
6. Uygulama yüklenmiş tüm aygıtları seçin.

    [![](manual-provisioning-images/provisioning-profile04.png "Uygulama yüklenmiş tüm cihazları seçin")](manual-provisioning-images/provisioning-profile04.png#lightbox)
7. Sağlama profili bir tanımlanabilen bir ad ve tuşuna sağlayın **devam** profili oluşturmak için:

    [![](manual-provisioning-images/provisioning-profile05.png "Sağlama profili bir tanımlanabilen bir ad sağlayın")](manual-provisioning-images/provisioning-profile05.png#lightbox)
8. Tuşuna **karşıdan** Mac üzerine sağlama profili indirmek için:

    [![](manual-provisioning-images/provisioning-profile06.png "Sağlama profili indirin")](manual-provisioning-images/provisioning-profile06.png#lightbox)
9. Xcode'da sağlama profili yüklemek için dosyaya çift tıklayın. Xcode açma dışında profil yüklü tüm visual clues gösterilmeyebilir unutmayın. Bu göz atarak doğrulanabilir **Xcode > Tercihler > hesapları**. Apple Kimliğinizi tıklatıp **ayrıntıları görüntüle...** . Aşağıda gösterildiği gibi yeni sağlama profili listelenmelidir:

      [![](manual-provisioning-images/provisioning-profile07.png "Xcode'da profil görüntüleme")](manual-provisioning-images/provisioning-profile07.png#lightbox)

Sağlama profili başarıyla oluşturulduktan sonra tüm geliştirme sertifikaları Mac ve Visual Studio için Visual Studio için kullanılabilir olacak şekilde, Xcode yenilemek gerekli olabilir.

<a name="download" />

## <a name="downloading-profiles-and-certificates-in-xcode"></a>Profilleri ve sertifikaları xcode'da indirme

Sertifikalar ve Apple Geliştirici Portalı'nda oluşturulan sağlama profilleri otomatik olarak görüntülenmeyebilir Xcode'da. Bu nedenle, bu nedenle karşıdan yüklemek için gerekli olabilir, bunlar Visual Studio tarafından Mac ve Visual Studio için erişilebilir olduğunu. Güncelleştirme ve Apple Geliştirici Portalı'nda oluşturulan herhangi bir sertifika yüklemek için aşağıdakileri yapın:

1.   Mac veya Visual Studio için Visual Studio çıkın.
2.   Xcode başlatın.
3.   Seçin **Xcode menü > tercihleri...**
4.   Tıklatın **hesapları** sekmesi.
5.   Bir takım seçin ve'ı tıklatın **karşıdan el ile profiller** düğmesi: [ ![ ] (manual-provisioning-images/selectteam1.png "el ile yükleme profilleri")](manual-provisioning-images/selectteam1.png#lightbox)

6.   Xcode çıkın.
7.  Mac veya Visual Studio için Visual Studio'yu başlatın.

Yeni sertifikalar veya sağlama profilleri Mac veya Visual Studio için Visual Studio'da kullanılabilir ve kullanıma hazır olacaktır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

> [!IMPORTANT]
> **Not:** durdurmanız ve herhangi bir yeni veya değiştirilmiş sertifikaları veya Xcode tarafından güncelleştirilmiş profilleri görürsünüz önce Mac için Visual Studio yeniden başlatmanız gerekebilir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> **Not:** durdurmanız ve tüm yeni veya değiştirilmiş sertifikaları veya Xcode tarafından güncelleştirilmiş profilleri görürsünüz önce Visual Studio yeniden başlatmanız gerekebilir.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Uygulama hizmetleri için sağlama

Apple özel uygulama hizmetleri, bir Xamarin.iOS uygulaması için etkin hale getirilebilir özellikleri, olarak da bilinir seçimi sağlar. Bu uygulama hizmetleri hem iOS sağlama portalı yapılandırılmalıdır zaman **uygulama kimliği** oluşturulur ve **Entitlements.plist** dosya Xamarin.iOS uygulamanın projesinin bir parçası. Uygulama Hizmetleri uygulamanıza ekleme hakkında daha fazla bilgi için bkz [yetenekleri giriş](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu ve [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

* Bir uygulama kimliği ile gerekli uygulama hizmetleri oluşturun.
* Yeni bir [sağlama profili](#provisioningprofile) , bu uygulama kimliğini içerir.
* Xamarin.iOS projesinde yetkilendirmeler ayarlayın

<a name="deploy" />

## <a name="deploying-to-a-device"></a>Bir cihaza dağıtma

Bu noktada sağlama tamamlanmış olmalıdır ve uygulama cihaza dağıtılmaya hazır olur. Bunu yapmak için aşağıdaki adımları izleyin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

> [!IMPORTANT]
> Başlamadan önce seçtiğinizden emin olun **el ile sağlama** içinde **Info.plist**.

1. Mac için cihaz takılır
2. Projenin **Info.plist**, paket tanımlayıcısı (uygulama kimliği bir joker karakter değilse) uygulama kimliği eşleştiğinden emin olun:

  ![](manual-provisioning-images/deploydevice01xs.png "Tanımlayıcı girme")

3. Proje Seçenekleri iletişim kutusunu görüntülemek ve için taramak için projeye sağ **Yapı > iOS paket imzalama**. Her ikisi de yanında aşağı açılan listeden **imzalama kimlik** ve **sağlama profili**, Mac için Visual Studio doğru profilleri Bkz belirli bir kimlik & profili seçin doğrulayın ve:

  ![](manual-provisioning-images/deploydevice02xs.png "Belirli bir kimlik & profili seçin")

Bu ayarlanırsa **otomatik**, Visual Studio için Mac kimlik ve ayarlandı paket kimliği temel alarak profili seçin. adımda #2.

4. Derleme yapılandırması ayarlamak için emin olun **iPhone** / **iPad**, bunun yerine simulator.
5. Tıklatın **çalıştırmak** Mac için Visual Studio ve cihazda çalışan uygulama görüntüleyin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Mac için cihaz takılır
2. Projenin **Info.plist**, paket tanımlayıcısı uygulama kimliği eşleştiğinden emin olun:

  ![](manual-provisioning-images/servicevs01.png "Tanımlayıcı girme")

3. Proje Seçenekleri iletişim kutusunu görüntülemek için projeye sağ tıklayın ve Gözat **Yapı > iOS paket imzalama**. Her ikisi de yanında aşağı açılan listeden **imzalama kimlik** ve **sağlama profili** Visual Studio doğru profilleri Bkz belirli bir kimlik & profili seçin doğrulayın ve.

    Bu ayarlanırsa **otomatik**, Visual Studio, kimlik ve #2. adımda ayarlanan paket kimliği temel profil seçebilir.

4. Derleme yapılandırması ayarlamak için emin olun **iPhone** veya **iPad**, bunun yerine simulator.
5. Tıklatın **çalıştırmak** Visual Studio ve cihazda çalışan uygulama görüntüleyin.


-----

## <a name="summary"></a>Özet

Bu kılavuz için Xamarin.iOS geliştirme ortamı kurulum için gerekli adımlar ele. Bu, nasıl bir geliştirici, kendi takım, bir uygulama çalıştıran cihazlar ve tek tek uygulama kimliği hakkında bilgi içeren imzalı kod uygulamadır incelediniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Ücretsiz Sağlama](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Uygulama Dağıtımı](~/ios/deploy-test/app-distribution/index.md)
- [Sorun giderme](~/ios/deploy-test/troubleshooting.md)
- [Apple - uygulama dağıtım kılavuzu](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
