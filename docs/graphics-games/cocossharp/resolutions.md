---
title: Birden çok çözümleri CocosSharp işleme
description: Bu kılavuz çeşitli çözümler cihazlarda düzgün görüntülemek oyun geliştirmek için CocosSharp çalışmak nasıl gösterir.
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 4077af2351b8ab3ef718a71cc672add54b6ef05a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>Birden çok çözümleri CocosSharp işleme

_Bu kılavuz çeşitli çözümler cihazlarda düzgün görüntülemek oyun geliştirmek için CocosSharp çalışmak nasıl gösterir._

CocosSharp oyununuzu sayısından bağımsız olarak fiziksel bir cihazın ekranındaki piksel nesne boyutlarında Standartlaştırma için yöntemleri sağlar. Geleneksel olarak, bilgisayarları ve oyun konsolları için geliştirilen oyunlar tek bir çözüm hedefleyebilir. Modern oyunlar – ve mobil cihazlar için özellikle oyunlar –, çok çeşitli aygıtları uyum sağlayacak şekilde oluşturulmalıdır. Bu kılavuz CocosSharp standartlaştırmak oyunlar fiziksel ekran çözünürlüğü özelliklerini bağımsız olarak gösterilmektedir.

Varsayılan çözünürlük CocosSharp, fiziksel piksel oyun koordinatları ile eşleşecek şekilde davranıştır. Aşağıdaki tabloda, çeşitli aygıtlar bir arka plan ortamı hareketli grafik genişliği ve yüksekliği 368 x 240 hale getiren gösterir. İlk satırın teknik değil gerçek bir cihaza, ancak aygıt çözünürlüğü bakılmaksızın hareketli yerine beklenen çizmeye şöyledir:


| **Cihazı** | **Ekran çözünürlüğü** | **Örnek ekran görüntüsü** |
|--- | --- |--- |
|İstediğiniz görüntüleme|368 x 240 (ile siyah çubuklar en boy oranını için)| ![368 x 240 (ile siyah çubuklar en boy oranını için)](resolutions-images/image1.png) |
|iPhone 4s|960x640| ![iPhone 4s 960x640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920x1080| ![iPhone 6 Plus 1920x1080](resolutions-images/image3.png) |

Bu belge yukarıdaki tabloda gösterilen sorunu gidermek için CocosSharp kullanmayı kapsar. Diğer bir deyişle, biz – ekran çözünürlüğü bağımsız olarak ilk satırın gösterildiği gibi işleme herhangi bir cihazda nasıl ele alacağız.


## <a name="working-with-setdesignresolutionsize"></a>SetDesignResolutionSize ile çalışma

`CCScene` Sınıfı tipik olarak tüm görsel nesneler için kök kapsayıcı olarak kullanılır, ancak ayrıca adlı bir statik yöntem sunar `SetDesignResolutionSize` tüm planda için varsayılan boyutunu belirtmek için. Diğer bir deyişle `SetDesignResolutionSize` yöntemi donanım çözümleri çeşitli görüntülenecektir oyun geliştirmek geliştiricilere olanak sağlar. CocosSharp proje şablonları aşağıdaki kodda gösterildiği gibi varsayılan proje boyutunu 1024 x 768'olarak ayarlamak için bu yöntemi kullanın:


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
    
    // This will set the world bounds to (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```

Değiştirebileceğiniz `desiredWidth` ve `desiredHeight` değişkenleri oyununuzu istenen çözünürlüğünü eşleşecek şekilde görünen değiştirmek için yukarıdaki. Örneğin, yukarıdaki kod gibi tam ekran 368 x 240 arka plan görüntülemek için değiştirilmesi:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 368.0f;
    float desiredHeight = 240.0f;
    
    // This will set the world bounds to(0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```


## <a name="ccsceneresolutionpolicy"></a>CCSceneResolutionPolicy

`SetDesignResolutionSize` bize nasıl oyun pencerenin istenen çözümleme ayarlar belirtmenizi sağlar. Aşağıdaki bölümlerde 500 x 500 görüntü ile farklı görüntülenme göstermek `CCSceneResolutonPolicy` geçirilen değerleri `SetDesignResolutionSize` yöntemi. Aşağıdaki değerleri tarafından sağlanan `CCSceneResolutionPolicy` enum:

 - `ShowAll` – En boy oranını koruyarak tüm istenen çözünürlüğünü görüntüler.
 - `ExactFit` – Tüm istenen çözümleme görüntüler, ancak en boy oranını korumaz.
 - `FixedWidth` – En boy oranını koruyarak tüm istenen genişliği görüntüler.
 - `FixedHeight` – En boy oranını koruyarak tüm istenen yüksekliği görüntüler.
 - `NoBorder` – Tüm yüksekliği ya da en boy oranını koruyarak tüm genişliğini görüntüler. Cihaz en boy oranını istenen çözünürlüğü en boy oranını büyükse, tüm genişliğini gösterilir. Cihaz en boy oranını istenen çözünürlüğü en boy oranını küçükse, tüm yüksekliği gösterilir.
 - `Custom` – Görüntü bağlıdır `Viewport` özelliği geçerli `CCScene`.

Tüm ekran görüntüleri yatay yönde (960 x 640) iPhone 4s çözünürlükte üretilen ve bu görüntüyü kullanın:

![](resolutions-images/image4.png "Tüm ekran görüntüleri yatay yönde 960 x 640 iPhone 4s çözünürlükte üretilen ve bu görüntüyü kullanma")


### <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` tüm oyun çözümleme ekran görünür olur, ancak görüntülenebilir belirtir *letterboxing* (için farklı en boy oranlarına ayarlamak için siyah çubuklar). Bu ilke tüm oyun görünümü ekrandaki tüm bozulma gösterilecek garanti olarak yaygın olarak kullanılır.

Kod örneği:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Letterboxing istenen çözünürlüğe daha geniş olan fiziksel en boy oranını hesaba görüntünün sağ ve sol görülebilir:

![](resolutions-images/image5.png "Letterboxing istenen çözünürlüğe daha geniş olan fiziksel en boy oranını hesaba görüntünün sağ ve sol tarafından görülebilir")


### <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` tüm oyun çözümleme hiçbir letterboxing ekran ile görünür olacağını belirtir. Görüntülenebilir alanı bozulabilir (en boy oranını tutulmaz) donanım en boy oranını göre.

Kod örneği:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

Hiçbir letterboxing görünür, ancak aygıt çözünürlüğü dikdörtgen olduğundan oyun görünümü bozuk:

![](resolutions-images/image6.png "Hiçbir letterboxing görünür, ancak aygıt çözünürlüğü dikdörtgen olduğundan oyun görünümü bozuk")


### <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` Görünüm genişliğini geçirilen genişlik değeri eşleşir belirtir `SetDesignResolutionSize`, ancak görüntülenebilir yükseklik fiziksel cihazın en boy oranını tabidir. Geçirilen yükseklik değeri `SetDesignResolutionSize` fiziksel cihazın en boy oranını göre çalışma zamanında hesaplanır beri göz ardı edilir. Başka bir deyişle, hesaplanan yükseklik (ve ekran dışı bırakılıyor oyun görünüm bölümlerinde sonuçlanır) istenen yüksekliği veya hesaplanan yükseklik (ve daha fazla görüntülenmesini oyun görünümünün sonuçlanır) istenen yüksekliği daha büyük olabilir küçük olabilir. Bu daha görüntülenmesini oyun sonuçlanabilir beri letterboxing oluştu gibi görünebilir; Ancak, ek boşluk mutlaka herhangi bir görsel nesne burada görünüyorsa siyah olmaz. 

Kod örneği:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

Hesaplanan yüksekliği yaklaşık 333 birimleri olacak şekilde 4s iPhone: 2, 3 en boy oranını sahiptir:

![](resolutions-images/image7.png "Hesaplanan yüksekliği yaklaşık 333 birimdir 4s iPhone: 2, 3 en boy oranını sahiptir, bu nedenle")


### <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

Kavramsal olarak, `FixedHeight` benzer şekilde davranır `FixedWidth` – oyun geçirilen yükseklik değeri uyarlar `SetDesignResolutionSize,` ancak dışına fiziksel çözünürlük tabanlı çalışma zamanında genişliği hesaplar. Yukarıda görüntülenen genişlikte Bunun anlamı söz edildiği gibi daha küçük veya oyun olma bölümünde kaynaklanan istenen genişliği büyük ekran veya daha fazla, sırasıyla görüntülenmesini oyun kapalı.

Kod örneği:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Aşağıdaki ekran görüntüsünde yatay modunda çalışan uygulama oluşturulduktan sonra hesaplanan genişlik'den 500 – özellikle 750 daha büyük. Ek çözümleme ekranın sağ tarafta görüntülenebilir olması için bu ilkeyi sola hizalı 0 X değerini korur.

![](resolutions-images/image8.png "Ek çözümleme ekranın sağ tarafta görüntülenebilir olması için bu ilkeyi 0 X değerini sola hizalı tutar")


### <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` (hiçbir bozulma) özgün en boy oranını koruyarak hiçbir letterboxing uygulamayla görüntülenecek çalışır. İstenen Çözümleme 's en boy oranını cihazın fiziksel en boy oranını eşleşirse, hiçbir kırpma meydana gelir. En boy oranlarına eşleşmiyorsa, ardından kırpma meydana gelir.

Kod örneği:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Aşağıdaki ekran görüntüsünde kırpılmış tüm 500 görüntü genişliğini piksel görüntülenirken görünen üst ve alt parçalarını görüntüler:

![](resolutions-images/image9.png "Bu ekran görüntüsü kırpılmış tüm 500 görüntü genişliğini piksel görüntülenirken görünen üst ve alt parçalarını görüntüler")


### <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` Her etkinleştirir `CCScene` kendi özel görünüm penceresinin içinde belirtilen çözünürlük göre belirtmek için `SetDesignResolutionSize`.

Planda tanımlamak kendi `Viewport` oluşturma özelliğini bir `CCViewport`, hangi sırayla yapılandırılmıştır ile bir `CCRect`. Daha fazla bilgi için `CCViewport` ve `CCRect` bkz [CocosSharp API belgelerine](https://developer.xamarin.com/api/namespace/CocosSharp/). Oluşturma bağlamında bir `CCViewport` `CCRect` Oluşturucusu ait parametreleri belirtin (sırasıyla):

 - x – her birim bir tüm ekran genişliği temsil ettiği görünüm penceresinin yatay uzaklığını tutar. Örneğin, ekran genişliği yarısı sağa gölgeye Sahne .5f sonuçları değerini kullanarak.
 - y – her birim bir tüm ekran yüksekliğini temsil ettiği görünüm penceresinin dikey uzaklık tutar. Örneğin, ekran yüksekliği yarısı gölgeye Sahne .5f sonuçları değerini kullanarak.
 - genişliği – Sahne bulunması gereken yatay bölümü. Örneğin, 1 değerini kullanarak / 3.0f yatay üçte ekranın kaplayan Sahne sonuçlanır.
 - Yükseklik – Sahne bulunması gereken dikey bölümü. Örneğin, 1 değerini kullanarak / 3.0f dikey ekran üçte kaplayan Sahne sonuçlanır.

Kod örneği:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.Custom);
...
float horizontalDisplayPortion = 2 / 3.0f;
float verticalDisplayPortion = 1 / 2.0f;
float displayOffsetInScreenWidths = 0;
float displayOffsetInScreenHeights = .5f;
var rectangle = new CCRect (
    displayOffsetInScreenWidths, 
    displayOffsetInScreenHeights, 
    horizontalDisplayPortion, 
    verticalDisplayPortion);
scene.Viewport = new CCViewport (rectangle);
```

Yukarıdaki kod sonuçları şunlardır:

![](resolutions-images/image10.png "Yukarıdaki kod bu ekran görüntüsünde sonuçları")


## <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

`DefaultTexelToContentSizeRatio` Daha yüksek çözünürlük ekranlar ile cihazlarda daha yüksek çözünürlükte dokuları kullanarak basitleştirir. Özellikle, bu özellik oyunlar boyutu veya görsel öğelerin konumlandırmasını değiştirmek için gerek kalmadan daha yüksek çözünürlükte varlıklar kullanmanıza olanak sağlar. 

Örneğin, oyun, 200 piksel uzunluğunda olan bir oyun karakteri ve oyun ekran için bir resim varlık içerebilir (belirtildiği gibi `SetDesignResolutionSize`) 400 piksel uzunluğunda olabilir. Bu durumda, karakteri ekranın dolduracaktır. Ancak, 400 piksel uzunluğunda varlık karakter daha yüksek çözünürlükte cihazlar için kullanıldıysa, karakteri görünen tüm yüksekliğini kaplar. Amaç karakter daha yüksek çözünürlük aygıtları yararlanmak için aynı boyutta tutmak için ise, bazı ek kod karakter hareketli ekran yarı yükseklikte tutmak gerekli.

Yukarıda gösterilen basit bir örnek oyun geliştirmede – aygıt çözünürlüğü göre her bir görsel öğe yeniden boyutlandırma gerçekleştirmek Geliştirici gerek olmadan daha büyük ölçekli varlıklar ekleme ortak bir sorunu temsil eder. `DefaultTexelToContentSizeRatio` Genel özellik görsel öğeleri yeniden boyutlandırma için kullanılır. Bu değer belirli bir türdeki tüm görsel öğeleri tarafından atanan değer göre yeniden boyutlandırır.

Bu özellik aşağıdaki sınıflarda bulunmaktadır: 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

Mac şablonları kümesi için CocosSharp Visual Studio `CCSprite.DefaultTexelToContentSizeRatio` cihazı nasıl kullanılabileceği bir örnek olarak genişliğini göre. Aşağıdaki kod yer `CCApplicationDelegate.ApplicationDidFinishLaunching`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
           
    ...
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
    ...           
}
```


### <a name="defaulttexeltocontentsizeratio-example"></a>DefaultTexelToContentSizeRatio example

Görmek için nasıl `DefaultTexelToContentSizeRatio` visual boyutunu etkiler öğeleri, yukarıda sunulan kodu göz önünde bulundurun:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Hangi 500 x 500 doku kullanarak aşağıdaki görüntüde neden olur:

![](resolutions-images/image5.png "500 x 500 doku kullanarak bu görüntüde neden olur")

İPad Retina fiziksel 2048 x 1536 çözünürlükte çalışır. Başka bir deyişle, yukarıda gösterildiği oyun 1536 fiziksel pikseller üzerinde 500 piksel uzatmak. Oyunlar yararlanabilir fazladan çözünürlük ne genellikle denir oluşturarak *hd* varlıklar – daha yüksek çözünürlüklü ekranlarda çalıştırmak için tasarlanmış varlıklar. Örneğin, bu oyun 1000 x 1000 boyutta bu doku hd sürümünü kullanabilirsiniz. Ancak, daha büyük görüntü kullanarak neden olacağından `CCSprite` genişlik ve yükseklik 1000 birimlerinin sahip. Bizim oyun her zaman 500 birim görüntüler bu yana (nedeniyle `SetDesignResolutioSize` çağrısı), bizim 1000 x 1000 hareketli grafik (yalnızca alt sol kısmını görüntüler) çok büyük olması:

![](resolutions-images/image11.png "Oyun her zaman SetDesignResolutioSize çağrısı nedeniyle 500 birim görüntüler olduğundan, 1000 x 1000 hareketli yalnızca alt sol kısmını görüntüler çok büyük olabilir")

Biz ayarlayabilirsiniz `CCSprite.DefaultTexelToContentSizeRatio` olarak genişlik ve yükseklik özgün 500 x 500 doku olarak iki kez olan 1000 x 1000 doku için hesap için:


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

Şimdi biz oyun çalıştırırsanız 1000 x 1000 doku tam olarak görünür olacaktır:

![](resolutions-images/image12.png "Biz oyun çalıştırırsanız şimdi 1000 x 1000 doku tam olarak görünür olur")


### <a name="defaulttexeltocontentsizeratio-details"></a>DefaultTexelToContentSizeRatio details

`DefaultTexelToContentSizeRatio` Özelliği `static,` uygulamadaki tüm hareketli grafik başka bir deyişle, aynı değere paylaşır. Tipik farklı çözümler için yapılan varlıklar oyunlar için varlıkları her çözümleme kategorisi için eksiksiz bir kümesini içeren bir yaklaşımdır. Varsayılan Mac şablonları için CocosSharp Visual Studio sağlamak **ld** ve **hd** dokuları iki kümelerini destekleme oyunlar için yararlı olabilecek varlıklar için klasörler. Bir örnek içerik klasörünü içerikle gibi görünebilir:

![](resolutions-images/image13.png "Bir örnek içerik klasörünü içerikle gibi görünebilir")

Varsayılan şablonu koşullu uygulamanın eklemek için kodunu da içeren bildirim `ContentSearchPaths`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
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
    ...           
}
```

Bu uygulama, yüksek çözünürlüklü veya normal çözümleme modunda çalışıp çalışmadığını göre yolları dosyalar için arama yaparlar anlamına gelir. Örneğin, varsa **hd** ve **ld** klasörleri içeren adlı bir dosya **background.png** sonra aşağıdaki kodu ve çözümleme için doğru dosyası seçin:


```csharp
backgroundSprite  = new CCSprite ("background");
```


## <a name="summary"></a>Özet

Bu makalede, aygıt çözünürlüğü bağımsız olarak doğru bir şekilde görüntüleyen oyunlar oluşturma yer almaktadır. Kullanım örnekleri farklı gösterir `CCSceneResolutionPolicy` aygıt çözünürlüğü göre oyunu yeniden boyutlandırma için değerleri. Ayrıca bir örnek sağlar `DefaultTexelToContentSizeRatio` ayrı olarak yeniden boyutlandırılıp görsel öğeleri gerek kalmadan içeriği birden çok kümesini barındırmak için kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [CocosSharp giriş](~/graphics-games/cocossharp/index.md)
- [CocosSharp API belgeleri](https://developer.xamarin.com/api/namespace/CocosSharp/)
