---
title: 'Xamarin.Essentials: Veri aktarımı'
description: Xamarin.Essentials DataTransfer sınıfında gibi metin ve web bağlantıları cihazdaki diğer uygulamalara veri paylaşımı bir uygulama sağlar.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c1ed298e1317d0a3f78f4dbd9fc89a2b01c6958c
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855116"
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

* `Subject` kullanılmıyor.
* `Title` kullanılmıyor. 

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

* `Title` Uygulama adı varsayılan değilse ayarlayın.
* `Subject` kullanılmıyor.

-----

## <a name="api"></a>API

- [Veri aktarımı kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Veri aktarımı API belgeleri](xref:Xamarin.Essentials.DataTransfer)
