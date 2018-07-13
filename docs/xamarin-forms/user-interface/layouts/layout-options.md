---
title: Xamarin.Forms içinde Düzen Seçenekleri
description: Xamarin.Forms her görünüm türü LayoutOptions HorizontalOptions ve VerticalOptions özellikleri vardır. Bu makalede hizalama ve görünüm genişletme her LayoutOptions değeri olan etkisini açıklar.
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: 1ede5f75925a3dafa93062d147fa349ff91f07d2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995316"
---
# <a name="layout-options-in-xamarinforms"></a>Xamarin.Forms içinde Düzen Seçenekleri

_Xamarin.Forms her görünüm türü LayoutOptions HorizontalOptions ve VerticalOptions özellikleri vardır. Bu makalede hizalama ve görünüm genişletme her LayoutOptions değeri olan etkisini açıklar._

## <a name="overview"></a>Genel Bakış

[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Yapısı iki Düzen tercihlerinizin kapsar:

- **Hizalama** – görüntüleme, konum ve boyut üst düzenini içinde belirleyen hizalama, tercih edilen.
- **Genişletme** – yalnızca tarafından kullanılan bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)ve varsa görünüm fazladan boşluk kullanması gerekip gerekmediğini belirtir.

Bu düzen tercihlerinizin uygulanabilir bir [ `View` ](xref:Xamarin.Forms.View), göreli olarak ayarlayarak, üst [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) veya [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özelliği `View` genel alanlarından birine [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) yapısı. Genel alanlar aşağıdaki gibidir:

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`Start`, `Center`, `End`, Ve `Fill` görünümün üst düzenini hizalamayı tanımlamak için kullanılan alanlar:

- Yatay hizalama için [ `Start` ](xref:Xamarin.Forms.LayoutOptions.Start) konumları [ `View` ](xref:Xamarin.Forms.View) , sol tarafta üst düzen ve dikey hizalama yerleştirir `View` üstünde üst düzen.
- Yatay ve dikey hizalama [ `Center` ](xref:Xamarin.Forms.LayoutOptions.Center) yatay veya dikey olarak ortalar [ `View` ](xref:Xamarin.Forms.View).
- Yatay hizalama [ `End` ](xref:Xamarin.Forms.LayoutOptions.End) konumları [ `View` ](xref:Xamarin.Forms.View) sağ tarafta üst düzen ve dikey hizalama, onu konumlandırır `View` altındaki üst düzeni.
- Yatay hizalama [ `Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill) sağlar [ `View` ](xref:Xamarin.Forms.View) genişliğini kaplayacak şekilde üst düzeni ve dikey hizalama, sağlar `View` doldurur. üst düzen yüksekliği.

`StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, Ve `FillAndExpand` değerleri hizalama tercih tanımlamak için kullanılır ve olup görünümü daha fazla alan üst içinde varsa kaplayacağı [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> Varsayılan değer olan bir görünümün [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) ve [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özellikleri [ `LayoutOptions.Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill).

<a name="alignment" />

## <a name="alignment"></a>Hizalama

Hizalama denetimleri üst düzen kullanılmayan alanı içerdiğinde bir görünümü kendi üst düzen içinde nasıl konumlandırılmış (diğer bir deyişle, üst düzen tüm alt öğelerini birleşik boyutundan daha büyük).

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) yalnızca uyar `Start`, `Center`, `End`, ve `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) ters yönde olan alt görünümleri ile sekmesindeki alanları için `StackLayout` yönü. Bu nedenle, alt bir dikey yönlendirilmiş içinde görünümler `StackLayout` ayarlayabilirsiniz kendi [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) özellikleri birine `Start`, `Center`, `End`, veya `Fill` alanları. Benzer şekilde, alt görüntüleyen bir yatay olarak yönlendirilmiş içinde `StackLayout` ayarlayabilirsiniz kendi [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özellikleri birine `Start`, `Center`, `End`, veya `Fill` alanları.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) saygı `Start`, `Center`, `End`, ve `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) alanları aynı yönde olan alt görünümleri `StackLayout` yönü. Bu nedenle, bir dikey yönlendirilmiş `StackLayout` yoksayar `Start`, `Center`, `End`, veya `Fill` üzerinde ayarlandıysa alanları [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) alt Görünüm Özellikleri. Benzer şekilde, bir yatay olarak yönlendirilmiş `StackLayout` yoksayar `Start`, `Center`, `End`, veya `Fill` üzerinde ayarlandıysa alanları [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) alt Görünüm Özellikleri.

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) geçersiz kılmaları kullanarak belirtilen istekleri genelde boyut [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) ve [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) özellikleri.

Aşağıdaki XAML kodu örnek dikey yönlendirilmiş gösterir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) burada her alt [ `Label` ](xref:Xamarin.Forms.Label) ayarlar kendi [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) özelliği dört hizalama alanlarından birine [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) yapısı:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Eşdeğer C# kodu aşağıda gösterilmiştir:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

Kod, aşağıdaki ekran görüntülerinde gösterilen düzenini sonuçlanır:

[![](layout-options-images/alignment.png "Hizalama düzen seçeneklerini")](layout-options-images/alignment-large.png#lightbox "hizalama Düzen Seçenekleri")

<a name="expansion" />

## <a name="expansion"></a>Genişletme

Genişletme bir görünüm içindeki kullanılabilir olması durumunda daha fazla alanı kaplar olup olmadığını denetleyen bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Varsa `StackLayout` kullanılmayan alanı içerir (diğer bir deyişle, `StackLayout` tüm alt öğelerini birleşik boyutundan daha büyük olan), kullanılmayan eşit olarak ayarlayarak, genişleme isteği tüm bağımlı görünümler tarafından paylaşılan kendi [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)veya [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özelliklerine bir [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) kullanan alanı `AndExpand` soneki. Dikkat edin tüm alanında `StackLayout` olan kullanılan genişletme seçenekleri etkisi yoktur.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) yalnızca alt görünüm yönünü yönünde genişletebilirsiniz. Bu nedenle, bir dikey yönlendirilmiş `StackLayout` ayarlamak alt görünümleri genişletebilirsiniz kendi [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özellikleri birine `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, veya `FillAndExpand` , alanlarda `StackLayout` kullanılmayan alanı içerir. Benzer şekilde, bir yatay olarak yönlendirilmiş `StackLayout` ayarlamak alt görünümleri genişletebilirsiniz kendi [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) özellikleri birine `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, veya `FillAndExpand` , alanlarda `StackLayout` kullanılmayan alanı içerir.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) yönünü yönün alt görünümlerde genişletilemiyor. Bu nedenle, üzerinde dikey yönlendirilmiş `StackLayout`ayarını [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) alt görünümü özelliği [ `StartAndExpand` ](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) özelliği ayarlamak aynı etkiye sahip [ `Start`](xref:Xamarin.Forms.LayoutOptions.Start).

> [!NOTE]
> Onu kullanmadığı sürece genişletmesini etkinleştirme bir görünümün boyutunu değişmemesini Not [ `LayoutOptions.FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand).

Aşağıdaki XAML kodu örnek dikey yönlendirilmiş gösterir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) burada her alt [ `Label` ](xref:Xamarin.Forms.Label) ayarlar kendi [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özelliği dört genişletme alanlarından birine [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) yapısı:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

Eşdeğer C# kodu aşağıda gösterilmiştir:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

Kod, aşağıdaki ekran görüntülerinde gösterilen düzenini sonuçlanır:

[![](layout-options-images/expansion.png "Genişletme düzen seçeneklerini")](layout-options-images/expansion-large.png#lightbox "genişletme Düzen Seçenekleri")

Her [ `Label` ](xref:Xamarin.Forms.Label) kapladığı alanı içinde aynı miktarda [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Ancak, yalnızca en son `Label`, hangi kümeleri kendi [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özelliğini [ `FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) farklı bir boyuta sahiptir. Ayrıca, her `Label` küçük kırmızı tarafından ayrılmış [ `BoxView` ](xref:Xamarin.Forms.BoxView), alanı sağlayan `Label` kolayca görüntülenmesine izin kaplar.

## <a name="summary"></a>Özet

Bu makalede açıklanan etkisi her [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) yapısı değerine sahip hizalama ve üst öğesiyle ilişkili bir görünümünün genişletme. `Start`, `Center`, `End`, Ve `Fill` üst düzen görünümün hizalama tanımlamak için kullanılan alanlar ve `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, ve `FillAndExpand` tanımlamak için kullanılan alanlar Hizalama tercihi ve görünümü içinde kullanılabilir olması durumunda daha fazla alanı dolduracaktır olup olmadığını belirlemek için bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).



## <a name="related-links"></a>İlgili bağlantılar

- [LayoutOptions (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
