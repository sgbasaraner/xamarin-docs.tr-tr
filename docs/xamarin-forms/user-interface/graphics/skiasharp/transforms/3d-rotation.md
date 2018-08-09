---
title: SkiaSharp, 3B Döndürme
description: Bu makalede, ilişkili olmayan dönüşümler 2B nesneleri 3B alanda döndürme için nasıl kullanılacağını açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B5894EA0-C415-41F9-93A4-BBF6EC72AFB9
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 84ebdd007d17eaf0bcfc1be119cb4130299503bc
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615671"
---
# <a name="3d-rotations-in-skiasharp"></a>SkiaSharp, 3B Döndürme

_2B nesneleri 3B alanda döndürme için ilişkili olmayan dönüşümler kullanın._

İlişkili olmayan dönüşümler bir ortak uygulaması, bir 2B nesnenin 3B alanda döndürme benzetimi:

![](3d-rotation-images/3drotationsexample.png "3B alanda döndürülmüş bir metin dizesi")

Üç boyutlu döndürmeler çalışmak ve sonra bir ilişkili olmayan türetme bu işi içerir `SKMatrix` bu 3B Döndürme gerçekleştiren Dönüştür.

Bu geliştirme yapmak zordur `SKMatrix` yalnızca iki boyutta içinde çalışma Dönüştür. Bu 3 x 3 matris 3B grafikler, kullanılan 4 x 4 matris türetilir güncelleştirildiğinde iş çok daha kolay hale gelir. SkiaSharp içerir [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) sınıfı bu amaç, ancak bazı arka planda 3B grafik 3B Döndürme ve 4 x 4 dönüştürme matrisi anlamak için gereklidir.

Üç boyutlu bir koordinat sistemini z kavramsal olarak adlandırılan üçüncü bir eksen ekler, dik ekranına Z ekseni altındadır. 3B alanda koordinat noktaları üç sayılarla gösterilir: (x, y, z). 3D X değerini artırmayı bu makalede kullanılan koordinat sistemi olan sağa ve aşağı olduğu gibi yalnızca iki boyutta Y artan değerleri gidin. Artan pozitif Z değerleri ekran dışında gelir. Kaynak sol üst köşede, 2B grafikler olduğu gibi ' dir. Ekranın bu düzlemine dik açılı, Z ekseni ile bir XY düzlem olarak düşünebilirsiniz.

Bu, sol taraftaki bir koordinat sistemi olarak adlandırılır. İçin sol taraftaki pozitif (sağda) koordinatlarına X yönünde erişebildiğinizden işaret ve (basılı), artan Y yönünde Orta parmağınızı koordinatları, işaret Z koordinatları artırma yönünde işaret — dan çıkış genişletme ekranı.

3B grafikler, bir 4 x 4 matris dönüşümleri temel alır. 4 x 4 kimlik matrisi şu şekildedir:

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

Ancak, SkiaSharp `Matrix44` sınıfı biraz farklıdır. Tek tek hücre değerlerini alma veya tek yolu `SKMatrix44` kullanarak [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) dizin oluşturucu. Satır ve sütun dizinleri sıfır tabanlı yerine bir tabanlı ve satırları ve sütunları değiştirilir. Yukarıdaki diyagramda M14 hücresinde dizin oluşturucu kullanılarak erişilen `[3, 0]` içinde bir `SKMatrix44` nesne.

Bir 3B grafik sistemde tarafından 4 x 4 dönüştürme matrisi çarparak için 1 ile 4 matris için 3B bir nokta (x, y, z) dönüştürülür:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

2B benzer meydana üç farklı boyutta dönüştüren, 3B dönüşümler dört boyutlarındaki gerçekleşmesi için kabul edilir. Dördüncü boyutu olarak adlandırılır ve 3B alanda W koordinatları 1'e eşit olduğu 4 D alanı içinde mevcut varsayılır. Dönüştürme formülleri aşağıdaki gibidir:

x' = M11·x + M21·y + M31·z + M41

y' = M12·x + M22·y + M32·z + M42

z' = M13·x + M23·y + M33·z + M43

w' M14·x M24·y + M34·z + M44 =

Dönüştürme formülden açıktır, hücre `M11`, `M22`, `M33` Etkenler X, Y ve Z yönde ölçeklendirme olan ve `M41`, `M42`, ve `M43` çeviri faktörlerdir X, Y ve Z yönergeleri izleyin.

Bu koordinatları burada W eşittir 1, x geri 3B alanına dönüştürmek için ', y', ve z 'koordinatları tüm w ayrılır':

'x = x' / w'

y"y =' / w'

z"z =' / w'

w"w =' / w' = 1

Bu bölme w' perspektif 3B alanda sağlar. W' 1'e eşittir ve hiçbir perspektif gerçekleşir.

3B alanda döndürme oldukça karmaşık olabilir, ancak basit döndürmenin X, Y ve Z ekseni etrafında olanlardır. Bu matris bir açının α X ekseni etrafında döndürme şöyledir:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

X değerleri aynı olduğunda bu dönüşüm tabi kalır. Y ekseni etrafında döndürme değerlerini değiştirmeden bırakır:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Z ekseni etrafında döndürme 2B grafikler gibi aynıdır:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Döndürme yönünü koordinat sistemini tanımlar tarafından kapsanır. Bu sol bir sistem, bunu belirli bir eksen için değerleri artırma doğrultusunda, sol taraftaki thumb işaret ederseniz — sağındaki X ekseni etrafında döndürme için Z ekseni etrafında döndürme için doğru ve Y ekseni etrafında döndürme için aşağı — eğrisini ardından yo diğer parmaklarınızın döndürme pozitif açı yönünü gösterir.

`SKMatrix44` statik genelleştirilmiş [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) ve [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) ekseni döndürme oluştuğu belirtmenize olanak tanıyan yöntemler:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

X ekseni etrafında döndürme için ilk üç bağımsız değişken için 1, 0, 0 ayarlayın. Y ekseni etrafında döndürme, bunları ayarlamak için 0, 1, 0 ve Z ekseni etrafında döndürme için bunları ayarlamak için 0, 0, 1.

4-tarafından-4 dördüncü sütun için açısıdır. `SKMatrix44` Perspektif dönüşümler oluşturmak için yöntemi yok, ancak bir aşağıdaki kodu kullanarak kendiniz oluşturabilirsiniz:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Bağımsız değişken adı nedeni `depth` kısa bir süre sonra yetkisiz değiştirmeye karşı korumalı hale gelir. Bu kodu matris oluşturur:

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

W aşağıdaki hesaplamaya dönüştürme formülleri neden ':

w' – z = / derinliği + 1

Bu, X ve Y koordinatları Z değerleri olan'den küçük olduğunda sıfır (kavramsal olarak XY düzlemi) azaltmak için hem X ve Y koordinatları Z pozitif değerler için artırmak için kullanılır. Ne zaman Z koordinatı eşittir `depth`, ardından w' sıfır ve koordinatları sonsuz olur. Üç boyutlu grafik sistemleri, bir kamera benzetme geçici olarak oluşturulur ve `depth` Buradaki değer koordinat sistemini başlangıcından kameranın uzaklık temsil eder. Bir grafik nesnesinin diğer bir deyişle koordine Z varsa `depth` birimleri kaynaktan kamera lensi kavramsal olarak oncollisionstay ve sonsuz büyük hale gelir.

Büyük olasılıkla bu kullanacaksınız olduğunu aklınızda bulundurun `perspectiveMatrix` döndürme matrislerde birlikte değeri. Döndürülmüş bir grafik nesnesinin X veya Y koordinatları büyüktür varsa `depth`, sonra da Z koordinatları büyüktür katılımını sağlamak bu nesneyi 3B alanda döndürme olasıdır `depth`. Bu kaçınılması gerekir! Oluştururken `perspectiveMatrix` ayarlamak istediğiniz `depth` grafik nesnesi nasıl döndürüldüğüne ne olursa olsun tüm koordinatlarında için yeterince büyük bir değer. Bu olduğunu hiçbir zaman herhangi bir sıfıra bölünme sağlar.

3B Döndürme ve perspektif birleştirerek 4 x 4 matrislerde birlikte çarparak gerektirir. Bu amaçla `SKMatrix44` birleştirme yöntemleri tanımlar. Varsa `A` ve `B` olan `SKMatrix44` nesneler eşit aşağıdaki kodu ayarlar daha sonra bir × B:

```csharp
A.PostConcat(B);
```

Bir 2B grafik sistemde 4 x 4 dönüştürme matrisi kullanıldığında, bu 2B nesnelere uygulanır. Bu nesneler, düz ve sıfır Z koordinatları varsayılır. Dönüştürme çarpma biraz daha önce gösterilen dönüştürme kolaydır:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

0 değeri üçüncü satırda matris hücreler içermeyen dönüştürme formüllerde z sonuçları için:

x' = M11·x + M21·y + M41

y' = M12·x + M22·y + M42

z' = M13·x + M23·y + M43

w' M14·x + M24·y M44 =

Ayrıca, z' koordinat ilgisiz burada de. Bir 2B grafik sistemde bir 3B nesne görüntülendiğinde, Z koordinatı değerleri yoksayma şeklinde iki boyutlu bir nesne için daraltılmış durumda. Aslında bu iki dönüştürme formüllerdir:

'x = x' / w'

y"y =' / w'

Üçüncü satır buna *ve* 4 x 4 matris üçüncü sütunda yoksayıldı.

Ancak, bunu neden olup olmadığını bile gerekli 4 x 4 matris ilk başta mi?

Üçüncü satır ve 4-tarafından-4'ün üçüncü sütunda, iki boyutlu dönüşümleri, üçüncü satır ve sütun ilgisiz olmasına rağmen *yapmak* zaman önce bir rol oynar çeşitli `SKMatrix44` değerleri birlikte çarpılır. Örneğin, perspektif dönüştürme Y ekseni etrafında döndürme Çarp varsayalım:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

Ürün, hücre `M14` artık bir perspektif değeri içerir. Bu matris 2B nesnelerine uygulamak istiyorsanız, üçüncü satır ve sütun 3-x-3 matris dönüştürmek için ortadan kalkar:

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

Dönüştürme formülleri şunlardır:

x' cos = (α) ·x

y' y =

z' = (sin (α) / derinliği) ·x + 1

Artık her şeyi z tarafından bölmek ':

x"cos = (α) ·x / (((α) sin / derinliği) ·x + 1)

"y = y / (((α) sin / derinliği) ·x + 1)

Bir pozitif açısı Y ekseni etrafında sonra pozitif ile 2B nesneler Döndürülmüş zaman arka planda çalışırken negatif X değerleri recede X değerleri ön plana gelir. X değerleri gibi görünüyor yakın (cosine değeri tarafından yönetilir) Y ekseni için Y ekseni uzağında koordinatları olarak taşımak için daha küçük veya dan Görüntüleyicisi taşınabilecek gibi daha büyük ya da Görüntüleyicisi yakın olur.

Kullanırken `SKMatrix44`, çeşitli çarpılarak tüm 3B Döndürme ve perspektif işlemleri gerçekleştirmek `SKMatrix44` değerleri. 3-x-3 iki boyutlu bir matris 4 tarafından 4 ayıklayabilir sonra matris kullanarak [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) özelliği `SKMatrix44` sınıfı. Bu özellik bir tanıdık döndürür `SKMatrix` değeri.

**3B Döndürme** sayfa ile 3B Döndürme deneme sağlar. [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) dosya dört X, Y ve Z ekseni etrafında döndürme ayarlayın ve derinlik değerini ayarlamak için kaydırıcıları başlatır:

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

Dikkat `depthSlider` ile başlatılmış bir `Minimum` 250 değeri. Bu, burada Döndürülmüş 2B nesnenin kaynağı etrafında bir 250 piksel RADIUS tarafından tanımlanan bir daire kısıtlı, X ve Y koordinatları olduğunu gösterir. Tüm bu nesneyi 3B alanda döndürme her zaman 250'den daha az koordinat değerleri neden olur.

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) arka plan kod dosyası, 300 piksel kare şeklinde bir bit eşlem yükler:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

3B dönüştürme bu bit eşlem ortalanır, köşeleri 250 piksel RADIUS her şeyi, bu nedenle Merkezi'nden 212 piksel çalışırken ardından X ve Y –150 150 arasındaki aralığı düzenler.

`PaintSurface` İşleyicisi oluşturur `SKMatrix44` nesneler üzerinde kaydırıcıları tabanlı ve birlikte kullanarak bunları çarpar `PostConcat`. `SKMatrix` Son ayıklanan değer `SKMatrix44` nesne çevrelenir tarafından merkezinde döndürme ekranın Orta dönüşümler çevir:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

Dördüncü kaydırıcı ile denemeler yapın, farklı derinliği ayarları Nesne Görüntüleyicisi uzağa daha fazla taşıma, ancak bunun yerine perspektif etkisini kapsamını alter fark edeceksiniz:

[![](3d-rotation-images/rotation3d-small.png "Üçlü sayfasının ekran görüntüsü döndürme 3B")](3d-rotation-images/rotation3d-large.png#lightbox "3B Döndürme sayfanın üç ekran görüntüsü")

**3B döndürmeye animasyon** de kullanır `SKMatrix44` 3B alanda bir metin dizesi animasyon uygulamak için. `textPaint` Kümesi nesnesi olarak alan oluşturucu içinde metnin sınırları belirlemek için kullanılır:

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

`OnAppearing` Geçersiz kılma tanımlar üç Xamarin.Forms `Animation` nesnelere animasyon ekleme `xRotationDegrees`, `yRotationDegrees`, ve `zRotationDegrees` farklı hızlarda alanları. Bu animasyonları dönemlerini numaraları hazırlamak ayarlanır: 5 saniye, 7 saniye, 11 saniye — genel birleşimi yalnızca her 385 saniye veya 10 dakikadan fazla yinelenecek şekilde:

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

Önceki program olduğu gibi `PaintCanvas` işleyicisi oluşturur `SKMatrix44` rotasyonu ve perspektifi için değerleri ve birlikte çarpar:

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

Döndürme merkezini ekranın merkezine taşımanızı ve ekran aynı genişlikte olmasını metin dizesi boyutunu ölçeklendirmek için çeşitli 2B dönüşüm ile bu 3B Döndürme arasına:

[![](3d-rotation-images/animatedrotation3d-small.png "Üçlü sayfasının ekran görüntüsü animasyonlu döndürme 3B")](3d-rotation-images/animatedrotation3d-large.png#lightbox "3B döndürmeye animasyon sayfanın üç ekran görüntüsü")


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
