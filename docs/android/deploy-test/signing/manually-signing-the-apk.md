---
title: APK el ile imzalama
ms.prod: xamarin
ms.assetid: 08549E1C-7F04-4D20-9E7A-794B9D09FD12
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9e1b168b7212f093b50a36c40550fba2e7d63e77
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767241"
---
# <a name="manually-signing-the-apk"></a>APK el ile imzalama


Uygulama sürümü için oluşturulduktan sonra böylece bir Android cihazında çalıştırılabilir APK önce dağıtım imzalanması gerekir. Bu işlem genellikle IDE ile yapılır, ancak APK el ile komut satırında imzalamak gerekli olduğu bazı durumlar vardır. Aşağıdaki adımlar, bir APK imzalama ile ilgilidir:

1.   **Bir özel anahtar oluştur** &ndash; Bu adım yalnızca bir kez gerçekleştirilmesi gerekir. APK dijital olarak imzalamak bir özel anahtar gereklidir.
    Özel anahtarı hazırladıktan sonra gelecekteki sürüm derlemeler için bu adımı atlanabilir.

2.   **Zipalign APK** &ndash; *Zipalign* bir uygulama üzerinde gerçekleştirilen bir en iyi duruma getirme işlemidir. Çalışma zamanında APK daha verimli bir şekilde etkileşimde Android sağlar. Xamarin.Android çalışma zamanında bir denetimi yapar ve uygulamanın APK zipaligned olmadıysa çalışmasına izin vermez.

3.  **APK oturum** &ndash; kullanarak bu adımı içerir **apksigner** yardımcı programı'ndan Android SDK ve önceki adımda oluşturulan özel anahtarla APK imzalama. Android SDK derleme araçlarını v24.0.3 önce eski sürümleri ile geliştirilen uygulamaları kullanacağınız **jarsigner** JDK uygulamadan. Bu araçların her ikisi de aşağıdaki daha ayrıntılı olarak açıklanmıştır. 

Adımlarının sırasını önemlidir ve APK imzalamak için kullanılan hangi aracı bağlıdır. Kullanırken **apksigner**, ilk olarak önemli **zipalign** uygulama ve sahip imzaladıktan sonra **apksigner**.  Kullanmak gerekli değilse **jarsigner** APK imzalamak için sonra ilk kez oturum APK ve sonra çalıştırmak önemli olduğunu **zipalign**. 



## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu kullanarak odaklanacaktır **apksigner** Android SDK derleme araçları, v24.0.3 ya da daha yüksek. Bu, bir APK zaten oluşturulmuş olduğunu varsayar.

Android SDK derleme araçlarını daha eski bir sürümü kullanılarak oluşturulan uygulamaların kullanmalıdır **jarsigner** açıklandığı gibi [APK jarsigner ile oturum](#Sign_the_APK_with_jarsigner) aşağıda.



## <a name="create-a-private-keystore"></a>Bir özel anahtar deposu oluşturma

A *keystore* programı kullanılarak oluşturulan bir veritabanı güvenlik sertifikaların [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) Java SDK. Android dijital olarak imzalanmamış uygulamalar çalışmaz gibi bir anahtar deposu bir Xamarin.Android uygulaması yayımlama için kritik öneme sahiptir.

Geliştirme sırasında Xamarin.Android doğrudan öykünücü veya debuggable uygulamaları kullanmak için yapılandırılan cihazlar dağıtılacak uygulama verir uygulamayı imzalamak için hata ayıklama anahtar deposu kullanır.
Ancak, bu anahtar deposu uygulamalarını dağıtmak amacıyla geçerli bir anahtar tanınmıyor.

Bu nedenle, özel bir anahtar oluşturulur ve uygulamaları imzalamak için kullanılır. Bu, aynı anahtar yayımlama güncelleştirmeler için kullanılan ve diğer uygulamaları imzalamak için kullanılabilir olarak yalnızca bir kez gerçekleştirilmesi gereken bir adımdır.

Bu anahtar deposu korumak önemlidir. Ardından kaybolursa, Google Play uygulamayla güncelleştirmeleri yayımlamak mümkün olmaz.
Kayıp bir anahtar tarafından neden olduğu sorunu yalnızca çözüme yeni bir anahtar oluşturmak, APK yeni anahtarla yeniden oturum açın ve ardından yeni bir uygulama göndermek amacıyla olacaktır. Ardından eski uygulamayı Google Play'den kaldırılması gerekir. Bu yeni bir anahtar tehlikeye veya herkese dağıtılmış, benzer şekilde, daha sonra dağıtılacak uygulama resmi olmayan veya kötü amaçlı sürümleri için mümkündür.



### <a name="create-a-new-keystore"></a>Yeni bir anahtar oluşturun

Yeni bir anahtar oluşturmak için komut satırı aracı gerekir [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) Java SDK. Aşağıdaki kod parçacığını nasıl kullanılacağını gösteren bir örnektir **keytool** (Değiştir `<my-filename>` anahtar deposu için dosya adıyla ve `<key-name>` anahtarı anahtar deposu içinde adıyla):

```shell
$ keytool -genkeypair -v -keystore <filename>.keystore -alias <key-name> -keyalg RSA \
          -keysize 2048 -validity 10000
```

İlk şey, **keytool** için ister bir anahtar parolası. Ardından, anahtar oluşturma konusunda yardımcı olabilecek bilgiler için sorar. Aşağıdaki kod parçacığında adlı yeni bir anahtar oluşturma örneğidir `publishingdoc` dosyasında depolanacağı `xample.keystore`:

```shell
$ keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Ham Chimpanze
What is the name of your organizational unit?
  [Unknown]:  NASA
What is the name of your organization?
  [Unknown]:  NASA
What is the name of your City or Locality?
  [Unknown]:  Cape Canaveral
What is the name of your State or Province?
  [Unknown]:  Florida
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US
Enter key password for <publishingdoc>
        (RETURN if same as keystore password):
Re-enter new password:
[Storing xample.keystore]
```

Bir anahtar deposunda saklanan anahtarlardan listelemek için kullanın **keytool** ile &ndash; `list` seçeneği:

```shell
$ keytool -list -keystore xample.keystore
```


## <a name="zipalign-the-apk"></a>Zipalign APK

Bir APK ile oturum açmadan önce **apksigner**, ilk kullanarak dosya en iyi duruma getirmek önemlidir **zipalign** Android SDK aracından. **zipalign** 4-bayt sınırları boyunca bir APK kaynaklarında yapılandırmayı. Bu hizalama kaynakları uygulamanın performansını artırmak ve büyük olasılıkla bellek kullanımını azaltmanın APK, hızlı bir şekilde yüklemek Android sağlar. Xamarin.Android APK zipaligned olup olmadığını belirlemek için bir çalışma zamanı denetimi yürütecektir. APK zipaligned değilse, uygulama çalışmaz.

İzleme komut imzalı APK kullanın ve işaretli üretmek, zipaligned APK adlı **helloworld.apk** , dağıtım için hazır.

```shell
$ zipalign -f -v 4 mono.samples.helloworld-unsigned.apk helloworld.apk
```


## <a name="sign-the-apk"></a>APK oturum

APK zipaligning sonra bir anahtar kullanarak imzalamak gereklidir. Bu gerçekleştirilir **apksigner** bulunan aracı **derleme Araçları** SDK derleme araçlarını sürümünün dizin.  Android SDK derleme araçlarını v25.0.3, örneğin, sonra yüklenir **apksigner** dizininde bulunabilir:

```bash
$ ls $ANDROID_HOME/build-tools/25.0.3/apksigner
/Users/tom/android-sdk-macosx/build-tools/25.0.3/apksigner*
```

Aşağıdaki kod parçacığında varsayar **apksigner** tarafından erişilebilen `PATH` ortam değişkeni. Anahtar diğer adı kullanarak bir APK imzalayacak `publishingdoc` dosyasında bulunan **xample.keystore**:

```shell
$ apksigner sign --ks xample.keystore --ks-key-alias publishingdoc mono.samples.helloworld.apk
```

Bu komutu çalıştırdığınızda, **apksigner** gerekirse anahtar parolasını isteyecektir.

Bkz: [Google belgelerine](https://developer.android.com/studio/command-line/apksigner.html) kullanımı hakkında daha fazla ayrıntı için **apksigner**.

> [!NOTE]
> Göre [Google sorunu 62696222](https://issuetracker.google.com/issues/62696222), **apksigner** "Android SDK eksik". Geçici çözüm bu Android SDK derleme araçlarını v25.0.3 yükleyin ve bu sürümü kullanmaktır **apksigner**.  


<a name="Sign_the_APK_with_jarsigner" />

### <a name="sign-the-apk-with-jarsigner"></a>APK jarsigner ile oturum açın

> [!WARNING]
> APK ile imzalamak için nececssary ise, bu bölüm yalnızca geçerlidir **jarsigner** yardımcı programı. Geliştiriciler kullanmaları **apksigner** APK imzalamak için.

Bu teknik APK kullanarak dosya imzalama içerir **[jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)** Java SDK komutu.  **Jarsigner** aracı Java SDK'sı tarafından sağlanır. 

Aşağıdaki kullanarak bir APK oturum gösterilmektedir **jarsigner** ve anahtar `publishingdoc` adlı bir anahtar deposu dosyasında bulunan **xample.keystore** :

```shell
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore mono.samples.helloworld.apk publishingdoc
```

> [!NOTE]
> Kullanırken **jarsigner**, APK oturum önemlidir _ilk_ve ardından kullanmak için **zipalign**.  



## <a name="related-links"></a>İlgili bağlantılar

- [Uygulamanızı imzalama](https://source.android.com/security/apksigning/)
- [Java JAR imzalama](https://docs.oracle.com/javase/8/docs/technotes~/jar/jar.html#Signed_JAR_File)
- [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)
- [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
- [zipalign](https://developer.android.com/studio/command-line/zipalign.html)
- [Derleme araçları 26.0.0 - apksigner nerede?](https://issuetracker.google.com/issues/62696222)
