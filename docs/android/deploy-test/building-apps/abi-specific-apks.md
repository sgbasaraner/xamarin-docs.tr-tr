---
title: "ABI özgü APKs oluşturma"
description: "Bu belge Xamarin.Android kullanarak tek bir ABI hedefleyen bir APK oluşturmak nasıl ele alınacaktır."
ms.topic: article
ms.prod: xamarin
ms.assetid: D21B195B-4530-4EB2-8704-5C4349A2CDD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: cf2f62929df63d08add76b7fb6de404d2780b2b3
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="building-abi-specific-apks"></a>ABI özgü APKs oluşturma

_Bu belge Xamarin.Android kullanarak tek bir ABI hedefleyen bir APK oluşturmak nasıl ele alınacaktır._



## <a name="overview"></a>Genel Bakış

Bazı durumlarda bir uygulama birden çok APKs sağlamak yararlı olabilir - her APK aynı anahtar ile imzalanmış ve aynı paket adı paylaşır, ancak belirli bir aygıt veya Android yapılandırma için derlenmiş. Bu önerilen yaklaşımı değil - birden çok aygıt ve yapılandırmaları destekleyen bir APK sahip çok daha kolaydır. Burada birden çok APKs oluşturma gibi yararlı olabilir bazı durumlar vardır:

-  **APK boyutunu azaltın** -Google Play APK dosyalarda 100 MB boyutunda uygular. Aygıt oluşturma yalnızca bir alt varlıkları ve uygulama için kaynak sağlamak gereken belirli APK's APK boyutunu azaltabilirsiniz.

-  **Destek farklı CPU mimariyi** - uygulamanız, CPU için yalnızca paylaşılan kitaplıklar dağıtmak kitaplıkları belirli CPU'lar için paylaşırsa.


Birden çok APKs dağıtımı - Google Play tarafından ele bir sorun karmaşık hale getirebilir. Google Play olun doğru APK uygulamanın sürüm koduna göre bir aygıtı ve bulunan diğer meta verileri için teslim edilir **AndroidManifest.XML**. Belirli Ayrıntılar ve Google Play birden çok APKs bir uygulama için nasıl desteklediği kısıtlamalar için başvurun [birden çok APK desteği Google belgelerine](http://developer.android.com/google/play/publishing/multiple-apks.html).

Bu kılavuz yapı komut dosyası nasıl ele alınacaktır belirli ABI hedefleme her APK bir Xamarin.Android uygulaması için birden çok APKs. Aşağıdaki konular ele alınacaktır:

1.  Benzersiz bir oluşturma *sürüm kodu* APK için.
1.  Geçici bir sürümünü oluşturmak **AndroidManifest.XML** bu APK için kullanılır.
1.  Uygulamayı kullanarak yapı **AndroidManifest.XML** önceki adımdan.
1.  APK imzalama ve onu zip-hizalama sürüm için hazırlayın.


AT bu kılavuzun sonudur adımları kullanarak komut dosyası nasıl gösteren bir anlatım [Rake](http://martinfowler.com/articles/rake.html).



### <a name="creating-the-version-code-for-the-apk"></a>APK için sürüm kodu oluşturma

Google önerir yedi rakamlı bir sürüm kod kullanan sürüm kodu için belirli bir algoritma (Lütfen bölümüne bakın *sürüm kodu şeması kullanarak* içinde [birden çok APK destek belgesi](http://developer.android.com/google/play/publishing/multiple-apks.html)).
Bu sürüm kodu düzeni sekiz basamağa genişleterek, Google Play doğru APK bir cihaza dağıttığınız sağlayacak sürüm koda bazı ABI bilgileri içerecek şekilde mümkündür. Aşağıdaki listede bu sekiz açıklar (soldan sağa dizinlenmiş) basamaklı sürüm kodu biçimi:

-   **Dizin 0** (Aşağıdaki diyagramda kırmızı) &ndash; tamsayı ABI için:
    -   1 &ndash; `armeabi`
    -   2 &ndash; `armeabi-v7a`
    -   6 &ndash; `x86`

-   **1-2 dizin** (Aşağıdaki diyagramda turuncu) &ndash; uygulama tarafından desteklenen en az API düzeyi.

-   **3-4 dizin** (Aşağıdaki diyagramda mavi) &ndash; desteklenen ekran boyutlarına:
    -   1 &ndash; küçük
    -   2 &ndash; normal
    -   3 &ndash; büyük
    -   4 &ndash; xlarge

-   **5-7 dizin** (Aşağıdaki diyagramda yeşil) &ndash; sürüm kodu için benzersiz bir numara. 
    Bu, geliştirici tarafından ayarlanır. Her uygulama için bir ortak sürümü artırmalısınız.

Aşağıdaki diyagram, yukarıdaki listede açıklanan her kod konumunu gösterir:

[![Renk tarafından kodlanmış sekiz basamaklı sürüm kodu biçimi diyagramı](abi-specific-apks-images/image00.png)](abi-specific-apks-images/image00.png#lightbox)


Google Play olun doğru APK göre aygıt teslim `versionCode` ve APK yapılandırma. En yüksek sürüm koduyla APK cihaza teslim edilecek. Örnek olarak, bir uygulama üç APKs aşağıdaki sürüm kodlara sahip olabilir:

-  11413456 - ABI olduğu `armeabi` ; atamak API Düzey 14; küçük büyük filtrelerine; 456 sürüm numarası.
-  21423456 - ABI olan `armeabi-v7a` ; atamak API Düzey 14; normal &amp; büyük ekranlar; 456 sürüm numarası.
-  61423456 - ABI olan `x86` ; atamak API Düzey 14; normal &amp; büyük ekranlar; 456 sürüm numarası.

Bu örnek ile çalışmaya devam etmek için özel bir hata düzeltildi düşünün `armeabi-v7a`. Uygulamanın sürüm için 457 artırır ve yeni bir APK ile oluşturulan `android:versionCode` 21423457 için ayarlayın. İçin versionCodes `armeabi` ve `x86` sürümleri aynı kalması.

Sürüm alır bazı x86 güncelleştirir veya hata daha yeni bir API (API düzeyi 19), hedef düzeltmeleri şimdi düşünün uygulamasının bu sürümü 500 yapma. Yeni `versionCode` pushservice/pushservice-v7a kalır değişiklik yapmadan 61923500 için değiştirir. Bu anda, sürüm kodlarını olacaktır:

-  11413456 - ABI olduğu `armeabi` ; atamak API Düzey 14; küçük büyük ekranlar; 456 bir sürüm adı.
-  21423457 - ABI olan `armeabi-v7a` ; atamak API Düzey 14; normal &amp; büyük ekranlar; 457 bir sürüm adı.
-  61923500 - ABI olan `x86` ; atamak API düzeyi 19; normal &amp; büyük ekranlar; 500 sürüm adı.


Bu sürüm kodlarını el ile koruma Geliştirici üzerinde önemli bir yük olabilir. Doğru hesaplama işlemi `android:versionCode` ve APK's oluşturma otomatikleştirilebilir.
Bunu yapmak nasıl bir örneği, bu belgenin sonundaki kılavuzda ele alınacaktır.


### <a name="create-a-temporary-androidmanifestxml"></a>Geçici AndroidManifest.XML oluşturma

Kesinlikle gerekli olmasa da, geçici bir oluşturma **AndroidManifest.XML** her ABI sızmasını bilgilerle bir APK diğerine doğabilecek sorunları önlemeye yardımcı olabilir. Örneğin, önemlidir, `android:versionCode` özniteliği her APK için benzersizdir.

Bunun nasıl yapılacağı söz konusu komut dosyası sistemde bağlıdır, ancak genellikle oluşturma işlemi sırasında bildirimini değiştirin, değiştirme ve ardından kullanarak geliştirme sırasında kullanılan Android bildiriminin bir kopyasını alma ilgilidir.



### <a name="compiling-the-apk"></a>APK derleme

ABI başına APK oluşturma en iyi şekilde gerçekleştirilir kullanarak `xbuild` veya `msbuild` aşağıdaki örnek komut satırında gösterildiği gibi:

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:Package /p:AndroidSupportedAbis=<TARGET_ABI> /p:IntermediateOutputPath=obj.<TARGET_ABI>/ /p:AndroidManifest=<PATH_TO_ANDROIDMANIFEST.XML> /p:OutputPath=bin.<TARGET_ABI> /p:Configuration=Release <CSPROJ FILE>
```

Her komut satırı parametresini aşağıdaki listede açıklanmaktadır:

-   `/t:Package` &ndash; Hata ayıklama anahtar deposu kullanarak imzalanmış bir Android APK oluşturur

-   `/p:AndroidSupportedAbis=<TARGET_ABI>` &ndash; Bu hedefe ABI. Aşağıdakilerden birini gerekir `armeabi`, `armeabi-v7a`, veya `x86`

-   `/p:IntermediateOutputPath=obj.<TARGET_ABI>/` &ndash; Bu yapı bir parçası olarak oluşturulan ara dosyalarını barındıracak dizindir. Gerekirse, Xamarin.Android ABI sonra gibi adlı bir dizin oluşturur, `obj.armeabi-v7a`. Bu sonuç "bir derlemeden diğerine sızmasını" dosyalarla olun sorunları engelleyecek şekilde bir klasör için her ABI kullanılması önerilir. Bu değer ile dizin ayırıcı sonlandırılır dikkat edin (bir `/` OS X söz konusu olduğunda).

-   `/p:AndroidManifest` &ndash; Bu özellik yolunu belirtir. **AndroidManifest.XML** derleme sırasında kullanılacak dosya.

-   `/p:OutputPath=bin.<TARGET_ABI>` &ndash; Bu, son APK barındırmak dizindir. Xamarin.Android sonra abı, örneğin adlı bir dizin oluşturur `bin.armeabi-v7a`.

-   `/p:Configuration=Release` &ndash; APK yayın derlemesinde gerçekleştirin. Hata ayıklama derlemeleri may Google Play karşıya yüklenemedi.

-   `<CS_PROJ FILE>` &ndash; Bu yolu olan `.csproj` Xamarin.Android projesi için dosya.



### <a name="sign-and-zipalign-the-apk"></a>Oturum ve Zipalign APK

Google Play dağıtılabilir önce APK imzalamak gereklidir. Bu kullanılarak gerçekleştirilebilir `jarsigner` Java Geliştirme Seti bir parçası olan uygulama. Aşağıdaki satırı demonstrats nasıl kullanılacağını komutu `jarsigner` komut satırında:

```shell
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore <PATH/TO/KEYSTORE> -storepass <PASSWORD> -signedjar <PATH/FOR/SIGNED_JAR> <PATH/FOR/JAR/TO/SIGN> <NAME_OF_KEY_IN_KEYSTORE>
```

Tüm Xamarin.Android uygulamaları zip-bir cihazda çalıştırmadan önce hizalanması gerekir. Bu biçimi kullanmak için komut satırının şöyledir:

```shell
zipalign -f -v 4 <SIGNED_APK_TO_ZIPALIGN> <PATH/TO/ZIP_ALIGNED.APK>
```


## <a name="automating-apk-creation-with-rake"></a>Rake ile APK oluşturmayı otomatikleştirme

Örnek Proje [OneABIPerAPK](https://github.com/xamarin/monodroid-samples/tree/master/OneABIPerAPK) ABI belirli bir sürüm numarası ve yapı üç ayrı APK'ın her aşağıdaki ABI's için hesaplamak nasıl gösterilmektedir basit bir Android projesidir:

-  pushservice
-  pushservice-v7a
-  x86


[Rakefile](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb) örnek proje her biri önceki bölümlerde açıklanan adımları gerçekleştirir:

1. [Android: versionCode oluşturma](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L30) APK için.

1. [Android: versionCode yazma](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L55) özel **AndroidManifest.XML** bu APK için.

1. [Yayın derlemesi derleme](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L63) ABI singularly hedeflediğini Xamarin.Android projesinin ve kullanarak **AndroidManifest.XML** önceki adımda oluşturulmuş.

1. [APK oturum ](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L66) üretim anahtar deposu ile.

1. [Zipalign](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L67) APK.


Tüm uygulama için APKs oluşturmak için çalıştırın `build` Rake görev komut satırından:

```shell
$ rake build
==> Building an APK for ABI armeabi with ./Properties/AndroidManifest.xml.armeabi, android:versionCode = 10814120.
==> Building an APK for ABI x86 with ./Properties/AndroidManifest.xml.x86, android:versionCode = 60814120.
==> Building an APK for ABI armeabi-v7a with ./Properties/AndroidManifest.xml.armeabi-v7a, android:versionCode = 20814120.
```

Rake görev tamamlandıktan sonra olacaktır üç `bin` dosya klasörlerle `xamarin.helloworld.apk`. Sonraki ekran görüntüsü içeriklerini ile bu klasörlerinin her biri gösterir:

[![Platforma özgü klasörleri xamarin.helloworld.apk içeren konumları](abi-specific-apks-images/image01.png)](abi-specific-apks-images/image01.png#lightbox)


> [!NOTE]
> Bu kılavuzda açıklanan derleme işlemi birçok farklı bir yapı sistemi uygulanabilir. Biz önceden yazılmış bir örnek gerekmese de mümkün olmalıdır [Powershell](http://technet.microsoft.com/en-ca/scriptcenter/powershell.aspx) / [psake](https://github.com/psake/psake) veya [sahte](http://fsharp.github.io/FAKE/).


## <a name="summary"></a>Özet

Bu kılavuz belirt ABI hedef bazı öneriler Android APK's oluşturma ile sağlanır. Ayrıca oluşturmak için bir olası düzeni ele alınan `android:versionCodes` APK yöneliktir CPU mimarisi sıralanmayacağı. İzlenecek yol Rake kullanarak komut dosyası, yapı sahip bir örnek proje dahil.



## <a name="related-links"></a>İlgili bağlantılar

- [OneABIPerAPK (örnek)](https://developer.xamarin.com/samples/OneABIPerAPK/)
- [Bir uygulama yayımlama](~/android/deploy-test/publishing/index.md)
- [Google Play için birden çok APK desteği](http://developer.android.com/google/play/publishing/multiple-apks.html)
