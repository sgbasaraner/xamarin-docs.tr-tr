---
title: Stil Xamarin.Forms basamaklı stil sayfalarını kullanarak uygulamaları
description: Xamarin.Forms geçişli stil sayfaları (CSS) kullanarak stil görsel öğeleri destekler.
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/28/2018
ms.openlocfilehash: cb46744b3c0a2f50a02491cc4824dfd4cf847235
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets"></a>Geçişli stil sayfaları kullanarak stil Xamarin.Forms uygulamaları

_Xamarin.Forms geçişli stil sayfaları (CSS) kullanarak stil görsel öğeleri destekler._

Xamarin.Forms 3.0 CSS kullanarak bir uygulama stil verme olanağı sunar. Bir veya daha fazla Seçici ve bildirimi bloğundaki oluşan her bir kural ile kurallarının bir listesini, stil sayfası oluşur. Bir bildirim blok küme ayraçları bildirimlerinde bir özellik, bir iki nokta üst üste ve bir değer oluşan her bildirimiyle listesini oluşur. Bir blok içinde birden çok bildirimler olduğunda, noktalı virgül ayırıcı olarak eklenir. Aşağıdaki kod örneğinde bazı Xamarin.Forms uyumlu CSS gösterir:

```css
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

Xamarin.Forms, CSS stil sayfaları ayrıştırılır ve derleme süresi yerine çalışma zamanı sırasında değerlendirilen ve stil sayfalarını kullanmak üzere yeniden ayrıştırılmış.

> [!NOTE]
> Şu anda tüm XAML stil ile mümkündür stil CSS ile gerçekleştirilemiyor. Ancak, XAML stiller CSS Xamarin.Forms tarafından şu anda desteklenmeyen özellikleri desteklemek üzere kullanılabilir. XAML stilleri hakkında daha fazla bilgi için bkz: [stil oluşturma Xamarin.Forms XAML stilleri kullanarak uygulamaları](~/xamarin-forms/user-interface/styles/xaml/index.md).

[MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) örnek CSS stil basit bir uygulama kullanmayı gösterir ve aşağıdaki ekran görüntülerinde gösterilir:

[![CSS stil MonkeyApp ana sayfası](css-images/MonkeyAppMainPage.png "MonkeyApp ana sayfa CSS stil ile")](css-images/MonkeyAppMainPage-Large.png#lightbox "CSS stil MonkeyApp ana sayfası")

[![CSS stil MonkeyApp ayrıntı sayfası](css-images/MonkeyAppDetailPage.png "MonkeyApp ayrıntı CSS stil sayfasıyla")](css-images/MonkeyAppDetailPage-Large.png#lightbox "CSS stil MonkeyApp Ayrıntı Sayfası")

> [!NOTE]
> Arka plan rengini stilini belirlemek şu anda olası değil bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) bir stil sayfasını kullanarak. Bu nedenle, örnek uygulamasında [ `NavigationPage.BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) özelliği, kodda ayarlanır.

## <a name="consuming-a-style-sheet"></a>Stil sayfası kullanma

İşlem bir stil sayfası bir çözüme eklemek için aşağıdaki gibidir:

1. Boş bir CSS dosya .NET standart kitaplığını projenize ekleyin.
1. CSS dosyası derleme eylem **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Stil sayfası yükleniyor

Stil sayfası yüklemek için kullanılan yaklaşım vardır.

### <a name="xaml"></a>XAML

Stil sayfası yüklenen ve ile Ayrıştırılan [ `StyleSheet` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StyleSheets.StyleSheet/) eklenmeden önce sınıfı [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) sayfası için:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    ...
</ContentPage>
```

[ `StyleSheet.Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StyleSheets.Source/) Özellik belirtir stil sayfası kapsayan XAML dosyasının konumunu göreli veya proje köküne ilişkin bir URI olarak URI ile başlarsa bir `/`.

> [!WARNING]
> CSS dosyası etkinleştirilmişse yüklenemeyecek yapı eylemi ayarlı değil **EmbeddedResource**.

Alternatif olarak, stil sayfası yüklenebilir ve ile Ayrıştırılan [ `StyleSheet` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StyleSheets.StyleSheet/) tarafından sınıf satır içi kullanım içinde bir `CDATA` bölümü:

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

### <a name="c"></a>C#

C# ' ta bir stil sayfası katıştırılmış bir kaynağı yüklenebilir ve eklenen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) sayfası için:

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

İlk bağımsız değişken `StyleSheet.FromAssemblyResource` yöntemi stil sayfası içeren derlemenin, ikinci bağımsız değişkeni bir `string` kaynak tanımlayıcısını temsil eden. Kaynak tanımlayıcısı elde edilebilir **özellikleri** CSS dosyası seçildiğinde penceresi.

Alternatif olarak, stil sayfası gelen yüklenebilir bir `StringReader` ve eklenen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) sayfası için:

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

## <a name="selecting-elements-and-applying-properties"></a>Öğeler seçme ve Özellikler Uygulanıyor

CSS Seçici hedeflemek için hangi öğelerin belirlemek için kullanır. Seçici eşleşen stilleri art arda, tanım sırayla uygulanır. Belirli bir öğede tanımlanan stiller her zaman en son uygulanır. Desteklenen seçiciler hakkında daha fazla bilgi için bkz: [Seçici başvuru](#selector-reference).

CSS özelliklerini seçili öğesinin stilini belirlemek için kullanır. Bir dizi olası değerler her bir özellik vardır ve diğer öğeleri gruplarına uygularken bazı özellikler öğesi, herhangi bir türde etkileyebilir. Desteklenen özellikler hakkında daha fazla bilgi için bkz: [Özellik Başvurusu](#property-reference).

### <a name="selecting-elements-by-type"></a>Öğeler türe göre seçme

Görsel ağaç öğeleri, büyük küçük harfe duyarlı türüyle tarafından seçilebilir `element` Seçici:

```css
stacklayout {
    margin: 20;
}
```

Bu herhangi seçiciyi [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) stil sayfasına tüketen ve bunların kenar boşluklarını Tekdüzen kalınlığı 20 için ayarlar sayfaları öğelerde.

> [!NOTE]
> `element` Seçici alt sınıflar belirtilen türde tanımlamıyor.

### <a name="selecting-elements-by-base-class"></a>Temel sınıfı tarafından öğeleri seçme

Görsel ağaç öğeleri, büyük küçük harfe duyarlı ile temel sınıfı tarafından seçilebilir `^base` Seçici:

```css
^contentpage {
    background-color: lightgray;
}
```

Bu herhangi seçiciyi [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) stil sayfasına tüketen ve bunların arka plan ayarlayan öğelerinin renk için `lightgray`.

> [!NOTE]
> `^base` Seçici Xamarin.Forms için özeldir ve CSS belirtiminin bir parçası değil.

### <a name="selecting-an-element-by-name"></a>Bir öğenin adına göre seçme

Görsel ağaç ayrı ayrı öğeler seçilebilir büyük küçük harfe duyarlı ile `#id` Seçici:

```css
#listView {
    background-color: lightgray;
}
```

Öğe bu seçiciyi, [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) özelliği ayarlanmış `listView`. Ancak, varsa `StyleId` özellik ayarlanmamışsa, seçici kullanmaya geri döner `x:Name` öğesi. Bu nedenle, aşağıdaki örnekte XAML, `#listView` Seçici sıralanmayacağı [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) , `x:Name` özniteliği `listView`ve buna ait arka plan rengini ayarlamak `lightgray`.

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

### <a name="selecting-elements-with-a-specific-class-attribute"></a>Belirli bir sınıf özniteliği olan öğeler seçme

Belirli bir sınıfa öznitelik öğeleriyle seçilebilir büyük küçük harfe duyarlı ile `.class` Seçici:

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

Bir CSS sınıfı ayarlayarak bir XAML öğesine atanabilir [ `StyleClass` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.StyleClass/) özelliği öğenin CSS sınıfı adı. Bu nedenle, aşağıdaki XAML örnekte stilleri tarafından tanımlanan `.detailPageTitle` sınıf ilk atanan [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), while tarafından tanımlanan stiller `.detailPageSubtitle` sınıf ikinci atanan `Label`.

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

### <a name="selecting-child-elements"></a>Alt öğeler seçme

Alt öğeler görsel ağaç seçilebilir büyük küçük harfe duyarlı ile `element element` Seçici:

```css
listview image {
    height: 60;
    width: 60;
}
```

Bu herhangi seçiciyi [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) , alt öğelerini [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) öğeleri ve bunların yüksekliğini ve genişliğini 60 olarak ayarlar. Bu nedenle, aşağıdaki örnekte XAML, `listview image` Seçici sıralanmayacağı [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) bir alt öğesi olan [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)ve yüksekliğini ve genişliğini 60 olarak ayarlar.

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
> `element element` Seçici olarak alt öğesi gerekli olmadığı bir _doğrudan_ üst-alt öğesi alt farklı bir üst öğeye sahip olabilir. Bir üst belirtilen ilk öğedir koşuluyla seçimi oluşur.

### <a name="selecting-direct-child-elements"></a>Doğrudan alt öğeleri seçme

Alt öğeler görsel ağaç seçilebilir büyük küçük harfe duyarlı ile doğrudan `element>element` Seçici:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Bu herhangi seçiciyi [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğeleri doğrudan alt [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) öğeleri ve bunların yükseklik ve genişlik 200'e ayarlar. Bu nedenle, aşağıdaki örnekte XAML, `stacklayout>image` Seçici sıralanmayacağı [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) doğrudan alt öğesi olan [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)ve yüksekliğini ve genişliğini 200'e ayarlar.

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
> `element>element` Seçiciyi gerektirdiği alt öğesi olan bir _doğrudan_ üst alt.

## <a name="selector-reference"></a>Seçici başvurusu

Aşağıdaki CSS Seçici Xamarin.Forms tarafından desteklenir:

|Seçici|Örnek|Açıklama|
|---|---|---|
|`.class`|`.header`|İle tüm öğeleri seçer `StyleClass` 'üstbilgisi' içeren özellik. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`#id`|`#email`|İle tüm öğeleri seçer `StyleId` kümesine `email`. Varsa `StyleId` , geri dönüş için ayarlanmadı `x:Name`. XAML kullanırken `x:Name` üzerinden tercih edilir `StyleId`. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`*`|`*`|Tüm öğeleri seçer.|
|`element`|`label`|Türündeki tüm öğeleri seçer `Label`, ancak alt sınıfların değil. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`^base`|`^contentpage`|İle tüm öğeleri seçer `ContentPage` temel sınıf olarak dahil olmak üzere `ContentPage` kendisi. Bu Seçici büyük küçük harfe duyarlı olduğunu ve CSS belirtiminin bir parçası değil unutmayın.|
|`element,element`|`label,button`|Tüm seçer `Button` öğeleri ve tüm `Label` öğeleri. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`element element`|`stacklayout label`|Tüm seçer `Label` öğeler içinde bir `StackLayout`. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`element>element`|`stacklayout>label`|Tüm seçer `Label` öğeleriyle `StackLayout` doğrudan üst öğe olarak. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`element+element`|`label+entry`|Tüm seçer `Entry` öğelerden sonra doğrudan bir `Label`. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|
|`element~element`|`label~entry`|Tüm seçer `Entry` öğeleri öncesinde tarafından bir `Label`. Bu Seçici büyük küçük harfe duyarlı olduğunu unutmayın.|

Seçici eşleşen stilleri art arda, tanım sırayla uygulanır. Belirli bir öğede tanımlanan stiller her zaman en son uygulanır.

> [!TIP]
> Seçici birleştirilebilir bunlarla sınırlı olmamak kaydıyla gibi `StackLayout>ContentView>label.email`.

Şu seçicileri şu anda desteklenmiyor:

- `[attribute]`
- `@media` Ve `@supports`
- `:` Ve `::`

> [!NOTE]
> Belirginliğe ve belirginliğe geçersiz kılmalar desteklenmez.

## <a name="property-reference"></a>Özellik Başvurusu

Aşağıdaki CSS özelliklerini Xamarin.Forms tarafından desteklenen (içinde **değerleri** sütun türleridir _italik_, dize değişmez değerleri durumdayken `gray`):

|Özellik|Uygulandığı öğe:|Değerler|Örnek|
|---|---|---|---|
|`background-color`|`VisualElement`|_Renk_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Dize_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`|_Renk_ \| `initial`|`border-color: #9acd32;`|
|`border-width`|`Button`|_Çift_ \| `initial` |`border-width: .5;`|
|`color`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`|_Renk_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Dize_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_çift_ \| _namedsize_  \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_Çift_ \| `initial` |`min-height: 250;`|
|`margin`|`View`|_Kalınlığı_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Kalınlığı_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Kalınlığı_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Kalınlığı_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Kalınlığı_ \| `initial` |`margin-bottom: 6;`|
|`min-height`|`VisualElement`|_Çift_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_Çift_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_Çift_ \| `initial` |`opacity: .3;`|
|`padding`|`Layout`, `Page`|_Kalınlığı_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Layout`, `Page`|_Çift_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Layout`, `Page`| _Çift_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Layout`, `Page`| _Çift_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Layout`, `Page`| _Çift_ \| `initial` |`padding-bottom: 6;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `right` \| `center` \| `start` \| `end` \| `initial`. `left` ve `right` sağdan sola ortamlarda kaçınılmalıdır.| `text-align: right;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_Çift_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` tüm özellikler için geçerli bir değer değil. Başka bir stili ayarlanan değer (varsayılan olarak sıfırlar) temizler.

Aşağıdaki özellikleri şu anda desteklenmiyor:

- `all: initial`.
- Yerleşim özellikleri (kutusu veya kılavuz).
- Özellikleri, gibi `font`, ve `border`.

Ayrıca, hiçbir `inherit` değeri ve bu nedenle devralma desteklenmiyor. Bu nedenle, örneğin, ayarlayamazsınız `font-size` bir düzende özellik ve tüm beklediğiniz [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) değeri alması için düzeninde örnekleri. Bir özel durum `direction` varsayılan değere sahip özelliği, `inherit`.

### <a name="color"></a>Renk

Aşağıdaki `color` değerler desteklenir:

- `X11` [renkleri](https://en.wikipedia.org/wiki/X11_color_names/), CSS renkleri, UWP önceden tanımlanmış renkleri ve Xamarin.Forms renkleri eşleşmesi. Bu renk değerleri büyük küçük harfe duyarlı olduğunu unutmayın.
- onaltılı renkleri: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- RGB renk: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Değerler 0-255 veya 0-%100 aralığındadır.
- rgba renkleri: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. Geçirgenlik değeri 0,0-1.0 aralığında değil.
- HSL renkleri: `hsl(120, 100%, 50%)`. S ve m aralığı % 0-% 100'durumdayken h 0-360 aralığında değerdir.
- hsla renkleri: `hsla(120, 100%, 50%, .8)`. Geçirgenlik değeri 0,0-1.0 aralığında değil.

### <a name="thickness"></a>Kalınlığı

Bir, iki, üç veya dört `thickness` değerler desteklenir, her beyaz boşlukla ayrılmış:

- Tek bir değer Tekdüzen kalınlığını belirtir.
- İki değer dikey sonra yatay kalınlığını belirtir.
- Üç değerler üst, sonra yatay (sol ve sağ) sonra alt kalınlığını belirtir.
- Dört değerleri, üst, sonra sağ, alt sonra sol kalınlığı gösterir.

> [!NOTE]
> CSS `thickness` değerleri farklı XAML [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/) değerleri. Örneğin, XAML'de iki değer `Thickness` dört değer sırasında yatay sonra dikey kalınlığını belirtir `Thickness` sol, sonra üst ve sağ tarafta, ardından kalınlığı alt gösterir. Ayrıca, XAML `Thickness` virgülle ayrılmış değerler.

### <a name="namedsize"></a>NamedSize

Aşağıdaki büyük küçük harfe duyarlı `namedsize` değerler desteklenir:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Her tam anlamını `namedsize` değerdir platforma bağımlı ve görünüm bağımlı.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>Xamarin.Forms Xamarin.University ile CSS

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 CSS, göre [Xamarin Üniversitesi](https://university.xamarin.com/)**

## <a name="related-links"></a>İlgili bağlantılar

- [MonkeyAppCSS (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [Stil Xamarin.Forms XAML stilleri kullanan uygulamalar](~/xamarin-forms/user-interface/styles/xaml/index.md)
