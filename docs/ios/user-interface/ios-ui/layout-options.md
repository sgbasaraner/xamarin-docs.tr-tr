---
title: "Düzen Seçenekleri"
ms.topic: article
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1e3139eb4c94264c91307f6f8a69b183f3bf7fa6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="layout-options"></a>Düzen Seçenekleri

Bir görünümü yeniden boyutlandırılmış veya Döndürülmüş düzenini denetlemek için iki farklı mekanizmalar vardır:

-  **Otomatik boyutlandırma** – Tasarımcısı'nda Otomatik boyutlandırma denetçisi ayarlamak için bir yöntem sunar `AutoresizingMask` özellikleri. Bu, kullanıcıların kapsayıcı kenarlarına bağlantılı ve/veya bunların boyutu düzeltme denetim sağlayacaktır. Otomatik boyutlandırma iOS tüm sürümlerinde çalışır. Bu aşağıdaki daha ayrıntılı olarak açıklanır
-  **Otomatik Düzen** – UI denetimlerinin ilişkileri üzerinde ayrıntılı denetim sağlar iOS6 sunulan bir özellik. Öğe diğer öğeleri göreli konumlarını Denetim tasarım yüzeyine izin verir. Bu konuda daha ayrıntılı olarak ele alınmıştır [Xamarin iOS Tasarımcısı otomatik düzeniyle](~/ios/user-interface/designer/designer-auto-layout.md) Kılavuzu.


## <a name="autosizing"></a>Otomatik boyutlandırma

Bir kullanıcı aygıtı ne zaman döndürülür ve yönlendirme değişiklikleri gibi bir pencere yeniden boyutlandırır olduğunda sistem kendi otomatik boyutlandırma kurallarına göre bu pencereyi içindeki görünümler otomatik olarak yeniden boyutlandırılır. Bu kurallar ayarlanabilir C# kullanarak `AutoresizingMask` özelliği `UIView` veya **özellikleri paneli** Tasarımcısı aşağıda gösterildiği gibi iOS:

 [ ![](layout-options-images/image41.png "Visual Studio için Mac Tasarımcısı")](layout-options-images/image41.png)

Bir Denetim seçildiğinde, bu el ile denetim boyutları ve konumunu belirtmenize olanak tanır seçme yanı sıra **otomatik boyutlandırma** davranışı. Aşağıdaki ekran görüntüsünde gösterildiği gibi biz struts ve yaylar otomatik boyutlandırma denetiminde kaydının üst öğeye seçili görünümün ilişki tanımlamak için kullanabilirsiniz:

 [ ![](layout-options-images/image42.png "Visual Studio için Mac Tasarımcısı")](layout-options-images/image42.png)

Ayarlama bir *yay* yeniden boyutlandırmak görünüm genişliği veya yüksekliği kendi üst görünümü göre neden olur. Ayarlama bir *strut* kendisi ve bu belirli Kenar çubuğunda kendi üst görünümü arasındaki sabit uzaklığı korumak görünüm hale getirir.

Bu ayarlar ayrıca kodda ayarlayabilirsiniz:

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


Otomatik boyutlandırma ayarlarını test etmek için farklı etkinleştirmek **desteklenen cihaz yönler** projenin seçenekleri:

 [ ![](layout-options-images/image43a.png "Otomatik boyutlandırma ayarları")](layout-options-images/image43a.png)

Arkasındaki kodda yatay olarak yeniden boyutlandırmak iki metin denetimleri neden olan aşağıdaki kodu kullanabilirsiniz:

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


Biz Tasarımcısı'nı kullanarak denetimler de ayarlayabilirsiniz. Struts aşağıda sergilenen olarak belirlenmesi görünümü alt kırpılmış olmadan sağa hizalı kalmak görüntü neden olur:

 [ ![](layout-options-images/autoresize.png "A")](layout-options-images/autoresize.png)

Bu ekran görüntüleri nasıl denetimleri yeniden boyutlandırma veya ekran döndürüldüğünde kendilerini yeniden konumlandırmak göster:

 [ ![](layout-options-images/image44a.png "A")](layout-options-images/image44a.png)

Metin görünümü ve metin alanı hem aynı sol tutmak için uzatma ve sağ kenar boşlukları nedeniyle dikkat edin `FlexibleWidth` ayarı. Görüntü alt ve sağ kenar boşluklarının – ekran döndürüldüğünde görüntü görünümünde tutma korur anlamı üst ve sol kenar boşluğu esnek, sahiptir. Karmaşık düzenleri genellikle bir kullanıcı arabirimi tutarlı tutmak için ve görünümün sınırları (döndürme veya başka bir yeniden boyutlandırma olayı nedeniyle) değiştirdiğinizde çakışan denetimlerini önlemek için bu ayarların her görünür denetimi belirtebilir.





## <a name="related-links"></a>İlgili bağlantılar

- [Denetimleri (örnek)](https://developer.xamarin.com/samples/Controls/)
