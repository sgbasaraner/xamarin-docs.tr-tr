---
title: Xamarin.Android uygulaması temelleri
description: Uygulama kavramları
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: caa51fa0a70da1b799d56c706e6de974f61a14d9
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436436"
---
# <a name="xamarinandroid-application-fundamentals"></a>Xamarin.Android uygulaması temelleri

Bu bölümde bazı daha yaygın şeyler görevler veya geliştiricilerin Android uygulamaları geliştirirken bilmeniz gereken kavramlar bir kılavuz sağlar.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[Erişilebilirlik](~/android/app-fundamentals/accessibility.md)

Bu sayfayı göre uygulamalar oluşturmak için Android erişilebilirlik API'ları kullanmayı açıklar [erişilebilirlik denetim listesi](~/cross-platform/app-fundamentals/accessibility.md).

##  <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Android API düzeylerini anlama](~/android/app-fundamentals/android-api-levels.md)

Bu kılavuz API düzeylerini Android Android farklı sürümleri arasında uygulama uyumluluğu yönetmek için nasıl kullandığını açıklar ve bu API düzeyleri, uygulamanızda dağıtmak için Xamarin.Android projesi ayarlarının nasıl yapılandırılacağı açıklanmaktadır. Ayrıca, bu kılavuz farklı API düzeyleriyle ilgilenir çalışma zamanı kodunun nasıl yazılacağını açıklar ve tüm Android API düzeylerini, sürüm numaralarını (örneğin, Android 8.0), Android kodu adları (örneğin, Oreo) ve yapı sürüm kodlarını başvuru listesini sağlar.



##  <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Android kaynakları](~/android/app-fundamentals/resources-in-android/index.md)

Bu makalede bunları nasıl kullanacağınızı Xamarin.Android ve belgeleri Android kaynaklarında kavramını sunmaktadır. Kaynaklarının Android uygulamanızda uygulama yerelleştirme ve değişen ekran boyutlarına ve densities dahil olmak üzere birden çok aygıt desteklemek için nasıl kullanılacağını kapsar.




##  <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[Etkinlik Yaşam Döngüsü](~/android/app-fundamentals/activity-lifecycle/index.md)

Android uygulamaları temel yapı bloğu etkinliklerin olduğundan ve farklı durumlara sayısında bulunabilir. Etkinlik yaşam döngüsü örneklemesi ile başlar ve yok etme ile sona erer ve birçok durumları arasında içerir. Bir etkinlik durumu değiştiğinde, uygun yaşam döngüsü olay yöntemi, Yaklaşan durum değişikliği etkinliği bildiren ve bu değişikliği uyarlamak için kod yürütmesine izin veren adı verilir. Bu makalede etkinlikleri yaşam döngüsü inceler ve Sorumluluk açıklar bir etkinlik her iyi çalışan, güvenilir bir uygulamanın parçası olarak bu durum değişiklikleri sırasında vardır.

##  <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[Yerelleştirme](~/android/app-fundamentals/localization.md)

Bu makalede, bir Xamarin.Android diğer dillere dizeleri çevirme ve diğer görüntüleri sağlayarak yerelleştirme açıklanmaktadır.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Hizmetler](~/android/app-fundamentals/services/index.md)

Bu makalede arka planda yapılacak işleri izin Android bileşenleridir Android hizmetlerini kapsar. Hizmetleri için uygun farklı senaryoları açıklar ve bunların nasıl uygulanacağını uzak yordam çağrıları için bir arabirim sağlayacağı konusunda uzun süre çalışan arka plan görevleri gerçekleştirmek için her ikisini de gösterir.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[Yayın Alıcıları](~/android/app-fundamentals/broadcast-receivers.md)

Bu kılavuz oluşturmak ve yayın alıcıları kullanmak nasıl Xamarin.Android, sistem genelinde yayınları isteğe yanıt veren bir Android bileşeni kapsar.



##  <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[İzinler](~/android/app-fundamentals/permissions.md)

Mac için Visual Studio veya Visual Studio içinde yerleşik araç desteği oluşturmak ve Android derleme bildirimi için izinleri eklemek için kullanabilirsiniz. Bu belge, Visual Studio ve Xamarin Studio izinleri eklemeyi açıklar.



##  <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[Grafikler ve Animasyon](~/android/app-fundamentals/graphics-and-animation.md)

Android 2B grafik ve animasyonları desteklemek için çok zengin ve farklı bir çerçeve sağlar. Bu belgede, bu çerçeveleri tanıtır ve özel grafikler ve animasyonları oluşturmak ve bunları bir Xamarin.Android uygulaması açıklanmaktadır.


##  <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[CPU Mimarileri](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android 32-bit ve 64-bit aygıtlar dahil olmak üzere birkaç CPU mimariyi destekler. Bu makalede, bir uygulama bir veya daha fazla CPU Android desteklenen mimariler için hedef açıklanmaktadır.




##  <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[Döndürmeyi İşleme](~/android/app-fundamentals/handling-rotation.md)

Bu makalede Xamarin.Android cihaz yönlendirmesini değişiklikleri nasıl ele alınacağını açıklar. Program aracılığıyla yönlendirmesini işlemek için değişiklikleri nasıl belirli cihaz yönlendirmesini de için kaynakları otomatik olarak yüklemek için Android kaynak sistemiyle çalışacak şekilde nasıl kapsar. Ardından bir aygıtı döndürüldüğünde durumunu korumak için teknikleri açıklar.



##  <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android ses](~/android/app-fundamentals/android-audio.md)

Android işletim sistemi, ses ve video multimedya için kapsamlı destek sağlar. Bu kılavuz, Android ses odaklanır ve çalma ve yerleşik ses player ve Kaydedici sınıfları yanı sıra, düşük düzey ses API kullanarak ses kaydını kapsar. Böylece geliştiriciler iyi çalışan uygulamalar oluşturabilir diğer uygulamalar tarafından yayınlanan ses olaylarla çalışma da kapsar.




##  <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[Bildirimler](~/android/app-fundamentals/notifications/index.md)

Bu bölümde, yerel ve uzak bildirimleri içinde Xamarin.Android uygulamak açıklanmaktadır. Bir Android bildirim çeşitli kullanıcı Arabirimi öğelerini açıklar ve API anlatılmaktadır oluşturma ve bir bildirim görüntüleme ile söz konusu. Uzak bildirimleri için Google Cloud Messaging hem Firebase Cloud Messaging açıklanmıştır. Adım adım izlenecek yollar ve kod örnekleri dahil edilir.



##  <a name="touchandroidapp-fundamentalstouchindexmd"></a>[Dokunma](~/android/app-fundamentals/touch/index.md)

Bu bölümde, kavramlar ve uygulama ayrıntılarını Android üzerinde gerçekleştirdiği hareketler touch açıklanmaktadır. Dokunma API'leri sunulan ve bir hareketi tanıyıcıları araştırması tarafından izlenen açıklanmıştır.



##  <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[HttpClient Yığını ve SSL/TLS](~/android/app-fundamentals/http-stack.md)

Bu bölümde, Android için HttpClient yığını ve SSL/TLS uygulama seçiciler açıklanmaktadır. Bu ayarlar, Xamarin.Android uygulamaları tarafından kullanılan HttpClient ve SSL/TLS uygulaması belirler.


##  <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[Esnek uygulamaları yazma](writing-responsive-apps.md)

Bu makalede, iş parçacığı oluşturma uzun süre çalışan görevler bir arka plan iş parçacığı açın taşıyarak bir Xamarin.Android uygulaması yanıt verebilir durumda tutmak için nasıl kullanılacağı açıklanmaktadır.