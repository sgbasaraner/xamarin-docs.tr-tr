---
title: Düzen sıkıştırma
description: Düzen sıkıştırma belirtilen düzenleri görsel ağaç'tan sayfa işleme performansını girişimi kaldırır. Bu makalede, Düzen sıkıştırma ve getirebilir miyim avantajları etkinleştirme açıklanmaktadır.
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: ba9be51daa32be1034e2bdfafafe80c45d00d83c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995238"
---
# <a name="layout-compression"></a>Düzen sıkıştırma

_Düzen sıkıştırma belirtilen düzenleri görsel ağaç'tan sayfa işleme performansını girişimi kaldırır. Bu makalede, Düzen sıkıştırma ve getirebilir miyim avantajları etkinleştirme açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms özyinelemeli yöntem çağrılarının iki serinin kullanarak Düzen gerçekleştirir:

- Üst kısmında bir sayfa ile görsel ağaç düzeni başlar ve tüm dallar sayfasında her görsel öğe kapsayacak şekilde görsel ağacı yoluyla ilerler. Üst öğeleri diğer öğelere olan öğeler, kendilerini göreli alt öğelerini konumlandırma ve boyutlandırma sorumludur.
- Geçersiz kılma olarak yeni bir düzen döngüsü bir öğe bir sayfa üzerinde değişiklik tetikler işlemidir. Artık doğru boyut ve konum varsa, öğeleri geçersiz olarak kabul edilir. Alt öğelerinden birine boyutları değiştiğinde, her öğenin alt öğeleri olan görsel ağaç'uyarı almak. Bu nedenle, bir öğenin görsel ağaç boyutundaki bir değişikliği ağacın ripple değişiklikleri neden olabilir.

Xamarin.Forms düzeni nasıl gerçekleştireceğini hakkında daha fazla bilgi için bkz. [özel düzen oluşturma](~/xamarin-forms/user-interface/layouts/custom.md).

Yerel denetimleri hiyerarşisini Düzen işleminin sonucudur. Ancak, bu hiyerarşi daha fazla iç içe hiyerarşisini görüntüle inflating platform oluşturucular için ek kapsayıcı oluşturucular ve sarmalayıcılar içerir. Xamarin.Forms bir sayfasını görüntülemek için gerçekleştirmesi gereken iş miktarı arttıkça daha derin iç içe geçme düzeyi; Karmaşık düzenler için hiyerarşisini görüntüle ayrıntılı ve geniş kapsamlı, birden çok iç içe geçen düzeye sahip olabilir.

Örneğin, aşağıdaki düğmeyi Facebook ile oturum açmak için örnek uygulamadaki göz önünde bulundurun:

![](layout-compression-images/facebook-button.png "Facebook düğmesi")

Bu düğme, aşağıdaki XAML görünüm hiyerarşisine sahip özel bir denetim olarak belirtilir:

```xaml
<ContentView ...>
    <StackLayout>
        <StackLayout ...>
            <AbsoluteLayout ...>
                <Button ... />    
                <Image ... />
                <Image ... />
                <BoxView ... />
                <Label ... />
                <Button ... />
            </AbsoluteLayout>
        </StackLayout>
        <Label ... />
    </StackLayout>    
</ContentView>
```

Ortaya çıkan iç içe görünüm hiyerarşisi ile incelenebilir [Xamarin Inspector'ı](~/tools/inspector/index.md). Android, iç içe görünüm hiyerarşisi 17 görünümleri içerir:

![](layout-compression-images/no-compression.png "Facebook düğmesi hiyerarşisini görüntüle")

İOS ve Android platformlarında Xamarin.Forms uygulamaları için kullanılabilir olan düzen sıkıştırma düzleştirilecek belirtilen düzenleri sayfa işleme performansını iyileştirebilir sanal ağaçtan kaldırarak iç içe görünüm amaçlar. Teslim edilen performans avantajı, bir sayfa, kullanılan işletim sistemi sürümü ve uygulamanın üzerinde çalıştığı aygıtın karmaşıklığına bağlı olarak değişir. Ancak, en büyük gelen performans artışı eski cihazlarda görülür.

> [!NOTE]
> Bu makalede, Android Düzen sıkıştırma uygulanması sonucu üzerinde odaklanmakla birlikte, iOS için eşit oranda geçerlidir.

## <a name="layout-compression"></a>Düzen sıkıştırma

XAML içinde düzen sıkıştırma ayarlayarak etkinleştirilebilir `CompressedLayout.IsHeadless` özelliğine bağlı `true` Düzen sınıfı hakkında:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Alternatif olarak, C# ' de ilk bağımsız değişkeni olarak bir düzen örneği belirterek etkinleştirilebilir `CompressedLayout.SetIsHeadless` yöntemi:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Düzen sıkıştırma görsel ağaç'tan bir düzen kaldırır olduğundan, görsel görünümüne sahip olan ya da, dokunma girişini elde düzenleri için uygun değil. Bu nedenle, düzeni kümesi [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) özellikleri (gibi [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor), [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible), [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation), [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) ve [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) veya hareketlerini kabul edin, düzen için aday değildir Sıkıştırma. Ancak, Düzen sıkıştırma, görsel görünümünü özelliklerini ayarlar veya hareketleri, kabul eden bir düzende etkinleştirme bir derleme veya çalışma zamanı hatasına neden olmaz. Bunun yerine, Düzen sıkıştırma uygulanır ve görsel görünümünü özellikleri ve tanıma hareketi, sessizce başarısız olur.

Facebook düğmesi için üç düzen sınıflarında Düzen sıkıştırma etkin hale getirilebilir:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
    <StackLayout CompressedLayout.IsHeadless="true" ...>
        <AbsoluteLayout CompressedLayout.IsHeadless="true" ...>
            ...
        </AbsoluteLayout>
    </StackLayout>
    ...
</StackLayout>  
```

Android'de bu 14 görünümleri iç içe görünüm hiyerarşisinde sonuçlanır:

![](layout-compression-images/layout-compression.png "Düzen sıkıştırma ile Facebook düğmesi hiyerarşisini görüntüle")

17 görünümler özgün iç içe görünüm hiyerarşisi karşılaştırıldığında, bu bir azalma %17 görüntüleme sayısını temsil eder. Hacmin anlamsız görünebilir, ancak bir sayfanın tamamını üzerinden görünümü azaltma daha önemli olabilir.

### <a name="fast-renderers"></a>Hızlı oluşturucular

Hızlı Oluşturucular, sonuçta elde edilen yerel görünüm hiyerarşisi düzleştirme tarafından Enflasyon ve Xamarin.Forms denetimleri android'de işleme maliyetlerini azaltın. Bu daha fazla performans hangi sırayla sonuçları daha az karmaşık bir görsel ağaç ve bellek kullanımını daha az daha az nesne oluşturarak artırır. Hızlı oluşturucular hakkında daha fazla bilgi için bkz: [hızlı oluşturucular](~/xamarin-forms/internals/fast-renderers.md).

Facebook düğmesi için örnek uygulamayı hızlı oluşturucular Düzen sıkıştırma birleştirmesiyle 8 görünümleri iç içe görünüm hiyerarşisi oluşturur:

![](layout-compression-images/layout-compression-with-fast-renderers.png "Düzen sıkıştırma ve hızlı oluşturucular ile Facebook düğmesi hiyerarşisini görüntüle")

17 görünümler özgün iç içe görünüm hiyerarşisi karşılaştırıldığında, bu bir indirim % 52'ye temsil eder.

Örnek uygulama, gerçek bir uygulamadan ayıklanan bir sayfa içerir. Düzen sıkıştırma ve hızlı oluşturucular sayfa android'de 130 görünümleri iç içe görünüm hiyerarşisi oluşturur. Hızlı oluşturucular ve uygun Düzen sınıflarda Düzen sıkıştırma etkinleştirme iç içe görünüm hiyerarşisi 70 görünümlerine %46 azaltma azaltır.

## <a name="summary"></a>Özet

Düzen sıkıştırma belirtilen düzenleri görsel ağaç'tan sayfa işleme performansını girişimi kaldırır. Bu teslim performans avantajı, bir sayfa, kullanılan işletim sistemi sürümü ve uygulamanın üzerinde çalıştığı aygıtın karmaşıklığına bağlı olarak değişir. Ancak, en büyük gelen performans artışı eski cihazlarda görülür.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Düzen Oluşturma](~/xamarin-forms/user-interface/layouts/custom.md)
- [Hızlı Oluşturucular](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
