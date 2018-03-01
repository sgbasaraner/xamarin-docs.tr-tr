---
title: "MonoGame oyun yüzeyi başvurusu"
description: "Oyun yüzeyi MonoGame giriş cihazları erişmek için standart, platformlar arası bir sınıftır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 14a38c07b2ae5552cd9fb67d0cec581eafbf61cb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="monogame-gamepad-reference"></a>MonoGame oyun yüzeyi başvurusu

_Oyun yüzeyi MonoGame giriş cihazları erişmek için standart, platformlar arası bir sınıftır._

`GamePad` birden çok MonoGame platformlarda giriş aygıtlardan giriş okumak için kullanılabilir. Bu kılavuz, oyun yüzeyi sınıfıyla çalışmak nasıl gösterir. Her giriş aygıtı düzeni ve sağladığı düğme sayısını farklı olduğundan, bu kılavuz çeşitli aygıt eşlemeleri Göster diyagramları içerir.


# <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Oyun yüzeyi yerini almak üzere Xbox360GamePad

Sağlanan özgün XNA API `Xbox360GamePad` PC ve Xbox 360 oyun denetleyicisinde okuma girişten için sınıf. MonoGame bu ile değiştirilen bir `GamePad` Xbox 360 denetleyicileri (örneğin, iOS veya Xbox One) çoğu MonoGame platformlarda kullanılamaz olduğundan sınıfı. Ad değişikliği, kullanımını rağmen `GamePad` sınıfı benzer `Xbox360GamePad` sınıfı.


# <a name="reading-input-from-gamepad"></a>Oyun yüzeyi girişten okuma

`GameController` Sınıfı herhangi bir MonoGame platformda okuma giriş standartlaştırılmış bir yol sağlar. Bu bilgileri iki yöntem sunar:

 - `GetState` – denetleyicinin düğmeler, analog çubukları ve d-Pad'i geçerli durumunu döndürür.
 - `GetCapabilities` – Denetleyici belirli olup düğmeleri veya titreşimi destekleyen gibi donanım özellikleri hakkında bilgi döndürür.


## <a name="example-moving-a-character"></a>Örnek: bir karakter taşıma

Aşağıdaki kodu ayarlayarak bir karakter taşımak için sol kaydırma çubuğu'nın nasıl kullanılabileceğini gösterir, `XVelocity` ve `YVelocity` özellikleri. Bu kod varsayar `characterInstance` sahip olan bir nesne örneği `XVelocity` ve `YVelocity` özellikleri:


```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```


## <a name="example-detecting-pushes"></a>Örnek: İter algılama

`GamePadState` olup belirli bir düğmeye basıldığında denetçisinin geçerli durumu hakkında bilgi sağlar. Bağlantı, bir karakter yapma gibi bazı eylemler, düğme gönderilen denetimi gerektirir (son çerçeve değil, ancak bu dilimidir) veya serbest (son çerçeve aşağı, ancak bu çerçeve aşağı yok edildi). 

Bu tür bir mantık, önceki çerçeve depolamak yerel değişkenler gerçekleştirmek için `GamePadState` ve geçerli çerçeve `GamePadState` oluşturulması gerekir. Aşağıdaki örnek önceki çerçevenin depolamak ve nasıl kullanılacağını gösterir `GamePadState` atlama uygulamak için:


```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed = 
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```


## <a name="example-checking-for-buttons"></a>Örnek: Düğmelerini denetleniyor

`GetCapabilities` bir denetleyici belirli düğmesini veya analog çubuk gibi belirli donanım olup olmadığını denetlemek için kullanılabilir. Aşağıdaki kod, her iki düğmeleri varlığını gerektiren bir oyun denetleyicisi üzerinde B ve Y düğmeleri denetlemek gösterilmektedir:


```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```


# <a name="ios"></a>iOS

iOS uygulamaları kablosuz denetleyicisinin giriş destekler.

> [!IMPORTANT]
> MonoGame 3.5 için NuGet paketlerini kablosuz oyun denetleyicileri için destek içermez. İos'ta oyun yüzeyi sınıfını kullanarak MonoGame 3.5 kaynağından oluşturma veya MonoGame 3.6 NuGet ikili dosyalarını kullanarak gerektirir. 



## <a name="ios-game-controller"></a>iOS oyun denetleyicileri

`GamePad` Sınıfı kablosuz denetleyicilerinden okuma özellikleri döndürür. Özelliklerinde `GamePad` aşağıdaki çizimde gösterildiği gibi standart iOS iyi kapsamı denetleyicisi donanım sağlayın:

![](input-images/image1.png "Oyun yüzeyi özelliklerinde iyi kapsamı standart iOS için denetleyici donanımı, bu diyagramda gösterildiği gibi sağlayın")


# <a name="apple-tv"></a>Apple TV

Apple TV oyunlar Siri uzaktan ya da kablosuz oyun denetleyicileri için giriş kullanabilirsiniz.


## <a name="siri-remote"></a>Siri uzaktan

*Siri uzaktan* yerel giriş Apple TV için aygıttır. Siri uzaktan değerlerinden aracılığıyla olaylarını okunabilmesine rağmen (gösterildiği gibi [Siri uzaktan ve Bluetooth denetleyicileri Kılavuzu](~/ios/tvos/platform/remote-bluetooth.md)), `GamePad` sınıfı Siri uzak değerleri dönebilirsiniz.

Dikkat `GamePad` yalnızca play düğmesinden giriş okuyabilir ve yüzeyini touch: 

![](input-images/image2.png "Oyun yüzeyi yalnızca play düğmesinden giriş okuyabilir ve yüzeyini touch olduğunu fark")

Bu yana dokunmatik yüzey taşıma okuyun `DPad` özelliği, hareket değerleri kullanarak raporlanır `ButtonState` sınıfı. Diğer bir deyişle, değerler yalnızca olarak kullanılabilir `ButtonState.Pressed` veya `ButtonState.Released`, sayısal değerleri veya hareketleri aksine.


## <a name="apple-tv-game-controller"></a>Apple TV oyun denetleyicileri

Apple TV için oyun denetleyicileri oyun denetleyicileri iOS uygulamaları için aynı şekilde davranır. Daha fazla bilgi için bkz: [iOS oyun denetleyicileri bölüm](#iOS_Game_Controller). 


# <a name="xbox-one"></a>Xbox One

Xbox tek bir konsoldan okuma giriş Xbox bir oyun denetleyicisinden destekler.


## <a name="xbox-one-game-controller"></a>Xbox bir oyun denetleyicisi

Xbox bir oyun denetleyicisi Xbox One ' için en yaygın giriş aygıttır. `GamePad` Sınıfı denetleyicisinin donanımdan giriş değerlerini sağlar.

![](input-images/image3.png "Oyun denetleyicisi donanımdan giriş değerleri oyun yüzeyi sınıfı sağlar")


# <a name="summary"></a>Özet

Bu kılavuz MonoGame'nın genel bir bakış sağlanan `GamePad` sınıfı, giriş okuma mantığı ve diyagramları ortak uygulanacak nasıl `GamePad` uygulamaları.

## <a name="related-links"></a>İlgili bağlantılar

- [MonoGame GamePad](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
