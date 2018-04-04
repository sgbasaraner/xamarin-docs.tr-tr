---
title: StackLayout
description: Bir boyut görünümleri koleksiyonları sunmak için StackLayout kullanın.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 638243958fce34871089b10185f150492dbd2b0d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="stacklayout"></a>StackLayout

`StackLayout` tek boyutlu bir çizgi ("yığını"), görünümlerde yatay veya dikey olarak düzenler. Öğesindeki görünümler bir `StackLayout` düzen seçenekleri kullanarak Düzen alana göre boyutlandırılmalıdır. Konumlandırma görünümleri düzeni ve görünümleri düzeni seçenekleri için eklenen sıraya göre belirlenir.

[![](stack-layout-images/layouts-sml.png "Xamarin.Forms düzenleri")](stack-layout-images/layouts.png#lightbox "Xamarin.Forms düzenleri")

## <a name="purpose"></a>Amaç

`StackLayout` Diğer görünümlere daha az karmaşıktır. Basit doğrusal arabirimleri görünümlerine ekleyerek oluşturulabilir bir `StackLayout`ve iç içe geçme tarafından oluşturulan daha karmaşık arabirimler.

## <a name="usage--behavior"></a>Kullanım ve davranışı

### <a name="spacing"></a>Aralığı

Varsayılan olarak, `StackLayout` 6px kenar boşluğu görünümler arasında ekleyeceksiniz. Bu yüklenebilir denetlenen veya ayarlayarak kenar olacak şekilde ayarlanmış `Spacing` StackLayout özelliği. Aralığı ve farklı aralığı seçenekleri etkisini nasıl ayarlanacağını gösterir:

XAML'de:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout Spacing="10" x:Name="layout">
      <Button Text="StackLayout" VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView Color="Yellow" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView Color="Green" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" Color="Blue" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

C# ' de:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { Text = "StackLayout", VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var yellowBox = new BoxView { Color = Color.Yellow, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var greenBox = new BoxView { Color = Color.Green, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var blueBox = new BoxView { Color = Color.Blue, VerticalOptions = LayoutOptions.FillAndExpand,
      HorizontalOptions = LayoutOptions.FillAndExpand, HeightRequest = 75 };

    layout.Children.Add(button);
    layout.Children.Add(yellowBox);
    layout.Children.Add(greenBox);
    layout.Children.Add(blueBox);
    layout.Spacing = 10;
    Content = layout;
  }
}
```

Aralık = 0:

![](stack-layout-images/spacing-zero.png "Aralığı StackLayout = 0")

On aralığı:

![](stack-layout-images/spacing-ten.png "Aralığı StackLayout = 10")

### <a name="sizing"></a>Boyutlandırma

Bir görünümde bir StackLayout boyutu hem yüksekliğini ve genişliğini istekleri hem de Düzen Seçenekleri bağlıdır. `StackLayout` doldurma zorlar. Aşağıdaki `LayoutOption`s düzenden kullanılabilir olduğu kadar alan kaplamaları için görünümleri neden olur:

- **CenterAndExpand** &ndash; düzeni görünümde merkezleri ve düzeni, verecektir gibi kadar yer kaplar genişletir.
- **EndAndExpand** &ndash; (Alttan veya en sağ sınır) Düzen sonunda görünümü yerleştirir ve düzeni, verecektir gibi kadar yer kaplar genişletir.
- **FillAndExpand** &ndash; görünümü yerleştirir, böylece dolgu sahiptir ve düzeni, verecektir gibi kadar alanı kaplar.
- **StartAndExpand** &ndash; görünümün düzenini başlangıcında yerleştirir ve üst verecektir gibi kadar alanı kaplar.

Daha fazla bilgi için bkz: [genişletme](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion).

### <a name="positioning"></a>Konumlandırma

Bir StackLayout görünümlerde yerleştirilebilir ve kullanarak boyutta `LayoutOptions`. Her görünüm verilen `VerticalOptions` ve `HorizontalOptions`, nasıl görünümleri kendilerini düzeni göreli konumunu tanımlama. Aşağıdaki önceden tanımlanmış `LayoutOptions` kullanılabilir:

- **Merkezi** &ndash; düzeni görünümde merkezleri.
- **Son** &ndash; (Alttan veya en sağ sınır) Düzen sonunda görünümü yerleştirir.
- **Dolgu** &ndash; dolgu sahip olması görünümü yerleştirir.
- **Başlat** &ndash; görünümün düzenini başlangıcında yerleştirir.

Aşağıdaki kod, ayarı düzen seçenekleri gösterir:

XAML'de:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout x:Name="layout">
      <Button VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

C# ' de:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var oneBox = new BoxView { VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var twoBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var threeBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };

    layout.Children.Add(button);
    layout.Children.Add(oneBox);
    layout.Children.Add(twoBox);
    layout.Children.Add(threeBox);
    Content = layout;
  }
}
```

Daha fazla bilgi için bkz: [hizalama](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment).

## <a name="exploring-a-complex-layout"></a>Karmaşık bir düzen keşfetme

Her düzenleri güçlü ve belirli düzenler oluşturmak için zayıf sahip. Düzen makaleleri bu dizi aynı sayfa düzeni üç farklı düzenleri kullanılarak uygulanan bir örnek uygulaması oluşturuldu.

Aşağıdaki XAML göz önünde bulundurun:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.StackLayoutPage"
BackgroundColor="Maroon"
Title="StackLayouts">
    <ContentPage.Content>
    <ScrollView>
      <StackLayout Spacing="0" Padding="0" BackgroundColor="Maroon">
        <BoxView HorizontalOptions="FillAndExpand" HeightRequest="100"
          VerticalOptions="Start" Color="Gray" />
        <Button BorderRadius="30" HeightRequest="60" WidthRequest="60"
          BackgroundColor="Red" HorizontalOptions="Center" VerticalOptions="Start" />
        <StackLayout HeightRequest="100" VerticalOptions="Start" HorizontalOptions="FillAndExpand" Spacing="20" BackgroundColor="Maroon">
          <Label Text="User Name" FontSize="28" HorizontalOptions="Center"
            VerticalOptions="Center" FontAttributes="Bold" />
          <Entry Text="Bio + Hashtags" TextColor="White"
            BackgroundColor="Maroon" HorizontalOptions="FillAndExpand" VerticalOptions="CenterAndExpand" />
        </StackLayout>
        <StackLayout Orientation="Horizontal" HeightRequest="50" BackgroundColor="White" Padding="5">
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="Start">
            <BoxView BackgroundColor="Black" WidthRequest="40" HeightRequest="40"  HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="EndAndExpand">
            <BoxView BackgroundColor="Maroon" WidthRequest="40" HeightRequest="40" HorizontalOptions="Start" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Age:" TextColor="White" HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="35" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Interests:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100"/>
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin, C#, .NET, Mono..." TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
      </StackLayout>
    </ScrollView>
    </ContentPage.Content>
</ContentPage>

```

Yukarıdaki kod aşağıdaki düzende sonuçları:

![](stack-layout-images/stack.png "Karmaşık StackLayout")

Düğmeleri Windows Phone tarafından nasıl işlendiğini içinde bir fark nedeniyle dikkat edin, daireleri bazıları, Windows Phone ekran görüntüsü boxviews tarafından değiştirilmiştir.

Dikkat `StackLayouts`s iç içe geçmiş, bazı durumlarda düzenleri iç içe geçme aynı düzen içindeki tüm öğeler sunma daha kolay olabilir çünkü. Ayrıca, çünkü dikkat `StackLayout` çakışan öğeleri sayfa değil sahip düzeni niceties bazıları içinde bulunan diğer düzenleri sayfaları desteklemiyor.



## <a name="related-links"></a>İlgili bağlantılar

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
