---
title: "Google play yayımlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 74635b10e97513d6b023cb44ede7745448aa153c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="publishing-to-google-play"></a>Google play yayımlama

Bir uygulama dağıtmak için çok sayıda uygulama pazarda olsa da, Google Play tartışmaya açık bir şekilde Android uygulamaları için dünyanın en büyük ve en sık ziyaret edilen deposunda mevcut. Google Play, dağıtma, reklam, satış ve bir Android uygulamasını satış çözümleme için tek bir platform sağlar.

Bu bölümde yükseltmeniz ve uygulamanıza tanıtma Google Play yardımcı olmak için uygulamanızın Google Play'deki derecelendirme ve filtreleri için kullanma yönergeleri varlıklar toplama bir yayımcı olmasını kaydetme gibi Google Play belirli konuları ele alınacaktır bir uygulamayı belirli cihazlara dağıtımını kısıtlar.

<a name="Requirements"  />

## <a name="requirements"></a>Gereksinimler

Google Play üzerinden bir uygulama dağıtmak için bir geliştirici hesabı oluşturulması gerekir. Bu yalnızca bir kez gerçekleştirilmesi gerekir ve bir tane içeren zaman ücret, $25 ABD Doları.

Tüm uygulamaları 22 Ekim 2033 sonra süresi dolar bir şifreleme anahtarı ile imzalanması gerekir.

Google Play üzerinde yayımlanan bir APK en büyük boyutu 100 MB'tır. Bir uygulama bu boyutu aşarsa, Google Play üzerinden teslim edilecek ek varlıklar sağlayacak *APK genişletme dosyaları*. Android genişletme dosyaları 2 ek dosyaları, bunların her biri 2 GB boyutunda olmasını APK izin verir. Google Play barındırmak ve bu dosyaları herhangi bir ücret ödemeden dağıtın. Genişleme dosya başka bir bölümünde açıklanmıştır.

Google Play genel olarak kullanılabilir değil. Bazı konumları uygulamaları dağıtım için desteklenmiyor olabilir.


<a name="Becoming_a_Publisher"  />

## <a name="becoming-a-publisher"></a>Bir yayımcı olma

Google play uygulamaları yayımlamak için bir yayımcı hesabınız gereklidir. Hesap için bir yayımcı kaydolmak için aşağıdaki adımları izleyin:

1.  Ziyaret [Google Play Geliştirici konsol](https://play.google.com/apps/publish).
1.  Geliştirici kimliğinizi hakkındaki temel bilgileri girin.
1.  Okuma ve bölgeniz için geliştirici dağıtım sözleşmesini kabul edin.
1.  $25 ABD Doları kayıt ücret ödersiniz.
1.  Doğrulama e-posta yoluyla onaylayın.
1.  Hesap oluşturulduktan sonra Google Play kullanan uygulamaları yayımlama mümkündür.


Google Play tüm ülkelerde dünyada desteklemez. En güncel ülkelerin listesi aşağıdaki bağlantılarda bulunabilir:

1.  [Konumlar için geliştirici desteklenen &amp; Satıcı Kayıt](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) &ndash; geliştiriciler burada ve tüccarların kaydetmek Ücretli uygulamaları satmak tüm ülkelerde listesini budur.

1.  [Google Play kullanıcılara dağıtmak üzere konumları desteklenen](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) &ndash; tüm ülkelerde nerede dağıtılmış uygulamalar listesini budur.


<a name="Preparing_Promotional_Assets"  />

### <a name="preparing-promotional-assets"></a>Promosyon varlıklar hazırlama

Etkili bir şekilde yükseltmek ve bir uygulama Google play'de tanıtmak için ekran görüntüleri, grafik ve video gibi tanıtım varlıklar gönderilecek göndermek geliştiricilerin Google sağlar. Google Play tanıtım ve uygulama yükseltmek için bu varlıkları sonra kullanır.


<a name="Launcher_Icons"  />

#### <a name="launcher-icons"></a>Başlatıcı simgeler

A *Başlatıcı simgesini* uygulamanın temsil eden bir grafiktir. Her Başlatıcı simgesini 32-bit PNG saydamlık alfa kanalına sahip olmalıdır. Uygulama simgeleri aşağıdaki listede özetlendiği gibi genelleştirilmiş ekran densities tamamı için sahip olmanız gerekir:

-   **ldpi** (120dpi) &ndash; 36 x 36 px
-   **mdpi** (160dpi) &ndash; 48 x 48 px
-   **hdpi** (240dpi) &ndash; 72 x 72 px
-   **xhdpi** (320dpi) &ndash; 96 x 96 piksel


Başlatıcı simgeler Başlatıcısı simgeleri görsel olarak çekici ve anlamlı yapmak için dikkatli olunması gerekir, böylece kullanıcı uygulamalarının Google play'de görürsünüz ilk noktalardır.

Başlatıcı simgeleri için ipuçları:

1.  **Basit ve KARMAŞASIZ** &ndash; basit ve KARMAŞASIZ Başlatıcısı simgeler'in saklanması. Bu uygulamanın adını simgesinden hariç anlamına gelir. Daha basit simgeleri daha etkileyici olacaktır ve daha küçük boyutlarda ayırt etmek daha kolay olacaktır.

1.  **Simgeler ince olmamalıdır** &ndash; aşırı ince simgeler değil göze iyi tüm arka plan üzerinde.

1.  **Alfa kanal kullanmak** &ndash; simgeleri olmalısınız alfa kanalını kullanın ve tam Çerçeveli görüntüleri olmamalıdır.


<a name="High_Resolution_Application_Icon"  />

#### <a name="high-resolution-application-icons"></a>Yüksek çözünürlüklü uygulama simgeleri

Google Play uygulamaları uygulama simgesi yüksek doğruluk sürümünü gerektirir. Yalnızca Google Play tarafından kullanılır ve uygulama Başlatıcı simgesini değiştirmez. Yüksek çözünürlüklü simgesiyle belirtimleri şunlardır:

1.  32-bit PNG alfa kanalına sahip
1.  512 x 512 piksel
1.  1024KB en büyük boyutu

[Android varlık Studio](https://romannurik.github.io/AndroidAssetStudio/) uygun Başlatıcısı simgeler ve yüksek çözünürlüklü uygulama simgesi oluşturmak için yararlı bir araçtır.


<a name="Screen_shots"  />

#### <a name="screen-shots"></a>Ekran görüntüleri

Google play bir uygulama için iki en az ve en fazla sekiz ekran görüntüleri gerektirir. Google play'de Uygulama Ayrıntıları sayfasında görüntülenir.

Ekran görüntüleri için özellikleri şunlardır:

1.  alfa kanal ile 24 bit PNG ya da JPG
1.  320w x 480h ya da x 800 y 480w veya 480w 854 h x. Landscaped görüntüleri kırpılır.


<a name="Promotional_Graphic" />

#### <a name="promotional-graphic"></a>Promosyon grafiği

Bu, Google Play tarafından kullanılan isteğe bağlı bir görüntüdür:

1.  180w 120 h 24 bit PNG veya JPG alfa kanal x olur.
1.  Resim kenarlık yok.


<a name="Feature_Graphic" />

#### <a name="feature-graphic"></a>Özellik grafiği

Google Play öne çıkan bölümü tarafından kullanılır. Bu grafiği tek başına bir uygulama simgesi görüntülenebilir.

1.  1024w x 500 y PNG veya JPG alfa kanal ve saydamlık yok.
1.  Tüm önemli içeriği 924 x 500 çerçevesinde olmalıdır. Bu çerçeve dışında piksel stil amacıyla kırpılmış olabilir.
1.  Bu grafik ölçeği: grafik basit tutmak ve büyük metin kullanın.


<a name="Video_Link" />

#### <a name="video-link"></a>Video bağlantı

Bu bir uygulama birtakım sergileyen video YouTube'da URL'dir. Video 30 saniye uzunluğu 2 dakika olarak ve en iyi uygulamanızın parçalarını sergiler gerekir.


<a name="pubgp" />

### <a name="publishing-to-google-play"></a>Google play yayımlama

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android 7.0 yayımlama uygulamalar için tümleşik bir iş akışı Google Play Visual Studio'dan sunar. Xamarin Android 7.0'den önceki bir sürümünü kullanıyorsanız, APK Google Play Geliştirici Konsolu aracılığıyla el ile yüklemeniz gerekir. Ayrıca, en az bir APK tümleşik iş akışı kullanabilmek için önceden yüklenmiş olması gerekir. Henüz ilk APK karşıya yüklediğiniz değil, onu el ile yüklemeniz gerekir. Daha fazla bilgi için bkz: [APK'el ile karşıya](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Yeni bir sertifika oluşturma](~/android/deploy-test/signing/index.md#newcert), Android uygulamalarını imzalamak için yeni bir sertifika oluşturmak nasıl açıklanmıştır. Google Play imzalı bir uygulama yayımlamak için sonraki adımdır bakın:

1. Google Play Geliştirici hesabınıza bağlı yeni bir proje oluşturmak için Google Play Geliştirici hesabınızda oturum açın.
2. Oluşturma bir **OAuth istemcisi** uygulamanızın kimliğini doğrulayan.
3. Visual Studio'ya elde edilen istemci Kimliğini ve istemci parolasını girin.
4. Hesabınızı Visual Studio ile kaydedin.
5. Uygulamasını sertifikanızla imzalayın.
6. İmzalı uygulamanızı Google Play yayımlayın.

İçinde [yayımlama arşiv](~/android/deploy-test/release-prep/index.md#archive), **dağıtım kanal** iletişim dağıtım için iki seçenek sunulur: **geçici** ve **Google Play** . Varsa **imzalama kimlik** iletişim bunun yerine görüntülenir, tıklatın **geri** geri dönmek için **dağıtım kanal** iletişim. Seçin **Google Play** tıklatıp **sonraki**:

[ ![Dağıtım Kanalı iletişim](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png)

İçinde **imzalama kimlik** kimliğini oluşturulan iletişim kutusunda [yeni bir sertifika oluşturma](~/android/deploy-test/signing/index.md#newcert) tıklatıp **devam**:

[ ![İmzalama kimlik iletişim kutusu](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png)

İçinde **Google Play hesapları** iletişim kutusunda, tıklatın  **+**  düğmesi yürütmek yeni bir Google hesabı eklemek için:

[ ![Google Play hesapları iletişim kutusu](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png)

İçinde **Google API erişimini kaydetmek** iletişim kutusunda, sağlamalıdır _istemci kimliği_ ve _gizli_ Google Play Geliştirici hesabınızda API erişim sağlar:

[ ![Kaydı Google API erişimi iletişim kutusu](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png)

Sonraki bölümde, yeni bir Google API projesi oluşturun ve gerekli oluşturmak açıklanmaktadır _istemci kimliği_ ve _gizli_.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio yayımlama uygulamaları Google play için tümleşik bir iş akışı vardır. Xamarin Studio 5.9'den önceki bir sürümünü kullanıyorsanız, APK Google Play Geliştirici Konsolu aracılığıyla el ile yükleyin ve ardından kullanmanız gerekir **Google Play Yayımla** iletişim sonraki APK güncelleştirmeler için. Ayrıca, en az bir APK kullanabilmeniz için önce önceden yüklenmiş olmalıdır **Google Play Yayımla**. Henüz ilk APK karşıya yüklediğiniz değil, onu el ile yüklemeniz gerekir. El ile bir APK karşıya yükleme hakkında daha fazla bilgi için bkz: [APK'el ile karşıya](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

[Yeni bir sertifika oluşturma](~/android/deploy-test/signing/index.md#newcert), ele alınan creatin Android uygulamalarını imzalamak için yeni bir sertifika. Google Play bir Xamarin.Android uygulaması yayımlama aşağıdaki adımları özetlemektedir:

1. Google Play Geliştirici hesabınıza bağlı yeni bir proje oluşturmak için Google Play Geliştirici hesabınızda oturum açın.
2. Oluşturma bir _OAuth istemcisi_ uygulamanızın kimliğini doğrulayan.
3. Elde edilen girin _istemci kimliği_ ve _gizli_ Mac için Visual Studio uygulamasına
4. Hesabınızı Mac için Visual Studio ile kaydetme
5. Uygulama sertifikanızla imzalayın.
6. Google play imzalı uygulamanızı yayımlayın.

İçinde [yayımlama arşiv](~/android/deploy-test/release-prep/index.md#archive), **oturum ve Dağıt...**  iletişim dağıtım için iki seçenek sunulur. Seçin **Google Play**tıklatıp **sonraki**:

[ ![Android dağıtım iletişim seçin](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png)

İçinde **Google Play API hesabı** iletişim kutusunda, sağlamalıdır _istemci kimliği_ ve _gizli_ Google Play Geliştirici hesabınızda API erişim sağlar:

[ ![Google Play API hesabı iletişim](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png)

Sonraki bölümde, yeni bir Google API projesi oluşturun ve gerekli oluşturmak açıklanmaktadır _istemci kimliği_ ve _gizli_.

-----


#### <a name="create-a-google-api-project"></a>Bir Google API projesi oluşturma

İlk olarak, oturum açın, [Google Play Geliştirici hesabını](https://play.google.com/apps/publish).
Bir Google Play Geliştirici hesabı zaten yoksa bkz [yayımlama ile çalışmaya başlama](http://developer.android.com/distribute/googleplay/start.html).
Ayrıca, Google Play Geliştirici API [Başlarken](https://developers.google.com/android-publisher/getting_started) Google Play Geliştirici API kullanımı açıklanmaktadır. Google Play Geliştirici konsolunda oturumu sonra tıklatın **ayarları**:

[ ![Ayarlar simgesi](images/01-google-play-developer-console-sml.png)](images/01-google-play-developer-console.png)

İçinde **ayarları** sayfasında, **API erişimini**tıklatıp **yeni proje oluştur** düğmesi:

[ ![Yeni Proje düğmesi oluşturma](images/02-create-new-project-sml.png)](images/02-create-new-project.png)

Bir dakika veya bunu sonra yeni API projesi otomatik olarak oluşturulan ve Google Play Geliştirici konsol hesabınıza bağlanır.

Sonraki adım (bir değil zaten oluşturulduysa) uygulaması için bir OAuth istemci oluşturmaktır. Kullanıcılar, uygulamayı kullanan özel verilerine erişmek istediğinde OAuth istemci kimliği, uygulamanızın kimliğini doğrulamak için kullanılır.
Tıklatın **oluşturma OAuth istemcisi** yeni bir OAuth istemci oluşturmak için:

[ ![OAuth istemcisi düğmesi oluşturma](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png)

Birkaç saniye sonra yeni bir istemci kimliği oluşturulur. Tıklatın **Google geliştiriciler konsol görünümünde** Google Developer's konsolundaki Yeni istemci Kimliğini görmek için:

[ ![Görüntülenen istemci kimliği](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png)

İstemci Kimliğini, adını ve oluşturma tarih görüntülenir. Tıklatın **Düzenle OAuth istemcisi** simgesi uygulamanız için istemci gizli anahtarı görüntülemek için:

[ ![Görünüm uygulama kimlik bilgileri](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png)

OAuth İstemcisi varsayılan adı *Google Play Android Geliştirici*. Bu Xamarin.Android uygulamasının adı veya uygun bir ad değiştirilebilir. Bu örnekte, uygulama adını OAuth istemcisi adı değiştirilene **Uygulamam**:

[ ![İstemci Kimliğini ve parolasını görüntülenir](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png)

Tıklatın **kaydetmek**değişiklikler kaydedilemiyor. Bu döndürür **kimlik bilgileri** tıklayarak kimlik bilgileri karşıdan yükleme konumu sayfasında **karşıdan JSON** simgesi:

[ ![Yükleme JSON simgesi](images/07-download-json-sml.png)](images/07-download-json.png)

Bu JSON dosyası istemci kimliği ve kesin ve yapıştırın gizli içerir **oturum ve Dağıt** sonraki adımda iletişim.


#### <a name="register-google-api-access"></a>Google API erişimini kaydetme

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tamamlamak için istemci kimliği ve istemci parolası kullanın **Google Play API hesabı** Mac için Visual Studio'da iletişim kutusu Hesap bir açıklama vermek mümkündür &ndash; bu birden çok Google Play hesabını kaydetmek ve gelecekteki APK ait farklı Google Play hesaplara yüklemek mümkün kılar. İstemci Kimliğini ve istemci parolası için bu iletişim kutusunu kopyalayın ve tıklatın **kaydetmek**:

[ ![Kaydı Google API erişimi iletişim kutusu](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png)

Bir web tarayıcısı açın ve Google Play Android Geliştirici hesabınızda oturum (, zaten oturum açtınız değil) ister. Oturum açtıktan sonra aşağıdaki komut istemini web tarayıcısında görüntülenir.
Tıklatın **izin** uygulama yetkilendirmek için:

[ ![Uygulama iletişim yetkilendirmek](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png)

#### <a name="publish"></a>Yayımlama

' I tıklattıktan sonra **izin**, tarayıcı raporları _doğrulama kodu alındı. Kapatılıyor..._  ve uygulamayı Visual Studio'da Google Play hesapları listesine eklenir. İçinde **Google Play hesapları** iletişim kutusunda, tıklatın **devam**:

[ ![Hesabı için Google Play Acccounts eklendi](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png)

Ardından, **Google Play izleme** iletişim sunulur. Google Play uygulamanızı karşıya yükleme için dört olası parça sunar:

* **Alpha** &ndash; sınayıcılar küçük bir listesi için uygulamanın çok erken bir sürümünü yüklemek için kullanılır.
* **Beta** &ndash; sınayıcılar daha büyük bir listesine uygulama eski bir sürümü yüklemek için kullanılır.
* **Sunum** &ndash; sağlar; uygulamanın güncelleştirilmiş bir sürümünü almak için kullanıcıların yüzdesi bu yavaş deyin, %10 kullanıcılar artırmak ve hatalar çıkışı Demir çalışırken kullanıcıların % 100 artırmak mümkün kılar.
* **Üretim** &ndash; uygulamayı Google Play Mağazası'ndan tam dağıtım için hazır olduğunda bu izleme seçin.

Hangi Google Play İzleme'ye tıklayın ve uygulama yüklemek için kullanılacak seçin **karşıya**. Seçerseniz **sunum**, bir yüzde değeri girdiğinizden emin olun:

[ ![Alfa, Beta, sunum veya üretim seçin](images/vs/08-google-play-track-sml.png)](images/vs/08-google-play-track.png)

Google Play sınama ve hazırlanan piyasa sürümlerini hakkında daha fazla bilgi için bkz: [alfa/beta testleri oluşturma](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Ardından, bir iletişim kutusu için imzalama sertifikası parola girmesini sunulur.
Parolayı girin ve tıklayın **Tamam**:

[ ![Parola iletişim imzalama](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png)

**Arşiv Yöneticisi** karşıya yükleme ilerlemesini görüntüler:

[ ![APK devam eden karşıya yükleme](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png)

Karşıya yükleme tamamlandığında, tamamlanma durumu sol alt köşesinde Visual Studio'nun gösterilir:

[ ![Yayımlama proje tamamlanmış iletisi](images/vs/11-published-sml.png)](images/vs/11-published.png)


### <a name="troubleshooting"></a>Sorun giderme

Bir APK için Google Play zaten gönderildi gerekir, Not önce depolamak **Google Play Yayımla** çalışır. Bir APK zaten karşıya Yayımlama Sihirbazı şu hatayla görüntüler **hataları** bölmesi:

[ ![Bu uygulama için ilk APK el ile yüklemeniz gerekir](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png)

Olduğunda bu hata occures el ile (örneğin, bir geçici derleme) bir APK Google Play Geliştirici Konsolu aracılığıyla karşıya yükleme ve kullanma **dağıtım kanal** iletişim sonraki APK güncelleştirmeler için.  Daha fazla bilgi için bkz: [APK'el ile karşıya](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md). APK sürüm kodunu, aksi takdirde şu hata meydana gelir her karşıya yükleme ile değiştirmeniz gerekir:

[ ![APK sürüm kod (1) ile zaten güncelleştirildi](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png)

Bu hatayı gidermek için uygulamanın farklı sürüm numarasıyla yeniden ve Google play yeniden gönderin **dağıtım kanal** iletişim.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Tamamlamak için istemci kimliği ve istemci parolası kullanın **Google Play API hesabı** Mac için Visual Studio'da iletişim kutusu Hesap bir açıklama vermek mümkündür &ndash; bu birden çok Google Play hesabını kaydetmek ve gelecekteki APK ait farklı Google Play hesaplara yüklemek mümkün kılar. İstemci Kimliğini ve istemci parolası için bu iletişim kutusunu kopyalayın ve tıklatın **kaydetmek**:

[ ![Erişim iletişim yetkilendirmek](images/xs/10-register-sml.png)](images/xs/10-register.png)

İstemci Kimliğini ve istemci parolası kabul edilirse bir **kaydı başarılı** iletisi görüntülenir. Tıklatın **sonraki**:

[ ![Kayıt başarılı iletisi](images/xs/11-registration-successful-sml.png)](images/xs/11-registration-successful.png)

İçinde **Google Play hesabı** iletişim kutusunda, bir Google hesabı ve uygulama karşıya yükleme için bir izleme seçin:

[ ![Google hesabı iletişim seçin](images/xs/12-choose-google-account-sml.png)](images/xs/12-choose-google-account.png)

Google Play uygulamanızı karşıya yükleme için dört olası parça sunar:

-   **Alpha** &ndash; sınayıcılar küçük bir listesi için uygulamanın çok erken bir sürümünü yüklemek için kullanılır.

-   **Beta** &ndash; sınayıcılar daha büyük bir listesine uygulama eski bir sürümü yüklemek için kullanılır.

-   **Sunum** &ndash; sağlar; uygulamanın güncelleştirilmiş bir sürümünü almak için kullanıcıların yüzdesi bu yavaş deyin, %10 kullanıcılar artırmak ve hatalar çıkışı Demir çalışırken kullanıcıların % 100 artırmak mümkün kılar.

-   **Üretim** &ndash; uygulamayı Google Play Mağazası'ndan tam dağıtım için hazır olduğunda bu izleme seçin.

Google Play sınama ve hazırlanan piyasa sürümlerini hakkında daha fazla bilgi için bkz: [alfa/beta testleri oluşturma](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Ardından, bir imza kimliği, uygulamayı imzalamak için kullanılan seçin.
Seçin **var olan anahtarı kullan** varolan kimlik imzalama kullanmak için Aksi halde Kılavuzu'na danışın [yeni bir sertifika oluşturma](~/android/deploy-test/signing/index.md#newcert) yeni bir anahtar oluşturma hakkında daha fazla bilgi için. Uygulamayı imzalamak için bir sertifika seçtikten sonra tıklatın **sonraki**:

[ ![Android imzalama kimlik iletişim kutusu](images/xs/13-android-signing-identity-sml.png)](images/xs/13-android-signing-identity.png)

Bu noktada uygulama Google Play karşıya yüklenebilir. **Google Play Yayımla** iletişim uygulamanız ile ilgili bilgileri özetler &ndash; tıklatın **Yayımla** Google Play uygulamanızı yayımlamak için:

[ ![Yayımlama için Google Play iletişim kutusu](images/xs/14-publish-to-google-play-sml.png)](images/xs/14-publish-to-google-play.png)

Bir APK için Google Play zaten gönderildi gerekir, Not önce depolamak **Google Play Yayımla** çalışır. Bir APK karşıya aşağıdaki hata oluşabilir:

> _Google Play bu uygulama için ilk APK el ile karşıya yüklemeniz gerekir. Bunu bir geçici APK için kullanabilirsiniz._

veya

> _Hiçbir uygulama için verilen paket adı bulunamadı. [404]_

Bu hatayı gidermek için el ile (örneğin, bir geçici derleme) bir APK Google Play Geliştirici Konsolu aracılığıyla karşıya yükleme ve kullanma **Google Play Yayımla** iletişim sonraki APK güncelleştirmeler için. El ile bir APK karşıya yükleme hakkında daha fazla bilgi için bkz: [APK'el ile karşıya](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

-----
