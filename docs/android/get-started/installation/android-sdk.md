---
title: Xamarin.Android için Android SDK'sını ayarlama
description: Visual Studio Android SDK Araçları, platformları ve geliştirme Xamarin.Android uygulamaları için gereken diğer bileşenleri yüklemek için kullandığınız bir Android SDK Yöneticisi'ni içerir.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/10/2018
ms.openlocfilehash: 895496f6a198f679ce08322ae48fe88e03b85629
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947276"
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

Xamarin Android SDK Yöneticisi, Visual Studio ile uyumlu değil.
2015. Visual Studio 2015'in kullanıcılar, Google Android SDK tarafından sağlanan SDK Yöneticisi Araçları kullanmanız gerekir.


Xamarin Android SDK Yöneticisi ayrıca Java Development Kit (hangi Xamarin.Android ile otomatik olarak yüklenir) gerektirir.
Xamarin.Android kullanan [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), geliştirme için API düzeyi 24 veya daha büyük olması durumunda gerekli olduğu (JDK 8 de destekler API düzeylerini 24'dan önceki). Kullanmaya devam edebilirsiniz [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özel olarak geliştirme veya önceki bir sürümü kullanıyorsanız.

> [!IMPORTANT]
> Xamarin.Android JDK 9 desteklemez.

 
## <a name="sdk-manager"></a>SDK Yöneticisi 

Visual Studio SDK Yöneticisi'ni başlatmak için tıklatın **Araçlar > Android > Android SDK Yöneticisi**:

[![Android SDK Yöneticisi'ni menü öğesinin konumu](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

**Xamarin Android SDK Yöneticisi** açılır **Android SDK ve araçları** ekran. Bu ekran iki sekme bulunur &ndash; **platformları** ve **Araçları**:

[![Android SDK Yöneticisi'nin ekran görüntüsü platformları sekmede aç](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**Android SDK ve araçları** ekran aşağıdaki bölümlerde daha ayrıntılı açıklanmıştır.


### <a name="android-sdk-location"></a>Android SDK konumu

Android SDK konumu üst kısmında yapılandırılır **Android SDK ve araçları** ekran, önceki görüntüde görüldüğü gibi. Bu konum doğru önce yapılandırılmalıdır **platformları** ve **Araçları** sekmeleri düzgün çalışır. Android SDK konumu için bir veya daha fazla aşağıdaki nedenlerden ayarlamanız gerekebilir:

1. Android SDK'sını bulmak Xamarin SDK yöneticisini bulamadı. 

2. Android SDK'sı bir alternatif (varsayılan olmayan) konumunda yüklediniz. 

Android SDK konumunu ayarlamak için tıklayın &hellip; en sağındaki düğmeye **Android SDK konumu**. Bu açılır **klasöre Gözat** Android SDK konumuna gezinmek için kullanılacak iletişim. Aşağıdaki ekran görüntüsünde, Android SDK'sındaki **Program dosyaları (x86)\\Android** seçili:

![Android SDK'sını bulmak için Windows klasörü Gözat iletişim kutusunun ekran görüntüsü](android-sdk-images/win/05-browse-for-folder.png)

Tıkladığınızda **Tamam**, seçili konumda yüklü Android SDK Xamarin Android SDK Yöneticisi yönetir.



### <a name="tools-tab"></a>Araçlar sekmesi

**Araçları** sekmesi bir listesini görüntüler _Araçları_ ve _ek özellikler_. Android SDK araçlarının, platform araçları yüklemek için bu sekmeyi kullanın ve derleme araçları.
Ayrıca, Android öykünücüsü yükleyebilirsiniz alt düzey hata ayıklayıcı (LLDB) NDK HAXM hızlandırma ve Google Play kitaplıkları.


Örneğin, Google Android öykünücüsü paketi indirmek için onay işaretine yanındaki tıklayın **Android öykünücüsü** tıklatıp **Değişiklikleri Uygula** düğmesi:

[![Araçlar sekmesindeki Android öykünücüsü'nü yükleme](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)



Bir iletişim kutusu iletisi ile gösterilebilir _bazı bileşenler güncelleştirilebilir. Şimdi güncelleştirmesini istiyor musunuz?_ **Evet**'i tıklayın. Ardından, bir lisans kabulü iletişim kutusu gösterilir:


![Lisans kabulü ekranı](android-sdk-images/win/07-license-acceptance.png)


Tıklayın **kabul** hüküm ve koşulları kabul ederseniz. Pencerenin en altında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. Yükleme tamamlandıktan sonra **Araçları** sekmesi seçili araçları ve ek özellikler yüklendiğini gösterir.



### <a name="platforms-tab"></a>Platformları sekmesi

**Platformları** sekmesi platform SDK sürümleri her platform için (sistem görüntüleri için gibi) diğer kaynaklar ile birlikte bir listesini görüntüler.


[![Platformları bölmesinin ekran görüntüsü](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)


Android sürümü bu ekranı listeler (gibi **Android 7.0**), kod adı (**Nougat**), API düzeyi (gibi **24**) ve durumu (**yüklü** platform yüklediyseniz). Kullandığınız **platformları** hedeflemek istediğiniz Android API düzeyi için bileşenleri yüklemek için sekmesinde (Android sürümleri ve API düzeyleri hakkında daha fazla bilgi için bkz. [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md)).

Tüm bileşenler bir platformun yüklü değilse, platform adının yanında bir onay işareti görünür. Kutu, platform için bir platformun tüm bileşenler yüklü değilse, doldurulur. 


Bileşenleri (ve hangi bileşenlerin yüklü) tıklayarak görmek için bir platform genişletebilirsiniz **+** platform solundaki kutusu.
Tıklayın **-** listelemek için bir platform bileşeni unexpand için.


Başka bir platform SDK eklemek için onay işaretine kadar platform yanındaki kutuyu görünen tüm bileşenleri, yüklemek için tıklatın ardından **Değişiklikleri Uygula**:


[![Örnek Android SDK'sı Android 7.1 Nougat bileşenleri ekleme](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)


Yüklemek için yalnızca SDK'yı tıklayın platform yanındaki kutuya bir kez. Gereksinim duyduğunuz tek tek bileşenlerin sonra seçebilirsiniz:


[![Örneğin bazı Android 7.1 bileşenler ekleme](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)




Yüklenecek bileşenleri sayısını yanındaki göründüğüne dikkat edin **Değişiklikleri Uygula** düğmesi. Yukarıdaki örnekte, altı bileşenlerini yüklemeye hazırsınız demektir. Tıkladıktan sonra **Değişiklikleri Uygula** düğmesi, göreceğiniz **lisans kabulü** ekran:



![Platform sekmesini lisans kabulü iletişim kutusu](android-sdk-images/win/11-license-screen.png)


Tıklayın **kabul** hüküm ve koşulları kabul ederseniz. Bu iletişim birden fazla kez görebilirsiniz yüklemek için birden çok bileşen olduğunda. Pencerenin en altında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. İndirme ve yükleme işlemi tamamlandığında (Bu, kaç bileşenlerin indirilmesi gereken bağlı olarak çok sayıda dakika sürebilir), eklenen bileşenler işaretiyle ve olarak listelenen **yüklü**.



# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


## <a name="requirements"></a>Gereksinimler

Xamarin Android SDK Yöneticisi'ni kullanmak için aşağıdakiler gerekir:

-   Visual Studio Mac 7.5 için (veya üzeri).

Xamarin Android SDK Yöneticisi ayrıca Java Development Kit (hangi Xamarin.Android ile otomatik olarak yüklenir) gerektirir.
Xamarin.Android kullanan [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), geliştirme için API düzeyi 24 veya daha büyük olması durumunda gerekli olduğu (JDK 8 de destekler API düzeylerini 24'dan önceki). Kullanmaya devam edebilirsiniz [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özel olarak geliştirme veya önceki bir sürümü kullanıyorsanız.

> [!IMPORTANT]
> Xamarin.Android JDK 9 desteklemez.

 
## <a name="sdk-manager"></a>SDK Yöneticisi 

SDK Yöneticisi, Mac için Visual Studio'da başlatmak için tıklatın **Araçlar > SDK Yöneticisi**:
 
![Android SDK Yöneticisi'ni menü öğesinin konumu](android-sdk-images/mac/sdkmanager-01.png )

**Android SDK Yöneticisi** açılır **tercihleri penceresini**, üç sekme içerir **platformları**, **Araçları**ve **Konumları**:

![Android SDK Yöneticisi'nin ekran görüntüsü platformları sekmede aç](android-sdk-images/mac/sdkmanager-02.png)

Xamarin Android SDK Yöneticisi'nin sekmeler, aşağıdaki bölümlerde açıklanmıştır.


### <a name="locations-tab"></a>Konumları sekmesi

**Konumları** Android SDK'sı, Android NDK ve Java SDK (JDK) konumları yapılandırmak için sekmesinde üç ayara sahiptir. Bu konumlar doğru önce yapılandırılmalıdır **platformları** ve **Araçları** sekmeleri düzgün çalışır.

SDK Yöneticisi yeniden başlatıldığında otomatik olarak yüklü her paket için yolu belirler ve olduğunu gösteren **bulunan** yolu yanında bir yeşil onay işareti simgesi yerleştirerek:

![Konumları sekmesinin Ekran görüntüsü](android-sdk-images/mac/sdkmanager-03.png)

Tıklayın **varsayılan ayarlarına geri döndürmeyi** SDK, NDK ve kendi varsayılan konumlarda JDK aramak için SDK Yöneticisi'ni neden düğme. 

Genellikle, kullandığınız **konumları** Android SDK ve/veya Java JDK konumunu değiştirmek için sekmesinde. Xamarin.Android uygulamaları geliştirmek için NDK yüklemenize gerek olmayan &ndash; NDK C ve C++ gibi yerel kodlu dilleri kullanarak uygulamanızın bölümlerini geliştirmeniz gerektiğinde kullanılır.

### <a name="tools-tab"></a>Araçlar sekmesi

**Araçları** sekmesi bir listesini görüntüler _Araçları_ ve _ek özellikler_. Android SDK araçlarının, platform araçları yüklemek için bu sekmeyi kullanın ve derleme araçları.
Ayrıca, Android öykünücüsü yükleyebilirsiniz alt düzey hata ayıklayıcı (LLDB) NDK HAXM hızlandırma ve Google Play kitaplıkları.


Örneğin, Google Android öykünücüsü paketi indirmek için onay işaretine yanındaki tıklayın **Android öykünücüsü** tıklatıp **Güncelleştirmeleri Yükle** düğmesi:

![Araçlar sekmesindeki Android öykünücüsü'nü yükleme](android-sdk-images/mac/sdkmanager-08.png)


Bir iletişim kutusu iletisi ile gösterilebilir _bazı bileşenler güncelleştirilebilir. Şimdi güncelleştirmesini istiyor musunuz?_ **Evet**'i tıklayın. Ardından, bir lisans kabulü iletişim kutusu gösterilir:


![Lisans kabulü ekranı](android-sdk-images/mac/sdkmanager-09.png)


Tıklayın **kabul** hüküm ve koşulları kabul ederseniz. Pencerenin en altında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. Yükleme tamamlandıktan sonra **Araçları** sekmesi seçili araçları ve ek özellikler yüklendiğini gösterir.



### <a name="platforms-tab"></a>Platformları sekmesi

**Platformları** sekmesi platform SDK sürümleri her platform için (sistem görüntüleri için gibi) diğer kaynaklar ile birlikte bir listesini görüntüler.


![Platformları bölmesinin ekran görüntüsü](android-sdk-images/mac/sdkmanager-11.png)


Android sürümü bu ekranı listeler (gibi **Android 7.0**), kod adı (**Nougat**), API düzeyi (gibi **24**) ve durumu (**yüklü** platform yüklediyseniz). Kullandığınız **platformları** hedeflemek istediğiniz Android API düzeyi için bileşenleri yüklemek için sekmesinde (Android sürümleri ve API düzeyleri hakkında daha fazla bilgi için bkz. [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md)).

Tüm bileşenler bir platformun yüklü değilse, platform adının yanında bir onay işareti görünür. Kutu, platform için bir platformun tüm bileşenler yüklü değilse, doldurulur. 


Bileşenleri (ve hangi bileşenlerin yüklü) tıklayarak görmek için bir platform genişletebilirsiniz **ok** sol tarafındaki platform.
Tıklayın **aşağı ok** listelemek için bir platform bileşeni unexpand için.


Başka bir platform SDK eklemek için onay işaretine kadar platform yanındaki kutuyu görünen tüm bileşenleri, yüklemek için tıklatın ardından **Değişiklikleri Uygula**:


![Örnek Android SDK'sı Android 4.4 bileşenleri ekleme](android-sdk-images/mac/sdkmanager-12.png)


Yüklemek için yalnızca SDK'yı tıklayın platform yanındaki kutuya bir kez. Gereksinim duyduğunuz tek tek bileşenlerin sonra seçebilirsiniz:


![Örneğin bazı Android 4.4 bileşenler ekleme](android-sdk-images/mac/sdkmanager-13.png)




Yüklenecek bileşenleri sayısını yanındaki göründüğüne dikkat edin **Değişiklikleri Uygula** düğmesi. Tıkladıktan sonra **Değişiklikleri Uygula** düğmesi, göreceğiniz **lisans kabulü** ekran:



![Platform sekmesini lisans kabulü iletişim kutusu](android-sdk-images/mac/sdkmanager-14.png)


Tıklayın **kabul** hüküm ve koşulları kabul ederseniz. Bu iletişim birden fazla kez görebilirsiniz yüklemek için birden çok bileşen olduğunda. Pencerenin en altında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. İndirme ve yükleme işlemi tamamlandığında (Bu, kaç bileşenlerin indirilmesi gereken bağlı olarak çok sayıda dakika sürebilir), eklenen bileşenler işaretiyle ve olarak listelenen **yüklü**.

-----

 
## <a name="summary"></a>Özet

Bu kılavuzda açıklanan yükleme ve Mac için Xamarin Android SDK Yöneticisi aracını Visual Studio ve Visual Studio


## <a name="related-links"></a>İlgili bağlantılar

- [Android SDK Aracı Üzerindeki Değişiklikler](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android API düzeylerini anlama](~/android/app-fundamentals/android-api-levels.md)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
