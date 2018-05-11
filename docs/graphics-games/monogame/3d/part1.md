---
title: Model sınıfını kullanma
description: Model sınıfı, 3B grafik işleme, geleneksel yöntemi karşılaştırıldığında karmaşık 3B nesneleri işleme büyük ölçüde basitleştirir. Model nesneleri içerik hiçbir özel kod ile kolay tümleştirme sağlar, içerik dosyalarını oluşturulur.
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 01e000b22749acb1b5c3a3203db7f372613cca16
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="using-the-model-class"></a>Model sınıfını kullanma

_Model sınıfı, 3B grafik işleme, geleneksel yöntemi karşılaştırıldığında karmaşık 3B nesneleri işleme büyük ölçüde basitleştirir. Model nesneleri içerik hiçbir özel kod ile kolay tümleştirme sağlar, içerik dosyalarını oluşturulur._

MonoGame API içeren bir `Model` verileri depolamak için kullanılan sınıfı yüklenen bir içerik dosyasından ve işleme gerçekleştirmek için. Model dosyaları (düz renkli üçgen gibi) basit olabilir veya dokuları'nı ve ışık gibi karmaşık işleme için bilgiler içerebilir.

Bu kılavuzda kullanılır [anlamamıza 3B modeli](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) ve aşağıdakileri kapsar:

- Yeni bir oyun projesi başlatma
- Model ve onun doku için XNBs oluşturma
- XNBs oyun projeye dahil
- Bir 3B modeli çizme
- Birden fazla modeli çizme

Tamamlandığında, Projemizin şu şekilde görünür:

![Tamamlanan örnek altı robots gösterme](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>Boş bir oyun projesi oluşturma

İlk MonoGame3D adlı bir oyun projesi kurmanız gerekir. Yeni bir MonoGame projesi oluşturma hakkında daha fazla bilgi için bkz: [bir Çapraz Platform Monogame proje oluşturma bu kılavuzda](~/graphics-games/monogame/introduction/part1.md).

Geçmeden önce biz proje açar ve doğru şekilde dağıtır doğrulamanız gerekir. Bir kez dağıtılan biz boş bir mavi ekran görmeniz gerekir:

![Boş mavi oyun ekran](part1-images/image2.png)


## <a name="including-the-xnbs-in-the-game-project"></a>XNBs oyun projeye dahil

.Xnb dosya biçimi yerleşik içerik için standart bir uzantıdır (tarafından oluşturulan içeriği [MonoGame ardışık düzen aracı](http://www.monogame.net/documentation/?page=Pipeline)). Tüm yerleşik içeriği (.fbx dosya modelimizi durumunda olan) kaynak dosyası ve bir hedef dosya (.xnb dosyası) sahiptir. Uygulamalar tarafından gibi oluşturulabilecek ortak bir 3B modeli biçimi .fbx biçimidir [Maya](http://www.autodesk.com/products/maya/overview) ve [Blender](http://www.blender.org/). 

`Model` Sınıfı 3B geometri verilerini içeren bir diski bir .xnb dosyası yükleyerek olacak oluşturulan.   Bu .xnb dosya üzerinden içerik Proje oluşturulur. Monogame şablonları otomatik olarak bizim içerik klasöründe bir içerik projeyle (uzantısı .mgcp) içerir. MonoGame ardışık düzen araç hakkında ayrıntılı bilgi için bkz: [içerik ardışık düzen Kılavuzu](~/graphics-games/cocossharp/content-pipeline/index.md).

Biz Atla kullanılarak MonoGame ardışık düzeni üzerinden bu kılavuzun aracı ve kullanır. Burada bulunan XNB dosyalar. Dikkat edin. XNB dosyaları platforma göre farklılık gösterir, böylece birlikte çalıştığınız herhangi bir platform için doğru XNB dosyaları kümesini kullandığınızdan emin olun.

Unzip [Content.zip dosya](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) böylece kapsanan .xnb dosyaları bizim oyunda kullanabilirsiniz. Bir Android projesi üzerinde çalışıyorsanız, sağ tıklayın **varlıklar** klasöründe **WalkingGame.Android** projesi. Bir iOS projesi üzerinde çalışıyorsanız, sağ tıklayın **WalkingGame.iOS** projesi. Seçin **Ekle -> dosyaları Ekle...**  ve üzerinde çalıştığınız platforma klasöründeki her iki .xnb dosyaları seçin.

İki dosya artık bizim projesinin bir parçası olması gerekir:

![Çözüm Gezgini içerik klasörünü xnb dosyaları](part1-images/xnbsinxs.png)

Mac için Visual Studio otomatik olarak yeni eklenen XNBs için yapı eylemi ayarlı değil. İOS için her dosyaları seçin ve sağ **yapı eylemi -> BundleResource**. Android için her dosyaları seçin ve sağ **yapı eylemi -> AndroidAsset**.

## <a name="rendering-a-3d-model"></a>Bir 3B model oluşturma

Model ekran görmek gereken son adım yükleme ve kod çizim eklemektir. Özellikle, biz aşağıdakileri yapmanız:

- Tanımlayan bir `Model` örneğini bizim `Game1` sınıfı
- Yükleme `Model` örneğini `Game1.LoadContent`
- Çizim `Model` örneğini `Game1.Draw`

Değiştir `Game1.cs` kod dosyasının (bulunan **WalkingGame** PCL) aşağıdaki:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;

        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
```

Biz bu kodu çalıştırırsanız biz model ekran görürsünüz:

![Ekranda gösterilen modelini](part1-images/image8.png "bu kodu çalıştırırsanız, model ekran görüntülenir")

### <a name="model-class"></a>Model sınıfı

`Model` Sınıftır içerik dosyalarını (.fbx dosyaları gibi) 3B işleme gerçekleştirmek için temel sınıf. 3B geometri, doku başvuruları da dahil olmak üzere işleme için gereken bilgilerin tümünü içerir ve `BasicEffect` konumlandırma, aydınlatma ve kamera değerleri denetleyen örnekleri.

`Model` Sınıfının kendisi doğrudan bir tek model örneği birden çok konumda işlenip beri bu kılavuzda göstereceğiz gibi konumlandırma için değişkenleri yok.

Her `Model` bir veya daha fazla oluşur `ModelMesh` aracılığıyla sunulan örnekleri `Meshes` özelliği. Kabul, ancak bir `Model` tek bir oyun (bir robot veya bir araba gibi), her nesne `ModelMesh` farklı çizilebilir `BasicEffect` değerleri. Örneğin, tek tek kafes bölümleri bir robot veya bir araba üzerinde Tekerlek Bacak temsil ve biz atayabilir `BasicEffect` Tekerlek döndürme veya Bacak yapmak için değerler taşır. 

### <a name="basiceffect-class"></a>BasicEffect sınıfı

`BasicEffect` Sınıfı işleme seçenekleri denetlemek için özellikler sağlar. Vermiyoruz için ilk değişiklik `BasicEffect` çağırmaktır `EnableDefaultLighting` yöntemi. Adından da anlaşılacağı gibi bu, doğrulamak için çok kullanışlıdır varsayılan aydınlatma sağlayan bir `Model` beklendiği gibi oyun görüntülenir. Biz çıkarırsanız `EnableDefaultLighting` yalnızca kendi doku ile ancak hiçbir gölgelendirme veya aynasal Işıma işlenen model göreceğiz sonra çağırın:

```csharp
//effect.EnableDefaultLighting ();
```

![Yalnızca kendi doku ile ancak hiçbir gölgelendirme veya aynasal Işıma işlenen model](part1-images/image9.png "yalnızca kendi doku ile ancak hiçbir gölgelendirme veya aynasal Işıma işlenen model")

`World` Özelliği, konum, döndürme ve modelin ölçeğini ayarlamak için kullanılabilir. Kullandığı yukarıdaki kodu `Matrix.Identity` anlamına değeri `Model` oyun tam olarak belirtilen .fbx dosyasında oluşturmaz. Biz matrisleri ve daha ayrıntılı olarak 3B koordinatları kapsayan [bölüm 3](~/graphics-games/monogame/3d/part3.md), ancak bir örnek olarak biz konumunu değiştirebilirsiniz `Model` değiştirerek `World` özelliğini aşağıdaki gibi:

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

Bu kod 3 world birimler tarafından taşınan nesne sonuçlanır:

![Bu kod 3 world birimler tarafından taşınan nesne sonuçlanır](part1-images/image10.png "3 world birimler tarafından taşınan nesne bu kodu sonuçlanır")

Üzerinde atanan son iki özellik `BasicEffect` olan `View` ve `Projection`. Biz de 3B kameralar kapsayan [bölüm 3](~/graphics-games/monogame/3d/part3.md), ancak örnek olarak, biz kamera konumu yerel değiştirerek değiştirebilirsiniz `cameraPosition` değişkeni:

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

Kamera, başka arka taşımıştır sonuçta görebiliriz `Model` perspektif nedeniyle daha küçük görünmesini:

![Kamera perspektif nedeniyle daha küçük görünmesini modelinde kaynaklanan başka arka taşındı](part1-images/image11.png "kamera perspektif nedeniyle daha küçük görünmesini modelinde kaynaklanan başka arka taşındı")

## <a name="rendering-multiple-models"></a>Birden çok modelleri oluşturma

Tek bir yukarıda belirtildiği gibi `Model` birden çok kez çizilebilir. Bu işlemi kolaylaştırmak için taşıma ulaşacağız `Model` kod istenen alan kendine özgü bir yöntem çizim `Model` bir parametre olarak konumu. Bir kez tamamlandı, bizim `Draw` ve `DrawModel` yöntemleri gibi görünür:


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);
    DrawModel (new Vector3 (-4, 0, 0));
    DrawModel (new Vector3 ( 0, 0, 0));
    DrawModel (new Vector3 ( 4, 0, 0));
    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));
    base.Draw(gameTime);
}
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            effect.World = Matrix.CreateTranslation (modelPosition);
            var cameraPosition = new Vector3 (0, 10, 0);
            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;
            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);
            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;
            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }
        // Now that we've assigned our properties on the effects we can
        // draw the entire mesh
        mesh.Draw ();
    }
}
```

Bu robot altı kat çizilen modeli sonuçlanır:

![Bu robot altı kat çizilen modeli sonuçları](part1-images/image1.png "bu robot altı kat çizilen modeli sonuçları")

## <a name="summary"></a>Özet

MonoGame'nın bu kılavuzda sunulan `Model` sınıfı. Bir .xnb bir .fbx dosyası dönüştürme kapsayan oturum aç, yüklenebilir bir `Model` sınıfı. Ayrıca gösterir nasıl değiştirileceği bir `BasicEffect` örneği etkileyebilir `Model` çizim.

## <a name="related-links"></a>İlgili bağlantılar

- [MonoGame modeli başvurusu](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [Projeyi (örnek)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
