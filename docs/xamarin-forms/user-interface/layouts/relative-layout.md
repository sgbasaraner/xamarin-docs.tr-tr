---
title: RelativeLayout
description: Tüm ekran boyutuna göre ölçeklendirme Uı'lar oluşturmak için RelativeLayout kullanın.
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 7c12319d9e688504c87368be37edf93e7294df30
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="relativelayout"></a>RelativeLayout

`RelativeLayout` konum ve boyut görünümleri düzeni veya eşdüzey görünümleri özelliklerini göre için kullanılır. Farklı `AbsoluteLayout`, `RelativeLayout` taşıma bağlantı kavramı yok ve Alttan veya sağ kenarlarının düzeni göre öğeleri konumlandırma için olanaklara sahip değil. `RelativeLayout` kendi sınırların dışında konumlandırma öğelerini desteklemiyor.

[![](relative-layout-images/layouts-sml.png "Xamarin.Forms düzenleri")](relative-layout-images/layouts.png#lightbox "Xamarin.Forms düzenleri")

## <a name="purpose"></a>Amaç

`RelativeLayout` görünümleri ekranında genel düzenini göreli veya diğer görünümlere konumlandırmak için kullanılabilir.

![](relative-layout-images/flag.png "RelativeLayout araştırması")

## <a name="usage"></a>Kullanım

### <a name="understanding-constraints"></a>Anlama kısıtlamaları

Konumlandırma ve bir görünüm içinde boyutlandırma bir `RelativeLayout` kısıtlamaları ile yapılır. Kısıtlama ifadesinde aşağıdaki bilgileri içerebilir:

- **Tür** &ndash; kısıtlaması üst göreli veya başka bir görünüme olup.
- **Özellik** &ndash; kısıtlaması için temel olarak kullanılacak hangi özelliği.
- **Faktörü** &ndash; özellik değerine uygulanacak faktör.
- **Sabit** &ndash; uzaklık değeri kullanılacak değer.
- **ElementName** &ndash; göreli sınırlamadır görünümün adı.

XAML'de kısıtlamaları olarak ifade edilir `ConstraintExpression`s. Aşağıdaki örnek göz önünde bulundurun:

```xaml
<BoxView Color="Green" WidthRequest="50" HeightRequest="50"
    RelativeLayout.XConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Width,
                             Factor=0.5,
                             Constant=-100}"
    RelativeLayout.YConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Height,
                             Factor=0.5,
                             Constant=-100}" />
```

C# ' ta kısıtlamaları biraz farklı işlevler ifadeleri görünümünde yerine ifade edilir. Kısıtlamaları düzeni'nın bağımsız değişkenleri olarak belirtilen `Add` yöntemi:

```csharp
layout.Children.Add(box, Constraint.RelativeToParent((parent) =>
    {
      return (.5 * parent.Width) - 100;
    }),
    Constraint.RelativeToParent((parent) =>
    {
        return (.5 * parent.Height) - 100;
    }),
    Constraint.Constant(50), Constraint.Constant(50));
```

Yukarıdaki düzeni şu yönlerini dikkat edin:

- `x` Ve `y` kısıtlamaları kendi kısıtlamaları ile belirtilir.
- C# ' ta göreli kısıtlamaları işlevler tanımlanır. Kavramları ister `Factor` olmadığı, ancak el ile uygulanabilir.
- Kutunun `x` koordinat -100 üst yarı genişlikte tanımlanır.
- Kutunun `y` koordinat -100 üst yarım yükseklik tanımlanır.

> [!NOTE]
> Kısıtlamaları tanımlanan şekilde nedeniyle, C# ' ta XAML ile belirtilen olandan daha karmaşık düzenleri olun mümkündür.

Yukarıdaki örneklerde her ikisi de olarak kısıtlamalarını tanımlayan `RelativeToParent` &ndash; diğer bir deyişle, kendi üst öğesi göre değerlerdir. Başka bir görünüm olarak göreli kısıtlamaları tanımlamak mümkündür. Bu için (için Geliştirici) daha sezgisel düzenleri verir ve düzeni kodunuzu amacı kolayca daha belirgin hale getirebilir.

Burada bir öğe 20 piksel diğerinden daha düşük olması gerekiyor bir düzen göz önünde bulundurun. Her iki öğeleri ile sabit değerleri tanımlanmışsa, daha düşük olabilir, `Y` 20 piksel daha büyük bir sabit olarak tanımlanan kısıtlaması `Y` daha yüksek öğesinin kısıtlaması. Bu yaklaşımı piksel boyutu bilinen değil böylece daha yüksek öğesi bir oran kullanarak konumlandırılır, kısa döner. Bu durumda, başka bir öğenin konumuna bağlı öğesi sınırlama daha sağlamdır:

```xaml
<RelativeLayout>
    <BoxView Color="Red" x:Name="redBox"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent,
            Property=Height,Factor=.15,Constant=0}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=1,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.8,Constant=0}" />
    <BoxView Color="Blue"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=Y,Factor=1,Constant=20}"
        RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=X,Factor=1,Constant=20}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=.5,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.5,Constant=0}" />
</RelativeLayout>
```

C# aynı düzeni gerçekleştirmek için:

```csharp
layout.Children.Add (redBox, Constraint.RelativeToParent ((parent) => {
        return parent.X;
    }), Constraint.RelativeToParent ((parent) => {
        return parent.Y * .15;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .8;
    }));
layout.Children.Add (blueBox, Constraint.RelativeToView (redBox, (Parent, sibling) => {
        return sibling.X + 20;
    }), Constraint.RelativeToView (blueBox, (parent, sibling) => {
        return sibling.Y + 20;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width * .5;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .5;
    }));
```

Bu çıktı, belirlenen mavi kutunun konumu ile aşağıdaki oluşturur _göreli_ kırmızı kutu konumuna:

![](relative-layout-images/red-blue-box.png "Kırmızı ve mavi BoxViews ile RelativeLayout")

### <a name="sizing"></a>Boyutlandırma

Görünümleri düzenlendiği `RelativeLayout` boyutlarına belirtmek için iki seçeneğiniz vardır:

- `HeightRequest & WidthRequest`
- `RelativeLayout.WidthConstraint` & `RelativeLayout.HeightConstraint`

`HeightRequest` ve `WidthRequest` hedeflenen yüksekliğini ve genişliğini görünümün belirtin, ancak tarafından düzenleri gerektiği gibi geçersiz kılınabilir. `WidthConstraint` ve `HeightConstraint` genişliği ve yüksekliği düzeni 's veya başka bir görünümün özelliklerini göre bir değer veya sabit bir değer olarak ayarını destekler.

## <a name="exploring-a-complex-layout"></a>Karmaşık bir düzen keşfetme
Her düzenleri güçlü ve belirli düzenler oluşturmak için zayıf sahip. Düzen makaleleri bu dizi aynı sayfa düzeni üç farklı düzenleri kullanılarak uygulanan bir örnek uygulaması oluşturuldu.

Aşağıdaki XAML göz önünde bulundurun:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.RelativeLayoutPage"
BackgroundColor="Maroon"
Title="RelativeLayout">
    <ContentPage.Content>
    <ScrollView>
      <RelativeLayout>
        <BoxView Color="Gray" HeightRequest="100"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Button BorderRadius="35" x:Name="imageCircleBack"
            BackgroundColor="Maroon" HeightRequest="70" WidthRequest="70" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.5, Constant = -35}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=70}" />
        <Button BorderRadius="30" BackgroundColor="Red" HeightRequest="60"
            WidthRequest="60" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=imageCircleBack, Property=X, Factor=1,Constant=5}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=75}" />
        <Label Text="User Name" FontAttributes="Bold" FontSize="26"
            HorizontalTextAlignment="Center" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=140}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Entry Text="Bio + Hashtags" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=180}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <RelativeLayout BackgroundColor="White" RelativeLayout.YConstraint="
            {ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=220}" HeightRequest="60" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" >
            <BoxView BackgroundColor="Black" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=5}" />
            <BoxView BackgroundColor="Maroon" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=}" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=60}" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=55}" />
        </RelativeLayout>
        <RelativeLayout Padding="5,0,0,0">
          <Label FontSize="14" Text="Age:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=305}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=280}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Interests:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=345}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=320}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
            LineBreakMode="WordWrap"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=395}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
            BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=370}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
      </RelativeLayout>
    </ScrollView>
  </ContentPage.Content>
</ContentPage>
```

Yukarıdaki kod aşağıdaki düzende sonuçları:

![](relative-layout-images/relative.png "Karmaşık RelativeLayout")

Dikkat `RelativeLayouts`s iç içe geçmiş, bazı durumlarda düzenleri iç içe geçme aynı düzen içindeki tüm öğeler sunma daha kolay olabilir çünkü. Ayrıca bazı öğeler bildirimi `RelativeToView`, görünümler arasındaki ilişki konumlandırma yol olduğunda, daha kolay ve daha sezgisel düzeni için izin verir.


## <a name="related-links"></a>İlgili bağlantılar

- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
