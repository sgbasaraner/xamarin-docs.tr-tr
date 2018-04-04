---
title: Sürüm Geçmişi
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: a9eb4b1ea958f2756eb2a21ee44d0d0dfcf8b931
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="release-history"></a>Sürüm Geçmişi

## <a name="34-october-11-2017"></a>3.4 (11 Ekim 2017)

[V3.4.0 indirin](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Xcode 9 desteği: iOS 11 macOS 10.13, 11, tvOS ve watchOS 4
* SIMD ve tgmath sorunlarını şimdi düzeltilmesi gereken
* Telemetri tamamen kaldırılmıştır

## <a name="33-august-3-2016"></a>3.3 (3 Ağustos 2016)

[V3.3.0 indirin](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Xcode 8 Beta 4, iOS 10, macOS 10,12, tvOS için 10 ve watchOS 3 destekler.
* Son ana derleme (2016-08-02) Clang güncelleştirildi
* [Telemetri gönderimini seçenekleri kalıcı](https://twitter.com/Symbiatch/status/760373403878559744) gelen `sharpie pod bind` için `sharpie bind`.

## <a name="32-june-14-2016"></a>3.2 (14 Haziran 2016)

[V3.2.3 indirin](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Xcode 8 Beta 1, iOS 10, macOS 10,12, tvOS için 10 ve watchOS 3 destekler.

## <a name="31-may-31-2016"></a>3.1 (31 Mayıs 2016)

[V3.1.1 indirin](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* CocoaPods 1.0 için destek
* Yerel oluşturarak CocoaPods bağlama güvenilirlik geliştirilmiş `.framework` ve, bağlama
* CocoaPods kopyalama `.framework` ve tanımı içine bağlama bir `Binding` Xamarin.iOS ve Xamarin.Mac bağlama projeleri ile tümleştirme kolaylaştırmak için dizin

## <a name="30-october-5-2015"></a>3.0 (5 Ekim 2015)

[V3.0.8 indirin](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Basit genel türler ve null atanabilirlik, Xcode 7'de tanıtılan dahil olmak üzere yeni Objective-C dil özellikleri için destek
* En son iOS ve Mac SDK'ları için destek.
* Derleme ve Destek ayrıştırma Xcode projesi! Tam bir Xcode projesi hedefi Sharpie şimdi geçirebilirsiniz ve en iyi yapmak için doğru şeyi bulmak için bunu (örneğin `sharpie bind Project.xcodeproj -sdk ios`).
* [CocoaPods](https://cocoapods.org) destek! Şimdi yapılandırabilir, yapı ve doğrudan hedefi Sharpie CocoaPods bağlayın (örneğin `sharpie pod init ios AFNetworking && sharpie pod bind`).
* Çerçeveler bağlanırken, framework bunları framework yapısı tarafından tanımlanan bu yana en doğru bir çerçevesinin ayrıştırma kaynaklanan destekliyorsa Clang modülleri etkinleştirilecek kendi `module.modulemap`.
* Xcode projesinde framework olmayan hedefler otomatik olarak çözümlenemiyor belirsizlikleri seçilmiş olabilir framework Ürün yapı Xcode projeleri için bu ürünü Ara ürün hedefleri yerine ayrıştırma.

## <a name="216-march-17-2015"></a>2.1.6 (17 Mart 2015)

[V2.1.6 indirin](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* Sabit ikili işleç ifade bağlama: sol tarafındaki ifade yanlış sağ ile takas (örneğin `1 << 0` yanlış olarak bağlıydı `0 << 1`). ADAM bu haberiniz bile için Kemp teşekkür ederiz!
* İle ilgili bir sorunu sabit `NSInteger` ve `NSUInteger` olarak bağlanan `int` ve `uint` yerine `nint` ve `nuint` i386 üzerinde; `-DNS_BUILD_32_LIKE_64` ayrıştırma yapmak için Clang için şimdi geçirilen `objc/NSObjCRuntime.h` üzerinde i386 beklendiği gibi çalışmayabilir.
* Mac OS X SDK'ları için varsayılan mimarisi (örneğin `-sdk macosx10.10`) x86_64 yerine i386, bu nedenle sunulmuştur `-arch` varsayılan geçersiz kılma istenen sürece atlanabilir.

## <a name="210-march-15-2015"></a>2.1.0 (15 Mart 2015)

[V2.1.0 indirin](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxc #27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): olun `using ObjCRuntime;` zaman üretilen `ArgumentSemantic` kullanılır.
* [bxc #27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): olun `using System.Runtime.InteropServices;` zaman üretilen `DllImport` kullanılır.
* [bxc #27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): varsayılan `DllImport` sembolleri yükleme için `__Internal`.
* [bxc #27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): İleri bildirilen Objective-C kapsayıcı bildirimleri atlayın.
* [bxc #27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): bağlama somut arabirimleri olarak tek bir niteliğe Protokolü türleriyle (`id<Foo>` olarak `Foo` yerine `Foundation.NSObject<Foo>`).
* [bxc #28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): bağlama `UInt32`, `UInt64`, ve `Int64` değişmez değerler olarak `Int32` bırakmaya `u` ve/veya `uL` sonekleri değerleri güvenle içine sığması zaman `Int32`.
* [bxc #28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): özgün yerel ada sahip başladığında enum adı eşleme düzeltme bir `k` öneki.
* `sizeof` C ifadeleri, bağımsız değişken türü bir C# ilkel türe eşlemiyor Clang içinde değerlendirilir ve geçersiz C# oluşturulmasını engellemek için değişmez değer tamsayı olarak bağlı.
* Objective-C sözdizimi türü olan bir blok özellikleri için düzeltme (Objective-C kodunu görünür açıklamalarında ilişkili bildirimleri yukarıda).
* Decayed türleri kendi özgün türü olarak bağlama (`int[]` için decays `int*` Clang anlamsal analiz sırasında ancak yazılmış özgün bunu bağlamak `int[]` yerine).

Çok fazla Dave Dunkin için bu nokta sürümde düzeltilen çoğunu raporlama için teşekkürler!

## <a name="200-march-9-2015"></a>2.0.0: 9 Mart 2015

[V2.0.0 indirin](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Amaç Sharpie 2.0 geliştirilmiş Clang tabanlı sürücüsü ve Ayrıştırıcı ve yeni bir NRefactory tabanlı bağlama motoru özellikleri önemli bir sürümüdür. Bu geliştirilmiş bileşenleri sağlamak _çok_ daha iyi özellikle türü bağlama geçici bağlamalar. Çok sayıda kullanıcıya görünen özellikleri gelecekteki sürümlerde sunacak hedefi Sharpie iç diğer birçok geliştirmeler yapıldı.

Amaç Sharpie 2.0 3.6.1 Clang üzerinde temel alır.

### <a name="type-binding-improvements"></a>Tür bağlama geliştirmeleri

* Objective-C blokları artık desteklenmektedir. Bu anonim/satır içi blokları içerir ve blokları adlı aracılığıyla `typedef`. Anonim blokları bağlı olarak `System.Action` veya `System.Func` temsilci olarak kesin olarak adlandırılmamış adlandırılmış blokları bağlanacak sırada `delegate` türleri.

* Geliştirilmiş adlandırma buluşsal yöntem tarafından hemen öncesinde anonim numaralandırmalar için yoktur bir `typedef` bir yerleşik tam sayı türü gibi çözme `long` veya `int`.

* C işaretçileri şimdi bağlı C# `unsafe` işaretçileri yerine `System.IntPtr`. Bu bağlama zaman işaretçi parametreleri içine etkinleştirmek isteyebilirsiniz için daha fazla netlik sonuçlanır `out` veya `ref` parametreleri. Her zaman bir parametresi olmalıdır olup olmadığını anlamak mümkün değildir `out` veya `ref`, işaretçinin daha kolay denetime olanak sağlamak için bağlama korunur.

* Objective-C nesnesine 2 derece işaretçi parametre olarak karşılaşıldığında yukarıdaki işaretçi bağlama için bir özel durumdur. Bu durumlarda, kuralı yaygındır ve parametre olarak bağlı `out` (örneğin `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>Öznitelik doğrulayın

Hedefi Sharpie tarafından üretilen bağlamaları şimdi ile Açıklama genellikle bulacaksınız `[Verify]` özniteliği. Bu öznitelikler, gerektiğini belirtmek _doğrulayın_ hedefi Sharpie (, ilişkili bildiriminin üstüne bir yorum sağlanacak) özgün C/Objective-C bildirimine bağlamayla karşılaştırarak doğru olanı vermedi.

Doğrulama _önerilen_ için tüm ilişkili bildirimleri, ancak büyük olasılıkla olan _gerekli_ ile bildirimleri açıklama için `[Verify]` özniteliği. Çoğu durumda olmadığı için yeterli meta verileri en iyi bir bağlama üretmek nasıl anlamak için özgün yerel kaynak kodunda budur. Belge veya kod açıklamaları en iyi bağlama kararı vermeniz başlık dosyalarının içindeki başvuru gerekebilir.

Bkz: [doğrulama öznitelikleri](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) daha fazla ayrıntı için belgeleri.

### <a name="other-notable-improvements"></a>Diğer önemli geliştirmeler

* `using` deyimleri artık bağlı türlerine göre oluşturulur. Örneği için bir `NSURL` bağlı olduğundan, bir `using Foundation;` deyimi de oluşturulur.

* `struct` ve `union` bildirimleri şimdi bağlı, kullanarak `[FieldOffset]` el birleşimler için.

* Enum değerleri sabit ifadesine başlatıcıları ile artık düzgün bağlanacak; C# tam ifade çevrilir.

* Variadic yöntemleri ve blokları şimdi bağlandı.

* Çerçeveler aracılığıyla artık desteklenmektedir `-framework` seçeneği. Belgelere bakın [bağlama yerel çerçeveler](http://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks) daha fazla ayrıntı için.

* Objective-C kaynak kodu olacaktır otomatik olarak algılanır artık, hangi geçirmek gereğini ortadan `-ObjC` veya `-xobjective-c` el ile Clang için.

* Clang modülü kullanımı (`@import`) otomatik olarak algılanır, sunulmuştur hangi geçirmek için gereken ortadan `-fmodules` Clang el ile Clang içinde yeni Modül desteği kullanacağınız kitaplıkları için.

* Xamarin birleşik API şimdi varsayılan bağlama hedefidir; kullanmak `-classic` 32-bit yalnızca klasik API hedeflemek için seçeneği.

### <a name="notable-bug-fixes"></a>Önemli hata düzeltmeleri

* Düzeltme `instancetype` Objective-C kategorisinde kullanıldığında bağlama
* Tam ad Objective-C kategorileri
* Objective-C protokollerle önek `I` (örneğin `INSCopying` yerine `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 21 aralık 2014

[V1.1.35 indirin](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

İkincil hata düzeltmeleri.

## <a name="111-december-15-2014"></a>1.1.1: 15 Aralık 2014

[V1.1.1 indirin](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 dahili kullanım ve Xamarin Nisan 2013'te hedefi Sharpie ilk önizlemesi şu anda geliştirme 1,5 yıl sonra ana ilk sürümdü. Kararlı ve çok çeşitli yeni bir Clang arka özelliğe sahip olan yerel kitaplıkları için kullanılabilir genellikle olarak kabul edilmesi için ilk sürümüdür.

