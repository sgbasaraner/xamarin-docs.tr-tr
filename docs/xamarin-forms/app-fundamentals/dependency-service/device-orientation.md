---
title: Cihaz yönünü denetleme
description: Bu makalede, cihaz yönü paylaşılan koddan erişmek için Xamarin.Forms DependencyService sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 620404a217b2e8a31192ae6613dcec023ac366cd
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995647"
---
# <a name="checking-device-orientation"></a>Cihaz yönünü denetleme

Bu makalede kullanmak için size yol gösterecek [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) cihaz yönü her platformda yerel API'lerini kullanarak paylaşılan koddan denetlemek için. Bu izlenecek yol var olan temel `DeviceOrientation` eklenti tarafından Ali Özgür. Bkz: [GitHub deposunu](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) daha fazla bilgi için.

- **[Arabirimi oluşturma](#Creating_the_Interface)**  &ndash; anlamak arabirimine nasıl paylaşılan kod oluşturulur.
- **[iOS uygulaması](#iOS_Implementation)**  &ndash; iOS için yerel kod içinde arabirim uygulamak hakkında bilgi edinin.
- **[Android uygulaması](#Android_Implementation)**  &ndash; arabirimi, Android için yerel koda uygulanması hakkında bilgi edinin.
- **[UWP uygulaması](#WindowsImplementation)**  &ndash; arabirimi, Evrensel Windows Platformu (UWP) için yerel koda uygulanması hakkında bilgi edinin.
- **[Paylaşılan kod içinde uygulama](#Implementing_in_Shared_Code)**  &ndash; nasıl kullanacağınızı öğrenin `DependencyService` paylaşılan kod yerel uygulamasından çağırmak için.

Uygulamayı kullanarak `DependencyService` aşağıdaki yapıya sahip:

![](device-orientation-images/orientation-diagram.png "DependencyService uygulama yapısı")

> [!NOTE]
> Gösterildiği gibi cihaz dikey veya yatay yönde paylaşılan kod içinde olup olmadığını algılamak mümkündür içinde [cihaz Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). Bu makalede açıklanan yöntemi, cihaz tersyüz olup olmadığı dahil yönlendirme hakkında daha fazla bilgi almak için yerel özellikleri kullanır.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Arabirimi oluşturma

İlk olarak bir arabirim uygulamak için planlama işlevselliğini ifade paylaşılan kodda oluşturun. Bu örnekte, tek bir yöntem arabirimi içerir:

```csharp
namespace DependencyServiceSample.Abstractions
{
    public enum DeviceOrientations
    {
        Undefined,
        Landscape,
        Portrait
    }

    public interface IDeviceOrientation
    {
        DeviceOrientations GetOrientation();
    }
}
```

Paylaşılan kodun bu arabirimde karşı kodlama her platformda cihaz yönü API'leri erişmek Xamarin.Forms uygulaması izin verir.

> [!NOTE]
> Arabirimini uygulayan sınıflar, çalışmak için parametresiz bir oluşturucusu olmalıdır `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS uygulaması

Her platforma özgü uygulama projesinde arabirimi uygulanmalıdır. Sınıfı, parametresiz bir oluşturucu olduğuna dikkat edin böylece `DependencyService` yeni örnekleri oluşturabilirsiniz:

```csharp
using UIKit;
using Foundation;

namespace DependencyServiceSample.iOS
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation(){ }

        public DeviceOrientations GetOrientation()
        {
            var currentOrientation = UIApplication.SharedApplication.StatusBarOrientation;
            bool isPortrait = currentOrientation == UIInterfaceOrientation.Portrait
                || currentOrientation == UIInterfaceOrientation.PortraitUpsideDown;

            return isPortrait ? DeviceOrientations.Portrait: DeviceOrientations.Landscape;
        }
    }
}
```

Son olarak, bu ekleme `[assembly]` öznitelik sınıfı yukarıda (ve tanımlanmış olan tüm ad alanlarını dışında) dahil olmak üzere tüm gerekli `using` ifadeleri:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Bu öznitelik sınıfı uygulaması kaydeder. `IDeviceOrientation` anlamına arabirimi `DependencyService.Get<IDeviceOrientation>` paylaşılan kodda bir örneğini oluşturmak için kullanılabilir.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android uygulaması

Aşağıdaki kod uygulayan `IDeviceOrientation` Android:

```csharp
using DependencyServiceSample.Droid;
using Android.Hardware;

namespace DependencyServiceSample.Droid
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public static void Init() { }

        public DeviceOrientations GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            var rotation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = rotation == SurfaceOrientation.Rotation90 || rotation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientations.Landscape : DeviceOrientations.Portrait;
        }
    }
}
```

Bu ekleme `[assembly]` öznitelik sınıfı yukarıda (ve tanımlanmış olan tüm ad alanlarını dışında) dahil olmak üzere tüm gerekli `using` ifadeleri:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Bu öznitelik sınıfı uygulaması kaydeder. `IDeviceOrientaiton` anlamına arabirimi `DependencyService.Get<IDeviceOrientation>` kullanılabilir paylaşılan kodun bir örneğini oluşturabilirsiniz.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Evrensel Windows platformu uygulaması

Aşağıdaki kod uygulayan `IDeviceOrientation` Evrensel Windows platformu arabiriminde:

```csharp
namespace DependencyServiceSample.WindowsPhone
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public DeviceOrientations GetOrientation()
        {
            var orientation = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            if (orientation == Windows.UI.ViewManagement.ApplicationViewOrientation.Landscape) {
                return DeviceOrientations.Landscape;
            }
            else {
                return DeviceOrientations.Portrait;
            }
        }
    }
}
```

Ekleme `[assembly]` öznitelik sınıfı yukarıda (ve tanımlanmış olan tüm ad alanlarını dışında) dahil olmak üzere tüm gerekli `using` ifadeleri:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Bu öznitelik sınıfı uygulaması kaydeder. `DeviceOrientationImplementation` anlamına arabirimi `DependencyService.Get<IDeviceOrientation>` kullanılabilir paylaşılan kodun bir örneğini oluşturabilirsiniz.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Paylaşılan kod içinde uygulama

Şimdi biz yazma ve test erişen paylaşılan kod `IDeviceOrientation` arabirimi. Bu basit sayfa cihaz yönlendirmeyi temel alarak kendi metni güncelleştiren bir düğme içerir. Kullandığı `DependencyService` örneğini almak için `IDeviceOrientation` arabirimi &ndash; çalışma zamanında bu örneği yerel SDK tam erişimi olan platforma özgü uygulama olacak:

```csharp
public MainPage ()
{
    var orient = new Button {
        Text = "Get Orientation",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    orient.Clicked += (sender, e) => {
       var orientation = DependencyService.Get<IDeviceOrientation>().GetOrientation();
       switch(orientation){
           case DeviceOrientations.Undefined:
                orient.Text = "Undefined";
                break;
           case DeviceOrientations.Landscape:
                orient.Text = "Landscape";
                break;
           case DeviceOrientations.Portrait:
                orient.Text = "Portrait";
                break;
       }
    };
    Content = orient;
}
```

Bu uygulama iOS, Android veya Windows platformları üzerinde çalışan ve düğmesine basarak sonuçlanır cihazın yönle güncelleştirme düğmenin metni.

![](device-orientation-images/orientation.png "Cihaz yönü örneği")


## <a name="related-links"></a>İlgili bağlantılar

- [DependencyService (örnek) kullanma](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (örnek)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms Örnekleri](https://github.com/xamarin/xamarin-forms-samples)
