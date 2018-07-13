---
title: Visual Basic.NET kullanarak Xamarin.Forms
description: Xamarin.Forms PCL proje şablonu, Visual Basic etkili bir şekilde VB.NET kullanarak platformlar arası mobil uygulamalar oluşturmanıza olanak tanıyan ana derleme için kullanılacak değiştirilebilir.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 256d5c81475be095c8fa0ab0408cbcf673c6b301
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997090"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Visual Basic.NET kullanarak Xamarin.Forms

Xamarin Visual Basic doğrudan desteklemez,-C# Xamarin.Forms PCL çözümü oluşturun ve sonra ortak kod PCL projesine Visual Basic ile değiştirmek için bu sayfadaki yönergeleri izleyin.

[![](xamarin-forms-images/hero-sml.png "Xamarin.Forms PCL çözümü oluşturun ve sonra ortak kod PCL projesine Visual Basic ile değiştirin")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Visual Basic ile programa Windows üzerinde Visual Studio kullanmanız gerekir.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Xamarin.Forms ile Visual Basic Kılavuzu

Visual Basic kullanan basit bir Xamarin.Forms projesi oluşturmak için aşağıdaki adımları izleyin:

1. Yeni bir *Xamarin.Forms C#* taşınabilir sınıf kitaplığı (PCL) kullanan bir çözüm.
Git **Dosya > Yeni proje** ve **yeni proje** penceresi gidin **yüklü > şablonları > Visual C# > Çoklu Platform** ardından  **Çapraz platform uygulaması (Xamarin.Forms veya yerel) > Xamarin.Forms**.

2. Çözüme sağ tıklayın ve **Ekle > Yeni proje**.

3. Seçin **Visual Basic > sınıf kitaplığı (taşınabilir)** proje türü:

   [![](xamarin-forms-images/add-vb-2-sml.png "Yeni bir taşınabilir sınıf kitaplığı projesi ekleme")](xamarin-forms-images/add-vb-2.png#lightbox)

4. Doğru PCL profilinin yapılandırmak için gösterildiği gibi platformları seçin (Xamarin.iOS ve Xamarin.Android eklediğinizden emin olun):

   ![](xamarin-forms-images/add-vb-3-sml.png "Desteklemek için platformları seçin")

5. Visual Basic proje üzerinde sağ tıklatın ve seçin **özellikleri**, ardından değiştirme **varsayılan ad alanı** eşleşen mevcut C# projeleri:

   ![](xamarin-forms-images/add-vb-4s-sml.png "Visual Basic kök ad eşleşen Xamarin.Forms uygulama emin olun.")

6. Yeni Visual Basic proje üzerinde sağ tıklatın ve seçin **Nuget paketlerini Yönet**, yüklemeyi **Xamarin.Forms** ve Paket Yöneticisi penceresini kapatın.

   [![](xamarin-forms-images/add-vb-4-sml.png "Formlar ve Paket Yöneticisi penceresini kapatın")](xamarin-forms-images/add-vb-4.png#lightbox)

7. Varsayılan Yeniden Adlandır **Class1** dosya *ve* sınıfının `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "Sınıf ve varsayılan Class1 dosyası için uygulamayı yeniden adlandırma")](xamarin-forms-images/add-vb-5.png#lightbox)

8. Aşağıdaki kodu yapıştırın **App.vb** dosyasını Xamarin.Forms uygulamanızın başlangıç noktası olur. Dahil etmeyi unutmayın `Imports Xamarin.Forms` ve ekleme `Inherits Application` sınıf:

    ```vb 
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Welcome to Xamarin.Forms with Visual Basic.NET"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Dim page = New ContentPage
            page.Content = stack
            MainPage = page

        End Sub

    End Class
    ```

9. Artık iOS ve Android projeleri için yeni bir Visual Basic projesi işaret edecek şekilde ihtiyacımız var.
Sağ **başvuruları** iOS ve Android projeleri açmak için düğümü **başvuru Yöneticisi**. Kaldırma değer çizgisi değer çizgisi VB taşınabilir kitaplık ve C# taşınabilir kitaplığı (yoksa unutursanız, iOS ve Android projeleri için bunu).

   [![](xamarin-forms-images/add-vb-8-sml.png "Eski proje başvurusu kaldırmak için Visual Basic Başvurusu Ekle")](xamarin-forms-images/add-vb-8.png#lightbox)

10. C# taşınabilir proje silin. Yeni Ekle **.vb** dosyalarını derlemek için Xamarin.Forms uygulamanızı kullanıma al. Yeni bir şablon `ContentPage`Visual Basic'te s aşağıda gösterilmektedir:

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Visual Basic ContentPage"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Content = stack
        End Sub
    End Class
    ```

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Xamarin.Forms içinde Visual Basic sınırlamaları

Belirtilen şekilde [taşınabilir bir Visual Basic.NET sayfa](~/cross-platform/platform/visual-basic/index.md), Xamarin, Visual Basic dili desteklemez. Başka bir deyişle, Visual Basic kullanabileceğiniz bazı sınırlamalar vardır:

 - Özel oluşturucular Visual Basic'te yazılamaz, bunlar C# dilinde yerel platform projeleri için yazılmış olmalıdır.

 - Bağımlılık hizmet uygulamaları, Visual Basic'te yazılamaz, bunlar C# dilinde yerel platform projeleri için yazılmış olmalıdır.

 - Visual Basic projesinde XAML sayfaları dahil edilemez - arka plan kod Oluşturucu yalnızca C# oluşturabilirsiniz. XAML ayrı, başvurulan, C# taşınabilir sınıf kitaplığa ve Visual Basic modelleri aracılığıyla XAML dosyaları doldurmak için veri bağlamasını kullanmak da mümkündür (buna örnek olarak yer aldığı [örnek](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Xamarin, Visual Basic.NET dil desteklemez.

## <a name="related-links"></a>İlgili bağlantılar

- [XamarinFormsVB (örnek)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [.NET Framework ile platformlar arası geliştirme](https://docs.microsoft.com/dotnet/standard/cross-platform/)
