---
title: SkiaSharp, matris dönüşümleri
description: Bu makalede, çok yönlü bir dönüştürme matrisi ile SkiaSharp dönüşümleri içine derin türlerine geçiyor ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: fafa883d013701b9e72e544aff03739a7ff9230c
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130874"
---
# <a name="matrix-transforms-in-skiasharp"></a>SkiaSharp, matris dönüşümleri

_SkiaSharp dönüşümleri çok yönlü bir dönüştürme matrisi ile derinlerine_

Uygulanan Dönüşümlerin `SKCanvas` nesne içinde tek bir örneği birleştirilmiş [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) yapısı. Tüm modern 2B grafikler sistemlere benzer bir standart 3 x 3 dönüştürme matrisi budur.

Gördüğünüz gibi dönüşümler SkiaSharp matris, ancak dönüştürme matrisi, teorik açısından önemlidir ve dönüşümler yolları değiştirmek için kullanırken önemlidir dönüştürme hakkında bilmek olmadan veya her ikisini de karmaşık dokunma girişini işleme için kullanabilirsiniz Bu makalede ve sonraki gösterilmiştir.

![](matrix-images/matrixtransformexample.png "Afin bir dönüşüm tabi bir bit eşlem")

Uygulanan geçerli dönüştürme matrisi `SKCanvas` salt okunur erişerek herhangi bir zamanda kullanılabilir [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) özelliği. Yeni bir dönüştürme matrisi kullanarak ayarlayabilirsiniz [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) yöntemi ve geri yükleyebilir, bu dönüştürme matrisi varsayılan değerlere çağırarak [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/).

Yalnızca diğer `SKCanvas` doğrudan tuval matris dönüşüm ile çalışan üye [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) hangi sıralar iki matrislerde birlikte çarpılarak.

Varsayılan dönüştürme matrisi kimlik matrisi olan ve 1 çapraz hücreler ve 0 her yerde başka oluşur:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

' Using static bir kimlik matrisi oluşturabilirsiniz [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) yöntemi:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix` Varsayılan oluşturucu yapar *değil* kimlik matrisi döndürür. Sıfıra ayarlanır hücrelerin tümü ile bir matris döndürür. Kullanmayın `SKMatrix` Oluşturucusu hücrelere el ile ayarlamak planlamıyorsanız.

SkiaSharp bir grafik nesnesi oluşturulduğunda, her bir nokta (x, y) üçüncü sütunda 1 ile 1 ile 3 matris etkili bir şekilde dönüştürülür:

<pre>
| x  y  1 |
</pre>

Bu 1 ile 3 matris 1 olarak ayarlanmış Z koordinatı ile üç boyutlu bir noktayı temsil eder. Üç boyutlu çalışan iki boyutlu bir matris dönüşüm gerektirir neden (daha sonra açıklanmıştır) matematik neden vardır. Burada Z eşittir 1 olarak temsil eden bir nokta bir 3B koordinat sisteminde, ancak her zaman 2B düzlemine bu 1 ile 3 matrisi düşünebilirsiniz.

Bu 1 ile 3 matris sonra dönüştürme matrisi ile çarpılır ve sonuç tuval üzerinde işlenen noktası:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

Standart matris çarpım kullanarak, dönüştürülen noktalar aşağıda belirtilmiştir:

x' = x

y' y =

z' = 1

Varsayılan dönüştürme olmasıdır.

Zaman `Translate` yöntemi çağrıldığında `SKCanvas` nesnesi `tx` ve `ty` bağımsız değişkenleri `Translate` yöntemi dönüştürme matris üçüncü satırda ilk iki hücreyi olur:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Çarpma işlemi artık şu şekilde olur:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Dönüştürme formülleri şunlardır:

x' = x + tx

y' = y + ty

Ölçekleme faktörü 1 varsayılan bir değere sahip. Çağırdığınızda `Scale` yöntemi yeni bir `SKCanvas` nesne içeren sonuç dönüştürme matrisi `sx` ve `sy` çapraz hücrelerde bağımsız değişkenleri:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Dönüştürme formülleri aşağıdaki gibidir:

x' sx · = x

y' sy · = y

Arama sonra dönüştürme matrisi `Skew` Matris hücrelerindeki ölçekleme faktörü için bitişik iki bağımsız değişken içerir:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Dönüştürme formülleri şunlardır:

x' = x + xSkew · y

y' ySkew · = x + y

Çağrıdan `RotateDegrees` veya `RotateRadians` α açısı için dönüştürme matrisi aşağıdaki gibidir:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Dönüştürme formülleri şunlardır:

x' cos(α) · = x - sin(α) · y

y' sin(α) · = x - cos(α) · y

0 derece α olduğunda, kimlik matrisi var. 180 derece α olduğunda, dönüştürme matrisi aşağıdaki gibidir:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

180 derece döndürme yatay olarak bir nesneyi çevirme ve dikey olarak, ölçek faktörünü – 1 değerini ayarlayarak gerçekleştirilir eşdeğerdir.

Bu tür dönüşümleri olarak sınıflandırılan *afin* dönüştürür. Afin dönüşümler üçüncü sütunda, 0, 0 ve 1 varsayılan değerlerinde kalır matris hiçbir zaman içerir. Makaleyi [olmayan afin dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) ilişkili olmayan dönüşümler açıklanır.

## <a name="matrix-multiplication"></a>Matris çarpım

Dönüştürme matrisi kullanarak büyük avantajlarından biri bileşik dönüşümler genellikle SkiaSharp belgelerinde adlandırılan matris çarpım tarafından alınabilir *birleştirme*. Birçok dönüştürme ilgili yöntemlere `SKCanvas` "öncesi birleştirme" ya da "pre-concat." Bu matris çarpım yer değiştirebilirlik olmadığı için önemli olan çarpma sırasına başvuruyor.

Örneğin, belgeler için [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) yöntemi yazan BT'nin "Öncesi concats belirtilen çeviri geçerli matris" sırasında belgeler için [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) yöntemi, BT "Pre-concats belirtilen ölçeği geçerli matris." diyor.

Bu yöntem çağrısı tarafından belirtilen dönüşüm çarpan (sol işlenen) ve geçerli bir dönüştürme matrisi çarpan (sağ taraf işleneni) anlamına gelir.

Varsayın `Translate` tarafından izlenen çağrılır `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale` Dönüştürme çarpılarak `Translate` dönüştürmek için bileşik bir dönüştürme matrisi:

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

Bu durumda, çarpma işleminin sıra ters çevrilir ve ölçekleme faktörü çeviri faktörlerine etkili bir şekilde uygulanır:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

İşte `Scale` pivot noktasıyla yöntemi:

```csharp
canvas.Scale(sx, sy, px, py);
```

Bu, aşağıdaki çeviri ve ölçeklendirme çağrısına eşdeğerdir:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Üç dönüştürme matrislerde yöntemleri kodda görüntülenme ters sırada çarpıldığı:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>SKMatrix yapısı

`SKMatrix` Yapısını tanımlayan dokuz okuma/yazma özellikleri türü `float` dönüştürme matrisi dokuz hücrelere karşılık gelen:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` adlı bir özellik de tanımlar [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) türü `float[]`. Bu özelliği ayarlamak veya sırada tek seferde dokuz değerlerini almak için kullanılabilir `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, ve `Persp2`.

`Persp0`, `Persp1`, Ve `Persp2` hücreleri makalede ele alınan [olmayan afin dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Bu hücre varsayılan değerlerine 0, 0 ve 1 varsa, böyle bir koordinat nokta dönüştürme çarpılır:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x' ScaleX · = x + SkewX · y + TransX

y' SkewX · = x + ScaleY · y + TransY

z' = 1

Tam iki boyutlu afin dönüşüm budur. Afin dönüşüm paralel satırları, yani bir dikdörtgen bir eğdiğinizde Paralel Kenar dışındaki herhangi bir şey olarak hiçbir zaman dönüştürülür korur.

`SKMatrix` Yapısını tanımlayan oluşturmak için çeşitli statik yöntemler `SKMatrix` değerleri. Bu tüm dönüş `SKMatrix` değerleri:

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) pivot noktası ile
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) için radyan cinsinden açı
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) pivot noktası ile radyan cinsinden açı için
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) pivot noktası ile
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` Ayrıca, iki matrisler, birleştirme birkaç statik yöntemler bunları çarpılacak anlamına gelir tanımlar. Bu yöntemler adlı `Concat`, `PostConcat`, ve `PreConcat`, ve her iki sürümü vardır. Bu yöntemler, hiçbir dönüş değerlerine sahip; Bunun yerine, mevcut oldukları `SKMatrix` aracılığıyla değerleri `ref` bağımsız değişkenler. Aşağıdaki örnekte, `A`, `B`, ve `R` ("sonuç") için tüm bunlar `SKMatrix` değerleri.

İki `Concat` yöntemler şu şekilde adlandırılır:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Bunlar aşağıdaki çarpma işlemi gerçekleştirin:

R = B × A

Diğer yöntemleri, yalnızca iki parametreye sahiptir. İlk parametre, değiştirdiğiniz ve yöntem çağrısından dönüşte, iki matrislerde ürününü içeriyor. İki `PostConcat` yöntemler şu şekilde adlandırılır:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Bu çağrılar aşağıdaki işlemi gerçekleştirin:

A B BİR × =

İki `PreConcat` benzer yöntemler şunlardır:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Bu çağrılar aşağıdaki işlemi gerçekleştirin:

A = B × A

Bu yöntem çağrılarının tüm sürümleri `ref` bağımsız değişkenler, temel alınan uygulamaları arayan biraz daha verimli ancak birine Kodunuzu okuyan ve herhangi bir şey ile varsayarak kafa karıştırıcı bir `ref` bağımsız değişkeni yöntem tarafından değiştirildi. Ayrıca, genellikle aşağıdakilerden biri bir sonucu olan bir bağımsız değişken geçmek uygun olan `Make` yöntemleri, örneğin:

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

Çeviri dönüşümü ile çarpılmış ölçekleme dönüşümü budur. Bu özel durumda `SKMatrix` yapı adında bir yöntemi ile bir kısayol sağlar [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Bu güvenli olduğunda birkaç kez biridir `SKMatrix` Oluşturucusu. `SetScaleTranslate` Matris dokuz tüm hücreleri yöntemini ayarlar. Kullanılacak güvenlidir `SKMatrix` statik bir oluşturucuyla `Rotate` ve `RotateDegrees` yöntemleri:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Bu yöntemler yapmak *değil* döndürme dönüşümü için var olan bir dönüştürme birleştirin. Matrisin tüm hücreler yöntemleri ayarlayın. Aynı işleve sahiptir olduklarını `MakeRotation` ve `MakeRotationDegrees` yöntemleri örneği yoksa dışında `SKMatrix` değeri.

Sahip olduğunuz varsayalım bir `SKPath` görüntülemek istediğiniz, ancak biraz farklı yön ayarına veya farklı bir orta nokta olduğunu tercih nesnesi. Söz konusu yoldaki tüm koordinatlarını çağırarak değiştirebilirsiniz [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) yöntemi `SKPath` ile bir `SKMatrix` bağımsız değişken. **Yolu dönüştürme** sayfasında bunun nasıl yapılacağı gösterilmektedir. [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) Sınıf başvuruları `HendecagramPath` nesne alanındaki ancak kullanır, yapıcısına bu yolun bir dönüştürme uygulamak için:

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

`HendecagramPath` Nesnesinde bir merkezinde (0, 0) ve yıldız on noktalarının her yöne 100 birimi tarafından bu Merkezi'nden dışa genişletebilirsiniz. Bu, yolun pozitif ve negatif koordinatları olduğu anlamına gelir. **Yolu dönüştürme** sayfa tercih üç kez olabildiğince büyük bir yıldız ile ve tüm pozitif koordinatları ile çalışmak. Ayrıca, bunu bir yıldız yukarı doğru yöneltin noktasını istememektedir. Bunun yerine doğrudan aşağı işaret etmek için bir yıldız noktası istiyor. (Yıldız on noktalarına sahip olduğundan, her ikisini birden olamaz.) Bu, yıldız 360 derece döndürme 22 bölünmüş gerektirir.

Oluşturucu oluşturur bir `SKMatrix` kullanarak üç ayrı dönüşümler nesneden `PostConcat` A, B ve C olduğu örneklerini şu desene yöntemiyle `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Bu sonucu şu şekilde olacak şekilde art arda gelen multiplications dizisidir:

A × B × C

Her dönüştürme ne yaptığını anlamak ardışık multiplications Yardımı. Koordinatları –300 300 aralığı. Bu nedenle ölçekleme dönüşümü 3 faktörüyle yolu koordinatları boyutu artar. Döndürme dönüşümü yıldız kaynağına çevresinde döndürür. Çeviri dönüşümü ardından, 300 piksel sağa ve aşağı, pozitif hale tüm koordinatları haline gelir.

Aynı matris oluşturan diğer dizileri vardır. Başka bir şöyledir:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

Bu merkezi etrafındaki yolun ilk döndürür ve ardından sağ 100 piksel çevirir ve böylece tüm ölçeğini koordinatları pozitif. Yıldız ardından noktası (0, 0) olan yeni sol üst köşesine göre boyutu artar.

`PaintSurface` İşleyici yalnızca bu yol oluştur:

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

Tuvalin sol üst köşesinde görünür:

[![](matrix-images/pathtransform-small.png "Üçlü sayfasının ekran görüntüsü yolu dönüştürme")](matrix-images/pathtransform-large.png#lightbox "Üçlü sayfasının ekran görüntüsü yolu dönüştürme")

Bu programın Oluşturucusu matris çağrısını yoluyla uygulama hedefi:

```csharp
transformedPath.Transform(matrix);
```

Yol mu *değil* bu Matris bir özellik olarak korur. Bunun yerine, bu dönüşüm tüm yol koordinatlarını uygular. Varsa `Transform` çağrılır yeniden dönüştürme yeniden uygulanır ve dönüştürme alır başka bir matris uygulayarak geri gidebilir tek yol budur. Neyse ki, `SKMatrix` yapısını tanımlayan bir [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix/) matrisi, alır yöntemi, belirli bir matris tersine çevirir:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Bu yöntem çağrıldığında `TryInverse` tüm matrislerde tersi, ancak tersi olmayan bir matris, grafik dönüştürme için kullanılacak olasılığı düşüktür.

Matris dönüşüm de uygulayabilirsiniz bir `SKPoint` değeri, bir dizi noktaları, bir `SKRect`, veya programınızdan bile yalnızca tek bir sayı. `SKMatrix` Yapısı kelimesiyle başlayan yöntemlerin koleksiyonunu bu işlemleri destekleyen `Map`, bunlar gibi:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Bu son yöntemi kullandığınızda, aklınızda `SKRect` yapısı döndürülmüş bir dikdörtgen gösterebilen değil. Yöntemi yalnızca mantıklı bir `SKMatrix` çeviri temsil eden ve ölçeklendirme değeri.

### <a name="interactive-experimentation"></a>Etkileşimli deneme

Afin dönüştürme için bir genel görünüm yapmanın bir yolu, etkileşimli olarak üç köşelerini ekranın etrafında bir bit eşlem taşıma ve hangi dönüşüm sonuçları görme ' dir. Bu fikirdir arkasında **afin Matrisi Göster** sayfası. Bu sayfa ayrıca diğer tanıtımlar içinde kullanılan iki sınıf gerektirir:

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchPoint.cs) Sınıfı, ekranın sürüklenebilir saydam bir daire görüntüler. `TouchPoint` gerektiren bir `SKCanvasView` veya üst öğesi olan bir öğeyi bir `SKCanvasView` sahip [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/TouchEffect.cs) bağlı. Ayarlama `Capture` özelliğini `true`. İçinde `TouchAction` olay işleyicisi, program çağırmalıdır `ProcessTouchEvent` yönteminde `TouchPoint` her `TouchPoint` örneği. Yöntem döndürür `true` touch olay taşıma touch noktasında oluştuysa. Ayrıca, `PaintSurface` işleyici çağırmalıdır `Paint` yöntemi her `TouchPoint` aktarması örneği `SKCanvas` nesne.

`TouchPoint` ortak bir gösteren ayrı bir sınıf içinde SkiaSharp görsel kapsüllenmiş, yolu. Sınıf görsel özelliklerini belirtmek için özellikleri tanımlayabilir ve adlı yöntemi `Paint` ile bir `SKCanvas` bağımsız değişkeni, onu işleyebilirsiniz.

`Center` Özelliği `TouchPoint` nesnenin konumu gösterir. Bu özellik, konumunu başlatamıyor ayarlanabilir; kullanıcının tuval çember sürüklediğinde özelliğini değiştirir.

**Afin matris sayfasını göster** ayrıca gerektirir [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/MatrixDisplay.cs) sınıfı. Bu sınıf hücrelerinin görüntüler bir `SKMatrix` nesne. İki genel yöntemi vardır: `Measure` işlenmiş matris boyutları edinme ve `Paint` görüntülenecek. Sınıfı içeren bir `MatrixPaint` türünün özelliği `SKPaint` , değiştirilebilir farklı yazı tipi boyutu veya renk için.

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) dosya başlatır `SKCanvasView` bağlayan bir `TouchEffect`. [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) arka plan kod dosyası oluşturur üç `TouchPoint` nesneleri ve konumlara bir embedded'dan yükleyen bir bit eşlem üç köşelerini karşılık gelen ayarlar Kaynak:

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
        {
            bitmap = SKBitmap.Decode(stream);
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

Afin bir matris üç noktayla benzersiz şekilde tanımlanır. Üç `TouchPoint` nesneleri, bit eşlem sol, sağ ve sol alt köşelerini karşılık gelir. Afin bir matris yalnızca dikdörtgen bir eğdiğinizde Paralel Kenar dönüştürme yapabildiğinden, dördüncü noktası tarafından diğer üç yapılmamaktadır. Oluşturucu çağrısı ile sonucuna `ComputeMatrix`, hücrelerinin hesaplayan bir `SKMatrix` bu üç nokta nesneden.

`TouchAction` İşleyicisi çağrılarını `ProcessTouchEvent` yöntemi her `TouchPoint`. `scale` Değeri piksel Xamarin.Forms koordinatlarından dönüştürür:

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

Varsa `TouchPoint` taşınmış, yöntemi çağıran sonra `ComputeMatrix` yeniden ve yüzey geçersiz kılar.

`ComputeMatrix` Yöntemi bu üç noktayla kapsanan matris belirler. Matris adlı `A` bir piksel bir kare dikdörtgen bir eğdiğinizde Paralel Kenar göre üç noktalarında dönüşümler while adlı ölçekleme dönüşümü `S` bit eşlemin piksel bir kare dikdörtgen için ölçeklendirir. Bileşik matris olan `S` × `A`:

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

        SKMatrix result = SKMatrix.MakeIdentity();
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Son olarak, `PaintSurface` yöntemi, matris tabanlı bit eşlem işler, ekranın alt kısmında matrisi görüntüler ve bit eşlem dokunma üç köşelere işler:

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

Sayfa da ilk yüklendiğinde iki diğer ekranlar, sonra bazı işleme gösterirken iOS ekran aşağıdaki bit eşlem gösterir:

[![](matrix-images/showaffinematrix-small.png "Üçlü sayfasının ekran görüntüsü afin Matrisi Göster")](matrix-images/showaffinematrix-large.png#lightbox "Üçlü sayfasının ekran görüntüsü afin Matrisi Göster")

Dokunma bit eşlemin köşeleri sürükleyerek gibi görünüyor olsa da, yalnızca bir görünümü olur. Köşeleri dokunma ile çakıştığı böylece dokunmatik noktalarından hesaplanan matris bit eşlem dönüştürür.

Taşıma, yeniden boyutlandırma ve köşeleri sürükleyerek değil bit eşlemler döndürmek kullanıcılar için daha doğal ancak kullanarak bir veya iki parmağınızı doğrudan sürüklemek için nesne üzerinde sıkıştırma ve döndürün. Bu sonraki makalede ele [düzenleme dokunma](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

### <a name="the-reason-for-the-3-by-3-matrix"></a>3-x-3 matris nedeni

İki boyutlu grafik sistemi yalnızca 2-tarafından-2 dönüştürme matrisi gerektirecek beklenebilir:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Bu, ölçeklendirme, döndürme ve hatta eğme için çalışır, ancak özellikli değil en çeviri olan temel, Dönüşümlerin.

2-tarafından-2 matrisi temsil ettiğini sorunudur bir *doğrusal* iki boyutta dönüştürün. Bazı temel aritmetik işlemleri doğrusal dönüşüm korur, ancak etkilerini doğrusal dönüşüm hiçbir zaman noktası (0, 0) değiştiren biridir. Doğrusal bir dönüştürme, çeviri imkansız hale getirir.

Üç boyutlu bir doğrusal bir dönüştürme matrisi şöyle görünür:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Etiketli hücre `SkewXY` y değerlerine göre X koordinatı değeri eğriltir anlamına gelir; hücre `SkewXZ` değeri Z; değerlerine göre X koordinatı eğriltir ve değerleri eğme benzer şekilde diğer anlamına gelir `Skew` hücreleri.

Ayarlayarak bu 3B bir dönüştürme matrisi için iki boyutlu bir düzlem kısıtlamak mümkündür `SkewZX` ve `SkewZY` 0 ve `ScaleZ` 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

İki boyutlu grafik tamamen Uçağın nerede Z eşittir 1 3B alanda üzerinde çizilir, dönüştürme çarpma şöyle görünür:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Her şeyi burada Z eşittir 1, iki boyutlu gezmeyi kalır ancak `SkewXZ` ve `SkewYZ` hücreleri etkili bir şekilde iki boyutlu çeviri Etkenler haline gelir.

Nasıl üç boyutlu bir doğrusal dönüştürme iki boyutlu bir doğrusal olmayan dönüştürme hizmet veren budur. (Benzerleme tarafından bir 4 x 4 matris 3B grafik içindeki dönüşümler dayanır.)

`SKMatrix` SkiaSharp yapısında üçüncü satır için özellikleri tanımlar:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Sıfır olmayan değerleri `Persp0` ve `Persp1` neden olduğu Z eşittir 1 iki boyutlu düzlemi kapalı nesneleri taşıma içindeki dönüşümler. Bu düzlemine nesneleri geri taşındığında ne makalesinde üzerinde kapsamında [olmayan afin dönüşümler](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
