---
title: "Derleme işlemi"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 51caebb86cb72b11ced70522fc253e608f5ccab0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="build-process"></a>Derleme işlemi


## <a name="overview"></a>Genel Bakış

Xamarin.Android derleme işlemi her şeyi birlikte yapıştırma için sorumludur: [oluşturma `Resource.designer.cs` ](~/android/internals/api-design.md), destek `AndroidAsset`, `AndroidResource`ve diğer [Eylemler yapı](#Build_Actions), oluşturma [Android aranabilir sarmalayıcılar](~/android/platform/java-integration/android-callable-wrappers.md), oluşturmasını bir `.apk` Android cihazlarda yürütme için.


## <a name="application-packages"></a>Uygulama paketleri

Geniş bağlamında, Android uygulama paketleri iki tür vardır (`.apk` dosyaları), Xamarin.Android yapı sistem oluşturabilirsiniz:

-   **Yayın** tam olarak kendi içinde bulunan ve yürütmek için ek paketlerini gerektirmeyen derlemeleri. Sağlanan paketler bunlar bir uygulama mağazasının.

-   **Hata ayıklama** olmayan derlemeleri.

Değil tesadüfen bu MSBuild eşleşen `Configuration` paket oluşturur.


### <a name="shared-runtime"></a>Paylaşılan çalışma zamanı

*Çalışma zamanı paylaşılan* temel sınıf kitaplığı sağlayan ek Android paketler çiftidir (`mscorlib.dll`, vs.) ve Android bağlama kitaplığı (`Mono.Android.dll`vb..). Hata ayıklama temel sınıf kitaplığı ve Android uygulama paketi içindeki küçük olması hata ayıklama paket izin vererek bağlama derlemelerde dahil olmak üzere yerine paylaşılan çalışma zamanı derlemeleri kullanır.

Paylaşılan çalışma zamanı devre dışı bırakılması ayarlayarak hata ayıklama derlemelerinde `$(AndroidUseSharedRuntime)` özelliğine `False`.

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>Hızlı Dağıtım

*Hızlı Dağıtım* daha fazla Android uygulama paketi boyutunu küçültmek için paylaşılan çalışma zamanı ile birlikte çalışır. Bu paketin içinde uygulamanın derlemeleri paketleme değil tarafından gerçekleştirilir. Hedefe bunun yerine, kopyaladığınız `adb push`. Olduğundan bu işlem yapı/dağıtmak/hata ayıklama döngüsü hızlandırır varsa *yalnızca* derlemeler değiştirilirse, paket değil yeniden yüklenir. Bunun yerine, yalnızca güncelleştirilmiş derlemeler ile hedef aygıta yeniden eşitlenmiş.

Hızlı Dağıtım engelleme cihazlarda vermesine bilinir `adb` dizinine eşitlenmesini `/data/data/@PACKAGE_NAME@/files/.__override__`.

Hızlı Dağıtım varsayılan olarak etkindir ve devre dışı hata ayıklama derlemeleri ayarlayarak `$(EmbedAssembliesIntoApk)` özelliğine `True`.



## <a name="msbuild-projects"></a>MSBuild projelerine

Xamarin.Android oluşturma işlemi aynı zamanda Mac ve Visual Studio için Visual Studio tarafından kullanılan proje dosyası biçimi olan MSBuild temel alır.
Normalde, kullanıcıların MSBuild dosyaları elle düzenleyin gerekmez &ndash; IDE tam olarak işlevsel projeleri oluşturur ve yapılan değişiklikler ile güncelleştirir ve gerektiğinde otomatik olarak yapı hedefleri çağırma.

İleri düzey kullanıcılar proje dosyasını doğrudan düzenleyerek yapı işlemi özelleştirilebilir olacak şekilde IDE'nin GUI tarafından desteklenmeyen şeyler isteyebilirsiniz.
Bu sayfa yalnızca Xamarin.Android özgü özellikler ve özelleştirmeleri belgeleri &ndash; pek çok şey daha normal MSBuild öğeler, özellikleri ve hedefleri ile mümkündür.

<a name="Build_Targets" />

## <a name="build-targets"></a>Hedefleri derleme

Aşağıdaki yapı hedefleri Xamarin.Android projeleri için tanımlanmıştır:

-   **Yapı** &ndash; paket oluşturur.

-   **Temiz** &ndash; yapı işlemi tarafından oluşturulan tüm dosyaları kaldırır.

-   **Yükleme** &ndash; varsayılan aygıt veya sanal aygıt paketi yükler.

-   **Kaldırma** &ndash; varsayılan aygıt ya da sanal aygıt Paketi kaldırır.

-   **SignAndroidPackage** &ndash; oluşturur ve paket imzalar (`.apk`). İle kullandığınız `/p:Configuration=Release` kendi içinde yer alan "Yayın" paketleri oluşturulacak.

-   **UpdateAndroidResources** &ndash; güncelleştirmeleri `Resource.designer.cs` dosya. Yeni kaynaklar projeye eklendiğinde bu hedef genellikle IDE tarafından çağrılır.


## <a name="build-properties"></a>Yapı Özellikleri

MSBuild özellikleri hedefleri davranışını denetler. Proje dosyası içinde örneğin belirtilir **MyApp.csproj**, içinde bir [MSBuild PropertyGroup öğesi](http://msdn.microsoft.com/en-us/library/t4w159bs.aspx).

-   **Yapılandırma** &ndash; , "Hata ayıklama" veya "Yayın" gibi kullanmak üzere derleme yapılandırma belirtir. Yapılandırma özelliği, hedef davranışını belirleyen diğer özellikler için varsayılan değerleri belirlemek için kullanılır. Ek yapılandırmalar, IDE içinde oluşturulabilir.

    *Varsayılan olarak*, `Debug` yapılandırma neden olur `Install` ve `SignAndroidPackage` diğer dosyaları varlığını gerektiren daha küçük bir Android paketini ve çalışmak için paketler oluşturma hedefler.

    Varsayılan `Release` yapılandırma neden olur, `Install` ve `SignAndroidPackage` bir Android olan paket oluşturma hedefleri *tek başına*ve herhangi bir paket veya dosyaları yüklemeden kullanılabilir.

-   **DebugSymbols** &ndash; Android paketini olup olmadığını belirleyen bir boolean değeri *debuggable*, birlikte `$(DebugType)` özelliği. Hata ayıklama simgeleri, ayarlar debuggable pakette `//application/@android:debuggable` özniteliğini `true`ve otomatik olarak ekler `INTERNET` izni böylece işleme bir hata ayıklayıcısı ekleyebilirsiniz. Bir uygulama debuggable varsa `DebugSymbols` olan `True` *ve* `DebugType` ya da boş bir dize veya `Full`.

-   **DebugType** &ndash; belirtir [hata ayıklama simgeleri türünü](http://msdn.microsoft.com/en-us/library/s5c8athz.aspx) uygulama debuggable olup olmadığını da etkiler derleme bir parçası olarak oluşturulacak. Olası değerler şunlardır:

    - **Tam**: tam simgeleri oluşturulur. Varsa `DebugSymbols` MSBuild özelliktir ayrıca `True`, uygulama paketi debuggable olur.

    - **PdbOnly**: "PDB" simgeleri oluşturulur. Uygulama paketi olacak *değil* debuggable olabilir.

    Varsa `DebugType` ayarlı değil veya boş bir dize sonra `DebugSymbols` özelliği, uygulama th debuggable olup olmadığını denetler.


### <a name="install-properties"></a>Özellikleri yükleme

Yükleme özellikleri davranışını denetleyen `Install` ve `Uninstall` hedefler.

-   **AdbTarget** &ndash; Android paketini Android hedef cihaz için yüklenmiş veya kaldırılmasını belirtir. Bu özelliğin değeri aynıdır [ `adb` hedef aygıt seçeneği](http://developer.android.com/tools/help/adb.html#issuingcommands):

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/xbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```


### <a name="packaging-properties"></a>Paketleme özellikleri

Paketleme özelliklerini Android paketini oluşturulmasını denetlemek ve tarafından kullanılan `Install` ve `SignAndroidPackage` hedefler.
[İmzalama özellikleri](#Signing_Properties) de ne zaman uygun olan packaing yayın uygulamaları.


-   **AndroidApplication** &ndash; projeyi Android uygulamaya ait olup olmadığını belirten bir Boole değeri (`True`) veya bir Android kitaplığı projesi için (`False` ya da mevcut değil).

    Yalnızca bir projeyle `<AndroidApplication>True</AndroidApplication>` Android bir paket içinde bulunabilir. (Ne yazık ki bu henüz, hangi Android kaynakları ile ilgili hafif ve garip hataları sonuçlanabilir doğrulanmaz.)

-   **AndroidBuildApplicationPackage** &ndash; oluşturun ve paketi (.apk) oturum kılmayacağını gösteren bir Boole değeri. Bu değeri ayarlamak `True` kullanmakla eşdeğerdir [SignAndroidPackage](#Build_Targets) hedef oluşturun.

    Xamarin.Android 7.1 sonra bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

-   **AndroidEnableMultiDex** &ndash; çok dex destek en son kullanılan olup olmadığını belirleyen bir boolean özelliği `.apk`.

    Xamarin.Android 5.1, bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

-   **AndroidEnableSGenConcurrent** &ndash; Mono olup olmadığını belirleyen bir boolean özelliği 's [eşzamanlı GC Toplayıcı](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen) kullanılır.

    Bu özellik için destek Xamarin.Android 7.2 eklendi.

    Bu özellik `False` varsayılan olarak.

-   **AndroidFastDeploymentType** &ndash; A `:` (iki nokta üst üste)-ayrılmış ne türlerini denetlemek için değerlerin listesi için dağıtılabilir [hızlı dağıtım dizinine](#Fast_Deployment) hedef aygıttaki zaman `$(EmbedAssembliesIntoApk)` MSBuild özelliği `False`. Bir kaynak dağıtılan hızlı bu ise, *değil* oluşturulan katıştırılmış `.apk`, hangi dağıtım işlemlerini hızlandırmak. (Hızlı, ardından daha az sıklıkta dağıtılan daha `.apk` yeniden oluşturulması için gereksinimleri ve yükleme işlemi daha hızlı.) Geçerli değerler şunlardır:

    - `Assemblies`: Uygulama derlemeleri dağıtın.

    - `Dexes`: Dağıtma `.dex` dosyaları, Android kaynakları ve Android varlıklar. **Bu değer *yalnızca* Android 4.4 veya üzeri (API-19) çalıştıran cihazlarda kullanılabilir.**

    Varsayılan değer `Assemblies` şeklindedir.

    **Deneysel**. Xamarin.Android 6.1 eklendi.

-   **AndroidApplicationJavaClass** &ndash; yerine kullanılacak tam Java sınıfı adı `android.app.Application` ne zaman bir sınıf devraldığı [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/).

    Bu özellik genellikle belirlediği *diğer* özellikleri gibi `$(AndroidEnableMultiDex)` MSBuild özelliği.

    Xamarin.Android 6.1 eklendi.

-   **AndroidHttpClientHandlerType** &ndash; değerini ayarlama izin [ `XA_HTTP_CLIENT_HANDLER_TYPE` ortam değişkeni](~/android/deploy-test/environment.md).
    Bu değer bir açıkça belirtilen geçersiz kılmaz `XA_HTTP_CLIENT_HANDLER_TYPE` değeri. Bir `XA_HTTP_CLIENT_HANDLER_TYPE` ortam değişkeni değeri belirtilen bir [ `@(AndroidEnvironment)` ](#AndroidEnvironment) dosya öncelik alır.

    Xamarin.Android 6.1 eklendi.

-   **AndroidTlsProvider** &ndash; hangi TLS sağlayıcısı uygulamada kullanılması gerektiğini belirten bir dize değeri. Olası değerler şunlardır: Geçerli değerler şunlardır:

    - `btls`: Kullanın [Boring SSL](https://boringssl.googlesource.com/boringssl) ile TLS iletişim için [HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx).
      Bu, TLS 1.2 kullanımını sağlar.

    - `legacy`: Geçmiş yönetilen SSL uygulaması ağ etkileşim için kullanın. Bu *yok* TLS 1.2 destekler.

    - `default`, ya da unset / boş dize: Xamarin.Android 7.1'nde, bu eşdeğerdir `legacy`.

    Varsayılan değer boş bir dizedir.

    **Deneysel**. Xamarin.Android 7.1 eklendi.

-   **AndroidLinkMode** &ndash; hangi tür belirtir [bağlama](~/android/deploy-test/linker.md) Android paket içinde bulunan derlemeler üzerinde gerçekleştirilmelidir. Yalnızca Android uygulaması projelerinde kullanılır. Varsayılan değer *SdkOnly*. Geçerli değerler şunlardır:

    - **Hiçbiri**: hiçbir bağlama denenir.

    - **SdkOnly**: bağlama olacaktır değil kullanıcının derlemeler yalnızca, temel sınıf kitaplıkları üzerinde gerçekleştirilen.

    - **Tam**: bağlama gerçekleştirilecek temel sınıf kitaplıkları ve kullanıcı derlemeleri. **Not:** kullanarak bir `AndroidLinkMode` değerini *tam* genellikle sonuçları bozuk uygulamalarında özellikle yansıma kullanıldığında. Sürece kaçının, *gerçekten* gerçekleştirmekte olduğunuz bildirin.

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; noktalı virgülle ayrılmış belirtir (`;`) derlemelerin değil bağlanması gereken dosya uzantıları olmadan derleme adları listesi. Yalnızca Android uygulaması projeleri içinde kullanılır.

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; denetimleri olup noktaları sıralı bir boolean özelliği, böylece dosya adı ve satır numarası bilgilerini gelen ayıklanabilir oluşturulur `Release` Yığın izlemeleri.

    Xamarin.Android 6.1 eklendi.

-   **AndroidManifest** &ndash; uygulamanın için şablon olarak kullanmak için bir dosya adı belirtir [ `AndroidManifest.xml` ](~/android/platform/android-manifest.md).
    Derleme sırasında gerekli herhangi bir değeri gerçek üretmek için birleştirilecek `AndroidManifest.xml`.
    `$(AndroidManifest)` Paket adı içermelidir `/manifest/@package` özniteliği.

-   **AndroidSdkBuildToolsVersion** &ndash; Android SDK derleme araçlarını paket sağlar **aapt** ve **zipalign** Araçlar, diğerlerinin yanı sıra. Derleme araçları paketini birden çok farklı sürümlerini aynı anda yüklenebilir. Paketleme için seçilen derleme araçları paketini denetleniyor ve varsa, bir "tercih edilen" derleme araçları sürümüyle gerçekleştirilir; "tercih edilen" sürüm ise *değil* sunmak yüklü sürümü tutulan highested derleme Araçları Paketi kullanılır.

    `$(AndroidSdkBuildToolsVersion)` MSBuild özelliği, tercih edilen derleme Araçları sürüm içerir. Varsayılan değer Xamarin.Android yapılandırma sistemi sağlar `Xamarin.Android.Common.targets`, ve (örneğin) en son aapt çıkışı bir önceki aapt sürüm kilitlenen varsa varsayılan değer bir alternatif derleme Araçları Sürüm seçmek için youur proje dosyası içinde geçersiz kılınabilir İş adı verilir.

-   **AndroidSupportedAbis** &ndash; noktalı virgül içeren bir dize özelliği (`;`)-ayrılmış listesini içine dahil edilmesi gereken ABIs `.apk`.

    Desteklenen değerler şunlardır:

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: Xamarin.Android 5.1 ve sonrası gerektirir.
    -   `x86_64`: Xamarin.Android 5.1 ve sonrası gerektirir.

-   **AndroidUseSharedRuntime** &ndash; olan bir boolean özelliği belirler olup olmadığını *çalışma zamanı paketleri paylaşılan* hedef aygıta uygulamayı çalıştırmak için gereklidir. Paylaşılan çalışma zamanı paketleri bağlı olan küçük, paket oluşturma ve dağıtma işlemini hızlandırma, daha hızlı bir yapı/dağıtmak/debug döngü döngüsünde kaynaklanan için uygulama paketi sağlar.

    Bu özellik olmalıdır `True` hata ayıklama yapıları için ve `False` yayın projeleri için.

-   **AotAssemblies** &ndash; derlemeler Ahead-in-yerel koda derlenmiş ve dahil süresi olacak olup olmadığını belirleyen bir boolean özelliği `.apk`.

    Xamarin.Android 5.1, bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

-   **EmbedAssembliesIntoApk** &ndash; uygulamanın derlemeleri uygulama pakete katıştırılmış olup olmadığını belirleyen bir boolean özelliği.

    Bu özellik olmalıdır `True` sürüm yapıları için ve `False` hata ayıklama yapıları için. Bu *olabilir* olmasına gerek `True` hızlı dağıtım hedef aygıt desteklemiyorsa hata ayıklama derlemelerinde.

    Bu özellik olduğunda `False`, sonra `$(AndroidFastDeploymentType)` MSBuild özelliği de denetler embedd ne olacağını içine `.apk`, dağıtım ve rebuidl kez etkileyebilir.

-   **EnableLLVM** &ndash; LLVM zaman Ahead zaman kullanılan olup olmadığını belirleyen bir boolean özelliği yerel kod içine assemblines derleme.

    Xamarin.Android 5.1, bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

    Sürece bu özellik yoksayılır `$(AotAssemblies)` MSBuild özelliği `True`.

-   **EnableProguard** &ndash; belirleyen bir boolean özelliği olup olmadığına [proguard](http://developer.android.com/tools/help/proguard.html) Java kod bağlamak için paketleme işleminin bir parçası olarak çalıştırmak.

    Xamarin.Android 5.1, bu özellik için destek eklendi.

    Bu özellik `False` varsayılan olarak.

    Zaman `True`, [ProguardConfiguration](#ProguardConfiguration) dosyaları denetlemek için kullanılacak `proguard` yürütme.

-   **JavaMaximumHeapSize** &ndash; değerini belirtir. **java** 
     `-Xmx` oluştururken kullanılacak parametre değeri `.dex` dosyası paketleme işleminin bir parçası olarak. Belirtilmezse, sonra `-Xmx` için seçeneği sağlanmadığında **java**.

    Bu özellik belirtme gereklidir, [ `_CompileDex` hedef atar bir `java.lang.OutOfMemoryError` ](https://bugzilla.xamarin.com/show_bug.cgi?id=18327).

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; geçirilecek ek komut satırı seçeneklerini belirtir. **java** oluştururken `.dex` dosya.

-   **MandroidI18n** &ndash; harmanlama gibi uygulama ile birlikte gelen ve tabloları sıralama Uluslararası Destek belirtir. Değer bir veya daha büyük küçük harf duyarsız şu değerlerden biri, virgül veya noktalı virgülle ayrılmış bir listesidir:

    -   **Hiçbiri**: hiçbir ek Kodlamalar içerir.

    -   **Tüm**: tüm kullanılabilir Kodlamalar içerir.

    -   **CJK**: dahil Çince, Japonca ve Korece Kodlamalar gibi *Japonca (EUC)* \[Çözücü-jp, CP51932\], *Japonca (Shift-JIS)* \[ ISO-2022-jp, shift\_JIS, CP932\], *Japonca (JIS)* \[CP50220\], *Basitleştirilmiş Çince (GB2312)* \[gb2312, CP936\], *Kore dili (UHC)* \[görevleri\_c\_5601 1987 ' CP949\], *Kore dili (EUC)* \[euc-kr, CP51949\], *Geleneksel Çince (Big5)* \[big5, CP950\], ve *Basitleştirilmiş Çince (GB18030)* \[ GB18030, CP54936\].

    -   **MidEast**: gibi dahil Orta-Doğu Kodlamalar *Türkçe (Windows)* \[ISO-8859-9, CP1254\], *İbranice (Windows)* \[ Windows 1255, CP1255\], *Arapça (Windows)* \[windows 1256, CP1256\], *Arapça (ISO)* \[ISO-8859-6, CP28596\], *İbranice (ISO)* \[ISO-8859-8, CP28598\], *Latin 5 (ISO)* \[ISO-8859-9, CP28599\]ve *İbranice (ISO alternatif)* \[ISO-8859-8, CP38598\].

    -   **Diğer**: diğer Kodlamalar gibi içeren *Kiril (Windows)* \[CP1251\], *Baltık (Windows)* \[ISO-8859-4, CP1257\], *Vietnam dili (Windows)* \[CP1258\], *Kiril (KOI8-R)* \[KOI8-r, CP1251\], *Ukrayna dili (KOI8-U)*  \[KOI8-u, CP1251\], *Baltık (ISO)* \[ISO-8859-4, CP1257\], *Kiril (ISO)* \[ISO-8859-5, CP1251\], *ISCII Davenagari* \[x-ISCII-de, CP57002\], *ISCII Bengalce* \[x-ISCII-olmalıdır, CP57003 \], *ISCII Tamilce* \[x-ISCII-eri, CP57004\], *ISCII Telugu dili* \[x-ISCII-te, CP57005\], *ISCII Assam dili* \[x-ISCII-olarak CP57006\], *ISCII Oriya dili* \[x-ISCII-veya CP57007\], *ISCII Kannada* \[x-ISCII-ka, CP57008\], *ISCII Malayalam* \[x-ISCII-ma, CP57009\], *ISCII Gucerat dili* \[x-ISCII-gu, CP57010\], *ISCII Pencap dili* \[x-ISCII-pa, CP57011\], ve *Tay dili (Windows)*  \[CP874\].

    -   **Nadir**: gibi dahil nadir Kodlamalar *IBM EBCDIC (Türkçe)* \[CP1026\], *IBM EBCDIC (açık sistemleri Latin 1)* \[CP1047\], *IBM EBCDIC (ABD-Kanada Euro ile)* \[CP1140\], *IBM EBCDIC (Almanya Euro ile)* \[CP1141\], *IBM EBCDIC (Danimarka/Norveç Euro ile)* \[CP1142\], *IBM EBCDIC (Finlandiya/İsveç Euro ile)* \[CP1143\], *IBM EBCDIC (İtalya Euro ile)* \[CP1144\], *IBM EBCDIC (Latin Amerika/İspanya Euro ile)* \[CP1145\], *IBM EBCDIC (Birleşik Euro ile Krallık)* \[CP1146\], *IBM EBCDIC (Fransa Euro ile)* \[CP1147\], *IBM EBCDIC (uluslararası Euro ile)*  \[CP1148\], *IBM EBCDIC (İzlanda Euro ile)* \[CP1149\], *IBM EBCDIC (Almanya)* \[ CP20273\], *IBM EBCDIC (Danimarka/Norveç)* \[CP20277\], *IBM EBCDIC (Finlandiya/İsveç)* \[CP20278\], *IBM EBCDIC (İtalya)* \[CP20280\], *IBM EBCDIC (Latin Amerika/İspanya)* \[CP20284\], *IBM EBCDIC (Birleşik Krallık)* \[CP20285\], *IBM EBCDIC (Japonca Katakana genişletilmiş)* \[CP20290\], *IBM EBCDIC (Fransa)* \[CP20297\], *IBM EBCDIC (Arapça)* \[CP20420\], *IBM EBCDIC (İbranice)* \[CP20424\], *IBM EBCDIC (İzlanda)* \[ CP20871\], *IBM EBCDIC (Kiril - Sırpça, Bulgarca)* \[CP21025\], *IBM EBCDIC (ABD-Kanada)* \[ CP37\], *IBM EBCDIC (uluslararası)* \[CP500\], *Arapça (ASMO 708)* \[CP708\], *Orta Avrupa (DOS)* \[CP852\], *Kiril (DOS)* \[CP855\], *Türk (DOS)* \[CP857\], *Batı Avrupa (Euro DOS)* \[CP858\], *İbranice (DOS)* \[CP862\], *Arapça (DOS)* \[CP864\], *Rusça (DOS)* \[CP866\], *Yunanca (DOS)* \[CP869\], *IBM EBCDIC (Latin 2)* \[CP870\] ve *IBM EBCDIC (Yunanca)* \[CP875\].

    -   **Batı**: dahil Batı Kodlamalar gibi *Batı Avrupa (Mac)* \[macintosh, CP10000\], *İzlanda (Mac)* \[x-mac-İzlanda CP10079\], *Orta Avrupa (Windows)* \[ISO 8859-2, CP1250\], *Batı Avrupa (Windows)* \[ISO-8859-1 CP1252\], *Yunanca (Windows)* \[ISO-8859-7, CP1253\], *Orta Avrupa (ISO)* \[ISO 8859-2, CP28592\], *Latin 3 (ISO)* \[ISO-8859-3, CP28593\], *Yunanca (ISO)* \[ISO-8859-7, CP28597\], *Latin 9 (ISO)*  \[ISO-8859-15, CP28605\], *OEM Amerika Birleşik Devletleri* \[CP437\], *Batı Avrupa (DOS)* \[CP850\], *Portekizce (DOS)* \[CP860\], *İzlanda (DOS)* \[CP861\],  *Fransızca Kanada (DOS)* \[CP863\], ve *İskandinav (DOS)* \[CP865\].

    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchive** &ndash; denetleyen bir boolean özelliği olup olmadığını `.mSYM` yapıları ile daha sonra kullanmak için oluşturulan `mono-symbolicate`ayıklamak için &ldquo;gerçek&rdquo; filename ve satır numarası bilgileri Yayın Yığın izlemeleri.

    Bu doğru ise için varsayılan olarak &ldquo;sürüm&rdquo; hata ayıklama simgeleri etkin olan uygulamalar: `$(EmbedAssembliesIntoApk)` doğru ise, `$(DebugSymbols)` True ise ve `$(Optimize)` true'dur.

    Xamarin.Android 7.1 eklendi.

-   **AndroidVersionCodePattern** &ndash; özelleştirmek Geliştirici sağlayan bir dize özelliği `versionCode` bildiriminde.
    Bkz: [APK için sürüm kod oluşturma](~/android/deploy-test/building-apps/abi-specific-apks.md) karar verme hakkında bilgi için bir `versionCode`.

    Bazı örnekler, `abi` olan `armeabi` ve `versionCode` bildirim `123`, `{abi}{versionCode}` versionCode, üretecektir `1123` zaman `$(AndroidCreatePackagePerAbi)` true, aksi takdirde 123 değeri oluşturur.
    Varsa `abi` olan `x86_64` ve `versionCode` bildirim `44`. Bu üretecektir `544` zaman `$(AndroidCreatePackagePerAbi)` true, aksi takdirde değerini oluşturur `44`.

    Biz biçim dizesi doldurma sol eklerseniz `{abi}{versionCode:0000}`, onu msgıd `50044` biz doldurma bırakılır çünkü `versionCode` ile `0`. Alternatif olarak, aşağıdaki gibi doldurma ondalık kullanabilirsiniz `{abi}{versionCode:D4}` hangi mu önceki örnek ile aynı.

    Yalnızca '0' ve 'biçimi dizeleri değerin itibaren desteklenir doldurma Dx' bir tamsayı olmalıdır.

    Anahtar öğeleri önceden tanımlanmış

    -   **ABI** &ndash; uygulama targetted ABI ekler
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; eklemeleri desteklenen en düşük Sdk değerinden `AndroidManifest.xml` veya `11` hiçbiri tanımlanmışsa.

    -   **versionCode** &ndash; gelen sürüm kodu direrctly kullanan `Properties\AndroidManifest.xml`.

    Kullanarak özel öğeleri tanımlayabilirsiniz `AndroidVersionCodeProperties` özelliği (sonraki tanımlanmış).

    Xamarin.Android 7.2 eklendi.

-   **AndroidVersionCodeProperties** &ndash; A string property which allows the developer to define custom items to use with the `AndroidVersionCodePattern`. Biçiminde olan bir `key=value` çifti. Tüm öğeleri `value` tamsayı değeri olmalıdır. Örneğin: `screen=23;target=$(_SupportedApiLevel)`. Mevcut veya özel MSBuild kullanma yapabilir gördüğünüz dize özellikleri.

    Xamarin.Android 7.2 eklendi.


### <a name="binding-project-build-properties"></a>Bağlama proje derleme özellikleri

Aşağıdaki MSBuild özellikleri ile kullanılan [projeleri bağlama](~/android/platform/binding-java-library/index.md):

-   **AndroidClassParser** &ndash; denetleyen bir dize özelliği nasıl `.jar` dosyaları ayrıştırılır. Olası değerler şunlardır:

    - **sınıf ayrıştırma**: kullanan `class-parse.exe` Java bayt bir JVM Yardım doğrudan ayrıştırılamadı. Bu Deneysel değerdir.


    - **jar2xml**: kullanmak `jar2xml.jar` türleri ve üyeleri ayıklamak için Java yansıma kullanmak için bir `.jar` dosyası.

    Avantajları `class-parse` üzerinden `jar2xml` şunlardır:

    - `class-parse` parametre adları içeren Java bayt ayıklamak yapabiliyor *hata ayıklama* simgeler (örneğin ile bayt derlenmiş `javac -g`).

    - `class-parse` "öğesinden devralmalı ya da çözülemeyen türleri üyeleri içeren sınıflarını atlayın değil".

    **Deneysel**. Xamarin.Android 6.0 eklendi.

    Varsayılan değer `jar2xml` şeklindedir.

    Varsayılan değer, bir sonraki sürümde değiştirir.

-   **AndroidCodegenTarget** &ndash; kod oluşturma hedef ABI denetleyen bir dize özelliği. Olası değerler şunlardır:

    - **XamarinAndroid**: Android 1.0 için mevcut itibaren Mono JNI bağlama API'sini kullanır. Xamarin.Android 5.0 veya üzeri ile oluşturulmuş bağlama derlemeler için yalnızca bir Xamarin.Android 5.0 veya üzeri (API/ABI ekleme), üzerinde çalıştırılabilir, ancak *kaynak* önceki ürün sürümleri ile uyumludur.

    - **XAJavaInterop1**: kullanım Java.Interop JNI etkinleştirmeleri için. Kullanarak bağlama derlemeler `XAJavaInterop1` yalnızca derleme ve Xamarin.Android 6.1 veya sonrası yürütün. Xamarin.Android 6.1 ve üzeri bağlama `Mono.Android.dll` bu değere sahip.

      Avantajları `XAJavaInterop1` içerir:

      - Daha küçük derlemeler.

      - `jmethodID` için önbelleğe alma `base` yöntemi çağrılarını, diğer tüm bağlama devralma hiyerarşisinde türleri sürece dahili `XAJavaInterop1` veya sonraki bir sürümü.

      - `jmethodID` Java aranabilir sarmalayıcısı oluşturucuları yönetilen alt sınıflar için önbelleğe almayı.

    Varsayılan değer `XamarinAndroid` şeklindedir.

    Varsayılan değer, bir sonraki sürümde değiştirir.



### <a name="resource-properties"></a>Kaynak özellikleri

Kaynak özelliklerini kontrol nesil `Resource.designer.cs` Android kaynaklara erişim sağlayan dosya.

-   **AndroidResgenExtraArgs** &ndash; geçirilecek ek komut satırı seçeneklerini belirtir. **aapt** Android varlıklar ve kaynakları işlerken komutu.

-   **AndroidResgenFile** &ndash; oluşturmak için kaynak dosyasının adını belirtir. Varsayılan şablonu bu ayarlar `Resource.designer.cs`.

-   **MonoAndroidResourcePrefix** &ndash; belirtir bir *yol öneki* bir yapı eylemi ile dosya adlarını başından kaldırılan `AndroidResource`. Bu kaynakların bulunduğu değiştirme izin vermektir.

    Varsayılan değer `Resources` şeklindedir. Bu değişiklik `res` Java için Proje yapısı.

-   **AndroidExplicitCrunch** &ndash; yerel drawables çok fazla sayıda ile bir uygulama geliştiriyorsanız, bir başlangıç yapı (veya yeniden oluşturma) tamamlamak için dakika sürebilir. Derleme işlemi hızlandırmak için bu özelliği de dahil olmak üzere deneyin ve ayarlamak `True`. Bu özellik ayarladığınızda, yapı işlemi .png dosyaları önceden crunches.

    **Deneysel**. Xamarin.Android 7.0 eklendi.


<a name="Signing_Properties" />

### <a name="signing-properties"></a>Özellikler imzalama

Bir Android cihaz yüklenebilir nasıl uygulama paketi imzalandığı özellikleri denetim imzalama. İmzalama oldukça yavaş olduğu için daha hızlı derleme yineleme izin vermek için Xamarin.Android görevleri paketleri oluşturma işlemi sırasında oturum yok. Bunun yerine, bunlar (gerekirse) yüklemeden önce veya dışa aktarma sırasında IDE tarafından imzalanmış veya *yükleme* hedef oluşturun. Çağırma *SignAndroidPackage* hedefi olan bir paket oluşturacak `-Signed.apk` çıktı dizinine soneki.

Varsayılan olarak, imzalama hedef gerekiyorsa, yeni bir hata ayıklama imzalama anahtarı oluşturur. Örneğin belirli bir anahtarın kullanmak istiyorsanız, bir derleme sunucuda aşağıdaki MSBuild özellikleri kullanılabilir:

-   **AndroidKeyStore** &ndash; özel imzalama bilgilerini kullanılması gerekip gerekmediğini gösteren bir Boole değeri. Varsayılan değer `False`, varsayılan hata ayıklama imzalama anahtarı paketleri imzalamak için kullanılacak anlamına gelir.

-   **AndroidSigningKeyAlias** &ndash; diğer ad anahtarı için bir anahtar deposunda belirtir. Bu **keytool-diğer adı** anahtar deposu oluştururken kullanılan değer.

-   **AndroidSigningKeyPass** &ndash; anahtarı anahtar deposu dosyasının içinde parolasını belirtir. Bu değerdir ne zaman girilen `keytool` ister **$(AndroidSigningKeyAlias) için anahtar parola gir**.

-   **AndroidSigningKeyStore** &ndash; tarafından oluşturulan anahtar deposu dosyasının dosya adını belirtir `keytool`. İçin sağlanan değer bu karşılık gelen **keytool - keystore** seçeneği.

-   **AndroidSigningStorePass** &ndash; için parolayı belirtir `$(AndroidSigningKeyStore)`. İçin sağlanan değer budur `keytool` anahtar deposu dosyasının oluştururken ve sorulan **bir anahtar parola gir:**.

Örneğin, aşağıdakileri göz önünde bulundurun `keytool` çağırma:

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

Yukarıda oluşturulan anahtar deposu kullanmak için özellik grubu kullanın:

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

<a name="Build_Actions" />

## <a name="build-actions"></a>Eylemler derleme

*Eylemler yapı* olan [dosyalara uygulanmasını](http://msdn.microsoft.com/en-us/library/bb629388.aspx) denetim dosyası nasıl işlenir ve proje içinde.

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

Bir yapı eylemi dosyalarıyla `AndroidEnvironment` gerçekleştirmek için kullanılır [ortam değişkenleri ve Sistem özellikleri işlem başlatma sırasında başlatmak](~/android/deploy-test/environment.md).
`AndroidEnvironment` Yapı eylemi birden çok dosyaya uygulanan ve (aynı ortam değişkeni veya sistem özelliği birden çok dosyada belirtmeyin şekilde) belirli bir sırada olarak değerlendirilir.


### <a name="androidjavasource"></a>AndroidJavaSource

Bir yapı eylemi dosyalarıyla `AndroidJavaSource` son Android paketinde bulunan Java kaynak kodu.


### <a name="androidjavalibrary"></a>AndroidJavaLibrary

Bir yapı eylemi dosyalarıyla `AndroidJavaLibrary` Java arşivleri olan ( `.jar` dosyaları), dahil edilir son Android paketinde.


### <a name="androidresource"></a>AndroidResource

Tüm dosyaları bir *AndroidResource* yapı eylemi Android kaynakları oluşturma işlemi sırasında derlenir ve aracılığıyla erişilebilir hale `$(AndroidResgenFile)`.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

Daha gelişmiş kullanıcılar belki farklı yapılandırmaları, ancak aynı etkili yolu ile kullanılan farklı kaynaklarınız isteyebilir. Bu, birden fazla kaynak dizine sahip ve bu farklı dizinleri içindeki aynı göreli yollara dosyalarıyla sahip ve koşullu farklı yapılandırmalarda farklı dosyaları dahil etmek için MSBuild koşulları kullanılarak gerçekleştirilebilir. Örneğin:

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

**LogicalName** &ndash; açıkça kaynak yolunu belirtir. Verir &ldquo;yumuşatma&rdquo; birden çok farklı kaynak adları olarak kullanılabilir olmaları böylece dosyaları.

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

[Yerel kitaplıkları](~/android/platform/native-libraries.md) yapı kendi yapı eylemi ayarlayarak eklenen `AndroidNativeLibrary`.

Derleme Sistemi Android birden çok uygulama ikili arabirimi (ABIs) desteklediğinden, yerel kitaplığı hangi ABI bilmelisiniz not için yerleşik olarak bulunur. Bu yapılabilir iki yolu vardır:

1.  Yol "algılaması".
2.  Kullanarak `Abi` öğesi özniteliği.

Yol algılaması ile yerel kitaplığı üst dizin adını ABI belirtmek için kullanılır, kitaplık hedefler. Bu nedenle, eklerseniz `lib/armeabi/libfoo.so` yapı, ardından ABI "olarak sniffed" `armeabi`.


### <a name="item-attribute-name"></a>Öğesi özniteliği adı

**ABI** &ndash; yerel kitaplığı ABI belirtir.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```

### <a name="content"></a>İçerik

Normal `Content` yapı eylemi (biz büyük olasılıkla maliyetli ilk çalıştırma adım desteklemek nasıl dahil edilir henüz gibi) desteklenmez.

Xamarin.Android thw kullanılmaya çalışılıyor 5.1, başlangıç `@(Content)` yapı eylemi neden olur bir `XA0101` uyarı.

### <a name="linkdescription"></a>LinkDescription

İle dosyaları bir *LinkDescription* yapı eylemi için kullanılan [bağlayıcı davranışını denetleyen](~/cross-platform/deploy-test/linker.md).


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

İle dosyaları bir *ProguardConfiguration* yapı eylemini içeren denetlemek için kullanılan seçenekler `proguard` davranışı. Bu yapı eylem hakkında daha fazla bilgi için bkz: [ProGuard](~/android/deploy-test/release-prep/proguard.md).

Bu dosyalar yoksayılır `$(EnableProguard)` MSBuild özelliği `True`.



## <a name="target-definitions"></a>Hedef tanımları

Derleme işlemi Xamarin.Android özgü bölümlerini tanımlanan `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets`, ancak normal dile özgü hedefleri gibi *Microsoft.CSharp.targets* derleme oluşturma için de gereklidir.

Tüm dil hedefleri almadan önce aşağıdaki yapı özelliklerini ayarlamanız gerekir:

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

Bunların tümü bu hedefler ve özellikler olabilir C# ' ta dahil içeri aktararak *Xamarin.Android.CSharp.targets*:

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

Bu dosyayı kolayca diğer diller için uyarlanabilir.
