---
title: WPF Platform Kurulumu
description: Xamarin.Forms artık WPF platform Önizleme desteği vardır
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 416e33f131c6e1ef144608f98964fd8372f454f8
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="wpf-platform-setup"></a>WPF Platform Kurulumu

![Önizleme](~/media/shared/preview.png)

Xamarin.Forms Önizleme destek Windows Presentation Foundation (WPF) için artık sahiptir. Bu makalede, bir Xamarin.Forms çözüme bir WPF projesi ekleme gösterilmiştir.

Başlangıç, Visual Studio 2017 içinde yeni bir Xamarin.Forms çözümü oluşturun veya varolan bir Xamarin.Forms çözümü Örneğin, kullanmadan önce [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/). Yalnızca, bir Xamarin.Forms çözümünü Windows WPF uygulamaları ekleyebilirsiniz.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Bir WPF projesi Xamarin.University ile Xamarin.Forms uygulaması ekleyin

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF desteği göre [Xamarin Üniversitesi](https://university.xamarin.com/)**

## <a name="adding-a-wpf-app"></a>Bir WPF uygulaması ekleme

Windows 7, 8 ve 10 masaüstlerinde çalışacak bir WPF uygulaması eklemek için aşağıdaki yönergeleri izleyin:

1. Visual Studio 2017 ' çözüm adına sağ tıklayın **Çözüm Gezgini** ve **Ekle > Yeni proje...** .

2. İçinde **yeni proje** penceresinde, soldaki seçin **Visual C#** ve **Windows Klasik Masaüstü**. Proje türleri listesinden seçip **WPF uygulaması (.NET Framework)**. 

3. Proje ile için bir ad yazın bir **WPF** uzantısı, örneğin, **BoxViewClock.WPF**. ' I tıklatın **Gözat** düğmesini seçin **BoxViewClock** klasörü ve tuşuna **Klasör Seç**. Bu çözümdeki diğer projeler ile aynı dizinde WPF projesi sokar.

    ![Yeni bir WPF projesi eklemek](wpf-images/add-new-project.png "yeni bir WPF projesi ekleme")

    Projeyi oluşturmak için Tamam'ı tıklatın.

4. İçinde **Çözüm Gezgini**, sağ tıklatın yeni **BoxViewClock.WPF** proje ve seçin **NuGet paketlerini Yönet**. Seçin **Gözat** sekmesini tıklatın, **dahil et** onay kutusunu arayın ve **Xamarin.Forms**.

    ![NuGet paketini seçin](wpf-images/select-nuget-package.png "NuGet paketi seçin")

    Paket ve select **yükleme** düğmesi.

5. Şimdi arama **Xamarin.Forms.Platform.WPF** paketini ve bir de yükleyin. Microsoft'tan paketi olduğundan emin olun!

6. Sağ tıklayın çözüm adı **Çözüm Gezgini** seçip **çözüm için NuGet paketlerini Yönet**. Seçin **güncelleştirme** sekmesi ve **Xamarin.Forms** paket. Tüm projeleri seçin ve bunları aynı Xamarin.Forms sürüme güncelleştirin:

    ![NuGet paketi güncelleştirme](wpf-images/update-nuget-package.png "NuGet paketi güncelleştirme") 

7. WPF projeye sağ tıklayın **başvurular**. İçinde **başvuru Yöneticisi** iletişim kutusunda **projeleri** sola ve onay bitişik onay kutusunu **BoxViewClock** proje:

    ![Paylaşılan bir proje başvurusu](wpf-images/reference-shared-project.png "paylaşılan proje başvurusu")

8. Düzen **MainWindow.xaml** WPF projesi dosyası. İçinde `Window` etiketi, bir XML ad alanı bildirimi için eklemek **Xamarin.Forms.Platform.WPF** derleme ve ad alanı:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Şimdi değiştir `Window` etiketini `wpf:FormsApplicationPage`. Değişiklik `Title` ayar, bu gibi bir durumda uygulamanızın adı için **BoxViewClock**. Tamamlanan XAML dosyasını aşağıdaki gibi görünmelidir:

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>
        
        </Grid>
    </wpf:FormsApplicationPage>
    ```

9. Düzen **MainWindow.xaml.cs** WPF projesi dosyası. İki yeni ekleme `using` yönergeleri:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Temel sınıfını değiştirme `MainWindow` gelen `Window` için `FormsApplicationPage`. Aşağıdaki `InitializeComponent` çağrısı, aşağıdaki iki deyimlerini ekleyin:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    Yorumlar dışında ve kullanılmayan `using` yönergeleri, tam **MainWindows.xaml.cs** dosya şu şekilde görünmelidir:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

10. WPF projeye sağ **Çözüm Gezgini** seçip **başlangıç projesi olarak ayarla**. Windows masaüstünde, Visual Studio hata ayıklayıcısı ile programı çalıştırmak için F5 tuşuna basın:

    ![WPF BoxView saati](wpf-images/wpf-boxviewclock.png "WPF BoxView saati" )

## <a name="next-steps"></a>Sonraki Adımlar

### <a name="platform-specifics"></a>Platform özellikleri

Xamarin.Forms uygulamanızı kod veya XAML çalıştığı hangi platformu belirleyebilirsiniz. Bu WPF üzerinde çalışırken program özelliklerini değiştirmenize olanak sağlar. Kod içinde değerini karşılaştırın `Device.RuntimePlatform` ile `Device.WPF` (hangi "WPF" dizesi eşittir) sabiti. Bir eşleşme varsa, uygulamanın WPF üzerinde çalışıyor.

XAML'de, kullandığınız `OnPlatform` etiketi platformuna özel bir özellik değeri seçin:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="window-size"></a>Pencere boyutu

WPF penceresinde ilk boyutunu ayarlayabilirsiniz **MainWindow.xaml** dosyası:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Sorunlar

Her şeyin üretim hazır olduğunu beklemelisiniz şekilde bu bir önizleme sürümü. Xamarin.Forms tüm NuGet paketlerini WPF için hazır olduğunu ve bazı özellikleri tam olarak çalışmıyor.

