---
title: C# yerel görünümleri
description: İOS, Android ve UWP yerel görünümleri doğrudan C# kullanarak oluşturulan Xamarin.Forms sayfalarından başvurulabilir. Bu makalede, C# kullanılarak oluşturulan bir Xamarin.Forms düzene yerel görünümler ekleme ve bunların ölçüm API'si kullanımı düzeltmek için özel görünümler düzenini geçersiz kılmak nasıl gösterilmektedir.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: c3a79947b02e0f877fd4ea1b0ddb72486c222719
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34050058"
---
# <a name="native-views-in-c"></a>C# yerel görünümleri

_İOS, Android ve UWP yerel görünümleri doğrudan C# kullanarak oluşturulan Xamarin.Forms sayfalarından başvurulabilir. Bu makalede, C# kullanılarak oluşturulan bir Xamarin.Forms düzene yerel görünümler ekleme ve bunların ölçüm API'si kullanımı düzeltmek için özel görünümler düzenini geçersiz kılmak nasıl gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Veren herhangi bir Xamarin.Forms denetimi `Content` çok ayarlamak için veya bir `Children` koleksiyonu, platforma özgü görünümler ekleyebilir. Örneğin, bir iOS `UILabel` doğrudan eklenebilir [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) özelliği veya [ `StackLayout.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) koleksiyonu. Ancak, bu işlev kullanımı gerektirdiğini unutmayın `#if` Xamarin.Forms paylaşılan proje çözümleri tanımlar ve Xamarin.Forms .NET standart kitaplığı çözümlerinden kullanılamaz.

Aşağıdaki ekran görüntüleri platforma özgü görünümler Xamarin.Forms için eklenene göstermek [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/):

[![](code-images/screenshots-sml.png "Platforma özgü görünümler içeren StackLayout")](code-images/screenshots.png#lightbox "platforma özgü görünümler içeren StackLayout")

Xamarin.Forms düzene platforma özgü görünümler ekleme yeteneği her platformda iki genişletme yöntemleri tarafından etkinleştirilir:

- `Add` – bir platforma özgü görünümüne ekler [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) bir düzen koleksiyonu.
- `ToView` – bir platforma özgü görünümü alır ve bir Xamarin.Forms sarmalar [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) olarak ayarlanabilir `Content` denetiminin özelliği.

Xamarin.Forms paylaşılan projede bu yöntemleri kullanarak uygun platforma özgü Xamarin.Forms ad almayı gerektirir:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Evrensel Windows Platformu (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Her platformda platforma özgü görünümler ekleme

Aşağıdaki bölümlerde her platformda Xamarin.Forms Düzen platforma özgü görünümler nasıl ekleneceğini gösterir.

### <a name="ios"></a>iOS

Aşağıdaki kod örneğinde nasıl ekleneceğini gösterir bir `UILabel` için bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ve [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var uiLabel = new UILabel {
  MinimumFontSize = 14f,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = originalText,
};
stackLayout.Children.Add (uiLabel);
contentView.Content = uiLabel.ToView();
```

Örnek varsayar `stackLayout` ve `contentView` örnekleri, daha önce XAML veya C# içinde oluşturuldu.

### <a name="android"></a>Android

Aşağıdaki kod örneğinde nasıl ekleneceğini gösterir bir `TextView` için bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ve [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

Örnek varsayar `stackLayout` ve `contentView` örnekleri, daha önce XAML veya C# içinde oluşturuldu.

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Aşağıdaki kod örneğinde nasıl ekleneceğini gösterir bir `TextBlock` için bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) ve [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textBlock = new TextBlock
{
    Text = originalText,
    FontSize = 14,
    FontFamily = new FontFamily("HelveticaNeue"),
    TextWrapping = TextWrapping.Wrap
};
stackLayout.Children.Add(textBlock);
contentView.Content = textBlock.ToView();
```

Örnek varsayar `stackLayout` ve `contentView` örnekleri, daha önce XAML veya C# içinde oluşturuldu.

## <a name="overriding-platform-measurements-for-custom-views"></a>Özel görünümler için platform ölçümleri geçersiz kılma

Her platformda özel görünümleri genellikle yalnızca doğru bunlar tasarlanmış düzen senaryosu için ölçüm uygulayın. Örneğin, özel bir görünüm yalnızca yarısı aygıt kullanılabilir genişliğini kaplayacak şekilde tasarlanmış olmasını. Ancak, diğer kullanıcılarla paylaşılmasını sonra özel görünüm aygıt tam kullanılabilir genişliğini kaplayacak gerekli olabilir. Bu nedenle, bir Xamarin.Forms düzeni yeniden kullanılıyor olduğunda özel görünümler ölçüm uygulama geçersiz kılmak gerekli olabilir. Bu nedenle, `Add` ve `ToView` genişletme yöntemleri sağlayan bir Xamarin.Forms düzene eklendiğinde, özel görünüm düzeni geçersiz kılabilirsiniz belirtilmesi ölçüm temsilciler izin geçersiz kılar.

Aşağıdaki bölümlerde, ölçüm API'si kullanımı düzeltmek için özel görünümler düzenini geçersiz kılmak nasıl ekleyebileceğiniz gösterilmektedir.

### <a name="ios"></a>iOS

Aşağıdaki örnekte gösterildiği kod `CustomControl` devraldığı sınıfı `UILabel`:

```csharp
public class CustomControl : UILabel
{
  public override string Text {
    get { return base.Text; }
    set { base.Text = value.ToUpper (); }
  }

  public override CGSize SizeThatFits (CGSize size)
  {
    return new CGSize (size.Width, 150);
  }
}
```

Bu görünüm örneği eklenen bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Ancak, çünkü `CustomControl.SizeThatFits` geçersiz kılma her zaman 150 yüksekliğini döndürür, görünümü boş alanı üstüne ve altına, aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

![](code-images/ios-bad-measurement.png "iOS özel denetim hatalı SizeThatFits uygulaması")

Bu sorun için çözüm sağlamaktır bir `GetDesiredSizeDelegate` aşağıdaki kod örneğinde gösterildiği gibi uygulama:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, double width, double height)
{
  var uiView = renderer.Control;

  if (uiView == null) {
    return null;
  }

  var constraint = new CGSize (width, height);

  // Let the CustomControl determine its size (which will be wrong)
  var badRect = uiView.SizeThatFits (constraint);

  // Use the width and substitute the height
  return new SizeRequest (new Size (badRect.Width, 70));
}
```

Bu yöntem tarafından sağlanan genişliğini kullanır `CustomControl.SizeThatFits` yöntemi, ancak 70 yüksekliğini 150 yüksekliğini değiştirir. Zaman `CustomControl` örneği eklenen [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), `FixSize` yöntemi olarak belirtilebilir `GetDesiredSizeDelegate` tarafından sağlanan hatalı ölçüm düzeltmek için `CustomControl` sınıfı:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Aşağıdaki ekran görüntüsünde gösterildiği gibi boş alanı üstüne ve altına, olmadan doğru görüntülenmesini özel görünümde bu sonuçları:

![](code-images/ios-good-measurement.png "iOS özel denetim GetDesiredSize geçersiz kılma ile")

### <a name="android"></a>Android

Aşağıdaki örnekte gösterildiği kod `CustomControl` devraldığı sınıfı `TextView`:

```csharp
public class CustomControl : TextView
{
  public CustomControl (Context context) : base (context)
  {
  }

  protected override void OnMeasure (int widthMeasureSpec, int heightMeasureSpec)
  {
    int width = MeasureSpec.GetSize (widthMeasureSpec);

    // Force the width to half of what's been requested.
    // This is deliberately wrong to demonstrate providing an override to fix it with.
    int widthSpec = MeasureSpec.MakeMeasureSpec (width / 2, MeasureSpec.GetMode (widthMeasureSpec));

    base.OnMeasure (widthSpec, heightMeasureSpec);
  }
}
```

Bu görünüm örneği eklenen bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Ancak, çünkü `CustomControl.OnMeasure` geçersiz kılma her zaman döndürür istenen genişliği yarısı, görünüm yalnızca yarı kullanılabilir cihaz genişliğini kaplayan aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

![](code-images/android-bad-measurement.png "Android özel denetim hatalı OnMeasure uygulaması")

Bu sorun için çözüm sağlamaktır bir `GetDesiredSizeDelegate` aşağıdaki kod örneğinde gösterildiği gibi uygulama:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, int widthConstraint, int heightConstraint)
{
  var nativeView = renderer.Control;

  if ((widthConstraint == 0 && heightConstraint == 0) || nativeView == null) {
    return null;
  }

  int width = Android.Views.View.MeasureSpec.GetSize (widthConstraint);
  int widthSpec = Android.Views.View.MeasureSpec.MakeMeasureSpec (
    width * 2, Android.Views.View.MeasureSpec.GetMode (widthConstraint));
  nativeView.Measure (widthSpec, heightConstraint);
  return new SizeRequest (new Size (nativeView.MeasuredWidth, nativeView.MeasuredHeight));
}
```

Bu yöntem tarafından sağlanan genişliğini kullanır `CustomControl.OnMeasure` yöntemi, ancak çarpar, iki tarafından. Zaman `CustomControl` örneği eklenen [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), `FixSize` yöntemi olarak belirtilebilir `GetDesiredSizeDelegate` tarafından sağlanan hatalı ölçüm düzeltmek için `CustomControl` sınıfı:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Bu olan özel görünüm düzgün görüntülenmeyebilir, aygıtı genişliğini kaplayan aşağıdaki ekran görüntüsünde gösterildiği gibi olur:

![](code-images/android-good-measurement.png "Android özel denetim özel GetDesiredSize temsilci ile")

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Aşağıdaki örnekte gösterildiği kod `CustomControl` devraldığı sınıfı `Panel`:

```csharp
public class CustomControl : Panel
{
  public static readonly DependencyProperty TextProperty =
    DependencyProperty.Register(
      "Text", typeof(string), typeof(CustomControl), new PropertyMetadata(default(string), OnTextPropertyChanged));

  public string Text
  {
    get { return (string)GetValue(TextProperty); }
    set { SetValue(TextProperty, value.ToUpper()); }
  }

  readonly TextBlock textBlock;

  public CustomControl()
  {
    textBlock = new TextBlock
    {
      MinHeight = 0,
      MaxHeight = double.PositiveInfinity,
      MinWidth = 0,
      MaxWidth = double.PositiveInfinity,
      FontSize = 14,
      TextWrapping = TextWrapping.Wrap,
      VerticalAlignment = VerticalAlignment.Center
    };

    Children.Add(textBlock);
  }

  static void OnTextPropertyChanged(DependencyObject dependencyObject, DependencyPropertyChangedEventArgs args)
  {
    ((CustomControl)dependencyObject).textBlock.Text = (string)args.NewValue;
  }

  protected override Size ArrangeOverride(Size finalSize)
  {
      // This is deliberately wrong to demonstrate providing an override to fix it with.
      textBlock.Arrange(new Rect(0, 0, finalSize.Width/2, finalSize.Height));
      return finalSize;
  }

  protected override Size MeasureOverride(Size availableSize)
  {
      textBlock.Measure(availableSize);
      return new Size(textBlock.DesiredSize.Width, textBlock.DesiredSize.Height);
  }
}
```

Bu görünüm örneği eklenen bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Ancak, çünkü `CustomControl.ArrangeOverride` geçersiz kılma her zaman döndürür istenen genişliği yarısı, aşağıdaki ekran görüntüsünde gösterildiği gibi cihaz yarım kullanılabilir genişliğini için görünümü kırpılacak:

![](code-images/winrt-bad-measurement.png "UWP özel denetim hatalı ArrangeOverride uygulaması")

Bu sorun için çözüm sağlamaktır bir `ArrangeOverrideDelegate` görünümüne eklerken uygulama [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
stackLayout.Children.Add(fixedControl, arrangeOverrideDelegate: (renderer, finalSize) =>
{
    if (finalSize.Width <= 0 || double.IsInfinity(finalSize.Width))
    {
        return null;
    }
    var frameworkElement = renderer.Control;
    frameworkElement.Arrange(new Rect(0, 0, finalSize.Width * 2, finalSize.Height));
    return finalSize;
});
```

Bu yöntem tarafından sağlanan genişliğini kullanır `CustomControl.ArrangeOverride` yöntemi, ancak çarpar, iki tarafından. Bu olan özel görünüm düzgün görüntülenmeyebilir, aygıtı genişliğini kaplayan aşağıdaki ekran görüntüsünde gösterildiği gibi olur:

![](code-images/winrt-good-measurement.png "UWP özel denetim ArrangeOverride temsilci ile")

## <a name="summary"></a>Özet

Bu makalede, C# kullanılarak oluşturulan bir Xamarin.Forms düzene yerel görünümler ekleme ve bunların ölçüm API'si kullanımı düzeltmek için özel görünümler düzenini geçersiz kılmak nasıl açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [NativeEmbedding (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [Yerel Formlar](~/xamarin-forms/platform/native-forms.md)
