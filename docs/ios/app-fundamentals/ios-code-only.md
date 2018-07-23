---
title: Xamarin.iOS kodunda iOS kullanıcı arabirimi oluşturma
description: Bu belge, bir Xamarin.iOS uygulaması için bir kullanıcı arabirimi oluşturmak için kod kullanmayı açıklar. Bu görünüm denetleyicileri, döndürmeyi ve daha fazla işleme bir görünüm hiyerarşisi derlenirken açıklanır.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: 5e9bf9555d10c8b34ad9323529d4af5ea66110f8
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156788"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Xamarin.iOS kodunda iOS kullanıcı arabirimi oluşturma

Bir mağaza gibi bir iOS uygulamasının kullanıcı arabirimidir: uygulama bir pencere genellikle alır, ancak ihtiyaç duyduğu ve düzenleme ve nesnelere bağlı olarak bir uygulama altyapılarını değiştirilebilir gibi birçok nesne istediği gibi görüntülemek bu pencereyi ile dolabilir. Bu senaryoda - kullanıcının gördüğü şeyler - nesneleri görünümleri çağrılır. Uygulamadaki tek bir ekran oluşturmak için görünümleri diğer üzerine bir içerik görünümü hiyerarşide dizilir ve hiyerarşisini tek bir görünüm denetleyicisi tarafından yönetilir. Birden çok ekrana uygulamalarla birden çok içerik görünümü, her biri kendi görünüm denetleyicisi hiyerarşilerinizin ve uygulama görünümleri, kullanıcı farklı bir içerik görünümü ekranda tabanlı hiyerarşi oluşturmak için pencere yerleştirir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aşağıdaki diyagramda, cihaz ekran için kullanıcı arabirimi Getir penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkiler gösterilmektedir: 

[![](ios-code-only-images/image9.png "Bu diyagram penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkileri gösterir")](ios-code-only-images/image9.png#lightbox)

Bu görünüm hiyerarşileri kullanılarak oluşturulabilir. [iOS için Xamarin Tasarımcı](~/ios/user-interface/designer/index.md) Visual Studio'da, ancak bunu tamamen kod içinde nasıl temel bir anlayışa sahip iyi. Bu makale, bazı temel noktaları öğrenin ve çalışır durumda yalnızca kod kullanıcı arabirimi geliştirme rehberlik yapacaktır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Aşağıdaki diyagramda, cihaz ekran için kullanıcı arabirimi Getir penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkiler gösterilmektedir: 

[![](ios-code-only-images/image9.png "Bu diyagram penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkileri gösterir")](ios-code-only-images/image9.png#lightbox)

Bu görünüm hiyerarşileri kullanılarak oluşturulabilir. [iOS için Xamarin Tasarımcı](~/ios/user-interface/designer/index.md) Mac için Visual Studio'da, ancak bunu tamamen kod içinde nasıl temel bir anlayışa sahip iyi. Bu makale, bazı temel noktaları öğrenin ve çalışır durumda yalnızca kod kullanıcı arabirimi geliştirme rehberlik yapacaktır.

-----

## <a name="creating-a-code-only-project"></a>Yalnızca kod projesi oluşturma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>iOS boş proje şablonu

İlk olarak, Visual Studio kullanarak bir iOS projesi oluşturun. **Dosya > Yeni Proje > Visual C# > iPhone & iPad > iOS uygulaması (Xamarin)** proje, aşağıda gösterilen:

[![Yeni Proje iletişim kutusu](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

Ardından **boş uygulama** proje şablonu:

[![Bir şablon iletişim seçin](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

Boş proje şablonu 4 dosyayı projeye ekler:

[![Proje dosyalarını](ios-code-only-images/empty-project.w157-sml.png "proje dosyaları")](ios-code-only-images/empty-project.w157.png#lightbox)


1. **AppDelegate.cs** -içeren bir `UIApplicationDelegate` alt `AppDelegate` , iOS uygulama olaylarını işlemek için kullanılır. Uygulama penceresinin oluşturulan `AppDelegate`'s `FinishedLaunching` yöntemi.
1. **Main.cs** -sınıfını belirtir uygulamanın giriş noktasını içeren için `AppDelegate` .
1. **Info.plist** -uygulama yapılandırma bilgilerini içeren özellik listesi dosyası.
1. **Entitlements.plist** – uygulama izinleri ve özellikleri hakkında bilgi içeren özellik listesi dosyası.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="ios-templates"></a>iOS şablonları


Mac için Visual Studio, boş bir şablon sağlamaz. Birincil şekilde bir kullanıcı Arabirimi oluşturmak için Apple önerir görsel taslak desteğiyle tüm şablonları gelir. Ancak, kullanıcı Arabirimi tamamen kodda oluşturmak mümkündür. 

Aşağıdaki adımları uygulamadan film şeridi kaldırma yoluyla Kılavuzu: 


1. Yeni bir iOS projesi oluşturmak için tek görünüm uygulaması şablonu kullanın:
    
    [![](ios-code-only-images/single-view-app.png "Tek görünüm uygulaması şablonunu kullanma")](ios-code-only-images/single-view-app.png#lightbox)

1. Silme `Main.Storyboard` ve `ViewController.cs` dosyaları. Yapmak **değil** Sil `LaunchScreen.Storyboard`. Arka plan kod içinde bir film şeridini oluşturulan bir görünüm denetleyicisi için olduğu gibi görünüm denetleyicisi silinmesi gerekir:
1. Seçtiğinizden emin olun **Sil** açılır iletişim kutusunda:
    
    [![](ios-code-only-images/delete.png "Açılan iletişim kutusundan Sil'i seçin")](ios-code-only-images/delete.png#lightbox)

1. Info.plist dosyasında içindeki bilgileri Sil **dağıtım bilgisi > ana arabirimi** seçeneği:
    
    [![](ios-code-only-images/main-interface.png "Ana arabirimi seçeneği içindeki bilgileri Sil")](ios-code-only-images/main-interface.png#lightbox)

1. Son olarak, aşağıdaki kodu ekleyin, `FinishedLaunching` AppDelegate sınıfında yöntemi:
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }

Eklenen kodu `FinishedLaunching` yöntemidir, yukarıdaki 5. adımda en düşük iOS uygulamanız için bir pencere oluşturmak için gereken kod miktarını.


-----



iOS uygulamaları kullanılarak oluşturulur [MVC örüntüsü](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). Görüntüleyen bir uygulama ilk ekran pencerenin kök görünümü denetleyicisi oluşturulur. Bkz: [iOS çok ekranlı Hello](~/ios/get-started/hello-ios-multiscreen/index.md) MVC hakkında daha fazla ayrıntı kendisini desen için yol.

Uygulamasını `AppDelegate` tarafından eklenen şablon uygulama penceresinin, burada her iOS uygulaması için yalnızca bir tane olan ve aşağıdaki kod ile görünür hale getirir oluşturan:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    public override UIWindow Window
            {
                get;
                set;
            }

    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        window.MakeKeyAndVisible();

        return true;
    }
}
```

Şimdi bu uygulamayı çalıştırmak için olsaydı, büyük olasılıkla belirten bir durum elde edebileceğiniz `Application windows are expected to have a root view controller at the end of application launch`. Şimdi bir denetleyici ekleyin ve uygulamanın kök görünüm denetleyicisi yapın.

## <a name="adding-a-controller"></a>Denetleyici ekleme

Uygulamanızı birçok görünüm denetleyicileri içerebilir ancak bu bir kök görünümü tüm görünüm denetleyicileri denetlemek için denetleyici sahip olması gerekir.  Denetleyici oluşturarak penceresine ekleme bir `UIViewController` örneği ve ayarı `window.RootViewController` özelliği:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;

        Window.RootViewController = controller;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Her denetleyici erişilebilir bir ilişkili görünüme sahip `View` özelliği. Yukarıdaki kod görünümün değişiklikleri `BackgroundColor` özelliğini `UIColor.LightGray` , aşağıda gösterildiği gibi görünür, böylece:

 [![](ios-code-only-images/image1.png "Görünümün arka plan olan bir görünen açık gri")](ios-code-only-images/image1.png#lightbox)

Biz herhangi ayarlayabilirsiniz `UIViewController` alt sınıf olarak `RootViewController` bu şekilde Uıkit aynı zamanda biz yazma kendimize genel denetleyicilerinden de dahil. Örneğin, aşağıdaki kodu ekler bir `UINavigationController` olarak `RootViewController`:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;
        controller.Title = "My Controller";

        var navController = new UINavigationController(controller);

        Window.RootViewController = navController;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Bu, aşağıda gösterildiği gibi Gezinti denetleyicisi içinde iç içe denetleyici oluşturur:

 [![](ios-code-only-images/image2.png "Denetleyici Gezinti denetleyicisi içinde iç içe geçmiş")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Görünüm denetleyicisi oluşturmak

Bir denetleyiciyi ekleme gördük göre `RootViewController` pencerenin kodda bir özel görünüm denetleyicisi oluşturmak nasıl görelim.

Adlı yeni bir sınıf ekleyin `CustomViewController` aşağıda gösterildiği gibi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "CustomViewController adlı yeni bir sınıf ekleyin")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](ios-code-only-images/new-file.png "CustomViewController adlı yeni bir sınıf ekleyin")](ios-code-only-images/new-file.png#lightbox)

-----

Sınıf alması gerektiğini `UIViewController`, içinde `UIKit` gösterildiği gibi ad alanı:

```csharp
using System;
using UIKit;

namespace CodeOnlyDemo
{
    class CustomViewController : UIViewController
    {
    }
}
```

<a name="Initializing_the_View"/>

## <a name="initializing-the-view"></a>Görünüm başlatılırken

`UIViewController` adlı bir yöntem içerir `ViewDidLoad` görünüm denetleyicisi ilk belleğe yüklendiğinde çağrılır. Bu görünümün özelliklerini ayarlamak gibi başlatma yapmak için uygun bir yerdir.

Örneğin, aşağıdaki kod, bir düğme ve düğmeye basıldığında gezinme yığınında üzerine yeni bir görünüm denetleyicisi göndermek için bir olay işleyicisi ekler:

```csharp
using System;
using CoreGraphics;
using UIKit;

namespace CodyOnlyDemo
{
    public class CustomViewController : UIViewController
    {
        public CustomViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            View.BackgroundColor = UIColor.White;
            Title = "My Custom View Controller";

            var btn = UIButton.FromType (UIButtonType.System);
            btn.Frame = new CGRect (20, 200, 280, 44);
            btn.SetTitle ("Click Me", UIControlState.Normal);

            var user = new UIViewController ();
            user.View.BackgroundColor = UIColor.Magenta;

            btn.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (user, true);
            };

            View.AddSubview (btn);

        }
    }
}
```

Uygulamanızda Bu denetleyici yük ve basit Gezinti göstermek için yeni bir örneğini oluşturma `CustomViewController`. Yeni bir gezinti denetleyicisi oluşturmak, görünüm denetleyicisi Örneğinizde geçirin ve pencerenin yeni gezinti denetleyicisi ayarlamak `RootViewController` içinde `AppDelegate` önceki gibi:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Şimdi uygulamayı yüklediğinde, `CustomViewController` içinde bir gezinti denetleyicisi yüklenir:

 [![](ios-code-only-images/customvc.png "İçinde bir gezinti denetleyicisi CustomViewController yüklenir")](ios-code-only-images/customvc.png#lightbox)
 
Düğmeye tıklandığında olacak _anında iletme_ gezinme yığınında üzerine yeni bir görünüm denetleyicisi:

[![](ios-code-only-images/customvca.png "Gezinme yığınına yeni bir görünüm denetleyicisi gönderildi")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Görünüm hiyerarşisi derlenirken

Yukarıdaki örnekte, bir düğme için Görünüm denetleyicisi ekleyerek kod içinde bir kullanıcı arabirimi oluşturma başlattık.

iOS kullanıcı arabirimleri görünüm hiyerarşisi oluşur. Etiketler, düğmeler, kaydırıcıları vb. gibi ek görünümler bazı üst görünümün subviews eklenir.

Örneğin, düzenleyelim `CustomViewController` burada kullanıcının girebileceği kullanıcı adı ve parola bir oturum açma ekranı oluşturmak için. Ekranda bir düğmeyi ve iki metin alanları içerir.

### <a name="adding-the-text-fields"></a>Metin alanları ekleme

İlk olarak, eklenen düğmesi ve olay işleyici Kaldır [görünüm başlatılırken](#Initializing_the_View) bölümü. 

Kullanıcı adı için bir denetim oluşturma ve başlatma ekleme bir `UITextField` ve ardından görünümü hiyerarşi için aşağıda gösterildiği gibi ekleme:

```csharp
class CustomViewController : UIViewController
{
    UITextField usernameField;

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        View.BackgroundColor = UIColor.Gray;

        nfloat h = 31.0f;
        nfloat w = View.Bounds.Width;

        usernameField = new UITextField
        {
            Placeholder = "Enter your username",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 82, w - 20, h)
        };

        View.AddSubview(usernameField);
    }
}
```

Biz oluşturduğunuzda `UITextField`, ayarladığımız `Frame` konumunu ve boyutunu tanımlamak için özellik. Sol üstteki x + ile sağa 0,0 koordinatı iOS olduğu ve + y aşağı. Ayarlanmasından sonra `Frame` diğer birkaç özellikleriyle birlikte diyoruz `View.AddSubview` eklemek için `UITextField` için hiyerarşisini görüntüle. Böylece `usernameField` , bir alt Görünüm `UIView` örneğin `View` özelliği başvuruları. Bir alt görünüm bir z üst görünüm ekranında önünde görünür, böylece kendi üst görünümü yüksek sıralı ile eklenir.

Uygulama ile `UITextField` dahil aşağıda gösterilmiştir:

 [![](ios-code-only-images/image4.png "Dahil edilen UITextField uygulamayla")](ios-code-only-images/image4.png#lightbox)

Ekleyebiliriz bir `UITextField` benzer bir biçimde parola için yalnızca bu kez ayarladığımız `SecureTextEntry` özelliğini aşağıda gösterildiği gibi true olarak:

```csharp
public class CustomViewController : UIViewController
{
    UITextField usernameField, passwordField;
    public override void ViewDidLoad()
    {
       // keep the code the username UITextField
        passwordField = new UITextField
        {
            Placeholder = "Enter your password",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 114, w - 20, h),
            SecureTextEntry = true
        };

      View.AddSubview(usernameField); 
      View.AddSubview(passwordField);
   }
}

```

Ayarı `SecureTextEntry = true` girilen metni gizler `UITextField` aşağıda gösterildiği gibi kullanıcı tarafından:

 [![](ios-code-only-images/image4a.png "SecureTextEntry ayarı true kullanıcı tarafından girilen metin gizler")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Düğme ekleme

Ardından, kullanıcının kullanıcı adı ve parola göndermek için bir düğme ekleyeceğiz. Düğme gibi başka herhangi bir denetim hiyerarşisini görüntüle üst görünümün için bağımsız değişken olarak geçirerek eklenir `AddSubview` yeniden yöntemi.

Aşağıdaki kod, bir düğme ekler ve bir olay işleyicisi için kaydeder `TouchUpInside` olay:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Bu yerinde oturum açma ekranı aşağıda gösterildiği gibi görünür:

 [![](ios-code-only-images/image5.png "Oturum açma ekranı")](ios-code-only-images/image5.png#lightbox)

İOS, önceki sürümlerde varsayılan düğme arka saydam benzemez. Düğmenin değiştirme `BackgroundColor` özelliğini bu değiştirir:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Tipik Kenar Çizgili düğme yuvarlanır yerine bu de bir kare düğme neden olur. Yuvarlatılmış edge almak için aşağıdaki kod parçacığını kullanın:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Bu değişikliklerle görünümü şöyle görünür:

[![](ios-code-only-images/image6.png "Bir görünümün örneği Çalıştır")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Birden çok görünüm için Görünüm hiyerarşisi ekleme

iOS kullanarak birden çok görünüm görünüm hiyerarşiye eklemek için bir olanak sağlar `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Düğme işlevsellik ekleme

Bir düğmeye tıklandığında, kullanıcılarınızın birşeyler olmasını bekler. Örneğin, bir uyarı gösterilir veya başka bir ekran için Gezinti gerçekleştirilir. 

Gezinme yığınına ikinci bir görünüm denetleyicisi gönderecek bazı kodları ekleyelim.

İlk olarak, ikinci bir görünüm denetleyicisi oluşturun:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Ardından, işlevsellik eklemek `TouchUpInside` olay:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Gezinti aşağıda gösterilmiştir:

[![](ios-code-only-images/navigation.png "Gezinti Bu grafikte gösterilmiştir.")](ios-code-only-images/navigation.png#lightbox)

Bir gezinti denetleyicisi kullandığınızda varsayılan olarak, iOS uygulamanın bir gezinti çubuğu ve geri dönün yığın içinde olanak tanımak için geri düğmesi verdiğini dikkat edin.

## <a name="iterating-through-the-view-hierarchy"></a>Görünüm hiyerarşisi aracılığıyla yineleme

Alt Görünüm hiyerarşisi aracılığıyla yineleme yapmak ve herhangi belirli bir görünüm çekme mümkündür. Örneğin, her bulmak için `UIButton` ve bu düğme farklı bir verin `BackgroundColor`, aşağıdaki kod parçacığı kullanılabilir

```csharp
foreach(var subview in View.Subviews)
{
    if (subview is UIButton)
    {
         var btn = subview as UIButton;
         btn.BackgroundColor = UIColor.Green;
    }
}
```

Bu, ancak çalışmaz için yinelendiğinde görünümü ise bir `UIView` tüm görünümlere olarak geri gelecek şekilde bir `UIView` üst görünümde eklenen nesneler olarak kendilerini devral `UIView`.

## <a name="handling-rotation"></a>Döndürmeyi işleme

Kullanıcı cihaz yatay olarak döndürür, aşağıdaki ekran görüntüsünde gösterildiği gibi denetimleri gerektiği gibi yeniden boyutlandırma:

 [![](ios-code-only-images/image7.png "Kullanıcı cihaz yatay olarak döndürür, denetimleri uygun şekilde yeniden boyutlandırma")](ios-code-only-images/image7.png#lightbox)

Bu düzeltmenin bir yolu olan ayarlayarak `AutoresizingMask` her görünüm hakkında daha fazla özellik. Her ayarlamanız ki bu durumda yatay olarak, esnetme için denetimleri istiyoruz `AutoresizingMask`. Aşağıdaki örnek için `usernameField`, ancak aynı görünümü hiyerarşideki her bir aracı uygulanması gerekir.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Biz cihaz veya simülatör döndürürken, artık her şeyi ek alanı dolduracak şekilde aşağıda gösterildiği gibi genişletir:

 [![](ios-code-only-images/image8.png "Tüm denetimleri ek alanı dolduracak şekilde uzatılır")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Özel görünümlerini oluşturma

Uıkit parçası olan denetimler kullanmanın yanı sıra özel görünümlerinizi de kullanılabilir. Özel Görünüm devralarak oluşturulabilir `UIView` ve geçersiz kılma `Draw`. Şimdi özel görünüm oluşturma ve göstermek için Görünüm hiyerarşisini ekleyin.

### <a name="inheriting-from-uiview"></a>Uıview devralıyor

Yapmamız gereken ilk şey, özel görünüm için bir sınıf oluşturmaktır. Biz kullanarak gerçekleştirirsiniz **sınıfı** adlı boş bir sınıf eklemek için Visual Studio şablon `CircleView`. Temel sınıf ayarlanmış olması gerekir `UIView`, biz geri çağırma içinde olduğu `UIKit` ad alanı. Ayrıca ihtiyacımız olanlar `System.Drawing` de ad alanı. Diğer çeşitli `System.*` ad alanları olmayacaktır Bu örnekte kullanılan, bu nedenle bunları kaldırmanız çekinmeyin.

Sınıfı, şu şekilde görünmelidir:

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>İçinde bir Uıview çizme

Her `UIView` sahip bir `Draw` yeniden çizilmesi gerektiğinde sistem tarafından çağrılan yöntem. `Draw` hiçbir zaman doğrudan çağrılmalıdır. Bu, sistem tarafından çalışma çevrim işleme sırasında çağrılır. İlk kez bir görünüm, görünüm hiyerarşiye eklendikten sonra çalışma döngü kendi `Draw` yöntemi çağrılır. Yapılan sonraki çağrılar `Draw` görünümü ya da çağırarak çizilecek gerek kalmadan olarak işaretlendiğinde gerçekleşir `SetNeedsDisplay` veya `SetNeedsDisplayInRect` görünümü.

Çizim kodu bizim görünümüne geçersiz kılınan içinde bu tür kod ekleyerek ekleyebiliriz `Draw` aşağıda gösterildiği gibi yöntemi:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    //get graphics context
    using (var g = UIGraphics.GetCurrentContext())
    {
        // set up drawing attributes
        g.SetLineWidth(10.0f);
        UIColor.Green.SetFill();
        UIColor.Blue.SetStroke();

        // create geometry
        var path = new CGPath();
        path.AddArc(Bounds.GetMidX(), Bounds.GetMidY(), 50f, 0, 2.0f * (float)Math.PI, true);

        // add geometry to graphics context and draw
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);
    }
}
```

Bu yana `CircleView` olduğu bir `UIView`, biz de ayarlayabilirsiniz `UIView` özellikleri de. Örneğin, biz ayarlayabilirsiniz `BackgroundColor` oluşturucu içinde:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Kullanılacak `CircleView` ile eşleşmediğinden şu ya da bir alt görünüm var olan bir denetleyici görünümü hiyerarşide ekleyebilirsiniz, yeni oluşturduğumuz `UILabels` ve `UIButton` daha önce veya size yeni bir denetleyici görünüm olarak yükleyebilirsiniz. İkinci olarak inceleyelim.

### <a name="loading-a-view"></a>Görünüm yükleniyor

 `UIViewController` adlı bir yöntemi olan `LoadView` alt görünüm oluşturmak için denetleyici tarafından çağrılır. Bu bir görünümünü oluşturmak ve denetleyicinin atama için uygun bir yerdir `View` özelliği.

Bir denetleyici ihtiyacımız ilk olarak, bu nedenle adlı yeni boş bir sınıf oluşturmak `CircleController`.

İçinde `CircleController` ayarlamak için aşağıdaki kodu ekleyin `View` için bir `CircleView` (değil, çağırmalıdır `base` geçersiz kılma uygulamasında):

```csharp
using UIKit;

namespace CodeOnlyDemo
{
    class CircleController : UIViewController
    {
        CircleView view;

        public override void LoadView()
        {
            view = new CircleView();
            View = view;
        }
    }
}
```

Son olarak, çalışma zamanında denetleyici sunmak ihtiyacımız var. Şimdi bunu şu şekilde daha önce eklediğimiz Gönder düğmesinin üzerindeki bir olay işleyicisi ekleyerek yapabilirsiniz:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Şimdi uygulamayı çalıştırabilir ve Gönder düğmesine dokunun, bir daire ile yeni görünüm görüntülenir:

 [![](ios-code-only-images/circles.png "Yeni bir daire ile görüntülenir")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Başlatma ekranı oluşturma

A [başlatma ekranı](~/ios/app-fundamentals/images-icons/launch-screens.md) yanıt verip vermediğini kullanıcılarınıza görüntülenecek bir yolu olarak, uygulamanız başlatıldığında görüntülenir. Uygulamanızı yüklenirken bir başlatma ekranı görüntülendiğinden, uygulama hala belleğe yüklenen olarak kodu oluşturulamıyor. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zaman, proje Visual Studio'da bir başlatma ekranı sizin bulunabilir bir .xib dosya biçiminde sağlanır iOS oluşturma **kaynakları** klasörün içinde projenizin. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Olduğunda, Mac, bir başlatma ekranı sağlanır için farklı bir görsel taslak dosyası biçiminde, Visual Studio'da bir iOS projesi oluşturun. 

-----

Çift üzerine tıklayarak ve iOS Designer'daki açmadan tarafından düzenlenebilir.

Apple iOS 8'i hedefleyen uygulamalar için kullanılan bir .xib veya görsel taslak dosyası veya iOS Designer'daki ya da dosya başlattığında, daha sonra Boyut sınıfları ve otomatik düzen iyi görünüyor ve tüm cihazlar için doğru görüntüler düzeninizi uyarlamak için kullanacağınız önerir. boyutları. Statik başlatma resmi .xib veya görsel taslak yanı sıra önceki sürümlerini hedefleyen uygulamalar için desteği sağlamak için kullanılabilir.

Bir başlatma ekranı oluşturma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Bir .xib kullanarak bir başlatma ekranı oluşturma](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Başlatma ekranları görsel Taslaklar ile yönetme](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> İOS 9 itibariyle, Film Şeritlerini bir başlatma ekranı oluşturma birincil yöntemi olarak kullanılması gereken Apple önerir.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Bir başlatma resmi öncesi iOS 8 uygulamaları oluşturma

Uygulamanızdaki iOS 8 önceki sürümlerini hedefliyse statik bir resim .xib veya başlatma ekranı görsel taslak ek olarak kullanılabilir. 

Bu statik görüntü Info.plist veya uygulamanızdaki (iOS 7 için) bir varlık kataloğu olarak ayarlayabilirsiniz. Uygulamanızı çalıştırın her cihaz boyutu (320 x 480, 640 x 960, 640 x 1136) için ayrı görüntülere sağlamanız gerekir. Başlatma ekran boyutları hakkında daha fazla bilgi için görüntüleme [başlatma ekran görüntüleri](~/ios/app-fundamentals/images-icons/launch-screens.md) Kılavuzu.

> [!IMPORTANT]
> Uygulamanızın başlatma ekran varsa, onu tam ekrana sığmayan fark edebilirsiniz. Bu durumda, en az adlı bir 640 x 1136 resim eklemek emin olmalısınız `Default-568@2x.png` Info.plist için. 



## <a name="summary"></a>Özet

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu makalede ele alınan program aracılığıyla Visual Studio'da iOS uygulamalarını nasıl geliştirebileceğinizi. Boş proje şablonundan bir proje oluşturmaya nasıl oluşturabilir ve kök görünüm denetleyicisi penceresine tartışıyor inceledik. Biz, ardından Uıkit denetimleri uygulama ekran geliştirmek için bir denetleyici içinde bir görünüm hiyerarşi oluşturmak için nasıl kullanılacağını gösterdi. Sonraki biz nasıl görünümleri yapmak için uygun şekilde farklı yönleri düzenlemek ve sınıflara göre özel bir görünüm oluşturmak nasıl gördüğümüz incelenirken `UIView`, de nasıl bir denetleyici içinde görünümü yüklenemedi. Son olarak bir uygulama için bir başlatma ekranı ekleme incelediniz.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu makalede ele alınan Mac için Visual Studio içinde program aracılığıyla iOS uygulamalarını nasıl geliştirebileceğinizi Tek görünüm şablondan bir proje oluşturmaya nasıl oluşturabilir ve kök görünüm denetleyicisi penceresine tartışıyor inceledik. Biz, ardından Uıkit denetimleri uygulama ekran geliştirmek için bir denetleyici içinde bir görünüm hiyerarşi oluşturmak için nasıl kullanılacağını gösterdi. Sonraki biz nasıl görünümleri yapmak için uygun şekilde farklı yönleri düzenlemek ve sınıflara göre özel bir görünüm oluşturmak nasıl gördüğümüz incelenirken `UIView`, de nasıl bir denetleyici içinde görünümü yüklenemedi. Son olarak bir uygulama için bir başlatma ekranı ekleme incelediniz.

-----




## <a name="related-links"></a>İlgili bağlantılar

- [SimpleLogin (örnek)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
