---
title: Android kaynakları
description: Bu makalede Xamarin.Android Android kaynaklarında kavramını tanıtır ve bunları nasıl kullanacağınızı belge. Kaynaklarının Android uygulamanızda uygulama yerelleştirme ve değişen ekran boyutlarına ve densities dahil olmak üzere birden çok aygıt desteklemek için nasıl kullanılacağını kapsar.
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 7b6ba9cdc222019bfa2e1cb9a61b54e290e69bba
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="android-resources"></a>Android kaynakları

_Bu makalede Xamarin.Android Android kaynaklarında kavramını tanıtır ve bunları nasıl kullanacağınızı belge. Kaynaklarının Android uygulamanızda uygulama yerelleştirme ve değişen ekran boyutlarına ve densities dahil olmak üzere birden çok aygıt desteklemek için nasıl kullanılacağını kapsar._


## <a name="overview"></a>Genel Bakış

Bir Android uygulamasını nadiren yalnızca kaynak kodu bulunur. Bir uygulamayı oluşturan diğer birçok dosyaları çoğunlukla vardır: video, görüntüler, yazı tipleri ve ses dosyaları yalnızca birkaçıdır. Toplu olarak, bu kaynak kodu olmayan dosyalar için kaynak olarak adlandırılır ve (kaynak kodu ile birlikte) oluşturma işlemi sırasında derlenir ve dağıtım ve aygıtları üzerine yükleme için bir APK olarak paketlenmiş:

![Paketleme diyagramı](images/packaging-diagram.png)

Kaynakları bir Android uygulaması için çok sayıda avantaj sağlar:

-  **Kod ayrımı** &ndash; görüntüleri, dizeleri, menüler, animasyonları, renkler, vb. kaynağı kaynak kodu ayırır. Bu nedenle kaynaklar önemli ölçüde yerelleştirilirken yardımcı olabilir.

-  **Birden çok aygıt hedef** &ndash; farklı cihaz yapılandırmaları kod değişiklikleri olmadan daha basit desteği sağlar.

-  **Derleme zamanı denetleme** &ndash; kaynakları statik ve uygulamaya derlenmiş. Bu zaman yakalamak ve çalışma zamanında daha zor bulmaya ve düzeltmek maliyetli olduğunda aksine hataları düzeltmek kolay olacak derleme zamanında denetlenmesi için kaynak kullanımını sağlar.

Yeni bir Xamarin.Android projesi başlatıldığında, bazı alt dizinleri ile birlikte kaynakları adlı özel bir dizin oluşturulur:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Kaynak klasörü ve içeriği](images/resources-folder-vs.png)

Yukarıdaki resimde uygulama kaynaklarını bu alt dizinlere kendi türüne göre düzenlenir: görüntüleri yazılacak **drawable** directory; Git görünümleri **düzeni** alt, vb.
 
# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Kaynak klasörü ve içeriği](images/resources-folder-xs.png)

Yukarıdaki resimde uygulama kaynaklarını bu alt dizinlere kendi türüne göre düzenlenir: görüntüleri yazılacak **mipmap** directory; Git görünümleri **düzeni** alt, vb.
 
-----

Bir Xamarin.Android uygulaması bu kaynaklara erişmek için iki yolu vardır: *program aracılığıyla* kodda ve *bildirimli olarak* özel bir XML sözdizimi kullanılarak XML.

Bu kaynaklar adlı *varsayılan kaynakları* ve daha belirli bir eşleşme belirtilmediği sürece tüm cihazlar tarafından kullanılır. Ayrıca, her kaynak türü isteğe bağlı olarak olabilir *alternatif kaynaklar* Android belirli cihazları hedeflemek için kullanabilir. Örneğin, kaynakları kullanıcının yerel ayarı, ekran boyutu, hedef için sağlanabilir veya aygıt 90 derece yatay olarak dikey olup olmadığını vb. Geliştirici tarafından fazladan kodlama herhangi çaba olmadan uygulama tarafından her bu gibi durumlarda, Android kullanım için kaynakları yükler.

Diğer kaynaklar olarak adlandırılan kısa bir dize ekleyerek belirtilen bir *niteleyici*, belirli bir kaynak türünü tutan dizinin sonuna.

Örneğin, **kaynakları/drawable-de** Almanca yerel ayar için ayarlanmış olan aygıtlar için görüntüleri belirleyeceksiniz sırada **kaynakları/drawable-fr** Fransızca yerel ayar olarak aygıtlar ayarlamak için görüntüleri tutmak. Diğer kaynaklar sağlayan bir örnek, aynı uygulama cihazını değiştirme, yalnızca yerel ayarıyla burada çalıştırılmakta olan aşağıdaki görüntüde görülebilir:

![Farklı yerel ayarlar için örnek ekranları](images/localized-screenshots.png)

Bu makalede, kaynakları kullanarak kapsamlı bir göz atalım ve aşağıdaki konuları içerir:

-  **Android kaynak Temelleri** &ndash; uygulamaya görüntüler ve yazı tipleri gibi kaynak türleri ekleme programlı ve bildirimli olarak, varsayılan kaynakları kullanarak.

-  **Cihaz belirli yapılandırmalar** &ndash; densities ve farklı ekran çözünürlükleri bir uygulamada destekleme.

-  **Yerelleştirme** &ndash; bir uygulama kullanılabilir farklı bölgelerde desteklemek için kaynakları kullanarak.


## <a name="related-links"></a>İlgili bağlantılar

- [Android Varlıklarını Kullanma](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [Uygulama Temelleri](http://developer.android.com/guide/topics/fundamentals.html)
- [Uygulama Kaynakları](http://developer.android.com/guide/topics/resources/index.html)
- [Birden çok ekran destekleme](http://developer.android.com/guide/practices/screens_support.html)
