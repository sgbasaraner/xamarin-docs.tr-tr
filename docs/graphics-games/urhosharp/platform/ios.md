---
title: "UrhoSharp iOS ve tvOS desteği"
description: "iOS ve tvOS özel kurulum ve UrhoSharp özellikleri."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 465fed25f360f29ad0b63146add8de939fa8924e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS ve tvOS desteği

_iOS ve tvOS özel kurulum ve özellikleri_

Taşınabilir sınıf kitaplığı Urho olan ve çeşitli platform oyun mantığınızı için kullanılmak üzere aynı API sağlar, hala Urho platform belirli sürücünüzü ve bazı durumlarda başlatmak için gerekir, platform belirli özelliklerden yararlanmak istersiniz .

Sayfalarında varsayımında `MyGame` sublcass biri olan `Application` sınıfı.

## <a name="ios-and-tvos"></a>iOS ve tvOS

**Desteklenen mimariler:** armv7, arm64, i386

## <a name="creating-a-project"></a>Proje Oluşturma

Bir iOS projesi oluşturun ve ardından veri kaynaklarını dizine ekleme ve tüm dosyaları olduğundan emin olun **BundleResource** olarak **yapı eylemi**.

![Proje Kurulum](ios-images/image-4.png "kaynakları dizine veri ekleme")

## <a name="configuring-and-launching-urho"></a>Urho başlatma ve yapılandırma

Using deyimleri için ekleme `Urho` ve `Urho.iOS` ad alanları ve ardından uygulamanızı başlatma yanı sıra Urho başlatma için bu kodu ekleyin:

```csharp
new MyGame().Run();
```

İOS bekliyor beri dikkat `FinishedLaunching` tamamlamak için çağrısı sıraya `Run()` yöntemi tamamlandıktan sonra bu ortak bir deyim çalıştırmaktır:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    LaunchGame();
    return true;
}

async void LaunchGame()
{
    await Task.Yield();
    new SamplyGame().Run();
}
```

Varsayılan iOS PNG iyileştirici Urho değil şu anda düzgün bir şekilde tüketebileceği görüntüleri oluşturur çünkü PNG iyileştirmeleri devre dışı önemlidir

## <a name="custom-embedding-of-urho"></a>Özel Urho katıştırma

Alternatif olarak zorunda Urho tüm uygulama ekran üzerinde sürebilir ve uygulamanızın bir bileşeni olarak kullanmak için oluşturabileceğiniz bir `UrhoSurface` olduğu bir `UIView` , mevcut uygulamanızda da eklenebilir.

Bu yapmanız gerekir.

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

Bu nedenle Urho sınıfınız barındıracak sonra bunu yapmanız:

```csharp
new MyGame().Run ();
```

