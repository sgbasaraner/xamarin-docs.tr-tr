---
title: Xamarin.Forms düğmesi
description: Düğme belirli bir görevi gerçekleştirmek için bir uygulama yönlendirir tıklayın veya dokunun yanıt verir.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/01/2018
ms.openlocfilehash: 095736e77b2f502261f9b85ab73c45dce74309b9
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734092"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms düğmesi

_Düğme belirli bir görevi gerçekleştirmek için bir uygulama yönlendirir tıklayın veya dokunun yanıt verir._ 

[ `Button` ](xref:Xamarin.Forms.Button) Xamarin.Forms tümünde en temel etkileşimli denetimdir. `Button` Bir komutu, ancak belirten kısa bir metin dizesi için de görüntüler. bir bit eşlem resim veya bir birleşimini metin ve görüntü genellikle görüntüler. Kullanıcı basarsa `Button` bir parmak ile veya komut başlatmak için fareyle tıklar.

Aşağıda ele alınan konuların çoğu karşılık sayfalarında [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) örnek.

## <a name="handling-button-clicks"></a>Düğmesini işleme tıklar

`Button` tanımlayan bir [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) kullanıcı dokunur zaman tetikleyen olayı `Button` parmak veya fare işaretçisi ile. Parmak veya fare düğmesini yüzeyden yayımlandığında, bu olay tetiklenir `Button`. `Button` Olmalıdır, [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) özelliğini `true` onun için dokunma yanıt vermesi için. 

**Temel düğmesini tıklatın** sayfasındaki [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) örnek gösterilmektedir örneği oluşturmak nasıl bir `Button` XAML ve tanıtıcı kendi `Clicked` olay. **BasicButtonClickPage.xaml** dosyasını içeren bir `StackLayout` hem bir `Label` ve `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.BasicButtonClickPage"
             Title="Basic Button Click">
    <StackLayout>
        
        <Label x:Name="label"
               Text="Click the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand" 
               HorizontalOptions="Center" />

        <Button Text="Click to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Clicked="OnButtonClicked" />
     
    </StackLayout>
</ContentPage>
```

`Button` İçin izin verilen tüm alan kaplar eğilimindedir. Örneğin ayarlamanız gerekmez, `HorizontalOptions` özelliği `Button` dışında bir şey için `Fill`, `Button` üst tam genişliğini kaplar.

Varsayılan olarak, `Button` dikdörtgen, değil ancak kullanarak BT yuvarlanmış köşeleri sağlayabilir [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius) özelliği, aşağıdaki bölümde açıklandığı gibi [ **düğmesini Görünüm** ](#button-appearance).

[ `Text` ](xref:Xamarin.Forms.Button.Text) Özellik belirtir görünür metin `Button`. [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Olay adlı bir olay işleyicisi için ayarlanmış `OnButtonClicked`. Bu işleyici arka plan kodu dosyasında bulunan **BasicButtonClickPage.xaml.cs**:

```csharp
public partial class BasicButtonClickPage : ContentPage
{
    public BasicButtonClickPage ()
    {
        InitializeComponent ();
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        await label.RelRotateTo(360, 1000);
    }
}
```

Zaman `Button` dokunduğunuz, `OnButtonClicked` yöntemini yürütür. `sender` Bağımsız değişkeni `Button` bu olay için sorumlu nesne. Bu erişmek için kullanabileceğiniz `Button` nesnesi veya birden çok arasında ayrım yapmak için `Button` aynı paylaşımı nesneleri `Clicked` olay.

Bu belirli `Clicked` işleyici çağırması döndüğü bir animasyon işlevi `Label` 360 derece 1000 milisaniye cinsinden. İOS ve Android cihazlarda ve evrensel Windows Platformu (UWP) uygulaması olarak Windows 10 masaüstünde çalışan program şöyledir:

[![Temel düğmesini tıklatın](button-images/BasicButtonClick.png "temel düğmesini tıklatın")](button-images/BasicButtonClick-Large.png#lightbox "temel düğmesini tıklatın")

Dikkat `OnButtonClicked` yöntemi içerir `async` değiştiricisi çünkü `await` olay işleyicisi içinde kullanılır. A `Clicked` olay işleyicisi gerektirir `async` yalnızca işleyici gövdesi kullanıyorsa değiştirici `await`.

Her platform işler `Button` kendi belirli şekilde. İçinde [ **düğmesini Görünüm** ](#button-appearance) bölümünde renklerini ayarlama yapmaya öğreneceksiniz `Button` kenarlığı için daha özelleştirilmiş görünümler görünür. `Button` uygulayan [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement) arabirim içerecek şekilde [ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize), ve [ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)özellikleri.

## <a name="creating-a-button-in-code"></a>Kodda bir düğme oluşturma

Örneği oluşturmak için ortak bir `Button` XAML'de ancak de oluşturabilirsiniz bir `Button` kod. Uygulamanız ile numaralandırılabilir verileri temel alan birden çok düğmeler oluşturmak gerektiğinde bu kullanışlı olabilir bir `foreach` döngü.

**Kod düğmesini** sayfasını gösteren işlevsel olarak eşdeğerdir bir sayfa oluşturma **temel düğmesini tıklatın** sayfasında ancak tamamen C#:

```csharp
public class CodeButtonClickPage : ContentPage
{
    public CodeButtonClickPage ()
    {
        Title = "Code Button Click";

        Label label = new Label
        {
            Text = "Click the Button below",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };

        Button button = new Button
        {
            Text = "Click to Rotate Text!",
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };
        button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);

        Content = new StackLayout
        {
            Children =
            {
                label,
                button
            }
        };
    }
}
```

Her şeyi sınıfının oluşturucusunda yapılır. Çünkü `Clicked` işleyici yalnızca bir deyim uzun, bu çok basitçe olaya eklenebilecek:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Elbette, ayrıca olay işleyicisi ayrı bir yöntem tanımlayabilirsiniz (olduğu gibi `OnButtonClick` yönteminde **temel düğmesini tıklatın**) ve bu yöntem olaya ekleyin:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Düğme devre dışı bırakma

Bazen bir uygulamanın belirli bir yerde belirli bir durumda olduğundan `Button` tıklatın geçerli bir işlem değil. Bu durumlarda, `Button` ayarlayarak devre dışı bırakılmalıdır kendi `IsEnabled` özelliğine `false`. Klasik örnek bir `Entry` denetimi tarafından bir dosyanın açılması eşlik bir dosya adı için `Button`: `Button` yalnızca belirli bir metin halinde yazılan ise etkinleştirilmelidir `Entry`. Kullanabileceğiniz bir `DataTrigger` gösterildiği gibi bu görev için [ **veri Tetikleyicileri** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) makalesi.

## <a name="using-the-command-interface"></a>Komut arabirimi kullanma

Yanıt bir uygulama için olası `Button` Tap işleme olmadan `Clicked` olay. `Button` Adlı bir alternatif bildirim mekanizması uygulayan _komutu_ veya _kumanda_ arabirimi. Bu iki özellikten oluşur:

- [`Command`](xref:Xamarin.Forms.Button.Command) tür [ `ICommand` ](xref:System.Windows.Input.ICommand), tanımlı bir arabirim [ `System.Windows.Input` ](xref:System.Windows.Input) ad alanı.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) özellik türü [ `Object` ](xref:System.Object).

Bu yaklaşım özellikle Model View ViewModel (MVVM) mimarisi uygularken özellikle veri bağlama bağlantılı olarak ve uygundur. Bu konularda makalelerinde açıklanan [veri bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md), [gelen veri bağlamalar MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), ve [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md).

MVVM uygulamada ViewModel türünün özelliklerini tanımlayan `ICommand` XAML için bağlı sonra `Button` veri bağlamaları öğeleriyle. Xamarin.Forms ayrıca tanımlar [ `Command` ]((xref:Xamarin.Forms.Command`1)) ve [ `Command<T>` ](xref:Xamarin.Forms.Command`1) sınıfları uygulayan `ICommand` arabirim ve ViewModel türününözelliklerinitanımlamanızayardımcı`ICommand`.

Kumanda açıklanmaktadır makalesinde daha ayrıntılı [ **komut arabirimi** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) ancak **temel düğmesi komutu** sayfasındaki [  **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) örnek temel yaklaşım gösterir.

`CommandDemoViewModel` Sınıftır türünde bir özellik tanımlayan çok basit bir ViewModel `double` adlı `Number`ve iki türünün özelliklerini `ICommand` adlı `MultiplyBy2Command` ve `DivideBy2Command`:

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    double number = 1;

    public event PropertyChangedEventHandler PropertyChanged;

    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(() => Number *= 2);

        DivideBy2Command = new Command(() => Number /= 2);
    }

    public double Number
    {
        set
        {
            if (number != value)
            {
                number = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Number"));
            }
        }
        get
        {
            return number;
        }
    }

    public ICommand MultiplyBy2Command { private set; get; }

    public ICommand DivideBy2Command { private set; get; }
}
```

İki `ICommand` özellikleri iki nesne türü ile sınıfının oluşturucusunda başlatılır `Command`. `Command` Oluşturucular dahil biraz işlevi (adlı `execute` oluşturucu bağımsız değişkeni) iki katına çıkar veya yarıları `Number` özelliği.

**BasicButtonCommand.xaml** dosya kümeleri kendi `BindingContext` örneğine `CommandDemoViewModel`. `Label` Öğesi ve iki `Button` öğelerini içeren üç özelliklerinde bağlamalar `CommandDemoViewModel`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.BasicButtonCommandPage"
             Title="Basic Button Command">
    
    <ContentPage.BindingContext>
        <local:CommandDemoViewModel />
    </ContentPage.BindingContext>
    
    <StackLayout>
        <Label Text="{Binding Number, StringFormat='Value is now {0}'}"
               FontSize="Large"
               VerticalOptions="CenterAndExpand" 
               HorizontalOptions="Center" />

        <Button Text="Multiply by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding MultiplyBy2Command}" />

        <Button Text="Divide by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding DivideBy2Command}" />
    </StackLayout>
</ContentPage>
```

İki olarak `Button` öğeleri dokunduğunuz, komutları çalıştırılır, ve sayı değerini değiştirir:

[![Temel düğmesi komutu](button-images/BasicButtonCommand.png "temel düğmesi komutu")](button-images/BasicButtonCommand-Large.png#lightbox)

Bu yaklaşımın avantajı `Clicked` işleyicileri olduğundan bu sayfayı işlevselliğini içeren tüm mantığı arka plan kod dosyası yerine ViewModel daha iyi bir kullanıcı arabirimi ayrılması iş mantığı alanından elde bulunur.

Ayrıca mümkündür `Command` etkinleştirme ve devre dışı bırakma, denetim nesnelere `Button` öğeleri. Örneğin, 2 arasında sayı değerleri aralığı sınırlamak istediğinizi varsayalım<sup>10</sup> ve 2<sup>&ndash;10</sup>. Oluşturucuya başka bir işlev ekleyebilirsiniz (adlı `canExecute` bağımsız değişkeni) döndüren `true` varsa `Button` etkinleştirilmiş olmalıdır. Değişiklik işte `CommandDemoViewModel` Oluşturucusu:

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    ···
    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(
            execute: () =>
            {
                Number *= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number < Math.Pow(2, 10));

        DivideBy2Command = new Command(
            execute: () =>
            {
                Number /= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number > Math.Pow(2, -10));
    }
    ···
}
```

Çağrıları `ChangeCanExecute` yöntemi `Command` gereken şekilde `Command` yöntemi çağırabilirsiniz `canExecute` yöntemi ve belirlemek olup olmadığını `Button` veya devre dışı bırakılması gerekir. Bu kod değişikliğine sayı olarak sınırına ulaştığında `Button` devre dışı bırakılır:

[![Temel düğmesi komutu değiştiren -](button-images/BasicButtonCommandModified.png "temel düğmesini Command - değiştiren")](button-images/BasicButtonCommandModified-Large.png#lightbox)

İki ya da daha fazla mümkündür `Button` aynı bağlanacak öğeleri `ICommand` özelliği. `Button` Öğeleri kullanılarak ayırt edilen [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter) özelliği `Button`. Bu durumda, genel kullanmak istersiniz [ `Command<T>` ](xref:Xamarin.Forms.Command`1) sınıfı. `CommandParameter` Nesne bağımsız değişken olarak geçirilen sonra `execute` ve `canExecute` yöntemleri. Bu teknik ayrıntılı olarak gösterilen [ **temel kumanda** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) bölümünü [ **komut arabirimi** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) makalesi.

[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) örnek bu tekniği de kullanır, `MainPage` sınıfı. **MainPage.xaml** dosyasını içeren bir `Button` örnek her sayfası için:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.MainPage"
             Title="Button Demos">
    <ScrollView>
        <FlexLayout Direction="Column"
                    JustifyContent="SpaceEvenly"
                    AlignItems="Center">

            <Button Text="Basic Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonClickPage}" />

            <Button Text="Code Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:CodeButtonClickPage}" />

            <Button Text="Basic Button Command"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonCommandPage}" />

            <Button Text="Press and Release Button"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:PressAndReleaseButtonPage}" />

            <Button Text="Button Appearance"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ButtonAppearancePage}" />

            <Button Text="Toggle Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ToggleButtonDemoPage}" />

            <Button Text="Image Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ImageButtonDemoPage}" />

        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Her `Button` sahip kendi `Command` özelliği bağlı adlı bir özellik `NavigateCommand`ve `CommandParameter` ayarlanmış bir [ `Type` ](xref:System.Type) projedeki sayfa sınıfların birine karşılık gelen nesne.

Olduğunu `NavigateCommand` özelliği türüdür `ICommand` ve arka plan kodu dosyasında tanımlanır:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Oluşturucu başlatır `NavigateCommand` özelliğine bir `Command<Type>` çünkü nesne `Type` türü `CommandParameter` XAML dosyasında ayarlanan nesnesi. Bunun anlamı `execute` yöntemi sahip türünde bir bağımsız değişken `Type` karşılık gelen için `CommandParameter` nesnesi. İşlev sayfası oluşturur ve ona gider.

Ayarlayarak Oluşturucusu sonucuna dikkat edin, `BindingContext` kendisi. Bu özellikler bağlamak için XAML dosyasındaki gereklidir `NavigateCommand` özelliği.

## <a name="pressing-and-releasing-the-button"></a>Tuşuna basarak ve düğme serbest bırakma

Yanında `Clicked` olayı `Button` ayrıca tanımlar [ `Pressed` ](xref:Xamarin.Forms.Button.Pressed) ve [ `Released` ](xref:Xamarin.Forms.Button.Released) olaylar. `Pressed` Olaylarının bir parmak bastığında bir `Button`, veya üzerine yerleştirilmiş işaretçisi ile fare düğmesini basılı `Button`. `Released` Olayı parmak veya fare düğmesini serbest bırakıldığında oluşur. Genellikle, bir `Clicked` olay aynı zamanda şu de `Released` parmak veya fare işaretçisini yüzeyinin çıktığınızda slayt varsa ancak olay `Button` yayımlanan önce `Clicked` olay meydana.

`Pressed` Ve `Released` olaylar genellikle kullanılmaz, ancak özel amaçlar için örnekte gösterildiği şekilde kullanılabilmesi için **basın ve yayın düğmesi** sayfası. XAML dosyasını içeren bir `Label` ve `Button` işleyicileri için bağlı olan `Pressed` ve `Released` olayları:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.PressAndReleaseButtonPage"
             Title="Press and Release Button">
    <StackLayout>

        <Label x:Name="label"
               Text="Press and hold the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand" 
               HorizontalOptions="Center" />

        <Button Text="Press to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Pressed="OnButtonPressed"
                Released="OnButtonReleased" />

    </StackLayout>
</ContentPage>
```

Arka plan kod dosyasına canlandırır `Label` zaman bir `Pressed` olay gerçekleşir, ancak döndürme askıya alır, bir `Released` olayı oluşur:

```csharp
public partial class PressAndReleaseButtonPage : ContentPage
{
    bool animationInProgress = false;
    Stopwatch stopwatch = new Stopwatch();

    public PressAndReleaseButtonPage ()
    {
        InitializeComponent ();
    }

    void OnButtonPressed(object sender, EventArgs args)
    {
        stopwatch.Start();
        animationInProgress = true;

        Device.StartTimer(TimeSpan.FromMilliseconds(16), () =>
        {
            label.Rotation = 360 * (stopwatch.Elapsed.TotalSeconds % 1);

            return animationInProgress;
        });
    }

    void OnButtonReleased(object sender, EventArgs args)
    {
        animationInProgress = false;
        stopwatch.Stop();
    }
}
```

Sonuç `Label` bağlantılı bir parmak iken yalnızca döndüğü `Button`ve parmak yayımlandığında durdurur:

[![Düğmesini basıp](button-images/PressAndReleaseButton.png "tuşuna basın ve düğmesini bırakın")](button-images/PressAndReleaseButton-Large.png)

Uygulamaları oyunlar için bu tür bir davranışı vardır: tutulan bir parmak bir `Button` belirli bir yönde taşımak üzerinde bir ekran nesnesi neden olabilir. 

<a name="button-appearance" />

## <a name="button-appearance"></a>Düğme görünümünü

`Button` Devralır veya görünümünü etkileyen çeşitli özellikleri tanımlar:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) rengi `Button` metin
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) Bu metin arka plan rengi
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) bir alanını çevreleyen rengi `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) yazı tipi ailesinin metni için kullanılır
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) metin boyutu
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) metni italik veya kalın olup olmadığını gösterir
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) Kenarlık genişliği 
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) Köşeleri yuvarlar

Bu özelliklerin altı etkilerini (hariç `FontFamily` ve `FontAttributes`) örneklerde gösterildiği **düğme görünümünü** sayfası. Başka bir özellik [ `Image` ](xref:Xamarin.Forms.Button.Image), bölümünde anlatılan [ **bit eşlemler düğmesini kullanarak**](#image-button).

Tüm görünümler ve verilerin bağlamalar **düğme görünümünü** sayfa XAML dosyasında tanımlanır:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ButtonAppearancePage"
             Title="Button Appearance">
    <StackLayout>
        <Button x:Name="button"
                Text="Button"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                TextColor="{Binding Source={x:Reference textColorPicker},
                                    Path=SelectedItem.Color}"
                BackgroundColor="{Binding Source={x:Reference backgroundColorPicker},
                                          Path=SelectedItem.Color}"
                BorderColor="{Binding Source={x:Reference borderColorPicker},
                                      Path=SelectedItem.Color}" />

        <StackLayout BindingContext="{x:Reference button}"
                     Padding="10">
            
            <Slider x:Name="fontSizeSlider"
                    Maximum="48"
                    Minimum="1"
                    Value="{Binding FontSize}" />

            <Label Text="{Binding Source={x:Reference fontSizeSlider},
                                  Path=Value,
                                  StringFormat='FontSize = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="borderWidthSlider"
                    Minimum="-1"
                    Maximum="12"
                    Value="{Binding BorderWidth}" />
            
            <Label Text="{Binding Source={x:Reference borderWidthSlider}, 
                                  Path=Value,
                                  StringFormat='BorderWidth = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="cornerRadiusSlider"
                    Minimum="-1"
                    Maximum="24"
                    Value="{Binding CornerRadius}" />

            <Label Text="{Binding Source={x:Reference cornerRadiusSlider}, 
                                  Path=Value,
                                  StringFormat='CornerRadius = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.Resources>
                    <Style TargetType="Label">
                        <Setter Property="VerticalOptions" Value="Center" />
                    </Style>
                </Grid.Resources>

                <Label Text="Text Color:"
                       Grid.Row="0" Grid.Column="0" />

                <Picker x:Name="textColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="0" Grid.Column="1" />

                <Label Text="Background Color:"
                       Grid.Row="1" Grid.Column="0" />

                <Picker x:Name="backgroundColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="1" Grid.Column="1" />

                <Label Text="Border Color:"
                       Grid.Row="2" Grid.Column="0" />

                <Picker x:Name="borderColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="2" Grid.Column="1" />
            </Grid>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

`Button` Sayfanın en üstünde, üç sahip `Color` ilişkili özellikleri `Picker` sayfanın öğeler. Öğeleri `Picker` öğeleridir renkten `NamedColor` projeye dahil sınıfı. Üç `Slider` öğeleri içeren iki yönlü bağlamaları `FontSize`, `BorderWidth`, ve `CornerRadius` özelliklerini `Button`.

Bu program, bu özellikleri birleşimlerine deneme sağlar:

[![Düğme görünümünü](button-images/ButtonAppearance.png "düğmesini görünümü")](button-images/ButtonAppearance-Large.png)

Görmek için `Button` kenarlık gerekir ayarlamak bir `BorderColor` dışında bir şey için `Default`ve `BorderWidth` için pozitif bir değer.

İos'ta, büyük kenarlık genişliklerini iç intrude fark edeceksiniz `Button` ve metin ekranıyla müdahale. Bir iOS kenarlık kullanmayı seçerseniz `Button`, sonra büyük olasılıkla başlamalı ve bitmelidir istersiniz `Text` görünürlüğü korumak boşluklu özellik.

UWP üzerinde seçerek bir `CornerRadius` yarım yüksekliğini aşıyor `Button` bir özel durum oluşturur.

## <a name="creating-a-toggle-button"></a>İki durumlu düğme oluşturma

Bir alt kümesi için mümkündür `Button` böylece bir açık-kapalı anahtarı gibi çalışır: iki durumlu düğme ve yeniden kapalı geçiş için dokunun kez düğmesine dokunun.

Aşağıdaki `ToggleButton` sınıfı türer `Button` ve adlı yeni bir olay tanımlar `Toggled` ve adlı bir Boolean özelliği `IsToggled`. Xamarin.Forms tarafından tanımlanan aynı iki özellikleri bunlar [ `Switch` ](xref:Xamarin.Forms.Switch):

```csharp
class ToggleButton : Button
{
    public event EventHandler<ToggledEventArgs> Toggled;

    public static BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled", typeof(bool), typeof(ToggleButton), false,
                                propertyChanged: OnIsToggledChanged);

    public ToggleButton()
    {
        Clicked += (sender, args) => IsToggled ^= true;
    }

    public bool IsToggled
    {
        set { SetValue(IsToggledProperty, value); }
        get { return (bool)GetValue(IsToggledProperty); }
    }

    protected override void OnParentSet()
    {
        base.OnParentSet();
        VisualStateManager.GoToState(this, "ToggledOff");
    }

    static void OnIsToggledChanged(BindableObject bindable, object oldValue, object newValue)
    {
        ToggleButton toggleButton = (ToggleButton)bindable;
        bool isToggled = (bool)newValue;

        // Fire event
        toggleButton.Toggled?.Invoke(toggleButton, new ToggledEventArgs(isToggled));

        // Set the visual state
        VisualStateManager.GoToState(toggleButton, isToggled ? "ToggledOn" : "ToggledOff");
    }
}
```

`ToggleButton` Oluşturucusu bağlayan bir işleyiciye `Clicked` onun değerini değiştirebilmeniz için olay `IsToggled` özelliği. `OnIsToggledChanged` Yöntemi ateşlenir `Toggled` olay. 

Son satırının `OnIsToggledChanged` yöntemini çağırır statik `VisualStateManager.GoToState` iki metin yöntemiyle dizeleri "ToggledOn" ve "ToggledOff". Okuyabilirsiniz bu yöntem ve uygulamanızı makalede visual durumları nasıl yanıtlayabilir hakkında [ **Xamarin.Forms görsel durum Yöneticisi**](~/xamarin-forms/user-interface/visual-state-manager.md). 

Çünkü `ToggleButton` çağrıda `VisualStateManager.GoToState`, sınıf temel düğmenin görünümünü değiştirmek için tüm ek özellikler içerir gerekmez, `IsToggled` durumu. Diğer bir deyişle barındıran XAML sorumluluk `ToggleButton`. 

**İki durumlu düğme Demo** sayfasını içeren iki örneğini `ToggleButton`, ayarlar görsel durum Yöneticisi biçimlendirme dahil olmak üzere `Text`, `BackgroundColor`, ve `TextColor` düğmenin görsel durumuna göre: 

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ToggleButtonDemoPage"
             Title="Toggle Button Demo">
    
    <ContentPage.Resources>
        <Style TargetType="local:ToggleButton">
            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            <Setter Property="HorizontalOptions" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <local:ToggleButton Toggled="OnItalicButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Italic Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Italic On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <local:ToggleButton Toggled="OnBoldButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Bold Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>
                    
                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Bold On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <Label x:Name="label"
               Text="Just a little passage of some sample text that can be formatted in italic or boldface by toggling the two buttons."
               FontSize="Large"
               HorizontalTextAlignment="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

`Toggled` Olay işleyicileri olan arka plan kod dosyasına. İçin ayar sorumlu `FontAttributes` özelliği `Label` düğmeleri durumuna bağlıdır:

```csharp
public partial class ToggleButtonDemoPage : ContentPage
{
    public ToggleButtonDemoPage ()
    {
        InitializeComponent ();
    }

    void OnItalicButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Italic;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Italic;
        }
    }

    void OnBoldButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Bold;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Bold;
        }
    }
}
```

İOS, Android ve UWP çalıştıran program şöyledir:

[![Değiştirme düğmesi Demo](button-images/ToggleButtonDemo.png "Değiştir düğmesi Tanıtımı")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>Bit eşlemler düğmeleriyle kullanma

`Button` Sınıfı tanımlayan bir [ `Image` ](xref:Xamarin.Forms.Button.Image) bir bit eşlem resim görüntülemek izin veren özellik `Button`, tek başına veya birlikte metinle. Metin ve resim nasıl düzenlenir de belirtebilirsiniz.

`Image` Özelliği türüdür [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), bit eşlemler kaynaklarını tek tek platform projelerinde ve .NET standart kitaplığı proje içinde değil olarak depolanması gereken anlamına gelir. 

Xamarin.Forms tarafından desteklenen her platform uygulaması çalışabilir çeşitli cihazların farklı piksel çözünürlüğü için çeşitli boyutlarda depolanması görüntüleri sağlar. Bu Çoklu bit eşlemler adlı veya aygıt video için işletim sistemini en iyi eşleşmeyi seçebilirsiniz şekilde depolanan ekran çözünürlüğü. 

Bir bit eşlem'i için bir `Button`en iyi boyutu genellikle 32 ve 64 CİHAZDAN bağımsız birimler arasında büyüklüğüne bağlı olarak, olmasını istiyorsanız. Bu örnekte kullanılan görüntüleri 48 CİHAZDAN bağımsız birimler boyutuna dayanır.

İOS projesinde **kaynakları** klasörü, bu görüntünün üç boyut içerir:

- Olarak saklanan bir 48 piksel kare bitmap **/Resources/MonkeyFace.png**
- Olarak depolanan 96 piksel kare bit eşlem **/Resource/MonkeyFace@2x.png**
- Olarak depolanan 144 piksel kare bit eşlem **/Resource/MonkeyFace@3x.png**

Tüm üç bit eşlemler verilmiş olan bir **yapı eylemi** , **BundleResource**.

Android projesi için tüm bit eşlemler aynı ada sahip, ancak farklı alt klasörlerinde depolanan **kaynakları** klasörü:

- Olarak saklanan bir 72 piksel kare bitmap **/Resources/drawable-hdpi/MonkeyFace.png**
- Olarak saklanan bir 96 piksel kare bitmap **/Resources/drawable-xhdpi/MonkeyFace.png**
- Olarak saklanan bir 144 piksel kare bitmap **/Resources/drawable-xxhdpi/MonkeyFace.png**
- Olarak saklanan bir 192 piksel kare bitmap **/Resources/drawable-xxxhdpi/MonkeyFace.png**

Bunlar verilmiş olan bir **yapı eylemi** , **AndroidResource**.

İçinde UWP projesini bit eşlemler depolanabilir herhangi bir yere projesinde, ancak genellikle özel bir klasörde depolanır veya **varlıklar** varolan bir klasörü. UWP projesi bu bit eşlemler içerir:

- Olarak saklanan bir 48 piksel kare bitmap **/Assets/MonkeyFace.scale-100.png**
- Olarak saklanan bir 96 piksel kare bitmap **/Assets/MonkeyFace.scale-200.png**
- Olarak saklanan bir 192 piksel kare bitmap **/Assets/MonkeyFace.scale-400.png**

Tüm verilmiş olan bir **yapı eylemi** , **içerik**.

Belirleyebileceğiniz nasıl `Text` ve `Image` özellikler üzerinde düzenlenir `Button` kullanarak [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout) özelliği `Button`. Bu özellik türünde [ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout), katıştırılmış bir sınıfta olduğu `Button`. [Oluşturucusu](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) iki bağımsız değişkenlere sahiptir:

- Üye [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) numaralandırma: `Left`, `Top`, `Right`, veya `Bottom` bit eşlem göre metin görünme biçimini belirten.
- A `double` bit eşlem ve metin arasındaki aralığı değeri.

Varsayılanlar `Left` ve 10 birim. İki salt okunur özelliklerini `ButtonContentLayout` adlı [ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) ve [ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) bu özelliklerin değerlerini sağlayın.

Kod içinde oluşturduğunuz bir `Button` ve `ContentLayout` özelliği şuna benzer:

```csharp
Button button = new Button
{
    Text = "button text",
    Image = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

XAML'de yalnızca numaralandırma üyesi ya da boşluk belirtmeniz ya da herhangi bir sırada hem de virgülle ayırarak:

```xaml
<Button Text="button text"
        Image="image filename"
        ContentLayout="Right, 20" />
```

**Görüntü düğmesi Demo** sayfasında kullanır `OnPlatform` iOS, Android ve UWP dosyaları bit eşlem farklı dosya adlarını belirtmek için. Tüm üç platformları için aynı adı kullanın ve kullanımını önlemek istiyorsanız `OnPlatform`, UWP bit eşlemler proje kök dizininde depolamak gerekir.

İlk `Button` üzerinde **görüntü düğmesi Demo** Ayarlar sayfasında `Image` özelliği kullanmamayı `Text` özelliği:

```xaml
<Button>
    <Button.Image>
        <OnPlatform x:TypeArguments="FileImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.Image>
</Button>
```

UWP bit eşlemler proje kök dizininde depolanır, bu biçimlendirme oldukça basit hale getirilebilir:

```xaml
<Button Image="MonkeyFace.png" />
```

Çok sayıda tekrarlayan biçimlendirme önlemek için **ImageButtonDemo.xaml** dosya, örtülü `Style` de ayarlamak için tanımlanan `Image` özelliği. Bu `Style` beş olmak üzere diğer otomatik olarak uygulanan `Button` öğeleri. Tam XAML dosyası şöyledir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">
        
        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="Image">
                    <OnPlatform x:TypeArguments="FileImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.Image>
                <OnPlatform x:TypeArguments="FileImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.Image>
        </Button>

        <Button Text="Default" />

        <Button Text="Left - 10"
                ContentLayout="Left, 10" />

        <Button Text="Top - 10"
                ContentLayout="Top, 10" />

        <Button Text="Right - 20"
                ContentLayout="Right, 20" />

        <Button Text="Bottom - 20" 
                ContentLayout="Bottom, 20" />
    </FlexLayout>
</ContentPage>
```

Son dört `Button` öğeleri olun kullanımı `ContentLayout` özelliği bir konum ve metin ve bit eşlem aralığını belirtin:

[![Görüntü düğmesi Demo](button-images/ImageButtonDemo.png "görüntü düğmesi Tanıtımı")](button-images/ImageButtonDemo-Large.png#lightbox)

Şimdi işleyebilir çeşitli şekillerde gördünüz `Button` olaylar ve değişiklik `Button` görünümü.

## <a name="related-links"></a>İlgili bağlantılar

- [ButtonDemos örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)
- [Düğme API](xref:Xamarin.Forms.Button)
