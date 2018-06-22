---
title: Visual Basic.NET kullanarak Xamarin.Forms
description: Xamarin.Forms PCL proje şablonu, etkili bir şekilde VB.NET kullanarak platformlar arası mobil uygulamalar oluşturmanıza olanak sağlayan ana derleme için Visual Basic kullanmak için değiştirilebilir.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b858e26de95d2abbc23917b1ed5a1de65105cd8d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917869"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Visual Basic.NET kullanarak Xamarin.Forms

Xamarin Visual Basic doğrudan desteklemez - C# Xamarin.Forms PCL çözümü oluşturun ve sonra ortak kod PCL projesini Visual Basic ile değiştirmek için bu sayfadaki yönergeleri izleyin.

[![](xamarin-forms-images/hero-sml.png "Xamarin.Forms PCL çözümü oluşturun ve sonra ortak kod PCL projesini Visual Basic ile değiştirin")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Visual Basic ile program için Windows Visual Studio kullanmanız gerekir.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Visual Basic kılavuza Xamarin.Forms

Visual Basic kullanan basit bir Xamarin.Forms projesi oluşturmak için aşağıdaki adımları izleyin:

1. Yeni bir *Xamarin.Forms C#* taşınabilir sınıf kitaplıkları (PCL) kullanan çözümü.
Git **Dosya > Yeni proje** ve **yeni proje** penceresi gidin **yüklü > şablonları > Visual C# > Çapraz Platform** ardından  **Çapraz platform uygulama (Xamarin.Forms veya yerel) > Xamarin.Forms**.

2. Çözüm üzerinde sağ tıklatın ve **Ekle > Yeni proje**.

3. Seçin **Visual Basic > sınıf kitaplığı (taşınabilir)** proje türü:

   [![](xamarin-forms-images/add-vb-2-sml.png "Yeni taşınabilir sınıf kitaplığı proje ekleyin")](xamarin-forms-images/add-vb-2.png#lightbox)

4. Doğru PCL profili yapılandırmak için gösterildiği gibi platformları seçin (Xamarin.iOS ve Xamarin.Android eklediğinizden emin olun):

   ![](xamarin-forms-images/add-vb-3-sml.png "Desteklemek için platformları seçin")

5. Visual Basic projeye sağ tıklayın ve seçin **özellikleri**, ardından değiştirme **varsayılan ad alanı** eşleşen varolan C# projeleri:

   ![](xamarin-forms-images/add-vb-4s-sml.png "Visual Basic kök ad alanı Xamarin.Forms uygulaması eşleştiğinden emin olun")

6. Yeni Visual Basic projeye sağ tıklayın ve seçin **Nuget paketlerini Yönet**, ardından yükleyin **Xamarin.Forms** ve Paket Yöneticisi penceresini kapatın.

   [![](xamarin-forms-images/add-vb-4-sml.png "Formlar ve Paket Yöneticisi penceresini kapatın")](xamarin-forms-images/add-vb-4.png#lightbox)

7. Varsayılan yeniden adlandırma **Class1** dosya *ve* sınıfının `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "Varsayılan Class1 dosya ve sınıfı için uygulama yeniden adlandırın")](xamarin-forms-images/add-vb-5.png#lightbox)

8. Aşağıdaki kodu yapıştırın **App.vb** Xamarin.Forms uygulamanızın başlangıç noktası olacak dosya. Dahil etmeyi unutmayın `Imports Xamarin.Forms` ve ekleme `Inherits Application` sınıfı için:

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

9. Şimdi biz iOS ve Android projeleri yeni Visual Basic projesinde noktası gerekir.
Sağ **başvuruları** düğümünü iOS ve Android projeleri açmak için **başvuru Yöneticisi**. Kaldırma onay onay VB taşınabilir kitaplığı ve C# taşınabilir kitaplığı (yok unutursanız, hem iOS hem de Android projeleri için bunu).

   [![](xamarin-forms-images/add-vb-8-sml.png "Eski proje başvurusu kaldırmak için Visual Basic Başvurusu Ekle")](xamarin-forms-images/add-vb-8.png#lightbox)

10. C# taşınabilir projeyi silin. Yeni Ekle **.vb** oluşturmak için dosyaları out Xamarin.Forms uygulaması. Yeni bir şablon `ContentPage`s Visual Basic'te aşağıda gösterilmektedir:

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

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Xamarin.Forms'de Visual Basic sınırlamaları

Üzerinde belirtildiği gibi [taşınabilir Visual Basic.NET sayfa](~/cross-platform/platform/visual-basic/index.md), Xamarin Visual Basic dil desteği yok. Başka bir deyişle, Visual Basic burada kullanabileceğiniz bazı sınırlamalar vardır:

 - Visual Basic'te özel Oluşturucu yazılamıyorsa, bunlar C# ' ta yerel platform projelerinde yazılmış olmalıdır.

 - Bağımlılık hizmet uygulamalarını Visual Basic'te yazılamıyorsa, bunlar C# ' ta yerel platform projelerinde yazılmış olmalıdır.

 - Visual Basic projesinde XAML sayfaları eklenemez - arka plan kodu Oluşturucu yalnızca C# oluşturabilirsiniz. XAML ayrı, başvurulan, C# taşınabilir sınıf kitaplığa ve Visual Basic modeller aracılığıyla XAML dosyaları doldurmak için veri bağlamasını kullanmak da mümkündür (Bunun bir örneği yer aldığı [örnek](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Xamarin Visual Basic.NET dil desteklemez.

## <a name="related-links"></a>İlgili bağlantılar

- [XamarinFormsVB (örnek)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [.NET Framework (Microsoft) ile platformlar arası geliştirme](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
