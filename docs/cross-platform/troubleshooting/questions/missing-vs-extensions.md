---
title: Yükleme sonrasında eksik Visual Studio uzantıları
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: 7b1f96807d77d9db0a892c5e78124eb3a9890edc
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780622"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Yükleme sonrasında eksik Visual Studio uzantıları

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Hata iletisi: Bu proje geçerli Visual Studio sürümü ile uyumlu değil

Visual Studio'nun uyumlu bir sürümü yüklü olduğundan emin olun:

-   Visual Studio 2017 (Community, Professional veya Enterprise)
-   Visual Studio 2015 (Community, Professional veya Enterprise)

Ayrıca bkz: [Windows gereksinimleri](~/cross-platform/get-started/requirements.md#windows-requirements).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Olası düzeltme 1: Visual Studio uzantıları yüklü olduğundan emin olmak için yüklemeyi değiştirmek

Belirli durumlarda, Xamarin yükleyici işaretini kaldırın otomatik olarak Visual Studio uzantıları yükleme seçeneklerini olabilir. Bu sorunun nedenini olursa, yükleyicinin kullanarak eksik Visual Studio uzantıları yükledikten **değişiklik** komutu. Örneğin, Visual Studio 2013 için uzantıları yüklemek için şunu yazın:

1. Windows açın **programlar ve Özellikler** Denetim Masası.

2. Sağ tıklayın **Xamarin** girişi ve select **değişiklik**.

3. Tıklayın **sonraki**, ardından **değişiklik**.

4. Emin **Xamarin için Visual Studio 2013** yüklemek için seçeneği ayarlanır:

    ![](missing-vs-extensions-images/installer.png "Xamarin için Visual Studio 2013 yükleme seçeneğini etkinleştir")

5. Yükleyici sihirbazının rest üzerinden geçin.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Olası düzeltme 2: Uzantıların yeniden ayarlamak için Visual Studio isteyin

1. Visual Studio uzantıları klasörüne kopyalandıktan Xamarin Uzantıları'nı kontrol edin:

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Uzantılar (3.1.228 sürüm) yüklü olduğundan, klasörde 60 öğe olacaktır:


    ![](missing-vs-extensions-images/folder.png "'Xamarin\3.1.228.0' klasör içeriklerini Gezgini'nde listesi")

2. Bu klasör yanlışlık onayladıktan sonra uzantıları yeniden kurmayı denemek için Visual Studio'yu bildirin:

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Olası düzeltme 3: yeni bir Xamarin yeniden deneyin.

1.  Windows Denetim Masası'ndan mevcut olan aşağıdakilerden herhangi birini kaldırın:

    *   Xamarin

    *   Windows için Xamarin

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Visual Studio için Xamarin

2.  Gezgini içinde kalan dosyaları Xamarin Visual Studio uzantı klasörleri Sil (her ikisi de dahil tüm sürümler **Program dosyaları** ve **Program dosyaları (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  Ayrıca "VirtualStore" dizininde olabilir, herhangi bir uzantı dizinleri "katman" kopyalarını görmek için denetleyin:

    `%LOCALAPPDATA%\VirtualStore`

4.  Kayıt Defteri Düzenleyicisi'ni (regedit) açın.

5.  Bu anahtar için bakın:

    _HKEY\_yerel\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  Bulmak ve bu desenle eşleşen tüm girişleri silin:

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  Bu anahtar için bakın:

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Xamarin için ilgili gibi ara tüm girişleri silin. Örneğin, burada'nın bir, Xamarin, eski sürümlerinde sorunlara neden kullanılır:

    _Mono.VisualStudio.Shell,1.0_

9.  Yeniden başlatma.

10.  Xamarin geçerli kararlı sürümünü yeniden [visualstudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Olası düzeltme 4: Visual Studio'yu onarın yükleme

1.  Windows açın **programlar ve Özellikler** Denetim Masası.

2.  İlgili Microsoft Visual Studio girişi sağ tıklatın ve seçin **Değiştir**

3.  Tıklayın **onarım** açılır Visual Studio iletişim kutusunda düğmesi.
