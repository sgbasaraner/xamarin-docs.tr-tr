---
title: "Birleşik API için bağlama geçirme"
description: "Bu makale varolan Xamarin bağlama Xamarin.IOS ve Xamarin.Mac uygulamalar için birleşik API'ları desteklemek için projesini güncelleştirmek için gerekli adımlar kapsanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2a04dc047674b67b8f21571ed9e7890ddf773f64
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="migrating-a-binding-to-the-unified-api"></a>Birleşik API için bağlama geçirme

_Bu makale varolan Xamarin bağlama Xamarin.IOS ve Xamarin.Mac uygulamalar için birleşik API'ları desteklemek için projesini güncelleştirmek için gerekli adımlar kapsanmaktadır._

## <a name="overview"></a>Genel Bakış

1 Şubat 2015 başlangıç Apple gerektiren tüm yeni gönderileri iTunes ve Mac App Store için 64 bit uygulamalar olmalıdır. Sonuç olarak, yeni Xamarin.iOS veya Xamarin.Mac uygulama yeni birleşik API 64 bit desteklemek için kullanılmasını varolan Klasik MonoTouch ve MonoMac API'leri yerine gerekecektir.

Ayrıca, herhangi bir bağlama Xamarin projeye yeni birleşik bir 64 bit Xamarin.iOS veya Xamarin.Mac projesinde dahil edilecek API'leri desteklemelidir. Bu makalede Unified API kullanmak için mevcut bir bağlama projesini güncelleştirmek için gerekli olan adımları kapsar.

## <a name="requirements"></a>Gereksinimler

Aşağıda sunulan bu makaledeki adımları tamamlamak için gereklidir:

 -  **Mac için Visual Studio** -Mac için Visual Studio en son sürümü yüklü ve geliştirme bilgisayarda yapılandırılmış.
 -  **Apple Mac** - Apple mac, iOS için bağlama projeler derlemek için gereklidir ve Mac bir

Bağlama projeleri, Visual Studio bir Windows makinesinde desteklenmez.

## <a name="modify-the-using-statements"></a>Using deyimleri değiştirme

Birleşik API'ları zamankinden kod Mac ve iOS yanı sıra aynı 32 ve 64 bit uygulamalarını desteklemek ikili sağlayarak arasında paylaşmak için kolaylaştırır. Bırakma tarafından _MonoMac_ ve _MonoTouch_ önekleri ad alanlarını, daha basit paylaşımı Xamarin.Mac ve Xamarin.iOS uygulaması projeler arasında sağlanır.

Sonuç olarak biz bizim bağlama sözleşmeleri birini değiştirmeniz gerekir (ve diğer `.cs` bizim bağlama proje dosyalarına) kaldırmak için _MonoMac_ ve _MonoTouch_ gelen önekleri bizim `using` deyimleri.

Örneğin, aşağıdaki verilen bir bağlama sözleşmede using deyimleri:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

Biz Şerit `MonoTouch` aşağıdaki kaynaklanan öneki:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

Yeniden herhangi için bunun ihtiyacımız `.cs` bağlama Projemizin dosyasında. Bu değişikliği yerinde bağlama Projemizin yeni yerel veri türlerini kullanmak için güncelleştirme sonraki adımdır.

Birleştirilmiş API'si hakkında daha fazla bilgi için lütfen bkz [Unified API](~/cross-platform/macios/unified/index.md) belgeleri. 32 ve 64 bit uygulamalar ve çerçeveler hakkında bilgi destekleme hakkında daha fazla arka plan için bkz: [32 ve 64 bit Platform konuları](~/cross-platform/macios/32-and-64/index.md) belgeleri.

## <a name="update-to-native-data-types"></a>Yerel veri türleri için güncelleştirme

Objective-C eşlemeleri `NSInteger` veri türü için `int32_t` 32 bit sistemler ve çok `int64_t` 64 bit sistemler üzerinde. Bu davranış eşleştirmek için önceki kullanımlarını yeni birleşik API değiştirir `int` (.NET içinde tanımlanan her zaman olduğu `System.Int32`) için yeni bir veri türü: `System.nint`.

Yeni birlikte `nint` veri türü, Unified API sunar. `nuint` ve `nfloat` eşleme için türleri `NSUInteger` ve `CGFloat` de türleri.

Yukarıda verilen, API'mize gözden geçirin ve herhangi bir örneğine sağlamak ihtiyacımız `NSInteger`, `NSUInteger` ve `CGFloat` biz önceden eşlenmiş `int`, `uint` ve `float` yeni güncelleştirilmesi `nint`, `nuint` ve `nfloat` türleri.

Örneğin, bir Objective-C yöntemi tanımını verilen:

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

Önceki bağlama sözleşme aşağıdaki tanımını varsa:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

Biz olmasını yeni bağlama güncelleştirmeniz gerekir:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
Biz kitaplığına ne biz başlangıçta bağlı daha yeni sürüm 3 taraf eşleme yapıyorsanız, gözden geçirmek ihtiyacımız `.h` üstbilgi dosyaları varsa bakın ve kitaplık için varolan, açık çağrılar `int`, `int32_t`, `unsigned int`, `uint32_t` veya `float` olması için yükseltilmiş bir `NSInteger`, `NSUInteger` veya `CGFloat`. Bu durumda, aynı değişiklikleri `nint`, `nuint` ve `nfloat` türleri kendi eşlemelere yapılması gerekir.

Bu veri türü değişiklikleri hakkında daha fazla bilgi için bkz: [yerel türler](~/cross-platform/macios/nativetypes.md) belge.

## <a name="update-the-coregraphics-types"></a>Güncelleştirme CoreGraphics türleri

İle kullanılan noktası, boyutu ve dikdörtgen veri türleri `CoreGraphics` 32 veya 64 bit bunlar üzerinde çalıştığını aygıt bağlı olarak kullanın. Xamarin iOS ve Mac API'leri ilk olarak bağlı veri türlerinde eşleşecek şekilde oldu mevcut veri yapılarını kullandık `System.Drawing` (`RectangleF` gibi).

64 bit ve yeni yerel veri türlerini desteklemek için gereksinimleri nedeniyle, aşağıdaki ayarlamaları çağrılırken var olan kodu için yapılması gereken `CoreGraphic` yöntemleri:

- **CGRect** -kullanım `CGRect` yerine `RectangleF` zaman kayan tanımlama'nın üzerine dikdörtgen bölgeler.
- **CGSize** -kullanım `CGSize` yerine `SizeF` kayan nokta boyutlarını (genişlik ve yükseklik) tanımlarken.
- **CGPoint** -kullanım `CGPoint` yerine `PointF` ne zaman bir kayan tanımlama izin ver noktası konumu (X ve Y koordinatları).

Yukarıda verilen, API'mize gözden geçirin ve herhangi bir örneğine sağlamak ihtiyacımız `CGRect`, `CGSize` veya `CGPoint` , daha önce bağlandı `RectangleF`, `SizeF` veya `PointF` yerel tür değiştirilmesi `CGRect`, `CGSize` veya `CGPoint` doğrudan.

Örneğin, bir Objective-C Başlatıcı, verilen:

```csharp
- (id)initWithFrame:(CGRect)frame;
```

Bizim önceki bağlama aşağıdaki kodu içeriyorsa:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

Biz bu kodu güncelleştirmeniz gerekir:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

Tüm kod değişiklikleri şimdi yerinde, biz bağlama Projemizin değiştirmek veya karşı Unified API bağlamak için dosya yapmak gerekir.

## <a name="modify-the-binding-project"></a>Bağlama proje değiştirme

Ya da değiştirmek ihtiyacımız Unified API'ları kullanmak için bizim bağlama projesini güncelleştirmek için son adımı olarak `MakeFile` proje veya Xamarin proje türü (biz gelen Visual Studio içinde Mac için bağlıyorsanız) oluşturun ve istemeniz kullandığımız _btouch_  olanları Klasik yerine birleşik API bağlama yapılacak.


### <a name="updating-a-makefile"></a>Derleme görevleri dosyası güncelleştiriliyor

Derleme görevleri dosyası bir Xamarin bizim bağlama Projeyi derlemek için kullanıyoruz durumunda. DLL, biz gerekecektir dahil `--new-style` komut satırı seçeneği ve çağrı `btouch-native` yerine `btouch`.

Bu nedenle aşağıdaki verilen `MakeFile`:

```csharp
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch


all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```

Arama gelen geçiş yapmak ihtiyacımız `btouch` için `btouch-native`, bizim makro tanımı şu şekilde ayarlamanız:

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

Çağrı güncelleştirmek `btouch` ve ekleme `--new-style` gibi seçeneği:

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

Biz şimdi yürütebilir bizim `MakeFile` API'mize yeni 64 bit sürümünü oluşturmak için normal olarak.

### <a name="updating-a-binding-project-type"></a>Bir bağlama proje türü güncelleştiriliyor

Biz API'mize oluşturmak için Visual Studio Mac bağlama proje şablonu için kullanıyorsanız, biz bağlama proje şablonu yeni birleşik API sürümü güncellemek gerekir. Bunu yapmanın en kolay yolu yeni bir birleşik API bağlama projesi ve kopyalama tüm var olan kodu ve ayarları başlatmaktır.

Aşağıdakileri yapın:

1. Mac için Visual Studio'yu başlatın
2. Seçin **dosya** > **yeni** > **çözüm...**
3. Yeni çözüm iletişim kutusunda seçin **iOS** > **Unified API** > **iOS projesi bağlama**: 

    [![](update-binding-images/image01new.png "Yeni çözüm iletişim kutusunda, iOS seçin / birleşik API / bağlama iOS projesi")](update-binding-images/image01new.png#lightbox)
4. 'Yeni projeniz Yapılandır' iletişim kutusundaki girin bir **adı** tıklatın ve yeni bağlama proje için **Tamam** düğmesi.
5. İçin bağlamalar oluşturma olacak Objective-C Kitaplığı 64 bit sürümünü içerir.
6. Varolan 32 bit Klasik API bağlama projenizden kaynak kodunu kopyalayın (gibi `ApiDefinition.cs` ve `StructsAndEnums.cs` dosyaları).
7. Kaynak kodu dosyaları için yukarıdaki not ettiğiniz değişiklikleri yapın.

Tüm yerinde bu değişiklikler, yeni API 64 bit sürümü 32 bit sürümü gibi oluşturabilirsiniz.

## <a name="summary"></a>Özet

Bu makalede Biz yeni birleşik API'ları ve 64 bit aygıtlarda desteklemek için varolan bir Xamarin projesine bağlama yapılması gereken değişiklikleri gösterilen ve bir API yeni 64 bit uyumlu sürümünü oluşturmak için gereken adımlar.



## <a name="related-links"></a>İlgili bağlantılar

- [Mac ve iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64 bit Platform konuları](~/cross-platform/macios/32-and-64/index.md)
- [Var olan iOS uygulamaları yükseltme](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
