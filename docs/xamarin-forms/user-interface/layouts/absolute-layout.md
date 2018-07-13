---
title: Xamarin.Forms AbsoluteLayout
description: Bu makalede, kusursuz kalitede kullanıcı arabirimleri oluşturmak için Xamarin.Forms AbsoluteLayout sınıfı kullanmayı açıklar. Bu sınıf, yerleştirir ve alt öğeleri kendi boyutunu ve konumunu veya mutlak değerlere göre orantılı boyutları.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 0d49b8c50db08ad07952425492591ee246de4f8b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998354"
---
# <a name="xamarinforms-absolutelayout"></a>Xamarin.Forms AbsoluteLayout

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) yerleştirir ve alt öğeleri kendi boyutunu ve konumunu veya mutlak değerlere göre orantılı boyutları. Alt görünümleri konumlandırılmış ve boyutta orantılı değerleri ya da statik değerlerini kullanarak ve orantılı olabilir ve statik değerleri karma olabilir.

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms düzenleri")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms düzenleri")

Bu makalede ele alınacaktır:

- **[Amaç](#Purpose)**  &ndash; yaygın kullanımları için `AbsoluteLayout`.
- **[Kullanım](#Usage)**  &ndash; nasıl kullanılacağını `AbsoluteLayout` istenen tasarımınızı elde etmek için.
  - **[Orantılı düzenleri](#Proportional_Layouts)**  &ndash; nasıl orantılı değerleri anlamak dalda bir `AbsoluteLayout`.
  - **[Değerleri belirtme](#Specifying_Values)**  &ndash; orantılı ve mutlak değerlerini nasıl belirtilen anlayın.
  - **[Orantılı değerleri](#Proportional_Values)**  &ndash; nasıl orantılı değerleri anlamak çalışır.
    - **[Mutlak değerler](#Absolute_Values)**  &ndash; mutlak değerlerini nasıl çalıştığını anlayın.

<a name="Purpose" />

## <a name="purpose"></a>Amaç

Yerleştirme modelinin nedeniyle `AbsoluteLayout`, düzeni, böylece temizleme kenarlarından düzeni ile veya ortalanmış öğeleri konumlandırmak oldukça basit kolaylaştırır. Orantılı boyutları ve pozisyonları, öğeleri ile bir `AbsoluteLayout` herhangi bir görünüm boyuta otomatik olarak ölçeklendirebilirsiniz. Burada yalnızca konumu ancak boyutu ölçeklendirilmesi gereken öğeler için mutlak ve orantılı değerleri karma olabilir.

`AbsoluteLayout` öğeleri görünümü içinde konumlandırılan gerekir ve kenarlar öğelerine hizalarken özellikle yararlı olur. her yerde kullanılabilir.

<a name="Usage" />

## <a name="usage"></a>Kullanım

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Orantılı düzenleri

`AbsoluteLayout` orantılı konumlandırma kullanıldığında öğeyi düzene göre konumlandırılır gibi bağlantı öğesinin göreli alt öğe yapabildiği konumlandırılmış bir benzersiz bağlantı modeli vardır. Mutlak konumlandırma kullanıldığında (0,0) görünümü içinde bağlantıdır. Bu iki önemli sonuçları vardır:

- Öğeleri orantılı değerleri kullanarak ekranı yerleştirilmiş olamaz.
- Öğeleri kenarlarından düzeni veya düzeni veya cihazın boyutundan bağımsız olarak Merkezi'ndeki boyunca güvenilir bir şekilde konumlandırılmalıdır.

`AbsoluteLayout`, gibi `RelativeLayout`, öğeleri örtüşecek şekilde konumlandırın kuramıyor.

Aşağıdaki ekran görüntüsünde, bağlantı kutusunun Not beyaz bir noktadır. Düzeni arasında hareket ettikçe kutusunu ve bağlantı arasındaki ilişkiyi dikkat edin:

![](absolute-layout-images/anchor-start.png "Bağlayıcı başlangıçta")
![](absolute-layout-images/anchor-center.png "merkezinde yer işareti")
![](absolute-layout-images/anchor-end.png "sonunda yer işareti")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Değerleri belirtme

Görünümler içinde bir `AbsoluteLayout` dört değerleri kullanarak konumlandırılır:

- **X** &ndash; görünümün bağlantı x (yatay) konumu
- **Y** &ndash; görünümün bağlantı y (dikey) konumu
- **Genişlik** &ndash; görünümünün genişliği
- **Yükseklik** &ndash; görünümü yüksekliği

Bu değerlerin her birinin olarak ayarlanabilir bir [orantılı](#Proportional_Values) değer veya bir [mutlak](#Absolute_Values) değeri.

Değerler sınırları ve bayrak birleşimini belirtilir. `LayoutBounds` olan bir [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) dört değerden oluşan: `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) değerleri nasıl yorumlanacaktır belirtir ve aşağıdaki önceden tanımlanmış seçenekler vardır:

- **Hiçbiri** &ndash; tüm değerleri mutlak yorumlar. Düzen bayrak belirtilmezse varsayılan değer budur.
- **Tüm** &ndash; tüm değerleri orantılı olarak yorumlar.
- **WidthProportional** &ndash; yorumlar `Width` mutlak olarak orantılı ve diğer tüm değerler olarak değeri.
- **HeightProportional** &ndash; yalnızca yüksekliğini değeriyle orantılı olarak diğer tüm değerler mutlak yorumlar.
- **XProportional** &ndash; yorumlar `X` değer orantılı, diğer tüm değerler mutlak davranılırken.
- **YProportional** &ndash; yorumlar `Y` değer orantılı, diğer tüm değerler mutlak davranılırken.
- **PositionProportional** &ndash; yorumlar `X` ve `Y` değerleri orantılı olarak, ancak boyut değerleri mutlak yorumlanır.
- **SizeProportional** &ndash; yorumlar `Width` ve `Height` değerleri orantılı olarak konum değerleri mutlak olmasına rağmen.

XAML içinde düzen görünümleri tanımının bir parçası olarak ayarlanır sınırları ve bayrakları kullanılarak `AbsoluteLayout.LayoutBounds` özelliği. Sınırları, değerleri virgülle ayrılmış listesi olarak ayarlanmış `X`, `Y`, `Width`, ve `Height`bu sırası. Bayrakları düzenini kullanarak görünüm bildiriminde belirtilen ayrıca `AbsoluteLayout.LayoutFlags` özelliği. Bayrakları virgülle ayrılmış bir liste kullanarak XAML içinde birleştirilebilir unutmayın. Aşağıdaki örnek göz önünde bulundurun:

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

- Etiketin ortasında boyutunu ve konumunu mutlak değerlerini kullanarak konumlandırıldı. Bu nedenle bu iPhone 4S üzerinde ortalanmış ve daha düşük ancak daha büyük cihazlarda ortalanmış görünür.
- Metin düzenini alt kısmındaki orantılı boyut ve konum değerleri kullanarak konumlandırıldı. Düzen alt merkezinde her zaman görünür, ancak boyutu daha büyük bir düzen boyutlarıyla büyüyecektir.
- Üç renkli `BoxView`s orantılı konumunu ve mutlak boyutunu kullanarak ekranın üst, sol ve sağ kenarları konumlandırılmıştır.

C# ' de aynı düzeni sağlar:

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

Orantılı değerleri bir düzen ve görünüm arasında bir ilişki tanımlayacağız. Bu ilişki, bir alt görünümün konumu veya ölçek değeri üst düzen karşılık gelen değeri tabanının oranı tanımlar. Bu değerler olarak ifade edilir `double`0 ile 1 arasındaki değerleri ile s.

Konum ve boyut görünümlere Düzen dahilinde orantılı değerler kullanılır. Bu nedenle, bir görünümün genişlik bütünün ayarlandığında, sonuç genişlik değeri çarpılmasıyla oranı olan `AbsoluteLayout`'s genişliği. Örneğin, bir `AbsoluteLayout` genişliğinin `500` ve 250 (500 x.5)..5 işlenmiş görünümü genişliğini orantılı genişliği olacak şekilde ayarlanmış bir görünüm olacaktır

Orantılı değerleri kullanmak için ayarlanmış `LayoutBounds` (x, y) kullanarak oranları ve orantılı boyutları ayarlayın `LayoutFlags` için `All`.

XAML içinde:

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

C# içinde:

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

<a name="Absolute_Values" />

### <a name="absolute-values"></a>Mutlak değerler

Görünümler içinde düzenini nerede konumlandırılacağını mutlak değerler açıkça tanımlayın. Orantılı, mutlak değerler konumlandırma ve boyutlandırma düzenini sınırları içinde uymayan bir görünüm özellikli değerlerdir.

Düzenini bilinmiyor konumlandırma için mutlak değerlerini kullanarak tehlikeli olabilir. Mutlak Konumlar kullanırken, diğer herhangi bir boyutta bir boyutta ekranın ortasında bir öğedeki uzaklığı. Desteklenen cihazlarınızın çeşitli ekran boyutları arasında uygulamanızı test etmek önemlidir.

Mutlak Düzen değerleri kullanmak için ayarlanmış `LayoutBounds` (x, y) kullanarak koordinatları ve açık boyutların ayarlayın `LayoutFlags` için `None`.

XAML içinde:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

C# içinde:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Karmaşık Düzen keşfetme

Her düzene sahip güçlü ve zayıf özel düzenler oluşturmak için. Bu makale serisi, düzen, aynı sayfa düzeni üç farklı düzenleri kullanılarak uygulanan bir örnek uygulaması oluşturuldu.

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

Yukarıdaki kod, aşağıdaki düzende sonuçlanır:

![](absolute-layout-images/abs.png "Karmaşık AbsoluteLayout")

Dikkat `AbsoluteLayout`s iç içe, bazı durumlarda düzenleri iç içe tüm öğeler aynı düzeni içinde sunma daha kolay olabilir çünkü.

## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 14 Xamarin.Forms ile mobil uygulamalar oluşturma](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
