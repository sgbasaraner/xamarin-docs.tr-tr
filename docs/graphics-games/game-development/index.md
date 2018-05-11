---
title: Xamarin ile oyun geliştirme giriş
description: Oyun geliştirme yapısını diğer tür uygulamaları geliştirme araçlarındaki önemli ölçüde farklı olabilir. Bu makalede Xamarin.iOS ve Xamarin.Android kullanılan teknoloji özellikleri oyun geliştirmeye giriş ' dir. Oyunlar nasıl yapılacağını, üst düzey bir tartışma ve teknolojilerinin kullanılabilir örnekleme Xamarin.iOS ve Xamarin.Android ile sağlar.
ms.prod: xamarin
ms.assetid: 0E3CDCD2-FBE4-49F5-A70E-8A7B937BAF1D
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 543d3e2d4e53d1b88213ac82689073a9fb925820
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-game-development-with-xamarin"></a>Xamarin ile oyun geliştirme giriş

_Oyun geliştirme yapısını diğer tür uygulamaları geliştirme araçlarındaki önemli ölçüde farklı olabilir. Bu makalede Xamarin.iOS ve Xamarin.Android kullanılan teknoloji özellikleri oyun geliştirmeye giriş ' dir. Oyunlar nasıl yapılacağını, üst düzey bir tartışma ve teknolojilerinin kullanılabilir örnekleme Xamarin.iOS ve Xamarin.Android ile sağlar._

Oyunlar geliştirme özellikle ne kadar kolay çalışmanızı mobil platformlarda yayımlama olabilir verilen çok heyecan verici olabilir. Bu makalede kavramları açıklar ve yüksek kaliteli AAA oyun ya da yalnızca için eğlenceli programı oluşturmak için amacınız olup olmadığını yardımcı olacak oyun geliştirme ilgili teknolojileri oyunlar, oluşturun.

Bu makalede aşağıdaki konuları içerir:

- **Oyun olmayan programlama kavramları ve oyun** – olan bazı kavramları ele alacağız oyun geliştirmeye benzersiz veya geliştirme diğer tür paylaşılan ancak burada önemine nedeniyle Vurgu hak.
- **Oyun Geliştirme ekibi** – Bu bölüm oyun geliştiricilerin ekibi çeşitli rollerde bakar.
- **Oyun bir fikir oluşturma** – Bu bölüm, yeni bir oyun fikir oluşturmanıza yardımcı olabilir – yeni bir oyun yapmadan ilk adımı.
- **Oyun Geliştirme Teknolojisi** – burada biz oyun geliştiricisi olarak verimliliğinizi artırmak platformlar arası teknolojilerden bazıları listelemek.


# <a name="game-vs-non-game-programming-concepts"></a>Oyun vs. Oyun olmayan programlama kavramları

Oyun Geliştirme taşıma programcıları genellikle yeni kavram ve geliştirme desenlerinden confronted. Bu bölümde bu kavramların bazıları üst düzey bir görünümünü sunar.


## <a name="the-game-loop"></a>Oyun döngüsü

Tipik bir oyun sabit taşıma veya kullanıcı etkileşimi ve otomatik oyun mantığı yanıt ekranında geçekleşmiş değişiklik gerektirir. Bu ne tipik olarak adlandırılır aracılığıyla elde edilen bir *oyun döngü*. Oyun döngü döngü 30 veya 60 gibi bir çok yüksek sıklığı çalışan deyimi (örneğin, bir süre-döngü) bazı türüdür *saniyedeki kare*.

Basit bir oyun Döngüsü diyagramı verilmiştir:

![](images/image1.png "Bu basit bir oyun Döngüsü diyagramı olur")

Aşağıda aşağıdakiler ele teknolojileri gerçek while döngü hemen soyut, ancak bu soyutlama rağmen çerçeve her güncelleştirmeleri kavramı mevcut olacaktır.

Kod performansı öncelik bile en basit oyunları alabilir. Örneğin: özellikle çerçeve başına birden çok kez çağrılırsa yürütmek için 10 milisaniye alan bir işlev oyun – performans üzerinde önemli bir etkisi olabilir. Oyununuzu 30 çerçeve adresindeki çalışıyorsa, ikinci sonra her çerçeve altında 33 milisaniye cinsinden yürütülmelidir anlamına gelir. Bunun aksine, bu tür bir işlevi bile, yalnızca bir oyun olmayan uygulamada düğmesini tıklatın yanıtta yürütülürse belirgin olmayabilir.

Çerçeve her gerçekleştirilen olabilir mantığının ortak türleri şunlardır:

- **Okuma giriş** – oyun dokunmatik ekranı, klavye, fare veya oyun denetleyicisi gibi giriş donanım denetleyerek kullanıcı oyun ile etkileşime varsa denetlemeniz gerekebilir.
- **Taşıma** – tek bir yerden hangi taşıma diğerine genellikle çok küçük taşınır nesneleri akıcı hareket atmosferini vermek için her çerçeve tutar.
- **Çakışma** – olup çeşitli nesneleri çakışan kesişen veya sık sık test birçok oyun gerektirir. Biz bu makalenin sonraki bölümünde daha fazla derinlemesine çakışma ele alacağız. Taşıma ve çakışma ayrılmış fizik benzetimi sistem tarafından işlenebilir.
- **Oyun özgü koşulları denetlemeyi** – player yeterli nokta veya ayrılan kez gerçekleştirilip gerçekleştirilmediğini kazanılan gibi belirli koşullar tarafından oyun durumunu kontrol.
- **AI davranışı** – bir ikincisinde ya da geçici bir oval toplantı rakibiniz sürücüleri hareketini patrolling gibi player tarafından denetlenmeyen nesneleri davranışını denetlemek için kullanılabilir çerçeve her mantığı.
- **İşleme** – çoğu oyunlar görüntülenenleri güncelleştirecektir üzerinde her çerçeve ekran. Bu yanıt etkisi oyun (örneğin, bir düzeye taşıma karakter) veya yalnızca olan değişiklikler yapılabilir visual Lehçe (örneğin, düşer halde kar veya animasyonlu simgeler) sağlamak için.

Birçok oyun olmayan uygulamaları gerçekleştirilen olaylarına yanıt olarak durum değişikliği eğilimindedir ancak yukarıda listelenen etkinliklere birçoğu tüm uygulama durumunu değiştirebilir göz önünde bulundurun.


## <a name="content-loading-and-unloading"></a>İçerik yükleme ve yüklemeyi kaldırma

El ile yükleme ve kaldırma (veya konumundaki) içerik geliştirme kullandığınız hangi teknolojisine bağlı olarak gerekli olabilir. El ile yükleme ve varlıkları yüklemeyi kaldırma birçok nedenden dolayı gerekli olabilir:

 - Varlıklar tek bir çerçeve uzunluğu göre yüklemek için uzun bir süre devam edebilir. Bazı varlıkların bile mid oyunlar yüklerse deneyimi ciddi bir şekilde kesintiye yüklemek için saniye sürebilir. Yükleme zamanında (ikinci birden fazla bir veya iki gibi) özellikle uzun olması durumunda animasyonlu görüntülemek isteyebilirsiniz ekran veya ilerleme çubuğunu yükleniyor.
 - Varlıklar çok oyunun Hedef platformlar tarafından sağlanan içine sığması için yüklenen etkin yönetim gerektiren RAM kullanabilir.
 - Oyunlar RAM sığmayacak daha fazla varlıklar görüntülemeniz gerekebilir. "Dünya açın" oyunlar genellikle hangi oynatıcıları aracılığıyla sorunsuz bir şekilde gidebilirsiniz – ile hiçbir yükleme ekranları olduğu büyük ortamlarda içerir. Bu durumda bir akış içeriği için özel sistem ve yönetme bellek kullanımı oluşturmanız gerekebilir.

Özel dosya biçimleri, yükleme zamanında özel yükleme kodu gerektiren işleme gerekebilir.


## <a name="math"></a>Matematik

Birçok oyun oyun olmayan uygulamaları'den daha gelişmiş matematik gerektirir. Doğal olarak, oyun kapsamına matematik düzeyine bağlıdır. Genel 3B oyun 2B'den daha fazla matematik gerektirir. Neyse ki her zaman basit oyunlar ile çalışmaya başlama ve ilerledikçe öğrenin. Oyun Geliştirme matematik öğrenmek için harika bir yolu olabilir!

Kartezyen kullanan düzlemi ile – aşinaysanız nesneleri – konumlandırmak için X ve Y koordinatları sonra oyun geliştirmeye başlamak için yeterli bildirin. Pozitif Y yukarıyı gösteren ile bir Kartezyen düzlemi gösterir:

![](images/image2.png "Bu pozitif Y yukarıyı gösteren ile bir Kartezyen düzlemi gösterir")

> [!IMPORTANT]
> Bazı motorları/API'leri, diğer sistemler pozitif Y yukarı olduğu bir koordinat sistemi kullanırken burada nesnenin Y değerinin artırılması, aşağı taşınır koordinat sistemi kullanır. Sistemler arasında taşıma varsa bunu göz önünde bulundurun.
Trigonometrik işlevler (örneğin, sinüsünü ve kosinüsünü) döndürme herhangi bir biçimde uygulayan 2B oyunlarda yaygın olarak kullanılır.



3B oyun yapan planlıyorsanız sonra büyük olasılıkla (hızlandırma uygulama için) bazı hesaplama yanı sıra Lineer Cebir (için dönüş ve hareket 3B uzaydaki) kavramlarını tanımanız gerekir.


## <a name="content-pipelines"></a>İçerik ardışık düzenleri

Terim *içerik ardışık düzen* bir oyunda kullanıldığında, son biçimi (.png görüntü dosyası gibi) yazılan zaman kendi biçiminden almak için bir dosyayı alır işlemine başvuruyor. İçerik türü üzerinde kullanılıyor yanı sıra teknolojisi içerik sunmak için kullanılan bitiş biçimi bağlıdır.

Bazı içerik ardışık düzen çok hızlı ve hiçbir el ile çaba gerektirir. Örneğin, çoğu oyun altyapılarını ve API'leri, işlenmemiş biçimi .png dosyası biçiminde yükleyebilirsiniz. Diğer taraftan, daha karmaşık biçimleri (örneğin, 3B modeller) farklı bir biçime yüklenen önce işlenmesi gereken ve bu işlem, varlığın boyutu ve karmaşıklığı bağlı olarak biraz zaman alabilir.


# <a name="game-development-teams"></a>Oyun Geliştirme ekiplerinin

Oyun geliştirme işleminde yer alan kişiler için yeni rolleri ve başlıkları tanıtır. Çoğu oyun geliştiricilerin uzmanlık alanlarından sayısı mevcut için tam bir oyun serbest bırakmak için gerekli niteliklere geniş kümesini karşılamak mümkün değildir. Bunun tam geliştirme – yalnızca bazı yaygın olanları alanlarının listesini olmadığını unutmayın.

- **Programcı** – bu makalede bu kategoriye ayrılır okuma çoğu kişi. Oyun geliştirmede Programcı rol oyun olmayan uygulama bir programcı rolünde benzer. Ekleme ve içerik görüntüleme ve – Elbette – hataları düzelttikten belirli bir projenin sistemleri bağlamında ortak görevler için geliştirme oyun akışını denetlemek için mantığı yazma sorumlulukları içerir.
- **2B sanatçı** – 2B Sanatçılar oluşturmaktan sorumlu *2B varlıklar*. Bunlar oyunun GUI, Parçacık, ortamları ve karakterler için resim dosyalarını içerir. Geliştirdiğiniz oyun 3B ise, 2B Sanatçılar ortamları ve karakterler için sorumlu olmayabilir. Ücretsiz bulabilirsiniz oyununuzu için resim [ http://opengameart.org/ ](http://opengameart.org/) .
- **3B tasarımcıları** – 3B tasarımcıları oluşturmaktan sorumlu *3B varlıklar*. Bu ortamlarda, karakterler ve özellik (Mobilya, araçları ve diğer inanimate nesneler) için 3B modelleri içerir. 3B tasarımcıları ve 3B animatörleri takım boyutuna bağlı olarak bazı ekipler birbirinden. Ücretsiz bulabilirsiniz oyununuzu için 3B bir resim [ http://opengameart.org/ ](http://opengameart.org/) .
- **Oyun Tasarımcısı** – oyun tasarımcıları nasıl Oyun oynanan tanımlamak için sorumlu. Bu ayarı gibi üst düzey kararları oyun, genel amacı, oyun ve nasıl bir oynatıcı oyun ilerler içerebilir. Oyun tasarımcıları da katsayısını taşıma veya düzeyi ups tanımlama ve düzey düzeni tasarlama eşleme girişi eylemlerine gibi çok ayrıntılı kararlarında söz konusu olabilir. Aklınızda terimi *Tasarımcısı* oyun Tasarımcısı veya bağlam bağlı olarak bir görsel tasarımcı başvurabilir.
- **Ses Tasarımcısı** – ses tasarımcıları oyunun ses varlıkları için sorumlu. Bazı ekipler küçük ekipleri için tüm ses sorumlu tek olabilir, ancak ses efekti ve composers, oluşturmaktan sorumlu olan kişiler birbirinden.


# <a name="creating-a-game-idea"></a>Oyun bir fikir oluşturma

Oyun tasarlama yapmak – tüm tek gereksinim kolay "eğlenceli bir şeyler yapın." olacak görünebilir Ne yazık ki, geliştirme başlatmak üzere bir fikir oluşturmak için zamanı geldiğinde geliştiricilerin çoğu kendilerini bir kaybıyla bulun.

Oyun tasarım uzmanlık değil kolayca açıklanmıştır ve resmi olarak artırmak için yöntem gerektirir veya programlama yapar ancak bu bölümde yolunu başlamanıza yardımcı olabilir.

Yeni geliştiriciler küçük başlamanız gerekir. Büyük, modern video oyun remake için buradaki eðilim sağlayacak şekilde zor olabilir, ancak daha küçük oyunlar daha iyi bir öğrenme ortamı olabilir ve daha hızlı ilerleme daha çekici bir deneyim sağlar.

Bir geliştirme ya da varolan bir oyun değişikliği olarak öğrenme yanı sıra ticari oyunlar amacıyla her ikisi de birçok oyun başlar. Fikirleri oluşturmak için bir diğer oyunlar esin için bakmak için yoludur. Örneğin, kişisel gibi bir oyun ve oyun oynama hakkında ne özelliklerini tanımlamak için try eğlenceli olun düşünebilirsiniz. Keşif, oyunun mekanizması veya Öykü İleri aşamalara mastery olabilir. Yeni fikirleri için arama yaparken de "geçmiş" oyunlar dikkate alınması gereken unutmayın.

Yeni fikirleri oluşturmak için başka bir teknik Bulmacanın oyunlar, strateji oyunları veya platformers gibi belirli bir tarzını ise. Geliştiriciler için tanıdık bir tarzını iyi bir başlangıç noktası sağlayabilir.

Bu tamamlanmış ürünün ticari letim gereksinimlerinin sınırlayabilir rağmen varolan oyunlar yeniden oluşturma ayrıca bir eğitim, deneyimidir. Oyun bile biri, doğru bir kopya oluşturma işlemi değerli bir eğitim deneyimi sağlar.


# <a name="game-development-technology"></a>Oyun geliştirme teknolojisi

Xamarin.Android ve Xamarin.iOS kullanan geliştiriciler, çeşitli teknolojileri bunlara oyun geliştirmeye yardımcı olmak için kullanılabilir sahip. Bu bölümde bazı en popüler platformlar arası çözümlerinin ele alınacaktır.


## <a name="cocossharp"></a>CocosSharp

CocosSharp Cocos 2B oyun altyapısı açık kaynak, platformlar arası sürümüdür. Engine, Android, iOS, Mac OS X, Windows Masaüstü, Windows RT ve Windows Phone için erişim sağlar.

2B oyun geliştirmeye yönelik basit Programcı API CocosSharp odaklanır. Mobil cihazlarda oyun artışa uygulanabilir bir teknoloji CocosSharp hobi ve ticari projeleri benzer olmasını 2B oyun geliştirme popülerliği reignite Yardım. (NuGet elde edilebilir) kaynak kodu veya .dll dosyaları olarak sağlanan ancak visual Düzenleyicisi sağlamaz; Bu nedenle, herhangi bir etkileşimi CocosSharp altyapı ile programlama bilgi gerektirir.

CocosSharp ile çalışmaya başlamak için kullanıma bizim [CocosSharp kılavuzları](~/graphics-games/cocossharp/index.md).

Oyun kızgın Ninjas CocosSharp ile oluşturulur ve birden çok platform için bir zaten çalışan oyun arıyorsanız iyi bir başlangıç noktası olabilir:

![](images/image3.png "Oyun kızgın Ninjas CocosSharp ile oluşturuldu")

İndirir ve daha fazla bilgi almak [AngryNinjas Github sayfası](https://github.com/xamarin/AngryNinjas).


## <a name="monogame"></a>MonoGame

MonoGame bir açık kaynak, Microsoft'un XNA API platform sürümü çapraz ' dir. MonoGame, iOS, Android, Mac OS X, Linux, Windows, Windows RT ve Windows Phone için oyunlar yapmak için kullanılabilir.

CocosSharp, teknik olmayan bir oyun altyapısı, ancak bunun yerine oyun geliştirme API MonoGame olur. Bu MonoGame çalışmak doğrudan oyun nesnelerini yönetme, el ile çizim nesneleri ve kameralar gibi ortak nesneleri uygulama gerektirdiği anlamına gelir ve *Sahne grafikleri* (üst alt hiyerarşi oyun nesneler arasında). Ayrım anlamanıza yardımcı olması için CocosSharp MonoGame üzerinde oluşturulmuştur göz önünde bulundurun. CocosSharp düzenleme ve oyun mantığı uygulamak için kod sağlarken MonoGame grafikler, işleme ve ses dosyaları gibi platforma özgü teknolojisi bazıları genelleştirir.

Programlama bilgisi MonoGame ile çalışma gerektiren şekilde MonoGame standart görsel geliştirme ortamı sağlamaz.

Oyunlar MonoGame kullanarak önemli örnekleri şunlardır:

FEZ:

![](images/image7.png "FEZ")

Savunma:

![](images/image8.jpg "Savunma")

MonoGame ile çalışmaya başlamak için üzerinden için head bizim [MonoGame kılavuzları](~/graphics-games/monogame/index.md).


## <a name="urhosharp"></a>UrhoSharp

UrhoSharp geometri, malzemeler, ışık ve Kameralar'ı kullanarak uygulamalarınızın animasyonlu 3B ve 2B planda oluşturmak için kullanılan bir platformlar arası üst düzey 3B ve 2B altyapısıdır.

![](images/urhosharp.gif "UrhoSharp animasyonlu 3B ve 2B planda oluşturmak için kullanılan bir platformlar arası üst düzey 3B ve 2B altyapısı.")

Kullanıma [UrhoSharp kılavuzları](~/graphics-games/urhosharp/index.md) başlamak için.

## <a name="additional-technology"></a>Ek teknolojisi

Yukarıdaki vurgulanmış teknolojileri yalnızca bir örnek teknolojilerden olur. Diğer önemli teknolojileri şunları içerir:

- **Hareketli grafik Seti** – Xamarin tüm yerel API işlevselliğini erişmenizi Apple'nın hareketli grafik Seti oyun framework için destek sağlar. Hareketli grafik paketi Apple tarafından oluşturulan teknolojisi olduğundan, iOS ekosistemi geri kalanı ile derin tümleştirme sağlar. Android kullanılamaz doğal olarak, hareketli grafik Seti platformlar arası değil. Hareketli grafik Seti'ni kullanma hakkında daha fazla bilgi için bu gönderisine bakın:  [http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/](http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/)
- **Sahne Seti** – Xamarin de destek sağlar 3B grafik iOS uygulamalarda uygulama basitleştirir Apple'nın Sahne Seti çerçevesi için. Sahne Seti Ayrıca, tümleştirme ve platforma özgü konuları hareketli grafik Seti için yukarıdaki nedenle Apple'nın sağladığı teknolojisidir. Sahne Seti hakkında daha fazla bilgi için bu gönderisine bakın: [http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/](http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/)
- **OpenTK –** (hangi açık araç seti için anlamına gelir) OpenTK iOS, Apple ve Mac için alt düzey OpenGL erişim sağlar donanım. Ana sayfanın OpenTK hakkında daha fazla bilgi için bkz:  [http://www.opentk.com/](http://www.opentk.com/)


# <a name="summary"></a>Özet

Bu makalede, oyun geliştirme temel kavramları kapsar ve ilk oyununuzu yapmadan kullanmaya başlama hakkında bilgi sağlar. Bu makalede bitirdikten sonra sonraki teknoloji seçin ve uygun bölümlerde yukarıdaki bağlı öğreticileri bizim dizi çalışmaya başlayın adımlardır.

## <a name="related-links"></a>İlgili bağlantılar

- [CocosSharp kılavuzları](~/graphics-games/cocossharp/index.md)
- [MonoGame kılavuzları](~/graphics-games/monogame/index.md)
- [UrhoSharp kılavuzları](~/graphics-games/urhosharp/index.md)
