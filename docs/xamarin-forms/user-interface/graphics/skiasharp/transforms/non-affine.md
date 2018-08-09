---
title: İlişkili olmayan dönüşümler
description: Bu makalede Perspektif ve Konik etkileri dönüştürme matris üçüncü sütun oluşturma açıklanır ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 13f2a1160d012a6b7720bd84340a1cdd0f991535
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615658"
---
# <a name="non-affine-transforms"></a>İlişkili olmayan dönüşümler

_Perspektif ve Konik etkileri dönüştürme matris üçüncü sütun oluşturma_

Çeviri, ölçeklendirme, döndürme ve eğriltme tüm olarak sınıflandırılan *afin* dönüştürür. Afin dönüşümler paralel satırları koru. İki satırdan önce dönüştürme paralel varsa, bunlar sonra dönüşüm paralel kalır. Dikdörtgenler parallelograms için her zaman dönüştürülür.

Ancak, SkiaSharp ayrıca ulaşabileceği herhangi dışbükey quadrilateral bir dikdörtgen dönüştürme yeteneği olan ilişkili olmayan dönüşümler verilmiştir:

![](non-affine-images/nonaffinetransformexample.png "Bir bit eşlem bir dışbükey quadrilateral dönüştürdü")

Dışbükey quadrilateral dört taraflı bir şekil ile her zaman değerinden 180 derece ve birbirlerine çapraz yoksa yüz iç açıları ' dir.

Dönüştürme matrisi üçüncü satır 0, 0 ve 1 dışındaki değerlere ayarlandığında olmayan afin sonucu dönüştürür. Tam `SKMatrix` çarpma olan:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Sonuç dönüştürme formülleri şunlardır:

x' ScaleX·x + SkewX·y TransX =

y' SkewY·x + ScaleY·y TransY =

z' Persp0·x + Persp1·y Persp2 =

3-x-3 matris için iki boyutlu dönüşümler kullanma, temel her şeyi burada Z eşittir 1 gezmeyi kalır kuralıdır. Sürece `Persp0` ve `Persp1` , 0 ve `Persp2` eşittir 1, dönüştürme, Düzlemi kapalı Z koordinatları taşındı.

Bu iki boyutlu bir dönüştürme için geri yüklemek için koordinatları geri bu düzlemine taşınmalıdır. Başka bir adım gereklidir. X', y', ve z 'değerleri tarafından z ayrılmalıdır':

'x = x' / z'

y"y =' / z'

z" = z' / z' = 1

Bunlar olarak bilinen *homojen koordinatları* ve bunlar tarafından matematikçi Ağustos Ferdinand Möbius, çok daha iyi kendi topolojik oddity için bilinen Möbius Şerit geliştirilmiştir.

Z' sonsuz koordinatlarına göre bölme sonuçları 0'dır. Aslında, homojen koordinatları geliştirmek için Möbius'ın motivasyonlardan biri ile sonlu sayılar sonsuz değerlerini temsil edecek şekilde becerisidir.

Ancak, grafik görüntülerken, sonsuz değerlerine dönüştürün koordinatları ile bir şey işleme önlemek istersiniz. Bu koordinatları işlenmez. Bu koordinatları kapsamına her şey çok büyük ve görsel olarak tutarlı olmayabilir.

Bu denklemde z değeri istemediğiniz ' sıfır haline:

z' Persp0·x + Persp1·y Persp2 =

`Persp2` Hücre sıfır veya sıfır değil ya da olabilir. Varsa `Persp2` sıfır sonra z' sıfır (0, 0) noktasının ve olmayan genellikle tercih o noktadan iki boyutlu grafik yaygın olduğu için. Varsa `Persp2` var. varsa genellik kaybı olmadan sıfır olarak eşit değil `Persp2` 1 sabittir. Örneğin, karar verirseniz `Persp2` yalnızca matris tüm hücreleri 5 tarafından getiren bölebilirsiniz sonra 5 olmalıdır `Persp2` 1'e eşit ve sonuç aynı olacaktır.

Bu nedenlerle, `Persp2` kimlik matrisi aynı değeri olan 1'den sık sabittir.

Genellikle, `Persp0` ve `Persp1` küçük sayılardır. Örneğin, bir kimlik matrisi ancak kümesi ile başlayan varsayalım `Persp0` 0,01 için:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Dönüştürme formülleri şunlardır:

x' = x / (0.01·x + 1)

y' y = / (0.01·x + 1)

Artık kaynak konumlandırılmış 100 piksel bir kare kutu işlemek için bu dönüştürme kullanın. İşte farkına nasıl dönüştürülür:

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

100, sonra da z x olduğunda ' ise, 2 x ve y koordinatları için etkili bir şekilde yarıya iner. Kutunun sağ tarafında sol tarafında kısa olur:

![](non-affine-images/nonaffinetransform.png "İlişkili olmayan bir dönüşüm tabi bir kutu")

`Persp` Bu hücre adları parçası foreshortening Görüntüleyicisi dan sağ tarafı kutusu artık eğimlendirildiğinde çünkü "perspektifi için" ifade eder.

**Test perspektifini** sayfası verir değerleriyle denemenizi `Persp0` ve `Pers1` nasıl çalıştıkları için bir genel görünüm alınamıyor. Bu matris hücre makul değerler: küçük, `Slider` Evrensel Windows platformu bunları düzgün bir şekilde işleyemez. UWP sorunu iki uyum sağlayacak şekilde `Slider` öğelerinde [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) aralığı 1-1 başlatılması gerekir:

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

Kaydırıcılar için olay işleyicileri [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) arka plan kod dosyası 0,01 –0.01 arasındaki aralığı, bu değerleri 100 ile bölün. Ayrıca, bir bit eşlem içinde Oluşturucu yükler:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

`PaintSurface` İşleyici hesaplar bir `SKMatrix` değeri `perspectiveMatrix` 100 ile bölünen bu iki kaydırıcıları değerlere göre. İle birlikte bu iki bit eşlem ortasında bu dönüşüm ortasına yerleştirin dönüşümler çevir:

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

Bazı örnek görüntüleri aşağıda verilmiştir:

[![](non-affine-images/testperspective-small.png "Üçlü sayfasının ekran görüntüsü Test perspektifini")](non-affine-images/testperspective-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Test perspektifi")

Kaydırıcıları ile denerken 0.0066 ötesinde veya –0.0066 aşağıdaki değerleri resmi aniden fractured ve tutarsız duruma neden bulabilirsiniz. Dönüştürülmekte olan bit eşlem 300 piksel kare ' dir. Bit eşlem koordinatlarını –150 150 için aralığı. Bu nedenle, kendi merkezi göre dönüştürülür. Bu geri çağırma z değeri ' olan:

z' Persp0·x Persp1·y + 1 =

Varsa `Persp0` veya `Persp1` 0.0066 büyükse veya –0.0066 sonra her zaman bir z sonuçları bit eşlem bazı koordinatını ' sıfır değeri. Sıfıra bölme neden ve işleme zorundaydık olur. İlişkili olmayan dönüşümler kullanırken, sıfıra bölme neden koordinatları ile her şeyi işleme engellemek istiyorsunuz.

Genellikle, ayar gerekmez `Persp0` ve `Persp1` yalıtım halinde. Bu ayrıca genellikle matriste ilişkili olmayan dönüşümler belirli türlerdeki elde etmek için diğer hücreleri ayarlamak gereklidir.

Bu tür ilişkili olmayan bir dönüşüm olduğu bir *Konik dönüştürme*. Bu tür bir ilişkili olmayan bir dönüşüm genel boyutlarını dikdörtgeninin korur, ancak bir tarafı sivrileşen:

![](non-affine-images/tapertransform.png "Bir kutu tabi Konik Dönüştür")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Sınıfı bu parametrelere göre ilişkili olmayan bir dönüşüm genelleştirilmiş bir hesaplama gerçekleştirir:

- dönüştürülmekte olan görüntünün dikdörtgen boyutuna,
- yan sivrileşen dikdörtgenin gösteren numaralandırmaya
- nasıl sivrileşen gösteren başka bir sabit listesi ve
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

Bu sınıf kullanılan **Konik dönüştürme** sayfası. İki XAML dosyası başlatır `Picker` numaralandırma değerlerinin seçilecek öğeleri ve `Slider` Konik kesir seçme. [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) İşleyici birleştirir Konik dönüşüm ile iki bit eşlem sol üst köşesine göre dönüştürme yapmak için dönüşümleri çevir:

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

Sonraki makalede gösterilmiştir, 3B Döndürme genelleştirilmiş ilişkili olmayan dönüşümler başka bir türünde [3B Döndürme](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md).

İlişkili olmayan dönüştürme herhangi dışbükey quadrilateral bir dikdörtgen dönüştürebilirsiniz. Bu tarafından gösterilmiştir **olmayan afin Matrisi Göster** sayfası. Çok benzer **afin Matrisi Göster** gelen sayfasında [matris dönüşümleri](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md) dördüncü sahip dışında makale `TouchPoint` bit eşlemin dördüncü köşe işlemek için nesne:

[![](non-affine-images/shownonaffinematrix-small.png "Üçlü sayfasının ekran görüntüsü olmayan afin Matrisi Göster")](non-affine-images/shownonaffinematrix-large.png#lightbox "Üçlü sayfasının ekran görüntüsü olmayan afin Matrisi Göster")

Bir iç açısını bit eşlemin köşelerden birini 180 derece büyüktür olun veya birbirlerine çapraz iki kenara çalışmayın sürece, bu yöntemi kullanarak dönüştürme program başarıyla hesaplar. [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) sınıfı:

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

Hesaplama kolaylığı için bu yöntem bir ürünü dönüşümleri dört bir yanından bit eşlemin nasıl değiştirileceği gösteren ok ile burada görüntülenir üç ayrı dönüşümleri toplam dönüşümü alır:

(0, 0) → (0, 0) → (0, 0) → (x, 0, y0) (sol)

(0, H) (sol alt) → (0, 1) → (0, 1) → (x1, y1)

(W, 0) (2, y2) x (1, 0) → → (1, 0) → (sağ üst köşede)

(G, H) (1, 1) → → (a, b) → (sağ alt köşedeki) (x, 3, y3)

Son koordinatları sağdaki dört dokunma ile ilişkili dört noktalarıdır. Bit eşlem köşelerini son koordinatlarını şunlardır.

G ve Y, genişlik ve yükseklik bit eşlemin temsil eder. İlk dönüşüm (`S`) yalnızca 1 piksel kare bit eşlem ölçeklendirir. İkinci dönüşüm ilişkili olmayan bir dönüşüm olduğu `N`, ve üçüncü afin dönüşüm `A`. Afin dönüşüm üç noktalarında dayalı olan, olduğu gibi yüzden daha önce afin [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) yöntemi ve dördüncü satırı içermeyen (a, b) noktası.

`a` Ve `b` değerleri üçüncü dönüştürme afin böylece hesaplanır. Kod afin dönüşüm tersini alır ve ardından, sağ alt köşedeki eşlemek için kullanır. (A, b) noktası olmasıdır.

Başka bir ilişkili olmayan dönüşümler üç boyutlu grafik taklit etmek için kullanılır. Sonraki makalede, [3B Döndürme](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md) iki boyutlu grafik 3B alanda döndürme öğrenin.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
