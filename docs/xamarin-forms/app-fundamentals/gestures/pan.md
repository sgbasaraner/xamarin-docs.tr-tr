---
title: Pan hareket tanıyıcı ekleme
description: Bu makalede, böylece, resim boyutları daha küçük bir Görünüm penceresi içinde görüntülenen tüm görüntü içeriğin görüntülenebilir bir yatay kaydırma hareketi için yatay ve dikey bir görüntü sürükleyin nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 45c0a1452916f193236e5ba741f8e8e19b6691aa
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996812"
---
# <a name="adding-a-pan-gesture-recognizer"></a>Pan hareket tanıyıcı ekleme

_Kaydırma hareketi sürükleyerek algılamak için kullanılır ve PanGestureRecognizer sınıfıyla uygulanır. Böylece, resim boyutları daha küçük bir Görünüm penceresi içinde görüntülenen tüm görüntü içeriğin görüntülenebilir kaydırma hareketi için yaygın bir senaryo yatay ve dikey olarak bir görüntü sürüklemektir. Bu görüntüyü görünüm penceresinin içinde taşıyarak elde edilir ve bu makalede gösterilmiştir._

## <a name="overview"></a>Genel Bakış

Bir kullanıcı arabirimi öğesi sürüklenebilir kaydırma hareketi ile yapmak için oluşturma bir [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) örneği, işleme [ `PanUpdated` ](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) olay ve eklemek için yeni hareket tanıyıcı [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) kullanıcı arabirimi öğesi koleksiyonu. Aşağıdaki kod örnekte gösterildiği bir `PanGestureRecognizer` iliştirilmiş bir [ `Image` ](xref:Xamarin.Forms.Image) öğesi:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Bu ayrıca XAML içinde aşağıdaki kod örneğinde gösterildiği gibi gerçekleştirilebilir:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Kodu `OnPanUpdated` olay işleyicisi ardından arka plan kod dosyasına eklenir:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Android'de doğru kaydırma gerektiren [Xamarin.Forms 2.1.0-pre1 NuGet paketini](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) en az.

## <a name="creating-a-pan-container"></a>Pan bir kapsayıcı oluşturma

Bu bölüm, genellikle görüntü veya haritalar içinde gezinmek için uygun olan serbest biçimli kaydırma gerçekleştiren genelleştirilmiş yardımcı bir sınıf içerir. Bir sürükleme işlemi gerçekleştirmek için pan hareket işleme kullanıcı arabirimini dönüştürmek için bazı matematik gerektirir. Bu matematik Sarmalanan kullanıcı arabirimi öğesi sınırları içinde yalnızca sürüklemek için kullanılır. Aşağıdaki örnekte gösterildiği kod `PanContainer` sınıfı:

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

Yatay kaydırma hareketi Sarmalanan kullanıcı arabirimi öğesi sürükleyin, böylece bu sınıf kullanıcı arabirimi öğesi sarmalanabilir. Aşağıdaki XAML kod örnekte gösterildiği `PanContainer` sarmalama bir [ `Image` ](xref:Xamarin.Forms.Image) öğesi:

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

Aşağıdaki kod örnekte gösterildiği nasıl `PanContainer` saran bir [ `Image` ](xref:Xamarin.Forms.Image) bir C# sayfasındaki öğe:

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

Her iki örnek [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) ve [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) özellikleri görüntülenen görüntünün genişlik ve yükseklik değerlerini ayarlayın.

Zaman [ `Image` ](xref:Xamarin.Forms.Image) öğesi bir yatay kaydırma hareketi alır, görüntülenen görüntünün sürüklediğiniz. Sürükleme tarafından gerçekleştirilen `PanContainer.OnPanUpdated` aşağıdaki kod örneğinde gösterilen yöntemi:

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

Bu yöntem, kullanıcının kaydırma hareketi üzerinde temel Sarmalanan kullanıcı arabirimi öğesi görüntülenebilir içeriğini günceller. Bu değerleri kullanılarak elde edilir [ `TotalX` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) ve [ `TotalY` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) özelliklerini [ `PanUpdatedEventArgs` ](xref:Xamarin.Forms.PanUpdatedEventArgs) yönü hesaplamak için örnek ve kaydırma uzaklığı. `App.ScreenWidth` Ve `App.ScreenHeight` Özellikler penceresinin genişliği ve yüksekliği sağlayın ve ekran genişliği ve cihazın ekran yüksekliği değerlerine karşılık gelen bir platforma özgü projeleri tarafından ayarlanır. Sarmalanan kullanıcı öğesini ayarlayarak ardından sürüklediğiniz kendi [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) ve [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) hesaplanan değerler özellikleri.

Görünüm penceresinin genişliğini ve yüksekliğini tam ekranı kaplayan olmayan bir öğe içeriğinde kaydırma, öğenin alınabilir [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) ve [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) özellikleri.

> [!NOTE]
> Yüksek çözünürlüklü resimleri görüntüleyen bir uygulamanın bellek Ayak izi önemli ölçüde artırabilirsiniz. Bu nedenle, bunlar yalnızca gerekli ve uygulama artık gerektirdiği hemen sonra serbest bırakılması oluşturulmalıdır. Daha fazla bilgi için [resim kaynakları en iyi duruma getirme](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="summary"></a>Özet

Yatay kaydırma hareketi sürükleyerek algılamak için kullanılır ve ile uygulanan [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) sınıfı.



## <a name="related-links"></a>İlgili bağlantılar

- [PanGesture (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)
