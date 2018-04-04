---
title: Xamarin.Forms Behaviors
description: Xamarin.Forms davranışları davranışı ya da davranışı türetme tarafından oluşturulan<T> sınıfı. Bu makalede, oluşturmasına ve Xamarin.Forms davranışları kullanmasına gösterilmiştir.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2848b554d2dbd6d3d69ae864846247b3612d64e6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms Behaviors

_Xamarin.Forms davranışları davranışı ya da davranışı türetme tarafından oluşturulan<T> sınıfı. Bu makalede, oluşturmasına ve Xamarin.Forms davranışları kullanmasına gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms davranışı oluşturma işlemi aşağıdaki gibidir:

1. Öğesinden devralınan bir sınıf oluşturun [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) veya [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) sınıfı, burada `T` davranışı uygulamalısınız denetim türüdür.
1. Geçersiz kılma [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) gerekli herhangi bir ayar yapmak için yöntemi.
1. Geçersiz kılma [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) tüm gerekli temizlenmesini yöntemi.
1. Davranış çekirdek işlevselliğini uygulayın.

Bu, aşağıdaki kod örneğinde gösterildiği yapısı sonuçlanır:

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

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Yöntemi davranışı denetime bağlı hemen sonra tetiklenir. Bu yöntem, bağlı olduğu ve olay işleyicileri kaydetmek veya davranışı işlevleri desteklemek için gerekli olan diğer Kurulumu gerçekleştirmek için kullanılan denetlemek için bir başvuru alır. Örneğin, bir denetim bir olaya abone. Olayı için olay işleyicisinde sonra davranış işlevselliği uygulanması.

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Yöntemi davranışı denetimden kaldırıldığında tetiklenir. Bu yöntem, bağlı olduğu ve tüm gerekli temizleme gerçekleştirmek için kullanılan denetlemek için bir başvuru alır. Örneğin, bir denetim bellek sızıntılarını önleme olayından aboneliği.

Davranışı ardından eklemeyi tarafından kullanılabilecek [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) uygun denetim koleksiyonu.

## <a name="creating-a-xamarinforms-behavior"></a>Xamarin.Forms davranışı oluşturma

Örnek uygulamayı gösteren bir `NumericValidationBehavior`, kullanıcı tarafından girilen değer vurgular bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) değilse kırmızı olarak kontrol bir `double`. Aşağıdaki kod örneğinde davranışı gösterilir:

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

`NumericValidationBehavior` Türetilen [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) sınıfı, burada `T` olan bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Yöntemi için bir olay işleyicisi kaydeder [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) olay ile [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) XML'dekikaydetmeyöntemi`TextChanged`bellek önlemek için olay sızdırıyor. Davranış çekirdek işlevselliğini tarafından sağlanan `OnEntryTextChanged` kullanıcı tarafından girilen değer ayrıştırır yöntemi `Entry`ve ayarlar [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) özelliğini değer değilse kırmızı bir `double`.

> [!NOTE]
> Xamarin.Forms ayarlı değil `BindingContext` bir davranış çünkü davranışları paylaşılan ve birden çok denetim stilleri aracılığıyla uygulanır.

## <a name="consuming-a-xamarinforms-behavior"></a>Xamarin.Forms davranışı kullanma

Her Xamarin.Forms denetimine sahip bir [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) olduğu bir veya daha fazla davranışları eklenebilir, aşağıdaki XAML kod örneğinde gösterildiği gibi koleksiyon:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Eşdeğer [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) C# ' de aşağıdaki kod örneğinde gösterilir:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Çalışma zamanında göre davranışı uygulama denetimi ile etkileşim için davranış yanıt. Aşağıdaki ekran görüntüleri için geçersiz giriş yanıt davranış gösterir:

[![](creating-images/screenshots-sml.png "Örnek uygulama Xamarin.Forms davranışı")](creating-images/screenshots.png#lightbox "örnek uygulama Xamarin.Forms davranışı")

> [!NOTE]
> Belirli bir denetim türünü (veya birçok denetimlere uygulayabileceğiniz bir üst sınıf) için davranışlar yazılır ve bunlar yalnızca uyumlu bir denetim eklenmesi gerekir. Uyumsuz bir denetim için bir davranış ekleme girişiminde oluşturulan bir özel durum neden olur.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Xamarin.Forms davranışı bir stil ile kullanma

Davranışları da açık veya örtülü stili tarafından kullanılabilecek. Ancak, ayarlayan bir stil oluşturma [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) denetimin özelliğini mümkün değildir özelliği salt okunur olduğundan. Ekli özellik ekleme ve kaldırma davranışını denetleyen davranış sınıfı eklemek için çözümüdür. İşlem aşağıdaki gibidir:

1. Ekli özellik ekleme veya kaldırma bağlı olduğu davranışı olacak denetlemek için davranış denetlemek için kullanılacak davranış sınıfına ekleyin. Ekli özelliğe kaydeder olun bir `propertyChanged` özelliğinin değeri değiştiğinde, yürütülecek temsilci.
1. Oluşturma bir `static` alıcı ve ekli özellik ayarlayıcı.
1. Uygulama mantığı `propertyChanged` eklemek ve davranışı kaldırmak için temsilci.

Aşağıdaki kod örneğinde ekleme ve kaldırma denetimleri iliştirilmiş bir özellik gösterir `NumericValidationBehavior`:

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

`NumericValidationBehavior` Sınıfı içeren adlı bir ekli özellik `AttachBehavior` ile bir `static` alıcı ve ekleme veya kaldırma için bunu ekleneceği denetlemek için davranış denetimleri ayarlayıcı. Bu özellik yazmaçlar bağlı `OnAttachBehaviorChanged` özelliğinin değeri değiştiğinde, yürütülecek yöntemi. Bu yöntem ekler veya değerine göre denetlemek için davranış kaldırır `AttachBehavior` özelliği eklenmiş.

Aşağıdaki örnekte gösterildiği kod bir *açık* için stil `NumericValidationBehavior` kullanan `AttachBehavior` bağlı özelliği ve hangi uygulanabilir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetimleri:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Uygulanabilir bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ayarlayarak denetim kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özelliğine `Style` kullanma örneği `StaticResource` biçimlendirme uzantısı, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Stilleri hakkında daha fazla bilgi için bkz: [stilleri](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Bağlanabilir özelliklerini ayarlamak veya, davranışları oluşturursanız, XAML sorgulanan bir davranış durumuna sahip ekleyebilirsiniz ancak bunlar denetimlerinde arasında Paylaşılmaması gereken bir `Style` içinde bir `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Bir davranış denetimden kaldırma

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Ne zaman bir davranış denetimden kaldırılır ve Bellek sızıntısını önlemek için bir olaydan aboneliği gibi gerekli temizleme işlemini yapmak için kullanılan yöntemi harekete. Ancak, davranışları örtük olarak denetimlerden sürece kaldırılmaz denetimin [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) tarafından koleksiyonu değiştirilmiş bir `Remove` veya `Clear` yöntemi. Aşağıdaki kod örneğinde, belirli bir davranışı bir denetimin kaldırma gösterilmektedir `Behaviors` koleksiyonu:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Alternatif olarak, denetimin [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) koleksiyonu temizlenmiş, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
entry.Behaviors.Clear();
```

Ayrıca, sayfa gezinti yığınından Sil'i zaman davranışları örtük olarak denetimlerden kaldırılmaz olduğunu unutmayın. Bunun yerine, bunlar açıkça kapsamının dışına giderek sayfaları öncesinde kaldırılmalıdır.

## <a name="summary"></a>Özet

Bu makalede nasıl oluşturulacağını ve Xamarin.Forms davranışları tüketen gösterilmektedir. Xamarin.Forms davranışları türetme tarafından oluşturulan [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) veya [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) sınıfı.


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Forms davranışı (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Xamarin.Forms davranışı stille (örnek) uygulanan](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Davranışı<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
