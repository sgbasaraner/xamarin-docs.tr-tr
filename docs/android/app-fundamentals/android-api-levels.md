---
title: Android API düzeylerini anlama
description: Xamarin.Android, Android birden çok sürümü ile uygulama uyumluluğunu belirlemek çeşitli Android API düzeyi ayarları vardır. Bu kılavuz, bu ayarlar ne anlama geliyor, bunların nasıl yapılandırılacağı ve etkisini açıklar çalışma zamanında uygulamanızı sahiptirler.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/02/2018
ms.openlocfilehash: 3b060567b47395bc213627c9378de4fca9db41bb
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403344"
---
# <a name="understanding-android-api-levels"></a>Android API düzeylerini anlama

_Xamarin.Android, Android birden çok sürümü ile uygulama uyumluluğunu belirlemek çeşitli Android API düzeyi ayarları vardır. Bu kılavuz, bu ayarlar ne anlama geliyor, bunların nasıl yapılandırılacağı ve etkisini açıklar çalışma zamanında uygulamanızı sahiptirler._


## <a name="quick-start"></a>Hızlı Başlangıç

Xamarin.Android üç Android API düzey proje ayarları kullanıma sunar:

-   [Hedef Framework](#framework) &ndash; uygulamanızı oluşturmaya kullanmak için hangi çerçevesini belirtir. Bu API düzeyi sırasında kullanılan *derleme* Xamarin.Android tarafından zaman.

-   [En düşük Android sürümü](#minimum) &ndash; desteklemek üzere uygulamanızı istediğiniz en eski Android sürümünü belirtir. Bu API düzeyi sırasında kullanılan *çalıştırma* zaman Android tarafından.

-   [Hedef Android sürümü](#target) &ndash; uygulamanız Android sürümünden hedeflenen çalıştırmak için belirtir. Bu API düzeyi sırasında kullanılan *çalıştırma* zaman Android tarafından.

Projeniz için bir API düzeyi yapılandırmadan önce bu API düzeyi için SDK platform bileşenleri yüklemeniz gerekir. Karşıdan yükleme ve Android SDK bileşenlerini yükleme hakkında daha fazla bilgi için bkz. [Android SDK Kurulumu](~/android/get-started/installation/android-sdk.md).

> [!NOTE]
> Ağustos 2018'de başlayarak, Google Play konsolunu yeni uygulamalar 26 (Android 8.0) API düzeyi hedefleyin gerektirir veya üzeri.
API düzeyi 26 ya da daha yüksek Kasım 2018'den itibaren hedeflemek için mevcut uygulamaları gerekli olacaktır. Daha fazla bilgi için [app security ve Google play'de gelecek yıl için performans iyileştirme](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Normalde, tüm üç Xamarin.Android API düzeyleri aynı değere ayarlanır. Üzerinde **uygulama** sayfasında **(hedef çerçeve) Android sürümünü kullanarak derle** en son kararlı API sürümü için (veya en azından, ihtiyacınız olan tüm özelliklere sahip bir Android sürümü için).
Aşağıdaki ekran görüntüsünde, hedef Framework'ü kümesine **Android 7.1 (API düzeyi 25 - Nougat)**:

[![Hedef Framework sürümü varsayılan olarak Android sürümü kullanarak derleme](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Üzerinde **Android bildirim** sayfasında, en düşük Android sürümü kümesine **kullanım SDK sürümünü kullanan derleme** ve hedef Android sürümü hedef çerçeve sürümünde (aşağıdaki aynı değere ayarlanır ekran görüntüsü, hedef Android çerçevesi ayarlandığında **Android 7.1 (Nougat)**):

[![Hedef Framework sürümü için en az ve hedef Android sürümleri ayarlayın](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Android önceki bir sürümü ile geriye dönük uyumluluğu korumak istiyorsanız, **en düşük Android sürümü hedef** desteklemek üzere uygulamanızı istediğiniz en eski sürümünü Android için. (API düzeyi 14 için gerekli en düşük API düzeyi olduğuna dikkat edin [Google Play Hizmetleri ve Destek Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) Aşağıdaki örnek yapılandırma API düzeyi 25'ya kadar geçerli Android API düzeyi 14 sürümlerini destekler:

[![API düzeyi 25'i kullanarak derleme Nougat, 14 API düzeyi için en düşük Android sürümü](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Normalde, tüm üç Xamarin.Android API düzeyleri aynı değere ayarlanır. Ayarlama **hedef Framework'ü** en son kararlı API sürümü için (veya en azından, ihtiyacınız olan tüm özelliklere sahip bir Android sürümü için). Ayarlanacak **hedef Framework'ü**, gitmek **Yapı > Genel** içinde **proje seçenekleri**. Aşağıdaki ekran görüntüsünde, hedef Framework'ü kümesine **en son yüklenen Platformu (8.0) kullanın**:

[![Hedef Framework'ü en son yüklenen platformu kullan için varsayılan olarak ayarlanıyor](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

En az ve hedef Android sürümü ayarlarının altında bulunabilir **Yapı > Android uygulaması** içinde **proje seçenekleri**. En düşük Android sürümü kümesine **otomatik - hedef çerçeve sürümünü kullan** ve hedef Android sürümü hedef Framework sürümü ile aynı değere ayarlanır. Aşağıdaki ekran görüntüsünde, hedef Android çerçevesi kümesine **Android 8.0 (API düzeyi 26)** yukarıdaki hedef çerçeve ayarıyla eşleşmesi için:

[![Proje seçeneklerinde hedef ve framework düzeylerini ayarlama](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Android önceki bir sürümü ile geriye dönük uyumluluk sağlamak isterseniz değiştirme **en düşük Android sürümü** desteklemek üzere uygulamanızı istediğiniz en eski sürümünü Android için. API düzeyi 14 için gerekli en düşük API düzeyi olduğuna dikkat edin [Google Play Hizmetleri ve Destek Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Örneğin, aşağıdaki yapılandırma API düzeyi 14 olabildiğince erken Android sürümlerini destekler:

[![En az ve hedef sürümlerinde - otomatik olarak ayarlandığından, hedef framework sürümü kullanın](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


Uygulamanızı birden çok Android sürümlerini destekliyorsa, kodunuzu uygulamanızı en düşük Android sürümü ayarı ile çalıştığından emin olmak için çalışma zamanı denetimleri içermelidir (bkz [çalışma zamanı denetimleri Android sürümleri için](#runtimechecks) altındaki ayrıntılar için). Kullanma veya bir kitaplık oluşturmak bkz [API düzeylerini ve kitaplıkları](#libraries) aşağıda en iyi yapılandırma API'si için kitaplıklar için ayarları düzey.



## <a name="android-versions-and-api-levels"></a>Android sürümleri ve API düzeyleri

Android platform geliştikçe ve yeni Android sürümleri yayımlandığında her Android sürümü adlı bir tamsayı benzersiz tanımlayıcı atanmış *API düzeyi*. Bu nedenle, her bir Android sürümü, tek bir Android API düzeyi karşılık gelir. Kullanıcıların uygulamaları eski de olarak en son Android sürümlerine yüklemesi olduğundan, gerçek Android uygulamaları birden çok Android API düzeyleri ile çalışacak şekilde tasarlanmalıdır.


### <a name="android-versions"></a>Android sürümleri

Her bir Android sürümü, birden çok adlarıyla geçer:

-   Android sürümü gibi **Android 7.1**
-   Bir kod adı gibi _Nougat_
-   Gibi ilgili API düzey **API düzeyi 25**

Bir Android kod adı birden çok sürümleri ve (aşağıda görüldüğü gibi) API düzeylerini karşılık, ancak her Android sürümü tam olarak bir API düzeyine karşılık gelir.

Ayrıca, Xamarin.Android tanımlar *sürüm kodlarını yapı* için şu anda bilinen Android API düzeyleri eşleyin. Aşağıdaki listede, API düzeyi Android sürümü, kod adı ve Xamarin.Android derleme sürüm kodu arasında çevirinize yardımcı olabilir.

-   **API 27 (Android 8.1)** &ndash; _Oreo_, aralık 2017 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.OMr1`

-   **API 26 (Android 8.0)** &ndash; _Oreo_, Ağustos 2017 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.O`

-   **API 25 (Android 7.1)** &ndash; _Nougat_, aralık 2016 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.NMr1`

-   **API 24 arasındaki sürümleri (Android 7.0)** &ndash; _Nougat_, Ağustos 2016 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.N`

-   **API 23 (Android 6.0)** &ndash; _Marshmallow_, Ağustos 2015 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.M`

-   **API 22 (Android 5.1)** &ndash; _Lollipop_, Mart 2015 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.LollipopMr1`

-   **API 21 (Android 5.0)** &ndash; _Lollipop_, Kasım 2014 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.Lollipop`

-   **API 20 (Android 4.4W)** &ndash; _Kitkat Watch_, Haziran 2014'te yayınlandı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.KitKatWatch`

-   **API 19 (Android 4.4)** &ndash; _Kitkat_, Ekim 2013 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.KitKat`

-   **API 18'e (Android 4.3)** &ndash; _Jelibon_, Temmuz 2013 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **API 17 (Android 4.2-4.2.2)** &ndash; _Jelibon_, Kasım 2012 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **API 16 (Android 4.1-4.1.1)** &ndash; _Jelibon_, Haziran 2012 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.JellyBean`

-   **API 15 (Android 4.0.3-4.0.4)** &ndash; _ICE Cream Sandwich_, aralık 2011 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **API 14 (Android 4.0-4.0.2)** &ndash; _ICE Cream Sandwich_, Ekim 2011 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.IceCreamSandwich`

-   **API 13 (Android 3.2)** &ndash; _bal peteği_, Haziran 2011 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.HoneyCombMr2`

-   **API 12 (Android 3.1.x)** &ndash; _bal peteği_, Mayıs 2011 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.HoneyCombMr1`

-   **API 11 (Android 3.0.x)** &ndash; _bal peteği_, Şubat 2011 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.HoneyComb`

-   **API 10 (Android 2.3.3-2.3.4)** &ndash; _Zencefilli kurabiye_, Şubat 2011 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.GingerBreadMr1`

-   **API 9 (Android 2.3-2.3.2)** &ndash; _Zencefilli kurabiye_, Kasım 2010 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.GingerBread`

-   **API 8 (Android 2.2.x)** &ndash; _Froyo_, Haziran 2010 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.Froyo`

-   **API 7 (Android 2.1.x)** &ndash; _Eclair_, Ocak 2010 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.EclairMr1`

-   **API 6 (Android 2.0.1)** &ndash; _Eclair_, Aralık 2009 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.Eclair01`

-   **API 5 (Android 2.0)** &ndash; _Eclair_, Kasım 2009 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.Eclair`

-   **API 4 (Android 1.6)** &ndash; _halka_, Eylül 2009 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.Donut`

-   **API 3 (Android 1.5)** &ndash; _Cupcake_, Mayıs 2009 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.Cupcake`

-   **API 2 (Android 1.1)** &ndash; _temel_, Şubat 2009 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.Base11`

-   **API 1 (Android 1.0)** &ndash; _temel_, Ekim 2008 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.Base`


Bu liste da anlaşılacağı gibi yeni Android sürümlerinde sık yayınlanır &ndash; yılda bazen çeşitli yayınlar. Sonuç olarak, uygulamanızı çalıştıran Android cihazlarının evreni eski ve yeni Android sürümlerinde, birçok farklı içerir. Nasıl uygulamanızın tutarlı ve güvenilir bir şekilde çok sayıda farklı Android sürümlerinde çalışacağını garanti edebilir? Android API düzeyleri, bu sorunu yönetmenize yardımcı olabilir.


### <a name="android-api-levels"></a>Android API düzeyleri

Her bir Android cihaz, tam olarak çalışan *bir* API düzeyi &ndash; bu API düzeyi Android platform sürümü benzersiz olması garanti edilir. API düzeyi tam olarak uygulamanızı çağırabilirsiniz API kümesi sürümünü tanımlayan; Bu bildirim öğeleri, izinler, vb., kodu karşı birleşimi bir geliştirici olarak tanımlar. Android API düzeyleri sistemi, uygulama bir cihazda uygulama yüklemeden önce bir Android sistem görüntüsü ile uyumlu olup olmadığını belirleme Android yardımcı olur.

Bir uygulama oluşturulduğunda, aşağıdaki API düzeyi bilgileri içerir:

-   *Hedef* uygulamayı çalıştırmak için yerleşik Android API düzeyi.

-   *Minimum* Android API düzeyi, uygulamanızı çalıştırmak için bir Android cihazı olması gerekir. 

Bu ayarlar, yükleme sırasında uygulama düzgün şekilde çalışması için gereken işlevsellik Android cihazda kullanılabilir olduğundan emin olmak için kullanılır. Aksi durumda, uygulama o cihazda çalıştırılması engellenir. Bir Android cihazının API düzeyi, uygulamanız için belirttiğiniz en düşük API düzeyi daha düşük maliyetlidir, örneğin, Android cihaz kullanıcı uygulamanızı yüklemenize engel olmaz.


## <a name="project-api-level-settings"></a>Proje API düzeyi ayarları

Aşağıdaki bölümlerde SDK Yöneticisi'ni ve ardından hedeflemek istediğiniz API düzeyleri için nasıl yapılandırılacağına dair ayrıntılı açıklamalar tarafından geliştirme ortamınızı hazırlama için nasıl kullanılacağını açıklayan *hedef Framework'ü*, *en az Android sürümü*, ve *hedef Android sürümü* Xamarin.Android ayarlar.


### <a name="android-sdk-platforms"></a>Android SDK'sı platformlar

Xamarin.Android bir hedef veya en düşük API düzeyi seçmeden önce bu API düzeyine karşılık gelen Android SDK platform sürümü yüklemeniz gerekir. Hedef Çerçeve ve en düşük Android sürümü hedef Android sürümü için kullanılabilir seçenekleri aralığını yüklemiş olduğunuz Android SDK aralığı sürümleriyle sınırlıdır. Gerekli Android SDK sürümlerinin yüklü olduğunu ve uygulamanız için gereken herhangi bir yeni API düzey eklemek için kullanın doğrulamak için SDK Yöneticisi'ni kullanabilirsiniz. API düzeylerini yükleme konusunda bilgi sahibi değilseniz bkz [Android SDK Kurulumu](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Hedef Çerçeve

*Hedef Framework'ü* (diğer adıyla `compileSdkVersion`) uygulamanızın derleme zamanında derlenir Android çerçevesi sürümü (API düzeyi). Bu ayar, uygulamanızın hangi API'ler belirtir *bekliyor* çalıştırır, ancak, API uygulamanıza kullanılabileceğinden yüklendiğinde hiçbir etkisi kullanılacak. Sonuç olarak, hedef çerçeve ayarıyla değiştirilmesi çalışma zamanı davranışı değişmez.

Hedef Framework'ü uygulamanızın bağlı karşı hangi kitaplık sürümleri tanımlar &ndash; bu uygulamanızda kullanabileceğiniz hangi API'ler belirler. Örneğin, kullanmak istediğiniz [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Android 5.0 Lollipop'ta sunulan yöntemi hedef Framework'ü ayarlamalısınız **API düzey 21 (Lollipop)** veya üzeri. Proje hedef Framework'ü API'ye gibi düzeyinde ayarlarsanız **API düzey 19 (KitKat)** ve çağırmayı deneyin `SetCategory` kodunuzda yöntem, bir derleme hatası alırsınız.

Her zaman ile derleme olmasını öneririz *son* kullanılabilir hedef Framework sürümü. Bunun yapılması yararlı uyarı iletileri ile kodunuz tarafından çağrılan tüm kaldırılmış API'ler için sağlar. En yeni hedef Framework sürümü kullanarak özellikle önemli olduğu son destek kitaplık sürümleri kullandığınızda &ndash; destek kitaplığın en düşük API düzeyinde derlenmiş veya büyük olması için uygulamanızı her kitaplık bekliyor. 


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da hedef çerçeve ayarıyla erişmek için proje özelliklerinde açın **Çözüm Gezgini** seçip **uygulama** sayfası:

[![Uygulama sayfası, proje özellikleri](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

API düzeyi aşağı açılan menüsünü seçerek hedef Framework'ü ayarlama **Android sürümünü kullanarak derle** yukarıda da gösterildiği gibi.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da hedef çerçeve ayarıyla erişmek için proje adını sağ tıklatın ve seçin **seçenekleri**; bu açılır **proje seçenekleri** iletişim. Bu iletişim kutusunda gidin **Yapı > Genel** burada gösterildiği gibi:

[![Proje seçenekleri sayfasına genel bir bölümünü derleme](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Bir API düzeyi sağındaki açılan menüsünü seçerek hedef Framework'ü ayarlama **hedef Framework'ü** yukarıda da gösterildiği gibi.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>En düşük Android sürümü

*En düşük Android sürümü* (diğer adıyla `minSdkVersion`) eski Android yükleyebilir ve uygulamanızı çalıştırmak işletim (yani, en düşük API düzeyi) sürümü. Varsayılan olarak, bir uygulama yalnızca hedef çerçeve ayarıyla eşleşen aygıtlar üzerinde yüklü ya da daha yüksek olabilir; en düşük Android sürümü ayarı ise *alt* hedef çerçeve ayarından daha önceki Android sürümlerinde de uygulamanızı çalıştırabilirsiniz. Örneğin, hedef Framework'ü ayarlayın **Android 7.1 (Nougat)** ve en düşük Android sürümü kümesine **Android 4.0.3 (ICE Cream Sandwich)**, uygulamanızı herhangi bir platformdan API düzeyi 15 yüklenebilir API düzeyi 25, kapsamlı için.

Uygulamanız başarıyla oluşturun ve bu çeşitli platformları üzerinde yükleme olsa da, bu başarılı olacağı garanti etmez *çalıştırma* tüm platformlar. Örneğin, uygulamanızın yüklendiyse **Android 5.0 (Lollipop)** ve kodunuzun yalnızca bir API çağırır **Android 7.1 (Nougat)** ve daha yeni sürümü, uygulamanız bir çalışma zamanı hatası alın ve muhtemelen kilitlenme. Bu nedenle, kodunuzu sağlamalısınız &ndash; zamanında &ndash; çalıştığı Android cihaz tarafından desteklenen API'lerini çağırır. Diğer bir deyişle, kodunuzu uygulamanızı desteklemek için yeni olan cihazlarda yeni API'ler kullanmasını sağlamak için açık bir çalışma zamanı denetimleri içermesi gerekir.
[Çalışma zamanı denetimleri Android sürümleri](#runtimechecks), bu kılavuzda daha sonra kodunuzda bu çalışma zamanı denetimleri ekleme işlemi açıklanmaktadır.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da en düşük Android sürümü ayarı erişmek için proje özelliklerinde açın **Çözüm Gezgini** seçip **Android bildirim** sayfası. Aşağı açılan menüde **en düşük Android sürümü** uygulamanız için seçebileceğiniz en düşük Android sürümü:

[![En düşük Android SDK sürümünü kullanan derleme için hedef seçeneği](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

Seçerseniz **kullanım SDK sürümünü kullanan derleme**, en düşük Android sürümü hedef çerçeve ayarıyla aynı olacaktır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio en düşük Android sürümü erişmek için proje adını sağ tıklatın ve seçin **seçenekleri**; bu açılır **proje seçenekleri** iletişim. Gidin **Yapı > Android uygulaması**.
Sağındaki açılan menüsünü kullanarak **en düşük Android sürümü**, uygulamanız için en düşük Android sürümü ayarlayabilirsiniz:

[![Otomatik - hedef çerçeve sürümünü kullan'olarak ayarlandığından en düşük Android sürümü](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

Seçerseniz **otomatik &ndash; hedef framework sürümünü kullanmak**, en düşük Android sürümü hedef çerçeve ayarıyla aynı olacaktır.

-----


<a name="target" />

### <a name="target-android-version"></a>Hedef Android sürümü

*Hedef Android sürümü* (diğer adıyla `targetSdkVersion`) burada bekliyor uygulamayı çalıştırmak için Android cihazda düzeyi API'dir. Android, herhangi bir uyumluluk davranışının etkinleştirilip etkinleştirilmeyeceğini belirlemek için bu ayarı kullanan &ndash; bu uygulamanızın beklediğiniz gibi çalışmaya devam etmesini sağlar. Android uygulamanızın hedef Android sürümü ayarı hangi davranış değişiklikleri uygulamanıza (Android ileriye dönük uyumluluk nasıl sağladığını budur) bozmadan uygulanabilir anlamak için kullanır.

Hedef Framework'ü ve çok benzer ada sırasında hedef Android sürümü, aynı şey değildir. Hedef Çerçeve ayarıyla kullanılmak üzere hedef API düzeyi bilgileri Xamarin.Android için iletişim kuran *derleme zamanı*hedef Android sürümü kullanılmak üzere hedef API düzeyi bilgileri android'e iletişim kuran korurken  *çalışma zamanı* (ne zaman uygulamanın yüklü olduğundan ve bir cihaz üzerinde).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu ayar Visual Studio'da erişmek için proje özelliklerinde açın **Çözüm Gezgini** seçip **Android bildirim** sayfası. Aşağı açılan menüde **hedef Android sürümü** hedef Android sürümü, uygulamanız için seçin:

[![Hedef Android sürümü SDK sürümünü kullanan derleme için ayarlayın](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Hedef Android sürümü açıkça uygulamanızı test etmek için kullandığınız Android son sürümüne ayarlamanızı öneririz. İdeal olarak, en son Android SDK'sı sürümüne ayarlanmalıdır &ndash; Bu davranış değişiklikleri üzerinden geçmeden önce yeni API'ler kullanmanıza olanak tanır. Çoğu geliştirici için biz *olmayan* hedef Android sürümü ayarlamanızı öneririz **kullanım SDK sürümünü kullanan derleme**.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da bu ayar erişmek için proje adını sağ tıklatın ve seçin **seçenekleri**; bu açılır **proje seçenekleri** iletişim. Gidin **Yapı > Android uygulaması**. Sağındaki açılan menüsünü kullanarak **hedef Android sürümü**, hedef Android sürümü, uygulamanız için ayarlayabilirsiniz:

[![Hedef Android sürümü otomatik - hedef çerçeve sürümünü kullan'olarak ayarlayın.](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Hedef Android sürümü açıkça uygulamanızı test etmek için kullandığınız Android son sürümüne ayarlamanızı öneririz. İdeal olarak, en son kullanılabilir Android SDK'sı sürüm için ayarlanmalıdır &ndash; Bu davranış değişiklikleri üzerinden geçmeden önce yeni API'ler kullanmanıza olanak tanır. Çoğu geliştirici için hedef Android sürümü ayarı önermiyoruz **otomatik - hedef çerçeve sürümünü kullan**.

-----

Genel olarak, hedef Android sürümü hedef Framework'ü ve en düşük Android sürümü ile sınırlanmış. Bunun anlamı:

**En düşük Android sürümü < hedef Android sürümü = < hedef Framework'ü =**

Android Geliştirici SDK düzeyleri hakkında daha fazla bilgi için bkz. [kullanır-sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) belgeleri.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Android sürümleri için çalışma zamanı denetimleri

Her yeni Android sürümü yayınlandığında yeni sağlamak için API framework güncelleştirilir veya değiştirilen işlevleri. Birkaç özel durum dışında önceki Android sürümlerinde API işlevleri değişiklik olmadan daha yeni Android sürüm İleri taşınır. Sonuç olarak, uygulamanızı belirli bir Android API düzeyi üzerinde çalışıyorsa, bu genellikle daha yeni bir Android API düzeyi değişiklik olmadan çalışan mümkün olacaktır. Ancak, ne de uygulamanızı önceki Android sürümlerinde çalıştırmak ister misiniz?

En düşük Android sürümü seçerseniz *alt* , hedef çerçeve ayarından bazı API uygulamanıza çalışma zamanında kullanılabilir olmayabilir. Ancak uygulamanızı yine de önceki bir cihazda, ancak azaltılmış işlevlerle çalıştırabilirsiniz. En düşük Android sürümü ayarına karşılık gelen Android platformları üzerinde kullanılabilir değil her bir API için kodunuzu açıkça değerini denetlemelisiniz `Android.OS.Build.VERSION.SdkInt` uygulama üzerinde çalışan platform API düzeyini belirlemek için özellik. API düzeyi ise *alt* aramak istediğiniz API'yi destekler en düşük Android sürümünden daha sonra bu API çağrısı yapmaya gerek olmadan düzgün bir şekilde bulmak kodunuzu sahiptir.

Örneğin, kullanılacak istediğimizi varsayalım [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) çalışırken bir bildirim kategorilere ayırmak için yöntemi **Android 5.0 Lollipop** (ve daha sonra), ancak yine de uygulamamıza istiyoruz Önceki Android sürümlerinde çalıştırmak **Android 4.1 Jelibon** (burada `SetCategory` kullanılamaz). Bu kılavuzun başındaki Android sürümü tablosuna başvuran, olduğunu görebiliriz için derleme sürüm kodu **Android 5.0 Lollipop** olduğu `Android.OS.BuildVersionCodes.Lollipop`. Android burada daha eski sürümlerini desteklemek üzere `SetCategory` olduğundan kullanılamıyor, kodumuz çalışma zamanı API düzeyinde algılayabilir ve koşullu olarak çağrı `SetCategory` yalnızca API düzeyi büyüktür veya eşittir Lollipop derleme sürüm kodu olduğunda:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

Bu örnekte, uygulamanın hedef Framework'ü kümesine **Android 5.0 (API düzey 21)** ve en düşük Android sürümü kümesine **Android 4.1 (API düzeyi 16)**. Çünkü `SetCategory` API düzeyinde kullanılabilir `Android.OS.BuildVersionCodes.Lollipop` ve daha sonra bu kod örneği çağıracak `SetCategory` yalnızca gerçekten kullanılabilir olduğunda &ndash; olacak *değil* aramaktadır `SetCategory` olduğunda API 16, 17, 18, 19 veya 20 düzeyidir. İşlevi, yalnızca düzgün (bunlar türüne göre kategorize edilir değil çünkü) bildirimleri sıralı değildir, ancak bildirimleri kullanıcıyı uyarmak için yine de yayımlanır için toplasa bile, bu önceki Android sürümlerinde azaltılır. Uygulamamızı hala çalışıyor, ancak işlevselliğini biraz düşer.

Genel olarak, derleme sürüm denetimi, kodunuzu eski yöntem ve yeni yolu bir şey yapmak arasında zamanında karar vermenize yardımcı olur. Örneğin:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    // Do things the Lollipop way
}
else
{
    // Do things the pre-Lollipop way
}
```

Bir veya daha fazla API eksik eski Android sürümleri üzerinde çalıştığında, uygulamanızın işlevselliğini değiştirmenize ya da azaltmak nasıl açıklayan hızlı ve basit kural yoktur. Bazı durumlarda (olduğu gibi `SetCategory` Yukarıdaki örneği), mevcut olmadığında yalnızca API çağrısı atlamak yeterlidir. Ancak, diğer durumlarda, alternatif işlevselliği için ne zaman uygulamak gerekebilir `Android.OS.Build.VERSION.SdkInt` az API düzeyi uygulamanızı kendi en iyi deneyimi sunmak gereken olmasını algılandı.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API düzeylerini ve kitaplıkları

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bir Xamarin.Android kitaplık projesi (örneğin, bir sınıf kitaplığı veya bağlamaları kitaplığı için) oluştururken, yalnızca hedef çerçeve ayarıyla yapılandırabileceğiniz &ndash; en düşük Android sürümü ve hedef Android sürümü ayarlar kullanılamaz. Yoktur çünkü hiçbir **Android bildirim** sayfası:

[![Yalnızca Android sürümü seçeneğiyle derleme kullanılabilir](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bir Xamarin.Android kitaplık projesi oluşturduğunuzda, var olan hiçbir **Android uygulaması** yapılandırabileceğiniz en düşük Android sürümü ve hedef Android sürümü sayfa &ndash; hedef ve en düşük Android sürümü Android sürümü ayarlar kullanılamaz.
Yoktur çünkü hiçbir **Yapı > Android uygulaması** sayfa):

[![Derleme en az ve hedef sürüm seçenekleri olmadan genel sayfası](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

Sonuçta elde edilen kitaplık tek başına bir uygulaması olmadığından ve en düşük Android sürümü hedef Android sürümü ayarları kullanılabilir değil &ndash; kitaplığı ile paketlenmiştir uygulamaya bağlı olarak herhangi bir Android sürümü üzerinde çalıştırılabilir. Nasıl kitaplığı olacak belirtebilirsiniz *derlenmiş*, ancak hangi platformu API düzeyi kitaplığı üzerinde çalıştırılacak tahmin edemezsiniz. Bunu aklınızda, aşağıdaki en iyi kullanma ya da kitaplıkları oluşturma gözlenmelidir:

-   **Bir Android kitaplığı tüketildiğinde** &ndash; Android kitaplığı uygulamanızda kullanıyorsa, uygulamanızın hedef Framework'ü API'ye düzeyi ayarının ayarladığınızdan emin olun *en az kadar yüksek olarak* hedefi Çerçeve ayarını kitaplığı.

-   **Bir Android kitaplığı oluştururken** &ndash; diğer uygulamalar tarafından kullanım için bir Android kitaplık oluşturuyorsanız, hedef çerçeve ayarıyla derlemek için gereken en düşük API düzeyi ayarlamak emin olun.

Bu en iyi uygulamalar, nerede (hangi uygulamanın kilitlenmesine neden olabilir) çalışma zamanında kullanılabilir değil bir API'yi çağırmak için bir kitaplık çalışır duruma önlemeye yardımcı olmak için önerilir. Bir kitaplığı geliştiricisi olarak, API çağrılarının toplam API yüzey alanı küçük ve tanınmış bir kısmına kullanımını kısıtlamak çaba göstermelisiniz. Bunun yapılması, bu nedenle kitaplığınızı daha geniş bir çeşitli Android sürümleri arasında güvenli bir şekilde kullanılabileceğini sağlamaya yardımcı olur.


## <a name="summary"></a>Özet

Bu kılavuz, farklı Android sürümleri arasında uygulama uyumluluğu yönetmek için Android API düzeyleri nasıl kullanıldığı açıklanmıştır. Xamarin.Android yapılandırmak için ayrıntılı adımlar sağlanan *hedef Framework'ü*, *en düşük Android sürümü*, ve *hedef Android sürümü* proje ayarları. Bu SDK paketleri yüklemek için Android SDK Yöneticisi'ni kullanarak yönergeler dahil farklı API düzeylerini zamanında uğraşmanız kodunun nasıl yazılacağını örnekleri ve oluştururken ya da Android kitaplıklarını tüketen API düzeylerini yönetme konusunda açıklandığı sağlanır. Ayrıca, API düzeyi Android sürüm numaraları (örneğin, Android 4.4), Android sürüm adları (örneğin, Kitkat) ve Xamarin.Android derleme sürüm kodlarını ilgili kapsamlı bir liste sağlanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android SDK Kurulumu](~/android/get-started/installation/android-sdk.md)
- [SDK'sı CLI araçları değiştirir](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [CompileSdkVersion minSdkVersion ve targetSdkVersion çekme](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [API düzeyi nedir?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenames, etiketler ve yapı numaraları](https://source.android.com/source/build-numbers)
