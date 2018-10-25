---
title: Geçişli stil sayfaları (CSS) kullanarak Xamarin.Forms uygulamalarında stil oluşturma
description: Xamarin.Forms, geçişli stil sayfaları (CSS) kullanılarak stil görsel öğeleri destekler.
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 09/28/2018
ms.openlocfilehash: 72bb4c359717f419eb500d471fe436d1ca195ae6
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "34794091"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Geçişli stil sayfaları (CSS) kullanarak Xamarin.Forms uygulamalarında stil oluşturma

_Xamarin.Forms, geçişli stil sayfaları (CSS) kullanılarak stil görsel öğeleri destekler._

CSS kullanarak Xamarin.Forms uygulamaları biçimlendirilebilir. Bir stil sayfası kuralları, bir veya daha fazla Seçici ve bir bildirimi blok oluşan her bir kural ile bir listesini içerir. Bir bildirimi blok küme ayraçları, bildirimlerinde bir özellik, bir iki nokta üst üste ve bir değer oluşan her bildirimiyle listesini oluşur. Bir blok içinde birden fazla bildirimi olduğunda, noktalı virgül ayırıcı olarak eklenir. Aşağıdaki kod örneği, bazı Xamarin.Forms uyumlu CSS gösterir:

```css
navigationpage {
    -xf-bar-background-color: lightgray;
}

^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

Xamarin.Forms, CSS stil sayfaları ayrıştırılır ve derleme zamanı yerine çalışma zamanı sırasında değerlendirilen ve stil sayfalarını kullanmak üzere yeniden ayrıştırılmış.

> [!NOTE]
> Şu anda tüm XAML stil ile mümkündür stil CSS ile gerçekleştirilemiyor. Ancak, XAML stiller CSS Xamarin.Forms tarafından şu anda desteklenmeyen özellikleri desteklemek için kullanılabilir. XAML stilleri hakkında daha fazla bilgi için bkz. [stil XAML stilleri kullanarak Xamarin.Forms uygulamalarında](~/xamarin-forms/user-interface/styles/xaml/index.md).

[MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) örnek CSS stil basit bir uygulama kullanmayı gösterir ve aşağıdaki ekran görüntülerinde gösterilir:

[![CSS stil MonkeyApp ana sayfayla](css-images/MonkeyAppMainPage.png "MonkeyApp ana sayfa ile CSS stil")](css-images/MonkeyAppMainPage-Large.png#lightbox "CSS stil MonkeyApp ana sayfası")

[![CSS stil ile MonkeyApp ayrıntı sayfası](css-images/MonkeyAppDetailPage.png "MonkeyApp ayrıntı sayfası ile CSS stil")](css-images/MonkeyAppDetailPage-Large.png#lightbox "MonkeyApp ayrıntı sayfası ile CSS stil oluşturma")

## <a name="consuming-a-style-sheet"></a>Bir stil sayfası kullanma

Bir stil sayfası bir çözüme ekleme işlemi aşağıdaki gibidir:

1. Boş bir CSS dosyası .NET Standard kitaplığı projenize ekleyin.
1. Ayarlamak için CSS dosyasının derleme eylemini **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Bir stil sayfası yükleniyor

Bir stil sayfası yüklemek için kullanılan yaklaşım vardır.

### <a name="xaml"></a>XAML

Bir stil sayfası yüklendi ve ile ayrıştırıldığında [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) eklenmeden önce sınıfı bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

[ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) Özellik belirtir stil sayfası kapsayan XAML dosyasının konumunu göre veya projenin köküne bir URI olarak URI ile başlarsa bir `/`.

> [!WARNING]
> CSS dosyasının derleme eylemini olarak ayarlanmazsa, yüklenemeyecek **EmbeddedResource**.

Alternatif olarak, bir stil sayfası yüklenebilir ve ile ayrıştırılmış [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) eklenmeden önce sınıfı, bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)kullanarak, satır içi kullanım içinde bir `CDATA` bölümü:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Kaynak sözlükleri hakkında daha fazla bilgi için bkz: [kaynak sözlükleri](~/xamarin-forms/xaml/resource-dictionaries.md).

### <a name="c"></a>C#

İçinde C#, stil sayfası bir gömülü kaynak yüklenebilir ve eklenen bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        this.Resources.Add(StyleSheet.FromAssemblyResource(
            IntrospectionExtensions.GetTypeInfo(typeof(MyPage)).Assembly,
            "MyProject.Assets.styles.css"));
    }
}
```

İlk bağımsız değişkeni `StyleSheet.FromAssemblyResource` yöntemi stil sayfası içeren derlemenin, ikinci bağımsız değişkeni bir `string` temsil eden kaynak tanımlayıcısı. Kaynak tanımlayıcısı örneğinden alınabilen **özellikleri** CSS dosyası seçildiğinde penceresi.

Alternatif olarak, bir stil sayfası ndan yüklenebilen bir `StringReader` ve eklenen bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

Bağımsız değişkeni `StyleSheet.FromReader` yöntemi `TextReader` stil sayfası okuma izni.

## <a name="selecting-elements-and-applying-properties"></a>Öğeleri seçme ve uygulama özellikleri

CSS Seçici hedeflemek için hangi öğelerin belirlemek için kullanır. Seçiciler eşleşen stilleri birbiri ardına tanımı sırayla uygulanır. Belirli bir öğe üzerinde tanımlanan stiller her zaman en son uygulanır. Desteklenen Seçici hakkında daha fazla bilgi için bkz: [Seçici başvuru](#selector-reference).

CSS özelliklerini seçili öğesinin stilini belirlemek için kullanır. Olası değerler kümesi her bir özellik vardır ve diğer öğeleri gruplarına uygularken bazı özellikler öğesi, herhangi bir türde etkileyebilir. Desteklenen özellikler hakkında daha fazla bilgi için bkz: [Özellik Başvurusu](#property-reference).

### <a name="selecting-elements-by-type"></a>Türe göre öğeleri seçme

Büyük/küçük harfe duyarlı olmayan türe göre öğeleri görsel ağacında seçilebilir `element` Seçici:

```css
stacklayout {
    margin: 20;
}
```

Bu herhangi seçiciyi [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) stil sayfası kullanmanıza ve kendi kenar boşlukları Tekdüzen bir 20 kalınlığı için ayarlar sayfalarındaki öğelerin.

> [!NOTE]
> `element` Seçici belirtilen türe ait alt sınıfları belirlemek değil.

### <a name="selecting-elements-by-base-class"></a>Taban sınıfı tarafından öğeleri seçme

Görsel ağaç öğeleri, büyük/küçük harfe duyarsız ile temel sınıfı tarafından seçilebilir `^base` Seçici:

```css
^contentpage {
    background-color: lightgray;
}
```

Bu herhangi seçiciyi [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) stil sayfası kullanmanıza ve kendi arka ayarlar öğelerinin rengi `lightgray`.

> [!NOTE]
> `^base` Seçici Xamarin.Forms için özeldir ve CSS belirtiminin parçası değildir.

### <a name="selecting-an-element-by-name"></a>Öğeyi adına göre seçme

Tek tek öğelerine görsel ağacında seçilebilir büyük küçük harfe duyarlı ile `#id` Seçici:

```css
#listView {
    background-color: lightgray;
}
```

Bu öğe seçiciyi olan [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) özelliği `listView`. Ancak, varsa `StyleId` özellik ayarlanmamışsa, seçici kullanmaya geri döner `x:Name` öğe. Bu nedenle, aşağıdaki örnekte XAML, `#listView` Seçici tanımlar [ `ListView` ](xref:Xamarin.Forms.ListView) olan `x:Name` özniteliği `listView`ve onun arka plan rengi ayarlamak `lightgray`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>Belirli bir sınıf özniteliği olan öğeleri seçme

Öğe belirli Sınıf özniteliği ile seçilebilir büyük küçük harfe duyarlı ile `.class` Seçici:

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

Ayarlayarak bir XAML öğesine bir CSS sınıfı atanabilir [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) özelliği öğesine bir CSS sınıfı adı. Bu nedenle, aşağıdaki XAML örnekte tanımlanan stiller `.detailPageTitle` sınıfı, ilk atanan [ `Label` ](xref:Xamarin.Forms.Label), while tarafından tanımlanan stiller `.detailPageSubtitle` sınıfı, ikinci düğüme atanır `Label`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>Alt öğeleri seçme

Alt öğeleri görsel ağacında seçilebilir büyük küçük harfe duyarlı ile `element element` Seçici:

```css
listview image {
    height: 60;
    width: 60;
}
```

Bu herhangi seçiciyi [ `Image` ](xref:Xamarin.Forms.Image) alt öğeleri [ `ListView` ](xref:Xamarin.Forms.ListView) öğeleri ve bunların yükseklik ve genişlik için 60 ayarlar. Bu nedenle, aşağıdaki örnekte XAML, `listview image` Seçici tanımlar [ `Image` ](xref:Xamarin.Forms.Image) alt öğesi olan [ `ListView` ](xref:Xamarin.Forms.ListView)ve 60 yüksekliğini ve genişliğini ayarlar.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> `element element` Alt öğe Seçici gerekli olmadığı bir _doğrudan_ üst-alt öğe alt farklı bir üst öğeye sahip olabilir. Belirtilen ilk öğenin üst öğesi olması şartıyla seçim gerçekleşir.

### <a name="selecting-direct-child-elements"></a>Doğrudan alt öğeleri seçme

Alt öğeleri görsel ağacında seçilebilir büyük küçük harfe duyarlı ile doğrudan `element>element` Seçici:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Bu herhangi seçiciyi [ `Image` ](xref:Xamarin.Forms.Image) öğeleri doğrudan alt [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) öğeleri ve bunların yükseklik ve genişlik 200'e ayarlar. Bu nedenle, aşağıdaki örnekte XAML, `stacklayout>image` Seçici tanımlar [ `Image` ](xref:Xamarin.Forms.Image) öğesinin doğrudan alt öğesi olan [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)ve yüksekliğini ve genişliğini 200'e ayarlar.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> `element>element` Seçici gerektirir alt öğe olduğundan bir _doğrudan_ üst alt.

## <a name="selector-reference"></a>Seçici başvurusu

Aşağıdaki CSS Seçici Xamarin.Forms tarafından desteklenir:

|Seçici|Örnek|Açıklama|
|---|---|---|
|`.class`|`.header`|İle tüm öğeleri seçer `StyleClass` içeren 'header' özelliği. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`#id`|`#email`|İle tüm öğeleri seçer `StyleId` kümesine `email`. Varsa `StyleId` , geri dönüş için ayarlanmadı `x:Name`. XAML, kullanırken `x:Name` üzerinden tercih edilen `StyleId`. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`*`|`*`|Tüm öğeleri seçer.|
|`element`|`label`|Türündeki tüm öğeleri seçer `Label`, ancak alt sınıfların değil. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`^base`|`^contentpage`|İle tüm öğeleri seçer `ContentPage` temel sınıf olarak da dahil olmak üzere `ContentPage` kendisi. Bu Seçici büyük/küçük harfe duyarlıdır ve CSS belirtiminin bir parçası olmayan unutmayın.|
|`element,element`|`label,button`|Tüm seçer `Button` öğeleri ve tüm `Label` öğeleri. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`element element`|`stacklayout label`|Tüm seçer `Label` içinde öğeleri bir `StackLayout`. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`element>element`|`stacklayout>label`|Tüm seçer `Label` öğelerle `StackLayout` doğrudan üst öğe olarak. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`element+element`|`label+entry`|Tüm seçer `Entry` öğelerden sonra doğrudan bir `Label`. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`element~element`|`label~entry`|Tüm seçer `Entry` öğeleri öncesinde bir `Label`. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|

Seçiciler eşleşen stilleri birbiri ardına tanımı sırayla uygulanır. Belirli bir öğe üzerinde tanımlanan stiller her zaman en son uygulanır.

> [!TIP]
> Seçiciler birleştirilebilir sınırlama olmaksızın gibi `StackLayout>ContentView>label.email`.

Şu seçicileri şu anda desteklenmiyor:

- `[attribute]`
- `@media` ve `@supports`
- `:` ve `::`

> [!NOTE]
> Belirginliğe sahiptir ve ayrıntısıyla geçersiz kılmaları desteklenmez.

## <a name="property-reference"></a>Özellik Başvurusu

Aşağıdaki CSS özelliklerini Xamarin.Forms tarafından desteklenir (içinde **değerleri** sütun türleridir _italik_, dize sabit değerleri çalışırken `gray`):

|Özellik|Uygulandığı öğe:|Değerler|Örnek|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial` |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial`|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_Renk_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_dize_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`|_Renk_ \| `initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`|_çift_ \| `intial` |`border-radius: 10;`|
|`border-width`|`Button`|_çift_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_Renk_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_çift_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| `column-reverse` \| `initial`|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_float_ \| `auto` \| `initial`. Ayrıca, de aralık 0 ile % 100 arasında bir yüzde değeri ile belirtilebilir `%` oturum.|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_kayan nokta_ \| `initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_kayan nokta_ \| `initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_dize_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_çift_ \| _namedsize_  \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_çift_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`line-height`|`Label`, `Span`|_çift_ \| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_Kalınlığı_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Kalınlığı_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Kalınlığı_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Kalınlığı_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Kalınlığı_ \| `initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_int_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_çift_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_çift_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_çift_ \| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|_int_ \| `initial`|`order: -1;`|
|`padding`|`Layout`, `Page`|_Kalınlığı_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Layout`, `Page`|_çift_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Layout`, `Page`| _çift_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Layout`, `Page`| _çift_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Layout`, `Page`| _çift_ \| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _çift_ \| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`. `left` ve `right` sağdan sola ortamlarda kaçınılmalıdır.| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _çift_, _çift_ \| `initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_çift_ \| `initial`|`min-width: 320;`|

Aşağıdaki Xamarin.Forms belirli CSS özelliklerini de desteklenmektedir (içinde **değerleri** sütun türleridir _italik_, dize sabit değerleri çalışırken `gray`):

|Özellik|Uygulandığı öğe:|Değerler|Örnek|
|---|---|---|---|
|`-xf-placeholder`|`Entry`, `Editor`, `SearchBar`|_sınırlandırılmış metin_ \| `initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`, `Editor`, `SearchBar`|_Renk_ \| `initial` |`-xf-placeholder-color: green;`|
|`-xf-max-length`|`Entry`, `Editor`|_int_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_Renk_ \| `initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_Renk_ \| `initial` |`-xf-bar-text-color: gray`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both` üzerinde yalnızca desteklenen bir `ScrollView`. |`-xf-orientation: horizontal;`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visbility: always;`|
|`-xf-min-track-color`|`Slider`|_Renk_ \| `initial` |`-xf-min-track-color: yellow;`|
|`-xf-max-track-color`|`Slider`|_Renk_ \| `initial` |`-xf-max-track-color: red;`|
|`-xf-thumb-color`|`Slider`|_Renk_ \| `initial` |`-xf-thumb-color: limegreen;`|
|`-xf-spacing`|`StackLayout`|_çift_ \| `initial` |`-xf-spacing: 8;`|

> [!NOTE]
> `initial` tüm özellikler için geçerli bir değerdir. Bu, başka bir stil ayarlanan değer (varsayılan olarak sıfırlanacak) temizler.

Aşağıdaki özellikler şu anda desteklenmiyor:

- `all: initial`.
- Düzen özelliklerini (kutusu veya kılavuz).
- Toplu özellikleri gibi `font`, ve `border`.

Ayrıca, hiçbir `inherit` değeri ve bu nedenle devralma desteklenmez. Bu nedenle, örneğin, ayarlayamazsınız `font-size` bir düzen özelliği ve tüm beklediğiniz [ `Label` ](xref:Xamarin.Forms.Label) değeri alması için düzeni örnekleri. Bir özel durum `direction` varsayılan değere sahip özelliği, `inherit`.

### <a name="color"></a>Renk

Aşağıdaki `color` değerler desteklenir:

- `X11` [renkleri](https://en.wikipedia.org/wiki/X11_color_names/), CSS renk, önceden tanımlanmış renkleri UWP ve Xamarin.Forms renkleri eşleşmesi. Bu renk değerleri büyük küçük harfe duyarlı olduğunu unutmayın.
- onaltılık renk: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- RGB renkleri: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Değerleri 0-255 veya 0-%100 aralığındadır.
- rgba renkler: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. Geçirgenlik değeri 0,0-1.0 aralığında ' dir.
- HSL renkler: `hsl(120, 100%, 50%)`. S ve m aralığı 0-%100 içinde çalışırken h değeri 0-360 aralığında ' dir.
- hsla renkler: `hsla(120, 100%, 50%, .8)`. Geçirgenlik değeri 0,0-1.0 aralığında ' dir.

### <a name="thickness"></a>Kalınlığı

Bir, iki, üç veya dört `thickness` değerler desteklenir, boşluk tarafından ayrılmış her:

- Tek bir değer Tekdüzen kalınlığı gösterir.
- İki değer dikey sonra yatay kalınlığı gösterir.
- Üç değer, üst, sonra yatay (sol ve sağ), sonra alt kalınlığı gösterir.
- Dört değer, üst, sonra sağ, alt sonra sol kalınlığı gösterir.

> [!NOTE]
> CSS `thickness` değerleri farklı XAML [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/) değerleri. Örneğin, iki değer XAML olarak `Thickness` dört değeri yatay sonra dikey kalınlığını belirten `Thickness` sol, ardından üst ardından sağa doğru ardından kalınlığı alt gösterir. Ayrıca, XAML `Thickness` virgülle ayrılmış değerler.

### <a name="namedsize"></a>NamedSize

Büyük küçük harfe duyarlı aşağıdaki `namedsize` değerler desteklenir:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Her tam anlamı `namedsize` değerdir platforma bağımlıdır ve görünüm bağımlı.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>Xamarin.Forms ile Xamarin.University CSS

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 CSS tarafından [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>İlgili bağlantılar

- [MonkeyAppCSS (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [Kaynak Sözlükler](~/xamarin-forms/xaml/resource-dictionaries.md)
- [XAML Stilleri Kullanarak Xamarin.Forms Uygulamalarında Stil Oluşturma](~/xamarin-forms/user-interface/styles/xaml/index.md)
