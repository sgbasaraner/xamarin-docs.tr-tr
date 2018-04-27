---
title: Xamarin.Forms Multiscreen hızlı başlangıç
ms.prod: quickstart
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 066f084187a486ca2f88882890b5e9ad277b8cff
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Xamarin.Forms Multiscreen hızlı başlangıç

Bu hızlı başlangıç Phoneword uygulamayı uygulama için arama geçmişini izlemek için ikinci bir ekran ekleyerek genişletebilir gösterilmiştir. Son uygulama aşağıda gösterilmiştir:

[![](quickstart-images/intro-app-examples-sml.png "Phoneword uygulama")](quickstart-images/intro-app-examples.png#lightbox "Phoneword uygulama")

Phoneword uygulamayı şu şekilde genişletir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio'yu başlatın. Başlangıç sayfasında **Proje Aç...** hem de **Proje Aç** iletişim Phoneword projesi için çözüm dosyasını seçin:

    ![](quickstart-images/vs/open-solution.png "Proje Aç")

2. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword** proje ve seçin **Ekle > Yeni öğe...** :

    ![](quickstart-images/vs/add-new-item.png "Yeni Öğe Ekle")

3. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# öğeleri > Xamarin.Forms > içerik sayfasını**, yeni öğe adı **CallHistoryPage**, tıklatıp **Ekle**  düğmesi. Bu adlı bir sayfaya ekler **CallHistoryPage** projeye:

    ![](quickstart-images/vs/add-callhistorypage-class.png "Xamarin.Forms proje şablonları")

4. İçinde **CallHistoryPage.xaml**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod sayfası için kullanıcı arabirimi bildirimli olarak tanımlar:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>
    ```

    Değişiklikleri kaydetmek **CallHistoryPage.xaml** basarak **CTRL + S**ve dosyayı kapatın.

5. İçinde **Çözüm Gezgini**, çift **App.xaml.cs** açmak için:

    ![](quickstart-images/vs/open-app-class.png "App.xaml.cs açın")

6. İçinde **App.xaml.cs**, içeri aktarma `System.Collections.Generic` ad alanı, bildirimi ekleme `PhoneNumbers` özelliği, özelliğinde başlatma `App` oluşturucu ve başlatma [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) özelliğini bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). `PhoneNumbers` Koleksiyonu, bir uygulama tarafından aranan her çevrilen telefon numarası listesini depolamak için kullanılacak:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Değişiklikleri kaydetmek **App.xaml.cs** basarak **CTRL + S**ve dosyayı kapatın.

7. İçinde **Çözüm Gezgini**, çift **MainPage.xaml** açmak için:

    ![](quickstart-images/vs/open-mainpage-xaml.png "MainPage.xaml açın")

8. İçinde **MainPage.xaml**, ekleme bir [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) sonunda denetim [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) denetim. Düğme çağrısı geçmişi sayfasına gitmek için kullanılır:

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Değişiklikleri kaydetmek **MainPage.xaml** basarak **CTRL + S**ve dosyayı kapatın.

9. İçinde **Çözüm Gezgini**, çift **MainPage.xaml.cs** açmak için:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs açın")

10. İçinde **MainPage.xaml.cs**, ekleme `OnCallHistory` olay işleyicisi yöntemi ve değiştirme `OnCall` çevrilen telefon numarası eklemek üzere olay işleyicisi yöntemi `App.PhoneNumbers` toplama ve Etkinleştir `callHistoryButton`, koşuluyla `dialer` değişken değil `null`:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Değişiklikleri kaydetmek **MainPage.xaml.cs** basarak **CTRL + S**ve dosyayı kapatın.

11. Visual Studio'da seçin **Yapı > Yapı çözümü** menü öğesi (veya basın **CTRL + SHIFT + B**). Uygulamayı oluşturacaksınız ve Visual Studio durum çubuğunda bir başarı iletisi görüntülenir:

    ![](quickstart-images/vs/build-successful.png "Yapı başarılı")

    Hatalar varsa, önceki adımı yineleyin ve uygulama başarıyla derlemeler kadar hataları düzeltin.

12. Visual Studio araç çubuğunda tuşlarına basarak **Başlat** düğmesini (Oynat düğmesini benzer üçgen düğmesi) uygulamayı başlatmak için:

    ![](quickstart-images/vs/start.png "Visual Studio araç")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword uygulama UWP")

13. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.Droid** proje ve seçin **başlangıç projesi olarak ayarla**.
14. Visual Studio araç çubuğunda tuşlarına basarak **Başlat** düğmesini (Oynat düğmesini benzer üçgen düğmesi) Android öykünücüsünde içinde uygulamayı başlatmak için.
15. Bir iOS aygıtı varsa ve Xamarin.Forms geliştirme için Mac sistem gereksinimlerini karşılayan, iOS cihazını uygulama dağıtmak için benzer bir teknik kullanın. Alternatif olarak, uygulamayı dağıtmak [iOS uzak simulator](~/tools/ios-simulator.md).

    Not: telefon aramaları tüm benzeticileri desteklenmez.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'yu başlatın Başlangıç sayfasında **Aç...** ve iletişim kutusunda Phoneword projesi için çözüm dosyasını seçin:

    ![](quickstart-images/xs/open-solution.png "Çözümü Aç")

2. İçinde **çözüm paneli**seçin **Phoneword** proje, sağ tıklatın ve seçin **Ekle > yeni dosya...** :

    ![](quickstart-images/xs/add-new-file.png "Yeni dosya ekleme")

3. İçinde **yeni dosya** iletişim kutusunda **Forms > Forms ContentPage Xaml**, yeni dosya adı **CallHistoryPage**, tıklatıp **yeni** düğme. Bu adlı bir sayfaya ekler **CallHistoryPage** projeye:

    ![](quickstart-images/xs/add-callhistorypage-class.png "Forms ContentPage Ekle")

4. İçinde **çözüm paneli**, çift **CallHistoryPage.xaml** açmak için:

    ![](quickstart-images/xs/open-callhistorypage-xaml.png "CallHistoryPage.xaml açın")

5. İçinde **CallHistoryPage.xaml**, tüm şablonu kodu kaldırın ve aşağıdaki kod ile değiştirin. Bu kod sayfası için kullanıcı arabirimi bildirimli olarak tanımlar:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>      
    ```

    Değişiklikleri kaydetmek **CallHistoryPage.xaml** seçerek **Dosya > Kaydet** (veya tuşlarına basarak  **&#8984; + S**) ve dosyayı kapatın.

6. İçinde **çözüm paneli**, çift **App.xaml.cs** açmak için:

    ![](quickstart-images/xs/open-app-class.png "App.xaml.cs açın")

7. İçinde **App.xaml.cs**, içeri aktarma `System.Collections.Generic` ad alanı, bildirimi ekleme `PhoneNumbers` özelliği, özelliğinde başlatma `App` oluşturucu ve başlatma [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) özelliğini bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). `PhoneNumbers` Koleksiyonu, bir uygulama tarafından aranan her çevrilen telefon numarası listesini depolamak için kullanılacak:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Değişiklikleri kaydetmek **App.xaml.cs** seçerek **Dosya > Kaydet** (veya tuşlarına basarak  **&#8984; + S**) ve dosyayı kapatın.

8. İçinde **çözüm paneli**, çift **MainPage.xaml** açmak için:

    ![](quickstart-images/xs/open-mainpage-xaml.png "MainPage.xaml açın")

9. İçinde **MainPage.xaml**, ekleme bir [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) sonunda denetim [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) denetim. Düğme çağrısı geçmişi sayfasına gitmek için kullanılır:

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Değişiklikleri kaydetmek **MainPage.xaml** seçerek **Dosya > Kaydet** (veya tuşlarına basarak  **&#8984; + S**) ve dosyayı kapatın.

10. İçinde **çözüm paneli**, çift **MainPage.xaml.cs** açmak için:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs açın")

11. İçinde **MainPage.xaml.cs**, ekleme `OnCallHistory` olay işleyicisi yöntemi ve değiştirme `OnCall` çevrilen telefon numarası eklemek üzere olay işleyicisi yöntemi `App.PhoneNumbers` toplama ve Etkinleştir `callHistoryButton`, koşuluyla `dialer` değişken değil `null`:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Değişiklikleri kaydetmek **MainPage.xaml.cs** seçerek **Dosya > Kaydet** (veya tuşlarına basarak  **&#8984; + S**) ve dosyayı kapatın.

12. Mac için Visual Studio'da seçin **Yapı > Yapı tüm** menü öğesi (veya basın  **&#8984; + B**). Uygulamayı oluşturacaksınız ve Mac araç için Visual Studio'da bir başarı iletisi görüntülenir:

    ![](quickstart-images/xs/build-successful.png "Yapı başarılı")

    Hatalar varsa, önceki adımı yineleyin ve uygulama başarıyla derlemeler kadar hataları düzeltin.

13. Mac araç için Visual Studio basın **Başlat** düğmesini (Oynat düğmesini benzer üçgen düğmesi) iOS simülatörü'nü içinde uygulamayı başlatmak için:

    ![](quickstart-images/xs/start.png "Visual Studio için Mac araç")
    ![](quickstart-images/xs/phone-result-ios.png "iOS simülatörü")

    Not: telefon aramaları iOS Simulator'da desteklenmez.

14. İçinde **çözüm paneli**seçin **Phoneword.Droid** proje, sağ tıklatın ve seçin **başlangıç projesi olarak ayarla**:

    ![](quickstart-images/xs/set-startup-project.png "Ayarlama başlangıç projesi")

15. Mac araç için Visual Studio basın **Başlat** düğmesini (Oynat düğmesini benzer üçgen düğmesi) Android öykünücüsünde içinde uygulamayı başlatmak için:

    ![](quickstart-images/xs/phone-result-android.png "Android öykünücüsü")

    Not: telefon aramaları Android öykünücüsünü içinde desteklenmez.

-----

Bir multiscreen Xamarin.Forms uygulaması Tamamlanıyor tebrikler. [Sonraki konuyu](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md) bu kılavuzda, bu kılavuzda, sayfa gezintisi anlaşılması ve Xamarin.Forms kullanarak veri bağlama geliştirmek için gerçekleştirilen adımları gözden geçirir.


## <a name="related-links"></a>İlgili bağlantılar

- [Phoneword (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
