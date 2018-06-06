---
title: Xamarin.iOS uygulamaları dağıtmak için TestFlight kullanma
description: TestFlight artık Apple'nın ait ve beta Xamarin.iOS uygulamalarınızı test etmek için birincil yoludur. Bu makalede iTunes Connect ile çalışmaya, uygulamanızı karşıya TestFlight işleminin – tüm adımlarda size yol gösterir.
ms.prod: xamarin
ms.assetid: BA880768-2BC8-41E4-B57E-A56F8EED4690
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: efb0a59ac43ca3e0c4959caa8478a51512e29a3a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785693"
---
# <a name="using-testflight-to-distribute-xamarinios-apps"></a>Xamarin.iOS uygulamaları dağıtmak için TestFlight kullanma

_TestFlight artık Apple'nın ait ve beta Xamarin.iOS uygulamalarınızı test etmek için birincil yoludur. Bu makalede iTunes Connect ile çalışmaya, uygulamanızı karşıya TestFlight işleminin – tüm adımlarda size yol gösterir._

Beta sürümü test yazılım geliştirme döngüsü ayrılmaz bir parçası olduğunu ve bu işlem gibi kolaylaştırmak sunan birçok platformlar arası uygulamalar [HockeyApp](http://hockeyapp.net/features/), [Applause](http://www.applause.com/mobile-app-testing)ve Elbette Google Play'nın yerel uygulama için Android uygulamalarını test etme Beta. Bu belge, Apple'nın TestFlight üzerinde odaklanmıştır.

TestFlight iOS uygulamaları için Apple'nın beta test etme hizmeti ve yalnızca üzerinden erişilebilir [iTunes Bağlan](https://itunesconnect.apple.com/). İOS 8.0 uygulamaları için ve yukarıdaki şu anda kullanılabilir. İç ve dış kullanıcılarla sınama beta TestFlight sağlar ve bir Beta nedeniyle İkincisi, uygulama gözden sağlar son incelemeniz çok daha kolay bir işlemde uygulama mağazasında yayınlarken.


Daha önce ikili Mac için Visual Studio içinde oluşturulan ve TestFlightApp Web sitesine dağıtım Sınayıcılar için yüklenir. Yeni işlemine birkaç yüksek kaliteli, yapmanıza izin uygulama mağazasında iyi sınanmış uygulamaları geliştirme vardır. Örneğin:


- Her ikisi de bağlılığı Apple'nın yönergeleri için gereksinim duyduğunuz dış test etmek için gerekli Beta uygulama gözden geçirme, son App Store gözden geçirmek için daha yüksek bir başarı olasılığı sağlar.
- Karşıya yüklemeden önce uygulamayı iTunes Connect ile kayıtlı olması gerekir. Bu, sağlama profilleri, adları ve sertifikaları arasında uyuşmazlık olacaktır sağlar.
- Daha hızlı çalışır şekilde TestFlight uygulama gerçek iOS uygulaması sunulmuştur.
- Beta sürümü test tamamlandıktan sonra gözden geçirmek için uygulama taşımak için hızlı ve verimli bir işlemdir; tek düğmesini tıklatın.

## <a name="requirements"></a>Gereksinimler

Yalnızca iOS 8.0 uygulamalar veya yukarıdaki TestFlight test edilebilir.

Tüm sınayıcılar uygulama, en az sınamalısınız iOS 8 cihazı. Ancak, en iyi uygulama, uygulamanız tüm iOS sürümlerinde test olduğunu belirler.

## <a name="provisioning"></a>Sağlama

Derlemeleriniz TestFlight ile test etmek için oluşturmanız gerekecektir bir *App Store dağıtım profili* yeni beta yetkilendirme ile. Bu yetkilendirme beta TestFlight üzerinden ve diğer sınanmasını sağlar **yeni** App Store dağıtım profili otomatik olarak bu yetkilendirme içerir. Adım adım yönergeleri izleyin [dağıtım profili oluşturma](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) yeni bir profil oluşturmak için kılavuz.

Dağıtım profilinizi beta yetkilendirme içerdiğini onaylamak zaman [yapınızın xcode'da doğrulama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)aşağıda gösterildiği gibi:

[![](testflight-images/validate-build.png "Apple App gönderiliyor")](testflight-images/validate-build.png#lightbox)


## <a name="testflight-workflow"></a>TestFlight iş akışı

Aşağıdaki iş akışı Beta uygulamanızı test etmek için TestFlight kullanmaya başlamak için gereken adımlar açıklanmıştır:

1. Yeni uygulamalar için oluşturma bir [iTunes Connect kaydı](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) .
2. [Yayımla ve arşivle](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) iTunes Bağlan uygulamanıza.
3. Beta sürümü test yönetin:
    - Meta veri ekleyin.
    - İç kullanıcılar ekleyin:
        - En fazla 25 kullanıcılar.
    - Dış kullanıcılar ekleyin:
        - En fazla 1000 kullanıcı.
        - Apple'nın yönergeleri ile uyumluluğu gerektiren bir beta test gözden geçirme gerektirir.
4. Kullanıcılardan geri bildirim almak, onun üzerinde işlem ve 2. adıma dönün.

## <a name="create-an-itunes-connect-record"></a>Bir iTunes Connect kaydı oluşturun

1.  Oturum açma [iTunes Bağlan Portal](https://itunesconnect.apple.com/) Apple Geliştirici kimlik bilgilerinizi kullanarak.
2.  Seçin **uygulamalarım**:

    [![](testflight-images/my-apps.png "Uygulamalarım seçin")](testflight-images/my-apps.png#lightbox)


3.  Üzerinde **My uygulamaları** ekranında, tıklayın **+** yeni bir uygulama eklemek için ekranın sol üst köşesindeki düğmesi. Mac ve iOS Geliştirici hesabınız varsa, burada yeni uygulama türü seçmek için istenir.

İle sunulur **yeni iOS uygulaması** uygulamanızın Info.plist tam olarak aynı bilgileri içermesi gerekir gönderme penceresi

Yeni bir iTunes Connect kaydı oluşturma hakkında daha fazla bilgi için başvurmak [bir iTunes Bağlan kayıt oluşturma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) Kılavuzu.



###  <a name="completing-the-new-ios-app-submission-form"></a>Yeni iOS uygulaması gönderme formu Tamamlanıyor

Form, aşağıda gösterildiği gibi tam olarak, uygulamanızın Info.plist dosyasındaki bilgileri yansıtması gerekir:

[![](testflight-images/infoplist.png "Uygulamanın Info.plist")](testflight-images/infoplist.png#lightbox)
[![](testflight-images/newiosapp.png "form üzerinde iTunes Bağlan")](testflight-images/newiosapp.png#lightbox)

-  **Ad** — uygulama paketi ayarlarken kullanılan tanımlayıcı adı. Bu bir tam eşleşme olmalıdır **uygulama adı** girişi, `Info.plist`.
-  **Birincil dili** — uygulama içinde kullanılan temel dili. Bu genellikle konuşurken herhangi bir dil olur.
-  **Paket kimliği** — tüm uygulama kimlikleri listeleyen menü açılan Geliştirici hesabınızda oluşturuldu.
    *   **Kimliği soneki paketini** — joker paket kimliği seçtiyseniz (yani ile biten bir *, örneğimizde gibi yukarıdaki), ek bir kutu, paket kimliği soneki sorulma görünür. Örnekte **paket kimliği** olan `mobi.chkn.*`, sonek **sayfa görünümü**. Bunlar birlikte oluşturan **paket tanımlayıcı** içinde bizim `Info.plist`.
-  **Sürüm** — karşıya yüklenen uygulamanın sürüm numarası. Bu, geliştirici tarafından seçilir.
-  **SKU** — SKU olan kullanıcılar tarafından görülen değil, uygulamanız için benzersiz bir kimliği. Bu, benzer şekilde bir ürün kimliği düşünülebilir Yukarıdaki örnekte t bir sürüm numarası ile birlikte tarihi, tarih için seçtiniz.


## <a name="upload-your-app"></a>Uygulamanızı karşıya yükle

Connect kaydı iTunes oluşturulduktan sonra yeni derlemeler yükleyebildiğini olacaktır. Derlemeleri yeni beta yetkilendirme olması gerektiğini unutmayın.

İlk olarak, yapı, [son dağıtılabilir](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) IDE'de, ardından [uygulamanızı apple'a gönderme](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) uygulama yükleyicisi ya da Xcode arşiv işlevinde üzerinden.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

###  <a name="create-an-archive"></a>Bir arşiv oluşturma

 Mac için Visual Studio'da bir ikili oluşturmak için kullanmanız gerekecektir _arşiv_ işlevi. Projeye sağ tıklayın ve seçin **arşiv yayımlama için**aşağıda gösterildiği gibi:

 [![](testflight-images/new-archive.png "Yayımlama Arşivi'ni seçin")](testflight-images/new-archive.png#lightbox)


 Başvurmak [Distributable oluşturma](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) daha fazla bilgi için Kılavuzu.

###  <a name="sign-and-distribute-your-app"></a>Oturum ve uygulamanızı dağıtın

 Bir arşiv oluşturma otomatik olarak açılır **arşivler Görünüm**, çözümü tarafından gruplandırılmış tüm arşivlenmiş projeleri görüntüleme. Uygulamanızı imzalamak ve dağıtım için hazırlamak üzere seçin **oturum ve Dağıt...** , aşağıda gösterilen:

[![](testflight-images/archive-view.png "Bir arşiv oluşturma arşivler Görünüm otomatik olarak açılır")](testflight-images/archive-view.png#lightbox)

 Bu, Yayımlama Sihirbazı'nı açar. Seçin **App Store** dağıtım kanal paket oluşturmak ve uygulama Yükleyicisi'ni açın. Sağlama profili ekranında, imzalama kimliğinize ve sağlama profili seçin veya başka bir kimlikle yeniden oturum açın. Paketinizi ayrıntılarını doğrulayın ve tıklatın **Yayımla** kaydetmek için `.ipa`

[![](testflight-images/group.png "Kimlik imzalama ve sağlama profili seçin veya başka bir kimlikle yeniden imzalama")](testflight-images/group.png#lightbox)

 Başvurmak [uygulamanızı apple'a gönderme](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) adımları hakkında daha fazla bilgi için bölüm.

###  <a name="submitting-your-build"></a>Yapınızın gönderiliyor
 Yayımlama Sihirbazı'nı yapınızın iTunes Bağlan karşıya yüklemek için tüm uygulama yükleyici programa açılır. Seçin **uygulamanızı teslim** seçeneği ve karşıya `.ipa` yukarıda oluşturduğunuz dosya. Uygulama Yükleyicisi doğrulamak ve yapınızın iTunes Bağlan yükleyin.

 Başvurmak [uygulamanızı apple'a gönderme](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) adımları hakkında daha fazla bilgi için bölüm.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

###  <a name="building-your-final-distributable"></a>Dağıtılabilir, son oluşturma
 Visual Studio için Xamarin eklentisi, uygulama mağazası yayımlama arşivleme Xamarin.iOS uygulamaları desteklemediğinden, Visual Studio'dan bir iOS uygulaması yayımlamak için iki seçenek vardır. Bunlar:

1. Yapı geçici IPA komutu oluşturulan bir IPA karşıya yükleyin.
1. Daraltılmış bir karşıya yükleme `.app` paket.

 [Distributable oluşturma](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) Kılavuzu yönergeler için bu seçeneklerin ikisi de vardır.

###  <a name="submitting-your-build"></a>Yapınızın gönderiliyor
 Apple uygulamanıza göndermek için yapı konağı taşıyın ve Xcode bir parçası olarak yüklenen uygulama yükleyicisi programını gerekecektir. Apple için uygulama yükleyicisi erişme hakkında daha fazla bilgi için bkz: [erişim uygulama yükleyicisi](http://help.apple.com/itc/apploader/#/apdATD1E927-D1E1A1303-D1E927A1126) Kılavuzu.

Açılan sonra seçeneğini **uygulamanızı teslim** seçeneği ve zip karşıya yükleme veya `.ipa` yukarıda oluşturduğunuz dosya. Uygulama Yükleyicisi doğrulamak ve yapınızın iTunes Bağlan yükleyin.

 Başvurmak [uygulamanızı apple'a gönderme](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) adımları hakkında daha fazla bilgi için bölüm.

-----


[Uygulama mağazasında yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) kılavuzda daha ayrıntılı yukarıdaki adımların tümünü açıklanmaktadır, bu daha derinlemesine bir bakış için uygulama mağazası gönderme işlemine bakın.

Dönme üzerine **My uygulamaları** bölüm iTunes Bağlan uygulamanız başarıyla yüklendiğini bulmanız gerekir. Bu noktada, şimdi bazı Beta test yapmak hazırsınız!

## <a name="manage-beta-testing"></a>Beta sürümü test yönetme

### <a name="add-metadata"></a>Meta verileri ekleme

TestFlight'ı kullanmaya başlamak için Gözat **yayın öncesi** , uygulamanızın sekmesi. Yapılar, iç Sınayıcılar ve dış sınayıcılar listesini gösteren üç sekme aşağıda gösterildiği gibi görmeniz gerekir:

[![](testflight-images/app-uploaded.png "Yapılar, iç Sınayıcılar ve dış sınayıcılar sekmeleri")](testflight-images/app-uploaded.png#lightbox)

Meta veri uygulamanıza eklemek için yapı numarası TestFlight sonra'ni tıklatın:

[![](testflight-images/metadata.png "Meta verileri ekleme")](testflight-images/metadata.png#lightbox)

Altında **Test bilgileri**, örneği için uygulamanız ile ilgili önemli bilgiler ile sınayıcılar sağlayabilir:

- Test gerekenler
- Uygulamanızı açıklaması.
- Pazarlama URL — bu eklemekte olduğunuz uygulama hakkında bilgi verir.
- Gizlilik İlkesi URL'si — Bir URL, şirketinizin gizlilik ilkesi bilgileri verir.
- Geri bildirim e-posta.

Unutmayın bu meta veriler **değil** iç Sınayıcılar için gerekli ancak **olan** dış Sınayıcılar için gereklidir.

<a name="beta-testing" />

### <a name="enable-beta-testing"></a>Beta sürümü test etkinleştir

Uygulamanızı test etme başlatmaya hazır olduğunuzda Aç **TestFlight Beta test** geçiş sürümünüz için:

[![](testflight-images/turn-on-testing.png "TestFlight Beta test anahtarda Aç")](testflight-images/turn-on-testing.png#lightbox)

Her için etkin bir yapıdır **60 gün** TestFlight Beta anahtarı açık tarihinden itibaren. Her bir derlemesi için kaç gün sol görebilirsiniz **Test bilgileri** sayfa:

[![](testflight-images/daysleft.png "Test bilgileri sayfası")](testflight-images/daysleft.png#lightbox)

Sınama herhangi bir zamanda kapatılabilir.

### <a name="internal-testers"></a>İç sınayıcılar

İç sınayıcılar iTunes Bağlan aşağıdaki rollerden biri atanmış olan Geliştirme ekibinizin üyeleri şunlardır:

-  **Yönetici** – iTunes Bağlan içinde yeni kullanıcıları ekleme ve yönetme için bir yönetici sorumludur.
-  **Yasal** – takım aracısıdır yasal rolü atanmış yalnızca yönetici kullanıcı. Bu yasal sözleşmeleri imzalamak sağlar.
-  **Teknik** – teknik bir kullanıcı bir uygulama ile ilgili çoğu özelliklerini değiştirebilirsiniz. Örneğin, uygulama bilgilerini düzenlemek, ikili dosya karşıya yükleme ve bir uygulama gözden geçirme için Gönder.

Her yapı en fazla 25 üyeleri ile paylaşılabilir.

Sınayıcılar eklemek için Gözat **kullanıcılar ve roller** ana iTunes bağlanma ekranında:

[![](testflight-images/users-and-roles.png "Kullanıcılar ve roller ana iTunes bağlanma ekranında")](testflight-images/users-and-roles.png#lightbox)

Varolan iTunes Bağlan kullanıcılar listesinde görünür. Bunları seçmek için bunların adını tıklatın, Aç **iç Tester** geçin ve tıklatın **kaydetmek**:

[![](testflight-images/internal-tester.png "İç Tester anahtarda Aç")](testflight-images/internal-tester.png#lightbox)

Listede olmayan bir kullanıcı eklemek için seçin **+** düğmesine *kullanıcılar*ve bir hesap oluşturmak için ilk adını, soyadını ve e-posta adresi sağlayın. Kullanıcı hesabını etkinleştirmek için e-postalarına onaylamanız gerekir:

[![](testflight-images/add-new-user.png "Kullanıcı ekleme")](testflight-images/add-new-user.png#lightbox)

İçin dönerseniz **My uygulamaları > yayın öncesi > iç sınayıcılar**, beta TestFlight iç test için eklenen kullanıcıları artık görürsünüz:

[![](testflight-images/select-users.png "Beta TestFlight iç test için eklenen kullanıcıların listesi")](testflight-images/select-users.png#lightbox)

Kişinin adını seçerek ve tıklatarak bu sınayıcılar davet edebilirsiniz **davet** düğmesi. Bunlar uygulamanızı test etmek için bir davet içeren bir e-posta alırsınız.

Kullanıcıların davet iç sınayıcılar sayfasının Durum sütununda durumunu görebilirsiniz:

[![](testflight-images/status-added.png "Davet durumu")](testflight-images/status-added.png#lightbox)


### <a name="external-testers"></a>Dış sınayıcılar

Beta uygulamanızı test etmek için harici sınayıcılar davet önce onu Beta gözden uygulama gitmesi gereken ve, bu nedenle, uygun gerekiyor [App Store gözden geçirme yönergeleri](https://developer.apple.com/app-store/review/guidelines/).

Gözden geçirme için uygulamanızı göndermek için tıklatın **göndermek için Beta uygulama gözden** metnin yanında yapınızın, aşağıdaki resimde gösterildiği gibi:

[![](testflight-images/beta-app-review.png "Beta uygulama İnceleme için Gönder")](testflight-images/beta-app-review.png#lightbox)

Uygulamanızı gözden geçirmek, tüm gerekli meta veriler TestFlight Beta Bilgileri sayfasında girmeniz gerekir.

Şimdi davetleri hazırlamak ve en fazla 2000 dış sınayıcılar dış sınayıcılar sekmesi aracılığıyla kendi e-posta, ad ve Soyadı, aşağıdaki ekran görüntüsünde gösterildiği gibi girerek eklemek başlatabilirsiniz. Girdiğiniz e-posta kendi Apple kimliği olmak zorunda değildir; Bu, yalnızca üzerinde daveti alacak e-postadır.

[![](testflight-images/add-external.png "Sınayıcılar davet et")](testflight-images/add-external.png#lightbox)

Çok sayıda dış sınayıcılar varsa, kullanabileceğiniz **Dosya Al** almak için bağlantı bir `CSV` dosya her satır aşağıdaki biçimde:

``` 
first name, last name, email address
```

Dış sınayıcılar düzenlenmiş, sınayıcılar tutmaya yardımcı olmak için farklı gruplar için de ekleyebilirsiniz.

Dış sınayıcılar ayrıntılarını girdikten sonra tıklatın **Ekle** ve bunları davet etmek için onay kullanıcıların sahip onaylayın:

[![](testflight-images/confirm-consent.png "Bunları davet etmek için onay kullanıcınız onaylayın")](testflight-images/confirm-consent.png#lightbox)

Yalnızca bir başarılı Beta uygulama gözden geçirdikten sonra dış Test edenlere Davetleri Gönder mümkün olacaktır. Bu noktada, altındaki metin **dış** sayfa değiştirir üzerinde yapı **Gönder başvurulmasını**. Tüm Test edenlere önceden eklediğiniz davetiye göndermek için burayı tıklatın.

Uygulamanızı reddedilirse gösterilen sorunları giderin gerekecek **çözümleme Merkezi**ve gözden geçirme için tüm güncelleştirilmiş ikili yeniden gönderin.

## <a name="as-a-beta-tester"></a>Beta Tester

Tester davet sonra bunlar aşağıdaki ekran görüntüsünde benzer bir e-postası alır:

[![](testflight-images/tester-email.png "Bir örnek davet e-posta")](testflight-images/tester-email.png#lightbox)

Bunlar tıkladığınızda **TestFlight içinde açmak** uygulamanızı TestFlight uygulamada açılır veya önceden indirilmiş taşınmadığından, uygulama mağazası doğrudan ve bunları indirme izin düğmesi.

Bir kez, uygulama açıldığında TestFlight içinde ne sınamak Ayrıntılar gösterilir ve uygulama, iOS 8.0 (veya üstü) aygıtınız yüklenecek tester ister:

[![](testflight-images/install-app.png "TestFlight ne sınamak Ayrıntılar gösterilir")](testflight-images/install-app.png#lightbox)

Test derlemeleri uygulama adı önceki bir turuncu noktayla aygıtın giriş ekranında gösterilir.

Sınayıcılar TestFlight uygulama üzerinden geri bildirimde bulunun ve bu bilgileri meta verilerde sağlanan e-posta adresinde hafifletmek.

### <a name="beta-testing-complete"></a>Beta testi Tamamla

Beta sürümü test tamamlandıktan sonra uygulamanızı Apple App Store gözden geçirme için şimdi gönderebilirsiniz. Bu işlem çok straightforwardly iTunes Bağlan tıklayarak yapılır **gözden geçirme için gönderme** düğmesi, aşağıda gösterildiği gibi:

[![](testflight-images/submit-for-review.png "Gözden geçirme düğmesi için Gönder'i tıklatın")](testflight-images/submit-for-review.png#lightbox)

## <a name="summary"></a>Özet

Bu makalede, Apple'nın TestFlight Beta test iTunes Bağlan aracılığıyla kullanmak nasıl arama. Yeni bir yapı iTunes Bağlan karşıya nasıl yükleneceğini ve uygulamamıza kullanmak için iç ve dış Beta Test edenlere davet nasıl ele.

## <a name="related-links"></a>İlgili bağlantılar

- [Bir iTunes Connect kaydı oluşturma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [App Store’da Yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Uygulama mağazası dağıtım için bir uygulama sağlama](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#provisioning)
- [Apple TestFlight Beta kullanma](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/BetaTestingTheApp.html#//apple_ref/doc/uid/TP40011225-CH35-SW2)
