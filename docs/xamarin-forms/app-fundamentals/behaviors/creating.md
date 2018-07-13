---
title: Xamarin.Forms davranışları
description: Xamarin.Forms davranışları davranış veya davranışını türetme tarafından oluşturulan<T> sınıfı. Bu makalede, oluşturma ve tüketme Xamarin.Forms davranışları gösterilmektedir.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 7e057567ec0bb72e9bcc016d4a9fef3af78a3ea1
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998901"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms davranışları

_Xamarin.Forms davranışları davranış veya davranışını türetme tarafından oluşturulan<T> sınıfı. Bu makalede, oluşturma ve tüketme Xamarin.Forms davranışları gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms davranışları oluşturma işlemi aşağıdaki gibidir:

1. Devralınan bir sınıf oluşturmanız [ `Behavior` ](xref:Xamarin.Forms.Behavior) veya [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) sınıfı burada `T` davranışı uygulanacağını denetim türü.
1. Geçersiz kılma [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) herhangi bir kurulum yapmak için yöntemi.
1. Geçersiz kılma [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) gerekli tüm temizleme işlemlerini gerçekleştirmek için yöntemi.
1. Davranış temel işlevlerini uygular.

Aşağıdaki kod örneğinde gösterilen yapı sonuçlanır:

```csharp
public class CustomBehavior : Behavior<View>
{
    protected override void OnAttachedTo (View bindable)
    {
        base.OnAttachedTo (bindable);
        // Perform setup
    }

    protected override void OnDetachingFrom (View bindable)
    {
        base.OnDetachingFrom (bindable);
        // Perform clean up
    }

    // Behavior implementation
}
```

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Yöntemi davranışı bir denetime hemen eklendikten sonra tetiklenir. Bu yöntem, Denetim, bağlı olduğu ve olay işleyicilerini kaydedin veya davranış işlevselliği desteklemek için gereken diğer Kurulumu gerçekleştirmek için kullanılan bir başvuru alır. Örneğin, denetim üzerinde bir olaya abone. Olayı için olay işleyicisinde sonra davranış işlevselliğini uygulanması.

[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Yöntemi davranışı denetiminden kaldırıldığında tetiklenir. Bu yöntem, Denetim, bağlı olduğu ve gerekli tüm temizleme işlemlerini gerçekleştirmek için kullanılan bir başvuru alır. Örneğin, bellek sızıntılarını önlemek için bir denetim üzerinde bir olay aboneliği.

Davranışı ardından eklemeyi tarafından tüketilebilecek [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) uygun denetim koleksiyonu.

## <a name="creating-a-xamarinforms-behavior"></a>Xamarin.Forms davranışı oluşturma

Örnek uygulamayı gösterir bir `NumericValidationBehavior`, kullanıcı tarafından girilen değer vurgular bir [ `Entry` ](xref:Xamarin.Forms.Entry) denetim kırmızı renkte değilse bir `double`. Davranışı aşağıdaki kod örneğinde gösterilmiştir:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    protected override void OnAttachedTo(Entry entry)
    {
        entry.TextChanged += OnEntryTextChanged;
        base.OnAttachedTo(entry);
    }

    protected override void OnDetachingFrom(Entry entry)
    {
        entry.TextChanged -= OnEntryTextChanged;
        base.OnDetachingFrom(entry);
    }

    void OnEntryTextChanged(object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` Türetildiği [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) sınıfı burada `T` olduğu bir [ `Entry` ](xref:Xamarin.Forms.Entry). [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Yöntemi için bir olay işleyicisi kaydeder [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) olay ile [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) XML'dekikaydetmeyöntemi`TextChanged`bellek önlemek için olay sızıntıları. Davranış temel işlevlerini tarafından sağlanan `OnEntryTextChanged` kullanıcı tarafından girilen değer ayrıştırır yöntemi `Entry`ve ayarlar [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) özellik değeri değilse, kırmızı bir `double`.

> [!NOTE]
> Xamarin.Forms ayarlı değil `BindingContext` bir davranış davranışları paylaşılan ve birden çok denetim stilleri aracılığıyla uygulanır.

## <a name="consuming-a-xamarinforms-behavior"></a>Xamarin.Forms davranışları kullanma

Her bir Xamarin.Forms denetimine sahip bir [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) olduğu bir veya daha fazla davranışları eklenebilir, aşağıdaki XAML kod örneğinde gösterildiği gibi koleksiyon:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Eşdeğer [ `Entry` ](xref:Xamarin.Forms.Entry) C# ' de aşağıdaki kod örneğinde gösterilir:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Çalışma zamanında davranış göre davranışını uygulama denetimi ile etkileşim için yanıtlar. Aşağıdaki ekran görüntüleri için geçersiz giriş yanıt davranış gösterir:

[![](creating-images/screenshots-sml.png "Örnek uygulama Xamarin.Forms davranış")](creating-images/screenshots.png#lightbox "örnek uygulama Xamarin.Forms davranışı")

> [!NOTE]
> Davranışlar, belirli bir denetim türünü (veya birçok denetim için uygulayabileceğiniz bir sınıf) yazılır ve uyumlu bir denetime yalnızca eklenmelidir. Uyumsuz bir denetime bir davranış ekleme girişimi oluşturulan bir özel durum neden olur.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Bir Xamarin.Forms davranışı bir stil ile kullanma

Davranışlar açık veya örtülü stili tarafından tarafından kullanılabilir. Ancak, ayarlayan bir stil oluşturma [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) denetimin bir özelliğine mümkün değildir özelliği salt okunur olduğundan. Çözüm ekleme ve kaldırma davranışını denetleyen davranış sınıfı ekli özelliği eklemektir. İşlemi aşağıdaki gibidir:

1. İliştirilmiş özellik eklenmesi veya kaldırılmasını davranışına bağlı olduğu davranışı olacak denetim denetlemek için kullanılacak davranış sınıfı ekleyin. Ekli özellik kaydedeceğini olun bir `propertyChanged` özelliğinin değeri değiştiğinde çalıştırılacak temsilci.
1. Oluşturma bir `static` alıcı ve ayarlayıcı için ekli özellik.
1. Uygulama mantığı `propertyChanged` ekleme ve kaldırma davranışı için temsilci.

Aşağıdaki kod örneği, ekleme ve kaldırma denetleyen ekli özelliği gösterir `NumericValidationBehavior`:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached ("AttachBehavior", typeof(bool), typeof(NumericValidationBehavior), false, propertyChanged: OnAttachBehaviorChanged);

    public static bool GetAttachBehavior (BindableObject view)
    {
        return (bool)view.GetValue (AttachBehaviorProperty);
    }

    public static void SetAttachBehavior (BindableObject view, bool value)
    {
        view.SetValue (AttachBehaviorProperty, value);
    }

    static void OnAttachBehaviorChanged (BindableObject view, object oldValue, object newValue)
    {
        var entry = view as Entry;
        if (entry == null) {
            return;
        }

        bool attachBehavior = (bool)newValue;
        if (attachBehavior) {
            entry.Behaviors.Add (new NumericValidationBehavior ());
        } else {
            var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
            if (toRemove != null) {
                entry.Behaviors.Remove (toRemove);
            }
        }
    }
    ...
}
```

`NumericValidationBehavior` Sınıfı içeren adlı ekli özelliği `AttachBehavior` ile bir `static` alıcı ve ayarlayıcı, ekleme veya kaldırma davranışı bağlı denetim denetleyen. İliştirilmiş özellik kayıtları `OnAttachBehaviorChanged` özelliğinin değeri değiştiğinde, yürütülecek yöntemi. Bu yöntem, ekler veya kaldırır değerine göre denetim, davranıştır `AttachBehavior` ekli özellik.

Aşağıdaki kod örnekte gösterildiği bir *açık* için stil `NumericValidationBehavior` kullanan `AttachBehavior` ekli özellik ve hangi uygulanabilir [ `Entry` ](xref:Xamarin.Forms.Entry) denetimler:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style) Uygulanabilir bir [ `Entry` ](xref:Xamarin.Forms.Entry) ayarlayarak denetim kendi [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özelliğini `Style` kullanma örneği `StaticResource` işaretleme uzantısı, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Stilleri hakkında daha fazla bilgi için bkz. [stilleri](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Bağlanabilir özellikler ayarlanır veya XAML içinde davranışları oluşturursanız, sorgulanan bir davranış için sahip durum ekleyebilirsiniz ancak bunlar arasında denetimleri Paylaşılmaması gereken bir `Style` içinde bir `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Bir davranış denetimden kaldırılıyor

[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Yöntemi bir davranış denetimden kaldırılır ve bir bellek sızıntısı önlemek için bir olay aboneliği gibi gerekli tüm temizleme işlemlerini gerçekleştirmek için kullanılan olduğunda tetiklenir. Ancak, davranışları örtük olarak denetimlerden sürece kaldırılmaz denetimin [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) koleksiyon tarafından değiştirildiğinde bir `Remove` veya `Clear` yöntemi. Aşağıdaki kod örneği, belirli bir davranışı bir denetimin kaldırılıyor gösterir `Behaviors` koleksiyonu:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Alternatif olarak, denetimin [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) koleksiyonu işaretinin kaldırılması, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
entry.Behaviors.Clear();
```

Ayrıca, sayfa gezinti yığından POP davranışları örtük olarak denetimlerden kaldırılmasını değil unutmayın. Bunun yerine, bunlar açıkça sayfaları kapsam dışına geçmeden önce kaldırılmalıdır.

## <a name="summary"></a>Özet

Bu makale oluşturma ve Xamarin.Forms davranışları kullanma gösterilmektedir. Xamarin.Forms davranışları türeterek oluşturulur [ `Behavior` ](xref:Xamarin.Forms.Behavior) veya [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) sınıfı.


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms davranışları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Xamarin.Forms davranışları (örnek) sahip bir stil uygulanmış](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Davranışı](xref:Xamarin.Forms.Behavior)
- [Davranışı<T>](xref:Xamarin.Forms.Behavior`1)
