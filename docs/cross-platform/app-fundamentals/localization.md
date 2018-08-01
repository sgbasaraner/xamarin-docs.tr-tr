---
title: Uygulama kullanıcı arabirimi yerelleştirme
description: Bu belge, uluslararası ve yerel platformlar arası kavramlarını açıklar ve bunların uygulama tasarımı nasıl etkiler inceler.
ms.prod: xamarin
ms.assetid: CC6847B2-23FB-4EDE-9F7E-EF29DD46A5C5
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 299fe28a896c2bf2e5c420b330b8e8bde7d9dc22
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781962"
---
# <a name="localization"></a>Yerelleştirme

Bu kılavuz kavramları tanıtır *uluslararası* ve *yerelleştirme* ve bu kavramları kullanarak Xamarin mobil uygulamaları oluşturmak yönergeler için bağlantılar.

Xamarin uygulamaları yerelleştirme düz ilişkin Teknik Ayrıntılar için atlamak istiyorsanız, bu platforma özgü nasıl yapılır makaleleri biriyle başlatın:

- [**Xamarin.Forms** ](~/xamarin-forms/app-fundamentals/localization/index.md) platformlar arası yerelleştirme RESX dosyaları kullanma.
- [**Xamarin.iOS** ](~/ios/app-fundamentals/localization/index.md) yerel platform yerelleştirme.
- [**Xamarin.Android** ](~/android/app-fundamentals/localization.md) yerel platform yerelleştirme.

## <a name="i18n-and-l10n"></a>i18n ve L10n

*Uluslararası duruma getirme* kodunuzu farklı dillerde görüntüleme ve görüntüsünü (örneğin, sayı ve tarih biçimlendirme) farklı yerel ayarlara adapte uyumlu hale getirme işlemidir. Bu ayrıca olarak adlandırılır *Genelleştirme*.

*Yerelleştirme* – her dil için kaynakları (örneğin, dize ve resimler) oluşturma ve bunları internationalize Uygulama paketleme izler adımdır.

Uluslararası duruma getirme genellikle i18n – 18 harf "i" ve "n" arasında için kestirme için kısaltılır. Yerelleştirme benzer şekilde kısaltılır L10n için – 10 Harfler "M" ve "n" arasında.

## <a name="overview"></a>Genel Bakış

Bu belge uluslararası duruma getirme ve yerelleştirme ve nasıl bunlar mobil uygulama geliştirme için genel uygulama ile ilişkili kavramları tanıtır.
Tasarlama ve uygulama, işleri oluşturma sabit kodlanmış önceden olabilir ancak yerelleştirme eklemek için parametreli hangi gerekir:

-   Ekranı düzeni ve metin
-   Simgeler, grafik ve renkler
-   Video ve ses dosyaları
-   Dinamik metin ve metin biçimlendirmesini (örneğin, sayılar, para birimi ve tarih)
 - Sağdan sola (RTL) dilleri için düzen değişiklikleri ve
-   Veri sıralama.

Hangi bağımsız olarak uygulamanız bu ipuçlarını hedefler mobil platformları yüksek kaliteli yerelleştirilmiş uygulama oluşturmanıza yardımcı olur.


## <a name="design-considerations"></a>Tasarım konuları

Böylece içeriği yerelleştirme olası uygulama mimarisi oluşturma uluslararası adı verilir. Uluslararası duruma getirme düzgün yapılması ve çalışma zamanında yüklenmesi için farklı bir dil yalnızca izin dizeleri – iyi tasarlanmış bir uygulama için tüm kaynakları değiştirilmesine izin vermeli sayısından daha fazla dil ve yerel ayar (görüntüleri, ses ve video dahil) dayalı olan uyarlayabilirsiniz biçimlendirme ve yerleştirme ile farklı başa çıkacak şekilde dizeleri boyuta sahip.

Bu bölümde uluslararası bir uygulama oluştururken dikkate alınacak bazı tasarım konuları açıklanmaktadır.

### <a name="layouts-and-string-length"></a>Düzenleri ve dize uzunluğu

Çince ve Japonca dizeleri çok kısa – bazen bir veya iki karakter için bir giriş alanı etiketi yeterince anlamlı olabilir.

Almanca dizeleri (örneğin) çok uzun olabilir; Bazen İngilizce görece kısa bir sözcük ya da kırpılmış veya başka beklenmedik bir şekilde düzeninizi içeriğin akışını yeniden haline gelen diğer dillerde – uzun olur.

İngilizce, Almanca ve Japonca IOS home ekranda birkaç öğe için dize uzunlukları karşılaştırın:

[![](localization-images/language-compare-sml.png "Almanca vs Japonca dize uzunluğu")](localization-images/language-compare.png#lightbox)

Dikkat **ayarları** İngilizce (8 karakter) Almanca çeviri ancak yalnızca 2 karakter uzunlukta Japonca 13 karakter gerektirir.

Görüntü etiketi ve giriş alanını yan yana nerede düzenleri zaman etiket uzunluğu büyük ölçüde değişebilir ile çalışmak zordur. Genellikle bir alanının üstünde etiketi görüntülenir burada bir düzen tam ekran genişliği etiketi ve giriş için kullanılabilir olmadığından yerelleştirme daha kolay olur.

Oluşturmakta olduğunuz ise genel bir kural olarak sabit düzenleri (özellikle yan yana öğeleri) % 50'en az İngilizce dizelerinizi genişliğinden daha fazla etiket ve metin için gerektirir izin verir. Bu her sorunu çözmenize olmaz ancak birçok durumda çalışacak bir arabellek sağlar.

### <a name="input-validation"></a>Giriş doğrulaması

Doğrulama kuralları yazılırken özelliklerinin kaybolacağını unutmayın. Tek bir harf nadiren herhangi bir anlam olduğundan "İngilizce olarak en az üç karakter gerektirecek şekilde" giriş metin alanı gerektirecek şekilde geçerli görünebilir. Çince ve Japonca dillerinde ancak tek bir karakterin geçerli bir giriş olabilir ve bir doğrulama ileti "en az 3 karakter gereklidir" Bu diller için mantıklı değildir.

Web sitesi URL'si karakterlerle daha karmaşık hale ASCII alt sınırlı değildir veya bir e-posta adresi doğrulama diğer görünen Basit görevler gibi.

Uluslararası duruma getirme – aklınızda doğrulama kurallarınızı en az kısıtlayıcı kuralları seçin ya da mantığı yazın, böylece her dil için farklı şekilde çalışır yazma.

### <a name="images-and-color"></a>Görüntüleri ve rengi

Her görüntü değiştirmek kullanıcının dil tercihine göre gerekir. Birçok simgeler veya fotoğraf tüm kullanıcılar için uygun, konuştukları hangi dilde önemli değildir.
Bazı kaynaklar rağmen yerelleştirme mantıklı gibi:

 - Görüntüleri gösteren kişi ya da belirli konumlara – uygulamanız eşitleyerek kullanıcılar için daha uygun yerel kişiler/konumları görünüyorsa.
 - Simgeler – bazı yansır kültüre özgü olabilir ve uygulamanızı yerel anlama yansıtacak şekilde görüntülerin yerelleştirme tarafından kullanmayı daha kolay hale getirebilir.
 - Renkleri bazı kültürler renkleri – farklı – anlamak kırmızı bir bölge, ancak başka bir İyi Şanslar uyarı anlamına gelebilir. Renkleri yerelleştirme için bir mekanizma oluşturmakta olup olmadığını belirlemek için uygulamanızı tasarlarken anadili ile denetleyin.


### <a name="videos-and-sound"></a>Video ve ses

Videolar ve bir uygulama çevrilen dizeleri almak görece kolay olsa da, birden çok voiceover kaydı izlediğinden yerelleştirilirken ses mevcut özel zorluklar veya video klip, pahalı ve zor olabilir.

(Özellikle, çok sayıda dilleri yerelleştirme veya çok sayıda medya dosyaları varsa) video ve ses dosyalarını birden çok kopyası, uygulamanızın boyutunu Ayrıca önemli ölçüde artırabilir. Kullanıcı, uygulamayı yükledi, ancak bu aynı zamanda düşük kullanıcı deneyiminde yavaş ağlarda sonuçlanabilir sonra yalnızca gerekli dil varlıklar indirme düşünebilirsiniz.

Genellikle Yerelleştirme sorunları çözmek için birden çok yolu vardır: önceden düşünün ve uygulamanızı bunları halletmeniz için tasarlanmış sağlamak için en önemli şeydir.


### <a name="dates-times-numbers-and-currency"></a>Tarihler, saatler, sayılar ve para birimi

.NET biçimlendirme işlevleri kullanıyorsanız, ondalık ayırıcı doğru Ayrıştırılan (ve oluşturulan dönüştürme özel durumları önlemek için) kültür belirtin unutmayın. Örneğin 1.99 ve 1,99 geçerli ondalık Beyanları bölgeniz bağlı olarak adlandırılır.

Veri bilinen bir kaynaktan yakında zaman (IE. kendi kodunuzu veya sizin denetlediğiniz bir web-hizmetinden) stillerinizin standart İngilizce dil biçimlendirme için çalışır InvariantCulture gibi biçimlendirme eşleşen bir kültür tanımlayıcısı olabilir.

```csharp
double.Parse("1,999.99", CultureInfo.InvariantCulture);
```

Uygulama kullanıcı tarafından giriş verileri, kendi yerel yansıtan bir CultureInfo örneği kullanarak ayrıştırılamıyor:

```csharp
double.Parse("1 999,99", CultureInfo.CreateSpecificCulture("fr-FR"));
```

Bkz: [sayısal dizeleri ayrıştırma](http://msdn.microsoft.com/library/xbtzcc4w(v=vs.110).aspx) ve [ayrıştırma tarih ve saat dizelerini](http://msdn.microsoft.com/library/2h3syy57(v=vs.110).aspx) ek bilgi için MSDN makaleleri.

<a name="rtl" />

### <a name="right-to-left-rtl-languages"></a>Sağdan sola (RTL) dilleri

Arapça, İbranice ve Urduca gibi bazı dillerde sağdan sola (örneğin), salt okunurdur.
Bu diller destekleyen uygulamalar için sağdan sola okuyucular, örneğin uyum ekran tasarımları kullanmanız gerekir:

 - Metni sağa hizalı olması gerekir.
 - Etiketleri giriş alanlarının sağa görüntülenmesi gerekir.
 - Varsayılan düğmesi yerleşimi genellikle ters çevrilir.
 - Hiyerarşik Gezinti geçirmeyi animasyon (ve diğer gezinti metaphors ve animasyonları) bağlam ayrıca ters mi için yönü kullanan.

İOS ve Android sağdan sola düzenleri ve yukarıdaki ayarlamalar yapmak için Yardım yerleşik özellikleri ile yazı tipi işlemeyi destekler. Xamarin.Forms RTL işleme otomatik olarak şu anda desteklemiyor.

### <a name="sorting"></a>Sıralama

Bile aynı karakter kümesini kullandığınızda farklı diller farklı şekilde, kendi alfabesinde sıralama düzenini tanımlayın.

Bkz: [dize karşılaştırma ayrıntı](http://msdn.microsoft.com/library/dd465121(v=vs.110).aspx#the_details_of_string_comparison) içinde [kullanarak dizeleri .NET Framework için en iyi uygulamaları](http://msdn.microsoft.com/library/dd465121(v=vs.110).aspx) burada dili (CultureInfo) etkiler sıralama düzenini bir örnek.

Dile özgü sıralama ek kod, iş mantığı uygulamak için gerekli olabilecek şekilde sıralama mobil platformlarda yerleşik veritabanı özellikleri destekleyecek olası değil.

### <a name="text-search"></a>Metin arama

Yazma ve göz önünde, arama algoritması birden çok dil ile test emin olun. Göz önünde bulundurmanız gerekenler şunlardır:

 - Otomatik Tamamlama işlevi oluşturulduysa otomatik tamamlama – kullanıcının dil ilgili öneriler kaynakları emin olun.
 - Eşleşen sorgu verilere – belirli bir girilen sorguları arayacaktır dil yalnızca içeriği o dilde karşı veya uygulamanızda tüm içeriği karşı yürütülebilir?
 - Benzer sözcükleri, word kökleri ve diğer arama iyileştirmelerini arama için aramanızı oluşturulduysa – kaynaklanan desteklediğiniz tüm diller için yerleşik bu iyileştirmeler misiniz?
 - Sıralama – sonuçları sıralandığını doğru (yukarıya bakın) emin olun.


### <a name="data-from-external-sources"></a>Dış kaynaklardan gelen verileri

Birçok uygulama, Twitter gelen dış kaynaklardan veri indirin ve hava durumu, haber veya hisse senedi fiyatları RSS akışları. Bu kullanıcıya görüntülerken, bunları ilgisiz veya okunamaz bilgilerinin bir ekran görüntülenir olasılığını dikkate almanız gerekir.

Deneyin ve uygulamanızı kullanıcıyla ilgili verileri görüntüler emin olmak için kullanabileceğiniz birkaç strateji vardır:

 - Farklı kaynaklar – uygulamanız, kullanıcının dilini veya yerel ayara bağlı olarak farklı bir kaynaktan veri karşıdan yükleyebilirsiniz. Yerel ayar Haberler, hava durumu ve hisse senedi fiyatları bir şey Kuzey Amerika akıştan indirilen çok daha fazla mantıklı olabilir.
 - Yerelleştirilmiş görünen – bir Twitter veya akış, fotoğraf görüntülüyorsanız, görüntülemesi gereken meta veriler (örneğin, geçen süre), kendi dilde bile içeriğin kendisini özgün dilinde kalır.
 - Çeviri – bir makine çevirisi gelen verilerin yapmak için uygulamanıza bir çeviri seçeneği oluşturabilirsiniz. Bu otomatik olarak veya kullanıcının istediğiniz kadar – yalnızca makine çevirileri hiçbir zaman kusursuz olduğundan bu yerde sürüyorsa kullanıcıya bildirmek emin olun!

Bu ayrıca ses izleri dış bağlantılar etkileyebilir veya kaynak için İleriyi emin olun, uygulamanızı tasarlama videolar – çevrilen içerik veya içerik içinde sunulmaz olduğunda kullanıcıların yeterli kullanıcı arabirimi tarafından haberdar olmasını sağlamaya kendi dili.


### <a name="dont-over-translate"></a>Aşırı Çevir yok

Uygulamanızda herhangi bir dize değil çevirme veya ihtiyacınız olabilir, en azından Çeviricisi tarafından özel dikkat etmeniz gereken. Örnekleri içerebilir:

 - URL'leri – bir URL listesi veya edebilir dili tarafından ayarlanması gerek kalmaz. Örneğin, facebook.com otomatik-ana sitedeki dil algıladığı çeviri gerektirmez. Diğer sitelere yerel ayarlara özgü içeriğe sahip ve yahoo.fr veya yahoo.it karşı yahoo.com gibi farklı bir URL sunmak isteyebilirsiniz.
 - Telefon numaraları – farklı ülke kodu veya numarası belirli bir dil seslendir çağıranlar için özellikle olanlar.
 - İlgili kişi ayrıntıları – adresleri ve diğer bilgileri dilini veya yerel ayarını göre değişebilir.
 - Ticari & ürün adları – bazı dizeleri bunlar her zaman aynı dilde çünkü çevirme gerek yoktur.

Son olarak, belirli dizeleri özel işleme gerekiyorsa ayrıntılı yönergeler için Çeviricisi eklediğinizden emin olun.


### <a name="formatted-text"></a>Biçimlendirilmiş metin

Mobil uygulamalar genellikle bir sorun dizeler genellikle zengin biçimlendirilmeyen olduğundan. (Örneğin kalın veya Yatık biçimlendirme) zengin metin uygulamanızda gerekliyse, ancak Çeviricisi bilir nasıl biçimlendirme giriş için dizeleri dosyalarınızı bunu doğru şekilde depolayın ve kullanıcıya görüntülenmeden önce düzgün biçimlendirildiğinden emin olun (IE. yanlışlıkla izin vermeyin biçimlendirme kodları kullanıcıya sunulan).



## <a name="translation-tips"></a>Çeviri ipuçları

Bir uygulama tarafından kullanılan dizeleri çevirme yerelleştirme işleminin bir parçası olarak kabul edilir. Genellikle bu görev bir çeviri hizmetine dış kaynaklı ve uygulamanızı veya işinizi bilemeyebilirsiniz çok dilli personeli tarafından gerçekleştirilen.

Aşağıdaki ipuçları doğru şekilde çevirin ve bu nedenle yerelleştirilmiş uygulamalarınızı kalitesini artırmak daha kolay dizeleri üretmenize yardımcı olur.



### <a name="localize-complete-strings-not-words"></a>Tam dizeleri, sözcükler olmamalıdır yerelleştirme

Bazen geliştiriciler, bunları uygulama genelinde yeniden kullanabilmesi için tek sözcük veya cümle 'parçacıkları' belirtin çalışılırken, bir yaklaşım uygular. Örneğin, metni ", 5 mesaj var." Çeviri için aşağıdaki dizeleri belirtebilir

**Hatalı**:

```csharp
"You have"
"no"
"message"
"messages"
```

ve ardından doğru tümcecik üzerinde-çalışma sırasında dize birleştirme kullanarak kod içinde oluşturmaya:

**Hatalı**:

```csharp
"You have" + " " + numMsgs + " " + "messages"
"You have" + " no " + "messages"
```

**Bu önerilmez** çünkü mutlaka tüm diller için çalışmaz ve Çeviricisi kısa her segment bağlamında anlaşılması zor olacaktır. Ayrıca farklı bağlamlarda kullanıldıkları (ve ardından güncelleştirilmesi gerekiyorsa), daha sonra sorunlara yol açabilir çevrilen dizeleri yeniden kullanım neden olmaktadır.


### <a name="allow-for-parameter-re-ordering"></a>Parametre yeniden sıralama için izin ver

.NET kavramı numaralı yer tutucuları, bu nedenle zaten destekler ancak bazı programlama dillerinin bir dizesinde parametreleri sırasını belirlemek için fazladan sözdizimi gerektirir

**İyi**:

```csharp
"a {0} b {1} cde {3}"
```

olabilir (burada yer tutucuları sırasını ve konumu değiştirildiğinde) aşağıdaki çevrilmiş

```csharp
"{2} {3} f g h {0}"
```

ve belirteçler hedeflenen Çeviricisi sıralanır. Her bir yer tutucu dize bir çevirmene gönderirken içeren bir açıklama eklediğinizden emin olun.


### <a name="use-multiple-strings-for-cardinality"></a>Önem düzeyi için birden çok dizenin kullanın

Dizeleri gibi kaçının `"You have {0} message/s."` daha iyi bir kullanıcı deneyimi sağlamak için belirli dizeleri her durum için kullanın:

**İyi**:

```csharp
"You have no messages."
"You have 1 messages."
"You have 2 messages."
"You have {0} messages."
```

Uygulamanıza görüntülenen sayı değerlendirmek için kod yazma ve uygun dize seçin gerekecektir. (İOS ve Android dahil) bazı platformlar, tercihlerine geçerli dil/bölge için en iyi çoğul dize otomatik olarak seçmek için yerleşik özellikler vardır.


### <a name="allowing-for-gender"></a>Cinsiyeti için izin verme

Latin tabanlı diller bazen konu cinsiyetiniz bağlı olarak farklı sözcükler kullanın. Uygulamanız hakkında cinsiyetiniz biliyorsa, bu yansıtmak üzere çevrilen dizelerin izin vermelidir.

Ayrıca daha belirgin söz konusu değildir bile dizeleri belirli kişi ya da kullanıcı, uygulamanızın nerede başvurmak İngilizce. Örneğin, bazı siteler iletileri ister Göster `"Bob commented on his post"` dizeleri için hem bir bir erkek, kadın ve ikili olmayan veya bilinmeyen cinsiyetiniz gerekir:

**İyi**:

```csharp
"{0} commented on his post"
"{0} commented on her post"
"{0} commented on their post"
```

### <a name="dont-reuse-strings"></a>Dizeleri yeniden yok

Veya dize farklı amaç veya anlamı olduğunda yalnızca benzer olduklarından dizeleri daha doğru bir şekilde yeniden yok.

Örneğin: bir açık/kapalı anahtar uygulamanızda olması ve anahtar denetim için metin 'on' ve 'off' yerelleştirilmesi gerekiyor düşünün. Siz de bu ayarın değerini başka bir yerde bir metin etiketi uygulamada görüntüler. Farklı dizeleri anahtarın durumu (varsayılan dilinizi aynı dizede olmaları durumunda bile) karşı– anahtar görüntülenmek örneğin kullanmanız gerekir:

-   "Açık" – anahtarda görüntülenir
-   "Kapalı" – anahtarda görüntülenir
-   Bir etiketi içinde "Açık" – görüntülenir
-   "Kapalı" – bir etiket görüntülenir

Bu, Çevirmen için en büyük esneklik sağlar:

-   Tasarım nedeniyle, belki de anahtar "açık" ve "kapalı" küçük kullanır ancak görüntü etiketi büyük harf "Açık" ve "Kapalı" kullanır.
-   Bazı diller anahtar değeri tam (çevrilmiş) sözcük etiketinde görünebilir ancak kullanıcı arabirimi denetim uyacak şekilde kısaltılmış gerekebilir.
-   Alternatif olarak, bazı dillerde kültür bilgisi için "Yeni" ve "O" kullanır, ancak hala "Açık" veya "Off" okumak için etiket isteyebilirsiniz, anahtar oluşturma olabilir.

### <a name="translation-services"></a>Çeviri Hizmetleri

#### <a name="machine-translation"></a>Makine çevirisi

Uygulamanıza çeviri özellikleri oluşturmak için göz önünde bulundurun [Azure Çeviricisi metin API](https://azure.microsoft.com/en-au/services/cognitive-services/translator-text-api/).

Test amacıyla, birçok çevrimiçi çeviri araçlardan birini bazı yerelleştirilmiş metin uygulamanızda geliştirme sırasında dahil etmek için kullanabilirsiniz:

- [Bing Çevirmen](https://www.bing.com/translator/)
- [Google Çevir](http://translate.google.com/)

Diğer birçok kullanılabilir vardır. Makine çevirisi kalitesini genellikle uygulama yayımlamayı iyi kabul değil ilk gözden ve profesyonel çevirmenler veya anadili test olmadan.

#### <a name="professional-translation"></a>Profesyonel çevirisi

Dizelerinizi almak ve bunları tamamlanmış çevirileri bir ücret karşılığında sağlanması kendi çevirmenler dağıtımın profesyonel çeviri hizmetleri de vardır.

En çok bilinen hizmetleri biri [LionBridge](http://www.lionbridge.com/). En Profesyonel hizmetler dizeleri, XML, RESX ve POT/SAS dahil olmak üzere tüm ortak dosya türlerini destekler.


## <a name="summary"></a>Özet

Bu makalede, uygulamanızın uluslararası hale getirme ve kaynaklarınızı yerelleştirme önce tanımanız gerekir ve ayrıca her platform için dil tercihlerini değiştirmek nasıl ele kavramların bazıları kullanıma sunuldu.

Bu kavramlar Xamarin ile olası çeşitli platforma özgü ve platformlar arası uluslararası teknikleri uygulanabilir.

Teknik Ayrıntılar ilgilendiğiniz platform için okuma devam edin:

- [Xamarin.Forms](~/xamarin-forms/app-fundamentals/localization/index.md) platformlar arası yerelleştirme RESX dosyaları kullanma.
- [Xamarin.iOS](~/ios/app-fundamentals/localization/index.md) yerel platform yerelleştirme.
- [Xamarin.Android](~/android/app-fundamentals/localization.md) yerel platform yerelleştirme.



## <a name="related-links"></a>İlgili bağlantılar

- [Apple'nın yerelleştirme genel bakış](https://developer.apple.com/internationalization/)
- [Android yerelleştirme denetim listesi](http://developer.android.com/distribute/tools/localization-checklist.html)
- [(MSDN) dünya çapında kullanılmaya hazır uygulamalar geliştirmek için en iyi uygulamalar](http://msdn.microsoft.com/library/w7x1y988%28v=vs.90%29.aspx)
