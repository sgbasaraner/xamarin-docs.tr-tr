---
title: ListView bölümleri ve İşlevler
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: ec9b6583cac9478957e14f709c7c7ee8eb273cbc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763988"
---
# <a name="listview-parts-and-functionality"></a>ListView bölümleri ve İşlevler


## <a name="overview"></a>Genel Bakış

A `ListView` aşağıdaki bölümlerden oluşur:

- **Satır** &ndash; veri listesinde görünür gösterimi.

- **Bağdaştırıcı** &ndash; liste görünümüne veri kaynağına bağlar bir görsel olmayan sınıf.

- **Hızlı kaydırma** &ndash; listenin uzunluğu kaydırma kullanıcı olanak sağlayan bir tanıtıcı.

- **Dizin bölümü** &ndash; kaydırma üzerinden gezinen bir kullanıcı arabirimi öğesi satırları geçerli satır listede nerede belirtmek için.

Bu ekran görüntüleri temel kullanmak `ListView` hızlı kaydırma ve bölüm dizini nasıl işlendiğini göstermek için denetimi:

[![Düz eski satırları kullanarak uygulamaları ekran görüntüleri hızlı kaydırma ve bölüm dizini](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

Oluşturan öğeler bir `ListView` aşağıdaki daha ayrıntılı olarak açıklanmıştır:


## <a name="rows"></a>Satırları

Her bir satır kendi sahip `View`. Görünüm tanımlanan yerleşik görünümleri birini olabilir `Android.Resources`, ya da özel bir görünüm. Her satır aynı görünümü Düzen kullanabilirsiniz veya tüm farklı olabilir. Bu belgede yerleşik düzenleri ve diğerleri özel düzenler tanımlamak nasıl açıklayan kullanmanın örnekler verilmiştir.


## <a name="adapter"></a>Bağdaştırıcı

`ListView` Denetim gerektiren bir `Adapter` biçimlendirilmiş sağlamak için `View` her satır için. Android yerleşik bağdaştırıcılar ve kullanılabilir görünümleri olan veya özel sınıflar oluşturulabilir.


## <a name="fast-scrolling"></a>Hızlı kaydırma

Zaman bir `ListView` satır sayısını içeren verilerin hızlı kaydırma listenin herhangi bir kısmını gidin kullanıcının yardımcı olmak için etkinleştirilebilir. Hızlı kaydırma 'kaydırma çubuğu' isteğe bağlı olarak etkin (ve API düzeyinde 11 özelleştirilmiş ve daha yüksek) olabilir.


## <a name="section-index"></a>Bölüm dizini

Uzun listeleriyle kaydırma sırasında isteğe bağlı bir bölüm dizin geri bildirim kullanıcıyla listesinin hangi bölümü şu anda görüntüledikleri üzerinde sağlar. Hızlı kaydırma ile birlikte genellikle uzun listelerde yalnızca uygundur.


## <a name="classes-overview"></a>Sınıfları genel bakış

Görüntülemek için kullanılan birincil sınıfların `ListViews` burada gösterilir:

[![ListView ve ilişkili sınıflar arasındaki ilişkileri gösteren UML diyagram](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

Her sınıf amacı, aşağıda belirtilmiştir:

- **ListView** &ndash; satır kaydırılabilir koleksiyonu görüntüler kullanıcı arabirimi öğesi. Üzerinde telefonlar, genellikle tüm ekran yukarı kullanır (; Bu durumda `ListActivity` sınıfı kullanılabilir) veya daha büyük bir düzen telefon veya tablet cihazlarda parçası olabilir.

- **Görünüm** &ndash; Android bir görünümde herhangi bir kullanıcı arabirimi öğesi olabilir ancak bağlamında bir `ListView` gerektirdiği bir `View` her satır için sağlanacak.

- **BaseAdapter** &ndash; bağlamak bağdaştırıcı uygulamaları için temel sınıf bir `ListView` bir veri kaynağına.

- **ArrayAdapter** &ndash; için bir dizeler dizisi bağlar yerleşik bağdaştırıcı sınıfı bir `ListView` görüntülemek için. Genel `ArrayAdapter<T>` aynı için diğer türlerle eşleşmiyor.

- **CursorAdapter** &ndash; kullanım `CursorAdapter` veya `SimpleCursorAdapter` bir SQLite sorguya dayalı verileri görüntülemek için.

Bu belgeyi kullanmak basit örnekler içeren bir `ArrayAdapter` özel uygulamaları gerektiren daha karmaşık örnekler yanı sıra `BaseAdapter` veya `CursorAdapter`.

