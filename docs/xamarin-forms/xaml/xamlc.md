---
title: XAML derleme
description: "XAML isteğe bağlı olarak ara dile (IL) XAML derleyici (XAMLC) ile doğrudan derlenebilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: 3ce0fd2b83599fca7f38c161bbd2137254b75bd5
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2018
---
# <a name="xaml-compilation"></a>XAML derleme

_XAML isteğe bağlı olarak ara dile (IL) XAML derleyici (XAMLC) ile doğrudan derlenebilir._

XAMLC bir avantajları sunar:

- Derleme zamanı hatalarını kullanıcı bildiren XAML denetimi gerçekleştirir.
- XAML öğeleri için yük ve örnek oluşturma saat bazıları kaldırır.
- Artık .xaml dosyaları ekleyerek son derlemeyi dosya boyutunu azaltmak için yardımcı olur.

XAMLC geriye dönük uyumluluğu sağlamak için varsayılan olarak devre dışıdır. Derleme ve sınıf düzeyinde zaman etkinleştirilebilir ekleyerek `XamlCompilation` özniteliği.

Aşağıdaki kod örneğinde, derleme düzeyinde XAMLC etkinleştirme gösterilmektedir:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

Bu örnekte, derleme zamanı tüm XAML denetimi içindeki `PhotoApp` ad alanı gerçekleştirilecek, çalışma zamanı yerine derleme zamanı raporlandığını XAML hatalı.
`assembly` İçin önek `XamlCompilation` özniteliği, öznitelik tüm derlemesi için geçerli olduğunu belirtir.

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

Bu örnekte, derleme zamanı için XAML denetimi `HomePage` sınıfı gerçekleştirilecek ve hataları derleme işleminin bir parçası raporlanır.

> [!NOTE]
> `XamlCompilation` Özniteliği ve `XamlCompilationOptions` numaralandırma bulunan `Xamarin.Forms.Xaml` bunları kullanmaya aktarılmalıdır ad alanı.


## <a name="related-links"></a>İlgili bağlantılar

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)