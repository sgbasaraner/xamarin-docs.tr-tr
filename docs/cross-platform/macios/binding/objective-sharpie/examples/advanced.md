---
title: Gelişmiş (el ile) gerçek örneği
description: Bu belge, başlık altında hedefi Sharpie yaptığı konusunda fikir sağlar hedefi Sharpie girdisi olarak xcodebuild çıktısını kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: c4f7f1e9702fb2ee0f5525343a52e3aacd85d68c
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855266"
---
# <a name="advanced-manual-real-world-example"></a>Gelişmiş (el ile) gerçek örneği

**Bu örnekte [Facebook POP kitaplığından](https://github.com/facebook/pop).**

Bu bölümde, burada kullanacağız Apple'nın bağlama, daha gelişmiş bir yaklaşım ele alınmaktadır `xcodebuild` ilk POP'ye projesi oluşturmak için aracı ve giriş hedefi Sharpie için el ile türetme. Bu temelde, hedefi Sharpie önceki bölümde başlık altında yapıyor da kapsar.

```
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Bir Xcode projesini POP kitaplığı sahip olduğundan (`pop.xcodeproj`), yalnızca kullanabiliriz `xcodebuild` POP oluşturulacak. Bu işlem, sırayla hedefi Sharpie ayrıştırmak gerekebilir üst bilgi dosyaları oluşturabilir. Bu bağlama önemli hale gelmeden önce oluşturmaya neden olur. İle derlerken `xcodebuild` geçirdiğiniz aynı SDK'sı tanımlayıcısı ve mimarisi olan emin olmak için hedefi Sharpie geçirin (ve hedefi Sharpie 3.0 genellikle bunu yapabilirsiniz, unutmayın!) istediğinize:

```
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

Bir parçası olarak çok sayıda derleme bilgileri çıkış konsolunda olacaktır `xcodebuild`. Yukarıda gösterilen şekilde bir "CpHeader" hedef çalıştırıldığı görebiliriz üst bilgi dosyaları derleme çıktı dizinine burada görüntülerle kopyalandı. Bu durum genellikle ve bağlamayı kolaylaştırır: yerel kitaplığın yapı işleminin bir parçası olarak, üst bilgi dosyaları genellikle daha kolay bağını ayrıştırma oluşturan tüketilebilir bir "Genel" konuma kopyalanır. Bu durumda, POP'ın üst bilgi dosyaları olduğunu biliyoruz `build/Headers` dizin.

Artık POP bağlamak hazırız. Derlemek için SDK'sı istiyoruz biliyoruz `iphoneos8.1` ile `arm64` mimarisi ve biz önem verdiğiniz üst bilgi dosyaları olduğunu `build/Headers` POP git checkout altında. Biz bakarsanız `build/Headers` dizin, biz üst bilgi dosyaları sayısını göreceksiniz:

```
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

Baktığımızda `POP.h`, kitaplığın ana üst düzey üst bilgi dosyası olduğu görebiliriz `#import`s diğer dosyalar. Bu nedenle, yalnızca geçirilecek ihtiyacımız `POP.h` hedefi Sharpie için ve clang gerisinde kalan yeterlidir:

```
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

Biz geçirilen görürsünüz bir `-scope build/Headers` hedefi Sharpie bağımsız değişkeni. C ve Objective-C kitaplıklarını gerekir çünkü `#import` veya `#include` bağlamak istediğiniz API'si ve kitaplık uygulama ayrıntıları diğer üst bilgi dosyaları `-scope` bağımsız değişken hedefi tanımlı değil herhangi bir API'yi yok saymak için Sharpie söyleyen bir içinde herhangi bir dosya `-scope` dizin.

Size `-scope` bağımsız değişkeni genellikle indrebilirsiniz uygulanan kitaplıkları için isteğe bağlı, ancak açıkça sağlama içinde hiçbir zarar yoktur.

Ayrıca, belirlemiş `-c -Ibuild/headers`. İlk olarak, `-c` bağımsız değişken belirten komut satırı bağımsız değişkenleri yorumlama durdurmak ve sonraki tüm bağımsız değişkenler geçirmek için hedef Sharpie _clang derleyici için doğrudan_. Bu nedenle, `-Ibuild/Headers` aranacak clang yönlendiren bir clang derleyici bağımsız değişken içeren altında `build/Headers`, burada POP üstbilgileri Canlı olduğu. Bu bağımsız değişken dosyalarını bulmak nereye clang bilmez, `POP.h` olduğu `#import`ing. _Clang geçirmek ne yaptığını anlamak için "hedefi Sharpie kullanarak neredeyse tüm sorunlar" boil_.

### <a name="completing-the-binding"></a>Bağlama Tamamlanıyor

Amaç Sharpie artık oluşturulan `Binding/ApiDefinitions.cs` ve `Binding/StructsAndEnums.cs` dosyaları.

Hedefi Sharpie'nın temel ilk geçişinde bağlama sırasında bunlar ve bazı durumlarda ihtiyacınız olabilir. Ancak yukarıda belirtildiği gibi geliştirici genellikle oluşturulan dosyalar için hedefi Sharpie tamamlandıktan sonra el ile değiştirmeniz gerekir [sorunları giderin](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) , değil otomatik olarak işlenecek araç tarafından.

Güncelleştirme tamamlandıktan sonra bu iki dosyayı artık Visual Studio'da bir bağlama projesi için Mac eklenebilir veya doğrudan geçirilen `btouch` veya `bmac` son bağlamayı oluşturmak için Araçlar.

Bağlama işlemi eksiksiz bir açıklaması için lütfen bkz. bizim [izlenecek tam yol yönergeleri](~/ios/platform/binding-objective-c/walkthrough.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)