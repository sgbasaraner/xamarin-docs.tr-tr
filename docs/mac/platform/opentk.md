---
title: Opentk'ya giriş Xamarin.Mac içinde
description: Bu makalede bir Xamarin.Mac uygulamasında OpenTK ile çalışmak için bir giriş sağlar. Oluşturma ve bir oyun penceresi bakımı, basit bir nesne oluşturma ve nesne kullanıcıya görüntüleme ele alınmaktadır.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 02cd32ac3016a3556cac8611ff839336f3089235
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780644"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Opentk'ya giriş Xamarin.Mac içinde

OpenTK (açık araç seti), OpenGL ve OpenCL OpenAL ile çalışmayı kolaylaştırır bir Gelişmiş, alt düzey C# kitaplıktır. OpenTK kullanılabilir oyunları, Bilimsel uygulamalar veya diğer 3B grafik gerektiren projeler, ses veya hesaplama işlevselliği için. Bu makalede, OpenTK bir Xamarin.Mac uygulamasını kullanarak kısa bir giriş sağlar.

[![](opentk-images/intro01.png "Bir örnek uygulama çalıştırma")](opentk-images/intro01.png#lightbox)

Bu makalede, biz bir Xamarin.Mac uygulamasını OpenTK temellerini ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>OpenTK hakkında

Yukarıda belirtildiği gibi OpenTK (açık araç seti), OpenGL ve OpenCL OpenAL ile çalışmayı kolaylaştırır bir Gelişmiş, alt düzey C# kitaplıktır. OpenTK bir Xamarin.Mac uygulamasını kullanarak aşağıdaki özellikleri sağlar:

- **Hızlı geliştirme** -OpenTK kodlama iş akışınızı artırmak ve hataları daha kolay ve daha çabuk yakalamak için güçlü veri türleri ve satır içi belgeler sağlar.
- **Kolay tümleştirme** -OpenTK kolayca .NET uygulamalarıyla tümleştirmek için tasarlanmıştır.
- **İhtiyari lisansı** -OpenTK MIT/X11 lisans altında dağıtılan ve tamamen ücretsizdir.
- **Zengin, tür kullanımı uyumlu bağlamaları** -OpenTK OpenGL OpenGL en son sürümlerini destekler | ES, OpenAL ve OpenCL otomatik uzantı yükleme ile hata denetleme ve satır içi belgeler.
- **Esnek GUI seçenekleri** -yüksek performanslı oyun penceresi özellikle oyunlar ve Xamarin.Mac için tasarlanmış, OpenTK yerel sağlar.
- **Tam olarak yönetilen, CLS uyumlu kod** -OpenTK herhangi bir yönetilmeyen kitaplığı ile macOS 32-bit ve 64 bit sürümlerini destekler.
- **3B matematik Araç Seti** OpenTK sağlayan `Vector`, `Matrix`, `Quaternion` ve `Bezier` yapılar, 3B matematik araç seti ile.

OpenTK kullanılabilir oyunları, Bilimsel uygulamalar veya diğer 3B grafik gerektiren projeler, ses veya hesaplama işlevselliği için.

Daha fazla bilgi için lütfen bkz [açık Araç Seti](http://www.opentk.com) Web sitesi.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK hızlı başlangıç

Bir Xamarin.Mac uygulamasını OpenTK kullanarak hızlı bir giriş, bir oyun görünümü açar, bu üçgen kullanıcıya göstermek için Mac uygulamanın ana penceresi için basit bir üçgen bu görünümünde ve attachs Game görünümü işleyen bir uygulama oluşturmak için kullanacağız.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Yeni bir projeye Başlarken

Mac için Visual Studio'yu başlatın ve yeni bir Xamarin.Mac çözümü oluşturun. Seçin **Mac** > **uygulama** > **genel** > **Cocoa uygulaması**:

[![](opentk-images/sample01.png "Yeni bir Cocoa uygulaması ekleme")](opentk-images/sample01.png#lightbox)

Girin `MacOpenTK` için **proje adı**:

[![](opentk-images/sample02.png "Proje adı ayarlama")](opentk-images/sample02.png#lightbox)

Tıklayın **Oluştur** yeni projeyi oluşturmak için düğme.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>OpenTK dahil

Bir Xamarin.Mac uygulamasında açık j kullanmadan önce OpenTK derlemesine bir başvuru eklemeniz gerekir. İçinde **Çözüm Gezgini**, sağ **başvuruları** klasörü ve select **başvuruları Düzenle...** .

Tarafından işaretleyin `OpenTK` tıklatıp **Tamam** düğmesi:

[![](opentk-images/sample03.png "Proje başvurularını düzenleme")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>OpenTK kullanma

Oluşturulan yeni proje ile çift `MainWindow.cs` dosyası **Çözüm Gezgini** düzenlemek üzere açın. Olun `MainWindow` sınıfı görünüm aşağıdaki gibi:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

Şimdi bu kodu aşağıda ayrıntılı olarak inceleyeceğiz.

<a name="Required_APIs" />

### <a name="required-apis"></a>Gerekli API

Birkaç başvuruları, OpenTK Xamarin.Mac sınıfında kullanmak için gereklidir. Şunları ekledik tanımının başında `using` ifadeleri:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```
Bu küçük grup OpenTK kullanarak herhangi bir sınıf için gerekli olacaktır.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Oyun görünüm ekleme

Ardından biz OpenTK bizim etkileşime içerir ve sonuçları görüntülemek için bir oyun görünüm oluşturmanız gerekir. Aşağıdaki kod kullandık:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Oyun görünümü bizim ana penceresinde aynı boyutta burada yaptık ve yeni içerik görünümü penceresinin yerini `MonoMacGameView`. Biz varolan pencere içeriğinin değiştirilmiş olduğundan, Main Windows yeniden boyutlandırıldığında bizim verdiğiniz görünümü otomatik olarak boyutlandırılır.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Olaylarına yanıt verme

Her oyunun görünümü yanıt vermesi gereken birkaç varsayılan olaylar vardır. Bu bölümde, gereken ana olayları ele alınacaktır.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Yükleme olayı

`Load` Olay, sistemi, disk görüntüleri, doku ya da müzik gibi kaynakları bileşiminin yükleneceği yerdir. Basit, test uygulaması için değil kullanıyoruz `Load` olay, ancak başvuru için dahil:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Yeniden Boyutlandır olayını

`Resize` Game görünümü yeniden boyutlandırıldı her zaman olay'nin çağrılabilir. Örnek uygulamamız için GL görünüm penceresinin (Bu Mac ana pencere tarafından yeniden boyutlandırılmış otomatik olarak) bizim oyun görünüm olarak aynı boyutta aşağıdaki kodla yapıyoruz:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>UpdateFrame olay

`UpdateFrame` Olay kullanıcı girişlerini işler, nesne konumları, çalışma fizik veya AI hesaplamalar güncelleştirmek için kullanılır. Basit, test uygulaması için değil kullanıyoruz `UpdateFrame` olay, ancak başvuru için dahil: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Xamarin.Mac uygulamasını OpenTK içermemesi `Input API`, klavye eklemek için API'ler ve fare desteği sağlanan Apple kullanmanız gerekecektir. İsteğe bağlı olarak özel bir örneğini oluşturabilirsiniz `MonoMacGameView` ve geçersiz kılma `KeyDown` ve `KeyUp` yöntemleri.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>RenderFrame olay

`RenderFrame` Olay içeriyor (çizim) işlemek için kullanılan kod, bir grafik. Örnek uygulamamız için biz oyun görünümü ile basit bir üçgen dolduruyor: 

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

Genellikle işleme kod size çağrısıyla durdurulmasını `GL.Clear` var olan öğeleri kaldırmak için önce çizilmiş yeni öğeler.

> [!IMPORTANT]
> OpenTK Xamarin.Mac sürümü için **olmayan** çağrı `SwapBuffers` yöntemi, `MonoMacGameView` işleme kodunuzu sonundaki örneği. Bunun yapılması, işlenmiş görünümde görüntüleniyor yerine hızlı bir şekilde strobe oyun görünüme neden olur.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Oyun görünümü çalışmasını

Tüm gerekli olayları tanımlayın ve oyun görünümü uygulamamızı ana Mac penceresine bağlı, oyun görünümü çalıştırmak ve bizim grafikleri görüntülemek için okunduğu. Aşağıdaki kodu kullanın:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Biz istenen kare hızı oyun görünüm güncelleştirmeyi istediğimiz geçmek için Bizim örneğimizde seçtik `60` kare / saniye (normal TV olarak aynı yenileme hızı).

Şimdi uygulamamıza çalıştırın ve çıktıyı görürsünüz:

[![](opentk-images/intro01.png "Uygulamaları çıktının bir örneği")](opentk-images/intro01.png#lightbox)

Size sunduğumuz penceresini yeniden boyutlandırırsanız, oyun görünümü bulunur ve bu üçgen yeniden boyutlandırıldı ve gerçek zamanlı güncelleştirme de aynı zamanda olacaktır.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Sonrakine Git nerede?

Bitti Xamarin.mac uygulamada OpenTk ile çalışmanın temelleri ile sonraki denemeye ne bazı öneriler şunlardır:

- Bu üçgen rengini ve arka plan rengi oyun görünümünde değiştirmeyi deneyin `Load` ve `RenderFrame` olayları.
- Kullanıcı bir anahtar olarak bastığınızda rengini değiştirmek üçgen olun `UpdateFrame` ve `RenderFrame` olayları veya kendi özel `MonoMacGameView` sınıf ve geçersiz kılma `KeyUp` ve `KeyDown` yöntemleri.
- Üçgen kullanan anahtarları kullanarak ekranı arasında geçiş yapmak `UpdateFrame` olay. : İpucu `Matrix4.CreateTranslation` çeviri matris ve çağrı yöntemini `GL.LoadMatrix` yöntemi içinde yüklenecek `RenderFrame` olay.
- Kullanım bir `for` Birkaç üçgenler işlenecek döngü `RenderFrame` olay.
- Bu üçgen 3B alanda farklı bir görünümünü sağlamak için kamerayı döndür. : İpucu `Matrix4.CreateTranslation` çeviri matris ve çağrı yöntemini `GL.LoadMatrix` yüklemek için yöntemi. Ayrıca `Vector2`, `Vector3`, `Vector4` ve `Matrix4` kamera işlemeleri sınıfları.

Daha fazla örnek için bkz. Lütfen [OpenTK örnekleri GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) depo. Bu, OpenTK kullanma örnekleri resmi bir listesini içerir. Bu örnekler, OpenTK Xamarin.Mac sürümüyle kullanmak için uyum zorunda kalırsınız.

Lütfen için daha karmaşık bir Xamarin.Mac örnek, OpenTK uygulaması, bakın bizim [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) örnek.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede bir Xamarin.Mac uygulamasında OpenTK ile çalışma Hızlı Bakış duruma getirdi. Bir oyun penceresi oluşturma gördüğümüz nasıl oyun penceresi için bir Mac pencere ekleme ve oyun penceresinde basit şekil işlemek nasıl.

## <a name="related-links"></a>İlgili bağlantılar

- [MacOpenTK (örnek)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (örnek)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows ile çalışma](~/mac/user-interface/window.md)
- [Açık Araç Seti](http://www.opentk.com)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
