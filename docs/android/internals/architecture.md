---
title: Mimari
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: e6a30247c13deab871bf230aba53b9963981fd02
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997406"
---
# <a name="architecture"></a>Mimari

Xamarin.Android uygulamaları Mono yürütme ortamı içinde çalışır.
Bu yürütme ortamı çalıştırır-yan Android çalışma zamanı (resim) sanal makine ile. Her iki çalışma zamanı ortamlarını Linux çekirdeğinin üzerinde çalıştırın ve sayesinde geliştiriciler, temel alınan sisteme erişmek için kullanıcı kodu için çeşitli API'ler üzerinden kullanıma sunacaksınız. Mono çalışma zamanı, C dilinde yazılır.

, Kullandığınız [sistem](xref:System), [System.IO](xref:System.IO), [System.Net](xref:System.Net) ve rest, .NET sınıf kitaplıkları, temel alınan Linux işletim sistemi özellikleri erişmek için.

Android, ses, grafik, OpenGL ve telefon gibi sistem özelliklerini çoğunu doğrudan yerel uygulamalar için kullanılabilir değil, bunlar yalnızca birinde bulunan Android çalışma zamanı Java API'leri aracılığıyla sunulan [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * ad alanları veya [Android](https://developer.xamarin.com/api/namespace/Android/). * ad alanları. Mimari kabaca şu şekilde verilmiştir:

[![Çekirdek üzerinde ve .NET/Java + bağlamaları altında diyagram Mono ve resimler](architecture-images/architecture1.png)](architecture-images/architecture1.png#lightbox)

Xamarin.Android geliştiricilerine işletim sisteminde .NET API alışık oldukları çağırma (düşük düzey erişimi için) veya bir köprü tarafından kullanıma sunulan Java API'leri sağlayan Android alanlarında ortaya konan sınıflar kullanarak çeşitli özelliklerine erişim Android çalışma zamanı.

Android sınıf Android çalışma zamanı sınıflar ile nasıl iletişim kuracağını hakkında daha fazla bilgi için bkz. [API tasarımı](~/android/internals/api-design.md) belge.


## <a name="application-packages"></a>Uygulama paketleri

Android uygulaması paketlerin ZIP kapsayıcılarla bir *.apk* dosya uzantısı. Xamarin.Android uygulama paketleri, aynı yapıya ve düzeni aşağıdaki eklemelerle normal Android paketler vardır:

-   (IL içeren) uygulama derlemeleri *depolanan* içinde sıkıştırılmamış *derlemeleri* klasör. Sürümündeki başlatma işlemi sırasında oluşturur *.apk* olan *mmap()* ed işlemi ve derlemeleri bellekten yüklendi. Derlemeleri önce yürütme ayıklanmasını gerekmez gibi bu daha hızlı uygulama başlatma izin verir. - *Not:* derleme konumu bilgileri gibi [Assembly.Location](xref:System.Reflection.Assembly.Location) ve [Assembly.CodeBase](xref:System.Reflection.Assembly.CodeBase)
    *bağlı yararlandı olamaz* sürümde oluşturur. Farklı dosya sistemi girişleri olarak bulunmaz ve sahip oldukları kullanılabilir konum yok.


-   Mono çalışma zamanı içeren yerel kitaplıkları içinde mevcut *.apk* . Bir Xamarin.Android uygulaması yerel kitaplıkları istenen/hedeflenen Android mimariler için örneğin içermelidir *armeabi* , *armeabi v7a* , *x86* . Xamarin.Android uygulamaları, uygun çalışma zamanı kitaplıkları içerdiği sürece bir platformda çalışmaz.


Xamarin.Android uygulamaları da içeren *Android çağrılabilir sarmalayıcıları* Android yönetilen koda çağrı yapmak izin vermek için.



## <a name="android-callable-wrappers"></a>Android çağrılabilir sarmalayıcıları

- **Android çağrılabilir sarmalayıcıları** olan bir [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) dilediğiniz zaman Android çalışma zamanı gereken yönetilen kodu çağırmak için kullanılan köprüsü. Android çağrılabilir sarmalayıcıları olan nasıl sanal yöntemleri geçersiz kılınabilir ve Java arabirimleri uygulanabilir. Bkz: [Java tümleştirmesine genel bakış](~/android/platform/java-integration/index.md) doc daha fazla bilgi için.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Yönetilen aranabilir sarmalayıcılar

Yönetilen aranabilir sarmalayıcılarını yönetilen kod Android kodu çağırabilir ve sanal yöntemleri geçersiz kılan ve Java arabirimleri uygulama için destek sağlamak için gereken her zaman kullanılan JNI Köprüsü ' dir. Tüm [Android](https://developer.xamarin.com/api/namespace/Android/). * ve ilgili ad alanları aracılığıyla oluşturulan yönetilen aranabilir sarmalayıcılarını [.jar bağlama](~/android/platform/binding-java-library/index.md).
Yönetilen aranabilir sarmalayıcılar, yönetilen ve Android türleri arasında dönüştürme ve JNI ile temel alınan Android platformu yöntemlerini çağırmaktan sorumludur.

Her yönetilen çağrılabilir sarmalayıcı ayrı tutma aracılığıyla erişilebilir bir Java genel başvurusu oluşturulmuş [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) özelliği. Genel başvuru Java örnekleri ve yönetilen örnekleri arasındaki eşlemeyi sağlamak için kullanılır. Genel başvurulardır sınırlı kaynak: öykünücüleri izin yalnızca 2000 Genel başvurular üzerinde 52,000 genel başvurular bir sırada mevcut çoğu donanım sağlar, ancak bir zamanda mevcut.

Genel başvuru oluşturulur ve imha ne zaman açtıklarını izlemek için ayarlayabileceğiniz [debug.mono.log](~/android/troubleshooting/index.md) sistem özelliğini içerecek şekilde [gref](~/android/troubleshooting/index.md).

Genel başvuru açıkça serbest çağırarak [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) üzerinde yönetilen çağrılabilir sarmalayıcı. Yönetilen örnek ile Java örnek arasındaki eşlemeyi kaldırmak ve toplanacak Java örnek izin. Java örnek yönetilen koddan yeniden erişilirse, yeni bir yönetilen çağrılabilir sarmalayıcı için oluşturulur.

Örnek yanlışlıkla örneği disposing olarak iş parçacıkları arasında paylaşılabilir varsa aranabilir sarmalayıcılar, yönetilen disposing başvurularından diğer iş parçacıklarını etkiler dikkatli olunması gerekir. En yüksek güvenliği, yalnızca `Dispose()` aracılığıyla ayrılmış örneklerin `new` *veya* yöntemlerinden, *bilmeniz* yeni örnekleri ve olabilen önbelleğe alınmış örnekleri her zaman ayırın iş parçacıkları arasında paylaşımı yanlışlıkla örneği neden.



## <a name="managed-callable-wrapper-subclasses"></a>Yönetilen çağrılabilir sarmalayıcı alt sınıfları

Yönetilen çağrılabilir sarmalayıcı alt tüm "ilginç" uygulamaya özgü mantık burada yaşayın sınıflarıdır. Bunlar özel [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) alt sınıfları (gibi [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) varsayılan proje şablonu türü). (Tüm bunlar özellikle *Java.Lang.Object* bunu kılabileceği *değil* içeren bir [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) özel öznitelik veya [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) olduğu *false*, varsayılan değerdir.)

Yönetilen aranabilir sarmalayıcılarını gibi yönetilen çağrılabilir sarmalayıcı alt sınıfları Ayrıca, erişilebilir genel bir başvuru içeren [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) özelliği. Yalnızca yönetilen aranabilir sarmalayıcılarını ile genel başvuruları açıkça çağrılarak serbest bırakılabileceğini gibi [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
Yönetilen aranabilir sarmalayıcılarını aksine *çok dikkatli* olarak böyle örneklerini disposing önce alınması gereken *Dispose()* ing örneğinin Java örneği arasındaki eşlemeyi bozar (örneği bir Android çağrılabilir sarmalayıcısı) ve yönetilen örnek.


### <a name="java-activation"></a>Java etkinleştirme

Olduğunda bir [Android çağrılabilir sarmalayıcı](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) Java'dan oluşturulur, ACW Oluşturucusu çağrılacak karşılık gelen C# Oluşturucu neden olur. Örneğin, ACW için *MainActivity* hizmetinizde bir varsayılan oluşturucu içerecek *MainActivity*ait varsayılan oluşturucu. (Bu yoluyla yapılır *TypeManager.Activate()* ACW oluşturucular içinde arayın.)

Sonucu başka bir yapıcı imzası yok: *(IntPtr, JniHandleOwnership)* Oluşturucusu. *(IntPtr, JniHandleOwnership)* Java nesne yönetilen kod için kullanıma sunulmuştur ve yönetilen çağrılabilir sarmalayıcı JNI tanıtıcı yönetmek için oluşturulması gereken her Oluşturucu çağrılır. Bu genellikle otomatik olarak gerçekleştirilir.

Burada iki senaryo vardır *(IntPtr, JniHandleOwnership)* oluşturucu üzerinde yönetilen çağrılabilir sarmalayıcı alt el ile da sağlanmalıdır:

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) alt. *Uygulama* olan özel; varsayılan *uygulama* Oluşturucusu olacak *hiçbir zaman* çağrılabilir ve [(IntPtr, JniHandleOwnership) Oluşturucusu yerine sağlanmalıdır ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Sanal bir yöntem çağrısından bir temel sınıf oluşturucusu.


Unutmayın (2) sızıntılı bir soyutlamadır. Olduğu gibi C#, Java sanal yöntemlere yapılan çağrılar bir oluşturucudan her zaman en türetilen uygulamaya yöntemi çağırın. Örneğin, [TextView (bağlam, AttributeSet, int) Oluşturucusu](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) sanal yöntemi çağırır [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), olarak bağlı [ TextView.DefaultMovementMethod özelliği](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
Bu nedenle, bir tür ederse [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) (1) olan [alt TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [TextView.DefaultMovementMethod geçersiz kılma](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)ve (3) [bir örneğine etkinleştir sınıfı, XML aracılığıyla](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) geçersiz kılınan *DefaultMovementMethod* ACW Oluşturucusu yürütülecek bir şansınız ve C# Oluşturucusu önce yürütülecek bir fırsat ortaya çıkabilecek önce özelliği'nin çağrılacak.

Bu örneği LogTextBox örneği tarafından desteklenen aracılığıyla [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) ACW LogTextBox örneği ilk girdiğinde Oluşturucu yönetilen kod ve daha sonra çağırma [ (Bağlam, IAttributeSet, int) LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) Oluşturucusu *aynı örneğinde* ACW Oluşturucu ne zaman yürütür.

Olayların sırası:

1.  Düzen XML olarak yüklendiği bir [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android düzeni Nesne grafiği oluşturur ve bir örneğini başlatır *monodroid.apidemo.LogTextBox* , için ACW *LogTextBox* .

3.  *Monodroid.apidemo.LogTextBox* Oluşturucusu yürütülmeden [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) Oluşturucusu.

4.  *TextView* oluşturucu çağırır *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* çağırır *LogTextBox.n_getDefaultMovementMethod()* , hangi çağırır *TextView.n_GetDefaultMovementMethod()* , hangi çağırır [Java.Lang.Object.GetObject&lt;TextView&gt; (işlemek, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* zaten karşılık gelen C# olup olmadığını kontrol eder örneği için *işlemek* . Varsa, döndürülür. Bu senaryoda, mevcut değildir, bu nedenle *Object.GetObject&lt;T&gt;()* oluşturmanız gerekir.

7.  *Object.GetObject&lt;T&gt;()* arar *LogTextBox (IntPtr, JniHandleOwneship)* oluşturucusu, onu çağırır, arasındaki eşlemeyi oluşturur *işlemek* ve oluşturulan örneği ve oluşturulan örneği döndürür.

8.  *TextView.n_GetDefaultMovementMethod()* çağırır *LogTextBox.DefaultMovementMethod* özellik Alıcısı.

9.  Denetim döner *android.widget.TextView* yürütme sona erdikten Oluşturucu.

10. *Monodroid.apidemo.LogTextBox* Oluşturucusu yürütülmeden, çağırma *TypeManager.Activate()* .

11. *LogTextBox (bağlam, IAttributeSet, int)* Oluşturucusu yürütülmeden *(7)'de oluşturulan aynı örneğinde* .

12. Varsa (IntPtr, JniHandleOwnership) Oluşturucu bulunamıyor ardından bir System.MissingMethodException](xref:System.MissingMethodException) oluşturulur.

<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Erken Dispose() çağrıları

JNI tanıtıcısı ve karşılık gelen C# örneği arasında bir eşleme yoktur. Bu eşleme Java.Lang.Object.Dispose() keser. JNI işleyici eşlemesi bozulmuş sonra yönetilen kod girerse, Java etkinleştirme gibi görünüyor ve *(IntPtr, JniHandleOwnership)* oluşturucusu için işaretli ve çağrılır. Bir oluşturucu yoksa, bir özel durum oluşturulur.

Örneğin, aşağıdaki yönetilen çağrılabilir Wraper alt verilen:

```csharp
class ManagedValue : Java.Lang.Object {

    public string Value {get; private set;}

    public ManagedValue (string value)
    {
        Value = value;
    }

    public override string ToString ()
    {
        return string.Format ("[Managed: Value={0}]", Value);
    }
}
```

Dispose() bunu bir örnek oluşturun ve yönetilen çağrılabilir yeniden oluşturulması sarmalayıcı neden olursa:

```csharp
var list = new JavaList<IJavaObject>();
list.Add (new ManagedValue ("value"));
list [0].Dispose ();
Console.WriteLine (list [0].ToString ());
```

Program öldürmüş:

```shell
E/mono    ( 2906): Unhandled Exception: System.NotSupportedException: Unable to activate instance of type Scratch.PrematureDispose.ManagedValue from native handle 4051c8c8 --->
System.MissingMethodException: No constructor found for Scratch.PrematureDispose.ManagedValue::.ctor(System.IntPtr, Android.Runtime.JniHandleOwnership)
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateProxy (System.Type type, IntPtr handle, JniHandleOwnership transfer) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   --- End of inner exception stack trace ---
E/mono    ( 2906):   at Java.Interop.TypeManager.CreateInstance (IntPtr handle, JniHandleOwnership transfer, System.Type targetType) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object.GetObject (IntPtr handle, JniHandleOwnership transfer, System.Type type) [0x00000] in <filename unknown>:0
E/mono    ( 2906):   at Java.Lang.Object._GetObject[IJavaObject] (IntPtr handle, JniHandleOwnership transfer) [0x00000
```

Alt içermiyorsa, bir *(IntPtr, JniHandleOwnership)* oluşturucusu, ardından bir *yeni* türünün örneği oluşturulur. Sonuç olarak, yeni bir örneği olduğu gibi "tüm örnek veri kaybetme için" örneği görünür. (Değer null olduğunu unutmayın.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Yalnızca *Dispose()* , aranabilir sarmalayıcısı kılabileceği Java nesne artık kullanılmayacak veya alt hiçbir örnek verilerinin bulunduğu bildiğinizde ve yönetilen *(IntPtr, JniHandleOwnership)* Oluşturucu sağlamıştır.



## <a name="application-startup"></a>Uygulama başlatma

Bir etkinlik, hizmet, vb. Android ilk denetleyecek zaten etkinlik/service/vb. barındırmak için çalışan bir işlem olup olmadığını görmek için başlatılır. Yeni bir işlem oluşturulur ardından, böyle bir işlem varsa, [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) okuma ve belirtilen türü [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) özniteliği yüklendi ve örneği. Ardından, tüm türleri tarafından belirtilen [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) öznitelik değerleri örneği oluşturulur ve sahip kullanıcıların [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) yöntemi çağrılır. Xamarin.Android kancaları'ekleyerek bu içine bir *mono. MonoRuntimeProvider* *ContentProvider* AndroidManifest.xml derleme işlemi sırasında için. *Mono. MonoRuntimeProvider.attachInfo()* yöntemi, Mono çalışma zamanını işleme yükleme için sorumludur.
Mono önceki bu noktaya kullanma girişimleri başarısız olur. ( *Not*: nedeni budur hangi alt türleri [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) sağlamanız gereken bir [(IntPtr, JniHandleOwnership) Oluşturucusu](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), uygulama örneği olarak Mono başlatılabilir önce oluşturuldu.)

Başlatma işlemi tamamlandıktan sonra `AndroidManifest.xml` etkinlik/service/başlatmak için vb. sınıf adını bulmak için alınmadığında. Örneğin, [ /manifest/application/activity/@android:name özniteliği](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) yüklemek için bir etkinliğin adını belirlemek için kullanılır. Bu tür etkinlikler için devralmalıdır [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Belirtilen tür aracılığıyla yüklenen [Class.forName()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (türün bir Java olmasını gerektirir türü, bu nedenle Android çağrılabilir sarmalayıcıları), ardından örneği. Android çağrılabilir sarmalayıcı örneğinin oluşturulmasını karşılık gelen C# türü bir örneğinin oluşturulmasını tetikler. Android sonra çağırılır [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , karşılık gelen neden olacak [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) çağrılacak, ve yarışa hazır hale kapalı gelirsiniz.
