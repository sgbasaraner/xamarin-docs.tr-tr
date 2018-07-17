---
title: Google Play'de yayımlama
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3525541ba0795f4e0b174b155c0ca219e3257bac
ms.sourcegitcommit: 6433b424410a850f504e0f934bbb5baf8f093e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067366"
---
# <a name="publishing-to-google-play"></a>Google Play'de yayımlama

Bir uygulamayı dağıtmak için çok sayıda uygulama pazarlara olsa da, Google Play Android uygulamaları için dünyanın en büyük ve en çok ziyaret edilen depoda tartışmaya olduğundan. Google Play, dağıtma, reklam, satış ve satış bir Android uygulamasının çözümleme için tek bir platform sağlar.

Bu bölümde, Google Play'e belirli tanıtın ve uygulamanızı tanıtma Google Play yardımcı olmak için uygulamanızı Google Play'den derecelendirme ve filtreleri için kullanma yönergeleri varlıklar toplama, bir yayımcı olacak kaydetme gibi konuları kapsar bir uygulamayı belirli cihazlara dağıtımını kısıtlar.


## <a name="requirements"></a>Gereksinimler

Google Play üzerinden bir uygulama dağıtmak için bir geliştirici hesabı oluşturulmalıdır. Bu yalnızca bir kez gerçekleştirilmesi gerekir ve bir ilgili zaman ücret 25 ABD Doları.

Tüm uygulamalar, 22 Ekim 2033 yılına sonra süresi dolan bir şifreleme anahtarıyla oturum açmış olmanız gerekir.

Google Play'den yayımlanan bir APK en büyük boyutu 100 MB'dir. Bir uygulama bu boyutu aşarsa, Google Play üzerinden teslim edilecek ek varlıklar sağlayacak *APK genişletme dosyaları*. Android genişletme dosyaları apk'yı 2 GB boyutundaki her biri 2 ek dosyalar için izin verir. Google Play barındırabilir ve hiçbir ücret ödemeden bu dosyaları dağıtabilirsiniz. Genişleme dosya başka bir bölümde açıklanmıştır.

Google Play, küresel olarak kullanılabilir değil. Bazı yerlerde uygulamaların dağıtımı desteklenmiyor olabilir.



## <a name="becoming-a-publisher"></a>Yayımcı olmak

Google play uygulamaları yayımlamak için bir yayımcı hesabı sağlamak gereklidir. Hesap için bir yayımcı kaydolmak için aşağıdaki adımları izleyin:

1.  Ziyaret [Google Play Geliştirici Konsolu'na](https://play.google.com/apps/publish).
1.  Geliştirici kimliğinizi hakkındaki temel bilgileri girin.
1.  Okuma ve bölgeniz için geliştirici dağıtım sözleşmesini kabul edin.
1.  25 ABD Doları kayıt ücret ödersiniz.
1.  Doğrulama e-posta yoluyla onaylayın.
1.  Hesap oluşturulduktan sonra Google Play kullanan uygulamaları yayımlama mümkündür.


Google Play, dünyanın tüm ülkeler desteklemez. Ülkeler en güncel listesi aşağıdaki bağlantılarda bulunabilir:

1.  [Geliştirici için desteklenen konumları &amp; satıcı kaydı](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) &ndash; geliştiriciler burada ve tüccarların kaydetme Ücretli uygulamaları satmak tüm ülkeler listesini budur.

1.  [Google Play kullanıcılara dağıtmak üzere desteklenen konumları](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) &ndash; burada uygulamaları dağıtılabilir tüm ülkeler listesini budur.



### <a name="preparing-promotional-assets"></a>Promosyon varlıklar için hazırlama

Etkili bir şekilde yükseltmek ve uygulamanın Google play'de bildirmek için ekran görüntüleri, grafik ve video gibi promosyon varlıklar gönderilecek göndermek geliştiricilerin Google sağlar. Google Play, ardından uygulamayı yükseltin ve bu varlıkları kullanacaktır.



#### <a name="launcher-icons"></a>Başlatıcı simgeleri

A *Başlatıcısı simgesini* bir uygulamasını temsil eden bir grafiktir. Her Başlatıcısı simgesini için saydam alfa kanalına sahip bir 32-bit PNG olmalıdır. Uygulama simgeleri tüm aşağıda ana hatlarıyla genelleştirilmiş ekran densities sahip olmanız gerekir:

-   **ldpi** (120 DPI) &ndash; 36 x 36 piksel
-   **mdpi** (160 DPI) &ndash; 48 x 48 piksel
-   **hdpı** (240dpi) &ndash; 72 x 72 piksel
-   **xhdpi** (320dpi) &ndash; 96 x 96 piksel


Başlatıcısı, görsel olarak çekici ve anlamlı Başlatıcısı simgeleri yapmak için dikkatli olunması, böylece uygulamaların Google play'de bir kullanıcının göreceği ilk şey simgelerdir.

Başlatıcı simgeleri için ipuçları:

1.  **Basit ve sade** &ndash; Başlatıcısı simgeler saklanır basit ve sade. Bu simge uygulama adını hariç anlamına gelir. Daha basit simgeleri daha etkileyici olur ve daha küçük boyutlarda ayırt etmek daha kolay olacaktır.

1.  **Simgeler ince olmamalıdır** &ndash; aşırı ince simgeleri değil göze de tüm arka planlar üzerinde.

1.  **Alfa kanalını kullanma** &ndash; simgeler olmalısınız alfa kanalını kullanın ve tam Çerçeveli resimler olmamalıdır.



#### <a name="high-resolution-application-icons"></a>Yüksek çözünürlüklü uygulama simgeleri

Google play'de uygulamalar, uygulama simgesi yüksek uygunluğa sahip bir sürümünü gerektirir. Yalnızca Google Play tarafından kullanılır ve uygulama Başlatıcısı simgesini yerini almaz. Yüksek çözünürlüklü simge özellikleri şunlardır:

1.  alfa kanalı ile 32-bit PNG
1.  512 x 512 piksel
1.  En büyük boyutu 1024KB

[Varlık Android Studio](https://romannurik.github.io/AndroidAssetStudio/) uygun Başlatıcısı simgeleri ve yüksek çözünürlüklü uygulamayı oluşturmak için yararlı bir araçtır.



#### <a name="screen-shots"></a>Ekran görüntüleri

Google play, bir uygulama için iki en az ve en fazla sekiz ekran görüntüleri gerektirir. Google play'de Uygulama Ayrıntıları sayfasında görüntülenir.

Ekran görüntüleri için özellikleri şunlardır:

1.  alfa kanalına ile 24 bit PNG veya JPG
1.  320w x 480h veya 480w x 800 y veya 480w 854 h x. Landscaped görüntüleri kırpılır.



#### <a name="promotional-graphic"></a>Promosyon grafiği

Google Play tarafından kullanılan isteğe bağlı görüntü budur:

1.  Bu, bir 180w 120 h 24 bit PNG veya JPG alfa kanalına x olur.
1.  Resim kenarlık yok.



#### <a name="feature-graphic"></a>Özellik grafiği

Öne çıkan Google Play bölümü tarafından kullanılır. Bu grafiği, tek başına bir uygulama simgesi görüntülenebilir.

1.  1024w x 500 y PNG veya JPG alfa kanalına ve saydamlık yok.
1.  Tüm önemli içeriğin 924 x 500 çerçeve içinde olmalıdır. Bu çerçeve dışında piksel biçimsel amacıyla kırpılmış olabilir.
1.  Bu grafiği ölçeklendirilebilir: grafik basit tutmak ve büyük metin kullanın.



#### <a name="video-link"></a>Video bağlantısı

Bu, bir uygulamayı gösteren video YouTube'da bir URL'dir. Video, 30 saniye ila 2 dakika uzunlukta olması ve en iyi uygulamanızın parçalarını gösterin.



### <a name="publishing-to-google-play"></a>Google Play'de yayımlama

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android 7.0, Google Play'e Visual Studio'dan yayımlama uygulamaları için tümleşik bir iş akışı sunar. Xamarin Android 7.0 öncesi bir sürümü kullanıyorsanız, Google Play Developer Console APK el ile yüklemeniz gerekir. Ayrıca, en az bir APK tümleşik bir iş akışı kullanabilmeniz için önceden yüklenmiş olması gerekir. Henüz ilk APK, karşıya yüklediğiniz değil, bunu el ile yüklemeniz gerekir. Daha fazla bilgi için [APK'el ile karşıya](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Yeni bir sertifika oluşturma](~/android/deploy-test/signing/index.md#newcert), Android uygulamaları imzalamak için yeni bir sertifika oluşturmak adımları açıklanmıştır. Sonraki adım, imzalı bir uygulamanın Google Play'e Yayımla olmaktır:

1. Google Play Geliştirici hesabınıza bağlı yeni bir proje oluşturmak için Google Play geliştiricisi hesabınızda oturum açın.
2. Oluşturma bir **OAuth istemcisi** , uygulamanızın kimliğini doğrulayan.
3. Visual Studio'ya, sonuçta elde edilen istemci Kimliğini ve istemci gizli anahtarını girin.
4. Visual Studio ile hesabınızı kaydedin.
5. Uygulamasını sertifikanızla imzalayın.
6. İmzalı uygulamanızı Google Play'e Yayımla.

İçinde [yayımlama için arşiv](~/android/deploy-test/release-prep/index.md#archive), **dağıtım kanalı** iletişim dağıtım için iki seçenek sunulur: **geçici** ve **Google Play** . Varsa **imzalama kimliği** iletişim bunun yerine görüntülenir, tıklayın **geri** dönmek için **dağıtım kanalı** iletişim. Seçin **Google Play** tıklatıp **sonraki**:

[![Dağıtım Kanalı iletişim](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

İçinde **imzalama kimliği** kimlik oluşturduğunuz iletişim kutusunda [yeni bir sertifika oluşturma](~/android/deploy-test/signing/index.md#newcert) tıklatıp **devam**:

[![İmzalama kimliği iletişim kutusu](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png#lightbox)

İçinde **Google Play hesapları** iletişim kutusunda, tıklayın **+** yeni bir Google Play hesabını eklemek için:

[![Google Play hesapları iletişim kutusu](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png#lightbox)

İçinde **Google API erişimini Kaydet** iletişim kutusunda sağlamalısınız _istemci kimliği_ ve _gizli_ Google Play Geliştirici hesabınıza API erişimi sağlayan:

[![Google API erişimi iletişim kaydetme](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png#lightbox)

Sonraki bölümde, yeni bir Google API projesi oluşturun ve gerekli oluşturmak açıklanmaktadır _istemci kimliği_ ve _gizli_.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio yayımlama uygulamalar Google play için tümleşik bir iş akışı vardır. Xamarin Studio 5.9'den önceki bir sürümünü kullanıyorsanız, Google Play Developer Console APK el ile yükleyin ve ardından kullanmanız gerekir **Google Play'e Yayımla** sonraki APK güncelleştirmeler için iletişim. Ayrıca, en az bir apk'yı kullanabilmeniz için önce önceden yüklenmiş olmalıdır **Google Play'e Yayımla**. Henüz ilk APK, karşıya yüklediğiniz değil, bunu el ile yüklemeniz gerekir. Bir APK el ile karşıya yükleme hakkında daha fazla bilgi için bkz: [APK'el ile karşıya](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Yeni bir sertifika oluşturma](~/android/deploy-test/signing/index.md#newcert)açıklanan Android uygulamaları imzalamak için yeni bir sertifika oluşturuluyor. Aşağıdaki adımlar, Google Play'e bir Xamarin.Android uygulama yayımlama özetlemektedir:

1. Google Play Geliştirici hesabınıza bağlı yeni bir proje oluşturmak için Google Play geliştiricisi hesabınızda oturum açın.
2. Oluşturma bir _OAuth istemcisi_ , uygulamanızın kimliğini doğrulayan.
3. Ortaya çıkan girin _istemci kimliği_ ve _gizli_ Mac için Visual Studio'yla
4. Mac için Visual Studio ile hesabınızı kaydedin
5. Uygulama, sertifika ile oturum açın.
6. Google play imzalı uygulamanızı yayımlayın.

İçinde [yayımlama için arşiv](~/android/deploy-test/release-prep/index.md#archive), **imzala ve Dağıt...**  iletişim dağıtım için iki seçenek sunulur. Seçin **Google Play**tıklatıp **sonraki**:

[![Android dağıtım iletişim seçin](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png#lightbox)

İçinde **Google Play API hesabı** iletişim kutusunda sağlamalısınız _istemci kimliği_ ve _gizli_ Google Play Geliştirici hesabınıza API erişimi sağlayan:

[![Google Play API hesabı iletişim](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png#lightbox)

Sonraki bölümde, yeni bir Google API projesi oluşturun ve gerekli oluşturmak açıklanmaktadır _istemci kimliği_ ve _gizli_.

-----


#### <a name="create-a-google-api-project"></a>Google API projesi oluşturma

İlk olarak, oturum, [Google Play Geliştirici hesabınıza](https://play.google.com/apps/publish).
Google Play Geliştirici hesabınıza zaten yoksa bkz [yayımlama ile çalışmaya başlama](http://developer.android.com/distribute/googleplay/start.html).
Ayrıca, Google Play Geliştirici API [Başlarken](https://developers.google.com/android-publisher/getting_started) Google Play Geliştirici API'SİNİN nasıl kullanılacağı açıklanmaktadır. Google Play Developer Console işaretinden sonra tıklayın **ayarları**:

[![Ayarlar simgesi](images/01-google-play-developer-console-sml.png)](images/01-google-play-developer-console.png#lightbox)

İçinde **ayarları** sayfasında **API erişimi**tıklatıp **yeni proje oluştur** düğmesi:

[![Yeni Proje oluştur düğmesi](images/02-create-new-project-sml.png)](images/02-create-new-project.png#lightbox)

Bir dakika ya da sonrasında, yeni API projesi otomatik olarak oluşturulan ve Google Play Developer Console hesabınıza bağlı.

Sonraki adıma (biri değil zaten oluşturulduysa) uygulaması için bir OAuth istemci oluşturmaktır. Kullanıcıların uygulamanızı kullanarak kendi özel verilere erişmesini istediğinizde, uygulamanızın kimlik doğrulaması için OAuth istemci Kimliğiniz kullanılır.
Tıklayın **OAuth istemcisi oluşturun** yeni bir OAuth istemcisi oluşturmak için:

[![OAuth istemcisi düğme oluşturma](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png#lightbox)

Birkaç saniye sonra yeni bir istemci kimliği üretilir. Tıklayın **Google geliştiriciler konsol görünümünde** yeni istemci Kimliğini Google Geliştirici konsolunda görmek için:

[![Görüntülenen istemci kimliği](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png#lightbox)

İstemci kimliği, adı ve oluşturma tarihi görüntülenir. Tıklayın **Düzenle OAuth istemcisi** simgesi uygulamanız için istemci gizli anahtarı görüntülemek için:

[![Görünüm uygulaması kimlik bilgileri](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png#lightbox)

OAuth İstemcisi varsayılan adıdır *Google Play Android Geliştirici*. Bu, bir Xamarin.Android uygulaması adını veya uygun bir ad değiştirilebilir. Bu örnekte, bir uygulama adı için OAuth istemci adı değiştirilir **MyApp**:

[![İstemci Kimliğini ve parolasını görüntülenen](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png#lightbox)

Tıklayın **Kaydet**değişiklikleri kaydedin. Bu döndürür **kimlik bilgilerini** nerede tıklayarak kimlik bilgilerini indirme sayfasında **indirme JSON** simgesi:

[![Yükleme JSON simgesi](images/07-download-json-sml.png)](images/07-download-json.png#lightbox)

Bu JSON dosyası, istemci Kimliğini ve istemci gizli anahtarı kesmek ve yapıştırmak içerir **imzala ve Dağıt** sonraki adımda iletişim.


#### <a name="register-google-api-access"></a>Google API erişimini Kaydet

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tamamlamak için istemci Kimliğini ve istemci gizli anahtarını kullanın **Google Play API hesabı** Mac için Visual Studio'da iletişim kutusu Hesap açıklamasını vermek mümkündür &ndash; bu birden fazla Google Play hesabını kaydeder ve gelecekteki APK farklı Google Play hesapları için karşıya yükleme mümkün kılar. Bu iletişim için istemci Kimliğini ve istemci gizli anahtarı kopyalayın ve tıklayın **kaydetme**:

[![Google API erişimi iletişim kaydetme](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png#lightbox)

Bir web tarayıcısı açın ve Google Play Android Geliştirici hesabınızda oturum (zaten oturum değil) ister. Oturum açtıktan sonra aşağıdaki istemi web tarayıcısında görüntülenir.
Tıklayın **izin** uygulama yetkilendirmek için:

[![Uygulama iletişim Yetkilendir](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png#lightbox)

#### <a name="publish"></a>Yayımlama

' I tıklattıktan sonra **izin**, tarayıcı raporları _alınan doğrulama kodu. Kapatılıyor..._  ve uygulamayı Visual Studio'da Google Play hesapları listesine eklenir. İçinde **Google Play hesapları** iletişim kutusunda, tıklayın **devam**:

[![Hesap için Google Play Acccounts eklendi](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png#lightbox)

Ardından, **Google Play izleme** iletişim sunulur. Google Play uygulamanızı karşıya yükleme için dört olası parça sunar:

* **Alfa** &ndash; test ediciler, küçük bir listesine uygulamanın çok erken bir sürümünü yüklemek için kullanılır.
* **Beta** &ndash; test daha büyük bir listesine uygulama eski bir sürümünü yüklemek için kullanılır.
* **Piyasaya çıkma** &ndash; sağlar; uygulamanın güncelleştirilmiş bir sürümü alacak kullanıcıların yüzdesi bu yavaş kullanıcıların yüzdesi % %10 varsayalım artırmak ve hataları kullanıma Demir % 100 kullanıcı siz artırmak mümkün kılar.
* **Üretim** &ndash; uygulamanın Google Play Store'dan tam dağıtım için hazır olduğunda, bu izlemeyi seçin.

Hangi Google Play İzleme'ye tıklayın ve uygulama yüklemek için kullanılacak seçin **karşıya**. Seçerseniz **piyasaya çıkma**, bir yüzde değeri girdiğinizden emin olun:

[![Alfa, Beta, ürün veya üretim seçin](images/vs/08-google-play-track-sml.png)](images/vs/08-google-play-track.png#lightbox)

Google Play test ve hazırlanmış piyasaya çıkarma hakkında daha fazla bilgi için bkz. [alfa/beta testleri ayarlayın](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Ardından, bir iletişim kutusu, imzalama sertifikası için parola girmesini sunulur.
Parolayı girin ve tıklatın **Tamam**:

[![İmzalama parolası iletişim kutusu](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png#lightbox)

**Arşiv Yöneticisi** karşıya yükleme ilerlemesini görüntüler:

[![APK ilerleme karşıya yükleme](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png#lightbox)

Karşıya yükleme tamamlandığında, sol alt köşede Visual Studio'nun tamamlanma durumu gösterilmektedir:

[![Proje Yayımlama tamamlandı iletisi](images/vs/11-published-sml.png)](images/vs/11-published.png#lightbox)


### <a name="troubleshooting"></a>Sorun giderme

Bir APK Google Play'e zaten gönderildi gerekir, Not önce depolamak **Google Play'e Yayımla** çalışır. Bir APK zaten karşıya Yayımlama Sihirbazı şu hatayla görüntüler **hataları** bölmesi:

[![Bu uygulama için ilk APK el ile yüklemeniz gerekir](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png#lightbox)

Olduğunda bu hata occures el ile Google Play Developer Console (örneğin, bir geçici derleme) bir APK yükleyin ve kullanın **dağıtım kanalı** sonraki APK güncelleştirmeler için iletişim.  Daha fazla bilgi için [APK'el ile karşıya](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md). Apk'yı sürüm kodunu, aksi takdirde şu hata ortaya çıkar her karşıya yükleme ile değiştirmeniz gerekir:

[![Sürüm kodlu (1) bir APK zaten güncelleştirildi](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png#lightbox)

Bu hatayı gidermek için uygulamayı farklı bir sürüm numarası ile yeniden oluşturmanız ve Google play yeniden **dağıtım kanalı** iletişim.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Tamamlamak için istemci Kimliğini ve istemci gizli anahtarını kullanın **Google Play API hesabı** Mac için Visual Studio'da iletişim kutusu Hesap açıklamasını vermek mümkündür &ndash; bu birden fazla Google Play hesabını kaydeder ve gelecekteki APK farklı Google Play hesapları için karşıya yükleme mümkün kılar. Bu iletişim için istemci Kimliğini ve istemci gizli anahtarı kopyalayın ve tıklayın **kaydetme**:

[![Erişim iletişim Yetkilendir](images/xs/10-register-sml.png)](images/xs/10-register.png#lightbox)

İstemci Kimliğini ve istemci gizli anahtarı kabul edilirse bir **kaydı başarılı** iletisi görüntülenir. Tıklayın **sonraki**:

[![Kayıt başarılı iletisi](images/xs/11-registration-successful-sml.png)](images/xs/11-registration-successful.png#lightbox)

İçinde **Google Play hesabını** iletişim kutusunda, bir Google hesabı ve uygulamanın yükleme için bir izleme seçin:

[![Google hesabı iletişim seçin](images/xs/12-choose-google-account-sml.png)](images/xs/12-choose-google-account.png#lightbox)

Google Play uygulamanızı karşıya yükleme için dört olası parça sunar:

-   **Alfa** &ndash; test ediciler, küçük bir listesine uygulamanın çok erken bir sürümünü yüklemek için kullanılır.

-   **Beta** &ndash; test daha büyük bir listesine uygulama eski bir sürümünü yüklemek için kullanılır.

-   **Piyasaya çıkma** &ndash; sağlar; uygulamanın güncelleştirilmiş bir sürümü alacak kullanıcıların yüzdesi bu yavaş kullanıcıların yüzdesi % %10 varsayalım artırmak ve hataları kullanıma Demir % 100 kullanıcı siz artırmak mümkün kılar.

-   **Üretim** &ndash; uygulamanın Google Play Store'dan tam dağıtım için hazır olduğunda, bu izlemeyi seçin.

Google Play test ve hazırlanmış piyasaya çıkarma hakkında daha fazla bilgi için bkz. [alfa/beta testleri ayarlayın](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Ardından, imzalama kimliği, uygulamayı imzalamak için kullanılan seçin.
Seçin **var olan bir anahtar kullanmak** mevcut bir imzalama kimliği kullanmak için Aksi takdirde kılavuzuna başvurmalısınız [yeni bir sertifika oluşturma](~/android/deploy-test/signing/index.md#newcert) yeni bir anahtar oluşturma hakkında daha fazla bilgi için. Uygulamayı imzalamak için bir sertifika seçtikten sonra tıklayın **sonraki**:

[![Android imzalama kimliği iletişim kutusu](images/xs/13-android-signing-identity-sml.png)](images/xs/13-android-signing-identity.png#lightbox)

Bu noktada uygulamayı Google Play'e karşıya yüklenebilir. **Google Play'e Yayımla** iletişim uygulamanızla ilgili bilgileri özetler &ndash; tıklayın **Yayımla** Google Play'e uygulamanızı yayımlamak için:

[![Yayımlama için Google Play iletişim kutusu](images/xs/14-publish-to-google-play-sml.png)](images/xs/14-publish-to-google-play.png#lightbox)

Bir APK Google Play'e zaten gönderildi gerekir, Not önce depolamak **Google Play'e Yayımla** çalışır. Bir APK karşıya şu hata ortaya çıkabilir:

> _Google Play, el ile bu uygulama için ilk APK karşıya gerektirir. Bunun için geçici bir APK kullanabilirsiniz._

veya

> _Uygulama için belirli bir paket adı bulunamadı. [404]_

Bu hatayı gidermek için el ile Google Play Developer Console (örneğin, bir geçici derleme) bir APK yükleme ve kullanma **Google Play'e Yayımla** sonraki APK güncelleştirmeler için iletişim. Bir APK el ile karşıya yükleme hakkında daha fazla bilgi için bkz: [APK'el ile karşıya](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

-----
