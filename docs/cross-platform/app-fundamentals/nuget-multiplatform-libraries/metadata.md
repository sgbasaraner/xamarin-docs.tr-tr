---
title: NuGet meta verileri düzenleme
description: Birden çok platformlu kitaplıkları NuGet meta verileri düzenlemek için Proje seçenekleri kullanın
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: dc2bd64339aa3deacd640cc982af5ee52d76ff07
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
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
