---
title: Haptic geribildirim sağlama
description: Bu makalede haptic geri bildirim iOS 10 ve Xamarin.iOS içinde uygulama bulunan yeni türleri yer almaktadır.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f2d1bd73ea764cd5bf56775abd7c7357b039bc79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="providing-haptic-feedback"></a>Haptic geribildirim sağlama

_Bu makalede haptic geri bildirim iOS 10 ve Xamarin.iOS içinde uygulama bulunan yeni türleri yer almaktadır._

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

İPhone ve iPhone 7 üzerinde fiziksel olarak kullanıcı bulunmaya ek yöntemler sağlayan yeni haptic yanıtlar 7 artı ve Apple eklendi. (Genellikle yalnızca Haptics adlandırılır) haptic geribildirim (üzerinden zorla, vibrations veya hareket) dokunmatik duygusu kullanıcı arabirimi tasarımında kullanır. Kullanıcının dikkatini ve eylemlerini pekiştirmek için bu yeni tactile geri bildirim seçeneklerini kullanın.

Aşağıdaki konularda ayrıntılı olarak ele alınacaktır:

- [Hakkında Haptic geri bildirim](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>Hakkında Haptic geri bildirim

Birkaç yerleşik kullanıcı Arabirimi öğeleri zaten seçiciler, anahtarlar ve kaydırıcılar gibi haptic geri bildirim sağlayın. iOS 10 şimdi haptics somut alt sınıfı kullanılarak programlı olarak tetiklemek yeteneği ekler `UIFeedbackGenerator` sınıfı.

Geliştirici aşağıdakilerden birini kullanabilir `UIFeedbackGenerator` alt sınıflar program aracılığıyla haptic geri bildirim tetikleyici için:

- `UIImpactFeedbackGenerator` -Bir eylem veya "thud" yerine bir görünüm slayt veya iki ekranda nesneleri çakışıyorsa sunan gibi görevi tamamlamak için bu geri bildirim oluşturucuyu kullanın.
- `UINotificationFeedbackGenerator` -Bu geri bildirim oluşturucunun bir eylem Tamamlanıyor, başarısız olan veya diğer tür uyarı gibi bildirimler için kullanın.
- `UISelectionFeedbackGenerator` -Bu geri bildirim oluşturucunun bir listeden bir öğe alma gibi etkin olarak değiştirme seçimi kullanın.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

Bir eylem veya "thud" yerine bir görünüm slayt veya iki ekranda nesneleri çakışıyorsa sunan gibi görevi tamamlamak için bu geri bildirim oluşturucuyu kullanın.

Tetikleyici etkisi geribildirim aşağıdaki kodu kullanın:

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Geliştirici, yeni bir örneğini oluşturduğunda `UIImpactFeedbackGenerator` sağladıkları sınıfı bir `UIImpactFeedbackStyle` geri bildirim gücünü belirtme:

- `Heavy`
- `Medium`
- `Light`

`Prepare` Yöntemi `UIImpactFeedbackGenerator` sistem haptic geri bildirim gecikme süresi en aza indirebilirsiniz böylece gerçekleşmek üzere olduğunu bildirmek için çağrılır.

`ImpactOccurred` Yöntemi sonra haptic geri bildirim tetikler.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

Bu geri bildirim oluşturucunun bir eylem Tamamlanıyor, başarısız olan veya diğer tür uyarı gibi bildirimler için kullanın.

Tetikleyici bildirim geribildirim aşağıdaki kodu kullanın:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Yeni bir örneğini `UINotificationFeedbackGenerator` sınıfı oluşturulur ve kendi `Prepare` yöntemi sistem haptic geri bildirim gecikme süresi en aza indirebilirsiniz böylece gerçekleşmek üzere olduğunu bildirmek üzere çağrılır.

`NotificationOccurred` Haptic geri bildirim tetiklemek için çağrılan bir verilen `UINotificationFeedbackType` biri:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

Listeden bir öğe alma gibi etkin olarak değiştirme seçimi için bu geri bildirim oluşturucuyu kullanın.

Tetikleyici seçimini geri bildirim aşağıdaki kodu kullanın:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Yeni bir örneğini `UISelectionFeedbackGenerator` sınıfı oluşturulur ve kendi `Prepare` yöntemi sistem haptic geri bildirim gecikme süresi en aza indirebilirsiniz böylece gerçekleşmek üzere olduğunu bildirmek üzere çağrılır.

`SelectionChanged` Yöntemi sonra haptic geri bildirim tetikler.

## <a name="summary"></a>Özet

Bu makalede ele haptic geri iOS 10 ve Xamarin.iOS içinde uygulama bulunan yeni türleri.

## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
