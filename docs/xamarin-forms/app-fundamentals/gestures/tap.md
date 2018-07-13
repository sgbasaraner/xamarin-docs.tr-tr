---
title: Bir dokunma hareketi hareket tanıyıcı ekleme
description: Bu makalede, bir Xamarin.Forms uygulaması dokunun algılama için dokunma hareketi kullanmayı açıklar. Dokunun algılama TapGestureRecognizer sınıfıyla uygulanır.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: e602ae1f140640d9a895b65d78feab3d0a3b7861
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994860"
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Bir dokunma hareketi hareket tanıyıcı ekleme

_Dokunma hareketi dokunun algılama için kullanılır ve TapGestureRecognizer sınıfıyla uygulanır._

## <a name="overview"></a>Genel Bakış

Bir kullanıcı arabirimi öğesi tıklanabilir dokunma hareketi sahip olmak için oluşturun bir [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) örneği, işlemek [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) olay ve eklemek için yeni hareket tanıyıcı [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) kullanıcı arabirimi öğesi koleksiyonu. Aşağıdaki kod örnekte gösterildiği bir `TapGestureRecognizer` iliştirilmiş bir [ `Image` ](xref:Xamarin.Forms.Image) öğesi:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Varsayılan olarak, görüntü için tek bir dokunma yanıt verir. Ayarlama [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) çift dokunun (veya gerekirse daha fazla Tap'ları) için beklenecek özelliği.

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Zaman [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) ayarlanır (Bu dönem değil yapılandırılabilir) zaman belirli bir süre içinde Tap'ları meydana gelirse birini olay işleyicisi yalnızca yürütülür. Belirtilen süre içinde ikinci (veya sonraki) Tap'ları gerçekleşmez gözardı etkili bir şekilde ve 'dokunun count' yeniden başlatır.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>XAML kullanarak

Hareket tanıyıcı, ekli özellikler kullanarak Xaml içindeki bir denetime eklenebilir. Eklemek için söz dizimi bir [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) görüntüye aşağıda gösterilen (Bu durumda tanımlayan bir *çift dokunun* olay):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

(Örnek) olay işleyicisi için kod bir sayaç artırılır ve görüntü rengi siyah olarak değiştirir. &amp; beyaz.

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## <a name="using-icommand"></a>ICommand'ı kullanma

Genellikle Mvvm düzenini kullanan uygulamaları kullanın `ICommand` doğrudan teknik olay işleyicileri'kurmak yerine. [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Kolayca destekleyebilir `ICommand` kod içinde bağlama ayarlayarak ya da:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

veya Xaml kullanarak:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Bu görünüm modeli için tam kod örneği bulunabilir. İlgili `Command` uygulama ayrıntıları aşağıda gösterilmektedir:

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```

## <a name="summary"></a>Özet

Dokunma hareketi dokunun algılama için kullanılır ve ile uygulanan [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) sınıfı. Tap'ları sayısı, çift dokunmayla tanımak için belirtilebilir (üçlü dokunun ya da daha fazla dokunduğunda) davranışı.


## <a name="related-links"></a>İlgili bağlantılar

- [TapGesture (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
