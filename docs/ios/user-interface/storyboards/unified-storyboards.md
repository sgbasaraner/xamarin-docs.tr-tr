---
title: "Birleşik film şeritleri"
description: "Birleşik film şeritleri iOS cihazı ekran boyutlarına genişleyen aralığını kapsamak amacıyla birden çok film şeritleri yerine tek bir film şeridi ile kullanıcı arabirimini oluşturmak Geliştirici izin verir. Bu makalede, birleştirilmiş film şeridi Xamarin.iOS içinde işlemde daha derin bir genel bakış verecek şekilde tasarlanmıştır."
ms.topic: article
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 60b2e6fa65226631fe2d2c847a56852ac9ae63d2
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="unified-storyboards"></a>Birleşik film şeritleri

_Birleşik film şeritleri iOS cihazı ekran boyutlarına genişleyen aralığını kapsamak amacıyla birden çok film şeritleri yerine tek bir film şeridi ile kullanıcı arabirimini oluşturmak Geliştirici izin verir. Bu makalede, birleştirilmiş film şeridi Xamarin.iOS içinde işlemde daha derin bir genel bakış verecek şekilde tasarlanmıştır._

iOS 8 kullanıcı arabirimini oluşturmak için yeni, kullanımı kolay bir mekanizma içerir — birleşik film şeridi. Farklı donanım ekran boyutlarına tümünün karşılamak üzere tek film şeridi, hızlı ve esnek görünümleri oluşturulabilir bir "tasarım-once, kullanım çok" stili.

İPhone ve iPad cihazları için ayrı ve belirli film şeridi oluşturmak için geliştiricinin artık gerek duyduğu gibi ortak bir arabirim ile uygulamalar tasarlama ve farklı boyutu sınıfları için bu arabirimi özelleştirme esnekliğine sahip olursunuz. Bu şekilde uygulama her form faktörü gücü için uyarlanabilir ve her kullanıcı arabirimi, en iyi deneyimi sağlamak için ayarlanmış.

<a name="size-classes" />

## <a name="size-classes"></a>Boyutu sınıfları

İOS 8 önce Geliştirici kullanılan `UIInterfaceOrientation` ve `UIInterfaceIdiom` dikey ve yatay modlar arasında ve iPhone ve iPad cihazları arasında ayırt etmek için. İOS8 Yönlendirme ve cihaz belirlenir kullanarak *boyutu sınıfları*.

Aygıtları dikey ve yatay eksen boyutu sınıfları tarafından tanımlanır ve iOS 8 boyutu sınıflarda iki tür vardır:

-  **Normal** – büyük ekran boyutu (örneğin, iPad) veya büyük bir boyuta izlenim sunan bir araç için budur (gibi bir `UIScrollView`
-  **Compact** – bu küçük aygıtlar (örneğin, bir iPhone) içindir. Bu boyut cihaz yönünü dikkate alır.


İki kavramları birlikte kullandıysanız, aşağıdaki diyagramda görüldüğü gibi iki farklı yönleri içinde kullanılabilir farklı olası boyutları tanımlar 2 x 2 kılavuz oluşur:

 [![](unified-storyboards-images/sizeclassgrid.png "Kullanılabilir farklı olası boyutları normal ve Compact yönler tanımlayan 2 x 2 kılavuz")](unified-storyboards-images/sizeclassgrid.png#lightbox)

Geliştirici farklı düzenleri (Yukarıdaki grafik görüldüğü gibi) neden olan dört olasılık birini kullanan bir görünüm denetleyicisi oluşturabilirsiniz.

### <a name="ipad-size-classes"></a>iPad boyutu sınıfları

Boyutu nedeniyle iPad sahip bir **normal** sınıfını hem yönler boyutu.

 [![](unified-storyboards-images/image1.png "iPad boyutu sınıfları")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone boyutu sınıfları

İPhone cihaz yönünü göre farklı boyutu sınıfları içerir:

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone boyutu sınıfları")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  Cihaz dikey modda olduğunda, ekran sahip bir **compact** yatay olarak sınıfı ve **normal** dikey
-  Cihaz yatay modda olduğunda, ekran sınıfları dikey modundan ters çevrilir.

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus boyutu sınıfları

Boyutlar dikey yönde olduğunda, yatay farklı ancak önceki iPhone ile aynıdır:

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus boyutu sınıfları")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

Çünkü iPhone 6 Plus büyüklükte bir ekran sahipse, normal bir genişlik boyutu sınıfın yatay modunda sahip mümkün olur.

### <a name="support-for-a-new-screen-scale"></a>Yeni bir ekran ölçeği desteği

İPhone 6 Plus ile ekran ölçek faktörü 3.0 (üç kez özgün iPhone ekran çözünürlüğü), yeni bir Retina HD ekran kullanır. Bu aygıtlar üzerinde olası en iyi deneyimi sağlamak için bu ekranı ölçek için tasarlanan yeni resmi içerir. Xcode 6 ve üzeri, varlık kataloglar 1 x, x 2 ve 3 x boyutları görüntülere içerebilir; Yeni görüntü varlıklarının iOS seçip doğru varlıklar bir iPhone 6 üzerinde çalışırken eklemeniz yeterlidir artı.

Ayrıca yükleme davranışını iOS içinde görüntü tanıdığı bir `@3x` görüntü dosyaları soneki. Örneğin, uygulamanın paket şu dosya adlarına sahip bir görüntü varlığı (çözünürlüklerde farklı) Geliştirici içerir: `MonkeyIcon.png`, `MonkeyIcon@2x.png`, ve `MonkeyIcon@3x.png`. İPhone 6 Plus `MonkeyIcon@3x.png` görüntü kullanılacak otomatik olarak Geliştirici aşağıdaki kodu kullanarak görüntü yüklerse:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
Ya da iOS kullanarak bir UI öğe için resmin atarsanız olarak Tasarımcı `MonkeyIcon.png`, `MonkeyIcon@3x.png` , otomatik olarak yeniden iPhone 6 kullanılacak artı.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>Dinamik başlatma ekranlar

Uygulamayı gerçekten başlayarak yukarı kullanıcı için geri bildirim sağlamak için bir iOS uygulaması başlatma sırasında başlatma ekranı dosya bir giriş ekranı görüntülenir. İOS 8 önce birden çok içerecek şekilde Geliştirici olurdu `Default.png` görüntü varlıkları için uygulamanın çalışıyor olması her aygıt türü, Yönlendirme ve ekran çözünürlüğü.

Yeni iOS 8, tek bir atomik Geliştirici oluşturabilirsiniz `.xib` otomatik düzeni ve boyutu sınıfları oluşturmak için kullandığı Xcode dosyasında bir *dinamik başlatma ekranı* her aygıt, çözümleme ve yönlendirme için çalışır. Bu yalnızca oluşturmak ve tüm gerekli görüntü varlıklarını korumak için geliştiricilerin yapması gereken iş miktarını azaltır, ancak uygulamanın yüklü paket boyutunu azaltır.

## <a name="traits"></a>Özellikleri

Nitelikler bir düzen ortamı değişikliklerin nasıl değiştiğini belirlemek için kullanılan özelliklerdir. Bir özellikler kümesi oluşur ( `HorizontalSizeClass` ve `VerticalSizeClass` göre `UIUserInterfaceSizeClass`), arabirim deyim yanı sıra ( `UIUserInterfaceIdiom`) ve görüntü ölçeği.

Yukarıdaki durumları tümünün ayırdedici nitelik koleksiyonu olarak Apple başvurduğu bir kapsayıcıda sarılır ( `UITraitCollection`), yalnızca özellikleri, ancak bunların değerleri de içerir.

## <a name="trait-environment"></a>Ayırdedici nitelik ortamı

Ayırdedici nitelik ortamları iOS 8 yeni arabiriminde ve aşağıdaki nesneler için ayırdedici nitelik koleksiyonu döndürmek kullanabilirsiniz:

-  Ekranlar ( `UIScreens` ).
-  Windows ( `UIWindows` ).
-  Görüntüleme denetleyicileri ( `UIViewController` ).
-  Görünümler ( `UIView` ).
-  Sunu denetleyicisi ( `UIPresentationController` ).


Geliştirici ayırdedici nitelik ortamı tarafından döndürülen ayırdedici nitelik koleksiyonu bir kullanıcı arabirimi nasıl düzenleneceğini belirlemek için kullanır.

Aşağıdaki diyagramda görüldüğü gibi tüm ayırdedici nitelik ortamların bir hiyerarşi olun:

 [![](unified-storyboards-images/viewhierarchy.png "Ayırdedici nitelik ortamları hiyerarşi diyagramı")](unified-storyboards-images/viewhierarchy.png#lightbox)

Ayırdedici nitelik koleksiyonu her yukarıdaki ayırdedici nitelik ortamlara sahip olduğundan, varsayılan olarak, alt ortamına üst öğeden akar.

Geçerli ayırdedici nitelik koleksiyonu alma yanı sıra ayırdedici nitelik ortamı sahip bir `TraitCollectionDidChange` görünüm veya Görünüm denetleyicisini sınıflarında geçersiz kılınabilir yöntemi. Geliştirici, bu nitelikler değiştiğinde nitelikler üzerinde bağımlı kullanıcı Arabirimi öğeleri birini değiştirmek için bu yöntemi kullanabilirsiniz.

## <a name="typical-trait-collections"></a>Tipik ayırdedici nitelik koleksiyonları

Bu bölümde, kullanıcı iOS 8 ile çalışırken yaşar ayırdedici nitelik koleksiyonları tipik türleri ele alınacaktır.

Geliştirici bir iPhone üzerinde görebileceğiniz tipik ayırdedici nitelik koleksiyonu verilmiştir:

|Özellik|Değer|
|--- |--- |
|`HorizontalSizeClass`|Sıkıştır|
|`VerticalSizeClass`|Normal|
|`UserInterfaceIdom`|Telefon|
|`DisplayScale`|2,0|

Ayırdedici nitelik özelliklerinin tümü için değerlerine sahip olarak yukarıdaki kümesi bir tam olarak nitelenmiş ayırdedici nitelik koleksiyonunu temsil eder.

Değerlerinin bazıları eksik bir ayırdedici nitelik koleksiyonu olması mümkündür (hangi Apple olarak başvurduğu *belirtilmemiş*):

|Özellik|Değer|
|--- |--- |
|`HorizontalSizeClass`|Sıkıştır|
|`VerticalSizeClass`|Belirtilmemiş|
|`UserInterfaceIdom`|Belirtilmemiş|
|`DisplayScale`|Belirtilmemiş|

Geliştirici kendi ayırdedici nitelik toplamalarında ayırdedici nitelik ortamı istediğinde, genellikle, ancak onu tam bir koleksiyon yukarıdaki örnekte görüldüğü gibi döndürür.

(Örneğin, bir görünüm veya Görünüm denetleyicisini) ayırdedici nitelik ortamı geçerli görünümü hiyerarşi içinde değilse, geliştirici bir veya daha fazla ayırdedici nitelik özellik için geri belirtilmeyen değerler alabilirsiniz.

Apple'nın gibi sağladığı oluşturma yöntemlerinden birini kullanırsanız, geliştirici de kısmen tam ayırdedici nitelik koleksiyonu almak `UITraitCollection.FromHorizontalSizeClass`, yeni bir koleksiyon oluşturmak için.

Birden çok ayırdedici nitelik koleksiyonlar üzerinde gerçekleştirilen bir işlem bunları birbirine başka bir içerip içermediğini bir ayırdedici nitelik koleksiyon isteyen içerir ile karşılaştırır. Tarafından amacı *kapsama* ikinci koleksiyonda belirtilen tüm ayırdedici nitelik için değeri ilk koleksiyonda değerle tam olarak eşleşmelidir, değil.

Test etmek için iki nitelikler kullanmak `Contains` yöntemi `UITraitCollection` sınanacak ayırdedici nitelik değerinde geçirme.

Geliştirici karşılaştırmaları el ile belirlemek için kodunda gerçekleştirebilirsiniz nasıl düzeni görünüm veya Görünüm denetleyicileri. Ancak, `UIKit` bu yöntem, bazı işlevlerini görünüm Proxy olduğu gibi örneğin sağlamak için dahili olarak kullanır.

## <a name="appearance-proxy"></a>Görünüm Proxy

Görünüm Proxy geliştiricilerin kendi görünümler özelliklerini özelleştirmek izin vermek için iOS önceki sürümlerini tanıtıldı. İOS 8 ayırdedici nitelik koleksiyonları destekleyecek şekilde genişletilmiştir.

Görünüm proxy'leri artık dahil yeni bir yöntemi `AppearanceForTraitCollection`, ayırdedici nitelik içinde geçirilen bir koleksiyon için yeni bir görünüm Proxy getirir. Geliştirici üzerinde gerçekleştiren özelleştirmeler görünüm Proxy yalnızca uygun görünümleri belirtilen ayırdedici nitelik koleksiyona etkili olur.

Geliştirici kısmen belirtilen bir ayırdedici nitelik koleksiyona genellikle geçer `AppearanceForTraitCollection` gibi bir yatay boyutu sınıfı, sıkıştırma, yalnızca belirtilen ve böylece yatay compact olan uygulama herhangi bir görünümde özelleştirme bir yöntem.

## <a name="uiimage"></a>UIImage

Apple ayırdedici nitelik koleksiyonuna eklenen başka bir sınıf `UIImage`. Geçmişte Geliştirici belirtmek zorunda bir @1X ve @2x (örneğin, bir simge) uygulama eklemek için giderek eşlemli grafik varlık sürümü.

iOS 8, görüntüyü birden fazla sürümünü bir ayırdedici nitelik koleksiyona bağlı bir görüntü katalog dahil etmek Geliştirici izin verecek şekilde genişletilmiştir. Örneğin, geliştirici Compact ayırdedici nitelik sınıfı ve herhangi bir koleksiyon için bir tam boyutlu görüntü ile çalışmak için daha küçük bir resim içerebilir.

Görüntülerden birini kullanıldığında içine bir `UIImageView` sınıfı, görüntü görünüm kendi ayırdedici nitelik koleksiyonu için görüntünün doğru sürümünü otomatik olarak görüntülenir. Ayırdedici nitelik ortam (yatay olarak dikey aygıttan kullanıcı değiştirme gibi) değişirse, resim görünümü otomatik olarak yeni ayırdedici nitelik koleksiyon eşlemek ve görüntü olma geçerli sürümü eşleşen boyutunu değiştirmek için yeni görüntü boyutunu seçin görüntülenir.

## <a name="uiimageasset"></a>UIImageAsset

Apple adlı yeni bir sınıf iOS 8 için ekledi `UIImageAsset` Geliştirici görüntü seçimi üzerinde daha fazla denetim sağlamak için.

Bir görüntü varlığı tüm görüntüyü farklı sürümlerini sarmalar ve içinde geçirilen bir ayırdedici nitelik koleksiyon eşleşen belirli bir görüntü için sormak Geliştirici sağlar. Görüntülerini eklenebilir veya bir görüntü varlığından kaldırıldı üzerinde-çalışma sırasında.

Apple'nın görüntü varlıklar hakkında daha fazla bilgi için bkz: [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) belgeleri.

## <a name="combining-trait-collections"></a>Ayırdedici nitelik koleksiyonları birleştirme

İki birlikte yüklenmesini eklemek için bir geliştirici ayırdedici nitelik koleksiyonlar üzerinde gerçekleştirebileceğiniz başka bir işlevi olan sonuç birleşik koleksiyondaki burada bir koleksiyondan belirtilmeyen değerler almıştır ikinci içinde belirtilen değerlere. Bu yapılır kullanarak `FromTraitsFromCollections` yöntemi `UITraitCollection` sınıfı.

Nitelikler hiçbirini ayırdedici nitelik koleksiyonları birinde belirtilmeyen ve başka bir programda belirtilirse, yukarıda belirtildiği gibi belirtilen sürüme değer ayarlanır. Ancak, belirtilen belirli bir değeri birden fazla sürümü varsa, değeri son kullanılan değeri ayırdedici nitelik koleksiyonu olacaktır.

## <a name="adaptive-view-controllers"></a>Uyarlamalı görünüm denetleyicileri

Bu bölümde, iOS görünümü ve görünüm denetleyicileri nitelikler ve boyutu sınıflarını otomatik olarak Geliştirici uygulamalarda daha Uyarlamalı olmasını kavramlarını nasıl benimseyen ayrıntılarını ele alınacaktır.

### <a name="split-view-controller"></a>Bölme görünüm denetleyicisi

En iOS 8 değişti View Controller sınıfları biri `UISplitViewController` sınıfı. Geçmişte, genellikle bir bölme görünüm denetleyicisini Geliştirici uygulama iPad sürümünü kullanmanız ve sonra bunlar uygulama iPhone sürümü için hiyerarşisini görüntüleme tamamen farklı bir sürümünü sağlayın etmesi gerekir.

İOS 8 ' de `UISplitViewController` sınıfı (iPad ve iPhone), her iki platformlarda kullanılabilir iPhone ve iPad için bir görünüm denetleyicisini hiyerarşisi oluşturmak geliştiricinin sağlar.

İPhone yatay olarak olduğunda, bölme görünüm denetleyicisini tıpkı bir iPad cihazında görüntülenmesini zaman olduğu gibi görünümleri yan yana, sunacaktır.

### <a name="overriding-traits"></a>Nitelikler geçersiz kılma

Yatay yönde iPad cihazında bir bölme görünüm denetleyicisini gösteren aşağıdaki grafikte olduğu gibi bir alt kapsayıcıdaki aşağıya doğru üst kapsayıcısından ayırdedici nitelik ortamları basamaklı:

 [![](unified-storyboards-images/cascadingclasses01.png "Bir iPad yatay yönde bölme görünüm denetleyicisinde")](unified-storyboards-images/cascadingclasses01.png#lightbox)

İPad içinde hem bir yatay ve dikey yönler normal bir boyut sınıfı olduğundan, bölme görünüm hem ana hem de ayrıntı görünümleri görüntüler.

Boyutu sınıfı hem yönler compact olduğu bir İphone'da bölme görünüm denetleyicisini yalnızca aşağıda görüldüğü gibi ayrıntı görünümü görüntüler:

 [![](unified-storyboards-images/cascadingclasses02.png "Bölme görünüm denetleyicisini yalnızca ayrıntı görünümü görüntüler")](unified-storyboards-images/cascadingclasses02.png#lightbox)

Burada yatay yönde iPhone hem ana hem de ayrıntı görüntülemek için geliştirici istediği bir uygulamada, geliştirici bölme görünüm denetleyici için bir üst öğe kapsayıcısı Ekle ve ayırdedici nitelik koleksiyonu geçersiz kılma gerekir. Aşağıdaki grafikte görüldüğü gibi:

 [![](unified-storyboards-images/cascadingclasses03.png "Geliştirici bölme görünüm denetleyici için bir üst öğe kapsayıcısı Ekle ve ayırdedici nitelik koleksiyonu geçersiz kıl")](unified-storyboards-images/cascadingclasses03.png#lightbox)

A `UIView` bölme görünüm denetleyicisini üst öğe olarak ayarlanır ve `SetOverrideTraitCollection` yöntemi bölme görünüm denetleyicisini hedefleme ve yeni bir ayırdedici nitelik koleksiyonunda geçirme görünümünde çağrılır. Yeni ayırdedici nitelik koleksiyonu geçersiz kılmaları `HorizontalSizeClass`, ayarı `Regular`, böylece bölme görünüm denetleyicisini yatay yönde iPhone hem ana hem de ayrıntı görünümleri görüntüler.

Unutmayın `VerticalSizeClass` ayarlandı `unspecified`, hangi üst ayırdedici nitelik koleksiyonda eklenecek yeni ayırdedici nitelik koleksiyon verir sonuçta bir `Compact VerticalSizeClass` alt bölme görünüm denetleyicisi için.

### <a name="trait-changes"></a>Ayırdedici nitelik değişiklikleri

Bu bölümde, ayırdedici nitelik ortamı değiştiğinde ayırdedici nitelik koleksiyonları nasıl geçiş sırasında ayrıntılı bir görünüm olur. Örneğin, ne zaman cihaz düşey yerine yatay olarak döndürülür.

 [![](unified-storyboards-images/traittransitions01.png "Yatay ayırdedici nitelik değişiklikleri genel bakış dikey")](unified-storyboards-images/traittransitions01.png#lightbox)

İlk olarak, iOS 8 gerçekleşmesi için geçiş için hazırlamak için bazı Kurulum'un yok. Ardından, sistem durum geçişi canlandırır. Son olarak, iOS 8 temizler geçişi sırasında gereken herhangi bir geçici durum yukarı.

iOS 8 Geliştirici aşağıdaki tabloda görüldüğü gibi ayırdedici nitelik değişikliği katılmak için kullanabileceği birkaç geri aramalar sağlar:

|Aşama|Geri çağırma|Açıklama|
|--- |--- |--- |
|Kurulum|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>Bu yöntem, ayırdedici nitelik koleksiyonu yeni değerini ayarlama önce ayırdedici nitelik değişiklik başında çağrılır.</li><li>Yöntemi, ayırdedici nitelik koleksiyon değeri değiştiğinde, ancak hiçbir animasyon gerçekleşmeden önce çağrılır.</li></ul>|
|Animasyon|`WillTransitionToTraitCollection`|Bu yönteme geçirilen geçiş Düzenleyici sahip bir `AnimateAlongside` birlikte varsayılan animasyonları yürütülecek animasyonları eklemek Geliştirici imkan tanıyan özellik.|
|Temizleme|`WillTransitionToTraitCollection`|Geçiş gerçekleştikten sonra kendi kodu temizleme dahil etmek geliştiricilere yönelik bir yöntem sağlar.|

`WillTransitionToTraitCollection` Yöntemdir ayırdedici nitelik koleksiyonu değişiklikleri birlikte görünüm denetleyicileri animasyon için harika. `WillTransitionToTraitCollection` Yöntemi kullanılabilir yalnızca görünüm denetleyicilerinde ( `UIViewController`) değil, diğer ayırdedici nitelik ortamları gibi `UIViews`.

`TraitCollectionDidChange` İle çalışmak için harikadır `UIView` sınıfı, burada Geliştirici isteyen kullanıcı Arabirimi özellikleri değiştirme gibi güncelleştirin.

### <a name="collapsing-the-split-view-controllers"></a>Bölme görünüm denetleyicileri daraltma

Şimdi Al ne daha ayrıntılı bir bakış bir bölme görünüm denetleyicisini olur şimdi iki sütundan bir sütun görünümüne daraltır. Bu değişikliğin bir parçası olarak gerçekleşmesi gereken iki işlemler vardır:

-  Daraltma gerçekleştikten sonra varsayılan olarak, birincil görünüm denetleyicisini bölme görünüm denetleyicisini görünüm olarak kullanın. Geliştirici bu davranışı geçersiz kılarak geçersiz kılabilirsiniz `GetPrimaryViewControllerForCollapsingSplitViewController` yöntemi `UISplitViewControllerDelegate` ve bunlar daraltılmış olarak görüntülemek istediğiniz herhangi bir görünüm denetleyicisini sağlama.
-  Birincil görünüm Denetleyicisi'nde birleştirilmiş ikincil View Controller sahiptir. Genellikle geliştirici bu adım için herhangi bir eylemde bulunmanız gerekmez; Bölme görünüm denetleyicisini donanım aygıtı temel alarak bu aşama otomatik işlenmesini içerir. Ancak, bu değişikliği ile etkileşim kurmak için geliştirici burada isteyeceksiniz bazı özel durumlar olabilir. Çağırma `CollapseSecondViewController` yöntemi `UISplitViewControllerDelegate` Daralt oluştuğunda Ayrıntılar görünümü yerine görüntülenecek ana görünüm denetleyicinin sağlar.


### <a name="expanding-the-split-view-controller"></a>Bölme görünüm denetleyicisini genişletme

Şimdi Al ne daha ayrıntılı bir bakış bir bölme görünüm denetleyicisini olur artık daraltılmış durumundan genişletilir. Bir kez daha, gerçekleşmesi gereken iki aşama vardır:

-  İlk olarak, yeni birincil View Controller tanımlayın. Varsayılan olarak, bölme görünüm denetleyicisini daraltılmış görünümüyle birincil görünüm denetleyicisinden otomatik olarak kullanır. Yeniden, geliştirici kullanarak bu davranışı geçersiz kılabilirsiniz `GetPrimaryViewControllerForExpandingSplitViewController` yöntemi `UISplitViewControllerDelegate` .
-  Birincil View Controller seçtikten sonra ikincil görünüm denetleyicisini yeniden oluşturulmalıdır. Yeniden, bölme görünüm denetleyicisini donanım aygıtı temel alarak bu aşama otomatik işlenmesini içerir. Geliştirici çağırarak bu davranışı geçersiz kılabilirsiniz `SeparateSecondaryViewController` yöntemi `UISplitViewControllerDelegate` .


Bir bölme görünüm denetleyicisi birincil View Controller genişleterek hem uygulayarak görünümlerini daraltma bölümünde oynadığı `CollapseSecondViewController` ve `SeparateSecondaryViewController` yöntemlerini `UISplitViewControllerDelegate`. `UINavigationController` otomatik olarak anında ve ikincil görünüm denetleyicisini pop bu yöntemlerini uygular.

### <a name="showing-view-controllers"></a>Gösteren görünüm denetleyicileri

Apple iOS 8 yaptı başka bir değişiklik, geliştirici görünüm denetleyicileri gösterir yoludur. Bir yaprak View Controller (örneğin, bir tablo görünümü denetleyici) uygulama vardı ve geliştirici uygulamasına ulaşması için denetleyici hiyerarşisi aracılığıyla geri (örneğin, yanıtta bir hücre kullanıcı dokunma), farklı bir gösterdi geçmişte Gezinti View Controller ve çağrı `PushViewController` yeni görünüm kendisine karşı yöntemi.

Bu gezinti denetleyici ve onu çalıştığı ortam arasında çok sıkı bağ sunulmuştur. İOS 8'de, Apple bu iki yeni yöntem sağlayarak ayrılmış:

-  `ShowViewController` – Kendi ortamına bağlı yeni görünüm denetleyicisini görüntülemek için uyum sağlar. Örneğin, bir `UINavigationController` yalnızca yeni görünüm yığına iter. Bir bölme görünüm denetleyicisi yeni görünüm denetleyicisini yeni birincil görünüm denetleyicisi olarak sol tarafta sunulur. Hiçbir kapsayıcı görünümü denetleyicisi varsa, yeni görünüm kalıcı bir görünüm denetleyicisi olarak görüntülenir.
-  `ShowDetailViewController` – İle benzer bir şekilde çalışır `ShowViewController`, geçirilen yeni görünüm denetleyicisiyle Ayrıntılar görünümü değiştirmek için bir bölme görünüm denetleyicisinde uygulanır ancak. Bölünmüş View Controller (bir iPhone uygulama görülebilir gibi) daraltılmışsa, çağrı yönlendirilecek `ShowViewController` yöntemi ve yeni görünüm birincil View Controller gösterilecek. Yeniden hiçbir kapsayıcı görünümü denetleyicisi varsa, yeni görünüm kalıcı bir görünüm denetleyicisi olarak görüntülenir.


Bu yöntemler yaprak görünüm denetleyicisinde başlatarak iş ve yeni görünüm görüntüsünü işlemek için doğru kapsayıcıda görünüm denetleyicisini bulana kadar görünüm hiyerarşisinde yukarı yol.

Geliştiriciler uygulayabilirsiniz `ShowViewController` ve `ShowDetailViewController` almak için kendi özel görünüm denetleyicileri aynı işlevselliği otomatik, `UINavigationController` ve `UISplitViewController` sağlar.

### <a name="how-it-works"></a>Nasıl çalışır?

Bu bölümde Biz bu yöntemleri iOS 8 gerçekte nasıl uygulandığı bir göz atalım. İlk yeni bakalım `GetTargetForAction` yöntemi:

 [![](unified-storyboards-images/gettargetforaction.png "Yeni GetTargetForAction yöntemi")](unified-storyboards-images/gettargetforaction.png#lightbox)

Bu yöntem, doğru kapsayıcı görünüm denetleyicisini bulunana kadar hiyerarşi zincirini anlatılmaktadır. Örneğin:

1.  Varsa bir `ShowViewController` yeni görünüm üst öğe olarak kullanılmak üzere Gezinti denetleyicisi yöntemi çağrıldığında, bu yöntem uygular zincirde ilk görünüm denetleyicisini.
1.  Varsa bir `ShowDetailViewController` yöntemi yerine çağrıldı, üst öğe olarak kullanılmak üzere uygulamak için ilk görünüm denetleyicisini bölme görünüm denetleyicisidir.


`GetTargetForAction` Yöntemi, belirli bir eylemi uygulayan bir görünüm denetleyicisi bulunurken ve ardından isteyen çalışır, görünüm bu eylemi almak istiyorsa, denetleyici. Bu yöntem ortak olduğundan, geliştiriciler işlevi gibi yerleşik içinde kendi özel yöntemler oluşturabilir `ShowViewController` ve `ShowDetailViewController` yöntemleri.

## <a name="adaptive-presentation"></a>Uyarlamalı sunu

İOS 8'de, Apple Popover sunuları yaptı ( `UIPopoverPresentationController`) Uyarlamalı de. Böylece Popover sunu View Controller normal Popover görünüme normal bir boyutu sınıfında otomatik olarak sunacaktır ancak yatay compact boyutu sınıfında tam ekran görüntülenir (gibi bir iPhone).

Birleşik film şeridi sistemi içinde değişiklik yapabilmek için yeni bir denetleyici nesnesi sunulan görünüm denetleyicileri yönetmek için oluşturuldu — `UIPresentationController`. Bu denetleyici, kapatılmadan görünüm denetleyicisini sunulan saati oluşturulur. Yöneten bir sınıf olarak hangi olan sonra sat tekrar görünüm denetleyicisini içine sunu denetleyicisi denetler View Controller (örneğin, yönlendirme) etkileyen aygıt değişikliklere yanıt verdiği gibi bir üst sınıf View Controller kabul edilebilir.

Geliştirici ne zaman sunan bir görünüm denetleyicisini kullanarak `PresentViewController` yöntemi, sunu işlemi yönetim karmalayan için `UIKit`. Uıkit işleme (diğerlerinin arasında) doğru denetleyici oluşturulan stili için ne zaman olan tek özel durum ile bir görünüm denetleyicisi, kümesine stilde `UIModalPresentationCustom`. Burada, uygulama kullanmak yerine kendi PresentationController olmasından sağlayabilir `UIKit` denetleyicisi.

### <a name="custom-presentation-styles"></a>Özel sunu stilleri

Bir özel sunu stiliyle geliştiriciler özel bir sunu denetleyicisi kullanmak için seçeneğiniz vardır. Bu özel denetleyicisi görünümünü ve onu için allied görünüm davranışını değiştirmek için kullanılabilir.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Boyutu sınıflar ile çalışma

Bu makalede yer Uyarlamalı fotoğraf Xamarin proje boyutu sınıfları ve Uyarlamalı görünüm denetleyicileri iOS 8 birleşik arabirimi uygulamada kullanarak çalışan bir örnek sağlar.

Uygulamanın kullanıcı Arabiriminde tamamen kodundan IOS Tasarımcısı'nı kullanarak aksine oluşturur ve birleşik film şeridi oluşturma, aynı teknikleri uygulamak çalışırken. Bu makalenin sonraki bölümlerinde boyutu sınıflarının birleşik bir film şeridi ve iOS Tasarımcısı'nda bir Xamarin uygulaması ile nasıl kullanılacağını göstereceğiz.

Şimdi nasıl Uyarlamalı fotoğraf proje boyutu sınıf özellikleri çeşitli iOS 8 Uyarlamalı bir uygulama oluşturmak için uyguladığı bir daha yakından bakalım.

### <a name="adapting-to-trait-environment-changes"></a>Ayırdedici nitelik ortamı değişikliklere uyum sağlamak için

Kullanıcı cihazı yatay olarak dikey döndürdüğünde Uyarlamalı fotoğraflar uygulaması bir iPhone üzerinde çalışırken, bölme görünüm denetleyicisini hem ana hem de Ayrıntılar görünümü görüntülenir:

 [![](unified-storyboards-images/rotation.png "Bölme görünüm denetleyicisini her iki ana görüntülenir ve burada görüldüğü gibi ayrıntıları görüntüleyin")](unified-storyboards-images/rotation.png#lightbox)

Bu geçersiz kılma tarafından gerçekleştirilir `UpdateConstraintsForTraitCollection` görünüm denetleyicisini ve kısıtlamalar ayarlama yöntemine temel değerine göre `VerticalSizeClass`. Örneğin:

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>Geçiş animasyon ekleme

Uyarlamalı uygulama gider fotoğraflardaki bölme görünüm denetleyicisini için genişletilmiş daraltılmış, animasyonları varsayılan animasyonları kılarak eklenen `WillTransitionToTraitCollection` görünüm denetleyicisini yöntemi. Örneğin:

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>Ayırdedici nitelik ortam geçersiz kılma

İPhone cihaz yatay görünümünde olduğunda yukarıda gösterdiği gibi uyarlamalı fotoğraflar uygulaması hem ayrıntılarını görüntülemek için bölme görünüm denetleyicisini ve ana görünüm zorlar.

Bu görünüm denetleyiciye aşağıdaki kodu kullanarak gerçekleştirilmiştir:

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>Genişletme ve daraltma bölme görünüm denetleyicisi

Sonraki Bölme görünüm denetleyicisini genişleyen ve çöken davranışını Xamarin içinde nasıl uygulanmıştır inceleyelim. İçinde `AppDelegate`, bölme görünüm denetleyicisini oluşturduğunuzda, bu değişiklikleri işlemek için kendi temsilci atanan:

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

`SeparateSecondaryViewController` Fotoğraf görüntülenmektedir ve o durumuna bağlı eylemde olmadığını yöntemi testleri. Hiçbir fotoğraf ise ana görünüm denetleyicisini görüntülenmesi için onu gösterildikten ikincil View Controller daraltır.

`CollapseSecondViewController` Yöntemi bölme görünüm denetleyicisini genişletirken, bu nedenle bu görünüme geri daraltır fotoğraflara yığında olup olmadığını görmek için kullanılır.

### <a name="moving-between-view-controllers"></a>Görünüm denetleyicileri arasında taşıma

Ardından, Uyarlamalı fotoğraflar uygulaması görünüm denetleyicileri arasında nasıl taşınır bir bakalım. İçinde `AAPLConversationViewController` sınıfı kullanıcı bir hücre tablosundan seçtiğinde `ShowDetailViewController` yöntemi ayrıntıları görüntülemek için çağrılır:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>Açığa göstergeleri görüntüleme

Uyarlamalı fotoğraf uygulamada açığa göstergeleri gizli veya değişiklikleri ayırdedici nitelik ortamına göre gösterilen olduğu çeşitli yerlerde vardır. Bu, aşağıdaki kod ile ele alınır:

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

Bunlar kullanılarak uygulanan `GetTargetViewControllerForAction` yukarıdaki ayrıntılı olarak ele alınan yöntemi.

Bir tablo görünümü denetleyicisi verileri görüntülerken desteklemediğini push gerçekleşecek ve buna uygun olarak açığa göstergesi gizleyin veya gösterin gerekip gerekmediğini görmek için yukarıdaki uygulanan yöntemleri kullanır:

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>Yeni `ShowDetailTargetDidChangeNotification` türü

Apple boyutu sınıfları ve ayırdedici nitelik ortamlarında bir bölme görünüm denetleyicisi ile çalışmak için yeni bir bildirim türü ekledi `ShowDetailTargetDidChangeNotification`. Bu bildirim, ne zaman denetleyicisi genişletir veya daraltır gibi bir bölme görünüm denetleyicisi için ayrıntılı görünüm hedef değiştiğinde gönderilir.

Uyarlamalı fotoğraflar uygulaması, ayrıntı görünümü denetleyicisi değiştiğinde açığa göstergesi durumunu güncelleştirmek için bu bildirimi kullanır:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

Tüm yolları Bu boyutu sınıfları görmek için Uyarlamalı fotoğraflar uygulaması yakın göz atın, ayırdedici nitelik koleksiyonları ve Uyarlamalı görünüm denetleyicileri kolayca Xamarin.iOS birleşik bir uygulama oluşturmak için kullanılabilir.

## <a name="unified-storyboards"></a>Birleşik film şeritleri

Yeni iOS 8, birleşik film şeritleri, oluşturmak geliştiricinin izin iPhone ve iPad aygıtlarda birden çok boyutu sınıfları hedefleyerek görüntülenebilir film şeridi dosya birleşik. Film şeritleri birleşik kullanarak, geliştirici daha az kullanıcı Arabirimi belirli kod yazar ve oluşturmak ve sürdürmek için yalnızca bir arabirim tasarıma sahiptir.

Film şeritleri birleşik kilit yararları şunlardır:

-  Aynı film şeridi dosya iPhone ve iPad için kullanın.
-  Geriye dönük iOS 6 ve iOS 7 dağıtın.
-  Farklı cihaz, yönler ve işletim sistemi sürümlerinden tüm Xamarin iOS Tasarımcısı içinde düzenini önizlemede.

Bu özellik Mac için Visual Studio'da tam olarak desteklenir

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Boyutu sınıflarını etkinleştirme

Varsayılan olarak, tüm yeni Xamarin.iOS projesi olacak bize boyut sınıfları. Boyutu sınıfları ve Uyarlamalı Segues daha eski bir projeden film şeridi içinde kullanmak için ilk Tasarımcısı iOS içinde Xcode 6 birleşik film şeridi biçiminden dönüştürülmelidir.

Bunu yapmak için iOS Tasarımcısı ve onay dönüştürülecek film şeridi açın **kullanım boyutu sınıfları** onay kutusunu:

 [![](unified-storyboards-images/sizeclass01.png "Kullanım boyutu sınıfları onay kutusu")](unified-storyboards-images/sizeclass01.png#lightbox)

Tasarımcı iOS Geliştirici boyutu sınıflarını kullanmak için film şeridi biçimine dönüştürmek istediği onaylar:

 [![](unified-storyboards-images/sizeclass02.png "Boyutu sınıfları uyarı kullanın")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> Otomatik düzeni düzgün çalışması için boyutu sınıfları ayrıca denetlenmesi gerekir.

### <a name="generic-device-types"></a>Genel cihaz türleri

Film şeridi boyutu sınıflarını kullanmak için dönüştürüldükten sonra onu tasarım yüzeyine görünürler ve **görünüm olarak** cihaz genel olacaktır:

 [![](unified-storyboards-images/sizeclass03.png "Genel cihaz türü olarak görüntüleme")](unified-storyboards-images/sizeclass03.png#lightbox)

Genel cihaz Türü seçildiğinde, tüm görünüm denetleyicileri 600 x 600 kareye boyutlandırılır. Bu kare herhangi genişlik ve yükseklik herhangi boyutunu temsil eder. İOS Tasarımcısı bu modda olduğunda, tüm düzenlemeleri boyutu sınıfları tümüne uygulanır.

Geliştirici tasarım yüzeyi iPhone olarak görüntüleme seçeneği de vardır:

 [![](unified-storyboards-images/sizeclass04.png "Tasarım yüzeyine iPhone görüntüleme")](unified-storyboards-images/sizeclass04.png#lightbox)

Ya da iPad görüntüleme:

 [![](unified-storyboards-images/sizeclass05.png "Tasarım yüzeyine iPad görüntüleme")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>Bir boyut sınıfı seçin

(Yakın görünüm olarak açılır) tasarım yüzeyi üst sol alt köşesindeki boyutu sınıf seçici düğmesi altındadır. Hangi boyutu sınıfları şu anda düzenlenmekte olan seçmek Geliştirici sağlar:

 [![](unified-storyboards-images/sizeclass06.png "Bir boyut sınıfı seçin")](unified-storyboards-images/sizeclass06.png#lightbox)

Seçici boyutu sınıfı seçimi 3 x 3 kılavuz olarak gösterir. Her bir ızgara kareler bir genişlik ve yüksekliğini sınıf bileşimini temsil eder. Merkezi kare (Birleşik film şeridi için varsayılan görünüm) herhangi Width/Any Yükseklik boyutu sınıfı seçer. Bu kare seçildiğinde, geliştirici diğer yapılandırmaları tarafından devralınır varsayılan düzeni düzenliyor.

Kılavuz üst sol alt köşesindeki kare Compact genişliği/Compact Yükseklik boyutu sınıfı temsil eder:

 [![](unified-storyboards-images/sizeclass07.png "Compact genişliği/Compact Yükseklik boyutu sınıfı")](unified-storyboards-images/sizeclass07.png#lightbox)

Bu mod, yatay yönde iPhone karşılık gelir. Kılavuz alt sağ alt köşesindeki kareyi normal genişliği/normal Yükseklik boyutu iPad temsil eden sınıf, temsil eder:

 [![](unified-storyboards-images/sizeclass08.png "Normal genişliği/normal Yükseklik boyutu sınıfı")](unified-storyboards-images/sizeclass08.png#lightbox)

İPhone dikey yönde düzenini düzenlemek için sol alt köşesinde kare seçin. Bu, Compact genişliği/normal Yükseklik boyutu sınıfı temsil eder:

 [![](unified-storyboards-images/sizeclass09.png "Compact genişliği/normal Yükseklik boyutu sınıfı")](unified-storyboards-images/sizeclass09.png#lightbox)

Kareyi seçmek üzere tıklatın ve tasarım yüzeyine yeni seçimle eşleşecek şekilde görünüm denetleyicileri boyutu değişir:

 [![](unified-storyboards-images/sizeclass10.png "Tasarım yüzeyine gösterildiği gibi yeni seçimle eşleşecek şekilde görünüm denetleyicileri boyutunu değiştirir")](unified-storyboards-images/sizeclass10.png#lightbox)

Boyutu sınıfları ve iPhone ve iPad cihazları için Düzen nasıl etkilediklerini üzerinde daha fazla bilgi için bu makalenin boyutu sınıfı bölümüne bakın.

### <a name="adaptive-segue-types"></a>Uyarlamalı ü türleri

Film şeritleri önce Geliştirici kullandı sonra varolan segue türleriyle tanıdık gelecektir **anında**, **kalıcı** ve **Popover**. Birleşik film şeridi dosya boyutu sınıfları etkin olduğunda, aşağıdaki Uyarlamalı ü (yukarıda açıklanan yeni görünüm denetleyicisini API karşılık gelir) türleri kullanılabilir hale getirilir: **Göster** ve **Ayrıntı Göster** .

> [!IMPORTANT]
> Boyutu sınıfları etkin olduğunda, var olan segues yeni türleri dönüştürülmesi.

Bir iOS örneği birleşik film şeridi denetleyicisiyle bölme görünüm asıl görünümünde basit oyun Gezinti menüsü olan kullanan 8 uygulamayı alın. Kullanıcı bir menü düğmesine tıklarsa, seçilen öğenin View Controller bölme görünüm denetleyicisini Ayrıntılar bölümünde bir iPad cihazında çalıştırırken gösterilmesi gerekir. İPhone üzerinde öğesi'nin View Controller Gezinti yığına gönderilir.

Bu etkiyi elde etmek için iOS Tasarımcısı control-düğmeyi tıklatın ve bir satır görüntülenecek görünüm denetleyiciye sürükleyin. Fare düğmesini serbest bırakıldığında seçin `Show Detail` ü türü açılır menüsünden:

 [![](unified-storyboards-images/segue01.png "Ayrıntı Göster ü türü açılır menüden seçin")](unified-storyboards-images/segue01.png#lightbox)

Yeni segue düğmesi ve görünüm denetleyicisini arasında oluşturulur. Şimdi uygulamayı iPhone benzeticisi çalıştırın ve ana menü görüntülenir:

 [![](unified-storyboards-images/segue02.png "Ana menü")](unified-storyboards-images/segue02.png#lightbox)

Tıklayın **seçin oyun** düğmesi ve öğenin View Controller gönderilen Gezinti yığına:

 [![](unified-storyboards-images/segue03.png "Görünüm denetleyicisini öğeleri gezinti yığına gösterildiği gibi gönderilir")](unified-storyboards-images/segue03.png#lightbox)

İPhone benzeticisi durdurun ve iPad Simulator uygulamayı çalıştırın. Yatay yönlendirme ile ana geçiş menüsü yeniden görüntülenir:

 [![](unified-storyboards-images/segue04.png "Görüntülenen ana menü")](unified-storyboards-images/segue04.png#lightbox)

Yeniden tıklayın **seçin oyun** düğmesi ve öğenin View Controller bölme görünüm denetleyicisini Ayrıntılar bölümünde gösterilir:

 [![](unified-storyboards-images/segue05.png "Görünüm bölme görünüm denetleyicisini Ayrıntılar bölümünde gösterilen denetleyicisi öğeleri")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>Bir boyutu sınıftan bir öğe dışlama

(Örneğin, bir görünüm, denetim veya kısıtlama) belirli bir öğenin içinde belirli bir boyut sınıfı gerekli olmadığında zamanlar vardır. Bir öğenin bir boyutu sınıftan dışlamak için dışarıda bırakılacak istediğiniz öğeyi seçin **tasarım yüzeyi**. Listenin sonuna kaydırın **özelliği Explorer** tıklatıp **dişli** açılır menüsünde. Birleşimi seçin **genişliği** ve **yükseklik** öğesinden dışlamak için:

[![](unified-storyboards-images/exclude-a.png "Genişliği ve yüksekliği birleşimi seçin")](unified-storyboards-images/exclude-a.png#lightbox)

Yeni bir *dışlama durumda* alt öğe eklenecek **özelliği Explorer**. Ardından, işaretini **yüklü** verilen boyutu sınıf onay kutusunu:

[![](unified-storyboards-images/exclude-b.png "Yüklü onay kutusunun işaretini kaldırın")](unified-storyboards-images/exclude-b.png#lightbox)

Öğeyi hariç tutuldu yükseklik ve genişlik tasarım yüzeyine geçin, verilen boyutu sınıf ancak olmayan tüm kullanıcı Arabirimi tasarımı kaldırıldı:

 [![](unified-storyboards-images/exclude02.png "Öğeyi hariç tutuldu yükseklik ve genişlik tasarım yüzeyine geçin")](unified-storyboards-images/exclude02.png#lightbox)

Any Width/Any Yükseklik boyutu sınıfı ve öğe geçiş hala yerinde şöyledir:

 [![](unified-storyboards-images/exclude03.png "Geçişi geri herhangi Width/Any Yükseklik boyutu sınıfı")](unified-storyboards-images/exclude03.png#lightbox)

Uygulamayı iPad Simulator çalıştırdığınızda öğesi görünür:

 [![](unified-storyboards-images/exclude04.png "Ne zaman gösterilen öğesinin Simulator iPad çalışan uygulamaya")](unified-storyboards-images/exclude04.png#lightbox)

Ve uygulamayı iPhone benzeticisi üzerinde çalıştırdığınızda öğesi eksik:

 [![](unified-storyboards-images/exclude05.png "Öğesinin ne zaman eksik iPhone benzeticisi çalışan uygulamaya")](unified-storyboards-images/exclude05.png#lightbox)

Bir dışlama durumu bir öğeyi kaldırmak için öğeyi seçmeniz yeterlidir **tasarım yüzeyi**, listenin sonuna kaydırın **özelliği Explorer** tıklatıp  **-** kaldırmak için durum yanındaki düğmesini.

Film şeritleri birleşik uygulaması görmek için bakmak `UnifiedStoryboard` örnek Xamarin iOS 8 uygulama bu belgeye eklenmiş.

## <a name="dynamic-launch-screens"></a>Dinamik başlatma ekranlar

Uygulamayı gerçekten başlayarak yukarı kullanıcı için geri bildirim sağlamak için bir iOS uygulaması başlatma sırasında başlatma ekranı dosya bir giriş ekranı görüntülenir. İOS 8 önce birden çok içerecek şekilde Geliştirici olurdu `Default.png` görüntü varlıkları için uygulamanın çalışıyor olması her aygıt türü, Yönlendirme ve ekran çözünürlüğü. Örneğin, `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`vb.

Yeni iPhone 6 ve iPhone 6 Finansman aygıtlar (ve yaklaşan Apple Watch) olan tüm mevcut iPhone ve iPad cihazları, bu farklı boyutlarda, yönler ve çözümler, uzun bir diziye temsil eden `Default.png` gerekir başlangıç ekran görüntüsü varlıklar oluşturulur ve saklanır. Ayrıca, bu dosyalar oldukça büyük olabilir ve "iTunes App Store (büyük olasılıkla bu bir cep telefonu şebekesi üzerinden teslim yapamamasına tutma) uygulamayı indirmek için gereken süre miktarını artırmayı teslim edilebilir uygulama paket Şişir" ve son kullanıcının cihazda gerekli depolama miktarını artırma.

Yeni iOS 8, tek bir atomik Geliştirici oluşturabilirsiniz `.xib` otomatik düzeni ve boyutu sınıfları oluşturmak için kullandığı Xcode dosyasında bir *dinamik başlatma ekranı* her aygıt, çözümleme ve yönlendirme için çalışır. Bu yalnızca oluşturmak ve tüm gerekli görüntü varlıklarını korumak için geliştiricilerin yapması gereken iş miktarını azaltır, ancak uygulamanın yüklü paket boyutu büyük ölçüde azaltır.


Dinamik başlatma ekranlar aşağıdaki sınırlamalar ve ilgili önemli noktalar vardır:

 - Yalnızca `UIKit` sınıfları.
 - Bir tek bir kök görünüm kullanmak bir `UIView` veya `UIViewController` nesnesi.
 - Uygulama kodunun bağlantılarına yapmayın (eklemeyin **Eylemler** veya **çıkışlar**).
 - Eklemeyin `UIWebView` nesneleri.
 - Tüm özel sınıflar kullanmayın.
 - Çalışma zamanı öznitelikleri kullanmayın.

Aklınızda yukarıdaki yönergeleri ile varolan bir Xamarin iOS 8 projesine dinamik başlatma ekran ekleme konumundaki bakalım.

Aşağıdakileri yapın:

1. Açık **Mac için Visual Studio** ve yük **çözüm** dinamik Başlat ekranına eklemek için.
2. İçinde **Çözüm Gezgini**, sağ `MainStoryboard.storyboard` dosya ve seçin **birlikte Aç** > **Xcode arabirimi Oluşturucu**:

    [![](unified-storyboards-images/dls01.png "Xcode arabirimi Oluşturucu ile Aç")](unified-storyboards-images/dls01.png#lightbox)
3. Xcode'da, seçin **dosya** > **yeni** > **dosya...** :

    [![](unified-storyboards-images/dls02.png "Dosya seçin / yeni")](unified-storyboards-images/dls02.png#lightbox)
4. Seçin **iOS** > **kullanıcı arabirimi** > **başlatma ekranı** tıklatıp **sonraki** düğmesi:

    [![](unified-storyboards-images/dls03.png "İOS seçin / kullanıcı arabirimi / başlatma ekranı")](unified-storyboards-images/dls03.png#lightbox)
5. Dosya adı `LaunchScreen.xib` tıklatıp **oluşturma** düğmesi:

    [![](unified-storyboards-images/dls04.png "LaunchScreen.xib dosya adı")](unified-storyboards-images/dls04.png#lightbox)
6. Grafik öğelerine ekleyerek ve belirli cihazlar, yönler ve ekran boyutlarına konumlandırmak için Düzen kısıtlamaları kullanarak başlatma ekranı tasarımını düzenleyin:

    [![](unified-storyboards-images/dls05.png "Başlatma ekranının tasarım düzenleme")](unified-storyboards-images/dls05.png#lightbox)
7. Değişiklikleri kaydetmek `LaunchScreen.xib`.
8. Seçin **uygulamalar hedef** ve **genel** sekmesi:

    [![](unified-storyboards-images/dls06.png "Uygulamaların hedef ve Genel sekmesini seçin")](unified-storyboards-images/dls06.png#lightbox)
9. Tıklatın **seçin Info.plist** düğmesini seçin `Info.plist` tıklatın ve Xamarin uygulaması için **Seç** düğmesi:

    [![](unified-storyboards-images/dls07.png "Xamarin uygulaması Info.plist seçin")](unified-storyboards-images/dls07.png#lightbox)
10. İçinde **uygulama simgeleri ve başlatma görüntüleri** bölümü, açık **başlatma ekranı dosya** açılır ve `LaunchScreen.xib` yukarıda oluşturduğunuz:

    [![](unified-storyboards-images/dls08.png "LaunchScreen.xib seçin")](unified-storyboards-images/dls08.png#lightbox)
11. Değişiklikleri dosyaya kaydedin ve Mac için Visual Studio'ya geri dönün
12. Visual Studio Mac Xcode değişikliklerle eşitlemeyi tamamlanması için bekleyin.
13. İçinde **Çözüm Gezgini**, sağ tıklayın **kaynak** klasörü ve select **Ekle** > **dosyaları Ekle...** :

    [![](unified-storyboards-images/dls09.png "Seç, Ekle / dosyaları Ekle...")](unified-storyboards-images/dls09.png#lightbox)
14. Seçin `LaunchScreen.xib` yukarıda oluşturulan dosya ve tıklatın **açık** düğmesi:

    [![](unified-storyboards-images/dls10.png "LaunchScreen.xib dosyasını seçin")](unified-storyboards-images/dls10.png#lightbox)
15. Uygulamayı oluşturun.

### <a name="testing-the-dynamic-launch-screen"></a>Dinamik başlatma ekranı test etme

Mac için Visual Studio, iPhone 4 Retina simulator seçin ve uygulamayı çalıştırın. Dinamik başlatma ekranı doğru biçimde ve yönlendirmesini görüntülenir:

[![](unified-storyboards-images/dls11.png "Dinamik Başlat dikey yönde görüntülenen ekran")](unified-storyboards-images/dls11.png#lightbox)

Mac için Visual Studio uygulamayı durdurun ve iPad iOS 8 cihazı seçin. Uygulamayı çalıştırın ve başlatma ekranı düzgün bu cihaz ve yönlendirme için Biçimlendirilecek:

[![](unified-storyboards-images/dls12.png "Dinamik başlatma yatay yönde görüntülenen ekran")](unified-storyboards-images/dls12.png#lightbox)

Mac için Visual Studio'ya geri dönün ve uygulamanın çalışmasını durdurun.

### <a name="working-with-ios-7"></a>İOS 7 ile çalışma

İOS 7 ile geriye dönük uyumluluğu korumak için yalnızca olarak normal dahil `Default.png` görüntü iOS 8 uygulamada normal olarak varlıklar. iOS önceki davranışa geri dönmek ve bu dosyaları başlangıç ekranında bir iOS 7 cihazının çalıştırırken kullanın.

Bir dinamik Başlat ekranında Xamarin uygulaması görmek için bakmak [dinamik başlatma ekranlar](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/) örnek iOS 8 uygulama bu belgeye eklenmiş.

## <a name="summary"></a>Özet

Bu makalede boyutu sınıfları ve iPhone ve iPad cihazları düzeninde nasıl etkilediklerini hızlı bir bakış sürdü. Bu nitelikler, ayırdedici nitelik ortamları ve ayırdedici nitelik koleksiyonları birleşik arabirimleri oluşturmak için boyutu sınıflarıyla nasıl ele. Uyarlamalı görünüm denetleyicileri kısa bakın ve birleşik arabirimler içinde boyutu sınıflarla nasıl çalıştığını sürdü. Bu boyut sınıflar ve arabirimler birleşik tamamen C# kodundan bir Xamarin iOS 8 uygulaması içinde uygulanmasına arama.

Son olarak, Xamarin iOS iOS cihazlarda çalışır Tasarımcısı ile birleştirilmiş film şeritleri oluşturma temelleri bu makalede ele ve tek bir dinamik başlatma ekranı oluşturma her bir iOS 8 aygıtı başlangıç ekranında olarak görüntülenir.

## <a name="related-links"></a>İlgili bağlantılar

- [Uyarlamalı fotoğraf (örnek)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [StoryboardIntro örnek](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [Dinamik başlatma ekranlar (örnek)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [iOS 8’e Giriş](~/ios/platform/introduction-to-ios8.md)
- [İOS8 - dinamik düzenleri gelişmesi 2014 (video)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
