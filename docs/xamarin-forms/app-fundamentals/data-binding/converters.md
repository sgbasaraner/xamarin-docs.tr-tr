---
title: Xamarin.Forms bağlama değeri dönüştürücüleri
description: Bu makalede, dönüştürme ya da bir Xamarin.Forms veri bağlama içindeki değerleri, (aynı olarak da bilinen bir bağlama dönüştürücü ya da bağlama değer dönüştürücü) bir değer dönüştürücü uygulayarak ısqlconnectionınfo'ya dönüştürmelidir. açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 28892692133020de1fa5a6eb007bb3f9bcf2612b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997493"
---
# <a name="xamarinforms-binding-value-converters"></a>Xamarin.Forms bağlama değeri dönüştürücüleri

Veri bağlamaları genellikle veri bir kaynak özelliği için bir hedef özelliği, hedef özelliğinden bazı durumlarda kaynak özelliğine aktarımı. Bu aktarma, kaynak ve hedef özelliklerin aynı türde olduğunda ya da diğer türüne örtük bir dönüştürme aracılığıyla bir türe dönüştürülüp oldukça basittir. Durum olmadığı durumlarda, tür dönüştürme gerçekleşmesi gerekir.

İçinde [ **biçimlendirme dizesi** ](string-formatting.md) makalesi, nasıl kullanabileceğinizi gördüğünüz `StringFormat` herhangi bir tür bir dizeye dönüştürmek için veri bağlama özelliği. Diğer tür dönüştürmeler için uygulayan bir sınıf içinde özel bir kod yazmanız gereken [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) arabirimi. (Evrensel Windows platformu adlı benzer bir sınıf içeren [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) içinde `Windows.UI.Xaml.Data` ancak bu ad alanı `IValueConverter` bulunduğu `Xamarin.Forms` ad.) Uygulayan sınıflar `IValueConverter` adlandırılır *değer dönüştürücüler*, ancak bunlar da olarak anılır *dönüştürücüler bağlama* veya *bağlama değeri dönüştürücüleri*.

## <a name="the-ivalueconverter-interface"></a>IValueConverter arabirimini

Source özelliği türü olduğu bir veri bağlamayı tanımlamak istediğiniz varsayalım `int` ancak hedef özelliği bir `bool`. Bu veri bağlamayı oluşturmak için istediğiniz bir `false` değeri 0'a eşit bir tamsayı kaynak olduğunda ve `true` Aksi takdirde.  

Uygulayan bir sınıf ile yapabilecekleriniz `IValueConverter` arabirimi:

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

İçin bu sınıfın bir örneği ayarladığınız [ `Converter` ](xref:Xamarin.Forms.Binding.Converter) özelliği `Binding` sınıfı veya [ `Converter` ](xref:Xamarin.Forms.Xaml.BindingExtension.Converter) özelliği `Binding` işaretleme uzantısı. Bu sınıf, veri bağlama bir parçası haline gelir.

`Convert` Kaynaktan hedef veri taşır, yöntemi çağrıldığında `OneWay` veya `TwoWay` bağlar. `value` Parametresi, nesne veya bağlama veri kaynağı değeri. Yöntemi, veri bağlama hedefi türü bir değer döndürmesi gerekir. Burada yayınlar gösterildiği yöntemi `value` parametresi bir `int` ve 0 ile karşılaştıran bir `bool` dönüş değeri.

`ConvertBack` Yöntemi, veri kaynağına hedeften getirdiğinde çağrılır `TwoWay` veya `OneWayToSource` bağlar. `ConvertBack` Ters dönüştürme gerçekleştirir: Bu varsayar `value` parametresi bir `bool` , hedef ve dönüştürür bir `int` kaynağı için bir değer döndürür.

Veri bağlamayı da içeriyorsa, bir `StringFormat` ayarını sonucu dize olarak biçimlendirilmeden önce değer dönüştürücü çağrılır.

**Etkinleştirme düğmeleri** sayfasını [ **veri bağlama tanıtımları** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) örnek bir veri bağlama bu değer dönüştürücüsü kullanmayı gösterir. `IntToBoolConverter` Sayfanın kaynak sözlüğünde oluşturulur. Ardından ile başvuruluyor bir `StaticResource` ayarlamak için işaretleme uzantısı `Converter` iki veri bağlamaları özelliği. Veri dönüştürücüler sayfasında birden fazla veri bağlamaları arasında paylaşmak çok yaygın bir uygulamadır:

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

Değer dönüştürücüsü, uygulamanızın birden çok sayfada kullandıysanız bunu kaynak sözlüğünde örneği oluşturabilir **App.xaml** dosya.

**Etkinleştirme düğmeleri** sayfasını gösterir gereken ortak bir `Button` içine kullanıcının yazdığı metni temel alan bir işlem gerçekleştirir bir `Entry` görünümü. Hiçbir şey içine yazıldığını varsa `Entry`, `Button` devre dışı bırakılması gerekir. Her `Button` Veri bağlamada içeren kendi `IsEnabled` özelliği. Veri bağlama kaynağı `Length` özelliği `Text` karşılık gelen özellik `Entry`. Bu, `Length` özelliği değer Dönüştürücü 0 döndürür `true` ve `Button` etkinleştirilir:

[![Düğmeler etkinleştir](converters-images/enablebuttons-small.png "etkinleştirme düğmeleri")](converters-images/enablebuttons-large.png#lightbox "düğmeleri etkinleştir")

Dikkat `Text` her bir özellik `Entry` boş dize olarak başlatılır. `Text` Özelliği `null` varsayılan ve veri bağlama bu durumda işe yaramaz.

Başkalarının genelleştirilmiş sırasında bazı değer dönüştürücüler özellikle belirli uygulamalar için yazılır. Değer dönüştürücüsü, yalnızca kullanılacak biliyorsanız `OneWay` bağlamaları, ardından `ConvertBack` yöntemi yalnızca döndürebilir `null`.

`Convert` Örtük olarak yukarıda gösterilen yöntemi varsayar `value` bağımsız değişken türü ise `int` ve dönüş değeri türünde olmalıdır `bool`. Benzer şekilde, `ConvertBack` yöntemi varsayar `value` bağımsız değişken türü ise `bool` ve dönüş değeri `int`. Durum bu değilse, bir çalışma zamanı özel durumu oluşur.

Değer dönüştürücüler daha genelleştirilmesini ve birçok farklı veri türlerini kabul edecek şekilde yazabilirsiniz. `Convert` Ve `ConvertBack` yöntemleri kullanabilir `as` veya `is` işleçlerle `value` parametresi veya çağırabilirsiniz `GetType` , yazın ve ardından bir şey belirlemek için bu parametreyi uygun. Her bir yöntemin dönüş değeri beklenen tür tarafından verilen `targetType` parametresi. Bazen, değer dönüştürücüler farklı bir hedef türleri veri bağlamaları ile kullanılır; değer dönüştürücüsü kullanabilirsiniz `targetType` için doğru türde bir dönüştürme gerçekleştirmek için bağımsız değişken.

Gerçekleştirilmekte olan dönüştürme için farklı kültürler farklı ise, kullanın `culture` bu amaç için parametre. `parameter` Bağımsız değişkeni `Convert` ve `ConvertBack` bu makalenin sonraki bölümlerinde ele alınmıştır.

## <a name="binding-converter-properties"></a>Dönüştürücü özellikleri bağlama

Değer dönüştürücüsü sınıfları, özellikleri ve genel parametreler olabilir. Bu özel değer dönüştürücü dönüştürür bir `bool` kaynak türünden bir nesne için `T` hedef için:

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

**Anahtar göstergeleri** sayfasını gösterir nasıl değerini görüntülemek için kullanılabilmesi için bir `Switch` görünümü. Bir kaynak sözlüğünde kaynağı olarak değer dönüştürücüler örneklemek için yaygın olsa da, bu sayfada alternatif gösterir: her değer dönüştürücü arasında örneği `Binding.Converter` özellik öğesi etiketleri. `x:TypeArguments` Genel bağımsız değişken belirtir ve `TrueObject` ve `FalseObject` hem de o türdeki nesneleri ayarlanır:

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

En son üç `Switch` ve `Label` çiftleri genel değişkeni ayarlanır `Style`ve tüm `Style` nesneleri değerleri için sağlanan `TrueObject` ve `FalseObject`. Bunlar için örtük stili geçersiz kılmak `Label` kaynak sözlüğünde ayarlanırsa, bu nedenle bu stil özellikleri açıkça atanmış `Label`. Geçiş `Switch` karşılık gelen neden `Label` değişimi yansıtmak için:

[![Geçiş göstergeleri](converters-images/switchindicators-small.png "geçiş göstergeleri")](converters-images/switchindicators-large.png#lightbox "geçiş göstergeleri")

Kullanmak da mümkündür [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) diğer görünümleri tabanlı kullanıcı arabiriminde benzer değişiklikleri uygulamak için.

## <a name="binding-converter-parameters"></a>Dönüştürücü parametreleri bağlama

`Binding` Sınıfı tanımlayan bir [ `ConverterParameter` ](xref:Xamarin.Forms.Binding.ConverterParameter) özelliği ve `Binding` işaretleme uzantısı da tanımlar bir [ `ConverterParameter` ](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter) özelliği. Bu özelliğin ayarlandığı sonra noktasına geçirilen değer `Convert` ve `ConvertBack` yöntemleri olarak `parameter` bağımsız değişken. Değer dönüştürücüsü örneğini birkaç veri bağlamaları arasında paylaşılan bile `ConverterParameter` biraz farklı dönüştürmeler gerçekleştirmek için farklı olabilir.

Kullanımını `ConverterParameter` bir renk seçimi programla gösterilmiştir. Bu durumda, `RgbColorViewModel` türünde üç özelliğe sahiptir `double` adlı `Red`, `Green`, ve `Blue` oluşturmak için kullandığı bir `Color` değeri:

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

`Red`, `Green`, Ve `Blue` özellikleri aralık 0 ile 1 arasında. Ancak, bileşenlerin iki basamaklı bir onaltılık değer olarak gösterilecek tercih edebilirsiniz.

Onaltılık değerler olarak XAML içinde görüntülemek için bunlar gerekir 255 ile çarpılan bir tamsayıya dönüştürülüp ve bir "X2" belirtimi ile ardından biçimlendirilmiş `StringFormat` özelliği. İlk iki görev (255 tarafından çarparak ve bir tamsayıya dönüştürme) değer dönüştürücüsü tarafından işlenebilir. Mümkün olduğunca genelleştirilmiş olarak değer dönüştürücü sağlamak için çarpma faktör ile belirtilebilir `ConverterParameter` girer, yani özellik `Convert` ve `ConvertBack` yöntemleri olarak `parameter` bağımsız değişkeni:

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

`Convert` Dönüştürür bir `double` için `int` tarafından çarpılmasıyla elde ederken `parameter` değeri; `ConvertBack` tamsayı böler `value` bağımsız değişkeni tarafından `parameter` ve döndüren bir `double` sonucu. (Aşağıda gösterilen programda değer dönüştürücü yalnızca biçimlendirme dizesi bağlantılı olarak bu nedenle kullanılan `ConvertBack` kullanılmaz.)

Türünü `parameter` bağımsız değişken veri bağlama kod ya da XAML tanımlanan bağlı olarak farklı olabilir. Varsa `ConverterParameter` özelliği `Binding` ayarlanmış kod içinde sayısal bir değere ayarlanmış olması olasılığı:

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter` Özelliği türüdür `Object`, C# Derleyici değişmez değer 255 tamsayı olarak yorumlar ve özelliği bu değere ayarlar.

Ancak, XAML içinde `ConverterParameter` büyük olasılıkla şu şekilde ayarlayın:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

Çünkü gibi bir numara, ancak 255 görünür `ConverterParameter` türünde `Object`, XAML ayrıştırıcı 255 bir dize olarak değerlendirir.

Bu nedenle, yukarıda gösterilen değer dönüştürücü ayrı bir içerir `GetParameter` durumlarda işleyen yöntem `parameter` türünden `double`, `int`, veya `string`.  

**RGB Renk Seçici** sayfa başlatır `DoubleToIntConverter` iki örtük stilleri tanımını aşağıdaki kaynak sözlüğünde:

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

Değerlerini `Red` ve `Green` özellikleri ile görüntülenir bir `Binding` işaretleme uzantısı. `Blue` Özelliği, ancak başlatır `Binding` açık nasıl göstermek için sınıf `double` değeri ayarlanabilir `ConverterParameter` özelliği.

Sonuç şu şekildedir:

[![RGB Renk Seçici](converters-images/rgbcolorselector-small.png "RGB Renk Seçici")](converters-images/rgbcolorselector-large.png#lightbox "RGB Renk Seçici")


## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölümden Xamarin.Forms kitabı](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
