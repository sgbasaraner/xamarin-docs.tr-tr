---
title: Bağlanabilir özellikleri
description: Xamarin.Forms içinde ortak dil çalışma zamanı (CLR) özellikleri işlevselliğini bağlanabilir özellikleri tarafından genişletilir. Bir bağlanabilirse özel türde bir özellik, özelliğin değerini Xamarin.Forms özellik sistemi tarafından nerede izlenen özelliğidir. Bu makalede bağlanabilirse özelliklerine bir giriş sağlar ve oluşturmak ve bunları kullanmak gösterilmiştir.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 7e1d3c82036ef703014ae548a6719937e89d22f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="bindable-properties"></a>Bağlanabilir özellikleri

_Xamarin.Forms içinde ortak dil çalışma zamanı (CLR) özellikleri işlevselliğini bağlanabilir özellikleri tarafından genişletilir. Bir bağlanabilirse özel türde bir özellik, özelliğin değerini Xamarin.Forms özellik sistemi tarafından nerede izlenen özelliğidir. Bu makalede bağlanabilirse özelliklerine bir giriş sağlar ve oluşturmak ve bunları kullanmak gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Bağlanabilir özellikleri sahip bir özellik yedekleyerek CLR özellik işlevselliğini genişletmek bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) bir alana sahip bir özellik yedekleme yerine türü. Amacı bağlanabilir özellikleri, veri bağlama, stiller, şablonları destekleyen bir özellik sistemi sağlamaktır ve üst-alt ilişkisi değerlerini ayarlayın. Ayrıca, bağlanabilir özellikler varsayılan değerleri, özellik değerlerinin özellik değişikliklerini izlemek geri aramalar ve doğrulama sağlayabilir.

Bir veya daha fazla aşağıdaki özellikleri desteklemek için bağlanabilir özellikleri olarak özellikleri uygulanması gerekir:

- Geçerli bir işlev gören *hedef* veri bağlama için özelliği.
- Özelliği üzerinden ayarını bir [stili](~/xamarin-forms/user-interface/styles/index.md).
- Özelliğin türü için varsayılandan farklı bir varsayılan özellik değeri sağlama.
- Özelliğinin değeri doğrulanıyor.
- Özellik değişiklikleri izleme.

Xamarin.Forms bağlanabilir özellikleri örneklerindendir [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/), [ `Button.BorderRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/), ve [ `StackLayout.Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/). Karşılık gelen bir bağlanabilirse her bir özellik vardır `public static readonly` türündeki özelliği [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) aynı sınıf içinde gösterilir ve bağlanabilirse özelliği tanıtıcısı olmasıdır. Örneğin, karşılık gelen bağlanabilirse özellik tanımlayıcısı için `Label.Text` özelliği [ `Label.TextProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Label.TextProperty/).

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>Oluşturma ve bağlanabilirse özelliğini kullanma

Bağlanabilir özelliği oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) birini örneğiyle [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi aşırı.
1. Özellik erişimcisi için tanımlamak [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) örneği.

Unutmayın tüm [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) örnekleri kullanıcı Arabirimi iş parçacığı üzerinde oluşturulmalıdır. Başka bir deyişle, yalnızca kullanıcı Arabirimi iş parçacığı üzerinde çalışan bir kod almak veya bağlanabilir özelliğinin değerini ayarlayın. Ancak, `BindableProperty` örnekleri erişilebilir diğer iş parçacıklarından kullanıcı Arabirimi iş parçacığı ile hazırlama tarafından [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) yöntemi.

### <a name="creating-a-property"></a>Bir özelliği oluşturma

Oluşturmak için bir `BindableProperty` örneği içeren sınıf öğesinden türetilmelidir [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) sınıfı. Ancak, `BindableObject` sınıfı olduğundan sınıf hiyerarşisi yüksek sınıfların çoğu kullanıcı arabirimi işlevselliği desteği bağlanabilir özellikleri için kullanılır.

Bağlanabilir özelliği bildirerek oluşturulabilmesi için bir `public static readonly` türündeki özelliği [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Bağlanabilir özelliği birinin döndürülen değerine ayarlanmalıdır [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi aşırı. Bildirim gövdesi içinde olmalıdır [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) türetilmiş sınıf, ancak hiçbir üye tanımları dışında.

En azından bir tanımlayıcı oluştururken belirtilmelidir bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/), aşağıdaki parametreleri birlikte:

- Adını [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/).
- Özelliğin türü.
- Sahibi olan nesnenin türü.
- Özelliğin varsayılan değeri. Bu özellik belirli varsayılan bir değer ayarlanmamış olduğunda ve özelliğin türü için varsayılan değer farklı olabilir her zaman döndürdüğünü sağlar. Varsayılan değer olacaktır geri [ `ClearValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.ClearValue/p/Xamarin.Forms.BindableProperty/) yöntemi bağlanabilirse özelliği çağrılır.

Aşağıdaki kod örneği bir tanımlayıcı ve dört gerekli parametrelerin değerlerini bağlanabilir bir özelliğin gösterir:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Bu oluşturur bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) adlı örneği `EventName`, türü `string`. Özelliği tarafından sahip olunan `EventToCommandBehavior` sınıfı ve varsayılan değerine sahip `null`. Bağlanabilir özellik tanımlayıcı olarak belirtilen özellik adı eşleşmelidir bağlanabilir özellikleri için adlandırma kuralı olan `Create` yöntemiyle "eklenmiş özellik". Bu nedenle, yukarıdaki örnekte bağlanabilirse özelliği tanımlayıcısıdır `EventNameProperty`.

İsteğe bağlı olarak, oluşturma sırasında bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) örneği, aşağıdaki parametreleri belirtilebilir:

- Bağlama modu. Bu özellik değeri değişiklikleri yaymak yönünü belirtmek için kullanılır. Varsayılan bağlama modunda gelen değişiklikler yayılır *kaynak* için *hedef*.
- Özellik değeri ayarladığınızda çağrılacak bir doğrulama temsilcisi. Daha fazla bilgi için bkz: [doğrulamalarının](#validation).
- Özellik değeri değiştiğinde çağrılan temsilci bir özelliği değiştirildi. Daha fazla bilgi için bkz: [algılama özelliği değişiklikleri](#propertychanges).
- Özellik değeri değiştirdiğinizde, çağrılacak temsilci değiştirme özelliği. Bu temsilci özellik değişti temsilci olarak aynı imzaya sahip.
- Özellik değeri değiştiğinde çağrılan bir coerce değeri temsilcisi. Daha fazla bilgi için bkz: [Coerce değeri geri çağırmaları](#coerce).
- A `Func` varsayılan özellik değeri başlatmak için kullanılır. Daha fazla bilgi için bkz: [varsayılan bir değeri ile bir Func oluşturma](#defaultfunc).

### <a name="creating-accessors"></a>Erişimciler oluşturma

Özellik erişimcisi bağlanabilir bir özelliğe erişmek için özellik sözdizimi kullanması gerekir. `Get` Erişimci karşılık gelen bağlanabilirse özelliğinde bulunan değer döndürmelidir. Bu çağırarak sağlanabilir [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) bağlanabilirse özellik tanımlayıcısı değeri almak istediğiniz geçirme ve gerekli tür sonucu atama yöntemi. `Set` Erişimci karşılık gelen bağlanabilirse özelliğinin değeri ayarlanmış olmalıdır. Bu çağırarak sağlanabilir [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) yöntemi, değer ve değeri ayarlamak için ayarlanacak üzerinde bağlanabilirse özelliği tanımlayıcısını geçirme.

Aşağıdaki kod örneğinde erişimcileri gösterir `EventName` bağlanabilirse özelliği:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>Bağlanabilir özelliğini kullanma

Bağlanabilir özelliği oluşturulduktan sonra XAML veya kod tüketilebilir. XAML'de ve ad alanı bildiriminin CLR ad ve isteğe bağlı olarak, bir derleme adı gösteren bir ad alanı öneki, bildirerek sağlanır. Daha fazla bilgi için bkz: [XAML ad uzayları](~/xamarin-forms/xaml/namespaces.md).

Aşağıdaki kod örneğinde özel türüne başvuran uygulama kodu aynı bütünleştirilmiş içinde tanımlanan bir bağlanabilirse özelliği içeren özel bir tür için XAML ad uzayı gösterilmektedir:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Ad alanı bildirimi ayarlarken kullanılan `EventName` aşağıdaki XAML kod örneğinde kanıtlanabilir olarak bağlanabilir özelliği:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>Gelişmiş senaryolar

Oluştururken bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Bu örnek, bir dizi Gelişmiş bağlanabilirse özellik senaryoları etkinleştirmek için ayarlanabilen isteğe bağlı parametre vardır. Bu bölümde bu senaryoları araştırır.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>Özellik değişiklikleri algılama

A `static` özelliği değiştirildi geri çağırma yöntemi kaydedilebilir bağlanabilirse özelliğiyle belirterek `propertyChanged` parametresi için [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi. Bağlanabilir özelliğinin değeri değiştiğinde belirtilen geri çağırma yöntemi çağrılır.

Aşağıdaki örnekte gösterildiği kod nasıl `EventName` bağlanabilirse özelliği yazmaçlar `OnEventNameChanged` yöntemi bir özelliği değiştirildi geri çağırma yöntemi olarak:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

Özellik değişti geri çağırma yöntemi [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) parametresi hangi sahip olan sınıfın örneğini bir değişiklik ve iki değerlerini bildirdi belirtmek için kullanılır `object` parametreleri eski ve yeni değerlerini temsil eder bağlanabilir özelliği.

<a name="validation" />

### <a name="validation-callbacks"></a>Doğrulamalarının

A `static` doğrulama geri çağırma yöntemi kaydedilebilir bağlanabilirse özelliğiyle belirterek `validateValue` parametresi için [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi. Bağlanabilir özelliğinin değeri olarak ayarlandığında belirtilen geri çağırma yöntemi çağrılır.

Aşağıdaki örnekte gösterildiği kod nasıl `Angle` bağlanabilirse özelliği yazmaçlar `IsValidValue` yöntemi bir doğrulama geri çağırma yöntemi olarak:

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

Doğrulama geri çağırmaları bir değerle sağlanır ve döndürmelidir `true` değer özelliği için geçerli değilse `false`. Doğrulama geri çağırma döndürürse, özel durum oluşturuldu `false`, hangi işlenecek geliştirici tarafından. Bağlanabilir özelliği ayarlı olduğunda doğrulama geri çağırma yöntemi, tipik kullanımını tamsayı veya double değerlerini kısıtlamadır. Örneğin, `IsValidValue` yöntemi özellik değeri olup olmadığını denetler bir `double` 0-360 aralığında.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>Coerce değeri geri çağrıları

A `static` coerce değeri geri çağırma yöntemi kaydedilebilir bağlanabilirse özelliğiyle belirterek `coerceValue` parametresi için [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi. Bağlanabilir özelliğinin değeri değiştiğinde belirtilen geri çağırma yöntemi çağrılır.

Coerce değeri geri aramalar özelliğinin değeri değiştiğinde yeniden değerlendirme bağlanabilirse özelliğinin zorlamak için kullanılır. Örneğin, bir coerce değeri geri bir bağlanabilirse özelliğinin değeri başka bir bağlanabilirse özellik değerinden büyük olmadığından emin olun için kullanılabilir.

Aşağıdaki örnekte gösterildiği kod nasıl `Angle` bağlanabilirse özelliği yazmaçlar `CoerceAngle` yöntemi coerce değeri geri çağırma metodu olarak:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle) {
    input = homePage.MaximumAngle;
  }

  return input;
}
```

`CoerceAngle` Yöntemi denetler değerini `MaximumAngle` özelliği ve `Angle` özellik değeri, büyük olduğundan, değer olacak şekilde zorlar `MaximumAngle` özellik değeri.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Varsayılan değer Func ile oluşturma

A `Func` bağlanabilirse özelliğinin varsayılan değeri başlatmak için aşağıdaki kod örneğinde gösterildiği gibi kullanılabilir:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` Parametre olarak ayarlanmış bir `Func` , çağırır [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) döndürülecek yöntemi bir `double` üzerinde kullanılan yazı tipi için adlandırılmış boyutu temsil eden bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) yerel platformunda.

## <a name="summary"></a>Özet

Bu makalede bağlanabilir özellikleri giriş sağlanan ve oluşturmak ve bunları kullanmak nasıl gösterilmektedir. Bir bağlanabilirse özel türde bir özellik, özelliğin değerini Xamarin.Forms özellik sistemi tarafından nerede izlenen özelliğidir.


## <a name="related-links"></a>İlgili bağlantılar

- [XAML Ad Alanları](~/xamarin-forms/xaml/namespaces.md)
- [Olayı için komut davranışı (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Doğrulama geri çağırma (örnek)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce değeri geri çağırma (örnek)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
