---
title: Bir NuGet varolan kitaplık projeleri oluşturma
description: Bu belge, bir NuGet paketi diğer geliştiricilere paylaşılması için kodu izin vererek mevcut bir kitaplık projesini oluşturmak açıklar.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 7f407b22d1793d585ae40aeae8c2d9b7616784e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34779989"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Bir NuGet varolan kitaplık projeleri oluşturma

Varolan PCL ya da .NET standart kitaplıkları NuGets içine açılabilir **proje seçenekleri** penceresi:

1. Kitaplık projeye sağ tıklayın **çözüm paneli** ve **seçenekleri**.

2. Git **NuGet paketini > meta veri** bölümünde ve tüm girin [gerekli bilgileri](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) içinde **genel** sekmesi:

  [![](existing-library-images/existing-metadata-sml.png "Gerekli meta veriler girin")](existing-library-images/existing-metadata.png#lightbox)

3. İsteğe bağlı olarak, [ek meta veri ekleme](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) içinde **ayrıntıları** sekmesi.

4. Meta veriler yapılandırıldıktan sonra projeye sağ tıklayın ve seçin **NuGet paketi oluştur** ve **.nupkg** NuGet paket dosyasının kaydedileceği **/bin/** klasör (hata ayıklama veya yayın, yapılandırmasına bağlı olarak).

  ![](existing-library-images/create-nuget-package.png "Sağ tıklatma menüsünden NuGet paketi oluştur")

5. NuGet paketi oluşturmak için _her_ derleme veya dağıtmak, Git **NuGet paketini > Yapı** bölümü ve onay **projesi oluştururken bir NuGet paketi oluşturma**:

    [![](existing-library-images/existing-tickbox-sml.png "Bir NuGet paketi oluşturmak için onay")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Derleme NuGet paketi derleme işlemlerini yavaşlatabilir. Bu kutu ticked değil ise, hala bir NuGet paketi el ile (4. adımda gösterilen) proje bağlam menüsünden herhangi bir zamanda oluşturabilirsiniz.

## <a name="verifying-the-output"></a>Çıktı doğrulanıyor

Ayrıca NuGet paketlerini ZIP dosyaları olduğundan, oluşturulan paketi iç yapısı denetlemek mümkündür.

Bu ekran yalnızca tek bir PCL derleme dahil bir PCL tabanlı NuGet – içeriğini gösterir:

![](existing-library-images/nuget-output.png "NuGet paketteki dosyaları")


## <a name="related-links"></a>İlgili bağlantılar

- [Meta Veri Kılavuzu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
