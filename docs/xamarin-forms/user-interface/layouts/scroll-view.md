---
title: ScrollView
description: Yalnızca bir ekrana sığar ve içerik yer açmak için klavye düzenleri sunmak için ScrollView kullanın.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2016
ms.openlocfilehash: 708fb39aa2e56861a8c9fc47ab30bd20ed20188e
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847543"
---
# <a name="scrollview"></a>ScrollView

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) düzenleri içerir ve kaydırma ekran dışında sağlar. `ScrollView` klavye gösterilirken ekranda görünen dilimini otomatik olarak taşımak görünümleri izin vermek için de kullanılır.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms düzenleri")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms düzenleri")

Bu makalede yer almaktadır:

- **[Amaç](#Purpose)**  &ndash; amaçla `ScrollView` ve ne zaman kullanılır.
- **[Kullanım](#Usage)**  &ndash; nasıl kullanılacağını `ScrollView` uygulamada.
- **[Özellikler](#Properties)**  &ndash; okuyup değiştiren ortak özellikler.
- **[Yöntemleri](#Methods)**  &ndash; görünümü kaydırma için çağrılan genel yöntemler.
- **[Olayları](#Events)**  &ndash; görünümün durumları değişiklikleri dinlemek için kullanılan olayları.

## <a name="purpose"></a>Amaç

`ScrollView` daha büyük görünümleri de küçük telefonlarda görünmesini sağlamak için kullanılabilir. Örneğin, bir iPhone 6s üzerinde çalıştığı bir düzen iPhone 4s kırpılmış. Kullanarak bir `ScrollView` küçük ekranda görüntülenecek düzeni kırpılmış bölümlerini olanak tanır.

## <a name="usage"></a>Kullanım

> [!NOTE]
> `ScrollView`s iç içe değil. Ayrıca, `ScrollView`s değil iç içe geçirilemez sağlayan kaydırma gibi başka denetimlerle birlikte `ListView` ve `WebView`.

`ScrollView` kullanıma sunan bir `Content` bir tek bir görünüm veya düzeni ayarlayabileceğiniz özelliği. Bu örnek tarafından izlenen bir çok büyük boxView bir düzen olarak göz önünde bulundurun bir `Entry`:

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

C# ' de:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,    HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Kullanıcı yalnızca aşağı kaydırdığında önce `BoxView` görünür:

![](scroll-view-images/scroll-start.png "ScrollView BoxView")

Kullanıcı metni girmek başladığında dikkat `Entry`, ekranda görünsün için görünümü kayar:

![](scroll-view-images/scroll-end.png "ScrollView giriş")

## <a name="properties"></a>Özellikler

ScrollView aşağıdaki özelliklere sahiptir:

- **İçerik** &ndash; alır veya ayarlar görüntülemek için görünümü `ScrollView`.
- **[ContentSize](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)**  &ndash; salt okunur bir genişlik ve yükseklik bileşen içeriğin boyutunu alır. Bu bağlanabilir bir özelliktir
- **[Yönlendirme](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)**  &ndash; bu bir [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/), ayarlanabilir bir numaralandırma olduğu `Horizontal`, `Vertical`, veya `Both`.
- **ScrollX** &ndash; salt okunur X boyutundaki geçerli kaydırma konumunu alır.
- **ScrollY** &ndash; salt okunur Y boyutundaki geçerli kaydırma konumunu alır.

## <a name="methods"></a>Yöntemler

`ScrollView` sağlayan bir `ScrollToAsync` görünümü kaydırmak için kullanılan yöntem koordinatları kullanarak ya da görünür yapılması gereken belirli bir görünüm belirterek.

Koordinatları kullanırken belirtin `x` ve `y` kaydırma animasyon gerekmediğini gösteren bir Boole değeri birlikte koordinatları:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

Belirli bir öğeye kaydırma sırasında `ScrollToPosition` numaralandırma Server'a burada görünümünde öğesi görünür:

- **Merkezi** &ndash; görünümünün görünür merkezi öğesine kaydırır.
- **Son** &ndash; görünümünün görünür sonuna öğesine kaydırır.
- **MakeVisible** &ndash; böylece görünümde görünür öğe birlikte kayar.
- **Başlat** &ndash; görünümünün görünür başlangıcı öğesine kaydırır.

`IsAnimated` Özelliği, görünüm kaydırılan nasıl belirtir. Ne zaman içerik görünümüne anında taşıma yerine true, kesintisiz bir animasyon olarak ayarlandığında kullanılır.

## <a name="events"></a>Olaylar

`ScrollView` yalnızca bir olayı sunar `Scrolled`. `Scrolled` Görünüm kaydırma tamamladığında tetiklenir. Olay işleyicisi `Scrolled` geçen `ScrolledEventArgs`, sahip olduğu `ScrollX` ve `ScrollY` özellikleri. Nasıl bir etiket ile geçerli kaydırma konumunu güncelleştirmek aşağıdaki gösteren bir `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Kaydırma konumlar negatif bir listesinin sonuna kaydırma sırasında Sıçrama etkisi nedeniyle olabileceğine dikkat edin.


## <a name="related-links"></a>İlgili bağlantılar

- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
