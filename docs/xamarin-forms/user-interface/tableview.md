---
title: Tablo görünümü
description: Kaydırma menüleri, ayarlar ve giriş formları sunar.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: dc55f3fe70450c71b639cf33166720fc27d45a10
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="tableview"></a>Tablo görünümü

[Tablo görünümü](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) veri ya da seçenek kaydırılabilir listesini görüntülemek için bir görünüm aynı şablonu paylaşmayın satırları olduğu. Farklı [ListView](~/xamarin-forms/user-interface/listview/index.md), tablo görünümü kavramı yok bir `ItemsSource`, öğeleri alt öğesi olarak el ile eklenmesi gerekir.

Bu kılavuz aşağıdaki bölümlerden oluşur:

- **[Kullanım örnekleri](#Use_Cases)**  &ndash; ne zaman Tablo görünümü ListView veya özel bir görünüm yerine kullanılır.
- **[Tablo görünümü yapısı](#TableView_Structure)**  &ndash; bir tablo görünümü içinde gerekli görünümleri hiyerarşisi.
- **[Tablo görünümü Görünüm](#TableView_Appearance)**  &ndash; Tablo görünümü özelleştirme seçenekleri.
- **[Yerleşik hücreleri](#Built-In_Cells)**  &ndash; yerleşik hücre seçenekleri de dahil olmak üzere, [EntryCell](#EntryCell) ve [SwitchCell](#SwitchCell).
- **[Özel hücreleri](#Custom_Cells)**  &ndash; kendi özel hücreleri yapma.

![](tableview-images/tableview-all-sml.png "Tablo görünümü örneği")

<a name="Use_Cases" />

## <a name="use-cases"></a>Kullanım örnekleri

Tablo görünümü ne zaman yararlı olur:

- ayarları listesi sunma,
- Formda, veri toplama veya
- satırdan satır (örn. sayılar, yüzdeleri ve görüntüleri) farklı şekilde sunulan veri gösteriliyor.

Tablo görünümü kaydırma ve satır çekici bölümlerde Yukarıdaki senaryoların ortak gereksinimi düzenlemeyi işler. `TableView` Denetimini kullanan her platformun arka plandaki eşdeğer görünümü kullanılabilir olduğunda, her platform için yerel bir görünüm oluşturma.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>Tablo görünümü yapısı

Öğeleri bir `TableView` bölümlere düzenlenir. Kökündeki `TableView` olan `TableRoot`, bir veya daha üst olduğu `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

Her `TableSection` başlığı ve bir veya daha fazla ViewCells oluşur. Burada gösteriliyor `TableSection`'s `Title` özelliğini *"Halkası"* oluşturucuda:

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

Yukarıdaki gibi XAML aynı düzende gerçekleştirmek için:

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
      <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

<a name="TableView_Appearance" />

## <a name="tableview-appearance"></a>Tablo görünümü Görünüm

Tablo görünümü sunan `Intent` bir numaralandırma aşağıdaki seçeneklerden birini özelliği:

- **Veri** &ndash; veri girişi görüntülenirken kullanılacak. Unutmayın [ListView](~/xamarin-forms/user-interface/listview/index.md) veri listeleri kaydırma için daha iyi bir seçenek olabilir.
- **Form** &ndash; Tablo görünümü Form olarak davrandığı zaman kullanmak için.
- **Menü** &ndash; seçimleri menüsüne sunan olduğunda kullanılacak.
- **Ayarları** &ndash; yapılandırma ayarlarının listesini görüntülenirken kullanılacak.

`TableIntent` Seçtiğiniz etkisi nasıl `TableView` her platformda görüntülenir. Olsa bile farklar NET değildir, bu seçmek için en iyi uygulamadır `TableIntent` tablo kullanmayı düşündüğünüz nasıl en yakından eşleşen.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Yerleşik hücreleri

Xamarin.Forms toplama ve bilgi görüntülemek için yerleşik hücreleri birlikte gönderilir. ListView ve Tablo görünümü aynı hücrelerin tümünü kullanabilmenize karşın, bir tablo görünümü senaryo için en uygun şunlardır:

- **SwitchCell** &ndash; sunan ve bir metin etiketi ile birlikte bir doğru/yanlış durumunu yakalama için.
- **EntryCell** &ndash; sunan ve metin yakalamak için.

Bkz: [ListView hücre Görünüm](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) ayrıntılı bir açıklaması için [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) ve [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) denetimi sunan ve yakalamak için kullanılacak bir açık/kapalı veya `true` / `false` durumu.

SwitchCells düzenlemek için metin ve açık/kapalı özelliği bir satır var. Bu özelliklerin her ikisi de bağlanabilir.

- `Text` &ndash; anahtar gösterilecek metin.
- `On` &ndash; anahtar olarak açık veya kapalı görüntülenip görüntülenmeyeceğini.

Unutmayın `SwitchCell` sunan `OnChanged` olay, hücrenin durumda değişikliklerine yanıt verme olanak sağlar.

![](tableview-images/switch-cell.png "SwitchCell örneği")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) Kullanıcı düzenleyebilir metin verileri görüntülemek gerektiğinde kullanışlıdır. `EntryCell`s özelleştirilebilir aşağıdaki özellikleri sunmaktadır:

- `Keyboard` &ndash; Düzenlerken görüntülemek için klavye. Sayısal değerler, e-posta, telefon numaralarını vb. gibi şeyler için seçeneği vardır. [API belgelerine bakın](http://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/).
- `Label` &ndash; Metin girişi alanı sağındaki görüntülenecek etiket metni.
- `LabelColor` &ndash; Etiket metninin rengi.
- `Placeholder` &ndash; Null veya boş olduğunda giriş alanını görüntülenecek metin. Metin girişi başladığında bu metin kaybolur.
- `Text` &ndash; Metin girişi alanında.
- `HorizontalTextAlignment` &ndash; Metnin yatay hizası. Center, sola veya sağa hizalı. [API belgelerine bakın](http://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/).

Unutmayın `EntryCell` sunan `Completed` kullanıcı 'Tamamlandı' klavyede metin düzenlerken geldiğinde tetiklenir olayı.

![](tableview-images/entry-cell.png "EntryCell örneği")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Özel hücreler
Yerleşik hücreleri yeterli olmadığı durumlarda özel hücreleri sunmak ve uygulamanız için anlamlı şekilde verilerini yakalamak için kullanılabilir. Örneğin, bir kullanıcının bir görüntü geçirgenliğini seçmesine izin ver kaydırıcıyı sunmak isteyebilirsiniz.

Tüm özel hücreleri öğesinden türetilmelidir [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), tüm yerleşik hücrenin yazdığı kullanım aynı temel sınıfı.

Bu, özel bir hücre örneğidir:

![](tableview-images/custom-cell.png "Özel hücre örneği")

### <a name="xaml"></a>XAML
Yukarıdaki düzeni oluşturmak için XAML aşağıdadır:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="DemoTableView.TablePage" Title="TableView">
    <ContentPage.Content>
        <TableView Intent="Settings">
            <TableRoot>
                <TableSection Title="Getting Started">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <Image Source="bulb.png" />
                            <Label Text="left"
                              TextColor="#f35e20" />
                            <Label Text="right"
                              HorizontalOptions="EndAndExpand"
                              TextColor="#503026" />
                        </StackLayout>
                    </ViewCell>
                </TableSection>
            </TableRoot>
        </TableView>
    </ContentPage.Content>
</ContentPage>

```

Yukarıdaki XAML çok yapıyor. Şimdi onu Bölünme:

- Kök öğesi altında `TableView` olan `TableRoot`.
- Var olan bir `TableSection` hemen altındaki `TableRoot`.
- `ViewCell` Doğrudan TableSection altında tanımlanır. Farklı `ListView`, `TableView` bu özel (veya herhangi) gerektirmez hücreleri tanımlanmış bir `ItemTemplate`.
- Bir StackLayout özel hücre düzenini yönetmek için kullanılır. Herhangi bir düzeni burada kullanılabilir.

### <a name="cnum"></a>C&num;

Çünkü `TableView` çalışır statik verileri veya el ile değiştirilen verileri ile bir öğe şablonu kavramı yok. Bunun yerine, özel hücreleri el ile oluşturulabilir ve tabloya yerleştirin. Özel oluşturma teknik hücre Not devraldığı `ViewCell`, eklemeyi sonra `TableView` yerleşik bir hücre istiyorsunuz, ayrıca desteklenir.
Yukarıdaki düzeni gerçekleştirmek için c# kod aşağıdaki gibidir:

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() {Source = "bulb.png"});
layout.Children.Add (new Label() {
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label () {
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot () {
    new TableSection("Getting Started") {
        new ViewCell() {View = layout}
    }
};

Content = table;
```

C# üzerinde çok yapıyor. Şimdi onu Bölünme:

- Kök öğesi altında `TableView` olan `TableRoot`.
- Var olan bir `TableSection` hemen altındaki `TableRoot`.
- `ViewCell` Doğrudan TableSection altında tanımlanır. Farklı `ListView`, `TableView` bu özel (veya herhangi) gerektirmez hücreleri tanımlanmış bir `ItemTemplate`.
- Bir StackLayout özel hücre düzenini yönetmek için kullanılır. Herhangi bir düzeni burada kullanılabilir.

Özel hücre için bir sınıf hiçbir zaman tanımlanan unutmayın. Bunun yerine, `ViewCell`ait görünüm özelliği belirli bir örneği için ayarlanmış `ViewCell`.



## <a name="related-links"></a>İlgili bağlantılar

- [Tablo görünümü (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
