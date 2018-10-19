---
title: Android (Widgets) denetler.
description: Xamarin.Android kullanıcı arabirimleri oluşturmaya yönelik yapı taşları
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 842fb1df2c9cc1aaf1a106687179a3730c2503bd
ms.sourcegitcommit: 7e4070bc104d612b6754ea35dd5a49c5c3d45f4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "32436719"
---
# <a name="android-controls-widgets"></a>Android (Widgets) denetler.

Xamarin.Android tüm Android tarafından sağlanan yerel kullanıcı arabirimi denetimleri (widgets) kullanıma sunar. Android Designer kullanarak Xamarin.Android uygulamaları için veya programlama arabiriminde XML düzeni dosyaları aracılığıyla bu denetimleri kolayca eklenebilir. Seçtiğiniz yöntem ne olursa olsun, Xamarin.Android tüm kullanıcı arabirimi nesne özelliklerini ve yöntemlerini C# dilinde kullanıma sunar. Aşağıdaki bölümlerde, en sık karşılaşılan Android kullanıcı arabirimi denetimleri tanıtır ve Xamarin.Android uygulamalarına eklemek açıklanmaktadır.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Eylem çubuğu](~/android/user-interface/controls/action-bar.md) 

`ActionBar` Etkinlik Başlığı, gezinti arabirimleri ve etkileşimli diğer öğeleri görüntüleyen bir araç çubuğudur. Genellikle, Eylem çubuğu etkinlik penceresinin en üstünde görünür.

![Örnek eylem](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Otomatik Tamamla](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` kullanıcının karşın, otomatik olarak tamamlama önerileri gösterir bir düzenlenebilir metin görünümü öğesidir. Bir açılan menüden kullanıcı düzenleme kutusu içeriğini değiştirmek için bir öğe seçebilir öneriler listesi görüntülenir.

![Otomatik Tamamlama örneği](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Düğmeler](~/android/user-interface/controls/buttons/index.md)

Bir eylemi gerçekleştirmek için kullanıcı dokunduğunda kullanıcı Arabirimi öğeleri düğmelerdir.

![Örneğin düğmeler](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Takvim](~/android/user-interface/controls/calendar.md)

`Calendar` Sınıfı, belirli bir örneğinde dönüştürmek için kullanılır (dönem uzaklığı bir milisaniyeden kısa değer) yıl, ay, saat, ayın günü ve sonraki hafta sonu gibi değerler için zaman.
`Calendar` çok sayıda olayları, katılımcı ve anımsatıcılar okuma ve yazma olanağı dahil olmak üzere Takvim verilerle etkileşimi seçenekleri destekler. Uygulamanızda Takvim sağlayıcısı kullanarak, API aracılığıyla eklediğiniz veri Android ile birlikte gelen yerleşik Takvim uygulamasını görünür.

![Örnek Takvim](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` kartları benzer görünümlerde metin ve resim içerik sunan bir UI bileşenidir. `CardView` olarak uygulanan bir `FrameLayout` yuvarlatılmış köşeler ve gölge pencere öğesi. Genellikle, bir `CardView` tek satır öğesinde sunmak için kullanılan bir `ListView` veya `GridView` görünümü grubu.

![Örnek kartı görüntüle](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Metni Düzenle](~/android/user-interface/controls/edit-text.md)

`EditText` girme ve metin değiştirmek için kullanılan bir kullanıcı Arabirimi öğesidir.

![Örnek metnini düzenle](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Galeri](~/android/user-interface/controls/gallery.md)

`Gallery` Yatay kaydırma listedeki öğeleri görüntülemek için kullanılan bir düzen pencere sınıflandırabilirsiniz. Bu görünümün merkezinde geçerli seçimi yerleştirir.

![Örnek Galerisi](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Gezinti Çubuğu](~/android/user-interface/controls/navigation-bar.md)

*Gezinti çubuğu* Gezinti denetimlerinin cihazlarda donanım düğmelerini içermez sağlar **giriş**, **geri**, ve **menü**.

![Örnek gezinti çubuğu](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Seçiciler](~/android/user-interface/controls/pickers/index.md)

*Seçiciler* Android tarafından sağlanan iletişim kutularını kullanarak bir tarih veya saat kullanıcının sağlayan kullanıcı Arabirimi öğeleri.

![Örnek Seçici](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Açılır Menü](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` belirli bir görünüme iliştirilmiş açılır menüler görüntülemek için kullanılır.

![Örnek açılır menü](images/popup-menu.png)


## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[RatingBar](~/android/user-interface/controls/ratingbar.md)

A `RatingBar` yıldız derecelendirmesi görüntüleyen bir kullanıcı Arabirimi öğesidir.

![Bir RatingBar örneği](ratingbar-images/01-ratingbar.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Değer Değiştirici](~/android/user-interface/controls/spinner.md)

`Spinner` Bir kümeden bir değer seçmek için hızlı bir yolunu sağlayan bir kullanıcı Arabirimi öğesidir. Bu, aşağı açılan listesine simmilar olur. 

![Örnek değer değiştirici](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[Anahtar](~/android/user-interface/controls/switch.md)

`Switch` iki durum arasında böyle bir şekilde geçiş veya kapalı açmasına izin veren bir kullanıcı Arabirimi öğesidir. `Switch` Varsayılan değer: kapalı.

![Örnek anahtarı](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` bir video veya görüntülenecek OpenGL içerik akışı sağlamak için donanım tarafından hızlandırılan 2B işleme kullanan bir görünümüdür.

![Örnek doku görünümü](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[Araç Çubuğu](~/android/user-interface/controls/tool-bar/index.md)

`Toolbar` (Android 5.0 Lollipop'ta sunulan) pencere öğesi düşünülebilir Eylem çubuğu arabirimi genelleştirilmiş &ndash; Eylem çubuğu değiştirmek için tasarlanmıştır. `Toolbar` Bir uygulama düzende her yerde kullanılabilir ve bir eylem çubuğu daha çok daha fazla özelleştirilebilir.

![Örnek araç çubuğu](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager` Sol ve sağda veri sayfaları aracılığıyla ters çevirmek kullanıcıya izin veren bir düzen yöneticisidir.

![Örnek ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` web sayfaları görüntülemek için kendi penceresi oluştur (veya tam bir tarayıcı bile geliştirmek) izin veren bir kullanıcı Arabirimi öğesidir.

![Örnek Web görünümü](images/web-view.png)

