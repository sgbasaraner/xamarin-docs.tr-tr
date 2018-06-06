---
title: BouncingGame ayrıntıları
description: Bu belge, BouncingGame, bir basit Sıçrama Top CocosSharp ile kurulu oyunu oluşturulması için bir kılavuz sağlar.
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: 1a2423fcbeec0c3414b766a6c9e6f64ab53f4bf9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783114"
---
# <a name="bouncinggame-details"></a>BouncingGame ayrıntıları

> [!TIP]
> BouncingGame için kod bulunabilir [Xamarin örnekleri](https://developer.xamarin.com/samples/mobile/BouncingGame/). En iyi yanı sıra bu kılavuzu takip etmek için indirin ve başvurduğu kılavuzu gibi kodu keşfedin.

Bu kılavuzda, basit Sıçrama Top CocosSharp kullanarak oyun uygulaması gösterilmiştir. 

 - Şunu Oyun içeriği
 - Ortak CocosSharp görsel öğeleri
 - Görsel öğeleri ekleme `GameLayer`
 - Çerçeve her mantığını uygulama

Bizim tamamlanmış oyun şuna benzeyecektir:

![Tamamlanan BouncingGame](bouncing-game-images/image1.png "BouncingGame, tamamlandı")

## <a name="unzipping-game-content"></a>Şunu Oyun içeriği

Oyun geliştiriciler sık kullandığınız terimi *içerik* genellikle visual sanatçılar, oyun tasarımcıları ya da ses tasarımcıları tarafından oluşturulan kodu olmayan dosyaları belirtmek için. Ortak içerik türlerini görselleri görüntülemek, ses çalma veya yapay bilgileri (AI) davranışını denetlemek için kullanılan dosyalar içerir. Bir oyun geliştirme ekibin açısından bakıldığında, içeriği genellikle programcıları olmayan tarafından oluşturulur. Bizim oyun iki içerik türlerini içerir:

 - Top ve raket nasıl göründüğünü tanımlamak için png dosyaları.
 - (Aşağıdaki puan görüntü eklediğimiz daha ayrıntılı olarak açıklanmıştır) puan görüntü tarafından kullanılan yazı tipi tanımlamak için bir tek xnb dosyası

Burada kullanılan bu içerik bulunabilir [içerik zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true). Biz bu dosyaları indirilir ve bu kılavuzda daha sonra erişimi bir konuma sıkıştırması açılmış gerekir.

## <a name="common-cocossharp-visual-elements"></a>Ortak CocosSharp görsel öğeleri

CocosSharp görselleri görüntülemek için kullanılan sınıflar sağlar. Diğer kuruluş için kullanılırken bazı öğeler doğrudan görülebilir. Oyun aşağıdaki kullanacağız:

 - `CCNode` – CocosSharp içindeki tüm nesneleri için temel sınıf. `CCNode` Sınıfı içeren bir `AddChild` bir üst/alt hiyerarşisi oluşturmak için kullanılan işlevi da denir bir *görsel ağaç* veya *Sahne grafik*. Aşağıda belirtilen tüm sınıflar devralınmalıdır `CCNode`.
 - `CCScene` – Tüm CocosSharp oyunlar için görsel ağaç kökü. Tüm görsel öğeleri ile visual ağacının bir parçası olmalıdır bir `CCScene` kökündeki veya bunlar görünmez.
 - `CCLayer` – Gibi visual kapsayıcı nesneleri `CCSprite`. Adından da anlaşılacağı gibi `CCLayer` sınıfı birbirinin nasıl görsel öğeleri katman denetlemek için kullanılır.
 - `CCSprite` – Bir görüntü veya görüntünün bir bölümünü görüntüler. `CCSprite` örnekleri değişirse yerleştirilebilir ve çok sayıda görsel efektler sağlar.
 - `CCLabel` – Bir dize ekranda görüntüler. Tarafından kullanılan yazı tipi `CCLabel` xnb dosyasında tanımlanır. Biz xnbs aşağıdaki daha ayrıntılı ele alacağız.

Farklı uygulama türleri nasıl kullanılacağını anlamak için tek bir ele alacağız `CCSprite`. Görsel öğeleri eklenmelidir bir `CCLayer`, ve görsel ağaç olmalıdır bir `CCScene` , kökü. Bu nedenle, tek bir üst/alt ilişkisi `CCSprite` olacaktır `CCScene`  >  `CCLayer`  >  `CCSprite`.

## <a name="adding-visual-elements-to-gamelayer"></a>GameLayer için görsel öğeleri ekleme

### <a name="adding-the-paddle-sprite"></a>Raket hareketli ekleme

Siyah ve tek bir eklemek için oyunun arka plan başlangıçta yaparız `CCSprite` ekranda oluşturma. Değiştirme `GameLayer` eklemek için sınıfı bir `CCSprite` gibi:

```csharp
using System;
using System.Collections.Generic;
using CocosSharp;
namespace BouncingGame
{
    public class GameLayer : CCLayer
    {
        CCSprite paddleSprite;
        public GameLayer () : base(CCColor4B.Black)
        {
            // "paddle" refers to the paddle.png image
            paddleSprite = new CCSprite ("paddle");
            paddleSprite.PositionX = 100;
            paddleSprite.PositionY = 100;
            AddChild (paddleSprite);
        }
        protected override void AddedToScene ()
        {
            base.AddedToScene ();
            // Use the bounds to layout the positioning of the drawable assets
            CCRect bounds = VisibleBoundsWorldspace;
            // Register for touch events
            var touchListener = new CCEventListenerTouchAllAtOnce ();
            touchListener.OnTouchesEnded = OnTouchesEnded;
            AddEventListener (touchListener, this);
        }
        void OnTouchesEnded (List<CCTouch> touches, CCEvent touchEvent)
        {
            if (touches.Count > 0)
            {
                // Perform touch handling here
            }
        }
    }
}
```

Yukarıdaki kod tek bir oluşturur `CCSprite` ve bir alt öğesi olarak ekler `GameLayer`. `CCSprite` Oluşturucusu bize bir dize olarak kullanılacak resim dosyasını tanımlamanızı sağlar. Adlı bir dosyayı aramak için CocosSharp kodumuza söyler `paddle` (uzantısı, hangi bu kılavuzda ele alacağız atlanmış). CocosSharp herhangi dosya adları için görünür `paddle` kök içerik klasörüne (olduğu **içerik**) eklenen tüm klasörler yanı sıra `gameView.ContentManager.SearchPaths` (önceki bölümde açıklanmıştır).

Dosyaları doğrudan kök dizinine ekleyeceğiz **içerik** iOS ve Android için klasör. Bunu yapmak için sağ tıklatın ya da denetim tıklatıldığında **içerik** seçin ve iOS projesi klasöründe **Ekle** > **dosyaları Ekle...** Burada size içeriği daha önce unzipped için gidin ve seçin **paddle.png**. Klasöre dosya ekleme hakkında sorular, biz seçmelisiniz **kopyalama** seçeneği:

![Klasör iletişim için Dosya Ekle](bouncing-game-images/image2.png "dosya klasörü iletişim Ekle")

Ardından, Android projesine dosya ekleyeceğiz. Sağ tıklatın veya içerik klasörünü denetim tıklayın (olduğu içinde **varlıklar** Android projelerde klasörü) seçip seçin **Ekle** > **dosyaları Ekle...** . Bu süre, iOS projesine ait gidin **içerik** klasör. Dosya ekleme hakkında sorulduğunda seçin **bağlantı eklemek** seçeneği:

![Klasör iletişim için Dosya Ekle](bouncing-game-images/addalink.png "klasörü iletişim dosyasına Ekle")

Dosyaları neden hem projelerine eklenmesi gerekiyordu ele alacağız. Her projenin **içerik** klasörü şimdi içeren **paddle.png** dosyası:

![İçerik klasörünün içeriğini](bouncing-game-images/image3.png "içerik klasörünün içeriğini")

Biz oyun çalıştırırsanız göreceğiz `CCSprite` çizilen:

![Ekran çizilmiş raket](bouncing-game-images/image4.png "ekran çizilmiş raket")

### <a name="file-details"></a>Dosya Ayrıntıları

Tek bir dosyayı projeye kadarki ekledik ve işlem oldukça düz ilet. Yalnızca eklediğimiz **paddle.png** dosya **içerik** klasörleri ve kodda başvurulabilir. Şimdi CocosSharp dosyaları ile çalışırken bazı noktalar aramak için bir dakikanızı ayırın.

#### <a name="capitalization"></a>Büyük/küçük harf

Dosya adı ve dosyaya erişmek için kod kullandık dize her ikisi de küçük olduğuna dikkat edin. Diğer platformlar (örneğin, Android ve iOS cihaz) büyük küçük harfe duyarlı durumdayken bazı platformlar (örneğin, Windows Masaüstü ve iOS simülatörü) büyük küçük harf duyarsız olmasıdır. Böylece dosyaları ve kod platformlar kalır tüm küçük dosyalar bu öğreticinin geri kalanı için kullanacağız mümkün olduğunca.

#### <a name="extensions"></a>Uzantıları

Hareketli oluşturmaya yönelik Oluşturucu raket dosya başvururken ".png" uzantısı dahil değildir. Aynı varlık türü için dosya uzantıları olarak CocosSharp projelerde çalışma platformları arasında farklılık gösterebilir, dosya uzantısı genellikle atlanır. Örneğin, ses dosyaları platformda bağlı olarak .aiff, .mp3 veya .wma dosya biçimlerinin olabilir. Uzantı devre dışı bırakarak, dosya uzantısı bağımsız olarak tüm platformlarda çalışmak için aynı kodu sağlar.

#### <a name="content-in-platform-specific-projects"></a>Platforma özgü projelerinde içerik

İçinde olabilecek kod dosyaları çoğunu aksine bir PCL, içerik dosyaları her platforma özgü projeye eklenmelidir. CocosSharp bu iki nedenden dolayı gerektirir:

1. Her platformun farklı vardır **derleme eylemleri**. İOS projelere eklenen içerik kullanması gereken **BundleResource** derleme eylemi. Android projelere eklenen içerik kullanması gereken **AndroidAsset** derleme eylemi.
2. Bazı platformları, platforma özgü dosya biçimleri gerektirir. Örneğin, bu kılavuzda daha sonra anlatıldığı gibi yazı tipi .xnb dosyaları iOS ve Android farklıdır.

Bir dosya biçiminde (örneğin, .png) platformlar arasındaki farklı değildir, her platform aynı konuma dosyasından bir veya daha fazla platform burada bağlanabilir aynı dosyasını kullanabilirsiniz.

## <a name="orientation"></a>Yönlendirme

Yalnızca uygulama, diğer uygulamalar gibi CocosSharp uygulamalar dikey veya yatay yönler çalıştırabilirsiniz. Biz yalnızca dikey modunda çalışacak şekilde oyun ayarlamanız. İlk olarak, biz dikey en boy oranını işlemek için oyundaki çözüm kodu değiştireceksiniz. Bunu yapmak için değiştirmeniz `width` ve `height` değerler `LoadGame` yönteminde `ViewController` iOS sınıfı ve `MainActivity` Android sınıfında:

```csharp
void LoadGame (object sender, EventArgs e)
{
    CCGameView gameView = sender as CCGameView;

    if (gameView != null)
    {
        var contentSearchPaths = new List<string> () { "Fonts", "Sounds" };
        CCSizeI viewSize = gameView.ViewSize;

        int width = 768;
        int height = 1027;
    // ...
```

Sonraki biz her platforma özgü projesini dikey modunda çalışacak şekilde değiştirmeniz gerekir.

### <a name="ios-orientation"></a>iOS yönü

İOS projesinin yönünü değiştirmek için seçin **Info.plist** dosyasını **BouncingGame.iOS** proje ve değiştirme **iPhone/iPod dağıtım bilgileri** ve **iPad dağıtım bilgileri** yalnızca dikey yönler eklemek için:

![Yönlendirme seçenekleri](bouncing-game-images/image5.png "yönlendirme seçenekleri")

### <a name="android-orientation"></a>Android yönü

Android projenin yönünü değiştirmek için BouncingGame.Android projesinde MainActivity.cs dosyasını açın. Yalnızca dikey yönlendirme gibi belirtir şekilde etkinlik öznitelik tanımı değiştirin:

```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```

## <a name="default-coordinate-system"></a>Varsayılan koordinat sistemi

Başlatır kodumuza bir `CCSprite`, ayarlar `PositionX` ve `PositionY` 100 değerleri. Varsayılan olarak, bunun anlamı `CCSprite` merkezi konumlandırılmış 100 piksel kadar ve ekranın alt soldan sağa. Koordinat sistemi ekleyerek değiştirilebilir bir `CCCamera` için `CCLayer`. Biz ile çalışma olmaz `CCCamera` bu proje ancak hakkında daha fazla bilgi `CCCamera` bulunabilir [CocosSharp API belgeleri](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/).

Donanımda piksel aksine "Oyun" piksel yukarıdaki 100 piksel. Başka bir deyişle, konumlandırılmış ve fiziksel ekran göre doğru boyutta nesnelerinde farklı bir çözünürlük (örneğin, bir iPhone ve iPad), bir cihazda aynı oyun çalıştıran sonuçlanır. Özellikle, oyunun görünür alanı her zaman 1024 piksel uzunluğunda ve 768 piksel genişliğinde olmalıdır, bu çözümü olarak size daha önce belirttiğiniz `LoadGame` yöntemi.

## <a name="adding-the-ball-sprite"></a>Top hareketli ekleme

Biz ile çalışmanın temelleri hakkında bilgi sahibi olduğunuza `CCSprite`, ikinci ekleyeceğiz `CCSprite` – Top. Biz raket oluşturmamıza için çok benzer adımları izleyerek `CCSprite`. 

İlk olarak, ekleyeceğiz **ball.png** iOS projesinin sıkıştırması açılmış klasöründen görüntü **içerik** klasör. İçin Select **kopya** dosyasına **içerik** dizin. Bir bağlantı eklemek için yukarıdaki adımları izleyin **ball.png** Android projesi dosyasında.

Sonraki oluşturma `CCSprite` üyesi ekleyerek bu Top için adlı `ballSprite` için `GameScene` örneklemesi kodunu yanı sıra sınıfı `ballSprite`. Tamamlandığında `GameLayer` sınıfı şu şekilde görünür:

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
    }
```

## <a name="adding-the-score-label"></a>Puan etiket ekleme

Hesabına oyuna ekleyeceğiz son görsel öğe bir `CCLabel` sayısını görüntülemek için kullanıcı başarıyla Top postanın geri dönme. `CCLabel` Oluşturucuda belirtilen yazı tipi dizeleri ekranda görüntülemek için kullanır.

Oluşturmak için aşağıdaki kodu ekleyin bir `CCLabel` örneğini `GameLayer`. Bir kez tamamlanmış kod şu şekilde görünmelidir:

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    CCLabel scoreLabel;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "Arial", 20, CCLabelFormat.SystemFont);
        scoreLabel.PositionX = 50;
        scoreLabel.PositionY = 1000;
        scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
        AddChild (scoreLabel);
    }
    // ...
```

ScoreLabel kullanarak bildirim bir `AnchorPoint` , `CCPoint.AnchorUpperLeft`, anlamına `PositionX` ve `PositionY` değerleri sol üst konumunu tanımlayın. Bu bize sağlar konumu `scoreLabel` etiketin boyutlarının göz önünde bulundurun gerek kalmadan ekranın sol üst göre.

## <a name="implementing-every-frame-logic"></a>Çerçeve her mantığını uygulama

Şu ana kadar oyun statik Sahne gösterir. Biz yüksek sıklığı nesnelerin konumunu güncelleştiren kodu ekleyerek Sahne nesneleri hareketini denetlemek için mantık ekleme. Bu durumda, saniyede da altmış adlandırılan altmış kez - kodu çalıştıracak *çerçeveler* saniyede (donanım bu sık güncelleştirme işleyemiyor sürece). Özellikle, biz dönmesine ve karşı Raketi giriş göre hareket ve Top raket sağlar her zaman oyuncunun puan güncelleştirmek için raket Sıçrama Top yapmak için mantık ekleme.

`Schedule` Tarafından sağlanan yöntemi `CCNode` sınıfı, bize oyuna çerçeve her mantığı ekleyin olanak sağlar. Koddan sonra ekleyeceğiz `// New code` GameLayer oluşturucuya:

```csharp
public GameLayer () : base (CCColor4B.Black)
{
    // "paddle" refers to the paddle.png image
    paddleSprite = new CCSprite ("paddle");
    paddleSprite.PositionX = 100;
    paddleSprite.PositionY = 100;
    AddChild (paddleSprite);
    ballSprite = new CCSprite ("ball");
    ballSprite.PositionX = 320;
    ballSprite.PositionY = 600;
    AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "arial", 22, CCLabelFormat.SpriteFont);
    scoreLabel.PositionX = 50;
    scoreLabel.PositionY = 1000;
    scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
    AddChild (scoreLabel);
    // New code:
    Schedule (RunGameLogic);
}
```

Şimdi oluşturmak bir `RunGameLogic` yönteminde `GameLayer` tüm çerçeve her mantığı barındırmak sınıfı:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Kayan nokta parametresi süreyi saniye cinsinden dilimidir tanımlar. Biz bu değer Top 's taşıma uygularken kullanırsınız.

### <a name="making-the-ball-fall"></a>Kalan Top yapma

Biz ya da el ile uygulama yer çekimi kod veya CocosSharp içinde yerleşik Box2D işlevi kullanarak kalan Top yapabilirsiniz. Box2D fizik benzetimi altyapısı CocosSharp oyunlar için kullanılabilir. Oldukça güçlü ve etkili, ancak Kurulum kodu yazma gerektirir. Fizik benzetimi oldukça basit olduğundan, burada onu el ile uygulanacaktır.

Yer çekimi uygulamak için biz deposu geçerli X ve Y hızını Top gerekir. İki üyelerine ekleyeceğiz `GameLayer` sınıfı:

```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

Sonraki biz düşer halde mantık uygulayabilirsiniz `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

### <a name="moving-the-paddle-with-touch-input"></a>Dokunmatik girişle raket taşıma

Top dönmeden, yatay hareket raket kullanarak ekleyeceğiz `CCEventListenerTouchAllAtOnce` nesnesi. Bu nesne giriş ilgili olayların sayısını sağlar. Bu durumda, biz raket konumunu ayarlayabilmeniz için herhangi bir touch noktası taşırsanız bildirilmesini istiyoruz. `GameLayer` Zaten başlatır bir `CCEventListenerTouchAllAtOnce`, biz yalnızca atamanız gerekir böylece `OnTouchesMoved` temsilci. Bunu yapmak için AddedToScene yöntemi aşağıdaki gibi değiştirin:

```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of the drawable assets
    CCRect bounds = VisibleBoundsWorldspace;
    // Register for touch events
    var touchListener = new CCEventListenerTouchAllAtOnce ();
    touchListener.OnTouchesEnded = OnTouchesEnded;
    // new code:
    touchListener.OnTouchesMoved = HandleTouchesMoved;
    AddEventListener (touchListener, this);
}
```

Ardından, biz uygulamak `HandleTouchesMoved`:

```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```

### <a name="implementing-ball-collision"></a>Top çakışma uygulama

Biz oyun çalıştırırsanız şimdi biz Top raket düştüğünü fark edeceksiniz. Uygulamanız *çakışma* (oyun nesneleri çakışan tepki vermek için mantık) çerçeve her kod. Her çerçeve taşıma nesneler değişiklik konumlandırır olduğundan, çakışma olup olmadığı denetleniyor genellikle ayrıca her çerçeve gerçekleştirilir. Biz de hız X ekseninde Top oyuna bazı sınaması eklemeyi rakete, ancak bu ekran kenarları hareket etmesini Top engellemek ihtiyacımız anlamına gelir ekleme. Biz bu gerçekleştirirsiniz `RunGameLogic` için hız uyguladıktan sonra kod `ballSprite` sonra `// New Code`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
    // New Code:
    // Check if the two CCSprites overlap...
    bool doesBallOverlapPaddle = ballSprite.BoundingBoxTransformedToParent.IntersectsRect(
        paddleSprite.BoundingBoxTransformedToParent);
    // ... and if the ball is moving downward.
    bool isMovingDownward = ballYVelocity < 0;
    if (doesBallOverlapPaddle && isMovingDownward)
    {
        // First let's invert the velocity:
        ballYVelocity *= -1;
        // Then let's assign a random value to the ball's x velocity:
        const float minXVelocity = -300;
        const float maxXVelocity = 300;
        ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    }
    // First let’s get the ball position:   
    float ballRight = ballSprite.BoundingBoxTransformedToParent.MaxX;
    float ballLeft = ballSprite.BoundingBoxTransformedToParent.MinX;
    // Then let’s get the screen edges
    float screenRight = VisibleBoundsWorldspace.MaxX;
    float screenLeft = VisibleBoundsWorldspace.MinX;
    // Check if the ball is either too far to the right or left:    
    bool shouldReflectXVelocity = 
        (ballRight > screenRight && ballXVelocity > 0) ||
        (ballLeft < screenLeft && ballXVelocity < 0);
    if (shouldReflectXVelocity)
    {
        ballXVelocity *= -1;
    }
}
```

## <a name="adding-scoring"></a>Puanlama ekleme

Oyun oynanabilir, son adımı Puanlama mantığı eklemektir. Puan üyesi adlı GameLayer sınıfı için ilk olarak, ekleyeceğiz `score`:

```csharp
int score;
```

Oyuncunun puan izlemek ve kullanarak görüntülemek için bu değişkeni kullanacağız `scoreLabel`. Bunu yapmak için IF deyimi aşağıdaki kodu ekleyin `RunGameLogic` zaman top ve raket çakışıyor:

```csharp
// ...
if (doesBallOverlapPaddle && isMovingDownward)
{
    // First let's invert the velocity:
    ballYVelocity *= -1;
    // Then let's assign a random to the ball's x velocity:
    const float minXVelocity = -300;
    const float maxXVelocity = 300;
    ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    // New code:
    score++;
    scoreLabel.Text = "Score: " + score;
}
// ...
```

Şimdi biz oyunu çalıştırmak ve oyun Top dışına raket geri dönmeler olarak hangi artırır bir puan görüntüler bakın:

![Tamamlanan oyun artacak puanı](bouncing-game-images/image1.png "artacak puanı tamamlanmış oyun")

## <a name="summary"></a>Özet

Bu kılavuz, platformlar arası oyun grafikleri, fizik ve CocosSharp kullanarak giriş oluşturmak sunulmuştur. CocosSharp oyun Geliştirmeye Başlarken, ilk adımıdır. Bazı yaygın sınıfları CocosSharp, nesneleri düzgün çizilir şekilde görsel ağaç oluşturma ve çerçeve her oyun mantığını uygulamak nasıl inceledik.

Bu kılavuzda ne CocosSharp oyun altyapısı sunar, yalnızca küçük bir bölümün ele. Bilgi ve diğer CocosSharp konular hakkında talimatlar için bkz: [CocosSharp kılavuzları kalan](~/graphics-games/cocossharp/index.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Tamamlanan oyun (örnek)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [Oyun içeriği (örnek)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
