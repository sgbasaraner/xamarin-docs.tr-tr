---
title: "Giriş"
description: "Xamarin.Forms Denetim şablonları, çalışma zamanında uygulama sayfaları kolayca tema ve yeniden tema yeteneği sağlar. Bu makalede denetim şablonları tanıtılmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: c3973b94168706e047ee5c312a8823503c9b0fd8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction"></a>Giriş

_Xamarin.Forms Denetim şablonları, çalışma zamanında uygulama sayfaları kolayca tema ve yeniden tema yeteneği sağlar. Bu makalede denetim şablonları tanıtılmaktadır._

Denetimleriniz farklı özellikler gibi `BackgroundColor` ve `TextColor`, denetimin görünümünü yönlerini tanımlayabilirsiniz. Bu özellikleri kullanılarak ayarlanabilir [stilleri](~/xamarin-forms/user-interface/styles/index.md), hangi değiştirilebilir temel tema uygulamak için çalışma zamanında. Ancak, stil sayfasının görünümünü ve içeriği arasındaki temiz bir birbirinden ayırmaya yok ve gibi özellikleri ayarlayarak yapılan değişiklikleri sınırlıdır.

Denetim şablonları, bir sayfa görünümünü ve bu nedenle, kolayca konulu edilebilecek sayfaları oluşturulmasını etkinleştirme içeriğini arasında temiz bir ayrım sağlar. Örneğin, bir uygulama koyu tema ve açık bir tema sağlayan uygulama düzeyi denetim şablonları içerebilir. Her [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) her sayfası tarafından görüntülenmekte olan içeriğin değiştirmeden denetim şablonlarından birini uygulayarak uygulamada konulu olabilir. Buna ek olarak, denetim şablonları tarafından sağlanan temaları denetimlerin özelliklerini değiştirmek için sınırlı değildir. Bunlar temayı uygulamak için kullanılan denetimler de değiştirebilirsiniz.

## <a name="creating-a-controltemplate"></a>ControlTemplate oluşturma

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) sayfa veya Görünüm görünümünü belirtir ve bir kök düzeni içerir ve düzeni, şablonu uygulayan denetimleri içinde. Genellikle, bir `ControlTemplate` yararlanacak olan bir [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) sayfa veya görünüm tarafından görüntülenen içeriğin nerede görüneceğini işaretlenecek. Sayfa veya tüketir Görünüm `ControlTemplate` tarafından görüntülenecek içerik ardından tanımlayacaksınız `ContentPresenter`. Aşağıdaki diyagramda gösterilmektedir bir `ControlTemplate` denetimler de dahil olmak üzere, çeşitli içeren bir sayfa için bir `ContentPresenter` mavi dikdörtgeni tarafından işaretlenen:

![](introduction-images/control-template.png "Bir sayfa için denetim şablonu")

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) ayarlayarak aşağıdaki türlerine uygulanabilir kendi `ControlTemplate` özellikleri:

- [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)
- [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)
- [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)

Zaman bir [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) oluşturulan ve atanan bu tür için tanımlanmış görünüm ile varolan bir görünümü değiştirilir `ControlTemplate`. Ayrıca, yanı sıra görünümünü kullanarak ayarlamayı `ControlTemplate` özelliği, şablonları da uygulanabilir daha da fazla stilleri kullanarak denetim tema özelliği'ni genişletin.

> [!NOTE]
>  *Hangi `TemplatedPage` ve `TemplatedView` türleri?* `TemplatedPage` temel sınıfı olan `ContentPage`ve Xamarin.Forms tarafından sağlanan en temel sayfa türü. Farklı `ContentPage`, `TemplatedPage` sahip olmayan bir `Content` özelliği. Bu nedenle, içeriği doğrudan eklenemiyor bir `TemplatedPage` örneği. Bunun yerine, içerik için denetim şablonu ayarlayarak eklenir `TemplatedPage` örneği. Benzer şekilde, `TemplatedView` temel sınıfı olan `ContentView`. Farklı `ContentView`, `TemplatedView` sahip olmayan bir `Content` özelliği. Bu nedenle, içeriği doğrudan eklenemiyor bir `TemplatedView` örneği. Bunun yerine, içerik için denetim şablonu ayarlayarak eklenir `TemplatedView` örneği.

Denetim şablonları XAML ve C# ' ta oluşturulabilir:

- XAML'de oluşturulan denetim şablonları tanımlanmış bir [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) için atanan [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) koleksiyonu sayfasının veya daha fazla genellikle için [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) uygulama koleksiyonu.
- C# ' de oluşturulan denetim şablonları genellikle sayfanın sınıf veya genel olarak erişilebilir bir sınıf içinde tanımlanır.

Nerede tanımlamalı seçerek bir [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) örneği burada kullanılabilir etkiler:

- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) sayfa düzeyinde tanımlanan örnekleri yalnızca sayfaya uygulanabilir.
- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) uygulama düzeyinde tanımlanan örnek uygulama boyunca sayfalara uygulanabilir.

Denetim şablonları görünüm hiyerarşinin alt düzeylerindeki yukarı yüksek tanımlanan önceliklidir. Örneğin, bir [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) adlı `DarkTheme` konumunda tanımlanan sayfa düzeyinde uygulama düzeyinde tanımlanan aynı adlı bir şablon önceliğe sahip olur. Bu nedenle, her bir uygulama sayfasını uygulanacak temayı tanımlayan bir denetim şablonu uygulama düzeyinde tanımlanmalıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [Stilleri](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
