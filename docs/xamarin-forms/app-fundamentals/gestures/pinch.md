---
title: "Tutarak hareketi tanıyıcı ekleme"
description: "Tutarak hareketi etkileşimli yakınlaştırma gerçekleştirmek için kullanılır ve PinchGestureRecognizer sınıfı uygulanır. Tutarak hareketi için yaygın bir senaryo tutarak konumda görüntünün etkileşimli yakınlaştırma gerçekleştirmektir. Bu görünüm penceresinin içeriğini ölçeklendirme tarafından gerçekleştirilir ve bu makalede gösterilmiştir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 38e46af1d928a3d4e5dc33e2a46fe04cd169ed60
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Tutarak hareketi tanıyıcı ekleme

_Tutarak hareketi etkileşimli yakınlaştırma gerçekleştirmek için kullanılır ve PinchGestureRecognizer sınıfı uygulanır. Tutarak hareketi için yaygın bir senaryo tutarak konumda görüntünün etkileşimli yakınlaştırma gerçekleştirmektir. Bu görünüm penceresinin içeriğini ölçeklendirme tarafından gerçekleştirilir ve bu makalede gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Bir kullanıcı arabirimi öğesi yakınlaştırılabilir tutarak hareketi sahip olmak için oluşturun bir [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) örneği, işleme [ `PinchUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PinchGestureRecognizer.PinchUpdated/) olayı ve yeni hareketi tanıyıcı eklemek [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) kullanıcı arabirimi öğesi koleksiyonu. Aşağıdaki örnekte gösterildiği kod bir `PinchGestureRecognizer` bağlı bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğe:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Bu ayrıca XAML'de, aşağıdaki kod örneğinde gösterildiği gibi elde edilebilir:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kodu `OnPinchUpdated` olay işleyicisi sonra arka plan kod dosyasına eklenir:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>PinchToZoom kapsayıcı oluşturma

Yakınlaştırma işlemi gerçekleştirmek için tutarak hareketi işleme kullanıcı arabirimini dönüştürmek için bazı matematik gerektirir. Bu bölüm, etkileşimli olarak herhangi bir kullanıcı arabirimi öğesi yakınlaştırma için kullanılan matematik gerçekleştirmek için genelleştirilmiş yardımcı sınıfı içerir. Aşağıdaki örnekte gösterildiği kod `PinchToZoomContainer` sınıfı:

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

Böylece tutarak hareketi Sarmalanan bir kullanıcı arabirimi öğesi yakınlaşır Bu sınıf bir kullanıcı arabirimi öğesi Sarmalanan. Aşağıdaki XAML kodu örnekteki `PinchToZoomContainer` kaydırma bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğe:

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

Aşağıdaki örnekte gösterildiği kod nasıl `PinchToZoomContainer` saran bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) bir C# sayfasındaki öğe:

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

Zaman [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğe, tutarak hareketi alır, görüntülenen görüntünün uzaklaştırılacağını bileşenini veya yetersiz. Yakınlaştırma tarafından gerçekleştirilen `PinchZoomContainer.OnPinchUpdated` aşağıdaki kod örneğinde gösterildiği yöntemi:

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

Bu yöntem, kullanıcının tutarak hareketi üzerinde temel Sarmalanan bir kullanıcı arabirimi öğesi yakınlaştırma düzeyini güncelleştirir. Bu değerleri kullanılarak elde edilir [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale/), [ `ScaleOrigin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin/) ve [ `Status` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Status/) özelliklerini [ `PinchGestureUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureUpdatedEventArgs/) tutarak hareketi başlangıcı sırasında uygulanacak ölçek çarpanı hesaplamak için örneği. Sarmalanan kullanıcı öğesi ayarlayarak tutarak hareketi kaynak sonra uzaklaştırılacağını kendi [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/), ve [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) hesaplanan değerler özellikleri.

## <a name="summary"></a>Özet

Tutarak hareketi etkileşimli yakınlaştırma gerçekleştirmek için kullanılır ve ile uygulanan [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) sınıfı.


## <a name="related-links"></a>İlgili bağlantılar

- [PinchGesture (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PinchGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)
