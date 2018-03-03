---
title: "Çok çekirdekli aygıtları & Xamarin.Android"
description: "Android üzerinde birkaç farklı bilgisayar mimarileri çalıştırabilirsiniz. Bu belge, bir Xamarin.Android uygulaması için işe farklı CPU mimari açıklanmaktadır. Bu belge nasıl Android uygulamaları da anlatılmıştır farklı CPU mimariyi desteklemek için hazırlanmıştır. Uygulama ikili arabirimi (ABI) sunulan ve bir Xamarin.Android uygulaması'nda kullanmak için hangi ABIs ilgili yönergeler sağlanacaktır."
ms.topic: article
ms.prod: xamarin
ms.assetid: D812883C-A14A-E74B-0F72-E50071E96328
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 2a7b2a856d51447d6b7ab2032ebf7445d3f06ecb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="multi-core-devices--xamarinandroid"></a>Çok çekirdekli aygıtları & Xamarin.Android

_Android üzerinde birkaç farklı bilgisayar mimarileri çalıştırabilirsiniz. Bu belge, bir Xamarin.Android uygulaması için işe farklı CPU mimari açıklanmaktadır. Bu belge nasıl Android uygulamaları da anlatılmıştır farklı CPU mimariyi desteklemek için hazırlanmıştır. Uygulama ikili arabirimi (ABI) sunulan ve bir Xamarin.Android uygulaması'nda kullanmak için hangi ABIs ilgili yönergeler sağlanacaktır._

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

Android için "fat ikili dosyaları," tek bir oluşturulmasını sağlayan `.apk` birden çok destekleyecek makine kodu içeren dosyası farklı CPU mimari. Bu makine koduyla her parçasının ilişkilendirerek gerçekleştirilir bir *uygulama ikili arabirimi*. ABI, hangi makine kodu belirli donanım aygıtta çalıştıracak denetlemek için kullanılır. Bir x86 üzerinde çalıştırmak Örneğin, bir Android uygulaması için onu aygıttır ABI destek uygulamayı derlerken x86 dahil etmek gerekli.

Özellikle, her bir Android uygulama en az bir destekleyecek *katıştırılmış uygulama ikili arabirimi* (EABI). Katıştırılmış yazılım programları için belirli kuralları EABI var. Tipik bir EABI gibi şeyler anlatmaktadır:

-   CPU yönerge kümesi.

-   Bellek endianness depolar ve çalışma zamanında yükler.

-   Nesne dosyaları ve program kitaplıkları, aynı zamanda içerik türünü izin verilen ya da bu dosyaları ve kitaplıkları desteklenen ikili biçimi.

-   Sistem ve uygulama kodu arasında veri iletmek için kullanılan çeşitli kuralları (örneğin: işlevleri çağrılır olduğunda, vb. hizalama kısıtlamaları nasıl kaydeder ve/veya yığın kullanılır.)

-   Numaralandırma türleri, yapıları, alanları ve diziler için hizalama ve boyutu kısıtlamaları.

-   Kullanılabilir makine kodunuz, çalışma zamanında genellikle belirli seçili kümesinden kitaplıkları işlevi simge listesi.



### <a name="armeabi-and-thread-safety"></a>pushservice ve iş parçacığı güvenliği

Uygulama ikili arabirimi aşağıda ayrıntılı olarak ele alınan, ancak unutulmaması önemlidir `armeabi` Xamarin.Android tarafından kullanılan çalışma zamanı *iş parçacığı güvenli olmayan*. Olan bir uygulama, `armeabi` destek dağıtıldığı bir `armeabi-v7a` aygıt, birçok garip ve unexplainable özel durum oluşur.

Android 4.0.0, 4.0.1, 4.0.2 ve 4.0.3 bir hata nedeniyle, yerel kitaplıkları gelen çekilir `armeabi` dizin olsa bile bir `armeabi-v7a` mevcut dizin ve cihaz bir `armeabi-v7a` aygıt.

> [!NOTE]
> **Not**: Xamarin.Android emin olun `.so` doğru sırada APK eklenir. Bu hata, bir sorun Xamarin.Android kullanıcıları için olmamalıdır.

<a name="ABI_Descriptions" />

### <a name="abi-descriptions"></a>ABI açıklamaları

Android tarafından desteklenen her ABI benzersiz bir ad tarafından tanımlanır.


<a name="armeabi" />

#### <a name="armeabi"></a>pushservice

Bu bir EABI ARMv5TE yönerge kümesi en az destek ARM tabanlı CPU için adıdır. Android little endian ARM GNU/Linux ABI izler. Bu abı, donanım destekli kayan nokta hesaplamaları desteklemez. Derleyicinin gelen yazılım yardımcı işlevleri tarafından gerçekleştirilen tüm FP işlemler `libgcc.a` statik kitaplık. SMP cihazlar tarafından desteklenmeyen `armeabi`.

**Not**: Xamarin.Android'ın `armeabi` kodu iş parçacığı içinde korumalı değil ve birden çok CPU üzerinde kullanılmamalıdır `armeabi-v7a`aygıtları (aşağıda açıklanmıştır). Kullanarak `aremabi` tek çekirdekli kodu `armeabi-v7a` aygıt güvenli değil.


<a name="armeabi-v7a" />

#### <a name="armeabi-v7a"></a>pushservice-v7a

Genişletir başka bir ARM tabanlı CPU yönerge kümesi budur `armeabi` EABI yukarıda açıklanan. `armeabi-v7a` EABI donanım kayan nokta işlemlerini ve birden çok CPU (SMP) cihazları için destek vardır. Kullanan bir uygulamayı `armeabi-v7a` EABI kullanan bir uygulama üzerinde önemli performans geliştirmeleri olasıdır `armeabi`.

**Not:** `armeabi-v7a` makine kodu ARMv5 cihazlarda çalışmaz.


<a name="arm64-v8a" />

#### <a name="arm64-v8a"></a>arm64-v8a

ARMv8 CPU mimarisine dayalı bir 64-bit yönerge kümesi budur. Bu mimari kullanılan *Nexus 9*.
Xamarin.Android 5.1 Bu mimarisine ait Deneysel destek sağlar (daha fazla bilgi için bkz: [Deneysel özellikleri](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Experimental_Features)).


<a name="x86" />

#### <a name="x86"></a>x86

Bu bir ABI yaygın olarak adlandırılmış kümesi yönerge destek CPU için adıdır *x86* veya *IA-32*. Bu ABI MMX, SSE, SSE2 ve SSE3 yönerge kümeleri dahil olmak üzere Pentium Pro yönerge kümesi için yönergeler karşılık gelir. Tüm diğer isteğe bağlı IA-32 yönerge kümesi uzantılar gibi içermez:

-  MOVBE yönerge.
-  Ek SSE3 uzantısı (SSSE3).
-  SSE4 herhangi bir türevi.


**Not:** Google TV x86 üzerinde çalışır ancak desteklenmiyor Android tarafından 's NDK veya Xamarin.Android. <a name="mips" />


<a name="x86_64" />

#### <a name="x8664"></a>x86_64

Bu bir ABI 64-bit x86 destek CPU için adıdır yönerge kümesi (olarak da adlandırılan *x64* veya *AMD64*). Xamarin.Android 5.1 Bu mimarisine ait Deneysel destek sağlar (daha fazla bilgi için bkz: [Deneysel özellikleri](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Experimental_Features)).


#### <a name="mips"></a>MIPS

Bu bir ABI en az destek MIPS tabanlı CPU için adıdır `MIPS32r1` yönerge kümesi. Hiçbiri MIPS 16 ya da `micromips` Android tarafından desteklenir.

**Not:** MIPS aygıtları Xamarin.Android tarafından şu anda desteklenmez, ancak gelecekteki bir sürümde görüntülenir.


<a name="APK_File_Format" />

#### <a name="apk-file-format"></a>APK dosya biçimi

Android uygulama paketi tüm kod, varlıklar, kaynakları ve sertifikalar bir Android uygulaması için gerekli tutan dosya biçimidir. Bu bir `.zip` kullanır ancak dosya `.apk` dosya adı uzantısı. Genişletildiğinde, içeriğini bir `.apk` tarafından oluşturulan Xamarin.Android aşağıdaki ekran görüntüsünde görülen:

[ ![.apk içeriğini](multicore-devices-images/00.png)](multicore-devices-images/00.png)

Hızlı içeriğini açıklamasını `.apk` dosyası:

-   **AndroidManifest.xml** &ndash; bu `AndroidManifest.xml` dosyasında ikili XML biçimi.

-   **Classes.dex** &ndash; bu içine derlenmiş uygulama kodu içeren `dex` Android çalışma zamanı VM tarafından kullanılan dosya biçimi.

-   **Resources.arsc** &ndash; bu dosya uygulama için önceden derlenmiş kaynakların tümünü içerir.

-   **LIB** &ndash; bu dizin için her ABI derlenmiş kod tutar. Bu önceki bölümde açıklanan her ABI için bir alt klasör içerir. Yukarıdaki ekran görüntüsü `.apk` söz konusu yerel kitaplıkları için her ikisini de sahip `armeabi-v7a` ve `x86` .

-   **META INF** &ndash; bu dizin (varsa) imzalama bilgileri, paket ve uzantısı yapılandırma verilerini depolamak için kullanılır.

-   **res** &ndash; içine derlendi değil kaynakları bu dizin tutan `resources.arsc` .

> [!NOTE]
> **Not**: dosya `libmonodroid.so` olan tüm Xamarin.Android uygulamalar tarafından gerektirilen yerel kitaplığı.


<a name="Android_Device_ABI_Support" />

#### <a name="android-device-abi-support"></a>Android cihaz ABI desteği

Android aygıtların yerel kod çalıştırırken en fazla iki ABIs destekler:

-   **"Birincil" ABI** &ndash; sistem görüntüde kullanılan makine kodu bu karşılık gelir.

-   **"İkincil" ABI** &ndash; de sistem görüntüsü tarafından desteklenen bir isteğe bağlı ABI budur.


Örneğin, tipik bir ARMv5TE aygıtın birincil abı, yalnızca olacaktır `armeabi`ARMv7 aygıt, birincil ABI belirtirsiniz yaparken `armeabi-v7a` ve ikincil abı, `armeabi`. Aygıt, birincil ABI yalnızca belirtirsiniz tipik bir x86 `x86`.

<a name="Android_Native_Library_Installation" />

### <a name="android-native-library-installation"></a>Android yerel kitaplığı yükleme

Paket yükleme sırasında yerel kitaplıkları içinde `.apk` uygulamanın yerel kitaplık dizinine genellikle ayıklanır `/data/data/<package-name>/lib`ve bundan sonra denir `$APP/lib`.

Android'ın yerel kitaplığı yükleme davranışı, Android sürümleri arasında önemli ölçüde farklılık gösterir.



#### <a name="installing-native-libraries-pre-android-40"></a>Yükleme yerel kitaplıklar: Ön Android 4.0

Android 4.0 önce dondurma Sandwich yalnızca yerel kitaplıklarından ayıklanır bir *tek ABI* içinde `.apk`. Android uygulamaları bu Mahsul, tüm yerel kitaplıkları için birincil ABI ayıklamak ilk deneyecek ve böyle bir kitaplık yoksa, Android sonra tüm yerel kitaplıkları için ikincil ABI ayıklar. Hayır "birleştirme" yapılır.

Örneğin, bir uygulamanın yüklü olduğu hakkında bir durum düşünün bir `armeabi-v7a` aygıt. `.apk,` Her ikisi de destekleyen `armeabi` ve `armeabi-v7a`, aşağıdaki ABI sahip `lib` dizinleri ve dosyaları:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Yükleme tamamlandıktan sonra yerel kitaplık dizinini içerir:

```shell
$APP/lib/libtwo.so # from the armeabi-v7a directory in the apk
```

Diğer bir deyişle, Hayır `libone.so` yüklenir. Bu sorunlara, olarak neden `libone.so` uygulamanın çalışma zamanında yük mevcut değil. Bu davranış beklenmeyen sırada olduğundan bir hata oturum açmış ve olarak yeniden sınıflandırılması "[tasarlandığı gibi çalışma](http://code.google.com/p/android/issues/detail?id=9089)."

Sonuç olarak, Android sürüm 4.0 önce hedeflerken sağlamak ise gerekli *tüm* yerel kitaplıkları için *her* uygulama, diğer bir deyişle, destekleyecek ABI `.apk` içermelidir :

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libone.so
lib/armeabi-v7a/libtwo.so
```


#### <a name="installing-native-libraries-android-40-ndash-android-403"></a>Yükleme yerel kitaplıklar: Android 4.0 &ndash; Android 4.0.3

Android 4.0 dondurma Sandwich ayıklama mantığı değiştirir. Bu, tüm yerel kitaplıkları listeleme, dosyanın basename ayıklanmış ve aşağıdaki koşulların her ikisi de karşılanıyorsa sonra kitaplık ayıklanacağı bakın:

-   Bunu zaten ayıklanmış kurmadı.

-   Yerel kitaplığın ABI hedefin birincil veya ikincil ABI eşleşir.


Bu koşullara uyan "birleştirme" davranış sağlar; diğer bir deyişle, biz varsa bir `.apk` aşağıdaki içeriğe sahip:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Yükleme sonrasında, ardından yerel kitaplık dizinini içerir:

```shell
$APP/lib/libone.so
$APP/lib/libtwo.so
```

Ne yazık ki, bu davranışı aşağıdaki belgede - açıklandığı gibi sırası bağımlı olduğundan [sorunu 24321: Galaxy Nexus 4.0.2 kullanan pushservice yerel kod pushservice ve pushservice-v7a dahil zaman içinde apk](http://code.google.com/p/android/issues/detail?id=25321).

Yerel kitaplıkları "sırayla" işlenir (tarafından listelendiği gibi örneğin, sıkıştırmasını açın) ve *ilk eşleşen* ayıklanır. Bu yana `.apk` içeren `armeabi` ve `armeabi-v7a` sürümleri `libtwo.so`ve `armeabi` listelenen olmasından önce `armeabi` ayıklanır, sürüm *değil* `armeabi-v7a` Sürüm:

```shell
$APP/lib/libone.so # armeabi
$APP/lib/libtwo.so # armeabi, NOT armeabi-v7a!
```

Ayrıca, hatta her iki `armeabi` ve `armeabi-v7a` ABIs belirtilir (aşağıdaki bölümde açıklandığı gibi *desteklenen ABIs bildirme*), Xamarin.Android aşağıdaki öğesinde oluşturur.
`csproj`:

```xml
<AndroidSupportedAbis>armeabi,armeabi-v7a</AndroidSupportedAbis>
```

Sonuç olarak, `armeabi` `libmonodroid.so` içinde ilk bulunacaktır `.apk`ve `armeabi` `libmonodroid.so` , rağmen ayıklanan bir olacaktır `armeabi-v7a` `libmonodroid.so` var ve için en iyi duruma getirilmiş Hedef. Bu ayrıca belirsiz çalışma zamanı hataları olarak sonuçlanabilir `armeabi` SMP güvenli değil.


##### <a name="installing-native-libraries-android-404-and-later"></a>Yükleme yerel kitaplıklar: Android 4.0.4 ve sonraki sürümler

Android 4.0.4 ayıklama mantığı değiştirir: tüm yerel kitaplıkları listeleme, dosyanın basename okuyun, ardından birincil ABI sürüm (varsa) veya ikincil ABI (varsa) ayıklayın. Bu davranış "birleştirme" sağlar; diğer bir deyişle, biz varsa bir `.apk` aşağıdaki içeriğe sahip:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Yükleme sonrasında, ardından yerel kitaplık dizinini içerir:

```shell
$APP/lib/libone.so # from armeabi
$APP/lib/libtwo.so # from armeabi-v7a
```

<a name="Xamarin.Android_and_ABIs" />

### <a name="xamarinandroid-and-abis"></a>Xamarin.Android ve ABIs

Xamarin.Android aşağıdaki mimariyi destekler:

-  `armeabi`
-  `armeabi-v7a`
-  `x86`

Xamarin.Android için aşağıdaki mimariler Deneysel destek sağlar:

-  `arm64-v8a`
-  `x86_64`


64 bit çalışma zamanları olduğuna dikkat edin *değil* 64 bit aygıtlarda uygulamanızı çalıştırmak için gereklidir. Xamarin.Android 5.1 Deneysel özellikleri hakkında daha fazla bilgi için bkz: [Deneysel özellikleri](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Experimental_Features).

Xamarin.Android şu an için destek sağlamaz `mips`.


<a name="Declaring_Supported_ABIs" />

### <a name="declaring-supported-abis"></a>ABI'ın desteklenen bildirme

Varsayılan olarak, Xamarin.Android için varsayılan `armeabi-v7a` için **sürüm** yapılar ve `armeabi-v7a` ve `x86` için **hata ayıklama** oluşturur. Farklı ABIs desteği, bir Xamarin.Android projesi için Proje seçenekleri aracılığıyla ayarlanabilir. Visual Studio'da bu ayarlanabilir **Android seçenekleri** proje sayfasının **özellikleri**altında **Gelişmiş** sekmesinde, aşağıdaki ekran görüntüsünde gösterildiği gibi:

![Android seçenekleri Gelişmiş Özellikler](multicore-devices-images/vs-abi-selections.png)


Mac için Visual Studio'da üzerinde desteklenen mimariler seçilebileceğini **Android derleme** sayfasında **proje seçenekleri**altında **Gelişmiş** sekmesinde, aşağıda gösterildiği gibi Ekran:

[![Android derleme ABIs desteklenen](multicore-devices-images/xs-abi-selections-sml.png)](multicore-devices-images/xs-abi-selections.png)

Ne zaman gibi ek ABI destek bildirmeniz gerekebilir, bazı durumlar vardır:

-   Uygulamayı dağıtmak bir `x86` aygıt.

-   Uygulamayı dağıtmak bir `armeabi-v7a` iş parçacığı güvenliği emin olmak için aygıt.



## <a name="summary"></a>Özet

Bu belgede ele alınan bir Android uygulaması çalışabilir farklı CPU mimari. Uygulama ikili arabirimi ve farklı CPU mimarileri desteklemek için Android tarafından nasıl kullanıldığı sunmuştur.
Ardından bir Xamarin.Android uygulaması ABI destek belirtme tartışmak için oluştu ve Xamarin.Android uygulamaları kullanırken ortaya çıkan sorunları vurgulanmış bir `armeabi-v7a` yalnızca hedeflenen aygıtı `armeabi`.


## <a name="related-links"></a>İlgili bağlantılar

- [MIPS mimarisi](http://www.mips.com/products/product-materials/processor/mips-architecture)
- [ABI ARM mimarisi (PDF) için](http://infocenter.arm.com/help/topic/com.arm.doc.ihi0036b/IHI0036B_bsabi.pdf)
- [Android NDK](http://developer.android.com/tools/sdk/ndk/index.html)
- [9089:nexus bir sorun - pushservice-v7a sırasında en az bir kitaplığın ise tüm yerel kitaplıkları pushservice yüklenmiyor](http://code.google.com/p/android/issues/detail?id=9089)
- [Sorunu 24321: pushservice ve pushservice-v7a dahil apk içinde Galaxy Nexus 4.0.2 pushservice yerel kod kullanır](http://code.google.com/p/android/issues/detail?id=25321)
