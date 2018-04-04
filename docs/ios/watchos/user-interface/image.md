---
title: Görüntü denetimi
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 370b9f2a57716de876c7e883afdaf445186fb577
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="image-control"></a>Görüntü denetimi

watchOS sağlayan bir [ `WKInterfaceImage` ](https://developer.xamarin.com/api/type/WatchKit.WKInterfaceImage/) görüntüler ve basit animasyonları görüntülemek için denetimi. Bazı denetimler de (örneğin, düğmeleri, grupları ve arabirim denetleyicilerini) arka plan resmi sağlayabilirsiniz.

![](image-images/image-walkway.png "Apple Watch gösteren resim") ![ ] (image-images/image-animation.png "Apple Watch basit animasyon ile")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

Varlık Kataloğu resimleri görüntüleri izleme Seti uygulamalara eklemek için kullanın.
Yalnızca **@2x** tüm Retina görüntüler sahip cihazları izlemek beri sürümleri gerekli.

![](image-images/asset-universal-sml.png "Tüm Retina görüntüler sahip cihazları izlemek beri yalnızca 2 x sürümlerinin gereklidir")

Görüntüleri kendilerini izleme görüntülenmesi için doğru boyutta olduğundan emin olmak için iyi bir uygulamadır. *Kaçının* yanlış boyutlu görüntüleri (özellikle büyük olanlar için) kullanarak ve saatin görüntülenecek ölçeklendirme.

Her görüntü boyutu için farklı görüntüleri belirtmek için bir varlık Kataloğu görüntüde izleme Seti boyutları (38 mm, 42 mm) kullanın.

![](image-images/asset-watch-sml.png "Her görüntü boyutu için farklı görüntüleri belirtmek için bir varlık Kataloğu görüntüde izleme Seti boyutları 38 mm, 42 mm kullanabilirsiniz")


## <a name="images-on-the-watch"></a>Gözcü görüntülerde

Görüntüleri göstermek için en etkili yoldur *izleme uygulama projeye dahil* ve bunları görüntülemek kullanarak `SetImage(string imageName)` yöntemi.

Örneğin, [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/) örnek sahip izleme uygulama projesi varlık kataloğunda eklenen resimlerinin sayısı:

![](image-images/asset-whale-sml.png "İzleme uygulaması projesi varlık kataloğunda eklenen görüntü sayısını WatchKitCatalog örnek içeriyor")

Bunlar verimli bir şekilde yüklenebilir ve izleme kullanarak görüntülenen `SetImage` dize ad parametresiyle:

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>Arka plan görüntüleri

İçin aynı mantığı uygular `SetBackgroundImage (string imageName)` üzerinde `Button`, `Group`, ve `InterfaceController` sınıfları. En iyi performans izleme uygulama kendisini görüntüleri depolayarak elde edilir.


## <a name="images-in-the-watch-extension"></a>İzleme uzantısı görüntülerde

Gözcü uygulamada kendisini depolanan görüntülerin yüklenmesini, ek olarak görünen izleme uygulamasında uzantısı paketindeki görüntüleri gönderebilirsiniz (veya görüntüleri uzak bir konumdan indirmek ve bu görüntülemek).

Gözcü uzantı görüntüleri yüklemek için oluşturmanız `UIImage` örnekleri ve ardından arama `SetImage` ile `UIImage` nesnesi.

Örneğin, [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) örnek sahip adlı bir resim **Bumblebee** izleme uzantısı projesinde:

![](image-images/asset-bumblebee-sml.png "İzleme uzantısı projesinde Bumblebee adlı bir resim WatchKitCatalog örnek içeriyor")

Aşağıdaki kod neden olur:

- belleğe yüklenen görüntü ve
- saatin görüntülenir.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>Animasyon

Bir görüntü kümesi animasyon eklemek için bunların tümü aynı önek ile başlayan ve bir sayı sonekine sahip gerekir.

[WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) örnek ile izleme uygulama projesinde numaralı görüntüleri bir dizi sahip **veri yolu** öneki:

![](image-images/asset-bus-animation-sml.png "WatchKitCatalog örnek veri yolu önekiyle izleme uygulama projesinde numaralı görüntüleri bir dizi sahiptir.")

Bu görüntüleri bir animasyon olarak görüntülemek için öncelikle görüntü kullanarak yük `SetImage` önek adı ve ardından arama `StartAnimating`:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

Çağrı `StopAnimating` animasyon döngüsü durdurmak için görüntü denetimi:

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>Ek: Görüntüleri (watchOS 1) önbelleğe alma

> [!IMPORTANT]
> watchOS 3 uygulamalar tamamen cihazda çalıştırın. Aşağıdaki bilgiler yalnızca watchOS 1 uygulamaları sağlar.

Uygulama art arda uzantısı'nda depolanır (veya indirildi) bir görüntü kullanıyorsa, resmin sonraki görüntüler için performansı artırmak için izleme'nin depolama önbelleğe mümkündür.

Kullanmak `WKInterfaceDevice`s `AddCachedImage` saate görüntüyü aktarın ve ardından yöntemi `SetImage` görüntülemek için bir dize olarak görüntü adı parametresiyle:

```csharp
var device = WKInterfaceDevice.CurrentDevice;
using (var image = UIImage.FromBundle ("Bumblebee")) {
    if (!device.AddCachedImage (image, "Bumblebee")) {
            Console.WriteLine ("Image cache full.");
        } else {
            cachedImage.SetImage ("Bumblebee");
        }
    }
}
```

Kod kullanarak görüntü önbelleğinin içeriğini sorgulayabilirsiniz `WKInterfaceDevice.CurrentDevice.WeakCachedImages`.


### <a name="managing-the-cache"></a>Önbellek yönetme

Önbellek boyutu yaklaşık 20 MB. Uygulama yeniden başlatmaları arasındaki tutulur ve dolduğunda kullanarak dosyaları temizlemek için sizin sorumluluğunuzdadır olan `RemoveCachedImage` veya `RemoveAllCachedImages` yöntemlere `WKInterfaceDevice.CurrentDevice` nesnesi.



## <a name="related-links"></a>İlgili bağlantılar

- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple'nın görüntü belge](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Images.html)
