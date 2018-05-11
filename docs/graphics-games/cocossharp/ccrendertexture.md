---
title: Performans ve CCRenderTexture ile görsel efektler
description: CCRenderTexture draw çağrıları azaltarak kendi CocosSharp oyunlar performansını artırmak için geliştiricilere olanak sağlar ve görsel efektler oluşturmak için kullanılabilir. Bu kılavuz, bu sınıfın etkili bir şekilde kullanmak nasıl uygulamalı örneği sağlamak için CCRenderTexture örnek eşlik.
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 82d39542b24c6b1669798a0b4e1fb14a81f6b44e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>Performans ve CCRenderTexture ile görsel efektler

_CCRenderTexture draw çağrıları azaltarak kendi CocosSharp oyunlar performansını artırmak için geliştiricilere olanak sağlar ve görsel efektler oluşturmak için kullanılabilir. Bu kılavuz, bu sınıfın etkili bir şekilde kullanmak nasıl uygulamalı örneği sağlamak için CCRenderTexture örnek eşlik._

`CCRenderTexture` Sınıfı tek bir doku CocosSharp nesnelere işlemek için işlevsellik sağlar. Bir kez oluşturulduktan sonra `CCRenderTexture` örnekleri grafiklerini verimli bir şekilde işlemek ve görsel efektler uygulamak için kullanılabilir. `CCRenderTexture` tek bir doku için bir kez işlenmek üzere birden çok nesne sağlar. Ardından, o doku olabilir çizim çağrıları toplam sayısını azaltmayı her çerçeve yeniden.

Bu kılavuz nasıl kullanılacağını inceler `CCRenderTexture` toplanabilir kartı oyun (CCG) işleme kartları performansını artırmak için nesne. Ayrıca gösterir nasıl `CCRenderTexture` tüm varlık saydam hale getirmek için kullanılabilir. Bu kılavuz başvuran `CCRenderTexture` [örnek proje](https://developer.xamarin.com/samples/mobile/CCRenderTexture/).

![](ccrendertexture-images/image1.png "Bu kılavuz CCRenderTexture örnek proje başvurur")


## <a name="card--a-typical-entity"></a>Kart – tipik varlık

Konumundaki nasıl kullanılacağını arayan önce `CCRenderTexture` nesnesi, biz ilk tanıyın kendisini ile `Card` bu proje boyunca keşfetmek için kullanacağız varlık `CCRenderTexture` sınıfı. `Card` Sınıftır özetlenen varlık desen aşağıdaki tipik bir varlık [varlık kılavuzu](~/graphics-games/cocossharp/entities.md). Kart sınıfı tüm görsel bileşenleri sahiptir (örneklerini `CCSprite` ve `CCLabel`) alanlar olarak listelenen:


```csharp
class Card : CCNode
{
    bool usesRenderTexture;
    List<CCNode> visualComponents = new List<CCNode>();
    CCSprite background;
    CCSprite colorIcon;
    CCSprite monsterSprite;
    CCLabel monsterNameDisplay;
    CCLabel hpDisplay;
    CCLabel descriptionDisplay;
```

Kart örnekleri çizilir kullanarak bir `CCRenderTexture`, ya da ayrı ayrı her visual bileşen çizim. Her bileşen bağımsız bir nesne olsa bile `CCNode` varlıklarda kullanılan sistem bölünerek yapar `Card` tek bir nesne olarak – en az çoğunlukla davranır. Örneğin, bir `Card` varlık yeniden konumlandırılır, yeniden boyutlandırılmış veya Döndürülmüş sonra tüm kapsanan görsel nesneleri tek bir nesne olarak görünen kartı hale getirmek için etkilenen. Tek bir nesne davranır kartları görmek için biz değiştirebilirsiniz `GameLayer.AddedToScene` ayarlamak için yöntemin `useRenderTextures` değişkenini `false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

`GameLayer` Kod hareket her bir görsel öğe bağımsız olarak, henüz visual her öğe içinde `Card` varlık doğru konumlandırılmış:

![](ccrendertexture-images/image1.png "GameLayer kodu her bir görsel öğe bağımsız olarak taşımaz henüz kartı varlık içindeki her görsel öğe doğru yerleştirilmiş")

Her visual bileşen kendisini işler yüklendiğinde oluşabilecek iki sorunları kullanıma sunmak için kodlanmış bir örneği:

- Birden çok çizim çağrıları nedeniyle performans olumsuz etkilenebilir
- Biz daha sonra inceleyeceksiniz gibi saydamlık gibi belirli görsel efektler doğru bir şekilde uygulanamayacağını


### <a name="card-draw-calls"></a>Kart çizim çağrıları

Bizim kodu tam olarak bulunabilir, basitleştirme *toplanabilir kartı oyun* (CCG) "Sihirli: toplama" veya "Hearthstone" gibi. Bizim oyun yalnızca aynı anda üç karttan görüntüler ve olası birimleri (mavi, yeşil ve turuncu) az sayıda sahiptir. Bunun aksine, tam oyun yirmiden kartları ekran belirli bir zamanda olabilir ve oyuncu kendi deste oluştururken seçebileceğiniz kartları yüzlerce sahip olabilir. Bizim oyun performans sorunlarından şu anda saptanmamış olsa bile, tam bir oyun benzer bir uygulama olabilir.

Çerçeve başına çizim çağrıları gösterme işleme performansı bazı fikirler gerçekleştirilen CocosSharp sağlar. Bizim `GameLayer.AddedToScene` yöntemi kümeleri `GameView.Stats.Enabled` için `true`, sonuçta elde edilen ekranın sol altında gösterilen performans bilgileri:

![](ccrendertexture-images/image2.png "GameLayer.AddedToScene yöntemi GameView.Stats.Enabled ekranın sol altında gösterilen performans bilgileri sonuçta true olarak ayarlar.")

Üç karttan ekranda sahip olmasına rağmen biz (her kart sonuçlarında altı çağrıları, performans bilgi hesapları için bir tane daha fazla görüntüleme metin çizme) on dokuz çizim çağrıları olduğunu dikkat edin. Draw çağrıları önemli bir etkisi oyunun performansına vardır, böylece bunları azaltmanın yollarını birkaç CocosSharp sağlar. Bir teknik açıklanan [CCSpriteSheet Kılavuzu](~/graphics-games/cocossharp/ccspritesheet.md). Kullanılacak başka bir tekniktir `CCRenderTexture` Biz bu kılavuzda inceleyeceğiz gibi her bir varlık için bir çağrı azaltmak için.


### <a name="card-transparency"></a>Kart saydamlık

Bizim `Card` varlık içeren bir `Opacity` aşağıdaki kod parçacığında gösterildiği gibi denetim saydam özelliği:


```csharp
public override byte Opacity
{
    get
    {
        return base.Opacity;
    }
    set
    {
        base.Opacity = value;
        if (usesRenderTexture)
        {
            this.renderTexture.Sprite.Opacity = value;
        }
        else
        {
            foreach (var component in visualComponents)
            {
                component.Opacity = value;
            }
        }
    }
}
```

Ayarlayıcı kullanılmasını destekler dokuları veya her bileşenin tek tek işleme işlemek dikkat edin. Etkisini görmek için değiştirmek `opacity` değeri `127` (kabaca yarım opaklık) içinde `GameLayer.AddedToScene` , her bileşen elde etmeyle neden olacak bir `Opacity` değerini `127`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    const byte opacity = 127;
    ...
}
```

Oyun artık bunları arka siyah olduğundan daha koyu görünmesine neden bazı saydamlık kartlarla oluşturulur:

![](ccrendertexture-images/image3.png "Oyun artık bunları arka siyah olduğundan daha koyu görünmesine neden bazı saydamlık kartlarla oluşturmaz")

İlk bakışta bizim kartları düzgün saydam olarak yapılmışsa görünebilir. Ancak, ekran visual sorunları sayısını görüntüler.

Bizim arka plan siyah olduğundan, biz bizim kartı her bölümünü nedeniyle saydamlık koyu olacak beklenir. Diğer bir deyişle, daha saydam bir kart olurken, koyu olur. Opaklık 0 konumundaki bir `Card` tamamen saydam olacak (tamamen siyah). Opaklık değiştirildi ancak, bazı bölümleri bizim kartı koyu olur kaydetmedi `127`. Daha fazla saydam geçtiğinde, kötüsü, bizim kartı bazı bölümleri gerçekten daha açık hale geldi. Hangi siyah bölümlerine bizim kartının bakalım *önce* saydam – özellikle ayrıntı metin ve canavarı grafiğin çevresinde siyah aşağıda ana hatlarıyla verilmiştir. Biz bu yan yana yerleştirirseniz, biz saydamlık uygulama etkisini görebilirsiniz:

![](ccrendertexture-images/image4.png "Yan yana girdiyseniz saydamlık uygulama etkisini görülebilir.")

Yukarıda belirtildiği gibi kart tüm parçalarını daha saydam hale zaman koyu duruma gelir, ancak alanları sayısında durum böyle değildir:

- Robot's anahat açık hale gelir (gri için siyah Giden)
- Açıklama metnini açık hale gelir (gri için siyah Giden)
- Robot yeşil parçası daha az doygun olur, ancak daha koyu hale değil

Bunun neden oluştuğunu görselleştirmenize yardımcı olmak için biz her visual bileşen çizilmiş bağımsız olarak, her kısmen saydam göz önünde bulundurmanız gerekir. Çizilen ilk visual kartın arka plan bileşendir. Sonraki saydam öğeleri üstünde kartı çizileceğini ve kart arka plan tarafından etkilenir. Bizim kartından bazı metinleri kaldırın ve Aşağı Taşı robot grafiği, kart arka plan robot nasıl etkilediğini görebiliriz. Üst kutunun turuncu satırından üzerinde robot görülebilir ve kart Center'da mavi stripe çakışıyor robot alanının koyu çizilmiş dikkat edin:

![](ccrendertexture-images/image5.png "Üst kutunun turuncu satırından üzerinde robot görülebilir ve kart Center'da mavi stripe çakışıyor robot alanının koyu çizilir dikkat edin")

Kullanarak bir `CCRenderTexture` bu kılavuzda göreceğiz gibi tüm kart saydam kart içinde bileşenleri tek tek işleme etkilemeden hale getirmemize sağlar.


## <a name="using-ccrendertexture"></a>CCRenderTexture kullanma

Her bileşenin tek tek işleme sorunları belirledik, biz işleme için açmak bir `CCRenderTexture` ve davranışı karşılaştırın.

İşlemeye etkinleştirmek için bir `CCRenderTexture`, değiştirme `userRenderTextures` değişkenini `true` içinde `GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


### <a name="card-draw-calls"></a>Kart çizim çağrıları

Biz oyun şimdi çalıştırırsanız dört on dokuz öğesinden küçültülmüş çizim çağrıları göreceğiz (her bir kart azaltılmış altı bir gelen):

![](ccrendertexture-images/image6.png "Her bir kart oyun şimdi çalıştırırsanız, dört on dokuz gelen çizim çağrıları azaltılmış altı bir öğesinden küçültülmüş")

Daha önce belirtildiği gibi bu tür azaltma önemli bir etkisi ekranında daha fazla visual varlıklarla oyunlar üzerinde olabilir.


### <a name="card-transparency"></a>Kart saydamlık

Bir kez `useRenderTextures` ayarlanır `true`, saydam kartları farklı sokacak:

![](ccrendertexture-images/image7.png "UseRenderTextures true, saydam kartlar ayarlandıktan sonra farklı oluşturmaz")

Şimdi işleme dokuları (karşılaştırması (soldan sağa doğru)) kullanarak saydam robot kart karşılaştırın:

![](ccrendertexture-images/image8.png "İşleme dokuları (soldaki) vs (sağ) olmadan kullanarak saydam robot kart Karşılaştır")

En bariz farklar ayrıntıları metin (siyah açık gri yerine) ve (açık yerine koyu ve doygunluğu azaltılmış) robot hareketli olan.


## <a name="ccrendertexture-details"></a>CCRenderTexture ayrıntıları

Kullanmanın avantajları gördük göre `CCRenderTexture`, içinde nasıl kullanıldığı bir bakalım `Card` varlık.

`CCRenderTexture` İşlemenin hedefi olabilir tuvali. Oyun ekranına kıyasla iki temel farklar vardır:

1. `CCRenderTexture` Ortası çerçeveler devam ettirir. Bunun anlamı bir `CCRenderTexture` değişiklik olduğunda yalnızca oluşturulması gerekir. Bu örnekte `Card` varlık hiçbir zaman değişikliği, böylece yalnızca bir kez işlenir. Varsa `Card` bileşenleri değişti, ardından kartı kendisine yeniden boyutlandırmaya gerek kendi `CCRenderTexture`. HP değeri (sistem durumu noktaları) saldırı zaman değiştirdiyseniz, örneğin, ardından kartı kendisini yeni HP değeri yansıtacak şekilde işlemek gerekir.
1. `CCRenderTexture` Piksel boyutları ekrana bağlı değil. A `CCRenderTexture` cihaz çözümleme daha büyük veya küçük olabilir. `Card` Kod oluşturur bir `CCRenderTexture` kendi arka plan hareketli grafik boyutunu kullanarak. Kart yönelik bir başvuru içeren bir `CCRenderTexture` adlı `renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

`renderTexture` Örnek kalır `null` kadar `UseRenderTexture` özelliği true olarak çağıran atandığında `SwitchToRenderTexture`:


```csharp
private void SwitchToRenderTexture()
{
    // The card needs to be moved to the origin (0,0) so it's rendered on the render target. 
    // After it's rendered to the CCRenderTexture, it will be moved back to its old position
    var oldPosition = this.Position;
    // Make sure visuals are part of the card so they get rendered
    bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
    if (!areVisualComponentsAlreadyAdded)
    {
        // Temporarily add them so we can render the object:
        foreach (var component in visualComponents)
        {
            this.AddChild(component);
        }
    }
    // Create the render texture if it hasn't yet been made:
    if (renderTexture == null)
    {
        // Even though the game is zoomed in to create a pixellated look, we are using
        // high-resolution textures. Therefore, we want to have our canvas be 2x as big as 
        // the background so fonts don't appear pixellated
        var pixelResolution = background.ContentSize * 2;
        var unitResolution = background.ContentSize;
        renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
        //renderTexture.Sprite.Scale = .5f;
    }
    // We don't want the render target to be a child of this when we update it:
    if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
    {
        this.Children.Remove(renderTexture.Sprite);
    }
    // Move this instance back to the origin so it is rendered inside the render texture:
    this.Position = CCPoint.Zero;
    // Clears the CCRenderTexture
    renderTexture.BeginWithClear(CCColor4B.Transparent);
    // Visit renders this object and all of its children
    this.Visit();
    // Ends the rendering, which means the CCRenderTexture's Sprite can be used
    renderTexture.End();
    // We no longer want the individual components to be drawn, so remove them:
    foreach (var component in visualComponents)
    {
        this.RemoveChild(component);
    }
    // Move this back to its original position:
    this.Position = oldPosition;
    // add the render texture sprite to this:
    renderTexture.Sprite.AnchorPoint = CCPoint.Zero;
    this.AddChild(renderTexture.Sprite);
}
```

`SwitchToRenderTexture` Doku yenilenmesi gerektiğinde yöntemi çağrılabilir. Kart zaten kullanıp kullanmadığını çağrılabilen kendi `CCRenderTexture` veya derlemesine geçme `CCRenderTexture` ilk kez.

Aşağıdaki bölümlerde keşfedin `SwitchToRenderTexture` yöntemi. 


### <a name="ccrendertexture-size"></a>CCRenderTexture boyutu

CCRenderTexture Oluşturucusu iki boyutlarının gerektirir. İlk boyutunu denetler `CCRenderTexture` zaman çizildiğinde ve ikinci piksel genişlik ve yükseklik içeriğini belirtir. `Card` Varlık başlatır, `CCRenderTexture` arka plan kullanma [ContentSize](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/). Bizim oyun sahip bir `DesignResolution` 512 tarafından gösterildiği gibi 384, `ViewController.LoadGame` iOS ve `MainActivity.LoadGame` android'de:


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

`CCRenderTexture` Oluşturucusu ile çağrılır `background.ContentSize` ilk parametre olarak gösteren `CCRenderTexture` yalnızca arka planı olarak büyüklüğünde olmalıdır `CCSprite`. Kart arka plan itibaren `CCSprite` 200 piksel uzunluğunda, kart kabaca dikey ekran yüksekliğini yarısı kaplar.

Geçirilen ikinci parametre `CCRenderTexture` Oluşturucusu belirtir piksel çözünürlüğü `CCRenderTexture`. ' Da anlatıldığı gibi [CocosSharp çözüm Kılavuzu](~/graphics-games/cocossharp/resolutions.md), genişlik ve yükseklik oyun birimleri görüntülenebilir alanının değil genellikle ekranın piksel çözünürlüğü ile aynı. Benzer şekilde, görselleri yüksek çözünürlüklü cihazlarda Canlı görünmesi için bir CCRenderTexture boyutundan daha büyük bir çözüm kullanabilirsiniz.

Piksel çözünürlük görünümlü pixelated metinden önlemek için CCRenderTexture boyutunun iki katı olur:


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

Karşılaştırılacak, biz eşleşecek şekilde pixelResolution değeri değiştirebilir `background.ContentSize` (olmadan iki katına) ve sonucu karşılaştırın: 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "Karşılaştırmak için arka plan eşleştirilecek pixelResolution değerini değiştirebilirsiniz. İki katına olmadan contentSize ve sonucu karşılaştırın")


### <a name="rendering-to-a-ccrendertexture"></a>Bir CCRenderTexture işleme

Genellikle, CocosSharp görsel nesneleri açıkça işlenmez. Görsel nesneler için bunun yerine, eklenen bir `CCLayer` parçası olan bir `CCScene`. CocosSharp otomatik olarak işler `CCScene` ve her çerçevesinde çağrılan herhangi bir işleme kod olmadan, visual hiyerarşisi. 

Bunun aksine, `CCRenderTexture` açıkça için çizilmesi gerektiğinde. Bu işleme üç adımı ayrılabilir:

1. `CCRenderTexture.BeginWithClear` olarak adlandırılan, tüm sonraki işleme arama hedeflediğini belirten `CCRenderTexture`.
1. İçinden devralma nesneleri `CCNode` (gibi `Card` varlık) için işlenen `CCRenderTexture` çağırarak `Visit`.
1. `CCRenderTexture.End` olarak adlandırılan, bu işleme belirten `CCRenderTexture` tamamlandı.

İstediğiniz sayıda nesne için işlenebilecek bir `CCRenderTexture` arasında kendi `Begin` ve `End` çağrıları. İşlemeden önce tüm gerekli görünür nesneleri alt öğesi olarak eklenir:


```csharp
bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
if (!areVisualComponentsAlreadyAdded)
{
    // Temporarily add them so we can render the object:
    foreach (var component in visualComponents)
    {
        this.AddChild(component);
    }
}
```

`renderTexture` Kaldırılmadan şekilde kartı parçası oluşturma sırasında olmamalıdır:


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

Şimdi `Card` örneği kendisine işleyebilirsiniz `CCRenderTexture` örneği:


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

Bileşenleri tek tek işleme tamamlandıktan sonra kaldırılır ve `CCRenderTexture` yeniden eklendi.


```csharp
// We no longer want the individual components to be drawn, so remove them:
foreach (var component in visualComponents)
{
    this.RemoveChild(component);
}
// add the render target sprite to this:
this.AddChild(renderTexture.Sprite);
```

## <a name="summary"></a>Özet

Bu kılavuzda ele `CCRenderTexture` sınıfı kullanarak bir `Card` collectible kartı oyunda kullanılabilecek varlık. Nasıl kullanılacağı gösterilmiştir `CCRenderTexture` çerçeve oranını artırmak ve düzgün şekilde varlık genelinde saydamlık uygulamak için sınıf.

Bu kılavuzda kullanılan rağmen bir `CCRenderTexture` bir varlığın içinde yer alan, bu sınıfın birden çok varlık işlemek için kullanılan ya da hatta tüm olabilir `CCLayer` örnekleri için ekran genelinde etkiler ve performans iyileştirmeleri.

## <a name="related-links"></a>İlgili bağlantılar

- [CCRenderTexture API Başvurusu](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [Tam proje (örnek)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
