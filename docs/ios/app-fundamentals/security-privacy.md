---
title: iOS güvenlik ve gizlilik özellikleri
description: Bu belge, güvenlik ve gizlilik özellikleri iOS açıklar ve bunların Xamarin.iOS ile nasıl kullanılacağı açıklanır. İOS 10 ve özel kullanıcı verilerinin nasıl yapılan güncelleştirmeler göz sürer.
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 1a28bf394d29c09bfd264f03e0eea6c8b582f271
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854759"
---
# <a name="ios-security-and-privacy-features"></a>iOS güvenlik ve gizlilik özellikleri

_Bu makale, güvenlik ve gizlilik iOS ve bir Xamarin.iOS uygulaması etkilemesi çalışma kapsar._

Apple geliştirici uygulamalarını güvenliğini artırmak ve son kullanıcının gizlilik olun yardımcı olacak bazı geliştirmeler güvenlik ve gizlilik iOS 10 (ve büyük) kullanıma sunmuştur. Bu makalede, bir Xamarin.iOS uygulaması bu özelliklerini uygulama ele alınacaktır.
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Genel iyileştirmeler

Aşağıdaki genel değişiklikler iOS 10 Güvenlik ve gizlilik için yapılmıştır:

- Ortak veri güvenlik mimarisi (CDSA) API kullanım dışıdır ve SecKey asimetrik anahtarlar oluşturmak için API ile değiştirilmelidir.
- Yeni `NSAllowsArbitraryLoadsInWebContent` anahtarı eklemeniz uygulamanın **Info.plist** dosya ve uygulama geri kalanı için Apple taşıma güvenliği (ATS) koruması hala etkinken doğru bir şekilde yüklemek web sayfaları izin verir. Daha fazla bilgi için lütfen bkz. bizim [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md) belgeleri.
- Yeni Pano iOS 10 ve macOS Sierra cihazlar arasında kopyalayıp kullanıcıya izin verdiğinden, API, belirli bir cihaz için sınırlı ve belirli bir anda otomatik olarak temizlenecek zaman damgalı bir Pano izin verecek şekilde genişletildi. Ayrıca, adlandırılmış pasteboards artık kalıcı ve paylaşılan çalışma alanı kapsayıcılar ile değiştirilmelidir.
- Tüm SSL/TLS bağlantılarını için RC4 simetrik şifre artık varsayılan olarak devre dışıdır. Ayrıca, güvenli aktarım API artık SSLv3'ü destekler ve SHA-1 ve 3DES şifreleme olabildiğince çabuk kullanarak Geliştirici durdurmanız önerilir.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Özel kullanıcı verilerine erişme

İOS 10 (veya üzeri) üzerinde çalışan uygulamalar, bunların amacı, bir veya daha fazla gizlilik anahtarlarında girerek belirli özellikler veya kullanıcı bilgilerini erişmek için statik olarak bildirmelidir kendi **Info.plist** elde etmek için uygulamayı neden isteyen kullanıcının açıklayan dosyaları erişim.

> [!IMPORTANT]
> Gerekli anahtarlarla sessizce sonlandırılacak sistem tarafından bir kısıtlı özellikleri veya kullanıcı bilgilerini erişmeye çalıştığında sağlayamamaktadır uygulamaları _hatasız_! Bir uygulamayı iOS 10 beklenmedik şekilde hata başlarsa, gerekli tüm emin **Info.plist** belirtilmedi.

Aşağıdaki gizlilik anahtarlar kullanılabilir ilgili:

- **Gizlilik - Apple Music kullanım açıklaması** (`NSAppleMusicUsageDescription`)-neden uygulamayı kullanıcının ortam kitaplık erişmek isteyen tanımlamak geliştiricinin sağlar.
- **Gizlilik - Bluetooth Çevre Birimi kullanım açıklaması** (`NSBluetoothPeripheralUsageDescription`)-neden uygulamayı kullanıcının cihazda Bluetooth erişmek isteyen tanımlamak geliştiricinin sağlar.
- **Gizlilik - takvimler kullanım açıklaması** (`NSCalendarsUsageDescription`)-neden uygulamayı kullanıcının Takvim erişmek isteyen tanımlamak geliştiricinin sağlar.
- **Gizlilik - kamera kullanım açıklaması** (`NSCameraUsageDescription`)-neden uygulamayı cihazın kamerasını erişmek isteyen tanımlamak geliştiricinin sağlar.
- **Gizlilik - kişiler kullanım açıklaması** (`NSContactsUsageDescription`)-neden uygulamayı kullanıcının kişilerini erişmek isteyen tanımlamak geliştiricinin sağlar.
- **Gizlilik - Health Share kullanım açıklaması** (`NSHealthShareUsageDescription`)-neden uygulama kullanıcı durumu verilerinin erişmek isteyen tanımlamak geliştiricinin sağlar. Daha fazla bilgi için lütfen Apple'nın bakın [HKHealthStore sınıf başvurusu](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Gizlilik - Health Update kullanım açıklaması** (`NSHealthUpdateUsageDescription`)-neden kullanıcı durumu verilerinin düzenlemek uygulamanın istediği tanımlamak geliştiricinin sağlar. Daha fazla bilgi için lütfen Apple'nın bakın [HKHealthStore sınıf başvurusu](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Gizlilik - HomeKit kullanım açıklaması** (`NSHomeKitUsageDescription`)-uygulama neden kullanıcının HomeKit yapılandırma verilerine erişmek istiyor tanımlamak geliştiricinin sağlar.
- **Gizlilik - konum Always kullanım açıklaması** (`NSLocationAlwaysUsageDescription`)-neden her zaman kullanıcının konuma erişimi olması uygulamanın istediği tanımlamak geliştiricinin sağlar.
- [Kullanım dışı] **Gizlilik - konum kullanım açıklaması** (`NSLocationUsageDescription`)-neden kullanıcının konuma erişmek üzere uygulamanın istediği tanımlamak geliştiricinin sağlar. *Not: Bu anahtar iOS 8 (ve büyük) kullanım dışıdır. Kullanım `NSLocationAlwaysUsageDescription` veya `NSLocationWhenInUseUsageDescription` yerine.*
- **Gizlilik - konum zaman içinde kullanımı açıklaması** (`NSLocationWhenInUseUsageDescription`)-uygulama çalışırken, kullanıcının konumuna erişmek isteyen neden tanımlamak geliştiricinin sağlar.
- [Kullanım dışı] **Gizlilik - medya kitaplığı kullanım açıklaması** -neden uygulamayı kullanıcının ortam kitaplığına erişmek isteyen tanımlamak geliştiricinin sağlar. *Not: Bu anahtar iOS 8 (ve büyük) kullanım dışıdır. Kullanım `NSAppleMusicUsageDescription` yerine.*
- **Gizlilik - mikrofon kullanım açıklaması** (`NSMicrophoneUsageDescription`)-neden uygulama cihazları mikrofon erişmek isteyen tanımlamak geliştiricinin sağlar.
- **Gizlilik - hareket kullanım açıklaması** (`NSMotionUsageDescription`)-neden uygulamayı cihazın ivme ölçer erişmek isteyen tanımlamak geliştiricinin sağlar.
- **Gizlilik - Fotoğraf Kitaplığı kullanım açıklaması** (`NSPhotoLibraryUsageDescription`)-neden kullanıcının fotoğraf kitaplığınıza erişmesine izin uygulama istediği tanımlamak geliştiricinin sağlar.
- **Gizlilik - anımsatıcılar kullanım açıklaması** (`NSRemindersUsageDescription`)-neden uygulamayı kullanıcının anımsatıcılar erişmek isteyen tanımlamak geliştiricinin sağlar.
- **Gizlilik - Siri kullanım açıklaması** (`NSSiriUsageDescription`)-uygulama neden Siri için kullanıcı verileri göndermek istiyor tanımlamak geliştiricinin sağlar.
- **Gizlilik - konuşma tanıma kullanım açıklaması** (`NSSpeechRecognitionUsageDescription`)-uygulama neden kullanıcı verilerini Apple'nın konuşma tanıma sunucularına göndermek istiyor tanımlamak geliştiricinin sağlar.
- **Gizlilik - TV sağlayıcısı kullanım açıklaması** (`NSVideoSubscriberAccountUsageDescription`)-neden uygulamayı kullanıcının TV sağlayıcısı hesabına erişmek isteyen tanımlamak geliştiricinin sağlar.

İle çalışma hakkında daha fazla bilgi için **Info.plist** , lütfen bkz. Apple'nın [bilgi özelliği liste anahtar başvurusu](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Gizlilik anahtarlar ayarlama

İOS 10 (ve büyük) HomeKit erişme aşağıdaki örnek, geliştirici eklemeniz gerekecektir `NSHomeKitUsageDescription` uygulamanın anahtarını **Info.plist** dosya ve uygulama istediği kullanıcının HomeKit erişmek için neden bildirme bir dize sağlayın Veritabanı. Bu dize, kullanıcılarınız uygulamayı çalıştırmadan kullanıcı ilk saat için sunulur:

![Bir örnek NSHomeKitUsageDescription uyarı](security-privacy-images/info01.png "örnek NSHomeKitUsageDescription uyarı")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS için Visual Studio şu anda düzenleme desteklemiyor **Info.plist** gizlilik anahtarları içinde varsayılan iOS bildirim Düzenleyicisi. Bunun yerine genel PList Düzenleyicisi kullanmak ihtiyacınız olacak şekilde aşağıdakileri yapın:

1. Sağ **Info.plist** dosyası **Çözüm Gezgini** seçip **birlikte Aç...** .
2. Seçin **genel PList Düzenleyicisi** dosyayı açmak için program listesinden ardından **Tamam**.

    ![Genel PList Düzenleyicisi](security-privacy-images/InfoEditorSelectionVs.png "genel PList Düzenleyicisi'ni seçin")
3. Tıklayın **+** son satıra yeni bir giriş listesine eklemek için düzenleyici düğmesine. Bu "özel", türü özelliğine sahip ayarlamak çağrılacak `String` ve boş bir değer.
4. Özellik adına tıklayın ve açılan bir menüde görünecektir.
5. Gizlilik anahtarı açılır listeden seçin (gibi **gizlilik - HomeKit kullanım açıklaması**): 

    ![Gizlilik anahtarı seçmek](security-privacy-images/InfoPListEditorSelectKey.png "gizlilik anahtarı seçin")
6. Bir açıklama neden uygulama belirli bir özellik ya da kullanıcı bilgileri erişmek isteyen için değer sütununa girin: 

    ![Bir açıklama girin](security-privacy-images/InfoPListSetValue.png "bir açıklama girin")
7. Değişiklikleri dosyaya kaydedin.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Herhangi bir gizlilik anahtarları ayarlamak için aşağıdakileri yapın:

1. Çift **Info.plist** dosyası **Çözüm Gezgini** düzenlemek üzere açın.
2. Ekranın alt kısmında, geçiş **kaynak** görünümü.
3. Yeni bir **giriş** listesi.
4. Gizlilik anahtarı açılır listeden seçin (gibi **gizlilik - HomeKit kullanım açıklaması**): 

    ![Gizlilik anahtarı seçmek](security-privacy-images/info02.png "gizlilik anahtarı seçin")
5. Neden belirli özellik ya da kullanıcı bilgilerine erişmek uygulamayı istediği için bir açıklama girin: 

    ![Bir açıklama girin](security-privacy-images/info03.png "bir açıklama girin")
6. Değişiklikleri dosyaya kaydedin.

-----

> [!IMPORTANT]
> Ayarlamak için hata, yukarıdaki örnekte verilen `NSHomeKitUsageDescription` anahtarını **Info.plist** dosya uygulamada sonuçlanır _sessizce başarısız olan_ (çalışma zamanı sistemi tarafından kapatılan) iOS 10 (çalıştırdığınızda hata olmadan veya üzeri).

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede güvenlik ve gizlilik değişiklikleri Apple iOS 10 ve bir Xamarin.iOS uygulaması etkilemesi hale getirdiği kapsamına.

## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
