---
title: Xamarin.Essentials SMS
description: Sms sınıfı bir alıcısına göndermek için belirtilen bir ileti ile varsayılan SMS uygulamayı açmak bir uygulama sağlar.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5baeee03626ba659ac7e5c06be40039476a67e08
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
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
