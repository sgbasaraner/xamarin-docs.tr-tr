---
title: Platformlar arası uygulamalar içindeki yerel türler ile çalışma
description: Bu makalede, Android veya Windows Phone işletim sistemleri gibi iOS olmayan cihazlara sahip code paylaşıldığı bir platformlar arası uygulamasında yeni iOS (nint, nuint, nfloat) birleşik API yerel türler kullanma yer almaktadır.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: 0b32cb68174183fd094f72a7ab20f7ed52b278ee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Platformlar arası uygulamalar içindeki yerel türler ile çalışma

_Bu makalede, Android veya Windows Phone işletim sistemleri gibi iOS olmayan cihazlara sahip code paylaşıldığı bir platformlar arası uygulamasında yeni iOS (nint, nuint, nfloat) birleşik API yerel türler kullanma yer almaktadır._


64-türleri yerel türler iOS ve Mac API'leri ile çalışır. Android veya Windows üzerinde de çalışan paylaşılan kod yazıyorsanız, Unified türleri dönüştürme paylaşabileceğiniz normal .NET türlerine yönetmek gerekir.

Bu belge paylaşılan ortak kodunuzdan Unified API ile birlikte çalışmak için farklı yolları açıklanmaktadır.

## <a name="when-to-use-the-native-types"></a>Yerel türler kullanma zamanı

Xamarin.iOS ve Xamarin.Mac birleşik API'leri hala dahil `int`, `uint` ve `float` veri türlerinin yanı sıra `RectangleF`, `SizeF` ve `PointF` türleri. Bu var olan veri türleri, tüm paylaşılan, platformlar arası kodda kullanılacak devam etmelidir. Yeni yerel veri türleri yalnızca mimarisi algılayan türleri gerekli olduğu desteği Mac veya iOS API çağrısı yaparken kullanılmalıdır.

Paylaşılan kod yapısına bağlı zamanlar olabilir, platformlar arası kod gerekebilir uğraşmanız `nint`, `nuint` ve `nfloat` veri türleri. Örneğin: dönüşümleri kullanarak önceden dikdörtgen verileri işleyen bir kitaplık `System.Drawing.RectangleF` işlevselliği bir uygulama Xamarin.iOS ve Xamarin.Android sürümleri arasında paylaşmak için İos'ta yerel türler işlemek için güncelleştirilmesi gerekir.

Bu değişiklikleri nasıl işleneceğini boyutu ve uygulama karmaşıklığına bağlıdır ve izleme bölümlerde göreceğiz olarak kod, paylaşımı form kullanıldı.

## <a name="code-sharing-considerations"></a>Dikkat edilecek noktalar paylaşımı kodu

İçinde belirtildiği gibi [paylaşımı kodu seçenekleri](~/cross-platform/app-fundamentals/code-sharing.md) belge, platformlar arası projeler arasında kod paylaşımını iki ana yolu vardır: paylaşılan projeleri ve taşınabilir sınıf kitaplıkları. İki tür hangisinin kullanılmış, platformlar arası kodda yerel veri türleri işlenirken sahibiz seçenekleri sınırlar.

### <a name="portable-class-library-projects"></a>Taşınabilir Sınıf Kitaplığı projelerinde

Taşınabilir sınıf kitaplığı (PCL) desteklemek ve platforma özgü işlevselliği sağlamak için arabirimleri kullanmak için istediğiniz platformları hedeflemesi için sağlar.

Aşağı PCL proje türü derlenmiş bu yana bir `.DLL` ve birleşik API hiçbir duygusu vardır, var olan veri türleri kullanmaya devam etmek için zorunlu (`int`, `uint`, `float`) PCL PCLs çağrıları kaynak kodu ve tür dönüştürme sınıflar ve yöntemler ön uç uygulamaları. Örneğin:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Paylaşılan projeleri

Kaynak kodunuz sonra otomatik olarak dahil ve derlenmiş ayrı bir projede tek tek platforma özgü ön uç uygulamalarda düzenlemek ve kullanmak paylaşılan varlık proje türü sağlar `#if` yönetmek için gerektiği şekilde derleyici yönergeleri platforma özgü gereksinimler.

Ön karmaşıklığına ve boyutu bitiş paylaşılan kodu, boyutu ve karmaşıklığı paylaşılmasını kod ile birlikte kullanma, destek yerel veri yöntemi seçme ile platformlar arası yazdığında dikkate alınması gereken mobil uygulamaları Paylaşılan varlık proje türü seçin.

Aşağıdaki etkenlere bağlı olarak, aşağıdaki çözümleri türlerini kullanarak uygulanabilir `if __UNIFIED__ ... #endif` koduna Unified API belirli değişiklikler işlenecek derleyici yönergeleri.

#### <a name="using-duplicate-methods"></a>Yinelenen yöntemlerini kullanma

Yukarıda verilen dikdörtgen veri dönüşümler bulunurken bir kitaplık örneği alın. Kitaplık yalnızca bir veya iki çok basit yöntemleri içeriyorsa, bu yöntemleri yinelenen sürümlerini Xamarin.iOS ve Xamarin.Android için oluşturmak tercih edebilirsiniz. Örneğin:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

Yukarıdaki kod bu yana `CalculateArea` yordamdır çok basit, biz koşullu derleme kullanılan ve oluşturulan ayrı, yöntemin Unified API sürümü. Kitaplık birçok yordamları veya birkaç karmaşık yordamları içeriyorsa, tüm yöntemleri değişiklikler veya hata düzeltmeleri için eşitlenmiş şekilde kalmasının bir sorun var gibi diğer yandan, bu çözüm uygun olmayacaktır.

#### <a name="using-method-overloads"></a>Yöntemini kullanan aşırı yüklemeler

Bu durumda, böylece artık aldıkları 32 bit veri türlerini kullanma yöntemleri aşırı sürümü oluşturmak için çözüm olabilir `CGRect` parametre ve/veya dönüş değeri, bu değerine dönüştürmek bir `RectangleF` (Bu dönüştürme bilerek `nfloat` için`float` kayıplı dönüştürme) ve gerçek iş yapmak için yordamı özgün sürümü çağırın. Örneğin:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

            // Call original routine to calculate area
            return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

Duyarlık kaybına uygulamanızın özel gereksinimlerinizi karşılamak için sonuçları etkilemez sürece yeniden, bu iyi bir çözümdür.

#### <a name="using-alias-directives"></a>Diğer ad yönergeleri kullanma

Bir sorun olduğu duyarlık kaybına alanları için başka bir olası çözüm kullanmaktır `using` herhangi bir dönüştürme ve paylaşılan kaynak kodu dosyaları üstüne aşağıdaki kodu dahil yerel ve CoreGraphics veri türleri için diğer ad oluşturmak için yönergeleri gerekli `int`, `uint` veya `float` değerler `nint`, `nuint` veya `nfloat`:

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

Böylece bizim örnek kod sonra alır:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

Unutmayın ki burada değişmiş `CalculateArea` yöntemi dönüş bir `nfloat` standart yerine `float`. Böylece biz çalışılırken bir derleme hatası elde edemezsiniz yapıldığı _örtük olarak_ Dönüştür `nfloat` bizim hesaplamanın sonucu (her iki değerin de çarpılan olduğundan `nfloat`) içine bir `float` dönüş değeri.

Kod derlenmiş ve bir harici Unified API cihazda çalıştırırsanız `using nfloat = global::System.Single;` eşlemeleri `nfloat` için bir `Single` hangi örtük olarak dönüştürmek için bir `float` çağırmak kullanıcı ön uç uygulama izin verme `CalculateArea` olmadan yöntemi değişikliği.


#### <a name="using-type-conversions-in-the-front-end-app"></a>Ön uç uygulamada tür dönüştürmeleri kullanma

Ön uç yalnızca paylaşılan kod kitaplığınıza çağrıları sayıda uygulamalarınızı gelmesi durumunda, başka bir çözüm değişmeden kitaplığı bırakmayı olabilir ve atama Xamarin.iOS veya Xamarin.Mac uygulamada var olan yordamı çağrılırken yazın. Örneğin:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Kullanıcı uygulama paylaşılan kod kitaplığı çağrıları yüzlerce yaparsa, bu yeniden olabilir iyi bir çözüm.

Bizim uygulamanın mimarisine bağlı olarak, biz yerel veri desteklemek için bir veya daha fazla yukarıdaki çözümleri kullanarak bitirebilirsiniz (gerektiğinde) platformlar arası kodumuza türleri.


## <a name="xamarinforms-applications"></a>Xamarin.Forms uygulamalar

Aşağıdakiler için de bir birleşik API uygulaması ile paylaşılacak platformlar arası Uı'lar Xamarin.Forms kullanmak için gereklidir:

- Çözümün tamamında sürüm 1.3.1 kullanıyor olmanız gerekir (veya daha büyük) Xamarin.Forms NuGet paketi.
- Tüm Xamarin.iOS özel işler için yukarıda sunulan çözümler aynı türde nasıl UI kod paylaşılan (paylaşılan bir proje veya PCL) bırakıldı üzerinde temel kullanın.

Standart bir platformlar arası uygulaması olduğu gibi varolan 32 bit veri türleri bir paylaşılan, platformlar arası kodda çoğu tüm durumlar için kullanılmalıdır. Yeni yerel veri türleri yalnızca mimarisi algılayan türleri gerekli olduğu desteği Mac veya iOS API çağrısı yaparken kullanılmalıdır.

Daha fazla ayrıntı için bkz: bizim [güncelleştirme mevcut Xamarin.Forms uygulamaları](http://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) belgeleri.

## <a name="summary"></a>Özet

Bu makalede bir birleşik API uygulaması ve bunların etkileri platformlar arası yerel veri türleri kullanmamız gereken zaman bakın sunuyoruz. Biz yeni yerel veri türleri platformlar arası kitaplıklarında burada kullanılması gereken durumlarda kullanılan çeşitli çözümler sağlamış. Ve birleşik API Xamarin.Forms platformlar arası uygulamalar desteklemede hızlı bir kılavuz gördük.



## <a name="related-links"></a>İlgili bağlantılar

- [Unified API](~/cross-platform/macios/unified/index.md)
- [Yerel Türler](~/cross-platform/macios/nativetypes.md)
- [Paylaşım kodu seçenekleri](~/cross-platform/app-fundamentals/code-sharing.md)
- [Paylaşım örnek kod](https://developer.xamarin.com/samples/mobile/SharingCode/)
