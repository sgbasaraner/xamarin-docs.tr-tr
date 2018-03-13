---
title: "Uygulama içi satın alma temelleri ve yapılandırma"
ms.topic: article
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 302bb1225067ad401f97ee6bad88b4cd16c6dc95
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="in-app-purchase-basics-and-configuration"></a>Uygulama içi satın alma temelleri ve yapılandırma

Uygulama içi satın almalara uygulama cihazda StoreKit API faydalanmak için uygulama gerektirir. StoreKit ürün bilgilerini almak ve işlemleri gerçekleştirmek için Apple'nın iTunes sunucularıyla tüm iletişimi yönetir. Sağlama profili, uygulama içi satın alma için yapılandırılmalıdır ve ürün bilgileri iTunes Bağlan girilmesi gerekir.

 [![](in-app-purchase-basics-and-configuration-images/image1.png "Bu grafikte gösterildiği gibi Apple'nın tüm iletişim StoreKit yönetir")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

Uygulama içi satın alma sağlamak için uygulama mağazası kullanarak aşağıdaki Kurulum ve yapılandırma gerektirir:

-  **iTunes Bağlan** – satmak ürünleri yapılandırma ve satın alma test etmek için korumalı alan kullanıcı hesaplarını ayarlama. Bunlar sizin adınıza toplanan fon havalesi şekilde de bankacılık ve vergi bilgilerinizi Apple'a sağladığınız gerekir.
-   **iOS sağlama portalı** – bir paket tanımlayıcısı oluşturma ve uygulamanız için App Store erişimi etkinleştirme.
-  **Kit depolamak** – ürünleri görüntüleme, ürün satın alma ve işlemleri geri yükleme için uygulamanız için kod ekleme.
-  **Özel kod** – müşteriler tarafından gerçekleştirilen satın alma işlemleri izlemek ve ürünleri veya hizmetleri satın aldığınız sağlamak için. Ayrıca (örneğin, books ve dergi sorunları) sunucusundan indirilen içerik ürünlerinizi oluşur, okundu bilgilerini doğrulamak için bir sunucu tarafı işlem uygulayın.


İki depolama Seti "sunucu ortamları" vardır:

-  **Üretim** – gerçek para işlemleri. Yalnızca gönderildi ve Apple tarafından onaylanan uygulamalar yoluyla erişilebilir. Uygulama içi satın alma ürünleri gerekir de gözden ve üretim ortamına kullanılabilir olmadan önce onaylandı.
-  **Korumalı alan** – Burada, test olur. Ürünü burada hemen (yalnızca geçerlidir üretim ortamına onay işlemi) oluşturulduktan sonra kullanılabilir. Korumalı alan işlemlerinde işlemleri gerçekleştirmek test kullanıcıları (değil gerçek Apple kimliklerini) gerektirir.

## <a name="in-app-purchase-rules"></a>Uygulama içi satın alma kuralları

Uygulamanızı içinde dijital ürünler veya hizmetler için ödeme başka biçimlerde kabul ya da bunları belirttiğinizden veya kullanıcılarınızın bunları içinde bir uygulamayı başvurmak olamaz. Başka bir deyişle, uygulama içi satın alma en uygun ödeme mekanizması kullanıldığında kredi kartı veya PayPal kabul edemez. Uygulama dışında dijital ürünleri satın almak için özel bir durum yoktur ancak için özel "login" ile ilişkili books bir Web sitesinde satın alma ve uygulama "login" kullanıcı erişimine izin verdiğini kullanarak gibi uygulama, satın alınan books kullanın.
Bu şekilde çalışan uygulamalara belirttiğinizden veya dış satın alma özelliği bağlamak için izin verilmiyor – geliştiriciler (belki de aracılığıyla e-posta pazarlama veya başka bir doğrudan kanala) diğer yollarla, kullanıcılar için bu özelliği iletişim kurması gerekir.

Kullanamazsınız beri izin verilen kullanım bakımından, bir alternatif ödeme mekanizması (ör. ancak, uygulama fiziksel mal satın alma Kredi kartı, PayPal) gelen uygulama içinde.

Apple üzerinde satış – ad, açıklama gider ve bir ekran görüntüsünü 'ürün' gözden geçirilmesi için gerekli önce her ürün onaylamanız gerekir. Ürün gözden geçirme kez uygulama incelemeleri ile aynıdır.

Tüm fiyat ürününüz için seçemezsiniz – yalnızca 'her ülke/Apple destekleyen para birimi belirli bir değere sahip bir fiyat Katmanı' seçebilirsiniz. Farklı fiyat katmanı farklı pazarlarda sahip olamaz.

## <a name="configuration"></a>Yapılandırma

Herhangi bir uygulama içi satın alma kod yazmadan önce bazı iTunes Connect Kurulum işlerinde yapmanız gerekir ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) ve iOS sağlama portalı ( [developer.apple.com/iOS](http://developer.apple.com/iOS)).

Bu üç adımı herhangi bir kod yazmadan önce tamamlanması:

-  **Apple Geliştirici hesabı** – Apple bankacılık ve vergi bilgi gönderin.
-  **iOS sağlama portalı** – uygulamanızı geçerli bir uygulama kimliği olduğundan emin olun (bir joker karakter olmayan bir yıldız işareti * da) ve uygulama satın alma işleminde etkinleştirdi.
-  **iTunes Bağlan Uygulama Yönetimi** – ürünleri uygulamanıza ekleyin.


### <a name="apple-developer-account"></a>Apple Geliştirici hesabı

Oluşturma ve ücretsiz uygulama dağıtmaya gerektiren çok az yapılandırmada [iTunes Bağlan](https://itunesconnect.apple.com), ücretli satmak uygulamaları veya uygulama içi satın almalara gerektirir ancak, Apple bankacılık ve vergi bilgileri sağlar. Tıklayın **anlaşmaları, vergi ve bankacılık** burada gösterilen ana menüden:

 [![](in-app-purchase-basics-and-configuration-images/image2.png "Anlaşmaları, vergi ve ana menüden bankacılık tıklayın")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

Geliştirici hesabınız olmalıdır bir **iOS Ücretli uygulamaları** gerçekte, bu ekran görüntüsünde gösterildiği gibi Sözleşme:

 [![](in-app-purchase-basics-and-configuration-images/image3.png "Ücretli uygulamaları yürürlükte sözleşme iOS Geliştirici hesabınızın olması gerekir")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

Sahip olduğunuz kadar tüm StoreKit işlevselliğini test etmek mümkün olmayacak bir **iOS Ücretli uygulamaları** sözleşme – kodunuzu StoreKit çağrılarında Apple işleyene kadar başarısız, **sözleşmeler, vergi ve bankacılık** bilgi.

### <a name="ios-provisioning-portal"></a>iOS sağlama portalı

Yeni uygulamalar ayarlanır **uygulama kimlikleri** bölümünü **iOS sağlama portalı**. Yeni bir uygulama kimliği oluşturmak için şu adrese gidin [iOS sağlama portalı üye merkezine](https://developer.apple.com/membercenter/index.action), gitmek **sertifikalar, tanımlayıcılarını ve profilleri** bölüm ve Portal'ı tıklatın **tanımlayıcıları** altında *iOS uygulamaları*. Ardından yeni bir uygulama kimliği oluşturmak için "+" üstte sağ tıklayın


Formun yeni oluşturmak için **uygulama kimlikleri**

 şuna benzer:

 [![](in-app-purchase-basics-and-configuration-images/image4.png "Yeni uygulama kimlikleri oluşturma formu")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

Uygun bir şey girin *açıklama*, bu uygulama Kimliğini listesini kolayca tanıyacak şekilde. İçin *uygulama kimliği öneki*, takım kimliği seçin

#### <a name="bundle-identifierapp-id-suffix-format"></a>Paket tanımlayıcısı/uygulama kimliği soneki biçimi

İstediğiniz için herhangi bir dize kullanabilirsiniz, **paket tanımlayıcısı** (ad hesabınıza benzersiz olduğu sürece), ancak, geriye doğru DNS biçiminde izleyin yerine, isteğe bağlı bir dize kullanmak Apple önerir. My_store_example gibi bir tanımlayıcı (Apple öneririz değil olsa bile) kullanmak için eşit oranda geçerli ancak bu makale eşlik örnek uygulama com.xamarin.storekit.testing paket tanımlayıcısı için kullanır.

> [!IMPORTANT]
> **Not**: Apple de sonuna eklenecek joker yıldız sağlar bir **paket tanımlayıcı** böylece tek bir uygulama kimliği birden çok uygulama için ancak kullanılabilir _joker uygulama kimlikleri için kullanılamaz AppPurchase_. Joker karakteriyle paket tanımlayıcısı com.xamarin.* olabilir örneği

#### <a name="enabling-app-services"></a>Uygulama Hizmetleri etkinleştirme

Unutmayın **uygulama içi satın alma** Hizmetler listesinde otomatik olarak etkinleştirilir:

 [![](in-app-purchase-basics-and-configuration-images/image5.png "Uygulama içi satın alma Hizmetler listesinde otomatik olarak etkinleştirilir")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>Sağlama profilleri

Normalde seçme gibi uygulama içi satın alma için ayarladığınız uygulama kimliği geliştirme ve üretim sağlama profilleri oluşturun. Başvurmak [iOS cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) ve [uygulama mağazasında yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) kılavuzları daha fazla bilgi için.

## <a name="itunes-connect"></a>iTunes Connect

Tıklatın **My uygulamaları** iTunes oluşturmak veya bir iOS uygulaması girişi düzenlemek için Bağlan içinde. Uygulama genel bakış sayfası aşağıda gösterilmiştir:

 [![](in-app-purchase-basics-and-configuration-images/image6.png "Uygulama genel bakış sayfası")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

Tıklatın **uygulama içi satın almalara** oluşturmak veya ürünlerinizin satış düzenlemek için. Bu ekran örnek uygulama zaten eklenmiş çeşitli ürünlerle gösterir:

 [![](in-app-purchase-basics-and-configuration-images/image7.png "Zaten eklenmiş çeşitli ürünlerle örnek uygulaması")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

Yeni ürün ekleme işlemi iki adımı vardır:

1.   Ürün türünü seçin: [ ![ ] (in-app-purchase-basics-and-configuration-images/image8.png "ürün türünü seçin")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2.   Ürün Kimliği de dahil olmak üzere, fiyatlandırma katmanı ve yerelleştirilmiş açıklamaları ürünün öznitelikleri girin: [ ![ ] (in-app-purchase-basics-and-configuration-images/image9.png "ürünleri öznitelikleri girme")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

Her uygulama içi satın alma ürün için gerekli alanlar, aşağıda açıklanmıştır:


### <a name="reference-name"></a>Başvuru adı

Başvuru adı kullanıcılarınıza görüntülenmez; iç kullanım içindir ve yalnızca iTunes Bağlan görüntülenir.

### <a name="product-id-format"></a>Ürün Kimliği biçimi

Bir ürün tanımlayıcısı yalnızca alfasayısal (A-Z, a-z, 0-9), içerebilir alt çizgi (_) ve nokta (.) karakter. Tanımlayıcılar için herhangi bir dize kullanabilmenize karşın, Apple geriye doğru DNS biçiminde önerir. Örneğin, örnek uygulama bu paket tanımlayıcısı kullanır:

 `com.xamarin.storekit.testing`

Bu nedenle uygulama içi satın alma ürünleri tanımlamak için kural aşağıdaki gibi olur:

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

Bu adlandırma kuralını zorlanmaz, ürünlerinizi yönetmenize yardımcı olmak amacıyla bir öneri yeterlidir. Ayrıca, aynı geriye doğru DNS kuralını aşağıdaki rağmen ürün tanımlayıcılardır *ilgili olmayan* olan ve paket tanımlayıcısı için gerekli değil aynı dizesiyle başlatmak için. Hala photo_product_greyscale gibi tanımlayıcıları (Apple öneririz değil olsa bile) kullanmak için geçerli olacaktır.

Ürün Kimliği kullanıcılarınıza görüntülenmez, ancak uygulama kodunuzda ürün başvurmak için kullanılan.

### <a name="product-type"></a>Ürün türü

Uygulama içi satın alma ürün, sunabileceğiniz beş türü vardır:

1.  **Kaynaklarda** – olan şeyleri 'kullanılan Yukarı', player harcayabilir oyun currency gibi. Kullanıcı yedekleme/geri yükleme mu veya aksi halde cihazlarını yeniledi, kaynaklarda bir işlem de (hangi etkili bir şekilde player baştan aynı avantajı verirsiniz) geri. Uygulama kodu işlem tamamlandıktan hemen sonra 'tüketilebilir öğesi' verdiğinizden emin olması gerekir.
1.  **Olmayan tüketilebilir** – bir kez, bir dijital dergi sorunu veya bir oyun düzeyi gibi satın alınan kullanıcı 'sahibi' ürünler.
1.  **Otomatik yenilenebilir abonelikleri** – yalnızca gerçek dergi abonelik gibi abonelik dönemin sonunda Apple otomatik olarak müşteri yeniden ücretler ve abonelik süresi süresiz olarak veya müşteri kadar açıkça genişletir Bunu iptal eder. Bu Newsstand uygulamalar için tercih edilen ödeme yöntemidir (aslında, uygulamaları Newsstand dağıtım için onaylanmış olması için bu ödeme yöntemini desteklemesi gerekir).
1.  **Ücretsiz abonelik** – yalnızca Newsstand etkin uygulamalarda sunulan ve müşteri aboneliğe içeriğe erişmek için kendi tüm aygıtlarda izin verir. Ücretsiz abonelik süresi dolmayacak.
1.  **Abonelik olmayan yenileme** – bir fotoğraf Arşiv bir ayın erişim gibi statik içerik süre sınırlı erişim satmak için kullanılmalıdır.


 *Bu belge şu anda yalnızca ilk iki ürün türleri (tüketilebilir ve olmayan tüketilebilir) kapsar.*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>Fiyat katmanları

Uygulama mağazası ürünleriniz için rasgele bir fiyat seçmenize izin vermez – Apple aralarından seçim yapabileceğiniz sabit fiyat katmanları sağlar. Her para birimi sabit fiyatları ve Apple (örneğin, sonra belirli bir para ve ABD Doları arasındaki göreli döviz kuru sürekli değişikliği) göreli fiyatlarını ayarlamak hakkını saklı tutar.

Apple istediğiniz para birimi/fiyat için doğru katmanı seçmenize yardımcı olmak için bir fiyat matris sağlar. Fiyat matrisi (Ağustos 2012) bir alıntı burada gösterilir:

 [![](in-app-purchase-basics-and-configuration-images/image10.png "Bir alıntı fiyat matrisi Ağustos 2012")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

(Haziran 2013) yazma sırasında ABD Doları gelen 87 katmanları vardır ABD Doları 999.99 0.99. Fiyatlandırma matris fiyat, müşterileriniz faydalı olur ve ayrıca Apple'dan – alacak bu daha az kendi % 30 ücret miktardır ve ayrıca tüm yerel vergiler (ABD ve Kanada satıcıları için 99 c p 70 c aldığınız örnekte bildirim toplamak için gereklidirler gösterir. Avustralya satıcılar yalnızca 63 c nedeniyle alırken eğiştir ' mal &amp; Hizmetleri vergi ' satış fiyatını levied).

Fiyatlandırma ürününüzün herhangi bir zamanda gelecekteki bir tarihte etkili zamanlanmış fiyat değişiklikleri de dahil olmak üzere, güncelleştirilebilir. Bu ekran görüntüsü, gelecekteki tarihli fiyat değişikliği nasıl eklenir gösterir – fiyat geçici olarak katman 1 Katman 3 Eylül ayında yalnızca değiştiriliyor:

 [![](in-app-purchase-basics-and-configuration-images/image11.png "Burada fiyat geçici olarak katman 1 Katman 3 Eylül ayında yalnızca değiştiriliyor gelecekteki tarihli fiyat değişikliği")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>Ücretsiz ürünler desteklenmiyor

Apple Newsstand uygulamalar için özel bir ücretsiz abonelik seçeneğine sağlanan olsa da, başka bir uygulama içi satın alma türleri için sıfır (ücretsiz) fiyatı belirlenen mümkün değil. Düzenleyebileceğiniz sırada (IE. alt) Fiyatlar satış yükseltmeler için uygulama içi satın almalara 'Bağlan iTunes ücretsiz' yapılamıyor.

### <a name="localization"></a>Yerelleştirme

İTunes Bağlan desteklenen dillerin herhangi bir sayı için farklı bir ad ve Açıklama metin girebilirsiniz. Her bir dilin eklenen/popup düzenlenebilir içinde olabilir:

 [![](in-app-purchase-basics-and-configuration-images/image12.png "Her bir dilin eklenen/popup düzenlenebilir içinde olabilir")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 Uygulamanızda ürün bilgilerini görüntülediğinizde, yerelleştirilmiş metin StoreKit görüntülemek için kullanılabilir. Para birimi görüntülemeyi biçimlendirme bu belgenin sonraki bölümlerinde ele alınmaktadır doğru simge ve ondalık biçimlendirme – göstermek için ayrıca yerelleştirilmiş olmalıdır.

### <a name="app-store-review"></a>App Store gözden geçirme

Uygulamaları – aynı her ürünün Apple'nın üzerinde satış gitmek için izin verilen önce gözden geçirilir. Ürünler için ad veya açıklama uygunsuz içeriği reddedilmiş olabilir veya Apple yanlış ürün türü (ör. seçtiğiniz karar verebilirsiniz bir kitap veya dergi sorunu oluşturduğunuz, ancak tüketilebilir ürün türü kullanılır). Ürün incelemeleri, bir uygulama gözden geçirme uzun sürebilir.

Uygulama ile satın alma (yeni bir uygulama olan veya mevcut bir işlevsellik eklenmiş olup olmadığını) etkin bir uygulama gönderilen ilk kez ile göndermek için bazı ürünler seçmeniz gerekir. İTunes Bağlan portalı, bunu yapmak için bu ekran görüntüsünde gösterildiği gibi ister:

 [![](in-app-purchase-basics-and-configuration-images/image13.png "İTunes Bağlan portal bazı ürünler de göndermek isteyip istemediğinizi sorar")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 (Bu uygulama onaylanan ürünlerden olmadan depoya Git değil! şekilde) hepsi aynı anda onaylanmış uygulama ve uygulama içi satın almalara birlikte incelenecektir.

İlk uygulama içi satın alma yeteneğini sürümünüzle onaylandıktan sonra daha fazla ürün ekleyin ve bunları gözden geçirme için herhangi bir zamanda gönderebilirler. Ayrıca belirli uygulama içi satın alma ürünleri birlikte yeni bir sürüm göndermek kullanarak seçebileceğiniz **sürümü ayrıntıları** istemi da anlaşılacağı gibi sayfa.

Başvurmak [App Store gözden geçirme yönergeleri](https://developer.apple.com/appstore/guidelines.html) daha fazla bilgi için.

 [Bölüm 2 - deposu Paketi'ne Genel Bakış ve yazdırılacak ürün bilgileri](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
