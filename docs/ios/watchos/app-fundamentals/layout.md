---
title: Düzen ile çalışma
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 38e26544a907ffcafdec38e3e2758d9bdac7b977
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-layout"></a>Düzen ile çalışma

Düzenleri için Apple Watch tasarlama [ekran boyutları](~/ios/watchos/app-fundamentals/screen-sizes.md) benzersiz zorluklar gösterir.

## <a name="design-tips"></a>Tasarım ipuçları

Anahtar noktasıdır: Kullanıcı Arabiriminizin okunabilir ve kullanışlı bir küçük izleme ekranında, büyük bir parmak ile yapın. Tasarlama için yakalama içine kalan yok **iOS simülatörü** (görünür çok büyük) ve bir **fare işaretçisini** (çalıştığı küçük dokunma hedefler)!

- Siyah bir arka plan kullanın - İzleme'nin siyah kasa ile daha büyük bir ekrana atmosferini oluşturur.

- Ekran düzeninizi paneli değil - doğal visual doldurma çerçeve oluşturur.

- Okunabilirlik odaklanın. Yazı tipleri ve renkler metin okunabilir olduğundan emin olmak için dikkatli kullanın. Otomatik dinamik tür destek almak için yerleşik metin stilleri kullanın.

![](layout-images/type.png "Dinamik tür destek örneği")

- Dokunma hedef boyutlarına odaklanın. Metin etiketleri düğmeleri/tappable tablo satırları tam ekran span. Apple "hiçbir zaman en fazla üç öğe yan yana yerleştirin" ve simgeler ve metin etiketleri kullanıyorsanız söyler.

- Kullanım [ `Menu` denetim](~/ios/watchos/user-interface/menu.md) uygulama tasarımınızı kısa ve öz tutmak için daha az sık kullanılan sunmaya işlevi için.


## <a name="implementation"></a>Uygulama

Gözcü Seti çekici izleme uygulama düzenleri oluşturmanıza yardımcı olmak için aşağıdaki denetimleri içerir:

### <a name="interface-controller"></a>Arabirim Denetleyicisi

`WKInterfaceController` Taban sınıf, planda tümünün değil.

Arabirim Denetleyicisi tasarım yüzeyi dikey gibi davranır **grup**: diğer denetimleri arabirimi denetleyicide sürükleyin ve diğer otomatik olarak düzenlenir genişletme yukarıdakine olacaktır:

![](layout-images/controller-scene.png "Diğer otomatik olarak düzenlenir genişletme yukarıdakine denetimleridir")

Ayarlayabileceğiniz **konumu** ve **boyutu** görünümlerini denetlemek için her denetim özellikleri:

![](layout-images/positionsize-attributes.png "Her denetim konum ve boyut özelliklerini ayarlama")

Boyutu ayarlandığında **kapsayıcısı göreli** orantılı bir değer ve bir uzaklık ayarlamasını sağlayabilirsiniz. Bu ekran izleme ekran genişliği % 80'i kullanacak şekilde ayarlanmış bir düğme gösterir (**0,8**):

![](layout-images/button-attributes.png "Orantılı bir değer ve bir uzaklık ayarlamasını sağlar")


### <a name="group"></a>Grup

`WKInterfaceGroup` dikey veya yatay olarak yığmak için yapılandırılmış bir basit düzen kapsayıcı denetimleri olur. Varsayılan olarak her denetim arasında boşluk içerir, ancak içinde aralığı (ve iç metinleri) değiştirebilirsiniz **öznitelikleri** denetçisi.

![](layout-images/group-attributes.png "Aralık ve iç metinleri öznitelikleri Denetçisi'nde değiştirme")

Gruplar kendilerine boyuta sahip ve denetimleri etrafında göre konumlandırılmış ve grupları karmaşık düzenleri oluşturmak için iç içe olabilir.

![](layout-images/group-scene.png "Grupları karmaşık düzenleri oluşturmak için iç içe olabilir")


### <a name="separator"></a>Ayırıcı

Ayırıcı denetim düzeninizi visual kılavuzunda sağlanmasına yardımcı olmak amacıyla tasarlanmıştır. Ayırıcılar (veya arka plan renklerini veya görüntü) hangi içerik ekranınızda ilgili anlamasına yardımcı olmak için kullanın.

![](layout-images/separator-scene.png "Ayırıcı kullanım örneği")

Tam ekran genişliği kullanmayan mavi ve yeşil ayırıcılar yapılandırıldı biriyle Not **sabit** veya **kapsayıcısı göreli** boyutları.

### <a name="content-controls"></a>İçerik Denetimleri

Hiçbir düzeni olmadan tam `Label`, `Image`, `Button`, `Switch`, `Slider`, `Map`, ve [diğer denetimleri](~/ios/watchos/user-interface/index.md).
Bu düzenleri kullanarak yerleştirilebilir **grupları** veya her bir denetimin konumunu ve boyutunu ayarlar.



## <a name="related-links"></a>İlgili bağlantılar

- [WatchKitCatalog (örnek)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Apple'nın düzeni başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Apple'nın renk & tipografi başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
