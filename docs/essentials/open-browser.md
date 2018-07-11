---
title: Xamarin.Essentials açık tarayıcı
description: Xamarin.Essentials tarayıcı sınıfında en iyi duruma getirilmiş sistem tercih edilen tarayıcıda veya dış tarayıcı web bağlantısı açmak için bir uygulama sağlar.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 563d3899cffb80c0215d90e8e4392046c4635256
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815712"
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials: tarayıcı

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Tarayıcı** sınıfı bir web bağlantısına en iyi duruma getirilmiş sistem tercih edilen tarayıcıda veya dış tarayıcı açmak için bir uygulama sağlar.

## <a name="using-browser"></a>Tarayıcıyı kullanarak

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

## <a name="platform-implementation-specifics"></a>Platform uygulaması özellikleri

# <a name="androidtabandroid"></a>[Android](#tab/android)

Başlatma türü, tarayıcı nasıl başlatılır belirler:

## <a name="system-preferred"></a>Tercih edilen sistem

[Özel sekmeler Chrome](https://developer.chrome.com/multidevice/android/customtabs) olacak kullanılacak çalıştı URI yüklemek ve gezinti tanıma tutun.

## <a name="external"></a>Harici

Bir `Intent` URI sistemleri normal tarayıcıdan açılabilir istemek için kullanılır.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Tercih edilen sistem

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) URI yüklenip Gezinti tanıma tutmak için kullanılır.

## <a name="external"></a>Harici

Standart `OpenUrl` ana uygulama üzerinde uygulama dışında varsayılan tarayıcıyı başlatmak için kullanılır.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Kullanıcının varsayılan tarayıcı bakılmaksızın her zaman başlatılacak `BrowserLaunchType`.

--------------

## <a name="api"></a>API

- [Tarayıcı kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Browser)
- [Tarayıcı API belgeleri](xref:Xamarin.Essentials.Browser)
