---
title: "Platform özellikleri oluşturma"
description: "Satıcılar kendi platform özellikleri ile efektler oluşturabilirsiniz. Bir etkisi platforma özgü sonra gösterilen belirli işlevleri sağlar. Sonuç fluent kod API ve XAML aracılığıyla daha kolay kullanılabilecek bir etkili olur. Bu makalede, bir etkisi platforma özgü aracılığıyla kullanıma sunmak gösterilmiştir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 7cdc67f8ea1038226bb6ef8c8add8c03e9635e6a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-platform-specifics"></a>Platform özellikleri oluşturma

_Satıcılar kendi platform özellikleri ile efektler oluşturabilirsiniz. Bir etkisi platforma özgü sonra gösterilen belirli işlevleri sağlar. Sonuç fluent kod API ve XAML aracılığıyla daha kolay kullanılabilecek bir etkili olur. Bu makalede, bir etkisi platforma özgü aracılığıyla kullanıma sunmak gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Platforma özgü oluşturma işlemi aşağıdaki gibidir:

1. Belirli işlevleri efekt olarak uygular. Daha fazla bilgi için bkz: [bir efekt oluşturarak](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Etkisi açığa çıkarır bir platforma özgü sınıf oluşturun. Daha fazla bilgi için bkz: [platforma özgü sınıfı oluşturma](#creating).
1. Platforma özgü sınıfında XAML tüketilmesi platforma özel izin vermek için iliştirilmiş bir özellik uygulayın. Daha fazla bilgi için bkz: [bağlı bir özellik ekleme](#attached_property).
1. Platforma özgü sınıfında fluent bir kod API tüketilmesi platforma özel izin veren genişletme yöntemleri uygulayın. Daha fazla bilgi için bkz: [ekleme genişletme yöntemleri](#extension_methods).
1. Etkin uygulama, platforma özgü etkisi aynı platformunda çağrılmış etkisi yalnızca uygulanırsa şekilde değiştirin. Daha fazla bilgi için bkz: [etkisi oluşturma](#creating_the_effect).

Platforma özgü bir etkiye gösterme etkisi daha kolay fluent kod API ve XAML aracılığıyla tüketilebilir olduğunu sonucudur.

> [!NOTE]
> Satıcıları bu teknik kullanıcılar tarafından kullanım kolaylığı için kendi platform-özellikleri oluşturmak için kullanacağı envisaged. Kullanıcılar kendi platform özellikleri oluşturmak tercih edebilirsiniz, ancak oluşturma ve efekt tüketen daha fazla kod gerektirir unutulmamalıdır.

Örnek uygulamayı gösteren bir `Shadow` gölge tarafından görüntülenen metni ekler platforma özgü bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetimi:

![](creating-images/screenshots.png "Gölge platforma özgü")

Örnek uygulama uygulayan `Shadow` platforma özgü anlama kolaylığı için her bir platformda. Bununla birlikte, her platforma özgü geçerlilik uygulanması yanı sıra, gölge sınıfı uyarlamasını her platform için büyük ölçüde benzerdir. Bu nedenle, bu kılavuz gölge sınıfı ve tek bir platform üzerinde ilişkili etkisi uyarlamasını üzerinde odaklanılmaktadır.

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

Uygulaması aşağıdaki bölümlerde ele `Shadow` platforma özgü ve ilişkili etkisi.

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>Ekli özellik ekleme

Ekli özellik eklenmeli `Shadow` platforma özgü XAML aracılığıyla kullanımına izin vermek için:

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

`IsShadowed` Ekli özellik eklemek için kullanılan `MyCompany.LabelShadowEffect` için efekt ve denetiminden kaldırma, `Shadow` sınıfı bağlı. Bu özellik yazmaçlar bağlı `OnIsShadowedPropertyChanged` özelliğinin değeri değiştiğinde, yürütülecek yöntemi. Buna karşılık, bu yöntemi çağırır `AttachEffect` veya `DetachEffect` etkisi ekleme veya kaldırma için yöntemi değeri temel alarak `IsShadowed` özelliği eklenmiş. Etkisi eklenecek veya denetimin değiştirerek denetimden kaldırılan [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu.

> [!NOTE]
> Üzerindeki etkisi uygulama belirtilen benzersiz tanımlayıcı ve çözümleme grup adı birleşimi olan bir değer belirterek, etkisi çözümlendiğini unutmayın. Daha fazla bilgi için bkz: [bir efekt oluşturarak](~/xamarin-forms/app-fundamentals/effects/creating.md).

Ekli özellikler hakkında daha fazla bilgi için bkz: [ekli özellikler](~/xamarin-forms/xaml/attached-properties.md).

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>Genişletme yöntemleri ekleme

Genişletme yöntemleri eklenmelidir `Shadow` platforma özgü fluent kod API aracılığıyla kullanımına izin vermek için:

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

`IsShadowed` Ve `SetIsShadowed` genişletme yöntemleri get çağırma ve ayarlamak için erişimcileri `IsShadowed` özelliği, sırasıyla eklenmiş. Her bir genişletme yöntemi çalıştırır `IPlatformElementConfiguration<iOS, FormsElement>` platforma özgü üzerinde çağrılabilir belirtir türü [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) iOS örneklerden.

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Etkisi oluşturma

`Shadow` Platforma özgü ekler `MyCompany.LabelShadowEffect` için bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)ve kaldırır. Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulama iOS projesi için:

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

`UpdateShadow` Yöntemi kümeleri `Control.Layer` gölge oluşturmak için özellikler sağlanan `IsShadowed` ekli özellik ayarlanmış `true`ve koşuluyla `Shadow` platforma özgü, aynı platformda çağrılan, Etkili için uygulanır. Bu denetimi ile gerçekleştirilen `OnThisPlatform` yöntemi.

Varsa `Shadow.IsShadowed` bağlı özellik değeri değişiklikleri çalışma zamanında etkili gölge kaldırarak yanıt gerekiyor. Bu nedenle, geçersiz kılınan bir sürümünü `OnElementPropertyChanged` yöntemi çağrılarak bağlanabilirse özellik değişikliği yanıt için kullanılan `UpdateShadow` yöntemi.

Efekt oluşturma hakkında daha fazla bilgi için bkz: [bir efekt oluşturarak](~/xamarin-forms/app-fundamentals/effects/creating.md) ve [geçirme etkisi parametreler özellikleri bağlı olarak](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

## <a name="consuming-a-platform-specific"></a>Platforma özgü kullanma

`Shadow` Platforma özgü tüketilen XAML'de ayarlayarak `Shadow.IsShadowed` özelliğine bağlı bir `boolean` değeri:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Alternatif olarak, bu fluent API kullanarak C# tüketilebilir:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

Platform özellikleri kullanma hakkında daha fazla bilgi için bkz: [tüketen Platform özellikleri](~/xamarin-forms/platform/platform-specifics/consuming/index.md).

## <a name="summary"></a>Özet

Bu makalede bir etkisi platforma özgü aracılığıyla kullanıma sunmak nasıl gösterilmektedir. Sonuç fluent kod API ve XAML aracılığıyla daha kolay kullanılabilecek bir etkili olur.


## <a name="related-links"></a>İlgili bağlantılar

- [ShadowPlatformSpecific (örnek)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Denetimleri efektleri ile özelleştirme](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Ekli özellikler](~/xamarin-forms/xaml/attached-properties.md)
