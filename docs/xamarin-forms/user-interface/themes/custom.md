---
title: Xamarin.Forms özel tema oluşturma
description: Bu makalede, bir uygulamada başvurmak için özel bir Xamarin.Forms Tema oluşturma açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 4FE08ADC-093F-47FA-B33C-20CF08B5D7E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 018193cf0b198fd87f0f09cbfeba52e9d2a0f68b
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38838176"
---
# <a name="creating-a-custom-xamarinforms-theme"></a>Xamarin.Forms özel tema oluşturma

![](~/media/shared/preview.png "Bu API, şu anda Önizleme aşamasındadır")

Bir tema, bir Nuget paketi ekleme yanı sıra (gibi [ışık](~/xamarin-forms/user-interface/themes/light.md) ve [koyu](~/xamarin-forms/user-interface/themes/dark.md) temaları), kendi kaynak uygulamanızda başvurulabilir sözlük Temalar oluşturabilirsiniz.

## <a name="example"></a>Örnek

Üç `BoxView`gösterilen s [Temalar sayfa](~/xamarin-forms/user-interface/themes/index.md) iki karşıdan yüklenebilir tema tanımlanan üç sınıflara göre biçimlendirilmiş.

Bu nasıl çalıştığını anlamak için aşağıdaki biçimlendirme doğrudan ekleyebilirsiniz eşdeğer bir stil oluşturur, **App.xaml**.

Not `Class` özniteliğini `Style` (başlangıcı yerine sonundan [ `x:Key` ](~/xamarin-forms/user-interface/styles/inheritance.md) özniteliği Xamarin.Forms önceki sürümlerinde kullanılabilir).

```xml
<ResourceDictionary>
  <!-- DEFINE ANY CONSTANTS -->
  <Color x:Key="SeparatorLineColor">#CCCCCC</Color>
  <Color x:Key="iOSDefaultTintColor">#007aff</Color>
  <Color x:Key="AndroidDefaultAccentColorColor">#1FAECE</Color>
  <OnPlatform x:TypeArguments="Color" x:Key="AccentColor">
    <On Platform="iOS" Value="{StaticResource iOSDefaultTintColor}" />
    <On Platform="Android" Value="{StaticResource AndroidDefaultAccentColorColor}" />
  </OnPlatform>
  <!--  BOXVIEW CLASSES -->
  <Style TargetType="BoxView" Class="HorizontalRule">
    <Setter Property="BackgroundColor" Value="{ StaticResource SeparatorLineColor }" />
    <Setter Property="HeightRequest" Value="1" />
  </Style>

  <Style TargetType="BoxView" Class="Circle">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="WidthRequest" Value="34"/>
    <Setter Property="HeightRequest" Value="34"/>
    <Setter Property="HorizontalOptions" Value="Start" />

    <Setter Property="local:ThemeEffects.Circle" Value="True" />
  </Style>

  <Style TargetType="BoxView" Class="Rounded">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />

    <Setter Property="local:ThemeEffects.CornerRadius" Value="4" />
  </Style>
</ResourceDictionary>
```

Olduğunu fark edeceksiniz `Rounded` sınıfı başvuran bir özel efekt `CornerRadius`.
Bu etkiyi kodunu doğru özel başvurmak için aşağıda - verilmiştir `xmlns` eklenmelidir **App.xaml**ait kök öğe:

```csharp
xmlns:local="clr-namespace:ThemesDemo;assembly=ThemesDemo"
```

### <a name="c-code-in-the-net-standard-library-project-or-shared-project"></a>C# kodu .NET Standard kitaplığı projesi ya da paylaşılan proje

Hepsini köşe oluşturmak için kod `BoxView` kullanan [etkileri](~/xamarin-forms/app-fundamentals/effects/index.md).
Köşe yarıçapı kullanılarak uygulanan bir `BindableProperty` ve uygulama tarafından uygulanan bir [etkisi](~/xamarin-forms/app-fundamentals/effects/index.md). Etkisi platforma özgü kod gerektirir [iOS](#ios) ve [Android](#android) projeleri (aşağıda gösterilmiştir.)

```csharp
namespace ThemesDemo
{
  public static class ThemeEffects
  {
  public static readonly BindableProperty CornerRadiusProperty =
    BindableProperty.CreateAttached("CornerRadius", typeof(double), typeof(ThemeEffects), 0.0, propertyChanged: OnChanged<CornerRadiusEffect, double>);
    private static void OnChanged<TEffect, TProp>(BindableObject bindable, object oldValue, object newValue)
              where TEffect : Effect, new()
    {
        var view = bindable as View;
        if (view == null)
        {
            return;
        }

        if (EqualityComparer<TProp>.Equals(newValue, default(TProp)))
        {
            var toRemove = view.Effects.FirstOrDefault(e => e is TEffect);
            if (toRemove != null)
            {
                view.Effects.Remove(toRemove);
            }
        }
        else
        {
            view.Effects.Add(new TEffect());
        }

    }
    public static void SetCornerRadius(BindableObject view, double radius)
    {
        view.SetValue(CornerRadiusProperty, radius);
    }

    public static double GetCornerRadius(BindableObject view)
    {
        return (double)view.GetValue(CornerRadiusProperty);
    }

    private class CornerRadiusEffect : RoutingEffect
    {
        public CornerRadiusEffect()
            : base("Xamarin.CornerRadiusEffect")
        {
        }
    }
  }
}
```

<a name="ios" />

### <a name="c-code-in-the-ios-project"></a>C# kodu iOS projesi

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;
using CoreGraphics;
using Foundation;
using XFThemes;

namespace ThemesDemo.iOS
{
    public class CornerRadiusEffect : PlatformEffect
    {
        private nfloat _originalRadius;

        protected override void OnAttached()
        {
            if (Container != null)
            {
                _originalRadius = Container.Layer.CornerRadius;
                Container.ClipsToBounds = true;

                UpdateCorner();
            }
        }

        protected override void OnDetached()
        {
            if (Container != null)
            {
                Container.Layer.CornerRadius = _originalRadius;
                Container.ClipsToBounds = false;
            }
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                UpdateCorner();
            }
        }

        private void UpdateCorner()
        {
            Container.Layer.CornerRadius = (nfloat)ThemeEffects.GetCornerRadius(Element);
        }
    }
}
```

<a name="android" />

### <a name="c-code-in-the-android-project"></a>C# kodu Android projesi

```csharp
using System;
using Xamarin.Forms.Platform;
using Xamarin.Forms.Platform.Android;
using Android.Views;
using Android.Graphics;

namespace ThemesDemo.Droid
{
    public class CornerRadiusEffect : BaseEffect
    {
        private ViewOutlineProvider _originalProvider;

        protected override bool CanBeApplied()
        {
            return Container != null && (int)Android.OS.Build.VERSION.SdkInt >= 21;
        }

        protected override void OnAttachedInternal()
        {
            _originalProvider = Container.OutlineProvider;
            Container.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            Container.ClipToOutline = true;
        }

        protected override void OnDetachedInternal()
        {
            Container.OutlineProvider = _originalProvider;
            Container.ClipToOutline = false;
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (!Attached)
            {
                return;
            }

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                Container.Invalidate();
            }
        }

        private class CornerRadiusOutlineProvider : ViewOutlineProvider
        {
            private Xamarin.Forms.Element _element;

            public CornerRadiusOutlineProvider(Xamarin.Forms.Element element)
            {
                _element = element;
            }

            public override void GetOutline(Android.Views.View view, Outline outline)
            {
                var pixles =
                    (float)ThemeEffects.GetCornerRadius(_element) *
                    view.Resources.DisplayMetrics.Density;

                outline.SetRoundRect(new Rect(0, 0, view.Width, view.Height), (int)pixles);
            }
        }
    }
}
```


## <a name="summary"></a>Özet

Özel bir tema özel görünüşünü gerektiren her denetimi için stiller tanımlayarak oluşturulabilir. Bir denetimi için birden çok stiller farklı ayırt `Class` kaynak sözlüğünde öznitelikleri ve ardından ayarlayarak uygulanan `StyleClass` denetim özniteliği.

Bir stil de kullanabilir [etkileri](~/xamarin-forms/app-fundamentals/effects/index.md) daha fazla denetimin görünümünü özelleştirme.

[Örtük stiller](~/xamarin-forms/user-interface/styles/implicit.md) (ya da olmadan bir `x:Key` veya `Style` özniteliği) eşleşen tüm denetimlere uygulanmaya devam `TargetType`.
