---
title: 'Xamarin.Essentials: Titreşim'
description: Bu belgede vibrate işlevselliğini istenen bir zaman miktarı için başlatıp olanak sağlayan Xamarin.Essentials Titreşim sınıfında açıklanmaktadır.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 622689342dd961a63318a88f098dea4d1a60e277
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353873"
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

Android projeye sağ tıklayın ve proje özelliklerini açın. Altında **Android bildirim** Bul **gerekli izinler:** alan ve onay **VIBRATE** izni. Bu otomatik olarak güncelleştirecektir **AndroidManifest.xml** dosya.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulum gerekli.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Hiçbir platform farklılıklarını.

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

# <a name="androidtabandroid"></a>[Android](#tab/android)

Hiçbir platform farklılıklarını.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Cihaz "Ring üzerinde Vibrate" olarak ayarlandığında yalnızca Titreşim çıkardığı.
* Her zaman aralığını 500 milisaniyenin için Titreşim çıkardığı.
* Titreşim iptal etmek mümkün değildir.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Hiçbir platform farklılıklarını.

-----

## <a name="api"></a>API

- [Titreşim kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Titreşim API belgeleri](xref:Xamarin.Essentials.Vibration)
