---
title: 'Xamarin.Essentials: sorun giderme'
description: Bu belgede Xamarin.Essentials kitaplığı ile geliştirme sırasında karşılaştığınız sorunların nasıl giderileceği açıklanmaktadır.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 8cb18ab029d2fd161c60fda7e130f319b8f0c3ab
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947380"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: sorun giderme

![NuGet yayın öncesi](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Hata: Xamarin.Android.Support.Compat için algılanan sürüm çakışması

NuGet paketlerini güncelleştirme yapılırken şu hata ortaya çıkabilir (veya yeni bir paket ekleme) Xamarin.Essentials kullanan bir Xamarin.Forms projesi ile:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

Sorun, eşleşmeyen iki Nuget'i bağımlılıkları olmasıdır. Bu bağımlılık belirli bir sürümünü el ile ekleyerek çözülebilir (Bu durumda **Xamarin.Android.Support.Compat**) hem de destekleyebilen.

Bunu yapmak için çakışmayı el ile kaynağıdır NuGet ekleme ve kullanma **sürüm** belirli bir sürümünü seçin. Şu anda 27.0.2.1 Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet sürümü bu hatayı çözülecektir.

Başvurmak [bu blog gönderisini](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) daha fazla bilgi ve sorunun nasıl çözümleneceği hakkında bir video.

Herhangi bir probleminiz ya da bir hatayı Lütfen bildirin üzerinde Bul karşılaşırsanız [Xamarin.Essentials GitHub deposu](http://github.com/xamarin/Essentials).
