---
title: "Apple TV uygulama mağazası yayımlama"
description: "Bu makalede, yapılandırma, yapı ve dağıtım Apple TV App Store aracılığıyla Xamarin.tvOS uygulama yayımlama gösterilmektedir. Uygulamanızı dağıtım için hazırlamak üzere nasıl, Apple'nın araçları gözden geçirme için uygulamanızı göndermek için nasıl kullanılacağı ve, son olarak, uygulamanızı Apple TV uygulama Mağazası'na yayımlamak nasıl kapsayan bir adım adım kılavuz içerir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: abb8ee30828e5d9856b9fd72cca8adb669959818
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Apple TV uygulama mağazası yayımlama

_Bu makalede, yapılandırma, yapı ve dağıtım Apple TV App Store aracılığıyla Xamarin.tvOS uygulama yayımlama gösterilmektedir. Uygulamanızı dağıtım için hazırlamak üzere nasıl, Apple'nın araçları gözden geçirme için uygulamanızı göndermek için nasıl kullanılacağı ve, son olarak, uygulamanızı Apple TV uygulama Mağazası'na yayımlamak nasıl kapsayan bir adım adım kılavuz içerir._

Tüm Apple TV cihazlara uygulamaları sırayla dağıtmak, Apple gerektirir üzerinden yayımlanan uygulamalar *Apple TV App Store*, bir Dur alışveriş konumunu tvOS uygulamalar için uygulama mağazası yaparak. Uygulamaları birçok türdeki geliştiriciler bu tek bir dağıtım noktasını yoğun başarı büyük harfe. Apple TV uygulama hazır bir çözüm uygulama geliştiriciler dağıtım ve ödeme sistemleri sunumu deposudur.

Apple TV App Store uygulamaya gönderme işleminin içerir:

1. Oluşturma bir *uygulama kimliği* ve seçerek *yetkilendirmeler*.
2. Oluşturma bir *sağlama profili dağıtım.*
3. Uygulamanızı oluşturmak için bu profili kullanarak.
4. Uygulamanızı aracılığıyla gönderme *iTunes Bağlan*.


Bu makalede biz sağlamak, geliştirmek ve Apple TV App Store dağıtım için bir uygulama göndermek için gereken tüm adımları kapsar.

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>Bir uygulama göndermeden önce

Apple TV App Store yayınlanması için bir uygulama gönderdikten sonra kalite ve içerik için Apple'nın yönergeleri karşıladığını güvence altına almaya Apple tarafından gözden geçirme sürecinde gider. Uygulamanız bu kılavuzları karşılamak başarısız olursa, Apple, Apple tarafından bildirdi uyumsuzluk adres ve daha sonra yeniden gönderin gerekir aynı zamanda reddeder.
Bu nedenle, Apple aracılığıyla kendiniz bu yönergelere hakkında bilgi edinme ve bunları uygulamanıza uyum çalışılırken gözden kolaylaştırarak en iyi ihtimalini bildirimde. Apple'nın yönergelerdir adresinde [App Store gözden geçirme yönergeleri](https://developer.apple.com/appstore/resources/approval/guidelines.html) ve [hazırlama Your App gönderme yeni Apple TV için](https://developer.apple.com/tvos/submit/).

Birkaç uygulama gönderirken dikkat edilmesi gerekenler şunlardır:

1. Uygulamanın açıklaması uygulamada işlevselliğin eşleştiğinden emin olun.
2. Uygulama kilitlenme değil test normal kullanım altında. Bu kullanım desteklediğiniz her Apple TV aygıtta içerir.


Ayrıca Apple, Apple TV App Store gönderimi ipuçları listesini tutar. Bunlara okuyabilirsiniz [App Store'da dağıtma](https://developer.apple.com/appstore/resources/submission/tips.html).

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>İTunes Bağlan uygulamanızı yapılandırma

[iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) başka şeylerin Apple TV App Store, tvOS uygulamaları yönetmek için web tabanlı araçları paketidir. Xamarin.tvOS uygulamanızı düzgün Kurulum olması gerekir ve Apple için gözden geçirilmesi için gönderilebilir ve sonuç olarak, satış veya Apple TV uygulama mağazasında bulunan ücretsiz bir uygulama olarak serbest bırakılacak önce iTunes Bağlan yapılandırılmış.

Aşağıdakileri yapın:

1. Uygun anlaşmaları yerinde ve içinde güncel olduğundan emin olun **anlaşmaları, vergi ve bankacılık** iTunes satış veya ücretsiz bir iOS uygulaması yayımlamayı Connect Bölümü.
2. Yeni bir **iTunes Connect kaydı** uygulama için ve belirtin, **görünen adı** (Apple TV uygulama Mağazası'ndaki görüldüğü gibi).
3. Seçin bir **satış fiyatı** veya uygulama ücretsiz yayımlanması belirtin.
4. Sağlayan bir **uygulama mağazası simgesini** (büyük simge) desteklediği Apple TV cihazlarda eylem uygulamanızda ekran görüntüleri. Bkz: bizim [simgeleri ve görüntüleri çalışma](~/ios/tvos/app-fundamentals/icons-images.md) daha fazla ayrıntı için Kılavuzu.
5. Bir düz sağlamak kısa **açıklama** uygulamasının özelliklerini de dahil olmak üzere ve son kullanıcıya yararlanabilirsiniz.
6. Sağlamak **kategorileri**, **alt kategorileri**, ve **anahtar sözcükleri** kullanıcı uygulamanızı Apple TV uygulama Mağazası'ndaki Bul yardımcı olacak.
7. Sağlamak **kişi** ve **Destek** Apple tarafından gerekli Web sitesine URL'lerini.
8. Uygulamanızın ayarlamak **derecelendirme**, ebeveyn denetimleri Apple TV App Store'tarafından kullanılıyor.
9. İsteğe bağlı uygulama mağazası teknolojileri gibi yapılandırmadan **Game Center** ve **uygulama içi satın alma**.

Daha fazla ayrıntı için lütfen bkz. bizim [, tvOS uygulama iTunes Connect yapılandırma](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) belgeleri.

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>Uygulama mağazası dağıtım için hazırlama

Apple TV uygulama Mağazası'na bir uygulama yayımlamak için ilk birçok adımı kapsar dağıtım oluşturmanız gerekir. Aşağıdaki bölümlerde oluşturulabilir ve Apple TV App Store gözden geçirme ve sürüm göndermek için yayımlama Xamarin.tvOS uygulama hazırlanmak için gereken her şeyi içerir.

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>Uygulama hizmetleri için sağlama

Apple özel uygulama hizmetleri için benzersiz bir kimlik oluşturduğunuzda, tvOS uygulamanız için etkin hale getirilebilir yetkilendirmeler olarak da adlandırılan, seçimi sağlar. Veya özel yetkilendirmeler kullanıp kullanmadığınızı hala Apple TV App Store'da yayımlanabilmesi Xamarin.tvOS uygulamanız için benzersiz bir kimliği oluşturmanız gerekir.

Bir uygulama kimliği oluşturma ve isteğe bağlı olarak yetkilendirmeler seçerek Apple'nın web tabanlı iOS sağlama portalı kullanarak aşağıdaki adımları içerir:

1. Seçin **sağlama** > **geliştirme**.
2. Tıklatın  **+**  düğmesine tıklayın ve sağlayan bir **adı** ve **paket kimliği** yeni uygulama için.
3. Ekranın alt kısmına kaydırın ve seçin **uygulama hizmetleri** , gerekir Xamarin.tvOS uygulamanız tarafından.
4. Tıklatın **devam** düğmesi ve aşağıdaki ekran yeni bir uygulama kimliği oluşturma yönergeleri

Seçme ve uygulama Kimliğiniz tanımlarken, gerekli uygulama hizmetlerini yapılandırma ek olarak, ayrıca uygulama kimliği ve yetkilendirmeler Xamarin.tvOS projenizde her ikisi de düzenleyerek yapılandırmanız gereken `Info.plist` ve `Entitlements.plist` dosyaları.

Visual Studio'da Mac için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Info.plist` dosyayı düzenlemek için açın.
2. İçinde **tvOS uygulama hedef** bölümünde, uygulamanız için bir ad girin ve girin **paket tanımlayıcı** bir uygulama kimliği tanımlandığında, oluşturduğunuz
3. Değişiklikleri kaydetmek `Info.plist` dosya.
4. İçinde **Çözüm Gezgini**, çift `Entitlements.plist` dosyayı düzenlemek için açın.
5. Seçin ve bunlar üzerinde gerçekleştirilen bir uygulama kimliği tanımlandığında Kurulum eşleşmesi sizin için Xamarin.tvOS uygulama gerekli yetkilendirmeler yapılandırın
6. Değişiklikleri kaydetmek `Entitlements.plist` dosya.

Ayrıntılı yönergeler için lütfen bkz bizim [için uygulama hizmetleri sağlama](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) belgeleri. Bu belgede iOS için yazılmış olsa da, aynı adımları Xamarin.tvOS uygulama sağlamak için kullanılır.

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>Uygulama simgeleri, başlatma görüntü ve üst raf görüntü ayarlama

Bir tvOS uygulama Apple TV App Store eklenmek üzere Apple tarafından kabul edilmesi uygun simgeleri, başlatma ve bunun üzerinde çalışacağı tüm Apple TV aygıtları için üst raf görüntüleri gerektirir. İçine derlenmiş gerekli görüntü varlıklarının ekleyeceksiniz bir `Assets.car` dosya ve iTunes Bağlan karşıya önce Xamarin.tvOS uygulamanızın pakete eklenen.

Ayrıntılı yönergeler için lütfen bkz bizim [simgeleri ve görüntüleri çalışma](~/ios/tvos/app-fundamentals/icons-images.md) belgeleri.

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>Oluşturma ve dağıtım profili yükleme

tvOS kullanan *sağlama profilleri* belirli uygulama yapı nasıl dağıtılabilir denetlemek için. Bunlar, bir uygulamayı imzalamak için kullanılan sertifika ile ilgili bilgiler içeren dosyalardır *uygulama kimliği*, ve burada uygulama yüklenebilir. Geliştirme ve geçici dağıtım için sağlama profili uygulamayı dağıtabilirsiniz izin verilen cihazların listesini de içerir. Ancak, Apple TV App Store dağıtım için yalnızca sertifika ve uygulama kimlik bilgileri dahil, genel dağıtım için yalnızca mekanizması Apple TV App Store olduğundan.

Hazırlama Apple'nın web tabanlı iOS sağlama portalı kullanarak aşağıdaki adımları içerir:

1.  Seçin **sağlama** > **dağıtım**.
2.  Tıklatın  **+**  düğmesini tıklatın ve olarak oluşturmak istediğiniz dağıtım profili türünü seçin **Apple TV App Store**.
3.  Seçin **uygulama kimliği** için dağıtım profili oluşturmak istediğiniz aşağı açılan listeden.
4.  Uygulamayı imzalamak için gerekli sertifikayı seçin.
5.  Girin bir **adı** yeni **dağıtım profili** ve profil oluşturur.
6.  Xcode'da kullanılabilir profiller listesini yenileyin.
7.  Sağlama profili için Visual Studio'da dağıtım seçin **uygulama mağazası** _yapı yapılandırma_.

Ayrıntılı yönergeler için lütfen bkz [dağıtım profili oluşturma](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile) ve [bir Xamarin.iOS projesi dağıtım profil seçme](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile). Yeniden, bu belgelerin her ikisi için iOS özel ancak aynı tekniği tvOS uygulamalar için kullanılır.


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>Uygulamanız için yapı yapılandırması

Varsayılan olarak, yeni bir Xamarin.tvOS uygulaması oluştururken, _yapı yapılandırmaları_ her ikisi için otomatik olarak oluşturulan **hata ayıklama** ve **sürüm** dağıtım. Son yapı, gönderme, uygulamanızın Apple'a yapmadan önce vardır tabanına vermeniz gereken birkaç değişiklik **sürüm** yapılandırma.

Aşağıdakileri yapın:

1. Sağ tıklayın **proje adı** içinde **Çözüm Gezgini** ve seçim **seçenekleri** düzenlemek için açın.
2. TvOS belirli bir sürümünü hedefliyorsanız, altında seçin **tvOS yapı** > **iOS SDK sürümü**. TvOS desteği Önizleme sürümü için lütfen ayarlamak bu değeri bırakın **varsayılan**.
3. Bağlama küçültür genel uygulamanızın dağıtılabilir kullanılmayan yöntemler, özellikler, sınıflar çıkarma tarafından vs. ve çoğu durumda varsayılan değer bırakılmalıdır **bağlantı framework SDK yalnızca**. Ne zaman gibi bazı durumlarda, bazı özel kullanarak 3. taraf kitaplıklar, bu değer ayarlanırsa zorlanabilir **bağlantı** kaldırılmakta olan gerekli öğe tutmak için.
4. Xamarin.tvOS uygulama dağıtmayı LLVM en iyi duruma getirme derleyici kullanıyor olmanız gerekir. Emin **derleyici en iyi duruma getirme LLVM kullanmak** kutunun işaretli altında **sürüm** yapılandırma.
5. Apple de tvOS uygulamaları bitcode'u kullanmanız gerekiyor. Yeniden altında **sürüm** Yapılandırması Ekle `--bitcode=asmonly` için **ek mtouch bağımsız değişkenleri** kutusu.
6. **İOS için en iyi duruma getirme PNG görüntü dosyalarını** bu daha fazla azaltmak için uygulamanızın teslim edilebilir boyutu yardımcı olacak şekilde, onay kutusu işaretlenmelidir.
7. Hata ayıklama gereken *değil* yapı gereksiz yere büyük yapar gibi etkin olmalıdır.


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>Derleme ve dağıtılabilir gönderiliyor

Düzgün şekilde yapılandırılıp yapılandırılmadığını Xamarin.tvOS uygulamanızı ile artık, gönderme son dağıtım yapı için Apple gözden geçirme ve yayın yapmak hazırsınız.

#### <a name="build-your-archive"></a>Arşiviniz derleme

1. Seçin **yayın | Aygıt** Mac için Visual Studio yapılandırması:

    ![](app-store-publishing-images/buildxs01new.png "Yayın yapılandırmasını seçin")
2. Gelen **yapı** menüsünde, select **yayımlama arşiv**:

    [![](app-store-publishing-images/buildxs02new.png "Yayımlama Arşivi'ni seçin")](app-store-publishing-images/buildxs02new.png#lightbox)
3. Arşiv oluşturulduktan sonra **arşivler** görünüm görüntülenecek:

    [![](app-store-publishing-images/buildxs03new.png "Arşivler görünümü")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>Oturum ve uygulamanızı dağıtın

Arşiv, uygulamanızın derleme her seferinde onu otomatik olarak açılır *arşivler Görünüm*, tüm görüntüleme projeleri arşivlenmiş; çözümü tarafından gruplandırılır. Varsayılan olarak bu görünüm yalnızca geçerli, açık çözüm gösterir. Arşivler sahip tüm çözümleri görmek için tıklayın **tüm arşivler Göster** seçeneği.

Böylece oluşturulan tüm hata ayıklama bilgileri daha sonraki bir tarihte görüntülenir (App Store veya Kurumsal dağıtımlar) müşterilere dağıtılan arşivler kalmasını önerilir.

Uygulamanızı imzalamak ve dağıtım için hazırlamak için:

1. Seçin **oturum ve Dağıt...** , aşağıda Resimli:

    [![](app-store-publishing-images/buildxs04new.png ", TheSign ve Dağıt seçin...")](app-store-publishing-images/buildxs04new.png#lightbox)
2. Bu, Yayımlama Sihirbazı'nı açar. Seçin **App Store** dağıtım kanal bir paket oluşturmak ve uygulama yükleyicisi açmak için:

    [![](app-store-publishing-images/distribute01.png "Uygulama mağazası dağıtım kanalını seçin")](app-store-publishing-images/distribute01.png#lightbox)
3. Sağlama profili ekranında, kimlik imzalama ve sağlama profili karşılık gelen seçin veya başka bir kimlikle yeniden oturum açın:

    [![](app-store-publishing-images/distribute02.png "Kimlik imzalama ve sağlama profili karşılık gelen seçin")](app-store-publishing-images/distribute02.png#lightbox)
4. Paketinizi ayrıntılarını doğrulayın ve tıklatın **Yayımla** kaydetmek için `.ipa` paketi:

    [![](app-store-publishing-images/distribute03.png "Paket ayrıntılarını doğrulayın")](app-store-publishing-images/distribute03.png#lightbox)
5. Bir kez, `.ipa` bırakıldı kaydedildi, uygulamanızı iTunes Connect uygulama yükleyicisi üzerinden yüklenecek hazırdır:

    [![](app-store-publishing-images/distribute04.png "İTunes Bağlan uygulama yükleyicisi aracılığıyla yüklenen")](app-store-publishing-images/distribute04.png#lightbox)

Dağıtımınız ile derleme oluşturulur ve arşivleneceğini, artık iTunes Bağlan uygulamanıza göndermek hazırsınız.

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Uygulamanızı apple'a gönderiliyor

Dağıtım yapı tamamlandı, iOS uygulamanız gözden geçirme için Apple göndermek ve App Store'da sürüm hazır olursunuz.


Kaydettikten sonra arşiv iş akışı Mac için Visual Studio içinde uygulama yükleyicisi otomatik olarak açılır `.ipa`:

2. Seçin *uygulamanızı teslim* tıklatıp *Seç* düğmesi:

    [![](app-store-publishing-images/publishvs01.png "Uygulamanızı seçin teslim")](app-store-publishing-images/publishvs01.png#lightbox)

3. Zip ya da yukarıda oluşturduğunuz IPA dosyasını seçin ve'ı tıklatın **Tamam** düğmesi.
4. Uygulama Yükleyicisi dosya doğrular:

    [![](app-store-publishing-images/publishvs02.png "Uygulama Yükleyicisi doğrulama ekranı")](app-store-publishing-images/publishvs02.png#lightbox)
5. Tıklatın *sonraki* düğmesi ve uygulamayı doğrulanmış karşı App Store:

    [![](app-store-publishing-images/publishvs03.png "Uygulama mağazası karşı Doğrulanmakta olan uygulama")](app-store-publishing-images/publishvs03.png#lightbox)
6. Tıklatın **Gönder** Apple uygulamaya gözden geçirme için Gönder düğmesi.
7. Uygulama Yükleyicisi zaman dosyası başarıyla karşıya yüklendi size bildirir.

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>iTunes bağlantı durumu

Oturum iTunes geri Bağlan ve uygulamanızı kullanılabilir uygulamaları listesinden seçin, iTunes Bağlan durumu şimdi bu olduğunu göstermelidir **gözden geçirilmek üzere bekleyen** (geçici olarak okuyabilirsiniz **karşıya alınan** işlenirken):

[![](app-store-publishing-images/image21.png "İTunes durum bağlanmak gözden geçirme için bekleniyor gösterme")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Sorun giderme

Apple TV App Store Xamarin.tvOS uygulamanıza gönderme sorunlarınız varsa lütfen bkz bizim [sorun giderme](~/ios/tvos/troubleshooting.md) Kılavuzu. Karşılaşabileceğiniz bazı bilinen sorunlar ve Xamarin.tvOS çözmek nasıl içerir.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, yapılandırma, oluşturma ve gönderme Apple TV App Store yayını için bir uygulama için adım adım kılavuz sunulmuştur. İlk olarak, oluşturmak ve sağlama profili bir dağıtım yüklemek için gerekli olan adımları ele. Ardından, Mac için Visual Studio dağıtım yapı oluşturmak için nasıl kullanılacağını aracılığıyla gitti. Son olarak, iTunes Bağlan ve Xcode arşiv aracı Apple TV App Store uygulamaya göndermek için nasıl kullanılacağı gösterilmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [Simgelerle ve Görüntülerle Çalışma](~/ios/tvos/app-fundamentals/icons-images.md)
- [Bilgisayarınızı uygulama gönderme yeni Apple TV için hazırlama](https://developer.apple.com/tvos/submit/)
- [Uygulama mağazası gönderimi ipuçları](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Ortak uygulama reddi](https://developer.apple.com/app-store/review/rejections/)
- [App Store gözden geçirme yönergeleri](https://developer.apple.com/appstore/resources/approval/guidelines.html)
