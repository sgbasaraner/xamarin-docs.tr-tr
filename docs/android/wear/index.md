---
title: Android Wear
description: "Android wearable cihazlar için uygulama oluşturma."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ac83b74f39497333de7aa80079784adf61bf2e65
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
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



## <a name="samples"></a>Örnekler

Bir dizi bulabilirsiniz [örnekleri](https://developer.xamarin.com/samples/android/Android%20Wear/) Android takmak kullanarak (veya doğrudan gitmek [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
          <strong>Örnek</strong>
      </th>
      <th>
          <strong>Açıklama</strong>
      </th>
      <th>
          <strong>ekran görüntüsü</strong>
      </th>
  </thead>
  <tbody>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/SkeletonWear/">SkeletonWear</a>
      </td>
      <td valign="top">
GridViewPager ve etkileşimli bildirimler de dahil olmak üzere wearable projeleri temelleri basit bir örneği.
      </td>
      <td>
          <img src="Images/skeleton.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/WatchViewStub/">WatchViewStub</a>
      </td>
      <td valign="top">
Basit tanıtım WatchViewStub denetiminin ekran şekli algılar ve doğru düzeni otomatik olarak yükler.
WatchViewStub şeklini görmek <b>Resources/layout/main_actvity.xml</b> düzeni.
      </td>
      <td>
          <img src="Images/watchview.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/RecipeAssistant/">RecipeAssistant</a>
      </td>
      <td valign="top">
Tanıtım yıpranması bildirim sayfaların tarif adımları biçiminde. Bildirimleri oluşturulan <b>RecipeService.cs</b>.
      </td>
      <td>
          <img src="Images/recipeassist.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/ElizaChat/">ElizaChat</a>
      </td>
      <td valign="top">
"Kişisel Yardımcısı" ile etkileşim, eğlenceli örnek Eliza, yıpranması etkileşimli bildirimleri kullanarak tamamlanmış yanıtlarını kullanarak bir konuşma oluşturmak için çağrılır.
      </td>
      <td>
          <img src="Images/eliza.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/GridViewPager/">GridViewPager</a>
      </td>
      <td valign="top">
GridViewPager kullanıcı swipes dikey burada 2B Gezinti deseni uygular ve seçenekleri ve içeriği yatay gidin.
      </td>
      <td>
          <img src="Images/gridviewpager.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/monodroid/wear/WatchFace">WatchFace</a>
      </td>
      <td valign="top">
          <b>WatchFace</b> analog stili saat, dakika ve ikinci el ile özel izleme yüz olduğu. Bu örnek, geçerli saati çizer izleme yüz hizmet oluşturmak için ve tanıtıcıları ortam modu ve görünürlük olayları değiştir nasıl gösterir. Saat dilimi değişiklikleri dinler ve otomatik olarak zaman buna göre güncelleştiren bir yayın alıcı içerir.
      </td>
      <td>
          <img src="Images/watchface.png" class="tableimg">
      </td>
  </tr>
  </tbody>
</table>

##  <a name="videos"></a>Videolar

Video Bu destek yıpranması ile Xamarin.Android ele bağlantılar denetleyin.

<table align="center" border="0" cellpadding="1" cellspacing="1">
    <tr>
        <td>
        <a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/"><img src="Images/video-android-l.png" border="0" /></td>
        <td><a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/">Android m ve çok daha fazlası</a>
        <br />
Android M Geliştirici önizlemesi, malzeme tasarım, bildirimler ve yeni bir animasyon birkaçıdır dahil olmak üzere, yararlanmak geliştiricilere yönelik yeni API sayısız kullanıma sunuldu.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=80H8tXByZQc"><img src="Images/video-eyes-ears.png" border="0" /></td>
        <td><a href="https://www.youtube.com/watch?v=80H8tXByZQc">C# Zevkten ve my gözler: Google cam ve Android takmak</a>
        <br />
Wearable bilgi işlem benzeri gelecek (veya bir denetçisi aracı bölüm) görünebilir, ancak çoğu kişi zaten gelecek bugün benimsemenin! C# geliştiricileri bu bilmeniz ve araçlar ve beceriler wearable aygıtlardan (geliştikçe 2014) gücünü harekete geçirmek zaten sahip.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU"><img src="Images/video-whats-new.png" border="0" /></td>
        <td><a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU">Xamarin.Android yenilikler</a>
        <br />
        <i>Android M, Android yıpranması, Android TV, Android otomatik, malzeme tasarım ve resim; Bunun için Xamarin geliştirici olarak anlamı nedir? </i> gelen 2014 geliştirin.</td>
    </tr>
</table>


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
