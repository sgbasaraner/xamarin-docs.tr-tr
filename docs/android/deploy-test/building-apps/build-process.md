---
title: Derleme işlemi
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: bf8dfb43115806f28935c6dec0ebd2d6d7bd2cdc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998266"
---
# <a name="build-process"></a>Derleme işlemi


## <a name="overview"></a>Genel Bakış

Xamarin.Android derleme işlemi, her şeyi birlikte yapıştırmak için sorumludur: [oluşturma `Resource.designer.cs` ](~/android/internals/api-design.md), destekleyici `AndroidAsset`, `AndroidResource`ve diğer [derleme eylemleri](#Build_Actions), oluşturma [Android çağrılabilir sarmalayıcıları](~/android/platform/java-integration/android-callable-wrappers.md)ve oluşturma bir `.apk` Android cihazlarda yürütme için.


## <a name="application-packages"></a>Uygulama paketleri

Geniş bağlamında, iki tür Android uygulama paketi vardır (`.apk` dosyaları) hangi Xamarin.Android yapı sistemi oluşturabilirsiniz: 

-   **Yayın** tamamen müstakil ve yürütmek için ek paketleri gerektirmeyen derlemeler. Bu, verilen paketlerin bir uygulama mağazasına. 

-   **Hata ayıklama** olmayan derlemeler. 

Tesadüfen değil, bu MSBuild eşleşen `Configuration` paket oluşturur.

### <a name="shared-runtime"></a>Paylaşılan çalışma zamanı

*Paylaşılan çalışma zamanı* temel sınıf kitaplığı sağlayan ek Android paketleri çiftinin (`mscorlib.dll`, vs.) ve Android bağlama kitaplığı (`Mono.Android.dll`vb..). Hata ayıklama yerine daha küçük olacak şekilde hata ayıklama paketi verme bağlama derlemeleri Android uygulama paketi içinde ve temel sınıf kitaplığı dahil olmak üzere paylaşılan çalışma zamanı derlemeleri kullanır.

Paylaşılan çalışma zamanı devre dışı bırakılabilir ayarlayarak hata ayıklama yapılarında `$(AndroidUseSharedRuntime)` özelliğini `False`. 

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>Hızlı Dağıtım

*Hızlı Dağıtım* paylaşılan çalışma zamanı Android uygulama paketi boyutu daha da daraltmak için birlikte çalışır. Bu, paket içindeki uygulamanın derlemeleri paketleme değil tarafından gerçekleştirilir. Hedef bunun yerine, kopyaladığınız `adb push`. Derleme/dağıtma/debug döngüsü, çünkü bu işlemini hızlandırır, *yalnızca* derlemeleri değiştirilirse, paket değil yüklenir. Bunun yerine, yalnızca güncelleştirilmiş derlemeleri hedef cihaza yeniden eşitlenen. 

Hızlı Dağıtım block cihazlarınıza başarısız olmasına bilinen `adb` dizine eşitlenmesini `/data/data/@PACKAGE_NAME@/files/.__override__`. 

Hızlı Dağıtım, varsayılan olarak etkindir ve devre dışı bırakılmış hata ayıklama derlemeleri ayarlayarak `$(EmbedAssembliesIntoApk)` özelliğini `True`.


## <a name="msbuild-projects"></a>MSBuild projeleri

Ayrıca Mac ve Visual Studio için Visual Studio tarafından kullanılan proje dosyası biçimi olan MSBuild, Xamarin.Android yapı işlemi dayanır.
Normalde, kullanıcıların MSBuild dosyaları el ile düzenlemeniz gerekir değil &ndash; IDE tam işlevsel projeleri oluşturur ve bunları yapılan değişiklikler ile güncelleştirir ve yapı hedefleri gerektiğinde otomatik olarak çağırır. 

Gelişmiş kullanıcılar, doğrudan proje dosyasını düzenleyerek derleme işlemi özelleştirilebilir, bu nedenle, IDE'nin GUI tarafından desteklenmeyen şeyler isteyebilirsiniz. Bu sayfa yalnızca Xamarin.Android özgü özellikler ve özelleştirmeler belgeleri &ndash; pek çok şey normal MSBuild öğeleri, özellikleri ve hedefleri ile mümkündür. 

<a name="Build_Targets" />

## <a name="build-targets"></a>Hedefleri derleme

Aşağıdaki yapı hedeflerini Xamarin.Android projeleri için tanımlanır:

-   **Derleme** &ndash; paket oluşturur.

-   **Temiz** &ndash; yapı işlemi tarafından oluşturulan bütün dosyaları kaldırır.

-   **Yükleme** &ndash; varsayılan cihaz veya sanal cihazın üzerine paketi yükler.

-   **Kaldırma** &ndash; paketi varsayılan cihaz veya sanal bir CİHAZDAN kaldırır.

-   **SignAndroidPackage** &ndash; oluşturur ve paket imzalar (`.apk`). İle kullanma `/p:Configuration=Release` müstakil "Sürüm" paketleri oluşturmak için.

-   **UpdateAndroidResources** &ndash; güncelleştirmeleri `Resource.designer.cs` dosya. Yeni kaynaklar projeye eklendiğinde bu hedef genellikle IDE tarafından çağrılır.


## <a name="build-properties"></a>Yapı Özellikleri

MSBuild özellikleri, hedefleri davranışını denetler. Örneğin proje dosyası içinde belirtilen **MyApp.csproj**dahilinde bir [MSBuild PropertyGroup öğesi](https://docs.microsoft.com/visualstudio/msbuild/propertygroup-element-msbuild).

-   **Yapılandırma** &ndash; "Debug" veya "Sürüm" gibi kullanmak için yapı yapılandırmasını belirtir. Yapılandırma özelliği, hedef davranışını belirleyen diğer özellikler için varsayılan değerleri belirlemek için kullanılır. Ek yapılandırmalar, IDE içinde oluşturulabilir.

    *Varsayılan olarak*, `Debug` yapılandırma sonuçlanır `Install` ve `SignAndroidPackage` hedefleri diğer dosyaları varlığını gerektiren daha küçük bir Android paketi ve çalışılacak paketleri oluşturma.

    Varsayılan `Release` yapılandırma sonuçlanır `Install` ve `SignAndroidPackage` hedefleri olan bir Android paketi oluşturuluyor *tek başına*ve herhangi bir paket veya dosyaları yüklemeden kullanılabilir.

-   **DebugSymbols** &ndash; Android paketi olup olmadığını belirleyen bir Boole değeri *hata ayıklanabilir*, birlikte `$(DebugType)` özelliği. Hata ayıklanabilir paket hata ayıklama sembolleri, kümeleri içeren `//application/@android:debuggable` özniteliğini `true`ve otomatik olarak ekler `INTERNET` izni böylece bir hata ayıklayıcı işleme iliştirilebilir. Hata ayıklanabilir uygulama varsa `DebugSymbols` olduğu `True` *ve* `DebugType` ya da boş dize veya `Full`.

-   **DebugType** &ndash; belirtir [hata ayıklama sembolleri türünü](https://docs.microsoft.com/visualstudio/msbuild/csc-task) hata ayıklanabilir uygulama olup olmadığını da etkilenir yapı işleminin bir parçası olarak oluşturulacak. Olası değerler şunlardır:

    - **Tam**: tam sembolleri oluşturulur. Varsa `DebugSymbols` MSBuild özelliği de `True`, uygulama paketini hata ayıklanabilir kaldırılır.

    - **PdbOnly**: "PDB" sembolleri oluşturulur. Uygulama paketi olacak *değil* hataları ayıklanabilir olmayacak.

    Varsa `DebugType` ayarlanmadı veya boş bir dize ise sonra `DebugSymbols` özelliği, uygulamanın hata ayıklanabilir olup olmadığını denetler.


### <a name="install-properties"></a>Özellikleri yükleme

Yükleme özellikleri davranışını denetleyen `Install` ve `Uninstall` hedefler.

-   **AdbTarget** &ndash; Android paketi Android hedef cihaz için yüklenmiş veya kaldırılmasını belirtir. Bu özelliğin değeri aynı olan [ `adb` hedef cihaz seçeneğini](http://developer.android.com/tools/help/adb.html#issuingcommands):

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```


### <a name="packaging-properties"></a>Paketleme özellikleri

Paketleme özellikleri Android paketi oluşturulmasını denetleyen ve tarafından kullanılan `Install` ve `SignAndroidPackage` hedefler.
[İmzalama özellikleri](#Signing_Properties) olan da uygun olduğunda paketleme sürüm uygulamalar.


-   **AndroidApkSigningAlgorithm** &ndash; ile kullanmak için kullanacağı imzalama algoritmasını belirleyen bir dize değeri `jarsigner -sigalg`.

    Varsayılan değer `md5withRSA` şeklindedir.

    Xamarin.Android 8.2 ' eklendi.

-   **AndroidApplication** &ndash; projesi bir Android uygulaması için olup olmadığını belirten bir Boole değeri (`True`) veya bir Android kitaplığı projesi (`False` veya yok).

    Yalnızca bir projeyle `<AndroidApplication>True</AndroidApplication>` Android bir paket içinde bulunabilir. (Ne yazık ki bu henüz, Android kaynakları ile ilgili hafif ve isteğine garip hatalarına yol açabilir doğrulanmadı.)

-   **AndroidBuildApplicationPackage** &ndash; oluşturmak ve paket (.apk) imzalamak etkinleştirilip etkinleştirilmeyeceğini gösteren bir Boole değeri. Bu değeri ayarlamak `True` kullanmakla eşdeğerdir [SignAndroidPackage](#Build_Targets) derleme hedefi.

    Xamarin.Android 7.1 sonra bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

-   **AndroidEnableMultiDex** &ndash; multi-dex desteği en son kullanılan olup olmadığını belirleyen bir boolean özelliği `.apk`.

    Xamarin.Android 5.1 Bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

-   **AndroidEnableSGenConcurrent** &ndash; Mono olup olmadığını belirleyen bir boolean özelliği 's [eş zamanlı GC Toplayıcı](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen) kullanılır.

    Xamarin.Android 7.2 Bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

-   **AndroidErrorOnCustomJavaObject** &ndash; türlerine uygulayabilir olup olmadığını belirleyen bir boolean özelliği `Android.Runtime.IJavaObject` 
     *olmadan* ayrıca dan devralan `Java.Lang.Object` veya `Java.Lang.Throwable`:

    ```csharp
    class BadType : IJavaObject {
        public IntPtr Handle {
            get {return IntPtr.Zero;}
        }

        public void Dispose()
        {
        }
    }
    ```

    TRUE olduğunda, bu türler XA4212 hataya neden olur, aksi takdirde XA4212 uyarısı oluşturulur.

    Xamarin.Android 8.1 Bu özellik için destek eklendi.

    Bu özellik `True` varsayılan olarak.

-   **AndroidFastDeploymentType** &ndash; A `:` (virgül)-ayrılmış ne türlerini denetlemek için değerler listesi dağıtılabilecek [hızlı dağıtım dizinine](#Fast_Deployment) hedef cihazdaki olduğunda `$(EmbedAssembliesIntoApk)` MSBuild özelliği `False`. Bir kaynak dağıtılan de hızlı içindeyse, *değil* oluşturulan katıştırılmış `.apk`, hangi dağıtım sürelerini hızlandırmak. (Hızlı, daha az sıklıkta dağıtılıp daha `.apk` derlenmeye gereksinimleri ve yükleme işlemi daha hızlı.) Geçerli değerler şunlardır:

    - `Assemblies`: Uygulama derlemeleri dağıtın.

    - `Dexes`: Dağıtma `.dex` dosyaları, Android kaynakları ve Android varlıklarını. **Bu değer *yalnızca* Android 4.4 veya üzeri (API-19) çalıştıran cihazlarda kullanılabilir.**

    Varsayılan değer `Assemblies` şeklindedir.

    **Deneysel**. Xamarin.Android 6.1 eklendi.

-   **AndroidApplicationJavaClass** &ndash; yerine kullanılacak tam Java sınıfı adı `android.app.Application` zaman sınıfından devralan bir sınıf [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/).

    Bu özellik genellikle belirlediği *diğer* özellikleri gibi `$(AndroidEnableMultiDex)` MSBuild özelliği.

    Xamarin.Android 6.1 eklendi.

-   **AndroidHttpClientHandlerType** &ndash; varsayılan denetimleri `System.Net.Http.HttpMessageHandler` tarafından kullanılacak uygulama `System.Net.Http.HttpClient` varsayılan oluşturucu. Değeri bir derleme nitelikli tür adı olan bir `HttpMessageHandler` ile kullanım için uygun alt [ `System.Type.GetType(string)` ](/dotnet/api/system.type.gettype?view=netcore-2.0#System_Type_GetType_System_String_).

    Varsayılan değer `System.Net.Http.HttpClientHandler, System.Net.Http` şeklindedir.

    Bu, bunun yerine içerecek şekilde kılınabilir `Xamarin.Android.Net.AndroidClientHandler`, ağ isteklerini gerçekleştirmek için Android Java API kullanır. Bu, temel alınan Android sürümü, TLS 1.2 desteklediğinde, TLS 1.2 URL'lerine erişme sağlar.  
    Yalnızca Android 5.0 ve üzeri, güvenilir bir şekilde Java aracılığıyla TLS 1.2 desteği sağlar.

    *Not*: 5.0 önce Android sürümlerinde, TLS 1.2 desteği gereklidir *veya* TLS 1.2 desteği ile gerekiyorsa `System.Net.WebClient` ve API'leri, ardından ilgili `$(AndroidTlsProvider)` kullanılmalıdır.

    *Not*: Bu özelliği ayarlayarak çalıştığı için destek [ `XA_HTTP_CLIENT_HANDLER_TYPE` ortam değişkeni](~/android/deploy-test/environment.md).
    A `$XA_HTTP_CLIENT_HANDLER_TYPE` değeri bir derleme eylemi ile bir dosya bulunan `@(AndroidEnvironment)` öncelikli olur.

    Xamarin.Android 6.1 eklendi.

-   **AndroidTlsProvider** &ndash; hangi TLS sağlayıcısı uygulamada kullanılması gerektiğini belirten bir dize değeri. Olası değerler şunlardır:

    - `btls`: Kullanın [Boring SSL](https://boringssl.googlesource.com/boringssl) TLS ile iletişim için [HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx).
      Bu, tüm Android sürümlerinde TLS 1.2 kullanımını sağlar.

    - `legacy`: Ağ etkileşimi için geçmiş yönetilen SSL uygulaması kullanın. Bu *yok* TLS 1.2 desteği.

    - `default`: İzin ver *Mono* varsayılan TLS sağlayıcısı seçme.
      Bunun eşdeğeri olan `legacy`Xamarin.Android 7.3 içinde olsa bile.  
      *Not*: Bu değer görünmesini düşüktür `.csproj` değerleri, IDE "Varsayılan" değer sonuçlarında *kaldırma* , `$(AndroidTlsProvider)` özelliği.

    - Ayarlama / boş dize: Xamarin.Android 7.1'nde, bu eşdeğerdir `legacy`.  
      Xamarin.Android 7.3 içinde bunun eşdeğeri olan `btls`.

    Varsayılan değer boş bir dizedir.

    Xamarin.Android 7.1 eklendi.

-   **AndroidLinkMode** &ndash; türünü belirtir [bağlama](~/android/deploy-test/linker.md) Android paketi içinde bulunan derlemeler üzerinde gerçekleştirilmelidir. Yalnızca Android uygulaması projelerinde kullanılır. Varsayılan değer *SdkOnly*. Geçerli değerler şunlardır:

    - **Hiçbiri**: hiçbir bağlantı kurulmaya çalışılır.

    - **SdkOnly**: bağlama olacak kullanıcının derlemeleri değil yalnızca temel sınıf kitaplıklarını temel gerçekleştirilen.

    - **Tam**: bağlama gerçekleştirilecek taban sınıfı kitaplıkları ve kullanıcı bütünleştirilmiş kodları. **Not:** kullanarak bir `AndroidLinkMode` değerini *tam* hızlıca Paylaşılmaması çoğunlukla benimsenme bozuk uygulamalar, özellikle yansıma kullanılırken. Yapmaktan kaçının, *gerçekten* ne yaptığınızı bildirin.

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; noktalı virgül ile ayrılmış belirtir (`;`) değil bağlanması gereken derlemelerin dosya uzantıları olmadan derleme adlarının listesi. Yalnızca Android uygulaması projeleri içinde kullanılır.

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; denetimleri olup olmadığı noktaları sıralı bir boolean özelliği, böylece dosya adı ve satır numarası bilgisi alanından ayıklanması gereken oluşturulur `Release` Yığın izlemeleri.

    Xamarin.Android 6.1 eklendi.

-   **AndroidManifest** &ndash; uygulamanın için şablon olarak kullanmak üzere bir dosya adı belirtir [ `AndroidManifest.xml` ](~/android/platform/android-manifest.md).
    Yapı sırasında diğer gerekli değerleri gerçek üretmek için birleştirilecektir `AndroidManifest.xml`.
    `$(AndroidManifest)` Nda paket adını içermelidir `/manifest/@package` özniteliği.

-   **AndroidSdkBuildToolsVersion** &ndash; Android SDK derleme Araçları Paketi sağlar **aapt** ve **zipalign** Araçlar, diğerlerinin yanı sıra. Derleme araçları paketini birden çok farklı sürümünü aynı anda yüklenebilir. Paketleme için seçilen derleme Araçları Paketi, denetleme ve varsa bir "tercih edilen" derleme araçları sürümü kullanılarak yapılır; "tercih edilen" sürüm ise *değil* sunmak en yüksek sürümü yüklü derleme Araçları Paketi kullanılır.

    `$(AndroidSdkBuildToolsVersion)` MSBuild özelliği, tercih edilen derleme araçları sürümü içerir. Varsayılan değer Xamarin.Android derleme sistemidir `Xamarin.Android.Common.targets`, ve (örneğin) en son aapt kullanıma önceki aapt sürümü sırasında kilitlenmesi durumunda varsayılan değer bir alternatif derleme araçları sürümü seçmek için proje dosyasında geçersiz kılınmış olabilir İş adı verilir.

-   **AndroidSupportedAbis** &ndash; noktalı virgül içeren bir dize özelliği (`;`)-ayrılmış bir içine dahil edilmesi gereken desteklenecek Abı'lerin listesi `.apk`.

    Desteklenen değerler şunlardır:

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: Xamarin.Android 5.1 ve üzeri gerektirir.
    -   `x86_64`: Xamarin.Android 5.1 ve üzeri gerektirir.

-   **AndroidUseSharedRuntime** &ndash; belirleyen bir boolean özelliği olup olmadığını *paylaşılan çalışma zamanı paketlerini* hedef cihazda uygulamayı çalıştırmak için gereklidir. Paylaşılan çalışma zamanı paketleri bağlı olan daha küçük, paket oluşturma ve dağıtma işlemini hızlandırmak, daha hızlı bir derleme/dağıtma/debug amortisman döngüsünde kaynaklanan uygulama paketi sağlar.

    Bu özellik olmalıdır `True` hata ayıklama yapıları için ve `False` yayın projeler için.

-   **AotAssemblies** &ndash; derlemeleri Ahead-ın-yerel kod içine derlenmiş ve dahil olacağı süre olup olmadığını belirleyen bir boolean özelliği `.apk`.

    Xamarin.Android 5.1 Bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

-   **EmbedAssembliesIntoApk** &ndash; derlemelerini uygulama pakete katıştırılmış olup olmadığını belirleyen bir boolean özelliği.

    Bu özellik olmalıdır `True` derleme yapıları ve `False` hata ayıklama yapıları için. Bunu *olabilir* olmasına gerek `True` hata ayıklama yapılarında hızlı dağıtım hedef cihaza desteklemiyorsa.

    Bu özellik olduğunda `False`, ardından `$(AndroidFastDeploymentType)` MSBuild özelliğini de denetler ne içine katıştırılır `.apk`, hangi dağıtım etkiler ve kez yeniden oluşturun.

-   **EnableLLVM** &ndash; LLVM Time Ahead kullanılan olup olmadığını belirleyen bir boolean özelliği bütünleştirilmiş kodları yerel koda derleniyor.

    Xamarin.Android 5.1 Bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

    Sürece bu özellik yoksayılır `$(AotAssemblies)` MSBuild özelliği `True`.

-   **EnableProguard** &ndash; belirleyen bir boolean özelliği olup olmadığını [proguard](http://developer.android.com/tools/help/proguard.html) Java kodunu bağlamak için paketleme işleminin bir parçası çalıştırılır.

    Xamarin.Android 5.1 Bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

    Zaman `True`, [ProguardConfiguration](#ProguardConfiguration) dosyaları denetlemek için kullanılacak `proguard` yürütme.

-   **JavaMaximumHeapSize** &ndash; değerini belirtir **java** 
     `-Xmx` oluştururken kullanılacak parametre değeri `.dex` paketleme işleminin bir parçası olarak dosya. Belirtilmezse, ardından `-Xmx` seçeneği için sağlanan değil **java**.

    Bu özelliği belirtmek gerekli olursa [ `_CompileDex` hedef oluşturur bir `java.lang.OutOfMemoryError` ](https://bugzilla.xamarin.com/show_bug.cgi?id=18327).

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; geçirilecek ek komut satırı seçeneklerini belirtir **java** oluştururken `.dex` dosya.

-   **MandroidI18n** &ndash; uygulama harmanlama gibi bulunan ve tabloları sıralama uluslararası desteğin belirtir. Bir veya daha büyük küçük harf duyarsız aşağıdaki değerleri virgülle veya noktalı virgülle ayrılmış listesini değerdir:

    -   **Hiçbiri**: hiçbir Kodlamalar içerir.

    -   **Tüm**: tüm kullanılabilir Kodlamalar içerir.

    -   **CJK**: gibi içeren Çince, Japonca ve Korece kodlamaları *Japonca (EUC)* \[enc-jp, CP51932\], *Japonca (Shift-JIS)* \[ ISO-2022-jp, SHIFT\_JIS, CP932\], *Japonca (JIS)* \[CP50220\], *Basitleştirilmiş Çince (GB2312)* \[gb2312, CP936\], *Korece (UHC)* \[görevleri\_c\_5601-1987 CP949\], *Kore dili (EUC)* \[euc-kr, CP51949\], *Geleneksel Çince (Big5)* \[big5, CP950\], ve *Basitleştirilmiş Çince (GB18030)* \[ GB18030, CP54936\].

    -   **MidEast**: gibi dahil Orta Doğu'ya ait kodlamaları *Türkçe (Windows)* \[ISO-8859-9, CP1254\], *İbranice (Windows)* \[ Windows-1255 CP1255\], *Arapça (Windows)* \[windows 1256, CP1256\], *Arapça (ISO)* \[ISO-8859-6, CP28596\], *İbranice (ISO)* \[ISO-8859-8, CP28598\], *Latin 5 (ISO)* \[ISO-8859-9, CP28599\]ve *İbranice (ISO alternatif)* \[ISO-8859-8, CP38598\].

    -   **Diğer**: gibi diğer Kodlamalar dahil *Cyrilice (Windows)* \[CP1251\], *Baltık (Windows)* \[ISO-8859-4, CP1257\], *Vietnamca (Windows)* \[CP1258\], *Cyrilice (KOI8-R)* \[KOI8-r CP1251\], *Ukrayna dili (KOI8-U)*  \[KOI8-u, CP1251\], *Baltık (ISO)* \[ISO-8859-4, CP1257\], *Cyrilice (ISO)* \[ISO-8859-5, CP1251\], *ISCII Davenagari* \[x-ISCII-de, CP57002\], *ISCII Bengali* \[x-ISCII-be, CP57003 \], *ISCII Tamil dili* \[x-ISCII-veri, CP57004\], *ISCII Telugu dili* \[x-ISCII-metin, CP57005\], *ISCII Assam dili* \[x-ISCII-olarak CP57006\], *ISCII Odia* \[x-ISCII-veya CP57007\], *ISCII Kannada dili* \[x-ISCII-ka, CP57008\], *ISCII Malayalam dili* \[x-ISCII-ma, CP57009\], *ISCII Gucerat dili* \[x-ISCII-gu, CP57010\], *ISCII Pencap dili* \[x-ISCII-pa, CP57011\], ve *Tay dili (Windows)*  \[CP874\].

    -   **Nadir**: gibi dahil nadir Kodlamalar *IBM EBCDIC (Türkçe)* \[CP1026\], *IBM EBCDIC (açık sistemleri Latin 1)* \[CP1047\], *IBM EBCDIC (ABD-Kanada Euro ile)* \[CP1140\], *IBM EBCDIC (Almanya Euro ile)* \[CP1141\], *IBM EBCDIC (Danimarka/Norveç Euro ile)* \[CP1142\], *IBM EBCDIC (Finlandiya/İsveç Euro ile)* \[CP1143\], *IBM EBCDIC (İtalya Euro ile)* \[CP1144\], *IBM EBCDIC (Latin Amerika/İspanya Euro ile)* \[CP1145\], *IBM EBCDIC (Birleşik Euro ile Krallık)* \[CP1146\], *IBM EBCDIC (Fransa Euro ile)* \[CP1147\], *IBM EBCDIC (uluslararası Euro ile)*  \[CP1148\], *IBM EBCDIC (İzlanda Euro ile)* \[CP1149\], *IBM EBCDIC (Almanya)* \[ CP20273\], *IBM EBCDIC (Danimarka/Norveç)* \[CP20277\], *IBM EBCDIC (Finlandiya/İsveç)* \[CP20278\], *IBM EBCDIC (İtalya)* \[CP20280\], *IBM EBCDIC (Latin Amerika/İspanya)* \[CP20284\], *IBM EBCDIC (Birleşik Krallık)* \[CP20285\], *IBM EBCDIC (Japonca Katakana genişletilmiş)* \[CP20290\], *IBM EBCDIC (Fransa)* \[CP20297\], *IBM EBCDIC (Arapça)* \[CP20420\], *IBM EBCDIC (İbranice)* \[CP20424\], *IBM EBCDIC (İzlanda)* \[ CP20871\], *IBM EBCDIC (Kiril - Sırpça, Bulgarca)* \[CP21025\], *IBM EBCDIC (ABD-Kanada)* \[ CP37\], *IBM EBCDIC (uluslararası)* \[CP500\], *Arapça (ASMO 708)* \[CP708\], *Orta Avrupa (DOS)* \[CP852\], *Kiril (DOS)* \[CP855\], *Türk (DOS)* \[CP857\], *Batı Avrupa (Euro DOS)* \[CP858\], *İbranice (DOS)* \[CP862\], *Arapça (DOS)* \[CP864\], *Rusça (DOS)* \[CP866\], *Yunanca (DOS)* \[CP869\], *IBM EBCDIC (Latin 2)* \[CP870\] ve *IBM EBCDIC (Yunanca)* \[CP875\].

    -   **Batı**: Batı dahil Kodlamalar gibi *Batı Avrupa (Mac)* \[macintosh CP10000\], *İzlanda dili (Mac)* \[x-mac-İzlanda CP10079\], *Orta Avrupa (Windows)* \[ISO-8859-2, CP1250\], *Batı Avrupa (Windows)* \[ISO-8859-1 CP1252\], *Yunanca (Windows)* \[ISO-8859-7, CP1253\], *Orta Avrupa (ISO)* \[ISO-8859-2, CP28592\], *Latin 3 (ISO)* \[ISO-8859-3, CP28593\], *Yunanca (ISO)* \[ISO-8859-7, CP28597\], *Latin 9 (ISO)*  \[ISO-8859-15, CP28605\], *OEM-USA* \[CP437\], *Batı Avrupa (DOS)* \[CP850\], *Portekizce (DOS)* \[CP860\], *İzlanda dili (DOS)* \[CP861\],  *Kanada Fransızcası (DOS)* \[CP863\], ve *İskandinav (DOS)* \[CP865\].


    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchive** &ndash; denetleyen bir boolean özelliği olup olmadığını `.mSYM` yapıt ile daha sonra kullanmak için oluşturulur `mono-symbolicate`ayıklamak amacıyla &ldquo;gerçek&rdquo; dosya adı ve satır numarası bilgileri Yayın yığın izlemesi.

    Bu doğru ise için varsayılan olarak &ldquo;yayın&rdquo; hata ayıklama simgeleri etkin olan uygulamalar: `$(EmbedAssembliesIntoApk)` True ise `$(DebugSymbols)` True ise ve `$(Optimize)` true'dur.

    Xamarin.Android 7.1 eklendi.

-   **AndroidVersionCodePattern** &ndash; özelleştirmek Geliştirici izin veren bir dize özelliği `versionCode` katıştırır.
    Bkz: [sürüm kod oluşturma için apk'yı](~/android/deploy-test/building-apps/abi-specific-apks.md) karar verme konusunda bilgi için bir `versionCode`.
    
    Bazı örnekler, `abi` olduğu `armeabi` ve `versionCode` içinde bildirim `123`, `{abi}{versionCode}` , bir versionCode üretecektir `1123` olduğunda `$(AndroidCreatePackagePerAbi)` true, aksi takdirde 123 değeri oluşturur.
    Varsa `abi` olduğu `x86_64` ve `versionCode` içinde bildirim `44`. Bu oluşturur `544` olduğunda `$(AndroidCreatePackagePerAbi)` true, aksi takdirde değeri üretecektir `44`.

    Biçim dizesi doldurma sol dahil ediyoruz, `{abi}{versionCode:0000}`, oluşturmak `50044` biz doldurma bırakılır çünkü `versionCode` ile `0`. Alternatif olarak, aşağıdakiler gibi doldurma ondalık kullanabilirsiniz `{abi}{versionCode:D4}` , yürüttüğü önceki örnek ile aynı.

    Yalnızca, '0' ve 'biçim dizeleri, değerin beri desteklenir doldurma Dx' bir tamsayı olmalıdır.
    
    Önceden tanımlanmış bir anahtar öğeleri

    -   **ABI** &ndash; uygulama için hedeflenen ABI ekler  
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; ekler desteklenen en düşük Sdk değerinden `AndroidManifest.xml` veya `11` tanımlanmışsa, yok.  

    -   **versionCode** &ndash; doğrudan sürüm kod `Properties\AndroidManifest.xml`. 

    Özel öğeleri kullanarak tanımladığınız `$(AndroidVersionCodeProperties)` özelliği (sonraki tanımlanır).

    Varsayılan olarak, değeri ayarlanacak `{abi}{versionCode:D6}`. Eski davranışı korumak bir geliştirici istiyorsa, ayarlayarak varsayılan kılabilirsiniz `$(AndroidUseLegacyVersionCode)` özelliği `true`

    Xamarin.Android 7.2 eklendi.

-   **AndroidVersionCodeProperties** &ndash; ile kullanılacak özel öğeleri tanımlamak Geliştirici izin veren bir dize özelliği `AndroidVersionCodePattern`. Biçiminde oldukları bir `key=value` çifti. Tüm öğeleri `value` tamsayı değeri olmalıdır. Örneğin: `screen=23;target=$(_SupportedApiLevel)`. ' In mevcut veya özel MSBuild kullanmak yapabilirsiniz gördüğünüz gibi dize özellikleri.

    Xamarin.Android 7.2 eklendi.

-   **AndroidUseLegacyVersionCode** &ndash; bir boolean özelliği izin verir, eski öncesi Xamarin.Android 8.2 davranışı versionCode hesaplamaya geri dönmek Geliştirici. Bu yalnızca Google Play Store içinde var olan uygulamalarla birlikte geliştiriciler için kullanılmalıdır. Yüksek oranda önerilir yeni `$(AndroidVersionCodePattern)` özelliği kullanılır.

    Xamarin.Android 8.2 ' eklendi.

-  **AndroidUseManagedDesignTimeResourceGenerator** &ndash; yönetilen kaynak ayrıştırıcısını kullanılacağı tasarım zaman içinde geçecektir bir boolean özelliği derlemeler yerine `aapt`.

    Xamarin.Android 8.1 eklendi.

-  **AndroidUseApkSigner** &ndash; kullanmak Geliştirici izin veren bir bool özelliği için `apksigner` aracı yerine `jarsigner`.

    Xamarin.Android 8.2 ' eklendi.

-  **AndroidApkSignerAdditionalArguments** &ndash; sağlamak için ek bağımsız değişkenler Geliştirici izin veren bir dize özelliği `apksigner` aracı.

    Xamarin.Android 8.2 ' eklendi.

### <a name="binding-project-build-properties"></a>Proje derleme özelliklerini bağlama

Aşağıdaki MSBuild özellikleri ile kullanılan [bağlama projeleri](~/android/platform/binding-java-library/index.md):

-   **AndroidClassParser** &ndash; denetleyen bir dize özelliğini nasıl `.jar` dosyaları ayrıştırılır. Olası değerler şunlardır:

    - **class-parse**: kullanan `class-parse.exe` Java bytecode'u bir JVM Yardımı doğrudan ayrıştırmak için. Bu Deneysel değerdir. 


    - **jar2xml**: kullanın `jar2xml.jar` türleri ve üyeleri ayıklamak üzere Java yansıması kullanmak için bir `.jar` dosya.

    Daha üstün olan yönleri `class-parse` üzerinden `jar2xml` şunlardır:

    - `class-parse` parametre adları içeren Java bytecode'u ayıklamanız mümkün olduğu *hata ayıklama* simgeleri (örneğin ile bayt derlenmiş `javac -g`).

    - `class-parse` devralınan veya bu çözümlenemeyen türlerin üyelerini içeren sınıfları "atlamaz".

    **Deneysel**. Xamarin.Android 6.0 eklendi.

    Varsayılan değer `jar2xml` şeklindedir.

    Varsayılan değer, gelecekteki bir sürümde değişecektir.

-   **AndroidCodegenTarget** &ndash; kod oluşturma hedefi ABI denetleyen bir dize özelliği. Olası değerler şunlardır:

    - **XamarinAndroid**: Android 1.0 için Mono beri mevcut JNI bağlama API kullanır. Xamarin.Android 5.0 veya üzeri (API/ABI ekleme), çalıştırılacak yalnızca bağlama derlemeleri Xamarin.Android 5.0 veya üzeri oluşturulmuş olabilir ancak *kaynak* önceki ürün sürümleriyle uyumludur.

    - **XAJavaInterop1**: JNI çağrıları için Java.Interop kullanın. Kullanarak bağlama derlemeleri `XAJavaInterop1` yalnızca derleme ve Xamarin.Android 6.1 veya üzeri yürütün. Xamarin.Android 6.1 ve üzeri bağlama `Mono.Android.dll` bu değere sahip.

      Avantajlarını `XAJavaInterop1` içerir:

      - Daha küçük bütünleştirilmiş kodları.

      - `jmethodID` için önbelleğe alma `base` yöntem çağrıları, diğer bağlamadaki devralma hiyerarşisinde türleri sürece yerleşik ile `XAJavaInterop1` veya üzeri.

      - `jmethodID` Java çağrılabilir sarmalayıcı oluşturucular yönetilen alt sınıflar için önbelleğe alma.

    Varsayılan değer `XamarinAndroid` şeklindedir.

    Varsayılan değer, gelecekteki bir sürümde değişecektir.


### <a name="resource-properties"></a>Kaynak özellikleri

Kaynak özelliklerini kontrol oluşturulmasını `Resource.designer.cs` Android kaynaklara erişim sağlayan dosya. 

-   **AndroidResgenExtraArgs** &ndash; geçirilecek ek komut satırı seçeneklerini belirtir **aapt** Android varlıklarını ve kaynakları işlerken komutu.

-   **AndroidResgenFile** &ndash; oluşturulacak kaynak dosyasının adını belirtir. Varsayılan şablon bu ayarlar `Resource.designer.cs`.

-   **MonoAndroidResourcePrefix** &ndash; belirtir bir *yol ön eki* bir derleme eylemi ile dosya adlarını başından itibaren kaldırıldı `AndroidResource`. Bu kaynakların yer aldığı değiştirme izin vermektir.

    Varsayılan değer `Resources` şeklindedir. Bu değişiklik `res` Java için Proje yapısı.

-   **AndroidExplicitCrunch** &ndash; yerel drawables çok büyük bir sayı ile bir uygulama oluşturuyorsanız ilk oluşturma (veya yeniden derleme) tamamlanması birkaç dakika sürebilir. Derleme sürecini hızlandırmak için bu özelliği dahil olmak üzere deneyin ve ayarlamak `True`. Bu özelliği ayarlı olduğunda derleme işleminin .png dosyaları önceden crunches.

    **Deneysel**. Xamarin.Android 7.0 eklendi.


<a name="Signing_Properties" />

### <a name="signing-properties"></a>İmzalama özellikleri

Bir Android cihaza yüklenebilir, böylece uygulama paketini nasıl imzalı özellikleri denetimi imzalama. İmzalama oldukça yavaş olduğundan daha hızlı derleme yineleme izin vermek için Xamarin.Android görevleri paketleri derleme işlemi sırasında oturum açmasını sağlamayan. Bunun yerine, (gerekirse) dışarı aktarma sırasında veya yüklemeden önce IDE'yi tarafından imzalandıkları veya *yükleme* derleme hedefi. Çağırma *SignAndroidPackage* sahip bir paketi hedef ilişkilendiren `-Signed.apk` çıkış dizinindeki soneki.

Varsayılan olarak, imzalama hedef gerekirse yeni bir hata ayıklama imzalama anahtarı oluşturur. Örneğin belirli bir anahtar kullanmak istiyorsanız, bir yapı sunucusunda aşağıdaki MSBuild özellikleri kullanılabilir:

-   **AndroidKeyStore** &ndash; özel imza bilgilerini kullanılıp kullanılmayacağını gösteren bir Boole değeri. Varsayılan değer `False`, varsayılan hata ayıklama imzalama anahtarı paketleri imzalamak için kullanılacak anlamına gelir.

-   **AndroidSigningKeyAlias** &ndash; anahtar diğer adında bir anahtar deposunda belirtir. Bu **keytool-diğer ad** keystore oluştururken kullanılan değer. 

-   **AndroidSigningKeyPass** &ndash; anahtar deposu dosyasının içinde anahtar parolasını belirtir. Bu değer oluşturulur ne zaman girilen `keytool` ister **$(AndroidSigningKeyAlias) için anahtar parola gir**.

-   **AndroidSigningKeyStore** &ndash; dosya tarafından oluşturulan anahtar deposu dosyasının adını belirtir `keytool`. Sağlanan değere karşılık gelen **keytool - keystore** seçeneği.

-   **AndroidSigningStorePass** &ndash; parolasını belirtir `$(AndroidSigningKeyStore)`. İçin sağlanan değer budur `keytool` anahtar deposu dosyasının oluştururken ve sorulan **Enter anahtar deposu parolası:**. 

Örneğin, aşağıdakileri dikkate alın `keytool` çağırma:

```shell
$ keytool -genkey -v -keystore filename.keystore -alias keystore.alias -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password: keystore.filename password
Re-enter new password: keystore.filename password
...
Is CN=... correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: ...
Enter key password for keystore.alias
        (RETURN if same as keystore password): keystore.alias password
[Storing filename.keystore]
```

Yukarıda oluşturulan anahtar deposu kullanmak için özellik grubunu kullanın:

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

-   **AndroidDebugKeyAlgorithm** &ndash; için kullanılacak varsayılan algoritmasını belirtir `debug.keystore`. Varsayılan `RSA`.

-   **AndroidDebugKeyValidity** &ndash; için kullanılacak varsayılan geçerlilik belirtir `debug.keystore`. Varsayılan `10950` veya `30 * 365` veya `30 years`.

<a name="Build_Actions" />

## <a name="build-actions"></a>Derleme eylemleri

*Derleme eylemleri* olan [dosyalara uygulanması](https://docs.microsoft.com/visualstudio/msbuild/common-msbuild-project-items) denetim dosyasının nasıl işlenir ve proje içinde. 

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

Bir derleme eylemi dosyalarıyla `AndroidEnvironment` alışık olduğunuz [ortam değişkenleri ve Sistem özellikleri işlemi başlatma sırasında başlatmak](~/android/deploy-test/environment.md).
`AndroidEnvironment` Yapı eylemi birden çok dosyaya uygulanan ve (aynı ortam değişkeni veya sistem özelliği birden çok dosyasında belirtmeyin şekilde) hiçbir özel sırayla değerlendirilir.


### <a name="androidjavasource"></a>AndroidJavaSource

Bir derleme eylemi dosyalarıyla `AndroidJavaSource` Java kaynak kodu son Android pakette yer alacak.


### <a name="androidjavalibrary"></a>AndroidJavaLibrary

Bir derleme eylemi dosyalarıyla `AndroidJavaLibrary` Java arşivleri olan ( `.jar` dosyaları), dahil edilecek son Android pakette.


### <a name="androidresource"></a>AndroidResource

Tüm dosyaları bir *AndroidResource* yapı eylemi derleme işlemi sırasında Android kaynakları derlenir ve aracılığıyla erişilebilir duruma `$(AndroidResgenFile)`.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

Daha gelişmiş kullanıcılar, belki de farklı kaynaklar farklı yapılandırmalarda ancak aynı etkin yola sahip olmasını isteyebilir. Bu, sahip birden çok kaynak dizinleri ve dosyaları ile aynı göreli yollar farklı bu dizinler içinde olması ve farklı dosyalar farklı yapılandırmalarda koşullu olarak dahil etmek için MSBuild koşulları kullanarak gerçekleştirilebilir. Örneğin:

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
<ItemGroup  Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug\values\strings.xml"/>
</ItemGroup>
<PropertyGroup>
  <MonoAndroidResourcePrefix>Resources;Resources-Debug<MonoAndroidResourcePrefix>
</PropertyGroup>
```

**LogicalName** &ndash; açıkça kaynak yolunu belirtir. Sağlar &ldquo;yumuşatma&rdquo; birden çok farklı kaynak adları kullanılabilir olmaları dosyalar.

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources/values/strings.xml"/>
</ItemGroup>
<ItemGroup Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug/values/strings.xml">
    <LogicalName>values/strings.xml</LogicalName>
  </AndroidResource>
</ItemGroup>
```


### <a name="androidnativelibrary"></a>AndroidNativeLibrary

[Yerel kitaplıkları](~/android/platform/native-libraries.md) yapı, yapı eylemi ayarlayarak eklenen `AndroidNativeLibrary`.

Derleme Sistemi Android birden çok uygulama ikili arabirimi (Abı'ler) desteklediğinden, yerel kitaplık hangi ABI bilmelisiniz Not oluşturulmuştur. Bu yapılabilir iki yolu vardır:

1.  Yol "algılaması".
2.  Kullanarak `Abi` öğesi özniteliği.

Yol algılaması ile yerel kitaplığı üst dizin adını ABI belirtmek için kullanılan, kitaplık hedefleri. Bu nedenle, eklerseniz `lib/armeabi/libfoo.so` yapı, ardından ABI "olarak sızılmasını" `armeabi`. 


#### <a name="item-attribute-name"></a>Öğe öznitelik adı

**ABI** &ndash; yerel kitaplığın ABI belirtir.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```


### <a name="androidaarlibrary"></a>AndroidAarLibrary

Derleme eylemini `AndroidAarLibrary` doğrudan .aar dosyalara başvurmak için kullanılmalıdır. Bu yapı eylemi, Xamarin bileşenleri tarafından en yaygın olarak kullanılır. Yani, Google Play ve çalışan diğer hizmetler için gerekli olan .aar dosyalara başvuruları eklemek için.

Bu derleme eylemi benzer şekilde kabul edilecek dosyalarla gömülü kaynaklar kitaplık projeleri içinde çok bulundu. Ara dizin .aar ayıklanır. Ardından tüm varlıkları kaynak ve .jar dosyalarını uygun öğesi grupları dahil edilir.  

### <a name="content"></a>İçerik

Normal `Content` yapı eylemi (biz büyük olasılıkla pahalı bir ilk kez çalıştırma adımı desteklemek nasıl yapacağınızı bilmiyorsanız gibi) desteklenmez.

Xamarin.Android 5.1 kullanılmaya çalışılıyor, başlangıç `@(Content)` yapı eylemi sonucunda bir `XA0101` uyarı.

### <a name="linkdescription"></a>LinkDescription

İle dosyaları bir *LinkDescription* yapı eylemi alışkın olduğunuz [denetleme bağlayıcı davranışı](~/cross-platform/deploy-test/linker.md).


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

İle dosyaları bir *ProguardConfiguration* derleme eylemini denetlemek için kullanılan seçenekler içeren `proguard` davranışı. Bu yapı eylemi hakkında daha fazla bilgi için bkz: [ProGuard](~/android/deploy-test/release-prep/proguard.md).

Bu dosyalar yoksayılır `$(EnableProguard)` MSBuild özelliği `True`.


## <a name="target-definitions"></a>Hedef tanımları

Yapı işlemi Xamarin.Android belirli bölümlerini tanımlanan `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets`, gibi ancak normal dile özgü hedefleri *Microsoft.CSharp.targets* derlemesi oluşturmak için gereklidir.

Aşağıdaki yapı özellikleri herhangi bir dil hedefleri içeri aktarmadan önce ayarlanmalıdır:

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

Tüm bu hedefleri ve özellikleri içeri aktararak için C# dahil edilebilir *Xamarin.Android.CSharp.targets*: 

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

Bu dosyayı kolayca diğer diller için uyarlanabilir.
