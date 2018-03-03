---
title: "Döşenir CocosSharp ile kullanma"
description: "Döşenir güçlü ve esnektir ve oyunlar için resme ve İzometrik bölmesi oluşturmak için olgun uygulama eşler. CocosSharp döşeli'nın yerel dosya biçimi için yerleşik tümleştirme sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 5a469a372a9299712be7aef46c51f3d644946535
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="using-tiled-with-cocossharp"></a>Döşenir CocosSharp ile kullanma

_Döşenir güçlü ve esnektir ve oyunlar için resme ve İzometrik bölmesi oluşturmak için olgun uygulama eşler. CocosSharp döşeli'nın yerel dosya biçimi için yerleşik tümleştirme sağlar._

*Döşeli* uygulama oluşturmak için bir standart olan *döşeme eşlemeleri* oyun geliştirme kullanmak için. Bu kılavuz, mevcut bir .tmx dosyasını (döşeli tarafından oluşturulan dosya) alır ve CocosSharp oyunda kullanma size yol gösterecek. Özellikle bu kılavuzda ele alınacaktır:

 - Döşeme eşlemeleri amacı
 - .Tmx dosyalarıyla çalışma
 - Piksel resim oluşturma konuları
 - Çalışma zamanında döşeme özelliklerini kullanma

Zaman tamamlanmış biz aşağıdaki demo gerekir:

![](tiled-images/image1.png "Bu kılavuzdaki adımları uygulayarak oluşturduğunuz tanıtım uygulamasını")


# <a name="the-purpose-of-tile-maps"></a>Döşeme eşlemeleri amacı

Döşeme eşlemeleri on yılları için oyun geliştirmede var, ancak 2B oyunlarda esthetics ve verimlilik için hala yaygın olarak kullanılır. Döşeme eşlemeleri çok yüksek düzeyde verimlilik kullanımlarını döşeme kümelerinin – döşeme haritalar tarafından kullanılan kaynak görüntüsü aracılığıyla elde edebilirsiniz. Döşeme kümesi, tek bir dosyaya birleştirilmiş görüntüleri koleksiyonudur. Döşeme eşlemelerinin kullanılan görüntüleri döşeme kümeleri başvurmak olmakla birlikte, birden çok daha küçük resimleri içeren dosyaları hareketli grafik sayfaları da denir veya hareketli grafik oyun geliştirme eşler. Biz kendi Tanıtımı kullanmaya başlayacağınız döşeme kümesine bir kılavuz ekleyerek döşeme kümelerinin nasıl kullanıldığı görselleştirin:

![](tiled-images/image2.png "Gösteride kullanılacak döşeme kümesine bir kılavuz ekleyerek döşeme kümelerinin nasıl kullanıldığı bir görselleştirilmiş görünümü")

Döşeme eşlemeleri döşeme kümelerinden ayrı döşeme düzenleyin. Biz her döşeme harita döşeme kopyasını depolamak gerekmez unutmamalısınız ayarlama – yerine, birden çok döşeme eşlemesi aynı döşeme kümesi başvurusu yapabilir. Başka bir deyişle, taş kümesini yanı sıra döşeme eşlemeleri çok az bellek gerektirir. Gibi bir büyük oyun alanı oluşturmak için bile kullanıldığı zaman bu kutucuğu haritalar, çok sayıda oluşturmanıza olanak tanıyan bir [platformer kaydırma](http://en.wikipedia.org/wiki/Platform_game) ortamı. Aynı döşeme kümesini kullanarak olası düzenlemeleri gösterir:

![](tiled-images/image3.png "Bu görüntü aynı döşeme kümesini kullanarak olası düzenlemeleri gösterir")

![](tiled-images/image4.png "Bu görüntü aynı döşeme kümesini kullanarak olası düzenlemeleri gösterir")


# <a name="working-with-tmx-files"></a>.Tmx dosyalarıyla çalışma

.Tmx dosya biçimi olabilir döşeli uygulama tarafından oluşturulan bir XML dosyasıdır [döşeli Web sitesinde ücretsiz olarak karşıdan](http://www.mapeditor.org/). .Tmx dosya biçimi döşeme eşlemeleri ilgili bilgileri depolar. Genellikle bir oyun her düzeyi veya ayrı bir alan için bir .tmx dosyası sahip olacaktır.

Bu kılavuz CocosSharp varolan .tmx dosyalarını kullanma odaklanır; Ancak, ek öğreticileri dahil olmak üzere çevrimiçi bulunabilir [döşeli Eşleme Düzenleyicisi bu giriş](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838).

Unzip gerekir [içerik zip dosyası](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true) bizim oyunda kullanmak için. Dikkat edilecek ilk şey döşeme eşlemeleri hem .tmx dosyası (dungeon.tmx) yanı sıra kullanın veya döşeme tanımlayan daha fazla görüntü dosyaları (dungeon_1.png) veri kümesi değil. Çalışma zamanında .tmx yüklemek için bu dosyaları her ikisi de dahil oyun gerekiyor böylece ekleyin hem de projenin **içerik** klasörü (içinde barındırılan **varlıklar** Android projeleri klasöründe). Özellikle, dosya adında bir klasör ekleme **tilemaps** içinde **içerik** klasörü:

![](tiled-images/image5.png "Dosyaları içerik klasörüne içinde tilemaps adlı bir klasör ekleyin")

**Dungeon.tmx** dosya artık yüklenebilir çalışma zamanında bir `CCTileMap` nesnesi. Ardından, değişiklik `GameLayer` (veya eşdeğer kök kapsayıcı nesnesinin) içerecek şekilde bir `CCTileMap` örneği ve bir alt öğesi olarak eklemek için:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

Biz göreceğiz oyun çalıştırırsanız döşeme harita ekranın sol alt köşesinde görüntülenir:

![](tiled-images/image6.png "Döşeme harita oyun çalıştırırsanız, ekranın sol alt köşesinde görüntülenir")


# <a name="considerations-for-rendering-pixel-art"></a>Piksel resim oluşturma konuları

Piksel resim, video oyun geliştirme bağlamında genellikle el ile oluşturulan ve düşük çözünürlüklü görülür 2B visual resim başvuruyor. Piksel resim giderilirken olabilir oluşturma piksel resim kutucuğu kümeleri genellikle 16 veya 32 piksel genişlik ve yükseklik gibi düşük çözünürlüklü kutucukları eklemek için yoğun zamanı. Çalışma zamanında genişletilmiş değil, piksel resim genellikle Çoğu modern telefonlar ve tabletler için çok küçük.

Görüntülenen boyutlar burada hesabına ekleyeceğiz yapılan bir çağrı bizim oyunun GameAppDelegate.cs dosyasında göre ayarlayabilirsiniz `CCScene.SetDefaultDesignResolution`:


```csharp
 public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");

    CCSize windowSize = mainWindow.WindowSizeInPixels;

    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

Daha fazla bilgi için `CCSceneResolutionPolicy`, kılavuzumuzu bakın [CocosSharp çözümleri işleme](~/graphics-games/cocossharp/resolutions.md).

Biz oyun şimdi çalıştırırsanız bizim aygıtının tam ekran yukarı oyun Al göreceğiz:

![](tiled-images/image7.png "Tam ekran cihazın yukarı oyun Al")

Son olarak biz bizim döşeme haritaya düzgünleştirme devre dışı bırakmak isteyebilirsiniz. `Antialiased` Özelliği, seçeneğinde nesneler oluşturulurken bir bulanıklaştırma efekti uygulanır. Düzgünleştirme grafik nesneleri pixelated görünümünü azaltmak için faydalı olabilir, ancak kendi işleme yapıları da çıkarabilir. Özellikle, her bölme içerikleri düzgünleştirme bulanıklaştırır. Bununla birlikte, her bölme kenarlarına, hangi göze oturum bitişik döşeme karıştırma yerine tek tek döşeme yapar bulanık değil. Biz piksel resim oyunlar genellikle korumak için pixelated görünümlerini korumak unutmamalısınız bir *geçmiş* eşitleyerek.

Ayarlama `Antialiased` için `false` oluşturma sonra `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

Şimdi bizim döşeme harita bulanık görünmez:

![](tiled-images/image8.png "Artık kutucuk harita bulanık görünmez")


# <a name="using-tile-properties-at-runtime"></a>Çalışma zamanında döşeme özelliklerini kullanma

Şu ana kadar sahibiz bir `CCTileMap` .tmx dosyası yükleniyor ve, görüntüleme ancak biz ile etkileşim yolu yoktur. Özellikle, belirli döşeme (örneğin, bizim treasure Üstten kapaklı) özel mantık olması gerekir. Özel döşeme özellikleri ve bir kez çalışma zamanında tanımlanan bu özellikleri tepki vermek için çeşitli şekillerde algılamak nasıl adım adım.

Biz kod yazmadan önce biz bizim döşeme eşlemesi döşeli üzerinden özellikler eklemek gerekir. Bunu yapmak için döşeli programa dungeon.tmx dosyasını açın. Oyun projesinde kullanılmakta olan dosya açmaya emin olun.

Açık sonra treasure Üstten kapaklı döşemenin özelliklerini görüntülemek için seçin:

![](tiled-images/image9.png "Açık olan oluşturulduktan sonra özelliklerini görüntülemek için ayarlanmış parçasında treasure Üstten kapaklı seçin")

Treasure Üstten kapaklı özellikleri gösterilmezse treasure Üstten kapaklı üzerinde sağ tıklatın ve seçin **döşeme özellikleri**:

![](tiled-images/image10.png "Treasure Üstten kapaklı özellikleri gösterilmezse treasure Üstten kapaklı üzerinde sağ tıklayın ve döşeme Özellikler'i seçin")

Döşeli özellikleri, bir ad ve değer ile uygulanır. Bir özellik eklemek için tıklatın  **+**  düğmesini tıklatın, bir ad girin **IsTreasure**, tıklatın **Tamam**, değeri girin **true**: 

![](tiled-images/image11.png "Bir özellik eklemek için düğmesini tıklatın, IsTreasure adı girin, Tamam'ı tıklatın, ardından true değerini girin")

Özelliklerini değiştirdikten sonra .tmx dosyayı kaydetmeyi unutmayın.

Son olarak, bizim için yeni eklenen özelliği aramak için kod ekleyeceğiz. Biz her döngüye girer `CCTileMapLayer` (bizim harita 2 Katmanlar varsa), her satır ve sütun olması için tüm kutucukları aramak için aracılığıyla sonra `IsTreasure` özelliği:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

Kod çoğu kendinden açıklamalıdır ancak treasure döşeme işlenmesini tartışması gerekir. Bu durumda biz treasure chests tanımlanan döşeme kaldırmış olursunuz. Bu durum, treasure chests büyük olasılıkla çalışma zamanında etkili çakışma ve player açıldığında treasure içeriğini ikramiye için özel kod gerekir çünkü. Ayrıca, treasure duruma tepki gerekebilir (görünümünü değiştirme) açılır ve ne zaman ekrandaki tüm görünen yalnızca enemies engellenmediğinden için mantığı sahip olabilir.

Diğer bir deyişle, treasure Üstten kapaklı basit bir parçasında olmak yerine bir varlık olması yararlı olacak `CCTileMap`. Oyun varlıklar hakkında daha fazla bilgi için bkz: [CocosSharp varlıklarda Kılavuzu](~/graphics-games/cocossharp/entities.md).


# <a name="summary"></a>Özet

Bu kılavuz bir CocosSharp uygulamasına döşeli tarafından oluşturulan .tmx dosyaları yüklemek nasıl ele alınmaktadır. Düşük çözünürlüklü piksel resim için hesap için uygulama çözümlemesi değiştirme ve döşeme varlık örnekleri oluşturma gibi özel mantık gerçekleştirmek için özelliklerine göre bulmak nasıl gösterir.

## <a name="related-links"></a>İlgili bağlantılar

- [Döşeli Web sitesi](http://www.mapeditor.org/)
- [İçerik sıkıştırma](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
