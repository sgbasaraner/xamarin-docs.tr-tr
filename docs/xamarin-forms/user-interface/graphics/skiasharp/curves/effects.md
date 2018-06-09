---
title: SkiaSharp yolu efektleri
description: Bu makalede vuruş yapması ve doldurmak için kullanılacak yollara izin verecek ve bu örnek kodu ile gösterir çeşitli SkiaSharp yol etkilerini açıklar.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 95167D1F-A718-405A-AFCC-90E596D422F3
author: charlespetzold
ms.author: chape
ms.date: 07/29/2017
ms.openlocfilehash: 2071a2fb140d0e9c78d4c86d6aa70d3606dc1f98
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244116"
---
# <a name="path-effects-in-skiasharp"></a>SkiaSharp yolu efektleri

_Yollara vuruş yapması ve doldurmak için kullanılacak izin verecek çeşitli yol etkilerini Bul_

A *yolu etkisi* örneği [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) sekiz statik biri ile oluşturulan sınıf `Create` yöntemleri. `SKPathEffect` Nesne ayarlanmış sonra [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) özelliği bir `SKPaint` küçük bir çoğaltılmış yola sahip bir satır vuruş yapması nesne ilginç efektler çeşitli için:

![](effects-images/patheffectsample.png "Bağlantılı zinciri örnek")

Yol etkilerini imkan tanır:

- Nokta ve tire içeren bir satır vuruş
- Vuruş dolgulu bir yolu olan bir satır
- Tarama satırlarıyla alanını doldurun
- Bir alanı döşeli bir yolu ile doldurma
- Yuvarlanmış köşeleri diyez olun
- Rastgele "çizgiler ve eğrilerle Değişimi" ekleyin

Ayrıca, iki veya daha fazla yol etkileri birleştirebilirsiniz.

Bu makalede ayrıca nasıl kullanılacağı gösterilmektedir `GetFillPath` yöntemi `SKPaint` özelliklerini uygulayarak başka bir yola bir yolu dönüştürmek için `SKPaint`dahil `StrokeWidth` ve `PathEffect`. Bu, başka bir yol anahattı bir yol alma gibi bazı ilginç teknikleri sonuçlanır. `GetFillPath` Ayrıca yolu etkileri bağlantılı olarak yardımcı olur.

## <a name="dots-and-dashes"></a>Nokta ve tire

Kullanımını [ `PathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) yöntemi makalesinde açıklanan [ **nokta ve kısa çizgilerden**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md). Yönteminin ilk bağımsız değişkeni bir çift sayı tire uzunlukları ve tireler arasındaki boşlukları uzunluklarının arasında geçiş yapma, iki veya daha fazla değer içeren bir dizi şöyledir:

```csharp
public static SKPathEffect CreateDash (Single[] intervals, Single phase)
```

Bu değerler *değil* vuruşun genişliğini göre. Örneğin, 10 vuruşun genişliğini ise ve bir satır kare çizgi ve kare boşlukların oluşan istediğiniz, ayarlar `intervals` için dizi {10, 10}. `phase` Bağımsız değişkeni gösterir tire deseni içinde satır başladığı. Çizginin kare boşluk ile başlamasını istiyorsanız, bu örnekte, ayarlayın `phase` 10.

Çizgilerin uçlarını etkilenir `StrokeCap` özelliği `SKPaint`. Geniş vuruş genişlikler için bu özelliği ayarlamak için çok yaygındır `SKStrokeCap.Round` çizgilerin uçlarını yuvarlanacak. Bu durumda, değerler `intervals` dizi *değil* döngüsel bir nokta sıfır genişliğini belirtilmesi gerekir yani ek uzunluğu yuvarlama alanından kaynaklanan eklemek. Döngüsel noktalar ve aynı çapta noktalar arasındaki boşluk ile bir satır oluşturmak için 10 vuruşun genişliğini için kullanmak bir `intervals` dizisi {0, 20}.

**Noktalı metin animasyonlu** sayfa benzer **özetlenen metin** makalede açıklanan sayfa [ **tümleştirme metin ve grafik** ](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md) içinde metin karakterlerinin ayarlayarak ana hatlarıyla gösterir, `Style` özelliği `SKPaint` nesnesine `SKPaintStyle.Stroke`. Ayrıca, **noktalı metin animasyonlu** kullanır `SKPathEffect.CreateDash` vermek için bir noktalı görünümü anahat ve program ayrıca canlandırır `phase` bağımsız değişkeni `SKPathEffect.CreateDash` metni etrafında seyahat göründüğü nokta yapma yöntemi karakter. Yatay modunda sayfa şöyledir:

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

`PaintSurface` İşleyici başlar oluşturarak bir `SKPaint` metni görüntülemek için nesne. `TextSize` Özelliği ekran genişliği göre ayarlandı:

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

Yönteminin sonuna doğru `SKPathEffect.CreateDash` yöntemi kullanarak çağrılır `dashArray` bir alan ve animasyonlu tanımlanan `phase` değeri. `SKPathEffect` Örneği ayarlanmış `PathEffect` özelliği `SKPaint` metni görüntülemek için nesne.

Alternatif olarak, ayarlayabileceğiniz `SKPathEffect` nesnesine `SKPaint` metin ölçme ve sayfada ortalama önce nesnesi. Bu durumda, ancak animasyonlu nokta ve tire bazı değişim İşlenmiş metin boyutunda neden ve metin biraz Titret eğilimindedir. (Deneyin!)

Ayrıca, metin karakterlerinin geçici animasyonlu nokta daire olduğunu belirli bir noktaya her kapalı eğrisindeki burada noktaların varlığı ve bu moddan pop göründüğü görürsünüz. Burada karakter ana hattını tanımlayan yol başlar ve biter budur. Yol uzunluğu tire deseni (Bu durumda 20 piksel) uzunluğunu tamsayı katı değilse bu deseni yalnızca parçası yolun sonunda uygun olamaz.

Yol uzunluğu uyacak şekilde tire deseni uzunluğunu ayarlamak mümkündür, ancak gerektiren olması bir teknik yolunun uzunluğu belirleme gelecekteki bir makalede ele alınan.

**Nokta / tire Morph** program canlandırır tire deseni böylece form tire yeniden birleştirme nokta bölmek için çizgi gibi görünebilir:

[![](effects-images/dotdashmorph-small.png "Üçlü sayfasının ekran görüntüsü nokta tire Morph")](effects-images/dotdashmorph-large.png#lightbox "Üçlü sayfasının ekran görüntüsü noktalı çizgi Morph")

[ `DotDashMorphPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DotDashMorphPage.cs) Geçersiz kılmaları sınıf `OnAppearing` ve `OnDisappearing` yöntemleri önceki program oldu, ancak sınıf tanımlar gibi `SKPaint` nesnesi bir alan olarak:

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

`PaintSurface` İşleyicisi sayfa boyutuna göre Eliptik bir yol oluşturur ve ayarlar kod uzun bir bölümünü yürütür `dashArray` ve `phase` değişkenleri. Animasyonlu değişken olarak `t` 1, 0'dan aralıklarına `if` blokları, bu süre içine dört üç aylık dönem ve her bu üç aylık dönemlerde bölmek `tsub` da 0 ile 1 aralığında için. Program çok sonunda oluşturur `SKPathEffect` ve ayarlar `SKPaint` çizim için nesnesi.

## <a name="from-path-to-path"></a>Yol yolu

[ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) Yöntemi `SKPaint` başka ayarlarında dayalı bir yolu dönüştürür `SKPaint` nesnesi. Bunun nasıl çalıştığını görmek için yerini `canvas.DrawPath` önceki programın aşağıdaki kod ile çağırın:

```csharp
SKPath newPath = new SKPath();
bool fill = ellipsePaint.GetFillPath(ellipsePath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);

```

Bu yeni kodda `GetFillPath` çağrısı dönüştürür `ellipsePath` (yalnızca bir oval olmayan) uygulamasına `newPath`, ardından görüntülendiği ile `newPaint`. `newPaint` Nesnesi, tüm varsayılan özellik ayarlarıyla oluşturulur dışında `Style` özelliği göre ayarlanmış üzerinde Boolean dönüş değerini `GetFillPath`.

Görsel ayarlanmış olan renk dışında aynı `ellipsePaint` ama `newPaint`. Tanımlanan basit elips yerine `ellipsePath`, `newPath` nokta ve tire dizi tanımlayan çok sayıda yol dağılımlarını içerir. Bu, çeşitli özelliklerini uygulamanın sonucu olan `ellipsePaint` — `StrokeWidth`, `StrokeCap`, ve `PathEffect` — için `ellipsePath` ve sonuç yolu koymaya `newPath`. `GetFillPath` Yöntemi döndürür gösteren bir Boole değeri doldurulması için hedef yolu olup olmadığını; Bu örnekte, dönüş değeri `true` yolu doldurmak için.

Değiştirmeyi deneyin `Style` ayarı `newPaint` için `SKPaintStyle.Stroke` ve bir piksel genişliği satırıyla özetlenen tek tek yolu dağılımlarını görürsünüz.

## <a name="stroking-with-a-path"></a>İle bir yola vuruş yapması

[ `SKPathEffect.Create1DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create1DPath/p/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPath1DPathEffectStyle/) Yöntemi benzer kavramsal olarak `SKPathEffect.CreateDash` dışında çizgi ve boşlukların desenini yerine bir yol belirtin. Bu yol, vuruş satırı veya eğri için birden çok kez çoğaltılır.

Sözdizimi şöyledir:

```csharp
public static SKPathEffect Create1DPath (SKPath path, Single advance,
                                         Single phase, SKPath1DPathEffectStyle style)
```

> [!IMPORTANT]
> Dikkat: bir aşırı yüklemesini yoktur `Create1DPath` numaralandırma türünde bir bağımsız değişken ile tanımlanan `SkPath1DPathEffect` küçük 'k.' ile Bu ad bir hatadır ve numaralandırma ve metodu kullanım dışı bırakılmıştır, ancak çok daha kolay bir kullanım dışı yöntemi kodunuzu bir parçası haline gelir ve tam olarak görmek zordur sonuç olarak yanlış.

Genel olarak, geçişi için yol `Create1DPath` küçük ve (0, 0) noktası etrafında ortalanmış olacaktır. `advance` Parametresi yolu satırda çoğaltılır gibi yol merkezleri uzaklığı gösterir. Bu değişken genellikle yolu yaklaşık genişliğine ayarlayın. `phase` Aynı role burada şekliyle yapar bağımsız değişkeni yürütür `CreateDash` yöntemi.

[ `SKPath1DPathEffectStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath1DPathEffectStyle/) Üç üyeler içeriyor:

- [`Translate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Translate/)
- [`Rotate`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Rotate/)
- [`Morph`](https://developer.xamarin.com/api/field/SkiaSharp.SKPath1DPathEffectStyle.Morph/)

`Translate` Üye yolun çizgi veya eğri çoğaltılmış olarak aynı yönde kalmasına neden olur. İçin `Rotate`, yolun bir eğri teğet göre döndürülür. Yolun normal yönünü yatay çizgiler için vardır. `Morph` benzer `Rotate` yolu da vuruş satır eğimi eşleşecek şekilde eğri dışında.

**1 D yolu etkisi** sayfanın şu üç seçenekten gösterir. [ **OneDimensionalPathEffectPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml) dosyası numaralandırması üç üyelerine karşılık gelen üç öğeleri içeren bir seçici tanımlar:

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

[ **OneDimensionalPathEffectPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/OneDimensionalPathEffectPage.xaml.cs) arka plan kod dosyası üç tanımlar `SKPathEffect` nesneleri alanları olarak. Bunlar tüm kullanılarak oluşturulan `SKPathEffect.Create1DPath` ile `SKPath` kullanılarak oluşturulan nesneler `SKPath.ParseSvgPathData`. İlk basit bir kutu, ikinci bir Karo şekli ve üçüncü dikdörtgen şeklinde. Bu, üç etkisi stilleri göstermek için kullanılır:

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

`PaintSurface` İşleyicisi oluşturur, kendisini döngüye girer ve hangi belirleme Seçici erişen bir Bézier eğrisi `PathEffect` , vuruş yapmak için kullanılmalıdır. Üç seçenekten — `Translate`, `Rotate`, ve `Morph` — soldan sağa gösterilir:

[![](effects-images/1dpatheffect-small.png "Üçlü sayfasının ekran görüntüsü 1D yolu etkisi")](effects-images/1dpatheffect-large.png#lightbox "Üçlü sayfasının ekran görüntüsü 1 D yolu etkisi")

Belirtilen yol `SKPathEffect.Create1DPath` yöntemi her zaman doldurulur. Belirtilen yol `DrawPath` yöntemi, her zaman vuruş `SKPaint` nesnesi kendi `PathEffect` özelliği 1 D yolu etkiyi ayarlanmış. Dikkat `pathPaint` nesne sahip Hayır `Style` normalde için varsayılan olarak `Fill`, ancak yolun bakılmaksızın vuruş.

İçinde kullanılan kutusunu `Translate` 20 piksel kare, bir örnektir ve `advance` bağımsız değişkeni, 24'e ayarlanır. Bu farkı yatay veya dikey çizgi kabaca, ancak kutusunun çapraz 28,3 piksel olduğundan satır çapraz olduğunda kutuları biraz üst üste kutuları arasında bir boşluk neden olur.

Elmas şeklinde `Rotate` örnektir aynı zamanda 20 piksel genişliğinde. `advance` Noktalarının elmas satır eğimi birlikte döndürülmüş olarak touch devam şekilde 20'ye ayarlayın.

Dikdörtgen şeklinde `Morph` örnektir 50 piksel genişliğinde ile bir `advance` bunlar Bézier eğrisi Bükülü gibi küçük bir boşluk dikdörtgenler arasında yapmak için 55 ayarlama.

Varsa `advance` bağımsız değişkeni, yolun boyutunu değerinden sonra çoğaltılmış yolları binebilir. Bu, bazı etkileri neden olabilir. **Bağlantılı zinciri** sayfası bir catenary farklı şeklinde askıda bir bağlantılı zinciri benzer görünüyor daireler çakışan bir dizi görüntüler:

[![](effects-images/linkedchain-small.png "Üçlü sayfasının ekran görüntüsü bağlantılı zinciri")](effects-images/linkedchain-large.png#lightbox "Üçlü sayfasının ekran görüntüsü bağlantılı zinciri")

Çok yakın bakın ve bunlar aslında daireler olmayan görürsünüz. Her zincirindeki boyutlandırılmış ve bitişik bağlantılarıyla bağlanmak göründüğü şekilde konumlandırılmış iki yaylar bağlantıdır.

Zincir veya kablo Tekdüzen ağırlık dağıtımının bir catenary biçiminde askıda kalır. Ters catenary biçiminde yerleşik bir arch avantaj baskısı bir arch ağırlığını gelen, eşit bir dağıtım. Catenary görünen basit matematiksel açıklamasını sahiptir:

y = bir · COSH(x / a)

*Cosh* hiperbolik kosinüsünü işlevdir. İçin *x* 0, eşit *cosh* sıfır ve *y* eşittir *bir*. Catenary merkezi olmasıdır. Gibi *kosinüsünü* işlevi, *cosh* olarak kabul edilir *bile*, anlamına *cosh(–x)* eşittir *cosh(x)*, ve değerlerini artırmak olumlu veya olumsuz bağımsız değişkenleri artırma. Bu değerleri catenary yanlarından form Eğriler açıklanmaktadır.

Doğru değeri bulma *bir* catenary telefonun sayfa boyutlarına uyacak şekilde doğrudan bir hesaplama değil. Varsa *w* ve *h* genişlik ve yükseklik dikdörtgenin, en uygun değeri *bir* deneylerin karşılar:

COSH (w/2/a) = 1 + h / a

Aşağıdaki yöntemi [ `LinkedChainPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/LinkedChainPage.cs) sınıfı, sol ve sağ eşittir işaretinin iki ifadeye başvurarak bu eşitlik içerir `left` ve `right`. Küçük değerler için *bir*, `left` değerinden daha büyük `right`; büyük değerler için *bir*, `left` olan değerinden `right`. `while` Döngü en iyi bir değeri temel olarak daraltır *bir*:

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

`SKPath` Bağlantıları sınıfının oluşturucusu ve sonuç oluşturulan için nesne `SKPathEffect` nesne ayarlanmış sonra `PathEffect` özelliği `SKPaint` bir alan olarak depolanan nesnesi:

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

Ana iş `PaintSurface` işleyicisidir catenary kendisi için bir yol oluşturulacak. En iyi duruma belirledikten sonra *bir* ve de depolamak `optA` değişken, bu da pencerenin üstündeki uzaklık hesaplamak gerekiyor. Ardından, bir koleksiyonu birikebilir `SKPoint` catenary değerlerini yola açın ve daha önce oluşturulmuş yolu çizin `SKPaint` nesnesi:

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

Kullanılan yolu bu programı tanımlayan `Create1DPath` sağlamak için (0, 0) noktası Merkezi'nde. Bu makul gibi görünür olduğundan (0, 0) satır veya onu eklemek eğri noktası yolun hizalanması. Ancak, bir harici merkezli kullanabilirsiniz (0, 0) noktası bazı özel etkiler.

**Taşıyıcı bandı** sayfa dikdörtgen taşıyıcı bandı eğri üst ve alt Bu pencere boyutları için boyutta benzeyen bir yol oluşturur. İle basit bir yol vuruş `SKPaint` 20 piksel genişliğinde ve renkli gri nesnesi ve yeniden vuruş başka bir işlemle sonra `SKPaint` nesnesi ile bir `SKPathEffect` az kova benzeyen bir yolu başvuran nesnesi:

[![](effects-images/conveyorbelt-small.png "Üçlü sayfasının ekran görüntüsü taşıyıcı bandı")](effects-images/conveyorbelt-large.png#lightbox "Üçlü sayfasının ekran görüntüsü taşıyıcı bandı")

(0, 0) demet yolu noktasıdır böyle olduğunda, tanıtıcı `phase` bağımsız değişkeni animasyonlu, belki de su altındaki kapsamı ve onu en üstünde dökme taşıyıcı bandı çevresinde döndürüleceğini demet gibi görünebilir.

[ `ConveyorBeltPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConveyorBeltPage.cs) Sınıfı geçersiz Kılmalara göre ile animasyon uygulayan `OnAppearing` ve `OnDisappearing` yöntemleri. Yolun kabı için sayfanın oluşturucuda tanımlanır:

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

Demet oluşturma kodu demet biraz daha büyük ve betiklerdeki kapatma iki dönüşümler ile tamamlar. Bu dönüşümler uygulanırken önceki kod tüm koordinatlarında ayarlama daha kolay.

`PaintSurface` İşleyici taşıyıcı bandı için bir yol tanımlayarak başlar. Bu, yalnızca satır çifti ve 20 piksel genişliğinde gri koyu satırıyla çizilir noktalı daireler çiftinden oluşur:

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

Taşıyıcı bandı çizim için mantığı yatay modda çalışmaz.

Demet 200 piksel taşıyıcı bandı birbirinden hakkında aralıklı. Ancak, taşıyıcı bandı uzun olarak anlamına 200 piksel olan büyük olasılıkla `phase` bağımsız değişkeni `SKPathEffect.Create1DPath` olan animasyon demet içine ve dışına varlığı pop.

Bu nedenle, program ilk adlı değeri hesaplar `length` diğer bir deyişle taşıyıcı bandı uzunluğu. Düz çizgiler ve noktalı daireler taşıyıcı bandı oluşur, basit bir hesaplama olmasıdır. Ardından, demet sayısı bölünmesiyle hesaplanır `length` 200 tarafından. Bu en yakın tamsayıya yuvarlanır ve sayı ise bölünmüş `length`. Bir tam sayı sayısı için bir aralığı sonucudur. `phase` Olmayan yalnızca bir kısmı, bağımsız değişken.

## <a name="from-path-to-path-again"></a>Yol yeniden yolu

Ekranın alt kısmındaki `DrawSurface` işleyicisinde **taşıyıcı bandı**, çıkışı açıklama `canvas.DrawPath` çağırın ve aşağıdaki kod ile değiştirin:

```csharp
SKPath newPath = new SKPath();
bool fill = bucketsPaint.GetFillPath(conveyerPath, newPath);
SKPaint newPaint = new SKPaint
{
    Style = fill ? SKPaintStyle.Fill : SKPaintStyle.Stroke
};
canvas.DrawPath(newPath, newPaint);
```

Önceki örneği ile `GetFillPath`, sonuçları rengi dışında aynı olduğunu görürsünüz. Yürütme sonrasında `GetFillPath`, `newPath` nesnesini içeren birden çok kopyasını demet yolu, her aynı nokta animasyonun bunları çağrı aynı anda konumlandırılmış konumlandırıldı.

## <a name="hatching-an-area"></a>Bir alanı tarama

[ `SKPathEffect.Create2DLines` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DLine/p/System.Single/SkiaSharp.SKMatrix/) Yöntemi doldurur adlandırılırlar paralel çizgi sahip bir alan *tarama satırları*. Yöntem sözdizimi aşağıdaki gibidir:

```csharp
public static SKPathEffect Create2DLine (Single width, SKMatrix matrix)
```

`width` Bağımsız değişkeni tarama satırları vuruşun genişliğini belirtir. `matrix` Parametresi ölçekleme ve isteğe bağlı döndürme birleşimidir. Ölçeklendirme faktörü Skia tarama satırları boşluk kullanmasını piksel artışı gösterir. Satırları arasında ayrım eksi ölçeklendirme faktördür `width` bağımsız değişkeni. Ölçeklendirme faktörü küçük veya eşit olup olmadığını `width` değeri, tarama satırları boşluk olacaktır ve alan doldurulacak görüntülenir. Yatay ve dikey ölçekleme için aynı değeri belirtin.

Varsayılan olarak, tarama satırları yatay. Varsa `matrix` tarama satırları saat yönünde Döndürülmüş, parametre döndürme içerir.

**Tarama doldurun** sayfa bu yolu etkiyi gösterir. [ `HatchFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/HatchFillPage.cs) Sınıfı tanımlayan üç yolu etkileri alanlar olarak, 6 piksel birbirinden olduklarını belirten bir ölçeklendirme faktörü ile 3 piksel genişliği yatay tarama satırıyla ilk aralıklı. Satırları arasında ayrım bu nedenle 3 pikseldir. 6 piksel genişliği dikey tarama satırıyla 24 piksel (Bu nedenle ayrılmasıdır 18 piksel), birbirinden aralıklı için ikinci yol etkili olan ve üçüncü Yatık tarama 12 piksel geniş aralıklı 36 piksel birbirinden satırlar aranır.

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

Matris fark `Multiply` yöntemi. Yatay ve dikey ölçeklendirme etkenleri aynı olduğundan, ölçeklendirme ve döndürme matrisleri çarpıldığı sırası önemli değildir.

`PaintSurface` İşleyici ile birlikte üç farklı renklerle bu üç yolu etkilerle kullanan `fillPaint` sayfaya sığacak şekilde boyutlandırılmış yuvarlak dikdörtgen doldurmak için. `Style` Özelliği üzerinde ayarlanmış `fillPaint` göz ardı edilir; zaman `SKPaint` nesnesi içerir oluşturulan bir yolu etkisi `SKPathEffect.Create2DLine`, alan bakılmaksızın doldurulur:

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

Sonuçları dikkatle bakarsanız, kırmızı ve mavi tarama satırları tam olarak yuvarlak dikdörtgen sınırlı değil görürsünüz. (Bu görünüşe göre bir arka plandaki Skia kod özelliğidir.) Bu yetersiz ise, yeşil Yatık tarama satırlar için alternatif bir yaklaşım gösterilir: yuvarlak dikdörtgen kırpma yolu olarak kullanılır ve tarama çizgileri tüm sayfasında çizilir.

`PaintSurface` İşleyici sonucuna çağrısıyla kırmızı ve mavi tarama satırları ile tutarsızlık görebilmeleri yuvarlak dikdörtgen yalnızca vuruş yapmak için:

[![](effects-images/hatchfill-small.png "Üçlü sayfasının ekran görüntüsü tarama dolgu")](effects-images/hatchfill-large.png#lightbox "Üçlü sayfasının ekran görüntüsü tarama doldurun")

Android ekran gerçekten gibi görünmüyor: ekran görüntüsü ölçeklendirme ince kırmızı çizgiler ve ekranınızda daha geniş kırmızı satırlarına birleştirmek için ince alanları ve daha geniş alanları neden oldu.

## <a name="filling-with-a-path"></a>Bir yol ile doldurma

[ `SKPathEffect.Create2DPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.Create2DPath/p/SkiaSharp.SKMatrix/SkiaSharp.SKPath/) Yürürlükte döşeme alanı yatay ve dikey olarak çoğaltılmış bir yol ile bir alanı dolduracak şekilde sağlar:

```csharp
public static SKPathEffect Create2DPath (SKMatrix matrix, SKPath path)
```

`SKMatrix` Ölçeklendirme etkenleri çoğaltılmış yolunun yatay ve dikey boşluğu gösterir. Ancak bu kullanarak yolu döndürülemiyor `matrix` bağımsız değişkeni; Döndürülmüş yolu istiyorsanız yol döndürmek kullanarak `Transform` tarafından tanımlanan yöntemi `SKPath`.

Çoğaltılmış yolu normal olarak doldurulan alanı yerine ekranın sol ve üst kenarlar ile hizalanır. 0 ve yatay ve dikey uzaklık sol ve üst iki tarafından belirtmek için ölçeklendirme etkenleri arasında çeviri Etkenler sağlayarak bu davranışı geçersiz kılabilirsiniz.

**Yolu döşemeyi doldurmak** sayfa bu yolu etkiyi gösterir. Alanı döşeme için kullanılan yolu alanı olarak tanımlanan [ `PathFileFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathTileFillPage.cs) sınıfı. Bu yol 80 piksel kare olduğu anlamına gelir yatay ve dikey koordinatlarını aralıktan – 40 40 için:

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

İçinde `PaintSurface` işleyicisi `SKPathEffect.Create2DPath` çağrıları yatay ve dikey boşluğu 80 piksel kare kutucuklar çakışmasına neden 64'e ayarlar. Neyse ki, yolun döşeme bitişik ile sorunsuz şekilde meshing Bulmacanın bir benzer:

[![](effects-images/pathtilefill-small.png "Üçlü sayfasının ekran görüntüsü yolu döşemeyi doldurmak")](effects-images/pathtilefill-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yolu döşeme doldurma")

Özgün ekran görüntüsünde ölçeklendirme özellikle Android ekranda bazı bozulmaya neden olur.

Bu kutucuklar her zaman tüm görünür ve hiçbir zaman kesilmiş dikkat edin. İlk iki ekran görüntüleri üzerinde doldurulan alanı yuvarlak dikdörtgen şeklinde bile korumalı değil. Bu kutucuklar için belirli bir alandaki bir kesecek şekilde kırpma yolu kullanın.

Ayarı deneyin `Style` özelliği `SKPaint` nesnesine `Stroke`, doldurulmuş yerine ana hatlarıyla tek tek kutucuklar görürsünüz.

## <a name="rounding-sharp-corners"></a>Sharp köşeleri yuvarlama

**Yuvarlanmasını Heptagon** program sunulan [ **yay çizmek için üç yol** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) makale bir Eğim yay yedi taraflı şekil noktalarının eğri için kullanılır. **Başka bir yuvarlanmasını Heptagon** sayfası gösterir, oluşturulan bir yolu efekti kullanan bir çok daha kolay yaklaşım [ `SKPathEffect.CreateCorner` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCorner/p/System.Single/) yöntemi:

```csharp
public static SKPathEffect CreateCorner (Single radius)
```

Tek bir bağımsız değişken adlı rağmen `radius` yarım istenen köşe RADIUS ile ayarlamanız gerekir. (Bu bir arka plandaki Skia kod özelliğidir.)

Burada `PaintSurface` işleyicisinde [ `AnotherRoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/AnotherRoundedHeptagonPage.cs) sınıfı:

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

Bu etkiyi vuruş yapması veya temel doldurma ile kullanabileceğiniz `Style` özelliği `SKPaint` nesnesi. Bu üç tüm platformlarda şöyledir:

[![](effects-images/anotherroundedheptagon-small.png "Üçlü sayfasının ekran görüntüsü başka bir yuvarlanmasını Heptagon")](effects-images/anotherroundedheptagon-large.png#lightbox "Üçlü sayfasının ekran görüntüsü başka bir yuvarlanmasını Heptagon")

Bu yuvarlak heptagon önceki programın aynı olduğunu görürsünüz. Daha fazla ikna gerekiyorsa köşe yarıçapını gerçekten 100 yerine 50 belirtilen `SKPathEffect.CreateCorner` çağrısı, açıklamadan çıkarın program ve bkz: 100 RADIUS daire son deyiminde koyulan köşede.

## <a name="random-jitter"></a>Rastgele değişimi

Bazen bilgisayar grafik kusursuz düz çizgilerden istediğinizi oldukça değildir ve küçük rastgele istenen. Bu durumda, deneyin istersiniz [ `SKPathEffect.CreateDiscrete` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDiscrete/p/System.Single/System.Single/System.UInt32/) yöntemi:

```csharp
public static SKPathEffect CreateDiscrete (Single segLength, Single deviation, UInt32 seedAssist)
```

Bu yol etkiyi vuruş yapması veya doldurmak için kullanabilirsiniz. Satırları bağlı parçalara ayrılmış — yaklaşık uzunluğu tarafından belirtilen `segLength` — ve farklı yönlerde genişletebilirsiniz. Özgün satır sapma kapsamını tarafından belirtilen `deviation`.

Son değişken efekt için kullanılan sözde rastgele dizisi oluşturmak için kullanılan çekirdek değeri. Değişim etkisi farklı oluştururken çekirdeği biraz farklı görünecektir. Bağımsız değişken her programı çalıştırdığınızda etkisi aynı olduğu anlamına gelir sıfır varsayılan bir değeri yok. Ekran yeniden çizilmiş her farklı değişim istiyorsanız, çekirdek ayarlayabileceğiniz `Millisecond` özelliği bir `DataTime.Now` (örneğin) değeri.


**Değişimi denemeler** sayfası dikdörtgen vuruş yapması içinde farklı değerler deneme sağlar:

[![](effects-images/jitterexperiment-small.png "Üçlü değişimi deneme sayfasının ekran görüntüsü")](effects-images/jitterexperiment-large.png#lightbox "Triple screenshot of the JitterExperiment page")

Straightfoward programdır. [ **JitterExperimentPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml) dosya başlatır iki `Slider` öğeleri ve bir `SKCanvasView`:

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

`PaintSurface` İşleyicisinde [ **JitterExperimentPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterExperimentPage.xaml.cs) arka plan kod dosyasına çağrılır her bir `Slider` değer değişiklikleri. Çağırır `SKPathEffect.CreateDiscrete` iki kullanarak `Slider` dikdörtgen vuruş yapmak için kullanan ve değerler:

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

Bu etkiyi; bu durumda doldurulmuş alanı özetini rastgele bu sapmalar tabi olduğunu doldurmak için de kullanabilirsiniz. **Değişimi metin** sayfa gösteren metni görüntülemek için bu yolu etkiyi kullanıyor. Kodda çoğu `PaintSurface` işleyicisine [ `JitterTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/JitterTextPage.cs) sınıfı boyutlandırma ve metin ortalama zamanının:

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

Burada, tüm üç platformlarda yatay modunda çalışıyor:

[![](effects-images/jittertext-small.png "Üçlü değişimi metin sayfasının ekran görüntüsü")](effects-images/jittertext-large.png#lightbox "Triple screenshot of the JitterText page")

## <a name="path-outlining"></a>Yol anahat oluşturma

İki çok az örnekler gördünüz [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/System.Single/) yöntemi `SKPaint`, bulunduğu aynı zamanda bir [aşırı](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/):

```csharp
public Boolean GetFillPath (SKPath src, SKPath dst, Single resScale)

public Boolean GetFillPath (SKPath src, SKPath dst, SKRect cullRect, Single resScale)
```

Yalnızca ilk iki bağımsız değişkenler zorunludur. Yöntem tarafından başvurulan yolu erişir `src` bağımsız değişkeni, vuruş özelliklerine göre yolu verilerini değiştirir `SKPaint` nesne (dahil olmak üzere `PathEffect` özelliği) ve ardından sonuçları içine Yazar `dst` yolu. `resScale` Parametresi verir daha küçük bir hedef yolu oluşturmak için duyarlılığı azaltır ve `cullRect` bağımsız değişkeni bir dikdörtgen dışında dağılımlarını ortadan kaldırabilir.

Bu yöntemin bir temel kullanım yolu etkileri hiç gerektirmez. Varsa `SKPaint` nesnesi kendi `Style` özelliğini `SKPaintStyle.Stroke`ve *değil* sahip kendi `PathEffect` ayarlayın, sonra `GetFillPath` temsil eden bir yol oluşturur bir *anahat*kaynak yolunun bu boyama özellikleri tarafından vuruş olur.

Örneğin, varsa `src` yoludur RADIUS 500, basit bir daire ve `SKPaint` nesnesini 100 vuruşun genişliğini belirtir sonra `dst` yolu 450 ve diğer RADIUS 550 yarıçapını ile biriyle iki eşmerkezli daireler olur. Yöntem çağrıldığında `GetFillPath` bu doldurma çünkü `dst` yol aynıdır vuruş yapması `src` yolu. Ancak aynı zamanda vuruş `dst` yol anahatları görmek için yol.

**Yol anahattına dokunun** bu gösterir. `SKCanvasView` Ve `TapGestureRecognizer` içinde örneği [ **TapToOutlineThePathPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml) dosya. [ **TapToOutlineThePathPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TapToOutlineThePathPage.xaml.cs) arka plan kod dosyası üç tanımlar `SKPaint` nesneleri ile vuruş yapması için iki alanları olarak vuruş 100 ve 20 ve doldurma üçüncü genişliklerini:

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

Ekran dokunduğunuz değil, `PaintSurface` işleyici kullanan `blueFill` ve `redThickStroke` boyamak döngüsel bir yol oluşturulacak nesneler:

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

Daireye doldurulmuş ve beklediğiniz gibi vuruş:

[![](effects-images/taptooutlinethepathnormal-small.png "Üçlü sayfasının ekran görüntüsü normal dokunun anahat yolu")](effects-images/taptooutlinethepathnormal-large.png#lightbox "Üçlü sayfasının ekran görüntüsü normal dokunun anahat yolu")

Ekran dokunduğunuzda `outlineThePath` ayarlanır `true`ve `PaintSurface` işleyicisi oluşturur baştan `SKPath` yapılan bir çağrı hedef yolu olarak kullanan ve nesne `GetFillPath` üzerinde `redThickStroke` boyama nesnesi. Hedef yol girdikten sonra ve ile vuruş `redThinStroke`, sonuçta elde edilen aşağıdaki:

[![](effects-images/taptooutlinethepathoutlined-small.png "Üçlü sayfasının ekran görüntüsü Anahatlı dokunun anahat yolu")](effects-images/taptooutlinethepathoutlined-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Anahatlı dokunun anahat yolu")

İki kırmızı daire açıkça özgün döngüsel yolu iki döngüsel dağılımlarını dönüştürülmüş gösterir.

Bu yöntem için kullanılacak yollar geliştirirken çok yararlı olabilir `SKPathEffect.Create1DPath` yöntemi. Bu yöntemleri belirttiğiniz yolları, her zaman yolları çoğaltıldığında doldurulur. Doldurulacak yolun tamamını istemiyorsanız, anahatları dikkatle tanımlamanız gerekir.

Örneğin, **bağlantılı zinciri** örnek, bağlantıları tanımlanan dört yaylar, her çifti doldurulması için yol alanına ana hatlarını belirlemek için iki yarıçaplarını dayalı bir dizi. Kodla mümkündür `LinkedChainPage` biraz farklı yapmak için sınıf.

İlk olarak, yeniden tanımlamak istersiniz `linkRadius` sabit:

```csharp
const float linkRadius = 27.5f;
const float linkThickness = 5;
```

`linkPath` Bu tek RADIUS istenen ile temel iki yaylar açıları başlatmak ve sweep açıları şimdi:

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

`outlinePath` Nesnesidir sonra özetini alıcı `linkPath` zaman onu vuruş belirtilen özelliklerle `strokePaint`.

Bu teknik kullanılarak başka bir örnek sonraki kullanılan yolu için yukarı gelen bir `SKPathEffect.Create2DPath` yöntemleri.

## <a name="combining-path-effects"></a>Yol etkilerini birleştirme

İki son statik oluşturma yöntemlerini `SKPathEffect` olan [ `SKPathEffect.CreateSum` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateSum/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/) ve [ `SKPathEffect.CreateCompose` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateCompose/p/SkiaSharp.SKPathEffect/SkiaSharp.SKPathEffect/):

```csharp
public static SKPathEffect CreateSum (SKPathEffect first, SKPathEffect second)

public static SKPathEffect CreateCompose (SKPathEffect outer, SKPathEffect inner)
```

Her iki yöntem bileşik yol efekti oluşturmak için iki yol etkileri birleştirin. `CreateSum` Yöntemi, ayrı ayrı uygulanan iki yol etkilerini benzer bir yol efekti oluşturur sırada `CreateCompose` bir yolu efekti uygular ( `inner`) ve ardından geçerlidir `outer` o.

Gördünüz nasıl `GetFillPath` yöntemi `SKPaint` bir yoluna göre başka bir yol dönüştürebilir `SKPaint` özellikleri (de dahil olmak üzere `PathEffect`) olmaması gerekir böylece *çok* bir nasılgizemli`SKPaint`nesne iki kez belirtilen iki yolu efektleri ile bu işlemi gerçekleştirebilir `CreateSum` veya `CreateCompose` yöntemleri.

Bir açık kullanımını `CreateSum` tanımlamaktır bir `SKPaint` ile tek bir yol etkili bir yol doldurur ve başka bir yolu etkisi yolu konturlar nesnesi. Bu, gösterilmektedir **kediler çerçevesinde** bir çerçevesinde kediler dizisi Fistolu kenarları görüntüler örnek:

[![](effects-images/catsinframe-small.png "Üçlü sayfasının ekran görüntüsü kediler içinde çerçeve")](effects-images/catsinframe-large.png#lightbox "Üçlü sayfasının ekran görüntüsü kediler, çerçeve")

[ `CatsInFramePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CatsInFramePage.cs) Sınıfı başlayan birkaç alanları tanımlayarak. İlk alanından tanıyabilir [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) sınıfıyla [ **SVG yol verileri** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) makalesi. Çizgi ve yay çerçevenin Tarak düzeni için ikinci yol dayanır:

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

`catPath` Kullanılabilen `SKPathEffect.Create2DPath` yöntemi varsa `SKPaint` nesne `Style` özelliği ayarlanmış `Stroke`. Ancak, varsa `catPath` bu programda sonra kat tüm başı doldurulur ve hatta yatay çizgilerinin görünmeyecektir doğrudan kullanılır. (Deneyin!) Bu yol özetini almak ve bu anahattı kullanmak için gerekli olan `SKPathEffect.Create2DPath` yöntemi.

Bu iş oluşturucusu yok. İlk iki dönüşümler geçerlidir `catPath` taşımak için (0, 0) center'ın üzerine gelin ve ölçeklemek boyutu. `GetFillPath` tüm ana hatlarını dağılımlarını edinir `outlinedCatPath`, ve söz konusu nesne kullanılan `SKPathEffect.Create2DPath` çağırın. Ölçeklendirme Etkenler `SKMatrix` değer yatay biraz daha büyük ve çeviri Etkenler kopyalanırken döşeme arasında küçük bir arabellek sağlamaya kat dikey boyutunu biraz türetilmiş empirically tam kat görünür olmasını sağlayın çerçevenin sol üst köşe:

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

Ardından oluşturucuyu çağırır `SKPathEffect.Create1DPath` Fistolu çerçevesi için. Yolun genişliğini 100 pikseldir, ancak çoğaltılmış yolu etrafındaki çerçevenin çakışan 75 piksel öncelikli olduğu dikkat edin. Son Deyim Oluşturucusu çağrılarının `SKPathEffect.CreateSum` iki yol etkilerini birleştirmek ve sonuç kümesi `SKPaint` nesnesi.

Bu iş verir `PaintSurface` oldukça basit olmasını işleyici. Yalnızca bir dikdörtgen tanımlamak ve kullanarak çizmek gereken `framePaint`:

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

Yol etkilerini arkasında algoritmaları her zaman bazı görseller dikdörtgenin dışında görünmesine neden görüntülenecek vuruş yapması veya doldurma için kullanılan dosyanın tam yolunu neden. `ClipRect` Öncesinde çağrı `DrawRect` çağrısı görselleri önemli ölçüde temizleyici olmasını sağlar. (Kırpma olmadan deneyin!)

Kullanmak için ortak olan `SKPathEffect.CreateCompose` bazı değişim başka bir yolu efekti eklemek için. Kesinlikle kendiniz deneyebilirsiniz, ancak biraz farklı bir örnek şudur:

**Kesikli tarama satırları** elips kesik tarama satırlar ile doldurur. Çoğu iş [ `DashedHatchLinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/DashedHatchLinesPage.cs) sınıfı gerçekleştirilir alan tanımlarını sağ. Bu alanların bir tire etkili olur ve bir tarama efekti tanımlayın. Olarak tanımlanan `static` , ardından içinde başvurulduğundan bir `SKPathEffect.CreateCompose` Çağır `SKPaint` tanımı:

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

`PaintSurface` İşleyici yalnızca standart yükünü artı yapılan bir çağrı içeren gerek `DrawOval`:

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

Zaten keşfettiniz gibi tarama satırları alanı iç için tam olarak sınırlı değildir ve bu örnekte, bunlar her zaman sol tam bir tire ile başlar:

[![](effects-images/dashedhatchlines-small.png "Üçlü sayfasının ekran görüntüsü kesik tarama satırları")](effects-images/dashedhatchlines-large.png#lightbox "Üçlü sayfasının ekran görüntüsü tarama satırları kısa çizgili")

Yol etkilerini garip birleşimleri basit nokta ve tire bu aralığa gördünüz, neler yapabileceğinizi görmek ve, hayal kullanın.



## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
