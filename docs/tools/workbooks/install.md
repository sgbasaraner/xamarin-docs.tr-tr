---
title: "Yükleme ve gereksinimler"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9D4E10E8-A288-4C6C-9475-02969198C119
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: abc9f9402b55a11e313b9938f07f37e5329b55b6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="installation-and-requirements"></a>Yükleme ve gereksinimler

<script> var inspectorOnLoad işlevi () = {var primaryTextBase = "Xamarin çalışma kitapları için"; var secondaryTextBase = "veya yükleyin"; var inspectorDownloadUrlMac "https://dl.xamarin.com/interactive/XamarinInteractive.pkg" =; var inspectorDownloadUrlWin = " https://DL.xamarin.com/interactive/XamarinInteractive.msi";

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  if (/win/i.test(navigator.platform.toLowerCase())) { aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase; }

  aMac.href inspectorDownloadUrlMac; = aMac.text = macTextBase + "Mac"; aWin.href inspectorDownloadUrlWin; = aWin.text = winTextBase + "Windows"; };

document.addEventListener ("DOMContentLoaded", inspectorOnLoad);
</script>

<a name="install" />

## <a name="download-and-install"></a>İndirme ve yükleme

<ol>
  <li>Denetleme <a href="#Requirements"> gereksinimleri</a> aşağıda.</li>
  <li>İndirme ve yükleme <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Mac için Xamarin çalışma kitaplarını</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">veya Windows yükleme</a>).
  </li>
  <li>Başlat <a href="~/tools/workbooks/workbook.md"> çalma</a> çalışma kitaplarını ya da deneme ile <a href="https://developer.xamarin.com/workbooks/">örnekleri</a>.
    </li>
</ol>

## <a name="requirements"></a>Gereksinimler

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 veya büyük
- **Windows** -Windows 7 veya daha büyük (Internet Explorer 11 veya üstü ve .NET 4.6.1 veya üzeri)

#### <a name="supported-app-platforms"></a>Desteklenen uygulama platformları

<table>
<thead>
  <tr>
    <th>Uygulama platformu</th>
    <th>İşletim sistemi desteği</th>
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
    <td>Mac ve Windows desteklenen</td>
    <td>
      <ul>
        <li>Xamarin.iOS 11.0 ve Xcode 9.0 veya büyük Mac üzerinde yüklü olmalıdır</li>
        <li>Yukarıdakilerin tümü, çalıştıran bir Mac yapı konak iOS çalışma kitaplarını Windows üzerinde çalışan gerektirir ve <a href="~/tools/ios-simulator.md">düğümlerde iOS simülatörü</a> Windows üzerinde yüklü.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Mac ve Windows desteklenen</td>
    <td>Google, Visual Studio veya Xamarin Android öykünücüsü, bir sanal aygıtla kullanmalısınız > 5.0 =</td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Yalnızca Windows üzerinde desteklenir</td>
    <td/>
  </tr>
  <tr>
    <td>Konsol (.NET Framework)</td>
    <td>Mac ve Windows desteklenen</td>
    <td/>
  </tr>
  <tr>
    <td>Konsol (.NET çekirdek)</td>
    <td>Mac ve Windows desteklenen</td>
    <td/>
  </tr>
</tbody>
</table>

## <a name="reporting-bugs"></a>Raporlama hataları

Lütfen [Github'da sorunları rapor][bugs]ve tüm aşağıdaki bilgileri içerir:

### <a name="log-files"></a>Günlük dosyaları

Çalışma kitapları istemci günlük dosyaları her zaman ekleyin:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4.x da günlük dosyası Bulucu (macOS) veya doğrudan Gezgini'nde (Windows) ana menüden seçebilme özellikleri:

- **Yardım → açığa günlük dosyası**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Günlük yolları çalışma kitaplarını 1.3 ve önceki sürümleri:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Platform sürüm bilgileri

İşletim sisteminizi ayrıntılarını bilmeniz çok yararlı olur ve Xamarin ürünler yüklü.

Çalışma kitapları ana menüden:

* **Yardım → kopyalama sürüm bilgileri**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Ve daha önce çalışma kitaplarının 1.3 için yönergeler:

Mac için Visual Studio

- **Visual Studio → Visual Studio → Göster Ayrıntılar → kopyalama bilgiler hakkında**
- Hata raporu yapıştırın

Visual Studio

- **Visual Studio → bilgi Kopyala hakkında → Yardım**
- İşletim sistemi sürümünüz ve 32 bit veya 64-bit Windows çalıştırdığınızdan bizi bilgilendirin.

### <a name="samples"></a>Örnekler

Ekleme ya da bağlantı `.workbooks` hatayı daha hızlı çözmenize yardımcı olabilecek, sorun yaşadığınız dosya.

### <a name="devices"></a>Cihazlar

İOS veya Android çalışma kitabı bağlanma sorunu yaşıyor ve zaten kullanıma [sorun giderme sayfamızı](~/tools/workbooks/troubleshooting/index.md), biz bilmeniz gerekir:

- Bağlanmaya çalıştığınız aygıt adı
- Cihazınızın işletim sistemi sürümü
- Android: x x86 kullandığınızdan emin olun öykünücüsü
- Android: Hangi öykünücüsü platformu kullanıyorsunuz? Google öykünücüsü?
  Visual Studio Android öykünücüsü? Xamarin Android Player?
- Windows İos'ta: Xamarin uzaktan iOS simülatörü hangi sürümü, yüklü olan (denetleyin `Add/Remove Programs` içinde `Control Panel`)?
- Windows İos'ta: Lütfen Platform sürüm bilgileri Mac yapı ana bilgisayarınız için de sağlayın
- Aygıt, ağ bağlantısı (denetimi web tarayıcısı aracılığıyla) var mı?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Kaldır

### <a name="windows"></a>Windows

Nasıl çalışma kitaplarını & Denetçisi aldığınız bağlı olarak, iki kaldırma yordamları gerçekleştirmeniz gerekebilir. Bu yazılımı tamamen kaldırmak için hem de gözden geçirin.

#### <a name="visual-studio-installer"></a>Visual Studio yükleyicisi

Visual Studio 2017 varsa, açık **Visual Studio yükleyicisi**ve konum **bileşenleri tek tek** için **Xamarin çalışma kitaplarını**. İşaretlenirse, onay işaretini kaldırın ve ardından **Değiştir** kaldırmak için.

#### <a name="system-uninstall"></a>Sistem Kaldır

Çalışma kitapları & Denetçisi kendiniz ile indirilen yükleyiciyi yüklediyseniz, üzerinden mi kaldırılması gerekir **uygulamalar ve Özellikler** sistem ayarları sayfası Windows 10 veya aracılığıyla **Program Ekle/Kaldır**önceki sürümlerinde Windows Denetim Masası'nda.

> **Başlatma → ayarları → sistem → uygulamalar ve Özellikler**

![](install-images/windows-remove.png "Xamarin çalışma kitaplarını ve içinde listelenen denetçisi &quot;uygulamaları &amp; özellikleri&quot;")

**Hala yordamı çalışma kitaplarını emin olmak Visual Studio Yükleyicisi için izlemeniz gereken & Denetçisi bilginiz dışında yeniden değil.**

<a name="uninstall-macos" />

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

## <a name="downgrading"></a>Önceki sürüme indirme

Paket tanımlayıcısı `/Applications/Xamarin Workbooks.app` değiştirildi `com.xamarin.Inspector` için `com.xamarin.Workbooks` gelecekteki Xamarin çalışma kitaplarını & Denetçisi yükleyicileri bölme kolaylaştırmak için 1.4 sürümde.

Eski yükleyicileri bir hata nedeniyle 1.3.2 veya eski yükleyicileri kullanarak 1.4 ya da daha yeni sürümleri düşürmek mümkün değil.

1.4 veya 1.3.2 için daha yeni veya eski düşürmek için:

1. [Çalışma kitapları & Denetçisi el ile kaldırma](#macOS)
2. 1.3.2 çalıştırmak veya daha eski `.pkg` yükleyici