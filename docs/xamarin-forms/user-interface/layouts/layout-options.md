---
title: LayoutOptions
description: "Her Xamarin.Forms görünüm türü LayoutOptions HorizontalOptions ve VerticalOptions özellikleri vardır. Bu makalede her LayoutOptions değeri hizalama ve bir görünüm genişlemesi etkisini açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: 978985c4e9803fad33760e4b40ab73d57f3ec420
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="layoutoptions"></a>LayoutOptions

_Her Xamarin.Forms görünüm türü LayoutOptions HorizontalOptions ve VerticalOptions özellikleri vardır. Bu makalede her LayoutOptions değeri hizalama ve bir görünüm genişlemesi etkisini açıklar._

## <a name="overview"></a>Genel Bakış

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Yapısı iki Düzen tercihlerinizin yalıtır:

- **Hizalama** – görünümü, konum ve boyut üst düzenini içinde belirler hizalama tercih edilen.
- **Genişletme** – yalnızca tarafından kullanılan bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)ve kullanılabilir durumdaysa görünümü ek boşluk kullanmanızın gerekli olup gösterir.

Bu düzen tercihlerinizin uygulanabilir bir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), ayarlayarak, üst göreli [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) veya [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özelliği `View` ortak alanlarından birine [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) yapısı. Genel alanlar aşağıdaki gibidir:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`Start`, `Center`, `End`, Ve `Fill` alanları üst düzeni görünümün hizalama tanımlamak için kullanılır:

- Yatay hizalama için [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/) Konumlar [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) sol tarafındaki üst düzeni ve dikey hizalama için onu konumlandırır `View` en üstündeki üst düzeni.
- Yatay ve dikey hizalama [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/) yatay veya dikey merkezleri [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).
- Yatay hizalama için [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/) Konumlar [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) sağ tarafta üst düzeni ve dikey hizalama için onu konumlandırır `View` altındaki üst düzeni.
- Yatay hizalama için [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) sağlar [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) genişliği doldurur üst düzeni ve dikey hizalama, sağlar `View` doldurur. üst düzen yüksekliği.

`StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, Ve `FillAndExpand` değerleri hizalama tercih tanımlamak için kullanılır ve olup görünümü daha fazla alan üst içinde varsa kaplar [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

> [!NOTE]
> Bir görünümün varsayılan değerini [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ve [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özellikleri [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/).

<a name="alignment" />

## <a name="alignment"></a>Hizalama

Hizalama denetimleri üst düzeni kullanılmayan alanı içerdiğinde bir görünüm içinde üst düzenini nasıl konumlandırılır (diğer bir deyişle, üst düzen birleşik tüm alt öğelerini büyük).

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) yalnızca uyar `Start`, `Center`, `End`, ve `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) ters yönde olan alt görünümleri alanları için `StackLayout` yönü. Bu nedenle, alt bir dikey yönelimli içinde görünümleri `StackLayout` ayarlayabilirsiniz kendi [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) birine özellikleri `Start`, `Center`, `End`, veya `Fill` alanları. Benzer şekilde, alt görünümleri bir yatay olarak yönlendirilmiş içinde `StackLayout` ayarlayabilirsiniz kendi [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) birine özellikleri `Start`, `Center`, `End`, veya `Fill` alanları.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) saygı `Start`, `Center`, `End`, ve `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) aynı yönde olan alt görünümleri alanları `StackLayout` yönü. Bu nedenle, bir dikey yönelimli `StackLayout` yoksayar `Start`, `Center`, `End`, veya `Fill` üzerinde ayarlandıysa alanları [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) alt görünümleri özelliklerini. Benzer şekilde, bir yatay olarak yönlendirilmiş `StackLayout` yoksayar `Start`, `Center`, `End`, veya `Fill` üzerinde ayarlandıysa alanları [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) alt görünümleri özelliklerini.

> [!NOTE]
> [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) geçersiz kılmaları kullanarak belirtilen istekleri genellikle boyut [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) ve [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) özellikleri.

Aşağıdaki XAML kod örneğinde dikey yönelimli gösterilmektedir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) burada her alt [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ayarlar kendi [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) özelliği dört hizalama alanlarından birine [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) yapısı:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Eşdeğer C# kod aşağıda verilmiştir:

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

Aşağıdaki ekran görüntülerinde gösterilen düzen kodu sonuçlanır:

[![](layout-options-images/alignment.png "Hizalama düzeni seçeneklerini")](layout-options-images/alignment-large.png "hizalama düzeni seçenekleri")

<a name="expansion" />

## <a name="expansion"></a>Genişletme

Genişletme denetimlerini bir görünüm içinde kullanılabilir olması durumunda daha fazla alan kaplar olup bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Varsa `StackLayout` kullanılmayan alanı içerir (diğer bir deyişle, `StackLayout` birleşik tüm alt büyük), kullanılmayan alanı eşit olarak ayarlayarak genişletme isteği tüm bağımlı görünümler tarafından paylaşılan kendi [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)veya [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özelliklerine bir [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) kullanan alanı `AndExpand` soneki. Unutmayın tüm alanında `StackLayout` olan kullanıldığında, genişletme seçenekleri herhangi bir etkisi yoktur.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) kendi yönelimdeki alt görünümlerinde yalnızca genişletebilirsiniz. Bu nedenle, bir dikey yönelimli `StackLayout` Ayarla alt görünümleri genişletebilirsiniz kendi [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) birine özellikleri `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, veya `FillAndExpand` , alanlarda `StackLayout` kullanılmayan alanı içerir. Benzer şekilde, bir yatay olarak yönlendirilmiş `StackLayout` Ayarla alt görünümleri genişletebilirsiniz kendi [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) birine özellikleri `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, veya `FillAndExpand` , alanlarda `StackLayout` kullanılmayan alanı içerir.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) yönünü yönün alt görünümlerde genişletilemiyor. Bu nedenle, üzerinde dikey yönelimli `StackLayout`, ayar [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) alt görünümüne özellikte [ `StartAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/) özelliğini ayarlamak aynı etkiye sahip [ `Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/).

> [!NOTE]
> Kullandığı sürece genişletme etkinleştirme bir görünüm boyutunu değiştirmez, Not [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/).

Aşağıdaki XAML kod örneğinde dikey yönelimli gösterilmektedir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) burada her alt [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ayarlar kendi [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özelliği dört genişletme alanlarından birine [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) yapısı:

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

Eşdeğer C# kod aşağıda verilmiştir:

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

Aşağıdaki ekran görüntülerinde gösterilen düzen kodu sonuçlanır:

[![](layout-options-images/expansion.png "Genişletme Düzen Seçenekleri")](layout-options-images/expansion-large.png "genişletme Düzen Seçenekleri")

Her [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) aynı miktarda alan içinde kapladığı [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Ancak, yalnızca en son `Label`, hangi kümeleri kendi [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özelliğine [ `FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) farklı bir boyutu vardır. Ayrıca, her `Label` küçük kırmızı tarafından ayrılmış [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/), alan sağlayan `Label` kolayca görüntülenmesine izin kaplar.

## <a name="summary"></a>Özet

Bu makalede etkisi açıklanmıştır ve her [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) yapısı değerine sahip hizalama ve kendi üst göreli bir görünümün genişletme. `Start`, `Center`, `End`, Ve `Fill` alanları üst düzeni görünümün hizalama tanımlamak için kullanılır ve `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, ve `FillAndExpand` alanları tanımlamak için kullanılır Hizalama tercih ve görünümü içinde varsa, daha fazla alan kaplar olup olmadığını belirlemek için bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).



## <a name="related-links"></a>İlgili bağlantılar

- [LayoutOptions (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)
