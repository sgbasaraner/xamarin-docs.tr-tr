---
title: Bir Seçici'ye ait öğeler koleksiyonuna ekleme verileri
description: Seçici görünüm veri listesinden bir öğe seçmek için bir denetimdir. Bu makalede öğeler koleksiyonuna ekleyerek bir seçici verilerle doldurmak nasıl ve nasıl öğe seçimi kullanıcı tarafından yanıt açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 63a72861895f79d2d0154297f88610ddb8bb8beb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30792660"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Bir Seçici'ye ait öğeler koleksiyonuna ekleme verileri

_Seçici görünüm veri listesinden bir öğe seçmek için bir denetimdir. Bu makalede öğeler koleksiyonuna ekleyerek bir seçici verilerle doldurmak nasıl ve nasıl öğe seçimi kullanıcı tarafından yanıt açıklanmaktadır._

## <a name="populating-a-picker-with-data"></a>Bir seçici verilerle doldurma

Xamarin.Forms 2.3.4, işlem doldurmak için önce bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) salt okunur olarak görüntülenecek veri eklemek için verilerle edildi [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) türündekoleksiyon`IList<string>`. Koleksiyondaki her öğe türü olmalıdır `string`. Öğeleri eklenebilir XAML'de başlatarak `Items` listesini özelliğiyle `x:String` öğeleri:

```xaml
<Picker Title="Select a monkey">
  <Picker.Items>
    <x:String>Baboon</x:String>
    <x:String>Capuchin Monkey</x:String>
    <x:String>Blue Monkey</x:String>
    <x:String>Squirrel Monkey</x:String>
    <x:String>Golden Lion Tamarin</x:String>
    <x:String>Howler Monkey</x:String>
    <x:String>Japanese Macaque</x:String>
  </Picker.Items>
</Picker>
```

Eşdeğer C# kod aşağıda verilmiştir:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

Verileri kullanarak ekleme yanı sıra `Items.Add` yöntemi, veri da eklenebilir koleksiyona kullanarak `Items.Insert` yöntemi.

## <a name="responding-to-item-selection"></a>Öğe seçimi yanıt

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) bir öğe seçimi birer birer destekler. Bir kullanıcı bir öğeyi seçtiğinde [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) olay etkinleşir ve [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) özellik listesinde seçili öğenin dizinini temsil eden bir tamsayı üzere güncelleştirilir. `SelectedIndex` Kullanıcı seçilen öğeyi belirten sıfır tabanlı bir sayı bir özelliktir. Öğe seçiliyse olduğu durumda olduğunda `Picker` ilk oluşturuldu ve başlatıldı, `SelectedIndex` -1 olur.

> [!NOTE]
> Öğe seçimi davranışı bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) iOS platforma özgü ile özelleştirilebilir. Daha fazla bilgi için bkz: [denetleme Seçici öğe seçimi](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Aşağıdaki örnekte gösterildiği kod `OnPickerSelectedIndexChanged` olan olay işleyicisi yöntemi yürütülmesi [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) olay ateşlenir:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = picker.Items[selectedIndex];
  }
}
```

Bu yöntem alır [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) özellik değeri ve değer seçili öğesini almak için kullandığı [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) koleksiyonu. Her öğe için `Items` koleksiyonu bir `string`, tarafından görüntülenebilen bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) bir cast gerektirmeden.

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ayarlayarak belirli bir öğeyi görüntülemek için başlatılan [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) özelliği. Ancak, `SelectedIndex` başlatma sonra özelliği ayarlanmalıdır [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) koleksiyonu.

## <a name="summary"></a>Özet

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Görünümdür veri listesinden bir öğe seçmek için bir denetim. Bu makalede açıklanan doldurmak nasıl bir `Picker` ekleyerek verilerle [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) toplama ve nasıl öğe seçimi kullanıcı tarafından yanıt. Kullanma işlemini, bu bir `Picker` Xamarin.Forms 2.3.4 önce.


## <a name="related-links"></a>İlgili bağlantılar

- [Seçici Demo (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Seçici](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
