---
title: Kaynak sözlükleri
description: XAML kaynakları, paylaşılan ve bir Xamarin.Forms uygulamanın tamamında yeniden kullanılan nesneleridir.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: d305de651f6e770d02ca35f5f8f8ffcf08424e28
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573652"
---
# <a name="resource-dictionaries"></a>Kaynak sözlükleri

_XAML, paylaşılan ve bir Xamarin.Forms uygulamanın tamamında yeniden kullanılan nesne tanımları kaynaklardır._

Bu kaynak nesneleri bir kaynak sözlüğünde depolanır. Bu makalede, oluşturma ve bir kaynak sözlüğü kullanma ve kaynak sözlükleri nasıl açıklar.

## <a name="overview"></a>Genel Bakış

A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) bir Xamarin.Forms uygulaması tarafından kullanılan kaynaklar için bir depodur. Depolanan genel kaynakları bir `ResourceDictionary` dahil [stilleri](~/xamarin-forms/user-interface/styles/index.md), [denetim şablonları](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [veri şablonları](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), renkleri ve dönüştürücü.

XAML, depolanan kaynakları içinde bir `ResourceDictionary` ardından alınabilir ve öğeleri kullanılarak uygulanan `StaticResource` işaretleme uzantısı. C# ' ta kaynakları da tanımlanabilir bir `ResourceDictionary` alınır ve dize tabanlı bir dizin oluşturucu kullanarak öğelerine uygulanır. Ancak, çok az önyüklenebildiği yoktur bir `ResourceDictionary` C# ' ta paylaşılan nesneler yalnızca alanlar ve Özellikler depolanan ve olmadan doğrudan erişim için ilk almak bunları sözlükten.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Oluşturma ve ResourceDictionary Kullanma

Kaynakları tanımlanmış bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) diğer bir deyişle sonra küme için aşağıdakilerden birini `Resources` özellikleri:

- [ `Resources` ](xref:Xamarin.Forms.Application.Resources) Türetilen herhangi bir sınıf özelliği [`Application`](xref:Xamarin.Forms.Application)
- [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Türetilen herhangi bir sınıf özelliği [`VisualElement`](xref:Xamarin.Forms.Application)

Xamarin.Forms program türetildiği yalnızca bir sınıf içeren `Application` ancak genellikle kullanır birçok sınıfların türetilmesi `VisualElement`sayfaları, düzenler ve denetimleri dahil. Bu nesnelerden herhangi birini içerebilir, `Resources` özelliğini bir `ResourceDictionary`. Belirli bir yerleştirileceği yeri seçme `ResourceDictionary` kaynakların nerede kullanılabilir etkiler:

- Kaynakları bir `ResourceDictionary` , bağlı bir görünüme gibi `Button` veya `Label` çok kullanışlı değildir yalnızca bu belirli nesnenin uygulanabilir.
- Kaynakları bir `ResourceDictionary` gibi bir düzene bağlı `StackLayout` veya `Grid` düzenini ve bu düzen tüm alt öğeleri için uygulanabilir.
- Kaynakları bir `ResourceDictionary` tanımlanan sayfa düzeyi sayfasına ve öğenin tüm alt öğelerine uygulanabilir.
- Kaynakları bir `ResourceDictionary` tanımlanan uygulamayı uygulama genelinde düzeyinde uygulanabilir.

Aşağıdaki XAML bir uygulama düzeyinde tanımlanan kaynaklar gösterilmektedir `ResourceDictionary` içinde **App.xaml** standart Xamarin.Forms programın bir parçası olarak oluşturulan dosya:

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

Bu `ResourceDictionary` üç tanımlar [ `Color` ](xref:Xamarin.Forms.Color) kaynakları ve [ `Style` ](xref:Xamarin.Forms.Style) kaynak. Hakkında daha fazla bilgi için `App` sınıfı [uygulama sınıfına](~/xamarin-forms/app-fundamentals/application-class.md).

Xamarin.Forms 3. 0 ' açık'den itibaren `ResourceDictionary` etiketler gerekli değildir. `ResourceDictionary` Nesne otomatik olarak oluşturulur ve kaynaklar arasında doğrudan ekleyebilirsiniz `Resources` özellik öğesi etiketleri:

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

Her kaynak kullanılarak belirtilen bir anahtara sahip `x:Key` , sözlük anahtarı dönüşen özniteliği `ResourceDictionary`. Bir kaynaktan almak için kullanılan anahtarı `ResourceDictionary` tarafından [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) ek kaynaklar içinde tanımlanan gösteren aşağıdaki XAML kod örneğinde gösterildiği gibi işaretleme uzantısı `StackLayout`:

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

İlk [ `Label` ](xref:Xamarin.Forms.Label) örneği alır ve tüketen `LabelPageHeadingStyle` kaynak uygulama düzeyinde tanımlanan `ResourceDictionary`, ikinci `Label` almaya ve kullananörnek`LabelNormalStyle`kaynak denetimi düzeyinde tanımlanan `ResourceDictionary`. Benzer şekilde, [ `Button` ](xref:Xamarin.Forms.Button) örneği alır ve tüketen `NormalTextColor` kaynak uygulama düzeyinde tanımlanan `ResourceDictionary`ve `MediumBoldText` kaynak denetimi düzeyinde tanımlanan `ResourceDictionary`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

[![](resource-dictionaries-images/screenshots-sml.png "ResourceDictionary kaynak tüketmeye")](resource-dictionaries-images/screenshots.png#lightbox "ResourceDictionary kaynakları kullanma")

> [!NOTE]
> Tek bir sayfaya özgü kaynakları kaynakları ardından yerine uygulama başlangıcında bir sayfa tarafından istendiğinde ayrıştırılacak bir uygulama düzeyinde kaynak sözlüğünde şekilde eklenmemelidir. Daha fazla bilgi için [uygulama kaynak sözlüğü boyutunu küçültmek](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Kaynakları geçersiz kılma

Zaman `ResourceDictionary` kaynakları paylaşan `x:Key` öznitelik değerleri, alt görünüm hiyerarşi içinde tanımlanan kaynaklara önceliklidir yukarı yüksek tanımlanmış. Örneğin, ayarlamak `PageBackgroundColor` kaynağa `Blue` uygulamayı düzeyi bir sayfa düzeyi tarafından geçersiz kılınır `PageBackgroundColor` kaynak kümesine `Yellow`. Benzer şekilde, bir sayfa düzeyi `PageBackgroundColor` Kaynak Denetim düzeyine göre yazılacak `PageBackgroundColor` kaynak. Bu öncelik aşağıdaki XAML kodu örnekte gösterilmiştir:

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

Özgün `PageBackgroundColor` ve `NormalTextColor` örnekleri, uygulama düzeyinde tanımlanan tarafından geçersiz kılınır `PageBackgroundColor` ve `NormalTextColor` sayfa düzeyinde tanımlanan örnekleri. Bu nedenle, arka plan rengini Mavi olur ve aşağıdaki ekran görüntülerinde gösterildiği gibi metin sayfasında sarı olur:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "ResourceDictionary geçersiz kılan")](resource-dictionaries-images/overridding-screenshots.png#lightbox "ResourceDictionary kaynakları geçersiz kılma")

Ancak, unutmayın arka plan çubuğuna [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) hala sarı çünkü [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) özelliği değeri olarak ayarlanır `PageBackgroundColor` uygulamada tanımlanan kaynak düzey `ResourceDictionary`.

Dikkat etmeniz gereken başka bir yol `ResourceDictionary` öncelik: karşılaştığında, XAML ayrıştırıcı bir `StaticResource`yukarı aracılığıyla görsel ağacın tarafından seyahat için eşleşen bir anahtar arar, bulduğu ilk eşleşme kullanılıyor. Bu arama sayfasında sona erer ve anahtarı hala bulundu taşınmadığından, XAML ayrıştırıcı arar `ResourceDictionary` bağlı `App` nesne. Anahtar hala bulunamazsa, bir özel durum oluşturulur.

## <a name="stand-alone-resource-dictionaries"></a>Tek başına kaynak sözlükleri

Öğesinden türetilen bir sınıf `ResourceDictionary` de tek başına ayrı bir dosyada olabilir. (Daha kesin bir şekilde türetilen bir sınıf `ResourceDictionary` genellikle gerektiren bir _çifti_ dosyalarının kaynakları bir XAML dosyası ancak bir arka plan kod dosyası ile tanımlandığından bir `InitializeComponent` çağrısı gerekli ayrıca.) Sonuç dosyası, ardından uygulamalar arasında paylaşılabilir.

Böyle bir dosya oluşturmak için yeni bir ekleme **içerik görünümü** veya **içerik sayfası** proje öğesine (ama bir **içerik görünümü** veya **içerik sayfası** ile yalnızca bir C# dosyası). XAML dosyası hem C# dosyası içinde temel sınıftan adını değiştirmek `ContentView` veya `ContentPage` için `ResourceDictionary`. XAML dosyasında en üst düzey öğe temel sınıf adıdır.

XAML aşağıdaki örnekte gösterildiği bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) adlı `MyResourceDictionary`:

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

Bu `ResourceDictionary` içeren bir nesne türü olan tek bir kaynak `DataTemplate`.

Örneği oluşturabilir `MyResourceDictionary` çifti arasında koyarak `Resources` özellik öğesi etiketleri, örneğin bir `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Örneği `MyResourceDictionary` ayarlanır `Resources` özelliği `ContentPage` nesne.

Ancak, bu yaklaşım bazı sınırlamalara sahiptir: `Resources` özelliği `ContentPage` yalnızca bir başvuruda `ResourceDictionary`. Çoğu durumda, diğer dahil olmak üzere istediğiniz `ResourceDictionary` örnekleri ve belki de diğer kaynağı da.

Bu görev, birleştirilmiş kaynak sözlükleri gerektirir.

## <a name="merged-resource-dictionaries"></a>Birleştirilmiş Kaynak Sözlükleri

Birleştirilmiş kaynak sözlükleri birleştiren bir veya daha fazla `ResourceDictionary` başka bir örneği `ResourceDictionary`. Bir XAML dosyasında ayarlayarak bunu yapabilirsiniz [ `MergedDictionaries` ](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) uygulama, sayfa veya denetim düzeyi birleştirilmiş kaynak sözlükleri bir veya daha fazla özelliğini `ResourceDictionary`.

> [!IMPORTANT]
> `ResourceDictionary` Ayrıca tanımlayan bir [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) özelliği. Bu özelliği kullanmayın; Bunu Xamarin.Forms 3.0 itibarıyla kullanım dışıdır.

Örneği `MyResourceDictionary` her uygulama, sayfa veya denetim düzeyi birleştirilebilir `ResourceDictionary`. Aşağıdaki XAML kod örneği, bir sayfa düzeyi birleştirilen gösterir `ResourceDictionary` kullanarak `MergedDictionaries` özelliği:

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

Bu biçimlendirme yalnızca bir örneği gösterilmektedir `MyResourceDictionary` eklenmesini `ResourceDictionary` ancak diğer de başvurabilirsiniz `ResourceDictionary` içinde örnekler `MergedDictionary` özellik öğesi etiketleri ve diğer kaynaklar bu etiketleri dışında:

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

Yalnızca bir `MergedDictionaries` konusundaki bir `ResourceDictionary`, ancak çok koyabilirsiniz `ResourceDictionary` istediğiniz burada örnekler.

Birleştirdiğimde [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) kaynakları paylaşan aynı `x:Key` Xamarin.Forms, öznitelik değerleri kullanan aşağıdaki kaynak öncelik:

1. Kaynak sözlüğü için yerel kaynaklar.
1. Birleştirilmiş kaynak sözlüğünde yer alan kaynakları kullanım dışı aracılığıyla [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) özelliği.
1. Aracılığıyla birleştirilmiş kaynak sözlükleri yer alan kaynakları `MergedDictionaries` içinde listelendikleri ters sırada koleksiyon `MergedDictionaries` özelliği.

> [!NOTE]
> Kaynak sözlükleri arama olabilir işlem bakımından yoğun bir görevi birden çok uygulama içeriyorsa, büyük kaynak sözlükleri. Bu nedenle, gereksiz arama önlemek için her bir uygulama sayfasını yalnızca sayfanın için uygun olan kaynak sözlükleri kullanır emin olmalısınız.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Xamarin.Forms 3.0 sözlükte birleştirme

Birleştirme işleminin Xamarin.Forms 3.0 ile başlayan [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) biraz daha kolay ve daha esnek örnekleri haline gelmiştir. `MergedDictionaries` Özellik öğesi etiketleri gerekli artık. Bunun yerine, kaynak sözlüğüne başka eklediğiniz `ResourceDictionary` yeni etiket [ `Source` ](xref:Xamarin.Forms.ResourceDictionary.Source) kaynaklar ile XAML dosyasının FileName özelliği:

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

Xamarin.Forms 3.0 otomatik olarak oluşturur çünkü `ResourceDictionary`, bu iki dış `ResourceDictionary` etiketleri gerekli artık:

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

Bu yeni sözdizimi denetimi yapar _değil_ örneği `MyResourceDictionary` sınıfı. Bunun yerine, XAML dosyası başvuruyor. Bu nedenle arka plan kod dosyası (**MyResourceDictionary.xaml.cs**) artık gerekli değildir. Ayrıca kaldırabilirsiniz `x:Class` kök etiketi özniteliğinden **MyResourceDictionary.xaml** dosya.

## <a name="summary"></a>Özet

Bu makale oluşturma ve kullanma konusunda açıklandığı bir [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)ve kaynak sözlükleri birleştirme. A `ResourceDictionary` tek bir konumda tanımlanmış ve bir Xamarin.Forms uygulamanın tamamında yeniden kullanılan kaynaklar sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [Kaynak sözlükleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Stiller](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
