---
title: Kuruluş uygulaması geliştirme giriş
description: Bu bölümde, Kurumsal uygulama geliştirme için bir giriş sağlar ve eShopOnContainers mobil uygulama sunar.
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9deb685c92092ceb0e1c775a1e53ac1bce5a4a57
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242966"
---
# <a name="introduction-to-enterprise-app-development"></a>Kuruluş uygulaması geliştirme giriş

Platformundan bağımsız olarak, Kurumsal uygulamaları, geliştiricilerin birkaç güçlüklerle karşılaşır:

-   Uygulama gereksinimleri zamanla değişebilir.
-   Yeni iş fırsatlarını ve sorunları.
-   Kapsam ve uygulama gereksinimlerini önemli ölçüde etkileyebilir geliştirme sırasında devam eden geri bildirim.

Bunlar aklınızda kolayca değiştirilebilir veya zamanla genişletilmiş uygulamalar oluşturmak önemlidir. Tek tek parçaları bağımsız olarak geliştirilen ve yalıtım modunda uygulama kalan etkilemeden test yazmasına izin veren bir mimari gerektirdiği gibi uyumluluk için tasarlama zor olabilir.

Birçok kuruluş birden fazla Geliştirici gerektirecek şekilde yeterince karmaşık uygulamalardır. Birden çok geliştiricilere etkili bir şekilde uygulama farklı parçalarının bağımsız olarak, parçaları birlikte sorunsuz bir şekilde uygulamada tümleştirildiğinde geldiğini sağlarken çalışabilmeniz için bir uygulama tasarlamak nasıl karar vermek için önemli bir sorun olabilir.

Tasarlama ve ne sonuçlarında bir uygulama oluşturmak için geleneksel yaklaşım olarak adlandırılır bir *tek yapılı* burada bileşenleri sıkı şekilde bağlı aralarında Temizle hiçbir ayırmalı uygulama. Genellikle, bu tek yapılı bir yaklaşım uygulamada diğer bileşenleri bozmadan hataları gidermek zor olabilir çünkü zor ve korumak için verimsiz uygulamaları müşteri adayları ve yeni özellik eklemek veya var olan özellikleri değiştirmek için zor olabilir.

Bu zorluklar için etkili bir remedy kolayca birlikte uygulamaya tümleştirilebilir ayrık, geniş eşleşmiş bileşenlere uygulama bölüm sağlamaktır. Bu tür bir yaklaşım birkaç avantaj sunar:

-   Geliştirilen, test, genişletilmiş ve farklı kişilere veya ekiplere tarafından tutulan tek tek işlevsellik sağlar.
-   Yeniden kullanma ve kimlik doğrulaması ve veri erişimi gibi uygulama yatay özelliklerini ve uygulama belirli iş işlevi gibi dikey özellikleri arasında sorunları temiz ayrılması yükseltir. Bu, daha kolay yönetilen uygulama bileşenleri arasındaki etkileşimler ve bağımlılıklar sağlar.
-   Bu rol ayrımı farklı kişiler veya ekipler, belirli bir görev veya uzmanlara göre işlevselliği parçası odaklanmak izin vererek korumaya yardımcı olur. Özellikle, kullanıcı arabirimi ve uygulamanın iş mantığı arasında temizleyici bir ayrım sağlar.

Ancak, bir uygulama ayrık, geniş eşleşmiş bileşenlerine böldüğünüzde çözümlenmelidir birçok sorunları vardır. Bu güncelleştirmeler şunlardır:

-   Kullanıcı arabirimi denetimlerini ve bunların mantığı arasında sorunları temiz ayrılması sağlamak nasıl karar verme. Xamarin.Forms kuruluş uygulama oluştururken en önemli kararlardan biri mi iş mantığı arka plan kod dosyalarında yerleştirmek, temiz bir kullanıcı arabirimi denetimlerini ve daha fazla uygulama yapmak için kendi mantığı arasında sorunları ayrılması oluşturulup oluşturulmayacağını mi korunabilir ve sınanabilir. Daha fazla bilgi için bkz: [Model View ViewModel](~/xamarin-forms/enterprise-application-patterns/mvvm.md).
-   Bir bağımlılık ekleme kapsayıcısını kullanılıp kullanılmayacağını belirleyin. Bağımlılık ekleme kapsayıcıları eklenen bağımlılıklarını ile sınıfların örneklerini oluşturmak için bir olanak sağlayarak nesneler arasındaki Kuplaj bağımlılık azaltmak ve kapsayıcı yapılandırmasını temel alarak kendi ömürleri yönetin. Daha fazla bilgi için bkz: [bağımlılık ekleme](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).
-   Sağlanan platformu olay arasında kabaca seçme nesne ve türü başvurularını bağlamak kullanışsız bileşenleri arasındaki iletişim ileti tabanlı bağlanmış. Daha fazla bilgi için bkz: Giriş [iletişim arasında gevşek bağlanmış bileşenleri](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md).
-   Gezinti, çağırmak nasıl dahil olmak üzere sayfaları arasında gezinmek nasıl karar ve gezinti mantığı bulunduğu. Daha fazla bilgi için bkz: [Gezinti](~/xamarin-forms/enterprise-application-patterns/navigation.md).
-   Kullanıcı girişi doğruluğunu doğrulamak nasıl belirleme. Karar kullanıcı girişini doğrulama etme ve doğrulama hataları hakkında kullanıcıya bildirim eklemeniz gerekir. Daha fazla bilgi için bkz: [doğrulama](~/xamarin-forms/enterprise-application-patterns/validation.md).
-   Kimlik doğrulaması gerçekleştirme ve yetkilendirme ile kaynakları korumak nasıl karar verme. Daha fazla bilgi için bkz: [kimlik doğrulama ve yetkilendirme](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md).
-   Web sunucusundan uzak verilere erişmek nasıl yapılacağına ilişkin, güvenilir bir şekilde veri alma ve verileri önbelleğe almak nasıl dahil olmak üzere Hizmetleri. Daha fazla bilgi için bkz: [erişme uzak veri](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).
-   Uygulamayı test etme karar verme. Daha fazla bilgi için bkz: [birim testi](~/xamarin-forms/enterprise-application-patterns/unit-testing.md).

Bu kılavuzda bu sorunları hakkında yönergeler sağlar ve Xamarin.Forms kullanarak bir platformlar arası kuruluş uygulaması oluşturmak için Mimari ve çekirdek desenleri odaklanmaktadır. Kılavuzu amaçlar ortak Xamarin.Forms Kurumsal uygulama geliştirme senaryoları adresleme ve sunu, sunum mantığı ve varlıklar için Destek aracılığıyla endişelere ayırarak uyarlanabilir, sürdürülebilir ve sınanabilir kodu oluşturmak için yardımcı olmak için Model-View-ViewModel (MVVM) deseni.

## <a name="sample-application"></a>Örnek uygulama

Bu kılavuz aşağıdaki işlevselliği içeren bir çevrimiçi depolama alanının eShopOnContainers, örnek bir uygulama içerir:

-   Kimlik doğrulaması ve arka uç hizmeti karşı yetkilendirme.
-   Bir katalog gömlekler, kahve bardaklar ve diğer pazarlama öğeleri tarama.
-   Katalog filtreleme.
-   Kataloğu'ndan öğeleri sıralama.
-   Kullanıcının Sipariş geçmişini görüntüleme.
-   Yapılandırma ayarları.

### <a name="sample-application-architecture"></a>Örnek uygulama mimarisi

Şekil 1-1 örnek uygulama mimarisi üst düzey bir genel bakış sağlar.

![](introduction-images/architecture.png "eShopOnContainers üst düzey mimari")

**Şekil 1-1**: eShopOnContainers üst düzey mimari

Örnek uygulamayı üç istemci uygulamaları ile birlikte gelir:

-   Bir MVC uygulamasında ASP.NET Core ile geliştirilmiştir.
-   Tek sayfa uygulama (SPA) Açısal 2 ve Typescript ile geliştirilmiştir. Web uygulamaları için bu yaklaşım, her işlemi ile sunucuya gidiş gerçekleştirme önler.
-   Bir mobil uygulama, iOS, Android ve evrensel Windows Platformu (UWP) destekleyen Xamarin.Forms ile geliştirilmiştir.

Web uygulamaları hakkında daha fazla bilgi için bkz: [Architecting ve ASP.NET Core ve Microsoft Azure ile Modern Web uygulamaları geliştirme](http://aka.ms/WebAppEbook).

Örnek uygulama aşağıdaki arka uç hizmetlerini içerir:

-   ASP.NET Core kimlik ve IdentityServer kullanan bir kimlik mikro.
-   Veri temelli bir oluşturma olan bir katalog mikro hizmet okuma, güncelleştirme, EntityFramework çekirdeği kullanarak bir SQL Server veritabanını kullanır (CRUD) hizmetini silin.
-   Etki alanı tabanlı tasarım modeli kullanan etki alanı tabanlı bir hizmet olan bir sıralama mikro hizmet.
-   Redis önbelleği kullanan veri güdümlü bir CRUD hizmeti Sepeti mikro.

Bu arka uç Hizmetleri, ASP.NET Core MVC kullanarak mikro uygulanır ve tek bir Docker ana benzersiz kapsayıcılara olarak dağıtılır. Topluca eShopOnContainers uygulama başvuru olarak bu arka uç Hizmetleri olarak denir. İstemci uygulamaları bir temsili durum aktarımı (REST) web arabirimi aracılığıyla arka uç hizmetleriyle iletişim kurar. Mikro ve Docker hakkında daha fazla bilgi için bkz: [kapsayıcılı mikro](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md).

Arka uç hizmetlerinin uygulaması hakkında daha fazla bilgi için bkz: [.NET mikro: mimarisi kapsayıcılı .NET uygulamaları için](https://aka.ms/microservicesebook).

### <a name="mobile-app"></a>Mobil uygulama

Bu kılavuz Xamarin.Forms kullanarak platformlar arası Kurumsal uygulamaları oluşturma üzerine odaklanır ve eShopOnContainers mobil uygulama örnek olarak kullanır. Şekil 1-2 daha önce açıklanan işlevleri sağlayan eShopOnContainers mobil uygulama sayfaları gösterir.

[![](introduction-images/screenshots.png "EShopOnContainers mobil uygulama")](introduction-images/screenshots-large.png#lightbox "eShopOnContainers mobil uygulama")

**Şekil 1-2**: eShopOnContainers mobil uygulama

Mobil uygulama eShopOnContainers başvuru uygulama tarafından sağlanan arka uç hizmetlerini kullanır. Ancak, arka uç hizmetlerini dağıtma kaçınmak isteyen olanlar için sahte hizmetlerinden verileri kullanmak üzere yapılandırılabilir.

EShopOnContainers mobil uygulama aşağıdaki Xamarin.Forms işlevselliği uygular:

-   XAML
-   Denetimler
-   Bağlamalar
-   Dönüştürücüler
-   Stilleri
-   Animasyonlar
-   Komutlar
-   Davranışlar
-   Tetikleyiciler
-   Etkiler
-   Özel oluşturucu
-   MessagingCenter
-   Özel denetimler

Bu işlevsellik hakkında daha fazla bilgi için bkz: [Xamarin.Forms belgelerine](~/xamarin-forms/index.yml), ve [Xamarin.Forms ile Mobile Apps oluşturma](https://aka.ms/xamebook).

Ayrıca, bazı eShopOnContainers mobil uygulamaya sınıfları için birim testleri sağlanır.

#### <a name="mobile-app-solution"></a>Mobil uygulama çözüm

EShopOnContainers mobil uygulama çözüm projelere kaynak kodu ve diğer kaynakları düzenler. Tüm projeleri kaynak kodu ve diğer kaynakları kategoriler halinde düzenlemek için klasörler kullanın. Aşağıdaki tabloda eShopOnContainers mobil uygulamayı olun projeleri özetlenmektedir:

|Proje|Açıklama|
|--- |--- |
|eShopOnContainers.Core|Bu proje paylaşılan kullanıcı Arabirimi ve paylaşılan kodu içeren taşınabilir sınıf kitaplığı (PCL) projesidir.|
|eShopOnContainers.Droid|Bu proje Android özel kod tutar ve Android için giriş noktasıdır.|
|eShopOnContainers.iOS|Bu proje iOS özel kod tutar ve iOS uygulaması giriş noktası.|
|eShopOnContainers.UWP|Bu proje Evrensel Windows Platformu (UWP) belirli bir kod içerir ve Windows uygulaması için giriş noktasıdır.|
|eShopOnContainers.TestRunner.Droid|Bu proje için eShopOnContainers.UnitTests projeyi Android test Çalıştırıcısı ' dir.|
|eShopOnContainers.TestRunner.iOS|İOS test Çalıştırıcısı eShopOnContainers.UnitTests projesi için projesidir.|
|eShopOnContainers.TestRunner.Windows|Bu proje eShopOnContainers.UnitTests proje için evrensel Windows platformu test Çalıştırıcısı ' dir.|
|eShopOnContainers.UnitTests|Bu proje eShopOnContainers.Core proje için birim testleri içerir.|

EShopOnContainers mobil uygulama sınıflardan çok az kayıpla veya hiç değişiklik ile Xamarin.Forms uygulamalarda tekrar kullanılabilir.

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core proje

EShopOnContainers.Core PCL proje aşağıdaki klasör içerir:

|Klasör|Açıklama|
|--- |--- |
|Animasyonlar|XAML'de tüketilmesi animasyonları sağlayan sınıflar içerir.|
|Davranışlar|Sınıfları görüntülemek için sunulan davranışları içerir.|
|Denetimler|Uygulama tarafından kullanılan özel denetimler içerir.|
|Dönüştürücüler|Özel mantık bir bağlama için geçerli değer dönüştürücüler içerir.|
|Etkiler|İçeren `EntryLineColorEffect` Özel kenarlık rengini değiştirmek için kullanılan sınıf `Entry` kontrol eder.|
|Özel Durumlar|Özel içeren `ServiceAuthenticationException`.|
|Uzantıları|İçin genişletme yöntemleri içeren `VisualElement` ve `IEnumerable` sınıfları.|
|Yardımcıları|Uygulama için yardımcı sınıfları içerir.|
|Modeller|Uygulama için model sınıfları içerir.|
|Özellikler|İçeren `AssemblyInfo.cs`, bir .NET derlemesi meta veri dosyası.|
|Hizmetler|Arabirimleri ve uygulamaya sağlanan hizmetleri uygulayan sınıflar içerir.|
|Tetikleyiciler|İçeren `BeginAnimation` XAML animasyonda çağırmak için kullanılan tetikleyici.|
|Doğrulama|Veri girişi doğrulama ilgili sınıflar içerir.|
|ViewModels|Sayfalara gösterilen uygulama mantığını içerir.|
|Görünümler|Uygulama sayfaları içerir.|

##### <a name="platform-projects"></a>Platform projeleri

Platform projeleri etkisi uygulamaları, özel Oluşturucu uygulamaları ve diğer platforma özgü kaynakları içerir.

## <a name="summary"></a>Özet

Xamarin'ın platformlar arası mobil uygulama geliştirme araçları ve platformları sağlamak kapsamlı bir çözüm için B2E, B2B ve B2C mobil istemci uygulamaları, tüm hedef platformlar (iOS, Android ve Windows) ve düşürmek için yardımcı kod paylaşma olanağı sağlar Toplam sahip olma maliyeti. Uygulamaları, kendi kullanıcı arabirimi ve uygulama mantığı kodu, yerel platform görünüm korurken paylaşabilirsiniz.

Kurumsal uygulamaları, geliştiricilerin uygulama mimarisi geliştirme sırasında değiştirebilir birkaç güçlüklerle karşılaşır. Bu nedenle, böylece değiştiren veya zamanla genişletilmiş bir uygulama oluşturmak önemlidir. Böyle uyumluluk için tasarlama zor olabilir, ancak genellikle uygulama kolayca birlikte uygulamaya tümleştirilebilir ayrık, geniş eşleşmiş bileşenlere bölümleme ilgilidir.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
