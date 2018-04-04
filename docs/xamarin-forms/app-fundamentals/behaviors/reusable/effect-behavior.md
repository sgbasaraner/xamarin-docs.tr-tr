---
title: Yeniden kullanılabilir EffectBehavior
description: Davranışları arka plan kod dosyaları koddan işleme kazan kalıbı etkisi kaldırma bir denetime efekt eklemek için yararlı bir yaklaşım ' dir. Bu makalede, bir denetim için bir efekt eklemek için bir Xamarin.Forms davranışını kullanarak gösterilmektedir.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: a1612d1e87f0e05c859babd93fd03ac9a5736b47
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="reusable-effectbehavior"></a>Yeniden kullanılabilir EffectBehavior

_Davranışları arka plan kod dosyaları koddan işleme kazan kalıbı etkisi kaldırma bir denetime efekt eklemek için yararlı bir yaklaşım ' dir. Bu makalede, bir denetim için bir efekt eklemek için bir Xamarin.Forms davranışını kullanarak gösterilmektedir._

## <a name="overview"></a>Genel Bakış

`EffectBehavior` Sınıftır ekleyen yeniden kullanılabilir bir özel Xamarin.Forms davranışı bir [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) davranışı denetime bağlı ve kaldırır denetim örneğine `Effect` davranışı olduğunda örneği denetimden ayrıldı.

Aşağıdaki davranışı özellikleri davranışı kullanacak şekilde ayarlamanız gerekir:

- **Grup** – değerini [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) etkisi Sınıf özniteliği.
- **Ad** – değerini [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) etkisi Sınıf özniteliği.

Etkileri hakkında daha fazla bilgi için bkz: [efektler](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="creating-the-behavior"></a>Davranışı oluşturma

`EffectBehavior` Sınıfı türer [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) sınıfı, burada `T` olan bir [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/). Bunun anlamı `EffectBehavior` sınıfı için herhangi bir Xamarin.Forms denetimi eklenebilir.

### <a name="implementing-bindable-properties"></a>Bağlanabilir özelliklerini uygulama

`EffectBehavior` Sınıfı tanımlayan iki [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) eklemek için kullanılan örnekler, bir [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) davranışı denetimine bağlı bir denetim için. Bu özellikler, aşağıdaki kod örneğinde gösterilir:

```csharp
public class EffectBehavior : Behavior<View>
{
  public static readonly BindableProperty GroupProperty =
    BindableProperty.Create ("Group", typeof(string), typeof(EffectBehavior), null);
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(EffectBehavior), null);

  public string Group {
    get { return (string)GetValue (GroupProperty); }
    set { SetValue (GroupProperty, value); }
  }

  public string Name {
    get { return(string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }
  ...
}
```

Zaman `EffectBehavior` tüketiliyor, `Group` özelliği değerine ayarlanmalıdır [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) etkisi özniteliği. Ayrıca, `Name` özelliği değerine ayarlanmalıdır [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) etkisi Sınıf özniteliği.

### <a name="implementing-the-overrides"></a>Geçersiz kılmaları uygulama

`EffectBehavior` Geçersiz kılmaları sınıf [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) ve [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) yöntemlerinin [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) aşağıdaki kodda gösterildiği gibi sınıfı Örnek:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  protected override void OnAttachedTo (BindableObject bindable)
  {
    base.OnAttachedTo (bindable);
    AddEffect (bindable as View);
  }

  protected override void OnDetachingFrom (BindableObject bindable)
  {
    RemoveEffect (bindable as View);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Yöntemi çağrılarak kurulum gerçekleştirir `AddEffect` yöntemi, ekli denetiminde bir parametre olarak geçirme. [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Yöntemi çağrılarak temizleme gerçekleştirir `RemoveEffect` yöntemi, ekli denetiminde bir parametre olarak geçirme.

### <a name="implementing-the-behavior-functionality"></a>Davranış işlevlerini uygulama

Davranış amacı eklemektir [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) tanımlanan `Group` ve `Name` davranışı denetime bağlı ve kaldırdığınız durumlarda bir denetim özelliklerine `Effect` davranışı olduğunda denetimden ayrıldı. Çekirdek davranışı işlevselliği, aşağıdaki kod örneğinde gösterilir:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  void AddEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Add (GetEffect ());
    }
  }

  void RemoveEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Remove (GetEffect ());
    }
  }

  Effect GetEffect ()
  {
    if (!string.IsNullOrWhiteSpace (Group) && !string.IsNullOrWhiteSpace (Name)) {
      return Effect.Resolve (string.Format ("{0}.{1}", Group, Name));
    }
    return null;
  }
}
```

`AddEffect` Yanıt olarak yöntemi yürütüldüğünde `EffectBehavior` bir denetim ve bu iliştirilmekte ekli denetim bir parametre olarak alır. Yöntemi, alınan etkisi sonra denetimin ekler. [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu. `RemoveEffect` Yanıt olarak yöntemi yürütüldüğünde `EffectBehavior` bir denetim ve onu ayrılmakta ekli denetim bir parametre olarak alır. Yöntemi, etkisi sonra denetimin kaldırır. [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu.

`GetEffect` Yöntemi kullanan [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) almak için yöntemini [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/). Etkili bir birleşimi bulunduğu `Group` ve `Name` özellik değerleri. Bir platformda etkili sağlamıyorsa `Effect.Resolve` yöntemi döndürür olmayan bir`null` değeri.

## <a name="consuming-the-behavior"></a>Davranış kullanma

`EffectBehavior` Sınıfı için eklenebilir [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) aşağıdaki XAML kod örneğinde gösterildiği gibi bir denetim koleksiyonu:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};
label.Behaviors.Add (new EffectBehavior {
  Group = "Xamarin",
  Name = "LabelShadowEffect"
});
```

`Group` Ve `Name` davranışı özellik değerleri kümesinin [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) ve [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) her platforma özgü etkisi sınıfında öznitelikleri Proje.

Davranış bağlı olduğu çalışma zamanında [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetimi `Xamarin.LabelShadowEffect` denetimin eklenecek [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) koleksiyonu. Bu tarafından görüntülenen metni eklenmesini gölge sonuçlanır `Label` denetlemek, aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](effect-behavior-images/screenshots.png "EffectsBehavior örnek uygulaması")

Eklemek ve etkileri denetimlerden kaldırmak için bu davranış kullanmanın avantajı, kazan kalıbı etkisi işleme kodu arka plan kodu dosyalarından kaldırılabilir ' dir.

## <a name="summary"></a>Özet

Bu makalede, bir denetime efekt eklemek için bir davranış kullanarak gösterilmektedir. `EffectBehavior` Sınıftır ekleyen yeniden kullanılabilir bir özel Xamarin.Forms davranışı bir [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) davranışı denetime bağlı ve kaldırır denetim örneğine `Effect` davranışı olduğunda örneği denetimden ayrıldı.


## <a name="related-links"></a>İlgili bağlantılar

- [Etkiler](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Etkili davranış (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Davranışı<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
