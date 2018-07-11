---
title: WPF Platform Kurulumu
description: Xamarin.Forms WPF platform Önizleme desteği sunuyor
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 416e33f131c6e1ef144608f98964fd8372f454f8
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831325"
---
# <a name="wpf-platform-setup"></a>WPF Platform Kurulumu

![Önizleme](~/media/shared/preview.png)

Xamarin.Forms, artık Windows Presentation Foundation (WPF) için Önizleme desteği vardır. Bu makalede, bir WPF proje için Xamarin.Forms çözümü eklemek gösterilmektedir.

Başlat, Visual Studio 2017'de yeni bir Xamarin.Forms çözümü oluşturun veya varolan bir Xamarin.Forms çözümü örneğin kullanın önce [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/). Yalnızca, bir Xamarin.Forms çözümü Windows içinde WPF uygulamaları ekleyebilirsiniz.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Xamarin.Forms uygulaması Xamarin.University ile WPF proje ekleme

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF desteği ile [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-wpf-app"></a>Bir WPF uygulaması ekleme

Windows 7, 8 ve 10 masaüstü bilgisayarlarda çalışacak bir WPF uygulaması eklemek için aşağıdaki yönergeleri izleyin:

1. Visual Studio 2017'de çözüm adına sağ tıklayın **Çözüm Gezgini** ve **Ekle > Yeni proje...** .

2. İçinde **yeni proje** penceresinin sol Seç **Visual C#** ve **Windows Klasik Masaüstü**. Proje türleri listesinde seçin **WPF uygulaması (.NET Framework)**. 

3. Proje için bir ad yazın bir **WPF** uzantısı, örneğin, **BoxViewClock.WPF**. Tıklayın **Gözat** düğmesini seçme **BoxViewClock** klasörü ve ENTER tuşuna **klasörü seçin**. Bu çözümdeki diğer projelerin ile aynı dizinde WPF projesi koyacaktır.

    ![Yeni bir WPF projesi eklemek](wpf-images/add-new-project.png "yeni bir WPF projesi eklemek")

    Projeyi oluşturmak için Tamam'ı tıklatın.

4. İçinde **Çözüm Gezgini**, sağ tıklayın yeni **BoxViewClock.WPF** seçin ve proje **NuGet paketlerini Yönet**. Seçin **Gözat** sekmesinde **ön sürümü dahil et** onay kutusunu ve arama **Xamarin.Forms**.

    ![NuGet paketini seçin](wpf-images/select-nuget-package.png "NuGet paketini seçin")

    Paket ve select **yükleme** düğmesi.

5. Artık arama **Xamarin.Forms.Platform.WPF** paketini ve bir de yükleyin. Microsoft paket olduğundan emin olun.

6. Çözüm adına sağ tıklayın **Çözüm Gezgini** seçip **çözüm için NuGet paketlerini Yönet**. Seçin **güncelleştirme** sekmesi ve **Xamarin.Forms** paket. Tüm projeleri seçin ve bunları aynı Xamarin.Forms sürüme güncelleştirin:

    ![NuGet Paketi güncelleştirmesi](wpf-images/update-nuget-package.png "NuGet paketini güncelleştir") 

7. WPF projesinde sağ **başvuruları**. İçinde **başvuru Yöneticisi** iletişim kutusunda **projeleri** sola ve onay bitişik onay **BoxViewClock** proje:

    ![Paylaşılan proje başvurusu](wpf-images/reference-shared-project.png "paylaşılan proje başvurusu")

8. Düzen **MainWindow.xaml** WPF projesi dosyası. İçinde `Window` etiketinde, bir XML ad alanı bildirimi ekleyin **Xamarin.Forms.Platform.WPF** derlemeyi ve ad alanı:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Şimdi değiştir `Window` etiketini `wpf:FormsApplicationPage`. Değişiklik `Title` , uygulamanızın, örneğin, adını ayarlama **BoxViewClock**. Tamamlanmış XAML dosyasını şu şekilde görünmelidir:

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

9. Düzen **MainWindow.xaml.cs** WPF projesi dosyası. İki yeni Ekle `using` yönergeleri:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Temel sınıfını değiştirmek `MainWindow` gelen `Window` için `FormsApplicationPage`. Aşağıdaki `InitializeComponent` çağrı, aşağıdaki iki deyimi ekleyin:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    Açıklama dışında ve kullanılmayan `using` yönergeleri, tam **MainWindows.xaml.cs** dosya şu şekilde görünmelidir:

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

10. WPF projeye sağ **Çözüm Gezgini** seçip **başlangıç projesi olarak ayarla**. Program, Windows masaüstünde, Visual Studio hata ayıklayıcısı ile çalıştırmak için F5 tuşuna basın:

    ![WPF BoxView saati](wpf-images/wpf-boxviewclock.png "WPF BoxView saati" )

## <a name="next-steps"></a>Sonraki Adımlar

### <a name="platform-specifics"></a>Platform özellikleri

Xamarin.Forms uygulamanızı kod veya XAML çalıştığı platformdan belirleyebilirsiniz. Bu program özelliklerini WPF üzerinde çalışırken değiştirmenize olanak sağlar. Kod içinde değerini karşılaştırın `Device.RuntimePlatform` ile `Device.WPF` (hangi "WPF" dize eşittir) sabiti. Bir eşleşme varsa, uygulama WPF üzerinde çalışıyor.

XAML içinde kullanabileceğiniz `OnPlatform` platforma özgü bir özellik değer seçmek için etiketi:

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

WPF penceresinde ilk boyutu ayarlayabileceğiniz **MainWindow.xaml** dosyası:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Sorunlar

Her şey üretime hazır olduğunu beklemelisiniz. Bu bir önizleme olduğundan. Xamarin.Forms için tüm NuGet paketlerini WPF için hazır ve bazı özellikleri tam olarak çalışmıyor.

