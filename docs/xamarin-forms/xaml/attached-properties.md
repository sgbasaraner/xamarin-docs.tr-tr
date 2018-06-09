---
title: Ekli Özellikler
description: Bu makalede, ekli özellikler bir giriş sağlar ve oluşturmak ve bunları kullanmak gösterilmiştir.
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: e0ecff37eaf615321c7fcdce35e334db89ae631a
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245904"
---
# <a name="attached-properties"></a>Ekli Özellikler

_Ekli özellik bir sınıf tarafından tanımlanan diğer nesnelere bağlı bağlanabilirse özelliği, ancak, özel bir tür ve bir sınıfı içeren bir öznitelik olarak XAML'de tanınabilir ve bir noktayla bir özellik adı ayrılmış. Bu makalede, ekli özellikler bir giriş sağlar ve oluşturmak ve bunları kullanmak gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Özellikleri etkinleştirin, kendi sınıfı tanımlamaz bir özellik için bir değer atamak için bir nesne eklenmiş. Örneğin, alt öğelerini kullanabilirsiniz ekli özellikler nasıl kullanıcı arabiriminde gösterilmesini olduklarından, kendi üst öğesi bildirebilirsiniz. [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Denetimi sağlar satır ve sütun ayarlayarak belirtilmesi için bir alt `Grid.Row` ve `Grid.Column` ekli özellikler. `Grid.Row` ve `Grid.Column` , alt öğelerini ayarlandığından ekli özellikler olan bir `Grid`, yerine `Grid` kendisi.

Ekli özellikler aşağıdaki senaryolarda olarak bağlanabilir özellikleri uygulanması gerekir:

- Özellik ayarı için bir mekanizma kullanılabilir sınıfları dışında olması gerekir olduğunda tanımlama sınıfı.
- Ne zaman sınıfı kolayca diğer sınıflar ile tümleştirilmesini gerektiren bir hizmeti temsil eder.

Bağlanabilir özellikleri hakkında daha fazla bilgi için bkz: [bağlanabilir özellikleri](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Oluşturma ve ekli özellik kullanma

Ekli özellik oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) birini örneğiyle [ `CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi aşırı.
1. Sağlamak `static` `Get` *PropertyName* ve `Set` *PropertyName* ekli özellik için erişimciler olarak yöntemler.

### <a name="creating-a-property"></a>Bir özelliği oluşturma

Kullanım için eklenen bir özellik diğer türlerinde oluştururken, özellik oluşturulduğu sınıf türetin yok [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/). Ancak, *hedef* özelliği erişimcisi için olması veya, türetilen [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

Ekli özellik bildirerek oluşturulabilmesi için bir `public static readonly` türündeki özelliği [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Bağlanabilir özelliği birinin döndürülen değerine ayarlanmalıdır [ `BindableProperty.CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi aşırı. Bildirim sahip olan sınıfın gövdesi, tüm üye tanımları dışında içinde olmalıdır.

Aşağıdaki kod iliştirilmiş bir özellik örneği gösterilmektedir:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Bu adlı iliştirilmiş bir özellik oluşturur `HasShadow`, türü `bool`. Özelliği tarafından sahip olunan `ShadowEffect` sınıfı ve varsayılan değerine sahip `false`. Ekli özellikler için adlandırma kuralı ekli özellik tanımlayıcısı olarak belirtilen özellik adı eşleşmelidir olduğu `CreateAttached` yöntemiyle "eklenmiş özellik". Bu nedenle, yukarıdaki örnekte ekli özellik tanımlayıcısıdır `HasShadowProperty`.

Oluşturma sırasında belirlenen parametreleri de dahil olmak üzere bağlanabilir özellikleri oluşturma hakkında daha fazla bilgi için bkz: [oluşturma ve bağlanabilirse özelliği tüketen](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Erişimciler oluşturma

Statik `Get` *PropertyName* ve `Set` *PropertyName* erişimciler ekli özellik için farklı yöntemleri gereklidir, aksi takdirde özellik sistemi kullanamadı olacaktır ekli özellik. `Get` *PropertyName* erişimci aşağıdaki imzası uyması:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName* erişimci ilgili bulunan değer döndürmelidir `BindableProperty` ekli özellik için alan. Bu çağırarak sağlanabilir [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) bağlanabilirse özellik tanımlayıcısı değeri almak istediğiniz geçirme ve çıkan değeri gerekli tür atama yöntemi.

`Set` *PropertyName* erişimci aşağıdaki imzası uyması:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName* erişimci, buna karşılık gelen değer ayarlanmalıdır `BindableProperty` ekli özellik için alan. Bu çağırarak sağlanabilir [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) yöntemi, değer ve değeri ayarlamak için ayarlanacak üzerinde bağlanabilirse özelliği tanımlayıcısını geçirme.

Her iki erişimciler için *hedef* nesne olması veya, türetilen [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

Aşağıdaki kod örneğinde erişimcileri gösterir `HasShadow` özelliği eklenmiş:

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### <a name="consuming-an-attached-property"></a>Ekli özellik kullanma

Ekli özellik oluşturulduktan sonra XAML veya kod tüketilebilir. XAML'de ve ad alanı bildiriminin ortak dil çalışma zamanı (CLR) ad alanı adı ve isteğe bağlı olarak bir derleme adı gösteren bir ad alanı öneki, bildirerek sağlanır. Daha fazla bilgi için bkz: [XAML ad uzayları](~/xamarin-forms/xaml/namespaces.md).

Aşağıdaki kod örneğinde özel türüne başvuran uygulama kodu aynı bütünleştirilmiş içinde tanımlanan iliştirilmiş bir özellik içeren özel bir tür için XAML ad uzayı gösterilmektedir:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Ad alanı bildirimi ardından olarak belirli bir denetimde ekli özellik ayarı aşağıdaki XAML kod örneğinde gösterildiği kullanıldığında:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Ekli bir özelliği bir stil ile kullanma

Ekli özellikler denetime stili tarafından da eklenebilir. Aşağıdaki XAML kodu örnekte gösterildiği bir *açık* kullanan stili `HasShadow` uygulanabilir özelliği, eklenmiş [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetimleri:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Uygulanabilir bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ayarlayarak kendi [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) özelliğine `Style` kullanarakörnek`StaticResource`biçimlendirme uzantısı, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Stilleri hakkında daha fazla bilgi için bkz: [stilleri](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Gelişmiş senaryolar

Ekli özellik oluştururken, bir dizi Gelişmiş ekli özellik senaryoları etkinleştirmek için ayarlanabilen isteğe bağlı parametre vardır. Bu özellik değişiklikleri algılama, özellik değerlerini doğrulamak ve özellik değerlerini zorlama içerir. Daha fazla bilgi için bkz: [Gelişmiş senaryoları](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Özet

Bu makalede, ekli özellikler giriş sağlanan ve oluşturmak ve bunları kullanmak nasıl gösterilmektedir. Ekli özellik bağlanabilir özelliği, ancak diğer nesnelere bağlı ve XAML'de tanınabilir bir sınıftaki bir sınıf ve bir noktayla ayrılmış bir özellik adı içeren öznitelikler tanımlanan özel bir türde değil.


## <a name="related-links"></a>İlgili bağlantılar

- [Bağlanabilir Özellikler](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML Ad Alanları](~/xamarin-forms/xaml/namespaces.md)
- [Gölge efekti (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
