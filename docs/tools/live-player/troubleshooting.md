---
title: Sorun giderme
description: "Xamarin Canlı Player ve bunları gidermek nasıl bilinen sorunlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: d7c5bedb03d7c869be65e3c704bac58a9cdfcbbd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Sorun giderme

![Önizleme özelliği](~/media/shared/preview.png)

Bu makalede, bazı yaygın sorunlar açıklanmıştır ve bunları düzeltmek için adımları sağlar.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Mobil cihaz tarama barkod (veya girme kod) sonra bağlanamıyor

Xamarin Canlı Player çalıştıran mobil cihaz IDE çalıştıran bilgisayarın aynı ağ üzerinde olmadığında oluşur. Aşağıdakileri denetleyin:

- Cihaz ve bilgisayar aynı WiFi ağda olduğundan emin olun.
  - Bilgisayar da kablolu bir ağa bağlıysa, kablolu bağlantı yönlendiriciyi deneyin.
- Ağ sıkı bir şekilde (bazı şirket ağları gibi), güvenli Xamarin Canlı oynatıcısının gerekli bağlantı noktalarının engellenmesi.
- Xamarin Canlı Player uygulamayı kapatıp yeniden başlatın.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE içinde "dağıtmak çalışılırken hata oluştu" iletisi

**"Ioexception: aktarım bağlantısından veri okunamıyor: engellemeyen yuva işlemi engelle"**

Xamarin Canlı Player çalıştıran mobil cihaz IDE çalıştıran bilgisayarın aynı ağ üzerinde olmadığında bu hata genellikle karşılaştı; Bu durum genellikle daha önce başarıyla eşleştirilmiş bir aygıta bağlanırken oluşur.

* Cihaz ve bilgisayar aynı WiFi ağda olup olmadığını denetleyin.
* Ağ sıkı bir şekilde (bazı şirket ağları gibi), güvenli Xamarin Canlı oynatıcısının gerekli bağlantı noktalarının engellenmesi. Aşağıdaki bağlantı noktaları için Xamarin Canlı Player gereklidir:
  * 37847 – iç ağ erişimi 
  * 8090 – dış ağ erişimi

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
