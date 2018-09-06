---
title: Xamarin Live Player Kurulumu
description: Bu belge, Xamarin Live Player ' ve çalışan bir uygulamaya Canlı düzenlemeler için kullanmak açıklar.
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: topgenorth
ms.author: toopge
ms.date: 08/08/2018
ms.openlocfilehash: 10fdc09ec946ff57b87ad4749cf6edaf04b5f7e8
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780612"
---
# <a name="xamarin-live-player-setup"></a>Xamarin Live Player Kurulumu

Xamarin Live Player, uygulamanızın Canlı düzenlemeler yapmanıza olanak tanır ve bu değişiklikleri Canlı cihazda yansıtılmasını. Kodunuz gerek öykünücüleri ' ayarlamak veya kablolar dağıtmak üzere kullanmak için Xamarin Live Player uygulamanın içinde çalışır! Bu makalede, Xamarin Live Player'ı ayarlama açıklanır.

![Önizleme özelliği](~/media/shared/preview.png)

## <a name="1-get-the-android-app"></a>1. Android uygulamasını edinin

Google play'den Android için Xamarin Live Player kullanılabilir:

[ ![Google play'de kullanılabilir](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Google Play olmadan Android cihazları için Xamarin Live Player aracılığıyla kullanılabilir [HockeyApp](https://aka.ms/xlp-hockeyapp) dağıtım. Android için seçim tarafından doğrudan Google Play'den yüklenebilir için ek olarak, erken Önizleme derlemelerini [açık beta programı](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="2-get-visual-studio-2017"></a>2. Visual Studio 2017'yi alın

Xamarin Live Player gerektirir:

- Visual Studio 2017 15.4 veya daha yeni.
- Visual Studio bilgisayarı ve aynı Wi-Fi ağ üzerindeki bir cihaz.

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Xamarin Live Player'ı ilk kez kullanma

1. Açık **Visual Studio 2017**.
2. Git **Araçlar > Seçenekler...**  seçip **Xamarin > diğer** sekmesi.
3. Değer çizgisi **Xamarin Live Player'ı etkinleştirme**:

    ![Xamarin Live Player'ı etkinleştir kutusuna Seçenekleri penceresini denetleyin](install-images/vs2017-options.png)

4. Bir Xamarin projesi oluşturun veya açın (veya [örnek](~/tools/live-player/samples.md)).
5. Seçin **Live Player** cihaz listesindeki:

    ![Xamarin Live Player seçeneği cihaz listesini içerir](install-images/devices-empty-windows.png)

    - Bir cihaz zaten eşleştirilmiş, isteğe bağlı olarak kullanıma sunulacaktır.
    - Aksi takdirde, gerekli olduğunda bir cihaz eşlemeden istenir.

6. Basın **çalıştırma** düğmesini veya şunlardan birini seçenekleri seçin **çalıştırmak** veya sağ tıklama menüsü:

    - **Hata ayıklama olmadan Başlat** – uygulama ve değişiklikler aygıtta bakın düzenleyebilir (değişiklik yapıldıysa ve kaydettiğiniz uygulama yeniden başlatıldığında).
    - **Hata ayıklama Başlat** – kesme noktaları ayarlayın ve değişkenleri denetleyin, ancak kod düzenlenemez.

    Alternatif olarak, seçin **Araçlar > Xamarin Live Player > geçerli görünümde Canlı Çalıştır**, olanak sağlayan uygulama ve değişiklikler aygıtta bakın düzenleyin. Geçerli görünümü (uygulamanın ana ekran yerine) gösterilir.

7. Bir cihaz zaten eşleştirildi ve Xamarin Live Player uygulamasını bir cihazda çalışan kod hemen yürütecek!

    Cihazı yok ise, yönergelerini içeren bir QR kodu görünür bir cihaz eşlemeden eşleştirilmiştir:

    ![Bir cihaz penceresi eşleştirin.](install-images/manage-empty-windows.png)

    Cihaz çifti için kurulamıyorsa, bir hata ortaya çıkabilir.

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

## <a name="2-get-visual-studio-for-mac"></a>2. Mac için Visual Studio'yu edinin

Xamarin Live Player gerektirir:

- OS X 10.11, macOS 10.12 veya üzeri
- Mac için Visual Studio
- Mac ve bir cihazda aynı Wi-Fi ağı

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Xamarin Live Player'ı ilk kez kullanma

1. Açık **Mac için Visual Studio**.
2. Git **Visual Studio > Tercihler...**  seçip **projeleri > Xamarin Live Player (Önizleme)** sekmesi.
3. Değer çizgisi **Xamarin Live Player'ı etkinleştirme**:

    [![Xamarin Live Player'ı etkinleştir kutusuna Seçenekleri penceresini denetleyin](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

4. Bir Xamarin projesi oluşturun veya açın (veya [örnek](~/tools/live-player/samples.md)).
5. Seçin **Live Player** aygıt listesinde.

    ![Xamarin Live Player seçeneği cihaz listesini içerir](install-images/devices.png)

    - Bir cihaz zaten eşleştirilmiş, isteğe bağlı olarak kullanıma sunulacaktır.
    - Aksi takdirde, gerekli olduğunda bir cihaz eşlemeden istenir.
    - Seçin **Xamarin Live Player cihazları...**  Xamarin Live Player ile kullanmak istediğiniz cihazları yönetmek için.

6. Basın **çalıştırma** düğmesini veya şunlardan birini seçenekleri seçin **çalıştırmak** veya sağ tıklama menüsü:

    ![Menü seçeneklerini çalıştırın](install-images/run-menu.png)

    - **Hata ayıklama olmadan Başlat** – uygulama ve değişiklikler aygıtta bakın düzenleyebilir (değişiklik yapıldıysa ve kaydettiğiniz uygulama yeniden başlatıldığında).
    - **Hata ayıklama Başlat** – kesme noktaları ayarlayın ve değişkenleri denetleyin, ancak kod düzenlenemez.
    - **Çalıştırma geçerli görünümde canlı** – uygulama ve değişiklikler aygıtta bakın düzenleyebilirsiniz. Geçerli görünümü (uygulamanın ana ekran yerine) gösterilir.

7. Bir cihaz zaten eşleştirildi ve Xamarin Live Player uygulamasını bir cihazda çalışan kod hemen yürütecek!

    Cihaz eşleştirilmiş, bir cihaz eşlemeden yönergelerini içeren bir QR kodu görüntülenir:

    ![Bir cihaz penceresi eşleştirin.](install-images/manage-empty.png)

    Cihaz çifti için kurulamıyorsa, bir hata görünür:

    ![Cihaz hata iletisi bağlanılamıyor](install-images/error-cannot-connect.png)

-----

Tüm sorunları yaşadıklarında veya değil bağlanabilir, bakın [sınırlamalar ve sorun giderme](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Sınırlamalar](~/tools/live-player/limitations.md)
- [Sorun giderme](~/tools/live-player/troubleshooting.md)
- [Xamarin Live Player örnekleri](~/tools/live-player/samples.md)
