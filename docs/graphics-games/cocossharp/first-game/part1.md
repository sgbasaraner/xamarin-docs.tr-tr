---
title: Birden çok Platform CocosSharp proje oluşturma
description: 'Bu kılavuz, yeni bir çok platformlu CocosSharp çözüm oluşturmak gösterilmektedir. Bu kılavuzda üç projeleri içeren Mac çözüm için Visual Studio sonucudur: bir taşınabilir sınıf kitaplığı proje, bir Android özel Proje ve bir iOS özel Proje. Elde edilen Proje çalıştırıldığında boş bir siyah ekranla görüntüler.'
ms.topic: article
ms.prod: xamarin
ms.assetid: 37C97693-B0A8-4064-97B6-A6FAB5BA4FB7
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2906035ce9bd44d111b89ccfe7443896775315b7
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="creating-a-multi-platform-cocossharp-project"></a>Birden çok Platform CocosSharp proje oluşturma

_Bu kılavuz, yeni bir çok platformlu CocosSharp çözüm oluşturmak gösterilmektedir. Bu kılavuzda üç projeleri içeren Mac çözüm için Visual Studio sonucudur: bir taşınabilir sınıf kitaplığı proje, bir Android özel Proje ve bir iOS özel Proje. Elde edilen Proje çalıştırıldığında boş bir siyah ekranla görüntüler._

Kod ve birden çok platform genelinde paylaşılmak üzere içerik CocosSharp 2B oyun altyapısı sağlar. Bu kılavuz, iOS ve Android geliştirme destekleyebilen bir proje oluşturmak gösterilmiştir. Özellikle, bu kılavuzda aşağıdaki konular ele alınacaktır:

 - CocosSharp yükleme
 - Yeni bir çözüm oluşturma
 - `LoadGame` Yöntemi

# <a name="installing-cocossharp"></a>CocosSharp yükleme

İlk olarak, CocosSharp Visual Studio'ya Mac için ekleyeceğiz Mac üzerinde çalışıyorsa seçin **Mac için Visual Studio** > **Eklenti Yöneticisi...**  . Windows üzerinde çalışan, seçin **Araçları** > **Eklenti Yöneticisi...**  . ' I tıklatın **galeri** sekmesinde, genişletin **CocosSharp öğesi**seçin **CocosSharp proje şablonları**ve son olarak tıklatın **yükleme...**  .

![CocosSharp eklentisi](part1-images/xamarinstudioaddinsmac.png "")

# <a name="creating-a-new-solution"></a>Yeni bir çözüm oluşturma

Artık CocosSharp yüklendiğine göre bir çözüm oluşturacağız. Mac için Visual Studio'da seçin **dosya** > **yeni** > **çözüm...** . Seçin **uygulama** altında seçeneği **platformlar arası** bölümünde, select **CocosSharp boş proje**ve ardından **sonraki**:

![](part1-images/image1.png "Platformlar arası bölümünde uygulama seçeneğini belirleyin, CocosSharp boş proje seçin ve İleri'yi tıklatın")

Bir ad girin **BouncingGame** için **proje adı**, ardından **oluşturma**:

![](part1-images/image2.png "Proje adı BouncingGame adı girin ve ardından Oluştur'u tıklatın")

Proje oluşturulup oluşturulmadığını ve Mac için Visual Studio, biz derleme ve çalıştırma gri bir arka plan görüntülemek için bir kez: 

![](part1-images/image3.png "Proje oluşturulur ve görsel silindikten sonra Mac Studio derleme ve çalıştırma gri bir arka plan görüntülemek için")


# <a name="loadgame-method"></a>LoadGame yöntemi

Varsayılan CocosSharp proje ayarlamak için iOS ve Android için belirli sınıfları içerir. bir `CCGameView`, CocosSharp başlatmak için kullanılır. `CCGameView` Örneği bir platforma özgü şekilde oluşturulur: iOS projesi oluşturur `CCGameView` içinde `Main.storyboard` dosya Android oluştururken `CCGameView` içinde `Main.axml` dosya. Her platform CCGameView örneğinde kullanan bir `LoadGame` bazı temel kurulum gerçekleştiren yöntemi. Biz bu kodunda değişiklik olmaz olsa bile, bazı önemli ayrıntıları bir göz atalım:

 - Kod kümeleri `gameView.DesignResolution` 1024 x 768 için. Bu, geçerli cihazın en boy oranı, fiziksel çözünürlük veya yönlendirme bağımsız olarak aygıtları üzerinden konumlandırma standartlaştırır. 
 - Kod birkaç arama yolları ekler. Arama yolları dizin önekleri yüklenmesi içeriği de olanak sağlar. Örneğin, bu yana `"Sounds"` yolu, bir arama yolu sonra dizindeki bir dosya olarak eklenir `"Content/Sounds/mysound.xnb"` yalnızca olarak yüklenmiş olabilir `"mysound.xnb"`. Arama yolları benzer `using` deyimleri kodda – bunlar kodu azaltabilir, ancak belirsizlik de ortaya çıkarabilir.
 - Kodu oluşturan bir `GameLayer` örneği. `GameLayer` olan bir `CCLayer`-burada biz ekleme tüm oyun mantığımızı sınıf devralma. Birden çok büyük oyunlar gerektirebilir `CCLayer` örneği veya hatta birden çok `CCScene` örnekleri (birden çok içerebilir `CCLayer` örnekleri), ancak tek bir takılıyor `CCLayer` Bu oyun için.

#  <a name="summary"></a>Özet

Bu kılavuzda Mac için Visual Studio kullanarak bir platformlar arası CocosSharp projesi oluşturmak nasıl ele Oyun her proje için başlangıç noktası olarak kullanılabilecek boş bir ekran sonucudur.