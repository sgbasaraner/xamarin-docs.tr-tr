---
title: Xamarin.Essentials açık tarayıcı
description: Tarayıcı sınıfı en iyi duruma getirilmiş sistem tercih edilen tarayıcıda veya dış tarayıcı web bağlantıyı açmak bir uygulama sağlar.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2a95d0b89b78ce8b0ddfdc94d86c284a6f8b2bbe
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2018
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials tarayıcı

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Tarayıcı** sınıfı en iyi duruma getirilmiş sistem tercih edilen tarayıcıda veya dış tarayıcı web bağlantıyı açmak bir uygulama sağlar.

## <a name="using-browser"></a>Tarayıcı kullanarak

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Tarayıcı işlevselliği çalışır çağırarak `OpenAsync` yöntemiyle `Uri` ve `BrowserLaunchType`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.OpenAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Platform uygulama özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

Başlatma türü, tarayıcı nasıl başlatılır belirler:

## <a name="system-preferred"></a>Tercih edilen sistem

[Chrome özel sekmeler](https://developer.chrome.com/multidevice/android/customtabs) kullanılacak çalıştı URI yük ve gezinti tanıma tutun.

## <a name="external"></a>Harici

Bir `Intent` URI sistemleri normal tarayıcı açılabilir istemek için kullanılır.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Tercih edilen sistem

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) URI yüklenemedi ve gezinti tanıma tutmak için kullanılır.

## <a name="external"></a>Harici

Standart `OpenUrl` ana uygulamanın uygulama dışında varsayılan tarayıcı başlatmak için kullanılır.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Kullanıcının varsayılan tarayıcı bakılmaksızın her zaman başlatılacağı `BrowserLaunchType`.

--------------

## <a name="api"></a>API

- [Tarayıcı kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Tarayıcı API belgeleri](xref:Xamarin.Essentials.Browser)