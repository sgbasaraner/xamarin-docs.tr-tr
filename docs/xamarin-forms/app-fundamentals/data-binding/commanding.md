---
title: Komut arabirimi
description: Uygulama `Command` veri bağlantılarına sahip özellik
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 7f8b40624b9434347f69a473eed3bdff5c1d3d33
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="the-command-interface"></a>Komut arabirimi

Model-View-ViewModel (MVVM) mimarisinde, genellikle türeyen bir sınıf olan ViewModel özelliklerinde arasında veri bağlamaları tanımlanan `INotifyPropertyChanged`ve Özellikler görünümünde, genellikle XAML dosyasıdır. Bazen bir uygulama şeyin ViewModel etkileyen komutları başlatan kullanıcının gerektiren tarafından bu özellik bağlamaları gidin gereksinimlerine sahiptir. Bu komutlar genellikle düğme tıklamaları tarafından işaret veya finger dokunma ve arka plan kod dosyası için bir işleyici içinde geleneksel olarak işlenen `Clicked` olayı `Button` veya `Tapped` olayı bir `TapGestureRecognizer`. 

Komut verme arabirimi çok MVVM mimarisi için daha uygun olan komutları uygulamak için alternatif bir yaklaşım sağlar. ViewModel görünümünde belirli bir etkinliğe olarak yürütülür yöntemleri komutlar içerebilirler bir `Button` ' ı tıklatın. Veri bağlama, bu komutları arasında tanımlanır ve `Button`.

Veri bağlama arasında izin vermek için bir `Button` ve ViewModel, `Button` iki özelliklerini tanımlar:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) türü <xref:System.Windows.Input.ICommand>
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) türü `Object`

Komut arabirimi kullanmak için hedefleyen bir veri bağlama tanımlamanız `Command` özelliği `Button` kaynak türü ViewModel özelliğinde olduğu `ICommand`. İle ilişkili kod ViewModel içeren `ICommand` düğmesine tıklandığında çalıştırılan özelliği. Ayarlayabileceğiniz `CommandParameter` tüm olmaları durumunda birden çok düğmeleri arasında ayrım yapmak için rasgele veriye bağlı aynı `ICommand` ViewModel özelliği.

`Command` Ve `CommandParameter` özellikleri de aşağıdaki sınıflar tarafından tanımlanır:

- [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) Bu nedenle, [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/), den türetilen `MenuItem`
- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) Bu nedenle, [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), den türetilen `TextCell`
- [`TapGestureRecognizer`](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)

[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) tanımlayan bir [ `SearchCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) türündeki özelliği `ICommand` ve [ `SearchCommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) özelliği. [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) Özelliği [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) de türünde `ICommand`. 

Bu komutlar, içinde bir ViewModel görünümünde belirli kullanıcı arabirimi nesnesi bağlı olmayan bir şekilde işlenebilir.

## <a name="the-icommand-interface"></a>The ICommand Interface

<xref:System.Windows.Input.ICommand> Arabirimi Xamarin.Forms parçası değil. Bunun yerine içinde tanımlandı [System.Windows.Input](xref:System.Windows.Input) ad alanı ve iki yöntem ve bir olay oluşur:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Komut arabirimi kullanmak için ViewModel türünün özelliklerini içeren `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

ViewModel de uygulayan bir sınıf başvurmalıdır `ICommand` arabirimi. Bu sınıf kısa süre içinde açıklanacaktır. Görünümünde `Command` özelliği bir `Button` bu özelliğe bağlıdır: 

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Kullanıcı bastığında `Button`, `Button` çağrıları `Execute` yönteminde `ICommand` nesnesi bağlı kendi `Command` özelliği. Komut verme arabirimi basit parçası olan.

`CanExecute` Yöntemdir daha karmaşık. Ne zaman bağlama ilk üzerinde tanımlanan `Command` özelliği `Button`, ve veri bağlama herhangi bir yolla değiştiğinde `Button` çağrıları `CanExecute` yönteminde `ICommand` nesnesi. Varsa `CanExecute` döndürür `false`, sonra `Button` kendini devre dışı bırakır. Bu, belirli komut şu anda kullanılamıyor veya geçersiz olduğunu gösterir.

`Button` De üzerinde işleyici iliştirir `CanExecuteChanged` olayı `ICommand`. Olay ViewModel içinde tetiklenir. Bu olay başlatıldığında `Button` çağrıları `CanExecute` yeniden. `Button` Kendisini etkinleştirir `CanExecute` döndürür `true` ve kendisini, devre dışı bırakır `CanExecute` döndürür `false`.

> [!IMPORTANT]
> Kullanmayın `IsEnabled` özelliği `Button` komut arabirimini kullanıyorsanız.  

## <a name="the-command-class"></a>Komut sınıfı

Ne zaman, ViewModel tanımlar ne tür `ICommand`, ViewModel gerekir de içeren veya uygulayan bir sınıf başvuru `ICommand` arabirimi. Bu sınıf başvuru içeren veya gerekir `Execute` ve `CanExecute` yöntemleri ve yangın `CanExecuteChanged` olay her `CanExecute` yöntemi farklı bir değer döndürür.

Böyle bir sınıfın kendiniz yazabilirsiniz veya başka birinin yazmıştır bir sınıf kullanabilirsiniz. Çünkü `ICommand` parçası olan Microsoft Windows, Windows MVVM uygulamalarla yıl için kullanılmış. Arabirimini uygulayan bir Windows sınıfını kullanarak `ICommand` , ViewModels Xamarin.Forms uygulamaları ile Windows uygulamaları arasında paylaşmanıza olanak tanır.

Windows ve Xamarin.Forms arasında ViewModels paylaşımı ilgili bir sorun değildir sonra kullanabileceğiniz [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) veya [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) uygulamakiçinXamarin.Formsdahilsınıfı`ICommand`arabirimi. Bu sınıfların gövdeleri belirtmenize olanak veren `Execute` ve `CanExecute` sınıf oluşturucular yöntemlerinde. Kullanmak `Command<T>` kullandığınızda `CommandParameter` birden çok görünüm arasında ayrım yapmak için özellik aynı bağlı `ICommand` özelliği ve basit `Command` sınıfı bir gereksinim olmadığı durumlarda.

## <a name="basic-commanding"></a>Temel komut verme

**Kişi girişi** sayfasındaki [ **veri bağlama gösterileri** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) program bir ViewModel uygulanan bazı basit komutları gösterir.

`PersonViewModel` Adlı üç özelliklerini tanımlar `Name`, `Age`, ve `Skills` bir kişi tanımlayın. Bu sınıfın *değil* içeremez `ICommand` özellikleri:

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    string name;
    double age;
    string skills;

    public event PropertyChangedEventHandler PropertyChanged;

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public double Age
    {
        set { SetProperty(ref age, value); }
        get { return age; }
    }

    public string Skills
    {
        set { SetProperty(ref skills, value); }
        get { return skills; }
    }

    public override string ToString()
    {
        return Name + ", age " + Age;
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

`PersonCollectionViewModel` Gösterilen aşağıda türünde yeni nesneler oluşturur `PersonViewModel` ve veri dolduran olanak tanır. Bu amaç için sınıf özelliklerini tanımlar `IsEditing` türü `bool` ve `PersonEdit` türü `PersonViewModel`. Ayrıca, sınıf türü üç özelliklerini tanımlar `ICommand` ve adlı bir özellik `Persons` türü `IList<PersonViewModel>`: 

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    PersonViewModel personEdit;
    bool isEditing;

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public bool IsEditing
    {
        private set { SetProperty(ref isEditing, value); }
        get { return isEditing; }
    }

    public PersonViewModel PersonEdit
    {
        set { SetProperty(ref personEdit, value); }
        get { return personEdit; }
    }

    public ICommand NewCommand { private set; get; }

    public ICommand SubmitCommand { private set; get; }

    public ICommand CancelCommand { private set; get; }

    public IList<PersonViewModel> Persons { get; } = new ObservableCollection<PersonViewModel>();

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

Bu kısaltılmış listeyi yerdir sınıfının oluşturucusu içermez türünün üç özelliklerini `ICommand` tanımlanır, hangi gösterilecek kısa süre içinde. Türünün üç özelliklerini değiştirir bildirimi `ICommand` ve `Persons` özelliği olmayan neden `PropertyChanged` harekete olaylar. Bu özellikler sınıfı ilk oluşturulduğunda tüm kümesi ve bundan sonra değiştirmeyin.

Oluşturucusunun inceleniyor önce `PersonCollectionViewModel` sınıfı, XAML dosyası için bakalım **kişi girişi** program. Bu içeren bir `Grid` ile kendi `BindingContext` özelliğini `PersonCollectionViewModel`. `Grid` İçeren bir `Button` metinle **yeni** ile kendi `Command` özelliği bağlı `NewCommand` özelliklere sahip bir giriş formu ViewModel özelliğinde bağlı `IsEditing` özelliği olarak özelliklerini hem de `PersonViewModel`, ve iki daha fazla düğme bağlı `SubmitCommand` ve `CancelCommand` ViewModel özelliklerini. En son `ListView` girilmiş kişi koleksiyonunu görüntüler:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.PersonEntryPage"
             Title="Person Entry">
    <Grid Margin="10">
        <Grid.BindingContext>
            <local:PersonCollectionViewModel />
        </Grid.BindingContext>
        
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <!-- New Button -->
        <Button Text="New"
                Grid.Row="0"
                Command="{Binding NewCommand}"
                HorizontalOptions="Start" />

        <!-- Entry Form -->
        <Grid Grid.Row="1"
              IsEnabled="{Binding IsEditing}">
            
            <Grid BindingContext="{Binding PersonEdit}">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Label Text="Name: " Grid.Row="0" Grid.Column="0" />
                <Entry Text="{Binding Name}" 
                       Grid.Row="0" Grid.Column="1" />

                <Label Text="Age: " Grid.Row="1" Grid.Column="0" />
                <StackLayout Orientation="Horizontal" 
                             Grid.Row="1" Grid.Column="1">
                    <Stepper Value="{Binding Age}"
                             Maximum="100" />
                    <Label Text="{Binding Age, StringFormat='{0} years old'}"
                           VerticalOptions="Center" />
                </StackLayout>

                <Label Text="Skills: " Grid.Row="2" Grid.Column="0" />
                <Entry Text="{Binding Skills}"
                       Grid.Row="2" Grid.Column="1" />

            </Grid>
        </Grid>

        <!-- Submit and Cancel Buttons -->
        <Grid Grid.Row="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <Button Text="Submit"
                    Grid.Column="0"
                    Command="{Binding SubmitCommand}"
                    VerticalOptions="CenterAndExpand" />

            <Button Text="Cancel"
                    Grid.Column="1"
                    Command="{Binding CancelCommand}"
                    VerticalOptions="CenterAndExpand" />
        </Grid>

        <!-- List of Persons -->
        <ListView Grid.Row="3"
                  ItemsSource="{Binding Persons}" />
    </Grid>
</ContentPage>
```

İşte nasıl çalışır?: kullanıcı ilk basarsa **yeni** düğmesi. Bu giriş formunu sağlar ancak devre dışı bırakır **yeni** düğmesi. Kullanıcı daha sonra bir ad, geçerlilik süresi ve yetenekleri girer. Kullanıcı düzenleme sırasında herhangi bir zamanda basabilirsiniz **iptal** üzerinden başlamak için Başlat. Bir ad ve geçerli yaş zaman girilmiş yalnızca **gönderme** düğmesi etkin. Bu tuşuna basarak **gönderme** düğme aktarır kişi tarafından görüntülenen koleksiyonuna `ListView`. Ya da sonra **iptal** veya **gönderme** düğmesine basıldığında, giriş formunun temizlenir ve **yeni** düğmesini yeniden etkinleştirildi.

Geçerli yaş girilmeden önce iOS ekranın sol düzeni gösterilir. Android ve UWP ekranları Göster **gönderme** bir geçerlilik süresi belirledikten sonra etkin düğmesi:

[![Kişi girişi](commanding-images/personentry-small.png "kişi girişi")](commanding-images/personentry-large.png#lightbox "kişi girişi")

Program girdilerinin düzenlemek için tüm olanaklara sahip değil ve giriş sayfadan kaydetmez.

Tüm mantığını **yeni**, **gönderme**, ve **iptal** düğmeleri işlenir `PersonCollectionViewModel` tanımları aracılığıyla `NewCommand`, `SubmitCommand`, ve `CancelCommand` özellikleri. Oluşturucusunun `PersonCollectionViewModel` türündeki nesneler için bu üç özellik ayarlar `Command`.  

A [Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) , `Command` sınıfı sağlar, tür bağımsız değişkenleri geçirmek `Action` ve `Func<bool>` karşılık gelen `Execute` ve `CanExecute` yöntemleri. Bu eylemleri ve işlevleri lamda işlevleri sağ olarak tanımlamak kolay `Command` Oluşturucusu. Tanımını işte `Command` için nesne `NewCommand` özelliği:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {
        NewCommand = new Command(
            execute: () =>
            {
                PersonEdit = new PersonViewModel();
                PersonEdit.PropertyChanged += OnPersonEditPropertyChanged;
                IsEditing = true;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return !IsEditing;
            });

        ···

    }

    void OnPersonEditPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        (SubmitCommand as Command).ChangeCanExecute();
    }
    
    void RefreshCanExecutes()
    {
        (NewCommand as Command).ChangeCanExecute();
        (SubmitCommand as Command).ChangeCanExecute();
        (CancelCommand as Command).ChangeCanExecute();
    }

    ···

}
```

Kullanıcı tıkladığında **yeni** düğmesini `execute` işlevi geçirilen `Command` Oluşturucusu gerçekleştirilir. Bu yeni bir oluşturur `PersonViewModel` nesne, o nesne üzerinde bir işleyici ayarlar `PropertyChanged` olay, ayarlar `IsEditing` için `true`ve çağırır `RefreshCanExecutes` sonra Oluşturucusu tanımlanmış yöntemi.

Uygulama yanı sıra `ICommand` arabirimi, `Command` sınıfı adlı bir yöntem de tanımlayan `ChangeCanExecute`. ViewModel çağırmalıdır `ChangeCanExecute` için bir `ICommand` herhangi bir şey bu durum her özellik dönüş değerini değişebilir `CanExecute` yöntemi. Çağrı `ChangeCanExecute` neden `Command` tetiklenecek sınıfı `CanExecuteChanged` yöntemi. `Button` Bu olay için bir işleyici eklenmiş ve çağırarak yanıt verdiğini `CanExecute` yeniden ve kendisini bu yöntemin dönüş değerini temel etkinleştirme.

Zaman `execute` yöntemi `NewCommand` çağrıları `RefreshCanExecutes`, `NewCommand` özelliğini alır yapılan bir çağrı `ChangeCanExecute`ve `Button` çağrıları `canExecute` şimdi döndürür yöntemi `false` çünkü `IsEditing`özelliktir şimdi `true`.

`PropertyChanged` Yeni işleyici `PersonViewModel` nesne çağrıları `ChangeCanExecute` yöntemi `SubmitCommand`. İşte bu komut özelliği nasıl uygulanır:


```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        SubmitCommand = new Command(
            execute: () =>
            {
                Persons.Add(PersonEdit);
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return PersonEdit != null && 
                       PersonEdit.Name != null && 
                       PersonEdit.Name.Length > 1 && 
                       PersonEdit.Age > 0;
            });

        ···
    }

    ···

}
```

`canExecute` İçin işlev `SubmitCommand` olduğundan her zaman içinde değiştirilen bir özellik olarak adlandırılır `PersonViewModel` düzenlenmekte olan nesne. Döndürdüğü `true` yalnızca `Name` en az bir karakter uzunluğunda bir özelliktir ve `Age` 0'dan büyük. Bu sırada, **gönderme** düğmesi etkin hale gelir. 

`execute` İçin işlev **gönderme** özelliği değiştirildi işleyicisinden kaldırır `PersonViewModel`, nesneye ekler `Persons` koleksiyonu ve her şeyi ilk koşullar döndürür.

`execute` İçin işlev **iptal** düğmesinin işlevini her şeyi, **gönderme** düğmesi mu execept nesne koleksiyona ekleyin:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        CancelCommand = new Command(
            execute: () =>
            {
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return IsEditing;
            }); 
    }

    ···

}
```

`canExecute` Yöntemi döndürür `true` herhangi bir anda bir `PersonViewModel` düzenleniyor.

Bu teknikler daha karmaşık senaryolar için uyarlanmış olabilir: bir özelliğe `PersonCollectionViewModel` bağlı `SelectedItem` özelliği `ListView` varolan öğeleri düzenlemek için ve bir **silmek** düğmesini eklenebiliyor silmek için Bu öğeler.

Tanımlamak gerekli değildir `execute` ve `canExecute` lambda işlevler olarak yöntemler. Bunları ViewModel olarak normal özel yöntemler yazmak ve onları başvuru `Command` oluşturucular. Ancak, bu yaklaşım ViewModel yalnızca bir kez başvurulan yöntemleri birçok neden eğilimindedir.

## <a name="using-command-parameters"></a>Komut parametreleri kullanma

Bazen bir veya daha fazla düğme (veya diğer kullanıcı arabirimi nesneleri için) aynı paylaşmak uygun olan `ICommand` ViewModel özelliği. Bu durumda, kullandığınız `CommandParameter` düğmeleri arasında ayrım yapmak için özellik. 

Kullanmaya devam edebilirsiniz `Command` bu paylaşılan için sınıf `ICommand` özellikleri. Sınıfı tanımlayan bir [alternatif Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action%7BSystem.Object%7D/System.Func%7BSystem.Object,System.Boolean%7D/) kabul eden `execute` ve `canExecute` türünde parametre yöntemleriyle `Object`. Bunun nasıl `CommandParameter` bu yöntemlere iletilen.

Ancak, kullanırken `CommandParameter`, genel kullanmak en kolayıdır [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) kümesine nesne türünü belirtmek için sınıf `CommandParameter`. `execute` Ve `canExecute` belirttiğiniz yöntemlerine sahip parametre türü.

**Ondalık klavye** sayfa ondalık sayıları girmek için bir tuş uygulamak nasıl yapıldığını göstererek Bu teknik gösterilmektedir. `BindingContext` İçin `Grid` olan bir `DecimalKeypadViewModel`. `Entry` Bu ViewModel özelliğinin bağlı `Text` özelliği bir `Label`. Tüm `Button` nesneleri ViewModel çeşitli komutlar bağlı: `ClearCommand`, `BackspaceCommand`, ve `DigitCommand`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.DecimalKeypadPage"
             Title="Decimal Keyboard">

    <Grid WidthRequest="240"
          HeightRequest="480"
          ColumnSpacing="2"
          RowSpacing="2"
          HorizontalOptions="Center"
          VerticalOptions="Center">

        <Grid.BindingContext>
            <local:DecimalKeypadViewModel />
        </Grid.BindingContext>
        
        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Button">
                    <Setter Property="FontSize" Value="32" />
                    <Setter Property="BorderWidth" Value="1" />
                    <Setter Property="BorderColor" Value="Black" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Label Text="{Binding Entry}"
               Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3"
               FontSize="32"
               LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center"
               HorizontalTextAlignment="End" />

        <Button Text="CLEAR"
                Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding ClearCommand}" />

        <Button Text="&#x21E6;"
                Grid.Row="1" Grid.Column="2" 
                Command="{Binding BackspaceCommand}" />

        <Button Text="7"
                Grid.Row="2" Grid.Column="0" 
                Command="{Binding DigitCommand}"
                CommandParameter="7" />

        <Button Text="8"
                Grid.Row="2" Grid.Column="1" 
                Command="{Binding DigitCommand}"
                CommandParameter="8" />
        
        <Button Text="9"
                Grid.Row="2" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="9" />

        <Button Text="4"
                Grid.Row="3" Grid.Column="0" 
                Command="{Binding DigitCommand}"
                CommandParameter="4" />

        <Button Text="5"
                Grid.Row="3" Grid.Column="1" 
                Command="{Binding DigitCommand}"
                CommandParameter="5" />

        <Button Text="6"
                Grid.Row="3" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="6" />

        <Button Text="1"
                Grid.Row="4" Grid.Column="0" 
                Command="{Binding DigitCommand}"
                CommandParameter="1" />

        <Button Text="2"
                Grid.Row="4" Grid.Column="1" 
                Command="{Binding DigitCommand}"
                CommandParameter="2" />

        <Button Text="3"
                Grid.Row="4" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="3" />

        <Button Text="0"
                Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding DigitCommand}"
                CommandParameter="0" />

        <Button Text="&#x00B7;"
                Grid.Row="5" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="." />
    </Grid>
</ContentPage>
```

Bir bağlama 10 tabanlı rakamlar ve Ondalık ayırıcının 11 düğmeleri paylaşmak `DigitCommand`. `CommandParameter` Bu düğmeleri arasında ayırır. Ayarlanan değer `CommandParameter` ondalık daha anlaşılır olması amacıyla bir orta nokta karakteriyle görüntülenen noktası dışında düğmesi tarafından görüntülenen metni genellikle aynıdır.

Eylem program şöyledir:

[![Ondalık klavye](commanding-images/decimalkeyboard-small.png "ondalık klavye")](commanding-images/decimalkeyboard-large.png#lightbox "ondalık klavye")

Girilen sayı ondalık içerdiğinden tüm üç ekran görüntüleri ondalık noktasının düğmesi devre dışıdır dikkat edin. 

`DecimalKeypadViewModel` Tanımlayan bir `Entry` türündeki özelliği `string` (tetikler tek özellik olduğu bir `PropertyChanged` olay) ve üç özellik türü `ICommand`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{
    string entry = "0";

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public string Entry
    {
        private set
        {
            if (entry != value)
            {
                entry = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Entry"));
            }
        }
        get
        {
            return entry;
        }
    }

    public ICommand ClearCommand { private set; get; }

    public ICommand BackspaceCommand { private set; get; }

    public ICommand DigitCommand { private set; get; }
}
```

Düğme karşılık gelen `ClearCommand` her zaman etkindir ve yalnızca girişine "0" ayarlar:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {
        ClearCommand = new Command(
            execute: () =>
            {
                Entry = "0";
                RefreshCanExecutes();
            });

        ···
    
    }

    void RefreshCanExecutes()
    {
        ((Command)BackspaceCommand).ChangeCanExecute();
        ((Command)DigitCommand).ChangeCanExecute();
    }

    ···
    
}
```

Düğme her zaman etkindir, belirtmek için gerekli değildir, çünkü bir `canExecute` değişkeninde `Command` Oluşturucusu.

Sayı girme ve geri alma için mantığı biraz zor olduğundan hiç basamak girdiyseniz, sonra `Entry` "0" dizesini bir özelliktir. Kullanıcı daha fazla sıfır yazdığında sonra `Entry` hala tek içeren sıfır. Kullanıcı herhangi bir basamak yazarsa, bu sayı sıfır yerini alır. Ancak kullanıcı önce diğer herhangi bir rakam, ondalık ayırıcıdan sonra yazdığında `Entry` "0" dizesidir.

**Geri** yalnızca giriş uzunluğu 1'den büyük olduğunda veya düğmesi etkin `Entry` "0" dizesini eşit değildir:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        BackspaceCommand = new Command(
            execute: () =>
            {
                Entry = Entry.Substring(0, Entry.Length - 1);
                if (Entry == "")
                {
                    Entry = "0";
                }
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return Entry.Length > 1 || Entry != "0";
            });

        ···

    }

    ···
    
}
```

Mantığını `execute` için işlev **geri** düğmesi sağlar `Entry` en az bir "0" dizesidir.

`DigitCommand` Özelliği 11 düğmeleri, her biri tanımlayan kendisiyle bağlı `CommandParameter` özelliği. `DigitCommand` Normal örneğine ayarlanabilir `Command` sınıfı, ancak kullanmak daha kolay `Command<T>` genel bir sınıf. Komut verme arabirimi XAML ile kullanırken `CommandParameter` özellikleri genellikle dizelerdir ve genel bağımsız değişken türü değil. `execute` Ve `canExecute` işlevleri sonra sahip bağımsız değişken türü `string`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        DigitCommand = new Command<string>(
            execute: (string arg) =>
            {
                Entry += arg;
                if (Entry.StartsWith("0") && !Entry.StartsWith("0."))
                {
                    Entry = Entry.Substring(1);
                }
                RefreshCanExecutes();
            },
            canExecute: (string arg) =>
            {
                return !(arg == "." && Entry.Contains("."));
            });
    }

    ···
    
}
```

`execute` Yöntemi ekler dize bağımsız değişkeni `Entry` özelliği. Sıfır (ancak değil sıfır ile ondalık) sonucu başlıyorsa, ancak, ardından bu ilk sıfır kullanarak kaldırılmalıdır `Substring` işlevi.

`canExecute` Yöntemi döndürür `false` yalnızca bağımsız değişken ondalık (Ondalık ayırıcının basılı olduğunu gösteren) ise ve `Entry` ondalık zaten içeriyor. 

Tüm `execute` yöntem çağrısı `RefreshCanExecutes`, ardından çağıran `ChangeCanExecute` her ikisi için de `DigitCommand` ve `ClearCommand`. Bu ondalık ayırıcıdan ve geri düğmeleri etkin veya devre dışı girilen rakamları geçerli dizisi üzerinde temel sağlar.

## <a name="adding-commands-to-existing-views"></a>Varolan görünümlere komut ekleme

Komut verme arabirimi desteklemeyen görünümleri ile kullanmak istiyorsanız, bir olay bir komuta dönüştürür Xamarin.Forms davranışı kullanmak da mümkündür. Bu makalede açıklanan [ **yeniden kullanılabilir EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Zaman uyumsuz için Gezinti menüler komut verme

Kumanda uygulamasında gibi Gezinti menüleri uygulamak için uygun [ **veri bağlama gösterileri** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) kendisini program. Parçası işte **MainPage.xaml**:


```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MainPage"
             Title="Data Binding Demos"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection Title="Basic Bindings">

                <TextCell Text="Basic Code Binding"
                          Detail="Define a data-binding in code"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicCodeBindingPage}" />

                <TextCell Text="Basic XAML Binding"
                          Detail="Define a data-binding in XAML"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicXamlBindingPage}" />

                <TextCell Text="Alternative Code Binding"
                          Detail="Define a data-binding in code without a BindingContext"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:AlternativeCodeBindingPage}" />

                ···

            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

XAML ile kumanda kullanırken `CommandParameter` özellikleri genellikle dizeye ayarlayın. Bu durumda, ancak XAML biçimlendirme uzantısı kullanılan böylece `CommandParameter` türü `System.Type`.

Her `Command` özelliği adlı bir özelliğe bağlı `NavigateCommand`. Özellik arka plan kod dosyasına tanımlandı **MainPage.xaml.cs**:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(
            async (Type pageType) =>
            {
                Page page = (Page)Activator.CreateInstance(pageType);
                await Navigation.PushAsync(page);
            });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Oluşturucu kümeleri `NavigateCommand` özelliğine bir `execute` başlatır yöntemi `System.Type` parametre ve kendisine gider. Çünkü `PushAsync` çağrısı gerektirir bir `await` işleci, `execute` yöntemi gerekir işaretlenen zaman uyumsuz olarak. Bu ile gerçekleştirilir `async` anahtar sözcüğü önce parametre listesi. 

Ayrıca Oluşturucusu ayarlar `BindingContext` kendisine sayfasının bağlamaları başvuru böylece `NavigateCommand` bu sınıftaki.

Bu oluşturucu kodda sırasını bir fark etmez: `InitializeComponent` çağrısı ayrıştırılması XAML neden olur, ancak o anda bir özelliği için bağlama adlı `NavigateCommand` çünkü çözümlenemiyor `BindingContext` ayarlanır `null`. Varsa `BindingContext` oluşturucuda ayarlamak *önce* `NavigateCommand` ayarlanır, bağlama ne zaman çözümlenebilir sonra `BindingContext` ayarlanır, ancak o zaman `NavigateCommand` hala `null`. Ayarı `NavigateCommand` sonra `BindingContext` çünkü bağlama üzerinde hiçbir etkisi olmaz bir değişiklik `NavigateCommand` yangın olmayan bir `PropertyChanged` olay ve bağlama değil biliyorsanız, `NavigateCommand` artık geçerli değil.

Her ikisi de ayarı `NavigateCommand` ve `BindingContext` (, herhangi bir sırada) çağrısından önce `InitializeComponent` XAML ayrıştırıcısı bağlama tanımı karşılaştığında hem bileşenleri bağlamanın ayarlandığından çalışır. 

Veri bağlamaları bazen zor olabilir, ancak bu makaleler dizide gördüğünüz gibi bunlar güçlü ve çok yönlü ve büyük ölçüde kullanıcı arabiriminden temel mantığını ayırarak kodunuzu düzenlemek için Yardım.



## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama gösterileri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
