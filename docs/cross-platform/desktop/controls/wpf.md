---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF vs. Xamarin.Forms: Benzerlikler & farkları'
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: e72297963b6be50572e4bb539263065592737c42
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF vs. Xamarin.Forms: Benzerlikler & farkları

## <a name="control-templates"></a>Denetim şablonları

WPF kavramını destekler *denetim şablonları* denetim için görselleştirme yönergeleri sağlar (`Button`, `ListBox`vb..). Yukarıda belirtildiği gibi Xamarin.Forms somut kullanan _işleme_ bu için denetimi görselleştirmek için yerel Platformu (iOS, Android, vb.) etkileşim sınıfları.

Ancak, Xamarin.Forms _mu_ sahip bir `ControlTemplate` türü - Tema oluşturma için kullanılan `Page` nesneleri. İçin bir tanım sağlayan bir `Page` , tutarlı içerik sağlar, ancak renkleri, yazı tipi, vb. değiştirip bile uygulama için benzersiz olması için öğeler eklemek sayfanın kullanıcısının verir.

Bunun yaygın kullanımları uygulama içinde özelleştirilebilir standartlaştırılmış ancak themable sayfası Görünüm ve kullanımında sağlamak için ve kimlik doğrulama iletişim kutuları, komut istemlerini gibi noktalardır. Bu destek bir parçası olarak, birçok tanıdık WPF adlı denetimleri kullanılır:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Ancak bunların olduğunu bilmeniz önemlidir _değil_ Xamarin.Forms içinde aynı amaca hizmet. Bu özellik hakkında daha fazla bilgi için kullanıma [belge sayfasının](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML bildirim temelli işaretleme dili olarak WPF ve Xamarin.Forms için kullanılır. Çoğunlukla, aynı söz dizimi - XAML grafikleri tarafından tanımlanan/oluşturulan nesneler birincil farktır.

- Xamarin.Forms destekleyen [XAML 2009 belirtimi](/dotnet/framework/xaml-services/xaml-2009-language-features/); bu verileri gibi tanımlamak kolaylaştırır `string`s, `int`s, vs. de olarak tanımlayan genel türler ve oluşturucuları için bağımsız değişkenleri geçirme.

- Şu anda bir yolu yoktur dyanmically yük XAML WPF ile gibi `XamlReader`. Aynı temel işlevselliğe sahip alabileceğiniz bir [NuGet paketi](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) rağmen.

### <a name="markup-extensions"></a>İşaretleme uzantıları

WPF benzediğini biçimlendirme uzantıları yoluyla XAML genişletme Xamarin.Forms destekler. Kutudan çıktığında, aynı temel yapı taşlarını içerir:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Ayrıca, içerdiği `{x:Reference}` XAML 2009 belirtiminden ve bir `{TemplateBinding}` özel sürümü için kullanılan biçimlendirme uzantısı `ControlTemplate` Xamarin.Forms tarafından desteklenir.

> [!WARNING]
> `ControlTemplate` Destek değil, aynı -, aynı ada sahip olsa bile.

Özel biçimlendirme uzantıları de - Xamarin.Forms destekler, ancak uygulama biraz farklıdır. Öğesinden türetilmelidir WPF içinde `MarkupExtension` -Özet temel sınıf. Xamarin.Forms içinde bir arabirim ile değiştirilir `IMarkupExtension` veya `IMarkupExtension<T>` olduğu daha esnektir.

Yalnızca WPF gibi tek gereken yöntemdir bir `ProvideValue` biçimlendirme uzantı değerini döndürmek için yöntem.

## <a name="binding-infrastructure"></a>Bağlama altyapısı

Taşınan temel kavramlar görsel özellikleri için .NET veri özellikleri bağlanmak için bir veri bağlama altyapısı biridir. Bu mimari desenler MVVM gibi sağlar. Temel tasarım aynıdır - bağlanabilirse temel sınıfı olan [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), bu WPF'de [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) sınıfı. Bu temel sınıf kök üst Veri bağlamada hedefleri olarak katılacak olan tüm nesneler için kullanılır. Türetilen sınıflar sonra kullanıma [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) özellik değerleri için yedekleme depolama birimi olarak davranan nesneleri (bunlar olarak tanımlanan [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) WPF nesneler).

### <a name="defining-bindable-properties"></a>Bağlanabilir özellikleri tanımlama

Xamarin.Forms bağlanabilirse özelliğinde tanımı WPF ile aynıdır:
1. Nesne öğesinden türetilmelidir `BindableObject`.
2. Türü bir public static alanı olmalıdır `BindableProperty` yedekleme depolama anahtar özelliği için tanımlamak için bildirilmedi.
3. Kullanan bir ortak örnek özellik sarmalayıcısı olmalıdır `GetValue` ve `SetValue` almak ve Özellikler değeri değiştirin.

Tam bir örnek için bkz: [Xamarin.Forms bağlanabilirse özelliklerinde](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Ekli özellikler

Ekli özellikler bağlanabilirse özelliği bir alt kümesidir ve WPF içinde yaparlar aynı şekilde çalışır. Birincil özellik sarmalayıcısı atlanırsa bu durumda olur ve statik get/set yöntemlere sahip olan sınıf kümesi yerine farktır. Bkz: [Xamarin.Forms bağlı özelliklerinde](~/xamarin-forms/xaml/attached-properties.md) daha fazla bilgi için.

### <a name="using-the-binding-engine"></a>Bağlama altyapısı kullanma

WPF içinde olduğu gibi bağlama altyapısı kullanma işlemini aynıdır. Kod arkasında, kullanılabilir oluşturarak bir `Binding` nesnesi bağlı bir kaynak nesnesi (herhangi bir .NET türü) ve isteğe bağlı özellik değeri (atlanırsa, bu kaynak nesnesi özellik olarak kendisi - gibi davranır, WPF). Daha sonra `SetBinding` herhangi `BindableObject` bağlamaya ilişkilendirilecek bir `BindableProperty`.

XAML kullanarak bağlama ilişki alternatif olarak, tanımlayabilirsiniz `BindingExtension`. WPF içinde uzantısı olarak aynı temel değerleri içeriyor.

Bağlama desteği ve altyapısı WPF Silverlight uygulaması daha benzerdir. Xamarin.Forms içinde uygulanan değil birkaç eksik özellikler şunlardır:

- Bağlamaları yer alan aşağıdaki özellikler için desteği yoktur:
    - BindingGroupName
    - BindsDirectlyToSource
    - Isasync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - ValidationRules koleksiyonu
    - XPath
    - XmlNamespaceManager
- `Binding.Mode` desteklemediği `OneTime`, bunun yerine yalnızca kullanın `OneWay`.

#### <a name="relativesource"></a>RelativeSource

İçin destek yok `RelativeSource` bağlar. WPF içinde bu XAML içinde tanımlanan diğer görsel öğelere bağlamanıza olanak sağlar. Xamarin.Forms içinde aynı bu özelliği kullanarak elde edilebilir `{x:Reference}` biçimlendirme uzantısı. Örneğin, biz "metin özelliğine adı otherControl" denetimiyle varsayarak, biz onu şöyle bağlayabilirsiniz:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Aynı özellik kullanılabilir `{RelativeSource Self}` özelliği. Ancak üst türe göre bulmak için desteği yoktur (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Bağlama bağlamı

WPF içinde tanımladığınız bir `DataContext` özelliği hangi reprents bağlama kaynağı varsayılan değer. Bir bağlama kaynağı tanımlanmazsa, bu özellik değeri kullanılır. Değer, daha yüksek bir düzeyde tanımlanan izin veren ve gruplar tarafından kullanılan görsel ağaç aşağı devralınır.

Xamarin.Forms içinde aynı özellik avaialable, ancak özellik adı `BindingContext`.

#### <a name="value-converters"></a>Değer dönüştürücüler

Değer dönüştürücüler Xamarin.Forms - WPF gibi tam olarak desteklenir. Aynı arabirim şekli kullanılır, ancak Xamarin.Forms tanımlanan arabirimine sahiptir `Xamarin.Forms` ad alanı.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM WPF ve Xamarin.Forms tarafından tamamen desteklenir.

WPF içeren yerleşik bir `RoutedCommand` bazen kullanılan; Xamarin.Forms sahip ötesinde yerleşik bir komut verme desteği yok `ICommand` arabirim tanımı. MVVM uygulamak için gerekli temel sınıfları eklemek üzere MVVM çerçeveleri çeşitli içerebilir.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged ve INotifyCollectionChanged

Her iki arabirimde Xamarin.Forms bağlama tam olarak desteklenir. XAML tabanlı birçok çerçeveleri aksine özellik değişikliği bildirimlerini Xamarin.Forms (gibi WPF) arka plan iş parçacığı üzerinde gerçekleştirilen ve bağlama altyapısı için kullanıcı Arabirimi iş parçacığı düzgün geçer.

Ayrıca, her iki ortama Destek `SynchronziationContext` ve `async` / `await` uygun iş parçacığı dizimi yapmak için. WPF içerir `Dispatcher` sınıfı tüm görsel öğeler üzerinde bir statik yöntem Xamarin.Forms sahip `Device.BeginInvokeOnMainThread` da kullanılabilir (ancak `SynchronizationContext` platformlar arası kodlama için tercih edilir).

- Xamarin.Forms içeren bir `ObservableCollection<T>` toplama değişiklik bildirimleri destekler.
- Kullanabileceğiniz `BindingBase.EnableCollectionSynchronization` bir koleksiyon için iş parçacıkları arası güncelleştirmeleri etkinleştirecek şekilde. WPF değişim biraz daha farklı bir API'dir [kullanım ayrıntıları için belgeleri denetleyin](https://developer.xamarin.com/api/member/Xamarin.Forms.BindingBase.EnableCollectionSynchronization/).

## <a name="data-templates"></a>Veri şablonları

Veri şablonları Xamarin.Forms işlenmesi özelleştirmek için desteklenen bir `ListView` satır (hücre). Kullanabilir WPF aksine `DataTemplate`s Xamarin.Forms şu anda hiçbir içerik yönelimli denetim için yalnızca kullanan bunları `ListView`. Satır içi olarak tanımlanan şablon tanımının olabilir (atanan `ItemTemplate` özelliği), veya bir kaynak olarak bir `ResourceDictionary`.

Buna ek olarak, bunlar oldukça kendi WPF karşılık gelen kadar esnek değildir.

1. Kök öğesinin `DataTemplate` gerekir _her zaman_ olması bir `ViewCell` nesnesi.
2. Veri tetikleyicileri veri şablonda tam olarak desteklenir, ancak içermelidir bir `DataType` tetikleyici ile ilişkili özelliğin türünü gösteren özellik.
3. `DataTemplateSelector` Ayrıca desteklenir, ancak türetilen `DataTemplate` ve bu nedenle yalnızca doğrudan atanan `ItemTemplate` özelliği (karşılaştırması `ItemTemplateSelector` WPF içinde).

## <a name="itemscontrol"></a>ItemsControl

Yerleşik hiçbir equivelent yoktur bir `ItemsControl` Xamarin.Forms; ancak bir [Xamarin.Forms kullanılabilir burada için özel bir](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Kullanıcı denetimleri

WPF, `UserControl`s davranışı ilişkili kullanıcı Arabirimi bir bölümünü sağlamak için kullanılır. Xamarin.Forms kullanırız `ContentView` aynı amaçla. XAML'de bağlama ve ekleme hem de destekler.

## <a name="navigation"></a>Gezinme

WPF içerir ender kullanılan `NavigationService` hangi "tarayıcı benzeri" gezinti özelliği sağlamak için kullanılabilir. Çoğu uygulamanın bunu ancak ve bunun yerine kullanılan rahatsız kaydetmedi farklı `Window` öğeleri veya verileri görüntülemek için pencereyi farklı bölümlerini.

Phone cihazlarının, farklı _ekranlar_ genellikle çözümdür ve bu nedenle Xamarin.Forms Gezinti birkaç formları için destek içerir:

| Gezinti stili | Sayfa türü |
|--- |--- |
|Yığın tabanlı (itme/pop)|NavigationPage|
|Ana/ayrıntı|MasterDetailPage|
|Sekmeleri|TabbedPage|
|Manyetik sol/sağ|CarouselView|

`NavigationPage` En yaygın yaklaşım olduğunu ve her sayfada bir `Navigation` İtme veya sayfalarını açma ve kapatma Gezinti yığını pop için kullanılan özellik. En yakın equivelent budur `NavigationService` WPF içinde bulunamadı.

### <a name="url-navigation"></a>URL gezinme

WPF bir masaüstü odaklı bir teknolojidir ve başlangıç davranışını yönlendirmek için komut satırı parametreleri kabul edebilir. Xamarin.Forms kullanabileceğiniz [derin URL bağlama](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) başlangıcında bir sayfaya atlamak için.
