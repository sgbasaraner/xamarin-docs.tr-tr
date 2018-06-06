---
title: Xamarin.iOS uygulamaları için uygulama mağazası yayımlama
description: Bu belge, yapılandırma, yapı ve dağıtım için bir Xamarin.iOS uygulaması App Store'da yayımlama açıklar.
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: b8bea29e71e055621e7d0b85d3736ec6cc9ba3b4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785706"
---
# <a name="publishing-xamarinios-apps-to-the-app-store"></a>Xamarin.iOS uygulamaları için uygulama mağazası yayımlama

Tüm iOS cihazları için uygulamaları sırayla dağıtmak, Apple gerektirir üzerinden yayımlanan uygulamalar *App Store*, uygulama mağazası iOS uygulamaları için tek adrestir alışveriş konum yapma. Deposundaki 500. 000'den uygulamalarıyla, bu tek bir dağıtım noktasını yoğun başarı birçok türdeki uygulamayı geliştiricileri büyük harfe. Uygulama geliştiriciler dağıtım ve ödeme sistemleri sunumu hazır bir çözüm uygulama Mağazası ' dir.

Uygulamanın uygulama mağazası gönderme işleminin içerir:

1. Oluşturma bir **uygulama kimliği** ve seçerek **yetkilendirmeler**.
2. Oluşturma bir **sağlama profili dağıtım**.
3. Uygulamanızı yapılandırmak için bu profili kullanarak.
4. Uygulamanız aracılığıyla gönderme **iTunes Bağlan**.


Bu makalede sağlamak, geliştirmek ve uygulama mağazası dağıtım için uygulama göndermek için gereken tüm adımları yer almaktadır.

## <a name="before-you-submit-an-application"></a>Bir uygulama göndermeden önce

Bir uygulama için uygulama mağazası yayına gönderdikten sonra kalite ve içerik için Apple'nın yönergeleri karşıladığını güvence altına almaya Apple tarafından gözden geçirme sürecinde gider. Uygulamanız bu kılavuzları karşılamak başarısız olursa, Apple, Apple tarafından bildirdi uyumsuzluk adres ve daha sonra yeniden gönderin gerekir aynı zamanda reddeder.
Bu nedenle, Apple aracılığıyla kendiniz bu yönergelere hakkında bilgi edinme ve bunları uygulamanıza uyum çalışılırken gözden kolaylaştırarak en iyi ihtimalini bildirimde. Apple yönergeler şurada bulunabilir: [App Store gözden geçirme yönergeleri](https://developer.apple.com/appstore/resources/approval/guidelines.html).

Birkaç uygulama gönderirken dikkat edilmesi gerekenler şunlardır:

1. Uygulamanın açıklaması uygulamada işlevselliğin eşleştiğinden emin olun.
2. Uygulama kilitlenme değil test normal kullanım altında. Bu kullanım desteklediğiniz her iOS cihazında içerir.


Ayrıca, Apple App Store gönderimi ipuçları listesini tutar. Bunlara okuyabilirsiniz [App Store'da dağıtma](https://developer.apple.com/appstore/resources/submission/tips.html).

## <a name="configuring-your-application-in-itunes-connect"></a>İTunes Bağlan uygulamanızı yapılandırma

[iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) başka şeylerin App Store'dan iOS uygulamalarınızı yönetmek için web tabanlı araçları paketidir. Xamarin.iOS uygulamanızı düzgün Kurulum olması gerekir ve Apple için gözden geçirilmesi için gönderilebilir ve sonuç olarak, satış veya uygulama mağazasında bulunan ücretsiz bir uygulama olarak serbest bırakılacak önce iTunes Bağlan yapılandırılır.

Aşağıdakileri yapın:

1. Uygun anlaşmaları yerinde ve içinde güncel olduğundan emin olun **anlaşmaları, vergi ve bankacılık** iTunes satış veya ücretsiz bir iOS uygulaması yayımlamayı Connect Bölümü.
2. Yeni bir **iTunes Connect kaydı** uygulama için ve belirtin, **görünen adı** (uygulama Mağazası'nda görüldüğü gibi).
3. Seçin bir **satış fiyatı** veya uygulama ücretsiz yayımlanması belirtin.
5. Bir düz sağlamak kısa **açıklama** uygulamasının özelliklerini de dahil olmak üzere ve son kullanıcıya yararlanabilirsiniz.
6. Sağlamak **kategorileri**, **alt kategorileri**, ve **anahtar sözcükleri** kullanıcı uygulamanızı uygulama Mağazası'nda Bul yardımcı olacak.
7. Sağlamak **kişi** ve **Destek** Apple tarafından gerekli Web sitesine URL'lerini.
8. Uygulamanızın ayarlamak **derecelendirme**, ebeveyn denetimleri uygulama Mağazası'tarafından kullanılıyor.
9. İsteğe bağlı uygulama mağazası teknolojileri gibi yapılandırmadan **Game Center** ve **uygulama içi satın alma**.

Daha fazla ayrıntı için lütfen bkz. bizim [uygulama iTunes Connect yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) belgeleri.

## <a name="preparing-for-app-store-distribution"></a>Uygulama mağazası dağıtım için hazırlama

Uygulama mağazası uygulama yayımlamak için ilk birçok adımdan dağıtım için oluşturmanız gerekir. Aşağıdaki bölümlerde oluşturulabilir ve uygulama mağazası gözden geçirme ve sürüm göndermek için bir Xamarin.iOS uygulaması yayını için hazırlamak üzere gerekli her şeyi içerir.

### <a name="provisioning-for-application-services"></a>Uygulama hizmetleri için sağlama

Apple özel uygulama hizmetleri için benzersiz bir kimlik oluşturduğunuzda, iOS uygulamanız için etkin hale getirilebilir yetkilendirmeler olarak da adlandırılan, seçimi sağlar. Veya özel yetkilendirmeler kullanıp kullanmadığınızı hala App Store'da yayımlanabilmesi Xamarin.iOS uygulamanızı için benzersiz bir kimliği oluşturmanız gerekir.

Bir uygulama kimliği oluşturma ve isteğe bağlı olarak yetkilendirmeler seçerek Apple'nın web tabanlı iOS sağlama portalı kullanarak aşağıdaki adımları içerir:

1. İçinde **tanımlayıcıları & profilleri, sertifikaları** bölümünde seçin **tanımlayıcıları** > **uygulama kimliği**.
2. Tıklatın **+** düğmesine tıklayın ve sağlayan bir **adı** ve **paket kimliği** yeni uygulama için.
3. Ekranın alt kısmına kaydırın ve seçin **uygulama hizmetleri** , gerekir Xamarin.iOS uygulamanız tarafından.
4. Tıklatın **devam** düğmesi ve aşağıdaki ekran yeni bir uygulama kimliği oluşturma yönergeleri

Seçme ve uygulama Kimliğiniz tanımlarken, gerekli uygulama hizmetlerini yapılandırma ek olarak, ayrıca uygulama kimliği ve yetkilendirmeler Xamarin.iOS projenizde her ikisi de düzenleyerek yapılandırmanız gereken `Info.plist` ve `Entitlements.plist` dosyaları.

Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Info.plist` dosyayı düzenlemek için açın.
2. İçinde **iOS uygulaması hedefi** bölümünde, uygulamanız için bir ad girin ve girin **paket tanımlayıcı** bir uygulama kimliği tanımlandığında, oluşturduğunuz
3. Değişiklikleri kaydetmek `Info.plist` dosya.
4. İçinde **Çözüm Gezgini**, çift `Entitlements.plist` dosyayı düzenlemek için açın.
5. Seçin ve bunlar üzerinde gerçekleştirilen bir uygulama kimliği tanımlandığında Kurulum eşleşmesi sizin için Xamarin.iOS uygulaması gereken yetkilendirmeler yapılandırın
6. Değişiklikleri kaydetmek `Entitlements.plist` dosya.

Ayrıntılı yönergeler için lütfen bkz bizim [için uygulama hizmetleri sağlama](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) belgeleri.

### <a name="setting-the-store-icons"></a>Mağaza simgeler ayarlama

Uygulama mağazasına simgeleri şimdi bir varlık Kataloğu tarafından teslim. Uygulama mağazası simge eklemek için önce bulun **AppIcon** görüntü kümesinde **Assets.xcassets** projenizin dosya.

Varlık kataloğunda gerekli simgesi adlı **App Store** ve olmalıdır **1024 x 1024** boyutu. Apple varlık kataloğunda uygulama mağazası simgesini saydam olamaz veya bir alfa kanal içeren belirtilmiş.

Depolama simgesi ayarlama hakkında bilgi için bkz [uygulama depolamak simgesini](~/ios/app-fundamentals/images-icons/app-store-icon.md) Kılavuzu.

### <a name="setting-the-apps-icons-and-launch-screens"></a>Uygulama simgeleri ve başlatma ekranlar ayarlama

Apple'nın App Store eklenmesi için kabul edilmesi bir iOS uygulaması için başlatma tüm iOS için üzerinde çalışacağı aygıtları ekranları ve uygun simgeleri gerektirir. Uygulama simgeleri eklenir projelerinizi bir varlık kataloğunda aracılığıyla bir **AppIcon** görüntü kümesinde **Assets.xcassets** dosya. Başlatma ekranlar film şeridi eklenir.

Uygulama simgeleri ve başlatma ekranlar oluşturma hakkında ayrıntılı yönergeler için bkz: [uygulama simgesi](~/ios/app-fundamentals/images-icons/app-icons.md) ve [başlatma ekranlar](~/ios/app-fundamentals/images-icons/launch-screens.md) kılavuzları.

### <a name="creating-and-installing-a-distribution-profile"></a>Oluşturma ve dağıtım profili yükleme

iOS kullanan *sağlama profilleri* belirli uygulama yapı nasıl dağıtılabilir denetlemek için. Bunlar, bir uygulamayı imzalamak için kullanılan sertifika ile ilgili bilgiler içeren dosyalardır *uygulama kimliği*, ve burada uygulama yüklenebilir. Geliştirme ve geçici dağıtım için sağlama profili uygulamayı dağıtabilirsiniz izin verilen cihazların listesini de içerir. Ancak, uygulama mağazası dağıtım için yalnızca sertifika ve uygulama kimlik bilgileri dahil, genel dağıtım için yalnızca mekanizması uygulama mağazası olduğundan.

Hazırlama Apple'nın web tabanlı iOS sağlama portalı kullanarak aşağıdaki adımları içerir:

1.  Seçin **sağlama** > **dağıtım**.
2.  Tıklatın **+** düğmesini tıklatın ve olarak oluşturmak istediğiniz dağıtım profili türünü seçin **App Store**.
3.  Seçin **uygulama kimliği** için dağıtım profili oluşturmak istediğiniz aşağı açılan listeden.
4.  Uygulamayı imzalamak için geçerli bir üretim (dağıtım) sertifika seçin.
5.  Girin bir **adı** yeni **dağıtım profili** ve profil oluşturur.
6.  Mac için Xcode açın ve gidin **Tercihler > [Apple Kimliğinizi seçin] > ayrıntıları görüntüle...** . Tüm kullanılabilir profiller Mac üzerinde xcode'da indirin
7.  IDE ve altına dönüş **iOS paket imzalama** seçeneklerini seçmek için doğru dağıtım sağlama profili _yapı yapılandırması_ (Bu da olacaktır **App Store**veya **sürüm**).

Ayrıntılı yönergeler için lütfen bkz [dağıtım profili oluşturma](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) ve [bir Xamarin.iOS projesi dağıtım profil seçme](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile).

## <a name="setting-the-build-configuration-for-your-application"></a>Uygulamanız için yapı yapılandırması

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Aşağıdakileri yapın:

1. Sağ tıklayın **proje adı** içinde **çözüm paneli** ve seçim **seçeneği** düzenlemek için açın.
2. Seçin **iOS yapı** seçip **yayın | iPhone** gelen **yapılandırma** açılır:

    ![](publishing-to-the-app-store-images/configurevs01.png "AppStore yapılandırma aşağı açılır listeden seçin")

3. Hedeflediğiniz belirli iOS sürüm varsa, buradan seçebilirsiniz **SDK sürümü** açılır listesinde, başka bu değer varsayılan ayarını bırakın.
4. Bağlama küçültür genel uygulamanızın dağıtılabilir kullanılmayan yöntemler, özellikler, sınıflar çıkarma tarafından vs. ve çoğu durumda varsayılan değer bırakılmalıdır **bağlantı SDK derlemeleri yalnızca**. Ne zaman gibi bazı durumlarda, bazı özel kullanarak 3. taraf kitaplıklar, bu değer ayarlanırsa zorlanabilir **bağlantı** kaldırılmakta olan gerekli öğeleri tutmak için. Daha fazla bilgi için bkz [iOS derleme mekanizması](~/ios/deploy-test/ios-build-mechanics.md) Kılavuzu.
5. **İOS için en iyi duruma getirme PNG görüntü dosyalarını** bu başka düşüş uygulamanızın teslim edilebilir boyutu yardımcı olacak şekilde, onay kutusu işaretlenmelidir.
6. Hata ayıklama gereken _değil_ yapı düşükse yapacak şekilde etkinleştirilmiş olmalıdır.
8. İOS 11 destekleyen aygıt mimarileri birini seçmeniz gerekir **ARM64**. 64 bit iOS cihazları için oluşturma hakkında daha fazla bilgi için lütfen bkz **etkinleştirme 64 Bit oluşturur, Xamarin.iOS uygulamaları** bölümünü [32/64 bit Platform konuları](~/cross-platform/macios/32-and-64/index.md) belgeleri.
9. İsteğe bağlı olarak kullanmak isteyebilirsiniz **LLVM** derleyici derlemek için daha uzun sürer ancak, daha küçük ve daha hızlı kod oluşturur.
10. Uygulamanızın gereksinimlerine göre size ayrıca türünü ayarlamak isteyebilirsiniz **çöp toplama** kullanılmakta ve kurulum için **uluslararası hale getirme**.
11. Yaptığınız değişiklikleri derleme yapılandırmasını kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da yeni bir Xamarin.iOS uygulaması oluşturduğunuzda varsayılan olarak, _yapı yapılandırmaları_ her ikisi için otomatik olarak oluşturulan **geçici** ve **App Store** dağıtımı. Son yapı, gönderme, uygulamanızın Apple'a yapmadan önce temel yapılandırma yapmanız gerekecektir birkaç değişiklik vardır.

Aşağıdakileri yapın:

1. Sağ tıklayın **proje adı** içinde **Çözüm Gezgini** ve seçim **özellikleri** düzenlemek için açın.
2. Seçin **iOS yapı** ve **AppStore** (veya **sürüm** AppStore kullanılamıyorsa) gelen **yapılandırma** açılır:

    ![](publishing-to-the-app-store-images/configurevs01.png "AppStore yapılandırma aşağı açılır listeden seçin")

3. Hedeflediğiniz belirli iOS sürüm varsa, buradan seçebilirsiniz **SDK sürümü** açılır listesinde, başka bu değer varsayılan ayarını bırakın.
4. Bağlama küçültür genel uygulamanızın dağıtılabilir kullanılmayan yöntemler, özellikler, sınıflar çıkarma tarafından vs. ve çoğu durumda varsayılan değer bırakılmalıdır **bağlantı SDK derlemeleri yalnızca**. Ne zaman gibi bazı durumlarda, bazı özel kullanarak 3. taraf kitaplıklar, bu değer ayarlanırsa zorlanabilir **bağlantı** kaldırılmakta olan gerekli öğeleri tutmak için. Daha fazla bilgi için bkz [iOS derleme mekanizması](~/ios/deploy-test/ios-build-mechanics.md) Kılavuzu.
5. **İOS için en iyi duruma getirme PNG görüntü dosyalarını** bu başka düşüş uygulamanızın teslim edilebilir boyutu yardımcı olacak şekilde, onay kutusu işaretlenmelidir.
6. Hata ayıklama gereken _değil_ yapı düşükse yapacak şekilde etkinleştirilmiş olmalıdır.
7. Tıklayın **Gelişmiş** sekmesi:

    ![](publishing-to-the-app-store-images/configurevs02.png "Gelişmiş sekmesi")

8. Xamarin.iOS uygulamanızı iOS 8 ve 64 bit iOS cihazlara hedefliyorsa, desteklediği cihaz mimarileri birini seçmeniz gerekir **ARM64**. 64 bit iOS cihazları için oluşturma hakkında daha fazla bilgi için lütfen bkz **etkinleştirme 64 Bit oluşturur, Xamarin.iOS uygulamaları** bölümünü [32/64 bit Platform konuları](~/cross-platform/macios/32-and-64/index.md) belgeleri.
9. İsteğe bağlı olarak kullanmak isteyebilirsiniz **LLVM** derleyici derlemek için daha uzun sürer ancak, daha küçük ve daha hızlı kod oluşturur.
10. Uygulamanızın gereksinimlerine göre size ayrıca türünü ayarlamak isteyebilirsiniz **çöp toplama** kullanılmakta ve kurulum için **uluslararası hale getirme**.
11. Yaptığınız değişiklikleri derleme yapılandırmasını kaydedin.

-----

## <a name="building-and-submitting-the-distributable"></a>Derleme ve dağıtılabilir gönderiliyor

Düzgün şekilde yapılandırılıp yapılandırılmadığını Xamarin.iOS uygulamanızı ile artık, gönderme son dağıtım yapı için Apple gözden geçirme ve yayın yapmak hazırsınız.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

### <a name="build-your-archive"></a>Arşiviniz derleme

1. Seçin **yayın | Aygıt** Mac için Visual Studio yapılandırması:

    ![](publishing-to-the-app-store-images/buildxs01new.png "Yayın Seç | Aygıt Yapılandırması")
1. Gelen **yapı** menüsünde, select **yayımlama arşiv**:

    ![](publishing-to-the-app-store-images/buildxs02new.png "Yayımlama Arşivi'ni seçin")

1. Arşiv oluşturulduktan sonra **arşivler** görünüm görüntülenecek:

    ![](publishing-to-the-app-store-images/buildxs03new.png "Arşivler görünümü görüntülenir")


> [!NOTE]
> While eski _App Store_ ve _geçici_ yapılandırmaları şimdi kaldırılmış tüm Mac şablon projeleri için Visual Studio, eski projeleri hala Bu yapılandırmalar içerdiğini fark edebilirsiniz. Bu durumda, kullanmaya devam edebilirsiniz **App Store | Aygıt** listenin yukarıdaki 1. adımda yapılandırma.

### <a name="sign-and-distribute-your-app"></a>Oturum ve uygulamanızı dağıtın

 Arşiv, uygulamanızın derleme her seferinde onu otomatik olarak açılır **arşivler Görünüm**, tüm görüntüleme projeleri arşivlenmiş; çözümü tarafından gruplandırılır. Varsayılan olarak bu görünüm yalnızca geçerli, açık çözüm gösterir. Arşivler sahip tüm çözümleri görmek için tıklayın **tüm arşivler Göster** seçeneği.

 Böylece oluşturulan tüm hata ayıklama bilgileri daha sonraki bir tarihte görüntülenir (App Store veya Kurumsal dağıtımlar) müşterilere dağıtılan arşivler kalmasını önerilir.

 Uygulamanızı imzalamak ve dağıtım için hazırlamak için:


1. Seçin **oturum ve Dağıt...** , aşağıda Resimli:

    ![](publishing-to-the-app-store-images/buildxs04new.png "Oturum seçin ve Dağıt...")

1. Bu, Yayımlama Sihirbazı'nı açar. Seçin **App Store** dağıtım kanal bir paket oluşturmak ve uygulama yükleyicisi açmak için:

    ![](publishing-to-the-app-store-images/distribute01.png "Açık uygulama yükleyicisi")

1. Sağlama profili ekranında, kimlik imzalama ve sağlama profili karşılık gelen seçin veya başka bir kimlikle yeniden oturum açın:

    ![](publishing-to-the-app-store-images/distribute02.png "Kimlik imzalama ve sağlama profili karşılık gelen seçin")

1. Paketinizi ayrıntılarını doğrulayın ve tıklatın **Yayımla** kaydetmek için `.ipa` paketi:

    ![](publishing-to-the-app-store-images/distribute03.png "Paket ayrıntılarını doğrulayın")

1. Bir kez, `.ipa` bırakıldı kaydedildi, uygulamanızı iTunes Connect uygulama yükleyicisi üzerinden yüklenecek hazırdır:

    ![](publishing-to-the-app-store-images/distribute04.png "Yayın başarılı ekranı")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio için Xamarin eklentisi şu anda arşivleme iş akışı uygulama mağazası yayımlama iOS uygulamaları için desteklemiyor. Sonuç olarak, bir IPA oluşturulan aracılığıyla karşıya yükleme sahip **yapı geçici IPA** aşağıda açıklanan komutu.


## <a name="build-an-ipa"></a>Bir IPA derleme

 Bu bölümde, geçici kullanırken, iş akışı veya kurumsal dağıtım için benzer bir IPA oluşturma açıklanmaktadır. Ancak, uygulama Mağazası'nın sağlama yukarıda oluşturduğunuz profili kullanarak imzalanacaktır.

 Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, Xamarin.iOS proje adına sağ tıklayın ve seçin **özellikleri** düzenlemek üzere açmak için:

    ![](publishing-to-the-app-store-images/imagevs01.png "Özellikler'i seçin")
1. Seçin **iOS paket imzalama** ve sağlama profili sağlama profili bir uygulama mağazasının değiştirin:

    ![](publishing-to-the-app-store-images/ipa01.png "İOS paket imzalama seçin ve sağlama profili sağlama profili bir uygulama mağazasının değiştirin")
1. Seçin **iOS IPA seçenekleri** seçip **geçici** gelen **yapılandırma** açılır listeden (varsa geçici görünmez, seçin **sürüm** Bunun yerine):

    ![](publishing-to-the-app-store-images/imagevs02.png "Geçici yapılandırma açılır listeden seçin")

1. İsteğe bağlı olarak belirleyebileceğiniz bir **paket adı** Xamarin.iOS projesi aynı ada sahip belirtilmezse IPA için.
1. Proje Özellikleri yaptığınız değişiklikleri kaydedin.
1. Seçin **geçici** Mac için Visual Studio'dan **yapı yapılandırması** açılır:

    ![](publishing-to-the-app-store-images/imagevs05.png "Geçici yapı yapılandırması aşağı açılır listeden seçin")
1. IPA paketi oluşturmak için projeyi oluşturun.
1. IPA oluşturulacak `Bin`  >  _iOS aygıtı_  >  `Ad Hoc` klasörü:

    ![](publishing-to-the-app-store-images/imagevs06.png "Dosya Gezgini'nde gösterilen IPA'sı")

-----


## <a name="customizing-the-ipa-location"></a>IPA konumu özelleştirme

Yeni bir **MSBuild** özelliği `IpaPackageDir` özelleştirmek kolaylaştırmak için eklenene `.ipa` çıkış dosyası konumu. Varsa `IpaPackageDir` özel bir konuma ayarlı `.ipa` dosya, varsayılan zaman damgalı alt yerine o konumda yerleştirilecek. Otomatik oluşturma sürekli tümleştirme (CI) derlemeler için kullanılanlar gibi doğru çalışması için bir özel dizin yolu Bel oluşturduğunda bu yararlı olabilir.

Yeni özellik kullanmak için olası birkaç yolu vardır:

Örneğin, çıktı için `.ipa` dosya eski varsayılan dizinine (Xamarin.iOS 9.6 olduğu gibi ve daha düşük), ayarladığınız `IpaPackageDir` özelliğine `$(OutputPath)` aşağıdaki yaklaşımlardan birini kullanarak. Her iki yaklaşımın yanı sıra kullanan komut satırı derlemeleri IDE yapılar dahil olmak üzere tüm birleşik API Xamarin.iOS derlemeleri ile uyumlu `xbuild`, `msbuild`, veya `mdtool`:

  - İlk seçenek ayarlamaktır `IpaPackageDir` özelliği içinde bir `<PropertyGroup>` öğesinde bir **MSBuild** dosya. Örneğin, aşağıdaki ekleyebilirsiniz `<PropertyGroup>` iOS uygulaması projesi altına `.csproj` dosyası (yalnızca kapatmadan önce `</Project>` etiketi):

      ```xml
        <PropertyGroup>
            <IpaPackageDir>$(OutputPath)</IpaPackageDir>
        </PropertyGroup>
      ```
  - Daha iyi bir yaklaşım eklemektir bir `<IpaPackageDir>` var olan alt öğeye `<PropertyGroup>` oluşturmak için kullanılan yapılandırma karşılık gelen `.ipa` dosyası. Bu proje iOS IPA seçeneklerini proje özellikleri sayfasında planlı bir ayar ile gelecekte uyumluluk hazırlayacağı iyidir. Şu anda kullanırsanız `Release|iPhone` oluşturmak için yapılandırma `.ipa` dosyası tam güncelleştirilmiş özellik grubu görünebilir aşağıdakine benzer:

      ```xml
      <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' )
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
      </PropertyGroup>
      ```
İçin alternatif bir tekniği `msbuild` veya `xbuild` komut satırı derlemeleri olduğu eklemek için bir `/p:` ayarlamak için komut satırı bağımsız değişkeni `IpaPackageDir` özelliği. Bu durumda unutmayın `msbuild` genişletme `$()` ifadeleri geçirilen komut satırında kullanmak mümkün değildir `$(OutputPath)` sözdizimi. Bunun yerine bir tam yol adı sağlamanız gerekir. Mono'nın `xbuild` Genişlet komutu `$()` ifadeler, ancak bir tam yol adı olduğundan kullanmak için hala tercih `xbuild` sonunda şunun için kullanım dışı kalacaktır [platformlar arası sürümü `msbuild` ](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x)gelecekte serbest bırakır. Bu yaklaşımı kullanır tam bir örnek Windows'da şuna benzer şekilde görünebilir:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Veya aşağıdaki Mac üzerinde:

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

Dağıtımınız ile derleme oluşturulur ve arşivleneceğini, artık iTunes Bağlan uygulamanıza göndermek hazırsınız.

### <a name="automatically-copy-app-bundles-back-to-windows"></a>Windows geri .app paketleri otomatik olarak kopyalama

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="submitting-your-app-to-apple"></a>Uygulamanızı apple'a gönderiliyor

> [!NOTE]
> Apple iOS uygulamaları için doğrulama işlemi son zamanlarda değişti ve uygulamalarla reddedebilir `iTunesMetadata.plist` IPA dahil. Hatayla karşılaşırsanız `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`açıklanan geçici çözüm [burada](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) sorun gidermeniz gerekir.

Dağıtım yapı tamamlandı, iOS uygulamanız gözden geçirme için Apple göndermek ve App Store'da sürüm hazır olursunuz.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 Aşağıdakileri yapın:

1. Başlat **Xcode**.
2. Gelen **penceresi** menüsünü seçin **düzenleyici**.
3. Tıklayın **arşivler** sekmesinde ve yukarıda oluşturulan Arşivi'ni seçin:

    ![](publishing-to-the-app-store-images/publishxs01.png "Göndermek için Arşivi'ni seçin")
4. Tıklayın **doğrula...**  düğmesi.
5. Karşı doğrulayın ve tıklatın için hesabı seçin **Seç** düğmesi:

    ![](publishing-to-the-app-store-images/publishxs02.png "Karşı doğrulamak için bir hesap seçin")
6. Tıklatın **doğrulama** düğmesi:

    ![](publishing-to-the-app-store-images/publishxs03.png "Dosya Özeti iletişim kutusu")
7. Paket herhangi bir sorun varsa, bunlar görüntülenir.
8. Tüm sorunları düzeltin ve Visual Studio'da arşiv Mac için yeniden oluşturma
9. Tıklayın **gönderme...**  düğmesi.
10. Karşı doğrulayın ve tıklatın için hesabı seçin **Seç** düğmesi:

    ![](publishing-to-the-app-store-images/publishxs04.png "Karşı doğrulamak için bir hesap seçin")
11. Tıklatın **gönderme** düğmesi:

    ![](publishing-to-the-app-store-images/publishxs05.png "Dosya Özeti iletişim kutusu")
12. Xcode bildirin, ne zaman tamamlandı iTunes Bağlan dosyası karşıya yükleniyor.


Kaydettikten sonra arşiv iş akışı Mac için Visual Studio içinde uygulama yükleyicisi otomatik olarak açılır _.ipa_:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Apple uygulamanıza gözden geçirme için gönderme uygulama yükleyicisi uygulama kullanılarak yapılır. Bu adımları Mac yapı ana bilgisayarda yapılması gerekir. Uygulama Yükleyicisi ' bir kopyasını yükleyebilirsiniz [burada](https://itunesconnect.apple.com/apploader/ApplicationLoader_3.0.dmg).

-----

1. Seçin *uygulamanızı teslim* tıklatıp *Seç* düğmesi:

    [![](publishing-to-the-app-store-images/publishvs01.png "Uygulamanızı seçin teslim")](publishing-to-the-app-store-images/publishvs01.png#lightbox)

2. Zip ya da yukarıda oluşturduğunuz IPA dosyasını seçin ve'ı tıklatın **Tamam** düğmesi.

3. Uygulama Yükleyicisi dosya doğrular:

    [![](publishing-to-the-app-store-images/publishvs02.png "Doğrulama ekranı")](publishing-to-the-app-store-images/publishvs02.png#lightbox)
4. Tıklatın *sonraki* düğmesi ve uygulamayı doğrulanmış karşı App Store:

    [![](publishing-to-the-app-store-images/publishvs03.png "Uygulama mağazası karşı doğrulama")](publishing-to-the-app-store-images/publishvs03.png#lightbox)
5. Tıklatın **Gönder** Apple uygulamaya gözden geçirme için Gönder düğmesi.
6. Uygulama Yükleyicisi zaman dosyası başarıyla karşıya yüklendi size bildirir.

## <a name="itunes-connect-status"></a>iTunes bağlantı durumu

Oturum iTunes geri Bağlan ve uygulamanızı kullanılabilir uygulamaları listesinden seçin, iTunes Bağlan durum şimdi bu olduğunu göstermelidir **gözden geçirilmek üzere bekleyen** (geçici olarak okuyabilirsiniz **alınanKarşıyaYükle** işlenirken):

[![](publishing-to-the-app-store-images/image21.png "İTunes durum Connect artık gözden geçirilmek üzere beklediğini gösterilmesi gerekir")](publishing-to-the-app-store-images/image21.png#lightbox)

## <a name="summary"></a>Özet

Bu makalede, yapılandırma, oluşturma ve uygulamanın uygulama mağazası yayının göndermek için adım adım kılavuz sunulmuştur. İlk olarak, oluşturmak ve sağlama profili bir dağıtım yüklemek için gerekli olan adımları ele. Ardından, Visual Studio ve Mac için Visual Studio dağıtım yapı oluşturmak için nasıl kullanılacağı aracılığıyla gitti. Son olarak, iTunes Bağlan ve aracı uygulamanın uygulama mağazası göndermek için nasıl kullanılacağı gösterilmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [Görüntülerle Çalışma](~/ios/app-fundamentals/images-icons/index.md)
- [iOS uygulama geliştirme iş akışı Kılavuzu: uygulamaları dağıtma](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [Uygulama mağazası gönderimi ipuçları](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Ortak uygulama reddi](https://developer.apple.com/app-store/review/rejections/)
- [App Store gözden geçirme yönergeleri](https://developer.apple.com/appstore/resources/approval/guidelines.html)
