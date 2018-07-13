---
title: Ekli özellikler olarak etkili parametreleri geçirme
description: Ekli özellikler, çalışma zamanı özellik değişikliklerine yanıt etkisi parametrelerini tanımlamak için kullanılabilir. Bu makalede, iliştirilmiş özellikler efekt ve çalışma zamanında bir parametre değiştirme parametreleri geçirmek için kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 9483e424a74a88ce3f0eb49624bb5315551f2062
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996457"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Ekli özellikler olarak etkili parametreleri geçirme

_Ekli özellikler, çalışma zamanı özellik değişikliklerine yanıt etkisi parametrelerini tanımlamak için kullanılabilir. Bu makalede, iliştirilmiş özellikler efekt ve çalışma zamanında bir parametre değiştirme parametreleri geçirmek için kullanma gösterilmektedir._

Çalışma zamanı özellik değişikliklerine yanıt efekt parametreleri oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir `static` efekti geçirilecek her parametre için ekli özelliği içeren sınıf.
1. Ek ekli özelliği eklenmesi veya sınıf iliştirilmiş denetim efekti kaldırılmasını denetlemek için kullanılan bir sınıf ekleyin. Bu özellik kayıtları bağlı emin bir `propertyChanged` özelliğinin değeri değiştiğinde çalıştırılacak temsilci.
1. Oluşturma `static` alıcılar ve ayarlayıcılar her için bağlı özelliği.
1. Uygulama mantığı `propertyChanged` eklemek ve etkisi kaldırmak için temsilci.
1. İç içe geçmiş bir sınıf içinde uygulama `static` etkisi sonra hangi alt sınıflar adlandırılan sınıfı `RoutingEffect` sınıfı. Oluşturucu için çözüm grubu adını ve her platforma özel efekt sınıfı belirtilen benzersiz kimliği bir birleştirme öğesinde geçen temel sınıf oluşturucusunu çağırın.

Parametreler, efekte iliştirilmiş özellikler ve özellik değerleri için uygun denetimi ekleyerek ardından geçirilebilir. Ayrıca, yeni bir ekli özellik değeri belirterek parametreleri, çalışma zamanında'e değiştirilebilir.

> [!NOTE]
> Ekli özelliği bağlanılabilir özellik, ancak diğer nesnelere bağlı ve XAML içinde tanınan bir sınıftaki bir sınıf ya da bir nokta ile ayrılmış bir özellik adını içeren öznitelikler tanımlanan özel bir türüdür. Daha fazla bilgi için [iliştirilmiş özellikler](~/xamarin-forms/xaml/attached-properties.md).

Örnek uygulamayı gösteren bir `ShadowEffect` tarafından görüntülenen metni gölge ekleyen bir [ `Label` ](xref:Xamarin.Forms.Label) denetimi. Ayrıca, çalışma zamanında gölge rengi değiştirilebilir. Örnek uygulamada, onlar arasındaki ilişkileri yanı sıra her bir proje sorumluluklarını Aşağıdaki diyagramda gösterilmektedir:

![](attached-properties-images/shadow-effect.png "Gölge efektini Proje Sorumlulukları")

A [ `Label` ](xref:Xamarin.Forms.Label) denetimi `HomePage` göre özelleştirilmiş `LabelShadowEffect` her platforma özgü projede. Parametreleri geçirilir her `LabelShadowEffect` aracılığıyla özelliklerinde bağlı `ShadowEffect` sınıfı. Her `LabelShadowEffect` sınıf türetilir `PlatformEffect` her platform için sınıf. Tarafından görüntülenen metni eklenen bir gölge sonuçlanır `Label` aşağıdaki ekran görüntülerinde gösterildiği denetimi:

![](attached-properties-images/screenshots.png "Her platformda gölge etkisi")

## <a name="creating-effect-parameters"></a>Efekt parametreleri oluşturma

A `static` sınıfı oluşturulması efekt parametreleri göstermek için aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public static class ShadowEffect
{
  public static readonly BindableProperty HasShadowProperty =
    BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false, propertyChanged: OnHasShadowChanged);
  public static readonly BindableProperty ColorProperty =
    BindableProperty.CreateAttached ("Color", typeof(Color), typeof(ShadowEffect), Color.Default);
  public static readonly BindableProperty RadiusProperty =
    BindableProperty.CreateAttached ("Radius", typeof(double), typeof(ShadowEffect), 1.0);
  public static readonly BindableProperty DistanceXProperty =
    BindableProperty.CreateAttached ("DistanceX", typeof(double), typeof(ShadowEffect), 0.0);
  public static readonly BindableProperty DistanceYProperty =
    BindableProperty.CreateAttached ("DistanceY", typeof(double), typeof(ShadowEffect), 0.0);

  public static bool GetHasShadow (BindableObject view)
  {
    return (bool)view.GetValue (HasShadowProperty);
  }

  public static void SetHasShadow (BindableObject view, bool value)
  {
    view.SetValue (HasShadowProperty, value);
  }
  ...

  static void OnHasShadowChanged (BindableObject bindable, object oldValue, object newValue)
  {
    var view = bindable as View;
    if (view == null) {
      return;
    }

    bool hasShadow = (bool)newValue;
    if (hasShadow) {
      view.Effects.Add (new LabelShadowEffect ());
    } else {
      var toRemove = view.Effects.FirstOrDefault (e => e is LabelShadowEffect);
      if (toRemove != null) {
        view.Effects.Remove (toRemove);
      }
    }
  }

  class LabelShadowEffect : RoutingEffect
  {
    public LabelShadowEffect () : base ("MyCompany.LabelShadowEffect")
    {
    }
  }
}
```

`ShadowEffect` Olan beş iliştirilmiş özellikler içeren `static` alıcılar ve ayarlayıcılar her için bağlı özelliği. Dört bu özelliklerin her platforma özel geçirilecek parametreler temsil `LabelShadowEffect`. `ShadowEffect` Sınıfı da tanımlar bir `HasShadow` ekli eklenmesi veya kaldırılmasını denetlemek için etkili denetlemek için kullanılan özellik, `ShadowEffect` sınıfı için eklenmiş olmasıdır. İliştirilmiş özellik kayıtları `OnHasShadowChanged` özelliğinin değeri değiştiğinde, yürütülecek yöntemi. Bu yöntemi ekler veya kaldırır değerine göre etkisini `HasShadow` ekli özellik.

İç içe `LabelShadowEffect` sınıfı, alt [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) sınıfı, destekleyen etkili eklenmesi ve kaldırılması. `RoutingEffect` Sınıf genellikle platforma özel efekt iç sarmalayan bir platformdan bağımsız etkisi temsil eder. Tür bilgilerini bir platforma özel efekt için hiçbir derleme zamanı erişimi olduğundan bu etkili temizleme işlemini kolaylaştırır. `LabelShadowEffect` Oluşturucusu çözümleme grup adı ve her platforma özel efekt sınıfı belirtilen benzersiz kimliği bir birleşimini içeren bir parametre olarak geçirerek, bir temel sınıf oluşturucusunu çağırır. Efekt ekleme ve kaldırma işlemleri, böylece `OnHasShadowChanged` yöntemini aşağıdaki gibi:

- **Efekt ekleme** – yeni bir örneğini `LabelShadowEffect` denetimin eklenen [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu. Bu kullanarak değiştirir [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) efekt eklemek için yöntemi.
- **Efekt kaldırma** – ilk örneğinin `LabelShadowEffect` denetimin içindeki [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyon alınır ve kaldırıldı.

## <a name="consuming-the-effect"></a>Etkisi kullanma

Her platforma özgü `LabelShadowEffect` iliştirilmiş özellikler için ekleyerek kullanılabilecek bir [ `Label` ](xref:Xamarin.Forms.Label) denetimi aşağıdaki XAML kod örneğinde gösterildiği gibi:

```xaml
<Label Text="Label Shadow Effect" ...
       local:ShadowEffect.HasShadow="true" local:ShadowEffect.Radius="5"
       local:ShadowEffect.DistanceX="5" local:ShadowEffect.DistanceY="5">
  <local:ShadowEffect.Color>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="Black" />
        <On Platform="Android" Value="White" />
        <On Platform="UWP" Value="Red" />
    </OnPlatform>
  </local:ShadowEffect.Color>
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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Ayarı `ShadowEffect.HasShadow` özelliğine bağlı `true` yürütür `ShadowEffect.OnHasShadowChanged` ekler veya kaldırır yöntemi `LabelShadowEffect` için [ `Label` ](xref:Xamarin.Forms.Label) denetimi. Her iki kod örneklerinde `ShadowEffect.Color` ekli özellik platforma özel renk değerleri sağlar. Daha fazla bilgi için [cihaz sınıfı](~/xamarin-forms/platform/device.md).

Ayrıca, bir [ `Button` ](xref:Xamarin.Forms.Button) gölge rengi çalışma zamanında değiştirilmesine izin verir. Zaman `Button` tıklatıldığında, aşağıdaki kod değişikliklerini gölge rengi ayarlayarak `ShadowEffect.Color` ekli özellik:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Geçerli bir stil ile kullanma

İliştirilmiş özellikler için bir denetim ekleyerek kullanılabilen efektleri stili tarafından da kullanılır. Aşağıdaki XAML kod örnekte gösterildiği bir *açık* stili için uygulanabilir gölge etkisi, [ `Label` ](xref:Xamarin.Forms.Label) denetimler:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="True" />
    <Setter Property="local:ShadowEffect.Radius" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceX" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceY" Value="5" />
  </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style) Uygulanabilir bir [ `Label` ](xref:Xamarin.Forms.Label) ayarlayarak onun [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özelliğini `Style` kullanarak`StaticResource`işaretleme uzantısı, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Stilleri hakkında daha fazla bilgi için bkz. [stilleri](~/xamarin-forms/user-interface/styles/index.md).

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
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                Control.Layer.ShadowOpacity = 1.0f;
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateRadius ()
        {
            Control.Layer.CornerRadius = (nfloat)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            Control.Layer.ShadowColor = ShadowEffect.GetColor (Element).ToCGColor ();
        }

        void UpdateOffset ()
        {
            Control.Layer.ShadowOffset = new CGSize (
                (double)ShadowEffect.GetDistanceX (Element),
                (double)ShadowEffect.GetDistanceY (Element));
        }
    }
```

`OnAttached` Yöntemi çağıran yöntemleri kullanarak ekli özellik değerlerini alma `ShadowEffect` alıcıları ve ayarlanan `Control.Layer` gölge oluşturmak için özellik değerlerini özellikleri. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi bağlı denetim sahip değil durumunda block `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

#### <a name="responding-to-property-changes"></a>Özellik değişikliklere yanıt verme

Varsa `ShadowEffect` ekli özellik değerleri değişiklik zamanında etkili değişiklikler görüntüleyerek yanıt gerekiyor. Geçersiz kılınan bir sürümünü `OnElementPropertyChanged` platforma özel efekt sınıfında yöntemi yerdir bağlanılabilir özellik değişikliklerine yanıt verme aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Yöntemi güncelleştirmeleri RADIUS, renk veya gölge uzaklığı uygun koşullarda `ShadowEffect` ekli özellik değeri değişti. Bu geçersiz kılma birden çok kez çağrılabilir olarak değişen bir özellik için bir onay her zaman yapılmalıdır.

### <a name="android-project"></a>Android projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulaması Android projesi için:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        Android.Widget.TextView control;
        Android.Graphics.Color color;
        float radius, distanceX, distanceY;

        protected override void OnAttached ()
        {
            try {
                control = Control as Android.Widget.TextView;
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                UpdateControl ();
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateControl ()
        {
            if (control != null) {
                control.SetShadowLayer (radius, distanceX, distanceY, color);
            }
        }

        void UpdateRadius ()
        {
            radius = (float)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            color = ShadowEffect.GetColor (Element).ToAndroid ();
        }

        void UpdateOffset ()
        {
            distanceX = (float)ShadowEffect.GetDistanceX (Element);
            distanceY = (float)ShadowEffect.GetDistanceY (Element);
        }
    }
```

`OnAttached` Yöntemi çağıran yöntemleri kullanarak ekli özellik değerlerini alma `ShadowEffect` alıcıları ve çağıran bir yöntemi çağıran [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) özellik değerlerini kullanarak bir gölge oluşturmak için yöntemi. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi bağlı denetim sahip değil durumunda block `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

#### <a name="responding-to-property-changes"></a>Özellik değişikliklere yanıt verme

Varsa `ShadowEffect` ekli özellik değerleri değişiklik zamanında etkili değişiklikler görüntüleyerek yanıt gerekiyor. Geçersiz kılınan bir sürümünü `OnElementPropertyChanged` platforma özel efekt sınıfında yöntemi yerdir bağlanılabilir özellik değişikliklerine yanıt verme aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
      UpdateControl ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Yöntemi güncelleştirmeleri RADIUS, renk veya gölge uzaklığı uygun koşullarda `ShadowEffect` ekli özellik değeri değişti. Bu geçersiz kılma birden çok kez çağrılabilir olarak değişen bir özellik için bir onay her zaman yapılmalıdır.

### <a name="universal-windows-platform-project"></a>Evrensel Windows platformu projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` Evrensel Windows Platformu (UWP) proje için uygulama:

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        Label shadowLabel;
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;

                    shadowLabel = new Label ();
                    shadowLabel.Text = textBlock.Text;
                    shadowLabel.FontAttributes = FontAttributes.Bold;
                    shadowLabel.HorizontalOptions = LayoutOptions.Center;
                    shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;

                    UpdateColor ();
                    UpdateOffset ();

                    ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                    shadowAdded = true;
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateColor ()
        {
            shadowLabel.TextColor = ShadowEffect.GetColor (Element);
        }

        void UpdateOffset ()
        {
            shadowLabel.TranslationX = ShadowEffect.GetDistanceX (Element);
            shadowLabel.TranslationY = ShadowEffect.GetDistanceY (Element);
        }
    }
}
```

Evrensel Windows platformu, bir gölge etkisi sağlamaz ve böylece `LabelShadowEffect` hem de platformlarda uygulama ikinci bir uzaklık ekleyerek bir benzetim [ `Label` ](xref:Xamarin.Forms.Label) birincil arkasında `Label`. `OnAttached` Yöntemi oluşturur Yeni `Label` ve üzerinde bazı düzen özellikleri ayarlar `Label`. Kullanarak ekli özellik değerlerini alma yöntemleri çağırır `ShadowEffect` , alıcıları ve gölge ayarlayarak oluşturur [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)ve [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) rengini ve konumunu denetlemek için Özellikler `Label`. `shadowLabel` Daha sonra eklenen birincil kaydırma `Label`. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi bağlı denetim sahip değil durumunda block `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

#### <a name="responding-to-property-changes"></a>Özellik değişikliklere yanıt verme

Varsa `ShadowEffect` ekli özellik değerleri değişiklik zamanında etkili değişiklikler görüntüleyerek yanıt gerekiyor. Geçersiz kılınan bir sürümünü `OnElementPropertyChanged` platforma özel efekt sınıfında yöntemi yerdir bağlanılabilir özellik değişikliklerine yanıt verme aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
                      args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Yöntemi güncelleştirmeleri rengini veya gölge uzaklığı uygun koşullarda `ShadowEffect` ekli özellik değeri değişti. Bu geçersiz kılma birden çok kez çağrılabilir olarak değişen bir özellik için bir onay her zaman yapılmalıdır.

## <a name="summary"></a>Özet

Bu makalede ekli efekt ve çalışma zamanında bir parametre değiştirme parametreleri geçirmek için özellikleri kullanarak gösterilmiştir. Ekli özellikler, çalışma zamanı özellik değişikliklerine yanıt etkisi parametrelerini tanımlamak için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Etkin](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Gölge efektini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
