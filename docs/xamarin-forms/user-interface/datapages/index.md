---
title: Xamarin.Forms DataPages
description: Bu makale, bir API için hızlı bir şekilde sağlar ve bir veri kaynağı önceden oluşturulmuş görünümler için kolayca bağlama Xamarin.Forms DataPages tanıtır.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815893"
---
# <a name="xamarinforms-datapages"></a>Xamarin.Forms DataPages

![](~/media/shared/preview.png "Bu API, şu anda Önizleme aşamasındadır")

> [!IMPORTANT]
> DataPages gerektiren bir [Xamarin.Forms tema](~/xamarin-forms/user-interface/themes/index.md) işlemek için başvuru.

Xamarin.Forms DataPages geliştikçe 2016'da açıkladığımız ve müşterilerin deneyin ve geri bildirim sağlamak için Önizleme olarak kullanılabilir.

DataPages hızlı ve kolay bir veri kaynağı için önceden oluşturulmuş görünümler bağlamak için bir API sağlar. Liste öğelerini ve ayrıntı sayfalarında verileri otomatik olarak işlenir ve Temalar kullanılarak özelleştirilebilir.

Geliştikçe açılış konuşması Tanıtımı nasıl çalıştığını görmek için kullanıma [Başlangıç Kılavuzu](get-started.md).

[![](images/demo-sml.png "DataPages örnek uygulama")](images/demo.png#lightbox "DataPages örnek uygulaması")

## <a name="introduction"></a>Giriş

Veri kaynakları ve ilişkili veriler sayfalar geliştiricilerin hızla ve kolayca desteklenen bir veri kaynağını kullanan ve yerleşik kullanıcı Arabirimi, yapı iskelesi özelleştirilebilir Temalar ile kullanarak işleme izin verir.

DataPages dahil ederek bir Xamarin.Forms uygulaması eklenir **Xamarin.Forms.Pages** Nuget paketi.

### <a name="data-sources"></a>Data Sources

Önizleme kullanıma bazı önceden oluşturulmuş veri kaynaklarına sahiptir:

* **JsonDataSource**
* **AzureDataSource** (Nuget ayrı)
* **AzureEasyTableDataSource** (Nuget ayrı)

Bkz: [Başlangıç Kılavuzu](get-started.md) bir örnek için bir `JsonDataSource`.


### <a name="pages--controls"></a>Sayfaları ve denetimleri

Sağlanan veri kaynakları için kolayca bağlama izin vermek için aşağıdaki sayfalar ve denetimler eklenmiştir:

* **ListDataPage** – bkz [örnek Başlarken](get-started.md).
* **DirectoryPage** – etkin gruplandırma içeren bir liste.
* **PersonDetailPage** – bir tek veri öğesi belirli nesne türünü (kişi girişi) için özelleştirilmiş bir görünüm.
* **DataView** – veri kaynağından genel bir şekilde kullanıma sunmak için bir görünüm.
* **CardView** – bir bir görüntü, başlık metni ve açıklama metnini içeren görünüm stili.
* **HeroImage** – bir görüntü işleme görünümü.
* **ListItem** – bir görünümü ile yerel iOS ve Android liste öğeleri için benzer bir düzen önceden oluşturulmuş.

Bkz: [DataPages denetimleri başvurusu](controls.md) örnekler.



### <a name="under-the-hood"></a>Başlık altında

Xamarin.Forms veri kaynağı için uyar `IDataSource` arabirimi.

Xamarin.Forms altyapı, aşağıdaki özelliklerle bir veri kaynağı ile etkileşim kurar:

* `Data` – görüntülenebilen veri öğeleri salt okunur listesi.
* `IsLoading` – veri yüklenir ve işleme için kullanılabilir olup olmadığını gösteren bir Boole değeri.
* `[key]` – öğeleri almak için bir dizin oluşturucu.

İki farklı yöntemle `MaskKey` ve `UnmaskKey` (örn veri öğesi özellikleri (gizlemek veya göstermek için) kullanılabilir. bunları çizilmesini engeller engelleme).
Anahtar karşılık gelen bir adlandırılmış özellik veri öğesi nesnesi.
