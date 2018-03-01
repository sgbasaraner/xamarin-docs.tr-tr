---
title: "Xamarin.Mac zamanında derleme öncesinde"
description: "Şimdi saati (Uygulama Nesne AĞACI) derleme dengelerin ve ilgili önemli noktalar"
ms.topic: article
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: f9ace41380987472b6957c94719e6ae9b6f7995d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinmac-ahead-of-time-compilation"></a>Xamarin.Mac zamanında derleme öncesinde

## <a name="overview"></a>Genel Bakış

Şimdi saati (Uygulama Nesne AĞACI) derleme başlangıç performansı artırmak için bir güçlü iyileştirme tekniğidir. Ancak, aynı zamanda derleme zamanında, uygulama boyutu ve program yürütme çok büyük şekillerde etkiler. Bunlara uygular bileşim anlamak için derleme ve bir uygulamanın yürütmesini biraz dalın yapacağız.

C# ve F # gibi yönetilen dilleri yazılan kod IL adlı bir ara temsili derlenir. Kitaplık ve program derlemelerde depolanan bu IL nispeten compact ve işlemci mimarileri arasında taşınabilir. IL, ancak yalnızca yönergeleri ve IL işlemciyi belirli makine koda çevrilmesi gerekir belirli bir noktada bir ara olur.

Bu işlem yapılabilir iki nokta vardır:

- **Tam zamanında (JIT)** – başlangıç ve makine kodu belleğe IL derlenmiş uygulamanızı yürütülmesi sırasında.
- **Süre (Uygulama Nesne AĞACI) tamamlanan** – IL derlenmiş ve yerel kitaplıklara yazılan ve, uygulama paket içinde depolanan derleme sırasında.

Her bir seçeneğin avantajları ve bileşim sayısı vardır:

- **JIT**
  - **Başlangıç zamanı** – JIT derleme başlangıçta yapılması gerekir. Çoğu uygulama için bu terabayt 100ms, ancak büyük uygulamalar için bu süre önemli ölçüde daha fazla olabilir.
  - **Yürütme** – olarak JIT kod kullanılan belirli bir işlemci için iyileştirilemiyor, biraz daha iyi bir kod oluşturulabilir. Çoğu uygulamada daha hızlı birkaç yüzdesi noktaları en fazla budur.
- **AOT**
  - **Başlangıç zamanı** – önceden derlenmiş dylib dosyalarından yüklenirken JIT derlemeleri önemli ölçüde daha hızlıdır.
  - **Disk alanı** – bu dylib dosyalarından önemli miktarda disk alanı ancak alabilir. Hangi derlemelerin AOTed yere bağlı olarak, çift veya daha fazla uygulamanızın kod bölümünün boyutu.
  - **Derleme süresi** – Uygulama Nesne AĞACI derleme önemli ölçüde daha yavaş bu JIT ve bunu kullanarak derlemeleri yavaşlatır. Bu yavaşlama saniye kadar bir dakika veya daha fazlasını bağlı olarak derlenmiş derlemeleri sayısı ve boyutu değişebilir.
  - **Gizleme** – ters mühendislik makine kodu daha önemli ölçüde daha kolaydır, mutlaka gerekli değil IL olarak hassas kodunu belirsizleştirirseniz yardımcı olmak için çıkarılmış. Bu "karma" seçeneği tanımlayın aşağıda gerektirir.

## <a name="enabling-aot"></a>Uygulama Nesne AĞACI etkinleştirme

Uygulama Nesne AĞACI seçenekleri, gelecekteki bir güncelleştirmede Mac Yapı bölmesine eklenir. O zamana kadar uygulama nesne AĞACI etkinleştirme Mac derleme "ek mmp bağımsız değişkenler" alanı aracılığıyla bir komut satırı bağımsız değişken geçirme gerektirir. Seçenekler şunlardır:


      --aot[=VALUE]          Specify assemblies that should be AOT compiled
                               - none - No AOT (default)
                               - all - Every assembly in MonoBundle
                               - core - Xamarin.Mac, System, mscorlib
                               - sdk - Xamarin.Mac.dll and BCL assemblies
                               - |hybrid after option enables hybrid AOT which
                               allows IL stripping but is slower (only valid
                               for 'all')
                                - Individual files can be included for AOT via +
                               FileName.dll and excluded via -FileName.dll

                               Examples:
                                 --aot:all,-MyAssembly.dll
                                 --aot:core,+MyOtherAssembly.dll,-mscorlib.dll



## <a name="hybrid-aot"></a>Karma uygulama nesne AĞACI

MacOS uygulama çalışma zamanı yürütme sırasında makine kodu kullanarak varsayılanlarını Uygulama Nesne AĞACI derlemesi tarafından üretilen yerel kitaplıklarından yüklendi. Ancak, burada JIT derleme önemli ölçüde daha iyileştirilmiş sonuçlar üretebilir bazı alanlar trampolines gibi kodunun vardır. Bu yönetilen derlemeler IL kullanılabilir olmasını gerektirir. İos'ta, uygulamaları herhangi JIT derleme kullanımdan sınırlıdır; kod bu bölümünde de derlenmiş uygulama nesne AĞACI alır.

Karma seçenek hem derleme için (iOS gibi) Bu bölümde ayrıca IL çalışma zamanında kullanılabilir olmayacak varsayın için ancak derleyiciye. Ardından bu IL atılması yapı gönderin. Yukarıda belirtildiği gibi çalışma zamanı daha az en iyi duruma getirilmiş yordamları bazı yerlerde kullanmak için zorunlu kılınır.

## <a name="further-considerations"></a>Dikkat edilecek diğer konular

Uygulama Nesne AĞACI negatif sonuçlarını boyutları ve işlenen derlemeleri sayısı ile ölçeklendirin. Tam [hedef framework](~/mac/platform/target-framework.md) örnek, bir önemli ölçüde daha büyük temel sınıf kitaplığı (BCL) Modern daha içerir ve böylece uygulama nesne AĞACI önemli ölçüde uzun sürer ve büyük paketlerin üretmek için. Bu araya geldiğinde bağlama ile tam hedef framework'ün uyumsuzluk tarafından hangi şeritler kullanılmayan kodu. En iyi sonuçlar için Modern ve etkinleştirme bağlama uygulamanıza taşımayı düşünün.

Uygulama Nesne AĞACI ek bir faydası, yerel hata ayıklama ve profil oluşturma toolchains ile geliştirilmiş etkileşimleri ile birlikte gelir. Codebase büyük bir çoğunluğu zaman önceden derleneceği olduğundan, işlev adları ve profil oluşturma ve hata ayıklama yerel kilitlenme raporları içinde okunmasını simgeleri gerekir. Oluşturulan JIT işlevleri değil şu adlara sahip ve genellikle çözümlemek oldukça zor adlandırılmamış onaltılık uzaklıkları gösterilir.
