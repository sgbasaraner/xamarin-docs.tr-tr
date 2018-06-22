---
title: iOS 7 kullanıcı arabirimine genel bakış
description: iOS 7 kullanıcı arabirimi değişiklikleri sayısız tanıtır. Bu makalede, bazı büyük değişiklikler denetimleri görsel görünümünü hem de yeni tasarım destekler API'lerini vurgular.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3f5ea8abd41e718f9ac947c5acb290dffe400ddd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30781931"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 kullanıcı arabirimine genel bakış

_iOS 7 kullanıcı arabirimi değişiklikleri sayısız tanıtır. Bu makalede, bazı büyük değişiklikler denetimleri görsel görünümünü hem de yeni tasarım destekler API'lerini vurgular._

iOS 7 chrome içerik üzerinde odaklanmıştır. İOS 7 kullanıcı arabirimi öğeleri chrome yabancı kenarlıklar, durum çubukları ve içerik görünümler tarafından kullanılan ekran alanı miktarını azaltmak gezinti çubukları gibi öznitelikleri kaldırarak devre dışı bırakma yap. İOS 7'de, içeriği tüm ekranı kullanmak üzere tasarlanmıştır.

iOS 7 diğer bazı değişiklikler içermektedir: renk düğmesi kenarlıklar gibi öznitelikleri yerine kullanıcı arabirimi öğelerini ayırt etmek için kullanılır. Gezinti çubuğu ve durum çubukları gibi çok sayıda öğesi artık bunları altındaki alana alma bulanık ve yarı saydam ya da saydam ile içerik görünümlerdir. Bu içerik görünümleri duyguyu açığa derinliği kullanıcı arabiriminde, saymayı bulanık çubukları aracılığıyla işleyebilir.

Bu makalede birkaç kullanıcı arabirimi öğeleri iOS 7 yanı sıra çeşitli API'ler için yeni kullanıcı arabirimi tasarımı ile ilgili değişiklikleri kapsar.

## <a name="view-and-control-changes"></a>Görünüm ve denetim değişiklikleri

Tüm Uıkit görünümlerde yeni görünüm ve yapısını için iOS 7 uygun. Bu bölüm, bazı değişiklikler yeni kullanıcı arabirimini desteklemek üzere değiştirilmiştir ilgili API'leri yanı sıra Bu görünümlere vurgular.

### <a name="uibutton"></a>UIButton

Oluşturulan düğmeleri `UIButton` sınıf artık varsayılan olarak, hiçbir arka plana sahip Kenarlıksız aşağıda gösterildiği gibi:

 ![](ios7-ui-images/button.png "Örnek UIButton")

`UIButtonType.RoundedRect` Stili kullanım dışı bırakıldı. İOS 7, kullandıysanız `UIButtonType.RoundedRect` sonuçlanır `UIButtonType.System` kullanılmakta olan üretir varsayılan düğme stili arka plan veya kenarlarına, yukarıda gösterildiği gibi.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Benzer şekilde `UIButton`, düğmeler ayrıca yeni varsayılan değer olarak Kenarlıksız olan `UIBarButtonItemStyle.Plain` aşağıda gösterilen stili:

 ![](ios7-ui-images/barbuttonplain.png "Örnek UIBarButtonItem")

Ayrıca, `UIBarButtonItemStyle.Bordered` stili kullanım dışı bırakıldı. Ayarı `UIBarButtonItemStyle.Bordered` 7 sonuçlanır iOS `UIBarButtonItemStyle.Plain` kullanılan stili.

`UIBarButtonItemStyle.Done` Stili olmayan kullanım dışı bırakıldı. Ancak, bu da Kenarlıksız düğmesi gösterildiği gibi yalnızca bir kalın metin stiliyle oluşturacak:

 ![](ios7-ui-images/barbuttondone.png "Örnek UIBarButtonItem Bitti'yi stilde")

### <a name="uialertview"></a>UIAlertView

Yeni iOS 7 görünüm stili değişikliğin yanı sıra, uyarı görünümleri özelleştirme alt görünüm aracılığıyla artık desteklememektedir. Olsa bile `UIAlertView` devraldığı `UIView`, arama `AddSubview` üzerinde bir `UIAlertView` hiçbir etkisi olmaz. Örneğin, aşağıdaki kodu göz önünde bulundurun:

```csharp
UIBarButtonItem button = new UIBarButtonItem ("Bar Button", UIBarButtonItemStyle.Plain, (s,e) =>
{
    UIAlertView alert = new UIAlertView ("Title", "Message", null, "Cancel", "OK");

    alert.AddSubview (new UIView () {
        Frame = new CGRect(50, 50,100, 100),
        BackgroundColor = UIColor.Green
    });

    alert.Show ();
});
```

Bu standart bir uyarı görünümü yoksayılıyor, alt görünüm ile aşağıda gösterildiği gibi oluşturur:

 ![](ios7-ui-images/alert.png "Örnek UIAlertView")
 
 Not: UIAlertView iOS 8 kullanımdan kaldırılmıştır. Görünüm [uyarı denetleyicisi](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/) tarif iOS 8 ve üzeri bir uyarı görünümü kullanma.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

İOS 7 bölümlenmiş denetimlerinde saydam ve renk destekler. Metin ve kenarlık rengi renk kullanılır. Bir segment seçildiğinde, öğe rengi aşağıda gösterildiği gibi seçili kesimi vurgulamak için kullanılan renk ile arka plan ve metin taşınır:

 ![](ios7-ui-images/segmentedcontrol.png "Örnek UISegmentedControl")

Ayrıca, `UISegmentedControlStyle` iOS 7 kullanım dışı bırakıldı.

### <a name="picker-views"></a>Seçici görünümleri

API Seçici görünümleri için büyük ölçüde değiştirilmez; Ancak, iOS 7 tasarım yönergeleri şimdi sürümleri gelen giriş görünümlerini animasyonlu gibi önceki iOS olduğu gibi bir gezinti denetleyicisinin yığını üzerine alt ekranın veya yeni bir denetleyici üzerinden gönderilen yerine Seçici görünümleri satır içi sunulması gereken durumu. Bu sistem Takvim uygulamada görülebilir:

 ![](ios7-ui-images/inlinepicker.png "Bu sistem Takvim uygulamada görülebilir.")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Arama çubuğunu şimdi Gezinti içinde gösterilen çubuk ne zaman `UISearchDisplayController.DisplaysSearchBarInNavigationBar` özelliği ayarlanmış true. -Varsayılan değer false olarak ayarlandığında - arama denetleyicisi görüntülendiğinde, gezinti çubuğunda gizlenir.

Aşağıdaki ekran görüntüsü, içinde arama çubuğunu gösterir bir `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Örnek UISearchDisplayController")

### <a name="uitableview"></a>UITableView

API'ler `UITableView` çoğunlukla olan değişmeden; ancak, stil önemli ölçüde yeni kullanıcı arabirimi tasarımı için uygun olarak değiştirildi. İç görünüm hiyerarşisine de biraz farklıdır. Bu değişiklik çoğu uygulamaları etkilemez ancak dikkat edilmesi gereken bir şey olacaktır.

#### <a name="grouped-table-style"></a>Gruplandırılmış tablo stili

Şimdi aşağıda gösterildiği gibi ekran kenarlarına genişletme içeriği değiştirildi gruplandırılmış stili güncelleştirdi:

 ![](ios7-ui-images/table1.png "Örnek gruplandırılmış tablo stili")

#### <a name="separatorinset"></a>SeparatorInset

Satır ayırıcı şimdi girintili ayarlayarak `UITableVIewCell.SeparatorInset` özelliği. Örneğin, aşağıdaki kod, sol kenarı hücrelerden girinti kullanılacak:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

Aşağıda gösterildiği gibi girintili hücrelerle Tablo görünümünde bu üretir:

 ![](ios7-ui-images/separatorinset.png "Örnek UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Tablo düğmesi stilleri

Tablo görünümlerde kullanılan çeşitli düğmeleri tüm değiştirilmiştir. Aşağıdaki ekran görüntüsünde bir tablo görünümü düzenleme modunda sunar:

 ![](ios7-ui-images/table2.png "Bu ekran görüntüsünde bir tablo görünümü düzenleme modunda gösterir.")

### <a name="additional-control-changes"></a>Ek denetim değişiklikleri

Diğer Uıkit denetimleri de kaydırıcılar, anahtarlar ve adımlayıcıların dahil olmak üzere değiştirilmiştir. Bu değişiklikler tamamen visual. Daha fazla bilgi için Apple için başvurun [iOS 7 UI Geçiş Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Genel kullanıcı arabirimi değişiklikleri

Uıkit değişiklikleri ek olarak, iOS 7 UI visual değişiklikleri çeşitli tanıtır dahil olmak üzere:

-  Tam ekran içeriği
-  Çubuk görünümü
-  Renk

<a name="fullscreen" />

### <a name="full-screen-content"></a>Tam ekran içeriği

iOS 7 tüm ekranı yararlanmak uygulamaları olanak sağlamak için tasarlanmıştır. -Durum ve gezinti çubukları görünen aksine varsa, görünüm denetleyicileri artık bir durum çubuğu ve gezinti çubuğu - tarafından çakışan görüntülenir.

Uygulamanız iOS 7 için hazırlanırken görsel olarak kullanarak subviews yeniden hizalamak *arabirimi Oluşturucu* veya *Xamarin iOS Tasarımcısı*. Ayrıca yeni API'lerin tam ekran içeriği programlı olarak işlemeye yardımcı olmak için kullanabilirsiniz. Bu API'leri altında sunulur.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide ve BottomLayoutGuide

 `TopLayoutGuide` ve `BottomLayoutGuide` burada görünümleri başlayamaz veya bitemez, için bir başvuru olarak içerik tarafından yarı saydam çakışan değil böylece hizmet `UIKit` aşağıdaki örnekteki gibi çubuğu:

 [![](ios7-ui-images/clipped.png "Örnek içeriği bir yarı saydam Uıkit çubukla çakışan değil")](ios7-ui-images/clipped.png#lightbox)

Bu API'leri üst veya ekranın alt kısmındaki bir görünümün öteleme hesaplamak için kullanılan ve içerik yerleştirme uygun şekilde ayarlayın:

```csharp
public override void ViewDidLayoutSubviews ()
{
    base.ViewDidLayoutSubviews ();

    if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
        nfloat displacement_y = this.TopLayoutGuide.Length;

        //load subviews with displacement
    }

}
```

Ayarlamak için yukarıdaki hesaplanan değeri kullanırız bizim `ImageView`'s görüntünün tamamını görünür olacak şekilde ekranın en üstünden öteleme:

 [![](ios7-ui-images/good2.png "Örnek ImageViews öteleme ekranın en üstünden")](ios7-ui-images/good2.png#lightbox)

Başvurmak [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) çalışma örneği.

Görünüm böylece okunmaya çalışılırken bir hiyerarşiye eklendikten sonra Öteleme değeri dinamik olarak üretilen `TopLayoutGuide` ve `BottomLayoutGuide` değerler `ViewDidLoad` 0 döndürür. Görünüm - Örneğin, içinde yüklendikten sonra değeri hesaplamak `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` ve `BottomLayoutGuide` iOS 11 lehinde kaldıran yeni güvenli alan düzeni içinde kullanım dışı bırakılmıştır. Apple belirtilen güvenli alanı kullanarak iOS 11 ' önceki iOS sürümüyle uyumlu olduğunu. Daha fazla bilgi için bkz: [uygulamanız iOS 11 için güncelleştirme](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) Kılavuzu.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Bu API görünümün hangi kenarları tam ekrana bağımsız olarak, translucency genişletilmesi belirtir. İOS 7 ' ın gezinti çubukları ve araç çubuklarını görünür katmanlı denetleyicinin görünümü - aksine önceki iOS sürümlerinde, burada bunlar aynı yer kaplar alamadık. İOS 7 fotoğraflar uygulaması varsayılan gösterilmektedir `UIViewController.EdgesForExtendedLayout` değeri `UIRectEdge.All`. Bu ayar, çakışan ve tam ekran efekti oluşturma içerikle görünümünde tüm dört kenarları doldurur:

 [![](ios7-ui-images/photos.png "Örnek EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

Görüntü dokunarak çubukları kaldırır ve görüntü tam ekran gösterilir:

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout kaldırılan çubukları")](ios7-ui-images/photos2.png#lightbox)

Tam ekran içerik varsayılan olduğundan, iOS 6 için yapılandırılan uygulamalar, aşağıdaki ekran görüntüsünde olduğu gibi kırpılmış görünüme parçası vardır:

 [![](ios7-ui-images/clipped.png "İOS 6, olduğu gibi bu ekran kırpılmış görünümün parçası için yapılandırılan uygulamalar")](ios7-ui-images/clipped.png#lightbox)

Değiştirme `UIViewController.EdgesForExtendedLayout` özelliği bu davranış için ayarlar. Biz bizim görünümü Gezinti veya araç çubukları (adresindeki her yönlendirme) tarafından kapladığı alanı içerik görüntüleme önlemek için görünümü herhangi kenarları doldurmak değil olduğunu belirtebilirsiniz:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

Görüntünün tamamını görünür olacak şekilde bizim uygulamada görünümü yeniden yeniden konumlandırılır, göreceğiz:

 [![](ios7-ui-images/good.png "Örneğin, tüm görüntü görünür")](ios7-ui-images/good.png#lightbox)

Etkilerini unutmayın `TopLayoutGuide/BottomLayoutGuide` ve `EdgesForExtendedLayout` API'leri benzer, bunlar farklı hedefler doldurmaya yöneliktir. Değiştirme `EdgesForExtendedLayout` varsayılan ayar, iOS 6 için tasarlanmış uygulamalarında kırpılmış görünümleri düzeltebilir, ancak iyi iOS 7 tasarım tam ekran estetik dikkate ve bir tam görüntüleme deneyimi, güvenmek ekran sağlamak `TopLayoutGuide` ve `BottomLayoutGuide`düzgün rahat bir yerde kullanıcı için içine yönetilebilmesini amacı içerik yerleştirmek için.

Başvurmak [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) çalışma örneği.

### <a name="status-and-navigation-bars"></a>Durum ve gezinti çubukları

Durum çubuğu ve gezinti çubukları saydamlığı olan işlenir. Araç çubukları ve gezinti çubukları saydam ve kullanıcı arabiriminde derinliği duyguları iletir iletmek için bulanık durumdayken durum çubukları görünmez. Aşağıdaki ekran görüntüsü bu Bulanıklaştırma ve saydamlık, burada koleksiyon görünümü mavi arka plan rengini açık mavi görünüm vermiş durum ve gezinti çubukları gösterir:

 ![](ios7-ui-images/transparent-navbar.png "Örnek durum ve Bulanıklaştırma çubuğu gezinme")

#### <a name="status-bar-styles"></a>Durum çubuğu stilleri

Bulanıklaştırma ve saydamlık yanı sıra, bir durum çubuğu ön açık veya koyu (Koyu varsayılan olan) olabilir. Durum çubuğu stil görünüm denetleyicisinden ayarlayabilirsiniz. Durum çubuğu gizli ya da görüntülenen bir görünüm denetleyicisini de ayarlayabilirsiniz.

Örneğin, aşağıdaki geçersiz kılmalar kod `PreferredStatusBarStyle` durum çubuğunu hafif bir ön plan görüntülemek için bir görünüm denetleyicisinin yöntemi:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Bu durum çubuğunda görünmesini neden olarak aşağıda:

 ![](ios7-ui-images/light-status-bar.png "Örnek durum çubuğu")

Durum çubuğu görünümü denetleyicinin kodundan gizlemek için geçersiz kılma `PrefersStatusBarHidden`, aşağıda gösterildiği gibi:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Bu durum çubuğunu gizler:

 ![](ios7-ui-images/status-bar-hidden.png "Durum çubuğu gizli")

### <a name="tint-color"></a>Renk

Düğmeler şimdi chrome daha az metin olarak görüntülenir. Metin rengi yeni kullanılarak denetlenebilir `TintColor` özelliği `UIView`. Ayarı `TintColor` rengi tüm görünüm hiyerarşi ayarlar görünümü için geçerlidir. Uygulanacak bir `TintColor`üzerinde ayarlanmış bir uygulama boyunca `Window`. Renk aracılığıyla değiştiğinde, da algılayabilir `UIView.TintColorDidChange` yöntemi.

Örneğin, aşağıdaki ekran görüntüsü bir gezinti denetleyicisinin görünümündeki ton rengini değiştirmek etkisini gösterir mor:

 ![](ios7-ui-images/tint-color.png "Gezinti denetleyicileri görünümünde mor renk")

Renk de görüntülere ne zaman uygulanabilir `RenderingMode` ayarlanır `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Renk kullanarak ayarlanamaz `UIAppearance`.


### <a name="dynamic-type"></a>Dinamik tür

İOS 7'de, kullanıcı sistem ayarlarını metin boyutu belirtebilirsiniz. Dinamik tür ile yazı tipi boyutundan bağımsız olarak iyi aramak için dinamik olarak ayarlanır. `UIFont.PreferredFontForTextStyle` kullanıcı tarafından denetlenen boyutu için optimize edilmiş bir yazı tipi almak için kullanılmalıdır.

## <a name="summary"></a>Özet

Bu makalede, iOS 7 kullanıcı arabirimi öğeleri değişiklikleri kapsar. Bu görünümler ve her iki visual değişiklikleri vurgulama Uıkit denetimlerinde yapılan değişikliklerin birkaçını inceler yanı sıra ilgili API'leri değiştirir. Son olarak, tam ekran içeriği, yeni TINT renk desteği ve dinamik tür çalışmak için yeni API'ler sunar.

## <a name="related-links"></a>İlgili bağlantılar

- [ImageViewer (örnek)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
