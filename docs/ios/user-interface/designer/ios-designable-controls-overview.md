---
title: İOS için Xamarin Tasarımcısı'nda özel denetimler
description: İOS için Xamarin Tasarımcısı projenizde oluşturulan veya Xamarin bileşen Deposu'nda gibi dış kaynaklardan başvurulan işleme özel denetimler destekler.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 113fab2fd0d1a055d566606885cefbafe3185529
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>İOS için Xamarin Tasarımcısı'nda özel denetimler

_İOS için Xamarin Tasarımcısı projenizde oluşturulan veya Xamarin bileşen Deposu'nda gibi dış kaynaklardan başvurulan işleme özel denetimler destekler._

İOS için Xamarin Tasarımcısı bir uygulamanın kullanıcı arabiriminde görselleştirme için güçlü bir araçtır ve WYSIWYG düzenleme çoğu iOS görünümleri ve görünüm denetleyicileri için destek sağlar. Uygulamanızı iOS yerleşik olanlar genişletmek özel denetimler de içerebilir. Bu özel denetimler aklınızda birkaç yönergeleri ile yazılan, iOS daha zengin bir düzenleme deneyimi sağlayan tasarımcısı tarafından da oluşturulabilir. Bu belgede, bu yönergeleri göz alır.

## <a name="requirements"></a>Gereksinimler

Aşağıdaki gereksinimleri karşılayan bir denetim tasarım yüzeyine oluşturulur:

1.  Doğrudan veya dolaylı öğesinin bir alt kümesi olan [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) veya [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller). Diğer [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) alt sınıfların, simge tasarım yüzeyine olarak görünür.
2.  Bunun bir [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Objective-c kullanıma sunmak için
3.  Bunun [gerekli IntPtr Oluşturucusu](~/ios/internals/api-design/index.md).
4.  Ya da uygulayan [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) arabirim veya sahip bir [DesignTimeVisibleAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DesignTimeVisibleAttribute/) True olarak ayarlayın.

Yukarıdaki gereksinimleri karşılamak kod içinde tanımlanan denetimleri simülatörü içeren kendi projesi derlendiğinde Tasarımcısı'nda görünür. Varsayılan olarak, tüm özel denetimlerini görünür **özel bileşenler** bölümünü **araç**. Ancak [CategoryAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.CategoryAttribute/) farklı bir bölümü belirtmek için özel denetimin sınıfa uygulanabilir.

Tasarımcı üçüncü taraf Objective-C kitaplıkları yükleme desteklemez.

## <a name="custom-properties"></a>Özel Özellikler

Aşağıdaki koşullar doğruysa özel bir denetim tarafından bildirilen bir özellik özellik panelinde görüntülenir:

1.  Özellik, ortak bir alıcı ve ayarlayıcıya sahip.
1.  Özellik sahip bir [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) yanı sıra bir [BrowsableAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.BrowsableAttribute/) True olarak ayarlayın.
1.  Özellik türü olan bir sayısal türü, numaralandırma türü, string, bool, [SizeF](https://developer.xamarin.com/api/type/System.Drawing.SizeF/), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), veya [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/). Bu liste desteklenen türlerinin gelecekte genişletilmiş.


Özelliği de ile donatılmış bir [DisplayNameAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DisplayNameAttribute/) için özellik panelinde görüntülenen etiketi belirlemek için.

## <a name="initialization"></a>Başlatma

İçin `UIViewController` alt sınıfların, kullanmanız gereken [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) Tasarımcısı'nda oluşturulan görünümleri bağlıdır kod yöntemi.

İçin `UIView` ve diğer `NSObject` alt sınıfların, [AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) düzeni dosyasından yüklendikten sonra özel denetimin başlatılmasını gerçekleştirmek için önerilen yer bir yöntemdir. Özellik Masası'nda belirlenen tüm özel özellikler denetimin Oluşturucusu çalıştırın, ancak önce ayarlanır ayarlanmaz olmasıdır `AwakeFromNib` adı verilir:


```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        // Initialize the view here.
    }
}
```

Denetim ayrıca doğrudan kodunuzdan oluşturulacak tasarlandıysa, bu gibi ortak başlatma koduna sahip bir yöntem oluşturmak isteyebilirsiniz:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
    }
}
```

## <a name="property-initialization-and-awakefromnib"></a>Özellik başlatma ve AwakeFromNib

Ne zaman ve nereye designable özelliklerinde Tasarımcısı iOS içinde ayarlanan değerler üzerine için özel bir bileşen başlatılamadı dikkatli olunmalıdır. Örnek olarak, aşağıdaki kodu alın:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {
    
    [Export ("Counter"), Browsable (true)]
    public int Counter {get; set;}

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
        Counter = 0;
    }
}
```

`CustomView` Bileşen kullanıma sunan bir `Counter` Tasarımcısı iOS içinde geliştirici tarafından ayarlanan özelliği. Ancak, hangi değer değerini Tasarımcı içinde ayarlanır olsun `Counter` özelliği her zaman sıfır (0) olabilir. Neden şöyledir:

-  Örneği `CustomControl` film şeridi dosyasından şişirileceğini.
-  İOS Tasarımcısı'nda değiştiren herhangi bir özellik kümesi (değerini ayarlama gibi `Counter` için iki (2), örneğin).
-  `AwakeFromNib` Yöntemi yürütüldüğünde ve bileşenin bir çağrı yapılır `Initialize` yöntemi.
-  İçinde `Initialize` değerini `Counter` özelliği sıfırlanır sıfır (0).


Yukarıdaki durumu düzeltmek için ya da başlatma `Counter` özelliği başka bir yerde (bileşenin Oluşturucusu olduğu gibi) veya geçersiz kılmaz `AwakeFromNib` yöntemi ve çağrı `Initialize` bileşen ne dışında başka hiçbir başlatma gerektiriyorsa şu anda kendi oluşturucular tarafından işlenen.

## <a name="design-mode"></a>Tasarım modu

Tasarım yüzeyine bir özel denetim için birkaç kısıtlamaları uyması gerekir:

-  Uygulama paket kaynaklarını Tasarım modunda kullanılamaz. Görüntüleri üzerinden yüklendiğinde kullanılabilir [UIImage yöntemleri](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) .
-  Web istekleri gibi zaman uyumsuz işlemleri Tasarım modunda gerçekleştirilmemelidir. Tasarım yüzeyine animasyon veya denetimin UI için zaman uyumsuz diğer güncelleştirmeleri desteklemiyor.


Özel bir denetim uygulayabilirsiniz [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) ve [DesignMode](https://developer.xamarin.com/api/property/System.ComponentModel.ISite.DesignMode/) özelliği tasarım yüzeyine olup olmadığını denetleyin. Bu örnekte, etiket "Tasarım modu" tasarım yüzeyi ve "Çalışma zamanı" çalışma zamanında görüntülenir:

```csharp
[Register ("DesignerAwareLabel")]
public class DesignerAwareLabel : UILabel, IComponent {

    #region IComponent implementation

    public ISite Site { get; set; }
    public event EventHandler Disposed;

    #endregion

    public DesignerAwareLabel (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        if (Site != null &amp;&amp; Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

Her zaman denetlemelisiniz `Site` özelliği için `null` üyeleri hiçbirini erişmeyi denemeden önce. Varsa `Site` olan `null`, Denetim Tasarımcısı'nda çalışmıyor varsaymak güvenlidir.
Tasarım modunda `Site` denetimin Oluşturucusu çalıştırıldıktan sonra ve önce ayarlanacak `AwakeFromNib` olarak adlandırılır.

## <a name="debugging"></a>Hata Ayıklama

Yukarıdaki gereksinimleri karşılayan bir denetim araç kutusunda görüntülenir ve yüzeysel olarak çizilir.
Bir denetim işlenmez denetimi veya bağımlılıklarından biri hatalar için kontrol edin.

Tasarım yüzeyine genellikle diğer denetimlerini işlemeye devam ederken ayrı ayrı denetimler tarafından oluşturulan özel durumları yakalayabilir. Hatalı denetim kırmızı tutucuyla değiştirilir ve özel durum izleme ünlem işareti simgesine tıklayarak görüntüleyebilirsiniz:

 ![](ios-designable-controls-overview-images/exception-box.png "Hatalı bir denetim olarak kırmızı yer tutucu ve özel durum ayrıntıları")

Hata ayıklama simgeleri denetimi için kullanılabilen, izleme dosya adlarını ve satır numaralarını yüklemeniz gerekir. Çift yığın izlemesi satırda tıklatarak söz konusu hatta kaynak kodundaki atlayacaktır.

Tasarımcı hatalı denetim ayıramazsınız, tasarım yüzeyine üstünde bir uyarı iletisi görüntülenir:

 ![](ios-designable-controls-overview-images/info-bar.png "Tasarım yüzeyine üstünde bir uyarı iletisi")

Hatalı denetim sabit veya tasarım yüzeyden kaldırıldığında tam işleme devam eder.

## <a name="summary"></a>Özet

Bu makalede oluşturma ve uygulamayı iOS Tasarımcısı'nda özel denetimlerin sunmuştur. İlk denetimleri tasarım yüzeyine oluşturulması ve özellik panelinde özel özellikleri kullanıma sunmak için karşılaması gereken gereksinimleri açıklanmaktadır. Arka plan kod - denetim ve DesignMode özelliği başlatma sırasında daha sonra arama. Son olarak neler açıklanan ne zaman özel durumlar ve bu nasıl çözülür.