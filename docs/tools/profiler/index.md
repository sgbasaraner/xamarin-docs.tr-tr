---
title: Xamarin Profiler
description: Bu kılavuz, Xamarin Profil Oluşturucu anahtar özelliklerini inceler. Bu Ara profil Oluşturucular, profil oluşturma ve ne zaman kullanılacağı ve standart bir iş akışı profil oluşturma Xamarin uygulamaları için.
ms.topic: article
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 7c44541c56d7b1a00a704cfc66812d5537ec83c4
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_Bu kılavuz, Xamarin Profil Oluşturucu anahtar özelliklerini inceler. Bu Ara profil Oluşturucular, profil oluşturma ve ne zaman kullanılacağı ve standart bir iş akışı profil oluşturma Xamarin uygulamaları için._

Bir uygulamanın başarı son kullanıcı deneyimi bağlıdır. Geliştirici olarak bazı gerçekten harika özellikler uygulamanızda uyguladıysanız, ancak uygulama ağır ya da tam çökme (Crash) ise, kullanıcı büyük olasılıkla bunu kurtulun.

Mono çalışma zamanı'nda çalışan programlar hakkında bilgi toplanıyor adlı için Mono güçlü bir komut satırı profil oluşturucu tarihsel olarak, özel [Mono günlük profil oluşturucu](http://www.mono-project.com/docs/debug+profile/profile/profiler/). Xamarin profil oluşturucu bir grafik arabirim Mono günlük profil oluşturucu ve Android, iOS, tvOS ve Mac uygulamalarını Mac ve Android, iOS ve Windows tvOS uygulamaları profil oluşturmayı destekler.

Profil oluşturma için kullanılabilir Instruments sayısı Xamarin profil oluşturucu sahip — ayırma, döngüler ve zaman profil oluşturucu. Bu kılavuzda ne bu Instruments ölçmek ve bunlar uygulamanızı nasıl analiz inceler ve her ekranda sunulan veriler anlamını açıklar.

Bu kılavuz genel profil oluşturma senaryoları inceler ve profil oluşturucu çözümlemek ve iOS ve Android uygulamaları en iyi duruma getirmek için bir araç olarak tanıtır.

## <a name="contents"></a>İçindekiler

- [İndirme ve yükleme](#Download_and_Install)
- [Profil oluşturucular ve profil oluşturma](#Profilers_and_Profiling)
- [Xamarin Profiler](#Xamarin_Profiler)
- [Profil Oluşturucu desteği](#Profiler_Support)
- [Profil Oluşturucu temelleri](#Profiler_Basics)
    - [Uygulamanızda profil izin verme](#Allowing_Profiling_in_your_App)
    - [Profil oluşturucu başlatma](#Launching_the_Profiler)
        - [Visual Studio'dan Mac için başlatma](#Launching_from_Xamarin_Studio)
        - [Visual Studio'dan başlatma](#Launching_from_Visual_Studio)
        - [Kaydetme ve profil oluşturucu oturumları yükleme](#Saving_and_Loading_Profiler_Sessions)
        - [Profil Oluşturucu özellikleri ve araçları](#Profiler_Features)
    - [Ayırma](#Allocations)
    - [Zaman profil oluşturucu](#Time_Profiler)
    - [Döngüler](#Cycles)
- [Profil oluşturma uygulamaları](#Profiling_Applications)
- [Özet](#Summary)

## <a name="download-and-install"></a>İndirme ve yükleme

> [!NOTE]
> **Not:** olması gerekir bir [Visual Studio Enterprise](https://www.visualstudio.com/vs/compare/) abone bir Mac bilgisayar üzerinde Mac için bu özelliği Windows ya da Visual Studio Enterprise ya da Visual Studio kilidini açmak için

Xamarin profil oluşturucu bir tek başına uygulamadır ve gelen IDE içinde profil oluşturmayı etkinleştirmek için Mac için Visual Studio ve Visual Studio ile tümleşiktir.

### <a name="download"></a>İndir

Platformunuza ilişkin yükleme paketini indirin:

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

Yüklendikten sonra sisteminize Xamarin profil oluşturucu ekleme için yükleyiciyi başlatın.


## <a name="profilers-and-profiling"></a>Profil oluşturucular ve profil oluşturma

Profil oluşturma bir önemli ve genellikle gözden kaçan önemli uygulama geliştirme adımdır. Profil oluşturma olduğundan, bir form **dinamik program analizi** -çalıştığından ve kullanımda olsa program analiz eder. Bir profil oluşturucu zaman karmaşıklık, belirli yöntemler kullanımını ve ayrılan bellek hakkındaki bilgileri toplayan bir veri araştırma aracıdır. Bir profil oluşturucu derinlikte detaya ve kod sorunlu alanları saptamak için bu ölçümleri analiz sağlar.

Tasarlama ve uygulama geliştirme, erken iyileştirmeyecek önemlidir; diğer bir deyişle, nadiren erişilir alanları kodunuzda geliştirme zamanı harcama. Profil oluşturma güç budur. Bir profil oluşturucu en çok bir anlayış kodunuzu temel bölümleri yaygın olarak kullanılan ve zaman yapmayı geliştirmeleri burada harcamanız alanları yardımcı bulun sağlar. Geliştiricilerin çoğu zaman, uygulamada zamanının nerede ve uygulamanız tarafından kullanılan bellek nasıl anlamak için dikkatli olun.

Profil oluşturma geliştirme tüm türleri yardımcı olur, ancak Mobil Geliştirme özellikle önemlidir. İyileştirilmemiş kodu mobil platformlarda masaüstü bilgisayarlarda çok daha belirgin ve uygulamanızın başarısını verimli bir şekilde çalışan güzel ve en iyi duruma getirilmiş koduna bağlıdır.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin profil oluşturucu geliştiricilere profili uygulamalardan için bir yol sağlayan Mac veya Visual Studio için Visual Studio içinde. Profil Oluşturucu toplar ve geliştirici tarafından uygulamanın davranışını çözümlemek için daha sonra kullanılabilir uygulama hakkında bilgi görüntüler. Xamarin Profil Oluşturucu ile bir uygulama profili için farklı yollar sayıda öğesine bellek profili oluşturma ve istatistiksel örnekleme. Bu ayırma ve zaman profil oluşturucu gerçekleştirildiği sırasıyla Instruments.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Şu anda, Xamarin profil oluşturucu Mac (aracılığıyla Visual Studio için Mac) Xamarin.iOS, Xamarin.Android ve Xamarin.Mac uygulamaları test etmek için kullanılabilir. Profil Oluşturucu IDE gelen ayrı bir işlemdir ve bu nedenle, Visual Studio'dan Mac için başlatılıyor ek olarak, bu bir tek başına uygulama olarak .exe incelemek için kullanılabilir ve `.mlpd` öğesinden üretilen dosyaları [mono günlük profil oluşturucu](http://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Şu anda, Xamarin profil oluşturucu (Visual Studio ve Mac için Visual Studio) üzerinden Windows Xamarin.Android uygulamaları test etmek için kullanılabilir. Profil Oluşturucu IDE gelen ayrı bir işlemdir ve bu nedenle, Visual Studio'dan başlatılıyor ek olarak, bu bir tek başına uygulama olarak .exe incelemek için kullanılabilir ve `.mlpd` öğesinden üretilen dosyaları [mono günlük profil oluşturucu](http://www.mono-project.com/docs/debug+profile/profile/profiler/) .

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>Profil Oluşturucu desteği

Xamarin profil oluşturucu desteği aşağıdaki platformlarda kullanılabilir:

 - Visual Studio için Mac (Enterprise Lisansı ile macOS)
    - Android
        - Aygıt ve öykünücüsü
    - iOS
        - Cihaz ve Simulator
    - tvOS (zaman gereç desteklenmez)
        - Cihaz ve Simulator
    - Mac

 - Visual Studio (yalnızca **Kurumsal** sürümü)
    - Android
        - Aygıt ve öykünücüsü
    - iOS [Experimental]
        - Cihaz ve Simulator
    - tvOS
        - Cihaz ve Simulator

Yapabileceğinizi unutmayın **yalnızca** profil **hata ayıklama** yapılandırmaları.

## <a name="profiler-basics"></a>Profil Oluşturucu temelleri

Bu bölümde, Xamarin profil oluşturucu bölümlerini tanıtır ve özelliklerini tours.

### <a name="allow-profiling-in-your-app"></a>Uygulamanızda profil izin ver

Uygulamanızı başarıyla profil önce uygulamanın proje seçeneklerinde profil oluşturma izni gerekir.

 - iOS:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

  **Derleme > iOS hata ayıklama > etkinleştirmek profil oluşturma**

  ![](images/ios-options-mac.png "iOS Mac için Visual Studio Seçenekleri iletişim kutusu")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Özellikler > iOS Yapı > etkinleştirmek profil oluşturma**

  ![](images/ios-project-options-vs.png "iOS Visual Studio Seçenekleri iletişim kutusu")

-----

 - Android:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

  **Derleme > Android hata ayıklama > Geliştirici Araçları'nı etkinleştir**

  ![Mac için Android Seçenekleri iletişim kutusunu Visual Studio'da](images/android-project-options.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Derleme > Android hata ayıklama > Geliştirici Araçları'nı etkinleştir**

  ![Mac için Android Seçenekleri iletişim kutusunu Visual Studio'da](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>Profil oluşturucu başlatma

Xamarin profil oluşturucu, iOS veya Android uygulamanızı profil olduğunda, IDE veya tek başına uygulama olarak başlatılabilir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

#### <a name="launching-from-visual-studio-for-mac"></a>Visual Studio'dan Mac için başlatma

1. İlk olarak, Mac için Visual Studio'da yüklenen uygulamanız varsa ve (varsayılan) hata ayıklama yapılandırmasını seçin emin olun.
2. Göz atın **çalıştırmak > Başlat profil**Mac Visual Studio'da veya **Çözümle > Xamarin profil oluşturucu** Visual Studio'da profil oluşturucu, aşağıdaki çizimde gösterildiği gibi açmak için:

  ![](images/start-profiling-xs.png "Mac için profil oluşturucu Visual Studio'dan başlatma")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="launching-from-visual-studio"></a>Visual Studio'dan başlatma

1.  İlk olarak, uygulamanızı Visual Studio yüklenmiş olması ve yukarıda belirtilen olarak (varsayılan) hata ayıklama yapılandırmasını seçin emin olun.
2.  Gözat **Çözümle > Xamarin profil oluşturucu** Visual Studio'da profil oluşturucu, aşağıdaki çizimde gösterildiği gibi açmak için:

![Profil Oluşturucu Visual Studio'dan başlatma](images/start-profiling-vs.png)

-----

Menü öğeleri gösterilmezse başvurmak [sorun giderme kılavuzu](~/tools/profiler/troubleshooting.md).

Bu profil oluşturucu başlatır ve uygulamayı profil otomatik olarak başlar.

Profil Oluşturucu, bellek ve performans ölçmek için kullanılabilir. Bu ayırmaları ve zaman profil oluşturucu aracılığıyla başarır sonraki bölümünde ayrıntılı olarak inceleyeceksiniz araçları.

#### <a name="saving-and-loading-profiler-sessions"></a>Kaydetme ve profil oluşturucu oturumları yükleme

Profil oluşturma oturumu herhangi bir zamanda kaydetmek üzere seçim yapın **Dosya > Kaydet...**  profil oluşturucu menü çubuğundan. Bu dosyaya kaydeder _mlpd_ biçimi, veri profil için bir özel, yüksek oranda sıkıştırılmış biçimi.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Yüklendikten sonra Xamarin profil oluşturucu aşağıdaki ekran görüntüsünde gösterildiği gibi uygulamaların klasörünüzde bulunabilir:

![](images/applications.png "Bağımsız profil oluşturucu Mac açın")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin profil oluşturucu uygulama yüklendikten sonra uygulama dizininizde bulunabilir:

![](images/applications-vs.png "Windows'dan bağımsız profil oluşturucu açın ")

-----

Yükünü *.mlpd* tek başına uygulama açarak profil oluşturucu dosyalarıyla seçme **seçin hedef** ve dosyası yükleniyor.

Daha fazla bilgi için bkz: [.mlpd dosyalar oluşturma ](~/tools/profiler/troubleshooting.md#gen_mlpd).


## <a name="profiler-features"></a>Profil Oluşturucu özellikleri

Xamarin profil oluşturucu aşağıda gösterildiği gibi beş bölümlerden oluşur:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](images/profiler-mac-sml.png "Mac için Visual Studio profil oluşturucu bölümlerde")](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/profiler-vs.png "Visual Studio profil oluşturucu bölümlerde")](images/profiler-vs.png#lightbox)

-----

- **Araç çubuğu** – profil oluşturucu en üstte yer alan, bu seçenekleri Başlat/Durdur profil için bir hedef işlem seçin, uygulamanın çalışma süresini görüntülemek ve profil oluşturucu uygulama oluşturma bölme görünümleri seçin sunar.
- **İzleme listesi** – bu profil oluşturma oturumu için yüklenen tüm Gereçleri listeler.
- **Grafiği çizmek** – bu grafikler yatay gereç listesinde ilgili Gereçleri ilgilidir. (Zaman profil oluşturucu altında gösterilen) kaydırıcı ölçeğini değiştirmek için kullanılabilir.
- **Ayrıntı alanı izleme** -geçerli Gereci seçili görünüm tarafından görüntülenen verileri içerir. Bu Görünümler bölümünde daha ayrıntılı ele alacağız.
- **Denetleyici Görünüm** – bu bölümlenmiş denetimi tarafından seçilen bölümleri içerir. Bölümler seçili Gereci bağımlıdır ve içerir: yapılandırma ayarları, istatistikler, yığın izleme bilgileri ve kök yolu.

### <a name="allocations"></a>Ayırma

Bunlar oluşturulan ve atık toplanan ayırmaları Gereci uygulamadaki nesneler hakkında ayrıntılı bilgi sağlar.

Profil Oluşturucu üstünde profil oluşturma sırasında düzenli aralıklarla ayrılan bellek miktarını görüntüler ayırmaları grafiktir. Şu anda ayırmaları ayırmaları toplam sayısı ve değil öbek boyutunu zamandaki o noktada grafiktir. Bir algılama, hiçbir zaman gidecek, her zaman sadece artacaktır. Bu, yığında ayrılmış nesneleri içerir. Kullanılan çalışma zamanı sürümü bağlı olarak, grafik bile aynı uygulama için farklı – bakabilirsiniz.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](images/allocations1.png "Ayırma yöntemi")](images/allocations1.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/allocations1-vs.png "Ayırma yöntemi")](images/allocations1-vs.png#lightbox)

-----

Geliştiriciler nasıl uygulamasını kullanarak ve bellek boşaltma çözümlemek izin veren ayırmaları Gereci farklı veri görünümleri vardır. Bu görünümler, aşağıda açıklanmıştır:

 -   **Ayırmalar** – Bu, tüm ayırmaları listesini görüntüler ve sınıf adına göre gruplandırır. Bu, sınıflar ve kullanılan yöntemleri, ne sıklıkta kullanılır ve kullanılan sınıflar toplu boyutunu harika bir genel bakış sağlar. Bir sınıf üzerinde çift tıklatarak gösterir ayrılmış bellek: 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

  [![](images/allocations3.png "Ayırmalar sekmesi")](images/allocations3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations2-vs.png "Ayırmalar sekmesi")](images/allocations2-vs.png#lightbox)

-----

Ayırmalar denetçisi görünümünü filtreleme ve nesneleri gruplama, istatistikleri ayrılan bellek sağlama ve üst ayırma seçenekleri yanı sıra, görünümler yığın izleme ve yol köküne sağlar.

 -   **Çağrı ağacı** – Bu uygulamadaki tüm iş parçacıklarının tüm çağrı ağacı görüntüler ve her bir düğümde ayrılan bellek hakkındaki bilgileri içerir. Listeden bir öğe seçildiğinde, tüm eşdüzey düğümleri görünür gri. Ağacı genişletin veya içine detaya öğesini çift tıklatın. Bu veri görünümü görüntülenirken, görüntü ayarları denetçisi görünümü, sunulan şeklini değiştirmek için kullanılabilir. Şu anda iki seçenek vardır:
    1.  **Çağrı ağacı ters** – Bu yığın izlemesi üstten alta göz önünde bulundurur. Derin yöntemleri gösteren bu uygun görünüm seçeneğini burada CPU kendi zamanı harcama aynıdır.
    2.  **İş parçacığı tarafından ayrı** – bu seçenek, iş parçacığı tarafından çağrı ağacı düzenler.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

  [![](images/allocations2.png "Çağrı ağacı sekmesi")](images/allocations2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations3-vs.png "Çağrı ağacı sekmesi")](images/allocations3-vs.png#lightbox)

-----

 -   **Anlık Görüntü** – bu bölmesi anlık bellek görüntüleri hakkındaki bilgileri görüntüler. Bu dinamik bir uygulama profili çıkarma sırasında üretmek için tıklatın _kamera_ belleği korunur ve yayımlanan görmek istediğiniz her bir noktada araç çubuğu düğmesini. Ardından başlık altında neler olduğunu keşfetmek için her anlık görüntü tıklayabilirsiniz. Anlık görüntü yalnızca dinamik olduğunda bir uygulama profili oluşturma alınabilir olduğunu unutmayın. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

  [![](images/allocations4.png "Anlık görüntü sekmesi")](images/allocations4.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations4-vs.png "Anlık görüntü sekmesi")](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>Zaman profil oluşturucu

Ne kadar zaman her bir yöntemin bir uygulaması, tam olarak harcanan zamanı profil oluşturucu Gereci ölçer. Uygulama düzenli aralıklarla duraklatıldı ve yığın izlemesi her etkin iş parçacığı üzerinde çalıştırın. Her satırda bir gereç ayrıntı alanı ve ardından yürütme yolunu gösterir.

Grafiği aşağıdaki ekran görüntüsünde gösterildiği gibi çalışan uygulama tarafından alınan örneklerin sayısını görüntüler:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Zaman profil oluşturucu izleme](images/time1.png)](images/time1.png#lightbox) 

[![Profil Oluşturucu izleme – örnekleri listesi süresi](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zaman profil oluşturucu izleme](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![Profil Oluşturucu izleme – örnekleri listesi süresi](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----


- **Çağrı ağacı** – gösterir harcanan zaman her bir yöntemin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

  [![](images/time2.png "Profil Oluşturucu izleme – çağrı ağacı süresi")](images/time2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/time2-vs.png "Profil Oluşturucu izleme – çağrı ağacı süresi")](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>Döngüler

C# ve F # yönetilen kodu kullanarak, oldukça yaygın ve hangi asla silinecek nesnelerin referansları oluşturmak ne yazık ki oldukça kolay olabilir. Bu intrument bu nesneler sabitleme ve uygulamanızda başvurulan döngüleri görüntülemek sağlar.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Döngüleri aracı](images/cycles-vs.png)](images/time1-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>Profil oluşturma uygulamaları

Şu anda, yalnızca varsayılan hata ayıklama yapılandırmaları profili.

Bir uygulama ile başka bir yapılandırma profili varsa, aşağıdaki ileti iletişim kutusuyla sunulur:


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Hata iletişim kutusu profil oluşturma](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/image1vs.png "Hata iletişim kutusu profil oluşturma")](images/image1vs.png#lightbox) 

-----


Seçin **güncelleştirme** devam etmek için.

<!---
## Profiling Android Applications


Due to the recent inclusion of the profiling libraries into any new Android project template, you will find that when profiling any legacy applications you are greeted with the message dialog above.

You will need to enable this to make sure that the profiling libraries are included in your Android application, for debug builds. This should not be checked for release builds as it creates overhead.


## Profiling iOS Applications

### Profiling tvOS

## Profiling Mac Applications
-->

### <a name="sgen-garbage-collector-and-profiling"></a>SGen atık toplayıcısını ve profil oluşturma

[SGen](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) atık toplayıcı tüm Xamarin platformları için kullanılır.

SGen olan bir uygulamayı üç yığın nesnelerin ayıran bir kişinin GC — çocuk odası, büyük öbek ve büyük nesne alanı. Çöp toplama daha hızlı yürütülmesini sağlar. SGen şu anda Xamarin.Android, varsayılan GC ve Xamarin.iOS birleşik uygulamalar.

Klasik API kullanarak Xamarin.iOS uygulaması Boehm GC – koruyucu, kişinin olmayan atık toplayıcı kullanılır. Koruyucu olduğu gibi profil oluşturucu kullanırken tutarsız sonuçlara yol açabilir kullanılabilir belleği boşaltmak daha az olasıdır. Bu nedenle, ayırmaları Gereci Boehm atık toplayıcı ile kullanılamaz.

Uygulamanız Boehm GC kullanıyorsa bir ileti iletişim istenir, ancak Xamarin dikkatli araştırma ve kapsamlı sınama yapılmadan SGen Boehm kullanan var olan iOS uygulama geçiş önermez. SGen için profil oluşturma ve bu sonuçları bellek kullanımı doğru Kıyaslama sağlamaz gibi geri, geçiş için geçiş Xamarin de önermez.

Bellek yönetimi hakkında daha fazla bilgi için başvurmak [bellek ve performans en iyi uygulamaları](~/cross-platform/deploy-test/memory-perf-best-practices.md) Kılavuzu.

## <a name="summary"></a>Özet

Bu kılavuzda hangi profil oluşturma ve nasıl geliştiriciler için yararlı olur inceledik. Biz sonra bazı geçmişi ve nasıl çalıştığı içine bilgi sağlama Xamarin profil oluşturucu kullanıma sunuldu. Son olarak biz Xamarin profil oluşturucu özelliklerini toured ve ayırma ve zaman profil oluşturucu Araçlar incelediniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Bellek ve performans en iyi uygulamalar](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Sürüm Notları](https://developer.xamarin.com/releases/profiler/preview/)
