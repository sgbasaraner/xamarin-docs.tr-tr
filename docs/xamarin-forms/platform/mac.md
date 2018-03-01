---
title: Mac Platform Setup
description: "Xamarin.Forms artık Mac platform Önizleme desteği vardır"
ms.topic: article
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: bda207796d1019f8188176acce055d782cb9e32d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="mac-platform-setup"></a>Mac Platform Setup

![Önizleme](~/media/shared/preview.png)

Başlamadan önce oluşturun (veya var olan kullanın) Xamarin.Forms projesi.
Yalnızca Mac için Visual Studio kullanarak Mac uygulamaları ekleyebilirsiniz.

## <a name="adding-a-mac-app"></a>Mac uygulama ekleme

MacOS üzerinde Sierra ve Mac OS X El Capitan çalışacak bir Mac uygulaması eklemek için aşağıdaki yönergeleri izleyin:

1. Mac için Visual Studio, varolan Xamarin.Forms çözüm üzerinde sağ tıklatın ve seçin **Ekle > Yeni Proje Ekle...**

2. İçinde **yeni proje** penceresi seçin **Mac > Uygulama > Cocoa uygulama** ve basın **sonraki**.

3. Tür bir **App Name** (ve isteğe bağlı olarak yerleştirme öğesi için farklı bir ad seçin), tuşuna basarak **sonraki**.

4. Tuşuna basın ve yapılandırmasını gözden **oluşturma**. Bu adımları aşağıda gösterilmektedir:

  ![Cocoa uygulama eklemek nasıl gösteren animasyonlu yönergeleri](mac-images/add-macos-proj.gif)

5. Mac projeye sağ tıklayın **paketleri > paketleri Ekle...**  eklemek için [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet. Ayrıca diğer projeleri bu sürüme güncelleştirmeniz gerekir.

6. Mac projeye sağ tıklayın **başvuruları** ve (Paylaşılan proje veya PCL) Xamarin.Forms projesine bir başvuru ekleyin.

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

8. Güncelleştirme `AppDelegate` Xamarin.Forms başlatmak için bir pencere oluşturma ve Xamarin.Forms uygulaması yükleme (uygun bir ayarlamak hatırlamak `Title`). _Başlatılması için gereken başka bir bağımlılık varsa, burada da geçerli._

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

9. Çift **Main.storyboard** Xcode'da düzenlemek için. Seçin **penceresi** ve _işaretini_ **ilk denetleyicisinin** (Bu, yukarıdaki kod bir pencere oluşturduğundan) onay kutusu:

  [ ![Xcode'da ilk denetleyicisinin onay kutusunun işaretini kaldırın](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png)

  İstenmeyen öğeleri kaldırmak için film şeridi menü sisteminde düzenleyebilirsiniz.

10. Son olarak, tüm yerel kaynakları (ör. Ekle görüntü dosyaları) gerekli varolan platform projelerden.

11. Mac proje Xamarin.Forms kodunuzu macOS üzerinde çalışmalıdır!

## <a name="next-steps"></a>Sonraki Adımlar

### <a name="styling"></a>Stil oluşturma

Yapılan son değişikliklerle `OnPlatform` platformları herhangi bir sayıda şimdi hedefleyebilirsiniz. MacOS dahildir.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Da çift şöyle platformlarda dikkat edin: `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Pencere boyutunu ve konumunu

İlk boyutunu ve pencere konumunu ayarlayın `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Bilinen Sorunlar

Her şeyin üretim hazır olduğunu beklemelisiniz şekilde bu bir önizleme sürümü. Aşağıda projelerinize macOS ekledikçe karşılaşabileceğiniz bazı noktalar şunlardır:

### <a name="not-all-nugets-are-ready-for-macos"></a>Tüm NuGets macOS için hazır

Paketleri macOS projede çalışmak üzere "xamarinmac20" hedeflemesi gerekir. Kullandığınız kitaplıkları bazıları henüz macOS desteklemediğini bulabilirsiniz.

Bu durumda, bir istek göndermesini eklemek için projenin Bakımcı gerekir. Destek sahip oldukları kadar alternatifleri için aramanız gerekebilir.

### <a name="missing-xamarinforms-features"></a>Xamarin.Forms özellikler eksik

Xamarin.Forms özelliklerinin tamamı bu Önizleme'de tam; Bazı henüz uygulanmadı işlevlerini listesi aşağıdadır:

* Altbilgi
* Görüntü – boyut
* ListView – ScrollTo, UnevenRows desteği, SeparatorVisibility SeparatorColor, yenileme
* MasterDetailPage – BackgroundColor
* Gezinti – InsertPageBefore
* OpenGLRenderer
* Seçici – Bindable/Observable uygulama
* TabbedPage – BarBackgroundColor, BarTextColor
* Tablo görünümü – UnevenRows
* ViewCell – IsEnabled, ForceUpdateSize
* Web görünümü – çoğu WebNavigationEvents


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.Mac](~/mac/index.yml)
