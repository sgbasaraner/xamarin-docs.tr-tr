---
title: "Çok İşlemli Hata Ayıklama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 852F8AB1-F9E2-4126-9C8A-12500315C599
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: 9454c65298dbb5417f91765f541d22ae1d6137d7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="multi-process-debugging"></a>Çok İşlemli Hata Ayıklama

Bu hedef farklı platformlar birden çok proje Mac için Visual Studio'da geliştirilen modern çözüm çok yaygındır. Örneğin, bir çözümü bir web hizmeti projesi tarafından sağlanan verileri dayanan bir mobil uygulama projesi olabilir. Bu çözüm geliştirirken, geliştirici hataları gidermek için eşzamanlı olarak çalışan her iki proje olması gerekebilir. ' Den başlayarak [Xamarin döngüsü 9 sürümü](https://releases.xamarin.com/stable-release-cycle-9/), artık aynı anda çalışan birden çok işlem hatalarını ayıklamak Mac için Visual Studio için mümkündür. Kesme noktaları, değişkenleri inceleyin ve iş parçacıkları birden fazla çalışan projesinde görüntülemek için ayarlanacak mümkün kılar. Bu olarak bilinir _birden çok işlem hata ayıklama_. 

Bu kılavuz birden çok işlem, birden çok işlem hatalarını ayıklamak için çözümleri yapılandırma ve Mac için Visual Studio ile var olan işlemler iliştirmek nasıl hata ayıklamayı desteklemek Mac için Visual Studio için yaptığınız değişikliklerden bazıları ele alınacaktır

## <a name="requirements"></a>Gereksinimler

Birden çok işlem hata ayıklama Mac için Visual Studio gerektirir

## <a name="ide-changes"></a>IDE değişiklikleri

Hata ayıklama çok işlemle geliştiricilere yardımcı olmak için Mac için Visual Studio kullanıcı arabirimi bazı değişiklikler gerçekleştirdi. Visual Studio Mac için güncelleştirilmiş olan **Debug araç**ve yeni bir **çözümleri yapılandırma** bölümüne **çözüm seçenekleri** klasör. Ayrıca, **iş parçacığı paneli** işlemleri ve iş parçacıklarını her işlem için çalışan şimdi görüntü olur. Belirli konular ister için Visual Studio Mac için birden çok hata ayıklama klavye takımı, her işlem için bir tane de görüntüler **uygulama çıktısı**.

### <a name="solution-configurations"></a>Çözüm yapılandırmaları

Varsayılan olarak, Mac için Visual Studio tek tek projesinde görüntüler **çözüm yapılandırması** debug araç alanı. Hata ayıklama oturumu başlatıldığında, bu Mac için Visual Studio ile başlamalıdır ve hata ayıklayıcısını projesidir.

Başlatmak ve Mac için birden çok işlem Visual Studio'da hata ayıklama için oluşturmak ise gerekli bir _çözüm yapılandırması_. Bir çözümde proje olması gereken bir çözüm yapılandırmasını açıklar hata ayıklama oturumu bir tıklatmayla başlatıldığında dahil **Başlat** düğmesini veya &#8984; &#8617; (**Cmd girin**) basıldığında. Aşağıdaki ekran görüntüsünde, birden çok çözüm yapılandırmaları olan Mac için Visual Studio'da Çözüm örneğidir:

![](multi-process-debugging-images/mpd01-xs.png "Birden çok çözüm yapılandırmaları Çözümle")

### <a name="parts-of-the-debug-toolbar"></a>Hata ayıklama araç bölümleri

Hata ayıklama araç çubuğu açılır menüsü üzerinden seçilecek çözüm yapılandırılmasına olanak verecek değişti. Bu ekran Debug araç parçaları gösterilmektedir:

![](multi-process-debugging-images/mpd02-xs.png "Hata ayıklama araç bölümleri")

1. **Çözüm Yapılandırması** -debug araç çözüm yapılandırmasında tıklayarak ve yapılandırma açılır menüden seçerek çözüm yapılandırmasını ayarlamak mümkündür:

    ![](multi-process-debugging-images/mpd03-xs.png "Örnek açılır çözüm yapılandırmaları ile")

2. **Hedef yapı** -bu projeler için yapı hedef tanımlar. Bu Mac için Visual Studio'nun önceki sürümlerden değiştirilmez
3. **Cihaz hedefleri** -çözüm üzerinde çalıştırılacak aygıtları seçer. Birbirinden ayrı cihaz veya öykünücü her proje için tanımlamak mümkündür.:

    ![](multi-process-debugging-images/mpd04-xs.png "Cihazlar için bir proje gösteren açılan")

### <a name="multiple-debug-pads"></a>Birden çok hata ayıklama klavye takımı

Çok çözüm yapılandırması başlatıldığında, bazı Visual Studio Mac klavye takımı için birden çok kez, her işlem için bir tane görünür. Örneğin, aşağıdaki ekran görüntüsünde iki gösterir **uygulama çıktısı** klavye takımı iki proje çalışan bir çözüm için:

![](multi-process-debugging-images/mpd05-xs.png "Bir çözüm yapılandırması için çıkış paneli")

### <a name="multiple-processes-and-the-active-thread"></a>Birden çok işlem ve _etkin iş parçacığı_

Tek bir işlemde bir kesme noktası karşılaşıldığında, diğer işlemler çalışmaya devam ederken bu işlemi yürütme duraklatılır. Bir tek işlem senaryosunda, Mac için Visual Studio kolayca iş parçacığı yerel değişkenleri, klavye takımı tek bir dizi uygulama çıktıda gibi bilgileri görüntüleyebilirsiniz. Birden çok kesme noktaları ve büyük olasılıkla birden çok iş parçacığı ile birden çok işlem olduğunda, ancak bunu tüm iş parçacıklarının tüm bilgileri görüntülemenize çalışılırken bir hata ayıklama oturumu bilgileriyle başa çıkacak şekilde geliştirici için zorlamayı kanıtlayabilirsiniz (ve işlemler) aynı anda.

Mac aynı anda yalnızca bir iş parçacığından diğerine bilgi görüntülenir için Visual Studio bu sorunu çözmek için bu olarak bilinir _etkin iş parçacığı_. Kesme noktasında duraklatır ilk iş parçacığı kabul _etkin iş parçacığı_. Etkin iş parçacığı odak noktası Geliştirici dikkat, iş parçacığı ' dir. Komutları gibi hata ayıklama **Step Over** &#8679; &#8984; O (Shift-Cmd-O) etkin iş parçacığı için verilir.

**İş parçacığı paneli** tüm işlemleri ve çözüm yapılandırmasında İnceleme altındaki iş parçacıklarını bilgilerini görüntülemek ve etkin iş parçacığı nedir konusunda görsel ipuçları sağlar:

![](multi-process-debugging-images/mpd06-xs.png "Bir çözüm yapılandırması için iş parçacığı paneli")

İş parçacığı bunları barındırma işlemi tarafından gruplandırılır. Proje adı ve etkin iş parçacığı kimliği kalın metin olarak görüntülenir ve sağ ok etkin iş parçacığı yanındaki cilt payı görünür. Önceki ekran görüntüsünde **iş parçacığı #1** içinde **kimliği 48703 işlem** (**FirstProject**) etkin iş parçacığı değil.

Birden çok işlem hata ayıklama sırasında görmek için etkin iş parçacığı mümkün kullanarak bilgi işlem (veya iş parçacığı) için hata ayıklama **iş parçacığı paneli**. Etkin iş parçacığı geçiş yapmak için istenen iş parçacığında seçin **iş parçacığı paneli** ve ardından çift tıklayın.

#### <a name="stepping-through-code-when-multiple-projects-are-stopped"></a>Birden çok proje durduğunda aracılığıyla koda atlama

Mac için Visual Studio iki (veya daha fazla) projeleri kesme noktası varsa, her iki işlem duraklatılır. Yalnızca mümkün **Step Over** etkin iş parçacığı kodu. Kapsam değişikliği etkin iş parçacığı odağı geçiş yapmak hata ayıklayıcı olanaklı kılar kadar diğer işlem duraklatılır. Örneğin, Visual Studio aşağıdaki ekran görüntüsünde iki projelerinde hata ayıklama Mac için göz önünde bulundurun:

![](multi-process-debugging-images/mpd09-xs.png  "Visual Studio için Mac iki projelerinde hata ayıklama")

Bu ekran görüntüsünde, her çözümü kendi kesme noktası vardır. Başlatır hata ayıklama sırasında karşılaşılan için ilk kesme açıktır **satır 10** , `MainClass` içinde **SecondProject**. Her iki proje kesme noktaları bulunduğundan, her işlem durdurulur. Kesme noktası karşılaştı sonra her çalıştırılışı **Step Over** etkin iş parçacığı kodda üzerinden adım Mac için Visual Studio neden olur.

Diğer işlem hala duraklatıldığında Mac için Visual Studio aynı anda kod satırı aracılığıyla, adım için kod atlama etkin iş parçacığı için sınırlı olur.

Önceki ekran görüntüsüne bir örnek olarak kullanarak, `for` döngü tamamlandı, Visual Studio Mac izin için **FirstProject** kesme noktasında kadar çalıştırmak için **satır 11** içinde `MainClass` edildi karşılaştı. Her **Step Over** komutu, hata ayıklayıcı ilerleyin, satır **FirstProject**, Visual Studio Mac anahtar etkin iş parçacığı için kullanılan iç buluşsal algoritmaları dön kadar  **SecondProject**.

Projelerden biri olan yalnızca bir kesme noktası ayarlayın ve ardından yalnızca bu işlemi duraklatıldı. Başka bir projeye geliştirici tarafından duraklatıldı kadar çalışmaya devam eder veya bir kesme noktası eklendi.

### <a name="pausing-and-resuming-a-processes"></a>Duraklatma ve sürdürme bir işlemler

Duraklatmak veya bir işlem üzerinde işlem sağ tıklayıp seçerek sürdürmek mümkündür **duraklatma** veya **devam** ve bağlam menüsünden:

![](multi-process-debugging-images/mpd08-xs.png "Duraklatmak veya devam ettirmek iş parçacığı defterinde")

Hata ayıklama araç çubuğunun görünümünü ayıklanacak projeleri durumuna bağlı olarak değişir. Birden çok proje çalıştırırken, hata ayıklama araç çubuğunda hem de görüntülenir **duraklatma** ve **Sürdür** düğmesi çalıştıran en az bir proje yoktur ve bir proje duraklatıldı burada:

![](multi-process-debugging-images/mpd07-xs.png  "Araç çubuğu hata ayıklama")

Tıklatarak **duraklatma** düğmesini **Debug araç** , a tıklayarak ayıklanacak tüm işlemler duraklatılır **sürdürmek** düğmeleri tüm duraklatılmış işlemleri devam edecek .

### <a name="debugging-a-second-project"></a>İkinci bir proje hata ayıklama

İlk proje Mac için Visual Studio tarafından başladıktan sonra ikinci bir proje hata ayıklamak mümkündür İlk Proje başladıktan sonra **sağ tıklatın* projeyi **çözüm paneli** seçip **hata ayıklama öğesi Başlat**:

![](multi-process-debugging-images/mpd13-xs.png  "Öğe hata ayıklamayı Başlat")

## <a name="creating-a-solution-configuration"></a>Bir çözüm yapılandırması oluşturma

A _çözüm yapılandırması_ Visual Studio Mac için hata ayıklama oturumu ile intiated olduğunda çalıştırmak için hangi proje söyler **Başlat** düğmesi. Birden fazla çözüm yapılandırması her bir çözüm olabilir. Bu projenin hata ayıklama sırasında hangi projeleri çalışması belirtmek üzere mümkün kılar.

Xamaring Studio'da yeni bir çözüm yapılandırması oluşturmak için:

1. Açık **çözüm seçenekleri** iletişim kutusunda Mac ve select için Visual Studio **Çalıştır > yapılandırmaları**:

    ![](multi-process-debugging-images/mpd10-xs.png "Çözüm yapılandırmasında çözüm Seçenekleri iletişim kutusu")

2. Tıklayın **yeni** düğmesi, yeni çözüm yapılandırması adını girin ve tıklatın **oluşturma**. Yeni çözüm yapılandırması görünür **yapılandırmaları** penceresi:

    ![](multi-process-debugging-images/mpd11-xs.png  "Yeni bir çözüm yapılandırma adlandırma")

3. Yeni çalışma yapılandırması yapılandırmaları listeden seçin. **Çözüm seçenekleri** iletişim çözümdeki her projeye görüntülenir. Hata ayıklama oturumu başlattığında başlatılması gereken her projesi denetleyin:

    ![](multi-process-debugging-images/mpd12-xs.png "Başlamak için projesini seçme")

**MultipleProjects** çözüm yapılandırması artık görünür **Debug araç**, bir geliştirici aynı anda iki proje hata ayıklamak olası hale getirerek.

## <a name="summary"></a>Özet

Bu kılavuzda ele alınan Mac için birden çok işlem Visual Studio'da hata ayıklama Çok işlemli hata ayıklamayı desteklemek için IDE'yi değişiklikleri bazıları ele ve ilişkili davranışa bazıları açıklanmaktadır.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin döngüsü 9 sürüm notları](https://releases.xamarin.com/stable-release-cycle-9/)
