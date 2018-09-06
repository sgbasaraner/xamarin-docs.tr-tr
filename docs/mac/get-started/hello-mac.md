---
title: Merhaba, Mac – gözden geçirme
description: Bu belge, bir Xamarin.Mac uygulamasının nasıl oluşturulacağını gösterir ve Visual Studio Mac, Xcode ve arabirim Oluşturucu tanıtır. Çıkışlar ve eylemleri aracılığıyla koda sunan UI denetimleri açıklar ve oluşturun, çalıştırın ve bir Xamarin.Mac uygulamasını test etme göstermektedir.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 37D0E9E6-979B-7069-B3BE-C5F0AF99BA72
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: 81a15f85c3b3b10525e2eb4966900edc95224fe0
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780649"
---
# <a name="hello-mac--walkthrough"></a>Merhaba, Mac – gözden geçirme

C# ve .NET geliştirme sırasında kullanılan arabirim denetimleri ve aynı OS X kitaplıkları kullanarak tamamen yerel Mac uygulamaları geliştirilmesini sağlar Xamarin.Mac *Objective-C* ve *Xcode*. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Geliştirici Xcode'un kullanabilirsiniz _arabirim Oluşturucu_ bir uygulamanın kullanıcı arabirimleri oluşturun (veya isteğe bağlı olarak bunları doğrudan C# kodu oluşturmak için).

Ayrıca, C# ve .NET dillerinde yazılan Xamarin.Mac uygulamaları olduğundan, genel arka uç kodu Xamarin.iOS ve Xamarin.Android mobil uygulamalarla paylaşılabilir; her platformda tüm yerel deneyimi sunarken.

Bu makalede Xamarin.Mac, basit bir oluşturma işleminde size walking tarafından Mac ve Xcode'un arabirim Oluşturucu için Visual Studio kullanarak Mac uygulaması oluşturmak için gereken temel kavramları başlatacaktır **Merhaba, Mac** sayar uygulama bir düğmeye tıkladı:

[![](hello-mac-images/run02.png "Merhaba, çalıştıran Mac uygulaması örneği")](hello-mac-images/run02.png#lightbox)

Aşağıdaki kavramlar ele alınacak:

-  **Mac için Visual Studio** – ve onunla Xamarin.Mac uygulamaları oluşturmak Mac için Visual Studio'ya giriş.
-  **Bir Xamarin.Mac uygulamasını anatomisi** – ne bir Xamarin.Mac uygulamasını oluşur.
-  **Xcode'un arabirim Oluşturucu** : Xcode kullanmak için bir uygulamanın kullanıcı arabirimini tanımlamak için arabirim Oluşturucu kullanıcının nasıl.
-  **Çıkışlar ve eylemleri** – kullanıcı arabirimi denetimleri'kurmak kablo çıkışlar ve Eylemler kullanma.
-  **Dağıtım/test'in** – çalıştırın ve bir Xamarin.Mac uygulamasını test etme.

## <a name="requirements"></a>Gereksinimler

En yeni macOS API'leri kullanarak Xamarin.Mac uygulamaları geliştirmek için ihtiyacınız olacak:

- Bir Mac bilgisayarda çalışan macOS High Sierra (10.13) veya üzeri.
- [Xcode 9 veya üzeri](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
- En son sürümünü [Xamarin.Mac ve Mac için Visual Studio](https://docs.microsoft.com/visualstudio/mac/installation).

Xamarin.Mac ile oluşturulmuş bir uygulama çalıştırmak için ihtiyacınız olacak:

- Mac OS X 10.7 veya üzeri çalıştıran bir Mac bilgisayara.

> [!WARNING]
> Yaklaşan Xamarin.Mac 4.8 sürümü yalnızca macOS 10.9 veya sonraki sürümleri destekleyecektir.
> Xamarin.Mac’in önceki sürümleri macOS 10.7 veya sonraki sürümleri destekler ancak bu eski macOS sürümleri TLS 1.2’yi destekleyecek yeterli TLS altyapısını barındırmaz. macOS 10.7 veya macOS 10.8’i hedeflemek için Xamarin.Mac 4.6 veya önceki sürümleri kullanın.

## <a name="starting-a-new-xamarinmac-app-in-visual-studio-for-mac"></a>Mac için Visual Studio'da yeni bir Xamarin.Mac uygulamasını başlatarak

Yukarıda belirtildiği gibi bu kılavuz olarak adlandırılan bir Mac uygulaması oluşturma adımlarında size yol gösterecektir `Hello_Mac` ana pencerenin, tek bir düğme ve etiketi ekler. Düğmeye tıklandığında, etiket, tıklanan sayısını görüntüler.

Başlamak için aşağıdakileri yapın:

1. Mac için Visual Studio'yu başlatın:

    [![](hello-mac-images/setup01.png "Ana Visual Studio Mac arabirimi")](hello-mac-images/setup01.png#lightbox)

2. Tıklayarak **yeni çözüm...**  bağlantı açmak için ekranın sol üst köşesinde **yeni proje** iletişim kutusunda:

    [![](hello-mac-images/setup03.png "Mac için Visual Studio'da yeni bir çözüm oluşturma")](hello-mac-images/setup02.png#lightbox)

3. Seçin **Mac** > **uygulama** > **Cocoa uygulaması** tıklatıp **sonraki** düğmesi:

    [![](hello-mac-images/setup03.png "Cocoa uygulaması seçme")](hello-mac-images/setup03.png#lightbox)

4. Girin `Hello_Mac` için **uygulama adı**ve varsayılan her şey tutun. Tıklayın **sonraki**:

    [![](hello-mac-images/setup05.png "Uygulamanın adını ayarlama")](hello-mac-images/setup05.png#lightbox)

4. Bir çözüm oluşturma birkaç farklı projelerde barındırmak zaman, geliştirici farklı bir ayarlamak isteyebileceğiniz **çözüm adı** burada ancak bu örnekte, bırakma amacıyla aynı olan varsayılan ayarlayın  **Proje adı**:

    [![](hello-mac-images/setup04.png "Yeni çözüm ayrıntılarını doğrulanıyor")](hello-mac-images/setup04.png#lightbox)

5. Tıklayın **Oluştur** düğmesi.

Mac için Visual Studio yeni Xamarin.Mac uygulaması oluşturma ve uygulamanın çözüme eklenin varsayılan dosyaları görüntüleyin:

 [![](hello-mac-images/project01.png "Yeni çözüm varsayılan görünümü")](hello-mac-images/project01.png#lightbox)

Visual Studio Mac kullanımlar için **çözümleri** ve **projeleri**, Visual Studio yapan tam aynı şekilde. Bir çözüm bir veya daha fazla proje tutan bir kapsayıcıdır; projeleri, uygulamaları, kitaplıkları, test uygulamaları destekleyen içerebilir. Bu durumda, Mac için Visual Studio çözüm hem uygulama projesinde otomatik olarak oluşturmuştur.

İstenirse, geliştirici bir veya daha fazla kod ortak ve paylaşılan kodu içeren bir kitaplık projeleri oluşturabilirsiniz. Bu kitaplık projeleri uygulama projesi tarafından kullanılan ya da diğer Xamarin.Mac uygulaması projeleri ile paylaşılan (veya Xamarin.iOS ve Xamarin.Android kod türüne göre), standart bir .NET uygulaması aynı şekilde.

## <a name="anatomy-of-a-xamarinmac-application"></a>Bir Xamarin.Mac uygulamasını anatomisi

İOS programlama ile ilgili bilgi sahibi değilseniz benzerlikler çok fazla vardır. Aslında, iOS bir Cocoa Mac tarafından kullanılan kavramları birçok üzerinden çapraz, slimmed aşağı sürümüdür CocoaTouch framework kullanır.

Proje dosyalarında göz atın:

-   `Main.cs` – Bu, uygulamanın ana girdi noktası içerir. Uygulama başlatıldığında çalıştırılan yöntemi ve çok birinci sınıf içerir.
-   `AppDelegate.cs` – Bu dosya işletim sisteminden olayları dinlemek için sorumlu ana uygulama sınıfı içerir.
-   `Info.plist` – Bu dosya, uygulama adı, simgeler vb. gibi uygulama özellikleri içerir.
-   `Entitlements.plist` -Bu dosya uygulama yetkilendirmelerini içerir ve korumalı alana alma ve iCloud desteği gibi şeyler erişmesini sağlar.
-  `Main.storyboard` – Bir uygulama için kullanıcı arabirimi (Windows ve menüler) tanımlar ve geçişler Uyarlamasız aracılığıyla Windows arasındaki bağlantılar kullanıma yerleştirir. Görsel Taslaklar görünümleri (kullanıcı arabirimi öğeleri) tanımını içeren XML dosyalarıdır. Bu dosya oluşturulur ve arabirim Oluşturucu Xcode içinde tarafından saklanır.
-   `ViewController.cs` – Bu ana pencereyi denetleyicisidir. Denetleyicileri başka bir makalede ayrıntılı ele alınacak, ancak şimdilik, ana herhangi bir görünüm altyapısı olan bir denetleyici düşünülebilir.
-   `ViewController.designer.cs` – Bu dosya yardımcı ana ekran kullanıcı arabirimi ile tümleştirilen tesisat kod içerir.

Aşağıdaki bölümlerde, bu dosyaların bazıları aracılığıyla Hızlı Bakış götürür. Daha sonra daha ayrıntılı olarak incelenecek, ancak artık kendi temel bilgileri anlamak için iyi bir fikirdir.

### <a name="maincs"></a>Main.cs

**Main.cs** dosyası çok basittir. Statik içerdiği `Main` yöntemini yeni bir Xamarin.Mac uygulama örneği oluşturur ve bu sınıfın adını geçirir, işletim sistemi olayları, bu durumda olduğu işleyecek `AppDelegate` sınıfı:

```csharp
using System;
using System.Drawing;
using Foundation;
using AppKit;
using ObjCRuntime;

namespace Hello_Mac
{
        class MainClass
        {
                static void Main (string[] args)
                {
                        NSApplication.Init ();
                        NSApplication.Main (args);
                }
        }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` Dosyasını içeren bir `AppDelegate` pencereleri oluşturma ve işletim sistemi olayları dinleme sorumludur sınıfı:

```csharp
using AppKit;
using Foundation;

namespace Hello_Mac
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

Bu kod, önce bir iOS uygulaması Geliştirici yerleşik olan ancak nispeten basittir sürece muhtemelen tanınmayan olur.

`DidFinishLaunching` Yöntemi çalıştıktan sonra uygulama örneği ve gerçekte uygulamanın penceresi oluşturma ve görünümü içinde görüntüleme işleminin başlangıç sorumludur.

`WillTerminate` Yöntemi, kullanıcı veya sistem uygulamasının bir kapatma örnek oluşturulduğunda çağrılır. Bu yöntem, geliştirici (kullanıcı tercihleri veya pencere boyutu ve konumu kaydetme gibi) sonlandırılmadan önce uygulamayı sonlandırmak için kullanmanız gerekir.

### <a name="viewcontrollercs"></a>ViewController.cs

Cocoa (ve türetme CocoaTouch) olarak bilinen kullanan *Model View Controller* (MVC) deseni. `ViewController` Bildirimi nesneyi temsil eden gerçek uygulama penceresi denetler. Genel olarak, oluşturulan her bir pencere için (ve windows birçok başka şeyler için), gösteren yeni görünümler (denetimler) ekleyerek bu, vb. gibi pencerenin yaşam döngüsü, sorumlu olduğu bir denetleyici yoktur.

`ViewController` Ana pencerenin denetleyicisi bir sınıftır. Ana pencere için yaşam döngüsünü sorumlu olduğu anlamına gelir. Bu ayrıntılı olarak daha sonra Şimdi Al, hızlı bir bakış için inceleneceğini:

```csharp
using System;

using AppKit;
using Foundation;

namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }
    }
}
```

### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Ana pencere sınıfı için tasarımcı dosyası şu anda boş, ancak kullanıcı arabirimi Xcode içinde arabirim Oluşturucu ile oluşturulan, otomatik olarak Visual Studio tarafından Mac için doldurulur:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac to store outlets and
// actions made in the UI designer. If it is removed, they will be lost.
// Manual changes to this file may not be handled correctly.
//
using Foundation;

namespace Hello_Mac
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

Mac için Visual Studio tarafından otomatik olarak yönetilen ve herhangi bir pencere veya uygulama görünümünde eklenmiş olan denetimlere erişim sağlar önkoşul tesisat kodu sağlamak gibi geliştirici genellikle Tasarımcı dosyalarla ilgili değildir.

Xamarin.Mac uygulama projesini oluşturduktan sonra ve bileşenlerinin temel bir anlayışa Xcode'a ve arabirim Oluşturucu kullanarak kullanıcı arabirimi oluşturmak için geçiş yapın.

### <a name="infoplist"></a>Info.plist

`Info.plist` Dosya içeren Xamarin.Mac uygulama hakkındaki bilgileri gibi kendi **adı** ve **paket grubu tanımlayıcısı**:

[![](hello-mac-images/infoplist01.png "Plist Düzenleyicisi Mac için Visual Studio")](hello-mac-images/infoplist01.png#lightbox)

Ayrıca tanımlar _film şeridi_ altında Xamarin.Mac uygulama için kullanıcı arabirimini görüntülemek için kullanılacak **ana arabirimi** açılır. Yukarıdaki örnekte, söz konusu olduğunda `Main` açılan listede ilişkili `Main.storyboard` projenin kaynak ağacında **Çözüm Gezgini**. Ayrıca belirterek uygulamanın olumsuz tanımlar *varlık Kataloğu* bunları içeren (**AppIcon** bu durumda).

### <a name="entitlementsplist"></a>Entitlements.plist

Uygulamanın `Entitlements.plist` dosya denetimleri gibi Xamarin.Mac uygulamasını olan yetkilendirmelerini **korumalı alana alma** ve **iCloud**:

[![](hello-mac-images/entitlements01.png "Yetkilendirmeler Düzenleyicisi Mac için Visual Studio")](hello-mac-images/entitlements01.png#lightbox)

Hello World örnek için hiçbir yetkilendirmeleri gerekir. Sonraki bölümde düzenlemek için Xcode'un arabirim Oluşturucu kullanmayı gösterir `Main.storyboard` dosya ve Xamarin.Mac uygulamanın kullanıcı arabirimini tanımlar.

## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode ve arabirim Oluşturucu giriş

Xcode bir parçası olarak, Apple sağlayan bir geliştirici, bir kullanıcı arabirimi Tasarımcısı'nda görsel olarak oluşturmak arabirim Oluşturucu adında bir araç oluşturmuştur. Xamarin.Mac yerlere Objective-C kullanıcı olarak aynı araçlarla oluşturulacak kullanıcı Arabirimi sağlayan arabirim Oluşturucu ile tümleştirilir.

Başlamak için çift `Main.storyboard` dosyası **Çözüm Gezgini** Xcode ve arabirim Oluşturucu düzenleme için açın:

[![](hello-mac-images/xcode01.png "Çözüm Gezgini'nde Main.storyboard dosyası")](hello-mac-images/xcode01.png#lightbox)

Bu Xcode başlatın ve aşağıdaki gibi görünür:

[![](hello-mac-images/xcode02.png "Varsayılan Xcode arabirim Oluşturucu görünümü")](hello-mac-images/xcode02.png#lightbox)

Arabirimi tasarım başlamadan önce kullanılacak ana özellikleri ile yönlendirmek için Xcode hızlı bir genel bakış atın.

> [!NOTE]
> Geliştirici bir Xamarin.Mac uygulamasını için kullanıcı arabirimi oluşturmak için Xcode ve arabirim Oluşturucu kullanmak zorunda değildir, kullanıcı arabirimini doğrudan C# kod oluşturulabilir ancak, bu makalenin kapsamı dışındadır. Bu öğreticinin geri kalanında kullanıcı arabirimi oluşturmak için basitleştirmek amacıyla, arabirim Oluşturucu kullanacaklardır.


### <a name="components-of-xcode"></a>Xcode bileşenleri

Açarken bir `.storyboard` dosya ile açılır Xcode'da Mac için Visual Studio'dan bir **Proje Gezgini** soldaki **arabirimi hiyerarşi** ve **Arayüzü Düzenleyicisi**ortada ve **elektrik ve özellikleri** bölümünde sağ taraftaki:

[![](hello-mac-images/xcode03.png "Xcode'da arabirim Oluşturucu çeşitli bölümlerini")](hello-mac-images/xcode03.png#lightbox)

Aşağıdaki bölümlerde, her birinin bu Xcode özellikleri yapın ve bunları bir Xamarin.Mac uygulamasını arabirimi oluşturmak için nasıl kullanacağınızı göz atın.

### <a name="project-navigation"></a>Proje gezinmesi

Açarken bir `.storyboard` dosya oluşturur Mac için Visual Studio Xcode'da düzenlemek için bir *Xcode proje dosyası* değişiklikleri kendisi ve Xcode arasında iletişim kurmak için arka planda. Geliştirici Xcode'dan Mac için Visual Studio'ya dönün geçirildiğinde, daha sonra bu proje için yapılan tüm değişiklikler Xamarin.Mac projenin Visual Studio tarafından Mac için eşitlenir

**Proje gezinmesi** bölümü sağlar, bu dosyaların tümünün arasında gezinmek Geliştirici _dolgu_ Xcode projesi. Genellikle, bunlar yalnızca ilginizi olacaktır `.storyboard` bu listede gibi dosyaları `Main.storyboard`.

### <a name="interface-hierarchy"></a>Hiyerarşi arabirimi

**Arabirimi hiyerarşi** bölümü kullanıcı arabiriminin birkaç anahtar özellikleri gibi kolayca erişmek Geliştirici sağlar, **yer tutucuları** ve ana **penceresi**. Bu bölümde, kullanıcı arabirimi oluşturan bireysel öğeleri (görünümleri) erişmeye ve bunlar etrafında hiyerarşisinde sürükleyerek yuvalanmış yöntemini ayarlamak için kullanılabilir.

### <a name="interface-editor"></a>Arayüzü Düzenleyicisi

**Arayüzü Düzenleyicisi** bölüm üzerinde kullanıcı arabirimi grafik düzenlendiği yüzeyi sağlar. Öğeleri sürükleyin **Kitaplığı** bölümünü **elektrik ve özellikleri** tasarım oluşturulacak bölüm. Kullanıcı arabirimi öğeleri (görünümleri) tasarım yüzeyine eklendikçe için eklenecekler **arabirimi hiyerarşi** bölümü içinde göründükleri sırayla **Arayüzü Düzenleyicisi**.

### <a name="properties--utilities"></a>Elektrik ve özellikleri

**Elektrik ve özellikleri** bölüm iki ana bölümlere ayrılmıştır **özellikleri** (denetçiler olarak da adlandırılır) ve **Kitaplığı**:

[![](hello-mac-images/xcode04.png "Özellik denetçisi")](hello-mac-images/xcode04.png#lightbox)

Başlangıçta bu bölümde ancak, bitmek üzere geliştirici bir öğedeki seçerse **Arayüzü Düzenleyicisi** veya **arabirimi hiyerarşi**, **özellikleri** bölümü olacaktır Belirtilen öğe ve ayarlamak özellikler hakkındaki bilgilerle doldurulur.

İçinde **özellikleri** bölümünde, vardır sekiz farklı *denetçisi sekmeleri*, aşağıdaki çizimde gösterildiği gibi:

[![](hello-mac-images/xcode05.png "Tüm denetçiler genel bakış")](hello-mac-images/xcode05.png#lightbox)

### <a name="properties--utility-types"></a>Özellikleri & yardımcı programı türleri

Soldan-sağa, aşağıdaki sekmelerden şunlardır:

-   **Inspector dosya** – dosya denetçisi düzenleniyor Xıb dosyasının konumunu ve dosya adı gibi dosya bilgileri gösterir.
-   **Hızlı Yardım** – hızlı yardımcı olmak için sekmesinde Xcode'da seçili üzerinde göre Bağlamsal Yardım sağlar.
-   **Kimlik denetçisi** – kimlik Inspector'ı Seçili denetim/görünüm hakkında bilgi sağlar.
-   **Öznitelikleri denetçisi** – öznitelikleri Inspector Geliştirici Seçili denetim/görünüm çeşitli özniteliklerini özelleştirmek sağlar.
-   **Boyut denetçisi** – boyutu Inspector Geliştirici boyutu ve yeniden boyutlandırma Seçili denetim/görünümün davranışını denetlemek sağlar.
-   **Bağlantıları denetçisi** – bağlantıları denetçisi gösterir **çıkışı** ve **eylem** seçili denetimlerin bağlantıları. Çıkışlar ve Eylemler aşağıda ayrıntılı olarak açıklanmıştır.
-   **Bağlamaları denetçisi** – bağlamaları Inspector Geliştirici değerlerine veri modelleri için otomatik olarak bağlanır, böylece denetimlerini yapılandırmak sağlar.
-   **Görüntüleme etkileri denetçisi** – görünümü etkileri Inspector Geliştirici animasyonları gibi denetimler etkileri belirtmek sağlar.

Kullanım **Kitaplığı** bölümüne denetimler ve grafik kullanıcı arabirimi oluşturmak için tasarımcıya yerleştirmek için nesneleri bulmak için:

[![](hello-mac-images/xcode06.png "Xcode kitaplığı denetçisi")](hello-mac-images/xcode06.png#lightbox)

## <a name="creating-the-interface"></a>Arabirimi oluşturma

Xcode IDE ve arabirim Oluşturucu kapsamında temelleri ile ana görünüm için kullanıcı arabirimi Geliştirici oluşturabilirsiniz.

Aşağıdakileri yapın:

1. Sürükleyin, Xcode'da bir **düğme** gelen **kitaplığı bölüm**:

    [![](hello-mac-images/xcode07.png "Bir NSButton kitaplığı Inspector'ı seçme")](hello-mac-images/xcode07.png#lightbox)

2. Düğme üzerine sürüklersiniz bırak **görünümü** (altında **penceresi denetleyicisi**) içinde **Arayüzü Düzenleyicisi**:

    [![](hello-mac-images/xcode08.png "Arabirim tasarımı için bir düğme ekleme")](hello-mac-images/xcode08.png#lightbox)

3. Tıklayarak **başlık** özelliğinde **özniteliği denetçisi** düğmenin başlık değiştirip `Click Me`:

    [![](hello-mac-images/xcode09.png "Düğmenin özelliklerini ayarlama")](hello-mac-images/xcode09.png#lightbox)

4. Sürükleme bir **etiket** gelen **kitaplığı bölüm**:

    [![](hello-mac-images/xcode10.png "Bir etiket kitaplığı Inspector'ı seçme")](hello-mac-images/xcode10.png#lightbox)

5. Etiket üzerine bırakın **penceresi** içinde düğmesinin yanındaki **Arayüzü Düzenleyicisi**:

    [![](hello-mac-images/xcode11.png "Arabirim tasarımı için bir etiket ekleme")](hello-mac-images/xcode11.png#lightbox)

6. Etiket üzerinde sağ tutamacını yakalayın ve pencerenin kenardaki olana kadar sürükleyin:

    [![](hello-mac-images/xcode12.png "Etiketi yeniden boyutlandırma")](hello-mac-images/xcode12.png#lightbox)

7. Yalnızca eklediğiniz düğmeyi seçin **Arayüzü Düzenleyicisi**, tıklatıp **Kısıtlamaları Düzenleyicisi** simgesine gidip pencerenin:

    [![](hello-mac-images/xcode13.png "Kısıtlamaları düğme ekleme")](hello-mac-images/xcode13.png#lightbox)

8. Düzenleyici üst kısmında tıklayın **kırmızı ben-kirişleri** üst ve sol konumunda. Pencereyi yeniden boyutlandırıldığından, bu düğmeyi aynı konumu ekranın sol üst köşesinde tutar.

9. Ardından, kontrol **yükseklik** ve **genişliği** kutuları ve varsayılan boyutlarını kullanın. Pencereyi yeniden boyutlandırdığında bu düğmeyi aynı boyutta tutar.

10. Tıklayın **ekleme 4 kısıtlamaları** düğmesine kısıtlama Ekle ve düzenleyiciyi kapatın.

11. Etiket seçin ve tıklayın **Kısıtlamaları Düzenleyicisi** yeniden simgesi:

    [![](hello-mac-images/xcode14.png "Etikete kısıtlamaları ekleme")](hello-mac-images/xcode14.png#lightbox)

12. Tıklayarak **kırmızı ben-kirişleri** üst, sağ ve sol **Kısıtlamaları Düzenleyicisi**, konumları, verilen X ve Y takılmış ve büyütme ve pencerenin çalışır boyutlandırıldığından küçültme etiketin bildirir uygulama.

13. Yeniden denetleyin **yükseklik** kutusunda ve varsayılan boyut kullanın ve ardından'a tıklayın **ekleme 4 kısıtlamaları** düğmesine kısıtlama Ekle ve düzenleyiciyi kapatın.

14. Kullanıcı arabirimi değişiklikleri kaydedin.

Yeniden boyutlandırma ve taşıma denetimlerin çalışırken, arabirim Oluşturucu dayalı yararlı ek ipuçları verir dikkat edin [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Bu yönergeleri, bilinen bir görünümü ve deneyimini Mac kullanıcıları için olan yüksek kaliteli uygulamalar oluşturmak için geliştirici yardımcı olur.

Konum **arabirimi hiyerarşi** bölüm düzeni ve kullanıcı arabirimi oluşturan öğeleri hiyerarşisini nasıl gösterildiğini görmek için:

[![](hello-mac-images/xcode15.png "Arabirim hiyerarşisinde bir öğe seçme")](hello-mac-images/xcode15.png#lightbox)

Buradan, geliştirici düzenleme veya gerekirse, kullanıcı Arabirimi öğeleri yeniden sıralamak için sürükleyin öğeleri seçebilirsiniz. Bir kullanıcı Arabirimi öğesi başka bir öğe tarafından kapsanan, örneğin, bunlar penceresinde en üst öğe yapma listenin sürükleyin.

Oluşturulan kullanıcı arabirimiyle, böylece Xamarin.Mac erişebilir ve bunlarla etkileşimli C# kodunda UI öğeleri kullanıma sunmak Geliştirici gerekir. Sonraki bölümde **çıkışlar ve eylemleri**, bunun nasıl yapılacağı gösterilmektedir.

### <a name="outlets-and-actions"></a>Çıkışlar ve Eylemler

Bu nedenle nelerdir **çıkışlar** ve **eylemleri**? Geleneksel .NET kullanıcı arabirimi programlama, kullanıcı arabiriminde bir denetim eklendiğinde bir özellik olarak otomatik olarak sunulur. Öğeleri farklı Mac için çalışır, yalnızca bir görünüm için denetim ekleme, koda erişilebilir duruma getirilmez. Geliştirici kod için UI öğesi açıkça kullanıma sunması gerekir. Sırayla Bunu yapmak için Apple iki seçenek sunar:

-   **Çıkışlar** – çıkışlar özelliklerine benzer. Geliştirici bir denetimi için bir çıkış bağlayan, kod bir özelliği aracılığıyla gösterilir, olay işleyicileri eklemek gibi işlemler yapabilirsiniz yöntemleri çağırmak Bu, vs.
-   **Eylemler** – eylemlerdir WPF komut desenine benzer. Örneğin, bir eylem denetim gerçekleştirildiğinde, varsayalım bir düğmeye tıkladığınızda, denetim kodu otomatik olarak bir yöntem çağırır. Geliştirici birçok denetimleri için aynı eylemi bağlayabileceğinizi olduğundan güçlü ve uygun eylemleri.

Xcode'da, **çıkışlar** ve **eylemleri** aracılığıyla kod içinde doğrudan eklenen *denetimini sürükleyerek*. Daha açık belirtmek gerekirse oluşturmak için buna bir **çıkışı** veya **eylem**, geliştirici eklemek için bir denetim öğesini seçersiniz bir **çıkışı** veya **eylem** basılı için **denetimi** klavyede tuş ve doğrudan koda bu denetimi sürükleyin.

Xamarin.Mac geliştiriciler için bu Geliştirici istedikleri oluşturmak için C# dosyasına karşılık gelen Objective-C saplama dosyalarını sürükleyin anlamına gelir **çıkışı** veya **eylem**. Mac için Visual Studio oluşturulan adlı bir dosya `ViewController.h` arabirim Oluşturucu kullanmak için oluşturulan Xcode projesi dolgu bir parçası olarak:

[![](hello-mac-images/xcode16.png "Xcode kaynağı görüntüleme")](hello-mac-images/xcode16.png#lightbox)

Bu saplama `.h` dosya yansıtmalar `ViewController.designer.cs` Xamarin.Mac projeye yeni bir otomatik olarak eklenen `NSWindow` oluşturulur. Bu dosya yerdir ve arabirim Oluşturucu tarafından yapılan değişiklikleri eşitlemek için kullanılacak **çıkışlar** ve **eylemleri** kullanıcı Arabirimi öğeleri için C# kodu açık şekilde oluşturulur.

#### <a name="adding-an-outlet"></a>Bir çıkış ekleme

Hangi temel bir anlayış **çıkışlar** ve **eylemleri** olan oluşturmak bir **çıkışı** C# kodumuz oluşturulan etiket kullanıma sunmak için.

Aşağıdakileri yapın:

1. Şu ana kadar sağ üst köşesinde ekranın en Xcode'da tıklayın **çift daire** açmak için düğmeyi **Yardımcısı Düzenleyicisi**:

    [![](hello-mac-images/outlet01.png "Yardımcısı Düzenleyicisi görüntülenirken")](hello-mac-images/outlet01.png#lightbox)

2. Xcode ile bir Bölünmüş Görünüm moduna geçiş yapar **Arayüzü Düzenleyicisi** bir tarafta ve **Kod Düzenleyicisi** diğer.

3. Xcode otomatik olarak teslim bildirimi **ViewController.m** dosyası **Kod Düzenleyicisi**, yanlış olduğu. Hangi tartışmasında **çıkışlar** ve **eylemleri** olan yukarıdaki Geliştirici olması gerekecektir **ViewController.h** seçili.

4. Üst kısmındaki **Kod Düzenleyicisi** tıklayarak **otomatik bağlantı** seçip `ViewController.h` dosyası:

    [![](hello-mac-images/outlet02.png "Doğru dosya seçme")](hello-mac-images/outlet02.png#lightbox)

5. Xcode, artık seçili dosyanın doğru olması gerekir:

    [![](hello-mac-images/outlet03.png "ViewController.h dosyayı görüntüleme")](hello-mac-images/outlet03.png#lightbox)

6. **Son adım çok önemli!** Geliştirici seçilen doğru dosya yoksa bunlar oluşturmak mümkün olmayacaktır **çıkışlar** ve **eylemleri** veya yanlış sınıfa C# dilinde sunulur!

7. İçinde **Arayüzü Düzenleyicisi**, basılı **denetimi** tıklatıp sürükleme Kod Düzenleyicisi'ni yukarıda oluşturulan etiket klavyede tuş ve hemen altına `@interface ViewController : NSViewController {}` kod:

    [![](hello-mac-images/outlet04.png "Bir çıkış oluşturmak için sürükleme")](hello-mac-images/outlet04.png#lightbox)

8. Bir iletişim kutusu görüntülenir. Bırakın **bağlantı** kümesine **çıkışı** girin `ClickedLabel` için **adı**:

    [![](hello-mac-images/outlet05.png "Çıkış tanımlama")](hello-mac-images/outlet05.png#lightbox)

9. Tıklayın **Connect** oluşturmak için düğmeyi **çıkışı**:

    [![](hello-mac-images/outlet06.png "Son çıkış görüntüleme")](hello-mac-images/outlet06.png#lightbox)

10. Değişiklikleri dosyaya kaydedin.

#### <a name="adding-an-action"></a>Bir eylem ekleme

Ardından, C# kodu düğmeyi kullanıma sunar. Yalnızca etiket gibi yukarıdaki, geliştirici düğme kadar wire bir **çıkışı**. Biz yalnızca yanıt tıklanan düğmeyi kullanmak istiyorsanız tamamlamanız bir **eylem** yerine.

Aşağıdakileri yapın:

1. Xcode hala olduğundan emin olun **Yardımcısı Düzenleyicisi** ve **ViewController.h** dosyasıdır görünür **Kod Düzenleyicisi**.
2. İçinde **Arayüzü Düzenleyicisi**, basılı **denetimi** klavyede tuş ve Kod Düzenleyicisi'ni yukarıda oluşturduğunuz düğmeyi tıklatın Sürükle hemen altına `@property (assign) IBOutlet NSTextField *ClickedLabel;` kod:

    [![](hello-mac-images/action01.png "Bir eylem oluşturmak için sürükleme")](hello-mac-images/action01.png#lightbox)

3. Değişiklik **bağlantı** için yazın **eylem**:

    [![](hello-mac-images/action02.png "Eylem tanımlama")](hello-mac-images/action02.png#lightbox)

4. Girin `ClickedButton` olarak **adı**:

    [![](hello-mac-images/action03.png "Yeni Eylem adlandırma")](hello-mac-images/action03.png#lightbox)

5. Tıklayın **Connect** oluşturmak için düğmeyi **eylem**:

    [![](hello-mac-images/action04.png "Son eylem görüntüleme")](hello-mac-images/action04.png#lightbox)

6. Değişiklikleri dosyaya kaydedin.

Kablolu yukarı ve C# kod sunulan kullanıcı arabirimi ile Mac için Visual Studio'ya dönün geçin ve, Xcode ve arabirim Oluşturucu yapılan değişiklikleri eşitlemek istiyorum.

> [!NOTE]
> Büyük olasılıkla bir kullanıcı arabirimi oluşturmak uzun sürdü ve **çıkışlar** ve **eylemleri** bunun için ilk uygulama ve birçok iş, gibi görünebilir, ancak çok sayıda yeni kavramları kullanıma sunulmuştur ve çok zaman harcanan yeni baştan sonra kapsayan. Bir süredir uygulayan ve arabirim Oluşturucu, bu arabirimi ve tüm çalışma sonra kendi **çıkışlar** ve **eylemleri** yalnızca bir veya iki dakika içinde oluşturulabilir.

### <a name="synchronizing-changes-with-xcode"></a>Xcode ile değişiklikler eşitleniyor

Geliştirici Xcode'dan Mac için Visual Studio'ya dönün geçtiğinde Xamarin.Mac projeyle Xcode'da yapmış olduğunuz değişiklikleri otomatik olarak eşitlenir.

Seçin **ViewController.designer.cs** içinde **Çözüm Gezgini** görmek için nasıl **çıkışı** ve **eylem** oluşturan C# dilinde kablolu Kod:

[![](hello-mac-images/sync01.png "Xcode ile değişiklikler eşitleniyor")](hello-mac-images/sync01.png#lightbox)

Bildirim nasıl iki tanımlarında **ViewController.designer.cs** dosyası:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Bulunan tanımlarla hizaya `ViewController.h` Xcode dosyasında:

```csharp
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Mac için Visual Studio bekleyen değişiklikleri **.h** dosya ve ilgili değişiklikleri otomatik olarak eşitler **. designer.cs** bunları uygulamasını kullanıma sunmak için dosya. Dikkat **ViewController.designer.cs** Mac için Visual Studio değiştirmek zorunda değildir kısmi bir sınıf olduğunu **ViewController.cs** , geliştirici yaptığı değişikliklerin üzerine sınıf.

Normalde, geliştirici hiçbir zaman açmanız gerekecek **ViewController.designer.cs**, burada yalnızca eğitim amacıyla sunuldu.

> [!NOTE]
> Çoğu durumda, Mac için Visual Studio otomatik olarak Xcode'da yapılan değişiklikleri görmek ve bunları Xamarin.Mac projeye eşitleyebilirsiniz. Eşitleme otomatik olarak gerçekleşmez kapalı örneği, geri için Xcode geçin ve sonra Visual Studio için Mac için yeniden. Bu başlatır normalde bir eşitleme döngüsü devre dışı.

## <a name="writing-the-code"></a>Kod yazma

Oluşturulan kullanıcı arabirimi ve kullanıcı Arabirimi öğelerine kodu aracılığıyla ortaya çıkardığınız **çıkışlar** ve **eylemleri**, program hayata geçirmek için kod yazmak son hazırız.

Her zaman ilk düğmesine tıklandığında, bu örnek uygulama için etiketi düğmeye tıkladı kaç kez göstermek için güncelleştirilir. Bunu gerçekleştirmek için açık `ViewController.cs` dosyasını çift tıklayarak düzenleme için **Çözüm Gezgini**:

[![](hello-mac-images/code01.png "ViewController.cs dosyası, Mac için Visual Studio'da görüntüleme")](hello-mac-images/code01.png#lightbox)

İlk olarak, bir sınıf düzeyi değişkenleri oluşturun `ViewController` gerçekleşen tıklama sayısı izlemek için sınıf. Sınıf tanımını düzenleyin ve aşağıdaki gibi görünmesi:

```csharp
namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Sonra aynı sınıftaki (`ViewController`), geçersiz kılma `ViewDidLoad` yöntemi ve ilk ileti etiketi ayarlamak için kod ekleyin:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Kullanım `ViewDidLoad`, gibi başka bir yöntem yerine `Initialize`, çünkü `ViewDidLoad` çağrılır *sonra* işletim sistemi yüklenir ve kullanıcı arabiriminden örneği **.storyboard** dosya. Geliştirici önce etiket denetimi erişmeyi denedi, **.storyboard** dosyası tam olarak yüklendi ve örneği, elde edecekleri bir `NullReferenceException` hata etiket denetimi henüz var olmayan olduğundan.

Ardından, düğmeye tıklandığında kullanıcıya yanıt vermek için kod ekleyin. Kısmi aşağıdaki yöntemi ekleyin `ViewController` sınıfı:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Bu kodu ekler **eylem** Xcode ve arabirim oluşturucu içinde oluşturulan ve kullanıcı düğmeye tıkladığında çağrılır.

## <a name="testing-the-application"></a>Uygulamayı Test Etme

Bu beklendiği gibi çalıştığından emin olmak için uygulamayı çalıştırın ve derleme zamanı geldi. Geliştirici oluşturabilir ve tümünü tek bir adımda çalıştırın ya da, onu çalıştıran olmadan oluşturabilirsiniz.

Uygulama yerleşik zaman ne çeşit yapının istedikleri Geliştirici seçebilirsiniz:

-   **Hata ayıklama** – bir hata ayıklama derlemesi derlenir bir **.app** bir sürü uygulama çalışırken neler hata ayıklamak Geliştirici sağlayan ek meta veri dosyasıyla (uygulama).
-   **Yayın** – da yayın derlemesinde oluşturur bir **.app** dosyası, ancak içermez hata ayıklama bilgileri, bu nedenle daha küçük ve daha hızlı yürütür.

Geliştirici derlemeden türünü seçebilirsiniz **yapılandırması Seçici** ekran Mac için Visual Studio üst sol köşesindeki:

[![](hello-mac-images/run01.png "Hata ayıklama derleme seçme")](hello-mac-images/run01.png#lightbox)

## <a name="building-the-application"></a>Uygulama Oluşturma

Bu örnekte, söz konusu olduğunda biz yalnızca hata ayıklama derlemesi'i istiyorsanız, bu nedenle emin **hata ayıklama** seçilir. Uygulama ilk ya basarak derleme **⌘B**, veya **derleme** menüsünde seçin **yapı tüm**.

Herhangi bir hata oluyorum bir **derleme başarılı** ileti gösterilecek Visual Studio'da Mac durum çubuğu için. Hatalar varsa, projeyi gözden geçirin ve yukarıdaki adımları doğru şekilde izlendiği emin olun. Kod (hem de Mac için Visual Studio ve xcode'da) öğreticide kodla eşleştiğini kontrol ederek başlayın.

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulamayı çalıştırmak için üç yolu vardır:

-  Tuşuna **⌘ + Enter**.
-  Gelen **çalıştırma** menüsünde seçin **hata ayıklama**.
-  Tıklayın **yürütmek** araç Mac için Visual Studio düğmesine (yukarıdaki **Çözüm Gezgini**).

Uygulama (Bunu zaten oluşturulmuş edilmemiş varsa) derleme, hata ayıklama modunda başlatın ve onun ana arabirimi penceresini görüntüleyin:

[![](hello-mac-images/run02.png "Uygulamayı çalıştırma")](hello-mac-images/run02.png#lightbox)

Birkaç kez düğmesine tıkladıysanız, etiket sayısı ile güncelleştirilmesi gereken:

[![](hello-mac-images/run03.png "Düğmeye tıklandığında, sonuçlar gösteriliyor")](hello-mac-images/run03.png#lightbox)

## <a name="where-to-next"></a>Sonraki yeri

Bir Xamarin.Mac uygulamasını ile çalışmanın temelleri ile alındığını daha iyi anlamak için aşağıdaki belgelere göz atın:

- [Görsel taslaklara giriş](~/mac/platform/storyboards/index.md) -bu makalede bir Xamarin.Mac uygulamasını görsel Taslaklar ile çalışma için bir tanıtım sunulmaktadır. Oluşturulması ve bakımının yapılması film şeritleri ve arabirim Oluşturucu Xcode'un kullanarak uygulamanın UI ele alınmaktadır.
- [Windows](~/mac/user-interface/window.md) -bu makalede bir Xamarin.Mac uygulamasını Windows ve panel çalışma kapsar. Oluşturup kullanarak Windows ve Windows için C# kodunda yanıt .xib dosyaları, yükleme, Windows ve panel Xcode ve arabirim Oluşturucu, Windows ve panel sürekli ele alınmaktadır.
- [İletişim kutuları](~/mac/user-interface/dialog.md) -bu makalede bir Xamarin.Mac uygulamasını iletişim kutuları ve kalıcı bir Windows çalışma kapsar. Oluşturma ve Xcode ve arabirim Oluşturucu standart iletişim kutuları ile çalışma, görüntüleme ve Windows için C# kodunda yanıt kalıcı Windows bakımı ele alınmaktadır.
- [Uyarılar](~/mac/user-interface/alert.md) -uyarılarla Xamarin.Mac uygulamasında bu makalede ele alınmıştır. Oluşturma ve C# kod uyarıları görüntüleme ve uyarılara yanıt verme ele alınmaktadır.
- [Menüler](~/mac/user-interface/menu.md) -menüler, çeşitli parçaları bir Mac uygulamanın kullanıcı arabirimini; herhangi bir pencere içinde görünebilir açılır ve bağlamsal menü için ekranın üst uygulamanın ana menüden kullanılır. Menüleri Mac uygulamanın kullanıcı deneyimini ayrılmaz bir parçasıdır. Bu makale, bir Xamarin.Mac uygulamasını Cocoa Menülerle çalışma kapsar.
- [Araç çubukları](~/mac/user-interface/toolbar.md) -araç çubukları çalışmak bir Xamarin.Mac uygulamasında bu makalede ele alınmıştır. Oluşturma ve araç çubukları, Xcode ve arabirim Oluşturucu koruma kapsayan çıkışlar ve eylemleri kullanarak, etkinleştirme ve araç çubuğu öğelerini devre dışı bırakma ve son olarak araç çubuğu öğeleri için C# kodunda yanıt koduna araç çubuğu öğelerini nasıl sunacağınızı öğrenin.
- [Tablo görünümleri](~/mac/user-interface/table-view.md) -bu makalede bir Xamarin.Mac uygulamasını tablo görünümleri ile çalışma kapsar. Oluşturma ve tablo görünümleri Xcode ve arabirim Oluşturucu koruma kapsayan çıkışlar ve eylemleri kullanarak, Tablo öğelerini doldurma ve Tablo görünümü öğelerine C# kodunda son yanıt kodu Tablo görünümü öğelerine nasıl sunacağınızı öğrenin.
- [Anahat görünümleri](~/mac/user-interface/outline-view.md) -bu makalede bir Xamarin.Mac uygulamasını anahat görünümleri ile çalışma kapsar. Oluşturup anahat görünümleri Xcode ve arabirim Oluşturucu sürekli kapsayan çıkışlar ve eylemleri kullanarak, ana hat öğeleri doldurmak ve son ana hat görünüm öğeleri için C# kodunda yanıt kodu için anahat öğeleri görüntüle nasıl sunacağınızı öğrenin.
- [Kaynak listeleri](~/mac/user-interface/source-list.md) -bu makalede bir Xamarin.Mac uygulamasını kaynak listeleri ile çalışma kapsar. Oluşturulması ve bakımının yapılması Xcode ve arabirim Oluşturucu kaynak listeler kapsadığı kaynak listesi öğeleri çıkışlar ve eylemleri kullanarak, kaynak liste öğeleri doldurmak ve son olarak kaynak öğelerini listelemek üzere C# kodunda yanıt kodu nasıl sunacağınızı öğrenin.
- [Koleksiyon görünümleri](~/mac/user-interface/collection-view.md) -koleksiyon görünümlerini çalışmak bir Xamarin.Mac uygulamasında bu makalede ele alınmıştır. Oluşturma ve koleksiyon görünümlerini Xcode ve arabirim Oluşturucu koruma kapsar çıkışlar ve eylemleri kullanarak, koleksiyon görünümlerini doldurma ve koleksiyon görünümlerini C# kodunda son yanıt kodu koleksiyon görünümü öğelerine nasıl sunacağınızı öğrenin.
- [Görüntülerle çalışma](~/mac/app-fundamentals/image.md) -bu makalede bir Xamarin.Mac uygulamasını görüntüler ve simgeler ile çalışma kapsar. Oluşturma konusunu ele alınmaktadır ve bir uygulamanın simgesi ve C# kodu ve arabirim Oluşturucu Xcode'un görüntüleri kullanarak oluşturmak görüntü Bakımı gerekli.

Ayrıca göz alma öneririz [Mac Örnekler Galerisi](https://developer.xamarin.com/samples/mac/all/), zengin bir Xamarin.Mac Proje hızlı bir şekilde buluta taşıyın Geliştirici yardımcı olacak kullanıma hazır kod içerir.

Birçok tipik bir Mac uygulamasında bulmak için bir kullanıcı beklediğiniz özellikleri içeren tam bir Xamarin.Mac uygulamasını bir örneği için lütfen bkz [SourceWriter örnek uygulaması](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter kod tamamlama ve basit söz dizimi vurgulama için destek sağlayan bir basit kaynak kod düzenleyicidir.

SourceWriter kod tamamen yorum yaptı ve mümkün olan durumlarda bağlantıları sahip olması sağlanan anahtar teknolojileri veya yöntemleri Xamarin.Mac kılavuzları belgelerinde ilgili bilgileri.

## <a name="summary"></a>Özet

Bu makalede, standart bir Xamarin.Mac uygulamasını temelleri ele. Kapsanan Mac için Visual Studio'da yeni bir uygulama oluşturma, Xcode kullanıcı arabiriminde ve arabirim Oluşturucu tasarlama, C# kullanarak kod için UI öğeleri gösterme **çıkışlar** ve **eylemleri**, birlikte çalışmak için kod ekleme Kullanıcı Arabirimi öğeleri ve son olarak, oluşturma ve bir Xamarin.Mac uygulamasını test etme.

## <a name="related-links"></a>İlgili bağlantılar

- [Merhaba, Mac (örnek)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
