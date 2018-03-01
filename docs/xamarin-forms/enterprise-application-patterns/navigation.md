---
title: Gezinme
ms.topic: article
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: a37773b666e015277d2fecc103066e82b6f7f108
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="navigation"></a>Gezinme

Xamarin.Forms genellikle kullanıcı Arabirimi ile kullanıcı etkileşimi veya uygulamanın kendi iç mantığı temelli durumu yapılan değişiklikler sonucunda sonuçları sayfa gezintisi için destek içerir. Ancak, gezinti aşağıdaki sorunları karşılanması gereken Model View ViewModel (MVVM) desenini kullanan uygulamalarda uygulama karmaşık olabilir:

-   Görünüm için gittiğinizde sıkı bağ ve görünümler arasındaki bağımlılıkları sunmaz bir yaklaşım kullanarak belirleme.
-   Kullandığı için gittiğinizde görünüme örneği ve başlatılan işlemi koordine etme. MVVM kullanırken, görünümü ve görünüm modeli örneği ve birbirleri ile görünümün bağlama bağlamı ile ilişkilendirilmiş olması gerekir. Bir uygulama bir bağımlılık ekleme kapsayıcısını kullanırken, görünümleri ve görünüm modelleri örneklemesi belirli yapım mekanizması gerektirebilir.
-   Verilip Görünüm ilk Gezinti gerçekleştirmek veya model first Gezinti görüntüleyin. Görünüm ilk Gezinti ile gitmek için sayfanın görünüm türü adını gösterir. Gezinti sırasında belirtilen görünümü, karşılık gelen kendi görünüm modeli ve diğer bağımlı hizmetlerin yanı sıra örneği. Alternatif bir yaklaşım, burada gitmek için sayfa görünümü model türünün adını başvuruyor görünüm modeli ilk Gezinti kullanmaktır.
-   Nasıl için düzgün bir şekilde uygulama gezinme davranışını görünümleri ve görünüm modelleri arasında ayırın. MVVM düzeni uygulamanın kullanıcı Arabirimi ve sunu ve iş mantığı arasında ayrım sağlar. Ancak, bir uygulama gezinti davranışını genellikle uygulama kullanıcı Arabirimi ve sunular bölümlerini span. Kullanıcı genellikle bir görünümden Gezinti başlatır ve görünümü Gezinti sonucu olarak değiştirilecek. Ancak, gezinti genellikle başlatılan veya gelen içinde görünüm modeli Eşgüdümlü etmeniz gerekebilir.
-   Başlatma amacıyla gezinti sırasında parametreleri geçirmek nasıl. Örneğin, kullanıcı sipariş ayrıntılarını güncelleştirmek için bir görünüm giderse, sipariş verilerini doğru veri görüntüleyebilmesi görünüme iletilen gerekir.
-   Belirli iş kurallarını obeyed emin olmak için birlikte ordinat Gezinti nasıl. Örneğin, kullanıcılar, böylece tüm geçersiz veriler düzeltebilir veya gönderin veya görünümde yapılan tüm veri değişiklikleri atmak için sizden bir görünümden gitmeden önce istenebilir.

Bu bölümde sunarak bu sorunlarını ele alır bir `NavigationService` görünüm modeli ilk sayfa gezintisi gerçekleştirmek için kullanılan sınıf.

> [!NOTE]
> `NavigationService` Tarafından kullanılan uygulama yalnızca ContentPage örnekleri arasında hiyerarşik Gezinti gerçekleştirmek üzere tasarlanmıştır. Diğer sayfa türleri arasında gezinmek için hizmet kullanarak beklenmeyen davranışlara neden.

## <a name="navigating-between-pages"></a>Sayfaları arasında gezinme

Gezinti mantığı bir görünümün kod arkasında bulunabilir veya bir veri görünüm modeli bağlı. Bir görünümde Gezinti mantığı yerleştirme en kolay yaklaşım olabilir ancak birim testleri kolayca sınanabilir değil. Gezinti mantığı görünümde yerleştirme modeli sınıfları gelir mantığı ile birim testleri denenmesi. Ayrıca, görünüm modeli sonra belirli iş kuralları zorunlu tutulmaz emin olmak için Denetim Gezinti mantığı uygulayabilirsiniz. Örneğin, bir uygulamayı başka bir sayfa ilk olmadan bir sayfaya geçemezsiniz kullanıcıya izin vermeyebilir girilen verilerin geçerli olduğundan emin olmanın.

A `NavigationService` sınıfı Test Edilebilirlik yükseltmek için Görünüm modellerinden genellikle çağrılır. Başvuru görünümleri ve özellikle etkin görünüm modeli hangi önerilmez, ilişkili olmayan görünümleri için Görünüm modelleri görünüm modellerinden görünümlerine gezinme ancak gerektirir. Bu nedenle, `NavigationService` sunulan burada gitmek için hedef olarak görünüm model türünü belirtir.

EShopOnContainers mobil uygulamanın kullandığı `NavigationService` görünüm modeli ilk Gezinti sağlamak için sınıf. Bu sınıf uygulayan `INavigationService` aşağıdaki kod örneğinde gösterildiği arabirimi:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

Bu arabirimi uygulayan bir sınıf aşağıdaki yöntemleri sağlamalısınız belirtir:

<table>
<thead>
<tr class="header">
<th>Yöntem</th>
<th>Amaç</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>InitializeAsync</code></td>
<td>Uygulama başlatıldığında bir iki sayfa gezinti gerçekleştirir.</td>
</tr>
<tr class="even">
<td><code>NavigateToAsync<T></code></td>
<td>Belirtilen sayfa için hiyerarşik Gezinti gerçekleştirir.</td>
</tr>
<tr class="odd">
<td><code>NavigateToAsync<T>(parameter)</code></td>
<td>Parametre geçirme belirtilen bir sayfaya hiyerarşik Gezinti gerçekleştirir.</td>
</tr>
<tr class="even">
<td><code>RemoveLastFromBackStackAsync</code></td>
<td>Önceki sayfaya gezinti yığından kaldırır.</td>
</tr>
<tr class="odd">
<td><code>RemoveBackStackAsync</code></td>
<td>Önceki tüm sayfaları Gezinti yığından kaldırır.</td>
</tr>
</tbody>
</table>

Ayrıca, `INavigationService` arabirimi belirtir uygulayan sınıfa sağlamalısınız bir `PreviousPageViewModel` özelliği. Bu özellik önceki sayfa gezinti yığınında ilişkili görünüm model türü döndürür.

> [!NOTE]
> Bir `INavigationService` arabirimi misiniz genellikle de belirtmeniz bir `GoBackAsync` program aracılığıyla gezinti yığını önceki sayfaya dönmek için kullanılan yöntem. Ancak, gerekli değildir çünkü bu yöntem eShopOnContainers mobil uygulamadan eksik.

### <a name="creating-the-navigationservice-instance"></a>NavigationService örneği oluşturma

`NavigationService` Sınıfı, hangi uygulayan `INavigationService` arabirimi, Autofac bağımlılık ekleme kapsayıcısını ile singleton olarak aşağıdaki kod örneğinde gösterildiği şekilde kayıtlı:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` Arabirimi Çözüldü `ViewModelBase` sınıfı oluşturucusu, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Bu bir başvuru döndürür `NavigationService` tarafından oluşturulan Autofac bağımlılık ekleme kapsayıcısını depolanan nesne `InitNavigation` yönteminde `App` sınıfı. Daha fazla bilgi için bkz: [gezinme, uygulama başlatıldığında](#navigating_when_the_app_is_launched).

`ViewModelBase` Sınıf depoları `NavigationService` örneğini bir `NavigationService` türündeki özelliği `INavigationService`. Bu nedenle, tüm öğesinden türetilen modeli sınıfları görüntüle `ViewModelBase` sınıfı, kullanabileceğiniz `NavigationService` özelliği tarafından belirtilen yöntemleri erişmek için `INavigationService` arabirimi. Bu injecting yükünü önler `NavigationService` her görünüm model sınıfı Autofac bağımlılık ekleme kapsayıcısına nesnesinden.

### <a name="handling-navigation-requests"></a>Gezinti istekleri işleme

Xamarin.Forms sağlar [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) kullanıcının olduğu iletir ve geriye doğru istediğiniz gibi sayfalar arasında gezinmek için bir hiyerarşik gezinme deneyimi uygulayan sınıfı. Hiyerarşik Gezinti hakkında daha fazla bilgi için bkz: [hiyerarşik Gezinti](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Yerine kullanmak [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) doğrudan eShopOnContainers uygulama sarmalayan sınıf `NavigationPage` sınıfını `CustomNavigationView` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

Stil kolaylığı için bu kaydırma amacı [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) örneğinin sınıfı için XAML dosyası içinde.

Gezinti birini çağırarak görünüm modeli sınıfları içinde gerçekleştirilir `NavigateToAsync` yöntemleri için aşağıdaki kod örneğinde gösterildiği şekilde gittiğinizde sayfa için Görünüm model türü belirtme:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

Aşağıdaki örnekte gösterildiği kod `NavigateToAsync` tarafından sağlanan yöntemleri `NavigationService` sınıfı:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Türetilen herhangi bir görünüm modeli sınıf her bir yöntem sağlar `ViewModelBase` çağırarak hiyerarşik Gezinti gerçekleştirmek için sınıf `InternalNavigateToAsync` yöntemi. Ayrıca, ikinci `NavigateToAsync` yöntemi için genellikle başlatılmasını gerçekleştirmek için kullanıldığı gittiğinizde görünüm modeli geçirilen bağımsız değişken olarak belirtilmesi Gezinti veri sağlar. Daha fazla bilgi için bkz: [gezinti sırasında geçirme parametreleri](#passing_parameters_during_navigation).

`InternalNavigateToAsync` Yöntemi Gezinti isteği yürütür ve aşağıdaki kod örneğinde gösterilir:

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

`InternalNavigateToAsync` Yöntemi tarafından ilk arama bir görünüm modeline Gezinti gerçekleştirir `CreatePage` yöntemi. Bu yöntem, belirtilen görünüm model türü için karşılık gelen ve oluşturur ve bu görünüm türü örneğini döndürür görünümü bulur. Görünüm model türü için karşılık gelen görünümü bulma, varsayan bir kurala dayalı yaklaşımı kullanır:

-   Görünüm model türü ile aynı bütünleştirilmiş kodda görünümlerdir.
-   Görünümleri olan bir. Görünümler alt ad alanı.
-   Görünüm modelleri olan bir. ViewModels alt ad alanı.
-   Görünüm adları karşılık gelen "kaldırılan Model" ile modeli adlarını görüntülemek için.

Bir görünümün örneği oluşturulduğunda, karşılık gelen kendi görünüm model ile ilişkili. Bu nasıl gerçekleştiği hakkında daha fazla bilgi için bkz: [otomatik olarak bir görünüm modeli bulucuyla görünüm Model oluşturma](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Oluşturulan görünüm ise bir `LoginView`, yeni bir örneğini içinde kaydırılan `CustomNavigationView` sınıfı ve atanan [ `Application.Current.MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) özelliği. Aksi takdirde, `CustomNavigationView` örneği alınır ve null olmayan sağlanan [ `PushAsync` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) yöntemi Gezinti yığına oluşturulan görünüm göndermek için çağrılır. Ancak, varsa alınan `CustomNavigationView` örneği `null`, yeni bir örneğini içinde oluşturulan görünüm Sarmalanan `CustomNavigationView` sınıfı ve atanan `Application.Current.MainPage` özelliği. Bu mekanizma gezinti sırasında sağlar, sayfalar doğru Gezinti yığını boş olduğunda, hem veri içerdiğinde eklenir.

> [!TIP]
> Sayfaları önbelleğe almayı düşünün. Bellek tüketimi şu anda görüntülenmeyen görünümler için önbelleğe alma sayfası sonuçlanır. Ancak, sayfa önbelleğe alma olmadan, yeni bir sayfa için karmaşık bir sayfa için bir performans etkisi olan görüntülendiği her zaman XAML ayrıştırma ve sayfa ve kendi görünüm modeli yapımı oluşacağını anlamına gelmez. Performans denetimleri aşırı sayıda kullanmayan iyi tasarlanmış bir sayfa için yeterli olmalıdır. Ancak, yavaş sayfa yükleme süreleri karşılaştıysanız sayfayı önbelleğe yardımcı olabilir.

Görünüm oluşturup için gittiğinizde sonra `InitializeAsync` görünümün ilişkili görünüm modeli yöntemi yürütüldüğünde. Daha fazla bilgi için bkz: [gezinti sırasında geçirme parametreleri](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Zaman uygulama gezinme başlatılır.

Uygulama başlatıldığında `InitNavigation` yönteminde `App` sınıfı çağrılır. Aşağıdaki kod örneği, bu yöntem gösterilmektedir:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Yöntem yeni bir oluşturur `NavigationService` Autofac bağımlılık ekleme kapsayıcısını nesnesinde ve çağırmadan önce bir başvuru döndürür, `InitializeAsync` yöntemi.

> [!NOTE]
> Zaman `INavigationService` arabirimi tarafından çözüldü `ViewModelBase` sınıfı, kapsayıcı bir başvuru döndürür `NavigationService` InitNavigation yöntemi çağrıldığında, oluşturulan nesne.

Aşağıdaki örnekte gösterildiği kod `NavigationService` `InitializeAsync` yöntemi:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView` İçin uygulamanın kimlik doğrulaması için kullanılan bir önbelleğe alınan erişim belirteci varsa gittiğinizde. Aksi takdirde `LoginView` için gittiğinizde.

Autofac bağımlılık ekleme kapsayıcısını hakkında daha fazla bilgi için bkz: [bağımlılık ekleme giriş](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Gezinti sırasında parametreleri geçirme

Aşağıdakilerden birini `NavigateToAsync` tarafından belirtilen yöntemleri `INavigationService` arabirim, etkinleştirir Gezinti verileri için genellikle başlatılmasını gerçekleştirmek için kullanıldığı gittiğinizde görünüm modeli geçirilen bağımsız değişken olarak belirtilmelidir.

Örneğin, `ProfileViewModel` sınıfı içeren bir `OrderDetailCommand` kullanıcı üzerinde bir sıra seçtiğinde yürütülen `ProfileView` sayfası. Bu sırayla yürütür `OrderDetailAsync` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Gezinti için bu yöntemi çağırır `OrderDetailViewModel`, geçen bir `Order` kullanıcı seçili sırasını temsil eden örneği `ProfileView` sayfası. Zaman `NavigationService` sınıfı oluşturur `OrderDetailView`, `OrderDetailViewModel` sınıf örneği ve görünümün atanan [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Gezinme sonra `OrderDetailView`, `InternalNavigateToAsync` yöntemini yürütür `InitializeAsync` yöntemi görünümün görünüm modeli ilişkili.

`InitializeAsync` Yöntemi tanımlanmış `ViewModelBase` geçersiz kılınabilir bir yöntem olarak sınıfı. Bu yöntem belirten bir `object` bir gezinti işlemi sırasında bir görünüm modeline iletilecek verileri temsil eden bağımsız değişkeni. Bu nedenle, bir gezinti işleminin veri almak istediğiniz görünümü model sınıfları kendi uygulamasını sağlamak `InitializeAsync` gerekli başlatılmasını gerçekleştirmek için yöntem. Aşağıdaki örnekte gösterildiği kod `InitializeAsync` yönteminden `OrderDetailViewModel` sınıfı:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

Bu yöntem alır `Order` görünüm modeline gezinme işlemi sırasında geçirilen ve tam sipariş almak için kullandığı örnek ayrıntıları `OrderService` örneği.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Davranışları kullanarak bildirilecekse gezinme

Gezinti, genellikle bir görünümden kullanıcı etkileşimi tarafından tetiklenir. Örneğin, `LoginView` başarılı kimlik doğrulaması ardından Gezinti gerçekleştirir. Aşağıdaki kod örneğinde, gezinti bir davranış tarafından nasıl çağrıldığını gösterir:

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

Çalışma zamanında `EventToCommandBehavior` etkileşim yanıtlar [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/). Zaman `WebView` bir web sayfasına gider [ `Navigating` ](https://developer.xamarin.com/api/event/Xamarin.Forms.WebView.Navigating/) olay yangın, hangi yürütecek `NavigateCommand` içinde `LoginViewModel`. Varsayılan olarak, olayının olay değişkenlerini komutu geçirilir. Bu veriler, kaynak ve hedef arasında belirtilen dönüştürücü tarafından geçirilen gibi dönüştürülür `EventArgsConverter` döndürür özelliği [ `Url` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebNavigationEventArgs.Url/) gelen [ `WebNavigatingEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebNavigatingEventArgs/). Bu nedenle, `NavigationCommand` olan yürütülen, web sayfasının URL'sini parametre olarak kayıtlı için geçirilen `Action`.

Buna karşılık, `NavigationCommand` yürütür `NavigateAsync` aşağıdaki kod örneğinde gösterildiği yöntemi:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Gezinti için bu yöntemi çağırır `MainViewModel`, ve gezinti, aşağıdaki kaldırır `LoginView` Gezinti yığını sayfasından.

### <a name="confirming-or-cancelling-navigation"></a>Onaylama ya da gezinti iptal ediliyor

Bir uygulama, böylece kullanıcı onaylayın veya gezinme iptal bir gezinti işlemi sırasında kullanıcıyla etkileşim gerekebilir. Örneğin, kullanıcının tam olarak bir veri giriş sayfası tamamlanmadan önce gidin girişiminde bulunduğunda bu gerekli olabilir. Bu durumda, bir uygulama kullanıcının sayfadan ayrılmak veya bu oluşmadan önce gezinme işlemi iptal etmek için sağlayan bir bildirim sağlamalıdır. Bu görünüm modeli sınıfında Gezinti desteklemediğini çağrılır denetlemek için bir bildirim yanıttan kullanarak elde edilebilir.

## <a name="summary"></a>Özet

Xamarin.Forms genellikle kullanıcı Arabirimi ile kullanıcı etkileşimi veya uygulama, iç mantık temelli durumu yapılan değişiklikler sonucunda sonuçları sayfa gezintisi için destek içerir. Ancak, gezinti MVVM desenini kullanan uygulamalarda uygulamak için karmaşık olabilir.

Bu bölümde sunulan bir `NavigationService` görünüm modeli ilk Gezinti görünümü modellerinden gerçekleştirmek için kullanılan sınıf. Gezinti mantığı görünümde yerleştirme modeli sınıfları gelir mantık otomatikleştirilmiş testleri denenmesi. Ayrıca, görünüm modeli sonra belirli iş kuralları zorunlu tutulmaz emin olmak için Denetim Gezinti mantığı uygulayabilirsiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
