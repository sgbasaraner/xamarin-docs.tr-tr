---
title: Kullansa ve parametrik denklemini
description: Bu makalede nasıl satırıyla işlemek için kullanım SkiaSharp ile parametrik denklemini tanımlayabileceğiniz açıklar ve bu örnek kodu ile gösterir.
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9539a21b7dbc91da63795639610886233ed705be
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245315"
---
# <a name="polylines-and-parametric-equations"></a>Kullansa ve parametrik denklemini

_İle parametrik denklemini tanımlayabilirsiniz çizgiyi işlemek için SkiaSharp kullanın_

Bu kılavuzda daha sonra bir parçası, çeşitli yöntemleri görürsünüz, `SKPath` belirli türde bir Eğriler işlemek için tanımlar. Ancak, bazen bir tür tarafından doğrudan desteklenmeyen eğri çizmek ise gerekli `SKPath`. Böyle bir durumda, matematiksel olarak tanımlayabilirsiniz eğrisi çizmek için çoklu çizgi (bağlantılı çizgilerin koleksiyonunu) kullanabilirsiniz. Varsa satırları küçük duruma ve çok sayıda yeterli, sonuç bir eğri gibi görünür. Bu spiral gerçekte 3.600 küçük satırları şöyledir:

![](polylines-images/spiralexample.png "Spiralin")

Genellikle bir eğri parametrik denklemini çifti açısından tanımlamak en iyisidir. X ve Y koordinatları için denklemini bunlar olarak da adlandırılan bir üçüncü değişkenine, bağlı `t` süre. Örneğin, aşağıdaki parametrik denklemini RADIUS (0, 0) noktada ortalanmış 1 ile bir daire tanımlamanız *t* 0 ile 1 arasında:

 x cos(2πt) y = sin(2πt) =

 Bir RADIUS 1'den büyük istediğiniz yaparsanız, yalnızca o yarıçap Sinüs ve Kosinüs değerleri Çarp ve merkezi başka bir konuma taşımanız gerekirse, bu değerleri ekleyin:

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

Yatay ve dikey eksen paralel ile elips için iki yarıçaplarını oynayan:

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

Ardından, çeşitli noktaları hesaplar ve bu bir yola ekler döngü eşdeğer SkiaSharp kodu koyabilirsiniz. Aşağıdaki SkiaSharp kod oluşturur bir `SKPath` görüntü yüzeyini doldurur elips nesnesi. Döngü üzerinden 360 derece doğrudan geçiş yapar. Yarı genişlik ve yükseklik görüntü yüzeyinin merkezi olduğunu ve dolayısıyla iki yarıçaplarını:

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

Bu, 360 küçük satırları tarafından tanımlanan bir elips sonuçlanır. İşlendiğinde, kesintisiz görüntülenir.

Elbette, bir çoklu çizgi kullanarak bir elips oluşturmanız gerekmez `SKPath` içeren bir `AddOval` sizin için yapar yöntemi. Ancak tarafından sağlanmayan görsel bir nesne çizmek isteyebilirsiniz `SKPath`.

**Archimedean Spiral** sayfasına sahip code benzer elips kod, ancak çok önemli bir fark. Bu 360 derece dairenin 10 kez sürekli olarak RADIUS ayarlama döngü:

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

Sonuç olarak da adlandırılan bir *aritmetik spiral* her döngü arasındaki uzaklığı sabit olduğundan:

[![](polylines-images/archimedeanspiral-small.png "Üçlü sayfasının ekran görüntüsü Archimedean Spiral")](polylines-images/archimedeanspiral-large.png#lightbox "Üçlü sayfasının ekran görüntüsü Archimedean Sarmal")

Dikkat `SKPath` oluşturulan bir `using` bloğu. Bu `SKPath` daha fazla bellek tüketir `SKPath` öneren önceki programları nesneleri bir `using` blok yönetilmeyen tüm kaynaklarını silmek daha uygun.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
