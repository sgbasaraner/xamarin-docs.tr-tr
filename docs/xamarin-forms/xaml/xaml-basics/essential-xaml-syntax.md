---
title: "2. bölüm. Temel XAML sözdizimi"
description: "XAML çoğunlukla başlatmasını ve nesneleri başlatma için tasarlanmıştır. Ancak genellikle, XML dize olarak kolayca gösterilemez karmaşık nesnelere özellikler ayarlanmalı ve bir sınıf tarafından tanımlanan özellikleri üzerinde alt sınıf bazen ayarlamanız gerekir. Bu iki ihtiyaçları ekli özellikler ve özellik öğelerinin temel XAML sözdizimi özellikleri gerektirir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: f99d4b177f5957b2e5f8c22171fe92799af8505a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="part-2-essential-xaml-syntax"></a>2. bölüm. Temel XAML sözdizimi

_XAML çoğunlukla başlatmasını ve nesneleri başlatma için tasarlanmıştır. Ancak genellikle, XML dize olarak kolayca gösterilemez karmaşık nesnelere özellikler ayarlanmalı ve bir sınıf tarafından tanımlanan özellikleri üzerinde alt sınıf bazen ayarlamanız gerekir. Bu iki ihtiyaçları ekli özellikler ve özellik öğelerinin temel XAML sözdizimi özellikleri gerektirir._

## <a name="property-elements"></a>Özellik öğeleri

XAML'de sınıfların özellikleri normalde XML özniteliği ayarlanır:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

Ancak, bir özellik XAML'de ayarlamak için alternatif bir yolu yoktur. Bu seçenek ile denemek için `TextColor`, varolan silmeniz `TextColor` ayarı:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

Boş öğesi açık `Label` başlangıç ve bitiş etiketleri ayırma tarafından etiketi:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

Bu etiketler içinde sınıf adı ve bir noktayla ayrılmış bir özellik adı oluşur başlangıç ve bitiş etiketleri ekleyin:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

Bu gibi bu yeni etiketler içeriği olarak özellik değerini ayarla:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Belirtmek için bu iki yolu `TextColor` özelliği işlevsel olarak eşdeğerdir ancak etkili bir şekilde iki kez niteliğine ve belirsiz olabilir çünkü iki yolla aynı özelliği için kullanmayın.

Bu yeni sözdizimiyle bazı kullanışlı terimleri tanıtılabilir:

-  `Label` olan bir *object öğesi*. Bu bir XML öğesi ifade edilen Xamarin.Forms nesnesidir.
-  `Text`, `VerticalOptions`, `FontAttributes` ve `FontSize` olan *özellik öznitelikleri*. Bunlar XML öznitelikleri olarak ifade edilen Xamarin.Forms özelliklerdir.
-  Bu son parçacığında bulunan `TextColor` hale gelen bir *özellik öğesi*. Bir Xamarin.Forms özellik, ancak bir XML öğesi sunulmuştur.


Konumundaki öğeleri olabilir özelliği tanımı ilk gözükmüyor XML sözdizimi ihlal ancak değil. Dönem XML'de özel bir anlamı yoktur. Bir XML kod çözücü için `Label.TextColor` yalnızca normal alt öğedir.

XAML'de, ancak, bu söz dizimini çok özeldir. Özelliği öğeleri için kuralları biri olan şey, görülebilir `Label.TextColor` etiketi. Özelliğinin değeri her zaman özellik öğesi başlangıç ve bitiş etiketleri arasında içerik olarak tanımlanır.

Birden fazla özellik özellik öğesi sözdizimini kullanabilirsiniz:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Veya tüm özellikler için özellik öğesi sözdizimini kullanabilirsiniz:

```xaml
<Label>
    <Label.Text>
        Hello, XAML!
    </Label.Text>
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
    <Label.VerticalOptions>
        Center
    </Label.VerticalOptions>
</Label>
```

Öncelikle, özellik öğesi sözdizimi oldukça oldukça basit bir şey için bir gereksiz long-winded değiştirme gibi görünebilir ve bu örneklerde, kesinlikle bir durumdur.

Bir özelliğin değerini basit bir dize ifade çok karmaşık ancak özellik öğesi sözdizimi gerekli olur. Özellik öğesi etiketleri başka bir nesne örneği ve özelliklerini ayarlayın. Örneğin, açıkça bir özellik gibi ayarlayabileceğiniz `VerticalOptions` için bir `LayoutOptions` değeri özellik ayarları ile:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

Başka bir örnek: `Grid` adlı iki özelliğe sahip `RowDefinitions` ve `ColumnDefinitions`. Bu iki özellik türlerinin `RowDefinitionCollection` ve `ColumnDefinitionCollection`, koleksiyonları olan `RowDefinition` ve `ColumnDefinition` nesneleri. Bu koleksiyonları ayarlamak için özellik öğesi sözdizimini kullanmanız gerekir.

XAML dosyasının başına İşte bir `GridDemoPage` için özellik öğesi etiketleri gösteren sınıf `RowDefinitions` ve `ColumnDefinitions` koleksiyonlar:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

Piksel genişlik ve yükseklik ve yıldız ayarlarını hücreleri otomatik ölçekli hücreleri tanımlamak için kısaltılmış sözdizimi dikkat edin.

## <a name="attached-properties"></a>Ekli Özellikler

Yalnızca, gördüğünüz `Grid` için özellik öğeleri gerektirir `RowDefinitions` ve `ColumnDefinitions` satırları ve sütunları tanımlamak için koleksiyonları. Ancak, aynı zamanda olması gerekir satır ve sütun göstermek programcı için bir yol olduğu her alt `Grid` yer alıyor.

Her alt etiketi içinde `Grid` satır ve sütun aşağıdaki öznitelikleri kullanarak o alt belirtin:

-  `Grid.Row`
-  `Grid.Column`

0 bu özniteliklerin varsayılan değerlerdir. Ayrıca bir alt birden fazla satır veya sütun bu özniteliklerle yayılan belirtir:

-  `Grid.RowSpan`
-  `Grid.ColumnSpan`

Bu iki özellik 1 varsayılan değerlerine sahiptir.

Tam GridDemoPage.xaml dosya şöyledir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               Grid.Row="0" Grid.Column="0"
               TextColor="White"
               BackgroundColor="Blue" />

        <BoxView Color="Silver"
                 HeightRequest="0"
                 Grid.Row="0" Grid.Column="1" />

        <BoxView Color="Teal"
                 Grid.Row="1" Grid.Column="0" />

        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two rows (or more if you want)"
               Grid.Row="0" Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two columns"
               Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

`Grid.Row` Ve `Grid.Column` ayarları 0 gerekli değildir ancak daha anlaşılır olması amacıyla genellikle dahil edilir.

İşte bu şekilde tüm üç platformlarda görünür:

[ ![](essential-xaml-syntax-images/griddemo.png "Kılavuz düzeni")](essential-xaml-syntax-images/griddemo-large.png "Kılavuz düzeni")

Sözdizimi yalnızca yüksek bu `Grid.Row`, `Grid.Column`, `Grid.RowSpan`, ve `Grid.ColumnSpan` öznitelikleri görünüyor statik alanları ya da özelliklerini `Grid`, ancak yeterli İlginçtir ki `Grid` adlı herhangi bir şey tanımlamıyor `Row`, `Column`, `RowSpan`, veya `ColumnSpan`.

Bunun yerine, `Grid` adlı dört bağlanabilir özelliklerini tanımlar `RowProperty`, `ColumnProperty`, `RowSpanProperty`, ve `ColumnSpanProperty`. Bu özel olarak bilinen bağlanabilir özellikleri türleridir *ekli özellikler*. Tarafından tanımlanan `Grid` ancak set alt öğelerinin sınıfı `Grid`.

Bunlar kullanmak istediğiniz zaman iliştirilmiş kodda, özellik `Grid` sınıfı adlı statik yöntemler sağlar `SetRow`, `GetColumn`, vb. Ancak XAML içinde alt öznitelikleri olarak bu ekli özellikler ayarlanır `Grid` basit özellik adlarını kullanma.

Ekli özellikler her zaman XAML dosyalarında öznitelikleri içeren bir sınıf ve bir noktayla ayrılmış bir özellik adı olarak tanınmıyor. Bunlar adlandırılır *ekli özellikler* bir sınıf tarafından tanımlanan olduğundan (Bu durumda, `Grid`) ancak diğer nesnelere bağlı (Bu durumda, alt öğelerinin `Grid`). Yerleşim sırasında `Grid` , her alt nereye yerleştireceğinizi bilmeniz ekli bu özelliklerin değerlerini sorgulayın.

`AbsoluteLayout` Sınıfı tanımlayan adlı iki ekli özellikler `LayoutBounds` ve `LayoutFlags`. İşte orantılı konumlandırmayı kullanarak gerçekleşen Damalı bir desen ve boyutlandırma özelliklerini `AbsoluteLayout`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.AbsoluteDemoPage"
             Title="Absolute Demo Page">

    <AbsoluteLayout BackgroundColor="#FF8080">
        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

  </AbsoluteLayout>
</ContentPage>
```

Ve aşağıda verilmiştir:

[ ![](essential-xaml-syntax-images/absolutedemo-large.png "Mutlak düzeni")](essential-xaml-syntax-images/absolutedemo-large.png "mutlak düzeni")

Şunun gibi XAML kullanmanın wisdom soru. Elbette yineleme ve düzenli `LayoutBounds` dikdörtgen öneren kodu daha iyi gerçekleşmiş.

Kesinlikle yasal etkinleştirildiğinde ve kullanıcı arabirimleri tanımlarken kod ve biçimlendirme kullanımını Dengeleme hiçbir sorun yok. Bazı görsel XAML'de tanımlama ve daha iyi Döngülerde oluşturulabilir daha fazla bazı görseller eklemek için arka plan kodu dosyanın oluşturucusu kullanma kolaydır.

## <a name="content-properties"></a>İçerik özellikleri

Önceki örneklerde, `StackLayout`, `Grid`, ve `AbsoluteLayout` küme nesneleri `Content` özelliği `ContentPage`, ve bu düzenlerden alt öğeleri gerçekten `Children` koleksiyonu. Henüz bu `Content` ve `Children` özelliklerdir saklanıyorsa XAML dosyasındaki.

Kesinlikle içerebilir `Content` ve `Children` özellikler gibi olarak özellik öğeleri olarak **XamlPlusCode** örnek:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout.Children>
                <Slider VerticalOptions="CenterAndExpand"
                        ValueChanged="OnSliderValueChanged" />

                <Label x:Name="valueLabel"
                       Text="A simple Label"
                       FontSize="Large"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand" />

                <Button Text="Click Me!"
                      HorizontalOptions="Center"
                      VerticalOptions="CenterAndExpand"
                      Clicked="OnButtonClicked" />
            </StackLayout.Children>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Gerçek soru şudur: neden bu özellik öğeleridir *değil* XAML dosyasında gerekli?

XAML'de kullanılmak üzere Xamarin.Forms içinde tanımlanmış öğelere izin verildiğini işaretlenmiş bir özellik `ContentProperty` Sınıf özniteliği. Bakarsanız `ContentPage` sınıfı çevrimiçi Xamarin.Forms belgelerinde, bu öznitelik görürsünüz:

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

Bunun anlamı `Content` özellik öğesi etiketleri gerekli değildir. Başlangıç ve bitiş arasında görünür XML içeriklerin `ContentPage` etiketleri atanacak varsayılır `Content` özelliği.

 `StackLayout`, `Grid`, `AbsoluteLayout`, ve `RelativeLayout` tüm öğesinden türetilen `Layout<View>`, ve bakarsanız `Layout<T>` Xamarin.Forms belgelerinde başka göreceğiniz `ContentProperty` özniteliği:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

Otomatik olarak eklenecek düzeninin içeriğinin `Children` olmadan koleksiyonu açık `Children` özellik öğesi etiketleri.

Diğer sınıflar de `ContentProperty` özniteliği tanımlar. Örneğin, içerik özelliği `Label` olan `Text`. Diğerleri için API belgelerine bakın.

## <a name="platform-differences-with-onplatform"></a>OnPlatform Platform farklılıklar

Tek sayfa uygulamaları ayarlamak için ortak olan `Padding` iOS durum çubuğu üzerine yazılmasını önlemek için sayfada özellik. Kodda, kullandığınız `Device.RuntimePlatform` özelliği bu amaç için:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

Şuna benzer bir XAML kullanarak da yapabilirsiniz `OnPlatform` ve `On` sınıfları. İlk için özellik öğeleri dahil `Padding` özelliği sayfanın üstüne yakın:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

Bu etiketler içinde içeren bir `OnPlatform` etiketi. `OnPlatform` Genel bir sınıftır. Genel tür bağımsız değişkeni, bu durumda, belirtmek zorunda `Thickness`, türündeki `Padding` özelliği. Neyse ki, özellikle adlı genel değişkenleri tanımlamak için XAML özniteliği yok `x:TypeArguments`. Bu ayarı özelliğinin türü ile eşleşmesi gerekir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">

        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`OnPlatform` adlı bir özelliğe sahiptir `Platforms` diğer bir deyişle bir `IList` , `On` nesneleri. Özellik öğesi etiketleri için bu özelliği kullanın:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>

            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Şimdi ekleyin `On` öğeleri. Her onem için ayarlanmış `Platform` özelliği ve `Value` için biçimlendirme özelliğine `Thickness` özelliği:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>
                <On Platform="iOS" Value="0, 20, 0, 0" />
                <On Platform="Android" Value="0, 0, 0, 0" />
                <On Platform="UWP" Value="0, 0, 0, 0" />
            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Bu biçimlendirme basit hale getirilebilir. İçerik özelliği `OnPlatform` olan `Platforms`, bu özellik öğesi etiketleri kaldırılabilmesi için:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android" Value="0, 0, 0, 0" />
            <On Platform="UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`Platform` Özelliği `On` türü `IList<string>`, bu değerleri aynıysa, birden çok platform ekleyebilirsiniz:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android, UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Android ve Windows varsayılan değere ayarlandığından `Padding`, etiket kaldırılabilir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Bu, platforma bağımlı ayarlamak için standart yoludur `Padding` XAML'de özelliği. Varsa `Value` ayarı tek bir dize temsil edilemez, bunun için özellik öğeleri tanımlayabilirsiniz:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS">
                <On.Value>
                    0, 20, 0, 0
                </On.Value>
            </On>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

## <a name="summary"></a>Özet

Özellik öğelerinin ve ekli özellikler ile temel XAML sözdizimi çoğunu kuruldu. Ancak, bazen özelliklerini nesnelere bir dolaylı şekilde, örneğin, bir kaynak sözlüğü ayarlamak gerekir. Bu yaklaşım sonraki bölümü, bölümü ele [3. XAML işaretleme uzantılarına](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).



## <a name="related-links"></a>İlgili bağlantılar

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1. bölüm. XAML ile çalışmaya başlama](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Bölüm 3. XAML işaretleme uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Bölüm 4. Veri bağlama temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Bölüm 5. Verileri için MVVM bağlama](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
