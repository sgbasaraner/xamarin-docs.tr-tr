---
title: "Malzeme tasarım özellikleri"
description: "Bu konu, malzeme tasarım uyumlu düzenleri oluşturmak geliştiriciler için kolaylaştıran Tasarımcısı özellikleri açıklar. Bu bölümde tanıtır ve malzeme kılavuz, malzeme renk paletini, tipografik ölçek ve tema Düzenleyicisi'ni nasıl kullanılacağını açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC55E1B2-C239-4019-B0C3-A16F6CF0D6E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: c0b5fa3e7eacb9f7fd8aa133a290d0e7654972ce
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="material-design-features"></a>Malzeme tasarım özellikleri

_Bu konu, malzeme tasarım uyumlu düzenleri oluşturmak geliştiriciler için kolaylaştıran Tasarımcısı özellikleri açıklar. Bu bölümde tanıtır ve malzeme kılavuz, malzeme renk paletini, tipografik ölçek ve tema Düzenleyicisi'ni nasıl kullanılacağını açıklar._

<a name="overview" />

## <a name="overview"></a>Genel Bakış

Xamarin.Android Tasarımcısı malzeme tasarım-uyumlu düzenlerini oluşturma kolaylaştıran özellikler içerir. Malzeme tasarımıyla bilmiyorsanız bkz [malzeme tasarım giriş](https://www.google.com/design/spec/material-design/introduction.html).

Bu kılavuzda, biz aşağıdaki Tasarımcısı özellikleri bakmak gerekir:

-   *Malzeme kılavuz* &ndash; bir katmana tasarım yüzeyinde kılavuz, aralık ve malzeme tasarım yönergelerine göre düzeni pencere öğeleri yerleştirmek yardımcı olması için anahtar satırlar gösterir.

-   *Malzeme renk paletini* &ndash; resmi malzeme tasarım paletinden bir renk seçerken yönetmenize yardımcı olan özellik panel iletişim.

-   *Tipografi ölçek* &ndash; için tasarım uyumlu Malzeme ayarları seçimine sağlayan bir özellik paneli iletişim `textAppearance` metin alanlarının özelliği.

-   *Tema Düzenleyicisi* &ndash; bir Tema rengi bilgi için bir alt kümesi olanak sağlayan bir küçük renk Kaynak Düzenleyici. Örneğin, Önizleme ve malzeme renkleri değiştirmek `colorPrimary`, `colorPrimaryDark`, ve `colorAccent`.

Biz, bu özelliklerin her biri bakın ve bunları nasıl kullanacağınızı örnekleri sağlayın.


<a name="material_grid" />

## <a name="material-design-grid"></a>Malzeme Tasarım Kılavuzu

Malzeme Tasarım Kılavuzu menü Tasarımcı üstündeki araç çubuğundan kullanılabilir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Malzeme Tasarım Kılavuzu](material-design-features-images/vs/01-material-design-grid-sml.png)](material-design-features-images/vs/01-material-design-grid.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Malzeme Tasarım Kılavuzu](material-design-features-images/xs/01-material-design-grid-sml.png)](material-design-features-images/xs/01-material-design-grid.png)

-----

Malzeme Tasarım Kılavuzu simgesini tıklatın, tasarımcı bir katmana aşağıdaki öğeleri içeren tasarım yüzeyine görüntüler:

-   Anahtar satırlar (turuncu satırlar)

-   Aralık (yeşil alanları)

-   Bir kılavuz (mavi çizgiler)

Bu öğeleri aşağıdaki ekran görüntüsünde görebilirsiniz:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Anahtar satır, aralık ve kılavuz](material-design-features-images/vs/02-grid-and-keylines-sml.png)](material-design-features-images/vs/02-grid-and-keylines.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Anahtar satır, aralık ve kılavuz](material-design-features-images/xs/02-grid-and-keylines-sml.png)](material-design-features-images/xs/02-grid-and-keylines.png)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu katmana öğelerin her birini yapılandırılabilir. Malzeme Tasarım Kılavuzu menüsünün yanındaki üç nokta işaretine tıkladığınızda, iletişim popover devre dışı bırak / kılavuz etkinleştirme, anahtar satırlar yerleşimini yapılandırmak ve boşluklar ayarlamanıza olanak veren açar. Tüm değerleri ifade edilir Not `dp` (yoğunluğu bağımsız piksel cinsinden):

![Kılavuz, anahtar satır ve aralığı yapılandırma](material-design-features-images/vs/03-grid-configuration.png)

Yeni bir anahtar satır eklemek için yeni bir uzaklık değeri girin **uzaklık** kutusunda, bir konum seçin (**sol**, **üst**, **sağ**, veya  **alt**) tıklatıp + yeni anahtar satır eklemek için simge.

Benzer şekilde, yeni bir aralık eklemek için uzaklık (içinde dp) ve boyutu girerek **boyutu** ve **uzaklık** kutular, sırasıyla. Bir konum seçin (**sol**, **üst**, **sağ**, veya **alt**) tıklatıp + yeni aralığı eklemek için simge.

Bu yapılandırma değerlerini değiştirdiğinizde, bunlar Düzen XML dosyasına kaydedilir ve düzenini yeniden açtığınızda yeniden.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu katmana öğelerin her birini yapılandırılabilir. Malzeme Tasarım Kılavuzu menüsünün yanındaki aşağı üçgen tıklattığınızda, iletişim popover devre dışı bırak / kılavuz etkinleştirme, anahtar satırlar yerleşimini yapılandırmak ve boşluklar ayarlamanıza olanak veren açar. Tüm değerleri ifade edilir Not `dp` (yoğunluğu bağımsız piksel cinsinden):

[![Kılavuz, anahtar satır ve aralığı yapılandırma](material-design-features-images/xs/03-grid-configuration-sml.png)](material-design-features-images/xs/03-grid-configuration.png)

Yeni bir anahtar satır eklemek için yeni bir uzaklık değeri girin **uzaklık** kutusunda, bir konum seçin (**sol**, **üst**, **sağ**, veya  **alt**) tıklatıp + yeni anahtar satır eklemek için simge.

Benzer şekilde, yeni bir aralık eklemek için uzaklık (içinde dp) ve boyutu girerek **boyutu** ve **uzaklık** kutular, sırasıyla. Bir konum seçin (**sol**, **üst**, **sağ**, veya **alt**) tıklatıp + yeni aralığı eklemek için simge.

Bu yapılandırma değerlerini değiştirdiğinizde, bunlar Düzen XML dosyasına kaydedilir ve düzenini yeniden açtığınızda yeniden.


## <a name="material-design-color-palette"></a>Malzeme tasarım renk paleti

Bir renk şimdi kabul her özellik Masası öğesi bu ekran görüntüsünde gösterildiği gibi malzeme tasarım renk paletini açmak için kullanabileceğiniz ek bir simge vardır:

[![Renk simgesi](material-design-features-images/xs/04-new-color-icon-sml.png)](material-design-features-images/xs/04-new-color-icon.png)

Bu simgeye tıklattığınızda, bir iletişim popover bu özellik malzeme tasarım renk paletindeki rengi yapılandırmanızı olanaklı kılan açar:

[![Malzeme tasarım renk paleti](material-design-features-images/xs/05-material-palette-sml.png)](material-design-features-images/xs/05-material-palette.png)

Palet alt tonlar seçili birincil renk için bir aralığı görüntülerken renk paletini üst birincil malzeme tasarım renkleri görüntüler. Örneğin, seçtiğinizde, **Indigo olarak biliniyordu**, koleksiyonu **Indigo olarak biliniyordu** tonlar iletişim kutusunun en altında görüntülenir.
Bir ton seçtiğinizde, renk özelliğinin seçili ton değiştirilir. Aşağıdaki örnekte, `Background Tint` düğmesine değiştirilir *Indigo olarak biliniyordu 500*:

[![Indigo olarak biliniyordu 500 seçin](material-design-features-images/xs/06-indigo-sml.png)](material-design-features-images/xs/06-indigo.png)

`Background Tint` renk kodunu ayarlanır *Indigo olarak biliniyordu 500* (`#ff3f51b5`), ve bu değişikliği yansıtacak şekilde düğmenin arka plan rengini Tasarımcısı güncelleştirir:

[![Arka plan TINT değişiklikleri](material-design-features-images/xs/07-background-tint-sml.png)](material-design-features-images/xs/07-background-tint.png)

Malzeme tasarım malzeme tasarım renk paletini hakkında daha fazla bilgi için bkz: [renk paletini Kılavuzu](http://www.google.com/design/spec/style/color.html#color-color-palette).

## <a name="typographic-scale"></a>Tipografi ölçek

**Metin görünümü** bölümünü **özelliği** paneli **stili** sekmesi vardır olanak sağlayan bir simge arasından seçim yapmasını bir `TextAppearance` malzeme tasarıma uygun stili belirtimi:

[![Stil sekmesi](material-design-features-images/xs/08-typo-scale-icon-sml.png)](material-design-features-images/xs/08-typo-scale-icon.png)

Bu simgeye tıklattığınızda, açılır **tipografik ölçek** aralarından seçim yapabileceğiniz önceden yapılandırılmış metin stilleri bir listesini sunar iletişim popover:

[![Metin stili Seçici](material-design-features-images/xs/09-text-appearance-sml.png)](material-design-features-images/xs/09-text-appearance.png)

Aşağıdaki örnekte tıklatarak **ekran 1** düğmenin metni büyük yazı tipini değiştirir **ekran 1**:

[![Görüntü 1 stili](material-design-features-images/xs/10-display-1-sml.png)](material-design-features-images/xs/10-display-1.png)

Metin stili **tipografik ölçek** iletişim izleyen **tema** ayarı. Örneğin, varsa **açık** tema Tasarımcısı'nda kullanılabilir metin stilleri yansıtmalar listesini seçilen **açık** tema:

[![Açık tema](material-design-features-images/xs/11-light-theme-sml.png)](material-design-features-images/xs/11-light-theme.png)

-----


<a name="theme_editor" />

## <a name="theme-editor"></a>Tema Düzenleyicisi

**Tema Düzenleyicisi** tema özniteliklerin bir alt kümesi için renk bilgilerini özelleştirmenizi sağlar. Açmak için **tema Düzenleyicisi**, araç çubuğundaki Fırçası simgesini tıklatın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Tema Düzenleyicisi simgesi](material-design-features-images/vs/04-theme-editor-icon.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![Tema Düzenleyicisi simgesi](material-design-features-images/xs/12a-theme-editor-icon-sml.png)](material-design-features-images/xs/12a-theme-editor-icon.png)

-----

Ancak **tema Düzenleyicisi** aşağıda açıklanan yeteneklerinin bir alt kümesini yalnızca hedef API düzey (Android 5.0 API 21 önce ise, kullanılabilir tüm Android sürümlerini ve API düzeylerini, hedef için araç çubuğundaki erişilebilir değil Lolipop).

Sol panelinde **tema Düzenleyicisi** şu anda seçilen tema yapma renkleri listesini görüntüler (Bu örnekte, kullanıyoruz `Default Theme`):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tema Düzenleyicisi](material-design-features-images/vs/05-theme-editor-sml.png)](material-design-features-images/vs/05-theme-editor.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tema Düzenleyicisi](material-design-features-images/xs/12b-theme-editor-sml.png)](material-design-features-images/xs/12b-theme-editor.png)

-----

Sol taraftaki bir renk seçtiğinizde, sağ panelde, renk düzenlemenize yardımcı olması için aşağıdaki sekmeleri sağlar:

-   **Devralma** &ndash; seçili renk için bir stil devralma diyagram görüntüler ve çözümlenen renk ve o Tema rengi atanan renk kodunu listeler.

-   **Renk Seçici** &ndash; herhangi bir rastgele değer rengini seçili değiştirmenize olanak tanır.

-   **Malzeme palet** &ndash; değiştirdiğiniz seçili renge malzeme tasarıma uygun bir değere izin verir.

-   **Kaynakları** &ndash; değiştirdiğiniz seçili renk için bir diğer var olan sağlar renk temasını kaynaklarında.

Bu sekmeleri ayrıntılı her biri bakalım.


<a name="theme_edit_inherit_tab" />

### <a name="inherit-tab"></a>Sekme devral

Aşağıdaki örnekte görüldüğü gibi **devral** sekmesi listeler için stil devralma **arka plan** rengini **varsayılan tema**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Sekme devral](material-design-features-images/vs/06-inherit-tab-sml.png)](material-design-features-images/vs/06-inherit-tab.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Sekme devral](material-design-features-images/xs/13-inherit-sml.png)](material-design-features-images/xs/13-inherit.png)

-----

Bu örnekte, **varsayılan tema** kullanan bir stil devralan `@color/background_material_dark` ancak ile geçersiz kılmaları `color/material_grey_850`, renk kodu değeri olan `#ff303030`.
Stil devralma hakkında daha fazla bilgi için bkz: [stilleri ve Temalar](http://developer.android.com/guide/topics/ui/themes.html#Inheritance).


<a name="theme_edit_color_picker" />

### <a name="color-picker"></a>Renk Seçici

Aşağıdaki ekran görüntüsü gösterilmektedir **Renk Seçici**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Renk Seçici](material-design-features-images/vs/07-color-picker-sml.png)](material-design-features-images/vs/07-color-picker.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Renk Seçici](material-design-features-images/xs/14-color-picker-sml.png)](material-design-features-images/xs/14-color-picker.png)

-----

Bu örnekte, **arka plan** renk, herhangi bir değer arasında çeşitli yollarla değiştirilebilir:

-   Bir renk doğrudan'yı tıklatın.
-   Ton, Doygunluk ve parlaklığını değerlerini girme.
-   RGB (kırmızı, yeşil, mavi) değerleri ondalık girme.
-   Seçili renk için alpha (geçirgenlik) ayarlama.
-   Onaltılık renk kodunu doğrudan girerek.

Renk Seçici'de seçtiğiniz renk *değil* malzeme tasarım yönergeleri veya kullanılabilir renk kaynak kümesi için kısıtlanmış.

<a name="theme_edit_resources" />

### <a name="resources"></a>Kaynaklar

**Kaynakları** sekmesini zaten temada mevcut renk kaynakların bir listesini sunar:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kaynakları](material-design-features-images/vs/08-resources-sml.png)](material-design-features-images/vs/08-resources.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kaynakları](material-design-features-images/xs/15-resources-sml.png)](material-design-features-images/xs/15-resources.png)

-----

Kullanarak **kaynakları** sekmesinde seçimlerinizi renklerin bu listeye kısıtlar. Zaten başka bir tema kısmına atanmış bir renk kaynağı seçerseniz, "(aynı renk sahip oldukları için) iki bitişik öğeleri UI birlikte çalışabilir," göz önünde bulundurun ve kullanıcı ayırt etmek zor hale gelir.


<a name="theme_edit_material_pallette" />

### <a name="material-palette"></a>Malzeme paleti

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Malzeme palet** sekmesinde açar **malzeme tasarım renk paletini**. Böylece malzeme tasarım yönergeleri ile tutarlı bir renk değeri bu paletinden seçme renk seçiminizi kısıtlar.

[![Malzeme paleti](material-design-features-images/vs/09-material-palette-sml.png)](material-design-features-images/vs/09-material-palette.png)

Palet alt tonlar seçili birincil renk için bir aralığı görüntülerken renk paletini üst birincil malzeme tasarım renkleri görüntüler. Örneğin, seçtiğinizde, **Indigo olarak biliniyordu**, koleksiyonu **Indigo olarak biliniyordu** tonlar iletişim kutusunun en altında görüntülenir.
Bir ton seçtiğinizde, renk özelliğinin seçili ton değiştirilir. Aşağıdaki örnekte, `Background Tint` düğmesine değiştirilir *Indigo olarak biliniyordu 500*:

![Select Indigo 500](material-design-features-images/vs/10-indigo.png)

`Background Tint` renk kodunu ayarlanır *Indigo olarak biliniyordu 500* (`#ff3f51b5`), ve bu değişikliği yansıtacak şekilde arka plan rengi Tasarımcısı güncelleştirir:

[![Arka plan TINT değiştirildi](material-design-features-images/vs/11-background-tint-sml.png)](material-design-features-images/vs/11-background-tint.png)

Malzeme tasarım malzeme tasarım renk paletini hakkında daha fazla bilgi için bkz: [renk paletini Kılavuzu](http://www.google.com/design/spec/style/color.html#color-color-palette).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

**Malzeme palet** sekmesinde açar **malzeme tasarım renk paletini** açıklanan [önceki](#material_palette). Böylece malzeme tasarım yönergeleri ile tutarlı bir renk değeri bu paletinden seçme renk seçiminizi kısıtlar.

[![Malzeme paleti](material-design-features-images/xs/16-material-palette-sml.png)](material-design-features-images/xs/16-material-palette.png)

-----


<a name="theme_create" />

### <a name="creating-a-new-theme"></a>Yeni bir tema oluşturma

Aşağıdaki örnekte, yeni özel bir tema oluşturmak için malzeme palet kullanacağız. İlk olarak, biz değiştireceğiz **arka plan** için renk *mavi 900*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Mavi 900 arka planını Değiştir](material-design-features-images/vs/12-change-background-to-blue.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Mavi 900 arka planını Değiştir](material-design-features-images/xs/17-change-background-to-blue-sml.png)](material-design-features-images/xs/17-change-background-to-blue.png)

-----


Bir renk kaynağı değiştirildiğinde, ileti'yi mesajı ile açılır *geçerli tema kaydedilmemiş değişiklikler var.*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Uyarı kaydedilmemiş değişiklikler](material-design-features-images/vs/13-unsaved-changes-sml.png)](material-design-features-images/vs/13-unsaved-changes.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Uyarı kaydedilmemiş değişiklikler](material-design-features-images/xs/18-unsaved-changes-sml.png)](material-design-features-images/xs/18-unsaved-changes.png)

-----


**Arka plan** renk Tasarımcısı'nda, yeni renk seçimi değişti, ancak bu değişiklik henüz kaydedilmedi. Değişiklik nasıl ele alınacağını için iki seçenek sunulur:

-   **Değişiklikleri atmak** &ndash; yeni renk seçim (ya da seçenek) atar ve tema özgün durumuna geri döner.

-   **Yeni Tema oluşturma** &ndash; şu anda seçili temayı türetilmiş ve yapılan renk geçersiz kılmaları kullanan yeni bir tema oluşturur **tema Düzenleyicisi**.

Tıkladığınızda **yeni Tema oluşturma**, aşağıdakilerden biri gerçekleşir, bağlı olarak seçilen tema:

-   Tasarımcı şu anda seçili temasına proje tema değilse (Seçilen tema özelleştirilmiş tema üst öğe olarak kullanarak) özelleştirilmiş tema ile yeni bir kaynak dosyası oluşturmak için seçeneği sunulur. Oluşturulduktan sonra Tasarımcı özelleştirilmiş tema geçer.

-   Şu anda seçili temayı tek bir yerde tanımlı bir proje tema ise, size sunulan **güncelleştirme tema** seçeneği; bu seçeneğini tıklatarak yeni bir tane oluşturmak yerine şu anda seçilen tema güncelleştirir.

-   Şu anda seçilen tema birden fazla konumda tanımlı bir proje tema ise (örneğin, içinde farklı-sonekine **değerleri** klasörler gibi **değerleri v21**), soran bir iletişim kutusu sunulur özelleştirilmiş tema kaydetmek için yeni bir konum.

Tıklatarak önceki örnekte, devam etmeden **yeni Tema oluşturma** adlı yeni bir tema oluşturma sonuçlarında **özel**:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Eklenen özel tema](material-design-features-images/vs/14-custom-theme-sml.png)](material-design-features-images/vs/14-custom-theme.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Eklenen özel tema](material-design-features-images/xs/19-custom-theme-sml.png)](material-design-features-images/xs/19-custom-theme.png)

-----


Şu anda seçilen tema proje tema olmadığından, seçilen tema güncelleştirmek veya yeni bir konum belirtmek için iletişim kutusu yok yok.

<a name="summary" />

## <a name="summary"></a>Özet

Bu konuda Xamarin.Android Tasarımcısı'nda kullanılabilen malzeme tasarım özellikleri açıklanmaktadır. Etkinleştirme ve yapılandırma malzeme Tasarım Kılavuzu, renk özelliklerini düzenlemek için malzeme tasarım renk paletini kullanma ve tipografik ölçek Seçici metin özelliklerini yapılandırmak için nasıl kullanılacağı açıklanmıştır. Ayrıca, tema Düzenleyicisi'ni malzeme tasarım yönergelerine uygun yeni özel temalar oluşturmak için nasıl kullanılacağı gösterilmektedir. Malzeme tasarım Xamarin.Android desteği hakkında daha fazla bilgi için bkz: [malzeme tema](~/android/user-interface/material-theme.md).



## <a name="related-links"></a>İlgili bağlantılar

- [Malzeme tema](~/android/user-interface/material-theme.md)
- [Malzeme tasarım giriş](https://www.google.com/design/spec/material-design/introduction.html)
