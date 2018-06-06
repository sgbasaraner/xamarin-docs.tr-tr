---
title: 'Xamarin.Essentials: SMS'
description: Xamarin.Essentials Sms sınıfında bir alıcısına göndermek için belirtilen bir ileti ile varsayılan SMS uygulamayı açmak bir uygulama sağlar.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783093"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

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

- [SMS kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [SMS API belgeleri](xref:Xamarin.Essentials.Sms)
