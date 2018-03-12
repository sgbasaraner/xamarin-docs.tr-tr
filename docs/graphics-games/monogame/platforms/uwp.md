---
title: "MonoGame UWP projesi oluşturma"
description: "MonoGame oyunları ve uygulamaları için evrensel Windows platformu, birden çok aygıt biriyle codebase hedefleme ve tek bir içerik kümesinin oluşturmak için kullanılabilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: d8f805d8a3fcadd9c2a6758f1dc5592c03fe3ed4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="creating-a-monogame-uwp-project"></a>MonoGame UWP projesi oluşturma

_MonoGame oyunları ve uygulamaları için evrensel Windows platformu, birden çok aygıt biriyle codebase hedefleme ve tek bir içerik kümesinin oluşturmak için kullanılabilir._

Bu kılavuzda MonoGame Evrensel Windows Platformu (UWP) proje oluşturma ve yükleme içeriği kapsar. UWP uygulamaları, masaüstü bilgisayarlar, tabletler, Windows telefonları ve Xbox One dahil olmak üzere tüm Windows 10 cihazlarda çalıştırabilirsiniz.

Bu kılavuzda görüntüleyen boş bir proje oluşturur bir *cornflower mavi* arka plan (XNA uygulamaları geleneksel arka plan rengini).


# <a name="requirements"></a>Gereksinimler

MonoGame UWP uygulama geliştirme gerektirir:

 - Windows 10 işletim sistemi
 - Visual Studio 2015 herhangi bir sürümü
 - Windows 10 geliştirici araçları
 - Geliştirici modu ayarı aygıta
- [Visual Studio için MonoGame 3.5](http://www.monogame.net/2016/03/17/monogame-3-5/) ya da daha yeni

Daha fazla bilgi için bkz [Windows 10 UWP geliştirme ayarlama sayfada](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-set-up).

Xbox One oyunlar perakende Xbox bir donanımda geliştirilebilir. Ek yazılım hem geliştirme PC ve Xbox One gereklidir. Bu sayfa bir Xbox bir oyun geliştirme için yapılandırma hakkında daha fazla bilgi için bakın [bir Xbox bir ayarı](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index).


# <a name="creating-an-empty-template"></a>Boş bir şablon oluşturma

Tüm gerekli kaynakları yüklü ve Windows 10 makinede Geliştirici modu etkinleştirildi sonra aşağıdaki adımları izleyerek Visual Studio kullanarak yeni bir MonoGame proje oluşturabilir:

1. Seçin **dosya** > **yeni** > **proje...**
1. Seçin **yüklü** > **şablonları** > **Visual C#** > **MonoGame** kategorisi: 

    ![](uwp-images/image1.png "MonoGame kategorisi")

1. Seçin **MonoGame Windows 10 Universal projesi** seçeneği: 

    ![](uwp-images/image2.png "MonoGame Windows 10 Universal projesi seçeneğini belirleyin")

1. Yeni proje için bir ad girin ve tıklayın **Tamam**.
Visual Studio, Tamam'ı tıklattıktan sonra herhangi bir hata görüntülerse, Windows 10 araçları yüklü olduğunu ve aygıtın geliştirici modunda olduğunu doğrulayın. 

Visual Studio şablonu oluşturduktan sonra biz çalıştıran boş proje görmek için çalıştırabilirsiniz:

![](uwp-images/image3.png "Visual Studio şablon oluşturma tamamlandığında, çalışan boş proje görmek için çalıştırın")

Köşeleri sayıları tanı bilgilerini sağlayın. Bu bilgiler kodda silerek kaldırılabilir `App.xaml.cs` içinde `DEBUG` engelleyin `OnLaunched` yöntemi:


```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.EnableFrameRateCounter = true;
    }
#endif
    ...
```

# <a name="running-on-xbox-one"></a>Xbox biri üzerinde çalışıyor

UWP projeleri aynı projeden tüm Windows 10 cihazına dağıtabilirsiniz. Windows 10 geliştirme makinesine ve Xbox bir yedekleme ayarladıktan sonra hedef uzak makineye geçiş ve Xbox One'nın IP adresi girerek UWP uygulamaları dağıtılabilir:

![](uwp-images/remote.png "UWP uygulamalar hedef uzak makineye geçiş ve Xbox olanları IP adresi girerek dağıtılabilir")

Bir Xbox beyaz kenarlık güvenli olmayan alan TV için temsil eder. Daha fazla bilgi için bkz: [güvenli alan bölüm](#Safe_Area_on_Xbox_One).

![](uwp-images/safearea.png "Bir Xbox beyaz kenarlık güvenli olmayan alanı TV için temsil eder")

## <a name="safe-area-on-xbox-one"></a>Xbox biri üzerinde güvenli alan

Oyun konsolları geliştirme alanıdır (örneğin, kullanıcı Arabirimi veya HUD) tüm kritik görselleri içermesi gereken ekranın Center'da güvenli alan dikkate gerektirir. Alan güvenli alan dışında bu alanda sunulan yerleştirilen görselleri bazı ekranlarda kısmen veya tamamen görünmez olabilir tüm televizyon üzerinde görünür olması garanti edilmez.

Xbox One MonoGame şablonu güvenli alan göz önünde bulundurur ve beyaz kenarlık olarak işler. Bu alan ayrıca çalışma zamanında oyunun yansıtılır `Window.ClientBounds` Gözcü penceresi Visual Studio'da bu görüntüsü gösterildiği gibi özelliği. İstemci sınırları yüksekliğini 1920 x 1080 ekran çözünürlüğünü rağmen 1016 olduğuna dikkat edin:

![](uwp-images/clientbounds.png "İstemci sınırları yüksekliğini 1920 x 1080 ekran çözünürlüğünü rağmen 1016 olduğuna dikkat edin")


# <a name="referencing-content-in-uwp-projects"></a>UWP projeleri içeriğinde başvurma

İçerik MonoGame projelerinde başvuruda bulunabilir doğrudan dosya veya aracılığıyla [MonoGame içerik ardışık düzen](~/graphics-games/cocossharp/content-pipeline/index.md). Küçük oyun projeleri dosyasından yüklenirken basitliği yararlı. Daha büyük projeler içerik ardışık düzen'ı kullanarak içerik boyutunu küçültmek ve kez yüklemek için en iyi duruma getirmek için yararlı olacaktır. Xbox 360 XNA aksine `System.IO.File` sınıftır Xbox bir UWP uygulamaları üzerinde kullanılabilir.

İçerik ardışık düzen kullanarak içerik yükleme ile ilgili daha fazla bilgi için bkz: [içeriği ardışık düzen Kılavuzu](~/graphics-games/cocossharp/content-pipeline/index.md). 


## <a name="loading-content-from-file"></a>İçerik dosyasından yükleniyor

İOS ve Android aksine, UWP projelerini göre yürütülebilir dosyaları başvuruda bulunabilir. Basit oyunlar bu tekniği yük içeriği gerek kalmadan içerik ardışık düzen projeyi oluşturmak ve değiştirmek için kullanabilirsiniz.

Yüklemek için bir `Texture2D` dosyasından:

1. UWP projesini içerik klasörüne bir .png dosyası ekleyin. İçerik klasörüne ekleme içerik XNA ve MonoGame bir kuraldır.
1. Yeni eklenen PNG üzerinde sağ tıklayın ve Özellikler'i seçin.
1. Değişiklik **çıktı dizinine Kopyala** için **yeniyse Kopyala**.
1. Yüklemek için oyunun Initialize yöntemine aşağıdaki kodu ekleyin bir `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

Kullanma hakkında daha fazla bilgi için bir `Texture2D`, bkz: [MonoGame Kılavuzu'na giriş](~/graphics-games/monogame/introduction/index.md).


# <a name="summary"></a>Özet

Bu kılavuz, yeni bir UWP projesi ve UWP özgü konuları dosyaları yüklerken oluşturma kapsar. Tam UWP oyunlar oluşturmakla ilgilenen geliştiriciler daha fazla bilgi edinebilirsiniz içinde MonoGame hakkında [MonoGame Kılavuzu giriş](~/graphics-games/monogame/introduction/index.md).