---
title: "Yerel bir kilitlenme hata ayıklama"
description: "Bu kılavuz, Objective-C çalışma zamanı'nda kaynaklanan özel durumları hata ayıklama açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: B0C0CE31-2737-4969-8EA5-D39D3333E9C2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: d8633a3f575b51d4eeac326cc5ea418fcbf5bd20
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="debugging-a-native-crash"></a>Yerel bir kilitlenme hata ayıklama

## <a name="overview"></a>Genel Bakış

Bazen hataları programlama neden çöküyor yerel Objective-C çalışma zamanı'nda. C# özel durumlar farklı olarak, bunlar düzeltmek için bakabilirsiniz kodunuzu belirli bir satırda işaret yok. Bazı durumlarda bunlar bulmak ve düzeltmek için Önemsiz olabilir ve bunlar bazen izlemek oldukça zor olabilir. 

Şimdi birkaç gerçek yerel kilitlenme örneklerle izlemek ve göz atın.

## <a name="example-1-assertion-failure"></a>Örnek 1: Onaylama hatası

Basit bir sınama uygulaması çökmesi ilk birkaç satırlık İşte (Bu bilgiler olacak **uygulama çıktısı** paneli):

```csharp
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.381 NSOutlineViewHottness[79111:1304993] (
    0   CoreFoundation                      0x91888343 __raiseError + 195
    1   libobjc.A.dylib                     0x9a5e6a2a objc_exception_throw + 276
    2   CoreFoundation                      0x918881ca +[NSException raise:format:arguments:] + 138
    3   Foundation                          0x950742b1 -[NSAssertionHandler handleFailureInMethod:object:file:lineNumber:description:] + 118
    4   AppKit                              0x975db476 -[NSTableView _uncachedRectHeightOfRow:] + 373
    5   AppKit                              0x975db2f8 -[_NSTableRowHeightStorage _uncachedRectHeightOfRow:] + 143
    6   AppKit                              0x975db206 -[_NSTableRowHeightStorage _cacheRowHeights] + 167
    7   AppKit                              0x975db130 -[_NSTableRowHeightStorage _createRowHeightsArray] + 226
    8   AppKit                              0x975b5851 -[_NSTableRowHeightStorage _ensureRowHeights] + 73
    9   AppKit                              0x975b5790 -[_NSTableRowHeightStorage computeTableHeightForNumberOfRows:] + 89
    10  AppKit                              0x975b4c38 -[NSTableView _totalHeightOfTableView] + 220
```

Numaralarıyla önekli satırdır yerel yığın izleme. Değerinden görebilirsiniz kilitlenme oluştu yere içinde `NSTableView` satır yükseklikte işleme. Ardından `NSAssertionHandler` ateşlenir bir `NSException (objc_exception_throw)` ve biz onaylama işlemi hatasına bakın:

```csharp
rowHeight error: The value must be > 0 for row 0, but the delegate
<NSOutlineView_ViewDelegate: 0xaa01860> gave -1.000
```

Bu gördüğünüzde, sahip oldukça temizleyin, bazı `NSOutlineViewDelegate` yöntemi negatif bir sayı döndürüyor. Bu sorun oluştu:

```csharp
public override nfloat GetRowHeight (NSTableView tableView, nint row)
{
    return -1;
}
```

## <a name="example-2-callback-jumped-into-middle-of-nowhere"></a>Örnek 2: ortasına saklanıyorsa Atlanan geri çağırma

```csharp
Stacktrace:

 at <unknown> <0xffffffff>
 at (wrapper managed-to-native) MonoMac.AppKit.NSApplication.NSApplicationMain (int,string[]) <IL 0x000a4, 0xffffffff>
 at MonoMac.AppKit.NSApplication.Main (string[]) [0x00041] in /Users/donblas/Programming/xamcore-master/src/AppKit/NSApplication.cs:107
 at NSOutlineViewHottness.MainClass.Main (string[]) [0x00007] in /Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/Main.cs:14
 at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr) <IL 0x00050, 0xffffffff>

Native stacktrace:

Debug info from gdb:

(lldb) command source -s 0 '/tmp/mono-gdb-commands.qrHllW'
Executing commands in '/private/tmp/mono-gdb-commands.qrHllW'.
(lldb) process attach --pid 79229
Process 79229 stopped
Executable module set to "/Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/bin/Debug/NSOutlineViewHottness.app/Contents/MacOS/NSOutlineViewHottness".
Architecture set to: i386-apple-macosx.
(lldb) thread list
Process 79229 stopped
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 thread #2: tid = 0x142790, 0x9af768d2 libsystem_kernel.dylib`kevent64 + 10, queue = 'com.apple.libdispatch-manager'
 thread #3: tid = 0x142792, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #4: tid = 0x142794, 0x9af6fa6a libsystem_kernel.dylib`semaphore_wait_trap + 10
 thread #5: tid = 0x142795, 0x9af75772 libsystem_kernel.dylib`__recvfrom + 10
 thread #6: tid = 0x142799, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #7: tid = 0x14279a, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #8: tid = 0x14279b, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #9: tid = 0x1427f8, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #10: tid = 0x1427fe, 0x9af6fa2e libsystem_kernel.dylib`mach_msg_trap + 10
(lldb) thread backtrace all
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 * frame #0: 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10
   frame #1: 0x986bfb25 libsystem_c.dylib`waitpid$UNIX2003 + 48
   frame #2: 0x028ba36d libmono-2.0.dylib`mono_handle_native_sigsegv(signal=11, ctx=0x03115fe0) + 541 at mini-exceptions.c:2323
   frame #3: 0x0290a8bb libmono-2.0.dylib`mono_arch_handle_altstack_exception(sigctx=<unavailable>, fault_addr=<unavailable>, stack_ovf=0) + 155 at exceptions-x86.c:1159
   frame #4: 0x0280b4fd libmono-2.0.dylib`mono_sigsegv_signal_handler(_dummy=<unavailable>, info=<unavailable>, context=<unavailable>) + 445 at mini.c:6861
   frame #5: 0x91ef403b libsystem_platform.dylib`_sigtramp + 43
   frame #6: 0x9a5dd0bd libobjc.A.dylib`objc_msgSend + 45
   frame #7: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #8: 0x9773ba91 AppKit`-[NSApplication sendAction:to:from:] + 548
   frame #9: 0x9773b82d AppKit`-[NSControl sendAction:to:] + 102
   frame #10: 0x97934d36 AppKit`__26-[NSCell _sendActionFrom:]_block_invoke + 176
   frame #11: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #12: 0x97787975 AppKit`-[NSCell _sendActionFrom:] + 161
   frame #13: 0x979188ea AppKit`-[NSButtonCell _sendActionFrom:] + 55
   frame #14: 0x979366e6 AppKit`__48-[NSCell trackMouse:inRect:ofView:untilMouseUp:]_block_invoke965 + 43
   frame #15: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #16: 0x977a3d00 AppKit`-[NSCell trackMouse:inRect:ofView:untilMouseUp:] + 2815
   frame #17: 0x977a2df4 AppKit`-[NSButtonCell trackMouse:inRect:ofView:untilMouseUp:] + 524
   frame #18: 0x977a233b AppKit`-[NSControl mouseDown:] + 762
   frame #19: 0x97cbc112 AppKit`-[_NSThemeWidget mouseDown:] + 378
   frame #20: 0x97d36d74 AppKit`-[NSWindow _reallySendEvent:] + 12353
   frame #21: 0x977201f9 AppKit`-[NSWindow sendEvent:] + 409
   frame #22: 0x976cdc67 AppKit`-[NSApplication sendEvent:] + 4679
   frame #23: 0x9754807c AppKit`-[NSApplication run] + 1003
```

Bu, izlemek çok daha zor bir sorundur. Gördüğünüzde `at <unknown> <0xffffffff>` veya `MonoMac.ObjCRuntime.Runtime.GetNSObject (IntPtr ptr)` Yönetilen yığın izleme üst kısmında, biz çalıştığınız çöp toplanmış olan bir nesne ile yönetilen kodu çalıştırmadıkları önerir. Yerel yığın izleme gösterir `trackMouse:inRect:ofView:untilMouseUp` içine `NSCell _sendActionFrom` biz sıralandığından click olayını işleme; Bu nedenle, C# ve Ölüyor geri arama çalışılıyor.

Genel olarak, bu gibi hataları takip etmek zordur. Ekledim `GC.Collect(2)` bir düğme işleyicisi sorun tekrarlanabilir yapma (çöp toplama zorlama) Bu sorunu belirlemelerine izlemeye yardımcı olmak için. Örnek bir süredir bisecting sonra sorun kadar kodun bölümlerini kaldırma kayboldu, onu dönüştü [bu hatayı](https://bugzilla.xamarin.com/show_bug.cgi?id=23378):

```csharp
mainWindowController.Window.StandardWindowButton (NSWindowButton.CloseButton).Activated += HandleActivated;
```

`NSButton` Tarafından döndürülen `StandardWindowButton()` bir olay (yani hatayı) kendisine kayıtlı olsa bile toplanmakta. Düğme toplanacak yüklediyse biz tıklayarak, bu olay çağrılacak çalıştığınızda biz kilitlenme.

Bu özel sorunun kök nedeni değildi rağmen Yığın izlemeleri gibi bu ayrıca hatalı yöntem imzaları işlevlerinde neden olabilir `[Export]`ed Objective-c için Örneğin, bir yöntemi olması için bir parametre bekler bir `out string` ve olarak yazın `string`, aynı şekilde kilitlenme.

## <a name="example-3-callbacks-and-managed-objects"></a>Örnek 3: Geri aramalar ve yönetilen nesneler

Birçok Cocoa API'leri "geri kitaplığı tarafından bazı olay gerçekleştiğinde bir iletmenize yanıt vermesi veya bir görevlerini gerçekleştirmek için bazı verilerin gerektiğinde çağrılan" içerir. Öncelikle, düşünebilirsiniz olsa **temsilci** ve **DataSource** desenleri, böylece iş API'leri çok sayıda vardır. Örneğin, yöntemlerinin kıldığınızda bir `NSView` ve görsel ağacına eklemek, geri belirli olaylar oluştuğunda sizi aramasını sağlayan AppKit bekler.

Neredeyse tüm durumlarda Xamarin.Mac doğru bu geri aramalar Yönetilen Nesne hedefinin bunlar yine geri çağrılabilir sırada Çöpünün toplanma engeller. Ancak, nadiren bağlama hataları bu kesintiye uğratabilir. Bu gerçekleştiğinde, aşağıdakine benzer kötü kilitlenme görebilirsiniz:

```csharp
Thread 0 Crashed:: Dispatch queue: com.apple.main-thread

0   libsystem_kernel.dylib              0x98c2f69a __pthread_kill + 10
1   libsystem_pthread.dylib             0x90341f19 pthread_kill + 101
2   libsystem_c.dylib                   0x9453feee abort + 156
3   libmonosgen-2.0.dylib               0x020bfba5 mono_handle_native_sigsegv + 757
4   libmonosgen-2.0.dylib               0x0210b812 mono_arch_handle_altstack_exception + 162
5   libmonosgen-2.0.dylib               0x0200c55e mono_sigsegv_signal_handler + 446
6   libsystem_platform.dylib            0x9513003b _sigtramp + 43
7   ???                                 0xffffffff 0 + 4294967295
8   libmonosgen-2.0.dylib               0x0200c3a0 mono_sigill_signal_handler + 48
9   com.apple.AppKit                    0x99f76041 -[NSView setFrame:] + 448
10  com.apple.AppKit                    0x9a1fd4ea -[NSToolbarView adjustToWindow:attachedToEdge:] + 198
11  com.apple.AppKit                    0x9a1fd414 -[NSToolbar _adjustViewToWindow] + 68
12  com.apple.AppKit                    0x9a01eb0d -[NSToolbar _windowWillShowToolbar] + 79

```

Bu kılavuz size izlemek yardımcı bu yapısı hatalar, kırparsanız doğru şekilde rapor bunları onlar çözülene ve bunları geçici bir çözüm olarak, o zamana kadar kodunuzu olduğunuz.

### <a name="locating"></a>Bulma

Neredeyse her durumda, bu yapısı hatalarda birincil belirti normalde şuna benzer bir öğe ile yerel kilitlenme olduğu `mono_sigsegv_signal_handler`, veya `_sigtrap` yığının üst çerçeve içinde. Cocoa, C# kodu geri çağrı girişimi, atık toplanan nesne basarsa ve kilitlenen. Ancak, bu simgeleri ile her kilitlenme şuna benzer bir bağlama sorunu neden olur, bu sorun olup olmadığını onaylamak için bazı ek sorunda yapmanız gerekir. 

Bu hatalar izlemek zordur kılan yalnızca oluşma olan **sonra** çöp toplama söz konusu nesne atıldı. Bu hatalar birini ziyaret düşünüyorsanız, aşağıdaki kod, başlatma sırası yere ekleyin:

```csharp
new System.Threading.Thread (() => 
{
    while (true) {
         System.Threading.Thread.Sleep (1000);
         GC.Collect ();
    }
}).Start (); 
```

Bu, her saniye atık toplayıcı çalıştırmak için uygulamanızın zorlar. Uygulamanızı yeniden çalıştırın ve hatayı yeniden deneyin. Hemen ya da tutarlı bir şekilde yerine rastgele kilitlenme, sağ kanalında demektir.

### <a name="reporting"></a>Raporlama

Bağlama için gelecek sürümlerde giderilip giderilemeyeceğini şekilde için Xamarin sorunu bildirmek için sonraki adımdır bakın. Bir iş ya da Kurumsal Lisans sahibi olması durumunda, bilet 

[http://xamarin.com/support](http://xamarin.com/support)

Aksi takdirde, var olan bir sorunu arayın:

- Denetleme [Xamarin.Mac forumları](https://forums.xamarin.com/categories/mac)
- Arama [sorunu deposu](https://github.com/xamarin/xamarin-macios/issues)
- GitHub konulara geçmeden önce Xamarin sorunları üzerinde izlenmekte olan [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Lütfen var. sorunları eşleştirmek için arama yapın.
- Eşleşen bir sorunu bulamazsanız, Lütfen yeni bir sorun dosya [GitHub sorunu depo](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub sorunlar tüm ortak verilmektedir. Yorumlar veya ekleri gizlemek mümkün değildir. 

Lütfen aşağıdaki mümkün olduğunca çok şunları içerir:

- Sorunu yeniden oluşturma, basit bir örnek. Bu **çok** mümkün olduğunda. 
- Kilitlenme tam yığın izleme.
- Kilitlenme çevreleyen C# kod.   

### <a name="working-around"></a>Geçici çalışma

Sorunu belirlemelerine takip sonra bağlama sabit kadar geçici bir çözüm sorun düzeltme eki uygulama basit olabilir. Amacı nesne engellemektir (**Görünüm**, **temsilci**, **DataSource**), yanlış kapatılır açık bir başvuru tutarak bellekten ayrılmadan.

Basit durumlar için nesne yalnızca tek bir örneği burada kodu öğesinden değiştirin:

```csharp
void AddObject ()
{
    item.View = new MyView ();
    ...
}
```

Bu gibi statik bir değişken kullanmak için:

```csharp
static NSObject view;
...

void AddObject ()
{
    view = new MyView ();
    item.View = view;
    ...
}
```

Burada birden çok örneği oluşturulabilir, bir statik durumlarda `HashSet` değişiklik:

```csharp
static HashSet<NSObject> collection = new HashSet<NSObject> ();
...

void AddObject ()
{
    item.View = new MyView ();
    collection.Add (item.View );
    ...
}
```

Geçici çözüm kodu bağlama sabit ve düzeltmeyi içerir Xamarin.Mac sürümüne yükseltme yapılandırdığınız sonra kaldırılabilir.

## <a name="exception-bubbling-and-objective-c"></a>Özel durum tırmanmasını ve Objective-C

Bir C# "yönetilen kod arama Objective-C yöntemi kaçınmak" özel durumu hiçbir zaman izin vermelidir. Bunu yaparsanız, sonuçları tanımsız ancak genellikle kilitlenen içerir. Genel olarak, biz geçebiliriz Kabarcık yardımcı olmak yerel ve yönetilen çökme yararlı bilgiler için her şeyi yapmak sorunlarınızı hızla çözdü.

Çok ile teknik nedenlerle neden çıkmaza olmadan, her yönetilen/yerel sınırında yönetilen özel durumları yakalamak için altyapıyı ayarı trivially olmayan pahalı olduğunu ve bir _çok_ içinde durum geçişlerinin birçok ortak işlemleri. Birçok işlemleri kullanıcı Arabirimi iş parçacığı ile ilgili olanları özellikle hızla durdurmalıdır veya uygulamanızın titremesine ve kabul edilebilir performans özellikleri olan. Bu ek yük her ikisi de maliyetli ve bu durumlarda gereksiz olacaktır şekilde bu geri aramalar çoğunu nadiren atma, olanağına sahip çok basit şeyler.

Bu nedenle, biz değil olanlar deneyin / sizin için yakalar. Kod burada mu Önemsiz olmayan şeyler yerleri (ötesine söyleyin döndüren bir Boole değerlerini veya basit matematik), kendiniz catch deneyebilirsiniz. 

