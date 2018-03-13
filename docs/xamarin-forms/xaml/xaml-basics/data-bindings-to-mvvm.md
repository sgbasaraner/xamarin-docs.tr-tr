---
title: "Bölüm 5. Veri bağlamaları MVVM için"
description: "Model-View-ViewModel (MVVM) tasarım örüntüsü XAML ile göz önünde bulunmuştur. Üç yazılım katmanlar arasında ayrım düzeni zorlar — Görünüm; adlı XAML kullanıcı arabirimi Model adı verilen temel alınan verileri; ve görünüm modeli arasında bir aracı ViewModel çağrılır. Genellikle görünümü ve ViewModel XAML dosyasında tanımlanan veri bağlamaları üzerinden bağlanır. Görünüm için Bindingparameters'te ViewModel, genellikle bir örneğidir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 207bf7649d588f973b400cb452d9d8b246955cdb
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>Bölüm 5. Veri bağlamaları MVVM için

_Model-View-ViewModel (MVVM) tasarım örüntüsü XAML ile göz önünde bulunmuştur. Üç yazılım katmanlar arasında ayrım düzeni zorlar — Görünüm; adlı XAML kullanıcı arabirimi Model adı verilen temel alınan verileri; ve görünüm modeli arasında bir aracı ViewModel çağrılır. Genellikle görünümü ve ViewModel XAML dosyasında tanımlanan veri bağlamaları üzerinden bağlanır. Görünüm için Bindingparameters'te ViewModel, genellikle bir örneğidir._

## <a name="a-simple-viewmodel"></a>Basit bir ViewModel

ViewModels giriş, önce bir programı olmadan bir bakalım.
Daha önce diğer derlemelerden başvuru sınıflarda XAML dosyasına izin vermek için yeni bir XML ad alanı bildirimi tanımlamak için nasıl gördünüz. Burada bir XML ad alanı bildirimi için tanımlayan bir programdır `System` ad alanı:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Program kullanabilir `x:Static` geçerli tarih ve saat statik elde etmek için `DateTime.Now` özelliği ve kümesi `DateTime` değeri `BindingContext` üzerinde bir `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` çok özel bir özelliğidir: ayarladığınızda `BindingContext` bir öğede bu öğenin tüm gruplar tarafından devralınır. Tüm alt buna `StackLayout` bu aynı olan `BindingContext`, ve bu nesnenin özellikleri basit bağlamalar içerebilir.

İçinde **One-Shot DateTime** iki alt program içeren, özelliklerini bağlamalar `DateTime` değeri, ancak diğer iki alt öğe içeren bir bağlama yolu eksik gibi görünüyor bağlamalar. Bunun anlamı `DateTime` değerinin kendisi için kullanıldığından `StringFormat`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.OneShotDateTimePage"
             Title="One-Shot DateTime Page">

    <StackLayout BindingContext="{x:Static sys:DateTime.Now}"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">

        <Label Text="{Binding Year, StringFormat='The year is {0}'}" />
        <Label Text="{Binding StringFormat='The month is {0:MMMM}'}" />
        <Label Text="{Binding Day, StringFormat='The day is {0}'}" />
        <Label Text="{Binding StringFormat='The time is {0:T}'}" />

    </StackLayout>
</ContentPage>
```

Elbette, tarih ve saat zaman sayfa ilk derlendikten sonra kümesi ve hiçbir zaman değişiklik olduğunu büyük sorun şudur:

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "Tarih ve saat görüntüleme görünümü")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "tarih ve saat görüntüleme görünümü")

XAML dosyası her zaman geçerli saati gösteren bir saat görüntüleyebilir ancak yardımcı olması için bazı kod gerekiyor. MVVM, modeli ve ViewModel açısından düşünmeye olduğunda tamamen kod içinde yazılmış sınıfları. Görünüm genellikle veri bağlamaları ViewModel tanımlanan özellikler başvuruda bulunan bir XAML dosyasıdır.

Uygun bir Model ViewModel kullanmayan ve uygun ViewModel görünümünü kullanmayan. Ancak, çok sık Programcı belirli kullanıcı arabirimleri ile ilişkili veri türleri için ViewModel tarafından sunulan veri türleri uyarlar. Örneğin, bir Model 8 bit karakter ASCII dizeleri içeren bir veritabanı erişirse, bu kullanıcı arabiriminde Unicode özel kullanıma uygun hale getirmek için Unicode dizeleri dizeleri arasında dönüştürme ViewModel gerekir.

MVVM basit örnekleri (örneğin, bunlar burada gösterilen), genellikle yoktur hiçbir Model hiç ve yalnızca bir görünüm düzeni içerir ViewModel veri bağlamaları ile bağlantılı.

Yalnızca adlandırılan tek bir özelliğe sahip bir saat için bir ViewModel işte `DateTime`, ancak hangi güncelleştirmelerin `DateTime` özelliği her saniye:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    class ClockViewModel : INotifyPropertyChanged
    {
        DateTime dateTime;

        public event PropertyChangedEventHandler PropertyChanged;

        public ClockViewModel()
        {
            this.DateTime = DateTime.Now;

            Device.StartTimer(TimeSpan.FromSeconds(1), () =>
                {
                    this.DateTime = DateTime.Now;
                    return true;
                });
        }

        public DateTime DateTime
        {
            set
            {
                if (dateTime != value)
                {
                    dateTime = value;

                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this, new PropertyChangedEventArgs("DateTime"));
                    }
                }
            }
            get
            {
                return dateTime;
            }
        }
    }
}
```

ViewModels genellikle uygulama `INotifyPropertyChanged` sınıfı harekete anlamına gelir arabirimi bir `PropertyChanged` özelliklerinden biri değiştiğinde olay. Xamarin.Forms veri bağlama yönteminde işleyici için iliştirir `PropertyChanged` bir özelliği değiştiğinde bildirilmesi için olay ve canlı yeni değerle güncelleştirilmiş hedef.

Bu ViewModel üzerinde dayalı bir saat olarak kadar basit olabilir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ClockPage"
             Title="Clock Page">

    <Label Text="{Binding DateTime, StringFormat='{0:T}'}"
           FontSize="Large"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Label.BindingContext>
            <local:ClockViewModel />
        </Label.BindingContext>
    </Label>
</ContentPage>
```

Bildirim nasıl `ClockViewModel` ayarlanır `BindingContext` , `Label` özellik öğesi etiketleri kullanarak. Alternatif olarak, örneği `ClockViewModel` içinde bir `Resources` koleksiyonu ve ayarlamak `BindingContext` aracılığıyla bir `StaticResource` biçimlendirme uzantısı. Ya da arka plan kod dosyasına ViewModel örneğini oluşturabilirsiniz.

`Binding` Biçimlendirme uzantısı `Text` özelliği `Label` biçimleri `DateTime` özelliği. Görüntü şöyledir:

[![](data-bindings-to-mvvm-images/clock.png "Tarih ve saat ViewModel aracılığıyla görüntüleme görünümü")](data-bindings-to-mvvm-images/clock-large.png#lightbox "tarih ve saat ViewModel aracılığıyla görüntüleme görünümü")

Tek tek özelliklerine erişmek mümkündür `DateTime` özellikleri nokta ile ayırarak tarafından ViewModel özelliği:

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>Etkileşimli MVVM

MVVM genellikle bir temel alınan veri modelini temel etkileşimli bir görünüm için iki yönlü veri bağlaması ile kullanılır.

Adlı bir sınıf işte `HslViewModel` dönüştüren bir `Color` içine değer `Hue`, `Saturation`, ve `Luminosity` değerleri, bunun tam tersi:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    public class HslViewModel : INotifyPropertyChanged
    {
        double hue, saturation, luminosity;
        Color color;

        public event PropertyChangedEventHandler PropertyChanged;

        public double Hue
        {
            set
            {
                if (hue != value)
                {
                    hue = value;
                    OnPropertyChanged("Hue");
                    SetNewColor();
                }
            }
            get
            {
                return hue;
            }
        }

        public double Saturation
        {
            set
            {
                if (saturation != value)
                {
                    saturation = value;
                    OnPropertyChanged("Saturation");
                    SetNewColor();
                }
            }
            get
            {
                return saturation;
            }
        }

        public double Luminosity
        {
            set
            {
                if (luminosity != value)
                {
                    luminosity = value;
                    OnPropertyChanged("Luminosity");
                    SetNewColor();
                }
            }
            get
            {
                return luminosity;
            }
        }

        public Color Color
        {
            set
            {
                if (color != value)
                {
                    color = value;
                    OnPropertyChanged("Color");

                    Hue = value.Hue;
                    Saturation = value.Saturation;
                    Luminosity = value.Luminosity;
                }
            }
            get
            {
                return color;
            }
        }

        void SetNewColor()
        {
            Color = Color.FromHsla(Hue, Saturation, Luminosity);
        }

        protected virtual void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Değişikliklerini `Hue`, `Saturation`, ve `Luminosity` özellikleri neden `Color` değiştirmek için özellik ve değişiklikler `Color` değiştirmek diğer üç özellikleri neden olur. Sınıf çağırma değil dışında bu sonsuz bir döngüde gibi görünebilir `PropertyChanged` olay özelliği gerçekte değiştirilmediği sürece. Bu, aksi takdirde denetlenemeyen geri döngü bir son koyar.

Aşağıdaki XAML dosyasını içeren bir `BoxView` , `Color` özelliği bağlı `Color` ViewModel ve üç özellik `Slider` ve üç `Label` görünümleri bağlı `Hue`, `Saturation`ve `Luminosity` özellikleri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.HslColorScrollPage"
             Title="HSL Color Scroll Page">
    <ContentPage.BindingContext>
        <local:HslViewModel Color="Aqua" />
    </ContentPage.BindingContext>

    <StackLayout Padding="10, 0">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Hue, Mode=TwoWay}" />

        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Saturation, Mode=TwoWay}" />

        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Luminosity, Mode=TwoWay}" />
    </StackLayout>
</ContentPage>
```

Her bağlama `Label` varsayılan `OneWay`. Değerini görüntülemek yalnızca gerekir. Ancak her bağlama `Slider` olan `TwoWay`. Böylece `Slider` ViewModel başlatılacak. Dikkat `Color` özelliği ayarlanmış `Blue` ViewModel örneği olduğunda. Ancak bir değişiklik `Slider` yeni bir renk hesaplar ViewModel içinde özelliği için yeni bir değer ayarlamak de gerekir.

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "İki yönlü veri bağlamalar kullanılarak MVVM")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "iki yönlü veri bağlamalar kullanılarak MVVM")

## <a name="commanding-with-viewmodels"></a>ViewModels ile komut verme

Çoğu durumda, MVVM düzeni veri öğeleri işlenmesini sınırlandırılır: kullanıcı arabirimi nesneleri görünümünde ViewModel veri nesneleri paralel.

Ancak, bazen görünümü ViewModel çeşitli eylemleri tetikleyen düğmeleri içermesi gerekir. Ancak ViewModel içermemelidir `Clicked` düğmeleri için işleyiciler belirli kullanıcı arabirimi standardı için ViewModel tie çünkü.

Belirli kullanıcı arabirimi nesneleri daha bağımsız ancak hala ViewModel içinde çağrılacak yöntem izin vermek ViewModels izin vermek için bir *komutu* arabirimi bulunmaktadır. Bu komut arabirimi Xamarin.Forms aşağıdaki öğeleri tarafından desteklenir:

-  `Button`
-  `MenuItem`
-  `ToolbarItem`
-  `SearchBar`
-  `TextCell` (Bu nedenle de `ImageCell`)
-  `ListView`
-  `TapGestureRecognizer`

Dışında `SearchBar` ve `ListView` öğesi, iki özellik bu öğeleri tanımlayın:

-  `Command` türü  `System.Windows.Input.ICommand`
-  `CommandParameter` türü  `Object`

`SearchBar` Tanımlar `SearchCommand` ve `SearchCommandParameter` özelliklerini sırada `ListView` tanımlayan bir `RefreshCommand` türündeki özelliği `ICommand`.

`ICommand` İki yöntem ve bir olay arabirimi tanımlar:

-  `void Execute(object arg)`
-  `bool CanExecute(object arg)`
-  `event EventHandler CanExecuteChanged`

ViewModel türünün özelliklerini tanımlayabilirsiniz `ICommand`. Bu özellikler sonra bağlayabilirsiniz `Command` her özellik `Button` veya başka bir öğenin ya da bu arabirimi gerçekleştiren belki de özel bir görünüm. İsteğe bağlı olarak ayarlayabilirsiniz `CommandParameter` tek tek tanımlamak için özellik `Button` nesneler (veya diğer öğeleri) bu ViewModel özelliğine bağlıdır. Dahili olarak, `Button` çağrıları `Execute` kullanıcı dokunur her yöntemi `Button`, için geçen `Execute` yöntemi kendi `CommandParameter`.

`CanExecute` Yöntemi ve `CanExecuteChanged` olay durumları için kullanılan burada bir `Button` dokunun olabilir şu anda geçersiz durumda `Button` kendini devre dışı bırakmanız gerekir. `Button` Çağrıları `CanExecute` zaman `Command` özelliği ilk ayarlayın ve her `CanExecuteChanged` olay tetiklenir. Varsa `CanExecute` döndürür `false`, `Button` kendini devre dışı bırakır ve üretmez `Execute` çağrıları.

ViewModels kumanda ekleme konusunda yardım için Xamarin.Forms uygulaması iki sınıf tanımlar `ICommand`: `Command` ve `Command<T>` nerede `T` bağımsız değişken türü `Execute` ve `CanExecute`. Bu iki sınıf birkaç oluşturucuları tanımlama artı bir `ChangeCanExecute` ViewModel zorlamak için çağırabilirsiniz yöntemi `Command` yangın nesnesine `CanExecuteChanged` olay.

Telefon numarası girmek için tasarlanmıştır basit bir tuş için ViewModel aşağıdadır. Dikkat `Execute` ve `CanExecute` yöntemi lambda işlevlerinizi doğrudan oluşturucuda olarak tanımlanır:

```csharp
using System;
using System.ComponentModel;
using System.Windows.Input;
using Xamarin.Forms;

namespace XamlSamples
{
    class KeypadViewModel : INotifyPropertyChanged
    {
        string inputString = "";
        string displayText = "";
        char[] specialChars = { '*', '#' };

        public event PropertyChangedEventHandler PropertyChanged;

        // Constructor
        public KeypadViewModel()
        {
            AddCharCommand = new Command<string>((key) =>
                {
                    // Add the key to the input string.
                    InputString += key;
                });

            DeleteCharCommand = new Command(() =>
                {
                    // Strip a character from the input string.
                    InputString = InputString.Substring(0, InputString.Length - 1);
                },
                () =>
                {
                    // Return true if there's something to delete.
                    return InputString.Length > 0;
                });
        }

        // Public properties
        public string InputString
        {
            protected set
            {
                if (inputString != value)
                {
                    inputString = value;
                    OnPropertyChanged("InputString");
                    DisplayText = FormatText(inputString);

                    // Perhaps the delete button must be enabled/disabled.
                    ((Command)DeleteCharCommand).ChangeCanExecute();
                }
            }

            get { return inputString; }
        }

        public string DisplayText
        {
            protected set
            {
                if (displayText != value)
                {
                    displayText = value;
                    OnPropertyChanged("DisplayText");
                }
            }
            get { return displayText; }
        }

        // ICommand implementations
        public ICommand AddCharCommand { protected set; get; }

        public ICommand DeleteCharCommand { protected set; get; }

        string FormatText(string str)
        {
            bool hasNonNumbers = str.IndexOfAny(specialChars) != -1;
            string formatted = str;

            if (hasNonNumbers || str.Length < 4 || str.Length > 10)
            {
            }
            else if (str.Length < 8)
            {
                formatted = String.Format("{0}-{1}",
                                          str.Substring(0, 3),
                                          str.Substring(3));
            }
            else
            {
                formatted = String.Format("({0}) {1}-{2}",
                                          str.Substring(0, 3),
                                          str.Substring(3, 3),
                                          str.Substring(6));
            }
            return formatted;
        }

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Bu ViewModel varsayar `AddCharCommand` özelliği bağlı `Command` birçok düğme (veya başka bir şey komut arabirimi olan), her biri tarafından tanımlanır özelliği `CommandParameter`. Bu düğme karakterle ekleme bir `InputString` sonra için bir telefon numarası olarak biçimlendirilmiş özelliği `DisplayText` özelliği.

Ayrıca türünde ikinci bir özellik olan `ICommand` adlı `DeleteCharCommand`. Bu arka-spacing düğmeye bağlıdır, ancak silmek için herhangi bir karakter yoksa düğmesi devre dışı.

Aşağıdaki tuş bunu olması gerektiği gibi görsel olarak gelişmiş bir dosya değil. Bunun yerine, daha net bir şekilde komut arabirimi kullanımını göstermek için en az olarak işaretleme düşürüldü:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.KeypadPage"
             Title="Keypad Page">

    <Grid HorizontalOptions="Center"
          VerticalOptions="Center">
        <Grid.BindingContext>
            <local:KeypadViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
        </Grid.ColumnDefinitions>

        <!-- Internal Grid for top row of items -->
        <Grid Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>

            <Frame Grid.Column="0"
                   OutlineColor="Accent">
                <Label Text="{Binding DisplayText}" />
            </Frame>

            <Button Text="&#x21E6;"
                    Command="{Binding DeleteCharCommand}"
                    Grid.Column="1"
                    BorderWidth="0" />
        </Grid>

        <Button Text="1"
                Command="{Binding AddCharCommand}"
                CommandParameter="1"
                Grid.Row="1" Grid.Column="0" />

        <Button Text="2"
                Command="{Binding AddCharCommand}"
                CommandParameter="2"
                Grid.Row="1" Grid.Column="1" />

        <Button Text="3"
                Command="{Binding AddCharCommand}"
                CommandParameter="3"
                Grid.Row="1" Grid.Column="2" />

        <Button Text="4"
                Command="{Binding AddCharCommand}"
                CommandParameter="4"
                Grid.Row="2" Grid.Column="0" />

        <Button Text="5"
                Command="{Binding AddCharCommand}"
                CommandParameter="5"
                Grid.Row="2" Grid.Column="1" />

        <Button Text="6"
                Command="{Binding AddCharCommand}"
                CommandParameter="6"
                Grid.Row="2" Grid.Column="2" />

        <Button Text="7"
                Command="{Binding AddCharCommand}"
                CommandParameter="7"
                Grid.Row="3" Grid.Column="0" />

        <Button Text="8"
                Command="{Binding AddCharCommand}"
                CommandParameter="8"
                Grid.Row="3" Grid.Column="1" />

        <Button Text="9"
                Command="{Binding AddCharCommand}"
                CommandParameter="9"
                Grid.Row="3" Grid.Column="2" />

        <Button Text="*"
                Command="{Binding AddCharCommand}"
                CommandParameter="*"
                Grid.Row="4" Grid.Column="0" />

        <Button Text="0"
                Command="{Binding AddCharCommand}"
                CommandParameter="0"
                Grid.Row="4" Grid.Column="1" />

        <Button Text="#"
                Command="{Binding AddCharCommand}"
                CommandParameter="#"
                Grid.Row="4" Grid.Column="2" />
    </Grid>
</ContentPage>
```

`Command` Özelliği ilk `Button` bu görünen biçimlendirme bağlı `DeleteCharCommand`; rest bağlı `AddCharCommand` ile bir `CommandParameter` yani aynı görünür karakter `Button` yüz. Eylem program şöyledir:

[![](data-bindings-to-mvvm-images/keypad.png "MVVM ve komutlarını kullanarak hesaplayıcı")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "MVVM ve komutlarını kullanarak hesaplayıcısı")

### <a name="invoking-asynchronous-methods"></a>Zaman uyumsuz yöntemleri çağırma

Komutları, zaman uyumsuz yöntemler de çalıştırabilirsiniz. Bu kullanılarak elde edilir `async` ve `await` belirtirken anahtar sözcükleri `Execute` yöntemi:

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

Bu belirten `DownloadAsync` yöntemi bir `Task` ve beklemenin:

```csharp
async Task DownloadAsync ()
{
    await Task.Run (() => Download ());
}

void Download ()
{
    ...
}
```

## <a name="implementing-a-navigation-menu"></a>Gezinti Menüsü uygulama

[XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/) makalelerin bu serideki tüm kaynak kodunu içerir program kendi giriş sayfası için bir ViewModel kullanır. Bu ViewModel adlı üç özelliklerle kısa bir sınıf tanımıdır `Type`, `Title`, ve `Description` her örnek sayfalar, başlık ve kısa bir açıklama türünü içerir. Ayrıca, ViewModel adlı bir statik özellik tanımlar `All` koleksiyonu program tüm sayfaların:

```csharp
public class PageDataViewModel
{
    public PageDataViewModel(Type type, string title, string description)
    {
        Type = type;
        Title = title;
        Description = description;
    }

    public Type Type { private set; get; }

    public string Title { private set; get; }

    public string Description { private set; get; }

    static PageDataViewModel()
    {
        All = new List<PageDataViewModel>
        {
            // Part 1. Getting Started with XAML
            new PageDataViewModel(typeof(HelloXamlPage), "Hello, XAML",
                                  "Display a Label with many properties set"),

            new PageDataViewModel(typeof(XamlPlusCodePage), "XAML + Code",
                                  "Interact with a Slider and Button"),

            // Part 2. Essential XAML Syntax
            new PageDataViewModel(typeof(GridDemoPage), "Grid Demo",
                                  "Explore XAML syntax with the Grid"),

            new PageDataViewModel(typeof(AbsoluteDemoPage), "Absolute Demo",
                                  "Explore XAML syntax with AbsoluteLayout"),

            // Part 3. XAML Markup Extensions
            new PageDataViewModel(typeof(SharedResourcesPage), "Shared Resources",
                                  "Using resource dictionaries to share resources"),

            new PageDataViewModel(typeof(StaticConstantsPage), "Static Constants",
                                  "Using the x:Static markup extensions"),

            new PageDataViewModel(typeof(RelativeLayoutPage), "Relative Layout",
                                  "Explore XAML markup extensions"),

            // Part 4. Data Binding Basics
            new PageDataViewModel(typeof(SliderBindingsPage), "Slider Bindings",
                                  "Bind properties of two views on the page"),

            new PageDataViewModel(typeof(SliderTransformsPage), "Slider Transforms",
                                  "Use Sliders with reverse bindings"),

            new PageDataViewModel(typeof(ListViewDemoPage), "ListView Demo",
                                  "Use a ListView with data bindings"),

            // Part 5. From Data Bindings to MVVM
            new PageDataViewModel(typeof(OneShotDateTimePage), "One-Shot DateTime",
                                  "Obtain the current DateTime and display it"),

            new PageDataViewModel(typeof(ClockPage), "Clock",
                                  "Dynamically display the current time"),

            new PageDataViewModel(typeof(HslColorScrollPage), "HSL Color Scroll",
                                  "Use a view model to select HSL colors"),

            new PageDataViewModel(typeof(KeypadPage), "Keypad",
                                  "Use a view model for numeric keypad logic")
        };
    }

    public static IList<PageDataViewModel> All { private set; get; } 
}
```

XAML dosyası için `MainPage` tanımlayan bir `ListBox` , `ItemsSource` özelliği için ayarlanmış `All` özelliği ve içeren bir `TextCell` görüntüleme için `Title` ve `Description` her sayfanın özelliklerini:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage"
             Padding="5, 0"
             Title="XAML Samples">

    <ListView ItemsSource="{x:Static local:PageDataViewModel.All}"
              ItemSelected="OnListViewItemSelected">
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding Title}"
                          Detail="{Binding Description}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Sayfaları kaydırılabilir bir listede gösterilir:

[![](data-bindings-to-mvvm-images/mainpage.png "Sayfaları kaydırılabilir listesi")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "sayfaları kaydırılabilir listesi")

Kullanıcı bir öğeyi seçtiğinde arka plan kod dosyasına işleyici tetiklenir. İşleyici kümeleri `SelectedItem` özelliği `ListBox` geri `null` ve ardından seçili sayfa oluşturur ve ona götürür:

```csharp
private async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
{
    (sender as ListView).SelectedItem = null;

    if (args.SelectedItem != null)
    {
        PageDataViewModel pageData = args.SelectedItem as PageDataViewModel;
        Page page = (Page)Activator.CreateInstance(pageData.Type);
        await Navigation.PushAsync(page);
    }
}
```

## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/DYRLcqG2BAY]

**Xamarin 2016 gelişmesi: Xamarin.Forms ve Prizma ile basit yapılan MVVM**

## <a name="summary"></a>Özet

XAML Xamarin.Forms uygulamalar, özellikle veri bağlaması olduğunda kullanıcı arabirimlerini tanımlamaya yönelik güçlü bir araçtır ve MVVM kullanılır. Bir kullanıcı arabirimi kodu tüm arka plan desteğiyle temiz, zarif ve potansiyel olarak oluşturulabildiğinden gösterimini sonucudur.


## <a name="related-links"></a>İlgili bağlantılar

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Bölüm 1. XAML Kullanmaya Başlarken](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Bölüm 2. Temel XAML Sözdizimi](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Bölüm 3. XAML Biçimlendirme Uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Bölüm 4. Temel Veri Bağlama Bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
