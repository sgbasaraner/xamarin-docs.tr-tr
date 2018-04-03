---
title: MonoGame PipelineTool kullanma
description: MonoGame ardışık düzen aracı MonoGame içerik proje oluşturmak ve yönetmek için kullanılır. İçerik projeleri dosyalarında Monogame ardışık düzen aracı tarafından işlenir ve CocosSharp ve MonoGame uygulamalarda kullanmak için .xnb dosyaları olarak yüzdelik.
ms.topic: article
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 37505b166488230be9d0e0690e415852506664f1
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="using-the-monogame-pipeline-tool"></a>MonoGame ardışık düzen Aracı'nı kullanma

_MonoGame ardışık düzen aracı MonoGame içerik proje oluşturmak ve yönetmek için kullanılır. İçerik projeleri dosyalarında Monogame ardışık düzen aracı tarafından işlenir ve CocosSharp ve MonoGame uygulamalarda kullanmak için .xnb dosyaları olarak yüzdelik._

İçerik dosyalarına dönüştürmek için bir kolay kullanımlı ortam MonoGame ardışık düzen aracı sağlar **.xnb** CocosSharp ve MonoGame uygulamalarda kullanmak için dosyaları. İçerik ardışık düzen ve neden oyun geliştirme yararlı hakkında daha fazla bilgi için bkz: [içerik ardışık düzen üzerinde bu giriş](~/graphics-games/cocossharp/content-pipeline/introduction.md)

Bu kılavuz aşağıdakileri kapsar:

 - MonoGame ardışık düzen aracını yükleme
 - CocosSharp proje oluşturma
 - İçerik Proje oluşturma
 - MonoGame ardışık düzen aracında dosyaları işleme
 - Çalışma zamanında dosyalarını kullanma

Bu kılavuzda CocosSharp proje göstermek için kullanılır. nasıl **.xnb** dosyaları yüklenebilir ve bir uygulamada kullanılan. MonoGame kullanıcılarının de erişebilir CocosSharp ve MonoGame aynı kullanırken bu kılavuzda başvuru **.xnb** içerik dosyaları.

Tamamlanan uygulama hareketli gelen doku görüntüleme tek bir grafik görüntüler bir **.xnb** dosya ve hareketli grafik yazı tipinden görüntüleme tek etiketli bir **.xnb** dosyası:

![](walkthrough-images/image1.png "Tamamlanan uygulama hareketli .xnb dosyasından bir doku görüntüleme tek bir grafik görüntüler")


## <a name="monogame-pipeline-tool-discussion"></a>MonoGame ardışık düzen aracı tartışma

MonoGame ardışık düzen aracını, Windows, OS X ve Linux üzerinde kullanılabilir. Bu kılavuz aracı Windows üzerinde çalışır, ancak, Mac ve Linux üzerinde de izlenebilir. Max veya Linux üzerinde ayarlanan Aracı'nı edinme hakkında daha fazla bilgi için bkz: [bu sayfayı](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/).

İOS uygulamaları bile zaman kullanarak bunu geliştiriciler Windows üzerinde çalışan içeriği oluşturmak MonoGame ardışık düzen aracı bulabildiği [Xamarin Mac arası](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Windows geliştirmeye devam mümkün olacaktır.


## <a name="installing-the-monogame-pipeline-tool"></a>MonoGame ardışık düzen aracını yükleme

MonoGame içerik ardışık düzen içeren MonoGame yükleyerek işlemini başlatacak. Mac için ayrı bir yükleme MonoGame içerik ardışık düzen unutmayın Tüm MonoGame yükleyicileri bulunabilir [MonoGame indirmeler sayfası](http://www.monogame.net/downloads/). Biz indirirsiniz MonoGame Visual Studio için ancak geliştirici yüklendikten sonra kullanabileceğiniz MonoGame Visual Studio'da Mac için çok:

![](walkthrough-images/image2.png "Visual Studio için ancak Mac için Visual Studio Geliştirici kullanım MonoGame çok yüklendikten sonra MonoGame indirin")

Yüklendikten sonra biz yükleyiciyi çalıştırın ve varsayılan seçenekleri kabul. Yükleme tamamlandıktan sonra MonoGame ardışık düzen aracı yüklenir ve Başlat menüsü aramada bulunabilir:

![](walkthrough-images/image3.png "Yükleme tamamlandıktan sonra MonoGame ardışık düzen aracı yüklü ve Başlat menüsü aramada bulunabilir.")

MonoGame ardışık düzen Aracı'nı başlatın:

![](walkthrough-images/image4.png "MonoGame ardışık düzen Aracı'nı başlatma")

MonoGame ardışık düzen aracı çalışmaya başladıktan sonra biz bizim oyun ve içerik projeleri yapmaya başlayabilirsiniz.


## <a name="creating-an-empty-cocossharp-project"></a>Boş bir CocosSharp projesi oluşturma

Sonraki adım CocosSharp projesi oluşturmaktır. Böylece biz CocosSharp projesi tarafından oluşturulan klasörü yapısı içinde içerik Projemizin kaydedebilirsiniz biz CocosSharp proje ilk oluşturmanız önemlidir. CocosSharp projenin yapısını anlamak için bir göz atalım [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md), hangi kullanacağınız bu kılavuzda. İçeriği eklemek istediğiniz varolan CocosSharp projesinde varsa, ancak bu proje BouncingGame yerine kullanılacak çekinmeyin.

Proje oluşturulduktan sonra bunu yapıların ve biz her şeyi olduğunu'ı düzgün bir şekilde ayarlanmış doğrulamak için çalıştırmanız:

![](walkthrough-images/image5.png "Proje oluşturulduktan sonra onu derlemeleri doğrulamak için ve her şeyin doğru şekilde ayarladığınız çalıştırın")


## <a name="creating-a-content-project"></a>İçerik Proje oluşturma

Oyun proje sahibiz, biz MonoGame ardışık düzen proje oluşturabilirsiniz. Bunun MonoGame ardışık düzen aracı Seç **Dosya > Yeni...**  ve projenizin içerik klasörüne gidin. Android için klasör bulunur **[root]\BouncingGame.Android\Assets\Content proje\**. İOS için klasör bulunur **[root]\BouncingGame.iOS\Content proje\**.

Değişiklik **dosya adı** için **ContentProject** tıklatıp **kaydetmek** düğmesi:

![](walkthrough-images/image8.png "ContentProject için dosya adını değiştirin ve Kaydet düğmesini tıklatın")

Proje oluşturulduktan sonra MonoGame ardışık düzen aracı proje hakkında bilgi görüntüler, kök **ContentProject** öğesi seçildiğinde:

![](walkthrough-images/image9.png "Proje oluşturulduktan sonra kök ContentProject öğesi seçildiğinde MonoGame ardışık düzen araç proje hakkında bilgi görüntüler")

İçerik Proje için en önemli seçenekleri bazıları bakalım.


### <a name="output-folder"></a>Çıkış klasörü

Bu klasör (içerik projeye göre kendisini) olduğu çıktı **.xnb** dosyaları kaydedilecek. Örneği basit tutmak için aynı klasöre bizim giriş tutun ve çıkış dosyaları için kullanacağız. Diğer bir deyişle biz değiştireceğiz **çıkış klasörü** olmasını **.\**  :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>Platform

Bu içerik için hedef platformu tanımlar. Bu bildirim **Windows** varsayılan olarak, böylece biz istemeniz bunu olan bizim hedef platformu değiştirmek **Android** (veya bir iOS projesi yanı sıra aşağıdaki durumunda iOS).

![](walkthrough-images/image11.png "Varsayılan olarak bu Windows olduğuna dikkat edin, bu nedenle bu, Android veya iOS bir iOS projesi yanı sıra aşağıdaki ise hedef platformu Değiştir")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>MonoGame ardışık düzen aracında dosyaları işleme

Ardından, biz içeriği ekleme bizim **ContentProject**. Bu proje için biz dosyaları proje kökündeki ekleme, ancak daha büyük projeler genellikle içeriklerini klasörlerde düzenler.

Projemizin için iki dosya ekleyeceğiz:

 - A **.png** bir hareketli grafik çizmek için kullanılan dosya. Bu dosya için [burada indirilen](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true).
 - A **.spritefont** metin ekranda çizmek için kullanılan dosya. İçerik ardışık düzen Aracı'nı indirmek için hiçbir dosya böylece yeni .spritefont dosyalar oluşturma destekler.


### <a name="adding-a-png-file"></a>.Png dosyası ekleme

Eklemek için bir **.png** dosyası projeye biz öncelikle sahip işlem hattı proje aynı dizine kopyalayın **.mgcb** uzantısı.

![](walkthrough-images/image12.png ".Png dosyası projeye ekleyin")

Ardından, ardışık düzen projeye dosya ekleyeceğiz. Bu MonoGame ardışık düzen aracında yapmak için seçin **Düzenle > Öğe Ekle...** seçin **ball.png** dosya ve tıklayın **açık**. Dosya artık içerik projesinin bir parçası olur ve seçili olduğunda, özelliklerini görüntüler:

![](walkthrough-images/image13.png "Dosya artık içerik projesinin bir parçası olur ve seçili olduğunda, özelliklerini görüntüler")

Hiçbir değişiklik CocosSharp .xnb dosyasında yüklemek için gerektiği şekilde tüm değerleri varsayılan değerlerde bırakarak gerekir. Biz seçerek dosyayı oluşturabilirsiniz **Yapı > Yapı** başlatılır derlemeyi ve derleme çıktısı görüntülemek menü seçeneği. Biz denetleyerek yapı düzgün çalıştığını doğrulayabilirsiniz **içerik** için yeni bir klasör **ball.xnb** dosyası:

![](walkthrough-images/image14.png "Yeni bir ball.xnb dosyası için içerik klasörünü denetleyerek derleme doğru şekilde çalıştığını doğrulayın")


### <a name="adding-a-spritefont-file"></a>.Spritefont dosya ekleme

Biz MonoGame ardışık düzen araçla .spritefont dosyası oluşturabilirsiniz. CocosSharp olması için yazı tiplerini gerektiren bir **yazı tiplerini** klasörü ve CocosSharp şablonları otomatik olarak bir yazı tipi klasörü otomatik olarak oluşturma. Bu klasör MonoGame ardışık düzen aracın seçerek ekleyebiliriz **Düzenle > Ekle > Varolan bir klasörü...** . Gözat **içerik** klasörü ve seçin **yazı tiplerini** klasörü ve tıklatın **Tamam**:

![](walkthrough-images/browsetofonts.png "İçerik klasörüne gözatın ve yazı tipleri klasörü seçin ve Tamam'ı tıklatın")

Yeni bir .sprintefont dosyası eklemek için yazı tipleri klasörü sağ tıklatın ve seçin **Ekle > Yeni öğe...** seçin **SpriteFont açıklama** seçeneği, bir ad girin **36 arial**, tıklatıp **Tamam**. CocosSharp gerektirir – gerekir olmaları [FontType] biçiminde - yazı tipi dosyalarının belirli adlandırma [FontSize]. Bir yazı tipi bu adlandırma biçimine eşleşmiyorsa, CocosSharp tarafından çalışma zamanında yüklenmeyecek.

![](walkthrough-images/image15.png "Bir yazı tipi bu adlandırma biçimine eşleşmiyorsa, CocosSharp tarafından çalışma zamanında yüklenmez")

Gerçekte Mac için Visual Studio dahil olmak üzere, herhangi bir metin düzenleyicisinde düzenlenebilen bir XML dosyası .spritefont dosyasıdır .Spritefont dosyasında düzenlenebilir en yaygın değişkenleri `FontName` ve `Size` özelliği:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

Biz dosyayı herhangi bir metin düzenleyicisinde açın. Olarak bizim **36.spritefont arial** adı önerir, bırakın `FontName` olarak `Arial` ancak değiştirmek `Size` değeri `36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>Çalışma zamanında dosyalarını kullanma

.Xnb dosyaları şimdi oluşturulur ve bizim projede kullanılmak üzere hazır. Biz dosyaları için Visual Studio için Mac ekleme sonra kodu ekleyeceğiz bizim `GameScene.cs` bu dosyaları yüklemek ve bunları görüntülemek için dosya.


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Mac için Visual Studio .xnb dosyaları ekleme

Bizim projeye dosyaları ilk ekleyeceğiz. Mac için Visual Studio'da biz genişletin **BouncingGame.Android** projesi, genişletin **varlıklar** klasörünü sağ tıklatın **içerik** klasörü ' nı seçip **Ekle > dosyaları Ekle...** İlk olarak, biz seçersiniz **ball.xnb** daha önce oluşturulmuş ve tıklayın **açık**. Ardından yukarıdaki adımları yineleyin, ancak eklemek **36.xnb arial** dosya. Biz seçersiniz **dosyayı geçerli kendi alt dizinindeki tutma** Mac için Visual Studio dosyasını nasıl ekleyeceğinizi isterse seçeneği. Bir kez tamamlanan her iki dosya bizim projesinin bir parçası olması gerekir:

![](walkthrough-images/image20.png "Bir kez tamamlanan her iki dosya projenin bir parçası olmalıdır")


### <a name="adding-gamescenecs"></a>Ekleme **GameScene.cs**

Adlı bir sınıf oluşturacağız `GameScene,` bizim hareketli grafik ve metin nesneleri içerecek. Bunu yapmak için sağ **BouncingGame** (değil BouncingGame.Android) seçin ve proje **Ekle > yeni dosya...** . Seçin **genel** kategorisi, select **boş sınıfı** seçeneğini ve ardından adını girin **GameScene**.

Oluşturduktan sonra biz değiştireceksiniz `GameScene.cs` dosya aşağıdaki kod içerir:


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

İçinde CCSprite ve CCLabelTtf gibi CocosSharp visual nesnelerle çalışmayı ele beri biz Yukarıdaki kod ele olmaz [BouncingGame Kılavuzu](~/graphics-games/cocossharp/bouncing-game.md).

Ayrıca yeni oluşturulan bizim yüklemek üzere kod eklemek ihtiyacımız `GameScene`. Bunu açmak yapmak için `GameAppDelegate.cs` dosyası (bulunan **BouncingGame** PCL) ve değiştirme `ApplicationDidFinishLaunching` gibi görünüyor şekilde yöntemi:


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

Çalıştırırken, bizim oyun gibi görünür:

![](walkthrough-images/image1.png "Çalıştırırken, oyun görüneceğini")


## <a name="summary"></a>Özet

Bu kılavuzda, yeni oluşturulan .sprintefont dosyasından yeni bir .xnb dosyası oluşturmak nasıl yanı sıra MonoGame ardışık düzen aracının bir giriş .png dosyasından .xnb dosyaları oluşturmak için nasıl kullanılacağı gösterilmiştir. Ayrıca nasıl CocosSharp projeleri .xnb dosyaları kullanmak için yapılandırılır ve bu dosyaları çalışma zamanında yük nasıl ele alınan.

## <a name="related-links"></a>İlgili bağlantılar

- [MonoGame yüklemeleri](http://www.monogame.net/downloads/)
- [MonoGame ardışık düzen belgeleri](http://www.monogame.net/documentation/?page=Pipeline)
- [Başlangıç BouncingGame proje Android (örnek)](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [Ball.PNG grafiği (örnek)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
