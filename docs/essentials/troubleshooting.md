---
title: 'Xamarin.Essentials: sorun giderme'
description: Bu belge, Xamarin.Essentials kitaplıkla geliştirirken karşılaşılan sorunları giderme açıklar.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 3dba315aec2475cb334110ba7555f773f4165aa1
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066740"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: sorun giderme

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Hata: Sürüm çakışması için Xamarin.Android.Support.Compat algılandı

NuGet paketlerini güncelleştirirken aşağıdaki hata oluşabilir (veya yeni bir paket ekleme), Xamarin.Essentials kullanan bir Xamarin.Forms proje ile:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.7.0.17-preview -> Xamarin.Android.Support.CustomTabs 27.0.2 -> Xamarin.Android.Support.Compat (= 27.0.2) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

İki NuGets eşleşmeyen bağımlılıkları sorunudur. Bu bağımlılık belirli bir sürümünü el ile ekleyerek çözülebilir (Bu durumda **Xamarin.Android.Support.Compat**) hem de destekleyebilen.

Bunu yapmak için çakışmayı el ile kaynağıdır NuGet ekleyip kullanabilir **sürüm** listesi belirli bir sürüm. Şu anda Xamarin.Android.Support.Compat NuGet 27.0.2 sürümü bu hatayı gidermek.

Başvurmak [bu blog gönderisine](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) daha fazla bilgi ve sorunun nasıl çözümleneceği hakkında bir video.

Herhangi bir probleminiz ya da bir hata Lütfen bildirin üzerinde Bul alıyorsanız [Xamarin.Essentials GitHub deposunu](http://github.com/xamarin/Essentials).
