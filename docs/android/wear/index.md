---
title: Android Wear
description: "Android wearable cihazlar için uygulama oluşturma."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/16/2018
ms.openlocfilehash: 31114df0b631aea909e82f3a8b836d5ef922d2c1
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="android-wear"></a>Android Wear

Android yıpranması akıllı saatlerde gibi wearable cihazlar için tasarlanmış Android sürümüdür. Bu bölüm, yükleme ve yıpranması geliştirme, ilk yıpranması Cihazınızı ve kendi oluşturmak için uygulamaları takması başvurabilir örnekleri listesini oluşturmak için adım adım için gerekli Araçları'nı yapılandırma hakkında yönergeler içerir.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[Başlarken](~/android/wear/get-started/index.md)

Android takmak tanıtır, yükleyin ve bilgisayarınızı yıpranması geliştirme için yapılandırma açıklar ve bir öykünücü veya yıpranması cihaz üzerinde ilk Android takmak uygulamanızı çalıştırın ve oluşturmanıza yardımcı için adımları sağlar.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Kullanıcı Arabirimi](~/android/wear/user-interface/index.md)

Android yıpranması özgü denetler ve bu denetimlerinin nasıl kullanıldığını gösteren örnekler için bağlantılar sağlar açıklanmaktadır.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[Platform Özellikleri](~/android/wear/platform/index.md)

Bu bölümdeki belgeler Android takmak için belirli özellikleri kapsar. Burada bir WatchFace oluşturmayı açıklar bir konuda bulabilirsiniz.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Ekran Boyutları](~/android/wear/screen-sizes.md)

Önizleme ve Kullanıcı Arabiriminizin kullanılabilir ekran boyutları için en iyi duruma getirme.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Dağıtım ve Test](~/android/wear/deploy-test/index.md)

Bir Android takmak cihazına veya yıpranması için yapılandırılmış Android öykünücüsü Android takmak uygulamanızı dağıtmak açıklanmaktadır. Hata ayıklama ipuçları ve geliştirme bilgisayarınıza ve bir Android cihazı arasında bir Bluetooth bağlantı kurma için bilgileri içerir.

##  <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[API takmak](https://developer.android.com/reference/android/support/wearable)

Android Geliştirici sitesi gibi anahtar takmak API'ler hakkında ayrıntılı bilgi sağlar [Wearable etkinlik](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), [hedefleri](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html), [kimlik doğrulaması](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [ Karışıklıklardan](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html), [işleme Karışıklıklardan](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [bildirimleri](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html), [görünümleri](https://developer.android.com/reference/android/support/wearable/view/package-summary.html), ve [WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html).



## <a name="samples"></a>Örnekler

Bir dizi bulabilirsiniz [örnekleri](https://developer.xamarin.com/samples/android/Android%20Wear/) Android takmak kullanarak (veya doğrudan gitmek [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

|Örnek|Açıklama|ekran görüntüsü|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/SkeletonWear/)|GridViewPager ve etkileşimli bildirimler de dahil olmak üzere wearable projeleri temelleri basit bir örneği.|![Skeletonwear ekran görüntüsü](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/)|Basit tanıtım WatchViewStub denetiminin ekran şekli algılar ve doğru düzeni otomatik olarak yükler.  WatchViewStub şeklini görmek **Resources/layout/main_actvity.xml** düzeni.|![WatchViewStub ekran görüntüsü](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/RecipeAssistant/)|Tanıtım yıpranması bildirim sayfaların tarif adımları biçiminde. Bildirimleri RecipeService.cs içinde oluşturulur.|![RecipeAssistant ekran görüntüsü](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/ElizaChat/)|"Kişisel Yardımcısı" ile etkileşim, eğlenceli örnek Eliza, yıpranması etkileşimli bildirimleri kullanarak tamamlanmış yanıtlarını kullanarak bir konuşma oluşturmak için çağrılır.|![ElizaChat ekran görüntüsü](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/)|GridViewPager kullanıcı swipes dikey burada 2B Gezinti deseni uygular ve seçenekleri ve içeriği yatay gidin.|![Screenshot of GridViewPager](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|Özel İzleme yüz analog stili saat, dakika ve ikinci el ile WatchFace olur. Bu örnek, geçerli saati çizer izleme yüz hizmet oluşturmak için ve tanıtıcıları ortam modu ve görünürlük olayları değiştir nasıl gösterir. Saat dilimi değişiklikleri dinler ve otomatik olarak zaman buna göre güncelleştiren bir yayın alıcı içerir.|![WatchFace ekran görüntüsü](images/gridviewpager.png)|


##  <a name="videos"></a>Videolar

Video Bu destek yıpranması ile Xamarin.Android ele bağlantılar denetleyin:

|Açıklama|ekran görüntüsü|
|--- |--- |
|[Android m ve çok daha](http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; Android M Geliştirici önizlemesi malzeme tasarım, bildirimler ve yeni bir animasyon birkaçıdır dahil olmak üzere yeni API'lerin geliştiricilerin, yararlanmak kullanılan çok sunmuştur.|![Sunu video ekran görüntüsü](images/video-android-l.png)|
|[C# Zevkten ve my gözler: Google cam ve Android takmak](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; Wearable bilgi işlem göründüğü benzeri gelecek (veya bir denetçisi aracı bölüm), ancak çoğu kişi zaten gelecek bugün benimsemenin! C# geliştiricileri bu bilmeniz ve araçlar ve beceriler wearable aygıtlardan (geliştikçe 2014) gücünü harekete geçirmek zaten sahip.|![Sunu video ekran görüntüsü](images/video-eyes-ears.png)|
|[Xamarin.Android yenilikler](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android M, Android takmak, Android TV, Android otomatik, malzeme tasarım ve resim; bu yaptığı Xamarin geliştirici olarak size ortalama? geliştikçe 2014 gelen.|![Sunu video ekran görüntüsü](Images/video-whats-new.png)|


<!--

March 18
http://blog.xamarin.com/android-wear/

August 14
http://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
http://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
