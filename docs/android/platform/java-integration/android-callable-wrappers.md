---
title: Android aranabilir sarmalayıcılar
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: bb15c7f3a36cc7f1ed235d92e343bbae67a718b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764566"
---
# <a name="android-callable-wrappers"></a>Android aranabilir sarmalayıcılar

Yönetilen kod Android çalışma zamanı çağırır her android aranabilir sarmalayıcılar (ACWs) gereklidir. Çalışma zamanında resimleri (Android çalışma zamanı) sınıfları kaydetmek için hiçbir şekilde olduğundan bu sarmalayıcıları gereklidir. (Özellikle [JNI DefineClass() işlevi](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) Android çalışma zamanı tarafından desteklenmiyor.} Android aranabilir sarmalayıcılar böylece çalışma zamanı türü kayıt desteğinin olmaması için oluşturur. 

*Her zaman* Android kod yürütmek için gereksinim duyduğu bir `virtual` veya arabirim yöntemini `overridden` veya bu yöntem için uygun yönetilen türü gönderilir, böylece yönetilen kodda uygulanır, Xamarin.Android Java proxy sağlamanız gerekir. Bu Java proxy "aynı" temel sınıf ve Java arabirim listesi aynı oluşturucular uygulama ve tüm geçersiz kılınmış temel sınıf ve arabirim yöntemleri bildirme yönetilen türünde Java kod türleridir. 

Android aranabilir sarmalayıcılar tarafından üretilen **monodroid.exe** sırasında program [derleme işlemi](~/android/deploy-test/building-apps/build-process.md): (doğrudan veya dolaylı olarak) devralan tüm türleri için oluşturulan [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/). 



## <a name="android-callable-wrapper-naming"></a>Android aranabilir sarmalayıcısı adlandırma

Android aranabilir sarmalayıcılar için paket adları, dışarı aktarılan türü bütünleştirilmiş kod tam adının MD5SUM üzerinde temel alır. Bu adlandırma teknik tarafından farklı derlemeleri paketleme hata oluşturmaksızın kullanılabilir duruma getirilmek üzere aynı tam olarak nitelenmiş tür adını mümkün hale getirir. 

Bu MD5SUM adlandırma şeması nedeniyle, ada göre türlerinizi doğrudan erişemez. Örneğin, aşağıdaki `adb` olduğundan komut çalışmaz tür adı `my.ActivityType` varsayılan olarak oluşturulan değil: 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

Ayrıca, ada göre bir türe başvuruda denerseniz aşağıdaki gibi hatalar görebilirsiniz:

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Varsa, *yapmak* erişmesi ada göre türleri için bu tür bir öznitelik bildirimi için bir ad bildirebilirsiniz. Örneğin, tam adıyla bir etkinliği bildiren kod işte `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

`ActivityAttribute.Name` Özelliği, bu etkinliğin adını açıkça bildirmek için ayarlanabilir: 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Bu özellik ayarı eklendikten sonra `my.ActivityType` harici kod'ndan ve ad tarafından erişilen `adb` komut dosyaları. `Name` Özniteliği de dahil olmak üzere birçok farklı türleri için ayarlanabilir `Activity`, `Application`, `Service`, `BroadcastReceiver`, ve `ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

MD5SUM tabanlı ACW adlandırma Xamarin.Android 5. 0'sunulmuştur. Öznitelik adlandırma hakkında daha fazla bilgi için bkz: [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/). 



## <a name="implementing-interfaces"></a>Arabirimler uygulama

Ne zaman ihtiyacınız olabilecek Android bir arabirim gibi uygulamak için zamanlar [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/). Tüm Android sınıfları ve arabirim genişletmek beri [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) arabirimi, sorunun ortaya çıkar: nasıl biz uygulamak `IJavaObject`? 

Soru yukarıda yanıtlandı: tüm Android türleri neden gerekiyor uygulamak `IJavaObject` , böylelikle Android, yani belirtilen tür için Java proxy sağlamak için bir Android aranabilir sarmalayıcısı Xamarin.Android sahiptir. Bu yana **monodroid.exe** yalnızca arar `Java.Lang.Object` alt sınıfların, ve `Java.Lang.Object` uygulayan `IJavaObject,` yanıt açıktır: subclass `Java.Lang.Object`: 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```


## <a name="implementation-details"></a>Uygulama Ayrıntıları

*Bu sayfanın kalanını bildirilmeksizin uygulama ayrıntılarını sağlar* (ve yalnızca geliştiriciler neler olup bittiğini hakkında merak olacağı için Burada sunulan). 

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

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Taban sınıfı korunduğunu dikkat edin ve `native` yöntem bildirimleri, yönetilen kod içinde geçersiz kılınır her bir yöntemin sağlanır. 
