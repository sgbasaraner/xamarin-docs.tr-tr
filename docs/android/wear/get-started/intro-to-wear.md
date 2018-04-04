---
title: Android yıpranması giriş
description: Harika Android uygulamaları geliştirmek için geldiğinde Google Android takmak başlanmasıyla, artık yalnızca telefonlar ve tabletler kısıtlanır. Android takmak Xamarin.Android'ın desteği C# kodunu bileğinizi üzerinde çalıştırmak mümkün hale getirir! Bu giriş, temel bir bakış Android takmak sağlar, anahtar özelliklerini açıklar ve Android takmak 2.0 kullanılabilir özelliklere genel bakış sunar. Bazı daha popüler Android takmak cihazları listeler ve daha fazla bilgi için Google Android takmak temel bir belge bağlantılar sağlar.
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 0ab166bb71c23d456cb70d35a2794717110642fd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-android-wear"></a>Android yıpranması giriş

_Harika Android uygulamaları geliştirmek için geldiğinde Google Android takmak başlanmasıyla, artık yalnızca telefonlar ve tabletler kısıtlanır. Android takmak Xamarin.Android'ın desteği C# kodunu bileğinizi üzerinde çalıştırmak mümkün hale getirir! Bu giriş, temel bir bakış Android takmak sağlar, anahtar özelliklerini açıklar ve Android takmak 2.0 kullanılabilir özelliklere genel bakış sunar. Bazı daha popüler Android takmak cihazları listeler ve daha fazla bilgi için Google Android takmak temel bir belge bağlantılar sağlar._


## <a name="overview"></a>Genel Bakış

Android yıpranması first-generation Motorola 360, LG'ın G izlemenize ve Samsung dişli Canlı dahil cihazları çeşitli üzerinde çalışır. Sony'nın SmartWatch 3 dahil olmak üzere ikinci oluşturma, yerleşik GPS ve çevrimdışı müzik çalma gibi ek yetenekler de yayımladı. Android takmak 2.0 için Google ile LG için iki yeni saatlerde bağdaştırıcıya: LG izleme Spor ve LG izleme stili.

![Android takmak 2.0 aygıtlar](intro-to-wear-images/hero-image.png "örnek Android takmak 2.0 aygıtlar")

Xamarin.Android 5.0 ve sonrasındaki destekler Android takmak bizim Android 4.4W (API 20) aracılığıyla destek ve ek ekler bir NuGet paketi yıpranması özgü UI denetler. Xamarin.Android 5.0 ve sonrasındaki yıpranması uygulamalarınızı paketleme işlevi de içerir. NuGet paketleri, Android takmak bu kılavuzda açıklanan 2.0 için de kullanılabilir.


## <a name="android-wear-basics"></a>Android yıpranması temelleri

Android yıpranması Android el uygulamaları farklı bir kullanıcı arabirimi standardı vardır. İlk wave yıpranması uygulamaların bir yardımcı genişletmek için tasarlanmış bazı şekilde, ancak Android yıpranması 2.0 ile başlayan el uygulamada, yıpranması uygulamalar kullanılan tek başına olabilir. Yıpranması uygulamayı dağıttığınızda, bir yardımcı el uygulaması ile paketlenmiştir. Çoğu takmak için taşınabilir yardımcı uygulama uygulamalar bağımlı, el uygulamalarla iletişim kurmak için bazı yol ihtiyaç duydukları. Aşağıdaki bölümlerde bu kullanım senaryoları açıklar ve gerekli Android takmak özelliklerin ana hatlarını vermektedir. 



### <a name="usage-scenarios"></a>Kullanım senaryoları

Android takmak ilk sürümü öncelikle Gelişmiş bildirimleri geçerli el uygulamalarla genişletme ve taşınabilir uygulama wearable uygulama arasında veri eşitlemeye odaklanmıştır. Bu nedenle, bu senaryolar uygulamak oldukça basittir.


#### <a name="wearable-notifications"></a>Wearable bildirimleri

Android takmak desteklemek için basit bildirimleri el ile wearable cihazı arasında paylaşılan yapısını yararlanmak için yoldur. Destek v4 bildirimi API kullanarak ve `WearableExtender` sınıfı (kullanılabilir [Xamarin Android destek kitaplığı](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)), gelen kutusu stili kartlar gibi platform yerel özelliklerini içine dokunun veya ses giriş. [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/) örnek Android takmak cihazına listesini bildirimler göndermek nasıl gösteren kod örneği sağlar. 



#### <a name="companion-applications"></a>Yardımcı uygulamalar

Başka bir strateji wearable cihazda yerel olarak çalışan ve yardımcı el uygulamayla çiftleri tam bir uygulamanın oluşturmaktır. Bu yaklaşımın iyi bir örnektir [test](https://developer.xamarin.com/samples/monodroid/wear/Quiz/) nasıl taşınabilir bir aygıtta çalışan ve test sorularını wearable cihazda soran bir test oluşturulduğunu gösteren örnek uygulama. 



### <a name="user-interface"></a>Kullanıcı Arabirimi

Birincil gezinti düzeni yıpranması için dikey olarak düzenlenmiş kartları dizisidir. Bu kartlar her ilişkili aynı satırda çıkışı katmanlı Eylemler sahip. `GridViewPager` Sınıfı bu işlevselliği sağlar; aynı bağdaştırıcısı kavram aynılarını `ListView`. Genellikle ilişkilendirmek `GridViewPager` ile bir `FragmentGridPagerAdaptor` (veya `GridPagerAdaptor`) olanak tanıyan her satır ve sütun hücreler olarak temsil eden bir `Fragment`: 

[![Gezinti takmak](intro-to-wear-images/2d-picker-sml.png "takmak gezinme")](intro-to-wear-images/2d-picker.png#lightbox)

Ayrıca büyük oluşur eylem düğmelerinin kullanımını (olarak Resimli yukarıda) kısa açıklama metnini bunun altındaki bir daire renkli yapar önler.  [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) örnek nasıl kullanılacağını gösteren `GridViewPager` ve `GridPagerAdapter` yıpranması uygulama.

Android takmak 2.0 yıpranması kullanıcı arabirimine bir gezinti bölümü, bir eylem bölümü ve satır içi eylem düğmeleri ekler. Android Android takmak 2.0 kullanıcı arabirimi öğeleri hakkında daha fazla bilgi için bkz: [anatomisi](https://www.google.com/design/spec-wear/system-overview/anatomy.html) konu. 



### <a name="communications"></a>İletişim

Android yıpranması iki farklı iletişim wearable uygulamalar ve yardımcı el uygulamalar arasındaki iletişimi kolaylaştırmak için API'ler sağlar: 

**Veri API** &ndash; bu API, wearable aygıt ve taşınabilir aygıt arasında eşitlenen veri deposunun benzer. Android wearable ve taşınabilir arasındaki değişiklikleri yayılıyor Bunu yapmak için en uygun olduğunda mvc'deki. Üstte taşınır aralık dışında olduğunda, daha sonra eşitleme sıralar. Bu API için ana giriş noktasıdır `WearableClass.DataApi`. Bu API hakkında daha fazla bilgi için bkz: Android [eşitleniyor veri öğeleri](https://developer.android.com/training/wearables/data-layer/data-items.html) konu. 

**İleti API** &ndash; bu API, alt düzey iletişim yolu kullanmanızı mümkün kılar: küçük bir yükü tek yönlü el ve wearable uygulamalar arasında eşitleme olmadan gönderilir.
Bu API için ana giriş noktasıdır `WearableClass.MessageApi`.
Bu API hakkında daha fazla bilgi için bkz: Android [gönderme ve alma iletileri](https://developer.android.com/training/wearables/data-layer/messages.html) konu.

Her API dinleyicisi arabirimi aracılığıyla bu iletileri almak için geri aramaları kaydetme veya alternatif olarak, uygulamanızda türeyen bir hizmet uygulamak seçebileceğiniz `WearableListenerService`.
Bu hizmet, Android takmak tarafından otomatik olarak oluşturulacak.
[FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/) örnek gösterilmektedir nasıl uygulanacağını bir `WearableListenerService`.



### <a name="deployment"></a>Dağıtım

Wearable her uygulamanın kendi APK dosyasının APK ana uygulama içinde katıştırılmış dağıtılır. Bu paketleme Xamarin.Android 5.0 ve daha sonra otomatik olarak gerçekleştirilir, ancak el ile Xamarin.Android sürüm 5. 0'den önceki sürümleri için gerçekleştirilmesi gerekir. 
[Paketle birlikte çalışma](~/android/wear/deploy-test/packaging.md) dağıtımı daha ayrıntılı açıklanır. 



## <a name="going-further"></a>Daha fazla işlenmesini 

Yapı ve ilk uygulamanızı test etmek için en iyi Android takmak ile aşina şekilde denetleyebilirsiniz. Aşağıdaki liste, hızlı bir şekilde hızlıca sağlamanıza yardımcı olması için önerilen okuma sırası sağlar:

1.  [Kurulum ve yükleme](~/android/wear/get-started/installation.md) yükleme ve Xamarin.Android yıpranması uygulamaları oluşturmak için geliştirme ortamınızı yapılandırma hakkında ayrıntılı yönergeler sağlar. 

2.  Gerekli paketleri yüklü ve bir öykünücü veya aygıt yapılandırdıktan sonra bkz: [Merhaba, takmak](~/android/wear/get-started/hello-wear.md) küçük bir Android takmak projesi bu tanıtıcıları düğmenin nasıl oluşturulacağını açıklayan adım adım yönergeler için tıklar ve görüntüler bir Sayaç yıpranması aygıtta'yi tıklatın. 

3.  [Dağıtım & test](~/android/wear/deploy-test/index.md) daha ayrıntılı yapılandırma ve Öykünücüler ve yıpranması cihazın Bluetooth yoluyla uygulamanızı dağıtmak yönergeler de dahil olmak üzere cihazlara dağıtma hakkında bilgi sağlar.

4.  [Ekran boyutlarıyla çalışma](~/android/wear/screen-sizes.md) Önizleme ve Kullanıcı Arabiriminizin yıpranması aygıtları çeşitli kullanılabilir ekran boyutlarına için en iyi duruma getirme açıklanmaktadır. 

5.  [Paketle birlikte çalışma](~/android/wear/deploy-test/packaging.md) el ile Google play'de dağıtım için yıpranması Uygulama paketleme için gereken adımları açıklar.

İlk yıpranması uygulamanızı oluşturduktan sonra Android takmak için bir özel izleme yazıtipi oluşturmayı denemek isteyebilirsiniz. 
[Gözcü yüz oluşturma](~/android/wear/platform/creating-a-watchface.md) bir kırpılmış dijital izleme yüz hizmeti, bir analog stili izleme yüz ek özelliklerle geliştirir daha fazla kod ve ardından aşağı geliştirmek için adım adım yönergeler ve örnek kod sağlar. 



## <a name="android-wear-20"></a>Android Wear 2.0

Android takmak 2.0 tanıtır yeni özellikler ve yetenekler, çeşitli gibi *karışıklıklardan*, eğri düzenleri, gezinti ve eylem çekmeceleri ve genişletilmiş bildirimler. Ayrıca, takmak 2.0 el uygulamaları bağımsız olarak çalışır tek başına uygulamalar oluşturmanızı mümkün kılar. Yeni *bileğe kadar hareketleri* uygulamanız ile tek elli etkileşimleri yeteneği sağlar. Aşağıdaki bölümlerde bu özellikler vurgulayın ve yardımcı olması için bağlantıları uygulamanızda kullanmaya başlamanıza sağlayın.



### <a name="install-wear-20-packages"></a>Yükleme takmak 2.0 paketleri

Xamarin.Android takmak 2.0 uygulamayla oluşturmak için eklemelisiniz **Xamarin.Android.Wear v2.0** paketini projenize (tıklatın **Gözat sekmesini**):

[![Xamarin.Android.Wear v2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "Xamarin.Android.Wear v2.0 NuGet yükleyin")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

Bu NuGet paketi Android destek Wearable hem takmak Compat kitaplıkları için bağlamaları içerir.

Ek olarak **Xamarin.Android.Wear**, yüklediğiniz öneririz **Xamarin.GooglePlayServices.Wearable** NuGet: 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Install the Xamarin.GooglePlayServices.Wearable NuGet")](intro-to-wear-images/gpsw-nuget.png#lightbox)


### <a name="key-features-of-wear-20"></a>Yıpranması 2.0 temel özellikleri

Android takmak 2.0 büyük Android takmak için ilk başlatma 2014 itibaren güncelleştirmesidir. Aşağıdaki bölümlerde Android takmak 2.0 anahtar özelliklerini vurgulayın ve bağlantıları yardımcı olmak için sağlanan uygulamanızda bu yeni özellikleri ile çalışmaya başlamak. 


#### <a name="complications"></a>Zorluklar

*Zorluk* küçük izleme izleme yüz içeri doğru çekin zorunda kalmadan bir bakışta görebilirsiniz yüz pencere öğeleri şunlardır. Zorluklar Masaüstü stili Pano pencere öğeleri için benzer; Bunlar, hava durumu, pil ömrünün, takvim olayları ve uygunluk uygulama istatistikleri gibi bilgileri görüntüle: 

![Karışıklıklardan örnek](intro-to-wear-images/complications.png "zorluklar örneği")

Android zorluklar hakkında daha fazla bilgi için bkz: [izleme yüz zorluklar](https://developer.android.com/wear/preview/features/complications.html) konu. 



#### <a name="navigation-and-action-drawers"></a>Gezinti ve eylem çekmeceleri 

İki yeni çekmeceleri takmak 2.0 dahil edilir. *Gezinti çekmecesini*, ekranın en üstünde görünür (sol tarafta gösterildiği gibi) uygulama görünümleri arasında gezinmek kullanıcıların sağlar. *Eylem çekmecesini*, Eylemler listesinden seçmek kullanıcıların sağlar (sağ taraftaki gösterildiği gibi) ekranın alt kısmında görünür. 

![Gezinti ve eylem çekmeceleri](intro-to-wear-images/drawers.png "gezinti ve eylem çekmeceleri")

Bu iki yeni etkileşimli çekmeceleri hakkında daha fazla bilgi için bkz: Android [takmak gezinti ve eylemleri](https://developer.android.com/wear/preview/features/ui-nav-actions.html) konu. 



#### <a name="curved-layouts"></a>Eğri düzenleri 

Yıpranması 2.0 eğri düzenleri yuvarlak yıpranması cihazlarda görüntülemek için yeni özellikler sunar. Özellikle, yeni `WearableRecyclerView` sınıfı en iyi duruma getirilmiş yuvarlak ekranlarda dikey öğelerinin bir listesini görüntülemek için: 

![Curved düzeni örneği](intro-to-wear-images/curved-layout.png "eğri düzeni örneği")

`WearableRecyclerView` genişletir `RecyclerView` eğri düzenleri ve döngüsel kaydırma hareketleri desteklemek için sınıf. Daha fazla bilgi için bkz: Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) API belgeleri. 



#### <a name="standalone-apps"></a>Tek başına uygulamaları 

Android takmak 2.0 uygulamaları el uygulamaları bağımsız olarak çalışabilir. Bunun anlamı, örneğin, akıllı izleme Yardımcısı taşınabilir aygıt devre dışı veya uzakta wearable aygıttan açık olsa bile tam işlevsellik sunmaya devam edebilirsiniz. Bu özellik hakkında daha fazla bilgi için bkz: Android [tek başına uygulamaları](https://developer.android.com/wear/preview/features/standalone-apps.html) konu.



#### <a name="wrist-gestures"></a>Bileğe kadar hareketleri 

Bileğe kadar hareketleri olanaklı hale getirir dokunmatik ekran kullanmadan uygulamanızı ile etkileşim kullanıcılar için &ndash; kullanıcılar tek el ile bir uygulamanın yanıt verebilir. İki bileğe kadar hareketleri desteklenir: 

-   Hareket bileğe kadar çıkışı
-   İçinde hareket bileğe kadar

Daha fazla bilgi için bkz: Android [bileğe kadar hareketleri](https://developer.android.com/wear/preview/features/gestures.html) konu. 


Satır içi Eylemler, akıllı yanıt, uzak giriş, genişletilmiş bildirimleri ve bildirimler için yeni bir köprü oluşturma modu gibi pek çok daha fazla takmak 2.0 özellikleri vardır. Android yeni takmak 2.0 özellikler hakkında daha fazla bilgi için bkz: [API genel bakış](https://developer.android.com/wear/preview/api-overview.html). 



## <a name="devices"></a>Cihazlar

Android takmak çalıştırabilirsiniz aygıtları bazı örnekleri şunlardır:

* [Motorola 360](https://moto360.motorola.com/)
* [LG G izleme](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [LG G izleme R](http://www.lg.com/us/smartwatch/g-watch-r)
* [Samsung dişli Canlı](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASUS ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)



## <a name="further-reading"></a>Daha Fazla Bilgi

Google Android takmak belgelerini denetleyin:

* [Android yıpranması hakkında](http://www.android.com/wear/)
* [Android yıpranması uygulama tasarımı](https://developer.android.com/design/wear/index.html)
* [Android.support.wearable kitaplığı ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android yıpranması 2.0](https://developer.android.com/wear/preview/index.html)



## <a name="summary"></a>Özet

Bu giriş, Android takmak bir genel bakış sağlanır. Android takmak, temel özellikleri özetlenen ve bir genel bakış Android takmak 2.0 sunulan özellikler dahil. Xamarin.Android yıpranması geliştirmeye başlamalarını geliştiricilere yardımcı olmak için temel okuma bağlantıları sağlanan ve şu anda piyasadaki Android takmak aygıtları bazı örnekleri aşağıda listelenen.


## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme ve Kurulum](~/android/wear/get-started/installation.md)
- [Başlarken](~/android/wear/get-started/index.md)
