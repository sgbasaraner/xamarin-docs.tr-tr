---
title: "Dokunun hareketi hareketi tanıyıcı ekleme"
description: "Dokunun hareketi dokunun algılama için kullanılır ve TapGestureRecognizer sınıfı uygulanır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: d767b50d98b88e6b97a07caffcc103c70cfda428
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Dokunun hareketi hareketi tanıyıcı ekleme

_Dokunun hareketi dokunun algılama için kullanılır ve TapGestureRecognizer sınıfı uygulanır._

## <a name="overview"></a>Genel Bakış

Bir kullanıcı arabirimi öğesi tıklanabilir dokunun hareketi sahip olmak için oluşturun bir [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) örneği, işleme [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) olay ve yeni hareketi tanıyıcı eklemek [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) kullanıcı arabirimi öğesi koleksiyonu. Aşağıdaki örnekte gösterildiği kod bir `TapGestureRecognizer` bağlı bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğe:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

Varsayılan olarak, görüntünün tek dokunma yanıt verir. Ayarlama [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) özelliği çift dokunmayla (veya gerekirse daha fazla Tap) için bekleyin.

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Zaman [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) ayarlanmış Tap ayarlanmış bir süre sonunda süresi (Bu dönem değil yapılandırılabilir) oluşursa bir olay işleyicisi yalnızca yürütülür. İkinci (veya sonraki) dokunma belirtilen süre içinde değil oluşursa etkili bir şekilde dikkate alınmaz ve 'dokunun count' yeniden başlatır.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>XAML kullanma

Hareketi tanıyıcı ekli özellikler kullanarak Xaml denetiminde eklenebilir. Eklemek için söz dizimi bir [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) görüntüye aşağıda verilmiştir (Bu durumda tanımlayan bir *çift dokunun* olay):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Bir sayaç artırılır ve görüntü rengi siyah olarak değişir (örnekteki) olay işleyicisi için kod &amp; beyaz.

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

## <a name="using-icommand"></a>ICommand kullanma

Genellikle Mvvm desen kullanan uygulamaları kullanmak `ICommand` yerine doğrudan kablolama olay işleyicilerini ayarlama. [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Kolayca destekleyebilir `ICommand` kodda bağlama ayarlayarak ya da:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

ya da Xaml kullanarak:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Bu görünüm modeli için tam kod örnek bulunabilir. İlgili `Command` uygulama ayrıntıları aşağıda gösterilmektedir:

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

Dokunun hareketi dokunun algılama için kullanılır ve ile uygulanan [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) sınıfı. Dokunma sayısı, çift dokunmayla tanımak için belirtilebilir (üçlü dokunma ya da daha fazla dokunur) davranışı.


## <a name="related-links"></a>İlgili bağlantılar

- [TapGesture (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [TapGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)
