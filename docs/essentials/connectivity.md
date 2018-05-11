---
title: Xamarin.Essentials bağlantısı
description: Bağlantı sınıfı, cihazın ağ koşulları değişiklikleri izlemek, geçerli ağ erişimi nasıl, şu anda bağlı olduğu denetleyip sağlar.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: fd757bec32d2854d2c9693812dece05ef11e2f80
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials bağlantısı

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

**Bağlantı** cihazın ağ koşulları değişiklikleri izlemek sınıfı sağlar, geçerli ağ erişimi ve nasıl, şu anda bağlı olduğu denetleyin.

## <a name="getting-started"></a>Başlarken

Erişim için **bağlantı** işlevselliği aşağıdaki platform özel kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` İzni gereklidir ve Android projesinde yapılandırılması gerekir. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

YA da güncelleştirme Android bildirim:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve aşağıdaki içine ekleyin **bildirim** düğümü.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Veya Anroid projeye sağ tıklayın ve projenin özelliklerini açın. Altında **Android derleme bildirimi** Bul **gerekli izinler:** alan ve onay **erişimi ağ durumu** izni. Bu otomatik olarak güncelleştirilecek **AndroidManifest.xml** dosya.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulumu gerekmez.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulumu gerekmez.

-----

## <a name="using-connectivity"></a>Bağlantı kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Geçerli ağ erişimini denetle:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Ağ erişimi](xref:Xamarin.Essentials.NetworkAccess) Aşağıdaki kategorilerde döner:

* **Internet** – yerel ve internet erişimi.
* **ConstrainedInternet** – Internet erişimi sınırlı. Burada web portalına yerel erişim sağlanır, ancak belirli kimlik bilgileri bir portal üzerinden sağlanan Internet erişimi gerektirir captive portal bağlantısı gösterir.
* **Yerel** – yerel ağ erişimi yalnızca.
* **Hiçbiri** – hiç bağlantı yok.
* **Bilinmeyen** – internet bağlantısı belirlenemiyor.

Ne tür denetleyebilirsiniz [bağlantı profili](xref:Xamarin.Essentials.ConnectionProfile) aygıt etkin olarak kullanan:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Bağlantı profili veya ağ erişim değiştiğinde tetiklendiğinde olay alabilirsiniz:

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(ConnectivityChangedEventArgs  e)
    {
        var access = e.NetworkAccess;
        var profiles = e.Profiles;
    }
}
```

## <a name="limitations"></a>Sınırlamalar

Olası olduğunu dikkate almak önemlidir, `Internet` tarafından bildirilen `NetworkAccess` ancak web tam erişim kullanılabilir değil. Bağlantı her platformda nasıl çalıştığını nedeniyle, yalnızca bir bağlantının kullanılabilir olması garanti edebilir. Aygıt bir Wi-Fi ağına örneği için bağlı ancak internet'ten yönlendirici bağlantısı kesildi. Bu örnekte Internet bildirilebilir ancak etkin bir bağlantı yok.

## <a name="api"></a>API

* [Bağlantı kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Essentials/Connectivity)
* [Bağlantı API belgeleri](xref:Xamarin.Essentials.Connectivity)
