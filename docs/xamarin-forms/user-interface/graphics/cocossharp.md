---
title: Xamarin.Forms içinde CocosSharp kullanma
description: CocosSharp kesin şekli, görüntü ve metin işleme için Gelişmiş görselleştirme uygulama eklemek için kullanılabilir
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: c823eb27552f0a42ad428ed6f36790e925079295
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998813"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Xamarin.Forms içinde CocosSharp kullanma

_CocosSharp kesin şekli, görüntü ve metin işleme için Gelişmiş görselleştirme uygulama eklemek için kullanılabilir_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**2016 evrim Geçiren: Cocos # Xamarin.Forms içinde**

## <a name="overview"></a>Genel Bakış

CocosSharp, grafik görüntüleme, dokunma girişini okuma, ses ve yönetirken içerikleri oynatmayı esnek ve güçlü bir teknolojidir. Bu kılavuzda, bir Xamarin.Forms uygulaması CocosSharp ekleme açıklanmaktadır. Aşağıdakileri içerir:

* [CocosSharp nedir?](#what)
* [CocosSharp Nuget paketleri ekleme](#nuget)
* [İzlenecek yol: CocosSharp için Xamarin.Forms uygulaması ekleme](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>CocosSharp nedir?

[CocosSharp](~/graphics-games/cocossharp/index.md) Xamarin platformda kullanılabilir bir açık kaynak oyun altyapısı.
CocosSharp aşağıdaki özellikleri içeren bir çalışma zamanı verimli kitaplığıdır:

* Görüntü işleme kullanan [CCSprite sınıfı](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* Şekil işleme kullanarak [CCDrawNode sınıfı](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* Her kare mantığı kullanılarak [CCNode.Schedule yöntemi](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* (Yükleme ve .png dosyaları gibi kaynakları kaldırma) İçerik Yönetimi'ni kullanarak [CCTextureCache sınıfı](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* Animasyonları kullanarak [CCAction sınıfı](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

Platformlar arası 2B oyunlar oluşturulmasını kolaylaştırmak için CocosSharp'in birincil odağı olan; Ancak, harika bir Xamarin Form uygulamaları eklemeyi de olabilir. Oyunlar genellikle verimli işleme ve görsellerin üzerinde kesin denetim gerektirdiğinden, CocosSharp güçlü Görselleştirme ve etkileri oyun içi uygulamalara eklemek için kullanılabilir.

Xamarin.Forms, yerel, platforma özgü kullanıcı Arabirimi sistemleri üzerinde oluşturulmuştur. Örneğin, [ `Button`s](xref:Xamarin.Forms.Button) iOS ve Android üzerinde farklı şekilde görüntülenir ve hatta işletim sistemi sürümüne göre farklılık gösterebilir. Bunun aksine, tüm görsel nesneler tüm platformlarda aynı görünmesi için CocosSharp hiçbir platforma özel görsel nesneler kullanmaz. Elbette, çözümleme ve en boy oranını cihazlar arasında farklılık gösterir ve bu CocosSharp görselleri nasıl işlediğini etkileyebilir. Bu ayrıntılar bu kılavuzun sonraki bölümlerinde açıklanmıştır.

Daha ayrıntılı bilgi bulunabilir [CocosSharp bölümü](~/graphics-games/cocossharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>CocosSharp Nuget paketleri ekleme

Geliştiriciler, CocosSharp kullanmadan önce bir Xamarin.Forms projesi bazı eklemeler yapmanız gerekir.
Bu kılavuz, bir Xamarin.Forms projesi ile bir iOS, Android ve .NET Standard varsayar kitaplığı projesi.
Tüm kodlar .NET Standard kitaplığı projesinde yazılır; Ancak, iOS ve Android projeleri için kitaplıkları eklenmelidir.

CocosSharp Nuget paketini CocosSharp nesneleri oluşturmak için gereken tüm nesneler içerir.
CocosSharp.Forms nuget paketini içeren `CocosSharpView` Xamarin.Forms içinde CocosSharp barındırmak için kullanılan sınıf.
Ekleme **CocosSharp.Forms** NuGet ve **CocosSharp** de otomatik olarak eklenir.
Bunu yapmak için sağ <span class="UIItem">paketleri</span> seçin ve .NET Standard kitaplığı proje klasöründe <span class="UIItem">paketleri Ekle... </span>. Arama terimi girin <span class="UIItem">CocosSharp.Forms</span>seçin <span class="UIItem">Xamarin.Forms için CocosSharp</span>, ardından <span class="UIItem">Paketi Ekle</span>.

![](cocossharp-images/image1.png "Paketler iletişim kutusu Ekle")

Her ikisi de **CocosSharp** ve **CocosSharp.Forms** NuGet paketleri projeye eklenecek:

![](cocossharp-images/image2.png "Paketler klasörü")

Platforma özgü projeleri (örneğin, iOS ve Android) için yukarıdaki adımları yineleyin.

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>İzlenecek yol: CocosSharp için Xamarin.Forms uygulaması ekleme

Xamarin.Forms uygulaması için basit CocosSharp görünüm eklemek için aşağıdaki adımları izleyin:

1. [Sayfa Forms bir Xamarin oluşturma](#1)
1. [Bir CocosSharpView ekleme](#2)
1. [GameScene oluşturma](#3)
1. [Bir daire ekleme](#4)
1. [CocosSharp ile etkileşim kurma](#5)

Xamarin.Forms uygulaması için bir CocosSharp görünüm başarıyla ekledikten sonra ziyaret [CocosSharp belgeleri](~/graphics-games/cocossharp/index.md) CocosSharp ile içerik oluşturma hakkında daha fazla bilgi edinmek için.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Sayfa Forms bir Xamarin oluşturma

Tüm Xamarin.Forms kapsayıcısında CocosSharp barındırılabilir. Bu sayfa için bu örnek adlı bir sayfa kullanan `HomePage`. `HomePage` tarafından ikiye bölünür bir `Grid` nasıl Xamarin.Forms ve CocosSharp aynı anda aynı sayfada işlenebilecek gösterilecek.

İçerdiği için Page up ilk olarak ayarlanmış bir `Grid` ve iki `Button` örnekleri:


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

İos'ta `HomePage` aşağıdaki görüntüde gösterildiği gibi görünür:

![](cocossharp-images/image3.png "Giriş sayfasının ekran görüntüsü")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Bir CocosSharpView ekleme

`CocosSharpView` Sınıfı, bir Xamarin.Forms uygulamaya CocosSharp eklemek için kullanılır. Bu yana `CocosSharpView` devraldığı [Xamarin.Forms.View](xref:Xamarin.Forms.View) sınıfı düzeni için bilinen bir arayüz sağlar ve bu gibi Düzen kapsayıcıları içinde kullanılabilir [Xamarin.Forms.Grid](xref:Xamarin.Forms.Grid). Yeni bir `CocosSharpView` tamamlayarak projeye `CreateTopHalf` yöntemi:


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

CocosSharp başlatma hemen değil, bu nedenle bir olay olduğunda kayıt `CocosSharpView` oluşturma tamamlandı. Bunu yapmak `HandleViewCreated` yöntemi:


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

`HandleViewCreated` Biz göz atan iki önemli ayrıntıları yöntemi vardır. İlk `GameScene` sonraki bölümde oluşturulan sınıf. Uygulama kadar derlemeyecektir dikkat edin önemlidir `GameScene` oluşturulur ve `gameScene` çözümlenmiş örnek başvuru.

İkinci önemli ayrıntısı `DesignResolution` özelliği CocosSharp nesneleri için oyunun görünür alanı tanımlar. `DesignResolution` Özellik aranabilir oluşturduktan sonra `GameScene`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. GameScene oluşturma

`GameScene` Sınıfından devralan CocosSharp 's `CCScene`. `GameScene` Biz burada tamamen CocosSharp ile uğraşmak ilk noktasıdır. İçindeki kod `GameScene` veya bir Xamarin.Forms projesi içinde bünyesinde olmadığını CocosSharp uygulamalarda çalışmaz.

`CCScene` Tüm CocosSharp işleme visual kökünde bir sınıftır. Herhangi bir görünür CocosSharp nesnenin içinde bulunması gereken bir `CCScene`. Görsel nesneler daha açık belirtmek gerekirse eklenmelidir `CCLayer` örnekleri ve bunları `CCLayer` örnekleri eklenmelidir bir `CCScene`.

Aşağıdaki grafikte, tipik bir CocosSharp hiyerarşi görselleştirmenize yardımcı olabilir:

![](cocossharp-images/image4.png "Tipik CocosSharp hiyerarşisi")

Yalnızca bir `CCScene` tek seferde etkin olabilir. Çoğu oyun kullanın `CCLayer` içeriği sıralama, ancak uygulamamız örneklerine tek kullanır. Benzer şekilde, birden çok görsel nesneler çoğu oyun kullanır, ancak biz yalnızca birinde uygulamamız gerekir. Daha ayrıntılı bir tartışma visual hiyerarşi içinde bulunabilir CocosSharp hakkında [BouncingGame izlenecek](~/graphics-games/cocossharp/bouncing-game.md).

Başlangıçta `GameScene` sınıfı neredeyse boş olacaktır – yalnızca başvuru karşılamak için oluşturacağız `HomePage`. Yeni bir sınıf adlı .NET Standard kitaplığı projenize ekleyin `GameScene`. Devralınacak `CCScene` sınıfına aşağıdaki gibi:


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

Biz artık Projemizin derleyebilir ve çalışan CocosSharp görmek için çalıştırın. Biz için herhangi bir şey eklemediniz bizim `GameScene,` – CocosSharp Sahne varsayılan rengi siyah sayfamızı üst yarısında, bu nedenle:

![](cocossharp-images/image5.png "Boş GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Bir daire ekleme

Uygulama şu anda boş görüntüleme CocosSharp altyapısı, çalışan bir örneğini yok `CCScene`. Ardından, bir görsel nesneyi ekleyeceğiz: bir daire. `CCDrawNode` Sınıfı kullanılabilir çeşitli geometrik şekiller çizmek için açıklandığı gibi [Kılavuzu CCDrawNode ile geometri çizim](~/graphics-games/cocossharp/ccdrawnode.md).

Bir dairenin ekleme bizim `GameScene` sınıfı ve aşağıdaki kodda gösterildiği gibi oluşturucuda örneği:


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

Uygulama artık çalışan bir daire CocosSharp görünen alanın sol tarafında gösterir:

![](cocossharp-images/image6.png "GameScene daire")


#### <a name="understanding-designresolution"></a>DesignResolution anlama

Bir görsel CocosSharp nesne görüntülenir, ki araştırabilirsiniz `DesignResolution` özelliği.

`DesignResolution` Yerleştirme ve nesneleri boyutlandırma CocosSharp alanının yüksekliğini ve genişliğini temsil eder. Alanının gerçek çözünürlük ölçülür *piksel* sırada `DesignResolution` dünyada ölçülür *birimleri*. Aşağıdaki diyagramda gösterildiği bir ekran çözünürlüğü 640 x 1136 piksel ile iPhone 5 görünümü çeşitli bölümlerini çözünürlüğünü gösterir:

![](cocossharp-images/image7.png "iPhone 5 sn tasarım çözümleme")

Yukarıdaki diyagramda, siyah metin ekran dış piksel boyutlarını görüntüler. Birimleri, beyaz metinli diyagramda iç görüntülenir. Yukarıda gösterilen bazı önemli ayrıntılar aşağıda verilmiştir:

* CocosSharp görüntü kaynağı sol alt kısmında ' dir. Sağa doğru ilerlendiğinde X değeri artırır ve yukarı taşınması Y değeri artırır. Bazı diğer 2B Düzen motorları için ve göre fark Y değerini tersine, burada (0,0) tuvalin sol üst.
* Varsayılan CocosSharp görünümünü en boy oranını korumak için davranışıdır. Kılavuzdaki ilk satırın uzunluğundan daha geniş olduğu CocosSharp noktalı beyaz bir dikdörtgen ile gösterildiği gibi hücre genişliğinin tamamını dolmaması. Bu davranış, açıklanan şekilde değiştirilebilir [işleme Cocossharp'ta birden fazla yol](~/graphics-games/cocossharp/resolutions.md).
* Bu örnekte, genişlik ve yükseklik boyutundan bağımsız olarak 100 birim görüntüleme alanı ya da kendi aygıtını en boy oranını CocosSharp tutacaktır. Bu kodu en sağdaki bağlı X = 100 temsil CocosSharp, tüm cihazlarda tutarlı kalmasını sağlama düzeni görüntülemek olduğunu varsayabilirsiniz anlamına gelir.


#### <a name="ccdrawnode-details"></a>CCDrawNode ayrıntıları

Bizim basit uygulama kullanan `CCDrawNode` sınıfı bir daire çizin. Bu sınıf, çünkü Xamarin.Forms eksik bir özellik – geometri vektör tabanlı işleme sağlar iş uygulamaları için çok yararlı olabilir. Daire yanı sıra `CCDrawNode` sınıfı dikdörtgenler, eğrileri çizgiler ve özel çokgenler çizmek için kullanılabilir. `CCDrawNode` görüntü dosyaları (örneğin, .png) kullanımı gerektirmediğinden da kolayca kullanılabilir. CCDrawNode ile daha ayrıntılı bir açıklaması bulunabilir [Kılavuzu CCDrawNode ile geometri çizim](~/graphics-games/cocossharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. CocosSharp ile etkileşim kurma

CocosSharp görsel öğeleri (gibi `CCDrawNode`) devralınacak `CCNode` sınıfı. `CCNode` üst öğesiyle ilişkili bir nesne yerleştirmek için kullanılabilen iki özellikleri sağlar: `PositionX` ve `PositionY`. Kodlarımızın şu anda bu iki özellik Orta dairenin konumlandırmak için bu kod parçacığında gösterildiği gibi kullanır:


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

CocosSharp nesneleri otomatik olarak üst düzen denetimlerini davranışını göre konumlandırılır çoğu Xamarin.Forms görünümleri aksine açık konum değerlerine göre konumlandırılır dikkat edin önemlidir.

Daire 10 birime (CocosSharp dünya birim alanı daire çizen beri değil piksel cinsinden) tarafından sola veya sağa taşımak için iki düğmelerden birine tıklayarak izin vermek için kod ekleyeceğiz. İlk iki genel yöntemleri oluşturacağız `GameScene` sınıfı:


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

İki düğme için işleyiciler sonra ekleyeceğiz `HomePage` tıklamalarına yanıt verme için. İşiniz bittiğinde, bizim `CreateBottomHalf` yöntem aşağıdaki kodu içerir:


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

CocosSharp daire tıklamalara yanıt artık taşır. Aynı zamanda açıkça CocosSharp tuval sınırları daire yeteri kadar sola veya sağa hareket ettirerek görebiliriz:

![](cocossharp-images/image8.png "Daire taşıma konusunda GameScene")

## <a name="summary"></a>Özet

Bu kılavuz, mevcut bir Xamarin.Forms için CocosSharp ekleme işlemi gösterilmektedir, proje oluşturma Xamarin.Forms ile CocosSharp, arasında etkileşim ve düzenleri içinde CocosSharp oluştururken çeşitli konuları anlatılmaktadır.

Bu kılavuzu yalnızca CocosSharp neler yapabileceğinizi yüzeysel bir açıklamadır için çok fazla işlevsellik ve derinliğini CocosSharp oyun motorunu sunar. Geliştiriciler okuma CocosSharp hakkında daha fazla bilgi edinmek, birçok makalelerde bulabilirsiniz [CocosSharp bölümü](~/graphics-games/cocossharp/index.md).



## <a name="related-links"></a>İlgili bağlantılar

- [CocosSharp API'leri](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (örnek)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
