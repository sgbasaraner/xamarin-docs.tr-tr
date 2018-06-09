---
title: Xamarin.Forms Seçici
description: Xamarin.Forms Seçici öğeleri, kullanıcı bir öğe seçebileceği kısa listesini görüntüler. Bu makalede Seçici sınıfının veri listesinden bir metin öğesini seçmek için nasıl kullanılacağını açıklar.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 82ae36a7be139e2a93d0e5c43c4bad355c49f217
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245045"
---
# <a name="xamarinforms-picker"></a>Xamarin.Forms Seçici

_Seçici görünüm veri listesinden bir öğe seçmek için bir denetimdir._

Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) öğeleri, kullanıcı seçebileceği bir öğe kısa listesini görüntüler. `Picker` sekiz özelliklerini tanımlar:

- [`Title`](xref:Xamarin.Forms.Picker.Title) tür `string`, varsayılan olarak `null`.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) tür `IList`, varsayılan olarak görüntülemek için öğe kaynak listesinin `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) tür `int`, -1 olarak varsayılan olarak seçilen öğenin dizini.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) tür `object`, varsayılan olarak seçilen öğeyi `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) tür [ `Color` ](xref:Xamarin.Forms.Color), varsayılan olarak metni görüntülemek için kullanılan renk [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) tür [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), varsayılan olarak [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) tür `string`, varsayılan olarak `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) tür `double`, -1.0 için varsayılan olarak.

Tüm sekiz özellikleri tarafından yedeklenen [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) nesneleri bunlar biçimlendirilebilir ve özellikler veri bağlamaların hedefleri olabilir anlamına gelir. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Ve [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) özelliklere sahip bir varsayılan bağlama modu [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), veri bağlamaların hedefleri olabileceği anlamına gelir kullanan bir uygulama içinde [Model View ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) mimarisi. Yazı tipi özelliklerini ayarlama hakkında daha fazla bilgi için bkz: [yazı tiplerini](~/xamarin-forms/user-interface/text/fonts.md).

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ilk görüntülendiğinde, herhangi bir veri göstermez. Bunun yerine, değerini kendi [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) özelliği, iOS ve Android platformları yer tutucu olarak gösterilir:

[![](images/picker-initial.png "İlk Seçici görüntü")](images/picker-initial-large.png#lightbox "ilk Seçici görüntüleme")

Zaman [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) kazancı odak, verileri görüntülenir ve kullanıcı bir öğeyi seçebilirsiniz:

[![](images/picker-selection.png "Bir öğeyi seçerek Seçici")](images/picker-selection-large.png#lightbox "öğeyi seçerek Seçici")

[ `Picker` ](xref:Xamarin.Forms.Picker) Ateşlenir bir [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) kullanıcı bir öğeyi seçtiğinde olay. Seçim seçili öğe olarak görüntülenir `Picker`:

![](images/picker-after-selection.png "Seçici seçim sonra")

Doldurmak için iki tekniği vardır bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) verilerle:

- Ayarı [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) görüntülenecek verileri özelliğine. Xamarin.Forms 2.3.4 tanıtılan önerilen yöntem budur. Daha fazla bilgi için bkz: [bir seçici'nın ItemsSource özelliği ayarlama](populating-itemssource.md).
- İçin görüntülenecek veri ekleme [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) koleksiyonu. Bu teknik doldurmak için özgün işlemi olan bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) verilerle. Daha fazla bilgi için bkz: [ekleme veri bir seçici 's öğeler koleksiyonuna](populating-items.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Seçici](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
