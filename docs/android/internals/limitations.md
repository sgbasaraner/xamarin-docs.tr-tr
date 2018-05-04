---
title: Xamarin.Android vs. Masaüstü - Mono çalışma zamanı farklılıkları
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: b1302bcf8d6835cac356d96b538d134891648420
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="limitations"></a>Sınırlamalar

Android uygulamaları oluşturma işlemi sırasında Java proxy türleri oluşturma gerekli olduğundan, çalışma zamanında tüm kod oluşturmak mümkün değil.

Bu Masaüstü Mono karşılaştırıldığında Xamarin.Android sınırlamalar vardır:


## <a name="limited-dynamic-language-support"></a>Sınırlı dinamik dil desteği

 [Android aranabilir sarmalayıcılar](~/android/platform/java-integration/android-callable-wrappers.md) Android çalışma zamanı gereken yönetilen kod çağırmak için her zaman gereklidir. Android aranabilir sarmalayıcılar derleme zamanında IL statik analizini göre oluşturulur. Net bunun sonucunda,:, *olamaz* bu dinamik türleri ayıklanıyor hiçbir şekilde olduğundan dinamik dil (IronPython, IronRuby, vb.) herhangi bir senaryoda sınıflara Java türleri (dolaylı sınıflara dahil), gerekli olduğu kullanma gerekli Android aranabilir sarmalayıcılar için derleme zamanında.


## <a name="limited-java-generation-support"></a>Sınırlı Java oluşturma desteği

[Android aranabilir sarmalayıcılar](~/android/platform/java-integration/android-callable-wrappers.md) yönetilen kod çağırmak Java kod sırada oluşturulması gerekir. *Varsayılan olarak*, Android aranabilir sarmalayıcılar (belirli) bildirilen oluşturucular ve sanal bir Java yöntemi geçersiz kılma yöntemleri yalnızca içerecek (yani sahip [ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) veya uygulayan bir Java arabirimi yöntemi ( benzer şekilde arabirimi olan `Attribute`).
  
4.1 sürümünden önce hiçbir ek yöntemleri bildirilen. 4.1 sürümüyle [ `Export` ve `ExportField` özel öznitelikler, Java yöntemleri hem de Android aranabilir sarmalayıcısı alanlarını bildirmek için kullanılabilir](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Eksik oluşturucular

Oluşturucular hassas, kalan sürece [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) kullanılır. Android aranabilir sarmalayıcısı oluşturucular oluşturmak için bir Java Oluşturucu varsa yayılan, algoritmasıdır:

1. Java eşleme tüm parametre türleri
2. Temel sınıfı aynı Oluşturucusu bildirir &ndash; bu gereklidir çünkü Android aranabilir sarmalayıcısı *gerekir* karşılık gelen taban sınıf oluşturucu çağrılamıyor; varsayılan bağımsız değişkenler (olmadığı için kolay bir yolu kullanılabilir değerleri olması gerektiğini belirlemek Java içinde kullanılan).

Örneğin, aşağıdaki sınıf göz önünde bulundurun:

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Bu görünümler mükemmel mantıksal elde edilen Android aranabilir sarmalayıcısı while *yayın derlemelerde* varsayılan bir oluşturucu içermiyor. Sonuç olarak, bu hizmet başlatmayı denerseniz (örneğin [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), başarısız olur:

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

Geçici bir çözüm değildir varsayılan bir oluşturucu bildirmek için kendisiyle ekleyin `ExportAttribute`ve [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```


### <a name="generic-c-classes"></a>Genel C# sınıflar

Genel C# sınıflar yalnızca kısmen desteklenir. Aşağıdaki sınırlamalar bulunmaktadır:


-   Genel türler kullanamazsınız `[Export]` veya `[ExportField`]. Bunu yapmak çalışırken oluşturacağını bir `XA4207` hata.

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

-   Genel yöntemler kullanamazsınız `[Export]` veya `[ExportField]`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

-   `[ExportField]` döndürmesi yöntemlere kullanılamaz `void`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

-   Genel türler örneklerini _bulunmamalıdır_ Java koddan oluşturulabilir.
    Yönetilen koddan yalnızca güvenli bir şekilde oluşturulabilir:

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```


## <a name="partial-java-generics-support"></a>Kısmi Java genel türler desteği

Genel türler bağlama desteği Java sınırlıdır. Özellikle, başka bir genel (örneği oluşturulan) sınıfından türetilen bir genel örneği sınıf üyeleri sol Java.Lang.Object sunulan. Örneğin, [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) yöntemi Java.Lang.Object döndürür. Silinen Java genel türler nedeni budur.
Bu sınırlama geçerli olmayan bazı sınıfları sahip olduğumuz ancak el ile ayarlanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android Çağrılabilir Sarmalayıcıları](~/android/platform/java-integration/android-callable-wrappers.md)
- [JNI ile çalışma](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
