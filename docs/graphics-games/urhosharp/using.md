---
title: 3B oyun oluşturmak için UrhoSharp kullanma
description: Bu belge, planda, bileşenleri, şekiller, kamera, Eylemler, kullanıcı girişi, ses ve daha fazla açıklayan UrhoSharp, genel bir bakış sağlar.
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: eb1e93e47528e801da08f402f452e0e8ce5014d8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784045"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>3B oyun oluşturmak için UrhoSharp kullanma

Temel kavramları familiarized istediğiniz ilk oyununuzu yazmadan önce:, Sahne ayarlama, (Bu içeren resminiz) kaynakların nasıl yüklendiği ve oyununuzu için basit etkileşimler oluşturma.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>Planda, düğümler, bileşenleri ve kameralar

Sahne modeli bir bileşen tabanlı Sahne grafik tanımlanabilir. Ayrıca tüm Sahne temsil eder kök düğümden başlayarak bir hiyerarşinin Sahne düğümlerinin Sahne oluşur. Her [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) 3B bir dönüştürme (konum, döndürme ve ölçek), bir ad, bir kimlik artı rastgele sayıda bileşeni vardır.  Bileşenleri yaşama bir düğüm Getir, görsel bir ekleme yapabilir ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), ses yayma ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), bir çakışma sınır vb. sağlayabilir.

Planda ve Kurulum düğümleri kullanarak oluşturabileceğiniz [Urho Düzenleyicisi](#UrhoEditor), veya, C# kodundan şeyler yapabilir.  Bu belgede biz ekranınızda göstermeyi işlerinizi için gereken öğeleri gösterdiği gibi kodu kullanarak yukarı ayarlamalar inceleyeceksiniz

, Sahne ayarlamaya ek olarak, kurulum için gereken bir [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/), kullanıcıya gösterilen ne belirler budur.

### <a name="setting-up-your-scene"></a>Sahne ayarlama

Genellikle, bu formu başlangıç yönteminizi oluşturacak:

```csharp
var scene = new Scene ();
// Create the Octree component to the scene. This is required before
// adding any drawable components, or else nothing will show up. The
// default octree volume will be from -1000, -1000, -1000) to
//(1000, 1000, 1000) in world coordinates; it is also legal to place
// objects outside the volume but their visibility can then not be
// checked in a hierarchically optimizing manner
scene.CreateComponent<Octree> ();
// Create a child scene node (at world origin) and a StaticModel
// component into it. Set the StaticModel to show a simple plane mesh
// with a "stone" material. Note that naming the scene nodes is
// optional. Scale the scene node larger (100 x 100 world units)
var planeNode = scene.CreateChild("Plane");
planeNode.Scale = new Vector3 (100, 1, 100);
var planeObject = planeNode.CreateComponent<StaticModel> ();
planeObject.Model = ResourceCache.GetModel ("Models/Plane.mdl");
planeObject.SetMaterial(ResourceCache.GetMaterial("Materials/StoneTiled.xml"));
```

### <a name="components"></a>Bileşenler

3B nesneleri oluşturma, ses kayıttan yürütme, fizik ve komut dosyalı mantığı güncelleştirmeleri tüm düğümlerin içine çağırarak farklı bileşenleri oluşturarak etkin [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/).  Örneğin, düğüm ve bu gibi açık bileşeni Kurulum:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Ada sahip bir düğüm üzerinde oluşturduk "`DirectionalLight`" ve bir yönü, ancak hiçbir şey için ayarlayın.  Şimdi, biz yukarıdaki düğümü açık yayma bir düğümüne ekleyerek kapatabilirsiniz bir [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) , bileşen ile `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

Oluşturulan bileşenleri `Scene` kendisini sahip özel bir rol: Sahne genelinde işlevselliği uygulamak için. Bunlar tüm bileşenlerle önce oluşturulmalıdır ve aşağıdakileri içerir:

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): uzamsal bölümleme uygular ve görünürlük sorguları hızlandırılmış. Nesneleri bu 3B işlenemiyor.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): Fizik benzetimi uygular. Bileşenleri gibi Fizik [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) veya [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) düzgün olmadan çalışmıyor.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): Implements hata ayıklama geometri işleme.

Sıradan bileşenleri ister [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) veya [ `StaticModel` ](https://developer.xamarin.com/api/type/Urho.StaticModel) doğrudan oluşturulmaması gerektiğini [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), ancak bunun yerine alt düğümleri içine.

Çok çeşitli düğümleriniz hayata geçirin ekleyebilirsiniz Bileşenleri Kitaplığı birlikte: kullanıcıya görünen öğeleri (modellerin), ses, katı gövdeleri, çakışma şekiller, kamera, açık kaynakları, parçacık vericileri ve çok daha fazlasını.

### <a name="shapes"></a>Şekiller

Kolaylık, çeşitli şekiller Urho.Shapes ad alanında basit düğümleri olarak kullanılabilir.  Bunlar, kutuları, Küreler, koniler, silindir ve düzlemleri içerir.

### <a name="camera-and-viewport"></a>Kamera ve Görünüm penceresi

Yalnızca açık gibi kameralar, bileşenleridir bileşeni için bir düğüm eklemek gerekmez, bu yapılır şöyle:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

Kamera bu ile oluşturduğunuz ve 3B dünyanın kamera yerleştirdiğiniz, bildirmek için sonraki adımdır `Application` bu kullanmak istediğiniz kamera olduğundan, bu aşağıdaki kod ile gerçekleştirilir:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

Ve şimdi oluşturduğunuzu sonuçlarını görüyor olmalısınız.

### <a name="identification-and-scene-hierarchy"></a>Tanımlama ve Sahne hiyerarşisi

Düğümlerin bileşenleri adları yoktur; bileşenler aynı düğümde içinde yalnızca tanımlanır, türü ve dizin oluşturma sırayla girilir düğümün bileşen listesinde, örneğin, alabilirsiniz [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) dışı bileşen `lightNode` nesnesi Yukarıdaki şöyle:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Alarak tüm bileşenlerin listesini alabilirsiniz [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) döndüren özelliğinin bir `IList<Component>` kullanabileceğiniz.

Oluşturduğunuzda, düğümleri ve bileşenleri Sahne genel tamsayı kimlikleri alın. Sahne bunlar sorgulanabilir işlevlerini kullanarak [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) ve [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/). Bu özyinelemeli ad tabanlı Sahne düğümü sorguları örneğin yapmaktan daha hızlıdır.

Bir varlık veya bir oyun nesnesi yerleşik kavramı yoktur; Bunun yerine düğümü hiyerarşi karar vermek için programcı kadar ve herhangi bir komut dosyası mantık yerleştirmek için hangi düğümlerin de sağlanır. Genellikle, 3B dünyanın nesneleri serbest taşıma kök düğümünün bir alt olarak oluşturulması. Düğümleri ile ya da ad kullanmadan oluşturulabilir [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/). Düğüm adı benzersizliğini zorlanmaz.

Bazı hiyerarşik birleşim olduğunda önerilen (ve hatta gerekli bileşenleri kendi 3B dönüşümler olmadığından), bir alt düğüm oluşturabilirsiniz.

Örneğin bir karakter nesneyi kendi el ile bulunduran, nesneyi karakterin el kemiğe üst öğe kendi düğüm olması gerekir (aynı zamanda bir [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  Fizik istisnadır [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), olabilen offsetted ve tek tek düğüm bağlantılı olarak Döndürülmüş.

Unutmayın [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)ait alt düğümleri türetilmiş world dönüşümler hesaplanırken dönüştürme bir iyileştirme var gözardı, böylece değiştirmenin etkisi yoktur ve (kaynak, hiçbir döndürme konumunda olduğu gibi bırakılmalıdır sahibi hiçbir ölçeklendirme.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) düğümleri ücretsiz olarak yeniden üst. Buna karşılık bileşenleri her zaman bağlı ve düğümler arasında adıyla taşınmaz düğüme ait olmalıdır. Düğümlerini ve bileşenlerini sağlayan bir [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) üst gitmek zorunda kalmadan bunu yapmanın işlevi. Düğüm kaldırıldığında, hiçbir işlem düğüme veya söz konusu bileşeni o işlevi çağırdıktan sonra güvenlidir.

Oluşturmak mümkündür bir `Node` bir Sahne ait olmayan. Örneğin, yüklenen olabilir veya kaydedilmiş bir görünüm olduğundan taşıma kamera ile kullanışlıdır ardından kamera birlikte gerçek Sahne kaydedilmeyecek ve Sahne yüklendiğinde yok edilmeyecek. Ancak, geometri, fizik veya komut dosyası bileşenleri eklenmemiş bir düğüme oluşturma ve daha sonra Sahne taşıma bu bileşenlerin doğru çalışmamasına neden olur unutmayın.

### <a name="scene-updates"></a>Sahne güncelleştirmeleri

Güncelleştirmeleri etkin bir Sahne (varsayılan) her ana döngü tekrarında otomatik olarak da güncelleştirilir.  Uygulamanın [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) olay işleyicisi üzerinde çağrılır.

Düğümler ve bileşenleri devre dışı bırakarak Sahne Update'ten bırakılabileceğini bkz [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled).  Davranış belirli bileşene bağlıdır, ancak ses kaynak bileşenini devre dışı bırakma, sesini kapatır sırada örneğin drawable bileşen devre dışı bırakma de görünmez, kolaylaştırır. Bir düğüm devre dışıysa, tüm bileşenleri kendi etkinleştir/devre dışı durumuna bakılmaksızın devre dışı olarak kabul edilir.

## <a name="adding-behavior-to-your-components"></a>Davranış bileşenlerinizin ekleme

En iyi yolu oyununuzu yapısı aktör veya oyununuzu öğede kapsülleyen kendi bileşen olmasını sağlamaktır.  Bu özellik kılar kendi içinde davranışını görüntülemek için kullanılan varlıklar gelen.

Bir bileşenin davranışı ekleme en basit yolu sıraya ve C# zaman uyumsuz programlama ile birleştirin yönergelerdir Eylemler kullanmaktır.  Bu, çok yüksek düzey bileşeniniz için davranış sağlar ve neler olduğunu anlamak daha basit hale getirir.

Alternatif olarak, tam olarak bileşeniniz için bileşen özelliklerinizi (tabanlı çerçeve davranışı bölümünde açıklanmıştır) her çerçeve güncelleştirerek olanlar kontrol edebilirsiniz.

### <a name="actions"></a>Eylemler

Davranış kolayca eylemleri kullanarak çok düğümlerine ekleyebilirsiniz.  Eylemler çeşitli düğüm özelliklerini değiştirmek ve bir süre boyunca yürütmek veya birkaç kez verilen animasyon eğri ile yineleyin.

Örneğin, bir "bulut" düğümünde, Sahne düşünün, onu şöyle silinerek:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Eylemler için farklı nesneleri yürüten eylem yeniden olanak tanıyan değişmez, nesneleridir.

Ortak bir deyim tersine çevirme işlemi gerçekleştiren bir eylem oluşturmaktır:

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

Aşağıdaki örnek nesne sizin için üç saniye döneminde silinerek.  Bir eylem de çalıştırabilirsiniz başka sonra Örneğin, ilk buluta taşıyın ve sonra gizleyin:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Her iki Eylemler aynı anda gerçekleşmesi istiyorsanız paralel eylemini kullanın ve paralel olarak yapılan istediğiniz tüm eylemleri sağlayabilir:

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

Yukarıdaki örnekte buluta taşıyın ve aynı anda Kıs.

Bu C# kullandığınızı fark edeceksiniz await, doğrusal olarak davranışı elde etmek istediğiniz hakkında düşünmeye sağlar.

### <a name="basic-actions"></a>Temel eylemleri

İçinde UrhoSharp desteklenen eylemler şunlardır:

* Düğümleri taşıma: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* Düğümleri döndürme: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* Düğümü ölçeklendirme: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* Düğümleri Soluklaşan: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* Tonlamak: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* İnstants: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* Döngü: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

Diğer gelişmiş özellikleri birleşimi içerir [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) ve [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) eylemler.

### <a name="easing---controlling-the-speed-of-your-actions"></a>Kolaylaştırma - eylemlerinizi hızına denetleme

Kolaylaştırma animasyon unfold ve, animasyonları çok daha fazla eğlenceli hale getirebilir biçimini yönlendiren bir yoludur.  Varsayılan olarak, Eylemler doğrusal bir davranış örneğin sahip bir [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) eylem çok robot taşıma sahip olabilir.  Eylemleriniz eyleme bu gibi bir durumda davranışını değiştirmek için bir hareket hızı yavaş taşımayı başlatmak, hızlandırmak ve yavaş sonuna ulaşana bir kayabilir ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

Varolan eylem bir hareket hızı eyleme örneğin kaydırma tarafından yapın:

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Birçok hareket hızı modu vardır, aşağıdaki tabloda çeşitli hareket hızı türleri ve davranışları baştan zaman diliminde denetlediğiniz nesnenin değerini gösterir:

![Modları kolaylaştırma](using-images/easing.png "bu grafik çeşitli hareket hızı türleri ve davranışları süre boyunca denetleme bunlar nesnenin değerini gösterir")

### <a name="using-actions-and-async-code"></a>Eylemler ve zaman uyumsuz kod kullanma

İçinde [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) alt bileşen davranışını hazırlar ve işlevselliği için sürücüleri bir zaman uyumsuz yöntem tanıtmak.
C# kullanarak bu yöntem çağıracaktır sonra `await` programınızın, başka bir bölümünden anahtar sözcüğü ya da, `Application.Start` yöntemi veya bir kullanıcı veya Öykü noktasına uygulamanızda yanıt.

Örneğin:

```csharp
class Robot : Component {
    public bool IsAlive;
    async void Launch ()
    {
        // Dress up our robot
        var cache = Application.ResourceCache;
        var model = node.CreateComponent<StaticModel>();
        model.Model = cache.GetModel ("robot.mdl"));
        model.SetMaterial (cache.GetMaterial ("robot.xml"));
        Node.SetScale (1);

        // Bring the robot into our scene
        await Node.RunActionsAsync(
            new MoveBy(duration: 0.6f, position: new Vector3(0, -2, 0)));
        // Make him move around to avoid the user fire
        MoveRandomly(minX: 1, maxX: 2, minY: -3, maxY: 3, duration: 1.5f);
        // And simultaneously have him shoot at the user
        StartShooting();
    }

    protected async void MoveRandomly (float minX, float maxX,
                                       float minY, float maxY,
                       float duration)
    {
        while (IsAlive){
            var moveAction = new MoveBy(duration,
                new Vector3(RandomHelper.NextRandom(minX, maxX),
                            RandomHelper.NextRandom(minY, maxY), 0));
            await Node.RunActionsAsync(moveAction, moveAction.Reverse());
        }
    }
    protected async void StartShooting()
    {
        while (IsAlive && Node.Components.Count > 0){
            foreach (var weapon in Node.Components.OfType<Weapon>()){
                await weapon.FireAsync(false);
                if (!IsAlive)
                    return;
            }
            await Node.RunActionsAsync(new DelayTime(0.1f));
        }
    }
}
```

İçinde `Launch` yöntemi üç eylem yukarıda başlatılacağı: Sahne robot gelir, bu eylem 0,6 saniye döneminde düğüm konumunu değiştirir.  Bu bir zaman uyumsuz seçenek olduğuna göre bu çağrı olan sonraki yönerge olarak eşzamanlı olarak gerçekleşecek şekilde `MoveRandomly`.  Bu yöntem rastgele bir konuma paralel robot konumunu değiştirir.  Bu iki bileşik Eylemler, yeni bir konuma taşımayı gerçekleştirerek sağlanır ve asıl geri dönerseniz getirin ve robot etkin kaldığı sürece bu işlemi yineleyin.  Ve şeyleri daha ilginç hale getirmek için robot aynı anda çekim tutmak.  Çekim 0,1 saniyede yalnızca başlar.

### <a name="frame-based-behavior-programming"></a>Çerçeve tabanlı davranış programlama

Bileşeniniz Eylemler kullanmak yerine kare kare temelinde davranışını denetlemek istiyorsanız, yapmayı tercih geçersiz kılmaktır [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) yöntemi, [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) bir alt kümesi.  Bu yöntem, çerçeve başına bir kez çağrılır ve yalnızca ReceiveSceneUpdates özelliği true olarak ayarlarsanız çağrılır.

Aşağıdaki nasıl oluşturulacağını gösterir bir `Rotator` sonra düğümüne döndürmek için neden olan bir düğüme bağlı bileşen:

```csharp
class Rotator : Component {
    public Rotator()
    {
        ReceiveSceneUpdates = true;
    }
    public Vector3 RotationSpeed { get; set; }
    protected override void OnUpdate(float timeStep)
    {
        Node.Rotate(new Quaternion(
            RotationSpeed.X * timeStep,
            RotationSpeed.Y * timeStep,
            RotationSpeed.Z * timeStep),
            TransformSpace.Local);
    }
}
```

Ve bu düğüm için bu bileşeni nasıl eklemek istersiniz:

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>Birleştirme stilleri

Yangın ve unut stilini programlama için harikadır davranışı çoğunu programlama için zaman uyumsuz/eylem tabanlı modelini kullanabilirsiniz, ancak bazı kodunun her çerçevesi de çalıştırmak için bileşenin davranışını da yapılandırarak ince ayar.

Örneğin, SamplyGame gösteride bu kullanılan `Enemy` sınıfı kodlar temel davranışı kullanır Eylemler, ancak aynı zamanda düğümle yönünü ayarlayarak bileşenleri doğru kullanıcı işaret etmesini sağlar `Node.LookAt`:

```csharp
    protected override void OnUpdate(SceneUpdateEventArgs args)
    {
        Node.LookAt(
            new Vector3(0, -3, 0),
            new Vector3(0, 1, -1),
            TransformSpace.World);
        base.OnUpdate(args);
    }
```

## <a name="loading-and-saving-scenes"></a>Yükleme ve planda kaydetme

Planda yüklenir ve XML biçiminde kaydedilir; İşlevler bkz [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) ve [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml). Bir Sahne yüklendiğinde, tüm var olan içeriğin (alt düğümleri ve bileşenleri) ilk kaldırılır. Düğümler ve geçici ile işaretlenmiş bileşenleri `Temporary` özelliği kaydedilmeyecek. Tüm yerleşik bileşenleri ve özellikleri serileştiricinin işlediği ancak özel özellikler ve bileşen sınıflarında tanımlanan alanları işlemek akıllı değil. Ancak, iki sanal yöntemler bu sağlar:

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) serileştirme için özel durumları, kaydolabilecekleri

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) Burada, kaydedilmiş özel durumlar elde edebilirsiniz.

Genellikle, özel bir bileşen aşağıdaki gibi görünür:

```csharp
class MyComponent : Component {
    // Constructor needed for deserialization
    public MyComponent(IntPtr handle) : base(handle) { }
    public MyComponent() { }
    // user defined properties (managed state):
    public Quaternion MyRotation { get; set; }
    public string MyName { get; set; }

    public override void OnSerialize(IComponentSerializer ser)
    {
        // register our properties with their names as keys using nameof()
        ser.Serialize(nameof(MyRotation), MyRotation);
        ser.Serialize(nameof(MyName), MyName);
    }

    public override void OnDeserialize(IComponentDeserializer des)
    {
        MyRotation = des.Deserialize<Quaternion>(nameof(MyRotation));
        MyName = des.Deserialize<string>(nameof(MyName));
    }
    // called when the component is attached to some node
    public override void OnAttachedToNode()
    {
        var node = this.Node;
    }
}
```

### <a name="object-prefabs"></a>Nesne Prefabs

Burada dinamik olarak oluşturulacak yeni nesneler gerekir, yalnızca yüklenirken veya tüm planda kaydetme oyunlar için yeterince esnektir değil. Diğer taraftan, karmaşık nesneleri oluşturma ve bunların özelliklerini ayarlama kodda de can sıkıcı olur. Bu nedenle, ayrıca kendi alt düğümleri, bileşenleri ve öznitelikleri içeren bir Sahne düğümü kaydetmek mümkündür. Bu, bir grup olarak daha sonra kolayca yüklenebilir.  Kaydedilmiş bir nesne, genellikle bir prefab da adlandırılır. Bunu yapmak için üç yolu vardır:

- Çağırarak kodda [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) düğümde
- Düzenleyicisi'nde hiyerarşi penceresinde düğümü ve seçerek "düğümü olarak Kaydet" "Dosya" menüsünden.
- "Düğümü" komutunu kullanarak `AssetImporter`, hangi Sahne düğümü hiyerarşi Kaydet ve tüm modelleri bulunan giriş varlığı (ör.) bir Collada dosyası)

Kaydedilen düğümü Sahne örneği oluşturmak için çağrı [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml). Düğüm alt Sahne öğesi olarak oluşturulur ancak serbestçe bundan sonra yeniden üst. Konum ve düğüm yerleştirmek için dönüş belirtilmesi gerekir. Aşağıdaki kod bir prefab örneği gösterilmiştir `Ninja.xm` Sahne istenen konuma ve döndürme için:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>Olaylar

Olay sayısı UrhoObjects yükseltmek, bu C# olayları olarak bunları oluşturan çeşitli sınıflarında ortaya çıkmış.  Ek C#-tabanlı olay modeli, bu da kullanmak mümkün bir `SubscribeToXXX` abone olma ve aboneliği belirteci tutmak sağlayacak yöntemleri daha sonra aboneliği için kullanabilirsiniz.  Fark eski abone olmak birçok arayanlara izin verir, ikincisi yalnızca biri sağlar ancak izin veren daha Hoş görünmesi lambda stili kullanılacak yaklaşmak ve henüz, aboneliğin kolay kaldırmaya izin vardır.  Bunlar karşılıklı olarak birbirini dışlar.

Bir olaya abone olduğunuzda, bir bağımsız değişken uygun olay bağımsız değişkenlere sahip bir yöntem sağlamanız gerekir.

Örneğin, nasıl bir fare düğmesini basılı olay abone şudur:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += HandleMouseButtonDown;
}

void HandleMouseButtonDown(MouseButtonDownEventArgs args)
{
    Console.WriteLine ("button pressed");
}
```

Lambda stiliyle:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

Bazen bu gibi durumlarda, olay kaydetmek için dönüş değeri için çağrısından bildirimleri almayı durdurmasını isteyeceksiniz `SubscribeTo` yöntemi ve aboneliği kaldırma yöntemini çağırma:

```csharp
Subscription mouseSub;

public void override Start ()
{
    mouseSub = UI.SubscribeToMouseButtonDown (args => {
    Console.WriteLine ("button pressed");
      mouseSub.Unsubscribe ();
    };
}
```

Olay işleyici tarafından alınan her olay için belirli olacaktır ve olay yükü içeren kesin türü belirtilmiş olay bağımsız değişkenleri sınıfı parametresidir.

## <a name="responding-to-user-input"></a>Kullanıcı girişine yanıt

Tuş vuruşları gibi çeşitli olayları aşağı olaya abone olmak ve teslim edilmesini girişine yanıt abone:

```csharp
Start ()
{
    UI.KeyDown += HandleKeyDown;
}

void HandleKeyDown (KeyDownEventArgs arg)
{
     switch (arg.Key){
     case Key.Esc: Engine.Exit (); return;
}
```

Ancak birçok senaryoda güncelleştirilmekte olan ve kodunuzu güncellemenize anahtarları geçerli durumunu denetlemek için Sahne güncelleştirme işleyicileri istiyor.  Örneğin, aşağıdaki klavye girdisi göre kamera konumuna güncelleştirmek için kullanılabilir:

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX * moveSpeed * timeStep, TransformSpace.Local);
}
```

## <a name="resources-assets"></a>Kaynakları (varlık)

Kaynaklar yığın depolama biriminden başlatma veya çalışma zamanı sırasında yüklenen çoğu UrhoSharp şeyler şunlardır:

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) -iskelet animasyonlarını kullanılan
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -çeşitli grafik biçimlerini depolanan görüntüleri temsil eder
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -3D modelleri
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -Modelleri oluşturmak için kullanılan malzemeleri.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [açıklar](http://urho3d.github.io/documentation/1.4/_particles.html) parçacık verici nasıl çalışır bkz "[parçacık](#particles)" altında.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -Özel gölgelendiriciler
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -Ses kayıttan yürütme için bkz: "[ses](#sound)" altında.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -Malzeme işleme teknikleri
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -2B doku
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -3D doku
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) -Küp doku
- `XmlFile`

Yönetilen ve tarafından yüklenen [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) alt sistem (kullanılabilir olarak [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

Kaynaklardaki kayıtlı kaynak dizini veya paket dosyalarını göreli dosya yollarına göre tanımlanır. Varsayılan olarak, kaynak dizinlerini altyapısı kaydeder `Data` ve `CoreData`, veya paketleri `Data.pak` ve `CoreData.pak` varsa.

Kaynak yükleme başarısız olursa bir hata günlüğe kaydedilir ve bir null başvuru döndürülür.

Aşağıdaki örnekte bir kaynak kaynak önbelleğinde getirme, tipik bir yolunu gösterir.  Bu durumda, bir kullanıcı Arabirimi öğesi için bir doku, bu kullanır `ResourceCache` özelliğinden `Application` sınıfı.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Kaynakları da el ile oluşturulan ve bunlar diskten yüklü olarak olsaydı kaynak önbelleğe depolanır.

Kaynak türü bellek bütçeleri ayarlanabilir: izin verilenden daha fazla bellek kaynaklarının kullanılmasına, en eski kaynak önbellekten kullanılmıyorsa artık kaldırılacak. Varsayılan olarak bellek bütçeleri sınırsız olarak ayarlanır.

### <a name="bringing-3d-models-and-images"></a>3B modelleri ve görüntüleri hale getirme

Urho3D çalıştığında varolan dosya biçimleri mümkün olduğunca kullanın ve özel dosya biçimleri modelleri için yalnızca kesinlikle gerekli gibi olduğunda tanımlamak (*.mdl) ve animasyonların (*.ani). Bu varlıklar türleri için bir dönüştürücü - Urho sağlar [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) fbx, dae, 3ds ve obj, vb. gibi birçok popüler 3B formatlar tüketebileceği.

Ayrıca vardır bir kullanışlı eklentisi Blender [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) Blender varlıklarınızı Urho3D için uygun biçimde verebilirsiniz.

### <a name="background-loading-of-resources"></a>Kaynakların arka planda yükleme

Normalde, aşağıdakilerden birini kullanarak kaynak isterken `ResourceCache`'s `Get` yöntemi, bunlar için gereken tüm adımları birkaç milisaniye sürebilir hemen ana iş parçacığında, yüklenen (diskten dosya yüklemek, verilerini ayrıştırma, gerekirse GPU'ya karşıya yükleme ) ve bu nedenle kare hızı bırakmaları neden olabilir.

Önceden hangi kaynaklara ihtiyacınız biliyorsanız, bunları çağırarak arka plan iş parçacığında yüklenecek isteyebilir `BackgroundLoadResource()`. Kullanarak kaynak arka plan yüklenen olaya abone olabilirsiniz `SubscribeToResourceBackgroundLoaded` yöntemi. Yükleme gerçekte bir başarı veya hata olup olmadığını bildirir. Kaynak, yükleme işlemi, yalnızca bir bölümünü bağlı olarak bir arka plan iş parçacığı taşınmış olabilir, örneğin tamamlama GPU karşıya yükleme adımı her zaman ana iş parçacığı içinde gerçekleşmesi gerekir. Arka planda yükleme için kuyruğa alınan bir kaynağın yöntemlerini yükleme kaynak birini çağırırsanız, yükleme işlemi tamamlanana kadar ana iş parçacığı bekler unutmayın.

Zaman uyumsuz Sahne işlevselliği yüklenirken `LoadAsync()` ve `LoadAsyncXML()` Sahne içerik yüklemek için ilk önce devam kaynaklara arka planda yükleme seçeneğine sahip. Yalnızca belirterek Sahne değiştirmeden kaynakları yüklemek için de kullanılabilir `LoadMode.ResourcesOnly`. Bu, hızlı örneklemesi Sahne veya nesne prefab dosyayı hazırlama sağlar.

Son olarak en uzun süreyi (milisaniye cinsinden) üzerinde her çerçeve harcanan yüklenen kaynakları ayarlanarak yapılandırılabilir arka plan tamamlama `FinishBackgroundResourcesMs` özelliği `ResourceCache`.

<a name="sound"/>

## <a name="sound"></a>Ses

Ses oyun oynama, önemli bir parçasıdır ve UrhoSharp framework oyunda ses çalma bir yol sağlar.  Ekleyerek sesleri çal bir [ `SoundSource` ](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/) bileşeni bir [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) ve kaynaklarınızı adlandırılmış bir dosyadan yürütülüyor.

Nasıl yapılır şudur:

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>Particles

Parçacık bazı basit ve ekonomik efektler uygulamanıza ekleme basit bir yol sağlar.  Gibi araçları kullanarak PEX biçiminde depolanan parçacık tüketebileceği [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/).

Parçacık bir düğüme eklenebilecek bileşenleridir.  Düğümün çağırmanız gerekir `CreateComponent<ParticleEmitter2D>` yöntemi parçacık oluşturmak ve ardından etkili özelliğini 2B bir etkisi, ayarlayarak parçacık yapılandırmak için kaynak önbellekten yüklenir.

Örneğin, bu yöntem, geldiğinde, bir açılımı işlenen bazı parçacık göstermek için bileşen çağırabilirsiniz:

```csharp
public async void Explode (Component target)
{
    // show a small explosion when the missile reaches an aircraft.
    var cache = Application.ResourceCache;
    var explosionNode = Scene.CreateChild();
    explosionNode.Position = target.Node.WorldPosition;
    explosionNode.SetScale(1f);
    var particle = explosionNode.CreateComponent<ParticleEmitter2D>();
    particle.Effect = cache.GetParticleEffect2D("explosion.pex");
    var scaleAction = new ScaleTo(0.5f, 0f);
    await explosionNode.RunActionsAsync(
        scaleAction, new DelayTime(0.5f));
    explosionNode.Remove();
}
```

Yukarıdaki kod geçerli bileşeniniz için bağlı bir açılımı düğümü oluşturur, bu açılımı düğüm içinde biz 2B parçacık verici oluşturur ve etkili özelliği ayarlanarak yapılandırın.  Biz, iki Eylemler, daha küçük olacak düğüme ölçeklendirilebilen, diğeri, o boyutta 0,5 saniye bırakır çalıştırın.  Ardından ayrıca ekranında parçacık etkisi kaldırır açılımı kaldırın.

Yukarıdaki parçacık şöyle bir küre doku kullanırken işler:

![Bir küre doku ile parçacık](using-images/image-1.png "küre doku kullanırken yukarıdaki parçacık şöyle işler")

Ve bu blok doku kullanırsanız ne aşağıdaki gibidir:

![Parçacık kutusunu dokusuyla](using-images/image-2.png "ve bu blok doku kullanıyorsanız göründüğünü")

## <a name="multithreading-support"></a>Çoklu iş parçacığı desteği

UrhoSharp bir tek iş parçacıklı kitaplıktır.  Bu arka plan iş parçacığı UrhoSharp içinde yöntemleri çağırmak çalışmamalısınız, veya uygulama durumu bozulmasını risk ve büyük olasılıkla, uygulamanızın kilitlenme anlamına gelir.

Arka planda bazı kodlar çalıştırmak ve ana UI Urho bileşenlerini güncelleştirmek istiyorsanız, kullanabileceğiniz [ `Application.InvokeOnMain(Action)` ](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain) yöntemi.  Ek olarak görev await C# kullanın ve .NET, kod, uygun iş parçacığı üzerinde yürütülür sağlamak için API'ler.

## <a name="urhoeditor"></a>UrhoEditor

Platformunuza Urho Düzenleyicisi'ni indirebilirsiniz [Urho Web sitesi](http://urho3d.github.io/), indirme gidin ve en son sürümünü seçin.

## <a name="copyrights"></a>Telif Hakkı

Bu belge Xamarin Inc orijinal içerikten ancak Urho3D projesi için açık kaynak belgelerindeki yaygın çizer ve ekran görüntüleri Cocos2D projeden içerir.

## <a name="related-links"></a>İlgili bağlantılar

- [Planet Earth çalışma kitabı](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Koordinatları çalışma kitabı keşfetme](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
