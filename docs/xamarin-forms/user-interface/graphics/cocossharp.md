---
title: Xamarin.Forms içinde CocosSharp kullanma
description: CocosSharp kesin şekli, resim ve metin işleme Gelişmiş görselleştirme için uygulama eklemek için kullanılabilir
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: 7ce541134e6db9a26699f96ab3114ced2ad22244
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="using-cocossharp-in-xamarinforms"></a>Xamarin.Forms içinde CocosSharp kullanma

_CocosSharp kesin şekli, resim ve metin işleme Gelişmiş görselleştirme için uygulama eklemek için kullanılabilir_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**2016 gelişmesi: Cocos #'ta Xamarin.Forms**

## <a name="overview"></a>Genel Bakış

CocosSharp grafik görüntüleme, dokunmatik giriş okuma, ses ve yönetme içeriği oynatmayı esnek, güçlü bir teknolojidir. Bu kılavuz, bir Xamarin.Forms uygulaması CocosSharp ekleme açıklar. Aşağıdakileri kapsar:

* [CocosSharp nedir?](#what)
* [CocosSharp Nuget paketleri ekleme](#nuget)
* [İzlenecek yol: Bir Xamarin.Forms uygulaması CocosSharp ekleme](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>CocosSharp nedir?

[CocosSharp](~/graphics-games/cocossharp/index.md) Xamarin platformda kullanılabilir olan bir açık kaynak oyun altyapısıdır.
CocosSharp aşağıdaki özellikleri içeren bir çalışma zamanı verimli kitaplığıdır:

* Görüntü işleme kullanan [CCSprite sınıfı](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* Şekil işleme kullanarak [CCDrawNode sınıfı](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* Çerçeve her mantığı kullanarak [CCNode.Schedule yöntemi](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* (Yükleme ve .png dosyaları gibi kaynakların yüklemeyi kaldırma) İçerik Yönetimi'ni kullanarak [CCTextureCache sınıfı](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* Kullanarak animasyonları [CCAction sınıfı](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

Platformlar arası 2B oyunlar oluşturma işlemini basitleştirmek için CocosSharp'ın birincil odağı olan; Ancak, Xamarin Form uygulamaları harika eklemeyi de olabilir. Oyunlar genellikle verimli oluşturma ve görsel üzerinde tam denetim gerektirir beri CocosSharp güçlü Görselleştirme ve etkileri oyun olmayan uygulamalara eklemek için kullanılabilir.

Xamarin.Forms yerel, platforma özgü UI sistemlerinin üzerine kurulur. Örneğin, [ `Button`s](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) iOS ve Android cihazlarda farklı şekilde görüntülenir ve hatta işletim sistemi sürümüne göre farklılık gösterebilir. Bunun aksine, tüm görsel nesneleri tüm platformlarda aynı görünmesi için CocosSharp hiçbir platforma özel görsel nesneler kullanmaz. Elbette, çözümleme ve en boy oranını cihazlar arasında farklılık gösterir ve bu CocosSharp kendi görselleri nasıl işlediğini etkileyebilir. Bu kılavuzda bu ayrıntıları incelenecektir.

Daha ayrıntılı bilgiler bulunabilir [CocosSharp bölüm](~/graphics-games/cocossharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>CocosSharp Nuget paketleri ekleme

CocosSharp kullanmadan önce geliştiricilerin kendi Xamarin.Forms proje birkaç eklemeler yapmanız gerekir.
Bu kılavuz bir iOS, Android ve .NET standart Xamarin.Forms projeyle varsayar kitaplığı projesi.
Tüm kod .NET standart kitaplığı projesinde yazılır; Ancak, kitaplıklar iOS ve Android projeleri eklenmesi gerekir.

CocosSharp Nuget paketi tüm CocosSharp nesneleri oluşturmak için gereken nesneleri içerir.
CocosSharp.Forms nuget paketini içeren `CocosSharpView` Xamarin.Forms CocosSharp barındırmak için kullanılan sınıf.
Ekleme **CocosSharp.Forms** NuGet ve **CocosSharp** de otomatik olarak eklenir.
Bunu yapmak için sağ <span class="UIItem">paketleri</span> seçin ve .NET standart kitaplığı proje klasöründe <span class="UIItem">paketleri Ekle... </span>. Arama terimi girin <span class="UIItem">CocosSharp.Forms</span>seçin <span class="UIItem">Xamarin.Forms CocosSharp</span>, ardından <span class="UIItem">Paketi Ekle</span>.

![](cocossharp-images/image1.png "Paketleri iletişim ekleyin")

Her ikisi de **CocosSharp** ve **CocosSharp.Forms** NuGet paketlerini projeye eklenir:

![](cocossharp-images/image2.png "Paketler klasörü")

(Örneğin, iOS ve Android) platforma özgü projeleri için yukarıdaki adımları yineleyin.

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>İzlenecek yol: Bir Xamarin.Forms uygulaması CocosSharp ekleme

Xamarin.Forms uygulaması için basit bir CocosSharp görünüm eklemek için aşağıdaki adımları izleyin:

1. [Bir Xamarin oluşturma sayfası oluşturur](#1)
1. [Bir CocosSharpView ekleme](#2)
1. [GameScene oluşturma](#3)
1. [Bir daire ekleme](#4)
1. [CocosSharp ile etkileşim kurma](#5)

Xamarin.Forms uygulaması için başarıyla bir CocosSharp görünüm ekledikten sonra ziyaret [CocosSharp belgelerine](~/graphics-games/cocossharp/index.md) CocosSharp ile içerik oluşturma hakkında daha fazla bilgi edinmek için.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Bir Xamarin oluşturma sayfası oluşturur

CocosSharp herhangi Xamarin.Forms kapsayıcısında barındırılabilir. Bu örnek için bu sayfayı adlı sayfaya kullanan `HomePage`. `HomePage` ikiye tarafından bölünmüş bir `Grid` nasıl Xamarin.Forms ve CocosSharp aynı anda aynı sayfa üzerinde işlenip göstermek için.

İçerdiği için ilk olarak, sayfayı ayarla bir `Grid` ve iki `Button` örnekleri:


```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

İOS, `HomePage` aşağıdaki görüntüde gösterildiği gibi görünür:

![](cocossharp-images/image3.png "Giriş sayfası ekran görüntüsü")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Bir CocosSharpView ekleme

`CocosSharpView` Sınıfı, bir Xamarin.Forms uygulamaya CocosSharp eklemek için kullanılır. Bu yana `CocosSharpView` devraldığı [Xamarin.Forms.View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) sınıfı, yerleşim için bilinen bir arayüz sağlar ve bu gibi düzeni kapsayıcılara kullanılabilir [Xamarin.Forms.Grid](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Yeni bir ekleme `CocosSharpView` tamamlayarak projeye `CreateTopHalf` yöntemi:


```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

CocosSharp başlatma hemen değil, bu nedenle bir olay olduğunda kaydolun `CocosSharpView` oluşturma tamamlandı. Bunu yapmak `HandleViewCreated` yöntemi:


```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

`HandleViewCreated` Yöntemi biz bakarak iki önemli ayrıntıları sahiptir. İlk `GameScene` sonraki bölümde oluşturulan sınıf. Uygulama kadar derlenmez dikkate almak önemlidir `GameScene` oluşturulur ve `gameScene` çözümlenmiş örnek başvuru.

İkinci önemli ayrıntısı `DesignResolution` oyunun görünür alanı CocosSharp nesneler için tanımlar özelliği. `DesignResolution` Özelliği arama sırasında oluşturduktan sonra `GameScene`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. GameScene oluşturma

`GameScene` Sınıfının devraldığı CocosSharp 's `CCScene`. `GameScene` Burada size tamamen CocosSharp ile ilgilenir ilk noktasıdır. Kod içinde yer alan `GameScene` CocosSharp uygulamalarda işlev veya Xamarin.Forms projesi içinde yerleştirilebilir olup olmadığını gösterir.

`CCScene` Tüm CocosSharp işleme visual kökünde bir sınıftır. Herhangi bir görünür CocosSharp nesnesi içinde bulunması gereken bir `CCScene`. Görsel nesneler daha belirgin olarak eklenmelidir `CCLayer` örnekleri ve bu `CCLayer` örnekleri eklenmelidir bir `CCScene`.

Aşağıdaki grafikte, tipik bir CocosSharp hiyerarşi görselleştirmenize yardımcı olabilir:

![](cocossharp-images/image4.png "Tipik CocosSharp hiyerarşisi")

Yalnızca bir `CCScene` aynı anda etkin olabilir. Çoğu birden çok kullanabilmesi `CCLayer` sıralama içeriği ancak uygulamamız örneklerine tek kullanır. Benzer şekilde, birden çok görsel nesneler çoğu oyunlar kullanır, ancak biz yalnızca uygulamamıza birinde sahip olacaksınız. Daha ayrıntılı bir tartışma visual hiyerarşi bulunabilir CocosSharp hakkında [BouncingGame izlenecek](~/graphics-games/cocossharp/bouncing-game.md).

Başlangıçta `GameScene` sınıfı neredeyse boş olacaktır – yalnızca başvurusunda karşılamak için oluşturacağız `HomePage`. Yeni bir sınıf adlı .NET standart kitaplığını projenize eklemek `GameScene`. Gelen alması gerektiğini `CCScene` gibi sınıfı:


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Şimdi `GameScene` olan tanımlanmış biz dönebilirsiniz `HomePage` ve bir alan ekleyin:


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Biz artık Projemizin derleyebilir ve çalışan CocosSharp görmek için çalıştırın. Biz için herhangi bir şey eklemediniz bizim `GameScene,` sayfamızı'nin üst yarısı siyah – CocosSharp Sahne varsayılan rengini gelir:

![](cocossharp-images/image5.png "Boş GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Bir daire ekleme

Uygulama şu anda çalışan bir örneği boş bir görüntüleme CocosSharp altyapısı yok `CCScene`. Ardından, bir görsel nesne ekleyeceğiz: bir daire. `CCDrawNode` Sınıfı kullanılabilir çeşitli geometrik şekiller çizmek için kısmında özetlendiği gibi [çizim geometri CCDrawNode Kılavuzu ile](~/graphics-games/cocossharp/ccdrawnode.md).

Bir daire eklemek bizim `GameScene` sınıfı ve aşağıdaki kodda gösterildiği gibi oluşturucuda örneği:


```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

Şimdi uygulamayı çalıştıran bir daire CocosSharp görüntü alanının sol tarafında gösterir:

![](cocossharp-images/image6.png "Daire GameScene içinde")


#### <a name="understanding-designresolution"></a>Anlama DesignResolution

Görsel bir CocosSharp nesne görüntülenir, biz araştırabilirsiniz `DesignResolution` özelliği.

`DesignResolution` Yerleştirme ve nesneleri boyutlandırma CocosSharp alanının yüksekliğini ve genişliğini temsil eder. Alanın gerçek çözünürlüğü ölçülür *piksel* sırada `DesignResolution` dünyada ölçülen *birimleri*. Aşağıdaki diyagram görünümü bir ekran çözünürlüğü 640 x 1136 piksel ile 5 iPhone görüntülendiği gibi çeşitli kısımlarını çözünürlüğünü gösterir:

![](cocossharp-images/image7.png "iPhone 5s'dir tasarım çözümleme")

Yukarıdaki diyagramda dış siyah metin ekran piksel boyutlarını görüntüler. Birimleri, beyaz metinlerin şemada iç görüntülenir. Yukarıda gösterilen bazı önemli ayrıntıları aşağıdadır:

* CocosSharp görüntü kaynağı sol alt kısmındaki ' dir. Sağa taşıma X değeri ve yukarı taşınması Y değeri çıkarır. Y değeri tersine, bazı diğer 2B Düzen motorları için karşılaştırıldığında fark nerede (0,0) tuvale sol üst olduğu.
* CocosSharp varsayılan davranışını görünümünü en boy oranını korumak için ' dir. İlk satırın kılavuz içinde uzunluğundan daha geniş olduğundan CocosSharp kendi hücre genişliğinin tamamını noktalı beyaz dikdörtgeni tarafından gösterildiği gibi doldurun değil. Bu davranış, açıklandığı şekilde değiştirilebilir [birden çok çözümleri CocosSharp işleme Kılavuzu](~/graphics-games/cocossharp/resolutions.md).
* Bu örnekte, genişlik ve yükseklik boyutuna bakılmaksızın 100 birim bir görüntü alanını ya da kendi aygıtını en boy oranını CocosSharp korur. Bu kod sağ uçta bağlı X = 100 temsil eder CocosSharp görüntülemesini, tüm cihazlarda tutarlı kalmasını sağlama düzenini varsayabilirsiniz anlamına gelir.


#### <a name="ccdrawnode-details"></a>CCDrawNode ayrıntıları

Bizim basit uygulama kullandığı `CCDrawNode` bir daire çizmek için sınıf. Bu sınıf, çünkü vektör tabanlı geometri işleme – Xamarin.Forms eksik bir özellik sağlar iş uygulamaları için çok kullanışlı olabilir. Daire yanı sıra `CCDrawNode` sınıfı, dikdörtgenler, eğrileri, satırları ve özel çokgenler çizmek için kullanılabilir. `CCDrawNode` de görüntü dosyaları (örneğin, .png) kullanımı gerektirmediğinden, kullanımı kolay olur. Daha ayrıntılı bir irdelemesi ve CCDrawNode bulunabilir [çizim geometri CCDrawNode Kılavuzu ile](~/graphics-games/cocossharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. CocosSharp ile etkileşim kurma

CocosSharp görsel öğeleri (gibi `CCDrawNode`) devralınmalıdır `CCNode` sınıfı. `CCNode` nesnenin üst göre konumlandırmak için kullanılan iki özellikleri sağlar: `PositionX` ve `PositionY`. Bizim kod şu anda bu iki özellik dairenin konumlandırmak için bu kod parçacığında gösterildiği gibi kullanır:


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

CocosSharp nesneleri kendi üst düzen denetimleri davranışa göre otomatik olarak konumlandırılmış çoğu Xamarin.Forms görünümleri aksine açık konum değerlerine göre yerleştirilmiş dikkate almak önemlidir.

Daireye 10 birim (CocosSharp world birim alanı daire çizer beri değil piksel cinsinden) tarafından sola veya sağa taşımak için iki düğmelerden birini tıklatın yapmalarına izin vermek için kod ekleyeceğiz. İlk iki ortak yöntemleri oluşturacağız `GameScene` sınıfı:


```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

İçindeki iki düğmelere işleyicileri ardından, ekleyeceğiz `HomePage` tıklamalarına yanıt verme için. Tamamlandığında, bizim `CreateBottomHalf` yöntemine aşağıdaki kodu içerir:


```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

CocosSharp daire şimdi yanıt tıklama olarak taşır. Aynı zamanda açıkça CocosSharp tuvale sınırlarının daireye yetecek kadar sola veya sağa hareket ettirerek görebiliriz:

![](cocossharp-images/image8.png "Daire taşıma konusunda GameScene")

## <a name="summary"></a>Özet

Bu kılavuz için var olan bir Xamarin.Forms CocosSharp eklemeyi gösterir, proje Xamarin.Forms CocosSharp, arasındaki etkileşimi oluşturma ve düzenleri içinde CocosSharp oluştururken, çeşitli hususlar anlatılmaktadır.

Bu kılavuz yalnızca CocosSharp ne yapabileceğinizi yüzeyini sıyırıyor şekilde CocosSharp oyun altyapısı çok fazla işlevsellik ve derinliğini sunar. Geliştiriciler CocosSharp hakkında daha fazla okuma ilginizi birçok makalelerde bulabilirsiniz [CocosSharp bölüm](~/graphics-games/cocossharp/index.md).



## <a name="related-links"></a>İlgili bağlantılar

- [CocosSharp API'leri](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (örnek)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
