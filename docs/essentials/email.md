---
title: Xamarin.Essentials e-posta
description: E-posta sınıfı varsayılan e-posta uygulamasının konu ve gövde alıcı (Kime, bilgi, gizli) dahil olmak üzere belirtilen bilgilerle açmak bir uygulama sağlar.
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: dde3f7f160d796afe3184ef1d61d8b437ec09ea8
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials e-posta

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**E-posta** sınıfı varsayılan e-posta uygulamasının konu ve gövde alıcı (Kime, bilgi, gizli) dahil olmak üzere belirtilen bilgilerle açmak bir uygulama sağlar.

## <a name="using-email"></a>E-posta kullanarak

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

E-posta işlevselliği çalışır çağırarak `ComposeAsync` yöntemi bir `EmailMessage` e-posta hakkında bilgi içerir:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string, body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            }
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Sms is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>API

- [E-posta kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [E-posta API belgeleri](xref:Xamarin.Essentials.Email)
