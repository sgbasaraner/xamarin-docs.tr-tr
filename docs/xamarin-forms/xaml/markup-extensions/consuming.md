---
title: XAML biçimlendirme uzantıları kullanma
description: XAML biçimlendirme uzantıları Xamarin.Forms içinde kullanılabilir kullanın
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: 25eada483e8bd2ce95cb3101dfe873ea38b283ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-xaml-markup-extensions"></a>XAML biçimlendirme uzantıları kullanma

XAML işaretleme uzantılarına çeşitli kaynaklardan ayarlanacak öğesi özniteliklerini sağlayarak güç ve XAML esnekliğini artırmak yardımcı olur. Birkaç XAML işaretleme uzantılarına XAML 2009 belirtimi bir parçasıdır. Bunlar her zamanki XAML dosyalarıyla görünür `x` ad alanı öneki ve olan yaygın olarak adlandırılır bu öneki. Bunlar aşağıdaki bölümlerde açıklanmıştır:

- [`x:Static`](#static) &ndash; statik özellikler, alanlar veya numaralandırma üyeleri başvuru.
- [`x:Reference`](#reference) &ndash; öğeleri sayfada adlı başvuru.
- [`x:Type`](#type) &ndash; bir öznitelik kümesine bir `System.Type` nesnesi.
- [`x:Array`](#array) &ndash; belirli bir türdeki nesneleri içeren bir dizi oluşturun.
- [`x:Null`](#null) &ndash; bir öznitelik kümesine bir `null` değeri.

Diğer üç XAML biçimlendirme uzantıları geçmişte diğer XAML uygulamaları tarafından desteklenen ve Xamarin.Forms tarafından da desteklenir. Bunlar daha eksiksiz diğer makalelerdeki açıklanmıştır:

- `StaticResource` &ndash; makalesinde açıklandığı gibi bir kaynak sözlüğünden başvuru nesneleri [ **kaynak sözlüklerindeki**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` &ndash; bir kaynak sözlüğü nesnelerindeki değişiklikler makalesinde açıklandığı şekilde yanıt [ **dinamik stilleri**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` &ndash; makalesinde açıklandığı gibi iki nesnelerin özelliklerini arasında bir bağlantı kurarsınız [ **veri bağlama**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` &ndash; veri bağlama denetimi şablonundan makalesinde ele alındığı gibi gerçekleştirir [**denetim şablondan bağlama**] / kılavuzları/xamarin-forms/uygulama-temelleri/şablonlar/denetimi-şablonları/şablonu-bağlama /)

[ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) Düzen kullanır özel biçimlendirme uzantısı [ `ConstraintExpression` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/). Bu biçimlendirme uzantısı makalesinde açıklanan [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static İşaretleme Uzantısı

`x:Static` Biçimlendirme uzantısı tarafından desteklenen [ `StaticExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) sınıfı. Sınıf adlı tek bir özelliğe sahip [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) türü `string` ortak sabiti, statik özelliği, statik alan veya numaralandırma üyesi adına ayarlayın.

Kullanmak için ortak bir yolu `x:Static` önce bazı sabitleri veya statik değişkenler sınıfıyla gibi tanımlamaktır bu küçük `AppConstants` sınıfını [ **MarkupExtensions** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) program:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X: Static Demo** sayfa kullanmak için birkaç yol gösterir `x:Static` biçimlendirme uzantısı. En ayrıntılı yaklaşım başlatır `StaticExtension` arasında sınıf `Label.FontSize` özellik öğesi etiketleri:

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

XAML ayrıştırıcısı de sağlar `StaticExtension` olarak kısaltılır sınıfı `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Bu daha basit hale getirilebilir, ancak bazı yeni sözdizimi değişikliği sunar: koyma oluşur `StaticExtension` sınıfı ve kaşlı ayraç ayarı üye. Elde edilen ifadesi doğrudan ayarlamak `FontSize` özniteliği:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Olduğuna dikkat edin *hiçbir* tırnak süslü ayraçlar içinde. `Member` Özelliği `StaticExtension` artık bir XML özniteliği değil. Bu, bunun yerine ifade biçimlendirme uzantısı için bir parçasıdır.

Kısaltma gibi `x:StaticExtension` için `x:Static` bir nesne öğesi olarak kullandığınızda, ayrıca, süslü ayraçlar içinde ifadesinde kısaltma:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension` Sınıfına sahip bir `ContentProperty` özelliğe başvurma özniteliği `Member`, bu özelliği sınıfın varsayılan içerik özelliği olarak işaretler. Süslü ayraçlar ile ifade XAML işaretleme uzantılarına, ortadan kaldırabileceğiniz `Member=` ifade parçası:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Bu en yaygın biçimidir `x:Static` biçimlendirme uzantısı.

**Statik Demo** sayfası, diğer iki örnek içerir. .NET için bir XML ad alanı bildirimi XAML dosyasının kök etiketi içeren `System` ad alanı:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Böylece `Label` statik alanın ayarlanacağı yazı tipi boyutu `Math.PI`. Bunun yerine küçük metinde sonuçları böylece `Scale` özelliği ayarlanmış `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Son örnek görüntüler `Device.RuntimePlatform` değeri. `Environment.NewLine` Statik özelliğe yeni satır karakteri ikisi arasındaki eklemek için kullanılan `Span` nesneler:

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

Aşağıda, tüm üç platformlarda çalışan örnek verilmiştir:

[![x: Static Demo](consuming-images/staticdemo-small.png "x: Static Demo")](consuming-images/staticdemo-large.png#lightbox "x: Static Tanıtımı")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference İşaretleme Uzantısı

`x:Reference` Biçimlendirme uzantısı tarafından desteklenen [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) sınıfı. Sınıf adlı tek bir özelliğe sahip [ `Name` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.ReferenceExtension.Name/) türü `string` bir adla verilen sayfasında bir öğe adı için ayarladığınız `x:Name`. Bu `Name` içerik özelliği bir özelliktir `ReferenceExtension`, bu nedenle `Name=` ne zaman gerekli değil `x:Reference` süslü ayraçlar içinde görüntülenir.

`x:Reference` Biçimlendirme uzantısı makalesinde daha ayrıntılı açıklanan özel olarak veri bağlamaları olarak kullanılan [ **veri bağlama**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**X: Reference Demo** sayfası iki kullanımını gösterir `x:Reference` veri bağlamalarla nerede kullanıldığı ayarlamak için ilk `Source` özelliği `Binding` nesne ve burada bunu ayarlamak için kullanılan ikinci `BindingContext` Özellik iki veri bağlamaları için:

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

Her ikisi de `x:Reference` ifadeleri kullanma kısaltılmış `ReferenceExtension` sınıfı adı ve ortadan `Name=` ifade parçası. İlk örnekteki `x:Reference` biçimlendirme uzantısı katıştırılmış `Binding` biçimlendirme uzantısı. Dikkat `Source` ve `StringFormat` ayarları virgülle ayrılır. Aşağıda, tüm üç platformlarında çalışan program verilmiştir:

[![x:Reference Demo](consuming-images/referencedemo-small.png "x:Reference Demo")](consuming-images/referencedemo-large.png#lightbox "x:Reference Demo")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type İşaretleme Uzantısı

`x:Type` Biçimlendirme uzantısıdır XAML denk C# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) anahtar sözcüğü. Tarafından desteklenen [ `TypeExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) adlı bir özellik tanımlıyor sınıfı [ `TypeName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.TypeExtension.TypeName/) türü `string` bir sınıf veya yapı adına ayarlayın. `x:Type` Biçimlendirme uzantısı döndürür [ `System.Type` ](https://developer.xamarin.com/api/type/System.Type/) Bu sınıf veya yapı nesne. `TypeName` içerik özelliği `TypeExtension`, bu nedenle `TypeName=` ne zaman gerekli değil `x:Type` ile süslü ayraçlar görüntülenir.

Xamarin.Forms içinde bağımsız değişken türü içeren birkaç özellik vardır `Type`. Örnekler [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) özelliği `Style`ve [x: TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) genel sınıflarda bağımsız değişkenlerini belirtmek için kullanılan öznitelik. Ancak, XAML ayrıştırıcısı gerçekleştirir `typeof` işlemi otomatik olarak ve `x:Type` biçimlendirme uzantısı şu durumlarda kullanılmaz.

Tek bir yerde nerede `x:Type` *olan* gerekli olan `x:Array` açıklanan biçimlendirme uzantısı [sonraki bölümde](#array).

`x:Type` Biçimlendirme uzantısı olduğunda da yararlı her bir menü öğesi için belirli bir türdeki bir nesne burada karşılık gelen bir menü oluşturma. İlişkilendirebilirsiniz bir `Type` nesne her Menü öğesiyle ve menü öğesi seçildiğinde nesne örneği.

Bunun nasıl Gezinti menüsünde `MainPage` içinde **biçimlendirme uzantıları** program çalışır. **MainPage.xaml** dosyasını içeren bir `TableView` her `TextCell` belirli bir programı sayfasında karşılık gelen:

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

Ana sayfası açılıyor işte **biçimlendirme uzantıları**:

[![Ana sayfa](consuming-images/mainpage-small.png "ana sayfa")](consuming-images/mainpage-large.png#lightbox "ana sayfası")

Her `CommandParameter` özelliği ayarlanmış bir `x:Type` diğer sayfalardan birini başvuran biçimlendirme uzantısı. `Command` Özelliği adlı bir özelliğe bağlı `NavigateCommand`. Bu özellik tanımlanan `MainPage` arka plan kod dosyası:

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

`NavigateCommand` Özelliği bir `Command` türünde bir bağımsız değişken içeren bir yürütme komutu uygulayan nesne `Type` &mdash; değerini `CommandParameter`. Bir yöntem `Activator.CreateInstance` sayfa örneği oluşturmak için ve kendisine gider. Ayarlayarak Oluşturucusu sonucuna `BindingContext` kendisine sayfasının sağlayan `Binding` üzerinde `Command` çalışmak için. Bkz: [ **veri bağlama** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) makale ve özellikle [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) kod bu tür hakkında daha fazla ayrıntı için makale.

**X: Type Demo** sayfa kullanan benzer bir teknik Xamarin.Forms öğeleri oluşturmak ve bunları eklemek için bir `StackLayout`. XAML dosyası başlangıçta üç oluşur `Button` öğeleriyle kendi `Command` özelliklerini ayarlamak bir `Binding` ve `CommandParameter` üç Xamarin.Forms görünüm türleri için ayarlanan özellikleri:

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

Arka plan kodu dosya tanımlar ve başlatır `CreateCommand` özelliği:

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

Yöntemi yürütülmesi bir `Button` basıldığında bağımsız değişkeni yeni bir örneğini oluşturur, ayarlar, `VerticalOptions` özelliği ve ona ekler `StackLayout`. Üç `Button` öğeleri dinamik olarak oluşturulan görünümlerle sonra sayfanın paylaşımı:

[![x:Type Demo](consuming-images/typedemo-small.png "x:Type Demo")](consuming-images/typedemo-large.png#lightbox "x:Type Demo")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array İşaretleme Uzantısı

`x:Array` Biçimlendirme uzantısı bir dizi biçimlendirmede tanımlamanıza izin verir. Tarafından desteklenen [ `ArrayExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) iki özelliklerini tanımlar sınıfı:

- `Type` tür `Type`, dizideki öğeler türünü gösterir.
- `Items` tür `IList`, öğelerinin koleksiyonu. Bu içerik özelliğidir `ArrayExtension`.

`x:Array` Biçimlendirme uzantısı kendisini hiçbir zaman süslü ayraçlar içinde görüntülenir. Bunun yerine, `x:Array` başlangıç ve bitiş etiketleri sınırlandırmak öğeleri listesi. Ayarlama `Type` özelliğine bir `x:Type` biçimlendirme uzantısı.

**X: Array Demo** sayfa nasıl kullanılacağını gösterir `x:Array` öğelerine eklemek için bir `ListView` ayarlayarak `ItemsSource` bir dizi özellik:

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

`ViewCell` Basit bir oluşturur `BoxView` her renk girişi için:

[![x: Array Demo](consuming-images/arraydemo-small.png "x: Array Demo")](consuming-images/arraydemo-large.png#lightbox "x: Array Tanıtımı")

Tek tek belirtmek için çeşitli yollar vardır `Color` Bu dizideki öğeler. Kullanabileceğiniz bir `x:Static` biçimlendirme uzantısı:

```xaml
<x:Static Member="Color.Blue" />
```

Veya, kullanabileceğiniz `StaticResource` kaynak sözlükten bir renk almak için:

```xaml
<StaticResource Key="myColor" />
```

Bu makalenin sonundaki ayrıca yeni bir renk değeri oluşturur özel bir XAML biçimlendirme uzantısı görürsünüz:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Dizeyi veya sayı gibi ortak türlerin dizileri tanımlarken, listelenen etiketleri kullanma [ **oluşturucu bağımsız değişkenleri geçirme** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) değerleri sınırlandırmak için makale.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null İşaretleme Uzantısı

`x:Null` Biçimlendirme uzantısı tarafından desteklenen [ `NullExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) sınıfı. Özelliği yok ve yalnızca XAML C# eşdeğerdir [ `null` ](/dotnet/csharp/language-reference/keywords/null/) anahtar sözcüğü.

`x:Null` Biçimlendirme uzantısı nadiren gerekir ve nadiren kullanılır, ancak bir gereksinimini bulursanız, mevcut memnun olacaktır.

**X: Null Demo** sayfa bir senaryo gösterilmektedir, `x:Null` kullanışlı olabilir. Örtülü tanımladığınız varsayalım `Style` için `Label` içeren bir `Setter` ayarlayan `FontFamily` özelliği bir platforma bağımlı aile adı:

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

Aşağıdakilerden birini olduğunu fark sonra `Label` öğeleri, tüm özellik ayarlarını örtük istediğiniz `Style` dışında `FontFamily`, varsayılan değeri olması istediğiniz. Başka bir tanımlayabilirsiniz `Style` bu amaçla ancak basit bir yaklaşım için basitçe ayarlamaktır `FontFamily` belirli özellik `Label` için `x:Null`Center'da gösterildiği gibi `Label`.

Üç platformlarda çalışan program şöyledir:

[![x: Null Demo](consuming-images/nulldemo-small.png "x: Null Demo")](consuming-images/nulldemo-large.png#lightbox "x: Null Tanıtımı")

Duyuru bu dört `Label` öğelerine sahip bir serif yazı tipi Merkezi `Label` varsayılan sans-serif yazı tipi vardır.

## <a name="define-your-own-markup-extensions"></a>Kendi biçimlendirme uzantıları tanımlayın

Xamarin.Forms içinde bulunmayan XAML biçimlendirme uzantısı gereksinimini karşılaştıysanız yapabilecekleriniz [kendinizinkini oluşturun](creating.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Biçimlendirme uzantıları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML biçimlendirme uzantıları bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Kaynak Sözlükler](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dinamik Stiller](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Veri Bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md)
