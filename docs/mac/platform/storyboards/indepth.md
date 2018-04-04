---
title: Film şeritleri ile çalışma
description: MacOS Xcode kullanarak film şeritleri ile kullanıcı arabirimleri oluşturma.
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3b72affd9b101b0a139301fec9f2bed343310507
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="storyboards"></a>Görsel Taslaklar

Film şeridi tüm görünüm denetleyicilerinin işlevsel olarak genel bakış ayrıntılarıyla belirli bir uygulamanın kullanıcı Arabirimi tanımlar. Xcode'nın arabirimi Oluşturucu'da, bu denetleyicilerinden her birinin kendi Sahne yaşar.

[![](indepth-images/intro01.png "Film şeridi Xcode'nın arabirimi Oluşturucu")](indepth-images/intro01.png#lightbox)

Film şeridi kaynak dosyasıdır (uzantılı `.storyboard`), dahil Xamarin.Mac uygulamanın pakette derlenmiş ve sevk olduğunda. Uygulamanız için başlangıç film şeridi tanımlamak için düzenlemek 's `Info.plist` dosya ve seçin **ana arabirimi** açılır kutusundan: 

[![](indepth-images/sb01.png "Info.plist Düzenleyicisi")](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>Koddan yükleniyor

Belirli bir film şeridi koddan yüklemek ve bir görünüm denetleyicisi el ile oluşturmak için gerektiğinde zamanlar olabilir. Bu eylemi gerçekleştirmek için aşağıdaki kodu kullanabilirsiniz:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

`FromName` Uygulamanın pakete eklenen verilen ada sahip film şeridi dosya yükler. `InstantiateControllerWithIdentifier` Verilen kimlikle View Controller örneğini oluşturur. Kullanıcı arabirimini tasarlarken kimliğini Xcode'nın arabirimi Oluşturucusu'nda ayarlayın:

[![](indepth-images/sb02.png "Film şeridi kimliği ayarlama")](indepth-images/sb02.png#lightbox)

İsteğe bağlı olarak kullanabileceğiniz `InstantiateInitialController` yöntemi arabirimi Oluşturucu ilk denetleyici atanan görünüm denetleyicisini yüklemek için:

[![](indepth-images/sb03.png "İlk denetleyicisini ayarlama")](indepth-images/sb03.png#lightbox)

Tarafından işaretlenen **film şeridi giriş noktası** ve yukarıdaki sona erdi açık oku.

<a name="View-Controllers" />

## <a name="view-controllers"></a>Görünüm denetleyicileri

Görünüm denetleyicileri bilgilerinin bir Mac uygulama içinde belirli bir görünümünü ve bu bilgileri sağlayan veri modeli arasındaki ilişkileri tanımlayın. Film şeridi her üst düzey Sahne Xamarin.Mac uygulamanın kodda bir görünüm denetleyicisini temsil eder.

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>Görünüm denetleyicisini yaşam döngüsü

Birkaç yeni yöntemler eklenmiştir `NSViewController` içinde macOS film şeritleri desteklemek için sınıf. En önemlisi izleme yöntemleri belirli görünüm denetleyici tarafından denetlenen görünüm yaşam döngüsü yanıtlamak için kullanılır:

- `ViewDidLoad` -Görünüm film şeridi dosyasından yüklediğinde bu yöntem çağrılır.
- `ViewWillAppear` -Görünümü ekranında yalnızca görüntülenmeden önce bu yöntem çağrılır.
- `ViewDidAppear` -Bu yöntem, doğrudan Görünümü ekranında görüntülenen sonra çağrılır.
- `ViewWillDisappear` -Görünüm ekranından yalnızca kaldırılmadan önce bu yöntem çağrılır.
- `ViewDidDisappear` -Bu yöntem, doğrudan görünümü ekranından kaldırıldıktan sonra çağrılır.
- `UpdateViewConstraints` -Bir görünümü tanımlama kısıtlamaları güncelleştirilmesi düzeni konum ve boyut gerekiyor otomatik olduğunda bu yöntem çağrılır.
- `ViewWillLayout` -Bu görünüm subviews ekranda yalnızca düzenlenmiştir önce bu yöntem çağrılır.
- `ViewDidLayout` -Bu yöntem doğrudan ekranda görünümünü subviews açıklanmıştır sonra çağrılır.

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>Yanıtlayıcı zinciri

Ayrıca, `NSViewControllers` Şimdi pencerenin parçası olan _Yanıtlayıcı zinciri_:

[![](indepth-images/vc01.png "Yanıtlayıcı zinciri")](indepth-images/vc01.png#lightbox)

Ve bu nedenle bunlar kablolu almak ve Kes, Kopyala ve Yapıştır menü öğesi seçimlerini olaylarına yanıt vermesi için yukarı. Bu otomatik View Controller kablo yukarı yalnızca macOS Sierra (10,12) üzerinde çalışan uygulamalar oluşur ve büyük.

<a name="Containment" />

### <a name="containment"></a>Kapsama

Film şeritleri içinde görünüm denetleyicileri (örneğin, bölme görünüm denetleyicisini ve sekmesini View Controller) şimdi uygulayabilirsiniz _kapsama_, "diğer alt görünüm denetleyicileri içerebilirler sağlayacak şekilde":

[![](indepth-images/vc02.png "Görünüm denetleyicisini kapsama örneği")](indepth-images/vc02.png#lightbox)

Kendi üst görünümü denetleyici ve görüntüleme ve görünümlere ekranından kaldırma çalışmak için bunları bağlamanın özellikleri arka ve alt görünümü denetleyicileri yöntemleri içerir.

MacOS oluşturulmuş tüm kapsayıcı görünümü denetleyicileri Apple öneren kendi özel kapsayıcı görünümü denetleyicileri oluşturuyorsanız izleyin belirli bir düzen vardır:

[![](indepth-images/vc03.png "Görünüm denetleyicisini düzeni")](indepth-images/vc03.png#lightbox)

Koleksiyon görünümü denetleyicisi koleksiyon görünümü öğeleri, her biri bir veya daha fazla görünüm kendi görünümleri içeren denetleyicileri içeren bir dizi içerir.

<a name="Segues" />

## <a name="segues"></a>Segues

Segues tüm uygulamanızın UI tanımla planda arasındaki ilişkileri sağlayın. Film şeritleri içinde iOS birlikte çalışarak hakkında bilginiz varsa, bilmeniz iOS genellikle tam ekran görünümler arasında geçişler tanımlamak için Segues. Segues genellikle tanımlarken bu macOS farklıdır "[kapsama](#Containment)", bir Sahne üst Sahne alt olduğu.

MacOS içinde çoğu uygulamanın kendi görünümler birlikte bölünmüş görünümler ve sekmeler gibi kullanıcı Arabirimi öğeleri kullanarak aynı pencere içinde grup eğilimindedir. Burada görünümleri açma ve kapatma ekran geçmiş olması gerekir, iOS, sınırlı fiziksel nedeniyle alanı görüntüler.

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>Sunu Segues

MacOS'ın tendencies kapsama doğrultusunda verildiğinde, bazı durumlarda nerede _sunu Segues_ kalıcı Windows, sayfa görünümleri ve Popovers gibi kullanılır. macOS aşağıdaki yerleşik ü sağlar türleri:

- **Göster** -Segue hedef kalıcı olmayan pencere olarak görüntüler. Örneğin, bir belge penceresi, uygulamanızda başka bir örneği sunmak için Segue bu türü kullanın.
- **Kalıcı** -Segue kalıcı bir pencere olarak hedef gösterir. Örneğin, uygulamanız için Tercihler penceresi sunmak için Segue bu türü kullanın.
- **Sayfa** -Segue hedefinin bir sayfa üst penceresine bağlı olarak gösterir. Örneğin, bu tür ü Bul ve Değiştir sayfası sunmak için kullanın.
- **Popover** -Segue hedefi olduğu gibi bir popover penceresi gösterir. Örneğin, bir kullanıcı Arabirimi öğesi kullanıcı tarafından tıklatıldığında seçenekleri sunmak için bu Segue türünü kullanın.
- **Özel** -özel ü geliştirici tarafından tanımlanan bir tür kullanarak Segue hedef gösterir. Bkz: [oluşturma özel Segues](#Creating-Custom-Segues) daha fazla ayrıntı için bölüm aşağıda.

Sunu Segues kullanırken, geçersiz kılabilirsiniz `PrepareForSegue` View Controller üst yöntemi başlatmak için sunu ve değişkenleri ve herhangi bir veri görünümü sunulmasını denetleyiciye sağlayın.

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>Tetiklenen Segues

Tetiklenen Segues adlandırılmış Segues belirtmenize olanak tanır (aracılığıyla kendi **tanımlayıcısı** arabirimi Oluşturucu özelliğinde) ve bunları çağırarak veya kullanıcının bir düğmeye tıklanması gibi olaylar tarafından tetiklenen `PerformSegue` kodda yöntemi:

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

Uygulamanın kullanıcı arabirimini yerleştirirken ü kimliği Xcode'nın arabirimi oluşturucu içinde tanımlanır:

[![](indepth-images/sg02.png "Girerek bir adı ü")](indepth-images/sg02.png#lightbox)

Segue kaynağı olarak hareket View Controller geçersiz kılmalısınız `PrepareForSegue` yöntemi ve sıfırlamaları gerekli Segue yürütülmeden önce yapın ve belirtilen görünüm denetleyici görüntülenir:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

İsteğe bağlı olarak, kılabilirsiniz `ShouldPerfromSegue` yöntemi ve desteklemediğini Segue gerçekten yürütüldüğünde C# kodu aracılığıyla denetimi. El ile sunulan görünüm denetleyicileri için çağrı kendi `DismissController` yöntemi artık gerektiğinde bunları görüntüden kaldırın.

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Özel oluşturma Segues

Ne zaman macOS içinde tanımlanan yapı Segues tarafından sağlanmayan bir Segue türü, uygulamanızın gerektirdiği zamanlar olabilir. Bu durumda, bir özel atanabilen ü Xcode'nın arabirimi Oluşturucusu'nda, uygulamanızın kullanıcı arabirimini yerleştirirken oluşturabilirsiniz.

Örneğin, geçerli View Controller (Sahne hedef yeni bir pencerede açmak) yerine bir pencere içinde yerini alan yeni bir Segue türü oluşturmak için size aşağıdaki kodu kullanabilirsiniz:

```csharp
using System;
using AppKit;
using Foundation;

namespace OnCardMac
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Swap the controllers
            source.View.Window.ContentViewController = destination;

            // Release memory
            source.RemoveFromParentViewController ();
        }
        #endregion

    }
        
}
```

Burada dikkat edilecek noktalar birkaç:

- Kullanıyoruz `Register` Objective-C/macOS bu sınıfa kullanıma sunmak için öznitelik.
- Biz geçersiz kılma `Perform` gerçekten bizim Özel Segue eylemi gerçekleştirmek için yöntem.
- Biz pencerenin değiştirme `ContentViewController` Segue target (hedef) tarafından tanımlanan bir denetleyici.
- Bellek kullanılarak boşaltmak için özgün View Controller kaldırmakta olduğunu `RemoveFromParentViewController` yöntemi.

Xcode'nın arabirimi Oluşturucu'da bu yeni Segue türünü kullanmak için uygulamayı derleyin sonra geçiş için Xcode ve yeni Segue iki Sahne arasında eklemek ihtiyacımız. Ayarlama **stili** için **özel** ve **ü sınıfı** için `ReplaceViewSegue` (bizim Özel Segue sınıf adı):

[![](indepth-images/sg01.png "Ayar Segue sınıfı")](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>Pencere denetleyicileri

Pencere denetleyicileri içeren ve macOS uygulamanızı oluşturabilirsiniz farklı penceresi türlerini denetleyebilirsiniz. Film şeritleri için bunlar aşağıdaki özelliklere sahiptir:

1. Bir içerik görünümü denetleyicisi sağlamaları gerekir. Bu içerik görünüm penceresinin alt sahip aynısı olacaktır.
2. `Storyboard` Özelliği penceresi denetleyicisi, başka yüklendi film şeridi içerecek `null` film şeridi görünümünden yüklü değil.
3. Çağırabilirsiniz `DismissController` verilen penceresini kapatın ve görünümden kaldırmak için yöntem.

Görünüm denetleyicileri gibi penceresi denetleyicileri uygulamak `PerformSegue`, `PrepareForSegue` ve `ShouldPerfromSegue` yöntemleri ve Segue işlem kaynağı olarak kullanılabilir.

Pencere denetleyicisi macOS uygulama aşağıdaki özellikler için sorumlu şunlardır:

- Belirli bir sayfa yönettikleri.
- Bunlar pencerenin başlık çubuğu ve araç çubuğu (varsa) yönetin.
- Bunlar penceresinin içeriğini görüntülemek için içerik görünümü denetleyicisi yönetin.

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>Hareketi tanıyıcıları

MacOS için hareketi tanıyıcıları dekiler iOS içinde neredeyse aynıdır ve uygulamanızın kullanıcı Arabirimi öğelerine Geliştirici hareketleri (örneğin, bir fare düğmesini tıklatarak) kolayca eklemenize olanak sağlar.

Ancak, burada iOS hareketleri (örneğin, iki parmakları ekran dokunarak), uygulamanın tasarım en macOS hareketleri donanım tarafından belirlenir tarafından belirlenir.

Hareketi tanıyıcıları kullanarak özel etkileşimler kullanıcı arabiriminde bir öğe eklemek için gerekli kod miktarını önemli ölçüde azaltabilir. Tek ve çift tıklamaları otomatik olarak belirlemek gibi tıklatın ve olaylar, vb. sürükleyin.

Geçersiz kılma yerine `MouseDown` olay görünümü denetleyicideki, kullanıyor hareketi tanıyıcı film şeritleri ile çalışırken, kullanıcı giriş olayını işlemek için.

Aşağıdaki hareketi tanıyıcıları macOS içinde kullanılabilir:

- `NSClickGestureRecognizer` -Fare aşağı ve olayları kaydedin.
- `NSPanGestureRecognizer` -Yazmaçlar Aşağı düğmesi fare, sürükleyin ve olayları bırakın.
- `NSPressGestureRecognizer` -Belirli bir zaman olay miktarı için fare düğmesini basılı kaydeder.
- `NSMagnificationGestureRecognizer` -Büyütme olay izleme paneli donanımdan kaydeder.
- `NSRotationGestureRecognizer` -Döndürme olay izleme paneli donanımdan kaydeder.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>Film şeridi başvuruları kullanma

Film şeridi başvuru büyük ve karmaşık bir film şeridi tasarım alabilir ve özgün başvurulan küçük film şeritleri içine bölün olanak tanır, böylece kaldırma karmaşıklık kaldırma ve elde edilen tek tek yaparak daha kolay tasarım film şeritleri ve korur.

Ayrıca, bir film şeridi başvuru sağlayabilir bir _bağlantı_ aynı film şeridi veya farklı bir üzerinde belirli bir görünüm içindeki başka bir görünüm için.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>Bir dış film şeridi başvurma

Bir dış film şeridi başvuru eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve seçin **Ekle** > **yeni dosya...**   >  **Mac** > **film şeridi**. Girin bir **adı** tıklatın ve yeni film şeridi için **yeni** düğmesi: 

    [![](indepth-images/ref01.png "Yeni bir film şeridi ekleme")](indepth-images/ref01.png#lightbox)
2. İçinde **Çözüm Gezgini**, Xcode'nın arabirimi Oluşturucusu'nda düzenlemek üzere açmak için yeni film şeridi adına çift tıklayın.
2. Normalde olur ve değişikliklerinizi kaydetmek gibi yeni film şeridi'nın planda düzenini tasarım: 

    [![](indepth-images/ref02.png "Arabirim tasarlama")](indepth-images/ref02.png#lightbox)
3. Başvuru arabirimi Oluşturucusu'nda eklemek zorunda kalacaklarını film şeridi geçin.
4. Sürükleme bir **film şeridi başvuru** gelen **Nesne Kitaplığı** tasarım yüzeyine: 

    [![](indepth-images/ref03.png "Film şeridi başvuru kitaplıkta seçme")](indepth-images/ref03.png#lightbox)
5. İçinde **özniteliği denetçisi**, adını seçin **film şeridi** yukarıda oluşturduğunuz: 

    [![](indepth-images/ref04.png "Başvuru yapılandırma")](indepth-images/ref04.png#lightbox)
6. Bir kullanıcı Arabirimi pencere öğesi (örneğin, bir düğme gibi) var olan bir görünüm üzerinde denetim tıklayın ve oluşturmak için yeni bir Segue **film şeridi başvuru** yeni oluşturduğunuz.  Açılan menüden seçin **Göster** Segue tamamlamak için: 

    [![](indepth-images/ref06.png "Ayar Segue türü")](indepth-images/ref06.png#lightbox) 
8. Film şeridi için yaptığınız değişiklikleri kaydedin.
9. Visual Studio değişikliklerinizi eşitlemek için Mac için geri dönün.

Uygulamayı çalıştırın ve Kimden, dış film şeridi Başvurusu'nda belirtilen film şeridi ilk penceresi denetleyicisinden Segue oluşturulan UI öğesinde kullanıcı tıkladığında görüntülenir.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>Bir dış film şeridi belirli Sahne başvurma

Belirli bir Sahne başvuru eklemek için bir dış film şeridi (ve değil ilk penceresi denetleyicisi), aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, Xcode'nın arabirimi Oluşturucusu'nda düzenlemek üzere açmak için dış film şeridi çift tıklayın.
2. Yeni bir görünüm ekleyin ve yerleşimi normal olarak tasarlamanız: 

    [![](indepth-images/ref07.png "Xcode'da düzeni tasarlama")](indepth-images/ref07.png#lightbox)
3. İçinde **kimlik denetçisi**, girin bir **film şeridi kimliği** yeni Sahne 's penceresi denetleyicisi için: 

    [![](indepth-images/ref08.png "Film şeridi kimliği ayarlama")](indepth-images/ref08.png#lightbox)
3. Başvuru arabirimi Oluşturucusu'nda eklemek zorunda kalacaklarını film şeridi açın.
4. Sürükleme bir **film şeridi başvuru** gelen **Nesne Kitaplığı** tasarım yüzeyine: 

    [![](indepth-images/ref03.png "Kitaplıktan bir film şeridi başvuru seçme")](indepth-images/ref03.png#lightbox)
5. İçinde **kimlik denetçisi**, adını seçin **film şeridi** ve **başvuru kimliği** (film şeridi kimliği), yukarıda oluşturduğunuz Sahne: 

    [![](indepth-images/ref09.png "Başvuru kimliği ayarlama")](indepth-images/ref09.png#lightbox)
6. Bir kullanıcı Arabirimi pencere öğesi (örneğin, bir düğme gibi) var olan bir görünüm üzerinde denetim tıklayın ve oluşturmak için yeni bir Segue **film şeridi başvuru** yeni oluşturduğunuz. Açılan menüden seçin **Göster** Segue tamamlamak için: 

    [![](indepth-images/ref06.png "Ayar Segue türü")](indepth-images/ref06.png#lightbox) 
8. Film şeridi için yaptığınız değişiklikleri kaydedin.
9. Visual Studio değişikliklerinizi eşitlemek için Mac için geri dönün.

Uygulamayı çalıştırın ve kullanıcı olduğunda tıkladığında, Segue, Sahne ile oluşturulan UI öğesi üzerinde verilen **film şeridi kimliği** dış film şeridi Başvurusu'nda belirtilen film şeridi görüntülenir.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>Aynı film şeridi belirli Sahne başvurma

Belirli bir Sahne aynı film şeridi başvuru eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, film şeridi düzenlemek üzere açmak için çift tıklayın.
2. Yeni bir görünüm ekleyin ve yerleşimi normal olarak tasarlamanız: 

    [![](indepth-images/ref11.png "Xcode'da film şeridi düzenleme")](indepth-images/ref11.png#lightbox)
3. İçinde **kimlik denetçisi**, girin bir **film şeridi kimliği** yeni Sahne 's penceresi denetleyicisi için: 

    [![](indepth-images/ref12.png "Film şeridi kimliği ayarlama")](indepth-images/ref12.png#lightbox)
3. Sürükleme bir **film şeridi başvuru** gelen **araç** tasarım yüzeyine: 

    [![](indepth-images/ref03.png "Kitaplıktan bir film şeridi başvuru seçme")](indepth-images/ref03.png#lightbox)
5. İçinde **özniteliği denetçisi**seçin **başvuru kimliği** (film şeridi kimliği), yukarıda oluşturduğunuz Sahne: 

    [![](indepth-images/ref13.png "Başvuru kimliği ayarlama")](indepth-images/ref13.png#lightbox)
6. Bir kullanıcı Arabirimi pencere öğesi (örneğin, bir düğme gibi) var olan bir görünüm üzerinde denetim tıklayın ve oluşturmak için yeni bir Segue **film şeridi başvuru** yeni oluşturduğunuz. Açılan menüden seçin **Göster** Segue tamamlamak için: 

    [![](indepth-images/ref06.png "Segue türü seçme")](indepth-images/ref06.png#lightbox) 
8. Film şeridi için yaptığınız değişiklikleri kaydedin.
9. Visual Studio değişikliklerinizi eşitlemek için Mac için geri dönün.

Uygulamayı çalıştırın ve kullanıcı olduğunda tıkladığında, Segue, Sahne ile oluşturulan UI öğesi üzerinde verilen **film şeridi kimliği** film şeridi Başvurusu'nda belirtilen aynı film şeridi görüntülenir.

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Karmaşık film şeridi örneği

Film şeritleri çalışmak Xamarin.Mac uygulamasında karmaşık bir örneği için lütfen bkz [SourceWriter örnek uygulaması](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter kod tamamlama ve basit sözdizimi vurgulama için destek sağlayan bir basit kaynak kod düzenleyicisidir.

SourceWriter kodu tam olarak geçersiz kılınan ve kullanılabilir olduğunda, bağlantılar sahip sağlanmalı temel teknolojileri veya yöntemlerini Xamarin.Mac kılavuzları belgelerinde ilgili bilgilere.

## <a name="related-links"></a>İlgili bağlantılar

- [MacStoryboard (örnek)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows ile birlikte çalışma](~/mac/user-interface/window.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
