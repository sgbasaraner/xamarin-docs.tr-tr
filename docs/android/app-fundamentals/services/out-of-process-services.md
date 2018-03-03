---
title: "Uzak işlemlerde çalışan Android Hizmetleri"
description: "Genellikle, bir Android uygulama içindeki tüm bileşenleri aynı işlem içinde çalışır. Bunlar kendi işlemlerini çalıştırmak için yapılandırılabilir ve diğer Android geliştiricilerden dahil olmak üzere, diğer uygulamalarla paylaşılan, android Hizmetleri bu önemli bir istisnadır. Bu kılavuz, Xamarin kullanarak Android bir uzak hizmet oluşturmak nasıl ele alınacaktır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 26609043e872241a2ec4f878086b97b12b064e87
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="running-android-services-in-remote-processes"></a>Uzak işlemlerde çalışan Android Hizmetleri

_Genellikle, bir Android uygulama içindeki tüm bileşenleri aynı işlem içinde çalışır. Bunlar kendi işlemlerini çalıştırmak için yapılandırılabilir ve diğer Android geliştiricilerden dahil olmak üzere, diğer uygulamalarla paylaşılan, android Hizmetleri bu önemli bir istisnadır. Bu kılavuz, Xamarin kullanarak Android bir uzak hizmet oluşturmak nasıl ele alınacaktır._

## <a name="out-of-process-services-overview"></a>Dışı işlem hizmetlerine genel bakış

Bir uygulama başlatıldığında Android uygulamayı çalıştırmak için bir işlem oluşturur. Tüm bileşenler genellikle, bu tek bir işlemde uygulamayı çalıştırın. Bunlar kendi işlemlerini çalıştırmak için yapılandırılabilir ve diğer Android geliştiricilerden dahil olmak üzere, diğer uygulamalarla paylaşılan, android Hizmetleri bu önemli bir istisnadır. Bu tür hizmet olarak adlandırılır _uzak Hizmetleri_ veya _işlem dışı Hizmetleri_. Bu hizmetler için kodu aynı APK ana uygulama olarak yer alır; Ancak, hizmeti başlatıldığında Android yalnızca bu hizmet için yeni bir işlem oluşturur. Buna karşılık, uygulamanın geri kalanına aynı işlemde çalışan hizmet bazen olarak adlandırılır bir _yerel hizmet_.

Genel olarak, bir uzak hizmet uygulamak bir uygulama için gerekli değildir. Yerel bir hizmet yeterli (ve arzu) bir uygulamanın gereksinimleri birçok durumda. Bir zaman-işlem, Android tarafından yönetilmesi gereken kendi bellek alanı yok. Bu genel uygulamaya daha fazla ek yükü tanıtmak rağmen Burada, hizmet kendi işlemini çalıştırmak için yararlı olabilir, bazı senaryo vardır:

1. **İşlevselliği paylaşımı** &ndash; bazı uygulama geliştiricileri birden çok uygulama ve uygulamalar arasında paylaşılan işlevselliği olabilir. Bu işlevselliği de kendi işleminde çalıştığı Uygulama Bakımı kolaylaştırmaya Android hizmetindeki paketleme. Kendi başına APK hizmet paketini ve uygulamanın geri kalanından ayrı olarak dağıtmak mümkündür.
2. **Kullanıcı Deneyimi Geliştirme** &ndash; bir işlem dışı hizmeti uygulamasının kullanıcı deneyimini artırabilir iki olası yolu vardır. İlk yol bellek yönetimi ile ilgilidir. Çöp toplama (GC) döngüsü oluşur, GC işlemi tamamlanana kadar Android işlemdeki tüm etkinlik duraklatılır. Kullanıcı bu Duraklat "titremesine" veya "jank" olarak bakışını. Hizmet çalışırken, kendi işlem, bu duraklatıldı, hizmet işlemi uygulama işlemi. Uygulama işlemi (ve bu nedenle kullanıcı arabirimi) duraklatıldığından bu Duraklat kullanıcıya daha az belirgin olacaktır.

    İkincisi, bir işlemin bellek gereksinimlerini çok büyük olursa, Android cihaz kaynakları açmak için bu işlemi sonlandırılamadı. Android zorla kaynaklarla kaldırsa zaman hizmet büyük bellek parmak izine sahip ve kullanıcı Arabirimi olarak aynı işlemde çalışan, sonra kullanıcı Arabirimi, uygulamayı başlatmak için kullanıcı zorlama kapatılır. Ancak, kendi işleminde çalışan bir hizmete Android tarafından kapatılırsa UI işlemi etkilenmemesini kalır. UI bağlayın (yeniden hizmeti, kullanıcı ve devam et normal işlevine için saydam ve).

3. **Uygulama performansı iyileştirme** &ndash; UI işlemi sona erdi veya hizmet işlemi bağımsız kapatın. Bir işlem dışı hizmetine uzun başlangıç görevleri taşıyarak (hizmet işlemi UI başlatılan zamanları arasında Canlı tutulur varsayılarak) UI başlangıç zamanını belki geliştirilmiş olduğunu mümkündür.

Birçok yolla bağlama başka bir işlemde çalışan bir hizmete aynıdır [yerel bir hizmet bağlama](~/android/app-fundamentals/services/creating-a-service/bound-services.md). İstemci çağıracağı `BindService` bağlayın (ve gerekirse, başlatmak için) hizmet. Bir `Android.OS.IServiceConnection` nesnesi, istemci ile hizmet arasında bağlantı yönetmek için oluşturulur. İstemci başarıyla hizmetine bağlanan sonra Android aracılığıyla bir nesne döndürür `IServiceConnection` hizmet yöntemleri çağırmak için kullanılabilir. İstemci, daha sonra bu nesneyi kullanarak hizmeti ile etkileşime girer. Gözden geçirmek için bir hizmete bağlamak için adımlar şunlardır:

* **Bir hedefi oluşturma** &ndash; açık amacına hizmet bağlama için kullanılması gerekir.
* **Uygulama ve Instantiate bir `IServiceConnection` nesne** &ndash; `IServiceConnection` nesne istemci ile hizmet arasında bir aracı olarak davranır.  İstemci ve sunucu arasındaki bağlantının izlemekten sorumludur.
* **Çağırma `BindService` yöntemi** &ndash; çağırma `BindService` amacını ve Al iletişim kurma ve hizmet başlatma ve üstlenir Android için önceki adımlarda oluşturduğunuz hizmet bağlantı gönderme istemci ile hizmet arasında.

İşlem sınırları arası için gereken ek karmaşıklık tanıtmak: tek yönlü (istemciden sunucuya) iletişimidir ve istemci ile hizmet sınıfı yöntemleri doğrudan çağrılamaz. Bir hizmeti istemci olarak aynı işlemde çalışırken Android sağlar geri çağırma bir `IBinder` çift yönlü iletişimi için izin verebilir nesnesidir. Bu durum kendi işleminde çalışan hizmetiyle değildir. Bir istemci uzak hizmetiyle iletişim kurar Yardımı `Android.OS.Messenger` sınıfı.

Uzak hizmetiyle bağlamak için bir istemci istediğinde, Android çağıracağı `Service.OnBind` iç döndürülecek yaşam döngüsü yöntemi `IBinder` yalıtılan nesne `Messenger`. `Messenger` Özel bir ince sarmalayıcı olan `IBinder` Android SDK'sı tarafından sağlanan uygulama. `Messenger` İki farklı işlemler arasındaki iletişimi ilgilenir. Geliştirici bir ileti seri hale getirme, ileti işlem sınırından dizimi ve istemcide seri durumdan ayrıntılarla unconcerned. Bu iş tarafından işlenen `Messenger` nesnesi. Bu diyagramda, bir istemci bir işlem dışı hizmetine bağlama başlattığında, ilgili olan istemci tarafı Android bileşenleri gösterilmiştir:

![Bir istemci bir hizmetine bağlama bileşenleri ve adımları gösterildiği diyagram](out-of-process-services-images/ipc-01.png "bir hizmetine bağlama bir istemci için adımları ve bileşenleri gösteren diyagramı.")

`Service` Uzak işlem sınıfında yerel bir işlem ilişkili hizmetinde Git aracılığıyla aynı yaşam döngüsü geri çağırmalar aracılığıyla gider ve söz konusu API'leri çoğunu aynıdır. `Service.OnCreate` başlatmak için kullanılan bir `Handler` ve, içine ekleme `Messenger` nesnesi. Benzer şekilde, `OnBind` geçersiz kılınan, ancak döndürme yerine bir `IBinder` nesne, hizmet döndürecektir `Messenger`.  Bu diyagramda, bir istemci bağlama ne olur hizmetinde gösterilir:

![Hizmet bileşenleri ve adımları gösterildiği diyagram zaman geçtiği uzak istemci tarafından bağlanan zaman](out-of-process-services-images/ipc-02.png "bileşenleri ve adımları hizmet gösterildiği diyagram zaman geçtiği uzak istemci tarafından bağlanan olduğunda.")

Zaman bir `Message` alındığında bir hizmet tarafından tarafından örneğinde işlendiği `Android.OS.Handler`. Hizmet kendi gerçekleştireceksiniz `Handler` geçersiz kılmanız gerekir bir alt `HandleMessage` yöntemi. Bu yöntem tarafından çağrılan `Messenger` alıp `Message` bir parametre olarak. `Handler` Araştırmasını `Message` meta veri ve hizmet yöntemleri çağırmak için bu bilgileri kullanın.

Tek yönlü iletişimin istemci oluşturduğunda, bir `Message` nesne ve kullanarak hizmet dağıtır `Messenger.Send` yöntemi. `Messenger.Send` seri hale `Message` ve ileti işlem sınırından yönlendirecek Android ve hizmetin devre dışı veri serileştirilmiş el.  `Messenger` Tarafından barındırılan hizmeti oluşturacak bir `Message` gelen veri nesnesi. Bu `Message` bir sıraya ileti gönderilen tek bir seferde nerede yerleştirilir `Handler`. `Handler` Bulunan meta verileri araştırmasını `Message` ve üzerinde uygun yöntemleri çağırma `Service`. Aşağıdaki diyagram, bu eylem çeşitli kavramlar gösterir:

![İletileri işlemler arasında nasıl geçtiğini gösteren diyagram](out-of-process-services-images/ipc-03.png "iletileri işlemler arasında nasıl geçtiğini gösteren diyagram.")

Bu kılavuz, bir işlem dışı hizmetini uygulama ayrıntılarını ele alınacaktır. Kendi işleminde çalışmak üzere tasarlanmıştır bir hizmeti uygulama ve bir istemci, hizmeti kullanmaya nasıl iletişim kurabilir ele alınacaktır `Messenger` framework. İki yönlü iletişim de kısaca ele alınacaktır: hizmet ve istemciye ileti gönderme hizmeti ileti gönderme istemci. Hizmetleri farklı uygulamalar arasında paylaşılabilir olduğundan, bu kılavuz ayrıca Android izinleri kullanarak hizmet istemci erişimi sınırlayan bir teknik olarak ele alınacaktır.

> [!IMPORTANT]
> [Yalıtılmış işlemlerde ve özel uygulama sınıfı hizmetleriyle Bugzilla 51940 - başarısız aşırı düzgün şekilde çözmek](https://bugzilla.xamarin.com/show_bug.cgi?id=51940) bir Xamarin.Android hizmeti düzgün şekilde başlatılmaz yukarı raporları zaman `IsolatedProcess` ayarlanır `true`. Bu kılavuz, bir başvuru için sağlanır. Bir Xamarin.Android uygulaması hala Java'da yazılmış bir işlem dışı hizmeti ile iletişim kurabilmesi gerekir.

## <a name="requirements"></a>Gereksinimler

Bu kılavuz hizmetleri oluşturma ile benzer olduğu varsayılır.

Örtük hedefleri uygulamalarla hedefleyen eski kullanmak mümkün olmakla Android API, bu kılavuz yalnızca açık hedefleri kullanılmasına odaklanır. Android 5.0 (API düzeyi 21) hedefleyen uygulamalar veya açık amacına sahip bir hizmet; bağlamak için daha yüksek bir kullanmanız gerekir Bu kılavuzda gösterilen tekniğidir...

## <a name="create-a-service-that-runs-in-a-separate-process"></a>Ayrı bir işlemde çalışan bir hizmet oluşturma

Yukarıda açıklandığı gibi kendi işleminde bir hizmeti çalıştıran olgu bazı farklı API'leri dahil olduğu anlamına gelir. Hızlı bir genel bakış ile bağlamak ve uzak hizmeti kullanmak için adımlar şunlardır:  

* **Oluşturma `Service` alt** &ndash; alt `Service` yazın ve ilişkili hizmet yaşam döngüsü yöntemleri uygulayın. Hizmet kendi işleminde çalıştırmaktır Android konusunda bilgi meta veri kümesi gereklidir.
* **Uygulama bir `Handler`**  &ndash; `Handler` istemci isteklerini çözümleme, istemciden geçirilen herhangi bir parametre ayıklanması ve hizmet üzerinde uygun yöntemlerini çağırma sorumludur.
* **Örneği bir `Messenger`**  &ndash; her yukarıda açıklandığı gibi `Service` örneği korumalıdır `Messenger` istemci isteklerini yönlendirecek sınıfı `Handler` önceki adımda oluşturulmuş.

Kendi işleminde çalışmak üzere tasarlanmıştır bir hizmet, temelde, hala bir ilişkili hizmetidir. Hizmet sınıfı, temel uzatır `Service` sınıfı ve ile donatılmış `ServiceAttribute` Android Android bildiriminde paketini gerekiyor meta verileri içeren. Aşağıdaki özelliklere sahip, başlamaya `ServiceAttribute` bir işlem dışı hizmeti için önemli olan:

1. `Exported` &ndash; Bu özelliği ayarlamak `true` diğer uygulamaların hizmetiyle etkileşim kurmasına izin vermek için. Bu özelliğin varsayılan değeri `false`.
2. `Process` &ndash; Bu özelliği ayarlamanız gerekir. Hizmet çalışacak işlemin adını belirtmek için kullanılır.
3. `IsolatedProcess` &ndash; Bu özellik iteract sistemin geri kalanı ile en az izni olan yalıtılmış bir korumalı alanı içinde hizmetini çalıştırmak için Android belirten ek güvenlik sağlar. Bkz: [Bugzilla 51940 - Hizmetleri yalıtılmış işlemlerde ve özel uygulama sınıfı ile başarısız aşırı düzgün şekilde çözmek](https://bugzilla.xamarin.com/show_bug.cgi?id=51940).
4. `Permission` &ndash; İstemcilerinin istemeniz gerekir (ve verilmesi) izin belirterek hizmetine istemci erişimi denetlemek mümkündür.

Hizmet kendi işlemini çalıştırmak için `Process` özellikte `ServiceAttribute` hizmetin adını ayarlamanız gerekir. Dış uygulamalarla etkileşim kurmak için `Exported` özelliği ayarlanmalıdır `true`. Varsa `Exported` olan `false`, (yani aynı uygulama) aynı APK yalnızca istemciler ve aynı işlemde çalışan hizmetiyle etkileşim kuramaz sonra.

Ne tür bir işlemi hizmetin çalışacağı değerine bağlıdır `Process` özelliği. Android işlemleri üç farklı türde tanımlar:

-   **Özel işlem** &ndash; bir özel yalnızca başlatıldığından uygulama için kullanılabilir olan bir işlemdir. Bir işlem özel olarak tanımlayacak adı ile başlamalıdır bir **:** (noktalı virgül). Önceki kod parçacığını ve ekran gösterilen hizmet özel bir işlemdir. Aşağıdaki kod parçacığını örneğidir `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **Genel işlem** &ndash; genel bir işlem içinde çalıştırma bir hizmet cihazda çalışan tüm uygulamaların erişilebilir. Genel işlem, bir küçük harf karakter ile başlayan bir tam sınıfı adı olmalıdır.
    (Hizmeti güvenli hale getirmek için adımları gerçekleştirilecek sürece, diğer uygulamalar bağlamak ve onlarla etkileşime. Hizmet yetkisiz kullanıma karşı güvenli hale getirme bu kılavuzda incelenecektir.)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **İşlem yalıtılmış** &ndash; yalıtılmış bir işlem sistemi ve kendi özel izinler ile geri kalan yalıtılmış kendi korumalı alanı içinde çalışan bir işlemdir. Hizmet yalıtılmış bir işlem içinde çalıştırmak için `IsolatedProcess` özelliği `ServiceAttribute` ayarlanır `true` Bu kod parçacığında gösterildiği gibi:
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> Bkz: [Bugzilla 51940 - Hizmetleri yalıtılmış işlemlerde ve özel uygulama sınıfı ile başarısız aşırı düzgün çözümlemek](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

Yalıtılmış bir hizmeti, bir uygulama ve cihaz güvenilmeyen kod karşı güvenliğini sağlamak için basit bir yoludur. Örneğin, bir uygulama indirin ve bir Web sitesinden bir betiğini yürütün. Bu durumda, bu yalıtılmış bir işlem içinde gerçekleştirilmesi ek bir Android cihaz tehlikeye güvenilmeyen kod karşı güvenlik katmanı sağlar.

> [!IMPORTANT]
> Hizmet dışarı aktarıldıktan sonra hizmetin adını değiştirmemeniz gerekir. Hizmet adının değiştirilmesi hizmetini kullanan diğer uygulamalar çalışmamasına neden olabilir.

Etkisini görmek için `Process` özelliğine sahiptir, kendi özel işlemde çalışan bir hizmete aşağıdaki ekran gösterilir:

![Özel bir işlem olarak çalışan bir hizmete gösteren ekran görüntüsü](out-of-process-services-images/ipc-04.png "özel bir işlem olarak çalışan bir hizmete gösteren ekran görüntüsü.")

Bu sonraki ekran gösterir `Process="com.xamarin.xample.messengerservice.timestampservice_process"` ve genel bir işlemde çalışan hizmeti:

![Genel bir işlemde çalışan bir hizmetin ekran](out-of-process-services-images/ipc-05.png "genel bir işlemde çalışan bir hizmetin ekran görüntüsü.")

Bir kez `ServiceAttribute` ayarlandıktan uygulamak için hizmet gereksinimlerine bir `Handler`.

### <a name="implementing-a-handler"></a>Bir işleyici uygulama

İstemci isteklerini işlemek için hizmet uygulamalıdır bir `Handler` ve geçersiz kılma `HandleMessage` methodThis olduğu yöntemi alır bir `Message` örneği hangi hangi istemciden yöntem çağrısı yalıtır ve bu çağrıyı bazı eylemleri çevirir veya hizmeti gerçekleştirir görev. `Message` Nesneyi kullanıma sunan adlı bir özellik `What` anlamını istemci ile hizmet arasında paylaşılan ve hizmet için istemci gerçekleştirmektir bazı görev ilişkili bir tamsayı değeri değil.

Aşağıdaki kod parçacığını örnek uygulamadaki bir örneği gösterilmektedir `HandleMessage`. Bu örnekte, bir istemcinin isteyebileceği hizmetinin iki eylem vardır:

* İlk eylem bir _Hello, World_ iletisi, istemci gönderdi basit bir ileti hizmete.
* İkinci eylemi hizmetinde bir yöntemini çağırmak ve bir dize almak, bu durumda dize ne zaman çalışır durumda hizmeti başlatıldı ve ne kadar süreyle döndüren bir ileti:

```csharp
public class TimestampRequestHandler : Android.OS.Handler
{
    // other code omitted for clarity

    public override void HandleMessage(Message msg)
    {
        int messageType = msg.What;
        Log.Debug(TAG, $"Message type: {messageType}.");

        switch (messageType)
        {
            case Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE:
                // The client as sent a simple Hello, say in the Android Log.
                break;

            case Constants.GET_UTC_TIMESTAMP:
                // Call methods on the service to retrive a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

Ayrıca hizmet için paket parametreleri mümkün `Message`. Bu kılavuzda ele. Dikkate alınması gereken sonraki konu oluşturma `Messenger` gelen işlenecek nesne `Message`s.

### <a name="instantiating-the-messenger"></a>Messenger örnek oluşturma

Daha önce seri durumdan açıklandığı gibi `Message` nesne ve çağırma `Handler.HandleMessage` , sorumluluk olduğu `Messenger` nesnesi. `Messenger` Sınıfı sağlar bir `IBinder` istemci hizmete iletileri göndermek için kullanacağı nesne.  

Hizmet başlatıldığında örneği `Messenger` ve ekleme `Handler`. Bu başlatılmasını gerçekleştirmek için uygun bir yerdir açıktır `OnCreate` hizmetinin yöntemi. Bu kod parçacığını kendi başlatır bir hizmetin bir örnektir `Handler` ve `Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

Bu noktada, son için adımdır `Service` geçersiz kılmak için `OnBind`.

### <a name="implementing-serviceonbind"></a>Service.OnBind uygulama

Tüm bağımlı hizmetleri zaman veya değil, kendi işleminde çalışır olup olmadığını uygulamalıdır `OnBind` yöntemi. Bu yöntemin dönüş değeri istemci hizmetiyle etkileşim kurmak için kullanabileceğiniz bazı nesnesidir. Bu nesne tam olarak nedir hizmetin yerel bir hizmet veya bir uzak hizmet olup olmadığını bağlıdır. Yerel bir hizmet bir özel döndürülecek sırada `IBinder` uygulama, bir uzak hizmet döndürecektir `IBinder` kapsüllenmiş ancak `Messenger` önceki bölümde oluşturulan:

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

Bu üç adımı elde sonra uzak hizmet tam olarak düşünülebilir.

## <a name="consuming-the-service"></a>Hizmet kullanma

Tüm istemcilerin bağlamak ve uzak hizmeti kullanmak bazı kod uygulamalıdır. Kavramsal olarak, istemcinin görüş açısından, yerel hizmet veya bir uzak hizmet bağlaması arasında çok az farklılıklar vardır. İstemci çağırır `BindService` Hizmeti'ni tanımlamak için açık amacına geçirerek yöntemini ve bir `IServiceConnection` istemci ile hizmet arasında bağlantı yönetmeye yardımcı olur.

Bu kod parçacığını nasıl oluşturulacağını örneğidir bir **açık hedefi** uzak hizmet bağlaması için. Amaç hizmeti ve hizmetin adını içeren paketi tanımlamanız gerekir. Bu bilgileri ayarlamak için tek yolu bir `Android.Content.ComponentName` nesne ve hedefi sağlamak için. Bu kod parçacığını bir örnek verilmiştir:  

```csharp
// This is the package name of the APK, set in the Android manifest
const string REMOTE_SERVICE_COMPONENT_NAME = "com.xamarin.TimestampService";
// This is the name of the service, according the value of ServiceAttribute.Name
const string REMOTE_SERVICE_PACKAGE_NAME   = "com.xamarin.xample.messengerservice";

// Provide the package name and the name of the service with a ComponentName object.
ComponentName cn = new ComponentName(REMOTE_SERVICE_PACKAGE_NAME, REMOTE_SERVICE_COMPONENT_NAME);
Intent serviceToStart = new Intent();
serviceToStart.SetComponent(cn);
```

Hizmet bağlandığında `IServiceConnection.OnServiceConnected` yöntemi çağrılır ve sağlayan bir `IBinder` istemciye. Ancak, istemci doğrudan kullanmaz `IBinder`. Bunun yerine, örneği bir `Messenger` , nesnesinden `IBinder`. Bu `Messenger` , istemci uzak hizmetiyle etkileşim kurmak için kullanır.

Çok basit bir örneği verilmiştir `IServiceConnection` uygulamasında, bir istemci bağlanma ve bir hizmetinden kesme nasıl işleneceğini gösterir. Dikkat `OnServiceConnected` yöntemi alır ve `IBinder`, ve istemciyi oluşturan bir `Messenger` gruptan `IBinder`:

```csharp
public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection
{
    static readonly string TAG = typeof(TimestampServiceConnection).FullName;

    MainActivity mainActivity;
    Messenger messenger;

    public TimestampServiceConnection(MainActivity activity)
    {
        IsConnected = false;
        mainActivity = activity;
    }

    public bool IsConnected { get; private set; }
    public Messenger Messenger { get; private set; }

    public void OnServiceConnected(ComponentName name, IBinder service)
    {
        Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

        IsConnected = service != null
        Messenger = new Messenger(service);

        if (IsConnected)
        {
            // things to do when the connection is successful. perhaps notify the client? enable UI features?
        }
        else
        {
            // things to do when the connection isn't successful.
        }
    }

    public void OnServiceDisconnected(ComponentName name)
    {
        Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
        IsConnected = false;
        Messenger = null;

        // Things to do when the service disconnects. perhaps notify the client? disable UI features?

    }
}
```

Hizmet bağlantı ve hedefi oluşturduktan sonra istemcinin çağırmak için olası `BindService` ve bağlama işlemini başlatın:

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

İstemci başarıyla hizmetine bağlı sonra ve `Messenger` olan kullanılabilir, istemcinin göndermek için mümkündür `Messages` hizmet.

## <a name="sending-messages-to-the-service"></a>Hizmet iletileri gönderme

İstemcinin bağlı olduğu ve sonra bir `Messenger` nesne göndermeyi tarafından hizmetiyle iletişim kurmak mümkündür `Message` aracılığıyla nesneleri `Messenger`. Bu iletişim tek yönlüdür, istemci iletisi gönderir ancak hiçbir dönüş iletiye hizmetinden istemci yoktur. Bu bağlamda `Message` bir yangın ve unut mekanizmadır.

Oluşturmak için tercih edilen yol bir `Message` nesnesi kullanmaktır [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods) Üreteç yöntemi. Bu yöntem çeker bir `Message` havuzundan Android tarafından korunan bir nesne. `Message.Obtain` Bazı aşırı yüklü izin yöntemleri de `Message` hizmeti için gereken parametreler ve değerler ile başlatılması için nesne.  Bir kez `Message` olan örneği, bu hizmete çağırarak gönderilen `Messenger.Send`. Bu kod parçacığında oluşturma ve gönderme bir örnektir bir `Message` hizmet işlemi için:

```csharp
Message msg = Message.Obtain(null, Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE);
try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a error trying to send the message.");
}
```

Birkaç farklı biçimlerde vardır `Message.Obtain` yöntemi. Önceki örnekte kullanılmaktadır [ `Message.Obtain(Handler h, Int32 what)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/). Bu işlem dışı hizmetine zaman uyumsuz bir istek olduğundan; Hizmet yanıt olacak şekilde `Handler` ayarlanır `null`. İkinci parametre `Int32 what`, içinde depolanan `.What` özelliği `Message` nesnesi. `.What` Özelliği hizmeti işlem kodu tarafından hizmet yöntemleri çağırma için kullanılır.

`Message` Sınıfı recipent için kullanışlı olabilecek iki ek özellikler de sunar: `Arg1` ve `Arg2`. Bu iki özellik bazı özel istemci ile hizmet arasında anlamları değerleri üzerinde anlaşmaya varılan olabilir tamsayı değerleri kullanılır. Örneğin, `Arg1` bir müşteri kimliği tutabilir ve `Arg2` satınalma siparişi numarası o müşteri için tutabilir. [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/) İki özelliklerini ayarlamak için kullanılan zaman `Message` oluşturulur. Bu iki değer doldurmak için başka bir şekilde ayarlamaktır `.Arg` ve `.Arg2` özellikleri doğrudan üzerinde `Message` oluşturulduktan sonra nesnesi.

### <a name="passing-additional-values-to-the-service"></a>Hizmet için ek değerler geçirme

Kullanarak daha karmaşık veri hizmetine iletmek olası bir `Bundle`. Bu durumda, ek değerler olarak yerleştirilebilecek bir `Bundle` ve ile birlikte gönderilen `Message` ayarlayarak [ `.Data` özelliği](https://developer.xamarin.com/api/property/Android.OS.Message.Data/) göndermeden önce özelliği.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> Genel olarak, bir `Message` bir yükü 1 MB'den büyük olmamalıdır. Boyut sınırını Android sürümü according değişebilir ve herhangi bir özel değişiklik satıcı kendi uygulamasına, Android açık kaynak projesi (aygıtla toplanmış AOSP) yapmış olabilirsiniz.

## <a name="returning-values-from-the-service"></a>Hizmetinden dönüş değerleri

Bu noktada ele alınan ileti mimarisi tek yönlü, istemci hizmeti için bir ileti gönderir. Ardından hizmetinin bir istemciye bir değer döndürmek gerekliyse, bu noktada ele alınan her şeyi ters çevrilir. Hizmet oluşturmalısınız bir `Message`, paketlenmiş tüm dönüş değerleri ve gönderme `Message` aracılığıyla bir `Messenger` istemciye. Ancak, hizmeti, kendi oluşturmaz `Messenger`; bunun yerine, istemci başlatmasını ve paket dayanır bir `Messenger` ilk isteğin bir parçası olarak. Hizmeti `Send` bu istemci tarafından sağlanan kullanarak ileti `Messenger`.  

Çift yönlü iletişimi için olayların sırası şudur:

1. İstemci hizmete bağlar. Hizmet ve istemci bağlandığınızda `IServiceConnection` tarafından tutulan istemci başvuru olacaktır bir `Messenger` iletmek için kullanılan nesne `Message`hizmetine s. Karışıklığı önlemek için bu için olarak anılacaktır _hizmet Messenger_.
2. İstemci başlatır bir `Handler` (olarak adlandırılan _istemci işleyicisi_) ve kendi başlatmak için kullanan `Messenger` ( _istemci Messenger_). Hizmet Messenger ve istemci Messenger iki farklı yönde arasındaki trafiği yöneten iki farklı nesneleri olduğuna dikkat edin. İstemci Messenger istemciye hizmetinden gelen iletileri işleyecek sırada hizmet Messenger hizmeti, istemciden gelen iletileri işler.
3. İstemciyi oluşturan bir `Message` nesne ve ayarlar `ReplyTo` istemci Messenger özelliğiyle. İleti hizmeti Messenger kullanarak hizmete gönderilir.
4. Hizmet istemciden gelen iletiyi alır ve istenen çalışma gerçekleştirir.
5. Hizmetinin yanıtı istemciye göndermek zamanı geldiğinde kullanacağı `Message.Obtain` yeni `Message` nesnesi.
6. Bu iletiyi istemciye göndermek için hizmet gelen istemci Messenger ayıklar `.ReplyTo` özelliği istemcinin ileti ve için kullanan `.Send` `Message` istemciye.
7. Yanıt istemci tarafından alındığında, kendi sahip `Handler` , işleyecek `Message` inceleniyor tarafından `.What` özelliği (ve gerekirse, içinde herhangi bir parametre ayıklama `Message`).

Bu örnek kod, istemcinin nasıl örneği oluşturulmaz göstermektedir `Message` ve paket bir `Messenger` hizmetinin yanıt için kullanması gereken:

```csharp
Handler clientHandler = new ActivityHandler();
Messenger clientMessenger = new Messenger(activityHandler);

Message msg = Message.Obtain(null, Constants.GET_UTC_TIMESTAMP);
msg.ReplyTo = clientMessenger;

try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a problem sending the message.");
}
```

Hizmetin bazı değişiklikler için kendi yapmalısınız `Handler` ayıklamak için `Messenger` ve yanıt istemciye gönderilecek kullanan. Bu kod parçacığını nasıl örneğidir hizmetin `Handler` oluşturacak bir `Message` ve istemciye gönderme:  

```csharp
// This is the message that the service will send to the client.
Message responseMessage = Message.Obtain(null, Constants.RESPONSE_TO_SERVICE);
Bundle dataToReturn = new Bundle();
dataToReturn.PutString(Constants.RESPONSE_MESSAGE_KEY, "This is the result from the service.");
responseMessage.Data = dataToReturn;

// The msg object here is the message that was received by the service. The service will not instantiate a client,
// It will use the client that is encapsulated by the message from the client.
Messenger clientMessenger = msg.ReplyTo;
if (clientMessenger!= null)
{
    try
    {
        clientMessenger.Send(responseMessage);
    }
    catch (Exception ex)
    {
        Log.Error(TAG, ex, "There was a problem sending the message.");
    }
}
```

Yukarıdaki kod örnekleri unutmayın `Messenger` istemci tarafından oluşturulan örnek *değil* hizmeti tarafından alınan aynı nesne. İki farklı bunlar `Messenger` iletişim kanalını temsil eden iki ayrı işlemlerde çalışan nesneler.

## <a name="securing-the-service-with-android-permissions"></a>Hizmet Android izinleri ile güvenli hale getirme

Genel bir işlemde çalışan hizmet, bu Android cihazda çalışan tüm uygulamalar tarafından erişilebilir. Bazı durumlarda bu açıklık ve kullanılabilirlik istenmeyen ve yetkisiz istemcilerden güvenli erişime karşı hizmeti gereklidir. Uzak Hizmet erişimi sınırlamak için bir yolu, Android izinleri kullanmaktır.

İzinleri tarafından belirlenebilir `Permission` özelliği `ServiceAttribute` , süsler `Service` alt sınıfı. Bu istemci hizmete bağlanırken verilmelidir izni adlandıracaktır. İstemci uygun izinlere sahip değil, ardından Android oluşturur, bir `Java.Lang.SecurityException` istemci hizmetine bağlama çalıştığında.

Android sağlar dört farklı izin düzeyleri şunlardır:

* **Normal** &ndash; varsayılan izin düzeyi budur. Android tarafından isteyen istemcilere otomatik olarak verilebilir düşük riskli izinlerini tanımlamak için kullanılır. Kullanıcı açıkça bu izinler vermeniz gerekmez, ancak uygulama ayarlarında izinler görüntülenebilir.
* **İmza** &ndash; tümü aynı sertifika ile imzalanmış uygulamalara Android tarafından otomatik olarak verilecek izni özel kategori budur. Bu izni bileşenleri veya sabit onayları kullanıcıdan rahatsız etme olmadan, uygulamalar arasında veri paylaşmak kolay bir uygulama geliştiricisi için olmak üzere tasarlanmıştır.
* **signatureOrSystem** &ndash; bu çok benzer **imza** yukarıda açıklanan izinleri. Otomatik olarak verilen aynı sertifika tarafından imzalanmış uygulamalara olmaya ek olarak, bu izni de imzalanmış uygulamalara Android sistemi görüntüsüyle uygulamaları imzalamak için kullanılan aynı sertifika yüklü verilecektir. Bu izin, üçüncü taraf uygulamalar ile çalışmak üzere uygulamalarını izin vermek için genellikle Android ROM geliştiriciler tarafından yalnızca kullanılır. Bu genellikle büyük ortak için genel dağıtım yöneliktir uygulamalar tarafından kullanılmaz.
* **tehlikeli** &ndash; tehlikeli izinler bilgilerdir kullanıcı için sorunlara neden olabilir. Bu nedenle, **tehlikeli** izinleri kullanıcı tarafından açıkça da onaylanmalıdır.

Çünkü `signature` ve `normal` izinler otomatik olarak verilen yüklü zaman Android tarafından barındırma hizmeti APK yüklenmesi önemli olduğu **önce** istemci içeren APK. İstemci önce yüklediyseniz, Android, izinleri verin değil. Bu durumda, istemci APK kaldırmak için APK hizmetini yükleyin ve istemci APK yeniden yüklemek gerekli olacaktır.

Android izinleri olan bir hizmeti güvenli hale getirmek için iki genel yolu vardır:

1.  **Uygulama imzası düzeyi güvenlik** &ndash; imza düzeyi güvenlik anlamına gelir izni otomatik olarak verilen hizmet bulunduran APK imzalamak için kullanılan aynı anahtarla imzalanmıştır bu uygulamalara olduğunu. Bu, geliştiricilerin kendi hizmet güvenli henüz kendi uygulamalardan erişilebilir tutmak basit bir yoludur. İmza düzeyinde izinler ayarlayarak bildirildiğinde `Permission` özelliği `ServiceAttribute` için `signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **Özel bir izin oluşturmak** &ndash; hizmeti için özel bir izin oluşturmak Geliştirici hizmetinin mümkündür. Bu, ne zaman bir geliştirici kendi hizmet diğer geliştiricilerden uygulamalarla paylaşmak istiyor için en iyi seçenektir. Özel bir izni uygulamak için biraz daha fazla çaba gerektirir ve aşağıda ele alınacaktır.

Özel oluşturma basitleştirilmiş bir örnek `normal` izni sonraki bölümde açıklanan. Android izinleri hakkında daha fazla bilgi için lütfen Google yönelik belgelere [en iyi yöntemler ve güvenlik](https://developer.android.com/training/articles/security-tips.html). Android izinleri hakkında daha fazla bilgi için bkz: [izinleri bölüm](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) Android izinleri hakkında daha fazla bilgi için uygulama bildirimi için Android belgelerin.

> [!NOTE]
> Genel olarak, [Google zorlaştırır özel izinleri kullanımını](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) olarak kullanıcılar için kafa karıştırıcı kanıtlamak.

### <a name="creating-a-custom-permission"></a>Özel bir izin oluşturma

İstemci izni açıkça istediğinde sırasında özel bir izni kullanmak için hizmet tarafından bildirildi.

APK, hizmetinde bir izin oluşturmak için bir `permission` öğesi eklenir `manifest` öğesinde**AndroidManifest.xml**. Bu izni olmalıdır `name`, `protectionLevel`, ve `label` öznitelikleri kümesi. `name` İzni benzersiz olarak tanımlayan bir dize özniteliği ayarlanmalıdır. Adı görüntülenir **uygulama bilgisi** görünümünü **Android ayarları** (sonraki bölümde gösterildiği gibi).

`protectionLevel` Özniteliği, yukarıda açıklanan dört dize değerlerden birine ayarlanmalıdır.  `label` Ve `description` dize kaynaklara başvurması gerekir ve kullanıcı için bir kolay ad ve açıklama sağlamak için kullanılır.

Bu kod parçacığında özel bildirme bir örnektir `permission` özniteliğini **AndroidManifest.xml** hizmeti içeren APK biri:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerservice">

    <uses-sdk android:minSdkVersion="21" />

    <permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP"
                android:protectionLevel="signature"
                android:label="@string/permission_label"
                android:description="@string/permission_description"
                />

    <application android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">

    </application>
</manifest>
```

Ardından, **AndroidManifest.xml** istemci APK açıkça bu yeni izin istemeniz gerekir. Bu ekleyerek yapılır `users-permission` özniteliğini **AndroidManifest.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerclient">

    <uses-sdk android:minSdkVersion="21" />

    <uses-permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP" />

    <application
            android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">
    </application>
    </manifest>
```

### <a name="view-the-permissions-granted-to-an-app"></a>Bir uygulamaya verilen izinleri görüntüleme

Bir uygulama verilen izinleri görüntülemek için Android ayarları uygulamasını açın ve seçin **uygulamaları**. Bulun ve uygulamayı listeden seçin. Gelen **uygulama bilgisi** ekranında, dokunun **izinleri** uygulamaya verilen tüm izinleri gösteren bir görünüm yukarı Getir:

[![Bir uygulamaya verilen izinler bulmak nasıl gösteren bir Android aygıttan ekran görüntüleri](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png)

## <a name="summary"></a>Özet

Bu kılavuz uzak bir işlemde bir Android hizmeti çalıştırma hakkında gelişmiş bir tartışma oluştu. Yerel ve uzak hizmet arasındaki farklar açıklanan, birlikte bazı nedenler neden bir uzak hizmet kararlılık ve performans Android uygulama için yararlı olabilir. Uzak hizmeti uygulama ve bir istemci hizmeti ile nasıl iletişim kurabildiğini açıklayan sonra Kılavuzu yalnızca yetkili istemcilerin hizmete erişimi sınırlamak için bir yol sağlamak üzere geçti.


## <a name="related-links"></a>İlgili bağlantılar

- [Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [İleti](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [Verilebilir özniteliği](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [Aşırı düzgün çözümlemek yalıtılmış işlemleri ve özel uygulama sınıfı hizmetleriyle başarısız](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [İşlemler ve iş parçacıkları](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android derleme bildirimi - izinleri](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [Güvenlik ipuçları](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
