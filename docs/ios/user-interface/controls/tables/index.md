---
title: Tablolar ve hücreler Xamarin.iOS ile çalışma
description: Bu belge bağlantıları çeşitli kılavuzlara verilerinin UITableView denetimi ile bir Xamarin.iOS uygulaması içinde nasıl görüntüleneceğini açıklar.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: ea5b6ba532d577bd503529065eef803acf3a7aa9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241704"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Tablolar ve hücreler Xamarin.iOS ile çalışma

Bu bölümde oluşturmak ve tabloları görüntülemek için kullanılan sınıflar tanıtır sonra bunları Xamarin.ios'ta kullanma örnekleri sağlar. Düzenleme ve görsel olarak bir tablo tasarlamak için Xamarin iOS Designer kullanarak uygulama düzenini özelleştirme tabloları için varsayılan görünümünü kullanarak kapsar. Bazen görünen açıkça bir satır (örneğin, Music uygulaması) ve diğer durumlarda (örneğin, kişiler uygulamasına veya mesajlar uygulamasının konuşmada düzenleme) tablo denetimi bilmek zordur listesidir.

Xamarin.Android ile platformlar arası uygulamalar üzerinde çalışan, UITableView denetim Android ListView sınıfında benzer (ve Android bağdaştırıcısı sınıflarına UITableViewSource sınıfı benzer).

Bu makaleler de dahil olmak üzere tablolarla çalışma kapsamlı göz atalım:

-   **Tablo bölümleri** – Introducing ve görsel öğeleri açıklayan `UITableView` denetimi. 
-   **Veri tablolarında görüntüleme** – oluşturmak ve bir tabloyu doldurmak nasıl gezinileceğini gösterir, farklı bir tablo ve hücre stilleri kullanın ve hücre nesneleri geri dönüştürerek bellek sorunlarını önlemek. 
-   **Gelişmiş kullanım** – özel hücreleri oluşturmaya ve UITableView sınıfı düzenleme özelliklerini kullanarak. 
-   **Tablo görsel olarak oluşturma** – bir görsel taslak ile tablo odaklı bir arabirim oluşturmak için iOS için Xamarin Tasarımcısı'nı kullanarak. 

## <a name="contents"></a>İçindekiler

 [Tablo bölümleri &amp; işlevi](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Bir Tabloyu Verilerle Doldurma](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Bir Tablonun Görünümünü Özelleştirme](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Düzenleme](~/ios/user-interface/controls/tables/editing.md)
 
 [Satır eylemleri](~/ios/user-interface/controls/tables/row-action.md)

 [İçinde bir film şeridini tablo oluşturma](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Satır Yüksekliğini Otomatik Boyutlandırma](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>İlgili bağlantılar

- [WorkingWithTables (örnek)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Tablolarda görsel Taslaklar (örnek)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Görsel Taslaklara Giriş](~/ios/user-interface/storyboards/index.md)
- [Tablo görünümü tarif görsel taslaklarla anlatma](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [MonoTouch.Dialog giriş](~/ios/user-interface/monotouch.dialog/index.md)
- [Github'daki TableEditing örnek](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Github'daki TableParts örnek](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Github'daki TableAndCellStyles örnek](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView sınıf başvurusu](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell sınıf başvurusu](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
