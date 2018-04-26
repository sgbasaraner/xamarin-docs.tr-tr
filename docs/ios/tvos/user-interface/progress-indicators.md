---
title: İlerleme göstergesi ile çalışma
description: Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde İlerleme göstergesi ile çalışma kapsar.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/25/2018
ms.openlocfilehash: d512dfddb3a6c81767f937272a4ffb1ab1a35372
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="working-with-progress-indicators"></a>İlerleme göstergesi ile çalışma

_Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde İlerleme göstergesi ile çalışma kapsar._

Yeni içerik yüklemek veya uzun işleme işlemi gerçekleştirmek için Xamarin.tvOS uygulamanızı gerektiği zaman zamanlar olabilir. Bu saatlerde etkinliği göstergesini veya uygulama hala çalıştığını bildiren kullanıcı sağlar ve bazı göstergesi çalıştırılan görevle konusunda vermek için bir ilerleme çubuğu sunmalıdır.

![Örnek İlerleme göstergesi](progress-indicators-images/intro01.png "örnek İlerleme göstergesi")

## <a name="about-activity-indicators"></a>Etkinlik göstergeleri hakkında

Bir etkinliği göstergesini dönen dişlisine gösterir ve belirlenmemiş bir uzunlukta bir görevi göstermek için kullanılır. Görev başlar ve görev tamamlandığında kaybolur göstergesi sunulur.

Apple etkinlik göstergeleri ile çalışmak için aşağıdaki önerileri vardır:

- **Mümkün olduğunda, ilerleme çubukları kullanmanız** - kullanıcının hiçbir geri bildirim nasıl aldığını çalıştırılan işlem uzun sürer, bir etkinlik göstergesi verir her zaman kullandığından bir ilerleme çubuğu uzunluğu (örneğin, bir dosyayı indirmek için kaç tane bayt) biliniyorsa.
- **Animasyonlu göstergesi tutmak** -kullanıcılar, gösterilen sırada göstergesi her zaman hale getirmeyi böylece bu sabit etkinliği göstergesini sızması uygulamaya ilişkilidir.
- **İşlenmekte olan görev açıklamak** -yalnızca etkinliği göstergesini tek başına görüntüleme, değil yeterli; bunlar bekleyen işlemi hakkında bilgi sahibi olmak kullanıcı gerekir. Görev açıkça tanımlayan anlamlı bir etiket (genellikle tek, tam bir cümle) içerir.

## <a name="about-progress-bars"></a>İlerleme çubukları hakkında

Durum ve zaman alıcı bir görev uzunluğunu belirtmek rengiyle doldurur bir satır olarak bir ilerleme çubuğu gösterir. İlerleme çubukları, görevleri uzunluğu bilinen veya hesaplanan değer her zaman kullanılmalıdır.

Apple ilerleme çubukları ile çalışmak için aşağıdaki önerileri vardır:

- **İlerleme'doğru şekilde rapor** -ilerleme çubukları bir görevi tamamlamak için gereken süreyi doğru bir temsili her zaman sunmak. Meşgul görünen uygulama yapmak için gereken süre hiçbir zaman çalıştırmasını sağlamak yanlış tanıtılır.
- **Kullanım için iyi tanımlanmış süreleri** -ilerleme çubukları yalnızca gösterme uzun bir iş sürüyor yerleştirin, ancak kullanıcı ve görevin ne kadarının tamamlandıktan göstergesi ve zaman kalan tahmini verin.

## <a name="progress-indicators-and-storyboards"></a>İlerleme göstergesi ve film şeritleri

Bir İlerleme göstergesi Xamarin.tvOS uygulamasında çalışmak için kolay iOS Tasarımcısı kullanarak uygulamanın UI eklemek için yoludur.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
    
1. İçinde **çözüm paneli**, çift **Main.storyboard** dosya ve düzenlemek için açın.

2. Sürükleme bir **etkinliği göstergesini** gelen **araç** ve görünümünde bırak: 

    ![Bir etkinliği göstergesini](progress-indicators-images/activity01.png "bir etkinlik göstergesi")

3. İçinde **pencere öğesi** sekmesinde **özellikleri paneli**, etkinliği göstergesini birkaç özelliklerini aşağıdaki gibi ayarlayabilirsiniz kendi **stili**, **davranışı**, ve **adı**: 

    ![Bir etkinliği göstergesini pencere öğesi sekmesini](progress-indicators-images/activity02.png "bir etkinliği göstergesini pencere öğesi sekmesi")
    
    **Adı** C# kodunda etkinliği göstergesini temsil eden özellik adını belirler.

4. Sürükleme bir **ilerleme durumunu görüntüle** gelen **araç** ve görünümünde bırak: 

    ![Bir ilerleme görünümü](progress-indicators-images/activity03.png "ilerleme durumunu görüntüle")

5. İçinde **pencere öğesi** sekmesinde **özelliği Explorer**, gibi çeşitli özellikleri ilerleme görünümünün ayarlayın, **stili**, **ilerleme**(tamamlanma yüzdesi), ve **adı**: 

    ![İlerleyenler görünümünde pencere öğesi sekmesini](progress-indicators-images/activity04.png "ilerleyenler görünümünde pencere öğesi sekmesi")
    
    **Adı** C# kodu ilerleme görünümünde temsil eden özellik adını belirler.

6. Değişikliklerinizi kaydedin.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. İçinde **Çözüm Gezgini**, çift **Main.storyboard** dosya ve düzenlemek için açın.

2. Sürükleme bir **etkinliği göstergesini** gelen **araç** ve görünümünde bırak: 

    ![Bir etkinliği göstergesini](progress-indicators-images/activity01-vs.png
    "bir etkinlik göstergesi")

3. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**, etkinliği göstergesini birkaç özelliklerini aşağıdaki gibi ayarlayabilirsiniz kendi **stili**, **davranışı**, ve **adı**: 

    ![Bir etkinliği göstergesini pencere öğesi sekmesini](progress-indicators-images/activity02-vs.png "bir etkinliği göstergesini pencere öğesi sekmesi")

    **Adı** C# kodunda etkinliği göstergesini temsil eden özellik adını belirler.

4. Sürükleme bir **ilerleme durumunu görüntüle** gelen **araç** ve görünümünde bırak: 

   ![Bir ilerleme görünümü](progress-indicators-images/activity03-vs.png "ilerleme durumunu görüntüle")

5. İçinde **pencere öğesi** sekmesinde **özelliği Explorer**, gibi çeşitli özellikleri ilerleme görünümünün ayarlayın, **stili**, **ilerleme**(tamamlanma yüzdesi), ve **adı**: 

    ![İlerleyenler görünümünde pencere öğesi sekmesini](progress-indicators-images/activity04-vs.png "ilerleyenler görünümünde pencere öğesi sekmesi")
    
    **Adı** C# kodu ilerleme görünümünde temsil eden özellik adını belirler.

6. Değişikliklerinizi kaydedin.

-----

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md). 

## <a name="working-with-activity-indicators"></a>Etkinlik göstergeleri ile çalışma

Yukarıda belirtildiği gibi uzun bir süreç belirsiz uzunluğu uygulamanızı çalıştırırken etkinlik göstergeleri gösterilmesi gerekir.

Herhangi bir noktada bir etkinliği göstergesini denetleyerek animasyon varsa görebilirsiniz, `IsAnimating` özelliği. Varsa `HidesWhenStopped` özelliği `true`, animasyonunun durdurulduğunda etkinliği göstergesini otomatik olarak gizlenir.

Animasyonu başlatmak için aşağıdaki kodu kullanabilirsiniz: 

```csharp
ActivityIndicator.StartAnimating();
```

Ve animasyon aşağıdaki durdurur:

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> Bu kod parçacıkları varsayımında etkinlik göstergenin **adı** ayarlandı **ActivityIndicator** içinde **pencere öğesi** Tasarımcısı iOS sekmesinde.

## <a name="working-with-progress-bars"></a>İlerleme çubukları ile çalışma

Yeniden, bir ilerleme çubuğu, uygulamanızın bilinen Duration uzun süre çalışan bir görev yürütme zaman kullanılmalıdır. 

`Progress` Özelliği 0-% %100 (0.0 ile 1.0) tamamlandı görev miktarını ayarlamak için kullanılır. Kullanım `ProgressTintColor` tamamlandı tutar çubuğunun rengini ayarlamak için özellik ve `TrackTintColor` (tamamlanmamış tutar) arka plan rengini ayarlamak için özellik.

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve Xamarin.tvOS uygulama içinde İlerleme göstergesi ile çalışma kapsamına.

## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
