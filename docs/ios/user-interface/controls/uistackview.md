---
title: Xamarin.iOS yığın görünümleri
description: Bu makalede, yatay veya dikey olarak düzenlenmiş bir yığın subviews her ikisinde kümesini yönetmek için yeni bir Xamarin.iOS uygulaması UIStackView denetiminde kullanmayı ele alır.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: a894efebe4089adefeb02007bd394c13fc77974c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790106"
---
# <a name="stack-views-in-xamarinios"></a>Xamarin.iOS yığın görünümleri

_Bu makalede, yatay veya dikey olarak düzenlenmiş bir yığın subviews her ikisinde kümesini yönetmek için yeni bir Xamarin.iOS uygulaması UIStackView denetiminde kullanmayı ele alır._

> [!IMPORTANT]
> Lütfen StackView iOS Tasarımcısı destekleniyorsa kullanılabilirlik hatalar kararlı kanal kullanırken karşılaşabileceğiniz olduğunu unutmayın. Geçiş Beta veya alfa kanal bu sorunu giderir. Gerekli düzeltmeleri kararlı kanalda uygulanana kadar Xcode kullanarak bu kılavuzda sunmak karar verdik.

Yığın Görünümü denetimi (`UIStackView`), iOS cihazını yönünü ve ekran boyutunu dinamik olarak yanıt güç otomatik düzeni ve boyutu sınıflarının subviews, yığınını yatay veya dikey olarak yönetmek için yararlanır.

Bir yığın görünüme bağlı tüm subviews düzenini eksen, dağıtım, hizalama ve aralığı gibi tanımlanmış Geliştirici özelliklerindeki göre tarafından yönetilir:

[![](uistackview-images/stacked01.png "Yığın düzeni diyagramı görüntüleme")](uistackview-images/stacked01.png#lightbox)

Kullanırken bir `UIStackView` bir Xamarin.iOS uygulaması geliştirici ya da iOS Tasarımcısı veya ekleme ve C# kodunda subviews kaldırarak film şeridi içinde subviews tanımlayabilirsiniz.

Bu belge iki bölümden oluşur: uygulama ilk yığın görüntülemek ve nasıl çalıştığı hakkında daha sonra bazı daha fazla teknik bilgi yardımcı olmak için hızlı bir başlangıç.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView, göre [Xamarin Üniversitesi](https://university.xamarin.com/)**

## <a name="uistackview-quickstart"></a>UIStackView hızlı başlangıç

Bir giriş olarak `UIStackView` denetimi olduğumuz 1'den bir derecelendirme 5 girmesini sağlayan basit bir arabirim oluşturmak için. Şu iki yığını görünümleri kullanma: bir arabirimde dikey cihazın ekran ve 1-5 derecelendirme simgeleri yatay ekranda Düzenle birine düzenlemek için.

### <a name="define-the-ui"></a>UI tanımla

Yeni bir Xamarin.iOS projesi başlatın ve düzenleme **Main.storyboard** dosyası Xcode'nın arabirimi Oluşturucu. İlk olarak, tek bir sürükleyin **dikey yığın görünümü** üzerinde **View Controller**:

[![](uistackview-images/quick01.png "Tek bir dikey yığın görünüm görünüm denetleyicisinde sürükleyin")](uistackview-images/quick01.png#lightbox)

İçinde **özniteliği denetçisi**, aşağıdaki seçenekleri belirleyin:

[![](uistackview-images/quick02.png "Yığın görünümü seçeneklerini ayarlama")](uistackview-images/quick02.png#lightbox)

Konum:

- **Eksen** – yığını görünüm subviews ya da düzenler varsa belirler **yatay** veya **dikey**.
- **Hizalama** – subviews yığın görünümü içinde nasıl hizalandığını denetler.
- **Dağıtım** – subviews yığın görünümde nasıl boyutlandırılır denetler.
- **Aralık** – yığını görünümünde her alt görünüm arasında en az alanı denetler.
- **Taban çizgisi göreli** – işaretlenirse, her alt görünüm dikey aralığını, taban çizgisinden elde edilir.
- **Düzen kenar boşlukları göreli** – subviews göre Standart Düzen kenar boşlukları yerleştirir.

Bir yığın görünümü ile çalışırken, düşünebilirsiniz **hizalama** olarak **X** ve **Y** alt görünüm konumunu ve **dağıtım** olarak **Yükseklik** ve **genişliği**.

> [!IMPORTANT]
> `UIStackView` tasarlanmış bir işleme olmayan kapsayıcı görünüm olarak ve bu nedenle, diğer alt sınıflarının gibi tuvale çizildiğinde değil `UIView`. Bu nedenle gibi özellikleri ayarlamak `BackgroundColor` veya geçersiz kılma `DrawRect` visual hiçbir etkisi olmaz.

Düzen, böylece aşağıdakine benzer bir etiket, ImageView, iki düğmeleri ve yatay bir yığın görünümü ekleyerek uygulamanın arabirimi devam edin:

[![](uistackview-images/quick03.png "Yığın görünümü kullanıcı arabirimini düzenleme")](uistackview-images/quick03.png#lightbox)

Yatay yığın görünümü ile aşağıdaki seçenekleri yapılandırın:

[![](uistackview-images/quick04.png "Yatay yığını görüntüleme seçeneklerini yapılandırın")](uistackview-images/quick04.png#lightbox)

Her "nokta" temsil eden simgeyi derecesi uzatılabilir için istemediğiniz çünkü bunu eklendiğinde yatay yığını görünümüne ayarlarız **hizalama** için **Center** ve  **Dağıtım** için **eşit doldurmak**.

Son olarak, aşağıdaki yukarı wire **çıkışlar** ve **Eylemler**:

[![](uistackview-images/quick05.png "Yığın Görünümü çıkışlar ve eylemleri")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Koddan UIStackView doldurma

Mac ve düzenleme için Visual Studio'ya dönmek **ViewController.cs** dosya ve aşağıdaki kodu ekleyin:

```csharp
public int Rating { get; set;} = 0;
...

partial void IncreaseRating (Foundation.NSObject sender) {

    // Maximum of 5 "stars"
    if (++Rating > 5 ) {
        // Abort
        Rating = 5;
        return;
    }

    // Create new rating icon and add it to stack
    var icon = new UIImageView (new UIImage("icon.png"));
    icon.ContentMode = UIViewContentMode.ScaleAspectFit;
    RatingView.AddArrangedSubview(icon);

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });

}

partial void DecreaseRating (Foundation.NSObject sender) {

    // Minimum of zero "stars"
    if (--Rating < 0) {
        // Abort
        Rating =0;
        return;
    }

    // Get the last subview added
    var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];

    // Remove from stack and screen
    RatingView.RemoveArrangedSubview(icon);
    icon.RemoveFromSuperview();

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });
}
```

Bir birkaç Bu kod parçalarını ayrıntılı olarak bakalım. İlk olarak, kullandığımız bir `if` deyimleri beşten fazla "yıldız" olmadığından emin olun ya da sıfırdan az.

Biz yükleme görüntüsünü yeni bir "yıldız" ekleyin ve ayarlayın, **içerik modu** için **ölçek en boy uygun**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Bu, "yıldız" simgesine yığını görünümüne eklendiğinde bozuk durumda tutar.

Ardından, biz yeni "yıldız" simgesine subviews yığın görünümün koleksiyona ekleyin:

```csharp
RatingView.AddArrangedSubview(icon);
```

Eklediğimiz fark edeceksiniz `UIImageView` için `UIStackView`'s `ArrangedSubviews` özelliği ve değil `SubView`. Düzenini denetlemek için yığın görünümü istediğiniz herhangi bir görünüm eklenmeli `ArrangedSubviews` özelliği.

Bir alt görünüm bir yığın görünümden kaldırmak için ilk biz kaldırmak için alt görünüm alın:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Hem de kaldırmak ihtiyacımız sonra `ArrangedSubviews` koleksiyonu ve Süper görünümü:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Bir alt görünüm gelen yalnızca kaldırma `ArrangedSubviews` koleksiyonu yığını görünümün denetimi dışında alır, ancak ekranından kaldırmaz.

### <a name="testing-the-ui"></a>UI testi

Tüm gerekli kullanıcı Arabirimi öğeleri ve koduyla yerinde, şimdi çalıştırma ve arabirim test etme. UI görüntülendiğinde, tüm öğeleri dikey yığın görünümünde eşit olarak üstten alta boşlukları.

Kullanıcı ne zaman dokunur **artırmak derecelendirme** düğmesi, başka bir "yıldız" (en çok 5) ekran eklenir:

[![](uistackview-images/intro01.png "Örnek uygulamayı çalıştırma")](uistackview-images/intro01.png#lightbox)

"Yıldız" otomatik olarak ortalanır ve yatay yığını görünümünde eşit olarak dağıtılır. Kullanıcı ne zaman dokunur **azaltmak derecelendirme** düğmesi, bir "yıldız" (hiçbiri bırakılır) kadar kaldırılır.

## <a name="stack-view-details"></a>Yığın ayrıntılarını görüntüleme

Biz ne genel bir fikir sahip olduğunuza `UIStackView` denetimi ve onu nasıl çalışır bazı özellikleri ve ayrıntılar daha derin bir göz atalım.

### <a name="auto-layout-and-size-classes"></a>Otomatik düzeni ve boyutu sınıfları

Yukarıdaki gördüğümüz gibi bir alt görünüm eklendiğinde yığını görünümüne düzenini tamamen, yığın konumu ve boyutu düzenlenen görünümler için otomatik düzeni ve boyutu sınıflarını kullanarak görünüm tarafından denetlenir.

Yığın Görünümü olacak _PIN_ ilk ve son alt görünüm, koleksiyondaki **üst** ve **alt** kenarları dikey yığın görünümler için veya **Sol**ve **sağ** kenarları yatay yığını görünümler için. Ayarlarsanız `LayoutMarginsRelativeArrangement` özelliğine `true`, görünüm ilgili kenar boşluklarına kenar yerine subviews sabitler sonra.

Yığın görünüm alt Görünüm'ın kullandığı `IntrinsicContentSize` tanımlı boyunca subviews boyutu hesaplanırken özelliği `Axis` (dışında `FillEqually Distribution`). `FillEqually Distribution` Aynı boyutta; böylece tüm subviews göre yeniden boyutlandırır böylece boyunca yığın görünümü doldurma `Axis`.

Dışında `Fill Alignment`, yığın görünüm alt Görünüm'ın kullandığı `IntrinsicContentSize` görünümün boyutu için dikey hesaplamak için özellik verilen `Axis`. İçin `Fill Alignment`, böylece dikeydir yığın görünümü doldurun tüm subviews boyutlandırılır verilen `Axis`.

### <a name="positioning-and-sizing-the-stack-view"></a>Konumlandırma ve yığın görünümü boyutlandırma

Yığın Görünümü tüm alt görünüm düzenini toplam denetime sahipken (özelliklerine gibi dayalı `Axis` ve `Distribution`), yine de yığın görünüm konumunu gerekiyorsa (`UIStackView`) otomatik düzeni ve boyutu sınıflarını kullanarak kendi üst görünümü içinde.

Genellikle, bu en az iki kenarları genişletin ve Sözleşme, böylece konumunu tanımlamak için yığın görünümünün sabitleme anlamına gelir. Başka kısıtlamalar, yığın görünümü otomatik olarak kendi subviews tümünün gibi uyacak şekilde yeniden boyutlandırılıp:

 - Boyutu boyunca kendi `Axis` tüm alt görünüm boyutları artı her alt görünüm arasında tanımlanan herhangi bir alanı toplamı olacaktır.
 - Varsa `LayoutMarginsRelativeArrangement` özelliği `true`, yığın görünümleri boyutu kenar boşlukları için yeriniz dahil edilir.
 - Dikeydir boyutu `Axis` koleksiyonunda en büyük alt görünüm için ayarlayın.

Ayrıca, yığın görünümün için kısıtlamalar belirtebilirsiniz **yükseklik** ve **genişliği**. Bu durumda, subviews (boyutlu) yığını tarafından belirlenen görünümü tarafından belirtilen alanı dolduracak şekilde düzenleneceğini `Distribution` ve `Alignment` özellikleri.

Varsa `BaselineRelativeArrangement` özelliği `true`, subviews kullanmak yerine ilk veya son alt görünüm ait temel göre düzenleneceğini **üst**, **alt** veya **Merkezi* -  **Y** konumu. Bunlar yığını Görünüm'ün içeriğine şöyle hesaplanmıştır:

 - Bir dikey yığın görünümü ilk taban çizgisi için ilk alt Görünüm ve son son döndürür. Bu subviews birini kendilerini yığın görünümler varsa, ilk veya son taban çizgisi kullanılır.
 - Yatay bir yığın görünümü en uzun kendi alt görünüm için hem bir ilk ve son temel kullanır. En uzun görünüm aynı zamanda bir yığın görünüm ise, en uzun alt görünüm taban çizgisi olarak kullanır.

> [!IMPORTANT]
> Taban çizgisi yanlış konuma hesaplanır gibi temel hizalama uzamış ya da sıkıştırılmış alt görünüm boyutlarına çalışmaz. Alt Görünüm'ın sağlamak için taban çizgisi hizalamasını, **yükseklik** iç içerik görünümün eşleşen **yükseklik**.

### <a name="common-stack-view-uses"></a>Ortak yığın görünümü kullanır

İyi yığın görünümü denetimleriyle çalışma birkaç yerleşim türleri vardır. Apple göre birkaç daha yaygın kullanımları şunlardır:

- **Boyut boyunca ekseni tanımlamak** – hem kenarları yığını görünümün boyunca sabitleme `Axis` ve bitişik kenarları görünümü büyümesine kendi subviews tarafından tanımlanan boşluğa uyacak şekilde ekseninde yığın konumu ayarlayın.
 -  **Alt Görünüm'ın konumunu tanımlamak** – yığını görünümün, üst bitişik kenarlarına sabitleme tarafından yığın görünümü onun içeren subviews sığması için her iki boyutlarında büyüyecektir.
- **Boyutunu ve konumunu yığınının tanımlamak** – yığını görünümün üst tüm dört kenarlarına sabitlemenin yığını görünüm yığın görünümü içinde tanımlanan alanı göre subviews düzenler.
 -  **Boyutu eksenine dik tanımlamak** – hem kenarları dikey olarak yığın görünümün sabitleme `Axis` ve bir görünüm büyümesine tarafından tanımlanan boşluğa uyacak şekilde eksenine dik yığın konumunu ayarlamak için ekseninde kenarlarının kendi subviews.

### <a name="managing-the-appearance"></a>Görünümü yönetme

`UIStackView` Tasarlanmıştır işleme olmayan kapsayıcı görünüm olarak ve bu nedenle, diğer alt sınıflarının gibi tuvale çizildiğinde değil `UIView`. Gibi özellikleri ayarlamak `BackgroundColor` veya geçersiz kılma `DrawRect` visual hiçbir etkisi olmaz.

Bir yığın görünümü subviews kendi koleksiyonunu nasıl düzenlenir denetleyen çeşitli özellikler şunlardır:

- **Eksen** – yığını görünüm subviews ya da düzenler varsa belirler **yatay** veya **dikey**.
- **Hizalama** – subviews yığın görünümü içinde nasıl hizalandığını denetler.
- **Dağıtım** – subviews yığın görünümde nasıl boyutlandırılır denetler.
- **Aralık** – yığını görünümünde her alt görünüm arasında en az alanı denetler.
- **Taban çizgisi göreli** – varsa `true`, her alt görünüm dikey aralığını, taban çizgisinden elde edilir.
- **Düzen kenar boşlukları göreli** – subviews göre Standart Düzen kenar boşlukları yerleştirir.

Tipik olarak az sayıda subviews düzenlemek için bir yığın görünümünü kullanın. Daha karmaşık kullanıcı arabirimleri, bir veya daha fazla yığın görünümlerini birbirine içinde iç içe geçme tarafından oluşturulabilir (biz yaptığınız gibi [UIStackView Quickstart](#UIStackView-Quickstart) yukarıda).

Daha fazla, ek kısıtlamalar (örneğin için Denetim genişlik ve yükseklik) subviews ekleyerek Uı'lar görünüm ince ayar yapabilirsiniz. Ancak, bu yığını görünüm tarafından kendisine sunulan çakışan kısıtlamaları içermeyecek şekilde dikkatli olunmalıdır.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Görünümler ve alt görünümleri tutarlılık koruma düzenlenmiş

Yığın Görünümü şunları sağlar, `ArrangedSubviews` özelliği olan her zaman bir alt kümesini kendi `Subviews` aşağıdaki kurallar kullanılarak özelliği:

 - Bir alt görünüm eklenirse `ArrangedSubviews` koleksiyonu, onu otomatik olarak eklenir `Subviews` koleksiyonu (zaten bu koleksiyonunun bir parçası değilse).
 - Bir alt görünüm kaldırılırsa `Subviews` (görüntüden kaldırılır) koleksiyon, bu da kaldırılır `ArrangedSubviews` koleksiyonu.
 - Gelen bir alt görünüm kaldırma `ArrangedSubviews` koleksiyonu ondan kaldırmaz `Subviews` koleksiyonu. Bu nedenle yığını görünüm tarafından artık düzenleneceğini, ancak hala ekranda görünür olacaktır.

`ArrangedSubviews` Koleksiyonudur her zaman bir alt kümesini `Subview` koleksiyonu, ancak ayrı ve aşağıdaki tarafından denetlenen her bir koleksiyonun içindeki tek tek subviews sırasını:

 - İçinde subviews sırasını `ArrangedSubviews` koleksiyonu belirlemeniz görüntü sıralarına yığın içinde.
 - İçinde subviews sırasını `Subview` ön görünümüne geri içinde koleksiyonu belirler kendi Z düzenini (veya katmanlama).

### <a name="dynamically-changing-content"></a>İçerik dinamik olarak değiştirme

Bir alt görünüm eklendiğinde, kaldırılan gizli veya her bir yığın görünümü subviews düzenini otomatik olarak ayarlayın. Yığın Görünümü herhangi bir özelliği ayarlanıyorsa düzenini de ayarlanacak (gibi kendi `Axis`).

Bir animasyon blokta örneğin girerek için bunları düzen değişiklikleri animasyonlu:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Yığın görünümün özelliklerin çoğu film şeridi içinde boyutu sınıfları kullanılarak belirtilebilir. Bu özellikler otomatik olarak animasyonlu boyutunu veya yönünü değişikliklere yanıt olan.

## <a name="summary"></a>Özet

Bu makalede yeni kapsamına `UIStackView` subviews bir Xamarin.iOS uygulaması içinde yatay veya dikey olarak düzenlenmiş yığınında kümesini yönetmek için (iOS 9) denetim.
Bir kullanıcı Arabirimi oluşturmak üzere yığını görünümleri kullanma basit bir örnek ile başlamıştır ve yığın görünümleri ve özelliklerini ve özellikleri daha ayrıntılı bir bakış ile tamamlandı.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [Yeni iOS 9.0 nedir](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Giriş UIStackView (video)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
