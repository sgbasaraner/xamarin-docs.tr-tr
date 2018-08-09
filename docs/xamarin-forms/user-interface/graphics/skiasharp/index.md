---
title: Xamarin.Forms içinde SkiaSharp kullanma
description: SkiaSharp, .NET ve C# Google ürünler, yaygın kullanılan açık kaynaklı Skia grafik altyapısı tarafından desteklenen bir 2B grafik sistemidir. Bu kılavuz, 2B grafikler kullanarak Xamarin.Forms uygulamalarında SkiaSharp kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: f7d97b798bf2a5a75af0731a665fe212491a6516
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615879"
---
# <a name="using-skiasharp-in-xamarinforms"></a>Xamarin.Forms içinde SkiaSharp kullanma

_2B grafikler kullanarak Xamarin.Forms uygulamalarında SkiaSharp kullanma_

SkiaSharp, .NET ve C# Google ürünler, yaygın kullanılan açık kaynaklı Skia grafik altyapısı tarafından desteklenen bir 2B grafik sistemidir. 2B vektör grafikleri, bit eşlemler ve metin çizmek için Xamarin.Forms uygulamalarınızı SkiaSharp kullanabilirsiniz. Bkz: [2B çizim](~/graphics-games/skiasharp/index.md) SkiaSharp Kitaplığı hakkında daha fazla genel bilgi ve bazı diğer öğreticiler için Kılavuzu.

Bu kılavuz, Xamarin.Forms programlama ile ilgili bilgi sahibi olduğunuzu varsayar.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Web Semineri: SkiaSharp Xamarin.Forms için**

## <a name="skiasharp-preliminaries"></a>SkiaSharp hazırlık aşamaları

SkiaSharp Xamarin.Forms için bir NuGet paketi olarak paketlenir. Xamarin.Forms çözümü Visual Studio veya Visual Studio Mac için oluşturduktan sonra NuGet Paket Yöneticisi aramak için kullanabileceğiniz **SkiaSharp.Views.Forms** paketini ve çözümünüze ekleyin. Size onay **başvuruları** bölümünde her projesi SkiaSharp ekledikten sonra gördüğünüz gibi çeşitli **SkiaSharp** kitaplıkları, çözümdeki projelerin her biri için eklenmiştir.

Xamarin.Forms uygulamanızı iOS hedefliyorsa, proje özellikleri sayfasında, iOS 8.0 en düşük dağıtım hedefini değiştirmek için kullanın.

SkiaSharp kullanan bir C# sayfasında eklemek isteyeceksiniz bir `using` yönergesi [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) tüm SkiaSharp sınıflar, yapılar ve grafiklerinizi kullanacağınız numaralandırmalar kapsayan ad alanı programlama. Ayrıca isteyeceksiniz bir `using` yönergesi [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) Xamarin.Forms için belirli sınıfları için ad alanı. En önemli işlemin sınıf ile çok daha küçük ad alanı, budur [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). Bu sınıf Xamarin.Forms türetilen `View` sınıfı ve SkiaSharp grafik çıkışınızı barındırır.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms` Ad alanı da içeren bir `SKGLView` türetilen sınıf `View` ancak OpenGL için grafik işleme kullanır. Kolaylık olması amacıyla, bu kılavuzun kendisine sınırlar `SKCanvasView`, ancak kullanarak `SKGLView` yerine oldukça benzerdir.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Temel SkiaSharp Çizimi Bilgileri](basics/index.md)

SkiaSharp ile çizebilirsiniz basit grafikler rakamları daireler, elips ve dikdörtgen bazılarıdır. Bu rakamlar görüntülenmesinde SkiaSharp koordinatları, boyutları ve renkleri hakkında bilgi edineceksiniz.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[SkiaSharp Satırları ve Yolları](paths/index.md)

Bir grafik yolu, bağlı düz çizgiler ve eğrilerle dizisidir. Konturlu, doldurulmuş, veya her ikisini de. Bu konuda vuruş uçları ve birleştirme gibi ve kesikli çizgi çizme ve noktalı çizgiler, ancak eğri geometriler eksikliği durakları birçok yönünü kapsar.

## <a name="skiasharp-transformstransformsindexmd"></a>[SkiaSharp Dönüşümleri](transforms/index.md)

Dönüşümler birörnek çevrilmiş, ölçeği, döndürülen dengesiz veya grafik nesneleri sağlar. Bu makalede ayrıca, ilişkili olmayan dönüşümler oluşturup yollara dönüşümler uygulanırken için standart bir 3-x-3 dönüştürme matrisi nasıl kullanabileceğinizi gösterir.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[SkiaSharp Eğrileri ve Yolları](curves/index.md)

Yolları incelenmesi için bir yol nesneleri eğrileri ekleme ve diğer güçlü bir yol özellikleri kötüye devam eder. Nasıl kısa metin dizesindeki tüm bir yol belirtebilirsiniz, yol etkileri kullanma ve yol iç işlevler araştırma yapmak nasıl görürsünüz.

## <a name="skiasharp-bitmapsbitmapsindexmd"></a>[SkiaSharp Bit Eşlemleri](bitmaps/index.md)

Bit eşlemler, bir görüntü cihazı piksele karşılık gelen bit dikdörtgen dizilerdir. Bu makale serisi, yükleme, kaydetme görüntülemek oluşturma Çiz animasyon ekleme ve SkiaSharp bit eşlem bitleri erişim gösterilmektedir.

## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [SkiaSharp ile Xamarin.Forms Web Semineri (video)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
