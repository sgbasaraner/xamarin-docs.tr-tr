---
title: CocosSharp varlıklar
description: Varlık düzeni oyun kod düzenlemek için güçlü bir yoludur. Okunabilirliğini artırır, kod korumak daha kolay hale getirir ve yerleşik üst/alt işlevselliğini kullanır.
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 58a8d4e6fcb8a2165fafad74a5c59481d1550351
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="entities-in-cocossharp"></a>CocosSharp varlıklar

_Varlık düzeni oyun kod düzenlemek için güçlü bir yoludur. Okunabilirliğini artırır, kod korumak daha kolay hale getirir ve yerleşik üst/alt işlevselliğini kullanır._

Varlık düzeni CocosSharp bir geliştiricinin çabayla geliştirilmiş kod kuruluş aracılığıyla artırabilir. Bu kılavuz, iki varlık – sevk varlık ve madde işareti varlığı oluşturma gösteren uygulamalı bir örnek olacaktır. Bu varlıklar kendi içinde yer alan nesneleri, bir kez örneği anlamına olacaktır otomatik olarak oluşturulur ve bunların türü için uygun şekilde hareket mantığı gerçekleştirir. 

Bu kılavuz, aşağıdaki konuları kapsar:

 - Oyun varlıklar giriş
 - Belirli bir varlık türleri ve genel
 - Proje Kurulumu
 - Sınıflar oluşturma
 - Varlık örnekleri ekleme `GameLayer`
 - Varlık mantığında tepki verme `GameLayer`

Tamamlanmış oyun şuna benzeyecektir:

![](entities-images/image1.png "Tamamlanmış oyun şuna benzeyecektir")


## <a name="introduction-to-game-entities"></a>Oyun varlıklar giriş

İşleme, çakışma, fizik veya yapay zeka mantığı gerek nesneleri tanımlayan sınıflar oyun varlıklardır. Neyse ki, temel bir oyunun kodu mevcut varlıklar genellikle oyun kavramsal nesneleri eşleşir. Bu doğruysa bir oyunda gereken varlıklar tanımlayan daha kolay gerçekleştirilebilir. 

Örneğin, bir boşluk konulu ['em oyun atış](http://en.wikipedia.org/wiki/Shoot_%27em_up) aşağıdaki varlıklar içerebilir:

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

Bu varlıklar oyundaki kendi sınıfları olabilir ve her bir örnek örneklemesi ötesinde çok az kayıpla veya hiç Kurulum gerektirir.


## <a name="general-vs-specific-entity-types"></a>Belirli bir varlık türleri ve genel

Bir varlık sistemini kullanarak oyun geliştiriciler tarafından karşılaştığı ilk soruları kendi varlıklar genelleştirmek için ne kadar biridir. Birkaç özelliklerine göre farklılık gösterse bile uygulamalarının en belirli sınıfları, varlığın her türü için tanımlarsınız. Daha fazla genel sistemleri bir sınıfına eşlemediğinden birleştirmek ve özelleştirilebilecek örneğine izin verme.

Örneğin biz aşağıdaki sınıflar tanımlayan alanı oyun düşünebilirsiniz:

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

Daha fazla genelleştirilmiş bir yaklaşım, tek bir sınıf için player verilir ve farklı sevk türlerini desteklemek için yapılandırılması rakip gelir için tek bir sınıf oluşturmak için olacaktır. Özelleştirme, taşıma katsayısını çekim yaparken oluşturmak için madde işareti varlıkları hangi tür yükleme için hangi görüntü içerebilir ve ikincisinde AI mantığı gelir. Bu durumda varlıkların listesi için azaltılabilir:

 - `PlayerShip`
 - `EnemyShip`

Elbette, bu varlık türleri daha fazla taşıma denetleme örnek özelleştirme vererek genelleştirilmiş. Rakip sevk örnekleri AI mantığı gerçekleştirirken Player sevk örnekleri girdiden okunan. Bu varlıklar genelleştirilmiş anlamına gelir. tek bir sınıf bile daha:

 - `Ship`

Genelleştirme daha – oyun tüm varlıklar için tek bir temel sınıf kullanabilir devam edebilirsiniz. Çağrılabilir bu tek sınıf `GameEntity`, her varlık örneğinin tüm oyundaki kullanılan sınıf olacaktır, madde işaretleri ve collectible öğeleri (güç-ups) gibi olmayan varlıklar dahil olmak üzere birlikte gelir.

Bu çoğu genel sistemlerinin genellikle olarak adlandırılır bir *bileşen sistem*. Böyle bir sistem bileşenleri işleme davranış ve görünümünü özelleştirmek için eklendi veya oyun varlıklar fizik, AI, gibi bileşenleri tek tek olabilir. Saf bileşen tabanlı sistemler ultimate esneklik etkinleştirin ve devralma gibi karmaşık devralma zincirleri kullanımı nedeniyle oluşan sorunları önleyebilirsiniz. Diğer Genelleştirme olduğu gibi etkili bileşen sistemleri ayarlamak zor olabilir ve kod anlamlılık azaltabilir.

Kullanılan Genelleştirme düzeyini de dahil olmak üzere birçok etmenlere bağlıdır:

 - Oyun boyutu – büyük oyunlar çok sayıda sınıfları yönetmek zor olabilir ancak belirli sınıfları oluşturmak daha küçük oyunlar destekleyebilir.
 - (Görüntüleri, 3B modeli ve veri dosyalarını JSON veya XML gibi) veri Bel oyunlar veri geliştirme – tabanlı fayda varlık türleri genelleştirilmiş gelen ve verilerine dayalı özellikleri yapılandırma. Bu, özellikle alma geliştirme sırasında veya oyun serbest bırakıldıktan sonra yeni içerik eklemek için beklediğiniz oyunlar için olur.
 - Diğer varlıklar düzenlemek nasıl karar vermek geliştiricilere izin verirken, oyun altyapısı desenleri – bazı oyun motorları bileşen sistemleri kullanımını kesinlikle öneririz. Geliştiriciler hiçbir varlık türünü uygulamak ücretsiz; bu nedenle CocosSharp bir bileşen sistem kullanımını gerektirmez. 

Basitleştirmek amacıyla, biz belirli bir sınıf tabanlı yaklaşım ile tek bir sevk ve madde işareti varlık Bu öğretici için kullanırsınız.


## <a name="project-setup"></a>Proje Kurulumu

Bizim varlıklar uygulama başlamadan önce bir proje oluşturmak gerekir. Biz CocosSharp proje şablonları projesi oluşturmayı basitleştirmek için kullanırsınız. [Bu post denetleyin](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio) Mac şablonları için Visual Studio'dan bir CocosSharp projesi oluşturma hakkında bilgi için. Bu kılavuzda kalan proje adı kullanacağı **EntityProject**.

Projemizin oluşturulduktan sonra 480 x 320 çalıştırmak için bizim oyun çözümlenmesi yaparız. Bunu yapmak için arama `CCScene.SetDefaultDesignResolution` içinde `GameAppDelegate.ApplicationDidFinishLaunching` yöntemini aşağıdaki şekilde:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

CocosSharp çözümleri yapılacağı hakkında daha fazla bilgi için bkz: bizim [birden çok çözümleri CocosSharp işleme kılavuzuna](~/graphics-games/cocossharp/resolutions.md).


## <a name="adding-content-to-the-project"></a>Projeye içerik ekleme

Projemizin oluşturulduktan sonra yer alan dosyaları ekleyeceğiz [bu içerik zip dosyası](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true). Bunu yapmak için ZIP dosyasını indirin ve onu sıkıştırmasını açın. Her ikisi de eklemek **ship.png** ve **bullet.png** için **içerik** klasör. **İçerik** klasörü içinde olacaktır **varlıklar** klasörü android'de ve iOS projesi kökündeki olacaktır. Eklendikten sonra her iki dosyalarında görmeliyiz **içerik** klasörü:

![](entities-images/image2.png "Eklendikten sonra her iki dosya içerik klasöründe olmalıdır")


## <a name="creating-the-ship-entity"></a>Sevk varlık oluşturma

`Ship` Sınıfı bizim oyunun ilk varlık olacaktır. Eklemek için bir `Ship` sınıfı, ilk adlı bir klasör oluşturun **varlıklar** projenin kök düzeyinde. Yeni bir sınıf ekleyin **varlıklar** adlı bir klasör `Ship`:

![](entities-images/image3.png "Sevk adlı varlıklar klasöründe yeni bir sınıf ekleyin")

Vermiyoruz için ilk değişiklik bizim `Ship` sınıfı, bu devralınan izin vermek için `CCNode` sınıfı. `CCNode` Ortak CocosSharp sınıfları için temel sınıf gören ister `CCSprite` ve `CCLayer`ve bize aşağıdaki işlevleri sunar:

 - `Position` Ekran sevk taşıma için özellik.
 - `Children` özellik eklemek için bir `CCSprite.`
 - `Parent` eklemek için kullanılan özellik `Ship` diğer örnekleri `CCNodes`. Biz bu özellik Bu öğreticide kullanılmasını olmaz; daha büyük oyunlar genellikle yararlanın diğer varlıklar ekleme `CCNodes`. 
 - `AddEventListener` Sevk taşımak için giriş yanıtlama yöntemi.
 - `Schedule` Madde işaretleri giderme yöntemi.

Ayrıca ekleyeceğiz bir `CCSprite` bizim sevk ekranda görülebilir böylece örneği:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

Ardından, sevkiyat ekleyeceğiz bizim `GameLayer` bizim oyunda görünmesini görmek için:


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

Biz şimdi göreceğiz bizim oyun bizim sevk varlık çalıştırırsanız:

![](entities-images/image4.png "Oyun çalıştırırken sevk varlık görüntülenir")


### <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>CCNode CCSprite yerine neden devralınmalıdır?

Bu noktada bizim `Ship` için basit bir sarmalayıcı sınıftır bir `CCSprite` örneği. Bu yana `CCSprite` de devraldığı `CCNode`, biz doğrudan devralınan `CCSprite`, hangi azaltılmış kodda `Ship.cs`. Ayrıca, doğrudan içinden devralma `CCSprite` bellekteki nesne sayısını azaltır ve bağımlılık ağacı küçülterek performansı artırabilir.

Kaynağından devralındı avantajlar rağmen biz `CCNode` bazı gizlemek için `CCSprite` her örneğinden özellikleri. Örneğin, `Texture` özelliği olmayan değiştirilmelidir dışında `Ship` sınıfı ve içinden devralma `CCNode` bu özelliği gizle olanak tanır. Bizim varlıkların Genel üyeler oyun büyük büyüdükçe ve ek geliştiricileri için takım eklendikçe özellikle önemli hale gelmiştir.


## <a name="adding-input-to-the-ship"></a>Sevk giriş ekleme

Bizim sevk ekranda görünür olmasını şimdi biz giriş eklemek olacaktır. Yaklaşımımız alınan yaklaşımın benzer olacaktır [BouncingGame Kılavuzu](~/graphics-games/cocossharp/bouncing-game.md), biz hareket kodunu yerleştirme ancak bu `Ship` sınıfı yerine içeren `CCLayer` veya `CCScene`.

Koda ekleme `Ship` kullanıcı ekran yerde değecek için taşıma desteklemek için:


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

Birçok 'em oyunlar uygulama geleneksel denetleyicisi tabanlı taşıma mimicking en yüksek hız, atış. Bu, biz yalnızca kodumuza kısa tutmak için hemen taşıma uygulamak belirtti.


## <a name="creating-the-bullet-entity"></a>Madde işareti varlık oluşturma

Bizim basit oyun ikinci varlıkta madde işaretleri görüntüleme bir varlıktır. Olduğu gibi `Ship` varlık, `Bullet` varlık içerecek bir `CCSprite` böylece ekranında görüntülenir. Taşıma için kullanıcı girişi dayanmayacak taşıma mantığı farklıdır; Bunun yerine, `Bullet` örnekleri hız özellikleri kullanılarak bir çizgide taşınır.

Yeni bir sınıf dosyasına ilk ekleyeceğiz bizim **varlıklar** klasörü ve çağrısından **madde işareti**:

![](entities-images/image5.png "Yeni bir sınıf dosyası varlıklar klasörüne ekleyin ve madde işareti çağırın")

Değiştirme eklendikten sonra `Bullet.cs` aşağıdaki gibi kod:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

İçin kullanılan dosyayı değiştirme yanı sıra `CCSprite` için `bullet.png`, kodda `ApplyVelocity` üzerinde iki katsayısını alan taşıma mantığı içerir: `VelocityX` ve `VelocityY`.

`Schedule` Çerçeve her çağrılacak temsilcileri ekleme yöntemi sağlar. Bu durumda biz eklediğiniz `ApplyVelocity` yöntemi böylece bizim madde işareti göre hız değerlerini taşır. `Schedule` Metodu bir `Action<float>`, burada float parametre süreyi (saniye cinsinden) zamana dayalı taşıma uygulamak için kullanırız son çerçeve itibaren belirtir. Zamanından bu yana değeri saniye cinsinden ölçülür ve hareket bizim hız değerlerini temsil eden *saniye başına piksel*.


## <a name="adding-bullets-to-gamelayer"></a>GameLayer ekleme işaretleri

Biz herhangi eklemeden önce `Bullet` bizim oyun örneklerine biz yapacak bir kapsayıcı özellikle bir `List<Bullet>`. Değiştirme `GameLayer` madde işaretleri listesini içerecek şekilde:


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

Sonraki doldurmak ihtiyacımız `Bullet` listesi. Ne zaman oluşturulacağını mantığını bir `Bullet` bulunan `Ship` varlık, ancak `GameLayer` madde işaretleri listesini depolamak için sorumludur. Fabrika düzeni izin vermek için kullanacağız `Ship` oluşturmak için varlık `Bullet` örnekleri. Bu fabrikada bir olay açığa çıkarır, `GameLayer` işleyebilir. 

Bunu yapmak için ilk olarak, bir klasör adı verilen Projemizin ekleyeceğiz **oluşturucuları**, ve ardından adlı yeni bir sınıf ekleyin `BulletFactory`:

![](entities-images/image6.png "Bir klasör oluşturucuları adlı projeye ekleyin ve sonra BulletFactory adlı yeni bir sınıf ekleyin")

Ardından, biz uygulamak `BulletFactory` singleton sınıfı:


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

`Ship` Varlık oluşturma işleyecek `Bullet` örnekleri – özellikle, onu işleyecek ne sıklıkta `Bullet` örnekleri oluşturulmalıdır (yani ne sıklıkta madde işareti tetiklenir), konumlarını ve bunların hız.

Değiştirme `Ship` yeni eklemek için varlığın Oluşturucusu `Schedule` çağırın ve bu yöntem aşağıdaki gibi gerçekleştirin:


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

Yeni oluşturulmasını işlemek için son adımdır `Bullet` içinde örnekleri `GameLayer` kodu. Bir olay işleyicisi ekleme `BulletCreated` yeni oluşturulan ekler olay `Bullet` uygun listeler için:


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

Biz oyunu çalıştırmak ve bakın artık `Ship` çekim `Bullet` örnekleri:

![](entities-images/image1.png "Oyun çalıştırın ve sevk madde işareti örnekleri çekim")


## <a name="why-gamelayer-has-ship-and-bullets-members"></a>Neden GameLayer sevk ve madde işaretlerini üyeler içeriyor

Bizim `GameLayer` sınıfı tanımlayan bizim varlık örneklerini başvuruları tutmak için iki alan (`ship` ve `bullets`), ancak bunlarla hiçbir şey yapmaz. Ayrıca, varlıklar, taşıma ve atma gibi kendi davranışı sorumludur. Bu nedenle neden eklediğimiz `ship` ve `bullets` alanları `GameLayer`?

Bir tam oyun uygulaması mantık gerektiren bu üyeleri eklediğimiz neden olduğundan `GameLayer` farklı varlıklar arasındaki etkileşim için. Örneğin, bu oyun oynatıcısının yok edilmesi enemies dahil etmek için daha fazla geliştirilebilir. Bu enemies yer bir `List` içinde `GameLayer`ve test etmek için mantığı olup olmadığını `Bullet` örnekleri birbiriyle çakışır ile enemies gerçekleştirilmesi `GameLayer` de. Diğer bir deyişle, `GameLayer` kökü *sahibi* tüm varlık örnekleri ve varlık örnekleri arasındaki etkileşimler sorumlu.


## <a name="bullet-destruction-considerations"></a>Madde işareti yok etme hakkında dikkat edilecek noktalar

Bizim oyun yok etme için kod şu anda eksik `Bullet` örnekleri. Her `Bullet` örneğinin ekranda taşıma mantığı vardır, ancak biz herhangi off-screen yok etmek için herhangi bir kod eklemediniz `Bullet` örnekleri.

Ayrıca, yok etme `Bullet` örnekleri değil de ait `GameLayer`. Örneğin, ekran dışı yok yerine `Bullet` varlık kendisini belirli bir miktar süre sonra yok etmek için mantığı sahip olabilir. Bu durumda, `Bullet` bu şekilde yok edilmesi, iletişim kurmak için bir yola ihtiyaç duyar `GameLayer`, gibi kadar `Ship` varlık iletişim için `GameLayer` , yeni bir `Bullet` üzerinden oluşturuldu `BulletFactory`.

En basit yok etme desteklemek için fabrika sınıfı sorumluluğunda genişletmek için çözümüdür. Fabrika olduğu gibi başka nesneler tarafından işlenmesi yok varlık örneğini bildirilebilir sonra `GameLayer` kendi listelerinden varlık örneğini kaldırma. 

## <a name="summary"></a>Özet

Bu kılavuz devralarak CocosSharp varlıklar oluşturmak nasıl gösterir `CCNode` sınıfı. Bu varlık, kendi görselleri ve Özel mantık oluşturulmasını işleme, kendi içinde bulunan nesneleridir. Bu kılavuz bir varlık (taşıma ve diğer varlıklar oluşturulmasını) ait kod kök varlık kapsayıcısında (Çakışma ve diğer varlık etkileşim mantığı) ait kodundan belirler.

## <a name="related-links"></a>İlgili bağlantılar

- [CocosSharp API belgeleri](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [İçerik sıkıştırma](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
