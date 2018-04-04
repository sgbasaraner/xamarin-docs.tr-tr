---
title: Düğmeleri
description: UIButton sınıfı, çeşitli farklı stillerde iOS ekranlarda düğmesinin temsil etmek için kullanılır. Bu bölüm iOS düğmeleri ile çalışmak için farklı seçenekler sunar.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c2c33103c005a5ed567b1c4703846f887d824ac4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="buttons"></a>Düğmeleri

_UIButton sınıfı, çeşitli farklı stillerde iOS ekranlarda düğmesinin temsil etmek için kullanılır. Bu bölüm iOS düğmeleri ile çalışmak için farklı seçenekler sunar._

`UIButton`Sınıfı, bir iOS düğmesi denetiminde temsil eder. 

Düğme Özellikleri düzenlenebilir `Properties Pad` iOS Designer:


![](buttons-images/properties.png "İOS Tasarımcısı özellikleri paneli")

## <a name="creating-a-button"></a>Bir düğme oluşturma

Bir UIButton yalnızca birkaç satır kod oluşturulabilir.

İlk olarak, yeni bir düğme örneği ve ihtiyacınız düğmesi türünü belirtin:

```csharp
UIButton myButton = new UIButton(UIButtonType.System);
```

UIButtonType aşağıdakilerden biri belirtilmelidir:

- **Sistem** -bu iOS tarafından kullanılan standart düğme türü ve en sık kullanacağınız türüdür.
- **DetailDisclosure** -ayrıntılı bilgi göstermek veya gizlemek için kullanılan düğmesi "azaltın" türünü gösterir.
- **InfoDark** -bir koyu ayrıntılı düğmesine bir daire içinde bir "i" görüntülenen bilgileri.
- **InfoLight** -ışık ayrıntılı düğmesine bir daire içinde bir "i" görüntülenen bilgileri.
- **AddContact** -düğme bir kişi Ekle düğmesi olarak görüntüler.
- **Özel** -düğmesinin birkaç nitelikler özelleştirmenizi sağlar.

Düğme türleri hakkında daha fazla bilgi bulunabilir [düğme türleri](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/create_different_types_of_buttons/) tarif.

Ardından, ekranda boyut tanımlayın ve düğmesinin konumu. Örnek:

```csharp
myButton.Frame = new CGRect (25, 25, 300, 150);
```

Düğme metni değiştirmek için kullanın `SetTitle` özelliği bir metin dizesinin ayarlamanızı gerektiren düğmesine ve `UIControlStyle`. Örneğin:

```csharp
myButton.SetTitle("Hello, World!", UIControlState.Normal);
```

Her durum için farklı özellikleri ayarlama (ör. kullanıcı için daha fazla bilgi iletişim kurmanıza olanak veren metin rengi devre dışı durumunu için gri olun). İOS Tasarımcısı kullanarak her durumunu arasında geçiş yapabilirsiniz veya program aracılığıyla yapabilirsiniz. Ayar düğme metni ve durumu hakkında daha fazla bilgi için başvurmak [düğme metni ayarlamak](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/set_button_text/) tarif.

## <a name="dealing-with-user-interactions"></a>Kullanıcı etkileşimleri postalarla


Bunlar tıklatıldığında bir şeyler sürece düğmeleri çok kullanışlı değildir! 

Kullanım temas tarafından kendi ekranında düğmesini ile etkileşime giren gibi iOS düğmeleri neredeyse her zaman dokunma olayları olaylardır. Tüm olası UI denetim olaylarının bir listesi listelenen [burada](https://developer.apple.com/documentation/uikit/uicontrolevents), ancak en yaygın kullanılan olay iOS `TouchUpInside`. Düğmeye basıldığında sonra bir şeyler için olay işleyicisi sonra oluşturabilirsiniz:


```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

### <a name="adding-events-in-the-ios-designer"></a>İOS Tasarımcısı olaylar ekleme
 
Olaylar sekmesi özelliği defterinde denetimlere olay eklemek için kullanın.

Olayı seçin ve ya da yeni bir olay işleyicisi ya da listeden seçin bir ad yazın. Bunun yapılması, yeni bir kısmi yöntemi View Controller sınıfınızda oluşturur.

![Olaylar sekmesi](buttons-images/image1.png)

## <a name="styling-a-button"></a>Bir düğme stil oluşturma

UIButtons başlık yalnızca yalnızca değiştirilememektedir bir duruma sahip oldukları, çoğu Uıkit denetimleri farklı, her biri için değiştirmek zorunda `UIControlState`. Başlık rengi ve gölge rengini ayarlama benzer bir şekilde gerçekleştirilir:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Ayrıca, öznitelikli metin düğmenin başlığı olarak kullanabilirsiniz. Örneğin:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Özel düğme türleri


Ayarladığınızda `Custom` düğmesi nesne türünde hiçbir varsayılan işleme. Düğmenin görünümünü farklı durumları için görüntü ayarlayarak yapılandırabilirsiniz. Örneğin, aşağıdaki kodu farklı görüntülerde ekleneceği gösterilmektedir `Normal`, `Highlighted` ve `Selected` durumları:


```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```


Olup kullanıcı düğmesini veya değecek bağlı olarak, bunu aşağıdaki görüntüleri biri olarak işlenir (`Normal`, `Highlighted` ve `Selected` sırasıyla durumları):


![](buttons-images/image22.png "UIButton durumu Normal")
![](buttons-images/image23.png "UIButton vurgulanmış durumu")
![](buttons-images/image24.png "UIButton seçili durumu")

Özel düğmeler ile çalışma hakkında daha fazla bilgi için başvurmak [bir düğme için resim kullanmak](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/use_an_image_for_a_button/).


## <a name="related-links"></a>İlgili bağlantılar

- [UIButton çalışma kitabı](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
