---
title: Abonelikler ve Xamarin.iOS içinde raporlama
description: Bu belge, yenileme olmayan abonelikler, ücretsiz abonelikler, otomatik yenilenebilir abonelikleri ve iTunes Bağlan bu öğeler ile ilgili rapor kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7e0873107a60b48e5ebfd8e159f3bf3b85d02867
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787037"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>Abonelikler ve Xamarin.iOS içinde raporlama

## <a name="about-non-renewing-subscriptions"></a>Yenileme dışı abonelikler hakkında

Abonelikleri olmayan yenileme (gezinti uygulaması bir haftanın erişimi) veya bir veri arşivine süre sınırlı erişim gibi zaman kısıtlaması hizmetiyle satış gösteren ürünleri için tasarlanmıştır.   
   
Yenileme olmayan abonelikleri ve diğer ürün türleri arasındaki farklar'yi tıklatın:

-  İTunes Bağlan ürün tanımında terimi içermez. Uygulama kodu ürün kimliği geçerlilik döneminden Infer mümkün olması gerekir 
-  Birden çok kez (bir tüketilebilir ürün gibi) alınabilir. Uygulamaları abonelik terim/süre sonu ve yenileme yönetmek ve çakışan bir abonelik satın alma kullanıcının engellemek için gereklidir. 
-  Satın alma işlemleri StoreKit geri işlevi tarafından desteklenmez. Abonelik bir kullanıcının tüm aygıtlarda kullanılabilir olması gerekiyorsa, uygulama tasarlama ve uzak bir sunucu ile birlikte bu özelliği uygulamanız gerekir. Uygulamaları yedekleme abonelik durumunu durumlarda bir aygıt yedeklenen olduğunda büyütün sorumlu de sonra geri gelen-yedekleme. 
-  Uygulama genel bakış
-  Abonelikleri olmayan yenileme normalde sunucu teslim iş akışını kullanarak ve tüketilebilir ürünler gibi yönetilen uygulanmalıdır. 


## <a name="about-free-subscriptions"></a>Ücretsiz abonelikleri hakkında

Ücretsiz abonelikler boş içerik (bunlar Newsstand olmayan uygulamalarda kullanılamaz) Newsstand uygulamalardaki yerleştirme geliştiricilerin olanak sağlar. Ücretsiz bir abonelik başladıktan sonra kullanıcının tüm cihazlarda kullanılabilir olacaktır. Ücretsiz abonelik süresi dolmayacak; Bunlar, yalnızca uygulama kaldırıldığında sonlandırın.

### <a name="implementation-overview"></a>Uygulama genel bakış

Ücretsiz abonelikler çok otomatik yenilenebilir abonelikleri gibi davranır. Uygulama iTunes Bağlan 'satın almak için' uygun bir ücretsiz abonelik ürün olmalıdır. Kullanıcı tarafından satın aldığınızda, ücretsiz aboneliği satın alma gibi otomatik yenilenebilir abonelik ürün doğrulanmalıdır. Ücretsiz abonelik işlemleri geri yüklenebilir.


## <a name="about-auto-renewable-subscriptions"></a>Otomatik yenilenebilir abonelikleri hakkında

Otomatik yenilenebilir abonelikleri çoğunlukla Newsstand uygulamalarda kullanılır. Bunlar, iTunes (7 günden 1 yıla kadar uzanan dönemlerde ayarlanır) Bağlan yapılandırıldığı zaman, belirli bir süre için dinamik içeriğe kullanıcı erişimi veren bir ürün temsil eder. Abonelikler, kullanıcı çevrilir genişletme sürece her abonelik döneminin sonunda kullanıcının Apple kimliği şarj otomatik olarak yenileyin. Bu ürün türü de dergi veya haber abonelikleri için burada kullanıcı aboneliğini geçerli olduğu halde yayımlanan her sorun erişim alır çalışır.

### <a name="implementation-overview"></a>Uygulama genel bakış

Otomatik yenilenebilir abonelikleri Server-Delivered ürünleri iş akışı kullanılarak gerçekleştirilen (başvurmak *giriş doğrulama ve Server-Delivered ürünleri* bölümü).

#### <a name="shared-secret"></a>Paylaşılan gizlilik

Uygulama içi satın alma paylaşılan gizlilik, sunucunuzda otomatik yenilenebilir abonelikleri doğrulanırken JSON istek kullanılmalıdır. Paylaşılan gizliliği Bağlan iTunes oluşturulan/erişilir.

Connect giriş sayfası seçin iTunes **My uygulamaları**:   
   
 [![](subscriptions-and-reporting-images/image2.png "Uygulamalarım seçin")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
Bir uygulama seçin ve tıklayın **uygulama içi satın almalara** sekmesi:

[![](subscriptions-and-reporting-images/image6.png "Uygulama içi satın almalara sekmesini tıklatın")](subscriptions-and-reporting-images/image6.png#lightbox)

Sayfanın alt kısmından seçin **görünüm veya bir paylaşılan gizlilik oluşturmak**:
   
 [![](subscriptions-and-reporting-images/image40.png "Görünüm seçin veya bir paylaşılan gizlilik oluştur")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "Paylaşılan gizlilik oluştur")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 Paylaşılan gizliliği kullanmak için Apple'nın sunucularına şöyle otomatik yenilenebilir bir abonelik için bir uygulama içi satın alma bilgisi doğrularken gönderilen JSON yükündeki ekleyin:

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

Satın alma diğer ürün türleriyle olarak geçerliyse, yanıtın durum alanı sıfır olur.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>Öğeler sonra ilk abonelik süresi yükleniyor

Abonelik ürünleri sunan bir parçası olarak kod sık Apple'nın sunucularda bilinen en son giriş doğrulamanız gerekir. Bir abonelik otomatik-bu yana son doğrulama yenilendi, JSON yanıt oluştu işlem uygulamayı bilgilendirecek ek alanlar içermez (genişlettiğiniz abonelikleri geçerlilik). JSON yanıtı içerir:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

Durum sıfırsa abonelik hala geçerli olduğundan ve diğer alanları geçerli veriler basılı tutun. 21006 durumundaysa abonelik süresi doldu. Bkz: [otomatik yenilenebilir bir abonelik girişi doğrulama](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) diğer hata kodları için belgeleri.

#### <a name="restoring-auto-renewable-subscriptions"></a>Otomatik yenilenebilir abonelikleri geri yükleme

Birden çok işlem – orijinal satın alma işlemi artı her abonelik yenilendi süreyi için ayrı bir işlemde geri alırsınız. Geçerlilik süresi nedir anlamak için başlangıç tarihlerini ve koşulları izlemek gerekir.   
   
   
   
 SKPaymentTransaction nesne abonelik süresi dahil değildir – her dönem için farklı bir ürün kimlik kullanın ve satın alma tarihinden abonelik süresi hareketin tahmin kod yazmanız.

#### <a name="testing-auto-renewal"></a>Otomatik yenileme test etme

Abonelikleri test kolaylaştırmak için korumalı alan sınarken süreler sıkıştırılır. 1 hafta abonelikleri 3 dakikada bir, 1 yıl saatte abonelikleri yenilemek yenileyin. Abonelikler otomatik korumalı alanda test ederken 6 kereye maksimum yenilemeyi.

## <a name="reporting"></a>Raporlama

iTunes Bağlan ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) sağlar:   
   
 **Satış ve eğilimleri** – uygulama ayrıntılarını görüntüler yükler, güncelleştirmeleri ve uygulama içi satın almalara.   
   
 **Ödemeler ve finansal raporlar** – siz ve ne kadar ödenmesi gereken için yapılan listeleme ödemeler yanı sıra, uygulamalar tarafından kazanılan geliri ayrıntıları.

Satış ve Eğilimleri raporu örneği aşağıda verilmiştir:   

 [![](subscriptions-and-reporting-images/image42.png "Bir örnek satış ve Eğilimleri raporu")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 Ayrıca bir [ **ITC bağlanmak mobil**iOS uygulaması (iTunes bağlantı)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8).
iPhone ekran görüntüleri kullanılabilir istatistikleri bazıları aşağıda gösterilmektedir:   
   
 [![](subscriptions-and-reporting-images/image43.png "kullanılabilir istatistikleri bazıları iPhone ekran görüntüleri")](subscriptions-and-reporting-images/image43.png#lightbox)
