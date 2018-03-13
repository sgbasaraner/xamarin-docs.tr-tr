---
title: "Xamarin Windows Visual Studio yükleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: bffc6bb1bc6537745fe9603906b5938fc3c34fe1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="installing-xamarin-in-visual-studio-on-windows"></a>Xamarin Windows Visual Studio yükleme

Xamarin ile artık bulunduğundan hiçbir ek Visual Studio'ya'nin tüm sürümlerinde maliyet ve ayrı bir lisansı gerektirmez, Visual Studio yükleyicisi indirmek ve Xamarin Araçları'nı yüklemek için kullanabilirsiniz.

<a name="requirements" />

## <a name="requirements"></a>Gereksinimler

Visual Studio Araçları için Xamarin yüklemek için gerekli şunlardır:

1. Windows 7 veya daha yüksek.

2. Visual Studio 2015 veya 2017 (Community, Professional veya Enterprise).

3. Visual Studio için Xamarin.

Xamarin Visual Studio Express sürümleri ile eklenti desteğinin olmaması için kullanılamayacağını unutmayın.

Yükleme ve Xamarin kullanma önkoşulları hakkında daha fazla bilgi için bkz: [sistem gereksinimleri](~/cross-platform/get-started/requirements.md).


<a name="installation" />

## <a name="installation"></a>Yükleme

Xamarin yeni bir Visual Studio yüklemesinin bir parçası olarak yüklenebilir.
Bunun için aşağıdaki adımları kullanın:

1. Visual Studio Community, Visual Studio Professional ya da Visual Studio kuruluş karşıdan [Visual Studio](https://www.visualstudio.com/vs/) sayfa (indirme bağlantıları en altında sağlanır).

2. Yüklemeyi başlatmak için indirilen paketi çift tıklatın.

3. Seçin **.NET ile Mobil Geliştirme** iş yükü yükleme ekranında: 

    [![İş yükleri ekranında .NET seçimi ile Mobil Geliştirme](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png#lightbox)

4. Sırada **.NET ile Mobil Geliştirme** olan seçili göz atın sahip **Özet** sağ panelde. Burada, yüklemek istiyor musunuz mobil geliştirme seçenekleri seçimini kaldırabilirsiniz. Varsayılan olarak, aşağıdaki ekran görüntüsünde gösterilen tüm seçenekleri yüklenir (**Xamarin çalışma kitaplarını**, **Xamarin profil oluşturucu**, **Xamarin düğümlerde Simulator**,  **Android NDK**, **Android SDK**, **Java SE Geliştirme Seti**, **Google Android öykünücüsü**, **F # Destek**, ve **Intel HAXM**):

    ![Özet panelini yüklemek için Xamarin seçenekleri listeleme](windows-images/02-summary.png)

5. Visual Studio yüklemeyi başlatmaya hazır olduğunuzda tıklatın **yükleme** sağ alt köşesinde düğmesini:

    ![Yükleme düğmesini konumu](windows-images/03-click-install.png)

   Visual Studio hangi sürümü yüklemekte olduğunuz bağlı olarak, yükleme işleminin tamamlanması uzun zaman alabilir. İlerleme çubukları yükleme izlemek için kullanabilirsiniz:

    ![İlerleme çubukları yükleme sırasında örnek ekran görüntüsü](windows-images/04-progress-bars.png)

6. Visual Studio yüklemesi tamamlandığında tıklayın **başlatma** düğmesi Visual Studio'yu başlatmak için:

    ![Başlat düğmesi konumu](windows-images/05-launch.png)


<a name="vs2017" />

### <a name="adding-xamarin-to-visual-studio-2017"></a>Visual Studio 2017 Xamarin ekleme

Visual Studio 2017 zaten yüklediyseniz, iş yüklerini değiştirmek için Visual Studio yükleyicisi yeniden çalıştırarak Xamarin ekleyebilirsiniz (bkz [değiştirmek Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) Ayrıntılar için). Ardından, Xamarin yüklemek için yukarıda listelenen adımları izleyin.

Visual Studio 2017 yükleyip hakkında daha fazla bilgi için bkz: [yükleme Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


<a name="vs2015" />

### <a name="adding-xamarin-to-visual-studio-2015"></a>Visual Studio 2015 için Xamarin ekleme

Visual Studio 2015 mevcut bir yüklemeye Xamarin.Android eklemek için aşağıdaki adımları kullanın:

1. Sağ Windows **Başlat** düğmesine tıklayın ve ardından **programlar ve Özellikler**.

2. Sağ **Microsoft Visual Studio** tıklatıp **değişiklik**.

3. Visual Studio yükleyicisi iletişim kutusu göründüğünde tıklayın **Değiştir** düğmesi.

4. İçinde **özellikleri** sekmesinde, aşağı kaydırarak **platformlar arası mobil geliştirme**. Yanındaki onay kutusuna tıklayın **C# / .NET (Xamarin)**:

    ![C# ekleme / Visual Studio 2015 için .NET Xamarin](windows-images/06-add-xamarin.png)

5. Tıklatın **güncelleştirme** Visual Studio için Xamarin eklemek için düğmeyi.


<a name="verifying" />

### <a name="verifying-installation"></a>Yüklemeyi doğrulama

Visual Studio 2017 içinde Xamarin tıklayarak yüklendiğini doğrulayabilirsiniz **yardımcı** menüsü. Xamarin yüklediyseniz görmelisiniz bir **Xamarin** bu ekran görüntüsünde gösterildiği gibi menü öğesi:

![Yardım menüsünde görüntülenen Xamarin menü öğesi](windows-images/12-xamarin-menu-item.png)

Bir Visual Studio'nun önceki sürümleri kullanıyorsanız, tıklayabilirsiniz **Yardım > Microsoft Visual Studio hakkında** ve Xamarin yüklü olup olmadığını görmek için yüklü ürünlerin listesini kaydırma:

![Visual Studio yüklü ürün ekranı](windows-images/13-xamarin-is-installed.png)

Sürüm bilgileri bulma hakkında daha fazla bilgi için bkz: [nereden bulabilirim my sürüm bilgileri ve günlükleri?](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

## <a name="next-steps"></a>Sonraki Adımlar

Xamarin için Visual Studio Araçları yükleme, uygulamalarınız için kod yazma başlatmanızı sağlar, ancak ek kurulum geliştirmek ve simulator, öykünücüsü ve cihaz uygulamalarınızı dağıtma gerektirir. Yüklemenizi tamamlamak ve platformlar arası uygulamalar oluşturmaya başlamak için aşağıdaki Kılavuzlar ziyaret edin.

### <a name="ios"></a>iOS

Daha ayrıntılı bilgi için bkz: [yükleme Xamarin.iOS Windows](~/ios/get-started/installation/windows/index.md) Kılavuzu. 

1. [Mac'inizde Xamarin.iOS araçlarını yükleme](~/ios/get-started/installation/windows/index.md#installation)
2. [Mac yapılandırma](~/ios/get-started/installation/windows/index.md#configuration)
3. [iOS Developer Kurulumu](~/ios/get-started/installation/windows/index.md#developersetup) (cihazda uygulamanızı çalıştırmak için).
4. [Visual Studio Mac yapı ana bilgisayara bağlanma](~/ios/get-started/installation/windows/index.md#connectingtomac)
5. [Düğümlerde iOS simülatörü](~/tools/ios-simulator.md)
6. [Visual Studio için Xamarin.iOS’a Giriş](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

Daha ayrıntılı bilgi için bkz: [yükleme Xamarin.Android Windows](~/android/get-started/installation/windows.md) Kılavuzu.

1. [Xamarin.Android Configuration](~/android/get-started/installation/windows.md#configuration)
2. [Xamarin Android SDK Yöneticisi'ni kullanma](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Android SDK Emulator](~/android/get-started/installation/android-emulator/index.md)
4. [Cihazı Dağıtım için Ayarlama](~/android/get-started/installation/set-up-device-for-development.md)
