---
title: Matris dönüşümleri
description: Çok yönlü dönüştürme matrisi ile SkiaSharp dönüşümler içine daha derin Dalış
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 87bddc8d541167cef350658ac69f8aaac6d6a2ee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="matrix-transforms"></a>Matris dönüşümleri

_Çok yönlü dönüştürme matrisi ile SkiaSharp dönüşümler içine daha derin Dalış_

Uygulanan Dönüşümlerin `SKCanvas` nesnesi tek bir örneği birleştirilmiş [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) yapısı. Tüm modern 2B grafik sistemlere benzer bir standart 3 x 3 dönüştürme matrisi budur.

Gördüğünüz gibi Dönüşümlerin içinde SkiaSharp hakkında dönüştürme matrisi, ancak dönüştürme matrisi teorik açısından önemli ve dönüşümler yolları değiştirmek için kullanırken önemlidir bilerek olmadan veya her ikisini de karmaşık dokunma girişini işleme için kullanabilirsiniz Bu makalede ve sonraki gösterilmiştir.

![](matrix-images/matrixtransformexample.png "Afin Dönüştür tabi bir bit eşlem")

Uygulanan geçerli dönüştürme matrisi `SKCanvas` salt okunur erişerek herhangi bir zamanda kullanılabilir [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) özelliği. Yeni bir dönüştürme matrisi kullanarak ayarlayabilirsiniz [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) yöntemi ve geri yükleyebilir, bu dönüştürme matrisi varsayılan değerlere çağırarak [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/).

Yalnızca diğer `SKCanvas` doğrudan tuval matris dönüştürme ile çalışır üye [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) , art arda ekler iki matrisi birlikte çarparak.

Varsayılan dönüştürme matrisi kimlik matrisini verir ve 1 lerin çapraz hücreler ve 0'ların her yerde başka oluşur:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Statik kullanarak bir kimlik matris oluşturabilirsiniz [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) yöntemi:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix` Varsayılan oluşturucu mu *değil* kimlik matris döndürür. Tüm sıfır olarak ayarlanmış hücreler içeren bir matris döndürür. Kullanmayın `SKMatrix` Oluşturucusu hücrelere el ile ayarlamak planlamıyorsanız.

Bir grafik nesnesi SkiaSharp işler, her nokta (x, y) etkili bir şekilde üçüncü sütun 1 ile 1 ile 3 matris dönüştürülür:

<pre>
| x  y  1 |
</pre>

Bu 1 ile 3 matris 1 olarak ayarlayın Z koordinatı ile üç boyutlu noktasını temsil eder. Üç boyutlarında çalışma iki boyutlu matris dönüşüm gerektiriyor neden (daha sonra ele) matematiksel nedenleri vardır. Burada Z eşittir 1 temsil eden bir 3B koordinat sistemi, ancak her zaman 2B düzlemi üzerinde bir nokta olarak bu 1 ile 3 matrisi düşünebilirsiniz.

Bu 1 ile 3 matris sonra tarafından dönüştürme matrisi çarpılması ve sonucu tuvalde çizilir noktasıdır:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Standart matris çarpım kullanarak, dönüştürülmüş noktaları aşağıdaki gibidir:

x' = x

y' = y

z' = 1

Varsayılan dönüştürme olmasıdır.

Zaman `Translate` yöntemi çağrılmadan `SKCanvas` nesnesi `tx` ve `ty` bağımsız değişkenleri `Translate` yöntemi dönüştürme matrisi üçüncü satır ilk iki hücrelerde olur:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Çarpma şimdi aşağıdaki gibidir:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Dönüştürme formüller şunlardır:

x' = x + tx

y' = y + ty

Ölçekleme faktörü 1 varsayılan bir değer olamaz. Çağırdığınızda `Scale` yöntemi yeni bir `SKCanvas` nesne içeren sonuç dönüştürme matrisi `sx` ve `sy` çapraz hücreler bağımsız değişkenler:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Dönüştürme formüller aşağıdaki gibidir:

x' = sx · x

y' = sy · y

Çağırdıktan sonra dönüştürme matrisi `Skew` ölçeklendirme etkenleri bitişik Matris hücrelerindeki iki bağımsız değişken içeriyor:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Dönüştürme formüller şunlardır:

x' = x + xSkew · y

y' = ySkew · x + y

Çağrı için `RotateDegrees` veya `RotateRadians` α açısı için dönüştürme matrisini aşağıdaki gibidir:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Dönüştürme formüller şunlardır:

x' = cos(α) · x - sin(α) · y

y' = sin(α) · x - cos(α) · y

α 0 derece olduğunda, kimlik matris olur. α 180 derece olduğunda dönüştürme matrisini aşağıdaki gibidir:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180 derece döndürme nesneyi yatay çevirme ve dikey olarak, hangi ölçek faktörleri – 1 ayarlayarak gerçekleştirilir eşdeğerdir.

Tüm bu tür dönüşümleri olarak sınıflandırılır *afin* dönüştürür. Afin dönüşümler hiçbir zaman 0, 0 ve 1 varsayılan değerlere kalır matrisi üçüncü sütun içerir. Makaleyi [olmayan afin dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) afin olmayan dönüşümler açıklanır.

## <a name="matrix-multiplication"></a>Matris çarpım

Dönüştürme matrisini kullanarak büyük avantajlarından biri bileşik dönüşümler genellikle SkiaSharp belgelerinde adlandırılır matris çarpım tarafından alınabilir *birleştirme*. İlgili Dönüştür yöntemlere birçok `SKCanvas` "ön birleştirme" ya da "pre-concat." Bu matris çarpım yer değiştirebilme olmadığından, önemli olan çarpma {ifade eder.

Örneğin, belgelerine [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) yöntemi diyor, BT'nin "Pre-concats belirtilen çeviri ile geçerli matris" belgelere sırasında için [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) yöntemi, BT'nin "Pre-concats belirtilen ölçekli geçerli matris." diyor

Bu yöntem çağrısı tarafından belirtilen dönüşüm çarpanı (sol işleneni) ve geçerli bir dönüştürme matrisi (sağ işleneni) multiplicand olduğunu anlamına gelir.

Varsayın `Translate` tarafından izlenen adlandırılır `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale` Dönüştürme çarpılarak `Translate` dönüştürmek için bileşik dönüştürme matrisi:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` önce çağrılabilir `Translate` şöyle:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

Bu durumda, çarpma sırasını tersine ve ölçeklendirme etkenleri etkili bir şekilde çeviri Etkenler uygulanır:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Burada `Scale` yöntemi ile bir pivot noktası:

```csharp
canvas.Scale(sx, sy, px, py);
```

Bu, aşağıdaki Çevir ve ölçek çağrıları eşdeğerdir:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Üç Dönüştürme Matrislerini ters sırada yöntemleri kodda görüntülenme çarpıldığı:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>SKMatrix yapısı

`SKMatrix` Yapısını tanımlar türü dokuz okuma/yazma özellikleri `float` dönüştürme matrisi dokuz hücrelere karşılık gelen:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` Ayrıca adlı bir özelliğini tanımlar [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) türü `float[]`. Bu özelliği ayarlamak veya sırada tek seferde dokuz değerleri elde etmek için kullanılabilir `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, ve `Persp2`.

`Persp0`, `Persp1`, Ve `Persp2` hücreleri makalesinde açıklanan [olmayan afin dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Bu hücreler varsayılan değerlerine 0, 0 ve 1 varsa, bu gibi koordinat noktası tarafından dönüştürme çarpılır:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x' = ScaleX · x + SkewX · y + TransX

y' = SkewX · x + ScaleY · y + TransY

z' = 1

Tam iki boyutlu afin dönüşüm budur. Afin dönüşüm dikdörtgen hiçbir zaman bir paralel kenarı dışında her şey dönüştürülür, yani paralel çizgi korur.

`SKMatrix` Yapısını tanımlar oluşturmak için çeşitli statik yöntemler `SKMatrix` değerleri. Bunlar tüm dönüş `SKMatrix` değerler:

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) pivot noktası ile
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) radyan cinsinden açı için
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) için bir Özet noktasıyla radyan cinsinden açı
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) pivot noktası ile
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` Ayrıca iki matrisi birleştirme çeşitli statik yöntemler bunları çarpılacağı anlamı tanımlar. Bu yöntemler adlı `Concat`, `PostConcat`, ve `PreConcat`, ve her iki sürümü vardır. Bu yöntemler hiçbir dönüş değerleri; yine de sahip istiyor musunuz? Bunun yerine, mevcut oldukları `SKMatrix` aracılığıyla değerleri `ref` bağımsız değişkenler. Aşağıdaki örnekte, `A`, `B`, ve `R` ("sonuç") için tüm olan `SKMatrix` değerleri.

İki `Concat` yöntemleri çağrılmadan şuna benzer:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Bunlar aşağıdaki çarpma gerçekleştirin:

R = B × A

Diğer yöntemleri, yalnızca iki parametreye sahiptir. İlk parametre değiştirilmiş ve yöntem çağrısının dönüş, iki matrisi ürün içerir. İki `PostConcat` yöntemleri çağrılmadan şuna benzer:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Bu çağrı, aşağıdaki işlemi gerçekleştirin:

A = A × B

İki `PreConcat` yöntemleri benzerdir:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Bu çağrı, aşağıdaki işlemi gerçekleştirin:

A = B × A

Bu yöntem çağrılarını tüm sürümlerini `ref` bağımsız değişkenler temel alınan uygulamaları arayan biraz daha verimlidir, ancak birine Kodunuzu okuyan ve herhangi bir şey ile varsayarak kafa karıştırıcı olabilir bir `ref` bağımsız değişkeni yöntemi tarafından değiştirildi. Ayrıca, genellikle bir sonuç birinin olmayan bir bağımsız değişken geçirmek uygun olan `Make` yöntemleri, örneğin:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

Bu, aşağıdaki matris oluşturur:

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

Çeviri dönüşüm tarafından çarpılan ölçeklendirme dönüşümü budur. Bu örnekte, `SKMatrix` yapısı adlı bir yöntem ile bir kısayol sunar [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Bu olduğu zaman güvenli birkaç kez biridir `SKMatrix` Oluşturucusu. `SetScaleTranslate` Yöntemi matrisi tüm dokuz hücreleri ayarlar. Kullanmak güvenlidir `SKMatrix` statik Oluşturucu `Rotate` ve `RotateDegrees` yöntemleri:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Bu yöntemler yapmak *değil* döndürme dönüşümü var olan bir dönüştürme için birleştirme. Yöntemleri matrisi tüm hücreleri ayarlayın. İşlevsel olarak aynı `MakeRotation` ve `MakeRotationDegrees` yöntemleri örneği yoktur ancak bu `SKMatrix` değeri.

Sahip olduğunuz varsayalım bir `SKPath` görüntülemek istediğiniz, ancak biraz farklı bir yön veya farklı bir orta nokta olmasını tercih nesnesi. Çağırarak bu yolun tüm koordinatların değiştirebilirsiniz [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) yöntemi `SKPath` ile bir `SKMatrix` bağımsız değişkeni. **Yolu dönüştürme** sayfa bunun nasıl yapılacağı gösterilmektedir. [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) Sınıf başvurularını `HendecagramPath` bir alan nesnesinde ancak kurucusu bir dönüşüm bu yolun uygulamak için kullanır:

```csharp
public class PathTransformPage : ContentPage
{
    SKPath transformedPath = HendecagramArrayPage.HendecagramPath;

    public PathTransformPage()
    {
        Title = "Path Transform";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        SKMatrix matrix = SKMatrix.MakeScale(3, 3);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(360f / 22));
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(300, 300));

        transformedPath.Transform(matrix);
    }
    ...
}
```

`HendecagramPath` Nesnesi bir merkezinde sahiptir (0, 0) ve tüm yönde 100 birimleri tarafından bu Merkezi'nden yıldız on noktalarının dışa genişletebilirsiniz. Bu, yolun pozitif ve negatif koordinatları olduğu anlamına gelir. **Yolu dönüştürme** sayfa tercih üç kez olarak yıldız büyük ve tüm pozitif koordinatları çalışmak. Ayrıca, bir yıldız yukarı doğru yöneltin noktasını istememektedir. Bunun yerine doğrudan aşağı işaret etmek için bir yıldız noktası istemektedir. (Yıldız on noktaları içerdiğinden, her ikisi de sahip olamaz.) Bu, yıldız 360 derece döndürme tarafından 22 bölünmüş gerektirir.

Oluşturucusu derlemeler bir `SKMatrix` kullanarak üç ayrı dönüşümler nesnesinden `PostConcat` A, B ve C nerede örneklerini şu deseni yöntemiyle `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Sonuç aşağıdaki gibi olması için bir dizi ardışık multiplications şudur:

A × B × C

Her dönüştürme ne yapacağını anlamak ardışık multiplications Yardımı. Koordinatlar –300 300 aralığı için ölçeklendirme dönüşümü 3, faktörüyle yolu koordinatları boyutu artar. Döndürme dönüşümü kaynağına geçici yıldız döndürür. Çeviri Dönüştürme sonra onu 300 piksel sağ tarafından ve aşağı, pozitif hale tüm koordinatları kaydırır.

Aynı matris üretmek diğer sırası yok. Başka bir şöyledir:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

Bu yol kendi merkezi etrafında ilk döndürür ve sağ 100 piksel çevirir ve bu nedenle tüm koordinatları pozitif. Yıldız sonra (0, 0) nokta, yeni sol üst köşesindeki göreli boyutu artar.

`PaintSurface` İşleyici yalnızca bu yolu oluştur:

```csharp
public class PathTransformPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Magenta;
            paint.StrokeWidth = 5;

            canvas.DrawPath(transformedPath, paint);
        }
    }
}

```

Tuvale sol üst köşesinde görüntülenir:

[![](matrix-images/pathtransform-small.png "Üçlü sayfasının ekran görüntüsü yolu dönüştürme")](matrix-images/pathtransform-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yolu dönüştürme")

Bu program Oluşturucusu matris aşağıdaki çağrıyı yolu için geçerlidir:

```csharp
transformedPath.Transform(matrix);
```

Yolun mu *değil* bu Matris özelliği olarak korur. Bunun yerine, bu dönüştürme tüm yol koordinatları için geçerlidir. Varsa `Transform` çağrılır yeniden dönüştürme yeniden uygulanır ve gittiğiniz geri tek dönüştürme alır başka bir matris uygulayarak yoludur. Neyse ki, `SKMatrix` yapısını tanımlayan bir [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix/) matrisi edinir yöntemi belirtilen bir matris tersine çevirir:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Yöntem çağrıldığında `TryInverse` çünkü tüm matrisleri tersi ancak tersi olmayan bir matris için bir grafik dönüştürme kullanılma olasılığı değil.

Ayrıca, bir matris dönüşüm uygulayabilirsiniz bir `SKPoint` değeri, bir dizi noktası, bir `SKRect`, ya da yalnızca tek bir sayı programınızdan. `SKMatrix` Yapısını destekler sözcüğüyle başlayan bir yöntem koleksiyonu ile bu işlemleri `Map`, bunlar gibi:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Son bu yöntemi kullanırsanız, aklınızda `SKRect` yapısı Döndürülmüş dikdörtgen temsil etme yeteneğine sahip değil. Yöntem yalnızca mantıklı bir `SKMatrix` çeviri temsil eden ve ölçeklendirme değeri.

### <a name="interactive-experimentation"></a>Etkileşimli deneme

Afin dönüştürme için bir fikir almak için bir etkileşimli olarak ekran geçici bir bit eşlem üç köşelerinde taşıma ve hangi dönüşüm sonuçları görmesini yoludur. Bu fikirdir arkasında **afin Matrisi Göster** sayfası. Bu sayfa ayrıca diğer gösterim kullanılan iki sınıf gerektirir:

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchPoint.cs) Sınıfı ekran sürüklenebilir saydam bir daire görüntüler. `TouchPoint` gerektiren bir `SKCanvasView` veya bir üst öğesi olan bir öğeyi bir `SKCanvasView` sahip [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchEffect.cs) bağlı. Ayarlama `Capture` özelliğine `true`. İçinde `TouchAction` olay işleyicisi program çağırmalıdır `ProcessTouchEvent` yönteminde `TouchPoint` her `TouchPoint` örneği. Yöntem `true` dokunma olay taşıma dokunma noktasında oluştuysa. Ayrıca, `PaintSurface` işleyici çağırmalıdır `Paint` yöntemi her `TouchPoint` kendisine geçirme örneği `SKCanvas` nesnesi.

`TouchPoint` bir ortak gösteren SkiaSharp visual ayrı bir sınıfta saklanmasını yolu. Sınıf görsel özelliklerini belirtmek için özellikler tanımlayabilir ve bir yöntem adlı `Paint` ile bir `SKCanvas` bağımsız değişkeni, işleyebilirsiniz.

`Center` Özelliği `TouchPoint` nesne konumunu belirtir. Bu özellik konumunu ayarlayabilirsiniz; Kullanıcı tuvale daire sürüklendiğinde özelliğini değiştirir.

**Afin matris sayfasını göster** de gerektirir [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/MatrixDisplay.cs) sınıfı. Bu sınıf hücrelerinin görüntüler bir `SKMatrix` nesnesi. İki genel yöntemi vardır: `Measure` işlenmiş matris boyutlarını elde edilir ve `Paint` görüntülemek için. Sınıfı içeren bir `MatrixPaint` türünde özellik `SKPaint` , değiştirilebilir farklı yazı tipi boyutu veya renk için.

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) dosya başlatır `SKCanvasView` ve ekler bir `TouchEffect`. [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) arka plan kod dosyası oluşturur üç `TouchPoint` nesneleri ve katıştırılmış yükleyen bir bit eşlem üç köşelerinde karşılık gelen konumlara ayarlar Kaynak:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    SKMatrix matrix;
    SKBitmap bitmap;
    SKSize bitmapSize;

    TouchPoint[] touchPoints = new TouchPoint[3];

    MatrixDisplay matrixDisplay = new MatrixDisplay();

    public ShowAffineMatrixPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }

        touchPoints[0] = new TouchPoint(100, 100);                  // upper-left corner
        touchPoints[1] = new TouchPoint(bitmap.Width + 100, 100);   // upper-right corner
        touchPoints[2] = new TouchPoint(100, bitmap.Height + 100);  // lower-left corner

        bitmapSize = new SKSize(bitmap.Width, bitmap.Height);
        matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                           touchPoints[1].Center,
                                           touchPoints[2].Center);
    }
    ...
}
```

Afin matris benzersiz olarak üç noktaları tarafından tanımlanır. Üç `TouchPoint` nesneleri bit eşlem sol üst, sağ üst ve sol alt köşesindeki karşılık gelir. Afin matris yalnızca dikdörtgen bir paralel kenarı dönüştürme yeteneğine sahip olduğundan, dördüncü noktası diğer üç tarafından kapsanır. Çağrısıyla Oluşturucusu sonucuna `ComputeMatrix`, hücrelerinin hesaplar bir `SKMatrix` bu üç nokta nesnesinden.

`TouchAction` İşleyicisi çağrılarını `ProcessTouchEvent` yöntemi her `TouchPoint`. `scale` Değeri piksel Xamarin.Forms koordinatları dönüştürür:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = canvasView.CanvasSize.Width / (float)canvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                               touchPoints[1].Center,
                                               touchPoints[2].Center);
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Varsa `TouchPoint` taşındı yöntemini çağırır sonra `ComputeMatrix` yeniden ve yüzeyini geçersiz kılar.

`ComputeMatrix` Yöntemi bu üç noktaları tarafından kapsanan matris belirler. Matris adlı `A` bir paralel kenarı bir tek pikselli kare dikdörtgen dayalı üç noktalarında dönüşümler while adlı ölçeklendirme dönüşümü `S` tek pikselli kare dikdörtgen için bit eşlem ölçeklendirir. Bileşik matris olan `S` × `A`:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL)
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

        SKMatrix result;
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Son olarak, `PaintSurface` yöntemi bu matrisinde tabanlı bit eşlem işler, ekranın alt kısmında matrisi görüntüler ve üç köşeleri dokunma noktalarda bit eşlem oluşturur:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap using the matrix
        canvas.Save();
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(matrix);

        matrixDisplay.Paint(canvas, matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));

        // Display the touchpoints
        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }
  }
```

Sayfa da ilk yüklendiğinde iki diğer ekranlar, bazı işleme sonra gösterirken aşağıdaki iOS ekrana bit eşlem gösterir:

[![](matrix-images/showaffinematrix-small.png "Üçlü sayfasının ekran görüntüsü afin Matrisi Göster")](matrix-images/showaffinematrix-large.png#lightbox "Üçlü sayfasının ekran görüntüsü afin Matrisi Göster")

Dokunma noktaları bit eşlem köşelerinde sürükleyin gibi görünüyor. ancak, yalnızca bir görünümü olmasıdır. Böylece köşeleri dokunma noktalarıyla çakıştığı dokunma noktalarından hesaplanan matris bit eşlem dönüştürür.

Taşıma, yeniden boyutlandırma ve köşeleri sürükleyerek değil bit eşlemler döndürmek kullanıcılar için daha doğal ancak kullanarak bir veya iki parmakları doğrudan sürüklemek için nesne üzerinde çimdik ve döndür. Bu sonraki makalesinde ele [Touch işleme](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

### <a name="the-reason-for-the-3-by-3-matrix"></a>3 x 3 matris nedeni

İki boyutlu grafik sistemi yalnızca 2 x 2 dönüştürme matrisi gerektirecek beklenen:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Bu ölçeklendirme, döndürme ve hatta eğriltme çalışır, ancak yeteneğine değil en çeviri olan temel dönüşümlerini olan.

2 x 2 matris temsil ettiğini sorunudur bir *doğrusal* dönüştürme iki boyut. Bazı temel aritmetik işlemler doğrusal bir dönüşüm korur ancak etkileri doğrusal dönüştürme (0, 0) noktası hiçbir zaman olarak değiştiren biridir. Doğrusal bir dönüşüm çeviri mümkün hale getirir.

Üç boyutlarında doğrusal dönüştürme matrisi şöyle görünür:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Etiketli hücre `SkewXY` değeri Y değerlerine göre X koordinatı Eğer anlamına gelir; hücre `SkewXZ` Z; değerlerine göre X koordinatı değeri Eğer ve değerleri eğme benzer şekilde diğeri için anlamına gelir `Skew` hücreleri.

Bu 3B bir dönüştürme matrisi ayarlayarak iki boyutlu bir düzlem kısıtlamak mümkündür `SkewZX` ve `SkewZY` 0 olarak ve `ScaleZ` 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

İki boyutlu grafik 3B uzaydaki burada Z eşittir 1 düzlemi tamamen üzerinde çizilir, dönüştürme çarpma şöyle görünür:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Her şeyi burada Z eşittir 1, iki boyutlu düzlemi üzerinde kalır ancak `SkewXZ` ve `SkewYZ` hücreleri etkili bir şekilde iki boyutlu çeviri Etkenler haline gelir.

Bu, nasıl üç boyutlu doğrusal dönüştürme iki boyutlu bir doğrusal olmayan dönüştürme hizmet veren olur. (Benzerleme tarafından bir 4 x 4 matris 3B grafik dönüşümler dayanır.)

`SKMatrix` SkiaSharp yapısında üçüncü satır özelliklerini tanımlar:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Sıfır olmayan değerleri `Persp0` ve `Persp1` neden olduğu Z eşittir 1 iki boyutlu düzlemi kapalı nesneleri taşıma içindeki dönüşümler. Bu nesneler bu düzlemi geri taşındığında ne olur makalesinde yer alan [olmayan afin dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
