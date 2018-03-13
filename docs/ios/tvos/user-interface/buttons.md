---
title: "Düğmeleri ile çalışma"
description: "Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde düğmeleri ile çalışma kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/07/2017
ms.openlocfilehash: 4b2a470d7fe2a1f9d4b8df40836c934547adf614
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-buttons"></a>Düğmeleri ile çalışma

_Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde düğmeleri ile çalışma kapsar._


Bir örneğini kullanması `UIButton` tvOS penceresinde odaklanabilir, seçilebilir düğme oluşturmak için sınıfı. Kullanıcı bir düğmesini seçtiğinde, hedef nesnesi için bir eylem iletisi gönderir, Xamarin.tvOS uygulama yanıt kullanıcı girdisini izin verin.

[![](buttons-images/buttons01.png "Örnek düğmeleri")](buttons-images/buttons01.png#lightbox)

Odak ile çalışma ve Siri uzaktan ile gezinme hakkında daha fazla bilgi için lütfen bkz bizim [gezinti ve odak çalışma](~/ios/tvos/app-fundamentals/navigation-focus.md) ve [Siri uzak ve Bluetooth denetleyicileri](~/ios/tvos/platform/remote-bluetooth.md) belgeleri.

<a name="About-Buttons" />

## <a name="about-buttons"></a>Düğmeler hakkında

TvOS, düğmeleri uygulamaya özgü eylemler için kullanılır ve bir başlık, bir simge veya her ikisini de içerebilir. Uygulamanın kullanıcı arabirimini kullanarak kullanıcı gider gibi [Siri uzaktan](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), odak, metin ve arka plan renklerini değiştirme kolaylaştırarak verilen düğmesi geçer. Gölge de kullanıcı arabirimi rest artmaya görünür kolaylaştırarak bir 3B efekti ekleme düğmesi uygulanır.

[![](buttons-images/buttons01.png "Örnek düğmeleri")](buttons-images/buttons01.png#lightbox)

Apple düğmeleri ile çalışmak için aşağıdaki önerileri vardır:

- **Başlık bir veya bir simgeyi** - her iki simge sırasında ve bir başlık bir düğme dahil edilebilir, yer sınırlı olduğunda her ikisini birden değil birleştirilecek nedenle deneyin.
- **Açıkça işareti bozucu düğmeleri** - düğmesi bir bozucu gerçekleştirir (örneğin, bir dosyayı silmek), eylemi açıkça işaretlemek şekilde metin ve/veya simgesini kullanarak. Bozucu Eylemler her zaman mevcut bir [uyarı](~/ios/tvos/user-interface/alerts.md) eylemi sınırlamak için kullanıcı isteyen.
- **Kullanım geri düğmeleri yok** -Siri uzak menü düğmesi, önceki ekrana döndürmek için kullanılır. Bu kural için bir özel uygulama içi satın almalara bozucu eylemler için mi burada bir **iptal** düğmesi görüntülenmesi.

Odak ve gezinti ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [gezinti ve odak çalışma](~/ios/tvos/app-fundamentals/navigation-focus.md) belgeleri.

<a name="Button-Icons" />

### <a name="button-icons"></a>Düğme simgeleri

Apple basit, yüksek oranda tanınabilir görüntüleri düğmesi simgelerini kullanın önerir. Fazla karmaşık simgeleri bir kalkmadan yer arasında TV ekranında tanımak, böylece hakkında fikir edinmek olası en basit gösterimi kullanmaya çalıştığınızda zordur. Mümkün olduğunda iyi bildiğiniz (örneğin, Büyüteç arama için) simgeler için görüntü, standart kullanın.

<a name="Button-Titles" />

### <a name="button-titles"></a>Düğme başlıkları

Apple başlıklarını, düğmelerini oluştururken aşağıdaki önerileri vardır:

- **Açıklayıcı metin altındaki simgeleri düğme Göster** - mümkün simgesinin altında yer işaretini kaldırın, açıklayıcı metin başka get üzerinden düğmenin amacı yalnızca düğmeler. burada.
- **Fiiller veya fiil cümleleri başlığını kullanın** -açıkça durum gerçekleştirilecek eylemi yerleştirin kullanıcı düğmesini tıklattığında.
- **Başlık stili büyük/küçük harf kullanın** - makaleler, bağlaç veya edatlar dışında (dört harf veya daha az), her sözcüğün düğmenin başlığı büyük harfli.
- **-Noktaya başlık kısa kullanmak** -kısa olası duyuruları düğmenin eylemini tanımlamak için kullanın.

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>Düğmelerin ve film şeritleri

Xamarin.tvOS uygulama düğmelerini çalışmak için en kolay yolu bunları iOS için Xamarin Tasarımcısı'nı kullanarak uygulamanın UI eklemektir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **düğmesini** gelen **Kitaplığı** ve görünümünde bırak: 

    [![](buttons-images/storyboard01.png "Bir düğme")](buttons-images/storyboard01.png#lightbox)
1. İçinde **özellikleri Explorer**, gibi çeşitli özellikleri düğmesinin ayarlayın, **başlık** ve **metin rengi**: 

    [![](buttons-images/storyboard02.png "Düğme Özellikleri")](buttons-images/storyboard02.png#lightbox)
1. Ardından, geçiş **olaylar sekmesi** ve kablo yukarı bir **olay** gelen **düğmesini** ve çağrısından `ButtonPressed`: 

    [![](buttons-images/storyboard03.png "Olaylar sekmesi")](buttons-images/storyboard03.png#lightbox)
1. Sizin için otomatik olarak bırakılacak `ViewController.cs` burada yerleştirebileceğiniz yeni eylemi kullanarak kod görünümü **yukarı** ve **aşağı** ok tuşları: 

    [![](buttons-images/storyboard04.png "Yeni bir eylem kodda yerleştirme")](buttons-images/storyboard04.png#lightbox)
1. Tuşuna **Enter** konumu seçmek için: 

    [![](buttons-images/storyboard05.png "Kod Düzenleyicisi")](buttons-images/storyboard05.png#lightbox)
1. Tüm dosyalara değişiklikleri kaydedin.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **düğmesini** gelen **Kitaplığı** ve görünümünde bırak: 

    [![](buttons-images/storyboard01vs.png "Bir düğme")](buttons-images/storyboard01vs.png#lightbox)
1. İçinde **özellikleri Explorer**, gibi çeşitli özellikleri düğmesinin ayarlayın, **başlık** ve **metin rengi**: 

    [![](buttons-images/storyboard02vs.png "Özellikler Gezgini")](buttons-images/storyboard02vs.png#lightbox)
1. Ardından, geçiş **olaylar sekmesi** ve kablo yukarı bir **olay** gelen **düğmesini** ve çağrısından `ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "Olaylar sekmesi")](buttons-images/storyboard03vs.png#lightbox)
1. Tüm dosyalara değişiklikleri kaydedin.



Görünüm denetleyicinizi Düzenle (örneğin `ViewController.cs`) dosya ve seçilmesini düğmesi işlemek için aşağıdaki kodu ekleyin:


```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

Düğmenin sürece `Enabled` özelliği `true` ve başka bir denetim veya görünümü tarafından kapsanmıyor, Siri uzaktan kullanarak odak öğesi yapılabilir. Kullanıcı düğmesini seçer ve dokunma yüzeyini tıklar `ButtonPressed` yukarıda tanımlanan eylem yürütülebilir.

> [!IMPORTANT]
> **Not:** eylemler gibi atamak mümkün olmakla birlikte `TouchUpInside` için bir `UIButton` iOS oluştururken Tasarımcısı'nda bir **olay işleyicisi**, Apple TV dokunmatik ekran veya destek olmadığından hiçbir zaman çağrılacağı dokunma olayları. Varsayılan değer her zaman kullanmalısınız **eylem türü** oluştururken **Eylemler** tvOS kullanıcı arabirimi öğeleri için.




Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>Düğmeler ve kodu

İsteğe bağlı olarak, bir `UIButton` C# kodunda oluşturulur ve tvOS uygulamanın görünümüne eklenir. Örneğin:

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

Yeni bir oluşturduğunuzda `UIButton` kodda, belirttiğiniz kendi `UIButtonType` aşağıdakilerden biri olarak:

- **Sistem** -bu düğmenin tvOS tarafından sunulan standart türü ve en sık kullanacağınız türüdür.
- **DetailDisclosure** -ayrıntılı bilgi göstermek veya gizlemek için kullanılan düğmesi "azaltın" türünü gösterir.
- **InfoDark** -bir koyu ayrıntılı düğmesine bir daire içinde bir "i" görüntülenen bilgileri.
- **InfoLight** -ışık ayrıntılı düğmesine bir daire içinde bir "i" görüntülenen bilgileri.
- **AddContact** -düğme bir kişi Ekle düğmesi olarak görüntüler.
- **Özel** -düğmesinin birkaç nitelikler özelleştirmenizi sağlar.

Ardından, ekranda boyutu tanımlarsınız ve düğmesinin konumu. Örnek:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Ardından, düğme için başlık ayarlayın. `UIButtons` en çok farklı `UIKit` yalnızca yalnızca edilemez şekilde bunların bir duruma sahip değişiklik denetimleri başlığı, için değiştirmek zorunda bir verilen `UIControlState`. Örneğin:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Ardından, kullanın `AllEvents` kullanıcı düğmesine tıklandığında görmek için olay. Örnek:

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

Son olarak, düğme görüntülemek için görünümüne ekleyin:

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> **Not:** eylemler gibi atamak mümkün olmakla birlikte `TouchUpInside` için bir `UIButton`, Apple TV ekranında veya dokunma olaylarını destekleyen bir touch sahip olmadığından hiçbir zaman çağrılır. Olayları gibi her zaman kullanmalısınız **AllEvents** veya **PrimaryActionTriggered**.




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>Bir düğme stil oluşturma

tvOS sağlayan birkaç özelliklerini bir `UIButton` başlığını sağlamak ve stil, arka plan rengi ve resimler gibi ile kullanılabilir.

<a name="Button-Titles" />

### <a name="button-titles"></a>Düğme başlıkları

Yukarıdaki gördüğümüz gibi `UIButtons` en çok farklı `UIKit` yalnızca yalnızca edilemez şekilde bunların bir duruma sahip değişiklik denetimleri başlığı, için değiştirmek zorunda bir verilen `UIControlState`. Örneğin:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Düğmesini kullanarak için başlık rengi ayarlayabilirsiniz `SetTitleColor` yöntemi. Örneğin:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

Ve başlığın ayarlayabilirsiniz kullanarak gölge `SetTitleShadowColor`. Örneğin:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Başlık gölge değiştirmek için ayarlayabileceğiniz *Engraved* için *Kabarık* aşağıdaki kodu kullanarak düğmesi vurgulanmış zaman:

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

Ayrıca, öznitelikli metin düğmenin başlığı olarak kullanabilirsiniz. Örneğin:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Düğme resimlerini

A `UIButton` görüntünün bağlı olabilir ve bir görüntü, arka plan olarak kullanabilirsiniz.

Bir düğme için arka plan görüntüsü ayarlamak için bir verilen `UIControlState`, aşağıdaki kodu kullanın:

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Ayarlama `AdjustsImageWhenHiglighted` özelliğine `true` düğmesi vurgulanmış, açık resim çizmek için (varsayılan değer budur). Ayarlama `AdjustsImageWhenDisabled` özelliğine `true` düğmesi devre dışı bırakıldığında koyu resim çizmek için (yeniden, varsayılan değer budur).

Düğmeyi görüntülenen resmi ayarlamak için aşağıdaki kodu kullanın:

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Kullanım `TintColor` başlığı ve düğmenin resim için uygulanan bir renk TINT ayarlamak için özellik. Düğmeleri için `Custom` türü, bu özelliğin etkisi yoktur, uygulamanız gereken `TintColor` davranışı kendiniz.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve Xamarin.tvOS uygulama içinde düğmeleri ile çalışma ele. Tasarımcısı iOS düğmeleri ile nasıl çalışılacağını ve C# kodunda düğmeler oluşturmak nasıl oluşturulacağını gösterir. Son olarak, bir düğmenin başlığı değiştirmek ve kendi stil ve görünümünü değiştirmek nasıl oluşturulacağını gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
