---
title: Bölüm 2 – WalkingGame uygulama
description: Bu kılavuz, oyun mantığı ve içeriği ile taşıma animasyonlu bir hareketli grafik gösterimini oluşturmak için boş bir MonoGame projesine giriş touch eklemek gösterilmiştir.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 87678d9d77f75bccc68a667d3fb0f35b641b937c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921670"
---
# <a name="part-2--implementing-the-walkinggame"></a>Bölüm 2 – WalkingGame uygulama

_Bu kılavuz, oyun mantığı ve içeriği ile taşıma animasyonlu bir hareketli grafik gösterimini oluşturmak için boş bir MonoGame projesine giriş touch eklemek gösterilmiştir._

Bu kılavuzun önceki bölümleri boş MonoGame projeleri nasıl oluşturulacağını gösterir. Basit bir oyun tanıtım yaparak Biz bu önceki bölümleri oluşturacaksınız. Bu makalede aşağıdaki bölümleri içerir:

- Bizim Oyun içeriği unzipping
- MonoGame sınıfına genel bakış
- Bizim ilk hareketli grafik oluşturma
- CharacterEntity oluşturma
- Oyuna CharacterEntity ekleme
- Animasyon sınıfı oluşturma
- CharacterEntity için ilk animasyon ekleme
- Taşıma için karakter ekleme
- Taşıma ve animasyon eşleştirme


## <a name="unzipping-our-game-content"></a>Bizim Oyun içeriği unzipping

Kod yazmaya başlamadan önce biz bizim oyun sıkıştırmasını isteyeceksiniz *içerik*. Oyun geliştiriciler sık kullandığınız terimi *içerik* genellikle visual sanatçılar, oyun tasarımcıları ya da ses tasarımcıları tarafından oluşturulan kodu olmayan dosyaları belirtmek için. Ortak içerik türlerini görselleri görüntülemek, ses çalma veya yapay bilgileri (AI) davranışını denetlemek için kullanılan dosyalar içerir. Oyun Geliştirme ekibinizin perspektif içeriği genellikle programcıları olmayan tarafından oluşturulur.

Burada kullanılan içerik bulunabilir [github'da](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true). Biz bu kılavuzda daha sonra erişimi bir konuma yüklenen şu dosyaları gerekir.

## <a name="monogame-class-overview"></a>MonoGame sınıfına genel bakış

Yeni başlayanlar biz temel işlemede kullanılan MonoGame sınıfları inceleyeceksiniz:

- `SpriteBatch` – 2B grafik ekrana çizmek için kullanılan. *Hareketli grafik* ekranda görüntüleri göstermek için kullanılan 2B görsel öğeler. `SpriteBatch` Nesne çizmek tek bir hareketli grafik bir zaman arasında kendi `Begin` ve `End` yöntemleri ya da birden fazla hareketli grafik birlikte gruplandırılabilir veya *toplu*.
- `Texture2D` – Çalışma zamanında bir görüntü nesneyi temsil eder. `Texture2D` Bunlar ayrıca dinamik olarak çalışma zamanında oluşturulabilir olsa da örnekleri genellikle dosya biçimlerden .png veya .bmp, gibi oluşturulur. `Texture2D` örnekleri ile oluşturulurken kullanılan `SpriteBatch` örnekleri.
- `Vector2` – genellikle görsel nesneler konumlandırma için kullanılan 2B koordinat sisteminde bir konumu temsil eder. MonoGame de içeren `Vector3` ve `Vector4` ancak yalnızca kullanacağız `Vector2` bu kılavuzda.
- `Rectangle` – bir dört taraflı alanıyla konum, genişlik ve yükseklik. Biz bu hangi kısmının tanımlamak için kullanmaya başlayacağınız bizim `Texture2D` biz animasyonları ile çalışmaya başladığınızda işlenecek.

Biz de MonoGame rağmen ad alanı Microsoft tarafından – tutulmayan unutmamalısınız. `Microsoft.Xna` Ad alanı içinde MonoGame mevcut XNA projeleri için MonoGame geçirmek kolaylaştırmak için kullanılır.

## <a name="rendering-our-first-sprite"></a>Bizim ilk hareketli grafik oluşturma

Sonraki biz MonoGame 2B işleme gerçekleştirme göstermek için ekrana tek bir hareketli grafik çizer.

### <a name="creating-a-texture2d"></a>Bir Texture2D oluşturma

Oluşturmamız gerekir bir `Texture2D` bizim hareketli grafik oluşturulurken kullanılacak örneği. Tüm oyun içeriği sonuçta adlı bir klasörde yer **içerik** platforma özgü projesinin içinde bulunur. İçerik platformuna özel yapı eylemleri kullanmanız gerektiği gibi paylaşılan MonoGame projeleri içerik içeremez. CocosSharp geliştiriciler bilinen bir kavram içerik klasörü bulacaksınız – bunlar CocosSharp ve MonoGame projeleri aynı yerde bulunur. İçerik klasörünü, iOS projesi ve varlıklar klasör Android projesi içinde bulunabilir.

Bizim oyunun içeriği eklemek için sağ **içerik** klasörü ve select **Ekle > dosyaları Ekle...** Content.zip dosya ayıkladığınız konumuna giderek seçin **charactersheet.png** dosya. Klasöre dosya ekleme hakkında sorular, biz seçmelisiniz **kopyalama** seçeneği:

![](part2-images/image1.png "Klasöre dosya ekleme hakkında sorular, Kopyala seçeneğini belirleyin.")

İçerik klasörünü artık charactersheet.png dosya içerir:

![](part2-images/image2.png "İçerik klasörü şimdi charactersheet.png dosyasını içerir")

Ardından, charactersheet.png dosyasını yüklemek ve oluşturmak için kodu ekleyeceğiz bir `Texture2D`. Bu açık yapmak için `Game1.cs` dosyasını açıp aşağıdaki alan Game1.cs sınıfına ekleyin:

```csharp
Texture2D characterSheetTexture;
```

Ardından, oluşturacağız `characterSheetTexture` içinde `LoadContent` yöntemi. Tüm değişiklikler önce `LoadContent` yöntemi şöyle görünür:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

Biz varsayılan proje zaten başlatır unutmamalısınız `spriteBatch` bize için örneği. Biz bu daha sonra kullanacağız ancak biz hazırlamak için herhangi bir ek kod ekleme olmaz `spriteBatch` kullanmak için. Diğer taraftan, bizim `spriteSheetTexture` biz bundan sonra başlatacak şekilde başlatma gerektiriyor mu `spriteBatch` oluşturulur:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

Biz sahip olduğunuza göre bir `SpriteBatch` örneği ve `Texture2D` biz işleme kodumuza ekleyebilirsiniz örneği `Game1.Draw` yöntemi:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Oyun şimdi çalıştıran charactersheet.png oluşturulan doku görüntüleme tek bir hareketli grafik gösterir:

![](part2-images/image3.png "Oyun şimdi çalıştıran charactersheet.png oluşturulan doku görüntüleme tek bir hareketli grafik gösterir")

## <a name="creating-the-characterentity"></a>CharacterEntity oluşturma

Tek bir hareketli grafik ekranına işlemek için kod kadarki ekledik; Ancak, diğer sınıflar oluşturmadan tam oyun geliştirmek için bulamadığımız, biz kodu kuruluş sorunla çalışır.

### <a name="what-is-an-entity"></a>Bir varlık nedir?

Oyun kod düzenlemek için genel bir desen her oyun için yeni bir sınıf oluşturmaktır *varlık* türü. Bir varlık oyun geliştirme (tümü gerekli değildir) aşağıdaki özelliklere bazıları içerebilen bir nesnedir:

- Hareketli grafik, metin veya 3D modeli gibi bir görsel öğe
- Fizik veya yolu ayarla veya giriş yanıtlama player karakter patrolling bir birim gibi her bir çerçeve davranış
- Oluşturulan ve dinamik olarak gücü görünmesini ve oynatıcısının toplanmakta olan gibi yok

Varlık kuruluş sistemleri karmaşık olabilir ve bu birçok oyun motorları varlıklar yönetmenize yardımcı olmak için sınıflar sağlar. Tam oyunlar genellikle Geliştirici Bölümü hakkında daha fazla kuruluş gerektiren belirtmeye değer olacak şekilde biz çok basit varlık sistem uygulanması.


### <a name="defining-the-characterentity"></a>CharacterEntity tanımlama

Arayacağız bizim varlık `CharacterEntity`, aşağıdaki özelliklere sahiptir:

- Kendi yükleme yeteneği `Texture2D`
- Kendisini yürüyen animasyon güncelleştirmek için arama yöntemleri içeren dahil olmak üzere işleme yeteneği
- `X` ve Y özelliklerini karakterin konumu denetlemek için.
- Özelliği kendisini – özellikle güncelleştirmek için dokunma değerlerinden ekran ve konumu uygun şekilde ayarlayın.

Eklemek için `CharacterEntity` bizim oyun, sağ tıklatın ya da denetim tıklatıldığında **WalkingGame** proje ve seçin **Ekle > yeni dosya...** . Seçin **boş sınıfı** seçeneği ve yeni dosya adı **CharacterEntity**, ardından **yeni**.

Özelliği ilk ekleyeceğiz `CharacterEntity` yüklemek için bir `Texture2D` kendisini çizmek için iyi. Biz yeni eklenen değiştirecek `CharacterEntity.cs` gibi dosya:

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

Yukarıdaki kod konumlandırma, oluşturma ve yükleme için içeriği sorumluluğu ekler `CharacterEntity`. Yukarıdaki kod gerçekleştirilecek bazı noktalar çıkış noktası için bir dakikanızı atalım.

### <a name="why-are-x-and-y-floats"></a>Neden olduğu X ve Y float?

Oyun programlama için yeni olan geliştiriciler neden merak ediyor `float` türü tersine kullanılıyor `int` veya `double`. 32-bitlik bir değer alt düzey işleme kodda konumlandırma için en yaygın nedenidir. Çift tür, nadiren nesneleri konumlandırma için gerekli olan duyarlık için 64 bit kaplar. Bir 32 bit fark Önemsiz görünebilir, ancak birçok modern oyun on binlerce konum değerleri işleme gerektiren grafik her çerçeve (30 veya 60 kez saniye başına) içerir. Bellek miktarı kesme gitmesi grafik ardışık düzen tarafından yarısı oyunun performans üzerinde önemli bir etkisi olabilir.

`int` Veri türü, bir uygun ölçü konumlandırma için olabilir, ancak varlıklar konumlandırıldığı yolda sınırlamaları yerleştirebilirsiniz. Örneğin, tamsayı değerleri kullanarak kesintisiz taşıma yakınlaştırma veya 3B kameralar destekleyen oyunlar için uygulama zorlaştıran (tarafından izin verilir `SpriteBatch`).

Son olarak, ekran bizim karakter taşır mantığı bunu oyunun zamanlama değerleri kullanarak şekilde yapmak görürsünüz. Kullanmak ihtiyacımız şekilde verilen çerçevede ne kadar zaman geçtikten hız çarparak sonucunu bir tam sayı nadiren neden olacak `float` için `X` ve `Y`.

### <a name="why-is-charactersheettexture-static"></a>CharacterSheetTexture neden olduğunu statik?

`characterSheetTexture` `Texture2D` Charactersheet.png dosya çalışma zamanı gösterimini örneğidir. Diğer bir deyişle, her piksel renk değerlerini kaynağından ayıklanan gibi içeren **charactersheet.png** dosya. Bizim oyun iki oluşturmak için olsaydı `CharacterEntity` örnekleri her biri erişim charactersheet.png bilgileri gerekir. Bu durumda biz charactersheet.png iki kez yüklenirken performans maliyeti tabi istemezsiniz veya ram depolanan yinelenen doku bellek olmasını istersiniz. Kullanarak bir `static` alan verir bize aynı paylaşmak `Texture2D` tüm `CharacterEntity` örnekleri.

Kullanarak `static` üyeleri için içerik çalışma zamanı gösterimini simplistic bir çözümdür ve yüklemeyi kaldırma içeriği gibi daha büyük oyunlarda bir düzeyinden diğerine geçerken karşılaşılan sorunları adresi değil. Bu kılavuzun kapsamı dışındadır olan, daha karmaşık çözümler kullanılarak MonoGame'nın içerik ardışık düzeni veya özel bir içerik yönetim sistemi oluşturma içerir.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>Neden bir bağımsız değişken INSTEAD of örneği varlık tarafından olarak SpriteBatch geçirilen mi?

`Draw` Alır uygulandığı şekilde yöntemi bir `SpriteBatch` bağımsız değişkeni, ancak buna karşın `characterSheetTexture` tarafından örneği `CharacterEntity`.

Bunun nedeni en verimli işleme olasıdır çünkü aynı `SpriteBatch` örneği kullanılan tüm `Draw` çağrıları ve ne zaman tüm `Draw` çağrıları tek bir dizi arasında yapılan `Begin` ve `End` çağrıları. Elbette, bizim oyun yalnızca tek bir varlık örneği içerir, ancak daha karmaşık oyunlar aynı kullanmak birden çok varlık veren düzeni yararlanacaktır `SpriteBatch` örneği.


## <a name="adding-characterentity-to-the-game"></a>Oyuna CharacterEntity ekleme

Ekledik göre bizim `CharacterEntity` kendisini işlemeye koduyla biz kodda değiştirebilirsiniz `Game1.cs` bu yeni bir varlık örneği kullanmak için. Bunu değiştir yapmak için `Texture2D` ile alan bir `CharacterEntity` alanındaki `Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

Ardından, bu varlık örneklemesi ekleyeceğiz `Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

Biz de kaldırmanız gerekir `Texture2D` oluşturulması `LoadContent` , artık içinde işlenir beri bizim `CharacterEntity`. `Game1.LoadContent` aşağıdaki gibi görünmelidir:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

Adını rağmen dikkate değerdir `LoadContent` yöntemi burada içerik yüklenebilir – player ulaşana kadar birçok oyunlar uygulama içerik kendi Initialize yöntemini veya hatta kendi kodunun içerik olarak yüklenmesi gerekli olmayabilir tek yer değil bir Oyun belirli noktası.

Son olarak Draw yöntemi aşağıdaki gibi değiştirebilirsiniz:


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Biz oyun çalıştırırsanız, şimdi karakter göreceğiz. Varsayılan 0 X ve Y bu yana karakter ekranın sol üst köşesinde karşı yerleştirilir:

![](part2-images/image4.png "Varsayılan 0 X ve Y bu yana karakter ekranın sol üst köşesinde karşı sonra konumlandırılır")

## <a name="creating-the-animation-class"></a>Animasyon sınıfı oluşturma

Şu anda bizim `CharacterEntity` tam görüntüler **charactersheet.png** dosya. Bu düzenlemenin bir dosyada birden çok görüntü olarak adlandırılır bir *hareketli grafik sayfası*. Genellikle, bir hareketli grafik hareketli grafik sayfası yalnızca bir kısmını işlemez. Biz değiştirecek `CharacterEntity` bu kısmı işlemek için **charactersheet.png**, ve bu bölümü yürüyen animasyon görüntülemek için zaman içinde değişir.

Oluşturacağız `Animation` mantığı ve CharacterEntity animasyon durumunu denetlemek için sınıf. Animasyon sınıfı herhangi bir varlığa yalnızca kullanılabilen genel bir sınıf olacaktır `CharacterEntity` animasyonları. Ultimate `Animation` sınıfı sağlayacaktır bir `Rectangle` hangi `CharacterEntity` kendisini çizerken kullanır. Ayrıca oluşturacağız bir `AnimationFrame` animasyon tanımlamak için kullanılan sınıfı.


### <a name="defining-animationframe"></a>AnimationFrame tanımlama

`AnimationFrame` animasyon ilgili herhangi bir mantık içermez. Biz, yalnızca verileri depolamak için kullanırsınız. Eklemek için `AnimationFrame` sınıfı, sağ tıklatın veya denetim tıklatıldığında **WalkingGame** paylaşılan proje ve select **Ekle > yeni dosya...** Bir ad girin **AnimationFrame** tıklatıp **yeni** düğmesi. Biz değiştireceksiniz `AnimationFrame.cs` aşağıdaki kodu içeren dosya:


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

`AnimationFrame` Sınıfı iki parça bilgi içerir:

- `SourceRectangle` – Alanını tanımlar `Texture2D` tarafından görüntüleneceği `AnimationFrame`. Bu değer, üst sol olan (0, 0) ile piksel cinsinden ölçülür.
- `Duration` – Ne kadar süreyle tanımlayan bir `AnimationFrame` kullanıldığında görüntülenen bir `Animation`.

### <a name="defining-animation"></a>Animasyon tanımlama

`Animation` İçereceği sınıfı bir `List<AnimationFrame>` hangi çerçeve ne kadar zaman geçtikten göre şu anda görüntülenen geçiş yapmak için mantığı yanı sıra.

Eklemek için `Animation` sınıfı, sağ tıklatın veya denetim tıklatıldığında **WalkingGame** paylaşılan proje ve select **Ekle > yeni dosya...** . Bir ad girin **animasyon** tıklatıp **yeni** düğmesi. Biz değiştireceksiniz `Animation.cs` aşağıdaki kodu içerecek şekilde dosya:


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

Bazı ayrıntılar bakalım `Animation` sınıfı.

### <a name="frames-list"></a>Çerçeve listesi

`frames` Üyesidir ne ekleyebilmemiz için verileri depolar. Animasyon başlatır kod ekleyeceksiniz `AnimationFrame` için örnekler `frames` aracılığıyla listesinde `AddFrame` yöntemi. Daha eksiksiz bir uygulama sunabilir `public` yöntemler veya değiştirmek için Özellikler `frames`, ancak bu kılavuz çerçeveler ekleme işlevselliğini sınırla.

### <a name="duration"></a>Süre

Süre döndürür toplam süresi `Animation,` hangi içerilen tüm süre eklenerek elde edilir `AnimationFrame` örnekleri. Bu değer, ise önbelleğe `AnimationFrame` değişmez nesne olan, ancak biz animasyonuna eklendikten sonra değiştirilebilen bir sınıf olarak AnimationFrame uygulanan olduğundan, bu değeri özellik erişildiğinde hesaplamak ihtiyacımız.

### <a name="update"></a>Güncelleştirme

`Update` Yöntemi her çerçeve (diğer bir deyişle, tüm oyun güncelleştirilir her zaman) adlı. Amacı artırmaktır `timeIntoAnimation` şu anda görüntülenen çerçeve döndürmek için kullanılan üyesi. Mantık `Update` engeller `timeIntoAnimation` engeller şimdiye kadar tüm animasyon süreden daha büyük.

Biz kullanmaya başlayacağınız için `Animation` sonuna ulaştı zaman yineleyin bu animasyon olmasını istiyoruz sonra yürüyen animasyon görüntülenecek sınıf. Biz olmadığını denetleyerek gerçekleştirebilirsiniz `timeIntoAnimation` büyük `Duration` değeri. Öyleyse başına geri döngü ve döngü animasyonda kalanı koruyabilirsiniz.

### <a name="obtaining-the-current-frames-rectangle"></a>Geçerli çerçevenin dikdörtgen alma

Amacı `Animation` sınıfı, sonuçta sağlamak için bir `Rectangle` kullanılabileceği bir hareketli grafik çizerken. Şu anda `Animation` sınıfı değiştirmek için mantığı içeren `timeIntoAnimation` da edinilir kullanacağız üye `Rectangle` görüntülenmesi gerekir. Biz oluşturarak gerçekleştirirsiniz bir `CurrentRectangle` özelliğinde `Animation` sınıfı. Bu özellik içine kopyalamak `Animation` sınıfı:

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime;
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>CharacterEntity için ilk animasyon ekleme

`CharacterEntity` Taramasını ve konumu, hem de geçerli bir başvuru animasyonlarını içerecek `Animation` görüntüleniyor.

İlk olarak, bizim ilk ekleyeceğiz `Animation,` , o ana kadarki yazıldığı şekilde işlevselliğini sınamak için kullanacağız. Aşağıdaki üyeleri CharacterEntity sınıfına ekleyelim:

```csharp
Animation walkDown;
Animation currentAnimation;
```

Ardından, şimdi tanımlamak `walkDown` animasyon. Yapmak için bunu değiştirmek `CharacterEntity` şekilde Oluşturucusu:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Daha önce belirtildiği gibi çağırmak ihtiyacımız `Animation.Update` yürütmek animasyonların zaman tabanlı. Ayrıca atamak ihtiyacımız `currentAnimation`. Şimdi biz atama `currentAnimation` için `walkDown`, ancak biz taşıma mantığımızı uyguladığınızda Biz bu kodu daha sonra değiştirme. Ekleyeceğiz `Update` yönteme `CharacterEntity` gibi:


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Biz sahip olduğunuza `currentAnimation` olan atanan ve güncelleştirilmiş, biz bunu bizim çizim gerçekleştirmek için kullanabilirsiniz. Biz değiştireceksiniz `CharacterEntity.Draw` gibi:

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

Alma için son adımı `CharacterEntity` animasyon çağırmaktır yeni eklenen `Update` yönteminden `Game1`. Değiştirme `Game1.Update` gibi:

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

Şimdi `CharacterEntity` oynayacağı kendi `walkDown` animasyon:

![](part2-images/image5.gif "CharacterEntity walkDown animasyonunun şimdi oynayacağı")

## <a name="adding-movement-to-the-character"></a>Taşıma için karakter ekleme

Ardından, biz taşıma dokunma denetimlerini kullanarak bizim karakter ekleme. Kullanıcının ekran dokunduğunda karakter ekran burada işlemdeki noktası taşınır. Hiçbir rötuşları algılanırsa karakter yerinde görünecektir.

### <a name="defining-getdesiredvelocityfrominput"></a>GetDesiredVelocityFromInput tanımlama

Biz MonoGame'nın kullanmaya başlayacağınız `TouchPanel` dokunmatik ekran geçerli durumu hakkında bilgi sağlayan sınıf. Denetleyecek bir yöntem ekleyelim `TouchPanel` ve bizim karakterin istenen hız döndürür:


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

`TouchPanel.GetState` Yöntemi döndürür bir `TouchCollection` kullanıcı ekran burada temas hakkında bilgiler içerir. Kullanıcının ekran temas değil, sonra `TouchCollection` ; bu durumda biz karakter taşıma döndürmemelidir boş olacaktır.

Kullanıcının ekran temas, biz ilk dokunma doğru karakter diğer bir deyişle taşınır, `TouchLocation` dizin 0 konumunda. İlk karakterin konumu ve ilk dokunma 's konumu arasındaki farkı eşit istenen hız yaparız:

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

Aşağıda, taşıma karakter aynı hızda tutar matematik biraz olur. Bu neden önemlidir açıklamasına yardımcı olmak için şimdi kullanıcı karakter bulunduğu çıktığınızda ekran 500 piksel burada temas bir durum düşünün. İlk satır nereye `desiredVelocity.X` olduğu atama 500 değeri. Ancak, kullanıcı yalnızca 100 karakter birimlerinden uzaklığını ekranında temas bundan sonra `desiredVelocity.X `100'e ayarlanır. Sonuç karakterin hareket hızı nasıl uzakta karakter dokunma noktasıdır yanıt emin olur. Her zaman aynı hızda taşımak için karakter istiyoruz olduğundan, biz desiredVelocity değiştirmeniz gerekir.

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` Deyimi denetimini hız durumda olmayan-sıfır – diğer bir deyişle, bu denetimi etkinleştirilmişse kullanıcı aynı nokta karakterin geçerli konumu olarak bitişik olmayan emin olmak için. Ardından dokunma uzakta nasıl bakılmaksızın sabit olması karakterin hızı ayarlamak ihtiyacımız olmadığını ise. Biz bunu 1 uzunluğu olan sonuçları hangi hız vektör normalleştirme tarafından gerçekleştirmek. Hız vektör karakter saniyede 1 piksel adresindeki taşınır 1 anlamına gelir. Biz bu 200 istenen hızını değeri çarparak hızlandırma.


### <a name="applying-velocity-to-position"></a>Hız konuma uygulama

Döndürülen hız `GetDesiredVelocityFromInput` karakterin uygulanmalıdır `X` ve `Y` değerleri çalışma zamanında hiçbir etkisi yoktur. Biz değiştireceksiniz `Update` yöntemini aşağıdaki şekilde:

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Ne burada uyguladık adlı *zamana dayalı* taşıma (tersine *tabanlı çerçeve* hareket). Zaman tabanlı taşıma hız değeri çarpar (depolanan değerlerden Bu örnekte `velocity` değişkeni) depolanan son güncelleştirmeden bu yana ne kadar zaman geçtikten tarafından `gameTime.ElapsedGameTime.TotalSeconds`. Oyun saniye başına daha az çerçeveler adresindeki çalıştırıyorsa, çerçeveler arasında geçen süre artar – son zaman tabanlı taşıma kullanarak nesneleri kare hızı bakılmaksızın aynı hızda her zaman taşınır sonucudur.

Biz bizim oyun şimdi çalıştırırsanız biz karakter dokunma konumun doğru taşıma olduğunu görürsünüz:

![](part2-images/image6.gif "Karakter dokunma konumun doğru taşıma")

## <a name="matching-movement-and-animation"></a>Taşıma ve animasyon eşleştirme

Taşıma ve tek bir animasyon yürütmeyi bizim karakter sahibiz sonra biz bizim animasyonları kalanı tanımlamak sonra karakter hareketini yansıtmak için bunları kullanın. Zaman tamamlanmış biz sekiz animasyonları toplam gerekir:

- Animasyon yukarı, aşağı, sol taramasını ve sağa için
- Animasyon durumu için hala ve aşağı yukarı, karşılıklı, sol ve sağ

### <a name="defining-the-rest-of-the-animations"></a>Animasyon kalan tanımlama

İlk ekleyeceğiz `Animation` için örnekler `CharacterEntity` bizim animasyonları burada eklediğimiz aynı yerde tamamı için sınıf `walkDown`. Biz bunu yaptıktan sonra `CharacterEntity` aşağıdaki olacaktır `Animation` üyeleri:

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

Animasyonları tanımlama olasılığınız yüksektir artık `CharacterEntity` şekilde Oluşturucusu:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Biz Yukarıdaki kod eklendi unutmamalısınız `CharacterEntity` Oluşturucusu bu kılavuzda daha kısa tutun. Oyunlar genellikle karakter animasyonları kendi sınıfları içine tanımını ayrı veya bu bilgileri bir veri biçimi XML veya JSON gibi yükleyin.

Ardından, biz karakter yalnızca durdurulmuşsa karakter Taşıma yönü göre ya da son animasyon göre animasyonları kullanmak için mantığı ayarlamak. Bunu yapmak için biz değiştireceksiniz `Update` yöntemi:


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

Animasyon geçiş yapmak için kod iki bloklara ayrılır. İlk hız eşit olup olmadığını denetler `Vector2.Zero` – bu bize karakter taşıma olup bildirir. Karakter taşıma sonra biz bakabilir `velocity.X` ve `velocity.Y` hangi yürüyen animasyonu yürütmek üzere belirlemek için değerleri.

Karakter olmayan taşıma sonra karakterin ayarlamak istiyoruz `currentAnimation` durumu animasyon için– ancak biz yalnızca varsa bunu `currentAnimation` bir taramasını olan animasyon veya animasyonun ayarlanmadı durumunda. Varsa `currentAnimation` biz değiştirmek gerek kalmaması karakter zaten duran sonra biri dört yürüyen animasyonları değil `currentAnimation`.

Bu kod karakter düzgün taramasını Dağıt ve onu durduğunda taramasını son yönü yüz emin sonucudur:

![](part2-images/image7.gif "Bu kodun sonucunda karakter düzgün taramasını Dağıt ve bunu durduğunda taramasını son yönü yüz olmasıdır")

## <a name="summary"></a>Özet

Bu kılavuz bir platformlar arası oluşturmak için MonoGame ile çalışmaya nasıl hareketli grafikler, taşıma nesneleri, giriş algılama ve animasyon oyun gösterilmiştir. Genel amaçlı animasyon sınıfı oluşturma kapsar. Kod mantığı düzenlemek için bir karakter varlığı nasıl oluşturulacağını gösterir.

## <a name="related-links"></a>İlgili bağlantılar

- [CharacterSheet görüntü kaynağı (örnek)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [Oyun tam (örnek) taramasını](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
