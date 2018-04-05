---
title: Android hata ayıklama günlüğü
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/04/2018
ms.openlocfilehash: e0e22fe35dc5042a7b3c895a250803e936611629
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="android-debug-log"></a>Android hata ayıklama günlüğü

Kendi uygulamalarında hata ayıklama için yaygın eli geliştiriciler kullanın biridir çağrı yapmak için `Console.WriteLine`. Ancak, Android gibi mobil platformda yoktur konsol yok. Android cihazları, uygulamaları yazarken kullanabileceğiniz bir günlük sağlar. Bu bazen olarak adlandırılır _logcat_ nedeniyle bunu almak için yazdığınız komutu. Kullanım **hata ayıklama günlüğünü** günlüğe kaydedilen verileri görüntülemek için aracı.

## <a name="android-debug-log-overview"></a>Android hata ayıklama günlüğü'ne genel bakış

**Hata ayıklama günlüğünü** aracı Visual Studio aracılığıyla uygulama hata ayıklama sırasında günlük çıkışı görüntülemek için bir yol sağlar. Hata ayıklama günlüğü aşağıdaki cihazları destekler:

-   Fiziksel Android telefonlar, tabletler ve wearables.
-   Google Android öykünücüsü üzerinde çalışan bir Android sanal cihazı. 

> [!NOTE]
> **Hata ayıklama günlüğünü** aracı Xamarin Canlı Player ile çalışmaz.

**Hata ayıklama günlüğünü** uygulama cihaz üzerinde tek başına çalışırken, oluşturulan günlük iletilerini görüntülemez (yani, bunu Visual Studio'dan değilken).


## <a name="accessing-the-debug-log-from-visual-studio"></a>Hata ayıklama günlüğü Visual Studio'dan erişme

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Açmak için **aygıt günlük** aracı, tıklatın **aygıt günlük (logcat)** araç çubuğundaki simgeye:

[![Cihaz günlüğü aracı araç çubuğundaki konumu](android-debug-log-images/vswin-01-logcat-sml.png)](android-debug-log-images/vswin-01-logcat.png#lightbox)

Alternatif olarak, başlatma **aygıt günlük** aracı aşağıdaki menü seçimlerini birinden:

-   **Görünüm > Diğer Pencereler > cihaz günlük**
-   **Araçlar > Android > cihaz günlük**

Aşağıdaki ekran görüntüsünde çeşitli kısımlarını gösterilmektedir **hata ayıklama aracını** penceresi:

[![Hata ayıklama araç penceresi bölümleri](android-debug-log-images/vswin-03-features-sml.png)](android-debug-log-images/vswin-03-features.png#lightbox)

-   **Cihaz Seçici** &ndash; hangi fiziksel aygıt veya izlemek için çalışan öykünücünüze seçer.

-   **Günlük girişleri** &ndash; logcat günlük iletileri tablosu.

-   **Günlük girişleri Temizle** &ndash; tablodan tüm geçerli günlük girişlerini temizler.

-   **Yürüt/Duraklat** &ndash; güncelleştirmek veya yeni günlük girişlerini görüntüleme duraklatma arasında geçiş yapar.

-   **Durdur** &ndash; yeni günlük girişlerini görüntülemeyi durdurur.

-   **Arama kutusu** &ndash; Enter arama dizelerini bir alt kümesini filtrelemek için bu kutuya günlük girişlerini.


Zaman **hata ayıklama günlüğünü** araç penceresi görüntülenir, izlemek için Android cihaz seçmek için aygıt aşağı açılır menüyü kullanın:

[![Cihaz Seçici konumu](android-debug-log-images/vswin-02-devices-combo-sml.png)](android-debug-log-images/vswin-02-devices-combo.png#lightbox)

Cihaz seçtikten sonra **aygıt günlük** aracı otomatik olarak çalışan bir uygulamadan günlük girişlerini ekler &ndash; bu günlüğü girişleri gösterilir günlük girişlerini tablo. Cihazlar arasında geçiş yapma durdurur ve cihaz günlüğü başlar. Tüm cihazlarda cihaz seçicide görünmesi için önce bir Android projesi yüklenmesi gerektiğini unutmayın. Aygıtın aygıt Seçici içinde görünmüyorsa, yanına Visual Studio aygıt açılır menüde kullanılabilir olduğunu doğrulamak **Başlat** düğmesi.


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Açmak için **aygıt günlük**, tıklatın **Görünüm > klavye takımı > cihaz günlük**:

[![Cihaz günlük menü öğesi konumu](android-debug-log-images/vsmac-01-logcat-sml.png)](android-debug-log-images/vsmac-01-logcat.png#lightbox)

Aşağıdaki ekran görüntüsünde çeşitli kısımlarını gösterilmektedir **hata ayıklama aracını** penceresi:

[![Hata ayıklama araç penceresi özellikleri](android-debug-log-images/vsmac-03-features-sml.png)](android-debug-log-images/vsmac-03-features.png#lightbox)

-   **Cihaz Seçici** &ndash; hangi fiziksel aygıt veya izlemek için çalışan öykünücünüze seçer.

-   **Günlük girişleri** &ndash; logcat günlük iletileri tablosu.

-   **Günlük girişleri Temizle** &ndash; tablodan tüm geçerli günlük girişlerini temizler.

-   **Arama kutusu** &ndash; Enter arama dizelerini bir alt kümesini filtrelemek için bu kutuya günlük girişlerini.

-   **İletileri göster** &ndash; bilgilendirici iletileri görünümünü değiştirir.

-   **Uyarıları göster** &ndash; (uyarı iletilerini sarı renkte gösterilir) uyarı iletilerini görünümünü değiştirir.

-   **Hataları göster** &ndash; (uyarı iletilerini kırmızı olarak gösterilir) hata iletileri görünümünü değiştirir.

-   **Yeniden** &ndash; aygıta yeniden bağlanır ve günlük girişi görüntü yeniler.

-   **İşaret eklemek** &ndash; işaret iletiye ekler (gibi `--- Marker N ---`) en son günlük girişi sonra burada _N_ yeni işaretçileri eklendikçe, 1 ve artışlarla 1 ile başlayan bir sayacıdır.

Hata ayıklama günlüğünü araç penceresi görüntülendiğinde, aygıt açılır menü izlemek için Android cihaz seçmek için kullanın:

[![Cihaz Seçici konumu](android-debug-log-images/vsmac-02-devices-combo-sml.png)](android-debug-log-images/vsmac-02-devices-combo.png#lightbox)

Cihaz seçtikten sonra **aygıt günlük** aracı otomatik olarak çalışan bir uygulamadan günlük girişlerini ekler &ndash; bu günlüğü girişleri gösterilir günlük girişlerini tablo. Cihazlar arasında geçiş yapma durdurur ve cihaz günlüğü başlar. Tüm cihazlarda cihaz seçicide görünmesi için önce bir Android projesi yüklenmesi gerektiğini unutmayın. Aygıtın aygıt Seçici içinde görünmüyorsa, yanına Visual Studio aygıt açılır menüde kullanılabilir olduğunu doğrulamak **Başlat** düğmesi.

-----


## <a name="accessing-from-the-command-line"></a>Komut satırından erişme

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Komut satırı aracılığıyla hata ayıklama günlüğünü görüntülemek için başka bir seçenek değil. Bir komut istemi penceresi açın ve Android SDK platformunuzun Araçlar klasöre gidin (genellikle, SDK platformunuzun Araçlar klasöründen bulunur **C:\\Program Files (x86)\\Android\\android sdk\\ Platform araçlarını**).

Yalnızca tek bir cihazı (fiziksel cihaz veya öykünücü) bağlı, günlük aşağıdaki komutu girerek görüntülenebilir:

```shell
$ adb logcat
```

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Komut satırı aracılığıyla hata ayıklama günlüğünü görüntülemek için başka bir seçenek değil. Bir Terminal penceresi açın ve Android SDK platformunuzun Araçlar klasöre gidin (genellikle, SDK platformunuzun Araçlar klasöründen bulunur **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/platform-tools**).

Yalnızca tek bir cihazı (fiziksel cihaz veya öykünücü) bağlı, günlük aşağıdaki komutu girerek görüntülenebilir:

```shell
$ ./adb logcat
```

-----


Birden çok aygıt bağlıysa, aygıt açıkça tanımlanmalıdır. Örneğin **adb -d logcat** yalnızca fiziksel cihazın günlük görüntüler bağlı, ancak **adb -e logcat** çalıştıran yalnızca öykünücüsü günlüğünü gösterir.

Daha fazla komutları girerek bulunabilir **adb** ve Yardım iletileri okumak.


## <a name="writing-to-the-debug-log"></a>Hata ayıklama günlüğüne yazma

İletileri yazılabilir **hata ayıklama günlüğünü** yöntemlerini kullanarak [Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) sınıfı.
Örneğin: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

Bu, aşağıdakine benzer bir çıktı üretir:

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

Kullanmak da mümkündür `Console.WriteLine` yazmak için **hata ayıklama günlüğünü** &ndash; logcat (Bu teknik olduğunda özellikle yararlı hata ayıklama Xamarin.Forms uygulamaları biraz farklı çıktı biçimi ile bu iletiler görünür Android):

```csharp
System.Console.WriteLine ("DEBUG - Button Clicked!");
```

Bu logcat aşağıdakine benzer bir çıktı üretir:

```
Info (19543) / mono-stdout: DEBUG - Button Clicked!
```

## <a name="interesting-messages"></a>İlginç iletileri

Günlük (ve özellikle sağlama oturum açtıklarında parçacıkları başkalarına) okunurken tamamının günlük dosyasında harcadığı çok sıkıcı görülür.
Günlük iletileri üzerinden gidin, aşağıdakine benzer bir günlük girişi için arayarak başlamanız daha kolay hale getirmek için:

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

Özellikle de uygulama paketinin adını içeren normal ifadeyle eşleşen bir satır için bakın:

```shell
^I.*ActivityManager.*Starting: Intent
```

Bu bir etkinlik başlangıç için karşılık gelen çizgidir ve *çoğu* (ancak tüm) aşağıdaki iletilerinin uygulamaya ilişkili olmalıdır.

Her ileti iletinin oluşturma işleminin işlem tanımlayıcısı (PID) içerdiğine dikkat edin. Yukarıdaki içinde `ActivityManager` iletisi, işlem `12944` ileti oluşturdu. Ayıklanacak uygulamasının işlemi olan işlemi belirlemek için Ara **mono. MonoRuntimeProvider** ileti: 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

Bu ileti başlatıldığından işleminden gelir. Bu PID içeren tüm sonraki iletiler için aynı işlemini gelir.
