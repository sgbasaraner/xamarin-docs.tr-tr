---
title: Xamarin.Forms uygulaması sınıfı
description: Bu makalede, ilk sayfaya uygulaması için ayarlamak için bir özellik içeren varsayılan App sınıfının özellikleri açıklanmaktadır ve kalıcı bir sözlük basit değerlerden yaşam döngüsü durum değişikliklerini depolayın.
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 6de4380f2ce2d19df4ff912b7c86b75ca9e7821b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38999040"
---
# <a name="xamarinforms-app-class"></a>Xamarin.Forms uygulaması sınıfı

`Application` Temel sınıf projeleri varsayılan sunulan aşağıdaki özellikleri sunan `App` alt sınıf:

* A `MainPage` nerede uygulama için başlangıç sayfasını ayarlama özelliği.
* Bir kalıcı [ `Properties` sözlük](#Properties_Dictionary) basit değerlerden yaşam döngüsü durum değişiklikleri depolamak için.
* Statik `Current` geçerli uygulama nesnesine bir başvuru içeren özelliği.

Ayrıca kullanıma sunduğu [yaşam döngüsü yöntemleri](~/xamarin-forms/app-fundamentals/app-lifecycle.md) gibi `OnStart`, `OnSleep`, ve `OnResume` kalıcı gezinme olayları yanı sıra.

Seçtiğiniz şablona bağlı, `App` sınıfı iki yoldan biriyle açıklanabilir:

* **C#**, veya
* **XAML &AMP; C#**

Oluşturmak için bir **uygulama** kullanarak XAML, varsayılan **uygulama** ile bir XAML sınıfı yerine **uygulama** sınıfı ve ilişkili kodunu arka plan, aşağıdaki kod örneğinde gösterildiği gibi:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

Aşağıdaki kod örneği, ilişkili arka plan kod gösterir:

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

Ayar yanı sıra [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) özelliği, arka plan kod çağırmalıdır ayrıca `InitializeComponent` yöntemi yüklenemedi ve ilişkili XAML ayrıştırılamadı.

## <a name="mainpage-property"></a>MainPage özelliği

`MainPage` Özelliği `Application` sınıfı, uygulamanın kök sayfasına ayarlar.

Örneğin, mantık oluşturabilirsiniz, `App` kullanıcı veya oturum bağlı olarak farklı bir sayfayı görüntülemek için sınıf.

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

`Application` Öğesinin alt sınıfı olan statik `Properties` özellikle verileri depolamak için kullanılmak üzere kullanılabilecek sözlük `OnStart`, `OnSleep`, ve `OnResume` yöntemleri. Bu herhangi bir Xamarin.Forms kullanarak kodunuzdaki içinde erişilebilir `Application.Current.Properties`.

`Properties` Sözlük kullanan bir `string` anahtar ve depoları bir `object` değeri.

Örneğin, bir persistant ayarlayabilirsiniz `"id"` kodunuzdaki herhangi bir özelliği (bir öğe seçildiğinde, bir sayfanın içinde `OnDisappearing` yöntemi veya `OnSleep` yöntemi) şöyle:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

İçinde `OnStart` veya `OnResume` sonra bu değer kullanıcının deneyimini şekilde yeniden oluşturmak için kullanabileceğiniz yöntemler. `Properties` Sözlük depoları `object`kullanmadan önce değerine dönüştürün. böylece, s.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Her zaman beklenmeyen hataları önlemek için erişmeden önce anahtarın olup olmadığını denetleyin.

> [!NOTE]
> `Properties` Sözlük yalnızca ilkel türler depolama için seri hale. Diğer türleri depolama girişimi (gibi `List<string>`) sessizce başarısız olabilir.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Kalıcılığı

`Properties` Sözlük kaydedildiğinde cihaza otomatik olarak.
Sözlüğe eklenmiş olan veriler, uygulama arka planından döndürdüğünde veya hatta yeniden başlatıldıktan sonra kullanılabilir olacaktır.

Xamarin.Forms 1.4 ek bir yöntem üzerinde sunulan `Application` sınıfı - `SavePropertiesAsync()` -hangi çağrılabilir proaktif olarak kalıcı hale getirmek için `Properties` sözlüğü. Bu, önemli güncelleştirmelerinden sonra özelliklerini kaydetmek yerine, bunları dışarı bir kilitlenme veya işletim sistemi tarafından sonlandırılan nedeniyle seri getirilemedi risk izin vermektir.

Kullanarak başvurular bulabilirsiniz `Properties` sözlüğünde **Xamarin.Forms ile Mobile Apps oluşturma** kitap bölümleri [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), ve [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)ve ilişkili [örnekleri](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Uygulama sınıfı

Eksiksiz bir `Application` sınıf uygulamasını aşağıda gösterilmektedir başvuru için:

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

Bu sınıf sonra her platforma özgü projede örneği ve geçirilen `LoadApplication` yerdir yöntemi `MainPage` yüklendiğinde ve kullanıcıya gösterilir.
Aşağıdaki bölümlerde her platform için kod gösterilmektedir. En son Xamarin.Forms çözüm şablonları, uygulamanız için önceden yapılandırılmış tüm bu kodu zaten içerir.


### <a name="ios-project"></a>iOS projesi

İOS `AppDelegate` sınıfının devraldığı `FormsApplicationDelegate`. Olmalıdır:

* Çağrı `LoadApplication` örneğiyle birlikte `App` sınıfı.

* Her zaman dönüş `base.FinishedLaunching (app, options);`.

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

### <a name="android-project"></a>Android projesi

Android `MainActivity` devraldığı `FormsAppCompatActivity`. İçinde `OnCreate` geçersiz kılma `LoadApplication` örneği ile yöntemi çağrıldığında `App` sınıfı.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="universal-windows-project-uwp-for-windows-10"></a>Windows 10 için evrensel Windows projesi (UWP)

Bkz: [kurulum Windows projeleri](~/xamarin-forms/platform/windows/installation/index.md) Xamarin.Forms UWP desteği hakkında daha fazla bilgi için.

UWP projesi ana sayfada alması gerektiğini `WindowsPage`:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

C# codebehind's yapım çağırmalıdır `LoadApplication` bir örneği, bir Xamarin.Forms oluşturmak için `App`. Açıkça nitelemek için uygulama ad alanını kullanacak şekilde iyi olduğuna dikkat edin `App` UWP uygulamaları da kendi olduğundan `App` için Xamarin.Forms ilgisi olmayan sınıf.

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

Unutmayın `Forms.Init()` çağrılması gerekir **App.xaml.cs** 63 satırına yakın bir yerde.
