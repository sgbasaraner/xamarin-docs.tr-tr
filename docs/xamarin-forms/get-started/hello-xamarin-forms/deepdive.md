---
title: Xamarin.Forms yakından
description: Bu makalede Xamarin.Forms kullanarak uygulama geliştirme temelleri inceler. Kapsanan konular bir Xamarin.Forms uygulaması, mimari ve uygulama temelleri ve kullanıcı arabirimi anatomisi dahil.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d97aa580-1eb9-48b3-b15b-0d7421ea7ae
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: f51992ec5311bdf0c7df7478651398f6ed8491a9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996239"
---
# <a name="xamarinforms-deep-dive"></a>Xamarin.Forms yakından

İçinde [Xamarin.Forms hızlı](~/xamarin-forms/get-started/hello-xamarin-forms/quickstart.md), Phoneword uygulama oluşturuldu. Bu makalede, bir Xamarin.Forms uygulamaların nasıl çalıştığı temellerini anlamak için nelerin oluşturulduğunu incelenir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Visual Studio'ya giriş

Visual Studio, Microsoft güçlü bir ıde'dir. Bu tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir bütünleştirilmiş kod tarayıcı, kaynak kodu tümleştirmesi ve diğer özellikleri. Bu makalede, Visual Studio temel özelliklerinden bazıları Xamarin eklenti kullanmaya odaklanmıştır.

Visual Studio kod halinde düzenler *çözümleri* ve *projeleri*. Bir çözüm bir veya daha fazla proje tutan bir kapsayıcıdır. Bir proje, bir uygulama, destek kitaplığı, bir test uygulaması ve daha fazla olabilir. Phoneword uygulama, aşağıdaki ekran görüntüsünde gösterildiği gibi dört projeleri içeren bir çözüm oluşur.

![](deepdive-images/vs/solution.png "Visual Studio Çözüm Gezgini")

Projeler şunlardır:

- Phoneword – Bu, tüm paylaşılan kullanıcı Arabirimi ve paylaşılan kodu içeren .NET Standard kitaplığı projesi projesidir.
- Phoneword.Android – bu proje Android özel kodun bulunduğu ve Android uygulaması için giriş noktasıdır.
- Phoneword.iOS – bu proje iOS özel kodun bulunduğu ve iOS uygulaması için giriş noktasıdır.
- Phoneword.UWP – bu proje, Evrensel Windows Platformu (UWP) belirli bir kod tutar ve UWP uygulaması için giriş noktasıdır.

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms uygulaması anatomisi

Aşağıdaki ekran görüntüsünde, Visual Studio'da Phoneword .NET Standard kitaplığı projesi içeriğini gösterir:

![](deepdive-images/vs/net-standard-project.png "Phoneword .NET Standard projesinin içeriği")

Proje bir **bağımlılıkları** içeren düğüm **NuGet** ve **SDK** düğümleri. **NuGet** düğüm Xamarin.Forms NuGet paketini projeye eklenen içerir ve **SDK** düğüm içeriyor `NETStandard.Library` NuGet paketlerini kümesinin tamamını başvuran metapackage .NET Standard tanımlayan.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Mac için Visual Studio'ya giriş

Mac için Visual Studio bir ücretsiz, açık kaynaklı Visual Studio'ya benzer bir ıde'dir. Bu tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir bütünleştirilmiş kod tarayıcı, kaynak kodu tümleştirmesi ve diğer özellikleri. Mac için Visual Studio hakkında daha fazla bilgi için bkz. [Mac için Visual Studio ile tanışın](/visualstudio/mac/).

Mac için Visual Studio aşağıdaki koda düzenleme Visual Studio uygulaması *çözümleri* ve *projeleri*. Bir çözüm bir veya daha fazla proje tutan bir kapsayıcıdır. Bir proje, bir uygulama, destek kitaplığı, bir test uygulaması ve daha fazla olabilir. Phoneword uygulama, aşağıdaki ekran görüntüsünde gösterildiği gibi üç projeleri içeren bir çözüm oluşur.

![](deepdive-images/xs/solution.png "Çözüm bölmesi Mac için Visual Studio")

Projeler şunlardır:

- Phoneword – Bu, tüm paylaşılan kullanıcı Arabirimi ve paylaşılan kodu içeren .NET Standard kitaplığı projesi projesidir.
- Phoneword.Droid – bu proje Android özel kodun bulunduğu ve Android uygulamaları için giriş noktasıdır.
- Phoneword.iOS – bu proje iOS özel kodun bulunduğu ve iOS uygulamaları için giriş noktasıdır.

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms uygulaması anatomisi

Aşağıdaki ekran görüntüsünde, Mac için Visual Studio'da Phoneword .NET Standard kitaplığı projesi içeriğini gösterir:

![](deepdive-images/xs/library-project.png "Phoneword .NET Standard kitaplığı projesi içeriği")

Proje bir **bağımlılıkları** içeren düğüm **NuGet** ve **SDK** düğümleri. **NuGet** düğüm Xamarin.Forms NuGet paketini projeye eklenen içerir ve **SDK** düğüm içeriyor `NETStandard.Library` NuGet paketlerini kümesinin tamamını başvuran metapackage .NET Standard tanımlayan.

-----

Proje, ayrıca dosyaların bir dizi oluşur:

- **App.XAML** – için XAML biçimlendirmesi `App` sınıfını, ki bu uygulama için bir kaynak sözlüğü tanımlar.
- **App.xaml.cs** – için gerideki `App` sınıfı her platformda uygulama tarafından görüntülenen ilk sayfa örnekleme ve uygulama yaşam döngüsü olaylarını işleme sorumludur.
- **IDialer.cs** – `IDialer` belirten arabirimi `Dial` yöntemi tüm sınıflar tarafından sağlanması gerekir.
- **MainPage.xaml** – için XAML biçimlendirmesi `MainPage` uygulamayı başlattığında gösterilen sayfası kullanıcı Arabiriminde tanımlayan sınıf.
- **MainPage.xaml.cs** – için gerideki `MainPage` kullanıcı sayfası ile etkileşim kurduğunda çalıştırılan iş mantığı içeren sınıf.
- **PhoneTranslator.cs** – çağrılır bir telefon numarasına bir telefon sözcük oluşturmaktan sorumludur iş mantığı **MainPage.xaml.cs**.

Bir Xamarin.iOS uygulaması anatomisi hakkında daha fazla bilgi için bkz. [Xamarin.iOS uygulamasının anatomisi](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy). Bir Xamarin.Android uygulaması anatomisi hakkında daha fazla bilgi için bkz. [bir Xamarin.Android uygulaması anatomisi](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Mimari ve uygulama temelleri

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.Forms uygulaması, geleneksel bir platformlar arası uygulama aynı şekilde geliştirilmiştir. Paylaşılan kod genellikle .NET standart kitaplığa yerleştirilir ve paylaşılan kod platforma özel uygulamalar kullanır. Bu ilişki Phoneword uygulama için genel bir bakış Aşağıdaki diyagramda gösterilmiştir:

![](deepdive-images/vs/architecture.png "Phoneword mimarisi")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Xamarin.Forms uygulaması, geleneksel bir platformlar arası uygulama aynı şekilde geliştirilmiştir. Paylaşılan kod genellikle .NET standart kitaplığa yerleştirilir ve paylaşılan kod platforma özel uygulamalar kullanır. Bu ilişki Phoneword uygulama için genel bir bakış Aşağıdaki diyagramda gösterilmiştir:

![](deepdive-images/xs/architecture.png "Phoneword mimarisi")

-----

Yeniden başlatma kodunu kullanımını en üst düzeye çıkarmak için Xamarin.Forms uygulamaları adlı tek bir sınıf sahip `App` sorumlu aşağıdaki kod örneğinde gösterildiği gibi her platformda uygulama tarafından görüntülenen ilk sayfa oluşturmak için:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace Phoneword
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new MainPage();
        }
        ...
    }
}
```

Bu kod ayarlar `MainPage` özelliği `App` sınıfı için yeni bir örneğini [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) sınıfı. Ayrıca, [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) özniteliği, böylece doğrudan Ara dilinde derlenmiş XAML XAML derleyicide kapatır. Daha fazla bilgi için [XAML derleme](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Her platformda uygulama başlatılıyor

### <a name="ios"></a>iOS

İos'ta ilk Xamarin.Forms sayfasını başlatmak için Phoneword.iOS projeyi içeren `AppDelegate` öğesinden devralınan sınıf `FormsApplicationDelegate` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
namespace Phoneword.iOS
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init ();
            LoadApplication (new App ());
            return base.FinishedLaunching (app, options);
        }
    }
}
```

`FinishedLaunching` Geçersiz kılma çağırarak Xamarin.Forms framework başlatır `Init` yöntemi. Bu kök görünüm denetleyicisi çağrısıyla ayarlanmadan önce uygulamanın yüklenmesi için Xamarin.Forms iOS özel uygulanışı neden `LoadApplication` yöntemi.

### <a name="android"></a>Android

Android ilk Xamarin.Forms sayfasını başlatmak için Phoneword.Droid projesi oluşturan kodu içerir bir `Activity` ile `MainLauncher` özniteliğiyle dan devralan etkinlik `FormsAppCompatActivity` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
namespace Phoneword.Droid
{
    [Activity(Label = "Phoneword", 
              Icon = "@mipmap/icon", 
              Theme = "@style/MainTheme", 
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        internal static MainActivity Instance { get; private set; }

        protected override void OnCreate(Bundle bundle)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(bundle);
            Instance = this;
            global::Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App());
        }
    }
}
```

`OnCreate` Geçersiz kılma çağırarak Xamarin.Forms framework başlatır `Init` yöntemi. Bu uygulamada Xamarin.Forms uygulama yüklenmeden önce yüklenmesi için Xamarin.Forms Android özel uygulanışı neden olur. Ayrıca, `MainActivity` sınıfı depolar kendisine bir başvuru `Instance` özelliği. `Instance` Özelliği yerel bağlamı olarak bilinir ve içinden başvuruda `PhoneDialer` sınıfı.

## <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Evrensel Windows Platformu (UWP) uygulamaları `Init` Xamarin.Forms framework başlatan yöntem ınvoked from `App` sınıfı:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Bu, uygulamanın yüklenmesi için Xamarin.Forms UWP özel uygulanışı neden olur. İlk Xamarin.Forms sayfası tarafından başlatılan `MainPage` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
namespace Phoneword.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Phoneword.App());
        }
    }
}
```

Xamarin.Forms uygulaması ile yüklenen `LoadApplication` yöntemi.

> [!NOTE]
> Xamarin.Forms ile oluşturulmuş, ancak yalnızca Windows üzerinde Visual Studio kullanarak evrensel Windows Platformu (UWP) uygulamaları olabilir.

## <a name="user-interface"></a>Kullanıcı Arabirimi

Xamarin.Forms uygulaması kullanıcı arabirimi oluşturmak için kullanılan dört ana denetim Grup vardır.

1. **Sayfaları** – Xamarin.Forms sayfaları, platformlar arası mobil uygulama ekranları temsil eder. Phoneword uygulamanın kullandığı [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) tek bir ekranda görüntülemek için sınıf. Sayfaları hakkında daha fazla bilgi için bkz. [Xamarin.Forms sayfaları](~/xamarin-forms/user-interface/controls/pages.md).
1. **Düzenleri** – Xamarin.Forms düzenleri, mantıksal yapılarda görünümleri oluşturmak için kullanılan kapsayıcılardır. Phoneword uygulamanın kullandığı [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) denetimleri yatay bir yığın halinde düzenlemek için sınıf. Düzenleri hakkında daha fazla bilgi için bkz: [Xamarin.Forms Layouts](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Görünümleri** – Xamarin.Forms görünümleri etiketleri, düğme ve metin girişi kutusu gibi kullanıcı arabiriminde görüntülenen denetimleri vardır. Phoneword uygulamanın kullandığı [ `Label` ](xref:Xamarin.Forms.Label), [ `Entry` ](xref:Xamarin.Forms.Entry), ve [ `Button` ](xref:Xamarin.Forms.Button) kontrol eder. Görünümler hakkında daha fazla bilgi için bkz. [Xamarin.Forms görünümleri](~/xamarin-forms/user-interface/controls/views.md).
1. **Hücreleri** – Xamarin.Forms hücre bir listedeki öğeler için kullanılan özel öğe ve listedeki her öğeye nasıl çizileceğini açıklanmaktadır. Uygulama kullanmayan Phoneword tüm hücreleri kullanın. Hücreleri hakkında daha fazla bilgi için bkz: [Xamarin.Forms hücreleri](~/xamarin-forms/user-interface/controls/cells.md).

Çalışma zamanında hangi işlenmiş olan yerel eşdeğerine, her denetimi eşleştirilir.

Herhangi bir platformda Phoneword uygulamayı çalıştırdığınızda, karşılık gelen tek bir ekranda görüntüler. bir [ `Page` ](xref:Xamarin.Forms.Page) Xamarin.Forms içinde. Bir `Page` temsil eden bir *ViewGroup* Android, bir *görünüm denetleyicisi* ios'ta, veya bir *sayfa* Evrensel Windows platformu üzerinde. Phoneword uygulama ayrıca oluşturur bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) temsil eden nesne `MainPage` olan XAML biçimlendirme aşağıdaki kod örneğinde gösterilen sınıfı:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                         xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                         x:Class="Phoneword.MainPage">
        ...
        <StackLayout>
            <Label Text="Enter a Phoneword:" />
            <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
            <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
            <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
</ContentPage>
```

`MainPage` Sınıfını kullanan bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) otomatik olarak ekran boyutu ne olursa olsun ekrandaki denetimleri düzenlemek için denetimi. Konumlandırılmış art arda, dikey içinde eklendikleri sırayla her bir alt öğesidir. `StackLayout` Denetimi içeren bir [ `Label` ](xref:Xamarin.Forms.Label) denetimi sayfasında, metni görüntülemek için bir [ `Entry` ](xref:Xamarin.Forms.Entry) metinsel kullanıcı girişini ve iki kabul etmek için Denetim [ `Button` ](xref:Xamarin.Forms.Button) dokunma olayları yanıtta kod yürütmek için kullanılan denetimler.

Xamarin.Forms içinde XAML hakkında daha fazla bilgi için bkz. [Xamarin.Forms XAML Temelleri](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="responding-to-user-interaction"></a>Kullanıcı etkileşimine yanıt verme

XAML içinde tanımlanmış bir nesne arka plan kod dosyasında işlenen bir olay tetikleyebilir. Aşağıdaki örnekte gösterildiği kod `OnTranslate` yöntemi için gerideki `MainPage` yanıt olarak yürütülen sınıf [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) üzerinde tetikleme olay *çevir* düğmesi.

```csharp
void OnTranslate(object sender, EventArgs e)
{
    translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
    if (!string.IsNullOrWhiteSpace (translatedNumber)) {
        callButton.IsEnabled = true;
        callButton.Text = "Call " + translatedNumber;
    } else {
        callButton.IsEnabled = false;
        callButton.Text = "Call";
    }
}
```

`OnTranslate` Yöntemi, karşılık gelen telefon numarası ve yanıt phoneword çevirir, arama çubuğunda özelliklerini ayarlar. XAML sınıfı için arka plan kod dosyası ile atanan ad kullanarak XAML içinde tanımlanan bir nesneye erişebilirsiniz `x:Name` özniteliği. Bir harf veya alt çizgi ile başlamalı ve hiç katıştırılmış boşluklar içeremez, bu öznitelik için atanan değer C# değişkenleri olarak aynı kurallara sahiptir.

Çeviri düğmenin kablolama `OnTranslate` yöntemi ortaya için XAML biçimlendirmesi içinde `MainPage` sınıfı:

```xaml
<Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword içinde sunulan ek kavramları

Xamarin.Forms Phoneword uygulamayı birkaç kavram bu makalede ele alınmayan kullanıma sundu. Bu kavramlar şunları içerir:

- Etkinleştirme ve düğmelerini devre dışı bırakma. A [ `Button` ](xref:Xamarin.Forms.Button) açıp değiştirerek değiştirilebilir, [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) özelliği. Örneğin, aşağıdaki örnekte devre dışı bırakır kod `callButton`:

    ```csharp
    callButton.IsEnabled = false;
    ```

- Uyarı iletişim kutusu görüntüleniyor. Kullanıcı, arama bastığında **düğmesi** Phoneword uygulama gösterir bir *uyarı iletişim kutusu* getirin ya da bir aramayı iptal et seçeneğine sahip. [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) Yöntemi kullanılır iletişim kutusu oluşturmak için aşağıdaki kod örneğinde gösterildiği gibi:

    ```csharp
    await this.DisplayAlert (
            "Dial a Number",
            "Would you like to call " + translatedNumber + "?",
            "Yes",
            "No");
    ```

- Yerel özellikleri aracılığıyla erişimi [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) sınıfı. Phoneword uygulamanın kullandığı `DependencyService` çözmek için sınıf `IDialer` Phoneword projeden aşağıdaki kod örneğinde gösterildiği gibi platforma özgü telefon arama uygulamaları için arabirim:

    ```csharp
    async void OnCall (object sender, EventArgs e)
    {
        ...
        var dialer = DependencyService.Get<IDialer> ();
        ...
    }
    ```

  Hakkında daha fazla bilgi için [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) sınıfı [erişen yerel özellikler DependencyService aracılığıyla](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

- Bir URL ile bir telefon araması yapılıyor. Phoneword uygulamanın kullandığı `OpenURL` Sistem telefon uygulamasını başlatmak için. URL oluşan bir `tel:` iOS projeden aşağıdaki kod örneğinde gösterildiği gibi önek ardından çağrılacak, telefon numarası:

    ```csharp
    return UIApplication.SharedApplication.OpenUrl (new NSUrl ("tel:" + number));
    ```

- Platform düzenini ince ayar yapma. [ `Device` ](xref:Xamarin.Forms.Device) Sınıfı, farklı kullanan aşağıdaki kod örneğinde gösterildiği gibi bir platform başına temelinde işlevselliği ve uygulama düzenini özelleştirmek geliştiricilerin sağlar [ `Padding` ](xref:Xamarin.Forms.Layout.Padding)her sayfanın düzgün görüntülenmesi için farklı platformlarda değerleri:

    ```xaml
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms" ... >
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        ...
    </ContentPage>
    ```

  Platform tweaks hakkında daha fazla bilgi için bkz: [cihaz sınıfı](~/xamarin-forms/platform/device.md).

## <a name="testing-and-deployment"></a>Test ve dağıtım

Mac için Visual Studio ve Visual Studio test etmek ve bir uygulamayı dağıtmak için pek çok seçenek sunar. Uygulamalarında hata ayıklama, uygulama geliştirme yaşam döngüsü, ortak bir parçasıdır ve kod sorunlarını tanılamak için yardımcı olur. Daha fazla bilgi için [bir kesme noktası ayarlamak](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [adım kod aracılığıyla](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), ve [günlük penceresini çıkış bilgileri](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).

Simülatörleri, dağıtma ve bir uygulamayı test başlatmak ve uygulamaları test etmek için kullanışlı işlevler özelliği için iyi bir yerdir. Ancak, kullanıcıların uygulamaları gerçek cihazlar üzerinde erken ve sıkça test edilmesi şekilde, simülatör son uygulamada tüketecektir değil. İOS cihaz sağlama hakkında daha fazla bilgi için bkz. [cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md). Android cihaz sağlama hakkında daha fazla bilgi için bkz. [cihaz geliştirme için ayarlanmış yukarı](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="summary"></a>Özet

Bu makalede Xamarin.Forms kullanarak uygulama geliştirme temelleri incelenmiştir. Kapsanan konular bir Xamarin.Forms uygulaması, mimari ve uygulama temelleri ve kullanıcı arabirimi anatomisi dahil.

Sonraki bölümde bu kılavuzun uygulama daha gelişmiş Xamarin.Forms mimarisi ve kavramları keşfetmek için birden fazla ekran, dahil etmek için genişletilir.
