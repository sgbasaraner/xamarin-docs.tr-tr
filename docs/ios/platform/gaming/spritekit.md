---
title: Xamarin.iOS SpriteKit
description: Bu belgede SpriteKit, SceneKit ile tümleştirilir, fizik ve animasyon içerir, aydınlatma ve gölgelendirme ve daha fazla bilgi için destek içerir Apple'nın 2B grafik framework açıklanmaktadır. SpriteKit 2B oyun oluşturmak için kullanılabilir.
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: b74b5a722aab240b55ed96bea2a33b162d7817eb
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786775"
---
# <a name="spritekit-in-xamarinios"></a>Xamarin.iOS SpriteKit

SpriteKit, 2B grafik framework Apple, iOS 8 ve OS X Yosemite ilginç bazı yeni özellikler vardır. Bunlar SceneKit, gölgelendirici desteği, aydınlatma, gölge, kısıtlamalar, normal eşlem oluşturma ve fizik geliştirmeleri ile tümleştirme içerir. Özellikle, yeni fizik özellikleri çok oyun için gerçekçi efektler eklemek kolaylaştırır.

## <a name="physics-bodies"></a>Fizik gövdeleri

SpriteKit 2B, katı gövde fizik API içerir. Bir ilişkili fizik gövdesi her hareketli grafik sahiptir (`SKPhysicsBody`), fizik dünyada yığın ve uyuşmazlık yanı sıra, gövde geometri gibi fizik özelliklerini tanımlar.

## <a name="creating-a-physics-body-from-a-texture"></a>Bir doku fizik gövde oluşturma
SpriteKit artık bir hareketli grafik fizik gövdesi, doku türetme destekler. Bu, daha doğal Ara çakışmaları uygulamak kolaylaştırır.

Örneğin, aşağıdaki çakışma nasıl neredeyse her görüntü yüzeyini Muz ve monkey birbiriyle çakışır dikkat edin:
 
![](spritekit-images/image13.png "Muz ve monkey neredeyse her görüntü yüzeyini birbiriyle çakışır")

Bu tür bir fizik gövde oluşturma SpriteKit tek satırlık bir kod ile mümkün kılar. Basit Arama `SKPhysicsBody.Create` doku ve boyutuna sahip: hareketli grafik. PhysicsBody = SKPhysicsBody.Create (hareketli grafik. Doku, hareketli grafik. Boyut);

## <a name="alpha-threshold"></a>Alfa eşiği

Yalnızca ayarı yanı sıra `PhysicsBody` doğrudan doku türetilen geometri özelliğine uygulamalar ayarlayabilirsiniz ve nasıl geometri türetilmiş denetlemek için alfa eşiği. 

Alfa eşiği elde edilen fizik gövdesinde dahil edilecek bir piksel olmalıdır minimum alfa değeri tanımlar. Örneğin, aşağıdaki kodu biraz farklı fizik gövdesinde sonuçları:

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

İle Muz yazılımlarla çakışma olduğunda monkey döner şekilde şöyle alfa eşiği uyguladıkça etkisini önceki çakışma fine-tunes:

![](spritekit-images/image14.png "İle Muz yazılımlarla çakışma olduğunda monkey döner")
 
## <a name="physics-fields"></a>Fizik alanları

Başka bir harika SpriteKit için yeni fizik alan Destek ektir. Bunlar, vortex alanları gibi şeyler eklemenize izin Radyal yer çekimi ve birkaç adlandırmak için yay alanları.

Fizik alanları gibi diğer Sahne eklenir SKFieldNode sınıfı kullanılarak oluşturulur `SKNode`. Çeşitli Fabrika yöntemler vardır `SKFieldNode` farklı fizik alanları oluşturmak için. Çağırarak yay alanı oluşturabilirsiniz `SKFieldNode.CreateSpringField()`, çağırarak Radyal yer çekimi alan `SKFieldNode.CreateRadialGravityField()`ve benzeri.

`SKFieldNode` Ayrıca alan gücü, alan bölge ve alan zorlar zayıflaması gibi alan öznitelikleri denetlemek için özellikleri vardır.

## <a name="spring-field"></a>Yay alan

Örneğin, aşağıdaki kod bir yay alanı oluşturur ve Sahne ekler:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Hareketli grafik ekleyin ve ayarlayın kendi `PhysicsBody` özellikleri böylece kullanıcı ekran dokunduğunda aşağıdaki kodu yaptığı gibi fizik alan hareketli grafik etkiler:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

Bu alan düğümü geçici yay gibi oscillate muzlar neden olur:

![](spritekit-images/image15.png "Alan düğümü geçici yay gibi muzlar oscillate")
 
## <a name="radial-gravity-field"></a>Radyal yer çekimi alan

Farklı bir alan ekleyerek benzer. Örneğin, aşağıdaki kod bir Radyal yer çekimi alan oluşturur:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

Bu, farklı zorla nerede muzlar alanın hakkında çevreye alınır bir alan sonuçlanır:

![](spritekit-images/image16.png "Muzlar çevreye alan alınır")
