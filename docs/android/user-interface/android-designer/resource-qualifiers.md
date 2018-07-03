---
title: Kaynak niteleyicileri ve görselleştirme seçenekleri
description: Bu konu, yalnızca bazı niteleyici değeri eşleştiğinde, kullanılacak kaynakları tanımlayan açıklanmaktadır. Basit bir örnek, bir dil tam dize kaynaktır. Bir dize kaynağı, varsayılan, ek diller için kullanılacak tanımlanan diğer alternatif kaynaklar ile tanımlanabilir. Düzen dahil olmak üzere tüm kaynak türleri nitelendirilmesi.
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/29/2018
ms.openlocfilehash: 7bc8c1066e557085c1bf34f77765edbb2259ba7a
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403305"
---
# <a name="resource-qualifiers-and-visualization-options"></a>Kaynak niteleyicileri ve görselleştirme seçenekleri

_Bu konu, yalnızca bazı niteleyici değeri eşleştiğinde, kullanılacak kaynakları tanımlayan açıklanmaktadır. Basit bir örnek, bir dil tam dize kaynaktır. Bir dize kaynağı, varsayılan, ek diller için kullanılacak tanımlanan diğer alternatif kaynaklar ile tanımlanabilir. Düzen dahil olmak üzere tüm kaynak türleri nitelendirilmesi._


## <a name="custom-device-configurations"></a>Özel cihaz yapılandırmaları

Android cihazlar ve ekran çözünürlükleri deseninizi oluşturmayı üzerinde kullanılabilir.
Birçok cihaz hedefleyen tasarım kullanıcı arabirimleri yardımcı olmak için tasarımcı çeşitli yerleşik cihaz yapılandırmaları ile birlikte gelir. Ek cihaz yapılandırmaları eklemeyi de destekler; Bu yapılandırmalar dayalı *niteleyicileri* bir cihaz yapılandırma diğerinden ayırt etmek için belirtin. Farklı türlerde niteleyicileri vardır. Bu kaynak türleri hakkında daha fazla bilgi için bkz. [Android kaynakları](~/android/app-fundamentals/resources-in-android/index.md).

Cihaz Seçici alt kısmındaki menü olduğu bir **Özelleştir** seçeneğini aşağıda gösterildiği gibi:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cihaz Seçici menüsü](resource-qualifiers-images/vs/01-device-selector-sml.png)](resource-qualifiers-images/vs/01-device-selector.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Cihaz Seçici menüsü](resource-qualifiers-images/xs/01-device-selector-sml.png)](resource-qualifiers-images/xs/01-device-selector.png#lightbox)

-----


Seçme **Özelleştir** bir iletişim kutusunu görüntüler, kullanılabilir cihaz yapılandırmaları göz atmak için kullanabilirsiniz. Tıkladığınızda **cihaz tanımları** sekmesinde, tüm bilinen cihaz tanımlarının bir listesi sunulur:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![AVD Manager](resource-qualifiers-images/vs/02-device-definitions-sml.png)](resource-qualifiers-images/vs/02-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![AVD Manager](resource-qualifiers-images/xs/02-device-definitions-sml.png)](resource-qualifiers-images/xs/02-device-definitions.png#lightbox)

-----


Önceden yapılandırılmış Tasarımcısı'nda cihazlar değiştirilemez. Ancak, tıklayabilirsiniz **cihaz oluştur...**  özel cihaz tanımını belirlemek için. Alternatif olarak, mevcut bir tanımı'nı seçin ve tıklayın **Kopyala...**  yeni bir tanımı için başlangıç noktası olarak kullanmak için.
Örneğin, seçme **Nexus 5** tanımı ve tıklayarak **Kopyala...**  aşağıdaki iletişim kutusu sunar:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cihaz kopyalama](resource-qualifiers-images/vs/03-clone-sml.png)](resource-qualifiers-images/vs/03-clone.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Cihaz kopyalama](resource-qualifiers-images/xs/03-clone-sml.png)](resource-qualifiers-images/xs/03-clone.png#lightbox)

-----


Sonraki ekran görüntüsünde, adı değiştirildi **Nexus 5 özel** ve yeni bir özel cihaz tanımı oluşturmak için cihaz parametreleri değiştirilir. Bu örnekte, **dikey** böylece cihaz tanımı yalnızca yatay devre dışı bırakıldı:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Özel cihaz](resource-qualifiers-images/vs/04-custom-sml.png)](resource-qualifiers-images/vs/04-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Özel cihaz](resource-qualifiers-images/xs/04-custom-sml.png)](resource-qualifiers-images/xs/04-custom.png#lightbox)

-----


Tıklayarak **kopya cihaz** artık yeni bir cihaz tanımı oluşturur **cihaz tanımları** listesi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Güncelleştirilmiş cihaz tanımları](resource-qualifiers-images/vs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/vs/05-updated-device-definitions.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Güncelleştirilmiş cihaz tanımları](resource-qualifiers-images/xs/05-updated-device-definitions-sml.png)](resource-qualifiers-images/xs/05-updated-device-definitions.png#lightbox)

-----


Her kullanıcı tarafından oluşturulmuş bir cihaz tanımı yukarıda da gösterildiği gibi yeşil bir simge ile görüntülendiğine dikkat edin. İçin döndüğünüzde **cihaz** Seçici menüsü, yeni özel cihaz tanımı sunulur listesinin en üst bölümünde (, liste, IDE yeniden başlatmayı deneyin, özel cihaz yapılandırma bu görmüyorsanız):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Özel cihaz cihaz listesinde görünür.](resource-qualifiers-images/vs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/vs/06-nexus-5-custom.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Özel cihaz cihaz listesinde görünür.](resource-qualifiers-images/xs/06-nexus-5-custom-sml.png)](resource-qualifiers-images/xs/06-nexus-5-custom.png#lightbox)

-----


Bu cihaz yapılandırması seçerek daha önce (Bu durumda, yalnızca yatay modda) oluşturulan özelleştirmeleri uygun düzenini değiştirir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Özel cihaz kullanımda](resource-qualifiers-images/vs/07-custom-in-use-sml.png)](resource-qualifiers-images/vs/07-custom-in-use.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Özel cihaz kullanımda](resource-qualifiers-images/xs/07-custom-in-use-sml.png)](resource-qualifiers-images/xs/07-custom-in-use.png#lightbox)

-----



## <a name="resource-qualifier-options"></a>Kaynak niteleyicisi seçenekleri

**Kaynak niteleyicisi seçenekleri** sağ tarafındaki üç noktaya tıklayarak erişilebilir **cihaz Yapılandırması** seçenekleri:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kaynak niteleyicisi seçenekleri](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kaynak niteleyicisi seçenekleri](resource-qualifiers-images/xs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt.png#lightbox)

-----


Bu iletişim kutusunda, aşağıdaki kaynak niteleyicilerini için aşağı açılır menüler sunar:

-   **Dil** &ndash; kullanılabilir dil kaynakları görüntüler ve yeni dil/bölge kaynakları eklemek için bir seçenek sunar.

-   **UI modu** &ndash; listelerini görüntülemek modları (gibi **araç içi yerleştirme** ve **Masaüstü yerleştirme**) düzeni yönergeleri yanı sıra.

Her biri bu aşağı açılır menüler, seçin ve kaynak niteleyicilerini (sonraki açıklandığı gibi) yapılandırabileceğiniz yeni iletişim kutuları açılır.



### <a name="language"></a>Dil

**Dil** açılır menüsünden tanımlanan kaynaklara sahip dilleri listeler (veya **tüm diller**, varsayılan değerdir). Ancak, de mevcuttur bir **dil/bölge Ekle...**  listeye yeni dil eklemenizi sağlayan seçeneği:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dil/bölge Ekle](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Dil/bölge Ekle](resource-qualifiers-images/xs/09-add-language-region-sml.png)](resource-qualifiers-images/xs/09-add-language-region.png#lightbox)

-----


Tıkladığınızda **dil/bölge Ekle...** , **dili seç** kullanılabilir diller ve bölgeler açılır listesini görüntülemek için iletişim kutusu açılır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dillerin listesini](resource-qualifiers-images/vs/10-languages.png "dillerin listesi")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Dillerin listesi](resource-qualifiers-images/xs/10-languages-sml.png)](resource-qualifiers-images/xs/10-languages.png#lightbox)

-----


Bu örnekte, seçtik **fr (Fransızca)** dil ve **BE** (Belçika) bölgesel diyalekti Fransızca için. Unutmayın **bölge** birçok dil özel bölgeler için bakmadan belirtildiği için alan isteğe bağlıdır. Zaman **dil** açılır menüsünden yeniden açıldığında, yeni eklenen dil/bölge kaynağı görüntüler:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dil ve bölge seçilen](resource-qualifiers-images/vs/11-language-region-added.png "seçilen dil ve bölge")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Dil ve bölge seçilen](resource-qualifiers-images/xs/11-language-region-added-sml.png)](resource-qualifiers-images/xs/11-language-region-added.png#lightbox)

-----


Yeni bir dil Ekle, ancak yeni kaynaklar için eklenen dil artık sonraki gösterilecek oluşturmayın projeyi açın unutmayın.



### <a name="ui-mode"></a>UI modu

Tıkladığınızda **UI modu** aşağı açılır menüsünde, modlarının bir listesi görüntülenir, gibi **Normal**, **araç içi yerleştirme**, **Masaüstü yerleştirme**, **Televizyon**, **Gereci**, ve **Watch**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![UI modu menüsü](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Bu liste gece modlarıdır **değil gece** ve **gece**Düzen yönergeleri çizgidir **soldan sağa** ve **sağdan sola** (için hakkında bilgi **soldan sağa** ve **sağdan sola** seçeneklerini görmek [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).
Son öğeler **kaynak niteleyicisi seçenekleri** iletişim kutusu **ekran yuvarlak** (için Android Wear ile kullanın) veya **değil yuvarlak ekranları** (hepsini hakkında bilgi için ve gidiş ekranlarına bakın [düzenleri](https://developer.android.com/training/wearables/ui/layouts.html)).
Android kullanıcı Arabirimi modları hakkında daha fazla bilgi için bkz. [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![UI modu menüsü](resource-qualifiers-images/xs/12-ui-mode-sml.png)](resource-qualifiers-images/xs/12-ui-mode.png#lightbox)

Bu liste gece modlarıdır **değil gece** ve **gece**Düzen yönergeleri çizgidir **soldan sağa** ve **sağdan sola**. Android kullanıcı Arabirimi modları hakkında daha fazla bilgi için bkz. [UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
Hakkında bilgi için **soldan sağa** ve **sağdan sola** seçeneklerini görmek [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).

### <a name="round-screen"></a>Yuvarlak ekran

Son öğenin **kaynak niteleyicisi seçenekleri** iletişim **ekran yuvarlak** menüsü. Bu menü ya da seçmenizi sağlar **Round ekranlarda** (için Android Wear ile kullanın) veya **dikdörtgen ekranların**:

[![Yuvarlak ekran menüsü](resource-qualifiers-images/xs/13-round-screen-sml.png)](resource-qualifiers-images/xs/13-round-screen.png#lightbox)

-----



## <a name="action-bar-settings"></a>Eylem çubuğu ayarları

**Eylem çubuğu ayarları** simgesi (Tema Düzenleyicisi) Fırçası simgesini solundaki kullanılabilir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Eylem çubuğu ayarları](resource-qualifiers-images/vs/14-action-bar.png "Eylem çubuğu ayarları")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Eylem çubuğu ayarları](resource-qualifiers-images/xs/13b-action-bar-sml.png)](resource-qualifiers-images/xs/13b-action-bar.png#lightbox)

-----


Bu simge, üç Eylem çubuğu modlarından birini seçmek için bir yol sağlayan bir iletişim popover açar:

-   **Standart** &ndash; ya da bir logo veya simgeyi ve Başlık metin ile isteğe bağlı bir alt başlık oluşur.

-   **Liste** &ndash; liste Gezinti modu. Statik başlık metnini yerine bu mod, bir etkinlik içinde gezinme için liste menüsü sunar (diğer bir deyişle, bu kullanıcıya bir açılan listedeki olarak sunulabilir).

-   **Sekmeleri** &ndash; sekme gezinme modu. Statik başlık metnini yerine bu mod, bir dizi etkinliği içinde gezinme sekmeleri sunar.



## <a name="themes"></a>Temalar

**Tema** temaları projede tanımlanan tüm açılan menü görüntüler. Seçme **daha Temalar** aşağıda gösterildiği gibi yüklü Android SDK'sından, kullanılabilir tüm temalar listesini içeren bir iletişim kutusu açılır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Daha fazla tema listesi](resource-qualifiers-images/vs/15-theme-menu-sml.png "daha Temalar listesi")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Daha fazla tema listesi](resource-qualifiers-images/xs/14-theme-menu-sml.png)](resource-qualifiers-images/xs/14-theme-menu.png#lightbox)

-----


Bir tema seçili olduğunda, tasarım yüzeyine yeni temayı etkisini göstermek için güncelleştirilir. Bu değişiklik kalıcı yapıldığını unutmayın yalnızca **Tamam** düğmesine tıklandığında **tema** iletişim. Bir tema seçildikten sonra dahil edilecek **tema** aşağı açılan menüsünde aşağıda gösterildiği gibi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Açık tema kullanıma sunuldu](resource-qualifiers-images/vs/16-light-theme.png "açık tema kullanıma sunuldu")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Açık tema kullanıma sunuldu](resource-qualifiers-images/xs/15-light-theme-sml.png)](resource-qualifiers-images/xs/15-light-theme.png#lightbox)

-----



## <a name="android-version"></a>Android sürümü

Android **sürüm** ayarlar Tasarımcısı'nda düzenini işlemek için kullanılan Android sürümü Seçici. Seçici proje hedef framework sürümü ile uyumlu olan tüm sürümlerini görüntüler:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android sürümlerinin listesi](resource-qualifiers-images/vs/17-android-version.png "Android listesi sürümleri")

Hedef framework sürümü proje ayarlarında ayarlanabilir **özellikleri > Uygulama > Android sürümünü kullanarak derle**. Hedef framework sürümü hakkında daha fazla bilgi için bkz: [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md).

Araç kutusunda kullanılabilir pencere öğeleri kümesi proje hedef framework sürümü tarafından belirlenir. Bu da kullanılabilir özellikleri için geçerlidir **Özellikler penceresi**. Kullanılabilir pencere öğeleri listesi *değil* seçilen değeri tarafından belirlendiği **sürüm** araç çubuğunun Seçici. Örneğin, projenin hedef sürümü için Android 4.4 ayarlarsanız, projeyi Android 6. 0'nasıl göründüğünü görmek için araç sürümü Seçici içinde Android 6.0 seçebilirsiniz ancak Android 6.0 sürümüne özgü pencere öğeleri eklemek mümkün olmayacaktır &ndash;  Android 4.4 içinde kullanılabilir olan bir pencere öğeleri sınırlıdır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Android sürümlerinin listesi](resource-qualifiers-images/xs/16-android-version-sml.png)](resource-qualifiers-images/xs/16-android-version.png#lightbox)

Hedef framework sürümü proje ayarlarında ayarlanabilir **proje Seçenekleri > derleme > Genel** bölümü. Hedef framework sürümü hakkında daha fazla bilgi için bkz: [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md).

Araç kutusunda kullanılabilir pencere öğeleri kümesi proje hedef framework sürümü tarafından belirlenir. Bu da kullanılabilir özellikleri için geçerlidir **özellik paneli**. Kullanılabilir pencere öğeleri listesi *değil* seçilen değeri tarafından belirlendiği **sürüm** araç çubuğunun Seçici. Örneğin, projenin hedef sürümü için Android 4.4 ayarlarsanız, projeyi Android 6. 0'nasıl göründüğünü görmek için araç sürümü Seçici içinde Android 6.0 seçebilirsiniz ancak Android 6.0 sürümüne özgü pencere öğeleri eklemek mümkün olmayacaktır &ndash;  Android 4.4 içinde kullanılabilir olan bir pencere öğeleri sınırlıdır.

-----
