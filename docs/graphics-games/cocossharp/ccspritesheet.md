---
title: Kare hızı CCSpriteSheet ile geliştirme
description: CCSpriteSheet birleştirme ve bir doku çok sayıda görüntü dosyaları kullanmak için işlevsellik sağlar. Doku sayısını azaltarak oyunun yükleme süreleri ve kare hızı artırabilir.
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 9b0f58554b26b1a5334970b8c1288234acbf8db7
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33922554"
---
# <a name="improving-frame-rate-with-ccspritesheet"></a>Kare hızı CCSpriteSheet ile geliştirme

_CCSpriteSheet birleştirme ve bir doku çok sayıda görüntü dosyaları kullanmak için işlevsellik sağlar. Doku sayısını azaltarak oyunun yükleme süreleri ve kare hızı artırabilir._

Birçok oyun sorunsuzca çalıştırması ve mobil donanımda hızlı bir şekilde yüklemek için en iyi duruma getirme çaba gerektirir. `CCSpriteSheet` Sınıfı tarafından CocosSharp oyunlar karşılaştı birçok ortak performans sorunlarına yardımcı olabilir. Bu kılavuz ortak performans sorunlarını ve bunları gidermek nasıl kapsar kullanarak `CCSpriteSheet` sınıfı.


## <a name="what-is-a-sprite-sheet"></a>Hareketli grafik sayfası nedir?

A *hareketli grafik sayfası*, hangi de başvurulabilir için farklı bir *doku atlas*, tek bir dosyada birden fazla görüntü birleştiren bir görüntü. Bu içerik yükleme süreleri yanı sıra çalışma zamanı performansını iyileştirebilir.

Örneğin, aşağıdaki üç ayrı görüntülere tarafından oluşturulan bir basit hareketli grafik sayfası görüntüdür. Ayrı ayrı görüntülere herhangi bir boyutta olabilir ve sonuçta elde edilen hareketli grafik sayfası tamamen doldurulması için gerekli değildir:

![](ccspritesheet-images/image1.png "Ayrı ayrı görüntülere herhangi bir boyutta olabilir ve sonuçta elde edilen hareketli grafik sayfası tamamen doldurulması için gerekli değildir")


### <a name="render-states"></a>Durumları işleme

Görsel CocosSharp nesneler (gibi `CCSprite`) köşe arabellekleri oluşturulmasını gerektiren MonoGame veya OpenGL gibi geleneksel grafik API'si işleme kod üzerinden işleme kodu basitleştirmek (kısmında özetlendiği gibi [3B grafik ile çizme MonoGame tepe](~/graphics-games/monogame/3d/part2.md) Kılavuzu). Kendi Basitlik rağmen CocosSharp ayarı maliyetini ortadan kaldırmaz *durumları işlemek*, işleme kodunun dokuları veya diğer işleme ile ilgili durumları geçmelidir sayısı olduğu.

CocosSharp'ın iç kodu her görsel öğeyi işler sırayla, geçerli görsel ağaç başlayarak geçme tarafından `CCScene`. Örneğin, aşağıdaki Sahne göz önünde bulundurun:

![](ccspritesheet-images/image2.png "Bu Sahne göz önünde bulundurun")

CocosSharp dört yıldız sırayla hale getiren:

![](ccspritesheet-images/image3.png "CocosSharp dört yıldız sırayla hale getiren")

Her itibaren `CCSprite` aynı doku kullanan CocosSharp gruplandırabilirsiniz tüm dört yıldız birlikte. Bu kod, çerçeve başına yalnızca bir işleme durumu atama (yıldız doku ataması) gerektirir. Bu çok verimli bir senaryodur.

Elbette, çok az oyunlar yalnızca bir görüntüyü kullanın. Aşağıdaki Sahne planet grafik sunar:

![](ccspritesheet-images/image4.png "Bu Sahne planet grafik sunar")

İdeal olarak CocosSharp ilk (örneğin yıldız) bir görüntü sonra başka bir görüntü (gezegen) kullanarak hareketli grafik geri kalanı kullanarak tüm hareketli grafik çizin:

![](ccspritesheet-images/image5.png "İdeal olarak tüm hareketli grafik bir görüntü ilk yıldız gibi diğer görüntü gezegen kullanarak hareketli grafik sonra geri kalanı kullanarak CocosSharp çizip")

Sıralama Yukarıdaki iki işlemek durumları gerektirir: Yıldız, birincisi ilk planet birinde.

Tüm `CCSprite` örnekleridir aynı alt `CCNode`, sonra da CocosSharp oluşturma durumu değişiklikleri azaltmak için çizim sırası en iyi duruma getirir. Açıksa, diğer yandan, `CCSprite` örnekleri CocosSharp işleme en iyi duruma alamıyor şekilde düzenlenmiştir (farklı varlık parçası oldukları gibi IF `CCNode` örnekleri), sonra da sipariş uygun olmayabilir. Aşağıdaki beş işleme eyaletinde kaynaklanan kötü olası çizim sırası gösterir:

![](ccspritesheet-images/image6.png "Bu beş işleme eyaletinde kaynaklanan kötü olası çizim sırası gösterir")

İşleme durumları çizim sırası görsel ağacını uyma gerekir çünkü en iyi duruma getirme zor olabilir `CCNode` örnekleri. Bu ağaç genellikle birlikte (visual bunların alt öğeleri içeren varlıklar gibi) çalışması kolay olacak şekilde yapılandırılmış veya nedeniyle istenen görsel düzen sanatçı tarafından tanımlandığı şekilde düzenlenmiştir.

Elbette, ideal durum birden fazla görüntü sahip olmasına rağmen bir tek işleme durumu sağlamaktır. CocosSharp oyunlar gerçekleştirmek bu tüm görüntüleri tek bir dosyada birleştirmek, ardından bu tek dosyası yükleniyor (kendi eşlik eden birlikte **.plist** dosyası) içine bir `CCSpriteSheet`. Kullanarak `CCSpriteSheet` sınıfı için çok sayıda görüntü varsa veya sahip olduğu çok oyunlar daha önemli hale karmaşık düzenler. 

### <a name="load-times"></a>Yükleme süreleri

Tek bir dosyada birden fazla görüntü birleştirme oyunun yükleme süreleri için pek çok geliştirir:

 - Tek bir dosyada birden fazla görüntü birleştirmek verimli paketleme aracılığıyla kullanılan piksel genel sayısını azaltabilir
 - Daha az dosyaları yükleme .png üstbilgileri ayrıştırılırken gibi daha az dosya başına yükünü anlamına gelir
 - Daha az dosyalarının yüklenmesi daha az arama DVD'ler ve geleneksel bilgisayarın sabit sürücüler gibi disk tabanlı medya önemlidir zaman gerektirir

## <a name="using-ccspritesheet-in-code"></a>CCSpriteSheet kod içinde kullanma

Oluşturmak için bir `CCSpriteSheet` örneği, kodu tedarik görüntü ve her çerçeve için kullanılacak görüntünün bölgeleri tanımlayan bir dosya gerekir. Görüntü olarak yüklenen bir **.png** veya **.xnb** dosyası (kullanıyorsanız [içerik ardışık düzen](~/graphics-games/cocossharp/content-pipeline/index.md)). Çerçeve tanımlama dosyası bir **.plist** el ile oluşturulan dosya veya *TexturePacker* (hangi aşağıda ele alacağız).

Örnek uygulaması, [burada indirilebilir](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/), oluşturur `CCSpriteSheet` gelen bir **.png** ve **.plist** aşağıdaki kodu kullanarak dosya:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

Bir kez yüklenen `CCSpriteSheet` içeren bir `List` , `CCSpriteFrame` – tüm sayfayı oluşturmak için kullanılan kaynak görüntüleri birine karşılık gelen her bir örnek örnekleri. Durumunda **SpriteSheetDemo** projesi `CCSpriteSheet` üç görüntülerini içerir. **.Plist** dosya Denetlenmekte Mac için Visual Studio veya hangi görüntüleri kullanılabilir olduğunu görmek için herhangi bir metin düzenleyicisi. Biz görüntülerseniz **.plist** dosyasını bir metin düzenleyicisinde şu üç çerçeve (anahtar adlarını vurgulamak için atlanmış bölümleri) görebilirsiniz:


```csharp
...
<dict>
    <key>frames</key>
    <dict>
        <key>farBackground.png</key>
        ...
        <key>foreground.png</key>
        ...
<key>forestBackground.png</key>
...
```

Biz kullanabilirsiniz `Find` çerçeveleri ada göre aşağıdaki gibi bulmak için yöntemi (kod atlanmış vurgulamak `CCSpriteFrame` kullanım):


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

Bu yana `CCSprite` Oluşturucusu gerçekleştirebileceğiniz bir `CCSpriteFrame` parametresi, kod hiçbir zaman sahip ayrıntılarını araştırmak `CCSpriteFrame`, hangi doku gibi kullanır veya ana hareketli grafik sayfasındaki görüntüsünün bölgesinden.


## <a name="creating-a-sprite-sheet-plist"></a>Hareketli grafik sayfası .plist oluşturma

Oluşturulan ve el ile düzenlenmiş bir xml tabanlı dosyasını .plist dosyasıdır. Benzer şekilde, görüntü düzenleme programlarında daha büyük bir dosyada birden çok dosya birleştirmek için kullanılabilir. Oluşturma ve hareketli grafik sayfaları çok zaman alabilir koruma itibaren dosyaları CocosSharp biçiminde dışarı aktarabilirsiniz TexturePacker program ele alacağız. TexturePacker ücretsiz ve "Pro" sürüm sunar ve Windows ve Mac OS için kullanılabilir. Bu kılavuzda kalan ücretsiz sürüm kullanılarak izlenebilir. 

TexturePacker olabilir [TexturePacker Web sitesinden indirdiğiniz](https://www.codeandweb.com/texturepacker). Açıldığında, TexturePacker yüklenen bir proje yok. Başlangıç ekranı hareketli grafik ekleyin, (diğer projeler oluşturulmuşsa) son projeleri açabilir ve hareketli grafik sayfası için kullanılacak biçimi seçin izin verir. CocosSharp Cocos2D veri biçimi kullanır:

![](ccspritesheet-images/image7.png "CocosSharp Cocos2D veri biçimi kullanır")

Görüntü dosyaları (gibi **.png**) TexturePacker için sürükle-bunları Windows Gezgini'nden Windows veya Mac üzerinde Bulucu bırakma eklenebilir Bir dosya eklendiğinde TexturePacker hareketli grafik sayfası önizlemesi otomatik olarak güncelleştirir:

![](ccspritesheet-images/image8.png "Bir dosya eklendiğinde TexturePacker hareketli grafik sayfası önizlemesi otomatik olarak güncelleştirir.")

Hareketli grafik sayfası vermek için tıklatın **Yayımla hareketli grafik sayfası** düğmesini tıklatın ve hareketli grafik sayfası için bir konum seçin. TexturePacker bir .plist dosyası ve bir görüntü dosyasına kaydeder.

Ortaya çıkan dosyalar kullanmak için .png ve .plist CocosSharp projeye ekleyin. Dosyaları CocosSharp projelerine ekleme hakkında daha fazla bilgi için bkz: [BouncingGame Kılavuzu](~/graphics-games/cocossharp/bouncing-game.md). Dosyalar eklendiğinde, bunların içine yüklenebilir bir `CCSpriteSheet` önceki yukarıdaki kodda gösterildiği:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

### <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>TexturePacker hareketli grafik sayfasını korumak için dikkat edilecek noktalar

Oyunlar geliştirilen gibi sanatçılar, ekleyin, kaldırın veya resim değiştirin. Herhangi bir değişiklik güncelleştirilmiş hareketli grafik sayfası gerektirir. Aşağıdaki konular hareketli grafik sayfası Bakımı kolaylaştırmaya:

 - Projenizi klasöründe özgün (dosyaları hareketli grafik sayfaları oluşturmak için kullanılan) tutun ve sürüm denetimi eklendiğinden emin olun. Bu dosyaları bir değişiklik yapıldığında hareketli grafik sayfası yeniden oluşturmanız gerekir.
 - Değil özgün dosyaları Visual Studio'ya Mac/Visual Studio için ekleme veya eklenirse, ayarlamak **yapı eylemi** için **hiçbiri**. Dosyaları eklenir ve platforma özgü olan **yapı eylemi**, sonuçta elde edilen uygulamanın dosya boyutunu gereksiz yere artıracaktır sonra.
 - Kullanmayı *akıllı klasörleri* TexturePacker içinde. Akıllı klasörleri içerilen tüm görüntüleri otomatik olarak hareketli grafik sayfasına ekleyin. Bu özellik çok zaman zaman geliştirme oyunları çok sayıda görüntü kaydedebilirsiniz. 

    ![](ccspritesheet-images/image9.png "Bu özellik çok sayıda görüntü çok zaman zaman geliştirme oyunları Kaydet")
 - Hareketli grafik sayfası doku boyutlarına takip. Bazı eski telefon donanım doku boyutu 2048 x 2048'den büyük desteklemez. Ayrıca, 2048 x 2048 32-bit görüntüsü yaklaşık 17 MB RAM – önemli miktarda bellek kullanır.
 - Ad çakışmaları olası; bu nedenle TexturePacker klasörleri hareketli grafik adlarında varsayılan olarak, içermez. Klasör eklemek için adları olup olmadığını veya geliştirme başında olmayan karar en iyisidir. Daha büyük oyunlar çakışmaları önlemek için klasör adları kullanmayı düşünmeniz gerekir. Klasör yollarını eklemek için tıklatın **Gelişmiş Göster** içinde **veri** bölümünde ve denetleme **Prepend klasör adı**. 

    ![](ccspritesheet-images/image10.png "Klasör yollarını eklemek için veri bölümünde Gelişmiş Göster tıklatıp Prepend klasör adını denetleyin")

## <a name="summary"></a>Özet

Bu kılavuz, oluşturma ve kullanma alınmaktadır `CCSpriteSheet` sınıfı. İçine yüklenen dosyalar oluşturma da kapsar `CCSpriteSheet` TexturePacker programını kullanarak örnekleri.

## <a name="related-links"></a>İlgili bağlantılar

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [Tam Demo (örnek)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
