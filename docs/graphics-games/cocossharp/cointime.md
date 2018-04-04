---
title: Para saati oyun ayrıntıları
description: Bu kılavuz para zaman oyundaki döşeme Eşlemleriyle Çalışma, varlıkları oluşturma, hareketli grafik animasyonu ve verimli çakışma uygulama da dahil olmak üzere uygulama ayrıntılarını açıklanır.
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 07a43dbf5f3095d1735d57fdbb13499532bfe415
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="coin-time-game-details"></a>Para saati oyun ayrıntıları

_Bu kılavuz para zaman oyundaki döşeme Eşlemleriyle Çalışma, varlıkları oluşturma, hareketli grafik animasyonu ve verimli çakışma uygulama da dahil olmak üzere uygulama ayrıntılarını açıklanır._

Para, iOS ve Android için oyun bir tam platformer saattir. Oyun bir düzeyinde bozuk para tümünün toplamak ve ardından çıkış kapı enemies ve engellerini kaçınarak ulaşmak için hedefidir.

![](cointime-images/image1.png "Bir düzeydeki bozuk para tümünün toplamak ve ardından çıkış kapı enemies ve engellerini kaçınarak ulaşmak için oyun belirtilir")

Bu kılavuz, aşağıdaki konuları kapsayan, para zaman içinde uygulama ayrıntıları ele alınmıştır:

- [Tmx dosyalarıyla çalışma](#working-with-tmx-files)
- [Yükleme düzeyi](#level-loading)
- [Varlıklar ekleme](#adding-new-entities)
- [Animasyonlu varlıklar](#animated-entities)


## <a name="content-in-coin-time"></a>Para sürede içerik

Para, tam bir CocosSharp proje nasıl organize gösteren bir örnek proje saattir. Para zaman yapısı eklenmesini ve içeriğin bakım basitleştirmek sağlamayı amaçlar. Kullandığı **.tmx** tarafından oluşturulan dosyaları [döşeli](http://www.mapeditor.org) düzeyleri ve animasyonları tanımlamak için XML dosyaları için. En az çaba ile değiştirerek veya yeni içerik ekleyerek elde edilebilir. 

Bu yaklaşım öğrenme ve deneme için etkili bir proje para zaman yaparken, ayrıca nasıl profesyonel oyunlar yansıtır yapılır. Bu kılavuzda ekleme ve içeriği değiştirme basitleştirmek için geçen yaklaşımlar bazıları açıklanmaktadır.


## <a name="working-with-tmx-files"></a>Tmx dosyalarıyla çalışma

Para zaman düzeyleri tarafından çıkış .tmx dosya biçimi kullanılarak tanımlanmış [döşeli](http://www.mapeditor.org) döşeme Eşleme Düzenleyicisi. Döşeli ile çalışma hakkında ayrıntılı bilgi için bkz: [kullanarak döşenir Cocos Sharp Kılavuzu ile](~/graphics-games/cocossharp/tiled.md). 

Her düzey bulunan kendi .tmx dosyasında tanımlanan **CoinTime/varlıklar/içeriğe/düzeyleri** klasör. Tüm para zaman düzeyleri tanımlanan bir tileset dosya paylaşımı **mastersheet.tsx** dosya. Bu dosya döşeme düz çakışma olup olmadığı veya döşeme bir varlık örneği tarafından değiştirilmesi gereken gibi her bölme için özel özellikleri tanımlar. Mastersheet.tsx dosyası yalnızca bir kez tanımlanabilir ve tüm düzeyleri arasında kullanılan özellikler sağlar. 


### <a name="editing-a-tile-map"></a>Döşeme harita düzenleme

Döşeme eşlemini düzenlemek için .tmx dosyasını çift veya döşeli Dosya menüsünden aracılığıyla açarak döşeli içinde .tmx dosyasını açın. Üç katman düzeyinde döşeme eşlemeleri içeren para süre: 

- **Varlıkları** – Bu katman, çalışma zamanında varlıkları ile değiştirilecek kutucuklar içeriyor. Örnekler player, bozuk para, enemies ve düzey son kapı içerir.
- **Terrain** – Bu katman düz çakışma genellikle olan kutucuklar içeriyor. Düz çakışma dönmeden olmadan bu döşeme gitmeye oynatıcı sağlar. Döşeme düz çakışma ile de duvar ve tavan davranamaz.
- **Arka plan** – arka plan katmanına statik arka planı olarak kullanılan kutucuklar içeriyor. Kamera parallax aracılığıyla derinliği görünümünü oluşturma düzeyi boyunca taşındığında bu katmana Kaymaz.

Biz daha sonra inceleyeceksiniz gibi bu üç katmanı tüm para zaman düzeyleri düzeyi yükleme kodu bekliyor.

#### <a name="editing-terrain"></a>Terrain düzenleme

Döşeme tıklayarak yerleştirilebilen **mastersheet** tileset ve kutucuğa tıklandığında eşleyin. Örneğin, bir düzeyinde yeni terrain boyamak için şunu yazın:

1. Terrain katmanı seçin
1. Çizmek için kutucuğa tıklayın
1. Veya itme tıklatıp kutucuğu boyamak için harita üzerinde sürükleyin

    ![](cointime-images/image2.png "1 çizmek için kutucuğa tıklayın")

Tileset sol üst tüm para zaman terrain içerir. Düz, terrain içeren **SolidCollision** ekranın sol bölme özelliklerinde gösterildiği gibi özelliği:

![](cointime-images/image3.png "Ekranın sol bölme özelliklerinde gösterildiği gibi düz, terrain SolidCollision özelliği içerir")

#### <a name="editing-entities"></a>Varlıkları düzenleme

Varlıklar eklenemez veya – terrain gibi bir düzeyinden kaldırılamaz. **Mastersheet** tileset bunlar sağa kaydırma olmadan görünmeyebilir şekilde tüm varlıklar hakkında yarı yarıya yatay olarak yerleştirilen vardır:

![](cointime-images/image4.png "Bunlar sağa kaydırma olmadan görünmeyebilir mastersheet tileset tüm varlıklar hakkında yarı yarıya yatay olarak yerleştirilen sahiptir, bu nedenle")

Yeni varlıklar yerleştirilmelidir **varlıklar** katmanı.

![](cointime-images/image5.png "Yeni varlıklar varlıklar katmanda yerleştirilmelidir")

CoinTime kod arar **EntityType** ne zaman bir düzeye yüklendiği varlıklar tarafından değiştirilmelidir döşeme tanımlamak için: 

![](cointime-images/image6.png "Varlıklar tarafından değiştirilmelidir döşeme tanımlamak için bir düzeye yüklendiğinde CoinTime kod EntityType için görünür.")

Dosyası değiştirildiğinde ve kaydedilen sonra değişiklikler otomatik olarak proje yerleşik mi, yoksa çalıştırın ve varsa görünür:

![](cointime-images/image7.png "Dosyası değiştirildiğinde ve kaydedilen sonra değişiklikler otomatik olarak proje yerleşik çalıştırın ve olup olmadığını gösterir")


### <a name="adding-new-levels"></a>Yeni düzeyleri ekleme

Para zaman düzeyleri ekleme işlemini hiçbir kod değişikliklerini ve yalnızca birkaç küçük değişiklikler projeye gerektirir. Yeni bir düzeyi eklemek için:

1. Açık düzey klasör konumunda <**CoinTime kök > \CoinTime\Assets\Content\levels**
1. Kopyalayıp düzeylerinden birini gibi **level0.tmx**
1. Varolan düzeyleriyle düzeyi numara sırası gibi devam eder yeni .tmx dosyasını yeniden adlandırın **level8.tmx**
1. Visual Studio veya Mac için Visual Studio'da yeni .tmx dosyası Android düzeyleri klasörüne ekleyin. Dosya kullandığını doğrulayın **AndroidAsset** derleme eylemi.

    ![](cointime-images/image8.png "Dosya AndroidAsset yapı eylemi kullandığını doğrulayın")

1. Yeni .tmx dosyası iOS düzeyleri klasörüne ekleyin. Özgün konumuna dosyasından bağlamak ve bunu kullandığını doğrulamak mutlaka **BundleResource** derleme eylemi.

    ![](cointime-images/image9.png "Özgün konumuna dosyasından bağlamak ve BundleResource yapı eylemi kullandığını doğrulamak emin olun")

Yeni düzeyi düzeyi seçin ekranında düzeyi 9 görünmesi gereken (düzey dosya adları 0'da başlatılır, ancak düzey düğmeleri numarası 1 ile başlayan):

![](cointime-images/image10.png "Yeni düzeyi düzeyi seçin ekranında düzeyi 9 düzeyi dosya adları 0 başlangıcında olarak görünür, ancak düzey düğmeleri 1 rakamla başlayamaz")


## <a name="level-loading"></a>Yükleme düzeyi

Daha önce gösterildiği gibi yeni düzeyleri gerektiren herhangi bir değişiklik kodda – doğru adlı ve eklenen oyun düzeyleri otomatik olarak algılar. **düzeyleri** doğru yapı eylemi klasörüyle (**BundleResource**veya **AndroidAsset**).

Düzey sayısını belirlemek için mantığı yer `LevelManager` sınıfı. Bir örneği olduğunda `LevelManager` (singleton deseni kullanarak), oluşturulan `DetermineAvailbleLevels` yöntemi çağrılır:


```csharp
private void DetermineAvailableLevels()
{
    // This game relies on levels being named "levelx.tmx" where x is an integer beginning with
    // 1. We have to rely on MonoGame's TitleContainer which doesn't give us a GetFiles method - we simply
    // have to check if a file exists, and if we get an exception on the call then we know the file doesn't
    // exist. 
    NumberOfLevels = 0;
    while (true)
    {
        bool fileExists = false;
        try
        {
            using(var stream = TitleContainer.OpenStream("Content/levels/level" + NumberOfLevels + ".tmx"))
            {
            }
            // if we got here then the file exists!
            fileExists = true;
        }
        catch
        {
            // do nothing, fileExists will remain false
        }
        if (!fileExists)
        {
            break;
        }
        else
        {
            NumberOfLevels++;
        }
    }
}
```

CocosSharp sağlamaz platformlar arası bir yaklaşım Bel gerekir böylece dosyaları yoksa algılamak için `TitleContainer` sınıfı bir akış açılmaya çalışıldı. Bir akış açmak için kod bir özel durum, ardından dosyayı oluşturursa var ve while döngüsü sonu. Döngü tamamlandıktan sonra `NumberOfLevels` özelliği bildirir kaç geçerli düzey projesinin bir parçasıdır.

`LevelSelectScene` Sınıfını kullanan `LevelManager.NumberOfLevels` oluşturmak için kaç tane düğmeleri belirlemek için `CreateLevelButtons` yöntemi:


```csharp
private void CreateLevelButtons()
{
    const int buttonsPerPage = 6;
    int levelIndex0Based = buttonsPerPage * pageNumber;
    int maxLevelExclusive = System.Math.Min (levelIndex0Based + 6, LevelManager.Self.NumberOfLevels);
    int buttonIndex = 0;
    float centerX = this.ContentSize.Center.X;
    const float topRowOffsetFromCenter = 16;
    float topRowY = this.ContentSize.Center.Y + topRowOffsetFromCenter;
    for (int i = levelIndex0Based; i < maxLevelExclusive; i++)
    {
        ...
    }
}
```

`NumberOflevels` Özelliği hangi düğmeleri oluşturulması gerektiğini belirlemek için kullanılır. Bu kod kullanıcı şu anda görüntüleme ve yalnızca en fazla sayfa başına altı düğmeleri oluşturur hangi sayfasında göz önünde bulundurur. Düğme tıklatıldığında, çağrı örnekleri `HandleButtonClicked` yöntemi:


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

Bu yöntem atar `CurrentLevel` tarafından kullanılan özellik `GameScene` bir düzeye yüklenirken. Ayar sonra `CurrentLevel`, `GoToGameScene` yöntemi oluşturulur, Sahne gelen geçiş `LevelSelectScene` için `GameScene`.

`GameScene` Oluşturucu çağrıları `GoToLevel`, düzey yükleme mantığı gerçekleştirir:


```csharp
private void GoToLevel(int levelNumber)
{
    LoadLevel (levelNumber);
    CreateCollision();
    ProcessTileProperties ();
    touchScreen = new TouchScreenInput(gameplayLayer);
    secondsLeft = secondsPerLevel;
}
```

Sonraki biz adlı yöntemleri göz atın `GoToLevel`.


### <a name="loadlevel"></a>LoadLevel

`LoadLevel` Yöntemdir .tmx dosyası yükleniyor ve eklemeyi sorumlu `GameScene`. Bu yöntem çakışma veya varlıklar gibi etkileşimli tüm nesneler oluşturmaz – yalnızca görsel olarak da adlandırılan düzeyi için oluşturduğu *ortam*.


```csharp
private void LoadLevel(int levelNumber)
{
    currentLevel = new CCTileMap ("level" + levelNumber + ".tmx");
    currentLevel.Antialiased = false;
    backgroundLayer = currentLevel.LayerNamed ("Background");
    // CCTileMap is a CCLayer, so we'll just add it under all entities
    this.AddChild (currentLevel);
    // put the game layer after
    this.RemoveChild(gameplayLayer);
    this.AddChild(gameplayLayer);
    this.RemoveChild (hudLayer);
    this.AddChild (hudLayer);
}
```

`CCTileMap` Oluşturucusu için geçirilen düzey numarası kullanılarak oluşturulan bir dosya adı geçen `LoadLevel`. Oluşturma ve bunlarla çalışma hakkında daha fazla bilgi için `CCTileMap` örnekleri, görmek [kullanarak döşenir CocosSharp Kılavuzu ile](~/graphics-games/cocossharp/tiled.md).

Şu anda CocosSharp katmanları olmadan kaldırarak ve bunları kendi üst öğeye yeniden ekleyerek yeniden sıralama izin vermiyor `CCScene` (olduğu `GameScene` bu durumda), son birkaç satır yönteminin katmanları yeniden sıralamak için gereklidir.


### <a name="createcollision"></a>CreateCollision

`CreateCollision` Yöntemi yapıları bir `LevelCollision` gerçekleştirmek için kullanılan örnek *düz çakışma* player ve ortam arasında.


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

Bu çakışma olmadan player düzey-döner ve oyun unplayable olacaktır. Düz çakışma yere yol oynatıcı sağlar ve duvarları taramasını veya yukarı tavan atlama player önler.

Çakışma para zaman içinde ek kod – döşeli dosyaları yalnızca değişiklikler ile eklenebilir. 


### <a name="processtileproperties"></a>ProcessTileProperties

Bir düzey yüklenir ve çakışma oluşturulur, sonra `ProcessTileProperties` döşeme özelliğe göre mantığı gerçekleştirmek üzere çağırılır. Para saati içeren bir `PropertyLocation` özellikleri ve bu özellikleri içeren kutucuğa koordinatlarını tanımlamak için yapısı:


```csharp
public struct PropertyLocation
{
    public CCTileMapLayer Layer;
    public CCTileMapCoordinates TileCoordinates;
    public float WorldX;
    public float WorldY;
    public Dictionary<string, string> Properties;
}
```

Bu yapı oluşturmak için kullanılan varlık örnekleri oluşturma ve gereksiz döşemeleri kaldırma `ProcessTileProperties` yöntemi:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```

Foreach döngüsü anahtar ya da ise denetimi, her bir kutucuğu özellik değerlendirir `EntityType` veya `RemoveMe`. `EntityType` bir varlık örneğinin oluşturulması gerektiğini belirtir. `RemoveMe` Döşeme çalışma zamanında tamamen kaldırılması gerektiğini belirtir.

Bir özellik varsa `EntityType` anahtar bulunursa, ardından `TryCreateEntity` adlı, eşleşen özelliğini kullanarak bir varlık oluşturmak için hangi denemeleri `EntityType` anahtarı:


```csharp
private bool TryCreateEntity(string entityType, float worldX, float worldY)
{
    CCNode entityAsNode = null;
    switch (entityType)
    {
    case "Player":
        player = new Player ();
        entityAsNode = player;
        break;
    case "Coin":
        Coin coin = new Coin ();
        entityAsNode = coin;
        coins.Add (coin);
        break;
    case "Door":
        door = new Door ();
        entityAsNode = door;
        break;
    case "Spikes":
        var spikes = new Spikes ();
        this.damageDealers.Add (spikes);
        entityAsNode = spikes;
        break;
    case "Enemy":
        var enemy = new Enemy ();
        this.damageDealers.Add (enemy);
        this.enemies.Add (enemy);
        entityAsNode = enemy;
        break;
    }
    if(entityAsNode != null)
    {
        entityAsNode.PositionX = worldX;
        entityAsNode.PositionY = worldY;
        gameplayLayer.AddChild (entityAsNode);
    }
    return entityAsNode != null;
}
```


## <a name="adding-new-entities"></a>Varlıklar ekleme

Para zaman oyun nesnelerine için varlık deseni kullanır (içinde ele [CocosSharp varlıklarda Kılavuzu](~/graphics-games/cocossharp/entities.md)). Tüm varlıklar devralınmalıdır `CCNode`, başka bir deyişle, bunlar eklenebilir alt olarak `gameplayLayer`.

Her bir varlık türü bir liste veya tek örnek aracılığıyla da başvurulmaktadır. Örneğin, `Player` tarafından başvurulan `player` alan ve tüm `Coin` örnekleri başvurulan bir `coins` listesi. Varlıkları doğrudan başvurular tutma (aralarında başvuran aksine `gameLayer.Children` listesi) varlıkları okumak daha kolay erişilir ve potansiyel olarak pahalı tür atama ortadan kaldıran kod yapar.

Var olan kodu varlık türleri bir dizi yeni varlıklar oluşturmak nasıl örnekleri olarak sağlar. Aşağıdaki adımlarda, yeni bir varlık oluşturmak için kullanılabilir:


### <a name="1---define-a-new-class-using-the-entity-pattern"></a>1 - varlık modeli kullanarak yeni bir sınıf tanımlama

Öğesinden devralınan bir sınıf oluşturmak için bir varlık oluşturmak için tek gereksinim olan `CCNode`. Çoğu varlık gibi bazı visual sahip bir `CCSprite`, hangi eklenecek kurucusu varlıkta alt olarak.

CoinTime sağlar `AnimatedSpriteEntity` sınıfını animasyonlu varlıklar oluşturulmasını basitleştirir. Animasyon kapsamında daha ayrıntılı olarak [animasyonlu varlıkların bölüm](#animated-entities).


### <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2 – yeni bir giriş TryCreateEntity switch ifadesine ekleyin

Yeni varlık örneklerini içinde başlatılamaz `TryCreateEntity`. Varlık çerçevesi her mantığı çakışma, AI veya okuma giriş gibi gerektiriyorsa sonra `GameScene` nesnesine başvuru tutmanız gerekir. Birden çok örneği gerekirse (gibi `Coin` veya `Enemy` örnekleri), ardından yeni `List` eklenmelidir `GameScene` sınıfı.


### <a name="3--modify-tile-properties-for-the-new-entity"></a>3 – yeni bir varlık için döşeme özelliklerini değiştirme

Kod yeni varlık oluşturmayı destekler sonra yeni varlık tileset eklenmesi gerekir. Herhangi bir düzeye açarak tileset düzenlenebilir `.tmx` dosya. 

Tileset .tmx ayrı depolanan **mastersheet.tsx** dosya düzenlenip düzenlenemeyeceğini önce onu alınması gerekir:

![](cointime-images/image11.png "Düzenlenebilir önce içe aktarılmalıdır tileset .tsx dosyasından ayrı depolanır, böylece")

Seçilen kutucukları özellikleri alınırken, düzenlenebilir ve EntityType eklenebilir sonra:

![](cointime-images/image12.png "Seçilen kutucukları özellikleri alınırken, düzenlenebilir sonra EntityType eklenebilir")

Yeni eşleşecek şekilde özelliği oluşturulduktan sonra değerini ayarlanabilir `case` içinde `TryCreateEntity`:

![](cointime-images/image13.png "TryCreateEntity yeni durumda eşleşecek şekilde özelliği oluşturulduktan sonra değerini ayarlanabilir")

Tileset değiştirildikten sonra dışarı aktarılmalıdır – bu değişiklikler diğer tüm düzeyleri için kullanılabilir hale getirir:

![](cointime-images/image14.png "Tileset değiştirildikten sonra verilmelidir")

Tileset var olanın üzerine yaz **mastersheet.tsx** tileset:

![](cointime-images/image15.png "He tileset varolan mastersheet.tsx tileset üzerine yazmanız gerekir")


## <a name="entity-tile-removal"></a>Varlık döşeme kaldırma

Döşeme harita oyun yüklendiğinde, tek tek döşeme statik nesneleridir. Taşıma gibi özel davranış varlıklar gerektirir, varlıkları oluşturulduğunda para zamanı kodu döşeme ortadan kaldırır.

`ProcessTileProperties` kullanarak varlıklar oluşturan döşeme kaldırmak için mantığı içeren `RemoveTile` yöntemi:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            ...
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        ...
    }
}
```

Bu otomatik kaldırma döşeme bozuk para ve enemies gibi tileset içinde yalnızca bir kutucuğu kaplar varlıklar için yeterli olur. Daha büyük varlıklar ek mantık ve özellikleri gerektirir.

Kapının tamamen çizilecek iki kutucuk gerektirir:

![](cointime-images/image16.png "Kapının tamamen çizilecek iki kutucuk gerektirir")

Bir varlık oluşturmak için özellikleri kapının alt parçasında içerir (**EntityType** kümesine **kapı**):

![](cointime-images/image17.png "Bir varlık için kapı EntityType kümesi oluşturmak için özellikleri kapının alt parçasında içerir")

Kapı örneği oluşturulduğunda, yalnızca alt parçasında kapı kaldırıldığı ek mantık çalışma zamanında üst kutucuğunu kaldırma için gereklidir. Üst kutucuğunda bir **RemoveMe** özelliğini **true**:

![](cointime-images/image18.png "Üst bölme bir RemoveMe özelliği ayarlanmış true")



Bu özellik döşemeleri kaldırmak için kullanılan `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        ...
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```


## <a name="entity-offsets"></a>Varlık uzaklıkları

Döşeme oluşturduğu varlıklar, varlık center kutucuğa Merkezi ile hizalayarak konumlandırılır. Daha büyük varlıklar, ister `Door`, doğru yerleştirilmesi için ek özellikler ve mantığı kullanın. 

Tanımlar alt kapı döşeme `Door` varlık yerleştirme belirten bir **YOffset** değeri 4. Bu özellik olmadan `Door` örneği döşeme merkezinde yerleştirilir:

![](cointime-images/image19.png "Bu özellik olmadan kapı örneği döşeme merkezinde yerleştirilir")

 

Bunu uygulayarak düzeltilinceye **YOffset** değeri `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            ...
        }
...
    }
}
```


## <a name="animated-entities"></a>Animasyonlu varlıklar

Para zaman birkaç animasyonlu varlıkları içerir. `Player` Ve `Enemy` varlıklarını oynatmak ilerlemesi animasyonları ve `Door` varlık tüm bozuk para toplandığında açılış animasyonun oynar.


### <a name="achx-files"></a>.achx dosyaları

Para zaman animasyonları .achx dosyalarında tanımlanır. Her animasyon arasında tanımlanan `AnimationChain` tanımlanan aşağıdaki animasyonda gösterildiği gibi etiketler **propanimations.achx**:


```xml
<AnimationChain>
  <Name>Spikes</Name>
  <ColorKey>0</ColorKey>
  <Frame>
    <FlipHorizontal>false</FlipHorizontal>
    <FlipVertical>false</FlipVertical>
    <TextureName>..\images\mastersheet.png</TextureName>
    <FrameLength>0.1</FrameLength>
    <LeftCoordinate>1152</LeftCoordinate>
    <RightCoordinate>1168</RightCoordinate>
    <TopCoordinate>128</TopCoordinate>
    <BottomCoordinate>144</BottomCoordinate>
    <RelativeX>0</RelativeX>
    <RelativeY>0</RelativeY>
  </Frame>
</AnimationChain> 
```

Bu animasyon yalnızca statik görüntü görüntülenirken depo varlıkta kaynaklanan tek bir çerçeve içerir. Tek veya birden çok çerçeve animasyonları görüntülemek isteyip varlıklar .achx dosyalarını kullanabilirsiniz. Ek çerçeve kodundaki değişiklikleri gerek kalmadan .achx dosyalara eklenebilir. 

Çerçeveler tanımlamak görüntülemek için hangi görüntü `TextureName` parametre ve görüntüsünü koordinatlarını `LeftCoordinate`, `RightCoordinate`, `TopCoordinate`, ve `BottomCoordinate` etiketler. Bu da kullanılıyor – görüntü animasyonda çerçeve piksel koordinatlarını temsil **mastersheet.png** bu durumda.

![](cointime-images/image20.png "Bu görüntüdeki animasyonun çerçeve piksel koordinatlarını temsil eder")

`FrameLength` Özelliği bir animasyon çerçevede görüntülenmesi gereken saniye sayısını tanımlar. Tek çerçeve animasyonları bu değeri yoksayar.

Diğer tüm AnimationChain özellikleri .achx dosyasındaki para zamanına göre göz ardı edilir.


### <a name="animatedspriteentity"></a>AnimatedSpriteEntity

Animasyon mantığı bulunduğu `AnimatedSpriteEntity` temel sınıf olarak kullanılan çoğu varlık için veren sınıfı `GameScene`. Aşağıdaki işlevleri sağlar:

 - Yüklenmesi `.achx` dosyaları
 - Yüklenen animasyonların animasyon önbelleği
 - Animasyonun görüntüleme CCSprite örneği
 - Geçerli çerçeve değiştirmek için mantığı

Ani Oluşturucusu yüklemek ve animasyonları kullanmak nasıl basit bir örnek sağlar:


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

**PropAnimations.achx** Oluşturucusu dizini tarafından bu animasyon erişen şekilde bir animasyon yalnızca içerir. Birden çok animasyon .achx dosyasını içeren sonra animasyonları gösterildiği gibi adıyla başvurulabilir `Enemy` Oluşturucusu:


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


## <a name="summary"></a>Özet

Bu kılavuz, uygulama ayrıntılarını para süreyi kapsar. Para zaman tam oyun olmasını oluşturuldu, ancak ayrıca kolayca değiştiren ve genişletilmiş bir projedir. Okuyucular yeni düzeyleri ekleme ve daha fazla para zaman nasıl uygulandığını anlamak için yeni varlıklar oluşturma zaman yapmayı değişiklikler düzeyleri için harcadıkları önerilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Oyun proje (örnek)](https://developer.xamarin.com/samples/mobile/CoinTime/)
