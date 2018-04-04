---
title: iOS derleme mekanizması
description: Uygulamalarınızı süreyi bu kılavuzda araştırır ve tüm yapı yapılandırmalarını için daha hızlı çalıştırılacağı yöntemlerinin nasıl kullanılacağını oluşturur.
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: df84e78709b0ff16087c4bb9816c5d45f6ec33ed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="ios-build-mechanics"></a>iOS derleme mekanizması

_Uygulamalarınızı süreyi bu kılavuzda araştırır ve tüm yapı yapılandırmalarını için daha hızlı çalıştırılacağı yöntemlerinin nasıl kullanılacağını oluşturur._

Harika uygulamaları geliştirme çalışan daha fazlasını yazma kodudur. İyi yazılmış bir uygulama, daha küçük ve daha hızlı çalışan uygulamaları ile daha hızlı yapıları gerçekleştirmek iyileştirmeler içermelidir. Bu iyileştirmeler yalnızca daha iyi bir deneyim için kullanıcı, aynı zamanda, veya proje üzerinde çalışan herhangi bir geliştirici sonuçlanır değil. Uygulamanız ile ilgilenirken her şeyi genellikle zaman aşımına emin olmak için gereklidir. 

Varsayılan seçenekleri güvenli ve hızlı, ancak her durum için en uygun durumda olmayan unutmayın. Ayrıca, birçok seçenek yavaşlaması veya geliştirme döngüsü bireysel proje bağlı olarak hızlandırmak. Örneğin, yerel çıkarma zaman alır, ancak çok az boyutu kazanılan, ardından harcanan süre çıkarma göre daha hızlı dağıtma kurtarılacak değil. Diğer taraftan, yerel çıkarma uygulama büyük ölçüde; bu durumda, daha hızlı dağıtmak için olacaktır küçültebilirsiniz. Bu projeleri arasında değişir ve bilmenin tek yolu test etmektir.

Xamarin derleme hızları ayrıca çeşitli kapasiteleri tarafından etkilenecek ve yetenekleri can'den bir bilgisayarın performansı etkiler: işlemci özellikleri, veri yolu hızları, fiziksel bellek, disk hızı, ağ hızını miktarı. Bu performans sınırlamaları bu belgenin kapsamı dışındadır ve geliştirici sorumluluğundadır.


## <a name="timing-apps"></a>Zamanlama uygulamalar

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio tanılama MSBuild çıktısı etkinleştirmek için:

1. Tıklatın **Mac için Visual Studio > tercihleri...**
2. Soldaki ağaç görünümünde seçin **projeleri > derleme**
3. Sağ taraftaki panelinde aşağı açılan günlük ayrıntı düzeyini ayarlamak **tanılama**: [ ![ ] (ios-build-mechanics-images/image2.png "günlük ayrıntı düzeyini ayarlama")](ios-build-mechanics-images/image2.png#lightbox)
4. **Tamam**’a tıklayın.
5. Mac için Visual Studio'yu yeniden başlatın
6. Temizleyin ve paketinizi yeniden derleyin
7. Hataları paneli içindeki tanılama çıktıları görüntülemek (Görünüm > klavye takımı > hatalar) yapı çıktı düğmesini tıklatarak


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio tanılama MSBuild çıktısı etkinleştirmek için:

1. Tıklatın **Araçlar > Seçenekler...**
2. Soldaki ağaç görünümünde seçin **projeler ve çözümler > derleme ve çalıştırma**
3. Sağ taraftaki panelinde ayarlayın *MSBuild derleme çıktısı ayrıntı açılır* için **tanılama**: [ ![ ] (ios-build-mechanics-images/image2-vs.png "MSBuild ayarı yapı çıktı Ayrıntı")](ios-build-mechanics-images/image2-vs.png#lightbox)
4. **Tamam**’a tıklayın.
5. Temizleyin ve paketinizi yeniden derleyin.
6. Tanılama çıktıları Çıktı panelinde görünür olur.

-----

## <a name="timing-mtouch"></a>Zamanlama mtouch

Geçişi mtouch oluşturma işlemi için belirli bilgileri görüntülemek için `--time --time` mtouch bağımsız değişkenler için **proje seçenekleri**. Sonuçları derleme çıktı için arama yaparak bulunan `MTouch` görevi:

```csharp
Setup: 36 ms
Resolve References: 982 ms
Extracted native link info: 987 ms
...
Total time: 1554 ms
```

## <a name="connecting-from-visual-studio-with-build-host"></a>Visual Studio'da derleme ana bilgisayarla bağlanma

Xamarin Araçları Teknik OS X 10.10 Yosemite veya daha sonra çalıştırabileceğiniz tüm Mac üzerinde çalışır. Ancak, geliştirici deneyimleri ve yapı kez Mac performans tarafından engelliyordu

Bağlantısı kesik durumda Visual Studio Windows yalnızca C# derleme aşamayı gerçekleştirir ve gerçekleştirmeyi denemez bağlama veya uygulamaya paketini Uygulama Nesne AĞACI derleme bir _.app_ paket ya da oturum uygulama paketi. (C# derleme nadiren bir performans düşüklüğü aşamasıdır.) Burada ardışık düzeninde yapı Mac için doğrudan konakta Mac yapı Visual Studio'da oluşturarak yavaşlamadan sabitleme girişimi


Ayrıca, sluggishness daha yaygın yerlerde Windows makine Mac yapı konak arasında ağ bağlantısı biridir. Bu, kablosuz bağlantı kullanarak ya da (örneğin, bir Mac-içinde--bulut hizmeti) doymuş makine gezinir gerek kalmadan, ağdaki fiziksel impediment nedeniyle olabilir.

## <a name="simulator-tricks"></a>Simulator püf noktaları

 
Mobil uygulamaları geliştirirken kodunu hızlı bir şekilde dağıtmak için gereklidir. Çeşitli nedenlerle hızı ve cihaz gereksinimleri sağlama eksikliği dahil olmak üzere için geliştiriciler genellikle önceden yüklenmiş simulator veya öykünücü dağıtmak seçin. Geliştirici Araçları üreticileri için kararı benzeticisi veya öykünücü sağlamak için bir denge hızı ve uyumluluk arasında gelir. 

Apple iOS geliştirme için bir benzetici kodu çalıştırmak için daha az kısıtlayıcı bir ortam oluşturarak uyumluluk hızı yükseltme sağlar. Bu daha az kısıtlayıcı ortam yalnızca içinde anında (JIT) derleyici simülatörü kullanmak Xamarin sağlar (tersine [Uygulama Nesne AĞACI](~/ios/internals/architecture.md) bir cihazda), yapı çalışma zamanında yerel koda derlenmiş olan anlamına gelir. Mac aygıt hızlıdır gibi bu daha iyi performans sağlar.

Benzetici, cihazda gerektiği her zaman, yerleşik aksine yüklenmek üzere Başlatıcısı sağlayan bir paylaşılan uygulama Başlatıcısı kullanır.

Yukarıdaki bilgileri göz önünde bulundurularak listesi oluşturma ve uygulamanıza simulator dağıtırken en iyi performansı sağlamak için yapılması gerekenler hakkında bazı bilgileri verir.
 
### <a name="tips"></a>İpuçları

- Derlemeler için: 
  - Seçimini **en iyi duruma getirme PNG görüntüleri** proje seçenekleri seçeneği. Bu iyileştirme simulator üzerinde yapılar için gerekli değildir.
  - Bağlayıcı kümesine **olmayan bağlantı**. Yürütülmekte önemli miktarda zaman aldığından bağlayıcı devre dışı bırakılması daha hızlıdır.
  - Paylaşılan uygulama Başlatıcısı'nı kullanarak devre dışı bırakma `--nofastsim` bayrağı çok daha yavaş olacak şekilde simulator derlemeleri neden olur. Artık gerekli olmadığında bu bayrağını kaldırın.
  - Yerel kitaplıkları kullanarak paylaşılan simlauncher ana yürütülebilir dosya gibi durumlarda yeniden kullanılamaz ve her derleme için derlenmesi uygulamaya özgü yürütülebilir olduğundan daha yavaştır.
- Dağıtım için
  - Her zaman mümkün olduğunda çalıştıran simulator tutun. 12 saniye soğuk başlatmak için simulator devam edebilir.
- Ek ipuçları
  - Yeniden yapılandırmadan önce temizler çünkü derleme yeniden tercih edilir. Kullanılabilecek başvuruları kaldırır temizleme uzun bir süre devam edebilir.
  - Simulator korumalı alan zorlamaz olgu yararlanın. Benzetici her uygulama başlatıldığında videolar veya projenize dahil diğer varlıklar gibi büyük kaynaklarına sahip maliyetli dosya kopyalama işlemleri oluşturabilirsiniz. Bu maliyeti yüksek işlemler bu dosyaları giriş dizininde tarafından yerleştirmez ve bunları uygulamanızda tam dosya yolunu başvuru.  
  - Şüpheli zaman kullanmak `–time –time` değişikliğinizin ölçmek için bayrağı

Aşağıdaki ekran görüntüsünde, iOS seçeneklerinizi simulator ilgili bu seçenekleri ayarlamak verilmektedir:

[![](ios-build-mechanics-images/image3.png "Ayar seçenekleri")](ios-build-mechanics-images/image3.png#lightbox)

## <a name="device-tricks"></a>Cihaz püf noktaları

İOS aygıtı için kullanılan derleme, küçük bir alt simulator olduğu gibi cihaza dağıtma simulator için dağıtımına benzer. Cihaz için yapı pek çok daha fazla adım gerekiyor, ancak uygulamanızı en iyi duruma getirmek için ek fırsatlar sağlama avantajına sahiptir.

### <a name="build-configurations"></a>Derleme yapılandırmaları

İOS uygulamaları dağıtırken sağlanan derleme yapılandırmaları vardır. Ne zaman ve neden, iyileştirme öğrenmek için her yapılandırmasının iyi anlamış olmanız önemlidir.

 - Hata ayıklama
  - Bu, bir uygulama geliştirildiği sırada kullanılmalıdır ve yapmanız gereken, ana yapılandırma bu nedenle, olabildiğince hızlı.
 - Sürüm
  - Yayın derlemeleri, kullanıcılarınıza gönderilen ve performans odaklanmak dönüştürmektir. Yayın yapılandırma kullanılırken, LLVM en iyi duruma getirme derleyici kullanın ve PNG dosyaları en iyi duruma getirmek isteyebilirsiniz.

 
Oluşturma ve dağıtma arasındaki ilişkiyi anlamak önemlidir. Dağıtım süresini uygulama boyutta bir işlevdir. Daha büyük bir uygulama dağıtmak için çok uzun sürüyor. Uygulama boyutu en aza indirerek, dağıtım süresini azaltabilir.

Uygulama boyutu en aza yapı süresini azaltabilir. Bu durum, kod uygulamadan kaldırma yerel olarak kullanılmayacak kod derleme daha az zaman alır çünkü. Daha küçük nesne dosyalara daha hızlı bağlama, daha küçük bir yürütülebilir dosyayı oluşturmak için daha az sembolleriyle oluşturan anlamına gelir. Alan, bu nedenle, kaydetme sahip olmasının nedeni bir çift gelir değerlerini, **bağlantı SDK** tüm cihaz için varsayılan yer almaktadır. 

> [!NOTE]
> **Bağlantı SDK** seçeneği yalnızca, bağlantı Framework SDK'ları yalnızca veya bağlantı SDK derlemeleri olarak kullanılıyor IDE bağlı olarak görünebilir.
 

### <a name="tips"></a>İpuçları

- Derleme: 
  - FAT ikiliyi (örneğin ARMv7 + ARM64) hızlı tek mimarisi (örneğin ARM64) oluşturma
  - Hata ayıklama sırasında PNG dosyaları en iyi duruma getirme kaçının
  - Tüm derlemelerde bağlama göz önünde bulundurun. Her derlemeyi en iyi duruma getirme 
  - Hata ayıklama simgeleri oluşturulmasını kullanarak devre dışı bırakma `--dsym=false`. Ancak, farkında bu devre dışı bırakma kilitlenme raporları uygulama yerleşik bu makinede yalnızca symbolicated, anlamına gelir ve yalnızca uygulama atılmış değildi olması gerekir.

 
Kaçınılması gereken bazı noktalar açıklanmaktadır:

- FAT ikili dosyaları (hata ayıklama) 
- Bağlayıcı devre dışı bırak `–nolink` 
- Çıkarma devre dışı bırakma 
  - Semboller `--nosymbolstrip` 
  - IL (sürüm) `--nostrip`.  
 
Ek ipuçları 

- Simulator'da yeniden derleme tercih gibi 
  - Uygulama Nesne AĞACI misiniz derlemeler (nesne dosyaları) önbelleğe alınır 
- Hata ayıklama sembolleri, dsymutil çalıştıran nedeniyle ve aygıtlara yüklemek için daha büyük ve ek anda yukarı bittikten sonra daha uzun süren oluşturur. 
- Yayın derlemeleri varsayılan olarak, bir IL Şerit derlemelerin yapın. Yalnızca biraz zaman alır ve büyük olasılıkla geri daha küçük bir .app cihazına dağıtırken kazanılan.
- Her yapı (hata ayıklama) büyük statik dosyaları dağıtmaktan kaçının 
  - UIFileSharingEnabled (info.plist) kullanın 
    - Varlıklar kez yüklenir 
- Şüpheli zaman kullanmak `–time –time` değişikliğinizin ölçmek için bayrağı

Aşağıdaki ekran görüntüsünde, iOS seçeneklerinizi simulator ilgili bu seçenekleri ayarlamak verilmektedir:

[![](ios-build-mechanics-images/image4.png "Ayar seçenekleri")](ios-build-mechanics-images/image4.png#lightbox)

## <a name="using-the-linker"></a>Bağlayıcı kullanma

Uygulamanızı oluştururken mtouch uygulama kullanmıyorsa kod kaldıran yönetilen kod için bir bağlayıcı kullanır. Teorik olarak, bu daha küçük ve bu nedenle daha hızlı derlemeleri sağlar. Bağlayıcı hakkında daha fazla bilgi için bkz [İos'ta bağlama](~/ios/deploy-test/linker.md) Kılavuzu.

Bağlayıcı kullanırken aşağıdaki seçenekleri göz önünde bulundurun:

- Seçme **olmayan bağlantı** bir aygıtı yapı büyük bir zaman miktarıdır sürer ve aynı zamanda daha büyük bir uygulama oluşturur. 
  - Boyut sınırını olmaları durumunda Apple uygulamaları reddeder. Bağımlı `MinimumOSVersion`, bu 60 MB olarak küçük olabilir. 
  - Yerel yürütülebilir dahil edilir. 
  - Kullanarak yok JIT derleme (aksine, bir cihazda Uygulama Nesne AĞACI) kullanıldığından simulator derlemeler için bağlantı daha hızlı değil.
- Bağlantı SDK varsayılan seçenektir.
- Tüm olmayabilir kullanmak, güvenli özellikle de bu kodu kullanarak değil, kendi böyle bir NuGets bağlantı veya bileşenleri. Derlemeleri bağlamayan seçerseniz tüm hizmetlerin kodundan potansiyel olarak daha büyük uygulamaları oluşturma, uygulama ile birlikte. 
  - Ancak, isterseniz **bağlantı tüm** dış bileşenlere özellikle kullanılıyorsa, uygulama kilitlenme. Bu belirli türlerinde yansıma kullanarak bazı bileşenleri kaynaklanır.
  - Statik çözümleme ve yansıma birlikte çalışmaz. 

Araçları kullanarak uygulama şeyler tutmak için sağlanabilir [ `[Preserve]` özniteliği](~/ios/deploy-test/linker.md). 

Kaynak kodu erişiminiz yok veya bir aracı tarafından oluşturulan ve bunu değiştirmek istiyor musunuz, bunu hala tüm türleri ve korunması gereken üyeleri tanımlayan bir XML dosyası oluşturarak bağlanabilir. Daha sonra bayrağı ekleyebilirsiniz `--xml={file.name}.xml` proje seçeneklerinizi hangi işlenen kodu tam olarak öznitelikleri kullanmakta olduğunuz gibi sorgulamanıza.


### <a name="partially-linking-applications"></a>Kısmen uygulamaları bağlama 

Uygulamanızın derleme zamanı en iyi duruma getirmek için uygulamaların, kısmen bağlamak mümkündür:

- Kullanım `Link All` ve bazı derlemeleri atlama 
  - Bazı uygulama boyut iyileştirmesi kaybolur.
  - Kaynak koduna erişim gereklidir.
  - Örneğin `--linkall --linkskip=fieldserviceiOS` .
 
- Kullanmak `Link SDK` seçeneğini ve kullanmak `[LinkerSafe]` ihtiyacınız derlemeleri özniteliği 
  - Gerekli kaynak koduna erişim.
  - Derlemenin bağlamak, güvenli olduğundan sistem bildirir ve Xamarin SDK olduğu gibi sorgulamanıza işlenir.
 
### <a name="objective-c-bindings"></a>Objective-C bağlamaları 

- Kullanarak `[Assembly: LinkerSafe]` zaman ve boyutu, bağlamaları öznitelikte kaydedebilir.

- SmartLink 
  - Yerel tarafında bitti 
  - Kullanım `[LinkWith (SmartLink=true)]` özniteliği
  - Bu, yerel kod karşı bağlıyorsanız kitaplığından ortadan kaldırmak için yerel bağlayıcı yardımcı olur. 
  - Not simgelerin dinamik aramanın bu ile çalışmaz. 

## <a name="summary"></a>Özet

Bu kılavuz, bir iOS uygulaması ve Proje yapı yapılandırması ve seçenekleri bağımlı dikkate alınması gereken seçenekleri süreyi incelediniz. 

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
