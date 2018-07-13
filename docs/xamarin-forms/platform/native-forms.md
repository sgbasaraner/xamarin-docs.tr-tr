---
title: Xamarin.Forms içinde Xamarin yerel projeleri
description: Bu makalede, Xamarin yerel projeleri için doğrudan eklenen ContentPage türetilmiş sayfaları kullanma ve bunlar arasında gezinmek nasıl açıklanmaktadır.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: 65bb3fa070c082fa6c6c489e326a870a80fb9502
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997529"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin.Forms içinde Xamarin yerel projeleri

_Yerel formlar yerel Evrensel Windows Platformu (UWP) Xamarin.iOS ve Xamarin.Android projesi tarafından kullanılacak sayfaları Xamarin.Forms ContentPage türetilmiş olanak verir. Yerel projeleri .NET Standard kitaplığı, .NET Standard kitaplığı ya da paylaşılan proje veya proje, doğrudan eklenen ContentPage türetilmiş sayfaları kullanabilir. Bu makalede, doğrudan yerel projeleri için eklenen ContentPage türetilmiş sayfaları kullanma ve bunlar arasında gezinmek nasıl açıklanmaktadır._

Genellikle, bir Xamarin.Forms uygulaması türetilen bir veya daha fazla sayfaları içerir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), ve bir .NET Standard kitaplığı projesi ya da paylaşılan proje bu sayfalara tüm platformlar tarafından paylaşılır. Ancak, yerel formlar tanır `ContentPage`-Xamarin.iOS ve Xamarin.Android UWP doğrudan yerel uygulamalar için eklenecek sayfaları türetilmiş. Tüketen yerel proje sahip olmaya karşılaştırıldığında `ContentPage`-türetilmiş sayfalarından .NET Standard kitaplığı projesi ya da paylaşılan proje, yerel projeleri için doğrudan sayfalarına ekleme avantajı olan sayfaları yerel görünümlerle genişletilebilir. Yerel görünümler sonra adı ile XAML içinde `x:Name` ve arka plan kod öğesinden başvurulan. Yerel görünümler hakkında daha fazla bilgi için bkz: [yerel görünümler](~/xamarin-forms/platform/native-views/index.md).

Bir Xamarin.Forms kullanma işlemi [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-yerel bir proje türetilmiş sayfasında aşağıdaki gibidir:

1. Yerel proje için Xamarin.Forms NuGet paketini ekleyin.
1. Ekleme [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-türetilmiş sayfası ve tüm bağımlılıklarını, yerel proje.
1. Çağrı `Forms.Init` yöntemi.
1. Olgusu [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-türetilmiş sayfasında ve uzantı aşağıdaki yöntemlerden birini kullanarak uygun yerel türüne dönüştürün: `CreateViewController` iOS, `CreateFragment` veya `CreateSupportFragment` Android, veya `CreateFrameworkElement` UWP için.
1. Gezinmek için yerel tür gösterimini [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-yerel Gezinti API kullanarak sayfası türetilmiş.

Xamarin.Forms çağırarak başlatılmalı `Forms.Init` yerel bir proje oluşturmak önce yöntemi bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-sayfası türetilmiş. Ne zaman öncelikle bunu seçerek bağımlı uygulama akışınızı en uygun olduğunda – uygulama başlangıcında veya hemen önce gerçekleştirilebilir `ContentPage`-türetilmiş sayfası oluşturulur. Bu makalede ve eşlik eden örnek uygulamalar `Forms.Init` yöntemi uygulama başlangıcında çağırılır.

> [!NOTE]
> **NativeForms** örnek uygulama çözümü, herhangi bir Xamarin.Forms projeyi içermiyor. Bunun yerine, bir Xamarin.iOS projesi, bir Xamarin.Android projesi ve bir UWP projesi oluşur. Her proje kullanmak üzere yerel formlar kullanan yerel bir projedir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-sayfaları türetilmiş. Ancak, neden uygulanamadı yerel projeleri tüketen bir neden yoktur `ContentPage`-.NET Standard kitaplığı projesi ya da paylaşılan proje sayfaları türetilmiş.

Yerel formlar kullanırken Xamarin.Forms gibi özellikleri [ `DependencyService` ](xref:Xamarin.Forms.DependencyService), [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)ve veri bağlama altyapısı, tüm hala iş.

## <a name="ios"></a>iOS

İos'ta `FinishedLaunching` içinde geçersiz kılma `AppDelegate` sınıfı, genellikle uygulamanın gerçekleştirmesi için yer başlatma ilgili görevleri. Uygulamayı kullanıma sundu ve genellikle ana pencereyi yapılandırmak ve denetleyici görüntülemek için geçersiz kılındı sonra çağrılır. Aşağıdaki örnekte gösterildiği kod `AppDelegate` örnek uygulamada sınıfı:

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
- Bir başvuru `AppDelegate` sınıfı depolanan `static` `Instance` alan. Bu tanımlı yöntem çağırmak diğer sınıflar için bir mekanizma sağlamaktır `AppDelegate` sınıfı.
- `UIWindow`, Hangi yerel iOS uygulamaları, görünümlerde ana kapsayıcısıdır oluşturulur.
- `PhonewordPage` Bir Xamarin.Forms sınıfının [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-sayfası XAML içinde tanımlanmış türetilmiş, oluşturulur ve dönüştürülen bir `UIViewController` kullanarak `CreateViewController` genişletme yöntemi.
- `Title` Özelliği `UIViewController` ayarlanmış, üzerinde görüntüleneceği `UINavigationBar`.
- A `UINavigationController` hiyerarşik gezinme yönetmek için oluşturulur. `UINavigationController` Sınıfı görünüm denetleyicileri yığınını yönetir ve `UIViewController` yöntemlere geçirilen Oluşturucu ne zaman başlangıçta sunulacak `UINavigationController` yüklenir.
- `UINavigationController` Örneği en üst düzey ayarlandığında `UIViewController` için `UIWindow`ve `UIWindow` anahtar penceresidir olarak ayarlanır ve görünür hale gelir.

Bir kez `FinishedLaunching` yöntemi yürütüldüğünde, UI Xamarin.Forms içinde tanımlı `PhonewordPage` sınıfı görüntülenir, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "iOS PhonewordPage")

Kullanıcı Arabirimi, örneğin dokunarak etkileşim bir [ `Button` ](xref:Xamarin.Forms.Button), olay işleyicileri ile sonuçlanır `PhonewordPage` arka plan kod yürütülüyor. Örneğin, bir kullanıcının ne zaman dokunduğunda **çağrı geçmişi** düğme, aşağıdaki olay işleyicisi yürütülür:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

`static` `AppDelegate.Instance` Alan verir `AppDelegate.NavigateToCallHistoryPage` aşağıdaki kod örneğinde gösterilen çağrılacak, yöntemi:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

`NavigateToCallHistoryPage` Yöntemi dönüştürür Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-sayfasına türetilmiş bir `UIViewController` ile `CreateViewController` genişletme yöntemi ve kümelerini `Title` özelliği `UIViewController`. `UIViewController` Üzerine itilir `UINavigationController` tarafından `PushViewController` yöntemi. Bu nedenle, kullanıcı arabirimini Xamarin.Forms içinde tanımlanan `CallHistoryPage` sınıfı görüntülenir, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "iOS CallHistoryPage")

Zaman `CallHistoryPage` , geri dokunarak görüntülenir ok pop `UIViewController` için `CallHistoryPage` gelen sınıfı `UINavigationController`, kullanıcıya döndürerek `UIViewController` için `PhonewordPage` sınıfı.

## <a name="android"></a>Android

Android, `OnCreate` içinde geçersiz kılma `MainActivity` sınıfı, genellikle uygulamanın gerçekleştirmesi için yer başlatma ilgili görevleri. Aşağıdaki örnekte gösterildiği kod `MainActivity` örnek uygulamada sınıfı:

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
- Bir başvuru `MainActivity` sınıfı depolanan `static` `Instance` alan. Bu tanımlı yöntem çağırmak diğer sınıflar için bir mekanizma sağlamaktır `MainActivity` sınıfı.
- `Activity` İçeriği Düzen kaynağı ayarlayın. Örnek uygulamada Düzen oluşan bir `LinearLayout` içeren bir `Toolbar`ve `FrameLayout` parça kapsayıcısı olarak görev yapacak.
- `Toolbar` Alınır ve ayarlamak için Eylem çubuğu olarak `Activity`, Eylem çubuğu başlığını ayarlayın.
- `PhonewordPage` Bir Xamarin.Forms sınıfının [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-sayfası XAML içinde tanımlanmış türetilmiş, oluşturulur ve dönüştürülen bir `Fragment` kullanarak `CreateFragment` genişletme yöntemi.
- `FragmentManager` Sınıfı oluşturur ve yerini alan bir işlem yürütmeleri `FrameLayout` ile örnek `Fragment` için `PhonewordPage` sınıfı.

Parçaları hakkında daha fazla bilgi için bkz: [parçaları](~/android/platform/fragments/index.md).

> [!NOTE]
> Ek olarak `CreateFragment` genişletme yöntemi, Xamarin.Forms yöntemlerine bir `CreateSupportFragment` yöntemi. `CreateFragment` Yöntemi oluşturur bir `Android.App.Fragment` API 11 hedefleyen uygulamalarda kullanılan ve büyük olabilir. `CreateSupportFragment` Yöntemi oluşturur bir `Android.Support.V4.App.Fragment` 11 önce API sürümlerini hedefleyen uygulamalarda kullanılabilir.

Bir kez `OnCreate` yöntemi yürütüldüğünde, UI Xamarin.Forms içinde tanımlı `PhonewordPage` sınıfı görüntülenir, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

Kullanıcı Arabirimi, örneğin dokunarak etkileşim bir [ `Button` ](xref:Xamarin.Forms.Button), olay işleyicileri ile sonuçlanır `PhonewordPage` arka plan kod yürütülüyor. Örneğin, bir kullanıcının ne zaman dokunduğunda **çağrı geçmişi** düğme, aşağıdaki olay işleyicisi yürütülür:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainActivity.Instance` Alan verir `MainActivity.NavigateToCallHistoryPage` aşağıdaki kod örneğinde gösterilen çağrılacak, yöntemi:

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

`NavigateToCallHistoryPage` Yöntemi Xamarin.Forms dönüştürür [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-sayfasına türetilmiş bir `Fragment` ile `CreateFragment` genişletme yöntemi ve ekler `Fragment` parçaya yığın yedekleyin. Bu nedenle, kullanıcı arabirimini Xamarin.Forms içinde tanımlanan `CallHistoryPage` , aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

Zaman `CallHistoryPage` görüntülenen, geri dokunarak ok pop `Fragment` için `CallHistoryPage` parça geri yığından kullanıcıya döndürerek `Fragment` için `PhonewordPage` sınıfı.

### <a name="enabling-back-navigation-support"></a>Geri gezinme desteğini etkinleştirme

`FragmentManager` Sınıfında bir `BackStackChanged` parça geri yığın içeriği değiştiğinde harekete olayı. `OnCreate` Yönteminde `MainActivity` sınıfı içeren bir anonim olay işleyicisi bu olayı için:

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Bu olay işleyicisi yok koşuluyla, bir veya daha fazla eylem çubuğunda geri düğmesi görüntüler `Fragment` parça örneklerinde geri yığını. Geri düğmesine dokunarak yanıt tarafından işlenen `OnOptionsItemSelected` geçersiz kıl:

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

`OnOptionsItemSelected` Geçersiz kılma seçenekleri menüsündeki bir öğeyi seçildiğinde çağrılır. Geri düğmesi seçilmiştir ve bir veya daha fazla koşuluyla bu uygulama geçerli parça parça geri yığından POP `Fragment` parça örneklerinde geri yığını.

### <a name="multiple-activities"></a>Birden çok etkinlik

Birden çok etkinliği uygulama oluşturulduğunda [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-türetilmiş sayfaları her etkinliğin gömülebilir. Bu senaryoda, `Forms.Init` yöntemi yalnızca çağrılabilir `OnCreate` ilk geçersiz kılma `Activity` , bir Xamarin.Forms katıştırır `ContentPage`. Ancak, bunun aşağıdaki etkisi vardır:

- Değerini `Xamarin.Forms.Color.Accent` alınır `Activity` çağrılan `Forms.Init` yöntemi.
- Değerini `Xamarin.Forms.Application.Current` ilişkilendirileceği `Activity` çağrılan `Forms.Init` yöntemi.

### <a name="choosing-a-file"></a>Bir dosya seçme

Ekleme yaparken bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-kullanan sayfa türetilmiş bir [ `WebView` ](xref:Xamarin.Forms.WebView) , desteklemek için bir HTML "Dosya Seç" düğmesi, `Activity` geçersiz kılmanız gerekir `OnActivityResult` yöntemi:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

UWP, yerel şirket `App` sınıfı, genellikle uygulamanın gerçekleştirmesi için yer başlatma ilgili görevleri. Xamarin.Forms genellikle başlatılır, Xamarin.Forms UWP uygulamalarında içinde `OnLaunched` yerel geçersiz kılma `App` geçirilecek sınıfı `LaunchActivatedEventArgs` bağımsız değişkeni `Forms.Init` yöntemi. Bu nedenle, bir Xamarin.Forms kullanan yerel UWP uygulamaları [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-türetilmiş sayfası en bir kolayca çağırabilir `Forms.Init` yönteminden `App.OnLaunched` yöntemi.

Varsayılan olarak, yerel `App` sınıfı başlatır `MainPage` uygulamanın ilk sayfası sınıfı. Aşağıdaki örnekte gösterildiği kod `MainPage` örnek uygulamada sınıfı:

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

`MainPage` Oluşturucu aşağıdaki görevleri gerçekleştirir:

- Önbelleğe alma, sayfa için etkin şekilde yeni bir `MainPage` kullanıcı sayfaya geri gittiğinde oluşturulmuş değil.
- Bir başvuru `MainPage` sınıfı depolanan `static` `Instance` alan. Bu tanımlı yöntem çağırmak diğer sınıflar için bir mekanizma sağlamaktır `MainPage` sınıfı.
- `PhonewordPage` Bir Xamarin.Forms sınıfının [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-sayfası XAML içinde tanımlanmış türetilmiş, oluşturulur ve dönüştürülen bir `FrameworkElement` kullanarak `CreateFrameworkElement` genişletme yöntemi ve içeriği olarak ayarlayın `MainPage` sınıfı.

Bir kez `MainPage` Oluşturucu yürütülen, UI Xamarin.Forms içinde tanımlı `PhonewordPage` sınıfı görüntülenir, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "UWP PhonewordPage")

Kullanıcı Arabirimi, örneğin dokunarak etkileşim bir [ `Button` ](xref:Xamarin.Forms.Button), olay işleyicileri ile sonuçlanır `PhonewordPage` arka plan kod yürütülüyor. Örneğin, bir kullanıcının ne zaman dokunduğunda **çağrı geçmişi** düğme, aşağıdaki olay işleyicisi yürütülür:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainPage.Instance` Alan verir `MainPage.NavigateToCallHistoryPage` aşağıdaki kod örneğinde gösterilen çağrılacak, yöntemi:

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

UWP gezintisi ile gerçekleştirilen genellikle `Frame.Navigate` gereken yöntemini bir `Page` bağımsız değişken. Xamarin.Forms tanımlayan bir `Frame.Navigate` alan genişletme yöntemi bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-türetilmiş sayfası örneği. Bu nedenle, `NavigateToCallHistoryPage` yöntemini yürütür, Xamarin.Forms içinde tanımlanan kullanıcı Arabirimi `CallHistoryPage` , aşağıdaki ekran görüntüsünde gösterildiği gibi görüntülenir:

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "UWP CallHistoryPage")

Zaman `CallHistoryPage` görüntülenen, geri dokunarak ok pop `FrameworkElement` için `CallHistoryPage` uygulama içi geri yığından kullanıcıya döndürerek `FrameworkElement` için `PhonewordPage` sınıfı.

### <a name="enabling-back-navigation-support"></a>Geri gezinme desteğini etkinleştirme

UWP üzerinde uygulamalar arasında farklı cihaz büyüklükleri geri gezinme için tüm donanım ve yazılım geri düğmeleri, etkinleştirmeniz gerekir. Bu olay işleyicisi kaydederek gerçekleştirilebilir `BackRequested` içinde gerçekleştirilen olay `OnLaunched` yerel yönteminde `App` sınıfı:

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

Uygulama başlatıldığında, `GetForCurrentView` yöntemi alır `SystemNavigationManager` nesne geçerli bir görünümle ilişkili ve ardından bir olay işleyicisi için kaydeder `BackRequested` olay. Ön plan uygulamasıdır ve yanıt olarak, çağıran uygulama yalnızca bu olay aldığında `OnBackRequested` olay işleyicisi:

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

`OnBackRequested` Olay işleyicisi çağrılarını `GoBack` yöntemi uygulama ve kümeleri kök çerçevesinde `BackRequestedEventArgs.Handled` özelliğini `true` olay işlenmiş olarak işaretleme. Olay işlenmiş olarak işaretleme hatası (açık mobil cihaz ailesi) uygulama uzağa gezinme veya (Masaüstü cihaz ailesi) olayı yoksayılıyor sistem sonuçlanabilir.

Uygulamayı telefonda sağlanan sistem geri düğmesi kullanır, ancak Masaüstü cihazlarda başlık çubuğundaki Geri düğmesi görüntülenip görüntülenmeyeceğini seçer. Bu ayarı gerçekleştirilir `AppViewBackButtonVisibility` özelliğini birine `AppViewBackButtonVisibility` sabit listesi değerleri:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated` Yanıt olarak yürütülen olay işleyicisi, `Navigated` sayfa gezintisi oluştuğunda olay tetikleme, güncelleştirmeler başlık çubuğunda geri düğmesinin görünürlüğünü. Bu, başlık çubuğunu geri düğmesini uygulama içi geri yığını boş değilse görülebilir veya uygulama içi geri yığını boş ise başlık çubuğundaki kaldırıldı sağlar.

UWP geri gezinme desteği hakkında daha fazla bilgi için bkz. [Gezinme geçmişi ve geriye doğru UWP uygulamaları için Gezinti](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Özet

Yerel formlar izin Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-yerel Evrensel Windows Platformu (UWP) Xamarin.iOS ve Xamarin.Android projesi tarafından kullanılacak sayfaları türetilmiş. Yerel projeleri kullanabileceği `ContentPage`-projeye veya .NET Standard kitaplığı projesi ya da paylaşılan proje doğrudan eklenen sayfaları türetilmiş. Bu makalede nasıl kullanılacağı açıklanmıştır `ContentPage`-yerel projeleri ve bunlar arasında gezinmek nasıl doğrudan eklenen sayfaları türetilmiş.


## <a name="related-links"></a>İlgili bağlantılar

- [NativeForms (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Yerel Görünümler](~/xamarin-forms/platform/native-views/index.md)
