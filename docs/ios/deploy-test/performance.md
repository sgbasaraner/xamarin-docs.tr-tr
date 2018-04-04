---
title: Xamarin.iOS Performance
description: Xamarin.iOS ile oluşturulan uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir. Bu makalede ve bu teknikler anlatılmaktadır.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/29/2016
ms.openlocfilehash: 3fc6263aa99edb94ae69f1ce8f87835043477392
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinios-performance"></a>Xamarin.iOS Performance

_Xamarin.iOS ile oluşturulan uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir. Bu makalede ve bu teknikler anlatılmaktadır._

Zayıf uygulama performans kendisini birçok yolla gösterir. Bir uygulama yapabilirsiniz yanıt vermeyen gibi görünebilir, yavaş kaydırma neden olabilir ve pil ömrünün azaltabilir. Ancak, performansı en iyi duruma getirme daha fazlasını verimli kod uygulama içerir. Uygulama performansı kullanıcı deneyimi de dikkate alınmalıdır. Örneğin, diğer etkinlikler gerçekleştirmeyi kullanıcı engellenmeden işlemlerini yürütmek sağlayarak kullanıcı deneyimini geliştirmek için yardımcı olabilir.

Performans ve Xamarin.iOS ile oluşturulan uygulamalar, algılanan performansını artırmak için teknikler mevcuttur. Bunlara aşağıdakiler dahildir:

- [Güçlü Başvuru döngüleri kaçının](#avoidcircularreferences)
- [Tablo görünümleri en iyi duruma getirme](#optimizetableviews)
- [Donuk görünümleri kullanma](#opaqueviews)
- [FAT XIBs kaçının](#avoidfatxibs)
- [Görüntü kaynakları en iyi duruma getirme](#optimizeimages)
- [Aygıtlarda test](#testondevices)
- [Görüntü yenilemeye animasyonları Eşitle](#synchronizeanimations)
- [Çekirdek animasyon saydamlık kaçının](#avoidtransparency)
- [Kod oluşturma kaçının](#avoidcodegeneration)

> [!NOTE]
> Bu makalede okumadan önce ilk okumalısınız [platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md), bellek kullanımı ve Xamarin platformu kullanılarak oluşturulan uygulamaların performansını artırmak için platform olmayan belirli teknikler açıklanır.

<a name="avoidcircularreferences" />

## <a name="avoid-strong-circular-references"></a>Güçlü döngüsel başvurulara kaçının

Bazı durumlarda nesneleri çöp toplayıcı tarafından iadesi kendi bellek kalmaktan önleyebilir güçlü başvuru döngüleri oluşturmak mümkündür. Örneğin, bir durum düşünün burada bir [ `NSObject` ](https://developer.xamarin.com/api/type/Foundation.NSObject/)-öğesinden devralınan bir sınıf gibi bir alt türetilmiş [ `UIView` ](https://developer.xamarin.com/api/type/UIKit.UIView/), eklenen bir `NSObject`-kapsayıcı türetilmiş ve güçlü olan Objective-C, aşağıdaki kod örneğinde gösterildiği gibi başvurulan:

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

Bu kodu oluşturduğunda `Container` örneği, C# nesne Objective-C nesneye güçlü bir başvuru olacaktır. Benzer şekilde, `MyView` örneği de güçlü bir başvuru Objective-C nesneye sahip.

Ayrıca, çağrısı `container.AddSubview` yönetilmeyen başvuru sayısı artacaktır `MyView` örneği. Bu gerçekleştiğinde, Xamarin.iOS çalışma zamanı oluşturur bir `GCHandle` tutmak için örnek `MyView` yönetilen kod olduğu için yönetilen nesnelerle herhangi bir garanti Canlı nesnesinde bir başvuru olmanızı. Yönetilen kod açısından `MyView` nesne iadesi sonra [ `AddSubview` ](https://developer.xamarin.com/api/member/UIKit.UIView.AddSubview/p/UIKit.UIView/) çağrısı değil için olan `GCHandle`.

Yönetilmeyen `MyView` nesne sahip olacak bir `GCHandle` olarak yönetilen nesneye işaret eden, bilinen bir *güçlü bağlantı*. Yönetilen Nesne başvuru içerecek `Container` örneği. Sırayla `Container` örneği yönetilen başvuru olacaktır `MyView` nesnesi.

Burada bir kapsanan nesne kapsayıcısı için bir bağlantı tutar durumlarda, döngüsel başvuru ile mücadele etmek kullanılabilir birkaç seçenek vardır:

-  El ile bağlantıyı kapsayıcıya ayarlanarak döngüsü sonu `null`.
-  El ile kapsanan nesne kapsayıcıdan kaldırın.
-  Çağrı `Dispose` nesneler üzerinde.
-  Kapsayıcı zayıf başvuru tutma döngüsel başvuru kaçının. Zayıf başvurular hakkında daha fazla bilgi için.

### <a name="using-weakreferences"></a>WeakReferences kullanma

Bir döngü önleme bir yolu, zayıf başvurularından alt üst kullanmaktır, örneğin, yukarıdaki kod şu şekilde yazılabilir:

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

Üst alt yapılan çağrı aracılığıyla etkin tutar yalnızca kapsanan nesne üst canlı, tutmayacaktır, yani `container.AddSubView`.

Bu deyim iOS temsilci ya da veri kaynağı düzeni burada eş sınıfı uygulama içereceği ayarlarken örneğin kullanın API'ları da yapılır. [ `Delegate` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.Delegate/) özelliği veya [ `DataSource` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.DataSource/) içinde [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) sınıfı.

Tamamen protokolü, örneğin uygulamak amacıyla oluşturulan sınıflar durumunda [ `IUITableViewDataSource` ](https://developer.xamarin.com/api/type/MonoTouch.UIKit.IUITableViewDataSource/)neler yapabileceğiniz bir alt sınıfı oluşturmak yerine, yalnızca sınıfında arabirimini uygulayan ve geçersiz kılma yöntem ve Ata `DataSource` özelliğine `this`.

### <a name="disposing-of-objects-with-strong-references"></a>Güçlü başvurularıyla nesnelerin atma

Güçlü bir başvuru varsa ve bağımlılığı kaldırmak zordur olun bir `Dispose` yöntemi üst işaretçi temizleyin.

Kapsayıcıları için geçersiz kılma `Dispose` yöntemi aşağıdaki kod örneğinde gösterildiği gibi kapsanan nesneleri kaldırmak için:

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

Üst güçlü başvuru tutan bir alt nesne için üst referansı temizleyin `Dispose` uygulama:

```csharp
    class MyChild : UIView {
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

Güçlü başvurularını serbest bırakma hakkında daha fazla bilgi için bkz: [yayın IDisposable kaynakları](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
Ayrıca bu blog gönderisine iyi bir tartışmada vardır: [Xamarin.iOS, atık toplayıcı ve bana](http://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/).

### <a name="more-information"></a>Daha fazla bilgi

Daha fazla bilgi için bkz: [korumak döngüleri önlemek için kuralları](http://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) Cocoa ile Love üzerinde [bu MonoTouch GC hatasıdır](http://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) StackOverflow üzerinde ve [MonoTouch GC yönetilen KILL refcount nesneleriyle'neden olamaz > 1? ](http://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) StackOverflow üzerinde.


<a name="optimizetableviews" />

## <a name="optimize-table-views"></a>Tablo görünümleri en iyi duruma getirme

Kullanıcıların beklediğiniz yumuşak kaydırma ve hızlı yükleme süreleri için [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) örnekleri. Ancak, performans kaydırma hücreleri iç içe görünüm hiyerarşileri içerdiğinde veya hücre karmaşık düzenleri içerdiğinde olumsuz etkilenebilir. Ancak, düşük önlemek için kullanılan teknikleri vardır `UITableView` performans:

- Hücreleri yeniden kullanabilirsiniz. Daha fazla bilgi için bkz: [hücreleri yeniden](#reusecells).
- Subviews sayısını azaltın.
- Bir web hizmetinden alınan önbellek hücre içeriği.
- Aynı değilse herhangi bir satır yüksekliğini önbelleğe alır.
- Hücre ve diğer görünümlere donuk olun.
- Resim ölçekleme ve gradyan kaçının.

Bu teknikler kalmasına topluca yardımcı olur [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) sorunsuz kaydırma örnekleri.

<a name="reusecells" />

### <a name="reuse-cells"></a>Hücreleri yeniden

Satırlarda yüzlerce görüntülerken bir [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/), yüzlerce oluşturmak için bellek kaybı olacaktır [ `UITableViewCell` ](https://developer.xamarin.com/api/type/UIKit.UITableViewCell/) nesneleri yalnızca az sayıda bunları ekranda aynı anda görüntülendiğinde. Bunun yerine, yalnızca ekranda görünür hücreleri belleğe yüklenebileceği **içerik** hücreleri yeniden bunları yükleniyor. Örnekleme süresi ve bellek kaydetme ek nesneler yüzlerce engeller.

Bir hücrenin ekranından kaybolduğunda bu nedenle, kendi görünüm yeniden kullanmak üzere, bir kuyruktaki aşağıdaki kod örneğinde gösterildiği gibi yerleştirilebilir:

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

Kullanıcı kayarken [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) çağrıları `GetCell` görüntülemek için yeni görünümler isteği için geçersiz kılın. Bu geçersiz sonra çağrıları [ `DequeueReusableCell` ](https://developer.xamarin.com/api/member/UIKit.UITableView.DequeueReusableCell/p/Foundation.NSString/) yöntemi ve bir hücre, döndürülecek kullanılabilmeleri için ise.

Daha fazla bilgi için bkz: [hücre yeniden](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) içinde [verilerinin bulunduğu bir tablo doldurma](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

<a name="opaqueviews" />

## <a name="use-opaque-views"></a>Donuk görünümleri kullanma

Tanımlı hiçbir saydamlığı olan görünümleri sahip olduğundan emin olun, [ `Opaque` ](https://developer.xamarin.com/api/property/UIKit.UIView.Opaque/) özellik kümesi. Bu, görünümleri en iyi şekilde çizim sistem tarafından işlenen güvence altına alır. Bir görünüm içindeki yerleşik olduğunda, bu özellikle önemlidir bir [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/), veya karmaşık bir animasyon bir parçasıdır. Aksi takdirde çizim sistem olacak bileşik görünümleri diğer içerikle performansı önemli ölçüde etkileyebilir.

<a name="avoidfatxibs" />

## <a name="avoid-fat-xibs"></a>FAT XIBs kaçının

XIBs büyük ölçüde film şeritleri tarafından değiştirilmiştir, vardır, ancak bazı durumlarda nerede XIBs hala kullanılıyor olabilir. Bir XIB belleğe yüklendiğinde, tüm içeriğini tüm görüntüleri dahil olmak üzere belleğe yüklenir. XIB hemen kullanılmakta olan bir görünümü içeriyorsa, ardından bellek boşa harcanmış olur. Bu nedenle, XIBs kullanırken görünüm denetleyici başına yalnızca bir XIB olduğundan emin olun ve mümkünse, görünüm denetleyicinin görünüm hiyerarşi ayrı XIBs ayırın.

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Görüntü kaynakları en iyi duruma getirme

Görüntüleri uygulamaları kullanan, en pahalı kaynakları bazıları verilmiştir ve genellikle yüksek çözünürlüklerde yakalanır. Bu nedenle, uygulamanın pakette görüntüden görüntülenirken bir [ `UIImageView` ](https://developer.xamarin.com/api/type/UIKit.UIImageView/), görüntü emin olun ve `UIImageView` aynı boyutta. Çalışma zamanında görüntüleri ölçekleme olabilir, pahalı bir işlem, özellikle `UIImageView` içinde katıştırılmış bir [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/).

Daha fazla bilgi için bkz: [görüntü kaynakları en iyi duruma getirme](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) içinde [platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md) Kılavuzu.

<a name="testondevices" />

## <a name="test-on-devices"></a>Aygıtlarda test

Dağıtma ve bir fiziksel aygıt olabildiğince erken test başlayın. Davranışları ve aygıtları sınırlamaları benzeticileri mükemmel eşleşmiyor ve bu nedenle olabildiğince erken gerçek cihaz senaryosunda test etmek önemlidir.

Özellikle simulator herhangi bir yolla değil bellek veya CPU kısıtlamaları bir fiziksel cihazın benzetimini.

<a name="synchronizeanimations" />

## <a name="synchronize-animations-with-the-display-refresh"></a>Görüntü yenilemeye animasyonları Eşitle

Oyunlar oyun mantığı çalıştırın ve ekrandaki güncelleştirmek için sıkı döngüler sahip olma eğilimi gösterir. Tipik çerçeve oranları aralığından otuz saniye başına altmış çerçeveleri. Bazı geliştiriciler düşünüyor: ekran kendi oyun benzetimi ekranına güncelleştirmeleriyle birleştirme saniyede mümkün olduğunca çok kez olarak güncelleştirmeniz gerekir ve Saniyedeki çerçeve sayısı altmış sunmamızı gerekebilir.

Ancak, görüntü sunucusu saniyede altmış kez üst sınır ekran güncelleştirmeleri gerçekleştirir. Bu nedenle, bu sınır daha hızlı ekran güncelleştirilmeye çalışılırken ekran hattının kaldırılması ve mikro takılmasını neden olabilir. Ekran güncelleştirmelerini görünen güncelleştirme ile eşitlenmesi için yapısı kodu en iyisidir. Bu kullanarak elde [ `CoreAnimation.CADisplayLink` ](https://developer.xamarin.com/api/type/CoreAnimation.CADisplayLink/) altmış görselleştirme için uygun bir süre ölçer sınıfı ve başından çalışan oyunlar Saniyedeki çerçeve.

<a name="avoidtransparency" />

## <a name="avoid-core-animation-transparency"></a>Çekirdek animasyon saydamlık kaçının

Çekirdek animasyon saydamlık önleme, bit eşlem birleştirme performansı artırır. Genel olarak, saydam katmanları ve bulanık Kenarlıklar mümkünse kaçının.

<a name="avoidcodegeneration" />

## <a name="avoid-code-generation"></a>Kod oluşturma kaçının

Dinamik olarak ile kod oluşturma `System.Reflection.Emit` veya *dinamik dil çalışma zamanı* iOS çekirdek dinamik kodu yürütme engellediğinden kaçınılması gerekir.

## <a name="summary"></a>Özet

Bu makalede açıklanan ve Xamarin.iOS ile oluşturulan uygulamaların performansını artırmak için teknikleri ele alınan. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası performansı](~/cross-platform/deploy-test/memory-perf-best-practices.md)
