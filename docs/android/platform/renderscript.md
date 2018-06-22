---
title: Renderscript giriş
description: Bu kılavuz Renderscript tanıtır ve iç Renderscript API'nin bir Xamarin.Android uygulamasındaki bu hedefleri API düzeyi 17 veya daha yüksek kullanımı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f9e21a51c409c5444f137a63eb2c6fadfef03cbe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772022"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript giriş

_Bu kılavuz Renderscript tanıtır ve iç Renderscript API'nin bir Xamarin.Android uygulamasındaki bu hedefleri API düzeyi 17 veya daha yüksek kullanımı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Renderscript kapsamlı hesaplama kaynaklarını gerektiren Android uygulamalar performansını iyileştirme amacıyla Google tarafından oluşturulan bir programlama çerçevesidir. Düşük düzey ve yüksek bir performans temel API olan [C99](http://en.wikipedia.org/wiki/C99). Düşük düzey CPU, GPU veya DSPs çalışacak API olduğu için Renderscript aşağıdakilerden birini yapmanız gerekebilir Android uygulamaları için uygundur:

* Grafikler
* Görüntü işleme
* Şifreleme
* Sinyal işleme
* Matematik yordamları

Renderscript kullanacağınız `clang` ve betikleri APK paketlenmiş LLVM bayt koduna derleyin. Uygulamasını ilk kez çalıştırdığınızda, cihaz üzerindeki için makine koda LLVM bayt kodu derlenecek. Bu mimari, kendileri için her işlemci cihazda kendilerini okuma ve yazma gerek kalmadan makine kodu geliştiricilere avantajlarından yararlanmak bir Android uygulamasını sağlar.

Renderscript yordamı iki bileşeni vardır:

1. **Renderscript çalışma zamanı** &ndash; Renderscript yürütmek için sorumlu olan yerel API'ları budur. Bu uygulama için yazılmış Renderscripts içerir.

2. **Android çerçevesinden sarmalayıcıları yönetilen** &ndash; yönetilen bir Android uygulaması denetlemek ve komut dosyaları ve Renderscript çalışma zamanı ile etkileşime girmesine izin ver sınıfları. Renderscript çalışma zamanı denetlemek için sağlanan framework sınıfları ek olarak, Android zincirinin Renderscript kaynak kodu inceleyin ve Android uygulama tarafından kullanım için yönetilen sarmalayıcı sınıflar oluşturur.

Aşağıdaki diyagram, bu bileşenlerin nasıl ilişkili olduğunu gösterir:

![Android Framework Renderscript çalışma zamanı ile nasıl etkileşim kurduğunu gösteren diyagram](renderscript-images/renderscript-01.png)

Android uygulamada Renderscripts kullanarak ilgili üç önemli kavramları şunlardır:

1. **Bir bağlam** &ndash; A yönetilen Renderscript için kaynakları ayırır ve geçirmek ve Renderscript veri almak Android uygulaması sağlayan Android SDK tarafından sağlanan API.

2. **A _çekirdek işlem_**  &ndash; olarak da bilinen _kök çekirdek_ veya _çekirdek_, bu işi yapan bir yordam. Çekirdek C işlevine çok benzer; ayrılmış bellek tüm veriler üzerinde çalışan paralelleştirilebilir bir yordamdır.

3. **Bellek tahsis** &ndash; veri çekirdek gelen ve giden geçirilen bir  _[ayırma](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_. Bir çekirdek bir giriş olabilir ve/veya bir ayırma çıkış.

[Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) ad alanı Renderscript çalışma zamanı ile etkileşim için sınıflar içerir. Özellikle, [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) sınıfı yaşam döngüsü ve kaynakları Renderscript altyapısı yöneteceğiniz. Android uygulaması bir veya daha fazla başlatmalıdır [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) nesneleri. Bir ayırma ayırma ve Android uygulaması ve Renderscript çalışma zamanı arasında paylaşılan bellek erişme sorumlu olan yönetilen bir API'dir. Genellikle, bir ayırma için giriş oluşturulur ve isteğe bağlı olarak başka bir ayırma çekirdek çıktısının oluşturulur. Renderscript çalışma zamanı altyapısı ve ilgili yönetilen sarmalayıcı sınıflar ayırmaları tarafından tutulan bellek erişimi yönetir, ek iş yapmak Android uygulama geliştiricisi için gerek yoktur.

Bir veya daha fazla bir ayırma içerecek [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/).
Her ayırma verileri tanımlayan özel bir tür öğelerdir.
Ayırma eşleşmelidir çıktı öğesi türlerini giriş öğesinin türü. Yürütürken bir Renderscript her bir öğesinde paralel giriş ayırma üzerinden yineleme ve sonuçları çıkış yazma ayırma. İki tür öğeler şunlardır:

- **Basit tür** &ndash; kavramsal olarak bu C veri türü ile aynı olur `float` veya `char`.

- **Karmaşık tür** &ndash; bu tür bir C benzer `struct`.

Renderscript altyapısı her ayırma öğelerinde çekirdek tarafından gerekli ile uyumlu olduğundan emin olmak için bir çalışma zamanı denetimi gerçekleştirir. Veri türü ayırma öğeleri çekirdek bekleniyor veri türü eşleşmiyorsa, bir özel durum.

Tüm Renderscript tekrar alt öğesi olan bir türü tarafından sarılır [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/) sınıfı. `Script` Sınıfı için Renderscript parametrelerini ayarlamak, uygun ayarlamak için kullanılan `Allocations`, ve Renderscript çalıştırın. Var olan iki `Script` Android SDK sınıflarında:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Bazı daha yaygın Renderscript görevleri Android SDK'ın paketlenmiştir ve öğesinin bir alt kümesi tarafından erişilebilir [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) sınıfı. Bir geliştirici almak için zaten sağlanan olarak uygulamalarında bu komut dosyalarını kullanmak için hiçbir ek adımlar gerek yoktur.

- **`ScriptC_XXXXX`** &ndash; Olarak da bilinen _kullanıcı komut dosyaları_, bunlar Geliştiricileriniz tarafından geliştirilen ve APK paketlenen betiklerdir. Derleme zamanında Android zincirinin Android uygulamada kullanılacak komut dosyaları sağlayacak yönetilen sarmalayıcı sınıflar oluşturur.
  Bu oluşturulan sınıflar adı önekine sahip Renderscript dosyasının adıdır `ScriptC_`. Yazma ve kullanıcı komut dosyaları ekleme, Xamarin.Android ve bu kılavuzun kapsamı dışında kalan resmi olarak desteklenmez.

Bu iki tür, yalnızca `StringIntrinsic` Xamarin.Android tarafından desteklenir. Bu kılavuz, bir Xamarin.Android uygulaması'nda iç komut dosyaları kullanmayı ele alınacaktır.

## <a name="requirements"></a>Gereksinimler

Bu kılavuz, hedef API düzeyi 17 veya daha yüksek Xamarin.Android uygulamalar için kullanılır. Kullanımını _kullanıcı komut dosyaları_ bu kılavuzda ele alınmamaktadır.

[Xamarin.Android V8 destek kitaplığı](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) backports instrinsic Renderscript API'nin Android SDK'ın eski sürümlerini hedefleyen uygulamalar için. Bu paket için bir Xamarin.Android projesi ekleme uygulamaları Android iç betikleri yararlanmak için SDK'ın bu hedef eski sürümleri izin vermelidir.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>İç Renderscripts Xamarin.Android içinde kullanma

İç komut dosyalarını en az miktarda ek kod yoğun bilgi işlem görevleri gerçekleştirmek için harika bir yoludur. Bunlar büyük arası bölümünde cihazların en iyi performans sağlamak için ayarlanmış el olmuştur.
Yönetilen kod daha hızlı bir şekilde x 10 ve özel bir C uygulamasını daha sonra 2-3 x kez çalıştırmak bir iç komut dosyası için seyrek değil. Tipik işleme senaryoları çoğunu iç komut dosyaları tarafından ele alınmıştır. Bu liste iç komut dosyalarının Xamarin.Android geçerli komut açıklanmaktadır:

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; 3B arama tablosu kullanarak RGBA için RGB dönüştürür. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh performans Renderscript API'leri için [BLAS](http://www.netlib.org/blas/). (Temel Lineer Cebir alt programlar) BLAS temel vektör ve matris işlemleri gerçekleştirmek için standart yapı taşlarını sağlar yordamları ' dir. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; iki ayırmaları birlikte karıştırır.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; Gauss Bulanıklığı bir ayırma için geçerlidir.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; (yani değişiklik renkler ton Ayarla) bir ayırma için renk matrisi uygular.

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; 3 x 3 renk matrisi bir ayırma için geçerlidir.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; 5 x 5 renk matrisi bir ayırma için geçerlidir.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; bir iç histogram Filtresi.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; kanal başına arama tablosu bir arabellek için geçerlidir.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; 2B bir ayırma boyutlandırma gerçekleştirmek için komut dosyası.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; YUV arabellek için RGB dönüştürür.

Her iç komut dosyalarının hakkında ayrıntılı bilgi için lütfen API belgelerine bakın.

Sonraki Renderscript bir Android uygulamasını kullanmaya yönelik temel adımlar açıklanmıştır.

**Renderscript bağlamı oluşturma** &ndash; [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) Renderscript bağlamı çevresinde yönetilen bir sarmalayıcı sınıftır ve denetim başlatma, kaynak yönetimi ve temizleme. Renderscript nesnesi kullanılarak oluşturulan `RenderScript.Create` bir parametre olarak (örneğin, bir etkinlik) bir Android bağlamı alan Üreteç yöntemi. Aşağıdaki kod satırını Renderscript içeriği başlatılamıyor gösterilmiştir:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Ayırmaları oluşturma** &ndash; iç betik bağlı olarak, bir veya iki oluşturmak gerekli olabilir `Allocation`s. [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) Sınıfı bir ayırma için bir iç örnekleme ile yardımcı olmak için birkaç Fabrika yöntemleri vardır. Örnek olarak, aşağıdaki kod parçacığını nasıl bit eşlemler için ayırma oluşturulacağını gösterir.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Genellikle, bu oluşturmak için gerekli olacaktır bir `Allocation` bir komut dosyasının çıktı verileri tutmak için. Bu aşağıdaki kod parçacığında nasıl kullanılacağını gösterir `Allocation.CreateTyped` ikinci bir örneğini oluşturmak için yardımcı `Allocation` aynı özgün olarak yazın:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Komut dosyası sarmalayıcı örneği** &ndash; iç betik Sarmalayıcı sınıfların her biri yardımcı yöntemler olmalıdır (genellikle adlı `Create`) komut dosyaları için bir sarmalayıcı nesnesi örneği için. Aşağıdaki kod parçacığını örneği oluşturmak nasıl bir örneği olan bir `ScriptIntrinsicBlur` nesne ölçeklendirilmelidir. `Element.U8_4` Yardımcı yöntem 4 alanlarının 8 bit işaretsiz tamsayı değerleri, verileri tutmak için uygun olan bir veri türü tanımlayan bir öğe oluşturacak bir `Bitmap` nesnesi:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Allocation(s), ayarlanan parametreler & komut dosyasını çalıştır Ata** &ndash; `Script` SAX bir `ForEach` Renderscript gerçekten çalıştırılacak yöntemi. Bu yöntem her yineleme `Element` içinde `Allocation` giriş verilerini tutan. Bazı durumlarda sağlamak gerekli olabilir bir `Allocation` çıkış tutar.
`ForEach` Çıkış içeriğinin üzerine ayırma. Bu örnek önceki adımlardan gelen kod parçacıkları taşımak için bir giriş ayırma atayabilir, bir parametre ve son olarak komut dosyasını çalıştırmak nasıl gösterir (çıkış sonuçları kopyalama ayırma):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Kullanıma isteyebilirsiniz [Renderscript görüntüyle Ölçeklendirilmelidir](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/) tarif, bir iç komut dosyasının içinde Xamarin.Android nasıl kullanılacağı tam bir örneği verilmiştir.

## <a name="summary"></a>Özet

Bu kılavuz, Renderscript ve bir Xamarin.Android uygulaması kullanma sunulur. Kısaca Renderscript nedir ve bir Android uygulamasını nasıl çalıştığı açıklanır. Bazı Renderscript ve arasındaki farkı anahtar bileşenlerini açıklanan _kullanıcı komut dosyaları_ ve _instrinsic betikleri_. Son olarak, bu kılavuz, bir Xamarin.Android uygulaması'nda iç bir komut dosyası kullanarak adımları açıklanmıştır.



## <a name="related-links"></a>İlgili bağlantılar

- [Android.Renderscripts ad alanı](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Görüntüyü Renderscript ölçeklendirilmelidir](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Öğretici: Renderscript ile çalışmaya başlama](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
