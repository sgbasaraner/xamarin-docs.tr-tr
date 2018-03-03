---
title: "Özellik listeleri ile çalışma"
description: "Bu belge Info.plist ve Entitlements.plist ile çalışmak için Visual Studio için Mac'ın grafik ve gelişmiş özellik listesi (.plist) Düzenleyicisi içerir. Ayar simgeler ve iOS uygulamalardan başlatma görüntülerde gösterilmektedir Mac için Visual Studio içinde"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 0d7b4c5a539470a3544d0117251f40fd6bd37f2b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-property-lists"></a>Özellik listeleri ile çalışma

_Bu belge Info.plist ve Entitlements.plist ile çalışmak için Visual Studio için Mac'ın grafik ve gelişmiş özellik listesi (.plist) Düzenleyicisi içerir. Ayar simgeler ve iOS uygulamalardan başlatma görüntülerde gösterilmektedir Mac için Visual Studio içinde_

Mac için Visual Studio düzenleme uygulama özelliklerini ve yeteneklerini daha kolay hale getirir bir grafik .plist Düzenleyicisi özellikleri. Visual Studio Mac için olan iki .plists - `Info.plist` uygulama özellikleri ve simgeler, düzenleme ve `Entitlements.plist` uygulama özelliklerini yönetme. Bu kılavuz Info.plists tanıtır ve bunlarla Mac için Visual Studio içindeki çalışma genel bir bakış sağlar Entitlements.plist hakkında daha fazla bilgi için bkz: [yetkilendirmeler ile çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

## <a name="infoplist"></a>Info.plist

Bilgi özellik listesi ( `Info.plist`), uygulamanızın yapılandırma sistemi hakkında bilgiler sağlar gerekli iOS dosyasıdır. Mac için Visual Studio özel `Info.plist` altındaki sekmeleri denetlediği üç panel Düzenleyicisi pencerenin sol Düzenleyicisi özellikleri:

 [ ![](property-lists-images/tabs.png "Alt Info.plist Düzenleyicisi sekmeleri Düzenleyicisi pencerenin sol")](property-lists-images/tabs.png)

Her paneli aşağıda özetlendiği gibi farklı özellikleri denetler:

-  **Uygulama panelini** -simgeleri ve başlatma yanı sıra, genel uygulama özelliklerini ayarlamak için bir grafik arabirim görüntüler; eşlemeleri tümleştirme ve modları backgrounding belirtin.
-  **Panel Gelişmiş** -Gelişmiş panelindeki desteklenen belge türü, UTIs, belirtmek için yerdir ve URL türleri.
-  **Kaynak paneli** -daha az görülen özelliklerinin yanı sıra uygulama için özel özellikler kaynağı paneli denetler.


Sonraki üç bölümde ayrıntılı her panelinde özelliklerini araştırın.

## <a name="application-panel"></a>Uygulama paneli

Mac için Visual Studio özellikleri ortak düzenlemek için bir grafik arabirim `Info.plist` girişleri bir uygulama için:

1.  Uygulama özellikleri
1.  Desteklenen cihaz türleri
1.  Her cihaz türü için destek yönler
1.  Durum stil ve renk çubuğu
1.  Simgeler ve başlangıç ekranlar
1.  MAPS ve arka plan modları


Bu sonraki bölümlerde daha ayrıntılı olarak açıklanmıştır.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>iOS uygulaması hedefi

Bu bölümde, uygulamanızın açıklayan önemli bilgiler içerir.
**Tanımlayıcısı** depolanan iTunes Bağlan (uygulama mağazası uygulamaları için) hem de geliştirme ve dağıtım sertifikalarını ve iOS sağlama Portalı Uygulama kimlikleri listesini girilen paket tanımlayıcısı burada eşleşmelidir.

 [ ![](property-lists-images/image24.png "iOS uygulaması hedefi")](property-lists-images/image24.png)

### <a name="device-deployment"></a>Aygıt dağıtımı

 [ ![](property-lists-images/deployment.png "Aygıt dağıtımı")](property-lists-images/deployment.png)

Aygıt **dağıtım** bilgisi bölümleri görüntülenir seçerek, seçime bağlı olarak **aygıtları** açılır listede **uygulama hedef** yukarıdaki bölümde. **Ana arabirimi** açılan ayarlanmış **MainStoryboard** film şeridi tabanlı uygulamalarda. Kullanıcı arabirimi tamamen kodda yazılır, sonra bu boş bırakılabilir.

### <a name="supported-device-orientations"></a>Desteklenen cihaz yönler

 **Cihaz yönler desteklenen** uygulama aygıt döndürme nasıl yanıt vereceğini denetler. İPhone/iPad uygulamaların yalnızca desteklemek için çok yaygındır **dikey**, ya da her şeyi ancak **ters**. Genellikle tüm yönleri oyunlar dışındaki tüm iPad uygulamaları desteklemelidir.

### <a name="status-bar-styles"></a>Durum çubuğu stilleri

**Durum çubuğu stilleri** bölümdür uygulamanın düzenlemek için bir grafik arabirim `UIStatusBarStyle`:

 [ ![](property-lists-images/status.png "Durum çubuğu stilleri")](property-lists-images/status.png)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Simgeler, başlatma görüntüler ve iTunes resmi

Simgeler, görüntü ve resimler Info.plist dosyanızdaki kullanma hakkında bilgi bulunabilir [görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) Kılavuzu.




### <a name="maps-integration-and-background-modes"></a>MAPS tümleştirme ve arka plan modları

`Info.plist` Eşlemeleri tümleştirme ve modları backgrounding belirtmek için özel bölümleri içerir. Desteklemek istediğiniz seçenekleri belirleyerek uygulamanıza sizin için gerekli özellikler ekleyeceksiniz.

 [ ![](property-lists-images/maps.png "MAPS tümleştirme")](property-lists-images/maps.png)

Xamarin Haritalar ile çalışma hakkında daha fazla bilgi için bkz [iOS eşlemeleri](~/ios/user-interface/controls/ios-maps/index.md) Kılavuzu.

 [ ![](property-lists-images/bging.png "Arka plan modları")](property-lists-images/bging.png)

Arka plan modları hakkında daha fazla bilgi için Xamarin başvuran [iOS Backgrounding](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) Kılavuzu.

## <a name="advanced-panel"></a>Gelişmiş paneli

Gelişmiş panelindeki belge türleri ve uygulamasının desteklediği URL şemalarını denetler.

 [ ![](property-lists-images/image34.png "Gelişmiş paneli")](property-lists-images/image34.png)

 <a name="Document_Types" />


## <a name="document-types"></a>Belge türü

Belirli türde dosyaları açma destekleyen uygulamalar için iOS sağlar `CFBundleDocumentTypes` anahtarı. Belirli - örneğin PDF - bilinen dosya türlerini desteklemek için uygulamamız istiyoruz, PDF değeri anahtarına eklersiniz. Bu bölüm içinde depolanan verileri girmek için en uygun yoldur `CFBundleDocumentTypes` anahtarını `Info.plist` dosya.

Üzerinde belgelere bakın [dosya türlerini Your App destekler kaydetme](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) bu değerlerinin nasıl yapılandırılacağı hakkında ayrıntılı bilgi için.

## <a name="utis"></a>UTIs

Bazen bir uygulamanın özel dosya türünü açarken desteklemesi gerekir. Örneğin, bir özel uzantısına sahip bir görüntü dosyaları açmaya istiyoruz *.xam*. Bir özel dosya türünü belirtmek için kullanarak özel bir UTI - Evrensel tür tanımlayıcısı - oluşturacağız `UIExportedTypeDeclarations` anahtarı. Aşağıdaki ekran görüntüsünde .xam uzantısı için özel bir UTI oluşturulacağını gösterir:

 [ ![](property-lists-images/uti.png "UTIs Düzenleyicisi")](property-lists-images/uti.png)

Yalnızca olarak verilen tür UTIs belirtin, uygulamanızın belirli özel UTIs *türüne UTIs alınan* ( `UIImportedTypeDeclarations` anahtarı) desteklenir, ancak, uygulamanız tarafından ait olmayan özel türlerini belirtin.

Apple için özel UTIs kullanma hakkında daha fazla bilgi için bkz [kaydetme dosya türleri Your App destekler](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) Kılavuzu.

## <a name="custom-urls"></a>Özel URL'leri

(Protokolü olarak da bilinir) bir URL şeması adı URL'sinin ilk bölümüdür. Örneğin, `http://` ve `https://` ortak URL düzenler. Uygulamanız için özel bir URL şemasına oluşturma seçeneğiniz vardır. Özel URL şemalarını iletişim kurar ve veri diğer uygulamalarla İleri ve geri göndermek için kullanılır. Aşağıdaki ekran görüntüsü olarak adlandırılan yeni özel URL şeması oluşturma gösterilmektedir `monkeys://`:

 [ ![](property-lists-images/url.png "Özel URL'leri")](property-lists-images/url.png)



Özel URL şemalarını uygulama konusunda daha fazla bilgi için Apple için başvuruda [uygulama özel URL şemalarını başlığına bakın](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Kaynağı paneli

**Kaynak** sekmesinde `Info.plist` dosya eklenmiş veya düzenlenmişse için özel değerler sağlar. Mac için Visual Studio en yaygın özelliklerinin bir listesi sağlar:

 [ ![](property-lists-images/image31.png "Aşağı açılır listeden yeni özellik ekleme")](property-lists-images/image31.png)

Mac için Visual Studio bilinen bir özellik için geçerli bir değer listesi tarafından aşağıdaki ekran görüntüsünde gösterildiği gibi olur:

 [ ![](property-lists-images/image32.png "Bir bilinen değer listesinden bir değer seçin")](property-lists-images/image32.png)

Mac için Visual Studio, özellik türü de gösterildiği gibi algılar:

 [ ![](property-lists-images/image33.png "Kullanılabilir özellik türleri")](property-lists-images/image33.png)

Apple'nın gözden [uygulama ilgili kaynaklar](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) isteğe bağlı özellikler hakkında daha fazla bilgi için bağlantılar.

 <a name="Entitlements" />

## <a name="summary"></a>Özet

Bu makalede, simgeler belirtin ve görüntüleri başlatmak için yaygın uygulama yapılandırmaları da düzenlemek için grafik ve Gelişmiş .plist düzenleyicileri kullanma gösterilmektedir. Bu ayrıca sunulmuştur `Entitlements.plist` ekleme ve uygulama özelliklerini yönetme.


## <a name="related-links"></a>İlgili bağlantılar

- [IDE](https://developer.xamarin.com/recipes/cross-platform/ide)
- [Uygulama ilgili kaynaklar](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Dosyayı kaydetme uygulama destekler, türleri](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Özel URL şemalarını uygulama](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Varlık kataloglar hakkında](https://developer.apple.com/library/ioshttps://developer.xamarin.com/recipes/xcode_help-image_catalog-1.0/Recipe.html)
