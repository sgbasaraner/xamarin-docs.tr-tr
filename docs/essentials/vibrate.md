---
title: 'Xamarin.Essentials: Titreşim'
description: Bu belgede vibrate işlevselliğini istenen bir zaman miktarı için başlatıp olanak sağlayan Xamarin.Essentials Titreşim sınıfında açıklanmaktadır.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1de464d289bc684015e5fb8489683e3134535b70
ms.sourcegitcommit: cb69bdb469db0b3118e365d71114091c6febb027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37406777"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: Titreşim

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Titreşim** sınıfı, vibrate işlevselliğini istenen bir zaman miktarı için başlatıp olanak tanır.

## <a name="getting-started"></a>Başlarken

Erişim için **Titreşim** işlevselliğini aşağıdaki platforma özgü Kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Vibrate izni gereklidir ve Android projede yapılandırılması gerekir. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

YA da Android bildirimini güncelleştir:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve içine aşağıdaki **bildirim** düğümü.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Anroid projeye sağ tıklayın ve proje özelliklerini açın. Altında **Android bildirim** Bul **gerekli izinler:** alan ve onay **VIBRATE** izni. Bu otomatik olarak güncelleştirecektir **AndroidManifest.xml** dosya.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulum gerekli.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulum gerekli.

-----

## <a name="using-vibration"></a>Titreşim kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Titreşim işlevselliği, belirli bir zaman veya aralığını 500 milisaniyenin varsayılan istenebilir.

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

Cihaz Titreşim iptali ile istenebilir `Cancel` yöntemi:

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="platform-differences"></a>Platform farklılıklarını

| Platform | Fark |
| --- | --- |
| iOS | Cihaz "Ring üzerinde Vibrate" olarak ayarlandığında yalnızca Titreşim çıkardığı. |
| iOS | Her zaman aralığını 500 milisaniyenin için Titreşim çıkardığı. |
| iOS | Titreşim iptal etmek mümkün değildir. |

## <a name="api"></a>API

- [Titreşim kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Titreşim API belgeleri](xref:Xamarin.Essentials.Vibration)
