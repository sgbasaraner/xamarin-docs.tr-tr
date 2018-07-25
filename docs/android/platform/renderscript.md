---
title: Renderscript giriş
description: Bu kılavuz, Renderscript tanıtır ve bu hedefleri API düzeyi 17 veya daha yüksek iç Renderscript API'SİNİN bir Xamarin.Android uygulamasındaki nasıl kullanıldığını açıklar.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 3331eb579f0aa2d7f29508773c588455c134f56a
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241194"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript giriş

_Bu kılavuz, Renderscript tanıtır ve bu hedefleri API düzeyi 17 veya daha yüksek iç Renderscript API'SİNİN bir Xamarin.Android uygulamasındaki nasıl kullanıldığını açıklar._

## <a name="overview"></a>Genel Bakış

Renderscript kapsamlı hesaplama kaynaklarını gerektiren Android uygulamalarının performansını iyileştirme amacıyla Google tarafından oluşturulan bir programlama çerçevesidir. Düşük düzey, yüksek bir performans temel API olduğu [C99](http://en.wikipedia.org/wiki/C99). Düşük düzeyde CPU, GPU'ları veya DSPs üzerinde çalışacak bir API olduğundan, Renderscript aşağıdakilerden herhangi birini gerçekleştirmek gerekebilir Android uygulamaları için uygundur:

* Grafikler
* Görüntü işleme
* Şifreleme
* Sinyal işleme
* Matematik yordamları

Renderscript kullanacağı `clang` ve betikleri APK paketlenmiş LLVM bayt kodu derleyin. Uygulamayı ilk kez çalıştırdığınızda, cihaz üzerindeki için makine koda LLVM bayt kod derlenir. Bu mimari, her işlemci için cihazda kendilerini yazmak zorunda makine kodu geliştiricilere avantajlarından yararlanmak için bir Android uygulamasına izin verir.

Renderscript yordama iki bileşeni vardır:

1. **Renderscript çalışma zamanı** &ndash; Renderscript yürütmek için sorumlu olan yerel API'lerin budur. Bu uygulama için yazılan tüm Renderscripts içerir.

2. **Android çerçevesi yönetilen sarmalayıcıları** &ndash; yönetilen denetlemek ve Renderscript çalışma zamanı ve betikleri ile etkileşim kurmak bir Android uygulaması sağlayan sınıflar. Renderscript çalışma zamanı denetlemek için framework tarafından sağlanan sınıflarının yanı sıra, Android araç zinciri Renderscript kaynak kodunu inceleyin ve Android uygulama tarafından kullanım için yönetilen sarmalayıcı sınıfları oluşturur.

Aşağıdaki diyagram, bu bileşenlerin nasıl ilişkili olduğunu gösterir:

![Android çerçevesi Renderscript çalışma zamanı ile nasıl etkileştiğini gösteren diyagram](renderscript-images/renderscript-01.png)

Bir Android uygulaması'nda Renderscripts kullanma için üç önemli kavram vardır:

1. **Bir bağlam** &ndash; yönetilen Renderscript için kaynakları ayırır ve Renderscript veri alıp geçirmek Android uygulamasını sağlayan Android SDK tarafından sağlanan API.

2. **A _çekirdek işlem_**  &ndash; olarak da bilinen _kök çekirdek_ veya _çekirdek_bu çalışır bir yordam. Çekirdek C işlevine çok benzer; ayrılmış bellekteki tüm verileri üzerinde çalıştırılacak paralelleştirilebilir bir yordamdır.

3. **Ayrılan bellek** &ndash; çekirdek gelen ve giden veri geçirilen bir  _[ayırma](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_. Bir çekirdek bir giriş olabilir ve/veya bir ayırma çıkış.

[Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) ad alanı Renderscript çalışma zamanı ile etkileşim kurmaya yönelik sınıflar içerir. Özellikle, [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) sınıfı Renderscript altyapı kaynaklarını ve yaşam döngüsü yönetecektir. Android uygulaması bir veya daha fazla başlatmalıdır [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) nesneleri. Ayırma, ayırma ve Android uygulaması ve Renderscript çalışma zamanı arasında paylaşılan bellek erişim sorumlu olduğu yönetilen bir API'dir. Genellikle, bir ayırma için giriş oluşturulur ve çekirdek çıktısını tutmak için isteğe bağlı olarak başka bir ayırma oluşturulur. Renderscript çalışma zamanı motoru ve ilişkili yönetilen sarmalayıcı sınıfları Tahsisatlar tarafından tutulan bellek erişimi yönetir, hiçbir ek iş yapması bir Android uygulama geliştiricisi için gerek yoktur.

Bir veya daha fazla ayırma içerecek [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/).
Her ayırma verileri tanımlayan özel bir tür öğeleridir.
Öğe türleri ayırma eşleşmelidir çıktı türleri input öğesi. Yürütülürken, bir Renderscript her öğesine paralel giriş ayırma üzerinden yineleme yapma ve sonuçları çıktı yazma ayırma. Öğelerin iki tür vardır:

- **Basit tür** &ndash; kavramsal olarak bu C veri türü ile aynı olur `float` veya `char`.

- **Karmaşık tür** &ndash; bu tür bir C benzer `struct`.

Renderscript altyapısı, her ayırma öğeleri çekirdek tarafından gerekli ile uyumlu olmasını sağlamak için bir çalışma zamanı denetimi gerçekleştirir. Ayırma öğelerin veri türü çekirdek bekleniyor veri türü eşleşmiyorsa bir özel durum oluşturulur.

Bir alt öğesi olan bir tür tarafından tüm Renderscript çekirdekler sarmalanır [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/) sınıfı. `Script` Sınıfı için bir Renderscript parametrelerini ayarlamak için uygun ayarlayın kullanılır `Allocations`, ve Renderscript çalıştırın. İki `Script` Android SDK'sı sınıflarında:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; En yaygın Renderscript görevlerden bazılarını Android SDK'yı paketlenir ve öğesinin tarafından erişilebilir [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) sınıfı. Bir geliştirici almak için zaten sağlanan bu betikleri uygulamalarında kullanmak için herhangi bir ekstra adım gerek yoktur.

- **`ScriptC_XXXXX`** &ndash; Olarak da bilinen _kullanıcı betikleri_, bunlar Geliştiricileriniz tarafından geliştirilen ve APK paketlenmiş betiklerdir. Derleme zamanında Android uygulamasında kullanılmak üzere komut dosyalarını tanıyacak yönetilen sarmalayıcı sınıfları Android araç zinciri oluşturur.
  Oluşturulan sınıfın adı ön ekine sahip Renderscript dosyanın adıdır `ScriptC_`. Yazma ve kullanıcı komut dosyaları ekleme, bu kılavuzun kapsamı dışındadır ve Xamarin.Android resmi olarak desteklenmez.

Bu iki tür, yalnızca `StringIntrinsic` Xamarin.Android tarafından desteklenir. Bu kılavuz, bir Xamarin.Android uygulamasına iç betiklerini kullanma işlemi ele alınmaktadır.

## <a name="requirements"></a>Gereksinimler

Bu kılavuz, hedef API düzeyi 17 veya üzeri Xamarin.Android uygulamalarında kullanılır. Kullanımını _kullanıcı betikleri_ bu kılavuzda ele alınmamaktadır.

[Xamarin.Android V8 destek kitaplığı](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) backports ' % s'instrinsic Renderscript API'nin Android SDK'sının daha eski sürümlerini hedefleyen uygulamalar için. Bu paket için bir Xamarin.Android projesi ekleme uygulamaları, eski sürümlerini hedefleyen iç betikleri yararlanmak için Android SDK'sının izin vermesi gerekir.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Xamarin.Android iç Renderscripts kullanma

İç komut en az miktarda bir ek kod ile bilgi işlem açısından yoğun kaynak gerektiren görevleri gerçekleştirmek için harika bir yoludur. Bunlar, cihazların büyük bir çapraz bölüm üzerinde en iyi performans sunmak için ayarlanmış el olmuştur.
10 kat daha hızlı bir şekilde yönetilen kod ve özel bir C uygulaması daha sonra 2-3 x kez çalıştırmak bir iç betiği için sık karşılaşılan bir durum değil. Tipik bir işleme senaryoların çoğunun iç betikler tarafından ele alınmaktadır. İç komut listesi Xamarin.Android geçerli betiklerde açıklanmaktadır:

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; 3B arama tablosu kullanarak RGBA için RGB dönüştürür. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh performans Renderscript API'leri için [BLAS](http://www.netlib.org/blas/). (Temel doğrusal Cebir alt programlar) BLAS temel vektör ve matris işlemlerini gerçekleştirmek için standart yapı taşlaı sağlayan yordamları ' dir. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; iki ayırmalarını birlikte karıştırır.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; Gauss Bulanıklaştırma ayırma için geçerlidir.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; (yani hue ayarlamak değişiklik renkler) bir ayırma için renk matrisi geçerlidir.

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; 3 x 3 renk matrisi ayırma için geçerlidir.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; 5 x 5 renk matrisi ayırma için geçerlidir.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; iç histogram filtre.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; kanal başına arama tablosu bir arabellek için geçerlidir.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; 2B bir ayırma boyutlandırma gerçekleştirmek için komut dosyası.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; YUV arabellek için RGB dönüştürür.

Her iç betikleri hakkında ayrıntılı bilgi için lütfen API belgelerine başvurun.

Bir Android uygulaması'nda Renderscript kullanma temel adımları daha sonra açıklanmaktadır.

**Renderscript bağlam oluşturma** &ndash; [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) Renderscript bağlam çevresinde yönetilen sarmalayıcı bir sınıftır denetlemenizi kaynak yönetimi, başlatma ve temizleme. Renderscript nesnesi kullanılarak oluşturulan `RenderScript.Create` (örneğin, bir etkinliği) Android bir bağlam parametresi olarak alan fabrika yöntemi. Aşağıdaki kod satırını Renderscript bağlamı başlatılamıyor gösterilmektedir:

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Ayırmaları oluşturma** &ndash; iç betik bağlı olarak, bir veya iki tane oluşturmak gerekli olabilir `Allocation`s. [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) Sınıfı bir ayırma için bir iç örnekleme ile yardımcı olmak için birkaç Fabrika yöntemleri vardır. Örneğin, aşağıdaki kod parçacığı ayırma için bit eşlemler oluşturma işlemini gösterir.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

Genellikle, bu oluşturmak için gerekli olacaktır bir `Allocation` bir betiğin çıktı verilerini tutacak. Bu aşağıdaki kod parçacığını nasıl kullanılacağını gösterir `Allocation.CreateTyped` ikinci bir örneğini oluşturmak için yardımcı `Allocation` aynı aslı yazın:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**Komut dosyası kapsayıcı örneği** &ndash; iç betik Sarmalayıcı sınıfların her birini yardımcı yöntemler olmalıdır (genellikle adlı `Create`) komut dosyaları için bir sarmalayıcı nesnesini örnekleme için. Aşağıdaki kod parçacığı örneği oluşturmak nasıl bir örneğidir bir `ScriptIntrinsicBlur` nesnenin Bulanıklığı. `Element.U8_4` Yardımcı yöntem 4 alan 8 bitlik işaretsiz tamsayı değerleri, verileri tutmak için uygun olan bir veri türü tanımlayan bir öğe oluşturur bir `Bitmap` nesnesi:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Allocation(s), ayarlanan parametreler & betiğini Çalıştır Ata** &ndash; `Script` sağlar sınıfını bir `ForEach` Renderscript çalıştırdığı için yöntemi. Bu yöntem her yineleme `Element` içinde `Allocation` girdi verilerini tutan. Bazı durumlarda sağlamak gerekli olabilir bir `Allocation` çıktısını tutan.
`ForEach` Çıkış içeriğin üzerine yazılacağı ayırma. Bu örnek önceki adımdan kod parçacıklarını yürütmek için bir giriş ayırma atamak, bir parametre ve son olarak komut dosyasını çalıştırmak nasıl gösterir (çıktı sonuçları kopyalama ayırma):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Kullanıma isteyebilirsiniz [Renderscript görüntüyle bulanıklaştıran](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript) tarif, içinde Xamarin.Android iç bir betik kullanmayı, tam bir örnek verilmiştir.

## <a name="summary"></a>Özet

Bu kılavuz, Renderscript ve bir Xamarin.Android uygulamasına kullanma kullanıma sunulmuştur. Kısaca Renderscript nedir ve bir Android uygulaması içinde nasıl çalıştığı açıklanmıştır. Bazı anahtar bileşenleri Renderscript ve arasındaki farkı açıklanan _kullanıcı betikleri_ ve _instrinsic betikleri_. Son olarak, bu kılavuz, bir Xamarin.Android uygulaması'nda iç bir komut dosyası kullanma adımları açıklanmıştır.



## <a name="related-links"></a>İlgili bağlantılar

- [Android.Renderscripts ad alanı](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Görüntüyü Renderscript bulanıklaştıran](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [Öğretici: Renderscript ile çalışmaya başlama](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
