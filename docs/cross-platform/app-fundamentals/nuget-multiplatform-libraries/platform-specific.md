---
title: "NuGet için Yeni platforma özgü kitaplık projeleri oluşturma"
ms.topic: article
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 6512387738217259067e7b9ae8076f73b4fbeb07
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet için Yeni platforma özgü kitaplık projeleri oluşturma

İOS ve Android gibi belirli platformlar hedef birden çok platformlu kitaplık projeleri, paylaşılan projeleri ile en iyi çalışır.

NuGet, hem iOS ve Android özel kod yanı sıra .NET kodu ortak içerebilir.

Birden çok derleme oluşturulur ve tek bir NuGet paket oluşturulmuş. NuGet standartları paket Xamarin iOS ve Android projeleri gibi tüm desteklenen proje türleri için eklenebilir emin olun.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Platformlar arası kitaplığı NuGet oluşturma adımları

1. Seçin **Dosya > Yeni bir çözüm** (veya varolan bir çözümü sağ tıklatın ve seçin **Ekle > Yeni proje**).

2. Seçin **Multiplatform Kitaplığı** gelen **Multiplatform > Kitaplığı** bölümü:

  [![](platform-specific-images/mulitplatform-library-sml.png "Tek bir kod tabanının için birden çok platform kitaplığı yapılandırma")](platform-specific-images/multiplatform-library.png#lightbox)

3. Girin bir **adı** ve **açıklama**ve seçin **Platform özel**:

  [![](platform-specific-images/specific-configure-sml.png "İOS ve Android için platforma özgü kitaplığı yapılandırma")](platform-specific-images/specific-configure.png#lightbox)

4. Sihirbazı tamamlayın. Şu projeler çözüme eklendi:

  - **Android projesi** – Android özgü kodu bu proje için isteğe bağlı olarak eklenebilir.
  - **iOS projesi** – iOS özgü kodu bu proje için isteğe bağlı olarak eklenebilir.
  - **NuGet proje** – herhangi bir kodu bu projeye eklenir. Diğer projeler başvuruyor ve NuGet paketi çıktı için meta veri yapılandırmayı içerir.
  - **Proje paylaşılan** – ortak kodun içinde platforma özgü kod dahil olmak üzere, bu proje için eklenmesi `#if` derleyici yönergeleri.

5. NuGet projeye sağ tıklayın ve seçin **seçenekleri**, ardından açık **NuGet paketini > meta veri** bölümünde ve girin [gerekli meta veriler](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (iyi tüm isteğe bağlı olarak Meta veriler):

  [![](platform-specific-images/specific-metadata-sml.png "Gerekli meta veriler girin")](platform-specific-images/specific-metadata.png#lightbox)

6. Ayrıca, **proje seçenekleri** penceresi açık **başvuru derlemeleri** bölümünde ve paylaşılan kitaplık "yemi ve anahtar" destekleneceğini hangi PCL profillerini seçin:

  ![](platform-specific-images/specific-reference-assemblies.png "Ayrıca proje Seçenekleri penceresinde başvuru derlemeleri bölüm açın ve paylaşılan kitaplık yemi ve anahtarıyla destekleyecek hangi PCL profillerini seçin")

  > [!NOTE]
> "Yemi ve anahtar" PCL derlemeler (platforma özgü kodu içeremez) kitaplığı tarafından kullanıma sunulan API yalnızca içereceği anlamına gelir. NuGet Xamarin projeye eklendiğinde, paylaşılan kitaplıklar karşı PCL derlenecek ancak gerçekte iOS veya Android projesi tarafından kullanılan kod platforma özgü derlemeleri içerir.

7. Projeye sağ tıklayın ve seçin **NuGet paketi oluştur** (veya derleme ya da çözümü dağıtmak) ve **.nupkg** NuGet paket dosyasının kaydedileceği **/bin/** klasörü () hata ayıklama veya yayın yapılandırmasına bağlı olarak).

  ![](platform-specific-images/create-nuget-package.png "NuGet paket dosyasını bin klasöründe hata ayıklama veya yayın, yapılandırmasına bağlı olarak kaydedilecek")


## <a name="verifying-the-output"></a>Çıktı doğrulanıyor

Ayrıca NuGet paketlerini ZIP dosyaları olduğundan, oluşturulan paketi iç yapısı denetlemek mümkündür.

Bu ekran gösterir, iOS ve Android destekleyen ve iki başvuru derlemeleri sahip bir platforma özgü NuGet içeriğini seçili:

![](platform-specific-images/nuget-output.png "NuGet paketteki dosyaları")


## <a name="related-links"></a>İlgili bağlantılar

- [Meta Veri Kılavuzu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
