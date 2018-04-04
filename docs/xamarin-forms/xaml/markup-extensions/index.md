---
title: XAML işaretleme uzantıları
description: Hangi XAML özniteliklerini ayarla kaynakları aralığını genişletin
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: b81bc4b31edd1d8b8f5f43f97885c38e889dd32c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-markup-extensions"></a>XAML işaretleme uzantıları

XAML işaretleme uzantılarına gücü ve esnekliği XAML metin dizelerini dışındaki kaynaklardan ayarlanacak öğesi özniteliklerini vererek genişletmek yardımcı olur.

Örneğin, normal olarak ayarladığınız `Color` özelliği `BoxView` şöyle:

```xaml
<BoxView Color="Blue" />
```

Veya, onaltılık RGB renk değerine ayarlayın:

```xaml
<BoxView Color="#FF0080" />
```

Her iki durumda da metin dizesini ayarlamak `Color` özniteliği için dönüştürülür bir `Color` tarafından değeri [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) sınıfı.

Ayarlamak bunun yerine tercih edebilirsiniz `Color` özniteliği bir kaynak sözlüğü depolanan bir değer veya oluşturduğunuz bir sınıfın statik bir özelliğin değerini veya türünde bir özellik `Color` sayfasında başka öğesinin veya oluşturulan gelen ton, Doygunluk ve parlaklığını değerleri ayırın.

XAML biçimlendirme uzantıları kullanarak tüm bu seçenekler mümkündür. Ancak tümcecik izin vermeyin "biçimlendirme uzantıları" korkutmak: XAML biçimlendirme uzantıları *değil* XML uzantıları. XAML biçimlendirme uzantıları ile bile, XAML her zaman yasal XML'dir. 

Biçimlendirme uzantısı bir öznitelik, bir öğenin express aslında bir farklı yoludur. XAML işaretleme uzantılarına süslü ayraç içine bir öznitelik ayarı tarafından genellikle tanımlanabilir:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Süslü ayraçlar hiçbir öznitelik ayardır *her zaman* XAML biçimlendirme uzantısı. Ancak, göreceğiniz gibi XAML işaretleme uzantılarına da süslü ayraçlar kullanmadan başvurulabilir.

Bu makalede iki bölüme ayrılır:

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[XAML Biçimlendirme Uzantılarını Kullanma](consuming.md)  

XAML biçimlendirme uzantıları Xamarin.Forms içinde tanımlı kullanın.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[XAML Biçimlendirme Uzantıları Oluşturma](creating.md) 

Kendi özel XAML biçimlendirme uzantıları yazma.



## <a name="related-links"></a>İlgili bağlantılar

- [Biçimlendirme uzantıları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML biçimlendirme uzantıları bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Kaynak Sözlükler](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dinamik Stiller](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Veri Bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md)
