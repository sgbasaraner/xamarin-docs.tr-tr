---
title: Tüketilemezi ürünleri satın alma
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 0a581dc222e43f8d4742bd52dc56dc691449a8f2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="purchasing-non-consumable-products"></a>Tüketilemezi ürünleri satın alma

Olmayan tüketilebilir ürünleri 'müşteri tarafından sahip olunan'. Bunlar her zaman, kayıp/çalındığında cihazlarını veya yeni bir tane satın olsa bile erişmesini beklenir. Bunlar defterleri, dergi sorunları, oyun düzeyleri, fotoğraf filtreleri, 'pro-Özellikleri', vb. için kullanışlıdır. Bir kullanıcı bir Tüketilemezi ürün satın alındıktan sonra hiçbir zaman için yeniden ödenecek sahiptirler. Kodunuzu yanlışlıkla denemelerini izin veriyorsa, StoreKit zaten satın alınan bir ileti gösterilir.

## <a name="non-consumable-products-sample"></a>Tüketilemezi ürünleri örnek

[InAppPurchaseSample kod](https://developer.xamarin.com/samples/monotouch/StoreKit/) adlı bir proje içeren *NonConsumables*. Kod örneği, örnek olarak fotoğraf filtreleri kullanarak Tüketilemezi ürünleri uygulamak gösterilmiştir. Bir filtre satın aldığınız sonra fotoğrafı tekrar tekrar uygulayabilirsiniz. Hiçbir zaman yeniden satın almanız gerekir.   
   
   
   
 Satın alma işlemini ekran görüntüleri – bu dizide gösterilen **satın** düğmesi özellik etkinleştirme düğmesi hale gelir:   
   
   
   
 [![](purchasing-non-consumable-products-images/image34.png "Satın alma işlemini bu ekran görüntüleri dizide gösterilir")](purchasing-non-consumable-products-images/image34.png#lightbox)   
   
   
   
 Satın alma işlemi tüketilebilir ürün ile aynıdır; en önemli fark, satın alma uygulama kodunda nasıl izleneceğini içinde ' dir. Satınalma düğmesi yalnızca bu örnekte ürünü zaten satın değil, kullanılabilir aksi düğmesini özelliğini etkinleştirir.   
   
   
   

Aşağıdaki diyagramda, sınıflar ve Tüketilemezi ürün satın alma gerçekleştirmek için App Store sunucu arasındaki etkileşimler gösterilmektedir:   
   
   
   
 [![](purchasing-non-consumable-products-images/image35.png "Sınıfları ve Tüketilemezi ürün gerçekleştirmek için App Store sunucusu arasındaki etkileşimler satın alma")](purchasing-non-consumable-products-images/image35.png#lightbox)   
   
   
   
 Anahtar tüketilebilir örneğindeki satın alma tamamlandıktan sonra kullanıcı arabirimi yeniden satın önlemek için güncelleştirilmiş farktır. Bu örnekte, başarılı bir işlem bildirim kullanıcı arabirimi güncelleştirmeleri böylece **satın** düğmesi özelliğini etkinleştirir düğmesinde dönüştürülür.

## <a name="re-purchasing-non-consumable-products"></a>Tüketilemezi ürünleri yeniden satın alma

Kodunuzu normalde Gizle veya ürün başarıyla, kullanıcının yeniden ürünü satın almak denemelerini önlemek için satın alınan bir kez bir satın alma düğmesi yeniden amaçlandırın gerekir. Örnek uygulama değiştirerek bunu yapar **satın** iş örnek Fotoğraf Filtresi yapar düğmesi düğmesinde.   
   
   
   
 Burada bir uygulama Tüketilemezi ürün önceden satın olup olmadığını bildiremez durumlar vardır:

-  Bir uygulama ise ve bir cihazda yeniden yüklü tüm satın alma kayıtları kaldırılmıştır olacaktır (sürece / kullanıcı bir yedeklemeyi geri yükleme mu kadar). 
-  Kullanıcının varsa uygulama iki (veya daha fazla) aygıtlarda yüklü ve aygıtlardan biri daha sonra satın almasını sağlar. Diğer aygıtları satın almak için uygun ürün göstermeye devam eder. 
-  Bir müşteri bu gibi durumlarda bir Tüketilemezi ürünü yeniden satın almaya çalışırsa, uygulama mağazası yeniden ücret olmadan ürün karşılar. Kullanıcı arabirimi bir satın alma gerçekleştirmek için başlangıçta görünür (örneğin, bir onay uyarısı görüntülenir ve Apple kimliği gerekli olacaktır) ancak kullanıcı bunları ürün önceden satın olduğunu bildiren bir ileti sonra görürsünüz.  
   
   
   
 Bu senaryoda kod yolu tam normal bir satın alma aynı olduğundan, yalnızca farklılıklar şunlardır:

-  Kullanıcı yeniden ürün için sizden ücret değil.
-  `SKPaymentTransaction` Uygulamaya geçirilen nesnesi sahip olacak bir `OriginalTransaction` ürünü başlangıçta satın aldığınızda, oluşturulan işlem başvuran özelliği. 
-  Olmayan tüketilebilir ürünler satmak uygulamaları StoreKit'ın da uygulanmalı **geri** varolan satın alma işlemleri almalarına yardımcı olmak için özellik. 
