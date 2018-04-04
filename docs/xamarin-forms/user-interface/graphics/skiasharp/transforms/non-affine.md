---
title: Afin olmayan dönüşümler
description: Perspektif ve Konik etkileri dönüştürme matrisi üçüncü sütun ile oluşturma
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 52bed94724d330b74a9604c54fcfebad1e562267
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="non-affine-transforms"></a>Afin olmayan dönüşümler

_Perspektif ve Konik etkileri dönüştürme matrisi üçüncü sütun ile oluşturma_

Çeviri, ölçekleme, döndürme ve eğriltme tüm olarak sınıflandırılan *afin* dönüştürür. Afin dönüşümler paralel çizgi korur. İki satır önce dönüştürme paralel varsa, sonra dönüştürme paralel kalırlar. Dikdörtgenler her zaman için parallelograms dönüştürülür.

Ancak, SkiaSharp de dikdörtgen dışbükey herhangi quadrilateral dönüştürme yeteneği olan afin olmayan dönüşümler yapabilir:

![](non-affine-images/nonaffinetransformexample.png "Dışbükey quadrilateral dönüştürülen bir bit eşlem")

Dışbükey quadrilateral dört taraflı bir şekilde iç açıları her zaman değerinden 180 derece ve birbirlerine arası yok tarafında ile gösterilmiştir.

Üçüncü satır dönüştürme matrisi değerleri 0, 0 ve 1 dışındaki ayarlandığında olmayan afin sonuç dönüştürür. Tam `SKMatrix` çarpma değil:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Sonuç dönüştürme formüller şunlardır:

x' ScaleX·x, SkewX·y + TransX =

y' SkewY·x, ScaleY·y + TransY =

z` = Persp0·x + Persp1·y + Persp2

İki boyutlu dönüşümler için 3 ile 3 matris kullanmanın temel her şeyi üzerinde düzlemi burada Z eşittir 1 kalır kuralıdır. Sürece `Persp0` ve `Persp1` 0, ve `Persp2` eşittir 1, dönüştürme, Düzlemi kapalı Z koordinatları taşınmıştır.

Bu iki boyutlu bir dönüştürme geri yüklemek için koordinatları bu düzlemi geri taşınması gerekir. Başka bir adım gereklidir. X', y', ve z 'değerleri z tarafından ayrılmalıdır':

x" = x' / z'

y" = y' / z'

z" = z' / z' = 1

Bunlar olarak bilinir *homojen koordinatları* ve bunlar matematikçi Ağustos Ferdinand Möbius, çok daha iyi topolojik kendi oddity için bilinen tarafından Möbius Şerit geliştirilmiştir.

Varsa z' sonsuz koordinatları bölme sonuçlarında 0'dır. Aslında, homojen koordinatları geliştirmek için Möbius'ın sözleri birini sonsuz sonlu sayılar değerlerle temsil becerisidir.

Ancak, grafik görüntülerken, sonsuz değerlerine dönüştürün koordinatları ile bir şey işleme önlemek istiyor. Bu koordinatları çizilir olmaz. Bu koordinatları kapsamına her şeyi çok büyük ve büyük olasılıkla görsel olarak tutarlı olur.

Bu denklemi z değerini istemediğiniz ' sıfır olma:

z` = Persp0·x + Persp1·y + Persp2

`Persp2` Hücre sıfır veya sıfır değil ya da olabilir. Varsa `Persp2` sıfır z olduğundan ' sıfır (0, 0) noktası için ve bu genellikle tercih o noktadan iki boyutlu grafik yaygın olduğundan değil. Varsa `Persp2` var. varsa sayılanların genel kaybı olmaksızın sıfır olarak eşit değil `Persp2` 1 sabit. Örneğin, karar verirseniz `Persp2` , yalnızca matris tüm hücreleri 5 tarafından kılan ayırabilirsiniz sonra 5, olmalıdır `Persp2` 1'e eşit ve sonucu aynı olacaktır.

Bu nedenlerle, `Persp2` kimlik Matristeki aynı değer olan 1'den genellikle sabittir.

Genellikle, `Persp0` ve `Persp1` küçük numaralarıdır. Örneğin, bir kimlik matris ancak kümesi başlamadan varsayalım `Persp0` 0,01 için:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Dönüştürme formüller şunlardır:

x' = x / (0.01·x + 1)

y' = y / (0.01·x + 1)

Şimdi bu dönüşüm sırasında kaynak konumlandırılmış 100 piksel kare kutu işlemek için kullanın. İşte dört köşe nasıl dönüştürülür:

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

100 sonra z x olduğunda ' payda olan 2 x ve y koordinatları için etkili bir şekilde yarıya iner. Kutusunun sağ tarafında sol tarafındaki kısa olur:

![](non-affine-images/nonaffinetransform.png "Afin olmayan bir dönüşüm tabi bir kutusu")

`Persp` Bu hücre adları parçası foreshortening kutunun sağ tarafında Görüntüleyici'dan daha ileride olan şimdi eğildiğini çünkü "perspektife" anlamına gelir.

**Test perspektifini** sayfası değerlerle denemeler sağlar `Persp0` ve `Pers1` nasıl çalıştıklarını için bir fikir almak için. Bu matris hücrelerin makul değerlerini kadar küçük, `Slider` Evrensel Windows platformu düzgün bunları işleyemiyor. UWP sorun, iki uyum sağlayacak şekilde `Slider` öğelerinde [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) aralığı 1 -1'den başlatılması gerekir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.TestPerspectivePage"
             Title="Test Perpsective">
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
                    <Setter Property="Minimum" Value="-1" />
                    <Setter Property="Maximum" Value="1" />
                    <Setter Property="Margin" Value="20, 0" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="persp0Slider"
                Grid.Row="0"
                ValueChanged="OnPersp0SliderValueChanged" />

        <Label x:Name="persp0Label"
               Text="Persp0 = 0.0000"
               Grid.Row="1" />

        <Slider x:Name="persp1Slider"
                Grid.Row="2"
                ValueChanged="OnPersp1SliderValueChanged" />

        <Label x:Name="persp1Label"
               Text="Persp1 = 0.0000"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Kaydırıcılar için olay işleyicileri [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) arka plan kod dosyasına –0.01 ve 0,01 aralığı böylece bu değerleri 100 ile ayırın. Ayrıca, bir bitmap Oluşturucusu yükler:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    SKBitmap bitmap;

    public TestPerspectivePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }

    void OnPersp0SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp0Label.Text = String.Format("Persp0 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }

    void OnPersp1SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp1Label.Text = String.Format("Persp1 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }
    ...
}
```

`PaintSurface` İşleyici hesaplar bir `SKMatrix` adlı değeri `perspectiveMatrix` 100'e bölünmüş bu iki kaydırıcılar değerlere göre. Bu ile birleştirilir iki bit eşlem Merkezi'nde bu dönüşüm merkezi put dönüşümler çevir:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Calculate perspective matrix
        SKMatrix perspectiveMatrix = SKMatrix.MakeIdentity();
        perspectiveMatrix.Persp0 = (float)persp0Slider.Value / 100;
        perspectiveMatrix.Persp1 = (float)persp1Slider.Value / 100;

        // Center of screen
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);
        SKMatrix.PostConcat(ref matrix, perspectiveMatrix);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(xCenter, yCenter));

        // Coordinates to center bitmap on canvas
        float x = xCenter - bitmap.Width / 2;
        float y = yCenter - bitmap.Height / 2;

        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

Bazı örnek görüntüleri şunlardır:

[![](non-affine-images/testperspective-small.png "Üçlü sayfasının ekran görüntüsü Test perspektifini")](non-affine-images/testperspective-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Test perspektifi")

Kaydırıcılar ile denerken değerleri 0.0066 ötesinde veya –0.0066 altında görüntünün aniden fractured ve tutarsız hale gelmesine neden bulabilirsiniz. Dönüştürülmekte olan bit eşlem 300 piksel kare olur. Bit eşlem koordinatları –150 150 için aralığı için kendi merkezi göre dönüştürülür. Sözcüğünün z değeri ' olan:

z` = Persp0·x + Persp1·y + 1

Varsa `Persp0` veya `Persp1` 0.0066 büyük ya da –0.0066 sonra her zaman bir z sonuçları bit eşlem bazı koordinatı ' sıfır değeri. Sıfıra bölme neden ve işleme bir karmaşa haline gelir. Afin olmayan dönüşümler kullanırken, sıfıra bölme neden koordinatları ile herhangi bir şey işleme önlemek istiyor.

Genellikle, ayar olmaz `Persp0` ve `Persp1` haklarında. Bu ayrıca genellikle diğer hücrelere belirli türde bir afin olmayan dönüşümler elde etmek için matrisinde ayarlamak gereklidir.

Bu tür afin olmayan bir dönüşüm olan bir *Konik dönüştürme*. Afin olmayan dönüştürme bu tür bir dikdörtgen genel boyutlarını korur ancak bir yan sivrileşen:

![](non-affine-images/tapertransform.png "Konik Dönüştür tabi bir kutusu")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Sınıfı bu parametrelere göre afin olmayan bir dönüşüm genelleştirilmiş bir hesaplama gerçekleştirir:

- dikdörtgen boyutu dönüştürülmekte olan görüntünün
- sivrileşen dikdörtgen tarafında gösteren bir numaralandırma
- nasıl sivrileşen gösteren başka bir numaralandırma ve
- Kayan uzantı.

Kod aşağıdaki gibidir:

```csharp
enum TaperSide { Left, Top, Right, Bottom }

enum TaperCorner { LeftOrTop, RightOrBottom, Both }

static class TaperTransform
{
    public static SKMatrix Make(SKSize size, TaperSide taperSide, TaperCorner taperCorner, float taperFraction)
    {
        SKMatrix matrix = SKMatrix.MakeIdentity();

        switch (taperSide)
        {
            case TaperSide.Left:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp0 = (taperFraction - 1) / size.Width;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Top:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp1 = (taperFraction - 1) / size.Height;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Right:
                matrix.ScaleX = 1 / taperFraction;
                matrix.Persp0 = (1 - taperFraction) / (size.Width * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        break;
                }
                break;

            case TaperSide.Bottom:
                matrix.ScaleY = 1 / taperFraction;
                matrix.Persp1 = (1 - taperFraction) / (size.Height * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        break;
                }
                break;
        }
        return matrix;
    }
}
```

Bu sınıf kullanılır **Konik dönüştürme** sayfası. İki XAML dosyası başlatır `Picker` numaralandırma değerlerini seçmek için öğeleri ve `Slider` Konik kesir seçme. [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) İşleyici birleştirir iki Konik dönüştürme Çevir dönüşümler bit eşlem sol üst köşesindeki göre dönüştürme yapmak için:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedIndex;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedIndex;
    float taperFraction = (float)taperFractionSlider.Value;

    SKMatrix taperMatrix =
        TaperTransform.Make(new SKSize(bitmap.Width, bitmap.Height),
                            taperSide, taperCorner, taperFraction);

    // Display the matrix in the lower-right corner
    SKSize matrixSize = matrixDisplay.Measure(taperMatrix);

    matrixDisplay.Paint(canvas, taperMatrix,
        new SKPoint(info.Width - matrixSize.Width,
                    info.Height - matrixSize.Height));

    // Center bitmap on canvas
    float x = (info.Width - bitmap.Width) / 2;
    float y = (info.Height - bitmap.Height) / 2;

    SKMatrix matrix = SKMatrix.MakeTranslation(-x, -y);
    SKMatrix.PostConcat(ref matrix, taperMatrix);
    SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(x, y));

    canvas.SetMatrix(matrix);
    canvas.DrawBitmap(bitmap, x, y);
}
```

Bazı örnekler şunlardır:

[![](non-affine-images/tapertransform-small.png "Üçlü sayfasının ekran görüntüsü Konik dönüştürme")](non-affine-images/tapertransform-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Konik dönüştürme")

Başka bir genelleştirilmiş afin olmayan dönüşümler sonraki makalede gösterilen, 3B bir döndürme türünde [3B Döndürme](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md).

Afin olmayan dönüşüm dikdörtgen dışbükey herhangi quadrilateral dönüştürebilirsiniz. Bu tarafından gösterilen **olmayan afin Matrisi Göster** sayfası. Çok benzer **afin Matrisi Göster** gelen sayfa [matris dönüşümleri](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md) dördüncü sahiptir ancak bu makale `TouchPoint` bit eşlem dördüncü köşesindeki işlemek için nesnesi:

[![](non-affine-images/shownonaffinematrix-small.png "Üçlü sayfasının ekran görüntüsü olmayan afin Matrisi Göster")](non-affine-images/shownonaffinematrix-large.png#lightbox "Üçlü sayfasının ekran görüntüsü olmayan afin Matrisi Göster")

Bit eşlem köşelerinde birinin iç açı 180 derece büyüktür yaptığınızda, veya birbiriyle arası iki kenara doğrulamaya çalışma sürece, program başarıyla bu yöntemi kullanarak dönüştürme hesaplar [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) sınıfı:

```csharp
static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL, SKPoint ptLR)
{
    // Scale transform
    SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

    // Affine transform
    SKMatrix A = new SKMatrix
    {
        ScaleX = ptUR.X - ptUL.X,
        SkewY = ptUR.Y - ptUL.Y,
        SkewX = ptLL.X - ptUL.X,
        ScaleY = ptLL.Y - ptUL.Y,
        TransX = ptUL.X,
        TransY = ptUL.Y,
        Persp2 = 1
    };

    // Non-Affine transform
    SKMatrix inverseA;
    A.TryInvert(out inverseA);
    SKPoint abPoint = inverseA.MapPoint(ptLR);
    float a = abPoint.X;
    float b = abPoint.Y;

    float scaleX = a / (a + b - 1);
    float scaleY = b / (a + b - 1);

    SKMatrix N = new SKMatrix
    {
        ScaleX = scaleX,
        ScaleY = scaleY,
        Persp0 = scaleX - 1,
        Persp1 = scaleY - 1,
        Persp2 = 1
    };

    // Multiply S * N * A
    SKMatrix result = SKMatrix.MakeIdentity();
    SKMatrix.PostConcat(ref result, S);
    SKMatrix.PostConcat(ref result, N);
    SKMatrix.PostConcat(ref result, A);

    return result;
}
```

Hesaplama kolaylığı için bu yöntem bu dönüşümler bit eşlem dört köşelerinde nasıl değiştirileceği gösteren oklarla burada görüntülenir üç ayrı dönüşümler, bir ürün olarak toplam dönüştürme alır:

(0, 0) → (0, 0) → (0, 0) → (sol üst) (x, 0, y0)

(0, Y) (sol) → (0, 1) → (0, 1) → (x1, y1)

(W, 0) → (1, 0) → (1, 0) → (sağ üst köşede) (x 2, y2)

(G, H) → (1, 1) → (a, b) → (sağ alt köşedeki) (x 3, y3)

Son koordinatları sağdaki dört dokunma noktalarıyla ilişkili dört noktalarıdır. Bit eşlem köşelerinde son koordinatlarını bunlar.

G ve H bit eşlem yüksekliğini ve genişliğini temsil eder. İlk dönüştürme (`S`) yalnızca 1 piksel kare bit eşlem ölçeklendirir. Afin olmayan dönüşüm ikinci dönüşüm olan `N`, ve üçüncü afin dönüşüm `A`. Sahip, olduğu gibi daha önce afin afin dönüşüm üç noktalarında dayalı, bu yüzden [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) yöntemi ve dördüncü satırı içermeyen (a, b) noktası.

`a` Ve `b` değerler hesaplanan üçüncü dönüştürme afin olmasını sağlayın. Kod afin dönüşüm tersini alır ve ardından, sağ alt köşedeki eşlemek için kullanır. (A, b) noktası olmasıdır.

Başka bir afin olmayan dönüşümler üç boyutlu grafik taklit etmek üzere kullanılır. Sonraki makalede [3B Döndürme](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md) iki boyutlu bir grafik 3B uzaydaki döndürmek bkz.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
