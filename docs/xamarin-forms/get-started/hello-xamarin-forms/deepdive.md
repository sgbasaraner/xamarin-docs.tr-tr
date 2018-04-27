---
title: Xamarin.Forms derin Dalış
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d97aa580-1eb9-48b3-b15b-0d7421ea7ae
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: e254aa14f5889cee6b5bee452f5275fd579eb8fc
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="xamarinforms-deep-dive"></a>Xamarin.Forms derin Dalış

İçinde [Xamarin.Forms Quickstart](~/xamarin-forms/get-started/hello-xamarin-forms/quickstart.md), Phoneword uygulama oluşturuldu. Bu makalede, ne Xamarin.Forms uygulamaların nasıl çalıştığı temellerini anlamak için oluşturulduğunu gözden geçirir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Visual Studio giriş

Visual Studio Microsoft güçlü bir IDE ' dir. Tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir derleme tarayıcı, kaynak kodu tümleştirme ve daha fazla bilgi sunar. Bu makalede, temel Visual Studio özelliklerinden bazıları Xamarin eklentisini kullanarak odaklanır.

Visual Studio düzenler koda *çözümleri* ve *projeleri*. Bir çözümü bir veya daha fazla projeleri tutan bir kapsayıcıdır. Proje, bir uygulama, bir destek kitaplığı, bir sınama uygulaması ve daha fazla olabilir. Phoneword uygulaması, aşağıdaki ekran görüntüsünde gösterildiği gibi dört projeleri içeren bir çözüm oluşur.

![](deepdive-images/vs/solution.png "Visual Studio Çözüm Gezgini")

Projeleri şunlardır:

- Phoneword – Bu, tüm paylaşılan kod ve paylaşılan UI tutan .NET standart kitaplığı proje projesidir.
- Phoneword.Android – bu proje Android özel kod tutar ve Android uygulaması için giriş noktasıdır.
- Phoneword.iOS – bu proje iOS özel kod tutar ve iOS uygulaması için giriş noktasıdır.
- Phoneword.UWP – bu proje Evrensel Windows Platformu (UWP) belirli bir kod tutar ve UWP uygulaması için giriş noktasıdır.

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms uygulaması anatomisi

Aşağıdaki ekran görüntüsünde, Visual Studio'da Phoneword .NET standart kitaplığı proje içeriğini gösterir:

![](deepdive-images/vs/net-standard-project.png "Phoneword .NET standart proje içeriği")

Proje içeren bir **bağımlılıkları** içeren düğümü **NuGet** ve **SDK** düğümleri. **NuGet** düğümü projeye eklenen Xamarin.Forms NuGet paketi içerir ve **SDK** düğüm içerir `NETStandard.Library` tamamını NuGet paketlerini başvuruyor metapackage .NET standart tanımlayın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Mac için Visual Studio giriş

Mac için Visual Studio bir ücretsiz, açık kaynaklı IDE Visual Studio'ya benzer ' dir. Tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir derleme tarayıcı, kaynak kodu tümleştirme ve daha fazla bilgi sunar. Mac için Visual Studio hakkında daha fazla bilgi için bkz: [Mac için Visual Studio Tanıtımı](/visualstudio/mac/).

Mac için Visual Studio aşağıdaki koda düzenleme Visual Studio uygulama *çözümleri* ve *projeleri*. Bir çözümü bir veya daha fazla projeleri tutan bir kapsayıcıdır. Proje, bir uygulama, bir destek kitaplığı, bir sınama uygulaması ve daha fazla olabilir. Phoneword uygulaması, aşağıdaki ekran görüntüsünde gösterildiği gibi üç projeleri içeren bir çözüm oluşur.

![](deepdive-images/xs/solution.png "Visual Studio için Mac çözüm bölmesi")

Projeleri şunlardır:

- Phoneword – Bu, tüm paylaşılan kod ve paylaşılan UI tutan taşınabilir sınıf kitaplığı (PCL) projesi projesidir.
- Phoneword.Droid – bu proje Android özel kod tutar ve Android uygulamaları için giriş noktasıdır.
- Phoneword.iOS – bu proje iOS özel kod tutar ve iOS uygulamaları için giriş noktasıdır.

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms uygulaması anatomisi

Aşağıdaki ekran görüntüsünde, Mac için Visual Studio'da Phoneword PCL proje içeriğini gösterir:

![](deepdive-images/xs/pcl-project.png "Phoneword PCL proje içeriği")

Proje üç klasörleri oluşur:

- **Başvuruları** – oluşturmak ve uygulamayı çalıştırmak için gerekli olan derlemeleri içerir. .NET taşınabilir alt klasörünü genişleterek ortaya çıkarır .NET derlemelerini başvurular gibi [sistem](http://msdn.microsoft.com/library/system%28v=vs.110%29.aspx), System.Core, ve [System.Xml](http://msdn.microsoft.com/library/system.xml%28v=vs.110%29.aspx). Genişletme **gelen paketler** klasörü Xamarin.Forms derlemelerine başvurular ortaya çıkarır.
- **Paketleri** – paketleri dizin evleri [NuGet](https://www.nuget.org) uygulamanızda üçüncü taraf kitaplıklar kullanılarak işlemini basitleştirmek paketler. Bu paketleri en son sürümleri için klasörü sağ tıklatın ve açılır menüde güncelleştirme seçeneğini seçerek güncelleştirilebilir.
- **Özellikler** – içerir **AssemblyInfo.cs**, bir .NET derlemesi meta veri dosyası. Bu dosya, uygulamanız hakkında bazı temel bilgileri doldurmak için iyi bir uygulamadır. Bu dosya hakkında daha fazla bilgi için bkz: [AssemblyInfo sınıfı](http://msdn.microsoft.com/library/microsoft.visualbasic.applicationservices.assemblyinfo(v=vs.110).aspx) konusuna bakın.

-----

Proje dosyaları sayısı da oluşur:

- **App.XAML** – için XAML biçimlendirme `App` uygulama için bir kaynak sözlüğü tanımlar sınıfı.
- **App.xaml.cs** – arka plan koduna `App` her platformda uygulama tarafından görüntülenen ilk sayfa örnek oluşturma ve uygulama yaşam döngüsü olayları işleme sorumludur sınıfı.
- **IDialer.cs** – `IDialer` belirleyen arabirimi `Dial` yöntemi tüm sınıflar tarafından sağlanmalıdır.
- **MainPage.xaml** – için XAML biçimlendirme `MainPage` tanımlar uygulaması başlatıldığında gösterilen sayfanın UI sınıfı.
- **MainPage.xaml.cs** – arka plan koduna `MainPage` sayfasıyla kullanıcı etkileşim kurduğunda çalıştırılan iş mantığı içeren sınıf.
- **Packages.config** – proje tarafından gerekli paketler ve ilgili sürümlerine izlemek için kullanılan NuGet paketleri hakkında bilgi içeren (yalnızca Mac için Visual Studio) bir XML dosyası. Mac için Visual Studio ve Visual Studio, otomatik olarak tüm eksik NuGet paketlerini kaynak kodunu diğer kullanıcılarla paylaşımını geri yüklemek üzere yapılandırılabilir. Bu dosyanın içeriğini NuGet Paket Yöneticisi tarafından denetlenir ve el ile düzenlenmemelidir.
- **PhoneTranslator.cs** – gelen çağrılan bir telefon numarasına bir telefon sözcük dönüştürmek için sorumlu olduğu iş mantığı **MainPage.xaml.cs**.

Bir Xamarin.iOS uygulaması anatomisi hakkında daha fazla bilgi için bkz: [bir Xamarin.iOS uygulaması anatomisi](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy). Bir Xamarin.Android uygulaması anatomisi hakkında daha fazla bilgi için bkz: [bir Xamarin.Android uygulaması anatomisi](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Mimari ve uygulama temelleri

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.Forms uygulaması için geleneksel bir platformlar arası uygulama aynı şekilde geliştirilmiştir. Paylaşılan kod genellikle bir .NET standart kitaplığında yerleştirilir ve platforma özgü uygulamaları paylaşılan kod kullanma. Aşağıdaki diyagramda bu ilişki Phoneword uygulama için genel bir bakış gösterilir:

![](deepdive-images/vs/architecture.png "Phoneword mimarisi")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Xamarin.Forms uygulaması için geleneksel bir platformlar arası uygulama aynı şekilde geliştirilmiştir. Paylaşılan kod genellikle bir taşınabilir sınıf kitaplığı (PCL) olarak yerleştirilir ve platforma özgü uygulamaları paylaşılan kod kullanma. Aşağıdaki diyagramda bu ilişki Phoneword uygulama için genel bir bakış gösterilir:

![](deepdive-images/xs/architecture.png "Phoneword mimarisi")

PCLs hakkında daha fazla bilgi için bkz: [taşınabilir sınıf kitaplıkları giriş](~/cross-platform/app-fundamentals/pcl.md).

-----

Başlangıç kodu kullanılmasını en üst düzeye çıkarmak için adlı tek bir sınıf Xamarin.Forms uygulamaların olması `App` sorumlu aşağıdaki kod örneğinde gösterildiği gibi her bir platformda uygulama tarafından görüntülenen ilk sayfa oluşturmak için:

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

Bu kod ayarlar `MainPage` özelliği `App` yeni bir örneğini sınıfına [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) sınıfı. Ayrıca, [ `XamlCompilation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/) özniteliği, böylece XAML doğrudan Ara dile derlenmiş XAML derleyici üzerinde kapatır. Daha fazla bilgi için bkz: [XAML derleme](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Her platformda uygulama başlatma

### <a name="ios"></a>iOS

İOS ilk Xamarin.Forms sayfasında başlatmak için Phoneword.iOS proje içeriyor `AppDelegate` devraldığı sınıfı `FormsApplicationDelegate` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

`FinishedLaunching` Geçersiz kılma çağırarak Xamarin.Forms framework başlatır `Init` yöntemi. Bu kök görünüm denetleyicisini çağrısıyla ayarlanmadan önce uygulamada yüklenecek Xamarin.Forms iOS özgü uyarlamasını neden `LoadApplication` yöntemi.

### <a name="android"></a>Android

Android ilk Xamarin.Forms sayfasında başlatmak için Phoneword.Droid proje oluşturan kod içerir bir `Activity` ile `MainLauncher` özniteliğiyle içinden devralma etkinlik `FormsApplicationActivity` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
namespace Phoneword.Droid
{
    [Activity(Label = "Phoneword",
              Icon = "@drawable/icon",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        internal static MainActivity Instance { get; private set; }

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            Instance = this;
            global::Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App());
        }
    }
}
```

`OnCreate` Geçersiz kılma çağırarak Xamarin.Forms framework başlatır `Init` yöntemi. Bu uygulamada Xamarin.Forms uygulaması yüklenmeden önce yüklenmesi Xamarin.Forms Android özgü uyarlamasını neden olur. Ayrıca, `MainActivity` sınıfı depolar kendisine bir başvuru `Instance` özelliği. `Instance` Özelliği yerel bağlamı olarak bilinir ve içinden başvuruda `PhoneDialer` sınıfı.

## <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Evrensel Windows Platformu (UWP) uygulamaları `Init` Xamarin.Forms framework başlatır yöntemi çağrılır `App` sınıfı:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Bu uygulamanın yüklenmesi Xamarin.Forms UWP özgü uyarlamasını neden olur. İlk Xamarin.Forms sayfası tarafından başlatılan `MainPage` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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
> Evrensel Windows Platformu (UWP) uygulamaları Xamarin.Forms oluşturulmuş, ancak yalnızca Visual Studio kullanarak Windows üzerinde olabilir.

## <a name="user-interface"></a>Kullanıcı Arabirimi

Kullanıcı arabirimi, bir Xamarin.Forms uygulaması oluşturmak için kullanılan dört ana denetim grubu yok.

1. **Sayfaları** – Xamarin.Forms sayfaları platformlar arası mobil uygulama ekranlar temsil eder. Phoneword uygulamanın kullandığı [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) tek bir ekranı sınıfı. Sayfaları hakkında daha fazla bilgi için bkz: [Xamarin.Forms sayfaları](~/xamarin-forms/user-interface/controls/pages.md).
1. **Düzenleri** – Xamarin.Forms düzenleri olan mantıksal yapılara görünümleri oluşturmak için kullanılan kapsayıcı. Phoneword uygulamanın kullandığı [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) yatay yığınında denetimlerini düzenlemek için sınıf. Düzenleri hakkında daha fazla bilgi için bkz: [Xamarin.Forms düzenleri](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Görünümler** – Xamarin.Forms görünümlerdir etiketleri, düğmeler ve metin girişi kutuları gibi kullanıcı arabiriminde görüntülenen kontrol eder. Phoneword uygulamanın kullandığı [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), ve [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) kontrol eder. Görünümler hakkında daha fazla bilgi için bkz: [Xamarin.Forms görünümleri](~/xamarin-forms/user-interface/controls/views.md).
1. **Hücreleri** – Xamarin.Forms hücreleri olan bir liste öğeleri için kullanılan özel öğeleri ve listedeki her öğeye nasıl çizileceğini açıklar. Uygulama kullanmayan Phoneword herhangi hücrelerinin kullanın. Hücreleri hakkında daha fazla bilgi için bkz: [Xamarin.Forms hücreleri](~/xamarin-forms/user-interface/controls/cells.md).

Çalışma zamanında her denetim ne işlenmiş olan kendi yerel eşdeğer eşleşecektir.

Herhangi bir platformda Phoneword uygulamayı çalıştırdığınızda, karşılık gelen tek bir ekran görüntüler bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Xamarin.Forms içinde. A `Page` temsil eden bir *ViewGroup* Android içinde bir *View Controller* iOS içinde veya bir *sayfa* Evrensel Windows platformu üzerinde. Phoneword uygulama aynı zamanda başlatır bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) temsil eden nesnesi `MainPage` sınıfı, XAML biçimlendirme, aşağıdaki kod örneğinde gösterilir:

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

`MainPage` Sınıfını kullanan bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) otomatik olarak ekran boyutu bağımsız ekranda denetimlerini düzenlemek için denetim. Konumlandırılmış art arda, dikey olarak eklenmiş sırada her alt öğedir. `StackLayout` Denetimi içeren bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) denetim sayfasında, metni görüntülemek için bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) metinsel kullanıcı girişi ve iki kabul etmek için Denetim [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) dokunma olayları yanıtta kod yürütmek için kullanılan denetimler.

Xamarin.Forms XAML'de hakkında daha fazla bilgi için bkz: [Xamarin.Forms XAML Temelleri](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="responding-to-user-interaction"></a>Kullanıcı etkileşimine yanıt verme

XAML içinde tanımlanan bir nesne arka plan kod dosyasına işlenen olay tetikleyebilir. Aşağıdaki örnekte gösterildiği kod `OnTranslate` arka plan koduna yönteminde `MainPage` yanıt olarak yürütülen sınıfı [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) üzerinde tetikleme olay *çevir* düğme.

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

`OnTranslate` Yöntemi phoneword karşılık gelen telefon numarasını ve yanıt çevirir, arama çubuğunda özelliklerini ayarlar. XAML sınıf için arka plan kod dosyası ile atanan adı kullanarak XAML içinde tanımlanan bir nesne erişebilirsiniz `x:Name` özniteliği. Bir harf veya alt çizgi ile başlamalı ve hiç katıştırılmış boşluklar içeremez gerekir, bu öznitelik için atanan değer C# değişkenleri olarak aynı kuralları vardır.

Çeviri düğme kablolama `OnTranslate` yöntemi için XAML biçimlendirme oluşuyor `MainPage` sınıfı:

```xaml
<Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword içinde sunulan ek kavramlar

Xamarin.Forms Phoneword uygulama bu makalede ele alınan değil birkaç kavram anlatılmıştır. Bu kavramları şunlardır:

- Etkinleştirme ve düğmelerini devre dışı bırakma. A [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) açmak veya kapatmak değiştirerek değiştirilebilir kendi [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) özelliği. Örneğin, aşağıdaki örnek devre dışı bırakır kod `callButton`:

    ```csharp
    callButton.IsEnabled = false;
    ```

- Uyarı iletişim kutusu görüntüleniyor. Kullanıcı aramayı bastığında **düğmesini** Phoneword uygulama gösterir bir *uyarı iletişim* yerleştirin ya da bir aramayı iptal et seçeneğine sahip. [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) Yöntemi oluştur iletişim kutusu için kullanılan aşağıdaki kod örneğinde gösterildiği gibi:

    ```csharp
    await this.DisplayAlert (
            "Dial a Number",
            "Would you like to call " + translatedNumber + "?",
            "Yes",
            "No");
    ```

- Aracılığıyla yerel özelliklerine erişme [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) sınıfı. Phoneword uygulamanın kullandığı `DependencyService` çözümlemek için sınıf `IDialer` Phoneword projeden aşağıdaki kod örneğinde gösterildiği gibi arabirimi platforma özgü telefon arama uygulamaları için:

    ```csharp
    async void OnCall (object sender, EventArgs e)
    {
        ...
        var dialer = DependencyService.Get<IDialer> ();
        ...
    }
    ```

  Hakkında daha fazla bilgi için [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) sınıfı için bkz: [erişme yerel özellikleri DependencyService aracılığıyla](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

- Bir URL içeren bir telefon araması yapılıyor. Phoneword uygulamanın kullandığı `OpenURL` sistem phone uygulamasını başlatmak için. URL oluşan bir `tel:` önekini ve ardından çağrılacak, telefon numarasına göre iOS projeden aşağıdaki kod örneğinde gösterildiği gibi:

    ```csharp
    return UIApplication.SharedApplication.OpenUrl (new NSUrl ("tel:" + number));
    ```

- Platform düzeni uyguladıkça. [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Sınıfı kullanır aşağıdaki kod örneğinde farklı gösterildiği gibi bir platform başına temelinde işlevselliği ve uygulama düzenini özelleştirmek geliştiricilere sağlar [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)doğru her sayfasını görüntülemek için farklı platformlarda değerler:

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

  Platform tweaks hakkında daha fazla bilgi için bkz: [aygıt sınıfı](~/xamarin-forms/platform/device.md).

## <a name="testing-and-deployment"></a>Test ve dağıtım

Mac için Visual Studio ve Visual Studio Test ve bir uygulamayı dağıtmak için birçok seçenek sağlar. Uygulamalarında hata ayıklama, uygulama geliştirme yaşam döngüsü, ortak bir parçasıdır ve kod sorunları tanılamak için yardımcı olur. Daha fazla bilgi için bkz: [bir kesme noktası belirleyerek](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [adım aracılığıyla kodu](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), ve [Günlük penceresine çıktı bilgilerini](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).

Benzeticileri dağıtma ve uygulama testi başlatmak ve uygulamaları test etmek için kullanışlı işlevsellik özelliği için iyi bir yerdir. Ancak, kullanıcılar erken ve genellikle, gerçek cihazlarda uygulamaları test edilmiş şekilde bir simulator son uygulamada tüketecektir değil. İOS aygıtı sağlama hakkında daha fazla bilgi için bkz: [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md). Android cihaz sağlama hakkında daha fazla bilgi için bkz: [ayarlamak yukarı geliştirme için cihazı](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="summary"></a>Özet

Bu makalede Xamarin.Forms kullanarak uygulama geliştirme ile ilgili temel bilgileri incelendi. Kapsanan konular, bir Xamarin.Forms uygulaması, mimari ve uygulama temelleri ve kullanıcı arabirimi anatomisi dahil.

Sonraki bölümde bu kılavuzun uygulama daha gelişmiş Xamarin.Forms mimarisi ve kavramları keşfetmek için birden çok ekran içerecek şekilde genişletilir.
