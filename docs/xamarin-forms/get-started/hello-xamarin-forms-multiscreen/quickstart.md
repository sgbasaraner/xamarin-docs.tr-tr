---
title: Xamarin.Forms çoklu ekranı hızlı başlangıç
description: Bu makalede, uygulama için arama geçmişini izlemek için ikinci bir ekran ekleyerek Phoneword uygulamayı genişletmek açıklanmaktadır.
ms.prod: quickstart
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: a4e27f1810a16b5d13838d2e2c1067950586fab3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996187"
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Xamarin.Forms çoklu ekranı hızlı başlangıç

Bu hızlı başlangıçta, uygulama için arama geçmişini izlemek için ikinci bir ekran ekleyerek Phoneword uygulamayı genişletmek gösterilmektedir. Son uygulama aşağıda gösterilmiştir:

[![](quickstart-images/intro-app-examples-sml.png "Phoneword uygulama")](quickstart-images/intro-app-examples.png#lightbox "Phoneword uygulama")

Phoneword uygulamayı şu şekilde genişletir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio'yu başlatın. Başlangıç sayfasında tıklayın **Proje Aç...** hem de **açık proje** iletişim Phoneword proje için çözüm dosyası seçin:

    ![](quickstart-images/vs/open-solution.png "Proje Aç")

2. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword** seçin ve proje **Ekle > Yeni öğe...** :

    ![](quickstart-images/vs/add-new-item.png "Yeni Öğe Ekle")

3. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# öğeleri > Xamarin.Forms > içerik sayfası**, yeni bir öğe adı **CallHistoryPage**, tıklatıp **Ekle**  düğmesi. Bu adlı bir sayfa ekler **CallHistoryPage** projeye:

    ![](quickstart-images/vs/add-callhistorypage-class.png "Xamarin.Forms proje şablonları")

4. İçinde **CallHistoryPage.xaml**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod sayfası için kullanıcı arabirimi bildirimli olarak tanımlar:

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

    Değişiklikleri kaydetmek **CallHistoryPage.xaml** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

5. İçinde **Çözüm Gezgini**, çift **App.xaml.cs** paylaşılan dosya **Phoneword** proje açmak için:

    ![](quickstart-images/vs/open-app-class.png "App.xaml.cs açın")

6. İçinde **App.xaml.cs**, içeri aktarma `System.Collections.Generic` ad alanı bildirimi ekleyin `PhoneNumbers` özelliği özelliğini başlatmak `App` oluşturucusu ve başlatma [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) özelliğini bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). `PhoneNumbers` Koleksiyonu, bir uygulama tarafından aranan her çevrilmiş telefon numarası listesini depolamak için kullanılır:

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

    Değişiklikleri kaydetmek **App.xaml.cs** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

7. İçinde **Çözüm Gezgini**, çift **MainPage.xaml** paylaşılan dosya **Phoneword** proje açmak için:

    ![](quickstart-images/vs/open-mainpage-xaml.png "Mainpage.XAML dosyasını açın")

8. İçinde **MainPage.xaml**, ekleme bir [ `Button` ](xref:Xamarin.Forms.Button) sonunda denetim [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) denetimi. Düğme, çağrı geçmişi sayfasına gitmek için kullanılacak:

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

    Değişiklikleri kaydetmek **MainPage.xaml** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

9. İçinde **Çözüm Gezgini**, çift **MainPage.xaml.cs** açmak için:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs açın")

10. İçinde **MainPage.xaml.cs**, ekleme `OnCallHistory` olay işleyicisi yönteminde ve değiştirme `OnCall` çevrilmiş telefon numarası eklemek üzere olay işleyicisi yöntemi `App.PhoneNumbers` toplama ve etkinleştirme `callHistoryButton`, koşuluyla `dialer` değişken `null`:

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

    Değişiklikleri kaydetmek **MainPage.xaml.cs** tuşuna basarak **CTRL + S**ve dosyayı kapatın.

11. Visual Studio'da **Yapı > Çözümü Derle** menü öğesi (veya basın **CTRL + SHIFT + B**). Uygulamayı derler ve Visual Studio durum çubuğunda bir başarı iletisi görüntülenir:

    ![](quickstart-images/vs/build-successful.png "Derleme başarılı oldu")

    Hatalar varsa, önceki adımları tekrarlayın ve uygulama başarıyla oluşturulursa kadar hataları düzeltin.

12. Visual Studio araç çubuğunda tuşlarına basarak **Başlat** düğmesine (Oynat düğmesine benzeyen üçgen düğme) uygulamayı başlatmak için:

    ![](quickstart-images/vs/start.png "Visual Studio araç")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword uygulama UWP")

13. İçinde **Çözüm Gezgini**, sağ tıklayın **Phoneword.Droid** seçin ve proje **başlangıç projesi olarak ayarla**.
14. Visual Studio araç çubuğunda tuşlarına basarak **Başlat** düğmesine (Oynat düğmesine benzeyen üçgen düğme) içinde Android öykünücüsünde uygulamayı başlatmak için.
15. Bir iOS cihazını ve Xamarin.Forms geliştirme Mac sistem gereksinimlerini, uygulamayı iOS cihazına dağıtmak için benzer bir teknik kullanın. Alternatif olarak, uygulamaya dağıtma [uzak iOS simülatörü](~/tools/ios-simulator.md).

    Not: üzerinde tüm simülatörleri telefon aramaları desteklenmez.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'yu başlatın Başlangıç sayfasında tıklayın **Aç...** , iletişim kutusunda Phoneword proje için çözüm dosyası seçin:

    ![](quickstart-images/xs/open-solution.png "Çözüm Aç")

2. İçinde **çözüm bölmesi**seçin **Phoneword** proje için sağ tıklatın ve seçin **Ekle > yeni dosya...** :

    ![](quickstart-images/xs/add-new-file.png "Yeni bir dosya ekleyin")

3. İçinde **yeni dosya** iletişim kutusunda **Forms > Forms ContentPage Xaml**, yeni dosya adı **CallHistoryPage**, tıklatıp **yeni** düğmesi. Bu adlı bir sayfa ekler **CallHistoryPage** projeye:

    ![](quickstart-images/xs/add-callhistorypage-class.png "Forms ContentPage Ekle")

4. İçinde **çözüm bölmesi**, çift **CallHistoryPage.xaml** açmak için:

    ![](quickstart-images/xs/open-callhistorypage-xaml.png "CallHistoryPage.xaml açın")

5. İçinde **CallHistoryPage.xaml**, tüm şablon kodunu kaldırın ve aşağıdaki kodla değiştirin. Bu kod sayfası için kullanıcı arabirimi bildirimli olarak tanımlar:

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

    Değişiklikleri kaydetmek **CallHistoryPage.xaml** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

6. İçinde **çözüm bölmesi**, çift **App.xaml.cs** açmak için:

    ![](quickstart-images/xs/open-app-class.png "App.xaml.cs açın")

7. İçinde **App.xaml.cs**, içeri aktarma `System.Collections.Generic` ad alanı bildirimi ekleyin `PhoneNumbers` özelliği özelliğini başlatmak `App` oluşturucusu ve başlatma [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) özelliğini bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). `PhoneNumbers` Koleksiyonu, bir uygulama tarafından aranan her çevrilmiş telefon numarası listesini depolamak için kullanılır:

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

    Değişiklikleri kaydetmek **App.xaml.cs** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

8. İçinde **çözüm bölmesi**, çift **MainPage.xaml** açmak için:

    ![](quickstart-images/xs/open-mainpage-xaml.png "Mainpage.XAML dosyasını açın")

9. İçinde **MainPage.xaml**, ekleme bir [ `Button` ](xref:Xamarin.Forms.Button) sonunda denetim [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) denetimi. Düğme, çağrı geçmişi sayfasına gitmek için kullanılacak:

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

    Değişiklikleri kaydetmek **MainPage.xaml** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

10. İçinde **çözüm bölmesi**, çift **MainPage.xaml.cs** açmak için:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs açın")

11. İçinde **MainPage.xaml.cs**, ekleme `OnCallHistory` olay işleyicisi yönteminde ve değiştirme `OnCall` çevrilmiş telefon numarası eklemek üzere olay işleyicisi yöntemi `App.PhoneNumbers` toplama ve etkinleştirme `callHistoryButton`, koşuluyla `dialer` değişken `null`:

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

    Değişiklikleri kaydetmek **MainPage.xaml.cs** seçerek **Dosya > Kaydet** (veya basarak  **&#8984; + S**) ve dosyayı kapatın.

12. Mac için Visual Studio'da **Yapı > derleme tüm** menü öğesi (veya basın  **&#8984; + B**). Uygulamayı derler ve araç Mac için Visual Studio'da bir başarı iletisi görüntülenir:

    ![](quickstart-images/xs/build-successful.png "Derleme başarılı oldu")

    Hatalar varsa, önceki adımları tekrarlayın ve uygulama başarıyla oluşturulursa kadar hataları düzeltin.

13. Araç Mac için Visual Studio'da basın **Başlat** düğmesine (Oynat düğmesine benzeyen üçgen düğme) iOS simülatörü içinde uygulamayı başlatmak için:

    ![](quickstart-images/xs/start.png "Araç Mac için Visual Studio")
    ![](quickstart-images/xs/phone-result-ios.png "iOS simülatörü")

    Not: iOS simülatörü telefon aramaları desteklenmez.

14. İçinde **çözüm bölmesi**seçin **Phoneword.Droid** proje için sağ tıklatın ve seçin **başlangıç projesi olarak ayarla**:

    ![](quickstart-images/xs/set-startup-project.png "Proje başlangıç olarak ayarla")

15. Araç Mac için Visual Studio'da basın **Başlat** düğmesine (Oynat düğmesine benzeyen üçgen düğme) içinde Android öykünücüsünde uygulamayı başlatmak için:

    ![](quickstart-images/xs/phone-result-android.png "Android öykünücüsü")

    Not: Android öykünücüleri telefon aramaları desteklenmez.

-----

Bir çoklu ekranı Xamarin.Forms uygulaması tamamlamada Tebrik ederiz. [Bir sonraki konu](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md) bu kılavuzda bir anlayış sayfa gezintisi ve Xamarin.Forms kullanarak veri bağlama geliştirmek için bu kılavuzda alınan adımları gözden geçirir.


## <a name="related-links"></a>İlgili bağlantılar

- [Phoneword (örnek)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (örnek)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
