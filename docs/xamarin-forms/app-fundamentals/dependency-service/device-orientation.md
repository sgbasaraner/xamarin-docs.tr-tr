---
title: Cihaz yönlendirmesini denetleniyor
description: DependencyService paylaşılan kodundan erişim cihaz yönlendirmesini kullanın
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: b8392dad578f94380e90da24cbf44120d38f754d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="checking-device-orientation"></a>Cihaz yönlendirmesini denetleniyor

Bu makalede kullanmak için size yol gösterecektir [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) her platformda yerel API'lerini kullanarak paylaşılan kodundan cihaz yönlendirmesini denetlemek için. Var olan bu kılavuz temel `DeviceOrientation` Ali Özgür göre eklenti. Bkz: [GitHub deposuna](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) daha fazla bilgi için.

- **[Arabirimi oluşturma](#Creating_the_Interface)**  &ndash; anlamak arabirimi nasıl paylaşılan kod içinde oluşturulur.
- **[iOS uygulaması](#iOS_Implementation)**  &ndash; iOS için yerel kodda arabirimini uygulayan öğrenin.
- **[Android uygulaması](#Android_Implementation)**  &ndash; arabirimini yerel kodda Android için uygulama öğrenin.
- **[Windows uygulaması](#WindowsImplementation)**  &ndash; Windows Phone ve evrensel Windows Platformu (UWP) için yerel kodda arabirimini uygulayan öğrenin.
- **[Paylaşılan kod içinde uygulama](#Implementing_in_Shared_Code)**  &ndash; nasıl kullanacağınızı öğrenin `DependencyService` paylaşılan koddan yerel uygulama çağırmak için.

Uygulamayı kullanarak `DependencyService` aşağıdaki yapı ayarlanmıştır:

![](device-orientation-images/orientation-diagram.png "DependencyService uygulama yapısı")

> [!NOTE]
> Cihaz dikey veya yatay yönde paylaşılan kodda gösterildiği gibi gerekmediğini mümkündür içinde [aygıt Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). Bu makalede açıklanan yöntemi yerel özellikleri cihazın ters olup olmadığı dahil olmak üzere yönlendirme hakkında daha fazla bilgi almak için kullanır.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Arabirimi oluşturma

İlk olarak, bir arabirim uygulamak için planlama işlevselliği ifade paylaşılan kod içinde oluşturun. Bu örnekte, tek bir yöntem arabirimi içerir:

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

Bu arabirim paylaşılan kodda karşı kodlama Xamarin.Forms uygulaması her platformda cihaz yönlendirmesini API'leri erişmesine izin verir.

> [!NOTE]
> Arabirimini uygulayan sınıflar çalışmak için parametresiz bir oluşturucusu olmalıdır `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS uygulaması

Arabirim her platforma özgü uygulama projesinde uygulanmalıdır. Sınıf parametresiz bir oluşturucuya sahip Not böylece `DependencyService` yeni örnekleri oluşturabilirsiniz:

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

Son olarak, bu eklemek `[assembly]` öznitelik sınıfı yukarıda (ve tanımlanmış tüm ad alanlarını dışında) dahil olmak üzere tüm gerekli `using` deyimleri:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Bu öznitelik sınıfı uygulaması kaydeder `IDeviceOrientation` anlamına arabirimi `DependencyService.Get<IDeviceOrientation>` paylaşılan kodda bir örneğini oluşturmak için kullanılabilir.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android uygulaması

Aşağıdaki kod uygulayan `IDeviceOrientation` android'de:

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

Bu ekleme `[assembly]` öznitelik sınıfı yukarıda (ve tanımlanmış tüm ad alanlarını dışında) dahil olmak üzere tüm gerekli `using` deyimleri:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Bu öznitelik sınıfı uygulaması kaydeder `IDeviceOrientaiton` anlamına arabirimi `DependencyService.Get<IDeviceOrientation>` kullanılabilir paylaşılan kod bir örneğini oluşturabilirsiniz.

<a name="WindowsImplementation" />

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone ve evrensel Windows Platform uygulaması

Aşağıdaki kod uygulayan `IDeviceOrientation` arabirimi Windows Phone ve evrensel Windows Platformu:

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

Ekleme `[assembly]` öznitelik sınıfı yukarıda (ve tanımlanmış tüm ad alanlarını dışında) dahil olmak üzere tüm gerekli `using` deyimleri:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Bu öznitelik sınıfı uygulaması kaydeder `DeviceOrientationImplementation` anlamına arabirimi `DependencyService.Get<IDeviceOrientation>` kullanılabilir paylaşılan kod bir örneğini oluşturabilirsiniz.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Paylaşılan kod içinde uygulama

Biz yazma ve erişen paylaşılan kodu test artık `IDeviceOrientation` arabirimi. Bu basit sayfası üzerinde aygıt yönlendirme göre kendi metin güncelleştirmeleri bir düğme içerir. Kullandığı `DependencyService` örneğini almak için `IDeviceOrientation` arabirimi &ndash; çalışma zamanında bu örneği yerel SDK tam erişime sahip platforma özgü uygulaması olacaktır:

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

Bu uygulamayı iOS, Android veya Windows platformları çalıştıran ve düğmesine basarak neden olur cihazın yönü güncelleştirme düğmenin metni.

![](device-orientation-images/orientation.png "Cihaz yönlendirmesini örnek")


## <a name="related-links"></a>İlgili bağlantılar

- [DependencyService (örnek) kullanma](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (örnek)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms Örnekleri](https://github.com/xamarin/xamarin-forms-samples)
