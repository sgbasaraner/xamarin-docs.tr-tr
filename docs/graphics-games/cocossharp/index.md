---
title: CocosSharp
description: Bu belge çeşitli ilgili makalelerin bağlantısı için CocosSharp ile oyun geliştirme.
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: a188863cf57706e3f9dd6c8f4d2d3e60b2591e0b
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="cocossharp"></a>CocosSharp

_CocosSharp C# ve F # kullanarak 2B oyunlar oluşturmaya yönelik bir kitaplıktır. Popüler Cocos2D altyapısı .NET bağlantı noktasıdır._

## <a name="introduction-to-cocossharp"></a>CocosSharp giriş

Platformlar arası oyunlar yapmak için teknolojisi CocosSharp 2B oyun altyapısı sağlar. Desteklenen platformlar tam listesi için bkz: [CocosSharp wiki github'da](https://github.com/mono/CocosSharp/wiki).
CocosSharp de F # ile tam olarak işlevsel olmasına rağmen bu kılavuzlara C# kod örnekleri için kullanın.

CocosSharp çekirdek tarafından sağlanan [MonoGame framework](http://www.monogame.net/), kendisini bir platformlar arası, donanım hızlandırılmış API varlıklar içeri aktarmak için grafik, ses, oyun durum yönetimi, giriş ve içerik işlem hattı sağlama.
CocosSharp iyi 2B oyunlar için uygun bir verimli Soyutlama Katmanı ' dir.
Ayrıca, oyunlar karmaşıklığı büyüdükçe büyük oyunlar kendi çekirdek kitaplıkları dışında kendi iyileştirmeler gerçekleştirebilirsiniz. Diğer bir deyişle, geliştiricilerin hızla oyun boyutu veya karmaşıklık sınırlamaksızın başlamak etkinleştirme kullanım kolaylığı ve performans, bir karışımını CocosSharp sağlar.

Bu uygulamalı video basit bir platformlar arası CocosSharp oluşturma oyun gösterir:

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

Bu kılavuzda, Oyun içeriği, oyun, ekleme oyun mantığı ve daha fazlasını oluşturmak için kullanılan çeşitli görsel öğeleri ile çalışma konusunda da dahil olmak üzere BouncingGame açıklanır.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Fruity döner oyun](~/graphics-games/cocossharp/fruity-falls.md)

![Fruity döner oyun ekran](images/fruity-falls.png "Fruity döner oyun ekran görüntüsü")

Bu kılavuzda, ortak CocosSharp ve fizik, içerik yönetimi, oyun durumu ve oyun tasarım gibi oyun geliştirme kavramları kapsayan Fruity döner oyun açıklanmaktadır.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Para zaman oyun](~/graphics-games/cocossharp/cointime.md)

![Zaman oyun ekran para](images/cointime.png "para zaman oyun ekran görüntüsü")

Para, iOS ve Android için oyun bir tam platformer saattir. Oyun bir düzeyinde bozuk para tümünün toplamak ve ardından çıkış kapı enemies ve engellerini kaçınarak ulaşmak için hedefidir.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[Geometri CCDrawNode ile çizme](~/graphics-games/cocossharp/ccdrawnode.md)

![İle CCDrawNode çizilen şekilleri](images/ccdrawnode.png "CCDrawNode ile çizilmiş şekiller")

CCDrawNode satırları, daire ve üçgen gibi temel çizim nesneler için yöntemleri sağlar.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[CCAction ile Animasyon](~/graphics-games/cocossharp/ccaction.md)

![CCAction animasyon](images/ccaction.png "A CCAction animasyon")

`CCAction` CocosSharp nesnelere animasyon ekleme için kullanılan temel bir sınıftır. Bu kılavuz yerleşik kapsar `CCAction` konumlandırma, ölçeklendirme ve döndürme gibi ortak görevler için uygulamaları. Ayrıca devralarak özel uygulamalar oluşturmak nasıl göründüğünü `CCAction`.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[CocosSharp ile Tiled Uygulamasını Kullanma](~/graphics-games/cocossharp/tiled.md)

![Bir düzey oyun](images/tiled.png "bir oyun düzeyi")

Döşenir güçlü ve esnektir ve oyunlar için resme ve İzometrik bölmesi oluşturmak için olgun uygulama eşler. CocosSharp döşeli'nın yerel dosya biçimi için yerleşik tümleştirme sağlar.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[CocosSharp’taki Varlıklar](~/graphics-games/cocossharp/entities.md)

![Oyun gelen savaş Gemisi](images/entities.png "savaş Gemisi oyun gelen")

Varlık düzeni oyun kod düzenlemek için güçlü bir yoludur. Okunabilirliğini artırır, kod korumak daha kolay hale getirir ve yerleşik üst/alt işlevselliğini kullanır.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[Birden çok çözümleri CocosSharp işleme](~/graphics-games/cocossharp/resolutions.md)

![Ekran çözünürlüğü temsil eden bir kılavuz](images/resolutions.png "ekran çözünürlüğü temsil eden bir kılavuz")

Bu kılavuz çeşitli çözümler cihazlarda düzgün görüntülemek oyun geliştirmek için CocosSharp çalışmak nasıl gösterir.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[CocosSharp İçerik Ardışık Düzeni](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

İçerik ardışık düzen içeriği en iyi duruma getirme ve belirli donanım veya belirli oyun geliştirme çerçeveleri ile yüklenebilecek şekilde biçimlendirmek için oyun geliştirilmesinde genellikle kullanılır.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[Kare hızı CCSpriteSheet ile geliştirme](~/graphics-games/cocossharp/ccspritesheet.md)

![Bir CCSpriteSheet ağacından](images/ccspritesheet.png "bir CCSpriteSheet ağacından")

`CCSpriteSheet` birleştirme ve bir doku çok sayıda görüntü dosyaları kullanmak için işlevsellik sağlar. Doku sayısını azaltarak oyunun yükleme süreleri ve kare hızı artırabilir.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[Doku CCTextureCache kullanarak önbelleğe alma](~/graphics-games/cocossharp/texture-cache.md)

![CocosSharp görüntüleri nasıl önbelleğe alınacağını bir temsilini](images/texture-cache.png "CocosSharp görüntüleri nasıl önbelleğe alınacağını bir temsilini")

CocosSharp'ın `CCTextureCache` sınıfı düzenlemek, önbellek ve içeriği kaldırma için standart bir yol sağlar. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[2B matematik CocosSharp ile](~/graphics-games/cocossharp/math.md)

![Görüntüyü Döndürülmüş](images/math.png "Döndürülmüş görüntü")

Bu kılavuz, oyun geliştirmeye yönelik 2B matematik kapsar. Ortak oyun geliştirme görevleri gerçekleştirmek nasıl göstermek için CocosSharp kullanır ve bu görevlerin arkasında matematik açıklar.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[Performans ve CCRenderTexture ile görsel efektler](~/graphics-games/cocossharp/ccrendertexture.md)

![Hareketli grafik oyun gelen](images/ccrendertexture.png "oyun gelen hareketli grafik")

`CCRenderTexture` Sınıfı tek bir doku CocosSharp nesnelere işlemek için işlevsellik sağlar. Bir kez oluşturulduktan sonra `CCRenderTexture` örnekleri grafiklerini verimli bir şekilde işlemek ve görsel efektler uygulamak için kullanılabilir.
