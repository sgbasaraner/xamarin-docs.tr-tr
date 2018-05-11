---
title: Fruity döner oyun ayrıntıları
description: Bu kılavuz, ortak CocosSharp ve fizik, içerik yönetimi, oyun durumu ve oyun tasarım gibi oyun geliştirme kavramları kapsayan Fruity döner oyun gözden geçirir.
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 19b57d41fdf8aa4f8a749cf4979755161a0e7e51
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="fruity-falls-game-details"></a>Fruity döner oyun ayrıntıları

_Bu kılavuz, ortak CocosSharp ve fizik, içerik yönetimi, oyun durumu ve oyun tasarım gibi oyun geliştirme kavramları kapsayan Fruity döner oyun gözden geçirir._

Fruity düştüğünde ve player kırmızı ve sarı ulaşılacak durumlardır renkli demet noktaları kazanmak için sıralar basit, fizik tabanlı oyun ' dir. Oyun amacı olabildiğince çok puan mümkün olduğunca ulaşılacak durumlardır açılan yanlış Kutusu'na oyun bitiş izin vererek olmadan almaktır.

![](fruity-falls-images/image1.png "Oyun amacı olabildiğince çok puan mümkün olduğunca ulaşılacak durumlardır açılan yanlış Kutusu'na oyun bitiş izin vererek olmadan almaktır")

Fruity döner genişletir içinde tanıtılan kavramları [BouncingGame Kılavuzu](~/graphics-games/cocossharp/bouncing-game.md) aşağıdakileri ekleyerek:

 - PNG'ler formunda içerik
 - Gelişmiş fizik
 - Oyun durumu (planda arasında geçiş)
 - Tek bir sınıf içinde yer alan değişkenleri aracılığıyla oyun katsayısını ayarlar yapma yeteneği
 - Yerleşik visual hata ayıklama desteği
 - Oyun varlıklar kullanan kodu kuruluş
 - Eğlenceli ve yeniden yürütme değeri odaklanmış oyun tasarım

Sırada [BouncingGame Kılavuzu](~/graphics-games/cocossharp/bouncing-game.md) CocosSharp kavramları Tanıtımı odaklanmış, Fruity döner tamamlanmış bir oyun üründe hepsini bir araya getirmek gösterilmektedir. Bu kılavuz BouncingGame başvurduğundan okuyucular ilk kendilerini kazanmalısınız [BouncingGame Kılavuzu](~/graphics-games/cocossharp/bouncing-game.md) bu kılavuzu okuduktan önce.

Bu kılavuz, kendi oyun yapmanıza yardımcı olacak bilgiler Fruity döner tasarımını ve uygulama kapsar. Aşağıdaki konuları içerir:


- [Oyun denetleyicisi sınıfı](#gamecontroller-class)
- [Oyun varlıklar](#game-entities)
- [Ulaşılacak durumlardır grafik](#fruit-graphics)
- [Fizik](#physics)
- [Oyun içerik](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>Oyun denetleyicisi sınıfı

Fruity döner PCL proje içeren bir `GameController` oyun başlatmasını ve görüntülerin arasında taşıma sorumlu olan sınıf. Bu sınıf, yinelenen kod ortadan kaldırmak için iOS ve Android projeler tarafından kullanılır.

İçinde yer alan kodu `Initialize` kodunda yöntemi benzer`Initialize` değiştirilmemiş bir CocosSharp şablonu, ancak yönteminde değişiklikler sayısını içeriyor.

Varsayılan olarak, `GameView.ContentManager.SearchPaths` cihazın çözünürlüğe bağlıdır. Aşağıda ayrıntılı olarak açıklandığı gibi Fruity döner aygıt çözünürlüğü bağımsız olarak aynı içeriği kullanır. böylece `Initialize` yöntemi ekler `Images` (alt klasörsüz ile) yoluna `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

Yeni CocosSharp şablonları içerir içinden devralma bir sınıf `CCLayer`, olduğunu belirtmek için bu sınıf oyun görselleri ve mantığı eklenmelidir. Bunun yerine, birden çok Fruity döner kullanır `CCLayer` denetim örneklerine sipariş çizin. Bunlar `CCLayer` örnekleri içinde bulunan `GameView` devraldığı sınıfı `CCScene`. Fruity düştüğünde de ilk olan birden çok Sahne içerir `TitleScene`. Bu nedenle, `Initialize` yöntemi başlatır bir `TitleScene` geçirilir örneğinin `RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

İçerik Fruity eşiğin estetik nedenlerle düşük çözünürlüklü piksel resmi olarak oluşturuldu. `GameView.DesignResolution` Oyun yalnızca geniş 384 birimleri ve 512 uzun olduğu şekilde ayarlanır:


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

Son olarak, `GameController` sınıfı tarafından çağrılan bir statik yöntem sağlar `CCGameScene` başka bir geçiş `CCScene`. Bu yöntem arasında hareket etmek için kullanılan `TitleScene` ve `GameScene`.


## <a name="game-entities"></a>Oyun varlıklar

Fruity döner varlık desenini oyun nesnelerin çoğu için kullanır. Bu desen hakkında ayrıntılı bir açıklama bulunabilir [CocosSharp varlıklarda Kılavuzu](~/graphics-games/cocossharp/entities.md).

Varlık uygulama oyun nesneleri varlıklar klasöründe bulunabilir **FruityFalls.Common** proje:

![](fruity-falls-images/image2.png "FruityFalls.Common projesindeki varlıkları klasörü")

Varlıklardır devralınan nesneleri `CCNode`ve görselleri, çakışma ve çerçeve her davranışa sahip olabilir.

`Fruit` Nesnesi, tipik bir CocosSharp varlığı temsil eder: görsel nesne, çakışma ve çerçeve her mantığı içerir. Kurucusu ulaşılacak durumlardır başlatmak için sorumludur:


```csharp
public Fruit ()
{
    CreateFruitGraphic ();
    if (GameCoefficients.ShowCollisionAreas)
    {
        CreateDebugGraphic ();
    }
    CreateCollision ();
    CreateExtraPointsLabel ();
    Acceleration.Y = GameCoefficients.FruitGravity;
}
```


### <a name="fruit-graphics"></a>Ulaşılacak durumlardır grafik

`CreateFruitGraphic` Yöntemi oluşturur bir `CCSprite` örneği ve ona ekler `Fruit`. `IsAntialiased` Özelliği false olarak ayarlanmış bir pixelated görünüm oyun vermek için. Bu değer, tüm false olarak ayarlayın `CCSprite` ve `CCLabel` oyun boyunca örnekleri:


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

Her player dokunur bir `Fruit` örneği `Paddle`, o ulaşılacak durumlardır noktası değerini birer birer artırır. Bu, ek bir sınama ulaşılacak durumlardır fazladan puan juggle için deneyimli oynatıcı sağlar. Ulaşılacak durumlardır noktası değerini kullanarak görüntülenen `extraPointsLabel` örneği.

`CreateExtraPointsLabel` Yöntemi oluşturur bir `CCLabel` örneği ve ona ekler `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Fruity döner ulaşılacak durumlardır – cherries ve lemons iki farklı türde içerir. `CreateFruitGraphic` Aracılığıyla varsayılan visual ancak ulaşılacak durumlardır görselleri değiştirilebilir atar `FruitColor` sırayla çağırır özelliği `UpdateGraphics`:


```csharp
private void UpdateGraphics()
{
if (GameCoefficients.ShowCollisionAreas)
    {
        debugGrahic.Clear ();
        const float borderWidth = 4;
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius,
            CCColor4B.Black);
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius - borderWidth,
            fruitColor.ToCCColor ());
    }
    if (this.FruitColor == FruitColor.Yellow)
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("lemon.png");
        extraPointsLabel.Color = CCColor3B.Black;
        extraPointsLabel.PositionY = 0;
    }
    else
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("cherry.png");
        extraPointsLabel.Color = CCColor3B.White;
        extraPointsLabel.PositionY = -8;
    }
}
```

İlk if deyimi içinde `UpdateGraphics` çakışma alanları görselleştirmek için kullanılan hata ayıklama grafik güncelleştirir. Oyun son sürümünü yapılır, ancak fizik hata ayıklama için geliştirme sırasında tutulabilir önce bu görselleri devre dışı bırakılmıştır. İkinci bölümü değişiklikleri `graphic.Texture` çağırarak özelliği `CCTextureCache.SharedTextureCache.AddImage`. Bu yöntem, dosya adına göre bir doku erişim sağlar. Bu yöntem hakkında daha fazla bilgi bulunabilir [doku önbelleğe alma Kılavuzu](~/graphics-games/cocossharp/texture-cache.md).

`extraPointsLabel` Renk ulaşılacak durumlardır görüntü karşıtlığını tutmak için ayarlandı ve kendi `PositionY` değer olarak ayarlandı Merkezi'ne `CCLabel` ulaşılacak durumlardır'ın üzerinde `CCSprite`:

![](fruity-falls-images/image3.png "ExtraPointsLabel renk ulaşılacak durumlardır görüntü karşıtlığını tutmak için ayarlanır ve PositionY değerini CCSprite fruits üzerinde CCLabel merkezi için ayarlanır")

![](fruity-falls-images/image4.png "ExtraPointsLabel renk ulaşılacak durumlardır görüntü karşıtlığını tutmak için ayarlanır ve PositionY değerini CCSprite fruits üzerinde CCLabel merkezi için ayarlanır")


### <a name="collision"></a>Çakışma

Fruity döner geometri klasöründe nesneleri kullanarak bir özel çakışma çözümü uygular:

![](fruity-falls-images/image5.png "Geometri klasöründe nesneleri kullanarak bir özel çakışma çözümü Fruity döner uygular")

Fruity döner çakışma kullanarak gerçekleştirilir `Circle` veya `Polygon` genellikle bir varlığın bir alt öğesi olarak eklenmekte olan iki şu türlerden birine sahip bir nesne. Örneğin, `Fruit` nesnesi bir `Circle` adlı `Collision` hangi içinde başlatır, `CreateCollision` yöntemi:


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

Unutmayın `Circle` sınıfının devraldığı `CCNode` sınıfı bir alt öğesi olarak bizim oyunun varlıklara eklenebilmesi için:


```csharp
public class Circle : CCNode
{
    ...
}
```

`Polygon` oluşturma benzer `Circle` gösterildiği gibi oluşturma `Paddle` sınıfı:


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

Çakışma mantığı kapsanan [daha sonra bu kılavuzdaki](#collision).


## <a name="physics"></a>Fizik

Fruity düştüğünde de fizik iki kategoriye ayrılabilir: taşıma ve çakışma. 


### <a name="movement-using-velocity-and-acceleration"></a>Hız ve Hızlandırma kullanarak taşıma

Fruity döner kullanan `Velocity` ve `Acceleration` varlıklarını, benzer şekilde hareket denetlemek için değerler [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md). Varlıkları adlı bir yöntemi, taşıma mantığı uygulamak `Activity`, adlandırılan bir kez çerçeve başına. Örneğin, hareket uyarlamasını görebiliriz `Fruit` sınıfı `Activity` yöntemi:

```csharp
public void Activity(float frameTimeInSeconds)
{
    timeUntilExtraPointsCanBeAdded -= frameTimeInSeconds;
    
    // linear approximation:
    this.Velocity += Acceleration * frameTimeInSeconds;

    // This is a linear approximation to drag. It's used to
    // keep the object from falling too fast, and eventually
    // to slow its horizontal movement. This makes the game easier
    this.Velocity -= Velocity * GameCoefficients.FruitDrag * frameTimeInSeconds;
    
    this.Position += Velocity * frameTimeInSeconds;
}
```
`Fruit` Nesnesidir Sürükle kendi uygulamasında benzersiz – ne kadar hızlı ulaşılacak durumlardır göre hız yavaşlatır bir değer taşıma. Bu sürükleme uygulamasını sağlar bir *terminal hız*, ulaşılacak durumlardır örneği dönebilir en yüksek hızı olduğu. Sürükleme de oyun yürütmek biraz daha kolay anlaşılır ulaşılacak durumlardır yatay hareket yavaşlatır.

`Paddle` Nesne için de geçerlidir `Velocity` içinde kendi `Activity` yöntemi, ancak kendi `Velocity` kullanılarak hesaplanan bir `desiredLocation` değeri:


```csharp
public void Activity(float frameTimeInSeconds)
{
    // This code will cause the cursor to lag behind the touch point
    // Increasing this value reduces how far the paddle lags
    // behind the player’s finger. 
    const float velocityCoefficient = 4;
    // Get the velocity from current location and touch location
    Velocity = (desiredLocation - this.Position) * velocityCoefficient;
    this.Position += Velocity * frameTimeInSeconds;
    ...
}
```

Genellikle, kullanan oyunlar `Velocity` doğrudan kendi nesneleri konumunu denetlemek için en az değil başlatma varlık konumlarını belirlemek. Doğrudan ayarı yerine `Paddle` konumu `Paddle.HandleInput` yöntemi atar `desiredLocation` değeri:

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>Çakışma

Fruity döner uygulayan ulaşılacak durumlardır ve diğer collidable nesneler arasında yarı gerçekçi çakışma gibi `Paddle` ve `GameScene.Splitter`. Çakışma hata ayıklama yardımcı olmak için Fruity döner çakışma alanları değiştirerek görünür hale getirilebilir `GameCoefficients.ShowDebugInfo` içinde `GameCoefficients.cs` dosyası:


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

Bu değeri ayarlamak `true` çakışma alanları işlenmesini sağlar:  

![](fruity-falls-images/image6.png "Çakışma alanları çizmeye bu değeri true olarak ayarlanmasını sağlar")

Çakışma mantığı başlar `GameScene.Activity` yöntemi:


```csharp
private void Activity(float frameTimeInSeconds)
{
    if (hasGameEnded == false)
    {
        paddle.Activity(frameTimeInSeconds);
        foreach (var fruit in fruitList)
        {
            fruit.Activity(frameTimeInSeconds);
        }
        spawner.Activity(frameTimeInSeconds);
        DebugActivity();
        PerformCollision();
    }
}
```

`PerformCollision` tüm yazılımlarla çakışma işleme `Fruit` diğer nesnelerle örnekleri: 


```csharp
private void PerformCollision()
{
    // reverse for loop since fruit may be destroyed:
    for(int i = fruitList.Count - 1; i > -1; i--)
    {
        var fruit = fruitList[i];
        FruitVsPaddle(fruit);
        FruitPolygonCollision(fruit, splitter.Polygon, CCPoint.Zero);
        FruitVsBorders(fruit);
        FruitVsBins(fruit);
    }
}
```

#### <a name="fruitvsborders"></a>FruitVsBorders

`FruitVsBorders` çakışma, farklı bir sınıf içinde yer alan mantığı güvenmek yerine çakışma için kendi mantığı gerçekleştirir. Bu farkı ulaşılacak durumlardır ve ekranının Kenarlıklar arasında çakışma mükemmel düz değil – ekran kenarından dikkatli raket taşıma tarafından edilmesini ulaşılacak durumlardır olasıdır çünkü bulunmaktadır. Ulaşılacak durumlardır zaman ile raket isabet ekran dışına Sıçrama, ancak player yavaş ulaşılacak durumlardır iter, kenar geçmiş ve ekranın taşır:


```csharp
private void FruitVsBorders(Fruit fruit)
{
    if(fruit.PositionX - fruit.Radius < 0 && fruit.Velocity.X < 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionX + fruit.Radius > gameplayLayer.ContentSize.Width && fruit.Velocity.X > 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionY + fruit.Radius > gameplayLayer.ContentSize.Height && fruit.Velocity.Y > 0)
    {
        fruit.Velocity.Y *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
}
```

#### <a name="fruitvsbins"></a>FruitVsBins

`FruitVsBins` Yöntemdir herhangi ulaşılacak durumlardır iki depo birine düştüğü olmadığını denetlemek için sorumlu. Bu durumda, (eşleşme ulaşılacak durumlardır/bin renkleri varsa) player puanlar veya (renkleri eşleşmiyorsa) oyun sona erer:


```csharp
private void FruitVsBins(Fruit fruit)
{
    foreach(var bin in fruitBins)
    {
        if(bin.Polygon.CollideAgainst(fruit.Collision))
        {
            if(bin.FruitColor == fruit.FruitColor)
            {
                // award points:
                score += 1 + fruit.ExtraPointValue;
                scoreText.Score = score;
                Destroy(fruit);
            }
            else
            {
                EndGame();
            }
            break;
        }
    }
}
```

#### <a name="fruitvspaddle-and-fruitpolygoncollision"></a>FruitVsPaddle ve FruitPolygonCollision

Ulaşılacak durumlardır raket ve ulaşılacak durumlardır Bölümlendirici (iki depo ayırarak alan) karşılaştırması ve her ikisi de Bel çakışma `FruitPolygonCollision` yöntemi. Bu yöntem üç bölümden oluşur:

1. Nesneleri birbiriyle çakışır alıp almadığını Sına
1. Böylece artık üst üste (yalnızca ulaşılacak durumlardır Fruity döner içinde) nesnelerini taşıma
1. Aşağıdaki kod, yukarıda gerçekleştirdiğiniz noktalarını gösterir. bir Sıçrama benzetimini yapmak için (yalnızca ulaşılacak durumlardır Fruity döner içinde) nesnelerin hız ayarlayın:


```csharp
private static bool FruitPolygonCollision(Fruit fruit, Polygon polygon, CCPoint polygonVelocity)
{
    // Test whether the fruit collides
    bool didCollide = polygon.CollideAgainst(fruit.Collision);
    if (didCollide)
    {
        var circle = fruit.Collision;
        // Get the separation vector to reposition the fruit so it doesn't overlap the polygon
        var separation = CollisionResponse.GetSeparationVector(circle, polygon);
        fruit.Position += separation;
        // Adjust the fruit's Velocity to make it bounce:
        var normal = separation;
        normal.Normalize();
        fruit.Velocity = CollisionResponse.ApplyBounce(
            fruit.Velocity, 
            polygonVelocity, 
            normal, 
            GameCoefficients.FruitCollisionElasticity);
    }
    return didCollide;
}
```

Fruity döner çakışma yanıt taraflı – yalnızca ulaşılacak durumlardır'ın hız ve konuma göre düzeltilir. Bu, diğer oyunlar hem bir boulder veya kilitlenen iki araba diğer Ftp'den karakteri gibi ilgili nesneleri etkiler çakışma olabilir dikkate değerdir.

Matematik içinde yer alan çakışma mantığının arkasındaki `Polygon` ve `CollisionResponse` sınıfları olduğundan bu kılavuzun kapsamı dışındadır, ancak olarak yazılmış, bu yöntemleri türlerde oyunlar için kullanılabilir. Çokgen ve `CollisionResponse` sınıfları bu kodu türlerde oyunlar için kullanılabilmesi için dikdörtgen olmayan ve dışbükey çokgenler bile destekler.

 


## <a name="game-content"></a>Oyun içerik

Fruity düştüğünde resmi hemen BouncingGame oyun ayırır. Oyun tasarımları benzer olmakla birlikte, oynatıcıları hemen nasıl iki oyunlar konum bir fark görmez. Oyun severler genellikle kendi görselleri tarafından oyun denemeye karar verin. Bu nedenle, geliştiricilerin görsel olarak çekici alımında kaynak yatırımları yaparlar oldukça önemlidir oyun.

Resim Fruity eşiğin aşağıdaki hedefleri ile oluşturuldu:

 - Tutarlı tema
 - Tek bir karakter – Xamarin monkey indirimlere
 - Bir gevşetme keyifli oluşturmak için açık renkleri deneyimi
 - Ön plan ve arka plan arasında oyunlar nesneleri görsel olarak izlemek kolay şekilde Karşıtlık
 - Kaynak ağır animasyonları olmadan basit görsel efektler oluşturma olanağı


### <a name="content-location"></a>İçeriğin konumu

Fruity döner Android projesi görüntüleri klasöründeki tüm içeriği içerir:

![](fruity-falls-images/image7.png "Fruity döner tüm içeriği Android projesi görüntüleri klasöründe içerir")

Bu aynı dosyalar yinelemesinden kaçınmak için iOS projesi bağlanır ve her iki proje bunu dosyalardaki değişiklikler etkileyebilir:

![](fruity-falls-images/image8.png "Bu aynı dosyalar yinelemesinden kaçınmak için iOS projesi bağlanır ve her iki proje bunu dosyalardaki değişiklikler etkileyebilir")

İçerik içinde yer almıyor dikkate değerdir **Ld** veya **Hd** varsayılan CocosSharp şablonunun parçası olan klasörler. **Ld** ve **Hd** klasörleri içerik – bir telefon ve tabletler gibi daha yüksek çözünürlükte aygıtları için bir gibi düşük çözünürlüklü cihazlar için iki kümesi sağlayan oyunlar için kullanılacak yöneliktir. Farklı ekran boyutları için içerik sağlamak gerekmez şekilde Fruity döner resim bilerek pixelated ile estetik, oluşturulur. Bu nedenle, **Ld** ve **Hd** klasörleri projeden tamamen da kaldırılmıştır.


### <a name="gamescene-layering"></a>GameScene katmanlama

Bu kılavuzda daha önce belirtildiği gibi `GameScene` tüm oyun nesne örneğini oluşturmada, konumlandırma ve nesneler arası mantığı (örneğin, çakışma) sorumludur. Tüm nesneler, dört birine eklenir `CCLayer` örnekleri:


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

Bu dört katmanı oluşturulan `CreateLayers` yöntemi. Eklendikten Not `GameScene` arkadan öne sırasına göre:


```csharp
private void CreateLayers()
{
    backgroundLayer = new CCLayer();
    this.AddLayer(backgroundLayer);
    gameplayLayer = new CCLayer();
    this.AddLayer(gameplayLayer);
    foregroundLayer = new CCLayer();
    this.AddLayer(foregroundLayer);
    hudLayer = new CCLayer();
    this.AddLayer(hudLayer);
}
```

Katmanları oluşturulduktan sonra tüm görsel nesneleri kullanarak Katmanlar eklenen `AddChild` gösterildiği gibi yöntemi `CreateBackground` yöntemi:


```csharp
private void CreateBackground()
{
    var background = new CCSprite("background.png");
    background.AnchorPoint = new CCPoint(0, 0);
    background.IsAntialiased = false;
    backgroundLayer.AddChild(background);
}
```


### <a name="vine-entity"></a>Asma varlık

`Vine` Varlık içerik için benzersiz olarak kullanılan – oyunlar üzerinde hiçbir etkisi olmaz. Yirmi oluşur `CCSprite` örnekler, deneme yanılma asma her zaman ekranın üstünde ulaştığında emin olmak için seçilen bir sayı:


```csharp
public Vine ()
{
    const int numberOfVinesToAdd = 20;
    for (int i = 0; i < numberOfVinesToAdd; i++)
    {
        var graphic = new CCSprite ("vine.png");
        graphic.AnchorPoint = new CCPoint(.5f, 0);
        graphic.PositionY = i * graphic.ContentSize.Height;
        this.AddChild (graphic);
    }
}
```

İlk `CCSprite` alt kenarı asma konumunu eşleşecek şekilde yerleştirilir. Bu AnchorPoint ayarlayarak gerçekleştirilir `new CCPoint(.5f, 0)`. `AnchorPoint` Özelliğini kullanan *koordinatları normalleştirilmiş* hangi aralık 0 ile 1 boyutuna bakılmaksızın arasında `CCSprite`:

![](fruity-falls-images/image9.png "AnchorPoint özelliği normalleştirilmiş koordinatları hangi aralık 0 ile 1 CCSprite boyutuna bakılmaksızın arasında kullanır.")

Sonraki asma hareketli grafik kullanarak konumlandırıldığı `graphic.ContentSize.Height` resmin yüksekliğini piksel cinsinden döndürür değeri. Aşağıdaki diyagramda, nasıl asma grafiğin yukarı bölmeleri gösterir:

![](fruity-falls-images/image10.png "Bu diyagramda nasıl asma grafiğin yukarı bölmeleri gösterir")

Kaynağı bu yana `Vine` varlıktır asma alt kısmında, sonra da konumlandırma de gösterildiği gibi basit `Paddle.CreateVines` yöntemi:


```csharp
private void CreateVines()
{
    // Increasing this value moves the vines closer to the 
    // center of the paddle.
    const int vineDistanceFromEdge = 4;
    leftVine = new Vine ();
    var leftEdge = -width / 2.0f;
    leftVine.PositionX = leftEdge + vineDistanceFromEdge;
    this.AddChild (leftVine);
    rightVine = new Vine ();
    var rightEdge = width / 2.0f;
    rightVine.PositionX = rightEdge - vineDistanceFromEdge;
    this.AddChild (rightVine);
}
```

Asma örnekleri `Paddle` varlık ilginç döndürme davranışı de vardır. Bu yana `Paddle` bir bütün olarak varlık döndürür (Bu kılavuzun aşağıdaki daha ayrıntılı olarak ele alınmaktadır) player girişini göre `Vine` örnekleri aynı zamanda bu döndürme etkilenebilir. Ancak, döndürme `Vine` ile birlikte örnekleri `Paddle` aşağıdaki animasyonda gösterildiği gibi istenmeyen görsel bir efekt üretir:

![](fruity-falls-images/image11.gif "Ancak, raket birlikte asma örnekleri döndürme istenmeyen görsel bir efekt aşağıdaki animasyonda gösterildiği gibi üretir")

Counteract için `Paddle` döndürme, `leftVine` ve `rightVine` örnekleri Döndürülmüş gösterildiği gibi raket ters yönünü `Paddle.Activity` yöntemi:


```csharp
public void Activity(float frameTimeInSeconds)
{
    ...
    // This value adds a slight amount of rotation
    // to the vine. Having a small amount of tilt looks nice.
    float vineAngle = this.Velocity.X / 100.0f;
    leftVine.Rotation = -this.Rotation + vineAngle;
    rightVine.Rotation = -this.Rotation + vineAngle;
}
```

Döndürme az miktarda geri asma eklendiğini fark `vineAngle` katsayısı. Bu değer, ne kadar vines döndürme ayarlamak için değiştirilebilir.


## <a name="gamecoefficients"></a>GameCoefficients

Adlı bir sınıf Fruity döner içerecek şekilde her iyi oyun yineleme, ürünüdür `GameCoefficients` denetleyen oyun nasıl oynatılır. Bu sınıf, Denetim fizik, oluşturma ve puanlama düzeni oyuna genelinde kullanılan açıklayıcı değişkenleri içerir.

Örneğin, ulaşılacak durumlardır fizik aşağıdaki değişkenleri tarafından denetlenir:


```csharp
public static class GameCoefficients
{
    ...
    
    // The strength of the gravity. Making this a 
    // smaller (bigger negative) number will make the
    // fruit fall faster. A larger (smaller negative) number
    // will make the fruit more floaty.
    public const float FruitGravity = -60;
    // The impact of "air drag" on the fruit, which gives
    // the fruit terminal velocity (max falling speed) and slows
    // the fruit's horizontal movement (makes the game easier)
    public const float FruitDrag = .1f;
    // Controls fruit collision bouncyness. A value of 1
    // means no momentum is lost. A value of 0 means all
    // momentum is lost. Values greater than 1 create a spring-like effect
    public const float FruitCollisionElasticity = .5f;
    
    ...
}
```

Adları gelmez, bu değişkenleri ne kadar hızlı ulaşılacak durumlardır düştüğünde, nasıl yatay ayarlamak için kullanılabilir taşıma zaman yavaşlar zaman ve raket bounciness.

Kod veya veri dosyaları (örneğin, XML) depolanan oyun katsayısını ayrıca programcıları olmayan oyun tasarımcılar ile takımlar için özellikle yararlı olabilir.

`GameCoefficients` Sınıfı ayrıca bilgi ya da çakışma alanları örnekten oluşturmak gibi hata ayıklama bilgileri açmak için değerleri içerir:


```csharp
public static class GameCoefficients
{
    ...
    // This controls whether debug information is displayed on screen.
    public const bool ShowDebugInfo = false;
    public const bool ShowCollisionAreas = false;
    ...
}
```


## <a name="conclusion"></a>Sonuç

Bu kılavuz Fruity döner oyun incelediniz. İçerik, fizik ve oyun durum yönetimi dahil olmak üzere kavramlarını ele.

## <a name="related-links"></a>İlgili bağlantılar

- [CocosSharp API belgeleri](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Projeyi (örnek)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
