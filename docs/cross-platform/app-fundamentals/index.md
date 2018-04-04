---
title: Uygulama Temelleri
description: Uygulama kavramları
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: f5bd66cfcfb6ee06abac7bec9151e7325ebb32a2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="application-fundamentals"></a>Uygulama Temelleri

Bu bölümde bazı daha yaygın şeyler görevleri veya geliştiriciler mobil uygulamaları geliştirirken bilmeniz gereken kavramlar hakkında bir kılavuz sağlar.

##  <a name="building-cross-platform-applicationscross-platformapp-fundamentalsbuilding-cross-platform-applicationsindexmd"></a>[Platformlar Arası Uygulamalar Oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)

Xamarin seçme ve mobil uygulamalarınızı tasarlayıp zaman birkaç aklınızda tutarak, mobil platformlarda paylaşımı inanılmaz kod unutmayın, pazara, süresini azaltabilir, varolan yeteneğiniz yararlanın, mobil erişim için Müşteri talebi karşılamak, ve platformlar arası karmaşıklığı azaltın. &nbsp;Bu belge aşağıdaki avantajları yardımcı programı ve üretkenlik uygulamaları için kullanıldıklarını için önemli bilgiler özetlenmiştir.

## <a name="code-sharing-optionscode-sharingmd"></a>[Kod Paylaşma Seçenekleri](code-sharing.md)

Taşınabilir sınıf kitaplıkları (PCLs), paylaşılan projeleri ve .NET standart kitaplıkları dahil Xamarin projeleri için kullanılabilir seçenekleri paylaşımı farklı kodu hakkında bilgi edinin.


## <a name="accessibilityaccessibilitymd"></a>[Erişilebilirlik](accessibility.md)

Erişilebilir uygulamalar oluşturmaya yönelik ipuçları.


## <a name="localizationlocalizationmd"></a>[Yerelleştirme](localization.md)

Yerel ayar kullanan uygulamalar yapmak için yönergeleri birden çok dillere çevrilebilir.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Taşınabilir Sınıf Kitaplıkları](~/cross-platform/app-fundamentals/pcl.md)

Taşınabilir Sınıf Kitaplığı projelerinde derleme ve birden fazla platformda çalışacak paylaşılan kodu içeren derlemeler dağıtmak olanak tanır. Taşınabilir sınıf kitaplığı (veya "PCL") oluşturmak için önce hedef sonra bu platformlar için tanımlanan profildeki kullanılabilir .NET Framework'ün bir alt kümesi karşı kod yazmak için hangi platformlarını seçin. Bu belge, oluşturma ve Xamarin ile PCLs kullanma açıklar.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Paylaşılan Projeler](~/cross-platform/app-fundamentals/shared-projects.md)

Paylaşılan projeleri, bir dizi farklı uygulama projeleri tarafından başvurulan ortak kod yazmanıza olanak tanır. Kod başvuruda bulunan her proje bir parçası olarak derlenmiş ve paylaşılan kod Base platforma özgü işlevselliğine sahiptir yardımcı olmak için derleyici yönergeleri dahil edebilirsiniz. Bu makalede, paylaşılan projeleri nasıl çalıştığı ve nasıl oluşturulacağı ve Xamarin projeleriyle kullanmak anlatılmaktadır.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standart kod platformları arasında paylaşmak için yeni bir seçenektir. Taşınabilir sınıf kitaplıkları için benzer bir şekilde çalışır; kod karşı belirli bir sürümü (1.0 1.6 aracılığıyla şu anda) oluşturulur ve ardından bu düzey desteği diğer projeler tarafından tüketilen ya da daha yüksek olabilir. .NET standart projeleri Mac için Xamarin Studio 6.2, Windows için Visual Studio ve Visual Studio içinde desteklenir

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet projeler: kod paylaşımını için birden çok platformlu kitaplıkları](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet paketleri otomatik olarak PCL ya da .NET standart projelerden oluşturulabilir; ve paylaşılan projeleri ayrı NuGet proje türü kullanarak "yemi ve anahtar" NuGet paketlerine paketlenmiş. Bu bölümde her kod paylaşımını senaryo için NuGet paketlerini oluşturma açıklanmaktadır.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[NuGet paketleri için Xamarin el ile oluşturma](~/cross-platform/app-fundamentals/nuget-manual.md)

NuGet paketleri oluşturmak için ipuçları Xamarin platformuyla çalışır.

##  <a name="cross-platform-data-accessxamarin-formsdata-cloudindexmd"></a>[Çapraz Platform veri erişimi](~/xamarin-forms/data-cloud/index.md)

Çoğu uygulama verileri cihaz üzerinde yerel olarak kaydetmek için bazı gereksinim vardır. Veri miktarını trivially küçük olmadığı sürece, bu genellikle bir veritabanı ve veri katmanı veritabanı erişimi yönetmek üzere uygulamada gerektirir. iOS ve Android "yerleşik" SQLite veritabanı altyapısı sahip ve depolamak ve veri almak için erişim Xamarin'ın platform tarafından basitleştirilmiştir. [Android veri erişimi](~/android/data-cloud/data-access/index.md), [iOS veri erişimi](~/ios/data-cloud/data/index.md), ve [Xamarin.Forms veri erişimi](~/xamarin-forms/data-cloud/index.md) kılavuzlar, her platformda SQLite erişmek nasıl örnekleri sağlar.


##  <a name="transport-layer-securitytransport-layer-securitymd"></a>[Aktarım Katmanı Güvenliği](transport-layer-security.md)

Uygulamanızın ağ bağlantısını korumak için seçimleri yapın doğru SSL/TLS uygulaması hakkında bilgiler.


##  <a name="notificationsxamarin-formsdata-cloudpush-notificationsindexmd"></a>[Bildirimler](~/xamarin-forms/data-cloud/push-notifications/index.md)

Mobil uygulama bildirimleri bazı uygulama belirli olay gerçekleştirilmedi kullanıcı bildiren bir örtük yolu olarak kullanır. Bildirimleri genellikle arka planda çalışan bir uygulama işleminin durumunu kullanıcıya bildirmek için kullanılır. Buna örnek olarak, büyük bir dosya indirme. Bu etkinlik arka planda gerçekleşmesi gereken şekilde bu dosyayı indirmek için bir uzun zaman alabilir. Yükleme tamamlandığında, kullanıcı tarafından bir bildirim olgu bilgilendirilir.
Ayrıca, bildirim ares yerel uygulamalar yalnızca bunlarla sınırlı değil. Bildirimler için mobil uygulamaları yayımlamak sunucu uygulamaları için de mümkündür. Bu makalede, Android ve iOS bildirimleri kullanmayı ele alınacaktır.
