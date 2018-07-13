---
title: Yeniden kullanılabilir EffectBehavior
description: Davranışlar efekt kazan blondan etkisi işleme kodunu arka plan kod dosyalarını kaldırma, bir denetim eklemek için kullanışlı bir yaklaşım olan. Bu makalede, efekt için bir denetim eklemek için bir Xamarin.Forms davranışı kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 1ce7eda6f556041cbffc3793b00e8e2cba44d3d0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995787"
---
# <a name="reusable-effectbehavior"></a>Yeniden kullanılabilir EffectBehavior

_Davranışlar efekt kazan blondan etkisi işleme kodunu arka plan kod dosyalarını kaldırma, bir denetim eklemek için kullanışlı bir yaklaşım olan. Bu makalede, efekt için bir denetim eklemek için bir Xamarin.Forms davranışı kullanmayı gösterir._

## <a name="overview"></a>Genel Bakış

`EffectBehavior` Sınıfı ekleyen yeniden kullanılabilir bir özel Xamarin.Forms davranışı, bir [ `Effect` ](xref:Xamarin.Forms.Effect) davranışı denetimine bağlı ve kaldırır denetime örneğine `Effect` davranıştır zaman örneği denetiminden kullanımdan çıkarıldı.

Davranışın kullanılabilmesi için aşağıdaki davranış özelliklerini ayarlamanız gerekir:

- **Grup** – değerini [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) etkisi sınıfı özniteliği.
- **Adı** – değerini [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) etkisi sınıfı özniteliği.

Etkileri hakkında daha fazla bilgi için bkz: [etkileri](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="creating-the-behavior"></a>Davranışı oluşturma

`EffectBehavior` Sınıf türetilir [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) sınıfı burada `T` olduğu bir [ `View` ](xref:Xamarin.Forms.View). Diğer bir deyişle `EffectBehavior` sınıfı için herhangi bir Xamarin.Forms denetimi eklenebilir.

### <a name="implementing-bindable-properties"></a>Bağlanabilir Özellikler uygulama

`EffectBehavior` Sınıfı tanımlayan iki [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) eklemek için kullanılan örnekler, bir [ `Effect` ](xref:Xamarin.Forms.Effect) davranışı denetime eklendiğinde denetim için. Bu özellikler aşağıdaki kod örneğinde gösterilmiştir:

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

Zaman `EffectBehavior` kullanılır, `Group` özelliğinin değeri olarak ayarlanması [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) efekt için öznitelik. Ayrıca, `Name` özelliğinin değeri olarak ayarlanması [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) etkisi sınıfı özniteliği.

### <a name="implementing-the-overrides"></a>Geçersiz kılmaları uygulama

`EffectBehavior` Sınıf geçersiz kılmalarını [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) ve [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) yöntemlerinin [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) aşağıdaki kodda gösterildiği gibi sınıfı Örnek:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Yöntemi çağırarak kurulum gerçekleştirir `AddEffect` ekli denetiminde bir parametre olarak geçirerek yöntemi. [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Yöntemi çağırarak temizleme gerçekleştirir `RemoveEffect` ekli denetiminde bir parametre olarak geçirerek yöntemi.

### <a name="implementing-the-behavior-functionality"></a>Davranış işlevselliği uygulama

Davranış amacı eklemektir [ `Effect` ](xref:Xamarin.Forms.Effect) tanımlanan `Group` ve `Name` özellikleri denetime davranışı denetimine bağlı ve kaldırmak için `Effect` davranıştır zaman denetiminden kullanımdan çıkarıldı. Çekirdek davranışı işlevselliği aşağıdaki kod örneğinde gösterilmiştir:

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

`AddEffect` Yanıt olarak yöntemi yürütüldüğünde `EffectBehavior` denetim ve bu bağlı eklenen denetimin bir parametre olarak alır. Yöntemi, alınan etkisi ardından denetimin ekler. [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu. `RemoveEffect` Yanıt olarak yöntemi yürütüldüğünde `EffectBehavior` denetim ve onu ayrılmakta ekli denetimi bir parametre olarak alır. Yöntemi, etkisi ardından denetimin kaldırır. [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu.

`GetEffect` Yöntemi kullanan [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) alınacak yöntemi [ `Effect` ](xref:Xamarin.Forms.Effect). Etkili bir birleşimi bulunduğu `Group` ve `Name` özellik değerleri. Bir platformda etkili sağlamıyorsa `Effect.Resolve` yöntemi döndürür olmayan bir`null` değeri.

## <a name="consuming-the-behavior"></a>Davranış kullanma

`EffectBehavior` Sınıfı için eklenebilir [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) aşağıdaki XAML kod örneğinde gösterildiği gibi bir denetim koleksiyonu:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

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

`Group` Ve `Name` davranış özelliklerini değerleri kümesi [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) ve [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) her platforma özel efekt sınıf için öznitelikleri Proje.

Davranış bağlı olduğu çalışma zamanında [ `Label` ](xref:Xamarin.Forms.Label) denetimi `Xamarin.LabelShadowEffect` denetimin eklenecek [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu. Tarafından görüntülenen metni eklenen bir gölge sonuçlanır `Label` aşağıdaki ekran görüntülerinde gösterildiği denetimi:

![](effect-behavior-images/screenshots.png "Örnek uygulama ile EffectsBehavior")

Bu davranışı eklemek ve denetimlerini etkiler kaldırmak için kullanmanın avantajı, kazan blondan etkisi işleme kodu arka plan kod dosyaları kaldırılabilir ' dir.

## <a name="summary"></a>Özet

Bu makalede, efekt için bir denetim eklemek için davranış kullanarak gösterdik. `EffectBehavior` Sınıfı ekleyen yeniden kullanılabilir bir özel Xamarin.Forms davranışı, bir [ `Effect` ](xref:Xamarin.Forms.Effect) davranışı denetimine bağlı ve kaldırır denetime örneğine `Effect` davranıştır zaman örneği denetiminden kullanımdan çıkarıldı.


## <a name="related-links"></a>İlgili bağlantılar

- [Etkiler](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Etkin davranış (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Davranışı](xref:Xamarin.Forms.Behavior)
- [Davranışı<T>](xref:Xamarin.Forms.Behavior`1)
