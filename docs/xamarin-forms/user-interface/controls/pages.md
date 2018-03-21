---
title: "Xamarin.Forms sayfaları"
description: "Xamarin.Forms sayfaları platformlar arası mobil uygulama ekranlar temsil eder."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 5f979d2dbb894107d8d606ec1f41de44c294cdd3
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms sayfaları

_Xamarin.Forms sayfaları platformlar arası mobil uygulama ekranlar temsil eder._

Aşağıda açıklanan tüm sayfa türleri Xamarin.Forms türetilen [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) sınıfı. Bu görsel öğelerin tümünü veya bir ekran çoğunu kaplar. A `Page` nesne gösteren bir `ViewController` iOS içinde ve bir `Page` Evrensel Windows Platform. Android, ekran gibi her bir sayfa kaplar bir `Activity`, ancak Xamarin.Forms sayfalarıdır *değil* `Activity` nesneleri.

[ ![](pages-images/pages-sml.png "Xamarin.Forms sayfasını")](pages-images/pages.png#lightbox "türleri Xamarin.Forms sayfası")

## <a name="pages"></a>Sayfaları

Xamarin.Forms aşağıdaki sayfa türlerini destekler:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     | 
| --- | --- | 
| [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) en basit ve en yaygın sayfası türüdür. Ayarlama [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentPage.Content/) tek bir özelliğe [ `View` ](views.md) genellikle nesnesi bir [ `Layout` ](layouts.md) gibi [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), veya [ `ScrollView` ](layouts.md#scrollView).<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) | [![ContentPage örnek](pages-images/ContentPage.png "ContentPage örnek")](pages-images/ContentPage-Large.png#lightbox "ContentPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     | 
| --- | --- | 
| A [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) bilgilerinin iki bölme yönetir. Ayarlama [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) genellikle bir liste veya menü gösteren bir sayfaya özelliği. Ayarlama [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) ana sayfasında seçilen bir öğeyi gösteren bir sayfaya özelliği. [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) Özelliğini ana veya ayrıntı sayfası görünür olup olmadığını yönetir.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) / [Kılavuzu](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![MasterDetailPage örnek](pages-images/MasterDetailPage.png "MasterDetailPage örnek")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) ile [arka plan kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     | 
| --- | --- | 
| [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Yığın tabanlı bir mimari kullanarak diğer sayfaları arasında gezinmeyi yönetir. Sayfa gezintisi, uygulamanızda kullanılırken, giriş sayfası örneği oluşturucusuna iletilmesi gereken bir `NavigationPage` nesnesi.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) / [Kılavuzu](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), ve [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![NavigationPage örnek](pages-images/NavigationPage.png "NavigationPage örnek")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) ile [kod arkasında =](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     | 
| --- | --- | 
| [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Özet türetilen [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) sınıfı ve alt arasında gezinmeyi sekmeleri kullanarak sayfaları sağlar. Ayarlama [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) özellik sayfaları veya kümesi koleksiyonuna [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) veri nesneleri koleksiyonu özelliğine ve [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) özelliği için bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) açıklayan nasıl her görsel olarak gösterilemeyecek kadar nesnesidir.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) / [Kılavuzu](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) ve [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![TabbedPage örnek](pages-images/TabbedPage.png "TabbedPage örnek")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     | 
| --- | --- | 
| [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Özet türetilen [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) sınıfı ve alt arasında gezinmeyi parmak geçirmeyi aracılığıyla sayfaları sağlar. Ayarlama [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) koleksiyonu özelliğine [ `ContentPage` ](#contentPage) nesneleri veya kümesi [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) veri nesneleri koleksiyonu özelliğine ve [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) özelliğine bir [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) açıklayan nasıl her görsel olarak gösterilemeyecek kadar nesnesidir.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) / [Kılavuzu](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [örnek 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) ve [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![CarouselPage örnek](pages-images/CarouselPage.png "CarouselPage örnek")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     | 
| --- | --- | 
| [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) tam ekran içeriğini bir denetim şablonuyla görüntüler ve temel sınıfı olan [ `ContentPage` ](#contentPage).<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) / [Kılavuzu](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedPage örnek](pages-images/TemplatedPage.png "TemplatedPage örnek")](pages-images/TemplatedPage.png "TemplatedPage örneği") |
|     |     |

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery örnek](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API belgeleri](https://developer.xamarin.com/api/root/Xamarin.Forms/)
