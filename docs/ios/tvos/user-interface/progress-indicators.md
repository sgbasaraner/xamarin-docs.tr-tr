---
title: "İlerleme göstergesi ile çalışma"
description: "Bu makalede tasarlama ve İlerleme göstergesi ile Xamarin.tvOS uygulama içinde çalışma kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c021550e17cf8206d59102856a11c72000ad06aa
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-progress-indicators"></a>İlerleme göstergesi ile çalışma

_Bu makalede tasarlama ve İlerleme göstergesi ile Xamarin.tvOS uygulama içinde çalışma kapsar._


Yeni içerik yüklemek veya uzun işleme işlemi gerçekleştirmek için Xamarin.tvOS uygulamanızı gerektiği zaman zamanlar olabilir. Bu saatlerde bir etkinliği göstergesini veya ilerleme çubuğunu uygulama hala çalıştığını bildiren kullanıcı sağlar ve bazı göstergesi çalıştırılan görevle konusunda vermek için sunmalıdır.

[![](progress-indicators-images/intro01.png "Örnek İlerleme göstergesi")](progress-indicators-images/intro01.png#lightbox)

<a name="About-Activity-Indicators" />

## <a name="about-activity-indicators"></a>Etkinlik göstergeleri hakkında

Bir etkinliği göstergesini dönen dişlisine görsel olarak gösterir ve belirlenmemiş bir uzunlukta bir görevi göstermek için kullanılır. Görev başlar ve görev tamamlandığında kaybolur göstergesi sunulur.

Apple etkinlik göstergeleri ile çalışmak için aşağıdaki önerileri vardır:

- **Mümkün olduğunda, ilerleme çubukları kullanmanız** - bir etkinliği göstergesini çalıştırılan işlem ne kadar sürer, konusunda hiçbir geri bildirim kullanıcı verdiğinden uzunluğu olduğunu biliyorsanız (örneğin, bir dosyayı indirmek için kaç tane bayt) her zaman bir ilerleme çubuğu kullanın.
- **Gösterge animasyonlu tutmak** -kullanıcılar, gösterilen sırada animasyonlu göstergesi her zaman gerekir böylece bu sabit bir etkinliği göstergesini sızması bir uygulama için ilgili.
- **İşlenmekte olan görev açıklamak** -yalnızca etkinliği göstergesini tek başına görüntüleme, değilse yeterli, bunlar bekletme işlemi hakkında bilgi sahibi olmak kullanıcı gerekir. Görev açıkça tanımlayan anlamlı bir etiket (genellikle tek, tam bir cümle) içerir.

<a name="Summary" />

## <a name="about-progress-bars"></a>İlerleme çubukları hakkında

Durum ve zaman alıcı bir görev uzunluğunu belirtmek rengiyle doldurur bir satır olarak bir ilerleme çubuğu gösterir. İlerleme çubukları, görevleri uzunluğu olduğunu veya hesaplanan değer her zaman kullanılmalıdır.

Apple ilerleme çubukları ile çalışmak için aşağıdaki önerileri vardır:

- **Doğru şekilde rapor ilerleme** -ilerleme çubukları her zaman bir görevi tamamlamak için gereken süreyi doğru bir gösterimi olmalıdır. Meşgul görünen uygulama yapmak için gereken süre hiçbir zaman çalıştırmasını sağlamak yanlış tanıtılır.
- **Kullanım Well-Defined süreleri için** -ilerleme çubuğu yalnızca gösterme uzun bir iş sürüyor yerleştirin, ancak kullanıcı ve görevin ne kadarının tamamlandıktan göstergesi ve zaman kalan tahmini verin.

<a name="Progress-Indicators-and-Storyboards" />

## <a name="progress-indicators-and-storyboards"></a>İlerleme göstergesi ve film şeritleri

Xamarin.tvOS uygulamada İlerleme göstergesi ile çalışmak için en kolay yolu, onları iOS Tasarımcısı kullanarak uygulamanın UI eklemektir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
    
1. İçinde **çözüm paneli**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **etkinliği göstergesini** gelen **araç** ve görünümünde bırak: 

    [![](progress-indicators-images/activity01.png "Bir etkinlik göstergesi")](progress-indicators-images/activity01.png#lightbox)
1. İçinde **pencere öğesi sekmesini** , **özellikleri paneli**, etkinliği göstergesini birkaç özelliklerini aşağıdaki gibi ayarlayabilirsiniz kendi **stili** ve **davranışı**: 

    [![](progress-indicators-images/activity02.png "Pencere öğesi sekmesi ")](progress-indicators-images/activity02.png#lightbox)
1. Sürükleme bir **ilerleme durumunu görüntüle** gelen **araç** ve görünümünde bırak: 

    [![](progress-indicators-images/activity03.png "İlerleme durumunu görüntüle")](progress-indicators-images/activity03.png#lightbox)
1. İçinde **pencere öğesi sekmesini** , **özelliği Explorer**, gibi çeşitli özellikleri ilerleme görünümünün ayarlayın, **stili** ve **ilerleme**(tamamlanma yüzdesi): 

    [![](progress-indicators-images/activity04.png "Pencere öğesi sekmesi")](progress-indicators-images/activity04.png#lightbox)
1. Son olarak, Ata **adları** denetimlere için C# kodunda yanıt vermesi. Örneğin: 

    [![](progress-indicators-images/activity05.png "Bir ad atayın")](progress-indicators-images/activity05.png#lightbox)
1. Değişikliklerinizi kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **etkinliği göstergesini** gelen **araç** ve görünümünde bırak: 

    [![](progress-indicators-images/activity01-vs.png "Bir etkinlik göstergesi")](progress-indicators-images/activity01-vs.png#lightbox)
1. İçinde **pencere öğesi sekmesini** , **özellikleri Explorer**, etkinliği göstergesini birkaç özelliklerini aşağıdaki gibi ayarlayabilirsiniz kendi **stili** ve **davranışı**: 

    [![](progress-indicators-images/activity02-vs.png "Pencere öğesi sekmesi")](progress-indicators-images/activity02-vs.png#lightbox)
1. Sürükleme bir **ilerleme durumunu görüntüle** gelen **araç** ve görünümünde bırak: 

    [![](progress-indicators-images/activity03-vs.png "İlerleme durumunu görüntüle")](progress-indicators-images/activity03-vs.png#lightbox)
1. İçinde **pencere öğesi sekmesini** , **özelliği Explorer**, gibi çeşitli özellikleri ilerleme görünümünün ayarlayın, **stili** ve **ilerleme**(tamamlanma yüzdesi): 

    [![](progress-indicators-images/activity04-vs.png "Pencere öğesi sekmesi")](progress-indicators-images/activity04-vs.png#lightbox)
1. Son olarak, Ata **adları** denetimlere için C# kodunda yanıt vermesi. Örneğin: 

    [![](progress-indicators-images/activity05-vs.png "Bir ad atayın")](progress-indicators-images/activity05-vs.png#lightbox)
1. Değişikliklerinizi kaydedin.

-----

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Activity-Indicators" />

## <a name="working-with-activity-indicators"></a>Etkinlik göstergeleri ile çalışma

Yukarıda belirtildiği gibi etkinlik göstergeleri uygulamanızın uzun bir işlem çalışıyor, ancak görev gerektirir tam süreyi tanımadığınız gösterilmesi gerekir.

Denetleyerek etkinliği göstergesini dönen animasyonunun çalışıp çalışmadığını görebilirsiniz herhangi bir noktada `IsAnimating` özelliği. Varsa `HidesWhenStopped` özelliği `true`, animasyonunun durdurulduğunda etkinliği göstergesini otomatik olarak gizlenir.

Animasyonu başlatmak için aşağıdaki kodu kullanabilirsiniz: 

```csharp
ActivityIndicator.StartAnimating();
```

Ve animasyon aşağıdaki durdurur:

```csharp
ActivityIndicator.StopAnimating();
```

<a name="Working-with-Progress-Bars" />

## <a name="working-with-progress-bars"></a>İlerleme çubukları ile çalışma

Yeniden, bir ilerleme çubuğu, uygulamanızın bilinen Duration uzun süre çalışan bir görev yürütme zaman kullanılmalıdır. 

`Progress` Özelliği 0-% %100 (0.0 ile 1.0) tamamlandı görev miktarını ayarlamak için kullanılır. Kullanım `ProgressTintColor` tamamlandı tutar çubuğunun rengini ayarlamak için özellik ve `TrackTintColor` (tamamlanmamış tutar) arka plan rengini ayarlamak için özellik.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve İlerleme göstergesi ile Xamarin.tvOS uygulama içinde çalışma ele.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
