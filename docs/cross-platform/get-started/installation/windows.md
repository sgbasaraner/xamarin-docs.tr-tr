---
title: Xamarin Visual Studio 2017 yükleme
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: 0f1c014f316ff4f3eb7341fa9815475175d11937
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="installing-xamarin-in-visual-studio-2017"></a>Xamarin Visual Studio 2017 yükleme

<a name="requirements" />

## <a name="requirements"></a>Gereksinimler

Xamarin Visual Studio 2017 yüklemek için gerekli şunlardır:

1. Windows 7 veya daha yüksek.

2. Visual Studio 2017 (Community, Professional veya Enterprise).

3. Visual Studio için Xamarin.

Yükleme ve Xamarin kullanma önkoşulları hakkında daha fazla bilgi için bkz: [sistem gereksinimleri](~/cross-platform/get-started/requirements.md).

<a name="installation" />

## <a name="installation"></a>Yükleme

Xamarin yeni bir Visual Studio 2017 yüklemesinin bir parçası olarak yüklenebilir.
Bunun için aşağıdaki adımları kullanın:

1. Visual Studio 2017 Community, Visual Studio Professional ya da Visual Studio kuruluş karşıdan [Visual Studio](https://www.visualstudio.com/vs/) sayfa (indirme bağlantıları en altında sağlanır).

2. Yüklemeyi başlatmak için indirilen paketi çift tıklatın.

3. Seçin **.NET ile Mobil Geliştirme** iş yükü yükleme ekranında: 

    [![İş yükleri ekranında .NET seçimi ile Mobil Geliştirme](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png#lightbox)

4. Sırada **.NET ile Mobil Geliştirme** olan seçili göz atın sahip **Özet** sağ panelde. Burada, yüklemek istiyor musunuz mobil geliştirme seçenekleri seçimini kaldırabilirsiniz. Varsayılan olarak, aşağıdaki ekran görüntüsünde gösterilen tüm seçenekleri yüklenir (**Xamarin çalışma kitaplarını**, **Xamarin profil oluşturucu**, **Xamarin düğümlerde Simulator**,  **Android NDK**, **Android SDK**, **Java SE Geliştirme Seti**, **Google Android öykünücüsü**, **F # Destek**, ve **Intel HAXM**):

    ![Özet panelini yüklemek için Xamarin seçenekleri listeleme](windows-images/02-summary.png)

5. Visual Studio 2017 yüklemeyi başlatmaya hazır olduğunuzda tıklatın **yükleme** sağ alt köşesinde düğmesini:

    ![Yükleme düğmesini konumu](windows-images/03-click-install.png)

   Visual Studio 2017 hangi sürümünün yüklemekte olduğunuz bağlı olarak, yükleme işleminin tamamlanması uzun zaman alabilir. İlerleme çubukları yükleme izlemek için kullanabilirsiniz:

    ![İlerleme çubukları yükleme sırasında örnek ekran görüntüsü](windows-images/04-progress-bars.png)

6. Visual Studio 2017 yükleme tamamlandığında, tıklayın **başlatma** düğmesi Visual Studio'yu başlatmak için:

    ![Başlat düğmesi konumu](windows-images/05-launch.png)

<a name="vs2017" />

### <a name="adding-xamarin-to-visual-studio-2017"></a>Visual Studio 2017 Xamarin ekleme

Visual Studio 2017 zaten yüklediyseniz, iş yüklerini değiştirmek için Visual Studio 2017 yükleyici yeniden çalıştırarak Xamarin ekleyebilirsiniz (bkz [değiştirmek Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) Ayrıntılar için). Ardından, Xamarin yüklemek için yukarıda listelenen adımları izleyin.

Visual Studio 2017 yükleyip hakkında daha fazla bilgi için bkz: [yükleme Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


### <a name="verifying-installation"></a>Yüklemeyi doğrulama

Visual Studio 2017 içinde Xamarin tıklayarak yüklendiğini doğrulayabilirsiniz **yardımcı** menüsü. Xamarin yüklediyseniz görmelisiniz bir **Xamarin** bu ekran görüntüsünde gösterildiği gibi menü öğesi:

![Yardım menüsünde görüntülenen Xamarin menü öğesi](windows-images/12-xamarin-menu-item.png)

Bir Visual Studio'nun önceki sürümleri kullanıyorsanız, tıklayabilirsiniz **Yardım > Microsoft Visual Studio hakkında** ve Xamarin yüklü olup olmadığını görmek için yüklü ürünlerin listesini kaydırma:

![Visual Studio yüklü ürün ekranı](windows-images/13-xamarin-is-installed.png)

Sürüm bilgileri bulma hakkında daha fazla bilgi için bkz: [nereden bulabilirim my sürüm bilgileri ve günlükleri?](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

## <a name="next-steps"></a>Sonraki Adımlar

Xamarin Visual Studio 2017 yükleme, uygulamalarınız için kod yazma başlatmanızı sağlar, ancak ek kurulum geliştirmek ve simulator, öykünücüsü ve cihaz uygulamalarınızı dağıtma gerektirir. Yüklemenizi tamamlamak ve platformlar arası uygulamalar oluşturmaya başlamak için aşağıdaki Kılavuzlar ziyaret edin.

### <a name="ios"></a>iOS

Daha ayrıntılı bilgi için bkz: [yükleme Xamarin.iOS Windows](~/ios/get-started/installation/windows/index.md) Kılavuzu. 

1. [Mac için Visual Studio yükleme](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Visual Studio, Mac yapı konağına Bağla](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [iOS Developer Kurulumu](~/ios/get-started/installation/device-provisioning/index.md) - cihazda uygulamanızı çalıştırmak için gerekli
5. [Düğümlerde iOS simülatörü](~/tools/ios-simulator.md)
6. [Visual Studio için Xamarin.iOS’a Giriş](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

Daha ayrıntılı bilgi için bkz: [yükleme Xamarin.Android Windows](~/android/get-started/installation/windows.md) Kılavuzu.

1. [Xamarin.Android yapılandırma](~/android/get-started/installation/windows.md#configuration)
2. [Xamarin Android SDK Yöneticisi'ni kullanma](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Android SDK Emulator](~/android/get-started/installation/android-emulator/index.md)
4. [Cihazı Dağıtım için Ayarlama](~/android/get-started/installation/set-up-device-for-development.md)
