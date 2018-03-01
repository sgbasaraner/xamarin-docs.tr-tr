---
title: "Nasıl Xamarin lisansları el ile eşitlemek?"
ms.topic: article
ms.prod: xamarin
ms.assetid: D0BD93E9-3A1F-4E5B-8EE8-36ADC33DCFE4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 2413b6b7563a6ed1e17a8db61d2d61ddc85e71ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-manually-resynchronize-xamarin-licenses"></a>Nasıl Xamarin lisansları el ile eşitlemek?

> [!IMPORTANT]
> Bu kılavuz sahibi veya Xamarin hesaplara günlük kullanmadığınız sürece olması gerekmez çünkü çoğu MSDN kullanıcılara uygulanmaz [Xamarin bileşenleri depolamak](https://components.xamarin.com/) veya [Mac (Mac) için Visual Studio](~/cross-platform/get-started/requirements.md).




## <a name="overview"></a>Genel Bakış

IDE yeniden başlattığınızda genellikle, lisans bilgilerini Xamarin lisans sunucusu ile otomatik olarak eşlenir. Örneğin, lisans yükseltmeleri, downgrades ve yenilemeleri normalde otomatik olarak IDE yeniden başlatıldıktan sonra görünür. IDE içinde gösterilen Lisans bilgileri üzerinde gösterilen bilgiler eşleşmelidir, [hesap sayfası](https://store.xamarin.com/account/my/subscription/computers). Bilgi hala eşleşmeyen ise, depolama hesabı bilgilerinizi doğru arar ve daha sonra el ile Lisans Günlük, disk üzerindeki lisans dosyalarını silerek ve sonra oturum açmayı yeniden eşitlemek ilk denetleyin.

### <a name="note-for-ios-developers-on-windows"></a>Windows iOS geliştiricileri için Not

iOS geliştiricileri Windows Ayrıca Mac için Visual Studio için bu adımları uygulamadan gerekebilir; eşleştirilmiş Mac yapı ana bilgisayarda doğrudan geliştirme yok olsa bile.

## <a name="quick-manual-refresh-steps"></a>Hızlı el ile yenileme adımları

Bu hızlı işlem bazı durumlarda yeterli olabilir disk üzerindeki Lisans dosyaları silme adımını atlar. 

1.  Xamarin hesabınıza IDE aracılığıyla oturum:
    -   Mac için Visual Studio
        -   Hoş Geldiniz ekranında sağ üst köşesinde
        -   **Mac için Visual Studio > Hesap** (Mac)
        -   **Araçlar > Hesap** (Windows)
    -   Visual Studio
        -   **Araçlar > Xamarin hesabı**
2.  Geri IDE Xamarin hesabınızda oturum açın.

## <a name="extended-manual-license-refresh-steps"></a>El ile Lisans yenileme adımları genişletilmiş

1.  Xamarin hesabınıza IDE aracılığıyla oturum açın. 
2.  IDE çıkın.
3.  Depolama hesabı bilgileri doğru görünüyorsa denetleyin. Özellikle yinelenen bilgisayar adları için üzerinde denetimi [bilgisayarlar sayfası](https://store.xamarin.com/account/my/subscription/computers).

4.  Yinelenen bilgisayar adları çifti görürseniz, kullanın **etkinliğini** kaldırmak için açılır menü öğesi _her ikisi de_ çiftinin üyeleri:
    
    ![Lisans https://store.xamarin.com/account/my/subscription/computers üzerinde devre dışı bırak ->](resync-licenses-images/deactivate.png "çiftinin her iki üyeleri kaldırmak için devre dışı bırak açılır menü öğesini kullanın")

5.  Disk üzerinde hala mevcut lisans dosyaları kalan tüm kopyalarını silin.
    -   Windows

        Aşağıdaki klasörlerin Tümünü Sil:
        -   `%PROGRAMDATA%\Mono for Android`
        -   `%PROGRAMDATA%\MonoTouch`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\Mono for Android`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\MonoTouch`

        Windows yüklemelerinde varsayılan:
        -   `%PROGRAMDATA%` genişletilir `C:\ProgramData`
        -   `%LOCALAPPDATA%` genişletilir `C:\Users\%USERNAME%\AppData\Local`

        `VirtualStore` Lisanslar kopyalarını otomatik olarak oluşturulur Windows tarafından belirli senaryolarda. VirtualStore kopya varsa, bunlar okunacak _yerine_ lisans `%PROGRAMDATA%`.

    -   Mac

        Aşağıdaki klasörlerin Tümünü Sil:

        -   `~/Library/MonoAndroid`
        -   `~/Library/MonoTouch`
        -   `~/Library/Xamarin.Mac`

        Bu yolları erişebilirsiniz **Bulucu** seçerek **Git > klasörüne gidin** menü ve iletişim kutusu penceresine yapıştırma. Bulucu otomatik olarak değiştirir ~ kullanıcı klasörünüzü tilde karakteriyle.

6.  Açık Mac için Visual Studio veya Visual Studio ve günlük Xamarin hesabınızda yedekleyin.

## <a name="supplementary-information-individual-license-file-locations"></a>Ek bilgi: tek tek lisans dosya konumları

### <a name="windows"></a>Windows

Windows üzerinde tek tek lisans dosyaları aşağıdaki konumlarda depolanır:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.licx`

İçin lisansları *deneme* abonelikleri farklı biçimde adlandırılır:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.trial.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.trial.licx`

### <a name="mac"></a>Mac

Mac üzerinde tek tek lisans dosyaları aşağıdaki konumlarda depolanır:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.v2`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License`

İçin lisansları *deneme* abonelikleri farklı biçimde adlandırılır:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License.trial`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.trial`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License.trial`

## <a name="additional-troubleshooting-steps"></a>Ek sorun giderme adımları

-   Kullanıcı adınızı, bilgisayarınızın adını veya herhangi bir lisans dosyaları tam olarak genişletilmiş yollarını herhangi ASCII olmayan karakterler olup olmadığını denetleyin.

-   [Xamarin Geliştirici Desteği ile iletişime geçin](http://xamarin.com/support)
