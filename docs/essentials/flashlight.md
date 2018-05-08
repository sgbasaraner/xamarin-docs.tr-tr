---
title: Xamarin.Essentials el feneri
description: El feneri sınıfı üzerinde veya cihazın kamera işaret feneri açmak için flash kapatma olanağı vardır.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3d867d742314f2677eeab35bbc354f696c99bf75
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials el feneri

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**El feneri** sınıfı üzerinde veya cihazın kamera işaret feneri açmak için flash kapatma olanağı vardır.

## <a name="getting-started"></a>Başlarken

Erişim için **el feneri** işlevselliği aşağıdaki platform özel kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

El feneri ve kamera izinler gereklidir ve Android projesinde yapılandırılması gerekir. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

YA da güncelleştirme Android bildirim:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve aşağıdaki içine ekleyin **bildirim** düğümü.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Veya Anroid projeye sağ tıklayın ve projenin özelliklerini açın. Altında **Android derleme bildirimi** Bul **gerekli izinler:** alan ve onay **el FENERİ** ve **kamera** izinleri. Bu otomatik olarak güncelleştirilecek **AndroidManifest.xml** dosya.

Bu izinler ekleyerek [Google Play otomatik olarak aygıtları filtre](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) belirli donanım olmadan. Bu sorunu Android projenizin AssemblyInfo.cs dosyanızda aşağıdaki ekleyerek alabilirsiniz:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulumu gerekmez.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulumu gerekmez.

-----

## <a name="using-flashlight"></a>El feneri kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

El feneri aracılığıyla açma ve kapatma açılabilir `TurnOnAsync` ve `TurnOffAsync` yöntemleri:

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

## <a name="platform-implementation-specifics"></a>Platform uygulama özellikleri

### <a name="androidtabandroid-specifics"></a>[Android](#tab/android-specifics)

Cihazın işletim sistemi temelinde optmized el feneri sınıfı olmuştur.

#### <a name="api-level-23-and-higher"></a>API düzeyi 23 ve üzeri

Yeni API düzeyde [Torch modu](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) açmak veya kapatmak cihaz flash ölçü için kullanılacak.

#### <a name="api-level-22-and-lower"></a>API düzeyi 22 ve daha düşük

Kamera yüzey dokusunu açmak veya kapatmak için oluşturduğunuz `FlashMode` kamera birimi. 

### <a name="iostabios-specifics"></a>[iOS](#tab/ios-specifics)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) üzerinde ve Torch ve aygıtın Flash modunu devre dışı bırakma için kullanılır.

### <a name="uwptabuwp-specifics"></a>[UWP](#tab/uwp-specifics)

[Ampul](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) açmak veya kapatmak için aygıtın arkasında ilk ampul algılamak için kullanılır.

-----

## <a name="api"></a>API

- [El feneri kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Essentials/Flashlight)
- [El feneri API belgeleri](xref:Xamarin.Essentials.Flashlight)
