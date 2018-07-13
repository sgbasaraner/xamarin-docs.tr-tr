---
title: Kullanıcı arabirimi öğelerinde erişilebilirlik değerlerini ayarlama
description: Bu makalede, böylece bir ekran okuyucu sayfada öğeleri hakkında konuşabilirsiniz AutomationProperties sınıfının nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 576fe34b0536c83825aa39bdd7111143d9474225
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998924"
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Kullanıcı arabirimi öğelerinde erişilebilirlik değerlerini ayarlama

_Xamarin.Forms yerel erişilebilirlik değerlerini ayarlama buna karşılık AutomationProperties sınıftan iliştirilmiş özellikler kullanarak kullanıcı arabirimi öğelerinde ayarlanacak erişilebilirlik değerlere izin verir. Bu makalede, böylece bir ekran okuyucu sayfada öğeleri hakkında konuşabilirsiniz AutomationProperties sınıfının nasıl kullanılacağı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms, kullanıcı arabirimi öğeleri aşağıdaki iliştirilmiş özellikler aracılığıyla ayarlanacak erişilebilirlik değerlere izin verir:

- `AutomationProperties.IsInAccessibleTree` – öğesi erişilebilir bir uygulama için kullanılabilir olup olmadığını belirtir. Daha fazla bilgi için [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` – kısa bir açıklama öğesinin öğesi için speakable bir tanımlayıcı görevi görür. Daha fazla bilgi için [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` –, araç ipucu metni öğe ile ilgili olarak düşünülebilir öğesinin uzun açıklaması. Daha fazla bilgi için [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` – başka bir öğe geçerli öğe için erişilebilirlik bilgileri tanımlamanıza olanak sağlar. Daha fazla bilgi için [AutomationProperties.LabeledBy](#labeledby).

Ekran Okuyucu öğe hakkında konuşabilirsiniz. böylece bunlar yerel erişilebilirlik değerlerini ayarlama özellikleri kullanıma açıldı. Ekli özellikler hakkında daha fazla bilgi için bkz. [iliştirilmiş özellikler](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Kullanarak `AutomationProperties` iliştirilmiş özellikler Android kullanıcı Arabirimi Test yürütme etkileyebilir. [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId), `AutomationProperties.Name` Ve `AutomationProperties.HelpText` özellikleri her ikisi de olarak ayarlanmadıysa yerel `ContentDescription` özelliği ile `AutomationProperties.Name` ve `AutomationProperties.HelpText` öncelik ayırdığınızözellikdeğerlerinin`AutomationId`değeri (her iki `AutomationProperties.Name` ve `AutomationProperties.HelpText` , değerleri birleştirilmiş ayarlanmıştır). Arama herhangi bir test buna `AutomationId` başarısız olur `AutomationProperties.Name` veya `AutomationProperties.HelpText` de öğe üzerinde ayarlanır. Bu senaryoda, kullanıcı Arabirimi testleri için değeri aranacağını değiştirilmesi gerekiyor `AutomationProperties.Name` veya `AutomationProperties.HelpText`, veya her ikisinin bir birleşimi.

Her platform erişilebilirlik değerlerini anlatıma farklı ekran okuyucu sahiptir:

- iOS VoiceOver sahiptir. Daha fazla bilgi için [Cihazınızı VoiceOver ile Test erişilebilirliği](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) developer.apple.com üzerinde.
- Android TalkBack sahiptir. Daha fazla bilgi için [test uygulamanızın erişilebilirlik](https://developer.android.com/training/accessibility/testing.html#talkback) developer.android.com üzerinde.
- Windows, ekran okuyucusu var. Daha fazla bilgi için [ekran okuyucusu ana uygulama senaryolarını doğrulamak](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Ancak, yazılım ve kullanıcının yapılandırılmasını tam ekran okuyucu davranışını bağlıdır. Örneğin, çoğu ekran okuyucular, sayfadaki denetimleri arasında taşırken kendilerini yönlendirmek kullanıcıların odağı aldığında bir denetimle ilişkilendirilen metni okuyun. Bazı ekran okuyucular de okuma tüm uygulama kullanıcı arabirimi bir sayfası görüntülendiğinde, kullanıcının tüm sayfa kullanılabilir bilgi içeriği gezinebilirsiniz denemeden önce almasını sağlar.

Ekran okuyucuları, ayrıca farklı erişilebilirlik değerlerini okuyun. Örnek uygulama:

- VoiceOver okuyup `Placeholder` değerini `Entry`denetimin kullanımıyla ilgili yönergeleri çizgidir.
- TalkBack okuyup `Placeholder` değerini `Entry`çizgidir `AutomationProperties.HelpText` denetimin kullanımıyla ilgili yönergeleri ardından değeri.
- Ekran Okuyucusu okuyup `AutomationProperties.LabeledBy` değerini `Entry`çizgidir denetimi kullanma yönergeleri.

Ayrıca, ekran okuyucusu sıralayacağını `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, ardından `AutomationProperties.HelpText`. Android'de TalkBack birleştirmek `AutomationProperties.Name` ve `AutomationProperties.HelpText` değerleri. Bu nedenle, kapsamlı erişilebilirliğini test bir en iyi deneyimi sağlamak için her bir platform üzerinde gerçekleştirilir, önerilir.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` Ekli özelliği bir `boolean` erişilebilir ve bu nedenle görünür, ekran okuyucular için öğe olup olmadığını belirler. Bu ayarlanmalıdır `true` iliştirilmiş özellikler diğer erişilebilirlik kullanın. Bu gibi XAML içinde gerçekleştirilebilir:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Alternatif olarak, C# ' de aşağıdaki gibi ayarlanabilir:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Unutmayın [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) yöntemi de kullanılabilir ayarlanacak `AutomationProperties.IsInAccessibleTree` ekli özellik – `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` Ekli özellik değeri, bir öğe duyurmaktan ekran okuyucu kullanan bir kısa ve açıklayıcı metin dizesi olmalıdır. Bu özellik, içeriği anlamak veya kullanıcı arabirimi ile etkileşim kurmak için önemli olan bir anlamı olan öğeler için ayarlanması gerekir. Bu gibi XAML içinde gerçekleştirilebilir:

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Alternatif olarak, C# ' de aşağıdaki gibi ayarlanabilir:

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Unutmayın [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) yöntemi de kullanılabilir ayarlanacak `AutomationProperties.Name` ekli özellik – `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` Ekli özelliği kullanıcı arabirimi öğesi açıklayan metin ayarlanmalıdır ve araç ipucu metni olarak döndürülebileceği bakımından öğe ile ilişkilendirilebilir. Bu gibi XAML içinde gerçekleştirilebilir:

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Alternatif olarak, C# ' de aşağıdaki gibi ayarlanabilir:

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Unutmayın [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) yöntemi de kullanılabilir ayarlanacak `AutomationProperties.HelpText` ekli özellik – `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Bazı platformlarda, düzenleme için denetimleri gibi bir [ `Entry` ](xref:Xamarin.Forms.Entry), `HelpText` özellik bazen atlanmış kullanılabilir ve yer tutucu metinle değiştirildi. Örneğin, "kendi adınızı girin burada" iyi aday olduğunu [ `Entry.Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder) önce kullanıcının gerçek girişi denetiminde metin yerleştirir özelliği.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` Ekli özellik geçerli öğe için erişilebilirlik bilgileri tanımlamak başka bir öğe sağlar. Örneğin, bir [ `Label` ](xref:Xamarin.Forms.Label) yanında bir [ `Entry` ](xref:Xamarin.Forms.Entry) ne açıklamak için kullanılan `Entry` temsil eder. Bu gibi XAML içinde gerçekleştirilebilir:

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Alternatif olarak, C# ' de aşağıdaki gibi ayarlanabilir:

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Unutmayın [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) yöntemi de kullanılabilir ayarlanacak `AutomationProperties.IsInAccessibleTree` ekli özellik – `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Özet

Bu makalede açıklanan erişilebilirlik değerlerini kullanıcı arabirimi öğeleri içinde bir Xamarin.Forms uygulaması ekli özellikleri kullanarak nasıl ayarlanacağını `AutomationProperties` sınıfı. Ekran Okuyucu sayfada öğeleri hakkında konuşabilirsiniz böylece bunlar yerel erişilebilirlik değerlerini ayarlama özellikleri eklenmiş.


## <a name="related-links"></a>İlgili bağlantılar

- [Ekli Özellikler](~/xamarin-forms/xaml/attached-properties.md)
- [Erişilebilirlik (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
