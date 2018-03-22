---
title: HomeKit
description: "HomeKit ev Otomasyon cihazları denetlemek için Apple'nın çerçevedir. Bu makalede HomeKit tanıtır ve HomeKit donatıyı Simulator ve basit bir Xamarin.iOS uygulaması yazma bu Donatılar ile etkileşim kurmak için yapılandırma test Donatılar kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 02116e8e11cb6ff050e2c885338777e1fd25c4cb
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="homekit"></a>HomeKit

_HomeKit ev Otomasyon cihazları denetlemek için Apple'nın çerçevedir. Bu makalede HomeKit tanıtır ve HomeKit donatıyı Simulator ve basit bir Xamarin.iOS uygulaması yazma bu Donatılar ile etkileşim kurmak için yapılandırma test Donatılar kapsar._

[![](homekit-images/accessory01.png "Uygulama örneği HomeKit etkin")](homekit-images/accessory01.png#lightbox)

Apple HomeKit iOS 8 sorunsuz bir şekilde çeşitli satıcılar birden çok giriş Otomasyon aygıtlardan tutarlı, tek bir birimde tümleştirmek için bir yöntem olarak kullanıma sunuldu. Bulmak için ortak bir protokolle yükselterek, yapılandırma ve ev Otomasyon cihazları denetleme HomeKit cihazların satıcılardan çalışmalarını koordine gerek kalmadan tüm tek tek satıcılara birlikte çalışacak biçimde ilgili olmayan sağlar.

HomeKit ile herhangi bir etkin HomeKit aygıt satıcı kullanılarak sağlanan API'leri veya uygulamaları olmadan denetleyen bir Xamarin.iOS uygulaması oluşturabilirsiniz. HomeKit'ni kullanarak aşağıdakileri yapabilirsiniz:

- Yeni etkin HomeKit ev Otomasyon cihazları bulmasına ve bunları tüm kullanıcının iOS cihazları arasında kalıcı bir veritabanı ekleyin.
- Kurulum, yapılandırmak, görüntülemek ve HomeKit herhangi bir CİHAZDAN kontrol _giriş yapılandırma veritabanı_.
- Önceden yapılandırılmış hiçbir HomeKit aygıtla iletişim kurmak ve tek tek eylemleri gerçekleştirmek ya da tüm mutfak ışık kapatma gibi birlikte çalışma komutu.

Aygıtları giriş yapılandırma veritabanında HomeKit etkin uygulamalar için ek olarak hizmet veren, HomeKit Siri ses komutları erişim sağlar. Uygun şekilde yapılandırılmış bir HomeKit Kurulum verildiğinde, kullanıcının ses komutları gibi verebilir "Siri, yaşam odada ışık Aç."

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>Ev yapılandırma veritabanı

HomeKit giriş koleksiyonuna belirli bir konumda tüm otomasyon cihazlar düzenler. Bu koleksiyon kullanıcının ev Otomasyon cihazlarını anlamlı, okunabilir etiketlerle mantıksal olarak düzenlenen birimlerine Grup bir yol sağlar.

Giriş koleksiyonu, bir giriş yapılandırma veritabanında otomatik olarak tüm kullanıcının iOS cihazları yedeklenen ve eşitlenen olacak depolanır. HomeKit giriş yapılandırma veritabanı ile çalışmak için aşağıdaki sınıflar sağlar:

- `HMHome` -Bu tek bir fiziksel konumda (ör. tüm bilgiler ve tüm giriş Otomasyon cihazlar için yapılandırmaları tutan, üst düzey kapsayıcıdır tek bir aile ikamet). Kullanıcı kendi ana giriş ve tatil temizleme gibi birden fazla ikamet sahip olabilir. Veya farklı "aynı özellikte ana ev ve bir konuk ev Garaj üzerinden gibi barındırıldığı" olabilir. Her iki durumda da en az bir `HMHome` nesne _gerekir_ kurulması ve diğer HomeKit bilgisi girilebilir önce depolanır.
- `HMRoom` -While isteğe bağlı bir `HMRoom` bir giriş içinde belirli odaları tanımlamak kullanıcı sağlar (`HMHome`) gibi: mutfak, banyo, Garaj veya yaşam yer. Kullanıcı kendi ev belirli bir konumda ev Otomasyon aygıtların tümünü gruplandırabilirsiniz bir `HMRoom` ve bunlar üzerine bir birim olarak davranır. Örneğin, Garaj ışık devre dışı bırakma Siri sorar.
- `HMAccessory` -Bu bir kişinin temsil eder, kullanıcının ikamet (örneğin, bir akıllı thermostat) içinde yüklü Otomasyon cihazın fiziksel HomeKit etkin. Her `HMAccessory` atanmış bir `HMRoom`. Kullanıcı tüm odaları yapılandırdıysanız, bu kurmadı HomeKit Donatılar özel varsayılan odasına atar.
- `HMService` -Tarafından sağlanan bir hizmet temsil eden bir verilen `HMAccessory`, açık/kapalı bir açık veya durumu rengini (rengini değiştirmek destekleniyorsa) gibi. Her `HMAccessory` ışık de içeren bir Garaj kapı açan gibi birden fazla hizmetin olabilir. Ayrıca, bir verilen `HMAccessory` kullanıcı denetimi dışında kalan Hizmetleri, bellenim güncelleştirme gibi olabilir.
- `HMZone` -Bir koleksiyonu Grup olanak tanır. `HMRoom` Upstairs, Downstairs veya zemin kat gibi mantıksal bölgelere nesneleri. İsteğe bağlı olsa da, bu Siri soran gibi etkileşimleri tüm açık downstairs devre dışı bırakma sağlar.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>HomeKit uygulama sağlama

HomeKit tarafından uygulanan güvenlik gereksinimleri olduğundan, HomeKit framework kullanan bir Xamarin.iOS uygulaması düzgün hem Apple Geliştirici Portalı'nda ve Xamarin.iOS projesi dosyasındaki yapılandırılması gerekir.

Aşağıdakileri yapın:

1. İçine oturum [Apple Geliştirici Portalı](http://developer.apple.com).
2. Tıklayın **tanımlayıcıları & profilleri, sertifikaları**.
3. Zaten yapmadıysanız, tıklayın **tanımlayıcıları** ve uygulamanız için bir kimlik oluşturun (örneğin `com.company.appname`), mevcut kimliğinizi başka Düzenle
4. Emin **HomeKit** hizmet verilen kimlik için kontrol edildiyse: 

    [![](homekit-images/provision01.png "Verilen kimlik için HomeKit hizmetini etkinleştirme")](homekit-images/provision01.png#lightbox)
5. Değişikliklerinizi kaydedin.
4. Tıklayın **sağlama profilleri** > **geliştirme** ve sağlama profili, uygulamanız için yeni bir yazılım geliştirme oluşturun: 

    [![](homekit-images/provision02.png "Sağlama profili uygulama için yeni bir yazılım geliştirme oluşturma")](homekit-images/provision02.png#lightbox)
5. Karşıdan yükle ve yeni sağlama profili yüklemek veya Xcode profili karşıdan yükleyip kullanabilirsiniz.
6. Xamarin.iOS projesi seçeneklerinizi düzenleyin ve yeni oluşturduğunuz sağlama profili kullandığınızdan emin olun: 

    [![](homekit-images/provision03.png "Yeni oluşturduğunuz sağlama profili seçin")](homekit-images/provision03.png#lightbox)
7. Ardından, düzenleme, **Info.plist** dosya ve sağlama profili oluşturmak için kullanılan uygulama kimliği kullandığından emin olun: 

    [![](homekit-images/provision04.png "Uygulama Kimliği ayarlayın ")](homekit-images/provision04.png#lightbox)
8. Son olarak, düzenleme, **Entitlements.plist** dosya ve emin **HomeKit** yetkilendirme seçili: 

    [![](homekit-images/provision05.png "HomeKit yetkilendirme etkinleştir")](homekit-images/provision05.png#lightbox)
9. Tüm dosyalara değişiklikleri kaydedin.

Yerinde bu ayarlarla uygulama HomeKit Framework API'lerine erişmek hazır. Hazırlama hakkında ayrıntılı bilgi için lütfen bkz bizim [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) ve [uygulamanızı sağlama](~/ios/get-started/installation/device-provisioning/index.md) kılavuzları.

> [!IMPORTANT]
> Etkin HomeKit uygulama sınama düzgün sağlanan gerçek iOS cihaz geliştirme için gerektirir. HomeKit iOS simülatörü sınanamıyor.

## <a name="the-homekit-accessory-simulator"></a>HomeKit donatıyı Simulator

Tüm olası ev Otomasyon aygıtlarının ve hizmetlerin, fiziksel bir aygıtı zorunluluğu olmadan test etmek için bir yol sağlamak üzere Apple oluşturulan _HomeKit donatıyı Simulator_. Bu simulator kullanarak, Kurulum ve sanal HomeKit aygıtlar yapılandırın.

### <a name="installing-the-simulator"></a>Simulator yükleme

Devam etmeden önce yüklemeniz gerekir böylece Apple Xcode ayrı bir yükleme olarak HomeKit donatıyı Simulator sağlar.

Aşağıdakileri yapın:

1. Bir web tarayıcısında ziyaret [Apple geliştiriciler için indirir](https://developer.apple.com/download/more/?name=for%20Xcode)
2. Karşıdan **Xcode xxx için ek araçlar** (xxx olduğu yüklediğiniz Xcode sürüm): 

    [![](homekit-images/simulator01.png "Ek araçlar için Xcode indirin")](homekit-images/simulator01.png#lightbox)
3. Disk görüntüsü açın ve araçları yüklemek, **uygulamaları** dizin.

HomeKit donatıyı yüklü simülatörü'ile sanal Donatılar test etmek için oluşturulabilir.

### <a name="creating-virtual-accessories"></a>Sanal Donatılar oluşturma

HomeKit donatıyı Simulator başlamak ve birkaç sanal Donatılar oluşturmak için aşağıdakileri yapın:

1. Uygulamaları klasöründen HomeKit donatıyı Simulator başlatın: 

    [![](homekit-images/simulator02.png "HomeKit donatıyı Simulator")](homekit-images/simulator02.png#lightbox)
2. Tıklatın  **+**  düğmesine tıklayın ve ardından **yeni donatıyı...** : 

    [![](homekit-images/simulator03.png "Yeni bir donatıyı ekleme")](homekit-images/simulator03.png#lightbox)
3. Yeni donatıyı hakkındaki bilgileri doldurun ve tıklayın **son** düğmesi: 

    [![](homekit-images/simulator04.png "Yeni donatıyı hakkındaki bilgileri doldurun")](homekit-images/simulator04.png#lightbox)
4. Tıklatın **hizmeti Ekle...** düğmesini ve bir hizmet türünün aşağı açılır listeden seçin: 

    [![](homekit-images/simulator05.png "Aşağı açılır listeden bir hizmet türü seçin")](homekit-images/simulator05.png#lightbox)
5. Sağlayan bir **adı** tıklatın ve hizmet için **son** düğmesi: 

    [![](homekit-images/simulator06.png "Hizmet için bir ad girin")](homekit-images/simulator06.png#lightbox)
6. Tıklayarak, bir hizmet için isteğe bağlı özellikleri sağlayabilir **ekleme özelliği** düğmesi ve gereken ayarları yapılandırma: 

    [![](homekit-images/simulator07.png "Gerekli ayarları yapılandırma")](homekit-images/simulator07.png#lightbox)
7. HomeKit destekleyen sanal giriş Otomasyon aygıt türlerinin birini oluşturmak için yukarıdaki adımları yineleyin.

Şimdi oluşturulur ve yapılandırılmış bazı örnek sanal HomeKit Donatılar ile kullanabilir ve bu aygıtlar arasında Xamarin.iOS uygulamanızı denetim.

## <a name="configuring-the-infoplist-file"></a>Info.plist dosyasını yapılandırma

Yeni iOS 10 için (ve büyük), geliştirici eklemeniz gerekir `NSHomeKitUsageDescription` uygulamanın anahtar `Info.plist` dosya ve uygulama neden isteyen kullanıcının HomeKit veritabanına erişmek için bildirme bir dize sağlayın. Bu dize, kullanıcılarınız uygulamayı çalıştırmadan kullanıcı ilk zamanı sunulur:

[![](homekit-images/info01.png "HomeKit izni iletişim")](homekit-images/info01.png#lightbox)

Bu anahtar ayarlamak için aşağıdakileri yapın:

1. Çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. Ekranın alt kısmındaki geçmek **kaynak** görünümü.
3. Yeni bir ekleme **girişi** listesi.
4. Açılır listeden seçin **gizlilik - HomeKit kullanım açıklama**: 

    [![](homekit-images/info02.png "Gizlilik - HomeKit kullanım açıklamasını seçin")](homekit-images/info02.png#lightbox)
5. Neden kullanıcının HomeKit veritabanına erişmek uygulamanın istediği için bir açıklama girin: 

    [![](homekit-images/info03.png "Bir açıklama girin")](homekit-images/info03.png#lightbox)
6. Değişiklikleri dosyaya kaydedin.

> [!IMPORTANT]
> Ayarlanamadı `NSHomeKitUsageDescription` anahtarını `Info.plist` dosya uygulamada sonuçlanacak _sessizce sorunlu_ (çalışma zamanında sistem tarafından kapatılan) iOS 10 (veya daha büyük) çalıştırdığınızda hatasız.

## <a name="connecting-to-homekit"></a>HomeKit için bağlanma

HomeKit ile iletişim kurması için Xamarin.iOS uygulamanızı bir örneği ilk gereken `HMHomeManager` sınıfı. Giriş Yöneticisi HomeKit içine Merkezi giriş noktasıdır ve kullanılabilir ev listesini sağlamaktan sorumludur güncelleştirme ve bu listeyi Bakımı ve kullanıcının döndürme _birincil giriş_.

`HMHome` Nesnesi odaları, gruplara veya bunu, birlikte yüklenmiş ev Otomasyon Donatılar içerebilir bölgeleri dahil olmak üzere verin ev ilgili tüm bilgileri içerir. HomeKit, en az bir herhangi bir işlem gerçekleştirilmeden önce `HMHome` oluşturulur ve birincil giriş sayfası olarak atanmış.

Uygulamanızın birincil giriş var olup olmadığını denetleme ve oluşturma ve mevcut değilse bir atama sorumludur.

### <a name="adding-a-home-manager"></a>Bir ev Yöneticisi ekleme

Bir Xamarin.iOS uygulaması HomeKit tanıma eklemek, düzenlemek **AppDelegate.cs** dosyasını düzenleyin ve aşağıdaki gibi görünmesini sağlamak için:

```csharp
using HomeKit;
...

public HMHomeManager HomeManager { get; set; }
...

public override void FinishedLaunching (UIApplication application)
{
    // Attach to the Home Manager
    HomeManager = new HMHomeManager ();
    Console.WriteLine ("{0} Home(s) defined in the Home Manager", HomeManager.Homes.Count());

    // Wire-up Home Manager Events
    HomeManager.DidAddHome += (sender, e) => {
        Console.WriteLine("Manager Added Home: {0}",e.Home);
    };

    HomeManager.DidRemoveHome += (sender, e) => {
        Console.WriteLine("Manager Removed Home: {0}",e.Home);
    };
    HomeManager.DidUpdateHomes += (sender, e) => {
        Console.WriteLine("Manager Updated Homes");
    };
    HomeManager.DidUpdatePrimaryHome += (sender, e) => {
        Console.WriteLine("Manager Updated Primary Home");
    };
}
```

Uygulamayı ilk kez çalıştırdığınızda, kullanıcının kendi HomeKit bilgilerini erişmesine izin vermek isteyip istemediğinizi istenir:

[![](homekit-images/home01.png "Kullanıcı kendi HomeKit bilgilerini erişmesine izin vermek isteyip istemediğinizi istenir.")](homekit-images/home01.png#lightbox)

Kullanıcı yanıtını verirse **Tamam**, uygulamanın kendi HomeKit Donatılar ile çalışabilmek için sonra aksi içinde değil ve HomeKit yapılan her çağrı, bir hata ile başarısız olur.

Giriş yöneticisiyle yerinde, birincil giriş yapılandırdıysanız görmek ve değilse, oluşturmak ve bir atamak kullanıcı için bir yol sağlamak uygulama sonraki gerekir.

### <a name="accessing-the-primary-home"></a>Birincil Giriş erişme

Yukarıda belirtildiği gibi bir birincil giriş oluşturulan ve HomeKit kullanılabilir ve oluşturmak ve bir birincil giriş varsa atamak kullanıcı için bir yol sağlamak üzere uygulamanın sorumluluk zaten yoksa kullanılmadan önce yapılandırılması gerekir.

Uygulamanızı önce başlatır veya arka planından döndürür, izlemek gereken `DidUpdateHomes` olayı `HMHomeManager` birincil giriş varlığını denetlemek için sınıf. Bir mevcut değilse kullanıcının oluşturmak bir arabirim sağlamalıdır.

Aşağıdaki kod için birincil giriş denetlemek için bir görünüm denetleyicisi eklenebilir:

```csharp
using HomeKit;
...

public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
...

// Wireup events
ThisApp.HomeManager.DidUpdateHomes += (sender, e) => {

    // Was a primary home found?
    if (ThisApp.HomeManager.PrimaryHome == null) {
        // Ask user to add a home
        PerformSegue("AddHomeSegue",this);
    }
};
```

Giriş Yöneticisi HomeKit için bir bağlantı yaptığında `DidUpdateHomes` olay harekete geçen, varolan bir ev ev manager'ın koleksiyonuna yüklenir ve birincil giriş, varsa yüklenecek.

### <a name="adding-a-primary-home"></a>Birincil giriş ekleme

Varsa `PrimaryHome` özelliği `HMHomeManager` olan `null` sonra bir `DidUpdateHomes` olay oluşturmak ve devam etmeden önce birincil giriş atamak kullanıcı için bir yol sağlamanız gereken.

Genellikle uygulama kullanıcının ev Manager Kurulumu birincil giriş sayfası olarak iletilir, yeni bir ev adı bir form sunacaktır. İçin **HomeKitIntro** örnek uygulaması, kalıcı bir görünüm IOS Tasarımcısı'nda oluşturulan ve tarafından çağrılır `AddHomeSegue` uygulamanın ana arabiriminden ü.

Kullanıcının bir giriş eklemek için bir düğmeyi ve yeni giriş adı bir metin alanı sağlar. Kullanıcı ne zaman dokunur **Ekle giriş** düğmesi, aşağıdaki kod, ev giriş eklemek için Yöneticisi çağırır:

```csharp
// Add new home to HomeKit
ThisApp.HomeManager.AddHome(HomeName.Text,(home,error) =>{
    // Did an error occur
    if (error!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Add Home Error",string.Format("Error adding {0}: {1}",HomeName.Text,error.LocalizedDescription),this);
        return;
    }

    // Make the primary house
    ThisApp.HomeManager.UpdatePrimaryHome(home,(err) => {
        // Error?
        if (err!=null) {
            // Inform user of error
            AlertView.PresentOKAlert("Add Home Error",string.Format("Unable to make this the primary home: {0}",err.LocalizedDescription),this);
            return ;
        }
    });

    // Close the window when the home is created
    DismissViewController(true,null);
});
```

`AddHome` Yöntemi, yeni giriş oluşturup verilen geri arama yordamı dönmek deneyecek. Varsa `error` özelliği `null`, bir hata oluştu ve kullanıcıya sunulan. En yaygın hataları, benzersiz olmayan bir giriş adı ya da ev HomeKit ile iletişim kurmak durum Yöneticisi tarafından hatalardır.

Giriş başarıyla oluşturulduysa çağırmanız gerekir `UpdatePrimaryHome` yöntemi yeni giriş birincil giriş sayfası olarak ayarlayın. Yeniden varsa `error` özelliği `null`, bir hata oluştu ve kullanıcıya sunulan.

Giriş Yöneticisi'nin de izlemeniz `DidAddHome` ve `DidRemoveHome` uygulamanın kullanıcı arabirimi gerektiği şekilde olayları ve güncelleştirme.

> [!IMPORTANT]
> `AlertView.PresentOKAlert` Yukarıdaki örnek kodda kullanılan bir yardımcı sınıfı ile iOS uyarıları daha kolay çalışma yapar HomeKitIntro uygulamada yöntemidir.


## <a name="finding-new-accessories"></a>Yeni Donatılar bulma

Birincil Giriş tanımlanmış veya giriş Yöneticisi'nden yüklenmiş sonra Xamarin.iOS uygulamanızı çağırabilirsiniz `HMAccessoryBrowser` herhangi yeni giriş Otomasyon Donatılar bulmak ve onları bir giriş eklemek için.

Çağrı `StartSearchingForNewAccessories` için yeni Donatılar aramaya başlamak için yöntem ve `StopSearchingForNewAccessories` bittiğinde yöntemi.

> [!IMPORTANT]
> `StartSearchingForNewAccessories` uzun bir süre için pil ömrünün ve iOS cihazı performansını olumsuz etkiler çünkü çalıştıran bırakılmamalıdır. Apple öneren arama `StopSearchingForNewAccessories` minute veya bulma donatıyı UI kullanıcıya sunulduğunda yalnızca arama sonra.

`DidFindNewAccessory` Yeni Donatılar bulunan ve için eklenecek olay çağrılır `DiscoveredAccessories` donatıyı tarayıcıda listesi.

`DiscoveredAccessories` Listesi koleksiyonunu içerecek `HMAccessory` verin HomeKit tanımlayan nesneleri ev Otomasyon cihaz ve ışık veya Garaj kapı denetimi gibi kullanılabilir hizmetlerinin etkin.

Yeni donatıyı bulundu sonra kullanıcıya sunulan ve bu nedenle bunlar bunu seçebilir ve bir giriş ekleyin. Örnek:

[![](homekit-images/accessory01.png "Yeni bir donatıyı bulma")](homekit-images/accessory01.png#lightbox)

Çağrı `AddAccessory` seçili donatıyı giriş 's koleksiyona eklemek için yöntem. Örneğin:

```csharp
// Add the requested accessory to the home
ThisApp.HomeManager.PrimaryHome.AddAccessory (_controller.AccessoryBrowser.DiscoveredAccessories [indexPath.Row], (err) => {
    // Did an error occur
    if (err !=null) {
        // Inform user of error
        AlertView.PresentOKAlert("Add Accessory Error",err.LocalizedDescription,_controller);
    }
});
```

Varsa `err` özelliği `null`, bir hata oluştu ve kullanıcıya sunulan. Aksi takdirde kullanıcı eklemek cihaz için Kurulum kodu girmeniz istenir:

[![](homekit-images/accessory02.png "Eklenecek cihaz için Kurulum kodu girin")](homekit-images/accessory02.png#lightbox)

Bu sayı HomeKit donatıyı Benzeticisinde altında bulunabilir **Kurulum kodu** alan:

[![](homekit-images/accessory03.png "Kurulum kodu alanında HomeKit donatıyı Simulator")](homekit-images/accessory03.png#lightbox)

Gerçek HomeKit Donatılar Kurulum kodu ya da cihazın kendisi, ürün kutusunun veya donatıyı ait kullanıcı kılavuzuna üzerindeki bir etikette bulunan basılır.

Aksesuar tarayıcının izlemelidir `DidRemoveNewAccessory` olay ve güncelleştirme kullanıcı arabirimi kullanıcı, kendi giriş koleksiyona eklenmiş sonra bir donatıyı kullanılabilir listesinden kaldırmak için.

## <a name="working-with-accessories"></a>Donatılar ile çalışma

Bir kez birincil ev ve kurulmuş ve Donatılar için eklenmiştir, kullanıcının çalışmak Donatılar (ve isteğe bağlı olarak odaları) listesini sunabilir.

`HMRoom` Nesnesi, verilen yer ile ilgili tüm bilgileri ve kendisine ait Donatılar içerir. Odaları isteğe bağlı olarak bir veya daha fazla bölgelere düzenlenebilir. A `HMZone` ve kendisine ait odaları tümünün verilen bölge hakkında tüm bilgiler verilmektedir.

Bu örnek amacıyla, biz şeyler basit ve odaları veya bölgelere düzenlemek yerine doğrudan ev 's Donatılar ile çalışmak tutma.

`HMHome` Nesne kullanıcıya sunulan atanan donatıyı listesini içeren kendi `Accessories` özelliği. Örneğin:

[![](homekit-images/accessory04.png "Bir örnek donatıyı")](homekit-images/accessory04.png#lightbox)

Burada formunda, kullanıcının belirli bir donatıyı seçin ve sağladığı hizmetler ile çalışmak.

## <a name="working-with-services"></a>Hizmetleri ile çalışma

Belirli bir HomeKit etkin ev Otomasyon aygıtla kullanıcı etkileşim genellikle sağladığı hizmetler aracılığıyla olur. `Services` Özelliği `HMAccessory` sınıfı içeren bir koleksiyonu `HMService` hizmetleri tanımlayan nesneleri bir aygıtı sunar.

Işık, termostatlar, Garaj kapı ayrıcalığına, anahtarlar veya kilitleri gibi şeyleri hizmetleridir. Bazı cihazlara (örneğin, bir Garaj kapı açan) ışık ve açmak veya kapatmak bir kapı yeteneği gibi birden fazla hizmeti sağlar.

Belirli bir donatıyı sağlayan belirli hizmetlere ek olarak, her donatıyı içeren bir `Information Service` adı, üreticisi, modeli ve seri numarası gibi özellikleri tanımlar.

### <a name="accessory-service-types"></a>Aksesuar hizmet türleri

Aşağıdaki hizmet türleri aracılığıyla kullanılabilir `HMServiceType` enum:

- **AccessoryInformation** -verilen giriş Otomasyon aygıt (donatıyı) hakkında bilgi sağlar.
- **AirQualitySensor** -hava kalite algılayıcı tanımlar.
- **Pil** -bir donatıyı 's pil durumunu tanımlar.
- **CarbonDioxideSensor** -karbon Dioksit algılayıcı tanımlar.
- **CarbonMonoxideSensor** -karbon Monoxide algılayıcı tanımlar.
- **ContactSensor** -kişi algılayıcı (örneğin, bir pencere açıldığında veya kapatıldığında) tanımlar.
- **Kapı** -(gibi açık veya kapalı) kapı durumu algılayıcı tanımlar.
- **Fan** -Uzaktan denetlenen fan tanımlar.
- **GarageDoorOpener** -Garaj kapı açan tanımlar.
- **HumiditySensor** -nem algılayıcı tanımlar.
- **LeakSensor** -sızıntısı algılayıcısı (like etkin su ısıtıcısı veya Bulaşık makinesi) tanımlar.
- **Ampul** -bir tek başına ışık veya başka bir donatıyı (gibi Garaj kapı açan) parçası olan bir ışık tanımlar.
- **LightSensor** -hafif algılayıcı tanımlar.
- **LockManagement** -otomatik kapının kilidi yöneten bir hizmet tanımlar.
- **LockMechanism** -Uzaktan denetlenen kilitleme (gibi bir kapının kilidi) tanımlar.
- **MotionSensor** -hareket algılayıcı tanımlar.
- **OccupancySensor** -doluluk algılayıcı tanımlar.
- **Çıkış** -Uzaktan denetlenen duvar çıkışı tanımlar.
- **SecuritySystem** -giriş güvenlik sistemi tanımlar.
- **StatefulProgrammableSwitch** -kez (Çevir gibi bir anahtar) tetiklenen verin durumda kalır programlanabilir bir anahtar tanımlar.
- **StatelessProgrammableSwitch** -(bir düğme gibi) tetiklenen sonra ilk durumuna geri döner programlanabilir bir anahtar tanımlar.
- **SmokeSensor** -Duman algılayıcı tanımlar.
- **Geçiş** -açık/kapalı anahtar bir standart duvar anahtar gibi tanımlar.
- **Sıcaklık algılayıcısı** -sıcaklık algılayıcısı tanımlar.
- **Thermostat** -HVAC sistem denetlemek için kullanılan bir akıllı thermostat tanımlar.
- **Pencere** -cane uzaktan açık veya kapalı, otomatik bir pencere tanımlar.
- **WindowCovering** -, açık veya kapalı Panjurlar gibi kapsayan Uzaktan denetlenen bir pencere tanımlar.

### <a name="displaying-service-information"></a>Hizmet bilgileri görüntüleme

Yükleme sonrasında sonra bir `HMAccessory` tek tek sorgu `HNService` nesneleri sağlar ve bu bilgileri kullanıcıya görüntüler:

[![](homekit-images/accessory05.png "Hizmet bilgileri görüntüleme")](homekit-images/accessory05.png#lightbox)

Her zaman denetlemelisiniz `Reachable` özelliği bir `HMAccessory` ile çalışmak denemeden önce. Bir donatıyı kullanıcı aygıt aralık içinde değil, erişilemiyor veya onu takılı olup olmadığını olabilir.

Bir hizmet seçildikten sonra kullanıcı görüntülemek veya izlemek veya belirtilen giriş Otomasyon cihazı denetlemek için bu hizmet bir veya daha fazla özelliklerini değiştirin.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>Özellikleri ile çalışma

Her `HMService` nesne koleksiyonu içerebilir `HMCharacteristic` (gibi açılan veya kapatılan bir kapı) hizmet durumu hakkındaki bilgileri belirtebilir veya (ışık rengini ayarlama gibi) bir durumda ayarlamasına izin nesneleri.

`HMCharacteristic` yalnızca bir özellik ve durumu hakkında bilgi sağlar, ancak aynı zamanda durumu ile birlikte çalışmak için yöntemler sağlar _özellik meta verileri_ (`HMCharacteristisMetadata`). Bu meta veriler (örneğin, en az düzeyde değeri aralık) için kullanıcı veya vermeden durumları değiştirmek için bilgileri görüntülerken, yararlı olan özellikler sağlar.

`HMCharacteristicType` Enum tanımlanan veya şu şekilde değiştirilmiş özellik meta veri değerleri kümesi sağlar:

 - AdminOnlyAccess
 - AirParticulateDensity
 - AirParticulateSize
 - AirQuality
 - AudioFeedback
 - BatteryLevel
 - Parlaklığını
 - CarbonDioxideDetected
 - CarbonDioxideLevel
 - CarbonDioxidePeakLevel
 - CarbonMonoxideDetected
 - CarbonMonoxideLevel
 - CarbonMonoxidePeakLevel
 - ChargingState
 - ContactState
 - CoolingThreshold
 - CurrentDoorState
 - CurrentHeatingCooling
 - CurrentHorizontalTilt
 - CurrentLightLevel
 - CurrentLockMechanismState
 - CurrentPosition
 - CurrentRelativeHumidity
 - CurrentSecuritySystemState
 - CurrentTemperature
 - CurrentVerticalTilt
 - FirmwareVersion
 - HardwareVersion
 - HeatingCoolingStatus
 - HeatingThreshold
 - HoldPosition
 - Hue
 - Tanımlayın
 - InputEvent
 - LeakDetected
 - LockManagementAutoSecureTimeout
 - LockManagementControlPoint
 - LockMechanismLastKnownAction
 - Günlükleri
 - Üretici
 - Model
 - MotionDetected
 - Ad
 - ObstructionDetected
 - OccupancyDetected
 - OutletInUse
 - OutputState
 - PositionState
 - PowerState
 - RotationDirection
 - RotationSpeed
 - Doygunluk
 - seri numarası
 - SmokeDetected
 - SoftwareVersion
 - StatusActive
 - StatusFault
 - StatusJammed
 - StatusLowBattery
 - StatusTampered
 - TargetDoorState
 - TargetHeatingCooling
 - TargetHorizontalTilt
 - TargetLockMechanismState
 - TargetPosition
 - TargetRelativeHumidity
 - TargetSecuritySystemState
 - TargetTemperature
 - TargetVerticalTilt
 - TemperatureUnits
 - Sürüm

### <a name="working-with-a-characteristics-value"></a>Bir özellik'ın değeri ile çalışma

Bu, uygulama belirli bir özellik son durumu sağlamak için çağrı `ReadValue` yöntemi `HMCharacteristic` sınıfı. Varsa `err` özelliği `null`, bir hata oluştu ve kullanıcıya sunulan değil veya olabilir.

Özelliğe ait `Value` özelliği belirtilen özellik geçerli durumunu içeren bir `NSObject`ve bu nedenle ile doğrudan C# dilinde çalışması olamaz.

Değerini okumak için aşağıdaki yardımcı sınıfı eklendi **HomeKitIntro** örnek uygulama:

```csharp
using System;
using Foundation;
using System.Globalization;
using CoreGraphics;

namespace HomeKitIntro
{
    /// <summary>
    /// NS object converter is a helper class that helps to convert NSObjects into
    /// C# objects
    /// </summary>
    public static class NSObjectConverter
    {
        #region Static Methods
        /// <summary>
        /// Converts to an object.
        /// </summary>
        /// <returns>The object.</returns>
        /// <param name="nsO">Ns o.</param>
        /// <param name="targetType">Target type.</param>
        public static Object ToObject (NSObject nsO, Type targetType)
        {
            if (nsO is NSString) {
                return nsO.ToString ();
            }

            if (nsO is NSDate) {
                var nsDate = (NSDate)nsO;
                return DateTime.SpecifyKind ((DateTime)nsDate, DateTimeKind.Unspecified);
            }

            if (nsO is NSDecimalNumber) {
                return decimal.Parse (nsO.ToString (), CultureInfo.InvariantCulture);
            }

            if (nsO is NSNumber) {
                var x = (NSNumber)nsO;

                switch (Type.GetTypeCode (targetType)) {
                case TypeCode.Boolean:
                    return x.BoolValue;
                case TypeCode.Char:
                    return Convert.ToChar (x.ByteValue);
                case TypeCode.SByte:
                    return x.SByteValue;
                case TypeCode.Byte:
                    return x.ByteValue;
                case TypeCode.Int16:
                    return x.Int16Value;
                case TypeCode.UInt16:
                    return x.UInt16Value;
                case TypeCode.Int32:
                    return x.Int32Value;
                case TypeCode.UInt32:
                    return x.UInt32Value;
                case TypeCode.Int64:
                    return x.Int64Value;
                case TypeCode.UInt64:
                    return x.UInt64Value;
                case TypeCode.Single:
                    return x.FloatValue;
                case TypeCode.Double:
                    return x.DoubleValue;
                }
            }

            if (nsO is NSValue) {
                var v = (NSValue)nsO;

                if (targetType == typeof(IntPtr)) {
                    return v.PointerValue;
                }

                if (targetType == typeof(CGSize)) {
                    return v.SizeFValue;
                }

                if (targetType == typeof(CGRect)) {
                    return v.RectangleFValue;
                }

                if (targetType == typeof(CGPoint)) {
                    return v.PointFValue;
                }
            }

            return nsO;
        }

        /// <summary>
        /// Convert to string
        /// </summary>
        /// <returns>The string.</returns>
        /// <param name="nsO">Ns o.</param>
        public static string ToString(NSObject nsO) {
            return (string)ToObject (nsO, typeof(string));
        }

        /// <summary>
        /// Convert to date time
        /// </summary>
        /// <returns>The date time.</returns>
        /// <param name="nsO">Ns o.</param>
        public static DateTime ToDateTime(NSObject nsO){
            return (DateTime)ToObject (nsO, typeof(DateTime));
        }

        /// <summary>
        /// Convert to decimal number
        /// </summary>
        /// <returns>The decimal.</returns>
        /// <param name="nsO">Ns o.</param>
        public static decimal ToDecimal(NSObject nsO){
            return (decimal)ToObject (nsO, typeof(decimal));
        }

        /// <summary>
        /// Convert to boolean
        /// </summary>
        /// <returns><c>true</c>, if bool was toed, <c>false</c> otherwise.</returns>
        /// <param name="nsO">Ns o.</param>
        public static bool ToBool(NSObject nsO){
            return (bool)ToObject (nsO, typeof(bool));
        }

        /// <summary>
        /// Convert to character
        /// </summary>
        /// <returns>The char.</returns>
        /// <param name="nsO">Ns o.</param>
        public static char ToChar(NSObject nsO){
            return (char)ToObject (nsO, typeof(char));
        }

        /// <summary>
        /// Convert to integer
        /// </summary>
        /// <returns>The int.</returns>
        /// <param name="nsO">Ns o.</param>
        public static int ToInt(NSObject nsO){
            return (int)ToObject (nsO, typeof(int));
        }

        /// <summary>
        /// Convert to float
        /// </summary>
        /// <returns>The float.</returns>
        /// <param name="nsO">Ns o.</param>
        public static float ToFloat(NSObject nsO){
            return (float)ToObject (nsO, typeof(float));
        }

        /// <summary>
        /// Converts to double
        /// </summary>
        /// <returns>The double.</returns>
        /// <param name="nsO">Ns o.</param>
        public static double ToDouble(NSObject nsO){
            return (double)ToObject (nsO, typeof(double));
        }
        #endregion
    }
}
```

`NSObjectConverter` Uygulama her bir özellik geçerli durumunu okuma gerektiğinde kullanılır. Örneğin:

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

Yukarıdaki satırında değerine dönüştürür bir `float` sonra kullanılabilecek Xamarin C# kodu.

Değiştirmek için bir `HMCharacteristic`, çağrı kendi `WriteValue` yöntemi ve yeni değeri sarmalayın bir `NSObject.FromObject` çağırın. Örneğin:

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

Varsa `err` özelliği `null`, bir hata oluştu ve kullanıcıya sunulmaz.

### <a name="testing-characteristic-value-changes"></a>Özellik değeri değiştiğinde test etme

İle çalışırken `HMCharacteristics` benzetimli Donatılar, değişiklikler ve `Value` özelliği içinde HomeKit donatıyı Simulator izlenebilir.

İle **HomeKitIntro** gerçek iOS cihaz donanım, bir özelliğe ait değer değişiklikleri üzerinde çalışan uygulama görülme neredeyse anında HomeKit donatıyı Benzeticisinde. Örneğin, bir iOS uygulaması ışığı durumunu değiştirme:

[![](homekit-images/test01.png "Bir iOS uygulaması ışığında durumunu değiştirme")](homekit-images/test01.png#lightbox)

Durumu HomeKit donatıyı Simulator ışığında değiştirmelisiniz. Değer değişmezse yeni özellik değerleri yazılırken hata iletisi durumunu denetleyin ve donatıyı hala erişilebilir olduğundan emin olun.

## <a name="advanced-homekit-features"></a>Gelişmiş HomeKit özellikleri

Bu makalede, bir Xamarin.iOS uygulaması içinde HomeKit Donatılar ile çalışmak için gereken temel özellikleri ele. Ancak, bu giriş kapsamadığı birkaç Gelişmiş özellikleri HomeKit vardır:

- **Odaları** -etkin HomeKit Donatılar isteğe bağlı olarak son kullanıcı tarafından odaları içine düzenlenir. Bu kullanıcının anlamak ve bunlarla çalışmak kolay bir şekilde mevcut Donatılar HomeKit sağlar. Apple'nın oluşturmak ve odaları sürdürmek daha fazla bilgi için bkz: [HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) belgeleri.
- **Bölgeleri** -odaları isteğe bağlı olarak düzenlenmiş bölgelere son kullanıcı tarafından. Bir bölgenin kullanıcı tek bir birim olarak davranır odaları koleksiyonu ifade eder. Örneğin: Upstairs, Downstairs veya zemin kat. Yeniden, bu sunmak ve Donatılar son kullanıcı için anlamlı bir şekilde çalışmak HomeKit sağlar. Apple'nın oluşturma ve bölgelerinin bakımıyla ilgili daha fazla bilgi için bkz: [HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) belgeleri.
- **Eylemler ve eylem ayarlar** -eylemleri aksesuar hizmet özelliklerini değiştirmek ve ayarlar gruplandırılabilir. Eylem kümelerini Donatılar birtakım denetlemek ve eylemlerini koordine etmek için komut dosyaları olarak davranır. Örneğin, "İzleme TV" betik Panjurlar kapatın, ışık dim ve televizyon ve ses sistemini açın. Apple'nın oluşturma ve eylemleri ve eylem kümelerini koruma hakkında daha fazla bilgi için bkz: [HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) ve [HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) belgeleri.
- **Tetikleyiciler** - bir tetikleyici bir etkinleştirebilir veya daha fazla eylem ayarlamak verilen dizi koşula zaman karşılandığından. Örneğin, portch ışığını açmak ve tüm dış kapı dışında koyu aldığında kilitleyin. Apple'nın oluşturma ve Tetikleyicileri bakımı hakkında daha fazla bilgi için bkz: [HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) belgeleri.

Yukarıda gösterilen aynı teknikleri bu özellikleri kullanan olduğundan, bunlar aşağıdaki Apple tarafından uygulamak kolay olmalıdır [HomeKitDeveloper Kılavuzu](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html), [HomeKit kullanıcı arabirimi yönergelerine](https://developer.apple.com/homekit/ui-guidelines/) ve [ HomeKit Framework başvurusu](https://developer.apple.com/library/ios/home_kit_framework_ref).

## <a name="homekit-app-review-guidelines"></a>HomeKit uygulama gözden geçirme yönergeleri

Bir HomeKit göndermeden önce iTunes Bağlan iTunes App Store'da yayımı için etkin Xamarin.iOS uygulamasına olun etkin HomeKit uygulamaları için Apple'nın yönergeleri izleyin:

 - Uygulamanın birincil amacı _gerekir_ HomeKit Framework'te kullanıyorsanız ev Otomasyon olabilir.
 - Uygulamanın pazarlama metin HomeKit kullanılır ve bir gizlilik ilkesi sağlamaları gerekir, kullanıcıların bildirmeniz gerekir.
 - Kullanıcı bilgilerini toplama veya reklam amaçlı HomeKit kullanarak kesinlikle yasaktır.

İçin tam yönergeleri gözden geçirin, lütfen bkz. Apple'nın [App Store gözden geçirme yönergeleri](https://developer.apple.com/app-store/review/guidelines/).

## <a name="whats-new-in-ios-9"></a>İOS 9 yeni nedir

Apple aşağıdaki değişiklikler ve eklemeler HomeKit için iOS 9 yaptı:

- **Var olan nesneleri koruma** - mevcut bir donatıyı değiştirilirse, giriş Yöneticisi (`HMHomeManager`), değiştirilen belirli öğeyi size bildirir.
- **Kalıcı tanımlayıcıları** -tüm ilgili HomeKit sınıfları artık dahil bir `UniqueIdentifier` belirli bir öğe HomeKit arasında benzersiz şekilde tanımlamak için özelliği etkin uygulamalar (veya aynı uygulama örnekleri).
- **Kullanıcı Yönetimi** -birincil kullanıcının giriş HomeKit cihazlara erişimi olan kullanıcılar üzerinden kullanıcı yönetimi sağlamak için bir yerleşik görünüm denetleyicisi eklendi.
- **Kullanıcı özellikleri** - HomeKit kullanıcıların artık bir dizi içinde HomeKit kullanabilmek için hangi işlevleri denetleyen ayrıcalık vardır ve HomeKit Donatılar etkin. Uygulamanız, geçerli kullanıcı için ilgili özellikleri yalnızca görüntülemelidir. Örneğin, yalnızca bir yöneticileri diğer kullanıcıların korumak gerekir.
- **Planda önceden tanımlanmış** -ortalama HomeKit kullanıcıdan oluşan dört ortak olaylar için önceden tanımlanmış planda oluşturulmuştur: hale getirmek, bırakın, dönüş, yatak için gidin. Bu önceden tanımlanmış planda bir evden silinemiyor.
- **Planda ve Siri** -Siri HomeKit içinde tanımlanan Sahne adını iOS 9 ve can görünümlerde tanımak için derin destek vardır. Bir kullanıcı adının Siri konuşarak Sahne yürütebilir.
- **Aksesuar kategorileri** -bir dizi önceden tanımlanmış kategoriler tüm Donatılar ve ev eklenmekte olan donatıyı türünü tanımlamak için yardımcı eklenecek veya üzerinde gelen uygulamanızda çalışılan. Bu yeni kategoriler donatıyı kurulumu sırasında kullanılabilir.
- **Apple Watch Destek** - HomeKit artık watchOS için kullanılabilir durumdadır ve Apple Watch denetimi HomeKit izleme olan bir iPhone olmadan cihazları etkinleştirilmiş yapabilirsiniz. HomeKit watchOS için aşağıdaki özellikleri destekler: görüntüleme ev, Donatılar denetleme ve planda çalıştırma.
- **Yeni Olay Tetikleyici türü** - iOS 8, iOS 9'u destekler olay tetikleyicileri donatıyı durumu (örneğin, algılayıcı verilerini) veya coğrafi konuma göre artık desteklenen Zamanlayıcı türü Tetikleyicileri yanı sıra. Olay tetikleyicileri kullanmak `NSPredicates` kendi yürütme için koşullar ayarlamak için.
- **Uzaktan erişim** -ile uzaktan erişim, kullanıcı şimdi denetleyebilmeniz house uzak bir konumdaki uzakta olduklarında kendi HomeKit giriş Otomasyon Donatılar etkin. İOS 8 3. nesil Apple TV evde bir kullanıcı sahipse, bu yalnızca destekleniyordu. İOS 9, bu sınırlamaya kaldırılmış ve Uzaktan erişim iCloud ve HomeKit donatıyı Protokolü (HAP) aracılığıyla desteklenir.
- **Yeni Bluetooth düşük enerji (bırak) yeteneklerini** -HomeKit şimdi Bluetooth düşük enerji (bırak) protokolü iletişim kurabilir daha fazla aksesuar türlerini destekler. (Bluetooth aralık dışında ise) HAP güvenli Tünelleme kullanarak HomeKit donatıyı başka bir Bluetooth donatıyı Wi-Fi hale getirebilir. İOS 9'da, bırak Donatılar bildirimleri ve meta veriler için tam destek vardır.
- **Yeni donatıyı kategoriler** -Apple iOS 9 aşağıdaki yeni donatıyı kategoriler eklenen: pencere Coverings, moped kapıları ve Windows, Alarm sistemleri, algılayıcılar ve programlanabilir anahtarlar.

Apple iOS 9 HomeKit yeni özellikler hakkında daha fazla bilgi için lütfen bkz [HomeKit dizin](https://developer.apple.com/homekit/) ve [HomeKit yenilikler](https://developer.apple.com/videos/wwdc/2015/?id=210) video.

## <a name="summary"></a>Özet

Bu makalede, Apple'nın HomeKit ev Otomasyonu çerçevesinin anlatılmıştır. Kurulum ve HomeKit donatıyı Simulator kullanarak test cihazları yapılandırma ve bulmak için iletişim ve HomeKit kullanarak giriş Otomasyon cihazları denetlemek için basit bir Xamarin.iOS uygulaması oluşturma gösterdi.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [Yeni iOS 9.0 nedir](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper Kılavuzu](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit kullanıcı arabirimi yönergeleri](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Framework başvurusu](https://developer.apple.com/library/ios/home_kit_framework_ref)
