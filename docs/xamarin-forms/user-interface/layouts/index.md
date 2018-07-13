---
title: Xamarin.Forms düzenleri
description: Xamarin.Forms birkaç düzenleri ve ekran içeriğini düzenlemek için özellikler vardır ve bunları bu makalede açıklanır.
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: cff5f9c15f4608ecfb643d2c49dd636df8b18b5c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995761"
---
# <a name="layouts-in-xamarinforms"></a>Xamarin.Forms düzenleri

Xamarin.Forms birkaç düzenleri ve ekran içeriğini düzenlemek için özellikler vardır.

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Xamarin.Forms düzenleri kullanarak, [Xamarin University](https://university.xamarin.com/)**

Her düzen denetimi, ekran yönünü değişiklikleri işleme konusunda ayrıntılar yanı sıra aşağıda açıklanmıştır:

* **[StackLayout](stack-layout.md)**  &ndash; görünümleri doğrusal olarak, düzenlemek için kullanılan yatay veya dikey. Bir StackLayout görünümlerde merkezine sola veya sağa düzeni hizalanabilir.
* **[AbsoluteLayout](absolute-layout.md)**  &ndash; koordinatları ayarlayarak görünümleri Tablola & Mutlak değerler veya oran hesaplıyorsanız açısından boyutu için kullanılır. AbsoluteLayout yanı sıra görünümleri katman sol, sağ veya center bağlantı için kullanılabilir.
* **[RelativeLayout](relative-layout.md)**  &ndash; kısıtlamaları kendi üst öğenin boyut ve konum göre ayarlayarak görünümleri düzenlemek için kullanılır.
* **[Kılavuz](grid.md)**  &ndash; kılavuz görünümlerde düzenlemek için kullanılır. Satırları ve sütunları mutlak değerler veya oran hesaplıyorsanız açısından belirtilebilir.
* **[FlexLayout](flex-layout.md)**  &ndash; görünümleri kaydırma ile yatay veya dikey olarak düzenlemek için kullanılır.
* **[ScrollView](scroll-view.md)**  &ndash; görünüm ekran sınırları içinde tamamen sığamıyorsa zaman kaydırmayı sağlamak için kullanılır.
* **[LayoutOptions](layout-options.md)**  &ndash; hizalama ve üst öğesiyle ilişkili bir görünüm için genişletme tanımlayın.
* **[Giriş saydamlık](#input_transparency)**  &ndash; bir öğe giriş alıp almadığını belirtir.
* **[Kenar boşlukları ve doldurma](margin-and-padding.md)**  &ndash; öğenin kullanıcı arabiriminde işlendiğinde düzen davranışını denetlemek nasıl gösterir.
* **[Cihaz yönü](device-orientation.md)**  &ndash; cihaz yönü değişiklikleri nasıl ele alınacağını açıklar.
* **[Tablet ve Masaüstü cihazlarında Düzen](tablet.md)**  &ndash; her platformda büyük ekranlar için en iyi duruma getirme işlemi gösterilmektedir.
* **[Özel bir düzen oluşturma](custom.md)**  &ndash; özel düzen sınıfı oluşturma açıklanır.
* **[Düzen sıkıştırma](layout-compression.md)**  &ndash; kaldırır belirtilen görsel ağacı düzeninden sayfa işleme performansını girişimi.

Platform denetimleri de kullanılabilir doğrudan Xamarin.Forms düzenleriyle [ **yerel ekleme** ](~/xamarin-forms/platform/native-views/index.md) (Xamarin.Forms 2.2 içinde yeni), ve [ **özel düzenleroluşturma** ](custom.md) belirli gereksinimleri karşılayacak şekilde.

Aşağıdaki grafikte, düzen denetimleri görselleştirir:

[![](images/layouts-sml.png "Xamarin.Forms düzenleri")](images/layouts.png#lightbox "Xamarin.Forms düzenleri")

## <a name="choosing-the-right-layout"></a>Sağ Düzen seçme

Uygulamanızı seçtiğiniz düzenleri Yardım veya olarak çekici ve kullanışlı bir Xamarin.Forms uygulaması oluşturuyorsanız, zor. Her Düzen works daha temiz ve daha ölçeklenebilir UI kod yazmanıza nasıl yardımcı olabileceğini göz önüne almanız gereken bazı saat sürüyor. Bir ekran, belirli bir tasarım elde etmek için farklı bir düzende bir birleşimi olabilir.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout` Yatay veya dikey çizgi görünümleri görüntülemek için kullanılır. Konum ve boyut Düzen dahilinde belirlenir bir görünümündeki tabanlı `HeightRequest`, `WidthRequest`, `HorizontalOptions` ve `VerticalOptions`. `StackLayout` düzenleme ekranında diğer düzenleri temel düzeni olarak sıklıkla kullanılır.

Ne zaman örneği `StackLayout` iyi bir seçenek, bir düğme ve etiketi sola hizalanmış ve sağa hizalı düğmesini içeren bir etiket görüntülemesi gereken bir uygulama düşünün.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="flexlayoutflex-layoutmd"></a>[FlexLayout](flex-layout.md)

`FlexLayout` Benzer `StackLayout` , yatay veya dikey olarak alt görünümleri görüntüler:

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">

    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

Ancak, bir tek bir satır veya columm, sığdırmak için çok fazla alt öğe varsa `FlexLayout` Ayrıca bu görünümleri sarmalama özelliğine sahiptir. `FlexLayout` CSS esnek kutusu düzeni modülü, temel alır ve birçok konumlandırma ve alt öğeleri hizalama için yerleşik seçeneklere sahiptir.

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout` Görünümler, boyut ve konum ya da belirtilen açık değerler ya da düzenini göreli değerlerini görüntülemek için kullanılır. Farklı `StackLayout` ve `Grid`, `AbsoluteLayout` alt çakıştırmayı görünümler sağlar. Farklı `RelativeLayout`, `AbsoluteLayout` öğeleri ekranın yerleştirme izin vermez.

Ne zaman örneği `AbsoluteLayout` iyi bir seçenek, yığın olarak nesne koleksiyonları sunmak için gereken bir uygulama düşünün. Bu genellikle şarkıya ve fotoğraf albümleri sunarken görülür. Aşağıdaki kod, yığın içeriğini ipucu döndürülen öğeleri ile bir küme görünümü verir:

XAML içinde:

```xaml
<AbsoluteLayout Padding="15">
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="30"
    Source="bottom.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="60"
    Source="middle.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
    Source="cover.png" />
</AbsoluteLayout>
```

Yukarıdaki kodu şu yönlerini dikkat edin:

- Her `Image` (ortasında yatay boşluk) aynı konumda görüntülenir
- `Padding` Tarafından dikkate `AbsoluteLayout`aksine `RelativeLayout`, hangi yoksayar.
- `AbsoluteLayout.LayoutFlags` Düzen sınırları nasıl yorumlanacaktır belirtir. Bu durumda `PositionProportional`, boyutu açık bir boyut yorumlanacaktır sırada koordinatları düzenini oranını anlamına gelir.
- `AbsoluteLayout.Layoutbounds` Bu sırada, yatay konum, dikey konumunu, genişliğini ve yüksekliğini belirtir.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout` Görünümler, boyut ve düzenini veya başka bir görünüme değerlerini göre değerler olarak belirtilen konumu ile görüntülemek için kullanılır. Göreli değerler eşleşecek şekilde kendisine karşılık gelen değer ilgili görünüm gerekmez. Örneğin, bir görünümün ayarlamak olası `Width` özelliğini başka bir görünüme ait orantılı `X` özelliği.

RelativeLayout cihaz boyutuna ölçekleyeceğinizi kullanıcı arabirimleri oluşturmak için kullanılabilir. Aşağıdaki XAML bir tasarımla kutuları üst köşelerde bir flagpole merkezinde bayrağıyla uygular:

```xaml
<RelativeLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
  <BoxView Color="Blue" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = 0}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Red" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .9}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Gray" WidthRequest="15" x:Name="pole"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .45}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = .25}" />
  <BoxView Color="Green"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
    RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.2, Constant=20}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

Yukarıdaki kodu şu yönlerini dikkat edin:

- Konumlar hem de boyutları kısıtlama olarak belirtilir.
- Flagpole adlı böylece bayrağın (yeşil kutunun) konumu flagpole göre ayarlanabilir.
- Kısıtlama ifadeleri sahip `Factor` ve `Constant` konumları ve boyutları katları tanımlamak için kullanılabilecek özellikler (veya kesir) özelliklerinin diğer nesnelerin yanı sıra, bir sabit. Sabitler negatif olabilir.

### <a name="gridgridmd"></a>[Kılavuz](grid.md)

`Grid` Satırlar ve sütunlar öğeleri görüntülemek için kullanılır. Hücre, satır üstbilgi ve altbilgi veya satırlar ve sütunlar arasındaki kavramını içermez kılavuz bir tablo olmadığını unutmayın. Genel olarak, kılavuz tablosal verileri görüntülemek için uygun değil. Bu kullanım için göz önünde bir [ListView](~/xamarin-forms/user-interface/listview/index.md) veya [Tablo görünümü](~/xamarin-forms/user-interface/tableview.md).

Ne zaman örneği bir `Grid` doğru düzeni kullanmak için sayısal bir giriş için bir hesap makinesi göz önünde bulundurun. Dört satır ve üç sütun, her bir düğmeye sahip bir hesap makinesi için bir sayısal giriş oluşabilir. Aşağıdaki kod bu tasarım uygular:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>

  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Button Text="1" Grid.Row="0" Grid.Column="0" />
  <Button Text="2" Grid.Row="0" Grid.Column="1" />
  <Button Text="3" Grid.Row="0" Grid.Column="2" />
  <Button Text="4" Grid.Row="1" Grid.Column="0" />
  <Button Text="5" Grid.Row="1" Grid.Column="1" />
  <Button Text="6" Grid.Row="1" Grid.Column="2" />
  <Button Text="7" Grid.Row="2" Grid.Column="0" />
  <Button Text="8" Grid.Row="2" Grid.Column="1" />
  <Button Text="9" Grid.Row="2" Grid.Column="2" />
  <Button Text="0" Grid.Row="3" Grid.Column="1" />
  <Button Text="&lt;-" Grid.Row="3" Grid.Column="2" />
</Grid>
```

Yukarıdaki kodu şu yönlerini dikkat edin:

- Kılavuzlar ve sütunları açık, çıkarılan içerikten değil belirtilir.
- `Height` ve `Width` değerleri ayarlanabilir yıldız için kılavuz bu değerleri kullanılabilir alanı dolduracak şekilde ayarlayacak anlamına gelir.
- Her bir düğmenin konumu tarafından belirtilen `Grid.Row`  &  `Grid.Column` özellikleri.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Yapısı, hizalama ve üst öğesiyle ilişkili bir görünüm için genişletme tanımlamak için kullanılabilir.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Kenar Boşlukları ve Doldurma](margin-and-padding.md)

[ `Margin` ](xref:Xamarin.Forms.View.Margin) Ve [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) özellikleri kullanıcı arabiriminde bir öğe oluşturulduğunda düzen davranışını denetler.

<a name="input_transparency" />

### <a name="input-transparency"></a>Giriş saydamlık

Her öğeye sahip bir [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) öğe giriş alıp almadığını tanımlamak için kullanılan özellik. Varsayılan değeri `false`, sağlama öğe giriş alır.

Ne zaman, değer aktarımları alt öğeleri için bir düzen sınıfı gibi bir kapsayıcı sınıfı üzerinde bu özelliği ayarlayın. Bu nedenle, ayarı [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) özelliğini `true` bir düzen sınıfı giriş almıyor Düzen dahilinde öğeleri neden olur.

### <a name="device-orientationdevice-orientationmd"></a>[Cihaz Yönü](device-orientation.md)

Xamarin.Forms ve kendi yerleşik düzenleri cihaz yönü değişiklikleri işleme yeteneğine sahiptir. Uygulamanızın destekleyeceği, de hale getireceğiz nasıl hangi yönleri göz önünde bulundurun yatay ve dikey modda sağlanan alanı kullanın.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Tablet ve Masaüstü uygulamaları için Düzen](tablet.md)

iOS, Android ve evrensel Windows platformu tüm destek büyük ekran boyutları üzerinde tablet cihazları (Ayrıca, dizüstü bilgisayarlar ve Windows için masaüstü bilgisayarlar). Xamarin.Forms cihaz türü ve sayfa düzeni ayarlamayı algılama veya tamamen farklı bir sayfa, tamamen büyük ekranlar için kullanarak uygulamanızı daha büyük ekranlar için en iyi hale getirmenize olanak tanır.

### <a name="creating-a-custom-layoutcustommd"></a>[Özel Düzen Oluşturma](custom.md)

Xamarin.Forms tanımlar dört düzen sınıfları - [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout), [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout), ve [ `Grid` ](xref:Xamarin.Forms.Grid), ve her alt öğeleri farklı bir şekilde düzenler. Ancak, bazen bir düzen kullanmayan sayfa içeriği düzenlemek için gerekli sağlanan Xamarin.Forms tarafından. Bu makalede, özel düzen sınıfının nasıl kullanılacağını açıklar ve yönlendirmesini duyarlı gösterir `WrapLayout` alt öğeleri yatay sayfada yerleştirir ve ardından sonraki alt öğelerine ek satırlar görüntülenmesini sarmalar sınıfı.

### <a name="layout-compressionlayout-compressionmd"></a>[Düzen Sıkıştırma](layout-compression.md)

Düzen sıkıştırma belirtilen düzenleri görsel ağaç'tan sayfa işleme performansını girişimi kaldırır. Bu teslim performans avantajı, bir sayfa, kullanılan işletim sistemi sürümü ve uygulamanın üzerinde çalıştığı aygıtın karmaşıklığına bağlı olarak değişir. Ancak, en büyük gelen performans artışı eski cihazlarda görülür.

## <a name="making-your-choice"></a>Seçiminizi yapma

Çoğu durumda, birden fazla Düzen seçim, istenen bir tasarım uygulamak için kullanılabileceğini unutmayın. Birden çok geçerli seçenek olduğunda, hangi yaklaşımın durumunuz için en kolay olacağını göz önünde bulundurun.
Olarak iç içe yerleştirme düzenlerini daha karmaşık tasarımlar oluşturmak için gereken şekilde çoğu tasarımları tek düzen ile gerçekleşen olamaz.


## <a name="related-links"></a>İlgili bağlantılar

- [Apple İnsan Arabirimi yönergelerine](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android tasarım Web sitesi](https://developer.android.com/design/index.html)
- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
