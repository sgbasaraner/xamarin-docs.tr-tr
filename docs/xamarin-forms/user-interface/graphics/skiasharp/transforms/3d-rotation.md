---
title: SkiaSharp içinde 3B Döndürme
description: Bu makalede afin olmayan dönüşümler 3B uzaydaki 2B nesneleri döndürmek için nasıl kullanılacağını açıklar ve bu örnek kodu ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: ad4bce6eff7df65185fc3bd754c747fd0db0c9f1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244305"
---
# <a name="3d-rotations-in-skiasharp"></a>SkiaSharp içinde 3B Döndürme

_Afin olmayan dönüşümler 2B nesnelere 3D alanda döndürmek için kullanın._

Afin olmayan dönüşümler, bir ortak uygulama 3B uzaydaki 2B nesne dönüşünü benzetimi:

![](3d-rotation-images/3drotationsexample.png "3B alanında döndürülmüş bir metin dizesi")

Bu iş üç boyutlu döndürmeler çalışma ve bir harici-afin türetme içerir `SKMatrix` bu 3B Döndürme gerçekleştirir Dönüştür.

Bu geliştirmek sabit `SKMatrix` yalnızca iki boyut içinde çalışma Dönüştür. Bu 3 x 3 matris 3B grafik kullanılan 4 x 4 matris türetilir güncelleştirildiğinde iş çok daha kolay hale gelir. SkiaSharp içeren [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) sınıfı bu amaç, ancak bazı arka planda 3B grafik 3B Döndürme ve 4 x 4 dönüştürme matrisi anlamak için gereklidir.

Üç boyutlu bir koordinat sistemi kavramsal z adlı üçüncü bir eksen ekler, dik ekranına Z ekseni altındadır. 3B uzaydaki koordinat noktaları üç numaralarıyla belirtilir: (x, y, z). 3B koordinat sistemi X değerleri artırma bu makalede kullanılan sağa doğru olan ve artan Y değerini aşağı, yalnızca iki boyut olduğu gibi gidin. Artan pozitif Z değerleri dışında ekrana gelir. Kaynak 2B grafik olduğu gibi yalnızca sol üst köşesindeki ' dir. Ekranın XY düzlemi dik bu düzlemi için adresindeki Z ekseni ile olarak düşünebilirsiniz.

Bu bir sol koordinat sistemi çağrılır. Pozitif koordinatları (sağında) X yönünde, sol taraftaki erişebildiğinizden noktası ve (aşağı), artan Y yönünde, Orta parmak koordinatları, Flash, Z koordinatları artan yönde işaret — gelen çıkışı genişletme Ekran.

3B grafik dönüşümler 4 x 4 matris üzerinde temel alır. 4 x 4 kimlik matris şöyledir:

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

4 x 4 matris ile çalışırken, hücre, satır ve sütun sayıları ile tanımlamak kullanışlıdır:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Ancak, SkiaSharp `Matrix44` sınıfı biraz farklıdır. Ayarlamak veya tek tek hücre değerlerini almak için tek yolu `SKMatrix44` kullanarak [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) dizin oluşturucu. Sıfır tabanlı yerine tabanlı satır ve sütun dizinleri ve satır ve sütunları takas. Yukarıdaki diyagramda M14 hücre dizin oluşturucu kullanılarak erişilir `[3, 0]` içinde bir `SKMatrix44` nesnesi.

Bir 3B grafik sisteminde tarafından 4 x 4 dönüştürme matrisi çarparak için 1 ile 4 bir matris için 3B bir nokta (x, y, z) dönüştürülür:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

2B benzer meydana üç boyutta dönüşümleri, 3B dönüşümler dört boyutlarındaki gerçekleşmesi varsayılır. Dördüncü boyutun W adlandırılır ve 3B uzaydaki W koordinatları 1'e eşit olduğu 4 D alanı içinde yok varsayılır. Dönüştürme formüller aşağıdaki gibidir:

x' = M11·x + M21·y + M31·z + M41

y' = M12·x + M22·y + M32·z + M42

z' = M13·x + M23·y + M33·z + M43

w' M14·x + M24·y + M34·z + M44 =

Dönüştürme formüller açıktır, hücreleri `M11`, `M22`, `M33` Etkenler X, Y ve Z yönde ölçekleme ve `M41`, `M42`, ve `M43` çeviri faktör X, Y ve Z yönergeleri.

Burada W eşittir 1, x geri 3B uzaydaki için bu koordinatları dönüştürmek için ', y', ve z 'koordinatları tüm w ayrılır':

x"= x' / w'

y"y =' / w'

z"z =' / w'

w"w =' / w' = 1

Bu bölme w' perspektif 3B uzaydaki sağlar. Varsa w' 1'e eşittir ve hiçbir perspektif oluşur.

3B uzaydaki döndürmeleri oldukça karmaşık olabilir, ancak basit döndürmeleri X, Y ve Z eksenleri etrafında dosyalardır. Bu matris açının α X ekseni etrafında döndürme şöyledir:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

X değerleri bu dönüştürme için tabi olduğunda'de aynı kalır. Y ekseni etrafında döndürme değerleri y değişmeden kalır:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Z ekseni etrafında döndürme 2B grafik olduğu gibi aynıdır:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Döndürme yönünü koordinat sistemini el kullanımı tarafından kapsanır. Sol sistem budur bunu belirli bir eksen için değerleri artırma doğrultusunda, sol kaydırma noktası — sağındaki X ekseni etrafında döndürme için aşağı dönüş Y ekseni etrafında ve sizin doğru Z ekseni etrafında döndürme için — eğrisini sonra yo diğer parmakları pozitif açıları dönüş yönünü belirtir.

`SKMatrix44` statik genelleştirilmiş [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) ve [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) döndürme oluştuğu eksen belirtmenize olanak veren yöntemleri:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

X ekseni etrafında döndürme için ilk üç bağımsız değişken için 1, 0, 0 ayarlayın. Y ekseni etrafında döndürme, bunları ayarlamak için 0, 1, 0 ve Z ekseni etrafında döndürme için bunları ayarlamak için 0, 0, 1.

Dördüncü 4 tarafından 4'ün perspektif için bir sütundur. `SKMatrix44` Perspektif dönüşümler oluşturmak için hiçbir yöntemlerine sahiptir, ancak kendiniz aşağıdaki kodu kullanarak oluşturabilirsiniz:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Bağımsız değişken adı nedeni `depth` kısa bir süre açık olacaktır. Bu kod, matris oluşturur:

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Dönüştürme formüller w aşağıdaki hesaplamaya neden ':

w' – z = / derinliği + 1

Bu z değerleri olan'den küçük olduğunda (kavramsal XY düzlemi) sıfır X ve Y koordinatları azaltmak ve z pozitif değerler için X ve Y koordinatları artırmak için hizmet eder. Ne zaman Z koordinatı eşittir `depth`, ardından w' sıfır değilse ve koordinatları sonsuz olur. Üç boyutlu grafik sistemleri kamera benzetimini yerleşiktir ve `depth` buraya değer koordinat sistemi kaynaktan kamera uzaklığını temsil eder. Bir grafik nesnesi başka bir deyişle koordine Z olup olmadığını `depth` birimleri kaynaktan kamera Mercek kavramsal dokunma ve sonsuz büyük olur.

Büyük olasılıkla bu kullanmaya başlayacağınız olduğunu aklınızda bulundurun `perspectiveMatrix` döndürme matrisleri ile birlikte değeri. Döndürülmüş bir grafik nesnesi X veya Y koordinatları büyük varsa `depth`, bu nesnenin 3B uzaydaki döndürme Z koordinatları büyük kapsayacak şekilde olasıdır sonra `depth`. Bu kaçınılması gerekir! Oluştururken `perspectiveMatrix` ayarlamak istediğiniz `depth` tüm koordinatlarında nasıl döndürüldüğüne olsun grafik nesnesi için yeterince büyük bir değer. Bu olduğunu asla herhangi sıfıra sağlar.

3B Döndürme ve perspektif birleştirme 4 x 4 matrisleri birlikte çarparak gerektirir. Bu amaçla `SKMatrix44` birleştirme yöntemleri tanımlar. Varsa `A` ve `B` olan `SKMatrix44` nesneleri aşağıdaki kod bir eşit ayarlar daha sonra bir × B:

```csharp
A.PostConcat(B);
```

4 x 4 dönüştürme matrisi bir 2B grafik sisteminde kullanıldığında, 2B nesnelere uygulanır. Bu nesneler düz ve sıfır Z koordinatlara sahip kabul edilir. Dönüştürme çarpma biraz daha önce gösterilen dönüştürme basittir:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

0 değeri matris üçüncü satırında hücreler içermeyen dönüştürme formüllerde z sonuçlar için:

x' = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

z' = M13·x + M23·y + M43

w' M14·x, M24·y + M44 =

Ayrıca, z' koordinat ilgisiz burada da. 3B bir nesneyi bir 2B grafik sisteminde görüntülendiğinde, iki boyutlu bir nesneye Z koordinat değerleri yoksayılıyor tarafından daraltılmıştır. Dönüştürme formüller aslında bu iki şunlardır:

x"= x' / w'

y"y =' / w'

Üçüncü satır buna *ve* 4 x 4 matrisi üçüncü sütun yoksayıldı.

Ancak, bunu neden olup olmadığını ilk başta 4 x 4 matris bile gerekli mi?

Üçüncü satır ve 4 tarafından 4'ün üçüncü sütun iki boyutlu dönüşümler, üçüncü satır ve sütun için ilgisiz olsa *yapmak* zaman önce bir rol oynar çeşitli `SKMatrix44` değerleri birlikte çarpılır. Örneğin, perspektif dönüştürme ile Y ekseni etrafında döndürme Çarp varsayın:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

Hücre üründeki `M14` şimdi bir perspektif değer içeriyor. Bu matris 2B nesnelerine uygulamak istiyorsanız, üçüncü satır ve sütun 3 x 3 matris dönüştürmek için ortadan kaldırılır:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Artık bir 2B noktası dönüştürmek için kullanılabilir:

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Dönüştürme formüller şunlardır:

x' cos = (α) ·x

y' y =

z' = (sin (α) / derinliği) ·x + 1

Artık her şey tarafından z bölmek ':

x"cos = (α) ·x / ((sin (α) / derinliği) ·x + 1)

y"y = / ((sin (α) / derinliği) ·x + 1)

2B nesneleri pozitif bir açı Y ekseni etrafında sonra pozitif ile döndürülüp zaman X değerleri negatif arka recede X değerleri ön plana gelir. X değerleri yakın (cosine değeri tarafından yönetilir) Y ekseni Y ekseni uzağında koordinatları olarak taşımak için daha küçük veya viewer'dan daha ileride taşırken daha büyük ya da Görüntüleyicisi daha yakın olur gibi görünüyor.

Kullanırken `SKMatrix44`, çeşitli çarparak tüm 3B Döndürme ve perspektif işlemleri gerçekleştirmek `SKMatrix44` değerleri. İki boyutlu bir 3 x 3 matris 4 tarafından 4 ayıklayabilirsiniz sonra matrisi kullanarak [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) özelliği `SKMatrix44` sınıfı. Bu özellik bir tanıdık döndürür `SKMatrix` değeri.

**Döndürme 3B** sayfa ile 3B Döndürme denemeler sağlar. [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) dosya başlatır dört kaydırıcılar X, Y ve Z eksenleri etrafında döndürme ayarlamak ve derinliği değeri ayarlamak için:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.Rotation3DPage"
             Title="Rotation 3D">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
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
                    <Setter Property="Maximum" Value="360" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="xRotateSlider"
                Grid.Row="0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference xRotateSlider},
                              Path=Value,
                              StringFormat='X-Axis Rotation = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="yRotateSlider"
                Grid.Row="2"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference yRotateSlider},
                              Path=Value,
                              StringFormat='Y-Axis Rotation = {0:F0}'}"
               Grid.Row="3" />

        <Slider x:Name="zRotateSlider"
                Grid.Row="4"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zRotateSlider},
                              Path=Value,
                              StringFormat='Z-Axis Rotation = {0:F0}'}"
               Grid.Row="5" />

        <Slider x:Name="depthSlider"
                Grid.Row="6"
                Maximum="2500"
                Minimum="250"
                ValueChanged="OnSliderValueChanged" />

        <Label Grid.Row="7"
               Text="{Binding Source={x:Reference depthSlider},
                              Path=Value,
                              StringFormat='Depth = {0:F0}'}" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="8"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Dikkat `depthSlider` ile başlatılmış bir `Minimum` 250 değeri. Bu, burada Döndürülmüş 2B nesne X ve Y koordinatları 250 piksel RADIUS başlangıcı etrafında tarafından tanımlanan bir daire sınırlı olduğunu gösterir. Bu nesnenin 3B uzaydaki herhangi bir döndürme her zaman 250'den koordinat değerleri neden olur.

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) arka plan kodu dosya 300 piksel kare şeklinde bir bit eşlem yükler:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
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

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

3B dönüştürme bu bit eşlem'i ortalanmış durumda ise, her şeyi 250 piksel RADIUS içinde olacak şekilde köşeleri Merkezi'nden 212 piksel durumdayken sonra X ve Y –150 ve 150 aralığında düzenler.

`PaintSurface` İşleyicisi oluşturur `SKMatrix44` nesneler üzerinde kaydırıcılar dayalı ve birlikte kullanarak bunları çarpar `PostConcat`. `SKMatrix` En son ayıklanan değer `SKMatrix44` nesne çevrelenen tarafından Merkezi'ndeki döndürme ekranın merkezi dönüşümler çevir:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
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

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
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

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Use 3D matrix for 3D rotations and perspective
        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, (float)xRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, (float)yRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, (float)zRotateSlider.Value));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / (float)depthSlider.Value;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the bitmap
        canvas.SetMatrix(matrix);
        float xBitmap = xCenter - bitmap.Width / 2;
        float yBitmap = yCenter - bitmap.Height / 2;
        canvas.DrawBitmap(bitmap, xBitmap, yBitmap);
    }
}
```

Dördüncü kaydırıcıyı denerken, farklı derinliği ayarları Nesne Görüntüleyicisi çıktığınızda daha fazla taşımayın, ancak bunun yerine perspektif etkisi kapsamını alter görürsünüz:

[![](3d-rotation-images/rotation3d-small.png "Üçlü sayfasının ekran görüntüsü döndürme 3B")](3d-rotation-images/rotation3d-large.png#lightbox "Üçlü sayfasının ekran görüntüsü döndürme 3B")

**Animasyonlu döndürme 3B** de kullanır `SKMatrix44` bir metin dizesi 3B uzaydaki animasyon uygulamak için. `textPaint` Nesne metnin sınırları belirlemek için kullanılan bir alan oluşturucuda gibi ayarlayın:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    SKCanvasView canvasView;
    float xRotationDegrees, yRotationDegrees, zRotationDegrees;
    string text = "SkiaSharp";
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        TextSize = 100,
        StrokeWidth = 3,
    };
    SKRect textBounds;

    public AnimatedRotation3DPage()
    {
        Title = "Animated Rotation 3D";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Measure the text
        textPaint.MeasureText(text, ref textBounds);
    }
    ...
}
```

`OnAppearing` Geçersiz kılma tanımlayan üç Xamarin.Forms `Animation` hale getirmeyi nesnelere `xRotationDegrees`, `yRotationDegrees`, ve `zRotationDegrees` farklı hızlarda alanları. Animasyonlarına dönemlerini numaraları hazırlamak için ayarlanır — 7 saniye ve 11 saniye 5 saniye — her 385 saniye ya da 10 dakikadan daha genel birleşimi yalnızca yinelenecek şekilde:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();

        new Animation((value) => xRotationDegrees = 360 * (float)value).
            Commit(this, "xRotationAnimation", length: 5000, repeat: () => true);

        new Animation((value) => yRotationDegrees = 360 * (float)value).
            Commit(this, "yRotationAnimation", length: 7000, repeat: () => true);

        new Animation((value) =>
        {
            zRotationDegrees = 360 * (float)value;
            canvasView.InvalidateSurface();
        }).Commit(this, "zRotationAnimation", length: 11000, repeat: () => true);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        this.AbortAnimation("xRotationAnimation");
        this.AbortAnimation("yRotationAnimation");
        this.AbortAnimation("zRotationAnimation");
    }
    ...
}
```

Önceki programın olduğu gibi `PaintCanvas` işleyicisi oluşturur `SKMatrix44` değerler döndürme ve perspektif ve birlikte çarpan:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Scale so text fits
        float scale = Math.Min(info.Width / textBounds.Width,
                               info.Height / textBounds.Height);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(scale, scale));

        // Calculate composite 3D transforms
        float depth = 0.75f * scale * textBounds.Width;

        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, xRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, yRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, zRotationDegrees));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / depth;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the text
        canvas.SetMatrix(matrix);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;
        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Bu 3B Döndürme Dönüş merkezini ekranın merkezine taşımanızı ve ekran aynı genişliğe olmasını metin dizesi boyutunu ölçeklendirmek için birçok 2B dönüşümler ile çevrelenmiş:

[![](3d-rotation-images/animatedrotation3d-small.png "Üçlü sayfasının ekran görüntüsü animasyonlu döndürme 3B")](3d-rotation-images/animatedrotation3d-large.png#lightbox "Üçlü sayfasının ekran görüntüsü animasyonlu döndürme 3B")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
