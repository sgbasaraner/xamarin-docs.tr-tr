---
title: Paylaşım kod genel bakış
description: 'Bu belge, platformlar arası projeler arasında kod paylaşımı için farklı yöntemler karşılaştırır: paylaşılan proje, taşınabilir sınıf kitaplıkları ve avantajları ve dezavantajları dahil olmak üzere .NET Standard.'
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 82a73619e4c0507e8857cc91d88ababa870013de
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270478"
---
# <a name="sharing-code-overview"></a>Paylaşım kod genel bakış

_Bu belge, platformlar arası projeler arasında kod paylaşımı için farklı yöntemler karşılaştırır: .NET standart, paylaşılan projeler ve taşınabilir sınıf kitaplıkları, avantajları ve dezavantajları dahil._

Platformlar arası uygulamalar arasında kod paylaşımı için üç yöntem vardır:

- [**.NET standart kitaplıkları** ](#Net_Standard) – .NET Standard projeleri birden çok platform genelinde paylaşılan kod uygulayabilir ve çok sayıda .NET API'leri (sürüm) bağlı olarak erişebilir. .NET Standard 2.0 en iyi kapsamını sağlarken API'leri, .NET standart 1.0 1.6 uygulamak giderek büyüyen ayarlar.
- [**Paylaşılan projeleri** ](#Shared_Projects) – kaynak kodunuzu düzenleme şeklinizdir ve paylaşılan varlık proje türü kullanmak `#if` platforma özgü gereksinimleri yönetmek için gerekli olarak derleyici yönergeleri.
- [**Taşınabilir sınıf kitaplıkları** ](#Portable_Class_Libraries) (kullanım dışı) – taşınabilir sınıf kitaplıkları (PCLs) ortak bir API yüzeyi ile birden fazla platformu hedefleyin ve platforma özel işlevsellik sağlamak için arabirimleri kullanır. Visual Studio'nun en son sürümlerde PCLs kullanım dışı &ndash; bunun yerine .NET Standard kullanın.

Bir kod paylaşımı stratejisi amacı mimarisi Bu diyagramda gösterilen tek bir kod temeli tarafından birden çok platform burada yararlanılabilir desteklemektir.

 ![Paylaşılan kod uygulama mimarisi](code-sharing-images/conceptualarchitecture.png "paylaşılan kod uygulama mimarisi")

Bu makalede, uygulamalarınız için doğru proje türünü seçmenize yardımcı olmak için kullanılabilen yöntemler karşılaştırır.

<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>.NET standart kitaplıkları

[.NET standard](~/cross-platform/app-fundamentals/net-standard.md) kitaplıkları, Xamarin.Android ve Xamarin.iOS gibi platformlar arası projeleri dahil olmak üzere, farklı proje türlerinde başvurulabilir taban sınıfı kitaplıkları iyi tanımlı bir dizi sağlar. .NET standard 2.0 ile mevcut .NET Framework kodu en büyük uyumluluk için önerilir.

![.NET standard diyagram](code-sharing-images/netstandard.png ".NET Standard diyagramı")

### <a name="benefits"></a>Yararları

- Birden çok projede kod paylaşmanıza olanak tanır.
- Yeniden düzenleme işlemleri, her zaman etkilenen tüm başvuruları güncelleştirin.
- PCL profilleri daha büyük bir yüzey alanını .NET temel sınıf kitaplığı (BCL) kullanılabilir. Özellikle, .NET Standard 2.0 neredeyse aynı API yüzeyi olarak .NET Framework vardır ve yeni uygulamalar ve taşıma mevcut PCLs için önerilir.

### <a name="disadvantages"></a>Dezavantajlar

- Derleyici yönergeleri gibi kullanamazsınız `#if __IOS__`.

### <a name="remarks"></a>Açıklamalar

.NET standard olan [PCL benzer](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries), ancak platform desteği ve daha fazla sayıda BCL sınıflardan için daha basit bir model.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>Paylaşılan projeler

[Paylaşılan projeleri](~/cross-platform/app-fundamentals/shared-projects.md) kod dosyaları ve onlara başvuran bir projeye dahil varlıkları içerir. Paylaşım projeleri kendi derlenmiş çıktı üretmez.

(Android, iOS ve Windows için) üç uygulama projeleri içeren bir çözüm dosyası bu ekran gösteren bir **paylaşılan** genel C# kaynak kodu dosyaları içeren proje:

![Paylaşılan proje çözüm](code-sharing-images/sharedsolution.png "paylaşılan proje çözüm")

Kavramsal mimaris burada her proje tüm paylaşılan kaynak dosyalarını içeren Aşağıdaki diyagramda gösterilmiştir:

![Paylaşılan proje diyagram](code-sharing-images/sharedassetproject.png "paylaşılan proje diyagramı")

### <a name="example"></a>Örnek

İOS, Android ve Windows destekleyen bir platformlar arası uygulama her platform için bir uygulama projesi gerektirir. Ortak kodun paylaşılan projede bulunur.

Örnek bir çözüm, aşağıdaki klasörleri ve projeleri (proje adları seçtiniz anlamlılık için projelerinizi aşağıdaki adlandırma yönergeleri izleyin gerekmez) içermesi gerekir:

- **Paylaşılan** – paylaşılan tüm projeler için ortak kod içeren proje.
- **AppAndroid** – Xamarin.Android uygulama projesi.
- **AppiOS** – Xamarin.iOS uygulama projesi.
- **AppWindows** – Windows uygulama projesi.

Bu şekilde üç uygulama projeleri aynı kaynak kodunun (C# dosyaları paylaşılan) paylaşıyor. Paylaşılan kodun herhangi bir düzenleme üç tüm projeler arasında paylaşılır.

### <a name="benefits"></a>Yararları

- Birden çok projede kod paylaşmanıza olanak tanır.
- Paylaşılan kod derleyici yönergeleri (örn. kullanarak platform temelinde dallandırılmış kullanarak `#if __ANDROID__` bölümünde açıklandığı gibi [Çapraz Platform uygulamaları oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) belge).
- Uygulama projeleri, paylaşılan kod kullanabilir platforma özgü başvurular içerebilir (kullanma gibi `Community.CsharpSqlite.WP7` Tasky örnekte Windows Phone için).

### <a name="disadvantages"></a>Dezavantajlar

- 'İnactive' derleyici yönergeleri içinde kod etkileyen düzenlemeler, bu yönergeleri içinde kod güncelleştirmez.
- Diğer çoğu proje türlerinin aksine, paylaşılan bir proje yok 'output' derleme sahiptir. Derleme sırasında dosyaları başvurulan projenin bir parçası kabul edilir ve bu bütünleştirilmiş kod içine derlenmiş. Bir derleme olarak kodunuzu paylaşmak istiyorsanız, daha sonra .NET Standard veya taşınabilir sınıf kitaplıkları daha iyi bir çözüm sunulur.

<a name="Shared_Remarks" />

### <a name="remarks"></a>Açıklamalar

Yalnızca kendi uygulamaları içinde paylaşmak (ve diğer geliştiricilere dağıtmak için değil) için tasarlanmıştır kod yazan uygulama geliştiriciler için iyi bir çözümdür.

<a name="Portable_Class_Libraries" />

## <a name="portable-class-libraries"></a>Taşınabilir sınıf kitaplıkları

> [!TIP]
> .NET standard 2.0 kitaplıkları taşınabilir sınıf kitaplıkları önerilir.

Taşınabilir sınıf kitaplıkları vardır ve [aşağıda ayrıntılı olarak ele alınan](~/cross-platform/app-fundamentals/pcl.md).

![Taşınabilir sınıf kitaplığı diyagramı](code-sharing-images/portableclasslibrary.png "taşınabilir sınıf kitaplığı diyagramı")

### <a name="benefits"></a>Yararları

- Birden çok projede kod paylaşmanıza olanak tanır.
- Yeniden düzenleme işlemleri, her zaman etkilenen tüm başvuruları güncelleştirin.

### <a name="disadvantages"></a>Dezavantajlar

- Visual Studio'nun en son sürümlerinde kullanımdan kaldırıldı, .NET standart kitaplıkları yerine önerilir. Bu [farklar açıklaması](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries) PCL ve .NET Standard arasında.
- Derleyici yönergeleri kullanamazsınız.
- Yalnızca seçilen profili tarafından belirlenen .NET framework'ün bir alt kümesi kullanmak kullanılabilir. (bkz [PCL giriş](~/cross-platform/app-fundamentals/pcl.md) daha fazla bilgi için).

### <a name="remarks"></a>Açıklamalar

PCL şablonu Visual Studio'nun en son sürümlerde kullanım dışı olarak kabul edilir.

## <a name="summary"></a>Özet

Kod paylaşımı stratejisi seçtiğiniz hedeflediğiniz platformlar tarafından temelli. Projeniz için en uygun bir yöntem seçin.

.NET standard paylaşılabilir kod kitaplıkları (NuGet üzerinde özellikle yayımlama) oluşturmaya yönelik en iyi seçenektir. Paylaşılan projeler iyi platformlar arası uygulamalarında platforma özgü işlevi çok sayıda kullanmayı planladığınız uygulama geliştiricileri için çalışır.

PCL projeleri Visual Studio'da desteklenmeye devam ederken, .NET Standard yeni projeler için önerilir.

## <a name="related-links"></a>İlgili bağlantılar

- [Çapraz Platform uygulamaları oluşturma (ana belge)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Taşınabilir Sınıf Kitaplıkları](~/cross-platform/app-fundamentals/pcl.md)
- [Paylaşılan Projeler](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Örnek Olay İncelemesi: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky örnek (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [PCL (github) kullanarak tasky örnek](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
