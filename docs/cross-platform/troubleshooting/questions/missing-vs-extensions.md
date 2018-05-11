---
title: Yükleme sonrasında eksik Visual Studio uzantıları
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: e47cfc4de77a6310a81867eefb07c3c1e5cc7060
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Yükleme sonrasında eksik Visual Studio uzantıları

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Hata iletisi: Bu projeyi Visual Studio geçerli sürümü ile uyumlu değil

Visual Studio uyumlu bir sürümü yüklü emin olun:

-   Visual Studio 2017 (Community, Professional veya Enterprise)
-   Visual Studio 2015 (Community, Professional veya Enterprise)

Ayrıca bkz. [Windows gereksinimleri](~/cross-platform/get-started/requirements.md#windows).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Olası düzeltme 1: Visual Studio uzantıları yüklü olduğundan emin olun yüklemeye değiştirme

Bazı durumlarda, Xamarin yükleyici kaldırma onay otomatik olarak Visual Studio Uzantıları'nı yükleme seçeneklerini olabilir. Bu sorunun nedeni olursa, yükleyicinin kullanarak eksik Visual Studio uzantıları yükleme **değişiklik** komutu. Örneğin, Visual Studio 2013 için Uzantıları'nı yüklemek için şunu yazın:

1. Açık pencereleri **programlar ve Özellikler** Denetim Masası.

2. Sağ tıklayın **Xamarin** seçin ve girişi **değişiklik**.

3. Tıklatın **sonraki**, ardından **değişiklik**.

4. Emin olun **Visual Studio 2013 için Xamarin** seçeneği ayarlanmış yüklemek için:

    ![](missing-vs-extensions-images/installer.png "Visual Studio 2013 yükleme seçeneği için Xamarin etkinleştir")

5. Yükleyici Sihirbazı'nı geri kalanı ile devam edin.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Olası düzeltme 2: Uzantıların yeniden ayarlamak için Visual Studio isteyin

1. Xamarin uzantıları Visual Studio uzantıları klasörüne kopyaladıysanız denetleyin:

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Uzantıları düzgün (3.1.228 sürümü) yüklü değilse klasöründe 60 öğeleri olacaktır:


    ![](missing-vs-extensions-images/folder.png "'Xamarin\3.1.228.0' klasör içeriklerini Explorer'da listesi")

2. Bu klasör doğru görünüyorsa onayladıktan sonra uzantıları yeniden kurmayı denemek için Visual Studio verin:

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Olası düzeltme 3: Xamarin yeni yeniden deneyin

1.  Windows Denetim Masası'ndan mevcut aşağıdakilerden herhangi birini kaldırın:

    *   Xamarin

    *   Windows için Xamarin

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Visual Studio için Xamarin

2.  Gezgini'nde, Xamarin Visual Studio uzantısı klasörlerinden kalan dosyaları silin (tüm sürümler, her ikisi de dahil olmak üzere **Program Files** ve **Program Files (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  Ayrıca olabilir, herhangi bir uzantı dizinler "katmana" kopyalarını görmek için "VirtualStore" dizininde denetleyin:

    `%LOCALAPPDATA%\VirtualStore`

4.  Kayıt Defteri Düzenleyicisi'ni (regedit) açın.

5.  Bu anahtar arayın:

    _HKEY\_yerel\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  Bulma ve bu bir desenle eşleşen herhangi bir giriş silme:

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  Bu anahtar arayın:

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Xamarin için ilgili gibi ara girişleri silin. Örneğin, burada'nın bir, Xamarin eski sürümleri sorun neden kullanılır:

    _Mono.VisualStudio.Shell,1.0_

9.  Yeniden başlatma.

10.  Xamarin geçerli kararlı sürümünü yeniden [visualstudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Olası düzeltme 4: Onarım Visual Studio yükleme

1.  Açık pencereleri **programlar ve Özellikler** Denetim Masası.

2.  İlgili Microsoft Visual Studio girişi sağ tıklatın ve seçin **Değiştir**

3.  Tıklatın **onarım** açar Visual Studio iletişim kutusunda düğme.
