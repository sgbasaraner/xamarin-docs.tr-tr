---
title: UrhoSharp giriş
description: Bu belgede bir UrhoSharp uygulama ve bağlantıları çeşitli kılavuzları ve nasıl UrhoSharp kullanılacağını gösteren örnek uygulamalar için temel yapısını açıklar.
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: a3e14ebca961e828fc578035adaca5ba2a809438
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "34783561"
---
# <a name="an-introduction-to-urhosharp"></a>UrhoSharp giriş

![UrhoSharp logosu](introduction-images/urhosharp-icon.png)

UrhoSharp Xamarin ve .NET geliştiricileri için güçlü bir 3B oyun Altyapısı ' dir.  Apple'nın SceneKit ve SpriteKit Ruhu içinde benzer ve fizik dahil, gezinti, ağ ve daha fazla platformlar arası hala olan while.

Bir .NET bağlama için olan [Urho3D](http://urho3d.github.io/) altyapısı ve sayesinde geliştiriciler, Android, iOS platformlarını hedefleyebilen platformlar arası kod yazmak için Windows ve Mac ile aynı kod temeli ve OpenGL hem Direct3D sistemlerine işleyebilirsiniz.

UrhoSharp işlevsellik çok sayıda oyun ticari altyapısıdır:

- Güçlü bir 3B grafik oluşturma
- [Fizik benzetimi](https://developer.xamarin.com/api/namespace/Urho.Physics/) (madde işareti kitaplığı kullanarak)
- [Sahne işleme](https://developer.xamarin.com/api/type/Urho.Scene/)
- Async/await desteği
- [Kolay Eylemler API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [3B 2B tümleştirme](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [Yazı tipi işlemesi FreeType ile](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [İstemci ve sunucu ağı yetenekleri](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [Çok çeşitli varlıklar alma](https://developer.xamarin.com/api/namespace/Urho.Resources/) (ile açık varlıkları kitaplığı)
- [Gezinti kafes ve pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (yeniden sapma kullanarak)
- [Çakışma algılaması için dışbükey Kabuk nesil](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (StanHull kullanarak)
- [Ses çalma](https://developer.xamarin.com/api/namespace/Urho.Audio/) (ile **libvorbis**)

## <a name="getting-started"></a>Başlarken

UrhoSharp olarak dağıtılmış rahatça bir [NuGet paketini](https://www.nuget.org/) ve Windows, Mac, Android veya iOS hedefleyen C# veya F # projelerine eklenebilir.  NuGet, hem programınızı çalıştırmak için gerekli kitaplıkları, hem de altyapısı tarafından kullanılan temel varlıklar (CoreData) ile birlikte gelir.

### <a name="urho-as-a-portable-class-library"></a>Taşınabilir sınıf kitaplığı olarak Urho

Urho paket bir platforma özgü projeden veya kodunuzun tamamını tüm platformlar arasında yeniden kullanmanıza izin vererek bir taşınabilir sınıf kitaplığı projesi tarafından kullanılabilir.  Bu tüm her platformda yapmak zorunda platform belirli giriş noktanız yazıp ardından paylaşılan oyun kodunuza denetim aktarımı anlamına gelir.

### <a name="samples"></a>Örnekler

Örnek çözüm Mac için Visual Studio veya Visual Studio'da açarak denemek için Urho yeteneklerini alabilirsiniz:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Varsayılan çözüm iOS, Android için projeleri içeren Windows ve Mac  Biz bu çözüm küçük platform belirli Başlatıcısı sunuyoruz ve tüm örnek kodlar ve oyun kodu bir taşınabilir sınıf kitaplığı içinde yer alan tüm platformlar arasında kod yeniden kullanımını en üst düzeye çıkarmak nasıl bulunacağını gösteren yapısal.

Başvurun [Urho ve bilgisayarınızı Platform](~/graphics-games/urhosharp/platform/index.md) kendi çözümlerini oluşturma hakkında daha fazla bilgi için.

Tüm örnekleri ortak bir kullanıcı arabirimi öğeleri kümesini paylaşır olduğundan, örnekleri temel kurulum bu dosyadaki soyutlanır:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Bu temel bazı tuş vuruşları işleyen bir örneği temel sınıf sağlar ve bir kamera dokunma olayları, ayarlar, temel kullanıcı arabirimi öğeleri ve bu sağlayan halkla belirli işlevleri üzerinde yoğunlaşmak her bir örnek sağlar.

Aşağıdaki örnek, altyapı neler yapabileceğini gösterir:

- [Samply oyun](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) ShootySkies basit bir kopyası.

Diğer örnekler özellikler her örneğinin gösterirken.

## <a name="basic-structure"></a>Basit Yapı

Oyununuzu öğesinin alt sınıfı olmalıdır [`Application`](https://developer.xamarin.com/api/type/Urho.Application/)
sınıfı, oyununuzu Kurulum'un budur (üzerinde [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) yöntemi) ve oyununuzu başlatın (içinde [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) yöntemi).  Ardından, ana kullanıcı arabirimi oluşturun.  3B Sahne, bazı kullanıcı Arabirimi öğeleri ve basit bir davranış eklemeyi kurmak için API'leri gösteren küçük bir örnek yol için kullanacağız.

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

Bu uygulamayı çalıştırırsanız, hızlı bir şekilde çalışma zamanı'nın hakkında şikayetçi keşfedersiniz varlıklarınızı yok.  Projenize özel bir dizin adı "Veri" ile başlayan bir hiyerarşi oluşturmak için yapmanız gerekenler olduğunu ve bunun içinde programınızda başvuru varlıkları yerleştirmeniz gerekir.  Ardından ayarlamalısınız her varlık "çıktı dizinine Kopyala" için "Kopyalama, yeni" öğesi özelliklerini emin olun, verilerinizi yoktur.

Bize Buradan neler olduğunu açıklar.

Uygulamanızı başlatmak için bu gibi uygulama sınıfınıza yeni bir örneğini oluşturarak ve ardından altyapısı başlatma işlevini çağırın:

```csharp
new MySample().Run();
```

Çalışma zamanı çağıracağı `Setup` ve `Start` sizin için yöntemleri.  Kılarsanız `Setup` altyapısı parametreleri (Bu örnekte gösterme) yapılandırabilirsiniz.

Geçersiz kılmanız gerekir `Start` gibi bu oyununuzu başlatılır.  Bu yöntemde,, varlıkları yükleme, olay işleyicileri bağlayın, sahneniz Kurulum ve istediğiniz herhangi bir eylem başlatmak.  Örneğimizde, hem kullanıcı hem de bir 3B Sahne ayarı mühendislerine göstermek için kullanıcı Arabirimi biraz oluştururuz.

Aşağıdaki kod parçası, bir metin öğesi oluşturmak ve uygulamanıza eklemek için kullanıcı Arabirimi çerçevesi kullanır:

```csharp
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
```

UI çerçevesi çok basit bir oyun içi kullanıcı arabirimi sağlamak yoktur ve için yeni düğüm ekleyerek çalışır [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) düğümü.

Bizim örnek kurulumları ikinci bölümü ana görünüm.  Bu adımlar, bir ışık, kamera ve bir Görünüm penceresi ekleme, ekranda bir 3B kutusu oluşturma bir 3B Sahne, oluşturma, bir dizi içerir.  Bu bölümde daha ayrıntılı incelenmektedir [Sahne, düğümleri, bileşenler ve kameralar](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras).

Örneğimizi üçüncü bölümü birkaç eylem tetikler.  Eylemleri olan belirli bir efekt açıklamak ve oluşturulduktan sonra bunlar tarifleri yürütülebilir isteğe bağlı bir düğüm tarafından çağırarak [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) metodunda bir `Node`.

İlk eylem Sıçrama efekti ile birlikte ölçeklenir ve ikinci bir kutunun sonsuza kadar döndürür:

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

Yukarıdaki nasıl oluşturacağımız ilk eylem gösterir bir [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) eylem budur yalnızca ölçeklendirme özelliği bir düğümün ikinci değer doğru ölçeklenebilmesini istediğinizi belirten bir tarif.  Bu eylem, sırayla kolaylaştırıcı bir eylem sarmalandıktan [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) eylem.  Kolaylaştırıcı eylemleri doğrusal bir eylemin yürütülmesi çarpıtan ve efekti uygulamak, bu durumda, geçirmek genişletme etkisi sağlar.
Bu nedenle bizim tarif olarak yazılabilir:

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

Tarif oluşturulduktan sonra biz tarif yürütün:

```csharp
await boxNode.RunActionsAsync (recipe)
```

Await belirten eylem tamamlandığında bu satırı sonra yürütmeye devam etmek istersiniz.  Eylemi tamamladığında biz ikinci animasyon tetikleyin.

[UrhoSharp kullanma](~/graphics-games/urhosharp/using.md) belge Urho ve oyun oluşturmak için kodunuzu nasıl kavramları daha derinlemesine araştırır.

## <a name="copyrights"></a>Telif Hakkı

Bu belge Xamarin Inc. orijinal içerikten ancak Urho3D proje için açık kaynak belgelerinde kapsamlı bir şekilde çizer ve ekran görüntüleri Cocos2D projesi içerir.

