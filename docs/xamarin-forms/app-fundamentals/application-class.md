---
title: Uygulama sınıfı
description: C# veya XAML olabilecek varsayılan uygulama sınıfının özellikleri
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 1e1039f513534885dffe9fef348d567243651e22
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="app-class"></a>Uygulama sınıfı

`Application` Temel sınıf projeleri varsayılan olarak sunulan aşağıdaki özellikleri sunmaktadır `App` bir alt kümesi:

* A `MainPage` uygulama için başlangıç sayfasını Ayarla yerdir özelliği.
* Kalıcı bir [ `Properties` sözlük](#Properties_Dictionary) yaşam döngüsü durum değişiklikleri basit değerlerini depolamak için.
* Statik `Current` özelliği geçerli uygulama nesnesi için bir başvuru içeriyor.

Ayrıca sunan [yaşam döngüsü yöntemleri](~/xamarin-forms/app-fundamentals/app-lifecycle.md) gibi `OnStart`, `OnSleep`, ve `OnResume` kalıcı gezinti olaylarını yanı sıra.

Seçtiğiniz şablonun bağlı olarak, `App` sınıfı tanımlanması iki yoldan biriyle:

* **C#**, veya
* **XAML & C#**

Oluşturmak için bir **uygulama** XAML, varsayılan kullanılarak sınıf **uygulama** XAML ile sınıfı yerini **uygulama** sınıfı ve ilişkili arka plan kod, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

Aşağıdaki kod örneğinde ilişkili arka plan kodu gösterir:

```csharp
public partial class App : Application
{
    public App ()
    {
        InitializeComponent ();
        MainPage = new HomePage ();
    }
    ...
}
```

Ayar yanı sıra [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) özelliği, arka plan kodu ayrıca çağırmalısınız `InitializeComponent` yöntemi yüklenemedi ve ilişkili XAML ayrıştırılamadı.

## <a name="mainpage-property"></a>MainPage özelliği

`MainPage` Özellikte `Application` sınıfı uygulama kök sayfasının ayarlar.

Örneğin, mantığı oluşturabilirsiniz, `App` kullanıcının veya oturum açtığı bağlı olarak farklı bir sayfasında görüntülenecek sınıf.

`MainPage` Özelliği ayarlanmalıdır `App` Oluşturucusu

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }
}
```

<a name="Properties_Dictionary" />

## <a name="properties-dictionary"></a>Özellikler sözlüğü

`Application` Bir alt kümesi olan bir statik `Properties` özellikle verileri depolamak için kullanmak üzere kullanılabilecek sözlük `OnStart`, `OnSleep`, ve `OnResume` yöntemleri. Bu herhangi bir yere Xamarin.Forms kodu kullanarak erişilebilir `Application.Current.Properties`.

`Properties` Sözlük kullanan bir `string` anahtar ve depoları bir `object` değeri.

Örneğin, bir persistant ayarlayabilir `"id"` özelliği, kodunuzda herhangi bir yerde (bir öğe seçildiğinde, bir sayfanın içinde `OnDisappearing` yöntemi veya `OnSleep` yöntemi) şöyle:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

İçinde `OnStart` veya `OnResume` sonra bu değer kullanıcı deneyimini şekilde yeniden oluşturmak için kullanabileceğiniz yöntemleri. `Properties` Sözlük depoları `object`kullanmadan önce değerini dönüştürmek gereken şekilde s.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Her zaman anahtar varlığını beklenmeyen hataları önlemek için erişmeden önce denetleyin.

> [!NOTE]
> `Properties` Sözlük yalnızca ilkel türler depolama için seri hale. Diğer türleri depolama girişimi (gibi `List<string>`) sessizce başarısız olabilir.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Kalıcılığı

`Properties` Sözlük kaydedildi cihaza otomatik olarak.
Sözlüğe eklenen veriler uygulama arka planından döndürdüğünde veya hatta yeniden başlatıldıktan sonra kullanılabilir.

Xamarin.Forms 1.4 üzerinde ek bir yöntem sunulan `Application` sınıfı - `SavePropertiesAsync()` -hangi çağrılabilir proaktif olarak devam ettirmek için `Properties` sözlük. Bu özellikler sonra önemli güncelleştirmeleri kaydetmek yerine bunları çıkışı bir kilitlenme veya işletim sistemi tarafından sonlandırıldı nedeniyle seri getirilmemelidir risk izin vermektir.

Kullanarak başvurular bulabilirsiniz `Properties` sözlükte **Xamarin.Forms ile Mobile Apps oluşturma** kitap bölümlerde [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), ve [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)ve ilişkili [örnekleri](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Uygulama sınıfı

Bir tam `Application` sınıf uygulamasını aşağıda gösterilmektedir başvuru için:

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }

    protected override void OnStart()
    {
        // Handle when your app starts
        Debug.WriteLine ("OnStart");
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Debug.WriteLine ("OnSleep");
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
        Debug.WriteLine ("OnResume");
    }
}

```

Bu sınıf sonra her platforma özgü projesinde örneği ve geçirilen `LoadApplication` yeri olan yöntemini `MainPage` yüklendiğinde ve kullanıcıya gösterilir.
Her platform için kod aşağıdaki bölümlerde gösterilmiştir. En son Xamarin.Forms çözümünü şablonlar zaten uygulamanız için önceden yapılandırılmış tüm bu kodu içerir.


### <a name="ios-project"></a>iOS Project

İOS `AppDelegate` şimdi sınıfının devraldığı `FormsApplicationDelegate`. Olmalıdır:

* Çağrı `LoadApplication` örneğiyle birlikte `App` sınıfı.

* Her zaman geri `base.FinishedLaunching (app, options);`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="android-project"></a>Android Project

Android `MainActivity` şimdi devraldığı `FormsApplicationActivity`. İçinde `OnCreate` geçersiz kılma `LoadApplication` yöntemi örneği ile çağrılır `App` sınıfı.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

> [!NOTE]
> Var olan daha yeni bir [ `FormsAppCompatActivity` ](~/xamarin-forms/platform/android/appcompat.md) temel Android malzeme tasarım daha iyi desteklemek için kullanılan sınıfı.
> Bu varsayılan Android şablonu gelecekte olur, ancak izleyebilirsiniz [bu yönergeleri](~/xamarin-forms/platform/android/appcompat.md) mevcut Android uygulamalarınızı güncelleştirmek için.


### <a name="windows-phone-project"></a>Windows Phone Project

Ana sayfa (Silverlight tabanlı) Windows Phone projedeki alması gerektiğini `FormsApplicationPage`. Bunun anlamı XAML ve C# ' ta `MainPage` başvuru `FormsApplicationPage` sınıfında gösterildiği gibi.

XAML, özel bir ad alanı kullanır, böylece kök öğesi yansıtır `FormsApplicationPage` sınıfı:

```xaml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

C# öğesinden devralınan `FormsApplicationPage` sınıfı ve çağrıları `LoadApplication` , Xamarin.Forms örneği oluşturmak için `App`. Açıkça nitelemek için uygulama ad alanını kullanmak için iyi bir uygulama olduğuna dikkat edin `App` Windows Phone uygulamaları da kendi olduğundan `App` Xamarin.Forms için ilgisiz sınıfı.

```csharp
public partial class MainPage :
    global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3, use the correct namespace
    }
 }
```

### <a name="windows-81-project"></a>Windows 8.1 Project

Ana sayfanın [Windows 8.1 (WinRT tabanlı)](~/xamarin-forms/platform/windows/installation/tablet.md) projeleri şimdi öğesinden devralınan `WindowsPage`. XAML için yani `MainPage` başvuru `WindowsPage` sınıfında gösterildiği gibi:

XAML, özel bir ad alanı kullanır, böylece kök öğesi yansıtır `FormsApplicationPage` sınıfı:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
   ...>
</forms:WindowsPage>
```

C# arkasındaki koda 's yapım çağırmalısınız `LoadApplication` , Xamarin.Forms örneği oluşturmak için `App`. Açıkça nitelemek için uygulama ad alanını kullanmak için iyi bir uygulama olduğuna dikkat edin `App` UWP uygulamaları da kendi olduğundan `App` Xamarin.Forms için ilgisiz sınıfı.

```csharp
public partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();
        LoadApplication(new YOUR_APP_NAMESPACE.App());
    }
 }
```

Unutmayın `Forms.Init()` çağrılması gerekir **App.xaml.cs** satır 65 geçici.

### <a name="universal-windows-project-uwp-for-windows-10"></a>Windows 10 için evrensel Windows projesi (UWP)

[Evrensel Windows projesi](~/xamarin-forms/platform/windows/installation/universal.md) Xamarin.Forms desteği şu anda önizlemede.

Ana sayfanın UWP projesini alması gerektiğini `WindowsPage`. Bunun anlamı XAML ve C# ' ta `MainPage` başvuru `FormsApplicationPage` sınıfında gösterildiği gibi.

XAML, özel bir ad alanı kullanır, böylece kök öğesi yansıtır `FormsApplicationPage` sınıfı:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

C# arkasındaki koda 's yapım çağırmalısınız `LoadApplication` , Xamarin.Forms örneği oluşturmak için `App`. Açıkça nitelemek için uygulama ad alanını kullanmak için iyi bir uygulama olduğuna dikkat edin `App` UWP uygulamaları da kendi olduğundan `App` Xamarin.Forms için ilgisiz sınıfı.

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

Unutmayın `Forms.Init()` çağrılması gerekir **App.xaml.cs** satır 63 geçici.
