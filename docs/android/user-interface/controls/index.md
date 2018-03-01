---
title: Denetimler
description: "Xamarin.Android kullanıcı arabirimleri oluşturmak için yapı taşları"
ms.topic: article
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 25afd284fc88df4f23aaa3dfa1f47a3dc4fee551
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="controls"></a>Denetimler


Xamarin.Android tüm Android tarafından sağlanan yerel kullanıcı arabirimi denetimlerini (Pencere) kullanıma sunar. Bu denetimler, kolayca Xamarin.Android uygulamaları Android Tasarımcısı'nı kullanarak veya programlı şekilde XML düzeni dosyaları aracılığıyla eklenebilir. Seçtiğiniz yöntem bağımsız olarak, tüm kullanıcı arabirimi nesne özellikleri ve yöntemleri C# Xamarin.Android gösterir. Aşağıdaki bölümlerde, en yaygın Android kullanıcı arabirimi denetimlerini tanıtır ve Xamarin.Android uygulamalarda eklemenizi açıklanmaktadır.

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[Eylem çubuğu](~/android/user-interface/controls/action-bar.md) 

`ActionBar` Etkinlik Başlığı, gezinti arabirimleri ve diğer etkileşimli öğeleri görüntüleyen bir araç çubuğu bulunur. Genellikle, Eylem çubuğu bir etkinliğin penceresinin en üstünde görünür.

![Örnek ActionBar](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[Otomatik Tamamlama](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` Kullanıcı yazarak sırada gösteren tamamlama önerileri otomatik olarak bir düzenlenebilir metin görünümü öğedir. Öneriler listesi bir açılan menüden kullanıcı düzenleme kutusu içeriğini değiştirmek için bir öğe seçebileceği görüntülenir.

![Otomatik Tamamlama örneği](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[Düğmeleri](~/android/user-interface/controls/buttons/index.md)

Düğmeleri bir eylemi gerçekleştirmek için kullanıcı dokunur kullanıcı Arabirimi öğeleri değildir.

![Örnek düğmeleri](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Takvim](~/android/user-interface/controls/calendar.md)

`Calendar` Sınıfı, belirli bir kopya dönüştürmek için kullanılır (dönem uzaklığı bir milisaniyelik değer) yıl, ay, saat, ayın günü ve sonraki hafta sonu gibi değerler süresini.
`Calendar` bol miktarda olayları, katılımcılar ve anımsatıcıları okuma ve yazma yeteneği dahil olmak üzere Takvim verilerle etkileşimi seçeneklerini destekler. Uygulamanızda Takvim sağlayıcısı kullanarak API aracılığıyla eklemek veri Android gelen yerleşik Takvim uygulamasını görünür.

![Örnek Takvim](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` metin ve resim içerik kartları benzer görünümlerde sunan bir UI bileşenidir. `CardView` olarak uygulanan bir `FrameLayout` Yuvarlatılmış köşeleri ve gölge ile pencere öğesi. Genellikle, bir `CardView` tek satır öğesinde sunmak için kullanılan bir `ListView` veya `GridView` Görünüm Grup.

![Örnek kart görünümü](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[Metin düzenleme](~/android/user-interface/controls/edit-text.md)

`EditText` girme ve metin değiştirmek için kullanılan bir UI öğedir.

![Örnek metnini düzenle](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[Galerisi](~/android/user-interface/controls/gallery.md)

`Gallery` Yatay kaydırma listedeki öğeleri görüntülemek için kullanılan bir düzen pencere öğesi olan; Bu görünüm merkezinde geçerli seçim yerleştirir.

![Örnek Galerisi](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[Gezinti çubuğu](~/android/user-interface/controls/navigation-bar.md)

*Gezinti çubuğu* Gezinti denetimleri için donanım düğmeleri içermez cihazlarda sağlar **giriş**, **geri**, ve **menü**.

![Örnek gezinti çubuğu](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[Pickers](~/android/user-interface/controls/pickers/index.md)

*Seçiciler* Android tarafından sağlanan iletişim kutuları kullanarak bir tarih veya saat çekme kullanıcıya izin UI öğeleridir.

![Örnek Seçici](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[Açılır menüsü](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` belirli bir görünüme bağlı açılır menüler görüntülemek için kullanılır.

![Örnek açılır menüsü](images/popup-menu.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[Değer değiştirici](~/android/user-interface/controls/spinner.md)

`Spinner` kümesinden tek bir değer seçmek için bir hızlı yol sağlayan bir UI öğedir. Aşağı açılan listesine simmilar olur. 

![Örnek değer değiştirici](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[geçiş](~/android/user-interface/controls/switch.md)

`Switch` iki durumlu arasında böyle bir birimi geçiş veya kapalı bir kullanıcının bir kullanıcı Arabirimi öğedir. `Switch` Varsayılan değerdir kapalı.

![Örnek anahtar](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` bir video veya içerik akışı görüntülenecek OpenGL etkinleştirmek için donanım hızlandırılmış 2B işleme kullanan bir görünümüdür.

![Örnek doku görünümü](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[Araç Çubuğu](~/android/user-interface/controls/tool-bar/index.md)

`Toolbar` (Android 5.0 Lolipop içinde sunulan) pencere öğesi zorlayıcı bir eylem çubuğu arabirimi Genelleştirme &ndash; Eylem çubuğu değiştirmek için tasarlanmıştır. `Toolbar` Bir uygulama düzende herhangi bir kullanılabilir ve bir eylem çubuğu çok daha özelleştirilebilir.

![Örnek araç çubuğu](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager` Sol ve sağ veri sayfaları aracılığıyla ters çevirmek kullanıcının sağlayan bir düzen yöneticisidir.

![Örnek ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[Web görünümü](~/android/user-interface/controls/web-view.md)

`WebView` web sayfalarını görüntüleme için kendi pencere oluşturun (veya hatta tam bir tarayıcı geliştirmek) izin veren bir UI öğedir.

![Örnek Web görünümü](images/web-view.png)

