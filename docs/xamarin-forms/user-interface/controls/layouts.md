---
title: Xamarin.Forms düzenleri
description: Xamarin.Forms düzenleri visual yapılara kullanıcı arabirimi denetimlerini oluşturmak için kullanılır. Bu makalede Xamarin.Forms dahil düzenlerini listeler.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 6e9889bf8ec748ed2034d63acfec9784d074ca44
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243096"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms düzenleri

_Xamarin.Forms düzenleri visual yapılara kullanıcı arabirimi denetimlerini oluşturmak için kullanılır._

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) Ve [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) Xamarin.Forms sınıflarda olan görünümleri ve diğer düzenleri için kapsayıcı olarak hareket görünümlerinin özelleştirilmiş subtypes. `Layout` Sınıfının kendisi türer [ `View` ](views.md). A `Layout` türevi genellikle alt öğelerinin boyutunu ve konumunu Xamarin.Forms uygulamalarda ayarlamak için mantık içerir.

[![Xamarin.Forms yerleşim türleri](layouts-images/layouts-sml.png "Xamarin.Forms yerleşim türleri")](layouts-images/layouts.png#lightbox "Xamarin.Forms yerleşim türleri")

Öğesinden türetilen sınıflar `Layout` iki kategoriye ayrılabilir:

## <a name="layouts-with-single-content"></a>Tek içerikle düzenleri

Bu sınıf türetin [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), tanımlayan [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) ve [ `IsClippedToBounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.IsClippedToBounds/) özellikleri.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) ile ayarlamak tek bir alt içeren [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) özelliği. `Content` Özelliği herhangi biri ayarlanabilir `View` diğer dahil olmak üzere türevi `Layout` türevleri. `ContentView` genellikle bir yapısal öğesi olarak kullanılır ve için temel sınıf olarak görev yapar [ `Frame` ](#frame).<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) | [![ContentView örnek](layouts-images/ContentView.png "ContentView örnek")](layouts-images/ContentView-Large.png#lightbox "ContentView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Çerçeve

|     |     |
| --- | --- |
| [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) Sınıfı türer [ `ContentView` ](#contentView) ve kendi alt etrafında dikdörtgen kare görüntüler. `Frame` bir varsayılan [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 20 değerini ve ayrıca tanımlar [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/), [ `CornerRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.CornerRadius/), ve [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/)özellikleri.<br /><br />[API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) | [![Çerçeve örnek](layouts-images/Frame.png "çerçeve örnek")](layouts-images/Frame-Large.png#lightbox "çerçeve örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) içeriği kaydırma yeteneğine sahiptir. Ayarlama [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) özelliği bir görünüm veya düzeni çok büyük ekranda sığmayacak. (İçeriğini bir `ScrollView` çok genellikle bir [ `StackLayout` ](#stackLayout).) Ayarlama [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) kaydırma dikey olup olmayacağını belirtmek için özelliği yatay ya da her ikisini de.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![ScrollView örnek](layouts-images/ScrollView.png "ScrollView örnek")](layouts-images/ScrollView-Large.png#lightbox "ScrollView örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) bir denetim şablonu ile içeriğini görüntüler ve temel sınıfı olan [ `ContentView` ](#contentView).<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) / [Kılavuzu](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedView örnek](layouts-images/TemplatedView.png "TemplatedView örnek")](layouts-images/TemplatedView.png#lightbox "TemplatedView örneği") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) içinde kullanılan şablonlu görünümleri için bir düzen yöneticisi olan bir [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) sunulacak olan içerik göründüğü işaretlenecek.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) / [Kılavuzu](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![ContentPresenter örnek](layouts-images/ContentPresenter.png "ContentPresenter örnek")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter örneği") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Birden fazla alt düzenleri

Bu sınıf türetin [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ya da yatay veya dikey temel yığındaki alt öğelerini konumlandırır [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) özelliği. [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) Özelliği alt arasındaki boşluğu yönetir ve 6'ın varsayılan bir değeri yok.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![StackLayout örnek](layouts-images/StackLayout.png "StackLayout örnek")](layouts-images/StackLayout-Large.png#lightbox "StackLayout örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Kılavuz

|     |     |
| --- | --- |
| [`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) alt öğeleri satırları ve sütunları kılavuzunda yerleştirir. Çocuk konumu kullanarak belirtilen [ekli özellikler](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/), [ `Column` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/), [ `RowSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/), ve [ `ColumnSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/).<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/grid.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Kılavuz örnek](layouts-images/Grid.png "kılavuz örnek")](layouts-images/Grid-Large.png#lightbox "kılavuz örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) alt öğe üst göre belirli konumlara yerleştirir. Çocuk konumu kullanarak belirtilen [ekli özellikler](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/) ve [ `LayoutFlags` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/). Bir `AbsoluteLayout` görünümleri konumlarını animasyon için yararlıdır.<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![AbsoluteLayout örnek](layouts-images/AbsoluteLayout.png "AbsoluteLayout örnek")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) ile [arka plan kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) alt öğelerine göreli konumlandırır `RelativeLayout` kendisini veya kendi eşdüzey. Çocuk konumu kullanarak belirtilen [ekli özellikler](~/xamarin-forms/xaml/attached-properties.md) türündeki nesnelere ayarlamak [ `Constraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/) ve [ `BoundsConstraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/).<br /><br />[API belgelerine](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![RelativeLayout örnek](layouts-images/RelativeLayout.png "RelativeLayout örnek")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) CSS tabanlı [Esnek kutusunu düzeni Modülü](http://www.w3.org/TR/css-flexbox-1/), yaygın olarak bilinen _düzeni esnek_ veya _esnek kutu_. `FlexLayout` altı bağlanabilir özellikleri ve Yığılmış veya birçok hizalama ve yönlendirme seçenekleriyle Sarmalanan alt öğelere izin beş ekli bağlanabilir özellikleri tanımlar.<br /><br />[API belgelerine](xref:Xamarin.Forms.FlexLayout) / [Kılavuzu](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/) | [![FlexLayout örnek](layouts-images/FlexLayout.png "FlexLayout örnek")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout örneği")<br />[Bu sayfa için C# kodu](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [XAML sayfası](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms giriş](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery örnek](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API belgeleri](https://developer.xamarin.com/api/root/Xamarin.Forms/)
