---
title: "Pan hareketi tanıyıcı ekleme"
description: "Pan hareketi sürükleyerek algılamak için kullanılır ve PanGestureRecognizer sınıfı uygulanır. Görüntü boyutları küçük bir görünüm penceresinin içinde görüntülenmektedir olduğunda tüm görüntü içeriğini görüntülenebilir pan hareketi için yaygın bir senaryo yatay ve dikey olarak bir görüntü sürüklemek için böylelikle. Bu görünüm penceresinin içinde görüntü taşıyarak gerçekleştirilir ve bu makalede gösterilmiştir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 4da42a92c83dcc1ec0b0ba2528de672e3fcf3be3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-pan-gesture-recognizer"></a>Pan hareketi tanıyıcı ekleme

_Pan hareketi sürükleyerek algılamak için kullanılır ve PanGestureRecognizer sınıfı uygulanır. Görüntü boyutları küçük bir görünüm penceresinin içinde görüntülenmektedir olduğunda tüm görüntü içeriğini görüntülenebilir pan hareketi için yaygın bir senaryo yatay ve dikey olarak bir görüntü sürüklemek için böylelikle. Bu görünüm penceresinin içinde görüntü taşıyarak gerçekleştirilir ve bu makalede gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Bir kullanıcı arabirimi öğesi sürüklenebilir pan hareketi sahip olmak için oluşturun bir [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) örneği, işleme [ `PanUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PanGestureRecognizer.PanUpdated/) olayı ve yeni hareketi tanıyıcı eklemek [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) kullanıcı arabirimi öğesi koleksiyonu. Aşağıdaki örnekte gösterildiği kod bir `PanGestureRecognizer` bağlı bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğe:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Bu ayrıca XAML'de, aşağıdaki kod örneğinde gösterildiği gibi elde edilebilir:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kodu `OnPanUpdated` olay işleyicisi sonra arka plan kod dosyasına eklenir:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Android doğru kaydırma gerektirir [Xamarin.Forms 2.1.0-pre1 NuGet paketi](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) en az.

## <a name="creating-a-pan-container"></a>Pan kapsayıcı oluşturma

Bu bölüm, genellikle görüntüleri veya eşlemeleri içinde gezinmek için de uygundur serbest kaydırma, gerçekleştiren bir genelleştirilmiş yardımcı sınıfı içerir. Sürükleme işlemi gerçekleştirmek için pan hareketi işleme kullanıcı arabirimini dönüştürmek için bazı matematik gerektirir. Bu matematik yalnızca Sarmalanan bir kullanıcı arabirimi öğesi sınırları içinde sürüklemek için kullanılır. Aşağıdaki örnekte gösterildiği kod `PanContainer` sınıfı:

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

Böylece pan hareketi Sarmalanan bir kullanıcı arabirimi öğesi sürükleyin Bu sınıf bir kullanıcı arabirimi öğesi Sarmalanan. Aşağıdaki XAML kodu örnekteki `PanContainer` kaydırma bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğe:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Aşağıdaki örnekte gösterildiği kod nasıl `PanContainer` saran bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) bir C# sayfasındaki öğe:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

Her iki örneklerde [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) ve [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) özellikleri görüntülenen görüntünün genişlik ve yükseklik değerleri ayarlayın.

Zaman [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğesi pan hareketi alır, görüntülenen görüntünün sürüklenebilir. Sürükle tarafından gerçekleştirilen `PanContainer.OnPanUpdated` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

Bu yöntem, kullanıcının pan hareketi üzerinde temel Sarmalanan bir kullanıcı arabirimi öğesi görüntülenebilir içeriği günceller. Bu değerleri kullanılarak elde edilir [ `TotalX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalX/) ve [ `TotalY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalY/) özelliklerini [ `PanUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanUpdatedEventArgs/) yönü hesaplamak için örnek ve Yatay kaydırma uzaklığı. `App.ScreenWidth` Ve `App.ScreenHeight` özellikler görünüm penceresinin genişliği ve yüksekliği sağlayın ve ekran genişliği ve ekran yükseklik değerleri cihaz için ilgili platforma özgü projeleri tarafından ayarlanır. Sarmalanan kullanıcı öğesi ayarlayarak sonra sürüklenen kendi [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) ve [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) hesaplanan değerler özellikleri.

Tam ekran kaplar olmayan bir öğe içeriğinde kaydırma olduğunda, görünüm penceresinin genişliği ve yüksekliği öğenin elde edilebilir [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) ve [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) özellikleri.

> [!NOTE]
> Yüksek çözünürlüklü görüntüleri görüntüleme, bir uygulamanın bellek alanını önemli ölçüde artırabilir. Bu nedenle, bunlar yalnızca gerekli ve uygulama artık gerektirmesi hemen serbest bırakılacak oluşturulmalıdır. Daha fazla bilgi için bkz: [görüntü kaynakları en iyi duruma getirme](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="summary"></a>Özet

Pan hareketi sürükleyerek algılamak için kullanılır ve ile uygulanan [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) sınıfı.



## <a name="related-links"></a>İlgili bağlantılar

- [PanGesture (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PanGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)
