---
title: "Android hata ayıklama günlüğü"
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 26ac42e4b7acbe19dee746130fc335fdf18ffc46
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="android-debug-log"></a>Android hata ayıklama günlüğü

## <a name="android-debug-log-overview"></a>Android hata ayıklama günlüğü'ne genel bakış

Kendi uygulamalarında hata ayıklama için yaygın eli geliştiriciler kullanın biridir çağrı yapmak için `Console.WriteLine`. Ancak, Android gibi mobil platformda yoktur konsol yok. Android cihazları, uygulamaları yazarken kullanabileceğiniz bir günlük sağlar. Bu bazen bunu almak için yazdığınız komutu nedeniyle 'logcat' olarak adlandırılır.

## <a name="accessing-from-visual-studio"></a>Visual Studio'dan erişme

Visual Studio 2015 ve üzeri Android ve iOS günlük klavye takımı birleştirilmiş.

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 & 2017

Visual Studio için yeni cihaz günlük araç penceresi, Android ve iOS cihazlar için günlükleri gösterir. Aşağıdaki komutları yürüterek gösterilebilir: 

-   **Görünüm > Diğer Pencereler > cihaz günlük**
-   **Araçlar > Android > cihaz günlük**
-   **Android araç > cihaz günlük**

Araç penceresi görüntülendiğinde, fiziksel aygıt aygıtlar açılan kutusundan seçilebilir. Aygıt seçildiğinde, günlük girişlerini tablosundaki çalışan bir uygulama eklemek otomatik olarak başlar. Cihazlar arasında geçiş yapma durdurun ve cihaz günlüğü Başlat. Combobox içinde görünmesi cihazlar için sırayla bir Android projesi yüklenmesi gerekir. Cihaz combobox içinde görünmüyorsa, ilk başlatma hata ayıklama açılır olup olmadığını denetleyin. 

Bu araç penceresi sağlar: günlük girişlerini, cihaz seçimi için açılan kutu, günlük girişlerini, arama kutusuna ve Dur/Yürüt/Duraklat düğmeleri temizlemek için bir yol tablosu. 



## <a name="accessing-from-the-command-line"></a>Komut satırından erişme

Hata ayıklama günlüğünü görüntülemek için başka bir komut satırı seçenektir. Bir konsol penceresi açın ve Android SDK platformunuzun Araçlar klasöre gidin (gibi **C:\android-sdk-windows\platform-tools**). 

Yalnızca bir aygıt bağlıysa, günlük ile görüntülenebilir:

```shell
$ adb logcat
```

Birden çok aygıt bağlıysa, aygıt tanımlanmalıdır. Örneğin `adb -d logcat` yalnızca fiziksel cihaz günlüğünü gösterir bağlı, ancak `adb -e logcat` çalıştıran yalnızca öykünücüsü günlüğünü gösterir. 

Daha fazla komut yalnızca çalıştırarak bulunabilir **adb**.



## <a name="writing-to-the-debug-log"></a>Hata ayıklama günlüğüne yazma

İletileri yazılabilir için hata ayıklama günlüğünü yöntemleri kullanarak [Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) sınıfı: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

Bu üretir:

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```


## <a name="interesting-messages"></a>İlginç iletileri

Günlük okunurken ve özellikle (olarak tam günlük dosyası (1) gereğinden fazla ve (2) okunamaz sağlamak), günlük parçacıkları başkalarına sağlanırken *en önemli* başlamak çizgidir bir satır benzeyen:

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

Normal ifadeyle eşleşen özellikle, bir satırı:

```shell
^I.*ActivityManager.*Starting: Intent
```

ve uygulama paketinin adını içeren. Bu bir etkinlik başlangıç için karşılık gelen çizgidir ve *çoğu* (ancak tüm) aşağıdaki iletilerinin uygulamaya ilişkili olmalıdır. 

Özellikle, her ileti iletinin oluşturma işleminin işlem tanımlayıcısını (PID) içerir. Yukarıdaki içinde `ActivityManager` iletisi, işlem `12944` ileti oluşturdu. Ayıklanacak uygulamasının işlemi olan işlemi belirlemek için mono bakın. MonoRuntimeProvider iletisi: 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

Bu ileti başlatıldığından işleminden gelen ve aynı işleminden aynı PID içeren tüm aşağıdaki iletileri gelir. 
