---
title: Dinamik yeniden yükle
description: XAML değişikliklerin Canlı, başka bir derleme gerektirmeden yansımasını bakın ve dağıtın.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 04/23/2018
ms.openlocfilehash: 627225fdeef781a8b24a79e9b46627a739fd15af
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="xamarin-live-reload"></a>Xamarin dinamik yeniden yükle

![Önizleme](~/media/shared/preview.png)

Xamarin Canlı yeniden olanak tanır **XAML için değişiklik ve canlı, başka bir derleme gerektirmeden yansıtılan görebileceği ve dağıtma**. XAML için yapılan tüm değişiklikler imzalanmasını kaydetme ve dağıtma hedefte yansıtılır.

Uygulamanızın Canlı yeniden kullanırken derlendiğinden tüm kitaplıkları ve üçüncü taraf denetimleri ile çalışır. Xamarin.Forms destekler, Android, iOS ve UWP, dahil ve benzeticileri, Öykünücüler ve fiziksel cihazları dahil olmak üzere tüm geçerli dağıtım hedefleri üzerinde çalışan tüm platformlarda yeniden works dinamik.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Dinamik yeniden yükleme şu anda yalnızca Visual Studio 2017 içinde kullanılabilir.

## <a name="requirements"></a>Gereksinimler

* [Visual Studio 2017 15.7 Önizleme 4](https://www.visualstudio.com/vs/preview/) veya üstü ile **.NET ile Mobil Geliştirme** iş yükü.
* [Xamarin.Forms 3.0.354232-pre3](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3) veya üstü.

## <a name="getting-started"></a>Başlarken
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Visual Studio marketten Xamarin dinamik yeniden yükleyin

Xamarin Canlı yeniden Visual Studio Market'te dağıtılır. Uzantıyı yüklemek için ziyaret [Visual Studio Market'te Xamarin Canlı yeniden sayfasında](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) Web sitesi ve tıklatın **karşıdan**.

İndirilir ve .vsix açmak **yükleme**.

![Visual Studio yükleyicisi Xamarin Canlı yükle onay](images/LiveReloadVSIXInstall.png)

Alternatif olarak, içinde arayabilirsiniz **çevrimiçi** sekmesinde **Uzantılar ve güncelleştirmeler** Visual Studio içinde iletişim.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Dinamik yeniden kullanmak için uygulamanızı yapılandırma

Dinamik yeniden varolan mobil uygulamalara ekleme, üç adımda yapılabilir:

1. Tüm projeleri kullanacak şekilde güncelleştirilir olun [Xamarin.Forms 3.0.354232-pre3](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3) veya üstü.
2. Yükleme **Xamarin.LiveReload** NuGet .NET standart 2.0 kitaplığa. Bu platform projelerinizde yüklü olması gerekmez. Emin **paket kaynağı** ayarlanır **tüm**.

![Xamarin dinamik yeniden NuGet ile NuGet Paket Yöneticisi ekleme](images/addlivereloadnuget.png)

3. Ekleme `LiveReload.Init();` oluşturucuda için `Application` aşağıdaki kod parçacığında gösterildiği gibi sınıfı:

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        LiveReload.Init();
    
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. Dinamik yeniden Başlat

Derleme ve uygulamanızı dağıtın. Uygulama dağıtılmış olduğunda, XAML dosyasını açın, bazı değişiklikler yapın ve dosyayı kaydedin. Değişikliklerinizi dağıtım hedef imzalanmasını.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Dinamik herhangi XAML dosyası yapılan değişikliklerle yeniden yüklemeye çalışır. C# veya NuGet paketleri ekleme/kaldırma değişiklikler yeni bir yapı gerektirir ve etkili olması için dağıtın.

## <a name="frequently-asked-questions"></a>Sıkça Sorulan Sorular 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Xamarin Canlı yeniden Mac için Visual Studio var mı? 

Xamarin Canlı yeniden ilk önizleme sürümü, yalnızca Visual Studio 2017 için kullanılabilir. Mac için Visual Studio desteği için gelecekteki bir sürüm planlanmaktadır.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Bu Prizma gibi tüm kitaplıkları ile çalışır mı? 

Uygulamanızı derlendiğinden Prizma ve üçüncü taraf denetimi kitaplıkları gibi Telerik, Infragistics, Syncfusion, ArcGIS, GrapeCity ve diğer denetim satıcıları gibi tüm kitaplıkları ile canlı yeniden yüklemeye çalışır.

### <a name="what-changes-does-live-reload-redeploy"></a>Hangi değişiklikleri Canlı yeniden dağıtmanız? 

Dinamik yeniden yalnızca XAML için yapılan değişiklikleri uygular. Bir C# dosyasına değişiklik yaparsanız, yeniden derlemeye gerekli olacaktır. C# yeniden desteği için gelecekteki bir sürüm planlanmaktadır.

### <a name="what-platforms-are-supported"></a>Hangi platformlar desteklenir? 

Android, iOS ve UWP dahil Xamarin.Forms tarafından desteklenen herhangi bir platformda dinamik yeniden yüklemeye çalışır.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Bu Öykünücüler, benzeticileri ve fiziksel cihazlar üzerinde çalışıyor mu? 

Evet, Canlı yeniden Android öykünücüsünü, iOS benzeticileri ve fiziksel cihazları dahil olmak üzere tüm geçerli dağıtım hedefleri ile çalışır. Bir aygıt için dağıtım bilgisayar ve aygıt aynı Wi-Fi ağ üzerinde olmalıdır.

### <a name="does-this-work-with-corporate-networks"></a>Bu, şirket ağlarına ile çalışır mı?

Bir Android öykünücü veya iOS simülatörü ayıkladığınız, Canlı yeniden localhost iletişim kurmak için kullanır. Bir aygıta dağıtmak istiyorsanız, bilgisayar ve aygıt aynı Wi-Fi ağ üzerinde olması gerekir. Bu olduğu olası senaryolarda yapabilecekleriniz [kendi Canlı yeniden sunucunuzu yapılandırmak](#live-reload-server), hangi etkinleştirir, Canlı yeniden yükleme için ağ bağlantı ayarlarını bağımsız olarak.

### <a name="does-it-require-debugging-the-app"></a>Uygulama hata ayıklama gerektiriyor mu? 

Hayır. Aslında, hatta tüm desteklenen uygulama hedeflerinizi (Android, iOS ve UWP) herhangi bir sayıda cihaz veya benzeticileri/Öykünücüler üzerinde başlatabilir ve bunların tümünü tek seferde güncelleştirme görmek. 

## <a name="limitations"></a>Sınırlamalar

* Yalnızca XAML yeniden yüklenmesi desteklenir.
* Kullanıcı Arabirimi durumu yeniden dağıtır arasında MVVM kullanmadığınız sürece korunmasını değil.

## <a name="known-issues"></a>Bilinen Sorunlar

* Yalnızca Visual Studio'da desteklenir.
* Yalnızca .NET standart kitaplıkları ile çalışır. Bu, sonraki Önizleme sürümünde düzeltilecektir.
* CSS stil sayfaları desteklenmez. Bu, sonraki Önizleme sürümünde düzeltilecektir.
* Uygulama genelinde kaynakları yeniden (yani **App.xaml** veya kaynak sözlüklerindeki paylaşılan), uygulama gezinti sıfırlanır. Bu, sonraki Önizleme sürümünde düzeltilecektir.
* UWP hata ayıklama çalışma zamanı çökmeyle neden olabilir ancak XAML düzenleme. Geçici çözüm: Kullanmak **başlatın (Ctrl + F5) hata ayıklama olmadan** yerine **hata ayıklama (F5) Başlangıç**.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="error-codes"></a>Hata kodları

* **XLR001**: *geçerli projeye başvuruda bulunan 'Xamarin.LiveReload' NuGet Paket sürümü [VERSION] ancak Xamarin Canlı yeniden uzantısı sürümü [VERSION] gerektirir.*

  Hızlı yineleme ve canlı yeniden yükle özelliği evrimi izin vermek üzere nuget paketi ve Visual Studio uzantısı tam olarak eşleşmelidir. Nuget paketi yüklediğiniz uzantısı aynı sürüme güncelleştirin.

* **XLR002**: *Canlı yeniden en azından 'MqttHostname' özelliği komut satırından derlerken. Alternatif olarak, 'EnableLiveReload', 'özelliği devre dışı bırakmak için false' olarak ayarlayın.*

  Dinamik yeniden tarafından gerekli özelliklerinin ne zaman kullanılabilir değil komut satırından (veya sürekli tümleştirme) oluşturma ve açıkça sağlanmalıdır. 

* **XLR003**: *Canlı yeniden nuget paketi gerektirir Xamarin Canlı yeniden Visual Studio uzantısı yükleme.*

  Dinamik yeniden nuget paketi başvuran bir projeyi derleme denedi ancak Visual uzantısı yüklü değil.  



### <a name="app-doesnt-connect"></a>Uygulama bağlanmıyor

Zaman uygulaması oluşturulur, bilgileri **Araçlar > Seçenekler > Xamarin > dinamik yeniden** (ana bilgisayar adı, bağlantı noktası ve şifreleme anahtarları) katıştırılmış uygulamada, bu nedenle olduğunda `LiveReload.Init()` çalıştığında, hiçbir eşleştirme veya yapılandırma olduğu bağlantının başarılı olması gereklidir.

Normal ağ sorunları dışında (Güvenlik Duvarı, farklı bir ağ aygıtı), uygulamanın başarıyla IDE bağlanılamayabilir ana nedeni çünkü yapılandırmasıyla Visual Studio'da bir farklılık gösterir. Bu durumlarda ortaya çıkabilir:

* Uygulama farklı bir makineye derlendi.
* Uygulama derlenmiş ve farklı bir Visual Studio oturumunda dağıtılan ve **şifreleme anahtarları otomatik oluştur** olan (varsayılan) iade **araçları > Seçenekler > Xamarin > Canlı yeniden**.
* Visual Studio ayarları (yani, ana bilgisayar adı, bağlantı noktası veya şifreleme anahtarları) değişti ancak uygulama yerleşik değil ve yeniden dağıtılabilir.

Bu durumların tüm oluşturma ve uygulama yeniden dağıtma çözülür.

### <a name="uninstalling-preview-1"></a>Önizleme 1 kaldırma

Eski bir önizleme varsa ve kaldırmayı sorunlar yaşıyorsanız, aşağıdaki adımları izleyin:

1. Klasörünü silmek **C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (Not: "Kurumsal" yüklü sürümünüz ile değiştirin ve "ile"2017"ise, Önizleme" kararlı bir VS yüklenir)
2. Açık bir **Geliştirici komut istemi** , Visual Studio ve çalıştırma için `devenv /updateconfiguration`. 

## <a name="tips--tricks"></a>İpuçları ve püf noktaları

* Dinamik yeniden yükleme ayarlarını değiştirmeyin sürece (şifreleme anahtarları dahil olmak üzere gelmesi gibi dışı **otomatik oluşturma şifreleme anahtarları**) ve aynı makineden derleme, derleme ve sonra ilk uygulama dağıtma gerekmez kod veya bağımlılıkları değiştirmediğiniz sürece dağıtın. Yalnızca önceden dağıtılmış bir uygulamayı yeniden başlatabilirsiniz ve son kullanılan ana bilgisayarı bağlanır.

* Aynı Visual Studio oturuma bağlanabilirsiniz kaç cihazlarda herhangi bir kısıtlama yoktur. Dağıtma ve dinamik reloading çalışma hepsinde aynı anda görmek için gereken sayıda aygıtları/benzeticileri uygulamayı başlatın.

* Dinamik yeniden yalnızca uygulamanızı kullanıcı arabirimi bölümünü yeniden ancak bunu yapar *değil* sayfalarınızı yeniden oluştururken, görünüm modeli (veya bağlama içeriği), değiştirin. Yani *tüm* uygulama durumu eklenen bağımlılıklar dahil olmak üzere yeniden yükler arasında her zaman korunur.

## <a name="live-reload-server"></a>Dinamik yeniden sunucu

Senaryolarda bağlantı burada çalışan uygulamalardan makinenize (kullanarak gösterilen `localhost` veya `127.0.0.1` içinde **araçları > Seçenekler > Xamarin > Canlı yeniden**) mümkün değildir (yani güvenlik duvarları, farklı ağlarda) Uzak bir sunucusu bunun yerine, hangi IDE ve uygulama yapılandırabileceğiniz conect için.

Dinamik yeniden kullanan standart [MQTT Protokolü](http://mqtt.org/) için exchange iletileri ve bu nedenle ile iletişim kurabildiğini [üçüncü taraf sunucular](https://github.com/mqtt/mqtt.github.io/wiki/servers). Var olan bile [ortak sunucuları](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (olarak da bilinen *aracıların*) kullanılabilir, kullanabilirsiniz. Dinamik yeniden test edilmiştir ile `broker.hivemq.com` ve `iot.eclipse.org` tarafından sağlanan hizmetlerin yanı sıra ana bilgisayar adlarını [www.cloudmqtt.com](https://www.cloudmqtt.com) ve [www.cloudamqp.com](https://www.cloudamqp.com). Bulutta, kendi MQTT sunucusu gibi dağıtabilirsiniz [azure'da HiveMQ](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud).

Herhangi bir bağlantı yapılandırabilirsiniz, ancak uzak sunucular için varsayılan 1883 bağlantı noktası kullanmak için yaygın bir durumdur. Uzak sunuculara bağlanmak güvenli olması için dinamik yeniden iletileri güçlü uçtan uca AES simetrik şifreleme kullanır. Varsayılan olarak, şifreleme anahtarını ve başlatma vektörü (IV) her Visual Studio oturumunda yeniden oluşturulur.

Büyük olasılıkla en kolay yolu yüklemektir [mosquitto](https://mosquitto.org) azure'da boş bir Ubuntu VM Server'da:

1. Azure Portalı'nda yeni bir Ubuntu Server VM oluşturma
2. Ağ sekmesinde 1883 (varsayılan MQTT bağlantı noktası) için yeni bir gelen bağlantı noktası kuralı ekleme
3. Açık [bulut Kabuk](https://docs.microsoft.com/azure/cloud-shell/overview) (modu bash)
4. Yazın `ssh [USERNAME]@[PUBLIC_IP]` 1'de seçtiğiniz kullanıcı adı kullanarak) ve VM genel bakış sayfanıza gösterilen genel IP
5. Çalıştırma `sudo apt-get install mosquitto`, 1'de seçtiğiniz parola girme)

Artık kendi MQTT sunucusuna bağlanmak için bu IP kullanabilirsiniz.
