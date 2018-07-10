---
title: Xamarin.Forms performans
description: Xamarin.Forms uygulamalarının performansını artırmaya yönelik birçok teknik vardır. Topluca bu tekniklerin bir CPU ve bir uygulama tarafından kullanılan bellek miktarı tarafından gerçekleştirilen iş miktarını önemli ölçüde azaltabilir. Bu makalede, tanımlar ve bu teknikler açıklanır.
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: ae284cf90ccb2d2735b4fafa0c0e44f69533638f
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935166"
---
# <a name="xamarinforms-performance"></a>Xamarin.Forms performans

_Xamarin.Forms uygulamalarının performansını artırmaya yönelik birçok teknik vardır. Topluca bu tekniklerin bir CPU ve bir uygulama tarafından kullanılan bellek miktarı tarafından gerçekleştirilen iş miktarını önemli ölçüde azaltabilir. Bu makalede, tanımlar ve bu teknikler açıklanır._

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**2016 evrim Geçiren: Xamarin.Forms ile uygulama performansını iyileştirme**

## <a name="overview"></a>Genel Bakış

Kötü uygulama performansı kendisini birçok şekilde gösterir. Bir uygulamayı yapabilirsiniz yanıt vermiyor, yavaş kaydırma neden olabilir ve pil ömrü azaltabilir. Ancak, performansı en iyi duruma getirme verimli kod daha fazlasını uygulama içerir. Uygulama performansı kullanıcı deneyimi de dikkate alınmalıdır. Örneğin, kullanıcının diğer etkinliklerini gerçekleştirmesi engellemeden işlemleri yürütmek sağlayarak kullanıcı deneyimini geliştirmek için yardımcı olabilir.

Çeşitli performans ve bir Xamarin.Forms uygulamasının algılanan performansını artırmaya yönelik teknikler vardır. Bunlara aşağıdakiler dahildir:

- [XAML derleyiciyi etkinleştir](#xamlc)
- [Doğru bir düzen seçin](#correctlayout)
- [Düzen sıkıştırmayı etkinleştir](#layoutcompression)
- [Hızlı oluşturucular kullanma](#fastrenderers)
- [Gereksiz bağlamaları azaltın](#databinding)
- [Düzen performansını iyileştirme](#optimizelayout)
- [ListView performansını iyileştirme](#optimizelistview)
- [Resim kaynakları en iyi duruma getirme](#optimizeimages)
- [Görsel ağacı azaltın](#visualtree)
- [Uygulama kaynak sözlüğü boyutunu azaltın](#resourcedictionary)
- [Özel oluşturucu desenini kullanma](#rendererpattern)

> [!NOTE]
>  Bu makalede okumadan önce okumalısınız [platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md), bellek kullanımı ve Xamarin platformu kullanılarak oluşturulan uygulamaların performansını artırmak için platform olmayan belirli teknikler açıklanır.

<a name="xamlc" />

## <a name="enable-the-xaml-compiler"></a>XAML derleyiciyi etkinleştir

XAML, Ara dil (IL) XAML derleyicisi (XAMLC) ile doğrudan isteğe bağlı olarak derlenebilir. XAMLC bir avantajları sunar:

- Bu hataların kullanıcıya bildirimde, XAML derleme zamanı denetimi gerçekleştirir.
- Bazı yük ve örnek oluşturma saati XAML öğeleri kaldırır.
- Artık .xaml dosyalarını dahil ederek son derlemeyi dosya boyutunu küçültmek için yardımcı olur.

XAMLC geriye dönük uyumluluk sağlamak için varsayılan olarak devre dışıdır. Ancak, bu derleme ve sınıf düzeyinde sırasında etkinleştirilebilir. Daha fazla bilgi için [XAML derleme](~/xamarin-forms/xaml/xamlc.md).

<a name="correctlayout" />

## <a name="choose-the-correct-layout"></a>Doğru bir düzen seçin

Birden çok alt öğeleri görüntüleme yeteneğine sahip olan, ancak yalnızca tek bir alt olan bir düzen kısıp. Aşağıdaki örnekte gösterildiği gibi kod bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) tek bir alt ile:

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

Kısıp budur ve [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) öğesi kaldırılmalıdır, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <Image Source="waterfront.jpg" />
    </ContentPage.Content>
</ContentPage>
```

Ayrıca, bu sonuçları gereksiz Düzen hesaplamalarında gerçekleştirilmekte olan diğer düzenleri birleşimlerini kullanarak belirli bir düzeni görünümünü oluşturmaya çalışmayın. Örneğin, oluşturmaya çalışmayın bir [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) bir birleşimini kullanarak Düzen [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) örnekleri. Aşağıdaki kod örneği, bu hatalı bir uygulama örneği gösterilmektedir:

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

Gereksiz Düzen hesaplamaların kısıp olmasıdır. Bunun yerine, istediğiniz düzene daha iyi kullanarak gerçekleştirilebilir bir [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)aşağıdaki kod örneğinde gösterildiği gibi:

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

Düzen sıkıştırma belirtilen düzenleri sanal ağaçtan sayfa işleme performansını girişimi kaldırır. Bu teslim performans avantajı, bir sayfa, kullanılan işletim sistemi sürümü ve uygulamanın üzerinde çalıştığı aygıtın karmaşıklığına bağlı olarak değişir. Ancak, en büyük gelen performans artışı eski cihazlarda görülür. Daha fazla bilgi için [Düzen sıkıştırma](~/xamarin-forms/user-interface/layouts/layout-compression.md).

<a name="fastrenderers" />

## <a name="use-fast-renderers"></a>Hızlı oluşturucular kullanma

Hızlı oluşturucular tarafından oluşturulan yerel denetim hiyerarşisi düzleştirme Enflasyon ve Xamarin.Forms denetimleri android'de işleme maliyetlerini azaltın. Bu daha fazla performans sonuçları daha az karmaşık bir görsel ağaç'a daha az bellek kullanımı kapatır, daha az nesne oluşturarak artırır. Daha fazla bilgi için [hızlı Oluşturucu](~/xamarin-forms/internals/fast-renderers.md).

<a name="databinding" />

## <a name="reduce-unnecessary-bindings"></a>Gereksiz bağlamaları azaltın

Bağlama, statik olarak kolayca ayarlanabilen içeriği için kullanmayın. Bağlamaları maliyet etkin olmadığından, bağlanması için gereken değil veri bağlama hiçbir avantajı yoktur. Örneğin, ayarlamak `Button.Text = "Accept"` bağlama daha az yüke sahip [ `Button.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/) bir ViewModel için `string` özelliği "Kabul et" değerine sahip.

<a name="optimizelayout" />

## <a name="optimize-layout-performance"></a>Düzen performansını iyileştirme

Xamarin.Forms 2 Düzen güncelleştirmeleri etkileyen bir en iyi duruma getirilmiş yerleşim altyapısı kullanıma sunuldu. Olası düzenini en iyi performansı elde etmek için aşağıdaki yönergeleri izleyin:

- Belirterek Düzen hiyerarşi derinliğini azaltmak [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) özellik değerleri, daha az sarmalama görünüm düzenleri oluşturulmasını sağlar. Daha fazla bilgi için [kenar boşlukları ve doldurma](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- Kullanırken bir [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), olabildiğince az sayıda satır ve sütun mümkün olduğunca ayarlandığından emin olmak deneyin [ `Auto` ](https://developer.xamarin.com/api/property/Xamarin.Forms.GridLength.Auto/) boyutu. Her Otomatik Boyutlandır satır veya sütun ek Düzen hesaplamalar gerçekleştirmek yerleşim altyapısı neden olur. Bunun yerine, sabit boyutlu satırlar ve sütunlar mümkünse kullanın. Alternatif olarak, satır ve sütun kaplayan orantılı bir boşluk miktarını ayarlama [ `GridUnitType.Star` ](xref:Xamarin.Forms.GridUnitType.Star) numaralandırma değeri, sağlanan üst ağaç bu düzen yönergeleri izler.
- Ayarlamamanız [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) ve [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) bir düzen özelliklerini gerekmedikçe. Varsayılan değerleri [ `LayoutOptions.Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill) ve [ `LayoutOptions.FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) izin vermek için en iyi Düzen iyileştirmesi. Bu özellikleri değiştirerek bir maliyeti vardır ve varsayılan değerlere ayarlandığında bile bunları bellek tüketir.
- Kullanmaktan kaçının bir [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) mümkün olduğunda. Bu, önemli ölçüde daha fazla iş yapmak zorunda CPU neden olur.
- Kullanırken bir [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), kullanmaktan kaçının [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) mümkün olduğunca özelliği.
- Kullanırken bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), bu yalnızca bir alt ayarlandığından emin olun [ `LayoutOptions.Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/). Bu özellik, belirtilen alt kaplayacağı sağlar en büyük alanı `StackLayout` kendisine verebilirsiniz; bu hesaplamalar birden çok kez kısıp.
- Yöntemlerinin birini çağırmaz [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) bunlar gerçekleştirilmekte olan pahalı Düzen hesaplamalara neden gibi sınıf. Bunun yerine, istediğiniz düzene davranışı ayarlayarak alınabilir olma olasılığı yüksektir [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) ve [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) özellikleri. Alternatif olarak, alt [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) istediğiniz düzene davranışı elde etmek için sınıf.
- Tüm güncelleştirme [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) etiketi boyutunu değiştirmek yeniden hesaplanan olan tüm ekran düzende oluşturacağından daha sık istenen sürümden örnekler.
- Ayarlamamanız [ `Label.VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) özelliği gerekmedikçe.
- Ayarlama [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) herhangi [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) için örnekler [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode.NoWrap) mümkün olduğunda.

<a name="optimizelistview" />

## <a name="optimize-listview-performance"></a>ListView performansını iyileştirme

Kullanırken bir [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetim optimize edilmesini kullanıcı deneyimleri sayısı vardır:

- **Başlatma** – denetimi oluşturulduğunda, başlayıp öğeler ekranda göründüğü zaman aralığı.
- **Kaydırma** – yeteneği listede gezinmek ve kullanıcı arabirimini lag değil emin olmak için dokunma hareketlerini.
- **Etkileşim** ekleme, silme ve öğeleri seçme.

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Denetimi veri kaynağı ve şablonları hücre için bir uygulama gerektirir. Bu nasıl yapılır denetimin performans üzerinde büyük etkiye sahip olur. Daha fazla bilgi için [ListView performans](~/xamarin-forms/user-interface/listview/performance.md).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Resim kaynakları en iyi duruma getirme

Görüntü kaynakları görüntüleme, uygulamanın bellek Ayak izi önemli ölçüde artırabilirsiniz. Bu nedenle, bunlar yalnızca gerekli ve uygulama artık gerektirdiği hemen sonra serbest bırakılması oluşturulmalıdır. Örneğin, bir uygulamayı bir akıştan verilerini okuyarak bir görüntüyü görüntüleme, yalnızca gerekli olduğunda bu oluşturulduğundan emin olun ve tutun artık gerekli olmadığında, akış yayımlanan emin olun. Bu sayfa oluşturulduğunda ya da akış oluşturarak gerçekleştirilebilir [ `Page.Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) olay harekete geçirilir ve ardından akışını disposing olduğunda [ `Page.Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Disappearing/) olay harekete geçirilir.

Bir ekran için bir görüntü indirirken [ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) yöntemi sağlayarak, indirilmiş bir görüntü önbelleği [ `UriImageSource.CachingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) özelliği `true`. Daha fazla bilgi için [görüntülerle çalışma](~/xamarin-forms/user-interface/images.md).

Daha fazla bilgi için [resim kaynakları en iyi duruma getirme](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

<a name="visualtree" />

## <a name="reduce-the-visual-tree-size"></a>Görsel ağacı azaltın

Sayfadaki öğelerin sayısını azaltmak daha hızlı işleme sayfası hale getirir. Bunu elde etmek için iki ana teknikler vardır. İlk görünmez öğelerini gizleme sağlamaktır. [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) Her öğenin özelliği, öğe veya görsel ağacın bir parçası olup olmayacağını belirler. Bu nedenle, bir öğe görünür değilse, diğer öğelerin gizlenmiş olduğundan öğesi kaldırmak veya ayarlamak kendi `IsVisible` özelliğini `false`.

İkinci yöntem, gereksiz öğeler kaldırmaktır. Örneğin, aşağıdaki kod örneği, bir dizi görüntüleyen bir sayfa düzeni gösterir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) öğeleri:

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

Aşağıdaki kod örneğinde gösterildiği gibi bir düşük öğe sayısı ile aynı sayfa düzeni korunabilir:

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

## <a name="reduce-the-application-resource-dictionary-size"></a>Uygulama kaynak sözlüğü boyutunu azaltın

Uygulamanın tamamında kullanılan tüm kaynakları yinelemesinden kaçınmak için uygulamanın kaynak sözlüğünde depolanması gerekir. Bu uygulamanın tamamında ayrıştırılacak olan XAML azaltmaya yardımcı olur. Aşağıdaki örnekte gösterildiği kod `HeadingLabelStyle` kaynak geniş kullanılan uygulama ve uygulamanın kaynak sözlüğünde şekilde tanımlanır:

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

Ancak, kaynakları ardından yerine uygulama başlangıcında bir sayfa tarafından istendiğinde ayrıştırılacak gibi bir sayfasına özel XAML uygulamanın kaynak sözlüğünde eklenmemelidir. Bir kaynak başlangıç sayfasını değil bir sayfa tarafından kullanılıyorsa, bu nedenle uygulama başladığında ayrıştırılır XAML azaltmaya yardımcı olur, bu sayfanın kaynak sözlüğünde yerleştirilmelidir. Aşağıdaki örnekte gösterildiği kod `HeadingLabelStyle` kaynak yalnızca tek bir sayfada ve bu nedenle sayfanın kaynak sözlüğünde tanımlanır:

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

## <a name="use-the-custom-renderer-pattern"></a>Özel oluşturucu desenini kullanma

Çoğu işleyici sınıflarını sunmaya `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için özel bir Xamarin.Forms Denetim oluşturulurken çağrılan yöntemi. Özel oluşturucu sınıflarda her platforma özgü işleyici sınıfı, örneği ve yerel denetimi özelleştirmek için bu yöntemi geçersiz kılın. `SetNativeControl` Yöntemi yerel denetim örneği oluşturmak için kullanılır ve bu yöntem ayrıca Denetim başvurusu atar `Control` özelliği.

Ancak, bazı durumlarda `OnElementChanged` yöntemi birden çok kez çağrılabilir. Bu nedenle, bir performans etkisi olabilir, bellek sızıntılarını önlemek için dikkatli yeni bir yerel denetim örneği oluşturulurken olunması gerekir. Özel oluşturucu içinde yeni bir yerel denetim örneği oluşturulurken kullanılacak bir yaklaşım aşağıdaki kod örneğinde gösterilmiştir:

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

Yeni bir yerel denetim yalnızca bir kez örneği, `Control` özelliği `null`. Denetim yalnızca yapılandırılmalıdır ve özel Oluşturucu yeni bir Xamarin.Forms öğe eklendiğinde olay işleyicileri abone. Öğesi işleyici değişiklikleri iliştirildiğinde benzer şekilde, abone tüm olay işleyicileri yalnızca gelen aboneliği olması gerekir. Bu yaklaşımı benimsemeyi bellek sızıntılardan etkilese değil, verimli bir şekilde gerçekleştirmek özel bir oluşturucu oluşturmaya yardımcı olur.

Özel oluşturucular hakkında daha fazla bilgi için bkz: [her platformda denetimleri özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="summary"></a>Özet

Bu makalede açıklanan ve Xamarin.Forms uygulamalarının performansını artırmak için tekniklerin ele alınan. Topluca bu tekniklerin bir CPU ve bir uygulama tarafından kullanılan bellek miktarı tarafından gerçekleştirilen iş miktarını önemli ölçüde azaltabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası performans](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [ListView performans](~/xamarin-forms/user-interface/listview/performance.md)
- [Hızlı Oluşturucular](~/xamarin-forms/internals/fast-renderers.md)
- [Düzen Sıkıştırma](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Xamarin.Forms görüntü Boyutlandırıcı örnek](https://developer.xamarin.com/samples/xamarin-forms/XamFormsImageResize/)
- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilation/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
