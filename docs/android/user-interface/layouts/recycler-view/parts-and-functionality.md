---
title: RecyclerView bölümleri ve İşlevler
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 44f042359c9363f8ec3008ee49d2f6a874983e12
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771216"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView bölümleri ve İşlevler


`RecyclerView` Bazı görevler dahili (kaydırma ve görünümlerini geri dönüştürme gibi), ancak tanıtıcıları yardımcı sınıfları koleksiyonu görüntülenecek koordinatları Yöneticisi temelde olur. `RecyclerView` Aşağıdaki yardımcı sınıfları görevler atar:

-   **`Adapter`** &ndash; Öğe düzenleri Şişir (düzeni dosyasının içeriğini başlatır) ve veri içinde görüntülenen görünümlere bağlar bir `RecyclerView`. Bağdaştırıcı da öğesi tıklama olaylarını bildirir.

-   **`LayoutManager`** &ndash; Ölçer ve madde görünümler içinde konumlandırır bir `RecyclerView` ve görünüm geri dönüştürme İlkesi yönetir.

-   **`ViewHolder`** &ndash; Arar ve görünümü başvuruları depolar. Görünüm sahibi öğesi görünümü tıklama algılama ile yardımcı olur.

-   **`ItemDecoration`** &ndash; Özel çizim ve düzeni uzaklıkları Bölücü çizim öğeleri, vurgular ve görsel gruplandırma sınırlar arasında belirli görünümler eklemek bir uygulama sağlar.

-   **`ItemAnimator`** &ndash; Öğesi eylemleri sırasında gerçekleşmesi ya da bağdaştırıcı için yapılan değişiklikler animasyonları tanımlar.

Arasındaki ilişkiyi `RecyclerView`, `LayoutManager`, ve `Adapter` sınıfları aşağıdaki çizimde gösterilen:

![LayoutManager içeren, veri kümesi erişmek için bağdaştırıcı kullanılarak RecyclerView diyagramı](parts-and-functionality-images/01-recyclerview-diagram.png)

Bu şekilde gösterildiği gibi `LayoutManager` , arasında aracı olarak düşünülebilir `Adapter` ve `RecyclerView`. `LayoutManager` İçine çağrılar `Adapter` adına yöntemleri `RecyclerView`. Örneğin, `LayoutManager` çağrıları bir `Adapter` maddeyi konumda için yeni bir görünüm oluşturmak için süre olduğunda yöntemi `RecyclerView`. `Adapter` Oluşturur ve bu öğe için Düzen Şişir bir `ViewHolder` önbellek başvuruları bu konumda görünümlere örneğine (gösterilmez). Zaman `LayoutManager` çağrıları `Adapter` belirli bir öğe veri kümesine bağlanmak için `Adapter` bu öğe için verileri bulur, veri kümesinden alır ve ilişkili öğe görünümüne kopyalar.

Kullanırken `RecyclerView` aşağıdaki sınıfların türetilmiş türler oluşturma, uygulamanızda gereklidir:

-   **`RecyclerView.Adapter`** &ndash; İçinde görüntülenen öğe görünümlere (, uygulamanıza özgü olan), uygulamanızın veri kümesindeki bir bağlama sağlar `RecyclerView`. Bağdaştırıcı her öğe görünümü konumda ilişkilendirilecek bildiği `RecyclerView` veri kaynağındaki belirli bir konuma. Ayrıca, bağdaştırıcı her tek tek öğe görünümü içinde içeriğinin düzenini işler ve her görünüm için Görünüm sahibi oluşturur. Bağdaştırıcı da madde görünüm tarafından algılanan öğeyi tıklama olaylarını bildirir.

-   **`RecyclerView.ViewHolder`** &ndash; Böylece kaynak aramalarını gereksiz yere yinelenmeyecek öğesi düzeni dosyanızı görünümlerde başvurularını önbelleğe alır. Görünüm sahibi bir kullanıcı görünüm sahibinin ilişkili öğe görünümü dokunur zaman bağdaştırıcıya iletilecek öğesi tıklama olaylarını de düzenler.

-   **`RecyclerView.LayoutManager`** &ndash; İçindeki öğeleri konumlandırır `RecyclerView`. Birçok önceden tanımlı düzeni yöneticileri birini kullanabilir veya kendi özel düzen Yöneticisi uygulayabilirsiniz.
    `RecyclerView` Temsilciler, uygulamanızın önemli yapmak zorunda kalmadan farklı düzen Yöneticisi'nde ekleyebilirsiniz şekilde Düzen Yöneticisi düzeni ilkeyi değiştirir.

Ayrıca, isteğe bağlı olarak görünüm ve yapısını değiştirmek için aşağıdaki sınıflar genişletebilirsiniz `RecyclerView` uygulamanızda:

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

Değil genişlettiğinizde `ItemDecoration` ve `ItemAnimator`, `RecyclerView` varsayılan uygulamaları kullanır. Bu kılavuz özel oluşturmak nasıl açıklamaz `ItemDecoration` ve `ItemAnimator` sınıfları; bu sınıfları hakkında daha fazla bilgi için bkz: [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) ve [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Nasıl çalışır geri dönüştürme görüntüleyin

`RecyclerView` veri kaynağındaki her öğe için bir öğe görünümü bırakmaz. Bunun yerine, yalnızca ekranda sığacak madde görünüm sayısını ayırır ve bu öğe düzenleri kullanıcı kayarken olarak yeniden kullanır. Görünüm ilk görüş dışına kaydırdığında aşağıdaki şekilde gösterilen geri dönüştürme süreci başlatılır:

[![Görünüm geri dönüştürme altı adım gösteren diyagram](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  Bir görünüm kayar dışında görüş ve artık görüntülenir, bu duruma bir *hurda Görünüm*.

2.  Atık görünümü bir havuzda yerleştirilir ve hale bir *görünüm geri dönüşüm*.
    Bu havuz aynı türde veri görüntüleyen veri görünümleri, bir önbelleğidir.

3.  Yeni bir öğe görüntülenecek olduğunda, görünüm yeniden kullanım için Geri Dönüşüm havuzundan alınır. Bu görünüm bağdaştırıcı tarafından görüntülenmeden önce yeniden bağlanması gerekir çünkü adlı bir *kirli Görünüm*.

4.  Kirli görünüm dönüştürülmeden: Bağdaştırıcı görüntülenecek sonraki öğe için verileri bulur ve bu veriler için bu öğeyi görünümleri kopyalar. Bu görünümler için başvuruları geri dönüştürüldüğünde görünüm ile ilişkilendirilen görünüm sahibi alınır.

5.  Geri dönüştürüldüğünde görünüm öğelerinin listesine eklenir `RecyclerView` olan ekran geçmek için.

6.  Geri dönüştürüldüğünde görünüm kullanıcı kayarken ekran gider `RecyclerView` listesindeki bir sonraki öğeye. Bu sırada, başka bir görünüm görüş dışına kaydırdığında ve yukarıdaki adımları göre geri dönüştürüldüğünde.

Öğe görünümü yeniden yanı sıra `RecyclerView` de başka bir verimlilik iyileştirme kullanır: sahiplerini görüntüleyin. A *görünüm sahibi* önbellekleri başvurularla basit bir sınıf. Bağdaştırıcı bir madde düzeni dosyası Şişir her zaman karşılık gelen bir görünüm sahibi da oluşturur. Görünüm sahibi kullanan `FindViewById` içinde inflated öğesi düzenini dosya görünümleri başvurularını almak için. Bu başvuruların yeni verileri göstermek için Düzen dönüştürülmeden her zaman yeni verileri görünümlere yüklemek için kullanılır.
 


### <a name="the-layout-manager"></a>Düzen Yöneticisi

Öğeleri konumlandırma için Düzen Yöneticisi sorumludur `RecyclerView` görüntüler; sunu türünü (bir liste veya kılavuz), (öğeleri görüntülenip görüntülenmeyeceğini dikey veya yatay) yönlendirme ve yönü öğeleri görüntüleneceğini belirler (normal sırayla veya ters sırada). Düzen Yöneticisi ayrıca boyutunu ve konumunu her öğenin hesaplamak için sorumlu olduğu **RecycleView** görüntüler.

Düzen Yöneticisi ek bir amacı vardır: zaman artık kullanıcı tarafından görülebilir öğesi görünümleri geri dönüştürmek için ilke belirler.
Düzen Yöneticisi görünümleri görünür (ve olmayan) kullanan olduğundan, ne zaman bir görünüm dönüştürülebilir karar vermek için en iyi konumunda değildir. Bir görünüm geri dönüştürmek için Düzen Yöneticisi genellikle farklı veriler ile geri dönüştürüldüğünde bir görünümü içeriğini değiştirmek için bağdaştırıcı daha önce açıklandığı gibi çağrılar [çalışır Görünümü'ne geri dönüştürme](#recycling).

Genişletebilirsiniz `RecyclerView.LayoutManager` kendi düzeni oluşturmak için Yöneticisi veya bir önceden tanımlanmış düzen Yöneticisi'ni kullanabilirsiniz. `RecyclerView` Aşağıdaki önceden tanımlanmış düzen yöneticilerine sağlar:

-   **`LinearLayoutManager`** &ndash; Öğeleri dikey olarak kaydırılabilir bir sütun veya yatay olarak kaydırılabilir bir satırda düzenler.

-   **`GridLayoutManager`** &ndash; Kılavuz öğeleri görüntüler.

-   **`StaggeredGridLayoutManager`** &ndash; Öğeleri bazı öğeler farklı yükseklik ve genişlik sahip olduğu düzenlenmiş kılavuzunda görüntüler.

Düzen Yöneticisi belirtmek için seçilen Düzen Yöneticisi örneği ve ona geçirmek `SetLayoutManager` yöntemi. Not aldığınız *gerekir* Düzen Yöneticisi'ni belirtin &ndash; `RecyclerView` önceden tanımlanmış düzen Yöneticisi varsayılan olarak seçin.

Düzen Yöneticisi hakkında daha fazla bilgi için bkz: [RecyclerView.LayoutManager sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html).


### <a name="the-view-holder"></a>Görünüm sahibi

Görünüm sahibi görünüm başvurularını önbelleğe almak için tanımlayan bir sınıftır. Bağdaştırıcı bu görünüm başvuruları içeriğine her görünüm bağlamak için kullanır. Her bir öğe `RecyclerView` bu öğe için Görünüm başvurularını önbelleğe alan bir ilişkili görünüm sahibi örneği vardır. Bir görünüm sahibi oluşturmak için görünümler her öğe kümesini tam tutmak için bir sınıf tanımlamak için aşağıdaki adımları kullanın:

1.  Bir alt kümesi `RecyclerView.ViewHolder`.
2.  Arar ve görünümü başvurularını depolayan bir oluşturucu uygulayın.
3.  Bağdaştırıcı bu başvuruları erişmek için kullanabileceğiniz özellikler uygular.

Ayrıntılı bir örnek olarak bir `ViewHolder` uygulama sunulur [A temel RecyclerView örnek](~/android/user-interface/layouts/recycler-view/recyclerview-example.md).
Hakkında daha fazla bilgi için `RecyclerView.ViewHolder`, bkz: [RecyclerView.ViewHolder sınıf başvurusu](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="the-adapter"></a>Bağdaştırıcı

"Ağır-kaldırma", çoğu `RecyclerView` tümleştirme kodu bağdaştırıcısında kurulur. `RecyclerView` türetilen bir bağdaştırıcı sağlamanızı ister `RecyclerView.Adapter` veri kaynağına erişmek ve veri kaynağından içerik her bir öğeyle doldurmak için.
Veri kaynağı uygulamaya özgü olduğundan, verilerinize erişmek nasıl anlar bağdaştırıcısı işlevselliği uygulamalıdır. Bağdaştırıcı veri kaynağından bilgileri ayıklar ve her öğe yükler `RecyclerView` koleksiyonu.

Aşağıdaki çizim nasıl bağdaştırıcı bir veri kaynağı görünümü sahiplerini aracılığıyla içeriği tek bir görünüm içindeki her satır öğesinin eşlendiği gösterilmektedir `RecyclerView`:

[![ViewHolders için veri kaynağına bağlanma bağdaştırıcısı gösteren diyagram](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

Her bağdaştırıcı yükler `RecyclerView` belirli bir satır öğesi için veri satırı. Satır konumu için *P*, örneğin, bağdaştırıcı konumunda ilişkili verileri bulur *P* kopyalar ve veri kaynağı içinde satır bu veri öğesi konumunda *P* içinde `RecyclerView` koleksiyonu.
Yukarıdaki çizimde, örneğin, bağdaştırıcı görünüm sahibi başvurularını aramak için kullanan `ImageView` ve `TextView` tekrar tekrar çağırmak yok şekilde bu konumda `FindViewById` bu görünümler kullanıcı için toplulukta birlikte kayar ve görünümleri yeniden kullanır.

Bir bağdaştırıcı uyguladığınızda, aşağıdaki geçersiz kılmanız gerekir `RecyclerView.Adapter` yöntemleri:

-   **`OnCreateViewHolder`** &ndash; Öğe düzeni dosya ve görünüm sahibi başlatır.

-   **`OnBindViewHolder`** &ndash; Belirli bir konumda veri içinde belirli görünüm sahibi olan başvurular depolanan görünümleri yükler.

-   **`ItemCount`** &ndash; Veri kaynağında öğe sayısını döndürür.

İçindeki öğeleri konumlandırma sırasında düzen Yöneticisi bu yöntemleri çağırır `RecyclerView`. 



### <a name="notifying-recyclerview-of-data-changes"></a>Veri değişikliklerinin RecyclerView bildirme

`RecyclerView` otomatik olarak kendi veri içeriğini değişiklikleri kaynak olduğunda görünümünü güncelleştirmez; Bağdaştırıcı üzerindedir `RecyclerView` veri kümesindeki bir değişiklik olduğunda. Veri kümesi birçok yolla değiştirebilirsiniz; Örneğin, bir öğenin içinden içeriği değiştirebilir veya genel yapısı verilerin değiştirilebilir.
`RecyclerView.Adapter` çeşitli çağırabilirsiniz yöntemler sağlar böylece `RecyclerView` veri değişikliklerini en verimli şekilde yanıt verir:

-  **`NotifyItemChanged`** &ndash; Belirli bir konumda öğesi değişti sinyaller.

-  **`NotifyItemRangeChanged`** &ndash; Konumlar Belirtilen aralıktaki öğeler değişti sinyaller.

-  **`NotifyItemInserted`** &ndash; Belirtilen konuma öğesinde yeni eklenen olduğunu bildirir.

-  **`NotifyItemRangeInserted`** &ndash; Konumlar Belirtilen aralıktaki öğelerinin yeni eklenmiş sinyaller.

-  **`NotifyItemRemoved`** &ndash; Belirtilen konumda öğesi kaldırıldıktan sinyaller.

-  **`NotifyItemRangeRemoved`** &ndash; Sinyaller konumlar Belirtilen aralıktaki öğeler kaldırıldı.

-  **`NotifyDataSetChanged`** &ndash; Veri kümesi değişti sinyaller (tam güncelleştirme zorlar).

Tam olarak veri kümenizi nasıl değiştiğini biliyorsanız, yenilemek için yukarıdaki uygun yöntemleri çağırabilir `RecyclerView` en verimli şekilde. Tam olarak veri kümenizi nasıl değiştiğini bilmiyorsanız çağırabilirsiniz `NotifyDataSetChanged`, çok daha az verimli olduğu çünkü `RecyclerView` kullanıcıya görünür olan tüm görünümleri yenilemeniz gerekir. Bu yöntemler hakkında daha fazla bilgi için bkz: [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).

Sonraki konusunda [A temel RecyclerView örnek](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), bir örnek uygulamayı gerçek kod örnekleri yukarıda özetlenen işlevselliği ve bölümleri göstermek için uygulanır.


## <a name="related-links"></a>İlgili bağlantılar

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Temel bir RecyclerView örneği](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView örnek genişletme](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
