---
title: Platform uygulamaları genel bakış oluşturma
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b3b15736b5ec750e0b8db078cf428a7f573bc435
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="building-cross-platform-applications-overview"></a>Platform uygulamaları genel bakış oluşturma

Bu kılavuz Xamarin platform ve kod yeniden kullanımını en üst düzeye çıkarmak ve tüm ana mobil platformları yüksek kaliteli yerel bir deneyim sağlamak için platformlar arası uygulama mimari nasıl sunar: iOS, Android ve Windows Phone.

Odağı üretkenliği ve yardımcı programı (oyun olmayan uygulamalar) ancak bu belgede kullanılan üretkenlik uygulamaları ve oyun uygulamaları için genel olarak geçerli bir yaklaşımdır. Bkz: [MonoGame belge giriş](~/graphics-games/monogame/introduction/index.md) veya kullanıma [Unity için Visual Studio Araçları](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity) platformlar arası oyun geliştirme kılavuzu için.

Tümcecik "yazma-kez, her yerde Çalıştır" extol için kullanılan tek bir virtues çalışmaları birden çok platformda değiştirilmemiş codebase. Kodu yeniden kullanma avantajı vardır, ancak bu yaklaşımı genellikle bir genel payda düşük özellik kümesine sahip uygulamalar ve uymayan genel görünümlü bir kullanıcı arabirimi için sorunsuz şekilde Hedef platformlar hiçbirine yol açar.

Xamarin değil yalnızca bir "yazma-kez, her yerde Çalıştır" platform, güçlü biri yerel kullanıcı arabirimleri her platform için özel olarak uygulama özelliği olduğundan. Ancak, paylaşmak hala mümkündür Düşünceli tasarımıyla çoğu kullanıcı olmayan arabirimi kodu ve iki tarafın en iyi alın: veri depolama ve iş mantığı kodunuzu bir kez yazın ve her platformda yerel Uı'lar sunar. Bu belgede, bu hedefe ulaşmak için genel bir mimari yaklaşım açıklanır.

Xamarin platformlar arası uygulamalar oluşturma için önemli noktaları bir özeti aşağıda verilmiştir:

-   **C# kullanan** -uygulamalarınızı C# dilinde yazma. C# ile yazılmış varolan kod iOS ve Android Xamarin kolayca kullanarak bağlantı noktası kurulmuş ve açıkça Windows uygulamalarında kullanılır.
-   **MVC veya MVVVM tasarım desenleri kullanan** -modeli/görünüme/denetleyiciye desenini kullanarak uygulamanızın kullanıcı arabirimi geliştirin. Bir modeli/görünüme/denetleyiciye yaklaşım ya da bir görünüm/modeli/ViewModel yaklaşım kullanarak uygulamanızı mimari "Modeli" ve rest arasında NET bir ayrım olduğu. Uygulamanızın hangi kısımlarının yerel kullanıcı arabirimi öğelerinin her platform için (iOS, Android, Windows, Mac) kullanarak ve bu iki bileşenden uygulamanıza bölmek için kılavuz olarak kullanın belirleyin: "Temel" ve "Kullanıcı arabirimi".
-   **Yerel arabirimleri oluşturmak** -her bir işletim sistemine özel uygulama farklı kullanıcı arabirimi katman (içinde uygulanan C# ile Yardım yerel UI Tasarım araçları) sağlar:

1.  İos'ta, Uıkit API'ları isteğe bağlı olarak, kullanıcı Arabirimi görsel olarak oluşturmak için Xamarin'ın iOS Tasarımcısı kullanarak, yerel görünümlü uygulamaları oluşturmak için kullanın.
1.  Android, Xamarin'ın UI Tasarımcısı yararlanarak, yerel görünümlü uygulamaları oluşturmak için Android.Views kullanın.
1.  Visual Studio ya da harmanlamanın UI Tasarımcısı'nda oluşturulan sunu katmanı için Windows XAML kullanacaksınız.
1.  Mac üzerinde Xcode'da oluşturduğunuz sunu katmanı için film şeritleri kullanır.

Xamarin.Forms projeleri tüm platformlarda desteklenir ve Xamarin.Forms XAML kullanılarak platformları arasında paylaşılabilir kullanıcı arabirimleri oluşturma izin verin. 

Kodu yeniden kullanma miktarını büyük ölçüde ne kadar kod paylaşılan çekirdek ve kullanıcı arabirimi ne kadar kodudur belirli tutulur bağlı olacaktır. Çekirdek doğrudan kullanıcıyla etkileşime girmez ancak bunun yerine, toplar ve bu bilgileri görüntülemek uygulama bölümlerinin hizmetleri sağlar. herhangi bir şey kodudur.

Kodu yeniden kullanma miktarını artırmak için bu sistemler arasındaki gibi ortak hizmetleri sağlayan platformlar arası bileşenleri benimseyebilirsiniz:

1.   [SQLite net](https://www.nuget.org/packages/sqlite-net-pcl/) yerel SQL depolama
1.   [Xamarin eklentileri](https://xamarin.com/plugins) kamera, kişiler ve coğrafi konum, gibi aygıta özgü özellikleri erişmek için
1.   [NuGet paketlerini](https://nuget.org) , Xamarin projelerle uyumlu gibi [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  .NET framework özellikleri, ağ, web Hizmetleri, GÇ ve daha fazla bilgi için kullanma.


Bu bileşenlerden bazıları uygulanır *Tasky* örnek olay incelemesi.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>Bir çekirdek Kitaplığı'na yeniden kullanılabilir kod ayırın

Uygulama Mimarinizi katmanlama ve yeniden kullanılabilir çekirdek kitaplığa platform belirsiz çekirdek işlevleri taşıyarak sorumluluk ayrımı ilkesini izleyerek, aşağıdaki şekilde platformları arasında paylaşımı kodu en üst düzeye gösterilmektedir:

 ![](overview-images/layers2.png "Uygulama Mimarinizi katmanlama ve yeniden kullanılabilir çekirdek kitaplığa platform belirsiz çekirdek işlevleri taşıyarak sorumluluk ayrımı ilkesini izleyerek, platformlar arası paylaşımı kodu en üst düzeye")

 <a name="Case_Studies" />


## <a name="case-studies"></a>Örnek olay incelemeleri

Bu belge – eşlik eden bir vaka çalışması yok *Tasky Pro*. Her örnek olay incelemesi uygulama gerçek örnekte bu belgede açıklanan kavramlar açıklanır. Açık kaynak kodudur ve kullanılabilir [github](https://github.com/xamarin/mobile-samples/).
