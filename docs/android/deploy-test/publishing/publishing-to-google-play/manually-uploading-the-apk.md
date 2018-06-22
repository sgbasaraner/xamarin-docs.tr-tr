---
title: APK el ile karşıya yükleme
ms.prod: xamarin
ms.assetid: 1309C251-ABF0-4412-B1F5-200DC8321A9D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 3bfddb315d74e6282004edeb10a35271dce3b9c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771040"
---
# <a name="manually-uploading-the-apk"></a>APK el ile karşıya yükleme


Bir APK gönderildi ilk kez Google Play (veya Xamarin.Android eski bir sürümü kullanılırsa) APK el ile yüklenmelidir [Google Play Geliştirici konsol](https://play.google.com/apps/publish). Bu kılavuz, bu işlem için gereken adımları açıklar. 


## <a name="google-play-developer-console"></a>Google Play Geliştirici konsol

APK derlendikten sonra hazırlanan, tanıtım varlıklar uygulamayı Google Play yüklenmelidir. Bu oturum açmayı gerçekleştirilir [Google Play Geliştirici konsol](https://play.google.com/apps/publish), sonraki resimdeki. Tıklatın **Google play'de Android uygulama yayımlama** düğmesine bir uygulamayı dağıtma işlemi başlatılamadı.

[![Google Play Geliştirici konsol](manually-uploading-the-apk-images/00-google-play-developer-console-sml.png)](manually-uploading-the-apk-images/00-google-play-developer-console.png#lightbox)

Google Play ile kayıtlı mevcut bir uygulamayı zaten varsa, tıklatın **yeni bir uygulama eklemek** düğmesi:

[![Yeni uygulama düğme ekleme](manually-uploading-the-apk-images/01-existing-app-sml.png)](manually-uploading-the-apk-images/01-existing-app.png#lightbox)

Zaman **yeni uygulama Ekle** iletişim kutusu gösterilir, tıklatın ve uygulama adı girin **karşıya APK**:

[![APK düğmesi karşıya yükle](manually-uploading-the-apk-images/02-add-new-application-sml.png)](manually-uploading-the-apk-images/02-add-new-application.png#lightbox)

Sonraki ekranda alfa sınama, beta sınama ve üretim için yayımlanan yazmasına izin verir. Aşağıdaki örnekte, **ALFA testi** sekmesi seçilmiştir. Çünkü **Uygulamam** lisans hizmetlerini kullanmaz **Get lisans anahtarı** düğmesi Bu örnek için tıklattığınız gerekmez. Burada, **ilk APK Alpha için karşıya yükleme** düğmesine tıklandığında alfa kanal yayımlamak için:

[![Alpha düğmesine ilk APK karşıya yükle](manually-uploading-the-apk-images/03-upload-to-alpha-sml.png)](manually-uploading-the-apk-images/03-upload-to-alpha.png#lightbox)

**Karşıya yeni APK için ALFA** iletişim kutusu gösterilir. APK ya da tıklayarak yüklenebilir **gözatma dosyası** düğmesini veya sürükleme ve bırakma APK: 

[![Yeni APK alfa iletişim için karşıya yükleme](manually-uploading-the-apk-images/04-upload-dialog-sml.png)](manually-uploading-the-apk-images/04-upload-dialog.png#lightbox)

Dağıtılacak olan sürüm için hazır APK karşıya emin olun.
Sonraki iletişim APK karşıya yükleme ilerlemesini gösterir:

[![İlerleme göstergesi karşıya yükle](manually-uploading-the-apk-images/05-upload-progress-sml.png)](manually-uploading-the-apk-images/05-upload-progress.png#lightbox)

APK karşıya yüklendikten sonra bir test yöntemi seçmek mümkündür:

[![Test yöntemi iletişim seçin](manually-uploading-the-apk-images/06-select-testing-method-sml.png)](manually-uploading-the-apk-images/06-select-testing-method.png#lightbox)

Google uygulamayı test etme hakkında daha fazla bilgi için bkz: [alfa/beta testleri oluşturma](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en) Kılavuzu.

APK yüklendikten sonra taslak olarak kaydedilir. Daha fazla ayrıntı için Google Play sonraki bölümde açıklandığı gibi sağlanana kadar yayımlanamaz.


## <a name="store-listing"></a>Mağaza listesi

Tıklatın **deposu listeleme** içinde **Google Play Geliştirici konsol** Google Play uygulaması potentials kullanıcılara görüntüler bilgi girmek için: 

[![Mağaza listesi iletişim](manually-uploading-the-apk-images/07-store-listing-sml.png)](manually-uploading-the-apk-images/07-store-listing.png#lightbox)


### <a name="graphics-assets"></a>Grafik varlıklar

Ekranı aşağı kaydırarak **grafik varlıklar** bölümünü **deposu listeleme** sayfa:

[![Grafik varlıklar bölümü](manually-uploading-the-apk-images/08-graphic-assets-sml.png)](manually-uploading-the-apk-images/08-graphic-assets.png#lightbox)

Bu bölümdeki tüm önceden hazırlanmış ve tanıtım varlıkları yüklenir. Hangi tanıtım varlıkları sağlanmalıdır ve hangi biçimi için bunlar içinde sağlanmalıdır yönergeler sağlanır.


### <a name="categorization"></a>Kategorilere ayırma

Sonra **grafik varlıklar** bölüm bir **kategori** bölümünde, kategori ve uygulama türünü seçin:

[![Kategori bölümü](manually-uploading-the-apk-images/09-categorization-sml.png)](manually-uploading-the-apk-images/09-categorization.png#lightbox)

İçerik derecelendirme sonra sonraki bölümde ele alınmıştır.


### <a name="contact-details"></a>İlgili kişi ayrıntıları

Bu sayfanın son bölümü bir **kişi ayrıntıları** bölümü. Bu bölümde, Uygulama geliştirici ilgili kişi bilgileri toplamak için kullanılır:

[![İlgili kişi ayrıntıları bölümü](manually-uploading-the-apk-images/10-contact-details-sml.png)](manually-uploading-the-apk-images/10-contact-details.png#lightbox)

Uygulama gizlilik ilkesi için bir URL sağlamak mümkün **gizlilik ilkesi** bölüm, yukarıda belirtildiği şekilde.


## <a name="content-rating"></a>İçerik derecelendirme

Tıklatın **içerik derecelendirme** içinde **Google Play Geliştirici konsol**. Bu sayfada, uygulamanız için içerik derecelendirme belirtin. Google Play tüm uygulamaların içerik derecelendirme belirtmenizi gerektirir. Tıklatın **devam** düğmesi içerik derecelendirme anketi tamamlamak için:

[![İçerik derecelendirme bölümü](manually-uploading-the-apk-images/11-content-rating-sml.png)](manually-uploading-the-apk-images/11-content-rating.png#lightbox)

Google play'de tüm uygulamaları Google Play derecelendirme sistemi göre derecelendirilmiş olması gerekir. İçerik derecelendirme ek olarak, tüm uygulamaları Google için uymalıdır [Geliştirici içerik İlkesi](http://www.android.com/us/developer-content-policy.html).

Aşağıdaki dört düzeyi Google Play derecelendirme sisteminde listeler ve özellikler veya gerektiren veya derecelendirme düzeyi zorla içerik bazı yönergeler sağlar: 

-   **Herkes** &ndash; erişemeyebilir, yayımlama veya konumu veri paylaşır. Herhangi bir kullanıcı tarafından oluşturulmuş içerik barındıramazsınız. Kullanıcılar arasındaki iletişimin etkinleştirmemeniz. 

-   **Düşük olgunluk** &ndash; erişim ancak paylaşmıyor, konum verileri uygulamalar. Hafif içerikli grafikleri veya çizgi şiddet. 

-   **Orta olgunluk** &ndash; İlaçlar, Alkol ve Tütün başvuruları. Temalar kumar veya benzetimli kumar. Tahrik edici içeriği. Uygunsuz metin ya da kaba mizah. Müstehcen veya cinsel başvurusu. 
    Yoğun fantezi şiddet. Gerçekçi şiddet. Kullanıcıların her diğer bulma olanak tanır. Kullanıcıların birbirleri ile iletişim kurmasına olanak tanır. 
    Bir kullanıcının konumu verilerin paylaşımını. 

-   **Yüksek olgunluk** &ndash; tüketimi veya Alkol, Tütün veya İlaçlar satışı odaklanır. Müstehcen veya cinsel başvuruları odaklanır. Grafik şiddet. 

Orta olgunluk listedeki öğeler öznel, bu nedenle bir orta olgunluk derecelendirme söylemesi görünebilir bir kılavuz yüksek olgunluk derecelendirme garanti etmeye yoğun olabileceğini mümkündür. 


## <a name="pricing-amp-distribution"></a>Fiyatlandırma &amp; dağıtım

Tıklatın **fiyatlandırma ve dağıtım** içinde **Google Play Geliştirici konsol**. Uygulama Ücretli bir uygulama ise bu sayfada bir fiyat ayarlayın.
Alternatif olarak, uygulama dağıtılmış tüm kullanıcılar için ücretsiz olarak. Bir uygulamayı ücretsiz olarak belirlendikten sonra boş kalmalıdır.
Google Play (ancak, bu ücretsiz bir uygulama ile uygulama faturalama içerikle satmak mümkündür) fiyatlı bir uygulama için değiştirilmesi ücretsiz bir uygulama izin vermez. Google Play herhangi bir zamanda ücretsiz bir uygulama olarak değiştirmek Ücretli bir uygulama izin verir.

Ticari bir hesap Ücretli bir uygulamayı yayımlamadan önce için gereklidir. Bunu yapmak için tıklatın **ticari bir hesabı ayarlamanız** ve yönergeleri izleyin.

[![Fiyatlandırma ve dağıtım iletişim](manually-uploading-the-apk-images/12-pricing-sml.png)](manually-uploading-the-apk-images/12-pricing.png#lightbox)


### <a name="manage-countries"></a>Ülkelerde yönetme

Sonraki bölümde **yönetmek ülkelerde**, hangi ülkelerde distibuted için bir uygulama olabilir üzerinde denetim sağlar:

[![Yönetme ülkelerde iletişim kutusu](manually-uploading-the-apk-images/13-manage-countries-sml.png)](manually-uploading-the-apk-images/13-manage-countries.png#lightbox)


### <a name="other-information"></a>Diğer bilgiler

Daha fazla uygulama ads içerip içermediğini belirtmek için ilerleyin. Ayrıca, **CİHAZ KATEGORİLERİ** bölüm Android takmak, Android TV veya Android otomatik için isteğe bağlı olarak uygulama dağıtmak için seçenekleri sağlar:

[![Reklam bölüm içerir.](manually-uploading-the-apk-images/14-contains-ads-sml.png)](manually-uploading-the-apk-images/14-contains-ads.png#lightbox)

Bu bölümde sonra içine kullanmama gibi seçilebileceğini ek seçenekler **aileleri için tasarlanmış** ve eğitim için Google Play üzerinden uygulama dağıtma.


### <a name="consent"></a>Onay

Ekranın alt kısmındaki **fiyatlandırma &amp; dağıtım** sayfası **onayı** bölümü.
Bu zorunlu bir bölümdür ve uygulama karşıladığını bildirmek için kullanılan [Android içerik yönergeleri](http://www.android.com/market/terms/developer-content-policy.html#hl=us) ve ABD ihracat yasalarına tabi uygulamasıdır alındı:

[![Bölüm onayı](manually-uploading-the-apk-images/15-consent-sml.png)](manually-uploading-the-apk-images/15-consent.png#lightbox)

Bu kılavuzda ele daha bir Xamarin.Android uygulaması yayımlama için daha fazla yoktur.
Google Play uygulamanızı yayımlama hakkında daha fazla bilgi için bkz: [Google Play Geliştirici konsol Yardım Merkezi'ne Hoş Geldiniz](https://support.google.com/googleplay/android-developer#topic=3450769).



## <a name="google-play-filters"></a>Google Play filtreleri

Kullanıcılar Google Play Web sitesi uygulamalar için göz attığınızda, tüm yayımlanan uygulamalar arayabilirsiniz. Kullanıcıların Google Play'den Android cihazın göz attığınızda, sonuçları biraz farklıdır. Sonuçları kullanılıyor cihaz uyumluluğunu göre filtrelenir. Uygulamanın kısa mesaj gönderebileceğini gerekir, örneğin, ardından Google Play SMS iletisi gönderilemiyor herhangi bir CİHAZDAN bu uygulamaya göstermez. Bir arama uyguladığınız filtreleri aşağıdakiler arasından oluşturulur:

1.  Cihaz donanım yapılandırması.
2.  Uygulamaları bildirimlerinde bildirim dosyası.
3.  (Varsa) kullanılan taşıyıcı.
4.  Aygıt konumu.

Uygulama nasıl filtre denetlemeye yardımcı olmak üzere uygulamanın bildirimine öğeleri eklemek mümkündür Google Play Mağazası'nda. Aşağıdaki liste öğeleri ve uygulamaları filtrelemede kullanılan öznitelik bildirimi:

-   [destekleyen ekran](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) &ndash; Google Play öznitelikleri uygulamanın ekran boyutuna göre bir cihaza dağıtılabilir varsa belirlemek için kullanın. 
    Google Play'de Android büyük ekranlar, ancak değil tam tersini küçük düzene uyarlayabilirsiniz varsayar. Bu nedenle normal ekranlar için destek bildiren bir uygulama büyük ekranlar, ancak olmayan küçük ekranlar arar görünür. Bir Xamarin.Android uygulaması sağlamaz, bir `<supports-screen>` bildirim dosyası, ardından Google Play öğesinde varsayın tüm öznitelikler varsa true değerini ve uygulamanın tüm ekran boyutlarına destekler. Bu öğe eklenmeli **AndroidManifest.xml** el ile. 

-   [kullandığı yapılandırma](http://developer.android.com/guide/topics/manifest/uses-configuration-element.html) &ndash; bu bildirim öğe türü klavye, gezinti cihazları, dokunmatik ekran, vb. gibi belirli donanım özellikleri istemek için kullanılır. Bu öğe eklenmeli **AndroidManifest.xml** el ile. 

-   [özelliğini kullanan](http://developer.android.com/guide/topics/manifest/uses-feature-element.html) &ndash; bildirim bu öğe bir aygıtı sırada uygulamanın çalışması gereken donanım veya yazılım özellikleri bildirir. Bu öznitelik yalnızca bilgilendirme amaçlıdır. Google Play Bu filtre karşılamayan cihazlar için uygulama görüntülenmez. Diğer yöntemlerle uygulamayı yüklemek hala mümkündür (el ile veya yükleme). Bu öğe eklenmeli **AndroidManifest.xml** el ile. 

-   [kullandığı kitaplığı](http://developer.android.com/guide/topics/manifest/uses-library-element.html) &ndash; bu öğe belirli paylaşılan kitaplıklar, örneğin Google haritalar aygıtta olması gerektiğini belirtir. Bu öğe ile de belirtilebilir `Android.App.UsesLibaryAttribute`. Örneğin: 

    ```csharp
    [assembly: UsesLibrary("com.google.android.maps", true)]
    ```

-   [kullandığı izni](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) &ndash; bu öğe düzgün ile bildirilmemiş uygulamayı çalıştırmak için gerekli olan belirli donanım özellikleri anlamak için kullanılan bir `<uses-feature>` öğesi. Bir uygulama kamera kullanma izni isterse olsa bile Örneğin, ardından Google Play aygıtlarının kamera olmalıdır varsayar hiçbir `<uses-feature>` kamera bildirme öğesi. Bu öğe ile ayarlanabilir `Android.App.UsesPermissionsAttribute`. Örneğin: 

    ```csharp
    [assembly: UsesPermission(Manifest.Permission.Camera)]
    ```

-   [kullanır-sdk](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html) &ndash; öğesi minimum Android API uygulaması için gerekli düzeyi bildirmek için kullanılır. Bu öğe bir Xamarin.Android projesi Xamarin.Android seçeneklerinde ayarlayabilir. 

-   [uyumlu ekranlar](http://developer.android.com/guide/topics/manifest/compatible-screens-element.html) &ndash; bu öğe ekran boyutu ve bu öğesi tarafından belirtilen yoğunluğu eşleşmiyor uygulamaları filtrelemek için kullanılır. Uygulamaların çoğu bu filtre kullanmamanız gerekir. Belirli yüksek performanslı oyunlar veya uygulama dağıtım katı denetimleri gerekli uygulamalar için tasarlanmıştır. `<support-screen>` Yukarıda belirtilen özniteliği, tercih edilen yöntemdir. 

-   [destekler gl doku](http://developer.android.com/guide/topics/manifest/supports-gl-texture-element.html) &ndash; bu öğe GL doku uygulama gerektiriyor sıkıştırma durum oluşumlarıyla bildirmek için kullanılır. Uygulamaların çoğu bu filtre kullanmamanız gerekir. Belirli yüksek performanslı oyunlar veya uygulama dağıtım katı denetimleri gerekli uygulamalar için tasarlanmıştır. 

Android uygulama bildirimi yapılandırma hakkında daha fazla bilgi için bkz: [uygulama bildirimi](https://developer.android.com/guide/topics/manifest/manifest-intro.html) konu.
