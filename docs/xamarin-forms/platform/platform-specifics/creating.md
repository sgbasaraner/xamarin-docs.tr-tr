---
title: Platform özellikleri oluşturma
description: Bu makalede, bir etkisi platforma özgü aracılığıyla kullanıma sunmak gösterilmiştir.
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 1d9f07a089eabedf07bef49c9815fe7e93128f09
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997262"
---
# <a name="creating-platform-specifics"></a>Platform özellikleri oluşturma

_Satıcılar, kendi platform özellikleri efektleriyle oluşturabilirsiniz. Bir efekti ardından platforma özgü sunulan belirli işlevleri sağlar. Fluent API'si kod ve XAML aracılığıyla daha kolay kullanılabilecek efekt sonucudur. Bu makalede, bir etkisi platforma özgü aracılığıyla kullanıma sunmak gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Platforma özgü oluşturma işlemi aşağıdaki gibidir:

1. Bir efekti olarak belirli işlevlerini uygular. Daha fazla bilgi için [efekt oluşturma](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Etkisi harekete geçirecek bir platforma özgü sınıf oluşturun. Daha fazla bilgi için [platforma özgü sınıfı oluşturma](#creating).
1. Ekli özelliği XAML ile kullanılması platforma özel izin vermek için platforma özgü sınıfında uygulayın. Daha fazla bilgi için [bağlı bir özellik ekleme](#attached_property).
1. Platforma özgü sınıfında fluent kod API kullanılması platforma özel izin veren genişletme yöntemleri uygulayın. Daha fazla bilgi için [genişletme yöntemleri ekleme](#extension_methods).
1. Efekt uygulama ise etkisi olarak aynı platformunda platforma özgü çağırıldığında etkisi yalnızca uygulandığı şekilde değiştirin. Daha fazla bilgi için [etkisine](#creating_the_effect).

Platforma özgü olarak bir efekti gösterme etkisini daha kolay XAML ve kod fluent API'si aracılığıyla kullanılabileceği sonucudur.

> [!NOTE]
> Satıcılar kendi platform özellikleri, kullanıcılar tarafından kullanım kolaylığı için oluşturmak için bu tekniği kullanır envisaged. Kullanıcılar kendi platform özellikleri oluşturma seçebilirsiniz, ancak daha fazla kod oluşturma ve bir efekti tüketen gerektirdiğini unutulmamalıdır.

Örnek uygulamayı gösteren bir `Shadow` gölge tarafından görüntülenen metni ekler platforma özgü bir [ `Label` ](xref:Xamarin.Forms.Label) denetimi:

![](creating-images/screenshots.png "Platforma özgü gölge")

Örnek uygulama uygular `Shadow` platforma özgü her platformda anlama kolaylığı. Ancak, her platforma özel efekt uygulama yanı sıra gölge sınıfın uygulaması her platform için büyük ölçüde aynıdır. Bu nedenle, bu kılavuz, gölge sınıf ve tek bir platformda ilişkili efekti uygulama odaklanılmaktadır.

Etkileri hakkında daha fazla bilgi için bkz: [etkileri özelleştirme denetimleriyle](~/xamarin-forms/app-fundamentals/effects/index.md).

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>Platforma özgü sınıfı oluşturma

Platforma özgü olarak oluşturulan bir `public static` sınıfı:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

Aşağıdaki bölümlerde yürütmesinin `Shadow` platforma özgü ve ilişkili efekti.

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>Ekli özelliği ekleme

Ekli özelliği eklenmelidir `Shadow` platforma özgü için XAML aracılığıyla kullanılmasına izin ver:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

`IsShadowed` Ekli özellik eklemek için kullanılan `MyCompany.LabelShadowEffect` için efekt ve denetiminden kaldırma, `Shadow` sınıfı bağlı. İliştirilmiş özellik kayıtları `OnIsShadowedPropertyChanged` özelliğinin değeri değiştiğinde, yürütülecek yöntemi. Sırayla bu yöntemi çağırır `AttachEffect` veya `DetachEffect` etkisi ekleme veya kaldırma için yöntemi değerini temel alarak `IsShadowed` ekli özellik. Etkisi eklendiğinde veya denetimin değiştirerek denetiminden kaldırıldı [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu.

> [!NOTE]
> Etkisini çözümleme grubu adı ve etkili uygulama belirtilen benzersiz tanımlayıcı bir birleşimi olan bir değer belirterek giderilmiş olduğunu unutmayın. Daha fazla bilgi için [efekt oluşturma](~/xamarin-forms/app-fundamentals/effects/creating.md).

Ekli özellikler hakkında daha fazla bilgi için bkz. [iliştirilmiş özellikler](~/xamarin-forms/xaml/attached-properties.md).

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>Uzantı yöntemleri ekleme

İçin genişletme yöntemleri eklenmelidir `Shadow` platforma özgü API fluent kod aracılığıyla kullanılmasına izin ver için:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

`IsShadowed` Ve `SetIsShadowed` genişletme yöntemleri get çağırmak ve ayarlamak için erişimciler `IsShadowed` ekli özellik, sırasıyla. Her bir genişletme yöntemi çalıştığını `IPlatformElementConfiguration<iOS, FormsElement>` platforma özgü üzerinde çağrılabilir belirten türü [ `Label` ](xref:Xamarin.Forms.Label) iOS örneklerden.

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Efekti oluşturma

`Shadow` Platforma özgü ekler `MyCompany.LabelShadowEffect` için bir [ `Label` ](xref:Xamarin.Forms.Label)ve kaldırır. Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulaması iOS projesi için:

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`UpdateShadow` Yöntemi kümeleri `Control.Layer` gölge oluşturmak için özellikler sağlanan `IsShadowed` ekli özelliği `true`ve koşuluyla `Shadow` platforma özgü platformda çağırıldı, Efekt için uygulanır. Bu denetimi ile gerçekleştirilir `OnThisPlatform` yöntemi.

Varsa `Shadow.IsShadowed` bağlı çalışma zamanında, özellik değeri değişiklikleri etkili gölge kaldırarak yanıt gerekiyor. Bu nedenle, geçersiz kılınan bir sürümünü `OnElementPropertyChanged` yöntemi çağırarak bağlanılabilir özellik değişikliği yanıtlamak için kullanılır `UpdateShadow` yöntemi.

Bir efekti oluşturma hakkında daha fazla bilgi için bkz. [efekt oluşturma](~/xamarin-forms/app-fundamentals/effects/creating.md) ve [etkisi parametrelere geçirme iliştirilmiş özellikler](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

## <a name="consuming-a-platform-specific"></a>Platforma özgü kullanma

`Shadow` Platforma özgü ayarlayarak XAML içinde kullanılır `Shadow.IsShadowed` özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Alternatif olarak, bu fluent API'sini kullanarak C# tarafından kullanılabilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

Platform özelliklerini kullanma hakkında daha fazla bilgi için bkz. [Platform özelliklerini kullanma](~/xamarin-forms/platform/platform-specifics/consuming/index.md).

## <a name="summary"></a>Özet

Bu makalede bir etkisi platforma özgü aracılığıyla kullanıma sunmak nasıl gösterilmiştir. Fluent API'si kod ve XAML aracılığıyla daha kolay kullanılabilecek efekt sonucudur.


## <a name="related-links"></a>İlgili bağlantılar

- [ShadowPlatformSpecific (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Etkileri olan denetimleri özelleştirme](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Ekli Özellikler](~/xamarin-forms/xaml/attached-properties.md)
