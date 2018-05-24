---
title: Xamarin.Essentials Pano
description: Pano sınıfı metnini sistem panoya uygulamalar arasında kopyalama ve yapıştırma olanak sağlar.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 67a0218325918b57e5ed2618b57d52d3fe3ee820
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials Pano

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
ClipBoard.SetText("Hello World");
```

Dosyasından metin okunamıyor **Pano**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Pano kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Pano API belgeleri](xref:Xamarin.Essentials.Clipboard)