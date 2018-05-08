---
title: Xamarin.Essentials veri aktarımı
description: DataTransfer sınıfı cihazda diğer uygulamalar için metin ve web bağlantıları gibi verileri paylaşmak bir uygulama sağlar.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6c6521c9bacd7b3e9da67165d2d271f5a40e4d7a
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials veri aktarımı

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**DataTransfer** sınıfı cihazda diğer uygulamalar için metin ve web bağlantıları gibi verileri paylaşmak bir uygulama sağlar.

## <a name="using-data-transfer"></a>Veri aktarımı kullanarak

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Veri aktarımı işlevselliği çalışır çağırarak `RequestAsync` diğer uygulamaları paylaşmak için bilgileri içeren veri isteği yükü yöntemiyle. Metin ve URI karışık ve her platformu içeriğine göre filtreleme işleyecek.

```csharp

public class DataTransferTest
{
    public async Task ShareText(string text)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

İstek yapıldığında görüntülenen dış uygulamaya paylaşmak için kullanıcı arabirimi:

![Veri aktarımı](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Platform farklar

| Platform | Fark |
| --- | --- |
| Android | Konu özelliği bir ileti istenen konu için kullanılır. |
| iOS | Konu kullanılmaz. |
| iOS | Başlık kullanılmaz. |
| UWP | Başlık uygulama adının varsayılan değilse ayarlayın. |
| UWP | Konu kullanılmaz. |

## <a name="api"></a>API

- [Veri aktarımı kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Essentials/DataTransfer)
- [Veri aktarma API belgeleri](xref:Xamarin.Essentials.DataTransfer)
