---
title: "iOS güvenlik ve gizlilik özellikleri"
description: "Bu makalede, güvenlik ve gizlilik iOS ve bir Xamarin.iOS uygulaması nasıl etkilediklerini ile çalışma yer almaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5e4bbc22403c6c0bfa5c8dc7ac4e3a39545051d4
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="ios-security-and-privacy-features"></a>iOS güvenlik ve gizlilik özellikleri

_Bu makalede, güvenlik ve gizlilik iOS ve bir Xamarin.iOS uygulaması nasıl etkilediklerini ile çalışma yer almaktadır._

Apple geliştirici uygulamalarını güvenliğini artırmak ve son kullanıcının gizliliği güvence altına yardımcı olacak bazı geliştirmeler güvenlik ve gizlilik iOS 10 (ve büyük) kullanıma sunmuştur. Bu makalede, bir Xamarin.iOS uygulaması bu özellikleri uygulama ele alınacaktır.

Aşağıdaki konularda ayrıntılı olarak ele alınacaktır:

- [Genel geliştirmeler](#General-Enhancements)
- [Özel kullanıcı verilerine erişme](#Accessing-Private-User-Data)
    - [Gizlilik anahtarları ayarlama](#Setting-Privacy-Keys)
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Genel geliştirmeler

Aşağıdaki genel güvenlik ve gizlilik için iOS 10 yapılan değişiklikler:

- Ortak veri güvenlik mimarisi (CDSA) API, kullanım dışı bırakıldı ve asimetrik anahtarları oluşturmak için SecKey API'si ile değiştirilmelidir.
- Yeni `NSAllowsArbitraryLoadsInWebContent` anahtar eklemeniz bir uygulamanın `Info.plist` dosya ve Apple Aktarım güvenlik (ATS) koruma uygulama geri kalanı için hala etkin durumdayken doğru bir şekilde yüklemek için web sayfaları izin verir. Daha fazla bilgi için lütfen bkz bizim [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md) belgeleri.
- Yeni Pano iOS 10 ve macOS Sierra cihazlar arasında kopyalayıp kullanıcıya izin verdiğinden, API için belirli bir aygıt sınırlı ve belirli bir anda otomatik olarak temizlenecek zaman damgalı bir Pano izin verecek şekilde genişletilmiştir. Ayrıca, adlandırılmış pasteboards artık kalıcı ve paylaşılan çalışma alanı kapsayıcılarını ile değiştirilmelidir.
- Tüm SSL/TLS bağlantılarını RC4 simetrik şifre artık varsayılan olarak devre dışıdır. Ayrıca, güvenli aktarım API artık SSLv3 destekler ve geliştirici SHA-1 ve 3DES şifreleme mümkün olan en kısa sürede kullanmak durdurmanız önerilir.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Özel kullanıcı verilerine erişme

Uygulamalar iOS 10 (veya sonraki sürümler) çalıştıran bir veya daha fazla gizlilik anahtarlarında girerek belirli özellikler veya kullanıcı bilgilerini erişmek için kendi hedefi statik olarak bildirmelidir kendi `Info.plist` neden uygulamanın istediği erişmek için kullanıcı açıklayan dosyaları.

> [!IMPORTANT]
> Gerekli anahtarlarla sessizce sonlandırılacak sistem tarafından kısıtlı özellikleri veya kullanıcı bilgilerini birini erişmeye çalıştığında sağlamak için başarısız uygulamaları _hatasız_! Bir uygulama 10 İos'ta beklenmedik şekilde başarısız başlarsa, tüm gereken, olun `Info.plist` belirtilmedi.

Aşağıdaki gizlilik anahtarların ilgili:

- **Gizlilik - Apple müzik kullanım açıklama** (`NSAppleMusicUsageDescription`)-uygulama neden kullanıcının Ortam Kitaplığı erişmek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - Bluetooth Çevre kullanım açıklama** (`NSBluetoothPeripheralUsageDescription`)-uygulama neden kullanıcının cihazda Bluetooth erişmek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - takvimleri kullanım açıklama** (`NSCalendarsUsageDescription`)-uygulama neden kullanıcının Takvim erişmek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - kamera kullanımını açıklama** (`NSCameraUsageDescription`)-uygulama neden cihazın kamera erişmek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - kişiler kullanım açıklama** (`NSContactsUsageDescription`)-uygulama neden kullanıcının kişilerini erişmek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - sistem durumu paylaşımı kullanım açıklama** (`NSHealthShareUsageDescription`)-neden kullanıcının sistem durumu verilerine erişmek uygulamanın istediği açıklamak Geliştirici sağlar. Daha fazla bilgi için lütfen Apple'nın bkz [HKHealthStore sınıf başvurusu](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Gizlilik - sistem durumu güncelleştirme kullanım açıklama** (`NSHealthUpdateUsageDescription`)-neden kullanıcı durumu verilerinin düzenlemek uygulamanın istediği açıklamak Geliştirici sağlar. Daha fazla bilgi için lütfen Apple'nın bkz [HKHealthStore sınıf başvurusu](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Gizlilik - HomeKit kullanım açıklama** (`NSHomeKitUsageDescription`)-neden kullanıcının HomeKit yapılandırma verilerine erişmek uygulamanın istediği açıklamak Geliştirici sağlar.
- **Gizlilik - konum her zaman kullanım açıklaması** (`NSLocationAlwaysUsageDescription`)-neden her zaman kullanıcının konuma erişimi olması uygulamanın istediği açıklamak Geliştirici sağlar.
- [Kullanım dışı] **Gizlilik - konum kullanım açıklaması** (`NSLocationUsageDescription`)-neden kullanıcının konuma erişmek üzere uygulamanın istediği açıklamak Geliştirici sağlar. *Not: Bu anahtar iOS 8 (ve büyük) kullanım dışıdır. Kullanım `NSLocationAlwaysUsageDescription` veya `NSLocationWhenInUseUsageDescription` yerine.*
- **Gizlilik - konum zaman içinde kullanım kullanım açıklaması** (`NSLocationWhenInUseUsageDescription`)-neden çalışırken kullanıcının konuma erişmek üzere uygulamanın istediği açıklamak Geliştirici sağlar.
- [Kullanım dışı] **Gizlilik - ortam kitaplığı kullanım açıklaması** -neden kullanıcının Ortam Kitaplığı'na erişmek uygulamanın istediği açıklamak Geliştirici sağlar. *Not: Bu anahtar iOS 8 (ve büyük) kullanım dışıdır. Kullanım `NSAppleMusicUsageDescription` yerine.*
- **Gizlilik - mikrofon kullanım açıklama** (`NSMicrophoneUsageDescription`)-uygulama neden aygıtları mikrofon erişmek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - hareket kullanım açıklama** (`NSMotionUsageDescription`)-uygulama neden cihazın accelerometer erişmek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - Fotoğraf Kitaplığı kullanım açıklaması** (`NSPhotoLibraryUsageDescription`)-uygulama neden kullanıcının Fotoğraf Kitaplığı erişmek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - anımsatıcıları kullanım açıklama** (`NSRemindersUsageDescription`)-uygulama neden kullanıcının anımsatıcıları erişmek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - Siri kullanım açıklama** (`NSSiriUsageDescription`)-uygulama neden Siri kullanıcı verileri göndermek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - konuşma tanıma kullanım açıklama** (`NSSpeechRecognitionUsageDescription`)-uygulama neden Apple'nın konuşma tanıma sunucularına kullanıcı verileri göndermek istiyor açıklamak Geliştirici sağlar.
- **Gizlilik - TV sağlayıcı kullanım açıklaması** (`NSVideoSubscriberAccountUsageDescription`)-neden kullanıcının TV sağlayıcısı hesabına erişmek uygulamanın istediği açıklamak Geliştirici sağlar.

İle çalışma hakkında daha fazla bilgi için `Info.plist` anahtarları lütfen bkz. Apple'nın [bilgi özellik listesi anahtar başvurusu](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Gizlilik anahtarları ayarlama

İOS 10 (ve büyük) HomeKit erişme aşağıdaki örnek almak, geliştirici eklemeniz gerekecektir `NSHomeKitUsageDescription` uygulamanın anahtar `Info.plist` dosya ve uygulama neden isteyen kullanıcının HomeKit veritabanına erişmek için bildirme bir dize sağlayın. Bu dize, kullanıcılarınız uygulamayı çalıştırmadan kullanıcı ilk zamanı sunulur:

[![](security-privacy-images/info01.png "Bir örnek NSHomeKitUsageDescription uyarı")](security-privacy-images/info01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio geçerli Xamarin.iOS güvenlik geliştirmesi düzenleme desteklemiyor `Info.plist` aşağıdaki geçici çözüm gerekli olacak şekilde IDE içinden ayarlarını:

1. Açık `Info.plist` harici bir metin düzenleyicisine dosyasında.
2. Son önce `</dict>` düğümü, aşağıdaki düğüm ekleyin: `<key>NSHomeKitUsageDescription</key>`
3. Gerekli açıklama sağlamak için aşağıdaki düğüm ekleyin: `<string>Allows the app to control HomeKit enabled devices.</string>`
4. `Info.plist` Dosya, aşağıdaki gibi görünmelidir: 

    [![](security-privacy-images/info02vs.png "Info.plist dosyasını aşağıdaki gibi görünmelidir.")](security-privacy-images/info02vs.png#lightbox)
4. Değişiklikleri dosyaya kaydedin.
5. Visual Studio'ya geri dönün ve uygulamayı yeniden derleyebilirsiniz kullanıcının.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Herhangi bir gizlilik anahtarı ayarlamak için aşağıdakileri yapın:

1. Çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. Ekranın alt kısmındaki geçmek **kaynak** görünümü.
3. Yeni bir ekleme **girişi** listesi.
4. Gizlilik anahtarı açılır listeden seçin (gibi **gizlilik - HomeKit kullanım açıklama**): 

    [![](security-privacy-images/info02.png "Gizlilik anahtarı seçin")](security-privacy-images/info02.png#lightbox)
5. Uygulama neden belirli özellik ya da kullanıcı bilgileri erişmek istiyor için bir açıklama girin: 

    [![](security-privacy-images/info03.png "Bir açıklama girin")](security-privacy-images/info03.png#lightbox)
6. Değişiklikleri dosyaya kaydedin.

-----

> [!IMPORTANT]
> Örnekte ayarlanamadı, yukarıda verilen `NSHomeKitUsageDescription` anahtarını `Info.plist` dosya uygulamada sonuçlanacak _sessizce sorunlu_ (çalışma zamanında sistem tarafından kapatılan) iOS 10 (veya daha büyük) çalıştırdığınızda hatasız.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede güvenlik ve gizlilik değişiklikler Apple iOS 10 ve bir Xamarin.iOS uygulaması nasıl etkilediklerini yaptı kapsamına.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
