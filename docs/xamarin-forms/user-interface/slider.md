---
title: Xamarin.Forms kaydırıcı
description: Xamarin.Forms kaydırıcıyı sürekli aralığından bir çift değer seçmek için kullanıcı tarafından işlenebilir yatay bir çubuk olan. Bu makalede, sürekli değer aralığını bir değer seçmek için kaydırıcı sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/10/2018
ms.openlocfilehash: 0069e59c1c09e242a74573ae66c8efade7d7f2a5
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38998252"
---
# <a name="xamarinforms-slider"></a>Xamarin.Forms kaydırıcı

_Bir aralıktaki sürekli değerleri seçmek için kaydırıcıyı kullanın._

Xamarin.Forms [ `Slider` ](xref:Xamarin.Forms.Slider) seçmek için kullanıcı tarafından işlenebilir yatay bir çubuk olan bir `double` sürekli bir aralıktan değeri.

`Slider` Üç tür özelliklerini tanımlar `double`:

- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) Varsayılan değer olan 0 ile aralığının en düşük gereksinimdir.
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) en büyük aralık, bu varsayılan değeri 1 ' dir.
- [`Value`](xref:Xamarin.Forms.Slider.Value) Kaydırıcının değeri, arasında değişebilir `Minimum` ve `Maximum` ve 0 varsayılan değerine sahiptir.

Tarafından desteklenen tüm üç özellik `BindableProperty` nesneleri. `Value` Özelliğine sahip bir varsayılan bağlama modu `BindingMode.TwoWay`, yani bir bağlama kaynağı kullanan bir uygulama olarak uygun olduğunu [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) mimarisi.

> [!WARNING]
> Dahili olarak `Slider` sağlar `Minimum` olduğu küçüktür `Maximum`. Varsa `Minimum` veya `Maximum` hiç olmadığı kadar ayarlanır böylece `Minimum` olan değerinden küçük olamaz `Maximum`, bir özel durum oluşturulur. Bkz: [ **önlemler** ](#precautions) bölümünde aşağıdaki ayarı hakkında daha fazla bilgi için `Minimum` ve `Maximum` özellikleri.

`Slider` Olacak şekilde zorlar `Value` arasında olduğundan özelliğini `Minimum` ve `Maximum`(dahil). Varsa `Minimum` özelliği ayarlanmış bir değer için büyük `Value` özelliği `Slider` ayarlar `Value` özelliğini `Minimum`. Benzer şekilde, varsa `Maximum` bir değere ayarlanmış küçüktür `Value`, ardından `Slider` ayarlar `Value` özelliğini `Maximum`.

`Slider` tanımlayan bir [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged) olduğunda harekete geçirilen olay `Value` değişiklikler, kullanıcı işlenmesini aracılığıyla `Slider` veya programın ne zaman ayarlar `Value` özelliği doğrudan. A `ValueChanged` da harekete ne zaman `Value` özelliği'nın durumunda bırakılması önceki paragrafta açıklandığı gibi.

[ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs) Eşlik eden bir nesne `ValueChanged` olay sahip iki özellik türü her ikisi de `double`: [ `OldValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) ve [ `NewValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue). Zaman olay tetiklenir, değerini `NewValue` aynı `Value` özelliği `Slider` nesne.

> [!WARNING]
> Sınırlandırılmamış yatay düzen seçeneklerini kullanmayın `Center`, `Start`, veya `End` ile `Slider`. Android ve UWP `Slider` daraltır sıfır uzunluklu ve ios'ta çubuğu çubuğuna çok kısa. Varsayılan tutun `HorizontalOptions` ayarıyla `Fill`ve genişliği kullanmayın `Auto` koyarak olduğunda `Slider` içinde bir `Grid` düzeni.

`Slider` Da görünümünü etkileyen çeşitli özellikleri tanımlar:

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty) çubuğu renk sol tarafında kalan kısmıdır.
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty) çubuğu renk sağ tarafında kalan kısmıdır.
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty) thumb renktir.
- [`ThumbImage`](xref:Xamarin.Forms.Slider.ThumbImageProperty) görüntü türü küçük resim için kullanılacak [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource).

> [!NOTE]
> `ThumbColor` Ve `ThumbImage` özelliklerini karşılıklı olarak birbirini dışlar. Her iki özellik ayarlandıysa `ThumbImage` özelliği öncelik alır.

## <a name="basic-slider-code-and-markup"></a>Temel kaydırıcı kod ve biçimlendirme

[ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) işlevsel olarak aynıdır, ancak farklı şekillerde uygulanan üç sayfalarıyla örnek başlar. İlk sayfa yalnızca C# kodu kullanır, ikinci olay işleyicisinde kodu ile XAML kullanan ve üçüncü olay işleyicisi XAML dosyasında veri bağlama kullanarak önlemek kullanabilirsiniz.

### <a name="creating-a-slider-in-code"></a>Kodda bir kaydırıcı oluşturma

**Temel kaydırıcı kod** sayfasını [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) örnek oluşturmak için Göster gösterir bir `Slider` ve iki `Label` koddaki nesneler:

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

`Slider` İçin başlatılan bir `Maximum` 360 özelliğidir. `ValueChanged` İşleyicisi `Slider` kullanır `Value` özelliği `slider` nesne ayarlayın `Rotation` özelliği ilk `Label` ve kullandığı `String.Format` yöntemiyle `NewValue` özelliği ayarlamak için olay bağımsız değişkenleri `Text` ikinci özelliği `Label`. Geçerli değerini almak için bu iki yaklaşımı `Slider` birbirinin yerine kullanılabilir.

Cihazlar iOS, Android ve evrensel Windows Platformu (UWP) üzerinde çalışan bir program şöyledir:

[![Temel kaydırıcı kod](slider-images/BasicSliderCode.png "temel kaydırıcı kod")](slider-images/BasicSliderCode-Large.png#lightbox)

İkinci `Label` kadar "(başlatılmamış)" metni görüntüler `Slider` yönetilebilir, ilk neden `ValueChanged` olay başlatılmasına izin. Görüntülenecek ondalık basamak sayısı üç platformları için farklı olduğuna dikkat edin. Bu farklılıklar, platform uygulamaları için ilişkili `Slider` ve bölümünde bu makalenin sonraki bölümlerinde ele alınmıştır [Platform uygulaması farklılıklarını](#implementations).

### <a name="creating-a-slider-in-xaml"></a>Bir kaydırıcı XAML içinde oluşturma

**Temel kaydırıcı XAML** sayfa, işlevsel olarak aynı **temel kaydırıcı kod** çoğunlukla XAML içinde uygulanan ancak:

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

Arka plan kod dosyası işleyicisini içeren `ValueChanged` olay:

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

Olay işleyicisi edinme olanağı da sağlar `Slider` aracılığıyla olay tetikleme `sender` bağımsız değişken. `Value` Özelliğini içeren geçerli değer:

```csharp
double value = ((Slider)sender).Value;
```

Varsa `Slider` nesnesi bir ad ile XAML dosyasında verilen bir `x:Name` özniteliği (örneğin, "kaydırıcı") ve ardından olay işleyicisi başvuru söz konusu nesne doğrudan:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Veri bağlama kaydırıcı

**Temel kaydırıcı bağlamaları** sayfası gösterir, ortadan kaldıran neredeyse eşdeğer bir program yazma `Value` kullanarak olay işleyicisi [veri bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md):

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

`Rotation` Özelliği ilk `Label` bağlı `Value` özelliği `Slider`, olduğu gibi `Text` ikinci özelliği `Label` ile bir `StringFormat` belirtimi. **Temel kaydırıcı bağlamaları** sayfa işlevlerini biraz farklı iki önceki sayfalarından: sayfa ilk kez görüntülendiğinde, ikinci `Label` değerle metin dizesini görüntüler. Veri bağlama kullanmanın bir avantajı budur. Veri bağlama olmadan metni görüntülemek için özellikle başlatmak gerekecektir `Text` özelliği `Label` veya bir Açmadığınızda, benzetimi `ValueChanged` sınıf oluşturucusunda olay işleyicisini çağırarak olay.

<a name="precautions" />

## <a name="precautions"></a>Uyarıları

Değerini `Minimum` özelliği her zaman küçük olmalıdır değerini `Maximum` özelliği. Aşağıdaki kod parçacığı nedenleri `Slider` bir özel durum:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

C# Derleyici dizisinde, bu iki özellik ayarlayan kodu oluşturur ve ne zaman `Minimum` özelliği 10 olarak ayarlanmışsa, varsayılandan daha büyükse `Maximum` 1 değeri. Ayarlayarak, bu durumda bir özel durumu önleyebilirsiniz `Maximum` özelliği ilk:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Ayarı `Maximum` varsayılandan daha büyük olduğu için 20'ye bir sorun değildir `Minimum` 0 değeri. Zaman `Minimum` , değer ayarlanmış küçüktür `Maximum` değeri 20.

XAML içinde aynı sorunu var. Özellikler, sağlayan bir sırayı ayarlamak `Maximum` her zaman büyük olup `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Ayarlayabileceğiniz `Minimum` ve `Maximum` değerleri negatif sayılar, ancak yalnızca bir sırayla burada `Minimum` olduğundan her zaman küçüktür `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value` Özelliği her zaman büyük veya eşittir `Minimum` değeri ve daha az veya buna eşit `Maximum`. Varsa `Value` ayarlanır bu aralığın dışında bir değere değer aralığında olacak şekilde durumunda bırakılması, ancak hiçbir özel durum oluşturulur. Örneğin, bu kod size *değil* özel durum:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Bunun yerine, `Value` özelliği durumunda bırakılması `Maximum` 1 değeri.

Yukarıda gösterilen bir kod parçacığı aşağıda verilmiştir:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Zaman `Minimum` 10'a, sonra ayarlanır `Value` de 10'a ayarlanır.

Varsa bir `ValueChanged` olay işleyicisi, anda bağlı, `Value` özelliği 0 varsayılan değerinin dışında bir şey durumunda bırakılması bir `ValueChanged` olay tetiklenir. XAML bir parçacığı aşağıda verilmiştir:

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Zaman `Minimum` 10'a ayarlanmış `Value` de 10'a ayarlanır ve `ValueChanged` olay tetiklenir. Sayfanın geri kalanını oluşturulur ve henüz oluşturulmamış diğer öğeleri sayfada başvurmak işleyici deneyebilir önce bu durum ortaya çıkabilir. İçin bazı kod eklemek isteyebilirsiniz `ValueChanged` denetler işleyici `null` sayfadaki diğer öğelerin değerleri. Veya ayarlayabilirsiniz `ValueChanged` sonra olay işleyicisi `Slider` değerleri başlatıldı.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Platform uygulaması farkları

Daha önce gösterilen ekran görüntüleri görüntüleme `Slider` ile farklı bir ondalık basamak sayısı. Bu nasıl ilişkili olduğu `Slider` Android ve UWP platformları üzerinde uygulanır.

### <a name="the-android-implementation"></a>Android uygulaması

Android uygulaması `Slider` Android tabanlı [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) ve her zaman ayarlar [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) 1000 özelliği. Diğer bir deyişle `Slider` Android'de yalnızca 1,001 ayrık değerler vardır. Ayarlarsanız `Slider` sağlamak için bir `Minimum` 0 ve `Maximum` 5000'a ve ardından olarak `Slider` yönetilebilir, `Value` değerleri 0, 5, 10, 15 ve benzeri özelliğine sahiptir.

### <a name="the-uwp-implementation"></a>UWP uygulaması

UWP uygulaması `Slider` UWP tabanlı [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) denetimi. `StepFrequency` UWP özelliği `Slider` fark için ayarlanmış `Maximum` ve `Minimum` özellikleri bölünmüş 10, ancak 1'den büyük.

Örneğin, varsayılan aralık 0-1 için `StepFrequency` özelliği ayarlanmış 0,1 aralığındadır. Olarak `Slider` yönetilebilir, `Value` özelliği 0, 0.1, 0.2, 0.3, 0.4, 0,5, 0,6, 0,7, 0,8, 0.9 ve 1.0 için Yasak. (Bu son sayfasında dikkati çekiyor [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) örnek.) Zaman arasındaki farkı `Maximum` ve `Minimum` özellikleri 10 ya da daha sonra `StepFrequency` 1 olarak ayarlanır ve `Value` tamsayı değerlerini özelliğine sahiptir.

### <a name="the-stepslider-solution"></a>StepSlider çözümü

Daha ayrıntılı `StepSlider` içinde ele alınmıştır [Bölüm 27. Özel oluşturucular](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) kitabın *Xamarin.Forms ile Mobile Apps oluşturma*. `StepSlider` Benzer `Slider` ekler, ancak bir `Steps` özellik değerleri arasında sayısını belirtmek için `Minimum` ve `Maximum`.

## <a name="sliders-for-color-selection"></a>Renk seçimi için kaydırıcıları

Son iki içinde sayfa [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) her ikisini de kullanmanız üç örnek `Slider` örnekleri için renk seçimi. İkinci sayfada bir ViewModel ile veri bağlama işlemi gösterilmektedir ancak arka plan kod dosyasında, tüm etkileşimleri ilk sayfasını işler.

### <a name="handling-sliders-in-the-code-behind-file"></a>Arka plan kod dosyasında kaydırıcıları işleme

**RGB renk kaydırıcıları** sayfa başlatır bir `BoxView` bir renk görüntülemek için üç `Slider` rengi ve üç kırmızı, yeşil ve mavi bileşenlerinin seçmek için örnekleri `Label` öğeleri bu renk görüntülemek için değerler:

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

A `Style` üç verir `Slider` aralığı 0 ile 255 arasında bir öğe. `Slider` Öğe paylaştırmak aynı `ValueChanged` arka plan kod dosyasında uygulanan işleyicisi:

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

İlk bölüm kümeleri `Text` özelliği birinin `Label` değerini belirten bir kısa metin dizesi örneklerine `Slider` onaltılık. Ardından, tüm üç `Slider` örnekleri oluşturmak için erişilebilir bir `Color` RGB bileşenleri değerden:

[![RGB renk kaydırıcıları](slider-images/RgbColorSliders.png "RGB renk kaydırıcılar")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Kaydırıcı için bir ViewModel bağlama

**HSL renk kaydırıcıları** sayfası bir ViewModel oluşturmak için kullanılan hesaplamalar gerçekleştirmek için nasıl kullanılacağını gösterir bir `Color` ton, Doygunluk ve parlaklık değerlerden değeri. Gibi tüm Viewmodel'lar `HSLColorViewModel` sınıfının Implements `INotifyPropertyChanged` arabirimi ve ateşlenir bir `PropertyChanged` özelliklerden biri değiştiğinde olay:

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

Viewmodel'lar ve `INotifyPropertyChanged` arabirimi makalesinde açıklanan [veri bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**HslColorSlidersPage.xaml** dosya başlatır `HslColorViewModel` ve sayfanın ayarlar `BindingContext` özelliği. Bu, ViewModel özelliklerinde bağlamak için XAML dosyasındaki tüm öğeleri sağlar:

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

Olarak `Slider` öğeler değiştirilebilir, `BoxView` ve `Label` öğeleri ViewModel güncelleştirildi:

[![HSL renk kaydırıcıları](slider-images/HslColorSliders.png "HSL renk kaydırıcılar")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat` Bileşeninin `Binding` işaretleme uzantısı iki onlu basamak görüntülemek için bir "F2" biçimi için ayarlanır. (Veri bağlamalara biçimlendirme dizesi makalesinde açıklanan [biçimlendirme dizesi](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Ancak, program uygulamanın UWP sürümündeki 0, 0.1, 0.2 değerleri sınırlıdır... 0.9 ve 1.0. Bu bir UWP uygulaması doğrudan sonucudur `Slider` yukarıdaki bölümde açıklandığı gibi [Platform uygulaması farklılıklarını](#implementations).

## <a name="related-links"></a>İlgili bağlantılar

- [Kaydırıcı tanıtımları örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Kaydırıcı API](xref:Xamarin.Forms.Slider)
