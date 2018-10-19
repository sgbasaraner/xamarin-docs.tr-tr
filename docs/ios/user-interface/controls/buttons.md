---
title: Xamarin.iOS düğmeleri
description: UIButton sınıfı, çeşitli farklı türlerde iOS ekranları düğmesini temsil etmek için kullanılır. Bu kılavuz, iOS düğmeleri ile çalışmak için farklı seçenekler sunar.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2018
ms.openlocfilehash: 35fc743944c04dd1fdb8e035ba94ad6aeb6156ea
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38986010"
---
# <a name="buttons-in-xamarinios"></a>Xamarin.iOS düğmeleri

İos'ta, `UIButton` sınıfı, bir düğme denetimini temsil eder.

Bir düğmenin özelliklerini programlama yoluyla veya ile değiştirilebilir **özellikler panelinde** iOS Designer'ın:

![İOS Designer, Özellikler panelinde](buttons-images/properties.png "iOS Designer, Özellikler panelinde")

## <a name="creating-a-button-programmatically"></a>Program aracılığıyla bir düğme oluşturma

A `UIButton` yalnızca birkaç satır kod ile oluşturulabilir.

- Bir düğme oluşturmak ve türünü belirtin:

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Düğmenin türü tarafından belirtilen bir `UIButtonType`:

  - `UIButtonType.System` -Bir genel amaçlı düğmesi
  - `UIButtonType.DetailDisclosure` -Genellikle, bir tablodaki belirli bir öğeyi hakkında ayrıntılı bilgi kullanılabilirliğini gösterir
  - `UIButtonType.InfoDark` -Yapılandırma bilgilerini kullanılabilirliğini gösterir. koyu renkli
  - `UIButtonType.InfoLight` -Yapılandırma bilgilerini kullanılabilirliğini gösterir. açık renkli
  - `UIButtonType..AddContact` -Kişi eklenebilir gösterir
  - `UIButtonType.Custom` -Özelleştirilebilir düğmesi

  Düğme türleri hakkında daha fazla bilgi için göz atın:
  
  - [Özel düğme türlerini](#custom-button-types) bu belgenin bölüm
  - [Düğme türleri](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons) tarif
  - Apple'nın [iOS İnsan Arabirimi yönergelerine](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/).

- Düğmenin boyutunu ve konumunu tanımlayın:

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Düğmenin metni ayarlayın. Kullanım `SetTitle` metin gerektiren yöntemi ve bir `UIControlState` değeri:

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  Bir düğmeye stil oluşturma ve metin ayarlama hakkında daha fazla bilgi için bakın:

  - [Bir düğmeye stil](#styling-a-button) bu belgenin bölüm
  - [Ayarla düğmesi metni](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) tarif.

## <a name="handling-a-button-tap"></a>Bir düğmeye dokunarak işleme

Bir düğmeye dokunarak için yanıt vermek için bir işleyici düğmenin için sağlamak `TouchUpInside` olay:

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` yalnızca düğme olayı değil. `UIButton` bir alt sınıfıdır `UIControl`, tanımlayan [birçok farklı olay](https://developer.xamarin.com/api/type/UIKit.UIControlEvent/).

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>İOS Designer düğmesi olay işleyicileri belirtmek için kullanma

Kullanım **olayları** sekmesinde **özellikler panelinde** çeşitli olayları bir düğme kullanıcının olay işleyicileri belirtmek için.

Uygun bir olay için yeni bir olay işleyicisi adı yazın veya listeden birini seçin. Bunu bir olay işleyicisi, düğmenin görünüm denetleyicisi için kod oluşturur.

![Özellikleri paneli olaylar sekmesinde](buttons-images/image1.png "olaylar sekmesinde özellikler paneli")

## <a name="styling-a-button"></a>Bir düğmeye stil oluşturma

`UIButton` denetimleri farklı durumları sayısında varolabilir, tarafından belirtilen her bir `UIControlState` değeri – `Normal`, `Disabled`, `Focused`, `Highlighted`vb. Her durum programlama yoluyla veya iOS Designer ile belirtilen benzersiz bir stili verilebilir.

> [!NOTE]
> Tüm tam bir listesi için `UIControlState` değerleri göz atın [`UIKit.UIControlState enumeration`](https://developer.xamarin.com/api/type/UIKit.UIControlState/)
> Belgeleri.

Örneğin, başlık rengi ve gölge rengini ayarlamak için `UIControlState.Normal`:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Aşağıdaki kodu için bir öznitelik (stilize) dizesi düğmesi başlığı ayarlar `UIControlState.Normal` ve `UIControlState.Highlighted`:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Özel düğme türleri

İle düğmeleri bir `UIButtonType` , `Custom` varsayılan bir stili vardır. Ancak, farklı durumlara yönelik bir görüntü ayarlayarak bir düğmenin görünümünü yapılandırmak mümkündür:

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

Olup kullanıcı düğmeyi veya oncollisionstay bağlı olarak, bunu aşağıdaki görüntülerden birini işlenir (`UIControlState.Normal`, `UIControlState.Highlighted` ve `UIControlState.Selected` durumları, sırasıyla):

![UIControlState.Normal](buttons-images/image22.png "UIControlState.Normal")
![UIControlState.Highlighted](buttons-images/image23.png "UIControlState.Highlighted") 
 ![UIControlState.Selected](buttons-images/image24.png "UIControlState.Selected")

Özel düğmeler ile çalışma hakkında daha fazla bilgi için [bir görüntüyü kullanmak için bir düğme](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) tarif.

