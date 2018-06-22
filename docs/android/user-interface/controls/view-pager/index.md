---
title: ViewPager
description: ViewPager jestsel Gezinti uygulamanıza olanak sağlayan bir düzen yöneticisidir. Sol ve sağ adıma veri sayfaları aracılığıyla jestsel Gezinti manyetik kullanıcıya sağlar. Bu kılavuz, jestsel Gezinti ViewPager, sahip olan ve olmayan parçaları uygulamak açıklanmaktadır. Ayrıca, nasıl PagerTitleStrip ve PagerTabStrip kullanarak sayfa göstergeleri ekleneceğini açıklar.
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: bd687175048bb6a19dde21e66619667511a76796
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769009"
---
# <a name="viewpager"></a>ViewPager

_ViewPager jestsel Gezinti uygulamanıza olanak sağlayan bir düzen yöneticisidir. Sol ve sağ adıma veri sayfaları aracılığıyla jestsel Gezinti manyetik kullanıcıya sağlar. Bu kılavuz, jestsel Gezinti ViewPager, sahip olan ve olmayan parçaları uygulamak açıklanmaktadır. Ayrıca, nasıl PagerTitleStrip ve PagerTabStrip kullanarak sayfa göstergeleri ekleneceğini açıklar._

 
## <a name="overview"></a>Genel Bakış

Uygulama geliştirme yaygın bir senaryo kullanıcılara eşdüzey görünümler arasında jestsel Gezinti sağlamak için gerekiyor. Bu yaklaşımda, kullanıcı erişim sayfaları içeriğin (örneğin, Kurulum Sihirbazı'nı veya slayt gösterisi) sağa veya sola swipes. Kullanarak bu manyetik görünümler oluşturabilirsiniz `ViewPager` pencere, kullanılabilir [Android destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). `ViewPager` Olan birden çok alt görünümlerini oluşan bir düzen pencere öğesi burada her alt görünüm sayfa düzeni oluşturan: 

[![Yatay Sağdan örnek ekran görüntüleri, TreePager uygulamayla](images/01-intro-sml.png)](images/01-intro.png#lightbox)

Genellikle, `ViewPager` ile birlikte kullanılan [parçaları](https://developer.xamarin.com/guides/android/platform_features/fragments/); ancak, burada kullanmak istediğiniz bazı durumlar vardır `ViewPager` eklenen karmaşıklığını olmadan `Fragment`s.

`ViewPager` Görüntülenecek görünüm sağlamak için bir bağdaştırıcı desen kullanır. Burada kullanılan bağdaştırıcısı tarafından kullanılan kavramsal olarak benzer [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; uygulaması sağladığınız `PagerAdapter` sayfaları oluşturmak için `ViewPager` kullanıcıya görüntüler. Tarafından görüntülenen sayfaları `ViewPager` olabilir `View`s veya `Fragment`s. Zaman `View`s görüntülenir, bağdaştırıcı alt sınıfların Android's `PagerAdapter` temel sınıfı. Varsa `Fragment`s görüntülenir, bağdaştırıcı alt sınıfların Android's `FragmentPagerAdapter`. Android desteği kitaplığı da içerir `FragmentPagerAdapter` (öğesinin bir alt `PagerAdapter`) bağlanma ile ilgili ayrıntıları yardımcı olmak için `Fragment`veri s. 

Bu kılavuz, her iki yaklaşımın gösterir: 

-   İçinde [görünümlerle Viewpager](~/android/user-interface/controls/view-pager/viewpager-and-views.md), [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) nasıl kullanılacağını göstermek için uygulama geliştirilmiş `ViewPager` (görüntü Galerisi yaprak döken ve her ağaçlarının) ağaç Katalog görünümleri görüntülemek için. 
    `PagerTabStrip`  ve `PagerTitleStrip` sayfa gezinme Yardım başlıklarını görüntülemek için kullanılır.

-   İçinde [parçalarla Viewpager](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md), biraz daha karmaşık bir [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) nasıl kullanılacağını göstermek için uygulama geliştirilmiş `ViewPager` ile `Fragment`s olarak matematik sorunları sunan bir uygulama oluşturmak için Flash kartları ve kullanıcı girişine yanıt verir. 


## <a name="requirements"></a>Gereksinimler

Kullanılacak `ViewPager` uygulama projenizde yüklemelisiniz [Android destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) paket. NuGet paketlerini yükleme hakkında daha fazla bilgi için bkz: [izlenecek yol: de dahil olmak üzere bir NuGet projenizdeki](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

 
## <a name="architecture"></a>Mimari

Üç bileşeni ile jestsel Gezinti uygulamak için kullanılan `ViewPager`:

-   ViewPager
-   Bağdaştırıcı
-   Çağrı cihazı göstergesi

Bu bileşenlerin her birini aşağıda özetlenmiştir.



### <a name="viewpager"></a>ViewPager

`ViewPager` koleksiyonunu görüntüleyen bir düzen yöneticisidir `View`birer birer s. Kullanıcının manyetik hareketi algılayabilir ve uygun şekilde sonraki veya önceki görünümüne gitmek için kendi iş. Örneğin, aşağıdaki ekran görüntüsünde gösteren bir `ViewPager` yanıt olarak bir kullanıcı hareketi sonraki bir görüntüden geçiş yapma: 

[![Closeup, TreePager uygulama görünümleri arasında geçiş görüntüleme](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>Bağdaştırıcı

`ViewPager` kendi veri çeker bir *bağdaştırıcısı*. Bağdaştırıcının iş oluşturmaktır `View`tarafından görüntülenen s `ViewPager`, gerektiğinde sağlama. Bu kavram Aşağıdaki diyagramda gösterilmiştir &ndash; bağdaştırıcı oluşturur ve doldurur `View`s ve bunlara sağlar `ViewPager`. Olarak `ViewPager` kullanıcının manyetik hareketleri algılar uygun sağlamak için bağdaştırıcıyı ister `View` görüntülemek için: 

[![Bağdaştırıcı görüntüler ve adları ViewPager şeklini gösteren diyagram](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

Bu örnekte, her `View` bir ağaç görüntüsü ve bir ağaç adı için geçmeden önce oluşturulan `ViewPager`. 



### <a name="pager-indicator"></a>Çağrı cihazı göstergesi

`ViewPager` büyük veri kümesini görüntülemek için kullanılabilir (örneğin, bir Resim Galerisi görüntüleri yüzlerce içerebilir). Büyük veri kümelerinde gezinmeyi kullanıcının yardımcı olmak için `ViewPager` tarafından eşlik bir *çağrı cihazı göstergesi* bir dize görüntüler. Dize, resim başlık, bir resim yazısını veya yalnızca geçerli görünümün konumu veri kümesinde olabilir. 

Bu gezinti bilgileri oluşturmak üzere iki görünüm vardır: `PagerTabStrip` ve `PagerTitleStrip.` her bir dize en üstünde görüntüleyen bir `ViewPager`, ve her kendi veri çeker `ViewPager`'s, BT'nin her zaman ile eşit kalması bağdaştırıcısı şu anda görüntülenen `View`. Aralarındaki fark `PagerTabStrip` görsel bir gösterge "geçerli" dizesini içeren `PagerTitleStrip` (Bu ekran görüntülerinde gösterilmez gibi) yapar: 

[![PagerTitleStrip ve PagerTabStrip TreePager uygulamayla ekran görüntüleri](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

Bu kılavuz gösteren immplement nasıl `ViewPager`, bağdaştırıcı ve göstergesi uygulama bileşenleri ve jestsel Gezinti desteklemek üzere bunları tümleştirebilirsiniz. 



## <a name="related-links"></a>İlgili bağlantılar

- [TreePager (örnek)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (örnek)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
