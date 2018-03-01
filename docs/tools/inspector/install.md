---
title: "Yükleme ve gereksinimler"
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a587935e35882ed1dc68817fbbe1ae3e91200f29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>Yükleme ve gereksinimler

<script> var inspectorOnLoad işlevi () = {var primaryTextBase = "Xamarin çalışma kitaplarını & denetçisine"; var secondaryTextBase = "veya yükleyin"; var inspectorDownloadUrlMac = "https://dl.xamarin.com/interactive/XamarinInteractive.pkg"; var inspectorDownloadUrlWin = "https://dl.xamarin.com/interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href inspectorDownloadUrlMac; = aMac.text = macTextBase + "Mac"; aWin.href inspectorDownloadUrlWin; = aWin.text = winTextBase + "Windows"; };

document.addEventListener ("DOMContentLoaded", inspectorOnLoad);
</script>

## <a name="download-and-installation"></a>İndirme ve yükleme

<ol>
  <li>İndirme ve yükleme <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin çalışma kitaplarını & Denetçisi Mac için</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">veya Windows yükleme</a>).
  </li>
  <li><a href="~/tools/inspector/inspect.md"> Kendi uygulamanızı inceleyin!</a>
    </li>
</ol>

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

<table>
<thead>
  <tr>
    <th>Uygulama platformu</th>
    <th>IDE desteği</th>
    <th>Notlar</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Mac (Birleşik)</td>
    <td>Yalnızca Mac üzerinde desteklenir</td>
    <td/>
  </tr>
  <tr>
    <td>iOS (Birleşik)</td>
    <td>Desteklenen XS ve Visual Studio</td>
    <td>İOS uygulamalarını Windows inceleniyor Mac yapı konakta da yüklenmesi denetçisinin aynı sürümünü gerektirir.</td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Desteklenen XS ve Visual Studio</td>
    <td>
      <ul>
        <li>Android hedeflemelidir > 4.0.3 =</li>
        <li>Fastdev etkinleştirmiş olmanız gerekir</li>
        <li>Google, Visual Studio veya Xamarin Android öykünücüsünü kullanmanız gerekir. Android 7 öykünücüsünü denetleme şu anda izin vermeyebilir.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Yalnızca Windows Visual Studio'da desteklenir</td>
    <td/>
  </tr>
</tbody>
</table>

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Raporlama hataları

Doğrudan Visual Studio bildirilen hatalar:

- **→ Gönderme geri bildirim → rapor bir sorun Yardım**

Lütfen aşağıdaki bilgileri içerir:

### <a name="platform-version-information"></a>Platform sürüm bilgileri

Bu bilgiler önemlidir.

Mac için Visual Studio

- **Visual Studio → Visual Studio → Göster Ayrıntılar → kopyalama bilgiler hakkında**
- Hata raporu yapıştırın

Xamarin Studio

- **Xamarin Studio → Xamarin Studio → Göster hakkında → kopyalama bilgileri ayrıntıları**
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

- **Yardım → açığa günlük dosyası**

Mac için Visual Studio

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Visual Studio içeriğini `Output` bölmesinde da bilgilendirici olabilir.

### <a name="project-settings"></a>Proje ayarları

Eklerseniz `.csproj` incelemek için çalıştığınız proje için son derece yararlı olacaktır. Bu, tek tek ayarlar hakkında isteyen daha kolaydır.

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

Visual Studio 2017 varsa, "Visual Studio yükleyicisi" açın ve "Xamarin çalışma kitapları için" "Bileşenleri tek tek" bakın. İşaretlenirse, onay işaretini kaldırın ve sonra kaldırmak için "Değiştir"'i tıklatın.

#### <a name="system-uninstall"></a>Sistem Kaldır

Çalışma kitapları & Denetçisi kendiniz ile indirilen yükleyiciyi yüklediyseniz, üzerinden mi kaldırılması gerekir **uygulamalar ve Özellikler** sistem ayarları sayfası Windows 10 veya aracılığıyla **Program Ekle/Kaldır**önceki sürümlerinde Windows Denetim Masası'nda.

> **Başlatma → ayarları → sistem → uygulamalar ve Özellikler**

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

