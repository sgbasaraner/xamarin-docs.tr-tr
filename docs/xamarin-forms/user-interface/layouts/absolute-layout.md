---
title: AbsoluteLayout
description: "AbsoluteLayout piksel mükemmel Uı'lar oluşturmak için kullanın."
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 90f1394103b5a3996029355a8ef260817b14a697
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="absolutelayout"></a>AbsoluteLayout

[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) yerleştirir ve alt öğelerini kendi boyutunu ve konumunu veya mutlak değerler orantılı boyutları. Alt görünümleri konumlandırılmış ve boyutlu orantılı değerleri veya statik değerleri kullanarak ve orantılı olabilir ve statik değerler karışabilir.

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms düzenleri")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms düzenleri")

Bu makalede ele alınacaktır:

- **[Amaç](#Purpose)**  &ndash; yaygın kullanımları `AbsoluteLayout`.
- **[Kullanım](#Usage)**  &ndash; nasıl kullanılacağını `AbsoluteLayout` istenen tasarımınızı elde etmek için.
  - **[Orantılı düzenleri](#Proportional_Layouts)**  &ndash; nasıl orantılı değerleri anlamak, iş bir `AbsoluteLayout`.
  - **[Değerleri belirtme](#Specifying_Values)**  &ndash; orantılı ve mutlak değerler nasıl belirtilen anlayın.
  - **[Orantılı değerleri](#Proportional_Values)**  &ndash; nasıl orantılı değerleri anlamak çalışır.
    - **[Mutlak değerler](#Absolute_Values)**  &ndash; mutlak değerler nasıl çalıştığını anlayın.

<a name="Purpose" />

## <a name="purpose"></a>Amaç

Konumlandırma modelini nedeniyle `AbsoluteLayout`, düzeni kenarlarından düzeninin ile temizleme ya da ortalanmış; böylece öğeleri konumlandırmak görece basit kolaylaştırır. Orantılı boyutları ve konumları, öğelerin bir `AbsoluteLayout` herhangi bir görünüm boyuta otomatik olarak ölçeklendirebilirsiniz. Burada yalnızca konumu ancak boyutu ölçeklendirmeniz gerekir öğeleri için mutlak ve orantılı değerleri karışabilir.

`AbsoluteLayout` öğeleri bir görünüm içinde konumlandırılması gerekiyorsa ve öğeleri kenarlarına hizalama özellikle yararlıdır her yerden kullanılabilir.

<a name="Usage" />

## <a name="usage"></a>Kullanım

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Orantılı düzenleri

`AbsoluteLayout` orantılı konumlandırma kullanıldığında öğesi düzeni göre yerleştirilmiş olarak öğesi bağlantı göre kendi öğesinin yapabildiği konumlandırılır benzersiz bağlantı modeli vardır. Mutlak konumlandırma kullanıldığında (0,0) görünümde bağlantıdır. Bu iki önemli sonuçları vardır:

- Öğeler ekranı orantılı değerleri kullanarak konumlandırılmış olamaz.
- Öğeleri kenarlarından düzeninin veya istediğiniz düzeni veya aygıt boyutunu bakılmaksızın Merkezi'ndeki boyunca güvenilir bir şekilde konumlandırılmış olabilir.

`AbsoluteLayout`, gibi `RelativeLayout`, böylece birbiriyle öğeleri konumlandırmak yapabiliyor.

Aşağıdaki ekran görüntüsü, bağlayıcı kutusunun Not beyaz bir noktadır. Düzen hareket ederken bağlantı ve kutunun arasındaki ilişkiyi dikkat edin:

![](absolute-layout-images/anchor-start.png "Başlangıç bağlantı")
![](absolute-layout-images/anchor-center.png "merkezinde yer işareti")
![](absolute-layout-images/anchor-end.png "sonunda yer işareti")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Değerleri belirtme

Görünümler içinde bir `AbsoluteLayout` dört değerleri kullanarak yerleştirilir:

- **X** &ndash; görünümün yer işaretine x (yatay) konumu
- **Y** &ndash; görünümün yer işaretine y (dikey) konumu
- **Genişlik** &ndash; görünümünün genişliğini
- **Yükseklik** &ndash; görünümün yüksekliğini

Bu değerlerin her birini olarak ayarlanabilir bir [orantılı](#Proportional_Values) değer veya bir [mutlak](#Absolute_Values) değeri.

Değerler birleşimi sınırları ve bir bayrak belirtilir. `LayoutBounds` olan bir [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) dört değerden oluşan: `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/) değerlerin nasıl yorumlanacak belirtir ve aşağıdaki önceden tanımlanmış seçenekler vardır:

- **Hiçbiri** &ndash; tüm değerleri mutlak yorumlar. Düzen bayrak belirtilmezse, varsayılan değer budur.
- **Tüm** &ndash; tüm değerleri orantılı olarak yorumlar.
- **WidthProportional** &ndash; yorumlar `Width` Oransal ve diğer tüm değerler olarak değeri mutlak olarak.
- **HeightProportional** &ndash; yalnızca yükseklik değeriyle orantılı olarak diğer tüm değerler mutlak yorumlar.
- **XProportional** &ndash; yorumlar `X` değer orantılı, diğer tüm değerler mutlak değerlendirmesini oluştu.
- **YProportional** &ndash; yorumlar `Y` değer orantılı, diğer tüm değerler mutlak değerlendirmesini oluştu.
- **PositionProportional** &ndash; yorumlar `X` ve `Y` değerleri orantılı olarak sırada boyutu değerleri mutlak yorumlanır.
- **SizeProportional** &ndash; yorumlar `Width` ve `Height` değerleri orantılı olarak konum değerleri mutlak durumdayken.

XAML'de, görünümler düzeni içinde tanımının bir parçası olarak ayarlanır sınırları ve bayrakları kullanarak `AbsoluteLayout.LayoutBounds` özelliği. Sınır değerleri, virgülle ayrılmış listesi olarak ayarlanmış `X`, `Y`, `Width`, ve `Height`bu sırası. Bayrakları düzenini kullanarak görünümlerinin bildiriminde belirtilen de `AbsoluteLayout.LayoutFlags` özelliği. Virgülle ayrılmış bir liste kullanarak XAML'de bayrakları birleştirilebilir unutmayın. Aşağıdaki örnek göz önünde bulundurun:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![](absolute-layout-images/exploration.png "AbsoluteLayout örnekleri")

Şunlara dikkat edin:

- Etiketin ortasında mutlak boyutunu ve konumunu değerleri kullanarak konumlandırıldı. Bu yüzden, iPhone 4S üzerinde ortalanmış ve alt ancak büyük cihazlarda ortalanmış görünür.
- Metin düzenini altındaki orantılı boyutunu ve konumunu değerleri kullanarak konumlandırıldı. Düzen alt merkezinde her zaman görünür ancak boyutuna büyük düzeni boyutlarıyla büyüyecektir.
- Üç renkli `BoxView`s orantılı konumu ve mutlak boyutu kullanarak ekranın üst, sol ve sağ kenarlarında konumlandırıldı.

C# aynı düzeni uygular:

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```
<a name="Proportional_Values" />

### <a name="proportional-values"></a>Orantılı değerleri

Bir düzen ve bir görünüm arasında bir ilişki orantılı değerleri tanımlayın. Bu ilişki bir alt görünümün konumu veya ölçek değeri üst düzeni karşılık gelen değeri bir kısmının tanımlar. Bu değerler olarak ifade edilir `double`değerleri 0 ile 1 arasında s.

Orantılı değerler, konum ve boyut görünümler düzeni içinde için kullanılır. Bir görünümün genişlik oranı ayarlandığında, bu nedenle, sonuç genişliği ile çarpılır oranı değerdir `AbsoluteLayout`'s genişliği. Örneğin, bir `AbsoluteLayout` genişliğinin `500` ve.5 görünümünün işlenmiş genişliğini orantılı genişliğini olacak şekilde ayarlanmış bir görünüm 250 (500 x.5). olacaktır

Orantılı değerleri kullanmak üzere ayarlanmış `LayoutBounds` (x, y) kullanarak oranları ve orantılı boyutları, daha sonra ayarlamak `LayoutFlags` için `All`.

XAML'de:

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

C# ' de:

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

<a name="Absolute_Values" />

### <a name="absolute-values"></a>Mutlak değerler

Mutlak değerler açıkça görünümleri içinde yerleşim yeri konumlandırılmalıdır tanımlayın. Orantılı değerleri mutlak değerler konumlandırma ve düzeni sınırları içinde sığmayan bir görünüm boyutlandırma özelliğine sahiptir.

Düzen boyutunu bilinmiyor konumlandırma için mutlak değerler kullanarak tehlikeli olabilir. Mutlak Konumlar kullanırken, bir boyut ekranında merkezi bir öğe başka bir boyutta uzaklık. Desteklenen aygıtlar çeşitli ekran boyutlarına arasında uygulamanızı test etmek önemlidir.

Mutlak düzeni değerleri kullanmak üzere ayarlanmış `LayoutBounds` (x, y) kullanarak koordinatları ve açık boyutları, daha sonra ayarlamak `LayoutFlags` için `None`.

XAML'de:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

C# ' de:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Karmaşık bir düzen keşfetme

Her düzenleri güçlü ve belirli düzenler oluşturmak için zayıf sahip. Düzen makaleleri bu dizi aynı sayfa düzeni üç farklı düzenleri kullanılarak uygulanan bir örnek uygulaması oluşturuldu.

Aşağıdaki XAML göz önünde bulundurun:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

Yukarıdaki kod aşağıdaki düzende sonuçları:

![](absolute-layout-images/abs.png "Karmaşık AbsoluteLayout")

Düğmeleri Windows Phone tarafından nasıl işlendiğini içinde bir fark nedeniyle dikkat edin, daireleri bazıları, Windows Phone ekran görüntüsü boxviews tarafından değiştirilmiştir.

Dikkat `AbsoluteLayout`s iç içe geçmiş, bazı durumlarda düzenleri iç içe geçme aynı düzen içindeki tüm öğeler sunma daha kolay olabilir çünkü.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 14 Xamarin.Forms ile mobil uygulamaları oluşturma](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)
- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
