---
title: 'Xamarin.Essentials: Veri aktarımı'
description: Xamarin.Essentials DataTransfer sınıfında cihazda diğer uygulamalar için metin ve web bağlantıları gibi verileri paylaşmak bir uygulama sağlar.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 69d429b1cdbbbd6dbb53e3cefa89695666494ba7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782391"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials: Veri aktarımı

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

- [Veri aktarımı kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Veri aktarma API belgeleri](xref:Xamarin.Essentials.DataTransfer)
