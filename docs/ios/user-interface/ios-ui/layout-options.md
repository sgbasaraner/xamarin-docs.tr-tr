---
title: Xamarin.iOS düzeni seçenekleri
description: Bu belgede Xamarin.iOS kullanıcı arabirimleri yerleştirme için farklı yollar açıklanmaktadır. Otomatik boyutlandırma ve otomatik yerleşimi açıklanır.
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: bad29eae308c8ca9f7228a1cbdfd69940894cf34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790122"
---
# <a name="layout-options-in-xamarinios"></a>Xamarin.iOS düzeni seçenekleri

Bir görünümü yeniden boyutlandırılmış veya Döndürülmüş düzenini denetlemek için iki farklı mekanizmalar vardır:

-  **Otomatik boyutlandırma** – Tasarımcısı'nda Otomatik boyutlandırma denetçisi ayarlamak için bir yöntem sunar `AutoresizingMask` özellikleri. Bu, kullanıcıların kapsayıcı kenarlarına bağlantılı ve/veya bunların boyutu düzeltme denetim sağlayacaktır. Otomatik boyutlandırma iOS tüm sürümlerinde çalışır. Bu aşağıdaki daha ayrıntılı olarak açıklanır
-  **Düzen otomatik** – UI denetimlerinin ilişkileri üzerinde ayrıntılı denetim iOS 6'de sunulan, bir özellik. Öğe diğer öğeleri göreli konumlarını Denetim tasarım yüzeyine izin verir. Bu konuda daha ayrıntılı olarak ele alınmıştır [Xamarin iOS Tasarımcısı otomatik düzeniyle](~/ios/user-interface/designer/designer-auto-layout.md) Kılavuzu.

## <a name="autosizing"></a>Otomatik boyutlandırma

Bir kullanıcı aygıtı ne zaman döndürülür ve yönlendirme değişiklikleri gibi bir pencere yeniden boyutlandırır olduğunda sistem kendi otomatik boyutlandırma kurallarına göre bu pencereyi içindeki görünümler otomatik olarak yeniden boyutlandırılır. Bu kurallar ayarlanabilir C# kullanarak `AutoresizingMask` özelliği `UIView` veya **özellikleri paneli** Tasarımcısı aşağıda gösterildiği gibi iOS:

 [![](layout-options-images/image41.png "Visual Studio için Mac Tasarımcısı")](layout-options-images/image41.png#lightbox)

Bir Denetim seçildiğinde, bu el ile denetim boyutları ve konumunu belirtmenize olanak tanır seçme yanı sıra **otomatik boyutlandırma** davranışı. Aşağıdaki ekran görüntüsünde gösterildiği gibi biz struts ve yaylar otomatik boyutlandırma denetiminde kaydının üst öğeye seçili görünümün ilişki tanımlamak için kullanabilirsiniz:

 [![](layout-options-images/image42.png "Visual Studio için Mac Tasarımcısı")](layout-options-images/image42.png#lightbox)

Ayarlama bir *yay* yeniden boyutlandırmak görünüm genişliği veya yüksekliği kendi üst görünümü göre neden olur. Ayarlama bir *strut* kendisi ve bu belirli Kenar çubuğunda kendi üst görünümü arasındaki sabit uzaklığı korumak görünüm hale getirir.

Bu ayarlar ayrıca kodda ayarlayabilirsiniz:

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


Otomatik boyutlandırma ayarlarını test etmek için farklı etkinleştirmek **desteklenen cihaz yönler** projenin seçenekleri:

 [![](layout-options-images/image43a.png "Otomatik boyutlandırma ayarları")](layout-options-images/image43a.png#lightbox)

Arkasındaki kodda yatay olarak yeniden boyutlandırmak iki metin denetimleri neden olan aşağıdaki kodu kullanabilirsiniz:

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


Biz Tasarımcısı'nı kullanarak denetimler de ayarlayabilirsiniz. Struts aşağıda sergilenen olarak belirlenmesi görünümü alt kırpılmış olmadan sağa hizalı kalmak görüntü neden olur:

 [![](layout-options-images/autoresize.png "A")](layout-options-images/autoresize.png#lightbox)

Bu ekran görüntüleri nasıl denetimleri yeniden boyutlandırma veya ekran döndürüldüğünde kendilerini yeniden konumlandırmak göster:

 [![](layout-options-images/image44a.png "A")](layout-options-images/image44a.png#lightbox)

Metin görünümü ve metin alanı hem aynı sol tutmak için uzatma ve sağ kenar boşlukları nedeniyle dikkat edin `FlexibleWidth` ayarı. Görüntü alt ve sağ kenar boşluklarının – ekran döndürüldüğünde görüntü görünümünde tutma korur anlamı üst ve sol kenar boşluğu esnek, sahiptir. Karmaşık düzenleri genellikle bir kullanıcı arabirimi tutarlı tutmak için ve görünümün sınırları (döndürme veya başka bir yeniden boyutlandırma olayı nedeniyle) değiştirdiğinizde çakışan denetimlerini önlemek için bu ayarların her görünür denetimi belirtebilir.





## <a name="related-links"></a>İlgili bağlantılar

- [Denetimleri (örnek)](https://developer.xamarin.com/samples/Controls/)
