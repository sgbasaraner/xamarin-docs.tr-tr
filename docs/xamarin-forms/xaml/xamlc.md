---
title: Xamarin.Forms XAML derleme
description: Bu makalede nasıl XAML isteğe bağlı olarak ara dil (IL) Xamarin.Forms XAML derleyicisi (XAMLC) ile doğrudan derlenebilir açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/02/2018
ms.openlocfilehash: b828e62ef1037bf47a2ae5fb303fbf8fcace6549
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997139"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Xamarin.Forms XAML derleme

_XAML, Ara dil (IL) XAML derleyicisi (XAMLC) ile doğrudan isteğe bağlı olarak derlenebilir._

XAMLC bir avantajları sunar:

- Bu hataların kullanıcıya bildirimde, XAML derleme zamanı denetimi gerçekleştirir.
- Bazı yük ve örnek oluşturma saati XAML öğeleri kaldırır.
- Artık .xaml dosyalarını dahil ederek son derlemeyi dosya boyutunu küçültmek için yardımcı olur.

XAMLC geriye dönük uyumluluk sağlamak için varsayılan olarak devre dışıdır. Hem derleme hem de sınıf düzeyinde zaman etkinleştirilebilir ekleyerek `XamlCompilation` özniteliği.

Aşağıdaki kod örneği, derleme düzeyinde XAMLC etkinleştirme gösterir:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

Bu örnekte, derleme zamanı derleme içinde bulunan tüm XAML denetimi, çalışma zamanı yerine derleme zamanı bildirilen XAML hatalarla gerçekleştirilir. Bu nedenle, `assembly` için önek `XamlCompilation` özniteliği, öznitelik tüm derleme için geçerli olduğunu belirtir.

Aşağıdaki kod örneği sınıf düzeyinde XAMLC etkinleştirme gösterir:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

Bu örnekte, derleme zamanı için XAML denetimi `HomePage` sınıfı gerçekleştirilmesi ve hatalarını bildirdi derleme işleminin bir parçası.

> [!NOTE]
> `XamlCompilation` Özniteliği ve `XamlCompilationOptions` numaralandırma bulunan `Xamarin.Forms.Xaml` ad alanı bunları kullanmak için içeri aktarılmalıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [XamlCompilation](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [XamlCompilationOptions](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
