---
title: RecyclerView bölümleri ve İşlevler
description: RecyclerView Düzen Yöneticisi, bağdaştırıcı ve görünüm sahibi genel bakış.
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/13/2018
ms.openlocfilehash: 4d55124e9a02489d1f55e900c537939ff3450509
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038514"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView bölümleri ve İşlevler


`RecyclerView` tanıtıcıları bazı görevler dahili olarak (kaydırma ve görünümleri geri dönüştürme gibi), ama aslında koordinatları yardımcı sınıfları koleksiyonu görüntülemek için bir yönetici olur. `RecyclerView` Aşağıdaki yardımcı sınıfları için temsilciler görevler:

-   **`Adapter`** &ndash; Öğe düzenleri Şişir (bir düzen dosyası içeriğini oluşturur) ve görünümler içinde görüntülenen verileri bağlar bir `RecyclerView`. Bağdaştırıcıyı ayrıca öğesi tıklama olaylarını bildirir.

-   **`LayoutManager`** &ndash; Ölçer ve öğesi görünümler içinde konumlandırır bir `RecyclerView` ve görünüm geri dönüştürme İlkesi yönetir.

-   **`ViewHolder`** &ndash; Arar ve görünümü başvuruları depolar. Görünüm sahibi öğesi görünümü tıklama algılama ile yardımcı olur.

-   **`ItemDecoration`** &ndash; Özel çizim ve Düzen uzaklıkları Bölücü öğeleri, öne çıkan özellikleri ve görsel gruplandırma sınırları arasında çizmek için belirli görünümler eklemek bir uygulama sağlar.

-   **`ItemAnimator`** &ndash; Öğe eylemleri sırasında gerçekleşmesi ya da bağdaştırıcı için yapılan değişiklikleri animasyonları tanımlar.

Arasındaki ilişkiyi `RecyclerView`, `LayoutManager`, ve `Adapter` sınıfları aşağıdaki Diyagramda gösterilen:

![LayoutManager içeren, veri kümesine erişmek için bağdaştırıcısı kullanarak RecyclerView diyagramı](parts-and-functionality-images/01-recyclerview-diagram.png)

Bu şekilde gösterildiği gibi `LayoutManager` , arasında bir aracı olarak düşünülebilir `Adapter` ve `RecyclerView`. `LayoutManager` İçine çağrılar `Adapter` yöntemleri adına `RecyclerView`. Örneğin, `LayoutManager` çağrıları bir `Adapter` belirli öğesi konumda yeni bir görünümü oluşturmak için zaman olduğunda yöntemi `RecyclerView`. `Adapter` Oluşturur ve bu öğe için Düzen Şişir bir `ViewHolder` görünümlere ilgili konumdaki önbellek referansları örneğine (gösterilmemiştir). Zaman `LayoutManager` çağrıları `Adapter` veri kümesi için belirli bir öğe bağlamak için `Adapter` bu öğeye ilişkin verileri bulur, veri kümesinden alır ve ilişkili öğesi görünümüne kopyalar.

Kullanırken `RecyclerView` uygulamanızda aşağıdaki sınıflar türetilmiş türlerini oluşturmak gerekli değildir:

-   **`RecyclerView.Adapter`** &ndash; (Bu, uygulamanıza özeldir), uygulamanızın veri kümesindeki bir bağlama içinde görüntülenen öğe görünümler sağlar `RecyclerView`. Bağdaştırıcı her öğesi görünümü konumda ilişkilendirilecek bildiği `RecyclerView` veri kaynağındaki belirli bir konuma. Ayrıca, bağdaştırıcı her ayrı öğe görünümündeki içeriğinin düzenini işler ve her görünüm için Görünüm tutucu oluşturur. Bağdaştırıcıyı ayrıca öğesi görünümü tarafından algılanan öğeyi tıklama olaylarını bildirir.

-   **`RecyclerView.ViewHolder`** &ndash; Böylece kaynak aramaları gereksiz yere yinelenmediğinden öğesi düzeni dosyanızı görünümlerde başvurularını önbelleğe alır. Görünüm sahibi de bir kullanıcı görünümü sahibinin ilişkili öğesi görünümü dokunduğunda bağdaştırıcısına iletilecek öğesi tıklama olaylarını düzenler.

-   **`RecyclerView.LayoutManager`** &ndash; Öğeleri içinde konumlandırır `RecyclerView`. Çeşitli önceden tanımlanmış bir düzen yöneticileri birini kullanabilir veya kendi özel düzen manager uygulayabilirsiniz.
    `RecyclerView` Temsilciler, uygulamanızın önemli yapmak zorunda kalmadan farklı bir düzen Yöneticisi'nde takabilirsiniz. Bu nedenle Düzen Yöneticisi düzeni ilkeye değiştirir.

Ayrıca, isteğe bağlı olarak görünümünü değiştirmek için aşağıdaki sınıfları genişletebilirsiniz `RecyclerView` uygulamanızda:

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Değil genişlettiğinizde `ItemDecoration` ve `ItemAnimator`, `RecyclerView` varsayılan uygulamaları kullanır. Bu kılavuzda, özel oluşturma açıklamaz `ItemDecoration` ve `ItemAnimator` sınıfları; bu sınıflar hakkında daha fazla bilgi için bkz. [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) ve [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Nasıl çalıştığını geri dönüştürme görüntüleyin

`RecyclerView` veri kaynağındaki her öğe için bir öğesi görünümü bırakmaz. Bunun yerine, yalnızca ekranda uyan öğesi görüntüleme sayısını ayırır ve bu öğe düzenleri ve kullanıcı sonuçlarda kullanır. Görünüm ilk görüş dışına kaydırdığında, aşağıdaki şekilde gösterilen geri dönüşüm sürecinde geçer:

[![Görünüm geri dönüştürme altı adımlarını gösteren diyagram](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  Görüş dışına kaydırdığında ve artık görüntülenmez görünümü, bu duruma bir *ıskarta görünümü*.

2.  Atık görünümü bir havuzda yerleştirilir ve olur bir *görünümü geri dönüşüm*.
    Bu havuz, aynı türde verileri görüntüleyen görüşnümleri bir önbellektir.

3.  Görüntülenecek yeni bir öğe olduğunda, görünüm yeniden kullanım için geri kazanma havuz alınır. Bu görünüm bağdaştırıcı tarafından görüntülenmeden önce yeniden bağlanması gerekir çünkü adlı bir *kirli görünümü*.

4.  Kirli görünümü dönüştürülmeden: bağdaştırıcısı görüntülenecek sonraki öğe için verileri bulur ve bu öğe için görünümler bu veri kopyalar. Bu görünümler için başvuruları geri görünüm ile ilişkilendirilen görünüm sahibi alınır.

5.  Geri Dönüşüm görünüm öğeleri listesine eklenir `RecyclerView` olan ekran geçmek için.

6.  Geri Dönüşüm görünümü ve kullanıcı sonuçlarda ekran gider `RecyclerView` listesindeki bir sonraki öğeye. Bu arada, başka bir görünüme görüş dışına kaydırdığında ve yukarıdaki adımları göre dönüştürülmeden.

Öğesi görünümü yeniden yanı sıra `RecyclerView` başka bir verimlilik iyileştirme de kullanır: sahiplerini görüntüleme. A *görünümü sahibi* önbellekler başvurularla basit bir sınıf. Bir öğe Düzen dosyası bağdaştırıcısı Şişir her zaman karşılık gelen bir görünüm sahibi de oluşturur. Görünüm sahibi kullanan `FindViewById` görünümleri başvuruları içinde inflated öğesi Düzen dosyası almak için. Bu başvurular, yeni verileri göstermek için Düzen dönüştürülmeden her zaman görünümlere yeni verileri yüklemek için kullanılır.
 


### <a name="the-layout-manager"></a>Düzen Yöneticisi

Öğeleri konumlandırma için Düzen Yöneticisi sorumludur `RecyclerView` görüntüler; sunu türü (bir liste veya kılavuz), (olup olmadığını öğeleri görüntülenmiyor yatay veya dikey olarak) yönlendirmesini ve hangi yönde öğelerinin görüntülenmesi gereken belirler (normal sırayla veya ters sırada). Düzen Yöneticisi ayrıca boyutunu ve her öğenin konumunu hesaplamak için sorumlu olduğu **RecycleView** görüntüler.

Düzen Yöneticisi ek bir amacı vardır: artık kullanıcıya görünür olan öğe görünümleri geri dönüştürmek ne zaman İlkesi belirler.
Düzen yöneticisidir görünümleri görülebilir (ve olmayan) kullanan, ne zaman bir görünüm dönüştürülebilir karar vermek için en iyi konumda olmasıdır. Bir görünüm geri dönüştürmek için Düzen Yöneticisi genellikle farklı verilerle geri görünümü içeriğini değiştirmek için bağdaştırıcı daha önce açıklandığı gibi çağrılar [nasıl geri dönüştürme görünümü çalışır](#recycling).

Genişletebileceğiniz `RecyclerView.LayoutManager` kendi düzen oluşturmak için Yöneticisi veya bir önceden tanımlanmış bir düzen Yöneticisi'ni kullanabilirsiniz. `RecyclerView` Aşağıdaki önceden tanımlanmış bir düzen yöneticileri sağlar:

-   **`LinearLayoutManager`** &ndash; Dikey olarak kaydırılabilir bir sütun ya da yatay olarak kaydırılan bir satır öğeleri düzenler.

-   **`GridLayoutManager`** &ndash; Öğeleri bir kılavuz halinde görüntüler.

-   **`StaggeredGridLayoutManager`** &ndash; Öğeleri bazı öğeler farklı yükseklik ve genişlik sahip olduğu bir aşamalı kılavuz halinde görüntüler.

Düzen Yöneticisi belirtmek için seçilen Düzen yöneticinize örneği oluşturun ve geçirin `SetLayoutManager` yöntemi. Not aldığınız *gerekir* Düzen Yöneticisi belirtin &ndash; `RecyclerView` önceden tanımlanmış bir düzen manager varsayılan olarak seçin.

Düzen Yöneticisi hakkında daha fazla bilgi için bkz: [RecyclerView.LayoutManager sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).


### <a name="the-view-holder"></a>Görünüm sahibi

Görünüm sahibi görünümü başvurularını önbelleğe almak için tanımlayan bir sınıftır. Bağdaştırıcı her görünüm içeriğine bağlamak için bu görünümü başvuruları kullanır. Her bir öğe `RecyclerView` görünümü başvuruları bu öğe için önbelleğe alan bir ilişkili görünüm sahibi örneği vardır. Bir görünümü sahibi oluşturmak için setini görünümleri her öğe için bir sınıf tanımlamak için aşağıdaki adımları kullanın:

1.  Alt `RecyclerView.ViewHolder`.
2.  Arar ve görünümü başvurularını depolayan bir yapıcısını uygular.
3.  Bağdaştırıcı, bu başvuruları erişmek için kullanabileceğiniz özellikler uygular.

Ayrıntılı bir örneği bir `ViewHolder` uygulama sunulur [bir temel RecyclerView örneği](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Hakkında daha fazla bilgi için `RecyclerView.ViewHolder`, bkz: [RecyclerView.ViewHolder sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="the-adapter"></a>Bağdaştırıcı

"Ağır-kaldırarak", çoğu `RecyclerView` tümleştirme kodunda bağdaştırıcısı gerçekleşir. `RecyclerView` türetilen bir bağdaştırıcı sağlamanızı ister `RecyclerView.Adapter` , veri kaynağına erişmek ve veri kaynağından içerik her bir öğeyle doldurmak için.
Veri kaynağı, uygulamaya özgü olduğundan, verilerinize erişmek nasıl anlayan bağdaştırıcısı işlevselliği uygulamalıdır. Bağdaştırıcı veri kaynağından bilgi ayıklar ve bunu her öğe yükler `RecyclerView` koleksiyonu.

Bir veri kaynağı görünümü sahiplerini aracılığıyla içeriğinde bağdaştırıcı her satır öğesi içinde tek bir görünüm için nasıl eşlendiğini aşağıdaki çizimde gösterilmektedir `RecyclerView`:

[![Veri kaynağına bağlanmak için ViewHolders bağdaştırıcısı gösteren diyagram](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

Her bağdaştırıcı yükler `RecyclerView` belirli bir satır öğesi için verileri içeren satır. Satır konumu için *P*, örneğin, bağdaştırıcı konumunda ilişkili verileri bulur *P* kopyalar ve veri kaynağı içinde bu verileri satır öğesi konumunda *P* içinde `RecyclerView` koleksiyonu.
Yukarıdaki çizimde, örneğin, bağdaştırıcı görünümü sahibi için başvurular aramak için kullanır `ImageView` ve `TextView` tekrar tekrar çağırmak zorunda değildir, bu nedenle, ilgili konumdaki `FindViewById` bu görünümler kullanıcı için koleksiyonda kayan ve görünümleri yeniden kullanır.

Bir bağdaştırıcı uygularken aşağıdaki geçersiz kılmanız gerekir `RecyclerView.Adapter` yöntemleri:

-   **`OnCreateViewHolder`** &ndash; Dosya ve görünüm öğesi düzeni sahibi başlatır.

-   **`OnBindViewHolder`** &ndash; Belirli bir konumda veri içinde belirli görünüm sahibi olan başvuruları depolanan görünümleri yükler.

-   **`ItemCount`** &ndash; Veri kaynağındaki öğe sayısını döndürür.

Bu öğeleri konumlandırma sırasında düzen Yöneticisi bu yöntemleri çağıran `RecyclerView`. 



### <a name="notifying-recyclerview-of-data-changes"></a>Veri değişikliği RecyclerView bildirme

`RecyclerView` değişiklik verilerini içeriğini kaynaktaki görünümünü otomatik olarak güncelleştirilmez; Bağdaştırıcı bilgilendirmelisiniz `RecyclerView` veri kümesindeki bir değişiklik olduğunda. Veri kümesi, birçok şekilde değiştirebilirsiniz. Örneğin, bir öğenin içeriği değiştirebilir veya genel verilerin yapısı değiştirilebilir.
`RecyclerView.Adapter` çeşitli çağırabileceğiniz yöntemler sağlar. böylece `RecyclerView` veri değişikliklerini en verimli şekilde yanıt verir:

-  **`NotifyItemChanged`** &ndash; Belirtilen konumdaki öğe değiştirildi sinyaller.

-  **`NotifyItemRangeChanged`** &ndash; Belirtilen aralıktaki öğeler konumlar değişmiş sinyaller.

-  **`NotifyItemInserted`** &ndash; Belirtilen konuma öğesinde yeni eklenen olduğunu bildirir.

-  **`NotifyItemRangeInserted`** &ndash; Belirtilen aralıktaki öğeler konumları yeni eklenmiş sinyaller.

-  **`NotifyItemRemoved`** &ndash; Sinyaller belirtilen konumda öğesi kaldırıldı.

-  **`NotifyItemRangeRemoved`** &ndash; Sinyaller konumları Belirtilen aralıktaki öğeler kaldırıldı.

-  **`NotifyDataSetChanged`** &ndash; Veri kümesi değişti sinyaller (tam güncelleştirme zorlar).

Veri kümeniz tam olarak nasıl değiştiğini biliyorsanız, yenilemek için yukarıdaki uygun yöntemleri çağırabilir `RecyclerView` en verimli şekilde. Veri kümeniz tam olarak nasıl değiştiğini bilmiyorsanız çağırabilirsiniz `NotifyDataSetChanged`, çok daha verimli olan çünkü `RecyclerView` kullanıcıya görünür olan tüm görünümleri yenilemeniz gerekir. Bu yöntemler hakkında daha fazla bilgi için bkz. [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

Bir sonraki konu başlığında [bir temel RecyclerView örneği](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), örnek uygulama bölümleri ve İşlevler yukarıda özetlenen gerçek kod örnekleri göstermek için uygulanır.


## <a name="related-links"></a>İlgili bağlantılar

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Basit bir RecyclerView örneği](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView örneği genişletme](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
