---
title: Bir seçicinin ItemsSource özelliğini ayarlama
description: Veri listesinden bir öğe seçmek için bir denetim Seçici görünümüdür. Bu makalede ItemsSource özelliğini ayarlayarak bir seçici verilerle doldurmak nasıl ve nasıl öğe seçimi kullanıcı tarafından yanıt açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 3f82e4b7d52988bfef9736ace8c476a9cd2da02b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994750"
---
# <a name="setting-a-pickers-itemssource-property"></a>Bir seçicinin ItemsSource özelliğini ayarlama

_Veri listesinden bir öğe seçmek için bir denetim Seçici görünümüdür. Bu makalede ItemsSource özelliğini ayarlayarak bir seçici verilerle doldurmak nasıl ve nasıl öğe seçimi kullanıcı tarafından yanıt açıklanmaktadır._

Xamarin.Forms 2.3.4 Gelişmiş [ `Picker` ](xref:Xamarin.Forms.Picker) ayarlayarak veri ile doldurulacak olanağı ekleyerek görünüm kendi [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) özelliği ve Seçiliöğeyialmakiçin[ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) özelliği. Ayrıca, seçili öğenin metin rengi ayarlayarak değiştirilebilir [ `TextColor` ](xref:Xamarin.Forms.Picker.TextColor) özelliğini bir [ `Color` ](xref:Xamarin.Forms.Color).

## <a name="populating-a-picker-with-data"></a>Bir seçici verilerle doldurma

A [ `Picker` ](xref:Xamarin.Forms.Picker) verilerle ayarlayarak doldurulabilir kendi [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) özelliğini bir `IList` koleksiyonu. Koleksiyondaki her öğe olması veya gerekir, türetilen tür `object`. Öğeleri eklenebilir XAML içinde başlatarak `ItemsSource` öğelerin bir dizisi özelliği:

```xaml
<Picker x:Name="picker" Title="Select a monkey">
  <Picker.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </Picker.ItemsSource>
</Picker>
```

> [!NOTE]
> Unutmayın `x:Array` öğesi gerektiriyor bir `Type` dizideki öğelerin türünü belirten özniteliği.

Eşdeğer C# kodu aşağıda gösterilmiştir:

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey" };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>Öğe seçimi yanıt

A [ `Picker` ](xref:Xamarin.Forms.Picker) aynı anda tek bir öğe seçimini destekler. Bir kullanıcı bir öğe seçtiğinde [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) olayı tetiklendiğinde, [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) listesindeki seçili öğenin dizinini temsil eden bir tamsayı özelliği güncelleştirildiğinde ve [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) özelliğinin güncelleştirilmesinin `object` seçilen öğeyi temsil eden. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Özelliğidir, seçilen kullanıcıya öğeyi gösteren sıfır tabanlı bir sayı. Hiçbir öğe seçili değilse olduğu durumda olduğunda [ `Picker` ](xref:Xamarin.Forms.Picker) önce oluşturulan ve başlatılan `SelectedIndex` -1 olur.

> [!NOTE]
> Öğe seçimi davranışı bir [ `Picker` ](xref:Xamarin.Forms.Picker) platforma özgü olan iOS özelleştirilebilir. Daha fazla bilgi için [denetleme Seçici öğe seçimi](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Aşağıdaki kod örneğinde nasıl alınacağını gösterir [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) özellik değerinden [ `Picker` ](xref:Xamarin.Forms.Picker) XAML içinde:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Eşdeğer C# kodu aşağıda gösterilmiştir:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Ayrıca, bir olay işleyicisi olabilir yürütülmesi [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) olay harekete geçirilir:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = (string)picker.ItemsSource[selectedIndex];
  }
}
```

Bu yöntem alır [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) özellik değeri ve seçili öğesini almak için bir değer kullanır [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) koleksiyonu. Bu seçili öğesini almak için işlevsel olarak eşdeğerdir [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) özelliği. Unutmayın her öğe `ItemsSource` koleksiyon türüdür `object`ve e dönüştürmelisiniz bir `string` görüntülemek için.

> [!NOTE]
> A [ `Picker` ](xref:Xamarin.Forms.Picker) ayarlayarak, belirli bir öğeyi görüntülemek için başlatılabilir [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) veya [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) özellikleri. Ancak, bu özellikleri başlatma sonrasında ayarlanmalıdır [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) koleksiyonu.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Seçici'yi kullanarak veri bağlama verilerle doldurma

A [ `Picker` ](xref:Xamarin.Forms.Picker) bağlamak için veri bağlama kullanarak verilerle da doldurulabilir kendi [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) özelliğini bir `IList` koleksiyonu. XAML içinde bu ile sağlanır [ `Binding` ](xref:Xamarin.Forms.Xaml.BindingExtension) işaretleme uzantısı:

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

Eşdeğer C# kodu aşağıda gösterilmiştir:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Özelliği veri bağlanır `Monkeys` döndüren bağlı Görünüm modeli özelliği bir `IList<Monkey>` koleksiyonu. Aşağıdaki örnekte gösterildiği kod `Monkey` dört özellikleri içeren bir sınıfı:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Nesneler listesine bağlanırken [ `Picker` ](xref:Xamarin.Forms.Picker) her nesneyi görüntülemek için hangi özelliğinin söyledik. Bu ayarı gerçekleştirilir [ `ItemDisplayBinding` ](xref:Xamarin.Forms.Picker.ItemDisplayBinding) her nesne için gerekli özelliği özelliği. Yukarıdaki kod örneklerinde `Picker` her görüntülenecek kümesi `Monkey.Name` özellik değeri.

### <a name="responding-to-item-selection"></a>Öğe seçimi yanıt

Veri bağlama, bir nesne ayarlamak için kullanılabilir [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) değiştiğinde özellik değeri:

```xaml
<Picker Title="Select a monkey"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

Eşdeğer C# kodu aşağıda gösterilmiştir:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.SetBinding(Picker.SelectedItemProperty, "SelectedMonkey");
picker.ItemDisplayBinding = new Binding("Name");

var nameLabel = new Label { ... };
nameLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Name");

var locationLabel = new Label { ... };
locationLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Location");

var image = new Image { ... };
image.SetBinding(Image.SourceProperty, "SelectedMonkey.ImageUrl");

var detailsLabel = new Label();
detailsLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Details");
```

[ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Özelliği veri bağlanır `SelectedMonkey` türünde bağlı Görünüm modeli özelliği `Monkey`. Bu nedenle, kullanıcı seçtiğinde bir öğeyi [ `Picker` ](xref:Xamarin.Forms.Picker), `SelectedMonkey` özelliğinin ayarlanması seçili `Monkey` nesne. `SelectedMonkey` Nesne verilerini tarafından kullanıcı arabiriminde görüntülenen [ `Label` ](xref:Xamarin.Forms.Label) ve [ `Image` ](xref:Xamarin.Forms.Image) görünümler:

![](populating-itemssource-images/monkeys.png "Seçici öğe seçimi")

> [!NOTE]
> Unutmayın [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) ve [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) özellikleri varsayılan olarak iki yönlü bağlamaları destekler.

## <a name="summary"></a>Özet

[ `Picker` ](xref:Xamarin.Forms.Picker) Görünümdür veri listesinden bir öğe seçmek için bir denetim. Bu makalede açıklanan doldurmak nasıl bir `Picker` ayarlayarak verilerle [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) özelliği ve nasıl öğe seçimi kullanıcı tarafından yanıt. Xamarin.Forms içinde 2.3.4 kullanılmıştır, bu yaklaşım ile etkileşim kurmak için önerilen yaklaşımdır bir `Picker`.


## <a name="related-links"></a>İlgili bağlantılar

- [Seçici tanıtım (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Monkey uygulama (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [Bağlanabilir Seçici (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [Seçici](xref:Xamarin.Forms.Picker)
