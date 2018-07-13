---
title: Bir efekti oluşturma
description: Bir denetimin özelleştirme etkileri basitleştirin. Bu makalede, Denetim odağı aldığında, giriş denetiminin arka plan rengi değişen efekt oluşturma işlemini gösterir.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: b29d83999724a35293882f7b9efc0158171c4fd2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998166"
---
# <a name="creating-an-effect"></a>Bir efekti oluşturma

_Bir denetimin özelleştirme etkileri basitleştirin. Bu makalede, Denetim odağı aldığında, giriş denetiminin arka plan rengi değişen efekt oluşturma işlemini gösterir._

Her platforma özgü projede efekt oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir öğesinin `PlatformEffect` sınıfı.
1. Geçersiz kılma `OnAttached` denetimi özelleştirmek için yöntemi ve yazma mantığı.
1. Geçersiz kılma `OnDetached` denetimi özelleştirme gerekirse temizlemek için yöntemi ve yazma mantığı.
1. Ekleme bir [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) etkisi sınıfı özniteliği. Bu öznitelik, bir şirket geniş ad alanı ile aynı ada sahip başka etkileri çakışmaları önleme etkileri için ayarlar. Bu öznitelik yalnızca bir kez proje uygulanabilir olduğunu unutmayın.
1. Ekleme bir [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) etkisi sınıfı özniteliği. Bu öznitelik etkisi Xamarin.Forms tarafından grup adının yanı sıra geçerli denetimi uygulamadan önce bulmak için kullanılan benzersiz bir kimliği ile kaydeder. Öznitelik, iki parametre – etkin ve etkin bir denetime uygulamadan önce bulmak için kullanılan benzersiz bir dize türü adını alır.

Efekt için uygun denetimi ekleyerek ardından tarafından kullanılabilir.

> [!NOTE]
> Her platform proje içinde bir efektin sağlamak isteğe bağlıdır. Kayıtlı değilse, efekt kullanılmaya çalışılıyor, hiçbir şey yapmaz ve null olmayan bir değer döndürür.

Örnek uygulamayı gösteren bir `FocusEffect` odak edince denetimin arka plan rengi değişiklikler. Örnek uygulamada, onlar arasındaki ilişkileri yanı sıra her bir proje sorumluluklarını Aşağıdaki diyagramda gösterilmektedir:

![](creating-images/focus-effect.png "Odağı geçerli proje sorumlulukları")

Bir [ `Entry` ](xref:Xamarin.Forms.Entry) denetimi `HomePage` göre özelleştirilmiş `FocusEffect` her platforma özgü projede sınıfı. Her `FocusEffect` sınıf türetilir `PlatformEffect` her platform için sınıf. Sonuçlanır `Entry` denetim odağı aldığında Değişeni aşağıdaki ekran görüntülerinde gösterildiği gibi bir platforma özgü arka plan rengi ile işlenen denetim:

![](creating-images/screenshots-1.png "Her platformda etkili odaklanmak")
![](creating-images/screenshots-2.png "her platformda etkili odaklanın")

## <a name="creating-the-effect-on-each-platform"></a>Her platformda efekti oluşturma

Aşağıdaki bölümlerde platforma özel uygulanışı `FocusEffect` sınıfı.

## <a name="ios-project"></a>iOS projesi

Aşağıdaki örnekte gösterildiği kod `FocusEffect` uygulaması iOS projesi için:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` Yöntemi kümeleri `BackgroundColor` özelliği ile açık mor denetimin `UIColor.FromRGB` yöntemi ve ayrıca bir alanda bu renk depolar. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi eklendiği denetim sahip değil durumunda engelleyecek bir `BackgroundColor` özelliği. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

`OnElementPropertyChanged` Geçersiz kılma yanıt bağlanılabilir özellik değişiklikleri Xamarin.Forms denetimi. Zaman [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) özellik değişiklikleri `BackgroundColor` denetimi özelliği, beyaz olarak değiştirildiğinde, Denetim odağa sahip değilse, aksi takdirde ışık mor olarak değiştirilir. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi eklendiği denetim sahip değil durumunda engelleyecek bir `BackgroundColor` özelliği.

## <a name="android-project"></a>Android projesi

Aşağıdaki örnekte gösterildiği kod `FocusEffect` uygulaması Android projesi için:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached ()
        {
            try {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor (backgroundColor);

            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);
            try {
                if (args.PropertyName == "IsFocused") {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor) {
                        Control.SetBackgroundColor (Android.Graphics.Color.Black);
                    } else {
                        Control.SetBackgroundColor (backgroundColor);
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` Yöntem çağrılarını `SetBackgroundColor` yöntemi açık denetimin arka plan rengini ayarlamak için yeşil ve ayrıca bir alanda bu renk depolar. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi eklendiği denetim sahip değil durumunda engelleyecek bir `SetBackgroundColor` özelliği. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

`OnElementPropertyChanged` Geçersiz kılma yanıt bağlanılabilir özellik değişiklikleri Xamarin.Forms denetimi. Zaman [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) özellik değişiklikleri denetim odağa sahip değilse denetimin arka plan rengi beyaza dönüştürülür, aksi takdirde için açık yeşil değiştirilir. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi eklendiği denetim sahip değil durumunda engelleyecek bir `BackgroundColor` özelliği.

## <a name="universal-windows-platform-projects"></a>Evrensel Windows platformu projelerinde

Aşağıdaki örnekte gösterildiği kod `FocusEffect` uygulama Evrensel Windows Platformu (UWP) projeleri için:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.UWP
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

`OnAttached` Yöntemi kümeleri `Background` Camgöbeği ve kümelerinin denetiminin özelliği `BackgroundFocusBrush` beyaz özelliği. Bu işlev içinde sarmalanmış bir `try` / `catch` etkisi eklendiği denetimi bu özellikleri eksik durumda engelleyin. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

## <a name="consuming-the-effect"></a>Etkisi kullanma

Efekt bir Xamarin.Forms .NET Standard kitaplığı veya paylaşılan kitaplık projesi kullanma işlemi aşağıdaki gibidir:

1. Efekti özelleştirilmiş bir denetimi bildirin.
1. Denetimin ekleyerek etkisi denetimine ekleme [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu.

> [!NOTE]
> Bir efekti örneği yalnızca tek bir denetim eklenebilir. Bu nedenle, iki denetimlerinde iki kez kullanmak için bir efekti çözümlenmesi gerekir.

## <a name="consuming-the-effect-in-xaml"></a>XAML etkisini kullanma

Aşağıdaki XAML kod örnekte gösterildiği bir [ `Entry` ](xref:Xamarin.Forms.Entry) denetim hangi `FocusEffect` eklenir:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` .NET Standard kitaplığı sınıfında etkisi tüketim XAML içinde destekler ve aşağıdaki kod örneğinde gösterilen:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect` Sınıfı kılabileceği [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) genellikle platforma özel efekt iç sarmalayan bir platformdan bağımsız etkisi temsil eden sınıf. `FocusEffect` Sınıfı çözümleme grup adının bir birleşimini içeren bir parametre geçirerek, bir temel sınıf oluşturucusunu çağırır (kullanarak belirtilen [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) etkisi sınıfı özniteliği), ve benzersiz kimliği kullanarak belirtilen [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) etkisi sınıfı özniteliği. Bu nedenle, [ `Entry` ](xref:Xamarin.Forms.Entry) çalışma zamanında, yeni bir örneğini başlatılır `MyCompany.FocusEffect` denetimin eklenen [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu.

Efektleri bir davranış kullanarak denetimlere de eklenmesi veya iliştirilmiş özellikler kullanarak. Efekt denetime bir davranış kullanarak ekleme hakkında daha fazla bilgi için bkz. [yeniden kullanılabilir EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Ekli özellikler kullanarak, denetime efekt ekleme hakkında daha fazla bilgi için bkz. [efekt parametrelerini geçirme](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>C etkisini kullanma&num;

Eşdeğer [ `Entry` ](xref:Xamarin.Forms.Entry) C# ' de aşağıdaki kod örneğinde gösterilir:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` İliştirildiği `Entry` denetimin etkisi ekleyerek örneği [ `Effects` ](xref:Xamarin.Forms.Element.Effects) aşağıdaki kod örneğinde gösterildiği gibi koleksiyon:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) Döndürür bir [ `Effect` ](xref:Xamarin.Forms.Effect) belirtilen ad için olan çözüm grubu adı (kullanarak belirtilen [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) etkisi sınıfı özniteliği) ve kullanarak belirtilen benzersiz kimliği [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) etkisi sınıfı özniteliği. Bir platformda etkili sağlamıyorsa `Effect.Resolve` yöntemi döndürür olmayan bir`null` değeri.

## <a name="summary"></a>Özet

Bu makalede gösterilen arka plan rengini değiştirir. efekt oluşturma [ `Entry` ](xref:Xamarin.Forms.Entry) ne zaman denetim odağı kazanır denetim.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Etkin](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [Arka plan rengi efekti (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Odak etkisi (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
