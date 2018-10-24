---
title: Canlı yeniden yükleme
description: Başka bir derleme gerek kalmadan canlı, XAML değişiklikler yansıtılır bakın ve dağıtın.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 05/11/2018
ms.openlocfilehash: ce48c4d271167b657505c52518e79c955e53b02e
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "38860673"
---
# <a name="xamarin-live-reload"></a>Xamarin Canlı yeniden yükleme

![Önizleme](~/media/shared/preview.png)

Xamarin Live yeniden olanak tanır **değişiklik yapmak için XAML ve bunları başka bir derleme gerek kalmadan canlı, yansıtılan görebilir ve dağıtma**. XAML için yapılan değişiklikler yeniden kaydedin ve dağıtım hedefinizin yansıtılır.

Uygulamanızı Canlı yeniden yükleme kullanırken derlendiğinden tüm kitaplıkları ve üçüncü taraf denetimleri ile çalışır. Xamarin.Forms destekler, Android, iOS ve UWP, dahil ve simülatörleri, Öykünücüler ve fiziksel cihazlar da dahil olmak üzere tüm geçerli dağıtım hedefleri üzerinde çalışan tüm platformlarda yeniden works Canlı.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Canlı yeniden yükleme şu anda yalnızca Visual Studio 2017'de kullanılabilir.

[![Sohbete katılın https://gitter.im/xamarin/live-reload](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/xamarin/live-reload?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## <a name="requirements"></a>Gereksinimler

* [Visual Studio 2017 sürüm 15.7 veya üzeri](https://visualstudio.microsoft.com/vs/) ile **.NET ile Mobil Geliştirme** iş yükü.
* [Xamarin.Forms 3.0.0 veya yukarıdaki](https://www.nuget.org/packages/Xamarin.Forms/).

## <a name="getting-started"></a>Başlarken
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Visual Studio Market'ten Xamarin Canlı yeniden yükleme

Xamarin Live yeniden Visual Studio Market aracılığıyla dağıtılır. Uzantıyı yüklemek için şurayı ziyaret edin [Visual Studio Market Xamarin Canlı yeniden yükleme sayfasında](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) tıklayın ve Web **indirme**.

İndirilir ve .vsix açın **yükleme**.

![Visual Studio yükleyicisi Xamarin Canlı yeniden yükleme onayı](images/LiveReloadVSIXInstall.png)

Alternatif olarak, içinde arama yapabilirsiniz **çevrimiçi** sekmesinde **Uzantılar ve güncelleştirmeler** Visual Studio içinde iletişim.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Canlı yeniden yükleme kullanmak için uygulamanızı yapılandırın

Canlı yeniden yükleme ekleme için var olan mobil uygulamaları üç adımda yapılabilir:

1. Tüm projeler, kullanılacak güncelleştirilir sağlamak [Xamarin.Forms 3.0.0 veya yukarıdaki](https://www.nuget.org/packages/Xamarin.Forms/) veya üzeri.

2. Ekleme **Xamarin.LiveReload** NuGet paketi:

    a. **.NET standard** – yükleme **Xamarin.LiveReload** NuGet, .NET Standard 2.0 kitaplığa. Bu platform projelerinizde yüklü olması gerekmez. Emin **paket kaynağı** ayarlanır **tüm**.
    
    b. **Paylaşılan projeleri** – yükleme **Xamarin.LiveReload** tüm platform proje içinde NuGet (Android gibi iOS, UWP, vb.). Emin **paket kaynağı** ayarlanır **tüm**.

    [![Xamarin Canlı yeniden yükleme NuGet ile NuGet Paket Yöneticisi Ekle](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. Ekleme `LiveReload.Init();` içinde oluşturucuya `Application` aşağıdaki kod parçacığında gösterildiği gibi sınıfı:

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        #if DEBUG
        LiveReload.Init();
        #endif
        
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. Canlı yeniden Başlat

Derleme ve uygulamanızın dağıtabilirsiniz. Dağıtılan uygulama başladıktan sonra bir XAML dosyasını açın, bazı değişiklikler yapmanız ve dosyayı kaydedin. Dağıtım hedefi için değişikliklerinizi yeniden.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Canlı. herhangi bir XAML dosyasını değişikliklerle yeniden çalışır. C# veya NuGet paketlerini ekleme/kaldırma değişiklikler yeni bir derleme gerektirir ve etkili olması için dağıtın.

## <a name="frequently-asked-questions"></a>Sıkça Sorulan Sorular 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Xamarin Live yeniden Mac için Visual Studio kullanılabilir? 

Xamarin Live yeniden ilk önizleme sürümü yalnızca Visual Studio 2017 için kullanılabilir. Mac için Visual Studio için destek gelecekteki sürümlerde sunulması planlanmaktadır.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Bu, Prism gibi tüm kitaplıkları ile çalışır mı? 

Uygulamanızı derlendiğinden, Canlı yeniden yükleme Prism ve üçüncü taraf denetimi kitaplıkları gibi Telerik, Infragistics, Syncfusion, Arcgıs, GrapeCity ve diğer denetim satıcıları gibi tüm kitaplıkları ile çalışır.

### <a name="what-changes-does-live-reload-redeploy"></a>Canlı yeniden yükleme değişiklikleri yeniden? 

Canlı yeniden yükleme, yalnızca XAML ya da CSS yapılan değişiklikleri uygular. Bir C# dosyasına değişiklik yaparsanız çalışabilmeleri gerekli olacaktır. C# yeniden yükleme desteği, gelecekteki sürümlerde sunulması planlanmaktadır.

### <a name="what-platforms-are-supported"></a>Hangi platformlar desteklenir? 

Canlı yeniden yükleme, Android, iOS ve UWP gibi Xamarin.Forms tarafından desteklenen tüm platformlarda çalışır.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Bu, Öykünücüler, simülatör ve fiziksel cihazlarda çalışır mı? 

Evet, Canlı yeniden yükleme Android öykünücüleri ve iOS simülatörleri fiziksel cihazlar da dahil olmak üzere tüm geçerli dağıtım hedefleri ile çalışır. Dağıtım için bir cihaz, cihaz ve bilgisayar aynı Wi-Fi ağında olmasını gerektirir.

### <a name="does-this-work-with-corporate-networks"></a>Bu, Kurumsal ağlar ile çalışır mı?

Bir Android öykünücüsü veya iOS simülatörü ayıklıyorsanız, Canlı yeniden yükleme localhost iletişim kurmak için kullanır. Bir aygıta dağıtmak istiyorsanız, cihaz ve bilgisayar aynı Wi-Fi ağında olması gerekir. Bu olduğu olası senaryolarda yapabilecekleriniz [kendi Canlı yeniden yükleme sunucu yapılandırma](#live-reload-server), hangi etkinleştirir, Canlı yeniden yükleme için ağ bağlantısı ayarlarına bakılmaksızın.

### <a name="does-it-require-debugging-the-app"></a>Uygulamayı hata ayıklama gerektiriyor mu? 

Hayır. Aslında, hatta tüm desteklenen uygulama hedeflerinizi (Android, iOS ve UWP) herhangi bir sayıda cihaz veya simülatör/Öykünücüler üzerinde başlatabilir ve tümünü tek seferde güncelleştirme konusuna bakın. 

## <a name="limitations"></a>Sınırlamalar

* Yalnızca XAML yeniden yüklenmesi desteklenir.
* UI durumu yeniden dağıtır arasında MVVM kullanmadığınız sürece yönetilebilir değil.

## <a name="known-issues"></a>Bilinen Sorunlar

* Yalnızca Visual Studio içinde desteklenir.
* Bağlama ayarlanmalıdır **bağlama** veya **yalnızca bağlantı Framework SDK'ları** 
* Uygulama genelinde kaynakları yeniden (yani **App.xaml** veya paylaşılan kaynak sözlükleri), gezintisini sıfırlanır. Bu, sonraki Önizleme sürümünde düzeltilecektir.
* ContentView şu anda yeniden yüklemeyi içeren sayfayı yeniden yüklemeyi gerektirir. Bu, sonraki Önizleme sürümünde düzeltilecektir.
* Automationıd içeren öğeleri yeniden başarısız olmasına neden.
* UWP hata ayıklama çalışma zamanı Çökmeye neden, ancak XAML düzenleme. Geçici çözüm: Kullanın **Başlat (Ctrl + F5) hata ayıklama olmadan** yerine **Start Debugging (F5)**.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="error-codes"></a>Hata kodları

* **XLR001**: *geçerli projenin 'Xamarin.LiveReload' NuGet paketi sürüm [sürüm] öğesine başvuruyor, ancak Xamarin Live yeniden uzantısı sürüm [sürüm] gerektirir.*

  Hızlı yineleme ve canlı yeniden yükleme özelliği evrimi izin vermek üzere nuget paketi ve Visual Studio uzantısı tam olarak eşleşmelidir. Nuget paketiniz, uzantının yüklü olduğu aynı sürüme güncelleştirin.

* **XLR002**: *Canlı yeniden yükleme en azından 'MqttHostname' özelliği komut satırından derlerken. Alternatif olarak, 'EnableLiveReload', 'özelliği devre dışı bırakmak için false' olarak ayarlayın.*

  Canlı yeniden yükleme tarafından gereken özellikleri ne zaman kullanılabilir değildir komut satırından (veya sürekli tümleştirme) oluşturma ve bu nedenle açıkça belirtilmelidir. 

* **XLR003**: *Canlı yeniden yükleme nuget paketini gerektirir Xamarin Live yeniden Visual Studio uzantısı yükleniyor.*

  Canlı yeniden yükleme nuget paketini başvuran bir projeyi derleme çalışıldı, ancak Visual uzantısı yüklü değil.  

* *Derlemeler yüklenirken özel durum oluştu: System.IO.FileNotFoundException: derleme yüklenemiyor ' Xamarin.Live.Reload, sürüm 0.3.27.0, Culture = neutral, PublicKeyToken = ='.*

  Ana proje kullanarak `PackageReference` yerine `packages.config`

### <a name="app-doesnt-connect"></a>Uygulama bağlanmıyor

Zaman uygulama oluşturulduğuna göre bilgileri **Araçlar > Seçenekler > Xamarin > Canlı yeniden yükleme** (ana bilgisayar adını, bağlantı noktası ve şifreleme anahtarları) katıştırıldığı uygulamada, bu nedenle olduğunda `LiveReload.Init()` çalıştığında, hiç eşleştirme veya yapılandırma olan bağlantının başarılı olması gereklidir.

Normal ağ sorunları dışında (Güvenlik Duvarı, farklı bir ağ aygıtı) uygulama başarıyla IDE bağlanılamayabilir temel nedeni çünkü Visual Studio'da bir yapılandırmasını farklıdır. Bu durum oluşabilir:

* Farklı bir makineye uygulama derlendi.
* Uygulama derlendi ve farklı bir Visual Studio oturumunda dağıtılan ve **şifreleme anahtarları otomatik olarak oluşturmak** olduğunu (varsayılan) iade **Araçlar > Seçenekler > Xamarin > Canlı yeniden yükleme**.
* Visual Studio ayarları (yani, ana bilgisayar adı, bağlantı noktası veya şifreleme anahtarları) değiştirilmiş ancak uygulama yerleşik değil ve yeniden dağıtılabilir.

Bu gibi durumlarda tüm oluşturmak ve uygulamayı yeniden dağıtma çözülür.

### <a name="uninstalling-preview-1"></a>Kaldırma Önizleme 1

Eski bir önizleme varsa ve kaldırmayı sorunlar yaşıyorsanız, aşağıdaki adımları izleyin:

1. Klasör Sil **C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (Not: "Kurumsal", yüklü olan sürümü ile değiştirin ve "ile"2017"ise, Preview" kararlı bir VS yüklü)
2. Açık bir **Geliştirici komut istemi** , Visual Studio ve çalıştırma için `devenv /updateconfiguration`. 

## <a name="tips--tricks"></a>İpuçları ve püf noktaları

* Canlı yeniden yükleme ayarlarını değiştirme sürece (şifreleme anahtarı dahil olmak üzere, IF gibi devre dışı **şifreleme anahtarları otomatik olarak oluşturmak**) ve aynı makine oluşturun, sonra ilk uygulaması derleyecek ve gerekmez kodu veya bağımlılıkları değiştirmediğiniz sürece dağıtın. Yalnızca daha önce Dağıtılmış bir uygulamayı yeniden başlatabilir ve son kullanılan konak bağlanır.

* Aynı Visual Studio oturumuna bağlanabilir kaç cihazlarda bir sınırlama yoktur. Dağıtım ve canlı yeniden yükleme çalışma hepsinde aynı anda görmek için gerektiği kadar cihazları/simülatörleri uygulamayı başlatın.

* Canlı yeniden yükleme yalnızca uygulamanızın kullanıcı arabirimi bölümünü yeniden, ancak mevcut *değil* sayfalarınızı yeniden oluşturma, görünüm model (veya bağlama bağlamı) diğerinden değiştirin. Başka bir deyişle *tüm* uygulama durumunu, eklenen bağımlılıklar dahil olmak üzere yeniden yükler arasında her zaman korunur.

## <a name="live-reload-server"></a>Canlı yeniden yükleme sunucu

Senaryolarda bağlantı burada çalışan bir uygulamadan makinenize (kullanarak sunulduğu şekilde `localhost` veya `127.0.0.1` içinde **Araçlar > Seçenekler > Xamarin > Canlı yeniden yükleme**) mümkün değildir (yani güvenlik duvarları, farklı ağlarda) Uzak bir sunucuya bunun yerine, IDE ve uygulama hem yapılandırabileceğiniz conect için.

Canlı yeniden yükleme kullanan standart [MQTT protokolünü](http://mqtt.org/) için mesaj alışverişi ve bu nedenle ile iletişim kurabildiğini [üçüncü taraf sunucuya](https://github.com/mqtt/mqtt.github.io/wiki/servers). Var olan bile [ortak sunucuları](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (olarak da bilinir *aracıları*) kullanılabilir, kullanabilirsiniz. Canlı yeniden yükleme ile edilmiştir `broker.hivemq.com` ve `iot.eclipse.org` tarafından sağlanan hizmetlerin yanı sıra, ana bilgisayar adları [www.cloudmqtt.com](https://www.cloudmqtt.com) ve [www.cloudamqp.com](https://www.cloudamqp.com). Bulutta kendi MQTT server gibi dağıtabilirsiniz [azure'da HiveMQ](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud).

Herhangi bir bağlantı yapılandırabilirsiniz, ancak uzak sunucular için varsayılan 1883 bağlantı noktası kullanımı yaygındır. Uzak sunuculara bağlanmak güvenli bir şekilde güçlü uçtan uca AES simetrik şifreleme, Canlı yeniden yükleme iletileri kullanın. Varsayılan olarak, hem şifreleme anahtarı hem de başlatma vektörü (IV) üzerinde her Visual Studio oturumu yeniden oluşturulur.

Büyük olasılıkla yüklemek için en kolay yolu olan [mosquitto](https://mosquitto.org) azure'da boş bir Ubuntu sanal makinesi sunucusu:

1. Azure portalında yeni bir Ubuntu Server VM oluşturma
2. Ağ sekmesinde 1883 (varsayılan MQTT bağlantı noktası) için yeni bir gelen bağlantı noktası kuralı Ekle
3. Açık [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (bash modu)
4. Türü `ssh [USERNAME]@[PUBLIC_IP]` 1'de seçtiğiniz kullanıcı adı kullanarak) ve VM genel bakış sayfasında gösterilen ortak IP
5. Çalıştırma `sudo apt-get install mosquitto`, 1'de seçtiğiniz parola girerek)

Artık kendi MQTT sunucusuna bağlanmak için bu IP kullanabilirsiniz.
