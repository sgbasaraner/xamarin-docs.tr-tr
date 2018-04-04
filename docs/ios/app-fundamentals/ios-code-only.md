---
title: Kodda iOS kullanıcı arabirimleri oluşturma
description: Xamarin.iOS bir kullanıcı arabirimi, uygulamanız için – kodda veya iOS için Xamarin Tasarımcısı ile oluşturma için iki yöntem sunar. Bu makalede iOS tamamen kod içinde kullanıcı arabirimleri oluşturma inceler. Bir uygulama ekran bir denetleyicide Uıkit görünümleri hiyerarşisini oluşturarak oluşturmak için bir proje şablonu başlangıç kullanmayı gösterir. Ardından, bir denetleyicisine yüklenebilir özel görünümleri oluşturma açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7e8460d2c946159a9869322d6d4944d213d3d801
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-ios-user-interfaces-in-code"></a>Kodda iOS kullanıcı arabirimleri oluşturma

_Xamarin.iOS bir kullanıcı arabirimi, uygulamanız için – kodda veya iOS için Xamarin Tasarımcısı ile oluşturma için iki yöntem sunar. Bu makalede iOS tamamen kod içinde kullanıcı arabirimleri oluşturma inceler. Bir uygulama ekran bir denetleyicide Uıkit görünümleri hiyerarşisini oluşturarak oluşturmak için bir proje şablonu başlangıç kullanmayı gösterir. Ardından, bir denetleyicisine yüklenebilir özel görünümleri oluşturma açıklanmaktadır._

Kullanıcı bir iOS uygulaması gibi bir mağaza arabirimdir – uygulama genellikle bir pencere alır, ancak bu pencereyi ile birçok nesnede gerekir ve nesneleri ve düzenlemeleri ne uygulamayı görüntülemek istediği bağlı olarak değiştirilebilir dolabilir. Bu senaryoda - kullanıcının gördüğü şeyler - nesneleri görünümleri denir. Bir uygulamadaki tek bir ekran oluşturmak için görünümleri diğer en üstünde bir içerik görünümü hiyerarşide yığılma ve hiyerarşisini tek bir görünüm denetleyici tarafından yönetilir. Birden çok ekran uygulamaları birden çok içerik görünümü hiyerarşi, her biri kendi görünüm denetleyicisi sahip ve kullanıcı açıktır, farklı içerik Görünümü ekranında dayalı bir hiyerarşide oluşturmak için penceresinde görünümleri uygulama yerleştirir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aşağıdaki diyagram aygıt ekranına kullanıcı arabirimi Getir penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkileri gösterir: 

[![](ios-code-only-images/image9.png "Bu diyagram penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkileri gösterir")](ios-code-only-images/image9.png#lightbox)

Bu görünüm hiyerarşileri kullanılarak oluşturulabilir [iOS için Xamarin Tasarımcısı](~/ios/user-interface/designer/index.md) Visual Studio'da, ancak bunu tamamen kod içinde çalışma konusunda temel bilgiye sahip iyi. Bu makalede bazı hale getirmek için temel noktaları ve yalnızca kod kullanıcı arabirimi geliştirme ile çalışmaya anlatılmaktadır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Aşağıdaki diyagram aygıt ekranına kullanıcı arabirimi Getir penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkileri gösterir: 

[![](ios-code-only-images/image9.png "Bu diyagram penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkileri gösterir")](ios-code-only-images/image9.png#lightbox)


Bu görünüm hiyerarşileri kullanılarak oluşturulabilir [iOS için Xamarin Tasarımcısı](~/ios/user-interface/designer/index.md) Mac için Visual Studio'da, ancak bunu tamamen kod içinde çalışma konusunda temel bilgiye sahip iyi. Bu makalede bazı hale getirmek için temel noktaları ve yalnızca kod kullanıcı arabirimi geliştirme ile çalışmaya anlatılmaktadır.


-----

## <a name="creating-a-code-only-project"></a>Yalnızca kod projesi oluşturma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>iOS boş proje şablonu

İPhone kullanarak Visual Studio'da ilk olarak, bir iOS projesi oluşturun **boş proje** şablonu, aşağıda gösterilen denetleyicileri ve görünümleri eklemek için genişletilen.


[![](ios-code-only-images/blankapp-vs.png "Yeni Proje iletişim kutusu")](ios-code-only-images/blankapp-vs.png#lightbox)


Boş proje şablonu 4 dosyaları projeye ekler:


[![](ios-code-only-images/empty-project.png "Proje dosyaları")](ios-code-only-images/empty-project.png#lightbox)


1. **AppDelegate.cs** -içeren bir `UIApplicationDelegate` alt `AppDelegate` , iOS uygulama olayları işlemek için kullanılır. Uygulama penceresi oluşturulur `AppDelegate`'s `FinishedLaunching` yöntemi.
1. **Main.cs** -sınıfı tanımlar uygulaması için giriş noktası içeren için `AppDelegate` .
1. **Info.plist** -uygulama yapılandırma bilgilerini içeren özellik listesi dosyası.
1. **Entitlements.plist** – uygulama izinleri ve özellikleri hakkında bilgi içeren özellik listesi dosyası.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="ios-templates"></a>iOS şablonları


Mac için Visual Studio, boş bir şablon sağlamaz. Bir kullanıcı Arabirimi oluşturmak için birincil şekilde Apple önerir film şeridi desteğiyle tüm şablonları gelir. Ancak, kullanıcı Arabirimi tamamen kodda oluşturmak mümkündür. 

Aşağıdaki adımları uygulamadan film şeridi kaldırma aracılığıyla Kılavuzu: 


1. Yeni bir iOS projesi oluşturmak için tek görünüm uygulaması şablonu kullanın:
    
    [![](ios-code-only-images/single-view-app.png "Tek görünüm uygulaması şablonu kullanın")](ios-code-only-images/single-view-app.png#lightbox)

1. Silme `Main.Storyboard` ve `ViewController.cs` dosyaları. Yapmak **değil** silmek `LaunchScreen.Storyboard`. Arka plan kod film şeridi oluşturulan görünümü denetleyici için olduğu gibi görünüm denetleyicisini silinmesi gerekir:
1. Seçtiğinizden emin olun **silmek** açılan iletişim kutusunda:
    
    [![](ios-code-only-images/delete.png "Açılan iletişim kutusundan Sil'i seçin")](ios-code-only-images/delete.png#lightbox)

1. İçindeki bilgileri Info.plist dosyasında silme **dağıtım bilgileri > ana arabirimi** seçeneği:
    
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


-----

## <a name="creating-a-window"></a>Bir pencere oluşturma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Eklenen kod `FinishedLaunching` yöntemdir adım 3 Yukarıdaki kod iOS uygulamanız için bir pencere oluşturmak için gereken en düşük miktarı.  

-----

iOS uygulamaları kullanan yerleşik [MVC örüntüsü](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). Bir uygulama görüntüler ilk ekran pencerenin kök görünümü denetleyicisinden oluşturulur. Bkz: [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) MVC hakkında daha fazla ayrıntı kendisini desen için yol.

Uygulamasını `AppDelegate` tarafından eklenen şablon uygulama penceresi, hangi var. her iOS uygulaması için yalnızca bir tane olduğundan ve aşağıdaki kod ile görünür hale getirir oluşturur:

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

Şimdi bu uygulamayı çalıştırmak için olsaydı, büyük olasılıkla belirten bir durum elde edebileceğiniz `Application windows are expected to have a root view controller at the end of application launch`. Şimdi bir denetleyici ekleyin ve uygulamanın kök görünümü denetleyicisi yapın.

## <a name="adding-a-controller"></a>Bir denetleyici ekleme

Uygulamanızı pek çok görünüm denetleyicileri içerebilir, ancak bir kök görünümü tüm görünüm denetleyicileri denetlemek için denetleyicisi olması gerekir.  Denetleyici oluşturarak penceresine ekleme bir `UIViewController` örneği ve ayarlamak `window.RootViewController` özelliği:

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

Erişilebilir bir ilişkili görünümü her denetleyicisine sahip `View` özelliği. Yukarıdaki kod görünümün değiştirir `BackgroundColor` özelliğine `UIColor.LightGray` , aşağıda gösterildiği gibi görünür, böylece:

 [![](ios-code-only-images/image1.png "Görünümün arka plan görünür açık gri olduğunu")](ios-code-only-images/image1.png#lightbox)

Biz herhangi ayarlayabilirsiniz `UIViewController` alt sınıfı olarak `RootViewController` bu şekilde Uıkit hem de biz yazma kendisini denetleyicilerinden de dahil. Örneğin, aşağıdaki kod ekler bir `UINavigationController` olarak `RootViewController`:

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

Bu gezinti denetleyiciyle aşağıda gösterildiği gibi iç içe geçmiş denetleyicisi oluşturur:

 [![](ios-code-only-images/image2.png "Gezinti denetleyiciyle iç içe geçmiş denetleyicisi")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Bir görünüm denetleyicisini oluşturma

Bir denetleyici olarak ekleme gördük göre `RootViewController` pencerenin kodda bir özel görünüm denetleyicisi oluşturmak nasıl görelim.

Adlı yeni bir sınıf ekleyin `CustomViewController` aşağıda gösterildiği gibi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.png "CustomViewController adlı yeni bir sınıf ekleyin")](ios-code-only-images/customviewcontroller.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](ios-code-only-images/new-file.png "CustomViewController adlı yeni bir sınıf ekleyin")](ios-code-only-images/new-file.png#lightbox)

-----

Sınıf alması gerektiğini `UIViewController`, içinde olduğu `UIKit` gösterildiği gibi ad:

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

## <a name="initializing-the-view"></a>Görünüm başlatılıyor

`UIViewController` adlı bir yöntem içerir `ViewDidLoad` görünüm denetleyicisini ilk belleğe yüklendiğinde çağrılır. Bu, onun özelliklerini ayarlama gibi görünümün başlatma yapmak için uygun bir yerdir.

Örneğin, aşağıdaki kod, bir düğmeyi ve yeni bir görünüm denetleyicisi Gezinti yığına düğmesine basıldığında göndermek için bir olay işleyicisi ekler:

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

Bu denetleyici uygulamanızda yüklemek ve basit Gezinti göstermek için yeni bir örneğini oluşturmak `CustomViewController`. Yeni bir gezinti denetleyicisi, görünüm denetleyicisini örneğinizi geçirmek oluşturup yeni gezinti denetleyicisi pencerenin `RootViewController` içinde `AppDelegate` olarak önce:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Şimdi uygulamayı yüklediğinde, `CustomViewController` Gezinti denetleyicisi yüklenir:

 [![](ios-code-only-images/customvc.png "CustomViewController içinde bir gezinti denetleyicisi yüklendi")](ios-code-only-images/customvc.png#lightbox)
 
Düğmesini tıklatarak olacak _itme_ Gezinti yığına yeni bir görünüm denetleyicisi:

[![](ios-code-only-images/customvca.png "Yeni bir görünüm denetleyicisi Gezinti yığına gönderilir")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Görünüm hiyerarşisine oluşturma

Yukarıdaki örnekte, biz görünüm denetleyiciye bir düğme ekleyerek kod içinde bir kullanıcı arabirimi oluşturma başladı.

iOS kullanıcı arabirimleri görünüm hiyerarşisini oluşur. Etiketler, düğmeleri, kaydırıcılar, vb. gibi ek görünümler bazı üst görünüm subviews eklenir.

Örneğin, düzenleyelim `CustomViewController` burada kullanıcının girebileceği bir kullanıcı adı ve parola oturum açma ekranı oluşturma. Ekran bir düğmeyi ve iki metin alanları içerir.

### <a name="adding-the-text-fields"></a>Metin alanları ekleme

İlk olarak, eklenmiştir düğmesi ve olay işleyiciyi kaldırmak [görünümü başlatma](#Initializing_the_View) bölümü. 

Kullanıcı adı için bir denetim oluşturma ve başlatma eklemek bir `UITextField` ve sonra Görünüm hiyerarşi için aşağıda gösterildiği gibi ekleme:

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

Biz oluşturduğunuzda `UITextField`, ayarlarız `Frame` konumunu ve boyutunu tanımlamak için özellik. Sol üst köşedeki ile + x sağa 0,0 koordinatı iOS olduğu ve + aşağı y. Ayar sonra `Frame` diğer birkaç özellikleriyle birlikte diyoruz `View.AddSubview` eklemek için `UITextField` görünüm hiyerarşisine. Böylece `usernameField` , alt Görünüm `UIView` örneğine `View` özelliği başvuruları. Bir alt görünüm bir z-ekranın üst görünümde önünde göründüğü şekilde, kendi üst görünümü yüksektir sırası ile eklenir.

Uygulama ile `UITextField` dahil aşağıda gösterilmiştir:

 [![](ios-code-only-images/image4.png "Uygulama dahil UITextField ile")](ios-code-only-images/image4.png#lightbox)

Ekleyebiliriz bir `UITextField` benzer bir şekilde parolasını yalnızca bu kez ayarlarız `SecureTextEntry` özelliği true, aşağıda gösterildiği gibi:

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

Ayarı `SecureTextEntry = true` girilen metin gizler `UITextField` aşağıda gösterildiği gibi kullanıcı tarafından:

 [![](ios-code-only-images/image4a.png "SecureTextEntry ayarı true kullanıcı tarafından girilen metin gizler")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Düğme ekleme

Ardından, kullanıcının kullanıcı adı ve parola göndermek için bir düğmeye ekleyeceğiz. Düğme hiyerarşisini gibi diğer herhangi bir denetimi görüntüleme üst görünümün için bağımsız değişken olarak geçirerek eklenir `AddSubview` yeniden yöntemi.

Aşağıdaki kod düğmesi ekler ve bir olay işleyicisi için kaydeder `TouchUpInside` olay:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Bu yerinde oturum açma ekranı artık aşağıda gösterildiği gibi görünür:

 [![](ios-code-only-images/image5.png "Oturum açma ekranı")](ios-code-only-images/image5.png#lightbox)

İOS, önceki sürümlerde varsayılan düğme arka planını saydam benzemez. Düğmenin değiştirilmesine `BackgroundColor` özellik bu değişiklikleri:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Kenar Çizgili düğmesi normal yuvarlatılmış yerine bu bir kare düğmesini neden olacaktır. Yuvarlak bir kenar elde etmek için aşağıdaki kod parçacığında kullanın:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Bu değişikliklerle görünümü şuna benzeyecektir:

[![](ios-code-only-images/image6.png "Görünümün bir örnek Çalıştır")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Görünüm hiyerarşisi birden çok görünüm ekleme

iOS kullanarak görünüm hiyerarşisi birden çok görünüm ekleme olanağı sağlar `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Düğme işlevsellik ekleme

Düğme tıklatıldığında, kullanıcılarınızın şey olmasını bekler. Örneğin, bir uyarı gösterilir veya başka bir ekrana Gezinti gerçekleştirilir. 

İkinci bir görünüm denetleyicisini Gezinti yığına itilecek biraz kod ekleyelim.

İlk olarak, İkinci görünüm denetleyicisini oluşturun:

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

[![](ios-code-only-images/navigation.png "Gezinti Bu grafikte gösterildiği")](ios-code-only-images/navigation.png#lightbox)

Bir gezinti denetleyicisi kullandığınızda, varsayılan olarak, iOS uygulamanın gezinti çubuğu ve yığınından geri taşımanıza olanak sağlamak için bir geri düğmesini sağlar dikkat edin.

## <a name="iterating-through-the-view-hierarchy"></a>Görünüm hiyerarşide yineleme

Alt Görünüm hiyerarşisi aracılığıyla yinelemek ve herhangi bir belirli görünüm çekme mümkündür. Örneğin, her bulmak için `UIButton` ve bu düğme farklı bir verin `BackgroundColor`, aşağıdaki kod parçacığında kullanılabilir

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

Bu, ancak çalışmaz için yinelendiğinde görünümü ise bir `UIView` tüm görünümleri olarak geri gelecektir gibi bir `UIView` üst görünüme eklenen nesneler olarak kendilerini devral `UIView`.

## <a name="handling-rotation"></a>İşleme döndürme

Kullanıcı yatay aygıta döndürür, aşağıdaki ekran görüntüsünde gösterildiği gibi denetimleri gerektiği gibi yeniden boyutlandırma değil:

 [![](ios-code-only-images/image7.png "Kullanıcı yatay aygıta döndürür, denetimleri uygun şekilde yeniden boyutlandırma")](ios-code-only-images/image7.png#lightbox)

Bu sorunu gidermek için bir yoldur ayarlayarak `AutoresizingMask` her görünüm özelliği. Her ayarlarız böylece bu durumda yatay uzatmak için denetimleri istiyoruz `AutoresizingMask`. Aşağıdaki örnek içindir `usernameField`, ancak aynı görünüm hiyerarşideki her aracı uygulanması gerekir.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Biz cihaz veya simulator döndürdüğünüzde, şimdi her şeyi ek alan doldurmak için aşağıda gösterildiği gibi uzatılır:

 [![](ios-code-only-images/image8.png "Tüm denetimler ek alanı dolduracak şekilde uzatılır")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Özel görünümler oluşturma

Uıkit parçası olan denetimleri kullanarak ek olarak, özel görünümler de kullanılabilir. Özel bir görünüm devralarak oluşturulabilir `UIView` ve geçersiz kılma `Draw`. Şimdi özel görünüm oluşturmak ve göstermek için Görünüm hiyerarşisine ekleyin.

### <a name="inheriting-from-uiview"></a>UIView devralma

Yapmamız gereken ilk şey özel görünüm için bir sınıf oluşturun. Biz kullanarak gerçekleştirirsiniz **sınıfı** adlı boş bir sınıf eklemek için Visual Studio şablon `CircleView`. Taban sınıfı ayarlanmalıdır `UIView`, biz geri çağırma içinde olduğu `UIKit` ad alanı. Biz de gerekir `System.Drawing` de ad alanı. Diğer çeşitli `System.*` ad alanları olmayacaktır Bu örnekte kullanılan, böylece bunları kaldırmak çekinmeyin.

Sınıfı aşağıdaki gibi görünmelidir:

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>Bir UIView çizme

Her `UIView` sahip bir `Draw` çizilmesi gerektiğinde sistem tarafından çağrılan yöntemi. `Draw` hiçbir zaman doğrudan çağrılmalıdır. Sistem tarafından çalıştırma döngü işleme sırasında adı verilir. İlk kez bir görünüm görünüm hiyerarşisine eklendikten sonra çalışma döngü kendi `Draw` yöntemi çağrılır. Sonraki çağrılar `Draw` görünümü ya da çağırarak çizilmesi gerek olarak işaretlendiğinde oluşur `SetNeedsDisplay` veya `SetNeedsDisplayInRect` görünüm.

Çizim kodunu bizim görünümüne geçersiz kılınan içinde bu tür kod ekleyerek ekleyebiliriz `Draw` aşağıda gösterildiği gibi yöntemi:

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

Bu yana `CircleView` olan bir `UIView`, biz de ayarlayabilirsiniz `UIView` özellikleri de. Örneğin, biz ayarlayabilirsiniz `BackgroundColor` oluşturucuda:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Kullanılacak `CircleView` yeni oluşturduğumuz, ile yaptığımız gibi biz ya da bir alt görünüm varolan bir denetleyiciyi görünüm hiyerarşisinde ekleyebilirsiniz `UILabels` ve `UIButton` daha önce veya Biz yeni bir denetleyici görünüm olarak yükleyebilirsiniz. Şimdi ikinci yapın.

### <a name="loading-a-view"></a>Bir görünüm yükleniyor

 `UIViewController` sahip adlı bir yöntem `LoadView` kendi görünüm oluşturmak için denetleyici tarafından çağrılır. Bu bir görünüm oluşturmak ve denetleyicinin atamak için uygun bir yerdir `View` özelliği.

Bir denetleyici ihtiyacımız ilk olarak, bu nedenle adlı yeni bir boş sınıf oluşturmak `CircleController`.

İçinde `CircleController` ayarlamak için aşağıdaki kodu ekleyin `View` için bir `CircleView` (değil çağırmalıdır `base` geçersiz kılma uygulamasında):

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

Son olarak, çalışma zamanında denetleyicisi sunmak ihtiyacımız. Şimdi daha önce aşağıdaki gibi eklediğimiz Gönder düğmesi olay işleyicisi ekleyerek bunu:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Şimdi uygulamayı çalıştırın ve Gönder düğmesine dokunun bir daire ile yeni görünüm görüntülenir:

 [![](ios-code-only-images/circles.png "Yeni bir daire görüntülenir")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Başlatma ekranı oluşturma

A [başlatma ekranı](~/ios/app-fundamentals/images-icons/launch-screens.md) uygulamanızı yanıt verdiğini kullanıcılarınızın görüntülemek için bir yol olarak başlatıldığında görüntülenir. Uygulamanızı yüklenirken Başlatma ekranı görüntülendiğinden, uygulama hala belleğe yüklenen gibi kodda oluşturulamıyor. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zaman, proje Visual Studio'da bir başlatma ekranı, bulunabilir bir .xib dosyası biçiminde sağlanır iOS oluşturma **kaynakları** projenizi bir klasöre. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Olduğunda, Mac, başlatma ekranı sağlanır, film şeridi dosya biçiminde bir iOS projesi Visual Studio'da oluşturmak. 

-----

Çift üzerine tıklayarak ve iOS Tasarımcısı açmadan tarafından düzenlenebilir.

Apple iOS 8 hedefleyen uygulamalar için kullanılan .xib veya film şeridi dosya veya Tasarımcısı iOS ya da dosyasında başlattığında, daha sonra boyutu sınıfları ve Otomatik Yerleşim iyi arar ve tüm aygıt için doğru görüntüler düzeninizi uyarlamak için kullanacağınız önerir boyutları. Bir statik başlatma görüntüsü .xib veya film şeridi yanı sıra önceki sürümlerini hedefleyen uygulamalar için destek izin vermek için kullanılabilir.

Başlatma ekranı oluşturma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Bir .xib kullanarak başlatma ekranı oluşturma](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Film şeritleri ile başlatma ekranları yönetme](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Film şeritleri başlatma ekranı oluşturma birincil yöntemi olarak kullanılması gereken Apple iOS 9'dan sonra önerir.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Başlatma görüntü öncesi iOS 8 uygulamaları oluşturma

Uygulama sürümleri iOS 8 Geri'yi hedefliyorsa statik görüntü .xib veya film şeridi başlatma ekranı ek olarak kullanılabilir. 

Bu statik görüntü Info.plist dosyasında veya bir varlık kataloğunda (iOS 7 için) uygulamanızın olarak ayarlayabilirsiniz. Uygulamanızı çalışabilir her aygıt boyutu (320 x 480, 640 x 960, 640 x 1136) için ayrı görüntülere sağlamanız gerekir. Başlatma ekranı boyutları hakkında daha fazla bilgi için görüntülemek [başlatma ekran görüntüleri](~/ios/app-fundamentals/images-icons/launch-screens.md) Kılavuzu.

> [!IMPORTANT]
> Uygulamanızı başlatma ekran, ekran tam olarak uymayan karşılaşabilirsiniz. Bu durumda, en az adlı bir 640 x 1136 görüntüsü eklediğinizden emin olmalısınız `Default-568@2x.png` , Info.plist için. 



## <a name="summary"></a>Özet

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu makalede, Visual Studio'da program aracılığıyla iOS uygulamaları geliştirmek nasıl ele. Boş bir proje şablonu, bir proje oluşturmak nasıl nasıl oluşturulacağı ve kök görünümü denetleyici penceresine ekleme ele inceledik. Biz sonra Uıkit denetimlerden bir uygulama ekran geliştirmek için bir görünüm hiyerarşisinde bir denetleyicisi oluşturmak için nasıl kullanılacağı gösterilmiştir. Sonraki biz nasıl görünümleri yapmak için uygun şekilde farklı yönleri düzenlemek ve sınıflara göre özel görünüm oluşturmak nasıl gördüğümüz incelenmesi `UIView`, de nasıl bir denetleyici görünümde yüklenemiyor. Son olarak bir uygulama başlatma ekranı eklemek nasıl incelediniz.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da program aracılığıyla iOS uygulamaları nasıl geliştireceğinizi de bu makalede ele alınan Tek bir Görünüm şablonundan bir projeyi oluşturmak nasıl nasıl oluşturulacağı ve kök görünümü denetleyici penceresine ekleme ele inceledik. Biz sonra Uıkit denetimlerden bir uygulama ekran geliştirmek için bir görünüm hiyerarşisinde bir denetleyicisi oluşturmak için nasıl kullanılacağı gösterilmiştir. Sonraki biz nasıl görünümleri yapmak için uygun şekilde farklı yönleri düzenlemek ve sınıflara göre özel görünüm oluşturmak nasıl gördüğümüz incelenmesi `UIView`, de nasıl bir denetleyici görünümde yüklenemiyor. Son olarak bir uygulama başlatma ekranı eklemek nasıl incelediniz.

-----




## <a name="related-links"></a>İlgili bağlantılar

- [SimpleLogin (örnek)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
