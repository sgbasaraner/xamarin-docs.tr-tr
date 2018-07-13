---
title: Xamarin.Forms komut arabirimi
description: Bu makalede, komut özelliği Xamarin.Forms veri bağlamaları ile uygulama açıklanmaktadır. Komut verme arabirimi çok MVVM mimari için daha iyi uygun komutlar uygulama için alternatif bir yaklaşım sağlar.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: b18d042e34146a72b488da9017648a430c9cd353
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996379"
---
# <a name="the-xamarinforms-command-interface"></a>Xamarin.Forms komut arabirimi

Model-View-ViewModel (MVVM) mimaride, genel olarak, türetilen bir sınıf olan ViewModel özelliklerinde arasında veri bağlamaları tanımlanan `INotifyPropertyChanged`ve özellikleri görünümünde, genellikle XAML dosyasıdır. Bazen bir uygulama, bu özelliği bağlamaları dışında bir şey ViewModel etkileyen komutlar başlatan kullanıcının gerektirerek Git gereksinimlerine sahiptir. Bu komutlar tarafından düğme tıklamaları genellikle sinyal veya finger Tap'ları ve arka plan kod dosyasında için bir işleyici geleneksel olarak işlenen `Clicked` olayı `Button` veya `Tapped` olayı bir `TapGestureRecognizer`.

Komut verme arabirimi çok MVVM mimari için daha iyi uygun komutlar uygulama için alternatif bir yaklaşım sağlar. ViewModel görünümünde belirli bir etkinlik olarak yürütülen yöntemler olan komutları içerebilir bir `Button` tıklayın. Bu komutlar arasında veri bağlamaları tanımlanır ve `Button`.

Bir veri bağlamayı arasında izin vermek için bir `Button` ve ViewModel, `Button` iki özelliklerini tanımlar:

- [`Command`](xref:Xamarin.Forms.Button.Command) türü <xref:System.Windows.Input.ICommand>
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) türü `Object`

Komut arabirimi kullanmak için hedefleyen veri bağlamayı tanımlayan `Command` özelliği `Button` kaynak türünün ViewModel özelliğinde olduğu `ICommand`. ViewModel ile ilişkili kod içeren `ICommand` düğmesine tıklandığında çalıştırılan özelliği. Ayarlayabileceğiniz `CommandParameter` tüm olmaları durumunda birden çok düğmeleri arasında ayırt etmek için rastgele verilere bağlı aynı `ICommand` ViewModel özelliği.

`Command` Ve `CommandParameter` özellikleri de aşağıdaki sınıflar tarafından tanımlanır:

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) Bu nedenle, [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem), öğesinden türetildiğini `MenuItem`
- [`TextCell`](xref:Xamarin.Forms.TextCell) Bu nedenle, [ `ImageCell` ](xref:Xamarin.Forms.ImageCell), öğesinden türetildiğini `TextCell`
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) tanımlayan bir [ `SearchCommand` ](xref:Xamarin.Forms.SearchBar.SearchCommand) türünün özelliği `ICommand` ve [ `SearchCommandParameter` ](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) özelliği. [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) Özelliği [ `ListView` ](xref:Xamarin.Forms.ListView) türünde de `ICommand`.

Bu komutlar, içinde bir ViewModel görünümünde belirli kullanıcı arabirimi nesnesi bağlı olmayan bir şekilde işlenebilir.

## <a name="the-icommand-interface"></a>ICommand arabirimi

<xref:System.Windows.Input.ICommand> Arabirimi Xamarin.Forms parçası değil. Bunun yerine içinde tanımlanır [System.Windows.Input](xref:System.Windows.Input) ad alanını ve iki yöntem ve bir olay oluşur:

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

ViewModel ayrıca uygulayan bir sınıfa başvurmalıdır `ICommand` arabirimi. Bu sınıf, kısa bir süre sonra açıklanacaktır. Görünümü'nde `Command` özelliği bir `Button` bu özelliğe bağlıdır:

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Kullanıcının bastığında `Button`, `Button` çağrıları `Execute` yönteminde `ICommand` nesne bağlı kendi `Command` özelliği. Basit bir komut verme arabirimi parçası olmasıdır.

`CanExecute` Yöntemi daha karmaşıktır. Ne zaman bağlama ilk üzerinde tanımlanan `Command` özelliği `Button`, ve veri bağlama, şekilde değiştiğinde `Button` çağrıları `CanExecute` yönteminde `ICommand` nesne. Varsa `CanExecute` döndürür `false`, ardından `Button` kendini devre dışı bırakır. Bu, belirli komut şu anda kullanılamıyor veya geçersiz olduğunu gösterir.

`Button` Ayrıca üzerinde bir işleyici ekler `CanExecuteChanged` olayı `ICommand`. Olay ViewModel içinde tetiklenir. Olay tetiklendiğinde `Button` çağrıları `CanExecute` yeniden. `Button` Kendisini varsa etkinleştirir `CanExecute` döndürür `true` ve kendisini, devre dışı bırakır `CanExecute` döndürür `false`.

> [!IMPORTANT]
> Kullanmayın `IsEnabled` özelliği `Button` komut arabirimi kullanıyorsanız.  

## <a name="the-command-class"></a>Komut sınıfı

Ne zaman bir ne tür tanımlar, ViewModel `ICommand`, ViewModel gerekir ayrıca içeren veya uygulayan bir sınıf başvurusu `ICommand` arabirimi. Bu sınıf içeren veya başvuru `Execute` ve `CanExecute` yöntemleri ve yangın `CanExecuteChanged` olay olduğunda `CanExecute` yöntemi, farklı bir değer döndürebilir.

Kendinizi bu tür bir sınıf yazabilirsiniz veya başkasının yazılmış bir sınıfı kullanabilirsiniz. Çünkü `ICommand` parçası olan Microsoft Windows Windows MVVM uygulamalarla yıl için kullanılmış. Uygulayan bir Windows sınıfını kullanarak `ICommand` , Viewmodel'lar Windows uygulamaları ve Xamarin.Forms uygulamaları arasında paylaşmanıza olanak tanır.

Xamarin.Forms ile Windows arasındaki Viewmodel'lar paylaşımı önemli değil sonra kullanabileceğiniz [ `Command` ](xref:Xamarin.Forms.Command) veya [ `Command<T>` ](xref:Xamarin.Forms.Command`1) uygulamakiçinXamarin.Formsiçindebulunansınıf`ICommand`arabirimi. Bu sınıfların gövdeleri belirlemenizi sağlayan `Execute` ve `CanExecute` sınıf oluşturucuları yöntemleri. Kullanma `Command<T>` kullandığınızda `CommandParameter` birden çok görünüm arasında ayrım yapmak için özelliğe aynı `ICommand` özellik ve daha basit `Command` sınıfı, bir gereksinim değildir.

## <a name="basic-commanding"></a>Temel komut vermeye genel

**Kişi giriş** sayfasını [ **veri bağlama tanıtımları** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) programı bir ViewModel içinde uygulanan bazı basit komutlar gösterir.

`PersonViewModel` Adlı üç özelliklerini tanımlar `Name`, `Age`, ve `Skills` bir kişi tanımlayın. Bu sınıfla *değil* içeremez `ICommand` özellikleri:

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

`PersonCollectionViewModel` Gösterilen aşağıda türü yeni nesneler oluşturur `PersonViewModel` ve verileri doldurun izin verir. Bu amaçla, sınıf özelliklerini tanımlar `IsEditing` türü `bool` ve `PersonEdit` türü `PersonViewModel`. Ayrıca, sınıf türünde üç özellik tanımlar `ICommand` ve adlı bir özellik `Persons` türü `IList<PersonViewModel>`:

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

Bu kısaltılmış listeyi nerede olan sınıfın oluşturucusu içermez üç tür özelliklerini `ICommand` tanımlanır, hangi gösterilecek kısa bir süre. Üç tür özelliklerine değiştiğine dikkat edin `ICommand` ve `Persons` özelliği içinde neden `PropertyChanged` tetiklenen olayları. Bu özellikler, sınıfı oluşturulduğunda tüm kümesidir ve bundan sonra değiştirmeyin.

Oluşturucusuna İnceleme önce `PersonCollectionViewModel` sınıfı, XAML dosyası için göz atalım **kişi giriş** program. Bu içeren bir `Grid` ile kendi `BindingContext` özelliğini `PersonCollectionViewModel`. `Grid` İçeren bir `Button` metinle **yeni** ile kendi `Command` özelliği bağlı `NewCommand` özelliklere sahip bir giriş formunu ViewModel özelliğine bağlı `IsEditing` özelliği olarak özellikleri olarak da `PersonViewModel`, ve bağlı iki daha fazla düğme `SubmitCommand` ve `CancelCommand` ViewModel özellikleri. En son `ListView` girilmiş kişiler koleksiyonunu görüntüler:

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

Çalışma şekli şöyledir: kullanıcı ilk basarsa **yeni** düğmesi. Bu giriş formunu sağlar ancak devre dışı bırakır **yeni** düğmesi. Kullanıcı daha sonra bir adı, yaşı ve becerileri girer. Düzenleme sırasında herhangi bir zamanda kullanıcı basabilirsiniz **iptal** düğmesi baştan başlayabilirsiniz. Ne zaman bir ad ve geçerli bir ömrü girildikten yalnızca **Gönder** düğmesi etkin. Bu tuşuna basarak **Gönder** düğme tarafından görüntülenen koleksiyona kişi aktarır `ListView`. Ya da sonra **iptal** veya **Gönder** düğmesine basıldığında, giriş formunu temizlenir ve **yeni** düğmeyi yeniden etkinleştirildi.

Geçerli bir yaş girilmeden önce iOS ekranın sol düzenini gösterir. Android ve UWP ekranları show **Gönder** düğmesi bir geçerlilik süresi belirledikten sonra etkin:

[![Kişi giriş](commanding-images/personentry-small.png "kişi giriş")](commanding-images/personentry-large.png#lightbox "kişi giriş")

Program girdilerinin düzenlemek için herhangi bir özelliği yok ve bu sayfadan ayrılmak gittiğinizde girişleri kaydetmez.

İçin tüm mantığı **yeni**, **Gönder**, ve **iptal** düğme olarak işlenir `PersonCollectionViewModel` tanımları aracılığıyla `NewCommand`, `SubmitCommand`, ve `CancelCommand` özellikleri. Oluşturucusuna `PersonCollectionViewModel` türündeki nesneler için bu üç özellik ayarlar `Command`.  

A [Oluşturucusu](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) , `Command` sınıf türü bağımsız değişkenleri geçirmeniz sağlar `Action` ve `Func<bool>` karşılık gelen `Execute` ve `CanExecute` yöntemleri. Bu eylemler ve İşlevler, lambda işlevleri sağ olarak tanımlamak kolay `Command` Oluşturucusu. İşte tanımını `Command` nesnesi `NewCommand` özelliği:

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

Kullanıcı tıkladığında **yeni** düğmesi `execute` işlevi geçirilen `Command` Oluşturucusu yürütülür. Bu yeni bir oluşturur `PersonViewModel` nesne, o nesne üzerinde bir işleyici ayarlar `PropertyChanged` olay ayarlar `IsEditing` için `true`ve çağıran `RefreshCanExecutes` sonra Oluşturucu tanımlanan yöntemi.

Uygulama yanı sıra `ICommand` arabirimi `Command` sınıf adında bir yöntemi de tanımlar `ChangeCanExecute`. ViewModel çağırmalıdır `ChangeCanExecute` için bir `ICommand` herhangi bir şey, gerçekleştiğinde bir özelliği, dönüş değeri değişebilir `CanExecute` yöntemi. Bir çağrı `ChangeCanExecute` neden `Command` ateşlenmesine sınıfı `CanExecuteChanged` yöntemi. `Button` Çağırarak yanıt verir ve bu olay işleyicisi eklenmiş `CanExecute` yeniden ve kendisi bu yöntemin dönüş değerini temel alarak etkinleştirme.

Zaman `execute` yöntemi `NewCommand` çağrıları `RefreshCanExecutes`, `NewCommand` özelliğini alır bir çağrı `ChangeCanExecute`ve `Button` çağrıları `canExecute` artık döndüren yöntemi `false` çünkü `IsEditing`özelliği, artık `true`.

`PropertyChanged` Yeni işleyici `PersonViewModel` nesne çağrıları `ChangeCanExecute` yöntemi `SubmitCommand`. Bu komut özelliğini nasıl uygulandığını aşağıda verilmiştir:


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

`canExecute` İçin işlev `SubmitCommand` var. her zaman içinde değişen bir özellik adında `PersonViewModel` düzenlenmekte olan nesne. Döndürür `true` yalnızca `Name` en az bir karakter uzunluğunda olmalı, bir özelliktir ve `Age` 0'dan büyük. O zaman **Gönder** düğmesi hale etkin.

`execute` İçin işlev **Gönder** özellik değişti işleyicisinden kaldırır `PersonViewModel`, nesneye ekler `Persons` koleksiyonu ve ilk koşulları her şeyi döndürür.

`execute` İçin işlev **iptal** düğmesinin işlevini her şeyi, **Gönder** düğmesi yoksa execept nesne koleksiyona ekleyin:

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

`canExecute` Yöntemi döndürür `true` herhangi bir zamanda bir `PersonViewModel` düzenleniyor.

Bu teknikler daha karmaşık senaryolar için uyarlanmış olabilir: bir özelliğe `PersonCollectionViewModel` bağlı `SelectedItem` özelliği `ListView` mevcut öğelerini düzenlemek için ve bir **Sil** silmek için düğmeyi eklenebiliyordu Bu öğeleri.

Tanımlamak gerekli değildir `execute` ve `canExecute` lambda işlevleri olarak yöntemler. Bunları düzenli olarak özel yöntemler ViewModel yazmak ve bunları başvuru `Command` oluşturucular. Ancak, bu yaklaşım ViewModel içinde yalnızca bir kez başvurulur yöntemleri birçok neden eğilimindedir.

## <a name="using-command-parameters"></a>Komut parametreleri kullanma

Bazen bir veya daha fazla düğme (veya diğer kullanıcı arabirimi nesneleri) aynı paylaşmak uygun olan `ICommand` ViewModel özelliği. Bu durumda, kullandığınız `CommandParameter` düğmeleri arasında ayrım yapmak için özellik.

Kullanmaya devam edebilirsiniz `Command` sınıfı için bu paylaşılan `ICommand` özellikleri. Sınıfı tanımlayan bir [alternatif Oluşturucusu](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean})) kabul eden `execute` ve `canExecute` türünde parametrelere sahip yöntemleri `Object`. Bu, nasıl `CommandParameter` bu yönteme geçirilir.

Ancak, kullanırken `CommandParameter`, genel en kolayıdır [ `Command<T>` ](xref:Xamarin.Forms.Command`1) ayarlamak nesne türünü belirtmek için sınıf `CommandParameter`. `execute` Ve `canExecute` belirttiğiniz yöntemi bu tür parametreleri vardır.

**Ondalık klavye** sayfası, ondalık sayılar girmek için bir tuş uygulamak nasıl yapıldığını göstererek Bu teknik gösterilmektedir. `BindingContext` İçin `Grid` olduğu bir `DecimalKeypadViewModel`. `Entry` Bu ViewModel özelliği bağlı `Text` özelliği bir `Label`. Tüm `Button` ViewModel içindeki çeşitli komutlara bağlı nesnelere: `ClearCommand`, `BackspaceCommand`, ve `DigitCommand`:

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

Bir bağlama için 10 tabanlı rakamlar ve ondalık nokta 11 düğmeleri paylaşmak `DigitCommand`. `CommandParameter` Bu düğmeler ayırır. Ayarlanan değer `CommandParameter` dışında bir orta nokta karakteri ile görüntülenen netlik amaçları için ondalık nokta, düğme tarafından görüntülenen metni genel olarak aynıdır.

Eylem program şöyledir:

[![Ondalık klavye](commanding-images/decimalkeyboard-small.png "ondalık klavye")](commanding-images/decimalkeyboard-large.png#lightbox "ondalık klavye")

Girilen sayı ondalık noktası içerdiğinden tüm üç ekran görüntüleri ondalık noktanın düğmesini devre dışı olduğunu dikkat edin.

`DecimalKeypadViewModel` Tanımlayan bir `Entry` türünün özelliği `string` (tetikleyen tek özellik olduğunu bir `PropertyChanged` olay) ve üç özellik türü `ICommand`:

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

Düğmeye her zaman etkindir, belirtmek gerekli değildir, çünkü bir `canExecute` değişkeninde `Command` Oluşturucusu.

Sayı girme ve geri alma mantığını biraz zor olduğundan herhangi bir basamak girilmiş, ardından `Entry` "0" dizeye bir özelliktir. Kullanıcı daha fazla sıfır yazarsa sonra `Entry` hala tek içeren sıfır. Kullanıcının herhangi bir basamak yazarsa, o sayı sıfır değiştirir. Ancak kullanıcı bir ondalık ayırıcıdan önce diğer herhangi bir basamak, ardından yazarsa `Entry` "0" dizesidir.

**Geri** düğmesi yalnızca girişin uzunluğu 1'den büyük olduğunda veya etkin `Entry` "0" dizeye eşit değil:

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

Mantığını `execute` için işlev **geri** düğmesi sağlar `Entry` en az bir dizedir "0".

`DigitCommand` Özelliği 11 düğmeleri, her biri tanımlayan kendisi ile bağlı `CommandParameter` özelliği. `DigitCommand` Normal örneğine ayarlanabileceğini `Command` sınıfı, ancak kullanmak daha kolay `Command<T>` genel bir sınıf. Komut verme arabirimi XAML ile kullanırken `CommandParameter` özellikleri genellikle dizelerdir ve genel bağımsız değişken türü. `execute` Ve `canExecute` işlevleri türünde bağımsız değişkenler ardından sahip `string`:

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

`execute` Yöntem için dize bağımsız değişkeni ekler `Entry` özelliği. Sonucun sıfır (ancak sıfır ve ondalık nokta ile) başlarsa ancak ardından bu ilk sıfır kullanarak kaldırılmalıdır `Substring` işlevi.

`canExecute` Yöntemi döndürür `false` yalnızca bağımsız değişken (ondalık basıldığında olduğunu gösterir) ondalık ise ve `Entry` zaten bir ondalık noktası içerir.

Tüm `execute` yöntemlerini çağırır `RefreshCanExecutes`, sonra çağıran `ChangeCanExecute` hem `DigitCommand` ve `ClearCommand`. Bu, ondalık ve geri düğmeleri etkin veya devre dışı geçerli girilen basamak dizisi üzerinde temel sağlar.

## <a name="adding-commands-to-existing-views"></a>Varolan görünümlere komut ekleme

Komut verme arabirimi desteklemeyen görünümleri ile kullanmak istiyorsanız, bir komut ekleyip olay dönüştüren bir Xamarin.Forms davranışı kullanmak da mümkündür. Bu makalede açıklanan [ **yeniden kullanılabilir EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Zaman uyumsuz için Gezinti menüleri komut vermeye genel

Komut vermeye genel, gibi Gezinti menüleri uygulamak için kullanışlı [ **veri bağlama tanıtımları** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) kendisini program. Burada, parçası olduğu **MainPage.xaml**:


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

XAML ile komut vermeye genel kullanırken `CommandParameter` özellikleri genellikle dizeye ayarlayın. Bu durumda, ancak XAML işaretleme uzantısı kullanılır böylece `CommandParameter` türünde `System.Type`.

Her `Command` özelliğe adlı bir özellik `NavigateCommand`. Özelliği arka plan kod dosyasında tanımlanır **MainPage.xaml.cs**:

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

Oluşturucu kümeleri `NavigateCommand` özelliğini bir `execute` başlatan yöntem `System.Type` parametresi ve kendisine gider. Çünkü `PushAsync` çağrı gerektirir. bir `await` işleci `execute` yöntemi gerekir işaretlenen zaman uyumsuz olarak. Bu ile gerçekleştirilir `async` parametre listesinden önce anahtar sözcüğü.

Oluşturucu ayrıca ayarlar `BindingContext` sayfasının kendisine bağlamaları başvuru böylece `NavigateCommand` bu sınıftaki.

Bu oluşturucu kodda sırası önemlidir: `InitializeComponent` çağrı ayrıştırılmak XAML neden olur, ancak o anda adlı bir özelliğe bağlandıktan `NavigateCommand` çünkü çözümlenemiyor `BindingContext` ayarlanır `null`. Varsa `BindingContext` oluşturucuda ayarlanır *önce* `NavigateCommand` ayarlandığından, bağlama ne zaman çözümlenebilir sonra `BindingContext` ayarlanmış, ancak o zaman `NavigateCommand` hala `null`. Ayarı `NavigateCommand` sonra `BindingContext` olduğundan bağlama üzerinde hiçbir etkisi olmayacak bir değişiklik `NavigateCommand` etkinleşmez bir `PropertyChanged` olay ve bağlama değil bilmeniz, `NavigateCommand` artık geçerlidir.

Her ikisi de ayarını `NavigateCommand` ve `BindingContext` (herhangi bir sırada) çağrıdan önce `InitializeComponent` bağlamanın iki bileşenin ayarlandığından XAML ayrıştırıcı bağlama tanımı karşılaştığında çalışır.

Veri bağlama bazen zor olabilir, ancak bu makale serisi gördüğünüz gibi bunlar güçlü ve çok yönlü ve büyük ölçüde kullanıcı arabiriminden temel mantığını ayrılarak kodunuzu düzenleme şeklinizdir yardımcı.



## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölümden Xamarin.Forms kitabı](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
