---
title: Mac Platform Kurulumu
description: Bu makalede, macOS Sierra ve Macos'ta El Capitan çalıştırabilen bir uygulama oluşturacak bir Xamarin.Forms projesi Mac proje ekleme açıklanmaktadır.
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: ae0fbfc7862a0d2147b2c3bbdbae7dd53dfce78f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831695"
---
# <a name="mac-platform-setup"></a>Mac Platform Kurulumu

![Önizleme](~/media/shared/preview.png)

Başlamadan önce oluşturun (veya mevcut bir kullanın) Xamarin.Forms projesi.
Mac için Visual Studio kullanarak Mac uygulamaları yalnızca ekleyebilirsiniz

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Bir macOS proje için Xamarin.Forms, ekleyerek [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Bir Mac uygulaması ekleme

MacOS Sierra macOS El Capitan üzerinde çalışacak bir Mac uygulaması eklemek için aşağıdaki yönergeleri izleyin:

1. Mac için Visual Studio, mevcut Xamarin.Forms çözümü sağ tıklatın ve seçin **Ekle > Yeni Proje Ekle...**

2. İçinde **yeni proje** pencere **Mac > Uygulama > Cocoa uygulaması** basın **sonraki**.

3. Türü bir **uygulama adı** (ve isteğe bağlı olarak Dock öğesi için farklı bir ad seçin) tuşuna **sonraki**.

4. Tuşuna basın ve yapılandırmayı gözden **Oluştur**. Bu adımlar aşağıda gösterilir:

  ![Animasyonlu yönergeleri gösteren bir Cocoa uygulaması ekleme](mac-images/add-macos-proj.gif)

5. Mac projesinde sağ **paketleri > paketleri Ekle...**  eklemek için [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet. Ayrıca diğer projeleri bu sürüme güncelleştirmeniz gerekir.

6. Mac projesinde sağ **başvuruları** (Paylaşılan proje veya .NET Standard kitaplığı projesi) Xamarin.Forms projeye bir başvuru ekleyin.

  ![Xamarin.Forms paylaşılan kod projesine bir başvuru ekleyin](mac-images/references-sml.png)

7. Güncelleştirme **Main.cs** başlatmak için `AppDelegate`:

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. Güncelleştirme `AppDelegate` Xamarin.Forms başlatmak için bir pencere oluşturmak ve Xamarin.Forms uygulama yükleme (uygun bir ayarlanacak hatırlamak `Title`). _Başlatılması için gereken diğer bağımlılıkları varsa, burada da bunu._

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. Çift **Main.storyboard** Xcode'da düzenlemek için. Seçin **penceresi** ve _işaretini kaldırın_ **ilk denetleyicisinin** onay kutusunu (Bu, yukarıdaki kod, bir pencere oluşturur çünkü):

  [![Xcode'da ilk denetleyicisinin onay kutusunun işaretini kaldırın](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  İstenmeyen öğeleri kaldırmak için görsel taslak menü sisteminde düzenleyebilirsiniz.

10. Son olarak, yerel kaynakları (örn. ekleyin görüntü dosyaları) gerekli olan mevcut platform projelerindeki.

11. Mac projesi Macos'ta Xamarin.Forms kodunuzu Şimdi Çalıştır!

## <a name="next-steps"></a>Sonraki Adımlar

### <a name="styling"></a>Stil oluşturma

Yapılan son değişikliklerle `OnPlatform` artık herhangi bir sayıda platformları hedefleyebilir. MacOS dahildir.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Unutmayın, ayrıca çift bu gibi platformlarda: `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Pencere boyutu ve konumu

Pencerede konumunu ve ilk boyutu ayarlayabileceğiniz `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Bilinen Sorunlar

Her şey üretime hazır olduğunu beklemelisiniz. Bu bir önizleme olduğundan. Aşağıda projelerinize macOS ekledikçe, karşılaşabileceğiniz bazı noktalar şunlardır:

### <a name="not-all-nugets-are-ready-for-macos"></a>Tüm Nuget'i macOS için hazır

Paketleri "xamarinmac20" bir macOS projedeki iş hedeflemesi gerekir. Kullandığınız kitaplıklar bazıları henüz macOS desteklemediğini bulabilirsiniz.

Bu durumda, eklemek için projenin Bakımcı bir istek göndermek gerekir. Destek sahip oldukları kadar Alternatiflere bakın gerekebilir.

### <a name="missing-xamarinforms-features"></a>Xamarin.Forms özellikleri eksik

Tüm Xamarin.Forms özellikler, bu Önizleme sürümünde getirildiğinden; henüz uygulanmadı işlevlerinden bazıları listesi aşağıda verilmiştir:

* Alt bilgi
* Görüntü – boyut
* ListView – ScrollTo, UnevenRows desteği, SeparatorVisibility SeparatorColor, yenileme
* MasterDetailPage – BackgroundColor
* Gezinti – InsertPageBefore
* OpenGLRenderer
* Seçici – Bindable/Observable uygulama
* TabbedPage – BarBackgroundColor, BarTextColor
* Tablo görünümü – UnevenRows
* Viewcell'i – IsEnabled, ForceUpdateSize
* WebView – çoğu WebNavigationEvents


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Mac](~/mac/index.yml)
