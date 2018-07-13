---
title: Xamarin.Forms sayfaları
description: Xamarin.Forms sayfaları, platformlar arası mobil uygulama ekranları temsil eder. Xamarin.Forms içinde bulunan sayfaları bu makalede listelenmektedir.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: fea1db6c65e533692601439f4712d15371740644
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995904"
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms sayfaları

_Xamarin.Forms sayfaları, platformlar arası mobil uygulama ekranları temsil eder._

Aşağıda açıklanan tüm sayfa türleri Xamarin.Forms türetilen [ `Page` ](xref:Xamarin.Forms.Page) sınıfı. Bu görsel öğelerin tümünün veya çoğunun ekranı kaplar. A `Page` nesnesini temsil eder bir `ViewController` ios'ta ve `Page` Evrensel Windows Platform. Android, ekran gibi her sayfanın kaplar bir `Activity`, ancak Xamarin.Forms sayfalar *değil* `Activity` nesneleri.

[ ![](pages-images/pages-sml.png "Xamarin.Forms sayfasını")](pages-images/pages.png#lightbox "türleri Xamarin.Forms sayfası")

## <a name="pages"></a>Sayfaları

Xamarin.Forms aşağıdaki sayfayı türlerini destekler:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) Basit ve en yaygın sayfası türüdür. Ayarlama [ `Content` ](xref:Xamarin.Forms.ContentPage.Content) tek bir özelliğe [ `View` ](views.md) çoğunlukla nesne bir [ `Layout` ](layouts.md) gibi [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), veya [ `ScrollView` ](layouts.md#scrollView).<br /><br />[API belgeleri](xref:Xamarin.Forms.ContentPage) | [![ContentPage örnek](pages-images/ContentPage.png "ContentPage örnek")](pages-images/ContentPage-Large.png#lightbox "ContentPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| A [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) bilgilerin iki bölme yönetir. Ayarlama [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) özelliğini genellikle listede veya menüde gösteren bir sayfa. Ayarlama [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) özelliğini seçili bir öğe ana sayfasından gösteren bir sayfa. [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) Özelliğini ana veya ayrıntı sayfası görülüp görülemeyeceğini yönetir.<br /><br />[API belgeleri](xref:Xamarin.Forms.MasterDetailPage) / [Kılavuzu](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![MasterDetailPage örnek](pages-images/MasterDetailPage.png "MasterDetailPage örnek")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) ile [arka plan kod](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Yığın tabanlı bir mimari kullanarak diğer sayfalar arasında gezinmeyi yönetir. Sayfa gezintisi uygulamanızda kullanırken, giriş sayfasının bir örneği oluşturucuya geçirilen bir `NavigationPage` nesne.<br /><br />[API belgeleri](xref:Xamarin.Forms.NavigationPage) / [Kılavuzu](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), ve [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![NavigationPage örnek](pages-images/NavigationPage.png "NavigationPage örnek")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) ile [kod arkasında =](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) Özet türetilen [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) sınıfı ve alt arasında gezinme sekmeleri kullanarak sayfalar sağlar. Ayarlama [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) özellik sayfaları veya kümesi koleksiyonuna [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) veri nesnelerinin bir koleksiyonunu özelliğini ve [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) özelliğini bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) her nesnenin nasıl görsel olarak gösterilemeyecek kadar açıklayan.<br /><br />[API belgeleri](xref:Xamarin.Forms.TabbedPage) / [Kılavuzu](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) ve [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![TabbedPage örnek](pages-images/TabbedPage.png "TabbedPage örnek")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) Özet türetilen [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) sınıfı ve alt arasında gezinmeyi parmak çekerek sayfaları sağlar. Ayarlama [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) koleksiyonu özelliğini [ `ContentPage` ](#contentPage) nesneleri veya set [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) veri nesnelerinin bir koleksiyonunu özelliğini ve [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) özelliğini bir [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) her nesnenin nasıl görsel olarak gösterilemeyecek kadar açıklayan.<br /><br />[API belgeleri](xref:Xamarin.Forms.CarouselPage) / [Kılavuzu](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) ve [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![CarouselPage örnek](pages-images/CarouselPage.png "CarouselPage örnek")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) tam ekran içeriğini bir denetim şablonu ile görüntüler ve temel sınıfı olan [ `ContentPage` ](#contentPage).<br /><br />[API belgeleri](xref:Xamarin.Forms.TemplatedPage) / [Kılavuzu](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedPage örnek](pages-images/TemplatedPage.png "TemplatedPage örnek")](pages-images/TemplatedPage.png "TemplatedPage örneği") |
|     |     |

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms'a giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery örnek](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API belgeleri](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
