---
title: Kullanıcı arabirimi Xamarin.iOS denetimlerinde
description: Bu belge bağlantılar çeşitli iOS kullanıcı arabirimi denetimlerini Xamarin.iOS geliştiricilerinin kullanabileceği açıklamak kılavuzlara. Uyarılar, düğmeleri, koleksiyon görünümlerini, görüntüler, el ile kamera denetimleri, haritalar, etiketleri, seçiciler, tarih seçiciler ve daha fazla bağlantılı içeriği açıklanır.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 477cb4e479224b9e795f3460e0f59e0aa2038630
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790048"
---
# <a name="user-interface-controls-in-xamarinios"></a>Kullanıcı arabirimi Xamarin.iOS denetimlerinde

Bu belge, bazı yaygın iOS kullanıcı arabirimi denetimlerini ve bunları nasıl kullanacağınızı tanıtır.

## <a name="alertsalertsmd"></a>[Uyarılar](alerts.md)

İOS 8 ile başlayarak, UIAlertController tamamlanmış değiştirilen UIActionSheet sahiptir ve UIAlertView hem de artık kullanım.

## <a name="buttonsbuttonsmd"></a>[Düğmeler](buttons.md)

UIButton sınıfı, çeşitli farklı stillerde iOS ekranlarda düğmesinin temsil etmek için kullanılır. Bu bölüm iOS düğmeleri ile çalışmak için farklı seçenekler sunar.

## <a name="collection-viewsuicollectionviewmd"></a>[Koleksiyon Görünümleri](uicollectionview.md)

Koleksiyon görünümleri, kullanılabilir `UICollectionView` sınıfı, birden çok öğe düzenleri kullanarak ekranda sunan tanıtan yeni bir kavram iOS 6 olarak olur. Veri sağlamak için desenleri bir `UICollectionView` öğelerini oluşturabilir ve aynı temsilci seçme ve veri kaynağı iOS geliştirme yaygın olarak kullanılan desenleri bu öğeleri izleyin etkileşim için.

## <a name="imagesimagemd"></a>[Görüntüler](image.md)

Uygulamanıza görüntüler ekleme, iki adımı gerektirir: önce görüntüler; projenize ekleyin. Ardından, denetimleri ve ekranda görüntülemek için kodu ekleyin. Başvurmak [görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) kapsamı içinde Xamarin.iOS işleme resminin ayrıntılı için makalesi.

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[El ile Kamera Denetimleri](intro-to-manual-camera-controls.md)

Tarafından sağlanan el ile kamera denetimleri `AVFoundation Framework` iOS 8'de, bir iOS cihazın kamera üzerinde tam denetim olabilmesi bir mobil uygulama izin verin. Bu hassas denetim düzeyini profesyonel düzeyi kamera uygulamalar oluşturmak ve hala bir resim veya video önünde bulundurularak kamera parametrelerinin uyguladıkça tarafından sanatçı kompozisyonlarınıza sağlamak için kullanılabilir.

## <a name="mapsios-mapsindexmd"></a>[Haritalar](ios-maps/index.md)

MAPS, modern mobil işletim sistemlerinin tümünde ortak bir özelliktir. iOS harita Seti çerçevesi aracılığıyla yerel olarak eşleme destek sunar. Harita Seti ile uygulamaları kolayca zengin ve etkileşimli eşlemeleri ekleyebilirsiniz. Bu eşlemeler, bir harita konumları işaretlemek için ek açıklamaları ekleme ve rastgele şekil grafik üst üste getirme gibi yollarla, çeşitli özelleştirilebilir. Harita Seti bile bir aygıt geçerli konumunu göstermek için yerleşik desteğe sahiptir.

## <a name="labelslabelsmd"></a>[Etiketler](labels.md)

`UILabel` Okuma yalnızca metin denetimi, tek veya çok satırlı görüntülemek için kullanılır.

## <a name="pickers-and-date-pickerspickermd"></a>[Seçiciler ve tarih seçiciler](picker.md)

Seçici denetim kaydırılabilir vurgulanmış seçili değerine sahip bir değer listesi içeren 'tekerleği benzeri' denetimi görüntüler. Kullanıcıların istedikleri seçeneğini tekerleği döndürün.

Belirli bir kullanıcı servis talebi için seçiciler, tarih ve / veya saat ayarlayın. Bu Apple için sağlamak için özel bir alt UIDatePicker adlı UIPickerView sınıfının oluşturdu.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[İlerleme Durumu ve Etkinlik Göstergeleri](progress-activity-indicator.md)

iOS, uygulamanızda ilerlemeyi göstermek için iki ana yol sağlar: etkinlik göstergeleri (belirli bir de dahil olmak üzere _ağ_ etkinliği göstergesini) ve ilerleme çubukları.

## <a name="search-barssearchbarmd"></a>[Arama çubukları](searchbar.md)

UISearchBar değerler listesini aramak için kullanılır. 

## <a name="sliders-steppers-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[Kaydırıcılar, adımlayıcıların ve segmentli denetimleri](slider-switch-segmented-controls.md)

Kaydırıcı denetimi basit bir sayısal değer bir aralık içinde seçilmesine olanak sağlar. iOS kullanan `UISwitch` bir Boole değeri, temsil ettiği diğer platformlarda radyo düğmesi giriş olarak. Bölümlenmiş bir denetim, az sayıda seçenekleri ile etkileşime girmesine izin vermek için düzenli bir yoludur.

## <a name="stack-viewuistackviewmd"></a>[Yığın Görünümü](uistackview.md)

Yığın Görünümü denetimi (`UIStackView`), iOS cihazını yönünü ve ekran boyutunu dinamik olarak yanıt güç otomatik düzeni ve boyutu sınıflarının subviews, yığınını yatay veya dikey olarak yönetmek için yararlanır.

## <a name="tables-and-cellstablesindexmd"></a>[Tablolar ve Hücreler](tables/index.md)

kendi bölümü oluşturmak ve tabloları görüntülemek için kullanılan sınıflar tanıtır sonra Xamarin.iOS içinde kullanma örnekleri sağlar. Düzenleme ve bir tablo görsel olarak tasarlamak için Xamarin iOS Tasarımcısını kullanarak uygulama düzeni özelleştirme tablolar için varsayılan görünümü kullanılarak kapsar. Bazen görüntü açıkça bir satır (örneğin, müzik uygulama) ve (kişiler uygulama ya da Messages uygulamasının konuşmada düzenleme gibi) tablo denetim tanıması zordur bazen listesidir.

## <a name="text-inputtext-inputmd"></a>[Metin Girişi](text-input.md)

Kullanıcı metin girişi kabul ile gerçekleştirilir `UITextField` tek satırlı girişleri ve çok satırlı düzenlenebilir metin UITextView. Bu denetimlerin ya da bir ekrana sürükleyin ve ilk metin ayarlamak için çift tıklayın.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[Sekme Çubukları ve Sekme Çubuğu Denetleyicileri](creating-tabbed-applications.md)

Sekme Gezinti kullanıcı arabirimini kullanarak iOS uygulamaları UITabBarController sınıfı kullanılarak oluşturulur. Bu makalede biz birkaç denetleyicileri ve görünümleri içeren bir sekmeli uygulama ayarlama konusunda size rehberlik. Biz, ardından kök denetleyicisi gibi bir oturum açma ekranı sonra olmadığında bir UITabBarController yükleme inceleyeceğiz.

## <a name="web-viewsuiwebviewmd"></a>[Web Görünümleri](uiwebview.md)

Bu makalede, sizi her Apple'nın sağladığı üç Web görünümlerinin inceleyeceksiniz: `UIWebView`, `WKWebview`, ve `SFSafariViewController`, kendi benzerlikler ve farklar ve bunların nasıl kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Denetimleri (örnek)](https://developer.xamarin.com/samples/Controls/)
