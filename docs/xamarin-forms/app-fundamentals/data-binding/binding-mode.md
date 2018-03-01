---
title: "Bağlama modu"
description: "Kaynak ve hedef arasındaki bilgi akışını denetleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: e85bc98b5da4c6e529f150c6e7945a8556e2c60f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-mode"></a>Bağlama modu

İçinde [önceki makalede](basic-bindings.md), **alternatif kod bağlama** ve **alternatif XAML bağlama** öne çıkan sayfaları bir `Label` ile kendi `Scale` özelliği bağlı `Value` özelliği bir `Slider`. Çünkü `Slider` ilk değeri 0 ise, bunun nedeni `Scale` özelliği `Label` 1 yerine 0 olarak ayarlanması için ve `Label` kayboldu.

İçinde [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) örnek, **ters bağlama** sayfa benzer programlar önceki makalede, veri bağlama üzerinde tanımlandıancakbu`Slider` yerine `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label" 
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

İlk başta, bu geriye doğru görünebilir: artık `Label` veri bağlama kaynağı ve `Slider` hedefidir. Bağlama başvuruları `Opacity` özelliği `Label`, 1 varsayılan değerine sahip.

Beklediğiniz gibi `Slider` 1 değerine Başlangıç Aşamasında durumundan başlatılmadan `Opacity` değerini `Label`. Bu, iOS ekran görüntüsü soldaki gösterilir:

[![Bağlama ters](binding-mode-images/reversebinding-small.png "bağlama ters")](binding-mode-images/reversebinding-large.png "bağlama ters çevir")

Ancak, olabilir, surprised `Slider` Android ve UWP ekran görüntüleri göstermek gibi çalışmaya devam eder. Bu veri bağlama daha iyi ne zaman çalıştığını önermek görünüyor `Slider` bağlama hedefi yerine `Label` bekliyoruz gibi başlatma çalıştığından.

Arasındaki farkı **ters bağlama** örnek ve önceki örnekleri içerir *bağlama modu*.

## <a name="the-default-binding-mode"></a>Varsayılan bağlama modu

Bağlama modu üyesi ile belirtilen [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) numaralandırma: 

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) 
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) &ndash; Kaynak ve hedef arasında çift yönlü veri gider
- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) &ndash; veri kaynağından hedefe gider.
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; veri kaynağına hedeften gider

Her bağlanabilirse bağlanabilir özelliği oluşturulduğunda ayarlanır modu bağlama varsayılan ve kullanılabilir olduğu özelliğinin [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) özelliği `BindableProperty` nesnesi. Bu özellik veri bağlama hedef olduğunda bu varsayılan bağlama modu modu etkin gösterir.

Çoğu özellikleri gibi varsayılan bağlama modunu `Rotation`, `Scale`, ve `Opacity` olan `OneWay`. Bu özellikler veri bağlama hedefleri olduğunda, hedef özelliği kaynaktan ayarlanır.

Ancak, varsayılan bağlama modunu `Value` özelliği `Slider` olan `TwoWay`. Yani `Value` özellik, bir veri bağlama hedef sonra hedef kaynak (her zamanki gibi) ayarlanır ancak kaynak hedef de ayarlayın. Nelere izin budur `Slider` Başlangıç Aşamasında durumundan ayarlanacak `Opacity` değeri.

Bu iki yönlü bağlama sonsuz bir döngüde oluşturmak için görünebilir, ancak bu gerçekleşmez. Özellik gerçekte değiştirilmediği sürece bağlanabilir özellikleri özellik değişikliği sinyal. Bu, sonsuz bir döngüde engeller.

### <a name="two-way-bindings"></a>İki yönlü bağlamaları

En bağlanabilirse özelliklere sahip bir varsayılan bağlama modu `OneWay` ancak varsayılan bağlama modu aşağıdaki özelliklere sahip `TwoWay`:

- `Date` özelliği `DatePicker`
- `Text` özelliği `Editor`, `Entry`, `SearchBar`, ve `EntryCell`
- `IsRefreshing` özelliği `ListView`
- `SelectedItem` özelliği `MultiPage`
- `SelectedIndex` ve `SelectedItem` özellikleri `Picker`
- `Value` özelliği `Slider` ve `Stepper`
- `IsToggled` özelliği `Switch` 
- `On` özelliği `SwitchCell`
- `Time` özelliği `TimePicker`

Bu belirli özellikleri olarak tanımlanan `TwoWay` çok iyi bir nedenle: 

Veri bağlamaları Model View ViewModel (MVVM) uygulama mimarisi ile kullanıldığında, ViewModel bağlama veri kaynağı ve görünümler gibi oluşuyorsa görünümü sınıftır `Slider`, veri bağlama hedeflerine. MVVM bağlamaları benzer **ters bağlama** örnek önceki örnekte bağlamaları'den fazla. Her görünüm ViewModel karşılık gelen özellik değeriyle başlatılması için sayfadaki istiyor, ancak görünümünde değişiklikleri de ViewModel özelliği etkiler olasılığı yüksektir.

Varsayılan bağlama modları özelliklerle `TwoWay` MVVM senaryolarda kullanılması büyük olasılıkla bu özellikleri.

### <a name="one-way-to-source-bindings"></a>Bir-Way için-kaynak bağlamaları

Salt okunur bağlanabilirse özelliklere sahip bir varsayılan bağlama modu `OneWayToSource`. Bir varsayılan bağlama modu olan yalnızca bir okuma/yazma bağlanabilirse özelliği `OneWayToSource`:

- `SelectedItem` özelliği `ListView`

Bağlamada olan stratejinin `SelectedItem` özelliği bağlama kaynağı ayarı neden. Bu makalenin sonraki bölümlerinde örnek bu davranışı geçersiz kılar.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModels ve özellik değişikliği bildirimleri

**Basit Renk Seçici** sayfa basit ViewModel kullanımını gösterir. Veri bağlamaları kullanıcının üç kullanarak bir renk seçmesine izin ver `Slider` ton, Doygunluk ve parlaklığını için öğeleri.

ViewModel veri bağlama kaynağıdır. ViewModel mu *değil* bağlanabilir özelliklerini tanımlamak, ancak bir özellik değeri değiştiğinde bildirim almak bağlama altyapısı izin veren bir bildirim mekanizması uygulayın. Bu bildirim mekanizmasıdır [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) adlı tek bir özelliğini tanımlar arabirimi [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/). Genellikle bu arabirimi uygulayan bir sınıf ortak özelliklerinden biri değeri değiştiğinde olay gönderir. Olay özellik hiçbir zaman değişirse harekete gerekmez. ( `INotifyPropertyChanged` Arabirimi tarafından uygulanan de `BindableObject` ve `PropertyChanged` bağlanabilir bir özellik değeri değiştiğinde olay tetiklenir.)

`HslColorViewModel` Sınıfı tanımlayan beş özellikleri: `Hue`, `Saturation`, `Luminosity`, ve `Color` özellikleri ilişkilidir. Değişiklikleri değeri bileşenleri üç herhangi biri renk zaman `Color` özelliği yeniden hesaplanması, ve `PropertyChanged` olaylar için tüm dört özellikleri:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name; 

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

Zaman `Color` özelliği değişiklikleri, statik `GetNearestColorName` yönteminde `NamedColor` sınıfı (de dahil **DataBindingDemos** çözüm) en yakın adlandırılmış rengi alır ve ayarlar `Name` özelliği. Bu `Name` özelliğine sahip bir özel `set` sınıfın dışından ndan ayarlanamıyor şekilde erişimcisi.

Bir ViewModel bağlama kaynağı olarak ayarlandığında, bir işleyici bağlama altyapısı iliştirir `PropertyChanged` olay. Bu şekilde bağlama özelliklerine yapılan değişiklikler, bildirim ve ardından hedef özelliklerini değiştirilen değerleri ayarlayabilirsiniz.

**Basit Renk Seçici** XAML dosyası başlatır `HslColorViewModel` sayfanın kaynak sözlüğü ve başlatır `Color` özelliği. `BindingContext` Özelliği `Grid` ayarlanmış bir `StaticResource` bu kaynağa başvuran uzantısı bağlama:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel" 
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
        
    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />
    
            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

`BoxView`, `Label`Ve üç `Slider` görünümleri devral bağlama bağlamı `Grid`. Bu kaynak ViewModel özelliklerinde başvuru tüm bağlama hedefleri görünümlerdir. İçin `Color` özelliği `BoxView`ve `Text` özelliği `Label`, veri bağlamaları olan `OneWay`: özellikler görünümünde ViewModel özelliklerinden ayarlanır.

`Value` Özelliği `Slider`, ancak `TwoWay`. Her böylece `Slider` ViewModel ve ayrıca her birinden ayarlanacak ViewModel için ayarlanacak `Slider`. 

Programı ilk kez çalıştırdığınızda, `BoxView`, `Label`ve üç `Slider` öğeleridir ilk dayalı ViewModel tüm kümesinden `Color` özelliği ViewModel örneğinin başlatılmasından ayarlanır. Bu, iOS ekran görüntüsü soldaki gösterilir:

[![Basit Renk Seçici](binding-mode-images/simplecolorselector-small.png "basit Renk Seçici")](binding-mode-images/simplecolorselector-large.png "basit Renk Seçici")

Kaydırıcılar işlemek gibi `BoxView` ve `Label` Android ve UWP ekran görüntüleri ile gösterildiği gibi buna göre güncelleştirilir.

Kaynak sözlüğünde ViewModel başlatmasını bir ortak bir yaklaşımdır. Özellik öğesi etiketleri içindeki ViewModel örneği oluşturmak mümkündür `BindingContext` özelliği. İçinde **basit Renk Seçici** XAML dosyası, kaldırmayı deneyin `HslColorViewModel` kaynak sözlüğünden ve ayarlamak `BindingContext` özelliği `Grid` şöyle:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>
        
    ···

</Grid>
```

Bağlama bağlamı çeşitli şekillerde ayarlayabilirsiniz. Bazı durumlarda, arka plan kod dosyasına ViewModel oluşturur ve ayarlar `BindingContext` sayfasının özelliği. Tüm geçerli yaklaşımlar bunlar.

## <a name="overriding-the-binding-mode"></a>Bağlama modu geçersiz kılma

Target özelliği varsayılan bağlama modunu belirli veri bağlama için uygun değilse, ayarlayarak geçersiz kılmanıza olanak [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) özelliği `Binding` (veya [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Mode/) özelliği `Binding` biçimlendirme uzantısı) üyelerini birine `BindingMode` numaralandırması.

Ancak, ayarı `Mode` özelliğine `TwoWay` beklediğiniz gibi her zaman çalışmıyor. Örneğin, değiştirmeyi deneyin **alternatif XAML bağlama** XAML dosyasını içerecek şekilde `TwoWay` bağlama tanımında:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Beklenen, `Slider` ilk değerine başlatılmış `Scale` 1 olduğu, ancak değil gerçekleşen özelliği. Zaman bir `TwoWay` bağlama başlatılır, hedef kaynak önce; yani ayarlanır `Scale` özelliği ayarlanmış `Slider` varsayılan değer 0. Zaman `TwoWay` bağlama ayarlanmış `Slider`, sonra `Slider` başlangıçta kaynak ayarlanır.

Bağlama modu ayarlamak `OneWayToSource` içinde **alternatif XAML bağlama** örnek:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Şimdi `Slider` 1 olarak başlatıldı (varsayılan değeri `Scale`) ancak bulmada `Slider` etkilemez `Scale` özelliği, bu çok kullanışlı değildir.

Varsayılan bağlama moduyla geçersiz kılma, çok kullanışlı bir uygulama `TwoWay` içerir `SelectedItem` özelliği `ListView`. Varsayılan bağlama modu `OneWayToSource`. Üzerinde veri bağlama ayarlandığında `SelectedItem` bir ViewModel kaynağı özelliğinde bu kaynak özelliğini ayarlayın daha sonra başvurmak için özellik `ListView` seçim. Ancak, bazı durumlarda, ayrıca isteyebilirsiniz `ListView` ViewModel başlatılacak.

**Örnek ayarlarını** sayfası bu tekniği gösterir. Bu sayfa bu gibi bir ViewModel çok sık tanımlanan uygulama ayarları, basit bir uygulamasını temsil eder `SampleSettingsViewModel` dosyası:

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Her uygulama ayarı adlı bir yöntem Xamarin.Forms özellikleri sözlükte için kaydedilmiş bir özelliktir `SaveState` ve oluşturucusunda bu sözlükten yüklendi. Sınıf altında ViewModels kolaylaştırmak ve hata potansiyeli daha az olun yardımcı iki yöntem vardır. `OnPropertyChanged` Yöntemi altındaki arama özelliği ayarlanmış isteğe bağlı bir parametre vardır. Bu özelliğin adı bir dize olarak belirtirken yazım hatalarını önler. 

`SetProperty` Sınıfında yöntemi daha da yapar: özelliğine bir alan olarak depolanan değerle ayarlanır değeri karşılaştırır ve yalnızca çağırır `OnPropertyChanged` zaman iki değer olmayan eşit. 

`SampleSettingsViewModel` Sınıfı tanımlayan iki özellik için arka plan rengi: `BackgroundNamedColor` özelliği türüdür `NamedColor`, hangi bir sınıf de dahil **DataBindingDemos** çözüm. `BackgroundColor` Özelliği türüdür `Color`ve gelen elde `Color` özelliği `NamedColor` nesnesi.

`NamedColor` Sınıfı, tüm statik genel alanlarında Xamarin.Forms numaralandırılacak .NET yansıma kullanan `Color` yapısını ve statik erişilebilir bir koleksiyondaki adları ile depolamaya `All` özelliği:

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                                (int)(255 * color.R),
                                                (int)(255 * color.G),
                                                (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

`App` Sınıfını **DataBindingDemos** proje tanımlar adlı bir özellik `Settings` türü `SampleSettingsViewModel`. Bu özellik başlatılan zaman `App` sınıf örneği ve `SaveState` yöntemi çağrılır `OnSleep` yöntemi çağrılır:

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

Uygulama yaşam döngüsü yöntemleri hakkında daha fazla bilgi için bkz: [ **uygulama yaşam döngüsü**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Neredeyse her şeyi başka içinde işlenir **SampleSettingsPage.xaml** dosya. `BindingContext` Sayfasında kullanılarak ayarlanan bir `Binding` biçimlendirme uzantısı: statik bağlama kaynağıdır `Application.Current` örnek özelliği, `App` projesinde, sınıf ve `Path` ayarlanır `Settings` olan özelliği, `SampleSettingsViewModel` nesnesi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Sayfanın tüm alt öğeleri bağlama bağlamını devralır. Bu sayfadaki diğer bağlamaları çoğu özelliklerinde için `SampleSettingsViewModel`. `BackgroundColor` Özelliği ayarlamak için kullanılan `BackgroundColor` özelliği `StackLayout`ve `Entry`, `DatePicker`, `Switch`, ve `Stepper` özellikleri tüm bağlı ViewModel diğer özellikleri.

`ItemsSource` Özelliği `ListView` statik olarak ayarlamanız `NamedColor.All` özelliği. Bu doldurur `ListView` tüm `NamedColor` örnekleri. Her öğe için `ListView`, bağlama bağlamı öğesi için ayarlanmış bir `NamedColor` nesnesi. `BoxView` Ve `Label` içinde `ViewCell` özelliklerinde bağlı `NamedColor`.

`SelectedItem` Özelliği `ListView` türü `NamedColor`ve bağlı `BackgroundNamedColor` özelliği `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Varsayılan bağlama modunu `SelectedItem` olan `OneWayToSource`, seçili öğesini ViewModel özelliğini ayarlar. `TwoWay` Modu sağlar `SelectedItem` ViewModel başlatılacak. 

Ancak, ne zaman `SelectedItem` bu şekilde ayarlanması `ListView` otomatik olarak seçilen öğeyi göstermek için Kaymaz. Arka plan kod dosyasına biraz kod gereklidir:

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem, 
                                   ScrollToPosition.MakeVisible, 
                                   false);
        }
    }
}
``` 

İlk kez çalıştırdığınızda soldaki iOS ekran program gösterir. Oluşturucuda `SampleSettingsViewModel` başlatır arka plan rengi beyaz ve içinde seçili olan `ListView`:

[![Örnek ayarlar](binding-mode-images/samplesettings-small.png "örnek ayarlar")](binding-mode-images/samplesettings-large.png "örnek ayarlar")

Diğer iki ekran görüntüleri değiştirilmiş ayarlarını göster. Bu sayfayla denemeler yaparken programın uyku veya aygıt ya da çalıştırıldığı öykünücüsü sonlandırmak için put unutmayın. Visual Studio hata ayıklayıcısı programdan sonlandırma değil neden olacak `OnSleep` geçersiz kılması `App` çağrılacak sınıfı.

Sonraki makalede nasıl belirleneceğini görürsünüz [ **biçimlendirme dizesi** ](string-formatting.md) ayarlandığı veri bağlamalarının `Text` özelliği `Label`.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama gösterileri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
