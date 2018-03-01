---
title: Mimari
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DC22A08-808A-DC0C-B331-2794DD1F9229
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9579acc6c070bf692b0db1bd444a31c9ea4aa7ca
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="architecture"></a>Mimari

Xamarin.Android uygulamaları Mono yürütme ortamı içinde çalışır.
Bu yürütme ortamı çalıştırır yan yana Android çalışma zamanı (resim) sanal makine ile. Her iki çalışma zamanı ortamları üstünde Linux çekirdek çalıştırın ve temel sistem erişmek geliştiricilerin sağlar kullanıcı kodu için çeşitli API'leri. Mono çalışma zamanı C dilinde yazılır.

, Kullandığınız [sistem](http://msdn.microsoft.com/en-us/library/system.aspx), [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx), [System.Net](http://msdn.microsoft.com/en-us/library/system.net.aspx) ve .NET kalan sınıf kitaplıkları temel Linux işletim sistemi özellikleri erişmek için.

Android, ses, grafik, OpenGL ve telefon gibi sistem tesis çoğunu doğrudan yerel uygulamalar için kullanılabilir değildir, bunlar yalnızca birinde bulunan Android çalışma zamanı Java API'leri aracılığıyla sunulan [Java](https://developer.xamarin.com/api/namespace/Java.Lang/). * ad alanları veya [Android](https://developer.xamarin.com/api/namespace/Android/). * ad alanları. Mimari kabaca gibi şudur:

[![Mono ve diyagramı resim çekirdek üstüne ve altına .NET/Java + bağlamaları](architecture-images/architecture1.png)](architecture-images/architecture1.png)

Xamarin.Android geliştiriciler işletim sisteminde (düşük düzey erişimi için) .NET API'lerini bildikleri çağırma veya bir köprü tarafından sunulan Java API'ları sağlayan Android ad alanları açığa sınıflarını kullanarak çeşitli özelliklere erişim Android çalışma zamanı.

Android sınıfları Android çalışma zamanı sınıflarının nasıl iletişim kuracağını hakkında daha fazla bilgi için bkz: [API tasarım](~/android/internals/api-design.md) belge.

<a name="Application_Packages" />

## <a name="application-packages"></a>Uygulama paketleri

Android uygulama paketleri ile ZIP kapsayıcıları olan bir *.apk* dosya uzantısı. Xamarin.Android uygulama paketleri, aşağıdaki eklemelerle normal Android paketler aynı yapısı ve düzeni vardır:

-   (IL içeren) uygulama derlemeler *depolanan* içinde sıkıştırılmamış *derlemeleri* klasör. Sürümde başlatma işlemi sırasında derlemeler *.apk* olan *mmap()* işlemi ve derlemeler ed bellekten yüklü. Bu daha hızlı bir uygulama başlatma öncesinde yürütme ayıklanacak gerekmez derlemeleri olarak izin verir. - *Not:* derleme konumu bilgileri gibi [Assembly.Location](https://developer.xamarin.com/api/property/System.Reflection.Assembly.Location/) ve [Assembly.CodeBase](https://developer.xamarin.com/api/property/System.Reflection.Assembly.CodeBase/)
    *bağlı dayanıyordu olamaz* sürümdeki oluşturur. Farklı dosya sistemi girişleri olarak yok ve sahip oldukları kullanılabilir konum yok.


-   Mono çalışma zamanı içeren yerel kitaplıkları içinde mevcut *.apk* . Bir Xamarin.Android uygulaması yerel kitaplıkları istenen/hedeflenen Android mimari, örneğin içermelidir *pushservice* , *pushservice-v7a* , *x86* . Xamarin.Android uygulamaları uygun çalışma zamanı kitaplıkları içermediği sürece bir platformda çalıştırılamıyor.


Xamarin.Android uygulamaları da içeren *Android aranabilir sarmalayıcılar* yönetilen koda çağrı Android izin vermek için.


<a name="Android_Callable_Wrappers" />

## <a name="android-callable-wrappers"></a>Android aranabilir sarmalayıcılar

- **Android aranabilir sarmalayıcılar** olan bir [JNI](http://en.wikipedia.org/wiki/Java_Native_Interface) Android çalışma zamanı gereken yönetilen kod çağırmak için her zaman kullanılan köprüsü. Android aranabilir sarmalayıcılar olan nasıl sanal yöntemleri geçersiz kılınabilir ve Java arabirimleri uygulanabilir. Bkz: [Java tümleştirmesine genel bakış](~/android/platform/java-integration/index.md) daha fazla bilgi için belge.


<a name="Managed_Callable_Wrappers" />

## <a name="managed-callable-wrappers"></a>Yönetilen aranabilir sarmalayıcılar

Yönetilen aranabilir sarmalayıcılar yönetilen kod Android kodu çağırma ve sanal yöntemleri geçersiz kılma ve Java arabirimler uygulama için destek sağlamak için gereken her zaman kullanılan JNI Köprü ' dir. Tüm [Android](https://developer.xamarin.com/api/namespace/Android/). * ve ilgili ad alanlarını aracılığıyla oluşturulan yönetilen aranabilir sarmalayıcılar [.jar bağlama](~/android/platform/binding-java-library/index.md).
Yönetilen aranabilir sarmalayıcılar yönetilen ve Android türleri arasında dönüştürme ve JNI aracılığıyla temel Android platformu yöntemlerini çağırmaktan sorumlu.

Her yönetilen aranabilir sarmalayıcısı ayrı tutma aracılığıyla erişilebilen bir Java genel başvurusu oluşturulmuş [Android.Runtime.IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) özelliği. Genel başvuru Java örnekleri ve yönetilen örnekleri arasında eşleme sağlamak için kullanılır. Genel başvuru olan sınırlı bir kaynağa: Öykünücüler izin yalnızca 2000 Genel başvurular üzerinde 52,000 genel başvurular bir sırada mevcut çoğu donanım sağlar, ancak aynı anda mevcut.

Genel başvuru oluşturduğunuzda ve yok izlemek için ayarlayabileceğiniz [debug.mono.log](~/android/troubleshooting/index.md) sistem özelliğini içerecek şekilde [gref](~/android/troubleshooting/index.md).

Genel başvuru açıkça serbest çağırarak [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) yönetilen aranabilir sarmalayıcısı üzerinde. Bu Java örneği ve yönetilen örneği arasındaki eşlemeyi kaldırmak ve Java örneği toplanmasına izin verin. Java örneği yönetilen koddan yeniden erişiliyorsa, yeni bir yönetilen aranabilir sarmalayıcısı için oluşturulur.

Örnek yanlışlıkla örneği atma olarak iş parçacıkları arasında paylaşılabilir varsa aranabilir sarmalayıcılar yönetilen atma başka bir iş parçacığı başvurularından etkiler dikkatli olunması gerekir. En fazla koruması, yalnızca `Dispose()` aracılığıyla ayrılmış örneklerin `new` *veya* yöntemlerden, *bilmeniz* yeni örnekleri ve olabilen olmayan önbelleğe alınmış örnekleri her zaman ayırın iş parçacıkları arasında paylaşımı yanlışlıkla örneği neden.


<a name="Managed_Callable_Wrapper_Subclasses" />

## <a name="managed-callable-wrapper-subclasses"></a>Aranabilir sarmalayıcısı alt sınıfların yönetilen

Burada "ilginç" uygulamaya özgü mantığı Canlı yönetilen aranabilir sarmalayıcısı alt sınıflarıdır. Özel bunlar [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) alt sınıflar (gibi [Activity1](https://github.com/xamarin/monodroid-samples/blob/master/HelloM4A/Activity1.cs#L13) varsayılan proje şablonu yazın). (Tüm bunlar özellikle *Java.Lang.Object* yapmak alt sınıfların *değil* içeren bir [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/) özel öznitelik veya [ RegisterAttribute.DoNotGenerateAcw](https://developer.xamarin.com/api/property/Android.Runtime.RegisterAttribute.DoNotGenerateAcw/) olan *yanlış*, varsayılan değerdir.)

Yönetilen aranabilir sarmalayıcılar gibi yönetilen aranabilir sarmalayıcısı alt sınıfların Ayrıca, üzerinden erişilebilir genel bir başvuru içeren [Java.Lang.Object.Handle](https://developer.xamarin.com/api/property/Java.Lang.Object.Handle/) özelliği. Yalnızca yönetilen aranabilir sarmalayıcılar ile genel başvuruları açıkça çağırarak serbest bırakılabilirler gibi [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/).
Yönetilen aranabilir sarmalayıcılar aksine *çok dikkatli* olarak bu tür örnekleri atma önce alınıp alınmayacağını *Dispose()*örneğinin lık Java örneği arasında eşleme bozar (örneği bir Android aranabilir sarmalayıcısı) ve yönetilen örneği.

<a name="Java_Activation" />

### <a name="java-activation"></a>Java etkinleştirme

Zaman bir [Android aranabilir sarmalayıcısı](~/android/platform/java-integration/android-callable-wrappers.md) (ACW) Java'dan oluşturulur, ACW Oluşturucusu çağrılacak karşılık gelen C# Oluşturucusu neden olur. Örneğin, ACW için *MainActivity* çağıracağı bir varsayılan oluşturucu içerecek *MainActivity*ait varsayılan oluşturucu. (Bu yoluyla yapılır *TypeManager.Activate()* çağrısı içinde ACW oluşturucular.)

Bir diğer Oluşturucusu imzasını sonuç yok: *(IntPtr, JniHandleOwnership)* Oluşturucusu. *(IntPtr, JniHandleOwnership)* Oluşturucusu çağrılır ne zaman bir Java nesnesi yönetilen koda sunulur ve yönetilen aranabilir sarmalayıcısı JNI tutamacı yönetmek için oluşturulması gerekiyor. Bu genellikle otomatik olarak gerçekleştirilir.

İki senaryo vardır *(IntPtr, JniHandleOwnership)* Oluşturucusu bir yönetilen aranabilir sarmalayıcısı alt kümesi üzerinde el ile da sağlanmalıdır:

1. [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) alt. *Uygulama* olan özel; varsayılan *uygulama* Oluşturucusu olacak *hiçbir zaman* çağrılabilir ve [(IntPtr, JniHandleOwnership) Oluşturucusu yerine sağlanmalıdır ](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/SanityTests/Hello.cs#L105).

2. Bir temel sınıf oluşturucu çağrısından sanal yöntemi.


Unutmayın (2) sızıntılı bir soyutlamadır. Olduğu gibi C#, Java sanal yöntemlere yapılan çağrılar bir oluşturucusundan her zaman en çok türetilen yöntem uygulaması çağırır. Örneğin, [kutusu TextView (bağlamı, AttributeSet, int) Oluşturucusu](https://developer.xamarin.com/api/constructor/Android.Widget.TextView.TextView/p/Android.Content.Context/Android.Util.IAttributeSet/System.Int32/) sanal yöntemi çağırır [TextView.getDefaultMovementMethod()](http://developer.android.com/reference/android/widget/TextView.html#getDefaultMovementMethod()), olarak bağlı [ TextView.DefaultMovementMethod özelliği](https://developer.xamarin.com/api/property/Android.Widget.TextView.DefaultMovementMethod/).
Bu nedenle, bir tür değilse [LogTextBox](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs) (1) olan [alt kutusu TextView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L26), (2) [TextView.DefaultMovementMethod geçersiz kılma](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L45)ve (3) [bir örneğine etkinleştir XML aracılığıyla sınıfı](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Resources/layout/log_text_box_1.xml#L29) geçersiz kılınan *DefaultMovementMethod* ACW Oluşturucusu yürütme şansınız ve C# Oluşturucusu yürütme şansınız önce oluşacak önce özelliği'nin çağrılacak.

Bu örneği LogTextBox örneği tarafından desteklenen aracılığıyla [LogTextView (IntPtr, JniHandleOwnership)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L28) ACW LogTextBox örneği ilk girdiğinde Oluşturucusu yönetilen kod ve ardından çağırma [ LogTextBox (bağlamı, IAttributeSet, int)](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox.cs#L41) Oluşturucusu *aynı örneğinde* zaman ACW Oluşturucusu yürütür.

Olayların sırası:

1.  Düzen XML içine yüklenir bir [ContentView](https://github.com/xamarin/monodroid-samples/blob/f01b5c31/ApiDemo/Text/LogTextBox1.cs#L41).

2.  Android düzeni Nesne grafiği oluşturur ve bir örneğini başlatır *monodroid.apidemo.LogTextBox* , ACW için *LogTextBox* .

3.  *Monodroid.apidemo.LogTextBox* Oluşturucusu yürütülmeden [android.widget.TextView](http://developer.android.com/reference/android/widget/TextView.html#TextView%28android.content.Context,%20android.util.AttributeSet%29) Oluşturucusu.

4.  *Kutusu TextView* Oluşturucusu çağırır *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* .

5.  *monodroid.apidemo.LogTextBox.getDefaultMovementMethod()* invokes *LogTextBox.n_getDefaultMovementMethod()* , which invokes *TextView.n_GetDefaultMovementMethod()* , which invokes [Java.Lang.Object.GetObject&lt;TextView&gt; (handle, JniHandleOwnership.DoNotTransfer)](https://developer.xamarin.com/api/member/Java.Lang.Object.GetObject%7BT%7D/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) .

6.  *Java.Lang.Object.GetObject&lt;TextView&gt;()* checks to see if there is already a corresponding C# instance for *handle* . Varsa, döndürülür. Bu senaryoda, hiç, bunu *Object.GetObject&lt;T&gt;()* oluşturmanız gerekir.

7.  *Object.GetObject&lt;T&gt;()* arar *LogTextBox (IntPtr, JniHandleOwneship)* oluşturucusu, çağırılır, arasında bir eşleme oluşturur *işlemek* ve oluşturulan örneği ve oluşturulan örneğini döndürür.

8.  *TextView.n_GetDefaultMovementMethod()* çağırır *LogTextBox.DefaultMovementMethod* özellik Alıcısı.

9.  Denetim döner *android.widget.TextView* yürütme sona erdikten Oluşturucusu.

10. *Monodroid.apidemo.LogTextBox* Oluşturucusu yürütülmeden, çağırma *TypeManager.Activate()* .

11. *LogTextBox (bağlamı, IAttributeSet, int)* Oluşturucusu yürütülmeden *(7)'de oluşturulan aynı örnek üzerinde* .

12. ...


Varsa (IntPtr, JniHandleOwnership) Oluşturucusu bulunamıyor, sonra bir [System.MissingMethodException](https://developer.xamarin.com/api/type/System.MissingMethodException/) oluşturulur.


<a name="Premature_Dispose_Calls" />

### <a name="premature-dispose-calls"></a>Erken Dispose() çağrıları

JNI tanıtıcısı ve karşılık gelen C# örnek arasında bir eşleme yoktur. Bu eşleme Java.Lang.Object.Dispose() keser. Eşleme bozulmuş sonra JNI tanıtıcı yönetilen kod girerse, Java etkinleştirme gibi görünüyor ve *(IntPtr, JniHandleOwnership)* Oluşturucu denetlediği ve çağrılır. Oluşturucu yoksa, bir özel durum atılır.

Örneğin, aşağıdaki yönetilen aranabilir Wraper alt verilen:

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

Örnek, bunu Dispose() oluşturmak ve yönetilen aranabilir yeniden oluşturulması sarmalayıcı neden varsa:

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

Bir alt içeriyorsa, bir *(IntPtr, JniHandleOwnership)* oluşturucusu, sonra bir *yeni* türünün örneği oluşturulur. Sonuç olarak, yeni bir örnek olarak "tüm örnek veri kaybı için" örnek görünür. (Değer null olduğunu unutmayın.)

```shell
I/mono-stdout( 2993): [Managed: Value=]
```

Yalnızca *Dispose()* , aranabilir sarmalayıcısı alt sınıfların Java nesne artık kullanılmayacak veya bir alt hiçbir örnek verilerini içeren bildiğinizde ve yönetilen *(IntPtr, JniHandleOwnership)* Oluşturucu sağlanmadı.


<a name="Application_Startup" />

## <a name="application-startup"></a>Uygulama başlatma

Bir etkinliği, hizmet, vb., Android ilk denetleyecek zaten etkinlik/service/vb. barındırmak için çalışan bir işlem olup olmadığını için başlatılır. Yeni bir işlem oluşturulacak, böyle bir işlem, varsa [AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-intro.html) okuma ve belirtilen türü [ /manifest/application/@android:name ](http://developer.android.com/guide/topics/manifest/application-element.html#nm) özniteliği yüklenen ve örneği. Sonraki, tarafından belirtilen tüm türleri [ /manifest/application/provider/@android:name ](http://developer.android.com/guide/topics/manifest/provider-element.html#nm) öznitelik değerleri oluşturulur ve sahip kendi [ContentProvider.attachInfo%28)](https://developer.xamarin.com/api/member/Android.Content.ContentProvider.AttachInfo/p/Android.Content.Context/Android.Content.PM.ProviderInfo/) yöntemi çağrılır. Bu içine Xamarin.Android kancaları ekleyerek bir *mono. MonoRuntimeProvider* *ContentProvider* oluşturma işlemi sırasında AndroidManifest.xml için. The *mono.MonoRuntimeProvider.attachInfo()* method is responsible for loading the Mono runtime into the process.
Mono önce bu noktası kullanma girişimleri başarısız olur. ( *Not*: nedeni budur hangi alt türleri [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/) vermesi gereken bir [(IntPtr, JniHandleOwnership) Oluşturucusu](https://github.com/xamarin/monodroid-samples/blob/a9e8ef23/SanityTests/Hello.cs#L103), Uygulama örneğinin olarak Mono başlatılabilir önce oluşturulan.)

İşlem başlatma tamamlandıktan sonra `AndroidManifest.xml` etkinlik hizmeti başlatmak için vb. sınıf adını bulmak için alınmadığında. Örneğin, [ /manifest/application/activity/@android:name özniteliği](http://developer.android.com/guide/topics/manifest/activity-element.html#nm) yüklemek için bir etkinliğin adını belirlemek için kullanılır. Bu tür etkinlikler için devralmalıdır [android.app.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/).
Belirtilen tür aracılığıyla yüklenen [Class.forName()](http://developer.android.com/reference/java/lang/Class.html#forName(java.lang.String)) (türü bir Java olmasını gerektirir türü, bu nedenle Android aranabilir sarmalayıcılar), ardından örneği. Bir Android aranabilir sarmalayıcısı örneğinin oluşturulmasını karşılık gelen C# türünün bir örneği oluşturulmasını tetikler. Android sonra çağırılır [Activity.onCreate(Bundle)](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) , karşılık gelen neden olacak [Activity.OnCreate(Bundle)](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) çağrılacak, ve diğerleriyle kapalı demektir.
