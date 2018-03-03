---
title: Picker
description: "Seçici görünüm veri listesinden bir öğe seçmek için bir denetimdir."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: edc724eb73b314c0accd3e8775b9b26b6eac16d9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="picker"></a>Picker

_Seçici görünüm veri listesinden bir öğe seçmek için bir denetimdir._

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) öğeleri, kullanıcı seçebileceği kısa listesini görüntüler. Ancak, bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ilk görüntülendiğinde, herhangi bir veri göstermez. Bunun yerine, değerini kendi [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) özelliği, iOS ve Android platformları yer tutucu olarak gösterilir:

[![](images/picker-initial.png "İlk Seçici görüntü")](images/picker-initial-large.png "ilk Seçici görüntüleme")

Zaman [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) kazancı odak, verileri görüntülenir ve kullanıcı bir öğeyi seçebilirsiniz:

[![](images/picker-selection.png "Bir öğeyi seçerek Seçici")](images/picker-selection-large.png "öğeyi seçerek Seçici")

Seçim seçili öğe olarak görüntülenir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/):

![](images/picker-after-selection.png "Seçici seçim sonra")

Doldurmak için iki tekniği vardır bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) verilerle:

- Ayarı [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) görüntülenecek verileri özelliğine. Xamarin.Forms 2.3.4 tanıtılan önerilen yöntem budur. Daha fazla bilgi için bkz: [bir seçici'nın ItemsSource özelliği ayarlama](populating-itemssource.md).
- İçin görüntülenecek veri ekleme [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) koleksiyonu. Bu teknik doldurmak için özgün işlemi olan bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) verilerle. Daha fazla bilgi için bkz: [ekleme veri bir seçici 's öğeler koleksiyonuna](populating-items.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Picker](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
