---
title: "Uygulama Yayımlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 51E19000-040A-2B74-C462-EC57C617085C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 99f66fd0d23f14224bcd915ef7d1c6d81367f173
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="publishing-an-application"></a>Uygulama Yayımlama

Harika bir uygulama oluşturduktan sonra kişilerin kullanmak istersiniz. Bu bölüm Android için e-posta, bir özel web sunucusu, Google Play veya Amazon uygulama mağazası gibi kanallar aracılığıyla Xamarin.Android ile oluşturulan bir uygulamanın genel dağıtım sahip adımlarını kapsar.

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

Son bir Xamarin.Android uygulaması geliştirme uygulamayı yayımlamak için adımdır. Yayımlama kullanıcıların cihazlarına yüklemek hazır iki önemli görevleri içerir, bir Xamarin.Android uygulaması derleme işlemidir:

-   **Yayın için hazırlama** &ndash; uygulama sürümü oluşturulur Android destekli cihazlara dağıtılabilir (bkz [yayımı için bir uygulama hazırlama](~/android/deploy-test/release-prep/index.md) hakkında daha fazla bilgi için Hazırlık sürüm).

-   **Dağıtım** &ndash; uygulama sürümünü bir veya daha fazla çeşitli dağıtım kanalları kullanılabilir hale getirilir.

Aşağıdaki diyagram, bir Xamarin.Android uygulaması yayımlama sahip adımlarını gösterir:

[ ![Yapı ve dağıtım akış çizelgesi](images/build-and-deploy-steps.png)](images/build-and-deploy-steps.png)

Yukarıdaki diyagramda görüldüğü gibi hazırlık dağıtım yöntemi bakılmaksızın aynı olacak. Bir Android uygulamasını yayımlanabilecek birkaç yolu vardır kullanıcılar için:

-   **Bir Web sitesi aracılığıyla** &ndash; bir Xamarin.Android uygulaması yapılabilir indirilebilir kullanıcıların daha sonra yüklemek uygulama bir bağlantıyı tıklatarak bir Web sitesinde.
-   **E-posta yoluyla** &ndash; kullanıcılar kendi e-posta bir Xamarin.Android uygulaması mümkündür. Ek bir Android destekli aygıtla açıldığında uygulama yüklenir.
-   **Bir Market üzerinden** &ndash; dağıtım için gibi mevcut birkaç uygulama Pazar vardır [Google Play](http://play.google.com/) veya [Amazon App Store Android için](http://www.amazon.com/mobile-apps/b?ie=UTF8&node=2350149011) .


Yerleşik bir Market kullanarak, en geniş pazarda erişebileceğiniz ve dağıtım üzerinde en yüksek denetim sağladığı gibi bir uygulamayı yayımlamak için en yaygın bir yoludur. Ancak, bir Market üzerinden bir uygulama yayımlama ek çaba gerekir.

Birden çok kanaldan bir Xamarin.Android uygulaması aynı anda dağıtabilirsiniz. Örneğin, bir uygulama Google play'de Android için Amazon App Store yayınlanabilir ve ayrıca bir web sunucusundan indirilmesi.

Diğer iki yöntemden (indirme veya e-posta) dağıtımının denetlenen bir alt kuruluş ortamında veya yalnızca bir kullanıcı küçük veya iyi belirtilen kümesi için tasarlanmıştır bir uygulama gibi kullanıcı için en kullanışlıdır.
Sunucu ve e-posta dağıtımı bir uygulama yayımlamak için daha az hazırlık gerektiren daha basit yayımlama modelleri de altındadır.

Amazon mobil uygulama dağıtım programı dağıtmak ve Amazon uygulamalarını satmak mobil uygulama geliştiricileri sağlar. Kullanıcıları bulmak ve Amazon uygulama mağazası uygulamasını kullanarak Android destekli cihazlarına uygulamalar için alışveriş. Amazon uygulama Mağazası'nin bir Android cihazında çalıştırılan bir ekran görüntüsü aşağıda yer almaktadır:

Google Play tartışmaya açık bir şekilde Android uygulamalar için en kapsamlı ve popüler Market olur. Google Play kullanıcıların bulmak, oranı, indirin ve uygulamalar için tek bir simge cihazlarında veya bilgisayarlarında tıklayarak ödeme olanak tanır. Google Play satış ve pazarlama eğilimleri çözümlemede size yardımcı olmak ve hangi cihazları denetlemek için araçlar da sağlar ve kullanıcıların bir uygulama indirebilirsiniz. Google Play'de Android cihazda çalışan bir ekran görüntüsü aşağıda yer almaktadır:

[ ![Google Play ekran görüntüsü](images/google-play-app.png)](images/google-play-app.png)

Bu bölümde Google Play gibi bir depolama uygun promosyon malzemelerini birlikte uygulamaya karşıya nasıl yükleneceğini gösterir. APK genişletme dosyaları nedir ve nasıl çalıştıklarını kavramsal genel bakış sağlayan açıklanmıştır. Google lisans hizmetleri de açıklanmaktadır. Son olarak, dağıtım alternatif anlamına gelir, Android için bir HTTP web sunucusu, basit bir e-posta dağıtımı ve Amazon App Store kullanımı dahil olmak üzere sunulan.


## <a name="related-links"></a>İlgili bağlantılar

- [HelloWorldPublishing (sample)](https://developer.xamarin.com/samples/monodroid/HelloWorldPublishing/)
- [Derleme işlemi](~/android/deploy-test/building-apps/build-process.md)
- [Bağlama](~/android/deploy-test/linker.md)
- [API anahtarı Google alma eşlemeleri](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Uygulamanızı imzalama](https://source.android.com/security/apksigning/)
- [Google play'de yayımlama](http://developer.android.com/distribute/googleplay/publish/index.html)
- [Google uygulama lisansı](http://developer.android.com/guide/google/play/licensing/index.html)
- [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary)
- [Mobil uygulama dağıtım portalı](https://developer.amazon.com/welcome.html)
- [Amazon mobil uygulama dağıtım ile ilgili SSS](https://developer.amazon.com/help/faq.html)
