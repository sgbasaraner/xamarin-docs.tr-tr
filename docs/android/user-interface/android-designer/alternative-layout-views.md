---
title: Alternatif Düzen görünümleri
description: Bu konuda nasıl düzenleri kaynak niteleyicileri kullanarak sürümlü olabilir açıklanmaktadır. Örneğin, cihaz yatay modda olduğunda, yalnızca kullanılan bir düzen sürümünü ve olabilir yalnızca dikey modu için bir düzen sürümü.
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: d2228169ed5d8575c9e332c85d963fca0400dea8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="alternative-layout-views"></a>Alternatif Düzen görünümleri

_Bu konuda nasıl düzenleri kaynak niteleyicileri kullanarak sürümlü olabilir açıklanmaktadır. Örneğin, cihaz yatay modda olduğunda, yalnızca kullanılan bir düzen sürümünü ve olabilir yalnızca dikey modu için bir düzen sürümü._


## <a name="creating-alternative-layouts"></a>Alternatif düzenleri oluşturma

Tıkladığınızda **alternatif düzeni görünümü** simgesi (sol tarafındaki **aygıt**), projenize kullanılabilir alternatif düzenleri listelemek için Önizleme bölmesini açar. Hiçbir alternatif düzenleri varsa **varsayılan** görünümü sunulur: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alternatif düzeni Görünüm bölmesi](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "alternatif düzeni Görünüm bölmesi")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Alternatif düzeni Görünüm bölmesi](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

-----

Tıkladığınızda yeşil artı yanına **yeni sürümü**, **oluşturma düzeni değişim** bu düzeni değişim için kaynak niteleyicileri seçebilmeniz için iletişim kutusunu açar: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Düzen Değişimi Oluştur](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "düzeni Değişimi Oluştur")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Düzen Değişimi Oluştur](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

-----


Aşağıdaki örnekte, kaynak niteleyicisi **ekran yönünü** ayarlanır **yatay**ve **ekran boyutu** değiştirilir **büyük**. Bu adlı yeni bir düzen sürümü oluşturur **kara büyük**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Büyük kara değişim](alternative-layout-views-images/vs/03-large-land-sml.png "büyük kara değişim")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Büyük kara değişim](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

-----


Soldaki önizleme bölmesinde kaynak niteleyicisi seçimleri etkilerini görüntüleyeceğini unutmayın. Tıklatarak **Ekle** alternatif düzeni oluşturur ve bu düzene Tasarımcı geçer. **Alternatif görünümü** önizleme bölmesinde gösterir hangi düzenin küçük sağ işaretçi aracılığıyla tasarımcısı içine aşağıdaki ekran görüntüsünde gösterildiği gibi yüklenir: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Yüklenen düzeni göstergesi](alternative-layout-views-images/vs/04-new-layout-sml.png "yüklenen düzeni göstergesi")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Yüklenen düzeni göstergesi](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

-----



## <a name="editing-alternative-layouts"></a>Alternatif düzenleri düzenleme

Alternatif düzenleri oluşturduğunuzda, genellikle bir düzen forked tüm sürümleri için geçerlidir tek bir değişiklik yapmanız önerilir. Örneğin, tüm düzenleri sarıya düğme metni değiştirmek isteyebilirsiniz. Çok sayıda düzenleri varsa, hızlı bir şekilde zahmetli ve hatalara eğilimli tüm sürümler için tek bir değişiklik yaymak gerekiyorsa bakım haline gelebilir. 

Birden çok Düzen sürümlerinin bakım basitleştirmek için tasarımcı sağlar bir **çok Düzenle** değişikliklerinizi bir veya daha fazla düzenleri arasında yayar modu. Birden fazla düzeni bulunduğunda **çok Düzenle** simgesi görüntülenir: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Çok Düzenle simgesine](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "çok Düzenle simgesi")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Çok Düzenle simgesi](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

-----


Tıkladığınızda **çok Düzenle** simge, satırları düzenleri (aşağıda gösterildiği gibi) bağlantılı olduğunu gösteren görüntülenir; diğer bir deyişle, bir düzene bir değişiklik yaptığınızda bu değişiklik için bağlantılı düzenleri yayılır. Aşağıdaki ekran görüntüsünde gösterilen daire içinde simgesini tıklatarak tüm düzenleri bağlantısını kaldırabilirsiniz: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Tüm düzenleri bağlantısını](alternative-layout-views-images/vs/06-multi-linked-sml.png "tüm düzenleri bağlantısını Kaldır")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Tüm düzenleri bağlantısını Kaldır](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

-----


İkiden fazla düzenleri varsa, hangi düzenleri birbirine bağlı belirlemek için her Yerleşim Önizleme solundaki Düzenle düğmesini seçerek geçiş yapabilirsiniz. Örneğin, yayar tek bir değişiklik yapmak istiyorsanız ilk ve son üç düzen için ilk Orta düzeni aşağıda gösterildiği gibi bağlantısını: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Bağlantıyı kaldır Orta düzeni](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "Bağlantıyı Kes'i Orta düzeni")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Orta düzeni bağlantısını Kaldır](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)
 
-----
 

Bu örnekte, bir değişiklik yapıldığında olarak **varsayılan** veya **uzun** diğer düzen ancak değil düzeni yayılır **kara büyük** düzeni. 



### <a name="multi-edit-example"></a>Çok düzenleme örneği 

Genel olarak, bir düzene bir değişiklik yaptığınızda bu aynı değişikliği diğer tüm bağlı düzenleri yayılır. Örneğin, yeni bir ekleme `TextView` pencere öğesi için **varsayılan** düzeni ve kendi metin değiştirme dizesi için `Portrait` tüm bağlı düzenleri yapılması aynı değişikliğe neden. İşte nasıl göründüğünü **varsayılan** düzeni: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kutusu TextView ekleme](alternative-layout-views-images/vs/08-add-textview-sml.png "kutusu TextView Ekle")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kutusu TextView Ekle](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)
 
-----
 

`TextView` De eklenir **kara büyük** düzeni görüntülemek için bağlı olduğundan **varsayılan** düzeni: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Yatay kutusu TextView](alternative-layout-views-images/vs/09-landscape-textview-sml.png "yatay kutusu TextView")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Yatay kutusu TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)
 
-----
 

Ancak yalnızca bir düzene yerel bir değişiklik yapmak isterseniz (diğer bir deyişle, herhangi bir başka düzenleri için dağıtılmasını değişiklik istemediğiniz)? Bunu yapmak için sonraki açıklandığı gibi değiştirmeden önce değiştirmek istediğiniz düzeni bağlantısını gerekir. 



### <a name="making-local-changes"></a>Yerel değişiklikler yapma 

Eklenen sağlamak için her iki düzende istiyoruz varsayalım `TextView`, ancak biz de metin dizesinde değiştirmek istediğiniz **kara büyük** düzene `Landscape` yerine `Portrait`. Biz bu değişikliği yaparsanız **kara büyük** her iki düzende bağlı olsa da, değişikliği geri yayılır **varsayılan** düzeni. Biz değişikliği yapmadan önce bu nedenle, biz ilk iki düzenleri bağlantısını gerekir. Metinde biz değiştirmek zaman **kara büyük** için `Landscape`, bu değişikliği yerel olduğunu belirtmek için kırmızı bir kare değişiklikle Tasarımcı işaretler **kara büyük** düzeni ve *değil* geri yayılmadan **varsayılan** düzeni: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Yerel değişiklik](alternative-layout-views-images/vs/10-local-change-sml.png "yerel Değiştir")](alternative-layout-views-images/vs/10-local-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Yerel Değiştir](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)
 
-----
 

Tıkladığınızda **varsayılan** , görüntülemek için Düzen `TextView` metin dizesi ayarlanmış hala `Portrait`. 



## <a name="handling-conflicts"></a>Çakışmalarını işleme 

İçindeki metnin rengi değiştirmeye karar **varsayılan** yeşil düzeni, bağlantılı düzeni görünür bir uyarı simgesi göreceksiniz. Bu düzen tıklatarak çakışması ortaya çıkarmak için Düzen açar. Çakışma nedeniyle pencere öğesi ile bir kırmızı kare vurgulanır ve aşağıdaki ileti görüntülenir: *son değişiklikler bu alternatif yerleşiminde çakışmalar olmuş*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Çakışan değişiklik](alternative-layout-views-images/vs/11-conflicting-change-sml.png "çakışan değişiklik")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Çakışan değişiklik](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)
 
-----
 

A *çakışma kutusunu* çakışma açıklamak için sağdaki pencere öğesinin görüntülenir: 

[![Çakışma Uyarısı](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

Çakışma kutusu değiştirilmiş özellikler listesini gösterir ve bunların değerleri listeler. Tıklatarak **yoksay çakışma** özellik değişikliği yalnızca bu pencere öğesi için geçerlidir. Tıklatarak **Uygula** Bu pencere öğesi de karşılık gelen pencere öğesinde bağlantılı konusunda özellik değişikliği uygulandığı **varsayılan** düzeni. Tüm özellik değişikliklerini uyguladıysanız, çakışma otomatik olarak atılır. 


### <a name="view-group-conflicts"></a>Görünüm Grup çakışmaları 

Özellik değişiklikleri çakışmaları yalnızca kaynağı değildir. Çakışmaları ekleme veya pencere öğeleri kaldırırken algılanabilir. Örneğin, **kara büyük** Düzen bağlantısız **varsayılan** düzeni ve `TextView` içinde **kara büyük** düzeni sürüklenip yukarıda bırakılmasına `Button`, çakışma belirtmek için taşınan pencere öğesi Tasarımcı işaretler:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Grup çakışma görüntülemek](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "Grup çakışma görüntüleyin")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Görünüm Grup çakışması](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)
 
-----
 

Ancak, olduğundan hiçbir işaret `Button`. Ancak konumunu `Button` değişti, `Button` özel uygulanan değişiklik gösterir **kara büyük** yapılandırma. 

Varsa bir `CheckBox` eklenen **varsayılan** düzeni, başka bir çakışma oluşturulur ve üzerinden bir uyarı simgesi görüntülenir **kara büyük** düzeni: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Onay kutusu çakışma](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "onay kutusunu çakışması")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Onay kutusu çakışması](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)
 
-----
 

Tıklatarak **kara büyük** düzeni çakışması ortaya çıkarır. Aşağıdaki ileti görüntülenir: *son değişiklikler bu alternatif yerleşiminde çakışmalar olmuş*. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alt Düzen Çakışma](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "Alt düzeni çakışması")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Alt düzeni çakışması](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)
 
-----
 

Ayrıca, çakışma kutusu aşağıdaki iletiyi görüntüler:

[![Çakışma iletisi](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

Ekleme `CheckBox` bir çakışma neden olur **kara büyük** düzeni cinsinden değişiklikleri olan `LinearLayout` , içerir. Ancak, bu durumda çakışma kutusunu yalnızca içine eklenen pencere öğesi görüntüler **varsayılan** Düzen ( `CheckBox`).

Tıklatırsanız **yoksay çakışma**, pencere öğesi olduğu eksik sürüklenen ve düzeni bırakılan çakışma kutusunda görüntülenen pencere öğesi izin vererek çakışma Tasarımcı çözümler (Bu durumda, **kara büyük**  düzeni): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Grup çakışma](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "Grup çakışma")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Grup çakışma](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)
 
-----
 

İle önceki örnekte görüldüğü gibi `Button`, `CheckBox` kırmızı değişiklik işareti olduğundan yok yalnızca `LinearLayout` içinde uygulanan değişiklikler sahip **kara büyük** düzeni.



### <a name="conflict-persistence"></a>Çakışma kalıcılığı

Çakışmaları düzeni dosyasında XML açıklamaları aşağıda gösterildiği gibi kalıcı:

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

Bu nedenle, bir projeyi yeniden açılacak ve kapandığında tüm çakışmaları hala olması var. &ndash; bile yok sayıldı olanlar.

