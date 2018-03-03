---
title: "Android hizmetleri oluşturma"
description: "Bu kılavuz bir etkin kullanıcı arabirimi olmadan yapılacak işleri izin Android bileşenleridir Xamarin.Android Hizmetleri açıklanır. Hizmetleri zaman alıcıdır hesaplamaları, dosyaları, müzik, oynatma indirme gibi arka planda gerçekleştirilen görevleri için çok sık kullanılan ve benzeri. Hizmetler için uygun farklı senaryoları açıklar ve bunları hem uzun süre çalışan arka plan görevleri gerçekleştirmek için yanı sıra uzak yordam çağrıları için bir arabirim sağlamak için uygulama gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: BA371A59-6F7A-F62A-02FC-28253504ACC9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 77a3db2b36a0fb423ac2481fbde80c1a732cac4b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-android-services"></a>Android hizmetleri oluşturma

_Bu kılavuz bir etkin kullanıcı arabirimi olmadan yapılacak işleri izin Android bileşenleridir Xamarin.Android Hizmetleri açıklanır. Hizmetleri zaman alıcıdır hesaplamaları, dosyaları, müzik, oynatma indirme gibi arka planda gerçekleştirilen görevleri için çok sık kullanılan ve benzeri. Hizmetler için uygun farklı senaryoları açıklar ve bunları hem uzun süre çalışan arka plan görevleri gerçekleştirmek için yanı sıra uzak yordam çağrıları için bir arabirim sağlamak için uygulama gösterilmektedir._

## <a name="android-services-overview"></a>Android Hizmetleri'ne Genel Bakış

Mobil uygulamalar gibi Masaüstü uygulamaları değildir. Ekran Gayrimenkul, bellek, depolama alanı ve bağlı güç kaynağı gibi kaynakları copious miktarda masaüstleri olması, mobil aygıtlar yapın. Bu kısıtlamaların farklı şekilde davranmasına mobil uygulamaları zorlar. Örneğin, bir mobil cihazda küçük ekran genellikle, yalnızca bir uygulama (yani etkinliği) aynı anda görünür olduğu anlamına gelir. Diğer etkinlikler arka plana taşınır ve herhangi bir iş yeri gerçekleştiremezsiniz askıya alınmış bir duruma gönderilir. Yalnızca bir Android uygulaması olduğundan ancak arka planda çalışmaya devam etmek için uygulama mümkün değildir anlamına gelmez. 

Android uygulamaları yapılma en az biri aşağıdaki dört birincil bileşenleri: _etkinlikleri_, _yayın alıcıları_, _içerik sağlayıcıları_ve _Hizmetleri_. Uygulama ile etkileşim kurmak bir kullanıcının kullanıcı arabirimini sağladıkları için etkinlikler birçok mükemmel Android uygulamaları taşlarıdır. Eşzamanlı gerçekleştirme veya arka plan çalışması için geldiğinde, ancak etkinlikler her zaman en iyi seçenek değildir.
 
Android arka plan çalışması için birincil mekanizma _hizmet_. Bir Android hizmeti kullanıcı arabirimi olmadan bazı iş yapmak için tasarlanmış bir bileşendir. Bir hizmet, dosya indirme, müzik veya görüntünün bir filtre uygulayın. Hizmetleri işlemler arası iletişim için de kullanılabilir (_IPC_) Android uygulamalar arasında. Örneğin, bir Android uygulaması başka bir uygulamadan müzik çalar hizmetini kullanabilir veya bir uygulamanın diğer uygulamalara bir hizmeti aracılığıyla (örneğin, bir kişinin iletişim bilgilerini) veri doğurabilir. 

Hizmetler ve arka plan iş gerçekleştirmek için kendi yeteneği kesintisiz ve sıvı kullanıcı arabirimi sağlamak için önemli. Tüm Android uygulamalarını sahip bir _ana iş parçacığı_ (olarak da bilinen bir _kullanıcı Arabirimi iş parçacığı_) etkinlikleri çalıştırdığınız üzerinde. Aygıt yanıt verebilir durumda tutmak için Android saniyede 60 çerçeve hızında kullanıcı arabirimi güncelleştirmek mümkün olması gerekir. Android uygulaması kadar iş ana iş parçacığı üzerinde gerçekleştirir sonra Android kısıtlamayı bıraktırır çerçeveler, sırayla neden olan düzensiz görünmesi UI (bazen de denir _janky_). Başka bir deyişle kullanıcı Arabirimi iş parçacığı üzerinde gerçekleştirilen herhangi bir iş iki çerçeve, yaklaşık 16 milisaniye (60 her çerçeve 1 saniye) arasındaki zaman aralığı içinde tamamlanmalıdır. 

Bu sorunu çözmenin bir geliştirici iş parçacığı bir etkinlikte UI engelleyebilecek bazı işlerini yapmak için kullanabilir. Ancak, bu sorunlara neden olabilir. Android silin ve etkinliğin birden çok örneğini yeniden çok mümkündür. Ancak, Android otomatik olarak bellek sızıntıları sonuçlanabilir iş parçacığı yok etmez. Bu prime örneği olduğunda [aygıt Döndürülmüş](~/android/app-fundamentals/handling-rotation.md) &ndash; Android deneyecek etkinlik örneğini silin ve yenisini oluşturun:

![Cihazı döndürdüğünde 1 örneği yok ve örnek 2 oluşturulur](images/image-01.png)

Bu olası bellek sızıntısı olan &ndash; etkinlik ilk örneği tarafından oluşturulan iş parçacığı hala çalışıyor. İş parçacığı etkinliği ilk örneği başvuru varsa, bu nesne toplama çöp Android engeller. Ancak, etkinliğin ikinci bir örneğini hala (hangi sırayla yeni bir iş parçacığı oluşturabilir) oluşturulur. Aygıt birkaç kez art arda hızlı dönen tüm RAM tüketebilir ve belleği geri almasını tüm sonlandırmak için Android zorla.

Bir etkinlik yapılması iş outlive, altın kural, ardından bir hizmet, çalışmayı gerçekleştirmek için oluşturulmalıdır. İş yalnızca bir etkinlik bağlamında geçerli ise, ancak, çalışmayı gerçekleştirmek için bir iş parçacığı oluşturma daha uygun olabilir. Örneğin, yalnızca bir Fotoğraf Galerisi uygulaması eklenmiş olan bir fotoğraf için bir küçük resim oluşturma büyük olasılıkla bir hizmet olmalıdır. Ancak, bir iş parçacığı bir etkinlik ön planda olsa da, yalnızca heard bazı müzik daha uygun olabilir.

Arka plan iş iki geniş sınıflarda ayrılabilir:

1. **Uzun çalışan görev** &ndash; durduruluncaya kadar devam eden iş budur. Örnek olarak bir _uzun süre çalışan görev_ müzik akışları bir uygulama ya da algılayıcı toplanan verileri izlemek gerekir. Uygulama görünen kullanıcı arabirimi olmadan sahip olsa da bu görevleri çalıştırmanız gerekir.
2. **Periyodik görevler** &ndash; (bazen denir bir _iş_) Periyodik görevi daha kısa bir süre (birkaç saniye) içindeki değil ve bir zamanlamaya göre çalıştırılan biridir (yani günde bir kez bir hafta ya da belki yalnızca bir kez de sonraki 60 saniye). Buna örnek olarak bir dosya Internet'ten veya bir görüntü için bir küçük resim oluşturma.

Dört farklı tür Android hizmetler şunlardır:

* **Hizmet bağlı** &ndash; A _bağlı hizmet_ kendisiyle ilişkili bazı başka bir bileşeninin (genellikle bir etkinliği) sahip bir hizmettir. Bağımlı bir hizmet ilişkili bileşeni ve birbiriyle etkileşim için hizmet veren bir arabirim sağlar. Android hizmetine bağlı hiçbir istemci sonra hizmet kapanır.

* **`IntentService`** &ndash; Bir  _`IntentService`_  özelleştirilmiş sınıfıdır `Service` hizmet oluşturma ve kullanım basitleştirir sınıfı. Bir `IntentService` ayrı otonom çağrıları işlemek için tasarlanmıştır. Aynı anda birden fazla çağrı işleyebilir, bir hizmet farklı bir `IntentService` gibi daha çok bir _iş kuyruğu İşlemci_ &ndash; iş sıraya ve bir `IntentService` her işlem, aynı anda tek iş parçacığı üzerinde işler. Genellikle, bir`IntentService` bir etkinlik veya bir parçasını bağlı değil. 

* **Hizmet başlatıldı** &ndash; A _hizmeti başlatıldı_ bazı diğer Android bileşeni (örneğin, bir etkinlik) tarafından başlatılan bir şey açıkça bildiren kadar continously arka planda çalışan bir hizmettir ve hizmeti durdurmak için. Bağımlı bir hizmet farklı olarak başlatılan bir hizmet doğrudan bağlı istemcilere sahip değil. Bu nedenle, böylece bunlar düzgün biçimde gerektiği gibi yeniden başlatılabilir başlatılan Hizmetleri tasarlamanız önemlidir.

* **Karma hizmet** &ndash; A _karma hizmet_ özelliklerine sahip bir hizmet bir _hizmeti başlatıldı_ ve _bağlı hizmet_. Bir karma hizmet bir bileşenin kendisine bağlar veya bazı olay tarafından başlatılamaz başlatılabilir. Bir istemci bileşeni olabilir veya karma hizmetine bağlı değil. Bir karma hizmeti durdurmak için açıkça belirtilmediği kadar ya da ona bağlı hiçbir istemci kadar çalışmaya devam edecektir.

Hangi tür hizmetini kullanacak biçimde çok uygulama gereksinimlerine bağlıdır. Altın kural, farklı bir `IntentService` veya bu iki tür hizmet birini tercih verilmelidir için bağımlı bir hizmet bir Android uygulamasını gerçekleştirmeniz gerekir, çoğu görevler için yeterli. Bir `IntentService` bir etkinlik/parçası olan sık etkileşimleri gerekli olduğunda bağlı hizmet uygun bulunurken bir dosya indirme gibi "tek adımda" görevler için iyi bir seçimdir. 

Çoğu Hizmetleri arka planda çalıştırırken olarak bilinen özel bir alt kategori yok bir _ön plan hizmet_. Bu, (normal bir hizmete karşılaştırıldığında) daha yüksek bir öncelik (örneğin, müzik Yürütülüyor) kullanıcı için bazı işlemler gerçekleştirmek için verilen bir hizmettir. 

Kendi işleminde aynı cihaza bir hizmeti çalıştırmaya mümkündür, bu bazen olarak adlandırılır bir _uzak hizmet_ veya farklı bir _işlem dışı hizmeti_. Bu oluşturmak için daha fazla çaba gerektirir ancak uygulamanın işlevselliğini başka uygulamalarla paylaşma gerektiği zaman için kullanışlı olabilir ve bazı durumlarda, bir uygulamanın kullanıcı deneyimini geliştirmek olabilir. 

Bu hizmetlerin her birini kendi özelliklerine ve behaviours vardır ve bu nedenle, kendi kılavuzlarındaki daha ayrıntılı ele alınacaktır.
