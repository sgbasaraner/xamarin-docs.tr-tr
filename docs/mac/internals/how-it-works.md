---
title: Xamarin.Mac nasıl çalışır?
description: Bu belgede Internet'in iç Xamarin.Mac açıklanmaktadır. Özellikle, Oluşturucular, zamanında derleme öncesinde bellek yönetimi ve kayıt şirketi görünüyor.
ms.prod: xamarin
ms.assetid: C2053ABB-6DBF-4233-AEEA-B72FC6A81FE1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/25/2017
ms.openlocfilehash: baa60d177a7d7d070a218108b2f6779eeaf94f78
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30787095"
---
# <a name="how-xamarinmac-works"></a>Xamarin.Mac nasıl çalışır?

Çoğu geliştiricinin hiçbir zaman sahip olur ancak, iç "Sihirli" Xamarin.Mac, biri hakkında endişelenmeye gerek zaman nasıl başlık altında şeyler works hem bir C# Mercek ile var olan belgeler yorumlama ve sorunları hata ayıklama yardımcı olur, kaba bir anlama sahip olduğunda Bunlar ortaya çıkar.

Xamarin.Mac içinde bir uygulamayı iki dünyanın arasında köprü: yerel sınıflarda örneklerini içeren temel Objective-C çalışma zamanı yok (`NSString`, `NSApplication`, vb.) ve yönetilen sınıflar örneklerini içeren C# çalışma zamanı (`System.String`, `HttpClient`, vb.). Bir uygulama yöntemleri (Seçici) Objective-C çağırabilir bu iki dünyanın Between Xamarin.Mac bir iki yönlü köprüsü oluşturur, bu nedenle (gibi `NSApplication.Init`) ve Objective-C, uygulamanın C# yöntemler geri (gibi bir uygulama temsilcisi yöntemlere) çağırabilirsiniz. Genel olarak, Objective-C çağrılarını saydam aracılığıyla işlenen **P/Invokes** ve bazı çalışma zamanı kodu Xamarin sağlar.

<a name="exposing-classes" />

## <a name="exposing-c-classes--methods-to-objective-c"></a>C# sınıfları gösterme / Objective-C yöntemleri

Ancak, Objective-bir uygulamanın C# nesnelerini geri çağırmaya C için bunlar Objective-C anlayabileceği bir şekilde açığa çıkarılması gerekir. Bu aracılığıyla yapılır `Register` ve `Export` öznitelikleri. Aşağıdaki örnek alın:

```csharp
[Register ("MyClass")]
public class MyClass : NSObject
{
   [Export ("init")]
   public MyClass ()
   {
   }

   [Export ("run")]
   public void Run ()
   {
   }
}
```

Bu örnekte, Objective-C çalışma zamanı adlı bir sınıf hakkında artık bilmesini `MyClass` adlı Seçici ile `init` ve `run`.

Bir uygulama alan çoğu geri aramalar geçersiz kılınan yöntemleriyle ya da açık olacak çoğu durumda bu Geliştirici yoksayabilirsiniz, bir uygulama ayrıntılarını aynıdır `base` sınıfları (gibi `AppDelegate`, `Delegates`, `DataSources`) veya  **Eylemler** API geçirildi. Bu durumların tümünde `Export` öznitelikleri C# kodunda gerekli değildir.

## <a name="constructor-runthrough"></a>Oluşturucu runthrough

Çoğu durumda onu gibi yerlerden oluşturulabilir şekilde uygulamanın C# sınıfları oluşturma API Objective-C çalışma zamanı için kullanıma sunmak Geliştirici gerekir film şeridi veya XIB dosyalarında çağrıldığında. Xamarin.Mac uygulamalarında kullanılan beş en yaygın oluşturucular şunlardır:

```csharp
// Called when created from unmanaged code
public CustomView (IntPtr handle) : base (handle)
{
   Initialize ();
}

// Called when created directly from a XIB file
[Export ("initWithCoder:")]
public CustomView (NSCoder coder) : base (coder)
{
   Initialize ();
}

// Called from C# to instance NSView with a Frame (initWithFrame)
public CustomView (CGRect frame) : base (frame)
{
}

// Called from C# to instance NSView without setting the frame (init)
public CustomView () : base ()
{
}

// This is a special case constructor that you call on a derived class when the derived called has an [Export] constructor.
// For example, if you call init on NSString then you don’t want to call init on NSObject.
public CustomView () : base (NSObjectFlag.Empty)
{
}
```

Genel olarak, geliştirici bırakmalısınız `IntPtr` ve `NSCoder` özel gibi bazı türleri oluştururken oluşturulan oluşturucuları `NSViews` tek başına. Xamarin.Mac şu oluşturuculardan birine Objective-C çalışma zamanı isteğine yanıt olarak çağırması gerekir ve kaldırmış olduğunuz, uygulama yerel kod kilitleniyor ve sorunu tam olarak anlamak zor olabilir.

## <a name="memory-management-and-cycles"></a>Bellek yönetimi ve döngüsü

Bellek yönetimi Xamarin.Mac'nde, birçok yolla Xamarin.iOS için çok benzer değildir. Bunu ayrıca bir karmaşık, bu belgenin kapsamı dışında kalan bir konudur. Lütfen okuyun [bellek ve performans en iyi uygulamaları](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="ahead-of-time-compilation"></a>Şimdi zaman derlenmesini

Genellikle, .NET uygulamaları derleme makine kodu aşağıya doğru oluşturulduklarında, bunun yerine alır IL kodu denilen bir ara katman derlenmesi _zaman içinde sadece_ (JIT), uygulama başlatıldığında makine koda derlenmiş.

Gerekli makine kodunun oluşturulması zaman yararlanırken mono çalışma zamanı JIT derleme için geçen süre bu makine kodu en fazla 20 oranında Xamarin.Mac uygulama başlatma yavaşlatabilir.

Apple tarafından uygulanan iOS sınırlamaları nedeniyle, JIT derleme IL kodu Xamarin.iOS için kullanılabilir değil. Sonuç olarak, tüm Xamarin.iOS uygulaması tam _zaman Ahead_ (Uygulama Nesne AĞACI) yapı döngüsü sırasında makine koda derlenmiş.

Xamarin.iOS yapabildiğiniz gibi Xamarin.Mac için uygulama nesne AĞACI yeteneği IL kodu ve uygulama yapı döngüsü sırasında yenidir. Xamarin.Mac kullanan bir _karma_ gerekli makine kodu çoğunu derler Uygulama Nesne AĞACI yaklaşım, ancak gerekli trampolines ve Reflection.Emit desteklemeye devam etmek için esneklik derlemek çalışma zamanı sağlar (ve diğer kullanım örnekleri şu anda Xamarin.Mac üzerinde çalışır).

Uygulama Nesne AĞACI Xamarin.Mac uygulama burada yardımcı olabilecek iki önemli konu vardır:

- **Daha iyi "yerel" kilitlenme günlükleri** - Xamarin.Mac uygulama ortak Cocoa API'leri geçersiz çağrılar yapılırken bulunduktan yerel kodda çökerse (gönderme gibi bir `null` kabul etmez yöntemiyle), yerel çöküş günlüklerini JIT ile Çerçeve analiz etmek zordur. JIT çerçeveler hata ayıklama bilgisi yoktur olduğundan, onaltılık uzaklıkları ile birden çok satır ve neler hakkında hiçbir ipucu olmayacaktır. Uygulama Nesne AĞACI "gerçek" adlandırılmış çerçeveler oluşturur ve izlemeleri okumak çok daha kolay. Bu aynı zamanda Xamarin.Mac uygulama etkileşim daha iyi yerel araçlarıyla gibi gelir **lldb** ve **Instruments**.
- **Daha iyi zaman performans'ı başlatın** - için birden çok ikinci başlangıç süresi, tüm kod derleme JIT olan büyük Xamarin.Mac uygulamalar önemli miktarda zaman alabilir. Uygulama Nesne AĞACI bu Önden çalışır.

### <a name="enabling-aot-compilation"></a>Uygulama Nesne AĞACI derleme etkinleştirme

Uygulama Nesne AĞACI etkin Xamarin.Mac çift tıklatarak **proje adı** içinde **Çözüm Gezgini**, gezinme için **Mac yapı** ve ekleyerek `--aot:[options]` için **Ek mmp bağımsız değişkenler:** alan (burada `[options]` bir veya daha fazla seçenekleri uygulama nesne AĞACI türü denetlemek için aşağıya bakın). Örneğin:

![Uygulama Nesne AĞACI ek mmp bağımsız değişkenleri ekleme](how-it-works-images/aot01.png "Uygulama Nesne AĞACI ek mmp bağımsız değişkenleri ekleme")

> [!IMPORTANT]
> Uygulama Nesne AĞACI etkinleştirme derleme bazen birkaç dakika kadar derleme zamanı önemli ölçüde artırır, ancak % 20 ortalama tarafından uygulama başlatma sürelerini geliştirebilir. Sonuç olarak, uygulama nesne AĞACI derleme yalnızca etkinleştirilmesi **sürüm** bir Xamarin.Mac uygulamasının oluşturur.

### <a name="aot-compilation-options"></a>Uygulama Nesne Ağacı derleme seçenekleri

Uygulama Nesne AĞACI derleme Xamarin.Mac bir uygulamada etkinleştirirken ayarlanabilir birkaç farklı seçenekler vardır:

- `none` -Hiçbir uygulama nesne AĞACI derleme. Varsayılan ayar budur.
- `all` -Uygulama Nesne AĞACI MonoBundle her derlemede derler.
- `core` -Uygulama Nesne AĞACI derler `Xamarin.Mac`, `System` ve `mscorlib` derlemeler.
- `sdk` -Uygulama Nesne AĞACI derler `Xamarin.Mac` ve temel sınıf kitaplıkları (BCL) derlemeler.
- `|hybrid` -Bu yukarıdaki seçeneklerden birine karma IL çıkarma sağlayan uygulama nesne AĞACI sağlar, ancak sonucunda kez artık derleme ekleme.
- `+` -Uygulama Nesne AĞACI derleme için tek bir içerir.
- `-` -Tek bir dosya uygulama nesne AĞACI derlemeden kaldırır.

Örneğin, `--aot:all,-MyAssembly.dll` tüm MonoBundle derlemelerde Uygulama Nesne AĞACI derleme sağlayacağı _dışında_ `MyAssembly.dll` ve `--aot:core|hybrid,+MyOtherAssembly.dll,-mscorlib.dll` kod Uygulama Nesne AĞACI içeriyor, karma sağlayacağı `MyOtherAssembly.dll` ve hariç`mscorlib.dll`.

## <a name="partial-static-registrar"></a>Kısmi statik kayıt

Xamarin.Mac Uygulama geliştirirken, bir değişiklik tamamladıktan ve test arasındaki süreyi en aza indirme geliştirme son tarihleri toplantı önemli olabilir. Stratejiler birini uyumluluğuna olarak kullanılabilecek kod temeli ve birim testleri yardımcı derleme sürelerini azaltmak için bir uygulama pahalı bir tam yeniden gerektirecektir sayısını azaltmak gibi gibi.

Ayrıca ve Xamarin.Mac, yeni _kısmi statik kayıt_ (Xamarin.iOS tarafından öncülüğünü gibi) bir Xamarin.Mac uygulamasını başlatma sürelerini önemli ölçüde azaltabilir **hata ayıklama** yapılandırma. Kısmi statik kaydedici kullanarak nasıl yapabilirsiniz anlama kayarken çıkışı bir bir hata ayıklama başlatma neredeyse 5 x gelişme biraz arka plan kayıt şirketi nedir, statik ve dinamik arasında ne fark vardır ve bu "kısmi statik" sürüm ne olur.

### <a name="about-the-registrar"></a>Kayıt hakkında

Tüm Xamarin.Mac başlık altında uygulama Apple ve Objective-C çalışma zamanı Cocoa çerçevesinden arasındadır. Bu "yerel world" ve "yönetilen world" C# arasında köprü oluşturma, Xamarin.Mac birincil sorumluluğundadır. Bu görev parçası içinde yürütülen kayıt şirketi tarafından işlenir `NSApplication.Init ()` yöntemi. Tüm Xamarin.Mac Cocoa API'leri kullanılmasını gerektiren bir neden de budur `NSApplication.Init` önce çağrılabilir.

Sınıflardan gibi türetilen uygulamanın C# sınıfları varlığını Objective-C çalışma zamanı hakkında bilgilendirmek için kayıt şirketinin iş `NSApplicationDelegate`, `NSView`, `NSWindow`, ve `NSObject`. Bu, uygulama içindeki tüm türler ihtiyaç duyduğu kaydetme ve rapor her türe hangi öğelerin belirlemek için bir tarama gerektirir.

Bu tarama ya da yapılabilir **dinamik olarak**, yansıma, olan uygulama başlangıcında veya **statik olarak**, derleme zamanı adımı olarak. Bir kayıt türü seçilmesinde Geliştirici aşağıdakileri bilmeniz gerekir:

- Statik kayıt başlatma sürelerini önemli ölçüde azaltabilir, ancak derlemeleri kez (genellikle birden fazla çift hata ayıklama derleme zamanında) önemli ölçüde yavaşlatabilir. Bu varsayılan olacaktır **sürüm** yapılandırma oluşturur.
- Dinamik kayıt bu işi uygulama başlatılırken uygulama başlatma ve kod oluşturma atlar, ancak bu ek iş belirgin duraklatma (en az iki saniye) oluşturabilirsiniz kadar geciktirir. Dinamik kayıt ve, yansıma daha yavaştır, varsayılan olarak hata ayıklama yapılandırması yapılarında özellikle fark budur.

İlk Xamarin.iOS 8.13 içinde sunulan kısmi statik kayıt Geliştirici en iyi her iki seçenek sunar. Her öğe kayıt bilgilerinin önceden bilgi işlem tarafından `Xamarin.Mac.dll` ve bu bilgilerle (yani, yalnızca derleme zamanında bağlantılı olması gerekir) statik kitaplıkta Xamarin.Mac sevkiyat, Microsoft çoğu dinamik yansıma zaman kaldırdı derleme zamanı etkileyen değil kayıt şirketi.

### <a name="enabling-the-partial-static-registrar"></a>Kısmi statik kayıt etkinleştirme

Kısmi statik kayıt içinde Xamarin.Mac çift tıklatarak etkin **proje adı** içinde **Çözüm Gezgini**, gezinme için **Mac yapı** ve ekleme`--registrar:static` için **ek mmp bağımsız değişkenler:** alan. Örneğin:

![Ek mmp bağımsız değişkenleri kısmi statik kayıt ekleme](how-it-works-images/psr01.png "ek mmp bağımsız değişkenleri kısmi statik kayıt ekleme")

## <a name="additional-resources"></a>Ek kaynaklar

Şeyler dahili olarak nasıl hakkında ile ilgili daha ayrıntılı bazı açıklamalar şunlardır:

- [Objective-C seçiciler](~/ios/internals/objective-c-selectors.md)
- [Kaydedici](~/ios/internals/registrar.md)
- [İOS ve OS X için Xamarin Unified API](~/cross-platform/macios/unified/index.md)
- [Theading temelleri](~/ios/app-fundamentals/threading.md)
- [Temsilciler, protokolleri ve olaylar](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [hakkında `newrefcount`](~/ios/internals/newrefcount.md)

