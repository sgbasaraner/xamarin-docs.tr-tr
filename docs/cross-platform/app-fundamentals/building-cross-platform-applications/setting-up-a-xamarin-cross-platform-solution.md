---
title: Bölüm 3 - bir Xamarin Çapraz Platform çözümü ayarladıktan
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: a13765805a3bc6be05522700960b032acbc864b5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>Bölüm 3 - bir Xamarin Çapraz Platform çözümü ayarladıktan

Hangi platformları kullanılıyor bağımsız olarak, Xamarin projeleri tüm aynı çözüm dosya biçimini kullanın (Visual Studio **.sln** dosya biçimi). Çözümleri geliştirme ortamlar genelinde paylaşılabilir (örneğin, Mac için Visual Studio'da Windows projesi) tek tek projeler bile yüklenemiyor.



Yeni bir çapraz platform uygulaması oluştururken, ilk adım boş bir çözüm oluşturmaktır. Bu bölümün sonraki olanlar: platformlar arası mobil uygulamaları oluşturmak için projeleri ayarlama.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>Kod Paylaşma

Başvurmak [kodu paylaşım seçeneklerini](~/cross-platform/app-fundamentals/code-sharing.md) belge nasıl kod paylaşımını platformlarında uygulanacağı ayrıntılı bir açıklaması için.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>Paylaşılan projeleri

Kod dosyaları paylaşımı en kolay yaklaşım kullanılır bir [paylaşılan proje](~/cross-platform/app-fundamentals/shared-projects.md).

Bu yöntem farklı platform projeler arasında aynı kodu paylaşır ve farklı, platforma özgü kod yollarını eklemek için derleyici yönergeleri kullanmanıza olanak tanır.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>Taşınabilir sınıf kitaplıkları (PCL)

Tarihsel olarak .NET proje dosyası (ve elde edilen derlemeyi) belirli framework sürümü için hedef alıyor. Bu proje ya da farklı çerçeveleri tarafından paylaşılıyor derleme önler.

Taşınabilir sınıf kitaplığı (PCL) Xamarin.iOS ve Xamarin.Android, yanı sıra WPF, Evrensel Windows platformu ve Xbox gibi farklı CLI platformlarda kullanılan proje özel bir türde değil. Kitaplığı, yalnızca bir alt Hedeflenen platformlar tarafından sınırlı tam .NET framework'ün kullanabilirler.

Daha fazla bilgiyi Xamarin'ın hakkında [desteklemek için taşınabilir sınıf kitaplıkları](~/cross-platform/app-fundamentals/pcl.md) ve görmek için iletideki yönergeleri izleyin nasıl [TaskyPortable örnek](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) çalışır.


### <a name="net-standard"></a>.NET standart

2016'da, sunulan [.NET standart](~/cross-platform/app-fundamentals/net-standard.md) projeleri Windows kullanılan derlemeleri oluşturan platformları, Xamarin platformları (iOS, Android, Mac) ve Linux kod paylaşmak için kolay bir yol sağlar.

.NET standart kitaplıkları oluşturulabilir ve PCLs gibi her sürümü (1.0 1.6 için) kullanılabilen API'leri daha kolay bulunur ve her bir sürümü geriye dönük olarak uyumludur dışında kullanılan alt sürüm numaralarıyla.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>Çözüm doldurma

Hangi yöntemin bağımsız olarak kod paylaşmak için kullanılan, genel çözüm yapısını kod paylaşımını teşvik eden katmanlı bir mimari uygulamanız gerekir.
Grup koda iki proje türleri için Xamarin yaklaşım şöyledir:

-   **Çekirdek proje** – farklı platformları arasında paylaşılacak, tek bir yerde yeniden kullanılabilir kod yazma. Mümkün olduğunda uygulama ayrıntılarını gizlemek için kapsülleme ilkeleri kullanın.
-   **Platforma özgü uygulama projeleri** – ile yeniden kullanılabilir kodu mümkün olduğunca az bağlantı olarak kullanabilir. Platforma özgü özellikleri, bu düzeyde çekirdek projesinde sunulan bileşenleri üzerine kurulu eklenir.


 <a name="Core_Project" />


### <a name="core-project"></a>Çekirdek proje

Paylaşılan kod projeleri yalnızca tüm platformlarda – IE derlemeleri başvuruda bulunmalıdır. Ortak framework ad alanları ister `System`, `System.Core` ve `System.Xml`.

Paylaşılan projeleri aşağıdaki katmanları içerebilir mümkün olduğu kadar UI olmayan işlevselliği uygulamanız gerekir:

-   **Veri katmanı** – fiziksel veri depolamasını ör mvc'deki kodu.  [SQLite NET](https://github.com/praeclarum/sqlite-net), alternatif bir veritabanına ister [Realm.io](https://realm.io/products/realm-mobile-database/) ve hatta XML dosyaları. Veri katmanı normalde sınıflardır yalnızca veri erişim katmanı tarafından kullanılır.
-   **Veri erişim katmanı** – yöntemleri erişim listeleri veri, tek tek veri öğeleri oluşturun ve ayrıca, düzenleme ve bunları silme gibi uygulamanın işlevselliği için gerekli veri işlemleri destekleyen bir API tanımlar.
-   **Hizmet erişim katmanı** – bulut sağlamak için isteğe bağlı bir katman uygulamaya Hizmetleri. Uzak ağ kaynaklarına (web Hizmetleri, görüntü yüklemeleri, vb.) erişen kodunu içerir ve büyük olasılıkla sonuçlarını önbelleğe alma.
-   **İş katmanı** – Model sınıfları ve platforma özel uygulamalara işlevselliği kullanıma cephesi veya yönetici sınıfları tanımı.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>Platforma özgü uygulama projeleri

Platforma özgü projeleri çekirdek paylaşılan kodunu projeyi yanı sıra her platformun SDK (Xamarin.iOS, Xamarin.Android, Xamarin.Mac ya da Windows) bağlamak için gerekli olan derlemeleri başvurmalıdır.

Platforma özgü projeleri uygulamanız gerekir:

-   **Uygulama katmanı** – Platform belirli işlevleri ve iş katmanı nesneleri ve kullanıcı arabirimi arasında bağlama/dönüştürme.
-   **Kullanıcı arabirimi katmanı** – ekranlar, özel kullanıcı arabirimi denetimlerini, doğrulama mantığını sunumu.


<a name="Example" />


### <a name="example"></a>Örnek

Uygulama Mimarisi Bu diyagramda gösterilmiştir:

 [ ![](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "Uygulama Mimarisi Bu diyagramda gösterilmiştir")](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

Bu ekran paylaşılan çekirdek proje, iOS ve Android uygulaması projeleri çözüm Kurulum'a gösterir. Paylaşılan proje her mimari katmanları (iş, hizmet, veri ve veri erişimi kodu) ilgili kod içerir:

 ![](setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "Her mimari katmanları (iş, hizmet, veri ve veri erişimi kodu) ilgili kod paylaşılan projesi içerir")


 <a name="Project_References" />


## <a name="project-references"></a>Proje başvuruları

Proje başvuruları için bir proje bağımlılıkları yansıtır. Çekirdek projeleri başvurularını ortak derlemeleri için sınırlamak kod paylaşmak kolaydır.
Platforma özgü uygulama projeleri paylaşılan kod yanı sıra, hedef platformu yararlanmak için ihtiyaç duydukları diğer platforma özgü derlemelerini başvuru.

Uygulama projeleri her paylaşılan proje başvurusu ve bu ekran görüntülerinde gösterildiği gibi kullanıcının mevcut işlevselliği için gerekli kullanıcı arabirimi kodu içerir:

![](setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "Her başvuru paylaşılan Proje uygulama projeleri") ![ ] (setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "her başvuru paylaşılan Proje uygulama projeleri")


Projeleri nasıl yapılandırılmış belirli örnekleri örnek olay incelemeleri verilir.

 <a name="Adding_Files" />


## <a name="adding-files"></a>Dosya ekleme

 <a name="Build_Action" />


### <a name="build-action"></a>Derleme eylemi

Doğru bir yapı eylem belirli dosya türlerini ayarlamak önemlidir. Bu liste, bazı ortak dosya türleri için yapı eylemi gösterir:

-  **Tüm C# dosyalarını** – yapı eylemi: derleme
-   **Xamarin.iOS & Windows görüntüleri** – yapı eylemi: içerik
-   **Xamarin.iOS XIB ve film şeridi dosyalarında** – yapı eylemi: InterfaceDefinition
-   **Görüntüleri ve Android AXML düzenleri** – yapı eylemi: AndroidResource
-  **XAML dosyaları Windows projelerinde** – yapı eylemi: sayfası
-  **Xamarin.Forms XAML dosyaları** – yapı eylemi: EmbeddedResource


Genellikle IDE dosya türünü algılar ve doğru yapı eylemi önerir.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Büyük/küçük harfe duyarlılık

Son olarak, bazı platformlar (ör. büyük küçük harfe duyarlı dosya sistemleri olduğunu unutmayın.
iOS ve Android) kadar bir tutarlı dosya adlandırma standardı kullanın ve kodda kullandığınız dosya adları filesystem tam olarak eşleştiğinden emin olun emin olun. Bu görüntüler ve kodda başvuru diğer kaynaklar için özellikle önemlidir.
