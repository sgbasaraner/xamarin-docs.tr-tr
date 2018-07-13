---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF vs. Xamarin.Forms: Benzerlikleri ve farkları'
description: Bu belge karşılaştırır ve WPF Xamarin.Forms ile karşılaştırır. Denetim şablonları, XAML, bağlama altyapısı, veri şablonları, ItemsControl, UserControl, gezinti ve URL gezintisini ele alınmaktadır.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 4d6585715b2fc118bb350c242abccbc68791ec0b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998524"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF vs. Xamarin.Forms: Benzerlikleri ve farkları

## <a name="control-templates"></a>Denetim şablonları

WPF kavramını destekler *denetim şablonları* bir denetim için görselleştirme yönergelerini sağlar (`Button`, `ListBox`vb..). Yukarıda belirtildiği gibi somut Xamarin.Forms kullanan _işleme_ bu denetimin görselleştirmek için yerel Platformu (iOS, Android, vb.) etkileşim sınıfları.

Ancak, Xamarin.Forms _mu_ sahip bir `ControlTemplate` türü - Tema oluşturma için kullanılan `Page` nesneleri. Bir tanımı için sağladığı bir `Page` , tutarlı içerik sağlar, ancak kullanıcının renkleri, yazı tipleri, vb. ve hatta uygulama için benzersiz olacak şekilde öğeleri eklemek sayfanın sağlar.

Bunun yaygın kullanımları nokta bir standartlaştırılmış ancak themable sayfa uygulama içinde özelleştirilebilen genel görünüme sağlamak ve kimlik doğrulaması iletişim kutuları, istemleri gibi yer almaktadır. Bu destek bir parçası olarak, çoğu tanıdık WPF adlı Denetim kullanılır:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Ancak bunların olduğunu bilmek önemlidir _değil_ Xamarin.Forms içinde aynı amaca hizmet. Bu özellik hakkında daha fazla bilgi için kullanıma [belgeleri sayfasını](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML, WPF ve Xamarin.Forms için bildirim temelli bir işaretleme dili olarak kullanılır. Çoğunlukla, sözdizimi aynıdır - birincil fark XAML grafikleri tarafından tanımlanan/oluşturulan nesneler.

- Xamarin.Forms destekler [XAML 2009 belirtimi](/dotnet/framework/xaml-services/xaml-2009-language-features/); bu sayede daha kolay gibi verileri tanımlamak `string`s, `int`s, vb. de olarak tanımlayan genel türler ve oluşturucular bağımsız değişkenleri geçirme.

- Şu anda XAML ile WPF gibi dinamik olarak yükleme olanağı, `XamlReader`. Aynı temel işlevsellikle alabileceğiniz bir [NuGet paketini](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) rağmen.

### <a name="markup-extensions"></a>Biçimlendirme uzantıları

Xamarin.Forms XAML WPF gibi biçimlendirme uzantıları aracılığıyla genişletme destekler. Kullanıma hazır, aynı temel yapı taşlarını içerir:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Ayrıca, içerdiği `{x:Reference}` XAML 2009 belirtiminden ve `{TemplateBinding}` özel sürümü için kullanılan biçimlendirme uzantısı `ControlTemplate` Xamarin.Forms tarafından desteklenir.

> [!WARNING]
> `ControlTemplate` Destek değil, aynı -, aynı ada sahip olsa da.

Özel biçimlendirme uzantıları da - Xamarin.Forms destekler, ancak uygulama biraz farklıdır. WPF içinde öğesinden türetilmelidir `MarkupExtension` -Özet temel sınıf. Xamarin.Forms içinde bir arabirim ile değiştirilir `IMarkupExtension` veya `IMarkupExtension<T>` daha esnek olduğu.

Tek gereken yöntemini sadece WPF gibi olan bir `ProvideValue` biçimlendirme uzantısından değeri döndürmek için yöntemi.

## <a name="binding-infrastructure"></a>Bağlama altyapısı

Devreden kavramları görsel özellikleri .NET veri özelliklerine bağlanmak için bir veri bağlama altyapısı biridir. Bu gibi MVVM mimari desenleri sağlar. Temel tasarım aynıdır - bağlanabilir ve temel bir sınıf sahip [BindableObject](xref:Xamarin.Forms.BindableObject), bu WPF [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) sınıfı. Bu temel sınıf kök üst veri bağlama hedeflerini olarak katılacak olan tüm nesneler için kullanılır. Türetilen sınıfların sunarsınız [BindableProperty](xref:Xamarin.Forms.BindableProperty) özellik değerleri için yedekleme depolama olarak davranan nesneleri (bunlar olarak tanımlanan [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) nesneleri ' WPF'de).

### <a name="defining-bindable-properties"></a>Bağlanabilir özellikler tanımlama

Xamarin.Forms içinde bağlanabilir bir özellik tanımı WPF aynıdır:
1. Nesne öğesinden türetilmelidir `BindableObject`.
2. Genel statik alan türü olmalıdır `BindableProperty` özelliği için destekleyen depolama anahtarını tanımlamak için bildirilmiş.
3. Kullanan genel örnek özelliği sarmalayıcı olmalıdır `GetValue` ve `SetValue` almak ve özellik değerini değiştirin.

Tam bir örnek için bkz. [Xamarin.Forms içinde bağlanabilir Özellikler](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Ekli özellikler

Ekli özellikler bağlanılabilir özellik bir alt kümesidir ve WPF içinde olduğu gibi çalışır. Başlıca fark, özellik sarmalayıcısı ommitted bu durumda olur ve bir dizi sahip olan sınıfın statik get/set yöntemleri yerine ' dir. Bkz: [Xamarin.Forms bağlı özelliklerinde](~/xamarin-forms/xaml/attached-properties.md) daha fazla bilgi için.

### <a name="using-the-binding-engine"></a>Bağlama altyapısı kullanma

WPF içinde olduğu gibi bağlama altyapısı kullanılarak işlemi aynıdır. Gerideki kod da yararlanılabilir oluşturarak bir `Binding` nesne kaynak nesne (herhangi bir .NET türü) ve isteğe bağlı özellik değeri için bağlı (ommitted, bu kaynak nesne özelliği kendisini - gibi davranır WPF). Ardından `SetBinding` herhangi `BindableObject` bağlamaya ilişkilendirilecek bir `BindableProperty`.

Alternatif olarak, XAML kullanarak bağlama ilişki tanımlayabilirsiniz `BindingExtension`. WPF içinde uzantısı olarak aynı temel değerleri içeren.

Bağlama desteği ve altyapı WPF Silverlight uygulamasına daha benzerdir. Xamarin.Forms içinde uygulanan olmayan çeşitli eksik özellikler vardır:

- Bağlamaları aşağıdaki özellikleri için desteği yoktur:
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

İçin desteği yoktur `RelativeSource` bağlar. WPF içinde bunlar, XAML içinde tanımlanan diğer görsel öğelere bağlamak izin verir. Xamarin.Forms içinde bu aynısını kullanarak gerçekleştirilebilir `{x:Reference}` işaretleme uzantısı. Örneğin, biz "metin özelliği adı otherControl" denetimiyle olduğunu varsayarsak, biz onu şu şekilde bağlayabilirsiniz:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Aynı özellik için kullanılabilir `{RelativeSource Self}` özelliği. Ancak türüne göre üst öğeleri bulmak için desteği yoktur (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Bağlama bağlamı

WPF içinde tanımladığınız bir `DataContext` özellik hangi reprents bağlama kaynağı varsayılan değeri. Bağlama kaynağı tanımlanmazsa, bu özellik değeri kullanılır. Değer, daha yüksek bir düzeyde tanımlanmasına olanak tanır ve ardından alt gruplar tarafından kullanılan görsel ağaç devralınır.

Xamarin.Forms içinde aynı özellik kullanılabilir, ancak özellik adı `BindingContext`.

#### <a name="value-converters"></a>Değer dönüştürücüler

Değer dönüştürücüler Xamarin.Forms - WPF gibi tam olarak desteklenir. Aynı arabirimi şekli kullanılır, ancak Xamarin.Forms sahip tanımlanan arabirimi `Xamarin.Forms` ad alanı.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM, WPF ve Xamarin.Forms tarafından tamamen desteklenir.

WPF içeren yerleşik bir `RoutedCommand` bazen kullanılır; Xamarin.Forms sahip yerleşik bir komut verme destek ötesinde `ICommand` arabirim tanımı. MVVM çerçeveleri MVVM uygulamak için gerekli temel sınıfları eklemek için çeşitli içerebilir.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged ve INotifyCollectionChanged

Her iki arabirimde Xamarin.Forms bağlarında tam olarak desteklenir. XAML tabanlı birçok çerçeveleri aksine Xamarin.Forms (olduğu gibi WPF) içinde arka plan iş parçacığı üzerinde özellik değişikliği bildirimleri yükseltilebilir ve bağlama altyapısı düzgün UI iş parçacığına geçiş yapacaktır.

Ayrıca, her iki ortama destekler `SynchronziationContext` ve `async` / `await` uygun iş parçacığı taşıma yapmak için. WPF içerir `Dispatcher` sınıfı tüm görsel öğeleri üzerinde statik bir yöntem Xamarin.Forms sahip `Device.BeginInvokeOnMainThread` da kullanılabilir (ancak `SynchronizationContext` platformlar arası kodlama için tercih edilir).

- Xamarin.Forms içeren bir `ObservableCollection<T>` koleksiyon değişiklik bildirimleri destekler.
- Kullanabileceğiniz `BindingBase.EnableCollectionSynchronization` bir koleksiyon için iş parçacıkları arası güncelleştirmeleri etkinleştirecek şekilde. WPF değişim biraz farklı bir API'dir [kullanım ayrıntıları için belgeleri denetleyin](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*).

## <a name="data-templates"></a>Veri şablonları

Veri şablonları işlenmesi özelleştirmek için Xamarin.Forms içinde desteklenen bir `ListView` satır (hücre). Hangi kullanabilir WPF aksine `DataTemplate`s Xamarin.Forms şu anda tüm içeriği odaklı denetimi için yalnızca kullandığı için bunları `ListView`. Satır içi olarak tanımlanan şablon tanımı olabilir (atanan `ItemTemplate` özelliği), veya bir kaynak olarak bir `ResourceDictionary`.

Ayrıca, bunlar oldukça, WPF karşılığı kadar esnek değildir.

1. Kök öğesi `DataTemplate` gerekir _her zaman_ olması bir `ViewCell` nesne.
2. Veri tetikleyicileri veri şablonu içinde tam olarak desteklenir, ancak içermelidir bir `DataType` tetikleyici ile ilişkili özelliğin türünü gösteren özellik.
3. `DataTemplateSelector` Ayrıca desteklenir, ancak türetilen `DataTemplate` ve bu nedenle yalnızca doğrudan atanan `ItemTemplate` özelliği (karşılaştırması `ItemTemplateSelector` WPF).

## <a name="itemscontrol"></a>ItemsControl

İçin hiçbir yerleşik equivelent var. bir `ItemsControl` xamarin.Forms'taki; ancak bir [buradan kullanılabilir Xamarin.Forms için özel bir](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Kullanıcı denetimleri

WPF, `UserControl`s davranışı ilişkili, kullanıcı arabiriminin bir bölümü sağlamak amacıyla kullanılır. Xamarin.Forms içinde kullandığımız `ContentView` aynı amaçla. XAML içinde bağlama ve dahil etme hem de destekler.

## <a name="navigation"></a>Gezinti

Nadiren kullanılan bir WPF içerir `NavigationService` kullanılabilen "tarayıcı benzeri" gezinti özelliği sağlamak için. Uygulamaların çoğu bu ancak ve bunun yerine kullanılan rahatsız yaramadı farklı `Window` öğeleri veya verileri görüntülemek için pencerenin farklı bölümlerini.

Phone cihazlarında, farklı _ekranlar_ genellikle çözüm olur ve bu nedenle Xamarin.Forms Gezinti birkaç biçimleri için destek içerir:

| Gezinti stili | Sayfa türü |
|--- |--- |
|Yığın tabanlı (anında iletme/pop)|NavigationPage|
|Ana/ayrıntı|MasterDetailPage|
|Sekmeleri|TabbedPage|
|Manyetik sol/sağ|CarouselView|

`NavigationPage` En yaygın bir yaklaşımdır ve her sayfanın sahip bir `Navigation` İtme veya sayfa açma ve kapatma gezinme yığınında pop için kullanılan özellik. İçin en yakın equivelent budur `NavigationService` WPF içinde bulunamadı.

### <a name="url-navigation"></a>URL gezintisi

WPF masaüstü tabanlı bir teknolojidir ve başlangıç davranışını yönlendirmek için komut satırı parametrelerini kabul edebilir. Xamarin.Forms kullanabileceğiniz [URL ayrıntılı bağlantı sağlama](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) başlangıçta bir sayfaya gitmek için.
