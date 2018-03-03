---
title: "Kullanıcı arabirimi öğeleri erişilebilirlik değerleri ayarlama"
description: "Xamarin.Forms içinde kümesi yerel erişilebilirlik değerleri kapatma ekli özellikler AutomationProperties sınıfından kullanarak kullanıcı arabirimi öğeleri ayarlanacak erişilebilirlik değerlere izin verir. Bu makalede, böylece bir ekran okuyucusu sayfada öğeleri hakkında konuşurken AutomationProperties sınıfının nasıl kullanılacağı açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 4aeeea7f946a121b12741d2da217daf531935849
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Kullanıcı arabirimi öğeleri erişilebilirlik değerleri ayarlama

_Xamarin.Forms içinde kümesi yerel erişilebilirlik değerleri kapatma ekli özellikler AutomationProperties sınıfından kullanarak kullanıcı arabirimi öğeleri ayarlanacak erişilebilirlik değerlere izin verir. Bu makalede, böylece bir ekran okuyucusu sayfada öğeleri hakkında konuşurken AutomationProperties sınıfının nasıl kullanılacağı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms kullanıcı arabirimi öğeleri aşağıdaki ekli özellikler aracılığıyla ayarlanacak erişilebilirlik değerlere izin verir:

- `AutomationProperties.IsInAccessibleTree` – öğesi erişilebilir bir uygulama için kullanılabilir olup olmadığını gösterir. Daha fazla bilgi için bkz: [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` – öğesi için speakable bir tanımlayıcı olarak hizmet veren öğesinin kısa bir açıklaması. Daha fazla bilgi için bkz: [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` – daha uzun bir açıklama öğesinin, araç ipucu metni öğeyle ilişkili olarak düşünülebilir. Daha fazla bilgi için bkz: [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` – başka bir öğenin geçerli öğe için erişilebilirlik bilgileri tanımlamanıza izin verir. Daha fazla bilgi için bkz: [AutomationProperties.LabeledBy](#labeledby).

Ekran Okuyucusu öğesi hakkında konuşurken, bu özellikler kümesi yerel erişilebilirlik değerleri bağlı. Ekli özellikler hakkında daha fazla bilgi için bkz: [ekli özellikler](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Kullanarak `AutomationProperties` ekli özellikler UI Test yürütmesi Android üzerinde etkisi. [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/), `AutomationProperties.Name` Ve `AutomationProperties.HelpText` özellikleri her ikisi de ayarlayacak yerel `ContentDescription` özelliği ile `AutomationProperties.Name` ve `AutomationProperties.HelpText` ÖncelikAlmaözellikdeğerlerini`AutomationId`değeri (her iki `AutomationProperties.Name` ve `AutomationProperties.HelpText` , değerleri birleştirilmiş ayarlanmıştır). Görmek herhangi bir test buna `AutomationId` başarısız olur `AutomationProperties.Name` veya `AutomationProperties.HelpText` öğede de ayarlayın. Bu senaryoda, UI testleri için değerini aramak için değiştirilmesi `AutomationProperties.Name` veya `AutomationProperties.HelpText`, veya her ikisinin bir birleşimi.

Her platformun erişilebilirlik değerleri ile Anlat için farklı ekran okuyucusu vardır:

- iOS VoiceOver sahiptir. Daha fazla bilgi için bkz: [Test erişilebilirlik VoiceOver bilgisayarınızı cihazına](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) developer.apple.com üzerinde.
- Android TalkBack sahiptir. Daha fazla bilgi için bkz: [sınama bilgisayarınızı uygulamanın erişilebilirlik](https://developer.android.com/training/accessibility/testing.html#talkback) developer.android.com üzerinde.
- Windows ekran okuyucusu var. Daha fazla bilgi için bkz: [ekran okuyucusu kullanarak ana uygulama senaryoları doğrulamak](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Ancak, yazılım ve kullanıcının yapılandırılmasını tam ekran okuyucusu davranışını bağlıdır. Örneğin, çoğu ekran okuyucular, sayfadaki denetimleri arasında taşıdığınızda kendilerini yönlendirmek kullanıcıları etkinleştirme odağı aldığında bir denetimle ilişkili metni okuyun. Bazı ekran okuyucuların de okuma tüm uygulama kullanıcı arabirimi bir sayfası göründüğünde, kullanıcının tüm sayfanın kullanılabilir bilgi içeriğinin onu gitmek denemeden önce almasını sağlar.

Ekran okuyucular ayrıca farklı erişilebilirlik değerleri okuyun. Örnek uygulama:

- VoiceOver okuyup okumayacağını `Placeholder` değerini `Entry`, denetimi kullanma yönergeleri izleyen.
- TalkBack okuyup okumayacağını `Placeholder` değerini `Entry`, ardından `AutomationProperties.HelpText` denetimi kullanma yönergeleri ve ardından değeri.
- Ekran Okuyucusu okuyup okumayacağını `AutomationProperties.LabeledBy` değerini `Entry`, denetimi kullanma yönergeleri izleyen.

Ayrıca, ekran okuyucusu sıralayacağını `AutomationProperties.Name`, `AutomationProperties.LabeledBy`ve ardından `AutomationProperties.HelpText`. Android, TalkBack birleştirmek `AutomationProperties.Name` ve `AutomationProperties.HelpText` değerleri. Bu nedenle, kapsamlı erişilebilirlik test bir en iyi deneyimi sağlamak için her platformda gerçekleştirilir, önerilir.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` İliştirilmiş özelliği bir `boolean` öğe erişilebilir durumda ve bu nedenle görünür, ekran okuyucuların ise belirler. Kümesine gerekir `true` özellikleri bağlı diğer erişilebilirlik kullanın. Bu gibi XAML'de gerçekleştirilebilir:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Alternatif olarak, C# ' ta aşağıdaki gibi ayarlanabilir:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Unutmayın [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) yöntemi de kullanılabilir ayarlamak için `AutomationProperties.IsInAccessibleTree` özelliği – eklenmiş `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` Ekli özellik değeri, öğenin duyurmaktan ekran okuyucusu kullanan bir kısa ve açıklayıcı metin dizesi olmalıdır. Bu özellik, içeriği anlamak veya kullanıcı arabirimiyle etkileşim için önemli bir anlama sahip öğeler için ayarlanmalıdır. Bu gibi XAML'de gerçekleştirilebilir:

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Alternatif olarak, C# ' ta aşağıdaki gibi ayarlanabilir:

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Unutmayın [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) yöntemi de kullanılabilir ayarlamak için `AutomationProperties.Name` özelliği – eklenmiş `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` Ekli özellik kullanıcı arabirimi öğesi açıklayan metin için ayarlanmış olmalıdır ve, bir düşünce araç ipucu metni olarak öğeyle ilişkili. Bu gibi XAML'de gerçekleştirilebilir:

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Alternatif olarak, C# ' ta aşağıdaki gibi ayarlanabilir:

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Unutmayın [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) yöntemi de kullanılabilir ayarlamak için `AutomationProperties.HelpText` özelliği – eklenmiş `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

Bazı platformlarda düzenleme için denetimleri gibi bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), `HelpText` özellik bazen atlanmış kullanılabilir ve yer tutucu metnini değiştirildi. Örneğin, "kendi adınızı girin burada" için iyi bir aday olduğunu [ `Entry.Placeholder` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) kullanıcının gerçek giriş önce denetiminde metni yerleştirir özelliği.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` Ekli özellik geçerli öğe için erişilebilirlik bilgileri tanımlamak başka bir öğe sağlar. Örneğin, bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) yanına bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ne tanımlamak için kullanılan `Entry` temsil eder. Bu gibi XAML'de gerçekleştirilebilir:

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Alternatif olarak, C# ' ta aşağıdaki gibi ayarlanabilir:

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Unutmayın [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) yöntemi de kullanılabilir ayarlamak için `AutomationProperties.IsInAccessibleTree` özelliği – eklenmiş `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Özet

Bu makalede erişilebilirlik değerleri kullanıcı arabirimi öğeleri bir Xamarin.Forms uygulaması ekli özelliklerinden kullanarak nasıl ayarlanacağı açıklanmıştır `AutomationProperties` sınıfı. Ekran Okuyucusu sayfada öğeleri hakkında konuşurken böylece bu özellikler kümesi yerel erişilebilirlik değerleri bağlı.


## <a name="related-links"></a>İlgili bağlantılar

- [Ekli özellikler](~/xamarin-forms/xaml/attached-properties.md)
- [Erişilebilirlik (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
