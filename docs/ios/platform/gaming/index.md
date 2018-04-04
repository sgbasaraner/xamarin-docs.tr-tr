---
title: iOS oyun API'leri
description: Bu makalede, Xamarin.iOS oyunun grafik ve ses özellikleri geliştirmek için kullanılan iOS tarafından 9 sağlanan yeni oyun geliştirmelerini ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 34d3d6980819510a3390e2c30069818d6dfd721f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="ios-gaming-apis"></a>iOS oyun API'leri

_Bu makalede, Xamarin.iOS oyunun grafik ve ses özellikleri geliştirmek için kullanılan iOS tarafından 9 sağlanan yeni oyun geliştirmelerini ele alınmaktadır._

Apple, oyun grafikleri ve ses bir Xamarin.iOS uygulaması uygulamak kolaylaştıran çeşitli teknolojik iyileştirmeler iOS 9'de API oyun yaptı.
Bunlar, her iki üst düzey çerçeveler ve geliştirilmiş hızı ve grafik yeteneklerini iOS cihazın GPU gücünü gücünden aracılığıyla geliştirme kolaylığı içerir.

[![](images/flocking01.png "Flocking çalışan bir uygulama örneği")](images/flocking01.png#lightbox)

Bu seçenek, GameplayKit, ReplayKit, Model g/ç, MetalKit ve Metal performans gölgelendiriciler birlikte Metal, SceneKit ve SpriteKit yeni, Gelişmiş özellikleri içerir.

Bu makalede, tüm iOS 9'ın yeni oyun geliştirmeleri ile Xamarin.iOS oyun geliştirmek için yöntemleri getirir:

## <a name="introducing-gameplaykit"></a>GameplayKit Tanıtımı

Apple'nın yeni GameplayKit framework oyunlar iOS cihazları için uygulama için gerekli yinelenen, ortak kod miktarını azaltarak oluşturmak kolaylaştıran bir dizi teknolojileri sağlar. Daha sonra kolayca grafik altyapı (örneğin, SceneKit veya SpriteKit) ile birleştirilebilir oyun mekanizması için hızlı bir şekilde geliştirmek için Araçlar tamamlanmış oyun teslim GameplayKit sağlar.

Oyun algoritmalar gibi GameplayKit birkaç, ortak içerir:

- Temel bir davranış, hareketleri ve AI otomatik olarak bir işin peşine düşmek hedefler tanımlamanıza olanak verir Aracısı benzetimi.
- Minmax yapay zeka oyun Aç tabanlı için.
- Veri temelli oyun mantığı emergent davranışı sağlamak için benzer mantığı ile bir kural sistem.

Ayrıca, GameplayKit aşağıdaki özellikleri sağlar modüler bir mimari kullanarak bir yapı bloğu yaklaşımını oyun geliştirme alır:

- Karmaşık, yordam kodu işlemek için Durum makinesi oyun sistemlerinde temel.
- Sağlamak için araçları oyun ve karşılaştırıldığında hata ayıklama sorunlara neden olmadan rastgele.
- Yeniden kullanılabilir, bileşenlerden oluşan bir varlık mimari temel.

GameplayKit hakkında daha fazla bilgi için Apple'nın bkz [Gameplaykit Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) ve [GameplayKit Framework başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199).

## <a name="gameplaykit-examples"></a>GameplayKit örnekleri

Bazı basit oyun mekanizması oyun Seti kullanarak bir Xamarin.iOS uygulaması uygulama bir hızlı bakalım.

### <a name="pathfinding"></a>Pathfinding

Pathfinding oyun tahtası kendi karşılaşmamak bulur oyun AI öğesi için yeteneğidir.
Örneğin, bir 2B ikincisinde bir Labirent kendi aşamalardaki veya ilk kişinin giderme world terrain aracılığıyla 3B bir karakter bulma.

Aşağıdaki harita göz önünde bulundurun:

[![](images/gkpathfindpath.png "Bir örnek pathfinding eşlemesi")](images/gkpathfindpath.png#lightbox)

Pathfinding kullanarak bu C# kod Haritası aşamalardaki bulabilirsiniz:

```csharp
var a = GKGraphNode2D.FromPoint (new Vector2 (0, 5));
var b = GKGraphNode2D.FromPoint (new Vector2 (3, 0));
var c = GKGraphNode2D.FromPoint (new Vector2 (2, 6));
var d = GKGraphNode2D.FromPoint (new Vector2 (4, 6));
var e = GKGraphNode2D.FromPoint (new Vector2 (6, 5));
var f = GKGraphNode2D.FromPoint (new Vector2 (6, 0));

a.AddConnections (new [] { b, c }, false);
b.AddConnections (new [] { e, f }, false);
c.AddConnections (new [] { d }, false);
d.AddConnections (new [] { e, f }, false);

var graph = GKGraph.FromNodes(new [] { a, b, c, d, e, f });

var a2e = graph.FindPath (a, e); // [ a, c, d, e ]
var a2f = graph.FindPath (a, f); // [ a, b, f ]

Console.WriteLine(String.Join ("->", (object[]) a2e));
Console.WriteLine(String.Join ("->", (object[]) a2f));
```

### <a name="classical-expert-system"></a>Klasik Expert sistem

Aşağıdaki C# kod parçacığını GameplayKit Klasik expert sistem uygulamak için nasıl kullanılabileceğini gösterir:

```csharp
string output = "";
bool reset = false;
int input = 15;

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    /*
    If reset is true, clear the output and set reset to false
    */
    var clearRule = GKRule.FromPredicate ((rules) => reset, rules => {
        output = "";
        reset = false;
    });
    clearRule.Salience = 1;

    var fizzRule = GKRule.FromPredicate (mod (3), rules => {
        output += "fizz";
    });
    fizzRule.Salience = 2;

    var buzzRule = GKRule.FromPredicate (mod (5), rules => {
        output += "buzz";
    });
    buzzRule.Salience = 2;

    /*
    This *always* evaluates to true, but is higher Salience, so evaluates after lower-salience items
    (which is counter-intuitive). Print the output, and reset (thus triggering "ResetRule" next time)
    */
    var outputRule = GKRule.FromPredicate (rules => true, rules => {
        System.Console.WriteLine(output == "" ? input.ToString() : output);
        reset = true;
    });
    outputRule.Salience = 3;

    var rs = new GKRuleSystem ();
    rs.AddRules (new [] {
        clearRule,
        fizzRule,
        buzzRule,
        outputRule
    });

    for (input = 1; input < 16; input++) {
        rs.Evaluate ();
        rs.Reset ();
    }
}

protected Func<GKRuleSystem, bool> mod(int m)
{
    Func<GKRuleSystem,bool> partiallyApplied = (rs) => input % m == 0;
    return partiallyApplied;
}
```

Belirli bir kural kümesine göre (`GKRule`) ve bilinen bir giriş, expert sistem kümesini (`GKRuleSystem`) tahmin edilebilir çıktısı oluşturur (`fizzbuzz` örneğimizde yukarıdaki için).

### <a name="flocking"></a>Flocking

Flocking AI birtakım hareketleri ve sağlama varlığın flock Kuşlar, ya da Okul yüzme balık, gibi eylemleri Grup burada yanıtlar bir flock olarak davranacak şekilde oyun varlıklar denetlenen sağlar.

Aşağıdaki C# kod parçacığını GameplayKit ve SpriteKit kullanarak için grafik görüntüleme flocking davranışı uygular:

```csharp
using System;
using SpriteKit;
using CoreGraphics;
using UIKit;
using GameplayKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
using OpenTK;

namespace FieldBehaviorExplorer
{
    public static class FlockRandom
    {
        private static GKARC4RandomSource rand = new GKARC4RandomSource ();

        static FlockRandom ()
        {
            rand.DropValues (769);
        }

        public static float NextUniform ()
        {
            return rand.GetNextUniform ();
        }
    }

    public class FlockingScene : SKScene
    {
        List<Boid> boids = new List<Boid> ();
        GKComponentSystem componentSystem;
        GKAgent2D trackingAgent; //Tracks finger on screen
        double lastUpdateTime = Double.NaN;
        //Hold on to behavior so it doesn't get GC'ed
        static GKBehavior flockingBehavior;
        static GKGoal seekGoal;


        public FlockingScene (CGSize size) : base (size)
        {
            AddRandomBoids (20);

            var scale = 0.4f;
            //Flocking system
            componentSystem = new GKComponentSystem (typeof(GKAgent2D));
            var behavior = DefineFlockingBehavior (boids.Select (boid => boid.Agent).ToArray<GKAgent2D>(), scale);
            boids.ForEach (boid => {
                boid.Agent.Behavior = behavior;
                componentSystem.AddComponent(boid.Agent);
            });

            trackingAgent = new GKAgent2D ();
            trackingAgent.Position = new Vector2 ((float) size.Width / 2.0f, (float) size.Height / 2.0f);
            seekGoal = GKGoal.GetGoalToSeekAgent (trackingAgent);
        }

        public override void TouchesBegan (NSSet touches, UIEvent evt)
        {
            boids.ForEach(boid => boid.Agent.Behavior.SetWeight(1.0f, seekGoal));
        }

        public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            boids.ForEach (boid => boid.Agent.Behavior.SetWeight (0.0f, seekGoal));
        }

        public override void TouchesMoved (NSSet touches, UIEvent evt)
        {
            var touch = (UITouch) touches.First();
            var loc = touch.LocationInNode (this);
            trackingAgent.Position = new Vector2((float) loc.X, (float) loc.Y);
        }


        private void AddRandomBoids (int count)
        {
            var scale = 0.4f;
            for (var i = 0; i < count; i++) {
                var b = new Boid (UIColor.Red, this.Size, scale);
                boids.Add (b);
                this.AddChild (b);
            }
        }

        internal static GKBehavior DefineFlockingBehavior(GKAgent2D[] boidBrains, float scale)
        {
            if (flockingBehavior == null) {
                var flockingGoals = new GKGoal[3];
                flockingGoals [0] = GKGoal.GetGoalToSeparate (boidBrains, 100.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [1] = GKGoal.GetGoalToAlign (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [2] = GKGoal.GetGoalToCohere (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);

                flockingBehavior = new GKBehavior ();
                flockingBehavior.SetWeight (25.0f, flockingGoals [0]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [1]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [2]);
            }
            return flockingBehavior;
        }

        public override void Update (double currentTime)
        {
            base.Update (currentTime);
            if (Double.IsNaN(lastUpdateTime)) {
                lastUpdateTime = currentTime;
            }
            var delta = currentTime - lastUpdateTime;
            componentSystem.Update (delta);
        }
    }

    public class Boid : SKNode, IGKAgentDelegate
    {
        public GKAgent2D Agent { get { return brains; } }
        public SKShapeNode Sprite { get { return sprite; } }

        class BoidSprite : SKShapeNode
        {
            public BoidSprite (UIColor color, float scale)
            {
                var rot = CGAffineTransform.MakeRotation((float) (Math.PI / 2.0f));
                var path = new CGPath ();
                path.MoveToPoint (rot, new CGPoint (10.0, 0.0));
                path.AddLineToPoint (rot, new CGPoint (0.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 20.0));
                path.AddLineToPoint (rot, new CGPoint (20.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 0.0));
                path.CloseSubpath ();

                this.SetScale (scale);
                this.Path = path;
                this.FillColor = color;
                this.StrokeColor = UIColor.White;

            }
        }

        private GKAgent2D brains;
        private BoidSprite sprite;
        private static int boidId = 0;

        public Boid (UIColor color, CGSize size, float scale)
        {
            brains = BoidBrains (size, scale);
            sprite = new BoidSprite (color, scale);
            sprite.Position = new CGPoint(brains.Position.X, brains.Position.Y);
            sprite.ZRotation = brains.Rotation;
            sprite.Name = boidId++.ToString ();

            brains.Delegate = this;

            this.AddChild (sprite);
        }

        private GKAgent2D BoidBrains(CGSize size, float scale)
        {
            var brains = new GKAgent2D ();
            var x = (float) (FlockRandom.NextUniform () * size.Width);
            var y = (float) (FlockRandom.NextUniform () * size.Height);
            brains.Position = new Vector2 (x, y);

            brains.Rotation = (float)(FlockRandom.NextUniform () * Math.PI * 2.0);
            brains.Radius = 30.0f * scale;
            brains.MaxSpeed = 0.5f;
            return brains;
        }

        [Export ("agentDidUpdate:")]
        public void AgentDidUpdate (GameplayKit.GKAgent agent)
        {
        }

        [Export ("agentWillUpdate:")]
        public void AgentWillUpdate (GameplayKit.GKAgent agent)
        {
            var brainsIn = (GKAgent2D) agent;
            sprite.Position = new CGPoint(brainsIn.Position.X, brainsIn.Position.Y);
            sprite.ZRotation = brainsIn.Rotation;
            Console.WriteLine ($"{sprite.Name} -> [{sprite.Position}], {sprite.ZRotation}");
        }
    }
}
```

Ardından, bu Sahne bir görünüm denetleyicisi uygulayın:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.
        this.View = new SKView {
        ShowsFPS = true,
        ShowsNodeCount = true,
        ShowsDrawCount = true
    };
}

public override void ViewWillLayoutSubviews ()
{
    base.ViewWillLayoutSubviews ();

    var v = (SKView)View;
    if (v.Scene == null) {
        var scene = new FlockingScene (View.Bounds.Size);
        scene.ScaleMode = SKSceneScaleMode.AspectFill;
        v.PresentScene (scene);
    }
}
```

Çalıştırdığınızda, biraz animasyonlu _"Boids"_ bizim parmak Tap flock:

[![](images/flocking01.png "Biraz animasyonlu Boids parmak Tap'ları flock")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>Diğer Apple örnekleri

Yukarıda gösterilen örnekleri yanı sıra, C# ve Xamarin.iOS için kod çevrimi olabilir aşağıdaki örnek uygulamaları Apple sağlamaktadır:

- [FourInARow: GameplayKit Minmax Strateji Uzmanı için rakibiniz AI kullanma](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: aracıları sistem GameplayKit içinde kullanma](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: platformlar arası oyun SpriteKit ve GameplayKit oluşturma](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

İOS 9'da, Apple birkaç değişiklikler ve eklemeler GPU düşük yükünü erişim sağlamak için Metal yapmıştır. Metal kullanarak, grafik ve iOS uygulamalarınızın bilgi işlem olası en üst düzeye çıkarabilirsiniz.

Metal framework aşağıdaki yeni özellikleri içerir:

- Yeni özel ve derinliği dokuları OS X için şablon.
- Derinlik clamping ve ayrı ön ve arka şablon değerleri ile geliştirilmiş gölge kalitesi.
- Metal gölgeleme dili ve Metal standart kitaplığı geliştirmeleri.
- Hesaplama gölgelendiriciler piksel biçimlerine daha geniş ölçüdeki destekler.

### <a name="the-metalkit-framework"></a>MetalKit Framework

MetalKit framework yardımcı sınıflar ve Metal bir iOS uygulamasını kullanmak için gereken iş miktarını azaltmak özellikler kümesi sağlar. MetalKit üç anahtar alanlar için destek sağlar:

1. Zaman uyumsuz doku kaynaklarını PNG, JPEG, KTX ve PVR gibi ortak biçimleri gibi çeşitli yükleniyor.
2. Model g/ç kolay erişimle varlıkları Metal belirli model işleme için temel. Bu özellikler modeli g/ç kafesleri ve Metal arabellekleri arasında verimli verileri aktarımını sağlamak için yüksek oranda iyileştirilmiş.
3. Önceden tanımlanmış Metal görünümleri ve bir iOS uygulamanızın içinde grafik renderings görüntülemek için gereken kod miktarını önemli ölçüde azaltan Görünüm Yönetimi.

MetalKit hakkında daha fazla bilgi için Apple'nın bkz [MetalKit Framework başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [Metal Programlama Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), [Metal Framework başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) ve [Metal Dil Kılavuzu gölgelendirme](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

### <a name="metal-performance-shaders-framework"></a>Metal performans gölgelendiriciler Framework

Grafik yüksek oranda iyileştirilmiş bir dizi Metal performans gölgelendirici framework sağlar ve hesaplama dayalı gölgelendiriciler, Metal kullanmak için temel iOS uygulamaları. Her gölgelendirici Metal performans framework özellikle Metal üzerinde yüksek performans sağlamak için ayarlanmış gölgelendirici işlemede iOS GPU desteklenir.

Metal performans gölgelendirici sınıflarını kullanarak hedef ve tek tek kod temelleri sürdürmek zorunda kalmadan her belirli iOS GPU olası en yüksek performans elde edebilirsiniz. Metal performans gölgelendiriciler dokuları ve arabellekleri gibi herhangi bir Metal kaynak ile kullanılabilir.

Metal performans gölgelendirici framework gibi ortak gölgelendiriciler kümesini sağlar:

- **Gauss Bulanıklığı** (`MPSImageGaussianBlur`)
- **Sobel kenar algılama** (`MPSImageSobel`)
- **Görüntü Histogram** (`MPSImageHistogram`)

Daha fazla bilgi için lütfen Apple'nın bkz [Metal gölgelendirme dil Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

## <a name="introducing-model-io"></a>Model g/ç Tanıtımı

Apple'nın Model g/ç çerçevesi (örneğin, modelleri ile ilgili kaynakları) 3B varlıklar derin bir anlayış sağlar. Model g/ç iOS oyunlarınızda fiziksel tabanlı malzemeleri modelleri ve GameplayKit, Metal ve SceneKit kullanılabilir aydınlatma ile sağlar.

Model g/ç ile aşağıdaki görevleri türlerini destekleyebilir:

- Işık, malzemeleri alma, veri, kamera ayarlarını ve diğer popüler yazılım ve oyun altyapısı biçimleri çeşitli Sahne tabanlı bilgileri mesh.
- İşlem veya Sahne tabanlı bilgileri procedurally oluşturmak gibi sky domes veya kafes ışık Fırından doku oluşturmak.
- MetalKit, SceneKit ve GLKit oyun varlıklar GPU arabellekleri işleme için verimli bir şekilde yüklemek için birlikte çalışır.
- Sahne tabanlı bilgilere çeşitli popüler yazılım ve oyun altyapısı biçimleri verin.

Model g/ç hakkında daha fazla bilgi için Apple'nın bkz [modeli g/ç Framework başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>ReplayKit Tanıtımı

Apple'nın yeni ReplayKit framework kolayca iOS oyununuzu oyun kaydını ekleyin ve hızlı ve kolay bir şekilde düzenleyin ve bu video uygulama içinde paylaşmak kullanıcı izin verir.

Apple'nın daha fazla bilgi için lütfen bkz [ReplayKit ve oyun merkezi video sosyal giderek](https://developer.apple.com/videos/wwdc/2015/?id=605) ve bunların [DemoBots: SpriteKit ve GameplayKit Çapraz Platform oyun oluşturma](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) örnek uygulama.

## <a name="scenekit"></a>SceneKit

Sahne, 3B Sahne grafik 3B grafik çalışmak basitleştirir API'si setidir. OS X 10.8 ilk sunulmuştur ve iOS 8 şimdi geldi. Sahne Seti'nde derinlikli 3B Görselleştirme ve sıradan 3B oyunlar oluşturma OpenGL uzmanlık gerektirmez. Ortak Sahne grafik kavramlar oluşturma, Sahne Seti hemen OpenGL ve çok 3B eklemek bir uygulama için içerik kolaylaşır OpenGL ES karmaşıklığını soyutlar. Ancak, OpenGL uzmanı varsa, Sahne Seti doğrudan OpenGL ile de bağlamadan harika desteğe sahiptir. Ayrıca fizik gibi 3B grafik tamamlayan çeşitli özellikler içerir ve çok iyi çekirdek animasyon, çekirdek görüntü ve hareketli grafik Seti gibi diğer Apple çerçeveleri ile tümleşir.

Daha fazla bilgi için lütfen bkz bizim [SceneKit](~/ios/platform/gaming/scenekit.md) belgeleri.

### <a name="scenekit-changes"></a>SceneKit değişiklikleri

Apple, iOS 9 SceneKit için aşağıdaki yeni özellikleri ekledi:

- Xcode şimdi Xcode içinde perde doğrudan düzenleyerek oyunlarını ve etkileşimli 3B Hızlı bir şekilde oluşturmanıza olanak sağlayan bir Sahne Düzenleyicisi sağlar.
- `SCNView` Ve `SCNSceneRenderer` sınıfları, Metal işleme (desteklenen iOS cihazlarda) etkinleştirmek için kullanılabilir.
- `SCNAudioPlayer` Ve `SCNNode` sınıfları, otomatik olarak bir iOS uygulamasının player konumu izleme uzamsal ses efektleri eklemek için kullanılabilir.

Daha fazla bilgi için lütfen bkz bizim [SceneKit belgelerine](~/ios/platform/introduction-to-ios8.md#scenekit) ve Apple'nın [SceneKit Framework başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) ve [Fox: SceneKit oyun Xcode Sahne Düzenleyicisiilederleme](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)örnek proje.

## <a name="spritekit"></a>SpriteKit

Hareketli grafik Seti, 2B oyun framework Apple, iOS 8 ve OS X Yosemite ilginç bazı yeni özellikler vardır. Bunlar Sahne Seti, gölgelendirici desteği, aydınlatma, gölge, kısıtlamalar, normal eşlem oluşturma ve fizik geliştirmeleri ile tümleştirme içerir. Özellikle, yeni fizik özellikleri çok oyun için gerçekçi efektler eklemek kolaylaştırır.

Daha fazla bilgi için lütfen bkz bizim [SpriteKit](~/ios/platform/gaming/spritekit.md) belgeleri.

### <a name="spritekit-changes"></a>SpriteKit Changes

Apple, iOS 9 SpriteKit için aşağıdaki yeni özellikleri ekledi:

- Otomatik olarak player'ın konumu ile izleme uzamsal ses efekti `SKAudioNode` sınıfı.
- Xcode şimdi Sahne Düzenleyicisi ve kolay 2B oyun ve uygulama oluşturmak için eylem Düzenleyicisi özellikleri.
- Kolay kaydırma yeni kamera düğümlerle oyun desteği (`SKCameraNode`) nesneleri.
- Özel OpenGL ES gölgelendiriciler zaten kullanmakta olduğunuz olsa bile Metal destekleyen iOS cihazlarda SpriteKit otomatik olarak onu işleme için kullanır.

Daha fazla bilgi için lütfen bkz bizim [SpriteKit belgelerine](~/ios/platform/introduction-to-ios8.md#spritekit) Apple'nın [SpriteKit Framework başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) ve bunların [DemoBots: SpriteKit ile Çapraz Platform oyun oluşturma ve GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) örnek uygulama.

## <a name="summary"></a>Özet

Bu makalede yeni kapsamına oyun özellikleri, iOS 9 Xamarin.iOS uygulamaları için sağlar.
GameplayKit ve Model g/ç sunulan; Metal önemli geliştirmeler; ve yeni özellikleri SceneKit ve SpriteKit.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
