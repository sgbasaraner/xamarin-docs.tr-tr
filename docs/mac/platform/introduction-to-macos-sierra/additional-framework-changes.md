---
title: Ek macOS Sierra Framework değişiklikleri
description: Bu belgede, küçük değişiklikler ve macOS Sierra sunulan varolan çerçeveleri geliştirmeler açıklanmaktadır. Accelerate framework, AppKit, AVFoundation, çekirdek veri, çekirdek görüntü, Foundation ve daha fazla değişiklik inceler.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3cfa2e9bcb0be4d65462914215045c9c7f01da5b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792599"
---
# <a name="additional-macos-sierra-framework-changes"></a>Ek macOS Sierra Framework değişiklikleri

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Framework geliştirmeleri hızlandırmak

Aşağıdaki geliştirme, macOS Sierra hızlandırmak altyapısına yapılan:

- Eklenen Kadrat (integral hesaplama).
- Sinir ağları oluşturmak için temel işlevleri eklendi.
- İki geometrik nesneleri kesişimi gibi işlemler için test etmek için eklenen geometrik koşul çalışır.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>AppKit Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra AppKit altyapısına yapılan:

- Bazı geliştirmeler `NSCollectionView` gibi:
    - **Daraltılabilir bölümleri** -tek yatay bir satırda bir koleksiyon görünümü bölümü daraltın olanak tanır.
    - **Üstbilgiler kayan** -üstbilgiler ve altbilgiler şimdi kaydırılmış (bir akış düzeni) olarak aynı API kullanarak [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) iOS içinde.
    - **Kaydırılabilir arka plan görünümleri** -bir koleksiyonu görünümleri arka planı artık içeriği ile birlikte kaydırmak için ayarlanabilir.
- Ertelenmiş görünüm düzeni geçişi en iyi duruma getirilmiş ve genişletilmiş.
- Sürükle ve bırak API artık yeni içerir `NSFilePromiseProvider` ve `NSFilePromiseReceiver` desteklemek için sınıflar sürükleyin flocking.
- Birkaç kolaylık oluşturucular mevcut denetimleri eklenmiştir:
    -  `NSButton` düğmeler, oluşturmak için yeni oluşturucular içeren onay kutularını ve radyo düğmeleri.
    -  `NSTextField` kaydırma ve kaydırma olmayan etiketleri, öznitelikli etiketleri ve düzenlenebilir metin alanları oluşturmak için yeni oluşturucular içerir.
    -  `NSSegmentedControl` Etiket ve görüntüleri bir gruptan bölümlenmiş denetimleri oluşturmak için yeni oluşturucular içerir.
    -  `NSSlider` Yatay doğrusal kaydırıcılar oluşturmak için yeni oluşturucular içerir.
    -  `NSImageView` düzenlenemeyen görüntü görünümleri oluşturmak için yeni oluşturucular içeren bir verilen `NSImage`.
-  Yeni `NSGridView` koleksiyona değişkeni içeren bir kılavuz alt görünümleri boyutta satırları ve sütunları dinamik olarak gizli veya gösterilen otomatik düzene eklendi.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>AVFoundation Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra AVFoundation altyapısına yapılan:

- MacOS içinde uygulama artık farklı uygulamak sahip [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) davranışları içerik türüne bağlı. Basitçe `Rate` özelliği ve AVFoundation belirleyecektir yeterli içerik durduruyor olmadan kayıttan yürütme için kullanılabilir olduğunda.
- Yeni `AVPlayerLooper` sınıfı kayıttan yürütme sırasında belirli bir medya döngü kolaylaştırır.
- `AVAssetDownloadURLSession` Sınıfı verir indirme için ve FairPlay sonraki çalınmasını şifrelenmiş HLS akış.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Çekirdek veri Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra için çekirdek veri çerçevesi için yapılan:

- Kök [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) eşzamanlı hatalı ve seri hale getirme getirme nesnelerini destekler.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) sınıfı SQLite veri depolarına havuzu korur.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) NİLEME günlük modu desteği yeni sorgu oluşturma SQLite veri depolarında nesneleriyle özellik burada Yönetilen Nesne bağlamları (MOC) gelecekteki getirme için belirli veritabanı sürümleriyle sabitlenmelidir ve hataya neden olan işlemleri.
- Üst düzey kullanarak `NSPersistenceContainer` başvuru `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) ve diğer temel verilere yapılandırma kaynakları.
- Birkaç yeni kullanışlı yöntemler için eklenene `NSManagedObject` öğesinden gerçekleştirmek ve alt sınıflar oluşturmak kolaylaştırır.

Daha fazla bilgi için lütfen Apple'nın bkz [çekirdek veri Framework başvurusu](https://developer.apple.com/reference/coredata).

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Çekirdek görüntü Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra için çekirdek görüntü çerçevesi için yapılan:

- `ImageWithExtent` Yöntemi [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) sınıfı, filtre işleminin özel işleme eklemek için kullanılabilir. Çekirdek görüntü çıktı için görüntü işleme sırasında filtreler arasında verilen geri çağırma veya görüntüleme.
- Uygulamayı şimdi önce ve sonra işlem renk alanını ve bu moddan dönüştürerek çekirdek görüntü bağlamın çalışma renk alanını dışında bir renk alanını görüntülerinde işleyebilir.
- Çekirdek görüntü çekirdek şimdi belirli piksel çıktı biçimi isteyebilir.
- Aşağıdaki yeni resim filtreleri eklenmiştir: `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` ve `CIClamp`.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Foundation Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra Foundation altyapısına yapılan:

- Kullanım [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) temsil eden, dönüştürme ve birçok yaygın fiziksel birimler gibi görüntülemek için API yığın, uzunluk, hız, süre ve sıcaklık.
- Kullanım [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) ayrıştırma ve ISO 8601 oluşturmak için sınıfı tarihleri biçimlendirilmiş.
- Yeni [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) aralıkları karşılaştırma ve aralığı kesişimlerini için test etme için süreleri gibi tarih ve saat aralığı hesaplamaları yapmak için sınıf.
- Kullanım [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) bir kişinin adını bir dizeden öğeleri ayrıştırmak için sınıf.
- Yeni [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) ölçümleri oturum ağ için bir URL almak için sınıf.

Daha fazla bilgi için lütfen Apple'nın bkz [Foundation sürüm notları OS X v10.12 ve iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>GameKit Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra GameKit altyapısına yapılan:

- **Game Center uygulamasının** kullanım ve macOS kaldırıldı. Uygulama GameKit, kullanıyorsa, _gerekir_ liderlik, vb. gibi GameKit özellikleri görüntülemek için kendi arabirimi sunar. 
- Yeni bir salt iCloud hesap türü tarafından uygulanan [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) sınıfı.
- Yeni [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) sınıfı Game Center kalıcı veri depolama yönetmek için genelleştirilmiş bir çözüm sağlar. `GKGameSession` oynatıcıları listesini tutar ve uygulama sorumlu form nasıl ve ne zaman katılımcı tarih depolanır, alınan veya okuyucular arasında alınıp uygulama. Çoğu durumda, mevcut Aç dayalı eşleşmeler, gerçek zamanlı eşleşiyor veya kalıcı oyun yöntemleri Kaydet oyun oturumları değiştirebilirsiniz.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>GamePlayKit Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra GamePlayKit altyapısına yapılan:

- Yordam gürültü nesil eklendi ve doğal görünümlü dokuları içinde daha iyi geliştirmek, daha iyi kamera hareketleri ekleyin ve zengin oyun dünyaları oluşturmak yardımcı olmak için kullanılabilir.
- Uzamsal bölümleme etkili bir arama için oyun world veri bölümlemek için kullanın.
- Yeni Monte Carlo Strateji Uzmanı ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) için kapsamlı olası taşıma hesaplama eklendi.
- Yeni bir karar ağacı API eklendi ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) ve [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) oyun yapı AI geliştirmek için.
- Var olan aracıyı ve yeni yol bulma davranışları 3B desteği eklendi [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) ve [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) sınıfları.
- Yeni [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) yüksek performanslı, doğal görünümlü yollar sağlamak için sınıf.
- Yeni [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) ve [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) GameplayKit ve SpriteKit her zamankinden daha kolay birleştirme sınıfları olun.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>Metal Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra Metal altyapısına yapılan:

- 3B uygulamaları ve oyunları şimdi kullanabileceğiniz _Mozaik döşeme_ karmaşık planda ve GPU aracılığıyla geometri verimli bir şekilde işlemek için.
- İşlev uzmanlık malzeme ve açık birleşimi işlevleri bir Sahne için yüksek oranda iyileştirilmiş koleksiyonu oluşturmak için kullanın.
- Hassas bir denetim Metal performansını iyileştirmek için kaynak ayırma kaynak yığınlardaki kullanarak uygulamaları ve Memoryless işleme hedefleri sağlayın.

Daha fazla bilgi için Apple'nın bkz [Metal Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>Model g/ç Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra modeli g/ç altyapısına yapılan:

- ABD Doları dosya biçimi artık desteklenmektedir.
- Yeni `MDLMaterialPropertyGraph` kolayca çalışma zamanı desteklemek için sınıf modellerle değiştirir.
- Uzaklık için destek eklenmiştir alan imzalı [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) sınıfı.
- Yeni `MDLLightProbeIrradianceDataSource` ışık araştırma yerleştirmeyi yardımcı sınıfı.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>Fotoğraf Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra fotoğraf altyapısına yapılan:

- Canlı fotoğraf düzenleme şimdi fotoğraf çerçevesini destekleyen uygulamalar ve fotoğraf uzantıları düzenleme için kullanılabilir (fotoğraf ve kamera içinde kullanmak için uygulamalar).
- Yeni [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) sınıfı düzenlemeleri hem video hem de Live Fotoğraf içeriği için hala geçerli.
- Kullanım [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) ve [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) düzenlemeleri gerçekleştirmek için yeni çekirdek Görüntü İşlemci özelliğinin avantajlarından yararlanmak için sınıflar.
- Live Fotoğraf desteklemek için [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) ve [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) sınıfları taşınmıştır iOS macOS için.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>SceneKit Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra SceneKit altyapısına yapılan:

- Şimdi yeni bir fiziksel olarak bağlı işleme (PBR) sistemi daha basit varlık yazma daha gerçekçi sonuçlar içerir.
- Yeni [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) gölgelendirme model ürün için çok çeşitli gerçekçi gölgelendirme etkileri yalnızca üç temel özellikleri gerektiren sırasında (`Diffuse`, `Metalness` ve `Roughness`).
- Ortam tabanlı aydınlatma ile en iyi şekilde çalışır gölgelendirme PBR itibaren kullanın `LightingEnvironment` görüntü tabanlı aydınlatma tüm Sahne tan atamak için özellik.
- Kullanım `IESProfileURL` gerçek değerlere yoğunluğu (içinde Lümen) gibi temel aydınlatma tanımlayan ve renk sıcaklık (derece cinsinden Kelvin) gerçek hafif armatürleri almak için özellik.
- [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) sınıfı HDR özellikleri ve efektler kullanarak büyük daha iyi sağlayabilir. Uyarlamalı Etkilenme otomatik efektler veya kullanım Vinyet oluşturma, renk koyu çizgileri ve oyuna filmatic efektleri eklemek için renk sınıflandırma oluşturmak için kullanın.
- Geleneksel işleme tekniklerini daha iyi sonuçlar PBR ve HDR Kamera özellikleri sağlar ve sonuç olarak, SceneKit şimdi renk hesaplamalarının (wide renk aygıt ekranlarda P3 renk skalasını kullanarak) doğrusal renk alanını gerçekleştirir.
- SceneKit şimdi rengi renk profil bilgileri okuyarak tüm renkleri eşleştirir.
- SceneKit renk bileşeni değerleri tüm gölgelendirici türleri için doğrusal bir RGB renk alanında yorumlar.
- SceneKit okur ve renk profil bilgilerini doku görüntülerinde ayarlamak için bu bilgiler sağlanmıştır emin olmak için tüm görüntüleri için varlık katalog kullanın.
- Doğrusal hem de alan işleme renk ve geniş renk belirterek devre dışı bırakılacak `SCNDisableLinearSpaceRendering` ve `SCNDisableWideGamut` uygulamanın anahtarlarında `Info.plist`.
- Rastgele Çokgen primates (veya dosyalarından yüklenen program aracılığıyla oluşturulan) yapı geometri yeni belirtmek için [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) sınıfı.

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>Güvenlik Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra güvenlik altyapısına yapılan:

- `SecKey` Arabirimi modernized ve tüm platformlarda (iOS, tvOS, watchOS ve macOS) birleşik.

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>SpriteKit Framework geliştirmeleri

Aşağıdaki geliştirme, macOS Sierra SpriteKit altyapısına yapılan:

- Tilemaps 2B, 2.5 D ve yan kaydırma oyunlar kullanarak için kare, Altıgen ve İzometrik bölmesi şekilleri şimdi desteği `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` ve `SKTileSet` sınıfları.
- Yeni `SKWarpGeometry` uzatma veya deforme sınıfı [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) veya [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) işleme. Yeni [SKAction](https://developer.apple.com/reference/spritekit/skaction) sınıfı, tüneli etkileri arasında geçişler animasyon uygulamak için kullanılabilir.
- Özel gölgelendiriciler öznitelikleri sağlayabilir (`SKAttribute`) yapılandırılabilir ayrı ayrı bir öznitelik değeri sağlayarak gölgelendirici kullanan her bir düğüm tarafından (`SKAttributeValue`).
- [SKView](https://developer.apple.com/reference/spritekit/skview) sınıfı nasıl ve ne zaman bir Sahne işlenmeden üzerinde ayrıntılı denetim vermek için birkaç yeni yöntemler sağlar.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>Yeni çerçeveler

Aşağıdaki çerçevelerini macOS Sierra eklenmiştir:

- **Hedefleri Framework** -bu çerçeve (örneğin, konum ya da kullanıcı eylemlerini) etkileşimleri inceleyin ve bu bilgiye bağlı harekete yazmasına izin.
- **SafariServices Framework** -bu framework macOS ve iOS için Safari'nin (örneğin, içerik blockers) için uygulama uzantıları geliştirmek uygulama izin verin.

## <a name="related-links"></a>İlgili bağlantılar

- [Mac Örnekleri](https://developer.xamarin.com/samples/mac/)
- [OS X 10,12 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
