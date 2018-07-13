---
title: XAML biçimlendirme uzantıları
description: Makale, gücü ve esnekliği XAML öğesi özniteliklerini düz metin dizelerini dışındaki kaynaklardan ayarlanacak vererek genişletmek için Xamarin.Forms XAML biçimlendirme uzantıları kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d507ff3c74de6bb4ea36c1a7b7dc2cd5dd60823b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996746"
---
# <a name="xaml-markup-extensions"></a>XAML biçimlendirme uzantıları

XAML biçimlendirme uzantıları, gücü ve esnekliği XAML öğesi özniteliklerini düz metin dizelerini dışındaki kaynaklardan ayarlanacak vererek genişletmeye yardımcı.

Örneğin, normalde ayarlamanız `Color` özelliği `BoxView` şöyle:

```xaml
<BoxView Color="Blue" />
```

Veya bir onaltılık RGB renk değeri ayarlayın:

```xaml
<BoxView Color="#FF0080" />
```

Metin dizesi her iki durumda da kümesine `Color` özniteliği dönüştürülen bir `Color` tarafından değeri [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) sınıfı.

Ayarlamak bunun yerine tercih edebilirsiniz `Color` özniteliği bir kaynak sözlüğünde depolanan bir değer veya oluşturduğunuz bir sınıfın statik bir özellik değerini veya vlastnost typu `Color` sayfasında başka bir öğe veya gelen oluşturulmuş ton, Doygunluk ve parlaklık değerleri virgülle ayırın.

XAML biçimlendirme uzantıları kullanarak tüm bu seçenekler mümkündür. Ancak tümceciği izin vermeyin "biçimlendirme uzantıları" korkutmak: XAML biçimlendirme uzantıları *değil* XML uzantıları. XAML biçimlendirme uzantıları ile bile, XAML, her zaman geçerli XML olur.

Aslında bir öznitelik bir öğenin ifade etmek için farklı şekilde bir işaretleme uzantısıdır. XAML biçimlendirme uzantıları, küme ayraçları içine alınmış bir özniteliğini adlarıyla genellikle şunlardır:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Küme ayracı hiçbir öznitelik ayardır *her zaman* XAML işaretleme uzantısı. Ancak, gördüğünüz gibi XAML biçimlendirme uzantıları da küme ayracı kullanmadan başvurulabilir.

Bu makalede iki bölüme ayrılır:

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[XAML Biçimlendirme Uzantılarını Kullanma](consuming.md)  

Xamarin.Forms içinde tanımlanan XAML biçimlendirme uzantıları kullanın.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[XAML Biçimlendirme Uzantıları Oluşturma](creating.md)

Kendi özel XAML biçimlendirme Uzantıları'nı yazın.



## <a name="related-links"></a>İlgili bağlantılar

- [Biçimlendirme uzantıları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML biçimlendirme uzantıları Xamarin.Forms kitabı bölümden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Kaynak Sözlükler](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dinamik Stiller](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Veri Bağlama](~/xamarin-forms/app-fundamentals/data-binding/index.md)
