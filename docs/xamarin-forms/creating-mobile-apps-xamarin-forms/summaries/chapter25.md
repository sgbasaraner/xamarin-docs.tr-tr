---
title: Bölüm 25 özeti. Sayfa çeşit
description: 'Xamarin.Forms ile mobil uygulamaları oluşturma: bölüm 25 özeti. Sayfa çeşit'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ecee7866f4bf9ac1a4f706853434dce2b9cef7f6
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241161"
---
# <a name="summary-of-chapter-25-page-varieties"></a>Bölüm 25 özeti. Sayfa çeşit

Öğesinden türetilen iki sınıf kadarki gördüğünüz `Page`: `ContentPage` ve `NavigationPage`. Bu bölüm iki başkalarının sunar:

- [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) iki sayfaları, bir ana ve bir ayrıntı yönetir
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) birden çok alt sayfa sekmeleri erişilen yönetir

Bu sayfa türleri daha karmaşık Gezinti seçenekler sunan `NavagationPage` ele [Bölüm 24. Sayfa Gezinti](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Ana ve ayrıntı

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Türünün iki özelliklerini tanımlayan `Page`: [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) ve [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/). Bu özelliklerin her biri ayarladığınız genellikle bir `ContentPage`. `MasterDetailPage` Görüntüler ve bu iki sayfaları arasında geçiş yapar.

Bu iki sayfaları arasında geçiş yapmak için iki temel yolu vardır:

- *bölme* ana ve ayrıntı nerede yan yana
- *popover* nereye ayrıntı sayfası kapsayan veya kısmen asıl kapsayan sayfa

Birkaç çeşidi vardır *popover* yaklaşım (*slayt*, *üst üste*, ve *takas*), ancak bunlar genellikle platformu bağımlı. Ayarlayabileceğiniz [ `MasterDetailBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) özelliği `MasterDetailPage` üyesi için [ `MasterBehavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterBehavior/) numaralandırma:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Default/)
- [`Split`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Split/)
- [`SplitOnLandscape`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnLandscape/)
- [`SplitOnPortrait`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnPortrait/)
- [`Popover`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Popover/)

Ancak, bu özelliğin telefonları üzerinde hiçbir etkisi yoktur. Telefonları popover davranışı her zaman vardır. Yalnızca tabletler ve Masaüstü windows bölünmüş davranışa sahip olabilir.

### <a name="exploring-the-behaviors"></a>Davranışları keşfetme

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) örnek varsayılan davranışı farklı cihazlarda deneme olanak tanır. İki ayrı programı içeren `ContentPage` ana ve ayrıntı için türevleri (ile bir `Title` özelliği hem ayarlanmış) ve türetilen başka bir sınıf `MasterDetailPage` bunları birleştirir. Ayrıntı Sayfası sınırlanan bir `NavigationPage` UWP program olmadan çalışmaz, çünkü.

Bir bit eşlem ayarlanması Windows 8.1 ve Windows Phone 8.1 platformlarını gerektiren `Icon` ana sayfasının özelliği.

### <a name="back-to-school"></a>Okul dönüş

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) örnek alanından Öğrenciler görüntülemek için program oluşturmak için biraz farklı bir yaklaşım alır [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) kitaplığı.

`Master` Ve `Detail` visual ağaçlarında ile tanımlanan özellikler [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) türetilen dosya `MasterDetailPage`. Bu düzenlemenin ana ve ayrıntı sayfaları arasında ayarlamak veri bağlamaları sağlar.

XAML dosyası ayrıca ayarlar [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) özelliği `MasterDetailPage` için `True`. Bu, başlangıçta görüntülenecek ana sayfa neden olur; Varsayılan olarak, ayrıntı sayfası görüntülenir. [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) dosya kümeleri `IsPresented` için `false` gelen bir öğe seçildiğinde `ListView` ana sayfasında. Ayrıntı Sayfası sonra görüntülenir:

[![Okul ve ayrıntı Üçlü ekran](images/ch25fg09-small.png "bir MasterDetailPage ayrıntı sayfasından")](images/ch25fg09-large.png#lightbox "bir MasterDetailPage ayrıntı sayfasından")

### <a name="your-own-user-interface"></a>Kendi kullanıcı arabirimi

Xamarin.Forms ana ve ayrıntı görünüm arasında geçiş için bir kullanıcı arabirimi sağlar ancak kendi sağlayabilir. Bunu yapmak için:

- Ayarlama [ `IsGestureEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsGestureEnabled/) özelliğine `false` geçirmeyi devre dışı bırakmak için
- Geçersiz kılma [ `ShouldShowToolbarButton` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton()/) yöntemi ve return `false` Windows 8.1 ve Windows Phone 8. 1 araç çubuğu düğmeleri gizlemek için.

Ardından tarafından gösterildiği gibi ana ve ayrıntı sayfaları arasında geçiş yapmak için bir yol sağlamalısınız [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) örnek.

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) örnek gösteren başka bir yaklaşım kullanarak bir `TapGestureRecognizer` ana ve ayrıntı sayfalarında.

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Sekmeleri kullanarak arasında geçiş yapabilirsiniz sayfaları koleksiyonudur. Öğesinden türetilen `MultiPage<Page>` ve ortak özellikleri ya da kendi yöntemleri tanımlar. [`MultiPage<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), ancak, bir özellik tanımlayın:

- [`Children`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) özellik türü `IList<T>`

Bu dolgu `Children` sayfa nesneleri koleksiyonu.

Başka bir yaklaşım tanımlamanıza olanak verir `TabbedPage` alt çok benzer şekilde bir `ListView` sekmeli sayfaları otomatik olarak oluştur Bu iki özellik kullanarak:

- [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) türü `IEnumerable`
- [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) türü `DataTemplate`

Ancak, koleksiyon daha çok sayıda öğe içerdiğinde bu yaklaşım iyi İos'ta çalışmaz.

`MultiPage<T>` hangi sayfa şu anda görüntülenen izlemenize olanak sağlayan iki tane daha özellik tanımlar:

- [`CurrentPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.CurrentPage/) tür `T`başvuran sayfa
- [`SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.SelectedItem/) tür `Object`nesneye başvuran `ItemsSource` koleksiyonu

`MultiPage<T>` Ayrıca, iki olay tanımlar:

- [`PagesChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.PagesChanged/) zaman `ItemsSource` koleksiyonu değişiklikleri
- [`CurrentPageChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.CurrentPageChanged/) Görüntülenen sayfasını değiştirdiğinde

### <a name="discrete-tab-pages"></a>Ayrık sekme sayfaları

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) üç farklı yolla renkleri görüntüleme üç sekmeli sayfaları örnek oluşur. Her sekme bir `ContentPage` türevi ve ardından `TabbedPage` türevi [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) üç sayfa birleştirir.

Görünür her bir sayfa için bir `TabbedPage`, `Title` özelliği metni sekmesinde belirlemek için gereklidir ve Apple Store simge de kullanılması gerekir böylece `Icon` özelliği, iOS için ayarlanır:

[![Üçlü ekran görüntüsü ayrık sekmeli renkleri](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) örnek sahip tüm Öğrenciler listeleyen bir giriş sayfası. Öğrencinin dokunduğunuz olduğunda bu gider bir `TabbedPage` türevi, [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), üç içerir `ContentPage` nesneleri görsel ağaçta biri sağlar, Öğrenci için bazı notlar girme.

### <a name="using-an-itemtemplate"></a>Bir ItemTemplate kullanma

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) örnek kullanır [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) dosya kümeleri `DataTemplate` özelliği `TabbedPage` bir görsel ağaç başlayan `ContentPage` özelliklerini bağlamalar içeren `NamedColor` ( bağlamayadahil`Title` özelliği).

Ancak, bu İos'ta sorunlu oluşturur. Yalnızca birkaç öğelerin görüntülenebilir ve simgeler vermek için iyi bir yolu yoktur.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 25 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Bölüm 25 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Ana-Ayrıntı Sayfası](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Sekmeli sayfası](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
