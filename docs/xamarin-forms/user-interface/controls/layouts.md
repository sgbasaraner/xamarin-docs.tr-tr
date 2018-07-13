---
title: Xamarin.Forms düzenleri
description: Xamarin.Forms düzenleri visual yapılarda kullanıcı arabirimi denetimleri oluşturmak için kullanılır. Xamarin.Forms içinde bulunan düzenleri bu makalede listelenmektedir.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 224eb2ee3958e5979382a3dc5e70110fdce51879
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994608"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms düzenleri

_Xamarin.Forms düzenleri visual yapılarda kullanıcı arabirimi denetimleri oluşturmak için kullanılır._

[ `Layout` ](xref:Xamarin.Forms.Layout) Ve [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) sınıflardır Xamarin.Forms içinde özel alt türleri, görünümleri ve diğer düzenleri için kapsayıcı işlevi gören bir görünüm. `Layout` Sınıfın kendisi türetilir [ `View` ](views.md). A `Layout` Türev genellikle Xamarin.Forms uygulamalarında alt öğelerinin boyutunu ve konumunu ayarlamak için mantığı içerir.

[![Xamarin.Forms düzeni türleri](layouts-images/layouts-sml.png "Xamarin.Forms düzeni türleri")](layouts-images/layouts.png#lightbox "Xamarin.Forms düzeni türleri")

Öğesinden türetilen sınıfları `Layout` iki kategoriye ayrılabilir:

## <a name="layouts-with-single-content"></a>Tek bir içerik düzenleri

Bu sınıfların türetilmesi [ `Layout` ](xref:Xamarin.Forms.Layout), tanımlayan [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) ve [ `IsClippedToBounds` ](xref:Xamarin.Forms.Layout.IsClippedToBounds) özellikleri.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView) tek bir alt kümesi içeren [ `Content` ](xref:Xamarin.Forms.ContentView.Content) özelliği. `Content` Özelliği herhangi birine ayarlanabilir `View` Türev, diğer dahil olmak üzere `Layout` türevleri. `ContentView` çoğunlukla bir yapısal öğesi olarak kullanılır ve bir temel sınıf olarak hizmet veren [ `Frame` ](#frame).<br /><br />[API belgeleri](xref:Xamarin.Forms.ContentView) | [![ContentView örnek](layouts-images/ContentView.png "ContentView örnek")](layouts-images/ContentView-Large.png#lightbox "ContentView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Çerçeve

|     |     |
| --- | --- |
| [ `Frame` ](xref:Xamarin.Forms.Frame) Sınıf türetilir [ `ContentView` ](#contentView) ve kendi alt etrafında dikdörtgen bir çerçeve görüntüler. `Frame` bir varsayılan [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) 20 değerini ve ayrıca tanımlar [ `OutlineColor` ](xref:Xamarin.Forms.Frame.OutlineColor), [ `CornerRadius` ](xref:Xamarin.Forms.Frame.CornerRadius), ve [ `HasShadow` ](xref:Xamarin.Forms.Frame.HasShadow)özellikleri.<br /><br />[API belgeleri](xref:Xamarin.Forms.Frame) | [![Çerçeve örnek](layouts-images/Frame.png "çerçeve örnek")](layouts-images/Frame-Large.png#lightbox "çerçeve örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView) içeriği kaydırmayı yeteneğine sahiptir. Ayarlama [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) görüntülemek veya çok büyük bir düzen ekranda uyacak şekilde özelliği. (İçeriğini bir `ScrollView` çok genellikle bir [ `StackLayout` ](#stackLayout).) Ayarlama [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) kaydırma dikey kaldırılması gerekip gerekmediğini belirtmek için özelliği yatay ya da her ikisini de.<br /><br />[API belgeleri](xref:Xamarin.Forms.ScrollView) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![ScrollView örnek](layouts-images/ScrollView.png "ScrollView örnek")](layouts-images/ScrollView-Large.png#lightbox "ScrollView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) bir denetim şablonu içeriğini görüntüler ve temel sınıfı olan [ `ContentView` ](#contentView).<br /><br />[API belgeleri](xref:Xamarin.Forms.TemplatedView) / [Kılavuzu](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedView örnek](layouts-images/TemplatedView.png "TemplatedView örnek")](layouts-images/TemplatedView.png#lightbox "TemplatedView örneği") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) içinde kullanılan şablonlu görünümleri için Düzen yöneticisidir bir [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) sunulacak olan içerik göründüğü işaretlenecek.<br /><br />[API belgeleri](xref:Xamarin.Forms.ContentPresenter) / [Kılavuzu](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![ContentPresenter örnek](layouts-images/ContentPresenter.png "ContentPresenter örnek")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter örneği") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Birden fazla alt düzenleri

Bu sınıfların türetilmesi [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) alt öğeleri yatay veya dikey olarak ya da temel bir yığın içinde konumlandırır [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) özelliği. [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) Özelliği alt aralığını yönetir ve 6 varsayılan değerine sahiptir.<br /><br />[API belgeleri](xref:Xamarin.Forms.StackLayout) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![StackLayout örnek](layouts-images/StackLayout.png "StackLayout örnek")](layouts-images/StackLayout-Large.png#lightbox "StackLayout örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Kılavuz

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid) alt öğeleri satır ve sütunları bir kılavuzda yerleştirir. Kullanarak bir çocuk konumu gösterilen [iliştirilmiş özellikler](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](xref:Xamarin.Forms.Grid.RowProperty), [ `Column` ](xref:Xamarin.Forms.Grid.ColumnProperty), [ `RowSpan` ](xref:Xamarin.Forms.Grid.RowSpanProperty), ve [ `ColumnSpan` ](xref:Xamarin.Forms.Grid.ColumnSpanProperty).<br /><br />[API belgeleri](xref:Xamarin.Forms.Grid) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/grid.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Grid örneği](layouts-images/Grid.png "Grid örneği")](layouts-images/Grid-Large.png#lightbox "Grid örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) alt öğeleri üst öğesiyle ilişkili belirli konumlara yerleştirir. Kullanarak bir çocuk konumu gösterilen [iliştirilmiş özellikler](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) ve [ `LayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty). Bir `AbsoluteLayout` görünümleri konumlarını animasyon için kullanışlıdır.<br /><br />[API belgeleri](xref:Xamarin.Forms.AbsoluteLayout) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![AbsoluteLayout örnek](layouts-images/AbsoluteLayout.png "AbsoluteLayout örnek")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) ile [arka plan kod](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) göreli alt öğeleri konumlandırır `RelativeLayout` kendisini veya kendi eşdüzey. Kullanarak bir çocuk konumu gösterilen [iliştirilmiş özellikler](~/xamarin-forms/xaml/attached-properties.md) türündeki nesneler için belirlenen [ `Constraint` ](xref:Xamarin.Forms.Constraint) ve [ `BoundsConstraint` ](xref:Xamarin.Forms.Constraint).<br /><br />[API belgeleri](xref:Xamarin.Forms.RelativeLayout) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![RelativeLayout örnek](layouts-images/RelativeLayout.png "RelativeLayout örnek")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) CSS dayalı [esnek kutusu düzeni Modülü](http://www.w3.org/TR/css-flexbox-1/)olarak bilinen yaygın olarak _esnek Düzen_ veya _flex-box_. `FlexLayout` altı bağlanabilir özellikler ve Yığılmış ya da birçok hizalama ve yönlendirme seçenekleriyle sarmalanmış alt izin beş ekli bağlanabilir özellikler tanımlar.<br /><br />[API belgeleri](xref:Xamarin.Forms.FlexLayout) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/) | [![FlexLayout örnek](layouts-images/FlexLayout.png "FlexLayout örnek")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms'a giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery örnek](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API belgeleri](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
