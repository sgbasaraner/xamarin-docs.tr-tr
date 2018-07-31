---
title: Xamarin.iOS, ARKit giriş
description: Bu belgede, iOS 11 ARKit genişletilmiş gerçeklikte açıklanmaktadır. Bu, bir uygulamaya bir 3B model eklemek, görünümü yapılandırma, oturum temsilci uygulamak, dünyada 3B modeli konumlandırmak ve genişletilmiş gerçeklik oturumunu Duraklat nasıl ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 14bbb35477c098738e9cd7e2cb92154422d394ee
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350621"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Xamarin.iOS, ARKit giriş

_İOS 11 için genişletilmiş gerçeklik_

ARKit genişletilmiş gerçeklik uygulamaları ve oyunları çok çeşitli sağlar. Bu bölümde aşağıdaki konuları içerir:

- [ARKit ile çalışmaya başlama](#gettingstarted)
- [ARKit ile UrhoSharp kullanma](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>ARKit ile çalışmaya başlama

Aşağıdaki yönergeleri ile genişletilmiş gerçeklik kullanmaya başlamak için bir basit uygulamasında izlenecek yol: 3B model konumlandırma ve modeli, izleme işlevselliğini korumak ARKit sağlar.

![Görüntü kameranın kayan jet 3B modeli](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. 3B model ekleme

Varlıklar ile projeye eklenmelidir **SceneKitAsset** derleme eylemi.

![SceneKit varlıklarını bir proje](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Görünümü yapılandırma

Görünüm denetleyicinin içinde `ViewDidLoad` yöntemi, Sahne varlık yükleme ve ayarlama `Scene` görünümü özelliği:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. İsteğe bağlı olarak bir oturum temsilci uygulamak

Basit durumlar için gerekli değildir, ancak bir oturum temsilci uygulama durumu ARKit oturumun (ve kullanıcıya geri bildirim sağlayan gerçek uygulamalar) hata ayıklama için yararlı olabilir. Aşağıdaki kodu kullanarak basit temsilci oluşturun:

```csharp
public class SessionDelegate : ARSessionDelegate
{
  public SessionDelegate() {}
  public override void CameraDidChangeTrackingState(ARSession session, ARCamera camera)
  {
    Console.WriteLine("{0} {1}", camera.TrackingState, camera.TrackingStateReason);
  }
}
```

Temsilci atama içinde `ViewDidLoad` yöntemi:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. 3B modeli dünyanın getirin

İçinde `ViewWillAppear`, aşağıdaki kod bir ARKit oturumu oluşturur ve cihazın kamerasını göreli 3B modeli konumunu ayarlar:

```csharp
// Create a session configuration
var configuration = new ARWorldTrackingConfiguration {
  PlaneDetection = ARPlaneDetection.Horizontal,
  LightEstimationEnabled = true
};

// Run the view's session
SceneView.Session.Run(configuration, ARSessionRunOptions.ResetTracking);

// Find the ship and position it just in front of the camera
var ship = SceneView.Scene.RootNode.FindChildNode("ship", true);

ship.Position = new SCNVector3(2f, -2f, -9f);
```

Uygulamayı çalıştırın ya da devam, her zaman 3B modeli kamera önüne yerleştirilir. Model konumlandırılmış kamerayı hareket ve ARKit konumlandırılmış modeli tutar izleyin.

### <a name="5-pause-the-augmented-reality-session"></a>5. Genişletilmiş gerçeklik oturumunu Duraklat

Görünüm denetleyicisi olmadığında görünür ARKit oturumunu Duraklat iyi bir uygulamadır (içinde `ViewWillDisappear` yöntemi:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Özet

Yukarıdaki kod, bir basit ARKit uygulamasında sonuçlanır. Daha karmaşık örnekler uygulamak için genişletilmiş gerçeklik oturumu barındırma görünüm denetleyicisi beklediğiniz `IARSCNViewDelegate`, ve ek yöntemleri uygulanmasını.

ARKit birçok yüzey izleme ve kullanıcı etkileşimi gibi daha gelişmiş özellikler sunar. Bkz: [UrhoSharp tanıtım](urhosharp.md) ile UrhoSharp izleme ARKit birleştiren bir örnek.


## <a name="related-links"></a>İlgili bağlantılar

- [Genişletilmiş gerçeklik (Apple)](https://developer.apple.com/arkit/)
- [ARKit ile UrhoSharp kullanma](urhosharp.md)
- [Basit ARKit (Jet) örneği](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit yerleştirme nesneleri (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [ARKit - iOS (WWDC) (video) için genişletilmiş gerçeklik Tanıtımı](https://developer.apple.com/videos/play/wwdc2017/602/)
