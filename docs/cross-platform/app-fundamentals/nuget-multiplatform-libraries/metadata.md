---
title: NuGet meta verileri düzenleme
description: Birden çok platformlu kitaplıkları NuGet meta verileri düzenlemek için Proje seçenekleri kullanın
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 6d30f564d54b96d358d37059f9dababaf8f3314e
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="editing-nuget-metadata"></a>NuGet meta verileri düzenleme

_Birden çok platformlu kitaplıkları NuGet meta verileri düzenlemek için Proje seçenekleri kullanın_

Kitaplığı proje türleri (örneğin, PCL veya .NET standart veya yeni NuGet proje türü) sahip bir **NuGet paketi** bölümüne **proje seçenekleri** penceresi.

**Meta veri** bölümünde kullanılan değerleri yapılandırır [ **.nuspec** NuGet paket bildirim dosyası](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Gerekli bilgiler

**Genel** sekmesinde bir NuGet paketi oluşturmak için girilmelidir dört alanlar içeriyor:

[![](metadata-images/metadata-general-sml.png "NuGet paket gerekli meta veriler penceresi")](metadata-images/metadata-general.png#lightbox)

- **Kimliği** – Nuget.org (veya paket yerde dağıtılacak içinde) benzersiz olmalıdır paket tanımlayıcısı. İzleyin [Kılavuzu](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) ve yalnızca bir URL geçerli karakterler kullanın (boşluk yok ve çoğu özel karakteri kaçının).
- **Sürüm** – ile tutarlı bir sürüm numarası seçebilir [NuGet sürümü oluşturma kuralları](https://docs.microsoft.com/nuget/create-packages/dependency-versions).
- **Yazarları** – adlarının virgülle ayrılmış listesi.
- **Açıklama** – kullanıcılar paket seçerken görüntülenen özelliklerine genel bakış paketin.

> [!NOTE]
> Dağıtım NuGet veya diğer kullanıcılar için yeni sürümler oluştururken sürüm numarasını Artır unutmayın.

Daha fazla bilgi için bkz: [gereken öğeleri başvurusu](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) bunlar hakkında ayrıntılı yönergeler gibi daha fazla bilgi için de [benzersiz paket tanımlayıcısı seçme ve sürüm numarasını ayarlama](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) ve [ Ayar paket türü](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Bu sekmedeki tüm alanların girilmesi gerekir; Aksi takdirde bir hata iletisi görüntülenir: _"bir NuGet paketi oluşturulmayacak şekilde projenin NuGet meta verileri yok. NuGet paket meta verileri proje seçenekleri meta veri bölümünde belirtilebilir "_

## <a name="optional-metadata"></a>İsteğe bağlı meta veriler

**Ayrıntıları** sekmesi NuGet paket bildirim dosyasında eklenecek isteğe bağlı alanları içerir.

[![](metadata-images/metadata-detail-sml.png "NuGet paketi isteğe bağlı meta veriler penceresi")](metadata-images/metadata-detail.png#lightbox)

Başvurmak [isteğe bağlı öğeleri başvurusu](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) gerekli ve isteğe bağlı alanları hakkında daha fazla bilgi.

> [!NOTE]
> Üzerinde NuGet paketi dağıtılıyor, [NuGet.org](https://www.nuget.org) mümkün olduğunca fazla bilgi sağlamak için önerilir.


## <a name="related-links"></a>İlgili bağlantılar

- [.nuspec başvurusu](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
