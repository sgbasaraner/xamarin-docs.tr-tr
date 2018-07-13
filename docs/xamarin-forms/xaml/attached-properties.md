---
title: Ekli Özellikler
description: Bu makalede ekli özelliklerini tanıtır ve oluşturmak ve bunları kullanma işlemi gösterilmektedir.
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 981e59fe3ba8c63d0f6c6a067ceb9f338a02da8f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997338"
---
# <a name="attached-properties"></a>Ekli Özellikler

_Ekli özelliği bir özel bir sınıf tarafından tanımlanan, ancak diğer nesnelere bağlı bağlanılabilir özellik türüdür, tanınan bir öznitelik olarak XAML içinde bir sınıf içeren ve bir özellik adı noktayla ayrılmış. Bu makalede ekli özelliklerini tanıtır ve oluşturmak ve bunları kullanma işlemi gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Özellikleri etkinleştirmek, kendi sınıf tanımlamıyor bir özellik için bir değer atamak için bir nesne eklenmiş. Örneğin, alt öğelerini kullanabilirsiniz iliştirilmiş özellikler nasıl kullanıcı arabiriminde sunulacak olduklarından, bunların üst öğe bildirmek için. [ `Grid` ](xref:Xamarin.Forms.Grid) Satır ve sütunları ayarlayarak belirtilmesi için bir alt denetimin izin `Grid.Row` ve `Grid.Column` iliştirilmiş özellikler. `Grid.Row` ve `Grid.Column` alt öğelerde ayarlandığından iliştirilmiş özellikler olan bir `Grid`, yerine `Grid` kendisi.

Bağlanabilir özellikler, aşağıdaki senaryolarda ekli özellikler olarak uygulanmalıdır:

- Özellik ayarı için bir mekanizma kullanılabilir sınıfların dışında olması gerekir olduğunda tanımlayan sınıf.
- Zaman sınıfın diğer sınıflarla kolayca tümleştirilebilen gereken bir hizmet temsil eder.

Bağlanabilir özellikler hakkında daha fazla bilgi için bkz: [bağlanabilir Özellikler](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Oluşturma ve ekli özelliği kullanma

Ekli özelliği oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) örneği biriyle [ `CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached*) yöntemi aşırı yüklemeleri.
1. Sağlamak `static` `Get` *PropertyName* ve `Set` *PropertyName* iliştirilmiş özellik erişimcileri olarak yöntemler.

### <a name="creating-a-property"></a>Bir özelliği oluşturma

Özellik oluşturulduğu sınıf türetmek yok ekli özelliği kullanmak için diğer türlerde oluştururken [ `BindableObject` ](xref:Xamarin.Forms.BindableObject). Ancak, *hedef* özellik erişimcileri için olması veya, türetilen [ `BindableObject` ](xref:Xamarin.Forms.BindableObject).

Ekli özelliği bildirerek oluşturulabilir. bir `public static readonly` türünün özelliği [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty). Döndürülen değeri şunlardan biri olarak bağlanılabilir özellik ayarlanmalıdır [ `BindableProperty.CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) yöntemi aşırı yüklemeleri. Bildirimi gövdesi sahip olan sınıfın, ancak herhangi bir üye tanımı içinde olmalıdır.

Aşağıdaki kod örneği ekli özelliği gösterir:

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

Bu adlı ekli özelliği oluşturur `HasShadow`, türü `bool`. Özelliği tarafından sahip olunan `ShadowEffect` sınıfı ve varsayılan değeri olan `false`. İliştirilmiş özellik tanımlayıcısı olarak belirtilen özellik adı eşleşmelidir iliştirilmiş özellikler için adlandırma kuralı olup `CreateAttached` yöntemiyle eklenmiş "özelliği". Bu nedenle, yukarıdaki örnekte ekli özellik tanımlayıcısıdır `HasShadowProperty`.

Bağlanabilir özellikler, oluşturma sırasında belirtilebilen parametreleri de dahil olmak üzere oluşturma hakkında daha fazla bilgi için bkz. [oluşturma ve tüketme bağlanılabilir özellik](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Erişimciler oluşturma

Statik `Get` *PropertyName* ve `Set` *PropertyName* iliştirilmiş özellik erişimcileri olarak yöntemleri gereklidir, aksi takdirde, özellik sistemi kullanamaz olacaktır ekli özellik. `Get` *PropertyName* erişimci aşağıdaki imza uyması:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName* erişimci ilgili yer alan bir değer döndürmelidir `BindableProperty` alanına ekli özellik. Bu çağrı yaparak da gerçekleştirilebilir [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) bağlanılabilir özellik tanımlayıcı değeri almak istediğiniz geçirme ve ardından sonuç değerini gerekli türüne atama yöntemi.

`Set` *PropertyName* erişimci aşağıdaki imza uyması:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName* erişimci karşılık gelen değer ayarlanmalıdır `BindableProperty` alanına ekli özellik. Bu çağrı yaparak da gerçekleştirilebilir [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) yöntemi, değer ve ayarlanacak değer ayarlanacağı bağlanılabilir özellik tanımlayıcı geçirme.

Her iki erişimcisi için *hedef* nesne olması veya, türetilen [ `BindableObject` ](xref:Xamarin.Forms.BindableObject).

Aşağıdaki kod örneği için erişimciler gösterir `HasShadow` ekli özellik:

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

### <a name="consuming-an-attached-property"></a>Ekli özelliği kullanma

Ekli özelliği oluşturulduktan sonra XAML ya da kod tüketilebilir. XAML içinde bir ad alanı öneki, ortak dil çalışma zamanı (CLR) ad alanı adı ve isteğe bağlı bir derleme adı belirten ad alanı bildirimi ile bildirerek sağlanır. Daha fazla bilgi için [XAML ad alanları](~/xamarin-forms/xaml/namespaces.md).

Aşağıdaki kod örneği, özel türe başvuran uygulama kodu ile aynı bütünleştirilmiş kod içinde tanımlanan bir ekli özellik içeren özel bir tür için XAML ad alanı gösterilmektedir:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Ad alanı bildiriminin ardından aşağıdaki XAML kod örneğinde gösterilen olarak belirli bir denetimde ekli özellik ayarı kullanılır:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Ekli özelliği bir stil ile kullanma

Ekli özellikler tarafından bir stil de denetime eklenebilir. Aşağıdaki XAML kod örnekte gösterildiği bir *açık* kullanan stili `HasShadow` uygulanabilir özelliği, bağlı [ `Label` ](xref:Xamarin.Forms.Label) denetimler:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style) Uygulanabilir bir [ `Label` ](xref:Xamarin.Forms.Label) ayarlayarak onun [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) özelliğini `Style` kullanarak`StaticResource`işaretleme uzantısı, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Stilleri hakkında daha fazla bilgi için bkz. [stilleri](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Gelişmiş senaryolar

Ekli özelliği oluştururken, bir dizi Gelişmiş ekli özellik senaryoları etkinleştirmek için ayarlayabileceğiniz isteğe bağlı parametre vardır. Bu özellik değişiklikleri algılama, özellik değerlerini doğrulamak ve zorlama özellik değerlerini içerir. Daha fazla bilgi için [Gelişmiş senaryoları](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Özet

Bu makalede ekli özellikler giriş sağlanan ve oluşturma ve bunları tüketme gösterilmiştir. Ekli özelliği bağlanılabilir özellik, ancak diğer nesnelere bağlı ve XAML içinde tanınan bir sınıftaki bir sınıf ya da bir nokta ile ayrılmış bir özellik adını içeren öznitelikler tanımlanan özel bir türüdür.


## <a name="related-links"></a>İlgili bağlantılar

- [Bağlanabilir Özellikler](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML Ad Alanları](~/xamarin-forms/xaml/namespaces.md)
- [Gölge efektini (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
