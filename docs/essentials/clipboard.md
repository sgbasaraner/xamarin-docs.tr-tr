---
title: 'Xamarin.Essentials: Pano'
description: Bu belgede metnini sistem panoya uygulamalar arasında kopyalama ve yapıştırma imkan tanıyan Xamarin.Essentials Pano sınıfında açıklanmaktadır.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782365"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: Pano

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Pano** sınıfı, metnini sistem panoya uygulamalar arasında kopyalama ve yapıştırma olanak sağlar.

## <a name="using-clipboard"></a>Pano kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Denetlemek için **Pano** metin yapıştırılmasına şu anda hazır vardır:

```csharp
var hasText = Clipboard.HasText;
```

Metni ayarlamak için **Pano**:

```csharp
Clipboard.SetText("Hello World");
```

Dosyasından metin okunamıyor **Pano**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Pano kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Pano API belgeleri](xref:Xamarin.Essentials.Clipboard)
