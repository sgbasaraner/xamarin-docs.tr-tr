---
title: Çekirdek NFC
description: İOS 11 kullanılarak okuma yakın arama alan iletişimi (NFC) etiketleri
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2016
ms.openlocfilehash: db0c417c1c79a988821a93b1cf20d94161f5c47a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="core-nfc"></a>Çekirdek NFC

_İOS 11 kullanılarak okuma yakın arama alan iletişimi (NFC) etiketleri_

CoreNFC olan yeni bir erişim sağlayan bir iOS 11 framework _yakın alan iletişimi_ (NFC) radyo uygulamaların içindeki etiketlerini okuyun. İPhone 7, üzerinde çalıştığı 7 artı, 8, artı ve X 8.

Tüm NFC etiket türleri 1 içeren 5 ile iOS cihazları NFC etiketi okuyucusunda destekler _NFC veri değişimi biçimi_ (NDEF) bilgi.

Dikkat edilmesi gereken bazı sınırlamalar vardır:

- CoreNFC (yazma veya biçimlendirme) okuma etiketi yalnızca destekler.
- Etiket taramaları, kullanıcı tarafından başlatılan, olmalıdır ve 60 saniye sonra zaman aşımı.
- Uygulamalar taramak için ön planda görünür olması gerekir.
- CoreNFC (değil, simulator) gerçek cihazlarda yalnızca test edilebilir.

Bu sayfa CoreNFC kullanmak için gerekli yapılandırmayı açıklar ve API kullanımı gösterilmiştir ["TFCTagReader" örnek koduna](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Yapılandırma

CoreNFC etkinleştirmek için üç öğe projenizde yapılandırmanız gerekir:

- Bir **Info.plist** gizlilik anahtarı.
- Bir **Entitlements.plist** girişi.
- Bir sağlama profiliyle **NFC etiketi okuma** yeteneği.

### <a name="infoplist"></a>Info.plist

Ekleme **NFCReaderUsageDescription** gizlilik anahtarı ve tarama gerçekleştirildiği sırada hangi kullanıcılara gösterilecek metin. Uygulamanız için uygun bir ileti kullanın (örneğin, tarama amacını açıklayan):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Uygulamanızı istemelidir **yakın alan iletişimi etiketi okuma** aşağıdaki anahtar/değer kullanarak özelliğine eşleştirin, **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Sağlama profili

Yeni bir **uygulama kimliği** ve emin **NFC etiketi okuma** hizmet işaretlendiğinden:

[![Geliştirici Portalı yeni uygulama kimliği sayfasıyla NFC etiketi seçili okuma](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Ardından, bu uygulama kimliği için yeni bir sağlama profili oluşturun ardından indirmeli ve geliştirme Mac üzerinde yükleme

## <a name="reading-a-tag"></a>Bir etiketi okuma

Projenizi yapılandırıldıktan sonra add `using CoreNFC;` okuma işlevselliği NFC uygulamak için bu üç adımı izleyin ve dosyasının üst kısmına etiketi:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Uygulama `INFCNdefReaderSessionDelegate`

Arabirim uygulanması için iki yöntem vardır:

- `DidDetect` – Bir etiket başarıyla okunduğunda çağrılır.
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

Bu yöntem birden çok kez çağrılabilir (ve iletileri bir dizi geçirilen) oturumu için birden çok etiket okuma izin veriyorsa. Bu üçüncü parametrenin kullanılarak yapılır `Start` yöntemi (açıklandığı [2. adım](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Geçersiz kılma birçok nedenden dolayı ortaya çıkabilir:

- Tarama sırasında bir hata oluştu.
- Uygulama ön planda olmasını ceased.
- Kullanıcı tarama iptal etmeyi seçtiniz.
- Tarama uygulama tarafından iptal edildi.

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

Bir oturum geçersiz kıldı sonra yeni bir oturum nesnesi taramayı yeniden oluşturulması gerekir.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Başlangıç bir `NFCNdefReaderSession`

Tarama Düğme basma gibi bir kullanıcı isteği ile başlamanız gerekir.
Aşağıdaki kod oluşturur ve bir tarama oturumu başlatır:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Parametrelerini `NFCNdefReaderSession` oluşturucusu aşağıdaki gibidir:

- `delegate` – Bir uygulaması, `INFCNdefReaderSessionDelegate`. Örnek kodda temsilci Tablo görünümü denetleyicisi bu nedenle uygulanan `this` temsilci parametre olarak kullanılır.
- `queue` – Geri aramalar üzerinde işlenir sırası. Bu olabilir `null`, bu durumda olması kullandığınızdan emin `DispatchQueue.MainQueue` kullanıcı arabirimi denetimlerini (aşağıdaki örnekte gösterildiği gibi) güncelleştirirken.
- `invalidateAfterFirstRead` – Ne zaman `true`, ilk başarılı taramadan sonra; tarama durdurur zaman `false` tarama işlemi devam eder ve Tarama iptal edildi veya 60 ikinci zaman aşımı ulaşılana kadar çok sayıda sonuç döndürdü.


### <a name="3-cancel-the-scanning-session"></a>3. Tarama oturum iptal et

Kullanıcı, kullanıcı arabiriminde bir sistem tarafından sağlanan düğmesiyle tarama oturum iptal edebilirsiniz:

![Tarama sırasında iptal düğmesi](corenfc-images/scan-cancel-sml.png)

Uygulama program aracılığıyla tarama çağırarak iptal edebilirsiniz `InvalidateSession` yöntemi:

```csharp
Session.InvalidateSession();
```

Her iki durumda da temsilci 's `DidInvalidate` yöntemi çağrılır.

## <a name="summary"></a>Özet

CoreNFC NFC etiketleri verileri okumak uygulamanızı sağlar. Etiket biçimlerini (NDEF türleri 1 ile 5) çeşitli okumayı destekler, ancak yazma veya biçimlendirme desteklemez.


## <a name="related-links"></a>İlgili bağlantılar

- [NFCTagReader (sample)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Giriş çekirdek NFC (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/718/)
