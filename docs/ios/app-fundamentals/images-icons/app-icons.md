---
title: Xamarin.iOS uygulama simgeleri
description: 'Bu belgede, çeşitli uygulama simgeleri Xamarin.iOS ile nasıl çalışılacağı açıklanmaktadır: uygulama simgesi, Spotlight simgeler, ayarlar simgeleri ve iTunes resmi.'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: cd67c564461721ade6f3eb269b461ddea5e2d2c4
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276008"
---
# <a name="application-icons-in-xamarinios"></a>Xamarin.iOS uygulama simgeleri

Aşağıdaki konular ayrıntılı olarak ele alınacak:

* [Uygulama, yenilik ve ayarlar simgeleri](#icon-types) -farklı tür için bir iOS uygulaması gereken simge.
* [Varlık katalogları simgelerle yönetme](#managing) - kullanarak varlık katalogları yönetme uygulama simgeleri.
* [iTunes resmi](#itunes) -uygulamanızı gerçekleştirirken geçici yöntemi için gerekli iTunes resmi sağlama.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Uygulama, yenilik ve ayarlar simgeleri

Bir Xamarin.iOS uygulaması görüntü varlıkları UI denetimleri ve belge simgeler olarak kullanabileceğiniz aynı şekilde, görüntü varlıkları uygulama simgeleri sağlamak için kullanılabilir. Aşağıdaki ekran görüntüleri bir iPad gelen iOS simgeleri üç kullanımlarını gösterir:

- **Uygulama simgesi** -tüm iOS uygulamalarını uygulama simgesi tanımlamanız gerekir. Bu kullanıcı dokunur simgesi, uygulamayı başlatmak için iOS giriş ekranından. Ayrıca, bu simge varsa Game Center tarafından kullanılır. Örnek: 

    [![](app-icons-images/000.png "Uygulama simgesi")](app-icons-images/000-full.png#lightbox)
- **Spotlight simgesi** - kullanıcı bir Spotlight araması'nda, bir uygulamanın adını girer olduğunda bu simge görüntülenir. Örnek: 

    [![](app-icons-images/000a.png "Spotlight simgesi")](app-icons-images/000a-full.png#lightbox)
- **Ayarlar simgesi** - kullanıcı girerse **ayarları** uygulama iOS cihazında, bu simgeyi sonunda görüntülenir **ayarları** uygulama için liste. Örnek: 

    [![](app-icons-images/000b.png "Ayarlar simgesi")](app-icons-images/000b-full.png#lightbox)

Aşağıdaki görüntü varlık boyutlarını ve çözünürlüklerini tüm iOS 9 (veya üstü) aracılığıyla iOS 5'i hedefleyen bir Xamarin.iOS uygulaması tarafından gerekli olan simge türleri desteklemek için ihtiyacınız:

### <a name="iphone-icon-sizes"></a>iPhone simgesi boyutları

- **iPhone: iOS 9 ve 10 (iPhone 6 ve 7 yanı sıra)**

    ||3 x|
    |---|---|
    |Uygulama simgesi|180x180|
    |Spotlight|120x120|
    |Ayarlar|87 x 87|

- **iPhone: iOS 7 ve 8**

    ||1x|2x|
    |---|---|---|
    |Uygulama simgesi|60x60<sup>1</sup>|120x120|
    |Spotlight|40x40<sup>2</sup>|80x80|
    |Ayarlar|-|-|

- **iPhone: iOS 5 ve 6**

    ||1x|2x|
    |---|---|---|
    |Uygulama simgesi|57x57|114x114|
    |Spotlight|29x29|58x58|
    |Ayarlar|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>iPad simgesi boyutları

- **iPad: iOS 9 ve 10**

    ||2 x (iPad Pro)|
    |---|---|
    |Uygulama simgesi|167x167<sup>6</sup>|
    |Spotlight|120x120<sup>6</sup>|
    |Ayarlar|58x58<sup>5</sup>|

- **iPad: iOS 7 ve 8**

    ||1x|2x|
    |---|---|---|
    |Uygulama simgesi|76x76|152x152|
    |Spotlight|40x40|80x80|
    |Ayarlar|-|-|

- **iPad: iOS 5 ve 6**

    ||1x|2x|
    |---|---|---|
    |Uygulama simgesi|72x72|144x144|
    |Spotlight|50x50|100x100|
    |Ayarlar|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Mac ve Xcode için her iki Visual Studio artık desteklemek için iOS 7, 1 x görüntüsü ayarlama.
 2. 1 x görüntüsü için iOS 7 ayarlama varlık katalogları kullanırken desteklenmiyor.
 3. iOS 7 ve 8, iOS 5 ve 6 aynı görüntü boyutları kullanın.
 4. Aynı görüntüleri ve boyutları Spotlight simgesi olarak kullanır.
 5. Aynı boyut simgeleri iPhone kullanır.
 6. Yalnızca varlık Kataloğu görüntü kümesi ile desteklenir.
 
 Apple'nın simgeler hakkında daha fazla bilgi için bkz [simgesini ve görüntü boyutları](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) belgeleri.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Varlık katalogları simgelerle yönetme

Simgeler, özel bir için `AppIcon` görüntü kümesi eklenebilir `Assets.xcassets` uygulamanın proje dosyasında. Görüntünün tüm çözümleri desteklemek için gereken tüm sürümünü dahil edilecek _xcasset_ ve birlikte gruplandırılır. Özel bir Mac için Visual Studio düzenleyicisinde geliştiricinin içerir ve bu görüntüleri grafik Kurulum sağlar.

Bir varlık kataloğu kullanmak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Info.plist` dosyası **Çözüm Gezgini** düzenlemek üzere açın.
2. Ekranı aşağı kaydırarak **uygulama simgeleri** bölümü.
3. Gelen **kaynak** açılan listesinde, olun **AppIcon** seçilir: 

    ![](app-icons-images/migrate01.png "AppIcon seçildiğinden emin olun")
4. Gelen **Çözüm Gezgini**, çift `Assets.xcassets` dosyayı düzenlemek için açın: 

    ![](app-icons-images/asset01.png "Çözüm Gezgini'nde Assets.xcassets dosyası")
5. Seçin `AppIcon` varlıklar görüntülenecek listesinden `Icon Editor`:

    ![](app-icons-images/asset02.png "AppIcon Düzenleyicisi")
6. Ya da belirtilen simge türü tıklayın ve bir resim dosyası gerekli türü/boyutu seçin veya bir görüntüde bir klasörden sürükleyip, istenen boyutu.
7. Tıklayın **açık** düğmesine görüntü projeye dahil etmek ve xcasset ayarlayın.
8. Gerekli tüm görüntüler için yineleyin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çift **Info.plist** dosyası **Çözüm Gezgini**:

    ![](app-icons-images/icon01w.png "Info.plist seçin")
2. Tıklayarak **görsel varlıklar** sekmesini ve tıklayarak **varlık Kataloğu kullan** düğmesini **uygulama simgeleri**: 

    ![](app-icons-images/icon02w.png "Görsel varlıklar sekmesini seçin")
4. Gelen **Çözüm Gezgini**, genişletme **varlık Kataloğu** klasörü: 

    ![](app-icons-images/image009.png "Varlık Kataloğu klasörü genişletin")
5. Çift **medya** dosyayı düzenleyicide açın: 

    ![](app-icons-images/image010.png "Medya Dosyası Düzenleyicisi'nde açın.")
6. Altında **özellikleri Gezgini** Geliştirici farklı türleri ve boyutları gereken simgelerin seçebilirsiniz.
7. Belirtilen simge türü tıklayın ve bir resim dosyası gerekli türü/boyutu seçin.
8. Tıklayın **açık** düğmesine görüntü projeye dahil etmek ve xcasset ayarlayın.
9. Gerekli tüm görüntüler için yineleyin.

-----

Bu, dahil etme ve uygulama için uygulama, yenilik ve ayarlar simgeleri sağlamak için kullanılacak bir görüntü varlıkları yönetme tercih edilen yöntemdir.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Varlık katalogları Info.plist ' geçiş

Mevcut bir Xamarin.iOS uygulama kullanmak `Info.plist` onun simgeler yönetmek için dosya, geliştirici, kullanılacak geçiş yapın, önerilen `AppIcons` görüntü varlığının içinde `Assets.xcassets`.

Aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Info.plist` dosyası **Çözüm Gezgini** düzenlemek üzere açın.
2. Ekranı aşağı kaydırarak **uygulama simgeleri** bölümü.
3. Gelen **kaynak** açılan listesinden **varlık katalogları geçiş**: 

    ![](app-icons-images/migrate02.png "Varlık katalogları seçin geçirme")
4. Mevcut tüm simgeleri tanımlanan `Info.plist` dosya için geçirilecek bir `AppIcons` resmi ayarlama eklenen `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Assets.xcassets AppIcons görüntü ayarlayın")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çift `Info.plist` dosyası **Çözüm Gezgini** düzenlemek üzere açın.
2. İPhone simgeleri bölümüne tıklayın: 

    ![](app-icons-images/image007.png "Rhe iPhone simgeleri Düzenleyicisi")
3. Ekranı aşağı kaydırarak **simgeler** bölümü.
4. Gelen **varlık Kataloğu** açılan listesinden **kullanım varlık katalogları**.
5. Mevcut tüm simgeleri tanımlanan `Info.plist` dosya için geçirilecek bir `Images` eklenen kümesi `Assets.xcassets`.
6. Değişiklikleri kaydetmek `Info.plist` dosya.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes resmi

Uygulama (veya Kurumsal kullanıcıların gerçek cihazlar üzerinde beta testi) gerçekleştirirken geçici yöntemi kullanırken, geliştirici de 512 x 512 ve iTunes uygulamayı temsil etmek için kullanılacak bir 1024 x 1024 görüntüsünü içermesi gerekir.

İTunes resmi belirtmek için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Info.plist` dosyası **Çözüm Gezgini** düzenlemek üzere açın.
2. Kaydırma **iTunes resmi** Düzenleyicisi bölümüne: 

    ![](app-icons-images/itunes01.png "İTunes Resim Düzenleyicisi bölümünü kaydırma")
3. Küçük Resim Düzenleyicisi'nde eksik herhangi bir görüntü için tıklayın, Dosya Aç iletişim kutusunda istenen iTunes resmi için resim dosyası seçin ve tıklayın **Tamam** düğmesi.
4. Bu kadar tüm görüntüleri uygulama için belirtilmiş adımları yineleyin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çift `Info.plist` dosyası **Çözüm Gezgini** düzenlemek üzere açın.

2. Tıklayarak **görsel varlıklar** genişletin ve sekme **iTunes resmi**: 

    ![](app-icons-images/itunes01w.png "İTunes resmi Visual Studio'da düzenleme")
4. Küçük Resim Düzenleyicisi'nde eksik herhangi bir görüntü için tıklayın, Dosya Aç iletişim kutusunda istenen iTunes resmi için resim dosyası seçin ve tıklayın **açık** düğmesi.
5. Bu kadar tüm görüntüleri uygulama için belirtilmiş adımları yineleyin.

-----

## <a name="related-links"></a>İlgili bağlantılar

- [(Örnek) görüntülerle çalışma](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Merhaba, iPhone](~/ios/get-started/hello-ios/index.md)
- [Özel bir simge ve görüntü oluşturma yönergeleri](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
