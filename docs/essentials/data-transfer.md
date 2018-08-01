---
title: 'Xamarin.Essentials: Veri aktarımı'
description: Xamarin.Essentials DataTransfer sınıfında gibi metin ve web bağlantıları cihazdaki diğer uygulamalara veri paylaşımı bir uygulama sağlar.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 31e27556a6681b144084d2177cf3fde8fe8e5459
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353525"
---
# <a name="xamarinessentials-data-transfer"></a>Xamarin.Essentials: Veri aktarımı

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**DataTransfer** sınıfı gibi metin ve web bağlantıları cihazdaki diğer uygulamalara veri paylaşımı bir uygulama sağlar.

## <a name="using-data-transfer"></a>Veri aktarımı kullanarak

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Veri aktarımı işlevini çağırarak çalışır `RequestAsync` yöntemi için diğer uygulamaları paylaşmak için bilgi içeren bir veri isteği yükü ile. Metin ve URI karma olabilir ve her platformu içeriğine göre filtreleme işleyecek.

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

İstek yapıldığında görüntülenen dış uygulamayı paylaşmak için kullanıcı arabirimi:

![Veri aktarımı](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Platform farklılıklarını

# <a name="androidtabandroid"></a>[Android](#tab/android)

* `Subject` özellik, istenen bir ileti konusu için kullanılır.

# <a name="iostabios"></a>[iOS](#tab/ios)

* `Subject` Kullanılmıyor.
* `Title` Kullanılmıyor.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `Title` Uygulama adı varsayılan değilse ayarlayın.
* `Subject` Kullanılmıyor.

-----

## <a name="api"></a>API

- [Veri aktarımı kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Veri aktarımı API belgeleri](xref:Xamarin.Essentials.DataTransfer)
