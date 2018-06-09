---
title: Xamarin.Forms DataPages
description: Bu makalede bir API için hızlı bir şekilde sağlamak ve bir veri kaynağı için önceden oluşturulmuş görünümler kolayca bağlamak Xamarin.Forms DataPages tanıtılır.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243336"
---
# <a name="xamarinforms-datapages"></a>Xamarin.Forms DataPages

![](~/media/shared/preview.png "Bu API şu anda önizlemede değil")

> [!IMPORTANT]
> DataPages gerektiren bir [Xamarin.Forms tema](~/xamarin-forms/user-interface/themes/index.md) işlemek için başvuru.

Xamarin.Forms DataPages geliştikçe 2016 duyurdu ve müşterilerin deneyin ve geri bildirim sağlamak önizleme olarak kullanılabilir.

DataPages hızlı ve kolay bir şekilde bir veri kaynağı için önceden oluşturulmuş görünümler bağlamak için bir API sağlar. Liste öğeleri ve ayrıntı sayfaları otomatik olarak bir veri oluşturmaz ve Temalar kullanılarak özelleştirilebilir.

Geliştikçe açılış konuşması Tanıtımı nasıl çalıştığını görmek için kullanıma [Başlangıç Kılavuzu](get-started.md).

[![](images/demo-sml.png "DataPages örnek uygulama")](images/demo.png#lightbox "DataPages örnek uygulama")

## <a name="introduction"></a>Giriş

Veri kaynakları ve ilişkili veriler sayfalar geliştiricilerin hızla ve kolayca desteklenen bir veri kaynağını kullanmasına ve kullanıcı Arabirimi, iskele özelleştirilebilir yerleşik temaları ile kullanarak işlemek izin verir.

DataPages ekleyerek bir Xamarin.Forms uygulaması eklenir **Xamarin.Forms.Pages** Nuget paketi.

### <a name="data-sources"></a>Data Sources

Önizleme kullanılabilir bazı önceden oluşturulmuş veri kaynaklarına sahip:

* **JsonDataSource**
* **AzureDataSource** (Nuget ayırın)
* **AzureEasyTableDataSource** (Nuget ayırın)

Bkz: [Başlangıç Kılavuzu](get-started.md) kullanarak bir örnek için bir `JsonDataSource`.


### <a name="pages--controls"></a>Sayfalar ve denetimler

Sağlanan veri kaynakları kolay bağlamaya izin vermek için aşağıdaki sayfalar ve denetimler eklenir:

* **ListDataPage** – bkz [örnek Başlarken](get-started.md).
* **DirectoryPage** – gruplandırma etkin olan bir liste.
* **PersonDetailPage** – bir tek veri öğesi belirli nesne türünü (kişi girişi) için özelleştirilmiş görünüm.
* **DataView** – veri kaynağından genel bir şekilde kullanıma sunmak için bir görünüm.
* **Kart görünümü** – bir görüntü, başlık metnini ve açıklama metnini içeren görünüm stili.
* **HeroImage** – bir görüntü işleme görünümü.
* **ListItem** – bir görünüm yerel iOS ve Android liste öğeleri için benzer bir düzeni ile önceden oluşturulmuş.

Bkz: [DataPages denetimleri başvuru](controls.md) örnekleri için.



### <a name="under-the-hood"></a>Başlık altında

Xamarin.Forms veri kaynağı aynılarını `IDataSource` arabirimi.

Xamarin.Forms altyapısı aracılığıyla aşağıdaki özellikleri veri kaynağı ile etkileşim kurar:

* `Data` – görüntülenebilir veri öğelerini salt okunur bir listesi.
* `IsLoading` – verilerin yüklenir ve işleme için kullanılabilir olup olmadığını gösteren bir Boole değeri.
* `[key]` – öğeleri almak için bir dizin oluşturucu.

İki yöntem vardır `MaskKey` ve `UnmaskKey` (örn veri öğesi özellikleri (göstermek veya gizlemek için) kullanılabilir. bunları çizilmesini engeller).
Anahtar karşılık gelen veri öğesi nesnesi adlı bir özellik.
