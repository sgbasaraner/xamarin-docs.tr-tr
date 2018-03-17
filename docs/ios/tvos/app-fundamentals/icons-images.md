---
title: "Simgeler ve görüntüleri ile çalışma"
description: "Bu makalede tasarlama ve simgeler ve Xamarin.tvOS uygulama içinde görüntülerle çalışma kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 3e3d1663e07b16721d1aa7253e7d0150a609718e
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="working-with-icons-and-images"></a>Simgeler ve görüntüleri ile çalışma

_Bu makalede tasarlama ve simgeler ve Xamarin.tvOS uygulama içinde görüntülerle çalışma kapsar._

Büyüleyici oluşturma simgeler ve görüntülerin olan Apple TV uygulamalarınız için derinlikli kullanıcı deneyimini geliştirmek için önemli bir bölümüdür. Bu kılavuz oluşturmak ve Xamarin.tvOS uygulamalarınız için gerekli grafik varlıkları eklemek için gerekli olan adımları kapsar:

- [Görüntü başlatma](#Launch-Image) -uygulamanızı ilk başlatıldığında ve başlatma tamamlandıktan sonra uygulamanın ilk ekran tarafından değiştirilirse başlatma görüntüsünü görüntüler.
- [Katmanlı görüntüleri](#Layered-Images) - Apple TV, Apple'nın yeni katmanlı görüntüleri çalışmak için seçilen öğeler 3B efekti oluşturmak için Parallax etkisi özgüdür. Birkaç yolu vardır [katmanlı görüntüleri oluşturmak](#Creating-Layered-Images).
- [Uygulama simgesi](#App-Icons) -simgeler, yalnızca Apple TV giriş ekranında için ancak App Store için gereklidir. Katmanlı bir görüntü olarak sağlanmalıdır.
- [Top raf görüntü](#Top-Shelf-Image) -uygulamanızı giriş ekranın üst satırda yerleştirdiyseniz, uygulamanızın özelliklerini vurgulamak için bir üst raf görüntüsü gerekir. İsteğe bağlı olarak, sağlayabilir [dinamik üst raf içerik](#Dynamic-Top-Shelf-Content) uygulamanızdaki içeriklerin vurgulayın.
- [Oyun merkezi görüntüleri](#Game-Center-Images) -uygulamanızı oyun olduğu ve oyun merkezi kullanıyorsa, birkaç ek görüntüleri gerekli olacaktır.
- [Xamarin.tvOS proje görüntüleri ayarlama](#Setting-Xamarin.tvOS-Project-Images) -uygulama simgesine ve başlatma görüntü Xamarin.tvOS uygulamanız için ayarlamak için gerekli adımlar kapsanmaktadır.

> [!IMPORTANT]
> **Not:** Apple TV tüm görüntülerinde 1 x çözünürlüğü olan (`@1x`) ve yapmanız gerekenler _yalnızca_ görüntüler bu boyutunu kullanır. Daha büyük dahil, daha yüksek çözünürlükte grafik yalnızca karşıdan yüklemek ve daha fazla bellek ve depolama alanı kullanmak için zaman, ancak çalışma zamanında dinamik olarak boyutlandırılan gerekir ve çizim performansı olumsuz etkiler.

<a name="Launch-Image" />

## <a name="launch-image"></a>Görüntü başlatma

Apple TV Xamarin.tvOS uygulamanızı ilk kez başlatıldığında görüntülenen ilk şey başlatma görüntüdür ve bu nedenle, her tvOS uygulama başlatma görüntü sağlamanız gerekir. 

Başlatma görüntü hızlı bir şekilde görüntülenir ve uygulamanızı hızlı ve esnek izlenim verir. Apple TV başlatma görüntü, uygulamanızın ilk ekranla kısa süre içinde var. sonra yerini alır.

Başlatma görüntüleri reklamları veya Artistik deyim için bir fırsat değildir, bunlar yalnızca uygulamanızı hızlı bir şekilde başlatır ve hazır izlenim vermek için mevcut kullanmak için.

|Görüntü boyutu başlatma|Notlar|
|---|---|
|1920x1080px|Yalnızca olmayan katmanlı .png dosyaları|

Apple, uygulamanızın başlatma görüntü tasarlamak için aşağıdaki önerileri sağlar:

- **Neredeyse aynı ilk ekranda** -bilgisayarınızı başlatma ekranı, uygulamanızın ilk ekran olarak mümkün olduğunca yakın olması gerekir. Farklı bir grafik veya öğenin de dahil olmak üzere bir sinir bozucu içinde "ilk ekran görüntülendiğinde flash" neden olabilir.
- **Kullanarak metin kaçının** -başlatma görüntüleri statik ve bu nedenle, görüntülenmeden önce yerelleştirilmiş olabilir değil.
- **Başlatma downplay** -çünkü Apple TV kullanıcılar sık geçiş uygulamalar, uygulama başlatma işlemine dikkat çekmek döndürmemelidir.
- **Herhangi bir reklam veya markalama** -görüntünüzü başlatma hakkında ekranında kullanılmamalıdır veya uygulamanızın ilk ekran statik parçası olmadığı sürece tüm marka içerir. Reklam kesinlikle yasaktır.

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>Başlatma görüntü ayarlama

TvOS projeniz için başlatma resmi ayarlamak için lütfen aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Assets.xcassets` düzenlemek üzere açmak için: 

    [![](icons-images-images/asset01.png "Assets.xcassets dosyası")](icons-images-images/asset01.png#lightbox)
2. İçinde **varlık Düzenleyicisi**, tıklayın `LaunchImages` varlık: 

    [![](icons-images-images/asset02.png "LaunchImages varlık")](icons-images-images/asset02.png#lightbox)
3. Tıklayın **Apple TV x 1** girişi başlatma görüntüsünü seçin ve isteğe bağlı olarak yeni bir görüntü sürükleme dosya sisteminden: 

    [![](icons-images-images/asset03.png "Başlatma görüntüsünü seçin")](icons-images-images/asset03.png#lightbox)
4. Değişikliklerinizi kaydedin.

<a name="Layered-Images" />

## <a name="layered-images"></a>Katmanlı görüntüleri

Apple TV, kullanıcının ekran içeriğini bağlı arasında yer şirketine yönelik kanepenizde tutmak için yardımcı olan bir 3B efekti üretmek için Parallax etkisi katmanlı görüntüleri çalışmak için yeni.

Katmanlı görüntüleri içeren iki (2) için beş (5) gelen ayrı tam bir görüntü oluşturmak için birleştirilir katmanları. Arka plan katmanına hariç olmak üzere her katman bir derinliği atmosferini oluşturmak için saydamlığı yanı sıra kendi Z düzenini kullanır. Katmanlı bir görüntüyle kullanıcı etkileşim kurduğunda, daha yüksek Z sıralı Katmanlar ölçeklenir ve bu efekti oluşturmak için çakışan.

[![](icons-images-images/layered01.png "Katmanlı görüntüleri Z sıralı diyagramı")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> **Not:** Layered görüntüleri, uygulamanızın simgelerini gereklidir ve diğer isteğe bağlı [odaklanabilir öğeleri](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (örneğin, üst raf görüntü). Ancak, Apple odak uygulamanızda alabilirsiniz herhangi bir görüntü için katmanlı görüntüleri kullanılmasını önerir.




Apple katmanlı görüntülerinizi tasarlamak için aşağıdaki önerileri sağlar:

- **Arka plan katmanı opak** -arka plan katmanınız (Katman 1) **gerekir** donuk veya Apple TV katmanlı görüntü kullanmaya çalıştığınızda bir hata iletisi alırsınız. Diğer tüm katmanları 3B efekti geliştirmek için saydamlığı birden çok düzeyi içerebilir.
- **Ön plan, Orta ve arka plan öğeleri yalıtmak** -ön planda belirgin öğeleri (örneğin, oyun karakter) koyun, Orta ikincil öğeleri ya da shadows için kullanın. Son olarak, bir aşama için üst katmanları sağlamak için dilden bağımsız bir arka plan içerir.
- **Ön planda metni değiştirmeyin** -genellikle metninizi tarafından yüksek düzeylerden görünmeyebilir istemediğiniz sürece, üst katmandaki olmalıdır.
- **Basit katmanlama kullanmak** -Parallax etkisi Zarif olacak şekilde tasarlanmıştır böylece katmanları jarring, belirtmeyi gerçekçi etkileri önlemek için bir minimal için tutun.
- **Güvenli bölge dahil** -üst katman Parallax etkisi sırasında kırpılmış olabilir çünkü her katmana güvenli bölge kenarlık yapı gerekir. İçeriğinizi çok Kapat Katmanlar kenar alırsanız, onu devre dışı kırpılmış hale. Üst katman daha fazla ölçeklendirme ve daha düşük katmanları kırpma karşılaşırsınız. Bkz: [boyutlandırma görüntü katmanlarını](#Sizing-Image-Layers) bölümüne bakın.
- **Önizleme genellikle** -katmanlı görüntüleri önizlemesi genellikle istenilen 3B efekti oluştuğunu ve tek tek katmanları içerik hiçbiri kırpılır gerektiğini emin olmak için. Katmanlı beklendiği gibi işleme emin olmak için görüntülerinizi gerçek Apple TV donanımda gözden geçirmelisiniz.

Mümkün olduğunda, yerleşik her zaman kullanmalısınız `UIKit` odak geldiğinizde bunlar otomatik olarak Parallax etkili aldıkça, katmanlı görüntüleri göstermek için denetimleri.

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>Boyutlandırma görüntü katmanları

Eklenecek unutulmaması önemlidir; bir _güvenli bölge_ katmanlı görüntünüzü oluşturan her katman kenarlığa. Tek tek katmanları, ölçeklendirilebilir ve sırasında Parallax etkisi kırpılmış olduğundan çok katmanın kenarına yakın ise katmanların içeriğini kapalı kırpılacağını:

[![](icons-images-images/layered02.png "35 piksel kenarlığı")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>Katmanlı görüntüleri oluşturma

tvOS katmanlı görüntülerle aşağıdaki biçimlerde çalışır:

- **ARABA dosyaları** -Apple tarafından oluşturulan özel bir varlık Kataloğu biçimi budur. ARABA dosyaları doğrudan oluşturmazsanız, tüm LSR dosyalarından derleme zamanında oluşturulan ve uygulama paketine eklenen.
- **LSR görüntüleri** -Apple tarafından oluşturulan özel görüntü biçimi budur. Kullanım [Parallax dışarı aktarma Adobe Photoshop eklentisi](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) veya [Parallax Önizleyicisi](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) oluşturmak ve LSR biçiminde katmanlı görüntüleri önizleme için.
- **Assets.xcassets** - gelen iki (2) için beş (5) standart `.png` bir ARABA veya LSR içine derlenmiş bir varlık kataloğunda bulunan biçimlendirilmiş görüntüleri derleme zamanında katmanlı görüntü biçimlendirilmiş.
- **LCR dosyaları** -bu Apple tarafından oluşturulan bir özel dosya biçimidir. LCR dosyaları, içerik sunucularının birinden indirilen ek içerik olarak kullanılmak üzere tasarlanmıştır. LCR dosya hiç uygulama pakette yer alması gerekir. LCR dosyaları LSR veya Photoshop dosyaları kullanılarak oluşturulan `layerutil` Xcode ile içerdiği komut satırı aracı.

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>Parallax genele gitmeyi

Oluşturulan Apple [Parallax Önizleyicisi](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) Önizleme ve oluşturulan katmanlı uygulama simgeleri ve isteğe bağlı odaklanabilir öğeleri için gereken görüntüler. Genele gitmeyi tamamlanmış katmanlı görüntü forms her katman gösterir:

[![](icons-images-images/layered03.png "Parallax genele gitmeyi")](icons-images-images/layered03.png#lightbox)

Katmanlı bir görüntü önizlerken görüntü döndürmek ve Parallax efekti önizlemek için fareyi kullanabilirsiniz. Kullanım  **+**  (artı) ve  **-**  (eksi) düğmelerini katman ekleme ve kaldırma.

Yeni bir katmanlı görüntüsü oluştururken LSR formatında dışa ve uygulamanızın pakete eklenen.

Apple'nın oluşturma ve katmanlı görüntüleri önizleme daha fazla bilgi için lütfen bkz [Parallax resim oluşturma](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) bölümünü [tvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/).

<a name="App-Icons" />

## <a name="app-icons"></a>Uygulama simgeleri

Xamarin.tvOS uygulamanızı Apple TV giriş ekranı, aynı zamanda App Store için bir simge için yalnızca bir uygulama simge gerektirir. Uygulama simgesi ilk olası kullanıcınız harika izlenim olarak değiştirin ve sonra uygulamanızın amacı bir bakışta iletişim kurmanız gerekir.

[![](icons-images-images/icon01.png "Uygulama simgesi")](icons-images-images/icon01.png#lightbox)

Her uygulama, küçük ve büyük bir uygulama simgesini sürümü sağlamanız gerekir. Uygulama yüklendiğinde Apple TV giriş ekranında küçük simge kullanılır. Büyük sürüm App Store tarafından kullanılır. Büyük uygulama simge küçük simge sürüm Görünüm ve yapısını taklit etmelidir.

|Küçük simgesi||Büyük simge||
|---|---|---|---|
|Gerçek Boyut|400x240px|Boyut|1280x768px|
|Güvenli bölge boyutu|370x222px|||
|Odaksız boyutu|300x180px|||
|Odaklanmış boyutu|370x222px|||

> [!IMPORTANT]
> Uygulama simgeleri olarak sağlanmalıdır **katmanlı görüntüleri**. Lütfen bakın [katmanlı görüntü](#Layered-Images) daha fazla ayrıntı için bölüm üstünde.




Apple App simgelerini oluşturmak için aşağıdaki önerileri sağlar:

- **Bir tek odak noktası sağlamanız** – tasarım tek odak noktası, simgesiyle doğrudan simgesi Merkezi'nde yerleştirilir.
- **Basit bir arka plan kullanmak** – üst katmanları göze böylece simgesi arka planınızı basit tutun. Basit renk veya Zarif gradyan kullanmayı düşünün.
- **Metin miktarını sınırlamak** – kullanıcı tarafından seçilen uygulamanın adını simgenin altında görünür olduğundan, simgeyi tasarım için gerekli olduğunda, yalnızca metin içermesi gerekir.
- **Ekran görüntüleri kullanmayın** – ekran görüntüleri için bir simge çok karmaşık ve uygulamanızı bir bakışta amacı görmek kullanıcının izin verilmez.
- **Canlı simgeler kare** – tvOS dilden çok az simgelerini köşelerinde yuvarlar maskesi otomatik olarak uygular. Kendiniz bu yuvarlama eklemeyin.
- **Dikkatle bilgisayarınızı katmanları ayrı** – metin çoğu katman, Orta ve görünmek üzere üst katmanları imkan tanıyan bağımsız bir arka plan ikincil öğeler üzerinde büyük olması gerekir.
- **Gradyan ve gölgeleri dikkatle kullanın** – gradyan ve gölge artar olmadan Parallax etkin şekilde dikkatle kullanılmalıdır. Basit üst alta, açık ve koyu gradyan stilleri en iyi çalışır. Gölgeleri normalde sharp, keskin kenarlı tonlarını olarak en iyi çalışır.
- **Katman saydamlığı farklılık** – uygulama 3B efekti artırmak için simge üst düzeyde saydamlığı düzeylerini değişen kullanın. Donuk arka plan katmanı olmalıdır veya bir hatayla sonuçlanır.

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>Uygulama simgeleri ayarlama

TvOS projeniz için gereken uygulama simgeleri ayarlamak için lütfen aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Assets.xcassets` düzenlemek üzere açmak için: 

    [![](icons-images-images/asset01.png "Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. İçinde **varlık Düzenleyicisi**, genişletin `App Icon & Top Shelf Image` varlık: 

    [![](icons-images-images/asset04.png "Üst raf görüntü varlığı genişletin")](icons-images-images/asset04.png#lightbox)
3. Ardından, genişletin `App Icon - Small` varlık: 

    [![](icons-images-images/asset05.png "App - küçük varlık simgesi")](icons-images-images/asset05.png#lightbox)
4. Ardından `Back` varlık ve tıklayarak `Contents` girişi: 

    [![](icons-images-images/asset06.png "Geri varlık genişletin")](icons-images-images/asset06.png#lightbox)
5. Tıklayın **Apple TV girişi x 1** ve bir görüntü dosyası seçin.
6. Yukarıdaki adımları yineleyin `Front` ve `Middle` varlıklar.
7. Tanımlamak için aynı adımları yineleyin `App Icon - Large` varlık.
4. Değişikliklerinizi kaydedin.

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>Üst raf görüntüsü

Kullanıcının Apple TV giriş ekranında üst satırda Xamarin.tvOS uygulamanızı yerleştirdiğini, uygulamanızı kullanıcı tarafından seçildiğinde büyük bir üst raf görüntü görüntülenir. Bu görüntü, uygulamanızı özelliklerini vurgulamak veya içeriği doğrudan bağlantılar sağlar.

[![](icons-images-images/topshelf01.png "Üst raf resim örneği")](icons-images-images/topshelf01.png#lightbox)

Üst raf görüntü ya da tek bir statik sağlanabilir `.png` veya `.lsr` dosyası (bkz [katmanlı görüntüleri oluşturma](#Creating-Layered-Images)) ya da dinamik olarak çalışma zamanında odaklanabilir öğeleri tek bir satır oluşturulamaz (bkz: [ Dinamik üst raf içerik](#Dynamic-Top-Shelf-Content) aşağıda).

|Üst raf görüntü boyutu|Notlar|
|---|---|
|1920x720px|Statik .png veya katmanlı .lsr dosyası|

Apple üst raf görüntülerinizi oluşturmak için aşağıdaki önerileri sağlar:

- **Zengin statik görüntülerin kullanmak** – uygulamanızı dinamik içerik sağlamazsa, kendi üst raf görüntü odaklanabilir olmayan olacaktır. Uygulama veya, marka özelliklerini vurgulamak için bu görüntüyü kullanın.
- **Uygulama içerik için bağlantı** – dinamik üst raf düzenleri, kullanıcı uygulamanızı en önemli bulur içeriği hızlı bir bağlantı sağlar. Uygulamanızı başlatmak ve hemen verilen içeriğin atlamak için hızlı bir bağlantı sağlamak için bu alanı kullanın.
- **En son içerik sergiler** – zengin üst raf içerik uygulamanıza bir kullanıcı çizme ve bunları daha kullanmak istediğiniz duruma getirin. Bu, en yüksek dereceli veya yeni içerik göstermek için bir alanı olarak kullanın.
- **İçerik kişiselleştirilmiş** – kullanıcılar yerine kullanıcıların en sık kullanılan veya giriş ekranın üst satırda sık kullanılan uygulamalar. Üst raf en ilgi olacaktır içeriği görüntülemek için kullanın.
- **Reklam izin** – Ads kesinlikle yasaktır üst raf görüntülenir. Son purchasable içerik gösterebilir, ancak hiçbir fiyatlandırma bilgileri görüntülenmesi gerekir.

### <a name="setting-the-top-shelf-image"></a>Üst raf görüntü ayarlama

Üst raf tvOS projeniz için gerekli görüntüsünü ayarlamak için lütfen aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Assets.xcassets` düzenlemek üzere açmak için: 

    [![](icons-images-images/asset01.png "Assets.xcassets dosyası")](icons-images-images/asset01.png#lightbox)
2. İçinde **varlık Düzenleyicisi**, genişletin `App Icon & Top Shelf Image` varlık: 

    [![](icons-images-images/asset04.png "Üst raf görüntü varlığı genişletin")](icons-images-images/asset04.png#lightbox)
3. Tıklayın `Top Shelf Image` varlık: 

    [![](icons-images-images/asset07.png "Üst raf görüntü varlığı")](icons-images-images/asset07.png#lightbox)
5. Tıklayın **Apple TV girişi x 1** ve bir görüntü dosyası seçin.
6. Değişikliklerinizi kaydedin.

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>Dinamik üst raf içeriği

Statik bir üst raf görüntü yerine üst raf dinamik bir satır içerebilir [odaklanabilir öğeleri](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) veya başlıkları kaydırma dinamik kümesi. Bu dinamik stili her ikisi de, uygulama veya atlama en fazla kullanılan özelliklerini içine tarafından sağlanan içeriği Vurgula olanak tanır.

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>Görüntülemektedir içerik satır

Bu dinamik üst raf içerik türü, kaydırma, odaklanabilir isteğe bağlı olarak bölümlere ayrılmış öğeleri tek bir satır sayısını gösterir. Yeni, sık kullanılan vurgulamak için genellikle kullanılan veya yakın zamanda uygulama içeriğini görüntülenebilir.

İçeriği tek, yatay kaydırma listesi (şu anda odaklanmış) içerik seçili içerik geçerli parça altında görünen bir etiketin olarak sunulur. Kullanıcı belirli bir içerik seçerse, uygulamanızı başlatılır ve bu içeriği doğrudan gerçekleştirilmelidir.

Aşağıdaki içerik boyutları gerekli olacaktır:

||Poster (2:3)|Kare (1:1)|HDTV (16:9)|
|---|---|---|---|
|Gerçek Boyut|404x608px|608x608px|908x512px|
|Güvenli bölge boyutu|380x570px|570x570px|852x479px|
|Odaksız boyutu|333x500px|500x500px|782x440px|
|Odaklanmış boyutu|380x570px|570x570px|852x479px|

Apple görüntülemektedir içerik satır için aşağıdaki önerileri sağlar:

- **Satır tamamlayın** – tam ekran genişliği span için yeterli içerik sağlamalıdır.
- **Karma görüntüleri ölçekleme** – görüntülemektedir içerik satırı görüntü boyutları (yukarıda verilen listeden) bir karışımını tutmak için tasarlanmıştır. Görüntü boyutları ancak karışımı varsa, ek ölçeklendirme içerik görüntüleme normalleştirmek için uygulanacağını unutmayın.

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>İç metin başlıkları kaydırma

İsteğe bağlı olarak, Xamarin.tvOS uygulamanızı içeriğini bir otomatik olarak kaydırma ve neredeyse ekranı kaplamasını başlıkları koleksiyonunu döngü olarak üst raf sunabilirsiniz. Bu stili genellikle yeni TV programları gibi zengin, yeni içerik göstermek için kullanılır.

Otomatik kaydırma yanı sıra kullanıcı başlıkları denetimini ele geçirmek ve Siri uzaktan kullanarak herhangi bir yönde kaydırın. Küçük bir yapmadan, döngüsel hareketi Siri bir başlık odakta olduğunda uzak Bu başlık Parallax etkisi etkinleşir.

**Başlık resmi (ek geniş)**

|   |   |
|---|---|
|Gerçek Boyut|1940x624px|
|Güvenli bölge boyutu|1740x620px|
|Odaksız boyutu|1740x560px|
|Odaklanmış boyutu|1740x620px|

İç metin başlıkları kaydırma ya da statik sağlanabilir `.png` veya katmanlı `.lsr` dosya.

Apple kaydırma dışarı başlıkları için aşağıdaki önerileri sağlar:

- **İçerik miktarı** -doğal eşitleyerek için kaydırma için en az üç (3) başlıkları sağlamalıdır. En fazla sekiz (8) başlıkları içermelidir veya kolaylaştırır Gezinti sabit için son kullanıcı.
- **İçerik metin** - metinde başlık görüntüde bulunması gerekir, başlık gerektirir. Katmanlı görüntüleri kullanıyorsanız, metninizi en üst katmanında olmalıdır.

Lütfen bkz. Apple'nın [TVServices Framework başvurusu](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) dinamik üst raf içerik sağlamak için uygulamanıza bir üst raf uzantısı ekleme hakkında daha fazla bilgi için.

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>Game Center görüntüleri

Oyun Xamarin.tvOS uygulamanızı ise ve oyun merkezi desteği dahil birkaç daha fazla görüntü varlıklarının gerekli olacaktır:

- **Başarı simgeler** -donuk görüntüyü otomatik olarak bir daireye kırpılır her başarı için gereklidir. Başarılar odaklanabilir olmayan öğelerdir.
- **Pano resmi** -, uygulamanızın Pano Game Center içinde üstünde görünür sağlanan isteğe bağlı bir görüntü olabilir. Bu görüntüleri odaklanabilir olmayan.
- **Skorbordu resmi** -birine (1) için üç (3) arasında 16:9 en boy oranını görüntüleri uygulamanızı destekleyen her Skorbordu için sağlamanız gerekir. Bunlar ya da statik olabilir `.png` veya katmanlı `.lsr` dosyaları. Skorbordu resmi odaklanabilir.

||Başarı simgeler|Pano resmi|Skorbordu resmi|
|---|---|---|---|
|Görünür boyutu|200x200px|923x150px|yok|
|Gerçek Boyut|320x320px|yok|659x371px|
|Güvenli bölge boyutu|yok|yok|618x348px|
|Odaksız boyutu|yok|yok|548x309px|
|Odaklanmış boyutu|yok|yok|618x348px|

Game Center ile çalışma hakkında daha fazla bilgi için lütfen Apple'nın bakın [oyun merkezi Programlama Kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html).

<a name="Working-with-Images" />

## <a name="working-with-images"></a>İmajlarla çalışma

TvOS 9 iOS 9 alt kümesidir, içerir ve bir Xamarin.iOS uygulaması görüntüleri göstermek için kullanılan aynı teknikleri de çalışmaya yönelik bir Xamarin.tvOS uygulamasını. Lütfen bakın bizim [görüntü görüntüleme](~/ios/app-fundamentals/images-icons/displaying-an-image.md) daha fazla bilgi için.

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>Xamarin.tvOS proje görüntüleri ayarlama

Yukarıda belirtildiği gibi tüm tvOS uygulamalar gerektiren bir [başlatma görüntü](#Launch-Image), ve [uygulama simgesini](#App-Icons). Bu bölüm, bir varlık kataloğunda ayarlandıktan sonra uygulama simgesine ve başlatma görüntü Xamarin.tvOS uygulama projeniz için seçme kapsar.

Aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Info.plist` düzenlemek üzere açmak için: 

    [![](icons-images-images/info01.png "Info.plist dosyası")](icons-images-images/info01.png#lightbox)
2. İçinde **Info.Plist Düzenleyicisi**, varlıklar Kataloğu'nu seçin (yukarıda yapılandırılmış [uygulama simgeleri ayarı](#Setting-the-App-Icons) bölüm) için **uygulama simgeleri**: 

    [![](icons-images-images/info02.png "Info.plist Düzenleyicisi")](icons-images-images/info02.png#lightbox)
3. Ardından, varlıklar Kataloğu'nu seçin (yukarıda yapılandırılmış [başlatma görüntü ayarlama](#Setting-the-Launch-Image) bölüm) için **başlatma görüntüleri**.
4. Değişikliklerinizi kaydedin.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede tüm resim türleri ve Xamarin.tvOS uygulamada kullanılan boyutları kapsamına. İlk olarak, başlatma görüntüleri, katmanlı görüntüleri, uygulama simgeleri, üst raf görüntüler ve oyun merkezi görüntüleri ele. Ardından, Xamarin.tvOS uygulamanızda görüntülerle çalışma ele.

## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
