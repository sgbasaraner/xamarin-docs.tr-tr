---
title: Xamarin.Forms Denetim şablonları giriş
description: Xamarin.Forms Denetim şablonları, çalışma zamanında uygulama sayfaları kolayca teması ve re-theme olanağı sağlar. Bu makalede, denetim şablonları tanıtılmaktadır.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6b7a6c6d9c9c541e1d5e821fc2dac202e98bec62
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994431"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Xamarin.Forms Denetim şablonları giriş

_Xamarin.Forms Denetim şablonları, çalışma zamanında uygulama sayfaları kolayca teması ve re-theme olanağı sağlar. Bu makalede, denetim şablonları tanıtılmaktadır._

Denetimleri farklı özellikleri gibi sahip `BackgroundColor` ve `TextColor`, denetimin görünümünü yönlerini tanımlayabilirsiniz. Bu özellikler kullanılarak ayarlanabilir [stilleri](~/xamarin-forms/user-interface/styles/index.md), hangi değiştirilebilir temel Tema oluşturma uygulamak için çalışma zamanında. Ancak, stilleri bir sayfa görünümünü ve içeriğini arasında NET bir ayrım sağlamak yoktur ve gibi özellikleri ayarlayarak yaptığınız değişiklikleri sınırlıdır.

Denetim şablonları, bir sayfa görünümünü ve bu nedenle sayfalarına temalı kolayca olabilir oluşturulmasını etkinleştirme içeriğini arasında NET bir ayrım sağlar. Örneğin, bir uygulama, koyu tema ve hafif bir tema sağlayan uygulama düzeyi denetim şablonları içerebilir. Her [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) uygulamada her sayfa tarafından görüntülenen içeriğini değiştirmeden denetim şablonlardan birini uygulayarak temalı olabilir. Ayrıca, denetim şablonları tarafından sağlanmıştır Temalar denetimlerin özelliklerini değiştirme ile sınırlı değildir. Bunlar ayrıca temayı uygulamak için kullanılan denetimler de değiştirebilirsiniz.

## <a name="creating-a-controltemplate"></a>ControlTemplate oluşturarak

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) bir sayfa ya da Görünüm görünümünü belirtir ve bir kök Düzen içerir ve düzeni, şablonu uygulayan denetimleri içinde. Genellikle, bir `ControlTemplate` yararlanacaktır bir [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) görünümü ve sayfa tarafından görüntülenen içeriğin nerede görüneceğini işaretlenecek. Sayfa veya tüketen Görünüm `ControlTemplate` tarafından görüntülenecek içeriği ardından tanımlayacak `ContentPresenter`. Aşağıdaki diyagramda gösterilmektedir bir `ControlTemplate` denetimleri de dahil olmak üzere, bir dizi içeren bir sayfa için bir `ContentPresenter` mavi bir dikdörtgen ile işaretlenmiş:

![](introduction-images/control-template.png "Denetim şablonu için bir sayfa")

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) ayarlayarak aşağıdaki türlerine uygulanabilir kendi `ControlTemplate` özellikleri:

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

Olduğunda bir [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) oluşturulur ve atanır bu tür için tanımlanmış görünüm ile varolan bir görünümü değiştirilir `ControlTemplate`. Ayrıca, yanı sıra görünümünü kullanarak autorefresh ayarlamayı `ControlTemplate` özelliği, denetim şablonları da uygulanabilir daha da fazla stilleri kullanarak tema özelliğini genişletin.

> [!NOTE]
>  *Hangi `TemplatedPage` ve `TemplatedView` türleri?* `TemplatedPage` temel sınıfı olan `ContentPage`ve Xamarin.Forms tarafından sağlanan en temel sayfa türü. Farklı `ContentPage`, `TemplatedPage` sahip olmadığı bir `Content` özelliği. Bu nedenle, içeriği doğrudan eklenemez bir `TemplatedPage` örneği. Bunun yerine, içerik için denetim şablonu ayarlayarak eklenir `TemplatedPage` örneği. Benzer şekilde, `TemplatedView` temel sınıfı olan `ContentView`. Farklı `ContentView`, `TemplatedView` sahip olmadığı bir `Content` özelliği. Bu nedenle, içeriği doğrudan eklenemez bir `TemplatedView` örneği. Bunun yerine, içerik için denetim şablonu ayarlayarak eklenir `TemplatedView` örneği.

Denetim şablonları, XAML ve C# ' ta oluşturulabilir:

- Denetim şablonları oluşturulan XAML içinde tanımlanmış bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) için atanan [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) koleksiyonu bir sayfa ya da daha genellikle [ `Resources` ](xref:Xamarin.Forms.Application.Resources) uygulama koleksiyonu.
- C# içinde oluşturulan denetim şablonları genellikle sayfanın sınıfı ya da genel olarak erişilebilen sınıfı olarak tanımlanır.

Tanımlama yeri seçme bir [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) örneği burada kullanılabileceği etkiler:

- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) sayfa düzeyinde tanımlanan örnekleri yalnızca sayfaya uygulanabilir.
- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) uygulama düzeyinde tanımlanan örnek uygulama boyunca sayfalara uygulanabilir.

Denetim şablonları görünümü hiyerarşide daha düşük yukarı yüksek tanımlanmış önceliklidir. Örneğin, bir [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) adlı `DarkTheme` konumunda tanımlanan sayfa düzeyi uygulama düzeyinde tanımlanan aynı adlı bir şablonu önceliklidir. Bu nedenle, bir uygulamada her sayfa için uygulanacak bir temayı tanımlayan bir denetim şablonu uygulama düzeyinde tanımlanması gerekir.


## <a name="related-links"></a>İlgili bağlantılar

- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
