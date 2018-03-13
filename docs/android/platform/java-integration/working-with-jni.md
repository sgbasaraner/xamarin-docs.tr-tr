---
title: "JNI ile çalışma"
description: "Xamarin.Android içinde C# Java yerine Android uygulamaları yazma izin verir. Birkaç derlemeler ile Xamarin.Android Mono.Android.dll ve Mono.Android.GoogleMaps.dll dahil olmak üzere, Java kitaplıkları için bağlamaları sağlayan sağlanır. Ancak, bağlamaları için olası her Java kitaplığı sağlanmayan ve her Java tür ve üye sağlanan bağlamaları bağlayabilirsiniz değil. İlişkisiz Java türleri ve üyeleri kullanmak için Java yerel arabirimi (JNI) kullanılabilir. Bu makalede JNI Java türleri ve üyeleri Xamarin.Android uygulamalardan etkileşim için nasıl kullanılacağı gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: A417DEE9-7B7B-4E35-A79C-284739E3838E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: e9a6f44637b77bf53c3cab00ac5051e6a2f27386
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="working-with-jni"></a>JNI ile çalışma

_Xamarin.Android içinde C# Java yerine Android uygulamaları yazma izin verir. Birkaç derlemeler ile Xamarin.Android Mono.Android.dll ve Mono.Android.GoogleMaps.dll dahil olmak üzere, Java kitaplıkları için bağlamaları sağlayan sağlanır. Ancak, bağlamaları için olası her Java kitaplığı sağlanmayan ve her Java tür ve üye sağlanan bağlamaları bağlayabilirsiniz değil. İlişkisiz Java türleri ve üyeleri kullanmak için Java yerel arabirimi (JNI) kullanılabilir. Bu makalede JNI Java türleri ve üyeleri Xamarin.Android uygulamalardan etkileşim için nasıl kullanılacağı gösterilmektedir._


## <a name="overview"></a>Genel Bakış

Her zaman gerekli veya bir yönetilen aranabilir sarmalayıcısı (Java kod çağrılacak MCW) oluşturmak mümkün değildir. Çoğu durumda, "satır içi" JNI mükemmel kabul edilebilir ve bir kerelik kullanımını ilişkisiz Java üyeleri için kullanışlıdır. Genellikle, tüm .jar bağlama oluşturmak üzere daha Java sınıfı üzerinde tek bir yöntemi çağırmak için JNI kullanmak daha basittir.

Xamarin.Android sağlar `Mono.Android.dll` Android için kullanıcının bir bağlama sağlayan derleme `android.jar` kitaplığı. Türleri ve üyeleri yok içinde `Mono.Android.dll` ve türleri yok içinde `android.jar` el ile bunları bağlama tarafından kullanılıyor olabilir. Java türleri ve üyeleri bağlamak için kullandığınız **Java yerel arabirimi** (**JNI**) türleri arama, okuma ve alanları yazma ve yöntemleri çağırma.

Xamarin.Android JNI API'de kavramsal olarak çok benzer `System.Reflection` .NET API:, türlerini aramak ve adına göre üyeleri okuma ve alan değerlerini yazma yöntemleri ve daha fazlasını çağırmak için mümkün kılar. JNI kullanabilirsiniz ve `Android.Runtime.RegisterAttribute` geçersiz kılma desteklemek için bağlı sanal yöntemler bildirmek için özel öznitelik. Böylece, C# ' ta uygulanabilir arabirimleri bağlayabilirsiniz.

Bu belgede açıklanmaktadır:

-  Nasıl JNI türlerine ifade eder.
-  Arama, okuma ve yazma alanları nasıl.
-  Arama ve yöntemleri çağırmak nasıl.
-  Yönetilen koddan geçersiz kılma izin vermek için sanal yöntemler kullanıma sunmak nasıl.
-  Nasıl arabirimleri kullanıma sunar.



## <a name="requirements"></a>Gereksinimler

Aracılığıyla gösterilen JNI [Android.Runtime.JNIEnv ad alanı](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), her Xamarin.Android sürümünde kullanılabilir.
Java türleri ve arabirimleri bağlamak için Xamarin.Android 4.0 veya sonraki sürümü kullanmanız gerekir.


## <a name="managed-callable-wrappers"></a>Yönetilen aranabilir sarmalayıcılar

A **yönetilen aranabilir sarmalayıcısı** (**MCW**) olan bir *bağlama* bir Java sınıfı veya tüm JNI makineler sarmalar ve böylece istemci C# kodu hakkında endişelenmeniz gerekmez arabirimi JNI temel karmaşıklığını. Çoğu `Mono.Android.dll` yönetilen aranabilir sarmalayıcılar oluşur.

Yönetilen aranabilir sarmalayıcılar iki amaca hizmet eder:

1.  Böylece istemci kodu temel karmaşıklık hakkında bilmeniz gereken değil JNI kullanım kapsüller.
1.  Java türleri alt sınıf ve Java arabirimlerini mümkün kılar.

İlk amaç yalnızca kolaylık sağlamak ve karmaşıklık kapsülleme, böylelikle sınıflarını kullanmak için basit, yönetilen bir dizi tüketiciye sahip. Bu çeşitli kullanılmasını gerektiren [JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) bu makalenin sonraki bölümlerinde açıklandığı gibi üyeleri. Aranabilir sarmalayıcılar yönetilen unutmayın kesinlikle gerekli olmayan &ndash; "satır içi JNI kullanma" edilebilir ve ilişkisiz Java üyelerinin tek seferlik kullanım için kullanışlıdır. Alt classing ve arabirim uygulaması yönetilen aranabilir sarmalayıcılar kullanılmasını gerektirir.



## <a name="android-callable-wrappers"></a>Android aranabilir sarmalayıcılar

Android aranabilir sarmalayıcılar (ACW), Android çalışma zamanı (resim) yönetilen kod çağırma gerektiğinde gereklidir; Çalışma zamanında resimleri sınıfları kaydetmek için hiçbir şekilde olduğundan bu sarmalayıcıları gereklidir.
(Özellikle [DefineClass](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html#wp15986) JNI işlevi Android çalışma zamanı tarafından desteklenmiyor. Android aranabilir sarmalayıcılar böylece çalışma zamanı türü kayıt desteğinin olmaması için oluşturan.)

Android kod sanal bir execute veya geçersiz veya yönetilen kodda uygulanan yöntemi arabirim gerektiğinde, böylece bu yöntem için uygun yönetilen türü gönderilen Xamarin.Android Java proxy sağlamanız gerekir. Bu Java proxy "aynı" temel sınıf ve Java arabirim listesi aynı oluşturucular uygulama ve tüm geçersiz kılınmış temel sınıf ve arabirim yöntemleri bildirme yönetilen türü sahip Java kod türleridir.

Android aranabilir sarmalayıcılar tarafından üretilen **monodroid.exe** sırasında program [derleme işlemi](~/android/deploy-test/building-apps/build-process.md)ve (doğrudan veya dolaylı olarak) devralan tüm türleri için oluşturulan [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).



### <a name="implementing-interfaces"></a>Arabirimler uygulama

Ne zaman ihtiyacınız olabilecek Android bir arabirim için zamanlar (gibi [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/)).

Tüm Android sınıflar ve arabirimler genişletmek [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) arabirim; bu nedenle, tüm Android türleri uygulamalıdır `IJavaObject`.
Xamarin.Android olgunun avantajlarından yararlanır &ndash; kullandığı `IJavaObject` Android için belirtilen yönetilen türü bir Java proxy (bir Android aranabilir sarmalayıcısı) ile sağlamak için. Çünkü **monodroid.exe** yalnızca arar `Java.Lang.Object` alt sınıflar (hangi uygulanmalı `IJavaObject`), sınıflara `Java.Lang.Object` bize yönetilen kodda arabirimleri uygulamak için bir yol sağlar. Örneğin:

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {
    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig) {
        // implementation goes here...
    }
    public void OnLowMemory () {
        // implementation goes here...
    }
}
```


### <a name="implementation-details"></a>Uygulama Ayrıntıları

*Bu makalenin sonraki bölümlerinde bildirilmeksizin uygulama ayrıntılarını sağlar* (ve yalnızca geliştiriciler başlık altında neler hakkında merak olabilir çünkü Burada sunulan).

Örneğin, aşağıdaki C# kaynak verilen:

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

**Mandroid.exe** program aşağıdaki Android aranabilir sarmalayıcısı üretir:

```java
package mono.samples.helloWorld;

public class HelloAndroid extends android.app.Activity {
    static final String __md_methods;
    static {
        __md_methods =
            "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" +
            "";
        mono.android.Runtime.register (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                HelloAndroid.class,
                __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null",
                "", this, new java.lang.Object[] { });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Taban sınıfı korunur ve yönetilen kod içinde geçersiz kılınır her bir yöntemin yerel yöntem bildirimleri sağlanan dikkat edin.



### <a name="exportattribute-and-exportfieldattribute"></a>ExportAttribute ve ExportFieldAttribute

Genellikle, Xamarin.Android otomatik olarak ACW oluşur Java kod oluşturur; bir sınıf bir Java sınıfından türetilen ve varolan Java yöntemlerini geçersiz kılar, bu oluşturma sınıfı ve yöntemi adları temel alır. Ancak, bazı senaryolarda, kod oluşturma aşağıda özetlendiği gibi yeterli, değil:

-   Android desteği eylem adları Düzen xml öznitelikleri, örneğin [android: onClick](https://developer.xamarin.com/api/member/Android.Views.View+IOnClickListener.OnClick/p/Android.Views.View/) XML özniteliği. Belirtildiğinde, inflated görünümü örneği deneyin Java yöntemi arayın.

-   [Java.io.Serializable](http://developer.android.com/reference/java/io/Serializable.html) arabirimi gerektirir `readObject` ve `writeObject` yöntemleri. Bu arabirim üyeleri değiller olduğundan, karşılık gelen bizim yönetilen uygulama Java kod bu yöntemlere kullanıma sunmuyor.

-   [Android.os.Parcelable](https://developer.xamarin.com/api/type/Android.Os.Parcelable/) arabirimi bekliyor uygulama sınıfı statik bir alana sahip olmalıdır `CREATOR` türü `Parcelable.Creator`. Oluşturulan Java kod bazı açık alan gerektirir. Standart Senaryomuzda ile yönetilen koddan Java kod çıktı alanına yolu yoktur.


Kod oluşturma rastgele adlar rasgele Java yöntemleriyle oluşturmak için bir çözüm sağlamadığından Xamarin.Android 4.2 ile başlayan [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/) ve [ExportFieldAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportFieldAttribute/) olan Yukarıdaki senaryolarda bir çözüm sunmak için kullanıma sunuldu. Her iki öznitelik bulunan `Java.Interop` ad alanı:

-   `ExportAttribute` &ndash; bir yöntem adı ve onun beklenen özel durum türleri (açık "oluşturduğunda Java'daki" vermek) belirtir. Bir yöntem kullanıldığında, yöntem karşılık gelen JNI çağırma Yönetilen yöntemi için bir gönderme kodu oluşturan bir Java yöntemi "verecektir". Bu ile kullanılabilir `android:onClick` ve `java.io.Serializable`.

-   `ExportFieldAttribute` &ndash; bir alan adı belirtir. Alan başlatıcı olarak çalışan bir yöntem üzerinde yer alıyor. Bu ile kullanılabilir `android.os.Parcelable`.

[ExportAttribute](https://developer.xamarin.com/samples/monodroid/ExportAttribute/) örnek proje bu öznitelikleri nasıl kullanıldığını gösterir.


#### <a name="troubleshooting-exportattribute-and-exportfieldattribute"></a>ExportAttribute ve ExportFieldAttribute sorun giderme

-   Eksik nedeniyle paketleme başarısız **Mono.Android.Export.dll** &ndash; kullandıysanız `ExportAttribute` veya `ExportFieldAttribute` eklemek zorunda kod veya bağımlı kitaplıkları bazı yöntemler üzerinde  **Mono.Android.Export.dll**. Bu derleme Java geri çağırma koddan desteklemek için ayrı tutulur. Ayrı **Mono.Android.dll** gibi ek boyutu uygulamayı ekler.

-   Yayın derlemesi içinde `MissingMethodException` için dışa aktarma yöntemleri oluşur &ndash; içinde yayın derlemesi `MissingMethodException` verme yöntemleri için oluşur. (Bu sorunu en son sürümünü Xamarin.Android sabittir.)



### <a name="exportparameterattribute"></a>ExportParameterAttribute

`ExportAttribute` ve `ExportFieldAttribute` bu Java Çalışma zamanı koduyla kullanabileceğiniz işlevsellik sağlar. Bu çalışma zamanı koduyla yönetilen kod özniteliklerle tarafından yönetilen oluşturulan JNI yöntemler yoluyla erişir. Sonuç olarak, yönetilen yöntemi bağlayan varolan Java yöntemi yoktur; Bu nedenle, Java yöntemi yönetilen yöntemi imzadan oluşturulur.

Ancak, bu durumda tam olarak determinant değil. Özellikle, bu yönetilen türler ve Java türleri gibi arasındaki bazı gelişmiş eşlemeleri geçerlidir:

-  InputStream
-  OutputStream
-  XmlPullParser
-  XmlResourceParser

Bunlar gibi türleri için verilen yöntemleri, gerektiğinde `ExportParameterAttribute` açıkça karşılık gelen bir parametre verin veya dönüş değeri bir tür için kullanılması gerekir.



### <a name="annotation-attribute"></a>Ek Açıklama özniteliği

Xamarin.Android 4.2 biz dönüştürülen `IAnnotation` uygulama türlerine öznitelikleri (System.Attribute'un) ve Java sarmalayıcıları ek açıklama oluşturma desteği eklendi.

Bu, aşağıdaki yön değişiklikleri anlamına gelir:

-   Bağlama üreteci oluşturur `Java.Lang.DeprecatedAttribute` gelen `java.Lang.Deprecated` (olmalıdır sırada `[Obsolete]` yönetilen kod).

-   Bu varolan gelmez `Java.Lang.Deprecated` sınıfı kaybolur. (Bu tür kullanımı varsa) Bu Java tabanlı nesneler hala normal Java nesneler olarak kullanılabilir. Olacaktır `Deprecated` ve `DeprecatedAttribute` sınıfları.

-   `Java.Lang.DeprecatedAttribute` Sınıfı olarak işaretli `[Annotation]` . Öğesinden devralınan özel bir öznitelik olduğunda `[Annotation]` özniteliği msbuild görevi, bir Java ek açıklama, özel öznitelik üretir (@Deprecated) Android aranabilir sarmalayıcısı (ACW) içinde.

-   Ek açıklamalar sınıfları, yöntemleri oluşturulabilir ve (bir yöntemi olan yönetilen kodda) alanları dışarı.

Kapsayan sınıfı (açıklamalı sınıfı veya açıklamalı üyeleri içeren sınıf) kayıtlı değilse, tüm Java sınıfı kaynak hiç açıklamalar dahil olmak üzere oluşturulmaz. Yöntemleri için belirttiğiniz `ExportAttribute` açıkça oluşturulur ve açıklama yöntemi alınamıyor. Ayrıca, "bir Java ek açıklama sınıf tanımı oluşturmak için" bir özellik değil. Diğer bir deyişle, belirli bir eklenti için özel bir yönetilen özniteliği tanımlarsanız, karşılık gelen Java ek açıklama sınıfı içeren başka bir .jar kitaplığını eklemeniz gerekir. Ek açıklama türünü tanımlayan bir Java kaynak dosyası ekleme yeterli değil. Java derleyicisi ile aynı şekilde çalışmaz **apt**.

Ayrıca aşağıdaki sınırlamalar uygulanır:

-   Bu dönüştürme işlemi dikkate almaz `@Target` kadarki ek açıklama ek açıklama türü.

-   Bir özelliğin üzerine öznitelikleri çalışmaz. Öznitelikleri özellik alıcısı veya ayarlayıcı için bunun yerine kullanın.



## <a name="class-binding"></a>Sınıf bağlama

Bir sınıf bağlama, temel alınan Java türü çağırma basitleştirmek için yönetilen bir aranabilir sarmalayıcısı yazma anlamına gelir.

C# ' dan geçersiz kılma izin vermek için sanal ve soyut yöntemler bağlama Xamarin.Android 4.0 gerektirir. Ancak, herhangi bir sürümünü Xamarin.Android saat dilimi ve tarih geçersiz kılmaları destekleme olmadan sanal olmayan yöntemler, statik yöntemler ya da sanal yöntemler bağlayabilirsiniz.

Bir bağlama genellikle aşağıdaki öğeleri içerir:

-  A [JNI işleme bağlanan Java türü](#_Looking_up_Java_Types).

-  [JNI alan kimlikleri ve ilişkili her alan özelliklerini](#_Instance_Fields).

-  [JNI yöntemi kimlikleri ve her yöntemleri bağlı yöntemi](#_Instance_Methods).

-  Alt classing gerekliyse, türü olması gerekiyorsa bir [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) özel öznitelik türü bildirimiyle üzerinde [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) kümesine `true`.



### <a name="declaring-type-handle"></a>Tür tanıtıcı bildirme

Alan ve yöntem arama yöntemleri kendi bildiri türü başvuran bir nesne başvurusu gerektirir. Kurala göre bu tutulur bir `class_ref` alan:

```csharp
static IntPtr class_ref = JNIEnv.FindClass(CLASS);
```

Bkz: [JNI tür başvuruları](#_JNI_Type_References) hakkında ayrıntılar için bölüm `CLASS` belirteci.


### <a name="binding-fields"></a>Alanların bağlama

Java alanları C# özellikleri, örneğin Java alanı olarak açığa [java.lang.System.in](http://developer.android.com/reference/java/lang/System.html#in) C# özelliği olarak bağlı [Java.Lang.JavaSystem.In](https://developer.xamarin.com/api/property/Java.Lang.JavaSystem.In/).
Ayrıca, statik ve örnek alanları arasında JNI ayırt olduğundan, farklı yöntemler özellikleri uygularken kullanılabilir.

Alan bağlama üç yöntem kümesi içerir:

1.  *Alan kimliğini Al* yöntemi. *Alan kimliğini Al* yöntemi bir alan işlemek döndürmek için sorumlu *alan değeri Al* ve *alan değeri ayarlayın* yöntemleri kullanır. Alan kimliği alma gerektirir, alanın adını yazın bildirme bilerek ve [JNI tür imzası](#_JNI_Type_Signatures) alanının.

1.  *Alan değeri Al* yöntemleri. Bu yöntemleri alan tanıtıcı gerektirir ve Java alanın değerini okumak için sorumludur.
    Yöntemini kullanmak üzere alanın türüne bağlıdır.

1.  *Alan değerini* yöntemleri. Bu yöntemleri alan tanıtıcı gerektirir ve Java içinde alanın değerini yazmak için sorumludur. Yöntemini kullanmak üzere alanın türüne bağlıdır.


 [Statik alanları](#_Static_Fields) kullanmak [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), `JNIEnv.GetStatic*Field`, ve [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/) yöntemleri.

 [Örnek alanları](#_Instance_Fields) kullanmak [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), `JNIEnv.Get*Field`, ve [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/) yöntemleri.

Örneğin, statik özelliği `JavaSystem.In` olarak uygulanabilir:

```csharp
static IntPtr in_jfieldID;
public static System.IO.Stream In
{
    get {
        if (in_jfieldId == IntPtr.Zero)
            in_jfieldId = JNIEnv.GetStaticFieldID (class_ref, "in", "Ljava/io/InputStream;");
        IntPtr __ret = JNIEnv.GetStaticObjectField (class_ref, in_jfieldId);
        return InputStreamInvoker.FromJniHandle (__ret, JniHandleOwnership.TransferLocalRef);
    }
}
```

Not: Biz kullanmakta olduğunuz [InputStreamInvoker.FromJniHandle](https://developer.xamarin.com/api/member/Android.Runtime.InputStreamInvoker.FromJniHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) içine JNI başvuru dönüştürmek için bir `System.IO.Stream` örneği ve biz kullanmakta olduğunuz `JniHandleOwnership.TransferLocalRef` çünkü [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) yerel bir başvuru döndürür.

Çoğu [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) türlerine sahip `FromJniHandle` bir JNI dönüştürecek yöntemleri başvuru istenen türüne dönüştürülemedi.



### <a name="method-binding"></a>Yöntem bağlama

Java yöntemleri, C# yöntemleri ve C# özellikleri olarak sunulur. Örneğin, Java yöntemi [java.lang.Runtime.runFinalizersOnExit](http://developer.android.com/reference/java/lang/Runtime.html#runFinalizersOnExit(boolean)) yöntemine bağlı olarak [Java.Lang.Runtime.RunFinalizersOnExit](https://developer.xamarin.com/api/member/Java.Lang.Runtime.RunFinalizersOnExit/) yöntemi ve [java.lang.Object.getClass ](http://developer.android.com/reference/java/lang/Object.html#getClass) yöntemine bağlı olarak [Java.Lang.Object.Class](https://developer.xamarin.com/api/property/Java.Lang.Object.Class/) özelliği.

Yöntem çağırma iki adımlı bir işlemdir:

1.  *Yöntemi kimliği alma* için çağrılacak yöntem. *Yöntemi kimliği alma* yöntemdir yöntemi çağırma yöntemlerini kullanan yöntemi tanıtıcı döndürmek için sorumlu. Yöntem kimlik alma gerektirir, yöntemin adını yazın bildirme bilerek ve [JNI tür imzası](#_JNI_Type_Signatures) yöntemi.

1.  Invoke yöntemi.

Yalnızca alanlarla gibi yöntem Kimliği almak ve yöntemini çağırmak için kullanabileceğiniz yöntemler statik ve örnek yöntemleri arasında farklılık gösterir.

[Statik yöntemler](#_Static_Methods_1) kullanmak [JNIEnv.GetStaticMethodID()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/) yöntemi kimliği arama ve kullanmak için `JNIEnv.CallStatic*Method` Çağırma yöntemlerini ailesi.

[Örnek yöntemleri](#_Instance_Methods) kullanmak [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) yöntemi kimliği arama ve kullanmak için `JNIEnv.Call*Method` ve `JNIEnv.CallNonvirtual*Method` Çağırma yöntemlerini aileleri.

Yöntem bağlama potansiyel olarak daha fazlasını yöntemi çağrıdır. Yöntem bağlama ayrıca (soyut ve son olmayan yöntemleri için) geçersiz kılınacak yöntemine izin verilmesi veya içerir (arabirim yöntemleri için) uygulanır. [Destekleyen devralma arabirimleri](#_Supporting_Inheritance,_Interfaces_1) bölüm, sanal ve arabirim yöntemleri destekleme karmaşıklıkları kapsar.

<a name="_Static_Methods_1" />

#### <a name="static-methods"></a>Statik yöntemler

Bir statik yöntem bağlama içerir kullanarak `JNIEnv.GetStaticMethodID` uygun kullanarak bir yöntem tanıtıcı elde için `JNIEnv.CallStatic*Method` yöntemin dönüş türüne bağlı olarak yöntemi. Aşağıdakiler için bir bağlama örneğidir [Runtime.getRuntime](http://developer.android.com/reference/java/lang/Runtime.html#getRuntime()) yöntemi:

```csharp
static IntPtr id_getRuntime;

[Register ("getRuntime", "()Ljava/lang/Runtime;", "")]
public static Java.Lang.Runtime GetRuntime ()
{
    if (id_getRuntime == IntPtr.Zero)
        id_getRuntime = JNIEnv.GetStaticMethodID (class_ref,
                "getRuntime", "()Ljava/lang/Runtime;");

    return Java.Lang.Object.GetObject<Java.Lang.Runtime> (
            JNIEnv.CallStaticObjectMethod  (class_ref, id_getRuntime),
            JniHandleOwnership.TransferLocalRef);
}
```

Biz yönteminin tutamaç statik bir alana depolamak Not `id_getRuntime`. Bu performans iyileştirme, böylelikle yönteminin tutamaç her çağrısının Aranan gerekmez. Bu şekilde yöntemi tanıtıcı önbelleğe almak gerekli değildir. Yönteminin tutamaç alındıktan sonra [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) yöntemini çağırmak için kullanılır. `JNIEnv.CallStaticObjectMethod` döndüren bir `IntPtr` döndürülen Java örneği tanıtıcısı içerir.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Java tanıtıcı türü kesin belirlenmiş bir nesne örneğine dönüştürmek için kullanılır.



#### <a name="non-virtual-instance-method-binding"></a>Sanal olmayan örnek yöntemi bağlama

Bağlama bir `final` örnek yöntemi veya geçersiz kılma, gerektirmeyen bir örnek yöntemi içerir kullanarak `JNIEnv.GetMethodID` uygun kullanarak bir yöntem tanıtıcı elde için `JNIEnv.Call*Method` yöntemin dönüş türüne bağlı olarak yöntemi. Aşağıdakiler için bir bağlama örneğidir `Object.Class` özelliği:

```csharp
static IntPtr id_getClass;
public Java.Lang.Class Class {
    get {
        if (id_getClass == IntPtr.Zero)
            id_getClass = JNIEnv.GetMethodID (class_ref, "getClass", "()Ljava/lang/Class;");
        return Java.Lang.Object.GetObject<Java.Lang.Class> (
                JNIEnv.CallObjectMethod (Handle, id_getClass),
                JniHandleOwnership.TransferLocalRef);
    }
}
```

Biz yönteminin tutamaç statik bir alana depolamak Not `id_getClass`.
Bu performans iyileştirme, böylelikle yönteminin tutamaç her çağrısının Aranan gerekmez. Bu şekilde yöntemi tanıtıcı önbelleğe almak gerekli değildir. Yönteminin tutamaç alındıktan sonra [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) yöntemini çağırmak için kullanılır. `JNIEnv.CallStaticObjectMethod` döndüren bir `IntPtr` döndürülen Java örneği tanıtıcısı içerir.
[Java.Lang.Object.GetObject&lt;T&gt;(IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Java tanıtıcı türü kesin belirlenmiş bir nesne örneğine dönüştürmek için kullanılır.


### <a name="binding-constructors"></a>Bağlama oluşturucular

Oluşturucular yöntemlerdir Java adıyla `"<init>"`. Yalnızca yöntemleriyle Java örneği olarak `JNIEnv.GetMethodID` Oluşturucusu tanıtıcı aramak için kullanılır. Java yöntemlerinden farklı olarak [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/) yöntemleri Oluşturucusu yöntemi tanıtıcı çağırmak için kullanılır. Dönüş değerini `JNIEnv.NewObject` JNI yerel başvurusu:


```csharp
int value = 42;
IntPtr class_ref    = JNIEnv.FindClass ("java/lang/Integer");
IntPtr id_ctor_I    = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
IntPtr lrefInstance = JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value));
// Dispose of lrefInstance, class_ref…
```

Normalde bir sınıf bağlama olacak bir alt [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/).
Sınıflara ayırırken `Java.Lang.Object`, bir ek anlam oyuna gelir: bir `Java.Lang.Object` örneği bulundurur Java örneğine genel başvuru `Java.Lang.Object.Handle` özelliği.

1.  `Java.Lang.Object` Varsayılan oluşturucu tahsis etmek bir Java örneği.

1.  Türündeyse bir `RegisterAttribute` , ve `RegisterAttribute.DoNotGenerateAcw` olan `true` , ardından örneği `RegisterAttribute.Name` türü, kendi varsayılan oluşturucu kullanılarak oluşturulur.

1.  Aksi takdirde, [Android aranabilir sarmalayıcısı](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) karşılık gelen `this.GetType` kendi varsayılan oluşturucu örneği. Android aranabilir sarmalayıcılar için paket oluşturma sırasında oluşturulan her `Java.Lang.Object` kendisi için bir alt `RegisterAttribute.DoNotGenerateAcw` ayarlanmazsa `true`.

Olan türleri bağlamaları sınıfı değil, bu beklenen aranır anlam: örnekleme bir `Mono.Samples.HelloWorld.HelloAndroid` C# örnek bir Java oluşturmak `mono.samples.helloworld.HelloAndroid` oluşturulan Android aranabilir sarmalayıcısı olan örneği.

Sınıf bağlamaları için varsayılan bir oluşturucu Java türünü içeren ve/veya diğer bir oluşturucuyu çağrılması gerekir, bu doğru davranış olabilir. Aksi halde, bir oluşturucu aşağıdaki eylemleri gerçekleştirir sağlanmalıdır:

1.  Çağırma [Java.Lang.Object (IntPtr, JniHandleOwnership)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) varsayılan yerine `Java.Lang.Object` Oluşturucusu. Bu, yeni bir Java örneği oluşturmamak için gereklidir.

1.  Değerini denetleyin [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) hiç Java örneği oluşturmadan önce. `Object.Handle` Özelliği olacaktır bir değer dışındaki `IntPtr.Zero` Android aranabilir sarmalayıcısı Java kodda oluşturulan ve oluşturulan Android aranabilir sarmalayıcısı örneği içerecek şekilde sınıfı bağlama oluşturulmuyor. Örneğin, Android oluşturduğunda bir `mono.samples.helloworld.HelloAndroid` örneği, Android aranabilir sarmalayıcısı ilk ve Java oluşturulacak `HelloAndroid` Oluşturucusu karşılık gelen bir örneğini oluşturur `Mono.Samples.HelloWorld.HelloAndroid` türü ile `Object.Handle` özelliği Java örneğine Oluşturucusu yürütme önce ayarlanan.

1.  Geçerli çalışma zamanı türü bildirme aynı değilse yazın, sonra karşılık gelen Android aranabilir sarmalayıcısı örneği oluşturulmalı ve kullanmak [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) tarafından döndürülen tanıtıcı depolamak için [ JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/).

1.  Geçerli çalışma zamanı türü bildiri türü ile aynı olduğunda, daha sonra Java Oluşturucusu çağırma ve kullanabilirsiniz [Object.SetHandle](https://developer.xamarin.com/api/member/Java.Lang.Object.SetHandle/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership)) tarafından döndürülen tanıtıcı depolamak için `JNIEnv.NewInstance` .


Örneğin, göz önünde bulundurun [java.lang.Integer(int)](http://developer.android.com/reference/java/lang/Integer.html#Integer(int)) Oluşturucusu. Bu, olarak bağlıdır:

```csharp
// Cache the constructor's method handle for later use
static IntPtr id_ctor_I;

// Need [Register] for subclassing
// RegisterAttribute.Name is always ".ctor"
// RegisterAttribute.Signature is tye JNI type signature of constructor
// RegisterAttribute.Connector is ignored; use ""
[Register (".ctor", "(I)V", "")]
public Integer (int value)
    // 1. Prevent Object default constructor execution
    : base (IntPtr.Zero, JniHandleOwnership.DoNotTransfer)
{
    // 2. Don't allocate Java instance if already allocated
    if (Handle != IntPtr.Zero)
        return;

    // 3. Derived type? Create Android Callable Wrapper
    if (GetType () != typeof (Integer)) {
        SetHandle (
                Android.Runtime.JNIEnv.CreateInstance (GetType (), "(I)V", new JValue (value)),
                JniHandleOwnership.TransferLocalRef);
        return;
    }

    // 4. Declaring type: lookup &amp; cache method id...
    if (id_ctor_I == IntPtr.Zero)
        id_ctor_I = JNIEnv.GetMethodID (class_ref, "<init>", "(I)V");
    // ...then create the Java instance and store
    SetHandle (
            JNIEnv.NewObject (class_ref, id_ctor_I, new JValue (value)),
            JniHandleOwnership.TransferLocalRef);
}
```

[JNIEnv.CreateInstance](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CreateInstance/) yöntemleridir Yardımcıları gerçekleştirmek için bir `JNIEnv.FindClass`, `JNIEnv.GetMethodID`, `JNIEnv.NewObject`, ve `JNIEnv.DeleteGlobalReference` döndürülen değeri `JNIEnv.FindClass`. Ayrıntılar için sonraki bölüme bakın.

<a name="_Supporting_Inheritance,_Interfaces_1" />

### <a name="supporting-inheritance-interfaces"></a>Devralma destekleyen, arabirimleri

Java türü sınıflara veya Java arabirimini uygulayan oluşturulmasını gerektirir [Android aranabilir sarmalayıcılar](~/android/platform/java-integration/android-callable-wrappers.md) (ACWs) için oluşturulan her `Java.Lang.Object` paketleme işlemi sırasında bir alt kümesi. ACW nesil aracılığıyla denetlenir [Android.Runtime.RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) özel öznitelik.

C# türleri için `[Register]` özel öznitelik Oluşturucusu bir bağımsız değişken gerektirir: [JNI Basitleştirilmiş türü başvurusu](#_Simplified_Type_References_1) karşılık gelen Java için yazın. Bu, Java ve C# arasında farklı adlar sağlayarak sağlar.

Xamarin.Android 4.0 önce `[Register]` özel öznitelik "diğer" mevcut Java türleri kullanılamaz. ACW oluşturma işlemi için ACWs oluşturur Bunun nedeni her `Java.Lang.Object` alt kümesi ile karşılaşıldı.

Xamarin.Android sunulan 4.0 [RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) özelliği. Bu özellik ACW oluşturma işlemine bildirir *atla* yeni yönetilen aranabilir, paket oluşturma sırasında oluşturulan ACWs oluşturmayacaktır sarmalayıcılar bildirimi izin vererek açıklamalı türü. Bu bağlama varolan Java türleri sağlar. Örneğin, aşağıdaki basit Java sınıfı göz önünde bulundurun `Adder`, bir yöntem içeren `add`, tamsayılara ekler ve sonucu döndürür:

```java
package mono.android.test;
public class Adder {
    public int add (int a, int b) {
        return a + b;
    }
}
```

`Adder` Türü bağlı olarak:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public partial class Adder : Java.Lang.Object {
    static IntPtr class_ref = JNIEnv.FindClass ( "mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }
}
partial class ManagedAdder : Adder {
}
```

Burada, `Adder` C# türü *diğer adlar* `Adder` Java türü. `[Register]` JNI adını belirtmek için kullanılan öznitelik `mono.android.test.Adder` Java türü ve `DoNotGenerateAcw` özelliği ACW nesil engelle için kullanılır. Bu bir ACW için nesil sonuçlanır `ManagedAdder` türü, hangi düzgün alt sınıfların `mono.android.test.Adder` türü. Varsa `RegisterAttribute.DoNotGenerateAcw` özelliği çalıştırmadıysanız kullanıldığını sonra Xamarin.Android derleme işlem yeni oluşturulan `mono.android.test.Adder` Java türü. Bu derleme hataları olarak oluşturacağı `mono.android.test.Adder` türü, iki kez iki ayrı dosyalarda mevcut olacaktır.



### <a name="binding-virtual-methods"></a>Sanal yöntemler bağlama

`ManagedAdder` alt sınıfların Java `Adder` türü, ancak özellikle ilginç değil: C# `Adder` türü olmayan tanımlama herhangi bir sanal yöntem nedenle `ManagedAdder` herhangi bir şey geçersiz kılamaz.

Bağlama `virtual` yöntemleri alt sınıflar tarafından geçersiz kılma izin vermek için aşağıdaki iki kategoriye ayrılan yapılması gereken birkaç şey gerektirir:

1.  **Yöntem bağlama**

1.  **Yöntem kayıt**


#### <a name="method-binding"></a>Yöntem bağlama

Bir yöntem bağlama için C# iki destek üyelerinin eklenmesini gerektirir `Adder` tanımı: `ThresholdType`, ve `ThresholdClass`.

##### <a name="thresholdtype"></a>ThresholdType

`ThresholdType` Özelliği geçerli bağlama türünü döndürür:

```csharp
partial class Adder {
    protected override System.Type ThresholdType {
        get {
            return typeof (Adder);
        }
    }
}
```

`ThresholdType` Bağlama yöntemini sanal olmayan bir yöntem gönderme karşılaştırması sanal gerçekleştirmesi gereken zaman belirlemek için kullanılır. Her zaman döndürmelidir bir `System.Type` bildiren C# türü için karşılık gelen örnek.

##### <a name="thresholdclass"></a>ThresholdClass

`ThresholdClass` Özelliği, bağlama türü JNI sınıfı başvurusunu döndürür:

```csharp
partial class Adder {
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```

`ThresholdClass` Bağlama yöntemini sanal olmayan yöntemleri çağrılırken kullanılır.

#### <a name="binding-implementation"></a>Uygulama bağlama

Yöntem bağlama uygulaması Java yöntemi çalışma zamanı çağırma için sorumludur. Ayrıca içeren bir `[Register]` yöntemi kayıt bir parçasıdır ve yöntemi kayıt bölümünde açıklanan özel bir öznitelik bildirimi:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }
}
```

`id_add` Alan çağırmak için Java yöntemi için yöntem Kimliğini içerir. `id_add` Öğesinden değeri elde `JNIEnv.GetMethodID`, bildiri sınıfı gerektirir (`class_ref`), Java yöntem adı (`"add"`) ve yönteminin JNI imza (`"(II)I"`).

Yöntem kimliği alındıktan sonra `GetType` karşılaştırılır `ThresholdType` sanal veya sanal olmayan gönderme gerekli olup olmadığını belirlemek için. Sanal gönderme çalıştırmadığında `GetType` eşleşen `ThresholdType`, olarak `Handle` yöntemi geçersiz kılan bir Java ayrılmış bir alt kümesi için başvurabilir.

Zaman `GetType` eşleşmeyen `ThresholdType`, `Adder` sınıflandırma (örneğin tarafından `ManagedAdder`) ve `Adder.Add` uygulaması yalnızca çağrılacak bir alt başlatılırsa `base.Add`. Where olan sanal olmayan gönderme böyledir `ThresholdClass` devreye girer. `ThresholdClass` Çağrılacak yöntemin kullanımı hangi Java sınıfı sağlayacak belirtir.



#### <a name="method-registration"></a>Yöntem kayıt

Güncelleştirilmiş sahibiz varsayın `ManagedAdder` geçersiz kılan tanımı `Adder.Add` yöntemi:

```csharp
partial class ManagedAdder : Adder {
    public override int Add (int a, int b) {
        return (a*2) + (b*2);
    }
}
```

Sözcüğünün `Adder.Add` sahip bir `[Register]` özel öznitelik:

```csharp
[Register ("add", "(II)I", "GetAddHandler")]
```

`[Register]` Özel öznitelik Oluşturucusu üç değerleri kabul eder:

1.  Java yöntemin adını `"add"` bu durumda.

1.  JNI tür imzası yöntem `"(II)I"` bu durumda.

1.  *Bağlayıcı yöntemi* , `GetAddHandler` bu durumda.
    Bağlayıcı yöntemleri daha sonra açıklanacaktır.


İlk iki parametre yöntemi geçersiz kılmak için bir yöntem bildirimi oluşturmak ACW oluşturma işlemine olanak sağlar. Sonuçta elde edilen ACW aşağıdaki kodu bazıları içermesi gerekir:

```csharp
public class ManagedAdder extends mono.android.test.Adder {
    static final String __md_methods;
    static {
        __md_methods = "n_add:(II)I:GetAddHandler\n" +
            "";
        mono.android.Runtime.register (...);
    }
    @Override
    public int add (int p0, int p1) {
        return n_add (p0, p1);
    }
    private native int n_add (int p0, int p1);
    // ...
}
```

Unutmayın bir `@Override` yöntemi bildirilen, hangi için temsilci bir `n_`-yöntemi aynı adının öneki. Bu emin Java kod istediğinde `ManagedAdder.add`, `ManagedAdder.n_add` , geçersiz kılma C# sağlayacak çağrılacak `ManagedAdder.Add` yürütülecek yöntemi.

Bu nedenle, en önemli Soru: ne olduğunu `ManagedAdder.n_add` kadar sayfaya `ManagedAdder.Add`?

Java `native` yöntemleri (Android çalışma zamanı) Java Çalışma zamanı ile kayıtlı [JNI RegisterNatives işlevi](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp17734).
`RegisterNatives` Aşağıdaki Java yöntem adı, JNI tür imzası ve bir işlev işaretçisi çağrılacak içeren yapıları dizisi alır [çağırma JNI](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/design.html#wp715).
İşlev işaretçisi ve ardından yöntemi parametrelerini iki işaretçi bağımsız değişken almayan bir işlev olmalıdır. Java `ManagedAdder.n_add` aşağıdaki C prototip olan bir işlev yöntemi uygulanır:

```csharp
int FunctionName(JNIEnv *env, jobject this, int a, int b)
```

Xamarin.Android uygulamaz bir `RegisterNatives` yöntemi. Bunun yerine, ACW ve MCW birlikte çağırmak gereken bilgileri girin `RegisterNatives`: yöntem adını ve JNI türü imza ACW varsa, tek şey takma için bir işlev işaretçisi eksik.

Bu yerdir *bağlayıcı yöntemi* devreye girer. Üçüncü `[Register]` özel öznitelik parametredir kayıtlı türün veya bir taban sınıf parametresiz kabul edip döndürür kayıtlı türünün tanımlı bir yöntemin adını bir `System.Delegate`. Döndürülen `System.Delegate` sırayla doğru JNI işlev imzasına sahip bir yöntemi gösterir. Son olarak, bağlayıcı yöntemi döndürür temsilci *gerekir* kök GC, toplama olmayan böylece temsilci Java için sağlanan gibi.

```csharp
#pragma warning disable 0169
static Delegate cb_add;
// This method must match the third parameter of the [Register]
// custom attribute, must be static, must return System.Delegate,
// and must accept no parameters.
static Delegate GetAddHandler ()
{
    if (cb_add == null)
        cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
    return cb_add;
}
// This method is registered with JNI.
static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
{
    Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
    return __this.Add (a, b);
}
#pragma warning restore 0169
```

`GetAddHandler` Yöntemi oluşturur bir `Func<IntPtr, IntPtr, int, int,
int>` başvurduğu temsilci `n_Add` yöntemi, ardından çağırır [JNINativeWrapper.CreateDelegate](https://developer.xamarin.com/api/member/Android.Runtime.JNINativeWrapper.CreateDelegate/).
`JNINativeWrapper.CreateDelegate` Böylece işlenmeyen özel durumlar işlenir ve yükseltme içinde neden olacak bir try/catch bloğu içinde sağlanan yöntemi sarmalar [AndroidEvent.UnhandledExceptionRaiser](https://developer.xamarin.com/api/event/Android.Runtime.AndroidEnvironment.UnhandledExceptionRaiser/) olay. Sonuçta elde edilen temsilci statik depolanan `cb_add` değişken böylece GC temsilci boş değil.

Son olarak, `n_Add` yöntemi için temsilci seçme çağrı sonra yöntemi karşılık gelen yönetilen türler JNI parametreleri hazırlama için sorumlu.

Not: Her zaman kullanın `JniHandleOwnership.DoNotTransfer` bir MCW bir Java örneği edinirken. Yerel bir referans davranma (ve dolayısıyla çağırma `JNIEnv.DeleteLocalRef`) kesintiye uğrar yönetilen -&gt; Java -&gt; Yönetilen yığın geçişleri.



### <a name="complete-adder-binding"></a>Ekleyici bağlama tamamlayın

Bağlama için tam yönetilen `mono.android.tests.Adder` türü:

```csharp
[Register ("mono/android/test/Adder", DoNotGenerateAcw=true)]
public class Adder : Java.Lang.Object {

    static IntPtr class_ref = JNIEnv.FindClass ("mono/android/test/Adder");

    public Adder ()
    {
    }

    public Adder (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    protected override Type ThresholdType {
        get {return typeof (Adder);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

#region Add
    static IntPtr id_add;

    [Register ("add", "(II)I", "GetAddHandler")]
    public virtual int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        if (GetType () == ThresholdType)
            return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
        return JNIEnv.CallNonvirtualIntMethod (Handle, ThresholdClass, id_add, new JValue (a), new JValue (b));
    }

#pragma warning disable 0169
    static Delegate cb_add;
    static Delegate GetAddHandler ()
    {
        if (cb_add == null)
            cb_add = JNINativeWrapper.CreateDelegate ((Func<IntPtr, IntPtr, int, int, int>) n_Add);
        return cb_add;
    }

    static int n_Add (IntPtr jnienv, IntPtr lrefThis, int a, int b)
    {
        Adder __this = Java.Lang.Object.GetObject<Adder>(lrefThis, JniHandleOwnership.DoNotTransfer);
        return __this.Add (a, b);
    }
#pragma warning restore 0169
#endregion
}
```



### <a name="restrictions"></a>Kısıtlamalar

Bir tür yazarken, aşağıdaki ölçütlere uyan:

1.  Alt sınıflar `Java.Lang.Object`

1.  Sahip bir `[Register]` özel özniteliği

1.  `RegisterAttribute.DoNotGenerateAcw` değil `true`


GC etkileşim türü için sonra *bulunmamalıdır* olabilir tüm alanlar bir `Java.Lang.Object` veya `Java.Lang.Object` çalışma zamanında bir alt kümesi. Örneğin, türünde alanlar `System.Object` ve herhangi bir arabirim türü izin verilmez. Başvuramıyor türlerini `Java.Lang.Object` örnekleri verilir, gibi `System.String` ve `List<int>`. Bu kısıtlama, erken nesne koleksiyonu GC tarafından engellemektir.

Türü başvurabilirsiniz bir örnek alanındaki içerip içermediğini bir `Java.Lang.Object` alan türü olması gerekir, örnek `System.WeakReference` veya `GCHandle`.



## <a name="binding-abstract-methods"></a>Soyut yöntemler bağlama

Bağlama `abstract` yöntemleri sanal yöntemler bağlama için büyük ölçüde aynı. Yalnızca iki farklar vardır:

1.  Özet yöntem soyuttur. Hala korur `[Register]` özniteliği ve ilişkili yöntemi kayıt, bağlama yöntemi yalnızca taşınırken için `Invoker` türü.

1.  Olmayan bir `abstract` `Invoker` türü hangi alt sınıfların oluşturulan soyut türü. `Invoker` Türü taban sınıf içinde bildirilen tüm soyut yöntemleri geçersiz kılması gerekir ve sanal olmayan gönderme durum göz ardı edilebilir olsa yöntemi bağlama uygulama geçersiz kılınan uygulamasıdır.


Örneğin, varsayımında yukarıdaki `mono.android.test.Adder.add` yöntemi olan `abstract`. C# bağlama değiştirecek şekilde `Adder.Add` soyut ve yeni bir `AdderInvoker` türü, uygulanan tanımlanması `Adder.Add`:

```csharp
partial class Adder {
    [Register ("add", "(II)I", "GetAddHandler")]
    public abstract int Add (int a, int b);

    // The Method Registration machinery is identical to the
    // virtual method case...
}

partial class AdderInvoker : Adder {
    public AdderInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
    }

    static IntPtr id_add;
    public override int Add (int a, int b)
    {
        if (id_add == IntPtr.Zero)
            id_add = JNIEnv.GetMethodID (class_ref, "add", "(II)I");
        return JNIEnv.CallIntMethod (Handle, id_add, new JValue (a), new JValue (b));
    }
}
```

`Invoker` Türü olduğunda yalnızca gerekli Java oluşturulan örnek JNI başvuruları alma.


## <a name="binding-interfaces"></a>Arabirimleri bağlama

Arabirimleri bağlama sınıfları sanal yöntemleri içeren bağlama için kavramsal olarak benzer, ancak özelliklerinin çoğunu Zarif (ve bu nedenle Zarif) özelliklerde farklılık gösterir. Aşağıdakileri göz önünde bulundurun [Java arabirim bildiriminde](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/Adder.java#L14):

```csharp
public interface Progress {
    void onAdd(int[] values, int currentIndex, int currentSum);
}
```

Arabirim bağlamaları iki bölümü vardır: C# arabirim tanımı ve arabirimi için bir çağırıcı tanımı.



### <a name="interface-definition"></a>Arabirim tanımı

C# arabirim tanımı aşağıdaki gereksinimleri karşılamanız gerekir:

-   Arabirim tanımı olmalıdır bir `[Register]` özel öznitelik.

-   Arabirim tanımı genişletmelidir `IJavaObject interface`.
    Bunun Sağlanamaması, Java arabirimden devralan ACWs engeller.

-   Her bir arabirim yöntemi içermesi gereken bir `[Register]` karşılık gelen Java yöntemi adı, JNI imza ve bağlayıcı yöntemi belirten özniteliği.

-   Bağlayıcı yöntemi, ayrıca bağlayıcı yöntemi üzerinde bulunan türünü belirtmeniz gerekir.

Bağlama sırasında `abstract` ve `virtual` yöntemleri, bağlayıcı yöntemi Kaydedilmekte türü devralma hiyerarşisi içinde aranır. Arabirimler, bu işe yaramazsa şekilde, bu nedenle gereksinim gövdeleri içeren yöntemlerin hiçbiri olabilir bir tür belirtilen Bağlayıcısı yöntemi nerede olduğunu gösteren. Tür sonra iki nokta üst üste bağlayıcı yöntemi dizesi içinde belirtilen `':'`, ve çağırıcı içeren türü derleme nitelenmiş tür adını olması gerekir.

Arabirim yöntem bildirimleri olan karşılık gelen Java yöntemi kullanarak bir çeviri *uyumlu* türleri. Java yerleşik türleri için uyumlu karşılık gelen C# türleri, örneğin Java türleridir `int` C# `int`. Referans türleri için uyumlu türü uygun Java türü JNI tanıtıcısı sağlayabilen bir türüdür.

Arabirim üyeleri tarafından Java doğrudan çağrılan değil &ndash; çağırma üzerinden çağırıcı türü mediated &ndash; miktar esneklik izin şekilde.

Java ilerleme arabirimi olabilir [C# bildirilen](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L83):

```csharp
[Register ("mono/android/test/Adder$Progress", DoNotGenerateAcw=true)]
public interface IAdderProgress : IJavaObject {
    [Register ("onAdd", "([III)V",
            "GetOnAddHandler:Mono.Samples.SanityTests.IAdderProgressInvoker, SanityTests, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null")]
    void OnAdd (JavaArray<int> values, int currentIndex, int currentSum);
}
```

Yukarıdaki biz Java eşleme bildirimde `int[]` parametresi için bir [JavaArray&lt;int&gt;](https://developer.xamarin.com/api/type/Android.Runtime.JavaArray%601/).
Bu gerekli değildir: biz bir C# için bağlı `int[]`, veya bir `IList<int>`, veya başka bir şey tamamen. Hangi tür seçilir, `Invoker` bir Java Çevir gerekir `int[]` çağırma türü.


### <a name="invoker-definition"></a>Çağırıcı tanımı

`Invoker` Tür tanımı devralmalıdır `Java.Lang.Object`uygun arabirimini uygulayan ve arabirim tanımında başvurulan tüm bağlantı yöntemleri sağlar. Öğesinden bir sınıf bağlama farklı bir daha fazla öneri yok: `class_ref` alan ve yöntem kimlikleri örnek üyesinin, statik üyeleri olmalıdır.

Örnek üyelerin tercih ederek nedeni ile ilgilidir `JNIEnv.GetMethodID` Android çalışma zamanı davranışı. (Bu Java davranışı olabilir; test kurmadı.) `JNIEnv.GetMethodID` uygulanan bir arabirim ve bildirilen arabirimi gelir bir yöntem ararken null döndürür. Göz önünde bulundurun [java.util.SortedMap&lt;K, V&gt; ](http://developer.android.com/reference/java/util/SortedMap.html) uygulayan Java arabirimini [java.util.Map&lt;K, V&gt; ](http://developer.android.com/reference/java/util/Map.html) arabirimi. Harita sağlayan bir [temizleyin](http://developer.android.com/reference/java/util/Map.html#clear()) yöntemi, böylece bir görünen makul `Invoker` SortedMap tanımı olacaktır:

```csharp
// Fails at runtime. DO NOT FOLLOW
partial class ISortedMapInvoker : Java.Lang.Object, ISortedMap {
    static IntPtr class_ref = JNIEnv.FindClass ("java/util/SortedMap");
    static IntPtr id_clear;
    public void Clear()
    {
        if (id_clear == IntPtr.Zero)
            id_clear = JNIEnv.GetMethodID(class_ref, "clear", "()V");
        JNIEnv.CallVoidMethod(Handle, id_clear);
    }
     // ...
}
```

Yukarıdaki başarısız olur `JNIEnv.GetMethodID` döndürülecek `null` bakarken `Map.clear` yöntemi aracılığıyla `SortedMap` sınıf örneği.

Bu iki çözümü vardır: her yöntemi gelir hangi arabirimi izlemek ve sahip bir `class_ref` her arabirimi veya örnek üyeleri olarak her şeyi tutmak ve çoğu türetilmiş sınıf türü, arabirim türü yöntemi arama gerçekleştirin. İkinci yapılır **Mono.Android.dll**.

Çağırıcı tanımı altı bölümü vardır: Oluşturucusu `Dispose` yöntemi, `ThresholdType` ve `ThresholdClass` üyeleri, `GetObject` yöntemini, arabirim yöntemi uygulaması ve bağlayıcı yöntemi uygulama.



#### <a name="constructor"></a>Oluşturucu

Çağrılan örnek çalışma zamanı sınıfının arama ve çalışma zamanı sınıf örneği depolamak Oluşturucusu gereken `class_ref` alan:

```csharp
partial class IAdderProgressInvoker {
    IntPtr class_ref;
    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref   = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }
}
```

Not: `Handle` Oluşturucusu gövdesinde özelliği kullanılır ve `handle` parametresi olarak Android v4.0 `handle` temel oluşturucuyu yürütme tamamlandıktan sonra parametresi geçersiz olabilir.


#### <a name="dispose-method"></a>Dispose Yöntemi

`Dispose` Yöntemi oluşturucuda ayrılan genel başvuru serbest gerekir:

```csharp
partial class IAdderProgressInvoker {
    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }
}
```


#### <a name="thresholdtype-and-thresholdclass"></a>ThresholdType ve ThresholdClass

`ThresholdType` Ve `ThresholdClass` üyeleri bir sınıf bağlama bulunan için aynıdır:

```csharp
partial class IAdderProgressInvoker {
    protected override Type ThresholdType {
        get {
            return typeof (IAdderProgressInvoker);
        }
    }
    protected override IntPtr ThresholdClass {
        get {
            return class_ref;
        }
    }
}
```


#### <a name="getobject-method"></a>GetObject Metodu

Statik `GetObject` yöntemi desteklemek için gereken [Extensions.JavaCast&lt;T&gt;()](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/):

```csharp
partial class IAdderProgressInvoker {
    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }
}
```


#### <a name="interface-methods"></a>Arabirim yöntemleri

Arabirimin her yöntemi JNI aracılığıyla karşılık gelen Java yöntemini çağıran bir uygulama olmalıdır:

```csharp
partial class IAdderProgressInvoker {
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd", "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd, new JValue (JNIEnv.ToJniHandle (values)), new JValue (currentIndex), new JValue (currentSum));
    }
}
```



#### <a name="connector-methods"></a>Bağlayıcı yöntemleri

Bağlayıcı yöntemleri ve destekleyici altyapının uygun C# türleri JNI parametreleri hazırlama için sorumludur. Java `int[]` parametre JNI geçirilecektir `jintArray`, olduğu bir `IntPtr` C# içinde. `IntPtr` İçin sıralanmış gerekir bir `JavaArray<int>` C# arabirimi çağırma desteklemek için:

```csharp
partial class IAdderProgressInvoker {
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
}
```

Varsa `int[]` üzerinden tercih edilen olacaktır `JavaList<int>`, ardından [JNIEnv.GetArray()](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetArray/(System.IntPtr%2cAndroid.Runtime.JniHandleOwnership%2cSystem.Type)) yerine kullanılabilir:

```csharp
int[] _values = (int[]) JNIEnv.GetArray(values, JniHandleOwnership.DoNotTransfer, typeof (int));
```

Ancak, unutmayın, `JNIEnv.GetArray` dizinin tamamı büyük diziler için bu çok sayıda eklenen GC baskısı sonuçlanabilir şekilde VM'ler arasında kopyalar.


### <a name="complete-invoker-definition"></a>Tam çağırıcı tanımı

[IAdderProgressInvoker tanımı tamamlamak](https://github.com/xamarin/monodroid-samples/blob/master/SanityTests/ManagedAdder.cs#L88):

```csharp
class IAdderProgressInvoker : Java.Lang.Object, IAdderProgress {

    IntPtr class_ref;

    public IAdderProgressInvoker (IntPtr handle, JniHandleOwnership transfer)
        : base (handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass (Handle);
        class_ref = JNIEnv.NewGlobalRef (lref);
        JNIEnv.DeleteLocalRef (lref);
    }

    protected override void Dispose (bool disposing)
    {
        if (this.class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef (this.class_ref);
        this.class_ref = IntPtr.Zero;
        base.Dispose (disposing);
    }

    protected override Type ThresholdType {
        get {return typeof (IAdderProgressInvoker);}
    }

    protected override IntPtr ThresholdClass {
        get {return class_ref;}
    }

    public static IAdderProgress GetObject (IntPtr handle, JniHandleOwnership transfer)
    {
        return new IAdderProgressInvoker (handle, transfer);
    }

#region OnAdd
    IntPtr id_onAdd;
    public void OnAdd (JavaArray<int> values, int currentIndex, int currentSum)
    {
        if (id_onAdd == IntPtr.Zero)
            id_onAdd = JNIEnv.GetMethodID (class_ref, "onAdd",
                    "([III)V");
        JNIEnv.CallVoidMethod (Handle, id_onAdd,
                new JValue (JNIEnv.ToJniHandle (values)),
                new JValue (currentIndex),
new JValue (currentSum));
    }

#pragma warning disable 0169
    static Delegate cb_onAdd;
    static Delegate GetOnAddHandler ()
    {
        if (cb_onAdd == null)
            cb_onAdd = JNINativeWrapper.CreateDelegate ((Action<IntPtr, IntPtr, IntPtr, int, int>) n_OnAdd);
        return cb_onAdd;
    }

    static void n_OnAdd (IntPtr jnienv, IntPtr lrefThis, IntPtr values, int currentIndex, int currentSum)
    {
        IAdderProgress __this = Java.Lang.Object.GetObject<IAdderProgress>(lrefThis, JniHandleOwnership.DoNotTransfer);
        using (var _values = new JavaArray<int>(values, JniHandleOwnership.DoNotTransfer)) {
            __this.OnAdd (_values, currentIndex, currentSum);
        }
    }
#pragma warning restore 0169
#endregion
}
```



## <a name="jni-object-references"></a>JNI Object References

Birçok JNIEnv yöntemleri döndürür *JNI* *nesnesinin başvurduğu*, benzer olduğu `GCHandle`s. JNI üç farklı türde nesne başvuruları sağlar: yerel başvuruları, genel başvuruları ve zayıf genel başvurular. Üç olarak temsil edilir `System.IntPtr`, *ancak* (göredir JNI işlev türleri bölümünde) tüm `IntPtr`döndürülen s `JNIEnv` başvuruları yöntemleridir. Örneğin, [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/) döndüren bir `IntPtr`, ancak bunu değil dönüş nesne başvurusu, döndürür bir `jmethodID`. Başvurun [JNI işlev belgelerine](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html) Ayrıntılar için.

Tarafından oluşturulan yerel başvuruları *çoğu* başvuru oluşturma yöntemleri.
Android, yalnızca belirli bir zamanda, genellikle 512 mevcut yerel başvuruları sınırlı sayıda izin verir. Yerel başvuruları aracılığıyla silinebilir [JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/).
JNI farklı olarak, tüm yerel başvuruları hangi dönüş nesne başvuruları geri JNIEnv yöntemleri başvuru; [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/) döndüren bir *genel* başvuru. Yerel başvuruları, büyük olasılıkla oluşturarak yapabilecekleriniz olabildiğince çabuk silme önemle tavsiye edilir bir [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) nesne geçici ve belirtme `JniHandleOwnership.TransferLocalRef` için [Java.Lang.Object (IntPtr işlemek, JniHandleOwnership aktarım)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) Oluşturucusu.

Genel başvuru tarafından oluşturulan [JNIEnv.NewGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewGlobalRef/) ve [JNIEnv.FindClass](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/).
Bunlar birlikte yok [JNIEnv.DeleteGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteGlobalRef/).
Donanım aygıtları yaklaşık 52,000 genel başvuruları sınırının bulunurken Öykünücüler 2.000 bekleyen genel başvuruları sınırı vardır.

Zayıf genel başvurular yalnızca Android v2.2 (Froyo) ve sonraki sürümlerinde kullanılabilir. Zayıf genel başvurular ile silinebilir [JNIEnv.DeleteWeakGlobalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteWeakGlobalRef/(System.IntPtr)).


### <a name="dealing-with-jni-local-references"></a>JNI yerel başvuruları postalarla

[JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/), [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/), [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/), [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/)ve [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) yöntemleri döndürür bir `IntPtr` bir Java nesnesine JNI yerel bir referans içerir veya `IntPtr.Zero` Java döndürülürse `null`. (512 girişleri) başvuruları emin olmak için tercih edilir sonra bekleyen yerel başvuruları sınırlı sayıda nedeniyle zamanında silinir. İle yerel başvuruları ele alınabilir üç yolu vardır: açıkça oluşturma, silme bir `Java.Lang.Object` bunları ve kullanarak tutmak için örnek `Java.Lang.Object.GetObject<T>()` etrafında bir yönetilen aranabilir sarmalayıcısı oluşturmak için.



### <a name="explicitly-deleting-local-references"></a>Açıkça yerel başvuruları silme

[JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) yerel başvuruları silmek için kullanılır. Yerel bir referans silindikten sonra emin olmak için dikkatli olunması gerekir böylece bu artık, kullanılamaz `JNIEnv.DeleteLocalRef` olan yerel bir referans ile yapılan son işlemdir.

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
try {
    // Do something with `lref`
}
finally {
    JNIEnv.DeleteLocalRef (lref);
}
```



### <a name="wrapping-with-javalangobject"></a>İle Java.Lang.Object sarmalama

`Java.Lang.Object` sağlayan bir [Java.Lang.Object (IntPtr tanıtıcısı, JniHandleOwnership aktarım)](https://developer.xamarin.com/api/constructor/Java.Lang.Object.Object/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) mevcut bir JNI başvuru sarmalamak için kullanılan Oluşturucu. [JniHandleOwnership](https://developer.xamarin.com/api/type/Android.Runtime.JniHandleOwnership/) parametre belirler nasıl `IntPtr` parametresi kabul:

-   [JniHandleOwnership.DoNotTransfer](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.DoNotTransfer/) &ndash; The created `Java.Lang.Object` instance will create a new global reference from the `handle` parameter, and `handle` is unchanged.
    Serbest bırakma için çağıran sorumludur `handle` gerekirse,.

-   [JniHandleOwnership.TransferLocalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; The created `Java.Lang.Object` instance will create a new global reference from the `handle` parameter, and `handle` is deleted with [JNIEnv.DeleteLocalRef](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.DeleteLocalRef/) . Arayan değil serbest gerekir `handle` , değil kullanmalıdır `handle` Oluşturucusu yürütme tamamlandıktan sonra.

-   [JniHandleOwnership.TransferGlobalRef](https://developer.xamarin.com/api/field/Android.Runtime.JniHandleOwnership.TransferLocalRef/) &ndash; The created `Java.Lang.Object` instance will take over ownership of the `handle` parameter. Arayan değil serbest gerekir `handle` .


Yerel refs JNI yöntemi çağırma yöntemleri döndürür beri `JniHandleOwnership.TransferLocalRef` normal olarak kullanılacaktır:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef);
```

Oluşturulan genel başvuru kadar boşaltılması değil `Java.Lang.Object` toplanacak örneğidir. İçin tamamlayabilirseniz örneğini atma çöp koleksiyonları hızlandırma genel başvuru boşaltmak:

```csharp
IntPtr lref = JNIEnv.CallObjectMethod(instance, methodID);
using (var value = new Java.Lang.Object (lref, JniHandleOwnership.TransferLocalRef)) {
    // use value ...
}
```



### <a name="using-javalangobjectgetobjectlttgt"></a>Java.Lang.Object.GetObject kullanarak&lt;T&gt;)

`Java.Lang.Object` sağlayan bir [Java.Lang.Object.GetObject&lt;T&gt;(IntPtr tanıtıcısı, JniHandleOwnership aktarım)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) belirtilen türde bir yönetilen aranabilir sarmalayıcısı oluşturmak için kullanılan yöntem.

Türü `T` aşağıdaki gereksinimleri karşılamanız gerekir:

1.  `T` bir başvuru türü olmalıdır.

1.  `T` uygulamalıdır `IJavaObject` arabirimi.

1.  Varsa `T` bir Özet sınıf veya arabirim, ardından değil `T` bir oluşturucu parametre türüyle sağlamalısınız `(IntPtr,
    JniHandleOwnership)` .

1.  Varsa `T` soyut bir sınıf ya da bir arabirim, var. *gerekir* olması bir *çağırıcı* için kullanılabilir `T` . Çağırıcısı devralan bir Özet olmayan türüdür `T` veya uygulayan `T` , ve aynı ada sahip `T` çağırıcı soneki. Örneğin, T arabirimi ise `Java.Lang.IRunnable` , ardından türü `Java.Lang.IRunnableInvoker` bulunmalı ve gerekli içermelidir `(IntPtr,
    JniHandleOwnership)` Oluşturucusu.


Yerel refs JNI yöntemi çağırma yöntemleri döndürür beri `JniHandleOwnership.TransferLocalRef` normal olarak kullanılacaktır:

```csharp
IntPtr lrefString = JNIEnv.CallObjectMethod(instance, methodID);
Java.Lang.String value = Java.Lang.Object.GetObject<Java.Lang.String>( lrefString, JniHandleOwnership.TransferLocalRef);
```

<a name="_Looking_up_Java_Types" />

## <a name="looking-up-java-types"></a>Java türlerini arama

Bir alan veya JNI yönteminde aramak için bildiri türü alan veya yöntem için ilk aranması gerekir. [Android.Runtime.JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)) yöntemi Java türlerini aramak için kullanılır. Dize parametresi *Basitleştirilmiş türü başvurusu* veya *tam tür başvuru* Java türü için. Bkz: [JNI türü başvurular bölümüne](#_JNI_Type_References) Basitleştirilmiş ve tam tür başvuruları hakkında ayrıntılı bilgi için.

Not: farklı olarak her diğer `JNIEnv` nesne örneklerini döndüren yöntemi `FindClass` olmayan yerel bir referans genel bir başvuru döndürür.

<a name="_Instance_Fields" />

## <a name="instance-fields"></a>Örnek alanı

Alanları aracılığıyla yönetilen *kimlikleri alan*. Alan kodlarının yoluyla elde [JNIEnv.GetFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFieldID/), sınıf gerektiren alan alanın adı, tanımlanmış ve [JNI tür imzası](#JNI_Type_Signatures) alanının.

Alan kimlikleri boşaltılması gerek yoktur ve karşılık gelen bir Java türü yüklü olduğu sürece geçerlidir. (Android şu anda sınıfı kaldırılmasını desteklemez.)

İki örneği alanları düzenleme için yöntemler kümesi vardır: Okuma örneği alanları ve bir örnek alanlarını yazmak için bir tane. Tüm yöntemler kümesi okumak veya alan değeri yazmak bir alan kodu gerektirir.


### <a name="reading-instance-field-values"></a>Okuma örneği alan değerleri

Örnek alan değerlerini okumak için yöntemler kümesi adlandırma deseni izler:

```csharp
* JNIEnv.Get*Field(IntPtr instance, IntPtr fieldID);
```
Burada `*` alan türü:

-   [JNIEnv.GetObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetObjectField/) &ndash; gibi bir yerleşik türünde olmayan herhangi bir örnek alan değerini okumak `java.lang.Object` , dizileri ve arabirim türleri. Döndürülen değer JNI yerel başvurudur.

-   [JNIEnv.GetBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetBooleanField/) &ndash; değerini okumak `bool` örneği alanları.

-   [JNIEnv.GetByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetByteField/) &ndash; değerini okumak `sbyte` örneği alanları.

-   [JNIEnv.GetCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetCharField/) &ndash; değerini okumak `char` örneği alanları.

-   [JNIEnv.GetShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetShortField/) &ndash; değerini okumak `short` örneği alanları.

-   [JNIEnv.GetIntField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetIntField/) &ndash; değerini okumak `int` örneği alanları.

-   [JNIEnv.GetLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetLongField/) &ndash; değerini okumak `long` örneği alanları.

-   [JNIEnv.GetFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetFloatField/) &ndash; değerini okumak `float` örneği alanları.

-   [JNIEnv.GetDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetDoubleField/) &ndash; değerini okumak `double` örneği alanları.





### <a name="writing-instance-field-values"></a>Örnek alanı değerleri yazılıyor

Örnek alan değerlerini yazma yöntemleri kümesini adlandırma deseni izler:

```csharp
JNIEnv.SetField(IntPtr instance, IntPtr fieldID, Type value);
```

Burada *türü* alan türü:

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; gibi bir yerleşik türünde olmayan herhangi bir alan değerini yazmak `java.lang.Object` , dizileri ve arabirim türleri. `IntPtr` Değeri JNI yerel başvuru, JNI genel başvurusu, JNI zayıf genel başvuru olabilir veya `IntPtr.Zero` (için `null` ).

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; değerini yazmak `bool` örneği alanları.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; değerini yazmak `sbyte` örneği alanları.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; değerini yazmak `char` örneği alanları.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; değerini yazmak `short` örneği alanları.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; değerini yazmak `int` örneği alanları.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; değerini yazmak `long` örneği alanları.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; değerini yazmak `float` örneği alanları.

-   [JNIEnv.SetField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; değerini yazmak `double` örneği alanları.


<a name="_Static_Fields" />

## <a name="static-fields"></a>Statik alanları

Statik alanları aracılığıyla yönetilen *kimlikleri alan*. Alan kodlarının yoluyla elde [JNIEnv.GetStaticFieldID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFieldID/), sınıf gerektiren alan alanın adı, tanımlanmış ve [JNI tür imzası](#JNI_Type_Signatures) alanının.

Alan kimlikleri boşaltılması gerek yoktur ve karşılık gelen bir Java türü yüklü olduğu sürece geçerlidir. (Android şu anda sınıfı kaldırılmasını desteklemez.)

Yöntem statik alanları düzenleme için iki kümesi vardır: Okuma örneği alanları ve bir örnek alanlarını yazmak için bir tane. Tüm yöntemler kümesi okumak veya alan değeri yazmak bir alan kodu gerektirir.


### <a name="reading-static-field-values"></a>Statik alan değerlerini okuma

Statik alan değerlerini okumak için yöntemler kümesi adlandırma deseni izler:

```csharp
* JNIEnv.GetStatic*Field(IntPtr class, IntPtr fieldID);
```

Burada `*` alan türü:

-   [JNIEnv.GetStaticObjectField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticObjectField/) &ndash; gibi bir yerleşik türünde olmayan herhangi bir statik alan değerini okumak `java.lang.Object` , dizileri ve arabirim türleri. Döndürülen değer JNI yerel başvurudur.

-   [JNIEnv.GetStaticBooleanField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticBooleanField/) &ndash; değerini okumak `bool` statik alanları.

-   [JNIEnv.GetStaticByteField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticByteField/) &ndash; değerini okumak `sbyte` statik alanları.

-   [JNIEnv.GetStaticCharField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticCharField/) &ndash; değerini okumak `char` statik alanları.

-   [JNIEnv.GetStaticShortField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticShortField/) &ndash; değerini okumak `short` statik alanları.

-   [JNIEnv.GetStaticLongField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticLongField/) &ndash; değerini okumak `long` statik alanları.

-   [JNIEnv.GetStaticFloatField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticFloatField/) &ndash; değerini okumak `float` statik alanları.

-   [JNIEnv.GetStaticDoubleField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticDoubleField/) &ndash; değerini okumak `double` statik alanları.



### <a name="writing-static-field-values"></a>Statik alan değerleri yazılıyor

Statik alan değerlerini yazma yöntemleri kümesini adlandırma deseni izler:

```csharp
JNIEnv.SetStaticField(IntPtr class, IntPtr fieldID, Type value);
```

Burada *türü* alan türü:

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.IntPtr)) &ndash; gibi bir yerleşik türünde olmayan herhangi bir statik alan değerini yazmak `java.lang.Object` , dizileri ve arabirim türleri. `IntPtr` Değeri JNI yerel başvuru, JNI genel başvurusu, JNI zayıf genel başvuru olabilir veya `IntPtr.Zero` (için `null` ).

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Boolean)) &ndash; değerini yazmak `bool` statik alanları.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.SByte)) &ndash; değerini yazmak `sbyte` statik alanları.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Char)) &ndash; değerini yazmak `char` statik alanları.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int16)) &ndash; değerini yazmak `short` statik alanları.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int32)) &ndash; değerini yazmak `int` statik alanları.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Int64)) &ndash; değerini yazmak `long` statik alanları.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Single)) &ndash; değerini yazmak `float` statik alanları.

-   [JNIEnv.SetStaticField](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.SetStaticField/(System.IntPtr,System.IntPtr,System.Double)) &ndash; değerini yazmak `double` statik alanları.


<a name="_Instance_Methods" />

## <a name="instance-methods"></a>Örnek yöntemleri

Örnek yöntemleri aracılığıyla çağrılır *yöntemi kimlikleri*. Yöntem kimlikleri elde edilir aracılığıyla [JNIEnv.GetMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetMethodID/), türü gerektiren yöntemi yönteminin adı, tanımlanan ve [JNI tür imzası](#JNI_Type_Signatures) yönteminin.

Yöntem kimlikleri boşaltılması gerek yoktur ve karşılık gelen bir Java türü yüklü olduğu sürece geçerlidir. (Android şu anda sınıfı kaldırılmasını desteklemez.)

Yöntemleri çağırma yöntemlerini iki kümesi vardır: yöntemler neredeyse çalıştırma ve yöntemlerini neredeyse olmayan çağırma için bir. Her iki yöntem kümesi yöntemini çağırmak için bir yöntem kimliği gerektirir ve sanal olmayan çağırma Ayrıca hangi sınıf uygulamasını çağrılması belirtmenizi gerektirir.

Arabirim yöntemleri, içinde bildiri türü yalnızca aranabilir; Genişletilmiş/devralınmış arabirimlerinden gelen yöntemleri Aranan olamaz. Sonraki bağlama arabirimleri Bkz / çağırıcı uygulama bölümünde daha fazla ayrıntı için.

Herhangi bir yöntemini sınıfında tanımlanan veya herhangi bir temel sınıf veya uygulanan arabirimi aranabilir.


### <a name="virtual-method-invocation"></a>Sanal yöntemi çağırma

Çağıran yöntemleri için yöntemler kümesi neredeyse adlandırma deseni izler:

```csharp
* JNIEnv.Call*Method( IntPtr instance, IntPtr methodID, params JValue[] args );
```

Burada `*` yöntemin dönüş türü.

-   [JNIEnv.CallObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallObjectMethod/) &ndash; gibi yerleşik olmayan türü döndüren bir yöntem çağırma `java.lang.Object` , dizi ve arabirimleri. Döndürülen değer JNI yerel başvurudur.

-   [JNIEnv.CallBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallBooleanMethod/) &ndash; döndüren bir yöntem çağırma bir `bool` değeri.

-   [JNIEnv.CallByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallByteMethod/) &ndash; döndüren bir yöntem çağırma bir `sbyte` değeri.

-   [JNIEnv.CallCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallCharMethod/) &ndash; döndüren bir yöntem çağırma bir `char` değeri.

-   [JNIEnv.CallShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallShortMethod/) &ndash; döndüren bir yöntem çağırma bir `short` değeri.

-   [JNIEnv.CallLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; döndüren bir yöntem çağırma bir `long` değeri.

-   [JNIEnv.CallFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallFloatMethod/) &ndash; döndüren bir yöntem çağırma bir `float` değeri.

-   [JNIEnv.CallDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallDoubleMethod/) &ndash; döndüren bir yöntem çağırma bir `double` değeri.



### <a name="non-virtual-method-invocation"></a>Sanal olmayan yöntemi çağırma

Çağıran yöntemleri için yöntemler kümesi neredeyse olmayan adlandırma deseni izler:

```csharp
* JNIEnv.CallNonvirtual*Method( IntPtr instance, IntPtr class, IntPtr methodID, params JValue[] args );
```

Burada `*` yöntemin dönüş türü. Sanal olmayan yöntemi çağırma genellikle sanal bir yöntem temel yöntemini çağırmak için kullanılır.

-   [JNIEnv.CallNonvirtualObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualObjectMethod/) &ndash; olmayan neredeyse gibi yerleşik olmayan türü döndüren bir yöntem çağırma `java.lang.Object` , dizi ve arabirimleri. Döndürülen değer JNI yerel başvurudur.

-   [JNIEnv.CallNonvirtualBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualBooleanMethod/) &ndash; olmayan neredeyse döndüren bir yöntem çağırma bir `bool` değeri.

-   [JNIEnv.CallNonvirtualByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualByteMethod/) &ndash; olmayan neredeyse döndüren bir yöntem çağırma bir `sbyte` değeri.

-   [JNIEnv.CallNonvirtualCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualCharMethod/) &ndash; olmayan neredeyse döndüren bir yöntem çağırma bir `char` değeri.

-   [JNIEnv.CallNonvirtualShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualShortMethod/) &ndash; olmayan neredeyse döndüren bir yöntem çağırma bir `short` değeri.

-   [JNIEnv.CallNonvirtualLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualLongMethod/) &ndash; olmayan neredeyse döndüren bir yöntem çağırma bir `long` değeri.

-   [JNIEnv.CallNonvirtualFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualFloatMethod/) &ndash; olmayan neredeyse döndüren bir yöntem çağırma bir `float` değeri.

-   [JNIEnv.CallNonvirtualDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallNonvirtualDoubleMethod/) &ndash; olmayan neredeyse döndüren bir yöntem çağırma bir `double` değeri.


<a name="_Static_Methods" />

## <a name="static-methods"></a>Statik yöntemler

Statik yöntemler aracılığıyla çağrılır *yöntemi kimlikleri*. Yöntem kimlikleri elde edilir aracılığıyla [JNIEnv.GetStaticMethodID](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.GetStaticMethodID/), türü gerektiren yöntemi yönteminin adı, tanımlanan ve [JNI tür imzası](#JNI_Type_Signatures) yönteminin.

Yöntem kimlikleri boşaltılması gerek yoktur ve karşılık gelen bir Java türü yüklü olduğu sürece geçerlidir. (Android şu anda sınıfı kaldırılmasını desteklemez.)



### <a name="static-method-invocation"></a>Statik yöntem çağırma

Çağıran yöntemleri için yöntemler kümesi neredeyse adlandırma deseni izler:

```csharp
* JNIEnv.CallStatic*Method( IntPtr class, IntPtr methodID, params JValue[] args );
```

Burada `*` yöntemin dönüş türü.

-   [JNIEnv.CallStaticObjectMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticObjectMethod/) &ndash; gibi yerleşik olmayan türü döndüren bir statik yöntem çağırma `java.lang.Object` , dizi ve arabirimleri. Döndürülen değer JNI yerel başvurudur.

-   [JNIEnv.CallStaticBooleanMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticBooleanMethod/) &ndash; Invoke a static method which returns a `bool` value.

-   [JNIEnv.CallStaticByteMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticByteMethod/) &ndash; döndüren statik yöntem çağrılacak bir `sbyte` değeri.

-   [JNIEnv.CallStaticCharMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticCharMethod/) &ndash; döndüren statik yöntem çağrılacak bir `char` değeri.

-   [JNIEnv.CallStaticShortMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticShortMethod/) &ndash; döndüren statik yöntem çağrılacak bir `short` değeri.

-   [JNIEnv.CallStaticLongMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallLongMethod/) &ndash; döndüren statik yöntem çağrılacak bir `long` değeri.

-   [JNIEnv.CallStaticFloatMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticFloatMethod/) &ndash; döndüren statik yöntem çağrılacak bir `float` değeri.

-   [JNIEnv.CallStaticDoubleMethod](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallStaticDoubleMethod/) &ndash; döndüren statik yöntem çağrılacak bir `double` değeri.


<a name="JNI_Type_Signatures" />

## <a name="jni-type-signatures"></a>JNI türü imzaları

[JNI türü imzaları](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/types.html#wp16432) olan [JNI tür başvuruları](#_JNI_Type_References) (ancak değil Basitleştirilmiş tür başvuruları) yöntemleri dışında. Yöntemleri ile açık parantez JNI tür imzası olan `'('`, tüm birlikte (hiçbir ayıran virgüller ya da başka bir şey), birleştirilmiş türleri ve ardından bir kapatma parantezi parametresinin türü başvurularını ardından `')'`, yöntemin dönüş türü JNI türü başvuru tarafından kullanılıyordu.

Örneğin, Java yöntemi verilen:

```java
long f(int n, String s, int[] array);
```

JNI tür imzası olacaktır:

```csharp
(ILjava/lang/String;[I)J
```

Genel olarak, olan *kesinlikle* kullanmak için önerilen `javap` JNI imzaları belirlemek için komutu. Örneğin, JNI türü imzası [java.lang.Thread.State.valueOf(String)](http://developer.android.com/reference/java/lang/Thread.State.html#valueOf(java.lang.String)) yöntemi "(Ljava/lang/dize;) Ljava/lang/iş parçacığı$ durumu;", JNI imzası yazarken [ java.lang.Thread.State.values](http://developer.android.com/reference/java/lang/Thread.State.html#values) yöntemi "() [Ljava/lang/iş parçacığı$ durumu;". Sondaki noktalı için dikkat edin; Bu *olan* JNI tür imzası parçası.

<a name="_JNI_Type_References" />

## <a name="jni-type-references"></a>JNI tür başvuruları

JNI tür başvuruları Java türü başvurularından farklıdır. Tam Java tür adları gibi kullanamazsınız `java.lang.String` JNI ile JNI Çeşitlemeler bunun yerine kullanmalısınız `"java/lang/String"` veya `"Ljava/lang/String;"`, bağlam bağlı olarak; Ayrıntılar için aşağıya bakın.
JNI tür başvuruları dört tür vardır:

-  **built-in**
-  **Basitleştirilmiş**
-  **Türü**
-  **Dizi**


### <a name="built-in-type-references"></a>Yerleşik tür başvuruları

Yerleşik tür başvuruları yerleşik değer türleri başvurmak için kullanılan tek bir karakter var. Eşleme aşağıdaki gibidir:

-  `"B"` için `sbyte` .
-  `"S"` için `short` .
-  `"I"` için `int` .
-  `"J"` için `long` .
-  `"F"` için `float` .
-  `"D"` için `double` .
-  `"C"` için `char` .
-  `"Z"` için `bool` .
-  `"V"` için `void` yönteminin dönüş türü.


<a name="_Simplified_Type_References_1" />

### <a name="simplified-type-references"></a>Basitleştirilmiş tür başvuruları

Basitleştirilmiş tür başvuruları yalnızca kullanılabilir [JNIEnv.FindClass(string)](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.FindClass/(System.String)).
Basitleştirilmiş türü başvurusu çıkarmaya iki yolu vardır:

1.  Bir tam Java adından yerine her `'.'` içinde paket adı ve tür adıyla önce `'/'` ve her `'.'` bir tür adıyla içinde `'$'` .

1.  Çıktısını okuma `'unzip -l android.jar | grep JavaName'` .


İki ya da bulunan Java tür sonuçlanır [java.lang.Thread.State](http://developer.android.com/reference/java/lang/Thread.State.html) Basitleştirilmiş türü referansı eşleniyor `java/lang/Thread$State`.


### <a name="type-references"></a>Tür başvuruları

Bir yerleşik türü bir başvuru veya Basitleştirilmiş türü ile bir tür başvurusu olan bir `'L'` öneki ve `';'` soneki. Java türü için [java.lang.String](http://developer.android.com/reference/java/lang/String.html), Basitleştirilmiş türü başvurusu `"java/lang/String"`, tür referansı olsa `"Ljava/lang/String;"`.

Tür başvuruları JNI imzalar ve dizi tür başvuruları ile kullanılır.

Çıktısını okuyarak türü başvuru sağlamak için ek bir yoludur `'javap -s -classpath android.jar fully.qualified.Java.Name'`.
Türüne bağlı olarak bir oluşturucu bildirimi kullanabilir karmaşık veya yönteminin dönüş türü JNI adını belirlemek için. Örneğin:

```shell
$ javap -classpath android.jar -s java.lang.Thread.State
Compiled from "Thread.java"
```

```java
public final class java.lang.Thread$State extends java.lang.Enum{
public static final java.lang.Thread$State NEW;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State RUNNABLE;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State BLOCKED;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TIMED_WAITING;
  Signature: Ljava/lang/Thread$State;
public static final java.lang.Thread$State TERMINATED;
  Signature: Ljava/lang/Thread$State;
public static java.lang.Thread$State[] values();
  Signature: ()[Ljava/lang/Thread$State;
public static java.lang.Thread$State valueOf(java.lang.String);
  Signature: (Ljava/lang/String;)Ljava/lang/Thread$State;
static {};
  Signature: ()V
}
```

`Thread.State` İmzası kullandığımız bir Java enum türü olduğundan `valueOf` tür referansı Ljava/lang/iş parçacığı$; olup olmadığını belirlemek amacıyla yöntemi.



### <a name="array-type-references"></a>Dizi tür başvuruları

Dizi türü başvuruları `'['` JNI türü başvuru öneki.
Diziler belirtirken Basitleştirilmiş tür başvuruları kullanılamaz.

Örneğin, `int[]` olan `"[I"`, `int[][]` olan `"[[I"`, ve `java.lang.Object[]` olan `"[Ljava/lang/Object;"`.



## <a name="java-generics-and-type-erasure"></a>Java genel türler ve türü silinme

*Çoğu* Java genel türler JNI görülen süreyi *yok*.
Bazı "kırışıklıkların" vardır, ancak bu kırışıklıkların nasıl Java nasıl JNI arar ve Genel üyeler çağırır ile değil genel türler, etkileşimde içinde olan.

JNI kullanılırken genel bir tür veya üye ve genel olmayan tür veya üye arasında fark yoktur. Örneğin, genel tür [java.lang.Class&lt;T&gt; ](http://developer.android.com/reference/java/lang/Class.html) de "ham" genel tür olan `java.lang.Class`, her ikisi de aynı Basitleştirilmiş türü başvuru yapıyor `"java/lang/Class"`.


## <a name="java-native-interface-support"></a>Java yerel arabirimi desteği

[Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/) Jave yerel arabirimi (JNI) için yönetilen sarmalayıcı değil. JNI işlevler içinde bildirilen [Java yerel arabirim belirtimi](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html), açık kaldırmak için yöntemleri değiştirilmiş olsa da `JNIEnv*` parametre ve `IntPtr` yerine kullanılan `jobject`, `jclass`, `jmethodID`vb. Örneğin, göz önünde bulundurun [JNI NewObject işlevi](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp4517):

```csharp
jobject NewObjectA(JNIEnv *env, jclass clazz, jmethodID methodID, jvalue *args);
```

Bu olarak sunulan [JNIEnv.NewObject](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.NewObject/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) yöntemi:

```csharp
public static IntPtr NewObject(IntPtr clazz, IntPtr jmethod, params JValue[] parms);
```

İki çağrıları arasında çevirme oldukça basittir. C'de olurdu:

```c
jobject CreateMapActivity(JNIEnv *env)
{
    jclass    Map_Class   = (*env)->FindClass(env, "mono/samples/googlemaps/MyMapActivity");
    jmethodID Map_defCtor = (*env)->GetMethodID (env, Map_Class, "<init>", "()V");
    jobject   instance    = (*env)->NewObject (env, Map_Class, Map_defCtor);

    return instance;
}
```

C# eşdeğer olacaktır:

```csharp
IntPtr CreateMapActivity()
{
    IntPtr Map_Class   = JNIEnv.FindClass ("mono/samples/googlemaps/MyMapActivity");
    IntPtr Map_defCtor = JNIEnv.GetMethodID (Map_Class, "<init>", "()V");
    IntPtr instance    = JNIEnv.NewObject (Map_Class, Map_defCtor);

    return instance;
}
```

Bir IntPtr tutulan bir Java nesne örneği olduktan sonra Bununla bir şeyler istersiniz. JNIEnv yöntemleri gibi kullanabilir [ <span class="external">JNIEnv.CallVoidMethod()</span> ](https://developer.xamarin.com/api/member/Android.Runtime.JNIEnv.CallVoidMethod/p/System.IntPtr/System.IntPtr/Android.Runtime.JValue[]/) olup olmadığını zaten bir analog C# sarmalayıcı sarmalayıcı JNI başvuru oluşturmak istersiniz sonra ancak bunu yapmak için. Bu nedenle aracılığıyla yapabilirsiniz [Extensions.JavaCast <t>()</t> ](https://developer.xamarin.com/api/member/Android.Runtime.Extensions.JavaCast%7BTResult%7D/p/Android.Runtime.IJavaObject/) genişletme yöntemi:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = new Java.Lang.Object(lrefActivity, JniHandleOwnership.TransferLocalRef)
    .JavaCast<Activity>();
```

Aynı zamanda [Java.Lang.Object.GetObject <t>()</t> ](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) yöntemi:

```csharp
IntPtr lrefActivity = CreateMapActivity();

// imagine that Activity were instead an interface or abstract type...
Activity mapActivity = Java.Lang.Object.GetObject<Activity>(lrefActivity, JniHandleOwnership.TransferLocalRef);
```

Ayrıca, tüm JNI işlevleri kaldırarak değiştirilmiş `JNIEnv*` parametresi her JNI işlevinde yok.


## <a name="summary"></a>Özet

JNI doğrudan postalarla maliyeti kaçınılmalıdır korkunç bir deneyimdir. Ne yazık ki, her zaman kaçınılabilir değildir; Neyse Android için Mono ilişkisiz Java talepleriyle isabet olduğunda bu kılavuzda bazı Yardım sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [SanityTests (örnek)](https://developer.xamarin.com/samples/SanityTests/)
- [Java yerel arabirim belirtimi](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/jniTOC.html)
- [Java yerel arabirimi işlevleri](http://download.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html)
