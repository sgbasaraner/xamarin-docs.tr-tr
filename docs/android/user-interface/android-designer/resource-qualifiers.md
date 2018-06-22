---
title: Kaynak niteleyicileri ve görselleştirme seçenekleri
description: Bu konuda, yalnızca bazı niteleyicisi değerler karşılandığında, kullanılacak kaynakları tanımlamak açıklanmaktadır. Basit bir örnek, bir dil tam dize kaynaktır. Bir dize kaynağı, varsayılan olarak, ek diller için kullanılacak tanımlı alternatif kaynaklar ile tanımlanabilir. Tüm kaynak türleri, Düzen dahil olmak üzere nitelendirilmesi.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: bc9eb145b6d9ed7bd441d625f533c5cbbd87fccd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771898"
---
# <a name="resource-qualifiers-and-visualization-options"></a>Kaynak niteleyicileri ve görselleştirme seçenekleri

_Bu konuda, yalnızca bazı niteleyicisi değerler karşılandığında, kullanılacak kaynakları tanımlamak açıklanmaktadır. Basit bir örnek, bir dil tam dize kaynaktır. Bir dize kaynağı, varsayılan olarak, ek diller için kullanılacak tanımlı alternatif kaynaklar ile tanımlanabilir. Tüm kaynak türleri, Düzen dahil olmak üzere nitelendirilmesi._


## <a name="custom-device-configurations"></a>Özel aygıt yapılandırmaları

Android aygıtlar ve ekran çözünürlükleri sayısız üzerinde kullanılabilir.
Birçok cihaz hedef tasarım kullanıcı arabirimleri yardımcı olmak için yerleşik cihaz yapılandırmalarını çeşitli tasarımcı birlikte gelir. Ayrıca, ek cihaz yapılandırmalarını ekleme destekler; Bu yapılandırmalar dayalı *niteleyicileri* bir aygıt yapılandırması diğerinden ayırt etmek için belirtin. Farklı türlerde niteleyicileri vardır. Bu kaynak türleri hakkında daha fazla bilgi için bkz: [Android kaynakları](~/android/app-fundamentals/resources-in-android/index.md).

Menü aygıt Seçici kısımda olduğu bir **Özelleştir** seçeneği aşağıda gösterildiği gibi:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cihaz Seçici menüsü](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Cihaz Seçici menüsü](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png#lightbox)

-----


Seçme **Özelleştir** bir iletişim kutusu görüntüler, kullanılabilir cihaz yapılandırmalarını tarama için kullanabilirsiniz. Tıkladığınızda **aygıt tanımları** sekmesinde tüm bilinen cihaz tanımları listesi sunulur:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![AVD Manager](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![AVD Manager](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png#lightbox)

-----


Tasarımcıda önceden yapılandırılmış aygıtları değiştirilemez. Ancak, tıklayabilirsiniz **cihaz oluştur...**  özel cihaz tanımını tanımlamak için. Alternatif olarak, varolan bir tanımını seçin ve tıklatın **kopya...**  yeni tanımı için başlangıç noktası olarak kullanmak için.
Örneğin, seçme **Nexus 5** tanımı ve tıklayarak **kopya...**  aşağıdaki iletişim kutusunu gösterir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kopya cihaz](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kopya cihaz](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png#lightbox)

-----


Sonraki ekran görüntüsünde, ad olarak değiştirilmesini **Nexus 5 özel** ve yeni bir özel cihaz tanımı oluşturmak için aygıt parametrelerini değiştirilebilir. Bu örnekte, **dikey** cihaz tanımı yalnızca yatay böylece devre dışı bırakılır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Özel aygıt](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Özel aygıt](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png#lightbox)

-----


Tıklatarak **kopya aygıt** artık yeni bir cihaz tanımı oluşturur **aygıt tanımları** listesi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Güncelleştirilmiş cihaz tanımları](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Güncelleştirilmiş cihaz tanımları](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png#lightbox)

-----


Her bir kullanıcı tarafından oluşturulan cihaz tanımı yeşil bir simge ile yukarıda gösterildiği gibi görüntülendiğine dikkat edin. Ne zaman döndürmek için **aygıt** Seçici menüsünde Yeni özel aygıt tanımı sunulur listesinin en üstteki bölümünde (, listesinde, IDE yeniden başlatmayı deneyin özel cihaz yapılandırmanızda bu görmüyorsanız):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Özel aygıt aygıt listesinde görüntülenir](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Özel aygıt aygıt listesinde görüntülenir](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png#lightbox)

-----


Bu aygıt yapılandırması seçerek daha önce (Bu durumda, yalnızca yatay modda) oluşturulan özelleştirmeleri uygun olması için Düzen değiştirir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Özel Aygıt kullanımda](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Özel Aygıt kullanımda](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png#lightbox)

-----



## <a name="resource-qualifier-options"></a>Kaynak niteleyicisi seçenekleri

**Kaynak niteleyicisi seçenekleri** sağındaki aşağı üçgen simgesini tıklatarak erişilebilir **aygıt yapılandırması** seçenekleri:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kaynak niteleyicisi seçenekleri](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kaynak niteleyicisi seçenekleri](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png#lightbox)

-----


Bu iletişim kutusunu aşağıdaki kaynak niteleyicileri için aşağı açılır menüler sunar:

-   **Dil** &ndash; görüntüler kullanılabilir dil kaynakları ve yeni dil/bölge kaynakları eklemek için bir seçenek sunar.

-   **UI modu** &ndash; listeleri görüntüleme modları (gibi **araba yerleştirme** ve **Masası yerleştirme**) düzeni yönergeleri yanı sıra.

Bu aşağı açılır menüler her burada seçin ve kaynak niteleyicileri (sonraki açıklandığı gibi) yapılandırmak yeni iletişim kutuları açılır.



### <a name="language"></a>Dil

**Dil** açılır menü tanımlanan kaynaklara sahip dilleri listeler (veya **tüm diller**, varsayılan). Ancak, yoktur ayrıca bir **dil/bölge Ekle...**  listesine yeni bir dil eklemek izin veren seçeneği:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dil/bölge ekleme](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Dil/bölge ekleme](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png#lightbox)

-----


Tıkladığınızda **dil/bölge Ekle...** , **Dil Seç** iletişim kutusunu açar kullanılabilir diller ve bölgeler açılır listesini görüntülemek için:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dillerin listesini](resource-qualifiers-images/vs/10-languages.png "dillerin listesi")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Dillerin listesi](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png#lightbox)

-----


Bu örnekte, biz seçtiniz **fr (Fransızca)** dil ve **BE** (Belçika) Fransızca bölgesel dialect için. Unutmayın **bölge** alan olduğundan isteğe bağlı birçok dilde belirli bölgeler için bakmadan belirtilebilir. Zaman **dil** açılır menü yeniden açıldığında, yeni eklenen dil/bölge kaynak görüntüler:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dil ve seçilen bölge](resource-qualifiers-images/vs/11-language-region-added.png "dil ve bölge seçilen")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Dil ve bölge seçilen](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png#lightbox)

-----


Yeni bir dil eklemek, ancak bu, eklenen bir dili artık sonraki açışınızda gösterilecek için yeni kaynaklar oluşturmayın projeyi açın unutmayın.



### <a name="ui-mode"></a>UI modu

Tıkladığınızda **UI modu** aşağı açılır menüsünde, modlarının bir listesi görüntülenir, gibi **Normal**, **araba yerleştirme**, **Masası yerleştirme**, **Televizyon**, **Gereci**, ve **izleme**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![UI modu menüsü](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Bu listede gece modları olan **değil gece** ve **gece**, Düzen yönergeleri izleyen **soldan sağa** ve **sağdan sola** (için hakkında bilgi **soldan sağa** ve **sağdan sola** seçenekleri bkz [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).
Son öğeleri **kaynak niteleyicisi seçenekleri** iletişim olan **ekranlar yuvarlak** (kullanmak için Android takmak ile) veya **değil yuvarlamak ekranları** (hepsini hakkında bilgi için ve gidiş olmayan ekranlarına bakın [düzenleri](https://developer.android.com/training/wearables/ui/layouts.html)).
Android UI modları hakkında daha fazla bilgi için bkz: [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![UI modu menüsü](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png#lightbox)

Bu listede gece modları olan **değil gece** ve **gece**, Düzen yönergeleri izleyen **soldan sağa** ve **sağdan sola**. Android UI modları hakkında daha fazla bilgi için bkz: [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Hakkında bilgi için **soldan sağa** ve **sağdan sola** seçenekleri bkz [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).

### <a name="round-screen"></a>Yuvarlak ekranı

Son öğenin **kaynak niteleyicisi seçenekleri** iletişim **ekran Round** menüsü. Bu menü ya da seçmenize izin verir **yuvarlamak ekranlar** (kullanmak için Android takmak ile) veya **dikdörtgen ekranlar**:

[![Yuvarlak ekranı menüsü](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png#lightbox)

-----



## <a name="action-bar-settings"></a>Eylem çubuğu ayarları

**Eylem çubuğu ayarları** simgedir Fırçası (Tema Düzenleyicisi) simgesinin solunda kullanılabilir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Eylem çubuğu ayarları](resource-qualifiers-images/vs/14-action-bar.png "Eylem çubuğu ayarları")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Eylem çubuğu ayarları](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png#lightbox)

-----


Bu simge, üç Eylem çubuğu modlarından birini seçmek için bir yol sağlayan bir iletişim popover açar:

-   **Standart** &ndash; ya da bir logo veya simgesini ve başlık metni ile isteğe bağlı bir alt başlık oluşur.

-   **Liste** &ndash; listesi Gezinti modu. Statik başlık metni yerine bu mod, gezinti etkinlik için bir liste menü sunar (yani, kullanıcıya bir açılır liste olarak sunulabilir).

-   **Sekmeleri** &ndash; sekmesini Gezinti modu. Statik başlık metni yerine bu mod, bir dizi etkinlik içinde gezinme için sekme sunar.



## <a name="themes"></a>Temalar

**Tema** açılır menü proje tanımlanan Temalar tümünün görüntüler. Seçme **daha Temalar** aşağıda gösterildiği gibi yüklü Android SDK, kullanılabilir tüm temalar listesini içeren bir iletişim kutusunu açar:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Daha fazla Temalar listesi](resource-qualifiers-images/vs/15-theme-menu-sml.png "daha Temalar listesi")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Daha fazla Temalar listesi](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png#lightbox)

-----


Bir tema seçildiğinde, yeni temayı etkisini göstermek için tasarım yüzeyine güncelleştirilir. Bu değişiklik kalıcı yapıldığını unutmayın yalnızca **Tamam** düğmesine tıklandığında **tema** iletişim. Bir tema seçildikten sonra onu dahil edilecek **tema** aşağı açılan menüsünde aşağıda gösterildiği gibi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Açık tema yayımlamıştır](resource-qualifiers-images/vs/16-light-theme.png "açık tema yayımlamıştır")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Açık tema artık kullanılabilir](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png#lightbox)

-----



## <a name="android-version"></a>Android sürüm

Android **sürüm** Seçici Tasarımcısı'nda düzeni işlemek için kullanılan Android sürümü ayarlar. Seçici projenin hedef framework sürümü ile uyumlu olan tüm sürümleri görüntüler:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android sürümlerinin listesi](resource-qualifiers-images/vs/17-android-version.png "Android listesi sürümleri")

Hedef framework sürümü projenin ayarları altında ayarlanabilir **Özellikler > Uygulama > Android sürümüyle derleme**. Hedef framework sürümü hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).

Araç kutusunda kullanılabilir pencere öğeleri kümesi projenin hedef framework sürümü tarafından belirlenir. Bu, aynı zamanda kullanılabilir özellikleri için geçerlidir **Özellikler penceresini**. Pencere öğeleri kullanılabilir listesi *değil* seçili değer tarafından belirlenen **sürüm** araç çubuğunun Seçici. Örneğin, Android 4.4 projenin hedef sürümü ayarlarsanız, proje gibi Android 6. 0'göründüğünü görmek için araç sürüm Seçici içinde Android 6.0 seçebilirsiniz ancak Android 6.0 belirli pencere öğeleri eklemek açamazsınız &ndash;  yine Android 4.4 kullanılabilir pencere öğeleri için sınırlı olacaktır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Android sürümlerinin listesi](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png#lightbox)

Hedef framework sürümü projenin ayarları altında ayarlanabilir **proje Seçenekleri > Yapı > Genel** bölümü. Hedef framework sürümü hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).

Araç kutusunda kullanılabilir pencere öğeleri kümesi projenin hedef framework sürümü tarafından belirlenir. Bu, aynı zamanda kullanılabilir özellikleri için geçerlidir **özelliği paneli**. Pencere öğeleri kullanılabilir listesi *değil* seçili değer tarafından belirlenen **sürüm** araç çubuğunun Seçici. Örneğin, Android 4.4 projenin hedef sürümü ayarlarsanız, proje gibi Android 6. 0'göründüğünü görmek için araç sürüm Seçici içinde Android 6.0 seçebilirsiniz ancak Android 6.0 belirli pencere öğeleri eklemek açamazsınız &ndash;  yine Android 4.4 kullanılabilir pencere öğeleri için sınırlı olacaktır.

-----
