---
title: "Kitaplık yükleme hatalarını çözme"
description: "Bazı durumlarda, Android destek kitaplıkları yüklenirken hatalar alabilirsiniz. Bu kılavuz, bazı yaygın hatalar için geçici çözümler sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 5589d512f9a4ee9c1148810f36fee12d561f725c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="resolving-library-installation-errors"></a>Kitaplık yükleme hatalarını çözme

_Bazı durumlarda, Android destek kitaplıkları yüklenirken hatalar alabilirsiniz. Bu kılavuz, bazı yaygın hatalar için geçici çözümler sağlar._

## <a name="overview"></a>Genel Bakış

Bir Xamarin.Android uygulaması projesi oluştururken, Visual Studio veya Mac için Visual Studio çalıştığınızda bağımlılık kitaplıkları karşıdan yükleyip derleme hataları alabilirsiniz. Bu hataların çoğu ağ bağlantısı sorunları, bozuk dosya veya sürüm oluşturma sorunları nedeniyle oluşur. Bu kılavuz, en yaygın destek kitaplığı yükleme hataları açıklar ve bu sorunları çözmenin ve uygulama projenizi yeniden oluşturmayı almak için adımları sağlar. 

 
 
## <a name="errors-while-downloading-m2repository"></a>M2Repository indirilirken hataları

Görebileceğiniz **m2repository** bir NuGet paketi Android destek kitaplıkları veya Google Play hizmetlerinin başvururken hataları. Hata iletisi aşağıdakine benzer:

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

İçin bu örnekte, **android\_m2repository\_r16**, ancak farklı bir sürüm için aynı bu hata iletisini gibi görebilirsiniz **android\_m2repository\_r18**  veya **android\_m2repository\_r25**. 



### <a name="automatic-recovery-from-m2repository-errors"></a>Otomatik Kurtarma m2repository hataları 

Genellikle, sorunlu kitaplığı silerek ve adımları göre yeniden oluşturma bu sorun düzeltilebilir: 

1. Bilgisayarınızda destek kitaplığı dizinine gidin:

    -   Windows, destek kitaplıkları adresindedir **C:\\kullanıcılar\\_kullanıcıadı_\\AppData\\yerel\\Xamarin**. 

    -   Mac OS x'te destek kitaplıkları adresindedir **/Users/_kullanıcıadı_/.local/share/Xamarin**. 

2. Hata iletisine karşılık gelen kitaplığı ve sürüm klasörünü bulun. Örneğin, yukarıdaki hata iletisi için kitaplık ve sürüm klasör bulunur **Android.Support.v4\\22.2.1**:

    [![Örnek klasör konumu 22.2.1 için kitaplık desteği](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. Sürüm klasörün içeriğini silin. Kaldırdığınızdan emin olun **.zip** dosya yanı sıra **içerik** ve **katıştırılmış** bu klasör içinde alt dizinler. Bu ekran görüntüsünde gösterilen alt dizinleri ve dosyaları yukarıda gösterilen örnek hata iletisi (**içerik**, **katıştırılmış**, ve **android_m2repository_r16.zip**) üzeresiniz silinecek:

    [![Kitaplık klasörü 22.2.1 örnek içeriğini desteği](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   Silmek önemli olduğunu unutmayın *tüm* bu klasörün içeriği. Bu klasör başlangıçta içerebilir ancak "eksik" **android\_m2repository\_r16.zip** dosyası, bu dosyayı kaybolmuş kısmen indirilen veya bozuk.

4. Projeyi yeniden &ndash; bunun nedenle eksik kitaplığı yeniden indirmek yapı işlemi neden olur.

Çoğu durumda, aşağıdaki adımları yapı hatayı giderin ve devam etmenizi sağlar. Bu kitaplık silme derleme hatası çözmezse, el ile karşıdan yükleyip gerekir **android\_m2repository\_r_nn_.zip** dosya sonraki bölümde açıklandığı gibi. 



### <a name="manually-downloading-m2repository"></a>El ile m2repository indirme

Yukarıdaki otomatik kurtarma adımları kullanarak çalışmış ve hala derleme hataları varsa, el ile indirebilir **android\_m2repository\_r_nn_.zip** (bir web tarayıcısı kullanarak) dosya ve göre yükleyin için aşağıdaki adımları. Bu yordam, ayrıca geliştirme bilgisayarınızda Internet erişimi yoktur ancak farklı bir bilgisayar kullanılarak arşiv indirebilirsiniz kullanışlıdır. 

1.  Karşıdan **android\_m2repository\_r_nn_.zip** hata iletisine karşılık gelen dosya &ndash; bağlantılar (birlikte her bağlantının karşılık gelen MD5 karması aşağıdaki listede verilmiştir URL):

    -   [Android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    -   [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    -   [Android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    -   [Android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    -   [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    -   [Android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    -   [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    -   [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    -   [Android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    -   [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    -   [Android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    -   [Android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    -   [Android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    -   [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    -   [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    -   [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    -   [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    -   [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    Varsa **m2repository** arşiv gösterilmiyor bu tabloda, oluşturabileceğiniz indirme URL'si eklenerek **https://dl-ssl.google.com/android/repository/** adını **m2repository**  karşıdan yüklemek için. Örneğin, **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** indirmek için **android\_m2repository\_r10.zip** .

2.  Yukarıdaki tabloda gösterilen indirme URL'si karşılık gelen MD5 karması için dosyayı yeniden adlandırın. Örneğin, indirdiğiniz **android\_m2repository\_r25.zip**, kendisine yeniden adlandırma **0B3F1796C97C707339FB13AE8507AF50.zip**. İndirilen dosya indirme URL'si için MD5 karma tablosunda görünmüyorsa, kullanabileceğiniz bir [çevrimiçi MD5 Oluşturucu](http://www.webconfs.com/online-md5-generator.php) URL bir MD5 karma değeri dizeye dönüştürülemiyor. 

3.  Xamarin dosyasını kopyalayın **zips** klasörü: 

    -   Windows, bu klasör bulunur **C:\\kullanıcılar\\***kullanıcıadı***\\AppData\\yerel\\Xamarin\\zips**. 

    -   Mac OS X üzerinde bu klasör bulunur **/Users/***kullanıcıadı***/.local/share/Xamarin/zips**. 

    Örneğin, aşağıdaki ekran görüntüsünde sonucunu gösterir, **android\_m2repository\_r16.zip** indirilir ve indirme URL'sini Windows MD5 karması olarak yeniden adlandırıldı:

    [![Örnek için 0595E577D19D31708195A83087881EE6.zip adı değiştirilmekte r16.zip depo](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)


Bu yordamı derleme hatası çözmezse, el ile yüklemelisiniz **android\_m2repository\_r_nn_.zip** dosya, onu sıkıştırmasını açın ve içeriğini sonraki bölümde açıklandığı gibi yükleyin. 


### <a name="manually-downloading-and-installing-m2repository-files"></a>El ile yükleme ve m2repository dosyalar yükleniyor

Öğesinden kurtarma işlemini tamamen elle **m2repository** indirme hataları kapsar **android\_m2repository\_r_nn_.zip** (bir web tarayıcısı kullanarak), dosya şunu Bu ve bilgisayarınızda destek kitaplığı dizinine içeriğinin kopyalama. Aşağıdaki örnekte, biz bu hata iletisinden kurtarma: 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

Karşıdan yüklemek için aşağıdaki adımları kullanın **m2repository** ve içeriğini yükleyin:

1.  Hata iletisine karşılık gelen kitaplığı klasörün içeriğini silin. Örneğin, yukarıdaki hata iletisinde, içeriğini siler **C:\\kullanıcılar\\***kullanıcıadı***\\AppData\\yerel\\Xamarin\\ Android.Support.v4\\23.1.1.0**. 
    Daha önce açıklandığı gibi bu dizinin tüm içeriği silmeniz gerekir:

    [![İçeriği silmeden, katıştırılmış ve 23.1.1.0 android_m2repository klasörlerinden klasörü](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2.  Karşıdan **android\_m2repository\_r_nn_.zip** hata durumuna karşılık gelen Google dosyasından (önceki bölümde bağlantılar için tabloya bakın) iletisi.

3.  Bu ayıklama **.zip** arşiv herhangi bir konuma (örneğin, Masaüstü). Bu adına karşılık gelen bir dizin oluşturmanız gerekir **.zip** arşiv. Bu dizin içinde adında bir alt dizin bulmalısınız **m2repository**: 

    [![Ayıklanan zip arşivindeki bulunan m2repository klasörü](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4.  1. adımda temizlendi sürümlü kitaplığı dizini yeniden oluşturun **içerik** ve **katıştırılmış** alt dizinleri. Örneğin, aşağıdaki ekran görüntüsü gösterilmektedir **içerik** ve **katıştırılmış** oluşturulmakta alt dizinleri **23.1.1.0** için klasör **android \_m2repository\_r25.zip**: 

    [![İçerik ve katıştırılmış klasörleri 23.1.1.0 oluşturma klasörü](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5.  Kopya **m2repository** ayıklanan gelen **.zip** içine **içerik** önceki adımda oluşturduğunuz dizin: 

    [![23.1.1.0/content klasörüne kopyalanmasını m2repository ekran görüntüsü](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6.  Ayıklanan içinde **.zip** dizin, Gözat ' **m2repository\\com\\android\\Destek\\destek v4** ve karşılık gelen klasörünü açın sürüm numarasını yukarıda oluşturduğunuz (Bu örnekte, **23.1.1**):

    [![Support-v4/23.1.1 klasöründe bulunan dosyaları örnek listesi](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7.  Bu klasördeki tüm dosyaları kopyalayın **katıştırılmış** 4. adımda oluşturulan dizin:

    [![23.1.1.0/embedded klasöre kopyalanan dosyalar örneği](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8.  Tüm dosyalar üzerinde kopyalandığından emin olun. **Katıştırılmış** dizin şimdi içermelidir dosyaları gibi **.jar**, **.aar**, ve **.pom**.

Bu noktada, eksik bileşenleri el ile yüklemiş ve projenizin hatasız oluşturmalısınız. Aksi durumda, indirmiş doğrulayın **m2repository** **.zip** arşiv tam hata iletisi sürümünde karşılık gelen sürüm ve içeriğini yüklü olduğunu doğrulayın konumları Yukarıdaki adımlarda açıklandığı şekilde düzeltin. 



## <a name="summary"></a>Özet 

Bu makalede otomatik karşıdan yükle ve bağımlılık kitaplıkları yüklenmesi sırasında gerçekleştirilebildiği ortak hataları kurtarmak nasıl açıklanmıştır. Sorunlu kitaplığı silin ve yeniden indirin ve Kitaplığı'nı yeniden yüklemek için bir yol olarak projeyi oluşturmak nasıl açıklanmaktadır. Nasıl kitaplığı yükleyip içinde açıklanan **zips** klasör. Ayrıca, el ile yükleyip gerekli dosyaları otomatik anlamına gelir çözümlenemiyor sorunları çözmek için bir yol olarak için daha karmaşık bir yordam açıklanmaktadır. 
