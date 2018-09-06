---
redirect_url: /xamarin/tools/live-player/
title: XAML Canlı Önizleme
description: Bu belge, Canlı Önizleme XAML sayfalarını, değişiklik yapmak için XAML ve anında cihazda görünmesi değişiklikleri görmek için Xamarin Live Player'ı kullanmak nasıl ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
author: topgenorth
ms.author: toopge
ms.date: 08/08/2018
ms.openlocfilehash: 200d19aa0a13d0557e52cb90021190978838ed39
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780653"
---
# <a name="xaml-live-previewing"></a>XAML Canlı Önizleme

Xamarin Live Player'ın avantajlarından biri, Canlı Önizleme XAML sayfalarını, Visual Studio kodda değişiklik ve Cihazınızda anında görünür değişiklikleri görmek için olanağıdır. Android cihazınıza veya öykünücü veya simülatör Canlı Önizleme yapılabilir. Bu kılavuz, XAML ekranları tek tek görüntülemek için Canlı Önizleme özelliği nasıl yapılacağı açıklanır.

## <a name="requirements"></a>Gereksinimler

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Windows 7 veya sonraki sürümlerini çalıştıran bir makine.
2. Visual Studio 2017 sürüm 15.4 veya üzeri ile **.NET ile Mobil Geliştirme** iş yükü yüklenmiş.

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

1. Bir Mac OS X 10.11, macOS 10.12 veya üzeri ile.
2. Visual Studio Mac 7.2 veya üzeri. En son sürümü öneririz.

-----

<a name="deploydevice" />

## <a name="deploying-to-device"></a>Cihaza dağıtılıyor

Xamarin Live Player ile Android cihazı kullanmadan önce Xamarin Live Player uygulamasını indirin ve açıklandığı gibi Visual Studio'ya eşleştirin gerekir [yükleme](~/tools/live-player/install.md) Kılavuzu. Visual Studio, cihazınıza başarıyla eşleştirilmiş sonra XAML sayfası canlı olarak önizlenebilmesini başlayabilirsiniz. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2017 düzenleyicide Canlı Önizleme istediğiniz XAML sayfasını açın:

    ![](live-view-images/vs-image1.png)

2. Cihaz yapılandırması ayarlanmış **hata ayıklama** ve Live Player cihazı listeden seçin:

    ![](live-view-images/vs-image2.png)

3. Bu XAML sayfası cihazınızdaki dinamik bir görünüm olarak çalıştırmak için seçin **Araçlar > Xamarin Live Player > geçerli görünümde Canlı Çalıştır** menü çubuğundan:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

1. İçin Mac Düzenleyicisi Visual Studio Canlı Önizleme istediğiniz XAML sayfayı açın:

    ![](live-view-images/image1.png)

2. Cihaz yapılandırması ayarlanmış **hata ayıklama** ve Live Player cihazı listeden seçin:

    ![](live-view-images/image2.png)

3. Bu XAML sayfası cihazınızdaki dinamik bir görünüm olarak çalıştırmak için seçin **çalıştırın > geçerli görünümde Canlı Çalıştır** menü çubuğundan:

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-android-emulator"></a>Android öykünücüsü için dağıtma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio 2017 düzenleyicide Canlı Önizleme istediğiniz XAML sayfasını açın:

    ![](live-view-images/vs-image1.png)

2. Cihaz yapılandırması ayarlanmış **hata ayıklama** Android ve Live Player cihaz listesinden seçin:

    ![](live-view-images/vs-image4.png)

3. Bu XAML sayfası Android emulator'da dinamik bir görünüm olarak çalıştırmak için seçin **Araçlar > Xamarin Live Player > geçerli görünümde Canlı Çalıştır** menü çubuğundan:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. İçin Mac Düzenleyicisi Visual Studio Canlı Önizleme istediğiniz XAML sayfayı açın:

    ![](live-view-images/image7.png)

2. Cihaz yapılandırması ayarlanmış **hata ayıklama** Android ve Live Player cihaz listesinden seçin:

    ![](live-view-images/image6.png)

3. Bu XAML sayfası cihazınızdaki dinamik bir görünüm olarak çalıştırmak için Çalıştır'ı seçin > geçerli görünümde Canlı Çalıştır menü çubuğundan:

    ![](live-view-images/image3.png)

-----

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin Live Player genel bakış](https://xamarin.com/live)
- [Blog gönderisi](https://blog.xamarin.com/live-player/)
- [Xamarin Live Player örnekleri](~/tools/live-player/samples.md)
