---
title: "İPad için çoklu"
description: "iOS 9 aynı zamanda, slayt kullanılarak üzerinden veya bölme görünüm çalıştıran iki uygulamaları destekler. Ayrıca, resim, resim çalma video destekler."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 34b51f784b549caa0dda2eeda066bb39dfc13020
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="multitasking-for-ipad"></a>İPad için çoklu

_iOS 9 aynı zamanda, slayt kullanılarak üzerinden veya bölme görünüm çalıştıran iki uygulamaları destekler. Ayrıca, resim, resim çalma video destekler._

![](multitasking-images/about02-sml.png "Ekran örnek bölme") ![ ] (multitasking-images/about03-sml.png "resim içinde resim örneği")

iOS 9 iPad belirli donanım üzerinde aynı anda çalışan iki uygulamalar için çoklu desteği ekler. İPad için çoklu aşağıdaki özellikler desteklenmez:

- [**Slayt üzerinden** ](#Slide-Over) -kullanıcının bir slayt şu anda çalışan ana uygulama yaklaşık % 25 kapsayan paneli (ya da dil yöne bağlı ekran sağ veya sol tarafındaki) kullanıma ikinci bir iOS uygulaması bir geçici olarak çalıştırmasına izin verir. Slayt üzerinde yalnızca bir iPad Pro, iPad hava, iPad hava 2, iPad Mini 2, iPad Mini 3 ya da iPad Mini 4 kullanılabilir.
- [**Bölünmüş Görünüm** ](#Split-View) -desteklenen iPad donanım (iPad hava 2, iPad Mini 4 ve iPad Pro yalnızca), kullanıcı ikinci bir uygulama seçin ve çalıştırın yan yana bölünmüş ekran modu şu anda çalışan uygulamaya sahip. Kullanıcı, her uygulama kapladığı ana ekran yüzdesini denetleyebilir.
- [**Resim içinde resim** ](#Picture-in-Picture) - video içeriği kayıttan yürütmek, video artık şu anda iOS cihaz üzerinde çalışan diğer uygulamalar üzerinden gezinen taşınabilir ve yeniden boyutlandırılabilir penceresinde yürütülebilir uygulamalar için. Kullanıcı boyutunu ve konumunu bu penceresinin üzerinde tam denetime sahiptir. Resim içinde resim, yalnızca bir iPad Pro, iPad hava, iPad hava 2, iPad Mini 2, iPad Mini 3 ya da iPad Mini 4 kullanılabilir.

Ne zaman göz önünde bulundurmanız gerekenler sayıda [çoklu uygulamanızda destekleyen](#Supporting-Multitasking-in-your-App)dahil:

- [Ekran boyutuna ve yönüne](#Screen-Size-Considerations)
- [Özel donanım klavye kısayolları](#Custom-Hardware-Keyboard-Shortcuts)
- [Kaynak Yönetimi](#Resource-Management-Considerations)

Bir uygulama geliştiricisi olarak şunları da yapabilirsiniz [çevirin görevli,](#Opting-Out-of-Multitasking)dahil [PIP Video kayıttan yürütme devre dışı bırakma](#Disabling-PIP-Video-Playback).

Bu makalede Xamarin.iOS uygulamanızı doğru çoklu bir ortamda çalışır veya iyi değilse, çoklu dışında kabul etme uygulamanız için uygun emin olmak için gerekli olan adımları kapsar.

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**İPad, çoklu tarafından [Xamarin Üniversitesi](https://university.xamarin.com)**


<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>Çoklu hızlı başlangıç

Desteklemek için **slayt üzerinden** veya **bölünmüş görünümlü** uygulamanızı aşağıdakileri yapmanız gerekir:

 - İOS 9 (veya daha büyük) karşı yerleştirilmiş olabilir.
 - Film şeridi için başlatma, ekranı kullanın (ve varlıklar görüntü değil).
 - Film şeridi otomatik düzenleme ve boyutu sınıfları için kullanıcı Arabiriminde kullanın.
 - Tüm 4 iOS aygıtı yönler (dikey, görüntülemediğini dikey, yatay sola ve yatay sağ) destekler.

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>İPad için çoklu hakkında

iOS 9 iPad girişiyle üzerinde yeni görevli yeteneklerini sunar _slayt üzerinden_, _bölünmüş görünümlü_ (iPad hava 2, iPad Mini 4 ve iPad Pro yalnızca) ve _resim içinde resim_. Biz bu özellikleri yakından aşağıdaki bölümlerde sürer.

<a name="Slide-Over" />

### <a name="slide-over"></a>Üzerinden slayt

İkinci bir uygulama seçin ve hızlı etkileşim sağlamak için küçük bir kayan panelinde görüntülemek kullanıcının slayt üzerinden özelliği sağlar. Slayt üzerinden paneli geçicidir ve kullanıcı geri ana uygulamayla yeniden çalışmaya gittiğinde kapatılacak.

[![](multitasking-images/about01.png "Slayt üzerinden paneli")](multitasking-images/about01.png#lightbox)

Anımsaması ana kullanıcı yan yana ve geliştirici bu işlem üzerinde denetimi yoktur iki hangi uygulamaların çalışacağı karar şeydir. Sonuç olarak, slayt üzerinden panelinde Xamarin.iOS uygulamanızı düzgün çalıştığını emin olmak için gerçekleştirmeniz gereken birkaç şey vardır:

- **Otomatik düzenleme ve boyutu sınıfları kullanma** — Xamarin.iOS uygulamanızı şimdi slayt kullanıma yan panelinde çalıştırılabilir olduğundan, artık cihazın, ekran boyutuna veya Düzen yönünü UI güvenebilirsiniz. Uygulamanız, arabirim doğru şekilde ölçekler emin olmak için otomatik düzenleme ve boyutu sınıfları kullanmak gerekir. Daha fazla bilgi için bizim [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) belgeleri.
- **Kaynakları kullanabildiğinden** — uygulamanız şimdi sistem çalışan başka bir uygulamayla paylaşacak çünkü uygulamanızın sistem kaynaklarını verimli bir şekilde kullanıp kritik öneme sahiptir. Bellek seyrek hale geldiğinde sistem çoğu bellek tükettikten uygulama otomatik olarak sona erdirir. Apple'nın bkz [enerji verimliliği Kılavuzu iOS uygulamaları için](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) daha fazla ayrıntı için.

Slayt üzerinde yalnızca bir iPad Pro, iPad hava, iPad hava 2, iPad Mini 2, iPad Mini 3 ya da iPad Mini 4 kullanılabilir. Uygulamanız için üzerinden slayt hazırlama hakkında daha fazla bilgi için Apple'nın bkz: [benimsenmesi görevli geliştirmeleri ipad'de](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) belgeleri.

<a name="Split-View" />

### <a name="split-view"></a>Bölünmüş Görünümü

Kullanıcı desteklenen iPad donanım (iPad hava 2, iPad Mini 4 ve iPad Pro yalnızca), ikinci bir uygulama seçin ve çalıştırın yan yana bölünmüş ekran modu şu anda çalışan uygulamaya sahip. Kullanıcının her uygulama sürükleyerek kapladığı ana ekran yüzdesini denetleyebilir bir ekran ayırıcı.

[![](multitasking-images/about02.png "Bölünmüş Görünümü")](multitasking-images/about02.png#lightbox)

Slayt üzerinden gibi iki hangi uygulamaların yan yana çalışıyor olacak kullanıcı karar ve yeniden geliştirici bu işlem üzerinde denetimi yoktur. Sonuç olarak, bölme görünüm benzer gereksinimleri bir Xamarin.iOS uygulaması yerleştirir:

- **Otomatik düzenleme ve boyutu sınıfları kullanma** — Xamarin.iOS uygulamanızı şimdi kullanıcının belirtilen boyutta bir bölünmüş ekran modunda çalıştırılabilir olduğundan, artık cihazın, ekran boyutuna veya Düzen yönünü UI güvenebilirsiniz. Uygulamanız, arabirim doğru şekilde ölçekler emin olmak için otomatik düzenleme ve boyutu sınıfları kullanmak gerekir. Daha fazla bilgi için bizim [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) belgeleri.
- **Kaynakları kullanabildiğinden** — uygulamanız şimdi sistem çalışan başka bir uygulamayla paylaşacak çünkü uygulamanızın sistem kaynaklarını verimli bir şekilde kullanıp kritik öneme sahiptir. Bellek seyrek hale geldiğinde sistem çoğu bellek tükettikten uygulama otomatik olarak sona erdirir. Apple'nın bkz [enerji verimliliği Kılavuzu iOS uygulamaları için](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) daha fazla ayrıntı için.

Uygulamanız için bölme görünüm hazırlama hakkında daha fazla bilgi için Apple'nın bkz [benimsenmesi görevli geliştirmeleri ipad'de](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) belgeleri.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>Resim içinde resim

Yeni resim resim özelliği (olarak da bilinen _PIP_) kullanıcı ekranın üstünde çalışan diğer uygulamalar üzerinde herhangi bir yere küçük, kayan bir pencere olarak bir video izlemek kullanıcının sağlar.

[![](multitasking-images/about03.png "Bir örnek resim kayan pencere resmi")](multitasking-images/about03.png#lightbox)

Olarak resim modunda resimdeki bir video izlemeyi üzerinde tam denetim kullanıcının slayt üzerinden ile bölünmüş görünüm vardır. Uygulamanızın main işlevi videoyu izlemek için ise doğru PIP modunda davranmasına bazı değiştirilmesi gerekir. Aksi takdirde, değişiklik PIP desteklemek için gerekli değildir.

Uygulamanıza PIP video kullanıcının isteği üzerine görüntülemek, ya da kullanıyor olmanız gerekecek _AVKit_ veya _AV Foundation API_. Media Player framework iOS 9 amorti ve PIP desteklemiyor.

Resim içinde resim, yalnızca bir iPad Pro, iPad hava, iPad hava 2, iPad Mini 2, iPad Mini 3 ya da iPad Mini 4 kullanılabilir. Daha fazla bilgi için lütfen bkz bizim [PictureInPicture örnek uygulaması](https://developer.xamarin.com/samples/ios/iOS9/) ve Apple'nın [Resim Hızlı Başlangıç resimde](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) belgeleri.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>Uygulamanızda görevli destekleme

Apple'nın tasarım kılavuzları ve en iyi uygulamaları iOS 8 için uygulamanızı zaten izlediği sürece varolan tüm Xamarin.iOS uygulaması için çoklu destekleyen bir saydam bir görevdir. Bu uygulama film şeritleri otomatik düzenleme ve boyutu sınıfları için kendi kullanıcı arabirimi düzenleri kullanılmalı olduğunu anlamına gelir (bkz bizim [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) daha fazla bilgi için).

Bu uygulamalar için çoklu desteklemek ve iyi içinde davranması için çok az kayıpla veya hiç değişiklik gerekmez. Uygulamanızın UI oluşturulduğu doğrudan konumlandırma ve C# kodu ya da belirli cihaz ekran boyutlarına veya yönler kullanır, kullanıcı Arabirimi öğeleri boyutlandırma gibi diğer yöntemleri kullanarak, iOS 9 görevli doğru desteklemek için önemli değişiklik gerekir.

İOS 9 görevli üzerinde yeni bir Xamarin.iOS uygulaması desteklemek için yeniden film şeritleri otomatik düzenleme ve boyutu sınıfları ile tüm uygulamanın kullanıcı arabirimi düzenleri için kullanın ve aşağıdaki bölümlerde yönergeleri uygulayın.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>Ekran boyutu ve yön konuları

İOS 9 önce uygulamanızın belirli cihaz ekran boyutlarına ve yönler karşı tasarım. Bir uygulamayı şimdi bir slayt çıkışı panelinde veya bölme görünüm modunda çalıştırılabilir çünkü kendisi ya da cihazın fiziksel yönü veya ekran boyutuna bakılmaksızın iPad compact veya normal yatay boyutu sınıfı çalışan bulabilirsiniz.

[![](multitasking-images/sizeclasses01.png "Ekran boyutu ve yön konuları")](multitasking-images/sizeclasses01.png#lightbox)

Bir iPad cihazında bir tam ekran uygulaması normal yatay ve dikey boyutu sınıfları içerir. Tüm iPhone ancak iPhone 6 Plus ve iPhone 6s Plus herhangi bir yönde her iki yönde Compact boyutu sınıfları sahiptir. İPhone 6 Plus ve iPhone 6s Plus yatay modunda bir normal yatay boyutu ve bir sıkıştırılmış dikey boyutu sınıfı (çok Mini iPad gibi) sahip.

Slayt üzerinden ve bölünmüş görünümlü destekleyen iPad cihazları üzerinde aşağıdaki birleşimlerine düşebilir:

| **Yönlendirme** | **Birincil uygulama** | **İkincil uygulama** |
|--- |--- |--- |
| **Dikey** |ekranın %75<br />Compact yatay<br />Normal dikey|Ekran % 25'ini<br />Compact yatay<br />Normal dikey|
| **Yatay** |ekranın %75<br />Normal yatay<br />Normal dikey|Ekran % 25'ini<br />Compact yatay<br />Normal dikey|
| **Yatay** |Ekran % 50'si<br />Compact yatay<br />Normal dikey|Ekran % 50'si<br />Compact yatay<br />Normal dikey|

Örnekte [MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) yatay modunda iPad cihazında tam ekran çalıştırılırsa, uygulama, onu düzenleyecek liste ve aynı anda ayrıntı görünümü:

[![](multitasking-images/sizeclasses03.png "Liste ve aynı anda sunulan ayrıntılı Görünüm")](multitasking-images/sizeclasses03.png#lightbox)

Aynı uygulama slayt üzerinden panelinde çalıştırırsanız Compact yatay boyutu sınıf olarak düzenlendiği ve yalnızca listesini görüntüler:

[![](multitasking-images/sizeclasses04.png "Yalnızca aygıt yatay olduğunda sunulan listesi")](multitasking-images/sizeclasses04.png#lightbox)

Uygulamanız bu durumlarda düzgün şekilde davranan emin olmak için ayırdedici nitelik koleksiyonları boyutu sınıfları birlikte benimsemeyi ve gerekir uygun `IUIContentContainer` ve `IUITraitEnvironment` arabirimleri. Apple'nın bkz [UITraitCollection sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) ve bizim [film şeritleri birleşik giriş](~/ios/user-interface/storyboards/unified-storyboards.md) daha fazla bilgi için Kılavuzu.

Ayrıca, uygulamanın görünür alanını tanımlamak için cihazları ekran sınırları artık güvenebilirsiniz, uygulamanızın penceresi sınırları kullanmanız gerekir. Pencere sınırları tam kullanıcı denetimi altında olduğundan, programlı olarak bunları ayarlamak veya kullanıcının bu sınırlar değiştirmesini engellemek.

Son olarak, uygulamanızı bir film şeridi dosya kendi başlatma kümesi kullanılarak aksine ekran sunmak için kullanmanız gerekir **.png** görüntü dosyaları ve tüm dört arabirimi yönler (dikey, görüntülemediğini dikey, yatay sola ve yatay sağ) desteği bir slayt üzerinden panelinde veya bölme görünüm modunda çalıştırmak için kabul edilmesi için.

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>Özel donanım klavye kısayolları

İçinde iOS 9 iPad üzerinde çalışan, Apple donanımı klavyelerde için genişletilmiş destek vardır. iPad cihazları her zaman Bluetooth ve fiziksel iOS özel anahtarları dahil bazı klavye oluşturulan üreticileri klavyeler aracılığıyla temel dış klavye destek eklediniz.

Şimdi, iOS 9 uygulamaları kendi özel klavye kısayolları oluşturabilirsiniz. Ayrıca, bazı temel klavye kısayolları gibi kullanılabilir **komutu-C** (kopya) **Command-X** (kesme), **komutu-V** (yapıştırın) ve **komutu-Shift-H**  (giriş), özel olarak yazılmış yanıt bunları olan bir uygulama olmadan.

**Komut sekmesi** hızla klavyeden MAC'te benzer uygulamalar arasında geçiş yapmasına izin veren bir uygulama değiştirici getirir:

[![](multitasking-images/keyboard01.png "Uygulama değiştirici")](multitasking-images/keyboard01.png#lightbox)

İOS 9 uygulama klavye kısayolları içeriyorsa, kullanıcı üzerinde tutabilir **komutu**, **seçeneği** veya **denetim** açılan penceresinde görüntülemek için anahtarları:

[![](multitasking-images/keyboard02.png "Klavye kısayolları açılan")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>Özel klavye kısayollarını tanımlama

Bu görünüm veya denetleyicisi görünür olduğunda aşağıdaki kod bir görünüm veya Görünüm denetleyicisini bizim uygulamada, eklediğimiz, özel klavye kısayolu kullanılabilir olacaktır:

```csharp
#region Custom Keyboard Shortcut
public override bool CanBecomeFirstResponder {
    get { return true; }
}

public override UIKeyCommand[] KeyCommands {
    get {

        var keyCommand = UIKeyCommand.Create (new NSString("n"), UIKeyModifierFlags.Command, new Selector ("NewEntry"), new NSString("New Entry"));
        return new UIKeyCommand[]{ keyCommand };
    }
}

[Export("NewEntry")]
public void NewEntry() {

    // Add a new entry
    ...

}
#endregion
```

İlk olarak, biz geçersiz kılma `CanBecomeFirstResponder` özelliği ve return `true` şekilde görünüm veya Görünüm denetleyicisini klavye girdisi alabilir. 

Ardından, biz geçersiz kılma `KeyCommands` özelliği ve yeni bir `UIKeyCommand` için **komut-N** tuş vuruşu. Tuş vuruşu etkinleştirildiğinde diyoruz `NewEntry` yöntemi (iOS 9'u kullanarak kullanıma `Export` komutu) istenen eylemi gerçekleştirmek için.

Biz bu uygulama bir iPad cihazında bir donanım klavye takılı ve kullanıcı türleri ile çalıştırırsanız **komut-N**, yeni bir giriş listesine eklenir. Kullanıcı üzerinde tutuyorsa **komutu** anahtar, kısayollarının listesi görüntülenir:

[![](multitasking-images/keyboard03.png "Klavye kısayolları açılan")](multitasking-images/keyboard03.png#lightbox)

Lütfen örnek bakın [MultiTask uygulama](http://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) örnek uygulama.

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>Kaynak yönetim değerlendirmeleri

İOS 8'ın tasarım kılavuzları ve en iyi yöntemler zaten kullanıyorsanız bile uygulamalar için verimli kaynak yönetimi hala bir sorun olabilir. Uygulamalar, iOS 9'da, bellek, CPU veya diğer sistem kaynaklarını özel kullanım artık yok.

Sonuç olarak, sistem kaynaklarını verimli kullanmak üzere Xamarin.iOS uygulamanızı hassas veya yetersiz bellek durumlar altında sonlandırma yüzler. Bu çoklu çevirin uygulamalar eşit true, saniyenin bu yana uygulama hala bir slayt üzerinden panelinde veya saniyede 60 çerçeve altına düşmesine resim resmi ek kaynakları gerektiren veya yenileme hızı neden penceresinde çalıştırılması.

Aşağıdaki kullanıcı eylemleri ve bunların etkilerini göz önünde bulundurun:

- **Bir slayt üzerinden panelinde metin girerek** -uygulamanızı herhangi bir metin giriş olsa bile, sistem klavye artık kendi kullanıcı Arabirimi görüntülenebilir. Sonuç olarak, uygulama (örneğin, gösterme ve gizleme klavye) klavye ekran bildirimlerini yanıt gerekebilir.
- **İkinci bir uygulamayı bir slayt üzerinden panelinde çalıştıran** -yeni uygulama ön planda çalışmıyor şimdi ve bellek ve CPU döngüsü gibi sistem kaynakları için varolan uygulama rekabet.
- **PIP penceresinde bir Video oynatma** - değil yalnızca bu pencerede, uygulamanızın arabiriminin parçası kapak, ancak video başlatılan uygulama hala arka planda çalışan ve CPU ve bellek kaynakları kullanma.

Uygulamanızı kaynaklarını verimli bir şekilde kullandığından emin olmak için aşağıdakileri yapmalısınız:

- **Profil Araçları ile uygulama** -bellek sızıntılarını, overt CPU kullanımı ve burada uygulama engelliyor olabilir ana iş parçacığı alanları kontrol edin.
- **Durumu geçişleri yöntemlerine yanıt** - de, **AppDelegate.cs** dosya geçersiz kılma ve durum yanıta girmek veya arka planından döndüren uygulama gibi yöntemleri değiştirin. Tüm gereksiz süslü varlıkları görüntüler, verileri veya görünümleri ve görünüm denetleyicisini gibi bırakın.
- **Yana birimi bellek yoğun uygulamalar ile test** - bellek yoğun gibi bir uygulama eşlemeleri (uydu görünüm modunda) Slayt çıkışı hem fiziksel iOS donanımda bölünmüş görünümü kullanarak uygulamanızı çalıştırma ve test hem uygulamaları yanıt verebilir durumda kalması ve kilitlenme değil.

Apple'nın bkz [enerji verimliliği Kılavuzu iOS uygulamaları için](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) kaynak yönetimi hakkında daha fazla bilgi için.

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>Çoklu kullanmama-genişletme

Vardır tüm iOS 9 uygulamaları çoklu destekler Apple öneren olsa da, bir uygulama için belirli nedenleri oyunlar veya düzgün çalışması için tam ekran gerektiren kamera uygulamalar gibi değil de olabilir.

Projenin Xamarin.iOS uygulamanızı panelinde ya da bir slayt çıkışı veya bölme görünüm modunda çalıştırın çevirin düzenleme **Info.plist** dosya ve denetleme **gerektirir tam ekran**:

[![](multitasking-images/fullscreen01.png "Çoklu kullanmama-genişletme")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> **Not:** slayt çıkışı veya bölme görünüm çalışacak uygulamanızı görevli Opting genişletme engeller, ancak **değil** başka bir uygulama slayt çıkışı veya resim video resimde ile birlikte görüntülemede çalıştırılmasını engellemek için uygulama.




<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>PIP Video oynatmayı devre dışı bırakma

Çoğu durumda, uygulamanızı bir resim resmi kayan penceresinde görüntüler herhangi bir video içerik yürütmek kullanıcı izin vermelidir. Ancak, burada bu, oyun Kes Sahne videolar gibi istenen değil durumlar olabilir.

PIP video çalınmasını çevirin için uygulamanızı aşağıdakileri yapın:

 - Kullanıyorsanız bir `AVPlayerViewController` video görüntülenecek ayarlamak `AllowsPictureInPicturePlayback` özelliğine `false`.
 - Kullanıyorsanız `AVPlayerLayer` video görüntülemek için örneği olmayan bir `AVPictureInPictureController`.
 - Kullanıyorsanız bir `WKWebView` video görüntülenecek ayarlamak `AllowsPictureInPictureMediaPlayback` özelliğine `false`.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.iOS uygulaması çalıştırın ve iPad cihazları iOS 9'ın yeni görevli becerisini de düzgün çalışmasını sağlamak için gerekli adımlar ele. Buna ek olarak, iyi bir tercihtir olduğu olmayan uygulamalar için çoklu kullanmama-giden ele.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [(Örnek) koşturmak](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [Birleşik film şeritleri giriş](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Çoklu geliştirmeleri iPad cihazında uygulamasını kullanma](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [blog gönderisi](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
