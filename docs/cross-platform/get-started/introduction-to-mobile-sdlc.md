---
title: Mobil yazılım geliştirme yaşam döngüsü giriş
description: Bu makalede, mobil uygulamalar göre yazılım geliştirme yaşam döngüsü açıklar ve bazı mobil projeleri oluştururken gerekli konuları ele alır. Yalnızca sağ ve oluşturmaya başlayın atlamak isteyen geliştiriciler için bu kılavuzda atlandı ve daha sonra daha eksiksiz bir mobil geliştirme anlamak için okuyun.
ms.prod: xamarin
ms.assetid: 420c5fdf-4610-4e71-9db5-fe894c961924
author: asb3993
ms.author: amburns
ms.date: 11/22/2016
ms.openlocfilehash: c93a063c9c933e1b9f397d172115471473cf8f35
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-the-mobile-software-development-lifecycle"></a>Mobil yazılım geliştirme yaşam döngüsü giriş

_Bu makalede, mobil uygulamalar göre yazılım geliştirme yaşam döngüsü açıklar ve bazı mobil projeleri oluştururken gerekli konuları ele alır. Yalnızca sağ ve oluşturmaya başlayın atlamak isteyen geliştiriciler için bu kılavuzda atlandı ve daha sonra daha eksiksiz bir mobil geliştirme anlamak için okuyun._

Mobil uygulamaları oluşturmak IDE'yi açma, bir şey birlikte atma, sınama ve bir uygulama mağazasının – tüm bir öğleden sonra yapılan gönderme hızlı bir bit yapılması kadar kolay olabilir. Veya ayrıntılı eylemli tasarım, kullanılabilirlik, cihazları, bir tam beta yaşam döngüsü ve ardından dağıtım binlerce üzerinde birkaç farklı yolla sınama QA sınamaları içerir son derece katılan bir işlem olabilir.

Bu belgede dahil olmak üzere, mobil uygulamaları oluşturma kapsamlı bir giriş incelemesi alın oluşturacağız:

1.   **İşlem** – yazılım geliştirme sürecinin yazılım geliştirme yaşam döngüsü (SDLC) adı verilir. Mobil uygulama geliştirme göre SDLC tüm aşamaları sizi inceleyeceğiz dahil olmak üzere: esin, tasarım, geliştirme, sabitlemeyi, dağıtım ve Bakım.
1.   **Dikkat edilecek noktalar** – özellikle geleneksel web veya Masaüstü uygulamaları aksine mobil uygulamalar oluştururken dikkat edilecek noktalar sayısı olan. Bu noktalar inceleyeceğiz ve mobil geliştirme nasıl etkilediklerini.

Bu belge, aynı şekilde yeni ve deneyimli uygulama geliştiricileri için mobil uygulama geliştirme hakkında temel soruları yanıtlamak için tasarlanmıştır. Tüm yazılım geliştirme yaşam döngüsü (SDLC) sırasında içine çalıştıracaksınız kavramları çoğunu tanıtımı için oldukça kapsamlı bir yaklaşım sürer. Ancak, bu belgede olmayabilir herkes için yalnızca uygulamalar oluşturmaya başlamak için sabırsızlanıyorsanız varsa, şimdi atlama öneririz [mobil geliştirme giriş](~/cross-platform/get-started/introduction-to-mobile-development.md) Kılavuzu'na ve ardından bu belgede daha sonra gelmeye.

## <a name="mobile-development-sdlc"></a>Mobil Geliştirme SDLC

Mobil geliştirme yaşam döngüsü büyük ölçüde Hayır SDLC web veya Masaüstü uygulamaları için farklıdır. Olanla gibi genellikle vardır işleminin 5 ana bölümleri:

1.   **Başlangıcı** – tüm uygulamalar bir fikirle başlayın. Bu fikir genellikle bir uygulama için sağlam bir temel içine iyileştirilmiştir.
1.   **Tasarım** – nasıl çalıştığını, genel yerleşimi hangi olduğu gibi uygulamanın kullanıcı deneyimini (UX) tanımlama tasarım aşamasında oluşur yardımıyla bir grafik Tasarımcı uygun bir kullanıcı arabirimi (UI) tasarımı içine bu UX kapatma yanı sıra, vb.
1.   **Geliştirme** – genellikle en kaynak yoğunluklu, bu uygulamanın gerçek yapı aşamasıdır.
1.   **Sabitlemeyi** – geliştirme bunların, QA uygulamayı test etme için genellikle başlar ve düzeltilen kadar yeterli olduğunda. Bir uygulama bir sınırlı beta aşamasında gireceğini bazen daha geniş bir kullanıcı kitleye kullanın ve geri bildirim sağlamak ve değişiklikler hakkında bilgilendirmek için bir fırsat verilir.
1.  **Dağıtım**

Genellikle bu bilgilerin çoğunu çakışan, örneğin, kullanıcı arabirimini sonlandırılır ve kullanıcı Arabirimi tasarımı bile bildirebilir geçmeden olması geliştirme için yaygın bir sorundur. Ayrıca, bir uygulama yeni bir sürüme eklenmekte olan yeni özellikleri aynı sabitlemeyi aşamada içine giriyor olabilir.

Ayrıca, bu aşamalar SDLC yöntemlerini Çevik, Spiral, Waterfall vb. gibi herhangi bir sayıda kullanılabilir.

Bunların her biri olacak aşamaları açıklanmaktadır tarafından aşağıdaki bölümlerde daha ayrıntılı.

### <a name="inception"></a>Başlangıcı

Kişiler olan mobil cihazları etkileşimi düzeyini ve yaygınlığıyla, neredeyse herkesin bir mobil uygulama için bir fikir sahip olduğu anlamına gelir. Mobil aygıtlar bilgi işlem, web ve hatta şirket altyapı ile etkileşim kurmak için yepyeni bir yol açın.

Tanımlama ve bir uygulama için bir fikir daraltmayı hakkında başlangıcı aşamasıdır.
Başarılı bir uygulama oluşturmak için temel bazı sorular sormak önemlidir. Bir uygulama bir genel uygulama mağazaları yayımlamadan önce dikkate alınması gereken bazı noktalar şunlardır:

-   **Rekabet avantajı** – ölçeklendiriyor benzer uygulamalar zaten vardır? Nasıl varsa, bu uygulama diğer bilgisayarlardan ayırt etmez?

Bir kuruluşta dağıtılan uygulamalar için:

-   **Altyapı tümleştirme** – hangi mevcut altyapıya tümleştirileceğini veya genişletmek?

Ayrıca, uygulamaları mobil form faktörü bağlamında değerlendirilmelidir:

-   **Değer** – hangi değeri kullanıcılar bu uygulamayı getirir? Bunu nasıl kullanacakları?
-   **Form/Mobility** – nasıl bu uygulamayı çalışır bir mobil form faktörünü? Nasıl ı ekleyebilirsiniz konumu tanıma, kamera, vb. gibi mobil teknolojileri kullanarak değeri.?

Bir uygulama işlevselliğini tasarlama konusunda yardımcı olmak için onu aktörler tanımlamak yararlı olabilir ve [kullanım örnekleri](http://en.wikipedia.org/wiki/Use_case). Aktör rolleri uygulama içindeki ve genellikle kullanıcılar. Kullanım genellikle bir eylem veya hedefleri örnekleridir.

İzleme uygulaması bir görev örneği için iki aktörler sahip olabilir: *kullanıcı* ve *arkadaş*. Bir kullanıcı olabilir *bir görev oluşturma*, ve *görev paylaşmak* bir arkadaş ile. Bu durumda, bir görev oluşturma ve görev paylaşımı aktörler ile art arda ne ekranları bilgilendirir, iki farklı kullanım örneklerini oluşturmak, hangi iş varlıkları ve mantığı geliştirilecek gerekecek yanı sıra gerekir'dır.

Uygun sayıda kullanım örnekleri ve aktörler yakalandıktan sonra bir uygulama tasarlamaya başlamak çok daha kolaydır. Geliştirme sonra uygulamasının nasıl oluşturulacağını, yerine ne uygulama olduğu veya yapmalısınız odaklanabilirsiniz.

### <a name="designing-mobile-applications"></a>Mobil uygulamalar tasarlama

Uygulamanın işlevselliğini ve özelliklerini belirlenmesinden sonra sonraki adım, kullanıcı deneyimi veya UX çözmeye çalışırken başlangıç olacaktır.

#### <a name="ux-design"></a>UX tasarım

UX wireframes veya çok birini kullanarak alıştırmalar aracılığıyla genellikle Bitti [tasarım araç takımları](https://docs.microsoft.com/windows/uwp/design/downloads/). UX alıştırmalar gerçek UI tasarımı hakkında endişelenmeye gerek kalmadan tasarlanmalıdır UX izin ver:

 [![](introduction-to-mobile-sdlc-images/balsamiq.png "UX genellikle wireframes veya Balsamiq gibi araçları kullanarak alıştırmalar aracılığıyla gerçekleştirilir")](introduction-to-mobile-sdlc-images/balsamiq.png#lightbox)

UX alıştırmalar oluştururken, uygulama hedeflediğini çeşitli platformlar için arabirimi yönergeleri dikkate almak önemlidir. Uygulama "evde her platformda geliyor olmalıdır". Her platform için resmi tasarım yönergeleri şunlardır:

1.   **Apple** -  [İnsan Arabirimi yönergelerine](https://developer.apple.com/ios/human-interface-guidelines/overview/themes/)
1.   **Android** – [tasarım yönergeleri](http://developer.android.com/design/index.html)
1.   **UWP** – [UWP tasarım temelleri](https://docs.microsoft.com/windows/uwp/design/basics/)

Örneğin, her uygulamanın uygulama bölümleri arasında geçiş yapmak için bir benzetimini vardır. iOS sekme çubuğunu ekranın alt kısmında, Android, ekranın en üstünde bir sekme çubuğunu kullanır ve UWP kullanır [PIVOT veya sekmesinde](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tabs-pivot) görünümü.

Ayrıca, donanım UX kararları belirler. Örneğin, iOS cihazları hiçbir fiziksel sahip *geri* düğmesine tıklayın ve bu nedenle Gezinti denetleyicisi benzetimini sunar:

 ![](introduction-to-mobile-sdlc-images/01-navigation-controller.png "iOS cihazları geri düğmesi ve bu nedenle Gezinti denetleyicisi benzetimini tanıtmak hiçbir fiziksel sahip")

Ayrıca, form faktörünün UX kararları etkiler. Tablet çok daha fazla Gayrimenkul vardır ve bu nedenle daha fazla bilgi görüntüleyebilirsiniz. Bir telefon birden çok ekranlarda ihtiyaç duyduğu, tek bir tablet için genellikle sıkıştırılır:

 [![](introduction-to-mobile-sdlc-images/iphone-vs-ipad.png "Genellikle bir telefon birden çok ekranlarda ihtiyaç duyduğu tek bir tablet için sıkıştırılır")](introduction-to-mobile-sdlc-images/iphone-vs-ipad.png#lightbox)

Ve form faktörleri dışarıdan çözümlenebilen nedeniyle genellikle de hedeflemek isteyebilirsiniz orta ölçekli form faktörleri (herhangi bir yerde arasında bir telefon ve tablet).

#### <a name="user-interface-ui-design"></a>Kullanıcı Arabirimi (UI) tasarımı

UX belirlendikten sonra sonraki adıma UI tasarım oluşturmaktır. UX genellikle yalnızca siyah beyaz alıştırmalar olmakla birlikte, kullanıcı Arabirimi tasarımı burada renkler, grafik, vb., sunulan ve kesin olarak aşamasıdır. Genellikle, profesyonel bir tasarım en popüler uygulamalara sahip ve iyi UI tasarım zamanı harcama önemlidir.

UX ile her platform olduğunu anlamak önemli olduğu gibi olan kendi tasarım dil, iyi tasarlanmış bir uygulama hala şekilde her platformda farklı görünür:

 [![](introduction-to-mobile-sdlc-images/multiplatform-1.png "İyi tasarlanmış bir uygulama hala her platformda farklı görünebilir")](introduction-to-mobile-sdlc-images/multiplatform-1.png#lightbox)

### <a name="development"></a>Geliştirme

Geliştirme aşaması genellikle çok erken başlatır. Bir fikir kavramsal/esin aşamasında bazı maturation olduğunda, aslında, genellikle bir çalışma prototip işlevselliği, varsayımlara ve iş kapsamını bir anlayış vermek için yardımcı doğrulayan geliştirilir.

Öğreticiler kalan biz büyük ölçüde geliştirme aşamaya odak.

### <a name="stabilization"></a>Sabitlemeyi

Sabitlemeyi uygulamanızdaki hataların çıkışı çalışma işlemidir. Değil yalnızca bir işlev açısından örn: "Bu düğmeye tıkladığınızda, ayrıca kullanılabilirliğini ve performansını kilitleniyor". Yüksek maliyetli hale gelmeden indirmelere düzeltmeleri gerçekleştirilmesi sabitlemeyi geliştirme süreci içinde çok erken başlatmak en iyisidir. Uygulamalar genellikle gitmesi *prototip*, *alfa*, *Beta*, ve *Sürüm Adayı* aşamaları. Farklı kişilerin bunlar farklı tanımlar, ancak bunlar genellikle aşağıdaki düzeni izleyin:

1.   **Prototip** – uygulama hala kavram kanıtı aşaması ve yalnızca çekirdek işlevselliğini olur veya uygulamanın belirli bölümlerine çalışıyor. Önemli hatalar yok.
1.   **Alpha** – çekirdek işlevselliğini genellikle kod tamamlama (yerleşik ancak tam olarak test). Önemli hatalar hala mevcut, harici işlevselliği hala bulunmuyor olabilir.
1.   **Beta** – çoğu işlevselliği tamamlanmıştır ve en az hafif test ve hata düzeltme oluşturdu. Önemli bilinen sorunları hala mevcut olabilir.
1.   **Sürüm Adayı** – tüm tam ve test edilmiş bir işlevdir. Yeni hatalar, uygulama, joker sürüme bir adaydır.

Bir uygulamayı test etmek başlamak için hiçbir zaman çok erken değil. Örneğin, bir ana sorunu prototip aşamasında bulunursa, uygulamanın UX hala uyum için değiştirilebilir. Bir performans sorunu alfa aşamasında bulunursa, çok fazla kod üstünde yanlış varsayımlar yerleşik önce mimarisi değiştirmek için erken.

Genellikle, bir uygulama boyunca çevriminin daha ayrıntılı olarak hareket ederken deneyin, test, vb. geri bildirim sağlamak için daha fazla kişinin açtı. Örneği için prototip uygulamalar yalnızca gösterilen veya olabilir erken erişimi için kaydolma müşterilere Sürüm Adayı uygulamaları dağıtılmış ancak önemli katılımcılara kullanılabilir.

Erken test etme ve nispeten az cihazlara dağıtımı için genellikle düz bir geliştirme makineden dağıtma yeterli olur. Ancak, hedef kitleyi widens gibi bu hızla sıkıcı olabilir. Bu nedenle, bir sınama havuzu kişileri davet izin vererek bu işlem çok daha kolay hale test dağıtım seçenekleri ölçeklendiriyor sayısı vardır, yayın web üzerinden oluşturur ve kullanıcı geri bildirim için izin araçları sunar.

Test ve dağıtım için kullanabileceğiniz [Uygulama Merkezi](https://appcenter.ms/) sürekli derleme, test, sürüm ve uygulamalarını izlemek için.

### <a name="distribution"></a>Dağıtım

Uygulama sabitlendi sonra joker kullanıma alma zamanı geldi. Platforma bağlı olarak farklı dağıtım seçenekleri mevcuttur.

#### <a name="ios"></a>iOS

Xamarin.iOS ve Objective-C uygulamaları tam olarak aynı şekilde dağıtılır:

1.   **Apple App Store** – Apple App Store deposudur Mac OS X iTunes oluşturulan genel olarak kullanılabilir çevrimiçi uygulama. Uygulamalar için en yaygın dağıtım yöntemi kullanarak olduğu ve pazara ve uygulamalarını çok az çaba ile çevrimiçi dağıtmak geliştiricilere sağlar.
1.   **Şirket içi dağıtım** – genel olarak App Store kullanılabilir olmayan şirket uygulamalarının iç dağıtım için şirket içi dağıtım yöneliktir.
1.   **Geçici dağıtım** – geçici dağıtım birincil olarak geliştirme ve test için tasarlanmıştır ve düzgün şekilde sağlanan aygıtlar için sınırlı sayıda dağıtmanıza olanak tanır. Mac için Xcode ya da Visual Studio aracılığıyla bir cihaza dağıttığınızda, geçici dağıtımı olarak bilinir.

#### <a name="android"></a>Android

Tüm Android uygulamaları dağıtılmış önce imzalanması gerekir. Geliştiriciler, özel bir anahtar tarafından korunan kendi sertifikasını kullanarak kendi uygulamalarında oturum açın. Bu sertifika zinciri Geliştirici oluşturulan ve yayımlanan uygulamalar için uygulama geliştiricisi bağlar Orijinallik sağlayabilir.
Bu, Android için geliştirme sertifikası tanınan bir sertifika yetkilisi tarafından imzalanması olsa da, çoğu geliştirici değil Bu hizmetleri kullanan aktarmayı ve sertifikalarını kendinden imzalı olduğunu not gerekir. Sertifikalar için ana amacı farklı geliştiriciler ve uygulamalar arasında ayırt sağlamaktır.
Android uygulamaları ve Android işletim sistemi içinde çalışan bileşenleri arasındaki izinleri için temsilci seçme zorlama yardımcı olmak için bu bilgileri kullanır.

Diğer popüler mobil platformları, Android uygulaması dağıtımı için çok açık bir yaklaşım alır. Cihazlar için tek, onaylanan uygulama mağazası kilitli.
Bunun yerine, herhangi bir uygulama deposu oluşturmak boş olduğundan ve en Android telefonlar uygulamaların bu üçüncü taraf depoları yüklenmesine izin verme.

Bu, geliştiricilerin uygulamaları için büyük olasılıkla daha büyük henüz daha karmaşık bir dağıtım kanal sağlar. [Google Play](https://play.google.com/store?hl=en) Google resmi uygulama deposu, ancak diğer birçok vardır. Birkaç popüler olanlardır:

1.  [AppBrain](http://www.appbrain.com/)
1.  [Android için Amazon uygulama Mağazası](http://www.amazon.com/mobile-apps/b?ie=UTF8&amp;node=2350149011)
1.  [Handango](http://www.handango.com/)
1.  [GetJar](http://www.getjar.com/)

#### <a name="uwp"></a>UWP 

UWP uygulamalar Microsoft Store aracılığıyla kullanıcılara dağıtılır. Geliştiriciler uygulamalarını deposunda sonra göründükleri onaya gönderir. UWP'ın Windows uygulamalarını yayımlama ile ilgili daha fazla bilgi için bkz: [Yayımla](https://docs.microsoft.com/windows/uwp/publish/) belgeleri.

## <a name="mobile-development-considerations"></a>Mobil geliştirme hakkında önemli noktalar

Mobil uygulamaları geliştirme temelde farklı olmasa da, geleneksel web/masaüstü geliştirme işlemi veya mimarisi bakımından, dikkat edilmesi gereken bazı noktalar vardır.

### <a name="common-considerations"></a>Ortak dikkat edilecek noktalar

#### <a name="multitasking"></a>Çoklu

Bir mobil cihazda (seferde çalıştıran birden çok uygulamalara sahip) çoklu için iki önemli zorluklar mevcuttur. İlk olarak, sınırlı ekran Gayrimenkul verilen, aynı anda birden çok uygulamaları görüntülemek zor olabilir. Bu nedenle, mobil cihazlarda yalnızca bir uygulama ön planda aynı anda olabilir. İkinci olarak, çoklu uygulamaları açık ve gerçekleştirme görevleri hızla pil gücü kullanabilirsiniz sahip.

Çoklu, her platform, bir süre sonra ele alacağız farklı şekilde işler.

#### <a name="form-factor"></a>Form faktörü

Mobil cihazlar genellikle iki kategoriye, telefonlar ve tabletler, birkaç çapraz aygıtlarla arasında ayrılır. Bu form faktörleri için geliştirme genellikle çok benzer, ancak bunlar için uygulamalar tasarlama çok farklı olabilir.
Telefonları sahip çok sınırlı ekran alanı ve tabletler, büyük sırada olan bile çoğu dizüstü bilgisayarlar daha az ekranı alan hala mobil aygıtlar. Bu nedenle, mobil platform UI denetimleri özellikle küçük form faktörleri üzerinde etkili olacak şekilde tasarlanmıştır.

#### <a name="device-and-os-fragmentation"></a>Aygıt ve işletim sistemi parçalanması

Tüm yazılım geliştirme yaşam döngüsü boyunca farklı cihaz dikkate almanız önemlidir:

1.   **Kavramsallaştırılması ve planlama** – bu donanım göz önünde bulundurun ve Özellikler cihaz için cihazın, belirli üzerinde dayalı bir uygulama değişir özellikleri belirli cihazlarda düzgün çalışmayabilir. Örneğin, tüm aygıtları kameraları sahip uygulama Mesajlaşma video oluşturuyorsanız, bazı cihazların videoları oynatma, ancak bunları geçmeyecek mümkün olabilir.
1.   **Tasarım** – dikkat edin farklı ekran oranları ve boyutları aygıtlar arasında bir uygulamanın kullanıcı deneyimini (UX) tasarlarken. Ayrıca, bir uygulamanın kullanıcı arabirimi (UI) tasarlarken, farklı ekran çözünürlükleri dikkate alınmalıdır.
1.   **Geliştirme** – bir kod, bu özellik varlığını özelliğini kullanarak her zaman ilk test edilmelidir. Örneğin, kamera gibi bir cihaz özelliği kullanmadan önce her zaman bu özelliği varlığını OS ilk sorgu. Sonra özellik/aygıt başlatılırken bu cihaz hakkında OS gelen şu anda desteklenen istek emin olun ve sonra bu yapılandırma ayarlarını kullanın.
1.   **Sınama** – genellikle üzerinde gerçek cihazlar ve uygulama erken test etmek son derece önemlidir. Aynı donanım özellikleri bile aygıtlarla yaygın'ndaki davranışları farklılık gösterebilir.

#### <a name="limited-resources"></a>Sınırlı kaynak

Mobil cihazların her zaman daha güçlü almak ancak Masaüstü veya dizüstü bilgisayar kıyasla kapasitesi sınırlı olduğundan hala mobil aygıtları etmektedir. Örneğin, Masaüstü geliştiriciler genellikle bellek kapasitesi hakkında endişelenmeyin. Mobil cihazlarda, hızlı bir şekilde tüm kullanılabilir belleği yüksek kaliteli resimleri sayıda yükleyerek tüketebileceği ancak fiziksel ve sanal bellek copious miktarlarında sahip olmak için kullanılırlar.

Ayrıca, işlemci kullanımı yoğun uygulamalar oyunlar veya metin tanıma gibi gerçekten mobil CPU vergi ve cihaz performansı olumsuz etkileyebilir.

Bu gibi nedeniyle, akıllı kod ve erken ve genellikle yanıt verdiğini doğrulamak için gerçek cihazlara dağıtmak için önemlidir.

### <a name="ios-considerations"></a>iOS dikkat edilecek noktalar

#### <a name="multitasking"></a>Çoklu

Çoklu çok sıkı bir şekilde iOS içinde denetlenir ve kuralların sayısını vardır ve başka bir uygulama için ön plan, aksi takdirde uygulamanız geldiğinde, uygulamanız için uymalıdır davranışları iOS tarafından sonlandırılacak.

#### <a name="device-specific-resources"></a>Aygıta özgü kaynakları

Belirli form faktörü içinde donanım önemli ölçüde farklı modelleri arasında farklılık gösterebilir. Örneğin, arka dönük kamera bazı aygıtlarınızda, de bazı ön dönük kamera ve bazı yok.

Bazı eski cihazlar (iPhone 3G ve daha eski) çoklu bile izin verme.

Aygıt modelleri arasındaki bu farklılıkları nedeniyle bir özellik için bunu kullanmayı denemeden önce olup olmadığını denetlemek önemlidir.

#### <a name="os-specific-constraints"></a>İşletim sistemi özel kısıtlamaları

Uygulamaları esnek ve güvenli olduğundan emin olmak için iOS uygulamaları tarafından uymanız gereken kuralların sayısını zorlar. Çoklu ilgili kurallarının yanı sıra, bir dizi olay yöntemi dışında uygulamanızı belirli bir miktar süre içinde aksi iOS tarafından sonlandırıldı döndürmelidir vardır.

Ayrıca belirtmeye değerinde uygulamaları uygulamanızı erişebilecek kişileri kısıtlayın güvenlik kısıtlamaları zorlayan bir ortamda bir korumalı bilinen içinde çalıştırın. Örneği için bir uygulama okuma ve kendi dizinine yazma, ancak bunu başka bir uygulama dizinine yazma girişiminde bulunursa sonlandırılacak.

### <a name="android-considerations"></a>Android dikkat edilecek noktalar

#### <a name="multitasking"></a>Çoklu

Android görevli iki bileşene sahiptir; Birincisi, aktivite yaşam döngüsü olmasıdır. Bir Android uygulamasını her ekranında etkinlik tarafından temsil edilen ve belirli bir uygulamanın arka planda yerleştirilir veya ön plana gelir kullanırken oluşan olaylar kümesi yok. Uygulamalar, esnek, iyi çalışan uygulamaları oluşturmak için bu yaşam döngüsünün uyması gerekir. Daha fazla bilgi için bkz: [etkinlik yaşam döngüsü](~/android/app-fundamentals/activity-lifecycle/index.md) Kılavuzu.

Android görevli ikinci Bileşen Hizmetleri kullanımıdır.
Hizmetleri mevcut uzun süre çalışan işlemleri uygulamadan bağımsız ve uygulama arka planda olsa da işlemleri yürütmek için kullanılır. Daha fazla bilgi için bkz: [Oluşturma Hizmetleri](~/android/app-fundamentals/services/index.md) Kılavuzu.

#### <a name="many-devices-and-many-form-factors"></a>Birçok cihaz ve birçok Form faktörleri

Google Android işletim sistemi hangi cihazlarda çalıştırabilirsiniz herhangi bir sınır koymak değil. Çok sayıda farklı aygıtlarla çok farklı donanım, ekran çözünürlükleri ve oranları, cihaz özelliklerini ve yeteneklerini tarafından doldurulan bir ürün ortamında bu açık kip sonuçlanır.

Aşırı parçalanma Android aygıtların nedeniyle çoğu kişi tasarlayın ve için test etmek için en popüler 5 veya 6 aygıtları seçin ve bu öncelik.

#### <a name="security-considerations"></a>Güvenlik Değerlendirmeleri

Tüm Android işletim sistemi uygulamalarda ayrı, yalıtılmış bir kimlik altında sınırlı izinlerle çalıştırın. Varsayılan olarak, uygulamaların çok az yapabilirsiniz. Örneğin, özel izinler olmadan bir uygulama olamaz kısa mesaj göndermek, telefon durumunu belirleme veya bile Internet erişim! Bu özelliklere erişmek için uygulamalar gibi ve yüklenen yaptıkları hangi izinlerin kendi uygulama bildirim dosyasında belirtmeniz gerekir; işletim sistemi bu izinleri okur, uygulama bu izinleri isteyen ve ardından devam etmek ya da yüklemeyi iptal olanak tanır. kullanıcıyı uyarır.
Uygulamalar iOS için örneği için olduğu gibi seçkin değil bu yana açık uygulama mağazası modeli nedeniyle Android dağıtım modelinde, önemli bir adım budur. Uygulama izinleri bir listesi için bkz: [bildirim izinleri](http://developer.android.com/reference/android/Manifest.permission.html) başvurusu makalesinde Android belgelerinde.

### <a name="windows-considerations"></a>Windows konuları

#### <a name="multitasking"></a>Çoklu

UWP çoklu da iki bölümden oluşur: sayfaları ve uygulamaları ve arka plan işlemleri için destek ömrü. Bir uygulamadaki her ekran etkin veya devre dışı (ile devre dışı durum işleme ya da "kaldırıldı" için özel kurallar) yapılan ile ilişkili olaylar sahip bir sayfa sınıfının bir örneği olur. 

İkinci bölümü, bile uygulama ön planda çalışmadığında görevler işlemek için arka plan aracısı sağlamaktır. 

#### <a name="device-capabilities"></a>Cihaz özellikleri

UWP donanım oldukça homojen olsa da yine isteğe bağlıdır ve bu nedenle özel kodlama sırasında dikkate gerektiren bileşenleri vardır. İsteğe bağlı donanım özellikleri, kamera, pusula ve jiroskop içerir. Ayrıca düşük ayrıcalık gerektiren bellek (256 MB), özel bir sınıf olduğunu veya geliştiriciler düşük bellek desteği çevirin.

#### <a name="security-considerations"></a>Güvenlik Değerlendirmeleri

UWP önemli güvenlik konuları hakkında daha fazla bilgi için başvurmak [güvenlik](https://docs.microsoft.com/windows/uwp/security/) belgeleri.

## <a name="summary"></a>Özet

Bu kılavuz, Mobil Geliştirme için ilgili olarak SDLC giriş verdi. Mobil uygulamaları oluşturmak için genel konular sunulan ve platforma özgü konuları tasarım, test ve dağıtım dahil olmak üzere çeşitli inceledi.

## <a name="related-links"></a>İlgili bağlantılar

- [Mobil Geliştirmeye Giriş](~/cross-platform/get-started/introduction-to-mobile-development.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, Android](http://developer.xamarin.com/get-started-droid/)
- [Uygulama Temelleri](~/cross-platform/app-fundamentals/index.md)
