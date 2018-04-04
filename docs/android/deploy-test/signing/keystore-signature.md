---
title: Keystore'nın imza bulma
ms.prod: xamarin
ms.assetid: 1b511fec-e6f6-453e-89c8-810aafb02b77
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 46b43e6689f751c4fac1e8668234fce7f953521e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="finding-your-keystores-signature"></a>Keystore'nın imza bulma

Bir Xamarin.Android uygulaması MD5 veya SHA1 imzası bağlıdır **.keystore** APK imzalamak için kullanılan dosya. Hata ayıklama derlemesi genellikle, farklı bir kullanır **.keystore** yayın derlemesi dosyasından.

## <a name="for-debug--non-custom-signed-builds"></a>Hata ayıklama için / özel olmayan derlemeleri imzalı

Xamarin.Android oturum açtığında, tüm hata ayıklama yapıları aynı **debug.keystore** dosya. Xamarin.Android ilk yüklendiğinde bu dosyası oluşturulur. Xamarin.Android varsayılan MD5 veya SHA1 imzası bulma işlemi aşağıdaki adımları ayrıntı **debug.keystore** dosya.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin bulun **debug.keystore** uygulamayı imzalamak için kullanılan dosya. Varsayılan olarak, bir Xamarin.Android uygulaması hata ayıklama sürümleri imzalamak için kullanılan bir anahtar şu konumda bulunabilir:

**C:\\kullanıcılar\\*kullanıcıadı*\\AppData\\yerel\\Xamarin\\Android Mono\\debug.keystore**

Bir anahtar deposu hakkında bilgi elde edilir çalıştırarak `keytool.exe` JDK komutunu. Bu aracı tipik olarak şu konumda bulunur:

**C:\\Program Files (x86)\\Java\\jdk*VERSION*\\bin\\keytool.exe**

Dizin içeren eklemek **keytool.exe** için `PATH` ortam değişkeni.
Açık bir **komut istemi** çalıştırıp `keytool.exe` aşağıdaki komutu kullanarak:

```cmd
keytool.exe -list -v -keystore "%LocalAppData%\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

Çalıştırdığınızda, **keytool.exe** aşağıdaki metin çıktısı. **MD5:** ve **SHA1:** etiketleri ilgili imzaları tanımlayın:

```cmd
Alias name: androiddebugkey
Creation date: Aug 19, 2014
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 53f3b126
Valid from: Tue Aug 19 13:18:46 PDT 2014 until: Sun Nov 15 12:18:46 PST 2043
Certificate fingerprints:
         MD5:  27:78:7C:31:64:C2:79:C6:ED:E5:80:51:33:9C:03:57
         SHA1: 00:E5:8B:DA:29:49:9D:FC:1D:DA:E7:EE:EE:1A:8A:C7:85:E7:31:23
         SHA256: 21:0D:73:90:1D:D6:3D:AB:4C:80:4E:C4:A9:CB:97:FF:34:DD:B4:42:FC:
08:13:E0:49:51:65:A6:7C:7C:90:45
         Signature algorithm name: SHA1withRSA
         Version: 3
```


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Xamarin bulun **debug.keystore** uygulamayı imzalamak için kullanılan dosya. Varsayılan olarak, bir Xamarin.Android uygulaması hata ayıklama sürümleri imzalamak için kullanılan bir anahtar şu konumda bulunabilir:

**Android/debug.keystore ~/.Local/Share/Xamarin/Mono**


Bir anahtar deposu hakkında bilgi elde edilir çalıştırarak **keytool** JDK komutunu. Bu aracı tipik olarak şu konumda bulunur:

**/System/Library/Java/JavaVirtualMachines/*VERSION*.jdk/Contents/Home/bin/keytool**

Dizin içeren eklemek **keytool** için **yolu** ortam değişkeni.
Açık bir **Terminal** çalıştırıp **keytool** aşağıdaki komutu kullanarak:

```bash
$ keytool -list -v -keystore ~/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

Çalıştırdığınızda, **keytool** aşağıdaki metin çıktısı. **MD5:** ve **SHA1:** etiketleri ilgili imzaları tanımlayın:

```bash
Alias name: androiddebugkey
Creation date: Apr 16, 2015
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 76afa120
Valid from: Thu Apr 16 10:52:27 PDT 2015 until: Sat Apr 08 10:52:27 PDT 2045
Certificate fingerprints:
     MD5:  0A:D3:7E:80:3D:40:2A:23:89:B9:AB:9C:4B:B6:63:36
     SHA1: 89:33:8F:F2:C5:0C:91:08:4A:CF:04:A5:EC:4A:31:80:84:18:0D:D4
     SHA256: 91:AC:3E:2F:CB:EF:50:07:2B:E0:D9:8D:8B:C2:42:87:6A:85:02:86:EB:44:84:10:34:02:ED:35:CE:C6:38:47
     Signature algorithm name: SHA256withRSA
     Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 65 6C FE 67 8E CD CB 9A   01 CD 9F E8 25 9C A4 A3  el.g........%...
0010: 4F 4E CF 17                                        ON..
]
]
```

-----

## <a name="for-release--custom-signed-builds"></a>Sürümü için / özel imzalı derlemeler

Özel bir imzalı yayın yapılar için işlem **.keystore** dosya aynıdır, yukarıda sürümüyle **.keystore** dosyası değiştirilirken **debug.keystore** dosyası Bu adres Xamarin.Android tarafından kullanılır. Bir anahtar parola ve yayın anahtar deposu dosyasının oluşturulduğu gelen diğer ad kendi değerlerini değiştirin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zaman Visual Studio **Dağıt** Sihirbazı, bir Xamarin.Android uygulaması imzalamak için kullanılır, sonuçta elde edilen anahtar şu konumda bulunur:

**C:\\kullanıcılar\\*kullanıcıadı*\\AppData\\yerel\\Xamarin\\Android Mono\\diğer\\alias.keystore**

Örneğin,'ndaki adımları izlediyseniz [yeni bir sertifika oluşturmak](~/android/deploy-test/signing/index.md#newcertvs) elde edilen örnek anahtar deposu yeni bir imzalama anahtarı oluşturmak için aşağıdaki konumda bulunur:

**C:\\kullanıcılar\\*kullanıcıadı*\\AppData\\yerel\\Xamarin\\Android Mono\\chimp\\chimp.keystore**

Bir Xamarin.Android uygulaması imzalama hakkında daha fazla bilgi için bkz: [Android uygulama paketi imzalama](~/android/deploy-test/signing/index.md).


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio **oturum ve Dağıt...**  elde edilen anahtar deposu uygulamanızı imzalamak için Sihirbazı şu konumda bulunur:

**~/Library/Developer/Xamarin/Keystore/*alias*/*alias*.keystore**

Örneğin,'ndaki adımları izlediyseniz [yeni bir sertifika oluşturmak](~/android/deploy-test/signing/index.md#newcertxs) elde edilen örnek anahtar deposu yeni bir imzalama anahtarı oluşturmak için aşağıdaki konumda bulunur:

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**

Bir Xamarin.Android uygulaması imzalama hakkında daha fazla bilgi için bkz: [Android uygulama paketi imzalama](~/android/deploy-test/signing/index.md).


-----
