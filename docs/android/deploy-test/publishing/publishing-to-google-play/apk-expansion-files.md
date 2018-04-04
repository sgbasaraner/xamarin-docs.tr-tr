---
title: APK genişletme dosyaları
ms.prod: xamarin
ms.assetid: DB7E38E8-3C4E-5191-27EA-22DE63044FE2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: f2ca86fb27b5bd4b6cb7ba855fbd0a2abfa1381d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="apk-expansion-files"></a>APK genişletme dosyaları

Bazı uygulamalar (bazı oyunları, örneğin) daha fazla kaynak ve Google Play tarafından uygulanan en fazla Android uygulaması boyut sınırı içinde sağlanan daha varlıklar gerektirir. Bu sınır, APK için hedeflenen Android sürümü bağlıdır:

-  Android 4.0 veya daha yüksek (API düzeyi 14 veya daha yüksek) hedef APKs için 100MB.
-  Android 3.2 hedef ya da (API düzeyi 13 veya daha yüksek) alt APKs için 50MB.

Bu sınırlamanın üstesinden gelmek için Google Play barındırmak ve iki dağıtın *genişletme dosyaları* dolaylı olarak bu sınırı aşarsanız uygulama izin verme bir APK birlikte gidin. 

Çoğu cihazlarda bir uygulama yüklendiğinde, genişleme dosya APK birlikte yüklenir ve paylaşılan depolama konumuna (SD kart veya USB monte bölüm) cihaz kaydedilir. Bazı eski cihazlarda genişletme dosyaları APK ile otomatik olarak yüklenmez. Bu durumlarda uygulamanın kullanıcı uygulamaları ilk kez çalıştırdığında, genişletme dosyaları indirir kodu içeren için gerekli değildir.

Genişleme dosya kabul edilir *donuk ikili BLOB'lar (obb)* ve 2 GB boyutunda olabilir. Android yüklendikten sonra bu dosyaları herhangi bir özel işlem gerçekleştirmez &ndash; dosyalar uygulama için uygun olan herhangi bir biçimdeki olabilir. Kavramsal olarak, genişletme dosyaları için önerilen yaklaşım şu şekildedir:

-   **Ana genişletme** &ndash; bu dosya kaynaklar ve APK boyut sınırı uymayan varlıklar için birincil genişletme dosyasıdır. Ana genişletme dosya, bir uygulama gerekiyor ve nadiren güncelleştirilmelidir birincil varlıkların içermelidir.
-   **Düzeltme eki genişletme** &ndash; bu ana genişletme dosya küçük güncelleştirmelerini yöneliktir. Bu dosya güncelleştirilebilir. Tüm gerekli düzeltme ekleri veya güncelleştirmeler bu dosyadan gerçekleştirmek için uygulamanın sorumluluğundadır.


APK karşıya gibi genişleme dosya aynı anda karşıya gerekir.
Google play varolan APK veya güncelleştirilmesi varolan APKs karşıya yüklenecek bir genişletme dosya izin vermiyor. Bir genişleme dosya güncelleştirmek için gereken sonra yeni APK ile yüklenmelidir `versionCode` güncelleştirildi.


## <a name="expansion-file-storage"></a>Genişleme dosya depolama

Dosyaları bir cihaza indirildiğinde, bunlar depolanır  **_paylaşılan deposu_/Android/obb/_paket adı_**:

-   **_Paylaşılan depolama_**  &ndash; tarafından belirtilen dizin budur `Android.OS.Environment.ExternalStorageDirectory` .
-   **_Paket adı_**  &ndash; bu uygulamanın Java stili paketinin adıdır.


Yüklendikten sonra genişleme dosya değil taşınamaz, değiştirilmiş, yeniden adlandırılmış veya cihazdaki konumlarından silinmiş. Bunu yapmak için yeniden yüklenmek üzere genişletme dosyaları neden olur ve eski dosyalar silinir. Ayrıca, genişleme dosya dizin yalnızca genişletme paketi dosyalarını içermelidir.

Genişletme dosyaları sunar hiçbir güvenlik veya içeriklerini çevresindeki korumayı &ndash; paylaşılan depolama kaydedilmiş tüm dosyalar diğer uygulamalar veya kullanıcılar erişebilir.

Bir genişletme dosyasının paketini açma gerekliyse paketi çözülen dosyaları birinde gibi ayrı bir dizinde saklanmalıdır `Android.OS.Environment.ExternalStorageDirectory`.

Bir genişletme dosyasından dosyalar ayıklanıyor varlıklarına ya da kaynaklarına doğrudan genişletme dosyasını okumaya alternatiftir. Genişleme dosya uygun bir kullanılabilir bir zip dosyası'den fazla bir şey olduğunu `ContentProvider`. [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary) bir derlemeyi içeren [System.IO.Compression.Zip](https://github.com/mattleibow/Android.Play.ExpansionLibrary/tree/master/System.IO.Compression.Zip), içeren bir `ContentProvider` bazı medya dosyaları doğrudan dosya erişimi için izin. Bir zip dosyasına medya dosyalarını paketlenmiş, medya kayıttan yürütme çağrıları doğrudan zip dosyaları zip dosyasının paketini açma gerek kalmadan kullanabilir. Ortam dosyaları zip dosyasını eklendiğinde sıkıştırılmasının gerekli değildir. 


### <a name="filename-format"></a>Dosya biçimi

Genişletme dosyaları indirildiğinde, Google Play genişletme adlandırmak için aşağıdaki düzenini kullanır:

    [main|patch].<expansion-version>.<package-name>.obb

Bu düzen üç bileşeni vardır:

-   `main` veya `patch` &ndash; bu bu ana olup olmadığını belirtir veya düzeltme eki genişletme dosya. Yalnızca bir olabilir her.
-   `<expansion-version>` &ndash; Bu eşleşen bir tamsayıdır `versionCode` APK dosyanın ilk ilişkili.
-   `<package-name>` &ndash; Bu uygulamanın Java stili paketinin adıdır.


Örneğin 21 APK sürümüdür ve bir paket adı, `mono.samples.helloworld`, ana genişletme dosya adlı **main.21.mono.samples.helloworld**.


## <a name="download-process"></a>Karşıdan yükleme işlemi

Bir uygulama Google Play'den yüklendiğinde genişletme dosyaları indirilip APK birlikte kaydedilen. Belirli durumlarda bu değil oluşabilir veya genişleme dosya silinmiş. Bu durum işlemek için gerekirse genişletme dosyaların mevcut olduğunu ve bunları karşıdan olup olmadığını görmek bir uygulama gerekir. Aşağıdaki akış çizelgesi, bu işlemin önerilen iş akışı görüntüler:

[![APK genişletme akış çizelgesi](apk-expansion-files-images/apkexpansion.png)](apk-expansion-files-images/apkexpansion.png#lightbox)

Bir uygulama başlatıldığında uygun genişletme dosyaları geçerli aygıtta olup olmadığını görmek için kontrol etmeniz gerekir. Yeterli değillerse sonra uygulama Google Play'den ait bir istek olmalısınız [uygulama lisans](http://developer.android.com/google/play/licensing/index.html). Bu onay kullanılarak yapılan *lisans doğrulama kitaplığı (LVL)*ve ücretsiz ve lisanslı uygulamalar için yapılması gerekir. LVL öncelikle lisans kısıtlamaları zorlamak için ücretli uygulamalar tarafından kullanılır. Ancak, böylece de genişletme kitaplıklarıyla kullanılabilir Google LVL genişletmiştir. Ücretsiz uygulamalar LVL denetimi gerçekleştirmek sahip, ancak lisans kısıtlamaları göz ardı edebilirsiniz. LVL isteği uygulama gerektiriyor genişletme dosyalar hakkında aşağıdaki bilgileri sağlamaktan sorumludur: 

-   **Dosya boyutu** &ndash; genişletme dosyaların dosya boyutları doğru genişletme dosyalarının zaten indirilip indirilmediğini olup olmadığını belirler onay bir parçası olarak kullanılır.
-   **Dosya adları** &ndash; genişletme paketleri hangi kaydedilmelidir için dosya adı (geçerli cihazda) budur.
-   **İndirme URL'sini** &ndash; genişletme paketleri yüklemek için kullanılacak URL. Bu her yükleme için benzersiz ve sağlanan kısa bir süre sonra dolacak.


LVL onay gerçekleştirildikten sonra uygulamayı indirme bir parçası olarak aşağıdaki noktaları göz önünde bulundurarak genişletme dosyaları karşıdan:

-  Cihaz genişletme dosyaları depolamak için yeterli alan olmayabilir.
-  Wi-Fi kullanılabilir durumda değilse, sonra kullanıcı duraklatmak veya istenmeyen verileri ücret oluşmasını önlemek için yüklemeyi iptal edin izin verileceğini.
-  Genişletme dosyalar kullanıcı etkileşimleri engelleme önlemek amacıyla arka planda indirilir.
-  Yükleme arka planda gerçekleştirildiği sırada bir İlerleme göstergesi görüntülenmesi gerekir.
-  Karşıdan yükleme sırasında oluşan hataları düzgün biçimde işlenmiş ve kurtarılabilir.



## <a name="architectural-overview"></a>Mimari genel bakış

Ana etkinlik başladığında, genişletme dosyaları indirilir görmek için denetler. Dosyaları yüklediyseniz, geçerlilik için denetlenmelidir.

Genişleme dosya yüklenmedi veya geçerli dosyaları geçersiz gerekiyorsa, yeni genişletme dosyaları yüklenmelidir. Sınırlanmış bir hizmet uygulaması bir parçası olarak oluşturulur. Uygulamanın ana etkinlik başlatıldığında, genişleme dosya adları ve dosyaları indirmek için URL öğrenmek için Google lisans Hizmetleri karşı bir denetimi gerçekleştirmek için sınırlı hizmeti kullanır. Sınırlanmış hizmet ardından arka plan iş parçacığında dosyaları indirir.

Bir uygulamaya genişletme dosyalarını tümleştirme için gereken çaba kolaylaştırmak için Google birkaç kitaplıkları Java'da oluşturulur. Söz konusu kitaplıkları şunlardır:

-   **Yükleyici Kitaplığı** &ndash; bu uygulamada genişletme dosyalarını tümleştirme için gereken çaba azaltan bir kitaplıktır. Kitaplığı bir arka plan hizmeti genişletme dosyalarında yükleme, kullanıcı bildirimlerinin görüntülenip, ağ bağlantısı sorunları ele, sürdürme indirmeler, vb.
-   **Lisans doğrulama kitaplığı (LVL)** &ndash; yapma ve Hizmetleri uygulama lisansı çağrıları işlemek için bir kitaplık. Ayrıca, uygulama cihaz üzerinde kullanım için yetkili olup olmadığını görmek için lisans denetimleri gerçekleştirmek için de kullanılabilir.
-   **APK genişletme Zip kitaplığı (isteğe bağlı)** &ndash; genişletme dosyaları zip dosyasında, bu kitaplığa içerik sağlayıcısı olarak hareket ve zip genişletin gerek kalmadan doğrudan zip dosyasından kaynakları ve varlıkları okumak bir uygulama izin verme dosya.


Bu kitaplıklar, C# için bağlantı noktası kurulmuş ve Apache 2.0 lisansı altında kullanılabilir. Bu kitaplıklar, hızlı bir şekilde genişleme dosya varolan bir uygulamaya tümleştirmek için varolan bir Xamarin.Android uygulaması eklenebilir. Kod şu adresten edinilebilir [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary) github'da.
