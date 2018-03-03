---
title: Uygulama simgeleri
description: "Bu makale, bir uygulama simgesi olarak kullanılacak bir görüntü varlığı bir Xamarin.iOS uygulaması yönetme ve dahil kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: 6de57e9523ff336c2e06e39903280db9c9ab95fa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="application-icons"></a>Uygulama simgeleri

_Bu makale, bir uygulama simgesi olarak kullanılacak bir görüntü varlığı bir Xamarin.iOS uygulaması yönetme ve dahil kapsar._

Aşağıdaki konularda ayrıntılı olarak ele alınacaktır:

* [Uygulama, Spotlight ve ayarları simgeler](#icon-types) -gerekli bir iOS uygulaması için simgeler farklı türde.
* [Varlık kataloglar simgelerle yönetme](#managing) - varlık kataloglar kullanarak yönetme uygulama simgeleri.
* [iTunes resmi](#itunes) -uygulamanızı gerçekleştirirken geçici yöntemi için gereken iTunes resmi sağlama.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Uygulama, Spotlight ve ayarları simgeler

Bir Xamarin.iOS uygulaması görüntü varlıklarının UI denetimleri ve belge simgeler olarak kullanabileceğiniz aynı şekilde görüntü varlıklarının uygulama simgeleri sağlamak için kullanılabilir. Aşağıdaki ekran görüntüleri iPad gelen iOS simgeleri üç kullanımları gösterilmektedir:

- **Uygulama simgesi** -tüm iOS uygulamalarını uygulama simgesi tanımlamanız gerekir. Bu kullanıcı dokunun simgedir ekranından uygulamayı başlatmak için IOS home. Ayrıca, bu simge varsa oyun Merkezi tarafından kullanılır. Örnek: 

    [ ![](app-icons-images/000.png "Uygulama simgesi")](app-icons-images/000-full.png)
- **Sahne Işığı simgesi** - kullanıcı bir uygulamanın adı bir Spotlight arama girer zaman bu simgeyi görüntülenir. Örnek: 

    [ ![](app-icons-images/000a.png "Spotlight simgesi")](app-icons-images/000a-full.png)
- **Ayarlar simgesine** - kullanıcı girerse **ayarları** uygulama iOS cihazında, bu simgeyi sonunda görüntülenir **ayarları** uygulama için liste. Örnek: 

    [ ![](app-icons-images/000b.png "Ayarlar simgesi")](app-icons-images/000b-full.png)

Aşağıdaki görüntü varlık boyutları ve çözümlemeleri tüm iOS 9 (veya daha büyük) aracılığıyla iOS 5 hedefleyen bir Xamarin.iOS uygulaması gerektirdiği simgesi türlerini desteklemek için gereklidir:

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPhone</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 & 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 & 8</b></td>
        <td align="center" bgcolor="#F9F9F9"><b>iOS 9 & 10<b><br/><i>(iPhone 6 & 7 Plus)</i></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>Simge türü</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>3x</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Uygulama simgesi</td>
        <td align="center">57x57</td>
        <td align="center">114x114</td>
        <td align="center" style="color:#BBBBBB;">60x60<sup>(1)</sup></td>
        <td align="center">120x120</td>
        <td align="center">180x180</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Spotlight</td>
        <td align="center">29x29</td>
        <td align="center">58x58</td>
        <td align="center" style="color:#BBBBBB;">40x40<sup>(2)</sup></td>
        <td align="center">80x80</td>
        <td align="center">120x120</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Ayarlar</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(4)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(4)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">87x87</td>
    </tr>
</table>

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPad</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 & 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 & 8</b></td>
        <td colspan="1" align="center" bgcolor="#F9F9F9"><b>iOS&nbsp;9 & 10</b></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>Simge türü</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>2x<br/>iPad&nbsp;Pro</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Uygulama simgesi</td>
        <td align="center">72x72</td>
        <td align="center">144x144</td>
        <td align="center">76x76</td>
        <td align="center">152x152</td>
        <td align="center">167x167<sup>(6)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Spotlight</td>
        <td align="center">50x50</td>
        <td align="center">100x100</td>
        <td align="center">40x40</td>
        <td align="center">80x80</td>
        <td align="center" style="color:#BBBBBB;">120x120<sup>(5)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Ayarlar</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(5)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(5)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(5)</sup></td>
    </tr>
</table>

1. _Mac ve Xcode için her iki Visual Studio artık desteklemek için iOS 7 1 x görüntü ayarlama._
2. _İOS 7 için 1 x görüntü ayarlama varlık kataloglar kullanılırken desteklenmiyor._
3. _iOS 7 ve 8, iOS 5 ve 6 aynı görüntü boyutları kullanın._
4. _Aynı görüntüler ve boyutları Spotlight simgesi olarak kullanır._
5. _Aynı boyut simgeleri iPhone kullanır._
6. _Yalnızca varlık Kataloğu görüntü kümesi ile desteklenir._

Apple'nın simgeleri hakkında daha fazla bilgi için lütfen bkz [simgesi ve görüntü boyutları](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) belgeleri.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Varlık kataloglar simgelerle yönetme

Simgeler, özel bir için `AppIcons` görüntü kümesi eklenebilir `Assets.xcassets` uygulamanın proje dosyasında. Görüntünün tüm çözümler desteklemek için gereken tüm sürümü dahil edilmiştir _xcasset_ ve birlikte gruplandırılır. Mac için Visual Studio'da özel bir düzenleyici içerir ve bu görüntüleri grafik Kurulumu Geliştirici sağlar.

Bir varlık Kataloğu'nu kullanmak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Çift `Info.plist` dosyasını **Çözüm Gezgini** düzenlemek için açın.
2. Ekranı aşağı kaydırarak **uygulama simgeleri** bölümü.
3. Gelen **kaynak** açılır listesinde, olun **AppIcons** seçilir: 

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

    ![](app-icons-images/image007.png "Rhe iPhone Icons editor")
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
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Özel bir simge ve görüntü oluşturma yönergeleri](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
