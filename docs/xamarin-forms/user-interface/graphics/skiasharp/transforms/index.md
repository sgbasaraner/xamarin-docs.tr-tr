---
title: SkiaSharp dönüşümleri
description: Bu makalede Xamarin.Forms uygulamalarında SkiaSharp grafikleri görüntülemek için dönüşümleri keşfediyor ve bu örnek kod ile gösterir.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: e2f83e3de574d11052b5301f1832988b7f4166bf
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998653"
---
# <a name="skiasharp-transforms"></a>SkiaSharp dönüşümleri

_SkiaSharp grafikleri görüntülemek için dönüşümleri hakkında bilgi edinin_

SkiaSharp destekler yöntemleri uygulanan geleneksel grafik dönüşümler [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) nesne. Dönüşümler koordinatları ve belirttiğiniz boyutları matematiksel olarak, alter `SKCanvas` grafik nesneler işlenir gibi işlevleri çizim. Dönüşümleri genellikle yinelenen grafik çizim veya animasyon için kullanışlıdır. Bazı teknikleri &mdash; bit eşlemler veya metin döndürme gibi &mdash; dönüşümleri kullanmadan mümkün değildir.

SkiaSharp dönüşümleri aşağıdaki işlemleri destekler:

- *Çevirme* shift koordinatlara bir konumdan diğerine
- *Ölçek* artırmak veya koordinatları ve boyutları azaltmak için
- *Döndürme* koordinatları bir nokta etrafında döndürmek için
- *Eğriltme* kaydırmak için yatay veya dikey olarak düzenler ve böylece bir dikdörtgen bir eğdiğinizde Paralel Kenar olur.

Bunlar olarak bilinen *afin* dönüştürür. Afin dönüşümler her zaman paralel satırları Koru ve hiçbir zaman bir koordinat ya da aynı boyuta sınırsız duruma neden olur. Bir kare hiçbir zaman bir eğdiğinizde Paralel Kenar dışındaki herhangi bir şey olarak dönüştürülür ve bir daire hiçbir zaman bir elips dışındaki herhangi bir şey olarak dönüştürülür.

SkiaSharp, ilişkili olmayan dönüşümler da destekler (olarak da bilinir *projective* veya *perspektif* dönüştüren) üzerinde standart bir 3-x-3 dönüştürme matrisi göre. İlişkili olmayan bir dönüşüm dışbükey herhangi quadrilateral (bir dört taraflı şekil tüm iç açıları ile değerinden 180 derece) dönüştürülecek bir kare sağlar. Olmayan afin dönüşümler koordinatları veya boyutları sınırsız duruma neden olabilir, ancak 3B efektleri için önemlidir.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>SkiaSharp dönüşümleri Xamarin.Forms arasındaki farklar

Xamarin.Forms içinde SkiaSharp benzer dönüşümleri da destekler. Xamarin.Forms [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) sınıfı aşağıdaki dönüştürme özellikleri tanımlar:

- `TranslationX` ve `TranslationY`
- `Scale`
- `Rotation`, `RotationX`, ve `RotationY`

`RotationX` Ve `RotationY` quasi 3B efektleri oluşturun perspektif dönüşümler özelliklerdir.

SkiaSharp dönüşümleri ve Xamarin.Forms dönüşümler arasında bazı önemli farklılıklar vardır:

İlk fark SkiaSharp dönüşümleri tüm uygulanan `SKCanvas` kişiye Xamarin.Forms dönüşümler uygulanırken nesne `VisualElement` türevleri. (Xamarin.Forms dönüşümler uygulayabilirsiniz `SKCanvasView` kendisi, çünkü nesne `SKCanvasView` türetildiği `VisualElement`, ancak içindeki `SKCanvasView`, SkiaSkarp dönüşüm uygulama.)

SkiaSharp dönüşümleri sol üst köşesine göre olan `SKCanvas` Xamarin.Forms dönüşümler göre sol üst köşesinde çalışırken `VisualElement` bunlar uygulandığı için. Ölçeklendirme uygularken bu fark önemlidir ve bu dönüştürmeler her zaman belirli bir noktaya göre olduğundan döndürme dönüştürür.

SKiaSharp dönüşümleri olduğunu gerçekten büyük fark ise *yöntemleri* Xamarin.Forms dönüşümler sırasında *özellikleri*. Söz dizimi fark ötesinde bir anlam fark budur: SkiaSharp dönüşümleri Xamarin.Forms dönüştürmeler bir durumu ayarlanırken bir işlem gerçekleştirin. SkiaSharp dönüşümleri, dönüşüm uygulamadan önce çizilir grafik nesneleri ancak sonradan çizilen grafik nesneleri uygulayın. Buna karşılık, bu özelliğin ayarlanması hemen sonra daha önce oluşturulmuş bir öğe için Xamarin.Forms dönüşüm uygular. SkiaSharp dönüşümleri, yöntem de çağrıldığında gibi toplu; Başka bir değerle özelliği ayarlandığında Xamarin.Forms dönüşümler değiştirilir.

Bu bölümdeki tüm örnek programlar başlığının altında görünür **dönüştüren** giriş sayfasında [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) programı ve [ **Dönüştüren** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) Çözüm klasörü.

## <a name="the-translate-transformtranslatemd"></a>[Çeviri Dönüşümü](translate.md)

Çeviri dönüşümü SkiaSharp grafik kaydırmak için kullanmayı öğrenin.

## <a name="the-scale-transformscalemd"></a>[Ölçekleme Dönüşümü](scale.md)

SkiaSharp ölçekleme dönüşümü, nesnelerin çeşitli boyutlarına ölçeklendirmeye yönelik keşfedin.

## <a name="the-rotate-transformrotatemd"></a>[Döndürme Dönüşümü](rotate.md)

Efektler ve animasyon SkiaSharp döndürme dönüşümü ile olası keşfedin.

## <a name="the-skew-transformskewmd"></a>[Eğme Dönüşümü](skew.md)

Eğme dönüşümü içinde SkiaSharp Eğimli grafik nesneleri nasıl oluşturabileceğiniz konusuna bakın.

## <a name="matrix-transformsmatrixmd"></a>[Matris Dönüşümleri](matrix.md)

SkiaSharp dönüşümleri çok yönlü bir dönüştürme matrisi ile derinlerine.

## <a name="touch-manipulationstouchmd"></a>[Dokunma İşlemeleri](touch.md)

Dokunma işlemeleri sürükleyerek, ölçeklendirme ve döndürme uygulamak için kullanım matris dönüştürür.

## <a name="non-affine-transformsnon-affinemd"></a>[İlişkili Olmayan Dönüşümler](non-affine.md)

İlişkili olmayan bir dönüşüm etkileri olan oridinary ötesine gidin.

## <a name="3d-rotation3d-rotationmd"></a>[3B Döndürme](3d-rotation.md)

2B nesneleri 3B alanda döndürme için ilişkili olmayan dönüşümler kullanın.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
