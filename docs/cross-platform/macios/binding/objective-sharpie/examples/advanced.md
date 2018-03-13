---
title: "(El ile) gerçek örnek Gelişmiş"
ms.topic: article
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: dc41c70495e40235d7acffa56c1255bfd074ca0a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="advanced-manual-real-world-example"></a>(El ile) gerçek örnek Gelişmiş


**Bu örnekte [Facebook POP kitaplığından](https://github.com/facebook/pop).**


Bu bölüm, burada kullanacağız Apple'nın bağlama, daha gelişmiş bir yaklaşım kapsar `xcodebuild` ilk POP Projeyi derlemek için aracı ve giriş hedefi Sharpie için el ile türetme. Bu, aslında hedefi Sharpie önceki bölümdeki başlık altında yaptıklarını kapsar.

```csharp
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

POP kitaplığı bir Xcode projesi olduğundan (`pop.xcodeproj`), biz yalnızca kullanabilirsiniz `xcodebuild` POP oluşturmak için. Bu işlem, sırayla hedefi Sharpie ayrıştırma gerekebilir üstbilgi dosyaları oluşturabilir. Bu bağlama önemlidir önce derleme neden olur. Aracılığıyla oluştururken `xcodebuild` geçirdiğiniz aynı SDK tanımlayıcısı ve mimari, emin olun, hedefi Sharpie geçirin (ve hedefi Sharpie 3.0 genellikle bunu sizin için unutmayın!) istediğiniz:

```csharp
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1
 
=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===
 
...
 
CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP
 
...
 
** BUILD SUCCEEDED **
```

Bir parçası olarak yapı bilgileri çıkışını konsolunda çok olacaktır `xcodebuild`. Yukarıda gösterildiği bir "CpHeader" hedef çalıştırıldığı görebiliriz üstbilgi dosyaları bir yapı çıktı dizinine; burada görüntülerle kopyalandı. Bu genellikle bir durumdur ve bağlama kolaylaştırır: yerel kitaplığın derleme bir parçası olarak, üst bilgi dosyaları çoğunlukla kolaylaştırmak için bağlama ayrıştırma oluşturan kaynaklarda bir "Genel" konuma kopyalanır. Bu durumda, POP'in başlık dosyalarının içinde olduğunu biliyoruz `build/Headers` dizin.

Biz şimdi POP bağlamak hazır olursunuz. SDK'sı için yapı istediğimizi biliyoruz `iphoneos8.1` ile `arm64` mimarisi ve biz çok önem verdiğiniz üstbilgi dosyaları olduğundan `build/Headers` POP git checkout altında. Biz bakarsanız `build/Headers` dizin, biz üstbilgi dosyaları sayısı göreceksiniz:

```csharp
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Biz bakarsanız `POP.h`, olan kitaplığın ana en üst düzey üstbilgi dosyası görebiliriz `#import`s diğer dosyalar. Bu nedenle, yalnızca geçirmek ihtiyacımız `POP.h` hedefi Sharpie için ve clang arka planda rest yeterlidir:

```csharp
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

Biz geçirilen fark edeceksiniz bir `-scope build/Headers` hedefi Sharpie bağımsız değişkeni. C ve Objective-C kitaplıkları gerekir çünkü `#import` veya `#include` kitaplığı ve bağlamak istediğiniz API uygulaması ayrıntılarını diğer üstbilgi dosyaları `-scope` bağımsız değişkeni hedefi tanımlı değil herhangi bir API'yi yoksaymak için Sharpie söyleyen bir içinde herhangi bir yerde dosya `-scope` dizin.

Size `-scope` bağımsız değişkeni genellikle düzgün bir şekilde uygulanan kitaplıkları için isteğe bağlı, ancak orada açıkça sağlama içinde hiçbir zarar.

Ayrıca, biz belirtilen `-c -Ibuild/headers`. İlk olarak, `-c` bağımsız değişkeni söyler hedefi komut satırı bağımsız değişkenleri yorumlama durdurmak ve sonraki tüm bağımsız değişkenler geçirmek için Sharpie _clang derleyici için doğrudan_. Bu nedenle, `-Ibuild/Headers` clang aramak için bir clang derleyici bağımsız değişkeni içeren altında `build/Headers`, burada POP üstbilgileri Canlı olduğu. Bu bağımsız değişken dosyaları bulmak nereye clang bilmez, `POP.h` olan `#import`lık. _Ne yaptığını clang geçirilecek anlamak için "hedefi Sharpie kullanılırken neredeyse tüm sorunlar" boil_.

### <a name="completing-the-binding"></a>Bağlama Tamamlanıyor

Amaç Sharpie şimdi oluşturursa `Binding/ApiDefinitions.cs` ve `Binding/StructsAndEnums.cs` dosyaları.

Bunlar hedefi Sharpie'nın temel ilk geçişi sırasında bağlama ve bazı durumlarda tek ihtiyacınız olabilir. Ancak yukarıda belirtildiği gibi geliştirici genellikle hedefi Sharpie sona erdikten sonra oluşturulan dosyaları el ile değiştirmeniz gerekecek [sorunları giderin](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , değil otomatik olarak işlenecek aracı tarafından.

Güncelleştirmeler tamamlandıktan sonra bu iki dosyayı şimdi Visual Studio'da bir bağlama projesi için Mac eklenebilir veya doğrudan geçirilen `btouch` veya `bmac` son bağlama oluşturmak için Araçlar.

Bağlama işlemi eksiksiz bir açıklaması için lütfen bkz bizim [izlenecek tam yol yönergeleri](~/ios/platform/binding-objective-c/walkthrough.md).

