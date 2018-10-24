---
title: Xamarin.Forms düğmesi
description: Düğmeyi bir uygulamanın belirli bir görevlerini gerçekleştirmek üzere yönlendiren ya tıklayın veya dokunun yanıt verir.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/26/2018
ms.openlocfilehash: ed711efe7f4b138b3ba61437dc96f8aa07d17aeb
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "35244922"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms düğmesi

_Düğmeyi bir uygulamanın belirli bir görevlerini gerçekleştirmek üzere yönlendiren ya tıklayın veya dokunun yanıt verir._

[ `Button` ](xref:Xamarin.Forms.Button) Xamarin.Forms tümünde en temel etkileşimli denetim. `Button` Genellikle bir bit eşlem görüntüsü veya bir birleşimini metin ve görüntü komutu, ancak bunu belirten bir kısa metin dizesi aynı zamanda görüntüler görüntüler. Kullanıcı basarsa `Button` nabzını ile veya komut başlatmak için fareyle tıklar.

Aşağıdaki konular çoğunu karşılık gelen sayfalarına [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) örnek.

## <a name="handling-button-clicks"></a>İşleme düğmesine tıklar

`Button` tanımlayan bir [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) kullanıcı dokunduğunda harekete geçirilen olay `Button` parmak veya fare işaretçisi. Yüzeyinin parmak veya fare düğmesi bırakıldığında bu olay harekete geçirilir `Button`. `Button` Olmalıdır, [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) özelliğini `true` , Tap'ları için yanıt vermesi için.

**Temel düğmesine** sayfasını [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) örnek oluşturmak nasıl gösterir bir `Button` XAML ve tanıtıcı kendi `Clicked` olay. **BasicButtonClickPage.xaml** dosyasını içeren bir `StackLayout` hem bir `Label` ve `Button`:

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

`Button` İçin izin verilen tüm alanı kaplayan eğilimindedir. Örneğin ayarlamazsanız `HorizontalOptions` özelliği `Button` dışında bir şey `Fill`, `Button` , üst öğesinin tam genişlikli dolduracaktır.

Varsayılan olarak, `Button` dikdörtgen, değil ancak kullanarak BT yuvarlatılmış köşeler tanıyabilirsiniz [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius) aşağıdaki bölümde açıklandığı gibi özellik [ **düğmesini Görünüm** ](#button-appearance).

[ `Text` ](xref:Xamarin.Forms.Button.Text) Özelliği içinde görüntülenen metni belirtir `Button`. [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Olay adlı bir olay işleyicisi için ayarlanmış `OnButtonClicked`. Bu işleyici arka plan kod dosyasında bulunan **BasicButtonClickPage.xaml.cs**:

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

Zaman `Button` dokunulduğunda, `OnButtonClicked` yöntemini yürütür. `sender` Bağımsız değişkeni `Button` bu olay için sorumlu nesne. Bu erişmek için kullanabileceğiniz `Button` nesnesi veya birden çok arasında ayrım yapmak için `Button` nesneleri aynı paylaşımı `Clicked` olay.

Bu özellikle `Clicked` işleyici çağırır döndürür bir animasyon işlevi `Label` 360 derece 1000 milisaniye cinsinden. Programın bir evrensel Windows Platformu (UWP) uygulaması ve iOS ve Android cihazlarda Windows 10 Masaüstü çalıştıran şu şekildedir:

[![Temel düğmesi Tıklamasından](button-images/BasicButtonClick.png "temel düğmesi Tıklamasından")](button-images/BasicButtonClick-Large.png#lightbox "temel düğmesini tıklatın")

Dikkat `OnButtonClicked` includes yöntemi `async` değiştiricisi çünkü `await` olay işleyicisinde kullanılır. A `Clicked` olay işleyicisi gerektirir `async` işleyicisinin gövdesi kullanıyorsa değiştiricisi `await`.

Her platform işler `Button` kendi belirli şekilde. İçinde [ **düğmesini Görünüm** ](#button-appearance) bölümünde renklerini ayarlama ve gördüğünüz `Button` kenarlık daha özelleştirilmiş görünümleri için görünür. `Button` uygulayan [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement) arabirim içerecek [ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize), ve [ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)özellikleri.

## <a name="creating-a-button-in-code"></a>Kodda bir düğme oluşturma

Örneklemek için ortak olan bir `Button` , XAML içinde oluşturabilirsiniz, ancak bir `Button` kod. Uygulamanız ile numaralandırılabilir verileri temel alan birden çok düğmeler oluşturma ihtiyacı olduğunda bu kullanışlı olabilir bir `foreach` döngü.

**Kod düğmesine** sayfası için işlevsel olarak eşdeğerdir bir sayfa oluşturmak nasıl gösterir **temel düğmesine tıklayın** tamamen ancak sayfa C#:

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

Her şeyi sınıfın oluşturucusunda gerçekleştirilir. Çünkü `Clicked` işleyicisi yalnızca bir deyim uzun olduğundan, bu çok basit bir olaya eklenebilecek:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Elbette, ayrıca olay işleyicisi ayrı bir yöntem tanımlayabilirsiniz (olduğu gibi `OnButtonClick` yöntemi **temel düğmesine**) ve bu yöntem olaya ekleyin:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Düğmeyi devre dışı bırakma

Bazen bir uygulama belirli bir yerde belirli bir durumda olduğundan `Button` tıklama geçerli bir işlem değildir. Bu durumlarda `Button` ayarlayarak devre dışı bırakılmalıdır kendi `IsEnabled` özelliğini `false`. Klasik örnek bir `Entry` denetimi için bir dosya adı tarafından bir dosya açma eşlik `Button`: `Button` içine metin yalnızca yazıldığını ise etkinleştirilmelidir `Entry`.
Kullanabileceğiniz bir `DataTrigger` gösterildiği gibi bu görev için [ **veri Tetikleyicileri** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) makalesi.

## <a name="using-the-command-interface"></a>Komut arabirimi kullanma

Yanıt vermek bir uygulama için olası `Button` Tap'ları işleme olmadan `Clicked` olay. `Button` Adlı bir diğer bildirim mekanizması uygular _komut_ veya _komut vermeye genel_ arabirimi. Bu iki özellikten oluşur:

- [`Command`](xref:Xamarin.Forms.Button.Command) tür [ `ICommand` ](xref:System.Windows.Input.ICommand), tanımlanan bir arabirim [ `System.Windows.Input` ](xref:System.Windows.Input) ad alanı.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) özellik türü [ `Object` ](xref:System.Object).

Bu yaklaşım özellikle Model-View-ViewModel (MVVM) mimarisi uygularken özellikle bağlantılı veri bağlama ve uygundur. Bu konu başlıkları makalelerinde açıklanan [veri bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md), [gelen veri bağlamaları-MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), ve [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md).

MVVM uygulamada ViewModel türünün özelliklerini tanımlar `ICommand` için XAML bağlı ardından `Button` veri bağlamaları ile öğeleri. Xamarin.Forms ayrıca tanımlar [ `Command` ]((xref:Xamarin.Forms.Command`1)) ve [ `Command<T>` ](xref:Xamarin.Forms.Command`1) uygulayan sınıflar `ICommand` arabirim ve ViewModel türününözelliklerinitanımlamanızayardımcı`ICommand`.

Komut vermeye genel makalesinde daha ayrıntılı açıklanmıştır [ **komut arabirimi** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) ancak **düğmesine temel komut** sayfasını [  **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) örnek temel yaklaşım gösterilmektedir.

`CommandDemoViewModel` Sınıfı, bir özellik türü tanımlayan çok basit bir ViewModel `double` adlı `Number`, iki tür özelliklerini `ICommand` adlı `MultiplyBy2Command` ve `DivideBy2Command`:

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

İki `ICommand` özellikleri ve sınıf oluşturucusunun iki nesne türü ile başlatılır `Command`. `Command` Oluşturucular biraz işlevi içerir (adlı `execute` oluşturucu bağımsız değişkeni) iki katına çıkar veya yarıları `Number` özelliği.

**BasicButtonCommand.xaml** dosya kümeleri kendi `BindingContext` örneğine `CommandDemoViewModel`. `Label` Öğesi ve iki `Button` öğeleri içeren bağlamaları üç özelliklerinde `CommandDemoViewModel`:

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

İki olarak `Button` öğeleri dokunulduğunda, komutlar çalıştırılır, ve sayı değeri değiştirilir:

[![Temel düğme komutunu](button-images/BasicButtonCommand.png "temel düğmesi komutu")](button-images/BasicButtonCommand-Large.png#lightbox)

Bu yaklaşımın avantajı `Clicked` işleyicileri olan bu sayfa işlevlerini içeren tüm mantığı arka plan kod dosyası yerine ViewModel kullanıcı arabirimi iş mantığından daha iyi bir ayrım sağlamak bulunur.

Ayrıca mümkündür `Command` etkinleştirme ve, devre dışı bırakma denetlemenizi nesneleri `Button` öğeleri. Örneğin, 2 arasındaki sayı değerleri aralığı sınırlamak istediğiniz varsayalım<sup>10</sup> ve 2<sup>&ndash;10</sup>. Oluşturucuya başka bir işlev ekleyebilirsiniz (adlı `canExecute` bağımsız değişkeni) döndüren `true` varsa `Button` etkinleştirilmelidir. Değişiklik işte `CommandDemoViewModel` Oluşturucusu:

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

Çağrıları `ChangeCanExecute` yöntemi `Command` gerekli şekilde `Command` yöntemi çağırabilir `canExecute` yöntemi ve belirlemek olmadığını `Button` veya devre dışı bırakılması gerekir. Bu kod değişikliğiyle sayı olarak sınırına ulaştığında `Button` devre dışı bırakıldı:

[![Temel düğmesi komutu değiştiren -](button-images/BasicButtonCommandModified.png "temel düğme komutunu - değişiklik")](button-images/BasicButtonCommandModified-Large.png#lightbox)

İki ya da daha fazla mümkündür `Button` aynı bağlı öğeleri `ICommand` özelliği. `Button` Öğeleri ayırt edilebilir kullanarak [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter) özelliği `Button`. Bu durumda, genel kullanmak isteyeceksiniz [ `Command<T>` ](xref:Xamarin.Forms.Command`1) sınıfı. `CommandParameter` Nesnesi bağımsız değişken olarak geçirilen ardından `execute` ve `canExecute` yöntemleri. Bu teknik ayrıntılı olarak gösterilen [ **temel komut vermeye genel** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) bölümünü [ **komut arabirimi** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) makalesi.

[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) örnek ayrıca, bu tekniği kullanır, `MainPage` sınıfı. **MainPage.xaml** dosyasını içeren bir `Button` örnek her sayfa için:

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

Her `Button` sahip kendi `Command` özelliğe adlı bir özellik `NavigateCommand`ve `CommandParameter` ayarlanmış bir [ `Type` ](xref:System.Type) projedeki page sınıflarınızı birine karşılık gelen nesne.

Olduğunu `NavigateCommand` özelliği türüdür `ICommand` ve arka plan kod dosyasında tanımlanır:

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

Oluşturucu başlatır `NavigateCommand` özelliğini bir `Command<Type>` çünkü nesne `Type` türü `CommandParameter` XAML dosyasında ayarlanan bir nesne. Diğer bir deyişle `execute` yöntemi olan bir bağımsız değişken türü `Type` karşılık gelen için `CommandParameter` nesne. İşlev sayfayı başlatır ve ardından ona gider.

Oluşturucu ayarlayarak sonucuna dikkat edin, `BindingContext` kendisine. Bu özellikleri bağlamak için XAML dosyasında gereklidir `NavigateCommand` özelliği.

## <a name="pressing-and-releasing-the-button"></a>Tuşuna basın ve düğmeyi serbest bırakma

Yanında `Clicked` olay `Button` ayrıca tanımlar [ `Pressed` ](xref:Xamarin.Forms.Button.Pressed) ve [ `Released` ](xref:Xamarin.Forms.Button.Released) olayları. `Pressed` Gerçekleştiğinde bir parmak bastığında bir `Button`, veya işaretçi üzerinden konumlanmış bir fare düğmesine basıldığında `Button`. `Released` Olay parmak veya fare düğmesi bırakıldığında gerçekleşir. Genellikle, bir `Clicked` olay aynı zamanda da tetiklenir `Released` parmak veya fare işaretçisi yüzeyine uzağa slaytlar, ancak olay `Button` yayımlanan önce `Clicked` olay yok oluşabilir.

`Pressed` Ve `Released` olaylar genellikle kullanılmaz, ancak özel amaçlar için gösterildiği şekilde kullanılabilmesi için **tuşuna basın ve yayın düğmesini** sayfası. XAML dosyası içeren bir `Label` ve `Button` işleyicileri için bağlı olan `Pressed` ve `Released` olayları:

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

Arka plan kod dosyası canlandırır `Label` olduğunda bir `Pressed` olay gerçekleşir, ancak rotasyonu askıya alır, bir `Released` olayı oluşur:

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

Sonuç `Label` bağlantılı bir parmak olmakla birlikte, yalnızca döndürür `Button`ve parmak serbest bırakıldığında durdurur:

[![Düğmeye basıp](button-images/PressAndReleaseButton.png "basın ve düğmeyi bırakın")](button-images/PressAndReleaseButton-Large.png)

Oyunlar için uygulama bu tür bir davranışı vardır: tutulan bir parmak bir `Button` belirli bir yönde hareket ettirme üzerinde bir ekran nesnesi çalışmasına neden olabilir.

<a name="button-appearance" />

## <a name="button-appearance"></a>Düğme görünümünü

`Button` Devralan veya görünümünü etkileyen çeşitli özellikleri tanımlar:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) rengi `Button` metin
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) Bu metin arka plan rengi
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) bir alanını çevreleyen renk `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) yazı tipi ailesini metni kullanılır
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) metin boyutu
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) metni Yatık veya kalın olup olmadığını gösterir
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) Kenarlık genişliği
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) köşelerini yuvarlar

> [!NOTE]
> `Button` Ayrıca sınıfında [ `Margin` ](xref:Xamarin.Forms.View.Margin) ve [ `Padding` ](xref:Xamarin.Forms.Button.Padding) düzen davranışını denetleyen özellikler `Button`. Daha fazla bilgi için [kenar boşlukları ve doldurma](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

Bu özelliklerin altı etkilerini (hariç `FontFamily` ve `FontAttributes`) içinde gösterilen **düğme görünümünü** sayfası. Başka bir özellik [ `Image` ](xref:Xamarin.Forms.Button.Image), bölümde ele alınmıştır [ **düğme bit eşlemler kullanarak**](#image-button).

Tüm görünümler ve veri bağlamaları **düğme görünümünü** sayfası XAML dosyasında tanımlanır:

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

`Button` Sayfanın en üstünde, üç sahip `Color` ilişkili özellikleri `Picker` sayfanın alt kısmındaki öğeleri. Öğeleri `Picker` öğeleridir renkleri `NamedColor` projeye dahil sınıfı. Üç `Slider` öğeleri içeren iki yönlü bağlamaları `FontSize`, `BorderWidth`, ve `CornerRadius` özelliklerini `Button`.

Bu program, bu özellikleri tüm birleşimlerle denemeler yapmak sağlar:

[![Düğme Görünüm](button-images/ButtonAppearance.png "görünüm düğmesi")](button-images/ButtonAppearance-Large.png)

Görmek için `Button` kenarlık ihtiyacınız ayarlamak bir `BorderColor` dışında bir şey `Default`ve `BorderWidth` pozitif bir değer.

İos'ta büyük kenarlık genişliklerini iç kısmını intrude fark edeceksiniz `Button` ve metin görünümünü müdahale. Bir iOS kenarlık kullanmayı seçerseniz `Button`, büyük olasılıkla başlamalı ve bitmelidir isteyebilirsiniz `Text` görünürlüğü korumak için alanları özelliğiyle.

UWP üzerinde seçerek bir `CornerRadius` yarı yüksekliğini aşıyor `Button` bir özel durum oluşturur.

## <a name="creating-a-toggle-button"></a>İki durumlu düğme oluşturma

Alt sınıfı için mümkündür `Button` açık-kapalı anahtar gibi çalışır, böylece: iki durumlu düğme ve yeniden kapalı geçiş dokunarak düğmesine bir kez dokunun.

Aşağıdaki `ToggleButton` sınıf türetilir `Button` ve adlı yeni bir olayı tanımlar `Toggled` ve adlı bir Boolean özelliği `IsToggled`. Xamarin.Forms tarafından tanımlanan aynı iki özellik şunlardır [ `Switch` ](xref:Xamarin.Forms.Switch):

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

`ToggleButton` Oluşturucusu için bir işleyici ekler `Clicked` BT'nin değerini değiştirebilmeniz için olay `IsToggled` özelliği. `OnIsToggledChanged` Yöntemi ateşlenir `Toggled` olay.

Son satırının `OnIsToggledChanged` statik metodu çağırır `VisualStateManager.GoToState` "ToggledOn" ve "ToggledOff" yöntemiyle iki metin dizeleri. Okuyabilirsiniz bu yöntem ve makaledeki görsel durumlar için uygulamanızın nasıl yanıt hakkında [ **Xamarin.Forms görsel durum Yöneticisi**](~/xamarin-forms/user-interface/visual-state-manager.md).

Çünkü `ToggleButton` çağrıda `VisualStateManager.GoToState`, sınıfın kendisi göre düğmenin görünümünü değiştirmek için herhangi bir ek özellikler içerir gerekmez, `IsToggled` durumu. Diğer bir deyişle barındıran XAML sorumluluğu `ToggleButton`.

**İki durumlu düğme tanıtım** sayfasını içeren iki örnek `ToggleButton`, ayarlar görsel durum Yöneticisi biçimlendirme dahil olmak üzere `Text`, `BackgroundColor`, ve `TextColor` visual durumuna bağlı düğmesi:

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

`Toggled` Olay işleyicileri arka plan kod dosyasında olan. Ayar için sorumlu oldukları `FontAttributes` özelliği `Label` düğmelerinin durumuna bağlı:

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

İOS, Android ve UWP üzerinde çalışan bir program şöyledir:

[![Değiştir düğmesi tanıtım](button-images/ToggleButtonDemo.png "Değiştir düğmesini Tanıtımı")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>Bit eşlemler düğmeleriyle kullanma

`Button` Sınıfı tanımlayan bir [ `Image` ](xref:Xamarin.Forms.Button.Image) bit eşlem görüntüsüne görüntülemek izin veren özellik `Button`, tek başına veya birlikte metin. Metin ve resim nasıl düzenlenir belirtebilirsiniz.

`Image` Özelliği türüdür [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), bit eşlemler olarak kaynakları tek tek bir platformu projelerinde ve .NET Standard kitaplığı projesinde olmayan depolanmalıdır anlamına gelir.

Xamarin.Forms tarafından desteklenen her platform görüntüleri uygulama çalışabilecek çeşitli cihazların farklı piksel çözünürlüğü için birden fazla boyut depolanması sağlar. Bu birden çok bit eşlemler adlı veya cihaz video için işletim sistemi en iyi eşleşmeyi seçebilir şekilde depolanan çözünürlüğü.

Bir bit eşlem için bir `Button`en iyi boyut genellikle 32 ve 64 CİHAZDAN bağımsız birimler arasında bağlı olarak ne kadar büyük olmasını istediğiniz. Bu örnekte kullanılan görüntülerin 48 CİHAZDAN bağımsız birimler boyutuna dayanır.

İOS projesinde **kaynakları** klasörü bu resmin üç boyut içerir:

- Olarak depolanan 48 piksellik bir kare bitmap **/Resources/MonkeyFace.png**
- Olarak depolanan bir 96 piksel kare bit eşlem **/Resource/MonkeyFace@2x.png**
- Olarak depolanan 144 piksellik bir kare bitmap **/Resource/MonkeyFace@3x.png**

Tüm üç bit eşlemler verilen bir **derleme eylemi** , **BundleResource**.

Bir Android projesi için tüm bit eşlemleri aynı ada sahip, ancak farklı alt klasörlerinde depolanırlar **kaynakları** klasörü:

- Olarak depolanan bir 72 piksel kare bit eşlem **/Resources/drawable-hdpi/MonkeyFace.png**
- Olarak depolanan bir 96 piksel kare bit eşlem **/Resources/drawable-xhdpi/MonkeyFace.png**
- Olarak depolanan 144 piksellik bir kare bitmap **/Resources/drawable-xxhdpi/MonkeyFace.png**
- Olarak depolanan 192 piksellik bir kare bitmap **/Resources/drawable-xxxhdpi/MonkeyFace.png**

Bunlar verilen bir **derleme eylemi** , **AndroidResource**.

UWP projede bit eşlemler herhangi bir projede depolanabilir, ancak genellikle özel bir klasörde depolanır veya **varlıklar** var olan bir klasörü. UWP projesi bu bit eşlemler içerir:

- Olarak depolanan 48 piksellik bir kare bitmap **/Assets/MonkeyFace.scale-100.png**
- Olarak depolanan bir 96 piksel kare bit eşlem **/Assets/MonkeyFace.scale-200.png**
- Olarak depolanan 192 piksellik bir kare bitmap **/Assets/MonkeyFace.scale-400.png**

Tüm verilen bir **derleme eylemi** , **içerik**.

Belirtebileceğiniz nasıl `Text` ve `Image` özellikleri düzenlenir `Button` kullanarak [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout) özelliği `Button`. Bu özellik türünde [ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout), katıştırılmış bir sınıf içinde `Button`. [Oluşturucusu](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) iki bağımsız değişkeni vardır:

- Üye [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) numaralandırma: `Left`, `Top`, `Right`, veya `Bottom` bit eşlem göre metnin nasıl görüntüleneceğini belirten.
- A `double` bit eşlem ve metin arasındaki aralığı için değer.

Varsayılanlar `Left` ve 10 birim. İki salt okunur özelliklerini `ButtonContentLayout` adlı [ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) ve [ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) bu özelliklerin değerlerini belirtin.

Kod içinde oluşturduğunuz bir `Button` ve `ContentLayout` özelliği şöyle:

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

XAML, yalnızca sabit listesi üyesi veya aralığı belirtmeniz gerekir veya herhangi bir sırada hem de virgülle ayırarak:

```xaml
<Button Text="button text"
        Image="image filename"
        ContentLayout="Right, 20" />
```

**Görüntü düğme tanıtım** sayfasında kullandığı `OnPlatform` iOS, Android ve UWP bit eşlem dosyaları farklı dosya adlarını belirtmek için. Tüm üç platformları için aynı adı kullanın ve kullanımını önlemek istiyorsanız `OnPlatform`, projenin kök dizininde UWP bit eşlemler depolamak gerekir.

İlk `Button` üzerinde **görüntü düğme tanıtım** Ayarlar sayfasında `Image` özelliği kullanmamayı `Text` özelliği:

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

UWP bit eşlemler, projenin kök dizininde depolanır, bu işaretleme oldukça basit hale getirilebilir:

```xaml
<Button Image="MonkeyFace.png" />
```

Tekrarlayan biçimlendirme içinde çok fazla önlemek için **ImageButtonDemo.xaml** dosya, örtük `Style` ayarlamak için de tanımlanan `Image` özelliği. Bu `Style` beş için diğer otomatik olarak uygulanan `Button` öğeleri. Tüm XAML dosyası aşağıda verilmiştir:

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

Son dört `Button` öğeleri olun kullanım `ContentLayout` özelliği bir konum ve metin ve bit eşlem aralığını belirtmek için:

[![Görüntü düğme tanıtım](button-images/ImageButtonDemo.png "görüntü düğme Tanıtımı")](button-images/ImageButtonDemo-Large.png#lightbox)

Artık işleyebilirsiniz çeşitli şekillerde gördünüz `Button` olaylar ve değişiklik `Button` görünümü.

## <a name="related-links"></a>İlgili bağlantılar

- [ButtonDemos örnek](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)
- [API düğmesi](xref:Xamarin.Forms.Button)
