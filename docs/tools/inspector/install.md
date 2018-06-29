---
title: Inspector yükleme ve gereksinimler
description: Bu belge, Xamarin denetçisi yüklemeyi açıklar ve desteklenen işletim sistemi, IDE ve uygulama platformları açıklanır.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: 690329aa1577c66b3aa2794342a8e367477d3a74
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066929"
---
# <a name="inspector-installation-and-requirements"></a>Inspector yükleme ve gereksinimler

## <a name="download-and-installation"></a>İndirme ve yükleme

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. İndirme ve yükleme [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/) seçip **.NET ile Mobil Geliştirme** iş yükü.
1. [Oturum](https://docs.microsoft.com/visualstudio/ide/signing-in-to-visual-studio) Kurumsal aboneliğinizi etkinleştirmek için.
1. [İnceleme](~/tools/inspector/inspect.md) kendi uygulamanızı!

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. İndirme ve yükleme [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/).
1. [Oturum](https://docs.microsoft.com/visualstudio/mac/activation) Kurumsal aboneliğinizi etkinleştirmek için.
1. [İnceleme](~/tools/inspector/inspect.md) kendi uygulamanızı!

-----

## <a name="requirements"></a>Gereksinimler

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 veya büyük
- **Windows** -Windows 7 veya daha büyük (Internet Explorer 11 veya üstü ve .NET 4.6.1 veya üzeri)

### <a name="supported-ides"></a>Desteklenen IDE

- Mac için Visual Studio
- Visual Studio 2017 ile **.NET ile Mobil Geliştirme** iş yükü

Dinamik uygulama İnceleme Kurumsal müşteriler için kullanılabilir.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Desteklenen uygulama platformları

|Uygulama platformu|IDE desteği|Notlar|
|--- |--- |--- |
|Mac|Yalnızca Mac için Visual Studio'da desteklenir|
|iOS|Visual Studio 2017 ve Visual Studio Mac için desteklenen| |
|Android|Visual Studio 2017 ve Visual Studio Mac için desteklenen|Android hedeflemelidir > ile 4.0.3, = **fastdev** etkin.<br />Google, Visual Studio veya Xamarin Android öykünücüsünü kullanmanız gerekir. Android 7 öykünücüsünü denetleme şu anda izin vermeyebilir.|
|WPF|Yalnızca Visual Studio 2017 içinde desteklenir|

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
