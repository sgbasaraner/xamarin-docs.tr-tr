---
title: 'Xamarin.Essentials: SMS'
description: Xamarin.Essentials Sms sınıfta bir alıcıya göndermek için belirtilen bir ileti ile varsayılan SMS uygulamayı açmak bir uygulama sağlar.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a93a67b83ea8f435a5e3ad5d26e1d6cbbb7092f7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815603"
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials: SMS

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Sms** sınıfı, belirtilen bir iletiyle bir alıcıya göndermek için varsayılan SMS uygulamayı açmak bir uygulama sağlar.

## <a name="using-sms"></a>SMS kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

SMS işlevselliğini çalışır çağırarak `ComposeAsync` yöntemi bir `SmsMessage` ileti alıcısı ve ikisi için de isteğe bağlı iletisinin gövdesini içerir.

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
