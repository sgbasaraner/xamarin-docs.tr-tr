---
title: 3B MonoGame koordinatları
description: 3B koordinat sistemi anlama 3B oyunlar geliştirilmesi konusunda önemli bir adımdır. MonoGame konumlandırma, Yönlendirme ve 3B uzaydaki nesneleri ölçekleme için sınıflar sağlar.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 2f14d21302ed4295d16baa28723df6ef79863686
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921644"
---
# <a name="3d-coordinates-in-monogame"></a>3B MonoGame koordinatları

_3B koordinat sistemi anlama 3B oyunlar geliştirilmesi konusunda önemli bir adımdır. MonoGame konumlandırma, Yönlendirme ve 3B uzaydaki nesneleri ölçekleme için sınıflar sağlar._

3B oyun geliştirmeye 3B koordinat sistemi nesneleri yönetmek nasıl bilinmesini gerektirir. Bu kılavuz, görsel nesneler (özellikle bir Model) işlemek nasıl ele alınacaktır. Biz, 3B kamera sınıfı oluşturmak için bir model denetleme kavramları yapı.

Sunulan kavramları Lineer Cebir kaynaklanan ancak güçlü matematik arka plan olmadan herhangi bir kullanıcı bu kavramları kendi oyunlarda uygulayabilmeniz için aşağıdakileri pratik bir yaklaşım olması.

Biz, aşağıdaki konuları kapsayan:

- Proje Oluşturma
- Bir Robot varlığı oluşturma
- Robot varlık taşıma
- Matris çarpım
- Kamera varlık oluşturma
- Girişi kamera taşıma

İşlemi tamamladıktan sonra size bir proje ile daire ve dokunma girişi tarafından denetlenen bir kamera taşıma anlamamıza sahip olursunuz:

![](part3-images/image1.gif "İşlemi tamamladıktan sonra uygulama bir proje ile daire ve dokunma girişi tarafından denetlenen bir kamera taşıma anlamamıza içerir")


## <a name="creating-a-project"></a>Proje Oluşturma

Bu kılavuz, 3B alanda nesneleri taşıma odaklanır. Proje oluşturma modellerini ve köşe diziler için ile başlarsınız [, şurada bulunabilir](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/). Yüklendikten sonra sıkıştırmasını açın ve çalıştırdığı ve görmeliyiz aşağıdaki emin olmak için projeyi açın:

![](part3-images/image2.png "Yüklendikten sonra sıkıştırmasını açın ve onu çalıştırır ve bu görünüm görüntülenmelidir emin olmak için projeyi açın")


## <a name="creating-a-robot-entity"></a>Bir Robot varlığı oluşturma

Bizim robot geçici taşıma başlamadan önce oluşturacağız bir `Robot` sınıf çizme ve taşıma için mantık içerir. Oyun geliştiriciler bu kapsülleme mantığı ve verileri olarak başvurduğu bir *varlık*.

Yeni bir boş sınıf dosyası ekleyin **MonoGame3D** taşınabilir sınıf kitaplığı (platforma özgü ModelAndVerts.Android değil). Bu ad **Robot** tıklatıp **yeni**:

![](part3-images/image3.png "Robot adlandırın ve yeni'yi tıklatın")

Değiştirme `Robot` gibi sınıfı:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);

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
    }
}
```

`Robot` Kodudur temelde aynı kodda `Game1` çizim için bir `Model`. Gözden geçirme için `Model` yükleme ve çizim bkz [modellerle çalışma üzerinde bu kılavuzda](~/graphics-games/monogame/3d/part1.md). Biz şimdi tüm kaldırabilirsiniz `Model` yükleme ve koddan işleme `Game1`ve bunların yerine bir `Robot` örneği:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
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

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Şimdi biz kodu çalıştırırsanız biz Sahne çoğunlukla altında kat çizilmiş yalnızca bir robot sahip olacaktır:

![](part3-images/image4.png "Kod şimdi çalıştırırsanız, uygulama bir Sahne çoğunlukla altında kat çizilmiş yalnızca bir robot ile görüntülenir")

## <a name="moving-the-robot"></a>Robot taşıma

Biz sahip olduğunuza göre bir `Robot` sınıfı, robot taşıma mantığı ekleyebiliriz. Bu durumda, biz yalnızca oyun saate bir daire içinde taşımak robot hale getireceğiz. Bu bir biraz pratik gerçek bir oyun girişi için bir karakter genellikle yanıt verebilir veya bize 3B konumlandırma ve döndürme keşfetmek için bir ortam sağlar yapay zeka, ancak beri uygulamasıdır.

Biz gerekir gelen dışında tek bilgi `Robot` sınıfı, geçerli oyun süre. Ekleyeceğiz bir `Update` alacak yöntemi bir `GameTime` parametresi. Bu `GameTime` parametresi, robot son konumunu belirlemek için kullanacağız bir açı değişkeni artırmak için kullanılır.

İlk olarak, açı alanın ekleyeceğiz `Robot` altında sınıf `model` alan:

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 Biz bu değeri artırabilirsiniz artık bir `Update` işlevi:

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

Emin olmak ihtiyacımız `Update` yöntemi çağrılır `Game1.Update`:

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

Elbette, bu noktada Açı alanı hiçbir şey yapmaz – kullanmak için kod yazmanız gerekir. Biz değiştireceksiniz `Draw` yöntemi böylece biz world hesaplayabilirsiniz `Matrix` adanmış bir yöntem: 

```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
}
```

Ardından, biz uygulamak `GetWorldMatrix` yönteminde `Robot` sınıfı:

```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;

    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
}
```

Bu kod çalıştırmanın sonuçlarını bir daire içinde taşıma robot sonuçlanır:

![](part3-images/image5.gif "Bu kod sonuçları bir daire içinde taşıma robot çalışıyor.")

## <a name="matrix-multiplication"></a>Matris çarpım

Yukarıdaki kod oluşturarak robot döndüğü bir `Matrix` içinde `GetWorldMatrix` yöntemi. `Matrix` Yapısı (kümesi konum) Çevir, döndürün ve ölçeklendirmek için kullanılan 16 float değerleri içerir (boyutunu ayarlayın). Biz atadığınızda `effect.World` özelliği biz bildiren sistem nasıl getirin, boyut ve ne olursa olsun hizalanması işleme arka plandaki çizim olması oluşacağını (bir `Model` veya köşeleri geometriye). 

Neyse ki, `Matrix` yapısı matrisleri genel türleri oluşturulmasını basitleştiren yöntemleri çeşitli içerir. Yukarıdaki kod içinde kullanılan ilk `Matrix.CreateTranslation`. Matematik terim *çeviri* bir nokta (veya bir model Bu örnekte) sonuçları bir işlem başvurduğu bir konumdan (örneğin, döndürme veya yeniden boyutlandırma) başka bir değişiklik yapmadan diğerine taşıma. İşlev çeviri için X, Y ve Z bir değer alır. Biz bizim Sahne yukarıdan aşağı, gelen görüntülerseniz bizim `CreateTranslation` yönteminde (yalıtım) aşağıdakileri gerçekleştirir:

![](part3-images/image6.png "Bu eylem yalıtım CreateTranslation yönteminde gerçekleştirir")

Döndürme matris oluşturuyoruz ikinci matris olduğu kullanarak `CreateRotationZ` matris. Bu, döndürme oluşturmak için kullanılan üç yöntem biridir:

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

Her yöntem hakkında belirli bir eksen döndürerek döndürme matris oluşturur. Örneğimizde, biz "yedekleme" işaret Z ekseni etrafında döndürme. Aşağıdaki nasıl eksen tabanlı döndürme görselleştirmenize yardımcı olabilecek çalışır:

![](part3-images/image7.png "Bu nasıl eksen tabanlı döndürme görselleştirmenize yardımcı çalışır")

Ayrıca kullanıyoruz `CreateRotationZ` nedeniyle zamanla artar Açı alanı yöntemiyle bizim `Update` çağrılan yöntem. Sonuç `CreateRotationZ` yöntemi bizim robot başlangıcı Zaman geçtikçe Yörünge neden olur.

Kodu son satırının iki matrisi tek istekte birleştirir:

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

Bu normal çarpma biraz farklı çalıştığını matris çarpım olarak adlandırılır. *Çarpmanın Yer değiştirebilme özelliğine* çarpma işlemine numaralarının sırasını sonucu değiştirmez belirtir. Diğer bir deyişle, 3 * 4 4 * 3'e eşdeğerdir. Yer değiştirebilme değil matris çarpım farklıdır. Diğer bir deyişle, yukarıdaki satır olarak "Model taşımak için translationMatrix uygulayın ve her şeyi rotationMatrix uygulayarak döndürün" okunabilir. Biz, yukarıdaki satırın konumunu ve döndürme gibi etkilediği şekilde görselleştirin:

![](part3-images/image8.png "Görselleştirme pf yukarıdaki satırın konumunu ve döndürme etkiler yolu")

Matris çarpım sırasını sonucu nasıl etkileyebileceğini anlamanıza yardımcı olması için burada matris çarpım ters aşağıdakileri göz önünde bulundurun:

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

Yukarıdaki kod ilk yerinde modeli döndürün ve sonra çevirebilir:

![](part3-images/image9.png "Yukarıdaki kod ilk yerinde modeli döndürün ve sonra çevirebilir")

Kod ile ters çarpma çalıştırırsanız biz döndürme önce geçerli olmadığından, yalnızca model yönünü etkiler ve model konumunu aynı kalır fark edeceksiniz. Diğer bir deyişle, modeli yerinde döndürür:

![](part3-images/image10.gif "Yerinde modeli döndürür")

## <a name="creating-the-camera-entity"></a>Kamera varlık oluşturma

`Camera` Varlık tüm giriş tabanlı taşıma gerçekleştirmek ve Özellikler atama için özellikler sağlamak için gerekli mantığı içerecek `BasicEffect` sınıfı.

İlk statik kamera (hiçbir giriş tabanlı taşıma) uygulamak ve bizim varolan bir projeye tümleştirebilirsiniz. Yeni bir sınıf ekleyin **MonoGame3D** taşınabilir sınıf kitaplığı (aynı proje ile `Robot.cs`) ve adlandırın **kamera**. Dosyasının içeriğini aşağıdaki kodla değiştirin:

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;

                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

Yukarıdaki kod koddan çok benzer `Game1` ve `Robot` , Ata matrisleri üzerinde `BasicEffect`. 

Biz yeni tümleştirebilirsiniz artık `Camera` bizim mevcut projeleri sınıfına. İlk olarak, biz değiştireceksiniz `Robot` yapılacak sınıfı bir `Camera` örneğini kendi `Draw` çok yinelenen kod giderilecektir yöntemi. Değiştir `Robot.Draw` aşağıdaki yöntemiyle:

```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
}
```

Ardından, değişiklik `Game1.cs` dosyası:

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

            floorVerts [0].Position = new Vector3 (-20, -20, 0);
            floorVerts [1].Position = new Vector3 (-20,  20, 0);
            floorVerts [2].Position = new Vector3 ( 20, -20, 0);

            floorVerts [3].Position = floorVerts[1].Position;
            floorVerts [4].Position = new Vector3 ( 20,  20, 0);
            floorVerts [5].Position = floorVerts[2].Position;

            int repetitions = 20;

            floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
            floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
            floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

            floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
            floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
            floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

            effect = new BasicEffect (graphics.GraphicsDevice);

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

            effect.TextureEnabled = true;
            effect.Texture = checkerboardTexture;

            foreach (var pass in effect.CurrentTechnique.Passes)
            {
                pass.Apply ();

                graphics.GraphicsDevice.DrawUserPrimitives (
                            PrimitiveType.TriangleList,
                    floorVerts,
                    0,
                    2);
            }
        }
    }
}
```

Değişiklikler `Game1` önceki sürümünden (ile tanımlanmış `// New camera code` ) şunlardır:

- `Camera` alanı `Game1`
- `Camera` Örnekleme içinde `Game1.Initialize`
- `Camera.Update` Çağır `Game1.Update`
- `Robot.Draw` Şimdi geçen bir `Camera` parametresi
- `Game1.Draw` Şimdi kullanan `Camera.ViewMatrix` ve `Camera.ProjectionMatrix`

## <a name="moving-the-camera-with-input"></a>Girişi kamera taşıma

Şu ana kadar ekledik bir `Camera` varlık ile çalışma zamanı davranışını değiştirmek için herhangi bir şey bu işlemi yapmadıysanız ancak. Kullanıcının veren davranışı ekleyeceğiz için:

- Sola doğru kamerayı ekranın sol tarafındaki dokunma
- Sağa kamerayı ekranın sağ tarafındaki dokunma
- Dokunmatik ekran kamera ilerlemek için merkezi

### <a name="making-lookat-relative"></a>LookAt göreli yapma

Biz öncelikle güncelleştireceğim `Camera` eklemek için sınıfı bir `angle` kullanılır alanı yönünü ayarlama `Camera` karşılıklı. Şu anda, bizim `Camera` yerel durumdadır yönünü belirler `lookAtVector`, için atanan `Vector3.Zero`. Diğer bir deyişle, bizim `Camera` başlangıcı sırasında her zaman görünür. Kamera geçerse, kamera karşılıklı açı da değiştirir:

![](part3-images/image11.gif "Kamera geçerse, ardından kamera karşılıklı açı da değiştirir")

İstediğimiz `Camera` – konumuna bakılmaksızın aynı yönde en az karşılıklı için biz döndürme mantığı uygulayana kadar `Camera` girişi kullanma. Ayarlamak için ilk değişiklik olacaktır `lookAtVector` dışına bizim geçerli konumuna bağlı için değişken yerine mutlak bir konumdaki görünümlü:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

Bu, sonuçlanır `Camera` düz üzerinde world görüntüleme. Dikkat ilk `position` değeri için değiştirilmiş `(0, 20, 10)` böylece `Camera` X ekseninde ortalanır. Çalışan oyun görüntüler:

![](part3-images/image12.png "Bu görünüm oyun çalıştıran görüntüler")

### <a name="creating-an-angle-variable"></a>Bir açının değişkeni oluşturma

`lookAtVector` Değişkeni bizim kamera görüntüleme açı denetler. Şu anda negatif Y ekseni görüntülemek için düzeltilen ve aşağı biraz eğildiğinde (gelen `-.5f` Z değeri). Oluşturacağız bir `angle` ayarlamak için kullanılan değişken `lookAtVector` özelliği. 

Bu kılavuzun önceki bölümlerinde matrisleri nesneler nasıl çizilir döndürmek için kullanılabilir gösterdi. Biz matrisleri gibi vektörlerinin döndürmek için de kullanabilirsiniz `lookAtVector` kullanarak `Vector3.Transform` yöntemi. 

Ekleme bir `angle` alan ve değiştirme `ViewMatrix` özelliğini aşağıdaki gibi:

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

### <a name="reading-input"></a>Giriş okuma

Bizim `Camera` varlık artık tam olarak denetlenebilir açı değişkenleri ve konum – biz giriş göre değiştirmek yeterlidir.

İlk olarak, biz elde edersiniz `TouchPanel` kullanıcı ekran burada temas bulmak için durumu. Kullanma hakkında daha fazla bilgi için `TouchPanel` sınıfı için bkz: [TouchPanel API Başvurusu](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel).

Kullanıcı sol üçüncü temas sonra ayarlamanız `angle` değeri böylece `Camera` sol döndürür ve kullanıcı doğru üçüncü temas, diğer bir yol döndürmek. Kullanıcının ekran Orta üçüncü temas sonra biz taşırsınız `Camera` ilet.

İlk olarak, kullanarak bir ekleyin nitelemek için deyimi `TouchPanel` ve `TouchCollection` sınıfları `Camera.cs`:

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

Ardından, değişiklik `Update` dokunma paneli okuyun ve ayarlamak için yöntem `angle` ve `position` değişkenleri uygun şekilde:

```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
}
```

Şimdi `Camera` giriş touch yanıt verir:

![](part3-images/image1.gif "Şimdi kamera giriş touch yanıtlar")

Update yöntemi çağrılarak başlar `TouchPanel.GetState`, rötuşları koleksiyonunu döndürür. Ancak `TouchPanel.GetState` birden çok dokunma noktaları döndürebilir biz yalnızca kolaylık sağlaması için birinci hakkında endişelenmeniz.

Kullanıcının ekran temas, kod, ilk dokunma sol, Orta veya ekranın sağ üçüncü olup olmadığını denetler. Sol ve sağ Üçe kamera artırarak veya azaltarak döndürme `angle` göre değişken `TotalSeconds` (oyun kare hızı bakılmaksızın aynı şekilde davranır böylece) değeri.

Kullanıcının üçüncü merkezi temas varsa ekranda, ardından kamera İleri taşınır. Bu başlangıçta negatif Y ekseni işaret eden olarak tanımlanmış, sonra kullanılarak oluşturulan bir matris tarafından döndürülen iletme vektör elde ederek ilk gerçekleştirilir `Matrix.CreateRotationZ` ve `angle` değeri. Son olarak `forwardVector` uygulanan `position` kullanarak `unitsPerSecond` katsayısı.

## <a name="summary"></a>Özet

Bu kılavuz, taşıma ve döndürmek alınmaktadır `Models` 3D boşluk kullanarak `Matrices` ve `BasicEffect.World` özelliği. Taşıma, bu formu, 3B oyunlarda nesneleri taşıma için temeli sağlar. Bu izlenecek yol da nasıl uygulanacağını kapsayan bir `Camera` herhangi bir konumu ve açı dünyadan görüntülemek için varlık.

## <a name="related-links"></a>İlgili bağlantılar

- [MonoGame API bağlantı](http://www.monogame.net/documentation/?page=api)
- [Tamamlanan proje (örnek)](https://developer.xamarin.com/samples/monodroid/MonoGame3DCamera/)
