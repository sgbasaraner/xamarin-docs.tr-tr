---
title: Xamarin.Android için Android SDK'sı ayarlama
description: Visual Studio Google'nın tek başına SDK Manager değiştiren bir Android SDK Manager içerir. Bu kılavuz SDK Manager Android SDK Araçları, platformlar ve geliştirme Xamarin.Android uygulamaları için gereken diğer bileşenleri indirmek için nasıl kullanılacağını açıklar.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 0af0ef56630103498041ad61f7c5ce900358b055
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732859"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Xamarin.Android için Android SDK'sı ayarlama

_Visual Studio Google'nın tek başına SDK Manager değiştiren bir Android SDK Manager içerir. Bu kılavuz SDK Manager Android SDK Araçları, platformlar ve geliştirme Xamarin.Android uygulamaları için gereken diğer bileşenleri indirmek için nasıl kullanılacağını açıklar._


## <a name="overview"></a>Genel Bakış

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu kılavuz yüklemek ve Windows'da Visual Studio için Xamarin Android SDK Yöneticisi'ni kullanmak nasıl açıklar (veya [Mac için](?tabs=vsmac)).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu kılavuz yüklemek ve Mac için Visual Studio için Xamarin Android SDK Yöneticisi'ni kullanmak nasıl açıklar (veya [Windows için](?tabs=vswin)).

> [!NOTE]
> Bu kılavuz yalnızca Visual Studio 2017 ve Visual Studio için Mac için geçerlidir  

-----

Xamarin Android SDK Manager Xamarin.Android uygulamanıza geliştirmek için gereken en son Android bileşenlerin karşıdan yardımcı olur.
Google'nın tek başına kullanım dışı bırakılmış SDK Manager değiştirir.

Neden yerine SDK'sı Android SDK ile birlikte gelen Yöneticisi Xamarin Android SDK Yöneticisi'ni kullanmak istiyor? Sürümünde Android SDK Araçları paketini 25.2.3, Google Android SDK korumak için yeni bir aracı kullanıma sunuldu. Bu yeni araç  **[sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)**, Android SDK'sı için tek başına UI Yöneticisi değiştiren bir komut satırı yardımcı programıdır. Bu nedenle, SDK Araçları sürüme (Android 8.0 için gereklidir) 26.0.1 güncelleştirirseniz veya üstü ve Android SDK'sını bir kullanıcı Arabirimi arabirimi üzerinden yönetmek devam etmek istiyorsanız, Xamarin Android SDK Yöneticisi'ni kullanmanız gerekir.

## <a name="requirements"></a>Gereksinimler

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin Android SDK Yöneticisi'ni kullanmak için aşağıdakiler gerekir:

- Visual Studio 2017 (Community, Professional veya Enterprise edition). Visual Studio 2017 15,5 veya sonraki bir sürümü gereklidir.

- Xamarin sürüm 4.5.0 için Visual Studio Araçları veya sonraki bir sürümü. 

Xamarin Android SDK Manager Visual Studio ile uyumlu değil
2015. Visual Studio 2015 kullanıcılarının Google Android SDK tarafından sağlanan SDK Yöneticisi Araçları kullanmanız gerekir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   Mac 7.0.0.3146 için Visual Studio (veya üstü).

-----

Xamarin Android SDK Yöneticisi ayrıca Java Geliştirme Seti (hangi Xamarin.Android ile otomatik olarak yüklenir) gerektirir.
Xamarin.Android kullanan [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), API düzeyi 24 için geliştirme veya daha büyük olduğunda gerekli olduğu (JDK 8 de destekler API düzeylerini 24'den önceki). Kullanmaya devam edebilirsiniz [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) API düzeyi 23 için özellikle geliştirme veya önceki bir sürümünü kullanıyorsanız.

> [!IMPORTANT]
> Xamarin.Android JDK 9 desteklemez.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installation"></a>Yükleme

Xamarin SDK Manager için Visual Studio 2017 yükleme sırasında eklenebilir. Visual Studio yüklediğinizde tıklatın **bileşenleri tek tek** ve ekranı aşağı kaydırarak **geliştirme etkinlikleri** bölümü.
Etkinleştirme **Xamarin SDK Manager** zaten işaretli değilse:

![Xamarin SDK Yöneticisi'nden bileşenleri tek tek etkinleştirme](android-sdk-images/win/01-sdk-manager-install.png)

Visual Studio 2017 zaten yüklü değilse bkz [değiştirmek Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/modify-visual-studio) ardından Visual Studio değiştirmek yönergeleri izleyin için Xamarin SDK Yöneticisi'ni etkinleştirmek için yukarıdaki yordamı. SDK Yöneticisi'ni güncelleştirmek için bir ileti görürseniz, Xamarin SDK Yöneticisi'ni yüklemek için bu yordamı kullanabilirsiniz.

Tıkladığınızda **Araçlar > Android > Android SDK Manager** (anlatıldığı yanında), Google Android SDK Manager yerine Xamarin Android SDK Manager başlatılacak. Xamarin Android SDK Manager'ı yüklemeden Google'nın tek başına Android SDK Yöneticisi'ni destekleyen Android SDK'ın önceki bir sürümünü kullanıyorsanız, bir çakışma oluşturmaz &ndash; , tek başına başlatabilirsiniz Google SDK Yöneticisi'nden dışında Android SDK'yı yönetmek için visual Studio.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
 
-----

 
## <a name="sdk-manager"></a>SDK Yöneticisi 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio SDK Yöneticisi'ni başlatmak için tıklatın **Araçlar > Android > Android SDK Manager**:

[![Android SDK Manager menü öğesi konumu](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

**Xamarin Android SDK Manager** açılır **Android SDK'lar ve Araçlar** ekran. Bu ekran iki sekme bulunur &ndash; **platformları** ve **Araçları**:

[![Android SDK Yöneticisi'nin ekran görüntüsü platformlar sekmesindeki açın](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**Android SDK'lar ve Araçlar** ekran aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmaktadır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio SDK Yöneticisi'ni başlatmak için tıklatın **Araçlar > SDK Manager**:
 
![Android SDK Manager menü öğesi konumu](android-sdk-images/mac/sdkmanager-01.png )

**Android SDK Manager** açılır **Tercihler penceresi**, üç sekme içerir **platformları**, **Araçları**ve **Konumları**:

![Android SDK Yöneticisi'nin ekran görüntüsü platformlar sekmesindeki açın](android-sdk-images/mac/sdkmanager-02.png)

Xamarin Android SDK Manager'ın Sekmeler aşağıdaki bölümlerde açıklanmıştır.

-----



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-location"></a>Android SDK'sı konumu

Android SDK'sı konumu en üstünde yapılandırılmıştır **Android SDK'lar ve Araçlar** önceki resimde görüldüğü gibi ekran. Bu konum doğru önce yapılandırılmalıdır **platformları** ve **Araçları** sekmeleri düzgün çalışır. Bir veya daha fazla aşağıdaki nedenlerden Android SDK'ın konumunu ayarlamanız gerekebilir:

1. Xamarin SDK Manager Android SDK'yı bulamadı. 

2. Bir alternatif (varsayılan olmayan) konumda Android SDK'yı yüklediniz. 

Android SDK'sı konumu ayarlamak için tıklatın &hellip; düğmesine sağ ucundaki **Android SDK konumu**. Bu açılır **klasöre Gözat** iletişim Android SDK'sı konumuyla gezinmek için kullanın. Aşağıdaki ekran görüntüsü, Android SDK'sındaki **Program Files (x86)\\Android** seçili olduğundan:

![Android sdk bulma klasöre Windows Gözat iletişim kutusunun ekran görüntüsü](android-sdk-images/win/05-browse-for-folder.png)

Tıkladığınızda **Tamam**, seçilen konumda yüklü Android SDK Xamarin Android SDK Yöneticisi yönetir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

### <a name="locations-tab"></a>Konumları sekmesi

**Konumları** sekmesi, Android SDK'sı, Android NDK ve Java SDK'sı (JDK) konumlarını yapılandırmak için üç ayarları vardır. Bu konumları doğru önce yapılandırılmalıdır **platformları** ve **Araçları** sekmeleri düzgün çalışır.

SDK Yöneticisi başladığında otomatik olarak yüklenen her paketin yolu belirler ve olup olmadığını gösterir **bulunan** yolu yanındaki yeşil onay işareti simgesine yerleştirerek:

![Konumları sekmesinin Ekran görüntüsü](android-sdk-images/mac/sdkmanager-03.png)

Tıklatın **varsayılan ayarlarına** SDK, NDK ve kendi varsayılan konumlara JDK aramak için SDK Yöneticisi'ni neden düğmesi. 

Genellikle, kullandığınız **konumları** Android SDK ve/veya Java JDK konumunu değiştirmek için sekmesi. Xamarin.Android uygulamaları geliştirmek için NDK yüklemek gerekmez &ndash; NDK C ve C++ gibi yerel kodlu dilleri kullanarak uygulamanızı bölümlerini geliştirmek gerektiğinde kullanılır.

-----


### <a name="tools-tab"></a>Araçlar sekmesi

**Araçları** sekmesini listesini görüntüler _Araçları_ ve _ek özellikler_. Oluşturma araçlarını ve Android SDK Araçları, platform araçlarını yüklemek için bu sekmeyi kullanın.
Ayrıca, Android öykünücüsü yükleyebilmek için alt düzey hata ayıklayıcı (LLDB) NDK HAXM hızlandırma ve Google Play kitaplıkları.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Örneğin, Google Android öykünücüsü paketini indirmek için onay işaretine yanındaki tıklayın **Android öykünücüsü** tıklatıp **değişikliklerini uygula** düğmesi:

[![Android öykünücüsünde araçları sekmesinden yükleme](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Örneğin, Google Android öykünücüsü paketini indirmek için onay işaretine yanındaki tıklayın **Android öykünücüsü** tıklatıp **Güncelleştirmeleri Yükle** düğmesi:

![Android öykünücüsünde araçları sekmesinden yükleme](android-sdk-images/mac/sdkmanager-08.png)

-----


Bir iletişim kutusu iletisi ile gösterilebilir _bazı bileşenleri güncelleştirilebilir. Şimdi güncelleştirmek istiyor musunuz?_ **Evet**'i tıklayın. Ardından, bir lisans kabulü iletişim kutusu gösterilir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Lisans kabulünü ekranı](android-sdk-images/win/07-license-acceptance.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Lisans kabulünü ekranı](android-sdk-images/mac/sdkmanager-09.png)

-----

Tıklatın **kabul** hüküm ve koşulları kabul ediyorsanız. Pencerenin alt kısmında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. Yükleme tamamlandıktan sonra **Araçları** sekmesi seçili araçları ve ek özellikler yüklendiğini gösterir.



### <a name="platforms-tab"></a>Platformlar sekmesi

**Platformları** sekmesi her platform için (sistem görüntüleri için gibi) diğer kaynaklar birlikte platform SDK sürümlerinin listesini görüntüler.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Platformlar bölmesinin ekran görüntüsü](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Platformlar bölmesinin ekran görüntüsü](android-sdk-images/mac/sdkmanager-11.png)

-----

Bu ekran Android sürümü listeler (gibi **Android 7.0**), kod adı (**Nougat**), API düzeyini (gibi **24**) ve durumu (**yüklü** platform yüklediyseniz). Kullandığınız **platformları** hedeflemek istediğiniz Android API düzeyi için bileşenlerini yüklemek için sekme (Android sürümleri ve API düzeyleri hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md)).

Bir platformun tüm bileşenlerin yüklü olduğundan ve platform adının yanındaki onay işareti görüntülenir. Bir platformun tüm bileşenler yüklü değilse, platform için kutu doldurulur. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bileşenlerinden (ve hangi bileşenlerin yüklü) tıklayarak görmek için bir platform genişletebilirsiniz **+** platform solundaki kutusunu.
Tıklatın **-** için bir platform listeleme bileşen unexpand için.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bileşenlerinden (ve hangi bileşenlerin yüklü) tıklayarak görmek için bir platform genişletebilirsiniz **ok** platform solundaki.
Tıklatın **aşağı ok** için bir platform listeleme bileşen unexpand için.

-----

Başka bir platform SDK eklemek için onay işaretine kadar platform yanındaki görünür tüm bileşenleri, yüklemek için tıklatın ardından **değişikliklerini uygula**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Örnek Android SDK'sı Android 7.1 Nougat bileşenleri ekleme](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Örnek Android SDK'sı Android 4.4 bileşenleri ekleme](android-sdk-images/mac/sdkmanager-12.png)

-----

Yalnızca SDK tıklatın platform yanındaki kutuya bir kez yüklemek için. Ardından, gereken tüm bileşenleri tek tek seçebilirsiniz:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Bazı Android 7.1 bileşenleri ekleme örneği](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Bazı Android 4.4 bileşenleri ekleme örneği](android-sdk-images/mac/sdkmanager-13.png)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Yüklenecek bileşenleri sayısı görünür bildirimi **değişikliklerini uygula** düğmesi. Yukarıdaki örnekte, altı bileşenleri yüklemek hazır olursunuz. Tıklattıktan sonra **değişikliklerini uygula** düğmesi, göreceğiniz **lisans kabulünü** ekran:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Yüklenecek bileşenleri sayısı görünür bildirimi **değişikliklerini uygula** düğmesi. Tıklattıktan sonra **değişikliklerini uygula** düğmesi, göreceğiniz **lisans kabulünü** ekran:

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Platform lisans kabulünü sekmesi](android-sdk-images/win/11-license-screen.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Platform lisans kabulünü sekmesi](android-sdk-images/mac/sdkmanager-14.png)

-----

Tıklatın **kabul** hüküm ve koşulları kabul ediyorsanız. Bu iletişim birden fazla kez görebilirsiniz yüklemek için birden çok bileşen olduğunda. Pencerenin alt kısmında bir ilerleme çubuğu indirme ve yükleme ilerlemesini gösterir. İndirme ve yükleme işlemi tamamlandığında (Bu, kaç tane bileşenleri yüklenmesine gerek bağlı olarak birkaç dakika sürebilir), eklenen bileşenleri işaretiyle ve olarak listelenen **yüklü**.

Artık uygulamanız için en son, en büyük Android API düzeyini geliştirmek hazırsınız!


 
## <a name="summary"></a>Özet

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu kılavuz, aracı yükleme ve Xamarin Android SDK Manager Visual Studio'da kullanma anlatılmıştır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu kılavuz Xamarin Android SDK Manager aracının Visual Studio'da Mac için nasıl kullanılacağı açıklanmıştır

-----


## <a name="related-links"></a>İlgili bağlantılar

- [Android SDK Aracı Üzerindeki Değişiklikler](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Android API düzeylerini anlama](~/android/app-fundamentals/android-api-levels.md)
- [Sürüm Notları (Google) SDK Araçları](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
