---
title: 'Xamarin.Essentials: Pano'
description: Bu belgede uygulamalar arasında sistem panoya metin kopyalayıp olanak sağlayan Xamarin.Essentials Pano sınıfında açıklanmaktadır.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842621"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: Pano

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Pano** sınıfı, uygulamalar arasında sistem panoya metin kopyalayıp olanak tanır.

## <a name="using-clipboard"></a>Pano kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Olmadığını denetlemek için **Pano** metnin yapıştırılmasına şu anda hazır vardır:

```csharp
var hasText = Clipboard.HasText;
```

Metin ayarlanacak **Pano**:

```csharp
Clipboard.SetText("Hello World");
```

Metinden okunacak **Pano**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Pano kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Panoya API'si belgeleri](xref:Xamarin.Essentials.Clipboard)
