---
title: Xamarin.Forms Seçici
description: Xamarin.Forms Seçici öğeleri kullanıcı bir öğe seçebilir, kısa listesini görüntüler. Bu makalede, veri listesinden bir öğe seçmek için Seçici sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: c852cd29197b000ed1ff53853d64cfa25fb699e7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996603"
---
# <a name="xamarinforms-picker"></a>Xamarin.Forms Seçici

_Veri listesinden bir öğe seçmek için bir denetim Seçici görünümüdür._

Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) öğeleri, kullanıcı bir öğe seçebilir, kısa bir listesini görüntüler. `Picker` sekiz özelliklerini tanımlar:

- [`Title`](xref:Xamarin.Forms.Picker.Title) tür `string`, bunun varsayılan `null`.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) tür `IList`, varsayılan olarak görüntülemek için öğeleri kaynak listesinde `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) tür `int`, -1 olarak varsayılan olarak seçilen öğenin dizini.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) tür `object`, varsayılan olarak seçili öğe `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) tür [ `Color` ](xref:Xamarin.Forms.Color), varsayılan olarak metni görüntülemek için kullanılan rengi [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) tür [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), bunun varsayılan [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) tür `string`, bunun varsayılan `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) tür `double`, -1.0 için varsayılan olarak.

Tüm sekiz özellikleri tarafından yedeklenen [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) nesneleri, bunlar biçimlendirilebilir ve veri bağlama hedefleri özellikler olabilir. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Ve [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) özelliklere sahip bir varsayılan bağlama modu [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), veri bağlama hedefleri olabileceği anlamına gelir kullanan bir uygulamada [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) mimarisi. Yazı tipi özelliklerini ayarlama hakkında daha fazla bilgi için bkz. [yazı tipleri](~/xamarin-forms/user-interface/text/fonts.md).

A [ `Picker` ](xref:Xamarin.Forms.Picker) ilk görüntülendiğinde verileri göstermez. Bunun yerine, değerini kendi [ `Title` ](xref:Xamarin.Forms.Picker.Title) özelliği, iOS ve Android platformlarında bir yer tutucu olarak gösterilmiştir:

[![](images/picker-initial.png "İlk Seçici görünen")](images/picker-initial-large.png#lightbox "ilk Seçici görüntüleme")

Zaman [ `Picker` ](xref:Xamarin.Forms.Picker) kazançlar odağı verilerini görüntülenir ve kullanıcı bir öğe seçebilir:

[![](images/picker-selection.png "Bir öğenin seçilmesi Seçici")](images/picker-selection-large.png#lightbox "seçici bir öğe seçme")

[ `Picker` ](xref:Xamarin.Forms.Picker) Ateşlenir bir [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) kullanıcı bir öğe seçtiğinde olay. Seçimi seçili öğe tarafından görüntülenir `Picker`:

![](images/picker-after-selection.png "Seçimi sonra Seçici")

Doldurmak için iki teknik vardır bir [ `Picker` ](xref:Xamarin.Forms.Picker) verilerle:

- Ayarı [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) özelliği görüntülenecek veriler. Xamarin.Forms içinde 2.3.4 sunulmuştur önerilen yöntem budur. Daha fazla bilgi için [bir seçicinin ItemsSource özelliğini ayarlama](populating-itemssource.md).
- Görüntülenmek üzere veri ekleme [ `Items` ](xref:Xamarin.Forms.Picker.Items) koleksiyonu. Özgün işlem doldurmak için bu tekniği olan bir [ `Picker` ](xref:Xamarin.Forms.Picker) verilerle. Daha fazla bilgi için [bir seçicinin öğe koleksiyonuna veri ekleme](populating-items.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Seçici](xref:Xamarin.Forms.Picker)
