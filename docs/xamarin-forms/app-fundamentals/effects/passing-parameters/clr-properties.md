---
title: Ortak dil çalışma zamanı özellikleri olarak etkili parametreleri geçirme
description: Ortak dil çalışma zamanı (CLR) özellikleri, çalışma zamanı özellik değişikliklerine yanıt yok etkisi parametrelerini tanımlamak için kullanılabilir. Bu makalede, efekt parametreleri geçirmek için CLR özelliklerini kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 1bb357b256a7cc6d52d1d92613f38cbf48400c4c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995774"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Ortak dil çalışma zamanı özellikleri olarak etkili parametreleri geçirme

_Ortak dil çalışma zamanı (CLR) özellikleri, çalışma zamanı özellik değişikliklerine yanıt yok etkisi parametrelerini tanımlamak için kullanılabilir. Bu makalede, efekt parametreleri geçirmek için CLR özelliklerini kullanmayı gösterir._

Çalışma zamanı özellik değişikliklerine yanıt yok efekt parametreleri oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir `public` alt sınıflara ayıran sınıfı [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) sınıfı. `RoutingEffect` Sınıf genellikle platforma özel efekt iç sarmalayan bir platformdan bağımsız etkisi temsil eder.
1. Çözüm grubu adını ve her platforma özel efekt sınıfı belirtilen benzersiz kimliği bir birleştirme öğesinde geçen temel sınıf oluşturucusunu çağırır, bir oluşturucu oluşturun.
1. Özellikleri efekti geçirilecek her parametre için bir sınıf ekleyin.

Parametreler, efekte etkisi örneklerken her bir özellik için değerleri belirtilerek ardından geçirilebilir.

Örnek uygulamayı gösteren bir `ShadowEffect` tarafından görüntülenen metni gölge ekleyen bir [ `Label` ](xref:Xamarin.Forms.Label) denetimi. Örnek uygulamada, onlar arasındaki ilişkileri yanı sıra her bir proje sorumluluklarını Aşağıdaki diyagramda gösterilmektedir:

![](clr-properties-images/shadow-effect.png "Gölge efektini Proje Sorumlulukları")

A [ `Label` ](xref:Xamarin.Forms.Label) denetimi `HomePage` göre özelleştirilmiş `LabelShadowEffect` her platforma özgü projede. Parametreleri geçirilir her `LabelShadowEffect` özelliklerinde aracılığıyla `ShadowEffect` sınıfı. Her `LabelShadowEffect` sınıf türetilir `PlatformEffect` her platform için sınıf. Tarafından görüntülenen metni eklenen bir gölge sonuçlanır `Label` aşağıdaki ekran görüntülerinde gösterildiği denetimi:

![](clr-properties-images/screenshots.png "Her platformda gölge etkisi")

## <a name="creating-effect-parameters"></a>Efekt parametreleri oluşturma

A `public` alt sınıflara ayıran sınıfı [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) sınıfı oluşturulması efekt parametreleri göstermek için aşağıdaki kod örneğinde gösterildiği gibi:

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

`ShadowEffect` Her platforma özel geçirilecek parametreler temsil eden dört özellikleri içeren `LabelShadowEffect`. Sınıf oluşturucu çözümleme grup adı ve her platforma özel efekt sınıfı belirtilen benzersiz kimliği bir birleşimini içeren bir parametre olarak geçirerek, bir temel sınıf oluşturucusunu çağırır. Bu nedenle, yeni bir örneğini `MyCompany.LabelShadowEffect` bir denetimin eklenecek [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu olduğunda bir `ShadowEffect` oluşturulur.

## <a name="consuming-the-effect"></a>Etkisi kullanma

Aşağıdaki XAML kod örnekte gösterildiği bir [ `Label` ](xref:Xamarin.Forms.Label) denetim hangi `ShadowEffect` eklenir:

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

Eşdeğer [ `Label` ](xref:Xamarin.Forms.Label) C# ' de aşağıdaki kod örneğinde gösterilir:

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

Her iki kod örneklerinde örneği `ShadowEffect` denetimin eklenmeden önce her bir özellik için belirtilen değerlerle sınıfının örneği [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu. Unutmayın `ShadowEffect.Color` özelliği, platforma özel renk değerleri kullanır. Daha fazla bilgi için [cihaz sınıfı](~/xamarin-forms/platform/device.md).

## <a name="creating-the-effect-on-each-platform"></a>Her platformda efekti oluşturma

Aşağıdaki bölümlerde platforma özel uygulanışı `LabelShadowEffect` sınıfı.

### <a name="ios-project"></a>iOS projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulaması iOS projesi için:

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

`OnAttached` Yöntemi alır `ShadowEffect` örneği ve kümeleri `Control.Layer` gölge oluşturmak için belirtilen özellik değerleri özellikleri. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi bağlı denetim sahip değil durumunda block `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

### <a name="android-project"></a>Android projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulaması Android projesi için:

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

`OnAttached` Yöntemi alır `ShadowEffect` çağrıları ve örneğini [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) belirtilen özellik değerlerini kullanarak bir gölge oluşturmak için yöntemi. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi bağlı denetim sahip değil durumunda block `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

### <a name="universal-windows-platform-project"></a>Evrensel Windows platformu projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` Evrensel Windows Platformu (UWP) proje için uygulama:

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

Evrensel Windows platformu, bir gölge etkisi sağlamaz ve böylece `LabelShadowEffect` hem de platformlarda uygulama ikinci bir uzaklık ekleyerek bir benzetim [ `Label` ](xref:Xamarin.Forms.Label) birincil arkasında `Label`. `OnAttached` Yöntemi alır `ShadowEffect` örnek, yeni oluşturur `Label`ve üzerinde bazı düzen özellikleri ayarlar `Label`. Ardından ayarlayarak bir gölge oluşturur [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), ve [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) konumuverengidenetlemekiçinÖzellikler`Label`. `shadowLabel` Daha sonra eklenen birincil kaydırma `Label`. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi bağlı denetim sahip değil durumunda block `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

## <a name="summary"></a>Özet

Bu makalede, efekt parametreleri geçirmek için CLR özelliklerini kullanarak gösterilmiştir. CLR özellikleri, çalışma zamanı özellik değişikliklerine yanıt yok etkisi parametrelerini tanımlamak için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Etkin](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Gölge efektini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
