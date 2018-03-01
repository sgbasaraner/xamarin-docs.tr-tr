---
title: "API anahtarı Google alma eşlemeleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: b529d0090595cc8a3020f37606d5dc3db5f0db74
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="obtaining-a-google-maps-api-key"></a>API anahtarı Google alma eşlemeleri

Android Google haritalar işlevselliği kullanmak için bir Maps API anahtarı Google ile kaydetmeniz gerekir. Bunu yapana kadar yalnızca bir harita yerine boş bir kılavuz uygulamalarınızda görürsünüz. Bir Google haritalar Android API v2 anahtarı edinmeniz gerekir - eski Google haritalar Android API anahtar v1'den anahtarlar çalışmaz.

Bir Maps API v2 anahtarı edinme, aşağıdaki adımları içerir:

1.  SHA-1 parmak izi, uygulamayı imzalamak için kullanılan bir anahtar alın.
2.  Google API Konsolu'nda bir proje oluşturun.
3.  API anahtarı edinme.

<a name="Step_1_-_Obtaining_your_Signing_Key_Fingerprint" />

## <a name="obtaining-your-signing-key-fingerprint"></a>İmzalama anahtarı parmak alma

Bir Maps API anahtarı Google istemek için uygulamayı imzalamak için kullanılan bir anahtar, SHA-1 parmak izi bilmeniz gerekir.
Genellikle, bu hata ayıklama anahtar deposu için SHA-1 parmak izi belirlemek gerekir ve ardından SHA-1 parmak izi yayımı için uygulamanızı imzalamak için kullanılan bir anahtar için anlamına gelir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Hata ayıklama sürümleri bir Xamarin.Android uygulaması olabilir imzalamak için kullanılan anahtar deposu varsayılan olarak şu konumda bulunamadı:

**C:\\kullanıcılar\\[USERNAME]\\AppData\\yerel\\Xamarin\\Android Mono\\debug.keystore**

Bir anahtar deposu hakkında bilgi elde edilir çalıştırarak `keytool` JDK komutunu. Bu araç genellikle Java bin dizininde bulunur:

**C:\\Program Files (x86)\\Java\\jdk[VERSION]\\bin\\keytool.exe**

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Hata ayıklama sürümleri bir Xamarin.Android uygulaması olabilir imzalamak için kullanılan anahtar deposu varsayılan olarak şu konumda bulunamadı:

**Android/debug.keystore /Users/[UserName]/.Local/Share/Xamarin/Mono**

Bir anahtar deposu hakkında bilgi elde edilir çalıştırarak `keytool` JDK komutunu. Bu araç genellikle Java bin dizininde bulunur:

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----


(Yukarıda gösterilen dosya yollarını kullanarak) aşağıdaki komutu kullanarak keytool çalıştırın:

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.KeyStore örneği

(Bu, sizin için hata ayıklama için otomatik olarak oluşturulur) varsayılan hata ayıklama anahtarı için bu komutu kullanın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----


### <a name="production-keys"></a>Üretim anahtarları

Bir uygulamayı Google Play dağıtırken olmalıdır [özel anahtarı ile imzalanmış](~/android/deploy-test/signing/index.md).
`keytool` Özel anahtar ayrıntılarını ve bir üretim Google haritalar API'si anahtarı oluşturmak için kullanılan elde edilen SHA-1 parmak izi ile çalıştırmanız gerekir. Güncelleştirmeyi unutmayın **AndroidManifest.xml** dağıtmadan önce doğru Google haritalar API'si anahtarı dosyasıyla.

### <a name="keytool-output"></a>Keytool çıkış

Konsol penceresinde çıktı aşağıdaki gibi bir şey görmeniz gerekir:

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

SHA-1 parmak izi kullanır (sonra listelenen **SHA1**) Bu kılavuzda daha sonra.

<a name="Step_2_-Create_an_API_project" />

## <a name="creating-an-api-project"></a>Bir API projesi oluşturma

İmzalama anahtar deposu, SHA-1 parmak izi aldıktan sonra Google API'leri konsolda yeni bir proje oluşturduğunuzda (veya var olan bir projeye Google haritalar Android API v2 hizmeti eklemek) gereklidir.

1. Bir tarayıcıda gidin [Google geliştiriciler konsol](https://console.developers.google.com/): tıklatıp **proje oluştur**:

   [![Google Developer konsolunda proje oluştur düğmesi](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png)

2. İçinde **yeni proje** görünür, iletişim proje adı girin.
   İletişim kutusu, bu örnekte gösterildiği gibi proje adına göre bir benzersiz proje kimliği üretirler:

   [![Yeni Proje XamarinMapsDemo adlı](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png)

3. Tıklatın **oluşturma** düğmesi. Bir dakika veya bunu sonra Proje oluşturulur ve gittiğiniz **API Yöneticisi** sayfası. İçinde **Kitaplığı** 'yi tıklatın **Google haritalar Android API'si**:

   [![Google haritalar Android API'si kitaplık bölümünde tıklatarak](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png)

4. Üstündeki **Google haritalar Android API'si** sayfasında, **etkinleştirmek** bu proje için etkinleştirmek için:

   [![Pano bölümünde Etkinleştir düğmesini tıklatın](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png)


Bu noktada API proje oluşturulup oluşturulmadığını ve Google haritalar Android API v2 eklendi. Ancak, kimlik bilgileri için oluşturduğunuz kadar bu API projenizde kullanamazsınız. Ardından bu anahtarı kullanmak için yetkili bir API anahtarı ve beyaz liste bir Xamarin.Android uygulaması oluşturmak üzere nasıl tümleştirildiği incelenmektedir.

<a name="Obtaining_the_API_Key" />

## <a name="obtaining-the-api-key"></a>API anahtarı edinme

Sonra **Google Developer konsolunda** API projesi süredir oluşturulan, onu bir Android API anahtarı oluşturmak gereklidir. Xamarin.Android uygulamaları, Android harita API v2 erişim verilmeden önce bir API anahtarı olması gerekir.

1. İçinde **Google haritalar Android API'si** görüntülenen sayfa (tıkladıktan sonra **etkinleştirmek** önceki adımda), tıklatın **kimlik bilgilerine Git** düğmesi:

   [![Bu API etkin iletisi](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png)

2. İçinde **kimlik bilgileri** sayfasında, **kimlik bilgileri ne yapmalıyım?** düğmesi:

   [![Proje iletişim için kimlik bilgilerini ekleyin](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png)

3. Bu düğmeye tıklandığında sonra API anahtarı oluşturulur. Ardından bu anahtar yalnızca uygulamanız bu anahtarla API'leri çağırmak şekilde kısıtlamak gereklidir. Tıklatın **kısıtlama anahtarı**:

   [![Kimlik bilgileri sayfasında tıklatmak kısıtlama anahtarı](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png)

4. Değişiklik **adı** alanının **API anahtar 1** anahtar için kullanılır hatırlamanıza yardımcı olacak bir adla (**XamarinMapsDemoKey** Bu örnekte kullanılan). Bundan sonra öğesini **Android uygulamalarını** radyo düğmesi:

   [![Kimlik bilgileri sayfasında Android uygulamaları seçme](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png)

5. SHA-1 parmak izini eklemek için tıklatın **+ Ekle paket adı ve parmak izi**:

   [![Tıklatmak Ekle paket adı ve parmak izi](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png)

6. Uygulamanızın paket adı girin ve SHA-1 sertifika parmak izini girin (aracılığıyla elde `keytool` bu kılavuzun önceki bölümlerinde açıklandığı gibi). Aşağıdaki örnekte, paket adı için `XamarinMapsDemo` girilen, alınan SHA-1 sertifika parmak izi arkasından **debug.keystore**:

   [![Girdiğiniz paket com.xamarin.docs.android.map adıdır](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png)

7. Google haritalar erişmek, APK için sırayla SHA-1 parmak izi içerir ve gerekir, APK imzalamak için kullandığınız her bir anahtar (hata ayıklama ve yayın) adlarını paketini, unutmayın. Hata ayıklama ve yayın APK oluşturmak için başka bir bilgisayar için bir bilgisayar kullanıyorsanız, örneğin, SHA-1 sertifika parmak izi ilk bilgisayarın hata ayıklama keystore gelen ve SHA-1 sertifika parmak izi yayın keystore gelen içermelidir İkinci bilgisayar. Tıklatın **+ Ekle paket adı ve parmak izi** Bu örnekte gösterildiği gibi başka bir parmak izi ve paket adı eklemek için:

   [![Başka bir SHA-1 sertifika başka bir parmak izi ekleme oluşturur](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png)

8. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için düğmesi. Ardından, API anahtarlarınızı listesi döndürülür. Daha önce oluşturduğunuz diğer API anahtarları varsa, aynı zamanda bir burada listelenir. Bu örnekte, yalnızca bir API anahtarı (önceki adımda oluşturulan) listelenir:

   [![XamarinMapsDemoKey API anahtarları listesinde gösterilir](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png)


<a name="Adding_the_Key" />

## <a name="adding-the-key-to-your-project"></a>Anahtar projenize ekleme

Son olarak, bu API anahtarı eklemek **AndroidManifest.XML** Xamarin.Android uygulamanıza dosyası. Aşağıdaki örnekte, `YOUR_API_KEY` önceki adımlarda oluşturulan API anahtarı ile değiştirilmelidir:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...

  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```


## <a name="related-links"></a>İlgili bağlantılar

- [Google API Konsolu](https://code.google.com/apis/console/)
- [Google haritalar API'si anahtarı](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
