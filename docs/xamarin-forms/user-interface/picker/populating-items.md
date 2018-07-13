---
title: Bir seçicinin öğe koleksiyonuna veri Ekle
description: Veri listesinden bir öğe seçmek için bir denetim Seçici görünümüdür. Bu makalede, öğeler koleksiyonuna ekleyerek bir seçici verilerle doldurmak nasıl ve nasıl öğe seçimi kullanıcı tarafından yanıt açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 8d911108d7d72586a37a3281803eab9c0841f16c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997119"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Bir seçicinin öğe koleksiyonuna veri Ekle

_Veri listesinden bir öğe seçmek için bir denetim Seçici görünümüdür. Bu makalede, öğeler koleksiyonuna ekleyerek bir seçici verilerle doldurmak nasıl ve nasıl öğe seçimi kullanıcı tarafından yanıt açıklanmaktadır._

## <a name="populating-a-picker-with-data"></a>Bir seçici verilerle doldurma

Xamarin.Forms 2.3.4, işlem doldurmak için önce bir [ `Picker` ](xref:Xamarin.Forms.Picker) salt okunur olarak görüntülenecek veri eklemek için verilerle olduğu [ `Items` ](xref:Xamarin.Forms.Picker.Items) türündeolankoleksiyon`IList<string>`. Koleksiyondaki her öğe türünde olmalıdır `string`. Öğeleri eklenebilir XAML içinde başlatarak `Items` listesini özelliğiyle `x:String` öğeleri:

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

Eşdeğer C# kodu aşağıda gösterilmiştir:

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

Verileri kullanarak ekleme yanı sıra `Items.Add` yöntemi, veri de eklenebilir koleksiyona kullanarak `Items.Insert` yöntemi.

## <a name="responding-to-item-selection"></a>Öğe seçimi yanıt

A [ `Picker` ](xref:Xamarin.Forms.Picker) aynı anda tek bir öğe seçimini destekler. Bir kullanıcı bir öğe seçtiğinde [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) olayı tetiklendiğinde ve [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) özelliğinin güncelleştirilmesinin listedeki seçili öğenin dizinini temsil eden bir tamsayı. `SelectedIndex` Kullanıcı seçili öğeyi gösteren birkaç sıfır tabanlı bir özelliktir. Hiçbir öğe seçili değilse olduğu durumda olduğunda `Picker` önce oluşturulan ve başlatılan `SelectedIndex` -1 olur.

> [!NOTE]
> Öğe seçimi davranışı bir [ `Picker` ](xref:Xamarin.Forms.Picker) platforma özgü olan iOS özelleştirilebilir. Daha fazla bilgi için [denetleme Seçici öğe seçimi](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Aşağıdaki örnekte gösterildiği kod `OnPickerSelectedIndexChanged` olan olay işleyicisi yönteminde, yürütülmesi [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) olay harekete geçirilir:

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

Bu yöntem alır [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) özellik değeri ve seçili öğesini almak için bir değer kullanır [ `Items` ](xref:Xamarin.Forms.Picker.Items) koleksiyonu. Her öğe için `Items` koleksiyonu bir `string`, tarafından görüntülenebilen bir [ `Label` ](xref:Xamarin.Forms.Label) bir dönüştürme işlemi gerektirmeden.

> [!NOTE]
> A [ `Picker` ](xref:Xamarin.Forms.Picker) ayarlayarak, belirli bir öğeyi görüntülemek için başlatılabilir [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) özelliği. Ancak, `SelectedIndex` başlatma sonrasında özelliği ayarlanmalıdır [ `Items` ](xref:Xamarin.Forms.Picker.Items) koleksiyonu.

## <a name="summary"></a>Özet

[ `Picker` ](xref:Xamarin.Forms.Picker) Görünümdür veri listesinden bir öğe seçmek için bir denetim. Bu makalede açıklanan doldurmak nasıl bir `Picker` ekleyerek verilerle [ `Items` ](xref:Xamarin.Forms.Picker.Items) toplama ve nasıl öğe seçimi kullanıcı tarafından yanıt. Bu kullanma işlemini olduğu bir `Picker` Xamarin.Forms 2.3.4 önce.


## <a name="related-links"></a>İlgili bağlantılar

- [Seçici tanıtım (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Seçici](xref:Xamarin.Forms.Picker)
