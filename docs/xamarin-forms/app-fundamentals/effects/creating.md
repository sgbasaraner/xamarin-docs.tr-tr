---
title: Efekt oluşturma
description: Bir denetimin özelleştirme etkileri basitleştirin. Bu makalede nasıl odağı denetimi elde edince giriş denetiminin arka plan rengi değişir bir efekt oluşturulacağı gösterilmektedir.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: 12b8906dd4562e58dede181e773e4046b8434214
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="creating-an-effect"></a>Efekt oluşturma

_Bir denetimin özelleştirme etkileri basitleştirin. Bu makalede nasıl odağı denetimi elde edince giriş denetiminin arka plan rengi değişir bir efekt oluşturulacağı gösterilmektedir._

Her platforma özgü project üzerinde bir etkisi oluşturma işlemi aşağıdaki gibidir:

1. Öğesinin bir alt kümesi oluşturmak `PlatformEffect` sınıfı.
1. Geçersiz kılma `OnAttached` denetimi özelleştirmek için yöntem ve yazma mantığı.
1. Geçersiz kılma `OnDetached` gerekirse denetim özelleştirme temizlemeye yöntemi ve yazma mantığı.
1. Ekleme bir [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) özniteliği etkisi sınıfı. Bu öznitelik, bir şirket uluslararası ad alanı aynı ada sahip başka etkileri çakışmalarla önleme efektler için ayarlar. Bu öznitelik yalnızca bir kez proje uygulanabilir olduğunu unutmayın.
1. Ekleme bir [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) özniteliği etkisi sınıfı. Bu öznitelik etkisi Xamarin.Forms tarafından grup adı ile birlikte bir denetime uygulamadan önce etkisi bulmak için kullanılan benzersiz bir kimliği ile kaydeder. Öznitelik iki parametre – etkisi ve denetime uygulamadan önce etkisi bulmak için kullanılan benzersiz bir dize türü adını alır.

Etkisi, ardından uygun denetimi ekleyerek tüketilebilir.

> [!NOTE]
> Her platform projesinde efekt sağlamak isteğe bağlıdır. Kayıtlı değilse, bir efekt kullanılmaya çalışılıyor hiçbir şey yapmaz boş olmayan bir değer döndürür.

Örnek uygulamayı gösteren bir `FocusEffect` odak elde edince denetim arka plan rengini değişiklikler. Aşağıdaki diyagram, her proje örnek uygulamasında, aralarındaki ilişkilerin birlikte sorumlulukları gösterir:

![](creating-images/focus-effect.png "Odağı etkisi Proje Sorumlulukları")

Bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetiminin `HomePage` tarafından özelleştirilmiş `FocusEffect` her platforma özgü projesinde sınıfı. Her `FocusEffect` sınıfı türer `PlatformEffect` her platform için sınıf. Bu, sonuçlanır `Entry` odağı denetimi elde edince Değişeni aşağıdaki ekran görüntülerinde gösterildiği gibi bir platforma özgü arka plan rengi ile işlenen denetim:

![](creating-images/screenshots-1.png "Odak her platformda etkili")
![](creating-images/screenshots-2.png "odak her platformun üzerindeki etkisi")

## <a name="creating-the-effect-on-each-platform"></a>Her platformun üzerindeki etkisi oluşturma

Aşağıdaki bölümlerde platforma özgü uygulanması açıklanmaktadır `FocusEffect` sınıfı.

## <a name="ios-project"></a>iOS projesi

Aşağıdaki örnekte gösterildiği kod `FocusEffect` uygulama iOS projesi için:

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

`OnAttached` Yöntemi kümeleri `BackgroundColor` özelliği ile açık mor denetiminin `UIColor.FromRGB` yöntemi ve ayrıca bir alanda bu renk depolar. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi eklendiği denetimi yoktur engelleme bir `BackgroundColor` özelliği. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

`OnElementPropertyChanged` Geçersiz kılma bağlanabilirse özellik değişikliklerini Xamarin.Forms denetimindeki yanıt verir. Zaman [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) özellik değişikliklerini `BackgroundColor` denetiminin özelliği denetimi odağı varsa beyaza değiştirilmiş, aksi takdirde Açık Mor olarak değiştirilir. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi eklendiği denetimi yoktur engelleme bir `BackgroundColor` özelliği.

## <a name="android-project"></a>Android projesi

Aşağıdaki örnekte gösterildiği kod `FocusEffect` uygulama Android projesi için:

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

`OnAttached` Yöntem çağrılarını `SetBackgroundColor` açık denetim arka plan rengini ayarlamak için yöntemin yeşil ve ayrıca bir alanda bu renk depolar. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi eklendiği denetimi yoktur engelleme bir `SetBackgroundColor` özelliği. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

`OnElementPropertyChanged` Geçersiz kılma bağlanabilirse özellik değişikliklerini Xamarin.Forms denetimindeki yanıt verir. Zaman [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) özellik değişikliklerini denetimi odağı varsa denetimin arka plan rengi beyaz değiştirildiğinde, aksi takdirde açık yeşil değiştirilir. Bu işlev içinde kaydırılan bir `try` / `catch` durumda etkisi eklendiği denetimi yoktur engelleme bir `BackgroundColor` özelliği.

## <a name="universal-windows-platform-projects"></a>Evrensel Windows platformu projeleri

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

`OnAttached` Yöntemi kümeleri `Background` mavi ve ayarlar denetiminin özelliği `BackgroundFocusBrush` beyaz özelliği. Bu işlev içinde kaydırılan bir `try` / `catch` etkisi eklendiği denetim bu özellikleri eksik durumda engelleyin. Hiçbir uygulama tarafından sağlanan `OnDetached` yöntemi temizleme gerekli olduğundan.

## <a name="consuming-the-effect"></a>Etkisi kullanma

Xamarin.Forms .NET standart kitaplığı veya paylaşılan kitaplığı projesinden efekt tüketimi için işlem aşağıdaki gibidir:

1. Efekti özelleştirilmiş bir denetimi bildirin.
1. Denetimin ekleyerek etkisi denetimine ekleme [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu.

> [!NOTE]
> Etkili örneği yalnızca tek bir denetim bağlanabilir. Bu nedenle, iki kez iki denetimleri kullanmak için bir etki çözülmelidir.

## <a name="consuming-the-effect-in-xaml"></a>XAML'de etkisi kullanma

Aşağıdaki XAML kodu örnekte gösterildiği bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) hangi denetimine `FocusEffect` eklenir:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` .NET standart kitaplığı sınıfında etkisi tüketim XAML'de destekler ve aşağıdaki kod örneğinde gösterildiği:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect` Sınıf alt sınıfların [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) genellikle platforma özgü bir iç efekt sarmalar platformdan bağımsız etkisi temsil eden sınıf. `FocusEffect` Sınıfı çağırır çözümleme grup adı birleşimini oluşan bir parametre geçirme temel sınıf oluşturucusu (kullanarak belirtilen [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) etkisi sınıfı özniteliği), ve benzersiz kimliği kullanarak belirtilen [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) etkisi Sınıf özniteliği. Bu nedenle, [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) çalışma zamanında, yeni bir örneğini başlatılmış `MyCompany.FocusEffect` denetimin eklenen [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu.

Efektler bir davranış kullanarak denetimlere da eklenebilir ya da ekli özelliklerini kullanarak. Bir davranış kullanarak denetime efekt ekleme hakkında daha fazla bilgi için bkz: [yeniden kullanılabilir EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Ekli özellikler kullanarak denetime efekt ekleme hakkında daha fazla bilgi için bkz: [efekt parametreleri geçirme](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>C aslında kullanma&num;

Eşdeğer [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) C# ' de aşağıdaki kod örneğinde gösterilir:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` Bağlı `Entry` denetimin etkisi ekleyerek örneği [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) Döndüren bir [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) için belirtilen ad, olduğu çözümleme grup adı birleşimini (kullanarak belirtilen [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) etkisi sınıfı özniteliği) ve kullanarak belirtilen benzersiz bir kimlik [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) etkisi Sınıf özniteliği. Bir platformda etkili sağlamıyorsa `Effect.Resolve` yöntemi döndürür olmayan bir`null` değeri.

## <a name="summary"></a>Özet

Arka plan rengini değiştirir efekt oluşturma bu makalede gösterilen [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) zaman denetim odağı kazanır denetim.


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Etkisi](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [Arka plan rengi efekti (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Odağı etkili (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
