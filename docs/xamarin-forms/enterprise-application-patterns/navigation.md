---
title: Kurumsal uygulama gezinti
description: Bu bölümde, hizmetine mobil uygulama görünüm model-first Gezinti görünümü modellerinden nasıl gerçekleştireceğini açıklar.
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d306b0c1c0d08129671e27b96911ec771acb658e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994776"
---
# <a name="enterprise-app-navigation"></a>Kurumsal uygulama gezinti

Xamarin.Forms, genellikle uygulamanın kendi iç durumu mantığı temelli yapılan değişiklikler sonucunda veya kullanıcı Arabirimi ile kullanıcı etkileşimi sonucunda sayfa gezintisi için destek içerir. Ancak, gezinti, aşağıdaki güçlüklerle karşılanması gereken gibi Model-View-ViewModel (MVVM) desenini kullanan uygulamalarda uygulamak için karmaşık olabilir:

-   Görünüm için geçtiğiniz için sıkı eşleştirme ve görünüm arasında bağımlılıklar sunmaz bir yaklaşım kullanarak belirleme.
-   Tarafından için geçtiğiniz için görünüm örneği ve başlatılan işlem koordine etme. MVVM kullanırken görünümü ve görünüm modeli örneği ve birbiriyle görünümün bağlama bağlamı ile ilişkili olması gerekir. Bir uygulama bir bağımlılık ekleme kapsayıcısını kullanırken, görünümleri ve görünüm modelleri örneğinin belirli oluşturma mekanizması gerektirebilir.
-   Görünüm-ilk Gezinti gerçekleştirmek veya model-first Gezinti görüntülemek belirtir. Görünüm ilk Gezinti ile gitmek için sayfanın görünüm türü adıdır. Gezinti sırasında karşılık gelen görünüm modeli ve diğer bağımlı hizmetler ile birlikte belirtilen görünüm oluşturulur. Alternatif bir yaklaşım, burada gitmek için sayfanın görünüm modeli türü adıdır görünüm model-first Gezinti kullanmaktır.
-   Nasıl düzenleyebilmeleri için gezinme davranışını uygulama görünümleri ve görünüm modelleri arasında ayırın. MVVM düzenini uygulamanın UI ve sunu ve iş mantığı arasında bir ayrım sağlar. Ancak, gezinti uygulamanın davranış şekli genellikle uygulama kullanıcı Arabirimi ve sunulara bölümlerini span. Kullanıcı, genellikle bir görünümden Gezinti başlatır ve görünüm gezinmenin sonucunda değiştirilecektir. Ancak, gezinti da sık sık başlatılan ya da öğesinden Görünüm modeli içinde Eşgüdümlü gerekebilir.
-   Başlatma amacıyla gezinti sırasında parametreleri geçirmek nasıl. Örneğin, kullanıcının sipariş ayrıntılarını güncelleştirmek için bir görünüm giderse, sipariş verilerini doğru veri görüntüleyebilmesi görünüme iletilen gerekir.
-   Belirli iş kuralları obeyed emin olmak için birlikte çalışmanız gezintisi, öğreneceksiniz. Örneğin, kullanıcıların herhangi bir geçersiz veri düzeltebilir veya gönderin veya görünümde yapılan tüm veri değişiklikleri atmak için sizden bir görünümden gitmeden önce istenebilir.

Bu bölümde sunarak bu sorunlarını ele alır bir `NavigationService` görünüm modeli ilk sayfa gezintisi gerçekleştirmek için kullanılan sınıf.

> [!NOTE]
> `NavigationService` Tarafından kullanılan uygulama yalnızca hiyerarşik gezinme ContentPage örnekleri arasında gerçekleştirmek için tasarlanmıştır. Diğer sayfası türleri arasında gezinmek için hizmetini kullanarak, beklenmeyen davranışlara neden olabilir.

## <a name="navigating-between-pages"></a>Sayfaları arasında gezinme

Gezinti mantıksal bir görünümün arka plan kod içinde bulunan veya Görünüm modeli içinde bir veri bağlı. Gezinti mantıksal Görünümü'nde yerleştirme en kolay yaklaşım olabilse birim testlerini kolayca test edilebilir değil. Gezinti mantığı görünümde yerleştirilmesi, model sınıfları anlamına gelir mantıksal birim testleri ile uygulanabilecek. Ayrıca, görünüm modeli ardından belirli iş kuralları uygulanmasını sağlamak için denetimi Gezinti mantığı uygulayabilir. Örneğin, bir uygulama uzağa olmayan bir sayfaya gitmek kullanıcı izin vermeyebilir girilen verilerin geçerli olduğundan emin olmanın.

A `NavigationService` sınıf genellikle Test Edilebilirlik yükseltmek için Görünüm modelleri çağrılır. Ancak, görünümü modellerinden görünümlerine gezinme başvuru görünümlerini ve özellikle etkin görünüm modeli, önerilmez, ile ilişkili olmayan görünümleri için Görünüm modelleri gerekir. Bu nedenle, `NavigationService` sunulan burada gitmek için hedef olarak görünüm modeli türünü belirtir.

Hizmetine mobil uygulama kullandığı `NavigationService` görünüm model-first Gezinti sağlamak için sınıf. Bu sınıfın uyguladığı `INavigationService` aşağıdaki kod örneğinde gösterilen arabirimi:

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

Bu arabirimi uygulayan bir sınıf aşağıdaki yöntemleri sağlamanız gerektiğini belirtir:

|Yöntem|Amaç|
|--- |--- |
|`InitializeAsync`|Uygulama başlatıldığında bir iki sayfa gezinti gerçekleştirir.|
|`NavigateToAsync`|Belirtilen sayfaya hiyerarşik gezinme gerçekleştirir.|
|`NavigateToAsync(parameter)`|Hiyerarşik bir parametre geçirerek, belirtilen bir sayfaya gitme işlemi gerçekleştirir.|
|`RemoveLastFromBackStackAsync`|Önceki Sayfa Gezinti yığından kaldırır.|
|`RemoveBackStackAsync`|Önceki sayfalara tüm gezinti yığından kaldırır.|

Ayrıca, `INavigationService` arabirimi uygulayan bir sınıfa sağlamalısınız belirtir bir `PreviousPageViewModel` özelliği. Bu özellik, önceki sayfanın gezinme yığınında ilişkili görünüm modeli türü döndürür.

> [!NOTE]
> Bir `INavigationService` arabirimi misiniz genellikle de belirtmeniz bir `GoBackAsync` gezinme yığınında önceki sayfaya programlama yoluyla döndürmek için kullanılan yöntem. Ancak, gerekli değildir çünkü bu yöntem hizmetine mobil uygulamadan eksik.

### <a name="creating-the-navigationservice-instance"></a>NavigationService örneği oluşturma

`NavigationService` Sınıfını `INavigationService` arabirim, singleton Autofac bağımlılık ekleme kapsayıcısına sahip olarak aşağıdaki kod örneğinde gösterildiği gibi kayıtlı:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` Arabirimi Çözüldü `ViewModelBase` sınıf oluşturucusu, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Bu bir başvuru döndürür `NavigationService` tarafından oluşturulan Autofac bağımlılık ekleme kapsayıcısını depolanan nesne `InitNavigation` yönteminde `App` sınıfı. Daha fazla bilgi için [gezinme, uygulama başlatıldığında](#navigating_when_the_app_is_launched).

`ViewModelBase` Sınıfı depoları `NavigationService` örneğini bir `NavigationService` türünün özelliği `INavigationService`. Bu nedenle, tüm türetilen model sınıfları görüntülemek `ViewModelBase` sınıfı, kullanabileceğiniz `NavigationService` özelliği tarafından belirtilen yöntemlere erişmek için `INavigationService` arabirimi. Bu ekleme yükünü ortadan kaldırır `NavigationService` her görünüm model sınıfı Autofac bağımlılık ekleme kapsayıcısına nesne.

### <a name="handling-navigation-requests"></a>Gezinme istekleri işleme

Xamarin.Forms sağlar [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) kullanıcı olduğu iletir ve geriye doğru istediğiniz şekilde, sayfada gezinme için bir hiyerarşik Gezinti deneyimi uygulayan bir sınıf. Hiyerarşik gezinme hakkında daha fazla bilgi için bkz. [hiyerarşik gezinme](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Yerine kullanmak [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) doğrudan hizmetine uygulama sarmalayan sınıf `NavigationPage` sınıfını `CustomNavigationView` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

Stil için bu sarmalama amacı kolaylığıdır [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) örnek sınıfı için XAML dosyası içinde.

Gezinti birini çağırarak görünüm modeli sınıfları içinde gerçekleştirilir `NavigateToAsync` yöntemleri için aşağıdaki kod örneğinde gösterildiği gibi çıkıldığında sayfa için Görünüm modeli türü belirtme:

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

Her yöntem, türetilen herhangi bir görünüm modeli sınıf tanır `ViewModelBase` çağırarak hiyerarşik gezinme gerçekleştirmek için sınıf `InternalNavigateToAsync` yöntemi. Ayrıca, ikinci `NavigateToAsync` yöntemi için genellikle başlatma gerçekleştirmek için kullanıldığı çıkıldığında görünüm modeli için geçirilen bağımsız değişken olarak belirtilmesi Gezinti veri sağlar. Daha fazla bilgi için [gezinti sırasında parametre geçirme](#passing_parameters_during_navigation).

`InternalNavigateToAsync` Yöntemi gezinme isteği yürütür ve aşağıdaki kod örneğinde gösterilir:

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

`InternalNavigateToAsync` Yöntemi bir görünüm modeline Gezinti ilk çağrısı yaparak gerçekleştirir `CreatePage` yöntemi. Bu yöntem, belirtilen görünüm model türü için karşılık gelen oluşturur ve bu görünüm türü örneğini döndürür görünümü bulur. Görünüm model türü için karşılık gelen görünüm bulma, varsayan bir kural tabanlı yaklaşımı kullanır:

-   Görünüm model türleri olarak aynı derlemede görünümleridir.
-   Görünümleri olan bir. Görünüm alt ad alanı.
-   Görünüm modelleri olan bir. Viewmodel'lar alt ad alanı.
-   Görünüm adları "ile Model kaldırıldı" modeli adları görüntülemek için karşılık gelir.

Bir görünümün örneği oluşturulduğunda, karşılık gelen görünüm model ile ilişkili olduğu. Bu nasıl gerçekleştiği hakkında daha fazla bilgi için bkz. [otomatik olarak bir görünüm modeli bulucuyla bir görünüm modeli oluşturma](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Oluşturulan görünüm ise bir `LoginView`, yeni bir örneğini sarmalanır `CustomNavigationView` sınıfı ve atanan [ `Application.Current.MainPage` ](xref:Xamarin.Forms.Application.MainPage) özelliği. Aksi takdirde, `CustomNavigationView` örneği alınır ve null olmayan sağlanan [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage) yöntemi gezinme yığınına oluşturulan görünüm göndermek için çağrılır. Ancak, alınan `CustomNavigationView` örneği `null`, yeni bir örneğini içinde oluşturulan görünümü sarmalandıktan `CustomNavigationView` sınıfı ve atanan `Application.Current.MainPage` özelliği. Bu mekanizma, gezinti sırasında sağlar, sayfalar doğru için gezinme yığınında boş olduğunda, hem verilerini içerdiğinde eklenir.

> [!TIP]
> Sayfaları önbelleğe almayı düşünün. Bellek tüketimi şu anda görüntülenmeyen görünümler için önbelleğe alma sayfası sonuçlanır. Ancak, sayfa önbelleğe almadan her seferinde yeni bir sayfa için karmaşık bir sayfa için bir performans etkisi olan görüntülendiği XAML ayrıştırma ve sayfa ve kendi görünüm modeli yapımı ortaya çıkar anlama gelir. Performans denetimleri aşırı sayıda kullanmayan iyi tasarlanmış bir sayfa için yeterli olmalıdır. Ancak, sayfa yükleme sürelerinin yavaş karşılaşılırsa sayfası önbelleğe alma yardımcı olabilir.

Görünüm oluşturup için çıkıldığında sonra `InitializeAsync` görünümün ilişkili görünüm modeli yöntemi yürütülür. Daha fazla bilgi için [gezinti sırasında parametre geçirme](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Gezinme olduğunda uygulama başlatılır.

Uygulama başlatıldığında `InitNavigation` yönteminde `App` sınıfı çağrılır. Aşağıdaki kod örneği, bu yöntem gösterir:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Yeni bir yöntem oluşturur `NavigationService` Autofac bağımlılık ekleme kapsayıcısını nesnesinde ve çağırmadan önce ona bir başvuru döndürür, `InitializeAsync` yöntemi.

> [!NOTE]
> Zaman `INavigationService` arabirimi tarafından çözüldü `ViewModelBase` sınıfı, bir kapsayıcı başvurusu döndürür `NavigationService` InitNavigation yöntemi çağrıldığında, oluşturulan nesne.

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

`MainView` İçin uygulamanın kimlik doğrulaması için kullanılan bir önbelleğe alınan erişim belirteci varsa gitme. Aksi takdirde, `LoginView` için gitme.

Autofac bağımlılık ekleme kapsayıcısını hakkında daha fazla bilgi için bkz: [bağımlılık ekleme giriş](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Gezinti sırasında parametreleri geçirme

Aşağıdakilerden birini `NavigateToAsync` yöntemleri tarafından belirtilen `INavigationService` arabirimi için genellikle başlatma gerçekleştirmek için kullanıldığı çıkıldığında görünüm modeli için geçirilen bağımsız değişken olarak belirtilmesi için Gezinti veri sağlar.

Örneğin, `ProfileViewModel` sınıfı içeren bir `OrderDetailCommand` kullanıcı üzerinde bir sipariş seçtiğinde yürütülen `ProfileView` sayfası. Bu sırayla yürütür `OrderDetailAsync` aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Gezinti için bu metodu çağıran `OrderDetailViewModel`, geçen bir `Order` kullanıcı seçili sırasını temsil eden örneği `ProfileView` sayfası. Zaman `NavigationService` sınıfı oluşturur `OrderDetailView`, `OrderDetailViewModel` sınıfı örneği ve görünümün atanan [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Gezinme sonra `OrderDetailView`, `InternalNavigateToAsync` yöntemini yürütür `InitializeAsync` yöntemi görünümün görünüm modeli ilişkili.

`InitializeAsync` Yöntemi tanımlanmış `ViewModelBase` geçersiz kılınabilir bir yöntemi olarak sınıf. Bu yöntem belirtir bir `object` gezinme işlemi sırasında bir görünüm modeline geçirilecek verileri temsil eden bir bağımsız değişken. Bu nedenle, bir gezinti işlemden verileri almak istediğiniz görünüm modeli sınıfları kendi uygulamasını sağlamak `InitializeAsync` gerekli başlatma gerçekleştirmek için yöntemi. Aşağıdaki örnekte gösterildiği kod `InitializeAsync` yönteminden `OrderDetailViewModel` sınıfı:

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

Bu yöntem alır `Order` gezinme işlemi sırasında görünüm modeline geçirildi ve tam siparişi almak için kullandığı örneği ayrıntıları gelen `OrderService` örneği.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Davranışları kullanarak çağrılıyor gezinme

Gezinti, genellikle bir görünümden bir kullanıcı etkileşimi tarafından tetiklenir. Örneğin, `LoginView` Gezinti aşağıdaki başarılı kimlik doğrulaması gerçekleştirir. Aşağıdaki kod örneği, nasıl gezinme bir davranış tarafından çağrılan gösterir:

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

Çalışma zamanında, `EventToCommandBehavior` etkileşim yanıtlar [ `WebView` ](xref:Xamarin.Forms.WebView). Zaman `WebView` bir web sayfasına gider [ `Navigating` ](xref:Xamarin.Forms.WebView.Navigating) olay yangın, hangi yürütecek `NavigateCommand` içinde `LoginViewModel`. Varsayılan olarak, olayının olay değişkenlerini komutuna geçirilir. Belirtilen dönüştürücü tarafından kaynak ve hedef arasında başarılı gibi bu verileri dönüştürülür `EventArgsConverter` döndüren özellik [ `Url` ](xref:Xamarin.Forms.WebNavigationEventArgs.Url) gelen [ `WebNavigatingEventArgs` ](xref:Xamarin.Forms.WebNavigatingEventArgs). Bu nedenle, `NavigationCommand` olan çalıştırılan, web sayfasının URL'sini bir parametre olarak kayıtlı geçen `Action`.

Buna karşılık, `NavigationCommand` yürütür `NavigateAsync` aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Gezinti için bu metodu çağıran `MainViewModel`, gezinti, aşağıdaki kaldırır `LoginView` gezinme yığınında sayfasından.

### <a name="confirming-or-cancelling-navigation"></a>Onaylayan veya gezinti iptal ediliyor

Uygulama, böylece kullanıcı onaylayın veya gezinti iptal bir Yönlenme işlemi sırasında kullanıcıyla etkileşim gerekebilir. Örneğin, kullanıcının tam olarak bir veri giriş sayfası tamamlamış önce gidin girişiminde bulunduğunda bu gerekli olabilir. Bu durumda, bir uygulama kullanıcının sayfadan ayrılmak ya da bunu gerçekleşmeden önce gezinme işlemi iptal etmek için sağlayan bir bildirim sağlamanız gerekir. Bu bir görünüm modeli sınıfta Gezinti olup olmadığını çağrılan denetlemek için bir bildirim yanıttan kullanarak gerçekleştirilebilir.

## <a name="summary"></a>Özet

Xamarin.Forms, genellikle kullanıcı Arabirimi ile kullanıcı etkileşimi veya iç mantık temelli durumu yapılan değişiklikler sonucunda uygulama içinden sonucunda sayfa gezintisi için destek içerir. Ancak, gezinti MVVM düzenini kullanan uygulamalarda uygulamak için karmaşık olabilir.

Bu bölümde sunulan bir `NavigationService` görünümü modellerinden görünüm model-first Gezinti gerçekleştirmek için kullanılan sınıf. Gezinti mantığı görünümde yerleştirilmesi, model sınıfları anlamına gelir mantık otomatik testler uygulanabilecek. Ayrıca, görünüm modeli ardından belirli iş kuralları uygulanmasını sağlamak için denetimi Gezinti mantığı uygulayabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [(2 Mb PDF) e-kitabı indir](https://aka.ms/xamarinpatternsebook)
- [Hizmetine (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
