---
title: "Android API düzeylerini anlama"
description: "Xamarin.Android Android birden çok sürümü ile uygulama uyumluluğunu belirlemek birkaç Android API düzeyi ayarlarına sahiptir. Bu kılavuz, bu ayarları anlamı, bunların nasıl yapılandırılacağı ve hangi etkisi açıklar çalışma zamanında uygulamanıza sahiptirler."
ms.topic: article
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 5a8b51f6c63d8632e71d1cddabb0c37758ee02f0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="understanding-android-api-levels"></a>Android API düzeylerini anlama

_Xamarin.Android Android birden çok sürümü ile uygulama uyumluluğunu belirlemek birkaç Android API düzeyi ayarlarına sahiptir. Bu kılavuz, bu ayarları anlamı, bunların nasıl yapılandırılacağı ve hangi etkisi açıklar çalışma zamanında uygulamanıza sahiptirler._

<a name="quick" />

## <a name="quick-start"></a>Hızlı Başlangıç

Xamarin.Android üç Android API düzey proje ayarları sunar:

-   [Hedef çerçevesi](#framework) &ndash; uygulamanızı oluşturmaya kullanmak için hangi framework belirtir. Bu API düzeyi sırasında kullanılan *derleme* Xamarin.Android tarafından zaman.

-   [En düşük Android sürümü](#minimum) &ndash; desteklemek için uygulamanızın istediğiniz eski Android sürümünü belirtir. Bu API düzeyi sırasında kullanılan *çalıştırmak* Android tarafından zaman.

-   [Hedef Android sürümü](#target) &ndash; uygulamanızın Android sürümü hedeflenen çalıştırılacağını belirtir. Bu API düzeyi sırasında kullanılan *çalıştırmak* Android tarafından zaman.

Projeniz için bir API düzeyi yapılandırmadan önce bu API düzeyi için SDK platform bileşenleri yüklemeniz gerekir. Karşıdan yükleme ve Android SDK bileşenlerini yükleme hakkında daha fazla bilgi için bkz: [Android SDK Kurulum](~/android/get-started/installation/android-sdk.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Normalde, tüm üç Xamarin.Android API düzeylerini aynı değere ayarlanır. Üzerinde **uygulama** sayfasında **Android sürümü (hedef Framework) kullanılarak derleme** en son kararlı API sürümüne (veya en azından, gereksinim duyduğunuz özelliklerin tümü, Android sürümü için).
Aşağıdaki ekran görüntüsünde, hedef Framework'ü ayarlamak **Android 7.1 (API düzeyi 25 - Nougat)**:

[![Hedef Framework sürümü varsayılanlara Android sürümünü kullanarak derleme](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png)

Üzerinde **Android derleme bildirimi** sayfasında, Minimum Android sürümü kümesine **SDK sürümü kullanarak kullanım derleme** ve hedef Android sürümü hedef Framework sürümünü (aşağıdaki aynı değere ayarlayın ekran görüntüsü, hedef Android Framework'ü ayarlanmış **Android 7.1 (Nougat)**):

[![Hedef Framework sürümü için en az ve hedef Android sürümleri ayarlayın](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png)

Android önceki bir sürümü ile geriye dönük uyumluluğu korumak istiyorsanız, **hedef için Minimum Android sürümü** desteklemek için uygulamanızın istediğiniz en eski sürümünü Android için. (API düzeyi 14 için gerekli en az API düzeyi olduğuna dikkat edin [Google Play hizmetlerini ve Firebase Destek](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) Aşağıdaki örnek yapılandırma API düzeyi 25 aracılığı API Düzey 14 Android sürümlerini destekler:

[![API düzeyi 25 kullanılarak derleme Nougat, Minimum Android sürümü 14 API düzeyine ayarlayın](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Normalde, tüm üç Xamarin.Android API düzeylerini aynı değere ayarlanır. Ayarlama **hedef framework** en son kararlı API sürümüne (veya en azından, gereksinim duyduğunuz özelliklerin tümü, Android sürümü için). Ayarlamak için **hedef framework**, gitmek **Yapı > Genel** içinde **proje seçenekleri**. Aşağıdaki ekran görüntüsünde, hedef Framework'ü ayarlamak **son yüklü platform (8.0) kullanmak**:

[![Hedef framework son yüklenmiş platform için varsayılan olarak ayarlanıyor](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png)

En az ve hedef Android sürümü ayarlar altında bulunabilir **Yapı > Android uygulaması** içinde **proje seçenekleri**. Minimum Android sürümü kümesine **otomatik - kullanım hedef framework sürümü** ve hedef Android sürümü hedef Framework sürümü ile aynı değere ayarlayın. Aşağıdaki ekran görüntüsünde, hedef Android Framework'ü ayarlamak **Android 8.0 (API düzeyi 26)** yukarıdaki hedef Framework ayarını eşleştirmek için:

[![Hedef ve framework düzeylerini proje seçenekleri ayarlama](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png)

Android önceki bir sürümü ile geriye dönük uyumluluğu korumak isterseniz, değiştirmek **Minimum Android sürümü** Android eski sürümüne desteklemek için uygulamanızın istiyor. API Düzey 14 için gerekli en az API düzeyi olduğuna dikkat edin [Google Play hizmetlerini ve Firebase Destek](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Örneğin, aşağıdaki yapılandırma API Düzey 14 gibi erken Android sürümlerini destekler:

[ ![Minimum otomatik olarak - ayarlamak hedef sürümlerini kullanan hedef framework sürümü](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png)

-----


Uygulamanız birden çok Android sürümlerini destekliyorsa, kodunuzu uygulamanızı Minimum Android sürümü ayarıyla çalıştığından emin olmak için çalışma zamanı denetimleri eklemeniz gerekir (bkz [çalışma zamanı denetler Android sürümleri için](#runtimechecks) aşağıda Ayrıntılar için). Kullanma veya bir kitaplığı oluşturmak, bkz: [API düzeyleri ve kitaplıkları](#libraries) aşağıda API yapılandırmada en iyi uygulamalar için ayarları kitaplıkları düzeyi.


<a name="verslevels" />

## <a name="android-versions-and-api-levels"></a>Android sürümleri ve API düzeyleri

Android platformu dönüşmesi ve yeni Android sürümlerini yayımlandığında olarak her Android sürümü olarak adlandırılan bir benzersiz tamsayı tanımlayıcı atanmış *API düzeyi*. Bu nedenle, her bir Android sürümü, tek bir Android API düzeyine karşılık gelir. Kullanıcıların eski de olarak en son sürümlerine ilişkin Android uygulamaları yüklemek için gerçek Android uygulamaları birden çok Android API düzeyi ile çalışmak için tasarlanmış olması gerekir.

<a name="versions" />

### <a name="android-versions"></a>Android sürümleri

Android her bir sürümü tarafından birden çok adı geçer:

-   Android sürüm gibi **Android 7.1**
-   Bir kod adı gibi _Nougat_
-   Karşılık gelen bir API düzey, gibi **API düzeyi 25**

Bir Android kod adı birden çok sürüm ve (aşağıda görüldüğü gibi) API düzeylerini karşılık, ancak her Android sürümü tam olarak bir API düzeyine karşılık gelir.

Ayrıca, Xamarin.Android tanımlar *sürüm kodlarını yapı* şu anda bilinen Android API düzeylerini eşleyin. Aşağıdaki listede, API düzeyi, Android sürümü, kod adı ve Xamarin.Android derleme sürüm kodu arasında Çevir yardımcı olabilir.

-   **API 27 (Android 8.1)** &ndash; _Oreo_, aralık 2017 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.OMr1`

-   **API 26 (Android 8.0)** &ndash; _Oreo_, Ağustos 2017 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.O`

-   **API 25 (Android 7.1)** &ndash; _Nougat_, aralık 2016 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.NMr1`

-   **API 24 (Android 7.0)** &ndash; _Nougat_, Ağustos 2016 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.N`

-   **API 23 (Android 6.0)** &ndash; _Marshmallow_, Ağustos 2015 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.M`

-   **API 22 (Android 5.1)** &ndash; _Lollipop_, released March 2015. Sürüm kodu derleme `Android.OS.BuildVersionCodes.LollipopMr1`

-   **API 21 (Android 5.0)** &ndash; _Lollipop_, released November 2014. Sürüm kodu derleme `Android.OS.BuildVersionCodes.Lollipop`

-   **API 20 (Android 4.4W)** &ndash; _Kitkat izleme_, Haziran 2014 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.KitKatWatch`

-   **API 19 (Android 4.4)** &ndash; _Kitkat_, Ekim 2013 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.KitKat`

-   **API 18 (Android 4.3)** &ndash; _Jelly Çekirdeklere_, Temmuz 2013 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **API 17 (Android 4.2-4.2.2)** &ndash; _Jelly Çekirdeklere_, Kasım 2012 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **API 16 (Android 4.1-4.1.1)** &ndash; _Jelly Çekirdeklere_, Haziran 2012 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.JellyBean`

-   **API 15 (Android 4.0.3-4.0.4)** &ndash; _dondurma Sandwich_, aralık 2011 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **API 14 (Android 4.0-4.0.2)** &ndash; _dondurma Sandwich_, Ekim 2011 serbest bırakıldı. Sürüm kodu derleme `Android.OS.BuildVersionCodes.IceCreamSandwich`

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


Bu liste da anlaşılacağı gibi yeni Android sürümlerini sık yayımlanan &ndash; yıllık bazen çeşitli yayınlar. Sonuç olarak, uygulamanızı çalışabilir Android cihazları universe çok çeşitli eski ve yeni Android sürümlerini içerir. Nasıl uygulamanızı tutarlı ve güvenilir bir şekilde çok sayıda farklı sürümlerine ilişkin Android çalışacağını garanti edebilir mi? Android'ın API düzeylerini, bu sorunu yönetmenize yardımcı olabilir.

<a name="apilevels" />

### <a name="android-api-levels"></a>Android API düzeyleri

Android aygıtların başından tam olarak çalışan *bir* API düzeyi &ndash; bu API düzeyi Android platformu sürümü benzersiz olması garanti edilmez. API düzeyini tam olarak uygulamanızı çağırabilirsiniz API kümesi sürümü tanımlar; bildirim öğeleri, izinler, vb., kodun karşı birleşimi bir geliştirici olarak tanımlar. Android'ın sistem API düzeylerinin bir uygulama bir cihazda uygulamayı yüklemeden önce bir Android sistem görüntüsü ile uyumlu olup olmadığını belirleme Android yardımcı olur.

Bir uygulama yapılandırıldığında, aşağıdaki API düzeyi bilgileri içerir:

-   *Hedef* uygulamayı çalıştırmak için yerleşik Android API düzeyi.

-   *Minimum* bir Android cihaz uygulamanızı çalıştırmak için gereken Android API düzeyi. 

Bu ayarlar, yükleme zamanında uygulamanın düzgün çalışması için gerekli işlevselliği Android cihazında kullanılabilir olduğundan emin olmak için kullanılır. Aksi durumda, bu cihazda çalışan uygulama engellenir. Bir Android cihaz API düzeyini uygulamanız için belirttiğiniz en düşük API düzeyini daha düşük ise, örneğin, Android cihazı kullanıcı uygulamanızı yüklemenize engel olmaz.

<a name="settings" />

## <a name="project-api-level-settings"></a>Proje API düzeyi ayarları

Aşağıdaki bölümlerde SDK Manager hedeflemek istediğiniz API düzeylerini izleyen için yapılandırma hakkında ayrıntılı açıklamalar tarafından geliştirme ortamınızı hazırlama için nasıl kullanılacağı açıklanmaktadır *hedef Framework*, *en az Android sürüm*, ve *hedef Android sürümü* Xamarin.Android ayarlar.

<a name="sdk" />

### <a name="android-sdk-platforms"></a>Android SDK'sı platformlar

Xamarin.Android içinde bir hedef veya en az API düzeyi seçmeden önce bu API düzeyine karşılık gelen Android SDK platform sürümünü yüklemeniz gerekir. Hedef Framework, Minimum Android sürümü ve hedef Android sürümü için kullanılabilir seçenekleri aralığını yüklediğiniz Android SDK aralığı sürümleriyle sınırlıdır. Gerekli Android SDK sürümlerinin yüklü olduğunu ve uygulamanız için gereken herhangi bir yeni API düzey eklemek için kullanın doğrulamak için SDK Yöneticisi'ni kullanabilirsiniz. API düzeylerini yükleme konusunda bilgi sahibi değilseniz bkz [Android SDK Kurulum](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Hedef çerçevesi

*Hedef Framework* (olarak da bilinen `compileSdkVersion`) uygulamanız için derleme zamanında derlenir belirli Android framework sürümü (API düzeyi). Bu ayar, uygulamanızın hangi API'leri belirtir *bekliyor* çalıştırır, ancak üzerinde API uygulamanıza kullanılabileceğinden yüklendiğinde herhangi bir etkisi olmaz kullanılacak. Sonuç olarak, hedef Framework ayarını değiştirme çalışma zamanı davranışı değiştirmez.

Uygulamanızı karşı bağlı olduğu hangi kitaplık sürümleri hedef Framework'ü tanımlayan &ndash; bu uygulamanızda kullanabileceğiniz hangi API'leri belirler. Kullanmak istiyorsanız, örneğin, [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Android 5.0 Lolipop içinde tanıtılan yöntemi hedef Framework'ü ayarlamalısınız **API düzeyi 21 (Lolipop)** veya sonraki bir sürümü. Projenizin hedef çerçevesini API'ye gibi düzeyi ayarlarsanız, **API düzeyi 19 (KitKat)** ve çağırırsanız `SetCategory` kodunuzu yönteminde, bir derleme hatası alırsınız.

Her zaman ile derleme olmasını öneririz *son* kullanılabilir hedef Framework sürümü. Bunun yapılması kodunuz tarafından çağrılan tüm kullanım dışı API'leri ile yararlı uyarı iletilerini sağlar. En son hedef Framework sürümünü kullanarak önemlidir özellikle en son destek kitaplığı Çıkanlar kullandığınızda &ndash; her kitaplık uygulamanızı destek kitaplığın en düşük API düzeyinde derlenmiş veya büyük bir değer bekler. 

> [!NOTE]
> **Not:** Ağustos 2018 başlayarak, Google Play konsol yeni uygulamalar API düzeyi (Android 8.0) 26 hedef gerektirir ya da daha yüksek.
Var olan uygulamaları API düzeyi 26 ya da daha yüksek Kasım 2018'den itibaren hedef gerekecektir. Daha fazla bilgi için bkz: [uygulama güvenliği ve performansı gelmesini Google play'de yıllık geliştirme](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da hedef Framework ayarını erişmek için Proje Özellikleri'nde açın **Çözüm Gezgini** seçip **uygulama** sayfa:

[![Uygulama sayfası proje özellikleri](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png)

Hedef Framework'ü altında açılır menüde bir API düzeyi seçerek ayarlayın **Android sürümüyle derleme** yukarıda gösterildiği gibi.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio hedef Framework ayarını erişmek için proje adına sağ tıklayın ve seçin **seçenekleri**; bu açılır **proje seçenekleri** iletişim. Bu iletişim kutusunda gidin **Yapı > Genel** aşağıda gösterildiği gibi:

[![Proje seçenekleri sayfasının genel bölümü oluşturma](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png)

Hedef Framework'ü sağındaki açılan menüde bir API düzeyi seçerek ayarlayın **hedef framework** yukarıda gösterildiği gibi.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>En düşük Android sürümü

*Minimum Android sürümü* (olarak da bilinen `minSdkVersion`) Android yükleyebilir ve uygulamanızı çalıştırın işletim sistemine (yani, en düşük API düzeyini) eski sürümü. Varsayılan olarak, bir uygulama yalnızca hedef Framework ayarını eşleşen aygıtlarda yüklü ya da daha yüksek olabilir; Minimum Android sürümü ayarı ise *alt* hedef Framework ayarından uygulamanızı de Android önceki sürümlerinde çalıştırabilirsiniz. Örneğin, hedef Framework'ü kümesine **Android 7.1 (Nougat)** ve Minimum Android sürümü **Android 4.0.3 (dondurma Sandwich)**, uygulamanızı API düzeyi 15'nden herhangi bir platformda yüklenebilir API düzeyini 25 ' (dahil) arasındadır.

Uygulamanızı başarıyla oluşturmak ve platformlar üzerinde bu aralıkta yüklemek, ancak bu başarıyla olacağı garanti etmez *çalıştırmak* tüm bu platformlar. Örneğin, uygulamanızın yüklenmesi durumunda **Android 5.0 (Lolipop)** ve yalnızca bir API kodunuzu çağırması **Android 7.1 (Nougat)** ve yeni, uygulamanızı bir çalışma zamanı hatası alın ve büyük olasılıkla kilitlenme. Bu nedenle, kodunuzu emin olmalısınız &ndash; çalışma zamanında &ndash; üzerinde çalıştırıldığı Android cihaz tarafından desteklenen API'lerini çağırır. Diğer bir deyişle, kodunuzu uygulamanızı desteklemek için yeni olan cihazlarda yeni API'ler kullanmasını sağlamak için açık çalışma zamanı denetimleri eklemeniz gerekir.
[Çalışma zamanı denetler Android sürümleri için](#runtimechecks)bu kılavuzda daha sonra kodunuzda bu çalışma zamanı denetimleri ekleme açıklar.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio Minimum Android sürümü ayarında erişmek için Proje Özellikleri'nde açın **Çözüm Gezgini** seçip **Android derleme bildirimi** sayfası. Altında açılan menüde **Minimum Android sürümü** Minimum Android sürümü, uygulamanız için seçebilirsiniz:

[![En düşük Android SDK sürümünü kullanarak derlemek için ayarlanan hedef seçeneği için](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png)

Seçerseniz **SDK sürümü kullanarak kullanım derleme**, Minimum Android sürümü hedef Framework ayarını aynı olacaktır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio hedef Framework ayarını erişmek için proje adına sağ tıklayın ve seçin **seçenekleri**; bu açılır **proje seçenekleri** iletişim. Gidin **Yapı > Android uygulaması**.
Sağındaki açılan menüsünü kullanarak **Minimum Android sürümü**, uygulamanız için Minimum Android sürümü ayarlayabilirsiniz:

[ ![Otomatik - kullanım hedef framework sürümü kümesi minimum Android sürümü](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png)

Seçerseniz **otomatik &ndash; hedef framework sürümünü kullanmak**, Minimum Android sürümü hedef Framework ayarını aynı olacaktır.

-----


<a name="target" />

### <a name="target-android-version"></a>Hedef Android sürümü

*Hedef Android sürümü* (olarak da bilinen `targetSdkVersion`) uygulama burada bekliyor çalıştırmak için Android aygıtına düzeyi API'dir. Android uyumluluk davranışları etkinleştirilip etkinleştirilmeyeceğini belirlemek için bu ayarı kullanır &ndash; bu uygulamanızı beklediğiniz gibi çalışmaya devam etmesini sağlar. Android hedef Android sürümü ayar, uygulamanızın hangi davranış değişiklikleri uygulamanıza (Android ileriye dönük uyumluluğu nasıl sağladığını budur) bozmadan uygulanabilir bulmak için kullanır.

Hedef Framework'ü ve çok benzer adlara sahip sırasında hedef Android sürümü de aynı değildir. Hedef Framework ayarını hedef API düzeyi bilgileri Xamarin.Android kullanılmak üzere iletişim kurar *derleme zamanı*hedef Android sürümü hedef API düzeyi bilgileri Android kullanılmak üzere iletişim kurar yaparken  *çalışma zamanı* (zaman uygulamanın yüklü olduğundan ve bir cihaz üzerinde çalışan).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu ayar Visual Studio'da erişmek için Proje Özellikleri'nde açın **Çözüm Gezgini** seçip **Android derleme bildirimi** sayfası. Altında açılan menüde **hedef Android sürümü** hedef Android sürümü, uygulamanız için seçebilirsiniz:

[![Hedef Android sürümü SDK sürümüyle derlemek için ayarlama](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png)

Hedef Android sürümü açıkça uygulamanızı test etmek için kullandığınız Android son sürüme ayarlamanızı öneririz. İdeal olarak, en son Android SDK sürüm ayarlanmalıdır &ndash; Bu davranış değişiklikleri aracılığıyla çalışma önce yeni API'ları kullanmanızı sağlar. Çoğu geliştiriciler için biz *sağlamadığı* hedef Android sürümü ayarlanması önerilir **SDK sürümü kullanarak kullanım derleme**.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio hedef Framework ayarını erişmek için proje adına sağ tıklayın ve seçin **seçenekleri**; bu açılır **proje seçenekleri** iletişim. Gidin **Yapı > Android uygulaması**.
Sağındaki açılan menüsünü kullanarak **hedef Android sürümü**, hedef Android sürümü, uygulamanız için ayarlayabilirsiniz:

[![Android sürüm değeri otomatik olarak ayarlanmış hedef - hedef framework sürümü kullanın](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png)

Hedef Android sürümü açıkça uygulamanızı test etmek için kullandığınız Android son sürüme ayarlamanızı öneririz. İdeal olarak, kullanılabilir en son Android SDK sürüme ayarlanmalıdır &ndash; Bu davranış değişiklikleri aracılığıyla çalışma önce yeni API'ları kullanmanızı sağlar. Çoğu geliştiriciler için hedef Android sürümü ayarını önermiyoruz **otomatik - kullanım hedef framework sürümü**.

-----

Genel olarak, hedef Android sürümü Minimum Android sürümü ve hedef Framework'ü tarafından ilişkisindeki. Bunun anlamı:

**En düşük Android sürümü < hedef Android sürümü = < hedef Framework =**

Android Geliştirici SDK düzeyleri hakkında daha fazla bilgi için bkz: [kullanır-sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) belgeleri.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Android sürümleri için çalışma zamanı denetimleri

Her yeni Android sürümü gibi yeni sağlamak için API framework güncelleştirilir ya da değiştirme işlevselliği. Birkaç istisna dışında API işlevleri Android önceki sürümlerinden değişiklik olmadan yeni Android sürümler halinde İleri taşınır. Sonuç olarak, uygulamanızı belirli bir Android API düzeyde çalıştırıyorsa, bu genellikle bir sonraki Android API düzeyde değişiklik olmadan çalıştırabilir ve olacaktır. Ancak, ne de Android önceki sürümlerinde uygulamanızı çalıştırmak isterseniz?

Minimum Android sürümü seçerseniz *alt* , hedef Framework ayarından bazı API uygulamanıza çalışma zamanında kullanılamayabilir. Ancak, uygulamanızı hala önceki cihazında ancak azaltılmış işlevlerle çalıştırabilirsiniz. Minimum Android sürümü ayarına karşılık gelen Android platformlarda kullanılamaz her API kodunuzu açıkça değerini denetlemelisiniz `Android.OS.Build.VERSION.SdkInt` özelliği uygulama üzerinde çalışan platform API düzeyini belirler. API düzeyi *alt* aramak istediğiniz API destekler Minimum Android sürümünden daha sonra bu API çağrısı yapmadan düzgün bir şekilde bulmak kodunuzu sahiptir.

Örneğin, şimdi biz kullanmak istediğinizi varsayalım [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) çalışırken bir bildirim kategorilere ayırmak için yöntemi **Android 5.0 Lolipop** (ve daha sonra), ancak yine de uygulamamıza istiyoruz Android önceki sürümlerinde çalıştırmak **Android 4.1 Jelly Çekirdeklere** (burada `SetCategory` kullanılamaz). Bu kılavuzun başında Android sürümü tabloya başvuran, biz görüp derleme sürüm kodunu **Android 5.0 Lolipop** olan `Android.OS.BuildVersionCodes.Lollipop`. Android where eski sürümleri desteklemek için `SetCategory` olan kullanılabilir kodumuza çalışma zamanı API düzeyinde algılayabilir ve koşullu çağrı `SetCategory` yalnızca API düzeyi Lolipop derleme sürüm kodu eşit veya daha büyük olduğunda:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

Bu örnekte, bizim uygulamanın hedef Framework'ü ayarlamak **Android 5.0 (API düzeyi 21)** ve Minimum Android sürümü kümesine **Android 4.1 (API düzeyi 16)**. Çünkü `SetCategory` API düzeyinde kullanılabilir `Android.OS.BuildVersionCodes.Lollipop` ve daha sonra bu kod örneği çağıracak `SetCategory` yalnızca gerçekten kullanılabilir olduğu zaman &ndash; içinde *değil* aramaktadır `SetCategory` zaman API Düzey 16, 17, 18, 19 veya 20 ' dir. İşlevsellik yalnızca düzgün (bunlar türüne göre sınıflandırılır değil çünkü) bildirimleri sıralanır değil, ancak bildirimleri hala kullanıcıyı uyarmak için yayımlanan toplasa bile daha önce bu Android sürümlerinde azalır. Uygulamamıza hala çalışıyor, ancak işlevselliğini biraz düşer.

Genel olarak, derleme sürüm denetimi kodunuzu şeyi yeni yoludur eski şekilde karşı yapmanın arasında çalışma zamanında karar vermenize yardımcı olur. Örneğin:

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

Veya bir veya daha fazla API'leri eksik eski Android sürümlerine çalıştığında, uygulamanızın işlevlerini değiştirmek nasıl açıklayan hızlı ve basit kural yok. Bazı durumlarda (gibi `SetCategory` Yukarıdaki örnek), kullanılabilir olmadığında yalnızca API çağrısı atlamak yeterlidir. Ancak, diğer durumlarda, alternatif işlevselliği için ne zaman uygulamak gerekebilir `Android.OS.Build.VERSION.SdkInt` küçük API uygulamanızı en iyi deneyimi sunmak için ihtiyaç duyduğu düzeyi olduğu algılandı.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API düzeylerini ve kitaplıklar

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

(Örneğin, bir sınıf kitaplığı veya bağlamaları kitaplık) bir Xamarin.Android kitaplığı projesi oluşturduğunuzda, yalnızca hedef Framework ayarını yapılandırabilirsiniz &ndash; Minimum Android sürümü ve hedef Android sürümü ayarlar kullanılamaz. Yoktur çünkü hiçbir **Android derleme bildirimi** sayfa:

[![Yalnızca Android sürümü seçeneği kullanılarak derleme kullanılabilir](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Xamarin.Android kitaplığı projesi oluşturduğunuzda, var olan hiçbir **Android uygulaması** yapılandırabileceğiniz Minimum Android sürümü hem de hedef Android sürümü sayfa &ndash; Minimum Android sürümü ve hedef Android sürüm ayarlar kullanılamaz.
Yoktur çünkü hiçbir **Yapı > Android uygulaması** sayfa):

[ ![Derleme en az ve hedef sürüm seçenekleri olmadan genel sayfası](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png)

-----

Sonuçta elde edilen kitaplık tek başına bir uygulama olmadığından Minimum Android sürümü ve hedef Android sürümü ayarları kullanılabilir olmadığından &ndash; kitaplığı ile paketlenmiştir uygulamaya bağlı olarak herhangi bir Android sürümü çalıştırabilirsiniz. Nasıl kitaplığı olacak belirtebilirsiniz *derlenmiş*, ancak hangi platform API düzeyi kitaplığı üzerinde çalıştırılacak tahmin edilemez. Bu durum dikkate alınarak, aşağıdaki en iyi yöntemler kullanma veya kitaplıkları oluşturma uyulması:

-   **Bir Android kitaplığıdır kullanırken** &ndash; bir Android kitaplığıdır uygulamanızda kullanıyorsa, uygulamanızın hedef Framework'ü bir API düzeyi ayarının ayarladığınızdan emin olun *en az olarak yüksek olarak* hedef Kitaplığın Framework ayarı.

-   **Bir Android kitaplığıdır oluştururken** &ndash; diğer uygulamalar tarafından kullanılmak üzere bir Android kitaplığıdır oluşturuyorsanız, hedef Framework ayarını derlemek için gereken en düşük API düzeyini ayarladığınızdan emin olun.

Bu en iyi yöntemler, bir kitaplık (hangi uygulamanın kilitlenmesine neden olabilir) çalışma zamanında kullanılabilir olmayan bir API çağrısı için girişimde bulunduğu durum önlemeye yardımcı olmak için önerilir. Kitaplık geliştiriciyseniz API çağrıları toplam API yüzey alanını küçük ve tanınmış alt kümesine kullanımınızı kısıtlamak çalışmalarımızı. Bunu yaparsanız, bu nedenle kitaplığınızın daha geniş bir aralığı Android sürümleri arasında güvenli bir şekilde kullanılabilir sağlamaya yardımcı olur.


## <a name="summary"></a>Özet

Bu kılavuz, Android API düzeylerini Android farklı sürümleri arasında uygulama uyumluluğu yönetmek için nasıl kullanıldığı açıklanmıştır. Xamarin.Android yapılandırmak için ayrıntılı adımlar sağlanan *hedef Framework*, *Minimum Android sürümü*, ve *hedef Android sürümü* proje ayarları. SDK paketleri yüklemek için Android SDK Manager'ı kullanma yönergeleri çalışma zamanında farklı API düzeylerini uğraşmanız kodunun nasıl yazılacağını örnekleri dahil ve oluştururken veya Android kitaplıkları tüketen API düzeylerini yönetme konusunda açıklanan sağlanan. Ayrıca, Android sürüm numaralarını (örneğin, Android 4.4), Android sürüm adları (örneğin, Kitkat) ve Xamarin.Android derleme sürüm kodlarını API düzeylerini ilgilidir kapsamlı bir liste sağladığı.


## <a name="related-links"></a>İlgili bağlantılar

- [Android SDK Kurulumu](~/android/get-started/installation/android-sdk.md)
- [SDK CLI araç değiştirir](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Picking your compileSdkVersion, minSdkVersion, and targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [API düzeyi nedir?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenames, etiketler ve yapı numaraları](https://source.android.com/source/build-numbers)
