---
title: Özel durum Xamarin.iOS içinde hazırlama
description: Bu belgede bir Xamarin.iOS uygulaması yerel ve yönetilen özel durumları ile nasıl çalışılacağını açıklar. Oluşabilen sorunlar ve bu sorunları için bir çözüm anlatılmaktadır.
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/05/2017
ms.openlocfilehash: dcf1074aacb6d139d107dac01fa86f459831d5f9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786749"
---
# <a name="exception-marshaling-in-xamarinios"></a>Özel durum Xamarin.iOS içinde hazırlama

_Xamarin.iOS özellikle yerel kodda özel durumlara yanıt vermek için yeni olayları içerir._

Yönetilen kod ve Objective-C çalışma zamanı özel durumları (try/catch/finally tümceleri) için destek vardır.

Ancak, kendi özel durumları işleme ve başka dillerde yazılan kodu çalıştırmak olduğunda çalışma zamanı kitaplıkları (Mono çalışma zamanı ve Objective-C çalışma zamanı kitaplıkları) sorunları olduğu anlamına gelir, farklı uygulamalarıdır.

Bu belge, oluşabilecek sorunları ve olası çözümleri açıklar.

Ayrıca bir örnek proje içerir [özel durum hazırlama](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling), farklı senaryolar ve bunların çözümleri test etmek için kullanılabilir.

## <a name="problem"></a>Sorun

Bir özel durum atılır ve yığını bir çerçeve geriye doğru izleme sırasında karşılaşılan oluşturulan özel durumun türünü eşleşmeyen sorun olduğunda oluşur.

Bu, tipik bir örnek Xamarin.iOS veya Xamarin.Mac için yerel bir API Objective-C özel durum oluşturur ve yığın unwinding işlem yönetilen bir çerçeve ulaştığında sonra o Objective-C özel durum şekilde ele alınması gerekir durumdur.

Hiçbir şey yapmak için varsayılan eylemdir. Yukarıdaki örnek için bu Objective-C çalışma zamanı yönetilen bırakma çerçeveler izin vererek anlamına gelir. Objective-C çalışma zamanı yönetilen çerçeveler bırakma nasıl bilmediğinden sorunlu olmasa da budur; Örneğin, herhangi bir yürütme olmaz `catch` veya `finally` çerçevenin yan tümcelerinde.

### <a name="broken-code"></a>Bozuk kodu

Aşağıdaki kod örneği göz önünde bulundurun:

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

Bu bir Objective-C NSInvalidArgumentException yerel kodda özel durum oluşturacak:

    NSInvalidArgumentException *** setObjectForKey: key cannot be nil

Ve yığın izleme şöyle olacaktır:

    0   CoreFoundation          __exceptionPreprocess + 194
    1   libobjc.A.dylib         objc_exception_throw + 52
    2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
    3   libobjc.A.dylib         objc_msgSend + 102
    4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
    5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
    6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()

Çerçeve 0-3: yerel çerçeveler ve Objective-C çalışma zamanında yığın unwinder _yapabilirsiniz_ çerçeveleri bırakma. Özellikle, Objective-C yürütecek `@catch` veya `@finally` yan tümceleri.

Ancak, Objective-C yığın unwinder olan _değil_ düzgün yönetilen çerçeveler (çerçeveler 4-6), çerçeve unwound olacaktır, ancak yönetilen özel durum mantığı çalıştırılmayacak, geriye doğru izleme yeteneğine.

Bu, genellikle aşağıdaki şekilde bu özel durumları yakalamak mümkün olmadığı anlamına gelir:

```csharp
try {
    var dict = new NSMutableDictionary ();
    dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero);
} catch (Exception ex) {
    Console.WriteLine (ex);
} finally {
    Console.WriteLine ("finally");
}
```

Objective-C yığın unwinder yönetilen hakkında bilmez olmasıdır `catch` yan tümcesi ve hiçbiri `finally` yan tümcesi yürütülebilir.

Zaman Yukarıdaki kod örneği _olan_ Objective-C işlenmemiş Objective-C özel durumları, bildirilmesini yöntemi olduğundan, bu etkilidir [`NSSetUncaughtExceptionHandler`][2], hangi Xamarin.iOS ve Xamarin.Mac kullanın ve bu noktada Objective-C özel durumlar için yönetilen özel durumlar dönüştürmek için çalışır.

## <a name="scenarios"></a>Senaryolar

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>Senaryo 1 - bir yönetilen catch işleyici ile Objective-C özel durumları yakalama

Aşağıdaki senaryoda, Objective-C özel durumları yakalamak olası yönetilen kullanarak `catch` işleyicileri:

1. Objective-C özel durum oluşur.
2. Objective-C çalışma zamanı yığın anlatılmaktadır (ancak bu bırakma değil), yerel için arayan `@catch` özel durumu işleyebilecek işleyici.
3. Objective-C çalışma zamanı herhangi bulamadığı `@catch` işleyicileri, çağrıları `NSGetUncaughtExceptionHandler`ve Xamarin.iOS/Xamarin.Mac tarafından yüklenen işleyiciyi çağırır.
4. Xamarin.iOS/Xamarin.Mac's işleyici yönetilen bir özel durum Objective-C özel durumu dönüştürün ve bu durum. Bu yana Objective-C çalışma zamanı (bunu gitti yalnızca) yığın bırakma alamadık, geçerli çerçeve aynı nerede Objective-C özel durum oluştu.

Mono çalışma zamanının nasıl Objective-C çerçeveler düzgün bırakma bilmediğinden Burada, başka bir sorun oluşur.

Xamarin.iOS yakalanmayan Objective-C özel durumu geri çağrıldığında yığını bu gibi.

     0 libxamarin-debug.dylib   exception_handler(exc=name: "NSInvalidArgumentException" - reason: "*** setObjectForKey: key cannot be nil")
     1 CoreFoundation           __handleUncaughtException + 809
     2 libobjc.A.dylib          _objc_terminate() + 100
     3 libc++abi.dylib          std::__terminate(void (*)()) + 14
     4 libc++abi.dylib          __cxa_throw + 122
     5 libobjc.A.dylib          objc_exception_throw + 337
     6 CoreFoundation           -[__NSDictionaryM setObject:forKey:] + 1015
     7 libxamarin-debug.dylib   xamarin_dyn_objc_msgSend + 102
     8 TestApp                  ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
     9 TestApp                  Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr) [0x00000]
    10 TestApp                  ExceptionMarshaling.Exceptions.ThrowObjectiveCException () [0x00013]

Burada, yalnızca yönetilen çerçevelerini çerçeveler 8-10, ancak bu 0 çerçevede yönetilen özel durum oluşur. Mono çalışma zamanı yerel çerçeveler 0-7, yukarıda açıklanan sorun için eşdeğer bir soruna neden olan bırakma gerekir, yani: yerel çerçeveler Mono çalışma zamanı bırakma ancak tüm Objective-C yürütme olmaz `@catch` veya `@finally` yan tümceleri .

Kod örneği:

```objc
-(id) setObject: (id) object forKey: (id) key
{
    @try {
        if (key == nil)
            [NSException raise: @"NSInvalidArgumentException"];
    } @finally {
        NSLog (@"This will not be executed");
    }
}
```

Ve `@finally` bu çerçeve unwinds Mono çalışma zamanı hakkında bilmediğinden yan tümcesi çalıştırılmayacak.

Bu bir çeşitlemesi yönetilen kod ve almak için yerel çerçeveler geriye doğru izleme yönetilen bir özel durum oluşturmaktır ilk yönetilen `catch` yan tümcesi:

```csharp
class AppDelegate : UIApplicationDelegate {
    public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
    {
        throw new Exception ("An exception");
    }
    static void Main (string [] args)
    {
        try {
            UIApplication.Main (args, null, typeof (AppDelegate));
        } catch (Exception ex) {
            Console.WriteLine ("Managed exception caught.");
        }
    }
}
```

Yönetilen `UIApplication:Main` yöntemi yerel çağıracaktır `UIApplicationMain` yöntemi ve iOS sonunda yönetilen çağırmadan önce yerel kod yürütme çok yeterlidir `AppDelegate:FinishedLaunching` hala yerel çerçeveler yönetilen özel durum olduğunda yığında birçok yöntemi Durum:

     0: TestApp                 ExceptionMarshaling.IOS.AppDelegate:FinishedLaunching (UIKit.UIApplication,Foundation.NSDictionary)
     1: TestApp                 (wrapper runtime-invoke) <Module>:runtime_invoke_bool__this___object_object (object,intptr,intptr,intptr) 
     2: libmonosgen-2.0.dylib   mono_jit_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
     3: libmonosgen-2.0.dylib   do_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
     4: libmonosgen-2.0.dylib   mono_runtime_invoke [inlined] mono_runtime_invoke_checked(method=<unavailable>, obj=<unavailable>, params=<unavailable>, error=0xbff45758)
     5: libmonosgen-2.0.dylib   mono_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>)
     6: libxamarin-debug.dylib  xamarin_invoke_trampoline(type=<unavailable>, self=<unavailable>, sel="application:didFinishLaunchingWithOptions:", iterator=<unavailable>), context=<unavailable>)
     7: libxamarin-debug.dylib  xamarin_arch_trampoline(state=0xbff45ad4)
     8: libxamarin-debug.dylib  xamarin_i386_common_trampoline
     9: UIKit                   -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:]
    10: UIKit                   -[UIApplication _callInitializationDelegatesForMainScene:transitionContext:]
    11: UIKit                   -[UIApplication _runWithMainScene:transitionContext:completion:]
    12: UIKit                   __84-[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]_block_invoke.3124
    13: UIKit                   -[UIApplication workspaceDidEndTransaction:]
    14: FrontBoardServices      __37-[FBSWorkspace clientEndTransaction:]_block_invoke_2
    15: FrontBoardServices      __40-[FBSWorkspace _performDelegateCallOut:]_block_invoke
    16: FrontBoardServices      __FBSSERIALQUEUE_IS_CALLING_OUT_TO_A_BLOCK__
    17: FrontBoardServices      -[FBSSerialQueue _performNext]
    18: FrontBoardServices      -[FBSSerialQueue _performNextFromRunLoopSource]
    19: FrontBoardServices      FBSSerialQueueRunLoopSourceHandler
    20: CoreFoundation          __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
    21: CoreFoundation          __CFRunLoopDoSources0
    22: CoreFoundation          __CFRunLoopRun
    23: CoreFoundation          CFRunLoopRunSpecific
    24: CoreFoundation          CFRunLoopRunInMode
    25: UIKit                   -[UIApplication _run]
    26: UIKit                   UIApplicationMain
    27: TestApp                 (wrapper managed-to-native) UIKit.UIApplication:UIApplicationMain (int,string[],intptr,intptr)
    28: TestApp                 UIKit.UIApplication:Main (string[],intptr,intptr)
    29: TestApp                 UIKit.UIApplication:Main (string[],string,string)
    30: TestApp                 ExceptionMarshaling.IOS.Application:Main (string[])

Tüm bunlar arasındaki yerel durumdayken çerçeveleri 0-1 ve 27-30 yönetilir. Mono bu çerçeveler, hiçbir Objective-C unwinds varsa `@catch` veya `@finally` yan tümceleri yürütülür.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>Senaryo 2 - Objective-C özel durumlarını yakalama mümkün değil

Aşağıdaki senaryoda olduğu _değil_ Objective-C özel durumları yakalamak olası kullanarak yönetilen `catch` işleyicileri Objective-C özel durumu başka bir şekilde işlenmesini nedeni:

1. Objective-C özel durum oluşur.
2. Objective-C çalışma zamanı yığın anlatılmaktadır (ancak bu bırakma değil), yerel için arayan `@catch` özel durumu işleyebilecek işleyici.
3. Objective-C çalışma zamanı bulur bir `@catch` işleyicisi yığın unwinds ve yürütme başlatır `@catch` işleyicisi.

Ana iş parçacığı üzerinde olduğundan genellikle aşağıdakine benzer bir kod bu senaryo genellikle Xamarin.iOS uygulamalarında bulunur:

``` objective-c
void UIApplicationMain ()
{
    @try {
        while (true) {
            ExecuteRunLoop ();
        }
    } @catch (NSException *ex) {
        NSLog (@"An unhandled exception occured: %@", exc);
        abort ();
    }
}

```

Bu ana iş parçacığı üzerinde hiçbir zaman gerçekten bir işlenmemiş Objective-C istisna vardır ve bu nedenle Objective-C özel durumlar için yönetilen özel durumlar dönüştürür bizim geri çağırma hiçbir zaman adlı anlamına gelir.

Bu da çoğu kullanıcı Arabirimi nesneleri hata ayıklayıcısında inceleniyor karşılık özellikleri yoksa seçiciler fetch üzerinde çalışan platform (dener çünkü Xamarin.Mac desteklediğinden daha önceki bir macOS sürümünü Xamarin.Mac uygulamaları hata ayıklama sırasında oldukça yaygındır daha yüksek bir macOS sürüm desteği Xamarin.Mac içerdiği için). Bu tür Seçici çağırma oluşturur bir `NSInvalidArgumentException` ("tanınmayan Seçici gönderilen..."), sonunda işleminin çökmesine neden olur.

Özetle, Objective-C çalışma zamanı ya da bunlar programlanmış değil Mono çalışma zamanı bırakma çerçeveler zorunda tanıtıcı kilitlenmeler, bellek sızıntıları ve diğer türleri beklenmedik (MIS) davranışları gibi tanımsız davranışları yol açabilir.

## <a name="solution"></a>Çözüm

Xamarin.iOS 10 ve Xamarin.Mac 2.10, tüm yönetilen yerel sınırında hem yönetilen hem de Objective-C özel durumları yakalama ve bu durum diğer tür dönüştürme için destek ekledik.

Sözde kodda, şuna benzer:

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

P/Invoke objc_msgSend ele ve bunun yerine adı verilir:

``` objective-c
void
xamarin_dyn_objc_msgSend (id obj, SEL sel)
{
    @try {
        objc_msgSend (obj, sel);
    } @catch (NSException *ex) {
        convert_to_and_throw_managed_exception (ex);
    }
}
```

Ve benzeri (Objective-C özel durumlar için yönetilen özel durumlar hazırlama) geriye doğru çalışması için yapılır.

Yönetilen yerel sınırında özel durumları yakalama değil maliyet ücretsiz, bu nedenle, her zaman varsayılan olarak etkindir:

- Xamarin.iOS/tvOS: Objective-C özel durumlar ele geçirilmesini benzeticisinde etkinleştirilir.
- Xamarin.watchOS: kişiler tarafından ele tüm durumlarda zorlanır, yönetilen Objective-C çalışma zamanı bırakma izin vererek çünkü çerçeveler atık toplayıcı karıştırır ve askıda kalmasına ya da kilitlenme ya da kolaylaştırır.
- Xamarin.Mac: hata ayıklama derlemeleri için Objective-C özel durumlar ele geçirilmesini etkinleştirilir.

[Derleme zamanı bayrakları](#build_time_flags) bölüm nasıl varsayılan olarak etkin değilse kişiler tarafından ele etkinleştirileceğini açıklar.

## <a name="events"></a>Olaylar

Bir özel durum engelledik sonra oluşan iki yeni olayları vardır: `Runtime.MarshalManagedException` ve `Runtime.MarshalObjectiveCException`.

Her iki olay geçirilen bir `EventArgs` oluşturuldu orijinal özel durumu içeren nesne ( `Exception` özelliği) ve bir `ExceptionMode` özel durumun nasıl sıralanması gerektiğini tanımlamak için özellik.

`ExceptionMode` Özellik değiştirilebilir olay işleyicisi yapılan herhangi bir özel işlem göre davranışını değiştirmek için işleyici. Belirli bir özel durum oluşursa işlemini iptal etmek için bir örnek olabilir.

Değiştirme `ExceptionMode` özelliği, tek olay uygulanır, gelecekte ele özel durumlar etkilemez.

Aşağıdaki modları kullanılabilir:

- `Default`: Varsayılan platforma göre değişir. Bu `ThrowObjectiveCException` GC işbirlikçi (watchOS) modundaysa ve `UnwindNativeCode` Aksi takdirde (iOS / watchOS / macOS). Varsayılan gelecekte değişebilir.
- `UnwindNativeCode`: Önceki (tanımsız) davranış budur. Bu GC işbirlikçi modunda kullanılırken kullanılabilir değil (watchOS yalnızca seçeneğini olduğu; bu nedenle, bu watchOS üzerinde geçerli bir seçenek değil), ancak tüm platformlar için varsayılan seçenektir.
- `ThrowObjectiveCException`: Yönetilen özel durum Objective-C istisna dönüştürün ve Objective-C özel durum. Bu watchOS üzerinde varsayılandır.
- `Abort`: İşlem iptal.
- `Disable`: Özel durumu ele geçirilmesini olay işleyicisini bu değeri ayarlamak için doesn't make Sense, ancak olay tetiklenir sonra bunu çok geç devre dışı bırakmak için devre dışı bırakır. Herhangi bir durumda, ayarlanırsa, bu olarak davranacak varsa `UnwindNativeCode`.

Yönetilen kod için özel durumlar Objective-C sıralama için aşağıdaki modları kullanılabilir:

- `Default`: Varsayılan platforma göre değişir. Bu `ThrowManagedException` GC işbirlikçi (watchOS) modundaysa ve `UnwindManagedCode` Aksi takdirde (iOS / tvOS / macOS). Varsayılan gelecekte değişebilir.
- `UnwindManagedCode`: Önceki (tanımsız) davranış budur. Bu GC işbirlikçi modunda kullanılırken kullanılabilir değil (yalnızca geçerli watchOS GC modunu olduğu; bu nedenle bu watchOS üzerinde geçerli bir seçenek değil), ancak tüm platformlar için varsayılandır.
- `ThrowManagedException`: Yönetilen bir özel durum Objective-C özel durumu dönüştürün ve yönetilen özel durum. Bu watchOS üzerinde varsayılandır.
- `Abort`: İşlem iptal.
- `Disable`: Özel durumu ele geçirilmesini devre dışı bırakır, ayarlamak için doesn't make Sense şekilde olay işleyicisi, ancak bir kez olayı yükseltildiğinde bu değer, çok geç devre dışı bırakmak için. Her durumda ayarlanırsa, bu işlem iptal edilecek durumunda.

Bu nedenle, bir özel durum sıralanmış her zaman görmek için bunu yapabilirsiniz:

``` csharp
Runtime.MarshalManagedException += (object sender, MarshalManagedExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling managed exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
    
};
Runtime.MarshalObjectiveCException += (object sender, MarshalObjectiveCExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling Objective-C exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
};
```

<a name="build_time_flags" />

## <a name="build-time-flags"></a>Derleme zamanı bayrakları

Aşağıdaki seçenekler geçirmek mümkün **mtouch** (Xamarin.iOS uygulamaları için) ve **mmp** (Xamarin.Mac uygulamalar) için özel durum kişiler tarafından ele etkinleştirilirse belirler ve varsayılan eylemi ayarlama gerçekleşeceğini:

- `--marshal-managed-exceptions=`
  - `default`
  - `unwindnativecode`
  - `throwobjectivecexception`
  - `abort`
  - `disable`

- `--marshal-objectivec-exceptions=`
  - `default`
  - `unwindmanagedcode`
  - `throwmanagedexception`
  - `abort`
  - `disable`

Dışında `disable`, bu değerleri özdeş `ExceptionMode` geçirilen değerleri `MarshalManagedException` ve `MarshalObjectiveCException` olaylar.

`disable` Seçeneği _çoğunlukla_ herhangi yürütme yükünü eklediğinizde değil, biz yine özel durumlar müdahale dışındaki kişiler tarafından ele, devre dışı bırakın. Hazırlama olayları yürütülen platform için varsayılan modu olan varsayılan mod ile bu özel durumlar için hala oluşturulur.

## <a name="limitations"></a>Sınırlamalar

Biz yalnızca P/Invokes öğesini müdahale `objc_msgSend` ailesi Objective-C özel durumları yakalamak çalışırken işlevlerini. Başka bir deyişle, P/Invoke ardından Objective-C özel durumları oluşturur, başka bir C işlevi (Bu gelecekte iyileştirilebilir) eski ve tanımlanmamış davranışı hala çalışır.

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>İlgili bağlantılar

- [Özel durum Marshaling (örnek)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
