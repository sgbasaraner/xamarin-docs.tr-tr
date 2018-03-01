---
title: "Xamarin.Forms hızlı başlangıç"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 7ce674ea38bc847bc9064a5a61113900a45b991d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms hızlı başlangıç

Bu kılavuz, bir sayısal telefon numarası kullanıcı tarafından girilen bir alfasayısal telefon numarasını çeviren ve numarası çağıran bir uygulama oluşturmak gösterilmiştir. Son uygulama aşağıda gösterilmiştir:

[![](quickstart-images/intro-app-examples-sml.png "Phoneword uygulama")](quickstart-images/intro-app-examples.png "Phoneword uygulama")

Phoneword uygulama gibi oluşturun:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. İçinde **Başlat** ekranında, Visual Studio'yu başlatın. Bu başlangıç sayfasını açar:

  ![](quickstart-images/vs/start-page.png "Visual Studio")

1. Visual Studio'da sırasıyla **yeni proje oluştur...**  yeni bir proje oluşturmak için:

  ![](quickstart-images/vs/new-solution.png "New Project")

1. İçinde **yeni proje** iletişim kutusunda, tıklatın **platformlar arası**seçin **Çapraz Platform uygulama (Xamarin.Forms)** şablon kümesi adı ve çözüm adı `Phoneword`, Proje için uygun bir konum seçin ve tıklatın **Tamam** düğmesi:

  ![](quickstart-images/vs/new-project.png "Platformlar arası proje şablonları")

1. İçinde **yeni Çapraz Platform uygulaması** iletişim kutusunda, tıklatın **boş uygulama**seçin **Xamarin.Forms** UI teknoloji olarak seçin **.NET standart** olarak Kod paylaşımı stratejisi ve tıklatın **Tamam** düğmesi:

  ![](quickstart-images/vs/new-app.png "Yeni platformlar arası uygulaması")

1. İçinde **Çözüm Gezgini**, **Phoneword** projesi, çift **MainPage.xaml** açmak için:

  ![](quickstart-images/vs/open-mainpage-xaml.png "MainPage.xaml açın")

1. İçinde **MainPage.xaml**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod sayfası için kullanıcı arabirimi bildirimli olarak tanımlar:

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.MainPage">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <Label Text="Enter a Phoneword:" />
              <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
              <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
              <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
            </StackLayout>
        </ContentPage>

  Değişiklikleri kaydetmek **MainPage.xaml** basarak **CTRL + S**ve dosyayı kapatın.

1. İçinde **Çözüm Gezgini**, genişletin **MainPage.xaml** çift tıklayın ve **MainPage.xaml.cs** açmak için:

  ![](quickstart-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs açın")

1. İçinde **MainPage.xaml.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. `OnTranslate` Ve `OnCall` yöntemleri, yanıt olarak yürütülür **çevir** ve **çağrısı** kullanıcı arabiriminde, sırasıyla tıklattınız düğmeleri:

        using System;
        using Xamarin.Forms;

        namespace Phoneword
        {
            public partial class MainPage : ContentPage
            {
                string translatedNumber;

                public MainPage ()
                {
                    InitializeComponent ();
                }

                void OnTranslate (object sender, EventArgs e)
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

                async void OnCall (object sender, EventArgs e)
                {
                    if (await this.DisplayAlert (
                            "Dial a Number",
                            "Would you like to call " + translatedNumber + "?",
                            "Yes",
                            "No")) {
                        var dialer = DependencyService.Get<IDialer> ();
                        if (dialer != null)
                            dialer.Dial (translatedNumber);
                    }
                }
            }
        }

  > [!NOTE]
> **Not**: Bu noktada uygulamayı oluşturmaya çalışırken, daha sonra düzeltilecektir hatalara neden olur.

  Değişiklikleri kaydetmek **MainPage.xaml.cs** basarak **CTRL + S**ve dosyayı kapatın.

1. İçinde **Çözüm Gezgini**, genişletin **App.xaml** çift tıklayın ve **App.xaml.cs** açmak için:

  ![](quickstart-images/vs/open-app-class.png "App.xaml.cs açın")

1. İçinde **App.xaml.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. `App` Oluşturucusu kümeleri `MainPage` sınıfı uygulama başladığında görüntülenir sayfa olarak:

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

                protected override void OnStart()
                {
                    // Handle when your app starts
                }

                protected override void OnSleep()
                {
                    // Handle when your app sleeps
                }

                protected override void OnResume()
                {
                    // Handle when your app resumes
                }
            }
        }

  Değişiklikleri kaydetmek **App.xaml.cs** basarak **CTRL + S**ve dosyayı kapatın.

1. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword** proje ve seçin **Ekle > Yeni öğe...** :

  ![](quickstart-images/vs/add-new-item.png "Yeni Öğe Ekle")

1. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > kod > sınıfı**, yeni dosya adı **PhoneTranslator**, tıklatıp **Ekle** düğmesi :

  ![](quickstart-images/vs/add-translator-class.png "Yeni sınıf ekleyin")

1. İçinde **PhoneTranslator.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod, bir telefon numarasına bir telefon sözcük çevirir:

        using System.Text;

        namespace Core
        {
            public static class PhonewordTranslator
            {
                public static string ToNumber(string raw)
                {
                    if (string.IsNullOrWhiteSpace(raw))
                        return null;

                    raw = raw.ToUpperInvariant();

                    var newNumber = new StringBuilder();
                    foreach (var c in raw)
                    {
                        if (" -0123456789".Contains(c))
                            newNumber.Append(c);
                        else
                        {
                            var result = TranslateToNumber(c);
                            if (result != null)
                                newNumber.Append(result);
                            // Bad character?
                            else
                                return null;
                        }
                    }
                    return newNumber.ToString();
                }

                static bool Contains(this string keyString, char c)
                {
                    return keyString.IndexOf(c) >= 0;
                }

                static readonly string[] digits = {
                    "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
                };

                static int? TranslateToNumber(char c)
                {
                    for (int i = 0; i < digits.Length; i++)
                    {
                        if (digits[i].Contains(c))
                            return 2 + i;
                    }
                    return null;
                }
            }
        }

  Değişiklikleri kaydetmek **PhoneTranslator.cs** basarak **CTRL + S**ve dosyayı kapatın.

1. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword** proje ve seçin **Ekle > Yeni öğe...** :

  ![](quickstart-images/vs/add-new-item.png "Yeni Öğe Ekle")

1. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > kod > arabirimi**, yeni dosya adı **IDialer**, tıklatıp **Ekle** düğmesi:

  ![](quickstart-images/vs/add-idialer-interface.png "Yeni arabirimi Ekle")

1. İçinde **IDialer.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kodu tanımlayan bir `Dial` çevrilmiş telefon numarasını aramak için her platformda uygulanmalı yöntemi:

        namespace Phoneword
        {
            public interface IDialer
            {
                bool Dial(string number);
            }
        }

  Değişiklikleri kaydetmek **IDialer.cs** basarak **CTRL + S**ve dosyayı kapatın.

  > [!NOTE]
> Uygulama için ortak kodun tamamlanmıştır. Platforma özgü Telefon Çeviricisi kodu şimdi olarak gerçekleştirilen bir [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

1. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.iOS** proje ve seçin **Ekle > Yeni öğe...** :

  ![](quickstart-images/vs/add-new-item-ios.png "Yeni Öğe Ekle")

1. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Apple > kod > sınıfı**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **Ekle** düğmesi:

  ![](quickstart-images/vs/new-phone-dialer-ios.png "Yeni sınıf ekleyin")

1. İçinde **PhoneDialer.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod oluşturur <code>Dial</code> iOS platformunda çevrilmiş telefon numarasını aramak için kullanılan yöntem:

        using Foundation;
        using Phoneword.iOS;
        using UIKit;
        using Xamarin.Forms;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.iOS
        {
            public class PhoneDialer : IDialer
            {
                public bool Dial(string number)
                {
                    return UIApplication.SharedApplication.OpenUrl (
                        new NSUrl ("tel:" + number));
                }
            }
        }

  Değişiklikleri kaydetmek **PhoneDialer.cs** basarak **CTRL + S**ve dosyayı kapatın.

1. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.Android** proje ve seçin **Ekle > Yeni öğe...** :

  ![](quickstart-images/vs/add-new-item-android.png "Yeni Öğe Ekle")

1. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > Android > sınıfı**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **Ekle** düğmesi:

  ![](quickstart-images/vs/new-phone-dialer-android.png "Yeni sınıf ekleyin")

1. İçinde **PhoneDialer.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod oluşturur `Dial` Android platformunda çevrilmiş telefon numarasını aramak için kullanılan yöntem:

        using Android.Content;
        using Android.Telephony;
        using Phoneword.Droid;
        using System.Linq;
        using Xamarin.Forms;
        using Uri = Android.Net.Uri;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.Droid
        {
            public class PhoneDialer : IDialer
            {
                public bool Dial(string number)
                {
                    var context = MainActivity.Instance;
                    if (context == null)
                        return false;

                    var intent = new Intent (Intent.ActionCall);
                    intent.SetData (Uri.Parse ("tel:" + number));

                    if (IsIntentAvailable (context, intent)) {
                        context.StartActivity (intent);
                        return true;
                    }

                    return false;
                }

                public static bool IsIntentAvailable(Context context, Intent intent)
                {
                    var packageManager = context.PackageManager;

                    var list = packageManager.QueryIntentServices (intent, 0)
                        .Union (packageManager.QueryIntentActivities (intent, 0));

                    if (list.Any ())
                        return true;

                    var manager = TelephonyManager.FromContext (context);
                    return manager.PhoneType != PhoneType.None;
                }
            }
        }

  Değişiklikleri kaydetmek **PhoneDialer.cs** basarak **CTRL + S**ve dosyayı kapatın.

1. İçinde **Çözüm Gezgini**, **Phoneword.Android** projesi, çift **MainActivity.cs** açmak için tüm şablonu kodu kaldırın ve ile değiştirin Aşağıdaki kod:

        using Android.App;
        using Android.Content.PM;
        using Android.OS;

        namespace Phoneword.Droid
        {
            [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

  Değişiklikleri kaydetmek **MainActivity.cs** basarak **CTRL + S**ve dosyayı kapatın.

1. İçinde **Çözüm Gezgini**, **Phoneword.Android** projesi, çift **özellikleri** seçip **Android derleme bildirimi** sekmesi:

  ![](quickstart-images/vs/android-manifest.png "Açık Android derleme bildirimi")

1. İçinde **gerekli izinleri** bölümünde, etkinleştirme **CALL_PHONE** izni. Bu, bir telefon araması yerleştirmek için uygulama izin verir:

  ![](quickstart-images/vs/android-manifest-changed.png "CallPhone izni etkinleştir")

  Tuşlarına basarak bildirime değişiklikleri kaydetmek **CTRL + S**ve dosyayı kapatın.

1. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.UWP** proje ve seçin **Ekle > Yeni öğe...** :

  ![](quickstart-images/vs/add-new-item-uwp.png "Yeni Öğe Ekle")

1. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > kod > sınıfı**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **Ekle** düğmesi:

  ![](quickstart-images/vs/new-phone-dialer-uwp.png "Yeni sınıf ekleyin")

1. İçinde **PhoneDialer.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod oluşturur `Dial` yöntemi ve evrensel Windows platformu üzerinde bir çevrilmiş telefon numarasını aramak için kullanılan yardımcı yöntemler:

        using Phoneword.UWP;
        using System;
        using System.Threading.Tasks;
        using Windows.ApplicationModel.Calls;
        using Windows.UI.Popups;
        using Xamarin.Forms;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.UWP
        {
            public class PhoneDialer : IDialer
            {
                bool dialled = false;

                public bool Dial(string number)
                {
                    DialNumber(number);
                    return dialled;
                }

                async Task DialNumber(string number)
                {
                    var phoneLine = await GetDefaultPhoneLineAsync();
                    if (phoneLine != null)
                    {
                        phoneLine.Dial(number, number);
                        dialled = true;
                    }
                    else
                    {
                        var dialog = new MessageDialog("No line found to place the call");
                        await dialog.ShowAsync();
                        dialled = false;
                    }
                }

                async Task<PhoneLine> GetDefaultPhoneLineAsync()
                {
                    var phoneCallStore = await PhoneCallManager.RequestStoreAsync();
                    var lineId = await phoneCallStore.GetDefaultLineAsync();
                    return await PhoneLine.FromIdAsync(lineId);
                }
            }
        }

  Değişiklikleri kaydetmek **PhoneDialer.cs** basarak **CTRL + S**ve dosyayı kapatın.

1. İçinde **Çözüm Gezgini**, **Phoneword.UWP** projesinde **başvuruları**seçip **Başvuru Ekle...** :

  ![](quickstart-images/vs/uwp-add-reference.png "Başvuru ekleme")

1. İçinde **başvuru Yöneticisi** iletişim kutusunda **Evrensel Windows > uzantılar > UWP için Windows Mobile uzantıları**, tıklatıp **Tamam** düğmesi:

  ![](quickstart-images/vs/uwp-add-reference-extensions.png "UWP için Windows Mobile uzantıları ekleme")

1. İçinde **Çözüm Gezgini**, **Phoneword.UWP** projesi, çift **Package.appxmanifest**:

  ![](quickstart-images/vs/uwp-manifest.png "UWP bildirimini açın")

1. İçinde **yetenekleri** sayfasında, etkinleştirme **telefon araması** yeteneği. Bu, bir telefon araması yerleştirmek için uygulama izin verir:

  ![](quickstart-images/vs/uwp-manifest-changed.png "Telefon araması özelliği etkinleştir")

  Tuşlarına basarak bildirime değişiklikleri kaydetmek **CTRL + S**ve dosyayı kapatın.

1. Visual Studio'da seçin **Yapı > Yapı çözümü** menü öğesi (veya basın **CTRL + SHIFT + B**). Uygulamayı oluşturacaksınız ve Visual Studio durum çubuğunda bir başarı iletisi görüntülenir:

  ![](quickstart-images/vs/build-successful.png "Yapı başarılı")

  Hatalar varsa, önceki adımı yineleyin ve uygulama başarıyla derlemeler kadar hataları düzeltin.

1. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.UWP** proje ve seçin **başlangıç projesi olarak ayarla**:

  ![](quickstart-images/vs/uwp-set-as-startup-project.png "Başlangıç projesi olarak ayarla")

1. Visual Studio araç çubuğunda tuşlarına basarak **Başlat** düğmesini (Oynat düğmesini benzer üçgen düğmesi) uygulamayı başlatmak için:

  ![](quickstart-images/vs/start.png "Visual Studio araç")
  ![](quickstart-images/vs/phone-result-uwp.png "Phoneword uygulama UWP")

1. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.Android** proje ve seçin **başlangıç projesi olarak ayarla**.
1. Visual Studio araç çubuğunda tuşlarına basarak **Başlat** düğmesini (Oynat düğmesini benzer üçgen düğmesi) Android öykünücüsünde içinde uygulamayı başlatmak için.
1. Bir iOS aygıtı varsa ve Xamarin.Forms geliştirme için Mac sistem gereksinimlerini karşılayan, iOS cihazını uygulama dağıtmak için benzer bir teknik kullanın. Alternatif olarak, uygulamayı dağıtmak [iOS uzak simulator](~/tools/ios-simulator.md).

  Not: telefon aramaları tüm benzeticileri desteklenmez.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Visual Studio Başlangıç sayfası ve Mac için Başlat'ı tıklatın **yeni proje...**  yeni bir proje oluşturmak için:

  ![](quickstart-images/xs/new-solution.png "Yeni çözümü")

1. İçinde **yeni projeniz için bir şablon seçin** iletişim kutusunda, tıklatın **Multiplatform > Uygulama**seçin **boş Forms uygulaması** şablonu ve tıklatın **İleri** düğmesi:

  ![](quickstart-images/xs/choose-template.png "Bir şablon seçin")

1. İçinde **boş formları uygulamanızı yapılandırma** iletişim kutusunda, yeni uygulama adı `Phoneword`, emin **kullanım taşınabilir sınıf kitaplığı** radyo düğmesi seçildiğinde, emin olun **kullanım XAML kullanıcı için Arabirim dosyaları** onay kutusu seçilidir ve tıklatın **sonraki** düğmesi:

  ![](quickstart-images/xs/configure-app.png "Forms uygulamayı yapılandırma")

1. İçinde **yeni boş formları uygulamanızı yapılandırma** iletişim, çözüm ve proje adları kümesine bırakın `Phoneword`, proje için uygun bir konum seçin ve'ı tıklatın **oluşturma** düğmesi oluşturmak için Proje:

  ![](quickstart-images/xs/configure-project.png "Forms projesi yapılandırın")

1. İçinde **çözüm paneli**seçin **Phoneword** proje, sağ tıklatın ve seçin **Ekle > yeni dosya...** :

  ![](quickstart-images/xs/add-new-file.png "Yeni dosya ekleme")

1. İçinde **yeni dosya** iletişim kutusunda **Forms > Forms ContentPage Xaml**, yeni dosya adı **MainPage**, tıklatıp **yeni** düğmesi. Bu adlı bir sayfaya ekler **MainPage** projeye:

  ![](quickstart-images/xs/add-mainpage-class.png "Yeni ContentPage ekleme")

1. İçinde **çözüm paneli**, çift **MainPage.xaml** açmak için:

  ![](quickstart-images/xs/open-mainpage-xaml.png "MainPage.xaml açın")

1. İçinde **MainPage.xaml**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod sayfası için kullanıcı arabirimi bildirimli olarak tanımlar:

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.MainPage">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <Label Text="Enter a Phoneword:" />
              <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
              <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
              <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
            </StackLayout>
        </ContentPage>

  Değişiklikleri kaydetmek **MainPage.xaml** seçerek **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**) ve dosyayı kapatın.

1. İçinde **çözüm paneli**, çift **MainPage.xaml.cs** açmak için:

  ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs açın")

1. İçinde **MainPage.xaml.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. `OnTranslate` Ve `OnCall` yöntemleri, yanıt olarak yürütülür **çevir** ve **çağrısı** kullanıcı arabiriminde sırasıyla tıklattınız düğmeleri:

        using System;
        using Xamarin.Forms;

        namespace Phoneword
        {
            public partial class MainPage : ContentPage
            {
                string translatedNumber;

                public MainPage ()
                {
                    InitializeComponent ();
                }

                void OnTranslate (object sender, EventArgs e)
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

                async void OnCall (object sender, EventArgs e)
                {
                    if (await this.DisplayAlert (
                            "Dial a Number",
                            "Would you like to call " + translatedNumber + "?",
                            "Yes",
                            "No")) {
                        var dialer = DependencyService.Get<IDialer> ();
                        if (dialer != null)
                            dialer.Dial (translatedNumber);
                    }
                }
            }
        }

  > [!NOTE]
> **Not**: Bu noktada uygulamayı oluşturmaya çalışırken, daha sonra düzeltilecektir hatalara neden olur.

  Değişiklikleri kaydetmek **MainPage.xaml.cs** seçerek **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**) ve dosyayı kapatın.

1. İçinde **çözüm paneli**, çift **App.xaml.cs** açmak için:

  ![](quickstart-images/xs/open-app-class.png "App.xaml.cs açın")

1. İçinde **App.xaml.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. `App` Oluşturucusu kümeleri `MainPage` sınıfı uygulama başladığında görüntülenir sayfa olarak:

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

                protected override void OnStart()
                {
                    // Handle when your app starts
                }

                protected override void OnSleep()
                {
                    // Handle when your app sleeps
                }

                protected override void OnResume()
                {
                    // Handle when your app resumes
                }
            }
        }

  Değişiklikleri kaydetmek **Phoneword.cs** seçerek **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**) ve dosyayı kapatın.

1. İçinde **çözüm paneli**seçin **Phoneword** proje, sağ tıklatın ve seçin **Ekle > yeni dosya...** :

  ![](quickstart-images/xs/add-new-translator-file.png "Yeni dosya ekleme")

1. İçinde **yeni dosya** iletişim kutusunda **genel > boş sınıfı**, yeni dosya adı **PhoneTranslator**, tıklatıp **yeni** düğmesi:

  ![](quickstart-images/xs/add-translator-class.png "Yeni sınıf ekleyin")

1. İçinde **PhoneTranslator.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod, bir telefon numarasına bir telefon sözcük çevirir:

        using System.Text;

        namespace Core
        {
            public static class PhonewordTranslator
            {
                public static string ToNumber(string raw)
                {
                    if (string.IsNullOrWhiteSpace(raw))
                        return null;

                    raw = raw.ToUpperInvariant();

                    var newNumber = new StringBuilder();
                    foreach (var c in raw)
                    {
                        if (" -0123456789".Contains(c))
                            newNumber.Append(c);
                        else
                        {
                            var result = TranslateToNumber(c);
                            if (result != null)
                                newNumber.Append(result);
                            // Bad character?
                            else
                                return null;
                        }
                    }
                    return newNumber.ToString();
                }

                static bool Contains(this string keyString, char c)
                {
                    return keyString.IndexOf(c) >= 0;
                }

                static readonly string[] digits = {
                    "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
                };

                static int? TranslateToNumber(char c)
                {
                    for (int i = 0; i < digits.Length; i++)
                    {
                        if (digits[i].Contains(c))
                            return 2 + i;
                    }
                    return null;
                }
            }
        }

  Değişiklikleri kaydetmek **PhoneTranslator.cs** seçerek **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**) ve dosyayı kapatın.

1. İçinde **çözüm paneli**seçin **Phoneword** proje, sağ tıklatın ve seçin **Ekle > yeni dosya...** :

  ![](quickstart-images/xs/add-new-interface.png "Yeni dosya ekleme")

1. İçinde **yeni dosya** iletişim kutusunda **genel > boş arabirimi**, yeni dosya adı **IDialer**, tıklatıp **yeni** düğmesi:

  ![](quickstart-images/xs/add-idialer-interface.png "Yeni arabirimi Ekle")

1. İçinde **IDialer.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kodu tanımlayan bir `Dial` çevrilmiş telefon numarasını aramak için her platformda uygulanmalı yöntemi:

        namespace Phoneword
        {
            public interface IDialer
            {
                bool Dial(string number);
            }
        }

  Değişiklikleri kaydetmek **IDialer.cs** seçerek **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**) ve dosyayı kapatın.

  > [!NOTE]
> Uygulama için ortak kodun tamamlanmıştır. Platforma özgü Telefon Çeviricisi kodu şimdi olarak gerçekleştirilen bir [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

1. İçinde **çözüm paneli**seçin **Phoneword.iOS** proje, sağ tıklatın ve seçin **Ekle > yeni dosya...** :

  ![](quickstart-images/xs/add-new-file-ios.png "Yeni dosya ekleme")

1. İçinde **yeni dosya** iletişim kutusunda **genel > boş sınıfı**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **yeni** düğmesi:

  ![](quickstart-images/xs/new-phonedialer-ios.png "Yeni sınıf ekleyin")

1. İçinde **PhoneDialer.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod oluşturur `Dial` iOS platformunda çevrilmiş telefon numarasını aramak için kullanılan yöntem:

        using Foundation;
        using Phoneword.iOS;
        using UIKit;
        using Xamarin.Forms;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.iOS
        {
            public class PhoneDialer : IDialer
            {
                public bool Dial(string number)
                {
                    return UIApplication.SharedApplication.OpenUrl (
                        new NSUrl ("tel:" + number));
                }
            }
        }

  Değişiklikleri kaydetmek **PhoneDialer.cs** seçerek **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**) ve dosyayı kapatın.

1. İçinde **çözüm paneli**seçin **Phoneword.Droid** proje, sağ tıklatın ve seçin **Ekle > yeni dosya...** :

  ![](quickstart-images/xs/add-new-file-android.png "Yeni dosya ekleme")

1. İçinde **yeni dosya** iletişim kutusunda **genel > boş sınıfı**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **yeni** düğmesi:

  ![](quickstart-images/xs/new-phonedialer-android.png "Yeni sınıf ekleyin")

1. İçinde **PhoneDialer.cs**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod oluşturur `Dial` Android platformunda çevrilmiş telefon numarasını aramak için kullanılan yöntem:

        using Android.Content;
        using Android.Telephony;
        using Phoneword.Droid;
        using System.Linq;
        using Xamarin.Forms;
        using Uri = Android.Net.Uri;

        [assembly: Dependency(typeof(PhoneDialer))]
        namespace Phoneword.Droid
        {
            public class PhoneDialer : IDialer
            {
                public bool Dial(string number)
                {
                    var context = MainActivity.Instance;
                    if (context == null)
                        return false;

                    var intent = new Intent (Intent.ActionCall);
                    intent.SetData (Uri.Parse ("tel:" + number));

                    if (IsIntentAvailable (context, intent)) {
                        context.StartActivity (intent);
                        return true;
                    }

                    return false;
                }

                public static bool IsIntentAvailable(Context context, Intent intent)
                {
                    var packageManager = context.PackageManager;

                    var list = packageManager.QueryIntentServices (intent, 0)
                        .Union (packageManager.QueryIntentActivities (intent, 0));

                    if (list.Any ())
                        return true;

                    var manager = TelephonyManager.FromContext (context);
                    return manager.PhoneType != PhoneType.None;
                }
            }
        }

  Değişiklikleri kaydetmek **PhoneDialer.cs** seçerek **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**) ve dosyayı kapatın.

1. İçinde **çözüm paneli**, **Phoneword.Droid** projesi, çift **MainActivity.cs** açmak için tüm şablonu kodu kaldırıp şununla değiştirin Kod:

        using Android.App;
        using Android.Content.PM;
        using Android.OS;

        namespace Phoneword.Droid
        {
            [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

  Değişiklikleri kaydetmek **MainActivity.cs** seçerek **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**) ve dosyayı kapatın.

1. İçinde **çözüm paneli**, genişletin **özellikleri** klasörü ve çift **AndroidManifest.xml** dosyası:

  ![](quickstart-images/xs/android-manifest.png "Açık Android derleme bildirimi")

1. İçinde **gerekli izinleri** bölümünde, etkinleştirme **CallPhone** izni. Bu, bir telefon araması yerleştirmek için uygulama izin verir:

  ![](quickstart-images/xs/android-manifest-changed.png "CallPhone izni etkinleştir")

  Değişiklikleri kaydetmek **AndroidManifest.xml** seçerek **Dosya > Kaydet** (veya tuşlarına basarak **&#8984; + S**) ve dosyayı kapatın.

1. İçinde **çözüm paneli**, kaldırma **PhonewordPage** sınıfıyla **Phoneword** projesi. Proje oluşturuldu ve artık gerekli değildir, bu sayfayı otomatik olarak eklendi.
1. Mac için Visual Studio'da seçin **Yapı > Yapı tüm** menü öğesi (veya basın **&#8984; + B**). Uygulamayı oluşturacaksınız ve Visual Studio Mac araç için bir başarı iletisi görünür.

  ![](quickstart-images/xs/build-successful.png "Yapı başarılı")

1. Hatalar varsa, önceki adımı yineleyin ve uygulama başarıyla derlemeler kadar hataları düzeltin.
1. Mac araç için Visual Studio basın **Başlat** düğmesini (Oynat düğmesini benzer üçgen düğmesi) iOS simülatörü'nü içinde uygulamayı başlatmak için:

  ![](quickstart-images/xs/start.png "Visual Studio için Mac araç")
  ![](quickstart-images/xs/phoneword-result-ios.png "iOS simülatörü")

  Not: telefon aramaları iOS Simulator'da desteklenmez.

1. İçinde **çözüm paneli**seçin **Phoneword.Droid** proje, sağ tıklatın ve seçin **başlangıç projesi olarak ayarla**:

  ![](quickstart-images/xs/set-startup-project.png "Başlangıç projesi olarak ayarla")

1. Mac araç için Visual Studio basın **Başlat** düğmesini (Oynat düğmesini benzer üçgen düğmesi) Android öykünücüsünde içinde uygulamayı başlatmak için:

  ![](quickstart-images/xs/phoneword-result-android.png "Android öykünücüsü")

  Not: telefon aramaları Android öykünücüsünü içinde desteklenmez.

-----

Xamarin.Forms uygulaması Tamamlanıyor tebrikler. [Sonraki konuyu](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) bu kılavuzda, bu kılavuzda Xamarin.Forms kullanarak uygulama geliştirme ile ilgili temel bilgileri anlamak için gerçekleştirilen adımları gözden geçirir.


## <a name="related-links"></a>İlgili bağlantılar

- [DependencyService aracılığıyla yerel özelliklerine erişme](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
