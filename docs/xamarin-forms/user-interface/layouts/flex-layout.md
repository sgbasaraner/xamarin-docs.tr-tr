---
title: Xamarin.Forms FlexLayout
description: FlexLayout yığınlama veya alt görünümleri koleksiyonunu sarmalama için kullanın.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: a6c1b0a4e0df1c25f595ca4eb53079c74b84972e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998589"
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.Forms FlexLayout

_FlexLayout yığınlama veya alt görünümleri koleksiyonunu sarmalama için kullanın._

Xamarin.Forms [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) Xamarin.Forms içinde sürüm 3. 0'da yenidir. CSS temel [esnek kutusu düzeni Modülü](http://www.w3.org/TR/css-flexbox-1/)olarak bilinen yaygın olarak _esnek Düzen_ veya _flex-box_, alt öğelerini düzenlemek için çok sayıda esnek seçenekler içerdiğinden bu nedenle çağırılır içinde düzeni.

`FlexLayout` Xamarin.Forms için benzer [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) içeren, alt öğeleri Yatayda ve Dikeyde bir yığın içinde düzenleyebilirsiniz. Ancak, `FlexLayout` ayrıca varsa bir tek bir satır veya sütunu sığdırmak için çok fazla alt sarmalama uyumlu olan ve ayrıca yönlendirme, hizalama ve çeşitli ekran boyutlarına uyum sağlamak için birçok seçenek vardır.

`FlexLayout` öğesinden türetilen [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) ve devralan bir [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) türünün özelliği `IList<View>`.

`FlexLayout` altı genel bağlanabilir özellikler ve boyutunu, yönünü ve onun alt öğeleri hizalamasını etkileyen beş ekli bağlanabilir özellikler tanımlar. (Eklenen bağlanabilir özellikler ile ilgili bilgi sahibi değilseniz bkz  **[iliştirilmiş özellikler](~/xamarin-forms/xaml/attached-properties.md)**.) Bu özellikleri bölümlerde ayrıntılı olarak açıklanan **[ayrıntılı bağlanabilir Özellikler](#bindable-properties)** ve  **[ayrıntılıeklibağlanabilirÖzellikler](#attached-properties)**. Ancak, bu makalede bir bölümü bazı başlar **[genel kullanım senaryoları](#common-scenarios)** , `FlexLayout` , bu özelliklerin çoğu daha az resmi açıklar. Makale sonuna doğru nasıl birleştirileceğini göreceğiniz `FlexLayout` ile [CSS stil sayfaları](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Genel kullanım senaryoları

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek programını içeren birkaç sayfadaki bazı yaygın kullanımları, o gösteren `FlexLayout` ve özellikleri ile denemeler olanak tanır.

### <a name="using-flexlayout-for-a-simple-stack"></a>FlexLayout için basit bir yığın kullanma

**Basit yığın** gösterir sayfasında nasıl `FlexLayout` için yedek bir `StackLayout` ancak daha basit biçimlendirme. Bu örnekte her şeyi XAML sayfası tanımlanır. `FlexLayout` Dört alt öğeleri içerir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.SimpleStackPage"
             Title="Simple Stack">

    <FlexLayout Direction="Column"
                AlignItems="Center"
                JustifyContent="SpaceEvenly">

        <Label Text="FlexLayout in Action"
               FontSize="Large" />

        <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />

        <Button Text="Do-Nothing Button" />

        <Label Text="Another Label" />
    </FlexLayout>
</ContentPage>
```

İOS, Android ve evrensel Windows platformu üzerinde çalışan bu sayfa şu şekildedir:

[![Basit yığın sayfa](flex-layout-images/SimpleStack.png "basit yığın sayfası")](flex-layout-images/SimpleStack-Large.png#lightbox)

Üç özelliklerini `FlexLayout` gösterilir **SimpleStackPage.xaml** dosyası:

- [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Özelliği değerine ayarlanmış [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection) sabit listesi. Varsayılan, `Row` değeridir. Özelliğini `Column` alt neden `FlexLayout` öğeleri tek bir sütunda düzenlenmesini.

    Zaman öğeler bir `FlexLayout` bir sütunda düzenlenmiş `FlexLayout` barındırıyor dikey olarak _ana eksende_ ve yatay _çapraz ekseni_.

- [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Özelliği türüdür [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems) ve öğeleri çapraz ekseni üzerinde nasıl hizalandığını belirtir. `Center` Seçeneği her öğenin yatay olarak ortalanmış neden olur.

    Kullandıysanız bir `StackLayout` yerine `FlexLayout` bu görev için tüm öğeleri atayarak Merkezi `HorizontalOptions` özelliği için her bir öğenin `Center`. `HorizontalOptions` Özelliği için alt işe yaramazsa bir `FlexLayout`, ancak tek `AlignItems` özelliği aynı hedefe gerçekleştirir. Gerekirse, kullanabileceğiniz `AlignSelf` geçersiz kılmak için bağlanılabilir özellik bağlı `AlignItems` tek tek öğelerin özelliği:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Bu değişiklikle bunu `Label` sol ucuna konumlandırılmış `FlexLayout` sağdan sola okuma düzeni olduğunda.

- [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Özelliği türüdür [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify)ve öğeleri ana eksende nasıl yerleştirildiğini belirtir. `SpaceEvenly` Seçeneği tüm kalan dikey boşluk tüm öğeleri arasında eşit olarak ve ilk öğenin üstünde ve son öğe aşağıda ayırır.

    Kullandıysanız bir `StackLayout`, atamanız gerekir `VerticalOptions` özelliği için her bir öğenin `CenterAndExpand` benzer etkiyi elde etmek için. Ancak `CenterAndExpand` seçeneği her öğeden ilk öğeden önce ve son öğeden sonra arasında iki katı kadar alan ayırmak. Taklit `CenterAndExpand` seçeneği `VerticalOptions` ayarlayarak `JustifyContent` özelliği `FlexLayout` için `SpaceAround`.

Bunlar `FlexLayout` özellikleri bölümünde daha ayrıntılı olarak ele alınmıştır **[ayrıntılı bağlanabilir Özellikler](#bindable-properties)** aşağıda.

### <a name="using-flexlayout-for-wrapping-items"></a>Öğeleri sarmalama için FlexLayout kullanma

**Fotoğraf sarmalama** sayfasının **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek gösterir nasıl `FlexLayout` alt ek satırlar veya sütunlar sarabilirsiniz. XAML dosyası başlatır `FlexLayout` ve iki özelliklerini atar:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.PhotoWrappingPage"
             Title="Photo Wrapping">
    <Grid>
        <ScrollView>
            <FlexLayout x:Name="flexLayout"
                        Wrap="Wrap"
                        JustifyContent="SpaceAround" />
        </ScrollView>

        <ActivityIndicator x:Name="activityIndicator"
                           IsRunning="True"
                           VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

`Direction` Bu özelliği `FlexLayout` varsayılan ayarı sahiptir, ayarlanmadı `Row`, alt satırlarda düzenlenir ve ana eksende yataydır anlamına gelir.

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Özelliği bir sabit listesi türü [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap). Bir satırın sığması için çok fazla öğe varsa, bu özellik ayarı öğeleri ve bir sonraki satıra sarmalamak neden olur.

Dikkat `FlexLayout` alt bir `ScrollView`. Sayfaya sığması için çok fazla satır varsa sonra `ScrollView` bir varsayılan `Orientation` özelliği `Vertical` ve dikey kaydırma sağlar.

`JustifyContent` Özelliği, böylece her bir öğe aynı miktarda boş alanı tarafından çevrilmiş (yatay ekseni) ana eksende arta kalan alanı ayırır.

Arka plan kod dosyası eklenmeye örnek fotoğraf koleksiyonunu erişir ve `Children` koleksiyonunu `FlexLayout`:

```csharp
public partial class PhotoWrappingPage : ContentPage
{
    // Class for deserializing JSON list of sample bitmaps
    [DataContract]
    class ImageList
    {
        [DataMember(Name = "photos")]
        public List<string> Photos = null;
    }

    public PhotoWrappingPage ()
    {
        InitializeComponent ();

        LoadBitmapCollection();
    }

    async void LoadBitmapCollection()
    {
        int imageDimension = Device.RuntimePlatform == Device.iOS ||
                             Device.RuntimePlatform == Device.Android ? 240 : 120;

        string urlSuffix = String.Format("?width={0}&height={0}&mode=max", imageDimension);

        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("http://docs.xamarin.com/demo/stock.json");
                byte[] data = await webClient.DownloadDataTaskAsync(uri);

                // Convert to a Stream object
                using (Stream stream = new MemoryStream(data))
                {
                    // Deserialize the JSON into an ImageList object
                    var jsonSerializer = new DataContractJsonSerializer(typeof(ImageList));
                    ImageList imageList = (ImageList)jsonSerializer.ReadObject(stream);

                    // Create an Image object for each bitmap
                    foreach (string filepath in imageList.Photos)
                    {
                        Image image = new Image
                        {
                            Source = ImageSource.FromUri(new Uri(filepath + urlSuffix))
                        };
                        flexLayout.Children.Add(image);
                    }
                }
            }
            catch
            {
                flexLayout.Children.Add(new Label
                {
                    Text = "Cannot access list of bitmap files"
                });
            }
        }

        activityIndicator.IsRunning = false;
        activityIndicator.IsVisible = false;
    }
}
```

Üstten alta aşamalı olarak kaydırılan üç platformlarında, çalışan bir program şöyledir:

[![Fotoğraf sarmalama sayfası](flex-layout-images/PhotoWrapping.png "fotoğraf sarmalama sayfası")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Sayfa düzeni FlexLayout ile

Web tasarım adlı bir Standart Düzen yok [ _holy grail_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) çok istenen, ancak genellikle mükemmelliğe mi ile hayata geçirmek sabit bir düzen biçimde olduğundan. Düzen sayfasının üst kısmındaki bir üstbilgi ve altbilgi hem genişletme sayfanın tam genişliğine altındaki oluşur. Sayfa merkezini kullanmaya olan ancak genellikle içerik ve destek bilgileri solundaki sütunlu bir menü ile ana içerik (olarak da adlandırılan bir _kenara_ alan) sağ. [CSS esnek kutu düzeni belirtiminin bölümü 5.4.1](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) holy grail düzen ile bir esnek kutu alırlar nasıl açıklar.

**Holy Grail Düzen** sayfasının **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek bu düzeni kullanarak basit bir uygulama gösterilmektedir `FlexLayout` başka iç içe geçmiş. Bu sayfa, dikey modda bir telefon için tasarlandığından, sol ve sağ içerik alanının yalnızca 50 piksel genişliğinde şunlardır:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.HolyGrailLayoutPage"
             Title="Holy Grail Layout">

    <FlexLayout Direction="Column">

        <!-- Header -->
        <Label Text="HEADER"
               FontSize="Large"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center" />

        <!-- Body -->
        <FlexLayout FlexLayout.Grow="1">

            <!-- Content -->
            <Label Text="CONTENT"
                   FontSize="Large"
                   BackgroundColor="Gray"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center"
                   FlexLayout.Grow="1" />

            <!-- Navigation items-->
            <BoxView FlexLayout.Basis="50"
                     FlexLayout.Order="-1"
                     Color="Blue" />

            <!-- Aside items -->
            <BoxView FlexLayout.Basis="50"
                     Color="Green" />

        </FlexLayout>

        <!-- Footer -->
        <Label Text="FOOTER"
               FontSize="Large"
               BackgroundColor="Pink"
               HorizontalTextAlignment="Center" />
    </FlexLayout>
</ContentPage>
```

Burada üç platformlarda çalıştığı:

[![Holy Grail düzen sayfası](flex-layout-images/HolyGrailLayout.png "Holy Grail düzen sayfası")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Gezinti ve aside alanları ile işlenen bir `BoxView` sol ve sağ.

İlk `FlexLayout` XAML dosyası ana dikey eksen var ve düzenlenmiş bir sütunda üç alt öğeleri içerir. Bunlar üst bilgi, sayfa alt bilgisi ve gövdesi. İç içe `FlexLayout` yatay bir ana ekseni üç çocuğu düzenlenmiş bir satır vardır.

Üç ekli bağlanabilir özellikler, bu programa gösterilmiştir:

- `Order` Ekli bağlanabilir özelliği ilk `BoxView`. Bu özellik, varsayılan değer olan 0 ile bir tamsayıdır. Düzen sırasını değiştirmek için bu özelliği kullanabilirsiniz. Genellikle geliştiriciler, gezinti öğeleri önce biçimlendirme görünmesini sayfasının içeriği tercih ve CPU'nun öğeleri. Ayarı `Order` özelliği ilk `BoxView` değerine diğer eşdüzey değerinden sıradaki ilk öğe olarak görünmesini neden olur. Benzer şekilde, ayarlayarak, bir öğenin son göründüğünü sağlayabilirsiniz `Order` eşdüzey büyük bir değere özellik.

- `Basis` Ekli bağlanabilir özelliği ayarlanmış iki `BoxView` 50 piksel genişliği vermek öğeleri. Bu özellik türünde `FlexBasis`, statik bir özellik türü tanımlayan bir yapısını `FlexBasis` adlı `Auto`, varsayılan değerdir. Kullanabileceğiniz `Basis` piksel boyutunu veya alan miktarını gösteren bir yüzdesini belirtmek için öğe ana eksende kaplar. Çağrıldığı bir _temel_ olduğundan tüm sonraki düzeninin temel bir öğe boyutu belirtir.

- `Grow` Özelliği üzerinde iç içe `Layout` ve `Label` içeriği temsil eden alt. Bu özellik türünde `float` ve 0 varsayılan değerine sahiptir. Pozitif bir değere ayarlandığında, bu öğeyi ve eşdüzey pozitif değerlere sahip ana ekseni boyunca tüm kalan alanı ayrılır `Grow`. Alanı yıldız belirtiminde biraz gibi değerler orantılı olarak ayrılan bir `Grid`.

    İlk `Grow` ekli özelliği üzerinde iç içe `FlexLayout`gösteren bu `FlexLayout` tüm kullanılmayan dikey boşluk içindeki dış kaplaması için `FlexLayout`. İkinci `Grow` ekli özelliği ayarlanmış `Label` bu içeriğin tüm kullanılmayan yatay boşluk içindeki iç kaplayabilir olduğunu gösteren içerik temsil eden `FlexLayout`.

    De mevcuttur benzer `Shrink` iliştirilmiş alt boyutu boyutunu aştığında kullanabileceğiniz bağlanabilir özelliği `FlexLayout` ancak kaydırma gerekli değildir.

### <a name="catalog-items-with-flexlayout"></a>Katalog öğeleri FlexLayout ile

**Katalog öğelerini** sayfasını **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek benzer [örnek 1'bölümü 1. 1'css esnek Düzen kutusunu belirtiminin](http://www.w3.org/TR/css-flexbox-1/#overview)dışında üç monkeys açıklamalarını ve resimleri yatay olarak kaydırılabilir bir dizi görüntüler:

[![Sayfa katalog öğelerini](flex-layout-images/CatalogItems.png "sayfa katalog öğeleri")](flex-layout-images/CatalogItems-Large.png#lightbox)

Her üç monkeys bir `FlexLayout` yer alan bir `Frame` bir açık yükseklik ve genişlik verilir ve olduğu ayrıca daha büyük bir alt `FlexLayout`. Bu XAML dosyasında özelliklerinin çoğunu `FlexLayout` alt biri olduğu örtülü bir stil dışındaki tüm stiller içinde belirtilir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CatalogItemsPage"
             Title="Catalog Items">
    <ContentPage.Resources>
        <Style TargetType="Frame">
            <Setter Property="BackgroundColor" Value="LightYellow" />
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="Margin" Value="10" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="Margin" Value="0, 4" />
        </Style>

        <Style x:Key="headerLabel" TargetType="Label">
            <Setter Property="Margin" Value="0, 8" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="Blue" />
        </Style>

        <Style TargetType="Image">
            <Setter Property="FlexLayout.Order" Value="-1" />
            <Setter Property="FlexLayout.AlignSelf" Value="Center" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="White" />
            <Setter Property="BackgroundColor" Value="Green" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}"
                           WidthRequest="240"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Örtük stil `Image` iki ekli bağlanabilir özelliklerinin ayarlardır `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order` Ayarıyla &ndash;1 nedenleri `Image` önce her bir iç içe geçmiş görüntülenecek öğe `FlexLayout` alt koleksiyonu içindeki konumuna bakılmaksızın görünümleri. `AlignSelf` Özelliği `Center` neden `Image` içinde ortalanmış için `FlexLayout`. Bu ayarı geçersiz kılar `AlignItems` varsayılan değere sahip özelliği, `Stretch`seçeneğidir; yani, `Label` ve `Button` alt tam genişliğine uzatılır `FlexLayout`.

Her üç `FlexLayout` görünümleri, boş bir `Label` önündeki `Button`, ancak bir `Grow` 1 ayarlama. Bu ek dikey boşluğu için bu alanı boş ayrılan anlamına gelir `Label`, hangi etkili bir şekilde gönderim `Button` altına.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Ayrıntılı bir şekilde bağlanabilir Özellikler

Bazı sık kullanılan uygulamaları gördüğünüze göre `FlexLayout`, özelliklerini `FlexLayout` daha ayrıntılı olarak notebook'ta incelenebilir. 
`FlexLayout` üzerinde ayarlanmış altı bağlanabilir özellikler tanımlar `FlexLayout` kendisi, kod veya XAML, Denetim orientatin ve hizalama. (Bu özelliklerden birini [ `Position` ](xref:Xamarin.Forms.FlexLayout.Position), bu makalenin kapsamında değildir.)

Beş ile kullanarak bağlanabilir özellikler kalan deneme **deneme** sayfasının **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek. Bu sayfa ekleme veya alt öğelerden kaldırın sağlar bir `FlexLayout` ve beş bağlanabilir özellikler birleşimlerini ayarlamak için. Tüm alt öğeleri `FlexLayout` olan `Label` çeşitli renkler ve boyutlar, görünümleri ile `Text` bir numara konumuna karşılık gelen özelliği `Children` koleksiyonu.

Program başladığında, beş `Picker` görünümleri görüntülemek için bu beş varsayılan değerleri `FlexLayout` özellikleri. `FlexLayout` Ekranın üç alt öğeleri içerir:

[![: Deneme sayfa Varsayılan](flex-layout-images/ExperimentDefault.png "deneme sayfası - varsayılan")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Her biri `Label` görünümlere sahip için ayrılan alanı gösteren gri renkli bir arka plan `Label` içinde `FlexLayout`. Arka planı `FlexLayout` kendisi Alice Mavisi değildir. Sağ ve sol konumunda küçük bir kenar boşluğu dışında sayfanın tüm alt alanı kaplar.

<a name="direction" />

### <a name="the-direction-property"></a>Direction özelliği

[ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Özelliği türüdür [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection), dört üye olan bir sabit listesi:

- `Column`
- `ColumnReverse` (veya "sütun ters" XAML içinde)
- `Row`, varsayılan
- `RowReverse` (veya "satır-tersten" XAML)

XAML, küçük, büyük harf, sabit listesi üye adları kullanarak bu özelliğin değerini belirtebilirsiniz veya büyük/küçük harf ya da CSS göstergeleri ile aynıdır, parantez içinde gösterilen iki ek dizelerini kullanabilirsiniz. ("Sütun önleyici" ve "satır önleyici" dizeleri tanımlanan [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) XAML ayrıştırıcı tarafından kullanılan sınıf.)

İşte **deneme** (soldan sağa) gösteren sayfa `Row` yön `Column` yön ve `ColumnReverse` yönü:

[![Deneme sayfası: Yönü](flex-layout-images/ExperimentDirection.png "deneme sayfası - yönü")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

İçin dikkat `Reverse` öğeleri başlatma seçenekleri, sağ veya altında.

<a name="wrap" />

### <a name="the-wrap-property"></a>Sarmalama özelliği

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Özelliği türüdür [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap), üç üyesi olan bir sabit listesi:

- `NoWrap`, varsayılan
- `Wrap`
- `Reverse` (veya "wrap-tersten" XAML)

Bu ekranları soldan sağa Göster `NoWrap`, `Wrap` ve `Reverse` 12 alt öğeleri için seçenekleri:

[![Deneme sayfası: Kaydırma](flex-layout-images/ExperimentWrap.png "deneme sayfası - Kaydır")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Zaman `Wrap` özelliği `NoWrap` ana eksende (olduğu gibi bu program) sınırlıdır ve ana eksende geniş veya tüm alt sığdırmak için yeterince uzun değil `FlexLayout` öğeleri daha küçük, iOS ekran olarak yapmaya çalışır gösterir. İle öğelerinin shrinkness denetleyebilirsiniz [ `Shrink` ](#shrink) bağlanılabilir özellik bağlı.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>JustifyContent özelliği

[ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Özelliği türüdür [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), altı üyeleri olan bir sabit listesi:

- `Start` (veya "XAML flex-start"), varsayılan
- `Center`
- `End` (veya "flex-end" XAML içinde)
- `SpaceBetween` (veya "alanı arasında" XAML içinde)
- `SpaceAround` (veya "alanı-ARO" XAML içinde)
- `SpaceEvenly`

Bu özellik, bu örnekte yatay ekseni ana eksende öğeleri nasıl aralıklı belirtir:

[![Deneme sayfası: İçerik Yasla](flex-layout-images/ExperimentJustifyContent.png "deneme sayfası - içerik Yasla")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

Tüm üç ekran görüntülerinde `Wrap` özelliği `Wrap`. `Start` Varsayılan Android önceki ekran görüntüsünde gösterilir. Burada iOS ekran görüntüsünde gösterilmektedir `Center` seçeneği: tüm öğeleri merkezine taşınır. Word ile başlayan diğer üç seçenekten `Space` öğeleri dolu olmayan ek alan ayırın. `SpaceBetween` eşit öğeler arasındaki boşluk ayırır; `SpaceAround` koyar, alan her bir öğenin çevresine eşit olsa `SpaceEvenly` puts alan her bir öğe arasındaki ve ilk öğeden önce ve son öğeden sonra satırdaki eşit.

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems özelliği

[ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Özelliği türüdür [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems), dört üye olan bir sabit listesi:

- `Stretch`, varsayılan
- `Center`
- `Start` (veya "flex-start" XAML içinde)
- `End` (veya "flex-end" XAML içinde)

Bu iki özelliklerinden biri (diğer olan [ `AlignContent` ](#align-content)) alt öğeleri çapraz ekseni üzerinde nasıl hizalandığını belirtir. Her satır içinde alt (önceki ekran görüntüsünde gösterildiği gibi) uzatılacağını veya start, center veya her bir öğenin son aşağıdaki üç ekran görüntülerinde gösterildiği gibi hizalanır:

[![Deneme sayfası: Öğeleri hizalama](flex-layout-images/ExperimentAlignItems.png "deneme sayfası - öğeleri hizalama")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

İOS ekran görüntüsünde, tüm alt taraflarının hizalanır. Android ekran görüntülerinde göre en uzun alt öğeleri dikey ortalanır. UWP ekran görüntüsünde, tüm öğeleri alta hizalanır.

Herhangi bir tek öğe için `AlignItems` ayarı ile kılınabilir [ `AlignSelf` ](#align-self) bağlanılabilir özellik bağlı.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent özelliği

[ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent) Özelliği türüdür [ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent), yedi üyeleri olan bir sabit listesi:

- `Stretch`, varsayılan
- `Center`
- `Start` (veya "flex-start" XAML içinde)
- `End` (veya "flex-end" XAML içinde)
- `SpaceBetween` (veya "alanı arasında" XAML içinde)
- `SpaceAround` (veya "alanı-ARO" XAML içinde)
- `SpaceEvenly`

Gibi `AlignItems`, `AlignContent` özelliği de alt üzerindeki artı eksenine hizalanır ancak tüm satırları veya sütunları etkiler:

[![Deneme sayfası: İçerik hizalama](flex-layout-images/ExperimentAlignContent.png "deneme sayfası - içerik hizalama")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

İOS screnshot içinde her iki satır üstünde olan; Android ekran görüntüsünde Merkezi'nde oldukları; ve UWP ekran görüntüsünde altındaki oldukları. Satırları çeşitli şekillerde aralıklı:

[![Deneme sayfası: İçerik 2 hizalama](flex-layout-images/ExperimentAlignContent2.png "deneme sayfası - içerik 2 Hizala")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` Yalnızca bir satır veya sütun olduğunda hiçbir etkisi olmaz.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Ayrıntılı ekli bağlanabilir Özellikler

`FlexLayout` beş ekli bağlanabilir özellikler tanımlar. Bu özellikleri üzerinde alt ayarlanmış `FlexLayout` ve yalnızca o alt öğesi olarak ilgilidir.

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf özelliği

[ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) Ekli bağlanılabilir özellik türüdür [ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent), beş üyeleri olan bir sabit listesi:

- `Auto`, varsayılan
- `Stretch`
- `Center`
- `Start` (veya "flex-start" XAML içinde)
- `End` (veya "flex-end" XAML içinde)

Tek bir alt için `FlexLayout`, bu özellik geçersiz kılma ayarı [ `AlignItems` ](#align-items) ayarlama özelliği `FlexLayout` kendisi. Varsayılan ayar `Auto` kullanmak anlamına gelir `AlignItems` ayarı.

İçin bir `Label` adlı bir öğe `label` (veya örnek) ayarlayabileceğiniz `AlignSelf` aşağıdakine benzer bir kod özelliği:

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

Başvuru fark `FlexLayout` üst `Label`. XAML içinde özellik şöyle ayarlayın:

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order özelliğini

[ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty) Özelliği türüdür `int`. Varsayılan değer 0’dır.

`Order` Özelliği sayesinde sırasını değiştirmek, alt `FlexLayout` düzenlenir. Genellikle, alt öğelerini bir `FlexLayout` düzenlenmiş görünürler aynı sıra `Children` koleksiyonu. Ayarlayarak bu sıralamayı geçersiz kılabilirsiniz `Order` bağlanılabilir özellik sıfır olmayan bir tamsayı değer üzerinde bir veya daha fazla alt bağlı. `FlexLayout` Ardından alt ayarına göre düzenler `Order` her alt, ancak alt ile aynı özellikte `Order` ayarı görünürler sırayla düzenlenmiş `Children` koleksiyonu.

### <a name="the-basis-property"></a>Temel özelliği

[ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty) Ekli bağlanabilir özelliği belirtir bir alt öğesi için ayrılan alan miktarını `FlexLayout` ana eksende. Boyutu belirtilen tarafından `Basis` özelliktir boyutu üst ana ekseni boyunca `FlexLayout`. Bu nedenle, `Basis` sütunlardaki alt öğeleri düzenlenir alt satır veya yüksekliği düzenlenir bir alt öğenin genişliğini belirtir.

`Basis` Özelliği türüdür [ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis), bir yapı. Her iki CİHAZDAN bağımsız birimler veya boyutunun yüzdesi cinsinden boyutu belirtilebilir `FlexLayout`. Varsayılan değer olan `Basis` statik özellik bir özelliktir `FlexBasis.Auto`, alt genişlik ve yükseklik kullanılan istenen anlamına gelir.

Kod içinde ayarlayabilirsiniz `Basis` özelliği için bir `Label` adlı `label` şöyle 40 CİHAZDAN bağımsız birimler için:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

İkinci bağımsız değişkeni `FlexBasis` Oluşturucusu adlı `isRelative` ve boyutu göreceli olup olmadığını belirtir (`true`) veya mutlak (`false`). Bağımsız değişken varsayılan değerine sahip `false`, aşağıdaki kodu kullanabilirsiniz:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Örtük bir dönüştürme `float` için `FlexBasis` tanımlanır, daha da kolaylaştırmak için:

```csharp
FlexLayout.SetBasis(label, 40);
```

25 oranında boyutu ayarlayabileceğiniz `FlexLayout` üst şöyle:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Bu kesirli değer 0 ile 1 aralığında olmalıdır.

XAML içinde boyut CİHAZDAN bağımsız birimler cinsinden bir sayı kullanabilirsiniz:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Veya aralığında, 0 ile % 100 arasında bir yüzde belirtebilirsiniz:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**Temel deneme** sayfasının **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek denemeler olanak tanır `Basis` özelliği. Sarmalanan bir sütun beş sayfasını görüntüler `Label` arka plan ve ön plan rengini değiştirme ile öğeleri. İki `Slider` öğeleri belirtmenize olanak tanır `Basis` dördüncü ve saniye değerlerini `Label`:

[![Temel sayfa deneme](flex-layout-images/BasisExperiment.png "temel sayfa denemeler yapın")](flex-layout-images/BasisExperiment-Large.png#lightbox)

Sol iOS ekran iki gösterir `Label` yüksekliklerini CİHAZDAN bağımsız birimler verilmiş öğeler. Bunları, toplam yüksekliği avantajlarının yüksekliklerini verilen Android ekran gösterir `FlexLayout`. Varsa `Basis` yüksekliğini alt olan % 100 oranında ayarlanmışsa `FlexLayout`, kaydırma sonraki sütuna ve UWP ekran görüntüsünde gösterildiği gibi bu sütun, tüm yüksekliğini kaplayabilir: beş alt satırda düzenlenmiş gibi görünür , ancak gerçekte beş sütun dizilmişlerdir.

### <a name="the-grow-property"></a>Özellik büyütün

[ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty) Ekli bağlanılabilir özellik türüdür `int`. Varsayılan değer 0'dır ve değeri 0'a eşit veya daha büyük olmalıdır.

`Grow` Özelliği olduğunda ne zaman bir rol oynar `Wrap` özelliği `NoWrap` ve satır alt toplam genişlik değerinden genişliği sahip `FlexLayout`, ya da daha kısa bir yükseklikte sütununun alt `FlexLayout`. `Grow` Özelliği alt öğelerin arasında kalan alanı apportion nasıl gösterir.

İçinde **büyüme deneme** sayfası, beş `Label` değişen öğeleri, bir sütun ve iki düzenlenir `Slider` öğelerin ayarlamanızı izin `Grow` özelliği ikinci ve dördüncü `Label`. Varsayılan sol ucundaki iOS ekran gösterir `Grow` 0 özellikleri:

[![Büyütme deneme sayfanın](flex-layout-images/GrowExperiment.png "büyütme deneme sayfası")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Herhangi bir alt öğesi bir pozitif belirtilmezse `Grow` değer Android ekran görüntüsünde gösterildiği gibi alt tüm kalan alanı kazanır. Bu alan ayrıca iki veya daha fazla alt öğeleri arasında ayrılabilir. UWP ekran görüntüsünde `Grow` ikinci özelliği `Label` 0,5 olarak ayarlanır ancak `Grow` dördüncü özelliği `Label` dördüncü veren 1,5 `Label` üç kata kadar saniyeolarakkalanalanı`Label`.

Alt Görünüm bu alanı nasıl kullandığı belirli alt türüne bağlıdır. İçin bir `Label`, toplam alanı içindeki metni konumlandırılmalıdır `Label` özelliklerini kullanarak `HorizontalTextAlignment` ve `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>Küçültme özelliği

[ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) Ekli bağlanılabilir özellik türüdür `int`. Varsayılan değer 1'dir ve değeri en az 0 olmalıdır.

`Shrink` Özelliği, bir rol oynar olduğunda `Wrap` özelliği `NoWrap` ve bir satır alt toplam genişliğini genişliğini büyükse `FlexLayout`, ya da tek bir sütun alt toplam yüksekliği büyüktür yüksekliği `FlexLayout`. Normalde `FlexLayout` bu alt boyutlarının constricting tarafından görüntülenir. `Shrink` Özelliği, hangi alt öğeleri kendi tam boyutlarda görüntülenmesini içinde öncelik verildiğinden belirtebilirsiniz.

**Küçültme deneme** sayfası oluşturur bir `FlexLayout` beş tek bir satır ile `Label` daha fazla alan gerektiren alt `FlexLayout` genişliği. Tüm iOS ekran sol gösterir `Label` varsayılan değeri 1 olan öğeleri:

[![Küçültme sayfayı deneme](flex-layout-images/ShrinkExperiment.png "küçültme sayfayı denemeler yapın")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android ekran görüntüsünde `Shrink` ikinci değer `Label` 0 ve, ayarlanır `Label` tam genişliğini görüntülenir. Ayrıca, dördüncü `Label` verilen bir `Shrink` birden fazla değer ve küçültülebilir. UWP ekran her ikisini de gösteren `Label` öğeleri yaları bir `Shrink` durumunda bunları tam boyutlarına görüntülenmesine izin vermek için 0 değerini mümkündür.

Her ikisi de ayarlayabilirsiniz `Grow` ve `Shrink` nerede toplama alt boyutları bazen bazen boyutu daha büyük veya küçük durumlarda uyum sağlamak için değerleri `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>FlexLayout ile CSS stil oluşturma

Kullanabileceğiniz [CSS stil](~/xamarin-forms/user-interface/styles/css/index.md) Xamarin.Forms 3.0 ile birlikte sunulan özelliğini `FlexLayout`. **CSS katalog öğelerini** sayfasının **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek yineleme düzenini **katalog öğelerini** sayfasında, ancak bir CSS Çoğu stilleri için stil sayfası:

[![CSS katalog öğeleri sayfası](flex-layout-images/CssCatalogItems.png "CSS katalog öğeleri sayfası")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Özgün **CatalogItemsPage.xaml** beş dosya vardır `Style` tanımlarında kendi `Resources` 15 bölümle `Setter` nesneleri. İçinde **CssCatalogItemsPage.xaml** iki sınırlı dosyası `Style` yalnızca dört tanımlarla `Setter` nesneleri. Bu stiller CSS stil sayfası Xamarin.Forms CSS stil özelliği şu anda desteklemiyor özellikleri için ek:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CssCatalogItemsPage"
             Title="CSS Catalog Items">
    <ContentPage.Resources>
        <StyleSheet Source="CatalogItemsStyles.css" />

        <Style TargetType="Frame">
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey" StyleClass="header" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey" StyleClass="header" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey" StyleClass="header" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

CSS stil sayfası ilk satırında başvuruluyor `Resources` bölümü:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Ayrıca her üç öğeye iki öğe içerdiğini fark `StyleClass` ayarları:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Bu Seçici başvurmak **CatalogItemsStyles.css** stil sayfası:

```css
frame {
    width: 300;
    height: 480;
    background-color: lightyellow;
    margin: 10;
}

label {
    margin: 4 0;
}

label.header {
    margin: 8 0;
    font-size: large;
    color: blue;
}

label.empty {
    flex-grow: 1;
}

image {
    height: 180;
    order: -1;
    align-self: center;
}

button {
    font-size: large;
    color: white;
    background-color: green;
}
```

Birkaç `FlexLayout` ekli bağlanabilir özellikler burada başvuru. İçinde `label.empty` Seçici, göreceksiniz `flex-grow` boş bir stil özniteliği `Label` yukarıdaki bazı boş alanı sağlamak üzere `Button`. `image` Seçici içeren bir `order` özniteliğini ve bir `align-self` özniteliği, ikisi için de karşılık gelir `FlexLayout` bağlanabilir özellikler eklenmiş.

Doğrudan özellikleri ayarlayabilirsiniz gördüğünüz `FlexLayout` ve ekli bağlanabilir özellikler üzerinde alt ayarlayabileceğiniz bir `FlexLayout`. Ya da dolaylı olarak geleneksel XAML tabanlı stilleri veya CSS stilleri kullanarak bu özellikleri ayarlayabilirsiniz. Bildiğiniz ve bu özellikleri anlamak için önemli olan. Bu özellikler ne yapar: `FlexLayout` gerçekten esnek. 

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout Xamarin.University ile

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 düzenine göre esnek [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>İlgili bağlantılar

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
