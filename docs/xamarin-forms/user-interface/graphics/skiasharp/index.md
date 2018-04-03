---
title: Xamarin.Forms içinde SkiaSharp kullanma
description: Xamarin.Forms uygulamalarınızda 2B grafikleri için SkiaSharp kullanın
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: 2183601ca6e642b2fb21f640ed4fddfe2c300dc8
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="using-skiasharp-in-xamarinforms"></a>Xamarin.Forms içinde SkiaSharp kullanma

_Xamarin.Forms uygulamalarınızda 2B grafikleri için SkiaSharp kullanın_

SkiaSharp .NET ve C# Google ürünlerinde yaygın kullanılan açık kaynaklı Skia grafik altyapısı tarafından desteklenen bir 2B grafik sistemidir. 2B vektör grafikleri, bit eşlemler ve metin çizmek için Xamarin.Forms uygulamalarınızda SkiaSharp kullanabilirsiniz. Bkz: [2B çizim](~/graphics-games/skiasharp/index.md) SkiaSharp Kitaplığı hakkında daha fazla genel bilgi ve bazı diğer öğreticileri için Kılavuzu.

Bu kılavuz, Xamarin.Forms programlamayla bildiğinizi varsayar.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Web Semineri: Xamarin.Forms SkiaSharp**

## <a name="skiasharp-preliminaries"></a>SkiaSharp başlangıç kuralları

Xamarin.Forms SkiaSharp bir NuGet paketi olarak paketlenir. Xamarin.Forms çözümünü Visual Studio veya Visual Studio'da Mac için oluşturduktan sonra aramak için NuGet Paket Yöneticisi'ni kullanabilirsiniz **SkiaSharp.Views.Forms** paketini ve çözümünüze ekleyin. Onay **başvuruları** bölümü, bakın her projesi SkiaSharp ekledikten sonra çeşitli **SkiaSharp** kitaplıkları, çözümdeki projelerin her biri için eklenmiştir.

Xamarin.Forms uygulamanız iOS hedefliyorsa, iOS 8.0 minimum dağıtım hedefini değiştirmek için proje özellikleri sayfasını kullanın.

Eklenecek isteyeceksiniz SkiaSharp kullanan bir C# sayfasında bir `using` için yönerge [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) tüm SkiaSharp sınıflar, yapılar ve grafiklerinizi içinde kullanacağınız numaralandırmaları kapsayan ad alanı programlama. Ayrıca isteyeceksiniz bir `using` için yönerge [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) Xamarin.Forms için belirli sınıfları için ad alanı. En önemli olan sınıf ile çok daha küçük gibi bir ad alanı burasıdır [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). Bu sınıf Xamarin.Forms türetilen `View` sınıfı ve SkiaSharp grafik çıkışını barındırır.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms` Ad alanı da içeren bir `SKGLView` öğesinden türetilen sınıf `View` ancak OpenGL işleme grafik için kullanır. Kolaylık olması amacıyla, bu kılavuzda kendisine kısıtlayan `SKCanvasView`, ancak kullanarak `SKGLView` yerine oldukça benzer.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Temel SkiaSharp Çizimi Bilgileri](basics/index.md)

SkiaSharp ile çizebilirsiniz basit grafik şekiller daire, Oval ve dikdörtgenler bazılarıdır. Bu rakamları görüntülenmesini de SkiaSharp koordinatları, boyutlar ve renkler hakkında bilgi edineceksiniz.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[SkiaSharp Satırları ve Yolları](paths/index.md)

Bir grafik yolu bağlı düz çizgiler ve eğrilerle dizisidir. Yollar vuruş, doldurulmuş, veya her ikisini de. Bu konuda stroke sona erer ve birleştirmeler ve dahil kesikli çizgi çizme ve noktalı çizgiler ancak eğri geometri eksikliği durakları pek çok görünüşünün kapsar.

## <a name="skiasharp-transformstransformsindexmd"></a>[SkiaSharp Dönüşümleri](transforms/index.md)

Dönüşümler hep çevrilmiş, ölçeği, döndürülen eğri veya grafik nesneleri izin verin. Bu makalede ayrıca afin olmayan dönüşümler oluşturma ve yollara dönüşümler uygulanırken bir standart 3 x 3 dönüştürme matrisi nasıl kullanabileceğiniz gösterilmektedir.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[SkiaSharp Eğrileri ve Yolları](curves/index.md)

Yollar incelenmesi Eğriler yolu nesnelere ekleme ve diğer güçlü yolu özellikleri yararlanmasını ile devam eder. Nasıl kısa metin dizesindeki tüm bir yolu belirtebilirsiniz, yol etkileri kullanma ve yol iç derinliklerine nasıl görürsünüz.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Xamarin.Forms Web Semineri (video) ile SkiaSharp](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
