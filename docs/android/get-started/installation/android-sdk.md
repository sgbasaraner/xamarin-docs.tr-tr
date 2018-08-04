---
title: Xamarin.Android için Android SDK'sını ayarlama
description: Visual Studio Android SDK Araçları, platformları ve geliştirme Xamarin.Android uygulamaları için gereken diğer bileşenleri yüklemek için kullandığınız bir Android SDK Yöneticisi'ni içerir.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/03/2018
ms.openlocfilehash: 92b2eec32aed27e630ac68f3522aa3b40cfc940a
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514496"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Xamarin.Android için Android SDK'sını ayarlama

_Visual Studio Android SDK Araçları, platformları ve geliştirme Xamarin.Android uygulamaları için gereken diğer bileşenleri yüklemek için kullandığınız bir Android SDK Yöneticisi'ni içerir._


## <a name="overview"></a>Genel Bakış

Bu kılavuz, Mac için Xamarin Android SDK Yöneticisi Visual Studio ve Visual Studio kullanmayı açıklar

> [!NOTE]
> Bu kılavuz yalnızca Visual Studio 2017 ve Visual Studio için Mac için geçerlidir  

Xamarin Android SDK Yöneticisi'ni (parçası olarak yüklenen **.NET ile Mobil Geliştirme** iş yükü), bir Xamarin.Android uygulaması geliştirmek için gereken en son Android bileşenlerini karşıdan yardımcı olur. Google'nın tek başına kullanım dışı SDK Yöneticisi değiştirir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="requirements"></a>Gereksinimler

Xamarin Android SDK Yöneticisi'ni kullanmak için aşağıdakiler gerekir:

- Visual Studio 2017 (Community, Professional veya Enterprise edition). Visual Studio 2017 sürüm 15.7 veya üzeri gereklidir.

- Sürüm 4.10.0 Xamarin için Visual Studio Araçları veya üzeri. 

Xamarin Android SDK Yöneticisi Visual Studio 2015 ile uyumlu değil. Visual Studio 2015'in kullanıcılar, Google Android SDK tarafından sağlanan SDK Yöneticisi Araçları kullanmanız gerekir.

Xamarin Android SDK Yöneticisi ayrıca Java Development Kit (hangi Xamarin.Android ile otomatik olarak yüklenir) gerektirir. Aralarından seçim yapabileceğiniz çeşitli JDK alternatifler vardır:

-   Varsayılan olarak, Xamarin.Android kullanır [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), geliştirme için API düzeyi 24 veya daha büyük olması durumunda gerekli olduğu (JDK 8 de destekler API düzeylerini 24'dan önceki).

-   Kullanmaya devam edebilirsiniz [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özel olarak geliştirme veya önceki bir sürümü kullanıyorsanız.

-   Daha sonra Microsoft dağıtımını kullanarak deneyebilirsiniz veya Visual Studio 15,8 Preview 5'i kullanıyorsanız [OpenJDK Microsoft dağıtımını](openjdk.md) (şu anda önizlemede) JDK 8 yerine.

> [!IMPORTANT]
> Xamarin.Android JDK 9 desteklemez.

 
## <a name="sdk-manager"></a>SDK Yöneticisi 

Visual Studio SDK Yöneticisi'ni başlatmak için tıklatın **Araçlar > Android > Android SDK Yöneticisi**:

[![Android SDK Yöneticisi'ni menü öğesinin konumu](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

Android SDK Yöneticisi açılır **Android SDK ve araçları** ekran. Bu ekran iki sekme bulunur &ndash; **platformları** ve **Araçları**:

[![Android SDK Yöneticisi'nin ekran görüntüsü platformları sekmede aç](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**Android SDK ve araçları** ekran aşağıdaki bölümlerde daha ayrıntılı açıklanmıştır.


### <a name="android-sdk-location"></a>Android SDK konumu

Android SDK konumu üst kısmında yapılandırılır **Android SDK ve araçları** ekranını önceki ekran görüntüsünde görüldüğü gibi. Bu konum doğru önce yapılandırılmalıdır **platformları** ve **Araçları** sekmeleri düzgün çalışır. Android SDK konumu için bir veya daha fazla aşağıdaki nedenlerden ayarlamanız gerekebilir:

1. Android SDK Yöneticisi Android SDK'yı bulamadı. 

2. Android SDK'sı bir alternatif (varsayılan olmayan) konumunda yüklediniz. 

Android SDK konumunu ayarlamak için üç nokta simgesine tıklayın (&hellip;) en sağındaki düğmeye **Android SDK konumu**. Bu açılır **klasöre Gözat** Android SDK konumuna gezinmek için kullanılacak iletişim. Aşağıdaki ekran görüntüsünde, Android SDK'sındaki **Program dosyaları (x86)\\Android** seçili:

![Android SDK'sını bulmak için Windows klasörü Gözat iletişim kutusunun ekran görüntüsü](android-sdk-images/win/05-browse-for-folder.png)

Tıkladığınızda **Tamam**, seçili konumda yüklü Android SDK SDK Yöneticisi yönetir.


### <a name="tools-tab"></a>Araçlar sekmesi

**Araçları** sekmesi bir listesini görüntüler _Araçları_ ve _ek özellikler_. Android SDK araçlarının, platform araçları yüklemek için bu sekmeyi kullanın ve derleme araçları.
Ayrıca, Android öykünücüsü yükleyebilirsiniz alt düzey hata ayıklayıcı (LLDB) NDK HAXM hızlandırma ve Google Play kitaplıkları.


Örneğin, Google Android öykünücüsü paketi indirmek için onay işaretine yanındaki tıklayın **Android öykünücüsü** tıklatıp **Değişiklikleri Uygula** düğmesi:

[![Araçlar sekmesindeki Android öykünücüsü'nü yükleme](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

Bir iletişim kutusu iletisi ile gösterilebilir _aşağıdaki paketi yüklemeden önce lisans koşullarını kabul etmenizi gerektiriyor_:

![Lisans kabulü ekranı](android-sdk-images/win/07-license-acceptance.png)

Tıklayın **kabul** hüküm ve koşulları kabul ederseniz. Pencerenin en altında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. Yükleme tamamlandıktan sonra **Araçları** sekmesi seçili araçları ve ek özellikler yüklendiğini gösterir.

### <a name="platforms-tab"></a>Platformları sekmesi

**Platformları** sekmesi platform SDK sürümleri her platform için (sistem görüntüleri için gibi) diğer kaynaklar ile birlikte bir listesini görüntüler:

[![Platformları bölmesinin ekran görüntüsü](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

Android sürümü bu ekranı listeler (gibi **Android 8.0**), kod adı (**Oreo**), API düzeyi (gibi **26**) ve bileşenleri (örneğin, platform için boyutları olarak **1 GB**). Kullandığınız **platformları** hedeflemek istediğiniz Android API düzeyi için bileşenleri yüklemek için sekmesinde. Android sürümleri ve API düzeyleri hakkında daha fazla bilgi için bkz. [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md).

Tüm bileşenler bir platformun yüklü olduğunda platform adının yanında bir onay işareti görünür. Kutu, platform için bir platformun tüm bileşenler yüklü değilse, doldurulur. Bileşenleri (ve hangi bileşenlerin yüklü) tıklayarak görmek için bir platform genişletebilirsiniz **+** platform solundaki kutusu.
Tıklayın **-** listelemek için bir platform bileşeni unexpand için.

Başka bir platform SDK eklemek için onay işaretine kadar platform yanındaki kutuyu görünen tüm bileşenleri, yüklemek için tıklatın ardından **Değişiklikleri Uygula**:

[![Örnek Android SDK'sı Android 7.1 Nougat bileşenleri ekleme](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

Yalnızca belirli bileşenleri yüklemek için bir kez platform yanındaki kutuyu tıklatın. Gereksinim duyduğunuz tek tek bileşenlerin sonra seçebilirsiniz:

[![Örneğin bazı Android 7.1 bileşenler ekleme](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

Yüklenecek bileşenleri sayısını yanındaki göründüğüne dikkat edin **Değişiklikleri Uygula** düğmesi. Tıkladıktan sonra **Değişiklikleri Uygula** düğmesi, göreceğiniz **lisans kabulü** daha önce gösterildiği ekran.
Tıklayın **kabul** hüküm ve koşulları kabul ederseniz. Bu iletişim birden fazla kez görebilirsiniz yüklemek için birden çok bileşen olduğunda. Pencerenin en altında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. İndirme ve yükleme işlemi tamamlandığında (Bu, kaç bileşenlerin indirilmesi gereken bağlı olarak çok sayıda dakika sürebilir), eklenen bileşenler işaretiyle ve olarak listelenen **yüklü**.

### <a name="respository-selection"></a>Depo seçimi

Varsayılan olarak, Android SDK Yöneticisi Microsoft tarafından yönetilen bir depodan platformu bileşenleri ve araçları yükler. Deneysel alfa/beta platformları ve henüz Microsoft depoda mevcut olmayan araçlarına erişim gerekiyorsa, Google deposu kullanmak için SDK Yöneticisi'ni geçiş yapabilirsiniz. Bu geçiş yapmak için sağ alt köşesindeki dişli simgesine tıklayıp **depo > Google (desteklenmiyor)**:

[![Google deposu seçme](android-sdk-images/win/11-google-repo-w157-sml.png)](android-sdk-images/win/11-google-repo-w157.png#lightbox)

Google deposu seçildiğinde, ek paketleri olarak görünebilir **platformları** önceden kullanılamayan sekmesi. (Yukarıdaki ekran görüntüsünde **Android SDK platformu 28** için Google deposu geçerek eklendi.) Google deposu kullanımı desteklenmez ve günlük geliştirme için önerilmez aklınızda bulundurun.

Platformlar ve Araçlar'ın desteklenen depoya geçmek için tıklatın **Microsoft (önerilir)**. Bu paketler ve Araçlar listesi için varsayılan seçim geri yükler.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="requirements"></a>Gereksinimler

Xamarin Android SDK Yöneticisi'ni kullanmak için aşağıdakiler gerekir:

-   Visual Studio Mac 7.5 için (veya üzeri).

Xamarin Android SDK Yöneticisi ayrıca Java Development Kit (hangi Xamarin.Android ile otomatik olarak yüklenir) gerektirir. Aralarından seçim yapabileceğiniz çeşitli JDK alternatifler vardır:

-   Varsayılan olarak, Xamarin.Android kullanır [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), geliştirme için API düzeyi 24 veya daha büyük olması durumunda gerekli olduğu (JDK 8 de destekler API düzeylerini 24'dan önceki).

-   Kullanmaya devam edebilirsiniz [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özel olarak geliştirme veya önceki bir sürümü kullanıyorsanız.

-   Veya üzeri 7.7 Mac için Visual Studio kullanıyorsanız, Microsoft dağıtımını kullanmayı deneyebilirsiniz [OpenJDK Microsoft dağıtımını](openjdk.md) (şu anda önizlemede) JDK 8 yerine.

> [!IMPORTANT]
> Xamarin.Android JDK 9 desteklemez.
 
## <a name="sdk-manager"></a>SDK Yöneticisi 

SDK Yöneticisi, Mac için Visual Studio'da başlatmak için tıklatın **Araçlar > SDK Yöneticisi**:
 
[![Android SDK Yöneticisi'ni menü öğesinin konumu](android-sdk-images/mac/01-sdk-manager-menu-item-m75-sml.png)](android-sdk-images/mac/01-sdk-manager-menu-item-m75.png#lightbox)

**Android SDK Yöneticisi** açılır **tercihleri penceresini**, üç sekme içerir **platformları**, **Araçları**ve **Konumları**:

[![Android SDK Yöneticisi'nin ekran görüntüsü platformları sekmede aç](android-sdk-images/mac/02-sdk-manager-platforms-m75-sml.png)](android-sdk-images/mac/02-sdk-manager-platforms-m75.png#lightbox)

Android SDK Yöneticisi'nin sekmeler, aşağıdaki bölümlerde açıklanmıştır.


### <a name="locations-tab"></a>Konumları sekmesi

**Konumları** Android SDK'sı, Android NDK ve Java SDK (JDK) konumları yapılandırmak için sekmesinde üç ayara sahiptir. Bu konumlar doğru önce yapılandırılmalıdır **platformları** ve **Araçları** sekmeleri düzgün çalışır.

SDK Yöneticisi yeniden başlatıldığında otomatik olarak yüklü her paket için yolu belirler ve olduğunu gösteren **bulunan** yolu yanında bir yeşil onay işareti simgesi yerleştirerek:

[![Konumları sekmesinin Ekran görüntüsü](android-sdk-images/mac/03-locations-tab-m75-sml.png)](android-sdk-images/mac/03-locations-tab-m75.png#lightbox)

Tıklayın **varsayılan ayarlarına geri döndürmeyi** SDK, NDK ve kendi varsayılan konumlarda JDK aramak için SDK Yöneticisi'ni neden düğme. 

Genellikle, kullandığınız **konumları** Android SDK ve/veya Java JDK konumunu değiştirmek için sekmesinde. Xamarin.Android uygulamaları geliştirmek için NDK yüklemenize gerek olmayan &ndash; NDK C ve C++ gibi yerel kodlu dilleri kullanarak uygulamanızın bölümlerini geliştirmeniz gerektiğinde kullanılır.

### <a name="tools-tab"></a>Araçlar sekmesi

**Araçları** sekmesi bir listesini görüntüler _Araçları_ ve _ek özellikler_. Android SDK araçlarının, platform araçları yüklemek için bu sekmeyi kullanın ve derleme araçları.
Ayrıca, Android öykünücüsü yükleyebilirsiniz alt düzey hata ayıklayıcı (LLDB) NDK HAXM hızlandırma ve Google Play kitaplıkları.

Örneğin, Google Android öykünücüsü paketi indirmek için onay işaretine yanındaki tıklayın **Android öykünücüsü** tıklatıp **Değişiklikleri Uygula** düğmesi:

[![Araçlar sekmesindeki Android öykünücüsü'nü yükleme](android-sdk-images/mac/04-tools-tab-m75-sml.png)](android-sdk-images/mac/04-tools-tab-m75.png#lightbox)

Bir iletişim kutusu iletisi ile gösterilebilir _aşağıdaki paketi yüklemeden önce lisans koşullarını kabul etmenizi gerektiriyor_:

[![Lisans kabulü ekranı](android-sdk-images/mac/05-license-acceptance-m75-sml.png)](android-sdk-images/mac/05-license-acceptance-m75.png#lightbox)

Tıklayın **kabul** hüküm ve koşulları kabul ederseniz. Pencerenin en altında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. Yükleme tamamlandıktan sonra **Araçları** sekmesi seçili araçları ve ek özellikler yüklendiğini gösterir.


### <a name="platforms-tab"></a>Platformları sekmesi

**Platformları** sekmesi platform SDK sürümleri her platform için (sistem görüntüleri için gibi) diğer kaynaklar ile birlikte bir listesini görüntüler:

[![Platformları bölmesinin ekran görüntüsü](android-sdk-images/mac/06-platforms-tab-m75-sml.png)](android-sdk-images/mac/06-platforms-tab-m75.png#lightbox)

Android sürümü bu ekranı listeler (gibi **Android 8.1**), kod adı (**Oreo**), API düzeyi (gibi **27**) ve bileşenleri (örneğin, platform için boyutları olarak **1 GB**). Kullandığınız **platformları** hedeflemek istediğiniz Android API düzeyi için bileşenleri yüklemek için sekmesinde. Android sürümleri ve API düzeyleri hakkında daha fazla bilgi için bkz. [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md).

Tüm bileşenler bir platformun yüklü olduğunda platform adının yanında bir onay işareti görünür. Kutu, platform için bir platformun tüm bileşenler yüklü değilse, doldurulur. Bileşenleri (ve hangi bileşenlerin yüklü) tıklayarak görmek için bir platform genişletebilirsiniz **ok** sol tarafındaki platform.
Tıklayın **aşağı ok** listelemek için bir platform bileşeni unexpand için.

Başka bir platform SDK eklemek için onay işaretine kadar platform yanındaki kutuyu görünen tüm bileşenleri, yüklemek için tıklatın ardından **Değişiklikleri Uygula**:

[![Örnek bir platformun tüm bileşenleri ekleme](android-sdk-images/mac/07-install-all-m75-sml.png)](android-sdk-images/mac/07-install-all-m75.png#lightbox)

Yalnızca bazı bileşenleri yüklemek için bir kez platform yanındaki kutuyu tıklatın. Gereksinim duyduğunuz tek tek bileşenlerin sonra seçebilirsiniz:

[![Örneğin, bazı bileşenler ekleme](android-sdk-images/mac/08-individual-components-m75-sml.png)](android-sdk-images/mac/08-individual-components-m75.png#lightbox)

Yüklenecek bileşenleri sayısını yanındaki göründüğüne dikkat edin **Değişiklikleri Uygula** düğmesi. Tıkladıktan sonra **Değişiklikleri Uygula** düğmesi, göreceğiniz **lisans kabulü** daha önce gösterildiği ekran.
Tıklayın **kabul** hüküm ve koşulları kabul ederseniz. Bu iletişim birden fazla kez görebilirsiniz yüklemek için birden çok bileşen olduğunda. Pencerenin en altında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. İndirme ve yükleme işlemi tamamlandığında (Bu, kaç bileşenlerin indirilmesi gereken bağlı olarak çok sayıda dakika sürebilir), eklenen bileşenler işaretiyle ve olarak listelenen **yüklü**.

### <a name="respository-selection"></a>Depo seçimi

Varsayılan olarak, Android SDK Yöneticisi Microsoft tarafından yönetilen bir depodan platformu bileşenleri ve araçları yükler. Deneysel alfa/beta platformları ve henüz Microsoft depoda mevcut olmayan araçlarına erişim gerekiyorsa, Google deposu kullanmak için SDK Yöneticisi'ni geçiş yapabilirsiniz. Bu geçiş yapmak için sağ alt köşesindeki dişli simgesine tıklayıp **depo > Google (desteklenmiyor)**:

[![Google deposu seçme](android-sdk-images/mac/09-google-repo-m75-sml.png)](android-sdk-images/mac/09-google-repo-m75.png#lightbox)

Google deposu seçildiğinde, ek paketleri olarak görünebilir **platformları** önceden kullanılamayan sekmesi. (Yukarıdaki ekran görüntüsünde **Android SDK platformu 28** için Google deposu geçerek eklendi.) Google deposu kullanımı desteklenmez ve günlük geliştirme için önerilmez aklınızda bulundurun.

Platformlar ve Araçlar'ın desteklenen depoya geçmek için tıklatın **Microsoft (önerilir)**. Bu paketler ve Araçlar listesi için varsayılan seçim geri yükler.

-----

 
## <a name="summary"></a>Özet

Bu kılavuzda açıklanan yükleme ve Mac için Xamarin Android SDK Yöneticisi aracını Visual Studio ve Visual Studio


## <a name="related-links"></a>İlgili bağlantılar

- [Android API düzeylerini anlama](~/android/app-fundamentals/android-api-levels.md)
- [Android SDK Aracı Üzerindeki Değişiklikler](~/android/troubleshooting/sdk-cli-tooling-changes.md)
