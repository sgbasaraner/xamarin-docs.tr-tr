---
title: "CCTextureCache kullanarak doku önbelleğe alma"
description: "CocosSharp'ın CCTextureCache sınıfı, önbellek, düzenlemek ve içeriği kaldırma için standart bir yol sağlar. Gruplandırma ve dokuları atma işlemi basitleştirme tamamen RAM uygun değildir büyük oyunlar özellikle yararlıdır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1B5F3F85-9E68-42A7-B516-E90E54BA7102
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 365e343a55a208b63f4dc52999e8857b5f0ec1f4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="texture-caching-using-cctexturecache"></a>CCTextureCache kullanarak doku önbelleğe alma

_CocosSharp'ın CCTextureCache sınıfı, önbellek, düzenlemek ve içeriği kaldırma için standart bir yol sağlar. Gruplandırma ve dokuları atma işlemi basitleştirme tamamen RAM uygun değildir büyük oyunlar özellikle yararlıdır._

`CCTextureCache` Sınıftır CocosSharp oyun geliştirme önemli bir parçasıdır. Çoğu CocosSharp oyunları kullanım `CCTextureCache` açıkça, sayıda CocosSharp yöntemleri dahili kullansanız bile, nesne bir *paylaşılan* doku önbelleği.

Bu kılavuz kapsar `CCTextureCache` ve oyun geliştirme için önemli olmasının. Özellikle şunları içerir:

 - Önbelleğe alma önemlidir neden doku
 - Doku kullanım ömrü
 - SharedTextureCache kullanma
 - Geç AddImage ile ön yüklemeyi ve yükleme
 - Dokular atma


# <a name="why-texture-caching-matters"></a>Önbelleğe alma önemlidir neden doku

Doku önbelleğe alma önemli bir oyun geliştirmede doku yüklenmesi uzun süren bir işlemdir ve önemli miktarda RAM çalışma zamanında dokuları gerektiren konudur.

Herhangi dosya işlemiyle olduğu gibi diskten dokuları yüklenirken pahalı bir işlem olabilir. Yüklenen dosya (png ve jpg görüntüleri için olduğu gibi) sıkıştırması açılmış gibi işleme gerektiriyorsa doku yükleme fazladan zaman alabilir. Doku önbelleğe alma, uygulama dosyalarını diskten yüklemelisiniz sayısını azaltabilir.

Yukarıda belirtildiği gibi doku çalışma zamanı bellek, büyük bir miktarını da kaplar. Örneğin PNG dosyası boyutu yalnızca birkaç kilobayt olsa bile bir iPhone 6 (1344 x 750) çözümlemeye boyuta sahip bir arka plan görüntüsü 4 megabayt RAM – kaplar. Doku önbelleğe alma, bir uygulama içinde doku başvuruları paylaşmak için bir yöntem ve ayrıca farklı oyun durumları arasında geçiş sırasında tüm içeriği kaldırma için kolay bir yol sağlar.


# <a name="texture-lifespan"></a>Doku kullanım ömrü

Bir uygulamanın yürütme tüm uzunluğu için bellekte CocosSharp dokuları tutulması ya da kısa süreli olabilir. Belleği en aza indirmek için kullanım bir uygulama artık gerekmediğinde dokuları silmesi gerekir. Elbette, bu dokular atıldı ve yükleme süreleri artırabilir veya performans yükleri sırasında ölçeklenme bir sonraki bir zamanda yeniden yüklenen anlamına gelir. 

Doku genellikle yüklenmesini gerektirir kolaylığını bellek kullanımı ve yük zamanları arasında / çalışma zamanı performans. Küçük bir doku bellek miktarını kullanan oyunlar tüm dokuları gerektiği gibi bellek kullanmaya devam edebilir, ancak büyük oyunlar alan boşaltmak için doku unload gerekebilir.

Aşağıdaki diyagramda, gerektiğinde dokuları yükler ve bunları yürütme tüm uzunluğu için bellekte tutar basit bir oyun gösterilmektedir:

![](texture-cache-images/image1.png "Bu diyagramda, gerektiğinde dokuları yükler ve bunları yürütme tüm uzunluğu için bellekte tutar basit bir oyun gösterir")

İlk iki çubukları, hemen oyunun yürütme sırasında gerekli dokuları temsil eder. Aşağıdaki üç çubukları gerektiğinde yüklenen her düzeyi için doku temsil eder.

Oyun büyüktü varsa yeterli sonunda aygıt ve işletim sistemi tarafından sağlanan tüm RAM doldurmak için yeterli dokuları yüklemek. Oyun bunu çözmek için artık gerekli olmadığında doku verileri bellekten. Örneğin, aşağıdaki diyagramda, artık gerekli olmadığında, ardından İleri düzey için Level2Texture yükler bağlandığınızda Level1Texture bellekten oyun gösterir. Nihai sonuç, yalnızca üç dokuları herhangi bir anda bellekte tutulan şöyledir: 

![](texture-cache-images/image2.png "Yalnızca üç dokuları herhangi bir anda bellekte tutulan nihai sonucu olan")


Yukarıda gösterilen diyagramı doku bellek kullanımı kaldırarak azaltılabilir, ancak bir oynatıcı bir düzeye yeniden yürütme karar verirse bu ek yükleme süreleri gerektirebilir gösterir. De UITexture ve MainCharacter dokuları yüklenen ve hiçbir zaman yüklenmemiş olabilir. Bu, her zaman bellekte tutulmasını şekilde tüm düzeyler, bu dokuları gereklidir anlamına gelir. 


# <a name="using-sharedtexturecache"></a>SharedTextureCache kullanma

CocosSharp aralarında yüklenirken dokuları otomatik olarak önbelleğe `CCSprite` Oluşturucusu. Örneğin aşağıdaki kod, yalnızca bir doku örneği oluşturur:


```csharp
for (int i = 0; i < 100; i++)
{
    CCSprite starSprite = new CCSprite ("star.png");
    starSprite.PositionX = i * 32;
    this.AddChild (starSprite);
} 
```

CocosSharp otomatik olarak önbelleğe `star.png` çok sayıda oluşturmanın pahalı alternatif önlemek için doku aynı `CCTexture2D` örnekleri. Bu tarafından gerçekleştirilir `AddImage` çağrılan bir paylaşılan üzerinde `CCTextureCache` örneği, özellikle `CCTextureCache.SharedTextureCache.Shared`. Anlamak için nasıl `SharedTextureCache` kullanılan şu arama için işlevsel olarak aynıdır aşağıdaki kod bakabilirsiniz `CCSprite` bir dize parametresi ile Oluşturucu:


```

CCSprite starSprite = new CCSprite ();
 starSprite.Texture = CCTextureCache.SharedTextureCache.AddImage ("star.png");
```

`AddImage` denetler bağımsız değişken dosyası (Bu durumda `star.png`) zaten yüklendi. Bu durumda, önbelleğe alınan örneği döndürülür. Dosya sisteminden değil ardından yüklenir ve doku başvuru için dahili olarak depolanıyorsa sonraki `AddImage` çağrıları. Diğer bir deyişle `star.png` görüntü yalnızca yüklendiği bir kez ve sonraki çağrılar, hiçbir ek disk erişimi veya ek doku bellek gerektirir.


# <a name="lazy-loading-vs-pre-loading-with-addimage"></a>Yavaş yükleniyor vs. AddImage ile önceden yükleme

`AddImage` aynı yazılması için kod sağlar mı istenen doku zaten veya yüklenir. Gerekli kadar içeriğin Bunun anlamı yüklü değil; Ancak, bu da yüklenirken beklenmeyen içerik nedeniyle çalışma zamanında performans sorunlarına neden olabilir.

Örneğin, burada oyuncunun Silah yükseltilebilir oyun göz önünde bulundurun. Yükseltildiğinde, projectiles ve Silah görünür şekilde kullanılan yeni dokular kaynaklanan değişir. İçerik yüklendi yavaş ise sonra yükseltilen Silahlar ile ilişkili dokuları başlangıçta, ne zaman player yükseltmeleri alır ancak bunun yerine daha sonraki bir zamanda yüklenmeyecek. 

Bu oyunlar Orta yükleme oyuna neden olabilir *pop*, kısa ancak belirgin dondurma yürütmesinde olduğu. Bunu önlemek için kodun hangi dokuları ön ayarlama ve önceden yükleme gerekli olabilecek tahmin edebilirsiniz. Örneğin, aşağıdaki dokuları önceden yükleme için kullanılabilir:


```csharp
void PreLoadImages()
{
    var cache = CCTextureCache.SharedTextureCache;

    cache.AddImage ("powerup1.png");
    cache.AddImage ("powerup2.png");
    cache.AddImage ("powerup3.png");

    cache.AddImage ("enemy1.png");
    cache.AddImage ("enemy2.png");
    cache.AddImage ("enemy3.png");

    // pre-load any additional content here to 
    // prevent pops at runtime
} 
```

Bu ön yüklemeyi harcanan bellekte neden olabilir ve başlangıç süresini artırabilir. Örneğin, player bir tarafından temsil edilen gücü aslında hiç edinebilirsiniz `powerup3.png` doku gereksiz yere yüklenecek şekilde. Kuşkusuz bu RAM sığacak önyüklemesi içeriği genellikle en iyisidir şekilde oyunlar içinde olası pop önlemek ödeme yapmak için gerekli bir maliyeti olabilir.


# <a name="disposing-textures"></a>Dokular atma

Oyun minimum belirtim cihazda bulunandan daha fazla doku bellek gerektirmiyorsa sonra dokuları çıkarılması gerekmez. Öte yandan, büyük oyunlar yer açmak için yeni içerik doku bellek serbest bırakmanız gerekebilir. Örneğin bir oyun büyük miktarda bir ortam için doku depolama bellek kullanabilir. Ortam yalnızca belirli bir düzeyde yer kullanılıyorsa düzeyi sona erdiğinde sonra kaldırılmış olmalıdır.


## <a name="disposing-a-single-texture"></a>Tek bir doku atma

Tek bir doku kaldırma ilk gerekir arama `Dispose` yöntemi, daha sonra el ile kaldırma `CCTextureCache`.

Aşağıdaki alt doku yanı sıra bir arka plan hareketli grafik tamamen kaldırmak nasıl gösterir:


```csharp
void DisposeBackground()
{
    // Assuming this is called from a CCLayer:
    this.RemoveChild (backgroundSprite);

    CCTextureCache.SharedTextureCache.RemoveTexture (backgroundsprite.Texture);

    backgroundSprite.Texture.Dispose ();
} 
```

Az sayıda dokuları ancak bu postalarla büyük doku kümeleriyle ilgilenirken hataya yatkın olabilir, doğrudan dokuları atma etkili olabilir.

Dokular özel (paylaşılmayan) gruplandırılabilir `CCTextureCache` doku temizleme basitleştirmek için örnekleri.

Örneğin, bir örneği göz önünde bulundurun düzeyi özgü kullanarak içeriği önceden burada `CCTextureCache` örneği. `CCTextureCache` Örnek düzeyi tanımlama sınıfında tanımlanan (olabileceği gibi bir `CCLayer` veya `CCScene`):


```csharp
CCTextureCache levelTextures; 
```

`levelTextures` Örneği kullanılabilecek düzeyi özgü dokuları önceden yüklemek için: 


```

void PreloadLevelTextures(CCApplication application)
{
    levelTextures = new CCTextureCache (application);

    levelTextures.AddImage ("Background.png");
    levelTextures.AddImage ("Foreground.png");
    levelTextures.AddImage ("Enemy1.png");
    levelTextures.AddImage ("Enemy2.png");
    levelTextures.AddImage ("Enemy3.png");

    levelTextures.AddImage ("Powerups.png");
    levelTextures.AddImage ("Particles.png");
} 
```

Düzeyi sona erdiğinde, son olarak dokuları aynı anda aracılığıyla tüm bırakılmış olabilir `CCTextureCache`:


```csharp
void EndLevel()
{
    levelTextures.Dispose ();
    // Perform any other end-level cleanup
} 
```

Dispose yöntemi bu dokuları tarafından kullanılan bellek çıkışı temizleme tüm iç dokuları dispose. Birleştirme `CCTextureCache.Shared` bir düzeyi veya oyun modu belirli `CCTextureCache` tüm oyun ve bazı düzeyleri son, bu kılavuz, başında sunulan diyagrama benzer şekilde kaldırılıyor aracılığıyla kalıcı bazı dokuları sonuçlanıyor örneği: 

![](texture-cache-images/image2.png "Bir düzey veya oyun moda özgü CCTextureCache örneği sonuçlarında tüm oyun ve bazı düzeyleri son, bu kılavuz, başında sunulan diyagrama benzer şekilde kaldırılıyor aracılığıyla kalıcı bazı dokuları ile CCTextureCache.Shared birleştirme")




# <a name="summary"></a>Özet

Bu kılavuz, nasıl kullanılacağını gösterir `CCTextureCache` Bakiye bellek kullanımı ve çalışma zamanı performans sınıfı. `CCTexturCache.SharedTextureCache` açıkça bulunabilir veya yüklemek ve uygulama yaşam için doku önbelleğe için örtük olarak kullanıldığında, while `CCTextureCache` örnekleri, bellek kullanımını azaltmak için doku kaldırmak için kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [https://github.com/mono/CocosSharp](https://github.com/mono/CocosSharp)
- [/api/type/CocosSharp.CCTextureCache/](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
