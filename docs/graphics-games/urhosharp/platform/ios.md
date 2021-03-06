---
title: UrhoSharp iOS ve tvOS desteği
description: Bu belgede iOS açıklanır ve UrhoSharp için tvOS desteği. Proje oluşturma, yapılandırma ve Urho başlatın ve Urho, özel bir embed gerçekleştirmek nasıl açıklar.
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 7e8975b6885f6c902634e05aafca0b8ee60a981c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783981"
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS ve tvOS desteği

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

