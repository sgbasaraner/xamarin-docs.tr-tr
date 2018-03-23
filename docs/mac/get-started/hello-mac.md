---
title: Hello, Mac
description: Bu kılavuz, ilk Xamarin.Mac uygulama oluşturma adımları ve Mac, Xcode ve arabirim Oluşturucu için Visual Studio gibi geliştirme zincirinin işleminde sunmaktadır. Çıkışlar ve kod için UI denetimleri kullanıma, Eylemler, aynı zamanda sunar ve son olarak, oluşturmak, çalıştırmak ve Xamarin.Mac uygulamayı test etme göstermektedir.
ms.topic: article
ms.prod: xamarin
ms.assetid: 37D0E9E6-979B-7069-B3BE-C5F0AF99BA72
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: 635577bbc35d9e80147ecf7e1a59540099f85b9d
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="hello-mac"></a>Hello, Mac

Xamarin.Mac sağlayan tam olarak yerel Mac uygulamalar C# ve aynı OS X kitaplıkları ve geliştirme sırasında kullanılır arabirimi denetimlerini kullanarak .NET geliştirme *Objective-C* ve *Xcode*. Xamarin.Mac Xcode ile doğrudan tümleşir olduğundan, geliştirici Xcode'nın kullanabilir _arabirimi Oluşturucu_ bir uygulamanın kullanıcı arabirimleri (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

Ayrıca, Xamarin.Mac uygulamaları C# ve .NET içinde yazılmış olduğundan, ortak arka uç kodu Xamarin.iOS ve Xamarin.Android mobil uygulamaları ile paylaşılabilir; her platformda tüm yerel bir deneyim sunarken.

Bu makalede Xamarin.Mac, basit bir oluşturma işlemi boyunca taramasını tarafından Mac ve Xcode'nın arabirimi Oluşturucu için Visual Studio kullanarak bir Mac uygulaması oluşturmak için gereken temel kavramları tanıtılacaktır **Hello, Mac** sayar uygulama bir düğme tıklamıştır:

[![](hello-mac-images/run02.png "Merhaba, çalışan Mac uygulama örneği")](hello-mac-images/run02.png#lightbox)

Aşağıdaki kavramlar ele alınacaktır:

-  **Mac için Visual Studio** – Mac ve onunla Xamarin.Mac uygulama oluşturma için Visual Studio giriş.
-  **Xamarin.Mac uygulama anatomisi** – ne bir Xamarin.Mac uygulama oluşur.
-  **Xcode'nın arabirimi Oluşturucu** – Xcode kullanmak için bir uygulamanın kullanıcı arabirimi tanımlamak için arabirimi Oluşturucu kullanıcının nasıl.
-  **Çıkışlar ve eylemleri** – kullanıcı arabirimi denetimlerini kablo çıkışlar ve eylemleri kullanma.
-  **Dağıtım/test** – çalıştırın ve Xamarin.Mac uygulamayı test etme.


<a name="Requirements" />

## <a name="requirements"></a>Gereksinimler

Aşağıdaki Xamarin.Mac macOS uygulamayla geliştirmek için gereklidir:

- MacOS Yosemite(10.10) çalıştıran bir Mac bilgisayara veya daha büyük.
- Xcode 7 ve üstü (en son kararlı sürümü yüklemek için önerilse de [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).)
- En son sürümünü Xamarin.Mac ve Mac için Visual Studio

Xamarin.Mac ile oluşturulan çalışan Mac uygulamaları aşağıdaki sistem gereksinimleri vardır:

- Mac OS X 10.7 veya üzeri çalıştıran bir Mac bilgisayar.

<a name="Starting_a_new_Xamarin.Mac_App_in_Xamarin_Studio" />

## <a name="starting-a-new-xamarinmac-app-in-visual-studio-for-mac"></a>Mac için Visual Studio'da yeni bir Xamarin.Mac uygulama başlatma

Yukarıda belirtildiği gibi bu kılavuz olarak adlandırılan bir Mac uygulaması oluşturmak için adımlarda size yol gösterir `Hello_Mac` , ana pencereyi tek düğme ve etiket ekler. Düğme tıklatıldığında etiketi tıklattınız sayısı görüntülenir.

Başlamak için aşağıdakileri yapın:

1. Mac için Visual Studio'yu başlatın:

    [![](hello-mac-images/setup01.png "Ana Visual Studio için Mac arabirimi")](hello-mac-images/setup01.png#lightbox)

2. Tıklayın **yeni çözüm...**  bağlantıyı açmak için ekranın üst sol alt köşesindeki **yeni proje** iletişim kutusunda:

    [![](hello-mac-images/setup03.png "Mac için Visual Studio'da yeni bir çözüm oluşturma")](hello-mac-images/setup02.png#lightbox)

3. Seçin **Mac** > **uygulama** > **Cocoa uygulama** tıklatıp **sonraki** düğmesi:

    [![](hello-mac-images/setup03.png "Cocoa uygulama seçme")](hello-mac-images/setup03.png#lightbox)

4. Girin `Hello_Mac` için **App Name**ve varsayılan olarak bir şey tutun. Tıklatın **sonraki**:

    [![](hello-mac-images/setup05.png "Uygulama adını ayarlama")](hello-mac-images/setup05.png#lightbox)

4. Çözümün oluşturulması birkaç farklı projelere barındırmak, geliştirici farklı bir ayarlamak isteyebilirsiniz **çözüm adı** burada ancak bu örnek, bırakın amacıyla aynı olma varsayılan ayarlama  **Proje adı**:

    [![](hello-mac-images/setup04.png "Yeni çözüm ayrıntılarını doğrulanıyor")](hello-mac-images/setup04.png#lightbox)

5. Tıklatın **oluşturma** düğmesi.

Mac için Visual Studio yeni Xamarin.Mac uygulaması oluşturun ve uygulamanın çözüme eklenir varsayılan dosyalar görüntüleyin:

 [![](hello-mac-images/project01.png "Yeni çözüm varsayılan görünüm")](hello-mac-images/project01.png#lightbox)

Mac kullanımlar için Visual Studio **çözümleri** ve **projeleri**, Visual Studio mu tam aynı şekilde. Bir veya daha fazla projeleri tutabilen bir kapsayıcı çözümdür; projeleri uygulamaları, destekleme kitaplıkları, sınama uygulamalarını, vb. içerebilir. Bu durumda, Mac için Visual Studio çözüm ve bir uygulama projesi otomatik olarak oluşturmuştur.

İsterseniz, geliştirici ortak, paylaşılan kodu içeren kitaplık projeleri bir veya daha fazla kod oluşturabilirsiniz. Bu kitaplık projeleri uygulamanın projesi tarafından tüketilen ya da diğer Xamarin.Mac uygulaması projeleri ile paylaşılan (veya Xamarin.iOS ve Xamarin.Android kod türüne göre), standart bir .NET uygulaması aynı şekilde.

<a name="The_Project" />

## <a name="anatomy-of-a-xamarinmac-application"></a>Xamarin.Mac uygulama anatomisi

İle programlama iOS sahibiyseniz, çok sayıda benzerlikler vardır. Aslında, iOS çok kavramlarını üzerinden çapraz şekilde Mac tarafından kullanılan Cocoa, slimmed aşağı sürümü CocoaTouch framework kullanır.

Proje dosyalarında göz atın:

-   `Main.cs` – Bu, uygulamanın ana giriş noktası içerir. Uygulama başlatıldığında, bu çok birinci sınıf ve çalıştırılan yöntemi içerir.
-   `AppDelegate.cs` – Bu dosya işletim sisteminden olaylarını dinleme sorumludur ana uygulama sınıfını içerir.
-   `Info.plist` – Bu dosya, uygulama adı, simgeler vb. gibi uygulama özelliklerini içerir.
-   `Entitlements.plist` -Bu dosyalar için yetkilendirmeleri içerir ve korumalı alan ve iCloud desteği gibi öğeler için erişim hakkı verir.
-  `Main.storyboard` – Bir uygulama için kullanıcı arabirimi (Windows ve menüleri) tanımlar ve Segues ile Windows arasında bağlantılar çıkışı yerleştirir. Film şeritleri görünümler (kullanıcı arabirimi öğeleri) tanımını içeren XML dosyalarıdır. Bu dosya oluşturulur ve Xcode içinde arabirimi Oluşturucu tarafından saklanır.
-   `ViewController.cs` – Bu ana penceresinde denetleyicisidir. Denetleyicileri başka bir makalede ayrıntılı kapsamına ancak şu an için herhangi bir görünüm ana altyapısı bir denetleyici düşünülebilir.
-   `ViewController.designer.cs` – Bu dosya ana ekranının kullanıcı arabirimi ile tümleştirmeye yardımcı olur tesisat kodunu içerir.

Aşağıdaki bölümlerde bu dosyaların bazıları hızlı göz götürür. Daha sonra daha ayrıntılı olarak incelenecek, ancak bunların temelleri şimdi anlamak için iyi bir fikirdir.

<a name="Main_cs" />

### <a name="maincs"></a>Main.cs

**Main.cs** dosya oldukça basittir. Statik içerdiği `Main` yeni bir Xamarin.Mac uygulama örneği oluşturur ve sınıfın adı geçen yöntemi, işletim sistemi olayları, bu durumda olduğu işleyecek `AppDelegate` sınıfı:

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

<a name="AppDelegate_cs" />

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` Dosyasını içeren bir `AppDelegate` pencereler oluşturma ve işletim sistemi olaylarını dinleme sorumlu sınıfı:

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

Bu kod, geliştirici önce bir iOS uygulaması oluşturdu, ancak oldukça basittir sürece muhtemelen tanınmayan olur.

`DidFinishLaunching` Yöntemi çalıştıktan sonra uygulama örneği ve gerçekte uygulamanın penceresi oluşturma ve görünümü içinde görüntüleme işleminin başlayan sorumludur.

`WillTerminate` Kullanıcı veya sistem uygulamasının bir kapatma örneği olduğunda yöntemi çağrılır. Geliştirici (kullanıcı tercihleri veya pencere boyutunu ve konumunu kaydetme gibi) sonlandırılmadan önce uygulamayı son haline getirmek için bu yöntem kullanmanız gerekir.

<a name="ViewController_cs" />

### <a name="viewcontrollercs"></a>ViewController.cs

Cocoa (ve türetme, CocoaTouch) olarak bilinen kullanan *Model View Controller* (MVC) deseni. `ViewController` Bildirimi gerçek uygulama penceresini denetleyen nesneyi temsil eder. Genellikle, oluşturulan her penceresi (ve diğer pek çok Windows'da için), yeni görünümler (denetimler) ekleme, vb. için göstermeyi gibi pencerenin yaşam döngüsü, sorumlu olduğu bir denetleyicisi yoktur.

`ViewController` Ana pencerenin denetleyicisi bir sınıftır. Ana penceresinin yaşam döngüsü için sorumlu olduğu anlamına gelir. Bu ayrıntılı olarak daha sonra Şimdi Al hızlı bir bakış için denenecektir:

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

<a name="ViewController_Designer_cs" />

### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Ana pencere sınıfı için tasarımcı dosyası şu anda boştur, ancak kullanıcı arabirimi Xcode içinde arabirimi Oluşturucu ile oluşturulmuş, otomatik olarak Visual Studio tarafından Mac için doldurulur:

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

Visual Studio tarafından otomatik olarak Mac için yönetilen ve herhangi bir pencere veya uygulama görünümünde eklenen denetimlere erişim veren gerekli tesisat kod sağlamak gibi geliştirici genellikle designer dosyaları ile ilgili değildir.

Oluşturulan Xamarin.Mac uygulama projesi ve bileşenlerinin temel bir anlayış ile arabirimi Oluşturucusu'nu kullanarak kullanıcı arabirimi oluşturmak için Xcode için geçiş yapar.

<a name="Info_plist" />

### <a name="infoplist"></a>Info.plist

`Info.plist` Dosyayı içeren Xamarin.Mac uygulama hakkında bilgi gibi kendi **adı** ve **paket tanımlayıcı**:

[![](hello-mac-images/infoplist01.png "Visual Studio Mac plist Düzenleyicisi")](hello-mac-images/infoplist01.png#lightbox)

Ayrıca tanımlar _film şeridi_ altında Xamarin.Mac uygulama için kullanıcı arabirimi görüntülemek için kullanılacak **ana arabirimi** açılır. Yukarıdaki örnek durumunda `Main` açılır listede teklifiyle `Main.storyboard` projenin kaynak ağacında **Çözüm Gezgini**. Belirterek ayrıca uygulamanın simgelerinden tanımlar *varlık Kataloğu* bunları (Bu durumda AppIcons) içerir.

### <a name="entitlementsplist"></a>Entitlements.plist

Uygulamanın `Entitlements.plist` dosyası denetler Xamarin.Mac uygulama gibi sahip yetkilendirmeler **korumalı alan** ve **iCloud**:

[![](hello-mac-images/entitlements01.png "Visual Studio Mac yetkilendirmeler Düzenleyicisi")](hello-mac-images/entitlements01.png#lightbox)

Hello World örnek için hiçbir yetkilendirmeler gerekli olacaktır. Sonraki bölümde Xcode'nın arabirimi Oluşturucu düzenlemek için nasıl kullanılacağı gösterilir `Main.storyboard` dosya ve Xamarin.Mac uygulamanın UI tanımlayın.

<a name="Introduction_to_Xcode_and_Interface_Builder" />

## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode ve arabirim Oluşturucu giriş

Xcode bir parçası olarak, bir kullanıcı arabirimi Tasarımcısı'nda görsel olarak oluşturmak için geliştirici verir arabirimi oluşturucu adı verilen bir aracı Apple oluşturdu. Xamarin.Mac fluently Objective-C kullanıcılar aynı araçlarıyla oluşturulması için kullanıcı Arabirimi sağlayan arabirim Oluşturucusu ile tümleştirir.

Başlamak için çift `Main.storyboard` dosyasını **Çözüm Gezgini** Xcode ve arabirim Oluşturucu düzenlemek için açın:

[![](hello-mac-images/xcode01.png "Çözüm Gezgini'nde Main.storyboard dosyası")](hello-mac-images/xcode01.png#lightbox)

Bu Xcode başlatın ve aşağıdaki gibi bir şeyi aramak gerekir:

[![](hello-mac-images/xcode02.png "Varsayılan Xcode arabirimi Oluşturucu görünümü")](hello-mac-images/xcode02.png#lightbox)

Arabirimini tasarlamak başlatmadan önce hızlı bir genel bakış, kullanılacak ana özelliklerle yönlendirmek için Xcode alın.

> [!NOTE]
> Geliştirici Xamarin.Mac uygulama için kullanıcı arabirimi oluşturmak için Xcode ve arabirim Oluşturucu kullanmak zorunda değil, kullanıcı arabirimini doğrudan C# kodundan oluşturulabilir ancak, bu makalenin kapsamı dışındadır. Bu öğreticinin geri kalanında kullanıcı arabirimi oluşturmak üzere basitleştirmek amacıyla arabirimi Oluşturucu kullanacaklardır.


<a name="Components_of_Xcode" />

### <a name="components-of-xcode"></a>Xcode bileşenleri

Açarken bir `.storyboard` dosya ile açar Mac için Visual Studio'dan Xcode içinde bir **Proje Gezgini** sol taraftaki **arabirimi hiyerarşi** ve **arabirimi Düzenleyicisi**ortada ve **özellikleri & yardımcı programları** sağdaki bölümü:

[![](hello-mac-images/xcode03.png "Xcode'da arabirimi Oluşturucu çeşitli bölümlerini")](hello-mac-images/xcode03.png#lightbox)

Aşağıdaki bölümlerde bu Xcode özellikleri yapın ve bunları arabirimi Xamarin.Mac uygulaması oluşturmak için nasıl kullanılacağı her göz atın.

<a name="Project_Navigation" />

### <a name="project-navigation"></a>Proje gezinme

Açarken bir `.storyboard` dosya Mac oluşturur için Xcode, Visual Studio düzenlemek için bir *Xcode proje dosyası* değişiklikleri kendisi ve Xcode arasında iletişim kurmak için arka planda. Geliştirici, Visual Studio'ya geri Mac için Xcode geçtiğinde, daha sonra bu proje için yapılan tüm değişiklikler Xamarin.Mac proje ile Visual Studio tarafından Mac için eşitlenir

**Proje Gezinti** bölümü sağlar tüm bu yapmak dosyaları arasında gezinmek Geliştirici _dolgusu_ Xcode projesi. Genellikle, bunlar yalnızca ilginizi olacaktır `.storyboard` bu listede gibi dosyalar `Main.storyboard`.

<a name="Interface_Hierarchy" />

### <a name="interface-hierarchy"></a>Hiyerarşi arabirimi

**Arabirimi hiyerarşi** bölümü sağlar kullanıcı arabiriminin birkaç anahtar özellikleri gibi kolayca erişmek geliştirici kendi **yer tutucuları** ve ana **penceresi**. Bu bölüm, kullanıcı arabirimini oluşturan ayrı ayrı öğeler (görünümler) erişmek için ve bunlar etrafında hiyerarşide sürükleyerek yuvalanmış şekilde ayarlamak için kullanılabilir.

<a name="Interface_Editor" />

### <a name="interface-editor"></a>Düzenleyici arabirimi

**Arabirimi Düzenleyicisi** bölüm üzerinde kullanıcı arabirimi grafik düzenlendiği yüzeyi sağlar. Sürükleme öğelerinden **Kitaplığı** bölümünü **özellikleri & yardımcı programları** tasarım oluşturmak için bölüm. Kullanıcı arabirimi öğeleri (görünümler) tasarım yüzeyine eklendikçe için eklenecekler **arabirimi hiyerarşi** bölüm içinde göründükleri sırada **arabirimi Düzenleyicisi**.

<a name="Properties_Utilities" />

### <a name="properties--utilities"></a>Özellikleri & yardımcı programları

**Özellikleri & yardımcı programları** bölüm iki ana bölümlere ayrılmıştır **özellikleri** (denetçiler olarak da adlandırılır) ve **Kitaplığı**:

[![](hello-mac-images/xcode04.png "Özellikler denetçisi")](hello-mac-images/xcode04.png#lightbox)

Başlangıçta bu bölümde ancak, bitmek üzere geliştirici bir öğedeki seçerse **arabirimi Düzenleyicisi** veya **arabirimi hiyerarşi**, **özellikleri** bölüm olacaktır Verilen öğe ve göre ayarlayabilirsiniz özellikler hakkındaki bilgileri ile doldurulur.

İçinde **özellikleri** bölümünde, vardır sekiz farklı *denetçisi sekmeleri*, aşağıdaki çizimde gösterildiği gibi:

[![](hello-mac-images/xcode05.png "Tüm denetçiler genel bakış")](hello-mac-images/xcode05.png#lightbox)

<a name="Properties_Utility_Types" />

### <a name="properties--utility-types"></a>Özellikleri & yardımcı programı türleri

Soldan sağa, aşağıdaki sekmelerden şunlardır:

-   **Inspector dosya** – dosya denetçisi düzenlenmekte olan Xib dosyasının konumunu ve dosya adı gibi dosya bilgileri gösterir.
-   **Hızlı Yardım** – hızlı Yardım sekmesini Xcode'da seçili üzerinde temel Bağlamsal Yardım sağlar.
-   **Kimlik denetçisi** – kimlik denetçisi Seçili denetim/görünüm hakkında bilgi sağlar.
-   **Öznitelikleri denetçisi** – öznitelikleri Inspector Geliştirici Seçili denetim/görünüm çeşitli özniteliklerini özelleştirmek sağlar.
-   **Boyut denetçisi** – boyutu Inspector Geliştirici boyutu ve yeniden boyutlandırma Seçili denetim/görünüm davranışını denetlemek izin verir.
-   **Bağlantıları denetçisi** – bağlantıları denetçisi gösterir **çıkışı** ve **eylem** seçili denetimlerin bağlantıları. Çıkışlar ve eylemleri aşağıda ayrıntılı olarak açıklanmıştır.
-   **Bağlamaları denetçisi** – bağlamaları Inspector Geliştirici değerlerine veri modelleri için otomatik olarak bağlı şekilde denetimleri yapılandırmak izin verir.
-   **Görüntüleme etkileri denetçisi** – görünüm etkileri Inspector Geliştirici animasyonları gibi denetimleri etkileri belirtmek sağlar.

Kullanım **Kitaplığı** bölüm denetimleri ve grafik kullanıcı arabirimini oluşturmak için tasarımcı yerleştirilecek nesneleri bulmak için:

[![](hello-mac-images/xcode06.png "Xcode kitaplığı denetçisi")](hello-mac-images/xcode06.png#lightbox)

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Arabirimi oluşturma

Xcode IDE ve arabirim kapsamdaki Oluşturucu temelleri ile ana görünüm için kullanıcı arabirimi Geliştirici oluşturabilirsiniz.

Aşağıdakileri yapın:

1. Xcode'da, sürükleyin bir **düğme** gelen **kitaplığı bölüm**:

    [![](hello-mac-images/xcode07.png "Kitaplık Denetçisi ' bir NSButton seçme")](hello-mac-images/xcode07.png#lightbox)

2. Düğmeyi üzerine bırakın **Görünüm** (altında **penceresi denetleyicisi**) içinde **arabirimi Düzenleyicisi**:

    [![](hello-mac-images/xcode08.png "Arabirimi tasarımı düğme ekleme")](hello-mac-images/xcode08.png#lightbox)

3. Tıklayın **başlık** özelliğinde **özniteliği denetçisi** düğmenin başlık değiştirip `Click Me`:

    [![](hello-mac-images/xcode09.png "Düğmenin özelliklerini ayarlama")](hello-mac-images/xcode09.png#lightbox)

4. Sürükleme bir **etiket** gelen **kitaplığı bölüm**:

    [![](hello-mac-images/xcode10.png "Kitaplık Denetçisi ' bir etiket seçme")](hello-mac-images/xcode10.png#lightbox)

5. Etiketin üzerine bırakma **penceresi** düğmesinin yanındaki **arabirimi Düzenleyicisi**:

    [![](hello-mac-images/xcode11.png "Arabirimi tasarımı için bir etiket ekleme")](hello-mac-images/xcode11.png#lightbox)

6. Etiket sağ tutamacı alın ve pencerenin kenarına olana kadar sürükleyin:

    [![](hello-mac-images/xcode12.png "Etiket yeniden boyutlandırma")](hello-mac-images/xcode12.png#lightbox)

7. Yeni eklenen düğmesini seçin **arabirimi Düzenleyicisi**, tıklatıp **Kısıtlamaları Düzenleyicisi** simgesi ve penceresinin alt kısmındaki:

    [![](hello-mac-images/xcode13.png "Kısıtlamaları düğme ekleme")](hello-mac-images/xcode13.png#lightbox)

8. Düzenleyici üstündeki **kırmızı t-kirişleri** üst ve sol. Pencere yeniden boyutlandırılır gibi bu düğme aynı konumda ekranın sol üst köşesindeki tutar.

9. Ardından, denetleme **yükseklik** ve **genişliği** kutular ve varsayılan boyutları kullanın. Yeniden boyutlandırır olduğunda bu düğmeye aynı boyutta tutar.

10. ' I tıklatın **eklemek 4 kısıtlamaları** düğmesine kısıtlamalar eklemek ve Düzenleyicisi'ni kapatın.

11. Etiketi seçin ve'ı tıklatın **Kısıtlamaları Düzenleyicisi** yeniden simgesi:

    [![](hello-mac-images/xcode14.png "Etikete kısıtlamaları ekleme")](hello-mac-images/xcode14.png#lightbox)

12. Tıklayarak **kırmızı t-kirişleri** sağ ve sol üst **Kısıtlamaları Düzenleyicisi**, kendi verilen X ve Y konumları takılmış ve büyür ve pencere çalışır boyutlandırıldığında küçültmek için etiket bildirir uygulama.

13. Yeniden denetleyin **yükseklik** kutusunda ve varsayılan boyut kullanın ve ardından **eklemek 4 kısıtlamaları** düğmesine kısıtlamalar eklemek ve Düzenleyicisi'ni kapatın.

14. İçin kullanıcı arabirimi değişiklikleri kaydedin.

Yeniden boyutlandırma ve taşıma geçici denetimleri sırasında arabirimi Oluşturucu dayalı yararlı ek ipuçları sağlayan fark [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Bu kılavuzu tanıdık bir görünüm ve kullanımında Mac kullanıcıları için olan yüksek kaliteli uygulamaları oluşturmak için geliştirici yardımcı olur.

Bakılacak yer **arabirimi hiyerarşi** bölüm düzeni ve kullanıcı arabirimini oluşturan öğeler hiyerarşisini nasıl gösterileceğini görmek için:

[![](hello-mac-images/xcode15.png "Arabirim hiyerarşi içinde bir öğe seçme")](hello-mac-images/xcode15.png#lightbox)

Buradan Geliştirici düzenlemek için kullanıcı Arabirimi öğeleri gerektiğinde yeniden sıralamak için sürükleyin öğeleri seçebilirsiniz. Bir kullanıcı Arabirimi öğesi başka bir öğe tarafından kapsanan, örneğin, bunlar pencerenin en üst öğede yapmak için listenin sürükleyin.

Oluşturulan kullanıcı arabirimiyle Geliştirici Xamarin.Mac erişebilir ve C# kodunda etkileşime böylece kullanıcı Arabirimi öğeleri göstermek gerekir. Sonraki bölümde **çıkışlar ve eylemleri**, bunun nasıl yapılacağı gösterilmektedir.

<a name="Outlets_and_Actions" />

### <a name="outlets-and-actions"></a>Çıkışlar ve eylemleri

Bu nedenle nelerdir **çıkışlar** ve **Eylemler**? Geleneksel .NET kullanıcı arabirimi programlamada eklendiğinde kullanıcı arabiriminde bir denetim bir özellik olarak otomatik olarak sunulur. Şeyler farklı Mac içinde çalışır, yalnızca bir görünüme denetim ekleme, kod erişilebilir değil. Geliştirici açıkça kod için UI öğesi kullanıma gerekir. Sırayla Bunu yapmak için Apple iki seçenek sunar:

-   **Çıkışlar** – çıkışlar özelliklerine benzer. Geliştirici denetimini oluşturan bir çıkış bağlayan varsa, kodu bir özelliği aracılığıyla gösterilir, olay işleyicileri ekleme gibi şeyler yapabilirsiniz yöntemlerini çağırın, vb. üzerinde.
-   **Eylemler** – Eylemler WPF komutu desende benzer. Örneğin, bir eylem denetim gerçekleştirildiğinde deyin bir düğmeye tıkladığınızda, denetim kodu otomatik olarak bir yöntemi çağırır. Geliştirici aynı eylemi birçok Denetimleri'ni wire olduğundan Eylemler güçlü ve uygun değil.

Xcode'da, **çıkışlar** ve **Eylemler** doğrudan kodda eklenen *denetimini sürükleyerek*. Daha belirgin olarak oluşturmak için buna bir **çıkışı** veya **eylem**, geliştirici eklemek için bir denetim öğesi seçecektir bir **çıkışı** veya **eylem** basılı için **denetim** anahtar klavyede ve doğrudan koda denetleyen sürükleyin.

Xamarin.Mac geliştiriciler için bu Geliştirici istedikleri oluşturmak için C# dosyasına karşılık gelen Objective-C saplama dosyalarıyla sürükleyin anlamına gelir **çıkışı** veya **eylem**. Mac için Visual Studio'nun oluşturduğu adlı bir dosya `ViewController.h` arabirimi oluşturucusunu kullanmak için oluşturulan bir Xcode projesi dolgusu bir parçası olarak:

[![](hello-mac-images/xcode16.png "Xcode kaynağında görüntüleme")](hello-mac-images/xcode16.png#lightbox)

Bu saplama `.h` dosya yansıtmalar `ViewController.designer.cs` Xamarin.Mac projeye yeni bir otomatik olarak eklenen `NSWindow` oluşturulur. Bu dosya arabirimi Oluşturucu tarafından yapılan değişiklikleri eşitlemek için kullanılan ve yerdir **çıkışlar** ve **Eylemler** kullanıcı Arabirimi öğeleri için C# kodu gösterilen şekilde oluşturulur.

<a name="Adding_an_Outlet" />

#### <a name="adding-an-outlet"></a>Prizine ekleme

Ne temel bir anlayış **çıkışlar** ve **Eylemler** olan oluşturma bir **çıkışı** bizim C# kod için oluşturulan etiket kullanıma sunmak için.

Aşağıdakileri yapın:

1. Ekranın üst eldeki sağda köşesindeki Xcode'da tıklatın **çift daire** açmak için düğmeye **Yardımcısı Düzenleyicisi**:

    [![](hello-mac-images/outlet01.png "Yardımcısı Düzenleyicisi'ni görüntüleme")](hello-mac-images/outlet01.png#lightbox)

2. Xcode ile bölünmüş görünüm moduna geçiş yapar **arabirimi Düzenleyicisi** bir tarafında ve **Kod düzenleyicisinde** diğer.

3. Xcode otomatik olarak çekilen bildirimi **ViewController.m** dosyasını **Kod düzenleyicisinde**, hatalı olduğu. Hangi tartışma gelen **çıkışlar** ve **Eylemler** olan yukarıdaki Geliştirici olması gerekir **ViewController.h** seçili.

4. Üstündeki **Kod düzenleyicisinde** tıklayın **otomatik bağlantı** seçip `ViewController.h` dosyası:

    [![](hello-mac-images/outlet02.png "Doğru dosya seçme")](hello-mac-images/outlet02.png#lightbox)

5. Xcode şimdi seçili doğru dosya olmalıdır:

    [![](hello-mac-images/outlet03.png "ViewController.h dosyanız görüntüleme")](hello-mac-images/outlet03.png#lightbox)

6. **Son adım çok önemlidir!** Geliştirici seçili doğru dosyanız varsa, bu kaydetmedi oluşturmak değiştiremezler **çıkışlar** ve **Eylemler** veya C# yanlış sınıfına sunulur!

7. İçinde **arabirimi Düzenleyicisi**, basılı **denetim** anahtar klavyede ve tıklatıp sürükleme Kod Düzenleyicisi'ni yukarıda oluşturduğunuz etiketi yalnızca aşağıda `@interface ViewController : NSViewController {}` kod:

    [![](hello-mac-images/outlet04.png "Prizine oluşturmak için sürükleme")](hello-mac-images/outlet04.png#lightbox)

8. Bir iletişim kutusu görüntülenir. Bırakın **bağlantı** kümesine **çıkışı** ve girin `ClickedLabel` için **adı**:

    [![](hello-mac-images/outlet05.png "Çıkış tanımlama")](hello-mac-images/outlet05.png#lightbox)

9. Tıklatın **Bağlan** oluşturmak için düğmesini **çıkışı**:

    [![](hello-mac-images/outlet06.png "Son çıkışı görüntüleme")](hello-mac-images/outlet06.png#lightbox)

10. Değişiklikleri dosyaya kaydedin.

<a name="Adding_an_Action" />

#### <a name="adding-an-action"></a>Bir eylem ekleme

Ardından, C# kodu düğme kullanıma sunar. Yalnızca etiket gibi yukarıdaki, geliştirici düğmesi kadar wire bir **çıkışı**. Yalnızca tıklandığında, düğme için kullanım yanıt istiyoruz bu yana bir **eylem** yerine.

Aşağıdakileri yapın:

1. Xcode hala olduğundan emin olun **Yardımcısı Düzenleyicisi** ve **ViewController.h** dosyasıdır görünür **Kod düzenleyicisinde**.
2. İçinde **arabirimi Düzenleyicisi**, basılı **denetim** anahtar klavyede ve Kod Düzenleyicisi'ni yukarıda oluşturduğunuz düğmesini tıklatıp sürükleme yalnızca aşağıda `@property (assign) IBOutlet NSTextField *ClickedLabel;` kod:

    [![](hello-mac-images/action01.png "Bir eylem oluşturmak için sürükleme")](hello-mac-images/action01.png#lightbox)

3. Değişiklik **bağlantı** için yazın **eylem**:

    [![](hello-mac-images/action02.png "Eylemi tanımlama")](hello-mac-images/action02.png#lightbox)

4. Girin `ClickedButton` olarak **adı**:

    [![](hello-mac-images/action03.png "Yeni Eylem adlandırma")](hello-mac-images/action03.png#lightbox)

5. Tıklatın **Bağlan** oluşturmak için düğmesini **eylem**:

    [![](hello-mac-images/action04.png "Son eylemi görüntüleme")](hello-mac-images/action04.png#lightbox)

6. Değişiklikleri dosyaya kaydedin.

Kablolu yukarı ve C# kodundaki için kullanıcı arabirimi ile Visual Studio'ya geri Mac için geçiş ve onu Xcode ve arabirim Builder yapılan değişiklikleri eşitlemek izin verin.

> [!NOTE]
> Büyük olasılıkla kullanıcı arabirimini oluşturmak için uzun zaman aldı ve **çıkışlar** ve **Eylemler** bunun için ilk uygulama ve iş, çok gibi görünebilir ancak çok sayıda yeni kavram kullanıma sunulmuştur ve çok zaman harcanan Yeni plan kapsayan. Bir süredir kapattığınızdan ve arabirim Oluşturucu, bu arabirimi ve tüm çalışma sonra kendi **çıkışlar** ve **Eylemler** yalnızca bir veya iki dakika içinde oluşturulabilir.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Xcode ile değişiklikler eşitleniyor

Geliştirici Visual Studio'ya geri Mac için Xcode geçiş yaptığında Xcode'da yapmış olduğunuz değişiklikleri otomatik olarak Xamarin.Mac proje ile eşitlenir.

Seçin **ViewController.designer.cs** içinde **Çözüm Gezgini** görmek için nasıl **çıkışı** ve **eylem** yukarı C# dilinde kablolu Kod:

[![](hello-mac-images/sync01.png "Xcode ile değişiklikler eşitleniyor")](hello-mac-images/sync01.png#lightbox)

Bildirim nasıl iki tanımlarında **ViewController.designer.cs** dosyası:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Tanımlarıyla hizaya `ViewController.h` Xcode dosyasında:

```csharp
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Mac için Visual Studio bekleyen değişiklikler için **.h** dosya ve ilgili değişiklikleri otomatik olarak eşitleyen **. designer.cs** uygulamaya göstermek için dosya. Dikkat **ViewController.designer.cs** bir parçalı sınıf, böylelikle Mac için Visual Studio değiştirmek yok **ViewController.cs** hangi için geliştirici tarafından yapılan değişikliklerin üzerine sınıf.

Normalde, geliştirici hiçbir zaman açmanız gerekecek **ViewController.designer.cs**, burada yalnızca eğitim amacıyla verildi.

> [!NOTE]
> Çoğu durumda, Mac için Visual Studio otomatik olarak Xcode'da yapılan değişiklikleri görmek ve Xamarin.Mac projeye eşitleme. Eşitleme otomatik olarak gerçekleşmez kapalı örneği, geri Xcode geçiş yapın ve ardından Visual Studio'ya Mac için yeniden. Bu, normalde bir eşitleme döngüsü tetiklersiniz.

<a name="Writing_the_Code" />

## <a name="writing-the-code"></a>Kod yazma

Oluşturulan kullanıcı arabirimi ve kodu aracılığıyla kullanıma sunulan kullanıcı Arabirimi öğeleri ile **çıkışlar** ve **Eylemler**, sizi program hayata geçirin için kod yazma son hazırsınız.

İlk düğme tıklatıldığında her zaman, bu örnek uygulama için kaç kez düğmesine tıklanana göstermek için etiket güncelleştirilir. Bunu gerçekleştirmek için açık `ViewController.cs` dosyasını çift tıklatarak düzenleme için **Çözüm Gezgini**:

[![](hello-mac-images/code01.png "Mac için Visual Studio'da ViewController.cs dosyayı görüntüleme")](hello-mac-images/code01.png#lightbox)

İlk olarak, bir sınıf düzeyi değişkeni oluşturun `ViewController` gelmiş tıklama sayısını izlemek için sınıf. Sınıf tanımını düzenlemek ve aşağıdaki gibi yapar:

```csharp
namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Ardından, aynı sınıfta (`ViewController`), geçersiz kılma `ViewDidLoad` yöntemi ve ilk ileti etiketi için ayarlamak üzere bazı kodu ekleyin:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Kullanım `ViewDidLoad`, gibi başka bir yöntem yerine `Initialize`, çünkü `ViewDidLoad` çağrılır *sonra* işletim sistemi yüklenir ve kullanıcı arabiriminden örneği **.storyboard** dosya. Geliştirici önce etiket denetimi erişmeye çalıştığınız varsa **.storyboard** dosya tam olarak yüklenir ve örneği, elde edecekleri bir `NullReferenceException` hata etiket denetimi henüz var olmayan olduğundan.

Ardından, düğmesinin tıklatıldığında kullanıcının yanıt vermesi için kodu ekleyin. Aşağıdaki kısmi yöntemine ekleyin `ViewController` sınıfı:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Bu kod ekler **eylem** Xcode ve arabirim Builder oluşturulur ve kullanıcı düğmesine tıkladığında dilediğiniz zaman çağrılır.

<a name="Testing_the_Application" />

## <a name="testing-the-application"></a>Uygulamayı Test Etme

Derleme ve beklendiği gibi çalışacağından emin olmak için uygulamayı çalıştırmak için zaman yapılır. Geliştirici oluşturma ve tümünü tek bir adımda çalıştırın ya da bunlar onu çalıştıran olmadan oluşturabilirsiniz.

Bir uygulama yerleşik olduğunda, geliştirici ne tür bir yapı istedikleri birini seçebilirsiniz:

-   **Hata ayıklama** – bir hata ayıklama derlemesi derlenir bir **.app** bir dizi uygulama çalışırken neler hata ayıklamak Geliştirici sağlayan ek meta veri dosyasıyla (uygulama).
-   **Yayın** – yayın derlemesi da oluşturur bir **.app** dosyası, ancak içermez hata ayıklama bilgileri, böylece daha küçüktür ve daha hızlı çalıştırır.

Geliştirici yapıdan seçin **yapılandırma Seçici** Mac ekran için Visual Studio üst sol alt köşesindeki:

[![](hello-mac-images/run01.png "Hata ayıklama derleme seçme")](hello-mac-images/run01.png#lightbox)

<a name="Building_the_Application" />

## <a name="building-the-application"></a>Uygulama Oluşturma

Bu örnekte, söz konusu olduğunda biz yalnızca hata ayıklama derlemesi istiyorsanız, bu nedenle emin **hata ayıklama** seçilir. Uygulamayı ilk ya basarak yapı **⌘B**, veya **yapı** menüsünde seçin **yapı tüm**.

Herhangi bir hata var. doğru ise bir **yapı başarılı** iletisi görüntülenir Visual Studio için Mac'ın durum çubuğu. Hatalar oluşursa, projeyi gözden geçirmek ve yukarıdaki adımları doğru izlendiği emin olun. Kod (her ikisi de Xcode ve Mac için Visual Studio) öğreticide kodu eşleştiğini onaylayan başlatın.

<a name="Running_the_Application" />

## <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulamayı çalıştırmak için üç yolu vardır:

-  Tuşuna **⌘ + Enter**.
-  Gelen **çalıştırmak** menüsünde seçin **hata ayıklama**.
-  Tıklatın **Yürüt** Visual Studio Mac araç çubuğu düğmesini (yukarıdaki **Çözüm Gezgini**).

Uygulama (Bu zaten oluşturulduğunu kurmadı varsa) oluşturmak, hata ayıklama modunda başlatmak ve kendi ana arabirimi penceresini görüntüleyin:

[![](hello-mac-images/run02.png "Uygulamayı çalıştırma")](hello-mac-images/run02.png#lightbox)

Birkaç kez düğmesine tıkladıysanız, etiket sayısı ile güncelleştirilmesi gerekir:

[![](hello-mac-images/run03.png "Düğmesini tıklatarak sonuçlarını gösterme")](hello-mac-images/run03.png#lightbox)

<a name="Where_to_Next" />

## <a name="where-to-next"></a>Sonraki yeri

Xamarin.Mac uygulama ile çalışmanın temelleri ile daha iyi anlamak için aşağıdaki belgelere göz atın:

- [Film şeritleri giriş](~/mac/platform/storyboards/index.md) -bu makalede Xamarin.Mac uygulamada film şeritleri ile çalışmak için bir giriş sağlar. Oluşturma ve uygulamanın UI film şeritleri ve Xcode'nın arabirimi Oluşturucusu'nu kullanarak koruma kapsar.
- [Windows](~/mac/user-interface/window.md) -bu makalede Windows ve paneller çalışmak Xamarin.Mac uygulamada yer almaktadır. Oluşturma ve Oluşturucusu'nda Windows kullanarak ve Windows için C# kodunda yanıt pencereleri ve panelleri .xib dosyalarından yüklenirken Xcode ve arabirimi, Windows ve paneller koruyarak kapsar.
- [İletişim kutuları](~/mac/user-interface/dialog.md) -bu makalede iletişim kutuları ve kalıcı Windows çalışmayı Xamarin.Mac uygulamada yer almaktadır. Oluşturma ve kalıcı Windows Xcode ve arabirim oluşturucusu, standart iletişim kutuları ile çalışma, görüntüleme ve Windows için C# kodunda yanıt korumaya kapsar.
- [Uyarıları](~/mac/user-interface/alert.md) -bu makalede uyarılarla çalışma Xamarin.Mac uygulamada yer almaktadır. Oluşturma ve C# kodundan uyarıları görüntüleme ve uyarılara yanıt verme kapsar.
- [Menüleri](~/mac/user-interface/menu.md) -menüleri Mac uygulamanın kullanıcı arabiriminde; bir penceresinde herhangi bir yerde görünebilir açılır ve bağlamsal menülere ekranın üstünde uygulamanın ana menüden çeşitli bölümlerinde kullanılır. Menüleri Mac uygulamanın kullanıcı deneyimi ayrılmaz bir parçasıdır. Bu makalede Cocoa Menülerle çalışma Xamarin.Mac uygulamada yer almaktadır.
- [Araç çubukları](~/mac/user-interface/toolbar.md) -bu makalede araç çubukları çalışmak Xamarin.Mac uygulamada yer almaktadır. Oluşturma ve araç çubuklarını Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan çıkışlar ve eylemleri kullanarak, etkinleştirme ve araç öğelerini devre dışı bırakma ve son olarak araç çubuğu öğeleri için C# kodunda yanıt kodu araç öğelerine kullanıma sunmak nasıl.
- [Tablo görünümleri](~/mac/user-interface/table-view.md) -bu makalede Xamarin.Mac uygulama tablo görünümlerle çalışma yer almaktadır. Oluşturma ve tablo görünümleri Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan çıkışlar ve eylemleri kullanarak, Tablo öğelerini doldurma ve son olarak C# kodunda Tablo görünümü öğelerine yanıt kodu Tablo görünümü öğelerini göstermek nasıl.
- [Anahat görünümleri](~/mac/user-interface/outline-view.md) -bu makalede Xamarin.Mac uygulama anahat görünümlerle çalışma yer almaktadır. Oluşturma ve anahat görünümleri Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan çıkışlar ve eylemleri kullanarak, Anahat öğelerini doldurma ve son olarak C# kodunda anahat görünümü öğelerine yanıt kodu anahat görünümü öğelerine kullanıma sunmak nasıl.
- [Kaynak listeleri](~/mac/user-interface/source-list.md) -bu makalede Xamarin.Mac uygulama kaynak listeleri ile çalışma yer almaktadır. Oluşturma ve kaynak listeler Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan kaynak listeler öğelerine çıkışlar ve eylemleri kullanarak, kaynak liste öğeleri doldurma ve son olarak C# kodunda kaynak liste öğelerine yanıt kodu kullanıma sunmak nasıl.
- [Koleksiyon görünümlerini](~/mac/user-interface/collection-view.md) -bu makalede koleksiyon görünümleri ile çalışma Xamarin.Mac uygulamada yer almaktadır. Oluşturma ve koleksiyon görünümlerini Xcode ve arabirim Oluşturucusu'nda, koruma kapsayan çıkışlar ve eylemleri kullanarak, koleksiyon görünümlerini doldurma ve son koleksiyon görünümleri için C# kodunda yanıt kodu koleksiyon görünümü öğelerine kullanıma sunmak nasıl.
- [İmajlarla çalışma](~/mac/app-fundamentals/image.md) -bu makalede görüntüler ve simgeleri çalışmak Xamarin.Mac uygulamada yer almaktadır. Görüntü Bakımı bir uygulamanın simgesini ve C# kodu ve Xcode'nın arabirimi Oluşturucu görüntüleri kullanarak oluşturmak gereken ve oluşturulması ele alınmaktadır.

Ayrıca göz atın alma öneririz [Mac Örnekler Galerisi](https://developer.xamarin.com/samples/mac/all/), bol Xamarin.Mac proje zemin hızlı şekilde ulaşmasını Geliştirici yardımcı olabilecek kullanıma hazır kod içerir.

Örneği bir kullanıcı beklediğiniz tipik bir Mac uygulaması'nda bulmak için özelliklerin çoğunu içeren tam bir Xamarin.Mac uygulama için lütfen bkz [SourceWriter örnek uygulaması](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter kod tamamlama ve basit sözdizimi vurgulama için destek sağlayan bir basit kaynak kod düzenleyicisidir.

SourceWriter kodu tam olarak geçersiz kılınan ve kullanılabilir olduğunda, bağlantılar sahip sağlanmalı temel teknolojileri veya yöntemlerini Xamarin.Mac kılavuzları belgelerinde ilgili bilgilere.

## <a name="summary"></a>Özet

Bu makalede ele alınan standart Xamarin.Mac uygulama temelleri. Kapsanan Mac için Visual Studio'da yeni bir uygulama oluşturma, Xcode kullanıcı arabiriminde ve arabirim Oluşturucu tasarlama, C# kod kullanarak için kullanıcı Arabirimi öğeleri gösterme **çıkışlar** ve **Eylemler**, çalışmak için kod ekleme Kullanıcı Arabirimi öğeleri ve son olarak, derleme ve Xamarin.Mac uygulamayı test etme.

## <a name="related-links"></a>İlgili bağlantılar

- [Merhaba, Mac (örnek)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
