---
title: Diğer kaynaklar
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: f7833e238afe41d4a5ac7b8dde4c168f298752fc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30770956"
---
# <a name="alternate-resources"></a>Diğer kaynaklar

Diğer kaynaklar belirli bir aygıt veya çalışma zamanı yapılandırması geçerli dil, belirli ekran boyutu veya piksel yoğunluğu gibi hedef kaynaklardır. Android varsayılan kaynak belirli bir aygıt veya yapılandırma için daha belirgin olan kaynak eşleşiyorsa, kaynak yerine kullanılır. Geçerli yapılandırmasıyla eşleşen bir alternatif kaynak bulamazsa, varsayılan kaynakları yüklenecektir. Android karar nasıl hangi kaynakların bir uygulama tarafından kullanılacak olan kaynak konumu bölümünde aşağıdaki daha ayrıntılı ele alınacak

Diğer kaynaklar olarak bir alt dizinin varsayılan kaynaklar gibi kaynak türüne göre kaynakları klasörün içine düzenlenir. Alternatif kaynak alt biçiminde adıdır: _ResourceType_-_niteleyicisi_

*Niteleyici* belirli cihaz yapılandırmasını tanımlayan bir addır.
Bir ad, bir tire ile ayrılmış bunların her biri birden fazla niteleyicisi olabilir. Örneğin, aşağıdaki ekran görüntüsünde, yerel ayar, ekran yoğunluğu, ekran boyutuna ve yönüne gibi çeşitli yapılandırmaları için alternatif kaynaklar içeren basit bir proje gösterir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Diğer kaynaklar](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Diğer kaynaklar](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

Niteleyiciler bir kaynak türü eklerken aşağıdaki kurallar geçerlidir:

1. Bir tire ile ayrılmış her niteleyici ile birden fazla niteleyicisi olabilir.

2. Belki de yalnızca bir kez belirtilen niteleyicileri.

3. Niteleyiciler, aşağıdaki tabloda göründükleri sırada olmalıdır.

Olası niteleyicileri başvurusunu aşağıda listelenmiştir:

- **MCC ve MNC** &ndash; [mobil ülke kodu](http://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) ve isteğe bağlı olarak [mobil ağ kodu](http://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC). SIM kart MCC sağlarken cihazın bağlandığı ağ MNC sağlayacaktır. Mobil ülke kodunu kullanarak hedef yerel ayarlarına mümkün olsa da, önerilen yaklaşım aşağıda belirtilen dil niteleyicisi kullanmaktır. Örneğin, Almanya, hedef kaynaklara niteleyici olacaktır `mcc262`. T-Mobile ABD hedef kaynaklarına niteleyici `mcc310-mnc026`.
  Mobil ülke kodlarına ve mobil ağ kodları tam listesi için bkz: <http://mcc-mnc.com/>.

- **Dil** &ndash; iki harfli [ISO 639-1 dil kodu](http://en.wikipedia.org/wiki/ISO_639-1) ve isteğe bağlı olarak izleyen iki harfli [ISO 3166 alfa 2 bölge kodu](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). 
  Her iki niteleyicileri sağlanan sonra tarafından ayrılmış bir `-r`. Örneğin, hedef Fransızca konuşan yerel sonra Niteleyici için `fr` kullanılır. French-Canadian yerel ayarlar, hedeflenecek `fr-rCA` kullanılacaktır. Dil kodlarını ve bölge kodları tam listesi için bkz: [kodları gösterimi, adları, diller için](http://www.loc.gov/standards/iso639-2/php/English_list.php) ve [ülke adları ve kod öğeleri](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm).

- **En küçük genişliği** &ndash; uygulamayı yürütmek için yöneliktir en küçük ekran genişliğini belirtir. Daha ayrıntılı olarak ele [değişen ekranlar oluşturma kaynakları](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  API düzeyi 13 (Android 3.2) ve üzeri kullanılabilir. Örneğin, Niteleyici `sw320dp` , yükseklik ve genişlik olduğu en az hedef aygıtları için kullanılan 320dp.

- **Kullanılabilir genişlik** &ndash; biçimi w ekranın en küçük genişliği*N*dp, burada *N* yoğunluğu genişliği bağımsız pikseldir.
  Bu değer, kullanıcı aygıtı döndüğü olarak değişebilir. Daha ayrıntılı olarak ele [değişen ekranlar oluşturma kaynakları](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Kullanılabilir API düzeyi 13 (Android 3.2) ve üzeri. Örnek: niteleyici w720dp en az 720dp genişliğini cihazları hedeflemek için kullanılır.

- **Kullanılabilir yükseklik** &ndash; biçimi h ekranında en küçük yüksekliğini*N*dp, burada *N* dp içinde yüksekliği. Bu değer, kullanıcı aygıtı döndüğü olarak değişebilir. Daha ayrıntılı olarak ele [değişen ekranlar oluşturma kaynakları](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Kullanılabilir API düzeyi 13 (Android 3.2) ve üzeri. Örneğin, en az 720dp yüksekliğini cihazları hedeflemek için niteleyici h720dp kullanılır

- **Ekran boyutu** &ndash; bu niteleyici ekran boyutunun bu kaynaklar içindir Genelleştirme olduğu. Daha ayrıntılı olarak ele [değişen ekranlar oluşturma kaynakları](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md). 
  Olası değerler şunlardır: `small`, `normal`, `large`, ve `xlarge`. API düzeyi 9 (Android 2.3/Android 2.3.1/Android 2.3.2) eklendi

- **Ekran en boy** &ndash; bu ekran yönünü en boy oranını temel alır. Uzun bir ekran daha geniştir. API düzey 4 (Android 1.6) eklendi. Olası değerler şunlardır: uzun ve notlong.

- **Ekran yönünü** &ndash; dikey veya yatay ekran yönünü. Bu uygulamanın süresi boyunca değiştirebilirsiniz.
  Olası değerler şunlardır: `port` ve `land`.

- **Mod yerleştirme** &ndash; bir araba cihazları sabitleme ya da bir Masası sabitlemek için. API düzeyi 8 (Android 2.2.x) eklendi. Olası değerler şunlardır: `car` ve `desk`.

- **Gece modu** &ndash; gece veya günün karşılamadığını uygulaması çalışıyor. Bu uygulamanın ömrü boyunca değiştirebilir ve geliştiricilerin gece arabirim koyu sürümlerini kullanan olanağı vermek için tasarlanmıştır. API düzeyi 8 (Android 2.2.x) eklendi. Olası değerler şunlardır: `night` ve `notnight`.

- **Ekran piksel yoğunluğu (DPI)** &ndash; fiziksel ekranında belirli bir alanında piksel sayısı. Genellikle nokta / inç (dpi) olarak ifade edilir. Olası değerler şunlardır:

    - `ldpi` &ndash; Düşük yoğunluğu ekranlar.

    - `mdpi` &ndash; Orta yoğunluk ekranlar

    - `hdpi` &ndash; Yüksek yoğunluklu ekranlar

    - `xhdpi` &ndash; Çok yüksek yoğunluklu ekranlar

    - `nodpi` &ndash; Değil ölçeklendirilmesi olan kaynaklar

    - `tvdpi` &ndash; Mdpi hdpi arasındaki API düzeyinde 13 (Android 3.2) ekranlar için kullanıma sunuldu.

- **Dokunmatik ekran türü** &ndash; bir aygıt sahip dokunmatik türünü belirtir. Olası değerler şunlardır: `notouch` (hiçbir dokunmatik ekran) `stylus` (resistive dokunmatik kalem için uygun), ve `finger` (dokunmatik).

- **Klavye kullanılabilirlik** &ndash; ne tür bir klavye kullanılabilir olduğunu belirtir. Bu uygulamanın süresi boyunca değişebilir &ndash; örneğin kullanıcı açtığında klavye donanımı. Olası değerler şunlardır:

    - `keysexposed` &ndash; Aygıtın kullanılabilir klavye vardır. Etkin yazılım klavye ise, donanım klavye açıldığında, ardından bu yalnızca kullanılır.

    - `keyshidden` &ndash; Cihaz donanım klavye sahip ancak gizli olduğu ve yazılım klavye etkindir.

    - `keyssoft` &ndash; Aygıt etkin bir yazılım klavye sahiptir.

- **Birincil metin giriş yöntemi** &ndash; ne tür donanımı anahtarlar için giriş kullanılabilir olduğunu belirtmek için kullanın. Olası değerler şunlardır:

    - `nokeys` &ndash; Giriş için hiçbir donanım anahtar vardır.

    - `qwerty` &ndash; Qwerty klavye kullanılabilir.

    - `12key` &ndash; Klavye 12 anahtar donanımı yok


- **Gezinti anahtar kullanılabilirlik** &ndash; 5-yönlü veya d-Pad'i (pad yönlü) gezinti kullanılabilir olduğunda için. Bu, uygulamanızın ömrü boyunca değiştirebilirsiniz. Olası değerler şunlardır:

    - `navexposed` &ndash; gezinti tuşları kullanıcı tarafından kullanılabilir

    - `navhidden` &ndash; gezinti tuşları kullanılabilir değil.

-  **Birincil olmayan dokunma Gezinti yöntemi** &ndash; cihazda kullanılabilir Gezinti türü. Olası değerler şunlardır:

    - `nonav` &ndash; yalnızca Gezinti kullanılabilir dokunmatik ekran özelliğidir

    - `dpad` &ndash; Gezinti için d-Pad'i (pad yönlü) kullanılabilir

    - `trackball` &ndash; Gezinti için topu aygıtın

    - `wheel` &ndash; Burada bir veya daha çok yönlü kullanılabilir Tekerlek seyrek senaryosu

-  **Platform sürümü (API düzeyi)** &ndash; v biçiminde aygıt tarafından desteklenen API düzeyi*N*, burada *N* hedeflenen API düzeyi. Örneğin, bir API düzeyi 11 (Android 3.0) v11 hedeflediğini aygıt.


Niteleyiciler kaynak hakkında daha ayrıntılı bilgi için bkz: [sağlama kaynakları](http://developer.android.com/guide/topics/resources/providing-resources.html) Android geliştiricileri Web sitesinde.


## <a name="how-android-determines-what-resources-to-use"></a>Android kullanım için hangi kaynakların nasıl belirler

Çok olası ve büyük olasılıkla bir Android uygulamasını birçok kaynağa içerecektir. Bir cihazda çalıştırıldığında, Android uygulama kaynaklarını nasıl seçer anlamak önemlidir.

Android temel kaynakları kuralları aşağıdaki test yineleme tarafından belirler:

- **Çelişkili niteleyicileri ortadan** &ndash; Örneğin, cihaz yönlendirmesini dikey ise, ardından tüm yatay kaynak dizinleri reddedilir.

- **Desteklenmeyen niteleyicileri Yoksay** &ndash; tüm niteleyicileri tüm API düzeyi için kullanılabilir. Bir kaynak dizin aygıt tarafından desteklenmeyen bir niteleyici içeriyorsa, bu kaynak dizini yoksayılacak.

- **Sonraki en yüksek öncelik niteleyici tanımlamak** &ndash; yukarıdaki tabloya başvuran sonraki en yüksek öncelik niteleyicisi (yukarıdan aşağıya) seçin.

- **Niteleyici için herhangi bir kaynak dizin tutmak** &ndash; yukarıdaki tabloya niteleyici eşleşen herhangi bir kaynak dizin ise sonraki en yüksek öncelik niteleyicisi (yukarıdan aşağıya) seçin.

Bu kurallar, aynı zamanda aşağıdaki akış şemasında gösterilmiştir:

[![Kaynakları akış çizelgesi](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

Sistemin yoğunluk özgü kaynakları arayan ve bunları bulunamıyor yoğunluğu belirli kaynaklar bulun ve bunları ölçeklendirmek deneyecek. Android varsayılan kaynakları kullanamazsınız.
Örneğin, düşük yoğunluklu bir kaynak ve onu arayan kullanılabilir olmadığı durumlarda, Android varsayılan veya Orta yoğunluklu kaynakları kaynak yoğunluklu sürümü seçebilirsiniz. Yüksek yoğunluklu kaynak 0,75 faktörü gerektiren bir orta yoğunluklu kaynak Ölçeklendirmesi daha az görünürlük sorunları sonuçlanır 0,5 faktörüyle Genişletilebilir çünkü bunu yapar.

Örnek olarak, aşağıdaki drawable kaynak dizinleri olan bir uygulama göz önünde bulundurun:

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

Ve şimdi aşağıdaki yapılandırma ile bir cihazda uygulamayı çalıştırın:

- **Yerel ayar** &ndash; tr GB
- **Yönlendirme** &ndash; bağlantı noktası
- **Ekran yoğunluğu** &ndash; hdpi
- **Dokunmatik ekran türü** &ndash; notouch
- **Birincil giriş yöntemi** &ndash; 12key

Yerel ayar kimliği ile çakışan gibi Fransızca kaynakları başından itibaren ortadan kalkar `en-GB`, bizimle bırakır:

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

İlk niteleyici yukarıdaki niteleyicileri tablodan sonraki, seçili: MCC ve MNC. Bu niteleyici MCC/MNC kodu göz ardı edilir şekilde içeren kaynak dizin yok vardır.

Dil olduğu sonraki niteleyici seçilir. Dil kodu eşleşen kaynak yok. Dil kodu eşleşmeyen tüm kaynak dizinleri `en` reddedilir, böylece kaynaklar listesi sunulmuştur:

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

Varsa sonraki niteleyici ekran yönünü ekran yönünü eşleşmeyen tüm kaynak dizinlerini olduğu `port` ortadan kaldırılır:

    drawable-en-port
    drawable-en-port-ldpi

Sonraki ekranda yoğunluğu niteleyicisi olduğu `ldpi`, daha fazla kaynak dizini bir dışlama sonuçları:

    drawable-en-port-ldpi

Bu işlem sonucunda, Android kaynak dizinde drawable kaynakları kullanır `drawable-en-port-ldpi` cihaz için.

> [!NOTE]
> Bu seçim işlemi bir özel durum ekran boyutu niteleyicileri sağlar. Geçerli cihaz sağlar daha küçük bir ekran için tasarlanmış kaynak seçmek Android mümkündür. Örneğin, büyük ekran cihaz kaynakları kullanabilir için normal bir boyutta ekran sağlayın. Ancak bunun tersi geçerli değildir: aynı büyük ekran aygıt xlarge ekran için sağlanan kaynakları kullanmaz. Android verilen ekran boyutu eşleşen bir kaynak kümesi bulamazsanız, uygulama kilitleniyor.
