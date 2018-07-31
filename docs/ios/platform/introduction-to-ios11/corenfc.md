---
title: Xamarin.iOS NFC çekirdek
description: Bu belge, yakın alan iletişimi etiketleri iOS 11 ' tanıtılan API'lerini kullanarak Xamarin.iOS okunacak açıklar.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 1381a4564f93fd091f181949454df3f06b31ae6b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350839"
---
# <a name="core-nfc-in-xamarinios"></a>Xamarin.iOS NFC çekirdek

_İOS 11 kullanarak okuma yakın arama alan iletişimi (NFC) etiketleri_

CoreNFC erişim sağlayan bir iOS 11'deki yeni bir çerçeve olan _yakın alan iletişimi_ etiketlerini uygulamaların içinden okunacak radyo (NFC). İPhone 7, üzerinde çalıştığı 7 artı 8, 8 artı ve X.

İOS cihazlarında NFC etiketi Okuyucu tüm NFC etiketi türlerini 1'den içeren 5 destekler _NFC veri Exchange biçimi_ (NDEF) bilgi.

Dikkat edilmesi gereken bazı sınırlamalar vardır:

- CoreNFC (yazma veya biçimlendirme) okuma etiketi yalnızca destekler.
- Etiket taramaları, kullanıcı tarafından başlatılan olmalıdır ve 60 saniye sonra zaman aşımı.
- Uygulamalar için tarama ön planda görünür olmalıdır.
- CoreNFC yalnızca (değil, simülatör) gerçek cihazlarda test edilebilir.

Bu sayfada CoreNFC kullanmak için gerekli yapılandırmayı açıklar ve API kullanımı gösterilmiştir ["TFCTagReader" örnek kod](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Yapılandırma

CoreNFC etkinleştirmek için projenizde üç öğe yapılandırmanız gerekir:

- Bir **Info.plist** gizlilik anahtarı.
- Bir **Entitlements.plist** girişi.
- Bir sağlama profiliyle **NFC etiketi okuma** yeteneği.

### <a name="infoplist"></a>Info.plist

Ekleme **NFCReaderUsageDescription** gizlilik anahtarı ve Tarama devam ederken, kullanıcıya görüntülenen metin. Uygulamanız için uygun bir ileti kullanın (örneğin, tarama amacını açıklayın):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Uygulamanızı istemelisiniz **yakın alan iletişimi etiketi okuma** aşağıdaki anahtar/değer kullanma yeteneği pair içinde **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Sağlama profili

Yeni bir **uygulama kimliği** olduğundan emin olun **NFC etiketi okuma** hizmet işaretlendiğinden:

[![Geliştirici Portalı, yeni uygulama Kimliğini sayfasıyla NFC etiketi seçili okuma](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Ardından, bu uygulama kimliği için yeni bir sağlama profili oluşturma sonra indirmeli ve Mac'te geliştirme yükleyin

## <a name="reading-a-tag"></a>Bir etiketi okuma

Projenizi yapılandırıldıktan sonra ekleme `using CoreNFC;` izleyin ve dosyanın üst kısmına NFC uygulamak için bu üç adımı okuma işlevselliği etiketi:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Uygulama `INFCNdefReaderSessionDelegate`

Arabirim uygulanması için iki yöntem vardır:

- `DidDetect` – Bir etiketi başarıyla okunduğunda çağırılır.
- `DidInvalidate` – Bir hata oluşursa veya 60 ikinci zaman aşımı ulaşıldığında çağrılır.

#### <a name="diddetect"></a>DidDetect

Örnek kodda, taranan her ileti bir tablo görünümüne eklenir:

```csharp
public void DidDetect(NFCNdefReaderSession session, NFCNdefMessage[] messages)
{
    foreach (NFCNdefMessage msg in messages)
    {  // adds the messages to a list view
        DetectedMessages.Add(msg);
    }
    DispatchQueue.MainQueue.DispatchAsync(() =>
    {
        this.TableView.ReloadData();
    });
}
```

Bu yöntem, birden çok kez çağrılabilir (ve iletiler dizisi olarak geçirilebilir) oturumu için birden çok etiket okuma izin veriyorsa. Bu üçüncü parametresi kullanılarak yapılır `Start` yöntemi (açıklandığı [2. adım](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Geçersiz kılma bir dizi nedenden ötürü ortaya çıkabilir:

- Tarama sırasında bir hata oluştu.
- Uygulama ön planda olmasını ceased.
- Tarama iptal etmek kullanıcının seçtiği.
- Tarama, uygulama tarafından iptal edildi.

Aşağıdaki kod bir hata nasıl ele alınacağını gösterir:

```csharp
public void DidInvalidate(NFCNdefReaderSession session, NSError error)
{
    var readerError = (NFCReaderError)(long)error.Code;
    if (readerError != NFCReaderError.ReaderSessionInvalidationErrorFirstNDEFTagRead &&
        readerError != NFCReaderError.ReaderSessionInvalidationErrorUserCanceled)
    {
      // some error handling
    }
}
```

Bir oturumu geçersiz hale getirildi sonra yeni bir oturum nesnesi taramayı yeniden oluşturulması gerekir.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Başlangıç bir `NFCNdefReaderSession`

Tarama düğmesine basma gibi bir kullanıcı isteği ile başlamanız gerekir.
Aşağıdaki kod oluşturur ve bir tarama oturumu başlatır:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Parametreler için `NFCNdefReaderSession` Oluşturucu aşağıdaki gibidir:

- `delegate` – Bir uygulaması `INFCNdefReaderSessionDelegate`. Örnek kodda, temsilci ve Tablo görünümü denetleyicisi bu nedenle uygulanan `this` temsilci parametresi olarak kullanılır.
- `queue` – Geri çağırmaları üzerinde işlenen kuyruk. Bu olabilir `null`, bu durumda olması kullandığınızdan emin `DispatchQueue.MainQueue` kullanıcı arabirimi denetimleri (aşağıdaki örnekte gösterildiği gibi) güncelleştirilirken.
- `invalidateAfterFirstRead` – Ne zaman `true`, ilk başarılı taramadan sonra; taramayı durdurur, `false` tarama işlemi devam edecek ve Tarama iptal edildi veya zaman aşımını 60 ikinci ulaşılana kadar çok sayıda sonuç döndürdü.


### <a name="3-cancel-the-scanning-session"></a>3. Tarama oturumu iptal et

Kullanıcı, kullanıcı arabiriminde bir sistem tarafından sağlanan düğmesiyle tarama oturumu iptal edebilirsiniz:

![Tarama sırasında iptal düğmesi](corenfc-images/scan-cancel-sml.png)

Uygulamayı programlı olarak tarama çağırarak iptal edebilirsiniz `InvalidateSession` yöntemi:

```csharp
Session.InvalidateSession();
```

Her iki durumda da temsilci 's `DidInvalidate` yöntemi çağrılır.

## <a name="summary"></a>Özet

CoreNFC NFC etiketlerini veri okuma olanak tanır. Etiket biçimleri (NDEF türleri: 1-5) çeşitli okumayı destekler, ancak yazma veya biçimlendirme desteklemez.


## <a name="related-links"></a>İlgili bağlantılar

- [NFCTagReader (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [NFC (WWDC çekirdek) (video) ile tanışın](https://developer.apple.com/videos/play/wwdc2017/718/)
