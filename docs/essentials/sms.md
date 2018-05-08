---
title: Xamarin.Essentials SMS
description: Sms sınıfı bir alıcısına göndermek için belirtilen bir ileti ile varsayılan SMS uygulamayı açmak bir uygulama sağlar.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 460aea01381934f1862946c6e17e314560e889f2
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials SMS

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Sms** sınıfı bir alıcısına göndermek için belirtilen bir ileti ile varsayılan SMS uygulamayı açmak bir uygulama sağlar.

## <a name="using-sms"></a>SMS kullanarak

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

SMS işlevselliği çalışır çağırarak `ComposeAsync` yöntemi bir `SmsMessage` ileti alıcısı ve her ikisi de isteğe bağlı ileti gövdesi içerir.

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, recipient);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [SMS kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Essentials/Sms)
- [SMS API belgeleri](xref:Xamarin.Essentials.Sms)
