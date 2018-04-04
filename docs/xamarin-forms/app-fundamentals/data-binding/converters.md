---
title: Bağlama değer dönüştürücüler
description: Cast veya veri bağlama içindeki değerleri Dönüştür
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 260db2372977202df3d73e32645a358066146b40
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-value-converters"></a>Bağlama değer dönüştürücüler

Veri bağlamaları genellikle veri kaynağı özelliğinden hedef özelliğe ve hedef özelliğinden bazı durumlarda kaynak özelliğine aktarın. Bu aktarma, kaynak ve hedef özellikleri aynı türde olduğunda ya da diğer türüne örtük bir dönüştürme aracılığıyla bir türe dönüştürülüp basittir. Bu durumda olmadığı durumlarda, tür dönüştürme gerçekleşmesi gerekir.

İçinde [ **biçimlendirme dizesi** ](string-formatting.md) makalesi, nasıl kullanacağınızı gördüğünüz `StringFormat` herhangi bir tür bir dizeye dönüştürmek için bir veri bağlama özelliği. Diğer dönüştürme türleri için uygulayan bir sınıf bazı özel kod yazmanız gerekir. [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) arabirimi. (Evrensel Windows platformu adlı benzer bir sınıfa içeren [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) içinde `Windows.UI.Xaml.Data` ad alanı, ancak bu `IValueConverter` yer `Xamarin.Forms` ad.) Sınıfları uygulayan `IValueConverter` denir *değer dönüştürücüler*, ancak bunlar ayrıca olarak anılır *dönüştürücüler bağlama* veya *değer dönüştürücüler bağlama*.

## <a name="the-ivalueconverter-interface"></a>IValueConverter arabirimini

Source özelliği türü olduğu veri bağlama tanımlamak istediğinizi varsayalım `int` ancak hedef özelliği bir `bool`. Bu veri bağlama üretmek için istediğiniz bir `false` değeri tamsayı kaynağı 0'a eşit olduğunda ve `true` Aksi takdirde.  

Bu uygulayan bir sınıf ile yapabileceğiniz `IValueConverter` arabirimi:

```csharp
public class IntToBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value != 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? 1 : 0;
    }
}
```

Bu sınıfın örneğini ayarlamak [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Converter/) özelliği `Binding` sınıfı veya [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Converter/) özelliği `Binding` biçimlendirme uzantısı. Bu sınıf, veri bağlama parçası haline gelir.

`Convert` Yöntemi çağrılır veri kaynağından hedef taşır `OneWay` veya `TwoWay` bağlar. `value` Parametredir nesne veya veri bağlama kaynağından değeri. Yöntemi veri bağlama hedefinin türünde bir değer döndürmesi gerekir. Burada atamaları gösterilen yöntemi `value` parametresi için bir `int` ve 0 ile karşılaştıran bir `bool` dönüş değeri.

`ConvertBack` Veri kaynağına hedef taşındığında yöntemi çağrıldığında `TwoWay` veya `OneWayToSource` bağlar. `ConvertBack` Ters dönüştürme gerçekleştirir: bunu varsayar `value` parametresi bir `bool` hedeften ve kendisine dönüştüren bir `int` dönüş değeri kaynağı için.

Veri bağlama da içeriyorsa, bir `StringFormat` ayarı sonucu dize olarak biçimlendirilmeden önce değer dönüştürücüsü çağrılır.

**Etkinleştirmek düğmeleri** sayfasındaki [ **veri bağlama gösterileri** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) örnek veri bağlamada bu değer dönüştürücüsü kullanmayı gösterir. `IntToBoolConverter` Sayfanın kaynak sözlükte örneği. Ardından ile başvuruluyor bir `StaticResource` ayarlamak için işaretleme uzantısı `Converter` iki veri bağlamaları özelliği. Veri dönüştürücüler sayfasında birden çok veri bağlaması arasında paylaşmak için çok yaygındır:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.EnableButtonsPage"
             Title="Enable Buttons">
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:IntToBoolConverter x:Key="intToBool" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <Entry x:Name="entry1"
               Text=""
               Placeholder="enter search term"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Search"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry1},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />

        <Entry x:Name="entry2"
               Text=""
               Placeholder="enter destination"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Submit"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry2},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />
    </StackLayout>
</ContentPage>
```

Değer dönüştürücüsü, uygulamanızın birden çok sayfada kullandıysanız, bu kaynak sözlüğü örneğini oluşturabilirsiniz **App.xaml** dosya.

**Etkinleştirmek düğmeleri** sayfasını gösteren ortak bir gereksinim bir `Button` içine kullanıcı türleri metni temel alan bir işlem gerçekleştirir bir `Entry` görünümü. Hiçbir şey içine yazıldığını varsa `Entry`, `Button` devre dışı bırakılması gerekir. Her `Button` Veri bağlamada içeren kendi `IsEnabled` özelliği. Veri bağlama kaynağı `Length` özelliği `Text` ilgili özellik `Entry`. Bu, `Length` özelliği 0 ise, değer dönüştürücüsü döndürür `true` ve `Button` etkin:

[![Düğmeleri etkinleştirme](converters-images/enablebuttons-small.png "etkinleştirmek düğmeleri")](converters-images/enablebuttons-large.png#lightbox "etkinleştirmek düğmeleri")

Dikkat `Text` her bir özellik `Entry` boş bir dize olarak başlatılır. `Text` Özelliği `null` varsayılan ve veri bağlama bu durumda çalışmaz.

Başkalarının genelleştirilmiş karşın bazı değer dönüştürücüler özellikle belirli uygulamalar için yazılmıştır. Değer dönüştürücüsü içinde yalnızca kullanılacak biliyorsanız `OneWay` bağlamaları, sonra `ConvertBack` yöntemi yalnızca dönebilirsiniz `null`.

`Convert` Örtük olarak yukarıda gösterilen yöntemi varsayar `value` bağımsız değişken türü ise `int` ve dönüş değeri türü olmalıdır `bool`. Benzer şekilde, `ConvertBack` yöntemi varsayar `value` bağımsız değişken türü ise `bool` ve dönüş değeri `int`. Bu durumda değilse, bir çalışma zamanı özel durum meydana gelir.

Değer dönüştürücüler daha genelleştirilmiş ve birkaç farklı veri türlerini kabul edecek şekilde yazabilirsiniz. `Convert` Ve `ConvertBack` yöntemleri kullanabilir `as` veya `is` işleçlerle `value` parametresi ya da çağırabilirsiniz `GetType` kendi yazın ve ardından bir şey belirlemek için bu parametreyi uygun. Her yöntemin dönüş değeri beklenen türde tarafından verilen `targetType` parametresi. Bazen, değer dönüştürücüler farklı bir hedef türleri veri bağlamalarla kullanılır; değer dönüştürücüsü kullanabilirsiniz `targetType` için doğru türde bir dönüştürme gerçekleştirmek için bağımsız değişken.

Gerçekleştirilen dönüştürme için farklı kültürler farklıysa kullanın `culture` parametresi bu amaç için. `parameter` Bağımsız değişkeni `Convert` ve `ConvertBack` bu makalenin sonraki bölümlerinde ele alınmıştır.

## <a name="binding-converter-properties"></a>Bağlama dönüştürücü özellikleri

Değer dönüştürücüsü sınıfları, özellikleri ve genel parametreler olabilir. Bu belirli değer dönüştürücüsü dönüştüren bir `bool` türünde bir nesne kaynağından `T` hedefi için:

```csharp
public class BoolToObjectConverter<T> : IValueConverter
{
    public T TrueObject { set; get; }

    public T FalseObject { set; get; }

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? TrueObject : FalseObject;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ((T)value).Equals(TrueObject);
    }
}
```

**Anahtar göstergeleri** sayfasını gösteren nasıl değerini görüntülemek için kullanılabilmesi için bir `Switch` görünümü. Kaynak bir kaynak sözlüğü olarak değer dönüştürücüler örneği oluşturmak için ortak olsa da, bu sayfayı bir alternatif gösterir: her değer dönüştürücüsü arasında örneği `Binding.Converter` özellik öğesi etiketleri. `x:TypeArguments` Genel bağımsız değişken gösterir ve `TrueObject` ve `FalseObject` her ikisi de bu tür nesneleri için ayarlanır:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SwitchIndicatorsPage"
             Title="Switch Indicators">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>

            <Style TargetType="Switch">
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Subscribe?" />
            <Switch x:Name="switch1" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch1}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Of course!"
                                                         FalseObject="No way!" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Allow popups?" />
            <Switch x:Name="switch2" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Yes"
                                                         FalseObject="No" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
                <Label.TextColor>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Color"
                                                         TrueObject="Green"
                                                         FalseObject="Red" />
                        </Binding.Converter>
                    </Binding>
                </Label.TextColor>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Learn more?" />
            <Switch x:Name="switch3" />
            <Label FontSize="18"
                   VerticalOptions="Center">
                <Label.Style>
                    <Binding Source="{x:Reference switch3}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Style">
                                <local:BoolToObjectConverter.TrueObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Indubitably!" />
                                        <Setter Property="FontAttributes" Value="Italic, Bold" />
                                        <Setter Property="TextColor" Value="Green" />
                                    </Style>                                    
                                </local:BoolToObjectConverter.TrueObject>

                                <local:BoolToObjectConverter.FalseObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Maybe later" />
                                        <Setter Property="FontAttributes" Value="None" />
                                        <Setter Property="TextColor" Value="Red" />
                                    </Style>
                                </local:BoolToObjectConverter.FalseObject>
                            </local:BoolToObjectConverter>
                        </Binding.Converter>
                    </Binding>
                </Label.Style>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Üç son içinde `Switch` ve `Label` çiftlerine genel bağımsız değişken olarak ayarlanmış `Style`ve tüm `Style` nesneleri değerleri için sağlanan `TrueObject` ve `FalseObject`. Bunlar için örtük stili geçersiz kılma `Label` kaynak sözlükte ayarlanırsa, bu nedenle bu stili özelliklerinde açıkça atanır `Label`. Geçiş `Switch` karşılık gelen neden `Label` değişikliği yansıtacak şekilde:

[![Geçiş göstergeleri](converters-images/switchindicators-small.png "geçiş göstergeleri")](converters-images/switchindicators-large.png#lightbox "geçiş göstergeleri")

Kullanmak da mümkündür [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) diğer görünümleri tabanlı kullanıcı arabiriminde benzer değişiklikleri uygulamak için.

## <a name="binding-converter-parameters"></a>Dönüştürücü parametreleri bağlama

`Binding` Sınıfı tanımlayan bir [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.ConverterParameter/) özelliği ve `Binding` biçimlendirme uzantısı da tanımlayan bir [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.ConverterParameter/) özelliği. Bu özellik ayarlanırsa sonra için geçirilen değer `Convert` ve `ConvertBack` yöntemleri olarak `parameter` bağımsız değişkeni. Değer dönüştürücüsü örneğini birkaç veri bağlamaları arasında paylaşılan olsa bile `ConverterParameter` biraz farklı dönüşümlerini gerçekleştirmek için farklı olabilir.

Kullanımını `ConverterParameter` bir renk seçimi programla gösterilmiştir. Bu durumda, `RgbColorViewModel` türünün üç özelliklerine sahip `double` adlı `Red`, `Green`, ve `Blue` oluşturmak için kullandığı bir `Color` değeri:

```csharp
public class RgbColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Red
    {
        set
        {
            if (color.R != value)
            {
                Color = new Color(value, color.G, color.B);
            }
        }
        get
        {
            return color.R;
        }
    }

    public double Green
    {
        set
        {
            if (color.G != value)
            {
                Color = new Color(color.R, value, color.B);
            }
        }
        get
        {
            return color.G;
        }
    }

    public double Blue
    {
        set
        {
            if (color.B != value)
            {
                Color = new Color(color.R, color.G, value);
            }
        }
        get
        {
            return color.B;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Red"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Green"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Blue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

`Red`, `Green`, Ve `Blue` özellikleri aralık 0 ile 1 arasında. Ancak, bileşenlerin iki basamaklı onaltılık değerler olarak görüntülenmesini tercih edebilirsiniz.

Onaltılık değerler olarak XAML'de görüntülemek için bunlar gerekir tarafından 255 çarparak, bir tamsayıya dönüştürülüp ve "X2" belirtimiyle sonra biçimlendirilmiş `StringFormat` özelliği. İlk iki görevler (255 tarafından çarparak ve bir tamsayıya dönüştürmek) değeri dönüştürücü tarafından işlenebilir. Değer dönüştürücüsü mümkün olduğunca genelleştirilmiş yapmak için çarpma faktörü ile belirtilebilir `ConverterParameter` girdiğinden emin anlamına gelir özelliği `Convert` ve `ConvertBack` yöntemleri olarak `parameter` bağımsız değişkeni:

```csharp
public class DoubleToIntConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)Math.Round((double)value * GetParameter(parameter));
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value / GetParameter(parameter);
    }

    double GetParameter(object parameter)
    {
        if (parameter is double)
            return (double)parameter;

        else if (parameter is int)
            return (int)parameter;

        else if (parameter is string)
            return double.Parse((string)parameter);

        return 1;
    }
}
```

`Convert` Dönüştürür bir `double` için `int` tarafından çarparak sırasında `parameter` değeri; `ConvertBack` tamsayı böler `value` bağımsız değişkeni tarafından `parameter` ve döndüren bir `double` sonuç. (Aşağıda gösterilen programında değer dönüştürücüsü yalnızca dize biçimlendirme bağlantılı olarak, bu nedenle kullanılan `ConvertBack` kullanılmaz.)

Türü `parameter` bağımsız değişken veri bağlama kod veya XAML tanımlı bağlı olarak farklı olması muhtemeldir. Varsa `ConverterParameter` özelliği `Binding` ayarlanır kod içinde sayısal bir değere ayarlanmış olması olabilir:

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter` Özelliği türüdür `Object`, C# Derleyici değişmez değer 255 bir tamsayı olarak yorumlar ve bu değeri özelliğini ayarlar.

XAML'de ancak `ConverterParameter` şu şekilde ayarlanmış olması olabilir:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255 görülüyor number, ancak çünkü `ConverterParameter` türü `Object`, XAML ayrıştırıcısı 255 dize olarak değerlendirir.

Bu nedenle, yukarıda gösterilen değer dönüştürücüsü ayrı bir içeren `GetParameter` durumları için işleme yöntemi `parameter` türü olan `double`, `int`, veya `string`.  

**RGB Renk Seçici** sayfa başlatır `DoubleToIntConverter` iki örtük stil tanımını izleyen kaynak sözlüğünde:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.RgbColorSelectorPage"
             Title="RGB Color Selector">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <local:DoubleToIntConverter x:Key="doubleToInt" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:RgbColorViewModel Color="Gray" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Red}" />
            <Label Text="{Binding Red,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0:X2}'}" />

            <Slider Value="{Binding Green}" />
            <Label Text="{Binding Green,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0:X2}'}" />

            <Slider Value="{Binding Blue}" />
            <Label>
                <Label.Text>
                    <Binding Path="Blue"
                             StringFormat="Blue = {0:X2}"
                             Converter="{StaticResource doubleToInt}">
                        <Binding.ConverterParameter>
                            <x:Double>255</x:Double>
                        </Binding.ConverterParameter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Değerlerini `Red` ve `Green` özellikleri ile görüntülenen bir `Binding` biçimlendirme uzantısı. `Blue` Özelliği, ancak başlatır `Binding` açık bir nasıl göstermek için sınıf `double` değer ayarlanabilir `ConverterParameter` özelliği.

Sonuç şöyledir:

[![RGB Renk Seçici](converters-images/rgbcolorselector-small.png "RGB Renk Seçici")](converters-images/rgbcolorselector-large.png#lightbox "RGB Renk Seçici")


## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama gösterileri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
