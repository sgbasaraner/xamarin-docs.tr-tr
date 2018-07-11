---
title: Xamarin.Forms hızlı başlangıç
description: Bu makalede, kullanıcı tarafından girilen bir sayısal telefon numarası bir alfasayısal telefon numarası çeviren ve sayı çağrılarının bir uygulamanın nasıl oluşturulacağını açıklar.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 5b5f8c80e49d66ed3bd8b008c975d1cfeda93ed4
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38832390"
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms hızlı başlangıç

Bu izlenecek yol, kullanıcı tarafından girilen bir sayısal telefon numarası bir alfasayısal telefon numarası çeviren ve sayı çağrılarının bir uygulamanın nasıl oluşturulacağını gösterir. Son uygulama aşağıda gösterilmiştir:

[![](quickstart-images/intro-app-examples-sml.png "Phoneword uygulama")](quickstart-images/intro-app-examples.png#lightbox "Phoneword uygulama")

Phoneword uygulamayı şu şekilde oluşturun:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. İçinde **Başlat** ekranında, Visual Studio'yu başlatın. Bu başlangıç sayfasını açar:

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. Visual Studio'da **yeni proje oluştur...**  yeni bir proje oluşturmak için:

    ![](quickstart-images/vs/new-solution.png "Yeni Proje")

3. İçinde **yeni proje** iletişim kutusunda, tıklayın **platformlar arası**seçin **mobil uygulama (Xamarin.Forms)** şablon kümesi adı **Phoneword**, proje için uygun bir konum seçin ve tıklayın **Tamam** düğmesi:

    ![](quickstart-images/vs/new-project.w157.png "Platformlar arası proje şablonları")

    > [!NOTE]
    > Ad çözüm başarısız **Phoneword** çok sayıda derleme hatalarına neden.

4. İçinde **yeni Çapraz Platform uygulaması** iletişim kutusunda, tıklayın **boş uygulama**seçin **.NET Standard** tıklatın ve kod paylaşımı stratejisi olarak **Tamam** Düğme:

    ![](quickstart-images/vs/new-app.png "Yeni bir Çoklu Platform uygulaması")

5. İçinde **Çözüm Gezgini**, **Phoneword** proje, çift **MainPage.xaml** açmak için:

    ![](quickstart-images/vs/open-mainpage-xaml.png "Mainpage.XAML dosyasını açın")

6. İçinde **MainPage.xaml**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod sayfası için kullanıcı arabirimi bildirimli olarak tanımlar:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Değişiklikleri kaydetmek **MainPage.xaml** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

7. İçinde **Çözüm Gezgini**, genişletme **MainPage.xaml** ve çift **MainPage.xaml.cs** açmak için:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs açın")

8. İçinde **MainPage.xaml.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. `OnTranslate` Ve `OnCall` yöntemleri, yanıt olarak yürütülecek **çevir** ve **çağrı** düğmeler kullanıcı arabiriminde sırasıyla tıklanan:

    ```csharp
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
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
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
    ```

    > [!NOTE]
    > Bu noktada uygulamayı oluşturmaya çalışırken, daha sonra düzeltilecek hatalara neden olabilecek.

    Değişiklikleri kaydetmek **MainPage.xaml.cs** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

9. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword** seçin ve proje **Ekle > Yeni öğe...** :

    ![](quickstart-images/vs/add-new-item.png "Yeni Öğe Ekle")

10. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > kod > sınıfı**, yeni dosya adı **PhoneTranslator**, tıklatıp **Ekle** düğmesi :

    ![](quickstart-images/vs/add-translator-class.w157.png "Yeni sınıf ekleyin")

11. İçinde **PhoneTranslator.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod, bir telefon numarasına bir telefon sözcük çevirir:

    ```csharp
    using System.Text;

    namespace Phoneword
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
    ```

    Değişiklikleri kaydetmek **PhoneTranslator.cs** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

12. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword** seçin ve proje **Ekle > Yeni öğe...** :

    ![](quickstart-images/vs/add-new-item.png "Yeni Öğe Ekle")

13. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > kod > arabirimi**, yeni dosya adı **IDialer**, tıklatıp **Ekle** düğmesi:

    ![](quickstart-images/vs/add-idialer-interface.w157.png "Yeni arabirim ekleme")

14. İçinde **IDialer.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kodu tanımlayan bir `Dial` her platformda çevrilmiş telefon numarası çevirmek için uygulanması gereken yöntemini:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```

    Değişiklikleri kaydetmek **IDialer.cs** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

    > [!NOTE]
    > Uygulama için ortak kodun tamamlanmıştır. Platforma özgü telefon çevirici kodu artık uygulanmasını olarak bir [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

15. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.iOS** seçin ve proje **Ekle > Yeni öğe...** :

    ![](quickstart-images/vs/add-new-item-ios.png "Yeni Öğe Ekle")

16. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Apple > kod > sınıfı**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **Ekle** düğmesi:

    ![](quickstart-images/vs/new-phone-dialer-ios.w157.png "Yeni sınıf ekleyin")

17. İçinde **PhoneDialer.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod oluşturur <code>Dial</code> iOS platformunda çevrilmiş telefon numarası çevirmek için kullanılacak yöntemi:

    ```csharp
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
    ```

    Değişiklikleri kaydetmek **PhoneDialer.cs** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

18. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.Android** seçin ve proje **Ekle > Yeni öğe...** :

    ![](quickstart-images/vs/add-new-item-android.png "Yeni Öğe Ekle")

19. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > Android > sınıfı**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **Ekle** düğmesi:

    ![](quickstart-images/vs/new-phone-dialer-android.w157.png "Yeni sınıf ekleyin")

20. İçinde **PhoneDialer.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod oluşturur `Dial` Android platformunda çevrilmiş telefon numarası çevirmek için kullanılacak yöntemi:

    ```csharp
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

                var intent = new Intent (Intent.ActionDial);
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
    ```

    Değişiklikleri kaydetmek **PhoneDialer.cs** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

21. İçinde **Çözüm Gezgini**, **Phoneword.Android** proje, çift **MainActivity.cs** açmak için tüm şablon kodunu kaldırın ve değiştirin Aşağıdaki kodu:

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
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

    Değişiklikleri kaydetmek **MainActivity.cs** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

22. İçinde **Çözüm Gezgini**, **Phoneword.Android** proje, çift **özellikleri** seçip **Android bildirim** sekmesinde:

    ![](quickstart-images/vs/android-manifest.png "Açık Android bildirimi")

23. İçinde **gerekli izinler** bölümünde, etkinleştirme **CALL_PHONE** izni. Bu, bir telefon araması yerleştirmek için uygulamaya izin verir:

    ![](quickstart-images/vs/android-manifest-changed.png "CallPhone iznini etkinleştirme")

    Tuşlarına basarak bildirime değişiklikleri kaydetmek **CTRL + S**ve dosyayı kapatın.

24. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.UWP** seçin ve proje **Ekle > Yeni öğe...** :

    ![](quickstart-images/vs/add-new-item-uwp.png "Yeni Öğe Ekle")

25. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# > kod > sınıfı**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **Ekle** düğmesi:

    ![](quickstart-images/vs/new-phone-dialer-uwp.w157.png "Yeni sınıf ekleyin")

26. İçinde **PhoneDialer.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod oluşturur `Dial` yöntemi ve evrensel Windows platformu üzerinde çevrilmiş telefon numarası çevirmek için kullanılan yardımcı yöntemler:

    ```csharp
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
    ```

    Değişiklikleri kaydetmek **PhoneDialer.cs** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

27. İçinde **Çözüm Gezgini**, **Phoneword.UWP** projesinde **başvuruları**seçip **Başvuru Ekle...** :

    ![](quickstart-images/vs/uwp-add-reference.png "Başvuru ekleme")

28. İçinde **başvuru Yöneticisi** iletişim kutusunda **Evrensel Windows > uzantılar > UWP için Windows Mobile uzantıları**, tıklatıp **Tamam** düğmesi:

    ![](quickstart-images/vs/uwp-add-reference-extensions.png "UWP için Windows mobil uzantıları ekleme")

29. İçinde **Çözüm Gezgini**, **Phoneword.UWP** proje, çift **Package.appxmanifest**:

    ![](quickstart-images/vs/uwp-manifest.png "UWP bildirimini açın")

30. İçinde **özellikleri** sayfasında, etkinleştirmek **telefon araması** yeteneği. Bu, bir telefon araması yerleştirmek için uygulamaya izin verir:

    ![](quickstart-images/vs/uwp-manifest-changed.png "Telefon araması özelliğini etkinleştir")

    Tuşlarına basarak bildirime değişiklikleri kaydetmek **CTRL + S**ve dosyayı kapatın.

31. Visual Studio'da **Yapı > Çözümü Derle** menü öğesi (veya basın **CTRL + SHIFT + B**). Uygulamayı derler ve Visual Studio durum çubuğunda bir başarı iletisi görüntülenir:

    ![](quickstart-images/vs/build-successful.png "Derleme başarılı oldu")

    Hatalar varsa, önceki adımları tekrarlayın ve uygulama başarıyla oluşturulursa kadar hataları düzeltin.

32. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.UWP** seçin ve proje **başlangıç projesi olarak ayarla**:

    ![](quickstart-images/vs/uwp-set-as-startup-project.png "Başlangıç projesi olarak ayarla")

33. Visual Studio araç çubuğunda tuşlarına basarak **Başlat** düğmesine (Oynat düğmesine benzeyen üçgen düğme) uygulamayı başlatmak için:

    ![](quickstart-images/vs/start.png "Visual Studio araç")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword uygulama UWP")

34. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.Android** seçin ve proje **başlangıç projesi olarak ayarla**.
35. Visual Studio araç çubuğunda tuşlarına basarak **Başlat** düğmesine (Oynat düğmesine benzeyen üçgen düğme) içinde Android öykünücüsünde uygulamayı başlatmak için.
36. Bir iOS cihazını ve Xamarin.Forms geliştirme Mac sistem gereksinimlerini, uygulamayı iOS cihazına dağıtmak için benzer bir teknik kullanın. Alternatif olarak, uygulamaya dağıtma [uzak iOS simülatörü](~/tools/ios-simulator.md).

    Not: üzerinde tüm simülatörleri telefon aramaları desteklenmez.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Visual Studio Başlangıç sayfasında ve Mac için Başlat'a tıklayın **yeni proje...**  yeni bir proje oluşturmak için:

    ![](quickstart-images/xs/new-solution.png "Yeni çözüm")

2. İçinde **yeni projeniz için bir şablon seçin** iletişim kutusunda, tıklayın **çok platformlu > Uygulama**seçin **boş Forms uygulaması** şablon seçeneğine tıklayıp **sonraki** düğmesi:

    ![](quickstart-images/xs/choose-template.png "Bir şablon seçin")

3. İçinde **boş Forms yapılandırmanız** iletişim kutusunda, yeni uygulama adı **Phoneword**, emin **kullanım .NET Standard** radyo düğmesi seçilir ve tıklayın**Sonraki** düğmesi:

    ![](quickstart-images/xs/configure-app.png "Forms uygulaması yapılandırma")

4. İçinde **yeni boş Forms yapılandırmanız** iletişim kutusunda, çözüm ve proje adları kümesine bırakın **Phoneword**, proje için uygun bir konum seçin ve tıklayın **Oluştur**projeyi oluşturmak için:

    ![](quickstart-images/xs/configure-project.png "Forms projesi yapılandırma")

    > [!NOTE]
    > Çözüm ve proje adı başarısız **Phoneword** çok sayıda derleme hatalarına neden.

5. İçinde **çözüm bölmesi**, çift **MainPage.xaml** açmak için:

    ![](quickstart-images/xs/open-mainpage-xaml.png "Mainpage.XAML dosyasını açın")

6. İçinde **MainPage.xaml**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod sayfası için kullanıcı arabirimi bildirimli olarak tanımlar:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    Değişiklikleri kaydetmek **MainPage.xaml** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

7. İçinde **çözüm bölmesi**, çift **MainPage.xaml.cs** açmak için:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs açın")

8. İçinde **MainPage.xaml.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. `OnTranslate` Ve `OnCall` yöntemleri, yanıt olarak yürütülecek **çevir** ve **çağrı** kullanıcı arabiriminde sırasıyla tıklanan düğmeler:

    ```csharp
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
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
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
    ```

    > [!NOTE]
    > Bu noktada uygulamayı oluşturmaya çalışırken, daha sonra düzeltilecek hatalara neden olabilecek.

    Değişiklikleri kaydetmek **MainPage.xaml.cs** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

9. İçinde **çözüm bölmesi**seçin **Phoneword** proje için sağ tıklatın ve seçin **Ekle > yeni dosya...** :

    ![](quickstart-images/xs/add-new-translator-file.png "Yeni bir dosya ekleyin")

10. İçinde **yeni dosya** iletişim kutusunda **genel > boş sınıf**, yeni dosya adı **PhoneTranslator**, tıklatıp **yeni** düğmesi:

    ![](quickstart-images/xs/add-translator-class.png "Yeni sınıf ekleyin")

11. İçinde **PhoneTranslator.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod, bir telefon numarasına bir telefon sözcük çevirir:

    ```csharp
    using System.Text;

    namespace Phoneword
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
    ```

    Değişiklikleri kaydetmek **PhoneTranslator.cs** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

12. İçinde **çözüm bölmesi**seçin **Phoneword** proje için sağ tıklatın ve seçin **Ekle > yeni dosya...** :

    ![](quickstart-images/xs/add-new-interface.png "Yeni bir dosya ekleyin")

13. İçinde **yeni dosya** iletişim kutusunda **genel > boş arabirim**, yeni dosya adı **IDialer**, tıklatıp **yeni** düğmesi:

    ![](quickstart-images/xs/add-idialer-interface.png "Yeni arabirim ekleme")

14. İçinde **IDialer.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kodu tanımlayan bir `Dial` her platformda çevrilmiş telefon numarası çevirmek için uygulanması gereken yöntemini:

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```
    Değişiklikleri kaydetmek **IDialer.cs** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

    > [!NOTE]
    > Uygulama için ortak kodun tamamlanmıştır. Platforma özgü telefon çevirici kodu artık uygulanmasını olarak bir [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

15. İçinde **çözüm bölmesi**seçin **Phoneword.iOS** proje için sağ tıklatın ve seçin **Ekle > yeni dosya...** :

    ![](quickstart-images/xs/add-new-file-ios.png "Yeni bir dosya ekleyin")

16. İçinde **yeni dosya** iletişim kutusunda **genel > boş sınıf**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **yeni** düğmesi:

    ![](quickstart-images/xs/new-phonedialer-ios.png "Yeni sınıf ekleyin")

17. İçinde **PhoneDialer.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod oluşturur `Dial` iOS platformunda çevrilmiş telefon numarası çevirmek için kullanılacak yöntemi:

    ```csharp
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
    ```

    Değişiklikleri kaydetmek **PhoneDialer.cs** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

18. İçinde **çözüm bölmesi**seçin **Phoneword.Droid** proje için sağ tıklatın ve seçin **Ekle > yeni dosya...** :

    ![](quickstart-images/xs/add-new-file-android.png "Yeni bir dosya ekleyin")

19. İçinde **yeni dosya** iletişim kutusunda **genel > boş sınıf**, yeni dosya adı **Telefon Çeviricisi**, tıklatıp **yeni** düğmesi:

    ![](quickstart-images/xs/new-phonedialer-android.png "Yeni sınıf ekleyin")

20. İçinde **PhoneDialer.cs**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod oluşturur `Dial` Android platformunda çevrilmiş telefon numarası çevirmek için kullanılacak yöntemi:

    ```csharp
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

                var intent = new Intent (Intent.ActionDial);
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
    ```

    Değişiklikleri kaydetmek **PhoneDialer.cs** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

21. İçinde **çözüm bölmesi**, **Phoneword.Droid** çift tıklayın, proje **MainActivity.cs** açmak için tüm şablon kodunu kaldırın ve aşağıdaki kod ile değiştirin :

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
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

    Değişiklikleri kaydetmek **MainActivity.cs** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

22. İçinde **çözüm bölmesi**, genişletme **özellikleri** klasörü ve çift **AndroidManifest.xml** dosyası:

    ![](quickstart-images/xs/android-manifest.png "Açık Android bildirimi")

23. İçinde **gerekli izinler** bölümünde, etkinleştirme **CallPhone** izni. Bu, bir telefon araması yerleştirmek için uygulamaya izin verir:

    ![](quickstart-images/xs/android-manifest-changed.png "CallPhone iznini etkinleştirme")

    Değişiklikleri kaydetmek **AndroidManifest.xml** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

24. Mac için Visual Studio'da **Yapı > derleme tüm** menü öğesi (veya basın  **&#8984; + B**). Uygulamayı derler ve araç Mac için Visual Studio bir başarı iletisi görünür.

    ![](quickstart-images/xs/build-successful.png "Derleme başarılı oldu")

25. Hatalar varsa, önceki adımları tekrarlayın ve uygulama başarıyla oluşturulursa kadar hataları düzeltin.
26. Araç Mac için Visual Studio'da basın **Başlat** düğmesine (Oynat düğmesine benzeyen üçgen düğme) iOS simülatörü içinde uygulamayı başlatmak için:

    ![](quickstart-images/xs/start.png "Araç Mac için Visual Studio")
    ![](quickstart-images/xs/phoneword-result-ios.png "iOS simülatörü")

    Not: iOS simülatörü telefon aramaları desteklenmez.

27. İçinde **çözüm bölmesi**seçin **Phoneword.Droid** proje için sağ tıklatın ve seçin **başlangıç projesi olarak ayarla**:

    ![](quickstart-images/xs/set-startup-project.png "Başlangıç projesi olarak ayarla")

28. Araç Mac için Visual Studio'da basın **Başlat** düğmesine (Oynat düğmesine benzeyen üçgen düğme) içinde Android öykünücüsünde uygulamayı başlatmak için:

    ![](quickstart-images/xs/phoneword-result-android.png "Android öykünücüsü")

    Not: Android öykünücüleri telefon aramaları desteklenmez.

-----

Xamarin.Forms uygulaması tamamlamada Tebrik ederiz. [Bir sonraki konu](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md) bu kılavuzda, bir Xamarin.Forms kullanarak uygulama geliştirme temelleri anlamak için bu kılavuzda alınan adımları gözden geçirir.


## <a name="related-links"></a>İlgili bağlantılar

- [Yerel özellikler DependencyService aracılığıyla erişme](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
