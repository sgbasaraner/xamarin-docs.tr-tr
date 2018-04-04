---
title: UrhoSharp giriş
description: Bu UrhoSharp kavramları kısa bir giriş sağlar
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 243498e1d5a24a0a6b8d1e911b374df61dfa6971
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="an-introduction-to-urhosharp"></a>UrhoSharp giriş

_Bu UrhoSharp kavramları kısa bir giriş sağlar_

![](introduction-images/urhosharp-icon.png "Xamarin ve .NET geliştiricileri için güçlü bir 3B oyun altyapısı UrhoSharp.")

UrhoSharp Xamarin ve .NET geliştiricileri için güçlü bir 3B oyun alt yapısıdır.  Apple'nın SceneKit ve SpriteKit içinde Ruhu benzer ve fizik dahil, gezinti, ağ ve çok daha hala platform olan while.

Bir .NET bağlaması olan [Urho3D](http://urho3d.github.io/) altyapısı ve geliştiricilerin Android, iOS Hedef platformlar arası kod yazmanıza olanak tanır, Windows ve Mac aynı codebase OpenGL ve Direct3D sistemlere hale getirebilir.

UrhoSharp kutusu dışında işlevlerin çok oyun altyapısıyla verilmiştir:

 - Güçlü 3B grafik oluşturma
- [Fizik benzetimi](https://developer.xamarin.com/api/namespace/Urho.Physics/) (madde işareti kitaplığı kullanarak)
- [Sahne işleme](https://developer.xamarin.com/api/type/Urho.Scene/)
- Await/zaman uyumsuz desteği
- [Kolay Eylemler API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [3B görüntülerin 2B tümleştirilmesi](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [Yazı tipi işlemesi FreeType ile](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [İstemci ve sunucu ağı yetenekleri](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [Çok çeşitli varlıklar alma](https://developer.xamarin.com/api/namespace/Urho.Resources/) (ile açık varlıkları kitaplığı)
- [Gezinti kafes ve pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (yeniden Detour kullanarak)
- [Çakışma algılaması için dışbükey hull oluşturma](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (StanHull kullanarak)
- [Ses çalma](https://developer.xamarin.com/api/namespace/Urho.Audio/) (ile **libvorbis**)

# <a name="getting-started"></a>Başlarken

UrhoSharp olarak dağıtılmış rahat bir [NuGet paketi](https://www.nuget.org/) ve Windows, Mac, Android veya iOS hedefleyen C# veya F # projelerinize eklenebilir.  NuGet hem programınızı çalıştırmak için gereken kitaplıklar yanı sıra motoru tarafından kullanılan temel varlıklar (CoreData) ile birlikte gelir.

## <a name="urho-as-a-portable-class-library"></a>Taşınabilir sınıf kitaplığı olarak Urho

Urho paket platforma özgü projeden veya tüm kodunuzu tüm platformlarda yeniden olanak tanıyan bir taşınabilir sınıf kitaplığı proje tüketilebilir.  Bu tüm her platformda yapmak zorunda platform belirli giriş noktası yazın ve ardından Denetim paylaşılan oyun kodunuzu aktarmak için anlamına gelir.

## <a name="samples"></a>Örnekler

Mac için Visual Studio veya Visual Studio'da örnek çözümden açarak Urho özellikleri hakkında bilgi sahibi alabilirsiniz:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Varsayılan çözüm iOS, Android için projeleri içeren Windows ve Mac  Biz bu çözüm küçük platform belirli Başlatıcısı sahibiz ve tüm örnek kod ve oyun kod yaşadığı bir taşınabilir Sınıf Kitaplığı'nda, böylece kodu yeniden kullanma tüm platformlarda en üst düzeye çıkarmak nasıl çalıştığını gösteren yapılandırılmış.

Başvurun [Urho ve bilgisayarınızı Platform](~/graphics-games/urhosharp/platform/index.md) kendi çözümleri oluşturma hakkında daha fazla bilgi için.

Tüm örnekleri ortak kullanıcı arabirimi öğelerinin sahip olduğundan, örnekleri bu dosyadaki temel kurulum soyutlanır:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Bu temel bazı tuş vuruşları işleyen bir örnek temel sınıf sağlar ve kamera dokunma olayları, ayarlar, temel kullanıcı arabirimi öğeleri ve bu verir showcased belirli işlevselliği odaklanmak her bir örnek sağlar.

Aşağıdaki örnek, altyapısı yapabileceğini nedir gösterir:

- [Samply oyun](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) ShootySkies basit bir kopyası.

Diğer örnekleri tek tek her örnek özelliklerini gösterirken.

# <a name="basic-structure"></a>Basit Yapı

Oyununuzu alt sınıf gereken [ `Application` ](https://developer.xamarin.com/api/type/Urho.Application/) sınıfı, burada oyununuzu Kurulum budur (üzerinde [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) yöntemi) ve oyununuzu başlatın (içinde [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) yöntemi).  Ardından, ana kullanıcı arabiriminizi oluşturun.  Bir 3B Sahne, bazı kullanıcı Arabirimi öğeleri ve basit bir davranış eklemeyi kurulumu için API'ler gösterir küçük bir örnek size rehberlik olacak.

```csharp
class MySample : Application {
    protected override void Start ()
    {
        CreateScene ();
        Input.KeyDown += (args) => {
            if (args.Key == Key.Esc) Exit ();
        };
    }

    async void CreateScene()
    {
        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from MySample",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

        // Create a top-level scene, must add the Octree
    // to visualize any 3D content.
        var scene = new Scene();
        scene.CreateComponent<Octree>();
        // Box
        Node boxNode = scene.CreateChild();
        boxNode.Position = new Vector3(0, 0, 5);
        boxNode.Rotation = new Quaternion(60, 0, 30);
        boxNode.SetScale(0f);
        StaticModel modelObject = boxNode.CreateComponent<StaticModel>();
        modelObject.Model = ResourceCache.GetModel("Models/Box.mdl");
        // Light
        Node lightNode = scene.CreateChild(name: "light");
        lightNode.SetDirection(new Vector3(0.6f, -1.0f, 0.8f));
        lightNode.CreateComponent<Light>();
        // Camera
        Node cameraNode = scene.CreateChild(name: "camera");
        Camera camera = cameraNode.CreateComponent<Camera>();
        // Viewport
        Renderer.SetViewport(0, new Viewport(scene, camera, null));
        // Perform some actions
        await boxNode.RunActionsAsync(
            new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
        await boxNode.RunActionsAsync(
            new RepeatForever(new RotateBy(duration: 1,
                deltaAngleX: 90, deltaAngleY: 0, deltaAngleZ: 0)));
     }
}
```

Bu uygulama çalıştırırsanız, hızlı bir şekilde çalışma zamanı hakkında şikayetçi olduğu keşfeder varlıklarınızı yok.  Projenizdeki özel dizin adı "Data" ile başlayan bir hiyerarşi oluşturmak için yapmanız gerekenler olduğunu ve bu içinde programınıza başvuru varlıklar yerleştireceğiniz.  Sonra ayarlamalısınız her varlık "Kopyalama için çıktı dizinine" "Kopyalama, yeni" öğesi özelliklerini, verilerinizi var olduğundan emin olun.

Bize burada neler olduğunu açıklayın.

Uygulamanızı başlatmak için bu gibi uygulama sınıfının yeni bir örneğini oluşturarak ve ardından altyapısı başlatma işlevini çağırın:

    new MySample().Run();

Çalışma zamanı çağıracağı `Setup` ve `Start` sizin için yöntemleri.  Geçersiz kılarsanız `Setup` altyapısı parametreleri (Bu örnekte gösterme) yapılandırabilirsiniz.

Geçersiz kılmanız gerekir `Start` bu oyununuzu başlatacak şekilde.  Bu yöntem, varlıklarınızı yük, olay işleyicileri bağlanmak, Sahne Kurulum ve istediğiniz herhangi bir eylem başlatmak.  Örneğimizde, her iki kullanıcı yanı sıra 3B Sahne ayarlamak için göstermek için kullanıcı Arabirimi biraz oluşturuyoruz.

Aşağıdaki kod parçası, UI çerçevesi metin öğesi oluşturun ve uygulamanıza eklemek için kullanır:

        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from UrhoSharp",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

UI çerçevesi çok basit bir oyun kullanıcı arabirimi sağlamak için yoktur ve bu yeni düğüm ekleyerek çalışır [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) düğümü.

Bizim örnek kurulumları ikinci bölümü ana görünüm.  Bu, çeşitli ışık, kamera ve bir görünüm penceresinin ekleme ekranında bir 3B kutunun oluşturma bir 3B Sahne, oluşturma adımları içerir.  Bu bölümde daha ayrıntılı ele alınan dizelerle "[Sahne, düğümler, bileşenleri ve kameralar](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)"

Bizim örnek üçüncü parçası birkaç Eylemler tetikler.  Eylemlerdir'ın belirli bir efekt açıklayan ve oluşturulduktan sonra bunlar tarif yürütülebilir isteğe bağlı bir düğüm tarafından çağırarak [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) yöntemi bir `Node`.

İlk eylem Sıçrama efekti kutusuyla ölçeklendirir ve ikinci bir kutunun sonsuza kadar döndürür:

    await boxNode.RunActionsAsync(
        new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));

Yukarıdaki nasıl oluşturuyoruz ilk eylem gösteren bir [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) , bu eylemdir yalnızca ikinci bir değeri bir doğrultusunda için bir düğümü scale özelliği ölçeklendirmek istediğiniz belirten bir tarif.  Bu eylem sırayla hareket hızı bir eylem kaydırılan [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) eylem.  Hareket hızı Eylemler doğrusal bir eylemin yürütülmesi deforme ve efekti uygulamak, bu durumda geçirmek çıkış etkisi sağlar.
Bu nedenle bizim tarif olarak yazılmış:

    var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));

Tarif oluşturulduktan sonra biz tarif yürütün:

    await boxNode.RunActionsAsync (recipe)

Bekleme belirten eylemi tamamladığında bu satırı sonra yürütmeye devam etmek istiyor musunuz.  Eylem tamamlandıktan sonra biz ikinci animasyon tetikler.

[Kullanarak UrhoSharp](~/graphics-games/urhosharp/using.md) belge Urho ve oyun oluşturmak için kodunuzu nasıl kavramları daha ayrıntılı olarak inceler.

# <a name="copyrights"></a>Telif Hakkı

Bu belge Xamarin Inc orijinal içerikten ancak Urho3D projesi için açık kaynak belgelerindeki yaygın çizer ve ekran görüntüleri Cocos2D projeden içerir.



## <a name="related-links"></a>İlgili bağlantılar

- [Planet Earth çalışma kitabı](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Koordinatları çalışma kitabı keşfetme](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
