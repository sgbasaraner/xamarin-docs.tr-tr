---
title: Xamarin.iOS yığın görünümleri
description: Bu makale, yatay veya dikey olarak düzenlenmiş bir yığın içerisinde subviews kümesini yönetmek için yeni bir Xamarin.iOS uygulaması UIStackView denetiminde kullanma kapsar.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: a894efebe4089adefeb02007bd394c13fc77974c
ms.sourcegitcommit: 213b0315f1d6d0791e255794f87512fb253c492f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "34790106"
---
# <a name="stack-views-in-xamarinios"></a>Xamarin.iOS yığın görünümleri

_Bu makale, yatay veya dikey olarak düzenlenmiş bir yığın içerisinde subviews kümesini yönetmek için yeni bir Xamarin.iOS uygulaması UIStackView denetiminde kullanma kapsar._

> [!IMPORTANT]
> İOS Designer'daki StackView desteklenir, ancak kullanılabilirlik hataları kararlı kanal kullanırken karşılaşabileceğiniz olduğunu lütfen unutmayın. Geçiş Beta veya alfa kanalları bu sorunu giderir. Gerekli düzeltmeleri, kararlı kanalda uygulanana kadar Xcode kullanarak bu izlenecek yol sunmak üzere verdik.

Yığın Görünümü denetimi (`UIStackView`) dinamik olarak iOS cihazın yönüne ve ekran boyutuna yanıt veren gücünü Otomatik Yerleşim ve boyut sınıfları subviews, yığınını yatay veya dikey olarak yönetmek için yararlanır.

Yığın görünümü bağlı tüm subviews düzenini eksen, dağıtım, hizalama ve boşluk gibi geliştirici tanımlanan özelliklere göre tarafından yönetilir:

[![](uistackview-images/stacked01.png "Yığın Görünümü yerleşim diyagramı")](uistackview-images/stacked01.png#lightbox)

Kullanırken bir `UIStackView` bir Xamarin.iOS uygulaması geliştirici ya da iOS Designer'daki ya da ekleme ve C# kodunda subviews kaldırarak bir film şeridi içinde subviews tanımlayabilirsiniz.

Bu belge iki bölümden oluşur: uygulamanız ilk yığınınızı görüntülemek ve nasıl çalıştığı hakkında daha sonra bazı daha fazla teknik bilgi yardımcı olmak için hızlı bir başlangıç.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**UIStackView tarafından [Xamarin University](https://university.xamarin.com/)**

## <a name="uistackview-quickstart"></a>UIStackView hızlı başlangıç

Hızlı bir giriş olarak `UIStackView` denetimi çalışacağız derecelendirmesi 1. 5'e girin. kullanıcıya izin veren basit bir arabirim oluşturmak için. İki yığın görünümü kullanacağız: dikey olarak cihazın ekran ve 1-5 derecelendirme simgeleri yatay ekranda yerleştirme için bir arabirimi düzenlemek için.

### <a name="define-the-ui"></a>Kullanıcı arabirimini tanımlayın

Yeni bir Xamarin.iOS projesi başlatın ve düzenleme **Main.storyboard** Xcode'un arabirimi oluşturucu dosyası. İlk olarak, tek bir sürükleme **dikey yığın görünümü** üzerinde **görünüm denetleyicisi**:

[![](uistackview-images/quick01.png "Görünüm denetleyicisi üzerinde tek bir dikey yığın görünümü sürükleyin")](uistackview-images/quick01.png#lightbox)

İçinde **özniteliği denetçisi**, aşağıdaki seçenekleri belirleyin:

[![](uistackview-images/quick02.png "Yığın görünümü seçeneklerini ayarlama")](uistackview-images/quick02.png#lightbox)

Konum:

- **Eksen** – yığın görünümü subviews ya da düzenler, belirler **yatay** veya **dikey**.
- **Hizalama** – subviews yığın görünümü içinde nasıl hizalandığını denetler.
- **Dağıtım** – subviews yığın görünümü içinde nasıl boyutlandırılır denetler.
- **Aralık** – Stack görünümünde her alt görünüm arasında en az alanı denetler.
- **Taban çizgisi göreli** – işaretlediyseniz, her alt görünüm dikey aralığını kendi taban çizgisinden elde edilir.
- **Yerleşim kenar boşlukları göreli** – subviews Standart Düzen kenar boşluklarını göre yerleştirir.

Bir yığın görünümü ile çalışırken, kadar aklınıza gelebilecek **hizalama** olarak **X** ve **Y** alt görünüm konumunu ve **dağıtım** olarak **Yükseklik** ve **genişliği**.

> [!IMPORTANT]
> `UIStackView` tasarlanmıştır işleme olmayan kapsayıcı bir görünüm olarak ve bu nedenle diğer alt sınıflarından birini gibi tuvale çizildiğinde değil `UIView`. Bu nedenle gibi özellikleri ayarlamak `BackgroundColor` veya geçersiz kılma `DrawRect` visual hiçbir etkisi olmaz.

Düzen, aşağıdakine benzer olacak şekilde bir etiket, ImageView, iki düğme ve yatay bir yığın görünümü ekleyerek uygulamanın arabirimi devam edin:

[![](uistackview-images/quick03.png "Yığın görünümü kullanıcı arabirimini düzenleme")](uistackview-images/quick03.png#lightbox)

Yatay yığın görünümü, aşağıdaki seçenekleri yapılandırın:

[![](uistackview-images/quick04.png "Yatay yığın görüntüleme seçeneklerini yapılandırma")](uistackview-images/quick04.png#lightbox)

Her "nokta" temsil eden simgeyi derecesi uzatılmış şekilde istemediğiniz için bu eklendiğinde yatay yığın görünümüne biz bildirimler'i **hizalama** için **Merkezi** ve  **Dağıtım** için **eşit dolgu**.

Son olarak, aşağıdaki yukarı wire **çıkışlar** ve **Eylemler**:

[![](uistackview-images/quick05.png "Yığın Görünümü çıkışlar ve Eylemler")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Koddan bir UIStackView Doldur

Mac ve düzenleme için Visual Studio'ya geri dönün **ViewController.cs** dosyasını açıp aşağıdaki kodu ekleyin:

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

Bir birkaç Bu kod parçaları ayrıntılı olarak bakalım. İlk olarak kullandığımız bir `if` deyimleri beşten fazla "yıldız" olmadığından emin olun veya sıfırdan küçüktür.

Resmi yüklediğimiz yeni bir "yıldız" ekleyin ve kendi **içerik modu** için **ölçek uygun en boy**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Bu, "yıldız" simgesini yığın görünümüne eklendiğinde bozuk durumda tutar.

Ardından, yeni "yıldız" simgesini subviews yığın görünümün koleksiyonuna ekliyoruz:

```csharp
RatingView.AddArrangedSubview(icon);
```

Eklediğimiz fark edeceksiniz `UIImageView` için `UIStackView`'s `ArrangedSubviews` özelliği ve değil `SubView`. Yığın Görünümü yerleşimini denetlemek için istediğiniz herhangi bir görünüm eklenmeli `ArrangedSubviews` özelliği.

Bir alt görünüm yığını görünümden kaldırmak için önce kaldırmak için alt görünüm aldığımız:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

Sonra da her ikisini de kaldırmak ihtiyacımız `ArrangedSubviews` koleksiyonu ve Süper görünümü:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Gelen yalnızca bir alt görünüm kaldırma `ArrangedSubviews` koleksiyonu için yığın görünümün denetimi dışında alır, ancak bu ekrandan kaldırmaz.

### <a name="testing-the-ui"></a>UI testi

Tüm gerekli kullanıcı Arabirimi öğeleri ve kod yerinde ile artık çalıştırabilir ve test arabirimi. Kullanıcı Arabiriminde görüntülendiğinde, tüm öğeleri dikey Yığın Görünümü'nde eşit üstten alta boşlukları.

Kullanıcı ne zaman dokunduğunda **artırmak derecelendirme** düğmesi, başka bir "yıldız" (en çok 5) ekran eklenir:

[![](uistackview-images/intro01.png "Örnek uygulama çalıştırma")](uistackview-images/intro01.png#lightbox)

"Yıldız" otomatik olarak ortalamak ve yatay Yığın Görünümü'nde eşit olarak dağıtılır. Kullanıcı ne zaman dokunduğunda **azaltmak derecelendirme** düğmesi, bir "yıldız" (hiçbiri bırakılır kadar) kaldırılır.

## <a name="stack-view-details"></a>Yığın ayrıntılarını görüntüle

Biz genel bir fikir ne olduğuna göre `UIStackView` denetimdir ve nasıl çalıştığını, bazı özellikler ve ayrıntılar daha kapsamlı bir göz atalım.

### <a name="auto-layout-and-size-classes"></a>Otomatik Yerleşim ve boyut sınıfları

Yukarıda gördüğümüz gibi bir alt görünüm eklendiğinde bir yığın görünümüne düzenini tamamen, yığın konumlandırabilir ve düzenlenmiş görünümleri boyut otomatik yerleşim ve boyut sınıflarını kullanarak görünüm tarafından denetlenir.

Yığın Görünümü olacak _PIN_ , koleksiyondaki ilk ve son alt Görünüm **üst** ve **alt** dikey yığın görünümleri için kenarlar veya **Sol**ve **sağ** kenarlar yatay yığın görünümler için. Ayarlarsanız `LayoutMarginsRelativeArrangement` özelliğini `true`, görünüm için edge yerine ilgili kenar boşluklarını subviews sabitler sonra.

Yığın Görünümü alt Görünüm'ın kullandığı `IntrinsicContentSize` subviews boyutu tanımlı boyunca hesaplarken özelliği `Axis` (dışında `FillEqually Distribution`). `FillEqually Distribution` Aynı boyutta olmasını sağlamak tüm subviews yeniden boyutlandırır. Bu nedenle boyunca yığın görünümü doldurma `Axis`.

Dışında `Fill Alignment`, yığın görünümü alt Görünüm'ın kullandığı `IntrinsicContentSize` özelliği için dikey görünümün boyutunu hesaplamak için verilen `Axis`. İçin `Fill Alignment`, bunlar için dikey yığın görünümü doldurun, böylece tüm subviews boyutlandırılır verilen `Axis`.

### <a name="positioning-and-sizing-the-stack-view"></a>Yığın Görünümü boyutlandırma ve konumlandırma

Yığın Görünümü tüm alt görünüm yerleşimini üzerinde tam denetim sahibi sahipken (özelliklerine gibi bağlı `Axis` ve `Distribution`), yığın görünümü konumlandırmak yine (`UIStackView`) otomatik yerleşim ve boyut sınıflarını kullanarak kendi üst görünümü içinde.

Genellikle, bu sözleşme, bu nedenle konumuna tanımlama ve genişletmek için yığın görünümü en az iki kenarlarına sabitleme anlamına gelir. Herhangi bir ek kısıtlamaları olmadan yığın görünümü tüm kendi subviews şu şekilde sığacak şekilde otomatik olarak boyutlandırılıp:

 - Boyutu boyunca kendi `Axis` yanı sıra her alt görünüm arasında tanımlanan herhangi bir alanı tüm alt görünüm boyutlarının toplamı olacaktır.
 - Varsa `LayoutMarginsRelativeArrangement` özelliği `true`, yığın görünümleri boyutu kenar boşluklarını yer de içerir.
 - Boyutu için dikey `Axis` koleksiyonundaki büyük alt görünüm ayarlanır.

Buna ek olarak, Stack görünümün için kısıtlamaları belirtebilirsiniz **yükseklik** ve **genişliği**. Bu durumda, subviews (boyutlu) tarafından belirlenen şekilde bir yığın görünümü tarafından belirtilen alanı dolduracak şekilde düzenleneceğini `Distribution` ve `Alignment` özellikleri.

Varsa `BaselineRelativeArrangement` özelliği `true`, subviews kullanmak yerine ilk veya son alt görünüm ait temel göre düzenleneceğini **üst**, **alt** veya **Merkezi** -  **Y** konumu. Bu yığın görünümün içeriği şu şekilde hesaplanır:

 - Dikey bir yığın görünümü ilk taban çizgisi için ilk alt Görünüm ve son son döndürür. Ya da bu subviews kendilerini yığın görünümler varsa, ilk veya son taban çizgisi kullanılır.
 - Yatay bir yığın görünümü, en uzun alt görünüm için hem bir ilk ve son temel kullanır. En uzun görünüm aynı zamanda bir yığın görünümü ise, en uzun alt görünüm temeli olarak kullanır.

> [!IMPORTANT]
> Taban çizgisi yanlış yere hesaplanır gibi temel hizalama esnetilen veya sıkıştırılmış alt görünüm boyutlarına çalışmıyor. Temel hizalama için alt Görünüm'ın emin **yükseklik** iç içerik görünümün eşleşen **yükseklik**.

### <a name="common-stack-view-uses"></a>Ortak yığın görünümü kullanır

İyi yığın görünümü denetimleriyle çalışmak birçok düzen türü vardır. Apple'nın göre birkaç daha yaygın kullanımları şunlardır:

- **Boyutu boyunca eksen tanımlamak** – Stack görünümün boyunca iki kenara sabitleme `Axis` ve bir görünümü büyütün, subviews tarafından tanımlanan alana uyacak şekilde ekseni boyunca yığın konumunu ayarlamak için bitişik kenarlarının.
 -  **Alt Görünüm'ün konumunu tanımlamak** – yığın görünümü kendi üst görünümü bitişik kenarlarına sabitleyerek yığın görünümü onun içeren subviews sığdırmak için her iki boyutu büyüyecektir.
- **Yığın konumunu ve boyutunu tanımlamak** – yığın görünümü üst görünümdeki tüm dört kenarlarına sabitleyerek yığın görünümü subviews yığın görünümü içinde tanımlanan alanına göre düzenler.
 -  **Boyut dikey eksen tanımlamak** – hem de kenarlar dikey yığın görünümün olarak sabitleme `Axis` ve kenarlar ekseni görünümü büyüme tarafından tanımlanan alana uyacak şekilde eksenine dik yığın konumunu ayarlamak için subviews.

### <a name="managing-the-appearance"></a>Görünümü yönetme

`UIStackView` Tasarlanmıştır işleme olmayan kapsayıcı bir görünüm olarak ve bu nedenle diğer alt sınıflarından birini gibi tuvale çizildiğinde değil `UIView`. Özellikleri gibi ayarlama `BackgroundColor` veya geçersiz kılma `DrawRect` visual hiçbir etkisi olmaz.

Yığın Görünümü subviews kendi koleksiyonunu nasıl düzenlenir denetleyen birkaç özellik vardır:

- **Eksen** – yığın görünümü subviews ya da düzenler, belirler **yatay** veya **dikey**.
- **Hizalama** – subviews yığın görünümü içinde nasıl hizalandığını denetler.
- **Dağıtım** – subviews yığın görünümü içinde nasıl boyutlandırılır denetler.
- **Aralık** – Stack görünümünde her alt görünüm arasında en az alanı denetler.
- **Taban çizgisi göreli** – `true`, her alt görünüm dikey aralığını kendi taban çizgisinden elde edilir.
- **Yerleşim kenar boşlukları göreli** – subviews Standart Düzen kenar boşluklarını göre yerleştirir.

Genellikle az sayıda subviews düzenlemek için bir yığın görünümü kullanır. Bir veya daha fazla yığın görünümlerini birbirine içinde iç içe geçirerek daha karmaşık kullanıcı arabirimleri oluşturulabilir (yaptığımız gibi [UIStackView hızlı](#UIStackView-Quickstart) yukarıda).

Ek kısıtlamalar subviews (örneğin için denetimi genişlik ve yükseklik) ekleyerek daha fazla kullanıcı arabirimleri görünümü hassas ayarlamalar yapabilirsiniz. Bununla birlikte, çakışan kısıtlamalarını sunulanlar Yığın Görünümü tarafından kendisi için içermeyecek şekilde dikkatli olunması.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Görünümler ve alt görünümleri tutarlılık düzenlenmiş, bakımını yapma

Yığın Görünümü emin olur, `ArrangedSubviews` özelliği, her zaman bir alt kümesini, `Subviews` özelliği aşağıdaki kuralları kullanarak:

 - Bir alt görünüm eklenirse `ArrangedSubviews` koleksiyonu, otomatik olarak eklenecek `Subviews` koleksiyon (zaten bu koleksiyonun parçası olmadığı sürece).
 - Bir alt görünüm kaldırılırsa `Subviews` (görüntüden kaldırılır) koleksiyonu da kaldırılır `ArrangedSubviews` koleksiyonu.
 - Bir alt görünüm öğesinden kaldırılıyor `ArrangedSubviews` koleksiyon CİHAZDAN kaldırmaz `Subviews` koleksiyonu. Bu nedenle yığın görünümü tarafından artık düzenleneceğini, ancak ekranda görünür olmaya devam eder.

`ArrangedSubviews` Koleksiyonu olduğundan her zaman bir alt kümesini `Subview` koleksiyonu, ancak ayrı ve aşağıdaki tarafından denetlenen her koleksiyon içindeki tek tek subviews sırası:

 - İçinde subviews sırasını `ArrangedSubviews` koleksiyon yığın içinde kendi görüntülenme sırasını belirler.
 - İçinde subviews sırasını `Subview` görünümüne ön içinde koleksiyonu belirler, Z düzenini (veya katman).

### <a name="dynamically-changing-content"></a>İçeriği dinamik olarak değiştirme

Bir alt görünüm eklendiğinde, kaldırılan gizli veya her bir yığın görünümü subviews düzenini otomatik olarak ayarlayın. Yığın Görünümü herhangi bir özelliği ayarlanıyorsa düzenini de ayarlanır (gibi kendi `Axis`).

Düzen değişiklikleri, bir animasyon blokta örneğin yerleştirerek için bunları animasyonu oluşturulabilen:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Yığın görünümün özelliklerin çoğu, içinde bir film şeridi boyut sınıfları kullanılarak belirtilebilir. Bu özellikler otomatik olarak hareketli boyutu ve yönü değişiklikleri yanıt olan.

## <a name="summary"></a>Özet

Bu makalede yeni kapsamına `UIStackView` bir Xamarin.iOS uygulaması içinde yatay veya dikey olarak düzenlenmiş yığınında subviews kümesini yönetmek için (iOS 9) denetim.
Bir kullanıcı Arabirimi oluşturmak için yığın görünümleri kullanarak basit bir örnek başladı ve yığın görünümleri ve özellikleri ile özellikleri daha ayrıntılı bilgi ile tamamlandı.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [Yeni iOS 9.0 nedir](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [UIStackView başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Karşınızda UIStackView (video)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
