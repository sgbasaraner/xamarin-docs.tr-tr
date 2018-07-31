---
title: 'Xamarin.Essentials: bağlantı'
description: Xamarin.Essentials bağlantı sınıfında, cihazın ağ koşulları değişiklikler için izleme, geçerli ağ erişimi ve bu şu anda nasıl bağlandığını denetimi sağlar.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 96b4ee0487034c651bec1dfb168fed7567b63c96
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353704"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: bağlantı

![NuGet yayın öncesi](~/media/shared/pre-release.png)

**Bağlantı** cihazın ağ koşulları, değişiklikler için izleme sınıfı sağlar, geçerli ağ erişimi ve bu şu anda nasıl bağlandığını denetleyin.

## <a name="getting-started"></a>Başlarken

Erişim için **bağlantı** işlevselliğini aşağıdaki platforma özgü Kurulum gereklidir.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` İzni gereklidir ve Android projede yapılandırılması gerekir. Bu, aşağıdaki yollarla eklenebilir:

Açık **AssemblyInfo.cs** altında dosya **özellikleri** klasör ekleyin:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

YA da Android bildirimini güncelleştir:

Açık **AndroidManifest.xml** altında dosya **özellikleri** klasörü ve içine aşağıdaki **bildirim** düğümü.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Android projeye sağ tıklayın ve proje özelliklerini açın. Altında **Android bildirim** Bul **gerekli izinler:** alan ve onay **erişim ağ durumu** izni. Bu otomatik olarak güncelleştirecektir **AndroidManifest.xml** dosya.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ek kurulum gerekli.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ek kurulum gerekli.

-----

## <a name="using-connectivity"></a>Bağlantı kullanma

Sınıfınızda Xamarin.Essentials bir başvuru ekleyin:

```csharp
using Xamarin.Essentials;
```

Geçerli ağ erişim denetimi:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Ağ erişimi](xref:Xamarin.Essentials.NetworkAccess) Aşağıdaki kategorilerde döner:

* **Internet** – yerel ve internet erişimi.
* **ConstrainedInternet** – sınırlı internet erişimi. Burada bir web portalını yerel erişim sağlanır, ancak belirli kimlik bilgilerini Portalı aracılığıyla sağlanan Internet erişimi gerektirir captive portal bağlantı gösterir.
* **Yerel** – yerel ağ erişimi yalnızca.
* **Hiçbiri** – bağlantısı olmadığında kullanılabilir.
* **Bilinmeyen** – internet bağlantısı belirlenemiyor.

Ne tür denetleyebilirsiniz [bağlantı profili](xref:Xamarin.Essentials.ConnectionProfile) cihaz etkin olarak kullanan:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Bağlantı profili veya ağ erişimi değiştiğinde tetikleyen bir olay alabilirsiniz:

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

Olası olduğuna dikkat edin önemlidir, `Internet` tarafından bildirilen `NetworkAccess` ancak tam web erişim kullanılabilir değil. Her platformda bağlantı nasıl çalıştığını nedeniyle, yalnızca bir bağlantı kullanılabilir olduğunu garanti edebilir. Cihaz bir Wi-Fi ağına örneği için bağlı, ancak internet'ten yönlendirici bağlantısı kesildi. Bu örnekte, Internet bildirilebilir, ancak etkin bir bağlantı kullanılamıyor.

## <a name="api"></a>API

* [Bağlantı kaynak kodu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Bağlantı API belgeleri](xref:Xamarin.Essentials.Connectivity)
