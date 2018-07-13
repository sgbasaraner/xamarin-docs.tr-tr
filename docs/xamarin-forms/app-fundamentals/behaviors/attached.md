---
title: Ekli davranışlar
description: Ekli davranışlar, bir veya daha fazla ekli özellikler ile statik sınıflardır. Bu makalede, ekli davranışlar oluşturup gösterilmektedir.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2c9bd9ad4e7572b9eae6f0073da8a2c8f1e7c9fc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995352"
---
# <a name="attached-behaviors"></a>Ekli davranışlar

_Ekli davranışlar, bir veya daha fazla ekli özellikler ile statik sınıflardır. Bu makalede, ekli davranışlar oluşturup gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Ekli özelliği, özel bir bağlanılabilir özellik türüdür. Bir sınıf içinde tanımlanan ancak diğer nesnelere bağlı ve bir sınıf ya da bir nokta ile ayrılmış bir özellik adını içeren öznitelikler olarak XAML içinde tanınan olduklarından.

Ekli özelliği tanımlayabilirsiniz bir `propertyChanged` özelliği denetimde ayarlandığında gibi özelliğinin değeri değiştiğinde çalıştırılacak temsilci. Zaman `propertyChanged` temsilcisini yürütür, başvuru üzerinde bağlı denetimi ve bir özellik için eski ve yeni değerleri içeren parametre geçti. Bu temsilci, özellik, şu şekilde geçirilir başvuru işleyerek iliştirildiği denetlemek için yeni işlevler eklemek için kullanılabilir:

1. `propertyChanged` Temsilci olarak alınan denetim başvurusu bıraktığı bir [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), davranıştır denetim türü artırmak için tasarlanmıştır.
1. `propertyChanged` Temsilci denetimin özelliklerini çekirdek davranışı işlevselliği uygulamak için denetim tarafından gösterilen olaylar için denetim ya da kayıtları olay işleyicisi yöntemleri değiştirir.

Ekli davranışlar ile bunlar içinde tanımlanır çıkıştır bir `static` sınıfı ile `static` özellikleri ve yöntemleri. Bu duruma sahip ekli davranışlar oluşturma zorlaştırır. Buna ek olarak, Xamarin.Forms davranışları ekli davranışlar davranışı oluşturma için tercih edilen yaklaşım olarak değiştirdiniz. Xamarin.Forms davranışları hakkında daha fazla bilgi için bkz. [Xamarin.Forms davranışları](~/xamarin-forms/app-fundamentals/behaviors/creating.md) ve [yeniden kullanılabilir davranışlar](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Ekli bir davranışı oluşturma

Örnek uygulamayı gösterir bir `NumericValidationBehavior`, kullanıcı tarafından girilen değer vurgular bir [ `Entry` ](xref:Xamarin.Forms.Entry) denetim kırmızı renkte değilse bir `double`. Davranışı aşağıdaki kod örneğinde gösterilmiştir:

```csharp
public static class NumericValidationBehavior
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached (
            "AttachBehavior",
            typeof(bool),
            typeof(NumericValidationBehavior),
            false,
            propertyChanged:OnAttachBehaviorChanged);

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
            entry.TextChanged += OnEntryTextChanged;
        } else {
            entry.TextChanged -= OnEntryTextChanged;
        }
    }

    static void OnEntryTextChanged (object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` Sınıfı içeren adlı ekli özelliği `AttachBehavior` ile bir `static` alıcı ve ayarlayıcı, ekleme veya kaldırma davranışı bağlı denetim denetleyen. İliştirilmiş özellik kayıtları `OnAttachBehaviorChanged` özelliğinin değeri değiştiğinde, yürütülecek yöntemi. Bu yöntem kaydeder veya devre dışı bırakmak için bir olay işleyicisi kaydeder [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) değerine göre olay `AttachBehavior` ekli özellik. Davranış temel işlevlerini tarafından sağlanan `OnEntryTextChanged` girilen değeri ayrıştırır yöntemi [ `Entry` ](xref:Xamarin.Forms.Entry) kümeleri ve kullanıcı tarafından `TextColor` özellik değeri değilse, kırmızı bir `double`.

## <a name="consuming-an-attached-behavior"></a>Ekli bir davranış kullanma

`NumericValidationBehavior` Sınıfı ekleyerek kullanılabilir `AttachBehavior` özelliğine bağlı bir [ `Entry` ](xref:Xamarin.Forms.Entry) denetimi aşağıdaki XAML kod örneğinde gösterildiği gibi:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Eşdeğer [ `Entry` ](xref:Xamarin.Forms.Entry) C# ' de aşağıdaki kod örneğinde gösterilir:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Çalışma zamanında davranış göre davranışını uygulama denetimi ile etkileşim için yanıtlar. Aşağıdaki ekran görüntüleri geçersiz girişine yanıt ekli davranış gösterir:

[![](attached-images/screenshots-sml.png "Örnek uygulama ekli davranış")](attached-images/screenshots.png#lightbox "örnek uygulama ekli davranışı")

> [!NOTE]
> Ekli davranışlar belirli denetim türü (veya birçok denetim için uygulayabileceğiniz bir sınıf) yazılır ve uyumlu bir denetime yalnızca eklenmelidir. Uyumsuz bir denetime bir davranış ekleme girişimi bilinmeyen davranışlara neden ve davranışı uygulamasının bağlıdır.

### <a name="removing-an-attached-behavior-from-a-control"></a>Ekli bir davranışı bir denetiminden kaldırma

`NumericValidationBehavior` Sınıfı kaldırılabilir denetimden ayarlayarak `AttachBehavior` özelliğine bağlı `false`, aşağıdaki XAML kod örneğinde gösterildiği gibi:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Eşdeğer [ `Entry` ](xref:Xamarin.Forms.Entry) C# ' de aşağıdaki kod örneğinde gösterilir:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

Çalışma zamanında, `OnAttachBehaviorChanged` yöntemi olacaktır yürütülmesi değerini `AttachBehavior` ekli özelliği `false`. `OnAttachBehaviorChanged` Yöntemi ardından kaldırması için olay işleyicisi [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) olayı, kullanıcı denetimle etkileşim kurdukça davranışı yürütülen olmadığından sağlama.

## <a name="summary"></a>Özet

Bu makalede ekli davranışlar oluşturup nasıl gösterilmiştir. Ekli davranışlar olan `static` sınıflarıyla bir veya daha fazla ekli özellikler.


## <a name="related-links"></a>İlgili bağlantılar

- [Ekli davranışlar (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
