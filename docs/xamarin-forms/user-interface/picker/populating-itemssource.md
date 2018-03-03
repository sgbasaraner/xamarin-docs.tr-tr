---
title: "Bir seçici'nın ItemsSource özelliği ayarlama"
description: "Seçici görünüm veri listesinden bir öğe seçmek için bir denetimdir. Bu makalede ItemsSource özelliği ayarlanarak bir seçici verilerle doldurmak nasıl ve nasıl öğe seçimi kullanıcı tarafından yanıt açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 5e3d20ad213df9fd9331c71c84003c7738bd5a29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="setting-a-pickers-itemssource-property"></a>Bir seçici'nın ItemsSource özelliği ayarlama

_Seçici görünüm veri listesinden bir öğe seçmek için bir denetimdir. Bu makalede ItemsSource özelliği ayarlanarak bir seçici verilerle doldurmak nasıl ve nasıl öğe seçimi kullanıcı tarafından yanıt açıklanmaktadır._

Xamarin.Forms 2.3.4 Gelişmiş [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ayarlayarak veri ile doldurulacak özelliği ekleyerek görünüm kendi [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) özelliği ve seçilen öğeyi almakiçin[ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) özelliği. Ayrıca, seçili öğenin metninin rengi ayarlayarak değiştirilebilir [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.TextColor/) özelliğine bir [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/).

## <a name="populating-a-picker-with-data"></a>Bir seçici verilerle doldurma

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) verilerle ayarlayarak doldurulabilir kendi [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) özelliğine bir `IList` koleksiyonu. Koleksiyondaki her öğe, veya gerekir, türetilen tür `object`. Öğeleri eklenebilir XAML'de başlatarak `ItemsSource` öğeleri dizisi özelliğinden:

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
> Unutmayın `x:Array` öğesi gerektiriyor bir `Type` dizisindeki öğelerin türünü belirten özniteliği.

Eşdeğer C# kod aşağıda verilmiştir:

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

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) bir öğe seçimi birer birer destekler. Bir kullanıcı bir öğeyi seçtiğinde [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) olay ateşlenir [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) özellik listesinde seçili öğenin dizinini temsil eden bir tamsayı üzere güncelleştirilir ve [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) özelliği güncelleştirileceğini `object` seçilen öğeyi temsil eden. [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Öğesi seçili kullanıcı gösteren sıfır tabanlı bir sayı bir özelliktir. Öğe seçiliyse olduğu durumda olduğunda [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ilk oluşturuldu ve başlatıldı, `SelectedIndex` -1 olur.

> [!NOTE]
> Öğe seçimi davranışı bir [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) iOS platforma özgü ile özelleştirilebilir. Daha fazla bilgi için bkz: [denetleme Seçici öğe seçimi](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Aşağıdaki kod örneğinde nasıl alınacağını gösterir [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) özellik değerinden [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) XAML'de:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Eşdeğer C# kod aşağıda verilmiştir:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Ayrıca, olay işleyici olabilir yürütülmesi [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) olay ateşlenir:

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

Bu yöntem alır [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) özellik değeri ve değer seçili öğesini almak için kullandığı [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) koleksiyonu. Bu seçili öğesini almak için işlevsel olarak eşdeğerdir [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) özelliği. Unutmayın her öğe `ItemsSource` koleksiyonu türüdür `object`ve için dönüştürmeniz gerekir böylece bir `string` görüntülemek için.

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ayarlayarak belirli bir öğeyi görüntülemek için başlatılan [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) veya [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) özellikleri. Ancak, bu özellikleri başlatma sonra olarak ayarlanmalıdır [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) koleksiyonu.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Bir seçici veri bağlama işlemini kullanma verilerle doldurma

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) bağlamak için veri bağlama kullanarak verilerle ayrıca doldurulabilir kendi [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) özelliğine bir `IList` koleksiyonu. XAML'de bu ile elde edilen [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) biçimlendirme uzantısı:

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

Eşdeğer C# kod aşağıda verilmiştir:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Özelliği veri bağlanır `Monkeys` döndürür bağlı Görünüm modeli özelliği bir `IList<Monkey>` koleksiyonu. Aşağıdaki örnekte gösterildiği kod `Monkey` dört özellikler içeren sınıfı:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Nesneler listesine bağlanırken [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) her nesnesinden görüntülemek için hangi özelliğinin söylediyse gerekir. Bu ayar sağlanır [ `ItemDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemDisplayBinding/) her nesnesinden gerekli özellik için özellik. Yukarıdaki kod örnekleri, `Picker` her görüntülemek üzere ayarlandığı `Monkey.Name` özellik değeri.

### <a name="responding-to-item-selection"></a>Öğe seçimi yanıt

Veri bağlama, bir nesne ayarlamak için kullanılabilir [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) değişiklik yapıldığında özellik değeri:

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

Eşdeğer C# kod aşağıda verilmiştir:

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

[ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Özelliği veri bağlanır `SelectedMonkey` türünde bağlı Görünüm modeli özelliğinin `Monkey`. Bu nedenle, kullanıcı seçtiğinde bir öğeyi [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), `SelectedMonkey` özelliğinin ayarlanması için seçilen `Monkey` nesnesi. `SelectedMonkey` Nesne verilerini tarafından kullanıcı arabiriminde görüntülenir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ve [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) görünümleri:

![](populating-itemssource-images/monkeys.png "Seçici öğe seçimi")

> [!NOTE]
> Unutmayın [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) ve [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) özellikleri her iki varsayılan olarak iki yönlü bağlamaları destekler.

## <a name="summary"></a>Özet

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Görünümdür veri listesinden bir öğe seçmek için bir denetim. Bu makalede açıklanan doldurmak nasıl bir `Picker` ayarlayarak verilerle [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) özelliği ve nasıl öğe seçimi kullanıcı tarafından yanıt. Xamarin.Forms 2.3.4 sunulan, bu yaklaşım ile etkileşim için önerilen yaklaşımdır bir `Picker`.


## <a name="related-links"></a>İlgili bağlantılar

- [Seçici Demo (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Monkey uygulama (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [Bağlanabilir Seçici (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [Picker](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
