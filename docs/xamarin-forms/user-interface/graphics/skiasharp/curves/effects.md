---
title: SkiaSharp yol etkileri
description: Bu makalede konturlama ve doldurmak için kullanılacak yollara izin verecek ve bu örnek kod ile gösterir çeşitli SkiaSharp yol etkileri açıklanmaktadır.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: charlespetzold
ms.author: chape
ms.date: 07/29/2017
ms.openlocfilehash: 28f628fb4e8ab77e9c36e6e1972d7269ad0dad4d
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615684"
---
# <a name="path-effects-in-skiasharp"></a>SkiaSharp yol etkileri

_Konturlama ve doldurmak için kullanılacak yollara izin verecek çeşitli yol etkileri keşfedin_

A *yolu efektini* örneğidir [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) biri sekiz statik sınıf `Create` yöntemleri. `SKPathEffect` Nesne ayarlanmış ardından [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) özelliği bir `SKPaint` çeşitli ilgi çekici efektleri için küçük bir çoğaltılmış yola sahip bir satır konturlama nesnesi:

![](effects-images/patheffectsample.png "Bağlantılı zinciri örneği")

Yol etkileri için izin ver:

- Fırça darbesi nokta ve tire içeren bir satır
- Fırça darbesi dolgulu bir yolu olan bir satırı
- Tarama satırı ile bir alanı doldurun
- Döşenmiş bir yol ile bir alanı doldurun
- Yuvarlatılmış köşeler diyez olun
- Rastgele "çizgiler ve eğrilerle için Değişimi" ekleyin

Ayrıca, iki veya daha fazla yol etkileri birleştirebilirsiniz.

Bu makalede ayrıca nasıl kullanılacağı gösterilmektedir `GetFillPath` yöntemi `SKPaint` özelliklerini uygulayarak bir yol başka bir yola dönüştürmek `SKPaint`de dahil olmak üzere `StrokeWidth` ve `PathEffect`. Bu, anahat başka bir yolun bir yol elde etme gibi bazı ilginç teknikleri sonuçlanır. `GetFillPath` Ayrıca yol etkileri bağlantılı olarak yararlı olur.

## <a name="dots-and-dashes"></a>Noktalar ve tireler

Kullanımını [ `PathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) yöntemi makalesinde açıklanan [ **nokta ve çizgi**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md). Yöntemin ilk bağımsız değişken uzunluklarının kısa çizgi ve çizgi arasındaki boşlukları uzunluklarının arasında geçiş yapma, iki veya daha fazla değer bir çift sayı içeren bir dizi şöyledir:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Bu değerler *değil* darbe genişliği göre. Örneğin, 10 darbe genişliği ve bir satıra kare çizgi ve kare boşlukları oluşan istediğiniz verilirse `intervals` için dizi {10, 10}. `phase` Bağımsız değişken içinde çizgi desenine satır başladığı belirtir. Kare boşluk ile başlamak için satır istiyorsanız, bu örnekte, ayarlama `phase` 10.

Tireler ucunda etkilenen `StrokeCap` özelliği `SKPaint`. Geniş bir vuruş genişlikleri için bu özelliği ayarlamak yaygın olarak `SKStrokeCap.Round` çizgilerin uçlarını yuvarlanacak. Bu durumda, değerler `intervals` dizi *değil* döngüsel bir nokta sıfır genişliğinde belirtilmesi gerekir, yani ek uzunluğu kaynaklanan yuvarlama gelen içerir. Döngüsel bir nokta ve aynı çapta noktaları arasındaki boşlukları bir çizgi oluşturmak için bir 10 darbe genişliği için kullanmak bir `intervals` dizisi {0, 20}.

**Noktalı metin animasyonlu** sayfasına benzer **özetlenen metin** makalesinde açıklanan sayfa [ **tümleştirme metin ve grafikleri** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) içinde görüntüler, metin karakterleri ayarlayarak özetlenen `Style` özelliği `SKPaint` nesnesini `SKPaintStyle.Stroke`. Ayrıca, **noktalı metin animasyonlu** kullanır `SKPathEffect.CreateDash` vermek için bir noktalı görünümü anahat ve programı da canlandırır `phase` bağımsız değişkeni `SKPathEffect.CreateDash` metin etrafına seyahat görünmektedir nokta kurmak üzere yöntemi karakter. Yatay modda sayfa şöyledir:

[![](effects-images/animateddottedtext-small.png "Üçlü sayfasının ekran görüntüsü noktalı metin animasyonlu")](effects-images/animateddottedtext-large.png#lightbox "Üçlü sayfasının ekran görüntüsü animasyonlu noktalı metin")

[ `AnimatedDottedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Sınıfı bazı sabitleri tanımlayarak başlar ve ayrıca geçersiz kılar `OnAppearing` ve `OnDisappearing` animasyon için yöntemleri:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    const string text = "DOTTED";
    const float strokeWidth = 10;
    static readonly float[] dashArray = { 0, 2 * strokeWidth };

    SKCanvasView canvasView;
    bool pageIsActive;

    public AnimatedDottedTextPage()
    {
        Title = "Animated Dotted Text";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;

        Device.StartTimer(TimeSpan.FromSeconds(1f / 60), () =>
        {
            canvasView.InvalidateSurface();
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`PaintSurface` İşleyici başlar oluşturarak bir `SKPaint` metni görüntülemek için nesne. `TextSize` Özelliği ekran genişliği göre düzeltilir:

```csharp
public class AnimatedDottedTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create an SKPaint object to display the text
        using (SKPaint textPaint = new SKPaint
            {
                Style = SKPaintStyle.Stroke,
                StrokeWidth = strokeWidth,
                StrokeCap = SKStrokeCap.Round,
                Color = SKColors.Blue,
            })
        {
            // Adjust TextSize property so text is 95% of screen width
            float textWidth = textPaint.MeasureText(text);
            textPaint.TextSize *= 0.95f * info.Width / textWidth;

            // Find the text bounds
            SKRect textBounds = new SKRect();
            textPaint.MeasureText(text, ref textBounds);

            // Calculate offsets to center the text on the screen
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            // Animate the phase; t is 0 to 1 every second
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 1 / 1);
            float phase = -t * 2 * strokeWidth;

            // Create dotted line effect based on dash array and phase
            using (SKPathEffect dashEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                // Set it to the paint object
                textPaint.PathEffect = dashEffect;

                // And draw the text
                canvas.DrawText(text, xText, yText, textPaint);
            }
        }
    }
}
```

Yönteminin sonuna doğru `SKPathEffect.CreateDash` yöntemi kullanılarak çağrılır `dashArray` bir alan ve animasyonlu tanımlanan `phase` değeri. `SKPathEffect` Örneği ayarlandığında `PathEffect` özelliği `SKPaint` metni görüntülemek için nesne.

Alternatif olarak, ayarlayabileceğiniz `SKPathEffect` nesnesini `SKPaint` metin ölçme ve sayfada ortalama önce nesne. Bu durumda, ancak animasyonlu nokta ve tire bazı değişim işlenen metin boyutu neden ve metin biraz vibrate eğilimindedir. (Deneyin!)

Ayrıca, metin karakterleri animasyonlu nokta çember olduğunu her kapalı eğri belirli bir noktaya burada noktaların varlığı içine ve dışına pop görünüyor görürsünüz. Yolun karakter anahat tanımlar burada başlar ve biter budur. Yol uzunluğu bir tamsayı çoklu çizgi düzeninde (Bu durumda 20 piksel cinsinden) uzunluğunu değilse bu düzeni yalnızca bir bölümünü yol sonunda uygun olamaz.

Yol uzunluğunun sığdırmak için çizgi desenine uzunluğunu ayarlamak mümkündür, ancak olması bir teknik yolunun uzunluğu belirleme gerektiren gelecek bir makalede ele alınan.

**Nokta / tire Morph** program canlandırır çizgi desenine böylece form çizgi için yeniden birleştirmek nokta ayırmak için tire gibi görünebilir:

[![](effects-images/dotdashmorph-small.png "Üçlü sayfasının ekran görüntüsü nokta Dash Morph")](effects-images/dotdashmorph-large.png#lightbox "nokta Dash Morph sayfanın üç ekran görüntüsü")

[ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Sınıf geçersiz kılmalarını `OnAppearing` ve `OnDisappearing` önceki program oldu, ancak bir sınıf tanımlar gibi yöntemleri `SKPaint` nesnesi bir alan olarak:

```csharp
public class DotDashMorphPage : ContentPage
{
    const float strokeWidth = 30;
    static readonly float[] dashArray = new float[4];

    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint ellipsePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = strokeWidth,
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Create elliptical path
        using (SKPath ellipsePath = new SKPath())
        {
            ellipsePath.AddOval(new SKRect(50, 50, info.Width - 50, info.Height - 50));

            // Create animated path effect
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 3 / 3);
            float phase = 0;

            if (t < 0.25f)  // 1, 0, 1, 2 --> 0, 2, 0, 2
            {
                float tsub = 4 * t;
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2 * tsub;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2;
            }
            else if (t < 0.5f)  // 0, 2, 0, 2 --> 1, 2, 1, 0
            {
                float tsub = 4 * (t - 0.25f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2 * (1 - tsub);
                phase = strokeWidth * tsub;
            }
            else if (t < 0.75f) // 1, 2, 1, 0 --> 0, 2, 0, 2
            {
                float tsub = 4 * (t - 0.5f);
                dashArray[0] = strokeWidth * (1 - tsub);
                dashArray[1] = strokeWidth * 2;
                dashArray[2] = strokeWidth * (1 - tsub);
                dashArray[3] = strokeWidth * 2 * tsub;
                phase = strokeWidth * (1 - tsub);
            }
            else               // 0, 2, 0, 2 --> 1, 0, 1, 2
            {
                float tsub = 4 * (t - 0.75f);
                dashArray[0] = strokeWidth * tsub;
                dashArray[1] = strokeWidth * 2 * (1 - tsub);
                dashArray[2] = strokeWidth * tsub;
                dashArray[3] = strokeWidth * 2;
            }

            using (SKPathEffect pathEffect = SKPathEffect.CreateDash(dashArray, phase))
            {
                ellipsePaint.PathEffect = pathEffect;
                canvas.DrawPath(ellipsePath, ellipsePaint);
            }
        }
    }
}
```

`PaintSurface` İşleyicisi sayfa boyutuna göre bir elips yol oluşturur ve ayarlar kod uzun bir bölümünü yürütür `dashArray` ve `phase` değişkenleri. Animasyonlu değişkeni olarak `t` 1, 0'dan aralıklarına `if` blokları, o zaman dörtte ve her birinde bu Çeyrek bölmek `tsub` 1 aralığı 0-de. Çok sonunda, programın oluşturduğu `SKPathEffect` ve ayarlar `SKPaint` çizim için nesne.

## <a name="from-path-to-path"></a>Yolundan yolu

[ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) Yöntemi `SKPaint` ayarlarında göre başka bir yolu dönüştürür `SKPaint` nesne. Bunun nasıl çalıştığını görmek isterseniz değiştirme `canvas.DrawPath` önceki program aşağıdaki kod ile çağırın:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

Bu yeni kodda `GetFillPath` çağrı dönüştürür `ellipsePath` (yalnızca oval olmayan) uygulamasına `newPath`, ardından görüntülenen ile `newPaint`. `newPaint` Nesnesi, tüm varsayılan özellik ayarlarını oluşturulur dışında `Style` özelliği göre ayarlanmış Boolean değeri döndürür `GetFillPath`.

Görsellerin ayarlanır renk dışında aynı olan `ellipsePaint` ama `newPaint`. İçinde tanımlanan basit bir elips yerine `ellipsePath`, `newPath` noktalar ve tireler dizi tanımlamak çok sayıda yolu dağılımlarını içerir. Çeşitli özelliklerini sonucu budur `ellipsePaint` — `StrokeWidth`, `StrokeCap`, ve `PathEffect` — için `ellipsePath` ve sonuçta elde edilen yolu koymaya `newPath`. `GetFillPath` Yöntemi döndürür belirten bir Boole değeri doldurulması için hedef yolda olup olmadığını; Bu örnekte, dönüş değeri olduğu `true` yolu doldurmak için.

Değiştirmeyi deneyin `Style` ayarı `newPaint` için `SKPaintStyle.Stroke` bir piksel genişliği satırla özetlenen tek yolu dağılımlarını göreceksiniz.

## <a name="stroking-with-a-path"></a>Bir yol ile konturlama

[ `SKPathEffect.Create1DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create1DPath/p/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPath1DPathEffectStyle/) Yöntemdir kavramsal olarak benzer şekilde `SKPathEffect.CreateDash` bir kesik çizgi ve boşluk yerine bir yolu belirtebilirsiniz. Bu yol, vuruş satır veya eğri için birden çok kez çoğaltılır.

Sözdizimi şöyledir:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

> [!IMPORTANT]
> Dikkat: bir aşırı yüklemesini yoktur `Create1DPath` numaralandırma türünde bir bağımsız değişken ile tanımlanan `SkPath1DPathEffect` bir 'k' küçük harfle Bu ad bir hatadır ve numaralandırma ve metodu kullanım dışı bırakılmıştır, ancak kullanım dışı yöntemi kodunuzu bir parçası haline çok kolay bir işlemdir ve tam olarak ne kadar zor olan sonuç olarak yanlış.

Genel olarak, geçirdiğiniz yolun `Create1DPath` küçük ve nokta (0, 0) etrafında ortalanmış olacaktır. `advance` Parametresi satırda yolu çoğaltılmasıyla yolu merkezleri uzaklığı gösterir. Bu bağımsız değişken genellikle yaklaşık genişliğine göre yolunu ayarlayın. `phase` Aynı rol buraya olarak bunu yapar bağımsız değişken yürütür `CreateDash` yöntemi.

[ `SKPath1DPathEffectStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath1DPathEffectStyle/) Üç üyeleri içerir:

- [`Translate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Translate/)
- [`Rotate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Rotate/)
- [`Morph`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Morph/)

`Translate` Üye yol bir çizgi veya eğri çoğaltılmasıyla aynı yönde kalmasına neden olur. İçin `Rotate`, yolun bir eğri teğet göre döndürülür. Normal yönünü yatay çizgiler için yol vardır. `Morph` benzer `Rotate` konturlanan satır eğimi eşleştirilecek yol de eğri hariç aynıdırlar.

**1 D yolu efektini** sayfası, bu üç seçenek gösterir. [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) dosyası numaralandırması üç üyelerine karşılık gelen üç öğeleri içeren bir seçici tanımlar:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.OneDimensionalPathEffectPage"
             Title="1D Path Effect">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="effectStylePicker"
                Title="Effect Style"
                Grid.Row="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Translate</x:String>
                <x:String>Rotate</x:String>
                <x:String>Morph</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1" />
    </Grid>
</ContentPage>
```

[ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) arka plan kod dosyası üç tanımlar `SKPathEffect` alanlar olarak nesneleri. Bu tüm kullanılarak oluşturulan `SKPathEffect.Create1DPath` ile `SKPath` kullanılarak oluşturulan nesneleri `SKPath.ParseSvgPathData`. İlk basit bir kutu, ikincisi ise bir Karo şekli ve üçüncü bir dikdörtgen. Bunlar, üç etkisi stilleri göstermek için kullanılır:

```csharp
public partial class OneDimensionalPathEffectPage : ContentPage
{
    SKPathEffect translatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 -10 L 10 -10, 10 10, -10 10 Z"),
                                  24, 0, SKPath1DPathEffectStyle.Translate);

    SKPathEffect rotatePathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -10 0 L 0 -10, 10 0, 0 10 Z"),
                                  20, 0, SKPath1DPathEffectStyle.Rotate);

    SKPathEffect morphPathEffect =
        SKPathEffect.Create1DPath(SKPath.ParseSvgPathData("M -25 -10 L 25 -10, 25 10, -25 10 Z"),
                                  55, 0, SKPath1DPathEffectStyle.Morph);

    SKPaint pathPaint = new SKPaint
    {
        Color = SKColors.Blue
    };

    public OneDimensionalPathEffectPage()
    {
        InitializeComponent();
    }

    void OnPickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath path = new SKPath())
        {
            path.MoveTo(new SKPoint(0, 0));
            path.CubicTo(new SKPoint(2 * info.Width, info.Height),
                         new SKPoint(-info.Width, info.Height),
                         new SKPoint(info.Width, 0));

            switch (effectStylePicker.Items[effectStylePicker.SelectedIndex])
            {
                case "Translate":
                    pathPaint.PathEffect = translatePathEffect;
                    break;

                case "Rotate":
                    pathPaint.PathEffect = rotatePathEffect;
                    break;

                case "Morph":
                    pathPaint.PathEffect = morphPathEffect;
                    break;
            }

            canvas.DrawPath(path, pathPaint);
        }
    }
}
```

`PaintSurface` İşleyicisi oluşturur, kendisini döngüye girer ve hangi belirleme Seçici erişen Bézier eğri `PathEffect` , vuruş yapmak için kullanılmalıdır. Üç seçenekten — `Translate`, `Rotate`, ve `Morph` — soldan sağa gösterilir:

[![](effects-images/1dpatheffect-small.png "Üçlü sayfasının ekran görüntüsü 1b yolu efektini")](effects-images/1dpatheffect-large.png#lightbox "Üçlü sayfasının ekran görüntüsü 1b yolu etkisi")

Belirtilen yola `SKPathEffect.Create1DPath` yöntemi her zaman doldurulur. Belirtilen yola `DrawPath` yöntemi, her zaman konturlanan `SKPaint` nesnesinin kendi `PathEffect` özelliği 1 D yolu efekti. Dikkat `pathPaint` nesne yok `Style` normalde için varsayılan olarak `Fill`, ancak yolun bakılmaksızın konturlanan.

Kutunun içinde kullanılan `Translate` 20 piksel kare, bir örnektir ve `advance` bağımsız değişkeni, 24'e ayarlanır. Bu fark, yatay veya dikey çizgi kabaca, ancak biraz kutunun alt kenar köşegeni 28,3 piksel olduğundan satırın çapraz olduğunda kutuları üst üste kullanarak kutular arasında bir boşluğa neden olur.

Elmas şeklinde `Rotate` örnektir de 20 piksel genişliğindedir. `advance` Noktaları elmas satır eğimi birlikte döndürülür gibi touch devam eder, 20'ye ayarlanır.

Dikdörtgen şeklinde `Morph` örnektir 50 piksel genişliğinde ile bir `advance` 55 bunlar Bézier eğrisi Eğilmiş gibi küçük dikdörtgenler arasında boşluk ayarını.

Varsa `advance` bağımsız değişkeni olan yolun boyuttan daha sonra çoğaltılan yolları üstüne binebilir. Bu, bazı ilginç etkilere neden olabilir. **Bağlı zinciri** sayfası bir dizi bir catenary ayırıcı şekli kilitleniyor bağlı zinciri benzeyecek şekilde görünen daire çakışan görüntüler:

[![](effects-images/linkedchain-small.png "Üçlü sayfasının ekran görüntüsü bağlı zinciri")](effects-images/linkedchain-large.png#lightbox "Üçlü sayfasının ekran görüntüsü bağlı zinciri")

Çok yakın bakın ve bunlar gerçekten daireler olmayan görürsünüz. Her zincirindeki boyutlandırıldığından ve bitişik bağlantılarla bağlanmak için göründüğü şekilde yerleştirilmiş iki yay bağlantıdır.

Zincir veya kablo Tekdüzen ağırlık dağıtımı bir catenary biçiminde yanıt vermemeye başlıyor. Yerleşik bir ters catenary biçiminde bir arch avantajlar, gelen bir arch ağırlığını baskısı eşit olan bir dağıtım. Catenary görünüşte basit matematik açıklamasını sahiptir:

y = bir · COSH(x / a)

*Cosh* hiperbolik kosinüsü işlevdir. İçin *x* , 0'a eşit *cosh* sıfırdır ve *y* eşittir *bir*. Catenary merkezidir. Gibi *kosinüsü* işlevi *cosh* olduğu söylenir *bile*, anlamına *cosh(–x)* eşittir *cosh(x)*, ve değerler pozitif veya negatif bir bağımsız değişken artırmaya yönelik artırır. Bu değerler, catenary tarafının form eğrileri tanımlar.

Doğru değeri bulma *bir* catenary telefonun sayfası boyutlarına uyacak şekilde doğrudan bir hesaplama değil. Varsa *w* ve *h* genişliği ve en yüksek değeri bir dikdörtgenin yüksekliğini *bir* deneylerin karşılar:

COSH (w/2/a) = 1 + h / a

Aşağıdaki yöntemi [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) sınıfı, sol ve sağ eşittir işareti iki ifadeden başvurarak bu eşitlik içerir `left` ve `right`. Küçük değerler için *bir*, `left` büyüktür `right`; büyük değerler için *bir*, `left` olduğu küçüktür `right`. `while` Döngü en iyi bir değeri temel olarak daralıyor *bir*:

```csharp
float FindOptimumA(float width, float height)
{
    Func<float, float> left = (float a) => (float)Math.Cosh(width / 2 / a);
    Func<float, float> right = (float a) => 1 + height / a;

    float gtA = 1;         // starting value for left > right
    float ltA = 10000;     // starting value for left < right

    while (Math.Abs(gtA - ltA) > 0.1f)
    {
        float avgA = (gtA + ltA) / 2;

        if (left(avgA) < right(avgA))
        {
            ltA = avgA;
        }
        else
        {
            gtA = avgA;
        }
    }

    return (gtA + ltA) / 2;
}
```

`SKPath` Bağlantıları oluşturulan sınıfın oluşturucusu ve sonuç nesnesi `SKPathEffect` nesne ayarlanmış ardından `PathEffect` özelliği `SKPaint` bir alan olarak depolanan nesne:

```csharp
public class LinkedChainPage : ContentPage
{
    const float linkRadius = 30;
    const float linkThickness = 5;

    Func<float, float, float> catenary = (float a, float x) => (float)(a * Math.Cosh(x / a));

    SKPaint linksPaint = new SKPaint
    {
        Color = SKColors.Silver
    };

    public LinkedChainPage()
    {
        Title = "Linked Chain";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the individual links
        SKRect outer = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
        SKRect inner = outer;
        inner.Inflate(-linkThickness, -linkThickness);

        using (SKPath linkPath = new SKPath())
        {
            linkPath.AddArc(outer, 55, 160);
            linkPath.ArcTo(inner, 215, -160, false);
            linkPath.Close();

            linkPath.AddArc(outer, 235, 160);
            linkPath.ArcTo(inner, 395, -160, false);
            linkPath.Close();

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(linkPath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);
        }
    }
    ...
}
```

Ana görevi `PaintSurface` işleyicisidir catenary için bir yol oluşturulacak. En iyi duruma belirledikten sonra *bir* ve içinde depolama `optA` değişken, bunun da penceresinin üst öğesinden bir uzaklık hesaplayın gerekir. Daha sonra koleksiyonu birikebilir `SKPoint` catenary değerleri, bir yola açın ve daha önce oluşturulan bir yol çizmek `SKPaint` nesnesi:

```csharp
public class LinkedChainPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        // Width and height of catenary
        int width = info.Width;
        float height = info.Height - linkRadius;

        // Find the optimum 'a' for this width and height
        float optA = FindOptimumA(width, height);

        // Calculate the vertical offset for that value of 'a'
        float yOffset = catenary(optA, -width / 2);

        // Create a path for the catenary
        SKPoint[] points = new SKPoint[width];

        for (int x = 0; x < width; x++)
        {
            points[x] = new SKPoint(x, yOffset - catenary(optA, x - width / 2));
        }

        using (SKPath path = new SKPath())
        {
            path.AddPoly(points, false);

            // And render that path with the linksPaint object
            canvas.DrawPath(path, linksPaint);
        }
    }
    ...
}
```

Bu program kullanılan yolunu tanımlar `Create1DPath` sağlamak için (0, 0) işaret ortasında. Bu makuldür olduğundan (0, 0) satır ya da bunu donatmak eğri noktası yolun hizalanır. Ancak, bir olmayan merkezli kullanabilirsiniz (0, 0) işaret bazı özel etkiler.

**Taşıyıcı bandı** sayfası bir eğri üst ve alt bu pencerenin boyutlarını boyutta bir dikdörtgen taşıyıcı bandı benzeyen bir yol oluşturur. Yol, bir basit ile konturlanan `SKPaint` 20 piksel genişliğinde ve gri renkli nesnesi ve yeniden konturlanan başka ardından `SKPaint` nesnesi ile bir `SKPathEffect` başvuran küçük bir kova benzeyen bir yol nesnesi:

[![](effects-images/conveyorbelt-small.png "Üçlü sayfasının ekran görüntüsü taşıyıcı bandı")](effects-images/conveyorbelt-large.png#lightbox "Üçlü sayfasının ekran görüntüsü taşıyıcı bandı")

(0, 0) demetine yolu noktasıdır tanıtıcı, bu nedenle `phase` bağımsız değişkeni animasyon, belki de en altta su'kurmak kapsamı ve teslim üstünde dökme taşıyıcı bandı etrafında döndürüleceğini demetlerin gibi görünüyor.

[ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) Sınıfı geçersiz kılmalarına yönelik ile animasyon uygular `OnAppearing` ve `OnDisappearing` yöntemleri. Demet için bir yol sayfanın oluşturucuda tanımlanmıştır:

```csharp
public class ConveyorBeltPage : ContentPage
{
    SKCanvasView canvasView;
    bool pageIsActive = false;

    SKPaint conveyerPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 20,
        Color = SKColors.DarkGray
    };

    SKPath bucketPath = new SKPath();

    SKPaint bucketsPaint = new SKPaint
    {
        Color = SKColors.BurlyWood,
    };

    public ConveyorBeltPage()
    {
        Title = "Conveyor Belt";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Create the path for the bucket starting with the handle
        bucketPath.AddRect(new SKRect(-5, -3, 25, 3));

        // Sides
        bucketPath.AddRoundedRect(new SKRect(25, -19, 27, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);
        bucketPath.AddRoundedRect(new SKRect(63, -19, 65, 18), 10, 10,
                                  SKPathDirection.CounterClockwise);

        // Five slats
        for (int i = 0; i < 5; i++)
        {
            bucketPath.MoveTo(25, -19 + 8 * i);
            bucketPath.LineTo(25, -13 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.CounterClockwise, 65, -13 + 8 * i);
            bucketPath.LineTo(65, -19 + 8 * i);
            bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                             SKPathDirection.Clockwise, 25, -19 + 8 * i);
            bucketPath.Close();
        }

        // Arc to suggest the hidden side
        bucketPath.MoveTo(25, -17);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.Clockwise, 65, -17);
        bucketPath.LineTo(65, -19);
        bucketPath.ArcTo(50, 50, 0, SKPathArcSize.Small,
                         SKPathDirection.CounterClockwise, 25, -19);
        bucketPath.Close();

        // Make it a little bigger and correct the orientation
        bucketPath.Transform(SKMatrix.MakeScale(-2, 2));
        bucketPath.Transform(SKMatrix.MakeRotationDegrees(90));
    }
    ...
```

Demet oluşturma kod biraz daha büyük bir demet ve yan çevirerek kapatma iki dönüşümleriyle tamamlar. Bu dönüşümler uygulanırken, önceki kod tüm koordinatlarında ayarlanması daha kolay.

`PaintSurface` İşleyici taşıyıcı bandı için bir yol tanımlayarak başlar. Bu yalnızca bir çift satırların ve 20 piksel genişliğinde koyu gri çizgi ile çizilir yarı daire çifti.

```csharp
public class ConveyorBeltPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float width = info.Width / 3;
        float verticalMargin = width / 2 + 150;

        using (SKPath conveyerPath = new SKPath())
        {
            // Straight verticals capped by semicircles on top and bottom
            conveyerPath.MoveTo(width, verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, 2 * width, verticalMargin);
            conveyerPath.LineTo(2 * width, info.Height - verticalMargin);
            conveyerPath.ArcTo(width / 2, width / 2, 0, SKPathArcSize.Large,
                               SKPathDirection.Clockwise, width, info.Height - verticalMargin);
            conveyerPath.Close();

            // Draw the conveyor belt itself
            canvas.DrawPath(conveyerPath, conveyerPaint);

            // Calculate spacing based on length of conveyer path
            float length = 2 * (info.Height - 2 * verticalMargin) +
                           2 * ((float)Math.PI * width / 2);

            // Value will be somewhere around 200
            float spacing = length / (float)Math.Round(length / 200);

            // Now animate the phase; t is 0 to 1 every 2 seconds
            TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
            float t = (float)(timeSpan.TotalSeconds % 2 / 2);
            float phase = -t * spacing;

            // Create the buckets PathEffect
            using (SKPathEffect bucketsPathEffect =
                        SKPathEffect.Create1DPath(bucketPath, spacing, phase,
                                                  SKPath1DPathEffectStyle.Rotate))
            {
                // Set it to the Paint object and draw the path again
                bucketsPaint.PathEffect = bucketsPathEffect;
                canvas.DrawPath(conveyerPath, bucketsPaint);
            }
        }
    }
}
```

Bant üzerinde çizim mantığı yatay modda çalışmaz.

Demetlerin 200 piksel taşıyıcı bandı üzerinde uzaklıkta hakkında aralıklı. Ancak, bant üzerinde katı uzun olarak anlamına 200 piksel değil büyük olasılıkla `phase` bağımsız değişkeni `SKPathEffect.Create1DPath` olan animasyon demetlerin içine ve dışına varlığı sorar.

Bu nedenle, program ilk adlı bir değeri hesaplar `length` diğer bir deyişle bant üzerinde uzunluğu. Bu, bant üzerinde düz çizgiler ve yarı daire oluştuğundan, basit bir hesaplamadır. Ardından, demet sayısını bölünmesiyle hesaplanır `length` 200 tarafından. Bu en yakın tamsayıya yuvarlanır ve sayı ise bölünmüş `length`. Bir tamsayı demetler için bir aralık sonucudur. `phase` Olmayan yalnızca bir bölümü, bağımsız değişken.

## <a name="from-path-to-path-again"></a>Yolundan yeniden yolu

Sayfanın alt kısmında `DrawSurface` işleyicisinde **taşıyıcı bandı**, açıklama `canvas.DrawPath` arayın ve aşağıdaki kodla değiştirin:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

Önceki örneği ile `GetFillPath`, sonuçları rengi dışında aynı olduğunu görürsünüz. Yürütmeden sonra `GetFillPath`, `newPath` nesne demetine yolu birden çok kopyasını içeriyorsa, her aynı nokta animasyon bunları aramanın zaman konumlandırılmış konumlandırılmış.

## <a name="hatching-an-area"></a>Bir alan tarama

[ `SKPathEffect.Create2DLines` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DLine/p/System.Single/SkiaSharp.SKMatrix/) Yöntemi paralel satırları, genellikle adı ile bir alanı doldurur *tarama satırları*. Yöntem sözdizimi aşağıdaki gibidir:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width` Tarama çizgilerinin darbe genişliği bağımsız değişkeni belirtir. `matrix` Bir birleşimini döndürme ölçekleme ve isteğe bağlı bir parametredir. Ölçeklendirme çarpanı Skia tarama satırlara boşluk kullanmasını piksel artırma gösterir. Satırları arasındaki ayrımı eksi Ölçeklendirme çarpanı olan `width` bağımsız değişken. Ölçeklendirme çarpanı küçük veya buna eşit olup olmadığı `width` değeri tarama satırlar arasında boşluk olacaktır ve doldurulacak alan görünür. Yatay ve dikey ölçeklendirme için aynı değeri belirtin.

Varsayılan olarak, tarama satırlar yatay olarak gelir. Varsa `matrix` tarama satırları saat yönünde Döndürülmüş, dönüş parametresi içerir.

**Dolgu tarama** sayfası bu yolu etkiyi gösterir. [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) Sınıfı alanları olarak üç yol etkileri tanımlar, olduklarını belirten bir ölçekleme faktörü ile 3 piksel genişliği yatay tarama satırıyla ilk 6 piksel uzaklıkta aralıklı. Satırları arasındaki ayrımı bu nedenle 3 pikseldir. İkinci yol etkisi 24 piksel (Bu nedenle ayrılmasıdır 18 piksel), birbirinden dikey tarama 6 piksel genişliği satırıyla aralıklı aranır ve üçüncü Yatık tarama 12 piksel geniş aralıklı 36 piksel uzaklıkta satırlar aranır.

```csharp
public class HatchFillPage : ContentPage
{
    SKPaint fillPaint = new SKPaint();

    SKPathEffect horzLinesPath = SKPathEffect.Create2DLine(3, SKMatrix.MakeScale(6, 6));

    SKPathEffect vertLinesPath = SKPathEffect.Create2DLine(6,
        Multiply(SKMatrix.MakeRotationDegrees(90), SKMatrix.MakeScale(24, 24)));

    SKPathEffect diagLinesPath = SKPathEffect.Create2DLine(12,
        Multiply(SKMatrix.MakeScale(36, 36), SKMatrix.MakeRotationDegrees(45)));

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

Matris fark `Multiply` yöntemi. Yatay ve dikey ölçekleme faktörü aynı olduğundan, ölçeklendirme ve döndürme matrislerde çarpıldığı sırası önemli değildir.

`PaintSurface` İşleyicisi şu üç yol etkileri birlikte üç farklı renklerde kullanır `fillPaint` Yuvarlatılmış Dikdörtgen sayfaya sığacak şekilde boyutlandırılmış doldurmak için. `Style` Ayarlama özelliği `fillPaint` göz ardı edilir; olduğunda `SKPaint` nesneyi içeren oluşturulan bir yolu etkisi `SKPathEffect.Create2DLine`, alan bakılmaksızın doldurulur:

```csharp
public class HatchFillPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath roundRectPath = new SKPath())
        {
            // Create a path
            roundRectPath.AddRoundedRect(
                new SKRect(50, 50, info.Width - 50, info.Height - 50), 100, 100);

            // Horizontal hatch marks
            fillPaint.PathEffect = horzLinesPath;
            fillPaint.Color = SKColors.Red;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Vertical hatch marks
            fillPaint.PathEffect = vertLinesPath;
            fillPaint.Color = SKColors.Blue;
            canvas.DrawPath(roundRectPath, fillPaint);

            // Diagonal hatch marks -- use clipping
            fillPaint.PathEffect = diagLinesPath;
            fillPaint.Color = SKColors.Green;

            canvas.Save();
            canvas.ClipPath(roundRectPath);
            canvas.DrawRect(new SKRect(0, 0, info.Width, info.Height), fillPaint);
            canvas.Restore();

            // Outline the path
            canvas.DrawPath(roundRectPath, strokePaint);
        }
    }
    ...
}
```

Sonuçları dikkatle bakarsanız, tarama kırmızı ve mavi satırlar için tam olarak Yuvarlatılmış Dikdörtgen sınırlı olmayan görürsünüz. (Bu görünüşe göre bir temel Skia kod özelliğidir.) Bu yetersiz ise, yeşil Yatık tarama satırlar için alternatif bir yaklaşım gösterilir: Yuvarlatılmış Dikdörtgen bir kırpma yolu olarak kullanılır ve tarama satırları sayfanın tamamını çizilir.

`PaintSurface` İşleyici sonucuna yalnızca tarama kırmızı ve mavi satırlar ile tutarsızlık görebilmeniz için Yuvarlatılmış Dikdörtgen vuruş yapmak için bir çağrı ile:

[![](effects-images/hatchfill-small.png "Üçlü sayfasının ekran görüntüsü dolgu tarama")](effects-images/hatchfill-large.png#lightbox "Üçlü sayfasının ekran görüntüsü dolgu tarama türü")

Android ekran gerçekten gibi görünmüyor: ekran görüntüsü ölçeklendirme ince kırmızı çizgiler ve daha geniş görünüşte kırmızı çizgiler birleştirmek için ince alanları ve daha geniş alanları neden oldu.

## <a name="filling-with-a-path"></a>Bir yol ile doldurma

[ `SKPathEffect.Create2DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DPath/p/SkiaSharp.SKMatrix/SkiaSharp.SKPath/) Yürürlükte döşeme alanı yatay ve dikey olarak çoğaltılmış bir yol ile bir alanı dolduracak şekilde sağlar:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix` Ölçekleme faktörü çoğaltılmış yolun yatay ve dikey boşluğu gösterir. Ancak bu kullanarak yolu döndürülemiyor `matrix` bağımsız değişkeni; Döndürülmüş yolu istiyorsanız yolu döndürme kullanarak `Transform` yöntemi tarafından tanımlanan `SKPath`.

Çoğaltılmış yolu normalde doldurulan alanı yerine ekranın sol ve üst kenarlar ile hizalanır. 0 ile sol ve üst iki tarafından yatay ve dikey uzaklık belirtmenizi ölçekleme faktörü arasında çeviri Etkenler sağlayarak bu davranışı geçersiz kılabilirsiniz.

**Yolu kutucuk doldurma** sayfası bu yolu etkiyi gösterir. Alan döşeme için kullanılan yolu alan olarak tanımlı [ `PathFileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) sınıfı. Bu yolu 80 piksel kare olduğu anlamına gelir yatay ve dikey koordinatları aralığından – 40, 40:

```csharp
public class PathTileFillPage : ContentPage
{
    SKPath tilePath = SKPath.ParseSvgPathData(
        "M -20 -20 L 2 -20, 2 -40, 18 -40, 18 -20, 40 -20, " +
        "40 -12, 20 -12, 20 12, 40 12, 40 40, 22 40, 22 20, " +
        "-2 20, -2 40, -20 40, -20 8, -40 8, -40 -8, -20 -8 Z");
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Red;

            using (SKPathEffect pathEffect =
                   SKPathEffect.Create2DPath(SKMatrix.MakeScale(64, 64), tilePath))
            {
                paint.PathEffect = pathEffect;

                canvas.DrawRoundRect(
                    new SKRect(50, 50, info.Width - 50, info.Height - 50),
                    100, 100, paint);
            }
        }
    }
}
```

İçinde `PaintSurface` işleyicisi `SKPathEffect.Create2DPath` çağrıları yatay ve dikey boşluğu 80 piksel kare kutucukları çakışmasına neden 64'e ayarlar. Neyse ki, kutucukları bitişik ile sorunsuz şekilde meshing Bulmacanın bir yol benzer:

[![](effects-images/pathtilefill-small.png "Üçlü sayfasının ekran görüntüsü yolu kutucuk doldurma")](effects-images/pathtilefill-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yolu kutucuk doldurun")

Özgün ekran görüntüsünden ölçeklendirme özellikle Android ekranda bazı bozulmaya neden olur.

Bu kutucukların her zaman tüm görünür ve hiçbir zaman kesilir dikkat edin. İlk iki ekran görüntüleri alan doldurulan bir Yuvarlatılmış Dikdörtgen olduğunu da yetkisiz değiştirmeye karşı korumalı değil. Bu kutucuklar için belirli bir alanı kesmek istiyorsanız, kırpma yolu kullanın.

Ayar deneyin `Style` özelliği `SKPaint` nesnesini `Stroke`, tek tek kutucuklara doldurulmuş yerine özetlenen göreceksiniz.

## <a name="rounding-sharp-corners"></a>Keskin köşeleri yuvarlama

**Heptagon yuvarlatılmış** program kısmında sunulmuştur [ **yay çizmenin üç yolu** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) makale Eğim yay yedi taraflı bir şekil noktalarını eğri için kullanılır. **Başka bir Heptagon yuvarlatılmış** sayfası gösterir oluşturulan bir yolu etkisi kullanan bir çok daha kolay bir yaklaşım [ `SKPathEffect.CreateCorner` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCorner/p/System.Single/) yöntemi:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

Tek bağımsız değişken olarak adlandırılır ancak `radius` için yarı istenen köşe yarıçapı ayarlamanız gerekir. (Temel alınan Skia kodun bir karakteristik budur.)

İşte `PaintSurface` işleyicisinde [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) sınıfı:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);
    SKPoint[] vertices = new SKPoint[numVertices];
    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    float cornerRadius = 100;

    // Create the path
    using (SKPath path = new SKPath())
    {
        path.AddPoly(vertices, true);

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            // Set argument to half the desired corner radius!
            paint.PathEffect = SKPathEffect.CreateCorner(cornerRadius / 2);

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);

            // Uncomment DrawCircle call to verify corner radius
            float offset = cornerRadius / (float)Math.Sin(Math.PI * (numVertices - 2) / numVertices / 2);
            paint.Color = SKColors.Green;
            // canvas.DrawCircle(vertices[0].X, vertices[0].Y + offset, cornerRadius, paint);
        }
    }
}
```

Bu etkiyi konturlama veya temel doldurma ile kullanabileceğiniz `Style` özelliği `SKPaint` nesne. Bu üç tüm platformlarda şöyledir:

[![](effects-images/anotherroundedheptagon-small.png "Üçlü sayfasının ekran görüntüsü başka bir Heptagon yuvarlatılmış")](effects-images/anotherroundedheptagon-large.png#lightbox "başka bir Heptagon yuvarlatılmış sayfanın üç ekran görüntüsü")

Bu yuvarlatılmış heptagon önceki programın aynı olduğunu görürsünüz. Daha fazla ikna gerekiyorsa köşe yarıçapı gerçekten 100 yerine 50 belirtilen `SKPathEffect.CreateCorner` çağrısı açıklamasını bkz: 100-radius daire ve program son deyiminde koyulmuş üst köşesindeki.

## <a name="random-jitter"></a>Rastgele değişimi

Bazen aciliyetinin düz çizgiler bilgisayar grafik oldukça istediklerinizi olmayan ve az doğrulukla istenen. Bu durumda, denemek isteyebilirsiniz [ `SKPathEffect.CreateDiscrete` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDiscrete/p/System.Single/System.Single/System.UInt32/) yöntemi:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

Bu yolu efektini konturlama veya doldurmak için kullanabilirsiniz. Satırları bağlı parçalara ayrılmış — yaklaşık uzunluğu tarafından belirtilen `segLength` — ve farklı yönlere genişletebilirsiniz. Orijinal satıra sapma kapsamını tarafından belirtilen `deviation`.

Efekt için kullanılan sözde rastgele bir sıra oluşturmak için kullanılan çekirdek son bağımsız değişkeni var. Değişimi etkisi farklı hızlarına biraz farklı görünür. Bağımsız değişken her programı çalıştırdığınızda etkisini aynı olduğu anlamına gelir sıfır, bir varsayılan değerine sahiptir. Ekranı yeniden çizilmesini her farklı değişimi istiyorsanız, çekirdek ayarlayabilirsiniz `Millisecond` özelliği bir `DataTime.Now` (örneğin) bir değer.


**Değişimi deneme** sayfası bir dikdörtgen konturlama içinde farklı değerlerle denemenizi sağlar:

[![](effects-images/jitterexperiment-small.png "Üçlü değişimi deneme sayfasının ekran görüntüsü")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

Straightfoward programdır. [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) dosya iki başlatır `Slider` öğeleri ve bir `SKCanvasView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Curves.JitterExperimentPage"
             Title="Jitter Experiment">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Minimum" Value="0" />
                    <Setter Property="Maximum" Value="100" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="segLengthSlider"
                Grid.Row="0"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference segLengthSlider},
                              Path=Value,
                              StringFormat='Segment Length = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="deviationSlider"
                Grid.Row="2"
                ValueChanged="sliderValueChanged" />

        <Label Text="{Binding Source={x:Reference deviationSlider},
                              Path=Value,
                              StringFormat='Deviation = {0:F0}'}"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

`PaintSurface` İşleyicisinde [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) arka plan kod dosyası çağrıldığında her bir `Slider` değişiklikleri değeri. Çağrı `SKPathEffect.CreateDiscrete` iki kullanarak `Slider` değerleri ve bir dikdörtgen vuruş yapmak için kullanan:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float segLength = (float)segLengthSlider.Value;
    float deviation = (float)deviationSlider.Value;

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.StrokeWidth = 5;
        paint.Color = SKColors.Blue;

        using (SKPathEffect pathEffect = SKPathEffect.CreateDiscrete(segLength, deviation))
        {
            paint.PathEffect = pathEffect;

            SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
            canvas.DrawRect(rect, paint);
        }
    }
}
```

Bu etkiyi bu durumda doldurulmuş alanın ana hatlarını rastgele bu sapmalar tabi olur doldurmak için de kullanabilirsiniz. **Metin değişimi** sayfasında metni görüntülemek için bu yolu efekt kullanarak gösterir. Kodun çoğu `PaintSurface` işleyicisi [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) sınıfı boyutlandırma ve metin ortalama zamanının:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "FUZZY";

    using (SKPaint textPaint = new SKPaint())
    {
        textPaint.Color = SKColors.Purple;
        textPaint.PathEffect = SKPathEffect.CreateDiscrete(3f, 10f);

        // Adjust TextSize property so text is 95% of screen width
        float textWidth = textPaint.MeasureText(text);
        textPaint.TextSize *= 0.95f * info.Width / textWidth;

        // Find the text bounds
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        // Calculate offsets to center the text on the screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Burada, tüm üç platformlarda yatay modda çalışıyor:

[![](effects-images/jittertext-small.png "Üç metin değişimi sayfasının ekran görüntüsü")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>Anahat oluşturma yolu

İki küçük örnekler gördünüz [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) yöntemi `SKPaint`, bulunduğu aynı zamanda bir [aşırı](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/):

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale)
```

Yalnızca ilk iki bağımsız değişkenleri gereklidir. Yöntem tarafından başvurulan yolu erişir `src` bağımsız değişkeni, vuruş özelliklerini temel yol verileri değiştiren `SKPaint` nesne (dahil olmak üzere `PathEffect` özelliği) ve ardından sonuçları içine Yazar `dst` yolu. `resScale` Parametresi, daha küçük bir hedef yolu oluşturmak için düşürmeyi olanak sağlar ve `cullRect` bağımsız değişkeni bir dikdörtgen dışında dağılımlarını ortadan kaldırabilir.

Bu yöntemin bir temel kullanım yol etkileri hiç gerektirmez. Varsa `SKPaint` nesnesinin kendi `Style` özelliğini `SKPaintStyle.Stroke`ve *değil* sahip kendi `PathEffect` ayarlayın, ardından `GetFillPath` temsil eden bir yol oluşturur bir *anahat*kaynak yolunun alacağı bunu Boya özellikleri tarafından konturlanan.

Örneğin, varsa `src` yoludur RADIUS 500, basit bir daire ve `SKPaint` nesnesini bir darbe genişliği 100 belirtir sonra `dst` yolu 550, bir RADIUS ile 450 ve başka bir RADIUS ile iki Eşmerkezli daire haline gelir. Yöntemin çağrıldığı `GetFillPath` bu doldurma çünkü `dst` yoludur konturlama aynı `src` yolu. Ancak ayrıca vuruş `dst` yol anahatlarını görmek için yol.

**Anahattına yolu dokunun** bu gösterir. `SKCanvasView` Ve `TapGestureRecognizer` örneği oluşturulur [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) dosya. [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) arka plan kod dosyası üç tanımlar `SKPaint` nesneler olarak alanları, iki ile konturlama vuruş genişlikleri 100, 20 ve üçüncü doldurma için:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    bool outlineThePath = false;

    SKPaint redThickStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 100
    };

    SKPaint redThinStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 20
    };

    SKPaint blueFill = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    public TapToOutlineThePathPage()
    {
        InitializeComponent();
    }

    void OnCanvasViewTapped(object sender, EventArgs args)
    {
        outlineThePath ^= true;
        (sender as SKCanvasView).InvalidateSurface();
    }
    ...
}
```

Ekran dokunulduğunda değil, `PaintSurface` işleyicisi kullanır `blueFill` ve `redThickStroke` boyama dairesel bir yola işlemek için nesneler:

```csharp
public partial class TapToOutlineThePathPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circlePath = new SKPath())
        {
            circlePath.AddCircle(info.Width / 2, info.Height / 2,
                                 Math.Min(info.Width / 2, info.Height / 2) -
                                 redThickStroke.StrokeWidth);

            if (!outlineThePath)
            {
                canvas.DrawPath(circlePath, blueFill);
                canvas.DrawPath(circlePath, redThickStroke);
            }
            else
            {
                using (SKPath outlinePath = new SKPath())
                {
                    redThickStroke.GetFillPath(circlePath, outlinePath);

                    canvas.DrawPath(outlinePath, blueFill);
                    canvas.DrawPath(outlinePath, redThinStroke);
                }
            }
        }
    }
}
```

Daire doldurulmuş ve beklediğiniz gibi konturlanan:

[![](effects-images/taptooutlinethepathnormal-small.png "Üçlü sayfasının ekran görüntüsü normal dokunun anahat yolu")](effects-images/taptooutlinethepathnormal-large.png#lightbox "Üçlü sayfasının ekran görüntüsü normal dokunun anahat yolu")

Ekran dokunduğunuzda `outlineThePath` ayarlanır `true`ve `PaintSurface` işleyicisi oluşturur yükseltilmek `SKPath` hedef yolu için bir çağrı olarak kullanan ve nesne `GetFillPath` üzerinde `redThickStroke` Boya nesne. Bu hedef yolu girdikten sonra ve ile konturlanan `redThinStroke`, aşağıdaki sonuç:

[![](effects-images/taptooutlinethepathoutlined-small.png "Üçlü sayfasının ekran görüntüsü anahatları belirlenmiş dokunun anahat yolu")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "Üçlü sayfasının ekran görüntüsü anahatları belirlenmiş dokunun anahat yolu")

İki kırmızı daire iki döngüsel dağılımlarını özgün döngüsel yolu dönüştürüldü açıkça gösterir.

Bu yöntem için kullanılacak yollar geliştirirken çok yararlı olabilir. `SKPathEffect.Create1DPath` yöntemi. Bu yöntemleri belirttiğiniz yolları, yolları çoğaltıldığında her zaman doldurulur. Doldurulacak yolun tamamını istemiyorsanız, ana hatlarını dikkatli bir şekilde tanımlamanız gerekir.

Örneğin, **bağlı zinciri** örnek bağlantıları tanımlanan dört yaylar, her çift alan yolunun doldurulması için ana hatlarını belirlemek için iki yarıçaplarını dayalı bir dizi. Kodu değiştirmek mümkündür `LinkedChainPage` beklediğinizden biraz farklı yapmak için sınıf.

İlk olarak, yeniden tanımlanacak isteyeceksiniz `linkRadius` sabit:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath` Artık yalnızca iki yay ile istenen bu tek RADIUS göre açıları başlatmak ve tarama açısı:

```csharp
using (SKPath linkPath = new SKPath())
{
    SKRect rect = new SKRect(-linkRadius, -linkRadius, linkRadius, linkRadius);
    linkPath.AddArc(rect, 55, 160);
    linkPath.AddArc(rect, 235, 160);

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = linkThickness;

        using (SKPath outlinePath = new SKPath())
        {
            strokePaint.GetFillPath(linkPath, outlinePath);

            // Set that path as the 1D path effect for linksPaint
            linksPaint.PathEffect =
                SKPathEffect.Create1DPath(outlinePath, 1.3f * linkRadius, 0,
                                          SKPath1DPathEffectStyle.Rotate);

        }

    }
}
```

`outlinePath` Nesnedir sonra ana hat alıcı `linkPath` zaman bunu konturlanan belirtilen özelliklerle `strokePaint`.

Bu tekniği kullanarak başka bir örnek kullanılan yolu için İleri'kurmak geldiği bir `SKPathEffect.Create2DPath` yöntemleri.

## <a name="combining-path-effects"></a>Yol etkileri birleştirme

İki son statik oluşturma yöntemlerini `SKPathEffect` olan [ `SKPathEffect.CreateSum` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateSum/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/) ve [ `SKPathEffect.CreateCompose` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCompose/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Her iki yöntem bir bileşik yolu efektini oluşturmak için iki yol etkileri birleştirin. `CreateSum` Yöntemi oluşturur, ayrı ayrı uygulanan iki yol etkileri için benzer bir yol etkisi sırada `CreateCompose` bir yolu efekti uygular ( `inner`) ve uygular `outer` .

Gördünüz nasıl `GetFillPath` yöntemi `SKPaint` göre başka bir yol için bir yol dönüştürebilirsiniz `SKPaint` özellikleri (dahil olmak üzere `PathEffect`) olmaması gerekir böylece *çok* bir nasılgizemli`SKPaint`nesne iki kez belirtilen iki yol etkileri olan bu işlemi gerçekleştirebilir `CreateSum` veya `CreateCompose` yöntemleri.

Bir açık kullanımını `CreateSum` tanımlamak için bir `SKPaint` ile bir yol etkili bir yol doldurur ve başka bir yolu etkisi olan yolun konturlar nesne. Bu gösterilmiştir **kediler çerçevesinde** Fistolu kenarları kediler çerçevesi içinde bir dizi görüntüleyen örnek:

[![](effects-images/catsinframe-small.png "Üçlü sayfasının ekran görüntüsü kediler, çerçeve")](effects-images/catsinframe-large.png#lightbox "Üçlü sayfasının ekran görüntüsü kediler, çerçeve")

[ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) Sınıfı çeşitli alanları tanımlayarak başlar. İlk alanından tanıyabilir [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) gelen sınıfı [ **SVG yol verileri** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) makalesi. İkinci yol bir çizgi ve yay çerçevenin Tarak deseni için temel alır:

```csharp
public class CatsInFramePage : ContentPage
{
    // From PathDataCatPage.cs
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint catStroke = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5
    };

    SKPath scallopPath =
        SKPath.ParseSvgPathData("M 0 0 L 50 0 A 60 60 0 0 1 -50 0 Z");

    SKPaint framePaint = new SKPaint
    {
        Color = SKColors.Black
    };
    ...
}
```

`catPath` İçinde kullanılabilir `SKPathEffect.Create2DPath` yöntemi varsa `SKPaint` nesne `Style` özelliği `Stroke`. Ancak, varsa `catPath` bu programda, ardından tüm cat başı doldurulur ve hatta yatay çizgilerinin yüzde birlik görünmeyecektir doğrudan kullanılır. (Deneyin!) Bu yolu özetini almak ve bu anahat içinde kullanmak için gerekli olan `SKPathEffect.Create2DPath` yöntemi.

Oluşturucu, bu işi yapar. İlk iki dönüşümler geçerlidir `catPath` taşımak (0, 0) için ilgili center'ın üzerine gelin ve ölçeği, boyutu. `GetFillPath` tüm ana hatlarını içinde dağılımlarını alır `outlinedCatPath`, ve o nesnenin kullanılır `SKPathEffect.Create2DPath` çağırın. Ölçeklendirme Etkenler `SKMatrix` değer yatay biraz daha büyük ve çeviri Etkenler sürerken kutucukları arasında küçük bir arabellek sağlamaya cat dikey boyutu biraz türetilmiş türü tam cat görünür olmasını sağlayın çerçevenin sol üst köşesinin:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    public CatsInFramePage()
    {
        Title = "Cats in Frame";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Move (0, 0) point to center of cat path
        catPath.Transform(SKMatrix.MakeTranslation(-240, -175));

        // Now catPath is 400 by 250
        // Scale it down to 160 by 100
        catPath.Transform(SKMatrix.MakeScale(0.40f, 0.40f));

        // Get the outlines of the contours of the cat path
        SKPath outlinedCatPath = new SKPath();
        catStroke.GetFillPath(catPath, outlinedCatPath);

        // Create a 2D path effect from those outlines
        SKPathEffect fillEffect = SKPathEffect.Create2DPath(
            new SKMatrix { ScaleX = 170, ScaleY = 110,
                           TransX = 75, TransY = 80,
                           Persp2 = 1 },
            outlinedCatPath);

        // Create a 1D path effect from the scallop path
        SKPathEffect strokeEffect =
            SKPathEffect.Create1DPath(scallopPath, 75, 0, SKPath1DPathEffectStyle.Rotate);

        // Set the sum the effects to frame paint
        framePaint.PathEffect = SKPathEffect.CreateSum(fillEffect, strokeEffect);
    }
    ...
}
```

Sonra oluşturucu çağırır `SKPathEffect.Create1DPath` Fistolu çerçevesi için. Yol genişliği 100 pikseldir, ancak çoğaltılmış yolu etrafında çerçeve örtüşüyor 75 piksel öncelikli olduğundan dikkat edin. Son ifade oluşturucu çağrılarının `SKPathEffect.CreateSum` iki yol etkileri birleştirmeniz ve sonuç kümesine `SKPaint` nesne.

Bu iş sağlayan `PaintSurface` oldukça basit olmasını işleyici. Yalnızca dikdörtgen tanımlama ve kullanma çizmek gereken `framePaint`:

```csharp
public class CatsInFramePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect rect = new SKRect(50, 50, info.Width - 50, info.Height - 50);
        canvas.ClipRect(rect);
        canvas.DrawRect(rect, framePaint);
    }
}
```

Yol etkileri algoritmalar, her zaman bazı görseller dışında dikdörtgen görünmesine neden görüntülenecek konturlama veya doldurma için kullanılan dosyanın tam yolunu neden. `ClipRect` Öncesinde çağrı `DrawRect` çağrı görsellerin önemli ölçüde daha temiz olmasını sağlar. (Kırpma olmadan deneyin!)

Kullanımı yaygındır `SKPathEffect.CreateCompose` bazı değişimi başka bir yolu efekt eklemek için. Kesinlikle kendi kendinize deneyebilirsiniz ancak biraz farklı bir örnek aşağıda verilmiştir:

**Kesikli tarama satırları** elips kesikli satırları tarama ile doldurur. Çoğu iş [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) sınıfı gerçekleştirilir alan tanımları sağ. Bu alanlar, bir çizgi etkili olur ve bir tarama etkisi tanımlayın. Olarak tanımlanan `static` çünkü bunlar ardından, başvurulan bir `SKPathEffect.CreateCompose` Çağır `SKPaint` tanımı:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    static SKPathEffect dashEffect =
        SKPathEffect.CreateDash(new float[] { 30, 30 }, 0);

    static SKPathEffect hatchEffect = SKPathEffect.Create2DLine(20,
        Multiply(SKMatrix.MakeScale(60, 60),
                 SKMatrix.MakeRotationDegrees(45)));

    SKPaint paint = new SKPaint()
    {
        PathEffect = SKPathEffect.CreateCompose(dashEffect, hatchEffect),
        StrokeCap = SKStrokeCap.Round,
        Color = SKColors.Blue
    };
    ...
    static SKMatrix Multiply(SKMatrix first, SKMatrix second)
    {
        SKMatrix target = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref target, first, second);
        return target;
    }
}
```

`PaintSurface` İşleyici yalnızca standart yükü artı bir çağrı içeren gerek `DrawOval`:

```csharp
public class DashedHatchLinesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        canvas.DrawOval(info.Width / 2, info.Height / 2,
                        0.45f * info.Width, 0.45f * info.Height,
                        paint);
    }
    ...
}
```

Keşfettiğiniz, tarama satırları alanının iç için tam olarak sınırlı değildir ve bu örnekte, bunlar her zaman sol tam bir kısa çizgi ile başlar:

[![](effects-images/dashedhatchlines-small.png "Üçlü sayfasının ekran görüntüsü kesik çizgili tarama satırları")](effects-images/dashedhatchlines-large.png#lightbox "Üçlü sayfasının ekran görüntüsü tarama satırları kesik çizgili")

Basit noktalar ve tireler arasındayken garip birleşimleri için yol etkileri gördüğünüze göre hayal gücünüzü kullanmak ve oluşturmak bkz.



## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
