---
title: UrhoSharp Android desteği
description: Bu belgede, Android özgü Kurulum ve UrhoSharp için özellik güvenlikle ilgili bilgiler açıklanmaktadır. Özellikle, desteklenen mimariler anlatılmaktadır yapılandırma ve Urho ve özel Urho katıştırma başlatmasını bir proje oluşturma.
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 6e489f52712989b5f94fa52d5ec6f22a13ce6252
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783788"
---
# <a name="urhosharp-android-support"></a>UrhoSharp Android desteği

_Android özel kurulum ve özellikleri_

Taşınabilir sınıf kitaplığı Urho olan ve çeşitli platform oyun mantığınızı için kullanılmak üzere aynı API sağlar, hala Urho platform belirli sürücünüzü ve bazı durumlarda başlatmak için gerekir, platform belirli özelliklerden yararlanmak istersiniz .

Sayfalarında varsayımında `MyGame` sınıfıdır `Application` sınıfı.

## <a name="architectures"></a>Mimariler

**Desteklenen mimariler**: x86, pushservice, pushservice-v7a

## <a name="create-a-project"></a>Proje oluşturma

Bir Android projesi oluşturun ve UrhoSharp NuGet paketini ekleyin.

Veri varlıklarınız için içeren eklemek **varlıklar** dizin ve emin olun dosyalarının tümüne sahip **AndroidAsset** olarak **yapı eylemi**.

![Proje Kurulum](android-images/image-3.png "varlıklar dizinine varlıkları içeren veri ekleme")

## <a name="configure-and-launching-urho"></a>Yapılandırma ve Urho başlatma

Using deyimleri için ekleme `Urho` ve `Urho.Android` ad alanları ve ardından uygulamanızı başlatma yanı sıra Urho başlatma için bu kodu ekleyin.

Bir oyun MyGame sınıfında uygulandığı şekilde çalıştırmak için en basit yolu çağırmaktır

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

Bu işlem, içerik olarak oyuna tam ekran etkinlik açar.

## <a name="custom-embedding-of-urho"></a>Özel Urho katıştırma

Alternatif olarak zorunda Urho tüm uygulama ekran üzerinde sürebilir ve uygulamanızın bir bileşeni olarak kullanmak için oluşturabileceğiniz bir `SurfaceView` aracılığıyla:

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

Bazı olaylar, etkinlik UrhoSharp için iletme gerekecektir örneğin:

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

Aynı yapmanız gereken: `OnResume`, `OnPause`, `OnLowMemory`, `OnDestroy`, `DispatchKeyEvent` ve `OnWindowFocusChanged`.

Oyun başlatır tipik bir etkinliği gösterir:

```csharp
[Activity(Label = "MyUrhoApp", MainLauncher = true,
    Icon = "@drawable/icon", Theme = "@android:style/Theme.NoTitleBar.Fullscreen",
    ConfigurationChanges = ConfigChanges.KeyboardHidden | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Portrait)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        var mLayout = new AbsoluteLayout(this);
        var surface = UrhoSurface.CreateSurface<MyUrhoApp>(this);
        mLayout.AddView(surface);
        SetContentView(mLayout);
    }

    protected override void OnResume()
    {
        UrhoSurface.OnResume();
        base.OnResume();
    }

    protected override void OnPause()
    {
        UrhoSurface.OnPause();
        base.OnPause();
    }

    public override void OnLowMemory()
    {
        UrhoSurface.OnLowMemory();
        base.OnLowMemory();
    }

    protected override void OnDestroy()
    {
        UrhoSurface.OnDestroy();
        base.OnDestroy();
    }

    public override bool DispatchKeyEvent(KeyEvent e)
    {
        if (!UrhoSurface.DispatchKeyEvent(e))
            return false;
        return base.DispatchKeyEvent(e);
    }

    public override void OnWindowFocusChanged(bool hasFocus)
    {
        UrhoSurface.OnWindowFocusChanged(hasFocus);
        base.OnWindowFocusChanged(hasFocus);
    }
}
```

