---
title: Xamarin Live Player sorunlarını giderme
description: Bu belgede, olası düzeltmeleri ve Xamarin Live Player ile ilgili bilinen sorunlar açıklanmaktadır. Bağlantı sorunları, yapılandırma sorunlarını ve diğer ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: topgenorth
ms.author: toopge
ms.date: 08/08/2018
ms.openlocfilehash: ceb8964ac378957dcf5883bbbfff9e984b079294
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780617"
---
# <a name="troubleshooting-xamarin-live-player"></a>Xamarin Live Player sorunlarını giderme

![Önizleme özelliği](~/media/shared/preview.png)

Bu makalede, bazı yaygın sorunlar açıklanır ve bunları düzeltmeye yönelik adımları sağlar.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Mobil cihaz barkod tarama (veya girme kod) sonra bağlanamıyor

Xamarin Live Player'ı çalıştıran mobil cihaz IDE'ı çalıştıran bilgisayarın aynı ağ üzerinde olmadığında gerçekleşir. Aşağıdakileri denetleyin:

- Aynı Wi-Fi ağında cihaz ve bilgisayar olduğunu onaylayın.
  - Bilgisayarın ayrıca kablolu bir ağa bağlıysa, kablolu bağlantı modemi çıkarmayı deneyin.
- Ağ (bazı şirket ağları gibi), sıkı bir şekilde korunması Xamarin Live Player'ın gerekli bağlantı noktalarını engelleme.
- Xamarin Live Player uygulamasını kapatıp yeniden başlatın.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE'de "dağıtılmaya çalışılırken hatayla" iletisi

**"Ioexception: taşıma bağlantısından veriler okunamıyor: yuvada engelleyici olmayan işlem engellendi"**

Xamarin Live Player'ı çalıştıran mobil cihaz olarak Visual Studio çalıştıran bilgisayarın aynı ağ üzerinde olmadığında bu hata genellikle karşılaştı; Bu, genellikle daha önce başarıyla eşleştirilmiş bir cihaza bağlanırken gerçekleşir.

* Hem cihaz hem de bilgisayar aynı Wi-Fi ağında olup olmadığını denetleyin.
* Ağ (bazı şirket ağları gibi), sıkı bir şekilde korunması Xamarin Live Player'ın gerekli bağlantı noktalarını engelleme. Aşağıdaki bağlantı noktaları için Xamarin Live Player gereklidir:
  * 37847 – iç ağ erişimi 
  * 8090 – dış ağ erişimi

## <a name="manually-configure-device"></a>Cihaz el ile yapılandırma

Cihazınıza Wi-Fi bağlanabilirsiniz değil, Cihazınızı yapılandırma dosyası aracılığıyla aşağıdaki adımlarla el ile yapılandırmak deneyebilirsiniz:

**1. adım: yapılandırma dosyasını açın**

Uygulama veri klasörüne gidin:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

Bu klasörde bulursunuz **PlayerDeviceList.xml** henüz yoksa bir oluşturmanız gerekir.

**2. adım: IP adresi alın**

Xamarin Live Player uygulamasında Git **hakkında > Bağlantı sınama > bağlantı Testi Başlat**.

Not, IP adresi listelenir Cihazınızı yapılandırdığınız IP adresi gerekir.

**3. adım: kodu eşleştirme Al**

Xamarin Live Player dokunun içinde **çifti** veya **çifti yeniden**, tuşuna **el ile girin**. Sayısal bir kod, yapılandırma dosyasını güncelleştirmek gereken görüntülenir.

**4. adım: GUID oluştur**

Git: https://www.guidgenerator.com/online-guid-generator.aspx yeni bir GUID oluşturun ve büyük harf olduğundan emin olun.

**5. adım: cihaz yapılandırma**

Açık yukarı **PlayerDeviceList.xml** yukarı Visual Studio veya Visual Studio Code gibi bir düzenleyicide. Cihazınızı bu dosyayı el ile yapılandırmanız gerekiyor. Varsayılan olarak, aşağıdaki boş dosya içermelidir `Devices` XML öğesi:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Android cihaz ekleyin:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Kapatın ve Visual Studio'da yeniden açın.** Cihazınızı listede gösterilmesi gerekir.

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>IDE'de "tür veya ad alanı bulunamadı" iletisi

Seçtiğiniz onay bir **başlangıç projesi** (örn. cihaz türünüzle eşleşen Android) ve yapılandırma, cihaz türü (örn.) ile eşleşir **Hata ayıklama** Android için).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Player'da "Konstruktor typu nebyl nalezen ' InterpretedXamarin.Forms.Button'" iletisini

Bazı sistem sınıfları, örneğin geçersiz kılınamaz:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: 'Resource.Layout' bir 'Main' tanımı içermiyor"

Bu hata, AXML dosyalarında tanımlanan kullanıcı arabirimleri ile Android projeleri için oluşur.
Xamarin Live Player'da AXML dosyaları şu anda desteklenmemektedir.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android araç ve sekmeler yanlış Xamarin.Forms kullanarak oluşturma

Xamarin.Forms Android projeleri "Toolbar.axml" ve "Tabbar.axml" ilgili Düzen dosyaların adlarını kullanmanız gerekir. Varsayılan şablonu bu adları kullanır; adlandırarak oluşturma sorunlarına neden olur.

Ek sorunları raporlamak lütfen [bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>İlgili bağlantılar

- [Sınırlamalar](~/tools/live-player/limitations.md)
- [Kurulum](~/tools/live-player/install.md)
