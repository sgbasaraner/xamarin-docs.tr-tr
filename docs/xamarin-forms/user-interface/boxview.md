---
title: BoxView
description: "Renkli bir dikdörtgen decoration, grafik ve etkileşim için kullanın."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: 4d50ea5c3db0f5a141f1b48cf0a948c10b63f7f0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="boxview"></a>BoxView

[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) belirtilen genişlik, yükseklik ve renk basit bir dikdörtgen işler. Kullanabileceğiniz `BoxView` decoration, ilkel grafik ve dokunma aracılığıyla kullanıcıyla etkileşim için.

Xamarin.Forms yerleşik vektör grafik sistemi olmadığından `BoxView` dengelemek için yardımcı olur. Bu makale kullanımda açıklanan örnek programların bazıları `BoxView` işleme grafikler için. `BoxView` Belirli genişlik ve kalınlığı kolu benzeyecek şekilde boyutlandırılmış ve tüm Açı kullanarak Döndürülmüş `Rotation` özelliği.

Ancak `BoxView` basit grafik taklit edebilirsiniz, araştırmak isteyebileceğiniz [kullanarak SkiaSharp Xamarin.Forms içinde](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) daha karmaşık grafik gereksinimleri için.

Bu makalede aşağıdaki konular ele alınmıştır:

- **[BoxView rengini ve boyutunu ayarlama](#colorandsize)**  &ndash; ayarlamak `BoxView` özellikleri.
- **[İşleme metin düzenlemelerinin](#textdecorations)**  &ndash; kullanan bir `BoxView` işleme satırlar için.
- **[BoxView renklerle listeleme](#listingcolors)**  &ndash; tüm sistem renkleri görüntüleme bir `ListView`.
- **[Oyun yaşam sınıflara BoxView tarafından çalma](#subclassing)**  &ndash; ünlü bir cep telefonu automaton uygulayın.
- **[Sayısal bir saat oluşturma](#digitalclock)**  &ndash; iğneli görüntü benzetimi.
- **[Bir Analog saati oluşturarak](#analogclock)**  &ndash; dönüştürme ve animasyon `BoxView` öğeleri.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>Ayar BoxView renk ve boyutu

Aşağıdaki üç özelliklerini çok sık ayarlarız `BoxView`:

- [`Color`](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) rengini ayarlamak için.
- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) genişliğini ayarlamak için `BoxView` CİHAZDAN bağımsız birimler.
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) yüksekliğini ayarlamak için `BoxView`.

`Color` Özelliği türüdür `Color`; özelliği için ayarlanabilir `Color` değeri alfabetik olarak arasında değişen renkleri adlı statik 141 salt okunur alanları da dahil olmak üzere `AliceBlue` için `YellowGreen`.

`WidthRequest` Ve `HeightRequest` özellikleri, bir rol yalnızca yürütmek `BoxView` olan *Kısıtlanmamış* düzeni. Düzen kapsayıcı alt kullanıcının boyut, örneğin, ne zaman bilmek gerektiğinde bu durumda `BoxView` otomatik ölçekli bir hücreye alt `Grid` düzeni. A `BoxView` de Kısıtlanmamış olduğunda kendi `HorizontalOptions` ve `VerticalOptions` özellikler ayarlanır değerlere dışında `LayoutOptions.Fill`. Varsa `BoxView` Kısıtlanmamış, olan ancak `WidthRequest` ve `HeightRequest` özellikleri ayarlı değil, sonra genişliği veya yüksekliği 40 birimleri ya da yaklaşık 1/4 inç mobil cihazlarda varsayılan değerlere ayarlanır.

`WidthRequest` Ve `HeightRequest` varsa özelliklerini göz ardı `BoxView` olan *kısıtlı* içinde durumu seçilebilir uygular, kendi boyutu üzerinde düzeninde `BoxView`. 

A `BoxView` bir boyuttaki kısıtlı ve diğer Kısıtlanmamış. Örneğin, varsa `BoxView` dikey alt `StackLayout`, dikey boyutunu `BoxView` olan Kısıtlanmamış ve yatay boyut genellikle sınırlı değildir. Ancak bu yatay boyut için özel durumlar: varsa `BoxView` sahip kendi `HorizontalOptions` özelliği için bir şeyler dışında ayarlanmış `LayoutOptions.Fill`, yatay boyut da Kısıtlanmamış ise. Ayrıca mümkündür `StackLayout` kendisini bir Kısıtlanmamış yatay boyut; bu durumda olmasını `BoxView` yatay Kısıtlanmamış olacaktır.

[ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) örnek bir tane inç-Kısıtlanmamış kare görüntüler `BoxView` kendi sayfası Center'da:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center" 
             HorizontalOptions="Center" />

</ContentPage>
```

Sonuç şöyledir:

[![Temel BoxView](boxview-images/basicboxview-small.png "temel BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

Varsa `VerticalOptions` ve `HorizontalOptions` özellikler kaldırılma `BoxView` etiketi veya şekilde ayarlanmış `Fill`, sonra `BoxView` sayfa boyutu tarafından kısıtlı olur ve sayfayı doldurmak için genişletir.

A `BoxView` ayrıca bir alt öğesi olarak bir `AbsoluteLayout`. Bu durumda, konum ve boyutunun `BoxView` kullanılarak ayarlanan `LayoutBounds` bağlanabilirse özelliği eklenmiş. `AbsoluteLayout` Makalesinde açıklanan [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md).

Tüm bu durumlarda izleyin örnek programlar örnekleri görürsünüz.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>Metin düzenlemelerinin işleme

Kullanabileceğiniz `BoxView` sayfalarınızda yatay ve dikey çizgileri form üzerinde bazı basit düzenlemelerinin eklemek için. [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) örnek bu gösterir. Programın görselleri tümünün içinde tanımlanan **MainPage.xaml** birkaç içeren dosya `Label` ve `BoxView` öğelerinde `StackLayout` burada gösterilen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TextDecoration"
             x:Class="TextDecoration.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Black" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ScrollView Margin="15">
        <StackLayout>

            ···

        </StackLayout>
    </ScrollView>
</ContentPage>
```

Aşağıdaki biçimlendirme alt öğelerinin tümü `StackLayout`. Bu biçimlendirme birçok türdeki dekoratif oluşur `BoxView` ile kullanılan öğeleri `Label` öğe:

[![Metin dekorasyonu](boxview-images/textdecoration-small.png "dekorasyonu")](boxview-images/textdecoration-large.png#lightbox "dekorasyonu")

Sayfanın üstündeki şık üstbilgisi ile elde edilen bir `AbsoluteLayout` dört alt öğeleri olan `BoxView` öğeleri ve `Label`, tüm hangi olan belirli konumları ve boyutları atanan:

```xaml
<AbsoluteLayout>
    <BoxView AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
    <BoxView AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
    <Label Text="Stylish Header"
           FontSize="24"
           AbsoluteLayout.LayoutBounds="30, 25, AutoSize, AutoSize"/>
</AbsoluteLayout>
```

XAML dosyasındaki `AbsoluteLayout` tarafından izlenen bir `Label` açıklayan metin biçimlendirilmiş `AbsoluteLayout`.

Her ikisi de kapsayan bir metin dizesi altını çizebilirsiniz `Label` ve `BoxView` içinde bir `StackLayout` olan kendi `HorizontalOptions` değerini ayarlamak için bir şeyler dışında `Fill`. Genişliğini `StackLayout` sonra genişliğini tarafından yönetilir `Label`, üzerinde daha sonra bu genişliği uygular `BoxView`. `BoxView` Açık bir yükseklik atanır:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Bu teknik uzun metin dizelerini veya bir paragraf içinde tek tek sözcükleri altı çizili için kullanamazsınız.

Kullanmak da mümkündür bir `BoxView` bir HTML benzeyecek şekilde `hr` (yatay çizgi) öğesi. Yalnızca genişliğini izin `BoxView` bu durumda kendi üst kapsayıcı tarafından belirlenemedi `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

Bir paragraf metni tarafında her ikisi de kapsayan bir dikey çizgi son olarak, çizebilirsiniz `BoxView` ve `Label` yatay olarak `StackLayout`. Bu durumda, yüksekliğini `BoxView` yüksekliği ile aynıdır `StackLayout`, yüksekliğini tarafından yönetilen `Label`: 

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
/StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>BoxView renklerle listeleme

`BoxView` Renkleri görüntüleme için uygundur. Bu program kullanan bir `ListView` tüm ortak statik salt okunur alanları Xamarin.Forms listelemek için `Color` yapısı:

[![ListView renkleri](boxview-images/listviewcolors-small.png "ListView renkleri")](boxview-images/listviewcolors-large.png#lightbox "ListView renkleri")

[ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) program içerir adlı bir sınıf `NamedColor`. Statik Oluşturucu tüm alanları erişmek için yansıma kullanan `Color` yapısı ve oluşturma bir `NamedColor` her biri için nesne. Bu statik depolanan `All` özelliği:

```csharp
public class NamedColor
{
    // Instance members.
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    // Static members.
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields ())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof (Color))
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
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }
}
```

Program görselleri XAML dosyası içinde açıklanmıştır. `ItemsSource` Özelliği `ListView` statik olarak ayarlamanız `NamedColor.All` anlamına özelliği `ListView` tüm tek tek görüntüler `NamedColor` nesneler: 

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewColors"
             x:Class="ListViewColors.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="10, 20, 10, 0" />
            <On Platform="Android, UWP" Value="10, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ListView SeparatorVisibility="None"
              ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.RowHeight>
            <OnPlatform x:TypeArguments="x:Int32">
                <On Platform="iOS, Android" Value="80" />
                <On Platform="UWP" Value="90" />
            </OnPlatform>
        </ListView.RowHeight>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ContentView Padding="5">
                        <Frame OutlineColor="Accent"
                               Padding="10">
                            <StackLayout Orientation="Horizontal">
                                <BoxView Color="{Binding Color}"
                                         WidthRequest="50"
                                         HeightRequest="50" />
                                <StackLayout>
                                    <Label Text="{Binding FriendlyName}"
                                           FontSize="22"
                                           VerticalOptions="StartAndExpand" />
                                    <Label Text="{Binding RgbDisplay, StringFormat='RGB = {0}'}"
                                           FontSize="16"
                                           VerticalOptions="CenterAndExpand" />
                                </StackLayout>
                            </StackLayout>
                        </Frame>
                    </ContentView>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage> 
```

`NamedColor` Tarafından biçimlendirilmiş nesneleri `ViewCell` veri şablonu olarak ayarlanmış nesne `ListView`. Bu şablonu içeren bir `BoxView` , `Color` özelliği bağlı `Color` özelliği `NamedColor` nesne.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Oyuna sınıflara BoxView tarafından ömrü

John Conway matematikçi tarafından geliştirilen ve sayfaları, popularized bir cep telefonu automaton, oyun ömrü olduğunu *bilimsel Amerikan* 1970s içinde. İyi bir giriş Wikipedia makaleyi tarafından sağlanan [, Conway'in oyun yaşam](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) programı adlı bir sınıf tanımlayan `LifeCell` , türetilen `BoxView`. Bu sınıf, oyun ömrü tek tek bir hücrede mantığı yalıtır:

```csharp
class LifeCell : BoxView
{
    bool isAlive;

    public event EventHandler Tapped;

    public LifeCell()
    {
        BackgroundColor = Color.White;

        TapGestureRecognizer tapGesture = new TapGestureRecognizer();
        tapGesture.Tapped += (sender, args) =>
        {
            Tapped?.Invoke(this, EventArgs.Empty);
        };
        GestureRecognizers.Add(tapGesture);
    }

    public int Col { set; get; }

    public int Row { set; get; }

    public bool IsAlive
    {
        set
        {
            if (isAlive != value)
            {
                isAlive = value;
                BackgroundColor = isAlive ? Color.Black : Color.White;
            }
        }
        get
        {
            return isAlive;
        }
    }
}
```

`LifeCell` üç daha fazla özelliklerine ekler `BoxView`: `Col` ve `Row` özelliklerini depolamak kılavuz hücrede konumunu ve `IsAlive` özelliği durumunu gösterir. `IsAlive` Özelliği de ayarlar `Color` özelliği `BoxView` hücre etkin değilse, canlı ve beyaz hücreyse siyah.

`LifeCell` aynı zamanda yükler bir `TapGestureRecognizer` bunları dokunarak hücreleri durumunu değiştirme yapmalarına izin vermek için. Sınıf çevirir `Tapped` kendi içine hareketi tanıyıcı olayından `Tapped` olay.

**GameOfLife** program de dahil bir `LifeGrid` oyun mantığı çoğunu yalıtır sınıfı ve bir `MainPage` programın görselleri işleme sınıfı. Bunlar oyunun kurallarını açıklayan bir katmana içerir. Birkaç yüz gösteren eylemde program işte `LifeCell` sayfadaki nesneler:

[![Yaşam oyunu](boxview-images/gameoflife-small.png "yaşam oyunu")](boxview-images/gameoflife-large.png#lightbox "yaşam oyunu")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>Sayısal bir saat oluşturma

[ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) program oluşturur 210 `BoxView` bir eski moda 5 tarafından 7 iğneli görüntü nokta benzetimini yapmak için öğeleri. Dikey veya yatay modu zamanında okuyabilir, ancak yatay olarak büyük:

[![İğneli saati](boxview-images/dotmatrixclock-small.png "iğneli saati")](boxview-images/dotmatrixclock-large.png#lightbox "iğneli saati")

XAML dosyası biraz birden fazla örneği `AbsoluteLayout` saati kullanılır:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DotMatrixClock"
             x:Class="DotMatrixClock.MainPage"
             Padding="10"
             SizeChanged="OnPageSizeChanged">

    <AbsoluteLayout x:Name="absoluteLayout"
                    VerticalOptions="Center" />
</ContentPage>
```

Şey arka plan kod dosyasına oluşur. İğneli görüntü mantığı her iki nokta ile 10 tabanlı rakamlar ve karşılık gelen noktalar açıklamak birkaç diziler tanımı tarafından büyük ölçüde basitleştirilmiş:

```csharp
public partial class MainPage : ContentPage
{
    // Total dots horizontally and vertically.
    const int horzDots = 41;
    const int vertDots = 7;

    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[, ,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 1, 1}, { 1, 0, 1, 0, 1}, 
            { 1, 1, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 0, 0}, { 0, 1, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, 
            { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, 
            { 0, 0, 1, 0, 0}, { 0, 1, 0, 0, 0}, { 1, 1, 1, 1, 1}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 0, 1, 0},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 0, 1, 0}, { 0, 0, 1, 1, 0}, { 0, 1, 0, 1, 0}, { 1, 0, 0, 1, 0}, 
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 0, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, { 0, 0, 0, 0, 1}, 
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 1, 0}, { 0, 1, 0, 0, 0}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, 
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, 
            { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}, 
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 1}, 
            { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 1, 1, 0, 0}
        },
    };

    // Dot matrix pattern for a colon.
    static readonly int[,] colonPattern = new int[7, 2]
    {
        { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }
    };

    // BoxView colors for on and off.
    static readonly Color colorOn = Color.Red;
    static readonly Color colorOff = new Color(0.5, 0.5, 0.5, 0.25);

    // Box views for 6 digits, 7 rows, 5 columns.
    BoxView[, ,] digitBoxViews = new BoxView[6, 7, 5];

    ···

}
```

Bu alanların üç boyutlu bir dizi ile sonuçlandırmak `BoxView` altı basamak nokta desenleri depolamak için öğeleri.

Oluşturucusu tüm oluşturur `BoxView` basamak ve iki nokta üst üste ve ayrıca başlatır için öğeleri `Color` özelliği `BoxView` iki nokta üst üste için öğeleri:

```csharp
public partial class MainPage : ContentPage
{

    ···

    public MainPage()
    {
        InitializeComponent();

        // BoxView dot dimensions.
        double height = 0.85 / vertDots;
        double width = 0.85 / horzDots;

        // Create and assemble the BoxViews.
        double xIncrement = 1.0 / (horzDots - 1);
        double yIncrement = 1.0 / (vertDots - 1);
        double x = 0;

        for (int digit = 0; digit < 6; digit++)
        {
            for (int col = 0; col < 5; col++)
            {
                double y = 0;

                for (int row = 0; row < 7; row++)
                {
                    // Create the digit BoxView and add to layout.
                    BoxView boxView = new BoxView();
                    digitBoxViews[digit, row, col] = boxView;
                    absoluteLayout.Children.Add(boxView,
                                                new Rectangle(x, y, width, height),
                                                AbsoluteLayoutFlags.All);
                    y += yIncrement;
                }
                x += xIncrement;
            }
            x += xIncrement;

            // Colons between the hours, minutes, and seconds.
            if (digit == 1 || digit == 3)
            {
                int colon = digit / 2;

                for (int col = 0; col < 2; col++)
                {
                    double y = 0;

                    for (int row = 0; row < 7; row++)
                    {
                        // Create the BoxView and set the color.
                        BoxView boxView = new BoxView
                            {
                                Color = colonPattern[row, col] == 1 ?
                                            colorOn : colorOff
                            };
                        absoluteLayout.Children.Add(boxView,
                                                    new Rectangle(x, y, width, height),
                                                    AbsoluteLayoutFlags.All);
                        y += yIncrement;
                    }
                    x += xIncrement;
                }
                x += xIncrement;
            }
        }

        // Set the timer and initialize with a manual call.
        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimer);
        OnTimer();
    }

    ···

}
```

Bu program göreli konumlandırma ve boyutlandırma özelliğini kullanan `AbsoluteLayout`. Genişlik ve yükseklik her `BoxView` yatay ve dikey noktalar numarasına göre bölünmüş %1 85'özellikle kesirli değerlerine ayarlayın. Konumlar aynı zamanda kesirli değerlere ayarlanır. 

Tüm boyutları ve konumları göre toplam boyutu olduğundan `AbsoluteLayout`, `SizeChanged` sayfası için işleyici gerektiğini yalnızca ayarlamak bir `HeightRequest` , `AbsoluteLayout`:

```csharp
public partial class MainPage : ContentPage
{
    
    ···
    
    void OnPageSizeChanged(object sender, EventArgs args)
    {
        // No chance a display will have an aspect ratio > 41:7
        absoluteLayout.HeightRequest = vertDots * Width / horzDots;
    }
    
    ···
    
}
```

Genişliğini `AbsoluteLayout` tam sayfa genişliğine göre uzatır olduğundan otomatik olarak ayarlanır.

Son kodda `MainPage` sınıfı Zamanlayıcı geri işler ve her basamak nokta renkleri. Çok boyutlu diziler arka plan kod dosyasının başında tanımını bu mantığı program basit bir parçası olmasına yardımcı olur:

```csharp
public partial class MainPage : ContentPage
{
   
    ···
 
    bool OnTimer()
    {
        DateTime dateTime = DateTime.Now;

        // Convert 24-hour clock to 12-hour clock.
        int hour = (dateTime.Hour + 11) % 12 + 1;

        // Set the dot colors for each digit separately.
        SetDotMatrix(0, hour / 10);
        SetDotMatrix(1, hour % 10);
        SetDotMatrix(2, dateTime.Minute / 10);
        SetDotMatrix(3, dateTime.Minute % 10);
        SetDotMatrix(4, dateTime.Second / 10);
        SetDotMatrix(5, dateTime.Second % 10);
        return true;
    }

    void SetDotMatrix(int index, int digit)
    {
        for (int row = 0; row < 7; row++)
            for (int col = 0; col < 5; col++)
            {
                bool isOn = numberPatterns[digit, row, col] == 1;
                Color color = isOn ? colorOn : colorOff;
                digitBoxViews[index, row, col].Color = color;
            }
    }
}
```
<a name="analogclock" />

## <a name="creating-an-analog-clock"></a>Bir Analog Saat oluşturma

İğneli saati belirgin bir uygulama olduğu görünebilir `BoxView`, ancak `BoxView` öğeleri özellikli bir analog saat kullanıldıklarını ayrıca:

[![BoxView saat](boxview-images/boxviewclock-small.png "BoxView saati")](boxview-images/boxviewclock-large.png#lightbox "BoxView saati")

Tüm Görsellere [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) program alt öğeleri olan bir `AbsoluteLayout`. Bu öğeleri kullanarak boyuta sahip `LayoutBounds` özelliği eklenmiş ve kullanılarak döndürülen `Rotation` özelliği. 

Üç `BoxView` saatin ellerini için öğeleri XAML dosyasında örneği ancak konumlandırılmış veya boyuta sahip:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BoxViewClock"
             x:Class="BoxViewClock.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <AbsoluteLayout x:Name="absoluteLayout"
                    SizeChanged="OnAbsoluteLayoutSizeChanged">
        
        <BoxView x:Name="hourHand"
                 Color="Black" />
        
        <BoxView x:Name="minuteHand"
                 Color="Black" />
        
        <BoxView x:Name="secondHand"
                 Color="Black" />
    </AbsoluteLayout>
</ContentPage>
```

Arka plan kodu dosyanın oluşturucusu 60 başlatır `BoxView` çizgilerinin çevresinde saatin çevresi için öğeleri:

```csharp
public partial class MainPage : ContentPage
{
      
    ···
 
    BoxView[] tickMarks = new BoxView[60];

    public MainPage()
    {
        InitializeComponent();

        // Create the tick marks (to be sized and positioned later).
        for (int i = 0; i < tickMarks.Length; i++)
        {
            tickMarks[i] = new BoxView { Color = Color.Black };
            absoluteLayout.Children.Add(tickMarks[i]);
        }

        Device.StartTimer(TimeSpan.FromSeconds(1.0 / 60), OnTimerTick);
    }
  
    ···
 
}
```

Boyutlandırma ve tüm konumlandırma `BoxView` öğeleri oluşuyor `SizeChanged` işleyicisi `AbsoluteLayout`. Küçük bir yapı sınıfa iç adlı `HandParams` her saat toplam boyutu göre üç ellerini boyutunu açıklar:

```csharp
public partial class MainPage : ContentPage
{
    // Structure for storing information about the three hands.
    struct HandParams
    {
        public HandParams(double width, double height, double offset) : this()
        {
            Width = width;
            Height = height;
            Offset = offset;
        }

        public double Width { private set; get; }   // fraction of radius
        public double Height { private set; get; }  // ditto
        public double Offset { private set; get; }  // relative to center pivot
    }

    static readonly HandParams secondParams = new HandParams(0.02, 1.1, 0.85);
    static readonly HandParams minuteParams = new HandParams(0.05, 0.8, 0.9);
    static readonly HandParams hourParams = new HandParams(0.125, 0.65, 0.9);
 
    ···
 
 }
```

`SizeChanged` İşleyici merkezi ve yarıçapını belirler `AbsoluteLayout`ve ardından boyutları ve 60 konumlandırır `BoxView` çizgilerinin kullanılan öğeleri. `for` Döngü sonucuna ayarlayarak `Rotation` her birinin özelliğini `BoxView` öğeleri. Sonunda `SizeChanged` işleyicisi `LayoutHand` yöntemi, boyut ve saatin üç ellerini konumlandırmak için çağrılır:

```csharp
public partial class MainPage : ContentPage
{
 
    ···
 
    void OnAbsoluteLayoutSizeChanged(object sender, EventArgs args)
    {
        // Get the center and radius of the AbsoluteLayout.
        Point center = new Point(absoluteLayout.Width / 2, absoluteLayout.Height / 2);
        double radius = 0.45 * Math.Min(absoluteLayout.Width, absoluteLayout.Height);

        // Position, size, and rotate the 60 tick marks.
        for (int index = 0; index < tickMarks.Length; index++)
        {
            double size = radius / (index % 5 == 0 ? 15 : 30);
            double radians = index * 2 * Math.PI / tickMarks.Length;
            double x = center.X + radius * Math.Sin(radians) - size / 2;
            double y = center.Y - radius * Math.Cos(radians) - size / 2;
            AbsoluteLayout.SetLayoutBounds(tickMarks[index], new Rectangle(x, y, size, size));
            tickMarks[index].Rotation = 180 * radians / Math.PI;
        }

        // Position and size the three hands.
        LayoutHand(secondHand, secondParams, center, radius);
        LayoutHand(minuteHand, minuteParams, center, radius);
        LayoutHand(hourHand, hourParams, center, radius);
    }

    void LayoutHand(BoxView boxView, HandParams handParams, Point center, double radius)
    {
        double width = handParams.Width * radius;
        double height = handParams.Height * radius;
        double offset = handParams.Offset;

        AbsoluteLayout.SetLayoutBounds(boxView,
            new Rectangle(center.X - 0.5 * width,
                          center.Y - offset * height,
                          width, height));

        // Set the AnchorY property for rotations.
        boxView.AnchorY = handParams.Offset;
    }
 
    ···
 
}
```

`LayoutHand` Yöntemi boyutları ve düz kadar 12:00 konumunu işaret edecek şekilde her elin yerleştirir. Yöntemi, sonunda `AnchorY` saati merkezine karşılık gelen bir konuma özelliğini ayarlayın. Bu Dönüş merkezini belirtir.

Ellerini Zamanlayıcı geri çağırma işlevi döndürülür:

```csharp
public partial class MainPage : ContentPage
{
 
    ···
     
    bool OnTimerTick()
    {
        // Set rotation angles for hour and minute hands.
        DateTime dateTime = DateTime.Now;
        hourHand.Rotation = 30 * (dateTime.Hour % 12) + 0.5 * dateTime.Minute;
        minuteHand.Rotation = 6 * dateTime.Minute + 0.1 * dateTime.Second;

        // Do an animation for the second hand.
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        secondHand.Rotation = 6 * (dateTime.Second + t);
        return true;
    }
}
```

Saniyeyi biraz farklı değerlendirilir: işlevi kolaylaştırma animasyon mekanik yerine kesintisiz göründüğü hareket yapmak için uygulanır. Her değer saniyeyi biraz geri çeker ve hedefine overshoots. Bu küçük bit kod çok hareketini daha iyi için ekler.

## <a name="conclusion"></a>Sonuç

`BoxView` İlk ancak siz en basit görünebilir görülen, oldukça çok yönlü olabilir ve neredeyse yalnızca normalde olası yeniden oluşturması görselleri vektör grafikleri. Daha karmaşık grafiklerde başvurun [kullanarak SkiaSharp Xamarin.Forms içinde](~/xamarin-forms/user-interface/graphics/skiasharp/index.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Temel BoxView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [Metin dekorasyonu (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [Renk ListBox (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [Oyun ömrü (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [İğneli saati (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [BoxView saati (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)
