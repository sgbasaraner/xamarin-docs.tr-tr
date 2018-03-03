---
title: "WatchOS 3 için hızlı etkileşim teknikleri"
description: "Bu makalede Hızlı etkileşim teknikleri kapsamaktadır Apple watchOS 3 ve bunların içinde Xamarin.iOS için Apple Watch nasıl uygulanacağını ekledi."
ms.topic: article
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 75a8e807a68a3fccfa76fc7ba1f260818b25174d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="quick-interaction-techniques-for-watchos-3"></a>WatchOS 3 için hızlı etkileşim teknikleri

_Bu makalede Hızlı etkileşim teknikleri kapsamaktadır Apple watchOS 3 ve bunların içinde Xamarin.iOS için Apple Watch nasıl uygulanacağını ekledi._

Sağlayan hızlı kullanıcı etkileşimleri ilgi çekici Apple Watch uygulamaları ve zorluklar oluşturmak için gereklidir. Yeni watchOS 3 için Apple hareketi tanıyıcıları, dijital Dama yapma ve yeni kullanıcı bildirimi ve gezinti teknikleri erişim için destek eklendi. Bu, SceneKit ve SpriteKit, eklenen desteğinin yanı sıra izin Geliştirici hızlı ve esnek zengin, gözatılabilir arabirimleri kolayca oluşturun.

## <a name="what-are-quick-interactions"></a>Hızlı etkileşimleri nelerdir

İOS veya macOS (burada kullanıcının uygulamayla etkileşimi geçirdiği süreyi ölçülür dakika veya saat cinsinden) için uygulamalar oluşturmak için kullanılan bir geliştirici, başarılı bir uygulama için Apple Watch tasarlama zor olabilir ve farklı bir gerektirir yaklaşım.

WatchOS içinde kullanıcı genellikle kendi bileğe kadar yükseltmek, hızlı bir şekilde (genellikle kısa birkaç saniye için), bir uygulama ile etkileşim sonra bunların bileğe kadar bırakın ve ne olursa olsun, bunlar kaldığınız olduğundan devam etmek istiyor.

Tipik hızlı etkileşimler Apple Watch üzerinde bazı örnekleri şunlardır:

- Süreölçer başlatılıyor.
- Hava durumu denetleniyor.
- Yapılacaklar listesi devre dışı bir öğe işaretleniyor.

Bu hedeflere ulaşmak için Apple Watch üzerinde bir uygulama olmalıdır:

- **Gözatılabilir** -hızla kullanıcının bakış hangi anlamına gelir ihtiyaç duydukları bilgileri elde edebilirsiniz. 
- **İşlem yapılabilir** -kullanıcıların başka bir deyişle, hızlı, iyi bilinçli kararlar gerekir.
- **Esnek** -kullanıcı başka bir deyişle, hiçbir zaman gereksinim duydukları bilgileri almak için istenen eylem elde etmek için bekleyin veya.

### <a name="quick-interactions-length"></a>Hızlı etkileşimleri uzunluğu

Hızlı etkileşim ideal uzunluğu iki saniye olmalıdır Apple Watch uygulamaları gözatılabilir yapısı nedeniyle, Apple öneren veya daha az. Bu iki ikinci sınır sonucu olarak, geliştirici hem tasarlama ve Apple Watch uygulama uygulama zaman önemli miktarda kullanması gerekir. 

## <a name="new-watchos-3-features-and-apis"></a>Yeni watchOS 3 özellikleri ve API'leri

Apple birkaç yeni özellik ve API'ler için Apple Watch uygulamalarını hızlı etkileşimleri ekleme Geliştirici yardımcı olmak için WatchKit eklemiştir:

- watchOS 3 kullanıcı girişi gibi yeni tür erişim sağlar:
    - Hareketi tanıyıcıları
    - Dijital Dama yapma döndürme 
- watchOS 3 görüntüleme ve bilgi, gibi güncelleştirme yeni yollar sağlar:
    - Geliştirilmiş tablo gezintisi
    - Yeni kullanıcı bildirimi framework desteği
    - SpriteKit ve SceneKit tümleştirmesi

Bu yeni özellikleri uygulayarak, geliştirici watchOS 3 uygulama Glanceable, Actionable ve Responsive olmasını sağlayabilirsiniz.

### <a name="gesture-recognizer-support"></a>Hareketi tanıyıcı desteği

Geliştirici hareketi tanıyıcıları iOS uyguladı, hareketi tanıyıcıları watchOS 3 nasıl çalıştığı ile bilgili olmalıdır. Yenilemek için hareketi tanıyıcıları, alt düzey dokunma olayları tanınabilir, önceden tanımlanmış hareketleri ayrıştırma nesneleridir.

watchOS 3 dört aşağıdaki hareketi tanıyıcıları destekler:

- Ayrık hareketleri türleri:
    - Manyetik hareketi (`WKSwipeGestureRecognizer`).
    - Dokunun hareketi (`WKTapGestureRecognizer`).
- Sürekli hareketi türleri:
    - Pan hareketi (`WKPanGestureRecognizer`).
    - Uzun tuşuna hareketi (`WKLongPressGestureRecognizer`).

Yeni hareketi tanıyıcıları birini uygulamak için Mac için Visual Studio tasarımcısında iOS içinde tasarım yüzeyine sürükleyin ve özelliklerini yapılandırın.

Kod içinde kullanıcı tarafından tetiklenen hareketi işlemek için tanıyıcı eylem yanıt verin. Yeniden, bu iOS işlenmiş gibi aynı şekilde gerçekleştirilir.

#### <a name="discrete-gesture-states"></a>Ayrık hareketi durumları

Ayrık hareketleri için hareketi tanınır ve durumunu bir eylem adlandırılır (`WKGestureRecognizerState`) olarak atanır:

[ ![](quick-interaction-techniques-images/quick01.png "Ayrık hareketi durumları")](quick-interaction-techniques-images/quick01.png)

Tüm ayrık hareketleri başlatmak `Possible` durumu ve geçiş ya da içine `Failed` veya `Recognized` durumu. Ayrık hareketleri kullanırken, geliştirici genellikle doğrudan durumuyla ilgili değil. Bunun yerine, hareketi yalnızca tanınan olduğunda çağrılan eylemini kullanır.

#### <a name="continuous-gesture-states"></a>Sürekli hareketi durumları

Sürekli hareketleri ayrık hareketi tanınan gibi eylemi birden çok kez burada çağrılır hareketleri, biraz farklılık gösterir:

[ ![](quick-interaction-techniques-images/quick02.png "Sürekli hareketi durumları")](quick-interaction-techniques-images/quick02.png)

Yeniden sürekli hareketleri başlatır `Possible` durumu, ancak bunlar ilerleme birden çok güncelleştirme. Burada Geliştirici tanıyıcı'nın durum göz önünde bulundurun ve uygulamanın UI sırasında güncelleştirme gerekecek `Changed` hareketi son kadar aşama `Recognized` veya `Canceled`.

#### <a name="gesture-recognizer-usage-tips"></a>Hareketi tanıyıcı kullanım ipuçları

Apple watchOS 3 hareketi tanıyıcıları ile çalışırken, şunları öneririz:

- Tek denetimleri yerine Grup öğelerine hareketi tanıyıcıları ekleyin. Apple Watch daha küçük bir fiziksel ekran boyutu olduğundan, Grup öğelerini büyük olma eğilimindedir ve kullanıcının isabet daha kolay hedefleri. Ayrıca, hareketi tanıyıcıları ile yerel UI denetimlerinde zaten hareketleri yerleşik çakışabilir.
- Bağımlılık ilişkileri izleme uygulamanın film şeridi ayarlayın.
- Bazı hareketi önceliklidir diğer hareketi türleri üzerinden gibi:
    - Kaydırma
    - Force Touch
 
### <a name="digital-crown-rotation"></a>Dijital Dama yapma döndürme

Bir geliştirici, kendi watchOS 3 uygulamalarında dijital Dama yapma desteği uygulayarak hızı ve duyarlık etkileşimler, kullanıcılar için artan Gezinti sağlayabilir.

2 watchOS bu yana Apple Watch uygulamanın kullanabilirsiniz `WKInterfacePicker` listesini sağlayarak dijital Dama yapma erişmek için nesne `WKPickerItems` ve bir seçici stili (liste, yığılmış veya görüntü sırası). watchOS sonra kullanıcının listeden bir öğe seçmek için dijital Dama yapma kullanmasına izin.

Kullanırken bir `WKInterfacePicker`, WatchKit tarafından işlerin çoğunu işleme:

- Çizim listesi ve tek tek arabirim öğeleri.
- Dijital Dama yapma olayları işleniyor.
- Bir öğe seçildiğinde bir eylem çağrılıyor.

Yeni watchOS 3 için geliştirici artık dönüş değerleri yanıt kendi kullanıcı Arabirimi öğeleri oluşturmak için bunları veren doğrudan dijital Dama yapma döndürme olayları erişimi olur.

Sayısal Dama yapma erişim aşağıdaki öğeleri tarafından sağlanır:

- `WKCrownSequencer` -Saniyede döndürmeleri erişim sağlar.
- `WKCrownDelegate` -Dönme delta olayları erişim sağlar.

#### <a name="rotations-per-second"></a>Saniye başına döndürme

Animasyon fizik çalışmak dayalı döndürmeleri saniye başına dijital Dama yapma erişme yararlıdır. Döndürme saniyede erişmek için `CrownSequencer` özelliği `WKInterfaceController` izleme uzantısı. Örneğin:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>Dönme farkları

Dönme farkları dijital Dama yapma alanından döndürmeleri sayısı için kullanın. Kullanım `CrownDidRotate` yöntemini geçersiz kılma `WKCrownDelegate` dönme farkları erişmek için. Örneğin:

```csharp
using System;
using WatchKit;
using Foundation;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class CrownDelegate : WKCrownDelegate
    {
        #region Computed Properties
        public double AccumulatedRotations { get; set;}
        #endregion

        #region Constructors
        public CrownDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void CrownDidRotate (WKCrownSequencer crownSequencer, double rotationalDelta)
        {
            base.CrownDidRotate (crownSequencer, rotationalDelta);

            // Accumulate rotations
            AccumulatedRotations += rotationalDelta;
        }
        #endregion
    }
}
```

Burada bir accumulator uygulamayı tutar (`AccumulatedRotations`) döndürmeleri sayısını belirlemek için. Bir tam dönüşünü dijital Dama yapma birikmiş bir delta eşittir `1.0` ve yarı döndürme olacağını `0.5`.

Apple Geliştirici nasıl güncelleştirilmesini UI öğesindeki değişiklikleri duyarlılığını döndürme sayıları eşleştiğini belirlemek için kadar bu ayrıldı.

Oturum açma (`+/-`) dönme Delta değeri kullanıcı dijital Dama yapma kapatma olduğunu yönü gösterir:

[ ![](quick-interaction-techniques-images/quick03.png "Kullanıcı dijital Dama yapma kapatma olduğunu yönü dönme Delta oturum gösterir")](quick-interaction-techniques-images/quick03.png)


Kullanıcı yukarı kaydırma olursa WatchKit pozitif farkları ve aşağı kaydırma, ardından negatif farkları, hangi yönlendirme kullanıcı izleme kartı olan olsun döndürülür, döndürür.

#### <a name="digital-crown-focus"></a>Dijital Dama yapma odak

Yalnızca diğer arabirim öğeleri gibi dijital Dama yapma odak kavramı vardır. Bu odak uzağa doğru dijital Dama yapma nasıl izleme kullanıcı etkileşim bağlı diğer arabirimi öğelerine gölgeye. 

Örneğin, aşağıdaki denetimleri dijital Dama yapma odak çalabilir:

- Picker
- Kaydırıcı
- Denetleyici kaydırma

Bu, kendi özel arabirim öğesi dijital Dama yapma odağı gerektiğinde belirlemek için geliştirici kadar olur. Apple yeni hareketi tanıyıcıları özel kullanıcı Arabirimi öğesi odakta kazanmak için kullanılması önerilir.

### <a name="vertical-paging"></a>Dikey disk belleği

Bir kullanıcı bir watchOS uygulaması Tablo görünümünde gider standart istenen veri parçası için kaydırın, ayrıntılı görüntülemek için ayrıntıları görüntülemeyi bitirdikten sonra geri düğmesine dokunun, belirli bir satırda'a dokunun ve diğer bilgiler için işlemi yineleyin yoludur, y ilgilendiğiniz öğesinden tablo içinde:

[ ![](quick-interaction-techniques-images/quick04.png "Bir tablo ayrıntı görünümü arasında taşıma")](quick-interaction-techniques-images/quick04.png)

Yeni watchOS 3 için geliştirici dikey disk belleği, Tablo görünümünde denetimlere etkinleştirebilirsiniz. Bu özellik etkinleştirildiğinde, kullanıcı bir tablo görünümü satırı bulun ve önce kendi ayrıntı olarak görüntülemek için satırı dokunun gezinebilirsiniz. Ancak, bunlar artık yukarı tablodaki veya select önceki satıra kadar sonraki satırı seçin (veya dijital Dama yapma kullanın), tüm tablo görünümü döndürün gerek kalmadan önce a doğru çekin:

[ ![](quick-interaction-techniques-images/quick05.png "Bir tablo ayrıntı görünümü arasında taşıma ve geçirmeyi yukarı ve aşağı diğer satırlar arasında taşımak için")](quick-interaction-techniques-images/quick05.png)

Bu modu etkinleştirmek için watchOS uygulamanın film şeridi Xcode'da düzenlemek için açın, Tablo görünümünde seçin ve denetleme **dikey ayrıntı disk belleği** onay kutusu:

[ ![](quick-interaction-techniques-images/quick06.png "Dikey ayrıntı disk belleği onay kutusunu işaretleyin")](quick-interaction-techniques-images/quick06.png)

Ayrıntılı Görünüm ve film şeridi için değişiklikleri kaydetmek ve eşitlemek Mac için Visual Studio dönmek için tablo Segues kullandığından emin olun.

Geliştirici program aracılığıyla dikey disk belleği bir tablo görünümü karşı aşağıdaki kodu kullanarak belirli bir satırdan görüşerek:

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

Dikey disk belleği kullanırken, geliştirici WatchKit önceden denetleyicilerinin otomatik olarak işleyecek ve kullanıcı arabirimini aslında görünür hale gelmeden önce sonuç olarak, bazı denetleyicisi yaşam döngüsü yöntemleri çağrılabilir farkında olması gerekir.

### <a name="notification-enhancements"></a>Bildirim geliştirmeleri

Bildirim birincil watchOS üzerinde genellikle bir kullanıcı deneyimleri hızlı etkileşim biçimidir ve watchOS 1 ve ilk Apple Watch itibaren kullanılabilir kapatıldı.

Tipik bir bildirim hızlı etkileşim aşağıdaki gibidir:

1. Yeni bir bildirim alındığında Kullanıcı bildirim Haptic hissettirir.
2. Bunlar, bildirim için kısa Ara arabirimi görmek için kendi bileğe kadar yükseltin.
3. Yükseltilmiş kendi bileğe kadar tutmak devam ederseniz, watchOS uzun Ara bildirim arabirimine otomatik olarak geçiş yapar.

Bir kullanıcı bildirimi yanıtlayın birkaç yolu vardır:

- Tanımlanan ve bildirim sunulan kutusu için kullanıcı hiçbir şey yapma ve yalnızca bildirim yok sayın.
- Ayrıca watchOS uygulamayı başlatmak için bildirim dokunun.
- Özel Eylemler destekleyen bir bildirimi için kullanıcı özel eylemlerden birini seçebilirsiniz. Bunlar olabilir:
    - **Ön plan Eylemler** -bu eylemi gerçekleştirmek için uygulamasını başlatın.
    - **Arka plan Eylemler** - watchOS 2 iPhone için her zaman yönlendirilecek ancak watchOS 3 içinde watchApp yönlendirilebilir.

Yeni watchOS için 3:

* Bildirim tüm platformlarda (iOS, watchOS, tvOS ve macOS) benzer bir API'yi kullanın.
* Yerel bildirim Apple Watch zamanlanabilir.
* Apple Watch zamanlanmış, arka plan bildirim uygulamanın uzantısı yönlendirilir.

#### <a name="notification-scheduling-and-delivery"></a>Bildirim zamanlama ve teslimat

Aşağıdaki oluştuğunda kullanıcının iPhone bildirimden İleri Apple Watch olacaktır:

* İPhone 's ekran kapalıdır.
* Apple Watch yıpranmış ve kilitlendi.

WatchOS 3, yerel bildirimler Apple Watch zamanlanabilir ve saatin yalnızca teslim edilir. Çalışır durumda uygulama tarafından gerekirse karşılık gelen bir iPhone bildirim zamanlamak için geliştirici.

Bildirim tanımlayıcısının Apple Watch ve bildirimleri iPhone sürümleri dahil olmak üzere yinelenen bildirimler saatin görüntülenmesini engeller. Bildirim Apple Watch sürümü iPhone sürümünün üzerine öncelikli olur.

WatchOS 3 aynı kullandığından `UINotification` iOS 10 olarak API çerçevesi bizim iOS 10 bakın [Kullanıcı bildirim Framework](~/ios/platform/user-notifications/index.md) daha fazla ayrıntı için belgeleri.

### <a name="using-spritekit-and-scenekit"></a>SpriteKit ve SceneKit kullanma

Yeni watchOS 3 için geliştirici artık SpritKit ve SceneKit nesneleri, uygulamanın kullanıcı arabirimi tasarımında 2B ve 3B grafik sunmak için kullanabilirsiniz.

Bu özelliği desteklemek için iki yeni arabirimi sınıflarını eklenmiştir:

- `WKInterfaceSKScene` -SpriteKit 2B grafiklerle çalışma için.
- `WKInterfaceSCNScene` -3B grafik SceneKit çalışma için.

Bu nesneler kullanmak için sadece bunları Xcode'nın arabirimi Oluşturucu izleme uygulamanın film şeridi içinde tasarım yüzeyine sürükleyin ve kullanmak **öznitelikleri denetçisi** onları yapılandırmak için.

Bir iOS uygulaması içinde yaptığı gibi bu noktasından SpriteKit veya SceneKit planda çalışmak aynı şekilde çalışır. İzleme uygulaması sunacaktır bir `WKInterfaceSKScene` birini çağırarak `Present` yöntemleri. SceneKit için basitçe ayarlamak `Scene` özelliği `WKInterfaceSCNScene` nesnesi.

## <a name="actionable-complications"></a>İşlem yapılabilir zorluklar

WatchOS 2, 3 taraf uygulamaları için Apple zorluklar kullanıma sunuldu. WatchOS 3'de, Apple geliştirici bir WatchKit olası içerebilir yeteneklerini genişletilmiştir. 

Ayrıca, daha fazla yerleşik izleme içinde yüzeyleri şimdi zorluklar içerebilir ve var olan izleme bakarken zaten desteklenen zorluklar artık dahil olduğunu daha da fazla zorluklar.

Ayrıca bir kullanıcı için hızlı bir şekilde sağdan sola veya sağa kendi Apple Watch yükledikleri izleme yüzeyleri tümünün aracılığıyla geçiş yapmak için yeni özelliğidir. Yeni Galeri Apple Watch ın Yardımcısı iPhone uygulamasını kullanarak, kullanıcı eklemek ve yeni izleme yüzeyleri ve dahil edebilirsiniz zorluklar hiçbirini özelleştirin.

Bu yeni özellikleri nedeniyle, her uygulama Apple Watch üzerinde en az bir olası içermesi gerekir ve bu nedenle, yerel Apple Watch uygulama artık zorluklar sahip Apple önerir.

Zorluklar bir uygulama için aşağıdaki özellikleri sağlar:

- Her zaman izleme yüz üzerinde mevcut olduğundan, yüksek oranda gözatılabilir.
- Karışıklıklardan watchOS tarafından sık sık güncelleştirilir. Kullanıcının şu anda görüntülenen izleme yüz üzerinde olası içeren herhangi bir uygulama en az iki saatte bir güncelleştirilir.
- Olası kullanıcının şu anda görüntülenen izleme yüz üzerinde herhangi bir uygulamayla hızlı başlatma uygulama yapmaz ve uygulama yanıt hızını artırır belleğinde tutulur.
- Zorluklar kullanıcının bir watchOS uygulamasında belirli işlevleri başlatmak kolay hale getirir.

## <a name="glanceable-notification"></a>Gözatılabilir bildirim

Apple Watch bildirim kullanıcı olayları veya gelen iletileri veya bir etkinlik uygulamasında bir amaca ulaşmada gibi yeni bilgiler hızlı bir şekilde bildirmek için harika, özelleştirilebilir bir yol sağlar.

Bir bildirim kullanarak değerli bilgiler, hızlı bir şekilde kullanıcıya sunulabilir. Çoğu durumda, iyi tasarlanmış bir bildirim kullanıcıya gerçekten uygulama başlatma gerekliliğini kaldırabilirsiniz.

Yeni watchOS 3, tüm bildirimler artık desteği için:

- SpriteKit
- SceneKit
- Satır içi Video

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>SpriteKit ve SceneKit ile geliştirilmiş kullanıcı Arabirimi

Genellikle, SpriteKit ve SceneKit belirtildiği zaman bir geliştirici oyun UI düşünebilirsiniz. Ancak, SpriteKit ve SceneKit oyun olmayan özelleştirilmiş düzenleri, içerik dahil Uı'lar ve aksi durumda WatchKit tek başına mümkün olmayan bir animasyon oluşturmak için yararlı olabilir.

Örneğin, paylaşım uygulamasının fotoğraf kullanıcı bildirimden SpriteKit gerçek görüntü ve aşağıdakilere kullanıcı deneyimini zenginleştirir özelleştirilmiş diğer bilgilerin yanı sıra resmi gönderilen kullanıcı dahil ederek zengin bir kullanıcı deneyimi sağlamak için kullanabilirsiniz.

Ayrıca, SpriteKit ve SceneKit standart WatchKit kullanıcı Arabirimi öğeleri uygulamanın kullanıcı arabirimi tasarım ile bir arada kullanılabilir.

## <a name="simple-navigation"></a>Basit gezinme

watchOS 3 sunar Geliştirici yeni gibi watchOS uygulamalarını içinde gezinme basitleştirebilir çeşitli yollardan [dikey disk belleği](#Vertical-Paging), [hareketi tanıyıcı Destek](#Gesture-Recognizer-Support) ve [dijital Dama yapma Döndürme](#Digital-Crown-Rotation) özellikleri sunulan üstünde.

Dijital Dama yapma Apple Watch benzersizdir ve gezinti basitleştirmek için birçok farklı şekillerde kullanılabilir. Örneğin, bir zamanlayıcı uygulaması dijital Dama yapma kullanılabilir Zamanlayıcı uzunlukları itmek için kullanabilirsiniz.

Özel hareketler bir izleme uygulaması ile etkileşim kurmak kullanıcı için yeni ve benzersiz şekilde sunabilir ve uygulama gezinti basitleştirmek için de kullanılabilir.

Apple önermek tümünü watchOS zengin, kolay ve hızlı watchOS uygulama arabirimler kullanacak şekilde sunmak için 3 eklenen yeni hızlı etkileşim Özellikleri birleştirmek için yollar aranıyor.

## <a name="finishing-the-quick-interaction"></a>Hızlı etkileşim tamamlama

İyi tasarlanmış hızlı etkileşim deneyimini kullanıcı kendi bileğe kadar bırakma (ve uygulamayla disengage için) güvenirlik verecektir bunlar bittiği geçerli etkileşim.

Bu özellikle olur burada izleme uygulama ağ bağlantısı herhangi bir türde yapılması ya da kendi yardımcı iPhone uygulamayla bilgi paylaşımı bir sorundur. İşlem sırasında hızlı etkileşimi arzu değil işlemi gerçekleşirken, bu genellikle bir bekleme göstergesi için yol açabilir. Aşağıdaki örnek alın:

[ ![](quick-interaction-techniques-images/quick07.png "Bir ağ bağlantısı yapılması ve kendi yardımcı iPhone uygulamayla bilgi paylaşımı izleme uygulama diyagramı")](quick-interaction-techniques-images/quick07.png)

1. Kullanıcı saatin satın almak için bir öğe seçer.
2. Bunlar satınalma düğmesine dokunun.
3. Uygulama ağ işlemi başlatır ve bir yükleme gösterge görüntüler.
4. Daha sonra bir süre işlem tamamlandıktan ve satın alma onayı uygulamayı görüntüler.
5. Kullanıcı kendi bileğe kadar bırakır ve uygulamayla disengages.

İşlem tamamlanana kadar kullanıcı satınalma düğmesi dokunur zamandan yükleme göstergesi arayan yükseltilmiş kendi bileğe kadar sahiptirler. Bu durumu çözmek için Apple Geliştirici yükleme göstergesi gösteren yerine kullanıcıya anında geri bildirim sunmalıdır önerir. 

Apple'nın önerilen modelini kullanarak yeniden aynı hızlı etkileşim göz atın:

[ ![](quick-interaction-techniques-images/quick08.png "Elmalar önerilen model diyagramı")](quick-interaction-techniques-images/quick08.png)

1. Kullanıcı saatin satın almak için bir öğe seçer.
2. Bunlar satınalma düğmesine dokunun.
3. Uygulama ağ işlemi başlatır ve satın alma başarıyla başlatıldığını belirten bir ileti görüntüler.
4. Kullanıcı kendi bileğe kadar bırakır ve uygulamayla disengages.
5. İşlem biraz zaman daha sonra başarıyla tamamlandığında, uygulama yerel bir kullanıcı başarılı bir satın alma hakkında bilgilendirmek için bildirim görüntüler.

Bunlar güvenle kendi bileğe kadar bırakabilir ve bu noktada hızlı etkileşim sonlandırmak için satın alma, başlatıldığını belirten bir ileti gösterilir satınalma düğmesi kullanıcı dokunur hemen sonra bu süre. Daha sonra bunlar başarılı ya da kullanıcı bildirimi işlemde hata bildirilir. Bu şekilde, kullanıcı yalnızca uygulamayla "active" işleminin aşamaları sırasında etkileşimde.

Ağ yapmakta olduğunuz uygulamalar için arka plan kullanabileceklerini `NSURLSession` indirme görevle ilgili ağ iletişimi işlemek için. Bu, indirilen bilgileri işlemek amacıyla arka planda uyandırılabilir uygulama izin verir. Arka plan işlemesi gerektiren uygulama için bir arka plan görevi onaylama gerekli işleme işlemek için kullanın.

## <a name="quick-interaction-design-tips"></a>Hızlı etkileşim tasarım ipuçları

İstenilen hızlı etkileşiminin iki saniye olduğundan veya daha az Geliştirici başından uygulamanın etkileşimlerini tasarım işleminin tasarım üzerinde durmalısınız. Burada bu etkileşimler (yukarıda sunulan teknik kullanılarak) Basitleştirilmiş ve uygulama hızlı ve esnek hale getirmek için watchOS 3'ün yeni özelliklerini kullanmak alanları bulun.

Apple aşağıdaki önerir:

- Hızlı etkileşimler için en fazla kullanılan özelliklerini ileri getirerek odaklanın.
- Ortak özellikler ve İşlevler yüzey için zorluklar ve kullanıcı bildirimleri kullanın.
- Zengin, gözatılabilir kullanıcı arabirimini SceneKit ve SpriteKit ile oluşturun.
- Mümkün olduğunda, uygulama içinde gezinti basitleştirin.
- Hiçbir zaman bekleyin, bunları kendi bileğe kadar bırakın ve uygulamayla mümkün olan en kısa sürede disengage izin kullanıcı olun.

## <a name="summary"></a>Özet

Bu makalede ele alınan hızlı etkileşim teknikleri Apple watchOS 3 ve bunların içinde Xamarin.iOS için Apple Watch nasıl uygulanacağını ekledi.

## <a name="related-links"></a>İlgili bağlantılar

- [watchOS örnekleri](https://developer.xamarin.com/samples/watchos/all/)
