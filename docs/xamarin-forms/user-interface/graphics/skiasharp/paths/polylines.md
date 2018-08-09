---
title: Çoklu çizgiler ve parametreli denklemler
description: Bu makalede nasıl herhangi bir satır işlemek için kullanım SkiaSharp ile parametreli denklemler tanımlayabileceğiniz açıklar ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-skiasharp
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9118ca8e23e4c4a9023a1add89e26c4484979c8f
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615801"
---
# <a name="polylines-and-parametric-equations"></a>Çoklu çizgiler ve parametreli denklemler

_Herhangi bir satır ile parametreli denklemler tanımlayabilirsiniz işlenecek SkiaSharp kullanma_

Bu kılavuz bir sonraki bölümünde, çeşitli yöntemler görürsünüz, `SKPath` eğrileri belirli türlerdeki işlemek için tanımlar. Ancak, bazen bir tür tarafından doğrudan desteklenmeyen eğrisinin çizmek gereklidir `SKPath`. Böyle bir durumda, matematiksel olarak tanımlayan bir eğri çizme için çoklu (bağlı satır koleksiyonu) kullanabilirsiniz. Varsa satırları yeterince duruma ve çok sayıda yeterli, sonuç bir eğri gibi görünür. Bu gönderilerinizi gerçekten 3.600 küçük satırları görürsünüz:

![](polylines-images/spiralexample.png "Bir gönderilerinizi")

Genellikle bir eğri parametreli denklemler çifti açısından tanımlamak idealdir. X ve Y, koordinatları için bunlar denklemler adlandırılan üçüncü değişkenine bağlı `t` kez. Örneğin, aşağıdaki parametreli denklemler 1 (0, 0) noktasında ortalanmış bir RADIUS daire tanımladığınız *t* 0-1:

 x cos(2πt) y = sin(2πt) =

 Bir RADIUS 1'den daha büyük istiyorsanız, yalnızca Sinüs ve Kosinüs değerlerini, yarıçap çarpın ve merkezi başka bir konuma taşımanız gerekiyorsa bu değerleri ekleyin:

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

İki yarıçaplarını için üç noktanın yatay ve dikey eksen paralel ile ilgilidir:

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

Ardından, çeşitli noktaları hesaplar ve bu yola ekler döngü içinde eşdeğer SkiaSharp kod koyabilirsiniz. Aşağıdaki SkiaSharp kod oluşturur bir `SKPath` uzaklaştırabilir dolduran bir elips nesnesi. Döngü 360 derece doğrudan geçiş yapar. Merkezi yarım genişlik ve yükseklik uzaklaştırabilir, ve bu nedenle iki yarıçaplarını:

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

360 küçük satırları tarafından tanımlanan bir elips sonuçlanır. İşlendiğinde, kesintisiz olarak görünür.

Elbette, bir elips çoklu kullanarak oluşturmanız gerekmez `SKPath` içeren bir `AddOval` bunu sizin için halleder yöntemi. Ancak tarafından sağlanmayan bir görsel nesneyi çizin isteyebileceğiniz `SKPath`.

**Archimedean Gönderilerinizi** sayfası olan benzer kodu elips koda ancak önemli bir fark dışında. Bu 360 derece dairenin 10 kez RADIUS sürekli olarak ayarlama döngüsü:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

Sonuç olarak da adlandırılan bir *aritmetik gönderilerinizi* her döngü arasındaki uzaklığı sabit olduğu için:

[![](polylines-images/archimedeanspiral-small.png "Üçlü sayfasının ekran görüntüsü Archimedean Gönderilerinizi")](polylines-images/archimedeanspiral-large.png#lightbox "Archimedean Gönderilerinizi sayfanın üç ekran görüntüsü")

Dikkat `SKPath` oluşturulan bir `using` blok. Bu `SKPath` daha fazla bellek tüketir `SKPath` öneren önceki programlarda nesneleri bir `using` yönetilmeyen tüm kaynaklarını silmek daha uygun blok.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
