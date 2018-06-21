---
title: Kaynak sözlükleri
description: XAML, paylaşılan ve bir Xamarin.Forms uygulaması yeniden kullanılan nesneleri kaynaklardır.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c2e6a5624baba251061bcd324fcb849e3d95ebfd
ms.sourcegitcommit: c2d1249cb67b877ee0d9cb8d095ec66fd51d8c31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291041"
---
# <a name="resource-dictionaries"></a>Kaynak sözlükleri

_XAML, paylaşılan ve bir Xamarin.Forms uygulaması yeniden kullanılan nesnelerin tanımları kaynaklardır._

Bu kaynak nesneleri kaynak sözlükte depolanır. Bu makalede nasıl oluşturulacağı ve bir kaynak sözlüğü tüketebilir ve kaynak sözlüklerindeki birleştirme nasıl açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) bir Xamarin.Forms uygulaması tarafından kullanılan kaynakları için bir depodur. İçinde depolanan genel kaynakları bir `ResourceDictionary` dahil [stilleri](~/xamarin-forms/user-interface/styles/index.md), [denetim şablonları](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [veri şablonları](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), renklerini ve dönüştürücüleri.

XAML'de depolanmış kaynakları bir `ResourceDictionary` sonra alınabilir ve kullanarak öğelerine uygulanan `StaticResource` biçimlendirme uzantısı. C# ' ta kaynakları da içinde tanımlanabilir bir `ResourceDictionary` alınır ve dize tabanlı bir dizin oluşturucu kullanarak öğelerine uygulanır. Ancak, çok az kullanmanın avantajı yoktur bir `ResourceDictionary` C# ' ta paylaşılan nesneler yalnızca alanları veya özellikleri depolanır ve doğrudan olmadan erişilebilir olması için ilk almak bunları sözlükten.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Oluşturma ve ResourceDictionary Kullanma

Kaynakları tanımlanmış bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) diğer bir deyişle sonra kümesine aşağıdakilerden birini `Resources` özellikleri:

- [ `Resources` ](xref:Xamarin.Forms.Application.Resources) Türetilen herhangi bir sınıf özelliği [`Application`](xref:Xamarin.Forms.Application)
- [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Türetilen herhangi bir sınıf özelliği [`VisualElement`](xref:Xamarin.Forms.Application)

Öğesinden türetilen yalnızca bir sınıf bir Xamarin.Forms programı içeren `Application` ancak genellikle kullanır öğesinden türetilen birçok sınıfları `VisualElement`sayfalar, Düzen ve denetimleri dahil olmak üzere. Bu nesnelerin herhangi biri olabilir, `Resources` özellik kümesine bir `ResourceDictionary`. Belirli bir nereye koyacağınızı seçmek `ResourceDictionary` kaynakların nerede kullanılabilir etkiler:

- Kaynakları bir `ResourceDictionary` , bağlı bir görünüme gibi `Button` veya `Label` bu çok kullanışlı değildir, belirli bir nesneye yalnızca uygulanabilir.
- Kaynakları bir `ResourceDictionary` bir düzen gibi bağlı `StackLayout` veya `Grid` düzenini ve tüm alt öğeleri düzenini uygulanabilir.
- Kaynakları bir `ResourceDictionary` tanımlanan sayfa ve tüm alt düzey sayfanın uygulanabilir.
- Kaynakları bir `ResourceDictionary` tanımlanan uygulamayı düzeyi uygulama genelinde uygulanabilir.

Bir uygulama düzeyinde tanımlanan kaynaklara aşağıdaki XAML gösterir `ResourceDictionary` içinde **App.xaml** standart Xamarin.Forms programın bir parçası olarak oluşturulan dosya:

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Bu `ResourceDictionary` üç tanımlar [ `Color` ](xref:Xamarin.Forms.Color) kaynakları ve [ `Style` ](xref:Xamarin.Forms.Style) kaynak. Hakkında daha fazla bilgi için `App` sınıfı için bkz: [uygulama sınıfı](~/xamarin-forms/app-fundamentals/application-class.md).

Xamarin.Forms 3. 0 ' açık'den itibaren `ResourceDictionary` etiketleri gerekli değildir. `ResourceDictionary` Nesnesi otomatik olarak oluşturulur ve kaynaklar arasında doğrudan ekleyebilirsiniz `Resources` özellik öğesi etiketleri:

```xaml
<Application ...>
    <Application.Resources>
        <Color x:Key="PageBackgroundColor">Yellow</Color>
        <Color x:Key="HeadingTextColor">Black</Color>
        <Color x:Key="NormalTextColor">Blue</Color>
        <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
            <Setter Property="FontAttributes" Value="Bold" />
            <Setter Property="HorizontalOptions" Value="Center" />
            <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
        </Style>
    </Application.Resources>
</Application>
```

Her kaynak kullanılarak belirtilen bir anahtara sahip `x:Key` sözlük anahtarında hale özniteliği `ResourceDictionary`. Anahtar bir kaynaktan almak için kullanılan `ResourceDictionary` tarafından [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) içinde tanımlanan ek kaynakları gösteren aşağıdaki XAML kod örneğinde gösterildiği gibi biçimlendirme uzantısı `StackLayout`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

İlk [ `Label` ](xref:Xamarin.Forms.Label) örneği alır ve tüketir `LabelPageHeadingStyle` uygulama düzeyinde tanımlanan kaynak `ResourceDictionary`, ikinci ile `Label` alma ve kullanmaörneği`LabelNormalStyle`kaynağı denetim düzeyi tanımlı `ResourceDictionary`. Benzer şekilde, [ `Button` ](xref:Xamarin.Forms.Button) örneği alır ve tüketir `NormalTextColor` uygulama düzeyinde tanımlanan kaynak `ResourceDictionary`ve `MediumBoldText` kaynağı denetim düzeyi tanımlı `ResourceDictionary`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

[![](resource-dictionaries-images/screenshots-sml.png "ResourceDictionary kaynakları tüketen")](resource-dictionaries-images/screenshots.png#lightbox "ResourceDictionary kaynakları kullanma")

> [!NOTE]
> Tek bir sayfaya belirli kaynaklar kaynakları sonra yerine uygulama başlangıcında bir sayfa tarafından istendiğinde ayrıştırılır bir uygulama düzeyi kaynak sözlük, bu nedenle dahil döndürmemelidir. Daha fazla bilgi için bkz: [uygulama kaynak sözlük boyutunu küçültmek](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Kaynakları geçersiz kılma

Zaman `ResourceDictionary` kaynakları paylaşmak `x:Key` öznitelik değerleri daha düşük görünüm hiyerarşi içinde tanımlanan kaynaklara önceliklidir yukarı yüksek tanımlanan. Örneğin, ayarlama `PageBackgroundColor` kaynağa `Blue` uygulamayı düzeyi sayfa düzeyi tarafından geçersiz kılınacaktır `PageBackgroundColor` kaynak kümesine `Yellow`. Benzer şekilde, bir sayfa düzeyi `PageBackgroundColor` Kaynak Denetim düzeyine göre geçersiz `PageBackgroundColor` kaynak. Bu öncelik aşağıdaki XAML kod örneği tarafından gösterilmiştir:

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

Özgün `PageBackgroundColor` ve `NormalTextColor` örnekleri, uygulama düzeyinde tanımlanan tarafından geçersiz kılınır `PageBackgroundColor` ve `NormalTextColor` sayfa düzeyinde tanımlanan örnekleri. Bu nedenle, sayfa arka plan rengi mavi olur ve aşağıdaki ekran görüntülerinde gösterildiği gibi metin sayfasında sarı olur:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "ResourceDictionary kaynakları geçersiz kılma")](resource-dictionaries-images/overridding-screenshots.png#lightbox "ResourceDictionary kaynakları geçersiz kılma")

Ancak, dikkat edin arka plan çubuğunu [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) hala sarı çünkü [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) özelliği değerine ayarlanmış `PageBackgroundColor` kaynağı uygulamada tanımlı düzey `ResourceDictionary`.

Dikkat etmeniz gereken başka bir yol `ResourceDictionary` öncelik: zaman XAML ayrıştırıcısı karşılaştığı bir `StaticResource`yukarı aracılığıyla görsel ağacın tarafından seyahat için eşleşen bir anahtarı arar, bulduğu ilk eşleşmeye kullanma. Bu arama sayfanın sona erer ve anahtar hala bulunan kurmadı XAML ayrıştırıcısı arar `ResourceDictionary` bağlı `App` nesnesi. Anahtar yine bulunamazsa, bir özel durum oluşturulur.

## <a name="stand-alone-resource-dictionaries"></a>Tek başına kaynak sözlükleri

Öğesinden türetilmiş bir sınıf `ResourceDictionary` ayrı bir tek başına dosyasında da olabilir. (Daha hassas bir şekilde türetilmiş bir sınıf `ResourceDictionary` genellikle gerektirir bir _çifti_ dosyaların kaynakları XAML dosyası ancak bir arka plan kodu dosyayla tanımlandığından bir `InitializeComponent` çağrıdır de gerekli.) Sonuç dosyası ardından uygulamalar arasında paylaşılabilir.

Böyle bir dosya oluşturmak için yeni bir ekleme **içerik görünümü** veya **içerik sayfasını** projeye öğe (ama bir **içerik görünümü** veya **içerik sayfasını** ile yalnızca bir C# dosyası). XAML dosyası hem C# dosyasına de, temel sınıfından adını değiştirmek `ContentView` veya `ContentPage` için `ResourceDictionary`. XAML dosyasında en üst düzey öğe temel sınıf adıdır.

Aşağıdaki XAML örnekte gösterildiği bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) adlı `MyResourceDictionary`:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

Bu `ResourceDictionary` türünde bir nesne olan tek bir kaynak içeriyor `DataTemplate`.

Örneğini oluşturabilirsiniz `MyResourceDictionary` çifti arasında koyarak `Resources` özellik öğesi etiketleri, örneğin, içinde bir `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Örneği `MyResourceDictionary` ayarlanır `Resources` özelliği `ContentPage` nesnesi.

Ancak, bu yaklaşımın bazı sınırlamalar vardır: `Resources` özelliği `ContentPage` bu tek başvuran `ResourceDictionary`. Çoğu durumda, istediğiniz diğer dahil olmak üzere seçeneği `ResourceDictionary` örnekleri ve belki de diğer kaynaklar da.

Bu görev birleştirilmiş kaynak sözlüklerindeki gerektirir.

## <a name="merged-resource-dictionaries"></a>Birleştirilmiş Kaynak Sözlükleri

Birleştirilmiş kaynak sözlüklerindeki birleştiren bir veya daha fazla `ResourceDictionary` başka bir örneği `ResourceDictionary`. XAML dosyası içinde ayarlayarak bunu yapabilirsiniz [ `MergedDictionaries` ](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) uygulama, sayfa ya da denetim düzeyi birleştirilmiş bir veya daha fazla kaynak sözlüklerindeki özelliğine `ResourceDictionary`.

> [!IMPORTANT]
> `ResourceDictionary` Ayrıca tanımlayan bir [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) özelliği. Bu özelliği kullanmayın; Xamarin.Forms 3.0 sürümünden itibaren kaldırıldı.

Örneği ile `MyResourceDictionary` herhangi bir uygulama, sayfa veya denetim düzeyi birleştirilmiş `ResourceDictionary`. Aşağıdaki XAML kod örneği, bir sayfa düzeye birleştirilen gösterir `ResourceDictionary` kullanarak `MergedDictionaries` özelliği:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Bu biçimlendirme yalnızca bir örneği gösterir `MyResourceDictionary` eklenmesini `ResourceDictionary` ancak aynı zamanda diğer başvurabilir `ResourceDictionary` içinde örnekleri `MergedDictionary` özellik öğesi etiketleri ve diğer kaynaklar bu etiketlerin dışında:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary.MergedDictionaries>

                <!-- Add more resource dictionaries here -->

                <local:MyResourceDictionary />

                <!-- Add more resource dictionaries here -->

            </ResourceDictionary.MergedDictionaries>

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Yalnızca bir olabilir `MergedDictionaries` bölümüne bir `ResourceDictionary`, ancak sayıda koyabilirsiniz `ResourceDictionary` istediğiniz var örnekleri.

Birleştirilmiş zaman [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) kaynakları paylaşmak aynı `x:Key` , aşağıdaki kaynak öncelik Xamarin.Forms öznitelik değerleri kullanır:

1. Kaynak sözlüğe yerel kaynaklar.
1. Birleştirilmiş kaynak sözlükte bulunan kaynaklar kullanım dışı aracılığıyla [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) özelliği.
1. Aracılığıyla birleştirilen kaynak sözlüklerindeki içerdiği kaynaklar `MergedDictionaries` koleksiyon, içinde listelenmiştir ters sırada `MergedDictionaries` özelliği.

> [!NOTE]
> Kaynak sözlüklerindeki arama olabilir bir pkı'ya yoğun görevi birden çok, bir uygulama içeriyorsa, büyük kaynak sözlük. Bu nedenle, gereksiz arama önlemek için her bir uygulama sayfasını yalnızca sayfasına uygun kaynak sözlüklerindeki kullandığından emin olun.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Xamarin.Forms 3.0 sözlükte birleştirme

Birleştirme işlemi Xamarin.Forms 3.0 ile başlayan [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) örnekleri duruma biraz daha kolay ve daha esnektir. `MergedDictionaries` Özellik öğesi etiketleri gerekli artık. Bunun yerine, kaynak sözlüğe başka eklediğiniz `ResourceDictionary` yeni etiket [ `Source` ](xref:Xamarin.Forms.ResourceDictionary.Source) kaynaklar ile XAML dosyasının dosya adı olarak ayarlanan özelliği:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary Source="MyResourceDictionary.xaml" />

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Xamarin.Forms 3.0 otomatik olarak başlatır çünkü `ResourceDictionary`, bu iki dış `ResourceDictionary` etiketleri gerekli artık:

```xaml
<ContentPage ...>
    <ContentPage.Resources>

        <!-- Add more resources here -->

        <ResourceDictionary Source="MyResourceDictionary.xaml" />

        <!-- Add more resources here -->

    </ContentPage.Resources>
    ...
</ContentPage>
```

Bu yeni söz diziminin mu _değil_ örneği `MyResourceDictionary` sınıfı. Bunun yerine, XAML dosyasına başvurur. Arka plan kod dosyasına nedenden dolayı (**MyResourceDictionary.xaml.cs**) artık gerekli değildir. Ayrıca kaldırabilirsiniz `x:Class` kök etiket özniteliğinden **MyResourceDictionary.xaml** dosya.

## <a name="summary"></a>Özet

Bu makalede açıklanan oluşturma ve kullanma hakkında bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ve kaynak sözlüklerindeki birleştirme. A `ResourceDictionary` tek bir konumda tanımlanmış ve bir Xamarin.Forms uygulaması yeniden kullanılabilir kaynaklar sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [Kaynak sözlüklerindeki (örnek)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
