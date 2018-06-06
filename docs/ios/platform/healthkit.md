---
title: Xamarin.iOS HealthKit
description: Bu belgede HealthKit, sistem durumu ile ilgili bilgiler için merkezi, Eşgüdümlü ve güvenli bir veri deposu sağlayan iOS 8'de sunulan bir çerçeve açıklanmaktadır. HealthKit uygulama sağlama ve HealthKit çerçevesi kullanır kodunun nasıl yazılacağını açıklar.
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 06c0231bbb9aa7b82b92e0a8c2157b8be9c8b05b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787539"
---
# <a name="healthkit-in-xamarinios"></a>Xamarin.iOS HealthKit

Sistem durumu Seti güvenli bir veri deposu için kullanıcının durumuyla ilgili bilgileri sağlar. Sistem durumu Seti uygulamalar kullanıcının açık izne okuma ve bu veri deposuna yazma ve ilgili verileri eklendiğinde bildirimleri almak. Uygulamalar verileri sunabilir veya kullanıcının Apple'nın sağlanan sistem durumu uygulamayı bir Pano tüm verilerini görüntülemek için kullanabilirsiniz.

Sistem durumu ile ilgili verileri sistem durumu Seti kesin türü belirtilmiş, ölçü birimleri hem açık ilişkilendirme (örneğin, kan glucose düzeyi veya Kalp oranı) kaydedilen bilgi türü ile birlikte bunu hassas ve önemli, olduğundan. Ayrıca, sistem durumu Seti uygulamaları açık yetkilendirmeler kullanmalıdır, belirli türde bilgileri ve kullanıcı isteği erişimini açıkça bu tür veri için uygulama erişimi vermelidir gerekir.

Bu makalede getirir:

- Uygulama sağlama ve kullanıcı durumu Seti veritabanına erişim izni isteyen dahil olmak üzere, sistem durumu Seti'nın güvenlik gereksinimlerini;
- Yanlış uygulayarak veya veri misinterpreting olasılığını en aza indirir sistem durumu Seti'nın tür sistemi;
- Paylaşılan, sistem genelinde sistem durumu Seti veri deposuna yazma.

Bu makalede, veritabanını sorgulama, ölçü birimleri arasında dönüştürme ya da yeni veri bildirim alma gibi daha gelişmiş konular ele.

Bu makalede, biz kullanıcının Kalp oranı kaydetmek için örnek bir uygulama oluşturmaya olursunuz:

[![](healthkit-images/image01.png "Kullanıcıların Kalp oranı kaydetmek için örnek bir uygulama")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>Gereksinimler

Bu makalede sunulan adımları tamamlamak için aşağıdakiler gereklidir:

- **Xcode 7 ve iOS 8 (veya daha büyük)** – Apple'nın en son Xcode ve iOS API gereken yüklenmeli ve geliştirici bilgisayarda yapılandırılmış.
- **Mac veya Visual Studio için Visual Studio** – Mac için Visual Studio en son sürümünü yüklenmeli ve geliştirici bilgisayarda yapılandırılmış.
- **iOS 8 (veya daha büyük) cihaz** – 8 veya test için büyük iOS en son sürümünü çalıştıran bir iOS cihazı.

> [!IMPORTANT]
> Sistem durumu Seti iOS 8 sunulmuştur. Şu anda, sistem durumu Seti iOS simulator'da kullanılabilir değil ve hata ayıklama fiziksel bir iOS cihazına bağlantısı gerektirir.




## <a name="creating-and-provisioning-a-health-kit-app"></a>Oluşturma ve bir sistem durumu Seti uygulaması sağlama
Bir Xamarin iOS 8 uygulaması HealthKit API kullanmadan önce onu düzgün yapılandırılmış sağlanan ve gerekir. Bu bölümde düzgün Xamarin uygulamanızı kurma için gerekli olan adımları kapsar.

Sistem durumu Seti uygulamaları gerekir:

- Açık bir **uygulama kimliği**.
- A **sağlama profili** ile açık ilişkili **uygulama kimliği** ile **sistem durumu Seti** izinleri.
- Bir `Entitlements.plist` ile bir `com.apple.developer.healthkit` türündeki özelliği `Boolean` kümesine `Yes`.
- Bir `Info.plist` , `UIRequiredDeviceCapabilities` anahtar ile bir giriş içerir `String` değeri `healthkit`.
- `Info.plist` Uygun gizlilik açıklama girişlerine sahip olması gerekir: bir `String` anahtarı için bir açıklama `NSHealthUpdateUsageDescription` veri yazmak için uygulama edecekse ve `String` anahtarı için bir açıklama `NSHealthShareUsageDescription` uygulama sistem durumu Seti okumak için edecekse veriler.

Bir iOS uygulaması sağlama hakkında daha fazla bilgi için [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) Xamarin'ın makalesinde **Başlarken** serisi Geliştirici sertifikalar, uygulama kimlikleri arasındaki ilişkiyi açıklar Sağlama profilleri ve uygulama Yetkilendirmelerini.

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>Açık uygulama kimliği ve sağlama profili

Açık bir oluşturulmasını **uygulama kimliği** ve uygun bir **sağlama profili** Apple içinde yapılır [iOS Geliştirme Merkezi](https://developer.apple.com/devcenter/ios/index.action). 

Geçerli **uygulama kimlikleri** içinde listelenen [tanımlayıcıları & profilleri, sertifikaları](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) Dev Center bölümü. Genellikle, bu listede gösterilir **kimliği** değerlerini `*`, belirten, **uygulama kimliği** - **adı** sonekleri herhangi bir sayı ile kullanılabilir. Bu tür *joker uygulama kimlikleri* sistem durumu Kit ile kullanılamaz.
 
Açık bir oluşturmak için **uygulama kimliği**, tıklatın **+** kullanmayı sağ üst düğmesini **iOS uygulama kimliği kaydetmek** sayfa:


[![](healthkit-images/image02.png "Apple Geliştirici Portalı üzerinde bir uygulama kaydetme")](healthkit-images/image02.png#lightbox)

Bir uygulama açıklaması oluşturduktan sonra görüntünün yukarıda gösterildiği gibi kullanın **açık uygulama kimliği** bölümünde, uygulamanız için bir kimlik oluşturun. İçinde **uygulama hizmetleri** bölümünde, onay **sistem durumu Seti** içinde **Hizmetleri'ni etkinleştir** bölümü.

İşiniz bittiğinde, basın **devam** kaydetmek için düğmesini **uygulama kimliği** hesabınızda. Size geri gidersiniz **sertifikalar, tanımlayıcılarını ve profilleri** sayfası. Tıklatın **sağlama profilleri** geçerli sağlama profillerinizi listesine alıp tıklatın **+** kullanmayı sağ üst köşesinde düğmesini **iOS ekleme Sağlama profili** sayfası. Seçin **iOS uygulaması geliştirme** seçeneğini ve tıklayın **devam** almak için **uygulama Kimliğini seçin** sayfası. Burada, açık seçtiğiniz **uygulama kimliği** daha önce belirttiğiniz:


[![](healthkit-images/image03.png "Açık uygulama kimliği seçin")](healthkit-images/image03.png#lightbox)

Tıklatın **devam** ve iş yeri belirtin kalan ekranlar aracılığıyla, **Geliştirici sertifikaları**, **aygıtlarını**ve bir **adı** bu **sağlama profili**:

[![](healthkit-images/image04.png "Sağlama profili oluşturuluyor")](healthkit-images/image04.png#lightbox)

Tıklatın **Generate** ve profilinizi oluşturulmasını bekler. Xcode'da yüklemek için çift tıklayın ve dosyayı indirin. Buna ait yükleme altında onaylayabilirsiniz **Xcode > Tercihler > hesapları > ayrıntıları görüntüle...** Henüz yüklü sağlama profilinizin görmeniz gerekir ve sistem durumu Seti ve içinde özel diğer hizmetler için simge olması gereken kendi **yetkilendirmeler** satır:

[![](healthkit-images/image05.png "Xcode'da profil görüntüleme")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>Uygulama Kimliği ilişkilendirme ve Xamarin.iOS uygulamanızı profiliyle sağlama

Oluşturulan ve uygun bir yüklü sonra **sağlama profili** açıklandığı gibi normalde Mac veya Visual Studio için Visual Studio'da bir çözüm oluşturmak için zaman olacaktır. Sistem durumu Seti erişim herhangi bir iOS C# veya F # projesi için kullanılabilir.

Xamarin iOS 8 proje el ile oluşturma işleminde size kılavuzluk yerine (Bu, önceden oluşturulmuş bir film şeridi ve kodu içerir) Bu makalede bağlı örnek uygulamasını açın. Örnek uygulama, sistem durumu etkin Seti ile ilişkilendirmek için **sağlama profili**, **çözüm paneli**, projeye sağ tıklayın ve ortaya çıkarmak kendi **seçenekleri** iletişim. Geçiş **iOS uygulama** panel ve açık girin **uygulama kimliği** uygulamanın daha önce oluşturduğunuz **paket tanımlayıcı**:

[![](healthkit-images/image06.png "Açık uygulama kimliği girin")](healthkit-images/image06.png#lightbox)

Şimdi geçmek **iOS paket imzalama** paneli. Son yüklenen, **sağlama profili**, açık olan ilişkisi ile **uygulama kimliği**, şimdi olarak kullanılabilecek **sağlama profili**:

[![](healthkit-images/image07.png "Sağlama profili seçin")](healthkit-images/image07.png#lightbox)

Varsa **sağlama profili** kullanılabilir değil, iki kez kontrol **paket tanımlayıcısı** içinde **iOS uygulama** paneli belirtilen karşı **iOS Geliştirici Merkezi** ve **sağlama profili** yüklenir (**Xcode > Tercihler > hesapları > ayrıntıları görüntüle...** ).

Zaman durumu Seti özellikli **sağlama profili** olan seçili tıklatın **Tamam** proje Seçenekleri iletişim kutusunu kapatmak için.

### <a name="entitlementsplist-and-infoplist-values"></a>Entitlements.plist ve Info.plist değerleri

Örnek uygulamayı içeren bir `Entitlements.plist` (sistem durumu Seti etkin uygulamalar için gerekli olan) dosyası ve her proje şablonu dahil değildir. Projenizi yetkilendirmeler içermiyorsa, projeye sağ tıklayın, seçin **Dosya > Yeni Dosya > iOS > Entitlements.plist** biri el ile eklemek için.

Sonuç olarak, `Entitlements.plist` aşağıdaki anahtar ve değer çifti olmalıdır:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.HealthKit</key>
    <true/>
</dict>
</plist>

```

Benzer şekilde, `Info.plist` uygulama değerini bulunmalıdır `healthkit` ile ilişkili `UIRequiredDeviceCapabilities` anahtarı:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

Bu makalede sağlanan örnek uygulama bir önceden yapılandırılmış içerir `Entitlements.plist` , tüm gerekli anahtarlarla içerir.

<a name="programming" />

## <a name="programming-health-kit"></a>Programlama durumu Seti

Sistem durumu Seti veri deposu uygulamalar arasında paylaşılan bir özel, kullanıcıya özgü veri deposu ' dir. Sistem durumu bilgisi önemli olduğundan, kullanıcı veri erişimine izin vermek için pozitif adımlar atmanız gerekir. Bu erişim kısmi (yazma ancak değil okuma, veri ve diğerleri, bazı türleri için erişim vb.) ve herhangi bir zamanda iptal edilebilir. Çok sayıda kullanıcı kendi sistem durumu ile ilgili bilgileri depolamak hakkında şüpheli olacağını anlama ile biçimde geliştirmelisiniz, sistem durumu Seti uygulamaları yazılması gerekir.

Sistem durumu Seti veri sınırlıdır Apple belirtilen türleri. Bu tür kesinlikle tanımlanır: başkalarının bir büyüklük (gram, kalori ve litre gibi) bir ölçü birimi birleştirin sırada kan türü gibi bazı sağlanan Apple numaralandırması, belirli değerleri sınırlıdır. Uyumlu bir ölçü paylaşmak bile veri ayırt edilen kendi `HKObjectType`; örneği için tür sistemi depolamak için hatalı bir girişim yakalar bir `HKQuantityTypeIdentifier.NumberOfTimesFallen` bekleniyor alan için değer bir `HKQuantityTypeIdentifier.FlightsClimbed` her ikisi de kullanmasına karşın` HKUnit.Count` Ölçü birimidir.

Sistem durumu Seti deposunda storable tüm alt sınıflarının türleridir `HKObjectType`. `HKCharacteristicType` nesneler Biyolojik seks, kan türünü ve tarihi, Doğum depolar. Ancak, daha fazla ortak olan `HKSampleType` belirli bir zamanda veya bir süre boyunca örneklenen verilerini temsil eden nesne. 

[![](healthkit-images/image08.png "HKSampleType nesneleri grafik")](healthkit-images/image08.png#lightbox)

`HKSampleType` soyut ve dört somut alt sınıfları olmayan. Şu anda yalnızca bir tür olduğundan `HKCategoryType` uyku analiz verileri. Sistem durumu Seti verilerde büyük çoğunluğu türündedir `HKQuantityType` ve kendi veri deposundaki `HKQuantitySample` tanıdık Fabrika tasarım modeli kullanılarak oluşturulan nesneler:

[![](healthkit-images/image09.png "Sistem durumu Seti verilerde büyük çoğunluğu türü HKQuantityType ve HKQuantitySample nesneleri verilerini depolamak")](healthkit-images/image09.png#lightbox)

`HKQuantityType` türleri aralığı `HKQuantityTypeIdentifier.ActiveEnergyBurned` için `HKQuantityTypeIdentifier.StepCount`. 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>Kullanıcıdan izin istemeden

Son kullanıcılar, bir uygulamanın durumu Seti veri okumaya veya yazmaya izin ver için pozitif adımlar atmanız gerekir. Bu, iOS 8 cihazlarda önceden yüklü olarak gelen sistem durumu uygulama aracılığıyla gerçekleştirilir. Sistem durumu Seti uygulama çalıştırıldığında, ilk kez kullanıcı bir sistem tarafından denetlenen ile sunulan **sistem durumu erişim** iletişim:

[![](healthkit-images/image10.png "Kullanıcıya sistem tarafından denetlenen sistem erişimi içeren bir iletişim kutusu sunulur")](healthkit-images/image10.png#lightbox)

Kullanıcı izinlerini sistem durumu uygulamanın kullanarak daha sonra değiştirebilirsiniz **kaynakları** iletişim:

[![](healthkit-images/image11.png "Kullanıcı izinleri sistem durumu uygulamalar kaynakları iletişim kutusunu kullanarak değiştirebilir")](healthkit-images/image11.png#lightbox)

Sistem durumu bilgisi son derece hassas olduğundan, uygulama geliştiriciler programlarını biçimde geliştirmelisiniz, izinler reddetti ve uygulama çalışırken değişti, Beklenti ile yazmanız gerekir. İzinler istemek için en yaygın deyim olduğundan `UIApplicationDelegate.OnActivated` yöntemi ve kullanıcı arabirimi uygun olarak değiştirin.

### <a name="permissions-walkthrough"></a>İzinleri gözden geçirme

Sistem durumu Seti sağlanan projenizde açmak `AppDelegate.cs` dosya. Using deyimi fark `HealthKit`; dosyasının üstünde.


Aşağıdaki kod, sistem durumu Seti izinleri ilişkili:

```csharp
private HKHealthStore healthKitStore = new HKHealthStore ();

public override void OnActivated (UIApplication application)
{
        ValidateAuthorization ();
}

private void ValidateAuthorization ()
{
        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
        var heartRateType = HKObjectType.GetQuantityType (heartRateId);
        var typesToWrite = new NSSet (new [] { heartRateType });
        var typesToRead = new NSSet ();
        healthKitStore.RequestAuthorizationToShare (
                typesToWrite, 
                typesToRead, 
                ReactToHealthCarePermissions);
}

void ReactToHealthCarePermissions (bool success, NSError error)
{
        var access = healthKitStore.GetAuthorizationStatus (HKObjectType.GetQuantityType (HKQuantityTypeIdentifierKey.HeartRate));
        if (access.HasFlag (HKAuthorizationStatus.SharingAuthorized)) {
                HeartRateModel.Instance.Enabled = true;
        } else {
                HeartRateModel.Instance.Enabled = false;
        }
}

```

Satır içi tüm bu yöntemleri kodda yapılabilir `OnActivated`, ancak bunların amacını daha anlaşılır yapmak için örnek uygulama ayrı yöntemleri kullanır: `ValidateAuthorization()` belirli yazılmakta türleri (ve uygulama isterseniz okuma) erişim isteğinde bulunmak için gerekli adımları vardır ve `ReactToHealthCarePermissions()` kullanıcı izinleri iletişim kutusunda Health.app ile etkileşime sonra etkin bir geri çağırma olduğu.

İş `ValidateAuthorization()` kümesini oluşturmak için `HKObjectTypes` uygulama yazmak ve bu verileri güncelleştirmek için yetkilendirme isteği. Örnek uygulama `HKObjectType` için anahtar `KHQuantityTypeIdentifierKey.HeartRate`. Bu tür kümeye eklenen `typesToWrite`, while kümesi `typesToRead` boş bırakılır. Bu ayarlar ve bir başvuru `ReactToHealthCarePermissions()` geri geçirilen `HKHealthStore.RequestAuthorizationToShare()`.

`ReactToHealthCarePermissions()` Kullanıcı izinleri iletişim kurduğunda ve iki ayrı bilgi geçtikten sonra geri çağırma çağrılır: bir `bool` değer `true` kullanıcı izinleri iletişim ve bir ileetkileşimevarsa`NSError`, null olmayan, belirten bir tür izinler iletişim sunan ile ilişkili hata.

> [!IMPORTANT]
> Bu işlev bağımsız değişkenleri hakkında temizlenmesini: _başarı_ ve _hata_ parametreleri değil belirtmek kullanıcı durumu Seti veri erişim izni verilmiş olup olmadığını! Bunlar, yalnızca kullanıcı veri erişimine izin vermek için Fırsat verildiğini gösterir.

Uygulama veri erişimi olup olmadığını doğrulamak için `HKHealthStore.GetAuthorizationStatus()` kullanılan, tümleştirilmesidir `HKQuantityTypeIdentifierKey.HeartRate`. Döndürülen durumuna bağlı olarak, uygulamayı etkinleştirir veya veri girme yeteneğini devre dışı bırakır. Erişim reddi ilgilenmek için hiçbir standart kullanıcı deneyimi yoktur ve birçok olası seçeneğiniz vardır. Örnek uygulama durumu üzerinde ayarlanmış bir `HeartRateModel` sırayla ilgili olayları başlatır, tekil nesnesi.

## <a name="model-view-and-controller"></a>Model, Görünüm ve denetleyici

Gözden geçirmek için `HeartRateModel` tekil nesnesi, açık `HeartRateModel.cs` dosyası:

```csharp
using System;
using HealthKit;
using Foundation;

namespace HKWork
{
        public class GenericEventArgs<T> : EventArgs
        {
                public T Value { get; protected set; }
                public DateTime Time { get; protected set; }

                public GenericEventArgs (T value)
                {
                        this.Value = value;
                        Time = DateTime.Now;
                }
        }

        public delegate void GenericEventHandler<T> (object sender,GenericEventArgs<T> args);

        public sealed class HeartRateModel : NSObject
        {
                private static volatile HeartRateModel singleton;
                private static object syncRoot = new Object ();

                private HeartRateModel ()
                {
                }

                public static HeartRateModel Instance {
                        get {
                                //Double-check lazy initialization
                                if (singleton == null) {
                                        lock (syncRoot) {
                                                if (singleton == null) {
                                                        singleton = new HeartRateModel ();
                                                }
                                        }
                                }

                                return singleton;
                        }
                }

                private bool enabled = false;

                public event GenericEventHandler<bool> EnabledChanged;
                public event GenericEventHandler<String> ErrorMessageChanged;
                public event GenericEventHandler<Double> HeartRateStored;

                public bool Enabled { 
                        get { return enabled; }
                        set {
                                if (enabled != value) {
                                        enabled = value;
                                        InvokeOnMainThread(() => EnabledChanged (this, new GenericEventArgs<bool>(value)));
                                }
                        }
                }

                public void PermissionsError(string msg)
                {
                        Enabled = false;
                        InvokeOnMainThread(() => ErrorMessageChanged (this, new GenericEventArgs<string>(msg)));
                }

                //Converts its argument into a strongly-typed quantity representing the value in beats-per-minute
                public HKQuantity HeartRateInBeatsPerMinute(ushort beatsPerMinute)
                {
                        var heartRateUnitType = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        var quantity = HKQuantity.FromQuantity (heartRateUnitType, beatsPerMinute);

                        return quantity;
                }
                        
                public void StoreHeartRate(HKQuantity quantity)
                {
                        var bpm = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        //Confirm that the value passed in is of a valid type (can be converted to beats-per-minute)
                        if (! quantity.IsCompatible(bpm))
                        {
                                InvokeOnMainThread(() => ErrorMessageChanged(this, new GenericEventArgs<string> ("Units must be compatible with BPM")));
                        }

                        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
                        var heartRateQuantityType = HKQuantityType.GetQuantityType (heartRateId);
                        var heartRateSample = HKQuantitySample.FromType (heartRateQuantityType, quantity, new NSDate (), new NSDate (), new HKMetadata());

                        using (var healthKitStore = new HKHealthStore ()) {
                                healthKitStore.SaveObject (heartRateSample, (success, error) => {
                                        InvokeOnMainThread (() => {
                                                if (success) {
                                                        HeartRateStored(this, new GenericEventArgs<Double>(quantity.GetDoubleValue(bpm)));
                                                } else {
                                                        ErrorMessageChanged(this, new GenericEventArgs<string>("Save failed"));
                                                }
                                                if (error != null) {
                                                        //If there's some kind of error, disable 
                                                        Enabled = false;
                                                        ErrorMessageChanged (this, new GenericEventArgs<string>(error.ToString()));
                                                }
                                        });
                                });
                        }
                }
        }
}

```

Genel olaylar ve işleyicileri oluşturmak için Demirbaş kod ilk bölümüdür. İlk kısmı `HeartRateModel` sınıfı, ayrıca bir iş parçacığı singleton nesnesi oluşturmak için ortak.

Ardından, `HeartRateModel` 3 olayları gösterir: 

- `EnabledChanged` -Kalp oranı depolama etkin veya devre dışı gösterir (depolama başlangıçta devre dışı bırakıldığını unutmayın). 
- `ErrorMessageChanged` -İçin bu örnek uygulama, biz çok basit bir hata işleme modeli vardır: son hata içeren bir dize. 
- `HeartRateStored` -Bir Kalp hızı sistem durumu Seti veritabanında depolanan tetiklenir.

Unutmayın bu olaylar her aracılığıyla yapılır `NSObject.InvokeOnMainThread()`, kullanıcı arabirimini güncelleştirmek aboneleri izin verir. Alternatif olarak, olaylar üzerinde arka plan iş parçacığı olarak gerçekleştirilen belgelenmiş ve uyumluluk sağlama sorumluluğu için kendi işleyicileri kalabilir. İzin isteği gibi işlevlerin çoğu zaman uyumsuz olduğundan ve kendi geri aramalar ana olmayan iş parçacıklarında yürütme iş parçacığı noktalar sistem durumu Seti uygulamalarda önemlidir.

Sistem durumu Seti belirli kodda `HeartRateModel` iki işlevler `HeartRateInBeatsPerMinute()` ve `StoreHeartRate()`. 

`HeartRateInBeatsPerMinute()` bağımsız değişken türü kesin belirlenmiş bir sistem durumu pakete dönüştürür `HKQuantity`. Miktarı tarafından belirtilen türünde `HKQuantityTypeIdentifierKey.HeartRate` ve miktarı birimleridir `HKUnit.Count` bölü `HKUnit.Minute` (diğer bir deyişle, birimdir *Vuruş dakika başına*). 

`StoreHeartRate()` İşlev sürer bir `HKQuantity` (örnek uygulamasında biri tarafından oluşturulan `HeartRateInBeatsPerMinute()` ). Verileri doğrulamak için kullandığı `HKQuantity.IsCompatible()` döndürür yöntemi `true` nesnenin birimleri bağımsız birimlerine dönüştürülebilir ise. Miktarı ile oluşturulmuşsa, `HeartRateInBeatsPerMinute()` bu açıkça döndürür `true`, ancak aynı zamanda döndürecekti `true` miktarı örneğin olarak oluşturulmuşsa *saatlik vuruş*. Daha sık `HKQuantity.IsCompatible()` yığın, doğrulamak için kullanılan uzaklık ve hangi kullanıcı veya aygıt giriş veya olabilir (örneğin, İngiliz birimleri) ölçü bir sistem görüntüsünde ancak hangi depolanır (örneğin, ölçü birimleri) başka bir sistem enerji. 

Miktarı uyumluluk doğrulandıktan sonra `HKQuantitySample.FromType()` kesin türü belirtilmiş bir oluşturmak için kullanılan Üreteç yöntemi `heartRateSample` nesnesi. `HKSample` nesneleri bir başlangıç ve bitiş tarihi; yine de sahip istiyor musunuz? örnekte olduğu gibi anlık okumalar için bu değerleri aynı olmalıdır. Örnek ayrıca herhangi bir anahtar-değer veri kümesinde değil, `HKMetadata` bağımsız değişkeni, ancak bir kodu aşağıdaki kod gibi algılayıcı konumu belirtmek için kullanın:

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

Bir kez `heartRateSample` bırakıldı oluşturulan kod veritabanına yeni bir bağlantı kullanarak oluşturur bloğu. Bu bloğu içinde `HKHealthStore.SaveObject()` yöntemi, zaman uyumsuz yazma veritabanına çalışır. Lambda ifadesi ortaya çıkan çağrı ilgili olayları ya da Tetikleyiciler `HeartRateStored` veya `ErrorMessageChanged`.

Model programlanmış, nasıl denetleyicisi model durumunu yansıtır bkz zamanı geldi. Açık `HKWorkViewController.cs` dosya. Kurucu yalnızca yukarı bağlayan `HeartRateModel` olay işleme yöntemleri tekliye (yeniden, bu satır içi lambda ifadeleri ile yapılabilir, ancak ayrı yöntemleri hedefi biraz daha belirgin hale):

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

İlgili işleyicileri şunlardır:

```csharp
void OnEnabledChanged (object sender, GenericEventArgs<bool> args)
{
        StoreData.Enabled = args.Value;
        PermissionsLabel.Text = args.Value ? "Ready to record" : "Not authorized to store data.";
        PermissionsLabel.SizeToFit ();
}

void OnErrorMessageChanged (object sender, GenericEventArgs<string> args)
{
        PermissionsLabel.Text = args.Value;
}

void OnHeartBeatStored (object sender, GenericEventArgs<double> args)
{
        PermissionsLabel.Text = String.Format ("Stored {0} BPM", args.Value);
}

```

Belli ki, uygulamanın tek bir denetleyici ayrı model nesnesi oluşturma ve denetim akışı olayları kullanılmasını önlemek mümkün olacaktır, ancak modeli nesnelerinin kullanımını gerçek uygulamalar için daha uygundur.

## <a name="running-the-sample-app"></a>Örnek uygulamayı çalıştırma

İOS simülatörü'nü sistem durumu Seti desteklemez. Hata ayıklama iOS 8 çalıştıran fiziksel bir aygıtta yapılması gerekir.

Düzgün olarak sağlanan iOS 8 geliştirme aygıt sisteminize ekleyin. Mac için Visual Studio dağıtım hedefi olarak seçin ve menüsünden **Çalıştır > hata ayıklama**.

> [!IMPORTANT]
> Bu noktada sağlamak için ilgili hatalar belirir. Hataları gidermek için oluşturma ve yukarıdaki bir sistem durumu Seti uygulama bölümü sağlama gözden geçirin. Bileşenleri şunlardır: 
>
> - **iOS Geliştirme Merkezi** -açık uygulama kimliği & sistem durumu Seti etkin sağlama profili. 
> - **Proje Seçenekleri** -paket tanımlayıcısı (açık uygulama kimliği) ve sağlama profili.
> - **Kaynak kodu** -Entitlements.plist & Info.plist

Hükümler düzgün ayarlamış olduğunuz varsayılarak, uygulamanızın başlar. Ulaştığında, `OnActivated` yöntemi, sistem durumu Seti yetkilendirme isteyecek. Bu işletim sistemi tarafından karşılaşılan ilk kez kullanıcı aşağıdaki iletişim kutusuyla sunulur:


[![](healthkit-images/image12.png "Bu iletişim kutusu ile kullanıcıya sunulur")](healthkit-images/image12.png#lightbox)

Kalp oranı verileri güncelleştirmek için uygulamanızı etkinleştirme ve uygulamanızı görünecektir. `ReactToHealthCarePermissions` Geri çağırma zaman uyumsuz olarak etkinleştirilecek. Bu neden olacak `HeartRateModel’s` `Enabled` oluşturacağı değiştirmek için özellik `EnabledChanged` neden olacak olay `HKPermissionsViewController.OnEnabledChanged()` çalıştırmak için hangi etkinleştirir olay işleyicisi `StoreData` düğmesi. Aşağıdaki diyagramda gösterilir:


[![](healthkit-images/image13.png "Bu diyagramda olayların sırası gösterir")](healthkit-images/image13.png#lightbox)

Tuşuna **kaydı** düğmesi. Bu neden olacak `StoreData_TouchUpInside()` değeri ayrıştırılamadı deneyeceği çalıştırmak için işleyici `heartRate` metin alanı içine dönüştürme bir `HKQuantity` daha önce ele alındığı aracılığıyla `HeartRateModel.HeartRateInBeatsPerMinute()` işlev ve bu miktar geçirmek `HeartRateModel.StoreHeartRate()`. Daha önce açıklandığı gibi bu verileri depolamak çalışır ve ya da oluşturacak bir `HeartRateStored` veya `ErrorMessageChanged` olay.

Çift **giriş** düğmesini aygıtınızda ve sistem durumu uygulamasını açın. Tıklatın **kaynakları** sekmesine tıkladığınızda listelenen örnek uygulaması görürsünüz. O bağlantıyı seçin ve Kalp hız verilerini güncelleştirmek için izinleri izin vermeyecek. Çift **giriş** düğmesi ve anahtar uygulamanıza geri. Bir kez daha, `ReactToHealthCarePermissions()` erişim reddedildiğinden, bu kez, çağrılacak **StoreData** düğmesi devre dışı duruma (Bu zaman uyumsuz olarak ortaya çıkar ve kullanıcı arabiriminde değişiklik son kullanıcıya görünür olabilir unutmayın).

## <a name="advanced-topics"></a>Gelişmiş Konular

Sistem durumu Seti'nden verileri okuma veritabanına veri yazmak için çok benzer: aşağıdakilerden birini erişmeye, istekleri yetkilendirme veri türlerini belirtir ve bu yetkilendirme atanırsa, verileri uyumlu birimleri otomatik dönüştürme ile kullanılabilir ölçü.

Koşulu tabanlı sorgular ve ilgili verileri güncelleştirildiğinde güncelleştirmeleri gerçekleştirmek sorgulara izin veren daha karmaşık sorgu işlevleri vardır. 

Sistem durumu Seti uygulaması geliştiricileri Apple'nın sistem durumu Seti bölümünü gözden geçirmeniz gereken [App gözden geçirme yönergeleri](https://developer.apple.com/app-store/review/guidelines/#healthkit).

Tür sistemi modelleri ve güvenlik anladım sonra depolama ve paylaşılan sistem durumu Seti veritabanındaki verileri okuma oldukça basittir. Sistem durumu Seti işlevlerinde çoğunu zaman uyumsuz olarak çalışır ve uygulama geliştiricileri programlarını uygun şekilde yazmanız gerekir.

Bu makalede makalenin yazıldığı sırada şu anda durumu Seti'nde Android veya Windows Phone için eşdeğeri yoktur.

## <a name="summary"></a>Özet

Sistem durumu Seti depolamak uygulamaların nasıl sağlayan gördük bu makalede, almak ve paylaşımı sistem durumu kullanıcı erişimi ve bu veriler üzerinde denetime izin veren standart bir sistem durumu uygulama aynı zamanda sağlarken, ilgili bilgiler. 

Biz nasıl gizlilik, güvenlik ve veri bütünlüğü geçersiz kılma durumuyla ilgili bilgileri endişelerini gördünüz ve sistem durumu Seti kullanarak uygulamaları ile artırma (hazırlama), uygulama yönetimi özelliklerini (sistem durumu Seti'nın türü kodlama karmaşıklığı ilgilenmeniz gerekir Sistem) ve kullanıcı (kullanıcı denetimi izinleri sistem iletişim kutuları ve sistem durumu uygulama aracılığıyla) deneyimi. 

Son olarak, bir basit uygulama, sistem durumu Seti deposuna sinyal verileri yazar ve zaman uyumsuz algılayan bir tasarıma sahiptir dahil edilmiş örnek uygulaması kullanılarak sistem durumu Seti'nin göz alma yaptık.

## <a name="related-links"></a>İlgili bağlantılar

- [HKWork (örnek)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [iOS 8’e Giriş](~/ios/platform/introduction-to-ios8.md)
