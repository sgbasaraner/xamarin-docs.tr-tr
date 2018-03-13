---
title: Xamarin.Android Environment
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: ee612d4a8982a6ae505b4d329b9abbc84624a1e0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="xamarinandroid-environment"></a>Xamarin.Android Environment

## <a name="execution-environment"></a>Yürütme Ortamı

*Yürütme ortamı* ortam değişkenleri ve program yürütme etkilemek Android Sistem özellikleri kümesidir. Android Sistem özellikleri ile ayarlanabilir `adb shell setprop` komutu ortam değişkenlerini ayarlayarak ayarlayabilirsiniz sırada `debug.mono.env` sistem özelliği:

```shell
## Enable GREF logging
adb shell setprop debug.mono.log gref

## Set the MONO_LOG_LEVEL and MONO_LOG_MASK environment variables
## so that additional Mono messages will be written to `adb logcat`.
adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
```

Android Sistem özellikleri tüm işlemler için hedef aygıtta ayarlanır.

Xamarin.Android 4.6 ile başlayarak, Sistem özellikleri ve ortam değişkenlerini ayarlama veya olabilir ekleyerek bir uygulama başına temelinde geçersiz kılınmış bir *ortam dosya* projeye. UNIX olarak biçimlendirilmiş bir düz metin dosyasıyla bir ortam dosyasıdır bir [ **yapı eylemi** , `AndroidEnvironment` ](~/android/deploy-test/building-apps/build-process.md).
Ortam dosyası biçiminde satırları içerir *anahtar = değer*.
Açıklamaları olan ile başlayan satırlar `#`. Boş satırlar yok sayılır.

Varsa *anahtar* büyük harfle, ardından başlatır *anahtar* bir ortam değişkeni kabul edilir ve **setenv**(3) belirtilen ortamdeğişkeniniayarlamakiçinkullanılır*değeri* işlem başlangıcı sırasında.

Varsa *anahtar* sonra bir küçük harfle başlayan *anahtar* bir Android sistem özelliği olarak kabul edilir ve *değeri* olan *varsayılan değer*: Xamarin.Android yürütme davranışını denetleyen android Sistem özellikleri ilk Android sistem özelliği sunucudan arandığı ve herhangi bir değer varsa, ardından ortamı dosyasında belirtilen değer kullanılır. Bu izin vermektir `adb shell setprop` tanılama ortam dosyasından gelen değerleri geçersiz kılmak için kullanılacak.

## <a name="xamarinandroid-environment-variables"></a>Xamarin.Android ortam değişkenleri

Xamarin.Android destekleyen `XA_HTTP_CLIENT_HANDLER_TYPE` aracılığıyla ya da ayarlanabilir değişkeni `adb shell setprop debug.mono.env` veya aracılığıyla `$(AndroidEnvironment)` yapı eylemi.


### `XA_HTTP_CLIENT_HANDLER_TYPE`

Öğesinden devralmalıdır bütünleştirilmiş kod tam türü [HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx) ve gelen oluşturulan [ `HttpClient()` varsayılan oluşturucu](https://msdn.microsoft.com/en-us/library/hh138077(v=vs.118).aspx).

Xamarin.Android 6.1 içinde varsayılan olarak, bu ortam değişkeni ayarlanmamış ve [HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx) kullanılır.

Alternatif olarak, değer `Xamarin.Android.Net.AndroidClientHandler` kullanmak için belirtilebilir [ `java.net.URLConnection` ](https://developer.xamarin.com/api/type/Java.Net.URLConnection/) ağ erişimi için hangi *olabilir* Android desteklediğinde, TLS 1.2 kullanımına izin verir.

Xamarin.Android 6.1 eklendi.

## <a name="xamarinandroid-system-properties"></a>Xamarin.Android System Properties

Xamarin.Android aracılığıyla ya da ayarlanabilir aşağıdaki sistem özelliklerini destekler `adb shell setprop` veya aracılığıyla `$(AndroidEnvironment)` yapı eylemi.

* `debug.mono.debug`
* `debug.mono.env`
* `debug.mono.gc`
* `debug.mono.log`
* `debug.mono.max_grefc`
* `debug.mono.profile`
* `debug.mono.runtime_args`
* `debug.mono.trace`
* `debug.mono.wref`
* `XA_HTTP_CLIENT_HANDLER_TYPE`

### `debug.mono.debug`

Değeri `debug.mono.debug` sistem özelliği bir tamsayı değil. Varsa `1`, işlem ile başlatılan "" ardından bilgisayarmış gibi `mono --debug`.
Bu genellikle dosya ve çizgi bilgilerini Yığın izlemeleri, vb. uygulama hata ayıklayıcı'dan başlatılması gerek kalmadan gösterir.

### `debug.mono.env`

İçeren bir `|`-ayrılmış ortam değişkenleri listesi.

### `debug.mono.gc`

Değeri `debug.mono.debug` sistem özelliği bir tamsayı değil.
Varsa `1`, sonra GC bilgileri oturum açmanız.

Bu zorunda eşdeğerdir `debug.mono.log` sistem özelliği içeren `gc`.

### `debug.mono.log`

Hangi ek bilgilerin Xamarin.Android oturum denetimleri `adb logcat`.
Virgülle ayrılmış bir dize olan (`,`), aşağıdaki değerlerden birini içeren:

* `all`: Çıkışı yazdırma *tüm* iletileri. İçerdiğinden bu ender iyi bir fikir aynıdır `lref` iletileri.
* `assembly`: Yazdırma çıkışı `.apk` ve iletileri ayrıştırma derleme.
* `gc`: GC ilgili iletilerini yazdırır.
* `gref`: JNI genel başvuru iletilerini yazdırır.
* `lref`: JNI yerel başvuru iletilerini yazdırır.  
    *Not*: Bu işlem *gerçekten* istenmeyen posta `adb logcat`.  
    Xamarin.Android 5.1, bu da oluşturacak bir `.__override__/lrefs.txt` alabilirsiniz dosya *aşırı büyük*.  
    Kaçının.
* `timing`: Bazı yöntemi zamanlama bilgileri yazdırın. Bu dosyaları da oluşturur `.__override__/methods.txt` ve `.__override__/counters.txt`.


### `debug.mono.max_grefc`

Değeri `debug.mono.max_grefc` sistem özelliği bir tamsayı değil.
Değer kullanıcının *geçersiz kılmaları* varsayılan hedef cihaz için en fazla GREF sayısı algılandı.

*Lütfen unutmayın:* bu yalnızca ile kullanılabilir `adb shell setprop
debug.mono.max_grefc` değeri zamanlı kullanılabilir olmayacak şekilde bir **environment.txt** dosya.

### `debug.mono.profile`

`debug.mono.profile` Sistem özelliği, profil oluşturucu sağlar.
Eşdeğerdir ve aynı değerlere, kullanır `mono --profile` seçeneği. (Bkz [ **mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) adam sayfasında daha fazla bilgi için.)

### `debug.mono.runtime_args`

`debug.mono.runtime_args` Sistem özelliği tarafından ayrıştırılması gerekip ek seçenekler içeren **mono**.

### `debug.mono.trace`

`debug.mono.trace` Sistem özelliği izlemeyi etkinleştirir.
Eşdeğerdir ve aynı değerlere, kullanır `mono --trace` seçeneği. (Bkz [ **mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) adam sayfasında daha fazla bilgi için.)

Genel olarak, *kullanmayın*. İzleme kullanımını istenmeyen `adb logcat` çıkışı, severaly program davranışı yavaş ve program davranış (en fazla ve ek hata koşulları ekleme gibi) değiştirin.

*Bazen*, ancak, bazı ek araştırma gerçekleştirilecek sağlar...

### `debug.mono.wref`

`debug.mono.wref` Sistem özelliği, varsayılan geçersiz kılma algılanan JNI zayıf başvuru mekanizması izin verir. İki desteklenen değerler şunlardır:

* `jni`: JNI zayıf başvurular tarafından oluşturulan olarak kullanmak `JNIEnv::NewWeakGlobalRef()` ve tarafından yok `JNIEnv::DeleteWeakGlobalREf()`.
* `java`: Kullanım JNI genel başvuran hangi başvuru `java.lang.WeakReference` örnekleri.

`java` Varsayılan olarak, en fazla API-7 kullanılır ve resim ile API-19 (Seti Kat) etkin. (API-8 eklenen `jni` başvuruları ve resim *ihlal* `jni` başvuruları.)

Bu sistem özelliği, test ve bazı formlar araştırma için yararlıdır.
*Genel*, onu değiştirilmemelidir.

### <a name="xahttpclienthandlertype"></a>XA\_HTTP\_İSTEMCİ\_İŞLEYİCİ\_TÜRÜ

İlk Xamarin.Android 6.1 sunulan, bu ortam değişkenini varsayılan bildirir `HttpMessageHandler` tarafından kullanılan uygulama `HttpClient`. Varsayılan olarak bu değişken ayarlanmaz ve Xamarin.Android kullanacağınız `HttpClientHandler`.

```shell
XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
```

> [!NOTE]
> Temel alınan Android cihazı, TLS 1.2 desteklemesi gerekir.
Android 5.0 ve sonrasındaki desteği TLS 1.2


## <a name="example"></a>Örnek

```shell
## Comments are lines which start with '#'
## Blank lines are ignored.


## Enable GREF messages to `adb logcat`
debug.mono.log=gref

## Clear out a Mono environment variable to decrease logging
MONO_LOG_LEVEL=
```



## <a name="related-links"></a>İlgili bağlantılar

- [Aktarım Katmanı Güvenliği](~/cross-platform/app-fundamentals/transport-layer-security.md)
