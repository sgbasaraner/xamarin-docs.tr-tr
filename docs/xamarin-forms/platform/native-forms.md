---
title: Yerel formlar
description: "Native Forms allow Xamarin.Forms ContentPage-derived pages to be consumed by native Xamarin.iOS, Xamarin.Android, and Universal Windows Platform (UWP) projects. Yerel projeleri projeye veya taşınabilir sınıf kitaplığı (PCL), .NET standart kitaplığı ya da paylaşılan proje doğrudan eklenen ContentPage türetilmiş sayfaları kullanabilir. Bu makalede, yerel projelerine doğrudan eklenen ContentPage türetilmiş sayfaları kullanma ve bunlar arasında gezinmek nasıl açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: 89636b874f8dbc8f66280dcc1ed99d0f832ff312
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="native-forms"></a>Yerel formlar

_Yerel Forms yerel Xamarin.iOS, Xamarin.Android ve evrensel Windows Platformu (UWP) projeler tarafından tüketilmesi sayfaları Xamarin.Forms ContentPage türetilmiş olanak verir. Yerel projeleri projeye veya taşınabilir sınıf kitaplığı (PCL), .NET standart kitaplığı ya da paylaşılan proje doğrudan eklenen ContentPage türetilmiş sayfaları kullanabilir. Bu makalede, yerel projelerine doğrudan eklenen ContentPage türetilmiş sayfaları kullanma ve bunlar arasında gezinmek nasıl açıklanmaktadır._

Genellikle, bir Xamarin.Forms uygulaması öğesinden türetilen bir veya daha fazla sayfalar içerir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), ve bu sayfaları PCL, .NET standart kitaplığı veya paylaşılan bir proje tüm platformlar tarafından paylaşılır. Ancak, yerel Forms tanır `ContentPage`-doğrudan yerel Xamarin.iOS, Xamarin.Android ve UWP uygulamaları eklenecek sayfaları türetilmiş. Tüketen yerel proje zorunda karşılaştırıldığında `ContentPage`-PCL, .NET standart kitaplığı veya paylaşılan proje, doğrudan yerel projelere sayfaları ekleme avantajı türetilmiş sayfalarından olan sayfaları yerel görünümlerle genişletilebilir. Yerel görünümleri sonra adlı ile XAML'de `x:Name` ve arka plan kodu gelen başvurulan. Yerel görünümler hakkında daha fazla bilgi için bkz: [yerel görünümleri](~/xamarin-forms/platform/native-views/index.md).

Bir Xamarin.Forms tüketimi için işlem [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-türetilen sayfa yerel projesinde aşağıdaki gibidir:

1. Xamarin.Forms NuGet paketi yerel projeye ekleyin.
1. Ekleme [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-türetilen sayfası ve yerel projeye tüm bağımlılıkları.
1. Çağrı `Forms.Init` yöntemi.
1. Olgusu [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-sayfası türetilmiş ve aşağıdaki uzantısı yöntemlerden birini kullanarak uygun yerel türüne dönüştürün: `CreateViewController` iOS, `CreateFragment` veya `CreateSupportFragment` Android için veya `CreateFrameworkElement` UWP için.
1. İçin yerel tür gösterimini gidin [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-yerel Gezinti API kullanarak sayfası türetilmiş.

Xamarin.Forms çağırarak başlatılmalı `Forms.Init` yerel bir proje oluşturmadan önce yöntemi bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-sayfası türetilmiş. Ne zaman öncelikle bunu seçme bağımlı uygulama akışınız en uygun olduğunda – uygulama başlangıcında veya hemen önce gerçekleştirilebilir `ContentPage`-türetilen sayfasında yapılandırılmıştır. Bu makalede ve eşlik eden örnek uygulamaları `Forms.Init` yöntemi uygulama başlangıcında çağrılır.

> [!NOTE]
> **Not**: **NativeForms** örnek uygulama çözümü tüm Xamarin.Forms projeleri içermiyor. Bunun yerine, bir Xamarin.iOS projesi, bir Xamarin.Android projesi ve bir UWP projesi oluşur. Her proje kullanmak için yerel Forms kullanan yerel bir projedir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-sayfaları türetilmiş. Ancak, yerel projeleri uygulanamadı neden kullanmasına neden yoktur `ContentPage`-sayfaları PCL, .NET standart kitaplığı veya paylaşılan proje türetilmiş.

Yerel formlarını kullanırken Xamarin.Forms gibi özellikleri [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/), [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)ve veri bağlama altyapısı, tüm hala çalışma.

## <a name="ios"></a>iOS

İOS, `FinishedLaunching` geçersiz kılması `AppDelegate` sınıfı yerdir genellikle uygulama gerçekleştirmek için başlatma ilgili görevleri. Uygulama başlattı ve genellikle ana pencereyi yapılandırmak ve denetleyicisini görüntülemek için geçersiz kılınmadığı sonra çağrılır. Aşağıdaki örnekte gösterildiği kod `AppDelegate` örnek uygulama sınıfında:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        var mainPage = new PhonewordPage().CreateViewController();
        mainPage.Title = "Phoneword";

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

`FinishedLaunching` Yöntemi aşağıdaki görevleri gerçekleştirir:

- Xamarin.Forms çağrılarak başlatılır `Forms.Init` yöntemi.
- Bir başvuru `AppDelegate` sınıfı depolanır `static` `Instance` alan. Bu tanımlanan yöntemleri çağırmak diğer sınıflar için bir mekanizma sağlamaktır `AppDelegate` sınıfı.
- `UIWindow`, Yerel iOS uygulamalarında görünümler için ana kapsayıcı olduğu oluşturulur.
- `PhonewordPage` Bir Xamarin.Forms olan sınıf [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-XAML içinde tanımlanan sayfa türetilmiş, oluşturulur ve dönüştürülen bir `UIViewController` kullanarak `CreateViewController` genişletme yöntemi.
- `Title` Özelliği `UIViewController` , hangi gösterileceği ayarlanır `UINavigationBar`.
- A `UINavigationController` hiyerarşik Gezinti yönetmek için oluşturulur. `UINavigationController` Sınıfı görünüm denetleyicilerinin yığınını yönetir ve `UIViewController` içine geçirilen Oluşturucusu başlangıçta zaman sunulur `UINavigationController` yüklenir.
- `UINavigationController` Örneği üst düzey ayarlanmış `UIViewController` için `UIWindow`ve `UIWindow` uygulama için anahtar pencere olarak ayarlayın ve görünür hale gelir.

Bir kez `FinishedLaunching` yöntemi yürütülebilir, UI Xamarin.Forms tanımlı `PhonewordPage` sınıfı görüntülenir, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png "iOS PhonewordPage")

Kullanıcı Arabirimi, örneğin e dokunabilirsiniz üzerinde etkileşim bir [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), olay işleyicileri ile sonuçlanır `PhonewordPage` arka plan kodu çalıştırma. Örneğin, bir kullanıcının ne zaman dokunur **Arama Geçmişi** düğme, aşağıdaki olay işleyicisini gerçekleştirilir:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

`static` `AppDelegate.Instance` Alan verir `AppDelegate.NavigateToCallHistoryPage` aşağıdaki kod örneğinde gösterildiği çağrılacak, yöntemi:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

`NavigateToCallHistoryPage` Yöntemi dönüştürür Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-sayfasına türetilmiş bir `UIViewController` ile `CreateViewController` genişletme yöntemi ve kümelerini `Title` özelliği `UIViewController`. `UIViewController` Üzerine sonra gönderilen `UINavigationController` tarafından `PushViewController` yöntemi. Bu nedenle, kullanıcı arabirimini Xamarin.Forms tanımlanan `CallHistoryPage` sınıfı görüntülenir, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png "iOS CallHistoryPage")

Zaman `CallHistoryPage` , geri dokunarak görüntülenir ok pop `UIViewController` için `CallHistoryPage` sınıfıyla `UINavigationController`, kullanıcıya döndürerek `UIViewController` için `PhonewordPage` sınıfı.

## <a name="android"></a>Android

Android'de `OnCreate` geçersiz kılması `MainActivity` sınıfı yerdir genellikle uygulama gerçekleştirmek için başlatma ilgili görevleri. Aşağıdaki örnekte gösterildiği kod `MainActivity` örnek uygulama sınıfında:

```csharp
public class MainActivity : AppCompatActivity
{
    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Phoneword";

        var mainPage = new PhonewordPage().CreateFragment(this);
        FragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

`OnCreate` Yöntemi aşağıdaki görevleri gerçekleştirir:

- Xamarin.Forms çağrılarak başlatılır `Forms.Init` yöntemi.
- Bir başvuru `MainActivity` sınıfı depolanır `static` `Instance` alan. Bu tanımlanan yöntemleri çağırmak diğer sınıflar için bir mekanizma sağlamaktır `MainActivity` sınıfı.
- `Activity` İçerik düzeni kaynağı ayarlayın. Örnek uygulama düzeni oluşan bir `LinearLayout` içeren bir `Toolbar`ve bir `FrameLayout` parça kapsayıcı olarak hareket edecek.
- `Toolbar` Alınır ve ayarlamak için Eylem çubuğu olarak `Activity`, ve Eylem çubuğu başlığı ayarlayın.
- `PhonewordPage` Bir Xamarin.Forms olan sınıf [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-XAML içinde tanımlanan sayfa türetilmiş, oluşturulur ve dönüştürülen bir `Fragment` kullanarak `CreateFragment` genişletme yöntemi.
- `FragmentManager` Sınıfı oluşturur ve yerini alan bir işlem tamamlar `FrameLayout` örneği `Fragment` için `PhonewordPage` sınıfı.

Parçaları hakkında daha fazla bilgi için bkz: [parçaları](~/android/platform/fragments/index.md).

> [!NOTE]
> **Not**: ek olarak `CreateFragment` genişletme yöntemi, Xamarin.Forms de içeren bir `CreateSupportFragment` yöntemi. `CreateFragment` Yöntemi oluşturur bir `Android.App.Fragment` API 11 hedefleyen uygulamalar kullanılır ve büyük olabilir. `CreateSupportFragment` Yöntemi oluşturur bir `Android.Support.V4.App.Fragment` 11 önce API sürümlerini hedefleyen uygulamalarda kullanılabilir.

Bir kez `OnCreate` yöntemi yürütülebilir, UI Xamarin.Forms tanımlı `PhonewordPage` sınıfı görüntülenir, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png "Android PhonewordPage")

Kullanıcı Arabirimi, örneğin e dokunabilirsiniz üzerinde etkileşim bir [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), olay işleyicileri ile sonuçlanır `PhonewordPage` arka plan kodu çalıştırma. Örneğin, bir kullanıcının ne zaman dokunur **Arama Geçmişi** düğme, aşağıdaki olay işleyicisini gerçekleştirilir:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainActivity.Instance` Alan verir `MainActivity.NavigateToCallHistoryPage` aşağıdaki kod örneğinde gösterildiği çağrılacak, yöntemi:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateFragment(this);
    FragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, callHistoryPage)
        .Commit();
}
```

`NavigateToCallHistoryPage` Yöntemi Xamarin.Forms dönüştürür [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-sayfasına türetilmiş bir `Fragment` ile `CreateFragment` genişletme yöntemi ve ekler `Fragment` yığın parçaya yedekleyin. Bu nedenle, kullanıcı arabirimini Xamarin.Forms tanımlanan `CallHistoryPage` aşağıdaki ekran görüntüsünde gösterildiği gibi gösterilir:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png "Android CallHistoryPage")

Zaman `CallHistoryPage` , geri dokunarak görüntülenir ok pop `Fragment` için `CallHistoryPage` parça geri yığınından kullanıcıya döndürerek `Fragment` için `PhonewordPage` sınıfı.

### <a name="enabling-back-navigation-support"></a>Geri gezinti desteğini etkinleştirme

`FragmentManager` Sınıfına sahip bir `BackStackChanged` parça geri yığınının içerik her değiştiğinde tetiklenen olay. `OnCreate` Yönteminde `MainActivity` sınıfı, bu olay için anonim olay işleyicisi içerir:

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Var olan bir veya daha fazla koşuluyla bu olay işleyicisi geri düğmesini eylem çubuğunda görüntüler. `Fragment` parça örneklerinde geri yığını. Geri düğmesine yanıt tarafından işlenen `OnOptionsItemSelected` geçersiz kıl:

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && FragmentManager.BackStackEntryCount > 0)
    {
        FragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

`OnOptionsItemSelected` Geçersiz kılma seçenekleri menüsündeki bir öğeyi seçildiğinde çağrılır. Bu uygulama geçerli parça parça geri yığından geri düğmesini seçili ve bir veya daha fazla vardır koşuluyla POP `Fragment` parça örneklerinde geri yığını.

### <a name="multiple-activities"></a>Birden çok etkinliği

Bir uygulama birden çok etkinliklerini, oluşturulduğunda [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-türetilen sayfalar katıştırılmış her etkinliğin. Bu senaryoda, `Forms.Init` yöntemi yalnızca çağrılabilir `OnCreate` ilk geçersiz kılma `Activity` bir Xamarin.Forms katıştırır `ContentPage`. Ancak, bunun aşağıdaki etkisi vardır:

- Değeri `Xamarin.Forms.Color.Accent` alınır `Activity` çağrılan `Forms.Init` yöntemi.
- Değeri `Xamarin.Forms.Application.Current` ile ilişkili `Activity` çağrılan `Forms.Init` yöntemi.

### <a name="choosing-a-file"></a>Bir dosya seçme

Katıştırma olduğunda bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-kullanan sayfasını türetilmiş bir [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) , desteklemek için bir HTML "Dosya Seç" düğmesini, `Activity` geçersiz kılma gerekecek `OnActivityResult` yöntem:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

UWP, yerel üzerinde `App` sınıfı yerdir genellikle uygulama gerçekleştirmek için başlatma ilgili görevleri. Xamarin.Forms genellikle başlatılır, Xamarin.Forms UWP uygulamalarında içinde `OnLaunched` yerel geçersiz kılma `App` geçirmek için sınıf `LaunchActivatedEventArgs` bağımsız değişkeni `Forms.Init` yöntemi. Bu nedenle, bir Xamarin.Forms tüketen yerel UWP uygulamaları [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-türetilen sayfa en kolay çağırabilir `Forms.Init` yönteminden `App.OnLaunched` yöntemi.

Varsayılan olarak, yerel `App` sınıfı başlatır `MainPage` uygulamanın ilk sayfası sınıfı. Aşağıdaki örnekte gösterildiği kod `MainPage` örnek uygulama sınıfında:

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        this.Content = new Phoneword.UWP.Views.PhonewordPage().CreateFrameworkElement();
    }
    ...
}
```

`MainPage` Oluşturucusu aşağıdaki görevleri gerçekleştirir:

- Önbelleğe alma sayfa için etkin şekilde yeni bir `MainPage` bir kullanıcı sayfasına geri gittiğinde oluşturulan değil.
- Bir başvuru `MainPage` sınıfı depolanır `static` `Instance` alan. Bu tanımlanan yöntemleri çağırmak diğer sınıflar için bir mekanizma sağlamaktır `MainPage` sınıfı.
- `PhonewordPage` Bir Xamarin.Forms olan sınıf [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-XAML içinde tanımlanan sayfa türetilmiş, oluşturulur ve dönüştürülen bir `FrameworkElement` kullanarak `CreateFrameworkElement` genişletme yöntemi olarak ayarlayın ve ardından içeriği olarak `MainPage` sınıfı.

Bir kez `MainPage` Oluşturucusu yürütülmeden, UI Xamarin.Forms tanımlı `PhonewordPage` sınıfı görüntülenir, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png "UWP PhonewordPage")

Kullanıcı Arabirimi, örneğin e dokunabilirsiniz üzerinde etkileşim bir [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), olay işleyicileri ile sonuçlanır `PhonewordPage` arka plan kodu çalıştırma. Örneğin, bir kullanıcının ne zaman dokunur **Arama Geçmişi** düğme, aşağıdaki olay işleyicisini gerçekleştirilir:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainPage.Instance` Alan verir `MainPage.NavigateToCallHistoryPage` aşağıdaki kod örneğinde gösterildiği çağrılacak, yöntemi:

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

Gezinti bölmesinde UWP ile gerçekleştirilen genellikle `Frame.Navigate` geçen yöntemi bir `Page` bağımsız değişkeni. Xamarin.Forms tanımlayan bir `Frame.Navigate` geçen genişletme yöntemi bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-sayfası örneği türetilmiş. Bu nedenle, `NavigateToCallHistoryPage` yöntemini yürütür, Xamarin.Forms tanımlanan UI `CallHistoryPage` aşağıdaki ekran görüntüsünde gösterildiği gibi gösterilir:

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png "UWP CallHistoryPage")

Zaman `CallHistoryPage` , geri dokunarak görüntülenir ok pop `FrameworkElement` için `CallHistoryPage` uygulama arka yığınından kullanıcıya döndürerek `FrameworkElement` için `PhonewordPage` sınıfı.

### <a name="enabling-back-navigation-support"></a>Geri gezinti desteğini etkinleştirme

UWP üzerinde uygulamalar arasında farklı cihaz form faktörleri tüm donanım ve yazılım geri düğmelerinin, geri gezinti etkinleştirmeniz gerekir. Bu olay işleyicisi için kaydederek gerçekleştirilebilir `BackRequested` başlığında gerçekleştirilen olay `OnLaunched` yerel yönteminde `App` sınıfı:

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    if (rootFrame == null)
    {
        ...      
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
    }
    ...
}
```

Uygulama başlatıldığında, `GetForCurrentView` yöntemi alır `SystemNavigationManager` nesne geçerli görünüm ile ilişkilendirilen sonra bir olay işleyicisi için kaydeder `BackRequested` olay. Ön plan uygulama ise ve yanıt olarak çağırır uygulamayı yalnızca bu olayı aldığı `OnBackRequested` olay işleyicisi:

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```

`OnBackRequested` Olay işleyicisi çağrılarını `GoBack` yöntemi uygulama ve kümelerini kök çerçevesinde `BackRequestedEventArgs.Handled` özelliğine `true` olay işlenmiş olarak işaretlenecek. Olay işlenmiş olarak işaretlemek için hata sistemin uygulamanın (mobil cihaz ailesi) çıktığınızda gezinme veya olayda (Masaüstü aygıt ailesi) yoksayılıyor neden olabilir.

Uygulama bir telefon sağlanan sistem geri düğmesinde kullanır, ancak Masaüstü cihazlarda başlık çubuğundaki Geri düğmesi gösterilip gösterilmeyeceğini seçer. Bu ayar sağlanır `AppViewBackButtonVisibility` özelliğini birine `AppViewBackButtonVisibility` numaralandırma değerlerinin:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated` Yanıt olarak yürütülen olay işleyicisi `Navigated` olay tetikleme, sayfa gezintisi oluştuğunda başlık çubuğu geri düğmesini görünürlüğünü güncelleştirir. Bu başlık çubuğunu geri düğmesini uygulama arka yığın boş değilse görülebilir veya uygulama arka yığını boşsa, başlık çubuğu'ndan kaldırılmış sağlar.

UWP geri gezinti desteği hakkında daha fazla bilgi için bkz: [Gezinme geçmişi ve geriye doğru UWP uygulamalar için Gezinti](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Özet

Yerel Forms izin Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-yerel Xamarin.iOS, Xamarin.Android ve evrensel Windows Platformu (UWP) projeler tarafından tüketilen sayfalarına türetilmiş. Yerel projeleri tüketebileceği `ContentPage`-projeye veya PCL, .NET standart kitaplığı ya da paylaşılan proje doğrudan eklenen sayfaları türetilmiş. Bu makalede açıklanan kullanma `ContentPage`-yerel projeleri ve bunlar arasında gezinmek nasıl doğrudan eklenen sayfaları türetilmiş.


## <a name="related-links"></a>İlgili bağlantılar

- [NativeForms (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Yerel görünümleri](~/xamarin-forms/platform/native-views/index.md)
