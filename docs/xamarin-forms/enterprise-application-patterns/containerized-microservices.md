---
title: "Kapsayıcılı mikro"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5872ad92-04e0-4f1a-9691-79d5602f5683
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 3ecbd5a301a64417ab5fb27bd8632b6d9790a7ac
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="containerized-microservices"></a>Kapsayıcılı mikro

İstemci-sunucu uygulamaları geliştirme her katmanında belirli teknolojileri kullanan katmanlı uygulamaları oluşturmaya odaklanmayı sonuçlandı. Bu tür uygulamalar genellikle denir *tek yapılı* uygulamaları ve bu paketlenmiş en yüksek yükler için önceden ölçeklendirilmiş donanıma. Bu geliştirme yaklaşım ana eksileri bileşenleri tek tek bileşenleri kolayca ölçeklendirilemiyor, her katman içinde ve test etme maliyetini arasında sıkı Bağ ' dir. Basit bir güncelleştirmenin katmanı kalan öngörülemeyen etkileri ve bir uygulama bileşeni için bir değişiklik retested imzalanması ve dağıtılması için kendi tüm katmanı gerektirir.

Özellikle ilgili bulut yaşı bileşenleri tek tek kolayca olamaz ölçeklendirilir. Tek yapılı bir uygulama etki alanına özgü işlevselliğini içerir ve genellikle ön uç, iş mantığı ve verileri depolama gibi işlev Katmanlar bölünür. Tek yapılı bir uygulama birden çok makinelere tüm uygulama kopyalanarak Şekil 8-1'de gösterildiği şekilde ölçeklendirilir.

![](containerized-microservices-images/monolithicapp.png "Tek yapılı uygulama ölçeklendirme yaklaşımı")

**Şekil 8-1**: yaklaşım ölçeklendirme tek yapılı uygulama

## <a name="microservices"></a>Microservices

Mikro uygulama geliştirme ve dağıtım için farklı bir yaklaşım, uygun bir yaklaşım çeviklik, ölçek ve modern bulut uygulamaları Güvenilirlik gereksinimleri sunar. Mikro uygulama uygulamanın genel işlevselliğini sunmak için birlikte çalışan bağımsız bileşenlerine ayrılmış. Böylece her mikro hizmet tek bir işlev uygulayan uygulamaları bağımsız sorunları yansıtacak şekilde küçük hizmetlerinden oluşur, terim mikro hizmet vurgular. Ayrıca, diğer mikro iletişim kurar ve veri ile paylaşma böylece her mikro hizmet iyi tanımlanmış sözleşmeleri olur. Mikro tipik örnekleri Alışveriş sepetleri dahil, işleme stok, alt sistemleri ve ödeme işleme satın alın.

Mikro bağımsız olarak, birlikte ölçeği çok büyük tek yapılı uygulamaları karşılaştırıldığında genişletme. Bu, isteğe bağlı desteklemek için daha fazla işlem gücü veya ağ bant genişliği gerektiren özel bir işlev alanı gereksiz yere uygulamanın diğer alanlarında ölçeklendirme yerine Genişletilebilir anlamına gelir. Şekil 8-2 gösterilmektedir burada mikro dağıtılan ve bağımsız olarak ölçeği, bu yaklaşım, makine genelinde Hizmetleri örneklerini oluşturma.

![](containerized-microservices-images/microservicesapp.png "Mikro uygulama ölçeklendirme yaklaşımı")

**Şekil 8-2**: mikro uygulama yaklaşımı ölçeklendirme

Mikro hizmet genişleme yükleri değiştirmeye uyarlamak uygulama izin verme neredeyse anlık olabilir. Örneğin, bir uygulamanın web dönük işlevinde tek mikro ek gelen trafiği işlemeye ölçeğini gereken uygulamada yalnızca mikro hizmet olabilir.

Klasik modeli uygulama ölçeklenebilirlik için kalıcı veri depolamak için bir paylaşılan dış veri deposu ile bir yük dengeli, durum bilgisiz katmanı olmalıdır. Durum bilgisi olan mikro genellikle üzerinde yerleştirildiğinden sunucuya yerel olarak depolamak kendi kalıcı veri yönetmek için Ağ Yükü önlemek için erişim ve karmaşıklığını arası işlemleri hizmet. Bu verileri hızlı olası işlenmesini sağlar ve sistemler önbelleğe alma ihtiyacını ortadan kaldırabilirsiniz. Ayrıca, ölçeklenebilir durum bilgisi olan mikro genellikle veri sonrasında tek bir sunucu destekleyebilir veri boyutu ve aktarım üretilen işini yönetmenizi örneklerini arasında bölüm.

Mikro bağımsız güncelleştirmeleri de destekler. Bu gevşek bağlantı mikro arasında hızlı ve güvenilir uygulama evrimi sağlar. Bağımsız, dağıtılmış yapılarına herhangi bir anda yalnızca tek bir mikro hizmet örneklerinin bir alt burada güncelleştirecektir çalışırken güncelleştirmeleri destekler. Bu nedenle, bir sorun algılanırsa, tüm örnekler hatalı kod veya yapılandırma güncelleştirmeden önce buggy güncelleştirme geri alınabilir. Benzer şekilde, bağımsız olarak hangi mikro hizmet örneği ile iletilen güncelleştirmeleri uygulanıyor olduğunda istemciler sürümünün tutarlı görebilmesi için mikro genellikle şema sürümü, kullanın.

Bu nedenle, mikro hizmet uygulamaları tek yapılı uygulamaları birçok avantaj vardır:

-   Her mikro hizmet görece küçük, yönetmek ve gelişmesi kolaydır.
-   Her mikro hizmet geliştirilen ve diğer hizmetler bağımsız olarak dağıtılabilir.
-   Her mikro hizmet bağımsız olarak ölçeklendirilmiş olabilir. Örneğin, bir katalog hizmeti veya alışveriş sepeti hizmet ölçeklendirilmiş bir sıralama hizmeti birden fazla olması gerekebilir. Bu nedenle, sonuçta elde edilen altyapı daha verimli bir şekilde kaynakları ölçeğini zaman tüketir.
-   Her mikro hizmet sorunları yalıtır. Örneğin, bir hizmetinde bir sorun olduğunda yalnızca bu hizmeti etkiler. Diğer hizmetlerin istekleri işlemeye devam edebilirsiniz.
-   Her mikro hizmet en yeni teknolojileri kullanabilirsiniz. Mikro otonom ve çalışma yan yana olduğundan, en yeni teknolojileri ve çerçeveleri, tek yapılı bir uygulama tarafından kullanılan eski bir çerçeve kullanmak zorunda yerine kullanılabilir.

Ancak, mikro hizmet tabanlı çözüm Ayrıca olası sakıncaları vardır:

-   Her mikro hizmet tamamen otonom, uçtan uca, kendi veri kaynakları için sorumluluk dahil edilecek taşıdığından mikro bir uygulamaya bölüm seçme, zor olabilir.
-   Geliştiriciler uygulama için karmaşıklık ve gecikme süresi ekler hizmet içi iletişim uygulamalıdır.
-   Genellikle birden çok mikro arasındaki atomik işlemleri mümkün değil. Bu nedenle, iş gereksinimlerini mikro arasında nihai tutarlılık çekirdeğin gerekir.
-   Üretim ortamına dağıtma ve birçok bağımsız hizmetlerini tehlikeye bir sistemi yönetme işletimsel bir karmaşıklık söz konusudur.
-   Doğrudan istemci mikro hizmet iletişim mikro sözleşmeleri yeniden düzenlemeniz zorlaştırabilir. Örneğin, zaman içinde Sistem Hizmetleri içine nasıl bölümlenmiş değiştirmeniz gerekebilir. Tek bir hizmeti iki veya daha fazla hizmetlerine bölme ve iki hizmet birleştirme. İstemciler doğrudan mikro ile iletişim kurduğunda, bu düzenleme iş İstemci uygulamalarıyla uyumluluk bozulabilir.

## <a name="containerization"></a>Kapsayıcıyla taşıma

Kapsayıcıyla taşıma olan bir yaklaşım, bir uygulama ile bağımlılıkları yanı sıra, dağıtım bildirim dosyaları olarak soyutlanır ortamı yapılandırmasını sürümlü kümesini paketlenir bir birim olarak test bir kapsayıcı görüntü olarak birlikte yazılım geliştirme ve bir ana bilgisayar işletim sistemi dağıtılır.

Uygulamanın diğer kapsayıcılar veya ana bilgisayar kaynakları dokunmadan nerede çalıştırabilirsiniz bir yalıtılmış, kaynak denetimli ve taşınabilir işletim ortamı, bir kapsayıcıdır. Bu nedenle, bir kapsayıcı arar ve yeni yüklenen bir fiziksel bilgisayara veya bir sanal makine gibi davranır.

Şekil 8-3'te gösterildiği gibi birçok benzerlikler kapsayıcıları ve sanal makineler arasında vardır.

![](containerized-microservices-images/containersvsvirtualmachines.png "Mikro uygulama ölçeklendirme yaklaşımı")

**Şekil 8-3**: sanal makineler ve kapsayıcıları karşılaştırması

Bir kapsayıcı bir işletim sistemi çalıştıran bir dosya sistemine sahip ve bir fiziksel veya sanal makine gibi bir ağ üzerinden erişilebilir. Ancak, teknoloji ve kavramları kapsayıcıları tarafından kullanılan sanal makinelerden çok farklı değildir. Sanal makineler, uygulamalar, gerekli bağımlılıkları ve tam konuk işletim sistemi içerir. Kapsayıcıları uygulama ve onun bağımlılıklarını içerir, ancak işletim sistemi yalıtılmış işlemlerde (yanı sıra özel bir sanal makine başına kapsayıcı içinde çalıştırmak Hyper-V kapsayıcıları) ana bilgisayar işletim sistemi üzerinde çalışan diğer kapsayıcılar paylaşın. Bu nedenle, kapsayıcıları kaynakları paylaşabilir ve genelde sanal makineleri daha az kaynak gerektirir.

Bir kapsayıcı odaklı geliştirme ve dağıtım yaklaşımın avantajı, çoğu tutarsız ortamı kurulumları ve bunlarla gelen sorunları ortaya çıkabilecek sorunları ortadan ' dir. Ayrıca, kapsayıcı gerektiği gibi yeni kapsayıcılar depolamasına tarafından hızlı uygulama büyütme işlevselliği izin verir.

Oluşturma ve kapsayıcılar ile çalışma temel kavramları şunlardır:

-   Kapsayıcı ana bilgisayarı: Fiziksel veya ana bilgisayar kapsayıcıları için yapılandırılmış sanal makine. Kapsayıcı konak bir veya daha fazla kapsayıcıları çalışır.
-   Kapsayıcı görüntü: Görüntünün üst üste katmanlı bağlanan dosya sistemlerinin birleşimi oluşur ve bir kapsayıcı temelini. Bir görüntü durumu yok ve farklı ortamlar için dağıtılan gibi hiçbir zaman değiştirir.
-   Kapsayıcı: Bir kapsayıcı, görüntünün bir çalışma zamanı örneğidir.
-   Kapsayıcı işletim sistemi görüntüsü: Kapsayıcıları görüntülerden dağıtılır. Kapsayıcı işletim sistemi görüntüsünü bir kapsayıcı olun potansiyel olarak birçok görüntü katmanlarında ilk katmanıdır. Bir kapsayıcı işletim sistemi sabittir ve değiştirilemez.
-   Kapsayıcı deposu: kapsayıcı görüntü her oluşturulduğunda görüntü ve bağımlılıklarını yerel deposunda depolanır. Bu görüntüleri kapsayıcı konakta birçok kez yeniden kullanılabilir. Kapsayıcı görüntüleri de ortak veya özel kayıt defterinde gibi depolanabilir [Docker hub'a](https://hub.docker.com/), böylece farklı kapsayıcı ana bilgisayarlar arasında kullanılır.

Mikro hizmet uygulama tabanlı uygulamaları ve Docker çoğu yazılım platformlarının ve bulut satıcıları tarafından benimsenen standart kabı duruma kuruluşlar kapsayıcıları giderek geliştirilmektedir.

EShopOnContainers başvuru uygulaması Docker kapsayıcılı dört uç mikro barındırmak için Şekil 8-4'te gösterildiği gibi kullanır.

![](containerized-microservices-images/microservicesarchitecture.png "Uygulama arka uç mikro eShopOnContainers başvurusu")

**Şekil 8-4**: eShopOnContainers uygulama arka uç mikro başvurusu

İş mikro ve kapsayıcılar formunu birden çok otonom alt sistemleri içine başvuru uygulaması arka uç Hizmetleri'nde mimarisini ayrılmış. Tek bir alanı işlevlerin her mikro hizmet sağlar: bir kimlik hizmeti, katalog hizmeti, bir sıralama hizmeti ve sepeti hizmet.

Her mikro hizmet tam olarak diğer mikro ayrılmış izin veren kendi veritabanı vardır. Gerektiğinde, farklı mikro veritabanlarından arasında tutarlılığı uygulama düzeyi olaylarını kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz: [arasındaki iletişimi mikro](#communication_between_microservices).

Başvuru uygulaması hakkında daha fazla bilgi için bkz: [.NET mikro: mimarisi kapsayıcılı .NET uygulamaları için](https://aka.ms/microservicesebook).

<a name="communication_between_client_and_microservices" />

## <a name="communication-between-client-and-microservices"></a>İstemci ve mikro hizmetler arasında iletişim

EShopOnContainers mobil uygulamayı kullanarak kapsayıcılı arka uç mikro ile iletişim kurar *istemci mikro doğrudan* Şekil 8-5'te gösterilen iletişim.

![](containerized-microservices-images/directclienttomicroservicecommunication.png "Mikro uygulama ölçeklendirme yaklaşımı")

**Şekil 8-5**: doğrudan istemci mikro hizmet iletişimi

Doğrudan istemci mikro hizmet iletişim ile mobil uygulama mikro hizmet başına farklı bir TCP bağlantı noktası ile kendi ortak uç nokta aracılığıyla doğrudan her mikro hizmet istekleri yapar. Üretimde uç nokta genellikle kullanılabilir örneklerinde istekleri dağıtır mikro ait yük dengeleyici eşleme.

> [!TIP]
> API ağ geçidi iletişim kullanmayı düşünün. Doğrudan istemci mikro hizmet iletişim dezavantajları tabanlı büyük ve karmaşık mikro hizmet oluşturma, ancak küçük bir uygulama için birden fazla yeterli olabilir. Mikro onlarca uygulamada dayalı olarak büyük mikro hizmet tasarlama, API ağ geçidi iletişim kullanmayı düşünün. Daha fazla bilgi için bkz: [.NET mikro: mimarisi kapsayıcılı .NET uygulamaları için](https://aka.ms/microservicesebook).

<a name="communication_between_microservices" />

## <a name="communication-between-microservices"></a>Mikro arasındaki iletişim

Mikro tabanlı uygulama büyük olasılıkla birden çok makinelerde çalışan dağıtılmış bir sistemdir. Her hizmet örneği genellikle bir işlemdir. Bu nedenle, hizmetleri bir işlemler arası iletişim protokolü, örneğin HTTP, TCP, Gelişmiş Message Queuing Protokolü (AMQP) veya her hizmetin yapısına bağlı olarak ikili protokoller kullanarak etkileşim kurmalıdır.

Mikro hizmet mikro hizmet iletişim için iki ortak yaklaşımlar HTTP REST iletişimini veri ve basit zaman uyumsuz güncelleştirmeleri arasında birden çok mikro kurarken Mesajlaşma sorgulanırken dayalıdır.

Zaman uyumsuz ileti tabanlı olay denetimli iletişim değişiklikleri arasında birden çok mikro yayıldığında önemlidir. Bu yaklaşımda, olaya bir şey olduğunda iş varlığı güncelleştirir dikkat çeken bir şey, örneğin, olur bir mikro hizmet yayımlar. Diğer mikro bu olaylara abone olma. Ardından, bir mikro hizmet bir olayı aldığında, daha fazla olay yayımlanmakta sırayla neden olabilir, kendi iş varlıklarını güncelleştirir. Bu yayımlama-abone olma işlevselliği genellikle bir olay bus ile elde edilir.

Bir olay veri yolu sağlayan yayımlama-abone olma Şekil 8-6'da gösterildiği gibi birbirinden açıkça farkında olması gereken bileşenleri gerektirmeden mikro arasındaki iletişim.

![](containerized-microservices-images/eventbus.png "Bir olay bus ile yayımlama-abonelik")

**Şekil 8-6:** Yayımla-abone bir olay bus ile

Bir uygulama açısından olay veri yolu yalnızca bir yayımlama olduğu-bir arabirim kullanıma sunulan kanal abone olun. Ancak, olay bus uygulandığı yol farklılık gösterebilir. Örneğin, bir olay veri yolu uygulaması RabbitMQ, Azure Service Bus ve diğer hizmet yollarına NServiceBus ve MassTransit gibi kullanabilirsiniz. Şekil 8-7 bir olay veri yoluna eShopOnContainers başvuru uygulamada nasıl kullanıldığını gösterir.

![](containerized-microservices-images/microservicesarchitecturewitheventbus.png "Olay kaynaklı başvuru uygulaması iletişiminde zaman uyumsuz")

**Şekil 8-7:** olay denetimli başvuru uygulaması iletişiminde zaman uyumsuz

EShopOnContainers olay RabbitMQ, kullanılarak uygulanan veri yolu, bir çok zaman uyumsuz yayımlama-abone işlevselliği sağlar. Bu, bir olay yayımladıktan sonra olabileceğini aynı olay için dinleme birden çok aboneye anlamına gelir. Şekil 8-9, bu ilişkiyi gösterir.

![](containerized-microservices-images/eventdrivencommunication.png "Bir çok iletişimi")

**Şekil 8-9**: bir çok iletişimi

Bu bir çok iletişimi yaklaşım olayları birden fazla hizmet, hizmetler arasında nihai tutarlılık sağlamaya span iş işlemleri uygulamak üzere kullanır. Tutarlı son işlem dağıtılmış adımları bir dizi oluşur. Bu nedenle, kullanıcı profil mikro hizmet UpdateUser komutu aldığında, güncelleştirmelerinin veritabanındaki kullanıcı ayrıntıları ve UserUpdated olay olay veri yoluna yayımlar. Sepeti mikro hizmet ve sıralama mikro hizmet bu olay alırsınız ve bunların ilgili veritabanlarını alıcı bilgilerinde yanıt olarak güncelleştirmek için abone oldunuz.

> [!NOTE]
> EShopOnContainers olay RabbitMQ, kullanılarak uygulanan veri yolu, yalnızca bir kavram kanıtı kullanılmak üzere tasarlanmıştır. Üretim sistemleri için alternatif olay veri yolu uygulamaları dikkate alınmalıdır.

Olay veri yolu uygulaması hakkında daha fazla bilgi için bkz: [.NET mikro: mimarisi kapsayıcılı .NET uygulamaları için](https://aka.ms/microservicesebook).

## <a name="summary"></a>Özet

Mikro uygulama geliştirme ve modern bulut uygulamaları çeviklik, ölçek ve güvenilirlik gereksinimlerine uygun dağıtım bir yaklaşım sunmaktadır. Ana avantajları mikro, bunlar bağımsız olarak, belirli bir işlevsel alan isteğe bağlı olarak gereksiz yere alanlarının ölçeklendirmeden desteklemek için daha fazla işlem gücü veya ağ bant genişliği gerektiren genişletilebilir, yani ölçeklendirilmiş olabileceğini biridir artan talep etkilenmeyen uygulama.

Uygulamanın diğer kapsayıcılar veya ana bilgisayar kaynakları dokunmadan nerede çalıştırabilirsiniz bir yalıtılmış, kaynak denetimli ve taşınabilir işletim ortamı, bir kapsayıcıdır. Mikro hizmet uygulama tabanlı uygulamaları ve Docker çoğu yazılım platformlarının ve bulut satıcıları tarafından benimsenen standart kabı duruma kuruluşlar kapsayıcıları giderek geliştirilmektedir.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
