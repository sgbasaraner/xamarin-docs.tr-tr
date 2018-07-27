---
title: Xamarin.Forms içinde bağımlılık çözümlemesi
description: Bu makalede, bir uygulamanın bağımlılık ekleme kapsayıcısını özel oluşturucular, efektleri ve DependencyService uygulamaları ömrünü ve yapı üzerinde denetime sahip olacak şekilde bir bağımlılık çözümleme yöntemi Xamarin.Forms ekleme açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/23/2018
ms.openlocfilehash: 2379c8ddc4bea6dd97bc4febd055dd8dfef39beb
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270494"
---
# <a name="dependency-resolution-in-xamarinforms"></a>Xamarin.Forms içinde bağımlılık çözümlemesi

_Bu makalede, bir uygulamanın bağımlılık ekleme kapsayıcısını özel oluşturucular, efektleri ve DependencyService uygulamaları ömrünü ve yapı üzerinde denetime sahip olacak şekilde bir bağımlılık çözümleme yöntemi Xamarin.Forms ekleme açıklanmaktadır. Kod örnekleri alınmıştır [bağımlılık çözümlemesi](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/) örnek._

Model-View-ViewModel (MVVM) desenini kullanan Xamarin.Forms uygulaması bağlamında bir bağımlılık ekleme kapsayıcısını ve Hizmetleri kaydetme ve bunları görünümü modellerini ekleme kaydetme ve çözümleme görünüm modelleri için kullanılabilir. Görünüm modeli oluşturma sırasında gerekli olan herhangi bir bağımlılığın kapsayıcıya ekler. Bu bağımlılıkların oluşturulmadı, kapsayıcı oluşturur ve ilk bağımlılıklarını çözümler. Görünüm modellerini bağımlılıkları ekleme örnekleri dahil olmak üzere, bağımlılık ekleme hakkında daha fazla bilgi için bkz. [bağımlılık ekleme](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

Oluşturma üzerinde denetim ve platformu projelerinde türlerinde ömrünü kullanan Xamarin.Forms tarafından geleneksel olarak gerçekleştirilen `Activator.CreateInstance` özel oluşturucular, efektler örneklerini oluşturmak için gereken yöntemini ve [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) uygulamaları. Ne yazık ki bu oluşturulmasını ve yaşam süresini bu türleri ve bunları bağımlılıkları ekleme yeteneği üzerinde Geliştirici kontrolünü sınırlar. Ancak, bu davranış, bir bağımlılık çözümleme yöntemi Xamarin.Forms nasıl türleri oluşturulur: denetleyen uygulamanın bağımlılık ekleme kapsayıcısını veya Xamarin.Forms ekleyerek değiştirilebilir.

> [!NOTE]
> Xamarin.Forms içinde bir bağımlılık çözümleme yöntemi eklemesine gereksinimi yoktur. Xamarin.Forms oluşturmak ve bir bağımlılık çözümleme yöntemi eklenen değil, platform proje türlerinde ömrünü yönetmek devam eder.

## <a name="injecting-a-dependency-resolution-method"></a>Bağımlılık çözümlemesi yöntemi ekleme

[ `DependencyResolver` ](xref:Xamarin.Forms.Internals.DependencyResolver) Sınıfı kullanarak Xamarin.Forms, bir bağımlılık çözümleme yöntemi ekleme olanağı sağlar [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) yöntemleri. Ardından, Xamarin.Forms belirli bir türün bir örneği gerektiğinde, bağımlılık çözümleme yöntemi örneği sunma fırsatı verilir. Bağımlılık çözümlemesi yöntemi döndürürse `null` istenen bir tür için tür oluşturulmaya çalışılırken için geri döner Xamarin.Forms örnek kendisi `Activator.CreateInstance` yöntemi.

Aşağıdaki örnek bağımlılık çözümlemesi yöntemle gösterilmiştir [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) yöntemi:

```csharp
using Autofac;
using Xamarin.Forms.Internals;
...

public partial class App : Application
{
    // IContainer and ContainerBuilder are provided by Autofac
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();

    public App()
    {
        ...
        DependencyResolver.ResolveUsing(type => container.IsRegistered(type) ? container.Resolve(type) : null);
        ...
    }
    ...
}
```

Bu örnekte, kapsayıcı ile kaydedilmiş tüm türleri çözümlemek için Autofac bağımlılık ekleme kapsayıcısını kullanan bir lambda ifadesine bağımlılık çözümleme yöntemi ayarlanır. Aksi takdirde `null` , türü çözümlemeye Xamarin.Forms içinde neden olacak döndürülür.

> [!NOTE]
> Bir bağımlılık ekleme kapsayıcısını tarafından kullanılan API'nin kapsayıcıya özeldir. Bu makaledeki kod örnekleri sağlayan bir bağımlılık ekleme kapsayıcısını Autofac kullanmak `IContainer` ve `ContainerBuilder` türleri. Alternatif bir bağımlılık ekleme kapsayıcıları eşit oranda kullanılabilir, ancak burada sunulan daha farklı API'leri kullanmanız gerekir.

Bağımlılık çözümlemesi yöntemi uygulama başlangıcında ayarlamak için bir gereksinim olduğuna dikkat edin. Herhangi bir zamanda ayarlanabilir. Xamarin.Forms uygulama bağımlılık ekleme kapsayıcısına kayıtlı türleri kullanma girişiminde zamanına göre bağımlılık çözümleme yöntemi hakkında bilmeniz gereken tek sınırlamadır. Bu nedenle, uygulama başlatma sırasında gerektirecek bağımlılık ekleme kapsayıcısını Hizmetleri varsa bağımlılık çözümleme yöntemi erken uygulamanın yaşam döngüsünde ayarlanması gerekir. Benzer şekilde, bağımlılık ekleme kapsayıcısını oluşturulmasını ve belirli bir yaşam süresini yöneten [ `Effect` ](xref:Xamarin.Forms.Effect), Xamarin.Forms bir görünüm oluşturmak çalışmadan önce bağımlılık çözümleme yöntemi hakkında bilmeniz gerekir, kullanan `Effect`.

> [!WARNING]
> Kaydetme ve türleri ile bir bağımlılık ekleme kapsayıcısını çözme Performans nedeniyle kapsayıcının her tür oluşturma için yansıma kullanmak, özellikle bağımlılıkları uygulamadaki her sayfa gezintisi için yeniden durumunda maliyetine sahiptir. Birçok veya ayrıntılı bağımlılıkları varsa, oluşturma maliyetini önemli ölçüde artırabilir.

## <a name="registering-types"></a>Türlerini kaydetme

Bağımlılık çözümlemesi yöntemiyle, çözmeden önce türleri ile bağımlılık ekleme kapsayıcısına kayıtlı olması gerekir. Aşağıdaki kod örneği, örnek uygulamayı gösterir kayıt yöntemleri gösterir. `App` Autofac kapsayıcısı için bir sınıf:

```csharp
using Autofac;
using Autofac.Core;
...

public partial class App : Application
{
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();
    ...

    public static void RegisterType<T>() where T : class
    {
        builder.RegisterType<T>();
    }

    public static void RegisterType<TInterface, T>() where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>().As<TInterface>();
    }

    public static void RegisterTypeWithParameters<T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where T : class
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        });
    }

    public static void BuildContainer()
    {
        container = builder.Build();
    }
    ...
}
```

Bir uygulama bir kapsayıcı türlerinden çözmek için bir bağımlılık çözümleme yöntemi kullandığında, türü kayıtlar genellikle platform projelerden gerçekleştirilir. Bu özel oluşturucular, efektler türleri kaydedilecek platformu projelerinde sağlar ve [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) uygulamaları.

Kayıt türü bir platform projesinde aşağıdaki `IContainer` nesne gerekir oluşturulur, hangi çağrılarak gerçekleştirilir `BuildContainer` yöntemi. Autofac'ın bu metodu çağıran `Build` metodunda `ContainerBuilder` örneğini yapılan kayıtları içeren yeni bir bağımlılık ekleme kapsayıcısını oluşturur.

İçinde aşağıdaki bölümlerde bir `Logger` uygulayan sınıf `ILogger` arabirim içinde sınıf oluşturucuları eklenmiş. `Logger` Sınıfının Implements basit günlüğe kaydetme işlevini kullanarak `Debug.WriteLine` yöntemi ve Hizmetleri özel oluşturucular, efektler nasıl yerleştirilebilir göstermek için kullanılır ve [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) uygulamaları.

### <a name="registering-custom-renderers"></a>Özel oluşturucular kaydediliyor

Örnek uygulamayı web videolar, XAML kaynak aşağıdaki örnekte gösterilen çalan bir sayfa içerir:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

`VideoPlayer` Görünümü her platformda tarafından gerçekleştirilen bir `VideoPlayerRenderer` videoyu oynatmak için işlevsellik sağlayan sınıf. Bu özel Oluşturucu sınıfları hakkında daha fazla bilgi için bkz. [bir video oynatıcıyı uygulama](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md).

İOS ve evrensel Windows Platformu (UWP) `VideoPlayerRenderer` sınıfları gerektiren aşağıdaki oluşturucuya sahip bir `ILogger` bağımsız değişkeni:

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Tarafından gerçekleştirilen tüm üç platformlarda bağımlılık ekleme kapsayıcısını türü kaydı `RegisterTypes` uygulamayla yüklenirken platformu önce çağrılan yöntemi `LoadApplication(new App())` yöntemi. Aşağıdaki örnekte gösterildiği `RegisterTypes` iOS platformunda yöntemi:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

Bu örnekte, `Logger` somut bir türde da arabirimi türünün karşı bir eşleme aracılığıyla kaydedilir ve `VideoPlayerRenderer` türü bir arabirim eşleme olmadan doğrudan kaydedilir. Kullanıcı ne zaman gittiğinde içeren sayfa `VideoPlayer` görünümü, çözmek için bağımlılık çözümleme yöntemi çağrılır `VideoPlayerRenderer` ayrıca çözmek ve ekleme bağımlılık ekleme kapsayıcısını türünden `Logger` içine yazın `VideoPlayerRenderer` Oluşturucusu.

`VideoPlayerRenderer` Android platformunda Oluşturucusu gerektiriyor biraz daha karmaşık bir `Context` ek bağımsız değişken `ILogger` bağımsız değişkeni:

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Aşağıdaki örnekte gösterildiği `RegisterTypes` Android platformunda yöntemi:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

Bu örnekte, `App.RegisterTypeWithParameters` yöntemi kayıtları `VideoPlayerRenderer` bağımlılık ekleme kapsayıcısına sahip. Kayıt yöntemi, sağlar `MainActivity` örneği eklenmiş olarak `Context` bağımsız değişkeni ve `Logger` türü eklenmiş olarak `ILogger` bağımsız değişken.

### <a name="registering-effects"></a>Etkileri kaydediliyor

Örnek uygulamayı sürüklemek için etkili izleme bir touch'ı kullanan bir sayfa içeren [ `BoxView` ](xref:Xamarin.Forms.BoxView) sayfada örnekleri. [ `Effect` ](xref:Xamarin.Forms.Effect) Eklenir `BoxView` aşağıdaki kodu kullanarak:

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

`TouchEffect` Sınıfı bir [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) her platformda tarafından uygulanan bir `TouchEffect` , sınıf sahip bir `PlatformEffect`. Platform `TouchEffect` sınıfı sürüklemek için işlevsellik sağlayan `BoxView` sayfasında. Bu etkin sınıflar hakkında daha fazla bilgi için bkz. [etkileri olayları çağırma](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Tüm platformlarda üç `TouchEffect` sınıfında aşağıdaki oluşturucuyu gerektiren bir `ILogger` bağımsız değişkeni:

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Tarafından gerçekleştirilen tüm üç platformlarda bağımlılık ekleme kapsayıcısını türü kaydı `RegisterTypes` uygulamayla yüklenirken platformu önce çağrılan yöntemi `LoadApplication(new App())` yöntemi. Aşağıdaki örnekte gösterildiği `RegisterTypes` Android platformunda yöntemi:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

Bu örnekte, `Logger` somut bir türde da arabirimi türünün karşı bir eşleme aracılığıyla kaydedilir ve `TouchEffect` türü bir arabirim eşleme olmadan doğrudan kaydedilir. Kullanıcı ne zaman gittiğinde sayfayı içeren bir [ `BoxView` ](xref:Xamarin.Forms.BoxView) olan örneği `TouchEffect` bağlı, bağımlılık çözümleme yöntemi platformu çözmek için çağrılacak `TouchEffect` bağımlılık türü Ayrıca çözmek ve Ekle ekleme kapsayıcı `Logger` yazdığınız `TouchEffect` Oluşturucusu.

### <a name="registering-dependencyservice-implementations"></a>DependencyService uygulamaları kaydetme

Örnek uygulamayı kullanan bir sayfa içeren [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) kullanıcının cihazın Resim Kitaplığı'ndan bir fotoğraf izin vermek için her platformda uygulamalar. `IPhotoPicker` Arabirimi tarafından uygulanan işlevleri tanımlar `DependencyService` uygulamaları ve aşağıdaki örnekte gösterilmiştir:

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

Her platform projesinde `PhotoPicker` sınıfının Implements `IPhotoPicker` platform API'leri kullanarak arabirimi. Bu bağımlılık hizmetleri hakkında daha fazla bilgi için bkz. [Resim Kitaplığı'ndan bir fotoğraf çekme](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Tüm platformlarda üç `PhotoPicker` sınıfında aşağıdaki oluşturucuyu gerektiren bir `ILogger` bağımsız değişkeni:

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Tarafından gerçekleştirilen tüm üç platformlarda bağımlılık ekleme kapsayıcısını türü kaydı `RegisterTypes` uygulamayla yüklenirken platformu önce çağrılan yöntemi `LoadApplication(new App())` yöntemi. Aşağıdaki örnekte gösterildiği `RegisterTypes` UWP yöntemi:

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

Bu örnekte, `Logger` somut bir türde da arabirimi türünün karşı bir eşleme aracılığıyla kaydedilir ve `PhotoPicker` türü bir arabirim eşlemesi aracılığıyla da kayıtlı. Kullanıcı fotoğraf çekme sayfasına gider ve bir fotoğraf seçmek seçtiği `OnSelectPhotoButtonClicked` işleyicisi yürütülür:

```csharp
async void OnSelectPhotoButtonClicked(object sender, EventArgs e)
{
    ...
    var photoPickerService = DependencyService.Resolve<IPhotoPicker>();
    var stream = await photoPickerService.GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }
    ...
}
```

Zaman [ `DependencyService.Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) yöntemi çağrıldığında, bağımlılık çözümleme yöntemi çözmek için çağrılacak `PhotoPicker` ayrıca çözmek ve ekleme bağımlılık ekleme kapsayıcısını türünden `Logger` türü içine `PhotoPicker` Oluşturucusu.

> [!NOTE]
> [ `Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) Yöntemi kullanılmalıdır, uygulamanın bağımlılık ekleme kapsayıcısını türünden çözülürken [ `DependencyService` ](xref:Xamarin.Forms.DependencyService).

## <a name="related-links"></a>İlgili bağlantılar

- [Bağımlılık çözümlemesi (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/)
- [Bağımlılık ekleme](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [Bir video oynatıcıyı uygulama](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [Etkileri olayları çağırma](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [Resim Kitaplığı'ndan bir fotoğraf çekme](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
