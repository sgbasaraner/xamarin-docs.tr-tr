---
title: Xamarin.iOS, özellik listeleri ile çalışma
description: Bu belge, Visual Studio Entitlements.plist Info.plist ile çalışmak için Mac grafik ve gelişmiş özellik listesi (.plist) Düzenleyicisi için tanıtır. Ayar simgeleri ve başlatma görüntüleri için iOS uygulamalarından gösterir Mac için Visual Studio içinde
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b102f268fd457ca42f3d64b5766a2b3824e3849d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242335"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Xamarin.iOS, özellik listeleri ile çalışma

_Bu belge, Visual Studio Entitlements.plist Info.plist ile çalışmak için Mac grafik ve gelişmiş özellik listesi (.plist) Düzenleyicisi için tanıtır. Ayar simgeleri ve başlatma görüntüleri için iOS uygulamalarından gösterir Mac için Visual Studio içinde_

Mac için Visual Studio düzenleme uygulama özellikleri ve yetenekleri daha kolay hale getiren bir grafik .plist Düzenleyici Özellikleri. Mac için Visual Studio sahip iki .plists - `Info.plist` uygulama özellikleri ve simgeler, düzenleme ve `Entitlements.plist` uygulama özellikleri yönetmek için. Bu kılavuz Info.plists tanıtır ve bunlarla Mac için Visual Studio'da çalışmaya genel bir bakış sağlar Entitlements.plist hakkında daha fazla bilgi için bkz: [Yetkilendirmelerle çalışma](~/ios/deploy-test/provisioning/entitlements.md) Kılavuzu.

## <a name="infoplist"></a>Info.plist

Bilgi özellik listesi ( `Info.plist`) sistemine uygulamanızın yapılandırma hakkında bilgi sağlayan bir gerekli iOS dosyasıdır. Mac için Visual Studio özel `Info.plist` en altındaki sekmeleri tarafından denetlenen üç panel Düzenleyicisi penceresinin sol Düzenleyici Özellikleri:

 [![](property-lists-images/tabs.png "Altındaki Info.plist Düzenleyici sekmeler Düzenleyicisi penceresinin sol")](property-lists-images/tabs.png#lightbox)

Her paneli aşağıda özetlendiği gibi farklı özellikleri denetler:

-  **Uygulama panelinde** -simgeleri ve başlatma yanı sıra, genel uygulama özellikleri ayarlamak için bir grafik arabirim görüntüler; haritalar tümleştirmesi ve modları arka planda işleme belirtin.
-  **Paneli Gelişmiş** -Gelişmiş panelindeki Utı'leri, desteklenen belge türlerini belirtmek için yerdir ve URL türleri.
-  **Kaynak panelini** -kaynak panelini daha az yaygın özelliklerinin yanı sıra, uygulama için özel özellikleri denetler.


Sonraki üç bölümde her panelinde ayrıntılı özelliklerini inceleyin.

## <a name="application-panel"></a>Uygulama paneli

Mac için Visual Studio özellikleri ortak düzenlemek için bir grafik arabirim `Info.plist` bir uygulama için:

1.  Uygulama özellikleri
1.  Desteklenen cihaz türleri
1.  Her cihaz türünün destek yönleri
1.  Durum çubuğu stili ve rengini
1.  Simgeleri ve başlatma ekranları
1.  Haritalar ve arka plan modları


Bu, sonraki bölümlerde daha ayrıntılı açıklanmıştır.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>iOS uygulama hedefi

Bu bölümde, uygulamanızın açıklayan önemli bilgileri içerir.
**Tanımlayıcı** depolanan burada iTunes Connect (App Store uygulamaları için) ve ayrıca geliştirme ve dağıtım sertifikalarını ve iOS sağlama Portalı Uygulama kimlikleri listesi girdiğiniz paket tanımlayıcısı ile eşleşmelidir.

 [![](property-lists-images/image24.png "iOS uygulama hedefi")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Cihaz dağıtımı

 [![](property-lists-images/deployment.png "Cihaz dağıtımı")](property-lists-images/deployment.png#lightbox)

Cihaz **dağıtım** bilgileri bölümlerinde yer alan görüntülenme seçerek, seçime bağlı olarak **cihazları** açılır menüde **uygulama hedef** yukarıdaki bölümde. **Ana arabirimi** açılan ayarlandığında **MainStoryboard** film şeridi odaklı uygulamalarda. Kullanıcı arabirimi tamamen kodunda yazılır, sonra bu boş bırakılabilir.

### <a name="supported-device-orientations"></a>Desteklenen cihaz yönleri

 **Desteklenen cihaz yönleri** uygulama cihaz döndürmeyi nasıl yanıt verdiğini denetler. İçin iPhone/iPad uygulama yalnızca desteklemek çok yaygındır **dikey**, her şeyi ancak **tersyüz**. Genellikle oyunlar dışındaki tüm iPad uygulamaları tüm yönleri desteklemelidir.

### <a name="status-bar-styles"></a>Durum çubuğu stilleri

**Durum çubuğu stilleri** bölümdür uygulamanın düzenlemek için bir grafik arabirim `UIStatusBarStyle`:

 [![](property-lists-images/status.png "Durum çubuğu stilleri")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>İTunes resmi simgeleri ve başlatma resimleri

Simgeler, görüntü ve resimler, uygulamanızın Info.plist dosyası içinde kullanma hakkında daha fazla bilgi bulunabilir [görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) Kılavuzu.




### <a name="maps-integration-and-background-modes"></a>Haritalar tümleştirmesi ve arka plan modları

`Info.plist` Haritalar tümleştirmesi ve modları arka planda işleme belirtmek için özel bölümleri içerir. Desteklemek istediğiniz seçenekleri belirleyerek, uygulamanız için gerekli özellikler ekleyeceksiniz.

 [![](property-lists-images/maps.png "Haritalar tümleştirmesi")](property-lists-images/maps.png#lightbox)

Haritalar ile çalışma hakkında daha fazla bilgi için başvurmak için Xamarin [iOS haritalar](~/ios/user-interface/controls/ios-maps/index.md) Kılavuzu.

 [![](property-lists-images/bging.png "Arka plan modları")](property-lists-images/bging.png#lightbox)

Xamarin için arka plan modları hakkında daha fazla bilgi için bkz [iOS arka planda işleme](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) Kılavuzu.

## <a name="advanced-panel"></a>Gelişmiş paneli

Gelişmiş panelindeki belge türleri ve uygulamanın desteklediği URL şemalarını denetler.

 [![](property-lists-images/image34.png "Gelişmiş paneli")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>Belge türleri

İOS açma belirli dosya türlerini destekleyen uygulamalar sağlayan `CFBundleDocumentTypes` anahtarı. Belirli bilinen dosya türleri - örneğin PDF - desteklemek için uygulamamız istiyoruz, anahtar PDF değeri ekleriz. Bu bölüm içinde depolanan verileri girmek için kullanışlı bir yol sağlayan `CFBundleDocumentTypes` anahtarını `Info.plist` dosya.

Belgelerine başvurun [dosya türleri bilgisayarınızı uygulamanın desteklediği kaydetme](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) bu değerlerinin nasıl yapılandırılacağı hakkında ayrıntılı bilgi için.

## <a name="utis"></a>Utı'leri

Bazen bir uygulama bir özel dosya türünü açmayı desteklemesi gerekir. Örneğin, özel bir uzantıya sahip resim dosyalarını açmak istediğimiz *.xam*. Bir özel dosya türünü belirtmek için kullanarak özel bir UTI - Evrensel tür tanımlayıcısı - oluşturacağız `UIExportedTypeDeclarations` anahtarı. Aşağıdaki ekran görüntüsünde .xam uzantısı için özel bir UTI oluşturma işlemini göstermektedir:

 [![](property-lists-images/uti.png "Utı Düzenleyicisi")](property-lists-images/uti.png#lightbox)

Yalnızca olarak dışarı aktarılan tür Utı'leri belirtin, uygulamanıza özgü özel Utı'leri *içeri aktarılan tür Utı'leri* ( `UIImportedTypeDeclarations` anahtarı) desteklenir, ancak uygulamanız tarafından sahip olunan değil özel türlerini belirtin.

Özel Utı'leri kullanma hakkında daha fazla bilgi için Apple başvuran [kaydetme dosya türleri bilgisayarınızı uygulamanın desteklediği](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) Kılavuzu.

## <a name="custom-urls"></a>Özel URL

(Protokol olarak da bilinir) bir URL düzeni adı, URL ilk parçasıdır. Örneğin, `http://` ve `https://` genel URL düzenler. Uygulamanız için özel bir URL şeması oluşturma seçeneğiniz vardır. Özel URL şemalarını iletişim kurmak ve diğer uygulamalarla sürekli veri göndermek için kullanılır. Adlı yeni bir özel URL şeması oluşturma aşağıdaki ekran görüntüsünde gösterilmektedir `monkeys://`:

 [![](property-lists-images/url.png "Özel URL")](property-lists-images/url.png#lightbox)



Özel URL şemalarını uygulama ile ilgili daha fazla bilgi için Apple başvuran [uygulama özel URL şemalarını bölümü bu kılavuzun](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Kaynağı bölmesi

**Kaynak** sekmesinde `Info.plist` dosyası eklenmişse veya düzenlenmişse için özel değerler sağlar. Mac için Visual Studio, en yaygın özellikleri listesi sağlar:

 [![](property-lists-images/image31.png "Açılan listeden yeni özellik ekleme")](property-lists-images/image31.png#lightbox)

Bilinen özellikleri Mac için Visual Studio için geçerli değerler listesi tarafından aşağıdaki ekran görüntüsünde gösterildiği gibi olacaktır:

 [![](property-lists-images/image32.png "Bir bilinen değer listesinden bir değer seçin")](property-lists-images/image32.png#lightbox)

Mac için Visual Studio özellik türü, gösterildiği gibi de algılar:

 [![](property-lists-images/image33.png "Kullanılabilir özellik türleri")](property-lists-images/image33.png#lightbox)

Apple'nın gözden [uygulama ilgili kaynakları](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) isteğe bağlı özellikler hakkında daha fazla bilgi için bağlantılar.

 <a name="Entitlements" />

## <a name="summary"></a>Özet

Bu makalede, simgeler belirtin ve başlatma görüntüleri için yaygın uygulama yapılandırmaları da düzenlemek için grafik ve Gelişmiş .plist düzenleyicileri kullanarak gösterdik. Bu da sunulan `Entitlements.plist` ekleme ve uygulama özelliklerini yönetme.


## <a name="related-links"></a>İlgili bağlantılar

- [IDE](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [Uygulama ilgili kaynaklar](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Uygulamanızın desteklediği dosya kaydetme türleri](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Özel URL düzeni uygulama](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Varlık Kataloğu biçimi başvurusu](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
