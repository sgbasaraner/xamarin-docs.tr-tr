---
title: ListView
description: Verilerinizi güzel, etkileşimli listelerinde sunar.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: ddd779fc7eb1a10e74c68504367083ff0efcdfcd
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="listview"></a>ListView

ListView veri, özellikle kaydırma gerektiren uzun listeler listesini sunan bir görünümüdür. Bu kılavuz size ListView kullanmayı gösterir:

1. **[Veri kaynakları](data-and-databinding.md)**  &ndash; ListView ile veya veri bağlama olmadan verileri ile doldurur.
2. **[Hücre görünümü](customizing-cell-appearance.md)**  &ndash; yerleşik hücrelerin görünüşünü özelleştirme veya kendi özel hücre oluşturma.
3. **[Liste görünümü](customizing-list-appearance.md)**  &ndash; ListView görünümünü özelleştirin. Üstbilgiler ve altbilgiler ayarlayabilir, gruplarını etkinleştirmek ve satırların yüksekliğini değiştirebilirsiniz.
4. **[Etkileşim](interactivity.md)**  &ndash; işlemek dokunma ve seçimleri, çekme yenileme uygulamak ve bağlamsal eylemler ekleyebilirsiniz.
5. **[Performans](performance.md)**  &ndash; performans sorunlarını önlemek.

## <a name="use-cases"></a>Kullanım örnekleri
ListView gereksinimleriniz için doğru denetim olduğundan emin olun. ListView burada veri kaydırılabilir listelerini görüntüleme herhangi bir durumda kullanılabilir. ListViews bağlamı eylemleri ve veri bağlamayı destekler.

ListView ile karıştırılmamalıdır [Tablo görünümü](~/xamarin-forms/user-interface/tableview.md). Bağlı olmayan bir liste seçenekleri ya da veri sahip olduklarında Tablo görünümü denetim daha iyi bir seçenektir. Örneğin, çoğunlukla önceden tanımlanmış bir seçenek kümesi vardır, iOS ayarlarını uygulama daha iyi ListView daha Tablo görünümü kullanmak için uygundur.

ListView en iyi olduğunu unutmayın homojen verilerini de uygun &ndash; diğer bir deyişle, tüm veriler aynı türde olmalıdır. Hücre yalnızca bir tür, listedeki her satır için kullanılabilir olmasıdır. Görünümleri karışık gerektiğinde daha iyi bir seçenek olduklarından TableViews birden çok hücre türlerini destekleyebilir.


## <a name="components"></a>Bileşenler
ListView bileşenleri her platform için yerel işlevselliği kullanmak kullanılabilir bir sayısına sahip. Bu bileşenlerin her birini aşağıda açıklanmıştır:

- **[Üstbilgiler ve altbilgiler](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; metin veya Görünüm bir liste başında ve sonunda görüntülemek için listenin verilerden ayırın. Üstbilgiler ve altbilgiler ListView'ın veri kaynağından bağımsız olarak bir veri kaynağına bağlanabilir.
- **[Grupları](customizing-list-appearance.md#Grouping)**  &ndash; ListView verileri için daha kolay gezinme gruplandırılabilir. Gruplar, genellikle veri bağlı şunlardır:

![](images/grouping-depth.png "ListView gruplandırılmış verilerle")

- **[Hücreleri](customizing-cell-appearance.md)**  &ndash; ListView içindeki veri hücrelerinde sunulur. Her bir hücre veri satırına karşılık gelir. Aralarından seçim yapabileceğiniz yerleşik hücreleri vardır veya kendi özel hücre tanımlayabilirsiniz. Yerleşik ve özel hücreleri kullanılan/XAML veya kod tanımlanmış olabilir.
  - **[Yerleşik](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; her platformda yerel denetimlere karşılık gelen beri hücrelerde, özellikle TextCell ve ImageCell, yerleşik performans için harika olabilir.
    - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; ayrıntı metni isteğe bağlı olarak bir metin dizesi görüntüler. Ayrıntı metin Aksan rengi ile daha küçük bir yazı tipi ikinci satır olarak işlenir.
    - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; metinle birlikte bir resim görüntüler. Sol taraftaki görüntüsü olan bir TextCell olarak görünür.
  - **[Özel hücreleri](customizing-cell-appearance.md#customcells)**  &ndash; karmaşık veri sunmak gerektiğinde özel hücreleri harika. Örneğin, özel bir görünüm albüm ve sanatçı dahil olmak üzere şarkıya listesini sunmak için kullanılabilir:

![](images/image-cell-default.png "ListView ImageCells ile")

ListView hücreleri özelleştirme hakkında daha fazla bilgi için bkz: [ListView hücre görünümünü özelleştirme](customizing-cell-appearance.md).

## <a name="functionality"></a>İşlevi
ListView gibi etkileşim stilleri destekler:

- **[Çekme yenileme](interactivity.md#Pull_to_Refresh)**  &ndash; ListView her platformda çekme yenileme destekler.
- **[Bağlam Eylemler](interactivity.md#Context_Actions)**  &ndash; ListView listesini tek tek öğelerde alma eylemini destekler. Örneğin, İos'ta manyetik eylemi uygulamak veya uzun dokunma Android eylemler.
- **[Seçim](interactivity.md#selectiontaps)**  &ndash; seçimleri ve bir satır dokunduğunuz durumlarda eyleme kaldırılmasına için dinler.

![](images/context-default.png "ListView bağlam eylemleri ile")

ListView etkileşim özellikleri hakkında daha fazla bilgi için bkz: [Eylemler & ListView ile etkileşim](interactivity.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Liste görünümü ile çalışma (örnek)](https://developer.xamarin.com/samples/WorkingWithListview)
- [İki yolla (örnek) bağlama](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [Yerleşik hücreler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Özel hücreleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Gruplandırma (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Özel oluşturucu görünümü (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [ListView etkileşim (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [iOS çalışma kitabı](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-ios.workbook)
- [Android çalışma kitabı](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-android.workbook)
