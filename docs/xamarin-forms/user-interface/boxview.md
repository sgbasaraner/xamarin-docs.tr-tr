---
title: Xamarin.Forms BoxView
description: Bu makalede, düzenleme, grafik ve bir Xamarin.Forms uygulaması etkileşiminde renkli bir dikdörtgen kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: 813a913c2c2fb27456c9a489c73b16d5892c4b8d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997058"
---
# <a name="xamarinforms-boxview"></a>Xamarin.Forms BoxView

[`BoxView`](xref:Xamarin.Forms.BoxView) belirtilen genişlik, yükseklik ve renk basit bir dikdörtgen çizer. Kullanabileceğiniz `BoxView` düzenleme ilkel grafikler ve dokunma aracılığıyla kullanıcı etkileşimi.

Xamarin.Forms yerleşik vektör grafik sistemi olmadığından `BoxView` dengelemek için yardımcı olur. Bu makalede kullanımda açıklanan örnek programlardan bazıları `BoxView` için grafik işleme. `BoxView` Belirli genişlik ve kalınlığını satırının benzeyecek şekilde boyutlandırıldığından ve ardından istediğiniz açı kullanarak Döndürülmüş `Rotation` özelliği.

Ancak `BoxView` basit grafikler taklit, araştırmak isteyebileceğiniz [Xamarin.Forms içinde SkiaSharp kullanma](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) daha karmaşık grafik gereksinimleri.

Bu makalede, aşağıdaki konular ele alınmaktadır:

- **[BoxView rengini ve boyutunu ayarlama](#colorandsize)**  &ndash; ayarlamak `BoxView` özellikleri.
- **[Metin süslemeleri işleme](#textdecorations)**  &ndash; kullanan bir `BoxView` işleme satırlar için.
- **[BoxView renklerle listeleme](#listingcolors)**  &ndash; tüm sistem renkleri görüntülemek bir `ListView`.
- **[Yaşam sınıflara BoxView tarafından oyun oynama](#subclassing)**  &ndash; ünlü bir hücresel Otomasyon uygulayın.
- **[Bir Dijital saat oluşturma](#digitalclock)**  &ndash; iğneli görünen benzetimi.
- **[Bir Analog saati oluşturarak](#analogclock)**  &ndash; dönüştürme ve animasyon `BoxView` öğeleri.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>Ayar BoxView rengini ve boyutunu

Çok sık aşağıdaki üç özelliklerini ayarlarsınız `BoxView`:

- [`Color`](xref:Xamarin.Forms.BoxView.Color) rengini ayarlamak için.
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) genişliğini ayarlamak için `BoxView` CİHAZDAN bağımsız birimler.
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) yüksekliğini ayarlanacak `BoxView`.

`Color` Özelliği türüdür `Color`; herhangi bir özellik ayarlanabilir `Color` 141 statik salt okunur alanları dahil olmak üzere adlandırılmış renkler alfabetik olarak arasında değişen değeri, `AliceBlue` için `YellowGreen`.

`WidthRequest` Ve `HeightRequest` özellikleri, bir rolü yalnızca yürütmek `BoxView` olduğu *sınırlandırılmamış* düzeni. Düzen kapsayıcısının alt kullanıcının boyutu, örneğin, ne zaman bilmek gerektiğinde bu durumda `BoxView` otomatik boyutlu bir hücreye alt `Grid` düzeni. A `BoxView` da sınırlandırılmamış olduğunda kendi `HorizontalOptions` ve `VerticalOptions` özellikleri ayarlanır değerlere dışında `LayoutOptions.Fill`. Varsa `BoxView` sınırlandırılmamış, olan ancak `WidthRequest` ve `HeightRequest` özellikleri ayarlanmadı, ardından genişliği veya yüksekliği 40 birimleri veya yaklaşık 1/4 inç mobil cihazlarda varsayılan değerlere ayarlanır.

`WidthRequest` Ve `HeightRequest` özellikleri yok sayılır varsa `BoxView` olduğu *kısıtlı* içinde çalışması Düzen kapsayıcısı uygular, kendi boyutu üzerinde düzeninde `BoxView`.

A `BoxView` kısıtlı bir boyut ve diğerinde sınırlandırılmamış. Örneğin, varsa `BoxView` dikey alt `StackLayout`, dikey boyutunu `BoxView` olduğu sınırlandırılmamış ve yatay, boyutu genellikle kısıtlanmış. Yatay o boyut için özel durumlar vardır, ancak: varsa `BoxView` sahip kendi `HorizontalOptions` özelliği dışında bir şeye ayarlanmış `LayoutOptions.Fill`, yatay boyutun da sınırlandırılmamış kaldırılır. De mümkündür `StackLayout` sınırlandırılmamış yatay boyut, bu durumda sahip kendisine `BoxView` yatay sınırlandırılmamış olacaktır.

[ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) örnek bir tek inç-sınırlandırılmamış kare görüntüler `BoxView` öğelerinden biri:

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

Sonuç şu şekildedir:

[![Temel BoxView](boxview-images/basicboxview-small.png "temel BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

Varsa `VerticalOptions` ve `HorizontalOptions` özellikler kaldırılma `BoxView` etiketi veya ayarlandığından `Fill`, ardından `BoxView` sayfa boyutu tarafından kısıtlanmış olur ve sayfa dolduracak şekilde genişletilir.

A `BoxView` ayrıca bir alt öğesi olarak bir `AbsoluteLayout`. Bu durumda, konumunu ve boyutunu `BoxView` kullanılarak ayarlanan `LayoutBounds` bağlanılabilir özellik bağlı. `AbsoluteLayout` Makalesinde açıklanan [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md).

Tüm bu durumlarda izleyen örnek programlardan örnekler göreceksiniz.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>Metin süslemeleri işleme

Kullanabileceğiniz `BoxView` bazı basit düzenlemelerini yatay ve dikey çizgileri biçiminde sayfalarınıza eklemek için. [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) örnek bunu gösterir. Tüm programın görsellerin tanımlanan **MainPage.xaml** birkaç içeren dosya `Label` ve `BoxView` öğelerinde `StackLayout` burada gösterilen:

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

Alt öğesi izleyen biçimlendirme tümü `StackLayout`. Bu biçimlendirme dekoratif, çeşitli oluşur `BoxView` ile kullanılan öğeleri `Label` öğesi:

[![Metin süslemesi](boxview-images/textdecoration-small.png "Metin süslemesi")](boxview-images/textdecoration-large.png#lightbox "metin düzenleme")

Sayfanın üstündeki şık başlığı ile elde edilen bir `AbsoluteLayout` dört alt öğeleri olan `BoxView` öğeleri ve `Label`, tüm belirli konumları ve boyutları bir işlem olan atanan:

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

XAML dosyasında `AbsoluteLayout` tarafından izlenen bir `Label` açıklayan metin biçimlendirilmiş `AbsoluteLayout`.

Her ikisi de kapsayan bir metin dizesini alt çizgi `Label` ve `BoxView` içinde bir `StackLayout` olan kendi `HorizontalOptions` değerini ayarlamak için bir şey dışında `Fill`. Genişliği `StackLayout` ardından genişliğini tarafından yönetilir `Label`, daha sonra bu genişliği üzerinde uygular. `BoxView`. `BoxView` Açık bir yükseklik atanır:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Bu teknik, uzun metin dizelerini veya bir paragraf içinde kelimeler altını çizmek için kullanamazsınız.

Kullanmak da mümkündür bir `BoxView` HTML benzeyecek şekilde `hr` (yatay çizgi) öğesi. Yalnızca genişliğini izin `BoxView` , bu durumda, üst kapsayıcı tarafından belirlenen `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

Bir metin paragrafı tarafında her ikisi de kapsayan bir dikey çizgi son olarak, çizebilirsiniz `BoxView` ve `Label` yatay olarak `StackLayout`. Bu durumda, yüksekliğini `BoxView` yüksekliği ile aynıdır `StackLayout`, yüksekliğini tarafından yönetilen `Label`:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
</StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>BoxView renklerle listeleme

`BoxView` Renkleri görüntülemek için kullanışlıdır. Bu programın kullandığı bir `ListView` tüm ortak statik salt okunur alanları Xamarin.Forms listelemek için `Color` yapısı:

[![ListView renkleri](boxview-images/listviewcolors-small.png "ListView renkleri")](boxview-images/listviewcolors-large.png#lightbox "ListView renkleri")

[ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) programı adlı bir sınıf içeren `NamedColor`. Statik Oluşturucu tüm alanları erişmek için yansıma kullanır `Color` yapısı ve oluşturma bir `NamedColor` her biri için nesne. Bu statik depolanan `All` özelliği:

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

Program görseller XAML dosyasında açıklanmaktadır. `ItemsSource` Özelliği `ListView` statik olarak ayarlamanız `NamedColor.All` anlamına özelliği `ListView` tüm tek tek görüntüler `NamedColor` nesneler:

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

`NamedColor` Tarafından biçimlendirilmiş nesneleri `ViewCell` nesnesini veri şablonu olarak ayarlama `ListView`. Bu şablon içeren bir `BoxView` olan `Color` özelliği bağlı `Color` özelliği `NamedColor` nesne.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Oyuna sınıflara BoxView tarafından ömrü

John Melvin matematikçi tarafından oluşturulmuştur ve sayfaları popüler bir hücresel Otomasyon, oyun ömrü olan *bilimsel Amerikan* 1970s içinde. İyi bir giriş Wikipedia makalesi tarafından sağlanan [, Conway'in oyun yaşam](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) programı adlı bir sınıf tanımlar `LifeCell` türetilen `BoxView`. Bu sınıf, tek bir hücre, oyun yaşamında mantığı kapsülleyen:

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

`LifeCell` üç daha fazla özellikler ekler `BoxView`: `Col` ve `Row` özelliklerini depolamak hücrenin grid'in içindeki konumunu ve `IsAlive` özelliği durumunu gösterir. `IsAlive` Özelliği de ayarlar `Color` özelliği `BoxView` hücre etkin değilse, etkin tutulan bağlantıyı destekliyorsa ve beyaz hücreyse siyaha.

`LifeCell` Ayrıca yükleyen bir `TapGestureRecognizer` yararlanılmasını sağlayarak hücreleri durumunu değiştirmek izin vermek için. Sınıf çevirir `Tapped` kendi içine hareket tanıyıcı olaydan `Tapped` olay.

**GameOfLife** program de dahil bir `LifeGrid` çok oyun mantığı kapsülleyen sınıftır ve `MainPage` programın görselleri işleme sınıfı. Bunlar, oyunun kurallarını tanımlayan bir katmana içerir. Eylem birkaç yüz gösteren program işte `LifeCell` sayfasında nesneler:

[![Yaşam oyunu](boxview-images/gameoflife-small.png "Game ömrü")](boxview-images/gameoflife-large.png#lightbox "yaşam oyunu")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>Bir Dijital saat oluşturma

[ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) programın oluşturduğu 210 `BoxView` eski moda bir 5 tarafından 7 iğneli görüntü noktaları benzetimini yapmak için öğeleri. Dikey veya yatay modda saati okuyabilir, ancak yatay görünümde büyüktür:

[![Nokta vuruşlu saat](boxview-images/dotmatrixclock-small.png "iğneli saat")](boxview-images/dotmatrixclock-large.png#lightbox "iğneli saati")

XAML dosyası örneği biraz fazla `AbsoluteLayout` saati kullanılır:

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

Diğer her şey arka plan kod dosyasında gerçekleşir. Nokta vuruşlu ekran mantığı her 10 rakam ve nokta karşılık gelen nokta tanımlayan çeşitli diziler tanımı tarafından önemli ölçüde basitleştirilmiş:

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

Bu alanlar, üç boyutlu bir dizi ile sonlandırma `BoxView` altı için nokta desenleri depolamak için öğeleri.

Oluşturucu tüm oluşturur `BoxView` basamak ve iki nokta üst üste ve ayrıca başlatır için öğeleri `Color` özelliği `BoxView` iki nokta üst üste için öğeleri:

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

Göreli konumlandırma ve boyutlandırma özelliği, bu programın kullandığı `AbsoluteLayout`. Genişlik ve yükseklik her `BoxView` kesirli değer, özellikle %85 1 yatay ve dikey nokta sayısına göre ayrılmış şekilde ayarlanmıştır. Konumları aynı zamanda kesirli değerlere ayarlanır.

Tüm boyutları ve pozisyonları göre toplam boyutu olduğundan `AbsoluteLayout`, `SizeChanged` gerektiğini sayfası için işleyici yalnızca ayarlamak bir `HeightRequest` , `AbsoluteLayout`:

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

Genişliği `AbsoluteLayout` sayfanın tam genişliğine uzatır olduğundan otomatik olarak ayarlanır.

Son kod `MainPage` sınıfı Zamanlayıcısı geri çağırma işlemleri ve her bir rakam, nokta renkleri. Çok boyutlu diziler arka plan kod dosyasının başında tanımını, bu mantıksal program basit bir parçası olun yardımcı olur:

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

Bir nokta vuruşlu saat belirgin bir uygulamayı damgalarıyla görünebilir `BoxView`, ancak `BoxView` öğeleri özellikli bir analog saat fark ayrıca:

[![BoxView saat](boxview-images/boxviewclock-small.png "BoxView saat")](boxview-images/boxviewclock-large.png#lightbox "BoxView saati")

Tüm görsellerde [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) program alt öğesi olan bir `AbsoluteLayout`. Bu öğeleri kullanarak boyutlandırılır `LayoutBounds` ekli özellik ve kullanılarak döndürülen `Rotation` özelliği.

Üç `BoxView` bire bir saat için öğeleri XAML dosyasında örneği ancak konumlandırılmış veya boyutu:

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

Arka plan kod dosyası oluşturucusunun 60 başlatır `BoxView` çevresinde saatin çevresi değer çizgilerinin öğeleri:

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

Boyutlandırma ve konumlandırma tüm `BoxView` öğeleri oluşuyor `SizeChanged` işleyicisi `AbsoluteLayout`. Sınıfı iç biraz yapısı adlı `HandParams` her saatin toplam boyutuna göre üç uygulamalı boyutunu açıklar:

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

`SizeChanged` İşleyici merkezi ve yarıçapını belirler `AbsoluteLayout`, 60 yerleştirir ve ardından boyutları `BoxView` çizgisi kullanılan öğeleri. `for` Döngü sonucuna ayarlayarak `Rotation` özelliği her birinin `BoxView` öğeleri. Sonunda `SizeChanged` işleyicisi `LayoutHand` yöntemi, boyut ve üç saat kullanımına konumlandırmak için çağrılır:

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

`LayoutHand` Yöntemi boyutları ve doğrudan en fazla 12:00 konumu işaret edecek şekilde düzende yerleştirir. Yönteminin sonuna `AnchorY` saatin merkezine karşılık gelen bir konuma özelliğini ayarlayın. Bu, döndürme merkezini belirtir.

Uygulamalı Zamanlayıcısı geri çağırma işlevi döndürülür:

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

Saniyeyi biraz daha farklı olarak kabul edilir: bir animasyonu hızlandırma işlevi görünen taşıma mekanik yerine sorunsuz hale getirmek için uygulanır. Her değer çizgisi saniyeyi biraz geri çeker ve ardından hedefine overshoots. Bu küçük bit kod hareketin gerçekçilik için çok ekler.

## <a name="conclusion"></a>Sonuç

`BoxView` İlk ancak siz en basit görünebilir görülen, çok yönlü olabilir ve neredeyse yalnızca normalde olası yeniden oluşturma görselleri vektör grafikleri. Daha karmaşık grafik için başvurun [Xamarin.Forms içinde SkiaSharp kullanma](~/xamarin-forms/user-interface/graphics/skiasharp/index.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Temel BoxView (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [Metin süslemesi (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [Renk ListBox (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [Oyun ömrü (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [Nokta vuruşlu saati (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [BoxView saati (örnek)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](xref:Xamarin.Forms.BoxView)
