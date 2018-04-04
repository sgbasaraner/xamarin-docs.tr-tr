---
title: Tasarımcı temelleri
description: Bu konuda Tasarımcısı özellikleri tanıtır Tasarımcısı'nı başlatmak açıklanmaktadır, tasarım yüzeyine açıklar ve pencere öğesi özelliklerini düzenlemek için Özellikler bölmesi kullanmayı ayrıntıları.
ms.prod: xamarin
ms.assetid: 48B20C9A-B2A2-AE82-76B2-A3C1E5A4050D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 6bac16a8ce9859e819299689489d9aad982c1f7f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="designer-basics"></a>Tasarımcı temelleri

_Bu konuda Tasarımcısı özellikleri tanıtır Tasarımcısı'nı başlatmak açıklanmaktadır, tasarım yüzeyine açıklar ve pencere öğesi özelliklerini düzenlemek için Özellikler bölmesi kullanmayı ayrıntıları._


## <a name="launching-the-designer"></a>Tasarımcısı'nı başlatma

Tasarımcı bir düzen oluşturulduğunda veya var olan bir .axml dosyasını çift tıklatarak başlatılabilir otomatik olarak başlatılır. Örneğin, çift **Main.axml** içinde **Kaynakları > düzeni** klasörü, Tasarımcı yükler, aşağıda gösterildiği gibi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Visual Studio'da Tasarımcı ekranı](designer-basics-images/vs/01-open-designer-sml.png)](designer-basics-images/vs/01-open-designer.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Mac için Visual Studio'da Tasarımcı ekranı](designer-basics-images/xs/01-open-designer-sml.png)](designer-basics-images/xs/01-open-designer.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Benzer şekilde, yeni bir düzen sağ tıklayarak ekleyebileceğiniz **düzeni** klasöründe **Çözüm Gezgini** ve seçerek **Ekle > Yeni öğe... > Android düzeni**:

[![Yeni öğe iletişim ekleyin](designer-basics-images/vs/02-add-new-layout-sml.png)](designer-basics-images/vs/02-add-new-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Benzer şekilde, yeni bir düzen sağ tıklayarak ekleyebileceğiniz **düzeni** klasöründe **çözüm paneli** ve seçerek **Ekle > Yeni Dosya > Android > düzeni**:

[![Yeni dosya iletişim kutusu ekleme](designer-basics-images/xs/02-add-new-layout-sml.png)](designer-basics-images/xs/02-add-new-layout.png#lightbox)

-----

Bu yeni bir .axml dosyası oluşturur ve tasarım yüzeyine yükler.



## <a name="designer-features"></a>Tasarımcısı özellikleri

Tasarımcı aşağıdaki ekran görüntüsünde gösterildiği gibi çeşitli özelliklerini destekleyen çeşitli bölümlerini oluşur:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tasarımcısı bölmeleri diyagramı](designer-basics-images/vs/03-designer-features-sml.png)](designer-basics-images/vs/03-designer-features.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tasarımcısı bölmeleri diyagramı](designer-basics-images/xs/03-designer-features-sml.png)](designer-basics-images/xs/03-designer-features.png#lightbox)

-----

Bir düzen Tasarımcısı'nda düzenlediğinizde, oluşturmak ve tasarımınızı şekil için aşağıdaki özellikleri kullanın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   **Tasarım yüzeyini** &ndash; düzeni cihazda nasıl görüneceğini düzenlenebilir bir temsilini sağlayarak kullanıcı arabirimi, görsel yapımı kolaylaştırır.

-   **Araç çubuğu** &ndash; Seçici listesini görüntüler: **aygıt**, **sürüm**, **tema**, Düzen yapılandırması ve Eylem çubuğu ayarları. Araç ayrıca tema Düzenleyicisi'ni başlatma ve etkinleştirme malzeme Tasarım Kılavuzu simgelerini içerir.

-   **Araç kutusu** &ndash; pencere öğeleri ve sürükle ve bırak tasarım yüzeyine düzenleri listesini sağlar.

-   **Özellikler penceresi** &ndash; görüntülemek ve düzenlemek için seçilen pencere öğesinin özellikleri listeler.

-   **Belge Anahattı** &ndash; düzenini oluşturma pencere öğeleri ağacı görüntüler. Tasarımcıda seçilmesini neden ağacındaki bir öğeyi tıklatabilirsiniz. Ayrıca, ağacındaki bir öğeyi tıklatarak öğenin özelliklerini özellikleri penceresine yükler.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   **Tasarım yüzeyini** &ndash; düzeni cihazda nasıl görüneceğini düzenlenebilir bir temsilini sağlayarak kullanıcı arabirimi, görsel yapımı kolaylaştırır.

-   **Araç çubuğu** &ndash; Seçici listesini görüntüler: **aygıt**, **sürüm**, **tema**, Düzen yapılandırması ve Eylem çubuğu ayarları. Araç ayrıca tema Düzenleyicisi'ni başlatma ve etkinleştirme malzeme Tasarım Kılavuzu simgelerini içerir.

-   **Araç kutusu** &ndash; pencere öğeleri ve sürükle ve bırak tasarım yüzeyine düzenleri listesini sağlar.

-   **Özellik paneli** &ndash; görüntülemek ve düzenlemek için seçilen pencere öğesinin özellikleri listeler.

-   **Belge Anahattı** &ndash; düzenini oluşturma pencere öğeleri ağacı görüntüler. Tasarımcıda seçilmesini neden ağacındaki bir öğeyi tıklatabilirsiniz. Ayrıca, ağacındaki bir öğeyi tıklatarak öğenin özelliklerini özellik defteri yükler.

-----



## <a name="toolbar"></a>Araç Çubuğu

Yapılandırma seçicileri ve aracı menüleri (tasarım yüzeyi konumlandırılmış) araç sunar:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tasarımcı araç diyagramı](designer-basics-images/vs/04-toolbar-sml.png)](designer-basics-images/vs/04-toolbar.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tasarımcı araç diyagramı](designer-basics-images/xs/04-toolbar-sml.png)](designer-basics-images/xs/04-toolbar.png#lightbox)

-----


Araç çubuğu erişim aşağıdaki özellikleri sağlar:

-   **Alternatif düzeni Seçici** &ndash; farklı düzen sürümlerden seçmenize olanak sağlar.

-   **Cihaz Seçici** &ndash; ekran boyutu, çözümleme ve klavye kullanılabilirlik gibi belirli bir aygıt ile ilişkili niteleyicileri kümesini tanımlar. Ayrıca, ekleyebilir ve yeni cihazları silin.

-   **Android sürüm Seçici** &ndash; düzeni hedefleme Android sürümü. Tasarımcı seçili Android sürümü göre düzeni işlemez.

-   **Tema Seçici** &ndash; düzeni için kullanıcı Arabirimi tema seçer.

-   **Yapılandırma Seçici** &ndash; aygıt yapılandırması gibi seçer *dikey* veya *yatay*.

-   **Kaynak niteleyicisi seçenekleri** &ndash; seçmek için aşağı açılır menüler sunan bir iletişim kutusu açılır *dil*, *UI modu*, *gece modu*, ve *Yuvarlak ekran* seçenekleri.

-   **Eylem çubuğunda ayarları** &ndash; düzeni için Eylem çubuğu ayarları yapılandırır.

-   **Tema Düzenleyicisi** &ndash; açılır *tema Düzenleyicisi*, hangi mümkün kılar seçilen tema öğelerini özelleştirmek için.

-   **Malzeme Tasarım Kılavuzu** &ndash; etkinleştirir veya devre dışı bırakır *malzeme Tasarım Kılavuzu*. Açılan menü öğesi malzeme tasarım kılavuzuna bitişik kılavuz özelleştirmenize olanak tanıyan bir iletişim kutusunu açar.

Bu özelliklerin her biri şu konularda daha ayrıntılı açıklanmıştır:

[Kaynak niteleyicileri ve görselleştirme seçenekleri](~/android/user-interface/android-designer/resource-qualifiers.md) hakkında ayrıntılı bilgi sağlar **aygıt Seçici**, **Android sürümü Seçici**, **tema Seçici**, **Yapılandırma Seçici**, **kaynak nitelikleri seçenekleri**, ve **Eylem çubuğu ayarları**.

[Alternatif Düzen görünümleri](~/android/user-interface/android-designer/alternative-layout-views.md) nasıl kullanılacağı açıklanmaktadır **alternatif düzeni Seçici**.

[Malzeme tasarım özellikleri](~/android/user-interface/android-designer/material-design-features.md) kapsamlı bir genel bakış sağlar **tema Düzenleyicisi** ve **malzeme Tasarım Kılavuzu**.



## <a name="design-surface"></a>Tasarım yüzeyi

Tasarımcı sürükleyip pencere öğeleri tasarım yüzeyine araç sağlar. (Yeni pencere öğeleri ekleme veya var olanları yeniden konumlandırma) Tasarımcısı'nda pencere öğeleri ile etkileşim kurduklarında, dikey ve yatay çizgiler kullanılabilir ekleme noktaları işaretlemek için görüntülenir. Aşağıdaki örnekte, yeni bir `Button` pencere öğesi, tasarım yüzeyine sürüklenmekte:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Örnek ekleme satırlarında tasarım yüzeyi](designer-basics-images/vs/05-insertion-points-sml.png)](designer-basics-images/vs/05-insertion-points.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Örnek ekleme satırlarında tasarım yüzeyi](designer-basics-images/xs/05-insertion-points-sml.png)](designer-basics-images/xs/05-insertion-points.png#lightbox)

-----

Ayrıca, pencere öğeleri kopyalanabilir: kopya kullanabilirsiniz ve bir pencere öğesi veya kopyalamak için Yapıştır sürükleyip bırakabilirsiniz tuşlarına basarak sırasında var olan bir pencere öğesi <kbd>Ctrl</kbd> anahtarı.


### <a name="context-menu-commands"></a>Bağlam menü komutları

Bir bağlam menüsü tasarım yüzeyine hem belge anahattı de kullanılabilir. Bu menüsü seçili pencere öğesi ve (olan her zaman tasarım yüzeyine seçmek kolay değil) kapsayıcıları üzerinde işlem gerçekleştirmek kolaylaştırma kapsayıcısı için kullanılabilir komut görüntüler. Bir bağlam menüsündeki bir örneği burada verilmiştir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tasarım yüzeyine sağ tıklattığınızda örnek bağlam menüsü](designer-basics-images/vs/06-context-menu-sml.png)](designer-basics-images/vs/06-context-menu.png#lightbox)

Bu örnekte, sağ tıklayarak bir `TextView` çeşitli seçenekler sunan bir bağlam menüsü açılır:

-   **LinearLayout** &ndash; düzenlemek için bir alt menü açılır `LinearLayout` üst `TextView`.

-   **Silme**, **kopya**, ve **Kes** &ndash; sağ için uygulanan işlemler `TextView`.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tasarım yüzeyine sağ tıklattığınızda örnek bağlam menüsü](designer-basics-images/xs/06-context-menu-sml.png)](designer-basics-images/xs/06-context-menu.png#lightbox)

Bu örnekte, sağ tıklayarak bir `TextView` çeşitli seçenekler sunan bir bağlam menüsü açılır:

-   **RelativeLayout** &ndash; düzenlemek için bir alt menü açılır `RelativeLayout` üst `TextView`.

-   **LinearLayout** &ndash; düzenlemek için bir alt menü açılır `LinearLayout` üst `TextView`.

-   **Silme**, **kopya**, ve **Kes** &ndash; sağ için uygulanan işlemler `TextView`.

-----

Bu örnekte, sağ tıklayarak bir `TextView` çeşitli seçenekler sunan bir bağlam menüsü açılır:

-   **LinearLayout** &ndash; düzenlemek için bir alt menü açılır `LinearLayout` üst `TextView`.

-   **Silme**, **kopya**, ve **Kes** &ndash; sağ için uygulanan işlemler `TextView`.



### <a name="zoom-controls"></a>Yakınlaştırma denetimleri

Tasarım yüzeyine gösterildiği gibi birkaç denetimleri yakınlaştırma destekler:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tasarım yüzeyi yakınlaştırma denetimlerini diyagramı](designer-basics-images/vs/07-zoom-controls-sml.png)](designer-basics-images/vs/07-zoom-controls.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tasarım yüzeyi yakınlaştırma denetimlerini diyagramı](designer-basics-images/xs/07-zoom-controls-sml.png)](designer-basics-images/xs/07-zoom-controls.png#lightbox)

-----

Bu denetimler kullanıcı arabiriminin Tasarımcısı'nda belirli alanları görmek kolaylaştırır:

-   **Kapsayıcıları vurgulayın** &ndash; yakınlaştırma ve uzaklaştırma sırasında bulmak daha kolay; böylece tasarım yüzeyine kapsayıcılarında vurgular.

-   **Normal Boyut** &ndash; düzeni Seçilen aygıt çözünürlükte nasıl görüneceğine görebilmeniz için Düzen piksel için-piksel işler.

-   **Penceresine sığacak** &ndash; yakınlaştırma düzeyini ayarlar, böylece tüm düzeni tasarım yüzeyine görülebilir.

-   **Yakınlaştırma** &ndash; düzeni büyütme, her bir tıklatmayla artımlı olarak yakınlaşır.

-   **Uzaklaştır** &ndash; tasarım yüzeyine daha küçük görünür düzeni yapma, her bir tıklatmayla artımlı olarak uzaklaştırır.

Seçilen yakınlaştırma ayarını uygulamanın çalışma zamanında kullanıcı arabiriminin etkilemez unutmayın.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="property-pad"></a>Özellik paneli

Tasarımcı aracılığıyla pencere öğesi özelliklerini düzenlemeyi destekler **özelliği paneli**. Tasarımcı yüzeyine seçili pencere öğesi bağlı olarak özellik paneli değişiklik listelenen özellikleri. Zaman `Button` önceki örnekte seçildiğinde, söz konusu özellikleri `Button` pencere öğesi gösterilir:

[![Özellik doldurma ekran görüntüsü](designer-basics-images/xs/08-property-pad-sml.png)](designer-basics-images/xs/08-property-pad.png#lightbox)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="properties-window"></a>Özellikler Penceresi

Tasarımcı aracılığıyla pencere öğesi özelliklerini düzenlemeyi destekler **Özellikler penceresini**. Tasarım yüzeyine seçili pencere öğesi bağlı olarak Özellikler penceresi değişiklik listelenen özellikleri.
Zaman `Button` önceki örnekte seçildiğinde, söz konusu özellikleri `Button` pencere öğesi gösterilir:

![Penceresinin ekran görüntüsü](designer-basics-images/vs/08-property-pad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="property-pad-sections"></a>Özellik paneli bölümleri

Özellik paneli benzer özellikleri gruplamak çeşitli bölümlere ayrılmıştır &ndash; bu ilgi özellikleri bulmak kolaylaştırır:

-   **Pencere öğesi** &ndash; pencere, ana özelliklerini gibi `id`, `visibility`, `text`vb. Pencere öğesi'nin içeriği yönetmek için özellikleri genellikle burada yerleştirilir.

-   **Stil** &ndash; pencere öğesi görsel görünümünü değiştirmeyi özellikleri `font`, `text color`, `background`vb.

-   **Düzen** &ndash; pencere boyutunu ve konumunu ayarlayın özellikleri.

-   **Kaydırma** &ndash; özellikleri kaydırma.

-   **Davranış** &ndash; pencere öğesi nasıl davranacağını ayarlamak bayrakları.

-----



### <a name="default-values"></a>Varsayılan değerler

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Çoğu pencere öğeleri özelliklerini boş **özellikleri** penceresi değerlerine seçili Android temayı devraldığı.
**Özellikleri** penceresinde yalnızca seçili pencere öğesi için açık olarak ayarlanmış olan değerler Göster; temayı devralınan değerleri göstermez.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Çoğu pencere öğeleri özelliklerini boş **özelliği paneli** değerlerine seçili Android temayı devraldığı. **Özelliği paneli** yalnızca seçilen pencere öğesi için açık olarak ayarlanıp değerleri gösterir; temayı devralınan değerleri göstermez.

-----


### <a name="referencing-resources"></a>Başvuru kaynakları

Bazı özellikler düzeni dışında dosyalarında tanımlanan kaynakları başvurabilir **.axml** dosya. Bu tür en yaygın örnekleri `string` ve `drawable` kaynakları. Ancak, başvuruları ayrıca diğer kaynaklar için aşağıdaki gibi kullanılabilir `Boolean` değerleri ve boyutları.
Ne zaman bir özelliğini destekleyen kaynak başvuruları, Gözat simgesi (üç nokta, bir &hellip;) özelliği için metin girişi yanındaki gösterilir.
Bu düğme tıklatıldığında kaynak seçici açar.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Örneğin, aşağıdaki ekran görüntüsünde metin alanının sağındaki üç nokta tıklatıldığında kullanılabilir kaynakları gösterir bir `Button` pencere öğesinde **özellikleri** penceresi:

[![İki kaynak listelenir örnek kaynaklar ekran görüntüsü](designer-basics-images/vs/09-resources-sml.png)](designer-basics-images/vs/09-resources.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Örneğin, aşağıdaki ekran görüntüsünde metin alanının sağındaki üç nokta tıklatıldığında kullanılabilir kaynakları gösterir bir `Button` pencere öğesinde **özelliği paneli**:

[![İki kaynak listelenir örnek kaynaklar ekran görüntüsü](designer-basics-images/xs/09-resources-sml.png)](designer-basics-images/xs/09-resources.png#lightbox)

-----

Sonraki örnekte kaynak seçicisini gösterilmektedir `Src` özelliği bir `ImageView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kaynak Seçici simgesi kaynak için bir ImageView listeleme](designer-basics-images/vs/10-src-resource-sml.png)](designer-basics-images/vs/10-src-resource.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kaynak Seçici simgesi kaynak için bir ImageView listeleme](designer-basics-images/xs/10-src-resource-sml.png)](designer-basics-images/xs/10-src-resource.png#lightbox)

-----



### <a name="boolean-property-references"></a>Boolean özelliği başvuruları

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

*Boolean* özellikleri Özellikleri penceresinde aşağı açılır menüden normalde seçilme. Seçebileceğiniz bir `true` veya `false` değeri veya seçim yapabileceğiniz bir özellik referansı tıklayarak **kaynak seçin...** . Bir değer gibi doğrudan da girin `true` veya `false`.

![Boole özellikleri ayarlama örneği](designer-basics-images/vs/11-boolean.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

*Boolean* özellikleri bir onay kutusu özelliği panelinde olarak normal olarak gösterilir. Zaman bir `Boolean` özelliğini destekleyen kaynak başvuruları, özellik yanında küçük bir onay kutusu görüntülenir. İşaretli onay kutusu anlamına gelir `true` ve boş bir kutunun anlamına gelir `false`. Bir değer gibi doğrudan da girin `true` veya `false`. Fare giriş getirildiğinde, bir küçük metin alanı simgesi getirir. Değeri el ile girebilirsiniz isterseniz, üzerinde tıklatabilirsiniz.

[![Boole özellikleri ayarlama örneği](designer-basics-images/xs/12-boolean-sml.png)](designer-basics-images/xs/12-boolean.png#lightbox)


## <a name="grouped-properties"></a>Gruplandırılmış özellikleri

Bazı pencere öğeleri birlikte gruplandırılmış birden çok değerli özelliklere sahip (gibi `Padding`, örneğin). Bu özellik değerlerini listelenen **özelliği paneli** genişletilebilir, tek bir satırda. Bu özelliklerden bazıları gibi doğrudan gruplandırılmış satırda düzenlenebilir `Padding` aşağıda gösterilen özelliği:

[![Padding özelliği için örnek ayarları](designer-basics-images/xs/13-padding-property-sml.png)](designer-basics-images/xs/13-padding-property.png#lightbox)

-----


## <a name="editing-properties-inline"></a>Özellikler satır içi düzenleme

Android Tasarımcısı'nı (özellik listesinde bu özellikleri için arama gerekmez) doğrudan tasarım yüzeyine belirli özelliklerin düzenlemeyi destekler. Metin, kenar boşluğu ve boyutu doğrudan düzenlenebilir özellikler içerir.

### <a name="text"></a>Metin

Bazı pencere öğeleri metin özelliklerini (gibi `Button` ve `TextView`), doğrudan tasarım yüzeyine düzenlenebilir. Bir pencere öğesi çift, aşağıda gösterildiği gibi düzenleme moduna sokar:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Metin kaynak hello dize](designer-basics-images/vs/12-text-resource.png "metin kaynak")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Metin kaynak hello dize](designer-basics-images/xs/14-text-resource-sml.png)](designer-basics-images/xs/14-text-resource.png#lightbox)

-----

Yeni bir metin değeri girin veya yeni bir kaynak dizesi girebilirsiniz. Aşağıdaki örnekte, `@string/hello` kaynak değiştirilir metinle `CLICK THIS BUTTON`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![SHIFT + Enter metin için yeni bir kaynak otomatik olarak bağlamak için](designer-basics-images/vs/13-shift-enter-resource.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![SHIFT + Enter metin için yeni bir kaynak otomatik olarak bağlamak için](designer-basics-images/xs/15-shift-enter-resource-sml.png)](designer-basics-images/xs/15-shift-enter-resource.png#lightbox)

-----

Bu değişiklik pencere öğesinde 's depolanan `text` özellik; atanan değer değiştirmez `@string/hello` kaynak.
Yeni bir metin dizesinde anahtar zaman basabilirsiniz <kbd>Shift</kbd> +
<kbd>Enter</kbd> otomatik olarak yeni bir kaynak için girilen metin bağlamak için.


### <a name="margin"></a>Kenar boşluğu

Bir pencere öğesini seçtiğinizde, Tasarımcı boyutunu veya pencere öğesinin kenar boşluğu etkileşimli olarak değiştirmenize izin tanıtıcıları görüntüler. Seçildiğinde pencere öğesini tıklatarak kenar boşluğu düzenleme moduna ve boyutu düzenleme modu arasında geçiş yapar.

İlk kez bir pencere öğesi tıkladığınızda, kenar boşluğu tanıtıcıları görüntülenir. Fare tanıtıcıları birine taşırsanız, tanıtıcı değiştirecek özelliği Designer görüntüler (için aşağıda gösterildiği gibi `layout_marginLeft` özellik):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Kenar boşluğu gösteren ekran Tasarımcısı'nda işleme](designer-basics-images/vs/15-margin-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kenar boşluğu gösteren ekran Tasarımcısı'nda işleme](designer-basics-images/xs/16-margin-handles-sml.png)](designer-basics-images/xs/16-margin-handles.png#lightbox)

-----

Kenar boşluğu zaten ayarlanmışsa kenar kapladığı alanı belirten noktalı çizgiler görüntülenir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Noktalı bir düğme boşluk işaretleme çizgiler örneği](designer-basics-images/vs/16-margins-set.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Noktalı bir düğme boşluk işaretleme çizgiler örneği](designer-basics-images/xs/17-margins-set-sml.png)](designer-basics-images/xs/17-margins-set.png#lightbox)

-----



### <a name="size"></a>Boyut

Daha önce belirtildiği gibi bir pencere öğesi seçildiğinde ' ı tıklatarak boyutu düzenleme moduna geçiş yapabilirsiniz. Belirtilen boyut boyutunu ayarlamak için üçgen tutamacı `wrap_content`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Kaydırma içerik ve yeniden boyutlandırma tanıtıcıları](designer-basics-images/vs/17-wrap-content.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kaydırma içerik ve yeniden boyutlandırma tanıtıcıları](designer-basics-images/xs/18-wrap-content-sml.png)](designer-basics-images/xs/18-wrap-content.png#lightbox)

-----

Tıklatarak **sarmalamak içerik** tanıtıcı kapalı içeriğini kaydırmak gerekenden daha adıdır büyük o boyutundaki pencere öğesi küçültür. Bu örnekte, düğme metni yatay sonraki ekran görüntüsünde gösterildiği gibi küçültür.

Boyut değeri ayarlandığında **sarmalamak içerik**, boyuta değiştirmek için ters yönde işaret eden bir üçgen tanıtıcı Designer görüntüler `match_parent`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Eşleşme üst tanıtıcısı](designer-basics-images/vs/18-match-parent.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Eşleşme üst tanıtıcısı](designer-basics-images/xs/19-match-parent-sml.png)](designer-basics-images/xs/19-match-parent.png#lightbox)

-----

Tıklatarak **eşleşme üst** tanıtıcı yükler, boyut boyutu böylece üst pencere öğesi ile aynı olur.

Ayrıca, döngüsel yeniden boyutlandırma (yukarıdaki ekran görüntülerinde gösterildiği gibi) için rastgele bir pencere öğesi yeniden boyutlandırmak için sürükleyebilirsiniz `dp` değeri. Bunu yaptığınızda hem de **sarmalamak içerik** ve **eşleşme üst** tanıtıcıları için bu boyut sunulur:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Döngüsel yeniden boyutlandırma](designer-basics-images/vs/19-resize-dp.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Döngüsel yeniden boyutlandırma](designer-basics-images/xs/20-resize-dp-sml.png)](designer-basics-images/xs/20-resize-dp.png#lightbox)

-----

Tüm kapsayıcıları düzenlenmesine izin `Size` bir pencere öğesinin. Örneğin, ekran görüntüsü ile dikkat `LinearLayout` seçili, yeniden boyutlandırma görünmez:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Hiçbir yeniden boyutlandırma](designer-basics-images/vs/20-no-resize-handles.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Hiçbir yeniden boyutlandırma](designer-basics-images/xs/21-no-resize-handles-sml.png)](designer-basics-images/xs/20-no-resize-handles.png#lightbox)

-----



## <a name="document-outline"></a>Belge Anahattı

**Belge anahattı** düzeni pencere öğesi hiyerarşisini görüntüler.
Aşağıdaki örnekte içeren `LinearLayout` pencere öğesi seçili:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Belge Anahattı](designer-basics-images/vs/21-document-outline.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Belge Anahattı](designer-basics-images/xs/22-outline-view-sml.png)](designer-basics-images/xs/22-outline-view.png#lightbox)

-----

Seçili pencere öğesinin ana hattını (Bu durumda, bir `LinearLayout`) de tasarım yüzeyine vurgulanır. Seçili pencere Belge Anahattı öğesinde kendisine karşılık gelen tasarım yüzeyine ile eşitlenmiş olarak tutulur. Bu, her zaman tasarım yüzeyine seçmek kolay değildir görünümü grupları seçmek için kullanışlıdır.

Belge Anahattı Kopyala ve Yapıştır destekler veya sürükle ve bırak. Belge Anahattı tasarım yüzeyine yanı sıra tasarım yüzeyine Belge Anahattı sürükle ve bırak desteklenir. Ayrıca, belge anahattı bir öğeyi sağ tıklayarak bu öğeyi (tasarım yüzeyine aynı pencere öğesinin sağ tıklattığınızda görüntülenen aynı bağlam menüsü) için bağlam menüsünde görüntüler.
