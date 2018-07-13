---
title: İOS için Xamarin Tasarımcısı'nda özel denetimler
description: İOS için Xamarin Tasarımcı projenizde oluşturulan veya Xamarin bileşeni Store gibi dış kaynaklardan başvurulan özel denetimler oluşturma destekler.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 05b190f4bfd4058e9e2f6e465e6026fa76dce6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995703"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>İOS için Xamarin Tasarımcısı'nda özel denetimler

_İOS için Xamarin Tasarımcı projenizde oluşturulan veya Xamarin bileşeni Store gibi dış kaynaklardan başvurulan özel denetimler oluşturma destekler._

İOS için Xamarin Tasarımcı, bir uygulamanın kullanıcı arabirimini görselleştirmek için güçlü bir araçtır ve WYSIWYG düzenleme çoğu iOS görünümleri ve görünüm denetleyicileri için destek sağlar. Uygulamanızı iOS yerleşik olanları genişleten özel denetimler de içerebilir. Bu özel denetimler aklınızda birkaç yönergeleri ile yazılan, iOS Designer, daha kapsamlı bir düzenleme deneyimi sağlama çalışmaları tarafından da oluşturulabilir. Bu belge, bu yönergeleri göz alır.

## <a name="requirements"></a>Gereksinimler

Aşağıdaki tüm gereksinimleri karşılayan bir denetimi tasarım yüzeyinde işlenir:

1.  Doğrudan veya dolaylı öğesinin olduğu [Uıview](https://developer.xamarin.com/api/type/UIKit.UIView/) veya [Uıviewcontroller](https://developer.xamarin.com/api/type/UIKit.UIView/Controller). Diğer [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) alt sınıflar, tasarım yüzeyinde bir simge olarak görünür.
2.  Bunun bir [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Objective-c göstermek için
3.  Sahip [gerekli IntPtr Oluşturucusu](~/ios/internals/api-design/index.md).
4.  Ya da uygulayan [IComponent](xref:System.ComponentModel.IComponent) arabirim veya sahip bir [DesignTimeVisibleAttribute](xref:System.ComponentModel.DesignTimeVisibleAttribute) True olarak ayarlayın.

Yukarıdaki gereksinimleri karşılayan, kod içinde tanımlanan denetimleri, içeren kendi projesi için simülatör derlendiğinde Tasarımcısı'nda görünür. Varsayılan olarak, tüm özel denetimleri içinde görünür **özel bileşenler** bölümünü **araç kutusu**. Ancak [CategoryAttribute](xref:System.ComponentModel.CategoryAttribute) farklı bir bölüme belirtmek için özel denetimin sınıfa uygulanabilir.

Tasarımcı, üçüncü taraf Objective-C kitaplıklarını yükleme desteklemez.

## <a name="custom-properties"></a>Özel Özellikler

Aşağıdaki koşullar doğruysa özel bir denetim tarafından bildirilen bir özelliği özellik panelinde görüntülenir:

1.  Genel alıcı ve ayarlayıcı özelliği vardır.
1.  Özelliğine sahip bir [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) yanı [BrowsableAttribute](xref:System.ComponentModel.BrowsableAttribute) True olarak ayarlayın.
1.  Özellik türü olan bir sayısal tür, numaralandırma türü, string, bool, [SizeF](xref:System.Drawing.SizeF), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), veya [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/). Desteklenen türler'ın bu liste gelecekte genişletilmiş.


Özelliği de ile donatılabilir bir [DisplayNameAttribute](xref:System.ComponentModel.DisplayNameAttribute) için özellik panelinde görüntülenen etiketi belirlemek için.

## <a name="initialization"></a>Başlatma

İçin `UIViewController` alt sınıflarından kullanmalıdır [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) Tasarımcısı'nda, oluşturduğunuz görünümleri bağlı olan kod için yöntemi.

İçin `UIView` ve diğer `NSObject` alt sınıflarından [AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) Düzen dosyasından yüklendikten sonra özel denetiminizin başlatma gerçekleştirmek için önerilen yer yöntemidir. Herhangi bir özel özellik Masası'nda belirlenen özelliği denetimin Oluşturucusu çalıştırın, ancak önce ayarlanır ayarlanmaz olmasıdır `AwakeFromNib` çağrılır:


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

Denetim ayrıca doğrudan koddan oluşturulacak tasarlanmışsa, bunun gibi ortak başlatma kodunu içeren bir yöntem oluşturmak isteyebilirsiniz:

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

Ne zaman ve nereye iOS Designer içinde ayarlanmış olan değerlerin üzerine değil için farklı bir özel bileşene designable özelliklerinde başlatmak dikkatli olunması. Örneğin, aşağıdaki kodu alın:

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

`CustomView` Bileşeni sunan bir `Counter` iOS Designer içinde geliştirici tarafından ayarlanabilir özelliği. Ancak, hangi değeri değerini Tasarımcı içinde ayarlanmış olsun `Counter` özelliği her zaman sıfır (0) olur. Bunu istememizin nedeni:

-  Örneği `CustomControl` film şeridi dosyasından şişirileceğini.
-  İOS Tasarımcısı'nda değiştiren herhangi bir özelliği ayarlayın (değerini ayarlamak gibi `Counter` için iki (2), örneğin).
-  `AwakeFromNib` Yöntemi yürütüldüğünde ve bileşenin bir çağrı yapılır `Initialize` yöntemi.
-  İçinde `Initialize` değerini `Counter` özelliği sıfırlanır sıfır (0).


Yukarıdaki durumu düzeltmek için ya da başlatması `Counter` özelliği başka bir yerde (bileşenin Oluşturucu gibi) veya geçersiz kılma `AwakeFromNib` yöntemi ve çağrı `Initialize` bileşeni ne dışında başka hiçbir başlatma gerektirmesi durumunda şu anda, oluşturucular tarafından işlenen.

## <a name="design-mode"></a>Tasarım modu

Tasarım yüzeyinde, bir özel denetim için birkaç kısıtlama uyması gerekir:

-  Uygulama paket kaynaklarını Tasarım modunda kullanılamaz. Görüntülerin kullanılabilir aracılığıyla yüklendiğinde [UIImage yöntemleri](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) .
-  Web istekleri gibi zaman uyumsuz işlemler, Tasarım modunda gerçekleştirilmemelidir. Tasarım yüzeyinde, animasyon veya diğer zaman uyumsuz güncelleştirmeleri denetimin kullanıcı arabirimi desteklemiyor.


Özel denetim uygulayabilirsiniz [IComponent](xref:System.ComponentModel.IComponent) ve [DesignMode](xref:System.ComponentModel.ISite.DesignMode) tasarım yüzeyinde olup olmadığı denetlenecek özellik. Bu örnekte, etiketin "Tasarım modu" tasarım yüzeyi ve "Çalışma zamanı" çalışma zamanında görüntüler:

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

Her zaman denetlemelisiniz `Site` özelliği `null` üyelerine erişmek denemeden önce. Varsa `Site` olduğu `null`, bu denetim Tasarımcısı'nda çalışmıyor varsayabiliriz.
Tasarım modunda `Site` denetimin Oluşturucusu çalıştırdıktan sonra ve önce ayarlanır `AwakeFromNib` çağrılır.

## <a name="debugging"></a>Hata Ayıklama

Yukarıdaki gereksinimleri karşılayan bir denetimi araç kutusunda görüntülenen ve yüzeyine çizilir.
Bir denetim işlenmez, denetim ya da bağımlılıklarından biri hataları kontrol edin.

Tasarım yüzeyinde, genellikle diğer denetimlerini işlemeye devam ederken tek denetimleri tarafından oluşturulan özel durumları yakalayabilir. Hatalı bir denetim kırmızı tutucuyla değiştirilir ve özel durum izleme ünlem simgesine tıklayarak görüntüleyebilirsiniz:

 ![](ios-designable-controls-overview-images/exception-box.png "Hatalı bir denetim kırmızı yer tutucu ve özel durum ayrıntıları")

Denetim için hata ayıklama sembolleri kullanamıyorsanız, izleme, dosya adları ve satır numaralarını olacaktır. Kaynak kodunda bu satıra çift tıklayarak yığın izlemesi bir satırda atlayacaktır.

Hatalı bir denetim Tasarımcısı ayıramazsınız, tasarım yüzeyinin üst kısmında bir uyarı iletisi görüntülenir:

 ![](ios-designable-controls-overview-images/info-bar.png "Tasarım yüzeyinde üstünde bir uyarı iletisi")

Hatalı denetim sabit veya tasarım yüzeyine kaldırıldı tam oluşturma devam edecek.

## <a name="summary"></a>Özet

Bu makalede oluşturma ve uygulamayı iOS Tasarımcısı'nda özel denetimlerin sunmuştur. İlk denetimleri tasarım yüzeyinde oluşturulması ve özel özellikler özelliği panelinde ortaya çıkarmak için karşılaması gereken gereksinimleri açıklanmaktadır. Ardından arkasında - denetim ve DesignMode özellik başlatma kodu görünüyordu. Son olarak ne açıklanan olduğunda özel durumlar ve bu sorunun çözümü.