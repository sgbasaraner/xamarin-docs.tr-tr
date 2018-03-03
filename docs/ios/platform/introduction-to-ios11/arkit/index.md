---
title: "ARKit giriş"
description: "Genişletilmiş gerçekte iOS 11"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 1655d426a0181c88479eef2bdea909eb5935e642
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-arkit"></a>ARKit giriş

_Genişletilmiş gerçekte iOS 11_

ARKit çok çeşitli genişletilmiş gerçekte uygulamaları ve oyunları sağlar. Bu bölüm aşağıdaki konuları içerir:

- [ARKit ile çalışmaya başlama](#gettingstarted)
- [ARKit UrhoSharp ile kullanma](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>ARKit ile çalışmaya başlama

Genişletilmiş gerçekte ile çalışmaya başlamak için aşağıdaki yönergeleri basit bir uygulama yol: bir 3B modeli konumlandırma ve model izleme işlevselliği ile bir yerde tutun ARKit izin vererek.

![Görüntü kameranın kayan jet 3D modeli](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. 3B model ekleme

Varlıklar ile projeye eklenmesi **SceneKitAsset** derleme eylemi.

![Bir projedeki SceneKit varlıklar](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Görünümü yapılandırma

Görünüm denetleyicinin içinde `ViewDidLoad` yöntemi, Sahne varlık yükleme ve ayarlama `Scene` görünümü özelliği:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. İsteğe bağlı olarak bir oturum temsilci uygulama

Basit durumlar için gerekli değildir, ancak bir oturum temsilci uygulama durumu ARKit oturumunun (ve kullanıcıya geri bildirim sağlayan gerçek uygulamalar) hata ayıklama için yararlı olabilir. Aşağıdaki kodu kullanarak basit bir temsilci oluşturun:

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

Temsilci atamak içinde `ViewDidLoad` yöntemi:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. 3B modeli dünyada getirin

İçinde `ViewWillAppear`, aşağıdaki kod bir ARKit oturumu oluşturur ve cihazın kamera göre alanındaki 3D modeli konumunu ayarlar:

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

Uygulamayı çalıştırın ya da sürdürüldü, her zaman 3D modeli kamera önüne yerleştirilir. Model konumlandırılmış sonra kamera taşıyın ve konumlandırılmış modeli ARKit tutar olarak izleyebilir.

### <a name="5-pause-the-augmented-reality-session"></a>5. Genişletilmiş gerçekte oturum duraklatma

Görünüm denetleyicisini olmadığında görünür ARKit oturum duraklatmak için iyi bir uygulamadır (içinde `ViewWillDisappear` yöntemi:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Özet

Yukarıdaki kod basit bir ARKit uygulama sonuçlanır. Daha karmaşık örnekler uygulamak için genişletilmiş gerçekte oturumu barındıran görünüm denetleyicisini beklediğiniz `IARSCNViewDelegate`, ek yöntemleri uygulamıştır.

ARKit çok sayıda yüzey izleme ve kullanıcı etkileşimi gibi daha gelişmiş özellikleri sağlar. Bkz: [UrhoSharp demo](urhosharp.md) UrhoSharp ile izleme ARKit birleştiren bir örnek.


## <a name="related-links"></a>İlgili bağlantılar

- [Genişletilmiş gerçekte (Apple)](https://developer.apple.com/arkit/)
- [ARKit UrhoSharp ile kullanma](urhosharp.md)
- [Basit ARKit (Jet) örnek](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit yerleştirme nesneleri (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [ARKit - iOS (WWDC) (video) için genişletilmiş gerçekte Tanıtımı](https://developer.apple.com/videos/play/wwdc2017/602/)
