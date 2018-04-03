---
title: SkiaSharp dönüşümler
description: Dönüşümler SkiaSharp grafik görüntüleme hakkında bilgi edinin
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 668488ab7efae66f1777e9ae6ded1f725833fe16
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="skiasharp-transforms"></a>SkiaSharp dönüşümler

_Dönüşümler SkiaSharp grafik görüntüleme hakkında bilgi edinin_

SkiaSharp destekleyen yöntemleri uygulanan geleneksel grafik dönüşümler [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) nesnesi. Dönüşümler koordinatları ve belirttiğiniz boyutları matematiksel, alter `SKCanvas` grafik nesneleri işlenen işlevleri çizim. Dönüşümler, genellikle animasyon veya yinelenen grafik çizim için kullanışlıdır. Bazı teknikleri &mdash; bit eşlemler veya metin döndürme gibi &mdash; dönüşümler kullanmadan mümkün değildir.

SkiaSharp dönüşümler aşağıdaki işlemleri destekler:

- *Çevir* shift koordinatlarına bir konumdan diğerine
- *Ölçek* artırmak veya koordinatları ve boyutları azaltmak için
- *Döndürme* koordinatları bir nokta çevresinde döndürmek için
- *Eğme* kaydırılacak yatay veya dikey koordinatları dikdörtgen bir paralel kenarı hale

Bunlar olarak bilinir *afin* dönüştürür. Afin dönüşümler paralel çizgi her zaman korumak ve hiçbir zaman boyutu veya bir koordinat sonsuz hale gelmesine neden. Kare hiçbir zaman bir paralel kenarı dışında her şey dönüştürülür ve bir daire hiçbir zaman elips dışında her şey dönüştürülür.

SkiaSharp afin olmayan dönüşümler da destekler (olarak da bilinir *projective* veya *perspektif* dönüştürür) üzerinde standart 3 x 3 dönüştürme matrisi tabanlı. Afin olmayan bir dönüşüm dışbükey herhangi quadrilateral (dört taraflı şekil tüm iç açıları ile değerinden 180 derece) dönüştürülecek bir kare sağlar. Olmayan afin dönüşümler koordinatları veya boyutları sonsuz hale gelmesine neden olabilir, ancak 3B efektler için önemli.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>SkiaSharp ve Xamarin.Forms dönüşümler arasındaki farklar

Xamarin.Forms SkiaSharp benzer dönüşümler de destekler. Xamarin.Forms [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) sınıfı aşağıdaki dönüştürme özellikleri tanımlar:

- `TranslationX` Ve `TranslationY`
- `Scale`
- `Rotation`, `RotationX`, ve `RotationY`

`RotationX` Ve `RotationY` 3B quasi efektler oluşturmak perspektif dönüşümler özelliklerdir.

SkiaSharp dönüşümler Xamarin.Forms dönüşümler arasında birkaç önemli farklılıklar vardır:

İlk farktır SkiaSharp dönüşümler tüm uygulanır `SKCanvas` kişiye Xamarin.Forms dönüşümler uygulanırken nesne `VisualElement` türevleri. (Xamarin.Forms dönüşümler uygulayabilirsiniz `SKCanvasView` kendisi, çünkü nesne `SKCanvasView` türetilen `VisualElement`, ancak içindeki `SKCanvasView`, SkiaSkarp dönüşümler geçerlidir.)

SkiaSharp dönüşümler göre sol üst köşesindeki olan `SKCanvas` Xamarin.Forms dönüşümler göre sol üst köşesindeki durumdayken `VisualElement` hangi uygulandıkları için. Ölçeklendirme uygularken bu fark önemlidir ve bu dönüşümler her zaman belirli bir noktaya göre olduğundan döndürme dönüştürür.

Gerçekten büyük fark SKiaSharp dönüşümler olmasıdır *yöntemleri* Xamarin.Forms dönüşümler durumdayken *özellikleri*. Bu bir söz dizimi fark ötesinde anlamsal farktır: SkiaSharp dönüşümler bir duruma Xamarin.Forms dönüşümler kümesi bir işlem gerçekleştirin. SkiaSharp dönüşümler sonradan çizilmiş grafik nesneleri, ancak dönüştürme uygulanmadan önce çizilir değil grafik nesneler için geçerlidir. Buna karşılık, özellik ayarlanmışsa hemen Xamarin.Forms dönüştürme önceden işlenmiş öğesine uygulanır. Yöntemleri adlı gibi SkiaSharp dönüşümler toplu; Başka bir değerle özelliği ayarlandığında Xamarin.Forms dönüşümler değiştirilir.

Bu bölümdeki tüm örnek programlar başlığı altında görünür **dönüştüren** giriş sayfasındaki [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program ve [ **Dönüştüren** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms) klasörü çözümün.

## <a name="the-translate-transformtranslatemd"></a>[Çeviri Dönüşümü](translate.md)

SkiaSharp grafik kaydırılacak Çevir Dönüştür kullanmayı öğrenin.

## <a name="the-scale-transformscalemd"></a>[Ölçekleme Dönüşümü](scale.md)

Çeşitli boyutlarda nesnelere ölçeklendirmeye yönelik SkiaSharp ölçeklendirme dönüşümü bulur.

## <a name="the-rotate-transformrotatemd"></a>[Döndürme Dönüşümü](rotate.md)

Etkiler ve animasyonları SkiaSharp döndürme dönüşümü ile olası keşfedin.

## <a name="the-skew-transformskewmd"></a>[Eğme Dönüşümü](skew.md)

Bkz. nasıl eğme dönüştürmesi Eğimli grafik nesneleri SkiaSharp oluşturabilirsiniz.

## <a name="matrix-transformsmatrixmd"></a>[Matris Dönüşümleri](matrix.md)

Çok yönlü dönüştürme matrisi ile SkiaSharp dönüşümler içine daha derin daha yakından inceleyin.

## <a name="touch-manipulationstouchmd"></a>[Dokunma İşlemeleri](touch.md)

Dokunma işlemeleri sürükleyerek, ölçeklendirme ve döndürme uygulamak için kullanım matris dönüştürür.

## <a name="non-affine-transformsnon-affinemd"></a>[İlişkili Olmayan Dönüşümler](non-affine.md)

Afin olmayan dönüşüm efektleri ile oridinary ötesine gidin.

## <a name="3d-rotation3d-rotationmd"></a>[3B Döndürme](3d-rotation.md)

Afin olmayan dönüşümler 2B nesnelere 3D alanda döndürmek için kullanın.


## <a name="related-links"></a>İlgili bağlantılar

- [SkiaSharp API'leri](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (örnek)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
