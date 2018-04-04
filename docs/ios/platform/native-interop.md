---
title: Yerel kitaplıkları başvurma
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/28/2016
ms.openlocfilehash: 4d58e869dc1357faef71ea88ed6b5ea30aaf960d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="referencing-native-libraries"></a>Yerel kitaplıkları başvurma

Xamarin.iOS yerel C kitaplıkları ve Objective-C kitaplıkları ile bağlama destekler. Bu belgeyi yerel C Kitaplıklarınızı Xamarin.iOS projenizle bağlanma açıklanır. Objective-C kitaplıkları için aynı yapılması hakkında daha fazla bilgi için bkz: bizim [bağlama Objective-C türleri](~/ios/platform/binding-objective-c/index.md) belge.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Evrensel yerel kitaplıkları (i386, ARMv7 ve ARM64) oluşturma

Genellikle her iOS geliştirme için desteklenen platformları için yerel Kitaplıklarınızı oluşturmak için tercih edilir (aygıtlar kendileri için Simulator ve ARMv7/ARM64 i386). Kitaplığınızda bir Xcode projesi zaten var, bunu yapmak için gerçekten önemsiz olur.

Yerel kitaplığınızın i386 sürümünü oluşturmak için terminal durumundan aşağıdaki komutu çalıştırın:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

Bu yerel bir statik kitaplık altında sonuçlanır `MyProject.xcodeproj/build/Release-iphonesimulator/`. Kitaplık arşiv dosyasına (libMyLibrary.a) benzersiz bir ad verip daha sonra kullanmak için güvenli bir yer kopyalayın (veya taşıma) (gibi **libMyLibrary i386.a**) böylece şunları yapacaksınız aynı kitaplığı arm64 ve armv7 sürümleriyle artar değil sonraki oluşturun.

Yerel kitaplığınızın ARM64 sürümünü oluşturmak için aşağıdaki komutu çalıştırın:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

Yerleşik yerel kitaplığı oluşturulur bu kez `MyProject.xcodeproj/build/Release-iphoneos/`. Bir kez daha, kopyalama (veya taşıma) bu dosyayı gibi bir yeniden adlandırma güvenli bir konuma **libMyLibrary arm64.a** böylece artar olmaz.

Şimdi kitaplığının ARMv7 sürümü oluşturun:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Sonuçta elde edilen kitaplık dosyasını kopyalamanız (veya taşımanız) aynı konuma kitaplığı diğer 2 sürümleri gibi bir yeniden adlandırma taşıdığınız **libMyLibrary armv7.a**.

Evrensel ikili yapmak için kullanabileceğiniz `lipo` aracı şu şekilde:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

Bu oluşturur `libMyLibrary.a` tüm iOS geliştirme hedefler için kullanılacak uygun bir evrensel (fat) kitaplık olacaktır.


### <a name="missing-required-architecture-i386"></a>Eksik gerekli mimarisi i386

Alıyorsanız bir `does not implement methodSignatureForSelector` veya `does not implement doesNotRecognizeSelector` çalışma zamanı iOS simülatörü'nü, kitaplığınızın Objective-C kitaplıkta büyük olasılıkla tüketmeye çalışırken çıkış iletisinde i386 mimarisi derlenmiş değildir (bkz [yapı Evrensel Yerel kitaplıkları](#building_native) yukarıdaki bölümde).

Belirli bir kitaplık tarafından desteklenen mimariler denetlemek için Terminal aşağıdaki komutu kullanın:

```bash
lipo -info /full/path/to/libraryname.a
```

Burada `/full/path/to/` tüketilen kitaplığı tam yolu ve `libraryname.a` kitaplığı söz konusu adıdır.

Kitaplık kaynağı varsa, uygulamanıza iOS simülatörü test etmek isterseniz, derlemek ve de i386 mimarisi paketini gerekir.

### <a name="linking-your-library"></a>Kitaplığınızı bağlama

Tükettiğiniz herhangi bir üçüncü taraf kitaplığı ile uygulamanızı statik olarak bağlantılı gerekir. 

Statik bağlantı kitaplığı "Internet veya yapı Xcode ile aldığınız libMyLibrary.a" istediyseniz, birkaç şey yapmanız gerekir:

-  Projenize kitaplığını Getir
-  Bağlantı kitaplığı Xamarin.iOS yapılandırın
-  Yöntemleri kitaplıktan erişin.


İçin **kitaplığını projenize Getir**, Çözüm Gezgini'nde ve tuşuna projeyi seçin **Command + Option + a**. LibMyLibrary.a gidin ve bunu projeye ekleyin. İstendiğinde, Mac için Visual Studio veya Visual Studio projeye kopyalamak için söyleyin. Bunu ekledikten sonra projede libFoo.a bulun, sağ tıklayın ve ayarlayın **yapı eylemi** için **hiçbiri**.

İçin **kitaplığı bağlamak için yapılandırma Xamarin.iOS**, son yürütülebilir dosyanın (kitaplık kendisini değil, ancak son program) için Proje seçenekleri de eklemeniz gerekir **iOS yapı**'s (bunlar ek bağımsız değişken Proje seçeneklerinizi parçası) "-gcc_flags" seçeneğini ve ardından, örneğin, program için gerekli olan tüm ek kitaplıklarını içerir tırnak içine alınmış bir dizeyle:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

Yukarıdaki örnekte bağlayacaksınız **libMyLibrary.a**

Kullanabileceğiniz `-gcc_flags` herhangi bir kümesi, yürütülebilir dosyanın son bağlantı yapmak için kullanılan GCC derleyici geçirmek için komut satırı bağımsız değişkenlerini belirtmek için. Örneğin, bu komut satırı de başvuran CFNetwork framework:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

Yerel kitaplığınızın C++ kodu içeriyorsa, böylece doğru derleyici kullanmak için Xamarin.iOS bilir, de - cxx bayrağı, "ek bağımsız değişkenler" geçmesi gerekir. C++ için önceki seçenekleri aşağıdaki gibidir:

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>C C yöntemlerini erişme&#35;

İos'ta yerel kitaplıkları iki tür vardır:

-  İşletim sisteminin parçası kitaplıkları paylaşılan.

-  Uygulamanızla birlikte sevk statik kitaplıklar.


Bunlardan herhangi birinde tanımlanan yöntemler erişmek için kullandığınız [Mono'nın P/Invoke işlevselliği](http://www.mono-project.com/docs/advanced/pinvoke/) kabaca olan .NET içinde kullanacağınız aynı teknolojiyi olduğu:

-  Çağrılacak istediğiniz C işlevi belirleme
-  İmzası belirleme
-  Yaşadığı içinde hangi kitaplığı belirleme
-  Uygun P/Invoke bildirimi yazma


P/Invoke kullandığınızda ile bağlantı oluşturduğunuzdan kitaplığı yolunu belirtmeniz gerekir. İOS kullanarak paylaşılan kitaplıklar, her iki stillerinizin yolu olabilir veya biz tanımladığınız kolaylık sabitleri kullanabilirsiniz bizim [sabitler sınıfı](https://developer.xamarin.com/api/type/Constants/), bu sabitleri paylaşılan iOS kitaplıkları kapsamalıdır.

Örneğin, C: Bu imzaya sahip Apple'nın Uıkit kitaplığından UIRectFrameUsingBlendMode yöntemini çağırmak istediyseniz

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

P/Invoke bildirimi şöyle olabilir:

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Constants.UIKitLibrary yalnızca olarak tanımlanan bir sabit değer "/ System/Library/Frameworks/UIKit.framework/UIKit", EntryPoint bize isteğe bağlı olarak (UIRectFramUsingBlendMode) dış adını belirtin C#, daha kısa farklı bir ad gösterme sırasında sağlar. RectFrameUsingBlendMode.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>C dylib dosyalarından erişme

C Dylib Xamarin.iOS uygulamanızda kullanmasına gerek varsa, biraz çağırmadan önce gerekli ek kurulum yok `DllImport` özniteliği.

Örneğin, biz varsa bir `Animal.dylib` ile bir `Animal_Version` biz bizim uygulamada çağırma yöntemi, ihtiyacımız bunu kullanmak denemeden önce Xamarin.iOS kitaplığı konumunu bildirmek.

Bunu yapmak için düzenleme `Main.CS` dosya ve şu şekilde görünür yapın:

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Burada `/full/path/to/` tüketilen Dylib tam yolu. Bu kod yerinde biz sonra bağlayabilirsiniz `Animal_Version` yöntemini aşağıdaki şekilde:

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>Statik kitaplıklar

Statik kitaplıklar yalnızca İos'ta kullanabilirsiniz, yoktur, bağlamak için dış paylaşılan kitaplık DllImport özniteliği path parametresinde özel ad kullanması gereken şekilde `__Internal` (adının başında çift alt çizgi karakterleri unutmayın) tersine yol adı.

Bu simgenin bir paylaşılan Kitaplığı'ndan yüklenmeye çalışılırken yerine geçerli programda başvurduğunuzdan yönteminin bakmak için DllImport zorlar.

