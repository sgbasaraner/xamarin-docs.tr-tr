---
title: Android Tasarımcısı'nı kullanarak
description: Bu konuda bir kılavuz Xamarin.Android Designer ' dir. Küçük renk tarayıcı uygulaması için kullanıcı arabirimi oluşturmak üzere nasıl gösterir; Bu kullanıcı arabirimi tamamen Tasarımcısı'nda oluşturulur.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/10/2018
ms.openlocfilehash: 8d1dc410d5336d9c2505a18720cc7f734e838c39
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798638"
---
# <a name="using-the-android-designer"></a>Android Tasarımcısı'nı kullanarak

_Bu konuda bir kılavuz Xamarin.Android Designer ' dir. Küçük renk tarayıcı uygulaması için kullanıcı arabirimi oluşturmak üzere nasıl gösterir; Bu kullanıcı arabirimi tamamen Tasarımcısı'nda oluşturulur._


## <a name="overview"></a>Genel Bakış

Android kullanıcı arabirimleri kullanarak XML dosyalarının veya program aracılığıyla kod yazma bildirimli olarak oluşturulabilir. Xamarin.Android Tasarımcısı'nı oluşturmak ve XML dosyalarının elle düzenlendiği birçoğunu ile mücadele etmek zorunda kalmadan bildirim temelli düzenleri görsel değiştirmek geliştiricilerin sağlar. Tasarımcı Ayrıca uygulama bir cihaz ya da bir öykünücü yeniden dağıtmak zorunda kalmadan UI değişiklikleri değerlendirmek Geliştirici sağlayan gerçek zamanlı geri bildirim sağlar. Bu Android UI geliştirme büyük ölçüde hızlandırabilir. Bu makalede, Xamarin.Android Tasarımcısı görsel olarak bir kullanıcı arabirimi oluşturmak için nasıl kullanılacağını gösteren bir kılavuz sunar.


## <a name="walkthrough"></a>İzlenecek yol

Bu kılavuzun amacı renklerini, adlarını ve RGB değerleri listesini sunan bir örnek renk tarayıcı uygulaması için kullanıcı arabirimi oluşturmak üzere Android Tasarımcısı'nı kullanmaktır. Biz bu pencere öğeleri görsel olarak düzenlemek nasıl yanı sıra tasarım yüzeyine pencere öğeleri ekleme adresindeki göreceğiz. Pencere öğeleri etkileşimli olarak tasarım yüzeyine veya Tasarımcısı kullanarak değiştirme bundan sonra açıklayacağız **özelliği paneli**. Son olarak, biz uygulamayı çalıştırdığınızda, bizim tasarım nasıl göründüğünü görürsünüz.

Haydi başlayalım!


### <a name="creating-a-new-project"></a>Yeni proje oluşturma

İlk adım, yeni bir Xamarin.Android projesi oluşturmaktır.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'yu başlatın ve tıklayın **yeni proje...**  ardından **Visual C\# > Android > Android uygulaması (Xamarin)** şablonu:

[![Android boş uygulama](designer-walkthrough-images/vs/01-android-app-sml.w157.png)](designer-walkthrough-images/vs/01-android-app.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac ve tıklatın için Visual Studio'yu başlatın **yeni çözüm...** . Seçin **Android uygulaması** şablonu ve tıklatın **sonraki**:

[![Android boş uygulama](designer-walkthrough-images/xs/01-android-app-sml.png)](designer-walkthrough-images/xs/01-android-app.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Yeni uygulama adı **DesignerWalkthrough** tıklatıp **Tamam**.

[![Uygulama adı](designer-walkthrough-images/vs/02-name-app-sml.png)](designer-walkthrough-images/vs/02-name-app.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Yeni uygulama adı **DesignerWalkthrough**. Altında **Hedef platformlar**seçin **son ve en büyük** tıklatıp **sonraki**:

[![Uygulama adı](designer-walkthrough-images/xs/02-designer-walkthrough-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough.png#lightbox)

Sonraki iletişim ekranında tıklatın **oluşturma**.

-----



### <a name="adding-a-layout"></a>Bir düzen ekleme

Oluşturalım bir **LinearLayout** biz bizim kullanıcı arabirimi öğeleri tutmak için kullanır.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da sağ **kaynakları/düzeni** içinde **Çözüm Gezgini** seçip **Ekle > Yeni öğe...** . İçinde **Yeni Öğe Ekle** iletişim kutusunda **Android düzeni**. Dosya adı **ListItem.axml** tıklatıp **Ekle**:

[![Yeni düzeni](designer-walkthrough-images/vs/03-new-layout-sml.w157.png)](designer-walkthrough-images/vs/03-new-layout.w157.png#lightbox)

Yeni **LISTITEM** düzeni Tasarımcısı'nda görüntülenir:

[![Tasarımcı görünümü](designer-walkthrough-images/vs/04-designer-view-sml.png)](designer-walkthrough-images/vs/04-designer-view.png#lightbox)

Tıklatın **kaynak** bu düzeni için XML kaynağını görüntülemek için Tasarımcısı'nın altındaki sekmesi:

[![Tasarımcı XML](designer-walkthrough-images/vs/05-designer-xml-sml.png)](designer-walkthrough-images/vs/05-designer-xml.png#lightbox)

Gelen **Görünüm** menüsünde tıklatın **diğer pencereler > Belge Anahattı** açmak için **belge anahattı**. **Belge anahattı** düzeni şu anda tek bir içerdiğini gösterir **LinearLayout** pencere öğesi:

[![Belge Anahattı](designer-walkthrough-images/vs/06-document-outline-sml.png)](designer-walkthrough-images/vs/06-document-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da sağ **kaynakları/düzeni** içinde **çözüm** doldurur ve seçin **Ekle > yeni dosya...** . İçinde **yeni dosya** iletişim kutusunda **Android > düzeni**. Dosya adı **LISTITEM** tıklatıp **yeni**:

[![Yeni düzeni](designer-walkthrough-images/xs/03-new-layout-sml.png)](designer-walkthrough-images/xs/03-new-layout.png#lightbox)

Yeni **LISTITEM** düzeni Tasarımcısı'nda görüntülenir:

[![Tasarımcı görünümü](designer-walkthrough-images/xs/04-designer-view-sml.png)](designer-walkthrough-images/xs/04-designer-view.png#lightbox)

Tıklatın **kaynak** bu düzeni için XML kaynağını görüntülemek için Tasarımcısı'nın altındaki sekmesi. Tıkladığınızda **belge anahattı** sekmesini sağ tarafta düzeni şu anda tek bir içerdiğini gösterir **LinearLayout** pencere öğesi:

[![Tasarımcı XML](designer-walkthrough-images/xs/05-designer-xml-sml.png)](designer-walkthrough-images/xs/05-designer-xml.png#lightbox)

-----



### <a name="creating-the-list-item-user-interface"></a>Liste öğesi kullanıcı arabirimi oluşturma

Ardından, biz renk tarayıcı uygulaması için kullanıcı arabirimi oluşturmak üzere adımıdır.
Tıklatın **Tasarımcısı** tasarım yüzeyine döndürülecek sekmesi.
İçinde **araç**, aşağı kaydırarak **görüntüleri & medya** bölümünde ve bulmanıza kadar her bir öğeyi tekrar kullanmanıza bir *ImageView*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView bulun](designer-walkthrough-images/vs/07-locate-imageview-sml.png)](designer-walkthrough-images/vs/07-locate-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![ImageView bulun](designer-walkthrough-images/xs/06-locate-imageview-sml.png)](designer-walkthrough-images/xs/06-locate-imageview.png#lightbox)

-----

Alternatif olarak, girebilirsiniz *ImageView* bulmak için arama çubuğunu içine `ImageView` pencere öğesi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView arama](designer-walkthrough-images/vs/08-imageview-search-sml.png)](designer-walkthrough-images/vs/08-imageview-search.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![ImageView arama](designer-walkthrough-images/xs/07-imageview-search-sml.png)](designer-walkthrough-images/xs/07-imageview-search.png#lightbox)

-----

Bu sürükleme `ImageView` tasarım yüzeyine:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tuval üzerinde ImageView](designer-walkthrough-images/vs/09-imageview-on-canvas-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tuval üzerinde ImageView](designer-walkthrough-images/xs/08-imageview-on-canvas-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas.png#lightbox)

-----

Bu `ImageView` bizim renk tarayıcı uygulamasında bir renk görüntülemek için kullanılır.

Ardından, sürükleyin bir `LinearLayout (Vertical)` pencere öğesini **araç** Tasarımcı içine. Mavi anahat eklenen sınırlarının gösterir Not `LinearLayout`ve **belge anahattı** doğrudan aşağıda bulunan gösterir `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Mavi anahattı](designer-walkthrough-images/vs/10-blue-outline-sml.png)](designer-walkthrough-images/vs/10-blue-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Mavi anahattı](designer-walkthrough-images/xs/10-blue-outline-sml.png)](designer-walkthrough-images/xs/10-blue-outline.png#lightbox)

-----

Seçtiğinizde, `ImageView` Tasarımcısı'nda mavi anahattı taşır çevreleyen `ImageView`; **belge anahattı**, seçimi taşır `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView seçin](designer-walkthrough-images/vs/11-select-imageview-sml.png)](designer-walkthrough-images/vs/11-select-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![ImageView seçin](designer-walkthrough-images/xs/11-select-imageview-sml.png)](designer-walkthrough-images/xs/11-select-imageview.png#lightbox)

-----

Ardından, sürükleyin bir `Text (Large)` pencere öğesini **araç** yeni eklenen içine `LinearLayout`. Yeni pencere öğesi ekleneceği göstermek için bildirim Tasarımcısı yeşil kullanır vurgular:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Yeşil vurgular](designer-walkthrough-images/vs/12-green-highlight-sml.png)](designer-walkthrough-images/vs/12-green-highlight.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Yeşil vurgular](designer-walkthrough-images/xs/12-green-highlight-sml.png)](designer-walkthrough-images/xs/12-green-highlight.png#lightbox)

-----

Ardından, eklemek bir `Text (Small)` pencere öğesi aşağıdaki `Text (Large)` pencere öğesi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Küçük metin pencere öğesi ekleme](designer-walkthrough-images/vs/13-add-small-text-sml.png)](designer-walkthrough-images/vs/13-add-small-text.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Küçük metin pencere öğesi ekleme](designer-walkthrough-images/xs/13-add-small-text-sml.png)](designer-walkthrough-images/xs/13-add-small-text.png#lightbox)

-----

Bu noktada, aşağıdaki ekran Tasarımcısı benzemelidir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tasarımcı yerleşimi](designer-walkthrough-images/vs/14-raw-layout-sml.png)](designer-walkthrough-images/vs/14-raw-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tasarımcı yerleşimi](designer-walkthrough-images/xs/14-raw-layout-sml.png)](designer-walkthrough-images/xs/14-raw-layout.png#lightbox)

-----

İki `textView` pencere öğeleri iç olmayan `linearLayout1`, bunları sürükleyebilirsiniz `linearLayout1` içinde **belge anahattı** ve önceki ekran görüntüsünde gösterildiği gibi görünecek şekilde Yerleştir (altında girintili `linearLayout1`).



### <a name="arranging-the-user-interface"></a>Kullanıcı arabirimini düzenleme

Görüntülenecek kullanıcı arabirimini değiştirelim `ImageView` solda, iki ile `TextView` pencere öğeleri Yığılmış sağında `ImageView`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Seçin `ImageView`.

2.  İçinde **Özellikler penceresini**, tıklatın **kategoriler** sıralama simgesi ve aşağı kaydırın **Düzen - ViewGroup** bölümü.

3.  Değişiklik `layout_width` ayarını `wrap_content`:

![Ayarlama kaydırma içerik](designer-walkthrough-images/vs/15-wrap-content.png "kümesi kaydırma içeriği")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  İle `ImageView` seçili tıklatın **özellikleri** sekmesi.

2.  Yalnızca aşağıdaki **özellikleri** sekmesini tıklatın, **düzeni**.

3.  Ekranı aşağı kaydırarak **ViewGroup** değiştirip `Width` ayarını `wrap_content`:

[![Set kaydırma içeriği](designer-walkthrough-images/xs/15-wrap-content-sml.png)](designer-walkthrough-images/xs/15-wrap-content.png#lightbox)

-----

Değiştirmek için başka bir yolu `Width` ayardır kendi genişliği ayarı geçiş yapmak için pencere öğesinin sağ taraftaki üçgen tıklattığınızdan `wrap_content`:

[![Genişliğini ayarlamak için sürükleyin](designer-walkthrough-images/xs/16-width-arrow-sml.png)](designer-walkthrough-images/xs/16-width-arrow.png#lightbox)

Üçgen yeniden tıklandığında döndürür `Width` ayarını `match_parent`.

Ardından, geçiş **belge anahattı** ve kök seçin `LinearLayout`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kök LinearLayout seçin](designer-walkthrough-images/vs/16-root-linearlayout-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kök LinearLayout seçin](designer-walkthrough-images/xs/17-root-linearlayout-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout.png#lightbox)

-----
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kök ile `LinearLayout` seçili, geri dönmek **özellikleri** penceresinde tıklatın **alfabetik** sıralama simgesi ve kaydırma bulana kadar `orientation`. Değişiklik `orientation` ayarını `horizontal`:

![Yatay Yönlendirme Seç](designer-walkthrough-images/vs/17-horizontal-orientation.png "yatay yönlendirme seçin")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Kök ile `LinearLayout` seçili, geri dönüp **özellikleri** sekmesinde **pencere öğesi**. Değişiklik `Orientation` ayarını `horizontal`:

[![Yatay yönlendirme seçin](designer-walkthrough-images/xs/18-horizontal-orientation-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation.png#lightbox)

-----

Bu noktada, aşağıdaki ekran Tasarımcısı benzemelidir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tasarımcı yerleşimi](designer-walkthrough-images/vs/18-designer-layout-sml.png)](designer-walkthrough-images/vs/18-designer-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tasarımcı yerleşimi](designer-walkthrough-images/xs/19-designer-layout-sml.png)](designer-walkthrough-images/xs/19-designer-layout.png#lightbox)

-----


### <a name="modifying-the-spacing"></a>Aralık değiştirme

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ardından, biz pencere öğeleri arasında daha fazla boşluk sağlamak için kullanıcı arabiriminde doldurma ve kenar boşluğu ayarlarını değiştireceksiniz. Seçin `ImageView`, tıklatın **kategoriler** arama simgesine **özellikleri** penceresini açın ve aşağı kaydırın **düzeni** bölümü. Değişiklik `Min Height` için `70dp`, `Min Width` için `50dp`ve `padding` için `10dp`. Bu, tüm kenarlarına çevresindeki dolgunun geçerlidir `ImageView` ve dikey olarak elongates:

[![Doldurma ayarlayın](designer-walkthrough-images/vs/19-padding-widths-sml.png)](designer-walkthrough-images/vs/19-padding-widths.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Ardından, biz pencere öğeleri arasında daha fazla boşluk sağlamak için kullanıcı arabiriminde doldurma ve kenar boşluğu ayarlarını değiştireceksiniz. Seçin `ImageView` tıklatıp **düzeni** altında sekmesinde **özellikleri**. Değişiklik `Padding` için `10dp`, `Min Width` için `50dp`ve `Min Height` için `70dp`. Bu, tüm kenarlarına çevresindeki dolgunun geçerlidir `ImageView` ve dikey olarak elongates:

[![Doldurma ayarlayın](designer-walkthrough-images/xs/20-padding-widths-sml.png)](designer-walkthrough-images/xs/20-padding-widths.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Alt, sol, sağ ve üst ayarları doldurma ayarlanabilir bağımsız olarak değerlere girerek `paddingBottom`, `paddingLeft`, `paddingRight`, ve `paddingTop` alanlar, sırasıyla.
Örneğin, `paddingLeft` alanı `5dp` ve `paddingBottom`, `paddingRight`, ve `paddingTop` alanları `10dp`:

[![Özel doldurma ayarları](designer-walkthrough-images/vs/20-custom-padding-sml.png)](designer-walkthrough-images/vs/20-custom-padding.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Üst, sağ, alt ve sol ayarları doldurma bağımsız olarak değerlere girerek ayarlanabilir `Top`, `Right`, `Bottom`, ve `Left` alanlar, sırasıyla doldurma. Örneğin, `Left` doldurma değeri için `5dp` ve `Top`, `Right`, ve `Bottom` değerlere doldurma `10dp`. Unutmayın `Padding` ayarı değişiklikleri virgülle ayrılmış listesini bu değerleri için:

[![Özel doldurma ayarları](designer-walkthrough-images/xs/21-custom-padding-sml.png)](designer-walkthrough-images/xs/21-custom-padding.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ardından, konumunu ince ayar `LinearLayout` iki içeren pencere öğesi `TextView` pencere öğeleri. İçinde **belge anahattı**seçin `linearLayout1`. İçinde **özellikleri** penceresinde, kaydırın **Düzen - ViewGroup** bölümü. Ayarlama `layout_marginBottom`, `layout_marginLeft`, `layout_marginRight`, ve `layout_marginTop` için `5dp`, `5dp`, `0dp`, ve `5dp` sırasıyla:

[![Kenar boşluklarını ayarlama](designer-walkthrough-images/vs/21-margins-sml.png)](designer-walkthrough-images/vs/21-margins.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Ardından, konumunu ince ayar `LinearLayout` iki içeren pencere öğesi `TextView` pencere öğeleri. İçinde **belge anahattı**seçin `linearLayout1`. İçinde **özellikleri** bölmesinde, **düzeni** sekmesi. Ekranı aşağı kaydırarak **ViewGroup** bölümünde ve ayarlama `Left`, `Top`, `Right`, ve `Bottom` için kenar boşlukları `5dp`, `5dp`, `0dp`, ve `5dp` Sırasıyla:

[![Kenar boşluklarını ayarlama](designer-walkthrough-images/xs/22-margins-sml.png)](designer-walkthrough-images/xs/22-margins.png#lightbox)

-----



### <a name="removing-the-default-image"></a>Varsayılan görüntü kaldırma

Kullanmakta olduğunuz çünkü `ImageView` renkleri (görüntüleri yerine) görüntülemek için şirketinizdeki şablon tarafından eklenen varsayılan görüntü kaynağı kaldırın.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Seçin `ImageView`.

2.  İçinde **özellikleri**, bulma `src` alan.

3.  Clear `src` böylece boş ayarı:

![ImageView src ayarı temizlemek](designer-walkthrough-images/vs/22-clear-img-src.png "ImageView src ayarını kaldırın")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Seçin `ImageView`.

2.  Tıklatın **pencere öğesi** altında sekmesinde **özellikleri**.

3.  Clear `Src` böylece boş ayarı:

[![ImageView src ayarını kaldırın](designer-walkthrough-images/xs/23-clear-src-sml.png)](designer-walkthrough-images/xs/23-clear-src.png#lightbox)

-----


### <a name="adding-a-listview-container"></a>ListView kapsayıcı ekleme

Şimdi **LISTITEM** ekleyeceğiz düzeni tanımlanmış bir `ListView` ana düzen. Bu `ListView` listesini içerecektir **ListItems**.
İçinde **araç**, bulun `ListView` pencere öğesi ve tasarım yüzeyine sürükleyin. `ListView` Tasarımcısı'nda seçildiğinde kenarlığını anahat bir mavi satırlar dışında boş olacaktır. Görüntüleyebileceğiniz **belge anahattı** doğrulamak için **ListView** doğru bir şekilde eklendi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Yeni ListView](designer-walkthrough-images/vs/23-new-listview.png "yeni ListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Yeni liste görünümü](designer-walkthrough-images/xs/24-new-listview-sml.png)](designer-walkthrough-images/xs/24-new-listview.png#lightbox)

-----

Varsayılan olarak, `ListView` verilen bir `Id` değerini `@+id/listView1`.
Açık **pencere öğesi** altında sekmesinde **özellikleri** değiştirip `Id` için `@+id/myListView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Kimliği için myListView yeniden adlandırma](designer-walkthrough-images/vs/24-change-id.png "myListView kimliğine yeniden adlandır")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kimliği için myListView yeniden adlandır](designer-walkthrough-images/xs/25-change-id-sml.png)](designer-walkthrough-images/xs/25-change-id.png#lightbox)

-----

Bu noktada, kullanıcı arabirimimizi kullanılmaya hazırdır.



### <a name="running-the-application"></a>Uygulamayı Çalıştırma


Açık **MainActivity.cs** ve kendi kod şununla değiştirin:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity (Label = "DesignerWalkthrough", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem> ();
        ListView listView;

        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);

            SetContentView (Resource.Layout.Main);
            listView = FindViewById<ListView> (Resource.Id.myListView);

            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.DarkRed,
                                               ColorName = "Dark Red", Code = "8B0000" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.SlateBlue,
                                               ColorName = "Slate Blue", Code = "6A5ACD" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.ForestGreen,
                                               ColorName = "Forest Green", Code = "228B22" });

            listView.Adapter = new ColorAdapter (this, colorItems);
        }
    }
    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView (int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.ListItem, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}

```

Bu kod bir özel kullanır `ListView` renk bilgilerini yüklemek ve bu verileri yeni oluşturduğumuz kullanıcı Arabiriminde görüntülemek için bağdaştırıcısı. Bu örnek kısa tutmak için renk bilgileri bir listede sabit kodlanmış, ancak bağdaştırıcı bir veri kaynağından renk bilgi ayıklamak için anında hesaplamak için değiştirilmiş veya. Hakkında daha fazla bilgi için `ListView` bağdaştırıcıları bkz [ListViews ve bağdaştırıcıları](~/android/user-interface/layouts/list-view/index.md).

Derleme ve uygulamayı çalıştırın. Aşağıdaki ekran görüntüsünde, uygulama bir cihazda çalıştırıldığında, nasıl göründüğünü, bir örnek verilmiştir:

[![Son ekran görüntüsü](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)



## <a name="summary"></a>Özet

Bu makalede, bir kullanıcı arabirimi oluşturmak üzere Mac için Visual Studio'da Xamarin.Android Tasarımcısı'nı kullanmak nasıl aracılığıyla gitti. Ardından, arabirim için tek bir öğe için bir liste oluşturma gösterilmektedir.
Yol boyunca nasıl kaynakları atamak ve bu pencere öğeleri üzerinde çeşitli özelliklerini ayarlamak için yanı sıra nasıl pencere öğeleri eklemek ve görsel olarak düzenlenmesine izin inceledik. Conclusion, örnek bir Uygulama Tasarımcısı'nda oluşturduğumuz arabirimi nasıl çalıştığı gösterilmektedir.
