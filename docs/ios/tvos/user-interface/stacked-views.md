---
title: Xamarin tvOS Yığılmış görünümler ile çalışma
description: Bu belge çalışılan tvOS ile Xamarin ile oluşturulan bir uygulamayı Yığılmış görünümlerde açıklar. Yığılmış görünümleri üst düzey bir genel bakış sağlar ve otomatik konumlandırma ve Yığılmış bir görünüm, ortak kullanır, film şeritleri ile tümleştirme ve daha fazla boyutlandırma düzeni anlatılmaktadır.
ms.prod: xamarin
ms.assetid: 00B07F85-F30B-4DD4-8664-A61D0A1CDB0E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7e718e525c23e78fbf846209602a07bf0f3f386e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789378"
---
# <a name="working-with-tvos-stacked-views-in-xamarin"></a>Xamarin tvOS Yığılmış görünümler ile çalışma

Yığın Görünümü denetimi (`UIStackView`), dinamik içerik değişikliklerini ve Apple TV cihazın ekran boyutu yanıt güç otomatik düzeni ve boyutu sınıflarının subviews, yığınını yatay veya dikey olarak yönetmek için yararlanır.

Bir yığın görünüme bağlı tüm subviews düzenini eksen, dağıtım, hizalama ve aralığı gibi tanımlanmış Geliştirici özelliklerindeki göre tarafından yönetilir:

[![](stacked-views-images/stacked01.png "Alt görünüm düzeni diyagramı")](stacked-views-images/stacked01.png#lightbox)

Kullanırken bir `UIStackView` Xamarin.tvOS uygulamada, geliştirici ya da iOS Tasarımcısı veya ekleme ve C# kodunda subviews kaldırarak film şeridi içinde subviews tanımlayabilirsiniz.

## <a name="about-stacked-view-controls"></a>Yığılmış görünüm denetimleri hakkında 

`UIStackView` Tasarlanmıştır işleme olmayan kapsayıcı görünüm olarak ve bu nedenle, diğer alt sınıflarının gibi tuvale çizildiğinde değil `UIView`. Gibi özellikleri ayarlamak `BackgroundColor` veya geçersiz kılma `DrawRect` visual hiçbir etkisi olmaz.

Bir yığın görünümü subviews kendi koleksiyonunu nasıl düzenlenir denetleyen çeşitli özellikler şunlardır:

- **Eksen** – yığını görünüm subviews ya da düzenler varsa belirler **yatay** veya **dikey**.
- **Hizalama** – subviews yığın görünümü içinde nasıl hizalandığını denetler.
- **Dağıtım** – subviews yığın görünümde nasıl boyutlandırılır denetler.
- **Aralık** – yığını görünümünde her alt görünüm arasında en az alanı denetler.
- **Taban çizgisi göreli** – varsa `true`, her alt görünüm dikey aralığını, taban çizgisinden elde edilir.
- **Düzen kenar boşlukları göreli** – subviews göre Standart Düzen kenar boşlukları yerleştirir.

Tipik olarak az sayıda subviews düzenlemek için bir yığın görünümünü kullanın. Daha karmaşık kullanıcı arabirimleri, bir veya daha fazla yığın görünümlerini birbirine içinde iç içe geçme tarafından oluşturulabilir.

Daha fazla, ek kısıtlamalar (örneğin için Denetim genişlik ve yükseklik) subviews ekleyerek Uı'lar görünüm ince ayar yapabilirsiniz. Ancak, bu yığını görünüm tarafından kendisine sunulan çakışan kısıtlamaları içermeyecek şekilde dikkatli olunmalıdır.

<a name="Auto-Layout-and-Size-Classes" />

## <a name="auto-layout-and-size-classes"></a>Otomatik düzeni ve boyutu sınıfları

Bir alt görünüm yığını görünümüne eklendiğinde düzenini tamamen, yığın konumu ve boyutu düzenlenen görünümler için otomatik düzeni ve boyutu sınıflarını kullanarak görünüm tarafından denetlenir.

Yığın Görünümü olacak _PIN_ ilk ve son alt görünüm, koleksiyondaki **üst** ve **alt** kenarları dikey yığın görünümler için veya **Sol**ve **sağ** kenarları yatay yığını görünümler için. Ayarlarsanız `LayoutMarginsRelativeArrangement` özelliğine `true`, görünüm ilgili kenar boşluklarına kenar yerine subviews sabitler sonra.

Yığın görünüm alt Görünüm'ın kullandığı `IntrinsicContentSize` tanımlı boyunca subviews boyutu hesaplanırken özelliği `Axis` (dışında `FillEqually Distribution`). `FillEqually Distribution` Aynı boyutta; böylece tüm subviews göre yeniden boyutlandırır böylece boyunca yığın görünümü doldurma `Axis`.

Dışında `Fill Alignment`, yığın görünüm alt Görünüm'ın kullandığı `IntrinsicContentSize` görünümün boyutu için dikey hesaplamak için özellik verilen `Axis`. İçin `Fill Alignment`, böylece dikeydir yığın görünümü doldurun tüm subviews boyutlandırılır verilen `Axis`.

<a name="Positioning-and-Sizing-the-Stack-View" />

## <a name="positioning-and-sizing-the-stack-view"></a>Konumlandırma ve yığın görünümü boyutlandırma

Yığın Görünümü tüm alt görünüm düzenini toplam denetime sahipken (özelliklerine gibi dayalı `Axis` ve `Distribution`), yine de yığın görünüm konumunu gerekiyorsa (`UIStackView`) otomatik düzeni ve boyutu sınıflarını kullanarak kendi üst görünümü içinde.

Genellikle, bu en az iki kenarları genişletin ve Sözleşme, böylece konumunu tanımlamak için yığın görünümünün sabitleme anlamına gelir. Başka kısıtlamalar, yığın görünümü otomatik olarak kendi subviews tümünün gibi uyacak şekilde yeniden boyutlandırılıp:

* Boyutu boyunca kendi `Axis` tüm alt görünüm boyutları artı her alt görünüm arasında tanımlanan herhangi bir alanı toplamı olacaktır.
* Varsa `LayoutMarginsRelativeArrangement` özelliği `true`, yığın görünümleri boyutu kenar boşlukları için yeriniz dahil edilir.
* Dikeydir boyutu `Axis` koleksiyonunda en büyük alt görünüm için ayarlayın.

Ayrıca, yığın görünümün için kısıtlamalar belirtebilirsiniz **yükseklik** ve **genişliği**. Bu durumda, subviews (boyutlu) yığını tarafından belirlenen görünümü tarafından belirtilen alanı dolduracak şekilde düzenleneceğini `Distribution` ve `Alignment` özellikleri.

Varsa `BaselineRelativeArrangement` özelliği `true`, subviews kullanmak yerine ilk veya son alt görünüm ait temel göre düzenleneceğini **üst**, **alt** veya **Merkezi* -  **Y** konumu. Bunlar yığını Görünüm'ün içeriğine şöyle hesaplanmıştır:

* Bir dikey yığın görünümü ilk taban çizgisi için ilk alt Görünüm ve son son döndürür. Bu subviews birini kendilerini yığın görünümler varsa, ilk veya son taban çizgisi kullanılır.
* Yatay bir yığın görünümü en uzun kendi alt görünüm için hem bir ilk ve son temel kullanır. En uzun görünüm aynı zamanda bir yığın görünüm ise, en uzun alt görünüm taban çizgisi olarak kullanır.

> [!IMPORTANT]
> Taban çizgisi yanlış konuma hesaplanır gibi temel hizalama uzamış ya da sıkıştırılmış alt görünüm boyutlarına çalışmaz. Alt Görünüm'ın sağlamak için taban çizgisi hizalamasını, **yükseklik** iç içerik görünümün eşleşen **yükseklik**.




<a name="Common-Stack-View-Uses" />

## <a name="common-stack-view-uses"></a>Ortak yığın görünümü kullanır

İyi yığın görünümü denetimleriyle çalışma birkaç yerleşim türleri vardır. Apple göre birkaç daha yaygın kullanımları şunlardır:

- **Boyut boyunca ekseni tanımlamak** – hem kenarları yığını görünümün boyunca sabitleme `Axis` ve bitişik kenarları görünümü büyümesine kendi subviews tarafından tanımlanan boşluğa uyacak şekilde ekseninde yığın konumu ayarlayın.
*  **Alt Görünüm'ın konumunu tanımlamak** – yığını görünümün, üst bitişik kenarlarına sabitleme tarafından yığın görünümü onun içeren subviews sığması için her iki boyutlarında büyüyecektir.
- **Boyutunu ve konumunu yığınının tanımlamak** – yığını görünümün üst tüm dört kenarlarına sabitlemenin yığını görünüm yığın görünümü içinde tanımlanan alanı göre subviews düzenler.
*  **Boyutu eksenine dik tanımlamak** – hem kenarları dikey olarak yığın görünümün sabitleme `Axis` ve bir görünüm büyümesine tarafından tanımlanan boşluğa uyacak şekilde eksenine dik yığın konumunu ayarlamak için ekseninde kenarlarının kendi subviews.

<a name="Stack-Views-and-Storyboards" />

## <a name="stack-views-and-storyboards"></a>Yığın görünümleri ve film şeritleri

Xamarin.tvOS uygulamada yığın görünümlerle çalışma yapmanın en kolay yolu, onları iOS Tasarımcısı kullanarak uygulamanın UI eklemektir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. İçinde **çözüm paneli**, bağlantısındaki `Main.storyboard` dosya ve düzenlemek için açın.
1. Yığın görünümüne eklemek için uygulayacağınız, ayrı ayrı öğeler düzenini tasarım: 

    [![](stacked-views-images/layout01.png "Öğe düzeni örneği")](stacked-views-images/layout01.png#lightbox)
1. Gerekli kısıtlamalar doğru ölçeklendirme emin olmak için öğelerine ekleyin. Öğe için yığın görünümü eklendikten sonra bu önemli bir adımdır.
1. Gerekli sayıda kopyasını (Bu durumda dört) olun: 

    [![](stacked-views-images/layout02.png "Kopya gerekli sayısı")](stacked-views-images/layout02.png#lightbox)
1. Sürükleme bir **yığın görünümü** gelen **araç** ve görünümünde bırak: 

    [![](stacked-views-images/layout03.png "Bir yığın görünümü")](stacked-views-images/layout03.png#lightbox)
1. Yığın görünümü seçin **pencere öğesi sekmesini** , **özellikleri paneli** seçin **doldurun** için **hizalama**, **doldurun Eşit oranda** için **dağıtım** ve girin `25` için **aralığı**: 

    [![](stacked-views-images/layout04.png "Pencere öğesi sekmesi")](stacked-views-images/layout04.png#lightbox)
1. Burada istediğiniz ve gerekli konumda kalmasını sağlamak için kısıtlamalar eklemek ekranında yığın görünümü getirin.
1. Bireysel öğeleri seçin ve yığın görünüme sürükleyin: 

    [![](stacked-views-images/layout05.png "Yığın görünümünde ayrı ayrı öğeler")](stacked-views-images/layout05.png#lightbox)
1. Düzen ayarlanacak ve öğeleri yukarıda belirlenen özniteliklerini temel alarak yığını görünümünde düzenlenir.
1. Ata **adları** içinde **pencere öğesi sekmesini** , **özellikleri Explorer** UI denetimlerinizi C# kodunda çalışmak için.
1. Değişikliklerinizi kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. İçinde **Çözüm Gezgini**, bağlantısındaki `Main.storyboard` dosya ve düzenlemek için açın.
1. Yığın görünümüne eklemek için uygulayacağınız, ayrı ayrı öğeler düzenini tasarım: 

    [![](stacked-views-images/layout01.png "Örnek öğesi düzeni")](stacked-views-images/layout01.png#lightbox)
1. Gerekli kısıtlamalar doğru ölçeklendirme emin olmak için öğelerine ekleyin. Öğe için yığın görünümü eklendikten sonra bu önemli bir adımdır.
1. Gerekli sayıda kopyasını (Bu durumda dört) olun: 

    [![](stacked-views-images/layout02.png "Kopya gerekli sayısı")](stacked-views-images/layout02.png#lightbox)
1. Sürükleme bir **yığın görünümü** gelen **araç** ve görünümünde bırak: 

    [![](stacked-views-images/layout03-vs.png "Bir yığın görünümü")](stacked-views-images/layout03-vs.png#lightbox)
1. Yığın görünümü seçin **pencere öğesi sekmesini** , **özellikleri Explorer** seçin **doldurun** için **hizalama**, **doldurun Eşit oranda** için **dağıtım** ve girin `25` için **aralığı**: 

    [![](stacked-views-images/layout04-vs.png "Pencere öğesi sekmesi")](stacked-views-images/layout04-vs.png#lightbox)
1. Burada istediğiniz ve gerekli konumda kalmasını sağlamak için kısıtlamalar eklemek ekranında yığın görünümü getirin.
1. Bireysel öğeleri seçin ve yığın görünüme sürükleyin: 

    [![](stacked-views-images/layout05-vs.png "Yığın görünümünde ayrı ayrı öğeler")](stacked-views-images/layout05-vs.png#lightbox)
1. Düzen ayarlanacak ve öğeleri yukarıda belirlenen özniteliklerini temel alarak yığını görünümünde düzenlenir.
1. Ata **adları** içinde **pencere öğesi sekmesini** , **özellikleri Explorer** UI denetimlerinizi C# kodunda çalışmak için.
1. Değişikliklerinizi kaydedin.

-----

> [!IMPORTANT]
> Eylemler gibi atamak mümkün olmakla birlikte `TouchUpInside` bir kullanıcı Arabirimi öğesine (gibi bir `UIButton`) Apple TV ekranında veya dokunma olaylarını destekleyen bir touch sahip olmadığından iOS olay işleyicisi oluştururken Tasarımcısı, hiçbir zaman çağrılır. Varsayılan değer her zaman kullanmalısınız `Action Type` Eylemler tvOS için kullanıcı arabirimi öğeleri oluşturulurken.

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md).

Bizim örneğimizde söz konusu olduğunda, biz bir çıkış ve Segment denetimi için eylem ve prizine her "player kart" için açık. Kodda, gizleme ve geçerli kesiminde tabanlı oynatıcı gösterir. Örneğin:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take Action based on the segment
    switch(PlayerCount.SelectedSegment) {
    case 0:
        Player1.Hidden = false;
        Player2.Hidden = true;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 1:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = true;
        Player4.Hidden = true;
        break;
    case 2:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = true;
        break;
    case 3:
        Player1.Hidden = false;
        Player2.Hidden = false;
        Player3.Hidden = false;
        Player4.Hidden = false;
        break;
    }
}
```

Uygulamayı çalıştırdığınızda, dört öğeleri bizim yığını görünümünde eşit olarak dağıtılır:

[![](stacked-views-images/layout06.png "Uygulamayı çalıştırdığınızda, dört öğeleri eşit bizim yığını görünümünde dağıtılır")](stacked-views-images/layout06.png#lightbox)

Oynatıcıları sayısı azalır, kullanılmayan görünümleri gizlidir ve yığın görünümü uyacak şekilde düzenini ayarlama:

[![](stacked-views-images/layout07.png "Oynatıcıları sayısı azalır, kullanılmayan görünümleri gizlidir ve yığın görünümü uyacak şekilde düzenini ayarlama")](stacked-views-images/layout07.png#lightbox)

<a name="Populate-a-Stack-View-from-Code" />

### <a name="populate-a-stack-view-from-code"></a>Kod yığını görünümünden doldurma

Tamamen yığını görünüm düzenini ve içeriği iOS Tasarımcısı tanımlamanın yanı sıra, oluşturabilir ve C# kodundan dinamik olarak kaldırın.

Gözden geçirme (1-5) "yıldız" işlemek için bir yığın görünüm kullanan aşağıdaki örnekte alın:

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

<a name="Dynamically-Changing-Content" />

## <a name="dynamically-changing-content"></a>İçerik dinamik olarak değiştirme

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

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve Xamarin.tvOS uygulama içinde Yığılmış görünümü ile çalışırken kapsamına.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
