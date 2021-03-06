---
title: UrhoSharp Windows desteği
description: Bu belge UrhoSharp için Windows Destek açıklanır. Proje oluşturma, yapılandırma açıklar ve Urho başlatın, WPF ile tümleştirmek ve UWP ile tümleştirin.
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 094eaf0ebe84ce8c1771bd6481ee897463349856
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783239"
---
# <a name="urhosharp-windows-support"></a>UrhoSharp Windows desteği

Taşınabilir sınıf kitaplığı Urho olan ve çeşitli platform oyun mantığınızı için kullanılmak üzere aynı API sağlar, hala Urho platform belirli sürücünüzü ve bazı durumlarda başlatmak için gerekir, platform belirli özelliklerden yararlanmak istersiniz .

Sayfalarında varsayımında `MyGame` sınıfıdır `Application` sınıfı.

**Desteklenen mimariler:** yalnızca 64 bit Windows.

Bu konuda kullanmayı gösteren tam örnek görebilirsiniz bizim [örnekleri](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

## <a name="standalone-project"></a>Tek başına proje

### <a name="creating-a-project"></a>Proje Oluşturma

Bir konsol projesi oluşturun, Urho NuGet başvuru ve varlıkları (veri dizinini içeren dizinler) bulabilir emin olun.

### <a name="configuring-and-launching-urho"></a>Urho başlatma ve yapılandırma

Uygulamanızı başlatmak için bunu yapın:

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

### <a name="example"></a>Örnek

[Tam örnek](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

## <a name="integrated-with-wpf"></a>WPF ile tümleşik

### <a name="creating-a-project"></a>Proje Oluşturma

Bir WPF projesi oluşturma Urho NuGet başvuru ve (veri dizinini içeren dizinler) varlıklar bulabilir emin olun.

### <a name="configuring-and-launching-urho-from-wpf"></a>WPF gelen Urho başlatma ve yapılandırma

Öğesinin bir alt kümesi oluşturmak `Window` ve varlıklarınızı bu gibi yapılandırın:

```csharp
    public partial class MainWindow : Window
    {
        Application currentApplication;

        public MainWindow()
        {
            InitializeComponent();
            DesktopUrhoInitializer.AssetsDirectory = @"../../Assets";
            Loaded += (s,e) => RunGame (new MyGame ());
        }

        async void RunGame(MyGame game)
        {
            var urhoSurface = new Panel { Dock = DockStyle.Fill };
            WindowsFormsHost.Child = urhoSurface;
            WindowsFormsHost.Focus();
            urhoSurface.Focus();
            await Task.Yield();
            var appOptions = new ApplicationOptions(assetsFolder: "Data")
                {
                    ExternalWindow = RunInSdlWindow.IsChecked.Value ? IntPtr.Zero : urhoSurface.Handle,
                    LimitFps = false, //true means "limit to 200fps"
                };
            currentApplication = Urho.Application.CreateInstance(value.Type, appOptions);
            currentApplication.Run();
        }
    }
```

### <a name="example"></a>Örnek

[Tam örnek](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/WPF)

## <a name="integrated-with-uwp"></a>UWP ile tümleşik

### <a name="creating-a-project"></a>Proje Oluşturma

UWP projesi oluşturun, Urho NuGet başvuru ve varlıkları (veri dizinini içeren dizinler) bulabilir emin olun.

### <a name="configuring-and-launching-urho-from-uwp"></a>UWP gelen Urho başlatma ve yapılandırma

Öğesinin bir alt kümesi oluşturmak `Window` ve varlıklarınızı bu gibi yapılandırın:

```csharp
{
            InitializeComponent();
            GameTypes = typeof(Sample).GetTypeInfo().Assembly.GetTypes()
                .Where(t => t.GetTypeInfo().IsSubclassOf(typeof(Application)) && t != typeof(Sample))
                .Select((t, i) => new TypeInfo(t, $"{i + 1}. {t.Name}", ""))
                .ToArray();
            DataContext = this;
            Loaded += (s, e) => RunGame (new MyGame ());
        }

        public void RunGame(TypeInfo value)
        {
            //at this moment, UWP supports assets only in pak files (see PackageTool)
            currentApplication = UrhoSurface.Run(value.Type, "Data.pak");
        }
    }
```

### <a name="example"></a>Örnek

[Tam örnek](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/UWP)

## <a name="integrated-with-windowsforms"></a>Windows.Forms ile tümleşik

### <a name="creating-a-project"></a>Proje Oluşturma

Windows.Forms projesi oluşturun, Urho NuGet başvuru ve varlıkları (veri dizinini içeren dizinler) bulabilir emin olun.

### <a name="configuring-and-launching-urho-from-windowsforms"></a>Yapılandırma ve Urho Windows.Forms öğesinden başlatma

Formdan Urho başlatmak için bkz: [tam örnek](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)
