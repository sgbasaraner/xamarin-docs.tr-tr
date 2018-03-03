---
title: "Xamarin Canlı Player Kurulumu"
description: "Düzenle ve iOS veya Android cihazında gerçek zamanlı uygulamaları test etme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/22/2017
ms.openlocfilehash: f3fa2502f55ad7ebf59d015923ff4008103b08bd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-live-player-setup"></a>Xamarin Canlı Player Kurulumu

Bu değişiklikleri Canlı cihazda yansıtılmasını ve Xamarin Canlı Player, uygulamanızın Canlı düzenlemeler yapmanıza olanak sağlar. Kodunuz Xamarin Canlı Player uygulama içinde – Öykünücüler ayarlamak veya dağıtmak için kablolar kullanmak için gerekli çalışır! Bu makalede, Xamarin Canlı Player ayarlamak açıklar.

![Önizleme özelliği](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1. Uygulamayı Al

### <a name="xamarin-live-player-for-android"></a>Android için Xamarin Canlı Player
Google Play'de Android için Xamarin Canlı Player kullanılabilir:

[ ![Google play'de kullanılabilir](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Google Play gerek kalmadan Android cihazlar için Xamarin Canlı Player aracılığıyla kullanılabilir [HockeyApp](https://aka.ms/xlp-hockeyapp) dağıtım. Android için seçim tarafından doğrudan Google Play'den yüklenebilir için erken Önizleme ek olarak, derlemeler [açık beta programı](https://play.google.com/apps/testing/com.xamarin.live)

### <a name="xamarin-live-player-for-ios"></a>İOS için Xamarin Canlı Player
Katılmak için kullanıcıların öneririz [Xamarin Canlı Player uygulama _iOS Önizleme_ ](https://aka.ms/liveplayeralpha) keyfini TestFlight üzerinden yapılan en son geliştirmeleri hızlı erişim için.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="2-get-visual-studio-2017-preview-on-windows-or-for-mactabsvsmac"></a>2. Visual Studio 2017 önizlemesini Windows alın (veya [Mac için](?tabs=vsmac))

Xamarin Canlı Player gerektirir:

- Visual Studio 2017 [15.4](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/#visualstudio2017) ya da daha yeni.
- Visual Studio bilgisayar ve aygıt aynı WiFi ağda

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Xamarin Canlı Player'ı ilk kez kullanma

1. Açık **Visual Studio 2017**.
2. Git **Araçlar > Seçenekler...**  seçip **Xamarin > diğer** sekmesi.
3. Değer çizgilerinin **etkinleştirmek Xamarin Canlı Player**:

  ![Seçenekleri penceresinde Xamarin Canlı Player etkinleştir kutusunu işaretleyin](install-images/vs2017-options.png)

2. Xamarin projesi oluşturun veya açın (veya bir [örnek](~/tools/live-player/samples.md)).
3. Seçin **Canlı Player** aygıt listesinde:

  ![Cihaz listesi bir Xamarin Canlı Player seçenek içerir](install-images/devices-empty-windows.png)

  * Bir aygıt zaten eşleştirilmiş, isteğe bağlı olarak kullanılabilir olacaktır.
  * Aksi takdirde gerektiğinde bir aygıtı eşleştirin istenir.
4. Tuşuna **çalıştırmak** düğmesini veya aşağıdaki seçeneklerden birin belirleyin **çalıştırmak** ya da sağ tıklatma menüsünden:

  - **Hata ayıklama olmadan Başlat** – uygulama ve değişiklikler cihazda bakın düzenleyebilirsiniz (uygulama yeniden değişiklik yapıldıysa ve kaydettiğiniz gibi).
  - **Hata ayıklama Başlat** – kesme ve değişkenleri incelemek ancak kod düzenlenemez.

  Alternatif olarak, seçin **Araçlar > Xamarin Canlı Player > Canlı geçerli görünümü çalıştırmak**, uygulama ve değişiklikler cihazda bakın düzenleme seçmenizi sağlar. Geçerli görünümü (uygulamanın ana ekran yerine) gösterilir.

5. Bir aygıt zaten eşleştirilmiş ve Xamarin Canlı Player uygulama cihaz üzerinde çalışırken, kod hemen yürütecek!

  Hiçbir aygıt ise, bir QR kodu yönergeler görünür bir aygıtı eşleştirmeye eşleştirilmiş:

    ![Çifti aygıt penceresi](install-images/manage-empty-windows.png)

  Cihaz çifti için kurulamıyorsa hata görünebilir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="2-get-visual-studio-for-mac-or-for-windowstabsvswin"></a>2. Mac için Visual Studio Al (veya [Windows için](?tabs=vswin))

Xamarin Canlı Player gerektirir:

- OS X 10.11 sürümünü, macOS 10.12 veya daha büyük
- Mac için Visual Studio
- Mac ve aynı WiFi ağ üzerindeki bir aygıt

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. Xamarin Canlı Player'ı ilk kez kullanma

1. Açık **Mac için Visual Studio**.
2. Git **Visual Studio > tercihleri...**  seçip **projeleri > Xamarin Canlı Player (Önizleme)** sekmesi.
3. Değer çizgilerinin **etkinleştirmek Xamarin Canlı Player**:

  [ ![Seçenekleri penceresinde Xamarin Canlı Player etkinleştir kutusunu işaretleyin](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png)

2. Xamarin projesi oluşturun veya açın (veya bir [örnek](~/tools/live-player/samples.md)).
3. Seçin **Canlı Player** aygıt listesinde.

  ![Cihaz listesi bir Xamarin Canlı Player seçenek içerir](install-images/devices.png)

  * Bir aygıt zaten eşleştirilmiş, isteğe bağlı olarak kullanılabilir olacaktır.
  * Aksi takdirde gerektiğinde bir aygıtı eşleştirin istenir.
  * Seçin **Xamarin Canlı Player aygıtları...**  Xamarin Canlı Player ile kullanmak istediğiniz aygıtları yönetme.

4. Tuşuna **çalıştırmak** düğmesini veya aşağıdaki seçeneklerden birin belirleyin **çalıştırmak** ya da sağ tıklatma menüsünden:

  ![Menü seçeneklerini çalıştırın](install-images/run-menu.png)

  - **Hata ayıklama olmadan Başlat** – uygulama ve değişiklikler cihazda bakın düzenleyebilirsiniz (uygulama yeniden değişiklik yapıldıysa ve kaydettiğiniz gibi).
  - **Hata ayıklama Başlat** – kesme ve değişkenleri incelemek ancak kod düzenlenemez.
  - **Geçerli Görünüm Çalıştır canlı** – uygulama ve değişiklikler cihazda bakın düzenleyebilirsiniz. Geçerli görünümü (uygulamanın ana ekran yerine) gösterilir.

5. Bir aygıt zaten eşleştirilmiş ve Xamarin Canlı Player uygulama cihaz üzerinde çalışırken, kod hemen yürütecek!

  Hiçbir aygıt eşleştirilmiş, bir QR kodu bir aygıtı eşleştirin konusunda yönergeler görünür:

    ![Çifti aygıt penceresi](install-images/manage-empty.png)

  Cihaz çifti için kurulamıyorsa bir hata görünür:

    ![Aygıt hata iletisine bağlanamıyor](install-images/error-cannot-connect.png)


-----

Herhangi bir sorunla karşılaşırsanız, bkz: [sınırlamalar ve sorun giderme](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Sınırlamalar](~/tools/live-player/limitations.md)
- [Sorun giderme](~/tools/live-player/troubleshooting.md)
- [Xamarin Canlı Player örnekleri](~/tools/livehttps://developer.xamarin.com/samples.md)
