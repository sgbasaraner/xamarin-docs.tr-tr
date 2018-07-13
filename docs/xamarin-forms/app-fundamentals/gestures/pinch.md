---
title: Tabletinizde hareket tanıyıcı ekleme
description: Bu makalede, etkileşimli yakınlaştırma tabletinizde konumda görüntünün gerçekleştirmek için tabletinizde hareket kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 37befdcd4ccbcd49e3cebda92d55ae6f70da2ad6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998709"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Tabletinizde hareket tanıyıcı ekleme

_Tabletinizde hareket etkileşimli yakınlaştırma gerçekleştirmek için kullanılır ve PinchGestureRecognizer sınıfıyla uygulanır. Tabletinizde hareket için yaygın bir senaryo, görüntünün tabletinizde konumda etkileşimli yakınlaştırma gerçekleştirmektir. Bu görünüm penceresinin içeriğini ölçeklendirme tarafından gerçekleştirilir ve bu makalede gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Bir kullanıcı arabirimi öğesi yakınlaştırılabilir tabletinizde hareket sahip olmak için oluşturun bir [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) örneği, işlemek [ `PinchUpdated` ](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) olay ve eklemek için yeni hareket tanıyıcı [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) kullanıcı arabirimi öğesi koleksiyonu. Aşağıdaki kod örnekte gösterildiği bir `PinchGestureRecognizer` iliştirilmiş bir [ `Image` ](xref:Xamarin.Forms.Image) öğesi:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Bu ayrıca XAML içinde aşağıdaki kod örneğinde gösterildiği gibi gerçekleştirilebilir:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kodu `OnPinchUpdated` olay işleyicisi ardından arka plan kod dosyasına eklenir:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>PinchToZoom bir kapsayıcı oluşturma

Yakınlaştırma işlemi gerçekleştirmek için tabletinizde hareket işleme kullanıcı arabirimini dönüştürmek için bazı matematik gerektirir. Bu bölüm, etkileşimli bir kullanıcı arabirimi öğesi yakınlaştırmak için kullanılan matematik gerçekleştirmek için genelleştirilmiş yardımcı bir sınıf içerir. Aşağıdaki örnekte gösterildiği kod `PinchToZoomContainer` sınıfı:

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

Tabletinizde hareket Sarmalanan kullanıcı arabirimi öğesi yakınlaşır. böylece bu sınıf kullanıcı arabirimi öğesi sarmalanabilir. Aşağıdaki XAML kod örnekte gösterildiği `PinchToZoomContainer` sarmalama bir [ `Image` ](xref:Xamarin.Forms.Image) öğesi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Aşağıdaki kod örnekte gösterildiği nasıl `PinchToZoomContainer` saran bir [ `Image` ](xref:Xamarin.Forms.Image) bir C# sayfasındaki öğe:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

Zaman [ `Image` ](xref:Xamarin.Forms.Image) öğesi tabletinizde hareket alır, görüntülenen görüntünün uzaklaştırılacağını açma veya giden. Yakınlaştırma tarafından gerçekleştirilen `PinchZoomContainer.OnPinchUpdated` aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

Bu yöntem, kullanıcının üzerinde tabletinizde hareket tabanlı Sarmalanan kullanıcı arabirimi öğesi yakınlaştırma seviyesini güncelleştirir. Bu değerleri kullanılarak elde edilir [ `Scale` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [ `ScaleOrigin` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) ve [ `Status` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) özelliklerini [ `PinchGestureUpdatedEventArgs` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) örneği tabletinizde hareket başlangıcı sırasında uygulanması için ölçek faktörünü hesaplayabilirsiniz. Sarmalanan kullanıcı öğesi ayarlayarak tabletinizde hareket kaynağını ardından uzaklaştırılacağını kendi [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY), ve [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) hesaplanan değerler özellikleri.

## <a name="summary"></a>Özet

Tabletinizde hareket ile birlikte uygulanır ve etkileşimli yakınlaştırma gerçekleştirmek için kullanılan [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) sınıfı.


## <a name="related-links"></a>İlgili bağlantılar

- [PinchGesture (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
