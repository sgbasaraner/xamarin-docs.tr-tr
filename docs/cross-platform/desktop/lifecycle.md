---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF vs. Xamarin.Forms uygulaması yaşam döngüsü
description: Bu belge karşılaştırır arasındaki benzerlikleri ve farkları Xamarin.Forms ve WPF uygulamaları için uygulama yaşam döngüsü. Ayrıca, görsel ağacı, grafik, kaynakları ve stilleri arar.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: a59f257d1e6285fa2d899271a1aae9778b04d985
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780596"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF vs. Xamarin.Forms uygulaması yaşam döngüsü

Xamarin.Forms, çok sayıda, daha önce özellikle WPF gelen XAML tabanlı çerçeveleri tasarım kılavuzluğu alır. Ancak, diğer yollarla, önemli ölçüde taşıma girişiminde kişiler için bir Yapışkan noktası olabilecek farklılık göstermesi. Bu belge, bu sorunlardan bazılarını tanımlamak ve mümkün olduğunda köprüsü WPF bilgi Xamarin.Forms için rehberlik dener.

## <a name="app-lifecycle"></a>Uygulama yaşam döngüsü

Uygulama yaşam döngüsünü WPF ve Xamarin.Forms arasında benzerdir. Hem dış (platform) kodda başlatın ve kullanıcı Arabirimi aracılığıyla bir yöntem çağrısının başlatın. Xamarin.Forms her zaman ardından başlatır ve uygulama için kullanıcı Arabirimi oluşturur, platforma özgü derlemede başladığını fark vardır.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main` Yöntemi olduğundan, varsayılan olarak, otomatik oluşturulan ve kod içinde görünür.

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Uygulama sınıfı

Hem WPF hem de Xamarin.Forms bir `Application` sınıfı singleton oluşturulur. Bu kesinlikle WPF'de gerekli değildir ancak çoğu durumda, uygulamaları özel bir uygulama sağlamak için bu sınıftan türetilen. Her ikisi de kullanıma bir `Application.Current` oluşturulan tekli bulmak için özellik.

### <a name="global-properties--persistence"></a>Genel Özellikler + kalıcılığı

Hem WPF hem de Xamarin.Forms bir `Application.Properties` herhangi bir uygulama içinde erişilebilir genel uygulama düzeyi nesneler depolayabileceği sözlük kullanılabilir. Xamarin.Forms olacak anahtar fark _kalıcı_ uygulama askıya alınır ve onu yeniden olduğunda bunları yeniden bir koleksiyonda depolanan herhangi bir basit türü. WPF otomatik olarak bu davranışı desteklemiyor - bunun yerine, geliştiricilerin çoğu yalıtılmış depolamadaki yararlandı veya yerleşik kullanılan `Settings` destekler.

## <a name="defining-pages-and-the-visual-tree"></a>Sayfa ve görsel ağacını tanımlama

WPF kullanan `Window` en üst düzey herhangi bir görsel öğe için kök öğesi olarak. Bu bilgileri görüntülemek için Windows dünyada bir HWND tanımlar. Oluşturabilir ve aynı anda WPF içinde dilediğiniz sayıda windows görüntüler.

Xamarin.Forms içinde üst düzey görsel her zaman - örneğin ios'ta platform tarafından tanımlanan, bir `UIWindow`. Xamarin.Forms işler kullanarak bu yerel platform gösterimleri içerikle bir `Page` sınıfı. Her `Page` bir anda görülebilir yalnızca Xamarin.Forms içinde bir benzersiz "sayfasını" uygulamasının nerede temsil eder.

Her iki WPFs `Window` ve Xamarin.Forms `Page` dahil bir `Title` görüntülenen başlığı ve her ikisi de etkilemek için özelliğine sahip bir `Icon` özellik sayfası için özel bir simge görüntülemek için (**Not** , Başlık ve simge her zaman Xamarin.Forms içinde görünür değildir). Ayrıca, hem de arka plan rengi ya da görüntü gibi ortak görsel özelliklerini değiştirebilirsiniz.

İçin iki ayrı platform görünüm işlemek teknik olarak mümkündür (örneğin iki tanımlamak `UIWindow` nesneleri ve bir dış görüntülemek ya da AirPlay için ikinci bir işleme sahip), bunu yapmak için platforma özgü kod gerektirir ve doğrudan desteklenen bir özelliği değil Xamarin.Forms kendisi.

### <a name="views"></a>Görünümler

Her iki çerçeveler için görsel hiyerarşisini benzerdir. WPF WYSIWYG belgeler için kendi destek nedeniyle biraz daha derin.

**WPF**

```
DependencyObject - base class for all bindable things
   Visual - rendering mechanics
      UIElement - common events + interactions
         FrameworkElement - adds layout
            Shape - 2D graphics
            Control - interactive controls
```

**Xamarin.Forms**

```
BindableObject - base class for all bindable things
   Element - basic parent/child support + resources + effects
      VisualElement - adds visual rendering properties (color, fonts, transforms, etc.)
         View - layout + gesture support
```

### <a name="view-lifecycle"></a>Görünüm yaşam döngüsü

Xamarin.Forms, mobil senaryolar çevresinde öncelikle odaklıdır. Bu nedenle, uygulamalardır _etkinleştirilmiş_, _askıya_, ve _yeniden_ kullanıcı bunlarla etkileşim kurdukça. Bu liste kutusundan benzer `Window` WPF uygulamasında ve bir dizi yöntem ve geçersiz kılabilir veya bu davranışı izlemek için içine kanca ilgili olaylar vardır.

| Amaç | WPF yöntemi | Xamarin.Forms yöntemi |
|--- |--- |--- |
|İlk etkinleştirme|ctor + Window.OnLoaded|ctor + Page.OnStart|
|Gösterilen|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disapearing|
|Askıya alma/kayıp odağı|Window.OnDeactivated|Page.OnSleep|
|Etkin/alınan odağı|Window.OnActivated|Page.OnResume|
|Closed|Window.OnClosing + Window.OnClosed|yok|


İki destek alt denetimleri gizleme/gösterme, WPF içinde üç durum özelliği, `IsVisible` (görünür, gizli ve daraltılmış). Xamarin.Forms içinde yalnızca görünür veya gizli aracılığıyla `IsVisible` özelliği.

### <a name="layout"></a>Düzen

Sayfa düzeni, aynı 2-WPF içinde gerçekleşen geçişi (ölçü/Düzenle) gerçekleşir. Xamarin.Forms aşağıdaki yöntemleri geçersiz kılarak sayfa düzeni bağlanabileceğiniz `Page` sınıfı:

| Yöntem | Amaç |
|--- |--- |
|OnChildMeasureInvalidated|Bir alt öğenin tercih edilen boyut değişti.|
|OnSizeAllocated|Sayfa genişliği/yüksekliğinin atanmıştır.|
|LayoutChanged olay|Düzen/sayfa boyutunu değişti.|

Orada bugün çağrılan genel düzen olay veya orada genel değil `CompositionTarget.Rendering` gibi olayı WPF içinde bulunamadı.

#### <a name="common-layout-properties"></a>Ortak yerleşim özellikleri

WPF ve her ikisi de destekleyen bir Xamarin.Forms `Margin` bir öğe etrafında denetimi aralığı için ve `Padding` denetim aralığı için _içinde_ bir öğe. Buna ek olarak, Xamarin.Forms Düzen görünümlerin birçoğu aralığı (ör. satır veya sütun) denetlemek için özelliklere sahiptir.

Ayrıca, çoğu öğelerin üst kapsayıcı içinde nasıl konumlandırılacağını etkilemek için özelliklere sahiptir:

| WPF | Xamarin.Forms | Amaç |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|Sol/Orta/sağ/Esnetme seçenekleri|
|VerticalAlignment|VerticalOptions|Üst/Orta/alt/Esnetme seçenekleri|

> [!NOTE]
> Bu özelliklerin gerçek yorumu üst kapsayıcı bağlıdır.

#### <a name="layout-views"></a>Düzen görünümleri

WPF ve Xamarin.Forms alt öğeleri konumlandırmak için düzen denetimleri kullanın. Çoğu durumda, bunlar çok birbirine yakın işlevsellik açısından aynıdır.

| WPF | Xamarin.Forms | Düzen Stili |
|--- |--- |--- |
|StackPanel|StackLayout|Soldan sağa ya da üst-alt sonsuz yığın|
|Kılavuz|Kılavuz|Tablo biçiminde (satırları ve sütunları)|
|DockPanel|yok|Pencereyi kenarlarını Yerleştir|
|Tuval|AbsoluteLayout|Piksel/koordinat konumlandırma|
|WrapPanel|yok|Yığını kaydırma|
|yok|RelativeLayout|Kural tabanlı göreli konumlandırma|

> [!NOTE]
> Xamarin.Forms desteklemiyor bir `GridSplitter`.

Her iki platform kullanmak _iliştirilmiş özellikler_ alt öğeleri üzerinde ince ayar için.

### <a name="rendering"></a>işleme

WPF ve Xamarin.Forms için işleme mekanizması önemli ölçüde farklıdır. WPF içinde ekrandaki piksel içeriği doğrudan oluşturduğunuz denetimleri işleyin. WPF tutar iki nesne grafikler (_ağaçları_) bu - temsil etmek için _mantıksal ağaç_ kodu veya XAML, içinde tanımlanan denetimleri temsil eder ve _görsel ağacı_ temsil eder olan ekranda oluşan gerçek işleme gerçekleştirilen bir görsel öğe tarafından doğrudan ya da (sanal çizim yöntem), veya XAML tanımlı aracılığıyla `ControlTemplate` olduğu değiştirilmesi veya özelleştirilmiş. Genellikle, görsel ağaç denetimleri, örtük içeriği, vb. için etiketleri kenarlıklar gibi bu içeren daha karmaşık. WPF içeren bir API kümesi (`LogicalTreeHelper` ve `VisualTreeHelper`) bu incelemek için grafik iki nesne.

Xamarin.Forms içinde denetimleri, tanımladığınız bir `Page` aslında basit veri nesneleridir. Mantıksal ağaç gösterimine benzerdir, ancak hiçbir zaman kendi içerik işleme. Bunun yerine, oldukları _veri modeli_ öğelerin işlenmesi etkiler. Gerçek işleme yapılır bir [ayrı kümesini _görsel Oluşturucu_ her denetim türü için eşleştirilir](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Bu oluşturucu platforma özgü projelerin her platforma özgü Xamarin.Forms derlemeler tarafından kaydedilir. Bir liste görebilirsiniz [burada](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md). Değiştirme veya Oluşturucu genişletme ek olarak, Xamarin.Forms için destek de vardır. [etkileri](~/xamarin-forms/app-fundamentals/effects/index.md) Platform başına temelinde yerel işleme etkilemek için kullanılabilir.

#### <a name="the-logicalvisual-tree"></a>Mantıksal/görsel ağaç

Xamarin.Forms içinde - mantıksal ağaçta yürüyebilir için açık bir API yoktur ancak yansıma aynı bilgileri almak için kullanabilirsiniz. Örneğin, [mantıksal alt öğeleri numaralandırabilirsiniz yöntemini işte](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) yansıma ile.

## <a name="graphics"></a>Grafikler

Xamarin.Forms içermez grafik sistem basit bir dikdörtgen ötesinde temelleri için (`BoxView`). 3. taraf kitaplıklar gibi içerebilir [SkiaSharp](~/graphics-games/skiasharp/index.md) platformlar arası 2B çizim almak veya [UrhoSharp](~/graphics-games/urhosharp/index.md) 3B.

## <a name="resources"></a>Kaynaklar

WPF ve Xamarin.Forms hem de kaynak ve kaynak sözlükleri kavramı vardır. Herhangi bir nesne türü yerleştirebilirsiniz bir `ResourceDictionary` bir anahtarla ve ardından arayın ile `{StaticResource}` değiştirmez, bir şeyi veya `{DynamicResource}` zamanında sözlükteki değiştirebileceğiniz şeyler için. Kullanım ve mekanikleri bir fark dışında aynıdır: Xamarin.Forms gerektirir tanımladığınız `ResourceDictionary` atamak `Resources` özelliği WPF önceden bir tane oluşturur ve sizin için atar.

Örneğin, aşağıdaki tanımına bakın:

**WPF**

```xaml
<Window.Resources>
   <Color x:Key="redColor">#ff0000</Color>
   ...
</Window.Resources>
```

**Xamarin.Forms**

```xaml
<ContentPage.Resources>
   <ResourceDictionary>
      <Color x:Key="redColor">#ff0000</Color>
      ...
   </ResourceDictionary>
</ContentPage.Resources>
```

Değil tanımlarsanız `ResourceDictionary`, bir çalışma zamanı hatası oluşturulur.

## <a name="styles"></a>Stilleri

Stilleri Xamarin.Forms içinde da tam olarak desteklenir ve olabilir UI'yi Xamarin.Forms öğeleri tema için kullanılır. Tetikleyiciler (özellik, olay ve veri) devralma yoluyla destekledikleri `BasedOn`ve kaynak aramaları için değerler. Stilleri öğelere uygulanan aracılığıyla ya da alınmalı `Style` özelliği veya - WPF gibi bir kaynak anahtarı sağlayarak değil tarafından implicitely.

### <a name="device-styles"></a>Cihaz stilleri

WPF sahip önceden tanımlanmış özellikler kümesi (gibi statik sınıf kümesi üzerinde statik bir değer olarak depolanan `SystemColors`) sistem renkleri, yazı tipleri ve ölçüm değerleri ve kaynak anahtarlarını biçiminde gerektirir. Xamarin.Forms benzer, ancak kümesini tanımlayan [cihaz stilleri](~/xamarin-forms/user-interface/styles/device.md) şeyi göstermek için. Bu stiller frameowrk tarafından sağlanan ve çalışma zamanı ortamı (ör. Erişilebilirlik) tabanlı değerlere ayarlayın.

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
