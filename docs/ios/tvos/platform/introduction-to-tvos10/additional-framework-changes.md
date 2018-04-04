---
title: Ek tvOS 10 çerçeveleri değişiklikleri
description: Bu makalede, ek, küçük değişiklikler veya tvOS 10 varolan çerçeveyi geliştirmeler kapsar.
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c53726b91a0cd3e79041b9b1d51e9f7fbb1c79bd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="additional-tvos-10-frameworks-changes"></a>Ek tvOS 10 çerçeveleri değişiklikleri

_Bu makalede, ek, küçük değişiklikler veya tvOS 10 varolan çerçeveyi geliştirmeler kapsar._


TvOS önemli değişiklikler yanı sıra, Apple değişikliklerini ve birkaç varolan çerçeveleri geliştirmeleri tvOS 10 yaptı.

<a name="AV-Foundation-Framework" />

## <a name="av-foundation-framework-additions"></a>AV Foundation Framework eklemeler

AVFoundation framework aşağıdaki geliştirmeleri içerir:

 - TvOS 10'da, uygulama artık farklı uygulayan [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) davranışları içerik türüne bağlı. Basitçe `Rate` özelliği ve AVFoundation belirleyecektir yeterli içerik durduruyor olmadan kayıttan yürütme için kullanılabilir olduğunda.
 - Yeni `AVPlayerLooper` sınıfı kayıttan yürütme sırasında belirli bir medya döngü kolaylaştırır.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>AVKit Framework geliştirmeleri

AVKit framework aşağıdaki geliştirmeleri içerir:

 - Uygulamayı şimdi atlanıyor davranışı üzerinde denetime sahip [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) şekilde atlanıyor hareketi çalma listesi ya da geçerli öğe içinde öncelikli sonraki öğeye gitme.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>Çekirdek veri geliştirmeleri

tvOS 10 çekirdek veri Framework aşağıdaki geliştirmeleri içerir:

 - Kök [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) eşzamanlı hatalı ve seri hale getirme getirme nesnelerini destekler.
 - [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) sınıfı SQLite veri depolarına havuzu korur.
 - [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) NİLEME günlük modu desteği yeni sorgu oluşturma SQLite veri depolarında nesneleriyle özellik burada Yönetilen Nesne bağlamları (MOC) gelecekteki getirme için belirli veritabanı sürümleriyle sabitlenmelidir ve hataya neden olan işlemleri.
 - Üst düzey kullanarak `NSPersistenceContainer` başvuru `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) ve diğer temel verilere yapılandırma kaynakları.
 - Birkaç yeni kullanışlı yöntemler için eklenene `NSManagedObject` öğesinden gerçekleştirmek ve alt sınıflar oluşturmak kolaylaştırır.

Daha fazla bilgi için lütfen Apple'nın bkz [çekirdek veri Framework başvurusu](https://developer.apple.com/reference/coredata).

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>Çekirdek grafik geliştirmeleri

tvOS 10 çekirdek grafik Framework aşağıdaki geliştirmeleri içerir:

 - Yeni [CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) sınıfı, bir dizi renk dönüştürme gerçekleştirmek için kullanılabilir.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Çekirdek görüntü geliştirmeleri

tvOS 10 çekirdek görüntü Framework aşağıdaki geliştirmeleri sağlar:

 - `ImageWithExtent` Yöntemi [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) sınıfı, filtre işleminin özel işleme eklemek için kullanılabilir. Çekirdek görüntü çıktı için görüntü işleme sırasında filtreler arasında verilen geri çağırma veya görüntüleme.
 - Uygulamayı şimdi önce ve sonra işlem renk alanını ve bu moddan dönüştürerek çekirdek görüntü bağlamın çalışma renk alanını dışında bir renk alanını görüntülerinde işleyebilir.
 - Çeşitli işleme performans iyileştirmelerinden yapılmıştır `UIImage` (çekirdek görüntü görüntü mağazaları tarafından yedeklenen olduğunda) işlemeye `UIImageView` nesneleri. 
 - `UIImage` nesneleri etiketli wide gam olarak wide gam renkte sokacak `UIImageView` geniş renk destekleyen iOS cihazlarda nesneleri.
 - Çekirdek görüntü çekirdek kodu artık belirli piksel Çıkış biçimleri isteyebilir.

Ayrıca, aşağıdaki yeni çekirdek resim filtreleri eklenmiştir:

 - `CINinePartTiled`
 - `CINinePartStretched`
 - `CIHueSaturationValueGradient`
 - `CIEdgePreserveUpsampleFilter`
 - `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Foundation geliştirmeleri

TvOS 10 Foundation altyapısına aşağıdaki geliştirmeler yapılmıştır:

 - Yeni [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) aralıkları karşılaştırma ve aralığı kesişimlerini için test etme için süreleri gibi tarih ve saat aralığı hesaplamaları yapmak için sınıf.
 - Çeşitli yeni özellikler eklendi [NSLocal](https://developer.apple.com/reference/foundation/nslocale) yerel bilgileri ve kullanılabilir görüntü biçimlerini edinmeye sınıfı.
 - Yeni [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) farklı birimleri, ölçü (ölçü birimi arasında) Dönüştür veya farklı UOMs değerleri hesaplamalar için sınıf.
 - Yeni [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) son kullanıcıya görüntüleme için yerelleştirilmiş ölçümleri biçimlendirmek için sınıf.
 - Yeni [NSUnit](https://developer.apple.com/reference/foundation/nsunit) ve [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) belirli UOMs temsil eden sınıf.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>GameKit geliştirmeleri

TvOS 10 GameKit çerçevesinde aşağıdaki geliştirmeler yapılmıştır:

 - Yeni bir salt iCloud hesap türü tarafından uygulanan [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) sınıfı.
 - Yeni [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) sınıfı Game Center kalıcı veri depolama yönetmek için genelleştirilmiş bir çözüm sağlar. `GKGameSession` oynatıcıları listesini tutar ve uygulama sorumlu form nasıl ve ne zaman katılımcı tarih depolanır, alınan veya okuyucular arasında alınıp uygulama. Çoğu durumda, mevcut Aç dayalı eşleşmeler, gerçek zamanlı eşleşiyor veya kalıcı oyun yöntemleri Kaydet oyun oturumları değiştirebilirsiniz.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>GameplayKit geliştirmeleri

TvOS 10 GameplayKit çerçevesinde aşağıdaki geliştirmeler yapılmıştır:

 - Yordam gürültü nesil eklendi ve doğal görünümlü dokuları içinde daha iyi geliştirmek, daha iyi kamera hareketleri ekleyin ve zengin oyun dünyaları oluşturmak yardımcı olmak için kullanılabilir.
 - Uzamsal bölümleme etkili bir arama için oyun world veri bölümlemek için kullanın.
 - Yeni Monte Carlo Strateji Uzmanı ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) için kapsamlı olası taşıma hesaplama eklendi.
 - Yeni bir karar ağacı API eklendi ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) ve [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) oyun yapı AI geliştirmek için.
 - Var olan aracıyı ve yeni yol bulma davranışları 3B desteği eklendi [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) ve [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) sınıfları.
 - Yeni [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) yüksek performanslı, doğal görünümlü yollar sağlamak için sınıf.
 - Yeni [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) ve [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) GameplayKit ve SpriteKit her zamankinden daha kolay birleştirme sınıfları olun.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>Metal geliştirmeleri

TvOS 10 Metal Framework'te aşağıdaki geliştirmeler yapılmıştır:

 - 3B uygulamaları ve oyunları şimdi kullanabileceğiniz _Mozaik döşeme_ karmaşık planda ve GPU aracılığıyla geometri verimli bir şekilde işlemek için.
 - İşlev uzmanlık malzeme ve açık birleşimi işlevleri bir Sahne için yüksek oranda iyileştirilmiş koleksiyonu oluşturmak için kullanın.
 - Hassas bir denetim Metal performansını iyileştirmek için kaynak ayırma kaynak yığınlardaki kullanarak uygulamaları ve Memoryless işleme hedefleri sağlayın.

Daha fazla bilgi için Apple'nın bkz [Metal Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>Metal performans gölgelendiriciler geliştirmeleri

TvOS 10 Metal performans gölgelendiriciler Framework'te aşağıdaki geliştirmeler yapılmıştır:

 - Birçok yeni tekrar renk alanı dönüşümler ve sinir ağı işlemleri gibi yüksek iyileştirilmiş, verileri paralel hesaplamalar yararlanmak uygulama izin vermek için Metal performans gölgelendiriciler Framework eklenmiştir.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>ModelIO geliştirmeleri

TvOS 10 ModelIO çerçevesinde aşağıdaki geliştirmeler yapılmıştır:

 - ABD Doları dosya biçimi artık desteklenmektedir.
 - Yeni `MDLMaterialPropertyGraph` kolayca çalışma zamanı desteklemek için sınıf modellerle değiştirir.
 - Uzaklık için destek eklenmiştir alan imzalı [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) sınıfı.
 - Yeni `MDLLightProbeIrradianceDataSource` ışık araştırma yerleştirmeyi yardımcı sınıfı.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>SceneKit geliştirmeleri

TvOS 10 SceneKit çerçevesinde aşağıdaki geliştirmeler yapılmıştır:

 - SceneKit artık daha basit varlık yazma daha gerçekçi sonuçlar yeni bir fiziksel olarak bağlı işleme (PBR) sistemi içerir.
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

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>SpriteKit geliştirmeleri

TvOS 10 SpriteKit çerçevesinde aşağıdaki geliştirmeler yapılmıştır:

 - Tilemaps 2B, 2.5 D ve yan kaydırma oyunlar kullanarak için kare, Altıgen ve İzometrik bölmesi şekilleri şimdi desteği `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` ve `SKTileSet` sınıfları.
 - Yeni `SKWarpGeometry` uzatma veya deforme sınıfı [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) veya [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) işleme. Yeni [SKAction](https://developer.apple.com/reference/spritekit/skaction) sınıfı, tüneli etkileri arasında geçişler animasyon uygulamak için kullanılabilir.
 - Özel gölgelendiriciler öznitelikleri sağlayabilir (`SKAttribute`) yapılandırılabilir ayrı ayrı bir öznitelik değeri sağlayarak gölgelendirici kullanan her bir düğüm tarafından (`SKAttributeValue`).
 - [SKView](https://developer.apple.com/reference/spritekit/skview) sınıfı nasıl ve ne zaman bir Sahne işlenmeden üzerinde ayrıntılı denetim vermek için birkaç yeni yöntemler sağlar.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>Uıkit geliştirmeleri

TvOS 10 Uıkit çerçevesinde aşağıdaki geliştirmeler yapılmıştır:

 - API odak görünümü olmayan öğesi odağını için ayrıca desteklemek için Gelişmiş `UIViews`. Odak desteği öğeleri _gerekir_ uygulamak `IUIFocusItem` arabirimi.
 - Yeni `UIGraphicsRender` sınıfı bit eşlemler veya PDF Uıkit işleme veya çekirdek grafikleri oluşturma, nesne yönelimli bir yöntem sağlar ve kullanım dışı değiştirir `UIGraphicsBeginImageContext` yöntemi.
 - `UIUserInterfaceStyle` Sınıfı hangi kullanıcı arayüzü temasını (koyu veya açık) şu anda etkin olduğunu belirlemek için eklenmiştir.
 - Yeni tamamen etkileşimli, nesne tabanlı, kesilebilir animasyon desteği eklendi ve van hareketleri için bağlı. Pleas bkz: Apple's [UIViewAnimating Protokolü başvurusu](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator sınıf başvurusu](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protokolü başvurusu](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters sınıf başvurusu](https://developer.apple.com/reference/uikit/uicubictimingparameters) ve [UISpringTimingParameter sınıf başvurusu](https://developer.apple.com/reference/uikit/uispringtimingparameters) daha fazla bilgi için.
 - Yeni `UIPreviewInteraction` ve `UIPreviewInteractionDelegate` gözlem ve pop işlemleri için özel bir arabirim sağlamak uygulama izin verin.
 - Yeni `UIAccessibilityCustomRotor` sınıfı gibi ses üzerinden yardımcı teknolojiler, bağlam özgü özel işlevsellik sağlamak uygulamanın olanak tanır.
 - Kullanım `UIAccessibilityIsAssistiveTouchRunning` ve `UIAccessibilityAssistiveTouchStatusDidChangeNotification` AssistiveTouch etkin olup olmadığını belirlemek için simgeler.
 - Kullanım `UIAccessibilityHearingDevicePairedEar` ve `UIAccessibilityHearingDevicePairedEarDidChangeNotification` herhangi durumunu almak için simgeler Eşli MFi işitme yardımları.
 - Yeni [UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) API uyumlu içerik türleri ortak sınıf türleri için otomatik olarak bildirir ve yeni seçenekler (örneğin, yaşam süresi sınırlamaları) sağlar.
 - Dinamik tür etiketler desteklemek için metin alanları ve metin kutuları yeni kullanma `PreferredFontForTextStyle` yöntemi `UIFont` sınıfı.
 - Bir öğe, yazı tipi güncelleştirmesi karar vermek için cihazları `UIContentSizeCategory` değişiklikleri, kullanmak `AdjustsFontForContentSizeCategory` özelliği `UIContentSizeCategoryAdjusting` temsilci.
 - Uygulamayı şimdi Sekme çubuğu öğeleri metin ve arka plan rengi gibi rozet görünümünü denetleyebilirsiniz.
 - Şimdi Yenile denetiminde alt sınıfların tüm kaydırma ve kaydırma görünümlerinde desteklenen (gibi `UICollectionView`).
 - `OpenURL` Yöntemi `UIApplication` sınıfı şimdi destekler Aç tamamlandıktan sonra çağrılan bir tamamlanma işleyici zaman uyumsuz olarak çağrılır.
 - CloudKit paylaşımı başlatmak ve özelliklerini kullanarak yeni değiştirerek `UICloudSharingController` ve `UICloudSharingControllerDelegate` sınıfları.
 - Kaydırma deneyimini geliştirmek için prefetched hücreleri yararlanmak `UICollectionViews` yeni `UICollectionViewDataSourcePrefetching` temsilci.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
