---
title: "Düzenini sıkıştırma"
description: "Düzenini sıkıştırma belirtilen düzenleri sayfa işleme performansı girişimi görsel ağaç kaldırır. Bu makalede düzenini sıkıştırma ve kullanıma sunabilirsiniz avantajları etkinleştirme açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: 15f198465c544989b347fe534978956741478ed2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="layout-compression"></a>Düzenini sıkıştırma

_Düzenini sıkıştırma belirtilen düzenleri sayfa işleme performansı girişimi görsel ağaç kaldırır. Bu makalede düzenini sıkıştırma ve kullanıma sunabilirsiniz avantajları etkinleştirme açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms iki dizi özyinelemeli yöntemi çağrısı kullanarak Düzen gerçekleştirir:

- Bir sayfayla görsel ağaç üstündeki düzeni başlar ve her bir sayfasında görsel öğe kapsayacak şekilde visual ağacının tüm dalları aracılığıyla devam eder. Diğer öğeler için üst öğeleri boyutlandırma ve bunların alt öğeleri kendilerini göreli konumlandırma sorumludur.
- Geçersiz kılma olarak öğe bir sayfa üzerinde değişiklik yeni düzen döngüsü tetikler işlemidir. Artık doğru boyutunu veya konumunu sahip olduğunuzda öğeleri geçersiz olarak kabul edilir. Alt öğelerinden boyutları değiştiğinde her öğenin alt öğesi yok görsel ağaç uyarı. Bu nedenle, bir değişiklik görsel ağaç bir öğenin boyutunu ağaca ripple değişiklikleri neden olabilir.

Xamarin.Forms düzeni nasıl gerçekleştireceğini hakkında daha fazla bilgi için bkz: [özel düzen oluşturma](~/xamarin-forms/user-interface/layouts/custom.md).

Düzen işlemine yerel denetimlere hiyerarşisini sonucudur. Ancak, bu hiyerarşideki ek kapsayıcı oluşturucu ve sarmalayıcıları daha fazla iç içe geçme hiyerarşisini görüntüleme inflating platform Oluşturucu için içerir. Daha derin iç içe geçme düzeyi, bir sayfasını görüntülemek için gerçekleştirmek için Xamarin.Forms sahip iş büyük tutar. Karmaşık düzenleri için Görünüm hiyerarşisine derin ve birden çok iç içe geçme düzeyi ile geniş olabilir.

Örneğin, aşağıdaki örnek uygulama için Facebook içine günlük düğmesinden göz önünde bulundurun:

![](layout-compression-images/facebook-button.png "Facebook Button")

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

Sonuçta elde edilen hiyerarşisini iç içe geçmiş görüntüleme ile incelenebilir [Xamarin denetçisi](~/tools/inspector/index.md). Android, iç içe geçmiş görünüm hiyerarşisine 17 görünümleri içerir:

![](layout-compression-images/no-compression.png "Facebook düğmesi için hiyerarşisini görüntüleme")

İOS ve Android platformları üzerinde Xamarin.Forms uygulamalar için kullanılabilir olan düzenini sıkıştırma görünüm sayfa işleme performansını visual ağacından belirtilen düzenleri kaldırarak iç içe geçme düzleştirmek amaçlar. Teslim performans avantajı bir sayfa, kullanılan işletim sistemi sürümü ve uygulamanın çalıştığı aygıt karmaşıklığına bağlı olarak değişir. Ancak, büyük performans artışı eski cihazlarda görülür.

> [!NOTE]
> **Not**: Bu makale düzenini sıkıştırma Android uygulama sonuçlarını odaklanır olsa da, iOS için eşit oranda geçerlidir.

## <a name="layout-compression"></a>Düzenini sıkıştırma

XAML'de ayarlayarak düzenini sıkıştırma etkinleştirilebilir `CompressedLayout.IsHeadless` özelliğine bağlı `true` bir düzen sınıfını:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Alternatif olarak, C# ' ilk bağımsız değişkeni olarak düzeni örneği belirterek etkinleştirilebilir `CompressedLayout.SetIsHeadless` yöntemi:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Görsel ağaç bir düzen düzenini sıkıştırma kaldırır olduğundan, bir görsel görünümünü sahip veya dokunmatik giriş elde düzenleri için uygun değil. Bu nedenle, düzenleri kümesi [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) özellikleri (gibi [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/), [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/), [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) ve [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)) veya hareketleri kabul, yerleşim için aday değildir Sıkıştırma. Ancak, visual görünüm özelliklerini ayarlayan veya hareketleri, kabul eden bir düzende düzenini sıkıştırma etkinleştirme bir yapı veya çalışma zamanı hatası oluşturmayacaktır. Bunun yerine, düzenini sıkıştırma uygulanır ve görsel görünümünü özellikleri ve hareketi tanıma sessizce başarısız olur.

Facebook düğmesi için üç düzeni sınıflarında düzenini sıkıştırma etkinleştirilebilir:

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

Android, bu 14 görünümlerinin iç içe geçmiş görünümünü hiyerarşisinde sonuçlanır:

![](layout-compression-images/layout-compression.png "Facebook düğmesi düzenini sıkıştırma ile hiyerarşisini görüntüleme")

17 görünümleri özgün iç içe geçmiş görünümünü hiyerarşi ile karşılaştırıldığında, bu görünüm %17 sayısını azalmasına temsil eder. Bu azaltma anlamsız görünebilir, ancak bir sayfanın tamamını üzerinden görünümü azaltma daha önemli olabilir.

### <a name="fast-renderers"></a>Hızlı Oluşturucu

Hızlı Oluşturucu, sonuçta elde edilen hiyerarşisini yerel görüntüleme düzleştirme tarafından Enflasyon ve Android Xamarin.Forms denetimlere işleme maliyetlerini azaltabilirsiniz. Bu daha fazla performans daha az nesne oluşturarak hangi sırayla sonuçları en az karmaşık bir görsel ağaç ve daha az bellek kullanımını artırır. Hızlı Oluşturucu hakkında daha fazla bilgi için bkz: [hızlı Oluşturucu](~/xamarin-forms/internals/fast-renderers.md).

Facebook düğmesini için örnek uygulama düzenini sıkıştırma ve hızlı Oluşturucu birleştirme 8 görünümler iç içe geçmiş görünümünü hiyerarşisini üretir:

![](layout-compression-images/layout-compression-with-fast-renderers.png "Facebook düğmesi düzenini sıkıştırma ve hızlı Oluşturucu ile hiyerarşisini görüntüleme")

17 görünümleri özgün iç içe geçmiş görünümünü hiyerarşi ile karşılaştırıldığında, bu azaltma %52 temsil eder.

Örnek uygulamayı gerçek bir uygulamadan ayıklanan bir sayfa içerir. Düzenini sıkıştırma ve hızlı Oluşturucu sayfa Android 130 görünümleri iç içe geçmiş görünümünü hiyerarşisi oluşturur. Hızlı oluşturucu ve uygun düzeni sınıfları düzenini sıkıştırma etkinleştirme iç içe geçmiş görünüm hiyerarşisine 70 görünümlerine azaltma %46 azaltır.

## <a name="summary"></a>Özet

Düzenini sıkıştırma belirtilen düzenleri sayfa işleme performansı girişimi görsel ağaç kaldırır. Bu teslim performans avantajı bir sayfa, kullanılan işletim sistemi sürümü ve uygulamanın çalıştığı aygıt karmaşıklığına bağlı olarak değişir. Ancak, büyük performans artışı eski cihazlarda görülür.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel bir düzen oluşturma](~/xamarin-forms/user-interface/layouts/custom.md)
- [Hızlı Oluşturucu](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
