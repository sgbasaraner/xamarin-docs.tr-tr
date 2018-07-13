---
title: Xamarin.Forms Tablo görünümü
description: Bu makalede, kaydırma menüler, ayarlar ve giriş forms uygulamaları sunmak için Xamarin.Forms Tablo görünümü sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 47cd79611cfeaf48c0422772d8f3e75eb57ba771
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996059"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms Tablo görünümü

[Tablo görünümü](xref:Xamarin.Forms.TableView) kaydırılabilir veri ya da seçenek listesini görüntülemek için bir görünüm olduğundan aynı şablonu paylaşmayın satırları olduğunda. Farklı [ListView](~/xamarin-forms/user-interface/listview/index.md), tablo görünümü kavramını yok bir `ItemsSource`, öğeleri alt öğesi olarak el ile eklenmesi gerekir.

Bu kılavuz aşağıdaki bölümlerden oluşur:

- **[Kullanım örnekleri](#Use_Cases)**  &ndash; ne zaman Tablo görünümü ListView ya da özel bir görünüm yerine kullanılır.
- **[Tablo görünümü yapısı](#TableView_Structure)**  &ndash; bir tablo görünümü içinde gerekli hiyerarşi görünüm.
- **[Tablo görünümü Görünüm](#TableView_Appearance)**  &ndash; Tablo görünümü özelleştirme seçenekleri.
- **[Yerleşik hücreleri](#Built-In_Cells)**  &ndash; dahil olmak üzere yerleşik hücre seçeneklerini [EntryCell](#EntryCell) ve [SwitchCell](#SwitchCell).
- **[Özel hücreleri](#Custom_Cells)**  &ndash; kendi özel hücreleri yapma.

![](tableview-images/tableview-all-sml.png "Tablo görünümü örneği")

<a name="Use_Cases" />

## <a name="use-cases"></a>Kullanım örnekleri

Tablo görünümü durumlarda yararlı olur:

- ayarlar listesi sunma,
- bir formdaki verileri toplama veya
- satırdan satır (örneğin, sayılar, yüzdeleri ve görüntüleri) farklı şekilde sunulan veriler gösteriliyor.

Tablo görünümü kaydırma ve satır çekici bölümlerde Yukarıdaki senaryoların ortak gereksinimi düzenlemeyi işler. `TableView` Denetimi, her platformun temel eşdeğer görünümü kullanılabilir olduğunda, her platform için yerel bir görünüm oluşturma kullanır.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>Tablo görünümü yapısı

Öğeleri bir `TableView` bölümler halinde düzenlenmiştir. Kökünde `TableView` olduğu `TableRoot`, bir veya daha üst olduğu `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

Her `TableSection` başlığı ve bir veya daha fazla ViewCells oluşur. Burada görürüz `TableSection`'s `Title` özelliğini *"Zil"* oluşturucu içinde:

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

## <a name="tableview-appearance"></a>Tablo görünümü görünümü

Tablo görünümü sunan `Intent` özelliği aşağıdaki seçeneklerden bir sabit listesidir:

- **Veri** &ndash; veri girişleri görüntülenirken kullanılacak. Unutmayın [ListView](~/xamarin-forms/user-interface/listview/index.md) veri listeleri kaydırma için daha iyi bir seçenek olabilir.
- **Form** &ndash; Tablo görünümü Form olarak davrandığı zaman kullanmak için.
- **Menü** &ndash; seçimleri menüsü sunarken kullanmak için.
- **Ayarları** &ndash; yapılandırma ayarlarının listesi görüntülenirken kullanılacak.

`TableIntent` Seçtiğiniz etkileyebilir nasıl `TableView` her platformda görünür. Olsa bile farklar NET değildir, seçmek için bir en iyi yöntemdir `TableIntent` nasıl tablosunu kullanmak istediğinize en yakından eşleşen.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Yerleşik hücreleri

Xamarin.Forms, toplama ve bilgi görüntülemek için yerleşik hücre ile birlikte gelir. ListView ve Tablo görünümü aynı hücrelerin tümü kullanmanız mümkün olmakla birlikte, bir tablo görünümü senaryo için en uygun şunlardır:

- **SwitchCell** &ndash; sunmak ve bir metin etiketi ile birlikte bir doğru/yanlış durumunu yakalama.
- **EntryCell** &ndash; sunmak ve metin yakalama.

Bkz: [ListView hücresi Görünüm](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) ayrıntılı bir açıklaması için [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) ve [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) Denetim sunmak ve yakalamak için kullanılacak bir açık/kapalı veya `true` / `false` durumu.

SwitchCells düzenlemek için metin ve açık/kapalı özelliği bir satır vardır. Bu özelliklerin her ikisi de bağlanabilir.

- `Text` &ndash; anahtar görüntülenen metin.
- `On` &ndash; olup açık veya kapalı anahtar görüntülenir.

Unutmayın `SwitchCell` sunan `OnChanged` olay, hücrenin durumunda değişikliklerine yanıt verme etmenize imkan sağlar.

![](tableview-images/switch-cell.png "SwitchCell örneği")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](xref:Xamarin.Forms.EntryCell) kullanıcının düzenleyebildiği metin verileri görüntülemek, ihtiyacınız olduğunda yararlıdır. `EntryCell`s özelleştirilebilir aşağıdaki özellikleri sunmaktadır:

- `Keyboard` &ndash; Düzenleme sırasında görüntülemek için klavye. Sayısal değerleri, e-posta, telefon numaralarını vb. gibi şeyler için seçenekler vardır. [API belgeleri görmek](xref:Xamarin.Forms.Keyboard).
- `Label` &ndash; Metin girişi alanı sağında görüntülenecek etiket metni.
- `LabelColor` &ndash; Etiketin metin rengi.
- `Placeholder` &ndash; Giriş alanı null veya boş olduğunda görüntülenen metin. Bu metin, metin girişi başladığında kaybolur.
- `Text` &ndash; Metin girişi alanında.
- `HorizontalTextAlignment` &ndash; Metnin yatay hizası. Sol veya sağ merkezi hizalanabilir. [API belgeleri görmek](xref:Xamarin.Forms.TextAlignment).

Unutmayın `EntryCell` sunan `Completed` kullanıcı 'Tamamlandı' klavyede metin düzenleme sırasında ulaştığında, harekete geçirilen olay.

![](tableview-images/entry-cell.png "EntryCell örneği")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Özel hücreleri
Yerleşik hücreleri yeterli olmadığı durumlarda, özel hücreleri sunmak ve uygulamanız için anlamlı bir şekilde veri yakalamak için kullanılabilir. Örneğin, bir kullanıcının görüntünün opaklığına seçmesine izin vermek için bir kaydırıcı sunmak isteyebilirsiniz.

Tüm özel hücreleri öğesinden türetilmelidir [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), aynı temel sınıf tüm yerleşik hücresi türlerini kullanın.

Bu, özel bir hücre örneğidir:

![](tableview-images/custom-cell.png "Özel hücre örneği")

### <a name="xaml"></a>XAML
Yukarıdaki bir düzen oluşturmak için XAML aşağıda verilmiştir:

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

Yukarıdaki XAML çok yapıyor. Bu konuyu biraz açalım:

- Kök öğe altında `TableView` olduğu `TableRoot`.
- Var olan bir `TableSection` hemen altındaki `TableRoot`.
- `ViewCell` Doğrudan altında TableSection tanımlanır. Farklı `ListView`, `TableView` bu özel (veya herhangi) gerektirmez hücreleri tanımlanmış bir `ItemTemplate`.
- Bir StackLayout özel hücre düzenini yönetmek için kullanılır. Herhangi bir düzeni burada kullanılabilir.

### <a name="cnum"></a>C&num;

Çünkü `TableView` çalışır statik veri veya el ile değiştirilen veri ile bir öğe şablonunu kavramı yoktur. Bunun yerine, özel hücreleri el ile oluşturulabilir ve tabloya yerleştirin. Sınıfından devralan bir özel oluşturma tekniği hücre Not `ViewCell`, eklemeden sonra `TableView` yerleşik bir hücre istiyorsunuz, ayrıca desteklenir.
Yukarıdaki düzenini gerçekleştirmek için c# kod aşağıdaki gibidir:

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

C# üzerinde çok yapıyor. Bu konuyu biraz açalım:

- Kök öğe altında `TableView` olduğu `TableRoot`.
- Var olan bir `TableSection` hemen altındaki `TableRoot`.
- `ViewCell` Doğrudan altında TableSection tanımlanır. Farklı `ListView`, `TableView` bu özel (veya herhangi) gerektirmez hücreleri tanımlanmış bir `ItemTemplate`.
- Bir StackLayout özel hücre düzenini yönetmek için kullanılır. Herhangi bir düzeni burada kullanılabilir.

Bir sınıf özel hücre için hiçbir zaman tanımlandığını aklınızda bulundurun. Bunun yerine, `ViewCell`ait görünüm özelliği için belirli bir örneğine ayarlanmış `ViewCell`.



## <a name="related-links"></a>İlgili bağlantılar

- [Tablo görünümü (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
