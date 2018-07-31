---
title: 'Xamarin.Essentials: Fener yeri'
description: Bu belge açma veya cihazın kamerasını bir Fener yeri açmak için flash kapatma olanağı vardır Xamarin.Essentials, Fener yeri sınıfında açıklar.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8c471f64c14a2e41693c450e02f89e7ac845d060
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353366"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials: Fener yeri

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Fener yeri** sınıfı üzerinde veya cihazın kamerasını bir Fener yeri açmak için flash kapatma olanağı vardır.

## <a name="getting-started"></a>Başlarken

Erişim için **Fener yeri** işlevselliğini aşağıdaki platforma özgü Kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Fener yeri ve kamera izinler gereklidir ve Android projede yapılandırılması gerekir. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

YA da Android bildirimini güncelleştir:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve içine aşağıdaki **bildirim** düğümü.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Android projeye sağ tıklayın ve proje özelliklerini açın. Altında **Android bildirim** Bul **gerekli izinler:** alan ve onay **Fener yeri** ve **kamera** izinleri. Bu otomatik olarak güncelleştirecektir **AndroidManifest.xml** dosya.

Bu izinleri ekleyerek [Google Play otomatik olarak cihazlarıyla dışarıdan filtre](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) belirli donanım olmadan. Bu sorunu, uygulamanızı AssemblyInfo.cs dosyası Android projenize aşağıdaki ekleyerek alabilirsiniz:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulum gerekli.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulum gerekli.

-----

## <a name="using-flashlight"></a>Fener yeri kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Fener yeri aracılığıyla açılıp kapatılabilir `TurnOnAsync` ve `TurnOffAsync` yöntemleri:

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## <a name="platform-implementation-specifics"></a>Platform uygulaması özellikleri

### <a name="androidtabandroid"></a>[Android](#tab/android)

Cihazın işletim sistemi temelinde Fener yeri sınıfı optimize edilmiştir.

#### <a name="api-level-23-and-higher"></a>API düzeyi 23 ve üzeri

Yeni API düzeyleri [Torch modu](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) cihazın flash birim açıp kapatması için kullanılacak.

#### <a name="api-level-22-and-lower"></a>API düzeyi 22 ve daha düşük

Kamera yüzey doku etkinleştirmek veya devre dışı bırakmak için oluşturulan `FlashMode` kamera birimi. 

### <a name="iostabios"></a>[iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) açma ve kapatma Torch ve cihazın Flash modu için kullanılır.

### <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[Lamp](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) arkasındaki etkinleştirmek veya devre dışı bırakmak için cihazın ilk lamp algılamak için kullanılır.

-----

## <a name="api"></a>API

- [Fener yeri kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Fener yeri API belgeleri](xref:Xamarin.Essentials.Flashlight)
