---
title: Xamarin.Forms FlexLayout
description: FlexLayout yığınlama veya alt görünümler koleksiyonu kaydırma için kullanın.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 7585138cd6c33c2a5dc537ba28101a84e1c4b7ae
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.Forms FlexLayout

_FlexLayout yığınlama veya alt görünümler koleksiyonu kaydırma için kullanın._

Xamarin.Forms [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) Xamarin.Forms sürüm 3.0 yenidir. CSS tabanlı [Esnek kutusunu düzeni Modülü](http://www.w3.org/TR/css-flexbox-1/), yaygın olarak bilinen _düzeni esnek_ veya _esnek kutu_, alt öğelerini düzenlemek için esnek birçok seçenek içerdiğinden bu nedenle çağrılır içinde düzeni.

`FlexLayout` Xamarin.Forms benzer [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) içeren, kendi alt öğelerini yatay ve dikey olarak yığında düzenleyebilirsiniz. Ancak, `FlexLayout` de varsa bir tek bir satır veya sütun sığdırmak için çok fazla alt sarmalama işleyebilir ve yönlendirme, hizalama ve çeşitli ekran boyutlarına uyum sağlamak için birçok seçeneğiniz de vardır.

`FlexLayout` türetilen [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) ve devralan bir [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) türündeki özelliği `IList<View>`.

`FlexLayout` altı ortak bağlanabilir özellikleri ve boyutunu, Yönlendirme ve onun alt öğelerin hizalamasını etkileyen beş ekli bağlanabilir özellikleri tanımlar. (Ekli bağlanabilirse özelliklerle bilmiyorsanız bkz  **[ekli özellikler](~/xamarin-forms/xaml/attached-properties.md)**.) Bu özellikler üzerinde bölümlerde ayrıntılı olarak açıklanmıştır **[bağlanabilir özellikleri ayrıntılı](#bindable-properties)** ve  **[ekli bağlanabilir özellikleri ayrıntılı](#attached-properties)**. Ancak, bu makalede bir bölümü bazı ile başlayan **[genel kullanım senaryoları](#common-scenarios)** , `FlexLayout` , bu özelliklerin çoğu daha az resmi açıklar. Makalenin sonundaki nasıl birleştirileceğini görürsünüz `FlexLayout` ile [CSS stil sayfaları](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Genel kullanım senaryoları

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek programı içeren birkaç sayfa bazı yaygın kullanımları, bu yol oluşturduk `FlexLayout` ve özellikleriyle birlikte denemeler olanak sağlar.

### <a name="using-flexlayout-for-a-simple-stack"></a>İçin basit bir yığın FlexLayout kullanma

**Basit yığın** gösterir sayfasında nasıl `FlexLayout` için birbirinin yerine kullanabilir bir `StackLayout` ancak daha basit biçimlendirme. Bu örnekte her şeyi XAML sayfası içinde tanımlanır. `FlexLayout` Dört alt öğeleri içerir:

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

İOS, Android ve evrensel Windows platformu üzerinde çalışan bu sayfayı şöyledir:

[![Basit yığın sayfa](flex-layout-images/SimpleStack.png "basit yığın sayfası")](flex-layout-images/SimpleStack-Large.png#lightbox)

Üç özelliklerini `FlexLayout` 'nda gösterilen **SimpleStackPage.xaml** dosyası:

- [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Özelliği değerine ayarlanmış [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection) numaralandırması. Varsayılan, `Row` değeridir. Özellik ayarını `Column` alt neden `FlexLayout` öğeleri tek bir sütunda düzenlenmesini.

    Zaman öğelerinin bir `FlexLayout` bir sütunda düzenlenmiş `FlexLayout` barındırıyor dikey olarak _ana eksende_ ve yatay _eksen arası_.

- [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Özelliği türüdür [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems) ve öğeleri artı ekseninde nasıl hizalanacağını belirtir. `Center` Seçenek yatay olarak ortalanmış her bir öğeyi neden olur.

    Kullanmakta olduğunuz varsa bir `StackLayout` yerine bir `FlexLayout` bu görev için tüm öğeleri atayarak Merkezi `HorizontalOptions` her öğeye özelliğinin `Center`. `HorizontalOptions` Özelliği çocuklar için işe yaramazsa bir `FlexLayout`, ancak tek `AlignItems` özelliği aynı hedefe gerçekleştirir. Gerekiyorsa, kullanabileceğiniz `AlignSelf` geçersiz kılmak için bağlanabilirse özelliği eklenmiş `AlignItems` özelliği ayrı öğeleri için:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Bu değişiklikle bu `Label` sol kenarı konumlandırılmış `FlexLayout` okuma sırası soldan sağa olduğunda.

- [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Özelliği türüdür [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify)ve öğeleri ana eksende nasıl düzenlenir belirtir. `SpaceEvenly` Seçeneği tüm kalan dikey alan tüm öğeler arasında eşit olarak ve ilk öğenin üstünde ve son öğenin altına ayırır.

    Kullanmakta olduğunuz varsa bir `StackLayout`, atamanız gerekir `VerticalOptions` her öğeye özelliğinin `CenterAndExpand` benzer bir etkiye elde etmek için. Ancak `CenterAndExpand` seçeneği her bir öğe arasındaki ilk öğeden önce ve son öğeden sonra iki katı kadar alan ayırmak. Taklit edebilirsiniz `CenterAndExpand` seçeneği `VerticalOptions` ayarlayarak `JustifyContent` özelliği `FlexLayout` için `SpaceAround`.

Bunlar `FlexLayout` özellikler bölümünde daha ayrıntılı olarak ele alınmıştır **[bağlanabilir özellikleri ayrıntılı](#bindable-properties)** aşağıda.

### <a name="using-flexlayout-for-wrapping-items"></a>Öğeleri kaydırma için FlexLayout kullanma

**Fotoğraf kaydırma** sayfasında **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek gösterilmektedir nasıl `FlexLayout` ek satır veya sütun, alt öğelerini kayabilir. XAML dosyası başlatır `FlexLayout` ve iki özelliklerini atar:

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

`Direction` Bu özelliği `FlexLayout` varsayılan ayarını sahiptir, ayarlanmadı `Row`, alt satır düzenlenir ve ana eksende yataydır anlamına gelir.

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Özelliği olan bir numaralandırma türü [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap). Bir satırda sığmayacak kadar çok sayıda öğe varsa, bu özellik ayarı sonraki satıra kaydırmak öğeleri neden olur.

Dikkat `FlexLayout` bir alt öğesi olan bir `ScrollView`. Sayfaya sığdırmak için çok fazla satır yoksa sonra `ScrollView` varsayılan sahip `Orientation` özelliği `Vertical` ve dikey kaydırma sağlar.

`JustifyContent` Özelliği, böylece her öğe aynı boş alan miktarı çevrelenen (yatay eksen) ana eksende kalan alan ayırır.

Arka plan kod dosyasına örnek fotoğraf koleksiyonu erişir ve eklenmektedir `Children` koleksiyonu `FlexLayout`:

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

Aşamalı olarak üstten alta kaydırılan üç platformlarda çalışan program şöyledir:

[![Fotoğraf kaydırma sayfası](flex-layout-images/PhotoWrapping.png "fotoğraf kaydırma sayfası")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Sayfa düzeni FlexLayout ile

Adlı web tasarımında standart bir düzeni olduğunu [ _holy grail_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) çok istenen, ancak genellikle sabit tat ile hayata geçirmek bir düzen biçimde olduğundan. Sayfanın üst kısmındaki bir üstbilgi ve altbilgi altındaki her iki genişletme tam sayfa genişliğine göre, düzeni oluşur. Sayfanın ortasına kaplayan olan ancak genellikle içeren bir sütun halinde menü içerik ve destek bilgileri solundaki ana içerik (bazen adlı bir _kenara_ alan) sağ. [CSS esnek kutu düzeni belirtimi 5.4.1 bölümünü](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) holy grail Düzen içeren bir esnek kutu nasıl gerçekleştiğini açıklar.

**Holy Grail düzeni** sayfasında **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek basit bir uygulama kullanarak bu düzende gösterir `FlexLayout` başka bir programda iç içe geçmiş. Bu sayfa bir telefon dikey modunda için tasarlandığından, sol ve sağ içerik alanının yalnızca 50 piksel genişliğinde şunlardır:

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

Gezinti ve kenara alanları ile işlenen bir `BoxView` sol ve sağ.

İlk `FlexLayout` XAML'de dosya ana dikey eksen ve bir sütunda düzenlenmiş üç alt öğeleri içerir. Bunlar başlık, sayfa ve altbilgi gövdesi. İç içe `FlexLayout` olan bir satır düzenlenmiş üç ana yatay eksen sahiptir.

Üç ekli bağlanabilir özellikler bu programda gösterilmiştir:

- `Order` Ekli bağlanabilirse özelliği ayarlanmış ilk `BoxView`. Bu özellik, bir varsayılan değeri 0 olan bir tamsayıdır. Düzen sırasını değiştirmek için bu özelliği kullanın. Genellikle geliştiriciler biçimlendirme gezinti öğelerini önce görünmesi sayfasının içeriği tercih ve kenara öğeler. Ayarı `Order` ilk özelliği `BoxView` değerine diğer eşdüzey değerinden satır listedeki ilk öğe olarak görünür neden olur. Benzer şekilde, ayarlayarak, bir öğenin son göründüğünü sağlayabilirsiniz `Order` eşdeğerleri büyük bir değere özelliği.

- `Basis` Ekli bağlanabilirse özelliği iki ayarlanmış `BoxView` öğeleri 50 piksel genişliğini vermek için. Bu özellik türünde `FlexBasis`, bir statik özellik türü tanımlayan bir yapı `FlexBasis` adlı `Auto`, varsayılan değerdir. Kullanabileceğiniz `Basis` piksel boyutu veya ne kadar alan gösteren yüzde belirtmek için ana eksende öğesi kaplar. Çağrılır bir _temel_ , sonraki tüm düzeni temelini bir madde boyutu belirtiyor.

- `Grow` Özelliği ayarlanmış iç içe üzerinde `Layout` ve `Label` içeriği temsil eden alt. Bu özellik türünde `float` ve varsayılan değer 0 olarak kullanılır. Pozitif bir değer ayarlandığında, tüm kalan alan ana ekseni boyunca bu öğeyi ve pozitif değerlerle eşdüzey ayrılır `Grow`. Alanı orantılı olarak yıldız belirtiminde gibi biraz değerleri ayrılmış bir `Grid`.

    İlk `Grow` ekli özelliği ayarlanmış iç içe üzerinde `FlexLayout`, belirten bu `FlexLayout` tüm kullanılmayan dikey alan dış içinde kaplar için `FlexLayout`. İkinci `Grow` ekli özelliği ayarlanmış `Label` bu içeriği tüm yatay kullanılmayan iç içinde kaplar olduğunu belirten içeriğe temsil eden `FlexLayout`.

    Ayrıca vardır benzer `Shrink` alt boyutu aştığında kullanabileceğiniz bağlanabilirse özelliği eklenmiş `FlexLayout` ancak kaydırma istenmediği.

### <a name="catalog-items-with-flexlayout"></a>FlexLayout katalog öğeleri

**Katalog öğeleri** sayfasındaki **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek benzer [örnek 1 Bölüm 1. 1 ' CSS esnek düzeni kutusunu belirtiminin](http://www.w3.org/TR/css-flexbox-1/#overview)dışında yatay olarak kaydırılabilir dizi resim ve üç monkeys açıklamalarını görüntüler:

[![Sayfa katalog öğelerini](flex-layout-images/CatalogItems.png "katalog sayfası öğeleri")](flex-layout-images/CatalogItems-Large.png#lightbox)

Her üç monkeys bir `FlexLayout` içinde yer alan bir `Frame` bir açık yükseklik ve genişlik verilen ve olduğu da büyük bir alt `FlexLayout`. Bu XAML dosyasında özelliklerinin çoğu `FlexLayout` alt olduğu örtülü bir stil ancak tüm stillerdeki belirtilir:

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

Örtük stili `Image` iki ekli bağlanabilir özelliklerini ayarlardır `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order` Ayarıyla &ndash;1 nedenler `Image` ilk her iç içe bir görüntülenecek öğesi `FlexLayout` alt koleksiyon içindeki konumuna bakılmaksızın görünümleri. `AlignSelf` Özelliği `Center` neden `Image` içinde ortalanmış için `FlexLayout`. Bu ayarı geçersiz kılar `AlignItems` varsayılan değere sahip özelliği, `Stretch`seçeneğidir; yani, `Label` ve `Button` alt tam genişliğini uzatılır `FlexLayout`.

Her üç `FlexLayout` görünümleri, boş bir `Label` önündeki `Button`, ancak bir `Grow` 1 ayarlayarak. Bu tüm fazla dikey boşluğu bu boş olarak ayrılan anlamına gelir `Label`, hangi etkili bir şekilde iter `Button` altına.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Ayrıntılı bağlanabilir özellikleri

Bazı ortak uygulamaları gördünüz göre `FlexLayout`, özelliklerini `FlexLayout` daha ayrıntılı olarak incelediniz. 
`FlexLayout` üzerinde ayarladığınız altı bağlanabilir özelliklerini tanımlar `FlexLayout` kendisi, kod veya denetim orientatin ve hizalama XAML. (Bu özelliklerden birini [ `Position` ](xref:Xamarin.Forms.FlexLayout.Position), bu makalede ele alınmamıştır.)

Beş ile kullanarak bağlanabilir özelliklerini kalan deneyebilirsiniz **denemeler** sayfasında **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek. Bu sayfa alt öğelerini ekleyip sayesinde bir `FlexLayout` ve beş bağlanabilir özellikleri birleşimlerini ayarlamak için. Tüm alt `FlexLayout` olan `Label` çeşitli renkleri ve boyutları, görünümleri ile `Text` bir sayı konumuna karşılık gelen özelliği `Children` koleksiyonu.

Program başladığında, beş `Picker` görünümleri görüntüler bu beş varsayılan değerlerini `FlexLayout` özellikleri. `FlexLayout` Ekranın üç alt öğeleri içerir:

[![Deneme sayfa: Varsayılan](flex-layout-images/ExperimentDefault.png "varsayılan deneme sayfa -")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Her biri `Label` görünümlere sahip için ayrılan alanı gösterir gri bir arka plan `Label` içinde `FlexLayout`. Arka planını `FlexLayout` kendisi Alice mavi değildir. Sol ve sağda little bir kenar boşluklarını dışında sayfanın tüm alt alanı kaplar.

<a name="direction" />

### <a name="the-direction-property"></a>Direction özelliği

[ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Özelliği türüdür [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection), dört üye olan bir numaralandırma:

- `Column`
- `ColumnReverse` (veya "sütun önleyici" XAML'de)
- `Row`, varsayılan
- `RowReverse` (veya "satır önleyici" XAML'de)

XAML, büyük/küçük harf ya da, CSS göstergeleri aynıdır parantez içinde gösterilen iki ek dizelerini kullanabilirsiniz veya numaralandırma üye adlarının küçük harflerle, büyük harfli kullanarak bu özelliğin değerini belirtebilirsiniz. ("Sütun önleyici" ve "satır önleyici" dizeleri tanımlanan [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) XAML ayrıştırıcısı tarafından kullanılan bir sınıftır.)

Burada **deneme** (soldan sağa) gösteren sayfa `Row` yönü, `Column` yön ve `ColumnReverse` yönü:

[![Deneme sayfa: Yönü](flex-layout-images/ExperimentDirection.png "deneme sayfası - yönü")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

İçin dikkat `Reverse` öğeleri başlatma seçenekleri, sağa veya altına.

<a name="wrap" />

### <a name="the-wrap-property"></a>Kaydırma özelliği

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Özelliği türüdür [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap), üç üyesi olan bir numaralandırma:

- `NoWrap`, varsayılan
- `Wrap`
- `Reverse` (veya "wrap-geriye doğru" XAML)

Bu ekranlarını soldan sağa Göster `NoWrap`, `Wrap` ve `Reverse` 12 çocuklar için seçenekleri:

[![Deneme sayfası: Kaydırma](flex-layout-images/ExperimentWrap.png "deneme sayfası - kaydırma")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Zaman `Wrap` özelliği ayarlanmış `NoWrap` ana eksende (olduğu gibi bu programı) sınırlı değildir ve ana eksende geniş veya tüm alt sığması için yeterince uzun değil `FlexLayout` öğeleri iOS ekran görüntüsü olarak küçültmek dener gösterir. Öğeleri shrinkness denetleyebilirsiniz [ `Shrink` ](#shrink) bağlanabilirse özelliği eklenmiş.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>JustifyContent özelliği

[ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Özelliği türüdür [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), altı olan bir numaralandırma:

- `Start` (veya "esnek XAML'de başlatma"), varsayılan
- `Center`
- `End` (veya "flex-end" XAML'de)
- `SpaceBetween` (veya "alanı XAML'de arasında")
- `SpaceAround` (veya "alanı-XAML'de geçici")
- `SpaceEvenly`

Bu özellik nasıl yatay eksen Bu örnekte ana eksende öğeleri aralıklı belirtir:

[![Deneme sayfası: İçerik Justify](flex-layout-images/ExperimentJustifyContent.png "deneme sayfası - Justify içeriği")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

Tüm üç ekran görüntülerinde `Wrap` özelliği ayarlanmış `Wrap`. `Start` Varsayılan önceki Android ekran görüntüsünde gösterilen. Burada iOS ekran görüntüsü gösterir `Center` seçeneği: tüm öğeler merkezine taşınır. Word ile başlayan üç diğer seçenekleri `Space` öğeleri kapladığı değil ek boşluk ayırın. `SpaceBetween` öğeler arasında eşit olarak alan ayırır; `SpaceAround` yerleştirmelerin eşit her bir öğe boşluk sırada `SpaceEvenly` yerleştirmelerin alan her bir öğe arasındaki ve ilk öğeden önce ve son öğeden sonra satırdaki eşit.

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems özelliği

[ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Özelliği türüdür [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems), dört üye olan bir numaralandırma:

- `Stretch`, varsayılan
- `Center`
- `Start` (veya "flex-XAML'de Başlat")
- `End` (veya "flex-end" XAML'de)

Bu iki özellik biridir (diğer olan [ `AlignContent` ](#align-content)) alt artı ekseninde nasıl hizalandığını gösterir. Her satır içinde alt (önceki ekran görüntüsünde gösterildiği gibi) uzatılmış veya start, center veya her bir öğeyi sonuna aşağıdaki üç ekran görüntülerinde gösterildiği gibi hizalanır:

[![Deneme sayfası: Öğeleri hizalamak](flex-layout-images/ExperimentAlignItems.png "deneme sayfası - öğeleri hizalama")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

İOS ekran görüntüsünde, tüm alt taraflarının hizalanır. Android ekran görüntülerinde en uzun alt temel öğeler dikey olarak ortalanır. UWP ekran görüntüsünde, tüm öğeleri alta hizalanır.

Herhangi bir tek öğe için `AlignItems` ayarını geçersiz kılınmış ile [ `AlignSelf` ](#align-self) bağlanabilirse özelliği eklenmiş.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent özelliği

[ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent) Özelliği türüdür [ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent), yedi üyeleri olan bir numaralandırma:

- `Stretch`, varsayılan
- `Center`
- `Start` (veya "flex-XAML'de Başlat")
- `End` (veya "flex-end" XAML'de)
- `SpaceBetween` (veya "alanı XAML'de arasında")
- `SpaceAround` (veya "alanı-XAML'de geçici")
- `SpaceEvenly`

Gibi `AlignItems`, `AlignContent` özelliği de alt artı ekseninde hizalar ancak tüm satır veya sütun etkiler:

[![Deneme sayfası: İçeriği hizalamak](flex-layout-images/ExperimentAlignContent.png "deneme sayfası - içerik hizalayın")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

İOS screnshot iki satır en üstünde olacak; Android ekran görüntüsünde Merkezi'nde olup olmadıklarını; ve UWP ekran görüntüsünde altındaki olup olmadıklarını. Satırları çeşitli şekillerde aralıklı:

[![Deneme sayfası: İçerik 2 Hizala](flex-layout-images/ExperimentAlignContent2.png "deneme sayfası - Align içerik 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` Yalnızca bir satır veya sütun olduğunda hiçbir etkisi olmaz.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Ayrıntılı bağlanabilirse ekli özellikler

`FlexLayout` beş ekli bağlanabilir özelliklerini tanımlar. Bu özellikleri üzerinde alt ayarlayın `FlexLayout` ve yalnızca bu belirli alt ile ilgilidir.

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf özelliği

[ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) Ekli bağlanabilirse özellik türüdür [ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent), beş üyeleri olan bir numaralandırma:

- `Auto`, varsayılan
- `Stretch`
- `Center`
- `Start` (veya "flex-XAML'de Başlat")
- `End` (veya "flex-end" XAML'de)

Tüm tek tek alt öğesi için `FlexLayout`, geçersiz kılma ayarı bu özellik [ `AlignItems` ](#align-items) özelliği üzerinde ayarlanmış `FlexLayout` kendisi. Varsayılan ayar `Auto` kullanma anlamına gelir `AlignItems` ayarı.

İçin bir `Label` adlı öğe `label` (veya örnek) ayarlayabileceğiniz `AlignSelf` aşağıdakine benzer bir kod özelliğinde:

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

Hiçbir referansı fark `FlexLayout` üst `Label`. XAML'de özelliğini şöyle ayarlayın:

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order özelliğini

[ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty) Özelliği türüdür `int`. Varsayılan değer 0’dır.

`Order` Özelliği sağlar, sırasını değiştirmek, alt öğelerinin `FlexLayout` düzenlenir. Genellikle, alt öğelerini bir `FlexLayout` düzenlenmiş görüntülendikleri aynı sırada `Children` koleksiyonu. Bu sırada ayarlayarak kılabilirsiniz `Order` bağlanabilirse özelliği bir veya daha fazla alt öğe sıfır tamsayı değerine bağlı. `FlexLayout` Öğelerinden biri ayarına göre düzenler `Order` her alt, ancak aynı alt özellikte `Order` ayarı içinde göründükleri sırada düzenlenir `Children` koleksiyonu.

### <a name="the-basis-property"></a>Temel özelliği

[ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty) Ekli bağlanabilirse özelliği, bir alt öğesi için ayrılan alan miktarını gösterir `FlexLayout` ana eksende. Boyutu belirtilen tarafından `Basis` özelliği boyutudur üst ana eksende `FlexLayout`. Bu nedenle, `Basis` alt sütunlarında düzenlenir alt satır veya yüksekliği düzenlenir alt genişliğini belirtir.

`Basis` Özelliği türüdür [ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis), bir yapı. Boyutu ya CİHAZDAN bağımsız birim veya boyutunun yüzdesi olarak belirtilen `FlexLayout`. Varsayılan değer olan `Basis` özelliktir statik özelliği `FlexBasis.Auto`, alt genişlik veya yüksekliğinden kullanılan istenen anlamına gelir.

Kodda, ayarladığınız `Basis` özelliği için bir `Label` adlı `label` şöyle 40 CİHAZDAN bağımsız birimler için:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

İkinci bağımsız değişkeni `FlexBasis` Oluşturucusu adlandırılan `isRelative` ve boyutu göreli olup olmadığını gösterir (`true`) veya mutlak (`false`). Bağımsız değişken varsayılan değerine sahip `false`, aşağıdaki kodu kullanabilirsiniz:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Örtük bir dönüştürme `float` için `FlexBasis` daha basitleştirmek için tanımlanır:

```csharp
FlexLayout.SetBasis(label, 40);
```

Boyutunu % 25 ayarlayabilirsiniz `FlexLayout` üst şuna benzer:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Bu kesir değerini 0 ve 1 arasında olmalıdır.

XAML'de boyutu CİHAZDAN bağımsız birimler için bir sayı kullanabilirsiniz:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Veya, %0 100 aralığında bir yüzde belirtebilirsiniz:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**Temel denemeler** sayfasında **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek deneme olanak tanır `Basis` özelliği. Sayfa beş kaydırılan bir sütunu görüntüler `Label` arka plan ve ön plan renkleri değişen ile öğeleri. İki `Slider` öğeleri belirtmenize izin `Basis` ikinci ve dördüncü değerlerini `Label`:

[![Temel denemeler sayfa](flex-layout-images/BasisExperiment.png "temel denemeler sayfası")](flex-layout-images/BasisExperiment-Large.png#lightbox)

İOS ekran görüntüsü soldaki iki gösterir `Label` yükseklikte CİHAZDAN bağımsız birimler verilen öğeleri. Android ekran bunları toplam yüksekliği kesir olan Yükseklik verilen gösterir `FlexLayout`. Varsa `Basis` alt yüksekliğini olan % 100 oranında ayarlanmışsa `FlexLayout`ve kaydırma sonraki sütuna ve UWP ekran görüntüsü gösterilmektedir gibi bu sütunun tüm yüksekliği kaplar: beş alt bir satırda düzenlenmiş gibi göründüğü , ancak beş sütunlarında gerçekte düzenlenmiş.

### <a name="the-grow-property"></a>Özellik Büyüt

[ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty) Ekli bağlanabilirse özellik türüdür `int`. Varsayılan değer 0'dır ve değeri sıfırdan büyük veya 0 değerine eşit olmalıdır.

`Grow` Özelliği, ne zaman bir rol oynar `Wrap` özelliği ayarlanmış `NoWrap` ve alt satır genişliğini'den küçük toplam genişliği sahip `FlexLayout`, ya da daha kısa bir yükseklik sütununda alt `FlexLayout`. `Grow` Özelliği, alt öğeleri arasında kalan alan apportion nasıl gösterir.

İçinde **büyümesine deneme** sayfası, beş `Label` renkleri değişen öğeleri bir sütun ve iki düzenlenir `Slider` öğelerin ayarlamak izin `Grow` özelliği ikinci ve dördüncü `Label`. Varsayılan sol ucundaki iOS ekran görüntüsü gösterilmektedir `Grow` 0 özellikleri:

[![Büyüt deneme sayfa](flex-layout-images/GrowExperiment.png "Büyüt deneme sayfası")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Herhangi bir alt bir pozitif belirtilmezse `Grow` Android ekran gösterdiği gibi alt tüm kalan alanı, kaplar sonra değer. Bu alanı arasında iki veya daha fazla alt öğe ayrıca ayrılabilir. UWP ekran görüntüsü `Grow` ikinci özelliği `Label` 0,5 olarak ayarlanmış sırada `Grow` dördüncü özelliğinin `Label` dördüncü verir 1.5 olduğu `Label` üç kez kadar kalan alan İkinciolarak,`Label`.

Bu alanı alt görünümü nasıl kullandığını alt belirli türüne bağlıdır. İçin bir `Label`, toplam alanı içindeki metnin yerleştirilebilir `Label` özelliklerini kullanarak `HorizontalTextAlignment` ve `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>Küçültme özelliği

[ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) Ekli bağlanabilirse özellik türüdür `int`. Varsayılan değer 1'dir ve değeri sıfırdan büyük veya 0 değerine eşit olmalıdır.

`Shrink` Özelliği bir rol oynar zaman `Wrap` özelliği ayarlanmış `NoWrap` ve bir satır alt toplam genişliğini genişliğinden büyükse `FlexLayout`, ya da tek bir sütun alt toplam yüksekliği değerinden daha büyük yüksekliğini `FlexLayout`. Normalde `FlexLayout` bu alt boyutlarının constricting tarafından görüntülenir. `Shrink` Özelliği, hangi Çocuklar kendi tam boyutlarda görüntülenmesini içinde öncelik verildiğinden belirtebilirsiniz.

**Küçült deneme** sayfası oluşturan bir `FlexLayout` beş tek bir satır ile `Label` daha fazla alan gerektiren alt `FlexLayout` genişliği. İOS ekran görüntüsü soldaki tüm gösterir `Label` 1 varsayılan değerlerle öğeleri:

[![Küçültme sayfayı denemeler](flex-layout-images/ShrinkExperiment.png "küçültme denemeler sayfası")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android ekran görüntüsü `Shrink` ikinci değeri `Label` 0 ve bu ayarlamak `Label` tam eni görüntülenir. Ayrıca, dördüncü `Label` verilen bir `Shrink` birden fazla değer ve küçültülebilir. UWP ekran görüntüsü, her ikisi de gösterir `Label` öğeleri yaları bir `Shrink` tam boyutlarına görüntülenmesi için bu izin vermek için 0 değeri mümkündür.

Her ikisi de ayarlayabilirsiniz `Grow` ve `Shrink` burada birleşik alt boyutları bazen az veya buna bazen büyük boyutunu durumlarda uyum sağlayacak şekilde değerleri `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>CSS stil FlexLayout ile

Kullanabileceğiniz [CSS stil](~/xamarin-forms/user-interface/styles/css/index.md) birlikte Xamarin.Forms 3.0 ile sunulan özelliğinin `FlexLayout`. **CSS katalog öğeleri** sayfasında **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** örnek yineleme düzenini **katalog öğeleri** sayfasında, ancak bir CSS ile Stil sayfası stilleri birçoğu için:

[![CSS katalog öğeleri sayfası](flex-layout-images/CssCatalogItems.png "CSS katalog öğeleri sayfası")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Özgün **CatalogItemsPage.xaml** dosya sahip beş `Style` tanımlarında kendi `Resources` 15 bölümle `Setter` nesneleri. İçinde **CssCatalogItemsPage.xaml** iki düşürüldü dosya, `Style` yalnızca dört tanımlarla `Setter` nesneleri. Bu stiller CSS stil sayfası Xamarin.Forms CSS stil özelliği şu anda desteklemeyen özellikleri için ek:

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

CSS stil sayfası ilk satırda başvuruluyor `Resources` bölümü:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Ayrıca üç öğelerin her birini iki öğelerinde içerdiğini fark `StyleClass` ayarları:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Bu Seçici içinde başvurmak **CatalogItemsStyles.css** stil sayfası:

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

Birkaç `FlexLayout` ekli bağlanabilir özellikler burada başvuru. İçinde `label.empty` Seçicisi göreceksiniz `flex-grow` boş bir stiller özniteliği `Label` yukarıdaki bazı boş alanı sağlamak üzere `Button`. `image` Seçici içerdiği bir `order` özniteliğini ve bir `align-self` özniteliği, her ikisi de karşılık `FlexLayout` iliştirilmiş bağlanabilir özellikler.

Doğrudan özellikleri ayarlayabilirsiniz gördünüz `FlexLayout` ve alt öğelerinin ekli bağlanabilir özellikleri ayarlayabilirsiniz bir `FlexLayout`. Ya da dolaylı olarak geleneksel XAML tabanlı stillerini veya CSS stilleri kullanarak bu özellikleri ayarlayabilirsiniz. Bildiğiniz ve bu özellikleri anlamak için önemli olan. Bu özellikler ne yapar: `FlexLayout` gerçekten esnek. 

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout Xamarin.University ile

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 düzeni göre esnek [Xamarin Üniversitesi](https://university.xamarin.com/)**

## <a name="related-links"></a>İlgili bağlantılar

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
