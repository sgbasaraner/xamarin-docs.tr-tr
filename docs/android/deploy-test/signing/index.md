---
title: Android uygulama paketi imzalama
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/26/2018
ms.openlocfilehash: 20a28d475e58a58a98abe21203e9841b7824fe48
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="signing-the-android-application-package"></a>Android uygulama paketi imzalama

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu bölümde, Visual Studio tarafından sağlanan APK imzalamak için tümleşik yayımlama iş akışı açıklanır. İçinde [sürüm için bir uygulama hazırlama](~/android/deploy-test/release-prep/index.md) **arşiv Yöneticisi** uygulamanızı oluşturmak ve imzalamak ve yayımlamak için bir arşivde yerleştirmek için kullanılır. Bu bölümde kimlik imzalama Android oluşturmak, Android uygulamaları için yeni bir imzalama sertifikası oluşturma ve arşivlenmiş uygulama yayımlama açıklanmaktadır *geçici* diske.
Sonuçta elde edilen APK bir uygulama mağazasına giderek olmadan Android cihazları içine dışarıdan yüklenebilir.

İçinde [yayımlama arşiv](~/android/deploy-test/release-prep/index.md#archive), **dağıtım kanal** iletişim dağıtım için iki seçenek sunulur. Seçin **geçici**:

[ ![Dağıtım Kanalı iletişim](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu bölümde, APK imzalamak için Visual Studio Mac'ın tümleşik yayımlama iş akışı için kullanacağız. İçinde [sürüm için bir uygulama hazırlama](~/android/deploy-test/release-prep/index.md), kullandık **arşiv Yöneticisi** uygulamanızı oluşturmak ve imzalamak ve yayımlamak için bir arşiv yerleştirin. Bu bölümde, biz kimlik imzalama Android oluşturmak, Android uygulamaları için yeni bir imzalama sertifikası oluşturma ve arşivlenmiş uygulama yayımlama öğreneceksiniz *geçici* diske. Sonuçta elde edilen APK bir uygulama mağazasına giderek olmadan Android cihazları içine dışarıdan yüklenebilir.

İçinde [yayımlama arşiv](~/android/deploy-test/release-prep/index.md#archive), **oturum ve Dağıt...**  iletişim dağıtım için iki seçenek bize sunulur. Seçin **geçici** tıklatıp **sonraki**:

[ ![Oturum ve Dağıt iletişim kutusu](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png)

-----

<a name="newcertvs" />
<a name="newcert" />
<a name="newcertxs" />

## <a name="create-a-new-certificate"></a>Yeni bir sertifika oluşturun

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sonra **geçici** seçildiğinde, Visual Studio açılır olan **imzalama kimlik** sonraki ekran görüntüsünde gösterildiği gibi iletişim sayfası. Yayımlanacak. APK, (sertifikası olarak da bilinir) bir imzalama anahtarı ile ilk imzalanmalıdır.

Varolan bir sertifikayı tıklayarak kullanılabilir **alma** düğmesine ve ardından devam etmek [APK oturum](#signapkvs). Aksi takdirde ' ı  **+**  düğmesi yeni bir sertifika oluşturmak için:

[ ![Geçici imzalama kimliği](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png)

**Oluşturma Android anahtar deposu** iletişim kutusu gösterilir; Android uygulamaları imzalamak için kullanabileceğiniz yeni bir imzalama sertifikası oluşturmak için bu iletişim kutusunu kullanın. Bu iletişim kutusunda gösterildiği gibi (kırmızı renkle) gerekli bilgileri girin:

[ ![Android anahtar Deposu iletişim kutusunda Oluştur](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png)

Aşağıdaki örnek sağlanmalıdır bilgi türünü gösterir. Tıklatın **oluşturma** yeni bir sertifika oluşturmak için:

[ ![Yeni bir sertifika oluşturma](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png)

Sonuçta elde edilen anahtar şu konumda bulunur:

**C:\\kullanıcılar\\*kullanıcıadı*\\AppData\\yerel\\Xamarin\\Android Mono\\diğer\\alias.keystore**

Örneğin, yukarıdaki adımları aşağıdaki konumda yeni bir imzalama anahtarı oluşturabilirsiniz:

**C:\\kullanıcılar\\*kullanıcıadı*\\AppData\\yerel\\Xamarin\\Android Mono\\chimp\\chimp.keystore**

> [!NOTE]
> **Not:** sonuçta elde edilen anahtar dosyasını güvenli bir yerde yedeklemek mutlaka &ndash; çözümde bulunmaz. Kaybetmeniz durumunda, bir anahtar dosya (örneğin, başka bir bilgisayara taşınmış veya Windows'u yeniden nedeniyle), uygulamanızı önceki sürümler ile aynı sertifika ile oturum kuramayacaktır.

Anahtar deposunun hakkında daha fazla bilgi için bkz: [Keystore'nın MD5 veya SHA1 imza bulma](~/android/deploy-test/signing/keystore-signature.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

' I tıklattıktan sonra **geçici**, Visual Studio için Mac açılır **Android imzalama kimlik** sonraki ekran görüntüsünde gösterildiği gibi iletişim. Yayımlanacak. APK, ilk olmalıdır (sertifikası olarak da bilinir) bir imzalama anahtarı ile imzalanmış. Bir sertifika zaten varsa, tıklatın **mevcut bir anahtarı içe** alıp için devam düğmesine [APK oturum](#signapkxs) yoksa,'i tıklatın **yeni bir anahtar oluşturun** düğmesi Yeni bir sertifika oluşturun: 

[ ![Android imzalama kimlik iletişim kutusu](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png)

**Yeni sertifika oluştur** iletişim Android uygulamaları imzalamak için kullanılan yeni bir imzalama sertifikası oluşturmak için kullanılır. Tıklatın **Tamam** gerekli bilgileri girdikten sonra:

[ ![Yeni sertifika iletişim kutusu oluşturma](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png)

Sonuçta elde edilen anahtar şu konumda bulunur:

**~/Library/Developer/Xamarin/Keystore/alias/alias.keystore**

Örneğin, yukarıdaki adımları aşağıdaki konumda yeni bir imzalama anahtarı oluşturabilirsiniz:

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**


> [!NOTE]
> **Not:** sonuçta elde edilen anahtar dosyasını güvenli bir yerde yedeklemek mutlaka &ndash; çözümde bulunmaz. Kaybetmeniz durumunda, bir anahtar dosya (örneğin, başka bir bilgisayara taşınmış veya Mac yeniden nedeniyle), uygulamanızı önceki sürümler ile aynı sertifika ile oturum kuramayacaktır.

Anahtar deposunun hakkında daha fazla bilgi için bkz: [Keystore'nın MD5 veya SHA1 imza bulma](~/android/deploy-test/signing/keystore-signature.md).

-----

<a name="signapkvs" />
<a name="signingxs" />

## <a name="sign-the-apk"></a>APK oturum

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zaman **oluşturma** tıklandığında, yeni bir anahtar (içeren yeni bir sertifika) depo kaydedilir ve altında listelenen **imzalama kimlik** sonraki ekran görüntüsünde gösterildiği gibi. Google play'de bir uygulamayı yayımlamak için tıklatın **iptal** ve Git [Google Play yayımlama](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Yayımlamak için *geçici*, tıklatıp imzalama için kullanan imza kimliği seçin **Kaydet** bağımsız dağıtım için uygulama yayımlamak için. Örneğin, **chimp** (daha önce oluşturduğunuz) kimlik imzalama bu ekran görüntüsünde seçili:

[ ![İmzalama kimlik örneği](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png)

Ardından, **arşiv Yöneticisi** yayımlama ilerleme durumunu görüntüler. Yayımlama işlemi tamamlandığında **Kaydet** iletişim kutusunu açar için bir konum sormak için burada oluşturulan. APK depolanması için dosyadır:

[ ![Farklı Kaydet iletişim kutusu](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png)

İstediğiniz konuma gidin ve tıklayın **kaydetmek**. Anahtar parolası bilinmiyorsa **imzalama parola** iletişim kutusu, seçili sertifika için parola iste görünecektir:

[ ![Parola iletişim imzalama](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png)

İmzalama işlemi tamamlandıktan sonra **Klasör Aç**:

[ ![Açık klasör düğmesi](images/vs/08-open-folder-vs-sml.png)](images/vs/08-open-folder-vs.png)

Bu, oluşturulan APK dosyasını içeren klasörü açmak için Windows Gezgini'ni neden olur. Bu noktada, Visual Studio dağıtım için hazır bir APK Xamarin.Android uygulamasına derlenmiş.
Aşağıdaki ekran görüntüsünde yayımlama hazır uygulama örneği görüntüler **MyApp.MyApp.apk**:

[ ![Windows Explorer'da gösterilen APK](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Burada görüldüğü gibi yeni bir sertifika anahtar deposuna eklendi. Google play'de bir uygulamayı yayımlamak için tıklatın **iptal** ve Git [Google Play yayımlama](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Aksi takdirde tıklatın **sonraki** uygulamayı yayımlamak için *geçici* (dağıtılmak bağımsız) Bu örnekte gösterildiği gibi:

[ ![Oturum ve Dağıt iletişim kutusu](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png)

**Olarak geçici yayımlama** iletişim kutusu, yayımlanmadan önce imzalı uygulama özetini sağlar. Bu bilgi doğruysa **Yayımla**.

[ ![Geçici iletişim kutusu olarak Yayımla](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png)

**Çıkış APK dosya** iletişim belirtilen yola APK Kaydet. **Kaydet**'e tıklayın.

![Çıktı APK dosya iletişim kutusu](images/xs/06-output-apk-file.png)

Ardından, sertifikanın parolasını girin (içinde kullanılan parolayı **yeni sertifika oluştur** iletişim) tıklatıp **Tamam**: 

![Sertifika parolası girin](images/xs/07-signing-certificate.png)

APK sertifikasıyla imzalanması ve belirtilen konuma kaydedilir. Tıklatın **Finder ortaya**:

[ ![Yayın başarılı iletişim](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png)

Bu, imzalı APK dosyasının konumu için bir Bulucu açar:

[ ![Finder gösterilen APK](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png)

APK Bulucu kopyalayın ve son hedefine göndermek hazırdır. APK Android cihazında yükleyip out dağıtımdan önce deneyin iyi bir fikirdir. Bkz: [yayımlama bağımsız olarak](~/android/deploy-test/publishing/publishing-independently.md) yayımlama hakkında daha fazla bilgi için bir *geçici* APK.

-----


<a name="nextsteps" />

## <a name="next-steps"></a>Sonraki Adımlar

Uygulama paketi sürümü için imzalanmış sonra yayımlanmalıdır. Aşağıdaki bölümlerde bir uygulamayı yayımlamak için çeşitli yollar açıklanmaktadır.
