---
title: "Ek iOS 10 çerçeveleri değişiklikleri"
description: "Bu makalede, ek, küçük değişiklikler veya iOS 10 varolan çerçeveyi geliştirmeler kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: fef543291f0743f8ebfa799b67fca2c8243be9bc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="additional-ios-10-frameworks-changes"></a>Ek iOS 10 çerçeveleri değişiklikleri

_Bu makalede, ek, küçük değişiklikler veya iOS 10 varolan çerçeveyi geliştirmeler kapsar._

## <a name="av-foundation-framework-additions"></a>AV Foundation Framework eklemeler

AVFoundation framework aşağıdaki geliştirmeleri içerir:

- İOS 10'da, geliştirici artık farklı uygulamak sahip [AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/) davranışları içerik türüne bağlı. Basitçe `Rate` özelliği ve AVFoundation belirleyecektir yeterli içerik durduruyor olmadan kayıttan yürütme için kullanılabilir olduğunda.
- Yeni [AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/) sınıfı değiştirir kullanım dışı `AVCaptureStillImageOutput` sınıfı ve tüm fotoğraf iş akışları Gelişmiş denetim sağlama ve izleme yakalama işlemini işlemek için birleştirilmiş bir yöntem sağlar ve Live Fotoğraf ve RAW yakalama biçimi gibi yeni özellikler için destek.
- Yeni `AVPlayerLooper` sınıfı kayıttan yürütme sırasında belirli bir medya döngü kolaylaştırır.
- `AVAssetDownloadURLSession` Sınıfı verir indirme için ve FairPlay sonraki çalınmasını şifrelenmiş HLS akış.
- Varsayılan olarak, [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/) sınıfı aygıt donanım desteklediğinde wide rengi, geniş gam yakalama otomatik olarak destekler. Apple'nın bkz [iOS cihaz uyumluluk başvurusu](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) daha fazla ayrıntı için.

## <a name="avkit-additions"></a>AVKit eklemeler

AVKit framework artık yeni içerir `UpdatesNowPlayingInfoCenter` Şimdi Yürütülüyor Bilgi Merkezi ne zaman güncelleştirileceğini belirtmek için özelliği.

## <a name="core-data-enhancements"></a>Çekirdek veri geliştirmeleri

iOS 10 çekirdek veri Framework aşağıdaki geliştirmeleri içerir:

- [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) NİLEME günlük modu desteği yeni sorgu oluşturma SQLite veri depolarında nesneleriyle özellik burada Yönetilen Nesne bağlamları (MOC) gelecekteki getirme için belirli veritabanı sürümleriyle sabitlenmelidir ve hataya neden olan işlemleri.
- Kök [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) eşzamanlı hatalı ve seri hale getirme getirme nesnelerini destekler.
- [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) sınıfı SQLite veri depolarına havuzu korur.
- Birkaç yeni kullanışlı yöntemler için eklenene `NSManagedObject` öğesinden gerçekleştirmek ve alt sınıflar oluşturmak kolaylaştırır.
- Üst düzey kullanarak `NSPersistenceContainer` başvuru `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/) ve diğer temel verilere yapılandırma kaynakları.

Daha fazla bilgi için lütfen Apple'nın bkz [çekirdek veri Framework başvurusu](https://developer.apple.com/reference/coredata).

## <a name="core-image-enhancements"></a>Çekirdek görüntü geliştirmeleri

iOS 10 çekirdek görüntü Framework aşağıdaki geliştirmeleri sağlar:

- Geliştirici, önce ve sonra işlem renk alanını ve bu moddan dönüştürerek çekirdek görüntü bağlamın çalışma renk alanını dışında bir renk alanını görüntülerinde şimdi işleyebilir.
- A8 veya A9 CPU kullanan iOS cihazlar için ham görüntü biçimi artık desteklenmektedir. Çekirdek görüntü şimdi 3 taraf kameradan veya yerleşik iSight Kamera ham görüntüleri çözülmesi için destek sağlar. Kullanım `FilterWithImageData` veya `FilterWithImageURL` yöntemlerinin [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) ham görüntüleri işlemek için sınıf.
- Çeşitli işleme performans iyileştirmelerinden yapılmıştır `UIImage` (çekirdek görüntü görüntü mağazaları tarafından yedeklenen olduğunda) işlemeye `UIImageView` nesneleri. 
- `UIImage` nesneleri etiketli wide gam olarak wide gam renkte sokacak `UIImageView` geniş renk destekleyen iOS cihazlarda nesneleri.
- Çekirdek görüntü çekirdek kodu artık belirli piksel Çıkış biçimleri isteyebilir.
- `ImageWithExtent` Yöntemi [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) sınıfı, filtre işleminin özel işleme eklemek için kullanılabilir. Çekirdek görüntü çıktı için görüntü işleme sırasında filtreler arasında verilen geri çağırma veya görüntüleme.

Ayrıca, aşağıdaki yeni çekirdek resim filtreleri eklenmiştir:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>Çekirdek hareket ekleme

Yeni iOS 10, çekirdek hareket framework hızlı, duraklatma ve sürdürme izleme çalışırken kullanıcının gerçek zamanlı bildirimleri almak bir uygulamanın pedometer olaylarını içerir. Kullanım [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/) ön plan veya arka plan pedometer olaylarını kaydetmek için.

## <a name="foundation-enhancements"></a>Foundation geliştirmeleri

İOS 10 Foundation altyapısına aşağıdaki geliştirmeler yapılmıştır:

- Yeni [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) son kullanıcıya görüntüleme için yerelleştirilmiş ölçümleri biçimlendirmek için sınıf.
- Yeni [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) aralıkları karşılaştırma ve aralığı kesişimlerini için test etme için süreleri gibi tarih ve saat aralığı hesaplamaları yapmak için sınıf.
- Yeni [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) farklı birimleri, ölçü (ölçü birimi arasında) Dönüştür veya farklı UOMs değerleri hesaplamalar için sınıf.

- Yeni [NSUnit](https://developer.apple.com/reference/foundation/nsunit) ve [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) belirli UOMs temsil eden sınıf.
- Çeşitli yeni özellikler eklendi [NSLocal](https://developer.apple.com/reference/foundation/nslocale) yerel bilgileri ve kullanılabilir görüntü biçimlerini edinmeye sınıfı.

## <a name="gamekit-enhancements"></a>GameKit geliştirmeleri

İOS 10 GameKit Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- **Game Center uygulamasının** kullanım ve iOS kaldırıldı. Uygulama GameKit, kullanıyorsa, _gerekir_ liderlik, vb. gibi GameKit özellikleri görüntülemek için kendi arabirimi sunar. 
- Yeni bir salt iCloud hesap türü tarafından uygulanan [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) sınıfı.
- Yeni [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) sınıfı Game Center kalıcı veri depolama yönetmek için genelleştirilmiş bir çözüm sağlar. `GKGameSession` oynatıcıları listesini tutar ve uygulama uygulamak için sorumlu olduğu nasıl ve ne zaman katılımcı tarih depolanır, alınan veya okuyucular arasında akar. Çoğu durumda, mevcut Aç dayalı eşleşmeler, gerçek zamanlı eşleşiyor veya kalıcı oyun yöntemleri Kaydet oyun oturumları değiştirebilirsiniz.

## <a name="gameplaykit-enhancements"></a>GameplayKit geliştirmeleri

İOS 10 GameplayKit Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- Yeni [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) yüksek performanslı, doğal görünümlü yollar sağlamak için sınıf.
- Yordam gürültü nesil eklendi ve doğal görünümlü dokuları içinde daha iyi geliştirmek, daha iyi kamera hareketleri ekleyin ve zengin oyun dünyaları oluşturmak yardımcı olmak için kullanılabilir.
- Uzamsal bölümleme etkili bir arama için oyun world veri bölümlemek için kullanın.
- Yeni Monte Carlo Strateji Uzmanı ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) için kapsamlı olası taşıma hesaplama eklendi.
- Var olan aracıyı ve yeni yol bulma davranışları 3B desteği eklendi [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) ve [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) sınıfları.
- Yeni [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) ve [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) GameplayKit ve SpriteKit her zamankinden daha kolay birleştirme sınıfları olun.
- Yeni bir karar ağacı API eklendi ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) ve [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) oyun yapı AI geliştirmek için.

## <a name="healthkit-enhancements"></a>HealthKit geliştirmeleri

İOS 10 HealthKit Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- Yeni meta veri anahtarları hava durumu türleri için eklenmiştir (gibi `HKWeatherConditionClear` ve `HKWeatherConditionCloudy`) ve etkinlik türleri (gibi `HKWorkoutActivityTypeFlexibility` ve `HKWorkoutActivityTypeWheelchairRunPace`) eklenmiştir.
- Yeni `HKCDADocument` Klinik belge mimarisi (CDA) göstermek için biçimlendirilmiş belge sınıfı eklendi.
- Yeni [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) belirlemek için sınıf `ActivityType` ve `LocationType` bir etkinlik olarak.
- Yeni [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) ve `WheelchairUse` yöntemi [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) sınıfı eklendi Tekerlekli ile çalışmak için ilişkili sistem durumu verileri.

## <a name="homekit-enhancements"></a>HomeKit geliştirmeleri

İOS 10 HomeKit Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- Yeni Hizmetleri ve özellikleri eklenmiştir.
- İPad kullanıcı izinlerini uzaktan aksesuar erişim sağlamak, Otomasyon Tetikleyicileri çalıştırın ve etkinleştirmek için bir HomeKit Hub paylaşılan olarak çalışacak şekilde yapılandırılabilir.
- Kamera ve kapı zili Donatılar için destek eklenmiştir.
- Daha fazla içerik ve yapılandırmaları için Donatılar belirtildi.

Lütfen bakın bizim [HomeKit giriş](~/ios/platform/homekit.md) daha fazla bilgi için.

## <a name="metal-enhancements"></a>Metal geliştirmeleri

İOS 10 Metal Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- 3B uygulamaları ve oyunları şimdi kullanabileceğiniz _Mozaik döşeme_ karmaşık planda ve GPU aracılığıyla geometri verimli bir şekilde işlemek için.
- Hassas bir denetim Metal performansını iyileştirmek için kaynak ayırma kaynak yığınlardaki kullanarak uygulamaları ve Memoryless işleme hedefleri sağlayın.
- İşlev uzmanlık malzeme ve açık birleşimi işlevleri bir Sahne için yüksek oranda iyileştirilmiş koleksiyonu oluşturmak için kullanın.

Daha fazla bilgi için Apple'nın bkz [Metal Programlama Kılavuzu](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

## <a name="modelio-enhancements"></a>ModelIO geliştirmeleri

İOS 10 ModelIO Framework'te için aşağıdaki geliştirmeler yapılmıştır:

 - ABD Doları dosya biçimi artık desteklenmektedir.
 - Uzaklık için destek eklenmiştir alan imzalı [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) sınıfı.
 - Yeni `MDLLightProbeIrradianceDataSource` ışık araştırma yerleştirmeyi yardımcı sınıfı.
 - Yeni `MDLMaterialPropertyGraph` kolayca çalışma zamanı desteklemek için sınıf modellerle değiştirir.

## <a name="photos-enhancements"></a>Fotoğraf geliştirmeleri

Aşağıdaki geliştirmeleri iOS 10 fotoğraf Framework'te yapılmıştır:

- Kullanım [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) ve [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) düzenlemeleri gerçekleştirmek için yeni çekirdek Görüntü İşlemci özelliğinin avantajlarından yararlanmak için sınıflar.
- Canlı fotoğraf düzenleme şimdi fotoğraf çerçevesini destekleyen uygulamalar ve fotoğraf uzantıları düzenleme için kullanılabilir (fotoğraf ve kamera içinde kullanmak için uygulamalar).
- Yeni [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) sınıfı düzenlemeleri hem video hem de Live Fotoğraf içeriği için hala geçerli.

## <a name="replaykit-enhancements"></a>ReplayKit geliştirmeleri

İOS 10 ReplayKit Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- Kullanım [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) ve [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) sınıfları, yayını desteklemek için kaydedilen 3. taraf aracılığıyla medya siteleri.
- Yayın kullanıcı Arabirimi ve yayın karşıya uzantıları uygulamada ReplayKit 3. taraf yayın Hizmetleri desteklemek için gereklidir.

## <a name="scenekit-enhancements"></a>SceneKit geliştirmeleri

İOS 10 SceneKit Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/) sınıfı HDR özellikleri ve efektler kullanarak büyük daha iyi sağlayabilir. Uyarlamalı Etkilenme otomatik efektler veya kullanım Vinyet oluşturma, renk koyu çizgileri ve oyuna fillmatic efektleri eklemek için renk sınıflandırma oluşturmak için kullanın.
- SceneKit artık daha basit varlık yazma daha gerçekçi sonuçlar yeni bir fiziksel olarak bağlı işleme (PBR) sistemi içerir.
- Yeni [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) gölgelendirme model ürün için çok çeşitli gerçekçi gölgelendirme etkileri yalnızca üç temel özellikleri gerektiren sırasında (`Diffuse`, `Metalness` ve `Roughness`).
- Ortam tabanlı aydınlatma ile en iyi şekilde çalışır gölgelendirme PBR itibaren kullanın `LightingEnvironment` için tüm bir Sahne görüntü tabanlı aydınlatma atamak için özellik.
- Kullanım `IESProfileURL` gerçek değerler yoğunluğu (içinde Lümen) ve renk sıcaklığı (derece cinsinden Kelvin) gibi temel aydınlatma tanımlayan gerçek hafif armatürleri almak için özellik.
- Geleneksel işleme tekniklerini daha iyi sonuçlar PBR ve HDR Kamera özellikleri sağlar ve sonuç olarak, SceneKit şimdi renk hesaplamalarının (wide renk aygıt ekranlarda P3 renk skalasını kullanarak) doğrusal renk alanını gerçekleştirir.
- SceneKit şimdi rengi renk profil bilgileri okuyarak tüm renkleri eşleştirir.
- SceneKit renk bileşeni değerleri tüm gölgelendirici türleri için doğrusal bir RGB renk alanında yorumlar.
- Doğrusal hem de alan işleme renk ve geniş renk belirterek devre dışı bırakılacak `SCNDisableLinearSpaceRendering` ve `SCNDisableWideGamut` uygulamanın anahtarlarında `Info.plist`.
- Rastgele Çokgen primates (veya dosyalarından yüklenen program aracılığıyla oluşturulan) yapı geometri yeni belirtmek için [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) sınıfı.
- SceneKit okur ve renk profil bilgilerini doku görüntülerinde ayarlamak için bu bilgiler sağlanmıştır emin olmak için tüm görüntüleri için varlık katalog kullanın.

## <a name="spritekit-enhancements"></a>SpriteKit geliştirmeleri

İOS 10 SpriteKit Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- Özel gölgelendiriciler öznitelikleri sağlayabilir (`SKAttribute`) yapılandırılabilir ayrı ayrı bir öznitelik değeri sağlayarak gölgelendirici kullanan her bir düğüm tarafından (`SKAttributeValue`).
- Tilemaps 2B, 2.5 D ve yan kaydırma oyunlar kullanarak için kare, Altıgen ve İzometrik bölmesi şekilleri şimdi desteği `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` ve `SKTileSet` sınıfları.
- Yeni `SKWarpGeometry` uzatma veya deforme sınıfı [SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/) veya [SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/) işleme. Yeni [SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) sınıfı, tüneli etkileri arasında geçişler animasyon uygulamak için kullanılabilir.
- [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/) sınıfı nasıl ve ne zaman bir Sahne işlenmeden üzerinde ayrıntılı denetim vermek için birkaç yeni yöntemler sağlar.

## <a name="scrollview-enhancements"></a>ScrollView geliştirmeleri

İOS 10.3 ScrollView denetiminde aşağıdaki geliştirmeler yapılmıştır:

- `UIScrollView` Şimdi dahil `IndexDisplayMode` kullanıcı olarak kaydırma sırasında dizin nasıl gösterilen denetlemek için özellik bir `UIScrollViewIndexDisplayMode` biri:
    - `Automatic` -Dizin görüntüsünü, işletim sistemi tarafından denetlenir.
    - `AlwaysHidden` -Dizin görüntüsünü her zaman gizlenir.

Bkz: [iOSTenThree örnek](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree) kullanım için.

## <a name="uikit-enhancements"></a>Uıkit geliştirmeleri

İOS 10 Uıkit Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- Yeni [UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) API uyumlu içerik türleri ortak sınıf türleri için otomatik olarak bildirir ve yeni seçenekler (örneğin, yaşam süresi sınırlamaları) sağlar.
- Yeni tamamen etkileşimli, nesne tabanlı, kesilebilir animasyon desteği eklendi ve hareketleri için bağlı olabilir. Pleas bkz: Apple's [UIViewAnimating Protokolü başvurusu](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator sınıf başvurusu](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protokolü başvurusu](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters sınıf başvurusu](https://developer.apple.com/reference/uikit/uicubictimingparameters) ve [UISpringTimingParameter sınıf başvurusu](https://developer.apple.com/reference/uikit/uispringtimingparameters) daha fazla bilgi için.
- Yeni `UIPreviewInteraction` ve `UIPreviewInteractionDelegate` gözlem ve pop işlemleri için özel bir arabirim sağlamak Geliştirici uygulamanın izin.
- Yeni `UIAccessibilityCustomRotor` sınıfı gibi ses üzerinden yardımcı teknolojiler, bağlam özgü özel işlevsellik sağlamak uygulamanın olanak tanır.
- Kullanım `UIAccessibilityIsAssistiveTouchRunning` ve `UIAccessibilityAssistiveTouchStatusDidChangeNotification` AssistiveTouch etkin olup olmadığını belirlemek için simgeler.
- Kullanım `UIAccessibilityHearingDevicePairedEar` ve `UIAccessibilityHearingDevicePairedEarDidChangeNotification` herhangi durumunu almak için simgeler Eşli MFi işitme yardımları.
- Dinamik tür etiketler desteklemek için metin alanları ve metin kutuları yeni kullanma `PreferredFontForTextStyle` yöntemi `UIFont` sınıfı.
- Bir öğe, yazı tipi güncelleştirmesi olup karar vermek için cihazın `UIContentSizeCategory` değişiklikleri, kullanmak `AdjustsFontForContentSizeCategory` özelliği `UIContentSizeCategoryAdjusting` temsilci.
- `OpenURL` Yöntemi `UIApplication` sınıfı zaman uyumsuz olarak adlandırılır ve artık açık eylem tamamlandıktan sonra çağrılan bir tamamlanma işleyici destekler.
- CloudKit paylaşımı başlatmak ve özelliklerini kullanarak yeni değiştirerek `UICloudSharingController` ve `UICloudSharingControllerDelegate` sınıfları.
- Kaydırma deneyimini geliştirmek için prefetched hücreleri yararlanmak `UICollectionViews` yeni `UICollectionViewDataSourcePrefetching` temsilci.
- Geliştirici şimdi Sekme çubuğu öğeleri (örneğin, metin ve arka plan rengi) rozet görünümünü denetleyebilirsiniz.
- Tüm kaydırma görünümü ve kaydırma görünüm sınıfları yenileme denetimi artık desteklenmektedir (gibi `UICollectionView`).

## <a name="webkit-enhancements"></a>WebKit geliştirmeleri

İOS 10 WebKit Framework'te için aşağıdaki geliştirmeler yapılmıştır:

- Peek ve pop destek eklenmiştir `WKWebView` sınıfı. Kullanım `ShouldPreviewElement` yöntemi verilen web görünümü Önizleme görüntülenip görüntülenmeyeceğini belirler.


## <a name="related-links"></a>İlgili bağlantılar

- [iOS 10 örnekleri](https://developer.xamarin.com/samples/ios/iOS10/)
- [İOS 10 yenilikler nelerdir?](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
