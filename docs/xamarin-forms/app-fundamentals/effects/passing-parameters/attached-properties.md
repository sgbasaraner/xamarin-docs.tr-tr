---
title: Ekli özellikler olarak etkisi parametreleri geçirme
description: Ekli özellikler çalışma zamanı özelliği değişikliklere etkisi parametrelerini tanımlamak için kullanılabilir. Bu makalede, iliştirilmiş bir etkisi ve çalışma zamanında bir parametre değiştirme parametreleri geçirmek için özellikler kullanarak gösterilmektedir.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 5bca36189100942e21d1d750dd156dab0cf45fc4
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Ekli özellikler olarak etkisi parametreleri geçirme

_Ekli özellikler çalışma zamanı özelliği değişikliklere etkisi parametrelerini tanımlamak için kullanılabilir. Bu makalede, iliştirilmiş bir etkisi ve çalışma zamanında bir parametre değiştirme parametreleri geçirmek için özellikler kullanarak gösterilmektedir._

Çalışma zamanı özelliği değişikliklere efekt parametreleri oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir `static` efekti geçirilecek her parametre için eklenen bir özellik içeren sınıf.
1. Bir ek ekli özellik eklenmesi veya sınıf iliştirilmiş denetim efekti kaldırılması denetlemek için kullanılan sınıfına ekleyin. Bu özellik yazmaçlar bağlı emin bir `propertyChanged` özelliğinin değeri değiştiğinde, yürütülecek temsilci.
1. Oluşturma `static` alıcılar ve ayarlayıcılar her bağlı özelliği.
1. Uygulama mantığı `propertyChanged` eklemek ve etkisi kaldırmak için temsilci.
1. İçinde iç içe bir sınıf uygulama `static` etkisi sonra hangi alt sınıfların adlı sınıf `RoutingEffect` sınıfı. Oluşturucusu, çözümleme grup adı ve her platforma özgü etkisi sınıf içinde belirtilmiş benzersiz kimliği birleşimini geçirme temel sınıf oluşturucu çağırın.

Parametreleri ardından uygun denetimi ekli özellikler ve özellik değerleri ekleyerek efekti geçirilebilir. Ayrıca, yeni bir ekli özellik değeri belirterek parametreleri, çalışma zamanında'e değiştirilebilir.

> [!NOTE]
> Ekli özellik bağlanabilir özelliği, ancak diğer nesnelere bağlı ve XAML'de tanınabilir bir sınıftaki bir sınıf ve bir noktayla ayrılmış bir özellik adı içeren öznitelikler tanımlanan özel bir türde değil. Daha fazla bilgi için bkz: [ekli özellikler](~/xamarin-forms/xaml/attached-properties.md).

Örnek uygulamayı gösteren bir `ShadowEffect` tarafından görüntülenen metni gölge ekleyen bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetim. Ayrıca, gölge rengini çalışma zamanında değiştirilebilir. Aşağıdaki diyagram, her proje örnek uygulamasında, aralarındaki ilişkilerin birlikte sorumlulukları gösterir:

![](attached-properties-images/shadow-effect.png "Gölge etkisi Proje Sorumlulukları")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetiminin `HomePage` tarafından özelleştirilmiş `LabelShadowEffect` her platforma özgü projesinde. Parametreleri geçirilir kadar her `LabelShadowEffect` aracılığıyla özelliklerinde bağlı `ShadowEffect` sınıfı. Her `LabelShadowEffect` sınıfı türer `PlatformEffect` her platform için sınıf. Bu tarafından görüntülenen metni eklenmesini gölge sonuçlanır `Label` denetlemek, aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](attached-properties-images/screenshots.png "Her platformda gölge efekti")

## <a name="creating-effect-parameters"></a>Efekt parametreleri oluşturma

A `static` sınıfı oluşturulmalıdır efekt parametreleri temsil etmek için aşağıdaki kod örneğinde gösterildiği gibi:

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

`ShadowEffect` Olan beş ekli özellikler içeren `static` alıcılar ve ayarlayıcılar her bağlı özelliği. Dört bu özelliklerin her platforma özel geçirilecek parametreleri temsil `LabelShadowEffect`. `ShadowEffect` Sınıfı ayrıca tanımlayan bir `HasShadow` iliştirilmiş eklenmesi veya etkisini denetlemek için kaldırılması denetlemek için kullanılan özelliği, `ShadowEffect` sınıfı bağlı. Bu özellik yazmaçlar bağlı `OnHasShadowChanged` özelliğinin değeri değiştiğinde, yürütülecek yöntemi. Bu yöntem ekler veya değerine göre etkisi kaldırır `HasShadow` özelliği eklenmiş.

İç içe `LabelShadowEffect` sınıfı, hangi alt sınıfların [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) sınıfı, destekler etkisi ekleme ve kaldırma. `RoutingEffect` Sınıfı, genellikle platforma özgü bir iç efekt sarmalar platformdan bağımsız etkisi temsil eder. Tür bilgisi için bir platforma özgü etkisi hiçbir derleme zamanı erişimi olduğundan etkisi kaldırma işlemi basitleştirir. `LabelShadowEffect` Oluşturucusu çözümleme grup adı ve her platforma özgü etkisi sınıf içinde belirtilmiş benzersiz kimliği birleşimini oluşan bir parametre olarak geçirme temel sınıf oluşturucu çağırır. Bu efekti ekleme ve kaldırma işleminde sağlar `OnHasShadowChanged` şekilde yöntemi:

- **Efekt toplama** – yeni bir örneğini `LabelShadowEffect` denetimin eklenen [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu. Bu kullanarak değiştirir [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) etkisi ekleme yöntemi.
- **Efekt Temizleme** – ilk örneğini `LabelShadowEffect` denetimin içinde [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu alınır ve kaldırıldı.

## <a name="consuming-the-effect"></a>Etkisi kullanma

Her platforma özgü `LabelShadowEffect` ekli özellikler ekleyerek kullanılabilecek bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) aşağıdaki XAML kod örneğinde gösterildiği gibi denetim:

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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Ayarı `ShadowEffect.HasShadow` özelliğine bağlı `true` yürütür `ShadowEffect.OnHasShadowChanged` ekler veya kaldırır yöntemi `LabelShadowEffect` için [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetim. Her iki kod örnekleri, `ShadowEffect.Color` ekli özellik platforma özgü renk değerleri sağlar. Daha fazla bilgi için bkz: [aygıt sınıfı](~/xamarin-forms/platform/device.md).

Ayrıca, bir [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) gölge rengini çalışma zamanında değiştirilmesine izin verir. Zaman `Button` tıklandığında, gölge renk ayarlayarak aşağıdaki kod değişikliklerini `ShadowEffect.Color` özelliği eklenmiş:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Geçerli bir stil ile kullanma

Bir denetime ekli özellikler ekleyerek tüketilebilir etkileri stili tarafından da tüketilebilir. Aşağıdaki XAML kodu örnekte gösterildiği bir *açık* stili için uygulanabilir gölge etkisi, [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetimleri:

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

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Uygulanabilir bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ayarlayarak kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özelliğine `Style` kullanarakörnek`StaticResource`biçimlendirme uzantısı, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Stilleri hakkında daha fazla bilgi için bkz: [stilleri](~/xamarin-forms/user-interface/styles/index.md).

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

`OnAttached` Yöntemini kullanarak ekli özellik değerlerini alma yöntemleri çağırır `ShadowEffect` alıcıları ve ayarlanan `Control.Layer` gölge oluşturmak için özellik değerlerini özellikleri. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi bağlı denetimi yoktur engelleme `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

#### <a name="responding-to-property-changes"></a>Özellik değişikliklere yanıt verme

Varsa `ShadowEffect` bağlı özellik değerleri değişikliği zamanında etkili değişiklikler görüntüleyerek yanıt gerekiyor. Geçersiz kılınan bir sürümünü `OnElementPropertyChanged` platforma özgü etkisi sınıftaki yöntemi yerdir bağlanabilirse özelliği değişikliklere için aşağıdaki kod örneğinde gösterildiği gibi:

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

`OnElementPropertyChanged` Yöntemi güncelleştirmeleri RADIUS, renk veya gölgenin uygun sağlanan `ShadowEffect` ekli özellik değeri değişti. Bu geçersiz kılma birçok kez çağrılabilir olarak değiştirildiğinde bir özellik için bir onay her zaman yapılmalıdır.

### <a name="android-project"></a>Android projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulama Android projesi için:

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

`OnAttached` Yöntemini kullanarak ekli özellik değerlerini alma yöntemleri çağırır `ShadowEffect` alıcılar, çağıran bir yöntemi çağırır [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) özellik değerlerini kullanarak bir gölge oluşturmak için yöntem. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi bağlı denetimi yoktur engelleme `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

#### <a name="responding-to-property-changes"></a>Özellik değişikliklere yanıt verme

Varsa `ShadowEffect` bağlı özellik değerleri değişikliği zamanında etkili değişiklikler görüntüleyerek yanıt gerekiyor. Geçersiz kılınan bir sürümünü `OnElementPropertyChanged` platforma özgü etkisi sınıftaki yöntemi yerdir bağlanabilirse özelliği değişikliklere için aşağıdaki kod örneğinde gösterildiği gibi:

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

`OnElementPropertyChanged` Yöntemi güncelleştirmeleri RADIUS, renk veya gölgenin uygun sağlanan `ShadowEffect` ekli özellik değeri değişti. Bu geçersiz kılma birçok kez çağrılabilir olarak değiştirildiğinde bir özellik için bir onay her zaman yapılmalıdır.

### <a name="universal-windows-platform-project"></a>Evrensel Windows platformu projesi

Aşağıdaki örnekte gösterildiği kod `LabelShadowEffect` uygulama Evrensel Windows Platformu (UWP) proje için:

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

Evrensel Windows platformu gölge efekti sağlamaz ve böylece `LabelShadowEffect` uygulaması hem platformlarda benzetim bir ikinci bir uzaklık ekleyerek [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) birincil arkasında `Label`. `OnAttached` Yöntemi oluşturur Yeni `Label` ve bazı yerleşim özellikleri ayarlar `Label`. Ardından kullanarak ekli özellik değerlerini alma yöntemlerini çağıran `ShadowEffect` alıcılar ve ayarlayarak gölge oluşturur [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)ve [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) özelliklerini konumunu ve renk denetlemek için `Label`. `shadowLabel` Daha sonra eklenen birincil uzaklığı `Label`. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi bağlı denetimi yoktur engelleme `Control.Layer` özellikleri. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

#### <a name="responding-to-property-changes"></a>Özellik değişikliklere yanıt verme

Varsa `ShadowEffect` bağlı özellik değerleri değişikliği zamanında etkili değişiklikler görüntüleyerek yanıt gerekiyor. Geçersiz kılınan bir sürümünü `OnElementPropertyChanged` platforma özgü etkisi sınıftaki yöntemi yerdir bağlanabilirse özelliği değişikliklere için aşağıdaki kod örneğinde gösterildiği gibi:

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

`OnElementPropertyChanged` Yöntemi güncelleştirmeleri rengini veya gölgenin uygun sağlanan `ShadowEffect` ekli özellik değeri değişti. Bu geçersiz kılma birçok kez çağrılabilir olarak değiştirildiğinde bir özellik için bir onay her zaman yapılmalıdır.

## <a name="summary"></a>Özet

Bu makalede, iliştirilmiş bir etkisi ve çalışma zamanında bir parametre değiştirme parametreleri geçirmek için özellikler kullanarak gösterilmiştir. Ekli özellikler çalışma zamanı özelliği değişikliklere etkisi parametrelerini tanımlamak için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Etkisi](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Gölge efekti (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
