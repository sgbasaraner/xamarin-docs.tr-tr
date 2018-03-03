---
title: CallKit
description: "Bu makalede yeni CallKit API, Apple iOS 10 ve Xamarin.iOS VoIP uygulamalarda uygulama yayımlanan yer almaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: c78396ce55c776c615f3b3027a97b5a334c0b7f8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="callkit"></a>CallKit

_Bu makalede yeni CallKit API, Apple iOS 10 ve Xamarin.iOS VoIP uygulamalarda uygulama yayımlanan yer almaktadır._


Yeni iOS 10 CallKit API VoIP uygulamaların UI iPhone ile tümleştirme, bilinen bir arayüz sağlar ve son kullanıcı deneyimi bir yol sağlar. Bu API ile kullanıcılar görebilir ve iOS cihaz kilit ekranı VoIP çağrılarından etkileşim ve telefon uygulamanın kullanarak kişileri Yönet **Sık Kullanılanlar** ve **Recents** görünümleri.

## <a name="about-callkit"></a>CallKit hakkında

Apple göre CallKit 3 taraf ses üzerinden IP (VoIP) uygulamalar, iOS 10 deneyimi 1. bir tarafa yükseltmesine yeni bir çerçevedir. CallKit API VoIP uygulamalarının UI iPhone ile tümleştirme, bilinen bir arayüz sağlar ve son kullanıcı deneyimi sağlar. Yalnızca yerleşik telefon uygulama gibi bir kullanıcının görüntüleyebileceği ve iOS cihaz kilit ekranı VoIP çağrılarından etkileşimde ve telefon uygulamanın kullanarak kişileri Yönet **Sık Kullanılanlar** ve **Recents** görünümleri.

Ayrıca, CallKit API uygulama adıyla (arayan kimliği) bir telefon numarası ilişkilendirmek veya bir sayı olmalıdır, sistem (çağrı engelleme) engellenen söyleyin uzantıları oluşturma olanağı sağlar.

### <a name="the-existing-voip-app-experience"></a>Varolan VoIP uygulama deneyimi

Yeni CallKit API ve kendi yeteneklerini ele almadan önce Al geçerli kullanıcı deneyimini 3 göz taraf VoIP uygulamada iOS 9 (ve daha az) kullanılarak MonkeyCall adlı kurgusal bir VoIP uygulama. MonkeyCall varolan iOS API'leri kullanılarak VoIP çağrıları gönderip kullanıcıya izin veren basit bir uygulamadır.

Şu anda kullanıcı MonkeyCall gelen bir çağrıda alıyor ve bunların iPhone kilitli, kilit ekranı üzerinde alınan bildirim başka bir bildirim türünden ayırt ise (ister olanlar iletilerden veya uygulamaları örneğin posta).

Kullanıcı aramaya yanıt istemeniz durumunda, bunlar uygulamasını açın ve aramayı kabul ve konuşma başlatmak için önce telefonun kilidini açmak için geçiş kodu (veya kullanıcı Touch ID) girmek için MonkeyCall bildirim slayt etmesi gerekir.

Telefon kilidi ise eşit sıkıcı deneyimidir. Yeniden gelen MonkeyCall çağrısı, ekranın üst kısmından slayt standart bildirim başlık olarak görüntülenir. Bildirim geçici olduğundan, bu kolayca bildirim merkezi açın ve yanıt sonra çağrısı veya bulmak ve MonkeyCall uygulamasını el ile başlatmak için belirli bildirim bulmak için bunları zorlama kullanıcı tarafından eksik.

### <a name="the-callkit-voip-app-experience"></a>CallKit VoIP uygulama deneyimi

Yeni CallKit API'leri MonkeyCall uygulamada uygulayarak, gelen bir VoIP arama kullanıcı deneyiminizi büyük ölçüde iOS 10 geliştirilebilir. Telefon üstten kilitlendiğinde VoIP çağrı alma kullanıcı örneği alın. Çağrı tam ekran, yerel kullanıcı Arabirimi ve standart manyetik yanıt işlevselliği ile yerleşik telefon uygulamasından alındıysa gibi CallKit uygulayarak çağrı iPhone's kilit ekranında görüntülenir.

Yeniden MonkeyCall VoIP çağrı alındığında iPhone kilidinin açık olduğundan, aynı tam ekran, yerel kullanıcı Arabirimi ve standart manyetik yanıt ve dokunun Reddet işlevselliğini yerleşik uygulama sunulan telefon ve MonkeyCall sahipse özel zil sesi çalma seçeneği .

CallKit MonkeyCall, VoIP izin vererek, ek işlevsellik çağırır Recents yerleşik görünmesi çağrısı, diğer türleri ile etkileşim kurmasına ve sık kullanılan listeler sağlar, yerleşik Rahatsız Etmeyin ve blok özellikleri kullanmak için Siri MonkeyCall çağrıları başlatın ve Kullanıcıların kişiler uygulama kişilere MonkeyCall çağrıları atamak olanak sağlar.

Aşağıdaki bölümlerde CallKit mimarisi ele alınacaktır, gelen ve giden akış ve CallKit API ayrıntılı olarak çağırın.


## <a name="the-callkit-architecture"></a>CallKit mimarisi

Örneğin, CarPlay üzerinde yapılan çağrıları sistem kullanıcı Arabirimi aracılığıyla CallKit bilinen şekilde iOS 10'da, CallKit tüm sistem hizmetleri Apple başlamıştır. İçinde MonkeyCall CallKit uyarlar beri aşağıda verilen örnek, sisteme aynı şekilde bu yerleşik sistem hizmetleri olarak bilinir ve tüm aynı özellikleri alır:

[ ![](callkit-images/callkit01.png "CallKit hizmet yığını")](callkit-images/callkit01.png)

Yukarıdaki diyagramda MonkeyCall uygulamadan daha yakın bir göz atın. Uygulama tüm, kendi ağ ile iletişim kurmak için kendi kod ve kendi kullanıcı arabirimlerini içerir. Sistemiyle iletişim kurmak için CallKit bağlar:

[ ![](callkit-images/callkit02.png "MonkeyCall uygulama mimarisi")](callkit-images/callkit02.png)

Uygulamanın kullandığı CallKit içinde iki ana arabirimi vardır:

- `CXProvider` -Bu MonkeyCall oluşabilecek herhangi bir bant dışı bildirim sistemi bildirmek yazmasına izin verir.
- `CXCallController` -MonkeyCall yerel kullanıcı eylemlerini sistem bildirmek yazmasına izin verir.

### <a name="the-cxprovider"></a>CXProvider

Yukarıda belirtildiği gibi `CXProvider` oluşabilecek herhangi bir bant dışı bildirim sistemi bildirmek bir uygulama sağlar. Yerel kullanıcı eylemlerini nedeniyle meydana gelmediğinden, ancak gelen çağrıları gibi dış olaylar nedeniyle ortaya bildirim bunlar.

Bir uygulama kullanması gereken `CXProvider` aşağıdaki için:

- Gelen bir arama sisteme bildirin.
- Bir, rapor çağrısı giden sisteme bağlı.
- Sistem çağrısı bitiş uzak kullanıcı bildirin.

Sisteme iletişim kurmak uygulamanın istediği zaman kullandığı `CXCallUpdate` sınıfı ve sistem uygulamayla iletişim kurmak gerektiğinde kullanır `CXAction` sınıfı:

[ ![](callkit-images/callkit03.png "Bir CXProvider üzerinden ile iletişim")](callkit-images/callkit03.png)

### <a name="the-cxcallcontroller"></a>CXCallController

`CXCallController` Bir VoIP çağrı başlangıç kullanıcı gibi yerel kullanıcı eylemleri sistem bildirmek bir uygulama sağlar. Uygulama tarafından bir `CXCallController` çağrıları diğer türleri sistemde Interplay uygulamayı alır. Örneğin, zaten bir etkin telefon araması sürüyor, ise `CXCallController` bu aramayı beklemeye almak ve Başlat veya bir VoIP aramaya yanıt VoIP yazmasına izin verebilirsiniz.

Bir uygulama kullanması gereken `CXCallController` aşağıdaki için:

- Kullanıcıya sisteme giden bir çağrı başlatıldığında rapor.
- Kullanıcı sisteme gelen bir arama yanıtladığında rapor.
- Kullanıcı sistem çağrısı sona erdiğinde rapor.

Yerel kullanıcı eylemlerini sisteme iletişim kurmak uygulamanın istediği zaman kullandığı `CXTransaction` sınıfı:

[ ![](callkit-images/callkit04.png "Bir CXCallController kullanarak sistem raporlama")](callkit-images/callkit04.png)

## <a name="implementing-callkit"></a>CallKit uygulama

Aşağıdaki bölümlerde, bir Xamarin.iOS VoIP uygulamasında CallKit uygulamak nasıl yapacağınızı gösterir. Örnek amacıyla, bu belgede kurgusal MonkeyCall VoIP uygulama kodundan kullanacak. Burada sunulan kodu çeşitli destekleyici sınıfları temsil eder, belirli kısımlarını olacak CallKit aşağıdaki bölümlerde ayrıntılı olarak ele.

### <a name="the-activecall-class"></a>ActiveCall sınıfı

`ActiveCall` Sınıfı, tüm gibi şu anda etkin olan bir VoIP çağrı hakkında bilgileri saklamak için MonkeyCall uygulama tarafından kullanılır:

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall` Arama ve arama durumu değiştiğinde yükseltilebilir iki olayları durumunu tanımlayan çeşitli özellikler içerir. Bu yalnızca bir örnek olduğundan, yanıtlama başlayıp bir çağrı benzetimi için kullanılan üç yöntem vardır.

### <a name="the-startcallrequest-class"></a>StartCallRequest sınıfı

`StartCallRequest` Statik sınıf giden ile çalışma çağırdığında kullanılacak birkaç yardımcı yöntemler sağlar:

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

`CallHandleFromURL` Ve `CallHandleFromActivity` sınıfları AppDelegate içinde giden bir çağrı çağrılan kişinin iletişim tanıtıcısı almak için kullanılır. Daha fazla bilgi için lütfen bkz [işleme giden çağrıları](#Handling-Outgoing-Calls) bölümüne bakın.

### <a name="the-activecallmanager-class"></a>ActiveCallManager sınıfı

`ActiveCallManager` Sınıfı MonkeyCall uygulamadaki tüm açık çağrılarını işler.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID == uuid) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

Yeniden, bu yalnızca bir benzetimi olduğundan `ActiveCallManager` yalnızca bir koleksiyonunu tutan `ActiveCall` nesneleri ve belirli bir çağrı tarafından bulma için bir yordam sahip kendi `UUID` özelliği. Ayrıca, Başlat, end ve giden bir çağrı durdurulma durumunu değiştirmek için yöntemler içerir. Daha fazla bilgi için lütfen bkz [işleme giden çağrıları](#Handling-Outgoing-Calls) bölümüne bakın.

### <a name="the-providerdelegate-class"></a>ProviderDelegate sınıfı

Yukarıda açıklandığı gibi bir `CXProvider` bant dışı bildirimleri için uygulama ve sistem arasında iki yönlü iletişim sağlar. Geliştirici özel sağlaması gerekir `CXProviderDelegate` ve ekleyebilir `CXProvider` bant dışı CallKit olayları işlemek uygulama. MonkeyCall kullanan aşağıdaki `CXProviderDelegate`:

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Mask
            var maskImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconMaskImageData = maskImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

Bu temsilci örneği oluşturulduğunda, geçirilen `ActiveCallManager` , herhangi bir çağrı etkinliği işlemek için kullanılacak. Ardından, tanıtıcı türleri tanımlar (`CXHandleType`), `CXProvider` yanıt verir:

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

Ve bir çağrı devam ederken, uygulamanın simgesine uygulanacak maskesi alır:

```csharp
// Get Image Mask
var maskImage = UIImage.FromFile ("telephone_receiver.png");
```

Bu değerleri içine paketlenmiş bir `CXProviderConfiguration` yapılandırmak için kullanılacak `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconMaskImageData = maskImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

Temsilci sonra yeni bir oluşturur `CXProvider` bu yapılandırmaları olan ve kendisine ekler:

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

CallKit kullanırken, uygulama artık oluşturmak ve kendi ses oturumları işlemek, bunun yerine yapılandırmak ve sistem oluşturmak ve işlemek için bir ses oturumu kullanmak gerekir. 

Bu gerçek bir uygulama olsaydı `DidActivateAudioSession` yöntemi çağrısı bir önceden yapılandırılmış başlatmak için kullanılabilir `AVAudioSession` sistem sağlanan:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

Ayrıca kullanırsınız `DidDeactivateAudioSession` sona erdirmek ve sistem bağlantısını yayın için yöntem sağlanan ses oturum:

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

Kod geri kalanını uygulayın bölümlerde ayrıntılı olarak ele alınacaktır.

### <a name="the-appdelegate-class"></a>AppDelegate sınıfı

MonkeyCall örneklerini tutmak için AppDelegate kullanan `ActiveCallManager` ve `CXProviderDelegate` uygulama boyunca kullanılır:

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url); 
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

`OpenUrl` Ve `ContinueUserActivity` geçersiz kılma yöntemleri uygulama giden bir çağrı işlenirken kullanılır. Daha fazla bilgi için lütfen bkz [işleme giden çağrıları](#Handling-Outgoing-Calls) bölümüne bakın.

## <a name="handling-incoming-calls"></a>Gelen çağrıları işleme

Birkaç durumları ve gelen bir VoIP arama sırasında normal gelen çağrı iş akışı gibi gidebilirsiniz işlemler vardır:

- Kullanıcı (ve sistem) gelen bir arama var olduğunu bildiren.
- Kullanıcı aramaya yanıt istediğinde bildirim alma ve diğer kullanıcı çağrısıyla başlatılıyor.
- Kullanıcının geçerli aramayı sonlandırmak istediğinde sistem ve iletişim ağ bildirin.

Aşağıdaki bölümlerde bir uygulama MonkeyCall VoIP uygulama örnek olarak kullanarak yeniden gelen çağrı iş akışı işlemek için CallKit nasıl kullanabileceğinizi ayrıntılı bir göz atalım.

### <a name="informing-user-of-incoming-call"></a>Gelen çağrıyı kullanıcı bildiren

Uzak bir kullanıcı bir VoIP konuşma yerel kullanıcıyla başladığında, aşağıdakiler gerçekleşir:

[ ![](callkit-images/callkit05.png "Uzak bir kullanıcı bir VoIP konuşma başlatıldı")](callkit-images/callkit05.png)

1. Uygulama, gelen VoIP araması olduğunu, kendi iletişimleri ağdan bir bildirim alır.
2. Uygulama kullandığı `CXProvider` göndermek için bir `CXCallUpdate` bu çağrının bildiren sistem.
3. Sistem, sistem kullanıcı Arabirimi, sistem hizmetleri ve CallKit kullanarak diğer VoIP uygulamaları çağrısı yayımlar.

Örneğin, `CXProviderDelegate`:

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

Bu kod yeni bir oluşturur `CXCallUpdate` örneği ve bir işleyici çağıran tanımlayacak ekler. Ardından, kullanan `ReportNewIncomingCall` yöntemi `CXProvider` çağrı sistem bildirmek için sınıf. Çağrı başarılı olursa, eklenen değilse, etkin çağrı uygulamanın koleksiyona hata kullanıcıya bildirilmesi gerekir.

### <a name="user-answering-incoming-call"></a>Kullanıcı yanıtlama gelen çağrıyı

Kullanıcı gelen VoIP aramaya yanıt isterse, aşağıdakiler gerçekleşir:

[ ![](callkit-images/callkit06.png "Kullanıcı gelen VoIP çağrıyı yanıtlar")](callkit-images/callkit06.png)

1. Sistem kullanıcı Arabirimi, kullanıcının VoIP aramaya yanıt vermek istediği sistem sizi bilgilendirir.
2. Sistem gönderir bir `CXAnswerCallAction` uygulamanın için `CXProvider` , yanıt hedefinin bildiren.
3. Uygulama kullanıcı Çağrı yanıtlanıyor ve VoIP çağrısı her zamanki gibi devam eder, iletişimin ağ bildirir.

Örneğin, `CXProviderDelegate`:

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Bu kod, etkin çağrı listesini verilen çağrısında ilk arar. Çağrı bulunamazsa sistem bildirilir ve yöntemi çıkar. Bulunursa, `AnswerCall` yöntemi `ActiveCall` sınıfı çağrısı başlatmak için çağrılır ve başarılı veya başarısız olursa bilgi sistemidir.

### <a name="user-ending-incoming-call"></a>Gelen çağrıyı bitiş kullanıcı

Kullanıcı uygulamanın kullanıcı Arabirimi içinden çağrısından yüklemeyi sonlandırmak isterse, aşağıdakiler gerçekleşir:

[ ![](callkit-images/callkit07.png "Kullanıcının uygulamanın kullanıcı Arabirimi içinden çağrısından sonlandırır")](callkit-images/callkit07.png)

1. Uygulamasını oluşturur `CXEndCallAction` , paket içine bir `CXTransaction` , gönderildiği sisteme çağrı sonlandırıyor bildirin.
2. Sistem çağrısı son hedefi doğrular ve gönderir `CXEndCallAction` aracılığıyla uygulamayı dön `CXProvider`.
3. Uygulama sonra çağrı sonlandırıyor iletişim ağ sizi bilgilendirir.

Örneğin, `CXProviderDelegate`:

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Bu kod, etkin çağrı listesini verilen çağrısında ilk arar. Çağrı bulunamazsa sistem bildirilir ve yöntemi çıkar. Bulunursa, `EndCall` yöntemi `ActiveCall` sınıfı aramayı sonlandırmak için çağrılır ve başarılı veya başarısız olursa bilgi sistemidir. Başarılı olursa, çağrı etkin çağrı koleksiyonundan kaldırılır.

## <a name="managing-multiple-calls"></a>Birden fazla çağrı yönetme

Çoğu VoIP uygulamalar aynı anda birden fazla çağrı işleyebilir. Örneğin, şu anda etkin bir VoIP çağrı ise ve bir orada yeni gelen bir arama olan kullanıcı olduğunu uygulama alır bildirim duraklatabilirsiniz veya kapanma ikincisi yanıtlamak için ilk çağrıda için.

Yukarıdaki durum verin, sistem göndereceğiniz bir `CXTransaction` birden çok eylemlerin bir listesini içeren bir uygulamaya (gibi `CXEndCallAction` ve `CXAnswerCallAction`). Sistem kullanıcı Arabirimi uygun şekilde güncelleştirebilmeniz için tüm bu işlemleri ayrı ayrı yerine getirilmesi gerekir.

## <a name="handling-outgoing-calls"></a>Giden aramalar işleme

Kullanıcı listesinden Recents (telefon uygulama) bir giriş dokunur, örneğin, çağrısından uygulamasına ait olan, gönderilecek bir _çağrısı hedefi Başlat_ sistem tarafından:

[ ![](callkit-images/callkit08.png "Bir başlangıç çağrısı hedefi alma")](callkit-images/callkit08.png)

1. Uygulama oluşturur bir _başlangıç eylemi çağırma_ Başlat çağrı sistemden alınan hedefi göre. 
2. Uygulama kullanır `CXCallController` başlangıç eylemi çağırma sistemden istemek için.
3. Eylem sistem kabul ederse, bu aracılığıyla uygulamayı döndürülürsünüz `XCProvider` temsilci.
4. Uygulama kendi iletişimi ağla giden çağrı başlatır.

Hedefleri hakkında daha fazla bilgi için lütfen bkz bizim [amaçları ve hedefleri UI uzantıları](~/ios/platform/sirikit/understanding-sirikit.md) belgeleri. 

### <a name="the-outgoing-call-lifecycle"></a>Giden çağrı yaşam döngüsü

CallKit ve giden bir çağrı ile çalışırken, uygulamanın aşağıdaki yaşam döngüsü olayları sistem bildirmeniz gerekir:

1. **Başlangıç** -giden bir çağrı başlamak için sistemi bildirin.
2. **Başlatılan** -giden bir çağrı başlatıldı sistem bildirin.
3. **Bağlanma** -giden çağrı bağlanma sistem bildirin.
4. **Bağlı** -giden bildirmek çağrısı bağlı ve her iki taraf şimdi iletişim kurabilirsiniz.

Örneğin, aşağıdaki kodu giden bir çağrı başlatılır:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Oluşturduğu bir `CXHandle` ve yapılandırmak için kullanılan bir `CXStartCallAction` içine paketlenmiş bir `CXTransaction` diğer bir deyişle sistem kullanmaya gönderilen `RequestTransaction` yöntemi `CXCallController` sınıfı. Çağırarak `RequestTransaction` yöntemi, sistem kaynağı olursa olsun tüm mevcut çağrıları durdurulma, yerleştirebileceğiniz (telefon uygulaması, FaceTime, VoIP, vb.), yeni çağrı başlamadan önce.

Giden bir VoIP çağrı başlatma isteği Siri, bir kişi kartına (kişiler uygulama) bir giriş gibi birkaç farklı kaynaklardan veya listesinden Recents (telefon uygulama) gelebilir. Bu durumda, bir başlangıç çağrısı hedefi içinde uygulama gönderilecek bir `NSUserActivity` ve işlenmesi AppDelegate gerekir:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

Burada `CallHandleFromActivity` yardımcı sınıfı yöntemi `StartCallRequest` tanıtıcı çağrılan kişiye almak için kullanılır (bkz [StartCallRequest sınıfı](#The-StartCallRequest-Class) yukarıda). 

`PerformStartCallAction` Yöntemi [ProviderDelegate sınıfı](#The-ProviderDelegate-Class) son gerçek giden çağrısı başlatmak ve kendi ömrü sistem bildirmek için kullanılır:

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Bir örneğini oluşturur `ActiveCall` (Sürüyor çağrı hakkındaki bilgileri tutmak üzere) sınıf ve çağrılan kişiyle doldurur. `StartingConnectionChanged` Ve `ConnectedChanged` olayları izlemek ve giden çağrı yaşam döngüsü bildirmek için kullanılır. Çağrı başlatılır ve eylem getirildiğini sistem bilgisi.

### <a name="ending-an-outgoing-call"></a>Giden bir çağrı bitiş

Kullanıcı giden bir çağrı ve onu sonuna dileklerimle tamamladıktan sonra aşağıdaki kodu kullanılabilir:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Varsa oluşturur bir `CXEndCallAction` sonlandırmak için çağrı UUID ile içinde sunmaktadır bir `CXTransaction` diğer bir deyişle sistem kullanmaya gönderilen `RequestTransaction` yöntemi `CXCallController` sınıfı. 

## <a name="additional-callkit-details"></a>Ek CallKit ayrıntıları

Bu bölümde, geliştirici ile CallKit gibi çalışırken dikkate gereken bazı ek ayrıntıları ele alınacaktır:

- Sağlayıcı Yapılandırması
- Eylem hataları
- Sistem kısıtlamaları
- VoIP ses

### <a name="provider-configuration"></a>Sağlayıcı Yapılandırması

Sağlayıcı Yapılandırması (içindeki yerel çağrı UI) kullanıcı deneyimini zaman özelleştirmek bir iOS 10 VoIP uygulamanın sağlar CallKit ile çalışma.

Bir uygulama aşağıdaki türlerdeki özelleştirmeleri yapabilirsiniz:

- Yerelleştirilmiş adını görüntüler.
- Görüntülü arama desteğini etkinleştirin.
- In-çağrı UI çubuğundaki düğmeler kendi maskelenmiş görüntü simgesi sunarak özelleştirin. Özel düğmeler ile kullanıcı etkileşimi işlenmek üzere doğrudan uygulamaya gönderilir. 

### <a name="action-errors"></a>Eylem hataları

düzgün biçimde başarısız olan eylemler işlemek ve eylem durumu her zaman haberdar kullanıcı tutmak CallKit kullanarak iOS 10 VoIP uygulamaları gerekir. 

Aşağıdaki örnek dikkate alın:

1. Uygulama başlangıç eylemi çağırma aldı ve iletişim ağ ile yeni bir VoIP çağrı başlatma işlemi başladı.
2. Sınırlı veya hiçbir ağ iletişimi özelliğini nedeniyle, bu bağlantı başarısız olur.
3. Uygulama *gerekir* Gönder **başarısız** ileti geri çağırma eylemi Başlat (`Action.Fail()`) hata sistem bilgilendirmek için.
4. Bu kullanıcının çağrı durumunu bildirmek sistem sağlar. Örneğin, çağrı hatası UI görüntülemek için şunu yazın.

Ayrıca, bir iOS 10 VoIP uygulama yanıt gerekecek _zaman aşımı hataları_ beklenen eylem verilen sürede işlendiğinde meydana gelebilir. CallKit tarafından sağlanan her eylem türü ile ilişkili en fazla bir zaman aşımı değerini sahiptir. Bu zaman aşımı değerleri, kullanıcı tarafından istenen herhangi bir CallKit eylem esnek bir şekilde, böylece işletim sistemi esnektir, hızlı yanıt de tutma ele emin olun.

Sağlayıcı temsilci üzerinde çeşitli yöntemler vardır (`CXProviderDelegate`), geçersiz kılınmış bu zaman aşımı durumlar da kolaylıkla idare edecek.

### <a name="system-restrictions"></a>Sistem kısıtlamaları

İOS 10 VoIP uygulamayı çalıştıran iOS cihaz geçerli durumuna bağlı olarak, bazı sistem kısıtlamalar zorlanabilir.

Örneğin, gelen bir VoIP arama, sistem tarafından kısıtlanabilir:

1. Arayan kişi kullanıcının engellenen çağıran listesinde yer alıyor.
2. Kullanıcının iOS aygıtı Not rahatsız modunda değil.

Bir VoIP çağrı bu durumların herhangi birinde tarafından sınırlandırılır işlenmesi için aşağıdaki kodu kullanın:

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);
    
        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>VoIP ses

CallKit iOS 10 VoIP uygulama canlı bir VoIP çağrısı sırasında gerektirecektir ses kaynakları işleme için birkaç avantaj sağlar. En büyük avantajlarından biri uygulamanın ses oturum yükselttiğinizden öncelikleri iOS 10 çalıştırırken. Bu telefon yerleşik olarak aynı öncelik düzeyine ve FaceTime uygulamaları ve bu geliştirilmiş öncelik düzeyi diğer çalışan uygulamalar VoIP uygulamanın ses oturumunu kesintiye engeller.

Ayrıca, CallKit performansını geliştirir ve akıllıca VoIP ses kullanıcı tercihleri ve cihaz durumları göre dinamik bir çağrı sırasında özel çıkış cihazlara yönlendirmek diğer ses Yönlendirme ipuçları erişebilir. Örneğin, Canlı CarPlay bağlantısı veya erişilebilirlik ayarlarını bluetooth kulaklıklar gibi ekli cihazlara bağlı.

Tipik bir VoIP yaşam döngüsü sırasında CallKit çağrıda, uygulama CallKit onu sağlayacak ses akışı yapılandırmanız gerekecektir. Aşağıdaki örnek göz atın:

[ ![](callkit-images/callkit09.png "Başlangıç çağrı eylem dizisi")](callkit-images/callkit09.png)

1. Eylemi Başlat çağırma gelen bir arama yanıtlamak için uygulama tarafından alınır.
2. Bu eylem uygulama tarafından getirilene önce yapılandırmayı için gerektirecektir sağlar, `AVAudioSession`.
3. Uygulama eylem yerine emin sistemin bildirir.
4. Çağrı bağlanmadan önce yüksek öncelikli CallKit sağlar `AVAudioSession` uygulama istenen yapılandırma eşleşen. Uygulama üzerinden bildirilecek `DidActivateAudioSession` yöntemi kendi `CXProviderDelegate`.

## <a name="working-with-call-directory-extensions"></a>Çağrı dizin uzantıları ile çalışma

CallKit ile çalışırken _çağrısı dizin uzantıları_ engellenen çağrısı sayıları ve belirli bir VoIP uygulamanın kişi uygulama iOS cihazında kişilere özgü numaraları tanımlamak için bir yol sağlar.

### <a name="implementing-a-call-directory-extension"></a>Bir arama dizinini uzantısı uygulama

Bir arama dizinini uzantısı bir Xamarin.iOS uygulaması uygulamak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'da uygulamanın çözümü açın
2. Çözüm adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle** > **Yeni Proje Ekle**.
3. Seçin **iOS** > **uzantıları** > **çağrısı dizin uzantıları** tıklatıp **sonraki** düğmesi: 

    [ ![](callkit-images/calldir01.png "Yeni bir çağrı dizin uzantısı oluşturma")](callkit-images/calldir01.png)
4. Girin bir **adı** tıklatın ve uzantı için **sonraki** düğmesi: 

    [ ![](callkit-images/calldir02.png "Uzantı için bir ad girme")](callkit-images/calldir02.png)
5. Ayarlama **proje adı** ve/veya **çözüm adı** gerekli ve tıklatırsanız **oluşturma** düğmesi: 

    [ ![](callkit-images/calldir03.png "Proje oluşturma")](callkit-images/calldir03.png) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Uygulamanın çözümü Visual Studio'da açın.
2. Çözüm adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle** > **Yeni Proje Ekle**.
3. Seçin **iOS** > **uzantıları** > **çağrısı dizin uzantıları** tıklatıp **sonraki** düğmesi: 

    [ ![](callkit-images/calldir01w.png "Yeni bir çağrı dizin uzantısı oluşturma")](callkit-images/calldir01.png)
4. Girin bir **adı** tıklatın ve uzantı için **Tamam** düğmesi

-----


Bu ekler bir `CallDirectoryHandler.cs` şuna benzeyen projesine sınıfı:

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

`BeginRequest` Yöntemi çağrısı dizin işleyicisinde gerekli işlevselliği sağlamak için değiştirilmesi gerekir. Yukarıdaki örnek söz konusu olduğunda, VoIP uygulamanın kişiler veritabanında engellenen ve kullanılabilir numaraları listesi ayarlamaya çalışır. Herhangi bir nedenle başarısız ya da istekleri oluşturmazsanız, bir `NSError` hata tanımlamak ve iletmektir `CancelRequest` yöntemi `CXCallDirectoryExtensionContext` sınıfı.

Engellenen numaralarını kullanın ayarlamak için `AddBlockingEntry` yöntemi `CXCallDirectoryExtensionContext` sınıfı. Yönteme sağlanan numaraları _gerekir_ sayısal olarak artan düzende olabilir. Olduğunda birçok telefon, en iyi performans ve bellek kullanımı için yalnızca belirli bir zamanda bir alt sayıların yükleniyor ve autorelease havuzlarını sırasında yüklenen sayıların her bir toplu iş ayrılmış nesneleri serbest kullanmayı düşünün.

Kişi uygulaması VoIP uygulamaya bilinen kişi sayıların bildirmek için `AddIdentificationEntry` yöntemi `CXCallDirectoryExtensionContext` sınıfı ve hem sayı hem de tanımlayıcı bir etiketi sağlar. Yönteme sağlanan numaralarını yeniden _gerekir_ sayısal olarak artan düzende olabilir. Olduğunda birçok telefon, en iyi performans ve bellek kullanımı için yalnızca belirli bir zamanda bir alt sayıların yükleniyor ve autorelease havuzlarını sırasında yüklenen sayıların her bir toplu iş ayrılmış nesneleri serbest kullanmayı düşünün.


## <a name="summary"></a>Özet

Bu makalede yeni CallKit API, Apple iOS 10 ve Xamarin.iOS VoIP uygulamalarda uygulama yayımlanan kapsamına. CallKit sistem iOS tümleştirmek bir uygulamanın nasıl sağlar, nasıl yerleşik uygulamalarla (örneğin, telefon) özellik eşliği sağlar ve nasıl Siri etkileşimleri aracılığıyla ve aracılığıyla konumlarda kilit ve giriş ekranlar gibi iOS boyunca bir uygulamanın görünürlük artırır göstermiştir Kişiler uygulamalar.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
