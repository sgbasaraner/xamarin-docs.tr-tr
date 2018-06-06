---
title: 'F # ile programlama UrhoSharp'
description: 'Bu belge, F # Mac için Visual Studio kullanarak basit Merhaba Dünya UrhoSharp uygulaması oluşturmak açıklar'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 64d69de70d6bc6f23b9907b498622b00c42b6f50
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783278"
---
# <a name="programming-urhosharp-with-f"></a>F # ile programlama UrhoSharp

F # aynı kitaplıkları ve C# programcıları tarafından kullanılan kavramları kullanarak ile UrhoSharp programlanabilir. [Kullanarak UrhoSharp](~/graphics-games/urhosharp/using.md) makale UrhoSharp altyapısı genel bir bakış sunar ve bu makalede önce okumanız gerekir.

C++ dünyada kaynaklanan birçok kitaplıklar gibi birçok UrhoSharp işlevler Boole değerlerini veya başarı veya başarısızlık gösteren tamsayı döndürür. Kullanmanız gereken `|> ignore` bu değerleri yoksaymak için.

[Örnek program](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) bir "Hello World" den F # UrhoSharp içindir.

## <a name="creating-an-empty-project"></a>Boş bir proje oluşturma

Hiçbir F # şablonlar UrhoSharp için henüz kullanılabilir, böylece kendi UrhoSharp projesi oluşturmak her iki Başlarken yapabilecekleriniz [örnek](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) veya şu adımları izleyin:

1. Mac için Visual Studio'da yeni bir oluşturma **çözüm**. Seçin **iOS > Uygulama > Single View uygulaması** seçip **F #** uygulama dili olarak. 
1. Silme **Main.storyboard** dosya. Açık **Info.plist** dosya ve **iPhone / iPod dağıtım bilgileri** bölmesinde, silmek `Main` içinde dize **ana arabirimi** açılır.
1. Silme **ViewController.fs** de dosya.

## <a name="building-hello-world-in-urho"></a>Urho yapı Hello World

Şimdi, oyunun sınıfları tanımlama başlamaya hazırsınız. En az bir alt sınıfı tanımlamanız gerekir `Urho.Application` ve geçersiz kılma kendi `Start` yöntemi. Bu dosyayı oluşturmak için F # projeye sağ tıklayın, seçin **yeni dosya Ekle...**  ve boş bir F # sınıfı projenize ekleyin. Yeni dosyayı projenize dosyaların listesini sonuna eklenir ancak göründüğü şekilde sürükleyin gerekir *önce* içinde kullanılan **AppDelegate.fs**.

1. Urho NuGet paketine bir başvuru ekleyin.
1. Varolan bir Urho projeden (büyük) dizinleri kopyalamak **CoreData /** ve **veri /** projenizin içine **kaynakları /** dizin. F # projenize, sağ tıklayın **kaynakları** klasörü ve kullanım **Ekle / varolan klasörü Ekle** tüm bu dosyaların projenize eklemek için.

Proje yapısı gibi görünmelidir:

![](fsharp-images/solutionpane.png "Proje yapısı gibi görünmelidir")

Bir alt türü, yeni oluşturulan sınıf tanımlama `Urho.Application` ve geçersiz kılma kendi `Start` yöntemi:

```csharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

Kodu oldukça basittir. Kullandığı `Urho.Gui.Text` merkezi hizalı dize belirli bir yazı tipi ve renk boyutu ile görüntülenecek sınıf. 

Ancak, bu kod çalıştırmadan önce UrhoSharp başlatılmalıdır. 

AppDelegate.fs dosyasını açın ve değiştirme `FinishedLaunching` yöntemini aşağıdaki şekilde:

```csharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

`ApplicationOptions.Default` Yatay modu uygulama için varsayılan seçenekler sağlar. Bunlar geçirmek `ApplicationOptions` için varsayılan oluşturucu için `Application` bir alt kümesi (tanımladığınız unutmayın `HelloWorld` sınıfı, satır `inherit Application(o)` temel sınıf oluşturucuyu çağırır). 

`Run` Yöntemi, `Application` program başlatır. Döndürme olarak tanımlanan bir `int`, hangi yöneltilen için `ignore`. 

Sonuç programı gibi görünmelidir:

![](fsharp-images/helloworldfsharp.png "Sonuç programı gibi görünmelidir")








## <a name="related-links"></a>İlgili bağlantılar

- [Github'da (örnek) göz atın](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
