---
title: Bir uygulama sürüm için hazırlama
ms.prod: xamarin
ms.assetid: 9C8145B3-FCF1-4649-8C6A-49672DDA4159
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2018
ms.openlocfilehash: 238e224a1dfbc17089c8b6d03e78043f77f3f383
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="preparing-an-application-for-release"></a>Bir uygulama sürüm için hazırlama


Bir uygulama kodlanmış ve test sonra paketi dağıtım için hazırlamak üzere gereklidir. Bu paket hazırlama için ilk görev çoğunlukla bazı uygulama özniteliklerini ayarlama kapsar yayın uygulaması oluşturmaktır.

Yayın uygulama oluşturmak için aşağıdaki adımları kullanın:

-   **[Uygulama simgesi belirtme](#Specify_the_Application_Icon)**  &ndash; her Xamarin.Android uygulaması belirtilen uygulama simgesi olmalıdır. Teknik olarak gerekli olmasa da, Google Play gibi bazı Piyasası, ihtiyaç duyar.

-   **[Sürüm uygulama](#Versioning)**  &ndash; başlatılıyor veya sürüm bilgisini güncelleştirirken bu adımı içerir. Bu, gelecekteki uygulama güncelleştirmeleri için ve kullanıcıların uygulamanın hangi sürümünün yüklü farkında olduğundan emin olmak önemlidir.

-   **[APK Küçült](#shrink_apk)**  &ndash; son APK boyutunu yönetilen kod ve ProGuard üzerinde Java bayt Xamarin.Android bağlayıcı kullanarak önemli ölçüde azaltılabilir.

-   **[Uygulama koruma](#protect_app)**  &ndash; önlemek kullanıcıların veya saldırganların hata ayıklama, izinsiz ya da devre dışı bırakarak hata ayıklama, yönetilen kod obfuscating, koruma hata ayıklama ve koruma değiştirilmeye ekleme uygulama mühendislik ters ve Yerel derleme kullanıyor.

-   **[Paketleme özelliklerini ayarlama](#Set_Packaging_Properties)**  &ndash; paketleme özellikleri Android uygulama paketi (APK) oluşturulmasını denetler. Bu adım APK en iyi duruma getirir, kendi varlıkları korur ve gerektiğinde paketleme modularizes.

-   **[Derleme](#Compile)**  &ndash; kod ve varlıkları yayın modunda derlemeler doğrulamak için bu adımı derler.

-   **[Arşiv yayımlama için](#archive)**  &ndash; bu adımı uygulamayı oluşturur ve imzalamak ve yayımlamak için bir arşiv yerleştirir.

Bu adımların her biri aşağıda ayrıntılı olarak açıklanmıştır.

<a name="Specify_the_Application_Icon" />

## <a name="specify-the-application-icon"></a>Uygulama simgesi belirtme

Her Xamarin.Android uygulaması uygulama simgesi belirtme önerilir. Bazı uygulama Pazar biri olmadan yayımlanmasını bir Android uygulaması izin vermez. `Icon` Özelliği `Application` öznitelik, bir Xamarin.Android projesi için uygulama simgesi belirtmek için kullanılır.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 2015 ve daha sonra uygulama simgesi üzerinden belirtin **Android derleme bildirimi** proje bölümünü **özellikleri**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Uygulama simgesi ayarlayın](images/vs/01-application-icon-sml.png)](images/vs/01-application-icon.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da da uygulama simgesi üzerinden belirtmek mümkündür **Android uygulaması** bölümünü **proje seçenekleri**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Uygulama simgesi ayarlayın](images/xs/01-application-icon-sml.png)](images/xs/01-application-icon.png#lightbox)

-----

Bu örneklerde `@drawable/icon` konumda bulunan bir simge dosyası başvurduğu **Resources/drawable/icon.png** (unutmayın **.png** kaynak adının uzantısı dahil değildir). Bu öznitelik dosyasında da bildirilebilir **Properties\AssemblyInfo.cs**, bu örnek parçacığında gösterildiği gibi:

```csharp
[assembly: Application(Icon = "@drawable/icon")]
```

Normalde, `using Android.App` en üstündeki bildirilmiş **AssemblyInfo.cs** (ad alanı `Application` özniteliği `Android.App`); ancak, bu eklemeniz gerekebilir `using` zaten mevcut değilse deyimi.


<a name="Versioning" />

## <a name="version-the-application"></a>Uygulama sürümü

Sürüm oluşturma, Android uygulaması Bakım ve dağıtım için önemlidir. Sürüm oluşturma yerinde çeşit, olmadan belirlemek zordur veya bir uygulamanın nasıl güncelleştirilmelidir. Sürüm oluşturma yardımcı olmak üzere iki farklı tür bilgiler Android tanır: 

-   **Sürüm numarası** &ndash; uygulamanın sürümünü temsil eder (Android ve uygulama tarafından dahili olarak kullanılır) bir tamsayı değeri. Uygulamaların çoğu bu değeri 1'e ayarlanmış olarak başlar ve ardından her bir yapı ile artırılır. Bu değer, herhangi bir ilişki veya benzeşim sürüm adı özniteliğiyle (aşağıya bakın) sahiptir. Bu değer kullanıcılara görüntülememek, uygulamaları ve hizmetleri yayımlama. Bu değer depolanan **AndroidManifest.xml** olarak dosya `android:versionCode`. 

-   **Sürüm adı** &ndash; yalnızca uygulama sürümü hakkında bilgileri kullanıcıya (belirli bir cihazda yüklü olarak) iletişim kurmak için kullanılan bir dize. Sürüm adı, kullanıcılara veya Google Play görüntülenmesi için tasarlanmıştır. Bu dize Android tarafından dahili olarak kullanılmaz. Sürüm adı cihazlarında yüklü yapı tanımlamak kullanıcı yardımcı olacak herhangi bir dize değeri olabilir. Bu değer depolanan **AndroidManifest.xml** olarak dosya `android:versionName`. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da bu değerleri ayarlanabilir ' **Android derleme bildirimi** proje bölümünü **özellikleri**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Sürüm numarasını ayarlama](images/vs/02-versioning-sml.png)](images/vs/02-versioning.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu değerleri aracılığıyla ayarlanabilir **Yapı > Android uygulaması** bölümünü **proje seçenekleri** aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Sürüm numarasını ayarlama](images/xs/02-versioning-sml.png)](images/xs/02-versioning.png#lightbox)

-----

<a name="shrink_apk" />

## <a name="shrink-the-apk"></a>APK Daralt

Xamarin.Android APKs yapılabilir gereksiz kaldırır Xamarin.Android bağlayıcı bir birleşimi küçük *yönetilen* kodunu ve *ProGuard* Android kaldıran SDK, aracı kullanılmayan *Java bayt*. Derleme işlemi Xamarin.Android bağlayıcı ilk yönetilen kod (C#) düzeyinde uygulamayı iyileştirmek için kullanır ve ardından daha sonra ProGuard (etkinse) Java bayt düzeyinde APK iyileştirmek için kullanır.


### <a name="configure-the-linker"></a>Bağlayıcı yapılandırma

Sürüm modu paylaşılan çalışma zamanı devre dışı bırakır ve böylece uygulama yalnızca çalışma zamanında gereken Xamarin.Android parçalarını gelir bağlama üzerinde kapatır. *Bağlayıcı* Xamarin.Android içinde hangi derlemelerin, türler ve tür üyeleri kullanılan veya bir Xamarin.Android uygulaması tarafından başvurulan belirlemek için statik çözümlemesini kullanır. Bağlayıcı sonra tüm kullanılmayan derlemeleri, türleri ve kullanılan (başvurulan veya) üyeleri atar. Bu paket boyutu önemli azalmaya neden olabilir. Örneğin, göz önünde bulundurun [HelloWorld](~/android/deploy-test/linker.md) kendi APK, nihai boyutuna, %83 azalma karşılaştığında örnek: 

-   Yapılandırma: Hiçbiri &ndash; Xamarin.Android 4.2.5 boyutu = 17.4 MB.

-   Yapılandırma: Yalnızca SDK derlemeleri &ndash; Xamarin.Android 4.2.5 boyutu = 3.0 MB.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aracılığıyla bağlayıcı seçeneklerini ayarlama **Android seçenekleri** proje bölümünü **özellikleri**:

[![Bağlayıcı seçenekleri](images/vs/03-linking-sml.png)](images/vs/03-linking.png#lightbox)

**Bağlama** açılır menü bağlayıcı denetlemek için aşağıdaki seçenekleri sağlar:

-   **Hiçbiri** &ndash; bu bağlayıcı devre dışı bırakır; hiçbir bağlama gerçekleştirilir.

-   **SDK derlemeleri yalnızca** &ndash; bu yalnızca olan derlemeleri bağlayacaksınız [Xamarin.Android tarafından gerekli](~/cross-platform/internals/available-assemblies.md). 
    Diğer derlemelerden bağlı değil.

-   **SDK ve kullanıcı derlemeleri** &ndash; bu uygulama tarafından istenen tüm derlemeler ve yalnızca Xamarin.Android tarafından gerekli olanları bağlayacaksınız.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Aracılığıyla bağlayıcı seçeneklerini ayarlama **bağlayıcı** sekmesinde **Android derleme** bölümünü **proje seçenekleri**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Bağlayıcı seçenekleri](images/xs/03-linking-sml.png)](images/xs/03-linking.png#lightbox)

Denetleme bağlayıcı seçenekleri aşağıdaki gibidir:

-   **Bağlantı** &ndash; bu bağlayıcı devre dışı bırakır; hiçbir bağlama gerçekleştirilir.

-   **Bağlantı yalnızca SDK derlemeleri** &ndash; bu yalnızca olan derlemeleri bağlayacaksınız [Xamarin.Android tarafından gerekli](~/cross-platform/internals/available-assemblies.md). Diğer derlemelerden bağlı değil.

-   **Tüm derlemelerde bağlantı** &ndash; bu uygulama tarafından istenen tüm derlemeler ve yalnızca Xamarin.Android tarafından gerekli olanları bağlayacaksınız.

-----

Bir uygulama yayın modunda bir fiziksel cihazda yeniden test edilmiş önemlidir bağlama istenmeyen bazı yan etkileri üretebilir.


### <a name="proguard"></a>ProGuard

*ProGuard* bağlanan bir Android SDK aracıdır ve Java kod bilgisayardan farklı gösterir. ProGuard normalde büyük dahil kitaplıklarında (örneğin, Google Play Hizmetleri), APK ayak azaltarak daha küçük uygulamaları oluşturmak için kullanılır. Sonuçta elde edilen uygulama küçültür kullanılmayan Java bayt ProGuard kaldırır. Örneğin, ProGuard küçük Xamarin.Android uygulamaları genellikle boyutu %24 düşüş hakkında göndermeyle &ndash; ProGuard birden çok kitaplık bağımlılıkları olan daha büyük uygulamalarını genellikle göndermeyle daha da büyük bir boyutunu azaltma. 

Değil Xamarin.Android bağlayıcıya proGuard alternatiftir. Xamarin.Android bağlayıcı bağlantılar *yönetilen* ProGuard bağlantılar Java bayt sırasında kodu. Derleme işlemi Xamarin.Android bağlayıcı ilk uygulama yönetilen (C#) kod iyileştirmek için kullanır ve ardından daha sonra ProGuard (etkinse) Java bayt düzeyinde APK iyileştirmek için kullanır. 

Zaman **etkinleştirmek ProGuard** denetlenir Xamarin.Android elde edilen APK üzerinde ProGuard Aracı'nı çalıştırır. Bir ProGuard yapılandırma dosyası oluşturulur ve derleme zamanında ProGuard tarafından kullanılır. Xamarin.Android ayrıca özel destekler *ProguardConfiguration* Eylemler oluşturabilir. Bir özel ProGuard yapılandırma dosyası projenize ekleyin, sağ tıklatın ve bu örnekte gösterildiği gibi bir yapı eylemi olarak seçin: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Proguard yapı eylemi](images/vs/05-proguard-build-action-sml.png)](images/vs/05-proguard-build-action.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Proguard yapı eylemi](images/xs/05-proguard-build-action-sml.png)](images/xs/05-proguard-build-action.png#lightbox)

-----

ProGuard varsayılan olarak devre dışıdır. **Etkinleştirmek ProGuard** seçeneği kullanılabilir yalnızca proje ayarlandığında **sürüm** modu. Tüm ProGuard yapı eylemleri yoksayılır **etkinleştirmek ProGuard** denetlenir. Xamarin.Android ProGuard yapılandırma APK belirsizleştirirseniz değil ve hatta özel yapılandırma dosyaları ile gizleme, etkinleştirmek mümkün değildir. Gizleme kullanmak istiyorsanız, lütfen bkz [Dotfuscator ile uygulama koruması](~/android/deploy-test/release-prep/index.md#dotfuscator). 

ProGuard Aracı'nı kullanma hakkında daha ayrıntılı bilgi için bkz: [ProGuard](~/android/deploy-test/release-prep/proguard.md).

<a name="protect_app" />

## <a name="protect-the-application"></a>Uygulama koruma

<a name="Disable_Debugging" />

### <a name="disable-debugging"></a>Hata ayıklama devre dışı bırak

Android uygulama geliştirme sırasında hata ayıklama kullanımı ile gerçekleştirilir *Java hata ayıklama kablo protokolü* (JDWP). Bu gibi araçlar sağlayan bir teknolojidir **adb** hata ayıklama amacıyla JVM ile iletişim kurmak için. JDWP Xamarin.Android uygulamasının hata ayıklama yapıları için varsayılan olarak açıktır. Geliştirme sırasında JDWP önemli olmakla birlikte, yayımlanan uygulamalar için bir güvenlik sorunu oluşturabilir. 

> [!IMPORTANT]
> Java işlemi tam erişmek ve bu hata ayıklama durumu devre dışı ise uygulama bağlamında rastgele kod yürütmek için (JDWP) mümkün olduğu gibi her zaman bir yayımlanan uygulama hata ayıklama durumda devre dışı bırakın.

Android derleme bildirimi içeren `android:debuggable` olup olmadığına uygulama hata ayıklaması yapılabilir denetimleri özniteliği. Ayarlamak için iyi bir uygulama olarak kabul edilir `android:debuggable` özniteliğini `false`. Koşullu derleme deyiminde ekleyerek bunu yapmanın en kolay yolu olan **AssemblyInfo.cs**: 

```csharp
#if DEBUG
[assembly: Application(Debuggable=true)]
#else
[assembly: Application(Debuggable=false)]
#endif
```

Otomatik olarak hata ayıklama derlemeleri Not daha kolay hata ayıklama yapmak için bazı izinleri ayarla (gibi **Internet** ve **ReadExternalStorage**). Yayın derlemeleri, ancak yalnızca açıkça yapılandırmanıza izinlerini kullanın. Yayın derlemesine geçme uygulamanızı hata ayıklama derlemesi kullanılabilir bir izin kaybetmenize neden olduğunu fark ederseniz, bu izni açıkça etkinleştirdiyseniz doğrulayın **gerekli izinleri** anlatıldığıgibilistesi[ İzinleri](~/android/app-fundamentals/permissions.md). 
 

<a name="dotfuscator" id="dotfuscator" />

### <a name="application-protection-with-dotfuscator"></a>Uygulama koruması Dotfuscator ile

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bile [devre dışı hata ayıklama](#Disable_Debugging), yeniden ekleyerek veya yapılandırma seçeneklerini veya izinleri kaldırarak bir uygulama paketini saldırganlar için hala mümkündür. Bu ters mühendislik, hata ayıklama ya da yetkisiz değiştirmeye karşı uygulamayla sağlar.
[Dotfuscator Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) yönetilen kod belirsizleştirirseniz ve çalışma zamanı güvenlik durumu algılama kodu derleme zamanında bir Xamarin.Android uygulamasına ekleme için kullanılabilir.

Visual Studio ile Dotfuscator CE eklenmiştir, ancak yalnızca Visual Studio 2015 güncelleştirme 3 (ve üzeri) Xamarin.Android ile çalışmak için doğru sürümde. Dotfuscator kullanmak için tıklatın **Araçlar > PreEmptive tarafından koruması - Dotfuscator**.

Dotfuscator CE yapılandırmak için lütfen bkz [kullanarak Dotfuscator Community Edition Xamarin ile](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator).
Bunu yapılandırıldıktan sonra Dotfuscator CE otomatik olarak oluşturulan her yapı koruyun.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bile [devre dışı hata ayıklama](#Disable_Debugging), yeniden ekleyerek veya yapılandırma seçeneklerini veya izinleri kaldırarak bir uygulama paketini saldırganlar için hala mümkündür. Bu ters mühendislik, hata ayıklama ya da yetkisiz değiştirmeye karşı uygulamayla sağlar.
Mac için Visual Studio desteklemez, ancak kullanabilirsiniz [Dotfuscator Community Edition (CE)](https://www.preemptive.com/products/dotfuscator/overview) yönetilen kod belirsizleştirirseniz ve çalışma zamanı güvenlik durumu algılama kodu derleme zamanında bir Xamarin.Android uygulamasına ekleme için Visual Studio ile .

Dotfuscator CE yapılandırmak için lütfen bkz [kullanarak Dotfuscator Community Edition Xamarin ile](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator).
Bunu yapılandırıldıktan sonra Dotfuscator CE otomatik olarak oluşturulan her yapı koruyun.

-----

<a name="bundle" />

### <a name="bundle-assemblies-into-native-code"></a>Yerel kod içine paket derlemeler

Bu seçenek etkinleştirildiğinde, yerel paylaşılan bir kitaplık derlemeleri paketlenmiştir. Bu seçenek kodu güvenli tutar; Yönetilen derlemelerden yerel ikili dosyalarda gömerek korur.

Bu seçenek bir Enterprise Lisansı gerektirir ve yalnızca kullanılabilir olduğunda **kullanım hızlı dağıtım** devre dışı bırakılır. **Yerel kod derlemelerine paketini** varsayılan olarak devre dışıdır.

Unutmayın **yerel kod içine paket** seçenektir *değil* yerel koda derlenmiş derlemeler anlamına gelir. Kullanmak mümkün değil [ **Uygulama Nesne AĞACI derleme** ](#aot) yerel kod derlemeye derlemesi için (şu anda yalnızca bir deneysel özellik ve üretim için kullanın).

<a name="aot" />

### <a name="aot-compilation"></a>Uygulama Nesne AĞACI derleme

**Uygulama Nesne AĞACI derleme** seçeneği (üzerinde [paketleme özelliklerini](#Set_Packaging_Properties) sayfa) derlemeler zamanı Ahead (Uygulama Nesne AĞACI) derlenmesini sağlar. Bu seçenek etkinleştirildiğinde, yalnızca ın anında (JIT) başlangıç yükünü önce çalışma zamanı derlemeleri önceden derleme en aza indirilir. Sonuçta elde edilen yerel koda derlenmemiş derlemeleri birlikte APK dahil edilir. Bu kısa uygulama başlangıç zamanı, ancak biraz daha büyük APK boyutları ödün verme pahasına olur.

**Uygulama Nesne AĞACI derleme** seçenek, bir Enterprise Lisansı gerektirir ya da daha yüksek. **Uygulama Nesne AĞACI derleme** yalnızca zaman proje sürüm modu için yapılandırılır ve varsayılan olarak devre dışıdır kullanılabilir. Uygulama Nesne AĞACI derleme hakkında daha fazla bilgi için bkz: [Uygulama Nesne AĞACI](http://www.mono-project.com/docs/advanced/aot/).


#### <a name="llvm-optimizing-compiler"></a>LLVM en iyi duruma getirme derleyici

_LLVM en iyi duruma getirme derleyici_ daha küçük ve daha hızlı derlenmiş kod ve yerel koda dönüştürme Uygulama Nesne AĞACI derlenmiş derlemeler oluşturma ancak daha yavaş saatleri sırasında expense, yapı. LLVM derleyici varsayılan olarak devre dışıdır. LLVM derleyici kullanılacak **Uygulama Nesne AĞACI derleme** seçeneği önce etkinleştirilmesi gerekir (üzerinde [paketleme özelliklerini](#Set_Packaging_Properties) sayfası).


> [!NOTE]
> **LLVM en iyi duruma getirme derleyici** seçeneği iş lisansı gerekir.  

<a name="Set_Packaging_Properties" />

## <a name="set-packaging-properties"></a>Paketleme özelliklerini ayarlama

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Paketleme özellikler ayarlanabilir **Android seçenekleri** proje bölümünü **özellikleri**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Paketleme özellikleri](images/vs/04-packaging-sml.png)](images/vs/04-packaging.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Paketleme özellikler ayarlanabilir **proje seçenekleri**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Paketleme özellikleri](images/xs/04-packaging-sml.png)](images/xs/04-packaging.png#lightbox)

-----

Bu özelliklerin çoğu gibi **kullanım çalışma zamanı paylaşılan**, ve **kullanım hızlı dağıtım** hata ayıklama modu için tasarlanmıştır. Ancak, uygulama sürüm modu için yapılandırıldığında, vardır nasıl uygulama belirlemek diğer ayarları [boyutunu ve yürütme hızı için en iyi duruma getirilmiş](#shrink_apk), [oynama yapmasını nasıl korumalı](#protect_app)ve nasıl farklı mimari ve boyut kısıtlamaları desteklemek için paketlenebilir.


### <a name="specify-supported-architectures"></a>Desteklenen mimariler belirtin

Bir Xamarin.Android uygulaması sürüm için hazırlık yaparken, desteklenen CPU mimarileri belirtmek gereklidir. Tek bir APK makine birden çok desteklemek için içerebilecek farklı mimari. Bkz: [CPU mimarileri](~/android/app-fundamentals/cpu-architectures.md) birden fazla CPU mimari destekleme hakkında ayrıntılar için.


### <a name="generate-one-package-apk-per-selected-abi"></a>Bir paket oluştur (. APK) seçili ABI başına

Bu seçenek etkinleştirildiğinde, bir APK her desteklenen ABI's için oluşturulacak (seçili **Gelişmiş** sekmesinde, açıklandığı gibi [CPU mimarileri](~/android/app-fundamentals/cpu-architectures.md)) tüm desteklenen tek, büyük bir APK yerine ABI ait. Bu seçenek, yalnızca proje sürüm modu için yapılandırıldığında ve varsayılan olarak devre dışıdır.


### <a name="multi-dex"></a>Multi-Dex

Zaman **etkinleştirmek çok Dex** seçeneği etkin olduğunda, Android SDK Araçları 65 K yöntemi sınırını atlamak için kullanılan **.dex** dosya biçimi. 65K yöntemi sınırlaması Java yöntemleri sayısına göre bir uygulama _başvuruları_ (uygulama bağımlı kitaplıkları de dahil) &ndash; olan yöntemlerini sayısına göre değil _yazılan Kaynak kodu_. Uygulamanın yalnızca birkaç yöntemleri tanımlar ancak birçok kullanır (veya büyük kitaplıklar) 65 K sınırı aşıldı mümkündür.

Bir uygulama her yöntem, başvurulan her Kitaplığı'nda kullanmıyor mümkündür; Bu nedenle, ProGuard (yukarıya bakın) gibi bir araç koddan kullanılmayan yöntemleri kaldırabilirsiniz mümkündür. Etkinleştirmek için en iyi uygulamadır **etkinleştirmek çok Dex** yalnızca kesinlikle gerekli olduğunda i.e.the uygulama hala birden fazla 65 K Java yöntemleri ProGuard hatta kullanıldıktan sonra başvuruyor.

Çok Dex hakkında daha fazla bilgi için bkz: [yapılandırma uygulamalarla 64 K yöntemleri üzerinden](http://developer.android.com/tools/building/multidex.html).

<a name="Compile" />

## <a name="compile"></a>Derleme

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Yukarıdaki adımların tümünü tamamlandıktan sonra uygulama derleme için hazırdır. Seçin **Yapı > çözümü yeniden derle** onu başarıyla yayın modunda derlemeleri doğrulanamadı. Bu adım bir APK henüz üretmez unutmayın.

[Uygulama paketi imzalama](~/android/deploy-test/signing/index.md) paketleme ve imzalama daha ayrıntılı olarak anlatılmaktadır.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Yukarıdaki adımların tümünü tamamlandıktan sonra uygulamayı derleyin (seçin **Yapı > Yapı tüm**) onu başarıyla yayın modunda derlemeleri doğrulanamadı. Bu adım bir APK henüz üretmez unutmayın.

-----


<a name="archive" />

## <a name="archive-for-publishing"></a>Arşiv yayımlama

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Yayımlama işlemine başlamak için'nde projeye sağ **Çözüm Gezgini** seçip **arşiv...**  bağlam menüsü öğesini:

[![Arşiv uygulama](images/vs/07-archive-for-publishing-sml.png)](images/vs/07-archive-for-publishing.png#lightbox)

**Arşiv...**  başlatır **arşiv Yöneticisi** ve bu ekran görüntüsünde gösterildiği gibi uygulama paketi arşivleme işlemi başlar:

[![Archive Manager](images/vs/08-archive-manager-sml.png)](images/vs/08-archive-manager.png#lightbox)

Bir arşiv oluşturmak için başka bir çözüme sağ yoldur **Çözüm Gezgini** seçip **arşiv tüm...** , çözüm oluşturan ve bir arşiv oluşturabilirsiniz tüm Xamarin projeleri arşivler:

[![Tüm arşiv](images/vs/09-archive-all-sml.png)](images/vs/09-archive-all.png#lightbox)


Her ikisi de **arşiv** ve **arşiv tüm** otomatik olarak başlatma **arşiv Yöneticisi**. Başlatmak için **arşiv Yöneticisi** doğrudan tıklatın **Araçlar > Arşiv Yöneticisi...**  menü öğesi:

[![Arşiv Yöneticisi'ni başlatın](images/vs/10-launch-archive-manager-sml.png)](images/vs/10-launch-archive-manager.png#lightbox)

Çözümün arşivler sağ tıklayarak herhangi bir zamanda **çözüm** düğümü ve seçerek **görünüm arşivler**:

[![Görünüm arşivler](images/vs/11-view-archives-sml.png)](images/vs/11-view-archives.png#lightbox)

### <a name="the-archive-manager"></a>Arşiv Yöneticisi

**Arşiv Yöneticisi** oluşur bir **çözüm listesi** bölmesinde, bir **arşivler listesi**ve bir **ayrıntılar bölmesini**:

[![Arşiv Yöneticisi bölmeleri](images/vs/12-archive-manager-detail-sml.png)](images/vs/12-archive-manager-detail.png#lightbox)

**Çözüm listesi** en az bir arşivlenmiş projesine sahip tüm çözümleri görüntüler. **Çözüm listesi** aşağıdaki bölümleri içerir:

* **Geçerli çözüme** &ndash; geçerli çözümü görüntüler. Bu alan geçerli çözüme varolan bir arşiv yoksa boş olabileceğine dikkat edin.
* **Tüm arşivler** &ndash; bir arşiv sahip tüm çözümleri görüntüler.
* **Arama** (üst) metin kutusuna &ndash; listelenen çözümleri filtreler **tüm arşivler** metin kutusuna girilen arama dizesi göre listesi.

**Arşivler listesi** seçilen çözüm için tüm arşivler listesini görüntüler. **Arşivler listesi** aşağıdaki bölümleri içerir:

* **Seçilen çözüm adı** &ndash; seçili çözüm adını görüntüler **çözüm listesi**. Gösterilen tüm bilgileri **arşivler listesi** seçili bu çözüme başvuruyor.
* **Platformlar filtre** &ndash; Bu alan arşivler platform türünü (örneğin, iOS veya Android) tarafından filtre mümkün kılar.
* **Arşivle** &ndash; seçili çözümü arşivler listesi. Bu listedeki her bir öğeyi proje adı, oluşturulma tarihi ve platformu sunar. Bir öğe yüklenirken da ilerleme gibi ek bilgileri gösterebilirsiniz arşivlenen veya yayımlandı.

**Ayrıntılar bölmesini** her Arşiv hakkında ek bilgiler görüntüler. Ayrıca, dağıtım iş akışını başlatmak veya dağıtım bir nerede oluşturulacağını klasörü açmak kullanıcının sağlar. **Yapı açıklamaları** bölüm arşive yapı açıklamaları eklemek mümkün kılar.

### <a name="distribution"></a>Dağıtım

Uygulamanın Arşivlenmiş bir sürümü yayımlamaya hazır olduğunda arşive seçme **arşiv Yöneticisi** tıklatıp **Dağıt...**  düğmesi:

[![Dağıt düğmesi](images/vs/13-distribute-sml.png)](images/vs/13-distribute.png#lightbox)

**Dağıtım kanal** iletişim uygulaması, bir dağıtım iş akışı İlerleme göstergesi ve dağıtım kanallarına seçimine hakkında bilgileri gösterir. İlk çalıştırılmasında iki seçenek sunulur:

[![Dağıtım kanalı seçin](images/vs/14-distribution-channel-sml.png)](images/vs/14-distribution-channel.png#lightbox)

Aşağıdaki dağıtım kanallarına birini mümkündür:

* **Geçici** &ndash; imzalı APK Android cihazlara dışarıdan yüklenebilir diske kaydeder. Devam [uygulama paketi imzalama](~/android/deploy-test/signing/index.md) kimlik imzalama Android oluşturmayı öğrenmek için Android uygulamaları için yeni bir imzalama sertifikası oluşturma ve yayımlama bir _geçici_ diske uygulamasının sürümü. Test etmek için bir APK oluşturmak için en iyi yolu budur.

* **Google Play** &ndash; imzalı APK Google Play yayımlar. Devam [Google Play yayımlama](~/android/deploy-test/publishing/publishing-to-google-play/index.md) oturum ve Google Play Mağazası'nda bir APK yayımlama öğrenin.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Yayımlama işlemi başlatmak için seçin **Yapı > Arşiv yayımlama**:

[![Arşiv yayımlama](images/xs/07-archive-for-publishing-sml.png)](images/xs/07-archive-for-publishing.png#lightbox)

**Arşiv yayımlama** projesi oluşturur ve bir arşiv dosyasına sunmaktadır. **Arşiv tüm** menü seçeneğini arşivler Çözümdeki tüm archivable projeler. Her iki seçenek otomatik olarak açılmasını **arşiv Yöneticisi** yapı ve paketleme işlemleri tamamlandığında:

[![Arşiv görünümü](images/xs/08-archives-view-sml.png)](images/xs/08-archives-view.png#lightbox)

Bu örnekte, **arşiv Yöneticisi** bir uygulama, arşivlenen yalnızca listeler **Uygulamam**. Açıklama alanı arşive kaydedilmesi kısa bir açıklama sağlar dikkat edin. Bir Xamarin.Android uygulaması arşivlenen bir sürümünü yayımlamak için uygulamada seçin **arşiv Yöneticisi** tıklatıp **oturum ve Dağıt...**  yukarıda gösterildiği gibi. Elde edilen **oturum ve Dağıt** iletişim iki seçenek sunar:

[![Oturum ve dağıtın](images/xs/09-sign-and-distribute-sml.png)](images/xs/09-sign-and-distribute.png#lightbox)


Buradan, dağıtım kanalı seçin mümkündür:

-   **Geçici** &ndash; imzalı APK, diske kaydeder, böylece Android cihazlara dışarıdan yüklenebilir. Devam [uygulama paketi imzalama](~/android/deploy-test/signing/index.md) kimlik imzalama Android oluşturmayı öğrenmek için Android uygulamaları için yeni bir imzalama sertifikası oluşturma ve yayımlama bir &ldquo;geçici&rdquo; diske uygulamasının sürümü. Test etmek için bir APK oluşturmak için en iyi yolu budur.


-   **Google Play** &ndash; imzalı APK Google Play yayımlar.
    Devam [Google Play yayımlama](~/android/deploy-test/publishing/publishing-to-google-play/index.md) oturum ve Google Play Mağazası'nda bir APK yayımlama öğrenin.

-----


## <a name="related-links"></a>İlgili bağlantılar

- [Çok çekirdekli aygıtlar ve Xamarin.Android](~/android/deploy-test/multicore-devices.md)
- [CPU Mimarileri](~/android/app-fundamentals/cpu-architectures.md)
- [AOT](http://www.mono-project.com/docs/advanced/aot/)
- [Kaynaklar ve kod daraltma](http://developer.android.com/tools/help/proguard.html)
- [Üzerinde 64 K yöntemleriyle uygulamaları yapılandırma](http://developer.android.com/tools/building/multidex.html)
