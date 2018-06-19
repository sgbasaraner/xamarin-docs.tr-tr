---
title: Uygulama iTunes Connect yapılandırma
description: Bu makalede Kurulum ve dağıtım App Store'dan yayımlanabilecek böylece iTunes Bağlan bir Xamarin.iOS uygulamasına korumak için gerekli adımlar kapsanmaktadır.
ms.prod: xamarin
ms.assetid: 74587317-4b15-4904-9582-dcd914827cbc
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 05492f866bb083326ef1eccb8db3d624d8dc4806
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209316"
---
# <a name="configuring-an-app-in-itunes-connect"></a>Uygulama iTunes Connect yapılandırma

> [!IMPORTANT]
> Apple [belirtti](https://developer.apple.com/news/?id=05072018a) Temmuz 2018 başlayarak, tüm uygulamaları ve güncelleştirmeleri uygulama mağazasında gönderilen iOS 11 SDK oluşturulmuş gerekir olduğunu ve [iPhone X görüntülenmesini desteklemek](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md).

iTunes Connect web tabanlı başka şeylerin App Store'dan iOS uygulamaları yönetmek için Araçlar paketidir. Bir Xamarin.iOS uygulaması düzgün şekilde ayarlanmış ve Apple'a gözden geçirilmek üzere gönderilen ve sonuçta satış ya da boş bir uygulama uygulama mağazasında olarak yayımlanan önce iTunes Bağlan yapılandırılması gerekir.

iTunes Bağlan aşağıdakiler için kullanılabilir:

- Uygulamanın adı (uygulama Mağazası'nda görüntülenen gibi) ayarlayın.
- Ekran görüntüleri veya video eylem uygulamasının desteklediği iOS cihazlarda sağlar.
- Buna ait özellikler de dahil olmak üzere uygulama NET, kısa bir açıklamasını sağlar ve son kullanıcıya yararlanabilirsiniz.
- Kategorileri ve alt kategoriler kullanıcı uygulamanın uygulama Mağazası'nda Bul yardımcı olmak üzere sağlar.
- Daha fazla kullanıcı uygulamayı Bul yardımcı olabilecek herhangi bir anahtar sözcük sağlar.
- (Apple tarafından gerekli) Web sitesine başvurun ve Destek URL'lerini belirtin.
- App Store'dan ebeveyn denetimleri bilgilendirmek için kullanılan uygulama derecelendirme ayarlayın.
- Bir satış fiyat seçin veya uygulama ücretsiz yayımlanması belirtin.
- Oyun merkezi ve uygulama içi satın alma gibi isteğe bağlı uygulama mağazası teknolojileri yapılandırın.

Ayrıca, Apple App Store'da özellik karar verir durumda uygulama da çekici, yüksek çözünürlüklü resmi kullanılabilir olmalıdır. Daha fazla bilgi için lütfen Apple'nın bkz [iTunes Bağlan Geliştirici Kılavuzu](https://developer.apple.com/support/itunes-connect/).

## <a name="managing-agreements-tax-and-banking"></a>Yönetme anlaşmaları, vergi ve bankacılık

**Anlaşmaları, vergi ve bankacılık** iTunes Bağlan bölümünde, iTunes Geliştirici ödemeler ilgili finansal gerekli bilgileri sağlayın ve withholdings vergi ve elinizde ile yerinde anlaşmaları durumunu izlemek için kullanılır Apple. App Store'dan iOS uygulama (ücretsiz veya satışı) yayınlamadan önce uygun anlaşmaları var ve varolan anlaşmaları yapılan tüm değişiklikler anlaştığınız gerekir.

[![](itunesconnect-images/agreement01.png "Yönetme anlaşmaları, vergi ve bankacılık")](itunesconnect-images/agreement01.png#lightbox)

Buradan şunları yapabilirsiniz:

- Sağlayan bir **Team Aracısı** ve diğer kullanıcı rolleri, iTunes Bağlan hesap için aşağıdaki gibi tanımlayın **yönetici** veya **Finans**.
- Kaydetmek ve bakımını **sözleşmeleri** App Store uygulamaları dağıtmak bir kuruluş izin verin.
- Belirtin **tüzel** bilgi, bankacılık sözleşmeleri eşleşmiyor ve kuruluşunuzla ilişkili bilgileri vergi için kullanılan ad (satıcı adı uygulama mağazasında).
- Sağlamak **bankacılık** ve **vergi** uygulamaları App Store aracılığıyla satmak kullanacaksanız bilgi.

Yeniden, bu bilgileri _gerekir_ , yerinde ve güncel bir iOS önce uygulama gönderilebilir iTunes Bağlan gözden geçirme ve sürüm için düzgün ayarlanmamış. Daha fazla bilgi için lütfen Apple'nın bkz [anlaşmalarını yönetme, vergi ve bankacılık](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/ManagingContractsandBanking.html#//apple_ref/doc/uid/TP40011225-CH21-SW1) belgeleri.

<a name="creating" />

## <a name="creating-an-itunes-connect-record"></a>Bir iTunes Connect kaydı oluşturma

Uygulama iTunes Bağlan App Store aracılığıyla dağıtmak için yüklenebilen bir Xamarin.iOS önce uygulama için bir kayıt iTunes Bağlan oluşturmanız gerekir. Bu kayıt (olarak birden çok dilde gerektiği gibi) uygulama mağazasında görünür ve tüm bilgileri uygulama dağıtım sürecinde yönetmek için gereken uygulama ile ilgili tüm bilgileri içerir. Ayrıca, uygulama ağ veya Game Center IAD gibi uygulama mağazası teknolojileri yapılandırmak için kullanılır.

Bir iOS uygulamasına iTunes olması gerekir Bağlan eklemek için **Team Aracısı** veya bir kullanıcıyla bir **yönetici** veya **teknik** rol.

Uygulamasında aşağıdakilerin [iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Tıklayın **uygulamalarım**:

    [![](itunesconnect-images/add01.png "Uygulamalarım üzerinde tıklatın")](itunesconnect-images/add01.png#lightbox)
2. Tıklatın **+** el sol alt köşesinde ve select üst **yeni iOS uygulaması**:

    [![](itunesconnect-images/add02.png "Yeni bir iOS uygulaması ekleme")](itunesconnect-images/add02.png#lightbox)
3. iTunes Bağlan görüntüler **yeni iOS uygulaması** iletişim:

    [![](itunesconnect-images/add03.png "Yeni iOS uygulaması iletişim kutusu")](itunesconnect-images/add03.png#lightbox)
4. Girin bir **adı** ve **sürüm numarası** uygulamanın uygulama Mağazası'nda görüntülenen.
5. Seçin **birincil dili**.
6. Girin bir **SKU** sayı, bu benzersiz bir sabit, kullanılan uygulama izleme olacaktır tanımlayıcısı. Bu son kullanıcı ile bu görüntülenmeyecek _olamaz_ uygulama oluşturulduktan sonra değiştirilmiş olabilir.
7. Seçin **paket kimliği** uygulama sağladığında Geliştirici Merkezi'nde oluşturduğunuz uygulama için. Bu aynı paket kimliği, Mac veya Visual Studio için Visual Studio dağıtım paketine oturum açarken kullanılacak gerekir. Daha fazla bilgi için lütfen bkz bizim [dağıtım profili oluşturma](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) ve [bir Xamarin.iOS projesi dağıtım profil seçme](~/ios/get-started/installation/device-provisioning/index.md) belgeleri.
8. Tıklatın **oluşturma** Bağlan kayıt uygulama için yeni iTunes oluşturmak için düğmesi.

Yeni uygulama iTunes Bağlan oluşturulur ve açıklama, fiyatlandırma, kategoriler, derecelendirme, vb. gibi gerekli bilgileri doldurmak hazır olacaktır.:

[![](itunesconnect-images/add04.png "Yeni uygulama iTunes Bağlan oluşturulur.")](itunesconnect-images/add04.png#lightbox)

<a name="managing" />

## <a name="managing-app-videos-and-screenshots"></a>Uygulama videolar ve ekran görüntüleri yönetme

Uygulama mağazasında bulunan iOS uygulamanız başarıyla pazarlama için en önemli unsurlardan birisi kümesidir harika ekran görüntüleri ve isteğe bağlı olarak, video önizlemeleri sağlanır. Uygulamanızı çalıştıran gerçek görünümlerini kullanın, kullanıcı etkileşimi vurgulayın ve benzersiz özelliklerini sergiler. Uygulama video önizlemesi kullanıcıların ne bu uygulamayı kullanmak için benzer bir fikir vermek için kullanın.

Apple aşağıdaki ekran görüntüleri duruma getirirken önerir:

- En iyi sunum uygulamanız tarafından desteklenen iOS cihazlarda, ekran iyileştirmek ve içeriği okunaklı olduğundan emin olun.
- Çerçeve ekran görüntüsünde bir iOS aygıtı görüntü yok.
- Tam ekran, grafik veya ekran Kenarlıklar olmadan kullanarak uygulamanızı gerçek görünümünü gösterir.
- Durum çubuğu ekran görüntüleri her zaman kaldırmak için bu alanı hariç boyutlarının ekran görüntüleri iTunes Bağlan bekliyor.
- Mümkün olduğunda, ekran görüntüleri gerçek, yüksek çözünürlüklü Retina iOS donanım (değil, iOS simülatörü) alırsınız.
- İPhone üzerinde uygulama Mağazası'nda arama sonucunda ilk ekran görüntülenir ve iPad hiçbir uygulama video önizlemesi kullanılabiliyorsa, bu nedenle yerleştirin en iyi ekran görüntüsü ilk.
- Tüm beş ekran görüntüleri çekici kolaylaştırır dakika vurgulama sırasında uygulamaya öyküsü bildirmek için kullanın.

Apple ekran görüntüleri gerektirir ve videolar her ekran boyutu ve uygulamanızı destekleyen çözünürlükte sağlanmalıdır. Ayrıca, desteklenen yönler göre yatay ve dikey sürümleri sağlanmalıdır.

Aşağıdaki ekran boyutlarına ve çözümlemeleri şu anda gereklidir:

[!include[](~/ios/includes/table-itunesconnect.md)]

### <a name="editing-screenshots-in-itunes-connect"></a>İTunes Bağlan ekran görüntüleri düzenleme

Uygulamasında aşağıdakilerin [iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Tıklayın **uygulamalarım**.
2. ' I tıklatın, uygulamanızın üzerinde **simgesi**.
3. Seçin **sürümleri** sekmesi.
4. Kaydırma **ekran görüntüleri** bölümü.
5. Seçin **görüntü boyutu** ve gerekli görüntüler (en fazla 5 ekran boyutu başına) sürükleyin:

    [![](itunesconnect-images/screenshot01.png "Görüntü boyutu seçin ve gerekli görüntüleri sürükleme")](itunesconnect-images/screenshot01.png#lightbox)
6. Tüm gerekli ekran boyutları için yineleyin.
7. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için ekranın üstündeki düğmesi.

> [!NOTE]
> Not: ekran görüntüleri veya uygulama Video Önizlemesi, uygulamanızda geçerli işlevi eşleşmiyorsa Apple gönderiminiz reddeder.

<a name="metadata" />

## <a name="managing-name-description-whats-new-keywords-and-urls"></a>Ad, açıklama, yenilikler, yönetme anahtar sözcükleri ve URL'leri

İTunes'un bu bölümünde bağlanmak uygulama kaydı yaptığı, yeni sürümler, arama ve IAD destek ve destekleyici URL'leri kullanılan anahtar sözcükleri yapılan tüm değişiklikler bu uygulamayla ilgili yerelleştirilmiş bilgileri sağlar.

### <a name="app-name"></a>Uygulama adı

Uygulamanızın ne yaptığını gösteren bir açıklayıcı uygulama adı seçin. Kısa ve mümkün olduğunca kısa olarak tutmaya çalışın. Uygulamanızın adı nasıl kullanıcıları aramak ve bulması kritik rol oynar, böylece basit ve anımsaması kolay adı koruyabilir. Adı bir iOS cihazında (iPad, iPhone ve iPod touch) görüntülendiğinde görünme için belirli dikkat edin.

Apple, bir uygulamanın adını seçerken aşağıdaki yönergeleri önerir:

- Kısa, basit ve anımsaması kolay ad.
- Telif hakkı veya ticari bir 3. taraf ihlal değil emin olun.
- Uygulamanın işlevselliğini eşleşen kolaylaştırır.
- Yerelleştirilmiş adlar yabancı pazarda için uygun olan yerlerde sağlayın.

### <a name="description"></a>Açıklama

Uygulama ve onun özelliklerini NET, kısa ve açıklayıcı bir açıklama yazın. İlk birkaç satırı en önemli olan ve size harika bir ilk izlenim ve kullanıcı çizmek için bir fırsat sunar. Ne uygulamanızı özel yapar ve benzer, diğer uygulamalardan ayıran açıklanmaktadır.

Apple uygulamanın açıklama yazmak için aşağıdaki önerir:

- Kısa açılış paragrafı veya iki ve kısa madde işaretli ana özelliklerin listesini içerir.
- Yerelleştirilmiş açıklamaları yabancı pazarda için uygun olan yerlerde sağlar.
- Kullanıcı incelemeleri, accolades ya da yalnızca sonunda referansları varsa içerir.
- Satır sonları ve madde işaretlerini okunabilirliği geliştirmek için kullanın.
- Nasıl uygulama açıklaması açıklamanızı en önemli cümlelerde kolaylıkla görünür olduğundan emin olmak için her cihaz türü üzerinde uygulama mağazası görüntüler dikkat edin.

### <a name="whats-new"></a>Yenilikler

Uygulamanızın, yeni bir sürümünü karşıya yüklenirken bir **bu sürümdeki yenilikler** alan kullanılabilir, tamamlanması kapsamlı ve bağlarla.

Apple aşağıdaki yönergeleri neleri yeni bilgileri içerir doldururken önerir:

- Kullanıcıları güncelleştirmeye teşvik Mesajlaşma ekleyin.
- Önem sırasına öğeleri listelemek ve değişiklikleri ve hata düzeltmeleri sabitleme.
- Teknik terimler yerine düz ve gerçek dil değişiklikleri sunar.

### <a name="keywords"></a>Anahtar Sözcükler

Uygulamanın işlevselliğini ilgili Düşünceli ve stratejik anahtar sözcükler, uygulamanızın üzerinde uygulama mağazası ararken kolayca bulabilmeniz yardımcı olur. Ayrıca, uygulamanızı IAD reklamları hizmet veriyorsa, uygulama ağ IAD anahtar sözcükleri uygulamanızda hedeflemek için reklam seçerken kullanır.

Apple anahtar sözcükleri seçerken aşağıdaki önerir:

- Rakip uygulama adları, şirket veya ürün adları veya ticari marka olarak kaydettirilmiş adları kullanmayın.
- Genel sözcükleri veya koşulları kullanmayın.
- Uygun veya uygunsuz koşulları veya ünlülerle adları gibi ilgisiz sözcükleri kaçının.
- Uygun olduğunda yabancı pazarda için anahtar sözcükleri yerelleştirme.

### <a name="urls"></a>URL'ler

Apple Geliştirici herhangi bir sorunu veya bir kullanıcı uygulama hakkındaki olabilir soruları desteklemek için Web sitesi için bir bağlantı sağlayın gerektirir. Bunlar ayrıca (hangi yeniden, Web sitenizde barındırılması gerekir) uygulamanın gizlilik ilkesi için bir bağlantı gerektirir.

İsteğe bağlı olarak, uygulama Mağazası'nda sağlanan daha uygulama hakkında daha fazla bilgi sağlamak için kullanılan Web sitenizde barındırılan pazarlama bilgilere bağlantı sağlayabilir.

### <a name="editing-name-description-whats-new-keywords-and-urls-in-itunes-connect"></a>Ad, açıklama, düzenleme yeni, anahtar sözcükleri ve URL'leri iTunes Connect nedir

Uygulamasında aşağıdakilerin [iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Tıklayın **uygulamalarım**.
2. ' I tıklatın, uygulamanızın üzerinde **simgesi**.
3. Seçin **sürümleri** sekmesi.
4. Kaydırma **adı** bölümü.
5. Gerekli tüm bilgileri doldurun:

    [![](itunesconnect-images/name01.png "Ad, açıklama, iTunes Bağlan yeni, anahtar sözcükleri ve URL'leri nedir düzenleme")](itunesconnect-images/name01.png#lightbox)
6. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için ekranın üstündeki düğmesi.

> [!IMPORTANT]
> Not: ad, açıklama, yeni, anahtar sözcükleri veya URL'leri nedir uygulamanızda geçerli işlevi eşleşmiyorsa Apple gönderiminiz reddeder.

<a name="general" />

## <a name="maintaining-general-app-information"></a>Genel Uygulama bilgi koruma

Bu bölümde (Apple tarafından atanan gibi) bağlanma uygulama kaydı uygulama benzersiz Kimliğini sağlar iTunes, uygulama ait olduğunu kategorileri, derecelendirme, telif hakkı ve uygulamanızı yayınlama şirket hakkında bilgiler.

### <a name="app-icon"></a>Uygulama simgesi

> [!IMPORTANT]
>  Uygulama simgeleri artık iTunes bağlantı gönderilir. Aracılığıyla gönderilmelidir **AppIcon** görüntü projenizin kümesinde **Assets.xcassets** dosya. Daha fazla bilgi için bkz: [uygulama mağazası simgesini](~/ios/app-fundamentals/images-icons/app-store-icon.md) Kılavuzu.

Yüz etkileyici olması gerekir böylece kullanıcılar ve görüntü iyi küçük bir boyutta, uygulamanızın uygulama simgedir. Etkileyici simgeler temiz, basit ve hemen tanımlanabilir.

Apple uygulamaları simge tasarlarken aşağıdaki yönergeleri önerir:

- Simge, uygulamanız için uygun olun.
- Uygulamanızı tasarım ile tutarlı olan basit bir simge oluşturun.
- Sözcükler, simgesini kullanmaktan kaçının.
- Genel olarak düşünün: bir tek uygulama simgesi tüm deposu bölgeler kullanılır.

1024 x 1024 piksel görüntü uygulama Mağazası'nda görüntülenen uygulama simgesi için gereklidir.

Apple'nın daha fazla bilgi için bkz: [iOS İnsan Arabirimi yönergelerine](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html#//apple_ref/doc/uid/TP40006556) ve büyük uygulama simgesi bölümünde açıklamasını [genel uygulama bilgileri](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Appendices/Properties.html#//apple_ref/doc/uid/TP40011225-CH26-SW7) belgeleri

### <a name="app-id"></a>Uygulama Kimliği

Bu iTunes bağlanmak kayıt oluşturulduğunda uygulamanıza Apple tarafından atanmış benzersiz bir kimlik numarasıdır. Çeşitli web tabanlı arabirimler, Apple çağırma sağlıyorsa, siteniz App Store bilgiler dahil olmak üzere, bu numarayı kullanabilirsiniz.

### <a name="version-number"></a>Sürüm numarası

Bu geçerli, etkin, uygulamanızın uygulama Mağazası'nda kullanıcıya gösterilen sürümüdür.

### <a name="category-and-sub-category"></a>Kategori ve alt kategori

Bir önemli bulunabilirliği uygulamanız için App Store'da görünür kategori yönüdür. Kategoriler, kullanıcıların uygulamaları toplulukta göz atıp ilginizi olanları bulmak sağlar. iTunes Bağlan uygulamanızı tanımlarken, en fazla iki farklı kategorileri atamanıza olanak tanır. En iyi uygulama ana işlevi açıklayan kategorileri dikkatli seçtiğinizden emin olun.

### <a name="ratings"></a>Derecelendirme

Tüm uygulamalar, uygulama Mağazası'nda bir derecelendirme sağlamak için gereklidir. Bu derecelendirme ebeveyn denetimi bildirin ve uygulamanın türüne ve içerik göre alt erişimi sınırlamak için kullanılır. Uygulamanızı tanımlarken iTunes Bağlan içerik açıklamaları için içeriği ne sıklıkta görünür uygulamanızda tanımlamak listesini sağlar. Bu seçimleri, uygulama Mağazası'nda görüntülenen derecelendirme uygulamasına dönüştürülür.

Çocuklar için uygulamaları oluştururken, uygulama mağazası alt 11 eski ve altında özel kategori vardır. Uygulamanızı özellikle çocukların hedeflenen değil olsa bile, uygun içerik derecelendirmeleri sağlayarak iyi seçimler yapmanıza, müşterilere yardımcı olun.

> [!IMPORTANT]
> Not: Apple müstehcen, pornografik, rahatsız edici veya iftira niteliğinde bulduğu uygulama gönderimi reddeder.

### <a name="copyright-and-company-information"></a>Telif hakkı ve şirket bilgileri

Apple uygulamanız için telif hakkı bilgileri vermeniz izin vermek ve uygulama (Kore dili uygulama mağazasında yayımlanan uygulamalar için gerekli olan) adresi ve ilgili kişi bilgilerini gibi serbest şirket hakkında bilgi gerektirir. Bu bilgiler, gerektiği gibi uygulama Mağazası'nda görüntülenir.

### <a name="editing-general-app-information-in-itunes-connect"></a>Genel uygulama bilgilerini iTunes Bağlan düzenleme

Uygulamasında aşağıdakilerin [iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Tıklayın **uygulamalarım**.
2. ' I tıklatın, uygulamanızın üzerinde **simgesi**.
3. Seçin **sürümleri** sekmesi.
4. Kaydırma **genel uygulama bilgileri** bölümü.
5. Gerekli tüm bilgileri doldurun:

    [![](itunesconnect-images/general01.png "Genel uygulama bilgilerini iTunes Bağlan düzenleme")](itunesconnect-images/general01.png#lightbox)
6. Tıklayın **Düzenle** tarafından düğmesini **derecelendirme** derecelendirme bilgilerini ayarlamak için:

    [![](itunesconnect-images/general02.png "Derecelendirme düzenleme")](itunesconnect-images/general02.png#lightbox)
6. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için ekranın üstündeki düğmesi.

> [!NOTE]
> Not: kategorileri veya derecelendirmeleri, uygulamanızda geçerli işlevi eşleşmiyorsa Apple gönderiminiz reddeder.

<a name="game-center" />

## <a name="maintaining-game-center-information"></a>Oyun merkezi bilgi koruma

Apple'nın oyun Merkezi, öncü panoları ve kullanıcı tarafından kullanılabilen oyun başarılar gibi bilgileri sağlayabilir ve uygulama çok oyunculu uyumlu olup olmadığını destekleyen iOS oyun uygulamaları için ağ bağlantısı üzerinden.

### <a name="editing-game-center-information-in-itunes-connect"></a>Oyun merkezi bilgileri iTunes Bağlan düzenleme

Uygulamasında aşağıdakilerin [iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Tıklayın **uygulamalarım**.
2. ' I tıklatın, uygulamanızın üzerinde **simgesi**.
3. Seçin **sürümleri** sekmesi.
4. Kaydırma **Game Center** bölümü.
5. Anahtar tarafından çevir **Game Center** için bölüm **üzerinde** konumu.
5. Gerekli tüm bilgileri doldurun:

    [![](itunesconnect-images/gamecenter01.png "Oyun merkezi bilgileri iTunes Bağlan düzenleme")](itunesconnect-images/gamecenter01.png#lightbox)
6. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için ekranın üstündeki düğmesi.

Kullanım **Game Center** Game Center etkinleştirmek ve herhangi bir kullanılabilir korumak için sekme **liderlik** veya **başarılar** bu uygulama için:

[![](itunesconnect-images/gamecenter02.png "Game Center etkinleştir")](itunesconnect-images/gamecenter02.png#lightbox)

[![](itunesconnect-images/gamecenter03.png "Bu uygulama için tüm kullanılabilir liderlik veya başarılar koru")](itunesconnect-images/gamecenter03.png#lightbox)

## <a name="maintaining-app-review-information"></a>Uygulama gözden geçirme bilgi koruma

(Teknik herhangi bir sorunuz varsa), uygulama kişi bilgilerini gibi gözden Apple personel, gerekli olabilecek herhangi bir tanıtım hesabı ve tester yardımcı olabilecek notları gerekli bilgileri sağlamak için bu bölümü kullanın başarılı bir şekilde uygulamanızı gözden geçirin.

### <a name="editing-app-review-information-in-itunes-connect"></a>Uygulama bilgileri gözden geçir iTunes Bağlan düzenleme

Uygulamasında aşağıdakilerin [iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Tıklayın **uygulamalarım**.
2. ' I tıklatın, uygulamanızın üzerinde **simgesi**.
3. Seçin **sürümleri** sekmesi.
4. Kaydırma **uygulama gözden geçirme bilgi** bölümü.
5. Gerekli tüm bilgileri doldurun:

    [![](itunesconnect-images/review01.png "Uygulama bilgileri gözden geçir iTunes Bağlan düzenleme")](itunesconnect-images/review01.png#lightbox)
6. Başarılı bir şekilde gözden sonra uygulama mağazasında yayımlanacak uygulama nasıl istediğiniz seçin:

    [![](itunesconnect-images/review02.png "İTunes Bağlan sürüm bilgilerini düzenleme")](itunesconnect-images/review02.png#lightbox)
6. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için ekranın üstündeki düğmesi.


## <a name="maintaining-pricing-information"></a>Fiyatlandırma bilgileri koruma

Uygulamanız için satış yayınlamayı planlıyoruz, Apple'nın mevcut fiyatlandırma katmanlarına ve verilen fiyatlandırma etkinleşen tarih birini seçerek satış fiyatı ayarlamanız gerekir. Örneğin, bu yazma süresi itibariyle **Katman 1** fiyatlandırma aşağıdaki gibi görünür:

[![](itunesconnect-images/price01.png "Fiyatlandırma bilgileri koruma")](itunesconnect-images/price01.png#lightbox)

### <a name="educational-discount"></a>Eğitim indirim

Uygulamanız birden çok kopya aynı anda satın aldığınızda eğitim kurumları için indirimle sunulacak istiyorsanız bu kutuyu işaretleyin. İndirim ayrıntılarını en son bulunan **Ücretli uygulama anlaşma**, önce bu uygulamayı oturum olacak eğitim müşteriler için kullanılabilir.

### <a name="custom-business-to-business-application"></a>Özel işletmeler için uygulama

Olarak ayarlanmış bir uygulama bir **özel işletmeler için uygulama** yalnızca kullanılabilir **toplu satın alma programı** iTunes Bağlan belirtin ve yalnızca olacaktır müşteriler Geçerli bölgeler (örneğin, ABD kullanılabilir Toplu satın alma programı müşterilerine ABD kullanmanız gerekir Uygulama mağazası toplu satın alma programı iş için).

Özel işletmeler için uygulamalar eğitim kurumları ya da genel uygulama mağazası müşterileri için kullanılabilir olmaz. Daha fazla bilgi edinmek için *uygulama mağazası toplu satın alma programı iş için*, Apple'nın ziyaret [ilgili sık sorulan sorular](http://vpp.itunes.apple.com/faq) sayfası. Müşterileriniz için ne kaydolabilirsiniz hakkında daha fazla bilgi edinmek için **toplu satın alma programı**, Apple'nın ziyaret [dağıtım programları](http://enroll.vpp.itunes.apple.com) sayfası.

### <a name="editing-pricing-information-in-itunes-connect"></a>Fiyatlandırma bilgileri iTunes Bağlan düzenleme

Uygulamasında aşağıdakilerin [iTunes Bağlan](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa):

1. Tıklayın **uygulamalarım**.
2. ' I tıklatın, uygulamanızın üzerinde **simgesi**.
3. Seçin **fiyatlandırma** sekmesi:

    [![](itunesconnect-images/price02.png "Fiyatlandırma bilgileri iTunes Bağlan düzenleme")](itunesconnect-images/price02.png#lightbox)
4. Seçin bir **kullanılabilirlik tarihi**.
5. İstenen fiyatından seçin **fiyat katmanı** açılır liste.
5. İsteğe bağlı olarak etkinleştirme **eğitim indirimleri kapsayan**.
6. İsteğe bağlı olarak uygulamayı tanımlayan bir **özel işletmeler için uygulama**.
6. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için düğmesi.

<a name="iap" />

## <a name="maintaining-in-app-purchase-information"></a>Uygulama içi satın alma bilgileri koruma

Sanal satış planlıyorsanız, uygulama içi ürünlerinizi (örneğin, yeni oyun düzeyleri veya uygulama özellikleri) uygulamanızdan bu bölümü oluşturmak ve sürdürmek için kullanacağınız öğeleri olanlar satın alın.

[![](itunesconnect-images/inapp01.png "Uygulama içi satın alma bilgileri koruma")](itunesconnect-images/inapp01.png#lightbox)

Xamarin.iOS uygulamasının uygulama içi satın almalara ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [uygulama içi satın alma](~/ios/platform/in-app-purchasing/index.md) belgeleri.

## <a name="viewing-application-reviews"></a>Uygulama incelemeler görüntüleme

Uygulamanızın uygulama mağazasında yayımlanan sonra satın alma veya uygulama için ücretsiz olarak karşıdan kullanıcılar uygulama incelenmesi yazma ve yıldız derecelendirmeleri bırakın. Bu incelemeler görmek için bu bölümü kullanın. Örneğin:

[![](itunesconnect-images/reviews01.png "Uygulama incelemeler görüntüleme")](itunesconnect-images/reviews01.png#lightbox)

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.iOS uygulaması sürüm uygulama mağazası için hazırlamak üzere iTunes Bağlan kullanmayı ve tüm depo uygulamanızda hakkında görüntülenen bilgileri korumak nasıl açıklanmaktadır.

## <a name="related-links"></a>İlgili bağlantılar

- [Görüntülerle Çalışma](~/ios/app-fundamentals/images-icons/index.md)
- [iOS uygulama geliştirme iş akışı Kılavuzu: uygulamaları dağıtma](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [Uygulama mağazası gönderimi ipuçları](https://developer.apple.com/appstore/resources/submission/tips.html)
- [App Store gözden geçirme yönergeleri](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [iTunes Bağlan Geliştirici Kılavuzu](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/About.html#//apple_ref/doc/uid/TP40011225-CH1-SW1)
