---
title: Mac için Visual Studio'da Hizmetleri bağlı
description: Visual Studio mobil uygulamaları için Azure veri depolama, kimlik doğrulaması ve anında iletme bildirimleri için Mac ekleyin
ms.prod: xamarin
ms.assetid: ADDFB3A5-DB6A-417C-8ACE-33D107FB75C2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b9aaa197ccf01c59d6e4bbb0476d10295a108f89
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="connected-services-walkthrough"></a>Bağlı hizmetler gözden geçirme

Hizmet eklemek için projenizin bırakmak zorunda kalmamak için bağlantılı hizmetler iş akışı Azure portal iş akışını Visual Studio'ya Mac için getirir.

Bu kılavuzda, bulut verilerini depolama, kimlik doğrulama ve platformlar arası Xamarin.Forms taşınabilir sınıf kitaplığı (PCL) uygulamasına anında iletme bildirimleri getirir bir Azure arka uç hizmetini nasıl ekleyeceğiniz gösterilir.


1.  Başlangıç çift tıklayarak **bağlantılı Hizmetler** getirir çözüm düğümünde **Services Galerisi**.
  Bu uygulama türü için tüm kullanılabilir hizmetler listesidir. Bir hizmet seçin (gibi **Azure uygulama hizmetiyle mobil arka uç**) üzerinde tıklatarak.

  [![](connected-services-images/image001-sml.png "Mac için Visual Studio hizmetler düğümünde bağlı")](connected-services-images/image001.png#lightbox)

2. Hizmet Ayrıntıları sayfası, hizmeti ve bağımlılıklarını yüklenmesi için bir açıklama vardır.
  Tıklatın **Ekle** düğmesi bağımlılıkları uygulamaya eklemek için:

  [![](connected-services-images/image002-sml.png "Azure ile mobil arka uç")](connected-services-images/image002.png#lightbox)

3. Bağımlılıkları hem PCL ve platforma özgü projelerine çalışmaya eklenmesi gerekir.
  Hizmet (doğrudan veya dolaylı olarak) başvurur her projeye eklemek için onay kutularını seçin:

  [![](connected-services-images/image003-sml.png "Hizmet başvurması gereken tüm projeleri denetleyin")](connected-services-images/image003.png#lightbox)

4. Seçin **kabul** üzerinde **lisans kabulünü** iletişim kutuları için NuGet paketleri.
  Kabul etmek için MobileClient ve bağımlılıklar için diğeri SQLiteStore için çevrimdışı veri eşitleme için gerekli olan iki iletişim kutuları olabilir:

  [![](connected-services-images/image004-sml.png "Lisans sözleşmelerini kabul edin")](connected-services-images/image004.png#lightbox)

  ![](connected-services-images/image005.png "Lisans kabulünü penceresi")

5. Bağımlılıkları eklendikten sonra Azure ile iletişim kurmak için kullanmak istediğiniz hesapla oturum istenir.
  A Microsoft ID oturum zaten oturum açtınız, Mac için Visual Studio, Azure aboneliklerinize ve bunlarla ilişkili tüm uygulama hizmetleri getirme girişiminde bulunur. Herhangi bir aboneliğiniz yoksa, ücretsiz deneme için kaydolan veya Azure portalında abonelik planı satın tarafından ekleyebilirsiniz.

6. Bir uygulama hizmeti listeden seçin. Bu şablon kodunu doldurur `MobileServiceClient` Azure uygulama hizmeti karşılık gelen URL'sini nesnesiyle:

  [![](connected-services-images/image006-sml.png "Uygulama hizmeti listeden seçin")](connected-services-images/image006.png#lightbox)

  Listelenen hiçbir Hizmetleri varsa tıklatın **yeni** düğmesini (bkz. adım 9.)

7. Şablon kodu kopyalamak `MobileServiceClient` PCL içine. Dosya konumu uygulamada önemli olmayan, yalnızca bir örneği var olduğu sürece.
  Oluşturmak için önerilen yaklaşımdır bir `AzureService` tüm Azure etkileşimleri işler ve kullandığı sınıf `MobileServiceClient`:

  ![](connected-services-images/image007.png "Uygulamada config kod Kopyala")

8. Belgelerde izleyin **sonraki adımlar** eklemek verileri, çevrimdışı eşitleme, kimlik doğrulama ve uygulamanıza anında iletme bildirimleri için:

  [![](connected-services-images/image008-sml.png "Sonraki adımlar yönergeleri gözden geçirin")](connected-services-images/image008.png#lightbox)

10. Mevcut tüm uygulama hizmetleri yoksa, Mac için Visual Studio içinde yeni hizmetlerinden oluşturabilirsiniz.
  Tıklatın **yeni** açmak için hizmetleri listesinin altındaki sol düğmesini **yeni uygulama hizmeti** iletişim:

  [![](connected-services-images/image009-sml.png "Mac için Visual Studio'da yeni bir uygulama hizmeti oluşturma")](connected-services-images/image009.png#lightbox)

Yeni bir hizmet aşağıdaki parametreleri gerektirir:

-   **Uygulama hizmeti adını** – plan için benzersiz ad/kimliği
-   **Abonelik** – hizmet için ödeme yapmak için kullanmak istediğiniz aboneliği
-   **Kaynak grubu** – bir yol ya da bir proje için tüm Azure kaynakları düzenleme. Varolanı kullanma veya yeni bir tane oluşturmak için seçeneği. İlk Azure hizmetiniz varsa, yeni bir tane oluşturun.
-   **Hizmet planı** – kullanan herhangi bir kaynağa maliyetini ve konumunu belirler. Varolanı kullanma veya yeni bir tane oluşturmak için seçeneği. İlk Azure hizmetiniz varsa, varsayılan kullanın veya ücretsiz katmanda (F1) yeni bir tane oluşturun.

Ziyaret [Azure App Service belgeleri](https://docs.microsoft.com/azure/app-service/) daha fazla bilgi için


## <a name="related-links"></a>İlgili bağlantılar

- [Azure uygulama hizmeti](https://docs.microsoft.com/en-us/azure/app-service/)
- [Sürüm Notları](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Connected_Services)
