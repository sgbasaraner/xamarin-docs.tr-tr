---
title: "Bölme görünüm denetleyicileri ile çalışma"
description: "Bu makalede tasarlama ve bölme görünüm denetleyicileriyle Xamarin.tvOS uygulama içinde çalışma kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 86a7690d4cf7291a4e44507a6250e3469c8f7ed2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-split-view-controllers"></a>Bölme görünüm denetleyicileri ile çalışma

_Bu makalede tasarlama ve bölme görünüm denetleyicileriyle Xamarin.tvOS uygulama içinde çalışma kapsar._


Bir bölme görünüm denetleyicisi sunar ve bir ana ve ayrıntı görünümü denetleyicisi yan yana, aynı anda ekranda yönetir. Bölme görünüm denetleyicileri kalıcı, odaklanabilir içerik ana görünümü (sol küçük bölümüne) sunmak için kullanılan ve ilgili ayrıntıları Ayrıntılar görünümünde (sağdaki büyük bölümü).

[![](split-views-images/intro01.png "Örnek bölünmüş görünümü")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>Bölme görünüm denetleyiciler hakkında

Yukarıda belirtildiği gibi bir bölme görünüm denetleyicisini ana ve yan yana solda, büyük sağdaki ayrıntı daha küçük görünümü olan ana sunulur ayrıntı görünümü denetleyicisi yönetir. 

Ana görünüm denetleyicisini can gizlenmediği ya da gösterilen ek olarak, gerektiği gibi: 

[![](split-views-images/intro02.png "Ana görünüm gizli denetleyicisi")](split-views-images/intro02.png#lightbox)

Bölünmüş görünümler denetleyicileri genellikle kullanacağı ana görünüm kategorileri ve ayrıntı görünümü filtrelenen sonuçları filtrelenebilir içerik listesini sunmak için. Bu genellikle sol taraftaki tablo görünüm olarak sunulan ve bir [koleksiyon görünümü](~/ios/tvos/user-interface/collection-views.md) sağdaki.

Bir bölme görünüm denetleyicisini gerektiren bir kullanıcı arabirimi tasarlarken, Apple ana ve (yalnızca içerik değişiklikler, değil yapısı) değiştirmeyin ayrıntı görünümü denetleyicileri kullanılmasını önerir. Görünüm denetleyicileri takas genişletmek gerekiyorsa (ana veya ayrıntı) değiştirmek için gereken View Controller temel olarak bir gezinti denetleyicisi kullanmak en iyisidir.

Apple bölme görünüm denetleyicileri ile çalışmak için aşağıdaki önerileri vardır:

- **Doğru bölünmüş yüzde kullanmak** -varsayılan olarak bölme görünüm denetleyicisini üçte ana görünüm denetleyicisini ekranında ve iki üç ayrıntı görünümü denetleyici için kullanır. İsteğe bağlı olarak, 50/50 bölme kullanabilirsiniz. Ekranda Dengeli, içerik görünmesi sağlamak için doğru yüzdesini seçin.
- **Ana seçimi kalıcı** - içerik sırasında üzerinde ayrıntılı görünüm can değişiklik kullanıcının seçim ana görünümü yanıta, ana görünüm içerik düzeltilmelidir. Ayrıca, asıl görünümünde seçili öğenin açıkça göstermelidir.
- **Tek bir birleşik başlık kullanmak** -genellikle, ayrıntı görünümünde, ayrıntı ve ana görünüm başlık yerine tek, ortalanmış bir başlık kullanmak istersiniz.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>Bölme görünüm denetleyicileri ve film şeritleri

Bölme görünüm denetleyicileriyle Xamarin.tvOS uygulamada çalışmaya en kolay yolu, bunları iOS Tasarımcısı kullanarak uygulamanın UI eklemektir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. İçinde **çözüm paneli**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **bölme görünüm denetleyicileri** gelen **araç** ve görünümünde bırak: 

    [![](split-views-images/activity01.png "Bir bölme görünüm denetleyicisi")](split-views-images/activity01.png#lightbox)
1. Varsayılan olarak, iOS Tasarımcısı asıl görünümünde bir gezinti denetleyici ve görünüm denetleyici yükler. Yalnızca bu, uygulamanızın gereksinimlerine uygun değilse, bunları silin.
1. Varsayılan ana görünümü kaldırırsanız, yeni bir görünüm denetleyicisi tasarım yüzeyine sürükleyin: 

    [![](split-views-images/activity02.png "Bir görünüm denetleyicisi")](split-views-images/activity02.png#lightbox)
1. Denetim tıklatın ve yeni ana görünüm denetleyiciye bölme görünüm denetleyicisinden sürükleyin. 
1. Seçin **ana** gelen **menü**: 

    [![](split-views-images/activity03.png "Asıl açılır menüden seçin")](split-views-images/activity03.png#lightbox)
1. Ana ve ayrıntı görünümleri içeriğini tasarım: 

    [![](split-views-images/activity04.png "Örnek düzeni")](split-views-images/activity04.png#lightbox)
1. Ata **adları** içinde **pencere öğesi sekmesini** , **özellikleri paneli** UI denetimlerinizi C# kodunda çalışmak için.
1. Yaptığınız değişiklikleri kaydedin ve Mac için Visual Studio'ya geri dönün

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **bölme görünüm denetleyicileri** gelen **araç** ve görünümünde bırak: 

    [![](split-views-images/activity01-vs.png "Bir bölme görünüm denetleyicisi")](split-views-images/activity01-vs.png#lightbox)
1. Varsayılan olarak, iOS Tasarımcısı Gezinti denetleyici ve görünüm denetleyici asıl görünümünde ekleyeceksiniz. Yalnızca bu, uygulamanızın gereksinimlerine uygun değilse, bunları silin.
1. Varsayılan ana görünümü kaldırırsanız, yeni bir görünüm denetleyicisi tasarım yüzeyine sürükleyin: 

    [![](split-views-images/activity02-vs.png "Bir görünüm denetleyicisi")](split-views-images/activity02-vs.png#lightbox)
1. Denetim tıklatın ve yeni ana görünüm denetleyiciye bölme görünüm denetleyicisinden sürükleyin. 
1. Seçin **ana** gelen **menü**: 

    [![](split-views-images/activity03-vs.png "Asıl açılır menüden seçin")](split-views-images/activity03-vs.png#lightbox)
1. Ana ve ayrıntı görünümleri içeriğini tasarım: 

    [![](split-views-images/activity04.png "İçerik düzeni")](split-views-images/activity04.png#lightbox)
1. Ata **adları** içinde **pencere öğesi sekmesini** , **özellikleri Explorer** UI denetimlerinizi C# kodunda çalışmak için.
1. Değişikliklerinizi kaydedin.
    
-----

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md).

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>Bölme görünüm denetleyicileri ile çalışma

Yukarıda belirtildiği gibi bir bölme görünüm denetleyicisi genellikle burada kullanıcıya filtrelenmiş içerik görüntüleme durumlarda kullanılır. Filtrelenen sonuçları Ayrıntılar görünümünde sağdaki kullanıcının seçimine göre ve ana kategoriler sol ana görünümünde görüntülenir.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>Ana ve ayrıntı erişme

Ana ve ayrıntı görünümü denetleyicilerine programlı olarak erişmek gereksinim duyarsanız kullanın `ViewControllers ` bölme görünüm denetleyicisini özelliği. Örneğin:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

İlk öğe ana View Controller (0) ve ikinci öğe (1) ayrıntı olduğu, bir dizi olarak sunulur.

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>Ayrıntı yöneticisinden erişme

Ayrıntılı bilgi genellikle ana kullanıcının seçime göre Ayrıntılar görünümünde görüntülüyorsunuz olduğundan, ayrıntı yöneticisinden erişmek için bir yol gerekir.

Örneğin, ana görünüm denetleyici sınıfı bir özellik kullanıma sunmak için bunu yapmanın en kolay yolu verilmiştir:

```csharp
public DetailViewController DetailController { get; set;}
```

Bölme görünüm denetleyicisini geçersiz kılma `ViewDidLoad` yöntemi ve bağ iki görünümleri birlikte. Örneğin:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Gain access to master and detail view controllers
    var masterController = ViewControllers [0] as MasterViewController;
    var detailController = ViewControllers [1] as DetailViewController;

    // Wire-up views
    masterController.SplitViewController = this;
    masterController.DetailController = detailController;
    detailController.SplitViewController = this;
}
```

Asıl gerektiği gibi yeni verileri sunmak için kullanabilirsiniz, ayrıntı görünümü denetleyicisinde özellikleri ve yöntemleri getirebilir.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>Gösterme ve gizleme Yöneticisi

İsteğe bağlı olarak göster ve ana görünüm denetleyicisini kullanarak Gizle `PreferredDisplayMode` bölme görünüm denetleyicisini özelliği. Örneğin:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

`UISplitViewControllerDisplayMode` Enum tanımlar nasıl ana görünüm denetleyicisini aşağıdakilerden biri olarak sunulur:

- **Otomatik** -tvOS ana ve ayrıntı görünümleri sunumu kontrol.
- **PrimaryHidden** -bu ana görünüm denetleyicisini gizler.
- **AllVisible** -hem ana hem de ayrıntı görünümü denetleyicileri yan yana görüntüler. Normal, varsayılan gösterim budur.
- **PrimaryOverlay** -ayrıntı görünümü denetleyici altında genişletir ve ana öğe tarafından ele alınmıştır.

Geçerli sunu durumunu almak için `DisplayMode` bölme görünüm denetleyicisini özelliği.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve bölme görünüm denetleyicileriyle Xamarin.tvOS uygulama içinde çalışma ele.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
