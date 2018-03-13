---
title: SiriKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/07/2017
ms.openlocfilehash: 557521bc3bce41b9023acbf31a344a57cb63d2a1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="sirikit"></a>SiriKit

SiriKit iOS 10, hizmet etki alanları (egzersizleriniz, kayıt ve çağrıları yapma kılma dahil) bir dizi sunulmuştur. Başvurmak [SiriKit bölüm](~/ios/platform/sirikit/index.md) SiriKit kavramları ve uygulamanızda SiriKit gerçekleştirme.

![Siri görev listesi Tanıtımı](sirikit-images/sirikit.png)

İOS 11 SiriKit bu yeni ve güncelleştirilmiş hedefi etki alanlarını ekler:

- [**Listeler ve Notlar** ](#listsnotes) – yeni! Görevler ve notlar işlemek için bir API uygulamaları için sağlar.
- **Görsel kodları** – yeni! Siri kişi bilgilerini paylaşma veya ödeme işlemlere katılmasına QR kodlarını görüntüleyebilirsiniz.
- **Ödemeler** – eklenen ödeme etkileşimler için arama ve aktarım amaçlar.
- **Kayıt ride** – eklenen kılma ve geri bildirim hedefleri iptal edin.

Diğer yeni özellikleri şunlardır:

- [**Alternatif uygulama adlarının** ](#alternativenames) – müşterilere yardımcı olun sağlar diğer adlar adları/Söyleyiş sunarak uygulamanızı hedeflemek için Siri söyleyin.
- **Egzersizleriniz başlangıç** – arka planda bir etkinlik başlangıç olanağı sağlar.

Bu özelliklerden bazıları aşağıda açıklanmıştır. Başkaları hakkında daha fazla bilgi almak için başvurmak [Apple SiriKit belgeleri](https://developer.apple.com/documentation/sirikit).

<a name="listsnotes" />

## <a name="lists-and-notes"></a>Listeler ve notlar

Yeni listeler ve notlar etki görevler ve notlar Siri ses istekler aracılığıyla işlemek için bir API uygulamaları için sağlar.

**Görevler**

- Başlık ve tamamlanma durumuna sahip.
- İsteğe bağlı olarak bir son tarih ve konumu içerir.

**Notlar**

- Başlık ve içerik alana sahip.

Görevler ve notlar gruplar halinde düzenlenebilir. Bu bölümde rest SiriKit, bu yeni etki alanında uygulamak açıklar kullanarak [TasksNotes SiriKit örnek](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/).

### <a name="how-to-process-a-sirikit-request"></a>Bir SiriKit isteği işlemek nasıl

Aşağıdaki adımları izleyerek bir SiriKit isteğinizi:

1. **Gidermek** – parametreleri doğrulayın ve daha fazla bilgi (gerekiyorsa) kullanıcıdan istemek.
2. **Onayla** – son doğrulama ve doğrulama isteği işlenebilir.
3. **Tanıtıcı** – (verileri güncelleştirmek veya ağ işlemlerini gerçekleştirme) işlem gerçekleştirilemiyor.

İlk iki adım (teşvik rağmen) isteğe bağlıdır, ve son adım gereklidir.
Daha ayrıntılı yönergelere vardır [SiriKit bölüm](~/ios/platform/sirikit/index.md).

### <a name="resolve-and-confirm-methods"></a>Çözümlemek ve yöntemleri onaylayın

Bu isteğe bağlı yöntemler doğrulama, select Varsayılanları veya istek ek bilgileri kullanıcıdan gerçekleştirmek kodunuzu olanak tanır.

Örneğin, için `IINCreateTaskListIntent` arabirimdir, gerekli yöntemi `HandleCreateTaskList`. Siri etkileşim üzerinde daha fazla denetim sağlamak dört isteğe bağlı yöntem vardır:

- `ResolveTitle` – Başlığı doğrular, varsayılan bir başlık (uygunsa) ayarlar veya veri gerekli olmadığını bildirir.
- `ResolveTaskTitles` – Kullanıcı tarafından konuşulan görevlerinin listesi doğrular.
- `ResolveGroupName` – Grup adını doğrular, bir varsayılan grubu seçer veya veri gerekli olmadığını bildirir.
- `ConfirmCreateTaskList` – Kodunuzu istenen işlemi gerçekleştirebilir, ancak bunu gerçekleştirmez doğrular (yalnızca `Handle*` yöntemleri verileri değiştirme).

### <a name="handle-the-intent"></a>Tanıtıcı hedefi

Listeler ve notlar etki alanında altı hedefleri, üç görevler ve notları üç vardır.
Bu hedefleri işlemek için uygulamanız gereken yöntemler şunlardır:

- Görevler için:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- İçin Notlar:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

Her yöntem Siri kullanıcının istekten Ayrıştırılan tüm bilgileri içeren, geçirilen belirli hedefi türüne sahip (ve muhtemelen güncelleştirilir `Resolve*` ve `Confirm*` yöntemleri).
Uygulamanız gerekir, sağlanan, verileri ayrıştırmak sonra bazı eylemler depolamak için ya da aksi takdirde işlem verileri gerçekleştirmek ve Siri konuşur ve kullanıcıya gösteren bir sonuç döndürür.

### <a name="response-codes"></a>Yanıt kodları

Gerekli `Handle*` ve isteğe bağlı `Confirm*` yöntemleri kendi tamamlama işleyicisine geçirirler nesne üzerinde bir değer ayarlanarak bir yanıt kodu belirtin. Yanıtları gelen `INCreateTaskListIntentResponseCode` numaralandırma:

- `Ready` – Döndürür onay aşamasında (IE. gelen bir `Confirm*` yöntemi, ancak bir `Handle*` yöntemi).
- `InProgress` – Uzun süre çalışan görevler (örneğin, bir ağ/sunucu işlemi) için kullanılır.
- `Success` – Başarılı bir işlem ayrıntılarını ile yanıt (yalnızca bir `Handle*` yöntemi).
- `Failure` – Bir hata oluştu ve işlemi tamamlanamadı anlamına gelir.
- `RequiringAppLaunch` – Hedefi tarafından işlenemez ancak uygulamada olası bir işlemdir.
- `Unspecified` – Kullanmayın: kullanıcıya hata iletisi görüntülenir.

Bu yöntem ve Apple'nın yanıtları hakkında daha fazla bilgi [SiriKit listeler ve belgeleri Not](https://developer.apple.com/documentation/sirikit/lists_and_notes).

### <a name="implementing-lists-and-notes"></a>Listeler ve notlar uygulama

[TasksNotes SiriKit örnek](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/) boş bir iOS uygulaması için SiriKit desteği eklemek için aşağıdaki adımları kullanarak oluşturuldu.

İlk olarak, SiriKit desteği eklemek için iOS uygulamanız için aşağıdaki adımları izleyin:

1. Değer çizgilerinin **SiriKit** içinde **Entitlements.plist**.
2. Ekleme **gizlilik – Siri kullanım açıklama** anahtarını **Info.plist**, müşterileriniz için bir ileti ile birlikte.
3. Çağrı `INPreferences.RequestSiriAuthorization` Siri etkileşimleri izin vermek için kullanıcıdan istemek için bu uygulamadaki yöntemi.
4. Uygulama Kimliğiniz Geliştirici portalındaki SiriKit eklemek ve yeni yetkilendirme dahil etmek için sağlama profilleri yeniden oluşturun.

Daha sonra yeni bir uzantı projesi Siri isteklerini işlemek için uygulamanıza ekleyin:

1. Çözüm üzerinde sağ tıklatın ve seçin **Ekle > Yeni Proje Ekle...** .
2. Seçin **iOS > uzantısı > hedefleri uzantısı** şablonu.
3. İki yeni projeler eklenir: amacını ve IntentUI. UI Özelleştirme isteğe bağlı, örnek kodda yalnızca içerecek şekilde **hedefi** projesi.

Uzantı projesi, burada tüm SiriKit istekleri işlenecek ' dir. Ayrı bir uzantısı olarak otomatik olarak iletişim kurmak için ana uygulamanızla – bu uygulama grupları kullanarak paylaşılan dosya depolama uygulayarak genellikle çözülene herhangi bir şekilde yok.

#### <a name="configure-the-intenthandler"></a>IntentHandler yapılandırın

`IntentHandler` Sınıftır giriş noktası Siri istekleri – her amacı geçirilir için `GetHandler` yöntemi isteği işleyen bir nesne döndürür.

Aşağıdaki kod, bir basit uygulama gösterir:

```csharp
[Register("IntentHandler")]
public partial class IntentHandler : INExtension, IINNotebookDomainHandling
{
  protected IntentHandler(IntPtr handle) : base(handle)
  {}
  public override NSObject GetHandler(INIntent intent)
  {
    // This is the default implementation.  If you want different objects to handle different intents,
    // you can override this and return the handler you want for that particular intent.
    return this;
  }
  // add intent handlers here!
}
```

Sınıf öğesinden devralmalıdır `INExtension`, ve örnek listeleri işlemeye geçiyor ve hedefleri Notlar olduğundan, ayrıca uygulayan `IINNotebookDomainHandling`.

> [!NOTE]
> **Adlandırma hakkında Not:** büyük ile önek olarak .NET arabirimler için bir kural yok `I`, hangi Xamarin iOS SDK'sı protokolleri bağlama sırasında aynılarını.
>
> Xamarin iOS türü adlarından da korur ve Apple ilk iki karakter türü ait framework yansıtacak şekilde tür adlarını kullanır.
>
> İçin `Intents` framework türleri öneki `IN*` (ör.) `INExtension`), ancak bunlar _değil_ arabirimleri.
> Ayrıca (C# arabirimleri hale) protokolleri iki şunun olduğunu izleyen `I`s, gibi `IINAddTasksIntentHandling`.

#### <a name="handling-intents"></a>İşleme hedefleri

Her hedefi (Görev Ekle, görev öznitelik kümesi, vb.) tek bir yöntem aşağıda gösterilene benzer uygulanır. Yöntemi, üç ana işlevleri gerçekleştirmeniz gerekir:

1. **Amaç işlem** – Siri tarafından ayrıştırılan verileri içinde kullanılabilir hale getirileceğini bir `intent` hedefi türüne belirli nesne. Uygulamanızı isteğe bağlı kullanarak bu verileri doğrulatma `Resolve*` yöntemleri.
2. **Doğrulama ve veri deposu güncelleştirme** – verilerini (ana iOS uygulamasını da onu erişebilmesi için uygulama grupları kullanarak) dosya sistemi veya bir ağ isteği aracılığıyla kaydedin.
3. **Yanıt sağlamak** – kullanım `completion` işleyici okuma/kullanıcıya görünen için geri Siri yanıt gönder:

```csharp
public void HandleCreateTaskList(INCreateTaskListIntent intent, Action<INCreateTaskListIntentResponse> completion)
{
  var list = TaskList.FromIntent(intent);
  // TODO: have to create the list and tasks... in your app data store
  var response = new INCreateTaskListIntentResponse(INCreateTaskListIntentResponseCode.Success, null)
  {
    CreatedTaskList = list
  };
  completion(response);
}
```

Dikkat `null` geçirilen yanıt – ikinci parametre olarak bu kullanıcı etkinlik parametresi ve onu sağlanmaz, varsayılan değer kullanılır.
İOS uygulamanızı aracılığıyla desteklediği sürece, bir özel etkinlik türü ayarlayabilirsiniz `NSUserActivityTypes` anahtarını **Info.plist**. Sonra uygulamanızı açıldığında, bu durumun üstesinden ve belirli işlemleri (örneğin, ilgili görünüm denetleyiciye açarak ve Siri işlemi verileri yüklenirken) gerçekleştirin.

Bu örnek ayrıca hardcodes `Success` sonucu, ancak gerçek senaryolarda uygun hata raporlama eklenmesi.

### <a name="test-phrases"></a>Test deyimleri

Aşağıdaki sınama tümcecikleri örnek uygulamasında çalışması gerekir:

- "İçinde TasksNotes elmalar, muzlar ve armut Market listesiyle make"
- "Görev WWDC TasksNotes içinde Ekle"
- "Görev WWDC TasksNotes eğitim listesine ekle"
- "İşareti katılın WWDC TasksNotes eksiksiz olarak"
- "İçinde TasksNotes giriş adımına geldiğinizde iphone satın hatırlat"
- "İşareti satın iPhone TasksNotes içinde tamamlandı olarak"
- "TasksNotes 8 sabah giriş bırakmayı hatırlat"

![Yeni bir liste örneği oluşturma](sirikit-images/createtasklist-sml.png) ![Tam bir örnek olarak ayarlanmış görevi](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> 11 iOS simülatörü (önceki sürümlerinden farklı olarak) ile Siri sınama destekler.
>
> Gerçek cihazlarda sınıyorsanız, uygulama Kimliğiniz ve sağlama profilleri SiriKit desteğini yapılandırmak unutmayın.

<a name="alternativenames" />

## <a name="alternative-names"></a>Diğer adlar

Bu yeni iOS 11 özellik, kullanıcıların doğru Siri ile tetiklemek yardımcı olmak, uygulamanız için diğer adlar yapılandırabilirsiniz anlamına gelir. Aşağıdaki anahtarları eklemek **Info.plist** iOS uygulaması proje dosyası:

![Alternatif uygulama adı anahtarları ve değerleri gösteren Info.plist](sirikit-images/alternative-names.png)

Alternatif uygulama adları kümesiyle şu tümceciklerden ayrıca için örnek uygulama çalışır (gerçekten adlı **TasksNotes**):

- "Elmalar, muzlar ve içinde Armut Market listesiyle olun _MonkeyNotes_"
- "Eklemek WWDC içinde görev _MonkeyTodo_"


## <a name="troubleshooting"></a>Sorun giderme

Örnek çalışıyor veya kendi uygulamalarına SiriKit ekleme sırasında karşılaşabileceğiniz bazı hatalar:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Objective-C özel durum oluşturuldu.  Ad: NSInternalInconsistencyException neden: sınıfın kullanımını < INPreferences: 0x60400082ff00 > bir uygulamadan yetkilendirme com.apple.developer.siri gerektirir. Xcode projenizde Siri yeteneğini etkinleştirmek?_

- SiriKit işaretlendiğinden **Entitlements.plist**.
- **Entitlements.plist** yapılandırılan **proje Seçenekleri > Yapı > iOS paket imzalama**.

  [![Yetkilendirmeler doğru olarak ayarlanmış gösteren proje seçenekleri](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (aygıt dağıtımı için) Etkin SiriKit uygulama Kimliğine sahip ve sağlama profili yüklenir.


## <a name="related-links"></a>İlgili bağlantılar

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [TasksNotes SiriKit örnek](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [Yeni içinde SiriKit (WWDC) (video) nedir](https://developer.apple.com/videos/play/wwdc2017/214/)
