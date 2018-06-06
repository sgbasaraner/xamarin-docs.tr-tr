---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF vs. Xamarin.Forms uygulama yaşam döngüsü
description: Bu belge karşılaştırır Xamarin.Forms ve WPF uygulamaları için uygulama yaşam döngüsü arasındaki farklar ve Benzerlikler. Ayrıca görsel ağaç, grafik, kaynakları ve stiller görünüyor.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: abb7773873fa181085464b5985cc8233715cc4be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781588"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF vs. Xamarin.Forms uygulama yaşam döngüsü

Xamarin.Forms çok fazla zaman önce özellikle WPF XAML tabanlı çerçeveleri tasarım kılavuzluğu alır. Ancak, diğer yollarla bunu önemli ölçüde ele geçirme girişiminde kişiler için Yapışkan noktası olabilen farklıdır. Bu belgede, bu sorunlardan bazıları tanımlamak ve mümkün olduğunda köprüsü WPF bilgi Xamarin.Forms için kılavuzluk dener.

## <a name="app-lifecycle"></a>Uygulama yaşam döngüsü

Uygulama yaşam döngüsü WPF Xamarin.Forms arasındaki benzer. Hem dış (platform) kodda başlatın ve bir yöntem çağrısı aracılığıyla kullanıcı arabirimini Başlat. Xamarin.Forms her zaman, daha sonra başlatır ve uygulama için kullanıcı Arabirimi oluşturur platforma özgü derlemede başladığını fark vardır.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main` Yöntemidir, varsayılan olarak, otomatik oluşturulan ve kodda görünmez.

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Uygulama sınıfı

WPF ve Xamarin.Forms sahip bir `Application` bir singleton olarak oluşturulan sınıf. Bu kesinlikle WPF'de gerekli değildir ancak çoğu durumda, uygulamaları özel bir uygulama sağlamak için bu sınıfından türetilir. Her ikisi de kullanıma bir `Application.Current` oluşturulan singleton bulmak için özellik.

### <a name="global-properties--persistence"></a>Genel Özellikler + kalıcılığı

WPF ve Xamarin.Forms sahip bir `Application.Properties` uygulama herhangi bir yerden erişilebilir genel uygulama düzeyinde nesneler depolayabileceğiniz sözlük kullanılabilir. Bu Xamarin.Forms anahtar farktır _kalıcı_ uygulama askıya alınır ve onu yeniden olduğunda bunları yeniden koleksiyonda depolanan tüm ilkel türler. WPF otomatik olarak bu davranışı desteklemiyor - bunun yerine, çoğu Geliştirici yalıtılmış depolamadaki dayanıyordu veya yerleşik kullanılan `Settings` destekler.

## <a name="defining-pages-and-the-visual-tree"></a>Sayfalar ve görsel ağaç tanımlama

WPF kullanan `Window` en üst düzey bir görsel öğe için kök öğesi olarak. Bu bilgileri görüntülemek için Windows dünyasında HWND tanımlar. Oluşturun ve WPF içinde aynı anda istediğiniz kadar pencere görüntüleyin.

Xamarin.Forms içinde en üst düzey visual her zaman platformda - örneğin iOS tarafından tanımlanan, bir `UIWindow`. Xamarin.Forms işler kullanarak bu yerel platform Beyanları içerik olduğu bir `Page` sınıfı. Her `Page` aynı anda görünür, yalnızca birisi Xamarin.Forms içinde benzersiz bir "Sayfa" uygulamasında, burada temsil eder.

Her iki WPFs `Window` ve Xamarin.Forms `Page` içeren bir `Title` görüntülenen Başlık ve her ikisi de etkilemek için özelliğine sahip bir `Icon` özellik sayfası için belirli bir simge görüntülemek için (**Not** , Başlık ve simge her zaman içinde Xamarin.Forms görünür değildir). Ayrıca, arka plan rengini veya görüntü gibi her iki ortak görsel özelliklerini değiştirebilirsiniz.

İki ayrı platform görünüm işlemek teknik mümkündür (örneğin iki tanımlamak `UIWindow` nesneleri ve bir dış görüntü veya AirPlay ikinci bir işleme sahip), bunu yapmak için platforma özgü kodu gerektirir ve doğrudan desteklenen özelliği değil Xamarin.Forms kendisi.

### <a name="views"></a>Görünümler

Her iki çerçeveler için görsel hiyerarşisini benzer. WPF, WYSIWYG belgeleri desteği nedeniyle biraz daha derin.

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

### <a name="view-lifcycle"></a>Görünüm Lifcycle

Xamarin.Forms mobil senaryolarda birincil olarak kaynaklı. Bu nedenle, uygulamalardır _etkinleştirilmiş_, _askıya_, ve _yeniden_ kullanıcı ile etkileşime giren gibi. Bu merkezden benzer `Window` bir WPF uygulamasında ve bir dizi yöntem ve geçersiz kılma veya bu davranışı izlemek için içine kanca ilgili olaylar vardır.

| Amaç | WPF yöntemi | Xamarin.Forms yöntemi |
|--- |--- |--- |
|İlk etkinleştirme|ctor + Window.OnLoaded|ctor + Page.OnStart|
|Gösterilen|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disapearing|
|Askıya alma kayıp odak|Window.OnDeactivated|Page.OnSleep|
|Etkinleştirilen eşitlenemiyor odak|Window.OnActivated|Page.OnResume|
|Closed|Window.OnClosing + Window.OnClosed|yok|


Her iki destek alt denetimleri gizleme/gösterme, bir üç durumlu özellik olduğundan WPF'de `IsVisible` (görünür, gizli ve daraltılmış). Xamarin.Forms içinde yalnızca görünür mü yoksa gizli aracılığıyla olmasından `IsVisible` özelliği.

### <a name="layout"></a>Düzen

Sayfa düzeni aynı 2-WPF içinde gerçekleşen geçişi (ölçü/Düzenle) oluşur. Xamarin.Forms aşağıdaki yöntemleri geçersiz kılarak sayfa düzeni kanca `Page` sınıfı:

| Yöntem | Amaç |
|--- |--- |
|OnChildMeasureInvalidated|Bir alt tercih edilen boyut değişti.|
|OnSizeAllocated|Sayfa genişliği/yüksekliğinin atanmıştır.|
|LayoutChanged olayı|Düzen/sayfa boyutunu değişti.|

Var. bugün denir genel düzeni olay kaydedilmez veya genel olduğundan `CompositionTarget.Rendering` olayı gibi WPF içinde bulunamadı.

#### <a name="common-layout-properties"></a>Ortak yerleşim özellikleri

WPF ve hem Xamarin.Forms `Margin` denetimi aralığı geçici bir öğe için ve `Padding` denetimi aralığı için _içinde_ bir öğe. Ayrıca, Xamarin.Forms Düzen görünümleri çoğunu aralığını (örn. satır veya sütun) denetlemek için özellikler vardır.

Ayrıca, öğelerin çoğu üst kapsayıcısında nasıl konumlandırılacağını etkilemek için özelliklere sahiptir:

| WPF | Xamarin.Forms | Amaç |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|Sol/Orta/sağ/Esnetme seçenekleri|
|VerticalAlignment|VerticalOptions|Üst/Orta/alt/Esnetme seçenekleri|

> [!NOTE]
> Bu özellikler gerçek yorumu üst kapsayıcı bağlıdır.

#### <a name="layout-views"></a>Düzen görünümleri

WPF ve Xamarin.Forms alt öğeleri yerleştirmek için düzen denetimleri kullanın. Çoğu durumda, çok diğer yakın işlevselliği açısından bunlar.

| WPF | Xamarin.Forms | Düzen Stili |
|--- |--- |--- |
|StackPanel|StackLayout|Soldan sağa ya da üst-alt sonsuz yığınlama|
|Kılavuz|Kılavuz|Tablo biçiminde (satırları ve sütunları)|
|DockPanel|yok|Pencere kenarlarına yerleştirme|
|Tuval|AbsoluteLayout|Piksel/koordinat konumlandırma|
|WrapPanel|yok|Yığın sarmalama|
|yok|RelativeLayout|Kural tabanlı göreli konumlandırma|

> [!NOTE]
> Xamarin.Forms desteklemiyor bir `GridSplitter`.

Her iki platformları kullanan _ekli özellikler_ alt ince ayar yapmak için.

### <a name="rendering"></a>İşleme

WPF ve Xamarin.Forms için işleme mekanizması tüketicisinin farklıdır. WPF içinde ekrandaki piksel içeriği doğrudan oluşturduğunuz denetimlerini işlemeye. WPF tutar iki nesne grafikleri (_ağaçları_) bu - temsil etmek için _mantıksal ağacının_ kodu veya XAML tanımlanan denetimleri temsil eder ve _görsel ağaç_ temsil eder olan ekranda oluşan gerçek işleme gerçekleştirilen bir görsel öğe tarafından doğrudan ya da (sanal çizim yöntem), ya da XAML tanımlı aracılığıyla `ControlTemplate` hangi değiştirilebilir veya özelleştirilmiş. Genellikle, bu denetimleri, örtük içerik, vb. için etiketleri kenarlıklar gibi içerdiği daha karmaşık görsel ağaç ' dir. WPF içeren bir dizi API (`LogicalTreeHelper` ve `VisualTreeHelper`) iki nesne grafikleri bunları inceleyin.

Xamarin.Forms içinde denetimleri içinde tanımladığınız bir `Page` gerçekten yalnızca basit veri nesneleridir. Mantıksal ağacının gösterimine benzerdir, ancak hiçbir zaman kendi içerik işleme. Bunun yerine, oldukları _veri modeli_ öğelerin işlenmesi etkiler. Asıl işlemesi yapılan bir [ayrı kümesi _visual Oluşturucu_ her denetim türü için eşlendi](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Bu oluşturucu her platforma özgü projeleri platforma özgü Xamarin.Forms derlemeleri tarafından kaydedilir. Listesini görebilirsiniz [burada](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md). Değiştirme veya Oluşturucu genişletme yanı sıra Xamarin.Forms desteği de vardır. [etkileri](~/xamarin-forms/app-fundamentals/effects/index.md) Platform başına temelinde yerel işleme etkilemek için kullanılabilir.

#### <a name="the-logicalvisual-tree"></a>Mantıksal/Visual ağacı

Xamarin.Forms içinde - mantıksal ağacının yürütmek için sunulan hiçbir API yoktur ancak yansıma aynı bilgileri almak için kullanabilirsiniz. Örneğin, [mantıksal alt numaralandırabilir bir yöntem işte](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) yansıma ile.

## <a name="graphics"></a>Grafikler

Xamarin.Forms içermez grafik sistemi için basit bir dikdörtgen ötesinde temelleri (`BoxView`). 3 taraf kitaplıklar gibi içeren [SkiaSharp](~/graphics-games/skiasharp/index.md) platformlar arası 2B çizim almak için veya [UrhoSharp](~/graphics-games/urhosharp/index.md) 3B.

## <a name="resources"></a>Kaynaklar

WPF ve Xamarin.Forms hem de kaynaklar ve kaynak sözlükleri kavramı vardır. Herhangi bir nesne türünün yerleştirebilirsiniz bir `ResourceDictionary` bir anahtarla ve ardından arayın ile `{StaticResource}` hangi değiştirmez, herhangi bir şeyi veya `{DynamicResource}` çalışma zamanında sözlükte değiştirebileceğiniz şeyler için. Kullanım ve mekanizması farklardan biri ile aynıdır: Xamarin.Forms gerektirir, tanımladığınız `ResourceDictionary` atamak için `Resources` özelliği WPF biri önceden oluşturur ve sizin için atar ancak.

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

Stilleri Xamarin.Forms içinde tam olarak desteklenir ve olabilir UI'yi olun Xamarin.Forms öğeleri tema için kullanılır. Tetikleyiciler (özelliği, olay ve veri) devralma yoluyla destekledikleri `BasedOn`ve değerler için kaynak aramalarını. Stilleri öğelerine uygulanan aracılığıyla ya da explicitely `Style` özelliği veya implicitely sağlayarak - WPF gibi bir kaynak anahtarı değil.

### <a name="device-styles"></a>Cihaz stilleri

WPF sahip bir dizi önceden tanımlanmış özellikler (statik sınıflar kümesi üzerinde statik değerler olarak gibi depolanan `SystemColors`) sistem renkleri, yazı tipleri ve değerlerin ve kaynak anahtarları biçiminde ölçümleri dikte. Xamarin.Forms benzer, ancak bir dizi tanımlar [aygıt stilleri](~/xamarin-forms/user-interface/styles/device.md) aynısını temsil etmek için. Bu stiller frameowrk tarafından sağlanan ve çalışma zamanı ortamı (örneğin Erişilebilirlik) tabanlı değerlere ayarlayın.

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
