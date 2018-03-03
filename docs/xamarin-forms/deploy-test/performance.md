---
title: Xamarin.Forms performans
description: "Xamarin.Forms uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir. Bu makalede ve bu teknikler anlatılmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b7b5093f9a564c0711ddc8a711f9b609d44e7dad
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-performance"></a>Xamarin.Forms performans

_Xamarin.Forms uygulamaların performansını artırmak için birçok tekniği vardır. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir. Bu makalede ve bu teknikler anlatılmaktadır._

[ ![](performance-images/evolve-jason-perf-sml.png "Xamarin.Forms ile uygulama performansı en iyi duruma getirme")](https://evolve.xamarin.com/session/56e205b0bad314273ca4d817)

[2016 gelişmesi: Xamarin.Forms ile uygulama performansı en iyi duruma getirme](https://evolve.xamarin.com/session/56e205b0bad314273ca4d817)

## <a name="overview"></a>Genel Bakış

Zayıf uygulama performans kendisini birçok yolla gösterir. Bir uygulama yapabilirsiniz yanıt vermeyen gibi görünebilir, yavaş kaydırma neden olabilir ve pil ömrünün azaltabilir. Ancak, performansı en iyi duruma getirme daha fazlasını verimli kod uygulama içerir. Uygulama performansı kullanıcı deneyimi de dikkate alınmalıdır. Örneğin, diğer etkinlikler gerçekleştirmeyi kullanıcı engellenmeden işlemlerini yürütmek sağlayarak kullanıcı deneyimini geliştirmek için yardımcı olabilir.

Performans ve algılanan, bir Xamarin.Forms uygulamanın performansını artırmak için teknikler mevcuttur. Bunlara aşağıdakiler dahildir:

- [XAML derleyici etkinleştir](#xamlc)
- [Doğru bir düzen seçin](#correctlayout)
- [Düzen sıkıştırmayı etkinleştir](#layoutcompression)
- [Hızlı Oluşturucu kullanın](#fastrenderers)
- [Gereksiz bağlamaları azaltın](#databinding)
- [Düzen performansı en iyi duruma getirme](#optimizelayout)
- [ListView performansı en iyi duruma getirme](#optimizelistview)
- [Görüntü kaynakları en iyi duruma getirme](#optimizeimages)
- [Görsel ağaç boyutunu azaltma](#visualtree)
- [Uygulama kaynak sözlük boyutunu azaltma](#resourcedictionary)
- [Özel oluşturucu düzeni kullanın](#rendererpattern)

> [!NOTE]
>  Bu makalede okumadan önce ilk okumalısınız [platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md), bellek kullanımı ve Xamarin platformu kullanılarak oluşturulan uygulamaların performansını artırmak için platform olmayan belirli teknikler açıklanır.

<a name="xamlc" />

## <a name="enable-the-xaml-compiler"></a>XAML derleyici etkinleştir

XAML isteğe bağlı olarak ara dile (IL) XAML derleyici (XAMLC) ile doğrudan derlenebilir. XAMLC bir avantajları sunar:

- Derleme zamanı hatalarını kullanıcı bildiren XAML denetimi gerçekleştirir.
- XAML öğeleri için yük ve örnek oluşturma saat bazıları kaldırır.
- Artık .xaml dosyaları ekleyerek son derlemeyi dosya boyutunu azaltmak için yardımcı olur.

XAMLC geriye dönük uyumluluğu sağlamak için varsayılan olarak devre dışıdır. Ancak, bu hem derleme ve sınıf düzeyinde etkinleştirilebilir. Daha fazla bilgi için bkz: [derleme XAML](~/xamarin-forms/xaml/xamlc.md).

<a name="correctlayout" />

## <a name="choose-the-correct-layout"></a>Doğru bir düzen seçin

Birden fazla alt görüntüleme yeteneğine sahip olan, ancak yalnızca tek bir alt olan bir düzen kayıp. Örneğin, aşağıdaki örnekte gösterildiği kod bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) tek bir alt ile:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <StackLayout>
            <Image Source="waterfront.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Bu kayıp ve [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) öğesi kaldırılması gerekir, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <Image Source="waterfront.jpg" />
    </ContentPage.Content>
</ContentPage>
```

Ayrıca, bu sonucu olarak gerçekleştirilen gereksiz Düzen hesaplamalarda başka düzenleri birleşimlerini kullanarak belirli bir düzen görünümünü yeniden denemesi yok. Örneğin, yeniden doğrulamaya çalışma bir [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) bir bileşimini kullanarak Düzen [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) örnekleri. Aşağıdaki kod örneğinde bu hatalı yöntem örneği gösterilmektedir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" />
                <Entry Placeholder="Enter your name" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Age:" />
                <Entry Placeholder="Enter your age" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Occupation:" />
                <Entry Placeholder="Enter your occupation" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Address:" />
                <Entry Placeholder="Enter your address" />
            </StackLayout>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Gereksiz Düzen hesaplamaları gerçekleştirilir kayıp olmasıdır. Bunun yerine, istediğiniz düzene daha iyi kullanarak elde edilebilir bir [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="100" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
            </Grid.RowDefinitions>
            <Label Text="Name:" />
            <Entry Grid.Column="1" Placeholder="Enter your name" />
            <Label Grid.Row="1" Text="Age:" />
            <Entry Grid.Row="1" Grid.Column="1" Placeholder="Enter your age" />
            <Label Grid.Row="2" Text="Occupation:" />
            <Entry Grid.Row="2" Grid.Column="1" Placeholder="Enter your occupation" />
            <Label Grid.Row="3" Text="Address:" />
            <Entry Grid.Row="3" Grid.Column="1" Placeholder="Enter your address" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

<a name="layoutcompression" />

## <a name="enable-layout-compression"></a>Düzen sıkıştırmayı etkinleştir

Düzenini sıkıştırma belirtilen düzenleri sayfa işleme performansı girişimi visual ağacından kaldırır. Bu teslim performans avantajı bir sayfa, kullanılan işletim sistemi sürümü ve uygulamanın çalıştığı aygıt karmaşıklığına bağlı olarak değişir. Ancak, büyük performans artışı eski cihazlarda görülür. Daha fazla bilgi için bkz: [düzenini sıkıştırma](~/xamarin-forms/user-interface/layouts/layout-compression.md).

<a name="fastrenderers" />

## <a name="use-fast-renderers"></a>Hızlı Oluşturucu kullanın

Hızlı Oluşturucu, sonuçta elde edilen yerel denetim hiyerarşisi düzleştirme tarafından Enflasyon ve Android Xamarin.Forms denetimlere işleme maliyetlerini azaltabilirsiniz. Bu daha fazla performans sonuçları en az karmaşık bir görsel ağaç ve bellek kullanımını daha az etkinleştirir, daha az nesne oluşturarak artırır. Daha fazla bilgi için bkz: [hızlı Oluşturucu](~/xamarin-forms/internals/fast-renderers.md).

<a name="databinding" />

## <a name="reduce-unnecessary-bindings"></a>Gereksiz bağlamaları azaltın

Bağlamaları kolayca statik olarak ayarlanabilir içerik için kullanmayın. Bağlamaları maliyet etkin olmadığından, bağlanması için gereken değil Veri bağlamada avantajlı yoktur. Örneğin, ayarlama `Button.Text = "Accept"` bağlama daha az yüke sahip [ `Button.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/) bir ViewModel için `string` özelliği "Kabul et" değerine sahip.

<a name="optimizelayout" />

## <a name="optimize-layout-performance"></a>Düzen performansı en iyi duruma getirme

Xamarin.Forms 2 Düzen güncelleştirmelerini etkiler bir en iyi duruma getirilmiş yerleşim altyapısı sunmuştur. Olası düzeni arasında en iyi performansı elde etmek için aşağıdaki yönergeleri izleyin:

- Belirterek düzeni hiyerarşileri derinliğini azaltın [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) özellik değerleri, daha az kaydırma görünümlerle düzenleri oluşturulmasına izin verme. Daha fazla bilgi için bkz: [kenar boşlukları ve doldurma](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- Kullanırken bir [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), olabildiğince az sayıda satır ve sütunların mümkün olduğunca kümesine sağlamak deneyin [ `Auto` ](https://developer.xamarin.com/api/property/Xamarin.Forms.GridLength.Auto/) boyutu. Her otomatik ölçekli satır veya sütun ek Düzen hesaplamalar yerleşim altyapısı neden olur. Bunun yerine, sabit boyutlu satırları ve sütunları mümkünse kullanın. Alternatif olarak, kümesinin satırları ve sütunları alanıyla orantılı miktarını kaplar [ `GridUnitType.Star` ](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) numaralandırma değeri, sağlanan üst ağaç bu düzeni yönergeleri izler.
- Ayarlamazsanız [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) ve [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) bir düzen özelliklerini gerekmiyorsa. Varsayılan değerleri [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) ve [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) en iyi düzeni iyileştirme için izin verin. Bu özellikleri değiştirme bir maliyeti vardır ve hatta bunları varsayılan değerlere ayarlarken bellek tüketir.
- Kullanmaktan kaçının bir [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) mümkün olduğunda. Önemli ölçüde daha fazla iş yapmak zorunda CPU neden olur.
- Kullanırken bir [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), kullanmaktan kaçının [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) özelliği mümkün olduğunda.
- Kullanırken bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), bu yalnızca bir alt ayarlandığından emin olun [ `LayoutOptions.Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/). Bu özellik, belirtilen alt kaplar sağlar en büyük alanı `StackLayout` ona verin ve bu hesaplamalar birden çok kez kayıp.
- Yöntemlerinden birini herhangi bir çağrıda yok [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) gerçekleştirilen pahalı düzeni hesaplamalarda neden sınıfı. Bunun yerine, istediğiniz düzene davranışı ayarlayarak alınabilir büyük olasılıkla [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) ve [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) özellikleri. Alternatif olarak, bir alt [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) istenen düzen davranışı elde etmek için sınıf.
- Herhangi bir güncelleştirme [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) etiketinin boyutunu değiştir yeniden hesaplanan olan tüm ekran düzende neden olabileceğinden gerekli daha sık örnekleri.
- Ayarlamazsanız [ `Label.VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) özelliği gerekmiyorsa.
- Ayarlama [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) herhangi [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) için örnekler [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/) mümkün olduğunda.

<a name="optimizelistview" />

## <a name="optimize-listview-performance"></a>ListView performansı en iyi duruma getirme

Kullanırken bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetim hale getirilmiştir kullanıcı deneyimleri dizi vardır:

- **Başlatma** – denetimi oluşturulduğunda, başlayıp öğeleri ekranda gösterilen zaman zaman aralığı.
- **Kaydırma** – liste boyunca kaydırma ve kullanıcı arabirimini öteleme değil emin olun olanağı touch hareketleri.
- **Etkileşim** ekleme, silme ve öğeler seçme.

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Denetimi veri kaynağı ve şablonları hücre için bir uygulama gerektirir. Bu, nasıl sağlanır denetimi performans üzerinde büyük bir etkisi sahip olur. Daha fazla bilgi için bkz: [ListView performans](~/xamarin-forms/user-interface/listview/performance.md).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Görüntü kaynakları en iyi duruma getirme

Görüntü kaynakları görüntüleme uygulamanın bellek alanını önemli ölçüde artırabilir. Bu nedenle, bunlar yalnızca gerekli ve uygulama artık gerektirmesi hemen serbest bırakılacak oluşturulmalıdır. Örneğin, bir uygulama üzerinden bir akış verilerini okuyarak görüntüyü görüntülüyorsa, yalnızca gerekli olduğunda bu akış oluşturulduğundan emin olun ve bunu artık gerekli olmadığında, akış yayımlanan emin olun. Bu sayfa oluşturulduğunda veya zaman akış oluşturarak sağlanabilir [ `Page.Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) olay etkinleşir ve akışı atma zaman [ `Page.Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Disappearing/) olay etkinleşir.

İle görüntü için görüntü indirirken [ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) yöntemi, önbellek, sağlayarak indirilen görüntü [ `UriImageSource.CachingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) özelliği ayarlanmış `true`. Daha fazla bilgi için bkz: [görüntülerle çalışma](~/xamarin-forms/user-interface/images.md).

Daha fazla bilgi için bkz: [görüntü kaynakları en iyi duruma getirme](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

<a name="visualtree" />

## <a name="reduce-the-visual-tree-size"></a>Görsel ağaç boyutunu azaltma

Sayfadaki öğelerin sayısını azaltmak daha hızlı Oluştur sayfası hale getirir. Bunu elde etmek için iki ana tekniği vardır. İlk görünmez öğelerini gizleme sağlamaktır. [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) Her öğenin özelliği, öğeyi görsel ağaç parçası olmadığını olup olmayacağını belirler. Bu nedenle, bir öğe görünür değilse, diğer öğeleri gizli olduğu öğesi kaldırmak veya ayarlamak kendi `IsVisible` özelliğine `false`.

Gereksiz öğeleri kaldırmak için ikinci tekniktir bakın. Örneğin, aşağıdaki kod örneği, bir dizi görüntüleyen bir sayfa düzeni gösterilir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) öğeleri:

```xaml
<ContentPage.Content>
    <StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Hello" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Welcome to the App!" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Downloading Data..." />
        </StackLayout>
    </StackLayout>
</ContentPage.Content>
```

Aşağıdaki kod örneğinde gösterildiği gibi daha az öğe sayısı ile aynı sayfa düzeni korunabilir:

```xaml
<ContentPage.Content>
  <StackLayout Padding="20,20,0,0" Spacing="25">
    <Label Text="Hello" />
    <Label Text="Welcome to the App!" />
    <Label Text="Downloading Data..." />
  </StackLayout>
</ContentPage.Content>
```

<a name="resourcedictionary" />

## <a name="reduce-the-application-resource-dictionary-size"></a>Uygulama kaynak sözlük boyutunu azaltma

Uygulama genelinde kullanılan herhangi bir kaynağa yinelemesinden kaçınmak için uygulamanın kaynak sözlükte depolanması gerekir. Bu uygulama genelinde ayrıştırılması gerekir XAML miktarını azaltmak için yardımcı olur. Aşağıdaki örnekte gösterildiği kod `HeadingLabelStyle` kullanılan bir uygulama geniş ve bu nedenle uygulamanın kaynak sözlükte tanımlı kaynak:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Resources.App">
     <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
     </Application.Resources>
</Application>
```

Ancak, kaynakları sonra yerine uygulama başlangıcında bir sayfa tarafından istendiğinde ayrıştırılır gibi bir sayfaya özgü XAML uygulamanın kaynak sözlükte eklenmemelidir. Bir kaynak başlangıç sayfasını değil bir sayfa tarafından kullanılıyorsa, bu nedenle uygulama başladığında ayrıştırılır XAML azaltmaya yardımcı bu sayfa için kaynak sözlüğünde yerleştirilmelidir. Aşağıdaki örnekte gösterildiği kod `HeadingLabelStyle` yalnızca tek bir sayfada ve bu nedenle sayfanın kaynak sözlükte tanımlı kaynak:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test.HomePage"
             Padding="0,20,0,0">
     <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
     </ContentPage.Resources>
    <ContentPage.Content>
      ...
    </ContentPage.Content>
</ContentPage>

```

Uygulama kaynakları hakkında daha fazla bilgi için bkz: [ `Working with Styles` ](~/xamarin-forms/user-interface/styles/index.md).

<a name="rendererpattern" />

## <a name="use-the-custom-renderer-pattern"></a>Özel oluşturucu düzeni kullanın

Çoğu Oluşturucu sınıfları açığa `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için bir Xamarin.Forms özel denetim oluşturulduğunda çağrılan yöntemi. Özel oluşturucu sınıflarda her platforma özgü işleyici sınıfı örneği ve yerel denetimi özelleştirmek için bu yöntemi geçersiz kılın. `SetNativeControl` Yöntemi yerel denetim örneği oluşturmak için kullanılır ve bu yöntem aynı zamanda Denetim referansı atayacaktır `Control` özelliği.

Ancak, bazı durumlarda `OnElementChanged` yöntemi birden çok kez çağrılabilir. Bu nedenle, bir performans etkisi olabilir, bellek sızıntıları önlemek için dikkatli yeni bir yerel denetim başlatılırken olunması gerekir. Aşağıdaki kod örneğinde yaklaşımı özel oluşturucu içinde yeni bir yerel denetim başlatılırken kullanacak şekilde gösterilir:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Yeni bir yerel denetim yalnızca bir kez örneğinin oluşturulması, ne zaman `Control` özelliği `null`. Denetimi yalnızca yapılandırılmalıdır ve olay işleyicileri özel Oluşturucu yeni bir Xamarin.Forms öğesi eklendiğinde abone. Oluşturucu öğesi değişiklikleri iliştirildiğinde benzer şekilde, abone tüm olay işleyicileri yalnızca gelen aboneliği olmalıdır. Bu yaklaşım benimsenmesi bellek sızıntılarını yaşar olmayan bir verimli bir şekilde gerçekleştirme özel Oluşturucu oluşturmak için yardımcı olur.

Özel oluşturucu hakkında daha fazla bilgi için bkz: [özelleştirme denetimleri her platformda](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="summary"></a>Özet

Bu makalede açıklanan ve teknikleri Xamarin.Forms uygulamaların performansını artırmak için ele alınan. Topluca bu teknikler bir CPU ve bir uygulama tarafından kullanılan bellek miktarına tarafından gerçekleştirilen çalışma miktarını önemli ölçüde azaltabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası performansı](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [ListView performansı](~/xamarin-forms/user-interface/listview/performance.md)
- [Hızlı Oluşturucu](~/xamarin-forms/internals/fast-renderers.md)
- [Düzenini sıkıştırma](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Xamarin.Forms resim Boyutlandır örneği](https://developer.xamarin.com/samples/xamarin-forms/XamFormsImageResize/)
- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilation/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
