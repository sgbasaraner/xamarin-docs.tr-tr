#<a name="---"></a>---
Başlık: "Bölümü 6 – test etme ve uygulama mağazası onayları" ms.prod: xamarin ms.assetid: 46E0578A-7EB9-C105-ABB0-A043E501F36B ms.technology: platformlar arası xamarin Yazar: asb3993 ms.author: amburns ms.date: 23/03/2017
---

# <a name="part-6---testing-and-app-store-approvals"></a>Bölüm 6 - test etme ve uygulama mağazası onayları


## <a name="testing"></a>Sınama

Birçok uygulama (bazı depoları üzerinde bile Android uygulamaları) bir onay işlemi yayımlanmadan önce geçmesi gerekir; sınama emin olmak için kritik olacak şekilde uygulamanızı Pazar ulaştığında (let alone müşterilerinizle başarılı). Test birçok, beta çok çeşitli donanım arasında sınama yönetmek için test Geliştirici düzeyi biriminden biçimde olabilir.


### <a name="test-on-all-platforms"></a>Tüm platformlarda test

.NET Windows telefon, tablet ve Masaüstü aygıtları, yanı anında oluşturulacak dinamik kod engellemenize iOS sınırlamalar destekleyen arasında küçük farklar vardır. Da, geliştirme veya düzenleme zamanlama ve güncelleştirmeleri uygulamanızı Proje sonunda modeli parçası olarak birden çok platformdaki kod test planlayın.

Bu her zaman işletim sistemi ve ayrıca farklı cihaz yetenekleri/yapılandırmaları birden fazla sürümünü sınamak üzere simulator/öykünücüsü kullanmak için iyi bir uygulamadır.

Ayrıca, mümkün olduğu gibi birçok farklı fiziksel donanım aygıtlarında test etmeniz gerekir.


#### <a name="devices-in-cloud"></a>Bulutta cihazları

Mobil telefon ve tablet ekosistemi test cihazları kullanılabilir gitgide artan sayısına olanaksız hale her zaman artıyor. Bu sorunu çözmek için hizmetlerin sayısı; böylece uygulamaların yüklü ve doğrudan içinde çok sayıda donanım gereksinimleri yatırım yapmalarına gerek kalmadan test birçok farklı cihaz uzaktan denetleme olanağı sunar.

[Uygulama Merkezi Test](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) iOS ve Android uygulamaları farklı cihaz yüzlerce test etmek için kolay bir yol sunar.


### <a name="test-management"></a>Test Yönetimi

Kuruluşunuz ya da bir beta programı ile dış kullanıcıları yönetme içinde uygulamaları test edilirken iki zorluklar mevcuttur:

-   **Dağıtım** – (özellikle iOS cihazları için) sağlama işlemini yönetme ve güncelleştirilmiş sürümlerini yazılım sınayıcıları için alma.
-   **Geri bildirim** – uygulama kullanımı hakkında bilgi ve ayrıntılı bilgileri oluşabilecek tüm hatalar toplama.


Uygulamanızın toplama ve kullanım ve hata raporu içinde yerleşik altyapısı sağlayan ve ayrıca Sınayıcılar ve cihazlarını kaydolma yardımcı olmak için sağlama işlemini hızlandırma bu sorunları gidermek için Hizmetleri Yardım dizi vardır .

[Visual Studio Uygulama Merkezi](/appcenter/) test sürüm dağıtım, kilitlenme bildirimini ve Gelişmiş uygulama kullanım bilgileri sağlayarak bu sorunları için bir çözüm sunar.



### <a name="test-automation"></a>Test Otomasyonu

Xamarin [UITest](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest) otomatik kullanıcı arabirimi yerel olarak çalıştırın ya da yüklenen test komut dosyaları oluşturmak için kullanılan [Center uygulamayı Test](https://docs.microsoft.com/appcenter/test-cloud/).




## <a name="unit-testing"></a>Birim testi



#### <a name="touchunit"></a>Touch.Unit

Xamarin.iOS, testleri yazma JUnit/NUnit stili takip eden Touch.Unit adlı bir birim testi çerçevesi içerir.

Başvurmak bizim [birim testi Xamarin.iOS](~/ios/deploy-test/touch.unit.md) belgelerine testleri yazma ve Touch.Unit çalıştırma hakkında bilgi.



#### <a name="andrunit"></a>Andr.Unit

Touch.Unit Andr.Unit adlı Android için bir açık kaynak eşdeğeri yoktur. Buradan indirebilirsiniz [github](https://github.com/spouliot/Andr.Unit) ve aracı hakkında okuyun [ @spouliotın blog](http://spouliot.wordpress.com/2011/10/30/andr-unit-joins-the-family/).




## <a name="app-store-approvals"></a>Uygulama mağazası onayları

Apple ve Microsoft çalışması kendi platformlarda yalnızca deposu: Market ve uygulama mağazası sırasıyla. Hem cihazlarını kilitleme ve uygulamaların indirilebilir kalitesini denetlemek için ayrıntılı uygulama gözden geçirme işlemini uygulayın. Depolama Seçenekleri geniş çapta kullanılabilir olduğunu ve hiçbir gözden geçirme işlemine, Android ve donanıma özgü çalışmalarını dağıtım daha sınırlı Samsung uygulamalar gibi Amazon'ın Appstore sahip Google Play arasında değişen bir dizi vardır Android'ın açık yapısı anlamına gelir ve bir onay işlemi uygulayın.

Bir uygulamayı gözden geçirilmesi için bekleyen çok gerilimli olabilir - uygulamalar onay için bir "hedeflenen" başlatma tarihi önce hata için çok az kenar boşluğu ile gönderilen iş pressures genellikle anlamına gelir. En fazla iki hafta sürebilir ve mutlaka saydam değil. işlem: yoktur sınırlı geri bildirim, uygulamanızın ilerlemeyi son reddedildi veya onaylanmış kadar. Reddetme fırsatı, pazarlama pencerenin eksik, özellikle birden çok kez olur ve özgün başlatma tarih hafta geçirmek ve uygulama son onaylandığında anlamına gelebilir.



### <a name="be-prepared"></a>Hazırlıklı olun

Öneri ilk parçasına: uygulamanızın başlatma iyi önceden planlayın ve kesintileri olası reddetme ve yeniden gönderme için yapın. Her mağaza uygulamanızı göndermeden önce bir hesap oluşturma - olabildiğince erken bunu gerektirir.
Google Play'nın kaydolma uygulamalarınızı boş olup olmadığını, yalnızca birkaç dakika sürer olsa da, bunları satış ve bankacılık sağlayın ve bilgi vergi gerekiyorsa, işlem çok daha karmaşık alır. Apple ve Microsoft'un işlemleri hem Google daha çok daha karmaşık, onu bir hafta ele geçirebilir veya onaylanmış hesabınızı almak için daha fazla bu nedenle bu kez başlatma planlarınızı faktörü markalarıdır.

Hesabınızı onaylandıktan sonra bir uygulama göndermeye hazırsınız. Uygulamaları göndermek için gerçek işlemi aşağıdaki belgelerinde ele alınmıştır:

-   [Apple iOS App Store'da yayımlama](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
-   [Bir uygulamayı Google Play için hazırlama](~/android/deploy-test/publishing/publishing-to-google-play/index.md)
-  Windows geliştiricileri ziyaret [Windows Geliştirme Merkezi](https://developer.microsoft.com/en-us/windows/windows-apps) uygulamalarını gönderme hakkında okunamıyor.


Bu bölüm geri kalanı uygulamanızı herhangi duraklamalarla onaylandığından emin olmak için dikkate yapması gereken şeyleri ele alınmaktadır.



### <a name="quality"></a>Kalite

Belirgin sesleri, ancak uygulamalar genellikle reddedilen belirli bir kalite düzeyini karşılayamadığı için: tüm bu neden seçkin depolarınız bir onay işlemi ilk başta nedeni!

Kilitlenme reddetme için yaygın bir nedenidir. Uygulama kilitlenme yapmak çok kolay ise reddedilmesi garanti. Çoğu geliştirici, kilitlenme, ancak genellikle yaparlar Beklenti ile uygulamalarını gönderme yok. Göndermeden, yalnızca üzerinde her şeyi çalıştığından emin yetişememe odaklanan aynı zamanda, ağ sorunlarını ve bellek veya depolama alanı gibi kaynak kısıtlamaları gibi yaygın mobil hata senaryoları işlemek önce uygulamanız baştan sona test edin. Test etmek için simulator ve fiziksel cihazları kullanın - kodu bir benzeticisinde ne kadar iyi çalışır bağımsız olarak, yalnızca bir aygıt bir uygulamanın gerçek performans gösteren. Bul ve beta Test edenlere ekibi listeleme yapabilecekleriniz - üçüncü taraf hizmetleri beta dağılımı ve geri bildirim yönetmeye yardımcı olabilen varsa gibi birçok farklı cihaz kullanın.

Tüm mobil işletim sistemleri yeterince hızlı başlamıyorsa uygulamanın KILL. İzin verilen süre değişir, ancak genel olarak uygulamaları birkaç saniye içinde yanıt ve arka plan görevlerinin daha uzun sürecektir herhangi bir iş yapabilir hedefleyin. Yük uzun sürmesine uygulamaları ya da olan normal kullanımda olmayan esnek yeterince reddedilir. Her zaman bir şey arka planda gerçekleştiği ya da uygulama çökme ve bir kez daha, reddedilen için görünür kullanıcı geri bildirim sağlayın.


### <a name="check-your-edge-cases"></a>Edge çalışmalarınızı denetleyin

Geliştiriciler için ortak bir yakalama adresi kenar örnekleri için başarısız olan özellikle de kendi benzeticisi veya aygıtın düzgün bir şekilde test etmek için yeniden yapılandırma gerektirir. Her müşteri "İzin ver" Geliştirici isteği bir kez kabul ettikten sonra bunların hiçbir zaman yeniden istenir çünkü konumlarına erişmek için uygulamanızı işaretleneceğini unuttunuz kolay olabilir. İzinleri ve ağ kullanımı özellikle üzerinde küçük bir gözetim göre şu alanlardaki içinde reddetme sonuçlanabilir anlamına gelir onay işlemi sırasında focussed.

Aşağıdaki listede kaçırmış olabileceği kenar-durumlarda denetleme iyi bir başlangıç noktasıdır:

-   **Müşteriler '' erişimi engellemek için Hizmetleri** – iOS, özellikle de gibi kullanıcı uygulamanızı izin verirse coğrafi konum bilgileri yalnızca sağlanan verilere erişim. Uygulama sınayıcılar sık ilk durumuna uygulamada yeniden yüklemeli ve uygulamanın uygun şekilde davranır emin olmak için hiçbir izin isteklerine izin verme. Üzerinde izin geçiş ve müşteriler fikrini değiştikçe doğru davranış doğrulamak için kapalı.
-   **Müşterilerdir her yerde** – bir uygulamayı yalnızca Şehir ya da nerede geliştirilmiş ülke kullanılacağını varsaymayın! GPS koordinatları, tarih ve saat değerleri ve para birimlerinin ile çalışan kod tüm müşteri'nin konumu ve yerel ayarları tarafından etkilenebilir. Tüm platformlar teklif farklı konumlarda ve yerel ayar - belirtmenize olanak tanıyan bir simulator, diğer hemispheres ve tarihleri ve para birimlerinin farklı biçim kültürler konumları sınamak için kullanın. Enlem ve boylam değerlerini olumlu veya olumsuz olabilir, Ondalık ayırıcının bir dönem veya virgül olabilir ve tarihleri çok biçimlendirilebilir - yöntemleri ile ilgili!
-   **Yavaş ağ bağlantıları** – uygulama geliştiriciler genellikle iş dünyasında' hızlı, her zaman açıktır gerçek dünya durumda olmayan ağ bağlantısı çalışma bir ideal'. Yavaş ağ bağlantısı (örneğin, kötü 3 G bağlantı) ile ve aynı zamanda ağ erişimi olmayan test buggy uygulama teslim edilmiyor sağlamak için önemlidir. Onay işlemi her zaman uçak modu aygıt ile bir test içerir, bu nedenle, kendiniz için test ettiğiniz emin olun.
-   **Donanım değişir** – desteklemeyi planladığınız en eski, en yavaş donanım test unutmayın. Uygulamanızı etkileyebilecek iki yön vardır: bir eski aygıt ve Kamera Mikrofon, GPS jiroskop veya diğer isteğe bağlı bir bileşen gibi donanım özellikleri için destek kullanılamayabilir performans. Uygulamaları kurtulabilirsiniz düzgün biçimde (ve çökme değil) bir bileşen olduğunda kullanılamaz.




### <a name="guidelines-are-more-than-just-a-guide"></a>Daha fazlasını 'kılavuz' yönergelerdir

Apple kendi platform anahtar gücü birini tutarlılık (ve kullanılabilirlik algılanan artış) gibi kendi İnsan Arabirimi yönergelerine bağlılığı hakkında katı olması için ünlü ' dir. Microsoft Metro stili UI uygulama Windows uygulamaları ile benzer bir yaklaşım sürdü. Onay işlemi iki platform için uygulamanızı kendi düzenlendi.%nÇözüm ilgili tasarımı felsefesi değerlendirilen içerecektir.

Bu, kullanıcı arabirimi yenilik değil veya desteklenen teşvik, ancak, "yalnızca yapmamalısınız" belirli konular vardır veya uygulamanızı reddedilecek olduğunu söylemek için değil.

İos'ta, bu yerleşik simgeler kötüye veya diğer iyi kurulan metaphors tutarlı olmayan bir şekilde kullanarak içerir; Örneğin yeni içerik oluşturma dışında her şey için 'Oluştur' simgesini kullanarak.

Windows geliştiricileri benzer şekilde dikkatli olmalıdır; sık karşılaşılan hata donanım geri düğmesini Microsoft'un yönergelerine göre düzgün desteklemek başarısız oluyor.

Okuyup her platform için tasarım yönergeleri izleyin, tasarımcıları teşvik edin.



### <a name="implementing-platform-specific-features"></a>Platforma özgü özelliklerini uygulama

Özellikle İos'ta platforma özgü Hizmetleri, uygulama için geldiğinde biraz daha sıkı noktalardır. Apple tarafından otomatik tarafından reddedilmesini önlemek için aşağıdaki iOS özelliklerle izlemek için bazı kurallar vardır:

-   **Uygulama içi satın alımlar** – uygulamaları oyun para birimi, uygulama özellikleri, dergi abonelikleri ve çok daha da dahil olmak üzere dijital ürünleri için dış ödeme mekanizmaları değil uygulaması gerekiyor. iOS uygulamaları, bu tür bir işlevsellik için Apple'nın iTunes tabanlı hizmeti kullanmanız gerekir. Loophole - Kindle okuyucu ve bazı abonelik tabanlı uygulamalar, başka bir yerde "aracılığıyla uygulamayı daha sonra erişebileceğiniz bir hesabı" iliştirilmiş içerik satın almanıza olanak gibi uygulamalar yoktur, ancak bu durumda uygulama içermemelidir bağlantılar veya başvurular app satın alma işlemi (veya bir kez daha, onu reddedilmesi).
-   **iCloud yedeklemesini** – iCloud geliştirilirken ile Apple'nın gözden geçirenler uygulamaları depolama (Müşteri'nin Uzak yedekleme deneyimini eğlenceli olduğundan emin olmak için) kullanma ile ilgili çok daha katı. Atık yedekleme yapabilir depolama alanı reddedilen, uygulamaları kullanın Önbellek klasörü uygun şekilde ve diğer depolama ile ilgili yönergeleri izleyin Apple kullanıcının.
-   **Newsstand** – gazete ve dergi uygulamalar uygulamaları onaylanması için karşıdan yükleme en az bir otomatik yenileme abonelik ve Destek arka plan uygulamalıdır ancak Apple'nın Newsstand için harika bir sığdırma şunlardır.
-   **Eşlemeleri** – mobil eşlenir yer paylaşımları ve diğer özellikleri eklemek için ancak harita saklamasını 'KREDİLERİ' bilgileri (örneğin, iOS5 Google logo) Bunun yapılması reddetme neden olacak şekilde dikkatli olmaması giderek daha çok yaygındır.




### <a name="manage-your-metadata"></a>Meta verilerinizin yönetme

Reddedilen bir uygulamada sonuçlanabilir belirgin teknik sorunların yanı sıra, özellikle meta verileri (Açıklama, anahtar sözcükleri ve pazarlama görüntüler) çevresinde reddetme aldığınız neden olabilir, Gönderiminizi bazı daha hafif konuları vardır App Store veya Market içinde görüntülemek için uygulama ile gönderin.

-   **Görüntülerin** – uygulama simgeleri platformun yönergeleri izleyin ve resimler depolar. Ticari marka olarak kaydettirilmiş görüntüleri kullanmayın, uygulamaları simgelerine iPhone çizimi öne çıkan için reddedilen gördük!
-   **Ticari** – kendi dışında tüm ticari markalar kullanmaktan kaçının. Uygulamaları, ticari uygulama açıklaması veya hatta Apple App Store anahtar sözcükleri söz engellendi.
-   **Açıklama** – kullanmayın 'beta' sözcük veya uygulama asal saat için hazır değil herhangi bir şekilde gösterir. (Uygulamanızın platformlar arası olsa bile) diğer mobil platformları Bahsediyor yok. En önemlisi, tam olarak ne mevcut söylenebilir uygulama olmadığından emin olun. Tanımınızda özelliklerinin bir demet listesi, daha iyi bu özelliklerin her biri kullanmayı belirgin veya "uygulamanın açıklamasında belirtilen özellik uygulanmadı" reddetme elde edersiniz.


Uygulama meta verileri içine kadar çaba geliştirme ve test etme gibi içine yerleştirin. Sağ almak üzere zaman ayırdığınız faydalı olması için ikincil infringements meta veriler için uygulamaları reddetti.



### <a name="app-stores-not-for-everyone"></a>Uygulama mağazaları: Herkes için

Birincil odağı her platformda depoları, tüketici dağıtımı - mümkün olduğu kadar müşterilere ulaşmak için özelliği ' dir. Ancak tüm uygulamaları hedefler tüketicileri yoktur bir hızla çalışanlar, sağlayıcılardan ya da müşterilere sınırlı dağıtım gerektiren şirket içi ve extranet benzeri uygulamalar temeli büyüyor. Bu uygulamaları "satışa" değil ve geliştirici denetimleri dağıtım kapalı bir kullanıcı grubu için bu yana onay gerek yoktur.
Bu dağıtım türü için destek platforma göre değişir.

Android en fazla esnekliği bu bağlamda sunar: uygulamaları (cihazın yapılandırma sağlayan sürece) doğrudan bir URL veya e-posta eki yüklenebilir. Bu, oluşturmak ve şirket içi Kurumsal uygulamaları dağıtmak veya belirli müşterilere veya Üreticiler uygulamaları yayımlamak için Önemsiz olduğu anlamına gelir.

Apple iOS App Store onay işlemi atlar ve çalışanların şirket içi uygulamaları dağıtmak şirketlerin sağlayan Geliştirici Kurumsal programı kayıtlı geliştiriciler bir şirket içi dağıtım seçeneği sağlar.
Ne yazık ki bu lisans extranet benzeri uygulama dağıtım için gereken diğer kapalı gruplarına müşteri veya tedarikçi adresi değil. [Enterprise (ve geçici) dağıtımı](~/ios/deploy-test/app-distribution/ipa-support.md)



### <a name="app-store-summary"></a>Uygulama mağazası özeti

Gözden geçirme işlemi göz korkutucu olabilir, ancak geliştirme yaşam döngüsü rest gibi bazı planlama ve dikkat edilerek ile başarılı sağlamaya yardımcı olur. Tümü için birkaç basit adımda gelir: Okuma ve platforma özgü özellikleri uyguluyorsanız kurallarına, sınamanız (sonra daha fazla bilgi bazı test etmek için) kalmayı ve son olarak, uygulama meta verileri emin olun kullanıcı arabirimi yönergelerine anlama göndermeden önce doğrudur.

Google Play'de yayımlama geliştiriciler için öneriler bir son sözcüğü: onay işlemi olmaması, işinizi - kolaylaştırır gibi görünebilir, ancak müşterilerinizin daha da fazla inceleme takımı yoğun olacaktır. Uygulamanızı reddedilen olsa, aksi takdirde, reddetme yapılması müşterilerinizin olarak aşağıdaki yönergeleri izleyin.

