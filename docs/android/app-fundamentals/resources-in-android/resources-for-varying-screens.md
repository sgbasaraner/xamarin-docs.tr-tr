---
title: Kaynaklar için değişen ekranlar oluşturma
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a98d65c6c04ae2400572a2405d487035ac466678
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-resources-for-varying-screens"></a>Kaynaklar için değişen ekranlar oluşturma

Android kendisini her çok çeşitli çözümler, ekran boyutlarına ve ekran densities sahip birçok farklı cihazlarda çalışır. Android ölçekleme ve bu aygıtlar üzerinde çalışmak, uygulamanızın yapmak için yeniden boyutlandırma gerçekleştirir, ancak bu bir alt en iyi kullanıcı deneyimi neden olabilir. Örneğin, görüntüleri bulanık görünebilir, görüntüleri hangi nedenler konumunu kullanıcı Arabirimi öğeleri yerleşiminde üst üste veya çok büyüklüğü şu ana kadar çok fazla (veya yeterli) ekran alan kaplar.


## <a name="concepts"></a>Kavramlar

Birkaç terimleri ve kavramları birden çok ekran desteklemek için anlamak önemlidir.

- **Ekran boyutu** &ndash; uygulamanızı görüntülemek için fiziksel alan miktarı

- **Ekran yoğunluğu** &ndash; herhangi belirli bir alanda ekrandaki piksel sayısı. Tipik ölçü nokta / inç (dpi) birimidir.

- **Çözümleme** &ndash; ekrandaki piksel toplam sayısı. Uygulamaları geliştirirken, çözümleme ekran boyutu ve yoğunluklu gibi önemli değildir.

- **Yoğunluğu bağımsız piksel (dp)** &ndash; bu tasarlanmalıdır düzenleri yoğunluğu bağımsız izin vermek için bir sanal ölçü birimidir. Ekran piksel dp dönüştürmek için aşağıdaki formülü kullanılır:

    px &equals; dp &times; dpi &divide; 160

- **Yönlendirme** &ndash; ekran yönünü uzunluğundan daha geniş olduğunda yatay olarak kabul edilir. Ekran, genişliğinden buna karşılık, dikey yönde olur. Kullanıcı aygıtı döndüğü gibi yönlendirmesini uygulama ömrü boyunca değiştirebilirsiniz.

Bu kavramları ilk üç arası ilişkili olduğunu fark &ndash; yoğunluğu artırma ekran boyutunu artıracaktır olmadan çözümleme artırma. Yoğunluğu ve çözümleme artar, ancak ardından ekran boyutu değişmeden kalabilir. Bu ilişki ekran boyutu, yoğunluğu ve çözümleme arasındaki ekran desteği çok hızlı bir şekilde zorlaştırabilir.

Bu karmaşıklık ile ilgilenir yardımcı olması için Android framework kullanmayı tercih eder *yoğunluğu bağımsız piksel (dp)* ekranı düzeni için. Bağımsız piksel yoğunluğu kullanarak, kullanıcı Arabirimi öğeleri ekranında farklı densities ile aynı fiziksel boyutuna sahip kullanıcıya görünür.


## <a name="supporting-various-screen-sizes-and-densities"></a>Çeşitli ekran boyutlarına ve Densities destekleme

Android düzenleri her ekran yapılandırması için düzgün işlenecek işlerin çoğunu işler. Bununla birlikte, sistem yardımcı olması için geçen bazı eylemler vardır.

Kullanmak yerine düzenleri gerçek piksel yoğunluğu bağımsız piksel yoğunluğu bağımsızlığı emin olmak için çoğu durumda yeterlidir.
Android drawables çalışma zamanında uygun boyuta ölçeklendirir.
Ancak, bu ölçeklendirme bulanık bit eşlemler neden mümkündür. Bunu önlemek için farklı densities için alternatif kaynakları sağlamak gerekli olabilir. Birden çok çözümleri ve daha kolay kanıtlamak ekran densities cihazlarını tasarlarken başlamak yoğunluğu ve daha yüksek çözünürlükte görüntüleri ve ölçeklendirmeyi azaltın. Bu, tüm bulanıklaştırma veya yeniden boyutlandırmasını sonuçlanabilir bozulma engeller.


### <a name="declare-the-screen-size-the-application-supports"></a>Uygulamanızın desteklediği ekran boyutu bildirme

Ekran boyutu bildirme yalnızca desteklenen aygıtlar uygulamasını indirebilirsiniz sağlar. Bu ayar gerçekleştirilir [destekler ekranlar](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) öğesinde **AndroidManifest.xml** dosya. Bu öğe, ne ekran boyutlarına uygulaması tarafından desteklenen belirtmek için kullanılır. Belirli bir ekran uygulama düzgün yapabiliyorsanız desteklendiği kabul ekranı doldurmak üzere düzenleri kullanıcının. Bu liste öğesi kullanarak, uygulama içinde gösterilmez [ *Google Play* ](https://play.google.com/) ekran belirtimleri karşılamayan cihazlar. Ancak, uygulama hala desteklenmeyen ekranlar ile cihazlarda çalışacak, ancak düzenleri bulanık görünebilir ve pixelated.

Xamarin.Android bunun için ilk olarak eklemek gerekli bir **AndroidManifest.xml** zaten yoksa, projeye dosya:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-vs-sml.png)](resources-for-varying-screens-images/01-android-manifest-vs.png#lightbox)

**AndroidManifest.xml** eklenen **özellikleri** dizin. Dosya sonra içerecek şekilde düzenlenebilir [destekler ekranlar](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![Destekler ekranlar ekleme](resources-for-varying-screens-images/02-adding-supports-screens-vs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-xs-sml.png)](resources-for-varying-screens-images/01-android-manifest-xs.png#lightbox)

**AndroidManifest.xml** eklenen **özellikleri** dizin. Dosya sonra içerecek şekilde düzenlenebilir [destekler ekranlar](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![Destekler ekranlar ekleme](resources-for-varying-screens-images/02-adding-supports-screens-xs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-xs.png#lightbox)

-----

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>Farklı ekran boyutları için alternatif düzenleri sağlayın

Android ekran boyutu according boyutlanır karşın, bu bazı durumlarda yeterli olmayabilir. Bazı kullanıcı Arabirimi öğeleri daha büyük ekranda boyutunu artırın veya daha küçük bir ekran için kullanıcı Arabirimi öğeleri konumlandırma değiştirmek için sahip olması istenebilir.

API düzeyi 13 (Android 3.2) itibaren ekran boyutlarına lehinde sw kullanarak kullanım dışıdır*N*dp niteleyicisi. Bu yeni niteleyicisi alanı belirli bir düzen gereken bildirir. Android 3.2 veya sonraki sürümünü çalıştırması gerekmektedir uygulamaları daha yeni bu niteleyicileri kullanarak, önerilir.

Örneğin, bir düzen ekran genişliğinin en az bir 700dp gerekirse, bir klasördeki alternatif düzeni geçecek **düzeni sw700dp**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Yerleşim klasörü 700dp ekran genişliği](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Yerleşim klasörü 700dp ekran genişliği](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


Kılavuz olarak, çeşitli aygıtlar için bazı sayılar şunlardır:

- **Tipik telefon** &ndash; 320dp: tipik bir telefon

- **5" tablet /"tweener"cihaz** &ndash; 480dp: Samsung Not gibi

- **7" tablet** &ndash; 600dp: Barnes gibi &amp; Noble Nook

- **10" tablet** &ndash; 720dp: Motorola Xoom gibi

Uygulamalar için hedef API 12 (Android 3.1) düzeyleri düzenleri niteleyicileri kullan dizinlerde gitmesi gereken **küçük**/**normal**/**büyük**  / **xlarge** çoğu cihazları kullanılabilen çeşitli ekran boyutlarına genelleştirme olarak. Örneğin, aşağıdaki resimde dört farklı ekran boyutlarına için alternatif kaynakları vardır:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dört ekran boyutlarına için diğer kaynaklar](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Dört ekran boyutlarına için diğer kaynaklar](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

Eski öncesi API düzeyi 13 ekran boyutu niteleyicileri yoğunluğu bağımsız piksel olarak nasıl karşılaştırmak karşılaştırmak verilmiştir:

- 426dp x 320dp olan **küçük**

- 470dp x 320dp olan **normal**

- 640dp x 480dp olan **büyük**

- 960dp x 720dp olan **xlarge**

Daha yeni ekran API düzeyinde 13 niteleyicileri boyut ve yukarı API düzeyinden 12 ve alt eski ekran niteleyicileri daha yüksek önceliğe sahiptir.
Eski ve yeni API düzeylerini yayılan uygulamalar için aşağıdaki ekran görüntüsünde gösterildiği gibi iki niteleyicileri kullanarak diğer kaynakları oluşturmak için gerekli olabilir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Her iki niteleyicileri ile diğer kaynaklar](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Her iki niteleyicileri ile diğer kaynaklar](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>Farklı ekran Densities için farklı bit eşlemler sağlayın

Android bir aygıt için gerekli eşlemleri ölçeklendirilmesine rağmen bit eşlemler kendilerini zarif bir şekilde yukarı veya aşağı ölçeklendirme değil: belirsiz ya da bulanık duruma gelebilir. Bit eşlemler ekran yoğunluğu için uygun sağlayarak bu sorunu azaltmak.

Örneğin, aşağıdaki resimde, oluşabilecek düzenini ve görünümünü sorunları örneğidir yoğunluğu belirttiğinizde kaynakları sağlanmadı.

![Ekran görüntüleri olmadan yoğunluğu kaynakları](resources-for-varying-screens-images/06-density-not-provided.png)

Bu, yoğunluğu belirli kaynaklarla tasarlanmış bir düzen Karşılaştırılacak:

![Yoğunluğu özgü kaynaklarla ekran görüntüleri](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>Android varlık Studio ile değişen yoğunluğu kaynakları oluşturun

Bu bit eşlemler, çeşitli densities oluşturulması biraz can sıkıcı olabilir. Bu nedenle, Google adlı bu bit eşlemler oluşturma ile ilgili birçoğunu bazıları azaltabilir bir çevrimiçi yardımcı oluşturdu [ **Android varlık Studio**](https://romannurik.github.io/AndroidAssetStudio/).

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

Bu Web sitesine bir görüntü sağlayarak dört ortak ekran densities hedef bit eşlemler oluşturulmasına yardımcı olur. Android varlık Studio sonra bit eşlemler ile bazı özelleştirmeler oluşturmak ve bunları bir zip dosyası olarak yüklenmesine izin.


## <a name="tips-for-multiple-screens"></a>Birden çok ekranlar için ipuçları

Android bewildering çok sayıda cihaz üzerinde çalışır ve ekran boyutlarına ve ekran densities birleşimi bunaltıcı gibi görünebilir. Aşağıdaki ipuçları, çeşitli aygıtları desteklemek için gereken çabasını en aza yardımcı olabilir:

- **Yalnızca tasarım ve geliştirme için hangi gerek** &ndash; çok farklı cihaz ölçeklendiriyor vardır, ancak bazı tasarlayıp için çaba çok sürebilir nadir form faktörleri mevcut. [ **Ekran boyutu ve yoğunluğu** ](http://developer.android.com/resources/dashboard/screens.html) Pano ekranı boyutu/ekran yoğunluğu matris bir dökümünü üzerinde verileri sağlayan Google tarafından sağlanan bir sayfa olduğu. Bu çözümleme ekranlar destekleme üzerinde geliştirme çalışmasını nasıl hakkında bilgi sağlar.

- **Piksel yerine DPs kullanmak** -piksel ekran yoğunluğu değiştikçe sorunlu olur. Değil stillerinizin piksel değerlerini yapın. Piksel dp (yoğunluğu bağımsız piksel) lehinde kaçının.

- **Kaçının** [AbsoluteLayout](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **yerde olası** &ndash; API Düzey 3 (Android 1.5) kullanım dışıdır ve kırılır düzenleri neden olur. Kullanılmamalıdır. Bunun yerine, daha esnek düzeni pencere öğeleri gibi kullanmaya çalıştığınızda [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/), [ **RelativeLayout**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), ya da yeni [ **GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/).

- **Varsayılan olarak bir düzen yönünü seçin** &ndash; alternatif kaynak sağlanmasını yerine Örneğin, **düzeni kara** ve **Düzen-port**, kaynaklarını yerleştirin içinde yatay **düzeni**ve dikey kaynaklarını **Düzen-port**.

- **Yükseklik ve genişlik için LayoutParams kullanmak** - kullanıcı Arabirimi öğeleri bir XML düzeni dosyasında tanımlarken kullanarak Android uygulama **wrap_content** ve **fill_parent** değerleri daha başarılı olacaktır uygun bir görünüm piksel veya yoğunluğu bağımsız birim kullanmaktan farklı aygıtlar arasında emin olun. Bu boyut değerler ölçek bit eşlem kaynaklarına uygun şekilde Android neden olur. Bu aynı nedenden dolayı yoğunluğu bağımsız birimler için en iyi ayrılmış kenar boşluklarını belirtme ve kullanıcı Arabirimi öğeleri doldurma.


## <a name="testing-multiple-screens"></a>Birden çok ekran test etme

Bir Android uygulamasını desteklenecek tüm yapılandırmaları karşı test edilmesi gerekir. İdeal olarak cihazları gerçek cihazlarda kendilerini test edilmesi ancak çoğu durumda bu olası veya kullanışlı değildir.
Bu durumda her aygıt yapılandırması için Android sanal cihaz kurulumu ve öykünücüsü kullanımını yararlı olacaktır.

Android SDK kaplamaları AVD'ın oluşturmak için kullanılabilir bazı öykünücüsü boyutu, yoğunluğu ve birçok aygıt çözünürlüğü çoğaltılacak sağlar.
Birçok donanım satıcıları benzer şekilde, cihazlarını kaplamaları sağlar.

Başka bir seçeneği bir üçüncü taraf hizmeti test etme Hizmetleri kullanmaktır.
Bu hizmetler bir APK alın, birçok farklı cihazlarda çalıştırmak ve nasıl çalıştığı uygulama geri bildirim sağlayın.
