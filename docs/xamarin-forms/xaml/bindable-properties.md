---
title: Bağlanabilir Özellikler
description: Bu makalede bağlanabilir özellikler tanıtır ve oluşturmak ve bunları kullanma işlemi gösterilmektedir.
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 115fff5f80eb531780aa208fde677b26b69e9294
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935634"
---
# <a name="bindable-properties"></a>Bağlanabilir Özellikler

_Xamarin.Forms içinde bağlanabilir özellikler tarafından ortak dil çalışma zamanı (CLR) özellikleri işlevselliği genişletilir. Bağlanılabilir özellik bir özel özellik, özelliğin değerini Xamarin.Forms özellik sistemi tarafından izlenen burada türüdür. Bu makalede bağlanabilir özellikler tanıtır ve oluşturmak ve bunları kullanma işlemi gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Bağlanabilir özellikler özelliği ile yedekleyerek CLR özellik işlevselliğini genişleten bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) yedekleme alanı olan bir özelliği yerine türü. Bağlanabilir özellikler amacı, veri bağlama, stilleri ve şablonları destekleyen bir özellik sistemi sağlamaktır ve üst-alt ilişkileri değerlerini ayarlayın. Ayrıca, bağlanabilir özellikler varsayılan değerleri, özellik değerlerini özellik değişikliklerini izleme geri aramaları ve doğrulama sağlar.

Bir veya daha fazla aşağıdaki özellikleri desteklemek için bağlanabilir özellikler uygulanan özellikler:

- Geçerli davranan *hedef* veri bağlama için özelliği.
- Özelliği aracılığıyla bir [stil](~/xamarin-forms/user-interface/styles/index.md).
- Özelliğin türü için varsayılan farklı bir varsayılan özellik değeri sağlayarak.
- Özelliğinin değeri doğrulanıyor.
- Özellik değişiklikleri izleme.

Xamarin.Forms bağlanabilir özellikler örnekler [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/), [ `Button.BorderRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/), ve [ `StackLayout.Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/). Karşılık gelen her bağlanılabilir özellik sahip `public static readonly` türünün özelliği [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) aynı sınıf kullanıma sunulan ve bağlanılabilir özellik tanımlayıcısı. Örneğin, karşılık gelen bağlanılabilir özellik tanımlayıcısı `Label.Text` özelliği [ `Label.TextProperty` ](xref:Xamarin.Forms.Label.TextProperty).

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>Oluşturma ve bağlanabilir özelliği kullanma

Bağlanılabilir özellik oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) örneği biriyle [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi aşırı yüklemeleri.
1. İçin özellik erişimcileri tanımlayın [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) örneği.

Unutmayın tüm [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) örnekleri UI iş parçacığı üzerinde oluşturulmalıdır. Başka bir deyişle, kullanıcı Arabirimi iş parçacığında çalıştırılan kod alın veya bağlanılabilir özellik değerini ayarlayın. Ancak, `BindableProperty` örnekleri erişilebilir diğer iş parçacıklarından ile UI iş parçacığına düzenlemesi [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) yöntemi.

### <a name="creating-a-property"></a>Bir özelliği oluşturma

Oluşturmak için bir `BindableProperty` örneğini içeren sınıfın türetilmesi gereken [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) sınıfı. Ancak, `BindableObject` sınıflarının çoğu kullanıcı arabirimi işlevleri destek bağlanabilir özellikler için kullanılan bu nedenle sınıf sınıf hiyerarşisini, yüksek.

Bağlanılabilir özellik bildirerek oluşturulabilir. bir `public static readonly` türünün özelliği [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Döndürülen değeri şunlardan biri olarak bağlanılabilir özellik ayarlanmalıdır [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi aşırı yüklemeleri. Bildirimi gövdesi içinde olmalıdır [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) türetilmiş sınıf, ancak herhangi bir üye tanımı dışında.

En az bir tanımlayıcı oluştururken belirtilmelidir bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/), aşağıdaki parametreleri birlikte:

- Adını [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/).
- Özelliğin türü.
- Sahip olan nesnenin türü.
- Bir özellik için varsayılan değeri. Bu özellik belirli bir varsayılan değer ayarlama ve özellik türü için varsayılan değerden farklı olabilir her zaman döndürdüğünü sağlar. Varsayılan değer olacaktır geri [ `ClearValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.ClearValue/p/Xamarin.Forms.BindableProperty/) bağlanabilir özelliği yöntemi çağrılır.

Aşağıdaki kod örneği bir tanımlayıcı ve dört gerekli parametrelerin değerleri bir bağlanılabilir özellik gösterir:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

Bu, oluşturur bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) adlı örnek `EventName`, türü `string`. Özelliği tarafından sahip olunan `EventToCommandBehavior` sınıfı ve varsayılan değeri olan `null`. Bağlanabilir özellikler için adlandırma kuralı bağlanılabilir özellik tanımlayıcısı olarak belirtilen özellik adı kullanılmasıdır `Create` yöntemiyle eklenmiş "özelliği". Bu nedenle, yukarıdaki örnekte bağlanılabilir özellik tanımlayıcısıdır `EventNameProperty`.

İsteğe bağlı olarak, oluştururken bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) örnek, aşağıdaki parametreleri belirtilebilir:

- Bağlama modu. Bu, özellik değeri değiştiğinde yayılır yönünü belirtmek için kullanılır. Varsayılan bağlama modunda değişiklikleri gelen yayan *kaynak* için *hedef*.
- Özellik değeri ayarlandığında çağrılacak bir doğrulama temsilcisi. Daha fazla bilgi için [doğrulama geri çağırmaları](#validation).
- Özellik değeri değiştiğinde çağrılacak temsilci bir özelliği değiştirildi. Daha fazla bilgi için [özellik değişiklikleri algılama](#propertychanges).
- Bir özellik özellik değeri değiştiğinde çağrılacak temsilci değiştirme. Bu temsilci, değiştirilen özellik temsilci aynı imzaya sahiptir.
- Özellik değeri değiştiğinde çağrılacak bir coerce değeri temsilcisi. Daha fazla bilgi için [Coerce değeri geri aramaları](#coerce).
- A `Func` varsayılan özellik değeri başlatmak için kullanılır. Daha fazla bilgi için [varsayılan bir değer ile bir işlev oluşturma](#defaultfunc).

### <a name="creating-accessors"></a>Erişimciler oluşturma

Özellik erişimcisi bağlanabilir bir özelliğe erişmek için özelliği sözdizimini kullanmak için gereklidir. `Get` Erişimci karşılık gelen bağlanılabilir özellik içinde yer alan değeri döndürmelidir. Bu çağrı yaparak da gerçekleştirilebilir [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) bağlanılabilir özellik tanımlayıcı değeri almak istediğiniz geçirme ve ardından sonucu gerekli türüne atama yöntemi. `Set` Erişimci karşılık gelen bağlanılabilir özellik değeri ayarlanmalıdır. Bu çağrı yaparak da gerçekleştirilebilir [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) yöntemi, değer ve ayarlanacak değer ayarlanacağı bağlanılabilir özellik tanımlayıcı geçirme.

Aşağıdaki kod örneği için erişimciler gösterir `EventName` bağlanılabilir özellik:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>Bağlanılabilir özellik kullanma

Bağlanılabilir özellik oluşturulduktan sonra XAML ya da kod tüketilebilir. XAML içinde bir ad alanı öneki, CLR ad alanı adı ve isteğe bağlı olarak, bir derleme adı belirten ad alanı bildirimi ile bildirerek sağlanır. Daha fazla bilgi için [XAML ad alanları](~/xamarin-forms/xaml/namespaces.md).

Aşağıdaki kod örneği, özel türe başvuran uygulama kodu ile aynı bütünleştirilmiş kod içinde tanımlanan bir bağlanılabilir özellik içeren özel bir tür için XAML ad alanı gösterilmektedir:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Ad alanı bildirimi ayarlanırken kullanılan `EventName` aşağıdaki XAML kod örneğinde kanıtlanabilir olarak bağlanılabilir özellik:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>Gelişmiş senaryolar

Oluştururken bir [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) örnek, bir dizi Gelişmiş bağlanılabilir özellik senaryoları etkinleştirmek için ayarlayabileceğiniz isteğe bağlı parametre vardır. Bu bölümde, bu senaryolar keşfediyor.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>Özellik değişiklikleri algılama

A `static` özelliği değiştirilmiş bir geri çağırma yöntemi kaydedilebilir bağlanabilir özelliğiyle belirterek `propertyChanged` parametresi için [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi. Belirtilen geri çağırma yöntemi bağlanılabilir özellik değeri değiştiğinde çağrılır.

Aşağıdaki kod örnekte gösterildiği nasıl `EventName` bağlanılabilir özellik kayıtları `OnEventNameChanged` özelliği değiştirilmiş bir geri çağırma yöntemi olarak yöntemi:

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

Özellik değişti geri çağırma yöntemi [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) parametresi hangi sahip olan sınıfın örneğini bir değişikliği ve değerleri iki bildirdi belirtmek için kullanılır `object` parametrelerini eski ve yeni değerlerini temsil eder bağlanılabilir özellik.

<a name="validation" />

### <a name="validation-callbacks"></a>Doğrulama geri çağırmaları

A `static` doğrulama geri çağırma yöntemi kaydedilebilir bağlanabilir özelliğiyle belirterek `validateValue` parametresi için [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi. Bağlanılabilir özellik değeri ayarlandığında belirtilen geri çağırma yöntemi çağrılır.

Aşağıdaki kod örnekte gösterildiği nasıl `Angle` bağlanılabilir özellik kayıtları `IsValidValue` yöntemi bir doğrulama geri çağırma yöntemi olarak:

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

Doğrulama geri çağırmaları bir değerle sağlanır ve döndürmelidir `true` değerin Aksi takdirde özellik için geçerli olup olmadığını `false`. Doğrulama geri çağırma döndürürse, bir özel durum `false`, hangi işlenecek geliştirici tarafından. Bağlanabilir özelliği ayarlandığında tipik bir kullanımı doğrulama geri çağırma yöntemi, tamsayı veya double değerleri sınırlama. Örneğin, `IsValidValue` yöntem, özellik değeri olup olmadığını denetler bir `double` 0-360 aralığında.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>Coerce değeri geri aramaları

A `static` coerce değeri geri çağırma yöntemi kaydedilebilir bağlanabilir özelliğiyle belirterek `coerceValue` parametresi için [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) yöntemi. Belirtilen geri çağırma yöntemi bağlanılabilir özellik değeri değiştiğinde çağrılır.

Coerce değeri geri aramaları özelliğinin değeri değiştiğinde bir yeniden değerlendirme bağlanabilir bir özelliğin zorlamak için kullanılır. Örneğin, bir coerce değeri geri araması bir bağlanılabilir özellik değerini başka bir bağlanılabilir özellik değerinden büyük olmadığından emin olmak için kullanılabilir.

Aşağıdaki kod örnekte gösterildiği nasıl `Angle` bağlanılabilir özellik kayıtları `CoerceAngle` coerce değeri geri çağırma yöntemi olarak yöntemi:

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

`CoerceAngle` Yöntemi değerini denetler `MaximumAngle` özelliği ve `Angle` özellik değeri, büyük, değeri olacak şekilde zorlar `MaximumAngle` özellik değeri.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Varsayılan değer bir işlev oluşturma

A `Func` bağlanabilir bir özelliğin varsayılan değeri başlatmak için aşağıdaki kod örneğinde gösterildiği gibi kullanılabilir:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` Parametrenin ayarlanmış bir `Func` , çağıran [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) döndürülecek yöntemi bir `double` adlandırılmış kullanılan yazı tipi boyutunu temsil eden bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) yerel platformunda.

## <a name="summary"></a>Özet

Bu makalede sağlanan bağlanabilir özellikler giriş ve oluşturma ve bunları tüketme gösterilmiştir. Bağlanılabilir özellik bir özel özellik, özelliğin değerini Xamarin.Forms özellik sistemi tarafından izlenen burada türüdür.


## <a name="related-links"></a>İlgili bağlantılar

- [XAML Ad Alanları](~/xamarin-forms/xaml/namespaces.md)
- [Olayı için komut davranışı (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Doğrulama geri çağırma (örnek)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce değeri geri çağırma (örnek)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
