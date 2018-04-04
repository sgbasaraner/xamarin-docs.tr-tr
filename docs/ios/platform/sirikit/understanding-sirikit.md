---
title: SiriKit kavramlarını anlama
description: Bu makalede, bir Xamarin.iOS uygulaması SiriKit ile çalışmak için gerekli olacak temel kavramları kapsar.
ms.prod: xamarin
ms.assetid: 99EC5C1E-484F-4371-8555-58C9F60DE37F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7140747307c15b51fb330a41b496079dc680e5fa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="understanding-sirikit-concepts"></a>SiriKit kavramlarını anlama

_Bu makalede, bir Xamarin.iOS uygulaması SiriKit ile çalışmak için gerekli olacak temel kavramları kapsar._


Yeni iOS 10, SiriKit Siri ve haritalar uygulama iOS cihazında kullanarak kullanıcı tarafından erişilebilir hizmetleri sağlamak bir Xamarin.iOS uygulaması sağlar. Bu işlevi kullanarak yeni bir veya daha fazla uygulama uzantısı'nda sağlanan **hedefleri** ve **hedefleri UI** çerçeveleri.

SiriKit sağlayan uygulama uzantıları ve yeni kullanarak iOS cihazında Siri ve haritalar uygulama kullanarak kullanıcı tarafından erişilebilir hizmetleri sağlamak bir iOS uygulaması **hedefleri** ve **hedefleri UI** çerçeveleri.

Siri çalışır kavramıyla **etki alanları**, grupları bilmeniz ilgili görevleri için Eylemler. Uygulama ile Siri sahip her etkileşim, bilinen bir hizmet etki alanlarından biri aşağıdaki gibi olmalıdır:

- Ses veya video çağırma.
- Bir kılma kayıt.
- Egzersizleriniz yönetme.
- İleti.
- Fotoğraf aranıyor.
- Gönderme veya ödemeler alma.

Kullanıcı, uygulama uzantının hizmetlerinden birini içeren Siri isteği yaptığında, SiriKit uzantısı gönderir bir **hedefi** kullanıcının isteği destekleyici verilerin tanımlayan nesne. Uygulama Uzantısı sonra uygun oluşturur **yanıt** için nesne verilen **hedefi**, uzantısı isteği nasıl işleyebilir ayrıntılı.

## <a name="the-intents-and-intents-ui-extensions"></a>Hedefleri ve hedefleri UI uzantıları

Siri ve haritalar app uygulama uzantıları iki farklı türde uygulamanın hizmetlerle etkileşimde:

- **Hedefleri uzantısı** -Siri sağlar ve Maps uygulama ile içerik ve desteklenen tüm hedefleri yerine getirmek için gereken görevleri gerçekleştirir.
- **Hedefleri UI uzantısı** -Siri veya eşlemeleri içinde uygulamanın içeriği için görüntülenecek özel bir kullanıcı Arabirimi sağlar.

Uygulama SiriKit desteklemek için hedefleri uzantı sağlamanız gerekir ve bunu Siri ve haritalar kullanıcıya sunabilir bilgi sağlamak için ve hedefleri işlemek için sorumludur.

Siri genellikle tüm kullanıcı etkileşimini işler ve bilgi desteklenen alanlarındaki sunmak için bir standart, yerleşik UI sahip olduğundan hedefleri UI uzantısı oluşturma isteğe bağlıdır. Hedefleri UI uzantı sağlayarak uygulama kullanabilirsiniz **hedefi UI** bir zengin, özel kullanıcı arabirimi uygulamayı markalama özelliklerine sahip sunmak için framework ve ek bilgiler.

## <a name="siri-and-the-maps-app-role"></a>Siri ve haritalar uygulama rolü

Kullanıcının konuşma işlenir ve bu istekleri hedefi uzantıları işleyebilir tıklatılabilir hedefleri kapatan Siri tarafından anlamsal olarak analiz dil isteklerdir.

Eşlemeleri kullanıcının eylemlerine yanıt olarak harita arabiriminde bilgileri görüntülemek için uygulamanın amacı uzantıları kullanır. Restoran isteme veya alma gibi uygulamanın Restoran inceler.

Siri ve eşlemeleri tüm kullanıcı etkileşimleri yönetmek ve standart sistem arabirimini kullanarak sonuçları görüntüleyin. Öncelikle uygulama uzantıları rolü görüntülenen veri sağlamaktır. İsteğe bağlı olarak, uygulama hedefleri UI uzantı sağlar ve varsayılan sistem arabirimi geliştirmek için özel bir kullanıcı Arabirimi sunar.

## <a name="interacting-with-siri-via-sirikit"></a>Siri SiriKit aracılığıyla ile etkileşim kurma

Bu bölümde, nasıl SiriKit Siri kullanarak uygulama ile etkileşim kurmalarına izin verir genel bir bakış sunacaktır. Bu örnek amacıyla, biz sahte MonkeyChat uygulamasını kullanarak:

[![](understanding-sirikit-images/monkeychat01.png "MonkeyChat simgesi")](understanding-sirikit-images/monkeychat01.png#lightbox)

Kullanıcının arkadaş kişi kendi rehberini MonkeyChat tutar, her bir ekran adı (ör, örneğin Bobo) ile ilişkili ve metin sohbetleri ekran adına göre her arkadaşınıza gönderin olanak tanır.

Farklı kişilerin aynı istekte çok farklı biçimlerde yapabileceğiniz bu yana kullanıcı uygulaması ile etkileşim başlatmak birçok yolu vardır.

Örneğin, kullanıcının kendi arkadaş Bobo bir ileti göndermek istiyorsanız aşağıdaki konuşma Siri ile olabilir:

_Kullanıcı: Hey Siri, MonkeyChat iletisi gönderin._<br />
_Siri: Kime?_<br />
_Kullanıcı: Bobo._<br />
_Siri: Ne için Bobo söylemek istiyorsunuz?_<br />
_Kullanıcı: Lütfen daha fazla Muzlar gönderin._<br />

Başka bir kişiye farklı bir konuşma ile aynı istekte yapabilir:

_Kullanıcı: Bobo MonkeyChat üzerinde bir ileti gönderin._<br />
_Siri: Ne için Bobo söylemek istiyorsunuz?_<br />
_Kullanıcı: Lütfen daha fazla muzlar gönderin._<br />

' İ tıklatın ve başka bir kullanıcı daha kısa bir istekte bulunun:

_Kullanıcı: MonkeyChat Bobo Lütfen daha fazla muzlar gönderin._<br />
_Siri: Tamam, iletiyi göndermeyi Lütfen daha fazla muzlar Monkeychat üzerinde Bobo gönderin._<br />

Veya farklı bir dilde bile aynı istekte olun:

_Kullanıcı: MonkeyChat Bobo s'il vous plaît bananes envoyer artı de._<br />
_Siri: OUI, envoi ileti s'il vous plaît envoyer artı de bananes à Bobo sur Monkeychat._<br />

Henüz başka bir kullanıcı kendi konuşmada çok ayrıntılı olabilir:

_Kullanıcı: Hey Siri,, Lütfen bana bir ayrıcalık yapın ve göndermek için MonkeyChat uygulama başlatma iletisi metinle, lütfen daha fazla muzlar gönderin._<br />
_Siri: Kime?_<br />
_Kullanıcı: My en iyi pal Bobo._<br />

Ayrıca, bir istek, bazı isteği nasıl yapıldı dayalı Siri yanıt vermeyebilir birçok yolu vardır:

- **Giriş düğmesini basılı tutarak** -Siri daha fazla visual yanıtları sınırlı sözlü görüş sağlayacaktır.
- **"Hey Siri" tarafından** -Siri daha sözlü ve daha az visual yanıtları sağlayın.

Siri, ayrıca kullanıcı erişilebilirlik gereksinimlerini karşılamak ve etkileşim ve bu ihtiyaçlara göre yanıt için ayarlanmıştır.

Nasıl bir istek yapıldığında veya Siri isteği nasıl yanıt vereceğini olsun kullanıcıyla konuşma Siri işler ve (aracılığıyla uzantılarının) uygulamayı işlevsellik sağlar.

Kullanıcı bir Siri sözlü istekte bulunduğunda bu Siri izleyeceği adımlar şunlardır:

[![](understanding-sirikit-images/monkeychat02.png "Siri izlediği adımları")](understanding-sirikit-images/monkeychat02.png#lightbox)

1. İlk olarak, kullanıcının ses Siri geçen **konuşma** ve metne dönüştürür.
2. Ardından, metin dönüştürülen bir **hedefi**, bir kullanıcının isteği gösterimini yapılandırılmış.
3. Amacına bağlı olarak, Siri sürer **eylem** kullanıcının isteği gerçekleştirmek için.
4. Son olarak, Siri sunacaktır **yanıtları** (visual ve sözlü) gerçekleştirilecek eylem üzerinde dayanarak kullanıcıya.

Uygulama Siri ile kullanıcının konuşmadaki yer alabilir üç ana yolu vardır:

[![](understanding-sirikit-images/monkeychat03.png "Siri ile kullanıcılar konuşmadaki uygulama yer alabilir üç ana yolu")](understanding-sirikit-images/monkeychat03.png#lightbox)

1. **Sözlük** -uygulama Siri ile etkileşim için bilmeniz gereken sözcükleri nasıl söyler budur.
2. **Uygulama mantığı** - bunlar eylemleri ve uygulama sürer yanıtları bağlı olarak amaçlar.
3. **Kullanıcı arabirimi** -bu uygulama yanıtları verebilirsiniz isteğe bağlı, özel kullanıcı arabirimidir.

### <a name="example"></a>Örnek

Yukarıdaki bilgilerin verildiğinde, aşağıdaki konuşma MonkeyChat uygulama ile nasıl etkileşime inceleyin:

_Kullanıcı: Hey Siri, MonkeyChat üzerinde Bobo iletisi gönder._<br />
_Siri: Ne için Bobo söylemek istiyorsunuz?_<br />
_Kullanıcı: Lütfen daha fazla muzlar gönderin._<br />

Uygulama konuşmada alan ilk Siri kullanıcının konuşma anlamanıza yardımcı olmak için rolüdür:

[![](understanding-sirikit-images/monkeychat04.png "Kullanıcıların konuşma anlamak Siri yardımcı olma")](understanding-sirikit-images/monkeychat04.png#lightbox)

Siri adı "Bobo" veritabanına sahip değil, ancak uygulama yapar ve bu bilgileri, sözlük aracılığıyla Siri paylaşılan. Uygulama Siri için bunları belirtilen beri Bobo bir alıcı olduğunu algılar Siri da yardımcı olur bir *kişi*.

Siri daha fazla ileti içeriği gerektirip gerektirmediğini görmek için uygulama uzantısı ile hızlı bir şekilde kontrol eder için yalnızca bir alıcı daha ileti göndermek için gerekli olduğunu bilir. MonkeyChat mu beri Siri soru kullanıcıyla yanıt verir: *"Ne sizin için Bobo söyleyin istiyor musunuz?"*

Yukarıdaki örnekte, kullanıcı yanıtladı, *"Lütfen daha fazla muzlar Gönder"*, hangi Siri yapılandırılmış paketini **hedefi**:

[![](understanding-sirikit-images/monkeychat05.png "Siri kullanıcının yanıt yapılandırılmış bir hedefi pakete ekler")](understanding-sirikit-images/monkeychat05.png#lightbox)

Yapılandırılmış amacıyla aşağıdaki bilgileri içerir:

- **Etki alanı:** iletileri
- **Amacı:** sendMessage
- **Alıcı:** Bobo
- **İçeriği:** Lütfen daha fazla muzlar gönderin

Her etki alanı kümesi olarak sahip bilmeniz *Eylemler* , bunların içinde yapılabilmesi ve etki alanı ve eylemin göre çok fazla parametre hedefi dahil sıfır uygulamaya gönderilir.

Amaç sonra uygulama uzantısı işleme gönderilir. Hedefi işleme uygulama sonucunda oluşturacak bir **IntentResponse** amacıyla toplanmış ve amacıyla uygulama ne yaptığını açıklayan parametreleri içerir.

Her IntentResponse de içerecek bir **yanıt kodu** uygulama veya isteği tamamlamaya edemediğini Siri söyleyen. Bazı etki alanlarına da gönderilebilir belirli hata yanıt kodları sahip.

Son olarak, IntentResponse içerecek bir `NSUserActivity` (ister el kapalı desteklemek için Kullanılanlar). `NSUserActivity` Siri ortam bırakın ve onu tamamlamak için uygulamaya girmek için yanıt gerektirmesi durumunda uygulamayı başlatmak için kullanılır. 

Siri otomatik olarak uygun bir oluşturacaksınız `NSUserActivity` kullanıcı Siri ortamda nerede bıraktığınız alma ve uygulamayı başlatmak için. Ancak, uygulama kendi sağlayabilir `NSUserActivity` ile gerekliyse, bilgi, özelleştirilmiş.

Uygulama hedefi işlenir ve Siri için bir yanıt döndürdü sonra ardından sonuçları kullanıcıya (sözlü ve görsel olarak) sunar:

[![](understanding-sirikit-images/monkeychat06.png "Sözlü ve görsel olarak kullanıcıya sunulan sonuçları")](understanding-sirikit-images/monkeychat06.png#lightbox)

Siri her uygulama için kullanılabilir etki alanları için birkaç yerleşik yanıt kullanıcı arabirimleri vardır. Ancak, MonkeyChat isteğe bağlı bir hedefi UI uzantı sağlanan olduğundan, yukarıdaki örnekte kullanıcıya konuşma sonuçlarını sunmak için kullanılır.

## <a name="the-intent-lifecycle"></a>Hedefi yaşam döngüsü

Uygulama Uzantısı amaçları ile ilgilenirken gerçekleştirmek için gereken üç ana görevleri şunlardır:

[![](understanding-sirikit-images/monkeychat07.png "Hedefi yaşam döngüsü")](understanding-sirikit-images/monkeychat07.png#lightbox)

1. Uygulama gerekir **gidermek** her bir olay parametresi. Uygulama ve kullanıcı ne istenen üzerinde kabul kadar sonuç olarak, uygulama Çöz (kez başına her bir parametreyi) birden çok kez ve bazen birden çok kez aynı parametrenin çağırır.
2. Uygulama gerekir **Onayla** , istenen hedefi işlemek ve böylelikle beklenen sonucu hakkında Siri söyleyin.
3. Son olarak, uygulamayı gerekir **işlemek** amacını ve istenen sonucu elde etmek için adımları uygulayın.

### <a name="the-resolve-stage"></a>Resolve aşaması

Resolve aşama Siri kullanıcı sağladı ve kullanıcının gerçekten amacı amacı uygulama tarafından işlendiğinde ne olacağını olmasını sağlar değerleri anlamanıza yardımcı olur. 

Bu aşama, uygulamanın kullanıcı Konuşma sırasında Siri'nin davranışını etkilemek için bir fırsat de sağlar. Bunu yapmak için uygulama sağlayacak bir **çözüm yanıtı**. Önceden tanımlanmış yanıt Siri anlar veri farklı türlerdeki mevcuttur.

En sık karşılaşılan çözümleme yanıt uygulamasından olacaktır **başarı**, uygulama anlamına eşleşen belirli veri parçası (örneğin, kullanıcı ekranı adı) parametreden bilir hakkında bilgi parçasına.

Belirtilen bir isteğin hakkında bilgi sahibi bilgileri doğru parçası eşleştiğini doğrulamak için uygulama gerektiği zaman zamanlar olabilir. Bu durumlarda, göndereceğiniz bir **ConfirmationRequired** Evet veya Hayır kullanıcıya gibi soru yanıtı *"Gönder ileti Bobo için mükemmel?"*

Uygulama seçeneklerinin kısa listeden seçmek kullanıcıdan burada ister diğer durumlar olabilir. Bu durumda, uygulama sağlayacak bir **Kesinleştirme** gibi seçim kullanıcıya iki ila on seçeneklerinin listesi Yanıtla: 

```csharp
Who do you want to message?

* Bobo the Great
* Bobo Jr.
* Little Bobo
```

Siri sözlü ya da Siri kullanıcı Arabirimi ile etkileşerek seçim yaparak kullanıcı işleyecek ve sonucu uygulamaya tekrar gönderilir.

Diğer durumlarda, uygulamanın parametresi gidermek için yeterli bilgi olmayabilir veya Kesinleştirme (örneğin, kendi ad 80 kullanıcılar Bobo ile) kullanarak çözümlemek için çok fazla eşleşme olabilir. Bu durumda, uygulama göndereceğiniz bir **NeedsMoreDetails** yanıt ve Siri daha belirgin kullanıcıya sorar.

Kullanıcı için işlem hedefi için gerekli olan bir değer sağlarsanız, bu kaydetmedi gönderebilirsiniz bir **NeedsValue** yanıt değeri kullanıcıdan Siri sahip.

Uygulama kullanıcı için belirli bir parametre verdiği bir değer desteklemiyorsa, gönderebilirsiniz **UnsupportedWithReason** neden değildi değeri desteklenen bir gerekçe sağlamasını yanıt. Siri sonra kullanıcı için tamamen yeni bir değer ister ve bunları nedeni bu gereklidir.

Son olarak **NotRequired** yanıt uygulaması için verilen bir parametrenin değeri gerektirmez Siri söyleyin. Kullanıcı bir yine de sağlarsa, yalnızca Siri tarafından yoksayılacak.

### <a name="the-confirm-stage"></a>Onayla aşaması

Onayla aşama iki amaca sahiptir:

- Siri kullanıcı olmasını neler olduğunu yapıp yapmadığını amacına işleme beklenen sonucu Siri bildirmek için.
- Bir Fırsat onay uygulamanın banka istenen ödeme yapmak için yeterli para olması gibi kullanıcı tarafından sunulan isteği tamamlamak için gerekebilecek tüm gerekli durumları sağlar.

Uygulama sağlayacak bir **hedefi yanıt** Onayla adımından, doldurulmuş ile Siri kullanıcıyla etkili bir şekilde kurabilmek için kadar bilgi uygulama kullanılabilir olduğu gibi.

Etki alanı ve eylem türüne bağlı olarak, Siri onay gibi ödeme gönderme veya bir kılma kayıt önce kullanıcıdan.

### <a name="the-handle-stage"></a>Tanıtıcı aşaması

Tanıtıcı aşama, burada uygulama kullanıcının isteği yapmak için istenmişti görev gerçekleştirerek yerine getiren noktası olduğundan, bir hedefi ile çalışma en önemli bir parçasıdır.

Yalnızca onaylayın aşamasında olduğu gibi uygulama Siri bu kullanıcıya ilişkilendirebilirsiniz şekilde sonucu ile ilgili mümkün olduğunca fazla bilgi sağlaması gerekir. Bazen bu bilgileri görsel olarak sunulur veya Siri bazen yalnızca bu kullanıcıya geri seslendir. 

Uygulama ağ çağrısı gecikmeler gibi belirli bir isteği işlemek için ek süre gerekebilir veya dinamik bir kişi (tamamladıktan ve sipariş sevkiyat veya kullanıcının konuma bir araba yürüten) gibi isteği gerçekleştirmek gerekiyorsa kez olabilir. Siri uygulaması'ndan yanıt beklenirken uygulama isteğini işliyor söyleyen kullanıcıya bekleyen bir kullanıcı Arabirimi görüntülenir.

İdeal olarak, uygulama Siri yanıt iki ila üç saniye içinde en çok sağlamalıdır. Uygulamayı belirtilen yanıt işleme daha uzun sürer gittiği biliyorsa göndermek gereken bir **devam ediyor** Siri yanıt kodu. Siri sonra uygulama arka planda isteği işlemeye ve Siri ortam bırakırsanız bile bunu yapmak devam edecek bilgilendirir.

## <a name="adding-sirikit-to-the-app"></a>Uygulamaya SiriKit ekleme

Apple iOS 10 içinde SiriKit ile iki yeni uzantı noktaları oluşturdu:

- **Hedefleri uzantısı** - uygulamanın içerikle Siri sağlar ve desteklenen tüm hedefleri yerine getirmek için gereken görevleri gerçekleştirir.
- **Hedefleri UI uzantısı** -Siri içinde uygulamaların içeriği için görüntülenecek özel bir kullanıcı Arabirimi sağlar. 

Sözcük ve tümcecikleri biçiminde tanımaya yardımcı olmak için Siri sağlamak için bir API vardır:

- **Uygulama sözlük** -sözcük ve her uygulama kullanıcı için ortak olan deyimleri.
- **Kullanıcı sözlük** -sözcük ve belirli bir uygulamanın kullanıcıya benzersiz deyimleri.

## <a name="the-intents-extension"></a>Hedefleri uzantısı

Hedefleri uzantısı, uygulama ve Siri arasında ana etkileşimleri aşağıdaki gibi işlemekten sorumludur:

[![](understanding-sirikit-images/intents01.png "Hedefleri uzantısı")](understanding-sirikit-images/intents01.png#lightbox)

Hedefi uzantısı bir veya daha fazla amacı destekleyebilen, nasıl SiriKit uygulamada uygulamak istedikleri karar vermek için geliştirici kadar olur. Geliştirici, ele alınması gereken her amaç için ayrı bir hedefi uzantı de ekleyebilirsiniz.  Bu, böylece Siri uygulamayı karşı daha fazla bellek ve süresi işlemek için gerektiren açmak birden çok işlem yok Geliştirici hedefi uzantıları sayısı sınırı Apple istekleri belirtti.

Geliştirici de Siri etkinken hedefi uzantısı arka planda çalışan olduğunu bilmeniz gerekir. Bu isteği bilgilerini işlemeye uzantılı hala iletişim sırasında bir görüşme kullanıcı ile etkin olarak taşımak Siri sağlar.

## <a name="privacy-and-security-considerations"></a>Gizlilik ve güvenlik konuları

Apple bilgi Siri ile çalışırken, güvenli olduğunu ve bu nedenle, iOS cihazında oturum açmanız kullanıcının gerektiren birkaç etkileşimleri özel bir kullanıcı emin olmak için harika ölçüleri sürdü. Örneğin, bir kılma isterken veya bir ödeme oluşturma.

Ayrıca, uygulama cihazda oturum açmış kullanıcı için sınırlamak isteyebilirsiniz belirli davranışları vardır. Bu durumlarda, uygulama isteyebilir **kısıtlamak kilitli durumda** davranışı. Bu ayarı aracılığıyla yapılır `Info.plist` dosya.

Uygulama kullanıcının ek kimlik doğrulama bilgileri isteyin aygıt zaten kilidi açılmış olsa bile yerel kimlik doğrulama hedefi uzantısı kullanılabilir çerçevedir.

Son olarak, Apple Pay uygulama Apple Pay kullanarak bir işlem tamamlayabilirsiniz ve yerleşik Apple Pay sayfanın Siri arabirimi görünür hedefi uzantısı için kullanılabilir.

Ayrıca, kullanıcıların bildiği şeklinde 3 taraf uygulama ve bu nedenle, kullanıcı bilgilerini ne zaman göndermesini sağlamak Apple istediği **gerekir** (belirtildiği gibi uygulamanın paket görünen adı) uygulamasının belirli adı zaman söyleyin bir istekte.

Apple Siri doğal, akıcı görüşmeleri kullanıcıyla ve bu nedenle yürütmek için tasarlanmış, birçok bölümleri konuşma içinde uygulamanın paket adı, bir doğal olarak kullanıcının isteği uygun her yerde kullanılabilir.

Kullanıcıların ne yapacağını yaygın şeyler "uygulamanın adını, verbify için" başka bir deyişle, uygulama adını alma ve fiili bir istek olarak kullanarak biridir. Örneğin, *"MonkeyChat Bobo Bu harika muzlar olan."*

## <a name="the-intents-ui-extension"></a>Hedefleri UI uzantısı

Hedefleri UI uzantısı uygulamanın kullanıcı Arabirimi ve Siri deneyim halinde markalama getirmek için bir fırsat sunar ve uygulamaya bağlı eşitleyerek kullanıcılar yapın. Bu uzantı, uygulama marka yanı sıra dökümü görsel ve diğer bilgileri kullanıma sunabilirsiniz.

[![](understanding-sirikit-images/intents02.png "Örnek hedefleri UI uzantısı çıktı")](understanding-sirikit-images/intents02.png#lightbox)

Hedefleri UI uzantısı her zaman döndürülecek bir `UIViewController` ve uygulama yöntemlerine ilk yanıt gider gösteren ek bilgiler gibi görünüm denetleyicisini içinde herhangi bir şey ekleyebilirsiniz. Hedefleri UI konumlarına erişmek için araba paylaşımı kılma ne kadar uzun sürer gibi uzun süren bir olayın durumu olan kullanıcı da güncelleştirebilirsiniz.

Hedefleri UI uzantısı her zaman diğer Siri gibi içerik uygulama simgesine ve kullanıcı arabirimini başındaki ad ile birlikte görüntülenir ve yüklenecek, amacına, (örneğin, Gönder veya iptal) düğmeleri en altında görüntülenebilir.

Uygulama Siri kullanıcıya Mesajlaşma gibi varsayılan olarak görüntüleme veya uygulamayı bir uygulamaya özel olarak hazırlanmış varsayılan deneyimi yeri değiştirebilirsiniz eşlemeleri bilgileri nerede değiştirebilirsiniz birkaç örneği vardır.

> [!IMPORTANT]
> Etkileşimli öğeler eklemek mümkün olmakla birlikte `UIButtons` veya `UITextFields` hedefi UI uzantının için `UIViewController`, bu amacı kullanıcı Arabiriminde etkileşimli olmayan olarak kesinlikle yasaktır ve kullanıcı etkileşim mümkün olmayacaktır.

Siri varsayılan bir hedefi her türü için UI içerdiğinden hedefi UI uzantı sağlamak uygulama için tamamen isteğe bağlıdır. Ayrıca, hedefleri UI arabirimleri yalnızca belirli hedefleri Apple kabul kullanıcı için yararlı olacaktır kullanılabilir.

## <a name="adding-sirikit-vocabulary"></a>SiriKit sözlük ekleme

SiriKit uygulamanın son parçası, gerekli sözlük sağlayarak uygulama içinde arasındadır. Birçok uygulama benzersiz şekilde kullanıcı uygulamaya bilgileri sağlarız ve kullanıcı bilgilerini tanımlama benzersiz yolu vardır.

Bu nedenle, sözcük ve deyimleri uygulamaya benzersiz anlamak için uygulamanın Yardım Siri gerektirir. Her kullanıcı bilmeniz ve bunları anladığınızdan emin bu tümcecikleri bazıları uygulamanın parçası olacak. Henüz başkalarının belirli bir kullanıcının uygulama için benzersiz olacaktır.

### <a name="app-specific-vocabulary"></a>Uygulama özel sözlük

Uygulama özel sözlük tüm uygulamanın kullanıcılar, araç türleri veya Etkinlik adları gibi belirli sözcük ve bilinecek deyimleri tanımlar. Bu uygulamanın bir parçası olduğundan, bunlar içinde tanımlanan bir `AppIntentVocabulary.plist` dosyası ana uygulama paketi bir parçası olarak. Ayrıca, bu sözcükleri ve tümcecikleri yerelleştirilmiş.

Sözlük için birkaç bölümden oluşur `AppIntentVocabulary.plist` dosyası:

- **Örnek uygulamanın kullandığı** -bu kullanıcı uygulama yapabilir istekleri için ortak kullanım durumları kümesi sağlar. Örneğin: *"bir etkinlik MonkeyFit ile başlatın."*
- **Parametreleri** -bu uygulama için belirli standart olmayan parametre türleri kümesi sağlar. Örneğin, etkinlik adları MonkeyFit uygulaması. Bu, şunlardan oluşur:
    - **Tümcecik** -uygulama için benzersiz koşullarını tanımlamak yazmasına izin verir. Örneğin: MonkeyFit uygulama için "Bananarific" etkinlik türü. 
    - **Söyleniş** -verir telaffuz ipuçları Siri tarafından verilen bir deyimi için basit bir ses yazım olarak. Örneğin, "ba nana RI belirli".
    - **Örnek** -verilen tümcecik uygulamada kullanmaya ilişkin bir örnek sağlar. Örneğin, *"Başlat bir Bananarific içinde MonkeyFit"*.

Daha fazla bilgi için lütfen Apple'nın bkz [uygulama sözlük dosya biçimi başvurusu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

### <a name="user-specific-vocabulary"></a>Kullanıcı belirli sözlük

Sözcükleri veya uygulama bireysel kullanıcılara benzersiz tümceleri sağlamak için kullanıcı belirli sözlük geçiyor. Bu ana uygulamadaki (uygulama uzantıları değil) çalışma zamanında listenin başlangıcında en önemli terimler sahip kullanıcılar için en önemli kullanım önceliği sipariş terimler sıralı bir dizi olarak sağlanacaktır.

Yukarıda gösterilen MonkeyChat uygulama örneği göz atın. MonkeyChat Siri kullanıcı belirli sözlük üzerinden göndereceği kullanıcının kişiler, tümünün listesini tutar. Ayrıca kullanıcı messaged 10 en son kişilerin listesini tutar ve her kullanıcı için sık kullanılan kişiler kümesi vardır. Bu örnekte, sık kullanılan kişiler bizim kullanıcı belirli yeni kişiler tarafından izlenen sözlük, başlangıcında sonra kullanıcının kişiler rest olmalıdır.

Aşağıdaki bilgi türlerini kullanıcı belirli sözlük tarafından desteklenir:

- Adları başvurun.
- Etkinlik adları.
- Fotoğraf albümü adları.
- Fotoğraf anahtar sözcükler.

Uygulama iOS adres defteri dayalıysa, bu bilgileri zaten Siri kullanılabilir olduğu gibi uygulama herhangi bir eylemde bulunmanız gerekmez. Uygulama, yalnızca uygulamanın kendi benzersiz bir veritabanı kişilerin varsa kişi adlarını sağlaması gerekir.

Sözlük tasarlarken, yalnızca kullanıcıların bilmeniz ve önem verdiğiniz gerekli değerleri girin. Telefon numarası veya e-posta adresleri gibi bilgileri sağlayan kaçının.

Uygulamanın, kullanıcı özel sözlük değiştiğinde Siri hemen güncelleştirmek gerekir. Kullanıcılar için bilgi isteyen Siri iOS cihazlarını eklenen anlık alışmanızı. Örneğin, kullanıcı uygulamada yeni bir kişi eklerse, kullanıcı kaydeder hemen Siri bu bilgileri gönderir.

Daha da önemlisi, uygulama _gerekir_ bu yana bir kullanıcı bir bilgiyi silindi ancak Siri hala algılamayı, saat veya gün sonra üzmeye hale bilgileri Siri sözlük hemen silin.

> [!IMPORTANT]
> Uygulama tüm kullanıcı belirli sözlük Siri kullanıcı uygulamayı sıfırlamayı seçerse veya varsa kaldırmalıdır bunlar oturum kapatma.

## <a name="sirikit-permissions"></a>SiriKit izinleri

Son parçası SiriKit, izinleri ortalanır. Yalnızca iOS (örneğin, fotoğraflar, kamera veya kişiler) diğer özelliklerini kullanarak gibi kullanıcıların uygulamanın Siri konuşun için açık izni gerekmez.

Uygulama Siri sağlamak ve kullanıcının bu erişim neden vermelisiniz konusunda bir nedeni verin hangi bilgilerin tanımlayan bir dize sağlayabilir. 

Apple app iOS 10 yükselttikten sonra kullanıcı uygulamayı açar Siri ilk kez kullanmaya kullanıcıdan izin iste önerir. Böylece kullanıcılar Siri tümleştirmesi hakkında bilmeniz ve ilk talebinde yapmadan önce önceden onaylanmış kullanım için budur.

## <a name="sirikit-and-maps"></a>SiriKit ve eşlemeleri

SiriKit iOS, dahili bir parçasıdır ve iOS 10 eklenen büyük hedefleri framework'ün kullanır. Hedefleri framework ortak ve paylaşılan eylemleri ve hedefleri sisteminin diğer bölümleriyle paylaşmak için tasarlanmıştır.

Hedefleri framework yalnızca Siri tümleştirme gider ve burada uygulama varsayılan telefon ya da belirli kişiler için Mesajlaşma uygulaması hale gelebilir kişiler tümleştirme gibi diğer özellikleri sağlar. Hedefleri de kullanıcılar olası en iyi VoIP deneyim sağlamak üzere CallKit ile derin tümleştirme sağlar.

İOS 10 eşlemeleri uygulamasında kullanıcı Maps UI içinde doğrudan kılma burada ayırabilirsiniz kılma paylaşımı gibi özellikleri eklemiştir. Kılma paylaşım (ve diğer) hedefleri Siri ve haritalar arasında paylaşılabilmesi SiriKit Haritalar ile ortak bir uzantı noktası sağlar. 

Bu uygulama SiriKit Uzantıları'nı benimsemiştir, onu da eşlemeleri tümleştirme ücretsiz alırsa, anlamına gelir. 

## <a name="designing-a-great-siri-experience"></a>Harika bir Siri deneyim tasarlama

Bir uygulama Siri tümleştirdiğinizde iyi kullanıcı deneyimini tasarlama harika uygulama kullanıcı arabirimi tasarlamaya göre farklıdır. Kullanıcının doğrudan üzerinde uygulamayla Burada etkileşim normal durumlarda aksine Siri var. kullanırken ekran olan birçok kez visual arabirimi olmadan hiç görünür olduğunda. Örneğin, kullanıcının başladığında sahip olan Konuşma *"Hey Siri"*.

### <a name="how-siri-helps-the-developer"></a>Siri Geliştirici nasıl yardımcı olur

Siri ile bir uygulamanın etkileşimleri tasarlarken, uygulamanın derleme bir *konuşma arabirimi*, bağlam anlamı türetilen Siri kullanıcı uygulamanın adına sahip olan konuşma.

Görsel bir başvuru olmaması durumunda, kullanıcı kendi head sunulmasını bilgileri izlemek gerekir. Bu nedenle, Siri kullanıcı gerçekleştirmek isteyen görev elde etmek için gerekli olan tam en az bilgiyi gösterir.

Konuşma arabirimi sorular ve yanıtlar tarafından hem kullanıcı hem de Siri sırasında Konuşma şeklinde. Bu nedenle nasıl Siri sorular sorar ve bu arabirim tasarlarken yanıt verdiğini hakkında düşünmek önemlidir.

Aşağıdaki örnek Siri soruyla yanıt vermeyebilir bir ileti oluşturma kullanıcının ele *"Göndermek hazır?"* . Kullanıcı birçok farklı yolla gibi yanıt verebilir *"gönderme"*, *"İptal"* veya tamamen ilgisiz bile bir şey bu soruyu. Nasıl konuşma oynatılacağını olsun, Siri uygulama için işlemek ve kullanılabilir hale geldiğinde yalnızca ilgili bilgileri gönderebilirsiniz.

Bir kullanıcı Siri konuşma başlatmak birkaç farklı yolu vardır:

- Kullanıcının cihazı seçerek giriş düğmesine basarak. Bu durumda, daha fazla visual arabirimleri ve daha az sözlü yanıtları Siri sunacaktır.
- Bildiren tarafından *"Hey Siri"* ve ellere serbest konuşma başlatılıyor. Bu durumda, daha az görsel ve daha sözlü Siri olacaktır.
- Bluetooth gibi erişilebilirlik özellikleri kullanılarak işitme burada UI özel gereksinimleri olan bir kullanıcı için kurulmasını yardımları etkinleştirildi.
- Burada kullanıcının dikkatini tutmak için gereken araba yürütmek kullanarak minimum karışıklıkları tutarak yürüten odaklanır.

### <a name="how-the-developer-helps-siri"></a>Geliştirici Siri nasıl yardımcı olur

Bir uygulama Siri ile tümleştirdiğinizde, geliştirici genellikle bu tümleştirme sınamak ve bunlar mümkün olduğunca gibi birçok farklı yolla aynı parça bilgi ya da görev için isteyerek birçok farklı istek kuran emin olmak gerekir.

Düşünme benzer hiçbir iki kişiler olduğundan, geliştirici ince ayar yardımcı olması için olabildiğince çok farklı beta Test edenlere Siri tümleştirme alır önemlidir. Kullanıcılar için bilgi sorun veya yapma yollarla hiçbir zaman ancak geliştirici istekleri, ve bu ince ayar en geniş kullanıcı grubuna sahip, uygulama ile Siri kullanarak harika bir deneyim sağlamaya yardımcı olur.

Farklı durumlarda hem de ortamlarında test edin. Tüm bu konuşmaları sıvı kalmasını sağlamak olası ve doğal yolla Siri sohbet başlatır. Burada kullanıcının büyük olasılıkla uygulama gibi bir kalabalık Spor salonu içinde kullanırlar konumlarda sınayın.

Uygulama Siri düzgün kullanıcıya istek ve sonucu temsil etmek gereken bilgilerin tümünü sağladığını emin olun. Siri ellere boş bir durumda kullanırken özellikle geçerlidir.

### <a name="siri-design-guidelines"></a>Siri tasarım yönergeleri

Siri uygulama adına bir konuşma kullanıcıyla sahip hiçbir zaman unutmayın. Geliştirici emin değilseniz için bu konuşma kalmasını esnektir ve mümkün olduğunca doğal olarak istemektedir.

Tüm önemli konuşma yaptıkları gibi geliştirici aşağıdaki emin olması gerekir:

- Uygulama için konuşma olduğunu hazırlanır.
- Uygulamanın tam olarak hangi kullanıcıya dinler gerçekleştirmek çalışıyor.
- Uygulama uygun soruları uygun zamanlarda isteyeceğini.
- Uygulama kullanıcı aramayı bilgilerle isteğine yanıt verdiğini.

#### <a name="preparing-for-the-conversation"></a>Konuşma için hazırlama

Anımsaması ilk şey, uygulamanın kullanıcılara tam olarak gibi geliştiriciyi olacağını değil, ' dir. Bunlar, farklı diller farklı arka planlar konuşun veya özel gereksinimleri ile uygulama çalışırken sahip gelebilir.

Geliştirici tasarlanmış ve uygulama yerleşik olduğundan, ayrıca, derin, hem uygulama ve onun çalışmalar ve normal bir kullanıcı değil olacak işlevlerini intimate bilgisine sahip oldukları. Bu nedenle Geliştirici normal bir kullanıcı farklı Siri isteği isteyebilir.

Siri aracılığıyla uygulamayı mümkün olduğunca çok sayıda farklı kişi etkileşimde olması önemlidir nedeni budur. Kullanıcıların Geliştirici hiçbir zaman ya da Geliştirici dikkate almadığından şekillerde zorlayıcı Siri aracılığıyla uygulamayı isteklerinin kalmasına neden olabilir.

#### <a name="ensure-the-app-is-a-good-listener"></a>Uygulama iyi bir dinleyici olduğundan emin olun

Geliştirici uygulama iyi bir dinleyici ve kullanıcının beklentilerine uygun konuşma ayrıntılarını alma sağlaması gerekir. Ancak da bunlar uygulama istenen görevi elde etmek için gerektirdiği tüm bilgiler sağlamamış olabilirsiniz, mümkündür.

Uygulama bu durumu yönetmek birkaç yolu vardır:

- **İyi bir varsayılan değeri eksik için çekme** - Örneğin bunlar burada gelen çekilmesi istedikleri belirtmediyseniz, uygulama paylaşımı kılma kullanıcının geçerli konumuna varsayılan.
- **Bir dayalı tahminde** -uygulama kullanıcıya topladı belirli bilgileri kullanarak, uygulama yapma ve dayalı bir tahmin kullanıcının kişi bilgileri eksik cep telefonu numarası doldurma gibi eksik bilgileri olabilir. Ancak, beklenmeyen durumları, çekme en ucuz seçeneği, vb. gibi hatalı, kaçınmak için dikkatli olunmalıdır.
- **Daha fazla bilgi için komut istemi** -uygulama eksik değeri kullanıcıdan Siri sahip olabilir. Ancak, anahtarı buraya basit ve noktasına görüşmeleri engelliyor. Hızlı bir şekilde kullanıcıların istedikleri elde etmek için birkaç soruları yanıtlamak varsa engellenen olur.
- **Yanlış işleyebilmesini** -kullanıcı uygulamayı beklediği değildi veya verilen içerikte işleyemeyen bir değer sağlayabilir. Uygulama bu durum, Temizle ve bunları düzeltmek daha kolay hale getirir şekilde kullanıcıya ilişkili emin olun.

Uygulama söz konusu olan tek bir değer ile sunulduğunda, bu durumu çözmek için tercih edilen yol onaylama için kullanıcıya sor Siri sağlamaktır. Örneğin, *", Bobo mükemmel mu demek istediniz?"* , hangi basit bir Evet veya Hayır Yanıtla yanıt verebilir.

Burada birkaç olası seçenek için tek bir değer doğru olabilir bir durum olduğunda Kesinleştirme tercih edilen işleme yöntemidir. Bu durumda Siri aralarından seçim yapabileceğiniz en fazla on olası seçenekler kullanıcıyla isteyebilir. Örneğin:

```csharp
Who do you want to send the message to?

* Bobo the Great!
* Bobo Jr.
* Little Bobo
```

Yine söz konusu ise, belirli bir değeri için tamamen yeni ve daha belirli bir yanıt sağlamak için kullanıcıya sor Siri sahip.

#### <a name="request-final-confirmation"></a>Son onay iste

Uygulamayı gerçekten kullanıcının isteği yerine getirmek için bir görev gerçekleştiren önce Siri her şeyin yerinde olduğundan emin olmak için uygulama uzantılı kontrol eder. Örneğin, kullanıcı hesabında istenen ödeme yapmak için yeterli para var mı?

Ayrıca, uygulamanın kullanıcıya sunmak ve yaklaşık gerçekleştirilecek görevi beklentileri karşıladığını onaylamak için tüm olası bilgileri Siri sunan emin olmak için gerekir.

Kullanıcı isteği ve uygulama onayladıktan sonra bunu yeniden, bunları kullanıcıya ilişkilendirmek için tüm sonuçları geri Siri sağladığından emin olmak için uygulama gereksinimlerini gerçekleştirdi.

#### <a name="responding-to-the-request"></a>İsteğe yanıt

Siri her etki alanları ve hakkında bilgi sahibi eylemler için birkaç yerleşik kullanıcı arabirimleri vardır. Ancak, uygun olan yerlerde, uygulama uygulamanın marka ve kullanıcı Arabirimi veya istekte mevcut olandan daha fazla bilgi sunarak kullanıcı deneyimini zenginleştirmek için özel bir amaç UI uzantısı sağlar.

Bu, özel arabirimler için Siri tasarlarken restraint kullanılması gerektiğini belirtti. Genellikle, kullanıcı olabildiğince çabuk yapılan belirli bir görevi almak isteyen ve gereksiz bilgilerle aşırı yüklenmiş istememektedir.

Özel kullanıcı Arabirimi arar ve doğru biçimde tümünde farklı iOS cihazları ve kullanıcı olabilir veya aygıt kullanarak yönler yanıt emin olmak için de dikkatli olunması.

Uygun olduğunda SiriKit API varsayılan Siri UI zaten mevcut tüm gereksiz bilgileri gizlemek için kullanın. Henüz, onu sözlü ellerini ücretsiz durumlarda sunabilir şekilde uygulama hala bilgileri Siri sağlayan emin olun.

Siri gibi kullanıcı istedi fotoğraf sunan kullanıcının isteği yerine getirmek için uygulamasını nerede başlatır durumlar olabilir. Bu durumlarda kullanıcının beklenmedik yok. Ara adımları veya daha fazla etkileşime gerek kalmadan beklenen bilgileri görüntüler. Hiçbir zaman bilgilerini görüntülemek veya kullanıcı bekleniyor değil bir görevi gerçekleştirme.

### <a name="polishing-the-design"></a>Tasarım polishing

Apple önermek konuşma arabirimleri tasarımını Lehçe için birkaç adım vardır. İlk olarak, NET, kısa dağarcığı ve kullanım örneği örneklere Siri sağlayarak olur.

Bir kullanıcı uygulama bulur yollarından biri olan Siri ve soran, konuşma başlatarak *"Ne yapabilirsiniz?"* Siri, geliştiricinin uygulama da dahil olmak üzere bunu yapabilirsiniz ve örnek kahramanı kullanım yoluyla sağlanan birçok farklı işlemler gösterir, `plist` dosya.

İyi örnek kullanım durumları yazmak nasıl:

- Örnek uygulama adı eklediğinizden emin olun.
- Kısa ve-noktaya örnek koruyun.
- Birden çok örnek her uygulamanın destekleyip amaçlar için sağlar.
- Hedefleri ve bunları en yaygın kullanım durumları uygulama için temel örneklerin öncelik verin.
- Uygulama yerelleştirilmiş örnekler sağladığından emin olun.
- Her örnek uygulama içinde beklendiği gibi çalıştığını verilen emin olun.
- Siri gibi metin içerme şekilde örneklerde adres önlemek *"Hey Siri..."*
- Tüm gereksiz pleasantries gibi kaçının *"Lütfen"* veya *"teşekkür ederiz"*.

Keşfetmek ve uygulama Siri kullanıcı kendi adına sahip olan konuşma nasıl biçimlendirebilirsiniz denemek için uygun zaman ayırın. İle bunların etkileşimler olarak işlemi boyunca tipik kullanıcılarla konuşun emin olun ve beklentileri uygulamanın zaman içinde değişebilir.

Farklı durumlarda uygulama ve tüm Siri konuşma çağırmak için farklı yöntemleri test etmek her zaman unutmayın. Test gerçek dünya konumlarda kullanıcının masasına ve office çıktığınızda uygulama kullanıyor olabilir.

Sıvı, doğal ve "yalnızca sağ eşitleyerek" sohbet Siri (adına, uygulama) olmaya çabalar. 

## <a name="summary"></a>Özet

Bu makalede SiriKit kullanmak için gereken ve Siri ve haritalar uygulama iOS cihazında kullanarak kullanıcı tarafından erişilebilir hizmetleri sağlamak için Xamarin.iOS uygulamaları ile etkileşim kurabilir gösterilen anahtar kavramlarını ele.




## <a name="related-links"></a>İlgili bağlantılar

- [ElizaChat Sample](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Hedefleri Framework başvurusu](https://developer.apple.com/reference/intents)
- [Hedefleri UI Framework başvurusu](https://developer.apple.com/reference/intentsui)
