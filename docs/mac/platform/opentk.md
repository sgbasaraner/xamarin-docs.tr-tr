---
title: OpenTK giriş
description: Bu makalede Xamarin.Mac uygulama içinde OpenTK ile çalışmak için bir giriş sağlar. Oluşturma ve bir oyun penceresi bakımı, basit bir nesne oluşturma ve kullanıcıya nesne görüntüleme kapsar.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d5f19dac8dc362e1ac4a36cbe5cf5db3e31ae363
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-opentk"></a>OpenTK giriş

OpenTK (açık araç takımı), OpenGL ve OpenCL OpenAL ile çalışmayı kolaylaştırır bir Gelişmiş, alt düzey C# kitaplıktır. OpenTK kullanılabilir oyunlar, bilimsel uygulamaları veya diğer 3B grafik gerektiren projeleri, ses veya hesaplama işlevselliği için. Bu makalede OpenTK Xamarin.Mac kullanmayla kısa bir giriş sağlar.

[![](opentk-images/intro01.png "Bir örnek uygulamayı çalıştırma")](opentk-images/intro01.png#lightbox)

Bu makalede, biz OpenTK temelleri Xamarin.Mac uygulamasında ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>OpenTK hakkında

Yukarıda belirtildiği gibi OpenTK (açık araç takımı), OpenGL ve OpenCL OpenAL ile çalışmayı kolaylaştırır bir Gelişmiş, alt düzey C# kitaplıktır. OpenTK Xamarin.Mac kullanmayla aşağıdaki özellikleri sağlar:

- **Hızlı geliştirme** -OpenTK güçlü veri türleri ve satır içi belgelerine kodlama akışınızı geliştirmeye ve daha kolay ve daha sonrası hatalarını yakalama sağlar.
- **Kolay tümleştirme** -OpenTK kolayca .NET uygulamaları ile tümleştirmek için tasarlanmıştır.
- **Lisanslı** -OpenTK altında MIT/X11 lisansları dağıtılır ve tamamen ücretsizdir.
- **Zengin, tür kullanımı uyumlu bağlamaları** -OpenTK OpenGL, OpenGL en son sürümlerini destekler | ES, OpenAL ve OpenCL otomatik uzantı yükleme ile hata denetimi ve satır içi belgeleri.
- **Esnek GUI seçenekleri** -yüksek performanslı oyun penceresi, özellikle oyunlar ve Xamarin.Mac için tasarlanmış, OpenTK yerel sağlar.
- **Tam olarak yönetilen, CLS uyumlu kod** -OpenTK hiçbir yönetilmeyen kitaplıklarla macOS 32-bit ve 64 bit sürümlerini destekler.
- **3B matematik Araç Seti** OpenTK sağlayan `Vector`, `Matrix`, `Quaternion` ve `Bezier` yapılar, 3B matematik araç takımı aracılığıyla.

OpenTK kullanılabilir oyunlar, bilimsel uygulamaları veya diğer 3B grafik gerektiren projeleri, ses veya hesaplama işlevselliği için.

Daha fazla bilgi için lütfen bkz [açık Araç Seti](http://www.opentk.com) Web sitesi.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK hızlı başlangıç

OpenTK bir Xamarin.Mac uygulamasını kullanarak bir hızlı giriş, size bir oyun görünümünü açar, bu görünümünde ve attachs oyun görünüm basit bir üçgen üçgen kullanıcıya görüntülenecek Mac uygulamanın ana penceresi işler basit bir uygulama oluşturmak için adımıdır.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Yeni bir proje başlangıç

Mac için Visual Studio'yu açın ve yeni bir Xamarin.Mac çözümü oluşturun. Seçin **Mac** > **uygulama** > **genel** > **Cocoa uygulama**:

[![](opentk-images/sample01.png "Yeni bir Cocoa uygulama ekleme")](opentk-images/sample01.png#lightbox)

Girin `MacOpenTK` için **proje adı**:

[![](opentk-images/sample02.png "Proje adı ayarlama")](opentk-images/sample02.png#lightbox)

Tıklatın **oluşturma** yeni projeyi derlemek için düğmesi.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>OpenTK dahil olmak üzere

Xamarin.Mac uygulamada açık j kullanmadan önce OpenTK derlemesine başvuru eklemeniz gerekir. İçinde **Çözüm Gezgini**, sağ **başvuruları** klasörü ve seçin **başvuruları Düzenle...** .

Tarafından işaretleyin `OpenTK` tıklatıp **Tamam** düğmesi:

[![](opentk-images/sample03.png "Proje başvurularını düzenleme")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>Using OpenTK

Oluşturulan yeni proje ile çift `MainWindow.cs` dosyasını **Çözüm Gezgini** düzenlemek için açın. Olun `MainWindow` sınıfı görünüm aşağıdaki gibi:

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

Bu kod ayrıntılı aşağıdaki üzerinden edelim.

<a name="Required_APIs" />

### <a name="required-apis"></a>Gerekli API'leri

Birkaç başvuruyu OpenTK Xamarin.Mac sınıfında kullanması gerekir. Tanımı başlangıcında biz aşağıdaki dahil ettiğiniz `using` deyimleri:

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
Bu en az kümesi OpenTK kullanarak herhangi bir sınıf için gerekli olacaktır.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Oyun görünümü ekleme

Sonraki biz OpenTK bizim etkileşim içeren ve sonuçları görüntülemek için bir oyun görünüm oluşturmanız gerekir. Aşağıdaki kod kullandık:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Oyun görünüm bizim ana Mac penceresi aynı boyutta burada yaptık ve içerik görünüm penceresinin yeni yerini `MonoMacGameView`. Biz varolan penceresi içeriği değiştirildi çünkü ana Windows yeniden boyutlandırıldığında bizim vermiş Görünüm otomatik olarak boyutlandırılır.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Olaylara yanıt verme

Her oyun görünüm yanıt vermesi gereken birkaç varsayılan olay vardır. Bu bölümde, gerekli ana olayları ele alınacaktır.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Load olayı

`Load` Olay, sistemi, görüntüleri, doku ya da müzik diskten kaynakları yüklemek için yerdir. Bizim basit, test uygulaması için değil kullanıyoruz `Load` olay, ancak başvuru için dahil:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Resize olayı

`Resize` Oyun görünümü yeniden boyutlandırılabilir her zaman olay'nin çağrılabilir. Bizim örnek uygulamamız için GL görünüm penceresinin bizim oyun (otomatik Mac ana penceresi yeniden boyutlandırılabilir olması) görünümü aynı boyutta şu kod ile yapıyoruz:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>UpdateFrame olayı

`UpdateFrame` Olay kullanıcı girişini işlemek, nesne konumları, çalışma fizik veya AI hesaplamaları güncelleştirmek için kullanılır. Bizim basit, test uygulaması için değil kullanıyoruz `UpdateFrame` olay, ancak başvuru için dahil: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> OpenTK Xamarin.Mac uyarlamasını içermemesi `Input API`, klavye eklemek için API'ları ve fare desteği sağlanan Apple kullanmanız gerekir. İsteğe bağlı olarak, özel bir örneğini oluşturabilirsiniz `MonoMacGameView` ve geçersiz kılma `KeyDown` ve `KeyUp` yöntemleri.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>RenderFrame olayı

`RenderFrame` Olay içerir (çizim) işlemek için kullanılan kod, grafik. Bizim örnek uygulama için biz olan basit bir üçgen oyun görünüm doldurma: 

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

Genellikle işleme kod size çağrısıyla olma `GL.Clear` olan öğeleri kaldırmak için önce çizilmiş yeni öğeler.

> [!IMPORTANT]
> OpenTK Xamarin.Mac sürümü için **sağlamadığı** çağrısı `SwapBuffers` yöntemi, `MonoMacGameView` işleme kodunuzu sonunda örneği. Bunun yapılması, işlenen görünümü görüntüleme yerine hızlı bir şekilde flaş oyun görünümüne neden olur.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Oyun görünümü çalışmasını

Tüm gerekli olayları tanımlayın ve oyun görünüm uygulamamıza ana Mac penceresine eklenen, oyun görünüm çalıştırmak ve bizim grafik görüntülemek için okunduğu. Aşağıdaki kodu kullanın:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

İstenen kare hızı en güncelleştirmek için oyun görünümünde istediğimiz biz geçişi, Bizim örneğimizde, biz seçtiniz `60` kare / saniye (normal TV olarak aynı yenileme hızı).

Şimdi bizim uygulamayı çalıştırın ve çıktıyı görürsünüz:

[![](opentk-images/intro01.png "Uygulamaları çıktı örneği")](opentk-images/intro01.png#lightbox)

Biz bizim pencereyi yeniden boyutlandırmak, oyun görünümü de bulunur ve üçgen yeniden boyutlandırılabilir ve de gerçek zamanlı güncelleştirildi.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Sonraki nerede?

Bitti Xamarin.mac uygulamada OpenTk ile çalışmanın temelleri ile sonraki denemeye ne bazı öneriler şunlardır:

- Üçgen rengini ve oyun görünümünde arka plan rengini değiştirmeyi deneyin `Load` ve `RenderFrame` olaylar.
- Kullanıcının bastığınızda, bir anahtar rengini değiştirmek üçgen olun `UpdateFrame` ve `RenderFrame` olayları veya kendi özel `MonoMacGameView` sınıfı ve geçersiz kılma `KeyUp` ve `KeyDown` yöntemleri.
- Uyumlu anahtarlar kullanılarak ekran üzerinde hareket üçgen olun `UpdateFrame` olay. : İpucu `Matrix4.CreateTranslation` çeviri matris ve çağrı oluşturmak için yöntemi `GL.LoadMatrix` yöntemi olarak yüklemek için `RenderFrame` olay.
- Kullanım bir `for` Birkaç üçgenler işlemek için döngü `RenderFrame` olay.
- 3B uzaydaki üçgen farklı bir görünümünü sunmak için kamera döndürün. : İpucu `Matrix4.CreateTranslation` çeviri matris ve çağrı oluşturmak için yöntemi `GL.LoadMatrix` yüklemek için yöntem. Aynı zamanda `Vector2`, `Vector3`, `Vector4` ve `Matrix4` kamera işlemeleri için sınıflar.

Daha fazla örnek için lütfen bkz [OpenTK örnekleri GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) deposu. OpenTK kullanma örnekleri resmi bir listesini içerir. OpenTK Xamarin.Mac sürümü ile kullanmak için bu örnekler uyarlamanız gerekir.

Bir daha karmaşık Xamarin.Mac örneği OpenTK uygulaması için lütfen bkz bizim [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) örnek.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede Xamarin.Mac uygulamada OpenTK ile çalışan bir Hızlı Bakış sürdü. Bir oyun penceresi oluşturma gördüğümüz oyun penceresi Mac penceresine ekleme ve basit bir şekil oyun penceresinde işlemek.

## <a name="related-links"></a>İlgili bağlantılar

- [MacOpenTK (örnek)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (örnek)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows ile birlikte çalışma](~/mac/user-interface/window.md)
- [Açık Araç Seti](http://www.opentk.com)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
