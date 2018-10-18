---
title: 3B oyun oluşturmak için UrhoSharp kullanma
description: Bu belge, sahneler, bileşenleri, şekiller, kamera, Eylemler, kullanıcı girişi, ses ve diğer açıklayan UrhoSharp, genel bir bakış sağlar.
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 7d07733ebf62e6e12ccee05f9b72eaf1a74afad2
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "34784045"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>3B oyun oluşturmak için UrhoSharp kullanma

İlk oyununuzu yazmadan önce temel kavramları analytics'in istediğiniz: sahneniz ayarlama, kaynakları (Bu içeriyor resminize) yükleme ve oyununuz için basit etkileşimleri oluşturma.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>Planda, düğümleri, bileşenler ve kameraları

Sahne modeli, bileşen bazlı Sahne grafiği olarak tanımlanabilir. Ayrıca tüm Sahne temsil eden kök düğümünden başlayan bir hiyerarşi Sahne düğümlerinin Sahne oluşur. Her [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) 3B bir dönüştürme (konum, döndürme ve ölçeklendirme), bir ad, bir kimliği ek bileşenleri rastgele bir sayıdan sahiptir.  Bileşenleri, bir düğüm yaşama getirir, görsel bir temsili ekleme yapabilir ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), ses yaydıkları ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), bir çakışma sınır vb. sağlayabilir.

Sahneleri ve Kurulum düğümleri kullanarak oluşturabileceğiniz [Urho Düzenleyicisi](#UrhoEditor), veya kendi C# kod şeyler yapabilirsiniz.  Bu belgede, ekranda görünmesini öğeleri almak gerekli öğeler gösterdiği gibi biz kod kullanılarak ayarlamalar inceleyeceksiniz

Sahneniz ayarlamanın yanı sıra kurulum için gereken bir [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/), hangi kullanıcıya gösterilen belirler budur.

### <a name="setting-up-your-scene"></a>Sahneniz ayarlama

Genellikle, bu formu başlangıç yönteminizi oluşturursunuz:

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

3B nesneleri oluşturma, ses kayıttan yürütme, fizik ve komut dosyalı mantıksal güncelleştirmeleri tüm çağırarak farklı bileşenleri düğümlere oluşturarak etkin [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/).  Örneğin, düğüm ve açık bileşen şöyle ayarlayın:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Ada sahip bir düğüm yukarıda oluşturduğumuz "`DirectionalLight`" bir yönü, ancak başka hiçbir şey için ayarlayın.  Şimdi biz yukarıdaki düğümü bir ışık yayma düğümüne ekleyerek kapatabilirsiniz bir [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) , bileşeni ile `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

Oluşturulan bileşenleri `Scene` kendisini sahip özel bir rol: Sahne genelinde işlevselliği uygulamak için. Tüm diğer bileşenleri önce oluşturulmalıdır ve aşağıdakileri içerir:

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): uzamsal bölümleme uygular ve hızlandırılmış görünürlük sorgular. Bu 3B nesneleri işlenebilecek değil.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): Fizik benzetimi uygular. Bileşenleri gibi Fizik [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) veya [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) düzgün bir şekilde bu olmadan çalışmıyor.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): uygular, geometri işleme hata ayıklama.

Sıradan bileşenleri ister [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) veya [`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)
doğrudan oluşturulamaz [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), ancak bunun yerine alt düğümleri olarak.

Kitaplığı çok çeşitli bunları hayata düğümlerinizi ekleyebileceğiniz bileşenleri ile birlikte gelir: kullanıcıya görünen öğeler (modeller), ses, katı gövdeleri, çakışma şekiller, kamera, ışık kaynaklarına, parçacık vericileri ve çok daha fazlası.

### <a name="shapes"></a>Şekiller

Kolaylık, çeşitli şekiller Urho.Shapes ad alanındaki basit düğümleri olarak mevcuttur.  Bunlar kutuları Küreler, koni silindir ve düzlemleri içerir.

### <a name="camera-and-viewport"></a>Kamera ve Görünüm penceresi

Yalnızca ışığı gibi kameraları, bileşenlerdir bileşeni için bir düğüm eklemek ihtiyacınız olacak şekilde yapıldığını şöyle:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

Bir kamera, bu, oluşturduğunuz ve Kamera 3B dünyada koyduysanız, sonraki adıma bilgilendirmektir `Application` bu kullanmak istediğiniz kamera olduğundan, bu aşağıdaki kod ile gerçekleştirilir:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

Ve artık, oluşturma sonuçları görmeye olmalıdır.

### <a name="identification-and-scene-hierarchy"></a>Kimlik ve Sahne hiyerarşisi

Düğümler bileşenleri adları yok; bileşenler aynı düğümde içinde yalnızca tanımlanır, türü ve dizin oluşturma sırayla doldurulmuş düğümün bileşen listesinde, örneğin, alabilirsiniz [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) tanesi bileşen `lightNode` nesnesi Yukarıdaki şöyle:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Alarak tüm bileşenlerin listesini alabilirsiniz [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) döndüren özellik bir `IList<Component>` kullanabileceğiniz.

Oluşturduğunuzda, hem düğümler ve bileşenleri Sahne genel tamsayı kimliklerini almak. Bir görünümü, sorgulanabilir işlevlerini kullanarak [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) ve [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/). Bu örneğin adı temelinde Sahne düğümü sorguları özyinelemeli yapmaktan daha hızlıdır.

Bir varlık veya oyun nesnesine yerleşik kavramı yoktur; Bunun yerine programcının düğüm hiyerarşisine karar vermek için en fazla ve herhangi bir komut dosyası mantık yerleştirmek için hangi düğümleri kullanılabilir. Genellikle, ücretsiz hareket eden nesneler 3B dünyada kök düğümünün alt öğesi olarak oluşturulması. Düğümleri veya bilginiz olmaksızın bir adı kullanarak oluşturulabilir [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/). Düğüm adlarının zorlanmaz.

Bazı hiyerarşik bir birleştirme olduğunda önerilen (ve hatta gerekli bileşenleri kendi 3B dönüşümler olmadığı için), bir alt düğüm oluşturmak için.

Örneğin bir karakter, el ile bir nesne bulunduran, nesne için karakterin el bone shapemap kendi düğüm olmalıdır (aynı zamanda bir [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  Fizik istisnadır [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), olabilen offsetted ve döndürülen tek tek düğüm ile ilgili.

Unutmayın [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)ait alt düğümleri dünya türetilmiş dönüştürmeleri hesaplarken dönüştürme bir iyileştirme kullanılamıyor.%n%nÇözüm yoksayıldı, bu nedenle değiştirmenin etkisi yoktur ve (hiçbir dönüş kaynak konumu olarak bırakılmalıdır sahibi ölçeklendirme yok.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) düğümleri serbestçe yeniden üst. Buna bileşenleri her zaman bağlı ve düğümler arasında taşınabilir olmayan düğüme ait. Hem düğümler ve bileşenleri sağlar bir [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) üst inmek zorunda kalmadan, bunu gerçekleştirmek için işlevi. Düğüm kaldırıldıktan sonra bu işlevi çağırdıktan sonra güvenli işlem düğümü veya söz konusu bileşen yok.

Oluşturulacak mümkündür bir `Node` bir görünüme ait olmayan. Örneğin, yüklenen veya kaydedilmiş olabilir sahnede olduğundan taşıma ile bir kamera kullanışlıdır ardından kamera yanı sıra gerçek Sahne kaydedilmez ve Sahne yüklendiğinde edilmeyeceği. Ancak, eklenmemiş bir düğüme geometri, fizik veya komut dosyası bileşenleri oluşturma ve daha sonra bir sahneye taşıma bu bileşenlerin doğru şekilde çalışmamasına neden olacağını unutmayın.

### <a name="scene-updates"></a>Sahne güncelleştirmeleri

Güncelleştirmeleri etkin bir Sahne (varsayılan) üzerinde her ana döngü yinelemesi otomatik olarak da güncelleştirilir.  Uygulamanın [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) olay işleyicisi üzerinde çağrılır.

Düğümlerini ve bileşenlerini devre dışı bırakarak Sahne güncelleştirmeyi bırakılabileceğini, bkz: [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled).  Davranışı belirli bileşene bağlıdır, ancak ses kaynaklı bir bileşen devre dışı bırakma, sesini kapatır sırasında örneğin drawable bileşen devre dışı bırakma görünmez, kolaylaştırır. Bir düğüm devre dışıysa, tüm bileşenleri kendi etkinleştirme/devre dışı durumuna bakılmaksızın devre dışı olarak kabul edilir.

## <a name="adding-behavior-to-your-components"></a>Bileşenleriniz için davranış ekleme

Oyununuzu yapı en iyi yolu, bir aktör veya oyununuzu öğede kapsayan kendi bileşeni olmasını sağlamaktır.  Bu özellik kılar kendi içinde gelen varlıkları, davranışını görüntülemek için kullanılır.

Bir bileşenin davranış ekleme en basit yolu, kuyruk ve C# zaman uyumsuz programlama ile birleştirerek, yönergelerdir Eylemler, kullanmaktır.  Bu, çok yüksek düzeyde olmasını bileşeniniz için davranış sağlar ve neler olduğunu anlamak daha basit hale getirir.

Alternatif olarak, tam olarak bileşeninize her karede (tabanlı çerçeve davranışı bölümde anlatılmaktadır), bileşen özelliklerini güncelleştirerek ne kontrol edebilirsiniz.

### <a name="actions"></a>Eylemler

Düğümleri kolayca eylemleri kullanarak çok davranış ekleyebilirsiniz.  Eylemler çeşitli düğüm özelliklerini değiştirmek ve bir süre yürütmelerine veya bunları birkaç kez bir verilen animasyon eğri ile yineleyin.

Örneğin, bir "bulut" düğümünde sahneniz düşünün, onu şöyle Soldurma:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Farklı nesneleri kullanımını eylemi yeniden olanak tanıyan değişmez nesneleri eylemlerdir.

Ortak bir deyim tersine çevirme işlemi gerçekleştiren bir eylem oluşturmaktır:

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

Aşağıdaki örnek nesneyi sizin için üç saniye verinceye Soldurma.  Bir eylem de çalıştırabilirsiniz diğerinden sonra gibi ilk buluta taşıyın ve sonra gizleyin:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Her iki eylem aynı zamanda gerçekleşmesi için istiyorsanız, paralel eylemini kullanın ve paralel olarak istediğiniz tüm eylemleri sağlar:

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

Yukarıdaki örnekte buluta taşıyın ve aynı anda Kıs.

Bu C# kullandığınızı fark edeceksiniz await, doğrusal olarak davranışı elde etmek istediğiniz hakkında düşünmeye sağlar.

### <a name="basic-actions"></a>Temel Eylemler

İçinde UrhoSharp desteklenen eylemler şunlardır:

* Düğümleri taşıma: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* Düğümleri döndürme: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* Düğümleri ölçeklendirme: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* Düğümleri Soluklaşan: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* Tonlandırmayı: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* İnstants: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* Döngü: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

Birleşimi diğer Gelişmiş Özellikler [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) ve [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) eylemler.

### <a name="easing---controlling-the-speed-of-your-actions"></a>Hızlandırma - eylemlerinizi hızına denetleme

Hızlandırma animasyon unfold ve animasyonlarınızı çok daha eğlenceli hale getirebilir yönlendiren bir yoludur.  Varsayılan olarak eylemlerinizi bir doğrusal davranışa, örneğin sahip bir [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) eylemi çok robot taşıma sahip.  Eylemlerinizi eyleme bu gibi bir durumda davranışını değiştirmek için bir hareket hızı yavaş hareketini başlatın, hızlandırın ve yavaş sonuna ulaşana bir kayabilir ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

Bunun için bir eylem kolaylaştırıcı bir eyleme örneğin sarmalama tarafından:

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Birçok kolaylaştırıcı modları, aşağıdaki tabloda çeşitli kolaylaştırıcı türleri ve davranışları baştan zaman diliminde denetlediğiniz nesnenin değerini gösterir:

![Modları hızlandırma](using-images/easing.png "bu grafik hızlandırma çeşitli türleri ve davranışları süre boyunca denetleme, nesnenin değerini gösterir")

### <a name="using-actions-and-async-code"></a>Eylemler ve zaman uyumsuz kodu kullanma

İçinde [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) alt bileşen davranış'ınızı hazırlar ve işlevselliği için sürücüleri zaman uyumsuz bir yöntem tanıtmak.
C# kullanarak bu yöntem çağıracaktır sonra `await` programınızın, başka bir bölümünden anahtar sözcüğü ya da, `Application.Start` yöntemi veya bir kullanıcı veya Öykü noktası uygulamanızda yanıt.

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

İçinde `Launch` yukarıdaki üç eylem yöntemine başlatılacağı: robot sahneye gelir, bu eylem bir zaman 0,6 saniye boyunca düğüm konumunu değiştirir.  Bu zaman uyumsuz seçeneği olduğundan, bu çağrı, sonraki yönergesi eş zamanlı olarak gerçekleşir için `MoveRandomly`.  Bu yöntem, paralel rastgele bir konum için robot konumunu değiştirir.  Bu iki bileşik eylemleri, yeni bir konuma taşımayı gerçekleştirerek sağlanır ve özgün yukarıda bahsedilen getirin ve canlı robot kaldığı sürece bu işlemi yineleyin.  Ve daha ilgi çekici şeyler yapmak için robot aynı anda atma tutmak.  Atma 0,1 saniyede yalnızca başlar.

### <a name="frame-based-behavior-programming"></a>Çerçeve tabanlı davranışını programlama

Bileşeniniz Eylemler kullanmak yerine bir kare kare temelinde davranışını denetlemek istiyorsanız, yapmayı tercih geçersiz kılmak için olan [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) yöntemi, [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) öğesinin alt sınıfı.  Bu yöntem, çerçeve başına bir kez çağrılır ve yalnızca ReceiveSceneUpdates özelliği true olarak ayarlanmışsa çağrılır.

Aşağıdakileri nasıl oluşturulacağını gösterir. bir `Rotator` ardından düğümü döndürmek neden bir düğüme bağlı bileşeni:

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

Ve bir düğüm için bu bileşenin nasıl eklemek istersiniz:

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>Birleştirme stilleri

Başlat ve unut stili programlama için harika olan davranışı çoğunu programlama için zaman uyumsuz/eylem tabanlı modeli kullanabilirsiniz ancak bileşeninizin davranışı, ayrıca her karede update kod çalıştırmak için de ince ayar yapabilirsiniz.

Örneğin, SamplyGame gösteride bu kullanılan `Enemy` sınıfı kodlar temel davranışı kullanan eylemleri, ancak aynı zamanda düğümle yönünü ayarlayarak bileşenleri doğru kullanıcı işaret etmesini sağlar `Node.LookAt`:

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

## <a name="loading-and-saving-scenes"></a>Yükleme ve sahneler kaydetme

Planda yüklenebilir ve XML biçiminde kaydedilmiş; işlevleri [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) ve [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml). Sahne yüklendiğinde, tüm varolan içeriğin (alt düğümleri ve bileşenleri) ilk önce kaldırılır. Düğümler ve geçici ile işaretlenmiş bileşenleri `Temporary` özelliği kaydedilmeyecek. Tüm yerleşik bileşenler ve Özellikler serileştiricinin işlediği ancak özel özellikler ve alanları, bileşen sınıflarında tanımlanan işlemek akıllı değildir. Ancak, iki sanal yöntemler bu sağlar:

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) serileştirme için özel durumlar, kaydolabilecekleri

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) Burada, kaydedilen özel durumlar elde edebilirsiniz.

Genellikle, bir özel bileşene aşağıdaki gibi görünür:

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

Burada yeni nesneler dinamik olarak oluşturulması gerektiğini, yalnızca yükleme ya da tüm sahneler kaydetme oyunlar için yeterince esnektir değil. Öte yandan, karmaşık nesneler oluşturma ve kod içinde özelliklerini ayarlayarak da tedious olacaktır. Bu nedenle, aynı zamanda kendi alt düğümleri, bileşenler ve öznitelikleri içeren bir Sahne düğümü kaydetmek mümkündür. Bu, bir grup olarak daha sonra bir kolayca yüklenebilir.  Kaydedilmiş bir nesne, genellikle bir prefab da adlandırılır. Bunu yapmak için üç yolu vardır:

- Çağırarak kod [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) düğümde
- Düzenleyici'de hiyerarşi penceresinde düğüm ve seçerek "düğümü olarak kaydetme" "Dosya" menüsünden.
- "Düğüm" komutunu kullanarak `AssetImporter`, hangi Sahne düğüm hiyerarşisine kaydetmek ve herhangi bir model bulunan giriş varlığı (örn.) Collada dosya)

Bir sahneye kaydedilmiş bir düğüm oluşturmak için çağrı [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml). Düğümü alt Sahne öğesi olarak oluşturulur, ancak ücretsiz, daha sonra yeniden üst. Konum ve döndürme düğümü yerleştirmek için belirtilmesi gerekir. Aşağıdaki kod örneği bir prefab gösterilmektedir `Ninja.xm` Sahne istenen konum ve döndürme için:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>Olaylar

Olay sayısı UrhoObjects yükseltmek, onları oluşturan çeşitli sınıflarda bu olayları C# olarak çıkarılır.  Ek olarak C#-tabanlı olay modeline mümkündür de kullanmak bir `SubscribeToXXX` abone olma ve aboneliği belirteci tutun sağlayacak yöntemleri daha sonra aboneliği için kullanabilirsiniz.  Eski abone olmak birçok arayanlara izin verir, ikincisi yalnızca biri izin verir ancak izin veren daha Hoş görünmesi lambda stili kullanılacak yaklaşımını ve henüz aboneliğinin kolay kaldırmaya izin fark vardır.  Bunlar birbirini dışlar.

Bir olaya abone olduğunuzda, uygun olay bağımsız değişkenleri olan bir bağımsız değişken alan bir yöntem sağlamanız gerekir.

Örneğin, nasıl bir fare düğmesini basılı olay abone budur:

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

Lambda stili:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

Bazen bu gibi durumlarda, olay kaydetmek için dönüş değeri çağrısından bildirim almayı durdurmak isteyeceksiniz `SubscribeTo` yöntemi ve aboneliği kaldırma yöntemini çağırın:

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

Olay işleyici tarafından alınan parametrenin her olay için özeldir ve olay yükü içeren bir türü kesin belirlenmiş olay bağımsız değişkenleri sınıftır.

## <a name="responding-to-user-input"></a>Kullanıcı girişine yanıt

Tuş vuruşları gibi çeşitli olayları aşağı olaya abone olma ve teslim edilen girişine yanıt abone:

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

Ancak birçok senaryoda güncelleştiriliyor ve kodunuzu buna göre güncelleştirin, anahtarların geçerli durumunu denetlemek için Sahne güncelleştirme işleyicileri istiyorsunuz.  Örneğin, aşağıdaki klavye girdisi göre kamera konumuna güncelleştirmek için kullanılabilir:

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

Kaynağı, yığın depolama alanından, başlatma veya çalışma zamanı sırasında yüklenen çoğu UrhoSharp şeyler içerir:

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) -iskelet animasyon için kullanılan
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -çeşitli grafik biçimlerini içinde depolanan görüntüler temsil eder
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -3B modelleri
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -malzemeleri modelleri oluşturmak için kullanılır.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [açıklar](http://urho3d.github.io/documentation/1.4/_particles.html) parçacık verici nasıl çalışır bkz "[Particles](#particles)" altında.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -Özel gölgelendirici
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -Ses kayıttan yürütme için bkz. "[ses](#sound)" altında.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -Malzeme işleme teknikleri
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -2B doku
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -3B dokunun
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) -Küp dokusu
- `XmlFile`

Yönetilen ve tarafından yüklenen [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) alt sistem (kullanılabilir olarak [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

Kaynakları, paket dosyalarını ve kaynak dizinleri göreli dosya yollarına göre tanımlanır. Varsayılan olarak, kaynak dizinleri altyapısı kaydeder `Data` ve `CoreData`, veya paketler `Data.pak` ve `CoreData.pak` varsa.

Bir kaynağı yüklenirken başarısız olursa, bir hata günlüğe kaydedilir ve null başvuru döndürülür.

Aşağıdaki örnek, bir kaynağın kaynak önbellekten getirilirken tipik bir yol gösterir.  Bu durumda, bir kullanıcı Arabirimi öğesi için bir doku, bunu kullanan `ResourceCache` özelliğinden `Application` sınıfı.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Kaynaklar ayrıca el ile oluşturulan ve diskten yüklü gibi kaynak önbelleğe depolanır.

Kaynak türü bellek bütçelerini ayarlanabilir: izin verilenden daha fazla bellek kaynaklarının kullanılmasına, eski kaynaklar önbellekten kullanılmıyorsa artık kaldırılır. Varsayılan olarak bellek bütçeleri sınırsız olarak ayarlanır.

### <a name="bringing-3d-models-and-images"></a>3B modeller ve görüntüleri

Urho3D dener mümkün olduğunda mevcut dosyası biçimleri kullanma ve modelleri için yalnızca gerçekten gerekli olduğu gibi özel dosya biçimlerini tanımlayın (*.mdl) ve animasyonları (*.ani). Bu tür varlıklar için bir dönüştürücü - Urho sağlar [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) fbx, dae, 3ds ve obj, vb. gibi birçok popüler 3B biçimleri kullanabilir.

De mevcuttur bir kullanışlı eklentisi Blender [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) Blender varlıklarınızı Urho3D için uygun biçimde dışarı aktarabilir.

### <a name="background-loading-of-resources"></a>Arka planda yükleme işlemini kaynakları

Normalde, aşağıdakilerden birini kullanarak kaynak istenirken `ResourceCache`'s `Get` yöntemi, bunlar için gereken tüm adımları birkaç milisaniye sürebilir hemen ana iş parçacığı içinde yüklenen (dosya diskten yüklemek, verileri ayrıştırılamadı, gerekirse GPU'ya yükleyin ) ve bu nedenle kare hızını bırakmaları neden olabilir.

Önceden hangi kaynaklara ihtiyacınız olduğunu biliyorsanız, arka plan iş parçacığında çağırarak yüklenmesini isteyebilirsiniz `BackgroundLoadResource()`. Kullanarak arka plan kaynak yüklendi olayına abone olabilirsiniz `SubscribeToResourceBackgroundLoaded` yöntemi. Yükleme gerçekten başarılı veya başarısız olduğunu bildirir. Yükleme işlemi yalnızca bir parçası olarak kaynağı bağlı bir arka plan iş parçacığı taşınmış olabilir, örneğin tamamlama GPU karşıya yükleme adımı her zaman ana iş parçacığında gerçekleştirilmesi gerekir. Bir yöntem arka plan yükleme için kuyruğa alınan bir kaynak için yükleme kaynak çağırırsanız, yükleme işlemi tamamlanana kadar ana iş parçacığı bekler unutmayın.

Zaman uyumsuz Sahne işlevselliği yüklenirken `LoadAsync()` ve `LoadAsyncXML()` Sahne içeriğin yüklenmesi için ilk önce devam kaynaklara arka plan yükleme seçeneğine sahiptir. Yalnızca belirterek Sahne değiştirmeden kaynakları yüklemek için de kullanılabilir `LoadMode.ResourcesOnly`. Bu hızlı örnek oluşturma Sahne veya nesne prefab dosyayı hazırlama sağlar.

Son olarak her karesi üzerinde harcanan maksimum süre (milisaniye cinsinden) arka plan yüklenen kaynakları ayarlanarak yapılandırılabilir tamamlama `FinishBackgroundResourcesMs` özellikte `ResourceCache`.

<a name="sound"/>

## <a name="sound"></a>Ses

Ses oyun oynama, önemli bir parçasıdır ve UrhoSharp framework oyununuzda katılımcılığı ses çalma bir yol sağlar.  Ekleyerek sesleri çalmak bir [`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/)
bileşeni bir [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) ve ardından kaynaklarınızın adlandırılmış bir dosyadan yürütülüyor.

Bu nasıl yapılır.

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>Particles

Parçacık, uygulamanız için bazı basit ve hesaplı etkileri ekleme basit bir yol sağlar.  PEX biçiminde depolanan particles gibi araçları kullanarak tüketebileceği [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/).

Parçacık bir düğüme eklenebilir bileşenlerdir.  Düğümün çağırmanız gerekir `CreateComponent<ParticleEmitter2D>` parçacık oluşturup sonra parçacık efekti 2B bir efekt, ayarlayarak yapılandırın yöntemi kaynak önbellekten yüklendi.

Örneğin, bu yöntem ulaştığında bunu miktarında bir patlama yaşanırken işlenen bazı particles göstermek için bileşeninizin çağırabilirsiniz:

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

Yukarıdaki kod, geçerli bileşeniniz için bağlı bir patlama düğüm oluşturur, bu açılımı düğümünde biz 2B parçacık verici oluşturun ve bunu etkisi özelliğini ayarlayarak yapılandırın.  İki eylem, düğüm daha küçük olacak şekilde ölçeklenen, diğeri, 0,5 saniye boyunca bu boyutta bırakır çalıştırıyoruz.  Sonra da parçacık efekti ekranından kaldırır açılımı kaldırın.

Yukarıdaki Parçacık, küre doku kullanırken şu şekilde işlenir:

![Bir küre doku ile particles](using-images/image-1.png "küre doku kullanırken yukarıdaki parçacık şu şekilde işler")

Ve blok doku kullanırsanız göründüğünü budur:

![Parçacık kutusu doku ile](using-images/image-2.png "ve bu blok doku kullanıyorsanız ne görünüyor")

## <a name="multithreading-support"></a>Çoklu iş parçacığı desteği

UrhoSharp bir tek iş parçacıklı kitaplıktır.  Bu, bir arka plan iş parçacığından UrhoSharp yöntemleri çağırmak çalışmamalısınız, ya da uygulama durumunu bozan risk ve büyük olasılıkla uygulamanızı kilitlenme anlamına gelir.

Arka planda kod çalıştırın ve ardından Urho bileşenleri ana kullanıcı arabiriminde güncelleştirmek istiyorsanız kullanabilirsiniz [`Application.InvokeOnMain(Action)`](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain)
yöntem.  Ek olarak C# await kullanın ve .NET API kod uygun iş parçacığında yürütülür emin olmak için görev.

## <a name="urhoeditor"></a>UrhoEditor

Platforma Urho Düzenleyicisi'ni indirebilirsiniz [Urho Web sitesi](http://urho3d.github.io/)yüklemeleri için Git ve en son sürümü seçin.

## <a name="copyrights"></a>Telif Hakkı

Bu belge Xamarin Inc. orijinal içerikten ancak Urho3D proje için açık kaynak belgelerinde kapsamlı bir şekilde çizer ve ekran görüntüleri Cocos2D projesi içerir.
