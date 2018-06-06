---
title: Tabloları ve Xamarin.iOS hücrelerde ile çalışma
description: Bu belge bağlantılar çeşitli kılavuzlara verilerinin UITableView denetimi ile bir Xamarin.iOS uygulaması nasıl görüntüleneceğini açıklar.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: ebdad2cc8e3083bee5acc127660b5641f42c731f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790022"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Tabloları ve Xamarin.iOS hücrelerde ile çalışma

Bu bölümde oluşturmak ve tabloları görüntülemek için kullanılan sınıflar tanıtır sonra Xamarin.iOS içinde kullanma örnekleri sağlar. Düzenleme ve bir tablo görsel olarak tasarlamak için Xamarin iOS Tasarımcısını kullanarak uygulama düzeni özelleştirme tablolar için varsayılan görünümü kullanılarak kapsar. Bazen görüntü açıkça bir satır (örneğin, müzik uygulama) ve (kişiler uygulama ya da Messages uygulamasının konuşmada düzenleme gibi) tablo denetim tanıması zordur bazen listesidir.

Platformlar arası uygulamalar Xamarin.Android ile çalışan, UITableView denetim Android ListView sınıfında benzer (ve UITableViewSource sınıfı Android'ın bağdaştırıcısı sınıflarına benzer).

Bu makaleler de dahil olmak üzere sekmelerle çalışma kapsamlı bir göz atalım:

-   **Tablo bölümleri** – Introducing ve görsel öğeleri açıklayan `UITableView` denetim. 
-   **Veri tablolarında görüntüleme** – oluşturmak ve bir tabloyu doldurmak nasıl gösteren, farklı tablo ve hücre stilleri kullanın ve hücre nesneleri dönüştürerek bellek sorunlarından kaçınmak. 
-   **Kullanım Gelişmiş** – özel hücreleri oluşturma ve düzenleme UITableView sınıf özelliklerini kullanarak. 
-   **Tablo görsel olarak oluşturma** – film şeridi tablo tabanlı bir arabirim oluşturmak için iOS için Xamarin Tasarımcısı'nı kullanarak. 

## <a name="contents"></a>İçindekiler

 [Tablo bölümleri &amp; işlevi](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Bir Tabloyu Verilerle Doldurma](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Bir Tablonun Görünümünü Özelleştirme](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Düzenleme](~/ios/user-interface/controls/tables/editing.md)
 
 [Satır Eylemler](~/ios/user-interface/controls/tables/row-action.md)

 [Bir film şeridi tabloları oluşturma](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Satır Yüksekliğini Otomatik Boyutlandırma](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>İlgili bağlantılar

- [WorkingWithTables (örnek)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Film şeritleri (örnek) tablolarında](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Görsel Taslaklara Giriş](~/ios/user-interface/storyboards/index.md)
- [Bir tablo görünümü tarif film şeridi](https://developer.xamarin.com/recipes/ios/general/storyboard/storyboard_a_tableview)
- [MonoTouch.Dialog giriş](~/ios/user-interface/monotouch.dialog/index.md)
- [Github'da TableEditing örnek](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Github'da TableParts örnek](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Github'da TableAndCellStyles örnek](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView sınıf başvurusu](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell sınıf başvurusu](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
