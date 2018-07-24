---
title: Hello, iOS – derinlemesine bakış
description: Bu belge Merhaba, daha kapsamlı bir bakış alır, iOS örnek uygulaması, mimarisi, kullanıcı arabirimi, içerik görünümü hiyerarşisi, test, dağıtım ve daha fazla düşünüyor.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 16920f27a1830dc6a3ab1a3cb0a267eb3b1d90ea
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203029"
---
# <a name="hello-ios--deep-dive"></a>Hello, iOS – derinlemesine bakış

Hızlı Başlangıç Kılavuzu, oluşturmak ve temel bir Xamarin.iOS uygulaması çalıştıran kullanıma sunuldu. Artık daha karmaşık programlar oluşturmanıza yardımcı olacak iOS uygulamalarını nasıl çalıştığını, daha derin bir anlayış geliştirmek için zamanı geldi. Bu kılavuzda adımları gözden geçirmeleri, Hello, iOS uygulama geliştirmenin temel kavramlarını anlama etkinleştirmek için iOS gözden geçirme.

Bu kılavuz, bir tek ekranlı iOS uygulaması oluşturmak için gereken bilgi ve becerilerinizi geliştirmenize yardımcı olur. Aracılığıyla çalıştıktan sonra bir Xamarin.iOS uygulaması ve bunlar birbirine nasıl uyduğunu farklı bölümlerini bir anlamış olmanız gerekir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Mac için Visual Studio'ya giriş

Mac için Visual Studio Visual Studio ve XCode özellikleri bir araya getiren ücretsiz, açık kaynaklı bir ıde'dir. Bu tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir bütünleştirilmiş kod tarayıcı, kaynak kodu tümleştirmesi ve diğer özellikleri. Bu kılavuz temel bazı Visual Studio Mac özellikleri içermektedir, ancak Mac için Visual Studio yeniyseniz, kullanıma [Mac için Visual Studio](https://docs.microsoft.com/visualstudio/mac/) belgeleri.

Mac için Visual Studio aşağıdaki koda düzenleme Visual Studio uygulaması *çözümleri* ve *projeleri*. Bir çözüm bir veya daha fazla proje tutan bir kapsayıcıdır. Bir proje (örneğin, iOS veya Android) bir uygulama, bir destek kitaplığı, bir test uygulaması ve daha fazla olabilir. Kullanarak yeni bir iPhone proje Phoneword uygulamada eklendikten sonra **tek görünüm uygulaması** şablonu. İlk çözüm şöyle Aranan:

![](hello-ios-deepdive-images/image30.png "İlk çözüm görüntüsü")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Visual Studio'ya giriş

Visual Studio, Microsoft güçlü bir ıde'dir. Bu tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir bütünleştirilmiş kod tarayıcı, kaynak kodu tümleştirmesi ve diğer özellikleri. Bu kılavuz, eklenti Xamarin ile bazı temel Visual Studio özellikleri tanıtır.

Visual Studio kod halinde düzenler _çözümleri_ ve *projeleri*. Bir çözüm bir veya daha fazla proje tutan bir kapsayıcıdır. Bir proje (örneğin, iOS veya Android) bir uygulama, bir destek kitaplığı, bir test uygulaması ve daha fazla olabilir. Kullanarak yeni bir iPhone proje Phoneword uygulamada eklendikten sonra **tek görünüm uygulaması** şablonu. İlk çözüm şöyle Aranan:

![](hello-ios-deepdive-images/vs-image30.png "İlk çözüm görüntüsü")

-----

## <a name="anatomy-of-a-xamarinios-application"></a>Bir Xamarin.iOS uygulaması anatomisi

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Soldaki *çözüm bölmesi*, dizin yapısını içerir ve tüm dosyaları çözümle ilişkili:

![](hello-ios-deepdive-images/image31.png "Dizin yapısı ve çözümle ilişkili tüm dosyaları içeren çözüm bölmesi")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sağ tarafta olduğunu *çözüm bölmesi*, dizin yapısını içerir ve tüm dosyaları çözümle ilişkili:

![](hello-ios-deepdive-images/vs-image31.png "Dizin yapısı ve çözümle ilişkili tüm dosyaları içeren çözüm bölmesi")

-----

İçinde [Hello, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) izlenecek yol, oluşturduğunuz adlı çözüm **Phoneword** ve bir iOS projesi - yerleştirilen **Phoneword_iOS** - içindeki. Proje içinde bu öğeler şunlardır:

-  **Başvuruları** -derlemek ve uygulamayı çalıştırmak için gerekli derlemelerini içerir. .NET derlemesine ilişkin başvurular gibi görmek için directory genişletin [sistem](http://msdn.microsoft.com/library/system%28v=vs.110%29.aspx) , System.Core, ve [System.Xml](http://msdn.microsoft.com/library/system.xml%28v=vs.110%29.aspx) , Xamarin'in Xamarin.iOS derlemesine bir başvuru yanı sıra.
-  **Paketleri** -paketler dizini hazır NuGet paketlerini içerir.
-  **Kaynakları** -kaynak klasör diğer medya depolar.
-  **Main.cs** – bu uygulamanın ana girdi noktası içerir. Ana uygulama sınıfının adı uygulamayı başlatmak için `AppDelegate`, geçirilir.
-  **AppDelegate.cs** – bu dosya ana uygulama sınıf içerir ve pencere oluşturma, kullanıcı arabirimini oluşturmaya ve işletim sisteminden olayları dinleme sorumludur.
-  **Main.Storyboard** -görsel taslak görsel tasarımını uygulamanın kullanıcı arabirimini içerir. Görsel taslak dosyaları iOS Designer adlı bir grafik Düzenleyicisi içinde açın.
-  **ViewController.cs** – görünüm denetleyicisi güç katan bir kullanıcı görür ve kümelerden ekran (View). Görünüm denetleyicisi, kullanıcı ve görünümü arasındaki etkileşimler işlenmesinden sorumludur.
-  **ViewController.designer.cs** – `designer.cs` görünümünde denetimleri ve görünüm denetleyicisi, kendi kod gösterimleri arasında bağlantılı olarak hizmet veren bir otomatik olarak oluşturulan dosya. Bu bir iç tesisat dosya olduğundan, IDE el ile yapılan tüm değişiklikler ve çoğu zaman bu dosyayı göz ardı edilebilir üzerine yazar. Görsel Tasarımcı ve yedekleme kod arasındaki ilişkiyi daha fazla bilgi için [iOS Designer giriş](~/ios/user-interface/designer/introduction.md) Kılavuzu.
-  **Info.plist** – `Info.plist` olan uygulama adı, simgeler, başlatma görüntüleri ve diğer uygulama özelliklerini ayarladığınız yerdir. Bu güçlü bir dosyasıdır ve bu kapsamlı bir giriş kullanılabilir [özelliği listeleriyle çalışma](~/ios/app-fundamentals/property-lists.md) Kılavuzu.
-  **Entitlements.plist** -bize uygulama belirtin yetkilendirmeler özellik listesini sağlayan *özellikleri* (App Store teknolojileri olarak da bilinir) iCloud PassKit ve daha fazlası gibi. Daha fazla bilgi `Entitlements.plist` bulunabilir [özelliği listeleriyle çalışma](~/ios/app-fundamentals/property-lists.md) Kılavuzu. Yetkilendirmeler genel bir giriş için bkz [cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

## <a name="architecture-and-app-fundamentals"></a>Mimari ve uygulama temelleri

Bir iOS uygulaması kullanıcı arabirimi yüklemeden önce iki şeyi yerinde olması gerekir. İlk olarak, uygulama tanımlamak gerek duyduğu bir *giriş noktası* – uygulama işlemi belleğe yüklendiğinde çalışan ilk kod. İkinci olarak, birçok farklı uygulama olayları işlemek ve işletim sistemi ile etkileşim kurmak için bir sınıf tanımlamak gerekir.

Bu bölümde, aşağıdaki diyagramda gösterildiği ilişkileri studies:

[![](hello-ios-deepdive-images/image32.png "Mimari ve uygulama temelleri ilişkileri Bu diyagramda gösterilmiştir.")](hello-ios-deepdive-images/image32.png#lightbox)

Şimdi başından itibaren başlatmak ve uygulama başlangıcında ne olacağını öğrenin.

### <a name="main"></a>Ana

Bir iOS uygulamasının ana giriş noktasıdır `Main.cs` dosya. `Main.cs` Yeni bir Xamarin.iOS uygulama örneği oluşturur ve adını aktaran bir statik Main yöntemine içeren *uygulama temsilcisi* işletim sistemi olayları işleyen sınıfı. Statik şablon kodunu `Main` yöntemi aşağıda görünür:

```csharp
using System;
using UIKit;

namespace Phoneword_iOS
{
    public class Application
    {
        static void Main (string[] args)
        {
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="application-delegate"></a>Uygulama temsilcisi

İos'ta, *uygulama temsilcisi* sınıfı sistem olaylarını işler; bu sınıfın içinde barındırılmasından `AppDelegate.cs`. `AppDelegate` Sınıfı uygulama yönetir *penceresi*. Tek bir örneğini penceredir `UIWindow` kullanıcı arabirimi için bir kapsayıcı görevi gören sınıf. Varsayılan olarak, uygulama içeriğinin yükleneceği tek penceresinden alır ve pencerenin bağlı olduğu bir *ekran* (tek `UIScreen` örneği) fiziksel boyutlarını eşleşen dikdörtgen sağlar cihaz ekranı.

*AppDelegate* ayrıca sistem güncelleştirmelerini uygulama başlatma tamamlandığında veya bellek düşük olduğunda gibi önemli uygulama olaylarıyla ilgili abone sorumludur.

AppDelegate şablon kodunu aşağıda sunulmuştur:

```csharp
using System;
using Foundation;
using UIKit;

namespace Phoneword_iOS
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        public override UIWindow Window {
            get;
            set;
        }

        ...
    }
}
```

Uygulama penceresi tanımladığı sonra kullanıcı arabirimi yüklenirken başlayabilirsiniz. Sonraki bölümde kullanıcı Arabirimi oluşturma keşfediyor.

## <a name="user-interface"></a>Kullanıcı arabirimi

Bir mağaza gibi bir iOS uygulamasının kullanıcı arabirimidir: uygulama bir pencere genellikle alır ancak bunu penceresi ile birçok nesnede gerekir ve nesneleri ve düzenlemeleri ne uygulamayı görüntülemek istediği bağlı olarak değiştirilebilir dolabilir. Bu senaryoda - kullanıcının gördüğü şeyler - nesneleri görünümleri çağrılır. Bir uygulamada tek bir ekran oluşturmak için görünümlerini birbirleriyle üzerine dizilir bir *içerik hiyerarşisini görüntüle*, ve hiyerarşisini tek bir görünüm denetleyicisi tarafından yönetilir. Birden çok ekrana uygulamalarla birden çok içerik görünümü, her biri kendi görünüm denetleyicisi hiyerarşilerinizin ve uygulama görünümleri, kullanıcı farklı bir içerik görünümü ekranda tabanlı hiyerarşi oluşturmak için pencere yerleştirir.

Bu bölümde, görünümler, içerik görünümü hiyerarşileri ve iOS Designer açıklayarak kullanıcı arabirimine türlerine geçiyor.

### <a name="ios-designer-and-storyboards"></a>iOS Tasarımcısı ile görsel Taslaklar

İOS Tasarımcısı Xamarin içinde kullanıcı arabirimleri oluşturmaya yönelik görsel bir araçtır. Tasarımcı, aşağıdaki ekran görüntüsüne benzeyen bir görünümünü açar tüm film şeridi (.storyboard) dosyasını çift tıklayarak başlatılabilir:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image33.png "iOS Tasarımcısı arabirimi")

A *film şeridi* bizim uygulamanın ekranlarını yanı sıra geçişleri ve ilişkileri ve ekranlar arasında görsel tasarımını içeren bir dosyadır. Bir film şeridi içinde bir uygulamanın ekran temsili olarak adlandırılan bir _Sahne_. Her görünüm, görünüm denetleyicisi ve Görünüm (içerik görünümü hiyerarşi) yönettiği yığını temsil eder. Yeni bir **tek görünüm uygulaması** Mac için Visual Studio otomatik olarak adlandırılan bir görsel taslak dosyası oluşturur, proje şablondan oluşturulan `Main.storyboard` ve tarafından ekran görüntüsünde gösterildiği gibi tek bir Sahne ile doldurur Aşağıda:

![](hello-ios-deepdive-images/image34.png "Mac için Visual Studio otomatik olarak Main.storyboard adlı bir görsel taslak dosyası oluşturur ve tek bir Sahne ile doldurur")

Görsel taslak ekranın alt kısmındaki siyah bir çubuk, görünüm için Görünüm denetleyicisi seçmek için seçilebilir. Görünüm denetleyicisi örneğidir `UIViewController` içerik görünümü hiyerarşi için yedekleme kod içeren sınıf. Bu görünüm Denetleyicisi Özellikleri görüntülenebilir ve içinde ayarlamak **özellikler panelinde**ekran aşağıda gösterildiği gibi:

![](hello-ios-deepdive-images/image35.png "Özellikler bölmesi")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image33.png "iOS Tasarımcısı arabirimi")

A *film şeridi* bizim uygulamanın ekranlarını yanı sıra geçişleri ve ilişkileri ve ekranlar arasında görsel tasarımını içeren bir dosyadır. Bir film şeridi içinde bir uygulamanın ekran temsili olarak adlandırılan bir _Sahne_. Her görünüm, görünüm denetleyicisi ve Görünüm (içerik görünümü hiyerarşi) yönettiği yığını temsil eder. Yeni bir **tek görünüm uygulaması** Visual Studio otomatik olarak adlandırılan bir görsel taslak dosyası oluşturur, proje şablondan oluşturulan `Main.storyboard` ve aşağıdaki ekran gösterildiği gibi tek bir Sahne ile doldurur:

![](hello-ios-deepdive-images/vs-image34.png "Visual Studio otomatik olarak Main.storyboard adlı bir görsel taslak dosyası oluşturur ve tek bir Sahne ile doldurur")

Görsel taslak ekranın alt kısmındaki çubuğu, görünüm için Görünüm denetleyicisi seçmek için seçilebilir. Görünüm denetleyicisi örneğidir `UIViewController` içerik görünümü hiyerarşi için yedekleme kod içeren sınıf. Bu görünüm Denetleyicisi Özellikleri görüntülenebilir ve içinde ayarlamak **Özellikler bölmesinde**ekran aşağıda gösterildiği gibi:

![](hello-ios-deepdive-images/vs-image35.png "Özellikler bölmesi")

-----

_Görünümü_ sahnenin beyaz bölümü içinde tıklayarak seçilebilir. Görünüm örneğidir `UIView` ekranın bir alan tanımlar ve alanındaki içeriği ile çalışmak için arabirimleri sağlar sınıfını. Tek bir varsayılan görünümü olan *kök görünümü* , tüm cihaz ekranı doldurur.

Ekran aşağıdaki gösterildiği gibi sahnenin soluna bir bayrak simgesiyle gösteren gri ok şöyledir:

 [![](hello-ios-deepdive-images/image37.png "Bir bayrak simgesiyle gösteren gri ok")](hello-ios-deepdive-images/image37.png#lightbox)

Adlı bir görsel taslak geçiş gösteren gri ok temsil eden bir *Segue* ("seg yönlü" olarak okunur). Bu Segue hiçbir kaynak olduğundan, çağrıldığı bir *Sourceless ü*. Sourceless ü, görünümleri yüklenen uygulama başlangıcında bizim uygulamanın penceresine ilk Sahne işaret eder. Sahne ve görünümleri içindeki uygulama yüklediğinde kullanıcının gördüğü ilk şey olacaktır.

Gelen bir kullanıcı arabirimi oluşturma sırasında ek görünümler sürüklenebilir **araç kutusu** tasarım yüzeyinde ekran aşağıda gösterildiği gibi ana görünüme sürükleyin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image38.png "Ek görünümler araç kutusundan tasarım yüzeyine ana görünümünde üzerine sürüklenebilen")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image38.png "Ek görünümler araç kutusundan tasarım yüzeyine ana görünümünde üzerine sürüklenebilen")

-----

Bu ek görünümler adlı *Subviews*. Kök görünümü ve Subviews birlikte parçası olan bir *içerik hiyerarşisini görüntüle* tarafından yönetilen `ViewController`. Sahnedeki tüm öğelerin anahat içinde inceleyerek görüntülenebilir **belge anahattı** paneli:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image39.png "Belge Anahattı paneli")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image39.png "Belge Anahattı paneli")

-----

Aşağıdaki diyagramda Subviews vurgulanmıştır:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image40.png "Diyagramda Subviews vurgulanır")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image40.png "Diyagramda Subviews vurgulanır")

-----

Sonraki bölümde bu Sahne tarafından temsil edilen içerik görünümü hiyerarşiyi ayırır.

## <a name="content-view-hierarchy"></a>İçerik görünümü hiyerarşisi

A _içerik hiyerarşisini görüntüle_ Aşağıdaki diyagramda gösterildiği gibi görünümler ve tek bir görünüm denetleyicisi tarafından yönetilen Subviews yığını olan:

 [![](hello-ios-deepdive-images/image41.png "İçerik görünümü hiyerarşisi")](hello-ios-deepdive-images/image41.png#lightbox)

İçerik görünümü hiyerarşisini yapabiliyoruz bizim `ViewController` sarı görünüm bölümünde geçici olarak kök görünümü arka plan rengini değiştirerek görmek daha kolay, **özellikler panelinde**ekran aşağıda gösterildiği gibi:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image42.png "Özellikleri paneli görünüm bölümünde sarı kök görünümü arka plan rengini değiştirme")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image42.png "Özellikleri paneli görünüm bölümünde sarı kök görünümü arka plan rengini değiştirme")

-----

Aşağıdaki diyagramda, cihaz ekran için kullanıcı arabirimi Getir penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkiler gösterilmektedir:

 [![](hello-ios-deepdive-images/image43.png "Pencere, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkileri")](hello-ios-deepdive-images/image43.png#lightbox)

Sonraki bölümde kodda görünümlerle çalışma ve görünüm denetleyicileri ve görünüm yaşam döngüsü kullanarak, kullanıcı etkileşimi için programlamayı öğrenin anlatılmaktadır.

## <a name="view-controllers-and-the-view-lifecycle"></a>Görünüm denetleyicilerini ve görünüm yaşam döngüsü

Her içerik hiyerarşisini görüntüle power kullanıcı etkileşimine karşılık gelen bir görünüm denetleyicisi vardır. Görünüm denetleyicisi rolünü içerik hiyerarşisini görüntüle görünümlerde yönetmektir. Görünüm denetleyicisi içerik görünümü hiyerarşinin parçası değil ve arabiriminde bir öğe değil. Bunun yerine, kullanıcının etkileşim ekrandaki nesnelere güç katan kodu sağlar.

### <a name="view-controllers-and-storyboards"></a>Görünüm denetleyicilerini ve görsel Taslaklar

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Görünüm denetleyicisi içinde bir film şeridini Sahne sayfanın alt kısmında bir çubuk olarak temsil edilir. Özelliklerini açılan View Controller'ı seçerek **özellikler panelinde**:

![](hello-ios-deepdive-images/image44.png "Görünüm denetleyicisi seçerek Özellikler bölmesinde özelliklerini getirir.")

İçerik görünümü bu görünüm tarafından temsil edilen hiyerarşi için özel bir görünüm denetleyicisi sınıfını düzenleyerek ayarlanabilir **sınıfı** özelliğinde **kimlik** bölümünü **özellikler panelinde**. Örneğin, bizim **Phoneword** uygulama kümeleri `ViewController` ekran aşağıda gösterildiği gibi ilk ekran için Görünüm denetleyicisi olarak:

![](hello-ios-deepdive-images/image45new.png "Phoneword uygulama ViewController görünüm denetleyicisi ayarlar.")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Görünüm denetleyicisi içinde bir film şeridini Sahne sayfanın alt kısmında bir çubuk olarak temsil edilir. Özelliklerini açılan View Controller'ı seçerek **Özellikler bölmesinde**:

![](hello-ios-deepdive-images/vs-image44.png "Görünüm denetleyicisi seçerek Özellikler bölmesinde özelliklerini getirir.")

İçerik görünümü bu görünüm tarafından temsil edilen hiyerarşi için özel bir görünüm denetleyicisi sınıfını düzenleyerek ayarlanabilir **sınıfı** özelliğinde **kimlik** bölümünü **Özelliklerbölmesi**. Örneğin, bizim **Phoneword** uygulama kümeleri `ViewController` ekran aşağıda gösterildiği gibi ilk ekran için Görünüm denetleyicisi olarak:

![](hello-ios-deepdive-images/vs-image45.png "Phoneword uygulama ViewController görünüm denetleyicisi ayarlar.")

-----

Bu görünüm denetleyicisi için görsel taslak gösterimini bağlar `ViewController` C# sınıfı. Açık `ViewController.cs` dosya ve görünüm denetleyicisi fark bir *alt* , `UIViewController`kodla gösterildiği gibi:

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

`ViewController` Artık bu görsel taslak görünüm denetleyicisi ilişkili içerik görünümü hiyerarşinin etkileşimler sürücüler. Sonraki görünüm denetleyicinin rol görünümü yaşam döngüsü adlı bir işlem sunarak görünümleri yönetme hakkında bilgi edineceksiniz.

> [!NOTE]
> Kullanıcı etkileşimi gerektirmeyen yalnızca visual ekranlar için **sınıfı** özelliği bırakılabilir boş **özellikler panelinde**. Bu görünüm denetleyicinin yedekleme sınıfı varsayılan uygulaması ayarlar bir `UIViewController`, özel kod ekleme planlamıyorsanız uygun olduğu.

### <a name="view-lifecycle"></a>Görünüm yaşam döngüsü

Yükleme ve içerik görünümü hiyerarşileri penceresinden kaldırma sorumlu görünüm denetleyicisi değil. Önem derecesi içerik görünümü hiyerarşideki bir görünüm için bir şey olduğunda işletim sistemi olaylar görünümü yaşam döngüsünde ile görünüm denetleyicisi bildirir. Görünüm yaşam döngüsünde yöntemi geçersiz kılarak, dinamik, hızlı yanıt veren bir kullanıcı arabirimi oluşturmak ve ekrandaki nesnelere etkileşim.

Temel yaşam döngüsü yöntemleri ve işlevleri şunlardır:

-  **ViewDidLoad** - çağrılan *sonra* ilk kez görünüm denetleyicisi yükler, içerik hiyerarşisini görüntüle belleğe. Subviews ilk kod içinde kullanılabilir duruma geldiğinde, olduğundan kurulum başlangıç için iyi bir yerdir.
-  **ViewWillAppear** -yaklaşık bir içerik görünümü hiyerarşiye eklenmesi ve ekranda görünen bir görünüm denetleyicinin görünümü her zaman çağrılır.
-  **ViewWillDisappear** -görünüm denetleyicinin görünümü hakkında bir içerik görünümü hiyerarşisinden kaldırılması ve ekranından kaybolur her zaman çağrılır. Bu yaşam döngüsü olayı, temizleme ve kayıt durumu için kullanılır.
-  **ViewDidAppear** ve **ViewDidDisappear** -bir görünüm eklendiğinde veya içerik görünümü hiyerarşiden sırasıyla kaldırıldığında çağrılır.


Özel kod yaşam döngüsünün, bu yaşam döngüsü yöntemi'nın herhangi bir aşamaya eklendiğinde *taban uygulamasını* olmalıdır *geçersiz kılınmış*. Bu, bazı kodlar zaten bağlı olan, varolan bir yaşam döngüsü yöntemi olarak ve ek kod ile genişletme tarafından sağlanır. Temel uygulama içinde özgün koda yeni kod önce çalıştığından emin olmak için yöntem çağrılır. Buna örnek olarak, sonraki bölümde gösterilmiştir.

Görünüm denetleyicileri ile çalışma hakkında daha fazla bilgi için Apple başvuran [iOS için Görünüm denetleyicisi Programming Guide](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1) ve [Uıviewcontroller başvuru](https://developer.apple.com/documentation/uikit/uiviewcontroller?language=objc).

### <a name="responding-to-user-interaction"></a>Kullanıcı etkileşimine yanıt verme

Görünüm denetleyicisi en önemli rolünü Düğme basma işlemlerine, gezinti ve diğer kullanıcı etkileşimine yanıt vermiyor. Kullanıcı etkileşimi işlemek için en basit yolu, kullanıcıya dinlemek için bir denetim girişi ve girişine yanıt vermek için bir olay işleyicisi ekleme yukarı wire sağlamaktır. Örneğin, bir düğme yukarı touch olaya yanıt vermek Phoneword uygulamada gösterildiği şekilde bağlanmamış.

Yoktur, görünümleri ve görünüm denetleyicileri daha kapsamlı bir anlayışa sahip şimdi bunun nasıl çalıştığını keşfedin.
İçinde `Phoneword_iOS` proje, bir düğme çağrılan eklenen `TranslateButton` için içerik hiyerarşisini görüntüle:

 [![](hello-ios-deepdive-images/image1.png "Bir düğme için içerik hiyerarşisini görüntüle çağrılan TranslateButton eklendi")](hello-ios-deepdive-images/image1.png#lightbox)

Olduğunda bir **adı** atandığı **düğmesi** denetim **özellikler panelinde**, iOS Tasarımcısı otomatik olarak bir denetime eşlenen  **ViewController.designer.cs**, yapmayı `TranslateButton` içinde kullanılabilir `ViewController` sınıfı. Denetimleri önce kullanılabilir hale gelir, `ViewDidLoad` görünümü bu yaşam döngüsü yöntemi kullanıcının yanıt kullanılacak şekilde yaşam döngüsü aşaması:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Adlı bir touch olay Phoneword uygulamanın kullandığı `TouchUpInside` için kullanıcının touch dinlemek için. `TouchUpInside` bir dokunma yukarı aşağı (ekran temas parmak) bir touch denetimin sınırları içinde aşağıdaki olayını (parmak ekranı kaldırarak) dinler. Tersini `TouchUpInside` olduğu `TouchDown` denetimde kullanıcı aşağı bastığında harekete olayı. `TouchDown` Olay çok fazla gürültü yakalayan ve touch kendi parmak denetimi devre dışı kaydırarak iptal seçeneği verir. `TouchUpInside` yanıt vermek için en yaygın yolu bir **düğmesi** dokunma ve kullanıcı bir düğmeye basıldığında bekliyor deneyimi oluşturur. Daha fazla bilgi bu Apple içinde kullanılabilir [iOS İnsan Arabirimi yönergelerine](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html).

Uygulama işlenen `TouchUpInside` olay bir lambda, ancak bir temsilci veya bir adlandırılmış olayı işleyicisi de kullanılabilirdi. Son düğme kodu aşağıdaki benzeyen:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
      translatedNumber = Core.PhonewordTranslator.ToNumber(PhoneNumberText.Text);
      PhoneNumberText.ResignFirstResponder ();

      if (translatedNumber == "") {
        CallButton.SetTitle ("Call", UIControlState.Normal);
        CallButton.Enabled = false;
      } else {
        CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
        CallButton.Enabled = true;
      }
  };
}
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword içinde sunulan ek kavramları

Bu kılavuzda ele alınmamaktadır birkaç kavram Phoneword uygulama sunmuştur. Bu kavramlar şunları içerir:

- **Düğme metnini değiştirme** – Phoneword uygulama gösterilen metni değiştirme bir **düğmesi** çağırarak `SetTitle` üzerinde **düğmesi** ve yeni metni geçirerek ve  **Düğme**'s _denetim durumu_. Örneğin, aşağıdaki kodu "Çağrısı" CallButton'ın metni değiştirir:

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```
- **Etkinleştirme ve devre dışı düğmeleri** – **düğmeleri** olabilir bir `Enabled` veya `Disabled` durumu. Devre dışı bir **düğmesi** kullanıcı girişine yanıt vermiyor. Örneğin, aşağıdaki devre dışı bırakır kod `CallButton`: CallButton.Enabled = false; Düğmeler hakkında daha fazla bilgi için başvurmak [düğmeleri](~/ios/user-interface/controls/buttons.md) Kılavuzu.
- **Klavye Kapat** – klavye girişi girin izin vermek için kullanıcı Tap'ları iOS metin alanı görüntüler. Ne yazık ki, klavye kapatmak için yerleşik işlevi yoktur. Aşağıdaki kodu eklenir `TranslateButton` kullanıcının bastığında klavye kapatmak için `TranslateButton`: PhoneNumberText.ResignFirstResponder (); Klavye artık başka bir örneği için başvurmak [klavye Kapat](https://github.com/xamarin/recipes/tree/master/Recipes/ios/input/keyboards/dismiss_the_keyboard) tarif.
- **Telefon arama URL'si ile** – Phoneword uygulamada, bir Apple URL düzeni Sistem telefon uygulamasını başlatmak için kullanılır. Özel URL şeması oluşan bir "tel:" öneki ve çevrilmiş telefon numarası, aşağıdaki kod ile gösterildiği gibi:

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```
- **Uyarı Göster** – çağrıları – örneğin simülatör desteklemeyen bir cihazda telefonla yerleştirmek bir kullanıcı çalışır veya iPod Touch – uyarı iletişim kutusu kullanıcının telefon araması yapılamıyor yerleştirilebilir bilmeniz gösterilir. Aşağıdaki kod oluşturur ve bir uyarı denetleyicisi doldurur:

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

İOS uyarı görünümleri hakkında daha fazla bilgi için başvurmak [uyarı denetleyicisi tarif](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller).

## <a name="testing-deployment-and-finishing-touches"></a>Test, dağıtım ve Son dokunuşları

Mac için Visual Studio hem de Visual Studio test etmek ve bir uygulamayı dağıtmak için pek çok seçenek sağlar. Bu bölümde, hata ayıklama seçeneklerini kapsar, cihaz üzerinde test uygulamaları gösterir ve özel uygulama simgeleri ve başlatma görüntüleri oluşturmaya yönelik araçlar sunar.

### <a name="debugging-tools"></a>Hata ayıklama araçları

Bazen uygulama kodunda sorunları tanılamak zordur. Karmaşık kod sorunlarının tanılanmasına yardımcı olmak için şunları yapabilirsiniz: [bir kesme noktası ayarlamak](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [adım kod aracılığıyla](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code), veya [günlük penceresini çıkış bilgileri](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

### <a name="deploy-to-a-device"></a>Bir cihaza dağıtın

İOS simülatörü, bir uygulamayı test etmek için hızlı bir yoludur. Sahte konumu dahil olmak üzere birkaç testi, kullanışlı iyileştirmeleri simülatör sahip [taşıma benzetimi](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/test_location_changes_in_simulator)ve daha fazlası. Ancak, kullanıcılar bir simülatör son uygulamada kullanır değil. Tüm uygulamaları gerçek cihazlar üzerinde erken ve sıkça test edilmelidir.

Bir cihazı sağlamak zaman alır ve bir Apple Geliştirici hesabı gerektirir. [Cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md) kılavuz, geliştirme için bir cihaz hazırlığı kapsamlı yönergeler verir.

> [!NOTE]
> Apple'dan bir gereksinim nedeniyle günümüzde bu sahip için gerekli olan bir geliştirme sertifikası veya _imzalama kimliği_ oluşturmak için cihaz veya simülatör için kod. Bağlantısındaki [cihaz sağlama Kılavuzu](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) bunu ayarlamak için.

Cihaz sağlandıktan sonra ona içinde hedef yapı araç çubuğunda bir iOS cihazını değiştirme ve tuşuna basarak takarak dağıtabileceğiniz **Başlat** ( **Play**) tarafından aşağıdaki ekran görüntüsünde gösterildiği gibi:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image46new.png "Tuşlarına basarak başlangıç/yürütme")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image46.png "Tuşlarına basarak başlangıç/yürütme")

-----

Uygulamayı iOS cihaza dağıtır:

[![](hello-ios-deepdive-images/image1.png "Uygulama iOS cihazını dağıtma ve çalıştırma")](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>Özel simgeleri oluşturma ve başlatma görüntüleri

Herkes özel simgeleri oluşturma ve başlatma görüntüleri bir uygulamanın Rekabetin ihtiyacı için kullanılabilecek bir tasarımcı sahiptir. Özel uygulama resmi oluşturmanın birkaç alternatif yaklaşımlar aşağıda verilmiştir:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

- [**Taslak** ](https://www.sketchapp.com") – taslak olan kullanıcı arabirimleri, simgeler ve diğer tasarlamak için bir Mac uygulaması. Bu, Xamarin uygulama simgeleri ve başlatma resimleri kümesi tasarlamak için kullanılan uygulamadır. Taslak 3 App Store üzerinde kullanılabilir. Ücretsiz deneyebilirsiniz [taslak aracı](http://bohemiancoding.com/sketch/tool/) de.
- [**Pixelmator** ](http://www.pixelmator.com/) – uygulaması yaklaşık 30 ABD Doları değerindedir Mac için düzenleme, çok yönlü bir görüntüsü.
- [**Glyphish** ](http://www.glyphish.com/) – yüksek kaliteli önceden oluşturulmuş simgesi ayarlar ücretsiz indirme ve satın alma.
- [**Fiverr** ](http://www.fiverr.com/) – tasarımcılar, 5 Başlangıç için bir simge oluşturmak için çeşitli seçin.  Miss hit veya olabilir ancak çalışma sırasında simgeler gerekiyorsa bir makaleden tasarlanmış

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

* Visual Studio – bu uygulama doğrudan IDE'de ayarlamak basit bir simge oluşturmak için kullanabilirsiniz.
- [**Glyphish** ](http://www.glyphish.com/) – yüksek kaliteli önceden oluşturulmuş simgesi ayarlar ücretsiz indirme ve satın alma.
- [**Fiverr** ](http://www.fiverr.com/) – tasarımcılar, 5 Başlangıç için bir simge oluşturmak için çeşitli seçin.  Miss hit veya olabilir ancak çalışma sırasında simgeler gerekiyorsa bir makaleden tasarlanmış

-----

Simgesi ve başlatma resim boyutları ve gereksinimleri hakkında daha fazla bilgi için [görüntüleri Kılavuzu ile çalışan](~/ios/app-fundamentals/images-icons/index.md).

## <a name="summary"></a>Özet

Tebrikler! Artık bir Xamarin.iOS uygulaması ve bunun yanı sıra bunları oluşturmak için kullanılan araçları bileşenlerinin düz bir anlayış var.
İçinde [Başlarken serideki sonraki öğretici](~/ios/get-started/hello-ios-multiscreen/index.md), birden fazla ekrana işlemek için uygulamamız genişletin. Birden çok ekrana işlemek için uygulamamız genişletirken bir gezinti denetleyicisi uygulamak, film şeridi etkinleştirilir hakkında bilgi edinin ve Model, görünüm denetleyicisi (MVC) tanıtmak süreç boyunca desen.


## <a name="related-links"></a>İlgili bağlantılar

- [Hello, iOS (örnek)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS İnsan Arabirimi yönergelerine](https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/)
- [iOS sağlama portalı](http://developer.apple.com/account/#/overview)
