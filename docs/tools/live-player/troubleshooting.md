---
title: Sorun giderme
description: Xamarin Canlı Player ve bunları gidermek nasıl bilinen sorunlar.
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: ab075cad0c3f3456ed23f3eb175dcdb3aa493510
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="troubleshooting"></a>Sorun giderme

![Önizleme özelliği](~/media/shared/preview.png)

Bu makalede, bazı yaygın sorunlar açıklanmıştır ve bunları düzeltmek için adımları sağlar.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Mobil cihaz tarama barkod (veya girme kod) sonra bağlanamıyor

Xamarin Canlı Player çalıştıran mobil cihaz IDE çalıştıran bilgisayarın aynı ağ üzerinde olmadığında oluşur. Aşağıdakileri denetleyin:

- Cihaz ve bilgisayar üzerinde aynı Wi-Fi ağı olduğundan emin olun.
  - Bilgisayar da kablolu bir ağa bağlıysa, kablolu bağlantı yönlendiriciyi deneyin.
- Ağ sıkı bir şekilde (bazı şirket ağları gibi), güvenli Xamarin Canlı oynatıcısının gerekli bağlantı noktalarının engellenmesi.
- Xamarin Canlı Player uygulamayı kapatıp yeniden başlatın.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE içinde "dağıtmak çalışılırken hata oluştu" iletisi

**"Ioexception: aktarım bağlantısından veri okunamıyor: engellemeyen yuva işlemi engelle"**

Xamarin Canlı Player çalıştıran mobil cihaz Visual Studio çalıştıran bilgisayarın aynı ağ üzerinde olmadığında bu hata genellikle karşılaştı; Bu durum genellikle daha önce başarıyla eşleştirilmiş bir aygıta bağlanırken oluşur.

* Cihaz ve bilgisayar üzerinde aynı Wi-Fi ağı olduğundan emin olun.
* Ağ sıkı bir şekilde (bazı şirket ağları gibi), güvenli Xamarin Canlı oynatıcısının gerekli bağlantı noktalarının engellenmesi. Aşağıdaki bağlantı noktaları için Xamarin Canlı Player gereklidir:
  * 37847 – iç ağ erişimi 
  * 8090 – dış ağ erişimi

## <a name="manually-configure-device"></a>Cihaz el ile yapılandırma

Cihazınız için Wi-Fi bağlayabilirsiniz değil, el ile yapılandırma dosyası aracılığıyla Cihazınızı aşağıdaki adımlarla yapılandırın deneyebilirsiniz:

**1. adım: yapılandırma dosyasını açın**

Uygulama verileri klasörüne head:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

Bu klasörde, bulacaksınız **PlayerDeviceList.xml** henüz yoksa bir oluşturmanız gerekir.

**2. adım: IP adresi al**

Xamarin Canlı Player uygulamada Git **hakkında > bağlantı testi > bağlantı Testi Başlat**.

Not IP adresi, Cihazınızı yapılandırırken listelenen IP adresi gerekir.

**3. adım: kodu eşleştirme Al**

Xamarin Canlı Player dokunun içinde **çifti** veya **yeniden çifti**, tuşuna basarak **el ile girin**. Sayısal bir kod, yapılandırma dosyasını güncelleştirmek ihtiyaç duyacağı görüntülenir.

**4. adım: GUID oluşturun**

Gidin: https://www.guidgenerator.com/online-guid-generator.aspx ve yeni bir GUID oluşturur ve büyük harf olduğundan emin olun.


**5. adım: cihaz yapılandırma**

Açık **PlayerDeviceList.xml** yukarı Visual Studio veya Visual Studio Code gibi bir düzenleyicide. Cihazınız bu dosyada el ile yapılandırmanız gerekir. Varsayılan olarak, aşağıdaki boş dosya içermelidir `Devices` XML öğesi:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**İOS aygıtı ekleme:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>iPhone Player</Name>
<Platform>iOS</Platform>
<AndroidApiLevel>0</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:36:03.9492291Z</LastConnectTimeUtc>
</PlayerDevice>
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

**Kapatın ve Visual Studio'yu yeniden açın.** Cihazınızı listede göstermelidir.


## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>IDE içinde "türü veya ad alanı bulunamadı" iletisi

Seçtiğiniz onay bir **başlangıç projesi** (iOS veya Android), cihaz türünüz ile eşleşen ve yapılandırma (ör. Bu cihaz türüyle eşleşen **Hata ayıklama | iPhone benzeticisi** iOS için).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Player "Oluşturucusu türündeki 'InterpretedXamarin.Forms.Button' bulunamadı" iletisi

Bazı sistem sınıfları, örneğin değiştirilemiyor:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: 'Resource.Layout', 'Ana' için tanım içermiyor"

Bu hata, AXML dosyalarında tanımlanan kullanıcı arabirimleri ile Android projeleri için oluşur.
AXML dosyaları Xamarin Canlı Player şu anda desteklenmemektedir.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Xamarin.Forms yanlış kullanılması Android araç çubuğu ve sekmeleri işleme

Xamarin.Forms Android projeleri "Toolbar.axml" ve "Tabbar.axml" ilgili düzeni dosyaların adlarını kullanmanız gerekir. Varsayılan şablonu bu adları kullanır; onları yeniden adlandırmayı işleme sorunları neden olur.


Lütfen ek sorunları hakkında rapor [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>İlgili bağlantılar

- [Sınırlamalar](~/tools/live-player/limitations.md)
- [Kurulum](~/tools/live-player/install.md)
