---
title: Ne ı gerçekleştirmek kapsamlı bir Visual Studio için Xamarin kaldırma?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 99fde9330498ee62d3cf6b5910c2cbfae39cfdeb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917674"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Ne ı gerçekleştirmek kapsamlı bir Visual Studio için Xamarin kaldırma?


1.  Windows Denetim Masası'ndan mevcut aşağıdakilerden herhangi birini kaldırın:

    -   Xamarin
    -   Windows için Xamarin
    -   Xamarin.Android
    -   Xamarin.iOS
    -   Visual Studio için Xamarin

2.  Gezgini'nde, Xamarin Visual Studio uzantısı klasörlerinden kalan dosyaları silin (tüm sürümler, her ikisi de dahil olmak üzere _Program Files_ ve _Program Files (x86)_):

    _C:\\Program dosyaları\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\uzantıları\\Xamarin_

3.  Visual Studio'nun MEF Bileşeni önbellek dizini de Sil:

    _% LOCALAPPDATA %\\Microsoft\\Visual Studio\\1\*.0\\ComponentModelCache_

    Aslında, bu adımı kendisi tarafından gibi hataları gidermek yeterli görülür:

    -   "'XamarinShellPackage' paketi düzgün yüklenemedi"

    -   "... Proje dosyası açılamıyor. Eksik proje alt yok"

    -   "Nesne başvurusu bir nesnenin örneğine ayarlanmadı.  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    -   "SetSite paketi için başarısız oldu" (Visual Studio içinde _ActivityLog.xml_)

    -   "LegacySitePackage paketi için başarısız oldu" (Visual Studio içinde _ActivityLog.xml_)

    (Ayrıca bkz. [MEF Bileşeni Önbelleği Temizle](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) Visual Studio uzantısı.  Ve [hata 40781, yorum 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) bu hatalara neden olabilir. Visual Studio'da Yukarı Akış sorun hakkında daha fazla bağlamının.)

4.  Ayrıca iade _VirtualStore_ Windows herhangi depoladığınız, görmek için directory kaplama dosyaları _uzantıları\\Xamarin_ veya _ComponentModelCache_dizinleri vardır:

    _% LOCALAPPDATA %\\VirtualStore_

5.  Kayıt Defteri Düzenleyicisi'ni açın (`regedit`).

6.  Şu anahtarı arayın:

    _HKEY\_yerel\_makine\\yazılım\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7.  Bulma ve bu bir desenle eşleşen herhangi bir giriş silme:

    _C:\\Program dosyaları\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\uzantıları\\Xamarin_

8.  Bu anahtar arayın:

    _HKEY\_geçerli\_kullanıcı\\yazılım\\Microsoft\\Visual Studio\\1\*.0\\ExtensionManager\\PendingDeletions_

9.  Xamarin için ilgili gibi ara girişleri silin.  Örneğin, burada'nın bir, Xamarin eski sürümleri sorun neden kullanılır:

    _Mono.VisualStudio.Shell,1.0_

10. Bir yönetici açın `cmd.exe` komut istemi ve ardından çalıştırın `devenv /setup` ve `devenv /updateconfiguration` komutları her yüklü Visual Studio sürümü.  Örneğin, Visual Studio 2015 için:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. Yeniden başlatma.

12. Xamarin kullanan geçerli kararlı sürümünü yeniden [visualstudio.com](https://visualstudio.com/xamarin/).

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>Ek "paketi doğru yüklenmedi için" sorun giderme adımları

Burada yukarıdaki adımları "paketi düzgün yüklenemedi" hata çözümleme durumlarda denemek için birkaç adım şunlardır.

1.  Yeni bir Windows kullanıcı hesabı oluşturun.

2.  Xamarin Visual Studio uzantıları yeni kullanıcı için hatasız yük olmadığını denetleyin.

3.  Uzantıları doğru yüklerseniz, ardından sorun büyük olasılıkla özgün kullanıcıya saklı ayarlarından bazıları kaynaklanır:

    -   **Explorer'da** – _LOCALAPPDATA %\\Microsoft\\Visual Studio\\1\*.0_
    -   **Regedit** – _HKEY\_geçerli\_kullanıcı\\yazılım\\Microsoft\\Visual Studio\\1\*.0_
    -   **Regedit** – _HKEY\_geçerli\_kullanıcı\\yazılım\\Microsoft\\Visual Studio\\1\*.0\_yapılandırma_

4.  Bu saklı ayarları gerçekten sorun görünüyorsa, yedekledikten ve onları silerek deneyebilirsiniz.
