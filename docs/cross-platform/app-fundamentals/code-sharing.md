---
title: "Paylaşım kodu seçenekleri"
description: "Bu belge kod platformlar arası projeler arasında paylaşımı için farklı yöntemler karşılaştırır: paylaşılan projeleri, taşınabilir sınıf kitaplıkları ve avantajları ve dezavantajları dahil .NET standart."
ms.topic: article
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 044dc0f3c0b5a86944fc852cdd97f8affcb8e874
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="sharing-code-options"></a>Paylaşım kodu seçenekleri

_Bu belge kod platformlar arası projeler arasında paylaşımı için farklı yöntemler karşılaştırır: paylaşılan projeleri, taşınabilir sınıf kitaplıkları ve avantajları ve dezavantajları dahil .NET standart._

## <a name="overview"></a>Genel Bakış

Platformlar arası uygulamalar arasında kod paylaşmak için üç alternatif yöntemler şunlardır:

-   [**Paylaşılan projeleri** ](#Shared_Projects) – kaynak kodunuzu düzenlemek için paylaşılan varlık proje türü kullanın ve platforma özgü gereksinimler yönetmek için gerektiği şekilde #if derleyici yönergeleri kullanın.
-   [**Taşınabilir sınıf kitaplıkları** ](#Portable_Class_Libraries) – taşınabilir sınıf kitaplığı (PCL) atamak istediğiniz desteklemek için platformları oluşturmak ve arabirimleri platforma özgü işlevsellik sağlamak için kullanır.
-   [**.NET standart kitaplıkları** ](#Net_Standard) – .NET standart projeleri çalışma benzer şekilde platforma özgü işlevsellik eklemesine arabirimleri kullanılmasını gerektirmek PCLs için.

Kod paylaşımını stratejisi amacı, tek bir kod temelinde birden çok platform tarafından nerede kullanılabilir Bu diyagramda gösterildiği mimarisi desteklemektir.

 ![](code-sharing-images/conceptualarchitecture.png "Paylaşılan kod uygulama mimarisi")

Bu makalede, uygulamalarınız için doğru proje türü seçmenize yardımcı olmak için üç yöntem karşılaştırır.

<a name="Shared_Projects" />

# <a name="shared-projects"></a>Paylaşılan projeleri

Kod dosyaları paylaşımı en kolay yaklaşım paylaşılan (Xamarin Studio 5 ve Visual Studio 2013 güncelleştirme 2'de kullanılmaya) proje kullanmaktır. Paylaşılan projeleri olan [aşağıda ayrıntılı olarak ele alınan](~/cross-platform/app-fundamentals/shared-projects.md).

Bu ekran (Android, iOS ve Windows Phone için), üç uygulama projeleri içeren bir çözüm dosyasını gösteren bir **paylaşılan** ortak C# kaynak kodu dosyaları içeren projesi:

 ![](code-sharing-images/sharedsolution.png "Paylaşılan bir proje çözümü")

Kavramsal mimaris burada her proje tüm paylaşılan kaynak dosyalarını içeren Aşağıdaki diyagramda gösterilmiştir:

 ![](code-sharing-images/sharedassetproject.png "Proje diyagramı paylaşılan")


## <a name="example"></a>Örnek

İOS, Android ve Windows Phone destekleyen platformlar arası uygulamasına her platform için bir uygulama projesi gerektirir. Ortak kodun paylaşılan projesinde yaşar.

Örnek bir çözüm, aşağıdaki klasörleri ve projeler (proje adları seçtiniz anlamlılık için projelerinizi adlandırma bu yönergelere gerekmez) içermesi gerekir:

-   **Paylaşılan** – tüm projeler için ortak bir kod içeren proje paylaşılan.
-   **AppAndroid** – Xamarin.Android uygulaması projesi.
-   **AppiOS** – Xamarin.iOS uygulaması projesi.
-   **AppWinPhone** – Windows Phone Uygulama projesi.


Bu şekilde, aynı kaynak kodunu (C# dosyalarını paylaşılan) üç uygulama projeleri paylaşıyorsunuz. Paylaşılan kod düzenlemeler, tüm üç projeler arasında paylaşılır.


## <a name="benefits"></a>Yararları

-  Birden çok projeler arasında kod paylaşmanıza olanak tanır.
-  Paylaşılan kod (ör. derleyici yönergeleri kullanarak platform göre dal kullanarak `#if __ANDROID__` de anlatıldığı gibi [Çapraz Platform uygulamaları oluşturma](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) belge).
-  Uygulama projeleri paylaşılan kod kullanabilir platforma özgü başvurular içerebilir (örneğin kullanarak `Community.CsharpSqlite.WP7` Tasky örnek Windows Phone için).



## <a name="disadvantages"></a>Dezavantajlar

-  Diğer birçok proje türlerinin aksine, paylaşılan bir proje yok 'output' derlemesi içeriyor. Derleme sırasında dosyalar başvuru projenin bir parçası olarak kabul edilir ve bu derlemeye derlenmiş. Kodunuzu bir derleme olarak paylaşmak istiyorsanız sonra taşınabilir sınıf kitaplıkları veya .NET standart olan daha iyi bir çözüm.
-  'İnactive' derleyici yönergeleri içinde kod etkileyen yapan yeniden düzenlemeler kodu güncelleştirmez.


 <a name="Shared_Remarks" />

## <a name="remarks"></a>Açıklamalar

Kod yazma uygulama geliştiricileri için iyi bir çözüm, yalnızca, Uygulama Paylaşımı (ve diğer geliştiricilerine dağıtma değil) için tasarlanmıştır.

 <a name="Portable_Class_Libraries" />


# <a name="portable-class-libraries"></a>Taşınabilir sınıf kitaplıkları


Taşınabilir sınıf kitaplıkları olan [aşağıda ayrıntılı olarak ele alınan](~/cross-platform/app-fundamentals/pcl.md).

 ![](code-sharing-images/portableclasslibrary.png "Taşınabilir sınıf kitaplığı diyagramı")


## <a name="benefits"></a>Yararları

-  Birden çok projeler arasında kod paylaşmanıza olanak tanır.
-  Yeniden düzenleme işlemi her zaman etkilenen tüm başvuruları güncelleştirin.


## <a name="disadvantages"></a>Dezavantajlar

-  Derleyici yönergeleri kullanamazsınız.
-  .NET framework'ün bir alt Seçili profili tarafından belirlenen kullanmak, yalnızca (bkz [PCL giriş](~/cross-platform/app-fundamentals/pcl.md) daha fazla bilgi için).


## <a name="remarks"></a>Açıklamalar

Elde edilen derlemeyi diğer geliştiricilerle paylaşabilirsiniz planlıyorsanız, iyi bir çözümdür.



<a name="Net_Standard" />

# <a name="net-standard-libraries"></a>.NET standart kitaplıkları

.NET standarttır [aşağıda ayrıntılı olarak ele alınan](~/cross-platform/app-fundamentals/net-standard.md).

![](code-sharing-images/netstandard.png ".NET standart diyagramı")

## <a name="benefits"></a>Yararları

-  Birden çok projeler arasında kod paylaşmanıza olanak tanır.
-  Yeniden düzenleme işlemi her zaman etkilenen tüm başvuruları güncelleştirin.
-  PCL profilleri daha büyük yüzey alanını .NET temel sınıf kitaplığı (BCL) kullanılabilir.

## <a name="disadvantages"></a>Dezavantajlar

 -  Derleyici yönergeleri kullanamazsınız.

## <a name="remarks"></a>Açıklamalar

.NET standart benzer PCL, ancak platform desteği ve daha fazla sayıda BCL sınıflardan için daha basit bir modeli.



# <a name="summary"></a>Özet

Seçtiğiniz stratejisi paylaşımı kod, hedeflediğiniz platformlar tarafından yürütülür. Projeniz için en uygun bir yöntem seçin.

PCL veya .NET standart paylaşılabilir kodu kitaplıkları (özellikle NuGet üzerinde yayımlama) oluşturmak için iyi bir seçenek olabilir. Paylaşılan projeleri iyi arası platforma uygulamalarını içinde çok sayıda Platform özgü işlevsellik kullanmayı planladığınız uygulama geliştiricileri için çalışır.


## <a name="related-links"></a>İlgili bağlantılar

- [Çapraz Platform uygulamaları oluşturma (ana belge)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md)
- [Paylaşılan projeleri](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Örnek olay incelemesi: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky örnek (github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [PCL (github) kullanarak tasky örnek](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
- [Visual Studio 2013 için proje başvuru Yöneticisi paylaşılan](http://visualstudiogallery.msdn.microsoft.com/315c13a7-2787-4f57-bdf7-adae6ed54450)
