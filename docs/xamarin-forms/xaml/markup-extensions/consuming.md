---
title: XAML biçimlendirme uzantılarını kullanma
description: Bu makalede, çeşitli kaynaklardan ayarlanacak öğenin öznitelikleri vererek gücü ve esnekliği XAML geliştirmek için Xamarin.Forms XAML biçimlendirme uzantıları kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2018
ms.openlocfilehash: e483716952aa97de4411733006f4fa12c3e6da98
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "39393678"
---
# <a name="consuming-xaml-markup-extensions"></a>XAML biçimlendirme uzantılarını kullanma

XAML biçimlendirme uzantıları, özel olarak çeşitli kaynaklardan ayarlanacak öğenin öznitelikleri vererek gücü ve esnekliği XAML artırmaya yardımcı olun. Birkaç XAML biçimlendirme uzantıları XAML 2009 belirtiminin bir parçasıdır. Bunlar her zamanki XAML dosyalarıyla görünür `x` çalıştırdığınız ve ad alanı öneki yaygın olarak adlandırılan bu öneki. Bu makalede aşağıdaki biçimlendirme uzantıları açıklanmaktadır:

- [`x:Static`](#static) – statik özellikler, alanlar veya numaralandırma üyelerini başvuru.
- [`x:Reference`](#reference) – sayfadaki öğeleri adlı başvuru.
- [`x:Type`](#type) – bir özniteliğini bir `System.Type` nesne.
- [`x:Array`](#array) – belirli bir türdeki nesneleri içeren bir dizi oluşturun.
- [`x:Null`](#null) – bir özniteliğini bir `null` değeri.
- [`OnPlatform`](#onplatform) -platform başına temelinde kullanıcı Arabirimi özelleştirme.
- [`OnIdiom`](#onidiom) – deyim uygulamanın çalıştığı cihazın temel kullanıcı arabirimini özelleştirme.

Ek XAML biçimlendirme uzantıları geçmişe yönelik olarak diğer XAML uygulamaları tarafından desteklenen ve Xamarin.Forms tarafından da desteklenir. Bu, diğer makalelerden daha ayrıntılı açıklanmıştır:

- `StaticResource` &ndash; makalesinde açıklandığı gibi nesneleri bir kaynak sözlüğünden başvuru [ **kaynak sözlükleri**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` &ndash; makalesinde açıklandığı gibi nesneleri bir kaynak sözlüğünde değişikliklere yanıt [ **dinamik stiller**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` &ndash; makalesinde açıklandığı gibi iki nesnelerin özelliklerini arasında bir bağlantı kurmak [ **veri bağlama**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` &ndash; veri bağlama denetimi şablondan makalesinde açıklandığı gibi gerçekleştirir [ **denetim şablondan bağlama**](/guides/xamarin-forms/application-fundamentals/templates/control-templates/template-binding/).

[ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) Düzeni kullanır özel biçimlendirme uzantısı [ `ConstraintExpression` ](xref:Xamarin.Forms.ConstraintExpression). Bu işaretleme uzantısı makalesinde açıklanan [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static İşaretleme Uzantısı

`x:Static` İşaretleme uzantısı tarafından desteklenen [ `StaticExtension` ](xref:Xamarin.Forms.Xaml.StaticExtension) sınıfı. Sınıf adlı tek bir özelliğe sahip [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) türü `string` genel sabiti, statik özelliği, statik alanı veya numaralandırma üyesi adına ayarlayın.

Kullanmak için sık kullanılan yöntemlerden birisi `x:Static` ilk gibi bazı sabitleri veya statik değişkenler, bir sınıf tanımlamak için bu küçük `AppConstants` sınıfını [ **Markupextension'lar** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) programı:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X: Static tanıtım** sayfa kullanmanın çeşitli yollarını gösterir `x:Static` işaretleme uzantısı. En ayrıntılı yaklaşım başlatır `StaticExtension` arasında sınıf `Label.FontSize` özellik öğesi etiketleri:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

XAML ayrıştırıcı de tanır `StaticExtension` sınıfı olarak kısaltılır `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Bu daha basit hale getirilebilir, ancak bazı yeni söz dizimini değişikliği içeriyor: yerleştirme oluşur `StaticExtension` sınıf ve üye ayraç içindeki ayarlama. Sonuçta elde edilen ifade doğrudan ayarlanır `FontSize` özniteliği:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Olduğuna dikkat edin *hiçbir* tırnak küme ayraçları içinde. `Member` Özelliği `StaticExtension` artık bir XML özniteliği değil. Bu, bunun yerine ifade işaretleme uzantısı için bir parçasıdır.

Kısaltabilirsiniz gibi `x:StaticExtension` için `x:Static` bir nesne öğesi olarak kullandığınızda, ayrıca, ayraç ifadesindeki kısaltabilirsiniz:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension` Sınıfında bir `ContentProperty` özelliğe başvurma özniteliği `Member`, bu özelliği sınıfın varsayılan içerik özelliği olarak işaretler. Küme ayracı ile ifade edilen XAML biçimlendirme uzantıları için ortadan kaldırabilir `Member=` ifadesinin parçası:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Bu en yaygın biçimindedir `x:Static` işaretleme uzantısı.

**Statik tanıtım** sayfa iki örnek içerir. XAML dosyasının kök etiketi, .NET için bir XML ad alanı bildirimi içeren `System` ad alanı:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Böylece `Label` statik alanı ayarlamak için yazı tipi boyutu `Math.PI`. Çok küçük metinde sonuçları böylece `Scale` özelliği `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Son örnek görüntüler `Device.RuntimePlatform` değeri. `Environment.NewLine` Statik özellik bir yeni satır karakteri ikisi arasındaki eklemek için kullanılan `Span` nesneler:

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Üç tüm platformlarda çalışan örneği aşağıdadır:

[![x: Static tanıtım](consuming-images/staticdemo-small.png "x: Static tanıtım")](consuming-images/staticdemo-large.png#lightbox "x: Static Tanıtımı")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference İşaretleme Uzantısı

`x:Reference` İşaretleme uzantısı tarafından desteklenen [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) sınıfı. Sınıf adlı tek bir özelliğe sahip [ `Name` ](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) türü `string` bir ad ile belirtilen sayfadaki bir öğeyi adına ayarlayın `x:Name`. Bu `Name` özelliktir içerik özelliğinin `ReferenceExtension`, bu nedenle `Name=` ne zaman gerekli değildir `x:Reference` kaşlı ayraçlar içinde görünür.

`x:Reference` İşaretleme uzantısı makalesinde daha ayrıntılı anlatılan özel veri bağlamaları ile kullanılan [ **veri bağlama**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**X: Reference tanıtım** sayfası iki kullanımlarını gösterir `x:Reference` veri bağlamaları ile kullanıldığı ayarlamak için ilk `Source` özelliği `Binding` nesne ve onu kullanıldığı ayarlamak için ikinci `BindingContext` iki veri bağlamaları için özellik:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Her ikisi de `x:Reference` ifadeleri kullanma kısaltılmış `ReferenceExtension` sınıf adı ve ortadan `Name=` ifadesinin parçası. İlk örnekte, `x:Reference` işaretleme uzantısı gömüldüğü `Binding` işaretleme uzantısı. Dikkat `Source` ve `StringFormat` ayarları virgülle ayrılır. Üç tüm platformlarda çalışan bir program şöyledir:

[![x: Reference tanıtım](consuming-images/referencedemo-small.png "x: Reference tanıtım")](consuming-images/referencedemo-large.png#lightbox "x: Reference Tanıtımı")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type İşaretleme Uzantısı

`x:Type` İşaretleme uzantısı, C# XAML aynıdır [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) anahtar sözcüğü. Tarafından desteklenen [ `TypeExtension` ](xref:Xamarin.Forms.Xaml.TypeExtension) adlı bir özellik tanımlayan sınıf [ `TypeName` ](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) türü `string` bir sınıf veya yapı adı ayarlayın. `x:Type` İşaretleme uzantısı döndürür [ `System.Type` ](xref:System.Type) o sınıfın veya yapının nesne. `TypeName` içerik özelliği `TypeExtension`, bu nedenle `TypeName=` ne zaman gerekli değildir `x:Type` küme ayracı ile görünür.

Xamarin.Forms içinde türünde bağımsız değişken içeren birkaç özellik vardır `Type`. Örnekler [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) özelliği `Style`ve [x: TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) genel sınıfları bağımsız değişkenlerini belirtmek için kullanılan öznitelik. Ancak, XAML ayrıştırıcı gerçekleştirir `typeof` işlemi otomatik olarak ve `x:Type` işaretleme uzantısı, şu durumlarda kullanılmaz.

Tek bir yerde nerede `x:Type` *olduğu* gerekli olduğu `x:Array` açıklanan işaretleme uzantısı [sonraki bölümde](#array).

`x:Type` İşaretleme uzantısı, ayrıca her bir menü öğesi belli bir türdeki bir nesne için burada karşılık gelen bir menü oluşturma gerektiğinde kullanışlıdır. İlişkilendirebilirsiniz bir `Type` ile her bir menü öğesi nesnesi ve menü öğesi seçildiğinde nesne örneği oluşturun.

Bu, nasıl Gezinti menüsünde `MainPage` içinde **biçimlendirme uzantıları** programı çalışır. **MainPage.xaml** dosyasını içeren bir `TableView` her `TextCell` programı belirli bir sayfaya karşılık gelen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

Açılış ana sayfasında işte **biçimlendirme uzantıları**:

[![Ana sayfa](consuming-images/mainpage-small.png "ana sayfa")](consuming-images/mainpage-large.png#lightbox "ana sayfası")

Her `CommandParameter` özelliği bir `x:Type` diğer sayfalardan biri başvuruda işaretleme uzantısı. `Command` Özelliğe adlı bir özellik `NavigateCommand`. Bu özellik tanımlanan `MainPage` arka plan kod dosyası:

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

`NavigateCommand` Özelliği bir `Command` türünde bir bağımsız değişken içeren bir yürütme komutu uygulayan nesne `Type` &mdash; değerini `CommandParameter`. Yöntemini kullanan `Activator.CreateInstance` sayfası oluşturmak için ve kendisine gider. Oluşturucu ayarlayarak sonucuna `BindingContext` sayfanın kendisi sağlayan `Binding` üzerinde `Command` çalışmak için. Bkz: [ **veri bağlama** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) makale ve özellikle [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) makalede bu tür kod hakkında daha fazla ayrıntı için.

**X: Type tanıtım** sayfası Xamarin.Forms öğeleri oluşturmak ve bunları eklemek için benzer bir teknik kullanır bir `StackLayout`. XAML dosyası başlangıçta üç oluşur `Button` öğelerle kendi `Command` özelliklerini ayarlamak bir `Binding` ve `CommandParameter` üç Xamarin.Forms görünüm türleri için ayarlanan özellikler:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

Arka plan kod dosyasını tanımlar ve başlatır `CreateCommand` özelliği:

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

Yöntemi zaman yürütülen bir `Button` basıldığında bağımsız değişkeni yeni bir örneğini oluşturur, ayarlar kendi `VerticalOptions` özelliği ve bu gruba ekler `StackLayout`. Üç `Button` öğeleri daha sonra sayfa dinamik olarak oluşturulan görünümleriyle paylaşın:

[![x: Type tanıtım](consuming-images/typedemo-small.png "x: Type tanıtım")](consuming-images/typedemo-large.png#lightbox "x: Type Tanıtımı")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array İşaretleme Uzantısı

`x:Array` İşaretleme uzantısı bir dizi biçimlendirme içinde tanımlamanıza izin verir. Tarafından desteklenen [ `ArrayExtension` ](xref:Xamarin.Forms.Xaml.ArrayExtension) iki özellikleri tanımlayan sınıf:

- `Type` tür `Type`, dizideki öğelerin türünü belirtir.
- `Items` tür `IList`, öğeleri koleksiyonu. Bu içerik özelliğidir `ArrayExtension`.

`x:Array` İşaretleme uzantısı kendisi asla kaşlı ayraçlar içinde görünür. Bunun yerine, `x:Array` başlangıç ve bitiş etiketleri öğe listesi sınırlandırın. Ayarlama `Type` özelliğini bir `x:Type` işaretleme uzantısı.

**X: Array tanıtım** sayfasını nasıl kullanılacağını göstermektedir `x:Array` öğelerine eklemek için bir `ListView` ayarlayarak `ItemsSource` bir dizi özelliği:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

`ViewCell` Basit oluşturur `BoxView` her renk girişi için:

[![x: Array tanıtım](consuming-images/arraydemo-small.png "x: Array tanıtım")](consuming-images/arraydemo-large.png#lightbox "x: Array Tanıtımı")

Tek tek belirtmek için çeşitli yollar vardır `Color` dizideki öğeleri. Kullanabileceğiniz bir `x:Static` işaretleme uzantısı:

```xaml
<x:Static Member="Color.Blue" />
```

Veya, kullanabileceğiniz `StaticResource` kaynak sözlüğünden bir renk almak için:

```xaml
<StaticResource Key="myColor" />
```

Bu makale sonuna doğru ayrıca yeni bir renk değeri oluşturan özel bir XAML işaretleme uzantısı görürsünüz:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Dize veya sayı gibi ortak bir türleri dizilerini tanımlarken, listelenen etiketler kullanma [ **oluşturucu bağımsız değişkenleri geçirme** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) değerleri sınırlandırmak için makale.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null İşaretleme Uzantısı

`x:Null` İşaretleme uzantısı tarafından desteklenen [ `NullExtension` ](xref:Xamarin.Forms.Xaml.NullExtension) sınıfı. Özellikleri yoktur ve yalnızca XAML C# eşdeğerdir [ `null` ](/dotnet/csharp/language-reference/keywords/null/) anahtar sözcüğü.

`x:Null` İşaretleme uzantısı nadiren gerekir ve nadiren kullanılır, ancak bir gereksinimini bulursanız, var olan memnun olacaktır.

**X: Null tanıtım** sayfası bir senaryo gösterilmektedir, `x:Null` kullanışlı olabilir. Örtük tanımladığınız varsayalım `Style` için `Label` içeren bir `Setter` ayarlayan `FontFamily` özelliğini bir platforma bağımlı aile adı:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

Biri için olduğunu fark sonra `Label` öğeleri, örtük olarak tüm özellik ayarları istediğiniz `Style` dışında `FontFamily`, varsayılan değer olmasını istediğiniz. Başka bir tanımlayabilirsiniz `Style` bu amaçla ancak basit bir yaklaşım için ayarlanacak teknolojidir `FontFamily` belirli özellik `Label` için `x:Null`Merkezi'nde gösterilen gibi `Label`.

Üç platformlarda çalışan bir program şöyledir:

[![x: Null tanıtım](consuming-images/nulldemo-small.png "x: Null tanıtım")](consuming-images/nulldemo-large.png#lightbox "x: Null Tanıtımı")

Bildirim, dört `Label` öğelere sahip bir serif yazı tipi, merkezi `Label` varsayılan sans-serif yazı tipi.

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>OnPlatform işaretleme uzantısı

`OnPlatform` İşaretleme uzantısı platform başına temelinde UI görünümünü özelleştirmenize olanak sağlar. Aynı işlevselliği sağlar [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) ve [ `On` ](xref:Xamarin.Forms.On) sınıfları, ancak daha kısa bir gösterim.

`OnPlatform` İşaretleme uzantısı tarafından desteklenen [ `OnPlatformExtension` ](xref:Xamarin.Forms.Xaml.OnPlatformExtension) aşağıdaki özellikleri tanımlayan sınıf:

- `Default` tür `object`, platformları temsil eden özellikler uygulanacak varsayılan bir değere ayarlayın.
- `Android` tür `object`, Android'de uygulanacak bir değere ayarlanmış.
- `GTK` tür `object`, GTK platformlarda uygulanacak bir değere ayarlanmış.
- `iOS` tür `object`, İos'ta uygulanacak bir değere ayarlanmış.
- `macOS` tür `object`, macOS üzerinde uygulanacak bir değere ayarlanmış.
- `Tizen` tür `object`, Tizen platformunda uygulanacak bir değere ayarlanmış.
- `UWP` tür `object`, Evrensel Windows platformu üzerinde uygulanacak bir değere ayarlanmış.
- `WPF` tür `object`, Windows Presentation Foundation platformunda uygulanacak bir değere ayarlanmış.
- `Converter` tür `IValueConverter`, ayarladığınız bir `IValueConverter` uygulaması.
- `ConverterParameter` tür `object`, geçirmek için bir değere ayarlanmış `IValueConverter` uygulaması.

> [!NOTE]
> XAML ayrıştırıcı sağlayan [ `OnPlatformExtension` ](xref:Xamarin.Forms.Xaml.OnPlatformExtension) sınıfı olarak kısaltılır `OnPlatform`.

`Default` Özelliktir içerik özelliğinin `OnPlatformExtension`. Bu nedenle, küme ayracı ile ifade edilen XAML biçimlendirme ifadeler için çıkarabilirsiniz `Default=` ilk bağımsız değişken olması şartıyla ifade parçası.

> [!IMPORTANT]
> XAML ayrıştırıcı doğru türde değerler tüketen özellikleri için sağlanan bekliyor `OnPlatform` işaretleme uzantısı. Tür dönüştürme gerekliyse, `OnPlatform` işaretleme uzantısı Xamarin.Forms tarafından sağlanan varsayılan dönüştürücüleri kullanarak gerçekleştirmeyi deneyecek. Ancak, varsayılan dönüştürücüleri ve bu gibi durumlarda gerçekleştirilemiyor bazı tür dönüştürmeleri vardır `Converter` özelliği ayarlanmalıdır bir `IValueConverter` uygulaması.

**OnPlatform tanıtım** sayfasını nasıl kullanılacağını göstermektedir `OnPlatform` işaretleme uzantısı:

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

Bu örnekte, üç `OnPlatform` ifadeleri kullanma kısaltılmış `OnPlatformExtension` sınıf adı. Üç `OnPlatform` biçimlendirme uzantıları kümesi [ `Color` ](xref:Xamarin.Forms.BoxView.Color), [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest), ve [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) özelliklerini [ `BoxView` ](xref:Xamarin.Forms.BoxView) iOS, Android ve UWP üzerindeki farklı değerler. Biçimlendirme uzantıları, sorununu ortadan kaldırırken belirtilmeyen platformları üzerinde bu özellikler için varsayılan değerleri de sağlar. `Default=` ifadesinin parçası. Ayarlanan işaretleme uzantısı özellikleri virgüllerle ayrılır dikkat edin.

Üç tüm platformlarda çalışan bir program şöyledir:

[![OnPlatform tanıtım](consuming-images/onplatformdemo-small.png "OnPlatform tanıtım")](consuming-images/onplatformdemo-large.png#lightbox "OnPlatform Tanıtımı")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>OnIdiom işaretleme uzantısı

`OnIdiom` Biçimlendirme uzantıları uygulamanın çalıştığı cihazın deyim temel kullanıcı arabirimini özelleştirme olanak tanır. Tarafından desteklenen [ `OnIdiomExtension` ](xref:Xamarin.Forms.Xaml.OnIdiomExtension) aşağıdaki özellikleri tanımlayan sınıf:

- `Default` tür `object`, cihaz deyimleri temsil eden özellikler uygulanacak varsayılan bir değere ayarlayın.
- `Phone` tür `object`, telefonlarda uygulanacak bir değere ayarlanmış.
- `Tablet` tür `object`, tabletlerde uygulanacak bir değere ayarlanmış.
- `Desktop` tür `object`, Masaüstü platformlarda uygulanacak bir değere ayarlanmış.
- `TV` tür `object`, TV platformlarda uygulanacak bir değere ayarlanmış.
- `Watch` tür `object`, Watch platformlarda uygulanacak bir değere ayarlanmış.
- `Converter` tür `IValueConverter`, ayarladığınız bir `IValueConverter` uygulaması.
- `ConverterParameter` tür `object`, geçirmek için bir değere ayarlanmış `IValueConverter` uygulaması.

> [!NOTE]
> XAML ayrıştırıcı sağlayan [ `OnIdiomExtension` ](xref:Xamarin.Forms.Xaml.OnIdiomExtension) sınıfı olarak kısaltılır `OnIdiom`.

`Default` Özelliktir içerik özelliğinin `OnIdiomExtension`. Bu nedenle, küme ayracı ile ifade edilen XAML biçimlendirme ifadeler için çıkarabilirsiniz `Default=` ilk bağımsız değişken olması şartıyla ifade parçası.

> [!IMPORTANT]
> XAML ayrıştırıcı doğru türde değerler tüketen özellikleri için sağlanan bekliyor `OnIdiom` işaretleme uzantısı. Tür dönüştürme gerekliyse, `OnIdiom` işaretleme uzantısı Xamarin.Forms tarafından sağlanan varsayılan dönüştürücüleri kullanarak gerçekleştirmeyi deneyecek. Ancak, varsayılan dönüştürücüleri ve bu gibi durumlarda gerçekleştirilemiyor bazı tür dönüştürmeleri vardır `Converter` özelliği ayarlanmalıdır bir `IValueConverter` uygulaması.

**OnIdiom tanıtım** sayfasını nasıl kullanılacağını göstermektedir `OnIdiom` işaretleme uzantısı:

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

Bu örnekte, üç `OnIdiom` ifadeleri kullanma kısaltılmış `OnIdiomExtension` sınıf adı. Üç `OnIdiom` biçimlendirme uzantıları kümesi [ `Color` ](xref:Xamarin.Forms.BoxView.Color), [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest), ve [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) özelliklerini [ `BoxView` ](xref:Xamarin.Forms.BoxView) telefon, tablet ve Masaüstü deyimleri üzerindeki farklı değerler. Biçimlendirme uzantıları, sorununu ortadan kaldırırken belirtilmeyen deyimleri üzerinde bu özellikler için varsayılan değerleri de sağlar. `Default=` ifadesinin parçası. Ayarlanan işaretleme uzantısı özellikleri virgüllerle ayrılır dikkat edin.

Üç tüm platformlarda çalışan bir program şöyledir:

[![OnIdiom tanıtım](consuming-images/onidiomdemo-small.png "OnIdiom tanıtım")](consuming-images/onidiomdemo-large.png#lightbox "OnIdiom Tanıtımı")

## <a name="define-your-own-markup-extensions"></a>Kendi biçimlendirme uzantılarını tanımla

Xamarin.Forms içinde kullanılamayan bir XAML işaretleme uzantısı gereksinimini karşılaştığınız varsa [kendi uzantınızı oluşturun](creating.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Biçimlendirme uzantıları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML biçimlendirme uzantıları Xamarin.Forms kitabı bölümden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Kaynak Sözlükler](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dinamik Stiller](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Veri Bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md)
