---
title: ProGuard
description: ProGuard Java sınıfı dosya shrinker, en iyi hale getirme, obfuscator ve öncesi Doğrulayıcı ' dir. Bunu algılar ve kullanılmayan kodunu kaldırır, analiz eder ve bayt en iyi duruma getirir ve sınıflar ve sınıf üyeleri bilgisayardan farklı gösterir. Bu kılavuz, ProGuard nasıl çalıştığını, projenizde etkinleştirme ve nasıl yapılandırılacağı açıklanmaktadır. Ayrıca, ProGuard yapılandırmaları bazı örnekleri sağlar.
ms.prod: xamarin
ms.assetid: 29C0E850-3A49-4618-9078-D59BE0284D5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: e65c78633ae91318bd8e9cce949bac9cc12675c0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771450"
---
# <a name="proguard"></a>ProGuard

_ProGuard Java sınıfı dosya shrinker, en iyi hale getirme, obfuscator ve öncesi Doğrulayıcı ' dir. Bunu algılar ve kullanılmayan kodunu kaldırır, analiz eder ve bayt en iyi duruma getirir ve sınıflar ve sınıf üyeleri bilgisayardan farklı gösterir. Bu kılavuz, ProGuard nasıl çalıştığını, projenizde etkinleştirme ve nasıl yapılandırılacağı açıklanmaktadır. Ayrıca, ProGuard yapılandırmaları bazı örnekleri sağlar._


## <a name="overview"></a>Genel Bakış

ProGuard algılar ve paketlenmiş uygulama içinden kullanılmayan sınıfları, alanları, yöntemleri ve öznitelikleri kaldırır. Bile aynı başvurulan kitaplıkları için bunu yapabilirsiniz (Bu, 64 k başvuru sınırı önlemenize yardımcı olabilir). Android SDK ProGuard aracından ayrıca bayt en iyi duruma getirme, kullanılmayan kod yönergeleri kaldırın ve kalan sınıfları, alanları ve daha kısa adlar yöntemleriyle belirsizleştirirseniz. ProGuard okuma **giriş Kavanoz** ve ardından küçültür, en iyi duruma getirir, bilgisayardan farklı gösterir ve bunları; önceden doğrular bir veya daha fazla sonuçları Yazar **çıkış Kavanoz**. 

APK'ın aşağıdaki adımları kullanarak proGuard işlemleri giriş: 

1.  **Adım küçültme** &ndash; ProGuard yinelemeli olarak belirler, sınıflar ve sınıf üyeleri kullanılır. Diğer tüm sınıflar ve sınıf üyeleri atılır. 

2.  **En iyi duruma getirme adım** &ndash; ProGuard daha fazla kod iyileştirir. 
    Diğer en iyi duruma getirme, sınıflar ve giriş noktaları özel, statik ya da son yapılan olmayan yöntemler arasında kullanılmayan parametreleri kaldırılabilir ve bazı yöntemler içermesinden olabilir. 

3.  **Gizleme adım** &ndash; ProGuard sınıflar ve giriş noktaları sınıf üyeleri yeniden adlandırır. Giriş noktaları korur, bunlar hala özgün adlarıyla erişilip erişilemediğini sağlar. 

4.  **Preverification adım** &ndash; Java bytecodes çalışma zamanı öncesinde üzerinde denetimleri gerçekleştirir ve sınıf dosyaları Java VM yararlanması açıklama ekler. Giriş noktaları bilmek zorunda değil tek adım budur. 

Bu adımların her biri olan *isteğe bağlı*. Xamarin.Android ProGuard, sonraki bölümde açıklandığı gibi bu adımları yalnızca bir kısmı kullanır. 



## <a name="proguard-in-xamarinandroid"></a>Xamarin.Android proGuard

Xamarin.Android ProGuard yapılandırma APK belirsizleştirirseniz değil. Aslında, ProGuard aracılığıyla gizleme (hatta kullanımı ile özel yapılandırma dosyaları) etkinleştirmek mümkün değildir. Bu nedenle, Xamarin.Android'ın ProGuard yalnızca gerçekleştirir **küçültme** ve **en iyi duruma getirme** adımları: 

[![Daraltma ve en iyi duruma getirme adımları](proguard-images/01-xa-chain-sml.png)](proguard-images/01-xa-chain.png#lightbox)

ProGuard kullanarak içinde nasıl çalışır önce önceden bilmek önemli bir öğe `Xamarin.Android` derleme işlemi. Bu işlem iki ayrı adımları kullanır: 

1.  Xamarin Android Linker

2.  ProGuard

Bu adımların her biri sonraki açıklanmıştır.



### <a name="linker-step"></a>Bağlayıcı adım

Xamarin.Android bağlayıcı aşağıdakileri belirlemek için uygulamanızın statik analiz uygular: 

-   Hangi derlemelerin gerçekte kullanılır.

-   Hangi tür gerçekte kullanılır.

-   Hangi üyeleri gerçekte kullanılır. 

Bağlayıcı ProGuard adımdan önce her zaman çalışır. Bu nedenle, bir derleme/türü/çalıştırmak için ProGuard bekleyebilirsiniz üyeyi bağlayıcı Şerit. (Xamarin.Android içinde bağlama hakkında daha fazla bilgi için bkz: [Android bağlama](~/android/deploy-test/linker.md).)



### <a name="proguard-step"></a>ProGuard adım

Bağlayıcı adım başarıyla tamamlandıktan sonra ProGuard kullanılmayan Java bayt kaldırmak için çalıştırılır. APK en iyi duruma getirir adım budur. 



## <a name="using-proguard"></a>ProGuard kullanma

Uygulama projenizde ProGuard kullanmak için önce ProGuard etkinleştirmeniz gerekir. Ardından, varsayılan ProGuard yapılandırma dosyası derleme işlemi kullanma Xamarin.Android ya da izin verebilir veya kendi özel yapılandırma dosyası kullanmak ProGuard oluşturabilirsiniz. 



### <a name="enabling-proguard"></a>ProGuard etkinleştirme

Uygulama projenizde ProGuard etkinleştirmek için aşağıdaki adımları kullanın:

1.  Projenizi ayarlandığından emin olun **sürüm** yapılandırma (Bu önemlidir çünkü çalıştırmak ProGuard bağlayıcı çalıştırmanız gerekir): 

    [![Yayın yapılandırmasını seçin](proguard-images/02-set-release-sml.png)](proguard-images/02-set-release.png#lightbox)
   
2.  Denetleyerek ProGuard etkinleştirmek **etkinleştirmek ProGuard** altında seçeneği **paketleme** sekmesinde **Özellikler > Android seçenekleri**: 

    [![Seçili Proguard seçeneğini etkinleştirin](proguard-images/03-enable-proguard-sml.png)](proguard-images/03-enable-proguard.png#lightbox)

Xamarin.Android uygulamaları için tüm kaldırmak yeterli (ve yalnızca) Xamarin.Android tarafından sağlanan varsayılan ProGuard yapılandırma dosyası olacaktır kullanılmayan kodu. Varsayılan ProGuard yapılandırmasını görüntülemek için konumundaki dosyayı açın **obj\\sürüm\\proguard\\proguard_xamarin.cfg**. Sonraki bölümde özelleştirilmiş ProGuard yapılandırma dosyasının nasıl oluşturulacağı açıklanmaktadır. 



### <a name="customizing-proguard"></a>ProGuard özelleştirme

İsteğe bağlı olarak, ProGuard araçları hakkında daha fazla denetime kullanmak için özel bir ProGuard yapılandırma dosyası ekleyebilirsiniz. Örneğin, ProGuard korumak için hangi sınıfların açıkça bildirmek isteyebilirsiniz. Bunu yapmak için yeni bir oluşturma **.cfg** dosya ve uygulama `ProGuardConfiguration` derleme eylemi **özellikleri** bölmesinde **Çözüm Gezgini**: 

[![Seçili ProguardConfiguration yapı eylem](proguard-images/04-build-action-sml.png)](proguard-images/04-build-action.png#lightbox)

Bu yapılandırma dosyası Xamarin.Android değiştirmez göz önünde bulundurmanız **proguard_xamarin.cfg** her ikisi de ProGuard tarafından kullanıldığından dosya. 

Aşağıdaki örnek bir tipik ProGuard yapılandırma dosyası gösterilmektedir:
    

    # This is Xamarin-specific (and enhanced) configuration.

    -dontobfuscate

    -keep class mono.MonoRuntimeProvider { *; <init>(...); }
    -keep class mono.MonoPackageManager { *; <init>(...); }
    -keep class mono.MonoPackageManager_Resources { *; <init>(...); }
    -keep class mono.android.** { *; <init>(...); }
    -keep class mono.java.** { *; <init>(...); }
    -keep class mono.javax.** { *; <init>(...); }
    -keep class opentk.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk.GameViewBase { *; <init>(...); }
    -keep class opentk_1_0.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk_1_0.GameViewBase { *; <init>(...); }

    -keep class android.runtime.** { <init>(***); }
    -keep class assembly_mono_android.android.runtime.** { <init>(***); }
    # hash for android.runtime and assembly_mono_android.android.runtime.
    -keep class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }
    -keepclassmembers class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }

    # Android's template misses fluent setters...
    -keepclassmembers class * extends android.view.View {
       *** set*(***);
    }

    # also misses those inflated custom layout stuff from xml...
    -keepclassmembers class * extends android.view.View {
       <init>(android.content.Context,android.util.AttributeSet);
       <init>(android.content.Context,android.util.AttributeSet,int);
    }
    

ProGuard düzgün bir şekilde uygulamanızı analiz alamadı olduğu durumlar olabilir; Ayrıca, uygulamanız gereken gerçek kodu potansiyel olarak kaldırabilirsiniz. Ekleyebileceğiniz bu ortaya çıkarsa, bir `-keep` özel ProGuard yapılandırma dosyanızın satır: 

    -keep public class MyClass

Bu örnekte, `MyClass` atlamak için ProGuard istediğiniz sınıfı gerçek adına ayarlayın.

Ayrıca, kendi adlarıyla kaydedebilirsiniz `[Register]` ek açıklamalar ve bunlar adları ProGuard kurallarını özelleştirmek için kullanın. Bağdaştırıcılar, görünümler, BroadcastReceivers, hizmetleri, ContentProviders, etkinlikler ve parçaları için adları kaydedebilirsiniz. Kullanma hakkında daha fazla bilgi için `[Register]` özel öznitelik bkz [JNI ile çalışma](~/android/platform/java-integration/working-with-jni.md).


### <a name="proguard-options"></a>ProGuard seçenekleri

ProGuard çalışması üzerinde daha hassas denetim sağlayan yapılandırdığınız seçenekler sunar. [ProGuard el ile](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/introduction.html) ProGuard birini kullanmak için eksiksiz belgeler sağlar. 

Xamarin.Android aşağıdaki ProGuard seçeneklerini destekler: 


-    [Giriş/Çıkış Seçenekleri](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#iooptions)

-    [Saklama Seçenekleri](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptions)

-    [Seçenekler küçültme](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#shrinkingoptions)

-    [Genel seçenekleri](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#generaloptions)

-    [Sınıf yolları](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classpath)

-    [Dosya adları](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filename)

-    [Dosya filtreleri](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filefilters)

-    [Filtreler](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filters)

-    [Genel Bakış `Keep` seçenekleri](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoverview)

-    [Seçenek değiştiricileri tutun](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptionmodifiers)

-    [Sınıf belirtimleri](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classspecification)

Aşağıdaki seçenekler *göz ardı* Xamarin.Android tarafından:

-    [En iyi duruma getirme seçenekleri](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#optimizationoptions)

-    [Gizleme seçenekleri](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#obfuscationoptions) 

-    [Preverification seçenekleri](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#preverificationoptions)



## <a name="proguard-and-android-nougat"></a>ProGuard ve Android Nougat

Android 7.0 karşı ya da daha sonra ProGuard çalışıyorsanız, Android SDK JDK 1.8 ile uyumlu olan yeni bir sürümle gelmez çünkü ProGuard daha yeni bir sürümü indirmeniz gerekir.

Bu kullanabilirsiniz [NuGet paketi](https://www.nuget.org/packages/name.atsushieno.proguard.facebook/5.3.0) daha yeni bir sürümünü yüklemek için `proguard.jar`. Android SDK varsayılan güncelleştirme hakkında daha fazla bilgi için `proguard.jar`, bu bkz [yığın taşması](http://stackoverflow.com/questions/39514518/xamarin-android-proguard-unsupported-class-version-number-52-0/39514706#39514706) tartışma.

ProGuard'in tüm sürümleri bulabilirsiniz [SourceForge sayfa](https://sourceforge.net/projects/proguard/files/). 



## <a name="example-proguard-configurations"></a>Örnek ProGuard yapılandırmaları

İki örnek ProGuard yapılandırma dosyaları, aşağıda listelenmiştir. Lütfen bu gibi durumlarda Xamarin.Android derleme işlemi, Not sağlayacak **giriş**, **çıkış**, ve **Kitaplığı** Kavanoz. Bu nedenle, gibi diğer seçenekleri odaklanabilirsiniz `-keep`. 

### <a name="a-simple-android-activity"></a>Basit bir Android etkinliği

Aşağıdaki örnekte basit bir Android etkinlik yapılandırması gösterilmektedir:

    -injars  bin/classes
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic

    -keep public class mypackage.MyActivity

### <a name="a-complete-android-application"></a>Tam bir Android uygulaması

Aşağıdaki örnek, tam bir Android uygulaması için yapılandırmayı gösterir:

    -injars  bin/classes
    -injars  libs
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic
    -keepattributes *Annotation*

    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Application
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.content.ContentProvider

    -keep public class * extends android.view.View {
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
    public void set*(...);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet, int);
    }

    -keepclassmembers class * implements android.os.Parcelable {
    static android.os.Parcelable$Creator CREATOR;
    }

    -keepclassmembers class **.R$* {
    public static <fields>;
    }


## <a name="proguard-and-the-xamarinandroid-build-process"></a>ProGuard ve Xamarin.Android derleme işlemi

Aşağıdaki bölümlerde, ProGuard sırasında bir Xamarin.Android nasıl çalıştığı açıklanmıştır **sürüm** oluşturun.


### <a name="what-command-is-proguard-running"></a>Hangi komut ProGuard çalışıyor mu?

ProGuard olan yalnızca bir `.jar` Android SDK'sı ile sağlanan. Bu nedenle, bir komut çağrılır: 

```shell
java -jar proguard.jar options ...
```

### <a name="the-proguard-task"></a>ProGuard görevi

ProGuard görev içinde bulunan **Xamarin.Android.Build.Tasks.dll** derleme. Parçası olan `_CompileToDalvikWithDx` bir parçasıdır hedef, `_CompileDex` hedef. 

Aşağıdaki liste varsayılan parametreler ilişkin bir örnek sağlar kullanarak yeni bir proje oluşturun, sonra oluşturulan **Dosya > Yeni proje**: 

    ProGuardJarPath = C:\Android\android-sdk\tools\proguard\lib\proguard.jar
    AndroidSdkDirectory = C:\Android\android-sdk\
    JavaToolPath = C:\Program Files (x86)\Java\jdk1.8.0_92\\bin
    ProGuardToolPath = C:\Android\android-sdk\tools\proguard\
    JavaPlatformJarPath = C:\Android\android-sdk\platforms\android-25\android.jar
    ClassesOutputDirectory = obj\Release\android\bin\classes
    AcwMapFile = obj\Release\acw-map.txt
    ProGuardCommonXamarinConfiguration = obj\Release\proguard\proguard_xamarin.cfg
    ProGuardGeneratedReferenceConfiguration = obj\Release\proguard\proguard_project_references.cfg
    ProGuardGeneratedApplicationConfiguration = obj\Release\proguard\proguard_project_primary.cfg
    ProGuardConfigurationFiles

      {sdk.dir}tools\proguard\proguard-android.txt;
      {intermediate.common.xamarin};
      {intermediate.references};
      {intermediate.application};
      ;
     
    JavaLibrariesToEmbed = C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar
    ProGuardJarInput = obj\Release\proguard\__proguard_input__.jar
    ProGuardJarOutput = obj\Release\proguard\__proguard_output__.jar
    DumpOutput = obj\Release\proguard\dump.txt
    PrintSeedsOutput = obj\Release\proguard\seeds.txt
    PrintUsageOutput = obj\Release\proguard\usage.txt
    PrintMappingOutput = obj\Release\proguard\mapping.txt

IDE içinden çalıştırıldığında tipik bir ProGuard komut sonraki örnek gösterilmektedir:

```cmd
C:\Program Files (x86)\Java\jdk1.8.0_92\\bin\java.exe -jar C:\Android\android-sdk\tools\proguard\lib\proguard.jar -include obj\Release\proguard\proguard_xamarin.cfg -include obj\Release\proguard\proguard_project_references.cfg -include obj\Release\proguard\proguard_project_primary.cfg "-injars 'obj\Release\proguard\__proguard_input__.jar';'C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar'" "-libraryjars 'C:\Android\android-sdk\platforms\android-25\android.jar'" -outjars "obj\Release\proguard\__proguard_output__.jar" -optimizations !code/allocation/variable
```

## <a name="troubleshooting"></a>Sorun giderme

### <a name="file-issues"></a>Dosya sorunları

ProGuard yapılandırma dosyası okuduğunda aşağıdaki hata iletisi görüntülenebilir: 

    Unknown option '-keep' in line 1 of file 'proguard.cfg'

Bu sorun genellikle çünkü Windows olur `.cfg` dosya sahip yanlış kodlaması. ProGuard işleyemez _Unicode bayt sırası işareti_ (BOM) hangi olabilir metin dosyalarında mevcut. Bir ürün reçetesi varsa, ProGuard yukarıdaki hatasıyla çıkılacak. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bu sorunu önlemek için bir ürün reçetesi kaydedilecek dosya sağlayacak bir metin düzenleyicisi özel yapılandırma dosyasından düzenleyin. Bu sorunu çözmek için metin düzenleyici kodlaması kümesine sahip olduğundan emin olun `UTF-8`. Örneğin, metin düzenleyici [not defteri ++](https://notepad-plus-plus.org/) AĞACI olmayan dosyaları seçerek kaydedebilirsiniz **kodlama &gt; UTF-8 olmadan BOM kodla** Dosya kaydedilirken. 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu sorunu önlemek için ürün reçetesi atlamaya izin veren bir metin Düzenleyicisi'nden özel yapılandırma dosyanızı kaydedin. 

-----


### <a name="other-issues"></a>Diğer Sorunlar

ProGuard [sorun giderme](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/troubleshooting.html) sayfa anlatılmaktadır karşılaşabileceğiniz ortak sorunları (ve çözümleri) ProGuard kullanırken.


## <a name="summary"></a>Özet

Bu kılavuz, ProGuard Xamarin.Android içinde nasıl çalıştığını, uygulama projenizde etkinleştirme ve nasıl yapılandırılacağı açıklanmıştır. Örnek ProGuard yapılandırmaları sağlanan ve sık karşılaşılan sorunların çözümleri açıklanmıştır. Android ve ProGuard aracı hakkında daha fazla bilgi için bkz: [kodunuzu küçültmek ve kaynakları](http://developer.android.com/tools/help/proguard.html). 


## <a name="related-links"></a>İlgili bağlantılar

- [Bir uygulama sürüm için hazırlama](~/android/deploy-test/release-prep/index.md)
