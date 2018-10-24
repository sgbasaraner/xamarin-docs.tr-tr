---
title: 4. bölüm. Temel veri bağlama bilgileri
description: Veri bağlamaları bir değişiklik diğer bir değişikliğe neden bağlanacak iki nesnelerin özelliklerini sağlar.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: ee6571012764e7578fa9ee03493e9f96aa7b45eb
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "35245956"
---
# <a name="part-4-data-binding-basics"></a>4. bölüm. Temel veri bağlama bilgileri

_Veri bağlamaları bir değişiklik diğer bir değişikliğe neden bağlanacak iki nesnelerin özelliklerini sağlar. Bu çok değerli bir araçtır ve veri bağlamalarını tamamen kod içinde tanımlanabilir, ancak XAML kısayolları ve kolaylık sağlar. Sonuç olarak, bir Xamarin.Forms en önemli işaretleme uzantılarında bağlama._

## <a name="data-bindings"></a>Veri bağlamaları

Veri bağlamaları Bağlan adlı iki nesnelerin özelliklerini *kaynak* ve *hedef*. Kodda, iki adım zorunludur: `BindingContext` hedef nesnenin özellik, kaynak nesnesi için ayarlanmış olması gerekir ve `SetBinding` yöntemi (genellikle birlikte kullanılan `Binding` sınıfı) bir özelliği, bağlama için hedef nesne üzerinde çağrılmalıdır kaynak nesnenin bir özelliğini nesne.

Hedef özelliği hedef nesne öğesinden türetilmelidir anlamına gelir. bir bağlanılabilir özellik olmalıdır `BindableObject`. Çevrimiçi Xamarin.Forms belgeleri, hangi özelliklerin bağlanabilir özellikler olduğunu gösterir. Bir özelliği `Label` gibi `Text` bağlanılabilir özellik ile ilişkilendirilen `TextProperty`.

Biçimlendirme içinde ayrıca kodda gerekli aynı iki adımları gerçekleştirmelisiniz dışında `Binding` işaretleme uzantısı yer alan `SetBinding` çağırın ve `Binding` sınıfı.

Ancak, XAML içinde veri bağlamaları tanımladığınızda, ayarlamak için birden çok yolu vardır `BindingContext` hedef nesnenin. Bazen kullanarak arka plan kod dosyasından, bazen ayarlanmış bir `StaticResource` veya `x:Static` işaretleme uzantısı ve içeriği olarak bazen `BindingContext` özellik öğesi etiketleri.

Bağlamaları kullanılır çoğunlukla bir programın görseller genellikle gerçekleştirme MVVM (Model-View-ViewModel) uygulama mimarisi, temel alınan bir veri modeli ile bağlanmak için açıklandığı gibi [5. bölüm. Gelen veri bağlamaları-MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), ancak diğer senaryoları şunlardır.

## <a name="view-to-view-bindings"></a>Görünüm-görünüm bağlamaları

Aynı sayfadaki iki görünüm özellikleri bağlamak için veri bağlama tanımlayabilirsiniz. Bu durumda, ayarladığınız `BindingContext` kullanarak hedef nesne `x:Reference` işaretleme uzantısı.

İçeren bir XAML dosyası İşte bir `Slider` ve iki `Label` görünümleri, biri Döndürülmüş tarafından `Slider` değeri ve görüntüleyen başka `Slider` değeri:

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

`Slider` İçeren bir `x:Name` iki tarafından başvuruda bulunulan öznitelik `Label` kullanarak görünümleri `x:Reference` işaretleme uzantısı.

`x:Reference` Uzantısı bağlama tanımlar adlı bir özellik `Name` başvurulan öğe adı için bu durumda ayarlanacak `slider`. Ancak, `ReferenceExtension` tanımlayan sınıf `x:Reference` işaretleme uzantısı da tanımlayan bir `ContentProperty` özniteliğini `Name`, açıkça gerekli olmadığını anlamına gelir. Çeşitli, yalnızca için ilk `x:Reference` içerir "Name =" ancak ikinci değildir:

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding` İşaretleme uzantısı kendisi olabilir çeşitli özellikleri, tıpkı `BindingBase` ve `Binding` sınıfı. `ContentProperty` İçin `Binding` olduğu `Path`, ancak "yolu =" işaretleme uzantısı bir parçası, yolun ilk öğe olarak varsa atlanabilir `Binding` işaretleme uzantısı. İlk örnek sahip "yolu =" ancak ikinci örnek yok sayar:

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Özellikleri veya çoklu satırlara ayrılmış tüm bir satırda olabilir:

```csharp
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

İnovasyonunuz ne olursa olsun uygundur yapın.

Bildirim `StringFormat` ikinci özellik `Binding` işaretleme uzantısı. Xamarin.Forms içinde herhangi bir örtük tür dönüştürmelerini bağlamaları gerçekleştirmez ve bir dize olarak bir dize olmayan nesne görüntülenecek ihtiyacınız varsa, bir tür dönüştürücüsü sağlayın veya kullanmalısınız `StringFormat`. Planda, statik `String.Format` yöntemi uygulamak için kullanılan `StringFormat`. .NET biçimlendirme özellikleri biçimlendirme uzantıları sınırlandırmak için kullanılan küme ayracı ilgili olası bir sorun olmasıdır. Bu, XAML ayrıştırıcı kafa karıştırıcı riski oluşturur. Bunu önlemek için tüm biçimlendirme dizesi tek tırnak içine koyun:

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Çalışan programa şu şekildedir:

[![](data-binding-basics-images/sliderbinding.png "Görünüm-görünüm bağlamaları")](data-binding-basics-images/sliderbinding-large.png#lightbox "görünüm-görünüm bağlamaları ")

## <a name="the-binding-mode"></a>Bağlama modu

Tek bir görünümde çeşitli özelliklerini veri bağlamaları olabilir. Her görünüm tek ancak sahip `BindingContext`, tüm bu görünümünde birden çok veri bağlamaları gerekir böylece başvuru aynı nesne özelliklerini.

Bu ve diğer sorunların çözümü içerir `Mode` üyesi için ayarlanan özellik `BindingMode` sabit listesi:

- `Default`
- `OneWay` — değerleri, hedef kaynak sunucudan aktarılır
- `OneWayToSource` — değerleri hedeften kaynağa aktarılır
- `TwoWay` — değerler, her iki yönde aktarılır, kaynak ve hedef arasında

Aşağıdaki program bir ortak kullanımını gösterir `OneWayToSource` ve `TwoWay` modları bağlama. Dört `Slider` görünümleri denetimine yönelik `Scale`, `Rotate`, `RotateX`, ve `RotateY` özelliklerini bir `Label`. İlk olarak göründüğü gibi bu dört özelliklerini `Label` her ayarlandığından, veri bağlama hedefleri olmalıdır bir `Slider`. Ancak, `BindingContext` , `Label` yalnızca bir nesne olabilir ve dört farklı kaydırıcılar vardır.

Bu nedenle, tüm bağlamaları görünüşte geriye doğru ayarlanan yolları: `BindingContext` dört kaydırıcıları her biri ayarlanır `Label`, ve bağlamaları ayarlanır `Value` kaydırıcıları özellikleri. Kullanarak `OneWayToSource` ve `TwoWay` modları, bunlar `Value` özellikleri olan kaynak özellikleri ayarlayabilirsiniz `Scale`, `Rotate`, `RotateX`, ve `RotateY` özelliklerini `Label`:

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

Üç bağlamalarda `Slider` görünümleridir `OneWayToSource`seçeneğidir; yani, `Slider` değeri özelliğinde bir değişiklik neden olur, `BindingContext`, olduğu `Label` adlı `label`. Bu üç `Slider` görünümleri değişiklikleri neden `Rotate`, `RotateX`, ve `RotateY` özelliklerini `Label`.

Ancak, bağını `Scale` özelliği `TwoWay`. Bunun nedeni, `Scale` özellik 1 ile kullanarak varsayılan değerine sahip bir `TwoWay` nedenler bağlamayı `Slider` ilk 1 0 yerine ayarlanacak değer. Bu bağlama olsaydı `OneWayToSource`, `Scale` özelliği başlangıçta ayarlanması 0'dan `Slider` varsayılan değer. `Label` Görünür olmaz ve kullanıcı için karışıklığa neden.

 [![](data-binding-basics-images/slidertransforms.png "Geriye dönük bağlamaları")](data-binding-basics-images/slidertransforms-large.png#lightbox "geriye doğru bağlamaları")

 > [!NOTE]
 > [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Ayrıca sınıfında [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) ve [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) ölçeklendirme özellikleri `VisualElement` x ekseni ve y ekseni sırasıyla.

## <a name="bindings-and-collections"></a>Bağlamalar ve koleksiyonları

Hiçbir şey XAML ve veri bağlamaları şablonlu iyi gücünü gösterir `ListView`.

`ListView` tanımlayan bir `ItemsSource` türünün özelliği `IEnumerable`, ve bu koleksiyondaki öğeleri görüntüler. Bu öğeleri herhangi bir türde nesneler olabilir. Varsayılan olarak, `ListView` kullanan `ToString` her öğenin o öğe görüntülemek için yöntemi. Bazen yalnızca istediğiniz, budur ancak birçok durumda, `ToString` nesne yalnızca tam sınıf adını döndürür.

Ancak, öğeler `ListView` koleksiyonunu kullanarak istediğiniz herhangi bir şekilde görüntülenebilir bir *şablon*, türetilen bir sınıf içerir `Cell`. Şablon, her öğe için kopyalanan `ListView`, ve şablonu üzerinde ayarlanan veri bağlamaları için tek tek kopyaları aktarılır.

Çok sık kullanarak bu öğeleri için özel bir hücre oluşturmak isteyebilirsiniz `ViewCell` sınıfı. Bu kod biraz karmaşık bir işlemdir ancak XAML içinde çok basit olur.

İçinde XamlSamples proje adlı bir sınıf içerir `NamedColor`. Her `NamedColor` nesnesinin `Name` ve `FriendlyName` türünün özelliklerini `string`ve `Color` türünün özelliği `Color`. Ayrıca, `NamedColor` türü 141 statik salt okunur alanlara sahip `Color` Xamarin.Forms içinde tanımlanan renklere karşılık gelen `Color` sınıfı. Statik Oluşturucu oluşturur bir `IEnumerable<NamedColor>` içeren koleksiyon `NamedColor` bu statik alanlara karşılık gelen nesneleri ve genel, statik olarak atar `All` özelliği.

Statik ayarlama `NamedColor.All` özelliğini `ItemsSource` , bir `ListView` kolaydır kullanarak `x:Static` işaretleme uzantısı:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

Sonuçta elde edilen görüntü öğeleri gerçekten türü olduğunu kurar `XamlSamples.NamedColor`:

[![](data-binding-basics-images/listview1.png "Koleksiyona bağlama")](data-binding-basics-images/listview1-large.png#lightbox "koleksiyona bağlama")

Kadar bilgi değil ancak `ListView` kaydırılabilir ve seçilebilir.

Öğeleri için bir şablon tanımlamak için ayırmak isteyebilirsiniz `ItemTemplate` özelliği bir özellik öğesi olarak ve ayarlamak bir `DataTemplate`, ardından başvuran bir `ViewCell`. İçin `View` özelliği `ViewCell` her öğeyi görüntülemek için bir veya daha fazla görünüm yerleşimini tanımlayabilirsiniz. Basit bir örnek aşağıda verilmiştir:

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

`Label` Ayarlanır `View` özelliği `ViewCell`. ( `ViewCell.View` Etiketleri değil çünkü gerekli `View` özelliktir içerik özelliğinin `ViewCell`.) Bu işaretleme görüntüler `FriendlyName` her özellik `NamedColor` nesnesi:

[![](data-binding-basics-images/listview2.png "DataTemplate ile bir koleksiyon bağlama")](data-binding-basics-images/listview2-large.png#lightbox "bir koleksiyona bir DataTemplate ile bağlama")

Daha iyi. Artık gerekli olan tek şey ladin öğe şablonu ile daha fazla bilgi ve gerçek renk'kurmak için. Bu şablon desteklemek için bazı değerler ve nesneler sayfanın kaynak sözlüğünde tanımlanmıştır:

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

    <ListView ItemsSource="{x:Static local:NamedColor.All}"
              RowHeight="{StaticResource rowHeight}">
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

Kullanımına dikkat edin `OnPlatform` boyutunu tanımlamak için bir `BoxView` ve yüksekliğini `ListView` satır. Üç tüm platformlar için değerler aynı olsa da, biçimlendirme kolayca görüntü ince ayar yapmak, diğer değerler için uyarlanmış olabilir.

## <a name="binding-value-converters"></a>Bağlama değeri dönüştürücüleri

Önceki **ListView tanıtım** XAML dosyasında tek tek görüntülenir `R`, `G`, ve `B` Xamarin.Forms özelliklerini `Color` yapısı. Bu özellikler, türlerinin `double` ve 0 ile 1 arasında. Onaltılık değerleri görüntülemek istiyorsanız, yalnızca kullanamazsınız `StringFormat` "belirtimi biçimlendirme X2" ile. Yalnızca çalışan tamsayılar için ve yanı sıra, `double` değerleri 255 ile çarpılmasına gerekir.

İle bu küçük bir sorun Çözüldü bir *değer dönüştürücü*ayrıca adlı bir *bağlama dönüştürücü*. Uygulayan bir sınıf budur `IValueConverter` adlı iki yöntem vardır, yani arabirimi `Convert` ve `ConvertBack`. `Convert` Yöntemi çağrıldığında bir değer kaynaktan hedefe; aktarılırken `ConvertBack` yöntemi çağrıldığında aktarımları için hedeften kaynağa `OneWayToSource` veya `TwoWay` bağlamaları:

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

`ConvertBack` Yöntemi bağlamaları yalnızca kaynaktan hedefe bir yolu olduğundan bu programda bir rol oynamıyor.

Bağlama ile bağlama dönüştürücü başvuran `Converter` özelliği. Bir bağlama Dönüştürücü ayrıca belirtilen bir parametre kabul `ConverterParameter` özelliği. Bazı çok yönlülük için çarpan nasıl belirtildiğine budur. Dönüştürücü parametresi için geçerli bir bağlama dönüştürücü denetler `double` değeri.

Birden çok bağlamaları arasında paylaşılabilir böylece Dönüştürücüsü kaynak sözlüğünde örneği:

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

Üç veri bağlamaları tek Bu örnek başvuru. Dikkat `Binding` işaretleme uzantısı içeren katıştırılmış `StaticResource` işaretleme uzantısı:

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

Sonuç şu şekildedir:

[![](data-binding-basics-images/listview3.png "Bir koleksiyona bir DataTemplate ve dönüştürücüler bağlama")](data-binding-basics-images/listview3-large.png#lightbox "DataTemplate ve dönüştürücüler koleksiyonuna bağlama")

`ListView` Belirli adımları atmanız oluşabilecek dinamik olarak ancak yalnızca temel alınan verilerdeki değişiklikler işlenirken oldukça karmaşık olduğundan. Öğelerin koleksiyonu için atanmışsa `ItemsSource` özelliği `ListView` değişiklikleri çalışma zamanı sırasında — öğeleri eklenebilir, ise veya koleksiyondan kaldırılır — kullanmak bir `ObservableCollection` bu öğeler için sınıf. `ObservableCollection` uygulayan `INotifyCollectionChanged` arabirimi ve `ListView` için bir işleyici yükleyecek `CollectionChanged` olay.

Öğelerin özelliklerini değiştirmek çalışma zamanı sırasında sonra koleksiyondaki öğelerin uygulamalıdır `INotifyPropertyChanged` özellik değerlerini kullanarak arabirimi ve sinyal değişiklikleri `PropertyChanged` olay. Bu bu serinin sonraki bölümünde gösterilmektedir [5. bölüm. MVVM için bağlama verilerden](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Özet

Veri bağlamaları özellikleri sayfasında iki nesne arasındaki veya görsel nesneler arasında bağlama ve temel alınan veriler için güçlü bir mekanizma sağlar. Ancak, uygulama veri kaynaklarıyla çalışmaya başladığında, popüler uygulama mimari deseni yararlı bir standardı ortaya çıkmaya başladı başlar. Bu bölümünde ele alınmıştır [5. bölüm. Gelen veri bağlamaları-MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).



## <a name="related-links"></a>İlgili bağlantılar

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Bölüm 1. XAML (örnek) ile çalışmaya başlama](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Bölüm 2. Temel XAML sözdizimi (örnek)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Bölüm 3. XAML biçimlendirme uzantıları (örnek)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Bölüm 5. MVVM (örnek) için veri bağlama](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
