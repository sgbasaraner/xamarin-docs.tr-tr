---
title: NuGet için yeni bir çok platformlu kitaplığı oluşturma
description: Bu belge, NuGet ile kullanım için birden çok platformlu bir kitaplık oluşturmayı açıklar. Bu yöntem, iş mantığı ve tamamen .NET temel sınıf kitaplığı'nda ifade ve böylece platforma özgü kodu olmadan tüm hedef platformlar üzerinde çalışacak algoritmalar için uygundur.
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b59450ac949bffdf927475598d3678564f09f8cf
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781334"
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>NuGet için yeni bir çok platformlu kitaplığı oluşturma

PCL kullanan bir Multiplatform kitaplığı projesi oluşturma veya elde edilen NuGet ASP.NET projeleri veya WinForms, WPF veya UWP kullanarak Masaüstü uygulamaları dahil olmak üzere hedef profilini destekleyen herhangi bir .NET projesine eklenebilir .NET standart anlamına gelir.

Kitaplık yalnızca seçilen PCL ya da .NET standart profil yanı sıra tarafından eklenen diğer NuGets desteklenen kodu içerebilir.
Bu, iş mantığı ve tamamen .NET temel sınıf kitaplığı'nda ifade algoritmaları için uygundur.

Tek bir derleme oluşturulur ve bir NuGet paketine oluşturulmuş.

Platforma özgü işlevselliği, daha sonra gerekirse [platforma özgü projeleri eklenebilir](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Birden çok platformlu kitaplığı NuGet oluşturma adımları

1. Seçin **Dosya > Yeni bir çözüm** (veya varolan bir çözümü sağ tıklatın ve seçin **Ekle > Yeni proje**).

2. Seçin **Multiplatform Kitaplığı** gelen **Multiplatform > Kitaplığı** bölümü:

  [![](single-codebase-images/mulitplatform-library-sml.png "Tek bir kod tabanının için birden çok platform kitaplığı yapılandırma")](single-codebase-images/mulitplatform-library.png#lightbox)

3. Girin bir **adı** ve **açıklama**ve seçin **tüm platformlar için tek**:

  [![](single-codebase-images/single-configure-sml.png "Tek bir kod tabanının için birden çok platform kitaplığı yapılandırma")](single-codebase-images/single-configure.png#lightbox)

4. Sihirbazı tamamlayın. Bir tek kitaplığı projesi çözümde oluşturulur.

5. Yeni Kitaplık projeye sağ tıklayın ve ardından **seçenekleri**. **Yapı > Genel** bölümü sağlar **hedef Framework** ayarlanacak – bir .NET taşınabilir PCL profili ya da .NET standart sürümünü seçin:

  [![](single-codebase-images/single-choose-type-sml.png "PCL veya .NET standart kitaplık türünü seçin")](single-codebase-images/single-choose-type.png#lightbox)

6. Ayrıca, **proje seçenekleri** penceresi açık **NuGet paketi > meta veri** bölümünde ve girin [gerekli meta veriler](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (yanı sıra tüm isteğe bağlı meta veriler):

  [![](single-codebase-images/single-metadata-sml.png "Gerekli meta veriler girin")](single-codebase-images/single-metadata.png#lightbox)

7. Kitaplık projeye sağ tıklayın ve seçin **NuGet paketi oluştur** (veya derleme ya da çözümü dağıtmak) ve **.nupkg** NuGet paket dosyasının kaydedileceği **/bin/** klasör (hata ayıklama veya yayın yapılandırmasına bağlı olarak):

  ![](single-codebase-images/create-nuget-package.png "NuGet paket dosyasını bin klasöründe hata ayıklama veya yayın, yapılandırmasına bağlı olarak kaydedilecek")


## <a name="verifying-the-output"></a>Çıktı doğrulanıyor

Ayrıca NuGet paketlerini ZIP dosyaları olduğundan, oluşturulan paketi iç yapısı denetlemek mümkündür.

Bu ekran yalnızca tek bir PCL derleme dahil bir PCL tabanlı NuGet – içeriğini gösterir:

![](single-codebase-images/nuget-output.png "NuGet paketteki dosyaları")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>Platforma özgü kod ekleme

PCL tabanlı projeleri ve .NET standart tabanlı projeler platforma özgü başvurular (örneğin, iOS veya Android işlevselliği) içeremez.

Varolan bir PCL veya .NET standart projesi platforma özgü kodu içerecek şekilde genişletilmiş gerekiyorsa, bu projeye sağ tıklayıp seçerek yapılabilir **Ekle > Platform uygulaması Ekle...** :

[![](single-codebase-images/add-later-sml.png "Platform uygulaması menü ekleme")](single-codebase-images/add-later.png#lightbox)

Bir veya daha fazla platform projeleri çözüme eklenebilir ve mevcut PCL ya da .NET standart kitaplığını isteğe bağlı olarak paylaşılan bir projeye dönüştürülebilir:

[![](single-codebase-images/add-later-platforms-sml.png "İOS, Android ve paylaşılan proje gibi platformu seçenekleri ekleme")](single-codebase-images/add-later-platforms-sml.png#lightbox)

Paylaşılan bir projeye dönüştürdükten sonra ziyaret **proje Seçenekleri > NuGet paketini > başvuru derlemeleri**
[bölüm](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) ve gereken tüm profiller seçilir emin olun (böylece NuGet içinde daha önce kullanılmış projeleri ile uyumlu olacak şekilde devam eder).


## <a name="related-links"></a>İlgili bağlantılar

- [Meta Veri Kılavuzu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
