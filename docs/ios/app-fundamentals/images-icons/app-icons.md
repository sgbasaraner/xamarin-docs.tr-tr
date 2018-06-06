---
title: Xamarin.iOS uygulaması simgeleri
description: 'Bu belge, çeşitli uygulama simgeleri Xamarin.iOS çalışmak açıklar: uygulama simgesi, Spotlight simgeler, ayarları simgeleri ve iTunes resmi.'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: 3c07f2573aa8ac6e28b2cd6bff56a773e6206aea
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784003"
---
# <a name="application-icons-in-xamarinios"></a>Xamarin.iOS uygulaması simgeleri

Aşağıdaki konularda ayrıntılı olarak ele alınacaktır:

* [Uygulama, Spotlight ve ayarları simgeler](#icon-types) -gerekli bir iOS uygulaması için simgeler farklı türde.
* [Varlık kataloglar simgelerle yönetme](#managing) - varlık kataloglar kullanarak yönetme uygulama simgeleri.
* [iTunes resmi](#itunes) -uygulamanızı gerçekleştirirken geçici yöntemi için gereken iTunes resmi sağlama.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Uygulama, Spotlight ve ayarları simgeler

Bir Xamarin.iOS uygulaması görüntü varlıklarının UI denetimleri ve belge simgeler olarak kullanabileceğiniz aynı şekilde görüntü varlıklarının uygulama simgeleri sağlamak için kullanılabilir. Aşağıdaki ekran görüntüleri iPad gelen iOS simgeleri üç kullanımları gösterilmektedir:

- **Uygulama simgesi** -tüm iOS uygulamalarını uygulama simgesi tanımlamanız gerekir. Bu kullanıcı dokunun simgedir ekranından uygulamayı başlatmak için IOS home. Ayrıca, bu simge varsa oyun Merkezi tarafından kullanılır. Örnek: 

    [![](app-icons-images/000.png "Uygulama simgesi")](app-icons-images/000-full.png#lightbox)
- **Sahne Işığı simgesi** - kullanıcı bir uygulamanın adı bir Spotlight arama girer zaman bu simgeyi görüntülenir. Örnek: 

    [![](app-icons-images/000a.png "Spotlight simgesi")](app-icons-images/000a-full.png#lightbox)
- **Ayarlar simgesine** - kullanıcı girerse **ayarları** uygulama iOS cihazında, bu simgeyi sonunda görüntülenir **ayarları** uygulama için liste. Örnek: 

    [![](app-icons-images/000b.png "Ayarlar simgesi")](app-icons-images/000b-full.png#lightbox)

Aşağıdaki görüntü varlık boyutları ve çözümlemeleri tüm iOS 9 (veya daha büyük) aracılığıyla iOS 5 hedefleyen bir Xamarin.iOS uygulaması gerektirdiği simgesi türlerini desteklemek için gereklidir:

### <a name="iphone-icon-sizes"></a>iPhone simgesi boyutları

- **iPhone: iOS 9 ve 10 (iPhone 6 ve 7 artı)**

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

 1. Mac ve Xcode için her iki Visual Studio artık desteklemek için iOS 7 1 x görüntü ayarlama.
 2. İOS 7 için 1 x görüntü ayarlama varlık kataloglar kullanılırken desteklenmiyor.
 3. iOS 7 ve 8, iOS 5 ve 6 aynı görüntü boyutları kullanın.
 4. Aynı görüntüler ve boyutları Spotlight simgesi olarak kullanır.
 5. Aynı boyut simgeleri iPhone kullanır.
 6. Yalnızca varlık Kataloğu görüntü kümesi ile desteklenir.
 
 Apple'nın simgeleri hakkında daha fazla bilgi için lütfen bkz [simgesi ve görüntü boyutları](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) belgeleri.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Varlık kataloglar simgelerle yönetme

Simgeler, özel bir için `AppIcons` görüntü kümesi eklenebilir `Assets.xcassets` uygulamanın proje dosyasında. Görüntünün tüm çözümler desteklemek için gereken tüm sürümü dahil edilmiştir _xcasset_ ve birlikte gruplandırılır. Mac için Visual Studio'da özel bir düzenleyici içerir ve bu görüntüleri grafik Kurulumu Geliştirici sağlar.

Bir varlık Kataloğu'nu kullanmak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. Ekranı aşağı kaydırarak **uygulama simgeleri** bölümü.
3. Gelen **kaynak** açılır listesinde, olun **AppIcon** seçilir: 

    ![](app-icons-images/migrate01.png "AppIcons seçili olduğundan emin olun")
4. Gelen **Çözüm Gezgini**, çift `Assets.xcassets` dosyayı düzenlemek için açın: 

    ![](app-icons-images/asset01.png "Çözüm Gezgini'nde Assets.xcassets dosyası")
5. Seçin `AppIcons` görüntülenecek varlıklar listesinden `Icon Editor`: 

    ![](app-icons-images/asset02.png "AppIcons Düzenleyicisi")
6. Ya da simge türü tıklayın ve bir görüntü dosyası gerekli türü/boyutu veya bir klasörden görüntüde sürükleyin ve istenen boyutu bırakın.
7. Tıklatın **açık** xcasset ayarlamak ve görüntünün projeye dahil etmek için düğmesi.
8. Gerekli tüm görüntüleri için yineleyin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çift **Info.plist** dosyasını **Çözüm Gezgini**:

    ![](app-icons-images/icon01w.png "Info.plist seçin")
2. Tıklayın **görsel varlıklar** sekmesinde ve tıklayın **kullanım varlık Kataloğu** altında düğmesini **uygulama simgeleri**: 

    ![](app-icons-images/icon02w.png "Görsel varlıklar sekmesini seçin")
4. Gelen **Çözüm Gezgini**, genişletin **varlık Kataloğu** klasörü: 

    ![](app-icons-images/image009.png "Varlık Kataloğu klasörünü genişletin")
5. Çift **medya** dosyayı düzenleyicide açın: 

    ![](app-icons-images/image010.png "Medya dosyası düzenleyicide açın")
6. Altında **özellikleri Explorer** Geliştirici farklı bağlantı türleri ve gereken simgelerin boyutları seçebilirsiniz.
7. Simge türü tıklayın ve bir görüntü dosyası gerekli türü/boyutu seçin.
8. Tıklatın **açık** xcasset ayarlamak ve görüntünün projeye dahil etmek için düğmesi.
9. Gerekli tüm görüntüleri için yineleyin.

-----

Bu da dahil olmak üzere ve bir uygulama için uygulama, Spotlight ve ayarları simgeler sağlamak için kullanılan görüntü varlıklarını yönetme tercih edilen yöntemdir.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Varlık Kataloğu Info.plist geçirme

Varolan bir Xamarin.iOS uygulaması kullanma `Info.plist` , simgeler yönetmek için dosya, geliştirici, kullanılacak geçebilir, önerilen `AppIcons` görüntü varlığı içinde `Assets.xcassets`.

Aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. Ekranı aşağı kaydırarak **uygulama simgeleri** bölümü.
3. Gelen **kaynak** açılır listesinden, **varlık kataloglar geçiş**: 

    ![](app-icons-images/migrate02.png "Varlık Kataloğu seçin geçirme")
4. Var olan tüm simgeler tanımlanan `Info.plist` dosyası için geçirilecek bir `AppIcons` resmi ayarlama eklenen `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Assets.xcassets AppIcons resmi ayarlama")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. İPhone üzerinde simgeler bölümüne tıklayın: 

    ![](app-icons-images/image007.png "Rhe iPhone simgeler Düzenleyicisi")
3. Ekranı aşağı kaydırarak **simgeleri** bölümü.
4. Gelen **varlık Kataloğu** açılır listesinden, **kullanım varlık kataloglar**.
5. Var olan tüm simgeler tanımlanan `Info.plist` dosyası için geçirilecek bir `Images` eklenen kümesi `Assets.xcassets`.
6. Değişiklikleri kaydetmek `Info.plist` dosya.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes resmi

Uygulama (veya kurumsal kullanıcılar için beta gerçek cihazlarda test etmek için) teslim etmek için geçici yöntemini kullanıyorsanız, geliştirici de 512 x 512 ve iTunes uygulamada temsil etmek için kullanılan bir 1024 x 1024 görüntüsü içermesi gerekir.

Resmin iTunes belirtmek için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. Kaydırma **iTunes resmi** Düzenleyicisi bölümünü: 

    ![](app-icons-images/itunes01.png "İTunes Düzenleyicisi resmi bölümüne gidin")
3. Küçük Resim Düzenleyicisi'nde eksik herhangi bir görüntü için tıklayın, Dosya Aç iletişim kutusundan istenen iTunes resmi görüntü dosyasını seçin ve tıklatın **Tamam** düğmesi.
4. Bu kadar tüm görüntüleri, uygulama için belirtilmiş de adımları yineleyin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.

2. Tıklayın **görsel varlıklar** sekmesinde ve genişletin **iTunes resmi**: 

    ![](app-icons-images/itunes01w.png "İTunes Visual Studio resim düzenleme")
4. Küçük Resim Düzenleyicisi'nde eksik herhangi bir görüntü için tıklayın, Dosya Aç iletişim kutusundan istenen iTunes resmi görüntü dosyasını seçin ve tıklatın **açık** düğmesi.
5. Bu kadar tüm görüntüleri, uygulama için belirtilmiş de adımları yineleyin.

-----

## <a name="related-links"></a>İlgili bağlantılar

- [Görüntüleri (örnek) ile çalışma](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Merhaba, iPhone](~/ios/get-started/hello-ios/index.md)
- [Özel bir simge ve görüntü oluşturma yönergeleri](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
