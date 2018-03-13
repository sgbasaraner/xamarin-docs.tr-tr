---
title: "Parçalar"
description: "Android 3.0 parçaları, daha esnek tasarımları telefonlar ve tabletler bulunan çok sayıda farklı ekran boyutlarına desteklemek nasıl gösteren sunmuştur. Bu makalede parçaları Xamarin.Android uygulamaları geliştirmek için nasıl kullanılacağı ve aynı zamanda önceden Android 3.0 (API düzeyi 11) cihazlarda parçaları desteklemek nasıl ele alınacaktır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 8595ecb63e49a4768120e98f41826b74c2dd43e4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="fragments"></a>Parçalar

_Android 3.0 parçaları, daha esnek tasarımları telefonlar ve tabletler bulunan çok sayıda farklı ekran boyutlarına desteklemek nasıl gösteren sunmuştur. Bu makalede parçaları Xamarin.Android uygulamaları geliştirmek için nasıl kullanılacağı ve aynı zamanda önceden Android 3.0 (API düzeyi 11) cihazlarda parçaları desteklemek nasıl ele alınacaktır._

## <a name="fragments-overview"></a>Parçaları genel bakış

Çoğu tabletler üzerinde bulunan büyük ekran boyutlarına karmaşıklığını fazladan bir katmanı Android geliştirme için eklenen — küçük ekran mutlaka de daha büyük ekranlar ve tam tersini çalışmaz için tasarlanmış bir düzeni. Bu sunulan zorluklar sayısını azaltmak için iki yeni özellik, Android 3.0 eklenen *parçaları* ve *destek paketleri*.

Parçaları, kullanıcı arabirimi modülleri düşünülebilir. Bunlar, kullanıcı arabiriminin ayrı etkinlikleri çalıştırılabilir yalıtılmış, yeniden kullanılabilir parçalara bölmek Geliştirici olanak tanır. Çalışma zamanında etkinlikleri kullanmak için hangi parçaları karar verir.

Destek paketleri ilk olarak adlı *uyumluluk kitaplıkları* ve Android 3.0 (API düzeyi 11) önce Android sürümlerini çalıştıran cihazlarda kullanılacak parçaları izin verilir.

Örneğin, aşağıdaki resimde nasıl tek bir uygulama parçaları arasında değişen aygıt form faktörleri kullanır gösterilmektedir.

[![Parçaları tabletler ve ahizeleri nasıl kullanıldığını diyagramı](images/00.png)](images/00.png#lightbox)

*Parça A* bir listesini içerir ancak *parça B* bu listede seçilen bir öğeye ilişkin ayrıntıları içerir. Tablet uygulamayı çalıştırdığınızda, her iki parçada aynı faaliyete görüntüleyebilirsiniz. (Küçük, ekran boyutu ile) ahize üzerinde aynı uygulamayı çalıştırdığınızda, parçaları iki ayrı etkinlikleri barındırılır. Parça A ve parça B hem form faktörleri aynı olan ancak onlara konağı etkinlikleri farklıdır.

Aktivite koordine etmek ve bu parçasının yönetmek yardımcı olması için Android adlı yeni bir sınıf sunulan *FragmentManager*. Her etkinlik kendi örneğine sahip bir `FragmentManager` eklemek için silme ve bulma parçaları barındırılan. Aşağıdaki diyagram, parça ve etkinlikleri arasındaki ilişkiyi gösterir:

[![Etkinlik, parça Yöneticisi ve parçaları arasındaki ilişkileri gösteren diyagram](images/01.png)](images/01.png#lightbox)

Bazı bakımından parçaları, bileşik denetimler veya mini etkinlikler olarak düşünülebilir. UI parçalarını sonra bağımsız olarak etkinliklerde geliştiriciler tarafından kullanılabilecek yeniden kullanılabilir modüllere yukarı paket. Bir parça hiyerarşisini görüntüleme sahip — bir etkinlik'olduğu gibi — ancak aktivite, ekranları arasında paylaşılabilir. Kendi ömrü parçaları sahip görünümleri parçaları farklı; görünümleri yoktur.

Etkinlik bir veya daha fazla parça konağa olsa da, parçaları doğrudan uyumlu değil. Benzer şekilde, parçaları doğrudan barındırma etkinliğinde diğer parçalarından tanımaz. Parçaları ve etkinlikleri ancak, farkında `FragmentManager` kendi etkinliğinde. Kullanarak `FragmentManager`, bir etkinlik veya bir parçasını parçanın belirli bir örneğine başvuru elde edin ve ardından bu örneğinde yöntemleri çağırmak için mümkündür. Bu şekilde, etkinlik veya parçaları iletişim kurabilmesini ve diğer parçalarından ile etkileşim.

Bu kılavuz dahil olmak üzere parçaları kullanma hakkında kapsamlı bilgi içerir:

-   **Parçaları oluşturma** – temel bir parçası ve uygulanmalı anahtar yöntemleri oluşturma.
-   **Yönetim ve işlemler parçalara** – nasıl çalışma zamanında parçaları yönetileceğini.
-   **Android destek paketi** – Android eski sürümleri kullanılacak izin kitaplıkları kullanmak için parça nasıl.


## <a name="requirements"></a>Gereksinimler

Aşağıdaki ekran görüntüsünde gösterildiği gibi parçalar Android API düzeyi 11 (Android 3.0) ile başlayan SDK'sı mevcuttur:

[![Android SDK Yöneticisi'nde API düzeyi seçme](images/02.png)](images/02.png#lightbox)

Parçaları kullanılabilir Xamarin.Android 4.0 ve üzeri. Bir Xamarin.Android uygulaması en az hedeflemelidir API düzeyi 11 (Android 3.0) veya parçaları kullanmak için daha yüksek. Hedef Framework'ü, aşağıda gösterildiği gibi proje seçenekleri ayarlanabilir:

[![Proje seçenekleri hedef Framework API düzeyini ayarlama](images/03.png)](images/03.png#lightbox)

Android destek paketi ve Xamarin.Android 4.2 kullanarak Android ya da daha yüksek eski sürümlerinde parçaları kullanmak da mümkündür. Bunun nasıl yapılacağı, bu bölümde belgelerde daha ayrıntılı ele alınmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Bal peteği Galerisi (örnek)](https://developer.xamarin.com/samples/monodroid/HoneycombGallery)
- [Parçalar](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Destek Paketi](http://developer.android.com/sdk/compatibility-library.html)
- [MOTODEV Web Semineri: Parçaları Tanıtımı](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
