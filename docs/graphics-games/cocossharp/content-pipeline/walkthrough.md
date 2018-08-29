---
title: MonoGame ardışık düzen aracını kullanma
description: MonoGame ardışık düzen aracını MonoGame içerik proje oluşturmak ve yönetmek için kullanılır. İçerik projelerdeki dosyaları MonoGame ardışık düzen aracını tarafından işlenen ve .xnb dosyalarıyla CocosSharp ve MonoGame uygulamalarında kullanılmak üzere yüzdelik.
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 347cb7e9d417f97cb6e8d78e67b1c76a378cd188
ms.sourcegitcommit: 7ffbecf4a44c204a3fce2a7fb6a3f815ac6ffa21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "34783305"
---
# <a name="using-the-monogame-pipeline-tool"></a>MonoGame ardışık düzen aracını kullanma

_MonoGame ardışık düzen aracını MonoGame içerik proje oluşturmak ve yönetmek için kullanılır. İçerik projelerdeki dosyaları MonoGame ardışık düzen aracını tarafından işlenen ve .xnb dosyalarıyla CocosSharp ve MonoGame uygulamalarında kullanılmak üzere yüzdelik._

İçerik dosyalarına dönüştürmek için bir kolay kullanımlı ortam MonoGame ardışık düzen aracını sağlar **.xnb** CocosSharp ve MonoGame uygulamalarda kullanmak için dosyaları. İçerik ardışık düzen ve neden yararlı oyun geliştirme hakkında daha fazla bilgi için bkz. [üzerinde içerik ardışık düzenlerine giriş](~/graphics-games/cocossharp/content-pipeline/introduction.md)

Bu izlenecek yol aşağıdakileri kapsar:

 - MonoGame ardışık düzen aracını yükleme
 - CocosSharp projesi oluşturma
 - İçerik Proje oluşturma
 - MonoGame ardışık düzen aracını dosyaları işleme
 - Çalışma zamanında dosyalarını kullanma

Bu kılavuzda CocosSharp projesi göstermek için kullanılır. nasıl **.xnb** yüklenebilir ve bir uygulamada kullanılan dosya. MonoGame kullanıcıları da CocosSharp ve MonoGame aynı kullanırken bu izlenecek yolda başvurabilmek olacaktır **.xnb** içerik dosyaları.

Tamamlanmış uygulamayı bir dokudan görüntüleme tek bir sprite görüntüleyecek bir **.xnb** dosyası ve bir sprite yazı tipinden görüntüleyen bir tek etiketli bir **.xnb** dosyası:

![](walkthrough-images/image1.png "Tamamlanmış uygulamayı görüntüleme .xnb dosyasından bir doku tek bir sprite görüntüler")


## <a name="monogame-pipeline-tool-discussion"></a>MonoGame ardışık düzen aracını tartışma

MonoGame ardışık düzen aracını, Windows, OS X ve Linux'ta kullanılabilir. Bu izlenecek yol aracı Windows üzerinde çalışır, ancak, Mac ve Linux üzerinde de izlenebilir. En fazla veya Linux üzerinde ayarlanmış Aracı'nı edinme hakkında daha fazla bilgi için bkz: [bu sayfayı](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/).

İOS uygulamalarını bile ne zaman bunu geliştiriciler kullanarak Windows üzerinde çalıştırmak için içerik oluşturmak MonoGame ardışık düzen aracını bulabildiği [Xamarin Mac Aracısı](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Windows üzerinde geliştirmeye devam etmek mümkün olacaktır.


## <a name="installing-the-monogame-pipeline-tool"></a>MonoGame ardışık düzen aracını yükleme

MonoGame içeriği ardışık düzeni içeren MonoGame yükleyerek başlayacağız. Mac için ayrı bir indirme MonoGame içeriği ardışık düzeni unutmayın Tüm MonoGame yükleyicileri bulunabilir [MonoGame indirmeler sayfasına](http://www.monogame.net/downloads/). Biz indirirsiniz MonoGame ancak Visual Studio için geliştirici yüklendikten sonra kullanabileceğiniz MonoGame Visual Studio'da Mac için çok:

![](walkthrough-images/image2.png "Ancak Visual Studio için Mac için Visual Studio Geliştirici kullanımı MonoGame çok yüklendikten sonra MonoGame indirin")

İndirildikten sonra biz yükleyiciyi çalıştırın ve varsayılan seçenekleri kabul. Yükleme tamamlandıktan sonra MonoGame ardışık düzen aracını yüklenir ve başlangıç menüsünde aramaya bulunabilir:

![](walkthrough-images/image3.png "Yükleme tamamlandıktan sonra MonoGame ardışık düzen aracını yüklenir ve başlangıç menüsünde aramaya bulunabilir")

MonoGame ardışık düzen aracını başlatın:

![](walkthrough-images/image4.png "MonoGame ardışık düzen aracını Başlat")

MonoGame ardışık düzen aracını çalıştırıldıktan sonra biz oyun ve içerik projelerimizden yapmaya başlayabilirsiniz.


## <a name="creating-an-empty-cocossharp-project"></a>Boş bir CocosSharp projesi oluşturma

Sonraki adım, CocosSharp projesi oluşturmaktır. Biz CocosSharp projesi tarafından oluşturulan klasör yapısındaki içerik Projemizin kaydedebilirsiniz biz CocosSharp projesi öncelikle oluşturmanız önemlidir. CocosSharp projesi yapısını anlamak için göz atın [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md), hangi kullanacağınız bu kılavuzda. İçerik eklemek istediğiniz mevcut bir CocosSharp projesi varsa, ancak proje BouncingGame yerine kullanmaktan çekinmeyin.

Proje oluşturulduktan sonra yapılar ve biz her şeyi olduğunu'ı düzgün bir şekilde ayarlaması doğrulamak için çalıştırın:

![](walkthrough-images/image5.png "Proje oluşturulduktan sonra bu derlemeler doğrulamak için ve her şeyin düzgün şekilde ayarlamanız çalıştırın")


## <a name="creating-a-content-project"></a>İçerik Proje oluşturma

Oyun projesi sahibiz, MonoGame işlem hattı proje oluşturabiliriz. MonoGame ardışık düzen aracını seçme içinde Bunu yapmak için **Dosya > Yeni...**  ve projenizin içerik klasörüne gidin. Android için şu klasöre konumdadır **[root]\BouncingGame.Android\Assets\Content proje\**. İOS için şu klasörü konumdadır **[root]\BouncingGame.iOS\Content proje\**.

Değişiklik **dosya adı** için **ContentProject** tıklatıp **Kaydet** düğmesi:

![](walkthrough-images/image8.png "Dosya adını değiştirmek için ContentProject ve Kaydet düğmesine tıklayın")

Proje oluşturulduktan sonra MonoGame ardışık düzen aracını projeyle ilgili bilgileri görüntüler, kök **ContentProject** öğesi seçildiğinde:

![](walkthrough-images/image9.png "Proje oluşturulduktan sonra kök ContentProject öğe seçildiğinde MonoGame ardışık düzen aracını projesi hakkında bilgi görüntülenir")

İçerik Proje için en önemli seçeneklerden bazılarını göz atalım.


### <a name="output-folder"></a>Çıktı klasörü

Bu klasörle (içerik projenin kendisinin) olduğu çıkış **.xnb** dosyaları kaydedilir. Örneği basit tutmak için aynı klasöre tutan sunduğumuz giriş ve çıkış dosyaları için kullanacağız. Diğer bir deyişle değiştireceğiz **çıkış klasörüne** olmasını **.\**  :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>Platform

Bu içerik için hedef platform tanımlar. Bu bildirim **Windows** varsayılan olarak, bu nedenle istiyoruz, bizim hedef platform için bunu değiştirmek **Android** (veya birlikte bir iOS projesi takip iOS).

![](walkthrough-images/image11.png "Varsayılan olarak bu Windows olduğuna dikkat edin, böylece bu, Android veya iOS yanı sıra bir iOS projesi takip olan hedef platform değiştirin")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>MonoGame ardışık düzen aracını dosyaları işleme

Ardından, içeriği ekleyeceğiz bizim **ContentProject**. Bu proje için dosyaları proje kök dizinine ekleyeceğiz ancak daha büyük projeler genellikle klasör içeriklerini organize eder.

Projemizin için iki dosya ekleyeceğiz:

 - A **.png** sprite çizmek için kullanılan dosya. Bu dosya için [burada indirilen](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true).
 - A **.spritefont** metin ekranda çizim yapmak için kullanılan dosya. İçerik ardışık düzen aracını indirmek için hiçbir dosya olduğundan yeni .spritefont dosyalar oluşturma destekler.


### <a name="adding-a-png-file"></a>Bir .png dosyası ekleme

Eklemek için bir **.png** dosya projeye, biz öncelikle sahip işlem hattı proje ile aynı dizine kopyalayın **.mgcb** uzantısı.

![](walkthrough-images/image12.png "Bir .png dosyası projeye ekleyin.")

Ardından, işlem hattı projeye dosya ekleyeceğiz. MonoGame ardışık düzen aracını bunun için seçin **Düzenle > öğesi Ekle...** seçin **ball.png** tıklayın ve dosya **açık**. Dosya artık içerik projenin bir parçası olacak ve bu onay kutusu seçildiğinde, özelliklerini görüntüler:

![](walkthrough-images/image13.png "Dosya artık içerik projenin bir parçası olur ve bu onay kutusu seçildiğinde, özelliklerini görüntüler")

Hiçbir değişiklik cocossharp'taki .xnb dosyasını yüklemek için gerekli olduğu gibi tüm değerleri değerlerinde bırakarak gerçekleştireceğiz. Biz seçerek dosyayı oluşturabilirsiniz **Yapı > derleme** derlemeyi başlatmak ve derleme çıktısı görüntüle menü seçeneği. Biz denetleyerek derleme doğru şekilde çalıştığını doğrulayın **içerik** için yeni bir klasör **ball.xnb** dosyası:

![](walkthrough-images/image14.png "Yeni bir ball.xnb dosyanın içerik klasörünü kontrol ederek derleme doğru şekilde çalıştığını doğrulayın")


### <a name="adding-a-spritefont-file"></a>.Spritefont dosyası ekleniyor

MonoGame ardışık düzen aracını .spritefont dosyasıyla oluşturabiliriz. CocosSharp olması için yazı tiplerini gerektiren bir **yazı tipleri** klasörü ve CocosSharp şablonları otomatik olarak bir yazı tipleri klasörü otomatik olarak oluşturun. Bu klasör için MonoGame ardışık düzen aracını seçerek ekleyebiliriz **Düzenle > Ekle > var olan bir klasörü...** . Göz atın **içerik** klasörü ve select **yazı tipleri** klasörü ve tıklatın **Tamam**:

![](walkthrough-images/browsetofonts.png "İçerik klasörüne gözatın ve yazı tipleri klasörü seçin ve Tamam'a tıklayın")

Yeni bir .sprintefont dosya eklemek için yazı tipleri klasörü sağ tıklatın ve seçin **Ekle > Yeni öğe...** seçin **SpriteFont açıklama** seçeneğinde, bir ad girin **36 arial**, tıklatıp **Tamam**. CocosSharp gerekir – bunlar olmalıdır [FontType] biçiminde - yazı tipi dosyalarının belirli adlandırma [FontSize]. Bir yazı tipi bu adlandırma biçimi eşleşmiyorsa, CocosSharp tarafından çalışma zamanında yüklenmez.

![](walkthrough-images/image15.png "Bir yazı tipi bu adlandırma biçimi eşleşmiyorsa, CocosSharp tarafından çalışma zamanında yüklenmez")

Aslında, Mac için Visual Studio da dahil olmak üzere, herhangi bir metin düzenleyicisi de düzenlenebilen bir XML dosyası .spritefont dosyasıdır En sık kullanılan değişkenler .spritefont dosyasında düzenlenebilir `FontName` ve `Size` özelliği:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

Biz, dosyanın herhangi bir metin düzenleyicisinde açacaksınız. Olarak bizim **36.spritefont arial** adı önerir, bırakın `FontName` olarak `Arial` ancak değiştirmek `Size` değerini `36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>Çalışma zamanında dosyalarını kullanma

.Xnb dosyaları artık yerleşik ve projemizdeki kullanılmak üzere hazır. Dosyalar için Visual Studio Mac için ekleyeceğiz sonra için kod ekleyeceğiz bizim `GameScene.cs` bu dosyaları yüklemek ve bunları görüntülemek için dosya.


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Mac için Visual Studio'ya .xnb dosyaları ekleme

Projemizin için dosyaları ilk ekleyeceğiz. Mac için Visual Studio'da biz genişletin **BouncingGame.Android** sırasıyla, proje **varlıklar** klasörü sağ tıklatın **içerik** klasör ' nı seçip **Ekle > dosyaları Ekle...** İlk olarak, seçeneğini belirleyeceğiz **ball.xnb** daha önce oluşturulan ve tıklayın **açık**. Ardından yukarıdaki adımları yineleyin, ancak ekleme **36.xnb arial** dosya. Seçeneğini belirleyeceğiz **, geçerli bir alt dizinde dosyayı tutma** Mac için Visual Studio dosya ekleme isterse seçeneği. Bir kez bittiğinde her iki dosya Projemizin parçası olması:

![](walkthrough-images/image20.png "Bir kez bittiğinde her iki dosya projenin bir parçası olmalıdır")


### <a name="adding-gamescenecs"></a>Ekleme **GameScene.cs**

Adlı bir sınıf oluşturacağız `GameScene,` bizim sprite ve metin nesnelerini içerir. Bunu yapmak için sağ **BouncingGame** (BouncingGame.Android değil) seçin ve proje **Ekle > yeni dosya...** . Seçin **genel** kategorisi, select **boş sınıf** seçeneğini ve ardından adını girin **GameScene**.

Oluşturulduktan sonra size değiştireceksiniz `GameScene.cs` dosyaya aşağıdaki kodu içerir:


```csharp
using System;
using CocosSharp;

namespace BouncingGame
{
    public class GameScene : CCScene
    {
        // All visual elements must be added to a CCLayer:
        CCLayer mainLayer;

        // The CCSprite is used to display the "ball" texture
        CCSprite sprite;
        // The CCLabelTtf is used to display the Arial36 sprite font
        CCLabelTtf label;

        public GameScene(CCWindow mainWindow) : base(mainWindow)
        {
            // Instantiate the CCLayer first:
            mainLayer = new CCLayer ();
            AddChild (mainLayer);

            // Now we can create the Sprite using the ball.xnb file:
            sprite = new CCSprite ("ball");
            sprite.PositionX = 200;
            sprite.PositionY = 200;
            mainLayer.AddChild (sprite);

            // The font name (arial) and size (36) need to match 
            // the .spritefont definition and file name.  
            label = new CCLabelTtf ("Using font 36", "arial", 36);
            label.PositionX = 200;
            label.PositionY = 300;
            mainLayer.AddChild (label);
        }
    }
} 
```

CocosSharp CCSprite ve CCLabelTtf gibi görsel nesneler ile çalışma ele alınmıştır beri biz Yukarıdaki kod görüştükten olmaz [BouncingGame Kılavuzu](~/graphics-games/cocossharp/bouncing-game.md).

Biz de yeni oluşturulan bizim yüklemek için kod eklemenize gerek `GameScene`. Açın bunu yapmanın `GameAppDelegate.cs` dosyası (bulunan **BouncingGame** PCL) ve değiştirme `ApplicationDidFinishLaunching` yöntemi gibi görünür:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";

    // New code:
    GameScene gameScene = new GameScene (mainWindow);
    mainWindow.RunWithScene (gameScene);
} 
```

Çalıştırırken, bizim oyun şöyle görünecektir:

![](walkthrough-images/image1.png "Çalıştırırken, oyun nasıl görüneceğini")


## <a name="summary"></a>Özet

Bu izlenecek yolda MonoGame ardışık düzen aracını bir giriş .png dosyasından .xnb dosyaları oluşturmak için nasıl kullanılacağını ve bunun yanı sıra yeni oluşturulan .sprintefont dosyasından yeni bir .xnb dosyası oluşturmak nasıl oluşturulacağını gösterir. Ayrıca, nasıl CocosSharp projeleri .xnb dosyaları kullanmak için yapılandırılır ve çalışma zamanında bu dosyalar yükleme ele.

## <a name="related-links"></a>İlgili bağlantılar

- [MonoGame indirmeleri](http://www.monogame.net/downloads/)
- [MonoGame işlem hattı belgeleri](http://www.monogame.net/documentation/?page=Pipeline)
- [Android (örnek) için başlangıç BouncingGame projesi](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [Ball.PNG grafiği (örnek)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
