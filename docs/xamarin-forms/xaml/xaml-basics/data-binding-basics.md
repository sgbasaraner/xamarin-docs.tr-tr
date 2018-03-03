---
title: "Bölüm 4. Veri bağlama temelleri"
description: "Veri bağlamaları bir değişiklik diğer birinde bir değişiklik neden bağlanması iki nesnelerin özelliklerini sağlar. Bu çok değerli bir araç ve veri bağlamaları tamamen kod içinde tanımlanabilir olsa da, XAML kısayolları ve kolaylık sağlar. Sonuç olarak, bir Xamarin.Forms en önemli biçimlendirme uzantılarında bağlama."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 46e0c1f9b2aff52c1d31774a15e818c78a70056a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="part-4-data-binding-basics"></a>Bölüm 4. Veri bağlama temelleri

_Veri bağlamaları bir değişiklik diğer birinde bir değişiklik neden bağlanması iki nesnelerin özelliklerini sağlar. Bu çok değerli bir araç ve veri bağlamaları tamamen kod içinde tanımlanabilir olsa da, XAML kısayolları ve kolaylık sağlar. Sonuç olarak, bir Xamarin.Forms en önemli biçimlendirme uzantılarında bağlama._

## <a name="data-bindings"></a>Veri bağlama

Veri bağlamaları bağlanmak adlı iki nesnelerin özelliklerini *kaynak* ve *hedef*. Kod içinde iki adım gerekli değildir: `BindingContext` kaynak nesneye hedef nesnenin özelliğini ayarlayın ve `SetBinding` yöntemi (genellikle birlikte kullanılan `Binding` sınıfı), bir özelliği bağlamak için hedef nesnede çağrılmalıdır kaynak nesnenin bir özelliğini nesne.

Target özelliği hedef nesne öğesinden türetilmelidir anlamına gelir bağlanabilir bir özellik olmalıdır `BindableObject`. Çevrimiçi Xamarin.Forms belgeleri hangi özelliklerin bağlanabilir özellikleri gösterir. Bir özelliği `Label` gibi `Text` bağlanabilirse özelliğiyle ilişkili `TextProperty`.

Biçimlendirme içinde kodda, gerekli aynı iki adımları da gerçekleştirmelisiniz dışında `Binding` biçimlendirme uzantısı yerini alır `SetBinding` çağırın ve `Binding` sınıfı.

Ancak, veri bağlamaları XAML'de tanımladığınızda, ayarlamak için birden çok yolu vardır `BindingContext` hedef nesnenin. Bazen bazen kullanarak arka plan kodu dosyasından ayarlanmış bir `StaticResource` veya `x:Static` biçimlendirme uzantısı ve içeriği olarak bazen `BindingContext` özellik öğesi etiketleri.

Bağlamaları çoğunlukla bir temel alınan veri modelinde, genellikle bir gerçekleştirme MVVM (Model-View-ViewModel) uygulama mimarisinin bir programın görselleri bağlamak için kullanılır anlatıldığı gibi [bölümü 5. MVVM veri bağlamalar gelen](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), ancak diğer senaryolar mümkündür.

## <a name="view-to-view-bindings"></a>Görünümü görünümü bağlamaları

Aynı sayfada iki görünüm özelliklerini bağlamak için veri bağlamaları tanımlayabilirsiniz. Bu durumda, ayarladığınız `BindingContext` kullanarak hedef nesne `x:Reference` biçimlendirme uzantısı.

İçeren bir XAML dosyası İşte bir `Slider` ve iki `Label` görünümler, bunlardan biri Döndürülmüş tarafından `Slider` değeri ve görüntüleyen başka `Slider` değeri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderBindingsPage"
             Title="Slider Bindings Page">

    <StackLayout>
        <Label Text="ROTATION"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` İçeren bir `x:Name` iki tarafından başvurulan öznitelik `Label` kullanarak görünümleri `x:Reference` biçimlendirme uzantısı.

`x:Reference` Tanımlar adlı bir özellik uzantısı bağlama `Name` başvurulan öğenin adına bu durumda ayarlamak için `slider`. Ancak, `ReferenceExtension` tanımlayan sınıf `x:Reference` biçimlendirme uzantısı da tanımlayan bir `ContentProperty` için öznitelik `Name`, açıkça gerekli olmadığını anlamına gelir. Çeşitli, yalnızca için ilk `x:Reference` içerir "Name =" ancak ikinci desteklemez:

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding` Biçimlendirme uzantısı kendisini olabilir çeşitli özellikler, tıpkı `BindingBase` ve `Binding` sınıfı. `ContentProperty` İçin `Binding` olan `Path`, ancak "yolu =" biçimlendirme uzantısı parçası atlanmış yol listedeki ilk öğe ise `Binding` biçimlendirme uzantısı. İlk örnek sahip "yolu =" ancak ikinci örneği, atlar:

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Özellikleri tüm tek bir satırda olabilir veya birden çok satıra ayrılmış:

```csharp
Text="{Binding Value, 
               StringFormat='The angle is {0:F0} degrees'}"
```

Kullanışlı ne olursa olsun yapın.

Bildirim `StringFormat` ikinci özelliği `Binding` biçimlendirme uzantısı. Xamarin.Forms tüm örtük tür dönüşümleri bağlamaları gerçekleştirmeyin ve dize olarak bir dize olmayan nesnesini görüntülemeniz gerekirse, bir tür dönüştürücüsünü sağlayın veya kullanmalısınız `StringFormat`. Planda, statik `String.Format` yöntemi uygulamak için kullanılan `StringFormat`. Biçimlendirme uzantıları sınırlandırmak için de kullanılır süslü ayraçlar .NET biçimlendirme belirtimleri ilgili olası bir sorunu olmasıdır. XAML ayrıştırıcısı kafa karıştırıcı riski oluşturur. Bunu önlemek için tüm biçimlendirme dizesi tek tırnak işaretleri içine koyun:

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Çalışan program şöyledir:

[ ![](data-binding-basics-images/sliderbinding.png "Görünümü görünümü bağlamaları")](data-binding-basics-images/sliderbinding-large.png "görünümü görünümü bağlamaları ")

## <a name="the-binding-mode"></a>Bağlama modu 

Tek bir görünüm veri bağlamaları birkaç özelliklerini olabilir. Bununla birlikte, her görünüm yalnızca bir bulunabilir `BindingContext`, tüm bu görünümde birden çok veri bağlaması gerekir böylece aynı nesne özelliklerini başvuru.

Bu ve diğer sorunları çözüme içerir `Mode` üyesi için ayarlama özelliği `BindingMode` numaralandırma:

- `Default` 
- `OneWay` — değerleri, hedef kaynak sunucudan aktarılır
- `OneWayToSource` — değerleri, hedef kaynak aktarılır
- `TwoWay` — değerleri, her iki yönde aktarılır, kaynak ve hedef arasında

Aşağıdaki programı bir ortak kullanımını gösteren `OneWayToSource` ve `TwoWay` modları bağlama. Dört `Slider` görünümleri denetimine yöneliktir `Scale`, `Rotate`, `RotateX`, ve `RotateY` özelliklerini bir `Label`. İlk olarak göründüğü gibi bu dört özelliklerini `Label` her ayarlandığından veri bağlama hedefleri olmalıdır bir `Slider`. Ancak, `BindingContext` , `Label` yalnızca bir nesne olabilir ve dört farklı kaydırıcılar vardır.

Bu nedenle, tüm bağlamaları görünen geriye doğru ayarlanır yolları: `BindingContext` her dört kaydırıcılar biri ayarlamak `Label`, ve bağlamaları ayarlanır `Value` kaydırıcılar özelliklerini. Kullanarak `OneWayToSource` ve `TwoWay` modları, bunlar `Value` özellikleri olan kaynak özellikleri ayarlayabilirsiniz `Scale`, `Rotate`, `RotateX`, ve `RotateY` özelliklerini `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderTransformsPage"
             Padding="5"
             Title="Slider Transforms Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <!-- Scaled and rotated Label -->
        <Label x:Name="label"
               Text="TEXT"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <!-- Slider and identifying Label for Scale -->
        <Slider x:Name="scaleSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="1" Grid.Column="0"
                Maximum="10"
                Value="{Binding Scale, Mode=TwoWay}" />

        <Label BindingContext="{x:Reference scaleSlider}"
               Text="{Binding Value, StringFormat='Scale = {0:F1}'}"
               Grid.Row="1" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for Rotation -->
        <Slider x:Name="rotationSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="2" Grid.Column="0"
                Maximum="360"
                Value="{Binding Rotation, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationSlider}"
               Text="{Binding Value, StringFormat='Rotation = {0:F0}'}"
               Grid.Row="2" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationX -->
        <Slider x:Name="rotationXSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="3" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationX, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationXSlider}"
               Text="{Binding Value, StringFormat='RotationX = {0:F0}'}"
               Grid.Row="3" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationY -->
        <Slider x:Name="rotationYSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="4" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationY, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationYSlider}"
               Text="{Binding Value, StringFormat='RotationY = {0:F0}'}"
               Grid.Row="4" Grid.Column="1"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Üç bağlantılarına `Slider` görünümlerdir `OneWayToSource`seçeneğidir; yani, `Slider` değeri özelliğinde bir değişiklik neden olan kendi `BindingContext`, olduğu `Label` adlı `label`. Bu üç `Slider` görünümleri değişiklikleri neden `Rotate`, `RotateX`, ve `RotateY` özelliklerini `Label`.

Ancak, bağlama için `Scale` özelliği `TwoWay`. Bunun nedeni, `Scale` özelliğinin varsayılan değeri 1 ile kullanarak bir `TwoWay` nedenler bağlama `Slider` ilk 0 yerine 1 için ayarlanacak değer. Bu bağlama olsaydı `OneWayToSource`, `Scale` özelliği başlangıçta ayarlanabilir 0'dan `Slider` varsayılan değer. `Label` Görünür olmaz ve kullanıcıya bazı karışıklığa neden.

 [ ![](data-binding-basics-images/slidertransforms.png "Geriye dönük bağlamaları")](data-binding-basics-images/slidertransforms-large.png "geriye doğru bağlamaları")

## <a name="bindings-and-collections"></a>Bağlamalar ve koleksiyonları

Hiçbir şey XAML ve veri bağlamaları şablonlu iyi güç gösterilmektedir `ListView`.

`ListView` tanımlayan bir `ItemsSource` türünde özellik `IEnumerable`, ve bu koleksiyondaki öğelerin görüntüler. Bu öğeler herhangi bir türde nesneler olabilir. Varsayılan olarak, `ListView` kullanan `ToString` bu öğeyi görüntülemek için her öğenin yöntemi. Bazı durumlarda yalnızca istediğinizi, budur ancak birçok durumda `ToString` nesnenin yalnızca tam sınıf adını döndürür.

Ancak, öğeleri `ListView` koleksiyonu kullanım yoluyla istediğiniz herhangi bir şekilde görüntülenebilir bir *şablonu*, türeyen bir sınıf içerir `Cell`. Her öğe için şablon klonlanmış `ListView`, ve şablona ayarlamak veri bağlamaları tek tek klonlar aktarılır.

Çok sık kullanarak bu öğeler için özel bir hücre oluşturmak istersiniz `ViewCell` sınıfı. Bu işlem biraz kod düzensiz ancak XAML'de çok kolay olur.

XamlSamples dahil adlı bir sınıf projedir `NamedColor`. Her `NamedColor` nesnesi `Name` ve `FriendlyName` türünün özelliklerini `string`ve bir `Color` türündeki özelliği `Color`. Ayrıca, `NamedColor` türü 141 statik salt okunur alan `Color` Xamarin.Forms tanımlanan renkleri karşılık gelen `Color` sınıfı. Statik Oluşturucu oluşturur bir `IEnumerable<NamedColor>` içeren koleksiyon `NamedColor` bu statik alanlarına karşılık gelen nesne ve kendi ortak statik olarak atar `All` özelliği.

Statik ayarı `NamedColor.All` özelliğine `ItemsSource` , bir `ListView` kolay kullanarak `x:Static` biçimlendirme uzantısı:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

Sonuç görünen öğeleri gerçekten türünde olmasını kurar `XamlSamples.NamedColor`:

[ ![](data-binding-basics-images/listview1.png "Bir koleksiyona bağlama")](data-binding-basics-images/listview1-large.png "koleksiyona bağlama")

Kadar bilgi değil ancak `ListView` kaydırılabilir ve seçilebilir.

Öğeleri için bir şablon tanımlamak için başlama istersiniz `ItemTemplate` özelliği bir özellik öğesi olarak ve ayarlamak bir `DataTemplate`, hangi sonra başvurular bir `ViewCell`. İçin `View` özelliği `ViewCell` her bir öğe görüntülemek için bir veya daha fazla görünümlerini yerleşimini tanımlayabilirsiniz. Basit bir örnek aşağıda verilmiştir:

```xaml
<ListView ItemsSource="{x:Static local:NamedColor.All}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.View>
                    <Label Text="{Binding FriendlyName}" />
                </ViewCell.View>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

`Label` Ayarlanır `View` özelliği `ViewCell`. ( `ViewCell.View` Etiketleri gerekli değildir çünkü `View` içerik özelliği bir özelliktir `ViewCell`.) Bu biçimlendirme görüntüler `FriendlyName` her özellik `NamedColor` nesnesi:

[ ![](data-binding-basics-images/listview2.png "Bir DataTemplate koleksiyonuyla bağlama")](data-binding-basics-images/listview2-large.png "DataTemplate koleksiyonuyla bağlama")

Daha iyi. Artık gerekli olan tek şey daha fazla bilgi ve gerçek rengi öğesi şablonu ladin için. Bu şablon desteklemek için bazı değerler ve nesneler sayfanın kaynak sözlükte tanımlanmıştır:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <OnPlatform x:Key="boxSize"
                        x:TypeArguments="x:Double">
                <On Platform="iOS, Android, UWP" Value="50" />
            </OnPlatform>

            <OnPlatform x:Key="rowHeight"
                        x:TypeArguments="x:Int32">
                <On Platform="iOS, Android, UWP" Value="60" />
            </OnPlatform>

            <local:DoubleToIntConverter x:Key="intConverter" />

        </ResourceDictionary>
    </ContentPage.Resources>

    <ListView ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <StackLayout Padding="5, 5, 0, 5"
                                 Orientation="Horizontal"
                                 Spacing="15">

                        <BoxView WidthRequest="{StaticResource boxSize}"
                                 HeightRequest="{StaticResource boxSize}"
                                 Color="{Binding Color}" />

                        <StackLayout Padding="5, 0, 0, 0"
                                     VerticalOptions="Center">

                            <Label Text="{Binding FriendlyName}"
                                   FontAttributes="Bold"
                                   FontSize="Medium" />

                            <StackLayout Orientation="Horizontal"
                                         Spacing="0">
                                <Label Text="{Binding Color.R,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat='R={0:X2}'}" />

                                <Label Text="{Binding Color.G,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', G={0:X2}'}" />

                                <Label Text="{Binding Color.B,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', B={0:X2}'}" />
                            </StackLayout>
                        </StackLayout>
                    </StackLayout>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Kullanımına dikkat edin `OnPlatform` boyutunu tanımlamak için bir `BoxView` ve yüksekliğini `ListView` satır. Tüm üç platformlar için değerleri aynı olsa da, biçimlendirme kolayca görünen ince ayar yapmak diğer değerler için uyarlanmış olabilir. 

## <a name="binding-value-converters"></a>Bağlama değer dönüştürücüler

Önceki **ListView Demo** XAML dosyası görüntüler tek tek `R`, `G`, ve `B` Xamarin.Forms özelliklerini `Color` yapısı. Bu özellikleri türlerinin `double` ve 0 ile 1 arasında. Onaltılık değerler görüntülemek istiyorsanız, yalnızca kullanamazsınız `StringFormat` "belirtimi biçimlendirme X2" ile. Yalnızca çalışan tamsayılar ve yanı sıra, `double` değerleri 255 tarafından çarpılacağı gerekir.

Bu küçük sorunu ile Çözüldü bir *değer dönüştürücüsü*de adlı bir *bağlama dönüştürücü*. Uygulayan bir sınıf budur `IValueConverter` adlı iki yöntem olduğu anlamına gelir arabirimi `Convert` ve `ConvertBack`. `Convert` Yöntemi bir değer kaynaktan hedefe; aktarıldığında çağrılır `ConvertBack` yöntemi çağrıldığında aktarımları için hedef kaynağına `OneWayToSource` veya `TwoWay` bağlamaları:

```csharp
using System;
using System.Globalization;
using Xamarin.Forms;

namespace XamlSamples
{
    class DoubleToIntConverter : IValueConverter
    {
        public object Convert(object value, Type targetType,
                              object parameter, CultureInfo culture)
        {
            double multiplier;

            if (!Double.TryParse(parameter as string, out multiplier))
                multiplier = 1;

            return (int)Math.Round(multiplier * (double)value);
        }

        public object ConvertBack(object value, Type targetType,
                                  object parameter, CultureInfo culture)
        {
            double divider;

            if (!Double.TryParse(parameter as string, out divider))
                divider = 1;

            return ((double)(int)value) / divider;
        }
    }
}
```

`ConvertBack` Yöntemi yürütmek bir rolü bu programda bağlamaları yalnızca kaynaktan hedefe tek yönlü olduğundan. 

Bir bağlama dönüştürücü ile bağlama başvuran `Converter` özelliği. Bir bağlama dönüştürücü ile belirtilen bir parametre kabul edebilir `ConverterParameter` özelliği. Bazı çok yönlülük için nasıl çarpanı belirtilen budur. Bağlama dönüştürücü için geçerli bir dönüştürücü parametresi denetler `double` değeri.

Birden fazla bağlama arasında paylaşılabilmesi dönüştürücü kaynak sözlükte örneği:

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

Üç veri bağlamaları bu tek örnek başvuru. Dikkat `Binding` biçimlendirme uzantısı içeren katıştırılmış `StaticResource` biçimlendirme uzantısı:

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

Sonuç şöyledir:

[ ![](data-binding-basics-images/listview3.png "Bir DataTemplate ve dönüştürücüleri koleksiyonuyla bağlama")](data-binding-basics-images/listview3-large.png "DataTemplate ve dönüştürücüleri koleksiyonuyla bağlama")

`ListView` Dinamik olarak temel oluşabilir ve bu değişiklikleri işleme oldukça karmaşık olan veri, ancak yalnızca belirli adımları gerçekleştirin. Öğeleri koleksiyonu için atanmışsa `ItemsSource` özelliği `ListView` değişiklikleri çalışma zamanı sırasında — öğeleri eklenebilir, ise veya koleksiyondan kaldırıldı — kullanmak bir `ObservableCollection` bu öğeler için sınıf. `ObservableCollection` uygulayan `INotifyCollectionChanged` arabirimi ve `ListView` için bir işleyici yükleyecek `CollectionChanged` olay.

Öğelerin özelliklerini değiştirin çalışma zamanı sırasında ardından koleksiyondaki öğelerin uygulamalıdır `INotifyPropertyChanged` arabirimi ve sinyal değişiklikleri özellik değerlerini kullanarak `PropertyChanged` olay. Bu, bu makalenin sonraki bölümünde gösterilen [bölümü 5. MVVM için bağlama verilerden](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Özet

Veri bağlama özellikleri sayfasında iki nesne arasındaki veya görsel nesneler arasında bağlama ve arka plandaki veri için güçlü bir mekanizma belirtin. Ancak, uygulama veri kaynakları ile çalışmaya başladığında, popüler uygulama tasarım örüntüsü yararlı bir standardı çıkmaya başladı. Bu ele alınmıştır [bölümü 5. MVVM veri bağlamalar gelen](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).



## <a name="related-links"></a>İlgili bağlantılar

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1. bölüm. XAML (örnek) ile çalışmaya başlama](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2. bölüm. Temel XAML sözdizimi (örnek)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Bölüm 3. XAML biçimlendirme uzantıları (örnek)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Bölüm 5. Veri bağlama MVVM (örnek)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
