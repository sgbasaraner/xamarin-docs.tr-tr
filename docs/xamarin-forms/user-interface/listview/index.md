---
title: Xamarin.Forms ListView
description: Bu kılavuz, göz alıcı, etkileşimli listelerinde verileri sunmak için kullanılan Xamarin.Forms ListView tanıtır.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: dc039a7a984fae9bd856a9e7147ad899f86f0592
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "35245007"
---
# <a name="xamarinforms-listview"></a>Xamarin.Forms ListView

ListView veri, özellikle kaydırma gerektiren uzun listelerini listeler sunmak için bir görünümüdür. Bu kılavuzda ListView kullanma işlemini gösterir:

1. **[Veri kaynakları](data-and-databinding.md)**  &ndash; bir ListView ile veya veri bağlama olmadan verileri ile doldurur.
2. **[Hücre görünümü](customizing-cell-appearance.md)**  &ndash; yerleşik hücrelerin görünüşünü özelleştirme veya kendi özel hücre oluşturun.
3. **[Liste görünümü](customizing-list-appearance.md)**  &ndash; ListView özelleştirme. Üstbilgiler ve altbilgiler ayarlayın, grupları ve satırların yüksekliğini değiştirin.
4. **[Etkileşim](interactivity.md)**  &ndash; başa Tap'ları ve seçimleri, çekme ve yenileme uygulamak ve bağlamsal eylemler ekleyin.
5. **[Performans](performance.md)**  &ndash; performans sorunlarını önler.

## <a name="use-cases"></a>Kullanım örnekleri
ListView gereksinimleriniz için doğru denetimi olduğundan emin olun. ListView burada veri kaydırılabilir listesini görüntüleyen herhangi bir durumda kullanılabilir. ListViews bağlam Eylemler ve veri bağlamayı destekler.

ListView ile karıştırılmamalıdır [Tablo görünümü](~/xamarin-forms/user-interface/tableview.md). Bağlı olmayan listesini seçenekleri veya veri olduğunda Tablo görünümü denetimi daha iyi bir seçenektir. Örneğin, çoğunlukla önceden tanımlanmış bir dizi seçenek olan iOS ayarları uygulamasından daha iyi ListView değerinden Tablo görünümü kullanmak için uygundur.

Bir ListView en iyi olduğuna dikkat edin homojen veriler için de uygun &ndash; diğer bir deyişle, tüm verileri aynı türde olmalıdır. Hücrenin yalnızca bir tür listesindeki her satır için kullanılabilir olmamasıdır. Görünümleri karıştırmak gerektiğinde daha iyi bir seçenek olduklarından TableViews birden çok hücre türlerini destekleyebilir.


## <a name="components"></a>Bileşenler
ListView çeşitli bileşenler her platform için yerel işlevselliği kullanmak kullanılabilir sahiptir. Bu bileşenlerin her birini aşağıda açıklanmaktadır:

- **[Üstbilgiler ve altbilgiler](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; metin veya görünüm listesi, başında ve sonunda görüntülemek için listenin verilerden ayırmak. Üstbilgiler ve altbilgiler ListView'ın veri kaynağından bağımsız olarak bir veri kaynağına bağlanabilir.
- **[Grupları](customizing-list-appearance.md#Grouping)**  &ndash; daha kolay gezinme için verileri bir ListView içinde gruplandırılabilir. Gruplar, genellikle veri bağımlı şunlardır:

![](images/grouping-depth.png "ListView gruplandırılmış verilerle")

- **[Hücreleri](customizing-cell-appearance.md)**  &ndash; bir ListView içindeki veri hücrelerinde sunulur. Her hücre bir veri satırına karşılık gelir. Aralarından seçim yapabileceğiniz yerleşik hücreleri vardır veya kendi özel hücre tanımlayabilirsiniz. Yerleşik ve özel hücreleri kullanılan/XAML ya da kod tanımlanan olabilir.
  - **[Yerleşik](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; hücrelerde, özellikle TextCell ve ImageCell, yerleşik performans için harika olabilir, bu yana her platformda yerel denetimlere karşılık gelir.
       - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; ayrıntı metin ile isteğe bağlı olarak bir metin dizesini görüntüler. Ayrıntı metin olarak bir Vurgu rengi ile daha küçük bir yazı tipi ikinci bir satır oluşturulur.
       - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; metin içeren bir resim görüntüler. Sol taraftaki görüntü ile bir TextCell olarak görünür.
  - **[Özel hücreleri](customizing-cell-appearance.md#customcells)**  &ndash; , karmaşık verileri sunmak ihtiyacınız olduğunda özel hücreleri harika. Örneğin, bir özel görünüm şarkı albüm ve sanatçı dahil olmak üzere, bir listesini göstermek için kullanılabilir:

![](images/image-cell-default.png "ListView ImageCells ile")

Bir ListView hücreleri özelleştirme hakkında daha fazla bilgi edinmek için [ListView hücresi görünümünü özelleştirme](customizing-cell-appearance.md).

## <a name="functionality"></a>İşlevi
ListView gibi etkileşim stilleri destekler:

- **[Çekme yenileme](interactivity.md#Pull_to_Refresh)**  &ndash; ListView her platformda çekme yenileme destekler.
- **[Bağlam Eylemler](interactivity.md#Context_Actions)**  &ndash; ListView listede tek tek öğeler üzerinde alma eylemini destekler. Örneğin, İos'ta geçirme eylemi uygulamak veya android'de eylemleri uzun dokunun.
- **[Seçimi](interactivity.md#selectiontaps)**  &ndash; seçimleri ve satır dokunulduğunda işlem kaldırılmasına dinleyebilirsiniz.

![](images/context-default.png "ListView bağlam eylemleri ile")

ListView etkileşim özellikleri hakkında daha fazla bilgi için bkz. [eylemleri & ListView etkileşimle](interactivity.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Birlikte çalışma ListView (örnek)](https://developer.xamarin.com/samples/WorkingWithListview)
- [İki yolla (örnek) bağlama](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [Yerleşik hücreler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Özel hücreleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Gruplandırma (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Özel oluşturucu Görünüm (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [ListView etkileşim (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
