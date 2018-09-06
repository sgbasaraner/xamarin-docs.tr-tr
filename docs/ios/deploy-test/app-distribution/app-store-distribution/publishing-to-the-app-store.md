---
title: App Store için xamarin iOS uygulamaları yayımlama
description: Bu belge, yapılandırma, derleme ve dağıtım için bir Xamarin.iOS uygulaması App Store yayımlama açıklar.
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 7560f66acc3a3ea683e75be2ae85f908036e008c
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780664"
---
# <a name="publishing-xamarinios-apps-to-the-app-store"></a>App Store için xamarin iOS uygulamaları yayımlama

Uygulama için Yayımlama [App Store](https://www.apple.com/ios/app-store/), uygulama geliştiricisi önce onu – ekran görüntüleri, açıklama, simgeler ve diğer bilgileri – gözden geçirme için Apple birlikte göndermeniz gerekir. Uygulama onaylandıktan sonra Apple, burada kullanıcıları satın alma ve iOS cihazlarından doğrudan yükleme App Store, yerleştirir.

Bu kılavuzda, bir uygulamayı App Store için hazırlama ve Apple'a gözden geçirme için göndermek için izlemeniz gereken adımlar açıklanmaktadır. Özellikle, açıklanmaktadır:

> [!div class="checklist"]
> - App Store gözden geçirme yönergeleri izleyerek
> - Uygulama kimliği ve yetkilendirmelerini ayarlama
> - App Store simgesi ve uygulama simgeleri
> - Bir uygulama sağlama profili Store ayarlama
> - Güncelleştirme **yayın** derleme yapılandırması
> - İTunes CONNECT'te uygulamanızı yapılandırma
> - Uygulamanızı oluşturmaya ve Apple'a gönderme

> [!IMPORTANT]
> Apple [belirtti](https://developer.apple.com/news/?id=05072018a) Temmuz 2018'den itibaren tüm uygulamaları ve App Store için gönderilen güncelleştirmelerini iOS 11 SDK oluşturulduğundan gerekir emin ve [iPhone X görünen desteklemelidir](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md).

## <a name="app-store-guidelines"></a>App Store yönergeleri

Bir uygulama yayımlama App Store için göndermeden önce Apple tarafından tanımlanan standartları karşılar emin olun [App Store gözden geçirme yönergeleri](https://developer.apple.com/appstore/resources/approval/guidelines.html).
Apple, App Store için uygulama gönderdiğinizde bu gereksinimleri karşıladığından emin olmak için inceler. Kullanmıyorsa, Apple içerdiği reddeder ve alıntı sorunları giderin ve yeniden gönderin gerekecektir.
Bu nedenle, bunu öğrenmek için iyi bir fikirdir geliştirme sürecinde mümkün olduğunca erken yönergeleri.

Bir uygulama gönderirken dikkat edilmesi gereken birkaç şey:

1. Uygulamanın açıklaması işlevselliğini eşleştiğinden emin olun.
2. Uygulama kilitlenme olmayan test normal kullanım altında. Bu, desteklediği her bir iOS cihazında kullanım içerir.

Ayrıca göz atın [App Store ilgili kaynaklara](https://developer.apple.com/app-store/resources/) , Apple sağlar.

## <a name="set-up-an-app-id-and-entitlements"></a>Uygulama kimliği ve yetkilendirmelerini ayarlama

Uygulama Hizmetleri ilişkilendirilmiş bir dizi olan benzersiz uygulama kimliği, her bir iOS uygulamasına sahip adlı *yetkilendirmeler*. Yetkilendirmeler izin çeşitli şeyler uygulamalar gibi anında iletme bildirimi, aldığınız HealthKit ve daha fazlası gibi iOS özelliklerine erişin.

Uygulama kimliği oluşturun ve seçin için yetkilendirmeler gerekli, ziyaret [Apple Developer Portal'a](https://developer.apple.com/account/) ve aşağıdaki adımları izleyin:

1. İçinde **sertifikalar, kimlikler ve profiller** bölümünden **tanımlayıcıları > Uygulama kimlikleri**.
2. Tıklayın **+** düğmesine tıklayın ve sağlayan bir **adı** ve **paket kimliği** yeni uygulama için.
3. Ekranın altına kaydırın ve herhangi **uygulama hizmetleri** Xamarin.iOS uygulamanızı gerekecektir. Uygulama Hizmetleri bölümünde daha ayrıntılı açıklanmıştır [Xamarin.iOS özelliklerinde çalışma](~/ios/deploy-test/provisioning/capabilities/index.md) Kılavuzu.
4. Tıklayın **devam** izleyin ekrandaki yönergeleri yeni uygulama kimliğini oluşturmak için

Seçme ve gerekli uygulama hizmetleri, uygulama Kimliğiniz tanımlarken yapılandırma yanı sıra, uygulama kimliği ve yetkilendirmeler Xamarin.iOS projenizi düzenleyerek yapılandırmanız gerekir **Info.plist** ve  **Entitlements.plist** dosyaları. Daha fazla bilgi için göz atın [Xamarin.iOS yetkilendirmeleri çalışma](~/ios/deploy-test/provisioning/entitlements.md) nasıl oluşturulacağını açıklayan kılavuz bir **Entitlements.plist** dosya ve çeşitli yetkilendirme ayarlarını anlamı Bu içerir.

## <a name="include-an-app-store-icon"></a>App Store simgesi

Apple uygulama gönderdiğinizde, App Store simgesi içeren bir varlık Kataloğu içerdiğinden emin olun. Bunu öğrenmek için göz atın [Xamarin.iOS App Store simgeleri](~/ios/app-fundamentals/images-icons/app-store-icon.md) Kılavuzu.

## <a name="set-the-apps-icons-and-launch-screens"></a>Uygulama simgeleri ayarlayın ve başlatma ekranları

Bir iOS uygulamasını App Store üzerinde kullanılabilir yapmak için Apple, uygun simgeleri ve başlatma ekranları tüm iOS cihazları için bunu çalıştırabilir olmalıdır. Uygulama simgeleri ve başlatma ekranları ayarlama hakkında daha fazla bilgi için aşağıdaki kılavuzlara okuyun:

- [Xamarin.iOS uygulama simgeleri](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Başlatma ekranları Xamarin.iOS uygulamaları için](~/ios/app-fundamentals/images-icons/launch-screens.md)

## <a name="create-and-install-an-app-store-provisioning-profile"></a>Oluşturma ve sağlama profili bir App Store yükleme

iOS kullanan *sağlama profilleri* nasıl belirli bir uygulama derleme dağıtılacağını denetlemek için. Bunlar uygulama kimliği, bir uygulamayı imzalamak için kullanılan sertifika bilgilerini içeren ve uygulamanın yüklenebileceği dosyalardır. Sağlama profili, ayrıca geliştirme ve geçici dağıtım için uygulama dağıtabileceğiniz izin verilen cihaz listesini içerir. Yalnızca genel dağıtım mekanizması App Store olduğundan ancak App Store dağıtımı için yalnızca sertifikası ve uygulama kimliği bilgileri dahil edilir.

Oluşturma ve sağlama profili bir App Store yüklemek için aşağıdaki adımları izleyin:

1. Oturum [Apple Developer Portal'a](https://developer.apple.com/account/).
2. İçinde **sertifikalar, kimlikler ve profiller**seçin **sağlama profilleri > Dağıtım**.
3. Tıklayın **+** düğmesini seçme **App Store**, tıklatıp **devam**.
4. Uygulamanızın seçin **uygulama kimliği** listesi ve **devam**.
5. Bir imzalama sertifikasını seçin ve tıklayın **devam**.
6. Girin bir **profil adı** tıklatıp **devam** profili oluşturulacak.
7. Xamarin'in kullanın [Apple hesap yönetimi](~/cross-platform/macios/apple-account-management.md) profil Mac'iniz için yeni oluşturulan sağlama indirmek için Araçlar Mac bilgisayarlarda kullanıyorsanız, sağlama profili doğrudan Apple Geliştirici Portalı'ndan indirin ve yüklemek için çift tıklayın.

Ayrıntılı yönergeler için bkz. [dağıtım profili oluşturma](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) ve [dağıtım profil seçme içinde bir Xamarin.iOS projesi](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile).

## <a name="update-the-release-build-configuration"></a>Yayın derleme yapılandırmasını güncelleştirme

Yeni Xamarin.iOS projeleri otomatik olarak ayarlamak **hata ayıklama** ve **yayın** _derleme yapılandırmaları_. Düzgün şekilde yapılandırmak için **yayın** oluşturun, aşağıdaki adımları izleyin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Gelen **çözüm bölmesi**açın **Info.plist**. Seçin **el ile sağlama**. Dosyayı kaydedin ve kapatın.
2. Sağ **proje adı** içinde **çözüm bölmesi**seçin **seçenekleri**gidin **iOS derleme** sekmesi.
3. Ayarlama **yapılandırma** için **yayın** ve **Platform** için **iPhone**.
4. Belirli bir iOS SDK'sı ile oluşturmak için seçim **SDK sürümü** listesi. Aksi takdirde bu değerde bırakın **varsayılan**.
5. Bağlama, kullanılmayan kodu şeridi oluşturma tarafından uygulamanın toplam boyutunu azaltır. Çoğu durumda **bağlayıcı davranışı** varsayılan değerine ayarlanmalıdır **bağlantı Framework SDK'ları yalnızca**. Bazı durumlarda, bazı üçüncü taraf kitaplıklar kullanırken, bu değeri ayarlamak için gerekli olabilir gibi **bağlama** gerekli kodu değil kaldırıldığından emin olmak için. Daha fazla bilgi için [bağlama Xamarin.iOS uygulamaları](~/ios/deploy-test/linker.md) Kılavuzu.
6. Denetleme **en iyi duruma getirme PNG görüntülerini** daha uygulamanızın boyutunu azaltmak için.
7. Hata ayıklama gereken _değil_ etkin olması, yapı gereksiz derecede büyük hale getirecek şekilde.
8. İOS 11 destekleyen cihaz mimarileri birini **ARM64**. 64-bit iOS cihazları için oluşturma hakkında daha fazla bilgi için lütfen bkz **xamarin iOS uygulamaları, etkinleştirme 64-Bit derlemeler** bölümünü [32/64 bit platformla ilgili dikkat edilecekler](~/cross-platform/macios/32-and-64/index.md) belgeleri.
9. Kullanmak istediğiniz **LLVM** daha küçük ve daha hızlı kod oluşturmak için derleyici. Ancak, bu seçenek, derleme sürelerini artırır.
10. Uygulamanızın ihtiyaçlarına göre ayrıca türünü ayarlamak isteyebilirsiniz **çöp toplama** kullanılan ve için ayarlanmış **uluslararası duruma getirme**.

    Yukarıda açıklanan seçenekler ayarladıktan sonra yapı ayarlarınızı şuna benzer görünmelidir:

    ![iOS derleme ayarlarını](publishing-to-the-app-store-images/build-m157.png "iOS derleme ayarları")

    Ayrıca bu göz atın [iOS derleme mekaniği](~/ios/deploy-test/ios-build-mechanics.md) Kılavuzu, daha fazla derleme ayarlarını açıklar.

11. Gidin **iOS paket grubu imzalama** sekmesi. Seçenekler burada düzenlenebilir değil, emin **el ile sağlama** seçili **Info.plist** dosya.
12. Emin olun **yapılandırma** ayarlanır **yayın** ve **Platform** ayarlanır **iPhone**.
13. Ayarlama **imzalama kimliği** için **dağıtım (otomatik)**.
14. İçin **sağlama profili**, sağlama profili App Store seçin [yukarıda oluşturulan](#create-and-install-an-app-store-provisioning-profile).

    Proje paket grubu imzalama seçenekleri artık şuna benzer görünmelidir:

    ![iOS paket grubu imzalama](publishing-to-the-app-store-images/bundleSigning-m157.png "iOS paket grubu imzalama")

15. Tıklayın **Tamam** değişiklikleri kaydetmek için proje özellikleri.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio 2017 aktarıldığından emin emin [eşleştirilmiş bir Mac derleme konağı](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Sağ **proje adı** içinde **Çözüm Gezgini**seçin **özellikleri**.
3. Gidin **iOS derleme** sekmesi ve ayarlayıp **yapılandırma** için **yayın** ve **Platform** için **iPhone**.
4. Belirli bir iOS SDK'sı ile oluşturmak için seçim **SDK sürümü** listesi. Aksi takdirde bu değerde bırakın **varsayılan**.
5. Bağlama, kullanılmayan kodu şeridi oluşturma tarafından uygulamanın toplam boyutunu azaltır. Çoğu durumda **bağlayıcı davranışı** varsayılan değerine ayarlanmalıdır **bağlantı Framework SDK'ları yalnızca**. Bazı durumlarda, bazı üçüncü taraf kitaplıklar kullanırken, bu değeri ayarlamak için gerekli olabilir gibi **bağlama** gerekli kodu değil kaldırıldığından emin olmak için. Daha fazla bilgi için [bağlama Xamarin.iOS uygulamaları](~/ios/deploy-test/linker.md) Kılavuzu.
6. Denetleme **en iyi duruma getirme PNG görüntülerini** daha uygulamanızın boyutunu azaltmak için.
7. Derleme gereksiz derecede büyük hale getirecek şekilde hata ayıklama, etkinleştirilmemelidir.
8. İOS 11 destekleyen cihaz mimarileri birini **ARM64**. 64-bit iOS cihazları için oluşturma hakkında daha fazla bilgi için lütfen bkz **xamarin iOS uygulamaları, etkinleştirme 64-Bit derlemeler** bölümünü [32/64 bit platformla ilgili dikkat edilecekler](~/cross-platform/macios/32-and-64/index.md) belgeleri.
9. Kullanmak istediğiniz **LLVM** daha küçük ve daha hızlı kod oluşturmak için derleyici. Ancak, bu seçenek, derleme sürelerini artırır.
10. Uygulamanızın ihtiyaçlarına göre ayrıca türünü ayarlamak isteyebilirsiniz **çöp toplama** kullanılan ve için ayarlanmış **uluslararası duruma getirme**.

    Yukarıda açıklanan seçenekler ayarladıktan sonra yapı ayarlarınızı şuna benzer görünmelidir:

    ![iOS derleme ayarlarını](publishing-to-the-app-store-images/build-w157.png "iOS derleme ayarları")

    Ayrıca bu göz atın [iOS derleme mekaniği](~/ios/deploy-test/ios-build-mechanics.md) Kılavuzu, daha fazla derleme ayarlarını açıklar.

11. Gidin **iOS paket grubu imzalama** sekmesi. Emin olun **yapılandırma** ayarlanır **yayın**, **Platform** ayarlanır **iPhone**ve **el ile sağlama**  seçilir.
12. Ayarlama **imzalama kimliği** için **dağıtım (otomatik)**.
13. İçin **sağlama profili**, sağlama profili App Store seçin [yukarıda oluşturulan](#create-and-install-an-app-store-provisioning-profile).

    Proje paket grubu imzalama seçenekleri artık şuna benzer görünmelidir:

    ![iOS paket grubu imzalama ayarlarını](publishing-to-the-app-store-images/bundleSigning-w157.png "iOS paket grubu imzalama ayarlarını")

14. Gidin **iOS IPA seçenekleri** sekmesi.
15. Emin olun **yapılandırma** ayarlanır **yayın** ve **Platform** ayarlanır **iPhone**.
16. Denetleme **iTunes paket Arşivi (IPA) Oluştur** onay kutusu. Bu ayar her neden olacak **yayın** .ipa dosyası oluşturmak için (, seçili yapılandırma olduğundan) oluşturun. Bu dosya için Apple App Store yayına yönelik gönderilebilir.

    > [!NOTE]
    > **iTunes meta verileri** ve **iTunesArtwork** App Store sürümleri için gerekli değildir. Daha fazla bilgi için göz atın [Xamarin.iOS uygulamalarında iTunesMetadata.plist dosyası](~/ios/deploy-test/app-distribution/itunesmetadata.md) ve [iTunes resmi](~/ios/app-fundamentals/images-icons/app-icons.md#itunes-artwork).

17. Xamarin.iOS projesi adından farklı bir .ipa dosya adı belirtmek için bu alana giriş **paket adı** alan.

    ![iOS paket grubu imzalama ayarlarını](publishing-to-the-app-store-images/ipaOptions-w157.png "iOS paket grubu imzalama ayarlarını")

18. Derleme yapılandırmasını kaydedin ve kapatın.

-----

## <a name="configure-your-app-in-itunes-connect"></a>İTunes CONNECT'te uygulamanızı yapılandırma

[iTunes Connect](https://itunesconnect.apple.com) App Store, iOS uygulamaları yönetmek için web tabanlı araçları paketidir. Apple'a gözden geçirmeniz için gönderildi ve App Store üzerinde yayımlanan önce Xamarin.iOS uygulamanızı iTunes CONNECT'te düzgün şekilde yapılandırılmalıdır.

Bunun nasıl yapılacağını öğrenmek için [uygulama iTunes CONNECT'te yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) Kılavuzu.

## <a name="build-and-submit-your-app"></a>Derleme ve uygulamanızı gönderin

Derleme ayarları düzgün yapılandırıldığını ve iTunes Connect gönderiminiz bekleyen ile artık uygulamanızı oluşturun ve Apple'a gönderme.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'da **yayın** yapılandırma ve oluşturmak istediğiniz bir cihaz (simülatörü değil) oluşturun.

    ![Yapılandırma ve platform seçimi derleme](publishing-to-the-app-store-images/chooseConfig-m157.png "yapı yapılandırma ve platform seçimi")

2. Gelen **derleme** menüsünde **yayımlama için arşiv**.
3. Arşiv oluşturulduktan sonra **arşivleri** görünümü görüntülenir:

    ![Arşivleri görüntüle](publishing-to-the-app-store-images/archives-m157.png "arşivleri görüntüle")

    > [!NOTE]
    > Varsayılan olarak **arşivleri** görünüm yalnızca açık çözüm için arşivleri gösterir. Arşivleri olan tüm çözümleri görmek için **tüm arşivleri Göster** onay kutusu. Hata ayıklama bilgileri içerirler gerekirse kilitlenme raporlarının sembollerini oluşturmak için kullanılabilir, böylece eski arşivleri tutmak için iyi bir fikirdir.

4. Tıklayın **imzala ve Dağıt...**  Yayımlama Sihirbazı'nı açın.
5. Seçin **App Store** dağıtım kanalı. **İleri**'ye tıklayın.

    ![Dağıtım Kanalı seçimi](publishing-to-the-app-store-images/distChannel-m157.png "dağıtım kanal seçimi")

6. İçinde **sağlama profili** penceresinde, imzalama kimliği, uygulama ve sağlama profili seçin. **İleri**'ye tıklayın.

    ![Sağlama profili seçimi](publishing-to-the-app-store-images/provProfileSelect-m157.png "sağlama profili seçimi")

7. Kullanımınızın Paket ayrıntılarını doğrulayın ve tıklayın **Yayımla** uygulamanız için bir .ipa dosyasını kaydetmek için:

    ![Uygulama ayrıntısı doğrulama](publishing-to-the-app-store-images/publish-m157.png "uygulama ayrıntısı doğrulama")

8. .İpa kaydedildikten sonra uygulamanızı iTunes CONNECT'te yüklenmek hazırdır.

    ![Gönderim için hazır](publishing-to-the-app-store-images/readyToGo-m157.png "gönderimi için hazır")

9. Tıklayın **açık uygulama yükleyicisi** ve oturum açın (yapmanız gerektiğini unutmayın [bir uygulamaya özgü parolası oluşturmanız](https://support.apple.com/ht204397) Apple Kimliğinizi için).

    > [!NOTE]
    > Aracı hakkında daha fazla bilgi için göz atın [uygulama yükleyicisi Apple'nın dilimden](https://help.apple.com/itc/apploader/#/apdS673accdb).

10. Seçin **uygulamanızı teslim** tıklatıp **Seç** düğmesi:

    ![Select teslim uygulamanızı](publishing-to-the-app-store-images/publishvs01.png "teslim uygulamanızı seçin")

11. Yukarıda oluşturduğunuz .ipa dosyasını seçin ve tıklayın **Tamam** düğmesi.
12. Uygulama Yükleyicisi dosya doğrular:

    ![Doğrulama ekranı](publishing-to-the-app-store-images/publishvs02.png "doğrulama ekranı")

13. Tıklayın **sonraki** düğmesi ve uygulamayı doğrulanmış karşı App Store:

    ![App Store karşı doğrulama](publishing-to-the-app-store-images/publishvs03.png "App Store karşı doğrulama")

14. Tıklayın **Gönder** Apple uygulamayı gözden geçirme için göndermek için düğme.
15. Uygulama Yükleyicisi zaman dosyası başarıyla karşıya yüklendi bilgilendirecektir.

    > [!NOTE]
    > Apple uygulamalarla Reddet **iTunesMetadata.plist** aşağıdaki gibi bir hata výsledek .ipa dosyasına dahil:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > Bu hata için bir geçici çözüm için göz atın [yazıya Xamarin forumlarında](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!NOTE]
> Visual Studio 2017 şu anda desteklemiyor **yayımlama için arşiv** iş akışı, Mac için Visual Studio içinde bulunamadı

1. Visual Studio 2017 aktarıldığından emin emin [eşleştirilmiş bir Mac derleme konağı](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Seçin **yayın** Visual Studio 2017'den **çözüm yapılandırmaları** açılır listesinde, ve **iPhone** gelen **çözüm platformları** açılır.

    ![Yapılandırma ve platform seçimi derleme](publishing-to-the-app-store-images/chooseConfig-w157.png "yapı yapılandırma ve platform seçimi")

3. Projeyi oluşturun. Bu, bir .ipa dosyası oluşturur.

    > [!NOTE]
    > [Yayın derleme yapılandırmasını güncelleştirme](#update-the-release-build-configuration) bölümünde bu belge, uygulamanın derleme ayarlarını her biri için bir .ipa dosyası oluşturmak için yapılandırılan **yayın** oluşturun.

4. Windows makinesinde .ipa dosyasını bulmak için Visual Studio 2017'de Xamarin.iOS proje adının üzerine sağ tıklayın **Çözüm Gezgini** ve **klasörü dosya Gezgini'nde Aç**. Ardından, tam olarak açılmış Windows **dosya Gezgini**, gitmek **iPhone/bin/yayın** alt. Sahip olduğunuz sürece [.ipa dosyasını çıkış konumunu özelleştirilmiş](#customize-the-ipa-location), bu dizinde olması gerekir.
5. Bunun yerine Mac derleme konağı .ipa dosyasını görüntülemek için Visual Studio 2017'de Xamarin.iOS proje adına sağ tıklayın **Çözüm Gezgini** (Windows üzerinde) seçip **derleme sunucusunda IPA dosyasını Göster**. Bunu bir **Bulucu** seçili .ipa dosyası ile Mac derleme konağı üzerindeki pencere.
6. Mac derleme konağı üzerinde açın **uygulama Başlatıcısı**. Xcode içindeki seçin **Xcode > açık bir geliştirme aracı > Uygulama Başlatıcısı**.

    > [!NOTE]
    > Aracı hakkında daha fazla bilgi için göz atın [uygulama yükleyicisi Apple'nın dilimden](https://help.apple.com/itc/apploader/#/apdS673accdb).

7. Uygulama Başlatıcısı için oturum açın (yapmanız gerektiğini Not [bir uygulamaya özgü parolası oluşturmanız](https://support.apple.com/ht204397) Apple Kimliğinizi için).
8. Seçin **uygulamanızı teslim** tıklatıp **Seç** düğmesi:

    ![Select teslim uygulamanızı ](publishing-to-the-app-store-images/publishvs01.png "teslim uygulamanızı seçin")

9. Yukarıda oluşturulan .ipa dosyasını seçin ve tıklayın **Tamam**.
10. Uygulama Yükleyicisi dosya doğrular:

    ![Doğrulama ekranı](publishing-to-the-app-store-images/publishvs02.png "doğrulama ekranı")

11. Tıklayın **sonraki** düğmesi ve uygulamayı doğrulanmış karşı App Store:

    ![App Store karşı doğrulama](publishing-to-the-app-store-images/publishvs03.png "App Store karşı doğrulama")

12. Tıklayın **Gönder** Apple uygulamayı gözden geçirme için göndermek için düğme.
13. Uygulama Yükleyicisi zaman dosyası başarıyla karşıya yüklendi bilgilendirecektir.

    > [!NOTE]
    > Apple uygulamalarla Reddet **iTunesMetadata.plist** aşağıdaki gibi bir hata výsledek .ipa dosyasına dahil:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > Bu hata için bir geçici çözüm için göz atın [yazıya Xamarin forumlarında](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1).

-----

## <a name="itunes-connect-status"></a>iTunes Connect durumu

Uygulama gönderiminiz durumunu görmek için iTunes CONNECT'te oturum açın ve uygulamanızı seçin. Başlangıç durumu olmalıdır **İnceleme için bekleyen**, geçici okuma ancak **karşıya alınan** bunu işlenirken.

![Gözden geçirme için bekleyen](publishing-to-the-app-store-images/image21.png "gözden geçirmeniz için bekleniyor")

## <a name="tips-and-tricks"></a>İpuçları ve püf noktaları

### <a name="customize-the-ipa-location"></a>.İpa konumunu özelleştirin

Bir **MSBuild** özelliği `IpaPackageDir`, .ipa dosyasını çıkış konumunu özelleştirmek mümkün kılar. Varsa `IpaPackageDir` ayarlanmış varsayılan zaman damgalı alt dizini yerine o konumda .ipa dosyası özel bir konuma yerleştirilir. Sürekli Tümleştirme (CI) derlemeleri için kullanılanlar gibi düzgün çalışması için bir özel dizin yolu dayanan derlemeleri otomatik oluşturma kullanılırken bu yararlı olabilir.

Yeni özelliği kullanmak için olası birkaç yolu vardır. Örneğin, eski varsayılan dizini (Xamarin.iOS 9.6 olduğu gibi ve daha düşük) .ipa dosyasına çıkarmak için ayarlayabileceğiniz `IpaPackageDir` özelliğini `$(OutputPath)` aşağıdaki yaklaşımlardan birini kullanarak. Her iki yaklaşım kullanan komut satırı derlemeleri yanı sıra, IDE yapılar da dahil olmak üzere tüm API Xamarin.iOS birleşik derlemeler ile uyumlu olan **msbuild** veya **mdtool**:

- İlk seçenek ayarlamaktır `IpaPackageDir` özelliği içinde bir `<PropertyGroup>` öğesinde bir **MSBuild** dosya. Örneğin, aşağıdaki ekleyebilirsiniz `<PropertyGroup>` iOS uygulaması proje .csproj dosyasının alt kısmına (yalnızca kapatmadan önce `</Project>` etiketi):

    ```xml
    <PropertyGroup>
      <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Daha iyi bir yaklaşım eklemektir bir `<IpaPackageDir>` varolan alt öğeye `<PropertyGroup>` .ipa dosyası oluşturmak için kullanılan yapılandırma karşılık gelir. İOS IPA seçenekleri proje özellikleri sayfasında bir planlı ayarıyla ileriye dönük uyumluluk için proje hazırlayacak daha iyi olmasıdır. Şu anda kullanıyorsanız `Release|iPhone` yapılandırma tam güncelleştirilmiş özellik grubunu .ipa dosyasını oluşturmak için aşağıdaki gibi görünebilir:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone'">
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

Alternatif bir yöntem için **msbuild** olan komut satırı derlemeleri eklemek için bir `/p:` ayarlamak için komut satırı bağımsız değişkeni `IpaPackageDir` özelliği. Bu durumda aklınızda bulundurun **msbuild** genişletme `$()` ifadeleri geçirilen komut satırında kullanmak mümkün değildir `$(OutputPath)` söz dizimi. Bunun yerine, bir tam yol adı belirtmeniz gerekir.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Veya aşağıdaki Mac üzerinde:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

Dağıtımınız ile yapı oluşturulan ve arşivlenmiş, artık uygulamanızı iTunes Connect göndermek hazır olursunuz.

## <a name="summary"></a>Özet

Bu makalede yapılandırma, oluşturma ve bir iOS App Store yayına yönelik gönderme açıklanmıştır.

## <a name="related-links"></a>İlgili bağlantılar

- [Apple Geliştirici Portalı'nı (Apple)](https://developer.apple.com/account/)
- [iTunes Connect (Apple)](https://itunesconnect.apple.com)
- [App Store gözden geçirme yönergeleri (Apple)](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Ortak uygulama redleri (Apple)](https://developer.apple.com/app-store/review/rejections/)
- [Xamarin.iOS'teki özellikler ile çalışma](~/ios/deploy-test/provisioning/capabilities/index.md)
- [Xamarin.iOS yetkilendirmeleri ile çalışma](~/ios/deploy-test/provisioning/entitlements.md)
- [Uygulama iTunes CONNECT'te yapılandırma](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Xamarin.iOS uygulama simgeleri](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Başlatma ekranları Xamarin.iOS uygulamaları için](~/ios/app-fundamentals/images-icons/launch-screens.md)
- [Uygulama Yükleyicisi belgeleri (Apple)](https://help.apple.com/itc/apploader/#/apdS673accdb)
