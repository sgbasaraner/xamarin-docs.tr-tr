---
title: "Ekli davranışları"
description: "Ekli davranışları, bir veya daha fazla ekli özellikler ile statik sınıflarıdır. Bu makalede, oluşturmasına ve ekli davranışları kullanmasına gösterilmiştir."
ms.topic: article
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 9751b39987819428f93e09d4bfb6bee261604bb5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="attached-behaviors"></a>Ekli davranışları

_Ekli davranışları, bir veya daha fazla ekli özellikler ile statik sınıflarıdır. Bu makalede, oluşturmasına ve ekli davranışları kullanmasına gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Ekli özellik bağlanabilirse özelliği özel bir türde değil. Bir sınıf tarafından tanımlanan ancak diğer nesnelere bağlı ve bunlar sınıfı ve bir noktayla ayrılmış bir özellik adı içeren öznitelikler olarak XAML'de tanımlanabilir.

Ekli özellik tanımlayabilirsiniz bir `propertyChanged` özelliğinin değeri değiştiğinde özelliği bir denetimde ayarlandığında gibi yürütülecek temsilcisi. Zaman `propertyChanged` temsilcisini yürütür, üzerinde bağlı denetim ve özellik için eski ve yeni değerler içeren parametreleri başvuru geçti. Bu temsilci özelliği, şu şekilde geçirilir başvuru işleyerek bağlı denetlemek için yeni işlevler eklemek için kullanılabilir:

1. `propertyChanged` Temsilci olarak alınan denetim başvurusu bıraktığı bir [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), geliştirmek için tasarlanmış davranış denetim türü.
1. `propertyChanged` Temsilci denetim özelliklerini denetim veya kayıtları olay işleyicileri çekirdek davranışı işlevselliği uygulamak için denetim tarafından kullanıma sunulan olaylar için çağrı yöntemlerini değiştirir.

Bunlar içinde tanımlanan ekli davranışları ile ilgili bir sorun olduğu bir `static` sınıfı ile `static` özellikleri ve yöntemleri. Bu duruma sahip ekli davranışları oluşturmak zorlaştırır. Ayrıca, Xamarin.Forms davranışları davranışı yapımı için tercih edilen yaklaşım olarak ekli davranışları yerini almıştır. Xamarin.Forms davranışları hakkında daha fazla bilgi için bkz: [Xamarin.Forms davranışları](~/xamarin-forms/app-fundamentals/behaviors/creating.md) ve [yeniden kullanılabilir davranışları](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Ekli davranışı oluşturma

Örnek uygulamayı gösteren bir `NumericValidationBehavior`, kullanıcı tarafından girilen değer vurgular bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) değilse kırmızı olarak kontrol bir `double`. Aşağıdaki kod örneğinde davranışı gösterilir:

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

`NumericValidationBehavior` Sınıfı içeren adlı bir ekli özellik `AttachBehavior` ile bir `static` alıcı ve ekleme veya kaldırma için bunu ekleneceği denetlemek için davranış denetimleri ayarlayıcı. Bu özellik yazmaçlar bağlı `OnAttachBehaviorChanged` özelliğinin değeri değiştiğinde, yürütülecek yöntemi. Bu yöntem kaydeder veya bir olay işleyicisi için XML'deki kaydeder [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) değerine göre olay `AttachBehavior` özelliği eklenmiş. Davranış çekirdek işlevselliğini tarafından sağlanan `OnEntryTextChanged` girilen değeri ayrıştırır yöntemi [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) kullanıcı ve kümeleri tarafından `TextColor` özelliğini değer değilse kırmızı bir `double`.

## <a name="consuming-an-attached-behavior"></a>Ekli bir davranış kullanma

`NumericValidationBehavior` Sınıfı ekleyerek nin tüketim `AttachBehavior` özelliğine bağlı bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) aşağıdaki XAML kod örneğinde gösterildiği gibi denetim:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Eşdeğer [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) C# ' de aşağıdaki kod örneğinde gösterilir:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Çalışma zamanında göre davranışı uygulama denetimi ile etkileşim için davranış yanıt. Aşağıdaki ekran görüntüleri için geçersiz giriş yanıt ekli davranış gösterir:

[ ![](attached-images/screenshots-sml.png "Örnek uygulama ekli davranışı")](attached-images/screenshots.png "örnek uygulama ekli davranışı")

> [!NOTE]
> **Not**: ekli davranışları, belirli bir denetim türünü (veya birçok denetimlere uygulayabileceğiniz bir üst sınıf) yazılır ve bunlar yalnızca uyumlu bir denetim eklenmesi gerekir. Uyumsuz bir denetim için bir davranış eklemeye çalışırken bilinmeyen davranışlara neden ve davranış mantığınız bağlıdır.

### <a name="removing-an-attached-behavior-from-a-control"></a>Ekli bir davranış denetimden kaldırma

`NumericValidationBehavior` Sınıfı kaldırılabilir denetimden ayarlayarak `AttachBehavior` özelliğine bağlı `false`, aşağıdaki XAML kod örneğinde gösterildiği gibi:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Eşdeğer [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) C# ' de aşağıdaki kod örneğinde gösterilir:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

Çalışma zamanında `OnAttachBehaviorChanged` yöntemi olacaktır yürütülmesi değerini `AttachBehavior` ekli özellik ayarlanmış `false`. `OnAttachBehaviorChanged` Yöntemi ardından XML'deki kaydetmek için olay işleyicisini [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) olayı, kullanıcı denetimi ile etkileşim gibi davranış yürütülen değil sağlama.

## <a name="summary"></a>Özet

Bu makalede nasıl oluşturulacağını ve ekli davranışları tüketen gösterilmektedir. Ekli davranışları olan `static` bir veya daha fazla ekli özellikler sınıflarıyla.


## <a name="related-links"></a>İlgili bağlantılar

- [Ekli davranışları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
