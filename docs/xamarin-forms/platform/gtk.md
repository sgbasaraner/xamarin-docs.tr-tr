---
title: 'GTK # Platform Kurulumu'
description: 'Xamarin.Forms GTK # platform Önizleme desteği artık sahiptir.'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 7f68b7c8affc11b50bdb4a2fc9589f8dcbfb45ec
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209196"
---
# <a name="gtk-platform-setup"></a>GTK # Platform Kurulumu

![Önizleme](~/media/shared/preview.png)

Xamarin.Forms GTK # uygulamaları için Önizleme destek artık sahiptir. GTK # GTK + Araç Seti ve GNOME kitaplıkları, çeşitli bağlanan bir grafik kullanıcı arabirimi araç Mono ve .NET kullanarak tam olarak yerel GNONE grafik uygulamaları geliştirme izin verme ' dir. Bu makalede, bir Xamarin.Forms çözüme GTK # projesi ekleme gösterilmiştir.

Başlat, yeni bir Xamarin.Forms çözümü oluşturun veya varolan bir Xamarin.Forms çözümü Örneğin, kullanmadan önce [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/).

> [!NOTE]
> Bu makalede GTK # uygulama VS2017 ve Visual Studio Xamarin.Forms çözümde Mac için ekleme üzerinde odaklanmakla birlikte, bu da gerçekleştirilebilecek [MonoDevelop](http://www.monodevelop.com/) Linux için.

## <a name="adding-a-gtk-app"></a>GTK # uygulama ekleme

GTK # macOS için ve Linux parçası olarak yüklü [Mono](http://www.mono-project.com/download/stable/). GTK # .NET için yüklenebilir bilgisayardaki Windows [GTK # yükleyici](http://www.mono-project.com/download/stable/#download-win).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Windows masaüstünde çalışacak GTK # uygulama eklemek için aşağıdaki yönergeleri izleyin:

1. Visual Studio 2017 ' çözüm adına sağ tıklayın **Çözüm Gezgini** ve **Ekle > Yeni proje...** .

2. İçinde **yeni proje** penceresinde, soldaki seçin **Visual C#** ve **Windows Klasik Masaüstü**. Proje türleri listesinden seçip **sınıf kitaplığı (.NET Framework)** ve emin **Framework** açılır, .NET Framework 4.7 en az olarak ayarlanır.

3. Proje ile için bir ad yazın bir **GTK** uzantısı, örneğin **GameOfLife.GTK**. ' I tıklatın **Gözat** düğmesini tıklatın, diğer platform içeren klasörü seçin projeleri ve tuşuna **Klasör Seç**. Bu çözümdeki diğer projeler ile aynı dizinde GTK proje sokar.

    ![Yeni bir GTK projesi eklemek](gtk-images/win/add-new-project.png "yeni bir GTK proje ekleyin")

    Tuşuna **Tamam** projesi oluşturmak için düğmesi.

4. İçinde **Çözüm Gezgini**yeni GTK projeye sağ tıklayın ve seçin **NuGet paketlerini Yönet**. Seçin **Gözat** arayın ve sekmesinde **Xamarin.Forms** 3.0 veya daha büyük.

    ![Xamarin.Forms NuGet paketini seçin](gtk-images/win/select-forms-nuget-package.png "Xamarin.Forms NuGet paketi seçin")

    Paketi seçin ve **yükleme** düğmesi.

5. Şimdi arama **Xamarin.Forms.Platform.GTK** 3.0 Paketi veya daha büyük.

    ![Xamarin.Forms.Platform.GTK NuGet paketini seçin](gtk-images/win/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet paketi seçin")

    Paketi seçin ve **yükleme** düğmesi.

6. İçinde **Çözüm Gezgini**, çözüm adına sağ tıklayın ve seçin **çözüm için NuGet paketlerini Yönet**. Seçin **güncelleştirme** sekmesi ve **Xamarin.Forms** paket. Tüm projeleri seçin ve bunları GTK projesi tarafından kullanılan aynı Xamarin.Forms sürüme güncelleştirin.

7. İçinde **Çözüm Gezgini**, sağ tıklayın **başvuruları** GTK projesinde. İçinde **başvuru Yöneticisi** iletişim kutusunda **projeleri** sola ve onay .NET standart ya da paylaşılan projeye bitişik onay kutusu:

    ![Paylaşılan bir proje başvurusu](gtk-images/win/reference-shared-project.png "paylaşılan proje başvurusu")

8. İçinde **başvuru Yöneticisi** iletişim, basın **Gözat** düğmesine tıklayın ve Gözat **C:\Program Files (x86)\GtkSharp\2.12\lib** klasörü ve seçin  **ATK sharp.dll**, **gdk sharp.dll**, **glade sharp.dll**, **sharp.dll glib**, **gtk dotnet.dll**, **gtk sharp.dll** dosyaları.

    ![GTK # kitaplıklarına](gtk-images/win/reference-gtk-libraries.png "GTK # Kitaplığı Başvurusu")

    Tuşuna **Tamam** düğmesi başvurular ekleyin.

9. GTK projeyi yeniden adlandırma **Class1.cs** için **Program.cs**.

10. GTK projesinde Düzenle **Program.cs** aşağıdaki kodu benzer dosyasını:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Bu kod GTK # ve Xamarin.Forms başlatır, bir uygulama penceresi oluşturur ve uygulamayı çalıştırır.

11. İçinde **Çözüm Gezgini**GTK projeye sağ tıklayın ve seçin **özellikleri**.

12. İçinde **özellikleri** penceresinde, seçin **uygulama** sekmesinde ve değiştirme **çıktı türü** aşağı açılan **Windows uygulaması**.

    ![Proje Çıktı türünü değiştirme](gtk-images/win/change-project-output-type.png "proje çıktı türünü değiştirme")

13. İçinde **Çözüm Gezgini**, WPF projesine sağ tıklatın ve **başlangıç projesi olarak ayarla**. Windows masaüstünde, Visual Studio hata ayıklayıcısı ile programı çalıştırmak için F5 tuşuna basın:

    ![GTK # yaşam oyunu](gtk-images/win/gtk-gameoflife.png "GTK # oyun ömrü")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac masaüstünde çalışacak GTK # uygulama eklemek için aşağıdaki yönergeleri izleyin:

1. Visual Studio'da Mac için Xamarin.Forms çözüm üzerinde sağ tıklatın ve seçin **Ekle > Yeni Proje Ekle...** .

2. İçinde **yeni proje** penceresi seçin **diğer > .NET > Gtk # 2.0 proje** ve basın **sonraki**.

3. Proje ile için bir ad yazın bir **GTK** uzantısı, örneğin **GameOfLife.GTK**ve basın **oluşturma**.

4. İçinde **çözüm paneli**, sağ tıklayın **paketleri > paketleri Ekle...**  GTK için proje ve Xamarin.Forms 3.0 yayın öncesi NuGet paketini ekleyin veya daha büyük.

    ![Xamarin.Forms NuGet paketini seçin](gtk-images/mac/select-forms-nuget-package.png "Xamarin.Forms NuGet paketi seçin")

5. İçinde **çözüm paneli**, sağ tıklayın **paketleri > paketleri Ekle...**  GTK için proje ve Xamarin.Forms.Platform.GTK 3.0 yayın öncesi NuGet paketini ekleyin veya daha büyük.

    ![Xamarin.Forms.Platform.GTK NuGet paketini seçin](gtk-images/mac/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet paketi seçin")

6. Diğer platform projeleri GTK projesi tarafından kullanılan aynı Xamarin.Forms sürümünü kullanacak şekilde güncelleştirin.

7. İçinde **çözüm paneli**, sağ tıklayın **başvuruları > başvurular Düzenle...**  GTK için proje ve (.NET standart ya da paylaşılan Proje) Xamarin.Forms projesine bir başvuru ekleyin.

    ![Paylaşılan bir proje başvurusu](gtk-images/mac/reference-shared-project.png "paylaşılan proje başvurusu")

8. Düzen **Program.cs** aşağıdaki kod, BT'nin benzer şekilde GTK proje dosyası:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Bu kod GTK # ve Xamarin.Forms başlatır, bir uygulama penceresi oluşturur ve uygulamayı çalıştırır.

9. İçinde **çözüm paneli**, GTK projesine sağ tıklatın ve **başlangıç projesi olarak ayarla**.

10. Mac araç için Visual Studio basın **Başlat** düğmesini (Oynat düğmesini benzer üçgen düğmesi) uygulamayı başlatmak için.

    ![GTK # yaşam oyunu](gtk-images/mac/gtk-gameoflife.png "GTK # oyun ömrü")

-----

## <a name="next-steps"></a>Sonraki Adımlar

### <a name="platform-specifics"></a>Platform özellikleri

Xamarin.Forms uygulamanızı XAML veya kod çalıştığı hangi platformu belirleyebilirsiniz. Bu program özelliklerini GTK # üzerinde çalışırken değiştirmenize olanak sağlar. Kod içinde değerini karşılaştırın `Device.RuntimePlatform` ile `Device.GTK` (hangi "GTK" dizesi eşittir) sabiti. Bir eşleşme varsa, uygulamanın GTK # üzerinde çalışıyor.

XAML'de, kullandığınız `OnPlatform` etiketi platformuna özel bir özellik değeri seçin:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="GTK" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="application-icon"></a>Uygulama simgesi

Başlangıçta uygulama simgesini ayarlayabilirsiniz:

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Temalar

Çok çeşitli temalar GTK # için kullanılabilir ve Xamarin.Forms uygulaması kullanılabilir:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Yerel formlar

Yerel Forms verir Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-GTK # projeleri dahil olmak üzere yerel projeler tarafından tüketilen sayfalarına türetilmiş. Bu örneği oluşturarak gerçekleştirilebilir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-türetilen sayfası ve yerel GTK # türü kullanarak dönüştürme `CreateContainer` genişletme yöntemi:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Yerel biçimleri hakkında daha fazla bilgi için bkz: [yerel Forms](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Sorunlar

Her şeyin üretim hazır olduğunu beklemelisiniz şekilde bu bir önizleme sürümü. Geçerli uygulama durumu için bkz: [durum](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)ve geçerli bilinen sorunlar için bkz: [bekleyen & bilinen sorunları](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
