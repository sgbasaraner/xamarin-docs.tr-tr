---
title: Bölüm 2 - mimarisi
description: Bu belge, platformlar arası uygulamaları oluşturmak için yararlı mimarisi desenleri açıklar. Genel uygulama katmanları (veri katmanı, veri erişim katmanı, vb.) ve ortak mobil yazılım desenler (MVVM, MVC, vb.) açıklanır
ms.prod: xamarin
ms.assetid: 2176DB2D-E84A-3757-CFAB-04A586068D50
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: ebcfe8880a826c552d55e7a5567271a5a764fa1d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780881"
---
# <a name="part-2---architecture"></a>Bölüm 2 - mimarisi

Platformlar arası uygulamalar oluşturma bir anahtar kendisini maximization platformlarında paylaşımı kodunun uygundur bir mimari oluşturmak için ilkesidir. Aşağıdaki nesne odaklı programlama ilkelerine uymak yardımcı olur, iyi tasarlanmış bir uygulama oluşturun:

-   **Kapsülleme** – sağlama sınıfları ve hatta mimari katmanları yalnızca kendi gerekli gerçekleştirir en az bir API ortaya işlevler ve uygulama ayrıntılarını gizler. Bir sınıf düzeyinde nesneleri 'siyah kutular' davranır ve kod tüketen nasıl bunlar görevlerini gerçekleştirmek bilmeniz gerekmez, bu anlamına gelir. Bir mimari düzeyde cephesi gibi daha soyut Katmanlar kodda adına daha karmaşık etkileşimler düzenler basitleştirilmiş bir API teşvik desenleri uygulama anlamına gelir. Bu UI kod (örneğin) yalnızca ekranlar görüntüleme ve kullanıcı girişini kabul sorumlu gerektiği anlamına gelir; ve hiçbir zaman veritabanı ile doğrudan etkileşim. Benzer şekilde veri erişim kodu salt okunur ve veritabanına yazma, ancak hiçbir zaman doğrudan düğmeleri veya etiketleri ile etkileşim.
-   **Sorumluluk ayrımı** – her bileşenin emin olun (hem mimari ve sınıf düzeyi) NET ve iyi tanımlanmış bir amacı vardır. Her bileşen yalnızca, tanımlı görevleri ve bu işlevselliği kullanmak için gereken diğer sınıflara erişilebilir bir API aracılığıyla kullanıma gerekir.
-   **Çok biçimlilik** – çekirdek kod yazılmış ve platforma özgü özelliklerle hala etkileşim sırasında platformları arasında paylaşılan, birden çok uygulamaları anlamına gelir destekleyen programlama arabirimi (veya soyut sınıf).


Doğal sonucu gerçek dünya ya da ayrı mantıksal katmanlarıyla soyut varlıklar sonra Modellenen bir uygulamadır. Kod Katmanlar yap uygulamalara anlamak daha kolay ayırmak, test ve sürdürün. Her katmandaki kod (ya da dizinleri ya da çok büyük uygulamalar için bile ayrı projeleri) fiziksel olarak ayrı olarak mantıksal olarak ayrı (ad alanlarını kullanma) olması önerilir.

 <a name="Typical_Application_Layers" />


## <a name="typical-application-layers"></a>Genel uygulama katmanları

Bu belge ve örnek olay incelemeleri boyunca size aşağıdaki altı uygulama katmanları bakın:

-   **Veri katmanı** – geçici olmayan veri kalıcılığını, büyük olasılıkla bir SQLite veritabanı XML dosyaları veya başka bir uygun mekanizma uygulanabilir ancak.
-   **Veri erişim katmanı** – oluşturma, okuma, güncelleştirme, uygulama ayrıntılarını çağırana gösterme olmadan verileri sil (CRUD) erişim sağlayan veri katmanı çevresinde sarmalayıcı. Örneğin, DAL sorgulamak ya da verileri güncelleştirmek için SQL deyimlerini içerebilir ancak başvuru yapan kod bunu bilmesi ihtiyaç duymaz.
-   **İş katmanı** – (iş mantığı katmanı veya BLL denir) iş varlık tanımları (modeli) ve iş mantığını içerir. İş cephesi düzeni için aday.
-   **Hizmet erişim katmanı** – bulut hizmetlerine erişmek için kullanılır: uzak sunuculardan karmaşık web hizmetlerinden (REST, JSON, WCF) veri ve görüntüleri basit alınması için. Ağ davranışı yalıtır ve uygulama ve kullanıcı Arabirimi katmanları tarafından kullanılması için basit bir API sağlar.
-   **Uygulama katmanı** – genellikle platforma özgüdür (değil genellikle platformları arasında paylaşılan) veya kodunu kodu (genellikle yeniden) uygulamaya özgü. İyi bir sınama olup uygulama katmanında UI katmanında karşı kod yerleştirmek sınıf herhangi bir gerçek görüntüleme denetim olup olmadığı veya (b), birden çok ekranlar veya aygıtlar arasında (ör. paylaşılıp (a) belirlemektir iPhone ve iPad).
-   **Kullanıcı Arabirimi (UI) katmanı** – kullanıcı dönük katman ekranlar, pencere öğeleri içerir ve denetleyiciler, yönetmek bunları.


Bir uygulamanın tüm katmanlar mutlaka içermeyebilir – örneğin hizmeti erişim katmanı ağ kaynaklarına erişim olmayan bir uygulamada var olmayacaktır. İşlemleri çok temel nedeni çok basit bir uygulama veri katmanı ve veri erişim katmanı birleştirme.

 <a name="Common_Mobile_Software_Patterns" />


## <a name="common-mobile-software-patterns"></a>Ortak mobil yazılım desenler

Desenler yinelenen yaygın sorunların çözümleri yakalamak için kurulmuş bir yoludur. Korunabilir ve anlaşılabilir duruma mobil uygulamalar oluşturmaya anlamak yararlı olan birkaç anahtar desenleri vardır.

-   **Model, görünüm, ViewModel (MVVM)** – Model-View-ViewModel düzeni popüler veri bağlama, Xamarin.Forms gibi destek çerçeveleri ile. Windows Presentation Foundation (WPF) ve Silverlight gibi XAML etkin SDK popularized; Burada ViewModel verileri (modeli) ve veri bağlama ve komutları aracılığıyla kullanıcı arabirimi (görünümü) arasında bir aracılık olarak görev yapar.
-   **Model, görünüm, denetleyici (MVC)** – bir ortak ve genellikle misunderstood desen MVC kullanıcı arabirimleri oluştururken en sık kullanılan ve (Görünüm), bir kullanıcı Arabirimi ekran işleme altyapısı arkasındaki gerçek tanımını arasında ayrım sağlar Etkileşim (denetleyicisi) ve bunu (modeli) doldurduğundan verileri. Gerçekte tamamen isteğe bağlı bir modeldir ve bu nedenle, bu deseni anlamanın çekirdek Görünüm ve denetleyici arasındadır. MVC, iOS uygulamaları için popüler bir yaklaşımdır.
-   **İş cephesi** – diğer adıyla Yöneticisi düzeni karmaşık iş için basitleştirilmiş bir giriş noktası sağlar. Örneğin, bir görev izleme uygulamasında olabilir bir `TaskManager` yöntemleriyle gibi sınıf `GetAllTasks()` , `GetTask(taskID)` , `SaveTask (task)` vb. `TaskManager` Cephesi gerçekte kaydetme/görevleri nesnelerin alma çalışmalar için sınıf sağlar.
-   **Singleton** – Singleton deseni, belirli bir nesnenin yalnızca tek bir örneğini hiç varolabilir bir yol sağlar. Örneğin, SQLite mobil uygulamalarda kullanırken, her zaman sadece bir örnek veritabanı istediğiniz. Bu basit bir şekilde Singleton deseni kullanmaktır.
-   **Sağlayıcı** – kodu yeniden kullanma Silverlight, WPF ve WinForms uygulamalarda teşvik eden bir desen de Microsoft (tartışmaya açık bir şekilde stratejisi veya temel bağımlılık ekleme benzer). Paylaşılan kod bir arabirim ya da soyut sınıf karşı yazılabilir ve platforma özgü somut uygulamaları yazılır ve kod kullanıldığında geçirildi.
-   **Zaman uyumsuz** – zaman uyumsuz desen uzun süre çalışan iş kullanıcı Arabirimi veya geçerli işlem tutmadan yürütülmek üzere gerektiğinde kullanılır Async anahtar sözcüğü ile karıştırılmamalıdır. Uzun süre çalışan görevler başka bir programda başlayacağı zamana en basit biçimiyle zaman uyumsuz deseni yalnızca açıklanır geçerli iş parçacığının sırasında iş parçacığı (veya benzer iş parçacığı Özet görevi gibi) işlemeye devam eder ve arka plan işlemi yanıt dinler , veri ve/veya durumu döndürüldüğünde kullanıcı arabirimini güncelleştirir.


Her desenleri pratik kullanımları örnek olay incelemeleri gösterildiği gibi daha ayrıntılı denenecektir. Wikipedia açıklamalarını daha ayrıntılı [MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel), [MVC](https://en.wikipedia.org/wiki/Model–view–controller), [cephesi](http://en.wikipedia.org/wiki/Facade_pattern), [Singleton](http://en.wikipedia.org/wiki/Singleton_pattern), [stratejisi](http://en.wikipedia.org/wiki/Strategy_pattern)ve [sağlayıcı](http://en.wikipedia.org/wiki/Provider_model) desenleri (ve [tasarım desenleri](http://en.wikipedia.org/wiki/Design_Patterns) genellikle).
