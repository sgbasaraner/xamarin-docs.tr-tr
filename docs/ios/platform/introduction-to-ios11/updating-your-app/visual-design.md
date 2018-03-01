---
title: "Görsel tasarım güncelleştirmeleri"
description: "İOS 11'ın yeni özellikleri keşfetmek"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 2904a7da73f5bf6e8960f65239d1f8dc52ab1aba
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="visual-design-updates"></a>Görsel tasarım güncelleştirmeleri

_İOS 11'ın yeni özellikleri keşfetmek_

İOS 11'da, gezinti çubuğunda, arama çubuğunda ve tablo görünümleri güncelleştirmeleri de dahil olmak üzere yeni görsel değişiklikler Apple kullanıma sunuldu. Ayrıca kenar boşluklarını ve tam ekran içerik üzerinde daha fazla esneklik sağlamak amacıyla iyileştirmeler yapılmıştır. Bu değişiklikler, bu kılavuzda ele alınmıştır.

## <a name="uikit"></a>Uıkit

Uıkit çubukları iOS son kullanıcılar için daha erişilebilir hale getirmek için 11 uyarlanan.

Bu tür bir değişiklik olan bir kullanıcı uzun-bir çubuğunda basarsa olduğunda görüntülenen bir yeni HUD ekran öğesi. Bu ayarı etkinleştirmek için ayarlanmış `largeContentSizeImage` özelliği `UIBarItem` ve aracılığıyla daha büyük bir görüntü ekleyin bir [varlık Kataloğu](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Gezinti çubuğu
iOS 11 gezinti çubuğu başlıkları okunmasını kolaylaştırmak için yeni işlevler sunulur. Uygulamalar, bu büyük başlık atayarak görüntüleyebilir `PrefersLargeTitles` özelliği true olarak:

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Ayar büyük başlıklar, uygulamanızda yapar _tüm_ aşağıdaki ekran görüntüsünde gösterildiği gibi gezinti çubukları başlıkları uygulamanızı arasında daha büyük görüntülenir:

![Büyük gezinti başlığı](visual-design-images/image7.png)

Büyük bir başlık Gezinti Çubuğu görüntülendiğinde denetlemek için ayarlanmış `LargeTitleDisplayMode` Gezinti öğesi üzerinde `Always`, `Never`, veya `Automatic`.

### <a name="search-controller"></a>Arama denetleyicisi

iOS 11 doğrudan Gezinti çubuğu arama denetleyici ekleme yapılan. Bir arama denetleyicisi oluşturduktan sonra gezinti çubuğunda ekleyin `SearchController` özelliği:

```csharp
NavigationItem.SearchController = searchController;
```

[![Arama çubuğunu büyük Gezinti başlık](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png)

Uygulamanızı işlevselliğini, bağlı olarak olabilir veya bir kullanıcı listesini kaydırdığında gizlemek için arama çubuğunu istemeyebilirsiniz. Bu kullanarak ayarlamanız `HidesSearchBarWhenScrolling` özelliği.

## <a name="margins"></a>Kenar boşlukları

Apple, yeni bir özellik – oluşturdu `directionalLayoutMargins` – görünümleri ve subviews arasındaki boşluğu ayarlamak için kullanılabilir. Kullanım `directionalLayoutMargins` ile `leading` veya `trailing` iç metinleri. Sistem soldan sağa veya sağdan sola dil olup bağımsız olarak, uygulamanızı aralığı uygun şekilde iOS tarafından ayarlanır.

İOS 10 ve önce tüm görünümleri hizalama minimum kenar boşluğu boyutu vardı. iOS 11 sunulan kullanarak bu geçersiz kılma seçeneği `ViewRespectsSystemMinimumLayoutMargins`. Örneğin, bu özelliği false olarak ayarlamak, kenar iç metinleri sıfır ayarlamanıza olanak sağlar:

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Resim gösteren kenar ios 11 içinde sıfır iç metin ile](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>Tam ekran içeriği

iOS 7 [sunulan](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` ve `bottomLayoutGuide` tarafından Uıkit çubukları gizli olmayan ve bir ekranınızı görünür alanındadır görünümlerinizi sınırlama bir yolu olarak. Bu iOS 11 şunun için de kullanım dışı bırakıldı _güvenli alan_.

Güvenli alan uygulamanız ve kısıtlamaları Süper görüntüsü ve bir görünüm arasında nasıl eklenir görünür alan düşünmek yeni yoludur. Örneğin, aşağıdaki resimde göz önünde bulundurun:

[![Güvenli alan vs üst ve alt düzen kılavuzu](visual-design-images/image10-sml.png)](visual-design-images/image10.png)

Bir görünüm eklendi ve yukarıdaki yeşil alanında görünür olmasını istiyorsanız, daha önce kendisine sınırlamak _alt_ , `TopLayoutGuide` ve _üst_ , `BottomLayoutGuide`. İOS 11'de, bunun yerine kendisine sınırlamak _üst_ ve _alt_ güvenli alanının. Bir örnek aşağıda verilmiştir:

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Tablo görünümü

UITableView küçük, ancak önemli değişiklik sayısı iOS 11 oluşturdu.

Varsayılan olarak, üstbilgileri, altbilgileri ve hücreler artık otomatik olarak içeriğine göre boyutlandırılır. Bu otomatik boyutlandırma davranışı kümesi dışında kabul etmek için `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, veya `EstimatedSectionFooterHeight` sıfır.

Ancak, bazı durumlarda (ne zaman gibi UITableViewController Tasarımcısı iOS veya varolan Storboards arabirimi Oluşturucusu'nda kullanırken ekleme) otomatik olarak boyutlandırma hücreleri el ile etkinleştirmeniz gerekebilir. Bunu yapmak için aşağıdaki özellikleri hücreler, üstbilgiler ve altbilgiler, Tablo görünümünde sırasıyla ayarladığınızdan emin olun:

```csharp
// Cells
TableView.RowHeight = UITableView.AutomaticDimension;
TableView.EstimatedRowHeight = UITableView.AutomaticDimension;

// Header
TableView.SectionHeaderHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionHeaderHeight = 40f;

//Footer
TableView.SectionFooterHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionFooterHeight = 40f;

```

iOS 11 satır Eylemler işlevselliğini genişletilmiştir. `UISwipeActionsConfiguration` bir Tablo görünümünde satır üzerindeki her iki yönde kullanıcı swipes zaman gerçekleşeceğini eylemleri kümesini tanımlamak için sunulmuştur. Bu davranış, yerel Mail.app benzer. Daha fazla bilgi için bkz: [satır Eylemler](~/ios/user-interface/controls/tables/row-action.md) Kılavuzu.

Tablo görünümleri için iOS 11 sürükleyip destek vardır. Daha fazla bilgi için bkz: [sürükle ve bırak](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) Kılavuzu.


## <a name="related-links"></a>İlgili bağlantılar

- [Yeni iOS 11 (Apple) nedir](https://developer.apple.com/ios/)
- [Güncelleştirilmiş uygulama mağazası ürün sayfası (Apple)](https://developer.apple.com/app-store/product-page/)
- [Güncelleştirme uygulamanızı iOS 11 (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/204/)
