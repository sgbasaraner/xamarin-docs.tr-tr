---
redirect_url: /xamarin/tools/live-player/
title: XAML Canlı Önizleme
description: Gerçek zamanlı iOS veya Android cihazında uygulama kod değişikliklerini test
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 12/21/2017
ms.openlocfilehash: 721266e1ddfe927b33529a9f4d0eb55a008dd4e8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-live-previewing"></a>XAML Canlı Önizleme

Xamarin Canlı Player avantajlarından biri Önizleme XAML sayfaları canlı, Visual Studio kodda değişiklik ve anında görüntülendiğini değişiklikleri görmek için yeteneğidir. Canlı Önizleme, iOS veya Android cihazında ya da bir simulator veya öykünücü yapılabilir. Bu kılavuz Canlı önizleme özelliğini tek tek XAML ekranları görüntülemek için nasıl kullanılacağını gösterir.

## <a name="requirements"></a>Gereksinimler

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Windows 7 veya sonraki sürümlerini çalıştıran bir makine.
2. Visual Studio 2017 sürüm 15.4 veya üstü ile **.NET ile Mobil Geliştirme** yüklü iş yükü.

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

1. Bir Mac OS X 10.11 sürümünü, macOS 10.12 ya da daha fazla ile.
2. Visual Studio Mac 7.2 veya daha sonra. En son sürümünü öneririz.

-----



<a name="deploydevice" />

## <a name="deploying-to-device"></a>Cihaza dağıtma

İOS veya Android Cihazınızı ile canlı Xamarin Player kullanmadan önce Xamarin Canlı Player uygulamayı indirmek ve Visual Studio için açıklandığı gibi eşleştirin gerekir [yükleme](~/tools/live-player/install.md) Kılavuzu. Visual Studio aygıtınıza başarıyla eşleştirilmiş sonra XAML sayfanızın Canlı Önizleme başlayabilirsiniz. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2017 düzenleyicisinde Canlı Önizleme istediğiniz XAML sayfası açın:

    ![](live-view-images/vs-image1.png)

2. Cihaz yapılandırma kümesine **hata ayıklama | iPhone** iOS için veya **hata ayıklama** Android ve listeden Canlı Player cihazı seçin:

    ![](live-view-images/vs-image2.png)

3. Bu XAML sayfası, Cihazınızda dinamik bir görünüm olarak çalıştırmak için seçin **Araçlar > Xamarin Canlı Player > Canlı geçerli görünümü çalıştırmak** menü çubuğundan:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

1. Visual Studio'da Canlı Önizleme için Mac Düzenleyicisi istediğiniz XAML sayfası açın:

    ![](live-view-images/image1.png)

2. Cihaz yapılandırma kümesine **hata ayıklama | iPhone** iOS için veya **hata ayıklama** Android ve listeden Canlı Player cihazı seçin:

    ![](live-view-images/image2.png)

3. Bu XAML sayfası, Cihazınızda dinamik bir görünüm olarak çalıştırmak için seçin **Çalıştır > Canlı geçerli görünümü çalıştırmak** menü çubuğundan:

    ![](live-view-images/image3.png)

-----








## <a name="deploying-to-android-emulator"></a>Android öykünücüsünü dağıtma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio 2017 düzenleyicisinde Canlı Önizleme istediğiniz XAML sayfası açın:

    ![](live-view-images/vs-image1.png)

2. Cihaz yapılandırma kümesine **hata ayıklama** Android ve listeden Canlı Player cihazı seçin:

    ![](live-view-images/vs-image4.png)

3. Bu XAML sayfası üzerinde Android emulator dinamik bir görünüm olarak çalıştırmak için seçin **Araçlar > Xamarin Canlı Player > Canlı geçerli görünümü çalıştırmak** menü çubuğundan:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Visual Studio'da Canlı Önizleme için Mac Düzenleyicisi istediğiniz XAML sayfası açın:

    ![](live-view-images/image7.png)

2. Cihaz yapılandırma kümesine **hata ayıklama** Android ve listeden Canlı Player cihazı seçin:

    ![](live-view-images/image6.png)

3. Bu XAML sayfası, Cihazınızda dinamik bir görünüm olarak çalıştırmak için Çalıştır'ı seçin > Canlı çalıştırmak geçerli görünümü menü çubuğundan:

    ![](live-view-images/image3.png)

-----





## <a name="deploying-to-ios-simulator"></a>İOS simülatörü dağıtma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Şu anda Windows düğümlerde iOS simulator'da üzerinde dinamik XAML Önizleme kullanma desteği yoktur. Bunun yerine gereken [bir aygıta dağıtmak](#deploydevice).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Visual Studio'da Canlı Önizleme için Mac Düzenleyicisi istediğiniz XAML sayfası açın:

    ![](live-view-images/image1.png)

2. Cihaz yapılandırma kümesine **hata ayıklama | iPhoneSimulator** iOS ve bir iOS simülatörü listeden seçin:

    ![](live-view-images/image2.png)

3. Seçin **Çalıştır > Canlı geçerli görünümü çalıştırmak** simulator başlatın ve XAML sayfası görüntülemek için menü çubuğundan:

    ![](live-view-images/image4.png)

4. Simulator başlattı sonra XAML düzenlemeye başlamak ve canlı görünür bir önizlemesini görürsünüz:

    ![](live-view-images/image5.png)  

-----








## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin Canlı Player genel bakış](https://xamarin.com/live)
- [blog gönderisi](https://blog.xamarin.com/live-player/)
- [Xamarin Canlı Player örnekleri](~/tools/livehttps://developer.xamarin.com/samples.md)
