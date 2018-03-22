---
title: "Kaydırıcı kullanma"
description: "Bir kaydırıcının sürekli değerleri arasında bir aralık seçmek için kullanın."
ms.topic: article
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/16/2018
ms.openlocfilehash: ce5d8c46282d3831edcf58af63b66d1f6292f75b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="using-slider"></a>Kaydırıcı kullanma

_Bir kaydırıcının sürekli değerleri arasında bir aralık seçmek için kullanın._

Xamarin.Forms [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) seçmek için kullanıcı tarafından yönetilebilir yatay bir çubuğu bir `double` sürekli bir aralıktan değeri. 

`Slider` Türünün üç özelliklerini tanımlayan `double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) Varsayılan değer 0 ile aralığının en düşük gereksinimdir.
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) Varsayılan değer olan 1 ile aralığının en fazla olabilir.
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) arasında değişebilir kaydırıcının değer `Minimum` ve `Maximum` ve varsayılan değer 0 olarak kullanılır.

Tüm üç özellik desteğiyle `BindableProperty` nesneleri. `Value` Özelliğine sahip bir varsayılan bağlama modu `BindingMode.TwoWay`, yani bir bağlama kaynağı kullanan bir uygulama olarak uygun olduğunu [Model View ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) mimarisi. 

> [!WARNING]
> Dahili olarak, `Slider` sağlar `Minimum` olan değerinden `Maximum`. Varsa `Minimum` veya `Maximum` hiç ayarlanır böylece `Minimum` olan küçük değildir `Maximum`, özel durum oluşturuldu. Bkz: [ **önlemleri** ](#precautions) ayarı hakkında daha fazla bilgi için bölüm aşağıda `Minimum` ve `Maximum` özellikleri.

`Slider` Olacak şekilde zorlar `Value` onun arasında olacak şekilde özelliği `Minimum` ve `Maximum`(dahil). Varsa `Minimum` özelliği ayarlanmış bir değere büyük `Value` özelliği, `Slider` ayarlar `Value` özelliğine `Minimum`. Benzer şekilde, varsa `Maximum` bir değere ayarlanmış değerinden `Value`, ardından `Slider` ayarlar `Value` özelliğine `Maximum`. 

`Slider` tanımlayan bir [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) durumlarda tetiklenir olay `Value` değişiklikler, kullanıcı işlenmesini yoluyla `Slider` veya programın ne zaman ayarlar `Value` doğrudan özelliği. A `ValueChanged` olay ayrıca şu ne zaman `Value` özelliği önceki paragrafta açıklanan yüklenen. 

[ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) Eşlik nesne `ValueChanged` olay sahip iki özellik türü her ikisi de `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/) ve [ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). Aynı anda olay şu, değeri `NewValue` aynı `Value` özelliği `Slider` nesnesi.

> [!WARNING]
> Kısıtlanmamış yatay düzen seçeneklerini kullanmayın `Center`, `Start`, veya `End` ile `Slider`. Android ve UWP `Slider` sıfır uzunluğunda ve iOS, çubuğu çubuğuna daraltır çok kısa. Varsayılan tutmak `HorizontalOptions` ayarıyla `Fill`ve genişliği kullanmayan `Auto` koyma zaman `Slider` içinde bir `Grid` düzeni.

## <a name="basic-slider-code-and-markup"></a>Temel kaydırıcı kod ve biçimlendirme

[ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) örnek işlevsel olarak aynıdır, ancak farklı şekillerde uygulanan üç sayfaları şununla başlar. Yalnızca C# kod ilk sayfa kullanır, ikinci kodundaki olay işleyici ile XAML kullanır ve üçüncü XAML dosyası içinde veri bağlama kullanarak olay işleyicisi kaçınabilirsiniz.

### <a name="creating-a-slider-in-code"></a>Kodda bir kaydırıcı oluşturma

**Temel kaydırıcı kod** sayfasındaki [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) örnek oluşturmak için Göster gösterir bir `Slider` ve iki `Label` koddaki nesneler:

```csharp
public class BasicSliderCodePage : ContentPage
{
    public BasicSliderCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Slider slider = new Slider
        {
            Maximum = 360
        };
        slider.ValueChanged += (sender, args) =>
        {
            rotationLabel.Rotation = slider.Value;
            displayLabel.Text = String.Format("The Slider value is {0}", args.NewValue);
        };

        Title = "Basic Slider Code";
        Padding = new Thickness(10, 0);
        Content = new StackLayout
        {
            Children =
            {
                rotationLabel,
                slider,
                displayLabel
            }
        };
    }
}
```

`Slider` Sağlamak için başlatılan bir `Maximum` 360 özelliği. `ValueChanged` İşleyicisi `Slider` kullanır `Value` özelliği `slider` ayarlamak için nesne `Rotation` özelliği ilk `Label` ve kullandığı `String.Format` yöntemiyle `NewValue` özelliği ayarlamak için olay bağımsız `Text` ikinci özelliği `Label`. Geçerli değeri elde etmek için bu iki yaklaşım `Slider` birbirinin yerine kullanılabilir. 

İOS, Android ve evrensel Windows Platformu (UWP) aygıtları çalıştıran program şöyledir:

[![Temel kaydırıcı kod](slider-images/BasicSliderCode.png "temel kaydırıcı kodu")](slider-images/BasicSliderCode-Large.png#lightbox)

İkinci `Label` kadar "(başlatılmadı)" metnini görüntüler `Slider` yönetilebilir, ilk durumlarda `ValueChanged` olay harekete. Görüntülenen ondalık basamak sayısını üç platformları için farklı olduğuna dikkat edin. Bu farklılıklar, platform uygulamalarını ilişkili `Slider` ve bölümünde bu makalenin sonraki bölümlerinde açıklanan [Platform uygulama farklılıkları](#implementations).

### <a name="creating-a-slider-in-xaml"></a>XAML'de kaydırıcıyı oluşturma

**Temel kaydırıcı XAML** sayfasıdır işlevsel olarak aynı **temel kaydırıcı kod** çoğunlukla XAML'de ancak uygulanan:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderXamlPage"
             Title="Basic Slider XAML"
             Padding="10, 0">
    <StackLayout>
        <Label x:Name="rotatingLabel" 
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider Maximum="360"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Arka plan kodu dosya işleyicisi içeren `ValueChanged` olay:

```csharp
public partial class BasicSliderXamlPage : ContentPage
{
    public BasicSliderXamlPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double value = args.NewValue;
        rotatingLabel.Rotation = value;
        displayLabel.Text = String.Format("The Slider value is {0}", value);
    }
}
```

Da edinilir için olay işleyicisini daha mümkündür `Slider` üzerinden olay tetikleme `sender` bağımsız değişkeni. `Value` Özelliği geçerli değeri içerir:

```csharp
double value = ((Slider)sender).Value;
```

Varsa `Slider` nesnesi ile XAML dosyasında bir adı verilen bir `x:Name` özniteliği (örneğin, "kaydırıcı"), sonra olay işleyicisi başvuru söz konusu nesne doğrudan:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Veri kaydırıcıyı bağlama

**Temel kaydırıcı bağlamaları** sayfası nasıl ortadan neredeyse eşdeğer bir program yazılacağını gösterir `Value` kullanarak olay işleyicisi [veri bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderBindingsPage"
             Title="Basic Slider Bindings"
             Padding="10, 0">
    <StackLayout>
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference slider}, 
                                  Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360" />

        <Label x:Name="displayLabel"
               Text="{Binding Source={x:Reference slider}, 
                              Path=Value, 
                              StringFormat='The Slider value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Rotation` Özelliği ilk `Label` bağlı `Value` özelliği `Slider`, olduğu gibi `Text` ikinci özelliğinin `Label` ile bir `StringFormat` belirtimi. **Temel kaydırıcı bağlamaları** sayfa işlevlerini biraz farklı iki önceki sayfaları: sayfa ilk görüntülendiğinde, ikinci `Label` değerle metin dizesini görüntüler. Veri bağlama kullanmanın bir avantajı budur. Veri bağlama olmadan metni görüntülemek için özellikle başlatmak gerekir `Text` özelliği `Label` veya, bir tetikleme benzetimi `ValueChanged` sınıf oluşturucudan olay işleyicisi çağırarak olay.

<a name="precautions" />

## <a name="precautions"></a>Önlemler

Değeri `Minimum` özelliği her zaman olmalıdır değerinden `Maximum` özelliği. Aşağıdaki kod parçacığını nedenler `Slider` bir özel durum yükseltmek için:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

C# Derleyici dizisinde, bu iki özellik ayarlar kodunu oluşturur ve ne zaman `Minimum` özelliği, 10 olarak ayarlanmışsa, varsayılandan daha büyük olan `Maximum` 1 değeri. Ayarlayarak, bu durumda bir özel durum önleyebilirsiniz `Maximum` özelliği ilk:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Ayarı `Maximum` varsayılandan daha büyük olduğu için 20'ye bir sorun teşkil etmez `Minimum` 0 ayarlanması. Zaman `Minimum` , değer ayarlanmadı küçük `Maximum` 20 değeri.

XAML'de aynı sorunu var. Özellikler sağlar bir sırayla ayarlanmalıdır `Maximum` her zaman değerinden daha büyük `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Ayarlayabileceğiniz `Minimum` ve `Maximum` değerleri negatif sayılar, ancak yalnızca bir sırayla burada `Minimum` olduğu her zaman değerinden `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value` Özelliği her zaman sıfırdan büyük veya eşit `Minimum` değeri ve küçük veya eşit `Maximum`. Varsa `Value` ayarlanmış bu aralığın dışında bir değer için değer aralık içinde olacak şekilde yüklenen, ancak hiçbir özel durum oluşturuldu. Örneğin, bu kod size *değil* bir özel durum:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Bunun yerine, `Value` için özellik yüklenen `Maximum` 1 değeri.

Yukarıda gösterilen bir kod parçacığı aşağıda verilmiştir: 

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Zaman `Minimum` 10'a, sonra ayarlanır `Value` da 10 olarak ayarlandı. 

Varsa bir `ValueChanged` olay işleyicisi, aynı anda bağlı, `Value` özelliği yüklenen varsayılan değerini 0 dışında bir şey için sonra bir `ValueChanged` olay tetiklenir. XAML parçacığı aşağıda verilmiştir: 

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Zaman `Minimum` 10'a ayarlanır `Value` de 10'a ayarlayın ve `ValueChanged` olay tetiklenir. Bu sayfaya geri kalanı oluşturulur ve henüz oluşturulmamış diğer öğeleri sayfada başvurmak işleyici deneyebilir önce ortaya çıkabilir. Bazı kodlar eklemek isteyebilirsiniz `ValueChanged` denetler işleyici `null` sayfadaki diğer öğelerin değerleri. Veya ayarlayabilirsiniz `ValueChanged` sonra olay işleyicisi `Slider` değerleri başlatıldı.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Platform uygulama farklılıkları

Daha önce gösterilen ekran görüntüleri görüntüleme `Slider` ondalık basamak, farklı bir değere sahip. Bu nasıl ilişkili olduğu `Slider` Android ve UWP platformlar üzerinde uygulanır.

### <a name="the-android-implementation"></a>Android uygulaması 

Android uygulaması `Slider` Android tabanlı [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) ve her zaman ayarlar [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) 1000 özelliğine. Bunun anlamı `Slider` Android yalnızca 1,001 ayrık değerler içeriyor. Ayarlarsanız `Slider` sağlamak için bir `Minimum` 0 ve `Maximum` 5000, sonra da olarak `Slider` yönetilebilir, `Value` özelliğinin değerleri 0, 5, 10, 15 ve benzeri. 

### <a name="the-uwp-implementation"></a>UWP uygulaması

UWP uygulaması `Slider` UWP üzerinde temel [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) denetim. `StepFrequency` UWP özelliğinin `Slider` fark için ayarlanmış `Maximum` ve `Minimum` özellikleri bölünmüş 10, ancak 1'den büyük. 

Örneğin, varsayılan aralığı 0'dan 1, `StepFrequency` özelliği 0,1 için ayarlanır. Olarak `Slider` yönetilebilir, `Value` özelliği 0, 0.1, 0.2, 0.3, 0.4, 0,5, 0,6, 0,7, 0,8, 0.9 ve 1.0 için kısıtlanmış. (Bu son sayfasında açıktır [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) örnek.) Zaman arasındaki farkı `Maximum` ve `Minimum` özellikleri 10 ya da daha sonra `StepFrequency` 1 olarak ayarlayın ve `Value` tam sayı değerleri özelliğine sahiptir. 

### <a name="the-stepslider-solution"></a>StepSlider çözümü

Daha ayrıntılı `StepSlider` içinde ele alınmıştır [Bölüm 27. Özel oluşturucu](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) defterinin *Xamarin.Forms ile Mobile Apps oluşturma*. `StepSlider` Benzer `Slider` ancak ekler bir `Steps` arasındaki değerleri sayısını belirtmek için özellik `Minimum` ve `Maximum`.

## <a name="sliders-for-color-selection"></a>Renk seçimi için kaydırıcılar

Son iki içinde sayfa [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) örnek her ikisini de kullanmanız üç `Slider` renk seçimi için örnek. Veri bağlama ile ViewModel kullanma gösterirken, sihirbazın ikinci sayfasında ilk sayfa arka plan kod dosyasına tüm etkileşimleri işler. 

### <a name="handling-sliders-in-the-code-behind-file"></a>Arka plan kod dosyasına işleme kaydırıcılar 

**RGB renk kaydırıcılar** sayfa başlatır bir `BoxView` bir renk görüntülemek için üç `Slider` rengi ve üç kırmızı, yeşil ve mavi bileşenleri seçmek için örnekleri `Label` bu renk görüntüleme için öğeleri değerler:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.RgbColorSlidersPage"
             Title="RGB Color Sliders">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="Maximum" Value="255" />
            </Style>
            
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView x:Name="boxView"
                 Color="Black"
                 VerticalOptions="FillAndExpand" />

        <Slider x:Name="redSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="redLabel" />

        <Slider x:Name="greenSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="greenLabel" />

        <Slider x:Name="blueSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="blueLabel" />
    </StackLayout>
</ContentPage>
```

A `Style` üçü verir `Slider` 0-255 dizi bir öğe. `Slider` Öğeleri aynı paylaşmak `ValueChanged` arka plan kod dosyasına uygulanan işleyici:

```csharp
public partial class RgbColorSlidersPage : ContentPage
{
    public RgbColorSlidersPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (sender == redSlider)
        {
            redLabel.Text = String.Format("Red = {0:X2}", (int)args.NewValue);
        }
        else if (sender == greenSlider)
        {
            greenLabel.Text = String.Format("Green = {0:X2}", (int)args.NewValue);
        }
        else if (sender == blueSlider)
        {
            blueLabel.Text = String.Format("Blue = {0:X2}", (int)args.NewValue);
        }

        boxView.Color = Color.FromRgb((int)redSlider.Value,
                                      (int)greenSlider.Value,
                                      (int)blueSlider.Value);
    }
}
```

İlk bölüm kümeleri `Text` birinin özelliği `Label` değerini belirten kısa bir metin dizesi örneklerine `Slider` onaltılık. Ardından, tüm üç `Slider` örnekleri oluşturmak için erişilebilir bir `Color` RGB bileşenlerinden değeri:

[![RGB renk kaydırıcılar](slider-images/RgbColorSliders.png "RGB renk kaydırıcılar")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Kaydırıcı bir ViewModel bağlama

**HSL renk kaydırıcılar** sayfası bir ViewModel oluşturmak için kullanılan hesaplamalar için nasıl kullanılacağını gösterir bir `Color` ton, Doygunluk ve parlaklığını değerlerinden değeri. Gibi tüm ViewModels `HSLColorViewModel` uygulayan sınıf `INotifyPropertyChanged` arabirimi ve ateşlenir bir `PropertyChanged` özelliklerinden biri değiştiğinde olay:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get 
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));
            }
        }
        get
        {
            return color;
        }
    }
}
```

ViewModels ve `INotifyPropertyChanged` arabirimi makalesinde açıklanan [veri bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**HslColorSlidersPage.xaml** dosya başlatır `HslColorViewModel` ve sayfanın ayarlar `BindingContext` özelliği. Bu ViewModel özelliklerinde bağlamak için XAML dosyasındaki tüm öğeleri sağlar:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SliderDemos"
             x:Class="SliderDemos.HslColorSlidersPage"
             Title="HSL Color Sliders">

    <ContentPage.BindingContext>
        <local:HslColorViewModel Color="Chocolate" />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</ContentPage> 
```

Olarak `Slider` öğeleri yönetilebilir, `BoxView` ve `Label` öğeleri ViewModel güncelleştirildi:

[![HSL renk kaydırıcılar](slider-images/HslColorSliders.png "HSL renk kaydırıcılar")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat` Bileşenini `Binding` biçimlendirme uzantısı iki ondalık basamak görüntülemek için bir "F2" biçimi için ayarlanır. (Veri bağlamaları biçimlendirme dizesi makalesinde açıklanan [biçimlendirme dizesi](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Ancak, program UWP sürümü 0, 0.1, 0.2 değerleri sınırlıdır... 0.9 ve 1.0. Bu bir UWP uygulaması doğrudan sonucudur `Slider` yukarıdaki bölümde açıklandığı gibi [Platform uygulama farklılıkları](#implementations).

## <a name="related-links"></a>İlgili bağlantılar

- [Kaydırıcı gösterileri örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Kaydırıcı API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
