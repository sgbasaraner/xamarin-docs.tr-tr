---
title: Xamarin.Forms kılavuz
description: Bu makalede, satırları ve sütunları sahip kılavuzlarda görünümleri sunmak için Xamarin.Forms kılavuz sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 01dd59d5e94b473316b03f9035d38305fad42880
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994509"
---
# <a name="xamarinforms-grid"></a>Xamarin.Forms kılavuz

[`Grid`](xref:Xamarin.Forms.Grid) görünümler, satırlar ve sütunlar halinde düzenleme destekler. Satırları ve sütunları orantılı boyutları veya mutlak boyutları için ayarlanabilir. `Grid` Düzen geleneksel tablolarla karıştırılmamalıdır ve tablo verileri sunmak için tasarlanmamıştır. `Grid` Satır, sütun veya hücre biçimlendirme kavramı yoktur. HTML tabloları aksine `Grid` tamamen içeriği teslim düzenlemek için tasarlanmıştır.

[![](grid-images/layouts-sml.png "Xamarin.Forms düzenleri")](grid-images/layouts.png#lightbox "Xamarin.Forms düzenleri")

Bu makalede ele alınacaktır:

- **[Amaç](#Purpose)**  &ndash; yaygın kullanımları için `Grid`.
- **[Kullanım](#Usage)**  &ndash; nasıl kullanılacağını `Grid` istenen tasarımınızı elde etmek için.
  - **[Satırları ve sütunları](#Rows_and_Columns)**  &ndash; satır ve sütun belirtin `Grid`.
  - **[Görünümleri yerleştirme](#Placing_Views)**  &ndash; görünümleri kılavuz belirli satırlar ve sütunlar ekleyin.
  - **[Aralık](#Spacing)**  &ndash; satırları ve sütunları arasındaki boşluklar yapılandırın.
  - **[Yayılma](#Spans)**  &ndash; satırlar veya sütunlar arasında yaymasına izin öğelerini yapılandırın.

![](grid-images/grid.png "Kılavuz keşfetme")

## <a name="purpose"></a>Amaç

`Grid` bir kılavuza görünümleri düzenlemek için kullanılabilir. Bu durumda bir süre içinde yararlıdır:

- Hesaplayıcı uygulama düğmeleri düzenleme
- Kılavuz, iOS veya Android giriş ekranlarını gibi düzenleyerek düğmeleri/seçenekleri
- Böylece bir boyut (bazı araç çubuklarında benzer) eşit boyutta olmaları görünümleri düzenleme

## <a name="usage"></a>Kullanım

Geleneksel tabloları aksine `Grid` sayısını ve boyutunu satırları ve sütunları içeriğinden Infer değil. Bunun yerine, `Grid` sahip `RowDefinitions` ve `ColumnDefinitions` koleksiyonları. Bu, kaç satır ve sütun düzenleneceğini tanımları tutun. Görünümleri eklenir `Grid` belirtilmiş satır ve sütun dizinlerini ile hangi Başkanın hangi satır ve sütun bir görünüm yerleştirilmelidir.

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>Satırları ve sütunları

Satır ve sütun bilgisi depolanan `Grid`'s `RowDefinitions`  &  `ColumnDefinitions` her koleksiyonları özellikleri, [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) ve [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition)nesneleri, sırasıyla. `RowDefinition` tek bir özelliğe sahip `Height`, ve `ColumnDefinition` tek bir özelliğe sahip `Width`. Yükseklik ve genişlik seçeneklerini aşağıdaki gibidir:

- **Otomatik** &ndash; satır veya sütun içeriği sığdıracak şekilde otomatik olarak boyutları. Belirtildiği şekilde [ `GridUnitType.Auto` ](xref:Xamarin.Forms.GridUnitType) C# veya olarak `Auto` XAML içinde.
- **Proportional(*)** &ndash; satırları ve sütunları kalan alanı tabanının oranı boyutlandırır. Bir değer olarak belirtilen ve `GridUnitType.Star` C# ve olarak `#*` , XAML içinde ile `#` istenen değerinizi oluşturuluyor. İçeren bir satır/sütun belirterek `*` kullanılabilir alanı dolduracak şekilde neden olur.
- **Mutlak** &ndash; belirli, sabit yükseklik ve genişlik değerlere sahip satırları ve sütunları boyutları. Bir değer olarak belirtilen ve `GridUnitType.Absolute` C# ve olarak `#` , XAML içinde ile `#` istenen değerinizi oluşturuluyor.

> [!NOTE]
> Sütun genişliği değerlerini olarak ayarlanan ' *'' Xamarin.Forms varsayılan olarak, hangi sağlar sütun kullanılabilir alanı doldurur.

Üç satır ve iki sütun gerektiren bir uygulama düşünün. En alttaki tam olarak uzun 200px olması ve en üst satır iki kez Orta satırında yüksekliğinde olması gerekir. Sol sütunda İçeriği sığdırmak için geniş olması gerekir ve kalan alanı dolduracak şekilde sağ sütunda gerekiyor.

XAML içinde:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="2*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="200" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

C# içinde:

```csharp
var grid = new Grid();
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(2, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(200)});
grid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (200) });
```

<a name="Placing_Views" />

### <a name="placing-views-in-a-grid"></a>Kılavuzda görünümleri yerleştirme

Görünümlerde yerleştirmek için bir `Grid` bunları kılavuza alt öğesi olarak ekleyin ve ardından hangi satır ve sütun, ait oldukları belirtmeniz gerekir.

XAML içinde kullanmak `Grid.Row` ve `Grid.Column` yerleştirme belirtmek için tek tek her görünüm. Unutmayın `Grid.Row` ve `Grid.Column` sıfır tabanlı satır ve sütun listesini temel alan konumu belirtin. Bu 4 x 4 kılavuzunda, sol üst hücreyi (0,0) ve alt sağ hücresi (3,3) olduğunu anlamına gelir.

`Grid` Gösterilen aşağıdaki dört alan içerir:

![](grid-images/label-grid.png "Dört görünümleri içeren kılavuz")

XAML içinde:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Label Text="Top Left" Grid.Row="0" Grid.Column="0" />
  <Label Text="Top Right" Grid.Row="0" Grid.Column="1" />
  <Label Text="Bottom Left" Grid.Row="1" Grid.Column="0" />
  <Label Text="Bottom Right" Grid.Row="1" Grid.Column="1" />
</Grid>
```

C# içinde:

```csharp
var grid = new Grid();

grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});

var topLeft = new Label { Text = "Top Left" };
var topRight = new Label { Text = "Top Right" };
var bottomLeft = new Label { Text = "Bottom Left" };
var bottomRight = new Label { Text = "Bottom Right" };

grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);
```

Yukarıdaki kod, dört etiketleri, iki sütun ve iki satır ile kılavuz oluşturur. Satırları tüm kullanılabilir alanı kullanmak için genişletilir ve her etiket aynı boyuta sahip olacağını unutmayın.

Yukarıdaki örnekte, görünümleri için eklenen [ `Grid.Children` ](xref:Xamarin.Forms.Grid.Children) koleksiyonunu kullanarak [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) sol ve üst bağımsız değişkenleri belirten bir aşırı yükleme. Kullanırken [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) sol belirten bir aşırı yükleme, sağ, üst ve alt bağımsız değişkenler, while sol ve üst bağımsız değişkenler her zaman içinde hücrelere başvurur [ `Grid` ](xref:Xamarin.Forms.Grid), sağa ve alt bağımsız değişkenleri dışında olan hücreleri başvurmak için görünebilir `Grid`. Sağ bağımsız değişkeni her zaman sol bağımsız değişkenden büyük olmalı ve alt bağımsız değişkeni her zaman üst bağımsız değişkenden büyük olmalıdır çünkü budur. Aşağıdaki örnek, her ikisini de kullanarak eşdeğer kod gösterir `Add` aşırı yüklemeler:

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);
grid.Children.Add(topRight, 1, 2, 0, 1);
grid.Children.Add(bottomLeft, 0, 1, 1, 2);
grid.Children.Add(bottomRight, 1, 2, 1, 2);
```

### <a name="spacing"></a>Aralığı

`Grid` satırları ve sütunları arasındaki boşluğu denetlemek için özelliklere sahiptir.  Aşağıdaki özellikleri özelleştirmek için kullanabileceğiniz `Grid`:

- **ColumnSpacing** &ndash; sütun arasındaki boşluk miktarı.
- **RowSpacing** &ndash; satırları arasındaki boşluk miktarı.

Aşağıdaki XAML belirtir bir `Grid` iki sütun, bir satır ve 5 piksel sütunlar arasındaki aralığı:

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinitions Width="*" />
    <ColumnDefinitions Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

C# içinde:

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>Yayılma

Genellikle bir kılavuz ile çalışırken, birden fazla satır veya sütun bulunması gereken bir öğe yok. Basit hesap makinesi uygulaması göz önünde bulundurun:

![](grid-images/calculator.png "Calulator uygulama")

0 düğmesini iki sütun gibi her platform için yerleşik hesaplayıcıları üzerinde yer aldığını unutmayın. Kullanılarak elde edilir `ColumnSpan` özelliği bir öğe kaç sütun kaplayan belirtir. Bu düğme için XAML:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

C# ve:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

İçinde bu kod, statik yöntemlerini Not `Grid` değişiklikler de dahil olmak üzere konumlandırma değişiklikleri gerçekleştirmek için kullanılan sınıf `ColumnSpan` ve `RowSpan`. Bunlar değiştirilmeden önce de herhangi bir zamanda ayarlanabilir diğer özelliklerin aksine, statik yöntemler kullanılarak ayarlanan özellikler zaten gerekir, kılavuz unutmayın.

Yukarıdaki hesaplayıcısı uygulaması için tam XAML aşağıdaki gibidir:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.CalculatorGridXAML"
Title = "Calculator - XAML"
BackgroundColor="#404040">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="plainButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#eee"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="darkerButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#ddd"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="orangeButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#E8AD00"/>
                <Setter Property="TextColor" Value="White" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <Grid x:Name="controlGrid" RowSpacing="1" ColumnSpacing="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="150" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Label Text="0" Grid.Row="0" HorizontalTextAlignment="End" VerticalTextAlignment="End" TextColor="White"
        FontSize="60" Grid.ColumnSpan="4" />
            <Button Text = "C" Grid.Row="1" Grid.Column="0"
        Style="{StaticResource darkerButton}" />
            <Button Text = "+/-" Grid.Row="1" Grid.Column="1"
        Style="{StaticResource darkerButton}" />
            <Button Text = "%" Grid.Row="1" Grid.Column="2"
        Style="{StaticResource darkerButton}" />
            <Button Text = "div" Grid.Row="1" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "7" Grid.Row="2" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "8" Grid.Row="2" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "9" Grid.Row="2" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "X" Grid.Row="2" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "4" Grid.Row="3" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "5" Grid.Row="3" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "6" Grid.Row="3" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "-" Grid.Row="3" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "1" Grid.Row="4" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "2" Grid.Row="4" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "3" Grid.Row="4" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "+" Grid.Row="4" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "0" Grid.ColumnSpan="2"
        Grid.Row="5" Grid.Column="0" Style="{StaticResource plainButton}" />
            <Button Text = "." Grid.Row="5" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "=" Grid.Row="5" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Hem üst kılavuza etiketi ve sıfır düğmesi olan bir sütun birden çok occuping dikkat edin. Benzer bir düzen iç içe geçmiş Kılavuzlar kullanarak elde edilebilir olsa da `ColumnSpan`  &  `RowSpan` yaklaşım basittir.

C# uygulaması:

```csharp
public CalculatorGridCode ()
{
  Title = "Calculator - C#";
  BackgroundColor = Color.FromHex ("#404040");

  var plainButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#eee") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var darkerButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#ddd") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var orangeButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#E8AD00") },
      new Setter { Property = Button.TextColorProperty, Value = Color.White },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };

  var controlGrid = new Grid { RowSpacing = 1, ColumnSpacing = 1 };
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (150) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });

  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });

  var label = new Label {
    Text = "0",
    HorizontalTextAlignment = TextAlignment.End,
    VerticalTextAlignment = TextAlignment.End,
    TextColor = Color.White,
    FontSize = 60
  };
  controlGrid.Children.Add (label, 0, 0);

  Grid.SetColumnSpan (label, 4);

  controlGrid.Children.Add (new Button { Text = "C", Style = darkerButton }, 0, 1);
  controlGrid.Children.Add (new Button { Text = "+/-", Style = darkerButton }, 1, 1);
  controlGrid.Children.Add (new Button { Text = "%", Style = darkerButton }, 2, 1);
  controlGrid.Children.Add (new Button { Text = "div", Style = orangeButton }, 3, 1);
  controlGrid.Children.Add (new Button { Text = "7", Style = plainButton }, 0, 2);
  controlGrid.Children.Add (new Button { Text = "8", Style = plainButton }, 1, 2);
  controlGrid.Children.Add (new Button { Text = "9", Style = plainButton }, 2, 2);
  controlGrid.Children.Add (new Button { Text = "X", Style = orangeButton }, 3, 2);
  controlGrid.Children.Add (new Button { Text = "4", Style = plainButton }, 0, 3);
  controlGrid.Children.Add (new Button { Text = "5", Style = plainButton }, 1, 3);
  controlGrid.Children.Add (new Button { Text = "6", Style = plainButton }, 2, 3);
  controlGrid.Children.Add (new Button { Text = "-", Style = orangeButton }, 3, 3);
  controlGrid.Children.Add (new Button { Text = "1", Style = plainButton }, 0, 4);
  controlGrid.Children.Add (new Button { Text = "2", Style = plainButton }, 1, 4);
  controlGrid.Children.Add (new Button { Text = "3", Style = plainButton }, 2, 4);
  controlGrid.Children.Add (new Button { Text = "+", Style = orangeButton }, 3, 4);
  controlGrid.Children.Add (new Button { Text = ".", Style = plainButton }, 2, 5);
  controlGrid.Children.Add (new Button { Text = "=", Style = orangeButton }, 3, 5);

  var zeroButton = new Button { Text = "0", Style = plainButton };
  controlGrid.Children.Add (zeroButton, 0, 5);
  Grid.SetColumnSpan (zeroButton, 2);

  Content = controlGrid;
}
```


## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 17 Xamarin.Forms ile mobil uygulamalar oluşturma](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [Kılavuz](xref:Xamarin.Forms.Grid)
- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
