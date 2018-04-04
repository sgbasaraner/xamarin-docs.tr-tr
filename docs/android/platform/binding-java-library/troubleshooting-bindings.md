---
title: Sorun giderme bağlamaları
description: Bu makalede, olası nedenleri ve bunları çözmek için önerilen yol birlikte bağlamaları oluştururken oluşabilir serveral sık karşılaşılan hataların özetlenmektedir.
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: da6286eed091114c117c723f462bbb8cac77034b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting-bindings"></a>Sorun giderme bağlamaları

_Bu makalede, olası nedenleri ve bunları çözmek için önerilen yol birlikte bağlamaları oluştururken oluşabilir serveral sık karşılaşılan hataların özetlenmektedir._


## <a name="overview"></a>Genel Bakış

Bir Android kitaplığıdır bağlama (bir **.aar** veya **.jar**) dosya nadiren basit affair; genellikle Java ve .NET arasındaki farklar kaynaklanan sorunları gidermek için ek çaba gerektirir.
Bu sorunları Android kitaplığı bağlama Xamarin.Android önlemek ve kendilerini derleme günlüğünde hata iletilerinde olarak sunar. Bu kılavuzda sorunlarını gidermek için bazı ipuçları sağlar, daha sık karşılaşılan sorunları/senaryolardan bazıları listelemek ve başarıyla Android kitaplığı bağlama için olası çözümler sunar.

Varolan bir Android kitaplığıdır bağlanırken, aşağıdaki noktaları göz önünde bulundurmanız gereklidir:

- **Kitaplık için dış bağımlılıklar** &ndash; Android kitaplığı tarafından gerekli herhangi bir Java bağımlı dahil, Xamarin.Android projesinde bir **ReferenceJar** veya farklı bir  **EmbeddedReferenceJar**.

- **Atamak olmayan Android kitaplığıdır Android API düzeyini** &ndash; Xamarin.Android bağlama proje düzeyi (veya sonrası) aynı API hedeflediğinden emin olun; "Android API düzeyini düşürmek" olası değil olarak Android kitaplığıdır.

- **Android kitaplığı paketlemek için kullanılan Android JDK sürümü** &ndash; bağlama hataları oluşabilir Android kitaplığı tarafından Xamarin.Android JDK kullanımda olandan farklı bir sürümü ile oluşturuldu. Mümkünse, Xamarin.Android yüklemenizin tarafından kullanılan JDK'ın aynı sürümünü kullanarak Android kitaplığı yeniden derleyin.

Xamarin.Android kitaplığı bağlama ile sorunlarını giderme ilk adımı etkinleştirmektir [tanılama MSBuild çıktı](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).
Tanılama çıktıları etkinleştirdikten, Xamarin.Android bağlama projeyi oluşturmak ve sorunun nedenini nedir hakkında ipuçları bulmak için derleme günlüğünü inceleyin.

Bu ayrıca Android kitaplığı uygulayamaz ve türleri ve Xamarin.Android bağlamak için çalışıyor yöntemleri incelemek yararlı. Bu, daha sonra bu kılavuzda daha ayrıntılı ele alınmıştır.


## <a name="decompiling-an-android-library"></a>Bir Android kitaplığıdır decompiling

Sınıflar ve yöntemler Java sınıfların inceleyerek bir kitaplık bağlama yardımcı olacak değerli bilgiler sağlayabilir.
[JD GUI](http://jd.benow.ca/) Java kaynak kodundan görüntüleyen bir grafik aracıdır **sınıfı** JAR içinde yer alan dosyaları. Bu bir tek başına uygulama veya bir eklenti olarak Intellij veya Eclipse için çalıştırılabilir.

Bir Android kitaplığıdır açık derlemesini **. JAR** Java decompiler dosyasıyla. Kitaplık ise bir **. AAR** , onu dosyadır dosyasını ayıklamak için gerekli **classes.jar** arşiv dosyasından. Çözümlenecek JD GUI kullanarak bir örnek ekran görüntüsü aşağıda verilmiştir [Picasso](http://square.github.io/picasso/) JAR:

![Java Decompiler picasso 2.5.2.jar analiz etmek için kullanma](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Android kitaplığı decompiled sonra kaynak kodu inceleyin. Genel olarak bakıldığında, şunlara dikkat edin:

- **Gizleme özelliklerine sınıfları** &ndash; karıştırılmış sınıfların özellikleri içerir:

    - Sınıf adını içeren bir **$**, yani **$.class**
    - Sınıf adı tamamen küçük harf karakter yani tehlikeye **a.class**      

- **`import` başvurulmayan kitaplıkları deyimleri** &ndash; başvurulmayan kitaplığı tanımlamak ve bu bağımlılıkların Xamarin.Android bağlama projeyle ekleyip bir **yapı eylemi** , **ReferenceJar**  veya **EmbedddedReferenceJar**.

> [!NOTE]
> Java kitaplığı decompiling olabilir yasaklanmış veya yasal sınırlamalara tabidir yerel yasalarınız veya altında Java kitaplığı yayımlanan lisans göre. Gerekirse, yasal professional Hizmetleri Java kitaplığı uygulayamaz ve kaynak kodunu incelemek üzere çalışmadan önce kaydettirin.


## <a name="inspect-apixml"></a>API inceleyin. XML

Bağlama proje oluşturmanın bir parçası olarak bir XML dosya adı Xamarin.Android oluşturacak **obj/Debug/api.xml**:

![Obj/Debug altında oluşturulan api.xml](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

Bu dosya, Xamarin.Android bağlama çalışıyor tüm Java API listesini sağlar. Bu dosyanın içeriğini eksik türleri ve yöntemleri tanımlamak, bağlama yinelenen yardımcı olabilir. Bu dosyanın denetleme yorucu ve zaman alıcı olsa da, tüm bağlama sorunlara neden üzerinde ipuçları için sağlayabilirsiniz. Örneğin, **api.xml** bir özellik uygun olmayan bir tür döndüren veya iki olan yönetilen aynı adı paylaşan türleri ortaya çıkarabilir.


## <a name="known-issues"></a>Bilinen Sorunlar

Bu bölümde bazı yaygın hata iletileri veya Belirtiler listelenecektir, my bir Android kitaplığıdır bağlamak çalışırken oluşur.


### <a name="problem-java-version-mismatch"></a>Sorun: Java sürüm uyuşmazlığı

Bazen türleri oluşturulmayacak veya beklenmeyen çökme (Crash) ya da Java ne kitaplığı ile derlenen için karşılaştırıldığında daha yeni veya eski bir sürümü kullandığınız nedeniyle oluşabilir. Android kitaplığı Xamarin.Android projenizi kullanarak JDK aynı sürümüyle yeniden derleyin.


### <a name="problem-at-least-one-java-library-is-required"></a>Sorun: en az bir Java kitaplığı gereklidir

Hata "en az bir Java kitaplığı gereklidir" rağmen aldığınız bir. JAR eklendi.

#### <a name="possible-causes"></a>Olası nedenler:

Emin olun derleme eylemi ayarlanmış `EmbeddedJar`. Birden çok derleme eylemleri olduğundan. JAR dosyalarını (gibi `InputJar`, `EmbeddedJar`, `ReferenceJar` ve `EmbeddedReferenceJar`), bağlama Oluşturucu otomatik olarak varsayılan olarak kullanmak üzere hangisinin tahmin edilemez. Yapı eylemleri hakkında daha fazla bilgi için bkz: [yapı eylemleri](~/android/platform/binding-java-library/index.md).


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>Sorun: araçlar bağlama yüklenemiyor. JAR kitaplığı

Bağlama kitaplığı Oluşturucusu yüklenemiyordur. JAR kitaplığı.

#### <a name="possible-causes"></a>Olası Nedenler

Bazı. Kod gizleme (aracılığıyla araçları Proguard gibi) kullanan JAR kitaplıkları Java araçları tarafından yüklenemez. Java yansıma kullanımını ve kitaplık mühendislik ASM bayt kodu aracımız yapar olduğundan, bu bağımlı araçları Android çalışma zamanı araçları iletebilir sırada karıştırılmış kitaplıkları reddedebilir. Elle-bağlama Oluşturucu kullanmak yerine bu kitaplıklar bağlamak için bu çözüm olabilir.



### <a name="problem-missing-c-types-in-generated-output"></a>Sorun: C# oluşturulan çıktı türlerinde eksik.

Bağlama **.dll** derlemeleri ancak bazı Java türleri İsabetsiz Okuma ya da oluşturulan C# kaynak eksik tür vardır bildiren bir hata nedeniyle oluşmuyor.

#### <a name="possible-causes"></a>Olası nedenler:

Bu hata, aşağıda listelenen çeşitli nedenlerden ötürü oluşabilir:

-   Bağlanan kitaplığı ikinci bir Java kitaplığı başvurabilir. Genel API ilişkili kitaplık için ikinci kitaplığından türleri kullanıyorsa, yönetilen bir bağlama de ikinci kitaplığın başvurmalıdır.

-   Bir kitaplık Java yansıma, yukarıda, meta verileri beklenmeyen yüklenmesini neden kitaplığı yükleme hatası nedeni benzer nedeniyle eklendi mümkündür. Xamarin.Android'in araç şu anda bu durum çözümlenemiyor. Böyle bir durumda kitaplığı el ile bağlanmalıdır.

-   Olması gereken zaman derlemeleri yüklenemedi .NET 4.0 çalışma zamanında bir hata oluştu. .NET 4.5 çalışma zamanı'nda bu sorun düzeltilmiştir.

-   Genel olmayan sınıfından ortak sınıf türetme Java verir, ancak bu .NET içinde desteklenmiyor. Bağlama Oluşturucu genel sınıfları için bağlamaları oluşturmadığından bunlar doğru üretilemiyor gibi tarafından türetilmiş sınıfları. Bu sorunu gidermek için meta veri girişi Kaldır düğümünde kullanarak bu türetilmiş sınıflar için ya da kaldırmak **Metadata.xml**, veya genel olmayan sınıf ortak yapma meta verileri düzeltin. C# kaynak oluşturacaksınız böylece ikinci çözüm bağlama oluşturmanıza karşın, genel olmayan sınıf kullanılmamalıdır.

    Örneğin:

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Java kitaplıkları belirsizleştirirseniz araçları bağlama Xamarin.Android oluşturucu ve C# sarmalayıcı sınıfları oluşturmak yeteneğini etkileyebilir. Aşağıdaki kod parçacığını nasıl güncelleştirileceğini gösterir **Metadata.xml** bir sınıf adı unobfuscate için:

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>Sorun: Parametre türü uyuşmazlığı nedeniyle oluşturulan C# kaynak derleme değil

Oluşturulan C# kaynak oluşmuyor. Yöntemin parametre türleri eşleşmiyor geçersiz kılındı.

#### <a name="possible-causes"></a>Olası nedenler:

Xamarin.Android C# bağlamaları numaralandırmaları eşlendiği Java alanları çeşitli içerir. Bu tür uyumsuzlukları oluşturulan bağlamaları neden olabilir. Bu sorunu çözmek için bağlama üreticisinden oluşturulan yöntem imzaları numaralandırmaları kullanmak için değiştirilmesi gerekir. Daha fazla imformation için lütfen bkz. [düzeltme numaralandırmaları](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md).

### <a name="problem-noclassdeffounderror-in-packaging"></a>Sorun: NoClassDefFoundError paketinde

`java.lang.NoClassDefFoundError` paketleme adımda atılır.

#### <a name="possible-causes"></a>Olası nedenler:

Bu hatanın en olası nedeni olan zorunlu bir Java kitaplığı uygulama projesine eklenmesi gerekir (**.csproj**). . JAR dosyalarını otomatik olarak çözümlenmeyen. Java kitaplığı bağlama hedef cihaz veya öykünücü var olmayan bir kullanıcı derlemesi karşı her zaman oluşturulmaz (Google haritalar gibi **maps.jar**). Bu durum Android kitaplığı proje desteği için kitaplık olarak değil. JAR kitaplığı DLL'de katıştırılır. Örneğin: [hata 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>Sorun: özel EventArgs türleri Çoğalt

Derleme nedeniyle yinelenen özel EventArgs türleri başarısız oluyor. Böyle bir hata oluşur:

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>Olası nedenler:

Aynı olan yöntemleri paylaşan birden çok arabirim "dinleyicisi" türünden gelen olay türleri arasındaki bazı çakışması olduğundan budur. Örneğin, aşağıdaki örnekte görüldüğü gibi iki Java arabirimi varsa Oluşturucu yaratır `DismissScreenEventArgs` her ikisi için de `MediationBannerListener` ve `MediationInterstitialListener`, sonuçta elde edilen hata.

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

Bu, böylece olay bağımsız değişken türleri uzun adları kaçınılması tasarım gereğidir. Bu çakışmaları önlemek için bazı meta veri dönüştürme gereklidir. Düzen [ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml) ve ekleme bir `argsType` özniteliği arabirimler birini (veya arabirim yöntemini):

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### <a name="problem-class-does-not-implement-interface-method"></a>Sorun: Sınıf arabirim yöntemini uygulamıyor

Oluşturulan sınıf oluşturulan sınıf uygulayan bir arabirim için gerekli olan bir yöntem uygulamıyor belirten bir hata iletisi oluşturulur. Ancak, oluşturulan koda bakmak, yöntemi uygulanır görebilirsiniz.

Hata bir örneği burada verilmiştir:

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>Olası nedenler:

Bu Java yöntemleri eşdeğişken dönüş türleri ile bağlama ile ortaya çıkan bir sorundur. Bu örnekte, yöntem `Oauth.Signpost.Http.IHttpRequest.UnWrap()` döndürmesi gerekir `Java.Lang.Object`. Ancak, yöntem `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` dönüş türüne sahip `HttpURLConnection`. Bu sorunu gidermek için iki yolu vardır:

-   İçin bir parçalı sınıf bildirimi ekleyin `HttpURLConnectionRequestAdapter` ve açıkça uygulama `IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   Kovaryans oluşturulan C# kodundan kaldırın. Bu aşağıdaki dönüştürme ekleme içerir **Transforms\Metadata.xml** bir dönüş türüne sahip oluşturulan C# kod neden olacak `Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>Sorun: Ad çakışmalarını iç sınıfları / özellikleri

Çakışan görünürlük devralınan nesneler üzerinde.

Java'da, onu türetilmiş bir sınıf kendi üst aynı görünürlük olması zorunlu değildir. Java yalnızca, sizin için düzeltir. Emin olmanız gerekir C açık olmasını olan #'ta, hiyerarşideki tüm sınıflar uygun görünürlüğe sahiptir. Aşağıdaki örnek, bir Java paket adını değiştirmek gösterilmiştir `com.evernote.android.job` için `Evernote.AndroidJob`:

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>Sorun: A **.so** bağlama tarafından gerekli kitaplığıdır değil yükleniyor

Bazı bağlama projeleri de işlevindeki bağımlı bir **.so** kitaplığı. Xamarin.Android otomatik olarak yüklenmez mümkündür **.so** kitaplığı. Sarmalanan Java kod yürütüldüğünde, Xamarin.Android JNI çağrısı ve hata iletisi hale getirmek başarısız olur _java.lang.UnsatisfiedLinkError: yerel yöntem bulunamadı:_ logcat uygulama için görünür.

El ile yüklemek için bu düzeltmenin mi **.so** çağrısıyla Kitaplığı `Java.Lang.JavaSystem.LoadLibrary`. Örneğin bir Xamarin.Android projesi paylaşılan kitaplığı olduğunu varsayarak **libpocketsphinx_jni.so** bir yapı eylemi bağlama projeyle dahil **EmbeddedNativeLibrary**, aşağıdaki (paylaşılan kitaplık kullanmadan önce yürütülen) parçacığı yükler **.so** kitaplığı:

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>Özet

Bu makalede, Java bağlamalarla ilişkili genel sorun giderme sorunları listelenir ve bunların nasıl çözüleceği açıklanmaktadır.


## <a name="related-links"></a>İlgili bağlantılar

- [Kitaplık projeleri](http://developer.android.com/tools/projects/index.html#LibraryProjects)
- [JNI ile çalışma](~/android/platform/java-integration/working-with-jni.md)
- [Tanılama çıktıları etkinleştir](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Xamarin Android geliştiricileri için](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
