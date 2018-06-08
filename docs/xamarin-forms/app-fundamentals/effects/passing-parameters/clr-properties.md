---
title: Ortak dil çalışma zamanı özellikleri olarak etkisi parametreleri geçirme
description: Ortak dil çalışma zamanı (CLR) özellikleri, çalışma zamanı özelliği değişikliklere yanıt verme etkisi parametrelerini tanımlamak için kullanılabilir. Bu makalede, efekt parametreleri geçirmek için CLR özelliklerini kullanarak gösterilmektedir.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: c85256803da137c850e502cfad917de703b161b5
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846480"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Ortak dil çalışma zamanı özellikleri olarak etkisi parametreleri geçirme

_Ortak dil çalışma zamanı (CLR) özellikleri, çalışma zamanı özelliği değişikliklere yanıt verme etkisi parametrelerini tanımlamak için kullanılabilir. Bu makalede, efekt parametreleri geçirmek için CLR özelliklerini kullanarak gösterilmektedir._

Çalışma zamanı özelliği değişikliklere yanıt verme efekt parametreleri oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir `public` alt sınıf [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) sınıfı. `RoutingEffect` Sınıfı, genellikle platforma özgü bir iç efekt sarmalar platformdan bağımsız etkisi temsil eder.
1. Çözümleme grup adı ve her platforma özgü etkisi sınıf içinde belirtilmiş benzersiz kimliği birleşimini geçirme temel sınıf oluşturucu çağıran bir oluşturucu oluşturun.
1. Özellikler efekti geçirilecek her bir parametreye sınıfına ekleyin.

Parametreleri, her bir özellik için değerleri etkisi başlatılırken belirterek efekti sonra geçirilebilir.

Örnek uygulamayı gösteren bir `ShadowEffect` tarafından görüntülenen metni gölge ekleyen bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetim. Aşağıdaki diyagram, her proje örnek uygulamasında, aralarındaki ilişkilerin birlikte sorumlulukları gösterir:

![](clr-properties-images/shadow-effect.png "Gölge etkisi Proje Sorumlulukları")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetiminin `HomePage` tarafından özelleştirilmiş `LabelShadowEffect` her platforma özgü projesinde. Parametreleri geçirilir kadar her `LabelShadowEffect` özelliklerinde aracılığıyla `ShadowEffect` sınıfı. Her `LabelShadowEffect` sınıfı türer `PlatformEffect` her platform için sınıf. Bu tarafından görüntülenen metni eklenmesini gölge sonuçlanır `Label` denetlemek, aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](clr-properties-images/screenshots.png "Her platformda gölge efekti")

## <a name="creating-effect-parameters"></a>Efekt parametreleri oluşturma

A `public` alt sınıf [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) sınıfı oluşturulmalıdır efekt parametreleri temsil etmek için aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public class ShadowEffect : RoutingEffect
{
  public float Radius { get; set; }

  public Color Color { get; set; }

  public float DistanceX { get; set; }

  public float DistanceY { get; set; }

  public ShadowEffect () : base ("MyCompany.LabelShadowEffect")
  {            
  }
}
```

`ShadowEffect` Her platforma özel geçirilecek parametreleri temsil eden dört özelliklerinin içeren `LabelShadowEffect`. Sınıf oluşturucu çözümleme grup adı ve her platforma özgü etkisi sınıf içinde belirtilmiş benzersiz kimliği birleşimini oluşan bir parametre olarak geçirme temel sınıf oluşturucu çağırır. Bu nedenle, yeni bir örneğini `MyCompany.LabelShadowEffect` bir denetimin eklenecek [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu olduğunda bir `ShadowEffect` örneği.

## <a name="consuming-the-effect"></a>Etkisi kullanma

Aşağıdaki XAML kodu örnekte gösterildiği bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) hangi denetimine `ShadowEffect` eklenir:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Effects>
    <local:ShadowEffect Radius="5" DistanceX="5" DistanceY="5">
      <local:ShadowEffect.Color>
        <OnPlatform x:TypeArguments="Color">
            <On Platform="iOS" Value="Black" />
            <On Platform="Android" Value="White" />
            <On Platform="UWP" Value="Red" />
        </OnPlatform>
      </local:ShadowEffect.Color>
    </local:ShadowEffect>
  </Label.Effects>
</Label>
```

Eşdeğer [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) C# ' de aşağıdaki kod örneğinde gösterilir:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};

Color color = Color.Default;
switch (Device.RuntimePlatform)
{
    case Device.iOS:
        color = Color.Black;
        break;
    case Device.Android:
        color = Color.White;
        break;
    case Device.UWP:
        color = Color.Red;
        break;
}

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

Her iki kod örnekleri, bir örneği olarak `ShadowEffect` sınıfının örneği denetimin eklenmeden önce her bir özellik için belirtilen değerlerle [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu. Unutmayın `ShadowEffect.Color` özelliği platforma özgü renk değerleri kullanır. Daha fazla bilgi için bkz: [aygıt sınıfı](~/xamarin-forms/platform/device.md).

## <a name="creating-the-effect-on-each-platform"></a>Her platformun üzerindeki etkisi oluşturma

Aşağıdaki bölümlerde platforma özgü uygulanması açıklanmaktadır `LabelShadowEffect` sınıfı.

### <a name="ios-project"></a>iOS projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulama iOS projesi için:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    Control.Layer.CornerRadius = effect.Radius;
                    Control.Layer.ShadowColor = effect.Color.ToCGColor ();
                    Control.Layer.ShadowOffset = new CGSize (effect.DistanceX, effect.DistanceY);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` Yöntemi alır `ShadowEffect` örneği ve ayarlar `Control.Layer` gölge oluşturmak için belirtilen özellik değerlerini özellikleri. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi bağlı denetimi yoktur engelleme `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

### <a name="android-project"></a>Android projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulama Android projesi için:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var control = Control as Android.Widget.TextView;
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    float radius = effect.Radius;
                    float distanceX = effect.DistanceX;
                    float distanceY = effect.DistanceY;
                    Android.Graphics.Color color = effect.Color.ToAndroid ();
                    control.SetShadowLayer (radius, distanceX, distanceY, color);
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` Yöntemi alır `ShadowEffect` örneği ve çağrıları [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) yöntemi belirtilen özellik değerleri kullanılarak bir gölge oluşturmak için. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi bağlı denetimi yoktur engelleme `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

### <a name="universal-windows-platform-project"></a>Evrensel Windows platformu projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulama Evrensel Windows Platformu (UWP) proje için:

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                    if (effect != null) {
                        var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;
                        var shadowLabel = new Label ();
                        shadowLabel.Text = textBlock.Text;
                        shadowLabel.FontAttributes = FontAttributes.Bold;
                        shadowLabel.HorizontalOptions = LayoutOptions.Center;
                        shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;
                        shadowLabel.TextColor = effect.Color;
                        shadowLabel.TranslationX = effect.DistanceX;
                        shadowLabel.TranslationY = effect.DistanceY;

                        ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                        shadowAdded = true;
                    }
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Evrensel Windows platformu gölge efekti sağlamaz ve böylece `LabelShadowEffect` uygulaması hem platformlarda benzetim bir ikinci bir uzaklık ekleyerek [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) birincil arkasında `Label`. `OnAttached` Yöntemi alır `ShadowEffect` örneği, yeni oluşturur `Label`ve üzerinde bazı yerleşim özellikleri ayarlar `Label`. Ayarlayarak sonra gölge oluşturur [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), ve [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) özelliklerini konumunuverenkdenetlemekiçin`Label`. `shadowLabel` Daha sonra eklenen birincil uzaklığı `Label`. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi bağlı denetimi yoktur engelleme `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

## <a name="summary"></a>Özet

Bu makalede, efekt parametreleri geçirmek için CLR özelliklerini kullanarak gösterilmiştir. CLR özellikleri, çalışma zamanı özelliği değişikliklere yanıt verme etkisi parametrelerini tanımlamak için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Etkisi](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Gölge efekti (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
