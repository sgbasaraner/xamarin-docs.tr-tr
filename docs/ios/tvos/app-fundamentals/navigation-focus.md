---
title: "Gezinti ve odak ile çalışma"
description: "Bu makalede kavramını odak ve sunmak ve gezinti Xamarin.tvOS uygulama içinde işlemek için nasıl kullanıldığı ele alınmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fe1358d330c2a0fd94016853cedeabe094c394da
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-navigation-and-focus"></a>Gezinti ve odak ile çalışma

_Bu makalede kavramını odak ve sunmak ve gezinti Xamarin.tvOS uygulama içinde işlemek için nasıl kullanıldığı ele alınmaktadır._


Bu makalede kavramı kapsayan [odak](#Focus-and-Selection) ve işlemek için nasıl kullanıldığı [Gezinti](#Navigation) Xamarin.tvOS uygulamanın kullanıcı arabiriminde. Biz, yerleşik tvOS Gezinti denetimlerinin odak, vurgulama ve seçimi Xamarin.tvOS uygulamanızın kullanıcı arabirimi Gezinti sağlamak için kullanma inceleyeceğiz.

[![](navigation-focus-images/intro01.png "tvOS uygulamalar kullanıcı arabirimi gezinme")](navigation-focus-images/intro01.png#lightbox)

Ardından, biz ile odak'ın nasıl kullanılabileceğini bir göz atalım [Parallax](#Focus-and-Parallax) ve *katmanlı görüntüleri* görsel ipuçları için son kullanıcı için geçerli gezinti durumu sağlamak için.

Son olarak, biz çalışmaya adresindeki göreceğiz [odak](#Working-with-Focus), [odak güncelleştirmeleri](#Working-with-Focus-Updates), [odak kılavuzları](#Working-with-Focus-Guides), [koleksiyonları odakta](#Working-with-Focus-in-Collections) ve [ Parallax etkinleştirme](#Enabling-Parallax) görüntü görünümleri Xamarin.tvOS uygulamalarınızdaki.

<a name="Navigation" />

## <a name="navigation"></a>Gezinme

Xamarin.tvOS uygulamanızdaki kullanıcıların değil etkileşim arabirimiyle burada bunlar dokunun cihazın ekran görüntülerinde ancak dolaylı olarak gelen arasında yer kullanarak, doğrudan iOS [Siri uzaktan](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote). Bu böylece doğal olarak akar henüz Apple TV deneyimi immersed kullanıcı tutar, uygulamanızın kullanıcı arabirimi tasarlarken göz önünde bulundurmanız gerekir.

Başarılı tvOS uygulama gezinti sorunsuz uygulamasının amacını ve gezinti dikkat çağırmadan gösterir verilerin yapısını destekler şekilde uygular. Böylece, kullanıcı arabirimi kapladığı veya içerik ve uygulamalar kullanıcı deneyimi çıktığınızda odak çizim olmadan doğal ve tanıdık hissi, gezinti tasarlayın.

[![](navigation-focus-images/nav01.png "TvOS ayarlarını uygulama")](navigation-focus-images/nav01.png#lightbox)

Kullanıcı bir Apple TV genellikle kullanarak ekranda, Yığılmış bir dizi ile her içerik belirli bir dizi sunan gider yapılırken. Buna karşılık, her yeni ekran bir veya daha fazla alt ekranlar gibi standart kullanıcı Arabirimi denetimlerini kullanarak içerik neden olabilir [düğmeleri](~/ios/tvos/user-interface/buttons.md), [sekmesini çubukları](~/ios/tvos/user-interface/tab-bars.md), tablolar, [koleksiyon görünümlerini](~/ios/tvos/user-interface/collection-views.md) veya [ Görünümleri bölme](~/ios/tvos/user-interface/split-views.md).

Veri ile her yeni ekran kullanıcıyı bu ekranlar yığına daha derin götürür. Kullanarak **menü** düğmesini Siri uzak bunlar geriye dönük bir önceki ekrana veya ana menüye dönmek için yığın aracılığıyla gidebilirsiniz.

Apple tvOS uygulamanız için Gezinti tasarlarken, aşağıdakileri göz önünde bulundurarak önerir:

- **Düzen, gezinti hızlı içerik bulma yapmak ve kolay** -dokunma, en az sayıda uygulamanızda içinde içeriğe erişmek için kullanıcıların istediğiniz tıklar ve mümkün olduğunca swipes. Gezinti basitleştirmek ve içerik ekranlar en az sayıda düzenleyebilirsiniz.
- **Kullanarak Touch bir sıvı arabirimi oluşturma** -kullanıcı arasında taşıyabilirsiniz olun _odaklanabilir öğeleri_ hareketleri olası en az sayıda kullanarak en az uyuşmazlık ile.
- **Tasarım aklınızda Odaklanılan** -kullanıcı arasında yer içeriği ile etkileşim olduğundan, bunlar Siri uzaktan kullanarak onunla etkileşimde önce bir kullanıcı arabirimi öğesine odak taşımanız gerekir. Bunları hedeflerine ulaşması için çok fazla hareketlerini gerekiyorsa kullanıcılar ile uygulamanızı engellenen alacak.
- **Geriye dönük gezinti menüsü düğmesiyle sağlamak** - oluşturmak kolay ve tanıdık bir deneyim izin vermek için geriye dönük Siri uzak bilgisayarın kullanarak gitmek için kullanıcıların **menü** düğmesi. Tuşuna basarak **menü** düğmesi her zaman önceki ekrana dönün veya uygulamanın ana menüye dönmek. Uygulamanın en üstünde düzeyi tuşuna basarak **menü** düğmesi Apple TV giriş ekranına döndürmelidir.
- **Genellikle bir geri düğmesini görüntüleme kaçının** - tuşuna basarak **menü** Siri uzak düğmesi gider geriye doğru ekran yığınından, bu davranış çoğaltan ek bir denetimi görüntüleme kaçının. Ekranlar veya ekranlar (gibi içerik silme) bozucu Eylemler ile satın aldığınız için bu kural için bir özel durumdur burada bir **iptal** düğmesini de görüntülenecek.
- **Birçok yerine tek bir ekran üzerinde büyük topluluklara Göster** -Siri uzaktan hızlı içeriğin büyük bir toplulukta taşıma ve kolay hareketleri kullanarak yapmak için tasarlanmıştır. Uygulamanızı büyük odaklanabilir öğeleri koleksiyonu ile çalışıyorsa, bunları kullanıcının bölümü hakkında daha fazla gezinti gerektiren birçok ekranlar parçalamak yerine tek bir ekran üzerinde tutmaya dikkat edin.
- **Gezinti için standart denetimleri kullanın** -yeniden, mümkün olduğunda bir kolay ve tanıdık bir kullanıcı deneyimi oluşturmak için yerleşik kullanın `UIKit` sayfa denetimleri, sekme çubukları, bölümlenmiş denetimleri, tablo görünümleri, koleksiyon görünümlerini ve bölünmüş gibi denetimleri Görünümler, uygulamanızın gezinti için. Kullanıcı zaten bu öğelerle tanıdık, bunlar sezgisel uygulamanızı gidin mümkün olur.
- **Yatay içerik gezinti favor** -soldan sağa Siri uzak geçirmeyi Apple TV yapısı nedeniyle, yukarı ve aşağı daha fazla doğal. Bu seçenek, uygulamanız için içerik düzenleri tasarlarken göz önünde bulundurun.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>Odak ve seçimi

Apple TV bir görüntü, düğme veya diğer kullanıcı Arabirimi öğesi olarak değerlendirilir _odakta_ geçerli Gezinti hedef olduğunda.

[![](navigation-focus-images/focus01.png "Odak ve seçim örneği")](navigation-focus-images/focus01.png#lightbox)

Aksine, burada kullanıcı doğrudan etkileşim aygıtın dokunmatik öğelerde ile iOS cihazlarının kullanıcıları tvOS öğelerinden Siri uzaktan kullanarak yer arasında etkileşime. Apple TV sunmak ve bu kullanıcı etkileşimi işlemek için kullandığı bir _odak_ model tabanlı.

Hareketleri ve düğmesini kullanarak bastığında [Siri uzaktan](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), odağı kullanıcı bir kullanıcı Arabirimi öğesinden diğerine taşır. Odağı istenen öğesine değişme sonra istediğiniz içeriği seçin veya bu öğe tarafından temsil edilen eylemini etkinleştirmek için kullanıcı tıklatır.

Odak değiştikçe Zarif animasyonları ve etkileri (örneğin, görüntüleri Parallax etkisi) odağa sahip kullanıcı arabirimi öğesi açıkça tanımlamak için kullanılır.

Apple odak ve seçim ile çalışmak için aşağıdaki önerileri vardır:

- **Hareket efektler için yerleşik kullanıcı Arabirimi denetimlerini kullanmak** - kullanarak `UIKit` ve odak API odak modeli, kullanıcı arabiriminde otomatik olarak varsayılan hareket ve görsel efektler, kullanıcı Arabirimi öğelerine uygulanır. Bu yerel ve Apple TV platform kullanıcılarının tanıdık eşitleyerek uygulamanızı kolaylaştırır ve esnektir ve sezgisel taşıma odaklanabilir öğeleri arasında olanak sağlar.
- **Odağı beklenen yönde taşıma** -üzerinde Apple TV, neredeyse her öğe dolaylı işleme kullanır. Örneğin, kullanıcı odağı taşımak için Siri uzaktan kullanır ve sistemin o anda odaklanılan öğeyi görünür durumda tutmak için arabirimi otomatik olarak kayar. Uygulamanız bu tür etkileşim uygularsa, odak kullanıcı hareketi yönde hareket etmesini sağlamak. Bu nedenle kullanıcı sağa doğru çekin, Siri uzaktan odak sağındaki (hangi sola kaydırma için ekranın neden olabilecek) taşımanız gerekir. Bu kural için bir özel doğrudan düzenlemesi (burada yukarı geçirmeyi öğeyi yukarı taşır) kullanan tam ekran öğeler ' dir.
- **Odaklanmış öğesi Obvious olduğundan emin olun** -kullanıcılarınızın itibaren afar, kullanıcı Arabirimi öğeleri ile etkileşim çünkü o anda odaklanılan öğeyi öne çıkması, kritik öneme sahiptir. Genellikle bu otomatik olarak yerleşik tarafından işlenecek `UIKit` öğeleri. Özel denetimler için madde boyutu veya gölge gibi özellikleri odak göstermek için kullanın.
- **Parallax olun odaklanmış öğeleri yanıt için kullanmak** - küçük, döngüsel hareketleri Siri uzak neden size, gerçek zamanlı odaklanmış öğesinin hareket. Bu [Parallax etkisi](#Focus-and-Parallax) içinde yerleşik `UIKit` _katmanlı görüntüleri_ kullanıcı odaklı öğesine bağlantının bir fikir vermek için.
- **Uygun boyutta odaklanabilir öğeleri oluşturma** -geniş aralığı büyük öğeleri seçin ve küçük öğeler gezinmek daha kolay.
- **Kullanıcı Arabirimi öğesi Ara iyi da odaklanmış veya Unfocused tasarım** -genellikle Apple TV boyutunu artırarak odaklanmış öğeyi temsil eder. Uygulamalarınızı ait kullanıcı Arabirimi öğeleri her sunu boyutta görünümlü gerekirse, varlıkları büyük boyutlu öğeleri için tedarik, emin olup.
- **Odak değişiklikleri sorunsuzca temsil** -animasyon öğeleri arasında yavaşça kullanmasını **Focused** ve **Unfocused** olma geçişler jarring tutmak için durumu.
- **Bir imleç gösterme** -kullanıcıların beklediğiniz odak kullanarak uygulamanızın UI gidin ve ekran imleci taşıyarak değil. Kullanıcı Arabiriminizin odak modeli, tutarlı bir kullanıcı deneyimi sunmak için her zaman kullanmalısınız.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>Odak ile çalışma

Odaklanabilir öğesi olabilmesi için özel bir denetim oluşturmak istediğiniz zamanlar olabilir. Bu nedenle geçersiz kılma `CanBecomeFocused` özelliği ve return `true`, aksi takdirde dönüş `false`. Örneğin:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

Herhangi bir zamanda kullandığınız `Focused` özelliği bir `UIKit` geçerli öğe olup olmadığını görmek için denetim. Varsa `true` kullanıcı Arabirimi öğesi odağa sahip, aksi takdirde yok. Örneğin:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

Doğrudan odak başka bir kullanıcı Arabirimi öğesine kodu aracılığıyla taşıyamazsınız olsa da, ekran ayarlayarak yüklendiğinde hangi kullanıcı Arabirimi öğesi ilk odak alır belirtebilirsiniz, `PreferredFocusedView` özelliğine `true`. Örneğin:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>Odağı Güncelleştirmeler ile çalışma 

Kullanıcı bir kullanıcı Arabirimi öğesinden diğerine (örneğin, bir hareketi Siri uzak yanıta) kaydırmak odak sebep olduğunda bir _odak güncelleştirme olay_ odağı kaybeden öğesi ve odak sağlamasını öğesi için gönderilir.

Devralınan özel öğeleri için `UIView` veya `UIViewController`, odak güncelleştirme olay ile çalışmak için birkaç yöntem geçersiz kılabilirsiniz:

- **DidUpdateFocus** -görünümü kazanır veya odağı kaybettiğinde dilediğiniz zaman bu yöntem çağrılır.
- **ShouldUpdateFocus** -taşımak için odak burada izin tanımlamak için bu yöntemi kullanın.

Odağı altyapısı hareket etmesini istemek için odak başa `PreferredFocusedView` kullanıcı Arabirimi öğesi, çağrı `SetNeedsUpdateFocus` görünüm denetleyicisini yöntemi.

> [!IMPORTANT]
> Çağırma `SetNeedsUpdateFocus` çağırıldığında karşı View Controller odağa sahip görünümü içeriyorsa, yalnızca bir etkisi olmaz.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>Odağı kılavuzları ile çalışma

Odak tvOS yerleşik bir yatay ve dikey kılavuz kalan öğeleri arasında hareketleri işleme sırasında harika altyapısıdır. Genellikle, hiçbir şey yapma birden fazla kullanıcı Arabirimi öğeleri arabirimi tasarımınızı ve odak altyapısı Geliştirici müdahalesi olmadan bu öğeler arasında hareket otomatik olarak işleyecek eklemek gerekir.

Ancak, zamanlar olabilir, UI tasarımınızı necessities nedeniyle kullanıcı Arabirimi öğeleri üzerinde yatay ve dikey kılavuz kalan yok ve diğer için çapraz olduklarından erişilemeyebilir. Odağı altyapısı aşağı, sağa ve sola hareket yalnızca kullanıcı Arabirimi öğeleri arasında basit işlemek üzere tasarlandığından meydana gelir.

Bir örnek için aşağıdaki UI düzeni alın:

 [![](navigation-focus-images/guide01.png "Odağı kılavuzları örnek ile çalışma")](navigation-focus-images/guide01.png#lightbox)
 
Çünkü **daha fazla bilgi** düğmesi ile yatay ve dikey kılavuz kalan değil **satın** düğmesi, olur kullanıcıya erişilemez. Ancak, bu kolayca kullanılarak düzeltilebilir bir _odak Kılavuzu_ taşıma ipuçları için odak altyapısı sağlamak için. 

Odağı Kılavuzu (`UIFocusGuide`) görünür olmayan bir alanı görünümünün Focusable böylece başka bir görünüme yönlendirilecek odağı sağlar odak altyapısı olarak kullanıma sunar.

Bu nedenle, yukarıda verilen örnek çözmek için aşağıdaki kodu arasında odak kılavuz oluşturmak için Görünüm denetleyicisini eklenemedi **daha fazla bilgi** ve **satın** düğmeleri:

```csharp
public UIFocusGuide FocusGuide = new UIFocusGuide ();
...

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Add Focus Guide to layout
    View.AddLayoutGuide (FocusGuide);

    // Define Focus Guide that will allow the user to move
    // between the More Info and Buy buttons.
    FocusGuide.LeftAnchor.ConstraintEqualTo (BuyButton.LeftAnchor).Active = true;
    FocusGuide.TopAnchor.ConstraintEqualTo (MoreInfoButton.TopAnchor).Active = true;
    FocusGuide.WidthAnchor.ConstraintEqualTo (BuyButton.WidthAnchor).Active = true;
    FocusGuide.HeightAnchor.ConstraintEqualTo (MoreInfoButton.HeightAnchor).Active = true;
}
```

İlk olarak, yeni bir `UIFocusGuide` oluşturulur ve görünümün Düzen kılavuzu koleksiyonu kullanarak eklenir `AddLayoutGuide` yöntemi.

Ardından, odak kılavuz üst, sol, genişlik ve yükseklik bağlayıcılarını göreli olarak ayarlanır **daha fazla bilgi** ve **satın** aralarında konumlandırmak için düğmeler. Bkz.

[![](navigation-focus-images/guide02.png "Örnek odak Kılavuzu")](navigation-focus-images/guide02.png#lightbox)

Ayarlayarak oluşturuldukları sırada yeni kısıtlamaları etkinleştirilmekte dikkate almak önemlidir kendi `Active` özelliğine `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

Yeni odak kurulmuş ve görünümün için Görünüm denetleyicisini eklenen Kılavuzu ile `DidUpdateFocus` yöntemi geçersiz ve aşağıdaki kodu eklenen arasında hareket etmek için **daha fazla bilgi** ve **satın** düğmeleri:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    base.DidUpdateFocus (context, coordinator);

    // Get next focusable item from context
    var nextFocusableItem = context.NextFocusedView;

    // Anything to process?
    if (nextFocusableItem == null) return;

    // Decide the next focusable item based on the current
    // item with focus
    if (nextFocusableItem == MoreInfoButton) {
        // Move from the More Info to Buy button
        FocusGuide.PreferredFocusedView = BuyButton;
    } else if (nextFocusableItem == BuyButton) {
        // Move from the Buy to the More Info button
        FocusGuide.PreferredFocusedView = MoreInfoButton;
    } else {
        // No valid move
        FocusGuide.PreferredFocusedView = null;
    }
}
```

İlk olarak, bu kod get's `NextFocusedView` gelen `UIFocusUpdateContext` içinde geçirilmiş (`context`). Bu görünüm ise `null`, ardından hiçbir işlem gerekli değildir ve yönteminden çıkıldı.

İleri `nextFocusableItem` değerlendirilir. Ya da eşleşirse **daha fazla bilgi** veya **satın** düğmeleri, odak odak kılavuz kullanarak ters düğmesi gönderilir `PreferredFocusedView` özelliği. Örneğin:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

Hiçbiri düğmesi odak shift kaynağıdır gerektiğinde, `PreferredFocusedView` özelliği temizlendiğinde:

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>Koleksiyonları odakta ile çalışma

Ayrı bir öğe içinde odaklanabilir olabilir olup olmadığına karar verirken bir `UICollectionView` veya `UITableView`, yöntemleri geçersiz kılmak `UICollectionViewDelegate` veya `UITableViewDelegate` sırasıyla. Örneğin:

```csharp
public class CardHandDelegate : UICollectionViewDelegateFlowLayout
{
    ...
    public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
    {
        if (indexPath == null) {
            return false;
        } else {
            var controller = collectionView as CardHandViewController;
            return !controller.Hand [indexPath.Row].IsFaceDown;
        }
    }
}
```

`CanFocusItem` Yöntemi döndürür `true` geçerli öğe odakta olabilir ise başka döndürür `false`.

İsterseniz bir `UICollectionView` veya `UITableView` unutmayın ve kaybeder ve yeniden kazanabilmesi odak odak son öğe geri yüklemek için ayarlamak `RemembersLastFocusedIndexPath` özelliğine `true`.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>Odak ve Parallax

Yukarıda belirtildiği gibi bir kullanıcı arabirimi öğesi olarak kabul edilir _odakta_ olduğu zaman geçerli öğe Gezinti olayı sırasında. Genellikle bir öğe odağı geldiğinde, boyutu biraz artar ve görsel olarak gölge kullanarak yükseltilmeden. 

Kullanıcı Siri uzak yavaş, döngüsel bir hareketi yaparsa odaklanmış öğesi bu taşıma yanıta gerçek zamanlı sway. Sway gerçekleştiği sırada aydınlatılmış bir görünüm görünmek görünmesini yüzeyini yapmadan görüntüsünü uygulanır. Verilen bir miktar kaldıktan sonra herhangi bir odak içerik karartır ve Focused öğesi daha da büyük büyüyecektir. 

Bu etkilerle TV ekran içeriğini ve kanepenizde Apple TV ile etkileşim kullanıcı arasında zihinsel bir bağlantı sunmak için birlikte çalışır.

Apple TV bu Parallax etkiyi sistem genelinde bir fikir 3B derinliği ve odak öğeleri için dynamics iletmek için kullanılır. tvOS kullanan bir dizi saydam [katmanlı görüntüleri](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) taşır ve bu efekti oluşturmak için dinamik olarak ölçeklendirir.

Katmanlı görüntüleri tvOS uygulamanızın simgesi için gereklidir ve dinamik üst raf içerik için desteklenir. Gerekli olmasa da, Apple, uygulamanızın kullanıcı arabiriminde odaklanabilir diğer öğeleri görüntüleri katmanlı uygulama önerir.

### <a name="enabling-parallax"></a>Parallax etkinleştirme

`UIImageView` Denetimi (veya devraldığı herhangi bir denetimi `UIImageView`) otomatik olarak Parallax etkisi destekler. Varsayılan olarak, bu destek, devre dışı bırakılır etkinleştirmek için aşağıdaki kodu kullanın:

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

Bu özelliği ile `true`, kullanıcı tarafından ve odak seçildiğinde resim görünümü Parallax etkisi otomatik olarak alır.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede kavramını odak ve Xamarin.tvOS uygulamanın kullanıcı arabiriminde gezinme işlemek için nasıl kullanıldığı ele. Bunu inceleyin nasıl yerleşik tvOS Gezinti denetimlerinin odak, vurgulama ve seçimi Gezinti sağlamak için kullanın. Ardından, nasıl odak Parallax ve katmanlı görüntüleri ile görsel ipuçları için son kullanıcı için geçerli gezinti durumu sağlamak için kullanılabilir Aranan. Son olarak, çalışma odak, odak güncelleştirmeleri, koleksiyonlar ve etkinleştirme Parallax odakta incelendi.




## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
