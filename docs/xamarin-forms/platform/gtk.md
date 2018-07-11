---
title: 'GTK # Platform Kurulumu'
description: 'Xamarin.Forms GTK # platform için Önizleme desteği sunuyor'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 7f68b7c8affc11b50bdb4a2fc9589f8dcbfb45ec
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830486"
---
# <a name="gtk-platform-setup"></a>GTK # Platform Kurulumu

![Önizleme](~/media/shared/preview.png)

Xamarin.Forms GTK # uygulamaları için Önizleme desteği sunuyor. GTK # GTK + Araç Seti ve GNOME kitaplıklarını çeşitli bağlanan bir grafik kullanıcı arabirimi araç seti olan Mono ve .NET kullanarak tamamen yerel GNONE grafik uygulamaları geliştirilmesini sağlar. Bu makalede, bir GTK # proje için Xamarin.Forms çözümü eklemek gösterilmektedir.

Başlat, yeni bir Xamarin.Forms çözümü oluşturun veya varolan bir Xamarin.Forms çözümü örneğin kullanın önce [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/).

> [!NOTE]
> Bu makalede bir GTK # uygulaması VS2017 hem Visual Studio'da Xamarin.Forms çözümü için Mac için ekleme odaklanmakla birlikte, bu da içinde gerçekleştirilebilir [MonoDevelop](http://www.monodevelop.com/) Linux için.

## <a name="adding-a-gtk-app"></a>Bir GTK # uygulaması ekleme

GTK # MacOS ve Linux'ın bir parçası olarak yüklenir [Mono](http://www.mono-project.com/download/stable/). GTK # .NET için yüklenebilir ile Windows [GTK # yükleyici](http://www.mono-project.com/download/stable/#download-win).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Windows masaüstünde çalışır bir GTK # uygulaması eklemek için aşağıdaki yönergeleri izleyin:

1. Visual Studio 2017'de çözüm adına sağ tıklayın **Çözüm Gezgini** ve **Ekle > Yeni proje...** .

2. İçinde **yeni proje** penceresinin sol Seç **Visual C#** ve **Windows Klasik Masaüstü**. Proje türleri listesinde seçin **sınıf kitaplığı (.NET Framework)**, olduğundan emin olun **Framework** açılan en az .NET Framework 4.7 ayarlayın.

3. Proje için bir ad yazın bir **GTK** uzantısı, örneğin **GameOfLife.GTK**. Tıklayın **Gözat** düğmesi, diğer platform içeren klasörü seçin projeleri ve ENTER tuşuna **Klasör Seç**. Bu çözümdeki diğer projelerin ile aynı dizinde GTK proje koyacaktır.

    ![Yeni bir GTK projesi eklemek](gtk-images/win/add-new-project.png "yeni bir GTK proje ekleyin")

    Tuşuna **Tamam** projeyi oluşturmak için.

4. İçinde **Çözüm Gezgini**yeni GTK projeye sağ tıklayın ve seçin **NuGet paketlerini Yönet**. Seçin **Gözat** sekmesinde ve arama **Xamarin.Forms** 3.0 veya üstü.

    ![Xamarin.Forms NuGet paketini seçin](gtk-images/win/select-forms-nuget-package.png "Xamarin.Forms NuGet paketini seçin")

    Paketi seçin ve tıklayın **yükleme** düğmesi.

5. Artık arama **Xamarin.Forms.Platform.GTK** 3.0 Paketi veya büyük.

    ![Xamarin.Forms.Platform.GTK NuGet paketini seçin](gtk-images/win/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet paketini seçin")

    Paketi seçin ve tıklayın **yükleme** düğmesi.

6. İçinde **Çözüm Gezgini**, çözüm adına sağ tıklayıp **çözüm için NuGet paketlerini Yönet**. Seçin **güncelleştirme** sekmesi ve **Xamarin.Forms** paket. Tüm projeleri seçin ve bunları GTK proje tarafından kullanılan aynı Xamarin.Forms sürüme güncelleştirin.

7. İçinde **Çözüm Gezgini**, sağ **başvuruları** GTK projedeki. İçinde **başvuru Yöneticisi** iletişim kutusunda **projeleri** sola ve .NET Standard ya da paylaşılan proje bitişik onay kutusu denetimi:

    ![Paylaşılan proje başvurusu](gtk-images/win/reference-shared-project.png "paylaşılan proje başvurusu")

8. İçinde **başvuru Yöneticisi** iletişim kutusunda, ENTER tuşuna **Gözat** göz atın ve düğme **C:\Program Files (x86)\GtkSharp\2.12\lib** klasörü ve seçin  **ATK sharp.dll**, **gdk sharp.dll**, **glade sharp.dll**, **sharp.dll glib**, **gtk-dotnet.dll**, **gtk-sharp.dll** dosyaları.

    ![GTK # kitaplıklarına](gtk-images/win/reference-gtk-libraries.png "GTK # kitaplıklarına")

    Tuşuna **Tamam** başvuruları eklemek için Ekle düğmesine.

9. GTK projesinde yeniden adlandırın **Class1.cs** için **Program.cs**.

10. GTK projesinde Düzenle **Program.cs** aşağıdaki koda benzer dosyasını:

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

12. İçinde **özellikleri** penceresinde **uygulama** sekmesini ve değiştirme **çıkış türü** açılan **Windows uygulama**.

    ![Proje Çıktı türünü değiştirme](gtk-images/win/change-project-output-type.png "proje çıktı türünü değiştirme")

13. İçinde **Çözüm Gezgini**, WPF projesini sağ tıklatın ve seçin **başlangıç projesi olarak ayarla**. Program, Windows masaüstünde, Visual Studio hata ayıklayıcısı ile çalıştırmak için F5 tuşuna basın:

    ![GTK # oyun ömrü](gtk-images/win/gtk-gameoflife.png "GTK # yaşam oyunu")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac masaüstünde çalışır bir GTK # uygulaması eklemek için aşağıdaki yönergeleri izleyin:

1. Mac için Visual Studio, Xamarin.Forms çözümü sağ tıklatın ve seçin **Ekle > Yeni Proje Ekle...** .

2. İçinde **yeni proje** pencere **diğer > .NET > Gtk # 2.0 projesi** basın **sonraki**.

3. Proje için bir ad yazın bir **GTK** uzantısı, örneğin **GameOfLife.GTK**basın **Oluştur**.

4. İçinde **çözüm bölmesi**, sağ **paketleri > paketleri Ekle...**  GTK için proje ve Xamarin.Forms 3.0 yayın öncesi NuGet paketini ekleyin veya daha büyük.

    ![Xamarin.Forms NuGet paketini seçin](gtk-images/mac/select-forms-nuget-package.png "Xamarin.Forms NuGet paketini seçin")

5. İçinde **çözüm bölmesi**, sağ **paketleri > paketleri Ekle...**  GTK için proje ve Xamarin.Forms.Platform.GTK 3.0 yayın öncesi NuGet paketini ekleyin veya daha büyük.

    ![Xamarin.Forms.Platform.GTK NuGet paketini seçin](gtk-images/mac/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet paketini seçin")

6. Diğer platform projelerini GTK proje tarafından kullanılan aynı Xamarin.Forms sürümünü kullanacak şekilde güncelleştirin.

7. İçinde **çözüm bölmesi**, sağ **başvuruları > başvuruları Düzenle...**  GTK için proje ve Xamarin.Forms projesi (.NET Standard ya da paylaşılan Proje) için bir başvuru ekleyin.

    ![Paylaşılan proje başvurusu](gtk-images/mac/reference-shared-project.png "paylaşılan proje başvurusu")

8. Düzen **Program.cs** olan aşağıdaki koda benzer şekilde GTK proje dosyası:

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

9. İçinde **çözüm bölmesi**GTK projeye sağ tıklayın ve seçin **başlangıç projesi olarak ayarla**.

10. Araç Mac için Visual Studio'da basın **Başlat** düğmesine (Oynat düğmesine benzeyen üçgen düğme) uygulamasını başlatmak için.

    ![GTK # oyun ömrü](gtk-images/mac/gtk-gameoflife.png "GTK # yaşam oyunu")

-----

## <a name="next-steps"></a>Sonraki Adımlar

### <a name="platform-specifics"></a>Platform özellikleri

Xamarin.Forms uygulamanızı XAML veya kod çalıştığı platformdan belirleyebilirsiniz. Bu GTK # üzerinde çalıştığı sırada, program özelliklerini değiştirmenizi sağlar. Kod içinde değerini karşılaştırın `Device.RuntimePlatform` ile `Device.GTK` (hangi "GTK" dize eşittir) sabiti. Bir eşleşme varsa, uygulama GTK # üzerinde çalışıyor.

XAML içinde kullanabileceğiniz `OnPlatform` platforma özgü bir özellik değer seçmek için etiketi:

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

Uygulama simgesi, başlangıçta ayarlayabilirsiniz:

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Temalar

GTK #'ta kullanılabilen çok çeşitli temaları vardır ve bir Xamarin.Forms uygulamasından kullanılabilir:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Yerel formlar

Yerel formlar sağlayan Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-GTK # projeleri dahil olmak üzere yerel projeleri tarafından kullanılacak sayfaları türetilmiş. Bu bir örneğini oluşturarak gerçekleştirilebilir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-türetilmiş sayfası ve yerel GTK # türü kullanarak için dönüştürme `CreateContainer` genişletme yöntemi:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Yerel formlar hakkında daha fazla bilgi için bkz: [yerel formlar](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Sorunlar

Her şey üretime hazır olduğunu beklemelisiniz. Bu bir önizleme olduğundan. Geçerli uygulama durumunu görmek [durumu](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)ve geçerli bilinen sorunlar için bkz. [bekleyen ve bilinen sorunları](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
