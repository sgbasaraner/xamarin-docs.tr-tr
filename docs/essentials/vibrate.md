---
title: 'Xamarin.Essentials: Titreşimi'
description: Bu belgede, istenen bir süre için vibrate işlevselliği durdurup başlatın sağlar Xamarin.Essentials titreşimi sınıfında açıklanmaktadır.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2a2902ca4eac8b889f6875580d7cb4ea352803a8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782927"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: Titreşimi

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Titreşimi** sınıfı, istenen bir süre için vibrate işlevselliği durdurup başlatın olanak sağlar.

## <a name="getting-started"></a>Başlarken

Erişim için **titreşimi** işlevselliği aşağıdaki platform özel kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Vibrate izni gereklidir ve Android projesinde yapılandırılması gerekir. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

YA da güncelleştirme Android bildirim:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve aşağıdaki içine ekleyin **bildirim** düğümü.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Veya Anroid projeye sağ tıklayın ve projenin özelliklerini açın. Altında **Android derleme bildirimi** Bul **gerekli izinler:** alan ve onay **VIBRATE** izni. Bu otomatik olarak güncelleştirilecek **AndroidManifest.xml** dosya.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulumu gerekmez.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulumu gerekmez.

-----

## <a name="using-vibration"></a>Titreşim kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Titreşim işlevselliği süreyi kümesi veya varsayılan değer 500 milisaniye istenebilir.

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

Cihaz titreşimi iptali ile istenebilir `Cancel` yöntemi:

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

## <a name="platform-differences"></a>Platform farklar

| Platform | Fark |
| --- | --- |
| iOS | Her zaman için 500 milisaniye Titreşim çıkardığı. |
| iOS | Titreşim iptal etmek mümkün değildir. |

## <a name="api"></a>API

- [Titreşim kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Titreşim API belgeleri](xref:Xamarin.Essentials.Vibration)
