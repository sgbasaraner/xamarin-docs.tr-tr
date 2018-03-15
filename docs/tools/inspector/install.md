---
title: "Inspector yükleme ve gereksinimler"
description: "İndirme, yükleme ve Xamarin denetçisi kullanmak nasıl."
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a2e6f254c77ac099b5700543db5763b8bbb44fef
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="inspector-installation-and-requirements"></a>Inspector yükleme ve gereksinimler

## <a name="download-and-installation"></a>İndirme ve yükleme


# <a name="windowstabvswin"></a>[Windows](#tab/vswin)

1. İndirme ve yükleme [Xamarin çalışma kitaplarını & Denetçisi for Windows](https://dl.xamarin.com/interactive/XamarinInteractive.msi).
2. [Kendi uygulamanızı inceleyin!](~/tools/inspector/inspect.md)

# <a name="macostabvsmac"></a>[macOS](#tab/vsmac)

1. İndirme ve yükleme [Xamarin çalışma kitaplarını & Denetçisi Mac için](https://dl.xamarin.com/interactive/XamarinInteractive.pkg).
2. [Kendi uygulamanızı inceleyin!](~/tools/inspector/inspect.md)

-----

## <a name="requirements"></a>Gereksinimler

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 veya büyük
- **Windows** -Windows 7 veya daha büyük (Internet Explorer 11 veya üstü ve .NET 4.6.1 veya üzeri)

### <a name="supported-ides"></a>Desteklenen IDE

- Xamarin Studio 6.2 veya büyük
- Visual Studio Mac Önizleme 4 veya daha büyük
- Xamarin ile Visual Studio 2015 4.3.x veya daha büyük
- Xamarin iş yükü ile Visual Studio 2017

Dinamik uygulama İnceleme Kurumsal müşteriler için kullanılabilir.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Desteklenen uygulama platformları

|Uygulama platformu|IDE desteği|Notlar|
|--- |--- |--- |
|Mac (Birleşik)|Yalnızca Mac üzerinde desteklenir|
|iOS (Birleşik)|Desteklenen XS ve Visual Studio|İOS uygulamalarını Windows inceleniyor Mac yapı konakta da yüklenmesi denetçisinin aynı sürümünü gerektirir.|
|Android|Desteklenen XS ve Visual Studio|Android hedeflemelidir > ile 4.0.3, = **fastdev** etkin.<br />Google, Visual Studio veya Xamarin Android öykünücüsünü kullanmanız gerekir. Android 7 öykünücüsünü denetleme şu anda izin vermeyebilir.|
|WPF|Yalnızca Windows Visual Studio'da desteklenir|


<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Raporlama hataları

Doğrudan Visual Studio bildirilen hatalar:

- **Yardım > Görüş Gönder > bir sorun bildirmek**

Lütfen aşağıdaki bilgileri içerir:

### <a name="platform-version-information"></a>Platform sürüm bilgileri

Bu bilgiler önemlidir.

Mac için Visual Studio

- **Visual Studio > Visual Studio hakkında > Ayrıntıları Göster > kopyalama bilgileri**
- Hata raporu yapıştırın

Xamarin Studio

- **Xamarin Studio > Xamarin Studio hakkında > Ayrıntıları Göster > kopyalama bilgileri**
- Hata raporu yapıştırın

Visual Studio

- **Yardım > Visual Studio hakkında > kopyalama bilgileri**
- İşletim sistemi sürümünüz ve 32 bit veya 64-bit Windows çalıştırdığınızdan bizi bilgilendirin.

### <a name="log-files"></a>Günlük dosyaları

IDE ve denetçisi istemci günlük dosyaları her zaman ekleyin.

Inspector istemci

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x da günlük dosyası Bulucu (macOS) veya doğrudan Gezgini'nde (Windows) ana menüden seçebilme özellikleri:

- **Yardım > ortaya günlük dosyası**

Mac için Visual Studio

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio içeriğini **çıkış** bölmesinde da bilgilendirici olabilir.

### <a name="project-settings"></a>Proje ayarları

Eklerseniz **.csproj** incelemek için çalıştığınız proje için son derece yararlı olacaktır. Bu, tek tek ayarlar hakkında isteyen daha kolaydır.

Ayrıca hata ayıklama yapılandırmasını da olduğunu lütfen doğrulayın.

### <a name="selected-devices"></a>Seçilen cihazlar

Android ve iOS için incelemek istediğinizde üzerinde ayıkladığınız hangi cihaz biliyoruz önemlidir. Bilmeniz gerekir:

- IDE içinde gösterildiği gibi cihaz adı
- Cihazınızın işletim sistemi sürümü
- Android: x x86 kullandığınızdan emin olun öykünücüsü
- Android: Hangi öykünücüsü platformu kullanıyorsunuz? Google öykünücüsü? Visual Studio Android öykünücüsü? Xamarin Android Player?
- Mu düzgün ayıkladığınız uygulama görüntülenir ve cihazda işlev?
- Aygıt, ağ bağlantısı (denetimi web tarayıcısı aracılığıyla) var mı?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Kaldır

### <a name="windows"></a>Windows

Nasıl çalışma kitaplarını & Denetçisi aldığınız bağlı olarak, iki kaldırma yordamları gerçekleştirmeniz gerekebilir. Bu yazılımı tamamen kaldırmak için hem de gözden geçirin.

#### <a name="visual-studio-installer"></a>Visual Studio yükleyicisi

Visual Studio 2017 varsa, açık **Visual Studio yükleyicisi**ve konum **bileşenleri tek tek** için **Xamarin çalışma kitaplarını**. İşaretlenirse, onay işaretini kaldırın ve sonra kaldırmak için "Değiştir"'i tıklatın.

#### <a name="system-uninstall"></a>Sistem Kaldır

Çalışma kitapları & Denetçisi kendiniz ile indirilen yükleyiciyi yüklediyseniz, üzerinden mi kaldırılması gerekir **uygulamalar ve Özellikler** sistem ayarları sayfası Windows 10 veya aracılığıyla **Program Ekle/Kaldır**önceki sürümlerinde Windows Denetim Masası'nda.

> **Başlat > Ayarlar > Sistem > uygulamalar ve Özellikler**

![](install-images/windows-remove.png "Xamarin çalışma kitaplarını ve 'Uygulamalar ve Özellikler' listelendiği gibi denetçisi")

**Hala yordamı çalışma kitaplarını emin olmak Visual Studio Yükleyicisi için izlemeniz gereken & Denetçisi bilginiz dışında yeniden değil.**

### <a name="macos"></a>macOS

İle başlayarak [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/), Xamarin çalışma kitaplarını & Denetçisi kaldırılması terminal durumundan çalıştırarak:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Kaldırıcı dosyaları ve dizinleri, kaldırmak ve devam etmeden önce onay isteyin ayrıntılarını kaydeder.

Geçirmek `-help` bağımsız değişkeni `uninstall` daha Gelişmiş senaryolar için komut dosyası.

Eski sürümleri için el ile aşağıdaki kaldırmanız gerekir:

1. Çalışma kitapları uygulamaya Sil `"/Applications/Xamarin Workbooks.app"`
2. Inspector uygulamaya Sil `"Applications/Xamarin Inspector.app"`
2. Eklentileri silin: `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` ve `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`
3. Inspector silin ve destek dosyaları burada: `/Library/Frameworks/Xamarin.Interactive.framework` ve `/Library/Frameworks/Xamarin.Inspector.framework`

