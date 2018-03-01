---
title: "Bir NuGet varolan kitaplık projeleri oluşturma"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 44a97114e7d325a1fa196d2c9828855ad1a30c94
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Bir NuGet varolan kitaplık projeleri oluşturma

Varolan PCL ya da .NET standart kitaplıkları NuGets içine açılabilir **proje seçenekleri** penceresi:

1. Kitaplık projeye sağ tıklayın **çözüm paneli** ve **seçenekleri**.

2. Git **NuGet paketini > meta veri** bölümünde ve tüm girin [gerekli bilgileri](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) içinde **genel** sekmesi:

  [ ![](existing-library-images/existing-metadata-sml.png "Gerekli meta veriler girin")](existing-library-images/existing-metadata.png)

3. İsteğe bağlı olarak, [ek meta veri ekleme](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) içinde **ayrıntıları** sekmesi.

4. Meta veriler yapılandırıldıktan sonra projeye sağ tıklayın ve seçin **NuGet paketi oluştur** ve **.nupkg** NuGet paket dosyasının kaydedileceği **/bin/** klasör (hata ayıklama veya yayın, yapılandırmasına bağlı olarak).

  ![](existing-library-images/create-nuget-package.png "Sağ tıklatma menüsünden NuGet paketi oluştur")

5. NuGet paketi oluşturmak için _her_ derleme veya dağıtmak, Git **NuGet paketini > Yapı** bölümü ve onay **projesi oluştururken bir NuGet paketi oluşturma**:

    [ ![](existing-library-images/existing-tickbox-sml.png "Bir NuGet paketi oluşturmak için onay")](existing-library-images/existing-tickbox.png)

> [!NOTE]
> Derleme NuGet paketi derleme işlemlerini yavaşlatabilir. Bu kutu ticked değil ise, hala bir NuGet paketi el ile (4. adımda gösterilen) proje bağlam menüsünden herhangi bir zamanda oluşturabilirsiniz.

## <a name="verifying-the-output"></a>Çıktı doğrulanıyor

Ayrıca NuGet paketlerini ZIP dosyaları olduğundan, oluşturulan paketi iç yapısı denetlemek mümkündür.

Bu ekran yalnızca tek bir PCL derleme dahil bir PCL tabanlı NuGet – içeriğini gösterir:

![](existing-library-images/nuget-output.png "NuGet paketteki dosyaları")


## <a name="related-links"></a>İlgili bağlantılar

- [Meta Veri Kılavuzu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
