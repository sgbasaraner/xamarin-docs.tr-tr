---
title: "Merhaba, tvOS Hızlı Başlangıç Kılavuzu"
description: "Bu kılavuz ilk Xamarin.tvOS uygulamanızı ve kendi geliştirme araç zinciri oluşturmada size yol gösterir. Aynı zamanda kod için UI denetimleri gösterir ve oluşturmak, çalıştırmak ve Xamarin.tvOS uygulama sınamak üzere verilmektedir Xamarin Tasarımcı ' da sunar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/02/2018
ms.openlocfilehash: fe58aa8ffb74a9b6e937be5a7f1dde0432794405
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="hello-tvos-quick-start-guide"></a>Merhaba, tvOS Hızlı Başlangıç Kılavuzu

_Bu kılavuz ilk Xamarin.tvOS uygulamanızı ve kendi geliştirme araç zinciri oluşturmada size yol gösterir. Aynı zamanda kod için UI denetimleri gösterir ve oluşturmak, çalıştırmak ve Xamarin.tvOS uygulama sınamak üzere verilmektedir Xamarin Tasarımcı ' da sunar._

Apple, Apple tvOS 11 çalışan TV, Apple TV 4 K 5 nesil yayımladı.

Apple TV platform, geliştiriciler, zengin, modern uygulamalar oluşturma ve Apple TV's yerleşik uygulama mağazası yayın vermeden açıktır.

İle Xamarin.iOS geliştirme hakkında bilginiz varsa tvOS geçişi oldukça basit bulmanız gerekir. API ve özelliklerin çoğu aynıdır, ancak birçok ortak API'leri (WebKit gibi) kullanılamaz. Ayrıca, çalışma ile Siri uzaktan dayalı dokunmatik iOS cihazları mevcut olmayan bazı tasarım zorluklar oluşturur.

Bu kılavuz, bir Xamarin uygulaması tvOS ile çalışmaya giriş verecektir. Apple'nın tvOS hakkında daha fazla bilgi için lütfen bkz [4 K Apple TV için hazırlanın](https://developer.apple.com/tvos/) belgeleri.

## <a name="overview"></a>Genel Bakış

Xamarin.tvOS C# ve aynı OS X kitaplıkları ve geliştirme sırasında kullanılır arabirimi denetimlerini kullanarak .NET tam olarak yerel Apple TV uygulamalar geliştirmenize olanak tanır *Swift* (veya *Objective-C*) ve *Xcode*.

Xamarin.tvOS uygulamalar C# ve .NET içinde yazılmış olduğundan, ayrıca, ortak, arka uç kodu Xamarin.iOS, Xamarin.Android ve Xamarin.Mac uygulamalarla paylaşılabilir; her platformda tüm yerel bir deneyim sunarken.

Bu makale için bir Apple TV, temel bir oluşturma işleminde size adım adım ilerlemenizi sağlayarak Xamarin.tvOS ve Visual Studio kullanarak uygulama oluşturmak için gereken temel kavramları tanıtılacaktır **Hello, tvOS** sayısını sayar uygulamanın bir düğme var tıklanan:

[ ![](hello-tvos-images/run05.png "Örnek uygulamayı çalıştırma")](hello-tvos-images/run05.png)

Aşağıdaki kavramlar şu konulara değineceğiz:

-  **Mac için Visual Studio** – Mac ve onunla Xamarin.tvOS uygulama oluşturma için Visual Studio giriş.
-  **Xamarin.tvOS uygulama anatomisi** – ne bir Xamarin.tvOS uygulama oluşur.
-  **Bir kullanıcı arabirimi oluşturma** – nasıl iOS için Xamarin Tasarımcısı için bir kullanıcı arabirimi oluşturmak üzere kullanın.
-  **Dağıtım ve test** – nasıl çalıştırın ve Simulator tvOS ve gerçek tvOS donanım uygulamanızı test etme.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Mac için Visual Studio'da yeni bir Xamarin.tvOS uygulama başlatma

Yukarıda belirtildiği gibi adlı bir Apple TV uygulama oluşturacağız `Hello-tvOS` , tek bir düğme ve etiket ana ekranına ekler. Düğme tıklatıldığında etiketi tıklattınız sayısı görüntülenir.

Başlamak için şimdi aşağıdakileri yapın:

1. Mac için Visual Studio'yu başlatın:

    [ ![](hello-tvos-images/setup01.png "Mac için Visual Studio")](hello-tvos-images/setup01.png)
2. Tıklayın **yeni çözüm...**  bağlantıyı açmak için ekranın üst sol alt köşesindeki **yeni proje** iletişim kutusu.
3. Seçin **tvOS** > **uygulama** > **Single View uygulaması** tıklatıp **sonraki** düğmesi:

    [ ![](hello-tvos-images/setup02.png "Tek görünüm uygulaması seçin")](hello-tvos-images/setup02.png)
4. Girin `Hello, tvOS` için **App Name**, girin, **kuruluş tanımlayıcı** tıklatıp **sonraki** düğmesi:

    [ ![](hello-tvos-images/setup04.png "Merhaba, tvOS girin")](hello-tvos-images/setup04.png)
5. Girin `Hello_tvOS` için **proje adı** tıklatıp **oluşturma** düğmesi:

    [ ![](hello-tvos-images/setup03.png "Enter HellotvOS")](hello-tvos-images/setup03.png)

Mac için Visual Studio yeni Xamarin.tvOS uygulaması oluşturun ve uygulamanızın çözüme eklenir varsayılan dosyalar görüntüleyin:

 [ ![](hello-tvos-images/project01.png "Varsayılan dosyalar görünümü")](hello-tvos-images/project01.png)

Mac kullanımlar için Visual Studio **çözümleri** ve **projeleri**, Visual Studio mu tam aynı şekilde. Bir veya daha fazla projeleri tutabilen bir kapsayıcı çözümdür; projeleri uygulamaları, destekleme kitaplıkları, sınama uygulamalarını, vb. içerebilir. Bu durumda, Mac için Visual Studio çözüm ve bir uygulama projesi sizin için oluşturdu.

İstiyorsanız, ortak, paylaşılan kodu içeren kitaplık projeleri bir veya daha fazla kod oluşturabilirsiniz. Bu kitaplık projeleri uygulama projesi tarafından tüketilen ya da diğer Xamarin.tvOS uygulaması projeleri ile paylaşılan (veya Xamarin.iOS, Xamarin.Android ve Xamarin.Mac kod türüne göre), standart bir .NET uygulaması oluşturuyorsanız, yaptığınız gibi.

## <a name="anatomy-of-a-xamarintvos-app"></a>Xamarin.tvOS uygulama anatomisi

İle programlama iOS sahibiyseniz, burada benzerlikler çok fark edeceksiniz. Aslında, çok kavramlarını burada çapraz şekilde tvOS 9 iOS 9, bir alt kümesidir.

Bir proje dosyalarında bakalım:

-   `Main.cs` – Bu, uygulamanın ana giriş noktası içerir. Uygulama başlatıldığında, bu çok birinci sınıf ve çalıştırılan yöntemi içerir.
-   `AppDelegate.cs` – Bu dosya işletim sisteminden olaylarını dinleme sorumludur ana uygulama sınıfı içerir.
-   `Info.plist` – Bu dosya, uygulama adı, simgeler vb. gibi uygulama özelliklerini içerir.
-   `ViewController.cs` – Bu, ana pencereyi temsil eder ve onu yaşam döngüsü denetleyen sınıftır.
-   `ViewController.designer.cs` – Bu dosya ana ekranının kullanıcı arabirimi ile tümleştirmenize yardımcı tesisat kodunu içerir.
-  `Main.storyboard` – Ana pencerede ilişkin kullanıcı Arabirimi. Bu dosya oluşturulur ve iOS için Xamarin tasarımcısı tarafından saklanır.

Aşağıdaki bölümlerde, biz bu dosyaların bazıları aracılığıyla hızlı bir göz atalım. Biz bunları daha ayrıntılı olarak daha sonra ele alacağız, ancak bunların temelleri şimdi anlamak için iyi bir fikirdir.

### <a name="maincs"></a>Main.cs

`Main.cs` Dosyasını içeren bir statik `Main` yeni bir Xamarin.tvOS uygulama örneği oluşturur ve sınıfın adı geçen yöntemi, işletim sistemi olayları, bu örnekte olduğu işleyecek `AppDelegate` sınıfı:

```csharp
using UIKit;

namespace Hello_tvOS
{
    public class Application
    {
        // This is the main entry point of the application.
        static void Main (string[] args)
        {
            // if you want to use a different Application Delegate class from "AppDelegate"
            // you can specify it here.
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` Dosyasını içeren bizim `AppDelegate` bizim penceresi oluşturma ve işletim sistemi olaylarını dinleme sorumlu sınıfı:

```csharp
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    // The UIApplicationDelegate for the application. This class is responsible for launching the
    // User Interface of the application, as well as listening (and optionally responding) to application events from iOS.
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        // class-level declarations

        public override UIWindow Window {
            get;
            set;
        }

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Override point for customization after application launch.
            // If not required for your application you can safely delete this method

            return true;
        }

        public override void OnResignActivation (UIApplication application)
        {
            // Invoked when the application is about to move from active to inactive state.
            // This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message)
            // or when the user quits the application and it begins the transition to the background state.
            // Games should use this method to pause the game.
        }

        public override void DidEnterBackground (UIApplication application)
        {
            // Use this method to release shared resources, save user data, invalidate timers and store the application state.
            // If your application supports background execution this method is called instead of WillTerminate when the user quits.
        }

        public override void WillEnterForeground (UIApplication application)
        {
            // Called as part of the transition from background to active state.
            // Here you can undo many of the changes made on entering the background.
        }

        public override void OnActivated (UIApplication application)
        {
            // Restart any tasks that were paused (or not yet started) while the application was inactive.
            // If the application was previously in the background, optionally refresh the user interface.
        }

        public override void WillTerminate (UIApplication application)
        {
            // Called when the application is about to terminate. Save data, if needed. See also DidEnterBackground.
        }
    }
}
```

Bu kod, bir iOS uygulaması önce oluşturuncaya ancak oldukça basittir sürece muhtemelen tanınmayan olur. Önemli satırları inceleyelim.

İlk olarak, bir sınıf düzeyi değişken bildirimi bakalım:

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

`Window` Özelliği ana penceresinde erişim sağlar. tvOS kullanan olarak bilinen *Model View Controller* (MVC) deseni. Genellikle, oluşturduğunuz her penceresi (ve diğer pek çok Windows'da için), yeni görünümler (denetimler) ekleme, vb. için göstermeyi gibi pencerenin yaşam döngüsü sorumlu olduğu bir denetleyicisi yoktur.

Ardından, sahibiz `FinishedLaunching` yöntemi. Bu yöntem, uygulama örneği ve gerçekte uygulama penceresi oluşturma ve görünümü içinde görüntüleme işleminin başlayan sorumludur sonra çalışır. Ek kod burada uygulamamıza kullanıcı Arabiriminde tanımlamak için bir film şeridi kullandığından, gereklidir.

Şablonda gibi sağlanan diğer birçok yöntem vardır `DidEnterBackground` ve `WillEnterForeground`. Uygulama olaylarını uygulamanızda kullanılmayan varsa bunlar güvenle kaldırılabilir.

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController` Bizim ana pencerenin denetleyicisi bir sınıftır. Ana pencerede yaşam döngüsü için sorumlu olduğu anlamına gelir. Yapacağız bu daha sonra ayrıntılı olarak incelemek için şu an için yalnızca bir hızlı onu bakalım:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
    }
}
```

#### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

Ana pencere sınıfı için tasarımcı dosyası şu anda boştur ancak biz bizim kullanıcı arabirimi ile Tasarımcısı iOS oluştururken, otomatik olarak Visual Studio tarafından Mac için doldurulur:

```csharp
using Foundation;

namespace HellotvOS
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

Biz bunlar otomatik olarak yalnızca Visual Studio tarafından Mac için yönetilen olarak genellikle designer dosyaları ile ilgili değildir ve yalnızca biz herhangi penceresi ekleyin veya uygulamamız içinde görüntülemek denetimlere erişim sağlayan gerekli tesisat kodu belirtin.

Xamarin.tvOS uygulamamıza oluşturduk ve bileşenlerinin temel bilgilere sahip olduğumuz göre kullanıcı Arabirimi oluşturma sırasında bakalım.

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>Kullanıcı arabirimi oluşturma

İOS için Xamarin Tasarımcısı Xamarin.tvOS uygulamanız için kullanıcı arabirimi oluşturmak üzere kullanmanız gerekmez, kullanıcı arabirimini doğrudan C# kodundan oluşturulabilir ancak, bu makalenin kapsamı dışındadır. Basitleştirmek amacıyla, biz Tasarımcısı iOS Bu öğreticinin geri kalanında kullanıcı arabirimimizi oluşturmak için kullanırsınız.

UI oluşturmaya başlamak için şimdi çift `Main.storyboard` dosyasını **Çözüm Gezgini** iOS Tasarımcısı düzenlemek üzere açmak için:

[ ![](hello-tvos-images/designer01.png "Çözüm Gezgini'nde Main.storyboard dosyası")](hello-tvos-images/designer01.png)

Bu Tasarımcısı'nı başlatın ve aşağıdaki gibi gerekir:

[ ![](hello-tvos-images/designer02.png "Tasarımcı")](hello-tvos-images/designer02.png)

İOS Tasarımcısı hakkında daha fazla bilgi ve nasıl çalıştığı için başvurmak [iOS için Xamarin Tasarımcısı giriş](~/ios/user-interface/designer/introduction.md) Kılavuzu.

Biz Xamarin.tvOS uygulamamıza tasarım yüzeyine denetimleri ekleme şimdi başlayabilirsiniz.

Aşağıdakileri yapın:

1. Bulun **araç**, tasarım yüzeyine sağında olmalıdır:

    [![](hello-tvos-images/designer03.png "Araç kutusu")](hello-tvos-images/designer03.png)

    Burada bulamazsanız, Gözat **Görünüm > klavye takımı > Araç** görüntülemek için.
2. Sürükleme bir **etiket** gelen **araç** tasarım yüzeyi için:

    [ ![](hello-tvos-images/designer04.png "Bir etiket Araç Kutusu'ndan sürükleyin")](hello-tvos-images/designer04.png)
3. Tıklayın **başlık** özelliğinde **özelliği paneli** düğmenin başlık değiştirip `Hello, tvOS` ve ayarlayın **yazı tipi boyutu** 128 için:

    [ ![](hello-tvos-images/designer05.png "Merhaba, tvOS başlığı ve 128'yazı tipi boyutunu ayarlayın")](hello-tvos-images/designer05.png)
4. Böylece tüm sözcükleri görünür ve pencerenin üst kısmındaki ortalanmış yerleştirin etiketi yeniden boyutlandırmak:

    [ ![](hello-tvos-images/designer06.png "Yeniden boyutlandırma ve etiket Merkezi")](hello-tvos-images/designer06.png)
5. Tasarlandığı gibi görünmesi etiketi şimdi kaydının konumunu kısıtlı gerekir. Ekran boyutundan bağımsız olarak. Bunu yapmak için etiketi kadar tıklayın *T şeklinde tanıtıcı* görünür:

    [ ![](hello-tvos-images/designer07.png "T şeklinde tanıtıcısı")](hello-tvos-images/designer07.png)
6. Etiket yatay sınırlamak için merkezi kare seçin ve dikey olarak kesikli çizgiye sürükleyin:

    [ ![](hello-tvos-images/designer08.png "Merkezi kare seçin")](hello-tvos-images/designer08zoom.png)

     Etiket turuncu etkinleştirmeniz gerekir.
7. Etiketin üst T tutamaç seçin ve pencerenin üst kenarına sürükleyin:

    [ ![](hello-tvos-images/designer09.png "Pencerenin üst kenarına tutamacını sürükleyin")](hello-tvos-images/designer09.png)
8. Bundan sonra Genişlik ve yükseklik öğesini *kemik tanıtıcı* aşağıda gösterildiği gibi:

    [ ![](hello-tvos-images/designer10.png "Genişlik ve yükseklik kemik tanıtıcıları")](hello-tvos-images/designer10.png)

     Zaman her *kemik tanıtıcı* olan tıklandığında, genişlik ve yükseklik sırasıyla sabit boyutlarını ayarlamak için seçin.
9. Tamamlandığında, kısıtlamalar özellikleri paneli düzeni sekmesinde benzer görünmelidir:

    [ ![](hello-tvos-images/designer11.png "Örnek kısıtlamaları")](hello-tvos-images/designer11.png)
8. Sürükleme bir **düğmesini** gelen **araç** ve etiketin altında yerleştirin.
9. Tıklayın **başlık** özelliğinde **özelliği paneli** düğmenin başlık değiştirip `Click Me`:

    [ ![](hello-tvos-images/designer12.png "Bana tıklatın düğmeleri başlığını değiştirme")](hello-tvos-images/designer12.png)
10. 5-8 tvOS penceresinde düğmesini sınırlamak için yukarıdaki adımları yineleyin. Ancak, pencerenin üst kısmındaki (olduğu gibi #7. adım) için T-tanıtıcı sürükleme yerine etiketinin altına sürükleyin:

    [ ![](hello-tvos-images/designer14.png "Düğme sınırlamak")](hello-tvos-images/designer14.png)
11. Başka bir etiket düğmesinin altındaki sürükleyin, aynı genişliğe kümesi ve ilk etiket olması için boyut kendi **hizalama** için **Center**:

    [ ![](hello-tvos-images/designer15.png "Başka bir etiket düğmesinin altındaki sürükleyin, ilk etiketi aynı genişliğe ve merkezine kendi hizalamasını ayarlama boyutu")](hello-tvos-images/designer15.png)
12. İlk etiket ve düğmesi gibi merkezi ve konum ve boyut sabitlemek için bu etiket ayarlayın:

    [ ![](hello-tvos-images/designer16.png "PIN etiket konum ve boyut içinde")](hello-tvos-images/designer16.png)
13. Kullanıcı arabirimine yaptığınız değişiklikleri kaydedin.

Yeniden boyutlandırma ve taşıma geçici denetimleri gibi tasarımcı, temel alan yararlı ek ipuçları sunar için fark etmiş [Apple TV İnsan Arabirimi yönergelerine](https://developer.apple.com/tvos/human-interface-guidelines/). Bu yönergeler, Apple TV kullanıcılar için tanıdık bir görünüm ve kullanımında içeren yüksek kaliteli uygulamalar oluşturmanıza yardımcı olur.

Bakarsanız **belge anahattı** bölümünde, Düzen ve hiyerarşi bizim kullanıcı arabirimi öğelerinin nasıl gösterileceğini dikkat edin:

[ ![](hello-tvos-images/designer17.png "Belge Anahattı bölümü")](hello-tvos-images/designer17.png)

Buradan, düzenlemek veya gerekirse kullanıcı Arabirimi öğeleri yeniden sıralamak için sürükleyin öğe seçebilirsiniz. Örneğin, bir kullanıcı Arabirimi öğesi başka bir öğe tarafından kapsanan, pencerenin en üst öğede yapmak için listenin altına sürükleyebilirsiniz.

Oluşturulan kullanıcı Arabirimimizi sahibiz, böylece Xamarin.tvOS erişmek ve C# kodunda etkileşime kullanıcı Arabirimi öğeleri kullanıma gerekir.

### <a name="accessing-the-controls-in-the-code-behind"></a>Arkasındaki kodda denetimlerine erişme

Kod iOS Tasarımcısından ekledikten denetimlere erişmek için başlıca iki yolu vardır:

* Olay işleyici denetim oluşturuluyor.
* Biz daha sonra başvurabilmek denetimi bir ad verip.

Ne zaman bunlardan eklenir, içindeki parçalı sınıf `ViewController.designer.cs` değişiklikleri yansıtacak şekilde güncelleştirilir. Bu görünüm denetleyicisini denetimlerinde erişmenize olanak tanır.

### <a name="creating-an-event-handler"></a>Olay işleyici oluşturma

Bu örnek uygulamasında düğmesine tıklandığında istiyoruz _bir şey_ yapılmasını olay işleyicisi düğmesine belirli bir olay eklenmesi gerekiyor. Bunu ayarlamak için aşağıdakileri yapın:

1. Xamarin iOS Tasarımcı'da, görünüm denetleyicisinde düğmesini seçin.
2. Özellikler defterinde seçin **olayları** sekmesi:

    [![](hello-tvos-images/event1.png "Olaylar sekmesi")](hello-tvos-images/event1.png)
3. TouchUpInside olayı bulun ve adlı bir olay işleyicisi verin `Clicked`:

    [![](hello-tvos-images/event2.png "TouchUpInside olayı")](hello-tvos-images/event2.png)
4. Bastığınızda **Enter**, **ViewController**.cs dosyası açılır, kodda, olay işleyicisi için konumları önerme. Ok tuşlarını klavyenizde konumu belirtmek için kullanın:

    [![](hello-tvos-images/event3.png "Konumu ayarlama")](hello-tvos-images/event3.png)
5. Aşağıda gösterildiği gibi bu kısmi yöntemi oluşturacak:

    [![](hello-tvos-images/event4.png "Kısmi yöntemi")](hello-tvos-images/event4.png)

Biz şimdi işlevi düğme izin vermek için biraz kod eklemeye başlamak hazırsınız.

### <a name="naming-a-control"></a>Bir denetim adlandırma

Düğme tıklatıldığında etiketi güncelleştirmelidir tıklama sayısına göre. Bunu yapmak için size bir kod etiketinde erişmek gerekir. Bu, bir ad verip tarafından gerçekleştirilir. Aşağıdakileri yapın:

1. Film şeridi açın ve görünüm denetleyicisini altındaki etiketi seçin.
2. Özellikler defterinde seçin **pencere öğesi** sekmesi:

    [![](hello-tvos-images/name1.png "Pencere öğesi sekmesini seçin")](hello-tvos-images/name1.png)
3. Altında **kimliği > adı**, ekleme `ClickedLabel`:

    [![](hello-tvos-images/name2.png "ClickedLabel ayarlama")](hello-tvos-images/name2.png)

Biz şimdi etiketi güncelleştirme başlamaya hazırsınız!

### <a name="how-controls-are-accessed"></a>Denetimlerin nasıl erişilir

Seçerseniz `ViewController.designer.cs` içinde **Çözüm Gezgini** görmeye devam nasıl `ClickedLabel` etiket ve `Clicked` olay işleyicisi eşlenmiş bir **çıkışı** ve  **Eylem** C#:

[ ![](hello-tvos-images/accesscontrol.png "Çıkışlar ve eylemleri")](hello-tvos-images/accesscontrol.png)

Ayrıca, fark edebilirsiniz `ViewController.designer.cs` bir parçalı sınıf, böylelikle Mac için Visual Studio değiştirmek yok `ViewController.cs` hangi sınıfa yapmış olduğunuz değişiklikleri üzerine.

Bu şekilde kullanıcı Arabirimi öğeleri gösterme görünüm denetleyiciye erişmesine izin verir.

Normalde hiçbir zaman açmanız gerekecek `ViewController.designer.cs` yalnızca eğitim amacıyla kendiniz burada verildi.

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>Kod yazma

Oluşturulan bizim kullanıcı arabirimi ve kodu aracılığıyla kullanıma sunulan kullanıcı Arabirimi öğeleri ile **çıkışlar** ve **Eylemler**, biz program işlevselliği vermek üzere kod yazmak son hazırsınız.

Her zaman ilk düğmesine tıklandığında, uygulamamız, kaç kez düğmesine tıklanana göstermek için etiketimizi güncelleştirmek için yapacağız. Bunu başarmak için açmak ihtiyacımız `ViewController.cs` dosyasını çift tıklatarak düzenleme için **çözüm paneli**:

[ ![](hello-tvos-images/code01.png "Çözüm paneli")](hello-tvos-images/code01.png)

İlk olarak, bir sınıf düzeyi değişken oluşturmak ihtiyacımız bizim `ViewController` gelmiş tıklama sayısını izlemek için sınıf. Sınıf tanımını düzenlemek ve aşağıdaki gibi yapar:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

Ardından, aynı sınıfta (`ViewController`), geçersiz kılmak ihtiyacımız **ViewDidLoad** yöntemi ve ilk ileti etiketimizi için ayarlamak üzere bazı kodu ekleyin:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

Kullanmak ihtiyacımız `ViewDidLoad `, gibi başka bir yöntem yerine `Initialize`, çünkü `ViewDidLoad ` çağrılır *sonra* işletim sistemi yüklenir ve kullanıcı arabiriminden örneği `.storyboard` dosya. Etiket denetimi önce erişmeye çalıştığında `.storyboard` dosya tam olarak yüklenir ve örneği, elde edecekleri bir `NullReferenceException` hata olduğundan etiket denetimi henüz oluşturulmamış.

Ardından, biz düğmesinin tıklatıldığında kullanıcıya yanıt vermek amacıyla kod eklemeniz gerekir. Kısmi için aşağıdakileri ekleyin, oluşturduğumuz sınıfı:

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

Bu kod kullanıcı bizim düğmesine tıklar dilediğiniz zaman çağrılır.

Her yerde biz şimdi oluşturmak ve Xamarin.tvOS uygulamamızı test etmek hazır olursunuz.

## <a name="testing-the-application"></a>Uygulamayı Test Etme

Derleme ve beklendiği gibi çalışacağından emin olmak için uygulamamızı çalıştırmak için zaman yapılır. Biz oluşturup tümü tek bir adımda çalıştırın ya da biz bunu çalıştıran olmadan oluşturabilirsiniz.

Biz bir uygulama oluşturmak zaman, biz ne tür bir yapı istiyoruz seçebilirsiniz:

-   **Hata ayıklama** – bir hata ayıklama derlemesi derlenir bir '' uygulama çalışırken neler hata ayıklama kurmamızı sağlayan ek meta veri dosyasıyla (uygulama).
-   **Yayın** – yayın derlemesi da oluşturur bir '' dosyası, ancak içermez hata ayıklama bilgileri, böylece daha küçüktür ve daha hızlı çalıştırır.  

Yapıdan türünü seçebilirsiniz **yapılandırma Seçici** Mac ekran için Visual Studio üst sol alt köşesindeki:

[ ![](hello-tvos-images/run01.png "Yapı türünü seçin")](hello-tvos-images/run01.png)

### <a name="building-the-application"></a>Uygulama Oluşturma

Bu örnekte, biz yalnızca hata ayıklama derlemesi istiyorsanız, sağlandığından emin olun **hata ayıklama** seçilir. Şimdi uygulamamız ya basarak ilk yapı **⌘B**, veya **yapı** menüsünde seçin **yapı tüm**.

Herhangi bir hata var. doğru ise, görürsünüz bir **yapı başarılı** Mac'ın durum çubuğu için Visual Studio iletisi. Hatalar oluşursa, projenizin gözden geçirin ve adımları doğru uyguladıysanız emin olun. Kodunuzu (her ikisi de Xcode ve Mac için Visual Studio) öğreticide kodu eşleştiğini onaylayan başlatın.

### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Uygulamayı çalıştırmak için şu üç seçeneğiniz vardır:

-  Tuşuna **⌘ + Enter**.
-  Gelen **çalıştırmak** menüsünde seçin **hata ayıklama**.
-  Tıklatın **Yürüt** Visual Studio Mac araç çubuğu düğmesini (yukarıdaki **Çözüm Gezgini**).

(Bu zaten oluşturulduğunu kurmadı varsa) uygulaması oluşturacaksınız, hata ayıklama modunda Simulator tvOS başlangıç başlatılır ve uygulamayı başlatın ve buna ait ana arabirimi penceresini görüntüleyin:

[ ![Örnek uygulama giriş ekranı](hello-tvos-images/run03.png)](hello-tvos-images/run03.png)

Gelen **donanım** menüsünü seçin **Apple TV uzak Göster** simulator denetimi.

[ ![](hello-tvos-images/run04.png "Göster Apple TV uzaktan seçin")](hello-tvos-images/run04.png)

Birkaç kez etiketi düğmesini Simulator'ın uzaktan kullanarak count ile güncelleştirilmesi gerekir:

[ ![](hello-tvos-images/run05.png "Güncelleştirilmiş sayısı etiketle")](hello-tvos-images/run05.png)

Tebrikler! Biz başından başlayarak burada çok kapsamdaki, ancak bu öğretici baştan izlediyseniz, düz bir bunları oluşturmak için kullanılan araçlar yanı sıra Xamarin.tvOS uygulama bileşenlerinin anlayış şimdi olmalıdır.

## <a name="where-to-next"></a>Sonraki nerede?

Apple TV uygulamaları sunar birkaç sınar kullanıcı, (Bu, kullanıcının el odada değil arasında değil) arabirimi ve sınırlamaları tvOS arasındaki bağlantıyı kes nedeniyle geliştirme uygulama boyutu ve depolama yerleştirir.

Sonuç olarak, yüksek oranda, okuma Xamarin.tvOS uygulamanın tasarım atlama önce aşağıdaki belgeleri öneririz:

- [TvOS 9 giriş](~/ios/tvos/platform/tvos9.md) – bu makalede Xamarin.tvOS geliştiriciler için tüm yeni ve değiştirilen API'ler ve tvOS 9 kullanılabilir özellikler sunar.
- [Gezinti ve odak çalışma](~/ios/tvos/app-fundamentals/navigation-focus.md) – Xamarin.tvOS uygulamanızdaki kullanıcıların değil etkileşim burada bunlar dokunun görüntüleri cihazın ekranında, ancak dolaylı olarak çapraz Siri uzaktan kullanarak yer onun arabirimiyle doğrudan iOS. Bu makalede kavramını odak ve Xamarin.tvOS uygulamanın kullanıcı arabiriminde gezinme işlemek için nasıl kullanıldığı ele alınmaktadır.
- [Siri uzak ve Bluetooth denetleyicileri](~/ios/tvos/platform/remote-bluetooth.md) – kullanıcılar Apple TV ve Xamarin.tvOS uygulamanız ile etkileşim, dahil edilen Siri uzaktan ana yol. Uygulamanızı bir oyun ise, isteğe bağlı olarak, yapılan için 3. taraf için destek iOS (MFI) Bluetooth oyun denetleyicileri, uygulamanızda oluşturabilirsiniz. Bu makalede, yeni Siri uzak ve Bluetooth oyun denetleyicileri Xamarin.tvOS uygulamalarınızda destekleme kapsar.
- [Kaynakları ve veri depolama](~/ios/tvos/app-fundamentals/resources-data-storage.md) – iOS cihazları, tvOS uygulamaları için sürekli, yerel depolama yeni Apple TV sağlamaz. Sonuç olarak, Xamarin.tvOS uygulamanız (örneğin, kullanıcı tercihlerini) bilgilerinin sürdürülmesi gerekiyorsa depolamak ve bu verileri iCloud almak gerekir. Bu makalede, kaynakları ve kalıcı veri depolamayı Xamarin.tvOS uygulamasında çalışmak yer almaktadır.
- [Simgeler ve görüntüleri çalışma](~/ios/tvos/app-fundamentals/icons-images.md) – oluşturma büyüleyici simgeler ve görüntülerin olan Apple TV uygulamalarınız için derinlikli kullanıcı deneyimini geliştirmek için önemli bir bölümüdür. Bu kılavuz oluşturmak ve Xamarin.tvOS uygulamalarınız için gerekli grafik varlıkları eklemek için gerekli olan adımları kapsar.
- [Kullanıcı arabirimi](~/ios/tvos/user-interface/index.md) – kullanıcı arabirimi (UI) denetimler de dahil olmak üzere genel kullanıcı deneyimini (UX) kapsamı, Xcode'nın arabirimi oluşturucusu ve UX tasarım ilkeleri Xamarin.tvOS ile çalışırken kullanın.
- [Dağıtım ve test](~/ios/tvos/deploy-test/index.md) – Bu bölüm yanı sıra bir uygulama dağıtmak için test etmek için kullanılan konuları kapsar. Burada konuların hata ayıklama, sınayıcılar ve Apple TV App Store için bir uygulamanın nasıl yayımlanacağını dağıtım için kullanılan araçları gibi nesnelerdir.

Xamarin.tvOS çalışma sorunları alıyorsanız, lütfen bakın bizim [sorun giderme](~/ios/tvos/troubleshooting.md) bir listesi için belgelere bilmeniz sorunlar ve çözümleri.

## <a name="summary"></a>Özet

Bu makalede tvOS uygulama basit bir Hello oluşturarak Mac için tvOS uygulamalarını Visual Studio ile geliştirme için hızlı bir başlangıç sağlanır. Sağlama, arabirimi oluşturma, tvOS için kodlama ve tvOS simulator'da sınama tvOS aygıt ile ilgili temel bilgiler ele.


## <a name="related-links"></a>İlgili bağlantılar

- [tvOS örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin (video) ile tvOS için uygulama oluşturma](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
