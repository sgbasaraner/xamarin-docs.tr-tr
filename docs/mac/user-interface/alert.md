---
title: "Uyarılar"
description: "Bu makalede Xamarin.Mac uygulamasında uyarılarla çalışma kapsar. Oluşturma ve C# kodundan uyarıları görüntüleme ve Kullanıcı etkileşimlerine yanıt açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 8901bb57ace4f05e8c26fdc43dfe8c476927903a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="alerts"></a>Uyarılar

_Bu makalede Xamarin.Mac uygulamasında uyarılarla çalışma kapsar. Oluşturma ve C# kodundan uyarıları görüntüleme ve Kullanıcı etkileşimlerine yanıt açıklar._

C# ve .NET Xamarin.Mac uygulamada çalışırken erişimine sahip aynı içinde çalışan bir geliştirici uyarıları *Objective-C* ve *Xcode* yapar. 

Bir uyarı (hata gibi) ciddi bir sorun oluştuğunda görüntülenen iletişim özel türüdür ya da (örneğin, bir dosyayı silmek hazırlanıyor) bir uyarı olarak. Bir uyarı iletişim kutusu olduğundan kapatılabilmesi için de kullanıcı yanıtı gerektirir.

[![](alert-images/alert06.png "Bir örnek uyarı")](alert-images/alert06.png#lightbox)

Bu makalede, biz Xamarin.Mac uygulamada uyarıları ile çalışmanın temelleri ele alacağız. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>Uyarıları giriş

Bir uyarı (hata gibi) ciddi bir sorun oluştuğunda görüntülenen iletişim özel türüdür ya da (örneğin, bir dosyayı silmek hazırlanıyor) bir uyarı olarak. Kullanıcı üzerinde görevini ile devam etmeden önce bunlar kapatıldığında gerekir bu yana kullanıcı uyarıları kesintiye çünkü kesinlikle gerekli olmadığı sürece bir uyarı görüntüleme kaçının.

Apple aşağıdaki yönergeleri öneririz:

- Bir uyarı yalnızca kullanıcıların bilgi vermek için kullanmayın.
- Ortak ve geri alınamaz eylemler için bir uyarı görüntülemez. Bu durum veri kaybına neden olsa bile.
- Bir uyarının worthy bir durumdur, görüntülemek için herhangi bir kullanıcı Arabirimi öğesi veya yöntemi kullanarak kaçının.
- Uyarı simgesi kullanılmamalıdır.
- Uyarı durumu, uyarı iletisinde açıkça ve temellerini açıklar.
- Varsayılan düğme adı uyarı iletisinde açıklamak eylem karşılık gelmelidir.

Daha fazla bilgi için bkz: [uyarıları](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>Bir uyarı anatomisi

Yukarıda belirtildiği gibi uyarılar ciddi bir sorun ortaya çıktığında, uygulamanızın kullanıcı veya olası veri kaybına (örneğin, kaydedilmemiş bir dosyayı kapatma) için bir uyarı olarak gösterilmesi gerekir. Xamarin.Mac içinde bir uyarı C# kodunda, örneğin oluşturulur:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

Yukarıdaki kod uyarı simgesi, bir başlığı, bir uyarı iletisi ve tek bir koyulan uygulamaları simgesiyle bir uyarı görüntüler **Tamam** düğmesi:

[![](alert-images/alert01.png "Bir uyarı Tamam düğmesi")](alert-images/alert01.png#lightbox)

Apple bir uyarı özelleştirmek için kullanılan birkaç özellikleri sağlar:

- **AlertStyle** bir uyarı türü şunlardan biri tanımlar:
    - **Uyarı** - kritik olmayan geçerli veya yaklaşan bir olay kullanıcıyı uyarmak için kullanılan. Varsayılan stili budur.
    - **Bilgilendirme** - geçerli veya yaklaşan bir olay hakkında kullanıcıyı uyarmak için kullanılan. Şu anda arasında görünür fark yoktur bir **uyarı** ve **bilgilendirici**
    - **Kritik** - ciddi sonuçları (örneğin, bir dosyayı silmek) yaklaşan bir olay hakkında kullanıcıyı uyarmak için kullanılan. Bu tür uyarılar kullanılmamalıdır.
- **İleti metni** -bu ana ileti veya uyarının başlık ve durum kullanıcıya hızla tanımlamanız gerekir.
- **InformativeText** -burada durum açıkça tanımlayın ve kullanıcıya çalışılabilir seçenekleri sunmak uyarı gövdesidir.
- **Simge** -kullanıcıya görüntülenecek özel bir simgesi sağlar.
- **HelpAnchor** & **ShowsHelp** -HelpBook uygulamaya bağlıdır ve uyarı için Yardım görüntülemek uyarı verir.
- **Düğmeleri** -varsayılan olarak, bir uyarı yalnızca sahip **Tamam** düğmesi ancak **düğmeleri** koleksiyonu, gerektiği gibi daha fazla seçenek eklemenize olanak sağlar.
- **ShowsSuppressionButton** - `true` kullanıcı onu tetikleyen olayı sonraki oluşumları için uyarıyı gizlemek için kullanabileceğiniz bir onay kutusu görüntüler.
- **AccessoryView** -başka bir alt görünüm ekleme gibi ek bilgileri sağlamak için uyarı ekleme sayesinde bir **metin alanı** veri girişi için. Yeni bir ayarlarsanız **AccessoryView** veya mevcut bir değişiklik yapmak için çağırmanız gerekir `Layout()` uyarı görünür düzenini ayarlamak için yöntem.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>Bir uyarı görüntüleme

Bir uyarı olabileceğini görüntülenen, serbest kaydırılan iki farklı yolla veya bir sayfa olarak. Aşağıdaki kod bir uyarı olarak serbest kaydırılan görüntüler:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.RunModal ();
```
Bu kodu çalıştırırsanız, aşağıdaki görüntülenir:

[![](alert-images/alert02.png "Basit bir uyarı")](alert-images/alert02.png#lightbox)

Aşağıdaki kod, bir sayfa olarak aynı uyarının görüntüler:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

Bu kodu çalıştırırsanız, aşağıdaki görüntülenir:

[![](alert-images/alert03.png "Bir sayfa olarak görüntülenen bir uyarı")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>Uyarı düğmelerle çalışma

Varsayılan olarak, yalnızca bir uyarı görüntüler **Tamam** düğmesi. Ancak, sizin için onlara ekleyerek ek düğmeler oluşturabilirsiniz, sınırlı değildir **düğmeleri** koleksiyonu. Aşağıdaki kod ile serbest kaydırılan bir uyarı oluşturur bir **Tamam**, **iptal** ve **belki** düğmesi:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

Eklenen ilk düğme olacaktır _varsayılan düğme_ , etkinleştirilecek kullanıcı Enter tuşuna basarsa. Döndürülen değer, basılı kullanıcı düğmesini temsil eden bir tamsayı olacaktır. Örneğimizde, aşağıdaki değerleri döner:

- **TAMAM** - 1000.
- **İptal** - 1001.
- **Belki de** - 1002.

Size kodu çalıştırırsanız, aşağıdaki görüntülenir:

[![](alert-images/alert04.png "Üç düğme seçenekleri olan bir uyarı")](alert-images/alert04.png#lightbox)

Bir sayfa olarak aynı uyarının kod aşağıdaki gibidir:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}", result);
});
```
Bu kodu çalıştırırsanız, aşağıdaki görüntülenir:

[![](alert-images/alert05.png "Bir sayfa olarak görüntülenen üç düğme Uyarısı")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> Hiçbir zaman bir uyarı üçten fazla düğmeleri eklemeniz gerekir.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>Engelle düğmesini gösteren

Uyarının `ShowSuppressButton` özelliği `true`, kullanıcı onu tetikleyen olayı sonraki oluşumları için uyarıyı gizlemek için kullanabileceğiniz bir onay kutusu uyarı görüntüler. Aşağıdaki kod bastır düğmesiyle serbest kaydırılan bir uyarı görüntüler:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Varsa değerini `alert.SuppressionButton.State` olan `NSCellStateValue.On`, kullanıcı Gizle onay kutusu işaretli değilse, başka sahip oldukları değil.

Kodu çalıştırırsanız, aşağıdaki görüntülenir:

[![](alert-images/alert06.png "Bir uyarı gösterme düğmesi")](alert-images/alert06.png#lightbox)

Bir sayfa olarak aynı uyarının kod aşağıdaki gibidir:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Bu kodu çalıştırırsanız, aşağıdaki görüntülenir:

[![](alert-images/alert07.png "Bir uyarı gösterme düğmesi bir sayfa olarak görüntülemek")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>Özel bir alt görünüm ekleme

Uyarılar bir `AccessoryView` uyarı daha fazla özelleştirmek ve şeyler ister eklemek için kullanılan özellik bir **metin alanı** kullanıcı girişi için. Aşağıdaki kod ile eklenen metin giriş alanını serbest kaydırılan bir uyarı oluşturur:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Burada anahtar satırlar `var input = new NSTextField (new CGRect (0, 0, 300, 20));` oluşturan yeni bir **metin alanı** biz uyarı alacağınız. `alert.AccessoryView = input;` hangi ekler **metin alanı** uyarı ve çağrısı `Layout()` uyarı yeni alt görünüm uyacak şekilde yeniden boyutlandırmak için gerekli yöntemi.

Size kodu çalıştırırsanız, aşağıdaki görüntülenir:

[![](alert-images/alert08.png "Size kodu çalıştırırsanız, aşağıdaki görüntülenir")](alert-images/alert08.png#lightbox)

Bir sayfa olarak aynı uyarı şöyledir:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Biz bu kodu çalıştırmak, aşağıdaki görüntülenir:

[![](alert-images/alert09.png "Özel bir görünümü olan bir uyarı")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede bir Xamarin.Mac uygulamasında uyarılarla çalışma ayrıntılı bir bakış sürdü. Farklı türler ve uyarılar, oluşturma ve uyarıları özelleştirme ve C# kodunda uyarılarla çalışma nasıl kullanımlarını gördük.

## <a name="related-links"></a>İlgili bağlantılar

- [MacWindows (örnek)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows ile birlikte çalışma](~/mac/user-interface/window.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
