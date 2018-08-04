---
title: Xamarin.iOS performans
description: Bu belge, performans ve bellek kullanımı Xamarin.iOS uygulamaları geliştirmek için kullanılan teknikleri açıklar.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/29/2016
ms.openlocfilehash: 40a2acf28819279b2a0d5c1d50c651a79b455465
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514469"
---
# <a name="xamarinios-performance"></a>Xamarin.iOS performans

Kötü uygulama performansı kendisini birçok şekilde gösterir. Bir uygulamayı yapabilirsiniz yanıt vermiyor, yavaş kaydırma neden olabilir ve pil ömrü azaltabilir. Ancak, performansı en iyi duruma getirme verimli kod daha fazlasını uygulama içerir. Uygulama performansı kullanıcı deneyimi de dikkate alınmalıdır. Örneğin, kullanıcının diğer etkinliklerini gerçekleştirmesi engellemeden işlemleri yürütmek sağlayarak kullanıcı deneyimini geliştirmek için yardımcı olabilir. 

Bu belge, performans ve bellek kullanımı Xamarin.iOS uygulamaları geliştirmek için kullanılan teknikleri açıklar.

> [!NOTE]
> Bu makalede okumadan önce okumalısınız [platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md), bellek kullanımı ve Xamarin platformu kullanılarak oluşturulan uygulamaların performansını artırmak için platform olmayan belirli teknikler açıklanır.

## <a name="avoid-strong-circular-references"></a>Güçlü döngüsel başvurulardan kaçının

Bazı durumlarda çöp toplayıcı tarafından elde edilen kendi bellek kalmamasını nesneleri engelleyebilir güçlü başvuru döngülerini oluşturmak mümkündür. Örneğin, bir durum düşünün burada bir [ `NSObject` ](https://developer.xamarin.com/api/type/Foundation.NSObject/)-öğesinden devralınan bir sınıf gibi bir alt sınıfı türetilmiş [ `UIView` ](https://developer.xamarin.com/api/type/UIKit.UIView/), eklenen bir `NSObject`-kapsayıcı türetilmiş ve kesin olan Objective-C, aşağıdaki kod örneğinde gösterildiği gibi başvurulan:

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    Container parent;
    public MyView (Container parent)
    {
        this.parent = parent;
    }

    void PokeParent ()
    {
        parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Bu kodu oluşturduğunda `Container` örnek, C# nesne Objective-C nesnesi için güçlü bir başvuru olacaktır. Benzer şekilde, `MyView` örneği de Objective-C nesnesi güçlü başvuruya sahip.

Ayrıca, çağrı `container.AddSubview` yönetilmeyen başvuru sayısını artırır `MyView` örneği. Bu durumda, Xamarin.iOS çalışma zamanı oluşturur bir `GCHandle` tutmak örneği `MyView` yönetilen kod yönetilen nesnelere herhangi bir garanti olduğundan Canlı nesneye bir başvuru olmanızı. Yönetilen kod perspektifinden `MyView` nesne iadesi sonra [ `AddSubview` ](https://developer.xamarin.com/api/member/UIKit.UIView.AddSubview/p/UIKit.UIView/) değil için olan çağrı `GCHandle`.

Yönetilmeyen `MyView` olmak bir `GCHandle` olarak yönetilen bir nesneye işaret eden, bilinen bir *güçlü bağlantı*. Yönetilen nesneye bir başvuru içerir `Container` örneği. Sırayla `Container` örneği yönetilen başvuruya sahip `MyView` nesne.

Burada bir kapsanan nesne kapsayıcısı için bir bağlantı tutar durumlarda, döngüsel başvuru ile uğraşmak için kullanılabilecek birkaç seçenek vardır:

-  El ile bağlantı kapsayıcıya ayarlayarak döngüden çıkmak `null`.
-  El ile kapsanan nesne kapsayıcıdan kaldırın.
-  Çağrı `Dispose` nesneler üzerinde.
-  Kapsayıcıya zayıf bir başvuru tutma döngüsel başvuru kaçının. Zayıf başvurular hakkında daha fazla bilgi için.

### <a name="using-weakreferences"></a>WeakReferences kullanma

Bir döngü önlemenin bir yolu, üst alt zayıf bir başvuru kullanmaktır. Örneğin, yukarıdaki kod şuna yazılabilir:

```csharp
class Container : UIView
{
    public void Poke ()
    {
        // Call this method to poke this object
    }
}

class MyView : UIView
{
    WeakReference<Container> weakParent;
    public MyView (Container parent)
    {
        this.weakParent = new WeakReference<Container> (parent);
    }

    void PokeParent ()
    {
        if (weakParent.TryGetTarget (out var parent))
            parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Burada, kapsanan nesne üst canlı tutar değil. Üst alt öğe üzerinden yapılan bir çağrı etkin tutar ancak `container.AddSubView`.

Bu, iOS burada bir eş sınıf ise uygulamayı içerir temsilci veya veri kaynağı desen kullanan API'ler de gerçekleşir; Örneğin, ayarlarken [ `Delegate` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.Delegate/) özelliği veya [ `DataSource` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.DataSource/) içinde [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) sınıfı.

Yalnızca bir protokol, örnek uygulama için oluşturulan sınıflar söz konusu olduğunda [ `IUITableViewDataSource` ](https://developer.xamarin.com/api/type/MonoTouch.UIKit.IUITableViewDataSource/)yapabilecekleriniz öğesinin alt sınıfı oluşturmak yerine, yalnızca sınıf içinde arabirim uygular ve geçersiz kılma yöntem ve Ata `DataSource` özelliğini `this`.

#### <a name="weak-attribute"></a>Zayıf özniteliği

[Xamarin.iOS 11.10](https://developer.xamarin.com/releases/ios/xamarin.ios_11/xamarin.ios_11.10/#WeakAttribute) sunulan `[Weak]` özniteliği. Gibi `WeakReference <T>`, `[Weak]` ayırmak için kullanılan [güçlü döngüsel başvurular](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/performance#avoid-strong-circular-references), ancak bile daha az kod.

Kullanan aşağıdaki kodu düşünün `WeakReference <T>`:

```csharp
public class MyFooDelegate : FooDelegate {
    WeakReference<MyViewController> controller;
    public MyFooDelegate (MyViewController ctrl) => controller = new WeakReference<MyViewController> (ctrl);
    public void CallDoSomething ()
    {
        MyViewController ctrl;
        if (controller.TryGetTarget (out ctrl)) {
            ctrl.DoSomething ();
        }
    }
}
```

Kullanan eşdeğer kod `[Weak]` çok daha kısa olabilir:

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

Kullanmanın başka bir örneği verilmiştir `[Weak]` bağlamında [temsilci](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html) Desen:

```csharp
public class MyViewController : UIViewController 
{
    WKWebView webView;

    protected MyViewController (IntPtr handle) : base (handle) { }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        webView = new WKWebView (View.Bounds, new WKWebViewConfiguration ());
        webView.UIDelegate = new UIDelegate (this);
        View.AddSubview (webView);
    }
}

public class UIDelegate : WKUIDelegate 
{
    [Weak] MyViewController controller;

    public UIDelegate (MyViewController ctrl) => controller = ctrl;

    public override void RunJavaScriptAlertPanel (WKWebView webView, string message, WKFrameInfo frame, Action completionHandler)
    {
        var msg = $"Hello from: {controller.Title}";
        var alertController = UIAlertController.Create (null, msg, UIAlertControllerStyle.Alert);
        alertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
        controller.PresentViewController (alertController, true, null);
        completionHandler ();
    }
}
```

### <a name="disposing-of-objects-with-strong-references"></a>Güçlü atıflar nesneleriyle atma

Güçlü bir başvuru var ve bağımlılığı kaldırmak zordur olun bir `Dispose` yöntemi üst işaretçi temizleyin.

Kapsayıcılar için geçersiz kılma `Dispose` yöntemini aşağıdaki kod örneğinde gösterildiği gibi kapsanan nesneleri Kaldır:

```csharp
class MyContainer : UIView
{
    public override void Dispose ()
    {
        // Brute force, remove everything
        foreach (var view in Subviews)
        {
              view.RemoveFromSuperview ();
        }
        base.Dispose ();
    }
}
```

Üst güçlü başvuru tutan bir alt nesne için üst başvuru Temizle `Dispose` uygulama:

```csharp
class MyChild : UIView 
{
    MyContainer container;
    public MyChild (MyContainer container)
    {
        this.container = container;
    }
    public override void Dispose ()
    {
        container = null;
    }
}
```

Tanımlayıcı başvurularını serbest bırakma hakkında daha fazla bilgi için bkz. [yayın IDisposable kaynakları](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
Bu Web günlüğü gönderisinde iyi bir tartışma de mevcuttur: [Xamarin.iOS, atık toplayıcı ve bana](http://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/).

### <a name="more-information"></a>Daha fazla bilgi

Daha fazla bilgi için bkz. [korumak döngüleri önlemek için kurallar](http://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) Cocoa ile Love üzerinde [bu bir hatadır MonoTouch gc'de](http://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) na StackOverflow, ve [MonoTouch GC yönetilen KILL refcount nesneleriyle'neden olamaz > 1? ](http://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) StackOverflow üzerinde.

## <a name="optimize-table-views"></a>Tablo görünümleri en iyi duruma getirme

Kullanıcıların, yumuşak kaydırma ve hızlı yükleme süreleri için beklediğiniz [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) örnekleri. Ancak, performans kaydırma hücre derin şekilde iç içe görünüm Hiyerarşiler içeriyor veya hücre karmaşık düzenler içerdiğinde olumsuz etkilenebilir. Ancak, düşük önlemek için kullanılır teknikler vardır `UITableView` performans:

- Hücreleri yeniden kullanın. Daha fazla bilgi için [hücreleri yeniden](#reusecells).
- Subviews sayısını azaltın.
- Bir web hizmetinden alınan önbellek hücre içeriği.
- Aynı değilse tüm satırların yüksekliğini önbelleğe alın.
- Hücre ve diğer görünümleri donuk olun.
- Resim ölçeklendirmeyi ve gradyanlar kaçının.

Bu teknikler tutmak topluca yardımcı olabilecek [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) yumuşak kaydırma örnekleri.

### <a name="reuse-cells"></a>Hücreleri yeniden kullanma

Satırlarda yüzlerce görüntülerken bir [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/), bellek yüzlerce oluşturmak için boşa harcanmasına olacaktır [ `UITableViewCell` ](https://developer.xamarin.com/api/type/UIKit.UITableViewCell/) nesneleri yalnızca az sayıda bunları ekranda aynı anda görüntülendiğinde. Bunun yerine, yalnızca ekranda görünen hücreleri belleğe yüklenebileceği **içeriği** hücreleri yeniden bunları yükleniyor. Bu ek nesneleri, süresi ve bellek kaydetme yüzlerce örneğinin engeller.

Bir hücreyi ekranından kaybolduğunda bu nedenle, onun görünümünü yeniden kullanmak, bir kuyruktaki aşağıdaki kod örneğinde gösterildiği gibi yerleştirilebilir:

```csharp
class MyTableSource : UITableViewSource
{
    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        // iOS will create a cell automatically if one isn't available in the reuse pool
        var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

        // Perform required cell actions
        return cell;
    }
}
```

Kullanıcı kaydırma yaparken [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) çağrıları `GetCell` görüntülemek için yeni görünümler isteği için geçersiz kılın. Ardından çağrıları bunu geçersiz kılmak [ `DequeueReusableCell` ](https://developer.xamarin.com/api/member/UIKit.UITableView.DequeueReusableCell/p/Foundation.NSString/) yöntemi ve bir hücre, döndürüleceği yeniden kullanmak üzere kullanılabilir olup olmadığını.

Daha fazla bilgi için [hücre yeniden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) içinde [bir tabloyu verilerle doldurma](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

## <a name="use-opaque-views"></a>Donuk görünümlerini kullanma

Tanımlı hiçbir saydamlık sahip tüm görünümleri olduğundan emin olun, [ `Opaque` ](https://developer.xamarin.com/api/property/UIKit.UIView.Opaque/) özellik kümesi. Bu görünüm en uygun şekilde çizim sistem tarafından işlenen garanti eder. İçinde bir görünüm eklendiğinde, bu özellikle önemli bir [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/), veya karmaşık bir animasyon bir parçasıdır. Aksi takdirde çizim sistem olacak bileşik görünümleri diğer içerikle performansı fark edilir derecede etkileyebilir.

## <a name="avoid-fat-xibs"></a>FAT XIBs kaçının

XIBs büyük ölçüde görsel Taslaklar ile değiştirilmiş olsa da, vardır bazı durumlarda burada XIBs hala kullanılabilir. Bir XIB belleğe yüklendiğinde tüm içeriğini tüm görüntüleri dahil olmak üzere belleğe yüklenir. XIB hemen kullanılmakta olan bir görünümü içeriyorsa, ardından bellek boşa harcanmış olur. Bu nedenle, XIBs kullanarak görünüm denetleyicisi başına yalnızca bir XIB olduğundan emin olun ve mümkün olduğunda, görünüm denetleyicinin hiyerarşisini görüntüle ayrı XIBs ayırın.

## <a name="optimize-image-resources"></a>Resim kaynakları en iyi duruma getirme

Görüntüleri olan bazı uygulamaları kullanan en pahalı kaynaklara ve genellikle yüksek çözünürlüklerde yakalanır. Bu nedenle, uygulamanın pakette bir görüntüden görüntülerken bir [ `UIImageView` ](https://developer.xamarin.com/api/type/UIKit.UIImageView/), görüntü olduğundan emin olun ve `UIImageView` aynı boyutta. Çalışma zamanında görüntüleri ölçekleme olabilir, pahalı bir işlem varsa, özellikle `UIImageView` katıştırılmış bir [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/).

Daha fazla bilgi için [resim kaynakları en iyi duruma getirme](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) içinde [platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md) Kılavuzu.

## <a name="test-on-devices"></a>Cihazlarda test edin

Dağıtma ve mümkün olduğunca erken bir uygulamayı fiziksel bir cihaz üzerinde test başlayın. Cihaz kısıtlamaları ve davranışları simülatörleri mükemmel eşleşmiyor ve bu nedenle mümkün olduğunca erken gerçek cihaz senaryosunda test etmek önemlidir.

Özellikle simülatör herhangi bir şekilde bellek veya CPU kısıtlamaları fiziksel bir cihazın benzetimini.

## <a name="synchronize-animations-with-the-display-refresh"></a>Animasyonları görüntü yenileme ile Eşitle

Oyun, oyun mantığı çalıştırın ve ekrandaki güncelleştirmek için sıkı döngüler sahip eğilimindedir. Tipik bir çerçeve oranları aralığından otuz saniye başına altmış çerçeveleri. Bazı geliştiriciler, ekran ekranına güncelleştirmeleri, oyun benzetimi birleştirme saniyede mümkün olduğunca fazla kez olarak güncelleştirmeniz gerekir ve altmış Saniyedeki ötesine geçip fikri size cazip olabilir düşünüyor.

Ancak, görüntü sunucunun saniyede altmış sayısı üst sınırını ekran güncelleştirmeleri gerçekleştirir. Bu nedenle, bu sınırı daha hızlı güncelleştirme ekranı çalışılıyor bozmadan ve mikro Takılmayı ekran açabilir. Ekran güncelleştirmeleri görüntüleme güncelleştirmesiyle eşitlenebilmesi amacıyla kod için idealdir. Bu kullanarak gerçekleştirilebilir [ `CoreAnimation.CADisplayLink` ](https://developer.xamarin.com/api/type/CoreAnimation.CADisplayLink/) altmış görselleştirme için uygun bir süre ölçer sınıfını ve, çalışan oyunlar saniye başına çerçeve.

## <a name="avoid-core-animation-transparency"></a>Temel animasyon saydamlık kaçının

Temel animasyon saydamlık önleme, bit eşlem birleştirme performansı artırır. Genel olarak, saydam katmanları ve bulanık Kenarlıklar mümkün olduğunda kaçının.

## <a name="avoid-code-generation"></a>Kod oluşturma kaçının

Dinamik olarak kod oluşturma `System.Reflection.Emit` veya *dinamik dil çalışma zamanı* iOS çekirdek dinamik kod yürütme engellediğinden kaçınılmalıdır.

## <a name="summary"></a>Özet

Bu makalede açıklanan ve Xamarin.iOS ile oluşturulan uygulamaların performansını artırmaya yönelik teknikleri ele alınan. Topluca bu tekniklerin bir CPU ve bir uygulama tarafından kullanılan bellek miktarı tarafından gerçekleştirilen iş miktarını önemli ölçüde azaltabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md)
