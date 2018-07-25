---
title: 'F # ile UrhoSharp programlama'
description: 'Bu belge F # Mac için Visual Studio kullanarak basit bir hello world UrhoSharp uygulamasının nasıl oluşturulacağını açıklar.'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 430c4eca7c6dbd7107692246b70ff93bafa44d01
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241840"
---
# <a name="programming-urhosharp-with-f"></a>F # ile UrhoSharp programlama

UrhoSharp ile F # aynı kitaplıkları ve C# programcıları tarafından kullanılan kavramları kullanarak programlanabilir. [UrhoSharp kullanma](~/graphics-games/urhosharp/using.md) makale UrhoSharp altyapısı genel bir bakış sağlar ve bu makalede önce okunmalıdır.

C++ dünyada kaynaklanan birçok kitaplıkları gibi birçok UrhoSharp İşlevler, Boole değerlerini veya başarısı veya başarısızlığı gösteren tamsayı döndürür. Kullanmanız gereken `|> ignore` bu değerleri yok sayılacak.

[Örnek program](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) UrhoSharp F #'dan bir "Merhaba Dünya" içindir.

## <a name="creating-an-empty-project"></a>Boş bir proje oluşturma

Hiçbir F # şablonlar UrhoSharp için henüz kullanılabilir, böylece kendi UrhoSharp projesi oluşturmak ya başlangıç ile yapabilecekleriniz [örnek](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) ya da şu adımları izleyin:

1. Mac için Visual Studio'dan yeni bir oluşturma **çözüm**. Seçin **iOS > Uygulama > tek görünüm uygulaması** seçip **F #** uygulama dili olarak. 
1. Silme **Main.storyboard** dosya. Açık **Info.plist** dosya ve **iPhone / iPod dağıtım bilgisi** bölmesinde Sil `Main` içinde dize **ana arabirimi** açılır.
1. Silme **ViewController.fs** de dosya.

## <a name="building-hello-world-in-urho"></a>Yapı Urho dilinde Merhaba Dünya

Artık, oyun sınıfları tanımlama başlamak hazırsınız. En az bir alt sınıfı tanımlamanız gerekir `Urho.Application` ve geçersiz kılma kendi `Start` yöntemi. Bu dosyayı oluşturmak için F # projenize sağ tıklayın, **yeni dosya Ekle...**  ve boş bir F # sınıfı projenize ekleyin. Yeni dosyayı projenize dosyaların listesinin sonuna eklenir, ancak bunu görünmesi sürüklemeniz gerekir *önce* kullanılır **AppDelegate.fs**.

1. Urho NuGet paketine başvuru ekleyin.
1. Mevcut bir Urho projeden (büyük) dizinleri kopyalama **CoreData /** ve **veri /** projenizin içine **kaynakları /** dizin. F # projenizde, sağ **kaynakları** klasörü ve kullanım **Ekle / mevcut klasörü Ekle** tüm bu dosyalar, projenize eklemek için.

Proje yapısı gibi görünmelidir:

![](fsharp-images/solutionpane.png "Proje yapısı şuna benzer görünmelidir")

Yeni oluşturulan sınıfınıza öğesinin alt öğesi tanımlamak `Urho.Application` ve geçersiz kılma kendi `Start` yöntemi:

```fsharp
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

Kod çok basittir. Kullandığı `Urho.Gui.Text` belirli bir yazı tipi ve renk boyutu ile merkezi hizalanmış bir dize görüntülemek için sınıf. 

Ancak, bu kodu çalıştırmadan önce UrhoSharp başlatılmalıdır. 

AppDelegate.fs dosya açıp değiştirdiğinizde `FinishedLaunching` yöntemini aşağıdaki şekilde:

```fsharp
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

`ApplicationOptions.Default` Yatay modda çalışan bir uygulama için varsayılan seçenekleri sağlar. Bunlar geçirmek `ApplicationOptions` için varsayılan oluşturucu için `Application` alt (tanımladığınız unutmayın `HelloWorld` sınıfı, satır `inherit Application(o)` temel sınıf oluşturucusunu çağırır). 

`Run` Yöntemi, `Application` program başlatır. Döndüren olarak tanımlanan bir `int`, hangi ayrıştırılabileceği `ignore`. 

Sonuç programı gibi görünmelidir:

![](fsharp-images/helloworldfsharp.png "Sonuç programı gibi görünmelidir")








## <a name="related-links"></a>İlgili bağlantılar

- [(Örnek) Github'da göz atın](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
