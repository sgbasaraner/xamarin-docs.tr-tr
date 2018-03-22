---
title: "Merhaba, iOS: derinlemesine bakış"
description: "Bu iki parçalı Kılavuzu, Mac veya Visual Studio için Visual Studio kullanarak temel bir Xamarin.iOS uygulaması oluşturma ve Xamarin ile iOS uygulaması geliştirme ile ilgili temel bilgileri bir anlayış geliştirmek açıklar. Araçlar, kavramlar ve oluşturmak ve bir Xamarin.iOS uygulaması dağıtmak için gerekli adımları tanıtılacaktır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 500c3d1c6f38427a921097a0c3104254ec5cb263
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="hello-ios-deep-dive"></a>Merhaba, iOS derinlemesine bakış

Hızlı Başlangıç Kılavuzu oluşturma ve temel bir Xamarin.iOS uygulaması çalıştırma kullanıma sunuldu. Şimdi daha karmaşık programları oluşturabilmeniz iOS uygulamaların nasıl çalıştığı, daha derin bir anlayış geliştirmek için zaman yapılır. Bu kılavuz adımları gözden geçirir, Hello, iOS uygulama geliştirme temel kavramlarını anlama etkinleştirmek için iOS gözden geçirme.

Aşağıdaki konular bu makalede ele alınan dizelerle:


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

- **Mac için Visual Studio giriş** – Mac ve yeni bir uygulama oluşturmak için Visual Studio giriş.
- **Bir Xamarin.iOS uygulaması anatomisi** -bir Xamarin.iOS uygulaması temel bölümlerini turu.
- **Mimari ve uygulama Temelleri** – bir iOS uygulaması ve bunlar arasındaki ilişkinin bölümlerini gözden geçirin.
- **Kullanıcı Arabirimi (UI)** – kullanıcı arabirimleri iOS Tasarımcısı ile oluşturma.
- **Denetleyicileri ve görünüm yaşam döngüsü görüntülemek** – bir görünüm yaşam döngüsü ve görünüm denetleyicisiyle içerik görünümü hiyerarşileri yönetme giriş.
- **Test, dağıtım ve son rötuşları** -sınama, dağıtım, oluşturma resmi ve daha fazla öneri uygulamanızla tamamlayın.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **Visual Studio giriş** – giriş Visual Studio ve yeni bir uygulama oluşturma.
- **Bir Xamarin.iOS uygulaması anatomisi** -bir Xamarin.iOS uygulaması temel bölümlerini turu.
- **Mimari ve uygulama Temelleri** – bir iOS uygulaması ve bunlar arasındaki ilişkinin bölümlerini gözden geçirin.
- **Kullanıcı Arabirimi (UI)** – kullanıcı arabirimleri iOS Tasarımcısı ile oluşturma.
- **Denetleyicileri ve görünüm yaşam döngüsü görüntülemek** – bir görünüm yaşam döngüsü ve görünüm denetleyicisiyle içerik görünümü hiyerarşileri yönetme giriş.
- **Test, dağıtım ve son rötuşları** -sınama, dağıtım, oluşturma resmi ve daha fazla öneri uygulamanızla tamamlayın.

-----

Bu kılavuz becerileri ve bir tek ekran iOS uygulaması oluşturmak için gerekli bilgileri geliştirmenize yardımcı olur. Onun üzerinden çalıştıktan sonra bir Xamarin.iOS uygulaması ve onların birlikte nasıl uyduğunu farklı parçalarının bilginizin olması gerekir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Mac için Visual Studio giriş

Mac için Visual Studio, Visual Studio ve XCode özelliklerinden bir araya getiren ücretsiz, açık kaynaklı bir IDE olur. Tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir derleme tarayıcı, kaynak kodu tümleştirme ve daha fazla bilgi sunar. Bu kılavuz temel bazı Visual Studio için Mac özellikleri sunar, ancak Mac için Visual Studio yeniyseniz, kullanıma [Mac için Visual Studio](https://docs.microsoft.com/visualstudio/mac/) belgeleri.

Mac için Visual Studio aşağıdaki koda düzenleme Visual Studio uygulama *çözümleri* ve *projeleri*. Bir çözümü bir veya daha fazla projeleri tutan bir kapsayıcıdır. Proje (örneğin, iOS veya Android) bir uygulama, bir destek kitaplığı, bir sınama uygulaması ve daha fazla olabilir. Phoneword uygulamada kullanarak yeni bir iPhone proje eklendi **tek görünüm uygulaması** şablonu. İlk çözüm şöyle Aranan:

![](hello-ios-deepdive-images/image30.png "İlk çözümün bir ekran görüntüsü")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Visual Studio giriş

Visual Studio Microsoft güçlü bir IDE ' dir. Tam olarak tümleşik bir görsel tasarımcı, yeniden düzenleme araçları ile tam bir metin düzenleyicisi, bir derleme tarayıcı, kaynak kodu tümleştirme ve daha fazla bilgi sunar. Bu kılavuz, eklenti Xamarin ile temel bazı Visual Studio özellikler sunar.

Visual Studio düzenler koda _çözümleri_ ve *projeleri*. Bir çözümü bir veya daha fazla projeleri tutan bir kapsayıcıdır. Proje (örneğin, iOS veya Android) bir uygulama, bir destek kitaplığı, bir sınama uygulaması ve daha fazla olabilir. Phoneword uygulamada kullanarak yeni bir iPhone proje eklendi **tek görünüm uygulaması** şablonu. İlk çözüm şöyle Aranan:

![](hello-ios-deepdive-images/vs-image30.png "İlk çözümün bir ekran görüntüsü")

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinios-application"></a>Bir Xamarin.iOS uygulaması anatomisi

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Soldaki *çözüm paneli*, dizin yapısını içerir ve tüm dosyaları çözümle ilişkili:

![](hello-ios-deepdive-images/image31.png "Dizin yapısını ve çözümle ilişkili tüm dosyalar içeren çözüm paneli")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sağ tarafta olan *çözüm bölmesinde*, dizin yapısını içerir ve tüm dosyaları çözümle ilişkili:

![](hello-ios-deepdive-images/vs-image31.png "Dizin yapısını ve çözümle ilişkili tüm dosyalar içeren çözüm bölmesi")

-----

İçinde [Hello, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) gözden geçirme, oluşturduğunuz adlı bir çözüm **Phoneword** ve bir iOS projesi - yerleştirilmiş **Phoneword_iOS** - içindeki. Proje içinde öğeleri içerir:

-  **Başvuruları** -oluşturmak ve uygulamayı çalıştırmak için gerekli olan derlemeleri içerir. .NET derleme başvurularını gibi görmek için dizini genişletin [sistem](http://msdn.microsoft.com/en-us/library/system%28v=vs.110%29.aspx) , System.Core, ve [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml%28v=vs.110%29.aspx) , Xamarin'ın Xamarin.iOS derlemesine başvuru yanı sıra.
-  **Paketleri** -paket dizini hazır NuGet paketlerini içerir.
-  **Kaynakları** -kaynak klasör, diğer medya depolar.
-  **Main.cs** – Bu, uygulamanın ana giriş noktası içerir. Ana uygulama sınıfın adını uygulamayı başlatmak için `AppDelegate`, geçirilen.
-  **AppDelegate.cs** – bu dosya ana uygulama sınıfını içerir ve pencere, kullanıcı arabirimini oluşturmaya ve işletim sisteminden olaylarını dinleme sorumludur.
-  **Main.Storyboard** -film şeridi uygulamanın kullanıcı arabiriminin görsel tasarım içerir. Film şeridi dosya Tasarımcısı iOS adlı bir grafik düzenleyicisinde açın.
-  **ViewController.cs** – View Controller kullanıcı görür ve dokunur ekran (Görünüm) destekler. Görünüm Denetleyicisi'ni, kullanıcı ve görünüm arasındaki etkileşimler işlemekten sorumludur.
-  **ViewController.designer.cs** – `designer.cs` görünümünde denetimleri ve kendi kod Beyanları görünüm denetleyicideki arasında bağlantılı olarak hizmet veren bir otomatik olarak oluşturulan bir dosyadır. Bu bir iç tesisat dosyası olduğundan, IDE el ile yapılan tüm değişiklikler ve çoğu bu dosya görmezden gelinebilir zaman üzerine yazar. Görsel Tasarımcı ve yedekleme kodunun arasındaki ilişki hakkında daha fazla bilgi için başvurmak [Tasarımcısı iOS giriş](~/ios/user-interface/designer/introduction.md) Kılavuzu.
-  **Info.plist** – `Info.plist` uygulama adı, simgeler, başlatma görüntüler ve daha fazla gibi uygulama özelliklerini belirlendiği değil. Bu güçlü bir dosyadır ve onu kapsamlı bir giriş bulunur [özelliği listeleriyle çalışma](~/ios/app-fundamentals/property-lists.md) Kılavuzu.
-  **Entitlements.plist** -yetkilendirmeler özellik listesi bize uygulama belirtin sağlar *yetenekleri* (uygulama depolama teknolojileri de denir) iCloud, PassKit ve daha fazlası gibi. Daha fazla bilgi `Entitlements.plist` bulunabilir [özelliği listeleriyle çalışma](~/ios/app-fundamentals/property-lists.md) Kılavuzu. Yetkilendirmeler genel bir giriş için bkz [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) Kılavuzu.

## <a name="architecture-and-app-fundamentals"></a>Mimari ve uygulama temelleri

Bir iOS uygulaması bir kullanıcı arabirimi yüklemeden önce iki şey yerinde olması gerekir. İlk olarak, tanımlamak uygulaması gereken bir *giriş noktası* – uygulama işlemi belleğe yüklendiğinde çalışan ilk kod. İkinci olarak, uygulama genelinde olayları işlemek ve işletim sistemi ile etkileşim kurmak için bir sınıf tanımlama gerekir.

Bu bölüm, aşağıdaki çizimde gösterilen ilişkileri studies:

[![](hello-ios-deepdive-images/image32.png "Mimari ve uygulama temelleri ilişkileri Bu diyagrama gösterilmiştir")](hello-ios-deepdive-images/image32.png#lightbox)

Şimdi en başından ve uygulama başlangıcında olanlar öğrenin.

### <a name="main"></a>Ana

Bir iOS uygulamasının ana giriş noktası `Main.cs` dosya. `Main.cs` Yeni bir Xamarin.iOS uygulaması örneği oluşturur ve adını aktaran bir statik Main yöntemini içeren *uygulama temsilci* OS olayları işleyecek sınıfı. Statik için şablonu kodu `Main` yöntemi aşağıda görünür:

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

İOS içinde *uygulama temsilci* sınıfı sistem olayları gerçekleştirir; Bu sınıf içinde bulunduğu `AppDelegate.cs`. `AppDelegate` Sınıfı yönetir uygulama *penceresi*. Tek bir örneğini penceredir `UIWindow` kullanıcı arabirimi için bir kapsayıcı görevi görür sınıfı. Varsayılan olarak, bir uygulama içeriğinin yükleneceği üzerine yalnızca bir pencere alır ve pencere bağlı bir *ekran* (tek `UIScreen` örnek) fiziksel boyutlarını eşleşen sınırlayıcı dikdörtgenini sağlar cihaz ekranı.

*AppDelegate* Ayrıca uygulama başlatma tamamlandığında veya bellek düşük olduğunda gibi önemli uygulama olaylarıyla ilgili sistem güncelleştirmelerini abone sorumludur.

AppDelegate şablon kodunu aşağıda gösterilmiştir:

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

Uygulama tanımlanmış onun penceresi sonra kullanıcı arabirimini yükleme başlayabilirsiniz. Sonraki bölümde, kullanıcı Arabirimi oluşturma araştırır.

## <a name="user-interface"></a>Kullanıcı Arabirimi

Kullanıcı bir iOS uygulaması gibi bir mağaza arabirimdir - uygulama genellikle bir pencere alır, ancak bu pencereyi ile birçok nesnede gerekir ve nesneleri ve düzenlemeleri ne uygulamayı görüntülemek istediği bağlı olarak değiştirilebilir dolabilir. Bu senaryoda - kullanıcının gördüğü şeyler - nesneleri görünümleri denir. Bir uygulamadaki tek bir ekran oluşturmak için görünümlerini birbirleriyle üstünde yığılma bir *içerik görünümü hiyerarşi*, ve hiyerarşisini tek bir görünüm denetleyici tarafından yönetilir. Birden çok ekran uygulamaları birden çok içerik görünümü hiyerarşi, her biri kendi görünüm denetleyicisi sahip ve kullanıcı açıktır, farklı içerik Görünümü ekranında dayalı bir hiyerarşide oluşturmak için penceresinde görünümleri uygulama yerleştirir.

Bu bölümde kullanıcı arabirimine çekecek görünümleri, içerik görünümü hiyerarşileri ve iOS Tasarımcısı açıklayarak.

### <a name="ios-designer-and-storyboards"></a>iOS Tasarımcısı ve film şeritleri

İOS Tasarımcısı Xamarin kullanıcı arabirimi oluşturmak için görsel bir araçtır. Aşağıdaki ekran görüntüsüne benzer bir görünüm açılır herhangi film şeridi (.storyboard) dosyasını çift tıklatarak Tasarımcı başlatılabilir:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image33.png "iOS Tasarımcısı arabirimi")

A *film şeridi* bizim uygulamanın ekranlar yanı sıra geçişler ve ekranlar arasındaki ilişkileri visual tasarımları içeren bir dosyadır. Bir uygulamanın ekranında film şeridi gösterimini adlı bir _Sahne_. Her Sahne View Controller ve görünümler (içerik görünümü hiyerarşi) yönetir yığınını temsil eder. Yeni bir **tek görünüm uygulaması** proje bir şablondan oluşturulan, Mac için Visual Studio otomatik olarak oluşturur adlı bir film şeridi dosya `Main.storyboard` ve tarafından ekran görüntüsü gösterildiği gibi tek bir Sahne ile doldurur Aşağıda:

![](hello-ios-deepdive-images/image34.png "Mac için Visual Studio otomatik olarak Main.storyboard adlı bir film şeridi dosya oluşturur ve tek bir Sahne ile doldurur")

Film şeridi ekranın altındaki siyah bir çubuk, görünüm için Görünüm denetleyicisini seçmek için seçilebilir. Görünüm denetleyicisini örneğidir `UIViewController` içerik görünümü hiyerarşi için yedekleme kodunun içeren sınıf. Bu görünüm denetleyicisindeki özellikleri görüntülenebilir ve içinde ayarlamak **özellikleri paneli**aşağıdaki ekran görüntüsüne gösterildiği gibi:

![](hello-ios-deepdive-images/image35.png "Özellikler bölmesi")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image33.png "iOS Tasarımcısı arabirimi")

A *film şeridi* bizim uygulamanın ekranlar yanı sıra geçişler ve ekranlar arasındaki ilişkileri visual tasarımları içeren bir dosyadır. Bir uygulamanın ekranında film şeridi gösterimini adlı bir _Sahne_. Her Sahne View Controller ve görünümler (içerik görünümü hiyerarşi) yönetir yığınını temsil eder. Yeni bir **tek görünüm uygulaması** proje bir şablondan oluşturulan, Visual Studio otomatik olarak oluşturur adlı bir film şeridi dosya `Main.storyboard` ve aşağıdaki ekran görüntüsüne gösterildiği gibi tek bir Sahne ile doldurur:

![](hello-ios-deepdive-images/vs-image34.png "Visual Studio otomatik olarak Main.storyboard adlı bir film şeridi dosya oluşturur ve tek bir Sahne ile doldurur")

Film şeridi ekranın altındaki çubuğu, görünüm için Görünüm denetleyicisini seçmek için seçilebilir. Görünüm denetleyicisini örneğidir `UIViewController` içerik görünümü hiyerarşi için yedekleme kodunun içeren sınıf. Bu görünüm denetleyicisindeki özellikleri görüntülenebilir ve içinde ayarlamak **Özellikler bölmesinde**aşağıdaki ekran görüntüsüne gösterildiği gibi:

![](hello-ios-deepdive-images/vs-image35.png "Özellikler bölmesi")

-----

_Görünüm_ Sahne beyaz parçası içinde tıklayarak seçilebilir. Görünümü örneği olan `UIView` ekranın bir alanı tanımlar ve o alanı içeriği ile çalışmak için arabirimleri sağlayan sınıfı. Varsayılan görünüm tek bir değer *kök görünümü* tüm cihaz ekranı doldurur.

Sahne solunda, aşağıdaki ekran görüntüsüne gösterildiği gibi bir bayrak simgesiyle gri bir ok şöyledir:

 [![](hello-ios-deepdive-images/image37.png "Bayrak simgesiyle gri oku")](hello-ios-deepdive-images/image37.png#lightbox)

Gri oku adlı bir film şeridi geçiş temsil eden bir *Segue* ("seg yönlü" denilir). Bu Segue hiçbir kaynak olduğundan, adlı bir *Sourceless ü*. Sourceless ü, görünümleri yüklenen uygulama başlangıcında bizim uygulamanın penceresine ilk Sahne işaret ediyor. Sahne ve görünümleri içindeki kullanıcı uygulamayı yüklediğinde görür ilk şey olacaktır.

Gelen bir kullanıcı arabirimi oluştururken, ek görünümler sürüklenebilir **araç** aşağıdaki ekran görüntüsüne gösterildiği gibi tasarım yüzeyi ana görünüm üzerine:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image38.png "Ek görünümler tasarım yüzeyine ana görünüm Kutusu'ndan sürüklenebilir")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image38.png "Ek görünümler tasarım yüzeyine ana görünüm Kutusu'ndan sürüklenebilir")

-----

Bu ek görünümler adlı *Subviews*. Birlikte, kök görünümü ve Subviews parçası olan bir *içerik görünümü hiyerarşi* tarafından yönetilen `ViewController`. Tüm öğeleri anahat içinde inceleyerek görüntülenebilir **belge anahattı** paneli:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image39.png "Belge Anahattı paneli")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image39.png "Belge Anahattı paneli")

-----

Aşağıdaki diyagramda Subviews vurgulanmıştır:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image40.png "Aşağıdaki çizimde vurgulanan Subviews")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image40.png "Aşağıdaki çizimde vurgulanan Subviews")

-----

Bu görünüm tarafından temsil edilen içerik görünümü hiyerarşide aşağı sonraki bölüme ayırır.

## <a name="content-view-hierarchy"></a>İçerik görünümü hiyerarşisi

A _içerik görünümü hiyerarşi_ görünümleri ve tek bir görünüm denetleyicisi tarafından yönetilen Subviews yığınını Aşağıdaki diyagramda gösterildiği gibi değil:

 [![](hello-ios-deepdive-images/image41.png "İçerik görünümü hiyerarşisi")](hello-ios-deepdive-images/image41.png#lightbox)

İçerik görünümü hiyerarşisini vermiyoruz bizim `ViewController` görünüm bölümünde sarıya geçici olarak görünüm kök arka plan rengi değiştirerek görmek daha kolay, **özellikleri paneli**aşağıdaki ekran görüntüsüne gösterildiği gibi:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image42.png "Sarı özellikleri paneli görünümü bölümünde görünüm kök arka plan rengini değiştirme")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image42.png "Sarı özellikleri paneli görünümü bölümünde görünüm kök arka plan rengini değiştirme")

-----

Aşağıdaki diyagram aygıt ekranına kullanıcı arabirimi Getir penceresi, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkileri gösterir:

 [![](hello-ios-deepdive-images/image43.png "Pencere, görünümler, Subviews ve görünüm denetleyicisi arasındaki ilişkileri")](hello-ios-deepdive-images/image43.png#lightbox)

Sonraki bölümde nasıl kodda görünümlerle çalışma ve görünüm denetleyicileri ve görünüm yaşam döngüsü kullanarak kullanıcı etkileşimi için programlamayı öğrenin açıklanmaktadır.

## <a name="view-controllers-and-the-view-lifecycle"></a>Görünüm denetleyicileri ve görünüm yaşam döngüsü

Her içerik hiyerarşisini görüntüleme güç kullanıcı etkileşimi karşılık gelen bir görünüm denetleyiciye sahiptir. Görünüm denetleyicisini rol içerik hiyerarşisini görüntüleme görünümlerde yönetmektir. Görünüm denetleyicisini içerik görünümü hiyerarşinin parçası değil ve arabiriminde bir öğe değil. Bunun yerine, kullanıcının etkileşimleri ekranında nesnelerle'nın temelini oluşturan kodu sağlar.

### <a name="view-controllers-and-storyboards"></a>Görünüm denetleyicileri ve film şeritleri

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Görünüm denetleyicisini bir film şeridi Sahne altındaki çubuğu olarak temsil edilir. Görünüm denetleyicisini seçerek getirir, özelliklerinde **özellikleri paneli**:

![](hello-ios-deepdive-images/image44.png "Görünüm denetleyicisini seçerek özelliklerini Özellikler bölmesinde yukarı getirir")

Bu Sahne tarafından temsil edilen içerik görünümü hiyerarşi için özel bir görünüm denetleyicisini sınıf düzenleyerek ayarlanabilir **sınıfı** özelliğinde **kimlik** bölümünü **özellikleri paneli**. Örneğin, bizim **Phoneword** uygulama kümeleri `ViewController` aşağıdaki ekran görüntüsüne gösterildiği gibi ilk ekran View Controller olarak:

![](hello-ios-deepdive-images/image45new.png "Phoneword uygulama ViewController görünümü denetleyicisi olarak ayarlar.")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Görünüm denetleyicisini bir film şeridi Sahne altındaki çubuğu olarak temsil edilir. Görünüm denetleyicisini seçerek getirir, özelliklerinde **Özellikler bölmesinde**:

![](hello-ios-deepdive-images/vs-image44.png "Görünüm denetleyicisini seçerek özelliklerini Özellikler bölmesinde yukarı getirir")

Bu görünüm tarafından temsil edilen içerik görünümü hiyerarşi için özel bir görünüm denetleyicisini sınıf düzenleyerek ayarlayabilirsiniz **sınıfı** özelliğinde **kimlik** bölümünü **Özellikler bölmesi**. Örneğin, bizim **Phoneword** uygulama kümeleri `ViewController` aşağıdaki ekran görüntüsüne gösterildiği gibi ilk ekran View Controller olarak:

![](hello-ios-deepdive-images/vs-image45.png "Phoneword uygulama ViewController görünümü denetleyicisi olarak ayarlar.")

-----

Bu görünüm denetleyiciye film şeridi gösterimini bağlar `ViewController` C# sınıfı. Açık `ViewController.cs` dosya ve View Controller fark bir *alt* , `UIViewController`kod tarafından gösterildiği gibi:

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

`ViewController` Şimdi film şeridi, bu görünüm Denetleyici ile ilişkili içerik görünümü hiyerarşisinin etkileşimleri sürücüler. Ardından Görünüm denetleyicisinin rol görünüm yaşam döngüsü adlı bir işlem sunarak görünümleri yönetme hakkında bilgi edineceksiniz.

> [!NOTE]
> Kullanıcı etkileşimi gerektirmeyen yalnızca visual ekranlar **sınıfı** özelliği bırakılabilir boş **özellikleri paneli**. Bu görünüm denetleyicinin yedekleme sınıfı varsayılan uygulaması ayarlar bir `UIViewController`, özel kod ekleme planlamıyorsanız uygun olduğu.

### <a name="view-lifecycle"></a>Görünüm yaşam döngüsü

Yükleme ve yüklemeyi kaldırma penceresinden içerik görünümü hiyerarşileri sorumlu görünümü denetleyicisidir. Önem derecesi içerik görünümü hiyerarşideki bir görünüm için bir şey olduğunda işletim sistemi görünüm denetleyici görünüm yaşam döngüsü olayları aracılığıyla bildirir. Görünüm yaşam döngüsü yöntemleri geçersiz kılarak, ekran üzerindeki nesnelerle etkileşim ve dinamik, yanıt veren kullanıcı arabirimi oluşturma.

Temel yaşam döngüsü yöntemleri ve bunların işlevi şunlardır:

-  **ViewDidLoad** - çağrılan *sonra* ilk kez görünüm denetleyicisini yükler, içerik görünümü hiyerarşisi belleğe. Bu, Subviews ilk kodda kullanılabilir olduğunda, olduğundan kurulum başlangıç için uygun bir yerdir.
-  **ViewWillAppear** -içerik görünümü hiyerarşisine eklenmesi ve ekranınızda hakkında için bir görünüm denetleyicisinin görünümdür her zaman çağrılır.
-  **ViewWillDisappear** -hakkında bir içerik görünümü hiyerarşisinden kaldırılması ve ekranından kayboluyor için bir görünüm denetleyicisinin görünümdür her zaman çağrılır. Bu yaşam döngüsü olay temizleme ve kaydetme durumu için kullanılır.
-  **ViewDidAppear** ve **ViewDidDisappear** -bir görünümü veya eklenen içerik görünümü hiyerarşiden sırasıyla kaldırıldığında çağrılır.


Tüm yaşam döngüsü, bu yaşam döngüsü yöntemi'nın hazırlamak için özel kod eklendiğinde *temel uygulamayı* olmalıdır *kılınmadı*. Bu, zaten kullanıma bazı koduna sahip, varolan bir yaşam döngüsü yöntemi içine dokunma ve ek koduyla genişletme tarafından sağlanır. Temel uygulama yönteminin içine özgün kod önce yeni kod çalışacağından emin olmak için çağrılır. Buna örnek olarak bir sonraki bölümde gösterilmiştir.

Görünüm denetleyicileri ile çalışma hakkında daha fazla bilgi için Apple için başvuruda [iOS için Görünüm denetleyicisini Programlama Kılavuzu](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/ViewLoadingandUnloading/ViewLoadingandUnloading.html) ve [UIViewController başvuru](https://developer.apple.com/library/ios/documentation/uikit/reference/UIViewController_Class/Reference/Reference.html).

### <a name="responding-to-user-interaction"></a>Kullanıcı etkileşimine yanıt verme

Görünüm denetleyicisini en önemli rolü düğmesine basarsa, gezinti ve daha fazlası gibi kullanıcı etkileşimi yanıt vermiyor. Kullanıcı etkileşimi işlemek için basit bir denetim kullanıcıya dinlemek için giriş ve girişine yanıt vermek için bir olay işleyicisi ekleme yukarı kablo yoldur. Örneğin, bir düğme yukarı dokunma olayına yanıt olarak Phoneword uygulamada gösterildiği gibi kablolu.

Yoktur, görünümleri ve görünüm denetleyicilerinin daha derin bir anlayış sahip nasıl işlediğine inceleyelim.
İçinde `Phoneword_iOS` proje, bir düğme çağrılan eklenen `TranslateButton` içerik hiyerarşisini görüntüleme için:

 [![](hello-ios-deepdive-images/image1.png "Düğme için içerik hiyerarşisini görüntüleme çağrılan TranslateButton eklendi")](hello-ios-deepdive-images/image1.png#lightbox)

Zaman bir **adı** atandığı **düğmesini** denetim **özellikleri paneli**, iOS Tasarımcısı'nı otomatik olarak bir denetime eşlenen  **ViewController.designer.cs**, yapmayı `TranslateButton` içinde kullanılabilir `ViewController` sınıfı. Denetimleri ilk duruma bulunan `ViewDidLoad` görünüm bu yaşam döngüsü yöntemi kullanıcının yanıt kullanılacak şekilde ömrü aşaması:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Adlı bir touch olay Phoneword uygulamanın kullandığı `TouchUpInside` kullanıcının dokunma dinlemek için. `TouchUpInside` denetimin sınırları içinde bir touch (ekran temas parmak) aşağı izler (ekranı kaldırma parmak) olay yukarı dokunma dinler. Tersini `TouchUpInside` olan `TouchDown` bir denetimde kullanıcı aşağı bastığında harekete olayı. `TouchDown` Olay çok fazla gürültü yakalar ve kullanıcı denetimi devre dışı kendi parmak kaydırarak dokunma iptal etmek için hiçbir seçeneği sunar. `TouchUpInside` yanıt için en yaygın yolu bir **düğmesini** dokunma ve kullanıcı bir düğmeye basıldığında bekliyor deneyimi oluşturur. Bunun hakkında daha fazla bilgi Apple içinde kullanılabilir [iOS İnsan Arabirimi yönergelerine](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html).

Uygulama işlenen `TouchUpInside` bir lambda ancak bir temsilci veya bir adlandırılmış olay işleyicisi olayla da kullanılabilirdi. Son düğme kodu aşağıdaki benzeyen:

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

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword içinde sunulan ek kavramlar

Phoneword uygulama bu kılavuzda ele alınmamaktadır birkaç kavramları sundu. Bu kavramları şunlardır:

- **Düğme metni değiştirme** – Phoneword uygulama gösterilen metnini değiştirmek nasıl bir **düğmesini** çağırarak `SetTitle` üzerinde **düğmesini** ve yeni bir metin geçirme ve  **Düğme**'s _denetim durumu_. Örneğin, aşağıdaki kodu CallButton'ın metin "Araması" değiştirir:

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```
- **Etkinleştirme ve devre dışı düğmeleri** – **düğmeleri** olabilir bir `Enabled` veya `Disabled` durumu. Devre dışı A **düğmesini** kullanıcı girişine yanıt vermiyor. Örneğin, aşağıdaki devre dışı bırakır kod `CallButton`: CallButton.Enabled = false; Düğmeler hakkında daha fazla bilgi için başvurmak [düğmeleri](~/ios/user-interface/controls/buttons.md) Kılavuzu.
- **Klavye yok sayın** – kullanıcı Tap'ları metin alanı, iOS giriş girmesine olanak klavye görüntüler. Ne yazık ki, klavye kapatmak için yerleşik bir işlevi yoktur. Aşağıdaki kod eklenen `TranslateButton` kullanıcı bastığında klavye kapatmak için `TranslateButton`: PhoneNumberText.ResignFirstResponder (); Klavye durdurulduğundan başka bir örnek için bkz [klavye yok sayın](https://developer.xamarin.com/recipes/ios/input/keyboards/dismiss_the_keyboard) tarif.
- **Telefon arama URL ile** – Phoneword uygulamada, sistem phone uygulamasını başlatmak için bir Apple URL şeması kullanılır. Özel URL şeması oluşan bir "tel:" önekini ve çevrilen telefon numarası, aşağıdaki kodu tarafından gösterildiği gibi:

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```
- **Uyarı Göster** – çağrıları – örneğin Simulator desteklemeyen bir aygıtta bir telefon araması yerleştirmek bir kullanıcı çalıştığında veya iPod Touch – uyarı iletişim kutusu kullanıcının telefon araması olamaz yerleştirilebilir görüntülenir. Aşağıdaki kodu oluşturur ve bir uyarı denetleyicisi doldurur:

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

İOS uyarı görünümler hakkında daha fazla bilgi için bkz [uyarı denetleyicisi tarif](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/).

## <a name="testing-deployment-and-finishing-touches"></a>Test, dağıtım ve son rötuşları

Mac için Visual Studio ve Visual Studio Test ve bir uygulamayı dağıtmak için birçok seçenek sağlar. Bu bölümde, hata ayıklama seçenekleri kapsamaktadır, cihazda test uygulamalar gösterir ve özel uygulama simgeleri ve başlatma görüntüleri oluşturmak için araçlar sunar.

### <a name="debugging-tools"></a>Hata Ayıklama Araçları

Bazen uygulama kodundaki sorunları tanılamak zordur. Karmaşık kod sorunlarını tanılamanıza yardımcı olması için verebilir [bir kesme noktası belirleyerek](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/), [adım aracılığıyla kodu](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/), veya [çıkış bilgileri Günlük penceresine](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/).

### <a name="deploy-to-a-device"></a>Bir aygıta dağıtmak

İOS simülatörü'nü, bir uygulamayı test etmek için hızlı bir yoludur. Sahte konumu dahil olmak üzere test, yararlı iyileştirmeler sayısı Simulator sahip [taşıma benzetimi](https://developer.xamarin.com/recipes/ios/multitasking/test_location_changes_in_simulator/)ve daha fazlası. Ancak, kullanıcıların bir Simulator son uygulamada tüketecektir değil. Tüm uygulamaları gerçek cihazlarda erken ve sıkça test edilmelidir.

Bir aygıt sağlamak için zaman alır ve bir Apple Developer hesabı gerektirir. [Cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) kılavuz, bir cihaz geliştirme için hazır hale kapsamlı yönergeler verir.

> [!NOTE]
> Günümüzde, Apple, bir gereksinim nedeniyle sağlamak ise gerekli bir geliştirme sertifikası veya _kimlik imzalama_ oluşturmak için aygıt veya simulator için kod. Adımları [cihaz sağlamayı Kılavuzu](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) bunu ayarlamak için.

Cihaz sağlandıktan sonra için iOS cihazını yapı araç hedef değiştirme ve tuşuna basarak içinde takarak dağıtabilirsiniz **Başlat** ( **Yürüt**) tarafından aşağıdaki ekran görüntüsünde gösterildiği gibi:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](hello-ios-deepdive-images/image46new.png "Tuşlarına basarak Başlat/kullan")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image46.png "Tuşlarına basarak Başlat/kullan")

-----

Uygulama iOS cihazını dağıtır:

[![](hello-ios-deepdive-images/image1.png "Uygulama iOS cihazını dağıtma ve çalıştırma")](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>Özel simge oluşturmak ve görüntüleri'ı Başlat

Herkes özel simge oluşturmak ve uygulama göze gerekiyor görüntüleri başlatmak için kullanılabilecek bir tasarımcı sahiptir. Özel uygulama resmi oluşturmak için birkaç alternatif yaklaşımlar şunlardır:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

- [**Taslak** ](https://www.sketchapp.com") – taslak olan kullanıcı arabirimleri, simgeler ve daha fazlasını tasarlamak için bir Mac uygulaması. Bu, Xamarin uygulama simgeleri ve başlatma görüntüleri kümesi tasarlamak için kullanılan uygulamadır. Taslak 3 App Store'da kullanılabilir. Ücretsiz deneyebilirsiniz [taslak aracı](http://bohemiancoding.com/sketch/tool/) de.
- [**Pixelmator** ](http://www.pixelmator.com/) – uygulama hakkında $30 maliyetleri Mac için düzenleme çok yönlü bir görüntü.
- [**Glyphish** ](http://www.glyphish.com/) – yüksek kaliteli önceden oluşturulmuş simgesi ayarlar ücretsiz indirme ve satın alma.
- [**Fiverr** ](http://www.fiverr.com/) –, 5'Başlangıç için ayarlanmış bir simge oluşturulmaya tasarımcıları çeşitli seçin.  Miss hit veya olabilir ancak simgeler gerekiyorsa iyi bir kaynak üzerinde kolay bir şekilde tasarlanmış

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

* Visual Studio – bu uygulamayı doğrudan IDE için ayarlama basit bir simge oluşturmak için kullanabilirsiniz.
- [**Glyphish** ](http://www.glyphish.com/) – yüksek kaliteli önceden oluşturulmuş simgesi ayarlar ücretsiz indirme ve satın alma.
- [**Fiverr** ](http://www.fiverr.com/) –, 5'Başlangıç için ayarlanmış bir simge oluşturulmaya tasarımcıları çeşitli seçin.  Miss hit veya olabilir ancak simgeler gerekiyorsa iyi bir kaynak üzerinde kolay bir şekilde tasarlanmış

-----

Simge ve başlatma görüntü boyutları ve gereksinimleri hakkında daha fazla bilgi için başvurmak [görüntüleri Kılavuzu ile çalışmaya](~/ios/app-fundamentals/images-icons/index.md).

## <a name="summary"></a>Özet

Tebrikler! Artık bileşenlerini, bir Xamarin.iOS uygulaması yanı sıra bunları oluşturmak için kullanılan araçları tam olarak anlaşılması var.
İçinde [Başlarken serideki sonraki öğretici](~/ios/get-started/hello-ios-multiscreen/index.md), birden çok ekran işlemek için uygulamamız genişletmek. Birden çok ekran işlemek için uygulamamız genişletirsiniz Gezinti denetleyicisi uygulamak, film şeridi Segues hakkında bilgi edinin ve Model, görünüm, denetleyici (MVC) tanıtmak yol boyunca desen.


## <a name="related-links"></a>İlgili bağlantılar

- [Merhaba, iOS (örnek)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS İnsan Arabirimi yönergelerine](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS sağlama portalı](https://developer.apple.com/ios/manage/overview/index.action)
