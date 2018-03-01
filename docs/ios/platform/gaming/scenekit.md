---
title: SceneKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 785faab84c4f3c5176750f4d86eeaeae3bb9332d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="scenekit"></a>SceneKit

Sahne, 3B Sahne grafik 3B grafik çalışmak basitleştirir API'si setidir. OS X 10.8 ilk sunulmuştur ve iOS 8 şimdi geldi. Sahne Seti'nde derinlikli 3B Görselleştirme ve sıradan 3B oyunlar oluşturma OpenGL uzmanlık gerektirmez. Ortak Sahne grafik kavramlar oluşturma, Sahne Seti hemen OpenGL ve çok 3B eklemek bir uygulama için içerik kolaylaşır OpenGL ES karmaşıklığını soyutlar. Ancak, OpenGL uzmanı varsa, Sahne Seti doğrudan OpenGL ile de bağlamadan harika desteğe sahiptir. Ayrıca fizik gibi 3B grafik tamamlayan çeşitli özellikler içerir ve çok iyi çekirdek animasyon, çekirdek görüntü ve hareketli grafik Seti gibi diğer Apple çerçeveleri ile tümleşir.

Sahne Seti çalışmak son derece kolaydır. Mvc'deki çizmeye bildirim temelli bir API'dir. Yalnızca Sahne ayarlayın, özellikleri ve Sahne Seti tanıtıcıları için Sahne işleme ekleyin.

Sahne Kit ile çalışmak için Sahne grafiğini kullanarak oluşturduğunuz `SCNScene` sınıfı. Bir hiyerarşi düğümlerinin örneği tarafından temsil edilen bir Sahne içeren `SCNNode`, 3B alanda konumları tanımlama. Her düğüm, aşağıdaki şekilde gösterildiği gibi geometri, aydınlatma ve hizmetin görünümünü etkileyen malzemeleri gibi özelliklere sahiptir:

![](scenekit-images/image7.png "SceneKit hiyerarşisi") 

## <a name="create-a-scene"></a>Bir görünüm oluşturma

Ekranda görüntülenen Sahne yapmak için ekleyebilmek için bir `SCNView` görünümün Sahne özelliğine atayarak. Ayrıca, Sahne için herhangi bir değişiklik yaparsanız `SCNView` değişiklikleri görüntülemek için kendisini güncelleştirecektir.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

Planda modelleme aracı 3B dışarı aktarılan dosyaları veya program aracılığıyla geometrik temelleri doldurulabilir. Örneğin, bu bir küre oluşturma ve Sahne ekleyin.

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Açık ekleme

Bu noktada Sahne hiçbir açık olduğundan küre herhangi bir şey görüntülenmez. Ekleme `SCNLight` düğümleri örneklerine Sahne Seti'nde ışık oluşturur. Tek yönlü ışık çeşitli formlardan ortam aydınlatma arasında değişen ışık çeşitli türleri vardır. Örneğin aşağıdaki kod bir mikrofonsa ışık küre yan tarafında oluşturur:

```csharp
// omnidirectional light
var light = SCNLight.Create ();
var lightNode = SCNNode.Create ();
light.LightType = SCNLightType.Omni;
light.Color = UIColor.Blue;
lightNode.Light = light;
lightNode.Position = new SCNVector3 (-40, 40, 60);
scene.RootNode.AddChildNode (lightNode);
```

Mikrofonsa aydınlatma işaret feneri ışığı gibi bile aydınlatma tür kaynaklanan dağıtma yansımasını oluşturur. Tüm yönlerde eşit inanılmaz gibi hiçbir yönüne sahip olsa da ortam ışığı oluşturma, benzer. Bunu :) ışık gibi ruh düşünün

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

Yerinde ışık ile küre şimdi Sahne görünür olur.

![](scenekit-images/image8.png "Küre Sahne aydınlatma zaman görülebilir")
 
## <a name="adding-a-camera"></a>Kamera ekleme

Kamera (SCNCamera) Sahne ekleme bakış açısından değiştirir. Kamera eklemek için desen benzer. Kamera oluşturun, bir düğüm ekleme ve Sahne düğüm ekleyin.

```csharp
// camera
camera = new SCNCamera {
    XFov = 80,
    YFov = 80
};
cameraNode = new SCNNode {
    Camera = camera,
    Position = new SCNVector3 (0, 0, 40)
};
scene.RootNode.AddChildNode (cameraNode);
```

Yukarıdaki kod, Sahne oluşturucuları kullanarak nesneleri oluşturulabilir Seti veya Create fabrika yöntemi görebileceğiniz gibi. C# Başlatıcısı sözdizimi kullanılarak eski izin verir, ancak hangisinin kullanılacağını sağlasa da, büyük ölçüde tercih konusudur.

Yerinde kamera ile tüm küre kullanıcıya görünür:

![](scenekit-images/image9.png "Tüm küre kullanıcıya görünür")
 
Görünüm için ek ışık ekleyebilirsiniz. İşte bu şekilde daha fazla birkaç mikrofonsa ışık görünür:

![](scenekit-images/image10.png "Daha fazla birkaç mikrofonsa ışık ile küre")
 
Ayarlayarak ek olarak, `sceneView.AllowsCameraControl = true`, kullanıcının bakış açısı dokunma hareketi ile değiştirebilirsiniz.

### <a name="materials"></a>Malzemeler

Malzemeleri SCNMaterial sınıfı ile oluşturulur. Örneğin, bir görüntü kürenin yüzeyine eklemek, resmi malzemenin ayarlama *Dağıt* içeriği.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

Aşağıda gösterildiği gibi bu düğüm yansımayı katmanlar:

![](scenekit-images/image11.png "Küre yansımayı katmanlama")
 
Bir malzeme çok ışık diğer türleri için yanıt verecek şekilde ayarlayabilirsiniz. Örneğin, nesne parlak yapılabilir ve aynasal içeriğinin yüzey üzerinde açık bir nokta aşağıda gösterildiği gibi sonuçta aynasal yansıma görüntülemek için ayarladığınız:

![](scenekit-images/image12.png "Yüzey üzerinde açık bir nokta sonuçta aynasal yansıma ile parlak yapılan nesnesi")
 
Çok çok az kod ile elde etmenizi sağlayan çok esnek malzemelerin. Örneğin, yerine dağıtma içeriği görüntüye ayarı, yansıtıcı içeriği için bunun yerine.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

Şimdi monkey görsel olarak küre içinde bakış açısını bağımsız sit görünüyor.

### <a name="animation"></a>Animasyon

Sahne Seti iyi animasyon ile çalışmak üzere tasarlanmıştır. Örtük veya açık animasyonları oluşturabilirsiniz ve çekirdek animasyon katman ağacından Sahne bile hale getirebilir. Örtük bir animasyon oluştururken, kendi geçiş sınıfı Sahne Seti sağlar `SCNTransaction`.

Küre döndüren örnek aşağıda verilmiştir:

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

Çok daha fazlasını döndürme ancak animasyon ekleyebilirsiniz. Birçok Sahne Seti canlandırılabilir özellikleridir. Örneğin, aşağıdaki kodu malzemenin canlandırır `Shininess` aynasal yansıma artırmak için.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

Sahne Seti kullanmak oldukça basittir. Bol miktarda kısıtlamaları, fizik, bildirim temelli Eylemler, 3B metin, alan desteği, hareketli grafik Seti tümleştirme ve birkaç adlandırmak için çekirdek görüntü tümleştirme derinliği de dahil olmak üzere ek özellikler sunar.