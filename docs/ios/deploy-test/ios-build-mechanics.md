---
title: iOS derleme mekaniği
description: Bu kılavuz, uygulamalarınızı süreyi keşfediyor ve tüm derleme yapılandırmaları için için daha hızlı çalıştırılacağı yöntemlerinin nasıl kullanılacağını oluşturur.
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 4145368281c2967bd1311389e5e1b1432af2c9b8
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "43780573"
---
# <a name="ios-build-mechanics"></a>iOS derleme mekaniği

_Bu kılavuz, uygulamalarınızı süreyi keşfediyor ve tüm derleme yapılandırmaları için için daha hızlı çalıştırılacağı yöntemlerinin nasıl kullanılacağını oluşturur._

Harika uygulamalar geliştirmeye daha fazlasını çalışır yazma kod değil. İyi yazılmış bir uygulama, daha küçük ve daha hızlı çalışan uygulamaları ile daha hızlı yapılar gerçekleştirmek en iyi duruma getirme içermelidir. Daha iyi bir deneyim için kullanıcı, aynı zamanda veya projede çalışan herhangi bir geliştirici bu iyileştirmeler yalnızca açmayacaktır. Uygulamanız ile ilgilenirken her şeyi genellikle zaman aşımına emin olmak için gereklidir. 

Varsayılan seçenekleri güvenli ve hızlı, ancak her durum için en uygun durumda olmayan unutmayın. Ayrıca, birçok seçenek yavaşlamasına veya projeyi bağlı olarak geliştirme döngüsü hızlandırın. Örneğin, yerel şeridi oluşturma zaman alır, ancak çok az boyutu kazanılan, ardından harcanan süre şeridi oluşturma tarafından daha hızlı dağıtma kurtarılmaz. Öte yandan, yerel şeridi oluşturma uygulama önemli ölçüde, bu durumda daha hızlı bir şekilde dağıtmak için de artar küçültebilirsiniz. Bu projeler arasında farklılık gösterir ve öğrenmek için tek yolu test etmektir.

Xamarin derleme hızları da çeşitli kapasiteler tarafından etkilenmez ve can'den bir bilgisayarın özellikleri, performansı etkiler: işlemci özellikleri, veri yolu hızı, fiziksel bellek, disk hızı, ağ hızını miktarı. Bu performans sınırlamalar, bu belgenin kapsamı dışındadır ve geliştiricinin sorumluluğudur.


## <a name="timing-apps"></a>Zamanlama uygulamaları

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio tanılama MSBuild çıktısı etkinleştirmek için:

1. Tıklayın **Mac için Visual Studio > Tercihler...**
2. Soldaki ağaç görünümünde seçin **projeleri > derleme**
3. Sağ bölmede aşağı açılan günlük ayrıntı düzeyini ayarlayın **tanılama**: [ ![](ios-build-mechanics-images/image2.png "günlük ayrıntı düzeyini ayarlama")](ios-build-mechanics-images/image2.png#lightbox)
4. **Tamam**’a tıklayın.
5. Mac için Visual Studio'yu yeniden başlatın
6. Temizleyin ve paketinizi yeniden derleyin
7. Hata paneli içinde tanılama çıkışı görüntülemek (Görüntüle > doldurmalar > hatalar) derleme çıkışı düğmesine tıklayarak


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio tanılama MSBuild çıktısı etkinleştirmek için:

1. Tıklayın **Araçlar > Seçenekler...**
2. Soldaki ağaç görünümünde seçin **projeler ve çözümler > derleme ve çalıştırma**
3. Sağ bölmede ayarlamak *MSBuild derleme çıkışı ayrıntı açılan* için **tanılama**: [ ![](ios-build-mechanics-images/image2-vs.png "MSBuild yapı çıkış Ayrıntı düzeyi")](ios-build-mechanics-images/image2-vs.png#lightbox)
4. **Tamam**’a tıklayın.
5. Temiz ve paketinizi yeniden derleyin.
6. Tanılama çıkışı Çıkış panelinde görünür olur.

-----

## <a name="timing-mtouch"></a>Zamanlama mtouch

Mtouch yapı işlemine özgü bilgileri görüntülemek için geçirmek `--time --time` mtouch bağımsız, **proje seçenekleri**. Sonuçları derleme çıktısında arayarak bulunan `MTouch` görevi:

```csharp
Setup: 36 ms
Resolve References: 982 ms
Extracted native link info: 987 ms
...
Total time: 1554 ms
```

## <a name="connecting-from-visual-studio-with-build-host"></a>Visual Studio'da derleme konağı ile bağlanma

Xamarin Araçları Teknik olarak, OS X Yosemite 10.10 ya da üzerini çalıştıran bir Mac üzerinde de çalışır. Ancak, geliştirici deneyimleri ve derleme zamanlarını Mac performansını tarafından engelliyordu

Bağlantısı kesik durumda Windows üzerinde Visual Studio yalnızca C# derleme aşaması gerçekleştirir ve gerçekleştirmeyi denemez bağlama veya AOT derlemesi paketini uygulamaya, bir _.app_ paket veya uygulama paket grubunu oturum. (C# derleme nadiren bir performans sorununun aşamasıdır.) Burada işlem hattı, yapı Mac için Visual Studio Mac derleme konağı üzerinde doğrudan oluşturarak yavaşlamasıdır saptamak deneyin


Ayrıca, sluggishness için daha yaygın yerlerden biri Windows makinenin Mac derleme konağı arasında ağ bağlantısı olduğunu. Bu bir kablosuz bağlantı ya da (örneğin, bir Mac---bulutta hizmeti) doygun bir makine gezinir gerek kalmadan ağdaki fiziksel bir engel olabilir.

## <a name="simulator-tricks"></a>Simülatör püf noktaları

 
Mobil uygulama geliştirirken kod hızlı bir şekilde dağıtmak için gereklidir. Bir çeşitli nedenlerle hızı ve bir cihaz sağlama gereksinimleri dahil olmak üzere, geliştiriciler genellikle önceden yüklenmiş simülatörü veya öykünücünüze dağıtmak seçin. Geliştirici Araçları'nın üreticileri için kararı simülatörü veya öykünücü sağlamak için bir denge hızını ve uyumluluğunu arasında gelir. 

Apple iOS geliştirme için bir benzetici kodu çalıştırmak için daha az kısıtlayıcı bir ortam oluşturarak uyumluluk hızını yükseltme sağlar. Xamarin için simülatör yalnızca zamanında (JIT) derleyici kullanmak bu daha az kısıtlayıcı bir ortam sağlar (başlangıcı yerine sonundan [AOT](~/ios/internals/architecture.md) bir cihazda), derleme zamanında yerel koda derlenmiş olan anlamına gelir. Mac cihaz çok daha hızlı olduğundan, bu en iyi performansı sağlar.

Simülatör, cihazda gerekli olduğundan her zaman oluşturulan aksine kullanılabilmeleri Başlatıcısı sağlayan bir paylaşılan uygulama Başlatıcısı kullanır.

Aşağıdaki liste, yukarıdaki bilgileri göz önünde bulundurularak oluştururken ve dağıtırken simülatör uygulamanızı en iyi performansı sağlamak için atılması gereken adımlar hakkında bazı bilgileri sağlar.
 
### <a name="tips"></a>İpuçları

- Yapılar için: 
  - Seçimini **en iyi duruma getirme PNG görüntülerini** proje seçeneklerinde seçeneği. Bu iyileştirme, simülatör derlemelerinde için gerekli değildir.
  - Bağlayıcı kümesine **bağlama**. Yürütme için oldukça uzun sürdüğü için bağlayıcının devre dışı bırakılması daha hızlıdır.
  - Paylaşılan uygulama Başlatıcısı'nı kullanarak devre dışı bırakma `--nofastsim` bayrağı simülatör yapılar çok yavaş olması neden olur. Artık gerekli olmadığında bu bayrağı kaldırın.
  - Yerel kitaplıkları kullanma gibi durumlarda paylaşılan simlauncher temel yürütülebilir dosyasını kullanılamayacak ve her derleme için derlenecek uygulamaya özgü çalıştırılabilir olduğundan daha yavaştır.
- Dağıtım için
  - Her zaman mümkün olduğunda çalıştıran simülatör tutun. Uygulamanın, simülatör hazırlıksız başlatma için 12 saniyeye kadar sürebilir.
- Ek ipuçları
  - Yeniden yapılandırmadan önce temizlediğinden, derleme yeniden tercih eder. Kullanılabilir başvurularını kaldırır gibi temizleme uzun zaman alabilir.
  - Simülatör sanal zorlamaz olgu yararlanın. Simülatörde her uygulama başlatıldığında videoları veya projenize dahil diğer varlıklar gibi büyük kaynaklara sahip yüksek maliyetli dosya kopyalama işlemleri oluşturabilirsiniz. Bu dosyalar giriş dizininde yerleştirerek bu pahalı işlemler kaçının ve bunları uygulamanızda tam dosya yolunu başvuru.  
  - Şüpheye düştüğünüzde kullanın `--time --time` değişikliğiniz ölçmek için bayrağı

Aşağıdaki ekran görüntüsünde, iOS seçeneklerinizi simülatör için bu seçenekleri ayarlamak verilmektedir:

[![](ios-build-mechanics-images/image3.png "Ayar seçenekleri")](ios-build-mechanics-images/image3.png#lightbox)

## <a name="device-tricks"></a>Cihaz püf noktaları

Simülatör küçük bir kısmı iOS cihazları için kullanılan bir derleme olduğundan cihaza dağıtım için simülatör, dağıtımına benzer. Cihaz için yapı çok daha fazla adım gerektirir, ancak uygulamanızı en iyi duruma getirmek için ek fırsatları sağlayarak avantajı vardır.

### <a name="build-configurations"></a>Derleme Yapılandırmaları

İOS uygulamaları dağıtırken sağlanan derleme yapılandırmaları vardır. Ne zaman ve neden, iyileştirme bilmeniz gereken her yapılandırmasının iyi anlamış olmanız önemlidir.

 - Hata ayıklama
  - Bu ana yapılandırmayı bir uygulama geliştirildiği sırada kullanılmalıdır ve gerekir, bu nedenle, olabildiğince hızlı.
 - Sürüm
  - Kullanıcılarınıza gönderilen sürüm yapıları olanlardır ve performansa odaklanan üst düzey öneme sahiptir. Sürüm yapılandırmasını kullanırken, PNG dosyaları iyileştirme ve LLVM iyileştirici derleyiciyi kullanmak isteyebilirsiniz.

 
Oluşturma ve dağıtma arasındaki ilişkiyi anlamak önemlidir. Dağıtım süresi, uygulama boyutunun bir işlevdir. Daha büyük bir uygulama dağıtmak için çok uzun sürüyor. Uygulama boyutu en aza indirerek, dağıtım süresini azaltabilirsiniz.

Uygulama boyutu en aza indirerek yapı süresini azaltabilir. Kod uygulamadan kaldırma yerel olarak kullanılmayacak kodu derlerken daha az zaman alacağından budur. Daha hızlı bağlama, daha küçük nesne dosyaları daha küçük bir yürütülebilir dosyayı oluşturmak için daha az sembol oluşturan anlamına gelir. Sahip olmasının nedeni bir çift gelir değerlerini, boşluk, bu nedenle, kaydetme **bağlantı SDK** tüm cihazlar için varsayılan yer almaktadır. 

> [!NOTE]
> **Bağlantı SDK** seçenek yalnızca bağlantı Framework SDK'ları yalnızca veya bağlantı SDK derlemeleri kullanılan IDE bağlı olarak görünebilir.
 

### <a name="tips"></a>İpuçları

- Derleme: 
  - FAT ikili (örn. ARMv7 + ARM64) hızlı tek bir mimari (örneğin, ARM64) oluşturma
  - Hata ayıklama sırasında PNG dosyaları en iyi duruma getirme kaçının
  - Tüm derlemelerin bağlanması göz önünde bulundurun. Her derleme en iyi duruma getirme 
  - Hata ayıklama sembolleri oluşturmayı kullanarak devre dışı bırakma `--dsym=false`. Ancak, bu devre dışı bırakma kilitlenme raporları oluşturulup bu makinede yalnızca symbolicated, anlamına gelir, uyumlu ve yalnızca uygulama çıkartılır değildi olması gerekir.

 
Kaçınılması gereken bazı noktalar şunlardır:

- FAT ikili (debug) 
- Bağlayıcı devre dışı bırak `--nolink` 
- Şeridi oluşturma devre dışı bırakma 
  - Semboller `--nosymbolstrip` 
  - IL (sürüm) `--nostrip`.  
 
Ek ipuçları 

- Simulator'da yeniden derleme tercih gibi 
  - AOT istediğiniz derlemeleri (nesne dosyaları) önbelleğe alınır 
- Hata ayıklama sembolleri dsymutil çalıştıran nedeniyle ve cihazlara yüklemek için daha büyük, ek süre olan yukarı bittikten sonra daha uzun sürer oluşturur. 
- Yayın derlemeleri varsayılan olarak, bir IL Şerit derlemelerin ne yapacağını. Yalnızca biraz zaman alır ve büyük olasılıkla daha küçük bir .app cihazına dağıtırken geri kazanılan.
- Her derlemede (hata ayıklama) büyük statik dosyaları dağıtmak gerekmez 
  - UIFileSharingEnabled (Info.plist) kullanın 
    - Varlıklar bir kez karşıya yüklenebilir 
- Şüpheye düştüğünüzde kullanın `--time --time` değişikliğiniz ölçmek için bayrağı

Aşağıdaki ekran görüntüsünde, iOS seçeneklerinizi simülatör için bu seçenekleri ayarlamak verilmektedir:

[![](ios-build-mechanics-images/image4.png "Ayar seçenekleri")](ios-build-mechanics-images/image4.png#lightbox)

## <a name="using-the-linker"></a>Bağlayıcı kullanma

Uygulamanızı oluştururken mtouch uygulama kullanmayan kod kaldıran yönetilen kod için bir bağlayıcı kullanır. Teorik olarak, bu daha küçük ve bu nedenle daha hızlı derlemeler sağlar. Bağlayıcı hakkında daha fazla bilgi için bkz [İos'ta bağlama](~/ios/deploy-test/linker.md) Kılavuzu.

Bağlayıcısı kullanırken aşağıdaki seçenekleri göz önünde bulundurun:

- Seçme **bağlama** bir cihaz için yapı büyük bir süre sürer ve aynı zamanda daha büyük bir uygulama oluşturur. 
  - Boyut sınırını olmaları durumunda Apple uygulamaları reddeder. Bağımlı `MinimumOSVersion`, bu 60 MB küçük olabilir. 
  - Yerel yürütülebilir dahil edilir. 
  - Kullanarak yoktur çünkü JIT derlemesi (aksine, bir cihazda AOT) kullanılıyor simülatör yapıları için bağlantı daha hızlı.
- Bağlantı SDK varsayılan seçenektir.
- Tüm olabilir kullanmak üzere güvenli özellikle de diğer bir deyişle kodunu kullanarak değil, kendi böyle bir Nuget'i bağlantı veya bileşenleri. Derlemeleri bağlamayan seçerseniz tüm bu hizmetler kodundan potansiyel olarak büyük uygulamalar oluşturma uygulamanızla dahil edilir. 
  - Ancak, isterseniz **bağlantı tüm** dış bileşenler özellikle kullanılması durumunda uygulama kilitlenme. Bu, bazı türleri üzerinde yansıma kullanarak bazı bileşenler kaynaklanır.
  - Statik analiz ve yansıma birlikte çalışmaz. 

Araçları kullanarak gözetildiği uygulama içinde sağlanabilir [ `[Preserve]` özniteliği](~/ios/deploy-test/linker.md). 

Kaynak koduna erişim iznine sahip değilsiniz veya bir araç tarafından oluşturulur ve değiştirmek istiyor musunuz, bunu hala tüm korunması gereken üyeleri ve türleri açıklayan bir XML dosyası oluşturarak bağlanabilir. Daha sonra bayrağı ekleyebilirsiniz `--xml={file.name}.xml` proje seçeneklerinizi işlenme kod öznitelikleri tam olarak kullandığınız artırmadığı.


### <a name="partially-linking-applications"></a>Kısmen uygulamaları bağlama 

Uygulamanızın derleme zamanını en iyi duruma getirmek için uygulamaların, kısmen bağlamak mümkündür:

- Kullanım `Link All` ve bazı bütünleştirilmiş kodları atla 
  - Bazı uygulama boyut iyileştirmesi kaybolur.
  - Kaynak koduna erişim gereklidir.
  - Örneğin `--linkall --linkskip=fieldserviceiOS` .
 
- Kullanın `Link SDK` kullanın ve seçenek `[LinkerSafe]` ihtiyacınız olan derlemeleri özniteliği 
  - Gerekli kaynak koduna erişim.
  - Derleme bağlamak güvenlidir sisteme söyler ve bir Xamarin SDK'sı olduğu gibi sorgulamanıza işlenir.
 
### <a name="objective-c-bindings"></a>Objective-C bağlama 

- Kullanarak `[Assembly: LinkerSafe]` zaman ve boyut özniteliği bağlamalarınızı kaydedebilir.

- SmartLink 
  - Yerel tarafında bitti 
  - Kullanım `[LinkWith (SmartLink=true)]` özniteliği
  - Bu, yerel bağlayıcı karşı bağlama kitaplığından yerel kodun ortadan kaldırılmasına yardımcı olur. 
  - Not, dinamik arama simgeleri bu ile çalışmaz. 

## <a name="summary"></a>Özet

Bu kılavuz, bir iOS uygulaması ve proje derleme yapılandırması ve Seçenekler bağımlı olan dikkate alınması gereken seçenekleri süreyi incelediniz. 

<!-----
# Benchmarks

## Layer 1: building again after making modifications, but _without_ cleaning should be faster 
 
The app should build a bit more quickly if you have only made changes to a subset of the libraries and you do not clean the build before re-deploying. 
 
 
 
### Clean build time 
178 seconds 
 
 
### Build again (without cleaning) after making _no changes_ 
12.5 seconds 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
3 trials: 45 seconds, 43 seconds, 43 seconds 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
 
3 trials: 45 seconds, 45 seconds, 45 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in each of the following files 
 
- ViewIOS/ImageResourcesHelper.cs 
- Sales.Native.Core.Tools/UIComponents/ListView/IListView.cs 
- View.Models/Mailing/MailingModel.cs 
- Sales.Native.Core.IOS.Ext/ServiceInterfaces/AlertDialog/Dialog.cs 
- Sales.Native.Core.Tools.IOS.Ext/BaseViews/BaseNavigationViewController.cs 
- View.Common/Services/DataTransferResult.cs 
 
45 seconds 
 
 
 
 
 
 
## Layer 2: "app thinning" aka "device specific builds" 
 
The idea of "app thinning" is that the IDE will only build the 1 architecture needed for the specific device that you're deploying to (rather than _both_ 32-bit and 64-bit architectures). 
 
As of the latest "Xamarin 4" builds, you can now enable "app thinning" in Visual Studio via the "Project Options -> iOS Build -> Enable device-specific builds" setting. 
 
Or if you prefer you can achieve a similar result by changing the "Project Options -> iOS Build -> Advanced [tab] -> Supported architectures" to select just _one_ architecture (for example ARM64 if you are developing on a 64-bit device). 
 
 
 
(Caveat: I ran the following builds in Visual Studio for Mac on the Mac rather than on the command line.) 
 
### Clean build time without "device specific builds" 
177 seconds 
 
 
 
### Clean build time _with_ "device specific builds"  
2 trials: 106 seconds, 98 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 31 seconds, 31 seconds 
 
 
* * * 
 
 
## Using the same strategy, but explicitly setting "Supported architectures" to select ARM64 _only_ (rather than using "device specific builds") 
 
(These builds were again run on the command line using `xbuild`.) 
 
 
 
### Clean build time with "Supported architectures" set to ARM64 _only_ 
2 trials: 80 seconds, 91 seconds 
 
 
 
### Build again (without cleaning) after changing 1 line in "ViewIOS/ImageResourcesHelper.cs" 
2 trials: 26 seconds, 26 seconds 
 
 
 
 
 
[1] Mac system used for testing: MacBookAir5,2 
 
- 2.0 GHz Core i7 (I7-3667U) 
 
2 Cores with hyper-threading 
 
L2 Cache (per Core): 256 KB 
L3 Cache: 4 MB 
 
- Standard MacBook soldered-in solid-state storage 
 
- 8 GB RAM 
---->


## <a name="related-links"></a>İlgili bağlantılar

- [Blog gönderisi](https://blog.xamarin.com/xamarin-ios-build-improvements/)
- [İos'ta bağlama](~/ios/deploy-test/linker.md)
- [Özel Bağlayıcı Yapılandırması](~/cross-platform/deploy-test/linker.md)
