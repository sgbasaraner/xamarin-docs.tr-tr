---
title: Hedef Çerçeve Xamarin.Mac için
description: Bu makalede, bir hedef çerçeveyi (temel sınıf kitaplıkları) Xamarin.Mac için kullanılabilir ve Xamarin.Mac projenizde kullanarak etkilerini kapsar.
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 28d312ae10ce736a1720384fe76714910c3ff8f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792508"
---
# <a name="target-framework-for-xamarinmac"></a>Hedef Çerçeve Xamarin.Mac için

_Bu makalede, bir hedef çerçeveyi (temel sınıf kitaplıkları) Xamarin.Mac için kullanılabilir ve Xamarin.Mac projenizde kullanarak etkilerini kapsar._

![Hedef framework seçeneklerini Xamarin.Mac](target-framework-images/select-target.png "Target Xamarin.Mac framework seçenekleri")

## <a name="background"></a>Arka Plan

Her .NET program veya kitaplık temel sınıf kitaplığı (BCL) tarafından sağlanan işlevselliği bağlıdır. Bu BCL mscorlib, sistem, System.Net.Http ve System.Xml gibi tüm .NET dillere yerleşik ortak işlevsellik sağlayan derlemeleri içerir.

Yıllar içinde birden çok farklı sürümlerini farklı kullanım durumları için en iyi duruma getirilmiş bu BCL var. geliştirdik. "Masaüstü" BCL uygulama ayak izini azaltmak için kullanılmayan kod kaldırır daha zengin bir mobil API'leri bağlama için güvenli sağlamaya odaklanmakla birlikte, diğer kullanım örnekleri için çok ağır olabilir kitaplıkları kümesi içerir.

Bu farklı bir hedef çerçeveyi, daha önemli varsa biri, belirli bir programın derlemelerde tümünün *gerekir* hedef uyumlu BCL derlemeler. Bu durumda değilse, iki derleme karşı farklı sürümlerini bağlı olabilir **System.dll** belirli bir türde imza hakkında disagreeing. Paylaşılan bir kitaplık olabilir ya da hedef [.NET standart 2](https://blog.xamarin.com/share-code-net-standard-2-0/), ortak bir alt kümesini bir hedef çerçeveyi veya belirli hedef framework olduğu.

Her biri farklı avantajları ve dengelerin Xamarin.Mac için kullanılabilir hedef çerçevesi için üç seçenek vardır:

- **Modern** (Mobile eski belgelerinde denir) – yüksek performans ve boyutu için ayarlanmış hangi üssünü Xamarin.iOS, çok benzer bir alt. Bu hedef çerçevesi bağlayıcı güvenli olduğundan, kullanılmayan kodu kaldırarak önemli ölçüde azaltılmış kendi son ayak bu projeleri olabilir.

- **Tam** (XM 4.5 eski belgelerinde denir) – birkaç küçük kaldırmalar ile "Masaüstü" BCL için çok benzer bir alt. Hedef Framework'ü net45 (ve daha sonra) neredeyse aynı olduğu gibi kolayca ya da netstandard2 sağlamaz, birçok nugets kullanabilir veya belirli Xamarin.Mac oluşturur. Ancak, System.Configuration kullanımı nedeniyle bağlama ile uyumlu değil.

- **Desteklenmeyen** (sistem eski belgelerinde – yerine olarak adlandırılır) Xamarin.Mac tarafından sağlanan bir BCL bağlanma geçerli sistem mono kullanın. Bu, tam (örneğin System.Drawing) bazı sorunlu olarak bilinen dahil olmak üzere derlemeler kümesini sağlar. Bu seçeneği yalnızca mevcut bir "son çare" sahiptir ve diğer seçenekleri kullanmadan önce tüketmesine önemle önerilir. Adından da anlaşılacağı gibi kullanım resmi destek kanalları tarafından desteklenmiyor.

## <a name="setting-the-target-framework"></a>Hedef Framework'ü ayarlama

Bir Xamarin.Mac proje için hedef Framework türünü değiştirmek için aşağıdakileri yapın:

1. Xamarin.Mac proje Mac için Visual Studio'da açın
2. İçinde **Çözüm Gezgini**, proje dosyasına çift tıklayarak açın **proje seçenekleri** iletişim kutusu.
3. Gelen **genel** sekmesinde, türünü seçin **hedef Framework** uygulamanızın gereksinimlerine uygun:

  [![Bir hedef Framework'ü seçmek için Proje Seçenekleri penceresini kullanarak](target-framework-images/select-target-full.png "bir hedef Framework'ü seçmek için Proje Seçenekleri penceresini kullanma")](target-framework-images/select-target-full-large.png#lightbox)

4. Tıklatın **Tamam** yaptığınız değişiklikleri kaydetmek için düğmesi.

Yapmanız gerekenler **temiz** ve ardından **yeniden** Xamarin.Mac projenizin hedef çerçevesini türü değiştirme sonra.

## <a name="summary"></a>Özet

Bu makalede hedef çerçeveyi (temel sınıf kitaplıkları) Xamarin.Mac uygulamaya ve her tür framework'ün kullanılması gereken kullanılabilen farklı türde kısaca ele.


## <a name="related-links"></a>İlgili bağlantılar

- [iOS ve Mac kod paylaşımını](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [Taşınabilir Sınıf Kitaplıkları](~/cross-platform/app-fundamentals/pcl.md)
- [Bütünleştirilmiş kodlar](~/cross-platform/internals/available-assemblies.md)
- [Mevcut Mac uygulamaları güncelleştirme](~/cross-platform/macios/unified/updating-mac-apps.md)
