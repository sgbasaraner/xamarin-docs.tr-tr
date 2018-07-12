---
title: Xamarin.Forms ScrollView
description: Bu makalede, yalnızca tek bir ekrana Sığdır kullanılamaz ve içerik klavye yer olan düzenleri sunmak için Xamarin.Forms ScrollView sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 814b74ce04269d18b9a280ada74204c1c86621e1
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38985984"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) düzenleri içerir ve kaydırma ekran dışında etkinleştirir. `ScrollView` klavye göstermek için ekran görünür bölümünün otomatik olarak taşımak görünümler izin vermek için de kullanılır.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms düzenleri")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms düzenleri")

Bu makalede ele alınmıştır:

- **[Amaç](#Purpose)**  &ndash; amaçla `ScrollView` ve bunu ne zaman kullanılır.
- **[Kullanım](#Usage)**  &ndash; nasıl kullanılacağını `ScrollView` uygulamada.
- **[Özellikleri](#Properties)**  &ndash; okuyup değiştiren ortak özellikleri.
- **[Yöntemleri](#Methods)**  &ndash; görünümü kaydırma için çağrılabilen genel yöntemleri.
- **[Olayları](#Events)**  &ndash; görünümün durumları değişiklikleri dinlemek için kullanılan olayları.

## <a name="purpose"></a>Amaç

`ScrollView` daha büyük görünümleri de küçük telefonlarda görünmesini sağlamak için kullanılabilir. Örneğin, bir iPhone 6s çalışır bir düzen İphone'da 4s kırpılarak. Kullanarak bir `ScrollView` daha küçük ekranda görüntülenecek düzenini kırpılmış bölümlerini çalıştırmasına olanak tanır.

## <a name="usage"></a>Kullanım

> [!NOTE]
> `ScrollView`s yuvalanmış değil. Ayrıca, `ScrollView`s iç içe Geçirilemeyen sağlayan kaydırma gibi diğer denetimlerle `ListView` ve `WebView`.

`ScrollView` kullanıma sunan bir `Content` bir tek bir görünüm veya düzeni ayarlanabilir özelliği. Ardından, çok büyük bir boxView düzeniyle bu örneğini düşünün bir `Entry`:

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

C# içinde:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,    HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Kullanıcı yalnızca aşağı kaydırma önce `BoxView` görülebilir:

![](scroll-view-images/scroll-start.png "ScrollView içinde BoxView")

Kullanıcının metin girmesini başladığında dikkat `Entry`, görünüm ekranında görünür tutmak için kaydırma:

![](scroll-view-images/scroll-end.png "ScrollView giriş")

## <a name="properties"></a>Özellikler

`ScrollView` Aşağıdaki özellikleri tanımlar:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) alır bir [ `Size` ](xref:Xamarin.Forms.Size) içeriğin boyutunu gösteren bir değer.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) alır veya ayarlar bir [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) kaydırma yönünü temsil eden bir numaralandırma değeri `ScrollView`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) alır bir `double` geçerli X kaydırma konumu temsil eden.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) alır bir `double` geçerli Y kaydırma konumunu temsil eden.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) alır veya ayarlar bir [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) yatay kaydırma çubuğunun görünür olduğunda gösteren bir değer.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) alır veya ayarlar bir [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) dikey kaydırma çubuğu görünür olduğunda gösteren bir değer.

## <a name="methods"></a>Yöntemler

`ScrollView` sağlar bir `ScrollToAsync` kaydırmak için kullanılan yöntem koordinatları kullanarak veya görünür yapılması gereken belirli bir görünüm belirterek.

Koordinatları kullanırken belirtin `x` ve `y` koordinatları, kaydırma animasyonu gerekmediğini gösteren bir Boole değeri birlikte:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

Belirli bir öğeye kaydırdığınızda `ScrollToPosition` numaralandırma Server'a burada görünüm öğesi görünür:

- **Merkezi** &ndash; görünümünün görünür merkezini öğesine kaydırır.
- **Son** &ndash; öğeye görünümünün görünür sonuna kadar ilerler.
- **MakeVisible** &ndash; öğesi görünümü içinde görünür olması kaydırır.
- **Başlangıç** &ndash; görünümünün görünür başlangıcını öğesine kaydırır.

`IsAnimated` Özelliği, görünüm kaydırılan nasıl belirtir. Ne zaman içerik görünümüne anında taşımak yerine true, düz bir animasyon olarak kullanılır.

## <a name="events"></a>Olaylar

`ScrollView` yalnızca bir olayı tanımlar `Scrolled`. `Scrolled` Görünüm, kaydırma tamamladıktan sonra ortaya çıkar. Olay işleyicisi `Scrolled` alır `ScrolledEventArgs`, sahip olduğu `ScrollX` ve `ScrollY` özellikleri. Aşağıdaki etiketi ile geçerli kaydırma konumunu güncelleştirme yapmayı gösteren bir `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Kaydırma konumları negatif bir listesinin sonunda kaydırdığınızda Sıçrama efekti nedeniyle olabileceğine dikkat edin.


## <a name="related-links"></a>İlgili bağlantılar

- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
