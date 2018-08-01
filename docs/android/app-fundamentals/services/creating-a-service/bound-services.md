---
title: Xamarin.Android içinde Hizmetleri bağlı
description: Bağımlı hizmetler (örneğin, bir Android etkinlik) istemci ile etkileşim kurabilen bir istemci-sunucu arabirimi Android hizmetlerdir. Bu kılavuz, ilişkili hizmet ve bir Xamarin.Android uygulaması kullanma oluşturmayla ilgili anahtar bileşenleri ele alınacaktır.
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 05/04/2018
ms.openlocfilehash: f4fe1bd753260f05dedb452655572d290c0781d0
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33850580"
---
# <a name="bound-services-in-xamarinandroid"></a>Xamarin.Android içinde Hizmetleri bağlı

_Bağımlı hizmetler (örneğin, bir Android etkinlik) istemci ile etkileşim kurabilen bir istemci-sunucu arabirimi Android hizmetlerdir. Bu kılavuz, ilişkili hizmet ve bir Xamarin.Android uygulaması kullanma oluşturmayla ilgili anahtar bileşenleri ele alınacaktır._

## <a name="bound-services-overview"></a>İlişkili Hizmetleri'ne Genel Bakış

İstemcilerin doğrudan etkileşim bir istemci-sunucu arabirimi ile hizmet sağlayan hizmetler denir _bağlı Hizmetleri_.  Birden çok istemci aynı anda tek bir hizmet örneğine bağlı olabilir. Bağlı hizmet ve istemci birbirinden yalıtılır. Bunun yerine, Android iki arasındaki bağlantının durumunu yöneten Ara nesneleri bir dizi sağlar. Bu durum uygulayan nesne tarafından korunur [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/) arabirimi.  Bu nesne istemci tarafından oluşturulan ve bir parametre olarak geçirilen [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/) yöntemi. `BindService` Herhangi kullanılabilir [ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context) nesne (örneğin, bir etkinlik). Hizmeti başlatın ve bir istemci için bağlamak için Android işletim sistemine bir istektir. Var olan bir istemci için üç yol bağlama kullanarak bir hizmet `BindService` yöntemi:

* **Bir hizmet Bağlayıcısı** &ndash; bir hizmet Bağlayıcısı uygulayan bir sınıftır [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder) arabirimi. Uygulamaların çoğu bu arabirimin doğrudan gerçekleştireceksiniz değil, bunun yerine uzatır [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) sınıfı. Bu en yaygın yöntem ve ne zaman hizmeti ve istemci aynı işlem içinde mevcut için uygundur.
* **Bir Messenger kullanarak** &ndash; Bu teknik hizmet ayrı bir işlemde zaman varolabilir için uygundur. Bunun yerine, istemci ve hizmet aracılığıyla arasında hizmet istekleri sıraya bir [ `Android.OS.Messenger` ](https://developer.xamarin.com/api/type/Android.OS.Messenger). Bir [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler) işleyecek hizmetinde oluşturulan `Messenger` istekleri. Başka bir Kılavuzu'nda ele alınacaktır.
* **AIDL kullanarak** &ndash; bu çoğu Android geliştiricileri Kalp içinde terör sağlar ve bu kılavuzda ele alınmamaktadır.

Bir istemci bir hizmete bağlı sonra ikisi arasındaki iletişimidir aracılığıyla oluşur `Android.OS.IBinder` nesnesi.  Bu nesne, hizmet ile etkileşim kurmak istemci sağlayacak arabirimi sorumludur. Bunu gerekli olmadığı sıfırdan bu arabirim uygulamak her Xamarin.Android uygulaması, Android SDK'sı sağlar [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) nesnesi arasında dizimi ile gerekli kodu çoğunu ilgilenir sınıfı İstemci ve hizmet.

Bir istemci hizmeti ile işiniz bittiğinde, bu ondan çağırarak bağlantısını gerekir `UnbindService` yöntemi. Son istemci bir hizmetinden ilişkisiz sonra Android durdurun ve ilişkili hizmet dispose.

Bu diyagramda, birbirleriyle nasıl etkinlik, hizmet bağlantı, bağlayıcı ve hizmet tüm ilişkili gösterilir:

![Hizmet bileşenleri birbirleriyle nasıl ilişkili olduğunu gösteren bir diyagram](bound-services-images/bound-services-02.png "ve hizmet bileşenlerinin birbirleriyle nasıl ilişkili olduğunu gösteren diyagram.")

Bu kılavuz nasıl genişletileceğini ele alınacaktır `Service` ilişkili hizmet uygulanacak sınıf. Ayrıca uygulama ele alınacaktır `IServiceConnection` ve genişletme `Binder` bir istemci hizmetiyle iletişim kurmasına izin vermek için. Adlı tek bir Xamarin.Android projesi ile bir çözüm içerir, bu kılavuz, örnek bir uygulama eşlik **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** . Bu, bir hizmeti uygulama ve nasıl etkinlik bağlanacağını gösterir çok basit bir uygulamadır. Yalnızca bir yöntemiyle çok basit bir API bağlı hizmeti olan `GetFormattedTimestamp`, kullanıcının hizmeti başlatıldığında bildiren bir dize ve ne kadar süreyle çalıştırıldığını döndürür. Uygulamanın, ayrıca el ile bağlantısını kesmek ve hizmete bağlamak kullanıcı verir.

[![Android phone'da çalışan uygulamasının ekran görüntüsü](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>Uygulama ve ilişkili hizmet kullanma

Bağlı hizmeti kullanmak bir Android uygulaması uygulanması gereken üç bileşeni vardır:

1. **Genişletme `Service` sınıfı ve yaşam döngüsü geri çağırma yöntemleri uygulamak** &ndash; Bu sınıf hizmet istenen iş gerçekleştirecek bir kod bulunacaktır. Aşağıdaki daha ayrıntılı olarak ele alınacaktır.
2. **Bu uygulayan bir sınıf oluşturun `IServiceConnection`**  &ndash; bu arabirim sağlayan geri arama yöntemleri olacak çağrılan istemci hizmeti bağlantısı değiştiğinde bildirmek için Android tarafından yani istemci bağlı için bağlantısı kesilmiş veya var. hizmet. Hizmet bağlantı ayrıca istemcinin doğrudan hizmetiyle etkileşim kurmak için kullanabileceği bir nesneye başvuru sağlar. Bu başvuru olarak bilinen _bağlayıcı_.
3. **Bu uygulayan bir sınıf oluşturun `IBinder`**  &ndash; A _bağlayıcı_ uygulamasını bir istemci hizmeti ile iletişim kurmak için kullandığı bir API sağlar. Bağlayıcı istemci yalıtır ve ilişkili hizmet uygulamasından gizler API sağlayabilir veya bağlayıcı ya da bağlı hizmetine başvuru doğrudan çağrılacak yöntemlerine olanak sağlayabilir. Bir `IBinder` gerekli kodu uzak yordam çağrıları için sağlamanız gerekir. Gerekli (veya önerilen) olmadığından uygulamak için `IBinder` doğrudan arabirim. Bunun yerine uygulamaların genişletmelidir `Binder` çoğu gerektirdiği temel işlevleri sağlayan türü bir `IBinder`.
4. **Başlangıç ve bir hizmetine bağlama** &ndash; hizmeti bağlantısı, bağlayıcı ve hizmeti oluşturulduktan sonra Android uygulama hizmetini başlatma ve kendisine bağlama sorumludur.

Bu adımların her biri aşağıdaki bölümlerde daha ayrıntılı açıklanmıştır.

### <a name="extend-the-service-class"></a>Genişletme `Service` sınıfı

Xamarin.Android kullanarak bir hizmet oluşturmak için bir alt kümesi için ise gerekli `Service` ve ekleyin sınıfıyla [ `ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute). Öznitelik Xamarin.Android derleme araçları tarafından düzgün şekilde hizmet uygulamanın içinde kaydetmek için kullanılan **AndroidManifest.xml** bir etkinlik benzer dosya, bağımlı bir hizmet ile ilişkili kendi yaşam döngüsü ve geri arama yöntemleri vardır buna ait yaşam döngüsü önemli olayları. Aşağıdaki listede, bazı hizmet gerçekleştireceksiniz daha yaygın geri arama yöntemleri bir örnektir:

* `OnCreate` &ndash; Bu yöntem, hizmet örneği olarak Android tarafından çağrılır. Tüm değişkenleri veya hizmet tarafından kendi ömrü boyunca gereken nesneleri başlatmak için kullanılır. Bu yöntem isteğe bağlıdır.
* `OnBind` &ndash; Bu yöntem, tüm bağımlı hizmetleri tarafından uygulanmalıdır. İlk istemci hizmete bağlanmayı denediğinde çağrılır. Örneği döndürür `IBinder` böylece istemci hizmeti ile etkileşimde bulunabilir. Hizmetin çalıştığını sürece `IBinder` nesne hizmetine bağlamak için tüm gelecekteki istemci isteklerini yerine getirmek için kullanılacak.
* `OnUnbind` &ndash; Tüm bağlı istemcileri ilişkisiz olduğunda bu yöntem çağrılır. Döndürerek `true` bu yöntemi, hizmet daha sonra çağıracak `OnRebind` geçirilen amacıyla `OnUnbind` zaman yeni istemciler bağlamak için. İlişkisiz sağlandıktan sonra çalışan bir hizmeti devam ettiğinde bunu. Yakın zamanda ilişkisiz hizmeti de başlatılan bir hizmet olsaydı olacağını ve `StopService` veya `StopSelf` olarak adlandırılan yüklediniz. Böyle bir senaryoda `OnRebind` alınması yapma sağlar. Varsayılan döndürür `false` , hangi hiçbir şey yapmaz. İsteğe bağlı.
* `OnDestroy` &ndash; Android hizmet yok etme, bu yöntem çağrılır. Bu yöntemde kaynakları serbest bırakma gibi gerekli temizleme gerçekleştirilmesi gerekir. İsteğe bağlı.

Bağlı hizmetin anahtar yaşam döngüsü olayları Bu diyagramda gösterilmektedir:

![Yaşam döngüsü yöntemleri çağrılmadan sırayı gösteren bir diyagram](bound-services-images/bound-services-01.png "yaşam döngüsü yöntemleri çağrılmadan sipariş gösteren diyagram.")

Bu kılavuz, eşlik Yardımcısı uygulamasından aşağıdaki kod parçacığını bir bağlı hizmeti Xamarin.Android içinde uygulama gösterir:

```csharp
using Android.App;
using Android.Util;
using Android.Content;
using Android.OS;

namespace BoundServiceDemo
{
    [Service(Name="com.xamarin.ServicesDemo1")]
    public class TimestampService : Service, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampService).FullName;
        IGetTimestamp timestamper;

        public IBinder Binder { get; private set; }

        public override void OnCreate()
        {
            // This method is optional to implement
            base.OnCreate();
            Log.Debug(TAG, "OnCreate");
            timestamper = new UtcTimestamper();
        }

        public override IBinder OnBind(Intent intent)
        {
            // This method must always be implemented
            Log.Debug(TAG, "OnBind");
            this.Binder = new TimestampBinder(this);
            return this.Binder;
        }

        public override bool OnUnbind(Intent intent)
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnUnbind");
            return base.OnUnbind(intent);
        }

        public override void OnDestroy()
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnDestroy");
            Binder = null;
            timestamper = null;
            base.OnDestroy();
        }

        /// <summary>
        /// This method will return a formatted timestamp to the client.
        /// </summary>
        /// <returns>A string that details what time the service started and how long it has been running.</returns>
        public string GetFormattedTimestamp()
        {
            return timestamper?.GetFormattedTimestamp();
        }
    }
}
```

Örnekte, `OnCreate` yöntemi almaya ve istemci tarafından istenen bir zaman damgası biçimlendirme mantığı tutan bir nesne başlatır. İlk istemci hizmetine bağlanmaya çalıştığında, Android çağıracağı `OnBind` yöntemi. Bu hizmet örneği bir `TimestampServiceBinder` çalışan service'nın bu örneğinin erişmesine olanak sağlayacak nesnesi. `TimestampServiceBinder` Sınıfı bir sonraki bölümde ele alınmıştır.

### <a name="implementing-ibinder"></a>IBinder uygulama

Belirtildiği gibi bir `IBinder` nesnesi, istemci ve hizmet arasındaki iletişim kanalını sağlar. Android uygulamaları uygulamaz `IBinder` arabirimi, [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/) genişletilmesi. `Binder` Sınıfı gerekli sıralama olan gerekli altyapıyı çoğunu istemciye (, ayrı bir işlemde çalışıyor olması) hizmetinden bağlayıcı nesnesi sağlar. Çoğu durumda, `Binder` alt yalnızca birkaç satır kod ve hizmetine başvuru sarmalar. Bu örnekte, `TimestampBinder` kullanıma sunan bir özelliğe sahiptir `TimestampService` istemciye:

```csharp
public class TimestampBinder : Binder
{
    public TimestampBinder(TimestampService service)
    {
        this.Service = service;
    }

    public TimestampService Service { get; private set; }
}
```

Bu `Binder` hizmette; ortak yöntemleri çağırma mümkün kılar örneğin:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

Bir kez `Binder` bırakıldı genişletilmiş, uygulamak ise gerekli `IServiceConnection` her şeyi birbirine bağlamak için.

### <a name="creating-the-service-connection"></a>Hizmet bağlantı oluşturma

`IServiceConnection` Sunacaktır | tanıtmak | kullanıma | bağlanmak `Binder` istemciye nesnesi. Uygulama yanı sıra `IServiceConnection` arabirimi, sınıf genişlemelidir `Java.Lang.Object`. Hizmet bağlantı ayrıca istemcinin erişebildiği bir şekilde sağlamalıdır `Binder` (ve bu nedenle bağlı hizmetiyle iletişim).

Bu kodu ile birlikte gelen örnek projedir uygulamak için olası bir yol `IServiceConnection`:

```csharp
using Android.Util;
using Android.OS;
using Android.Content;

namespace BoundServiceDemo
{
    public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampServiceConnection).FullName;

        MainActivity mainActivity;
        public TimestampServiceConnection(MainActivity activity)
        {
            IsConnected = false;
            Binder = null;
            mainActivity = activity;
        }

        public bool IsConnected { get; private set; }
        public TimestampBinder Binder { get; private set; }

        public void OnServiceConnected(ComponentName name, IBinder service)
        {
            Binder = service as TimestampBinder;
            IsConnected = this.Binder != null;

            string message = "onServiceConnected - ";
            Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

            if (IsConnected)
            {
                message = message + " bound to service " + name.ClassName;
                mainActivity.UpdateUiForBoundService();
            }
            else
            {
                message = message + " not bound to service " + name.ClassName;
                mainActivity.UpdateUiForUnboundService();
            }

            Log.Info(TAG, message);
            mainActivity.timestampMessageTextView.Text = message;

        }

        public void OnServiceDisconnected(ComponentName name)
        {
            Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
            IsConnected = false;
            Binder = null;
            mainActivity.UpdateUiForUnboundService();
        }

        public string GetFormattedTimestamp()
        {
            if (!IsConnected)
            {
                return null;
            }

            return Binder?.GetFormattedTimestamp();
        }
    }
}

```

Bağlama işleminin parçası olarak, Android çağıracağı `OnServiceConnected` yöntemi, sağlama `name` bağlanan hizmetinin ve `binder` hizmetine başvuru içerir. Bu örnekte, iki özellik bağlayıcısını ve bir Boole bayrağı için bir başvuru veya hizmete istemci bağlıysa tutan bir hizmet bağlantı vardır.

`OnServiceDisconnected` Yöntemi yalnızca çağrılan bir istemci ve hizmet arasındaki bağlantı beklenmedik bir şekilde kaybolduğunda veya bozuk. Bu yöntem istemcinin hizmet kesintisini yanıt olanağı sağlar.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>Başlangıç ve açık amacına sahip bir hizmet bağlama

Bir bağlı hizmeti kullanmak için bir istemcinin (örneğin, bir etkinlik) arabirimini uygulayan bir nesneye oluşturmalıdır `Android.Content.IServiceConnection` ve çağırma `BindService` yöntemi.` BindService` Döndürülecek `true` hizmet için bağlıysa `false` değilse. `BindService` Yöntemi üç parametreleri alır:

* **Bir `Intent`**  &ndash; hedefi bağlanmak için hangi hizmet açıkça tanımlamak.
* **Bir `IServiceConnection` nesne** &ndash; bağlı hizmeti başlatıldığında ve durdurulduğunda istemci bildirmek için geri çağırma yöntemleri sağlayan bir aracı bu nesnesidir.
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) Enum** &ndash; Bu parametre bayrakları kümesi olduğu sistem tarafından ne zaman nesneyi bağlamak için kullanılır. En yaygın kullanılan değer [ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/), hangi otomatik olarak başlatılacak hizmet zaten çalışıyor durumda değilse.

Aşağıdaki kod parçacığını bir bağlı hizmetinin açık amacına kullanarak bir etkinlikte nasıl başlatılacağı örneğidir:

```csharp
protected override void OnStart ()
{
    base.OnStart ();

    if (serviceConnection == null)
    {
        this.serviceConnection = new TimestampServiceConnection(this);
    }

    Intent serviceToStart = new Intent(this, typeof(TimestampService));
    BindService(serviceToStart, this.serviceConnection, Bind.AutoCreate);

}
```

> [!IMPORTANT]
> Bunu Android 5.0 (API düzeyi 21) ile başlayan yalnızca açık amacına sahip bir hizmet bağlamak mümkün olur.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>Hizmet bağlantı ve bağlayıcıyı ilgili mimari notları.

Bazı OOP purists önceki uygulaması onayını geri `TimestampBinder` sınıf ihlal olarak [yasalar, Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) , basit haliyle, durumları "tanımadığınız için; konuşun yok yalnızca arkadaşlarınıza konuşabilir". Bu belirli bir uygulamaya somut sunan `TimestampService` tüm istemcilere sınıfı.

NET olarak söylemek hakkında bilgi edinmek istemcinin gerekli değil `TimestampService` ve istemcilere somut o sınıfın gösterme yapabilir uygulamanın daha kırılır ve kendi ömürleri boyunca korumak daha zor. Alternatif bir yaklaşım sunan bir arabirim kullanmaktır `GetFormattedTimestamp()` yöntemi ve hizmet aracılığıyla proxy çağrıları `Binder` (veya olası hizmet bağlantı sınıfı):  

```csharp
public class TimestampBinder : Binder, IGetTimestamp
{
    TimestampService service;
    public TimestampBinder(TimestampService service)
    {
        this.service = service;
    }

    public string GetFormattedTimestamp()
    {
        return service?.GetFormattedTimestamp();
    }
}
```

Söz konusu örnekte service yöntemleri çağırmak bir etkinlik izin ver:

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```


## <a name="related-links"></a>İlgili bağlantılar

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.Content.Bind](https://developer.xamarin.com/api/type/Android.Content.Bind/)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Android.Content.IServiceConnection](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/)
- [Android.OS.Binder](https://developer.xamarin.com/api/type/Android.OS.Binder/)
- [Android.OS.IBinder](https://developer.xamarin.com/api/type/Android.OS.IBinder)
- [BoundServiceDemo (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/BoundServiceDemo/)
