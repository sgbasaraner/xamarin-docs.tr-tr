---
title: "Geniş rengi"
description: "Bu makalede, geniş renk ve bir Xamarin.iOS veya Xamarin.Mac uygulamasının nasıl kullanılabileceğini kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 95098cd5c97ccc8357531feb79e55600f53a4be5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="wide-color"></a>Geniş rengi

_Bu makalede, geniş renk ve bir Xamarin.iOS veya Xamarin.Mac uygulamasının nasıl kullanılabileceğini kapsar._

iOS 10 ve macOS Sierra genişletilmiş aralığı piksel biçimleri ve çekirdek grafikleri, çekirdek görüntü, Metal ve AVFoundation gibi çerçeveleri dahil olmak üzere Sistem genelindeki wide gam renk alanları desteği geliştirir. Geniş renk görüntüler ile cihazları için destek Bu davranış tüm grafik yığını boyunca sağlayarak daha fazla hızının.

## <a name="about-wide-color"></a>Geniş renk hakkında

Yukarıda belirtildiği gibi iOS 10 ve macOS Sierra genişletilmiş aralığı piksel biçimleri ve çekirdek grafikleri, çekirdek görüntü, Metal ve AVFoundation gibi çerçeveleri dahil olmak üzere Sistem genelindeki wide gam renk alanları desteği geliştirir. Geniş renk görüntüler ile cihazları için destek Bu davranış tüm grafik yığını boyunca sağlayarak daha fazla hızının.

90 Apple Mac üzerindeki işleme renk işlemek için ColorSync oluşturuldu. Bunlar, ayrıca oluşturun ve bilgisayar donanımında renk işleme standartlarına kümesini yükseltmek için Uluslararası renk Konsorsiyumu (ICC) bulunan yardımcı oldu. ICC ile Apple'nın çalışma ColorSync içinde bulunur ve OS X (şimdi macOS denir) çekirdek oluşturuldu.

Apple popülerliği Retina görüntülemek, yeni P3 görüntülemek ve görüntüleme P3 rengi (iMac 2015'te yayımlanan) alanı gibi donanım ile forefront adresindeki süredir ayrıca ve iPad uzmanları, iPhone 7 ve 7 iPhone TrueTone görüntüler artı.

MacOS Sierra geniş renk ve iOS 10 ile Apple macOS ve iOS yeni görünen teknolojiler tam olarak yararlanmak için renk işlemek değiştirmektedir.

## <a name="core-color-concepts"></a>Renk kavramları

Aşağıdaki kavramları rengi renk macOS ve iOS daha derin göz çıkarmadan önce ele alınması gerekir:

### <a name="color-space"></a>Renk alanını

Bir renk alanını içinde renkleri temsil karşılaştırıldığında ve yönetilebilen bir ortamıdır. Renk bileşenlerinden yoğunluğu tarafından tanımlanan bir dörde boyutlu alanı olabilir. 

[![](wide-color-images/color00.png "Bir renk alanını")](wide-color-images/color00.png#lightbox)

### <a name="color-channels"></a>Renk kanalları

Renk bileşenleri, ayrıca renk kanalları başvuruda bulunulabilir. Bazı bilinen Beyanları RGB alanları, gri alanları, CMYK alanları veya aygıt bağımsız alanları olacaktır. 

[![](wide-color-images/color02.png "Renk bileşenleri de için renk kanalları belirtilebilir")](wide-color-images/color02.png#lightbox)

### <a name="color-primaries"></a>Renk ana

Renk ana karşılaştırın ve renkleri hesaplamak için kullanılan koordinat sistemi sağlar. Renk ana genellikle bir renk kanal içinde oluşturulan belirli rengi en yoğun sürümü ayrılır.

[![](wide-color-images/color01.png "Renk ana karşılaştırın ve renkleri hesaplamak için kullanılan koordinat sistemi sağlama")](wide-color-images/color01.png#lightbox)

RGB renk yukarıda gösterilen alanı söz konusu olduğunda, renk ana yeri olan `1.0` koordinatları bağlantılı (gibi `[1.0, 0.0, 0.0]` kırmızı için).

### <a name="color-gamut"></a>Renk skalasını

Tüm tek tek renk kanalları verin renk alanını içinde bir birleşimi olarak tanımlı renklerin renk skalasını başvuruyor.

[![](wide-color-images/color03.png "Renk skalasını örneği")](wide-color-images/color03.png#lightbox)

## <a name="what-is-wide-color"></a>Geniş renk nedir

Geniş renk konu kapsayan önce bir tartışma rengini, standart RGB renk alanını (sRGB) standart geçerli endüstri hakkında sahip. Bugün bilgi işlem en yaygın olarak kullanılan renk alanını bağlıdır ve varsayılan renk alanını iOS ve macOS için.

Renk alanını sRGB aşağıdaki özelliklere sahiptir:

- ITU-R BT.709 standardına dayanır.
- 2.2, yaklaşık bir gama sahiptir.
- Bu, tipik aydınlatma koşullarına (D65) temsil eder.

SRGB kadar yaygın sektörün kullanıldığında, bir geliştirici bazı varsayımlarda, yapabilirsiniz olduğundan belirtilen renk faithfully görüntülendiği herhangi bir cihazda temsil edilir. Ancak, bu durumda her zaman olmayabilir. Ayrıca, renk alanını sRGB uymayan ve içinde therefor, gösterilemez birkaç renkleri vardır.

Örneğin, birçok textiles birçok mürekkep ve sRGB dışında dönmeden Boya kullanarak iş parçacığı ile tasarlanmıştır. Ayrıca bir kişi kendi günlük hayatta karşılaştığında birçok ürün renk alanını sRGB dışında kalan açık, Canlı renklerle oluşturulur. İçinde sRGB gösterilemez renkleri en ilgi çekici örnekleri bazıları sunsets, Sonbahar bırakır, exotic çiçekler ve tropikal sularında gibi yapısı noktalardır.

Ham biçimde dijital görüntüleri yakalama kullanıcılar, cihazlarında sRGB Renk alanını düzgün gösterilemez ve therefor düzgün ekranda görüntülenemiyor olsa bile, tüm bu renk verileri içeren görüntüleri olabilir.

### <a name="the-display-p3-color-space"></a>Görüntü P3 renk alanını

2015'te Apple renk alanını sRGB tarafından oluşturulan sorunlarını gidermek için yeni görünen P3 renk alanını sağlayan yeni ürünler (iMac ve iPad Pro 9.7") yayımladı.

[![](wide-color-images/color04.png "Yeni görünen P3 renk alanını")](wide-color-images/color04.png#lightbox)

Görüntü P3 renk alanını aşağıdaki özelliklere sahiptir:

- Geniş renk alanını için modern donanım platformları destekler.
- SMPTE DCI-P3 standardına bağlı. DCI P3 dijital projektörler için tasarlanmıştır ancak izleyiciler desteklemek için Apple tarafından değiştirildi.
- 2.2, yaklaşık bir gama sahiptir.
- Bu, tipik aydınlatma koşullarına (D65) temsil eder.

Apple göre kullanıcıların kendi mobil platformlar için kendi iş akışları taşıma. Daha fazlasını geniş renk görüntü dahil olmak üzere gerekli profesyonel mobil cihazlarda (iPad uzmanları) sRGB tarafından sunulan renk sunu sorunlarını çözme. Her bir cihaz Fabrika cihaz için cihazın, renk görüntü doğru ve tutarlı olduğundan emin olmanın en ayarlandı şekilde Fabrika ayarı'nı yükseltmek için çözümlerden birini oluştu.

Başka bir çözümdür, Apple iOS 10 ve macOS Sierra yerleşik tam, sistem genelinde renk yönetimi dahil edilmesi belirtir. 

### <a name="the-extended-range-srgb-color-space"></a>Genişletilmiş aralık sRGB Renk alanını

Tüm oluşturulan ve sRGB için ince ayar var olan iOS uygulamaları için hesap yeni iOS 10 sistem genelinde renk yönetimi içerir. Bu var olan uygulamaların ya da rengi gösterimi veya uygulama performansını etkiler kaydetmedi emin olmak için tasarlanmıştır.

Bu durumu yönetmek için Apple iOS 10 içinde genişletilmiş aralık sRGB Renk alanını (ve macOS Sierra de) eklemiştir.

Genişletilmiş aralık sRGB Renk alanını aşağıdaki özelliklere sahiptir:

- Tüm ana aynı sRGB sahiptir.
- 2.2, yaklaşık bir gama sahiptir.
- Bu, tipik aydınlatma koşullarına (D65) temsil eder.
- Negatif değerler destekler ve birden (1) büyük değerler.

Tarafından değerleri sıfırdan için izin verme ve daha büyük bir performans isabet veya bozulma, ancak olmadan sRGB mevcut renkler için var olan uygulamalar için renk alanını değil yalnızca izin verir genişletilmiş aralık sRGB görünür içinde herhangi bir renk temsil etmek renk alanını sağlar spektrumun. Tüm bunlar gerçekleştirilir hala aynı bağlantı noktalarını renk alanını sRGB olarak tutarken.

### <a name="extended-range-srgb-in-action"></a>Genişletilmiş aralık sRGB eylem

Sıfır ve bir dışında değerler genişletilmiş aralık sRGB Renk alanını nasıl çalıştığını görmek için aşağıdaki örneği uygulamanız görüntü P3 renk uzayındaki kullanılabilir en doymuş kırmızı:

[![](wide-color-images/color05.png "Sıfır ve bir dışında değerler genişletilmiş aralık sRGB Renk alanını nasıl çalışır?")](wide-color-images/color05.png#lightbox)

Görüntü P3 içinde bu renk olarak temsil edilir `[1.0, 0.0, 0.0]` ve onu olacaktır genişletilmiş aralık sRGB `[1.358, -0.074, -0.012]`. SRGB değerleri tam olduğundan görüntü P3 içinde kapsanan ve görüntü P3 değerleri "dış" sRGB aralıklarını düzenleyin.

Extreme negatif değerleri aşırı pozitif gitmek piksel değerlerini sağlayan fiziksel donanım için görünür spektrumun kullanılabilir herhangi bir renk görüntüleyebilir ve bu değerleri genişletilmiş aralık sRGB Renk alanını temsil edilebilir.

### <a name="device-pixel-formats"></a>Aygıt piksel biçimleri 

Renk alanını büyük ölçüde açıldı sRGB standartlaştırmak renk kanal başına 8 bit olduğundan çoğunlukla bir 8 bit piksel biçimi kullanarak sRGB renklerde açıklamak yeterli. Bu mükemmel ancak iyi yeterli değildir ve görüntüleri göstermek için bellek ve işlemci kullanımı arasında iyi kolaylığını verir.

Görüntü P3 renk koordinatları sRGB Renk alanını dışında gösterdiğinden genişletilmiş aralık sRGB Renk alanını renklerle doğru şekilde temsil etmek için renk kanal başına 16 bit gerektirir.

## <a name="system-wide-wide-color-support"></a>Sistem genelinde geniş renk desteği

Geniş renk ve iOS 10 içinde geniş gam ve macOS Sierra tam olarak desteklemek için Apple genişletilmiş aralık sRGB Renk alanını tam anlamıyla ve görüntü P3 almak için aşağıdaki çerçevelerini genişletilmiş:

- Uıkit (için yalnızca iOS)
- SceneKit
- Çekirdek grafikleri
- ImageIO
- Çekirdek görüntüsü
- Web Kiti
- SpriteKit
- Çekirdek animasyon
- AppKit (için yalnızca macOS)

Ayrıca, Retina genişletilmiş aralık sRGB Renk alanını desteği geliştirilmiştir görüntüleme ve görüntü P3 görüntüler.

Geniş renk desteklenir ve aşağıdaki uygulama içerik türlerinde kullanılabilir:

- Statik görüntü kaynakları uygulama paketine eklenen.
- Belge ve ağ görüntüsü kaynaklara bağlı.
- Live Fotoğraf veya ham biçimde görüntüleri gibi gelişmiş medya.
- 3B grafik gölgelendirici doku görüntüler.

## <a name="solving-the-color-problem"></a>Renk sorun giderme

Bir uygulamada görüntülenen içeriği çok çeşitli renk zengin kaynaklardan gelebilir. Ayrıca, bu içeriği çok çeşitli aygıtlar, her biri kendi renk görüntüleme yetenekleri aralığını gösterilebilir.

Bir iOS 10 uygulaması arasında köprü yeni yerleşik, sistem genelinde renk yönetmek sistemi kullanarak bu iki sorunları arasındaki fark. Bu sistem, görüntüyü aynı görüntü kodlanan hangi renk alanını olsun, herhangi bir iOS cihazda göründüğünü fark sağlar.

Renk Yönetimi bir ilişkili renk alanını (veya renk profili) sahip her görüntü ile başlar. Bu bilgiler kullanılır _renk işlem eşleştirme_ burada kaynak görüntü renkleri eşleştirilir çıktı aygıtı renkler için. Görüntüdeki her piksel renk eşleşen olması gerektiğinden, bu zaman alabilir ve yükü cihazın CPU üzerine getirin.

Doğası nedeniyle _renk işlem eşleştirme_, bu dönüştürme "büyük olasılıkla çıkış cihazı kaynak görüntü daha küçük bir skalasını varsa kayıplı" olabilir.

Neyse ki, yerleştirilmesini hesaplamalar _renk işlem eşleştirme_ (ya da CPU veya GPU) donanım kolayca hızlandırılabilir ve Apple sağlar Quartz 2B gibi temel sistemleri içine destek oluşturarak otomatik olarak çalışıyor ColorSync ve çekirdek animasyon. Doğru etiketli içerik için hiçbir kodlama, bu özelliklerden yararlanmak için gereklidir.

Renk Yönetimi her platformda gibi desteklenen:

- **macOS** -macOS en başından itibaren yönetilen renk açıldı.
- **iOS** -iOS iOS 9.3 (ve üstü) bu yana otomatik renk yönetimi desteklenen.

### <a name="designing-for-wide-gamut"></a>Geniş gam için tasarlama

Apple tasarlama aşağıdaki önerisi sahiptir ve renk geniş kullanarak, iOS ve macOS uygulamalarında geniş gam görüntü içeriği:

- Yalnızca olun burada geniş gam içeriği kullanmak uygulamada, bunlar otomatik olarak kullanılmamalıdır her yerde anlamlıdır.
- Yalnızca geniş gam içerik canlı renkler kullanıcı deneyimini iyileştirmek burada kullanın.
- Tüm içerik için var olan uygulamalar için P3 değiştirmek için gerekli değil.

Apple'nın araç zinciri geniş renk bir uygulamada destekleme ya hep ya hiç bir durum değildir bir aşamalı katılımı, geniş gam görüntü içerik için destek sağlar.

### <a name="upgrading-existing-content-to-wide-color"></a>Var olan içerik için geniş renk yükseltme

Apple mevcut görüntü içeriğini geniş renge yükseltmek için aşağıdaki önerileri vardır:

- Yalnızca "P3 profili görüntü uygulama düzenleme içeriği atama yok". Bunun yapılması, yalnızca yeni renk alanını içerik böylece görüntü değiştirmeden yeni alanı sığması için renkleri uzatma gibi beklenmeyen sonuçlarla mevcut renk etiketlerse.
- Görüntü içeriğini "uygulama düzenleme görüntü kullanarak görüntü P3 profili dönüştürülecek" gerekir.

### <a name="file-formats-and-color-profiles"></a>Dosya biçimlerini ve renk profilleri

Apple dosya biçimlerini ve uygulamanın geniş renk görüntü içeriğini kullanılan rengi profilleri için aşağıdaki önerileri vardır:

- "Görünen P3" renk profili RGB çalışma alanları için kullanın.
- 16 bit başına renk kanal modunu kullanın.
- Late 2015 iMac kullanın (veya üstü) doğru şekilde görüntü içeriğini önizlemek için.
- Görüntü varlıklarının gömülü "Görünen P3" ICC profili 16 bit PNG dosyaları olarak dışarı aktarın.

> [!IMPORTANT]
> **Not:** kullanma **Web için Kaydet** veya **verme varlıklar** özellikleri bulunan yazılım düzenleme en popüler görüntüde _almayacak_ geniş renk görüntüleri için bu yana iş Bu özellikler gerekli dosya biçimi belirtimlerine henüz desteklemek için güncelleştirilmemiş.

### <a name="supporting-wide-color-with-asset-catalogs"></a>Varlık kataloglarıyla geniş renk destekleme

Apple iOS 10 ve macOS Sierra, geniş renk desteklemek için varlık içerir ve uygulamanın paket statik görüntü içeriğini kategorilere ayırmak için kullanılan katalog genişletilmiştir.

Kullanarak varlık kataloglar uygulamaya aşağıdaki avantajları sunar:

- Statik görüntü varlıklar için en iyi dağıtım seçeneği sağlarlar.
- Bunlar otomatik renk düzeltme destekler.
- Bunlar otomatik piksel biçimi iyileştirme destekler.
- Bunlar uygulama dilimleme ve yalnızca ilgili get'in içeriği son kullanıcının cihaza teslim sağlayan uygulama yoğunluk destekler.

Apple aşağıdaki geliştirmeleri geniş renk desteği varlık Kataloğu yaptı:

- Bunlar, 16 bit (her renk kanalı) kaynak içerik destekler.
- Görüntü gam tarafından kataloglama içerik destekledikleri. İçerik sRGB veya görüntü P3 gamutları birbirinden için etiketlenebilir.

Geliştirici, kendi uygulamalarında geniş renk içerik desteklemek için üç seçenek vardır:

1. **Hiçbir şey yapma** -olarak geniş renk içeriği yalnızca uygulamanın burada gerekir (burada bunlar geliştirmek kullanıcı deneyimini) canlı renkler görüntülenecek durumlarda kullanılmalıdır olduğundan, bu gereksinim dışında içerik bırakılmalıdır-değil. Tüm donanım durumlarda düzgün işlenecek devam eder.
2. **Var olan içerik görüntüleme P3 yükseltme** -bu P3 biçiminde yeni, yükseltilmiş bir dosya ile bunların varlık kataloğunda mevcut görüntü içeriğinin yerine geçecek ve bu nedenle varlık Düzenleyicisi'nde etiketlemek Geliştirici gerektirir. Derleme zamanında sRGB türetilmiş görüntü bu varlıklarından oluşturulur.
3. **En iyi duruma getirilmiş varlık içerik sağlamak** -bu durumda, geliştirici bir 8 bit sRGB ve 16 bit görüntü P3 Vizyon (varlık Düzenleyicisi'nde düzgün etiketli) varlık kataloğunda her görüntünün sağlar.

### <a name="asset-catalog-deployment"></a>Varlık Kataloğu dağıtım

Geliştirici varlık kataloglarıyla geniş renk görüntü içerik uygulama mağazası uygulama gönderdiğinde aşağıdakiler gerçekleşir:

- Uygulama son kullanıcıya dağıtıldığında, uygulama dilimleme yalnızca uygun içerik değişken kullanıcının cihazına teslim edilmesini.
- Geniş renk desteklemeyen aygıtta yoktur geniş renk içerik dahil etmek için yükü ücret ödemeden cihaza hiçbir zaman sevk edilmiş olarak.
- `NSImage` macOS Sierra üzerinde (ve daha sonra) donanım görüntüleme için en iyi içerik gösterimi otomatik olarak seçer.
- Görüntülenen içeriği veya aygıtlar donanım görüntü özelliklerini değiştirme olsa otomatik olarak yenilenir.

### <a name="asset-catalog-storage"></a>Varlık Kataloğu depolama

Varlık Kataloğu depolama aşağıdaki özellikleri ve bir uygulama için etkileri vardır:

- Derleme zamanında Apple yüksek kaliteli görüntü dönüşümleri yoluyla görüntü içerik depolamayı optimize etmek çalışır.
- 16 bit renk kanal başına geniş renk içerik için kullanılır.
- İçerik bağımlı görüntü sıkıştırma teslim edilebilir içerik boyutları düşürmek için kullanılır. Daha fazla içerik boyutları en iyi duruma getirme için yeni "kayıplı" sıkıştırmaları eklenmiştir.

## <a name="colors-in-user-interfaces"></a>Kullanıcı arabirimleri renkleri

Renkleri bir kullanıcı arabirimi ile çalışırken, ekrandaki piksel bir düz renk çoğu. Ayrıca, çoğu bu piksel görüntüleri varlıklarından gelen yok ancak uygulama tarafından doğrudan (veya işletim sistemi uygulama adına göre) çizilir.

Geniş gam renk birkaç kullanıcı Arabirimi düzeyinde çalışırken zorluklar çıkarabilir:

- **Renk iletişim** - tasarımcılar ve olur ve genellikle geliştiriciler arasında rengi hakkında konuşurken bir _kabul_ sRGB Renk alanını söz konusu. Bir renk olarak bildirilmesi şekilde `rgb(128, 45, 56)` veya `#FF0456`. Bir geniş gam tasarım işbu Beyanları belirtilen renk doğru şekilde temsil etmek için yeterli bilgi sağlamıyorsa, renk alanını çalışma de dahil edilmelidir. Apple öneren kullanarak `P3(128, 45, 56)` ve `P3#FF0456` yerine. 
- **Renkleri çekme** - çoğu popüler görüntüyü düzenleme ve yazılım yaşar renk alanını sRGB onunla aynı sınırlamalara gelen kendi yerleşik renk seçiciler kullanırken tasarımının. Tasarımcı bunlar Renk Seçici "Görünen P3" renk uzayındaki geniş renk tasarımlar ile çalışırken emin olun.
- **Renkleri kodlama** - iki `NSColor` (macOS) ve `UIColor` (iOS & tvOS) sahip P3 renkleri doğrudan oluşturmak için yeni kullanışlı yöntemler ve her ikisi de renkleri genişletilmiş aralık sRGB içinde renk alanını de desteklemek için genişletilmiştir.
- **Renkleri depolama** -depolanmasını geniş gam bir uygulamanın belgedeki renkler zaman dikkatli'nin alınması. 8 bit yerlerde ince sRGB Renk alanını çalışılan renk kanal başına 16 bit renk kanal başına geniş renkler için kullanılmalıdır. Geliştirici ayrıca izlemesi gerekir (her şeyi geleneksel olarak yalnızca sRGB başlatıldığından beri) varsayılan renk alanını örnekleri.

## <a name="colors-on-the-web"></a>Web renkleri

Web sayfaları ve geniş renk görüntü destekleyen cihazlarda geniş renkle çalışırken dikkatli olunmalıdır. Tüm Web sitesi dahil görüntü içeriğe uygun şekilde etiketli, iOS ve macOS otomatik olarak eşleşme renk ve bunları doğru görüntüler.

Medya sorguları varlıkları P3 ve sRGB özellikli cihazların arasında gidermek de kullanılabilir:

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple kabul sRGB alanı yanı sıra diğer renk alanları belirtilmelidir CSS sağlayacak bir WebKit teklifi de vardır.

## <a name="rendering-off-screen-images-in-app"></a>Uygulamasında ekran dışı görüntüleri işleme

Oluşturulan uygulama türüne bağlı olarak, internet'ten topladığınız veya görüntü içeriği doğrudan uygulama (örneğin, uygulama örneğin çizim vektör) içinde oluşturma görüntüsü içeriğine dahil etmek kullanıcı izin verebilir.

Her iki durumda, uygulamanın gerekli görüntülerin hem iOS hem de macOS eklenen Gelişmiş özellikleri kullanarak geniş renkte ekran dışı duruma getirebilir.

### <a name="drawing-wide-color-in-ios"></a>İOS geniş renk çizme

Bir geniş renk resim iOS 10 doğru çizmek nasıl ele almadan önce aşağıdaki ortak iOS çizim kodu göz atın:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...
    
    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
}
```

Sorunları ele alınması gereken standart kod _önce_ geniş renk resim çizmek için kullanılabilir. `UIGraphics.BeginImageContext (size)` İOS resim çizim başlatmak için kullanılan yöntem aşağıdaki sınırlamalara sahiptir:

- Renk kanal başına 8 bitten daha fazla ile görüntü bağlamları oluşturulamaz.
- Genişletilmiş aralık sRGB Renk alanını renkleri temsil edilemez.
- Arka planda çağrılan alt düzey C yordamları nedeniyle bir sRGB olmayan renk alanında bağlam oluşturmak için bir arabirim sağlama yeteneği yok.

Bu sınırlamalara işlemek ve iOS 10 geniş renk resim çizmek için aşağıdaki kodu kullanın:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

Yeni `UIGraphicsImageRenderer` sınıfı genişletilmiş aralık sRGB Renk alanını işleyebilen yeni bir görüntü bağlam oluşturur ve aşağıdaki özelliklere sahiptir:

- Tam olarak, varsayılan olarak yönetilen rengi olur.
- Varsayılan olarak genişletilmiş aralık sRGB Renk alanını destekler.
- Akıllıca onu sRGB veya genişletilmiş aralık sRGB Renk uygulamayı çalıştıran iOS cihaz özelliklerini temel alan oluşturulup oluşturulmayacağını belirler.
- Görüntü bağlam tam olarak ve otomatik olarak yönettiği (`CGContext`) Geliştirici arama hakkında endişelenmeniz gerekmez için yaşam süresi başlar ve son bağlam komutları.
- İle uyumlu `UIGraphics.GetCurrentContext()` yöntemi.

`CreateImage` Yöntemi `UIGraphicsImageRenderer` sınıfı bir geniş renk görüntüsü oluşturmak için çağrılır ve tamamlanma işleyicisi içine çizmek için yansıma bağlamına sahip geçirildi. Tüm çizim yapılır bu tamamlama işleyici içinde aşağıdaki gibi:

- `UIColor.FromDisplayP3` Yöntemi geniş gam görüntü P3 renk alanını yeni bir doymuş kırmızı renk oluşturur ve ilk yarısı dikdörtgen çizmek için kullanılır. 
- Dikdörtgen yarısı çizilmiş normal sRGB içinde tam olarak ikinci karşılaştırma için kırmızı renk doygun.

### <a name="drawing-wide-color-in-macos"></a>MacOS geniş renkte çizim

`NSImage` Sınıfı macOS Sierra geniş renk görüntüleri çizim destekleyecek şekilde genişletilmiştir. Örneğin:

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);
    
    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();
    
    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();
    
    // Return modified
    return true;
});
```

## <a name="rendering-on-screen-images-in-app"></a>Görüntüleri uygulamasında ekran oluşturma

Geniş renk görüntüleri ekran işlemek için işlem macOS ve yukarıda gösterilen iOS ekran dışı geniş renk resmi çizim benzer olarak çalışır.

### <a name="rendering-on-screen-in-ios"></a>İOS ekran oluşturma

Bir görüntü geniş renkli ekran iOS olarak işlemek uygulamanın ihtiyacı olduğunda geçersiz kılma `Draw` yöntemi `UIView` normal olarak söz konusu. Örneğin:

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
            ...
        }
    }
}
``` 

İOS 10 ile yaptığı gibi `UIGraphicsImageRenderer` yukarıda gösterilen akıllıca karar onu sRGB veya genişletilmiş aralık sRGB Renk uygulama etkin olduğunda çalışırken iOS cihaz özelliklerini temel alan oluşturulup oluşturulmayacağını sınıfı `Draw` yöntemi çağrılır. Ayrıca, `UIImageView` yönetilen de iOS 9.3 itibaren renk açıldı.

Uygulama üzerinde işleme nasıl yapıldığını bilmeniz gerek olup bir `UIView` veya `UIViewController`, yeni denetleyebilirsiniz `DisplayGamut` özelliği `UITraitCollection` sınıfı. Bu değer olacaktır bir `UIDisplayGamut` enum şunlardan:

- P3
- Srgb
- Belirtilmemiş

Uygulama, bir resim çizmek için kullanılan renk alanı denetimi isterse, yeni bir kullanabilirsiniz `ContentsFormat` özelliği `CALayer` istenen renk alanını belirlemek için. Bu değer bir `CAContentsFormat` enum şunlardan:

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>MacOS içinde ekran oluşturma

Bir görüntü geniş renkli macOS ekran olarak işlemek uygulamanın ihtiyacı olduğunda geçersiz kılma `DrawRect` yöntemi `NSView` normal olarak söz konusu. Örneğin:

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

Yeniden, bu akıllıca onu sRGB veya genişletilmiş aralık sRGB Renk uygulama etkin olduğunda çalışırken Mac donanım özelliklerine göre alanı oluşturulup oluşturulmayacağını karar `DrawRect` yöntemi çağrılır.

Uygulama, bir resim çizmek için kullanılan renk alanı denetimi isterse, yeni bir kullanabilirsiniz `DepthLimit` özelliği `NSWindow` istenen renk alanını belirlemek için sınıf. Bu değer bir `NSWindowDepth` enum şunlardan:

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb

## <a name="summary"></a>Özet

Bu makalede, geniş renk ve onu uygulanabilir ve bir Xamarin.iOS veya Xamarin.Mac uygulaması içinde kullanılan olduğunu yolları ele.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
