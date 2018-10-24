---
title: Xamarin.Forms bağlama modu
description: Bu makalede, kaynak ve hedef BindingMode numaralandırma üyesi ile belirtilen bir bağlama modu kullanılarak arasındaki bilgi akışını denetlemek nasıl açıklar. Bu özellik bir veri bağlama hedeflendiğinde gösteren modu geçerli bir varsayılan bağlama modu her bağlanılabilir özellik vardır.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: 420c1de0691de419180dd497a9031ea5e7dd1054
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "37935673"
---
# <a name="xamarinforms-binding-mode"></a>Xamarin.Forms bağlama modu

İçinde [önceki makalede](basic-bindings.md), **alternatif bağlama kodunu** ve **XAML alternatif bağlama** öne çıkan sayfalar bir `Label` ile kendi `Scale` özelliği bağlı `Value` özelliği bir `Slider`. Çünkü `Slider` ilk değer 0 ise, bunun nedeni `Scale` özelliği `Label` 1 yerine 0 olarak ayarlanması ve `Label` kayboldu.

İçinde [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) örneği **ters bağlama** sayfa benzer programlar önceki makalede dışında veri bağlama üzerinde tanımlanır`Slider` yerine `Label`:

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

İlk başta, bu geriye doğru görünebilir: artık `Label` veri bağlama kaynağı ve `Slider` hedefidir. Bağlama başvurularının `Opacity` özelliği `Label`, 1 varsayılan değerine sahip.

Bekleyebileceğiniz gibi `Slider` değeri 1 ilk başlatılır `Opacity` değerini `Label`. Bu, sol taraftaki iOS ekran görüntüsünde gösterilmiştir:

[![Bağlama ters](binding-mode-images/reversebinding-small.png "bağlama ters")](binding-mode-images/reversebinding-large.png#lightbox "bağlama ters çevir")

Ancak, olabilir, surprised `Slider` Android ve UWP ekran görüntüleri göz atarak çalışmaya devam eder. Veri bağlama daha iyi ne zaman çalıştığını önermek için gibi görünüyor `Slider` bağlama hedefi yerine `Label` beklediğimiz gibi başlatma çalıştığından.

Arasındaki fark **ters bağlama** örnek ve önceki örnekleri içerir *bağlama modu*.

## <a name="the-default-binding-mode"></a>Varsayılan bağlama modu

Bağlama modu üyesi ile belirtilen [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) sabit listesi:

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) &ndash; Kaynak ve hedef arasında iki yönlü olarak veri gelişir
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) &ndash; verileri kaynaktan hedefe gider.
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; veri kaynağına hedefinden gider.
- [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; veri kaynağından hedef, ancak yalnızca giden `BindingContext` değişiklikleri (Xamarin.Forms 3.0 ile yeni)

Varsayılan bağlanılabilir özellik oluşturulduğunda ayarlanır modu bağlama ve kullanılabilir olduğu her bağlanılabilir özellik sahip [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) özelliği `BindableProperty` nesne. Bu özellik bir veri bağlama hedefi olduğunda bu varsayılan bağlama modu modu etkin gösterir.

Varsayılan bağlama modu gibi özelliklerin çoğu için `Rotation`, `Scale`, ve `Opacity` olduğu `OneWay`. Bu özelliklere veri bağlama hedefleri olduğunda hedef özelliği kaynak ayarlayın.

Bununla birlikte, varsayılan bağlama modu `Value` özelliği `Slider` olduğu `TwoWay`. Yani `Value` özelliği olan bir veri bağlama hedefi sonra hedef kaynaktan (her zaman olduğu gibi) ayarlanır ancak kaynak de hedeften ayarlanır. Bu ne sağlar, `Slider` ilk ayarlanacak `Opacity` değeri.

Sonsuz bir döngü oluşturmak için bu iki yönlü bir bağlama görünebilir, ancak bu gerçekleşmez. Bağlanabilir özellikler özelliği gerçekten değiştirilmediği sürece, bir özellik değişiminin sinyal. Bu, sonsuz bir döngüye engeller.

### <a name="two-way-bindings"></a>İki yönlü bağlamaları

En bağlanabilir özelliklere sahip bir varsayılan bağlama modu `OneWay` ancak varsayılan bağlama modu şu özelliklere sahip `TwoWay`:

- `Date` özelliği `DatePicker`
- `Text` özelliği `Editor`, `Entry`, `SearchBar`, ve `EntryCell`
- `IsRefreshing` özelliği `ListView`
- `SelectedItem` özelliği `MultiPage`
- `SelectedIndex` ve `SelectedItem` özellikleri `Picker`
- `Value` özelliği `Slider` ve `Stepper`
- `IsToggled` özelliği `Switch`
- `On` özelliği `SwitchCell`
- `Time` özelliği `TimePicker`

Bu belirli özellikleri olarak tanımlanan `TwoWay` çok iyi nedeni:

Veri bağlamaları ile Model-View-ViewModel (MVVM) uygulama mimarisi kullanıldığında, veri bağlama kaynağı ve görünümler gibi oluşan görünümü ViewModel sınıfı olan `Slider`, veri bağlama hedeflerdir. MVVM bağlamaları benzer **ters bağlama** önceki örneklerdeki bağlamaları'den daha fazla örnek. Büyük olasılıkla her görünümü ViewModel içinde karşılık gelen özellik değeriyle başlatılması için sayfasında istediğiniz, ancak değişiklikler görünümünde da ViewModel özelliğini etkiler.

Varsayılan bağlama modları özelliklerle `TwoWay` MVVM senaryolarda kullanılması büyük olasılıkla bu özellikleri.

### <a name="one-way-to-source-bindings"></a>Bir-Way-için-Source bağlamaları

Salt okunur bağlanabilir özelliklere sahip bir varsayılan bağlama modu `OneWayToSource`. Varsayılan bağlama modu içeren tek bir okuma/yazma bağlanabilir özelliği `OneWayToSource`:

- `SelectedItem` özelliği `ListView`

Üzerindeki bir bağlamaya olan stratejinin `SelectedItem` özelliği bağlama kaynağı ayarı neden. Bu makaledeki örnek bu davranışı geçersiz kılar.

### <a name="one-time-bindings"></a>Tek seferlik bağlamaları

Varsayılan bağlama modu çeşitli özelliklere sahip `OneTime`. Bunlar:

- `IsTextPredictionEnabled` özelliği `Entry`
- `Text`, `BackgroundColor`, ve `Style` özelliklerini `Span`.

Hedef bir bağlama modu özelliklerle `OneTime` bağlama bağlamı değiştiğinde güncelleştirilir. Kaynak Özellikleri'nde değişiklikleri izlemek gerekli olduğundan bu hedef özellikleri bağlamalarda Bu bağlama altyapısı kolaylaştırır.

## <a name="viewmodels-and-property-change-notifications"></a>Viewmodel'lar ve özellik değişikliği bildirimleri

**Basit Renk Seçici** sayfa basit ViewModel kullanımını gösterir. Veri bağlamaları sonrasında kullanıcı üç kullanarak bir renk seçin `Slider` ton, Doygunluk ve parlaklık için öğeleri.

ViewModel veri bağlama kaynağıdır. ViewModel mu *değil* bağlanabilir özellikler tanımlayın, ancak bir özelliğinin değeri değiştiğinde bildirim almak bağlama altyapısı sağlayan bir bildirim mekanizması uygulayın. Bu bildirim mekanizması [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) adlı tek bir özellik tanımlayan arabirimi [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged). Genellikle bu arabirimi uygulayan bir sınıf, ortak özelliklerinden birini değeri değiştiğinde bir olayı tetikler. Olay özellik hiçbir zaman değişirse harekete gerekmez. ( `INotifyPropertyChanged` Arabirim tarafından uygulandığında da `BindableObject` ve `PropertyChanged` bağlanılabilir özellik değeri değiştiğinde olay harekete geçirilir.)

`HslColorViewModel` Sınıfı beş özellikleri tanımlar: `Hue`, `Saturation`, `Luminosity`, ve `Color` özellikleri ilişkilidir. Değişiklikleri değeri bileşenleri üç herhangi biri renk olduğunda `Color` özelliği yeniden hesaplanır ve `PropertyChanged` olaylar için tüm dört özellik:

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

Zaman `Color` özellik değişiklikleri, statik `GetNearestColorName` yönteminde `NamedColor` sınıfı (de dahil **DataBindingDemos** çözüm) en yakın adlandırılmış rengi alır ve ayarlar `Name` özelliği. Bu `Name` özelliğine sahip bir özel `set` sınıfın dışından gelen ayarlanamıyor şekilde erişimcisi.

Bir ViewModel bağlama kaynağı olarak ayarlandığında, bağlama altyapısı için bir işleyici ekler `PropertyChanged` olay. Bu şekilde, bağlama özelliklerde yapılan değişiklikler, bildirim ve ardından hedef özelliklerini değiştirilmiş değerleri ayarlayabilirsiniz.

Ancak, bir hedef özellik (veya `Binding` bir hedef özellik tanımını) sahip bir `BindingMode` , `OneTime`, üzerinde bir işleyici eklemek bağlama altyapısı için gerekli değildir `PropertyChanged` olay. Hedef özelliği güncelleştirildiğinde yalnızca `BindingContext` değişiklikleri ve kaynak özelliği değiştiğinde değil.

**Basit Renk Seçici** XAML dosyası başlatır `HslColorViewModel` sayfanın kaynak sözlüğü ve başlatır `Color` özelliği. `BindingContext` Özelliği `Grid` ayarlanmış bir `StaticResource` uzantısı, kaynak başvurmak için bağlama:

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

`BoxView`, `Label`Ve üç `Slider` görünümleri devral bağlama bağlamdan `Grid`. Bu kaynak özelliklerinde ViewModel başvuran tüm bağlama hedefleri görünümleridir. İçin `Color` özelliği `BoxView`ve `Text` özelliği `Label`, veri bağlamaları `OneWay`: ViewModel Özellikleri'nden görünümünde özelliklerini ayarlayın.

`Value` Özelliği `Slider`, ancak `TwoWay`. Böylece her `Slider` ViewModel ve ayrıca her birinden ayarlanacak ViewModel için ayarlanacak `Slider`.

Programı ilk kez çalıştırdığınızda, `BoxView`, `Label`ve üç `Slider` öğeleridir ilk temel ViewModel tüm kümesinden `Color` ViewModel oluşturulduğunda özelliği. Bu, sol iOS ekran görüntüsünde gösterilmiştir:

[![Basit bir renk seçici](binding-mode-images/simplecolorselector-small.png "basit bir renk seçici")](binding-mode-images/simplecolorselector-large.png#lightbox "basit bir renk seçici")

Kaydırıcıları düzenleme gibi `BoxView` ve `Label` Android ve UWP ekran görüntüleri ile gösterildiği gibi buna göre güncelleştirilir.

Kaynak sözlüğünde ViewModel örnekleme bir yaygın bir yaklaşımdır. Özellik öğesi etiketleri içinde ViewModel örneklemek mümkündür `BindingContext` özelliği. İçinde **basit Renk Seçici** XAML dosya, uygulamadan yetkilendirmeleri `HslColorViewModel` kaynak sözlüğünden ve `BindingContext` özelliği `Grid` şöyle:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

Bağlama bağlamı, çeşitli şekillerde ayarlayabilirsiniz. Bazı durumlarda, arka plan kod dosyası ViewModel örneğini oluşturur ve ayarlar `BindingContext` sayfasının özelliği. Bu, tüm geçerli yaklaşımları vardır.

## <a name="overriding-the-binding-mode"></a>Bağlama modu geçersiz kılma

Hedef özelliği varsayılan bağlama modunu belirli veri bağlama için uygun değilse, ayarlayarak geçersiz kılmak olası [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) özelliği `Binding` (veya [ `Mode` ](xref:Xamarin.Forms.Xaml.BindingExtension.Mode) özelliği `Binding` işaretleme uzantısı) üyelerinin birine `BindingMode` sabit listesi.

Ancak, ayarı `Mode` özelliğini `TwoWay` bekleyebileceğiniz gibi her zaman çalışmaz. Örneğin, değiştirmeyi deneyin **alternatif XAML bağlama** XAML dosyasını içerecek şekilde `TwoWay` bağlama tanımı:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Beklenen, `Slider` ilk değeri olarak başlatılan `Scale` 1 olduğu, ancak değil gerçekleşen özelliği. Olduğunda bir `TwoWay` bağlama başlatılan, hedef kaynak sunucudan ilk olarak, yani ayarlanır `Scale` özelliği `Slider` varsayılan değer 0. Zaman `TwoWay` bağlama ayarlanır `Slider`, ardından `Slider` başlangıçta kaynak ayarlanır.

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

Artık `Slider` 1 olarak başlatılır (varsayılan değeri `Scale`) ancak çağırmanın `Slider` etkilemez `Scale` özelliği, bu nedenle bu çok yararlı değildir.

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Sınıfı da tanımlar [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) ve [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) ölçeklendirebilirsiniz özellikleri `VisualElement` içinde farklı Yatay ve dikey yönde.

Varsayılan bağlama moduyla geçersiz kılma, faydalı bir uygulama `TwoWay` içerir `SelectedItem` özelliği `ListView`. Varsayılan bağlama mod `OneWayToSource`. Üzerinde veri bağlama ayarlandığında `SelectedItem` bir ViewModel kaynağı özelliğinde, kaynak özelliğini ayarlayın daha sonra başvurmak için özellik `ListView` seçimi. Ancak, bazı durumlarda da isteyebilirsiniz `ListView` ViewModel başlatılacak.

**Örnek ayarlarını** sayfası, bu teknik gösterir. Bu sayfa çok sık bunun gibi bir ViewModel tanımlanan uygulama ayarları, basit bir uygulamasını temsil eder `SampleSettingsViewModel` dosyası:

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

Her uygulama ayarı Xamarin.Forms özellikleri sözlüğüne adlı bir yöntem olarak kaydedilmiş bir özelliktir `SaveState` oluşturucusunda bu sözlüğünden yüklenip yüklenmediğini. Sınıfının altında Viewmodel'lar kolaylaştırmaya ve hata potansiyeli daha az yapmanız yardımcı iki yöntem vardır. `OnPropertyChanged` Yöntemi altındaki arama özelliği için isteğe bağlı bir parametre vardır. Bu özelliğin adını bir dize olarak belirlerken yazım hatalarını ortadan kaldırır.

`SetProperty` Sınıfında yöntemi bile daha fazlasını yapar: özellik için bir alan olarak depolanan değeri ile ayarlanan değeri karşılaştırır ve yalnızca çağıran `OnPropertyChanged` zaman iki değer eşit olup.

`SampleSettingsViewModel` Sınıfı tanımlayan iki özellik için arka plan rengi: `BackgroundNamedColor` özelliği türüdür `NamedColor`, hangi bir sınıf da dahil **DataBindingDemos** çözüm. `BackgroundColor` Özelliği türüdür `Color`ve öğesinden alınan `Color` özelliği `NamedColor` nesne.

`NamedColor` Sınıfı, tüm statik ortak alanları Xamarin.Forms numaralandırmak için .NET yansıtma kullanır `Color` yapısını ve statik erişilebilir bir koleksiyondaki adları ile depolamaya `All` özelliği:

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

`App` Sınıfını **DataBindingDemos** proje tanımlar adlı bir özellik `Settings` türü `SampleSettingsViewModel`. Bu özellik başlatılır, `App` sınıfı oluşturulana ve `SaveState` yöntemi çağrılır `OnSleep` yöntemi çağrılır:

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

Uygulama yaşam döngüsü yöntemleri hakkında daha fazla bilgi için bkz [ **uygulama yaşam döngüsü**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Neredeyse her şey ele alınır **SampleSettingsPage.xaml** dosya. `BindingContext` Sayfanın kullanılarak ayarlanan bir `Binding` işaretleme uzantısı: statik bağlama kaynağı olan `Application.Current` örneği olan özelliği, `App` sınıfı projesinde ve `Path` ayarlanır `Settings` özelliğinin `SampleSettingsViewModel` nesnesi:

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

Sayfanın tüm alt öğeleri, bağlama bağlamı devralır. Bu sayfadaki diğer bağlamalar çoğu özelliklere `SampleSettingsViewModel`. `BackgroundColor` Özelliği ayarlamak için kullanılan `BackgroundColor` özelliği `StackLayout`ve `Entry`, `DatePicker`, `Switch`, ve `Stepper` özellikleri tüm bağlı ViewModel diğer özellikleri.

`ItemsSource` Özelliği `ListView` statik olarak ayarlamanız `NamedColor.All` özelliği. Bunu girer `ListView` tüm `NamedColor` örnekleri. Her öğe için `ListView`, bağlama bağlamı öğe için ayarlanmış bir `NamedColor` nesne. `BoxView` Ve `Label` içinde `ViewCell` özelliklerinde bağlı `NamedColor`.

`SelectedItem` Özelliği `ListView` türünde `NamedColor`ve bağlı `BackgroundNamedColor` özelliği `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Varsayılan bağlama modu `SelectedItem` olduğu `OneWayToSource`, seçili öğesini ViewModel özelliği ayarlar. `TwoWay` Modu sağlar `SelectedItem` ViewModel başlatılacak.

Ancak, `SelectedItem` bu şekilde ayarlanmış `ListView` seçili öğe gösterecek şekilde otomatik olarak yapışıktır. Arka plan kod dosyasında biraz kod gereklidir:

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

İlk kez çalıştırdığınızda soldaki iOS ekran program gösterir. Oluşturucuda `SampleSettingsViewModel` başlatır arka plan rengini beyaz olarak ve ne seçili olursa `ListView`:

[![Örnek ayarlar](binding-mode-images/samplesettings-small.png "örnek ayarlar")](binding-mode-images/samplesettings-large.png#lightbox "örnek ayarlar")

Diğer iki ekran görüntüleri, değiştirilen ayarlarını göster. Bu sayfa ile denemeler yaparken uyku ya da cihaz veya çalışan bir öykünücü üzerinde sonlandırmak için program koymayı unutmayın. Visual Studio hata ayıklayıcı programın sonlandırılması değil neden olacak `OnSleep` içinde geçersiz kılma `App` çağrılacak sınıfı.

Sonraki makalede nasıl belirtileceğini göreceğiniz [ **biçimlendirme dizesi** ](string-formatting.md) ayarlandığı veri bağlamalarının `Text` özelliği `Label`.


## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölümden Xamarin.Forms kitabı](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
