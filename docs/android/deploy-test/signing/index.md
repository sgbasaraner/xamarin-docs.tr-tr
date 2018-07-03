---
title: Android uygulama paketi imzalama
description: Android uygulama paketi (APK) yayını için oturum açma
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/02/2018
ms.openlocfilehash: 4afcf42750cd9366bfd9fa5855fe1e7c0f114162
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403318"
---
# <a name="signing-the-android-application-package"></a>Android uygulama paketi imzalama

İçinde [uygulama yayına hazırlanma](~/android/deploy-test/release-prep/index.md) **arşiv Yöneticisi** uygulama oluşturmak ve imzalamak ve yayımlamak için bir arşiv yerleştirin için kullanıldı. Bu bölümde, bir Android imzalama kimliği oluşturmak için Android uygulamaları için yeni bir imzalama sertifikası oluşturup arşivlenmiş uygulama yayımlama açıklanmaktadır *geçici* diske. Sonuçta elde edilen APK bir uygulama mağazasına giderek olmadan Android cihazlar olarak dışarıdan yüklenebilir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

İçinde [yayımlama için arşiv](~/android/deploy-test/release-prep/index.md#archive), **dağıtım kanalı** iletişim dağıtım için iki seçenek sunulur. Seçin **geçici**:

[![Dağıtım Kanalı iletişim](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

İçinde [yayımlama için arşiv](~/android/deploy-test/release-prep/index.md#archive), **imzala ve Dağıt...**  iletişim dağıtım için iki seçenek bize sunulur. Seçin **geçici** tıklatıp **sonraki**:

[![İmzala ve Dağıt iletişim kutusu](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png#lightbox)

-----

<a name="newcertvs" />
<a name="newcert" />
<a name="newcertxs" />

## <a name="create-a-new-certificate"></a>Yeni bir sertifika oluşturun

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sonra **geçici** seçildiğinde, Visual Studio açılır olan **imzalama kimliği** sonraki ekran görüntüsünde gösterildiği gibi iletişim kutusunun sayfasında. Yayımlanacak. APK, bir imzalama anahtarı (sertifikası olarak da bilinir) ile ilk imzalanmalıdır.

Mevcut bir sertifikayı tıklayarak kullanılabilir **alma** düğmesine ve ardından devam etmek [APK oturum](#signapkvs). Aksi takdirde ' ı **+** yeni bir sertifika oluşturmak için:

[![Geçici imzalama kimliği](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png#lightbox)

**Oluştur Android anahtarı Store** iletişim kutusu görüntülenir; Android uygulamaları imzalamak için kullanabileceğiniz yeni bir imzalama sertifikası oluşturmak için bu iletişim kutusunu kullanın. Bu iletişim kutusunda gösterildiği gibi (kırmızı renkle) gerekli bilgileri girin:

[![Android anahtar Store iletişim kutusu oluşturma](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png#lightbox)

Aşağıdaki örnek sağlanmalıdır bilgi türünü gösterir. Tıklayın **Oluştur** yeni bir sertifika oluşturmak için:

[![Yeni bir sertifika oluşturma](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png#lightbox)

Sonuçta elde edilen anahtar deposu şu konumda bulunur:

**C:\\kullanıcılar\\*kullanıcıadı*\\AppData\\yerel\\Xamarin\\Android için Mono\\Keystore\\ *Diğer*\\*diğer*.keystore**

Örneğin, kullanarak **chimp** diğer ad olarak yukarıdaki adımları şu konumda yeni bir imzalama anahtarı oluşturursunuz:

**C:\\kullanıcılar\\*kullanıcıadı*\\AppData\\yerel\\Xamarin\\Android için Mono\\Keystore\\chimp\\chimp.keystore**

> [!NOTE]
> Sonuçta elde edilen anahtar deposu dosyasının ve parolayı güvenli bir yere yedekleyin mutlaka &ndash; çözüme dahil değildir. Kaybederseniz, anahtar deposu dosya (örneğin, başka bir bilgisayara veya Windows yeniden nedeniyle), uygulamanızı önceki sürümleri aynı sertifika ile imzalamak mümkün olmayacaktır.

Anahtar deposu hakkında daha fazla bilgi için bkz: [deponuzu'nın MD5 veya SHA1 imza bulma](~/android/deploy-test/signing/keystore-signature.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

' I tıklattıktan sonra **geçici**, açılır Mac için Visual Studio **Android imzalama kimliği** sonraki ekran görüntüsünde gösterildiği gibi iletişim. Yayımlanacak. APK, ilk olmalıdır (sertifikası olarak da bilinir) bir imzalama anahtarı ile imzalanmış. Bir sertifika zaten varsa, tıklayın **mevcut bir anahtarı içeri aktarma** içe aktarın ve ardından devam düğmesine [APK oturum](#signapkxs) 'a tıklayıp **yeni anahtar oluştur** düğmesi Yeni bir sertifika oluşturun: 

[![Android imzalama kimliği iletişim kutusu](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png#lightbox)

**Yeni sertifika oluştur** iletişim Android uygulamaları imzalamak için kullanılabilecek yeni bir imzalama sertifikası oluşturmak için kullanılır. Tıklayın **Tamam** gerekli bilgileri girdikten sonra:

[![Yeni sertifika iletişim kutusu oluşturma](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png#lightbox)

Sonuçta elde edilen anahtar deposu şu konumda bulunur:

**~/Library/Developer/Xamarin/KeyStore/Alias/Alias.KeyStore**

Örneğin, yukarıdaki adımları şu konumda yeni bir imzalama anahtarı oluşturabilirsiniz:

**~/Library/Developer/Xamarin/KeyStore/chimp/chimp.KeyStore**


> [!NOTE]
> Sonuçta elde edilen anahtar deposu dosyasının ve parolayı güvenli bir yere yedekleyin mutlaka &ndash; çözüme dahil değildir. Kaybederseniz, anahtar deposu dosya (örneğin, başka bir bilgisayara veya macOS yeniden nedeniyle), uygulamanızı önceki sürümleri aynı sertifika ile imzalamak mümkün olmayacaktır.

Anahtar deposu hakkında daha fazla bilgi için bkz: [deponuzu'nın MD5 veya SHA1 imza bulma](~/android/deploy-test/signing/keystore-signature.md).

-----

<a name="signapkvs" />

## <a name="sign-the-apk"></a>Apk'yı imzalama

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zaman **Oluştur** tıklandığında yeni bir anahtar deposu (içeren yeni bir sertifika) kaydedilir ve altında listelenen **imzalama kimliği** sonraki ekran görüntüsünde gösterildiği gibi. Google play'de bir uygulamayı yayımlamak için tıklatın **iptal** gidin [Google Play'e yayımlama](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Yayımlanacak *geçici*, tıklayın ve imzalama için kullanan imza kimliği seçin **Kaydet** bağımsız dağıtım için uygulama yayımlama. Örneğin, **chimp** imzalama kimliği (daha önce oluşturduğunuz) bu ekran görüntüsünde seçili:

[![İmzalama kimliği örneği](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png#lightbox)

Ardından, **arşiv Yöneticisi** yayımlama ilerleme durumunu görüntüler. Yayımlama işlemi tamamlandığında **Kaydet** iletişim kutusu açılır istemek için bir konum için burada oluşturulan. Depolanacak APK dosyası şudur:

[![Farklı Kaydet iletişim kutusu](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png#lightbox)

İstenilen konuma gelin ve tıklayın **Kaydet**. Anahtar parolasını bilinmiyorsa **imzalama parola** Seçili sertifika için parola iste için iletişim kutusu görüntülenir:

[![İmzalama parolası iletişim kutusu](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png#lightbox)

İmzalama işlemi tamamlandıktan sonra tıklayın **açık dağıtım**:

[![Dağıtım düğmesini açın](images/vs/08-open-distribution-sml.png)](images/vs/08-open-distribution.png#lightbox)

Bu, oluşturulan bir APK dosyasını içeren klasörü açmak Windows Explorer neden olur. Bu noktada, Visual Studio dağıtım için hazır bir APK bir Xamarin.Android uygulamasına derlenmiş.
Yayımlama hazır uygulamasını bir örneğini aşağıdaki ekran görüntüsünde görüntüler **MyApp.MyApp.apk**:

[![Windows Gezgini'nde gösterilen APK](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Burada görüldüğü gibi yeni bir sertifika anahtar deposuna eklendi. Google play'de bir uygulamayı yayımlamak için tıklatın **iptal** gidin [Google Play'e yayımlama](~/android/deploy-test/publishing/publishing-to-google-play/index.md).
Aksi takdirde **sonraki** uygulamayı ıntune'a yayımlamak *geçici* (bağımsız dağıtım için) Bu örnekte gösterildiği gibi:

[![İmzala ve Dağıt iletişim kutusu](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png#lightbox)

**Geçici olarak Yayımla** iletişim kutusu, yayımlanmadan önce imzalanmış uygulamasını özetini sağlar. Bu bilgi doğruysa tıklayın **Yayımla**.

[![Geçici iletişim kutusu olarak yayımlama](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png#lightbox)

**Çıkış APK dosyası** iletişim APK belirtilen yola kaydetmek. **Kaydet**'e tıklayın.

![Çıkış APK dosya iletişim kutusu](images/xs/06-output-apk-file.png)

Ardından, sertifikanın parolasını girin (kullanıldı parola **yeni sertifika oluştur** iletişim) tıklayıp **Tamam**: 

![Sertifika parolasını girin](images/xs/07-signing-certificate.png)

Apk'yı otomatik olarak imzalanan sertifika ile ve belirtilen konuma kaydedilir. Tıklayın **Finder'da Göster**:

[![Yayımlama başarılı iletişim](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png#lightbox)

Bu, imzalı APK dosyası konumunu Bulucu açar:

[![APK Finder'da gösterilen](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png#lightbox)

Apk'yı Bulucu kopyalayın ve son hedefine göndermek hazırdır. Bir Android cihazında apk'yı yüklemek ve dışarı dağıtımdan önce denemek için iyi bir fikirdir. Bkz: [yayımlama bağımsız olarak](~/android/deploy-test/publishing/publishing-independently.md) yayımlama hakkında daha fazla bilgi için bir *geçici* APK.

-----



## <a name="next-steps"></a>Sonraki Adımlar

Uygulama paketi için yayın imzalanmış sonra yayımlanması gerekir. Aşağıdaki bölümlerde, bir uygulamayı yayımlamak için çeşitli yollar açıklanmaktadır.
