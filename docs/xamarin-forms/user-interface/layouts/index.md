---
title: "Düzenleri"
description: "Görünümleri ekranında yerleştirme."
ms.topic: article
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 1fe290983bf7b130dee6f1a1878a32dce3efc4c4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="layouts"></a>Düzenleri

Xamarin.Forms birçok düzenleri ve ekran içeriğini düzenlemek için özellikler içerir. Her bir düzen denetimi, ekran yönünü değişiklikleri işlemesi hakkında ayrıntılar yanı sıra aşağıda verilmektedir:

* **[StackLayout](stack-layout.md)**  &ndash; görünümleri doğrusal olarak, düzenlemek için kullanılan yatay veya dikey olarak. Bir StackLayout görünümlerde için sola veya sağa düzeninin merkezi hizalanabilir.
* **[AbsoluteLayout](absolute-layout.md)**  &ndash; koordinatları ayarlayarak görünümleri düzenleme & Mutlak değerler veya oranları bakımından boyutu için kullanılır. AbsoluteLayout yanı sıra görünümleri katman sol, sağ veya center bağlamak için kullanılabilir.
* **[RelativeLayout](relative-layout.md)**  &ndash; kısıtlamaları kendi üst öğenin boyut ve konum göre ayarlayarak görünümleri düzenlemek için kullanılan.
* **[Kılavuz](grid.md)**  &ndash; bir kılavuz görünümlerde düzenlemek için kullanılır. Satırları ve sütunları mutlak değerler veya oranları bakımından belirtilebilir.
* **[ScrollView](scroll-view.md)**  &ndash; bir görünüm tamamen ekran sınırları içinde sığamıyorsa zaman kaydırmayı sağlamak için kullanılır.
* **[LayoutOptions](layout-options.md)**  &ndash; hizalama ve kendi üst göreli bir görünüm için genişletme tanımlayın.
* **[Giriş saydamlık](#input_transparency)**  &ndash; bir öğe girişi alıp almadığını belirtir.
* **[Kenar boşluğu bırakma ve doldurma](margin-and-padding.md)**  &ndash; bir öğenin kullanıcı arabiriminde işlendiğinde düzen davranışını denetleyen gösterilmiştir.
* **[Cihaz yönlendirmesini](device-orientation.md)**  &ndash; cihaz yönlendirmesini değişiklikleri nasıl ele alınacağını açıklar.
* **[Düzen tablet ve Masaüstü cihazlarda](tablet.md)**  &ndash; daha büyük ekranda her platform için en iyi duruma getirme gösterir.
* **[Özel bir düzen oluşturma](custom.md)**  &ndash; özel yerleşim sınıfın nasıl oluşturulacağı açıklanmaktadır.
* **[Düzenini sıkıştırma](layout-compression.md)**  &ndash; kaldırır belirtilen görsel ağaç düzeninden sayfa işleme performansı girişimi.

Platform denetimleri de kullanılabilir doğrudan Xamarin.Forms düzenleriyle [ **yerel katıştırma** ](~/xamarin-forms/platform/native-views/index.md) (yeni Xamarin.Forms 2.2), ve [ **özel düzenleroluşturma** ](custom.md) belirli gereksinimlerini karşılamak için.

Aşağıdaki grafikte düzen denetimleri visualizes:

[ ![](images/layouts-sml.png "Xamarin.Forms düzenleri")](images/layouts.png "Xamarin.Forms düzenleri")

## <a name="choosing-the-right-layout"></a>Sağ Düzen seçme

Uygulamanızda seçtiğiniz düzenleri Yardım veya çekici ve kullanılabilir bir Xamarin.Forms uygulaması oluşturduğunuzu gibi ölçeklenme. Her düzeni works temizleyici ve daha ölçeklenebilir UI kod yazmayı nasıl yardımcı olabileceğini göz önüne almanız gereken bazı sürüyor. Bir ekran, belirli bir tasarım elde etmek için farklı düzenleri bileşimini olabilir.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout` Yatay veya dikey çizgi boyunca görünümleri görüntülemek için kullanılır. Konum ve boyut Düzen dahilinde belirlenir bir görünümün üzerinde temel `HeightRequest`, `WidthRequest`, `HorizontalOptions` ve `VerticalOptions`. `StackLayout` genellikle diğer düzenleri ekranında düzenleme temel düzeni olarak kullanılır.

Ne zaman ilişkin bir örnek `StackLayout` iyi bir seçimdir olması, bir düğmeyi ve sola hizalı etiketli ve sağa hizalı düğmesine bir etiket görüntülemek için gereken bir uygulamayı göz önünde bulundurun.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout` Görünümlerle, boyutunu ve konumunu ya da belirtilen açık değerleri veya değerleri düzeni boyutuna göre görüntülemek için kullanılır. Farklı `StackLayout` ve `Grid`, `AbsoluteLayout` alt çakışmasına görünümleri sağlar. Farklı `RelativeLayout`, `AbsoluteLayout` ekranın öğeleri yerleştirme izin vermez.

Ne zaman ilişkin bir örnek `AbsoluteLayout` iyi bir seçimdir olması, yığınları olarak nesne koleksiyonları sunmak için gereken bir uygulamayı göz önünde bulundurun. Bu genellikle, fotoğraf veya şarkıya albümleri sunan görülür. Aşağıdaki kod, küme görünümünü yığını içeriğini ipucu için Döndürülmüş öğelerle sağlar:

XAML'de:

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

Yukarıdaki kod şu yönlerini dikkat edin:

- Her `Image` (ortasında yatay boşluk) aynı konumda görüntülenir
- `Padding` Tarafından kabul `AbsoluteLayout`öğesinden farklı olarak `RelativeLayout`, hangi yoksayar.
- `AbsoluteLayout.LayoutFlags` Düzen sınırları nasıl yorumlanacak belirtir. Bu durumda `PositionProportional`, boyutu açık boyutu olarak yorumlanacak sırada koordinatları düzeni boyutunu oranını olacağı anlamına gelir.
- `AbsoluteLayout.Layoutbounds` Yatay konum, dikey konum, genişlik ve yükseklik bu sırayla belirtir.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout` Boyutunu ve konumunu değerleri düzeni veya başka bir görünüm değerlerini göreli olarak belirtilen ile görünümleri görüntülemek için kullanılır. Göreli değerler eşleşecek şekilde kendisine karşılık gelen değer ilgili görünümde gerekmez. Örnek olarak, bir görünümün ayarlamak olası `Width` başka bir görünüme ait orantılı özelliğini `X` özelliği.

RelativeLayout aygıt boyutları arasında orantılı olarak ölçeklendirilir Uı'lar oluşturmak için kullanılabilir. Aşağıdaki XAML bir tasarımla en üstteki köşelerinde kutusuyla flagpole Center'da bayrağı uygular:

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

Yukarıdaki kod şu yönlerini dikkat edin:

- Konumlar ve boyutları kısıtlamaları olarak belirtilir.
- Flagpole adlı böylece bayrağın (yeşil kutunun) konumu flagpole göre ayarlanabilir.
- Kısıtlama ifadelerin `Factor` ve `Constant` konumlarını ve boyutlarını katları tanımlamak için kullanılan özellikler (veya kesirler) diğer nesnelerin yanı sıra, bir sabit özelliklerinin. Sabitler negatif olabilir.

### <a name="gridgridmd"></a>[Kılavuz](grid.md)

`Grid` Satırlar ve sütunlar öğeleri görüntülemek için kullanılır. Hücre, üstbilgi ve altbilgi satırları veya satırlar ve sütunlar arasındaki kenarlıkları kavramı yok kılavuz bir tablo olmadığından olduğunu unutmayın. Genel olarak, kılavuz tablo veri görüntülemek için uygun değil. Bu kullanım için göz önünde bulundurun bir [ListView](~/xamarin-forms/user-interface/listview/index.md) veya [Tablo görünümü](~/xamarin-forms/user-interface/tableview.md).

Ne zaman örneği için bir `Grid` sağ düzeni kullanmak için bir sayısal giriş hesaplayıcı için göz önünde bulundurun. Bir hesap makinesi için sayısal bir giriş dört satır ve her bir düğme ile üç sütun içerebilir. Aşağıdaki kod bu tasarım uygular:

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

Yukarıdaki kod şu yönlerini dikkat edin:

- Kılavuzları ve sütunları açıkça, içerikten olayla değil belirtilir.
- `Height` ve `Width` değerleri ayarlanabilir yıldız için kılavuz kullanılabilir alanı dolduracak şekilde bu değerleri ayarlayacaksınız anlamına gelir.
- Her düğmenin konumu tarafından belirtilen `Grid.Row`  &  `Grid.Column` özellikleri.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Yapısı hizalama ve kendi üst göreli bir görünüm için genişletme tanımlamak için kullanılabilir.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Kenar boşluğu bırakma ve doldurma](margin-and-padding.md)

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Ve [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) özellikleri, bir öğenin kullanıcı arabiriminde işlendiğinde düzen davranışını denetler.

<a name="input_transparency" />

### <a name="input-transparency"></a>Giriş saydamlık

Her öğeye sahip bir [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) öğe girişi alıp almadığını tanımlamak için kullanılan özellik. Varsayılan değeri olduğu `false`, öğe girişi alır sağlama.

Bu özellik, değer aktarımları alt öğeleri için bir düzen sınıfı gibi bir kapsayıcı sınıfını ayarlandığında. Bu nedenle, ayarı [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) özelliğine `true` bir düzen giriş almıyor düzeni içinde öğelerin sınıfı sonuçlanır.

### <a name="device-orientationdevice-orientationmd"></a>[Cihaz yönlendirmesini](device-orientation.md)

Xamarin.Forms ve yerleşik düzenleri cihaz yönlendirmesini değişiklikleri işleyebilir. Uygulamanızı destekler, de hale getireceğiz nasıl hangi yönler göz önünde bulundurun yatay ve dikey modlarında sağlanan alanı kullanın.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Tablet ve Masaüstü uygulamaları için düzeni](tablet.md)

iOS, Android ve Windows platformları üzerinde büyük ekran boyutlarına desteklemez tablet aygıtları (yanı sıra dizüstü ve masaüstü bilgisayarlar Windows için). Xamarin.Forms cihaz türüne ve sayfa düzeni ayarlama algılama veya farklı bir sayfa için büyük ekranlar tamamen kullanarak uygulamanızı büyük ekranlar için en iyi hale getirmenize olanak tanır.

### <a name="creating-a-custom-layoutcustommd"></a>[Özel bir düzen oluşturma](custom.md)

Xamarin.Forms tanımlar dört düzeni sınıfları - [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/), ve [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), ve her alt farklı bir şekilde düzenler. Ancak, bazen bir düzen kullanmayan sayfa içeriği düzenlemek için gerekli sağlanan tarafından Xamarin.Forms. Bu makalede, nasıl özel yerleşim sınıfı yazılacağını açıklar ve yönlendirme duyarlı gösteren `WrapLayout` , alt öğelerini yatay sayfa boyunca düzenler ve ek satırlar sonraki alt öğelerini görüntüsünü sarmalar sınıfı.

### <a name="layout-compressionlayout-compressionmd"></a>[Düzenini sıkıştırma](layout-compression.md)

Düzenini sıkıştırma belirtilen düzenleri sayfa işleme performansı girişimi görsel ağaç kaldırır. Bu teslim performans avantajı bir sayfa, kullanılan işletim sistemi sürümü ve uygulamanın çalıştığı aygıt karmaşıklığına bağlı olarak değişir. Ancak, büyük performans artışı eski cihazlarda görülür.

## <a name="making-your-choice"></a>Tercih ettiğiniz yapma

Çoğu durumda, birden fazla düzeni seçim istenen tasarımınızı uygulamak için kullanılabilir olduğunu unutmayın. Birden fazla geçerli seçim olduğunda hangi yaklaşımın durumunuz için en kolay olacaktır göz önünde bulundurun.
İç içe düzenleri olarak daha karmaşık tasarımlar oluşturmak için gereken şekilde çoğu tasarımları tek düzen, gerçekleşen alamazsınız.


## <a name="related-links"></a>İlgili bağlantılar

- [Apple İnsan Arabirimi yönergelerine](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android tasarım Web sitesi](https://developer.android.com/design/index.html)
- [Düzen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble örneği (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
