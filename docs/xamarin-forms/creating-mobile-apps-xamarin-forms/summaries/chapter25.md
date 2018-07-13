---
title: Bölüm 25 özeti. Sayfa çeşitleri
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: bölüm 25 özeti. Sayfa çeşitleri'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 148388b80137bd335bbb977ea230726da1f4a32d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995891"
---
# <a name="summary-of-chapter-25-page-varieties"></a>Bölüm 25 özeti. Sayfa çeşitleri

Şu ana kadar öğesinden türeyen iki sınıf gördünüz `Page`: `ContentPage` ve `NavigationPage`. Bu bölümde, diğer iki sunar:

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) iki sayfa, bir ana ve bir ayrıntı yönetir
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) birden çok alt sayfa sekmeleri erişilen yönetir

Bu sayfa türler değerinden daha gelişmiş bir gezinti seçenekleri sağlar `NavagationPage` ele [Bölüm 24. Sayfa Gezinti](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Ana ve ayrıntıları

[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) İki tür özelliklerini tanımlar `Page`: [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) ve [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail). İçin bu özelliklerden her biri genel olarak ayarlanmış bir `ContentPage`. `MasterDetailPage` Görüntüler ve bu iki sayfalar arasında geçiş yapar.

Bu iki sayfalar arasında geçiş yapmak için iki temel yolu vardır:

- *bölme* ana ve ayrıntı olduğu yan yana
- *popover* burada ayrıntı sayfası kapsayan veya ana kısmen kapsanan sayfası

Birkaç çeşidi vardır *popover* yaklaşım (*slayt*, *çakışma*, ve *takas*), ancak bunlar genellikle platform bağımlı. Ayarlayabileceğiniz [ `MasterDetailBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) özelliği `MasterDetailPage` üyesinin [ `MasterBehavior` ](xref:Xamarin.Forms.MasterBehavior) sabit listesi:

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

Ancak, bu özellik, telefonlarda etkiye sahip değildir. Telefonlar, her zaman bir popover davranışa sahip. Yalnızca Tablet ve Masaüstü windows bölünmüş davranışa sahip olabilir.

### <a name="exploring-the-behaviors"></a>Davranışlar keşfetme

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) örnek varsayılan davranışını farklı cihazlarda denemeler olanak tanır. İki ayrı programını içeren `ContentPage` türevleri ana ve ayrıntı için (ile bir `Title` özelliği hem de) ve türeyen başka bir sınıf `MasterDetailPage` bunları birleştirir. Ayrıntı Sayfası sınırlanan bir `NavigationPage` çünkü UWP programın bu olmadan çalışmaz.

Bir bit eşlem ayarlanması Windows 8.1 ve Windows Phone 8.1 platformlarını gerektiren `Icon` ana sayfa özelliği.

### <a name="back-to-school"></a>Okul dönüş

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) öğrencilerden görüntülenecek program oluşturmak için örneği biraz farklı bir yaklaşım alır [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) kitaplığı.

`Master` Ve `Detail` özellikleri visual ağaçlarında tanımlanır [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) türetilen dosya `MasterDetailPage`. Bu düzenleme veri bağlamaları ana ve ayrıntı sayfaları arasında ayarlamanızı sağlar.

XAML dosyası ayrıca ayarlar [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) özelliği `MasterDetailPage` için `True`. Bu ana sayfanın başlangıçta görüntülenecek neden olur; Varsayılan ayrıntı sayfası görüntülenir. [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) dosya kümelerini `IsPresented` için `false` gelen bir öğe seçildiğinde `ListView` ana sayfasında. Ayrıntı Sayfası sonra görüntülenir:

[![Okul ve ayrıntı üç ekran](images/ch25fg09-small.png "bir MasterDetailPage ayrıntıları sayfasından")](images/ch25fg09-large.png#lightbox "bir MasterDetailPage sayfasından ayrıntısı")

### <a name="your-own-user-interface"></a>Kendi kullanıcı arabirimi

Xamarin.Forms ana ve ayrıntılı görünüm arasında geçiş yapmak için bir kullanıcı arabirimi sağlar, ancak kendi sağlayabilirsiniz. Bunu yapmak için:

- Ayarlama [ `IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled) özelliğini `false` çekerek devre dışı bırakmak için
- Geçersiz kılma [ `ShouldShowToolbarButton` ](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton) yöntemi ve dönüş `false` Windows 8.1 ve Windows Phone 8. 1 araç çubuğu düğmeleri gizlemek için.

Ardından tarafından gösterildiği gibi ana ve ayrıntı sayfalar arasında geçiş yapmak için bir yol sağlamalıdır [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) örnek.

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) örnek gösterir kullanarak başka bir yaklaşım bir `TapGestureRecognizer` ana şablonunun hem de ayrıntı sayfalarında.

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Sekmeleri kullanarak arasında geçiş yapabilirsiniz sayfaları oluşan bir koleksiyondur. Öğesinden türetilen `MultiPage<Page>` ve hiçbir genel özelliklerini veya kendi yöntemlerini tanımlar. [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1), ancak bir özellik tanımlayın:

- [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) özellik türü `IList<T>`

Bu, doldurmanız `Children` sayfa nesneler ile koleksiyon.

Başka bir yaklaşım tanımlamanıza olanak tanıyan `TabbedPage` alt benzer şekilde bir `ListView` sekmeli sayfalar otomatik olarak oluşturan bu iki özellik kullanarak:

- [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) türü `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) türü `DataTemplate`

Ancak, koleksiyon birkaç taneden fazla öğe içerdiğinde bu yaklaşım da İos'ta çalışmaz.

`MultiPage<T>` hangi sayfa şu anda görüntülenen izlemenize olanak sağlayan iki tane daha özellik tanımlar:

- [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage) tür `T`temsilen sayfası
- [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem) tür `Object`, nesneye başvuran `ItemsSource` koleksiyonu

`MultiPage<T>` Ayrıca, iki olay tanımlar:

- [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged) zaman `ItemsSource` koleksiyonu değişiklikleri
- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged) Görüntülenen sayfasını değiştirdiğinde

### <a name="discrete-tab-pages"></a>Ayrık sekme sayfaları

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) renkler üç farklı şekillerde göstermek sekmeli sayfa üç örnek oluşur. Her sekme bir `ContentPage` Türev ve ardından `TabbedPage` Türev [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) üç sayfası birleştirir.

Görüntülenen her sayfa için bir `TabbedPage`, `Title` özelliği sekmesinde gösterilen yazıyı belirtmek için gereklidir ve Apple Store simge de kullanılması gerekir. böylece `Icon` özelliği, iOS için ayarlanır:

[![Ayrık sekmeli renkler üç ekran](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) örnek tüm Öğrenciler listeleyen bir giriş sayfası sahiptir. Bir öğrenci dokunulduğunda bu gider bir `TabbedPage` Türev, [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), üç içerir `ContentPage` nesneleri, görsel ağaçta biri sağlar, Öğrenci için bazı notları girme.

### <a name="using-an-itemtemplate"></a>Bir ItemTemplate kullanarak

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) kullanan örnek [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) dosya kümelerini `DataTemplate` özelliği `TabbedPage` bir görsel ağacı başlayan `ContentPage` bağlamaları özelliklerini içeren `NamedColor` ( bağlamayadahil`Title` özelliği).

Ancak, İos'ta sorunlu budur. Yalnızca birkaç öğeleri görüntülenebilir ve bunları simgeler vermek için iyi bir yolu yoktur.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 25 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Bölüm 25 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Ana Öğe-Ayrıntı Sayfası](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Sekmeli sayfa](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
