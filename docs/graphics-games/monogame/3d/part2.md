---
title: 3B grafik köşeleri ile MonoGame içinde çizme
description: Köşeleri dizileri 3B bir nesneyi bir nokta başına temelinde nasıl işleneceğini tanımlayacak MonoGame kullanılmasını destekler. Kullanıcılar, dinamik geometri oluşturmak, özel efektler uygulamak ve yüzey kaldırma aracılığıyla kendi işleme verimliliğini artırmak için köşe dizi avantajından yararlanabilirsiniz.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 4fa6929a8de19608c06690e4cf24514f6749480a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>3B grafik köşeleri ile MonoGame içinde çizme

_Köşeleri dizileri 3B bir nesneyi bir nokta başına temelinde nasıl işleneceğini tanımlayacak MonoGame kullanılmasını destekler. Kullanıcılar, dinamik geometri oluşturmak, özel efektler uygulamak ve yüzey kaldırma aracılığıyla kendi işleme verimliliğini artırmak için köşe dizi avantajından yararlanabilirsiniz._

Okuma aracılığıyla kullanıcılar [modelleri oluşturma hakkında kılavuz](~/graphics-games/monogame/3d/part1.md) MonoGame 3D modelde işleme ile tanıdık gelecektir. `Model` (Örneğin, .fbx) dosyasında tanımlanan verilerle çalışırken ve statik verilerle ilgilenirken 3B grafik işlemek için etkili bir yol bir sınıftır. Bazı oyunlar tanımlanan veya çalışma zamanında dinamik olarak yönetilen 3B geometri gerektirir. Bu durumda, biz dizi kullanabilirsiniz *köşeleri* tanımlayın ve geometri işlemek için. Bir köşe noktası geometri tanımlamak için kullanılan bir sıralı liste parçası olan 3B uzaydaki için genel bir terimdir. Genellikle köşeleri üçgenler bir dizi tanımlamak için farklı bir şekilde sıralanır.

Köşeleri 3B nesneleri oluşturmak için nasıl kullanıldığı görselleştirmenize yardımcı olmak için aşağıdaki küre şimdi göz önünde bulundurun:

![](part2-images/image1.png "Köşeleri 3B nesneleri oluşturmak için nasıl kullanıldığı görselleştirmenize yardımcı olmak için bu küre göz önünde bulundurun.")

Yukarıda gösterildiği gibi küre birden çok üçgenler açıkça oluşur. Biz köşeleri form üçgenler nasıl bağlanacağını görmek için kürenin Tel Çerçeve görüntüleyebilirsiniz:

![](part2-images/image2.png "Form üçgenler köşeleri nasıl bağlanacağını görmek için kürenin Tel Çerçeve görüntüleyin")

Bu kılavuzda aşağıdaki konular ele alınacaktır:

- Proje oluşturma
- Köşeleri oluşturma
- Çizim kod ekleme
- Bir doku ile işleme
- Doku koordinatları değiştirme
- Köşeleri modelleri ile işleme

Tamamlanmış projenin köşe dizisi kullanılarak çizilmiş Damalı kat içerir:

![](part2-images/image3.png "Tamamlanmış projenin köşe dizisi kullanılarak çizilmiş Damalı kat içerir")

## <a name="creating-a-project"></a>Proje Oluşturma

İlk olarak, biz bizim başlangıç noktası olarak hizmet verecek bir projesi indirirsiniz. Modeli projesi kullanacağız [, şurada bulunabilir](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/).

İndirilen ve sıkıştırması açılmış sonra açmak ve projeyi çalıştırın. Ekran çizilen altı robot modelleri görmeyi beklediğiniz:

![](part2-images/image4.png "Ekran çizilen altı robot modelleri")

Bu proje sonuna biz kendi özel köşe işleme robot ile birleştirerek `Model`, biz robot işleme kod silmek için giderek değil. Bunun yerine, biz Temizle yalnızca `Game1.Draw` 6 robots çizim şimdilik kaldırmak için çağırın. Bunu yapmak için açın **Game1.cs** dosya ve bulun `Draw` yöntemi. Aşağıdaki kod içerecek şekilde değiştirin:

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

Bu, boş bir mavi ekran görüntüleme bizim oyunda neden olur:

![](part2-images/image5.png "Bu, boş bir mavi ekran görüntüleme oyunda neden olacak")

## <a name="creating-the-vertices"></a>Köşeleri oluşturma

Bizim geometri tanımlamak için köşeleri dizisi oluşturacağız. Bu kılavuzda, bir 3B düzlem (3B uzaydaki kare, bir uçak) oluşturuluyor. Bizim düzlemi dört yanları ve dört köşe olsa da, her biri üç köşeleri gerektiren iki üçgenler oluşacaktır. Bu nedenle, biz altı noktaları toplam tanımlama.

Şu ana kadar biz genel bir fikir tepe hakkında konuşurken ancak MonoGame köşe için kullanılabilecek bazı standart yapılar sağlar:

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

Her tür adı içerdiği bileşenleri gösterir. Örneğin, `VertexPositionColor` konumu ve rengi değerlerini içerir. Her bileşenlerini bakalım:

- Konum – tüm köşe türler bir `Position` bileşeni. `Position` Değerleri tanımlayın köşe 3B alanda (X, Y ve Z) bulunduğu.
- Renk – köşeleri belirtebilirsiniz isteğe bağlı olarak bir `Color` özel tonlamak gerçekleştirmek için değer.
- Normal – normalleri nesnenin yüzeyinin karşılıklı hangi yolla tanımlayın. Normalleri yönü bu yana aydınlatma sahip bir nesne oluşturma, bir yüzey etkileri ne kadar açık aldığı karşılıklı gereklidir. Normalleri olarak belirtilen genellikle bir *birim vektör* – 1 uzunluğuna sahip bir 3B vektör.
- Doku – doku doku hangi kısmının verilen köşelerde görünmesi gereken doku koordinatları – başka bir deyişle, ifade eder. Bir 3B nesnesi ile bir doku işleme, doku değerleri gereklidir. Doku koordinatlar değerleri 0 ile 1 arasında kalan yani normalleştirilmiş koordinatları belirlenir. Biz doku koordinatları bu kılavuzun ilerleyen bölümlerinde daha ayrıntılı olarak ele alacağız.

Bizim düzlemi kat hizmet verir ve biz kullanacağız şekilde bizim işleme gerçekleştirirken bir doku uygulamak isteyeceksiniz `VertexPositionTexture` bizim köşeleri tanımlamak için türü.

Bizim Game1 sınıfı için ilk olarak, bir üye ekleyeceğiz:

```csharp
VertexPositionTexture[] floorVerts; 
```

Ardından, bizim tepe tanımlamak `Game1.Initialize`. Bu makalenin önceki bölümlerinde başvurulan sağlanan şablonu içermiyor bildirimi bir `Game1.Initialize` tüm yöntem eklemek ihtiyacımız şekilde yöntemi `Game1`:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];
    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);
    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // We’ll be assigning texture values later
    base.Initialize ();
}
```

Bizim köşeleri nasıl görüneceğini görselleştirmenize yardımcı olmak için aşağıdaki diyagramda göz önünde bulundurun:

![](part2-images/image6.png "Köşeleri nasıl görüneceğini görselleştirmenize yardımcı olması için bu diyagrama göz önünde bulundurun.")

Bizim işleme kod uygulama tamamlanana kadar köşeleri görselleştirmek için bizim diyagramda yararlanmayı gerekir.

## <a name="adding-drawing-code"></a>Çizim kodunu ekleme

Konumlar tanımlanan bizim geometri için sahip olduğumuz, biz bizim işleme kod yazabilirsiniz.

İlk olarak, tanımlamak ihtiyacımız bir `BasicEffect` konumu gibi işleme ve Aydınlatma parametrelerini tutacak örneği. Bunu yapmak için ekleyin bir `BasicEffect` üyesine `Game1` where aşağıda sınıfı `floorVerts` alan tanımlanır:


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

Ardından, değişiklik `Initialize` yöntemi etkisi tanımlamak için:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

Şimdi biz çizim gerçekleştirmek için kod ekleyebilirsiniz:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
            // We’ll be rendering two trinalges
            PrimitiveType.TriangleList,
            // The array of verts that we want to render
            floorVerts,
            // The offset, which is 0 since we want to start 
            // at the beginning of the floorVerts array
            0,
            // The number of triangles to draw
            2);
    }
}
```

Çağrılacak gerekir `DrawGround` içinde bizim `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

Uygulama çalıştırıldığında şunları görüntüler:

![](part2-images/image7.png "Uygulama bu çalıştırıldığında görüntülenir")

Yukarıdaki kod Ayrıntılar bazıları bakalım.

### <a name="view-and-projection-properties"></a>Projeksiyon özelliklerini görüntüleme ve

`View` Ve `Projection` denetim özelliklerini nasıl biz Sahne görüntüleyin. Biz model işleme kodunu yeniden eklediğinizde, biz bu kodu daha sonra değiştirme. Özellikle, `View` konumunu ve kamera yönünü denetler ve `Projection` denetimleri *görünüm alanı* (kullanılabileceği kamera yakınlaştırmak için).

### <a name="techniques-and-passes"></a>Teknikleri ve geçişleri

Bir kez biz gerçek işleme biz gerçekleştirebilirsiniz bizim etkisi özellikleri atadınız. 

Biz değiştirme olmaz `CurrentTechnique` bu kılavuz, ancak daha gelişmiş oyunlar özelliğinde (örneğin, renk değeri nasıl uygulanacağını), farklı şekillerde çizim gerçekleştiren tek bir etkisi olabilir. Bu işleme modların her birinde, önce işleme atanabilecek bir teknik olarak gösterilebilir. Ayrıca, her bir teknik düzgün bir şekilde işlemek için birden çok geçiş gerektirebilir. Efektler birden çok geçiş Işıyan yüzeyini veya fur gibi karmaşık görselleri işleme yüklemeniz gerekebilir.

Göz önünde bulundurmanız gereken önemli şey olan `foreach` döngü sağlayan temel karmaşıklığını bağımsız olarak herhangi bir etkisi işlemek aynı C# kodu `BasicEffect`.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` Burada köşeleri işlendiğini olur. İlk parametresi, biz bizim köşeleri nasıl organize yöntemi bildirir. Böylece kullandığımız için her üçgen üç sıralı köşeleri tarafından tanımlanan bunları yapılandırılmış `PrimitiveType.TriangleList` değeri.

İkinci parametre daha önce tanımlanan köşeleri dizisidir.

Üçüncü parametre çizmek için ilk dizini belirtir. İşlenmek üzere bizim tüm köşe dizi istiyoruz olduğundan, biz 0 değeri geçmesi.

Son olarak, işlemek için kaç tane üçgenler belirtin. İki üçgenler bizim köşe dizi içeriyor, bu nedenle 2 değerini geçirin.

## <a name="rendering-with-a-texture"></a>Bir doku ile işleme

Bu noktada uygulamamıza beyaz düzlemi (perspektifindeki) işler. Bizim düzlemi oluşturulurken kullanılacak Projemizin doku sonraki ekleyeceğiz. 

Örneği basit tutmak için .png doğrudan Projemizin yerine MonoGame ardışık düzen aracını kullanarak ekleyeceğiz. Bunu yapmak için indirme [bu .png dosyası](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) bilgisayarınıza. Yüklendikten sonra sağ **içerik** seçin ve çözüm paneli klasöründe **Ekle > dosyaları Ekle...**  . Android üzerinde çalışıyorsanız, ardından bu klasörü altında bulunur **varlıklar** Android özgü proje klasöründe. İOS varsa, daha sonra bu klasör iOS proje kök dizininde olacaktır. Konuma gidin, **checkerboard.png** kaydedilir ve bu dosyayı seçin. Dizine dosya kopyalamak için seçin.

Ardından, oluşturmak için kodu ekleyeceğiz bizim `Texture2D` örneği. İlk olarak, ekleyin `Texture2D` bir üyesi olarak `Game1` altında `BasicEffect` örneği:

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

Değiştirme `Game1.LoadContent` gibi:


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

Ardından, değişiklik `DrawGround` yöntemi. Yalnızca gerekli atamak için değişikliktir `effect.TextureEnabled` için `true` ve ayarlamak için `effect.Texture` için `checkerboardTexture`:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
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

    // new code:
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
```

Son olarak değiştirmek ihtiyacımız `Game1.Initialize` yöntemi de doku atamak için bizim köşelerinin düzenler:


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

Size kodu çalıştırırsanız, biz bizim düzlemi şimdi Damalı bir desen görüntüler görebilirsiniz:

![](part2-images/image8.png "Düzlemi şimdi Damalı bir desen görüntüler")

## <a name="modifying-texture-coordinates"></a>Doku değiştirme koordinatları

MonoGame kullandığı koordinatları 0 ile 1 arasında yerine 0 ile doku genişliği veya yüksekliği arasında olan doku koordinatları normalleştirilmiş. Aşağıdaki diyagramda, normalleştirilmiş koordinatları görselleştirmenize yardımcı olabilir:

![](part2-images/image9.png "Bu diyagramda normalleştirilmiş koordinatları görselleştirmenize yardımcı olabilir")

Normalleştirilmiş doku koordinatları doku kodu yeniden yazmanız veya modelleri (.fbx dosyaları gibi) yeniden gerek kalmadan yeniden boyutlandırma izin verin. Normalleştirilmiş koordinatları belirli piksel yerine bir oranı gösterdiğinden, bu mümkündür. Örneğin, (1,1) olacak her zaman temsil sağ alt köşe doku boyutu ne olursa olsun.

Biz tekrarları sayısı için tek bir değişken kullanmak için doku koordinat atama değiştirebilirsiniz:


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

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

    base.Initialize ();
}
```

20 kez yinelenen doku sonuçları:

![](part2-images/image10.png "Bu 20 kez yinelenen doku sonuçları")


## <a name="rendering-vertices-with-models"></a>Köşeleri modelleri ile işleme

Bizim düzlemi düzgün işleme, biz her şeyi birlikte görüntüleme modelleri yeniden ekleyebilirsiniz. İlk olarak, model kodunu yeniden ekleyeceğiz bizim `Game1.Draw` yöntemiyle (değiştirilmiş konumlar):

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

Ayrıca oluşturacağız bir `Vector3` içinde `Game1` bizim kameranın konumunu göstermek için. Bir alanı altında ekleyeceğiz bizim `checkerboardTexture` bildirimi:

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

Ardından, yerel kaldırmak `cameraPosition` değişkeni `DrawModel` yöntemi:

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

Benzer şekilde yerel kaldırmak `cameraPosition` değişkeni `DrawGround` yöntemi:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

Biz kodu çalıştırırsanız şimdi biz modelleri ve zemin aynı anda görebilirsiniz:

![](part2-images/image11.png "Modeller ve zemin aynı anda görüntülenir")

Biz kamera konumunu değiştirirseniz (gibi X değerini artırarak, bu durumda taşır kamera sola) değeri başından başlayarak ve modelleri etkiler görebiliriz:

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

Bu kod sonuçları şunlardır:

![](part2-images/image3.png "Bu kod bu görünümde sonuçları")

## <a name="summary"></a>Özet

Bu kılavuzda köşe dizi özel işleme gerçekleştirmek için nasıl kullanılacağı gösterilmiştir. Bu durumda, Damalı kat bizim köşe tabanlı işlemesi bir doku ile birleştirerek oluşturduğumuz ve `BasicEffect`, ancak kodu herhangi 3B işleme için temel olarak hizmet etmesi Burada sunulan. Temel köşe işleme ile aynı modellere karışabilir de gösterilmiştir.

## <a name="related-links"></a>İlgili bağlantılar

- [Damalı dosya (örnek)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [Projeyi (örnek)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
