---
title: C# içindeki yerel görünümler
description: İOS, Android ve UWP yerel görüntülerden oluşturulan C# kullanarak Xamarin.Forms sayfalarından doğrudan başvurulabilir. Bu makalede, C# kullanarak oluşturulan bir Xamarin.Forms düzenine yerel görünümler ekleme ve bunların ölçüm API'si kullanımı düzeltmek için özel görünüm düzenini geçersiz kılma gösterir.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: ad633f49c1c448529fa4c2b50483ec233c1ee841
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996200"
---
# <a name="native-views-in-c"></a>C# içindeki yerel görünümler

_İOS, Android ve UWP yerel görüntülerden oluşturulan C# kullanarak Xamarin.Forms sayfalarından doğrudan başvurulabilir. Bu makalede, C# kullanarak oluşturulan bir Xamarin.Forms düzenine yerel görünümler ekleme ve bunların ölçüm API'si kullanımı düzeltmek için özel görünüm düzenini geçersiz kılma gösterir._

## <a name="overview"></a>Genel Bakış

Veren herhangi bir Xamarin.Forms denetimi `Content` çok ayarlamak için veya bir `Children` koleksiyonu, platforma özgü görünümler ekleyebilir. Örneğin, bir iOS `UILabel` doğrudan eklenebilir [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) özelliği veya [ `StackLayout.Children` ](xref:Xamarin.Forms.Layout`1.Children) koleksiyonu. Ancak bu işlevselliği kullanımı gerektirdiğini unutmayın `#if` Xamarin.Forms paylaşılan proje içeren çözümlerde tanımlar ve Xamarin.Forms .NET Standard kitaplığı çözümlerinden kullanılamaz.

Platforma özgü görünümler için bir Xamarin.Forms eklenmiş aşağıdaki ekran görüntüleri gösteren [ `StackLayout` ](xref:Xamarin.Forms.StackLayout):

[![](code-images/screenshots-sml.png "Platforma özgü görünümler içeren StackLayout")](code-images/screenshots.png#lightbox "platforma özel görünümleri içeren StackLayout")

Platforma özgü görünümler bir Xamarin.Forms düzenine ekleme olanağı iki uzantı yöntemi her platformda etkinleştirilir:

- `Add` – bir platforma özgü görünümüne ekler [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) bir düzen koleksiyonu.
- `ToView` – bir platforma özgü görünümü alır ve bir Xamarin.Forms sarmalar [ `View` ](xref:Xamarin.Forms.View) olarak ayarlanabilir `Content` denetimin özellik.

Bu yöntemleri kullanarak bir Xamarin.Forms paylaşılan projede uygun platforma özgü Xamarin.Forms ad alanı alma gerektirir:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Evrensel Windows Platformu (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Her platformda platforma özgü görünüm ekleme

Aşağıdaki bölümlerde her platformda bir Xamarin.Forms Düzen platforma özgü görünümler nasıl ekleneceğini gösterir.

### <a name="ios"></a>iOS

Aşağıdaki kod örneğinde nasıl ekleneceğini gösterir. bir `UILabel` için bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) ve [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

Örnek olduğunu varsayar `stackLayout` ve `contentView` örnekleri, daha önce XAML veya C# içinde oluşturuldu.

### <a name="android"></a>Android

Aşağıdaki kod örneğinde nasıl ekleneceğini gösterir. bir `TextView` için bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) ve [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

Örnek olduğunu varsayar `stackLayout` ve `contentView` örnekleri, daha önce XAML veya C# içinde oluşturuldu.

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Aşağıdaki kod örneğinde nasıl ekleneceğini gösterir. bir `TextBlock` için bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) ve [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

Örnek olduğunu varsayar `stackLayout` ve `contentView` örnekleri, daha önce XAML veya C# içinde oluşturuldu.

## <a name="overriding-platform-measurements-for-custom-views"></a>Platform ölçümlerine özel görünümler için geçersiz kılma

Özel görünümler her platformda, genellikle yalnızca doğru kullanıcılar için tasarlanmış düzen senaryo için ölçüm uygulayın. Örneğin, bir özel görünüm yalnızca cihaz kullanılabilir genişliğini yarısını kaplayacak şekilde tasarlanmış olmasını. Ancak, diğer kullanıcılarla paylaşılıyor sonra özel görünüm cihazın tam kullanılabilir genişlik kaplaması için gerekli olabilir. Bu nedenle, bir Xamarin.Forms düzende yeniden kullanılmasını, özel görünümler ölçüm uygulama geçersiz kılmak gerekli olabilir. Bu nedenle `Add` ve `ToView` Xamarin.Forms düzene eklendiğinde, özel görünüm yerleşimini kılabilirsiniz belirtilmesi ölçüm temsilciler izin geçersiz kılmaları genişletme yöntemleri sağlar.

Aşağıdaki bölümlerde, ölçüm API'si kullanımı düzeltmek için özel görünümler düzenini geçersiz kılmak nasıl ekleyebileceğiniz gösterilmektedir.

### <a name="ios"></a>iOS

Aşağıdaki örnekte gösterildiği kod `CustomControl` öğesinden devralan sınıf `UILabel`:

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

Bu görünüm örneği eklenen bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Ancak, çünkü `CustomControl.SizeThatFits` geçersiz kılma her zaman 150 yüksekliğini döndürür, görünümün üstüne ve altına, boş alana sahip aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

![](code-images/ios-bad-measurement.png "iOS özel denetim hatalı SizeThatFits uygulaması")

Bu soruna bir çözüm sağlamaktır bir `GetDesiredSizeDelegate` aşağıdaki kod örneğinde gösterildiği gibi uygulama:

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

Bu yöntem tarafından sağlanan genişliğini kullanan `CustomControl.SizeThatFits` yöntemi, ancak bir yükseklikte 70 150 yüksekliğini yerini alır. Zaman `CustomControl` örneği eklenir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), `FixSize` yöntemi olarak belirtilebilir `GetDesiredSizeDelegate` tarafından sağlanan bir bozuk ölçü düzeltileceğini `CustomControl` sınıfı:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Aşağıdaki ekran görüntüsünde gösterildiği gibi altındaki ve üstündeki boş boşluk olmadan düzgün şekilde görüntülenmesini özel görünümünde bu sonuçları:

![](code-images/ios-good-measurement.png "iOS özel denetim GetDesiredSize geçersiz kılma ile")

### <a name="android"></a>Android

Aşağıdaki örnekte gösterildiği kod `CustomControl` öğesinden devralan sınıf `TextView`:

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

Bu görünüm örneği eklenen bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Ancak, çünkü `CustomControl.OnMeasure` geçersiz kılma her zaman döndürür istenen genişliği yarısı, görünüm cihazın kullanılabilir yalnızca yarım genişlik yer alan aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

![](code-images/android-bad-measurement.png "Hatalı OnMeasure uygulama ile Android özel denetim")

Bu soruna bir çözüm sağlamaktır bir `GetDesiredSizeDelegate` aşağıdaki kod örneğinde gösterildiği gibi uygulama:

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

Bu yöntem tarafından sağlanan genişliğini kullanan `CustomControl.OnMeasure` yöntemi, ancak çoğaltır, iki ile. Zaman `CustomControl` örneği eklenir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), `FixSize` yöntemi olarak belirtilebilir `GetDesiredSizeDelegate` tarafından sağlanan bir bozuk ölçü düzeltileceğini `CustomControl` sınıfı:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Bu özel görünüm olduğu doğru görüntüleniyorsa, cihaz genişliği kaplayan aşağıdaki ekran görüntüsünde gösterildiği gibi olur:

![](code-images/android-good-measurement.png "Özel GetDesiredSize temsilci ile Android özel denetim")

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Aşağıdaki örnekte gösterildiği kod `CustomControl` öğesinden devralan sınıf `Panel`:

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

Bu görünüm örneği eklenen bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Ancak, çünkü `CustomControl.ArrangeOverride` geçersiz kılma her zaman döndürür istenen genişliği yarısı, aşağıdaki ekran görüntüsünde gösterildiği gibi cihaz yarım kullanılabilir genişliğini için Görünüm kırpılır:

![](code-images/winrt-bad-measurement.png "Hatalı ArrangeOverride uygulama ile UWP özel denetim")

Bu soruna bir çözüm sağlamaktır bir `ArrangeOverrideDelegate` görünüme eklerken uygulama [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), aşağıdaki kod örneğinde gösterildiği gibi:

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

Bu yöntem tarafından sağlanan genişliğini kullanan `CustomControl.ArrangeOverride` yöntemi, ancak çoğaltır, iki ile. Bu özel görünüm olduğu doğru görüntüleniyorsa, cihaz genişliği kaplayan aşağıdaki ekran görüntüsünde gösterildiği gibi olur:

![](code-images/winrt-good-measurement.png "ArrangeOverride temsilci ile UWP özel denetim")

## <a name="summary"></a>Özet

Bu makalede, C# kullanarak oluşturulan bir Xamarin.Forms düzenine yerel görünümler ekleme ve bunların ölçüm API'si kullanımı düzeltmek için özel görünüm düzenini geçersiz kılma açıklaması.


## <a name="related-links"></a>İlgili bağlantılar

- [NativeEmbedding (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [Yerel Formlar](~/xamarin-forms/platform/native-forms.md)
