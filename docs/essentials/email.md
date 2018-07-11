---
title: 'Xamarin.Essentials: e-posta'
description: E-posta sınıfı Xamarin.Essentials, konu ve gövde alıcılar (Kime, bilgi, gizli) dahil olmak üzere belirtilen bilgilerle varsayılan e-posta uygulamasını açmak için bir uygulama sağlar.
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: aea2f429126180ae3d98bc665bed5574f416ea53
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38848552"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: e-posta

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**E-posta** sınıfı konu ve gövde alıcı (Kime, bilgi, gizli) dahil olmak üzere belirtilen bilgilerle varsayılan e-posta uygulamasını açmak için bir uygulama sağlar.

## <a name="using-email"></a>E-posta kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

E-posta işlevselliği çalışır çağırarak `ComposeAsync` yöntemi bir `EmailMessage` , e-posta hakkında bilgiler içerir:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
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
            };
            
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
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
