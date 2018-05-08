---
title: Xamarin.Essentials Pano
description: Pano sınıfı metnini sistem panoya uygulamalar arasında kopyalama ve yapıştırma olanak sağlar.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 218f90746f130f02c47040a9191b1001fa80c203
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
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

- [Pano kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Essentials/Clipboard)
- [Pano API belgeleri](xref:Xamarin.Essentials.Clipboard)
