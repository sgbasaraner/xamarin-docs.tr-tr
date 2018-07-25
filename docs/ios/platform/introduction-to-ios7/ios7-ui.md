---
title: iOS 7 kullanıcı arabirimine genel bakış
description: iOS 7 kullanıcı arabirimi değişiklikleri deseninizi oluşturmayı tanıtır. Bu makalede bazı büyük değişikliklerin, denetimlerin görsel görünümünü hem yeni tasarım destekleyen API'ler de vurgular.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7e7725418ed104bd9be014b80d20fd62de144ca5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242296"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 kullanıcı arabirimine genel bakış

_iOS 7 kullanıcı arabirimi değişiklikleri deseninizi oluşturmayı tanıtır. Bu makalede bazı büyük değişikliklerin, denetimlerin görsel görünümünü hem yeni tasarım destekleyen API'ler de vurgular._

iOS 7 içeriğine chrome odaklanır. İOS 7'de kullanıcı arabirimi öğeleri chrome içerik görünümler tarafından kullanılan ekran alanı miktarını azaltmak gezinti çubukları, fazlalık Kenarlıklar ve durum çubukları gibi öznitelikleri kaldırarak yinelenenleri yap. İOS 7'de, içerik, ekranın tamamını kullanmak için tasarlanmıştır.

iOS 7 birkaç değişiklik getirir: renk düğmesi kenarlıklar gibi öznitelikler yerine kullanıcı arabirimi öğeleri ayırmak için kullanılır. Gezinti çubuğu ve durum çubukları gibi birçok öğe artık bunları altındaki alana ayırdığınız bulanık ve yarı saydam veya saydam ile içerik görünümleridir. Bu içerik görünümler, kullanıcı arayüzü derinlemesine bir güvenliymiş yaygınlaşmıştır bulanık çubukları aracılığıyla işleyin.

Bu makale, birkaç kullanıcı arabirimi öğeleri iOS 7 yanı sıra çeşitli API'leri için yeni kullanıcı arabirimi tasarımı ilgili değişiklikleri kapsar.

## <a name="view-and-control-changes"></a>Görünüm ve denetim değişiklikleri

Tüm Uıkit görünümlerinde yeni Azure görünümü ve deneyimini için iOS 7 uygun. Bu bölümde, bazı değişiklikler yeni kullanıcı Arabirimi destekleyecek şekilde değiştirilmiştir ilgili API'leri yanı sıra bu görünümler vurgular.

### <a name="uibutton"></a>UIButton

Oluşturulan düğmeleri `UIButton` sınıfı artık varsayılan olarak, arka plan ile Kenarlıksız aşağıda gösterildiği gibi:

 ![](ios7-ui-images/button.png "Örnek UIButton")

`UIButtonType.RoundedRect` Stil kullanımdan kaldırıldı. İOS 7 kullandıysanız `UIButtonType.RoundedRect` sonuçlanır `UIButtonType.System` kullanılmakta olan üretir varsayılan düğme stilini arka plan veya kenarlarına, yukarıda gösterildiği gibi.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

Benzer şekilde `UIButton`, düğmeler de Kenarlıksız yeni açıklamalı olan `UIBarButtonItemStyle.Plain` aşağıda gösterilen stili:

 ![](ios7-ui-images/barbuttonplain.png "Örnek UIBarButtonItem")

Ayrıca, `UIBarButtonItemStyle.Bordered` stili kullanımdan kaldırıldı. Ayarı `UIBarButtonItemStyle.Bordered` 7 sonuçlanır iOS `UIBarButtonItemStyle.Plain` kullanılan stil.

`UIBarButtonItemStyle.Done` Stili değil kullanımdan kaldırıldı. Ancak, seveceği bir düğme de gösterildiği gibi yalnızca bir kalın metin stili oluşturacağı:

 ![](ios7-ui-images/barbuttondone.png "Örnek UIBarButtonItem bitti stili")

### <a name="uialertview"></a>UIAlertView

Yeni iOS 7 görünümü sunmalarına stil değişikliği yanı sıra uyarı görünümleri artık alt görünüm aracılığıyla özelleştirmeyi destekler. Olsa da `UIAlertView` devraldığı `UIView`çağırarak `AddSubview` üzerinde bir `UIAlertView` hiçbir etkisi olmaz. Örneğin, aşağıdaki kodu düşünün:

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

Bu standart bir uyarı görünümü gözardı ediliyor alt görünüm ile aşağıda gösterildiği gibi oluşturur:

 ![](ios7-ui-images/alert.png "Örnek UIAlertView")
 
 Not: UIAlertView iOS 8'de kullanım dışı bırakıldı. Görünüm [uyarı denetleyicisi](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller) tarif iOS 8 ve üzeri bir uyarı görünümü kullanma.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

İOS 7'deki bölümlenmiş denetimler saydam ve renk tonu rengi destekler. Metin ve kenarlık rengini, renk tonu rengi kullanılır. Bir segmenti seçtiğinizde öğe renk seçili segment, vurgulamak için aşağıda gösterildiği gibi kullanılan renk tonu rengi ile arka plan ve metin taşınır:

 ![](ios7-ui-images/segmentedcontrol.png "Örnek UISegmentedControl")

Ayrıca, `UISegmentedControlStyle` iOS 7 ' kullanım dışı bırakıldı.

### <a name="picker-views"></a>Seçici görünümleri

API Seçici görünümleri için büyük ölçüde değiştirilmez; Ancak, iOS 7 tasarım yönergeleri artık sürümleri gelen giriş görünümleri animasyonlu gibi önceki iOS olduğu gibi bir gezinti denetleyicinin yığın üzerine alt ekranın veya yeni bir denetleyici üzerinden gönderilen yerine satır içi Seçici görünümleri sunulması gereken durumu. Sistem Takvim uygulamada görülebilir:

 ![](ios7-ui-images/inlinepicker.png "Bu sistem Takvim uygulamada görülebilir.")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

Arama çubuğuna şimdi Gezinti içinde gösterilen zaman çubuk `UISearchDisplayController.DisplaysSearchBarInNavigationBar` özelliği ayarlanmışsa true. -Varsayılan değer false olarak ayarlandığında - arama denetleyicisi görüntülendiğinde gezinti çubuğunu gizlenir.

Arama çubuğu içinde aşağıdaki ekran görüntüsünde gösterilmektedir bir `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "Örnek UISearchDisplayController")

### <a name="uitableview"></a>UITableView

İlgili API'ler `UITableView` çoğunlukla olan değişmeden; ancak, stili önemli ölçüde yeni kullanıcı arabirimi tasarımı için uygun şekilde değiştirilmiştir. İç görünümü hiyerarşiyi de biraz farklıdır. Uygulamaların çoğu bu değişikliği etkilemez, ancak dikkat edilmesi gereken bir şey değildir.

#### <a name="grouped-table-style"></a>Gruplandırılmış bir tablo stili

Şimdi aşağıda gösterildiği gibi ekran kenarlarına genişletme içeriği değiştirildi gruplandırılmış stili güncelleştirdi:

 ![](ios7-ui-images/table1.png "Örnek gruplandırılmış tablo stili")

#### <a name="separatorinset"></a>SeparatorInset

Satır ayırıcı artık girintili ayarlayarak `UITableVIewCell.SeparatorInset` özelliği. Örneğin, aşağıdaki kod hücreleri sol kenarından girintilemek için kullanılacak:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

Aşağıda gösterildiği gibi girintili hücrelerle Tablo görünümünde bu oluşturur:

 ![](ios7-ui-images/separatorinset.png "Örnek UITableView SeparatorInset")

#### <a name="table-button-styles"></a>Tablo düğme stilleri

Tablo görünümleri kullanılan çeşitli düğmeler tüm değişti. Aşağıdaki ekran görüntüsünde, bir tablo görünümü düzenleme modunda sunar:

 ![](ios7-ui-images/table2.png "Bu ekran bir tablo görünümü düzenleme modunda sunar.")

### <a name="additional-control-changes"></a>Ek denetim değişiklikleri

Kaydırıcılar, anahtarlar ve adımlayıcıların gibi diğer Uıkit denetimleri de değişti. Bu değişiklikler, tamamen visual. Daha fazla bilgi için Apple başvurun [iOS 7 kullanıcı Arabirimi Geçiş Kılavuzu](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html).

## <a name="general-user-interface-changes"></a>Genel kullanıcı arabirimi değişiklikleri

Uıkit değişiklikleri yanı sıra, iOS 7 kullanıcı arabirimi, görsel değişiklikler çeşitli tanıtır dahil olmak üzere:

-  Tam ekran içeriği
-  Çubuk görünümü
-  Renk tonu rengi

<a name="fullscreen" />

### <a name="full-screen-content"></a>Tam ekran içeriği

iOS 7 ekranın tamamını yararlanmak uygulamaları sağlamak için tasarlanmıştır. -Durum ve gezinti çubukları görünen aksine varsa görünüm denetleyicileri artık bir durum çubuğu ile gezinti çubuğu - çakışan görüntülenir.

Uygulama iOS 7'için hazırlanırken, görsel olarak kullanarak subviews oranında düşürülecektir *arabirim Oluşturucu* veya *Xamarin iOS Tasarımcısı*. Ayrıca yeni API'lerden birini tam ekran içeriği programlı bir şekilde işlemeye yardımcı olmak için kullanabilirsiniz. Bu API'ler, aşağıda sunulmuştur.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide ve BottomLayoutGuide

 `TopLayoutGuide` ve `BottomLayoutGuide` içeriği bir saydam çakışan değil, burada görünümleri başlayacağı veya sonlandırmak için bir başvuru olarak hizmet `UIKit` çubuğu, aşağıdaki örnekte olduğu gibi:

 [![](ios7-ui-images/clipped.png "Örnek içerik saydam bir Uıkit çubukla çakışan değil")](ios7-ui-images/clipped.png#lightbox)

Bu API'ler, üst veya alt kısmındaki bir görünümün öteleme hesaplamak için kullanılan ve içerik yerleştirme buna uygun şekilde ayarlayabilirsiniz:

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

Ayarlamak için yukarıdaki hesaplanan değeri kullanabiliriz bizim `ImageView`ait tüm görüntü görünür, bu nedenle ekranın üst yer:

 [![](ios7-ui-images/good2.png "Ekranın üst örnek ImageViews öteleme")](ios7-ui-images/good2.png#lightbox)

Başvurmak [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) çalışma örneği.

Görünümü, bu nedenle okunmaya çalışılırken bir hiyerarşiye eklendikten sonra yer değiştirme değeri dinamik olarak oluşturulan `TopLayoutGuide` ve `BottomLayoutGuide` değerler `ViewDidLoad` 0 döndürür. Görünüm - Örneğin, yüklendikten sonra değeri hesaplamak `ViewDidLayoutSubviews`.

> [!IMPORTANT]
> `TopLayoutGuide` ve `BottomLayoutGuide` yeni güvenli alan düzeni ile değiştiriliyor iOS 11'de kullanım dışı bırakılmıştır. Apple belirtilen güvenli alan kullanan iOS 11 ' önceki iOS sürümüyle uyumlu olduğunu. Daha fazla bilgi için [uygulamanızı iOS 11 için güncelleştirme](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) Kılavuzu.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

Bu API, hangi kenarlarına bir görünüm için tam ekran, bağımsız olarak, translucency genişletilmesi belirtir. İOS 7 ' de gezinti çubukları ve araç çubukları görünür katmanlı denetleyicinin görünümü - aksine önceki iOS sürümlerinde, nerede bunlar aynı yer kaplar kaydetmedi. İOS 7 fotoğraflar uygulaması varsayılan gösterir `UIViewController.EdgesForExtendedLayout` değeri `UIRectEdge.All`. Bu ayar, çakışan ve tam ekran etkisine içerikle görünümünde tüm dört kenarına doldurur:

 [![](ios7-ui-images/photos.png "Örnek EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

Görüntü dokunarak çubukları kaldırır ve görüntü tam ekran gösterilir:

 [![](ios7-ui-images/photos2.png "EdgesForExtendedLayout çubuklu kaldırıldı")](ios7-ui-images/photos2.png#lightbox)

Tam ekran içeriği varsayılan olduğundan, iOS 6'için yapılandırılan uygulamalar, aşağıdaki ekran görüntüsünde gösterildiği gibi kırpılarak görünümün bir parçası olacaktır:

 [![](ios7-ui-images/clipped.png "İOS 6, bu ekran görüntüsünde gösterildiği gibi kırpılarak görünümü bir parçası olacak kullanıcılar için yapılandırılan uygulamalar")](ios7-ui-images/clipped.png#lightbox)

Değiştirme `UIViewController.EdgesForExtendedLayout` özelliği bu davranış için ayarlar. Biz, bizim görünümü gezinti ya da araç çubukları (her yön), kapladığı alanı içeriği görüntüleme önlemek için Görünüm herhangi kenarları doldurmak değil olduğunu belirtebilirsiniz:

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

Tüm görüntü görünür olacak şekilde uygulamamıza, görünümü yeniden yeniden konumlandırıldığında, görüyoruz:

 [![](ios7-ui-images/good.png "Örneğin, tüm görüntü görünür")](ios7-ui-images/good.png#lightbox)

Etkilerini dikkat `TopLayoutGuide/BottomLayoutGuide` ve `EdgesForExtendedLayout` API'leri benzerdir, bunlar farklı hedeflere doldurmak için tasarlanmıştır. Değiştirme `EdgesForExtendedLayout` varsayılan ayar, iOS 6'için tasarlanmış uygulamalarda bölünmüş görünümleri düzeltebilir, ancak iyi iOS 7 tasarım tam ekran estetik dikkate ve bir tam görüntüleme deneyimi, bağlı ekran sağlamak `TopLayoutGuide` ve `BottomLayoutGuide`düzgün deneyimli bir yerde kullanıcı için oturum yönetilebilmesini hazırlanmıştır içerik yerleştirmek için.

Başvurmak [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) çalışma örneği.

### <a name="status-and-navigation-bars"></a>Durum ve gezinti çubukları

Gezinti çubuğu ve durum çubuğu saydamlık ile işlenir. Durum çubukları, araç çubukları ve gezinti çubukları saydam ve bulanık derinliği kullanıcı arabiriminde, güvenliymiş aktarmaya çalışırken saydamdır. Aşağıdaki ekran görüntüsünde bu bulanıklaştırarak ve şeffaflık, burada koleksiyon görünümü mavi arka plan rengini açık mavi bir görünüm vermeden hem durumu hem de gezinti çubukları gösterir:

 ![](ios7-ui-images/transparent-navbar.png "Örnek durum ve gezinti çubuğu Bulanıklaştırma")

#### <a name="status-bar-styles"></a>Durum çubuğu stilleri

Bulanıklaştırma ve saydamlık yanı sıra, bir durum çubuğu ön açık veya koyu (Koyu varsayılan olan) olabilir. Durum çubuğu stili görünüm denetleyicisi ayarlayabilirsiniz. Durum çubuğu gizli veya gösterilen bir görünüm denetleyicisi de ayarlayabilirsiniz.

Örneğin, aşağıdaki geçersiz kılmalar kod `PreferredStatusBarStyle` durum çubuğunu açık bir ön plan görüntülemek için bir görünüm denetleyicisini yöntemi:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

Bu durum çubuğunda görüntülenecek neden olarak aşağıda:

 ![](ios7-ui-images/light-status-bar.png "Örnek durum çubuğu")

Durum çubuğu görünümü denetleyicinin koddan gizlemek için geçersiz kılma `PrefersStatusBarHidden`, aşağıda gösterildiği gibi:

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

Bu durum çubuğunda gizler:

 ![](ios7-ui-images/status-bar-hidden.png "Durum çubuğu gizli")

### <a name="tint-color"></a>Renk tonu rengi

Düğmeleri artık chrome olmadan metin olarak görüntülenir. Metin rengi kullanarak yeni denetlenebilir `TintColor` özellikte `UIView`. Ayar `TintColor` rengi tüm görünüm hiyerarşisi ayarlar görünüm için geçerlidir. Uygulanacak bir `TintColor`üzerinde ayarlanmış bir uygulama boyunca `Window`. Renk tonu rengi aracılığıyla değiştiğinde, ayrıca algılayabilir `UIView.TintColorDidChange` yöntemi.

Örneğin, aşağıdaki ekran görüntüsünde bir gezinti denetleyicinin görünümünde renk tonu rengi değiştirme etkisini gösterir mor:

 ![](ios7-ui-images/tint-color.png "Bir gezinti denetleyicisi görünümü mor renk tonu rengi")

Renk tonu rengi görüntüleri de ne zaman uygulanabilir `RenderingMode` ayarlanır `UIImageRenderingMode.AlwaysTemplate`.

> [!IMPORTANT]
> Renk tonu rengi kullanarak ayarlanamaz `UIAppearance`.


### <a name="dynamic-type"></a>Dinamik tür

İOS 7'de, kullanıcı ayarlarına metin boyutunu belirtebilirsiniz. Dinamik tür ile yazı tipi boyutundan bağımsız olarak iyi aramak için dinamik olarak ayarlanır. `UIFont.PreferredFontForTextStyle` iyileştirilmiş bir yazı tipi için kullanıcı tarafından denetlenen boyutunu almak için kullanılmalıdır.

## <a name="summary"></a>Özet

Bu makalede, iOS 7 kullanıcı arabirimi öğelerinde yapılan değişiklikleri kapsar. Bu görünüm ve her iki visual değişiklikleri vurgulama, Uıkit içinde yapılan değişikliklerin birkaçını inceler yanı sıra ilgili API'leri için değiştirir. Son olarak, tam ekran içeriği, yeni renk tonu rengi desteği ve dinamik tür ile çalışmak için yeni API'ler sunar.

## <a name="related-links"></a>İlgili bağlantılar

- [ImageViewer (örnek)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
